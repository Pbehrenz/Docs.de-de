---
title: Mit einem bestimmten Schema in ASP.NET Core autorisieren
author: rick-anderson
description: In diesem Artikel erläutert die Identität, die für ein bestimmtes Schema zu beschränken, bei der Arbeit mit mehrere Authentifizierungsmethoden.
ms.author: riande
ms.date: 10/12/2017
uid: security/authorization/limitingidentitybyscheme
ms.openlocfilehash: 231c664006ee7ff91f471aa8d16c1fd18dcbabb1
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278199"
---
# <a name="authorize-with-a-specific-scheme-in-aspnet-core"></a>Mit einem bestimmten Schema in ASP.NET Core autorisieren

In einigen Szenarien, z. B. Single Page Applications (SPAs) ist es üblich, dass mehrere Authentifizierungsmethoden verwendet. Beispielsweise kann die app für JavaScript-Anforderungen Cookie-basierte Authentifizierung, um sich anzumelden und JWT-trägertoken-Authentifizierung verwenden. In einigen Fällen kann die app mehrere Instanzen von einen authentifizierungshandler sein. Angenommen, wird zwei Cookie-Handler, in denen eine eine grundlegende Identität enthält, und eine erstellt, wenn ein Multi-Factor Authentication (MFA) ausgelöst wurde. MFA kann ausgelöst werden, da der Benutzer einen Vorgang angefordert, der zusätzliche Sicherheit erfordert.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Ein anderes Authentifizierungsschema wird mit dem Namen, wenn der Authentifizierungsdienst während der Authentifizierung konfiguriert ist. Zum Beispiel:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Code omitted for brevity

    services.AddAuthentication()
        .AddCookie(options => {
            options.LoginPath = "/Account/Unauthorized/";
            options.AccessDeniedPath = "/Account/Forbidden/";
        })
        .AddJwtBearer(options => {
            options.Audience = "http://localhost:5001/";
            options.Authority = "http://localhost:5000/";
        });
```

Im vorangehenden Code wurden zwei authentifizierungshandler hinzugefügt: eine für Cookies und eine für trägertoken.

>[!NOTE]
>Das Standardschema Angabe führt zu der `HttpContext.User` -Eigenschaft, die auf diese Identität festgelegt wird. Wenn dieses Verhalten nicht wünschen, deaktivieren Sie ihn durch Aufrufen der parameterlosen Formulars der `AddAuthentication`.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Authentifizierungsschemas werden mit dem Namen, wenn Authentifizierung Middlewares während der Authentifizierung konfiguriert sind. Zum Beispiel:

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
{
    // Code omitted for brevity

    app.UseCookieAuthentication(new CookieAuthenticationOptions()
    {
        AuthenticationScheme = "Cookie",
        LoginPath = "/Account/Unauthorized/",
        AccessDeniedPath = "/Account/Forbidden/",
        AutomaticAuthenticate = false
    });
    
    app.UseJwtBearerAuthentication(new JwtBearerOptions()
    {
        AuthenticationScheme = "Bearer",
        AutomaticAuthenticate = false,
        Audience = "http://localhost:5001/",
        Authority = "http://localhost:5000/",
        RequireHttpsMetadata = false
    });
```

Im vorangehenden Code wurden zwei Authentifizierung Middlewares hinzugefügt: eine für Cookies und eine für trägertoken.

>[!NOTE]
>Das Standardschema Angabe führt zu der `HttpContext.User` -Eigenschaft, die auf diese Identität festgelegt wird. Wenn dieses Verhalten nicht wünschen, deaktivieren Sie es durch Festlegen der `AuthenticationOptions.AutomaticAuthenticate` Eigenschaft `false`.

---

## <a name="selecting-the-scheme-with-the-authorize-attribute"></a>Wählen das Schema mit dem Authorize-Attribut

Die app zum Zeitpunkt der Autorisierung gibt an, dass der Handler verwendet werden. Wählen Sie den Handler mit dem die app durch Übergeben einer durch Trennzeichen getrennte Liste der Authentifizierungsschemas autorisieren wird `[Authorize]`. Die `[Authorize]` Attribut gibt an, das Authentifizierungsschema oder Schemas verwenden, unabhängig davon, ob ein Standardwert konfiguriert ist. Zum Beispiel:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
[Authorize(AuthenticationSchemes = AuthSchemes)]
public class MixedController : Controller
    // Requires the following imports:
    // using Microsoft.AspNetCore.Authentication.Cookies;
    // using Microsoft.AspNetCore.Authentication.JwtBearer;
    private const string AuthSchemes =
        CookieAuthenticationDefaults.AuthenticationScheme + "," +
        JwtBearerDefaults.AuthenticationScheme;
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
[Authorize(ActiveAuthenticationSchemes = AuthSchemes)]
public class MixedController : Controller
    // Requires the following imports:
    // using Microsoft.AspNetCore.Authentication.Cookies;
    // using Microsoft.AspNetCore.Authentication.JwtBearer;
    private const string AuthSchemes =
        CookieAuthenticationDefaults.AuthenticationScheme + "," +
        JwtBearerDefaults.AuthenticationScheme;
```

---

Im vorherigen Beispiel wird dem Cookie und den Träger-Handler ausgeführt und haben die Möglichkeit, zu erstellen, und fügen eine Identität für den aktuellen Benutzer. Führt durch die Angabe nur eines einzelnen Schemas, die entsprechenden Handler.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
[Authorize(AuthenticationSchemes = 
    JwtBearerDefaults.AuthenticationScheme)]
public class MixedController : Controller
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
[Authorize(ActiveAuthenticationSchemes = 
    JwtBearerDefaults.AuthenticationScheme)]
public class MixedController : Controller
```

---

Im vorangehenden Code ausgeführt wird nur der Handler mit dem Schema "Träger" aus. Cookie-basierte Identitäten werden ignoriert.

## <a name="selecting-the-scheme-with-policies"></a>Das Schema mit Richtlinien auswählen

Wenn Sie es vorziehen, geben Sie die gewünschten Schemas in [Richtlinie](xref:security/authorization/policies), Sie können festlegen, die `AuthenticationSchemes` Auflistung bei Ihrer Richtlinie hinzufügen:

```csharp
services.AddAuthorization(options =>
{
    options.AddPolicy("Over18", policy =>
    {
        policy.AuthenticationSchemes.Add(JwtBearerDefaults.AuthenticationScheme);
        policy.RequireAuthenticatedUser();
        policy.Requirements.Add(new MinimumAgeRequirement());
    });
});
```

Im vorherigen Beispiel wird die Richtlinie "Over18" nur ausgeführt, mit der Identität, die vom Handler "Träger" erstellt werden. Verwenden Sie die Richtlinie durch Festlegen der `[Authorize]` des Attributs `Policy` Eigenschaft:

```csharp
[Authorize(Policy = "Over18")]
public class RegistrationController : Controller
```
