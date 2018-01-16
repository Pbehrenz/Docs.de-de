---
title: ASP.NET Core Middleware
author: rick-anderson
description: Informationen Sie zu ASP.NET Core Middleware und der Anforderungspipeline.
keywords: ASP.NET Core, Middleware-Pipeline, Delegaten
ms.author: riande
manager: wpickett
ms.date: 10/14/2017
ms.topic: article
ms.assetid: db9a86ab-46c2-40e0-baed-86e38c16af1f
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/middleware
ms.openlocfilehash: ad8d207b1e6de396f16d098fb07ddc89bea2c520
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
# <a name="aspnet-core-middleware-fundamentals"></a><span data-ttu-id="c7256-104">ASP.NET Core Middleware-Grundlagen</span><span class="sxs-lookup"><span data-stu-id="c7256-104">ASP.NET Core Middleware Fundamentals</span></span>

<a name="fundamentals-middleware"></a>

<span data-ttu-id="c7256-105">Durch [Rick Anderson](https://twitter.com/RickAndMSFT) und [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="c7256-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="c7256-106">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c7256-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="what-is-middleware"></a><span data-ttu-id="c7256-107">Was ist die Middleware</span><span class="sxs-lookup"><span data-stu-id="c7256-107">What is middleware</span></span>

<span data-ttu-id="c7256-108">Middleware ist eine Software, die in einer Pipeline der Anwendung zum Verarbeiten von Anforderungen und Antworten eingebaut wird.</span><span class="sxs-lookup"><span data-stu-id="c7256-108">Middleware is software that is assembled into an application pipeline to handle requests and responses.</span></span> <span data-ttu-id="c7256-109">Jede Komponente:</span><span class="sxs-lookup"><span data-stu-id="c7256-109">Each component:</span></span>

* <span data-ttu-id="c7256-110">Wählt aus, ob die Anforderung an die nächste Komponente in der Pipeline übergeben werden sollen.</span><span class="sxs-lookup"><span data-stu-id="c7256-110">Chooses whether to pass the request to the next component in the pipeline.</span></span>
* <span data-ttu-id="c7256-111">Kann Aufgaben ausführen, bevor und nachdem die nächste Komponente in der Pipeline aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="c7256-111">Can perform work before and after the next component in the pipeline is invoked.</span></span> 

<span data-ttu-id="c7256-112">Anforderung Delegaten werden verwendet, um die Pipeline zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="c7256-112">Request delegates are used to build the request pipeline.</span></span> <span data-ttu-id="c7256-113">Die Anforderung Delegaten behandeln jede HTTP-Anforderung.</span><span class="sxs-lookup"><span data-stu-id="c7256-113">The request delegates handle each HTTP request.</span></span>

<span data-ttu-id="c7256-114">Delegaten werden mithilfe von konfiguriert anfordern [ausführen](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions), [Zuordnung](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions), und [verwenden](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions) Erweiterungsmethoden [c#].</span><span class="sxs-lookup"><span data-stu-id="c7256-114">Request delegates are configured using [Run](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions), [Map](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions), and [Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions) extension methods.</span></span> <span data-ttu-id="c7256-115">Ein einzelne Anforderung Delegaten kann angegebene Zeile als einer anonymen Methode, die (als Inline-Middleware) sein, oder sie kann in einer wiederverwendbaren Klasse definiert werden.</span><span class="sxs-lookup"><span data-stu-id="c7256-115">An individual request delegate can be specified in-line as an anonymous method (called in-line middleware), or it can be defined in a reusable class.</span></span> <span data-ttu-id="c7256-116">Diese wiederverwendbare Klassen und Inline-anonyme Methoden sind *Middleware*, oder *middlewarekomponenten gemeinsam*.</span><span class="sxs-lookup"><span data-stu-id="c7256-116">These reusable classes and in-line anonymous methods are *middleware*, or *middleware components*.</span></span> <span data-ttu-id="c7256-117">Jede middlewarekomponente in der Anforderungspipeline ist verantwortlich für die nächste Komponente in der Pipeline aufrufen oder die Kette verkürzte, falls zutreffend.</span><span class="sxs-lookup"><span data-stu-id="c7256-117">Each middleware component in the request pipeline is responsible for invoking the next component in the pipeline, or short-circuiting the chain if appropriate.</span></span>

<span data-ttu-id="c7256-118">[Migrieren von HTTP-Module auf Middleware](../migration/http-modules.md) erläutert den Unterschied zwischen der Anforderung Pipelines in ASP.NET Core und den Vorgängerversionen und stellt Beispiele für weitere Middleware.</span><span class="sxs-lookup"><span data-stu-id="c7256-118">[Migrating HTTP Modules to Middleware](../migration/http-modules.md) explains the difference between request pipelines in ASP.NET Core and the previous versions and provides more middleware samples.</span></span>

## <a name="creating-a-middleware-pipeline-with-iapplicationbuilder"></a><span data-ttu-id="c7256-119">Erstellen eine middlewarepipeline mit IApplicationBuilder</span><span class="sxs-lookup"><span data-stu-id="c7256-119">Creating a middleware pipeline with IApplicationBuilder</span></span>

<span data-ttu-id="c7256-120">Die Anforderungspipeline ASP.NET Core besteht aus einer Sequenz von Anforderung-Delegaten, die nach der anderen aufgerufen, wie in diesem Diagramm (den Thread der Ausführung folgt der schwarzen Pfeile) gezeigt:</span><span class="sxs-lookup"><span data-stu-id="c7256-120">The ASP.NET Core request pipeline consists of a sequence of request delegates, called one after the other, as this diagram shows (the thread of execution follows the black arrows):</span></span>

![Anforderung verarbeiten des Musters mit der eine Anforderung, die empfangen, die Verarbeitung durch drei Middlewares und die Antwort, die die Anwendung.](middleware/_static/request-delegate-pipeline.png)

<span data-ttu-id="c7256-124">Jeder Delegat kann Vorgänge vor und nach dem nächsten Delegaten ausführen.</span><span class="sxs-lookup"><span data-stu-id="c7256-124">Each delegate can perform operations before and after the next delegate.</span></span> <span data-ttu-id="c7256-125">Ein Delegat können auch eine Anforderung nicht an dem nächsten Delegaten übergeben Sie die verkürzte der Anforderungspipeline aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="c7256-125">A delegate can also decide to not pass a request to the next delegate, which is called short-circuiting the request pipeline.</span></span> <span data-ttu-id="c7256-126">Verkürzte ist häufig wünschenswert, da unnötige Arbeit vermieden werden.</span><span class="sxs-lookup"><span data-stu-id="c7256-126">Short-circuiting is often desirable because it avoids unnecessary work.</span></span> <span data-ttu-id="c7256-127">Die Middleware für statische Dateien kann z. B. eine Anforderung für eine statische Datei zurückgeben und Kurzschluss den Rest der Pipeline.</span><span class="sxs-lookup"><span data-stu-id="c7256-127">For example, the static file middleware can return a request for a static file and short-circuit the rest of the pipeline.</span></span> <span data-ttu-id="c7256-128">Behandlung von Ausnahmen Delegaten müssen zu einem frühen Zeitpunkt in der Pipeline aufgerufen werden, sodass sie Ausnahmen abfangen können, die in späteren Phasen der Pipeline auftreten.</span><span class="sxs-lookup"><span data-stu-id="c7256-128">Exception-handling delegates need to be called early in the pipeline, so they can catch exceptions that occur in later stages of the pipeline.</span></span>

<span data-ttu-id="c7256-129">Die einfachste mögliche ASP.NET Core app richtet ein Anfrage-Delegat, der alle Anforderungen verarbeitet.</span><span class="sxs-lookup"><span data-stu-id="c7256-129">The simplest possible ASP.NET Core app sets up a single request delegate that handles all requests.</span></span> <span data-ttu-id="c7256-130">Eine tatsächliche Anforderungspipeline ist in diesem Fall enthalten.</span><span class="sxs-lookup"><span data-stu-id="c7256-130">This case doesn't include an actual request pipeline.</span></span> <span data-ttu-id="c7256-131">Stattdessen wird eine anonyme Funktion als Antwort auf jede HTTP-Anforderung aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="c7256-131">Instead, a single anonymous function is called in response to every HTTP request.</span></span>

[!code-csharp[Main](middleware/sample/Middleware/Startup.cs)]

<span data-ttu-id="c7256-132">Die erste [app. Führen Sie](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions) Delegat wird die Pipeline beendet.</span><span class="sxs-lookup"><span data-stu-id="c7256-132">The first [app.Run](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions) delegate terminates the pipeline.</span></span>

<span data-ttu-id="c7256-133">Sie können mehrere Anforderung Delegaten zusammen mit verketten [app. Verwendung](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions).</span><span class="sxs-lookup"><span data-stu-id="c7256-133">You can chain multiple request delegates together with [app.Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions).</span></span> <span data-ttu-id="c7256-134">Die `next` Parameter darstellt, den nächsten Delegaten in der Pipeline.</span><span class="sxs-lookup"><span data-stu-id="c7256-134">The `next` parameter represents the next delegate in the pipeline.</span></span> <span data-ttu-id="c7256-135">(Beachten Sie, dass Sie die Pipeline mit Circuit können *nicht* Aufrufen der *Weiter* Parameter.) Sie können in der Regel Aktionen vor und nach der nächsten Delegat ausführen, wie in diesem Beispiel veranschaulicht:</span><span class="sxs-lookup"><span data-stu-id="c7256-135">(Remember that you can short-circuit the pipeline by *not* calling the *next* parameter.) You can typically perform actions both before and after the next delegate, as this example demonstrates:</span></span>

[!code-csharp[Main](middleware/sample/Chain/Startup.cs?name=snippet1)]

>[!WARNING]
> <span data-ttu-id="c7256-136">Rufen Sie nicht `next.Invoke` nachdem die Antwort an den Client gesendet wurde.</span><span class="sxs-lookup"><span data-stu-id="c7256-136">Do not call `next.Invoke` after the response has been sent to the client.</span></span> <span data-ttu-id="c7256-137">Änderungen an `HttpResponse` nach dem Start der Antwort wird eine Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="c7256-137">Changes to `HttpResponse` after the response has started will throw an exception.</span></span> <span data-ttu-id="c7256-138">Änderungen, z. B. das Festlegen von Headern, Statuscode "" usw., werden z. B. eine Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="c7256-138">For example, changes such as setting headers, status code, etc,  will throw an exception.</span></span> <span data-ttu-id="c7256-139">Schreiben in den Antworttext nach dem Aufruf `next`:</span><span class="sxs-lookup"><span data-stu-id="c7256-139">Writing to the response body after calling `next`:</span></span>
> - <span data-ttu-id="c7256-140">Kann ein Protokoll verletzen.</span><span class="sxs-lookup"><span data-stu-id="c7256-140">May cause a protocol violation.</span></span> <span data-ttu-id="c7256-141">Schreiben Sie z. B. mehr als die genannten `content-length`.</span><span class="sxs-lookup"><span data-stu-id="c7256-141">For example, writing more than the stated `content-length`.</span></span>
> - <span data-ttu-id="c7256-142">Das Textformat kann zu einer Beschädigung.</span><span class="sxs-lookup"><span data-stu-id="c7256-142">May corrupt the body format.</span></span> <span data-ttu-id="c7256-143">Schreiben Sie z. B. eine HTML-Fußzeile in einer CSS-Datei.</span><span class="sxs-lookup"><span data-stu-id="c7256-143">For example, writing an HTML footer to a CSS file.</span></span>
>
> <span data-ttu-id="c7256-144">[HttpResponse.HasStarted](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.httpresponsefeature#Microsoft_AspNetCore_Http_Features_HttpResponseFeature_HasStarted) ist ein Hinweis nützlich, um anzugeben, ob der Header gesendet wurden, und/oder der Text wurde in den geschrieben.</span><span class="sxs-lookup"><span data-stu-id="c7256-144">[HttpResponse.HasStarted](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.httpresponsefeature#Microsoft_AspNetCore_Http_Features_HttpResponseFeature_HasStarted) is a useful hint to indicate if headers have been sent and/or the body has been written to.</span></span>

## <a name="ordering"></a><span data-ttu-id="c7256-145">Sortieren</span><span class="sxs-lookup"><span data-stu-id="c7256-145">Ordering</span></span>

<span data-ttu-id="c7256-146">Die Reihenfolge, die Middleware Komponenten, in hinzugefügt werden der `Configure` Methode definiert, die Reihenfolge, in dem sie aufgerufen werden für Anforderungen, und die umgekehrte Reihenfolge für die Antwort.</span><span class="sxs-lookup"><span data-stu-id="c7256-146">The order that middleware components are added in the `Configure` method defines the order in which they are invoked on requests, and the reverse order for the response.</span></span> <span data-ttu-id="c7256-147">Diese Reihenfolge ist wichtig für die Sicherheit, Leistung und Funktionalität.</span><span class="sxs-lookup"><span data-stu-id="c7256-147">This ordering is critical for security, performance, and functionality.</span></span>

<span data-ttu-id="c7256-148">Die Configure-Methode (siehe unten) Fügt die folgenden Middleware-Komponenten:</span><span class="sxs-lookup"><span data-stu-id="c7256-148">The Configure method (shown below) adds the following middleware components:</span></span>

1. <span data-ttu-id="c7256-149">Ausnahme/Fehlerbehandlung</span><span class="sxs-lookup"><span data-stu-id="c7256-149">Exception/error handling</span></span>
2. <span data-ttu-id="c7256-150">Statische Dateiserver</span><span class="sxs-lookup"><span data-stu-id="c7256-150">Static file server</span></span>
3. <span data-ttu-id="c7256-151">Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="c7256-151">Authentication</span></span>
4. <span data-ttu-id="c7256-152">MVC</span><span class="sxs-lookup"><span data-stu-id="c7256-152">MVC</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c7256-153">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c7256-153">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)


```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseExceptionHandler("/Home/Error"); // Call first to catch exceptions
                                            // thrown in the following middleware.

    app.UseStaticFiles();                   // Return static files and end pipeline.

    app.UseAuthentication();               // Authenticate before you access
                                           // secure resources.

    app.UseMvcWithDefaultRoute();          // Add MVC to the request pipeline.
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c7256-154">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c7256-154">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseExceptionHandler("/Home/Error"); // Call first to catch exceptions
                                            // thrown in the following middleware.

    app.UseStaticFiles();                   // Return static files and end pipeline.

    app.UseIdentity();                     // Authenticate before you access
                                           // secure resources.

    app.UseMvcWithDefaultRoute();          // Add MVC to the request pipeline.
}
```

-----------

<span data-ttu-id="c7256-155">Im obigen Code `UseExceptionHandler` ist die erste middlewarekomponente, die der Pipeline hinzugefügt – aus diesem Grund, fängt Sie alle Ausnahmen, die in späteren Aufrufen auftreten.</span><span class="sxs-lookup"><span data-stu-id="c7256-155">In the code above, `UseExceptionHandler` is the first middleware component added to the pipeline—therefore, it catches any exceptions that occur in later calls.</span></span>

<span data-ttu-id="c7256-156">Die Middleware für statische Dateien wird einem frühen Zeitpunkt in der Pipeline aufgerufen, damit Anforderungen verarbeitet und Kurzschluss, ohne die verbleibenden Komponenten durchlaufen werden kann.</span><span class="sxs-lookup"><span data-stu-id="c7256-156">The static file middleware is called early in the pipeline so it can handle requests and short-circuit without going through the remaining components.</span></span> <span data-ttu-id="c7256-157">Stellt die Middleware für statische Dateien **keine** autorisierungsüberprüfungen vorgenommen.</span><span class="sxs-lookup"><span data-stu-id="c7256-157">The static file middleware provides **no** authorization checks.</span></span> <span data-ttu-id="c7256-158">Alle Dateien von bedient, u. a. die *"Wwwroot"*, sind öffentlich verfügbar.</span><span class="sxs-lookup"><span data-stu-id="c7256-158">Any files served by it, including those under *wwwroot*, are publicly available.</span></span> <span data-ttu-id="c7256-159">Finden Sie unter [arbeiten mit statischen Dateien](xref:fundamentals/static-files) eine Methode, um statische Dateien zu sichern.</span><span class="sxs-lookup"><span data-stu-id="c7256-159">See [Working with static files](xref:fundamentals/static-files) for an approach to secure static files.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c7256-160">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c7256-160">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)


<span data-ttu-id="c7256-161">Wenn die Anforderung nicht von der Middleware für statische Dateien behandelt wird, erfolgt eine Übergabe auf an die Identity-Middleware (`app.UseAuthentication`), die die Authentifizierung durchführt.</span><span class="sxs-lookup"><span data-stu-id="c7256-161">If the request is not handled by the static file middleware, it's passed on to the Identity middleware (`app.UseAuthentication`), which performs authentication.</span></span> <span data-ttu-id="c7256-162">Identität nicht authentifizierte Anforderungen keinen Kurzschluss ausführt.</span><span class="sxs-lookup"><span data-stu-id="c7256-162">Identity does not short-circuit unauthenticated requests.</span></span> <span data-ttu-id="c7256-163">Obwohl Identität Anforderungen authentifiziert hat, tritt auf, Autorisierung (und Ablehnung) erst nach MVC einem bestimmten Razor-Seite oder Controller und Aktion auswählt.</span><span class="sxs-lookup"><span data-stu-id="c7256-163">Although Identity authenticates requests,  authorization (and rejection) occurs only after MVC selects a specific Razor Page or controller and action.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c7256-164">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c7256-164">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="c7256-165">Wenn die Anforderung nicht von der Middleware für statische Dateien behandelt wird, erfolgt eine Übergabe auf an die Identity-Middleware (`app.UseIdentity`), die die Authentifizierung durchführt.</span><span class="sxs-lookup"><span data-stu-id="c7256-165">If the request is not handled by the static file middleware, it's passed on to the Identity middleware (`app.UseIdentity`), which performs authentication.</span></span> <span data-ttu-id="c7256-166">Identität nicht authentifizierte Anforderungen keinen Kurzschluss ausführt.</span><span class="sxs-lookup"><span data-stu-id="c7256-166">Identity does not short-circuit unauthenticated requests.</span></span> <span data-ttu-id="c7256-167">Obwohl Identität Anforderungen authentifiziert hat, tritt auf, Autorisierung (und Ablehnung) erst nach MVC einen bestimmten Controller und Aktion auswählt.</span><span class="sxs-lookup"><span data-stu-id="c7256-167">Although Identity authenticates requests,  authorization (and rejection) occurs only after MVC selects a specific controller and action.</span></span>

-----------

<span data-ttu-id="c7256-168">Das folgende Beispiel zeigt eine Middleware, die Reihenfolge, in dem Anforderungen für statische Dateien vom die Middleware für statische Dateien vor der Komprimierung-Middleware Antwort behandelt werden.</span><span class="sxs-lookup"><span data-stu-id="c7256-168">The following example demonstrates a middleware ordering where requests for static files are handled by the static file middleware before the response compression middleware.</span></span> <span data-ttu-id="c7256-169">Statische Dateien werden nicht komprimiert, mit der diese Reihenfolge der Middleware.</span><span class="sxs-lookup"><span data-stu-id="c7256-169">Static files are not compressed with this ordering of the middleware.</span></span> <span data-ttu-id="c7256-170">Das MVC-Antworten von [UseMvcWithDefaultRoute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) können komprimiert werden.</span><span class="sxs-lookup"><span data-stu-id="c7256-170">The MVC responses from [UseMvcWithDefaultRoute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) can be compressed.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseStaticFiles();         // Static files not compressed
                                  // by middleware.
    app.UseResponseCompression();
    app.UseMvcWithDefaultRoute();
}
```

<a name="middleware-run-map-use"></a>

### <a name="use-run-and-map"></a><span data-ttu-id="c7256-171">Verwenden Sie, führen Sie aus und zuordnen</span><span class="sxs-lookup"><span data-stu-id="c7256-171">Use, Run, and Map</span></span>

<span data-ttu-id="c7256-172">Sie konfigurieren Sie das HTTP-Pipeline mit `Use`, `Run`, und `Map`.</span><span class="sxs-lookup"><span data-stu-id="c7256-172">You configure the HTTP pipeline using `Use`, `Run`, and `Map`.</span></span> <span data-ttu-id="c7256-173">Die `Use` Methode kann die Pipeline Kurzschluss (d. h., wenn er nicht aufgerufen wird eine `next` Anforderung Delegaten).</span><span class="sxs-lookup"><span data-stu-id="c7256-173">The `Use` method can short-circuit the pipeline (that is, if it does not call a `next` request delegate).</span></span> <span data-ttu-id="c7256-174">`Run`ist eine Konvention, und einige Komponenten Middleware können ausgesetzt `Run[Middleware]` Methoden, die am Ende der Pipeline ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="c7256-174">`Run` is a convention, and some middleware components may expose `Run[Middleware]` methods that run at the end of the pipeline.</span></span>

<span data-ttu-id="c7256-175">`Map*`Erweiterungen werden als eine Konvention verwendet, für die Pipeline zu verzweigen.</span><span class="sxs-lookup"><span data-stu-id="c7256-175">`Map*` extensions are used as a convention for branching the pipeline.</span></span> <span data-ttu-id="c7256-176">[Zuordnung](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions) Verzweigungen die Anforderungspipeline auf Übereinstimmungen von den angegebenen Anforderungspfad basierend.</span><span class="sxs-lookup"><span data-stu-id="c7256-176">[Map](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions) branches the request pipeline based on matches of the given request path.</span></span> <span data-ttu-id="c7256-177">Wenn der Anforderungspfad mit dem angegebenen Pfad beginnt, wird die Verzweigung ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="c7256-177">If the request path starts with the given path, the branch is executed.</span></span>

[!code-csharp[Main](middleware/sample/Chain/StartupMap.cs?name=snippet1)]

<span data-ttu-id="c7256-178">Die folgende Tabelle zeigt die Anforderungen und Antworten von `http://localhost:1234` mit dem vorherigen Code:</span><span class="sxs-lookup"><span data-stu-id="c7256-178">The following table shows the requests and responses from `http://localhost:1234` using the previous code:</span></span>

| <span data-ttu-id="c7256-179">Anforderung</span><span class="sxs-lookup"><span data-stu-id="c7256-179">Request</span></span> | <span data-ttu-id="c7256-180">Antwort</span><span class="sxs-lookup"><span data-stu-id="c7256-180">Response</span></span> |
| --- | --- |
| <span data-ttu-id="c7256-181">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="c7256-181">localhost:1234</span></span> | <span data-ttu-id="c7256-182">Hallo von nicht-Map-Delegat.</span><span class="sxs-lookup"><span data-stu-id="c7256-182">Hello from non-Map delegate.</span></span>  |
| <span data-ttu-id="c7256-183">Localhost:1234 / "map1"</span><span class="sxs-lookup"><span data-stu-id="c7256-183">localhost:1234/map1</span></span> | <span data-ttu-id="c7256-184">Test 1 zuordnen</span><span class="sxs-lookup"><span data-stu-id="c7256-184">Map Test 1</span></span> |
| <span data-ttu-id="c7256-185">Localhost:1234 / map2</span><span class="sxs-lookup"><span data-stu-id="c7256-185">localhost:1234/map2</span></span> | <span data-ttu-id="c7256-186">Test 2 für Zuordnung</span><span class="sxs-lookup"><span data-stu-id="c7256-186">Map Test 2</span></span> |
| <span data-ttu-id="c7256-187">Localhost:1234 / map3</span><span class="sxs-lookup"><span data-stu-id="c7256-187">localhost:1234/map3</span></span> | <span data-ttu-id="c7256-188">Hallo von nicht-Map-Delegat.</span><span class="sxs-lookup"><span data-stu-id="c7256-188">Hello from non-Map delegate.</span></span>  |

<span data-ttu-id="c7256-189">Wenn `Map` wird verwendet, werden die übereinstimmenden Pfad Segment(s) aus entfernt `HttpRequest.Path` und auf angefügten `HttpRequest.PathBase` für jede Anforderung.</span><span class="sxs-lookup"><span data-stu-id="c7256-189">When `Map` is used, the matched path segment(s) are removed from `HttpRequest.Path` and appended to `HttpRequest.PathBase` for each request.</span></span>

<span data-ttu-id="c7256-190">[MapWhen](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapwhenextensions) Verzweigungen die Anforderungspipeline auf dem Ergebnis des angegebenen Prädikats basierend.</span><span class="sxs-lookup"><span data-stu-id="c7256-190">[MapWhen](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapwhenextensions) branches the request pipeline based on the result of the given predicate.</span></span> <span data-ttu-id="c7256-191">Jedes Prädikat des Typs `Func<HttpContext, bool>` kann zum Zuordnen von Anforderungen in eine neue Verzweigung der Pipeline verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="c7256-191">Any predicate of type `Func<HttpContext, bool>` can be used to map requests to a new branch of the pipeline.</span></span> <span data-ttu-id="c7256-192">Im folgenden Beispiel wird ein Prädikat zum Erkennen des Vorhandenseins von Abfragezeichenfolgen-Variable verwendet `branch`:</span><span class="sxs-lookup"><span data-stu-id="c7256-192">In the following example, a predicate is used to detect the presence of a query string variable `branch`:</span></span>

[!code-csharp[Main](middleware/sample/Chain/StartupMapWhen.cs?name=snippet1)]

<span data-ttu-id="c7256-193">Die folgende Tabelle zeigt die Anforderungen und Antworten von `http://localhost:1234` mit dem vorherigen Code:</span><span class="sxs-lookup"><span data-stu-id="c7256-193">The following table shows the requests and responses from `http://localhost:1234` using the previous code:</span></span>

| <span data-ttu-id="c7256-194">Anforderung</span><span class="sxs-lookup"><span data-stu-id="c7256-194">Request</span></span> | <span data-ttu-id="c7256-195">Antwort</span><span class="sxs-lookup"><span data-stu-id="c7256-195">Response</span></span> |
| --- | --- |
| <span data-ttu-id="c7256-196">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="c7256-196">localhost:1234</span></span> | <span data-ttu-id="c7256-197">Hallo von nicht-Map-Delegat.</span><span class="sxs-lookup"><span data-stu-id="c7256-197">Hello from non-Map delegate.</span></span>  |
| <span data-ttu-id="c7256-198">Localhost:1234 /? Verzweigung = Master</span><span class="sxs-lookup"><span data-stu-id="c7256-198">localhost:1234/?branch=master</span></span> | <span data-ttu-id="c7256-199">Verzweigung verwendet = Master</span><span class="sxs-lookup"><span data-stu-id="c7256-199">Branch used = master</span></span>|

<span data-ttu-id="c7256-200">`Map`unterstützt das schachteln, z. B.:</span><span class="sxs-lookup"><span data-stu-id="c7256-200">`Map` supports nesting, for example:</span></span>

```csharp
app.Map("/level1", level1App => {
       level1App.Map("/level2a", level2AApp => {
           // "/level1/level2a"
           //...
       });
       level1App.Map("/level2b", level2BApp => {
           // "/level1/level2b"
           //...
       });
   });
   ```

<span data-ttu-id="c7256-201">`Map`kann auch mehrere Segmente gleichzeitig, zum Beispiel entsprechen:</span><span class="sxs-lookup"><span data-stu-id="c7256-201">`Map` can also match multiple segments at once, for example:</span></span>

 ```csharp
app.Map("/level1/level2", HandleMultiSeg);
```

## <a name="built-in-middleware"></a><span data-ttu-id="c7256-202">Integrierte middleware</span><span class="sxs-lookup"><span data-stu-id="c7256-202">Built-in middleware</span></span>

<span data-ttu-id="c7256-203">ASP.NET Core umfasst die folgenden Middleware-Komponenten:</span><span class="sxs-lookup"><span data-stu-id="c7256-203">ASP.NET Core ships with the following middleware components:</span></span>

| <span data-ttu-id="c7256-204">Middleware</span><span class="sxs-lookup"><span data-stu-id="c7256-204">Middleware</span></span> | <span data-ttu-id="c7256-205">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="c7256-205">Description</span></span> |
| ----- | ------- |
| [<span data-ttu-id="c7256-206">Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="c7256-206">Authentication</span></span>](xref:security/authentication/identity) | <span data-ttu-id="c7256-207">Bietet Unterstützung für die Authentifizierung an.</span><span class="sxs-lookup"><span data-stu-id="c7256-207">Provides authentication support.</span></span> |
| [<span data-ttu-id="c7256-208">CORS</span><span class="sxs-lookup"><span data-stu-id="c7256-208">CORS</span></span>](xref:security/cors) | <span data-ttu-id="c7256-209">Konfiguriert die Cross-Origin Resource Sharing.</span><span class="sxs-lookup"><span data-stu-id="c7256-209">Configures Cross-Origin Resource Sharing.</span></span> |
| [<span data-ttu-id="c7256-210">Zwischenspeichern von Antworten</span><span class="sxs-lookup"><span data-stu-id="c7256-210">Response Caching</span></span>](xref:performance/caching/middleware) | <span data-ttu-id="c7256-211">Bietet Unterstützung für das Zwischenspeichern von Antworten.</span><span class="sxs-lookup"><span data-stu-id="c7256-211">Provides support for caching responses.</span></span> |
| [<span data-ttu-id="c7256-212">Antwort-Komprimierung</span><span class="sxs-lookup"><span data-stu-id="c7256-212">Response Compression</span></span>](xref:performance/response-compression) | <span data-ttu-id="c7256-213">Bietet Unterstützung für die Komprimierung von Antworten an.</span><span class="sxs-lookup"><span data-stu-id="c7256-213">Provides support for compressing responses.</span></span> |
| [<span data-ttu-id="c7256-214">Routing</span><span class="sxs-lookup"><span data-stu-id="c7256-214">Routing</span></span>](xref:fundamentals/routing) | <span data-ttu-id="c7256-215">Definiert, und schränkt die Routen der Anforderung.</span><span class="sxs-lookup"><span data-stu-id="c7256-215">Defines and constrains request routes.</span></span> |
| [<span data-ttu-id="c7256-216">Sitzung</span><span class="sxs-lookup"><span data-stu-id="c7256-216">Session</span></span>](xref:fundamentals/app-state) | <span data-ttu-id="c7256-217">Bietet Unterstützung für die Verwaltung von benutzersitzungen.</span><span class="sxs-lookup"><span data-stu-id="c7256-217">Provides support for managing user sessions.</span></span> |
| [<span data-ttu-id="c7256-218">Statische Dateien</span><span class="sxs-lookup"><span data-stu-id="c7256-218">Static Files</span></span>](xref:fundamentals/static-files) | <span data-ttu-id="c7256-219">Bietet Unterstützung für statische Dateien und die Verzeichnissuche bedient.</span><span class="sxs-lookup"><span data-stu-id="c7256-219">Provides support for serving static files and directory browsing.</span></span> |
| [<span data-ttu-id="c7256-220">URL-umschreibende Middleware</span><span class="sxs-lookup"><span data-stu-id="c7256-220">URL Rewriting Middleware</span></span>](xref:fundamentals/url-rewriting) | <span data-ttu-id="c7256-221">Bietet Unterstützung für das Umschreiben von URLs und das Umleiten von Anforderungen.</span><span class="sxs-lookup"><span data-stu-id="c7256-221">Provides support for rewriting URLs and redirecting requests.</span></span> |

<a name="middleware-writing-middleware"></a>

## <a name="writing-middleware"></a><span data-ttu-id="c7256-222">Schreiben von middleware</span><span class="sxs-lookup"><span data-stu-id="c7256-222">Writing middleware</span></span>

<span data-ttu-id="c7256-223">Middleware wird in der Regel in einer Klasse gekapselt und mit einer Erweiterungsmethode verfügbar gemacht.</span><span class="sxs-lookup"><span data-stu-id="c7256-223">Middleware is generally encapsulated in a class and exposed with an extension method.</span></span> <span data-ttu-id="c7256-224">Betrachten Sie die folgenden Middleware, die aus der Abfragezeichenfolge die Kultur für die aktuelle Anforderung festlegt:</span><span class="sxs-lookup"><span data-stu-id="c7256-224">Consider the following middleware, which sets the culture for the current request from the query string:</span></span>

[!code-csharp[Main](middleware/sample/Culture/StartupCulture.cs?name=snippet1)]

<span data-ttu-id="c7256-225">Hinweis: Im obigen Beispielcode dient zur Veranschaulichung erstellen eine middlewarekomponente.</span><span class="sxs-lookup"><span data-stu-id="c7256-225">Note: The sample code above is used to demonstrate creating a middleware component.</span></span> <span data-ttu-id="c7256-226">Finden Sie unter [ Globalisierung und Lokalisierung](xref:fundamentals/localization) für ASP.NET Core des integrierten lokalisierungsunterstützung.</span><span class="sxs-lookup"><span data-stu-id="c7256-226">See [ Globalization and localization](xref:fundamentals/localization) for ASP.NET Core's built-in localization support.</span></span>

<span data-ttu-id="c7256-227">Sie können die Middleware testen, indem Sie in der Kultur, z. B. Übergabe `http://localhost:7997/?culture=no`.</span><span class="sxs-lookup"><span data-stu-id="c7256-227">You can test the middleware by passing in the culture, for example `http://localhost:7997/?culture=no`.</span></span>

<span data-ttu-id="c7256-228">Der folgende Code wechselt der Middleware-Delegat in einer Klasse:</span><span class="sxs-lookup"><span data-stu-id="c7256-228">The following code moves the middleware delegate to a class:</span></span>

[!code-csharp[Main](middleware/sample/Culture/RequestCultureMiddleware.cs)]

<span data-ttu-id="c7256-229">Die folgende Erweiterungsmethode macht die Middleware über [IApplicationBuilder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder):</span><span class="sxs-lookup"><span data-stu-id="c7256-229">The following extension method exposes the middleware through [IApplicationBuilder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder):</span></span>

[!code-csharp[Main](middleware/sample/Culture/RequestCultureMiddlewareExtensions.cs)]

<span data-ttu-id="c7256-230">Der folgende Code Ruft die Middleware aus `Configure`:</span><span class="sxs-lookup"><span data-stu-id="c7256-230">The following code calls the middleware from `Configure`:</span></span>

[!code-csharp[Main](middleware/sample/Culture/Startup.cs?name=snippet1&highlight=5)]

<span data-ttu-id="c7256-231">Middleware befolgen die [expliziten Abhängigkeiten Prinzip](http://deviq.com/explicit-dependencies-principle/) , verfügbar machen, dessen Abhängigkeiten in ihren Konstruktor.</span><span class="sxs-lookup"><span data-stu-id="c7256-231">Middleware should follow the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/) by exposing its dependencies in its constructor.</span></span> <span data-ttu-id="c7256-232">Middleware wird einmal pro erstellt *Lebensdauer der Anwendung*.</span><span class="sxs-lookup"><span data-stu-id="c7256-232">Middleware is constructed once per *application lifetime*.</span></span> <span data-ttu-id="c7256-233">Finden Sie unter *Abhängigkeiten pro Anforderung* unten für den Fall müssen Sie Dienste mit Middleware innerhalb einer Anforderung freigeben.</span><span class="sxs-lookup"><span data-stu-id="c7256-233">See *Per-request dependencies* below if you need to share services with middleware within a request.</span></span>

<span data-ttu-id="c7256-234">Middleware-Komponenten können ihre Abhängigkeiten von Abhängigkeitsinjektion über Konstruktorparameter aufgelöst werden.</span><span class="sxs-lookup"><span data-stu-id="c7256-234">Middleware components can resolve their dependencies from dependency injection through constructor parameters.</span></span> <span data-ttu-id="c7256-235">[`UseMiddleware<T>`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.usemiddlewareextensions#methods_summary)kann auch weitere Parameter direkt akzeptieren.</span><span class="sxs-lookup"><span data-stu-id="c7256-235">[`UseMiddleware<T>`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.usemiddlewareextensions#methods_summary) can also accept additional parameters directly.</span></span>

### <a name="per-request-dependencies"></a><span data-ttu-id="c7256-236">Abhängigkeiten pro Anforderung</span><span class="sxs-lookup"><span data-stu-id="c7256-236">Per-request dependencies</span></span>

<span data-ttu-id="c7256-237">Da Middleware, beim Starten der app, nicht pro Anforderung erstellt wurde, *im Bereich einer* Lebensdauerdienste von Middleware Konstruktoren verwendet werden nicht für andere Typen Abhängigkeit eingefügter freigegeben, während jede Anforderung.</span><span class="sxs-lookup"><span data-stu-id="c7256-237">Because middleware is constructed at app startup, not per-request, *scoped* lifetime services used by middleware constructors are not  shared with other dependency-injected types during each request.</span></span> <span data-ttu-id="c7256-238">Wenn Sie freigeben müssen eine *im Bereich einer* service zwischen Ihrer Middleware und anderen Projekttypen, fügen Sie diese Dienste für die `Invoke` Methodensignatur.</span><span class="sxs-lookup"><span data-stu-id="c7256-238">If you must share a *scoped* service between your middleware and other types, add these services to the `Invoke` method's signature.</span></span> <span data-ttu-id="c7256-239">Die `Invoke` Methode kann zusätzliche Parameter annehmen, die vom Abhängigkeitsinjektion aufgefüllt werden.</span><span class="sxs-lookup"><span data-stu-id="c7256-239">The `Invoke` method can accept additional parameters that are populated by dependency injection.</span></span> <span data-ttu-id="c7256-240">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="c7256-240">For example:</span></span>

```c#
public class MyMiddleware
{
    private readonly RequestDelegate _next;

    public MyMiddleware(RequestDelegate next)
    {
        _next = next;
    }

    public async Task Invoke(HttpContext httpContext, IMyScopedService svc)
    {
        svc.MyProperty = 1000;
        await _next(httpContext);
    }
}
```

## <a name="resources"></a><span data-ttu-id="c7256-241">Ressourcen</span><span class="sxs-lookup"><span data-stu-id="c7256-241">Resources</span></span>

* [<span data-ttu-id="c7256-242">Beispielcode, der in diesem Dokument verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="c7256-242">Sample code used in this doc</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/sample)
* [<span data-ttu-id="c7256-243">Migrating HTTP Modules to Middleware (Migration von HTTP-Modulen zu Middleware)</span><span class="sxs-lookup"><span data-stu-id="c7256-243">Migrating HTTP Modules to Middleware</span></span>](../migration/http-modules.md)
* [<span data-ttu-id="c7256-244">Application Startup (Starten von Anwendungen)</span><span class="sxs-lookup"><span data-stu-id="c7256-244">Application Startup</span></span>](startup.md)
* [<span data-ttu-id="c7256-245">Erforderliche Funktionen</span><span class="sxs-lookup"><span data-stu-id="c7256-245">Request Features</span></span>](request-features.md)