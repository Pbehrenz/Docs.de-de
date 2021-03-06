---
uid: mvc/overview/getting-started/database-first-development/setting-up-database
title: Erste Schritte mit Entity Framework 6 Database First anhand von MVC 5 | Microsoft-Dokumentation
author: tfitzmac
description: Verwenden MVC, Entity Framework und ASP.NET-Gerüstbau, können Sie eine Webanwendung erstellen, die eine Schnittstelle für eine vorhandene Datenbank bereitstellt. Dieses Tutorial Seri...
ms.author: riande
ms.date: 10/01/2014
ms.assetid: 095abad4-3bfe-4f06-b092-ae6a735b7e49
msc.legacyurl: /mvc/overview/getting-started/database-first-development/setting-up-database
msc.type: authoredcontent
ms.openlocfilehash: 44725b772922426b08c72de83d384c1e03e319cb
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833311"
---
<a name="getting-started-with-entity-framework-6-database-first-using-mvc-5"></a>Erste Schritte mit Entity Framework 6 Database First anhand von MVC 5
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> Verwenden MVC, Entity Framework und ASP.NET-Gerüstbau, können Sie eine Webanwendung erstellen, die eine Schnittstelle für eine vorhandene Datenbank bereitstellt. Dieser tutorialreihe erfahren Sie, wie Sie automatisch generierter Code, der ermöglicht Benutzern das anzeigen, bearbeiten, erstellen und Löschen von Daten, die in einer Datenbanktabelle gespeichert. Der generierte Code entspricht die Spalten in der Datenbanktabelle. Im letzten Teil der Reihe werden Sie den Standort und die Datenbank in Azure bereitstellen.
> 
> Dieser Teil der Serie konzentriert sich auf eine Datenbank erstellen und ihn mit Daten aufzufüllen.
> 
> Dieser Serie wurde mit Beiträgen Tom Dykstra und Rick Anderson geschrieben. Es wurde basierend auf dem Feedback von Benutzern im Abschnitt "Kommentare" verbessert.


## <a name="introduction"></a>Einführung

In diesem Thema wird gezeigt, wie für den Einstieg eine vorhandene Datenbank und erstellen Sie schnell eine Anwendung, die Benutzern ermöglicht, die mit den Daten interagieren. Er verwendet das Entity Framework 6 und MVC 5, um die Webanwendung zu erstellen. Die ASP.NET-Gerüstbau-Funktion ermöglicht Ihnen das automatische Generieren von Code zum Anzeigen, aktualisieren, erstellen und Löschen von Daten. Verwenden die Tools für die Veröffentlichung in Visual Studio, können Sie problemlos den Standort und die Datenbank in Azure bereitstellen.

In diesem Thema werden die Situation, in dem Sie eine Datenbank und zum Generieren von Code für eine Webanwendung, die auf Grundlage der Felder für diese Datenbank werden soll. Dieser Ansatz heißt Database First-Entwicklung. Wenn Sie nicht bereits über eine vorhandene Datenbank verfügen, können Sie stattdessen, eine Methode, die Code First-Entwicklung und dazu Datenklassen zu definieren und die Datenbank aus den Eigenschaften der Klasse aufgerufen.

Ein einführendes Beispiel Code First-Entwicklung, finden Sie unter [erste Schritte mit ASP.NET MVC 5](../introduction/getting-started.md). Ein komplexeres Beispiel, finden Sie unter [erstellen ein Entity Framework-Datenmodells für eine ASP.NET MVC 4-App](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

Hilfestellung bei der Auswahl von Entity Framework Ansatz verwenden, finden Sie unter [Entity Framework-Webentwicklungsansätzen](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).

## <a name="prerequisites"></a>Erforderliche Komponenten

Visual Studio 2013 oder Visual Studio Express 2013, für das Web

## <a name="set-up-the-database"></a>Richten Sie die Datenbank

Zum Imitieren der Umgebung von einer vorhandenen Datenbank müssen Sie zunächst eine Datenbank erstellen, mit einigen Daten vorab ausgefüllten, und klicken Sie dann Ihre Webanwendung die Verbindung mit der Datenbank erstellen.

In diesem Tutorial wurde die Verwendung von LocalDB mit Visual Studio 2013 oder Visual Studio Express 2013 für Web entwickelt. Sie können einen vorhandenen Datenbankserver verwenden, statt mit LocalDB, aber je nach Ihrer Version von Visual Studio und den Typ der Datenbank, aller Data Tools in Visual Studio möglicherweise nicht unterstützt. Wenn die Tools nicht für die Datenbank verfügbar sind, müssen Sie möglicherweise einige der Schritte in der Management Suite datenbankspezifischen für Ihre Datenbank ausführen.

Wenn Sie ein Problem mit der Datenbanktools in Ihrer Version von Visual Studio verfügen, stellen Sie sicher, dass Sie die neueste Version der Datenbanktools installiert haben. Informationen zu aktualisieren oder die Datenbanktools installieren, finden Sie unter [Microsoft SQL Server Data Tools](https://msdn.microsoft.com/data/hh297027).

Starten Sie Visual Studio, und erstellen Sie eine **SQL Server-Datenbankprojekt**. Nennen Sie das Projekt **ContosoUniversityData**.

![Datenbankprojekt erstellen](setting-up-database/_static/image1.png)

Sie haben jetzt ein leeres Datenbankprojekt. Sie werden diese Datenbank in Azure weiter unten in diesem Tutorial bereitgestellt, daher Sie Azure SQL-Datenbank als Zielplattform für das Projekt festlegen müssen. Festlegen der Zielplattform ist die Datenbank keine tatsächliche Bereitstellung; Es bedeutet lediglich, dass das Datenbankprojekt sicherstellen, dass der Entwurf der Datenbank mit der Zielplattform kompatibel ist. Öffnen Sie zum Festlegen der Zielplattform der **Eigenschaften** für das Projekt und wählen Sie **Microsoft Azure SQL-Datenbank** für die Zielplattform.

![Set-Zielplattform](setting-up-database/_static/image2.png)

Sie können die Tabellen, die für dieses Tutorial erforderlich sind, durch das Hinzufügen von SQL-Skripts, die definieren, die Tabellen erstellen. Mit der rechten Maustaste in des Projekts, und fügen Sie ein neues Element hinzu.

![Neues Element hinzufügen](setting-up-database/_static/image3.png)

Fügen Sie eine neue Tabelle namens "Student" hinzu.

![Hinzufügen von Tabelle "Student"](setting-up-database/_static/image4.png)

Ersetzen Sie in der Datei die T-SQL-Befehl mit dem folgenden Code zum Erstellen der Tabelle ein.

[!code-sql[Main](setting-up-database/samples/sample1.sql)]

Beachten Sie, dass das Entwurfsfenster automatisch mit dem Code synchronisiert wird. Sie können mit dem Code oder Designer arbeiten.

![Anzeigen von Code und Entwurf](setting-up-database/_static/image5.png)

Fügen Sie einer anderen Tabelle hinzu. Die Zeit, nennen Sie sie Kurs und verwenden Sie den folgenden T-SQL-Befehl.

[!code-sql[Main](setting-up-database/samples/sample2.sql)]

Und noch einmal zum Erstellen einer Tabelle mit dem Namen Registrierung zu wiederholen.

[!code-sql[Main](setting-up-database/samples/sample3.sql)]

Sie können Ihre Datenbank mit Daten über ein Skript auffüllen, die ausgeführt wird, nachdem die Datenbank bereitgestellt wird. Fügen Sie ein Skript nach der Bereitstellung zum Projekt hinzu. Sie können den Standardnamen verwenden.

![Skript nach der Bereitstellung hinzufügen](setting-up-database/_static/image6.png)

Fügen Sie den folgenden T-SQL-Code, um das Skript nach der Bereitstellung. Dieses Skript fügt Daten einfach mit der Datenbank, wenn kein übereinstimmender Datensatz gefunden wird. Nicht überschreiben oder löschen alle Daten, die Sie in die Datenbank eingegeben haben.

[!code-sql[Main](setting-up-database/samples/sample4.sql)]

Es ist wichtig zu beachten, dass das Skript nach der Bereitstellung ausgeführt wird, jedes Mal, wenn Sie das Datenbankprojekt bereitstellen. Aus diesem Grund müssen Sie Ihren Anforderungen sorgfältig zu überlegen, wenn Sie dieses Skript zu schreiben. In einigen Fällen möchten Sie möglicherweise über Starten von einem bekannten Satz von Daten jedes Mal, wenn das Projekt bereitgestellt wird. In anderen Fällen können Sie nicht die vorhandenen Daten in keiner Weise ändern möchten. Je nach Ihren Anforderungen, können Sie entscheiden, ob Sie benötigen ein Skript nach der Bereitstellung oder in das Skript eingefügt werden sollen. Weitere Informationen zum Auffüllen Ihrer Datenbank mit einem Skript nach der Bereitstellung finden Sie unter [einschließlich der Daten in einem SQL Server-Datenbankprojekt](https://blogs.msdn.com/b/ssdt/archive/2012/02/02/including-data-in-an-sql-server-database-project.aspx).

Sie verfügen jetzt über 4 SQL-Skriptdateien, aber keine tatsächlichen Tabellen. Sie sind bereit für die Bereitstellung des Datenbankprojekts in Localdb. Klicken Sie in Visual Studio auf die Schaltfläche "Start" (oder F5) zum Erstellen und Bereitstellen des Datenbankprojekts. Überprüfen Sie die Registerkarte "Datenausgabe", um sicherzustellen, dass die Erstellung und Bereitstellung war erfolgreich.

![Ausgabe anzeigen](setting-up-database/_static/image7.png)

Um anzuzeigen, dass die neue Datenbank erstellt wurde, öffnen **Objekt-Explorer von SQL Server** und suchen Sie nach den Namen des Projekts in der richtigen lokalen Datenbankserver (in diesem Fall **(Localdb) \ProjectsV12**).

![Anzeigen der neuen Datenbank](setting-up-database/_static/image8.png)

Um anzuzeigen, dass die Tabellen mit Daten aufgefüllt werden, mit der rechten Maustaste in einer Tabelle aus, und wählen **Ansichtsdaten**.

![Anzeigen von Tabellendaten](setting-up-database/_static/image9.png)

Eine bearbeitbare Ansicht der Tabellendaten wird angezeigt.

![Table-Data-Ergebnisse anzeigen](setting-up-database/_static/image10.png)

Ihre Datenbank weist nun eingerichtet und mit Daten aufgefüllt. Im nächsten Tutorial erstellen Sie eine Webanwendung für die Datenbank.

> [!div class="step-by-step"]
> [Nächste](creating-the-web-application.md)
