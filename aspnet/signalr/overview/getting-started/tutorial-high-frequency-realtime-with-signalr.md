---
uid: signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
title: 'Tutorial: Von Echtzeitnachrichten mit SignalR 2 | Microsoft-Dokumentation'
author: pfletcher
description: Dieses Tutorial veranschaulicht, wie eine Webanwendung erstellen, die ASP.NET SignalR verwendet wird, um häufige-messaging-Funktionalität bereitzustellen. Häufige Messagings in...
ms.author: riande
ms.date: 06/10/2014
ms.assetid: 9f969dda-78ea-4329-b1e3-e51c02210a2b
msc.legacyurl: /signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: e710980cecf9093ea9046b5790379befb5b61841
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836217"
---
<a name="tutorial-high-frequency-realtime-with-signalr-2"></a>Tutorial: Von Echtzeitnachrichten mit SignalR 2
====================
durch [Patrick Fletcher](https://github.com/pfletcher)

[Abgeschlossenes Projekt herunterladen](http://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a)

> Dieses Tutorial veranschaulicht, wie eine Webanwendung erstellen, die ASP.NET SignalR 2 verwendet wird, um häufige-messaging-Funktionalität bereitzustellen. Häufige messaging bedeutet in diesem Fall die Updates, die mit einer festen Rate gesendet werden; Bei dieser Anwendung bis zu 10 Nachrichten pro Sekunde.
> 
> Die Anwendung, die Sie in diesem Tutorial erstellen, zeigt eine Form, die Benutzer ziehen können. Die Position der Form in allen anderen verbundenen Browsern wird dann entsprechend die Position der gezogene Form mithilfe von zeitlich festgelegte Updates aktualisiert werden.
> 
> In diesem Tutorial eingeführte Konzepte müssen Anwendungen in Echtzeit Spiele und andere Anwendungen für die Simulation.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Softwareversionen, die in diesem Tutorial verwendet werden.
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR-Version 2
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a>In diesem Tutorial mithilfe von Visual Studio 2012
> 
> 
> Um Visual Studio 2012 mit diesem Tutorial verwenden möchten, führen Sie folgende Schritte aus:
> 
> - Aktualisieren Ihrer [-Paket-Manager](http://docs.nuget.org/docs/start-here/installing-nuget) auf die neueste Version.
> - Installieren Sie die [Webplattform-Installer](https://www.microsoft.com/web/downloads/platform.aspx).
> - Suchen Sie in den Webplattform-Installer, und installieren Sie **ASP.NET und Web Tools 2013.1 für Visual Studio 2012**. Dadurch wird Visual Studio-Vorlagen für SignalR-Klassen wie z. B. installiert **Hub**.
> - Einige Vorlagen (z. B. **OWIN-Startklasse**) nicht zur Verfügung; verwenden Sie für diese stattdessen eine Klassendatei.
> 
> 
> ## <a name="tutorial-versions"></a>Lernprogramm-Versionen
> 
> Weitere Informationen zu früheren Versionen von SignalR, finden Sie unter [ältere Versionen von SignalR](../older-versions/index.md).
> 
> ## <a name="questions-and-comments"></a>Fragen und Kommentare
> 
> Lassen Sie Feedback, auf wie Ihnen in diesem Tutorial gefallen hat und was wir in den Kommentaren am unteren Rand der Seite verbessern können. Wenn Sie Fragen, die nicht direkt mit dem Tutorial verknüpft sind haben, können Sie sie veröffentlichen das [ASP.NET SignalR-Forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oder [StackOverflow.com](http://stackoverflow.com/).


## <a name="overview"></a>Übersicht

In diesem Tutorial wird veranschaulicht, wie eine Anwendung erstellen, die den Zustand eines Objekts mit anderen Browsern in Echtzeit gemeinsam verwendet wird. Die Anwendung erstellten wird MoveShape aufgerufen. Die Seite "MoveShape" wird ein HTML-Div-Element anzeigen, die der Benutzer ziehen können; der Benutzer das DIV-Element zieht, wird seiner neue Position an den Server gesendet werden, die dann alle anderen verbundenen Clients, um die Position der Form entsprechend zu aktualisieren. darüber informiert.

![Das Anwendungsfenster](tutorial-high-frequency-realtime-with-signalr/_static/image1.png)

Die Anwendung, die in diesem Tutorial erstellten basiert auf einer Demo von Damian Edwards. Ein Video, enthält diese Demo sehen [hier](https://channel9.msdn.com/Series/Building-Web-Apps-with-ASP-NET-Jump-Start/Building-Web-Apps-with-ASPNET-Jump-Start-08-Real-time-Communication-with-SignalR).

Das Lernprogramm zunächst veranschaulicht, wie SignalR-Nachrichten von jedem Ereignis zu senden, die ausgelöst wird, wie die Form gezogen wird. Jede verbundene Clients klicken Sie dann aktualisiert die Position der lokalen Version von der Form jedes Mal, die eine Nachricht empfangen wird.

Während die Anwendung mit dieser Methode verwendet werden kann, ist dies keine empfohlene Programmiermodell, da gäbe es keine obere Grenze für die Anzahl der Nachrichten, Abrufen von gesendet werden, damit die Clients und Server mit Nachrichten überlastet zu erhalten konnte, und würde die Leistung beeinträchtigen . Die angezeigten Animation, auf dem Client wäre auch reibungslos auf jeder neuen Speicherort verschieben, statt separater, da die Form von jeder Methode, sofort verschoben werden sollen. Abschnitten des Lernprogramms zeige, wie zum Erstellen einer timerfunktion, die die maximale Rate einschränkt, mit der Nachrichten vom Client oder Server gesendet werden, und wie Sie die Form reibungslos zwischen Standorten verschieben. Die endgültige Version der in diesem Lernprogramm erstellten Anwendung kann heruntergeladen werden [Code Gallery](https://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a).

In diesem Tutorial enthält die folgenden Abschnitte:

- [Erforderliche Komponenten](#prerequisites)
- [Erstellen Sie des Projekts und fügen Sie des SignalR und JQuery.UI NuGet-Pakets hinzu](#createtheproject2013)
- [Erstellen Sie die grundlegende Anwendung](#baseapp)
- [Starten Sie den Hub, beim Starten der Anwendung](#startup2013)
- [Die Client-Schleife hinzufügen](#clientloop)
- [Der Server-Schleife hinzufügen](#serverloop)
- [Fügen Sie auf dem Client flüssige Animation hinzu](#animation)
- [Weitere Schritte](#furthersteps)

<a id="prerequisites"></a>

## <a name="prerequisites"></a>Erforderliche Komponenten

Dieses Lernprogramm erfordert Visual Studio 2013.

<a id="createtheproject2013"></a>

## <a name="create-the-project-and-add-the-signalr-and-jqueryui-nuget-package"></a>Erstellen Sie des Projekts und fügen Sie des SignalR und JQuery.UI NuGet-Pakets hinzu

In diesem Abschnitt erstellen wir das Projekt in Visual Studio 2013.

Verwenden Sie die folgenden Schritte aus Visual Studio 2013 erstellen eine leere ASP.NET-Webanwendung und Hinzufügen von die SignalR und jQuery.UI-Bibliotheken:

1. Erstellen Sie eine ASP.NET-Webanwendung in Visual Studio.

    ![Erstellen Sie web](tutorial-high-frequency-realtime-with-signalr/_static/image2.png)
2. In der **neues ASP.NET-Projekt** Fenster verlassen **leere** ausgewählt, und klicken Sie auf **Erstellen eines Projekts**.

    ![Leeres Web erstellen](tutorial-high-frequency-realtime-with-signalr/_static/image3.png)
3. In **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, wählen Sie **hinzufügen | SignalR-Hubklasse (v2)**. Nennen Sie die Klasse **MoveShapeHub.cs** und fügen sie dem Projekt hinzu. Dieser Schritt erstellt der **MoveShapeHub** -Klasse und fügt Sie dem Projekt einen Satz von Skriptdateien und Assemblyverweise, die SignalR unterstützen.

    > [!NOTE]
    > Sie können auch SignalR zu einem Projekt hinzufügen, indem Sie auf **Tools | Bibliothekspaket-Manager | Paket-Manager-Konsole** und einen Befehl ausführen:

    `install-package Microsoft.AspNet.SignalR` 

    Wenn Sie die Konsole verwenden, um SignalR hinzuzufügen, erstellen Sie die SignalR-hubklasse als separater Schritt nach dem Hinzufügen von SignalR.
4. Klicken Sie auf **Tools | Bibliothekspaket-Manager | Paket-Manager-Konsole**. Führen Sie im Fenster Paket-Manager den folgenden Befehl ein:

    `Install-Package jQuery.UI.Combined`

    Dadurch wird die jQuery-UI-Bibliothek, die Sie verwenden werden, um die Form zu animieren installiert.
5. In **Projektmappen-Explorer** erweitern Sie den Knoten des Skripts. Skriptbibliotheken für jQuery, jQueryUI und SignalR werden in das Projekt angezeigt.

    ![Verweisen auf](tutorial-high-frequency-realtime-with-signalr/_static/image4.png)

<a id="baseapp"></a>

## <a name="create-the-base-application"></a>Erstellen Sie die grundlegende Anwendung

In diesem Abschnitt erstellen wir eine Browser-Anwendung, die den Speicherort der Form an den Server, während jede MouseMove-Ereignis sendet. Der Server überträgt klicken Sie dann diese Informationen, um alle anderen verbundenen Clients, wie sie empfangen wird. Wir müssen für diese Anwendung in späteren Abschnitten erweitern.

1. Wenn Sie in der Klasse MoveShapeHub.cs erstellt haben **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie **hinzufügen**, **Klasse...** . Nennen Sie die Klasse **MoveShapeHub** , und klicken Sie auf **hinzufügen**.
2. Ersetzen Sie den Code in der neuen **MoveShapeHub** Klasse durch den folgenden Code.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample1.cs)]

    Die `MoveShapeHub` oben gezeigte Klasse ist eine Implementierung eines SignalR-Hubs. Wie in der [erste Schritte mit SignalR](tutorial-getting-started-with-signalr.md) Tutorial der Hub verfügt über eine Methode, die den Clients direkt aufruft. In diesem Fall sendet der Client ein Objekt mit den neuen X und Y-Koordinaten der Form auf dem Server, wodurch die haben ruft dann alle anderen verbundenen Clients übertragen. SignalR wird automatisch dieses Objekts unter Verwendung von JSON zu serialisieren.

    Das Objekt, das an den Client gesendet wird (`ShapeModel`) enthält Elemente, um die Position der Form zu speichern. Die Version des Objekts auf dem Server enthält auch ein Element zum Nachverfolgen der Clientdaten gespeichert wird, damit ein bestimmtes Clients nicht ihre eigenen Daten gesendet wird. Dieses Element verwendet die `JsonIgnore` Attribut, um zu verhindern serialisiert und an den Client gesendet.

<a id="startup2013"></a>
## <a name="starting-the-hub-when-the-application-starts"></a>Starten Sie den Hub, beim Starten der Anwendung

1. Als Nächstes müssen wir richten Zuordnung mit dem Hub beim Starten der Anwendung. In SignalR 2, dies erfolgt durch Hinzufügen einer OWIN-Startklasse wiederum `MapSignalR` bei der der Startup-Klasse `Configuration` Methode wird ausgeführt, wenn OWIN gestartet wird. Die Klasse wird hinzugefügt, um OWINs-Startup Verarbeitung mit der `OwinStartup` Assembly-Attribut.

    In **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und klicken Sie auf **hinzufügen | OWIN-Startklasse**. Nennen Sie die Klasse *Start* , und klicken Sie auf **OK**.
2. Ändern Sie den Inhalt der "Startup.cs" wie folgt aus:

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample2.cs)]

<a id="client"></a>
## <a name="adding-the-client"></a>Hinzufügen des Clients

1. Als Nächstes fügen wir den Client. In **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und klicken Sie auf **hinzufügen | Neues Element**. In der **neues Element hinzufügen** wählen Sie im Dialogfeld **Html-Seite**. Nennen Sie die Seite **"default.HTML"** , und klicken Sie auf **hinzufügen**.
2. In **Projektmappen-Explorer**mit der rechten Maustaste auf die Seite, die Sie gerade erstellt haben, und klicken Sie auf **als Startseite festlegen**.
3. Ersetzen Sie den Standardcode in der HTML-Seite mit den folgenden Codeausschnitt ein.

    > [!NOTE]
    > Stellen Sie sicher, dass das Skript unten Übereinstimmung, die Pakete dem Projekt im Ordner "Scripts" hinzugefügt verweist.

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample3.html)]

    Der obige HTML und JavaScript-Code erstellt ein Rot DIV-Element wird aufgerufen, Form, können Sie das Verhalten des Shapes ziehen mit der jQuery-Bibliothek und verwendet die Form vom Typ `drag` Ereignis, um die Position der Form an den Server gesendet.
4. Starten Sie die Anwendung durch Drücken von F5. Kopieren Sie die URL der Seite, und fügen Sie ihn in ein zweites Browserfenster. Ziehen Sie die Form eines Browserfenster; die Form im anderen Browserfenster sollten verschieben.

    ![Das Anwendungsfenster](tutorial-high-frequency-realtime-with-signalr/_static/image5.png)

<a id="clientloop"></a>

## <a name="add-the-client-loop"></a>Die Client-Schleife hinzufügen

Da ein ist ggf. unnötig Umfang des Netzwerkverkehrs, senden den Speicherort der Form auf jede MouseMove-Ereignis erstellt werden wird, müssen die Nachrichten vom Client gedrosselt werden. Wir verwenden den JavaScript-Code `setInterval` Funktion, um eine Schleife einrichten, die Informationen zur neuen Position an den Server mit einer festen Rate sendet. Diese Schleife ist eine einfache Darstellung einer "spielschleife" eine wiederholt aufgerufene Funktion, die die Funktionalität eines Spiels oder einer anderen Simulation steuert.

1. Aktualisieren Sie den Clientcode in der HTML-Seite entsprechend den folgenden Codeausschnitt ein.

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample4.html)]

    Die oben genannten Update fügt die `updateServerModel` -Funktion, die einer festen Häufigkeit aufgerufen wird. Diese Funktion sendet die Positionsdaten, an den Server nach der `moved` Flag gibt an, dass neue die zu sendenden Positionsdaten.
2. Starten Sie die Anwendung durch Drücken von F5. Kopieren Sie die URL der Seite, und fügen Sie ihn in ein zweites Browserfenster. Ziehen Sie die Form eines Browserfenster; die Form im anderen Browserfenster sollten verschieben. Da die Anzahl der Nachrichten, die an den Server gesendet werden können gedrosselt werden, wird die Animation nicht so reibungslos wie im vorherigen Abschnitt angezeigt.

    ![Das Anwendungsfenster](tutorial-high-frequency-realtime-with-signalr/_static/image6.png)

<a id="serverloop"></a>

## <a name="add-the-server-loop"></a>Der Server-Schleife hinzufügen

Die aktuelle Anwendung wechseln Sie in Nachrichten, die vom Server an den Client gesendet so oft, wie sie empfangen werden. Dies stellt ein ähnliches Problem wie auf dem Client aufgetreten ist; Nachrichten können als werden bei Bedarf, und die Verbindung kann daher überflutet werden häufig gesendet werden. Dieser Abschnitt beschreibt das Aktualisieren des Servers zum Implementieren eines Zeitgebers, das die Rate der ausgehenden Nachrichten drosselt.

1. Ersetzen Sie den Inhalt der `MoveShapeHub.cs` mit dem folgenden Codeausschnitt.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample5.cs)]

    Der obige Code dehnt der Client hinzufügen die `Broadcaster` -Klasse, die schränkt den ausgehenden Nachrichten anhand der `Timer` Klasse von .NET Framework.

    Da der Hub selbst vorübergehende ist (es wird erstellt jedes Mal, wenn er benötigt wird), die `Broadcaster` als Singleton erstellt werden. Verzögerter Initialisierung (eingeführt in .NET 4) wird zum seiner Erstellung zu verzögern, bis diese benötigt wird, um sicherzustellen, dass die erste hubinstanz vollständig erstellt wurde, bevor der Timer gestartet wird, verwendet.

    Der Aufruf des Clients `UpdateShape` Funktion wird dann aus des Hubs des verschoben `UpdateModel` Methode, sodass die It nicht mehr aufgerufen wird, sobald eingehende Nachrichten empfangen werden. Stattdessen werden die Nachrichten an die Clients gesendet werden, mit einer Geschwindigkeit von 25 Aufrufe pro Sekunde, von verwaltet die `_broadcastLoop` Zeitgeber innerhalb der `Broadcaster` Klasse.

    Abschließend anstelle der Methode aus dem Hub direkt die `Broadcaster` Klasse muss einen Verweis auf den aktuell ausgeführten Hub abrufen (`_hubContext`) mithilfe der `GlobalHost`.
2. Starten Sie die Anwendung durch Drücken von F5. Kopieren Sie die URL der Seite, und fügen Sie ihn in ein zweites Browserfenster. Ziehen Sie die Form eines Browserfenster; die Form im anderen Browserfenster sollten verschieben. Es werden nicht sichtbarer Unterschied im Browser aus dem vorherigen Abschnitt, aber die Anzahl der Nachrichten, die an den Client gesendet werden können, werden gedrosselt werden.

    ![Das Anwendungsfenster](tutorial-high-frequency-realtime-with-signalr/_static/image7.png)

<a id="animation"></a>

## <a name="add-smooth-animation-on-the-client"></a>Fügen Sie auf dem Client flüssige Animation hinzu

Die Anwendung ist fast abgeschlossen, aber stellen wir eine weitere Verbesserung, die während der Übertragung der Form auf dem Client während der Übertragung als Antwort auf die Server-Meldungen. Anstatt die Position der Form an den neuen Speicherort, der vom Server erhalten, verwenden wir der JQuery-UI-Bibliotheks `animate` Funktion, um die Form reibungslos zwischen der aktuellen und neuen Position zu verschieben.

1. Aktualisieren des Clients `updateShape` Methode, um zu sehen, wie den folgenden hervorgehobenen Code:

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample6.html?highlight=33-40)]

    Der obige Code verschiebt die Form am alten Speicherort in das neue Projekt, das vom Server im Verlauf der Animation-Intervall (in diesem Fall 100 Millisekunden) angegeben. Alle vorherige Animation, die unter der Form "wird gelöscht, bevor die neue Animation gestartet wird.
2. Starten Sie die Anwendung durch Drücken von F5. Kopieren Sie die URL der Seite, und fügen Sie ihn in ein zweites Browserfenster. Ziehen Sie die Form eines Browserfenster; die Form im anderen Browserfenster sollten verschieben. Die Verschiebung von der Form, in dem anderen Fenster sollte weniger ruckartige, wie ihre Bewegung auf einmal pro eingehender Nachricht festgelegt wird, statt Zeit interpoliert wird angezeigt.

    ![Das Anwendungsfenster](tutorial-high-frequency-realtime-with-signalr/_static/image8.png)

<a id="furthersteps"></a>

## <a name="further-steps"></a>Weitere Schritte

In diesem Tutorial haben Sie gelernt, wie Sie eine SignalR-Anwendung programmieren, die häufige Nachrichten zwischen Clients und Servern sendet. Dieses Paradigma für die Kommunikation ist nützlich für die Entwicklung von Onlinespielen und andere Simulationen, z. B. [ShootR Spiel erstellt, die mit SignalR](http://shootr.signalr.net).

Die vollständige Anwendung, die in diesem Tutorial erstellten kann heruntergeladen werden [Code Gallery](https://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a).

Weitere Informationen zu Konzepten der SignalR-Entwicklung finden Sie unter den folgenden Websites für SignalR-Quellcode und Ressourcen:

- [SignalR-Projekt](http://signalr.net)
- [SignalR Github und Beispiele](https://github.com/SignalR/SignalR)
- [SignalR-Wiki](https://github.com/SignalR/SignalR/wiki)

Eine exemplarische Vorgehensweise zum Bereitstellen einer SignalR-Anwendung in Azure finden Sie unter [mithilfe von SignalR mit Web-Apps in Azure App Service](../deployment/using-signalr-with-azure-web-sites.md). Ausführliche Informationen dazu, wie Sie ein Visual Studio-Webprojekt auf einer Windows Azure-Website bereitstellen, finden Sie unter [ASP.NET Web-app in Azure App Service erstellen](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).
