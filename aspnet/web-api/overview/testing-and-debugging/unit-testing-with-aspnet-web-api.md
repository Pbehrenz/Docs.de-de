---
uid: web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
title: "Komponententests für ASP.NET Web API 2 | Microsoft Docs"
author: tfitzmac
description: "Diese Anleitung und die Anwendung wird gezeigt, wie einfache Komponententests für Ihre Web-API 2-Anwendung erstellen. In diesem Lernprogramm wird gezeigt, wie eine Einheit Test Proj eingeschlossen wird..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/05/2014
ms.topic: article
ms.assetid: bf20f78d-ff91-48be-abd1-88e23dcc70ba
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 13211ee4543e17a4bfb2f83495f4041880f37df2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="unit-testing-aspnet-web-api-2"></a><span data-ttu-id="74c96-104">Komponententests für ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="74c96-104">Unit Testing ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="74c96-105">durch [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="74c96-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[<span data-ttu-id="74c96-106">Herunterladen des abgeschlossenen Projekts</span><span class="sxs-lookup"><span data-stu-id="74c96-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-e2867d4d)

> <span data-ttu-id="74c96-107">Diese Anleitung und die Anwendung wird gezeigt, wie einfache Komponententests für Ihre Web-API 2-Anwendung erstellen.</span><span class="sxs-lookup"><span data-stu-id="74c96-107">This guidance and application demonstrate how to create simple unit tests for your Web API 2 application.</span></span> <span data-ttu-id="74c96-108">In diesem Lernprogramm wird gezeigt, wie ein Komponententestprojekt in der Projektmappe, und Schreiben Testmethoden, mit die die zurückgegebenen Werte aus einem Controllermethode überprüft wird.</span><span class="sxs-lookup"><span data-stu-id="74c96-108">This tutorial shows how to include a unit test project in your solution, and write test methods that check the returned values from a controller method.</span></span>
> 
> <span data-ttu-id="74c96-109">In diesem Lernprogramm wird davon ausgegangen, dass Sie mit den grundlegenden Konzepten der ASP.NET Web-API vertraut sind.</span><span class="sxs-lookup"><span data-stu-id="74c96-109">This tutorial assumes you are familiar with the basic concepts of ASP.NET Web API.</span></span> <span data-ttu-id="74c96-110">Ein einführendes Lernprogramm finden Sie unter [erste Schritte mit ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="74c96-110">For an introductory tutorial, see [Getting Started with ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span></span>
> 
> <span data-ttu-id="74c96-111">Die Komponententests in diesem Thema sind einfache Datenszenarien absichtlich beschränkt.</span><span class="sxs-lookup"><span data-stu-id="74c96-111">The unit tests in this topic are intentionally limited to simple data scenarios.</span></span> <span data-ttu-id="74c96-112">Komponententests, die erweiterte Datenszenarien, finden Sie unter [imitieren Entity Framework beim Einheit Testen von ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).</span><span class="sxs-lookup"><span data-stu-id="74c96-112">For unit testing more advanced data scenarios, see [Mocking Entity Framework when Unit Testing ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="74c96-113">In diesem Lernprogramm verwendeten Versionen der Software</span><span class="sxs-lookup"><span data-stu-id="74c96-113">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="74c96-114">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="74c96-114">Visual Studio 2017</span></span>](https://www.visualstudio.com/vs/)
> - <span data-ttu-id="74c96-115">Web-API 2</span><span class="sxs-lookup"><span data-stu-id="74c96-115">Web API 2</span></span>


## <a name="in-this-topic"></a><span data-ttu-id="74c96-116">In diesem Thema</span><span class="sxs-lookup"><span data-stu-id="74c96-116">In this topic</span></span>

<span data-ttu-id="74c96-117">Dieses Thema enthält folgende Abschnitte:</span><span class="sxs-lookup"><span data-stu-id="74c96-117">This topic contains the following sections:</span></span>

- [<span data-ttu-id="74c96-118">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="74c96-118">Prerequisites</span></span>](#prereqs)
- [<span data-ttu-id="74c96-119">Herunterladen von code</span><span class="sxs-lookup"><span data-stu-id="74c96-119">Download code</span></span>](#download)
- [<span data-ttu-id="74c96-120">Anwendung mit Komponententestprojekt erstellen</span><span class="sxs-lookup"><span data-stu-id="74c96-120">Create application with unit test project</span></span>](#appwithunittest)

    - [<span data-ttu-id="74c96-121">Fügen Sie beim Erstellen der Anwendung Komponententestprojekt hinzu</span><span class="sxs-lookup"><span data-stu-id="74c96-121">Add unit test project when creating the application</span></span>](#whencreate)
    - [<span data-ttu-id="74c96-122">Komponententestprojekt zu einer vorhandenen Anwendung hinzufügen</span><span class="sxs-lookup"><span data-stu-id="74c96-122">Add unit test project to an existing application</span></span>](#addtoexisting)
- [<span data-ttu-id="74c96-123">Einrichten der Web-API 2-Anwendung</span><span class="sxs-lookup"><span data-stu-id="74c96-123">Set up the Web API 2 application</span></span>](#setupproject)
- [<span data-ttu-id="74c96-124">Installieren von NuGet-Pakete im Test-Projekt</span><span class="sxs-lookup"><span data-stu-id="74c96-124">Install NuGet packages in test project</span></span>](#testpackages)
- [<span data-ttu-id="74c96-125">Erstellen von tests</span><span class="sxs-lookup"><span data-stu-id="74c96-125">Create tests</span></span>](#tests)
- [<span data-ttu-id="74c96-126">Ausführen von tests</span><span class="sxs-lookup"><span data-stu-id="74c96-126">Run tests</span></span>](#runtests)

<a id="prereqs"></a>
## <a name="prerequisites"></a><span data-ttu-id="74c96-127">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="74c96-127">Prerequisites</span></span>

<span data-ttu-id="74c96-128">Visual Studio 2017 Community, Professional oder Enterprise edition</span><span class="sxs-lookup"><span data-stu-id="74c96-128">Visual Studio 2017 Community, Professional or Enterprise edition</span></span>

<a id="download"></a>
## <a name="download-code"></a><span data-ttu-id="74c96-129">Herunterladen von code</span><span class="sxs-lookup"><span data-stu-id="74c96-129">Download code</span></span>

<span data-ttu-id="74c96-130">Herunterladen der [abgeschlossenes Projekt](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11).</span><span class="sxs-lookup"><span data-stu-id="74c96-130">Download the [completed project](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11).</span></span> <span data-ttu-id="74c96-131">Herunterladbare Projekt umfasst Komponententestcode für dieses Thema und die [imitieren Entity Framework beim Einheit ASP.NET Web API zum Testen der](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md) Thema.</span><span class="sxs-lookup"><span data-stu-id="74c96-131">The downloadable project includes unit test code for this topic and for the [Mocking Entity Framework when Unit Testing ASP.NET Web API](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md) topic.</span></span>

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a><span data-ttu-id="74c96-132">Anwendung mit Komponententestprojekt erstellen</span><span class="sxs-lookup"><span data-stu-id="74c96-132">Create application with unit test project</span></span>

<span data-ttu-id="74c96-133">Sie können entweder ein Komponententestprojekt erstellen, wenn Ihre Anwendung erstellen oder hinzufügen ein Komponententestprojekts für eine vorhandene Anwendung.</span><span class="sxs-lookup"><span data-stu-id="74c96-133">You can either create a unit test project when creating your application or add a unit test project to an existing application.</span></span> <span data-ttu-id="74c96-134">Dieses Lernprogramm zeigt beide Methoden zum Erstellen eines Komponententestprojekts.</span><span class="sxs-lookup"><span data-stu-id="74c96-134">This tutorial shows both methods for creating a unit test project.</span></span> <span data-ttu-id="74c96-135">Um dieses Lernprogramm auszuführen, können Sie beide Vorgehensweisen verwenden.</span><span class="sxs-lookup"><span data-stu-id="74c96-135">To follow this tutorial, you can use either approach.</span></span>

<a id="whencreate"></a>
### <a name="add-unit-test-project-when-creating-the-application"></a><span data-ttu-id="74c96-136">Fügen Sie beim Erstellen der Anwendung Komponententestprojekt hinzu</span><span class="sxs-lookup"><span data-stu-id="74c96-136">Add unit test project when creating the application</span></span>

<span data-ttu-id="74c96-137">Erstellen einer neuen ASP.NET Web-Anwendung mit dem Namen **StoreApp**.</span><span class="sxs-lookup"><span data-stu-id="74c96-137">Create a new ASP.NET Web Application named **StoreApp**.</span></span>

![Projekt erstellen](unit-testing-with-aspnet-web-api/_static/image1.png)

<span data-ttu-id="74c96-139">Wählen Sie im Fenster Neues ASP.NET-Projekt den **leere** Vorlage und Hinzufügen von Ordnern und Verweise für die Web-API core.</span><span class="sxs-lookup"><span data-stu-id="74c96-139">In the New ASP.NET Project windows, select the **Empty** template and add folders and core references for Web API.</span></span> <span data-ttu-id="74c96-140">Wählen Sie die **Komponententests hinzufügen** Option.</span><span class="sxs-lookup"><span data-stu-id="74c96-140">Select the **Add unit tests** option.</span></span> <span data-ttu-id="74c96-141">Das Komponententestprojekt wird automatisch mit dem Namen **StoreApp.Tests**.</span><span class="sxs-lookup"><span data-stu-id="74c96-141">The unit test project is automatically named **StoreApp.Tests**.</span></span> <span data-ttu-id="74c96-142">Sie können diesen Namen beibehalten.</span><span class="sxs-lookup"><span data-stu-id="74c96-142">You can keep this name.</span></span>

![Komponententestprojekt erstellen](unit-testing-with-aspnet-web-api/_static/image2.png)

<span data-ttu-id="74c96-144">Nach dem Erstellen der Anwendung, sehen Sie sich, dass es zwei Projekte enthält.</span><span class="sxs-lookup"><span data-stu-id="74c96-144">After creating the application, you will see it contains two projects.</span></span>

![zwei Projekte](unit-testing-with-aspnet-web-api/_static/image3.png)

<a id="addtoexisting"></a>
### <a name="add-unit-test-project-to-an-existing-application"></a><span data-ttu-id="74c96-146">Komponententestprojekt zu einer vorhandenen Anwendung hinzufügen</span><span class="sxs-lookup"><span data-stu-id="74c96-146">Add unit test project to an existing application</span></span>

<span data-ttu-id="74c96-147">Wenn Sie nicht das Komponententestprojekt erstellt haben, wenn Sie Ihre Anwendung erstellt haben, können Sie eine zu einem beliebigen Zeitpunkt hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="74c96-147">If you did not create the unit test project when you created your application, you can add one at any time.</span></span> <span data-ttu-id="74c96-148">Nehmen wir beispielsweise an, dass Sie bereits über eine Anwendung mit dem Namen StoreApp verfügen und Komponententests hinzugefügt werden soll.</span><span class="sxs-lookup"><span data-stu-id="74c96-148">For example, suppose you already have an application named StoreApp, and you want to add unit tests.</span></span> <span data-ttu-id="74c96-149">Klicken Sie zum Hinzufügen eines Komponententestprojekts mit der rechten Maustaste in der Projektmappe, und wählen Sie **hinzufügen** und **neues Projekt**.</span><span class="sxs-lookup"><span data-stu-id="74c96-149">To add a unit test project, right-click your solution and select **Add** and **New Project**.</span></span>

![Neues Projekt zur Projektmappe hinzugefügt](unit-testing-with-aspnet-web-api/_static/image4.png)

<span data-ttu-id="74c96-151">Wählen Sie **Test** im linken Bereich, und wählen **Komponententestprojekt** für den Projekttyp.</span><span class="sxs-lookup"><span data-stu-id="74c96-151">Select **Test** in the left pane, and select **Unit Test Project** for the project type.</span></span> <span data-ttu-id="74c96-152">Nennen Sie das Projekt **StoreApp.Tests**.</span><span class="sxs-lookup"><span data-stu-id="74c96-152">Name the project **StoreApp.Tests**.</span></span>

![Fügen Sie Komponententestprojekt hinzu](unit-testing-with-aspnet-web-api/_static/image5.png)

<span data-ttu-id="74c96-154">Das Komponententestprojekt sehen Sie in der Projektmappe.</span><span class="sxs-lookup"><span data-stu-id="74c96-154">You will see the unit test project in your solution.</span></span>

<span data-ttu-id="74c96-155">Fügen Sie einen Projektverweis auf das ursprüngliche Projekt hinzu, in das Komponententestprojekt.</span><span class="sxs-lookup"><span data-stu-id="74c96-155">In the unit test project, add a project reference to the original project.</span></span>

<a id="setupproject"></a>
## <a name="set-up-the-web-api-2-application"></a><span data-ttu-id="74c96-156">Einrichten der Web-API 2-Anwendung</span><span class="sxs-lookup"><span data-stu-id="74c96-156">Set up the Web API 2 application</span></span>

<span data-ttu-id="74c96-157">Fügen Sie in Ihrem Projekt StoreApp eine Klassendatei, die **Modelle** Ordner mit dem Namen **Product.cs**.</span><span class="sxs-lookup"><span data-stu-id="74c96-157">In your StoreApp project, add a class file to the **Models** folder named **Product.cs**.</span></span> <span data-ttu-id="74c96-158">Ersetzen Sie den Inhalt der Datei durch den folgenden Code ein.</span><span class="sxs-lookup"><span data-stu-id="74c96-158">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="74c96-159">Erstellen Sie die Projektmappe.</span><span class="sxs-lookup"><span data-stu-id="74c96-159">Build the solution.</span></span>

<span data-ttu-id="74c96-160">Mit der rechten Maustaste in des Ordners Controller, und wählen Sie **hinzufügen** und **neues Gerüstelement**.</span><span class="sxs-lookup"><span data-stu-id="74c96-160">Right-click the Controllers folder and select **Add** and **New Scaffolded Item**.</span></span> <span data-ttu-id="74c96-161">Wählen Sie **Web-API 2-Controller: leere**.</span><span class="sxs-lookup"><span data-stu-id="74c96-161">Select **Web API 2 Controller - Empty**.</span></span>

![Hinzufügen eines neuen Controllers](unit-testing-with-aspnet-web-api/_static/image6.png)

<span data-ttu-id="74c96-163">Legen Sie den Namen des Controllers auf **SimpleProductController**, und klicken Sie auf **hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="74c96-163">Set the controller name to **SimpleProductController**, and click **Add**.</span></span>

![Geben Sie die controller](unit-testing-with-aspnet-web-api/_static/image7.png)

<span data-ttu-id="74c96-165">Ersetzen Sie den vorhandenen Code durch folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="74c96-165">Replace the existing code with the following code.</span></span> <span data-ttu-id="74c96-166">Zur Vereinfachung dieses Beispiels werden die Daten in einer Liste anstatt einer Datenbank gespeichert.</span><span class="sxs-lookup"><span data-stu-id="74c96-166">To simplify this example, the data is stored in a list rather than a database.</span></span> <span data-ttu-id="74c96-167">Die in dieser Klasse definierte Liste stellt die Produktionsdaten dar.</span><span class="sxs-lookup"><span data-stu-id="74c96-167">The list defined in this class represents the production data.</span></span> <span data-ttu-id="74c96-168">Beachten Sie, dass der Controller enthält einen Konstruktor, der als Parameter eine Liste von Product-Objekten akzeptiert.</span><span class="sxs-lookup"><span data-stu-id="74c96-168">Notice that the controller includes a constructor that takes as a parameter a list of Product objects.</span></span> <span data-ttu-id="74c96-169">Dieser Konstruktor können Sie Testdaten übergeben bei Komponententests bereit.</span><span class="sxs-lookup"><span data-stu-id="74c96-169">This constructor enables you to pass test data when unit testing.</span></span> <span data-ttu-id="74c96-170">Der Controller enthält auch zwei **Async** Methoden, um Komponententests für asynchrone Methoden zu veranschaulichen.</span><span class="sxs-lookup"><span data-stu-id="74c96-170">The controller also includes two **async** methods to illustrate unit testing asynchronous methods.</span></span> <span data-ttu-id="74c96-171">Diese asynchronen Methoden wurden durch den Aufruf implementiert **Task.FromResult** äußeren Code, aber normalerweise die Methoden minimieren würde ressourcenintensive Vorgänge enthalten.</span><span class="sxs-lookup"><span data-stu-id="74c96-171">These async methods were implemented by calling **Task.FromResult** to minimize extraneous code, but normally the methods would include resource-intensive operations.</span></span>

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="74c96-172">Gibt die GetProduct-Methode eine Instanz von der **IHttpActionResult** Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="74c96-172">The GetProduct method returns an instance of the **IHttpActionResult** interface.</span></span> <span data-ttu-id="74c96-173">IHttpActionResult ist eine der neuen Funktionen in Web-API 2 und Unit Testentwicklung vereinfacht.</span><span class="sxs-lookup"><span data-stu-id="74c96-173">IHttpActionResult is one of the new features in Web API 2, and it simplifies unit test development.</span></span> <span data-ttu-id="74c96-174">Klassen, die die IHttpActionResult-Schnittstelle implementieren befinden sich in der [System.Web.Http.Results](https://msdn.microsoft.com/en-us/library/system.web.http.results.aspx) Namespace.</span><span class="sxs-lookup"><span data-stu-id="74c96-174">Classes that implement the IHttpActionResult interface are found in the [System.Web.Http.Results](https://msdn.microsoft.com/en-us/library/system.web.http.results.aspx) namespace.</span></span> <span data-ttu-id="74c96-175">Diese Klassen stellen mögliche Antworten auf eine Aktion Anfrage, und sie HTTP-Statuscodes entsprechen.</span><span class="sxs-lookup"><span data-stu-id="74c96-175">These classes represent possible responses from an action request, and they correspond to HTTP status codes.</span></span>

<span data-ttu-id="74c96-176">Erstellen Sie die Projektmappe.</span><span class="sxs-lookup"><span data-stu-id="74c96-176">Build the solution.</span></span>

<span data-ttu-id="74c96-177">Sie können nun können Sie das Testprojekt einrichten.</span><span class="sxs-lookup"><span data-stu-id="74c96-177">You are now ready to set up the test project.</span></span>

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a><span data-ttu-id="74c96-178">Installieren von NuGet-Pakete im Test-Projekt</span><span class="sxs-lookup"><span data-stu-id="74c96-178">Install NuGet packages in test project</span></span>

<span data-ttu-id="74c96-179">Wenn Sie die leere Vorlage verwenden, um eine Anwendung erstellen, umfasst das Komponententestprojekt (StoreApp.Tests) keine installierten NuGet-Pakete.</span><span class="sxs-lookup"><span data-stu-id="74c96-179">When you use the Empty template to create an application, the unit test project (StoreApp.Tests) does not include any installed NuGet packages.</span></span> <span data-ttu-id="74c96-180">Andere Vorlagen, z. B. die Web-API-Vorlage enthalten einige NuGet-Pakete in das Komponententestprojekt.</span><span class="sxs-lookup"><span data-stu-id="74c96-180">Other templates, such as the Web API template, include some NuGet packages in the unit test project.</span></span> <span data-ttu-id="74c96-181">Für dieses Lernprogramm müssen Sie das Paket Microsoft ASP.NET Web API 2 Core des Testprojekts einschließen.</span><span class="sxs-lookup"><span data-stu-id="74c96-181">For this tutorial, you must include the Microsoft ASP.NET Web API 2 Core package to the test project.</span></span>

<span data-ttu-id="74c96-182">Mit der rechten Maustaste des StoreApp.Tests-Projekts, und wählen Sie **NuGet-Pakete verwalten**.</span><span class="sxs-lookup"><span data-stu-id="74c96-182">Right-click the StoreApp.Tests project and select **Manage NuGet Packages**.</span></span> <span data-ttu-id="74c96-183">Sie müssen das Projekt StoreApp.Tests, um die Pakete dem Projekt hinzufügen auswählen.</span><span class="sxs-lookup"><span data-stu-id="74c96-183">You must select the StoreApp.Tests project to add the packages to that project.</span></span>

![Verwalten von Paketen](unit-testing-with-aspnet-web-api/_static/image8.png)

<span data-ttu-id="74c96-185">Suchen Sie und installieren Sie Microsoft ASP.NET Web API 2-Core-Paket.</span><span class="sxs-lookup"><span data-stu-id="74c96-185">Find and install Microsoft ASP.NET Web API 2 Core package.</span></span>

![Installieren Sie Web api Core-Pakets](unit-testing-with-aspnet-web-api/_static/image9.png)

<span data-ttu-id="74c96-187">Schließen Sie das NuGet-Pakete verwalten-Fenster.</span><span class="sxs-lookup"><span data-stu-id="74c96-187">Close the Manage NuGet Packages window.</span></span>

<a id="tests"></a>
## <a name="create-tests"></a><span data-ttu-id="74c96-188">Tests erstellen</span><span class="sxs-lookup"><span data-stu-id="74c96-188">Create tests</span></span>

<span data-ttu-id="74c96-189">Standardmäßig enthält das Testprojekt eine leeren Test-Datei, die mit dem Namen "UnitTest1.cs".</span><span class="sxs-lookup"><span data-stu-id="74c96-189">By default, your test project includes an empty test file named UnitTest1.cs.</span></span> <span data-ttu-id="74c96-190">Diese Datei zeigt die Attribute, dass Sie zum Erstellen der Testmethoden verwenden.</span><span class="sxs-lookup"><span data-stu-id="74c96-190">This file shows the attributes you use to create test methods.</span></span> <span data-ttu-id="74c96-191">Für die Komponententests können Sie diese Datei verwenden oder erstellen eine eigene Datei.</span><span class="sxs-lookup"><span data-stu-id="74c96-191">For your unit tests, you can either use this file or create your own file.</span></span>

![UnitTest1](unit-testing-with-aspnet-web-api/_static/image10.png)

<span data-ttu-id="74c96-193">In diesem Lernprogramm erstellen Sie eine eigene Testklasse.</span><span class="sxs-lookup"><span data-stu-id="74c96-193">For this tutorial, you will create your own test class.</span></span> <span data-ttu-id="74c96-194">Sie können die Datei "UnitTest1.cs" löschen.</span><span class="sxs-lookup"><span data-stu-id="74c96-194">You can delete the UnitTest1.cs file.</span></span> <span data-ttu-id="74c96-195">Fügen Sie eine Klasse mit dem Namen **TestSimpleProductController.cs**, und Ersetzen Sie den Code durch den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="74c96-195">Add a class named **TestSimpleProductController.cs**, and replace the code with the following code.</span></span>

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample3.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a><span data-ttu-id="74c96-196">Tests durchführen</span><span class="sxs-lookup"><span data-stu-id="74c96-196">Run tests</span></span>

<span data-ttu-id="74c96-197">Sie sind jetzt bereit, um die Tests auszuführen.</span><span class="sxs-lookup"><span data-stu-id="74c96-197">You are now ready to run the tests.</span></span> <span data-ttu-id="74c96-198">Alle von der Methode, die mit markiert sind die **TestMethod** Attribut wird getestet werden.</span><span class="sxs-lookup"><span data-stu-id="74c96-198">All of the method that are marked with the **TestMethod** attribute will be tested.</span></span> <span data-ttu-id="74c96-199">Aus der **Test** Menüelement klicken, führen Sie die Tests.</span><span class="sxs-lookup"><span data-stu-id="74c96-199">From the **Test** menu item, run the tests.</span></span>

![Tests durchführen](unit-testing-with-aspnet-web-api/_static/image11.png)

<span data-ttu-id="74c96-201">Öffnen der **Test-Explorer** Fenster, und beachten Sie die Ergebnisse der Tests.</span><span class="sxs-lookup"><span data-stu-id="74c96-201">Open the **Test Explorer** window, and notice the results of the tests.</span></span>

![Testergebnisse](unit-testing-with-aspnet-web-api/_static/image12.png)

## <a name="summary"></a><span data-ttu-id="74c96-203">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="74c96-203">Summary</span></span>

<span data-ttu-id="74c96-204">Sie haben dieses Lernprogramm abgeschlossen.</span><span class="sxs-lookup"><span data-stu-id="74c96-204">You have completed this tutorial.</span></span> <span data-ttu-id="74c96-205">Die Daten in diesem Lernprogramm wurde absichtlich legen den Schwerpunkt auf Komponententests Bedingungen vereinfacht.</span><span class="sxs-lookup"><span data-stu-id="74c96-205">The data in this tutorial was intentionally simplified to focus on unit testing conditions.</span></span> <span data-ttu-id="74c96-206">Komponententests, die erweiterte Datenszenarien, finden Sie unter [imitieren Entity Framework beim Einheit Testen von ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).</span><span class="sxs-lookup"><span data-stu-id="74c96-206">For unit testing more advanced data scenarios, see [Mocking Entity Framework when Unit Testing ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).</span></span>