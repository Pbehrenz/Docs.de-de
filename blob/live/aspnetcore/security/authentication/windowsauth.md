---
title: Konfigurieren der Windows-Authentifizierung in ASP.NET Core
author: ardalis
description: Dieser Artikel beschreibt, wie die Windows-Authentifizierung in ASP.NET Core mit IIS Express, IIS, HTTP.sys und WebListener konfiguriert.
keywords: ASP.NET Core, Windows-Authentifizierung, Authorize-Attribut, AllowAnonymous-Attribut
ms.author: riande
manager: wpickett
ms.date: 10/24/2017
ms.topic: article
ms.assetid: cf119f21-1a2b-49a2-b052-548ccb66ee83
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/windowsauth
ms.openlocfilehash: e5ceffe5b7f7e3ef4f6158b6b7b7d571a21ee130
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2018
---
# <a name="configure-windows-authentication-in-an-aspnet-core-app"></a><span data-ttu-id="9bfc8-104">Konfigurieren der Windows-Authentifizierung in einer ASP.NET Core-app</span><span class="sxs-lookup"><span data-stu-id="9bfc8-104">Configure Windows authentication in an ASP.NET Core app</span></span>

<span data-ttu-id="9bfc8-105">Durch [Steve Smith](https://ardalis.com) und [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="9bfc8-105">By [Steve Smith](https://ardalis.com) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="9bfc8-106">Windows-Authentifizierung konfiguriert werden kann, für ASP.NET Core-apps, die mit IIS gehostet [HTTP.sys](xref:fundamentals/servers/httpsys), oder [WebListener](xref:fundamentals/servers/weblistener).</span><span class="sxs-lookup"><span data-stu-id="9bfc8-106">Windows authentication can be configured for ASP.NET Core apps hosted with IIS, [HTTP.sys](xref:fundamentals/servers/httpsys), or [WebListener](xref:fundamentals/servers/weblistener).</span></span>

## <a name="what-is-windows-authentication"></a><span data-ttu-id="9bfc8-107">Was ist Windows-Authentifizierung?</span><span class="sxs-lookup"><span data-stu-id="9bfc8-107">What is Windows authentication?</span></span>

<span data-ttu-id="9bfc8-108">Windows-Authentifizierung basiert auf dem Betriebssystem zur Authentifizierung von Benutzern von ASP.NET Core-apps.</span><span class="sxs-lookup"><span data-stu-id="9bfc8-108">Windows authentication relies on the operating system to authenticate users of ASP.NET Core apps.</span></span> <span data-ttu-id="9bfc8-109">Sie können Windows-Authentifizierung verwenden, wenn Ihre Server in einem Unternehmensnetzwerk mithilfe von Active Directory-Domäne-Identitäten oder andere Windows-Konten zur Identifizierung von Benutzern ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="9bfc8-109">You can use Windows authentication when your server runs on a corporate network using Active Directory domain identities or other Windows accounts to identify users.</span></span> <span data-ttu-id="9bfc8-110">Windows-Authentifizierung ist für Intranetumgebungen, in denen Benutzer, Clientanwendungen und Webservern zu derselben Windows-Domäne gehören, geeignet.</span><span class="sxs-lookup"><span data-stu-id="9bfc8-110">Windows authentication is best suited to intranet environments in which users, client applications, and web servers belong to the same Windows domain.</span></span>

<span data-ttu-id="9bfc8-111">[Erfahren Sie mehr über Windows-Authentifizierung und die Installation für IIS](https://docs.microsoft.com/iis/configuration/system.webServer/security/authentication/windowsAuthentication/).</span><span class="sxs-lookup"><span data-stu-id="9bfc8-111">[Learn more about Windows authentication and installing it for IIS](https://docs.microsoft.com/iis/configuration/system.webServer/security/authentication/windowsAuthentication/).</span></span>

## <a name="enable-windows-authentication-in-an-aspnet-core-app"></a><span data-ttu-id="9bfc8-112">Windows-Authentifizierung in einer ASP.NET Core app aktivieren</span><span class="sxs-lookup"><span data-stu-id="9bfc8-112">Enable Windows authentication in an ASP.NET Core app</span></span>

<span data-ttu-id="9bfc8-113">Der Visual Studio Web Application-Vorlage kann für die Unterstützung der Windows-Authentifizierung konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="9bfc8-113">The Visual Studio Web Application template can be configured to support Windows authentication.</span></span>

### <a name="use-the-windows-authentication-app-template"></a><span data-ttu-id="9bfc8-114">Verwenden Sie die Windows-Authentifizierung app-Vorlage</span><span class="sxs-lookup"><span data-stu-id="9bfc8-114">Use the Windows authentication app template</span></span>

<span data-ttu-id="9bfc8-115">In Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="9bfc8-115">In Visual Studio:</span></span>
1. <span data-ttu-id="9bfc8-116">Erstellen Sie eine neue ASP.NET Core-Webanwendung.</span><span class="sxs-lookup"><span data-stu-id="9bfc8-116">Create a new ASP.NET Core Web Application.</span></span> 
1. <span data-ttu-id="9bfc8-117">Wählen Sie aus der Liste der Vorlagen-Webanwendung.</span><span class="sxs-lookup"><span data-stu-id="9bfc8-117">Select Web Application from the list of templates.</span></span>
1. <span data-ttu-id="9bfc8-118">Wählen Sie die **Authentifizierung ändern** Schaltfläche und wählen Sie **Windows-Authentifizierung**.</span><span class="sxs-lookup"><span data-stu-id="9bfc8-118">Select the **Change Authentication** button and select **Windows Authentication**.</span></span> 

<span data-ttu-id="9bfc8-119">Führen Sie die App aus.</span><span class="sxs-lookup"><span data-stu-id="9bfc8-119">Run the app.</span></span> <span data-ttu-id="9bfc8-120">Der Benutzername wird angezeigt, in der oberen rechten der app.</span><span class="sxs-lookup"><span data-stu-id="9bfc8-120">The username appears in the top right of the app.</span></span>

![Windows-Authentifizierung-Browser-Screenshot](windowsauth/_static/browser-screenshot.png)

<span data-ttu-id="9bfc8-122">Die Vorlage bereitstellt für Entwicklungen mit IIS Express die zur Verwendung der Windows-Authentifizierung erforderlich Konfiguration.</span><span class="sxs-lookup"><span data-stu-id="9bfc8-122">For development work using IIS Express, the template provides all the configuration necessary to use Windows authentication.</span></span> <span data-ttu-id="9bfc8-123">Der folgende Abschnitt zeigt, wie eine ASP.NET Core-app für Windows-Authentifizierung manuell konfiguriert wird.</span><span class="sxs-lookup"><span data-stu-id="9bfc8-123">The following section shows how to manually configure an ASP.NET Core app for Windows authentication.</span></span>

### <a name="visual-studio-settings-for-windows-and-anonymous-authentication"></a><span data-ttu-id="9bfc8-124">Visual Studio-Einstellungen für Windows und anonyme Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="9bfc8-124">Visual Studio settings for Windows and anonymous authentication</span></span>

<span data-ttu-id="9bfc8-125">Visual Studio-Projekt **Eigenschaften** Seite **Debuggen** Registerkarte enthält die Kontrollkästchen für die Windows-Authentifizierung und die anonyme Authentifizierung.</span><span class="sxs-lookup"><span data-stu-id="9bfc8-125">The Visual Studio project **Properties** page's **Debug** tab provides check boxes for Windows authentication and anonymous authentication.</span></span>

![Windows-Authentifizierung-Browser-Screenshot](windowsauth/_static/vs-auth-property-menu.png)

<span data-ttu-id="9bfc8-127">Alternativ können diese beiden Eigenschaften konfiguriert werden, der *launchSettings.json* Datei:</span><span class="sxs-lookup"><span data-stu-id="9bfc8-127">Alternatively, these two properties can be configured in the *launchSettings.json* file:</span></span>

[!code-json[](windowsauth/sample/launchSettings.json?highlight=3-4)]

## <a name="enable-windows-authentication-with-iis"></a><span data-ttu-id="9bfc8-128">Windows-Authentifizierung in IIS aktivieren</span><span class="sxs-lookup"><span data-stu-id="9bfc8-128">Enable Windows authentication with IIS</span></span>

<span data-ttu-id="9bfc8-129">IIS verwendet den [ASP.NET Core-Modul](xref:fundamentals/servers/aspnet-core-module) (ANCM) zum Hosten von ASP.NET Core-apps.</span><span class="sxs-lookup"><span data-stu-id="9bfc8-129">IIS uses the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) (ANCM) to host ASP.NET Core apps.</span></span> <span data-ttu-id="9bfc8-130">Die ANCM Flüsse Windows-Authentifizierung in IIS standardmäßig.</span><span class="sxs-lookup"><span data-stu-id="9bfc8-130">The ANCM flows Windows authentication to IIS by default.</span></span> <span data-ttu-id="9bfc8-131">Konfiguration der Windows-Authentifizierung erfolgt innerhalb von IIS nicht auf das Anwendungsprojekt.</span><span class="sxs-lookup"><span data-stu-id="9bfc8-131">Configuration of Windows authentication is done within IIS, not the application project.</span></span> <span data-ttu-id="9bfc8-132">Die folgenden Abschnitte zeigen, wie IIS-Manager verwenden, um eine ASP.NET Core-Anwendung, um die Windows-Authentifizierung zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="9bfc8-132">The following sections show how to use IIS Manager to configure an ASP.NET Core app to use Windows authentication.</span></span>

### <a name="create-a-new-iis-site"></a><span data-ttu-id="9bfc8-133">Erstellen einer neuen IIS-Website</span><span class="sxs-lookup"><span data-stu-id="9bfc8-133">Create a new IIS site</span></span>

<span data-ttu-id="9bfc8-134">Geben Sie einen Namen und den Ordner, und lassen sie einen neuen Anwendungspool zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="9bfc8-134">Specify a name and folder and allow it to create a new application pool.</span></span>

### <a name="customize-authentication"></a><span data-ttu-id="9bfc8-135">Anpassen der Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="9bfc8-135">Customize authentication</span></span>

<span data-ttu-id="9bfc8-136">Öffnen Sie die Authentifizierung für den Standort.</span><span class="sxs-lookup"><span data-stu-id="9bfc8-136">Open the Authentication menu for the site.</span></span>

![Menü für IIS-Authentifizierung](windowsauth/_static/iis-authentication-menu.png)

<span data-ttu-id="9bfc8-138">Deaktivieren Sie anonyme Authentifizierung und aktivieren Sie die Windows-Authentifizierung.</span><span class="sxs-lookup"><span data-stu-id="9bfc8-138">Disable Anonymous Authentication and enable Windows Authentication.</span></span>

![IIS-Authentifizierungseinstellungen](windowsauth/_static/iis-auth-settings.png)

### <a name="publish-your-project-to-the-iis-site-folder"></a><span data-ttu-id="9bfc8-140">Veröffentlichen Sie Ihr Projekt in den IIS-Website-Ordner</span><span class="sxs-lookup"><span data-stu-id="9bfc8-140">Publish your project to the IIS site folder</span></span>

<span data-ttu-id="9bfc8-141">Veröffentlichen Sie die app mithilfe von Visual Studio oder die .NET Core-CLI, in den Zielordner an.</span><span class="sxs-lookup"><span data-stu-id="9bfc8-141">Using Visual Studio or the .NET Core CLI, publish the app to the destination folder.</span></span>

![Visual Studio, Dialogfeld "Veröffentlichen"](windowsauth/_static/vs-publish-app.png)

<span data-ttu-id="9bfc8-143">Erfahren Sie mehr über [in IIS veröffentlichen](xref:host-and-deploy/iis/index).</span><span class="sxs-lookup"><span data-stu-id="9bfc8-143">Learn more about [publishing to IIS](xref:host-and-deploy/iis/index).</span></span>

<span data-ttu-id="9bfc8-144">Starten Sie die app aus, um sicherzustellen, dass die Windows-Authentifizierung funktionsfähig ist.</span><span class="sxs-lookup"><span data-stu-id="9bfc8-144">Launch the app to verify Windows authentication is working.</span></span>

## <a name="enable-windows-authentication-with-httpsys-or-weblistener"></a><span data-ttu-id="9bfc8-145">Windows-Authentifizierung mit HTTP.sys oder WebListener aktivieren</span><span class="sxs-lookup"><span data-stu-id="9bfc8-145">Enable Windows authentication with HTTP.sys or WebListener</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9bfc8-146">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9bfc8-146">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="9bfc8-147">Obwohl Kestrel Windows-Authentifizierung nicht unterstützt, können Sie [HTTP.sys](xref:fundamentals/servers/httpsys) selbst gehostete Szenarien unter Windows unterstützt.</span><span class="sxs-lookup"><span data-stu-id="9bfc8-147">Although Kestrel doesn't support Windows authentication, you can use [HTTP.sys](xref:fundamentals/servers/httpsys) to support self-hosted scenarios on Windows.</span></span> <span data-ttu-id="9bfc8-148">Das folgende Beispiel konfiguriert die app Webhost um HTTP.sys mit Windows-Authentifizierung zu verwenden:</span><span class="sxs-lookup"><span data-stu-id="9bfc8-148">The following example configures the app's web host to use HTTP.sys with Windows authentication:</span></span>

[!code-csharp[](windowsauth/sample/Program2x.cs?highlight=9-14)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9bfc8-149">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9bfc8-149">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="9bfc8-150">Obwohl Kestrel Windows-Authentifizierung nicht unterstützt, können Sie [WebListener](xref:fundamentals/servers/weblistener) selbst gehostete Szenarien unter Windows unterstützt.</span><span class="sxs-lookup"><span data-stu-id="9bfc8-150">Although Kestrel doesn't support Windows authentication, you can use [WebListener](xref:fundamentals/servers/weblistener) to support self-hosted scenarios on Windows.</span></span> <span data-ttu-id="9bfc8-151">Das folgende Beispiel konfiguriert die app Webhost um WebListener mit Windows-Authentifizierung zu verwenden:</span><span class="sxs-lookup"><span data-stu-id="9bfc8-151">The following example configures the app's web host to use WebListener with Windows authentication:</span></span>

[!code-csharp[](windowsauth/sample/Program1x.cs?highlight=6-11)]

---

## <a name="work-with-windows-authentication"></a><span data-ttu-id="9bfc8-152">Arbeiten mit Windows-Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="9bfc8-152">Work with Windows authentication</span></span>

<span data-ttu-id="9bfc8-153">Der Konfigurationsstatus des anonymen Zugriffs bestimmt die Art, wie die `[Authorize]` und `[AllowAnonymous]` Attribute werden in der app verwendet.</span><span class="sxs-lookup"><span data-stu-id="9bfc8-153">The configuration state of anonymous access determines the way in which the `[Authorize]` and `[AllowAnonymous]` attributes are used in the app.</span></span> <span data-ttu-id="9bfc8-154">Die folgenden beiden Abschnitten wird erläutert, wie die Zustände, zulässige und unzulässige Konfiguration des anonymen Zugriffs zu behandeln.</span><span class="sxs-lookup"><span data-stu-id="9bfc8-154">The following two sections explain how to handle the disallowed and allowed configuration states of anonymous access.</span></span>

### <a name="disallow-anonymous-access"></a><span data-ttu-id="9bfc8-155">Anonyme Zugriffe nicht zulassen</span><span class="sxs-lookup"><span data-stu-id="9bfc8-155">Disallow anonymous access</span></span>

<span data-ttu-id="9bfc8-156">Wenn Windows-Authentifizierung aktiviert ist und der anonyme Zugriff deaktiviert ist, die `[Authorize]` und `[AllowAnonymous]` Attribute haben keine Auswirkungen.</span><span class="sxs-lookup"><span data-stu-id="9bfc8-156">When Windows authentication is enabled and anonymous access is disabled, the `[Authorize]` and `[AllowAnonymous]` attributes have no effect.</span></span> <span data-ttu-id="9bfc8-157">Wenn die IIS-Website (oder HTTP.sys oder WebListener Server) konfiguriert ist, um den anonymen Zugriff zu unterbinden, erreicht die Anforderung nie Ihrer app.</span><span class="sxs-lookup"><span data-stu-id="9bfc8-157">If the IIS site (or HTTP.sys or WebListener server) is configured to disallow anonymous access, the request never reaches your app.</span></span> <span data-ttu-id="9bfc8-158">Aus diesem Grund die `[AllowAnonymous]` Attribut nicht zutreffend ist.</span><span class="sxs-lookup"><span data-stu-id="9bfc8-158">For this reason, the `[AllowAnonymous]` attribute isn't applicable.</span></span>

### <a name="allow-anonymous-access"></a><span data-ttu-id="9bfc8-159">Anonymen Zugriff zulassen</span><span class="sxs-lookup"><span data-stu-id="9bfc8-159">Allow anonymous access</span></span>

<span data-ttu-id="9bfc8-160">Wenn Windows-Authentifizierung und anonymem Zugriff aktiviert sind, verwenden die `[Authorize]` und `[AllowAnonymous]` Attribute.</span><span class="sxs-lookup"><span data-stu-id="9bfc8-160">When both Windows authentication and anonymous access are enabled, use the `[Authorize]` and `[AllowAnonymous]` attributes.</span></span> <span data-ttu-id="9bfc8-161">Die `[Authorize]` Attribut können Sie Teile der app zu sichern die Windows-Authentifizierung tatsächlich benötigen.</span><span class="sxs-lookup"><span data-stu-id="9bfc8-161">The `[Authorize]` attribute allows you to secure pieces of the app which truly do require Windows authentication.</span></span> <span data-ttu-id="9bfc8-162">Die `[AllowAnonymous]` -Attribut überschreibt `[Authorize]` -Attribut Auslastung in apps, die anonymen Zugriff zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="9bfc8-162">The `[AllowAnonymous]` attribute overrides `[Authorize]` attribute usage within apps which allow anonymous access.</span></span> <span data-ttu-id="9bfc8-163">Finden Sie unter [einfache Autorisierung](xref:security/authorization/simple) für Nutzungsdetails Attribut.</span><span class="sxs-lookup"><span data-stu-id="9bfc8-163">See [Simple Authorization](xref:security/authorization/simple) for attribute usage details.</span></span>

<span data-ttu-id="9bfc8-164">In ASP.NET Core 2.x, die `[Authorize]` Attribut erfordert zusätzliche Konfigurationsschritte in *Startup.cs* , fordern Sie anonyme Anforderungen für die Windows-Authentifizierung.</span><span class="sxs-lookup"><span data-stu-id="9bfc8-164">In ASP.NET Core 2.x, the `[Authorize]` attribute requires additional configuration in *Startup.cs* to challenge anonymous requests for Windows authentication.</span></span> <span data-ttu-id="9bfc8-165">Die empfohlene Konfiguration variiert leicht basierend auf dem Webserver verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="9bfc8-165">The recommended configuration varies slightly based on the web server being used.</span></span>

#### <a name="iis"></a><span data-ttu-id="9bfc8-166">IIS</span><span class="sxs-lookup"><span data-stu-id="9bfc8-166">IIS</span></span>

<span data-ttu-id="9bfc8-167">Wenn Sie IIS verwenden, fügen Sie Folgendes verwenden, um die `ConfigureServices` Methode:</span><span class="sxs-lookup"><span data-stu-id="9bfc8-167">If using IIS, add the following to the `ConfigureServices` method:</span></span> 

```csharp
// IISDefaults requires the following import:
// using Microsoft.AspNetCore.Server.IISIntegration;
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

#### <a name="httpsys"></a><span data-ttu-id="9bfc8-168">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="9bfc8-168">HTTP.sys</span></span>

<span data-ttu-id="9bfc8-169">Wenn HTTP.sys verwenden zu können, fügen Sie Folgendes verwenden, um die `ConfigureServices` Methode:</span><span class="sxs-lookup"><span data-stu-id="9bfc8-169">If using HTTP.sys, add the following to the `ConfigureServices` method:</span></span>

```csharp
// HttpSysDefaults requires the following import:
// using Microsoft.AspNetCore.Server.HttpSys;
services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
```

### <a name="impersonation"></a><span data-ttu-id="9bfc8-170">Identitätswechsel</span><span class="sxs-lookup"><span data-stu-id="9bfc8-170">Impersonation</span></span>

<span data-ttu-id="9bfc8-171">ASP.NET Core implementiert keine Identitätswechsel.</span><span class="sxs-lookup"><span data-stu-id="9bfc8-171">ASP.NET Core doesn't implement impersonation.</span></span> <span data-ttu-id="9bfc8-172">Apps, die mit der Anwendungsidentität für alle Anforderungen, die mit app-Pool oder Prozess Identität ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="9bfc8-172">Apps run with the application identity for all requests, using app pool or process identity.</span></span> <span data-ttu-id="9bfc8-173">Wenn Sie explizit eine Aktion im Auftrag eines Benutzers ausführen müssen, verwenden Sie `WindowsIdentity.RunImpersonated`.</span><span class="sxs-lookup"><span data-stu-id="9bfc8-173">If you need to explicitly perform an action on behalf of a user, use `WindowsIdentity.RunImpersonated`.</span></span> <span data-ttu-id="9bfc8-174">Eine einzelne Aktion in diesem Kontext ausgeführt, und schließen Sie dann den Kontext.</span><span class="sxs-lookup"><span data-stu-id="9bfc8-174">Run a single action in this context and then close the context.</span></span>

[!code-csharp[](windowsauth/sample/Startup.cs?name=snippet_Impersonate&highlight=10-18)]

<span data-ttu-id="9bfc8-175">Beachten Sie, dass `RunImpersonated` keine asynchrone Vorgänge unterstützt und sollte nicht für komplexe Szenarios verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="9bfc8-175">Note that `RunImpersonated` doesn't support asynchronous operations and shouldn't be used for complex scenarios.</span></span> <span data-ttu-id="9bfc8-176">Z. B. wird nicht wrapping gesamten Anfragen oder Middleware Ketten unterstützt oder empfohlen.</span><span class="sxs-lookup"><span data-stu-id="9bfc8-176">For example, wrapping entire requests or middleware chains isn't supported or recommended.</span></span>

---