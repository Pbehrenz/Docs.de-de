---
title: Tag-Hilfsprogramme in Formularen in ASP.NET Core
author: rick-anderson
description: Beschreibt die integrierte Tag Hilfsprogramme mit Forms verwendet.
keywords: ASP.NET Core, Tag-Hilfsprogramm taghelpers., HTML-Formular bildet.
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: 25595059-4fac-4785-8152-f88590e3169b
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/working-with-forms
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: da36985206521798d3bfe71f6372dc5cc4fca09a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
# <a name="introduction-to-using-tag-helpers-in-forms-in-aspnet-core"></a><span data-ttu-id="eb9db-104">Einführung in die Verwendung von Tag-Hilfsprogramme in Formularen in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="eb9db-104">Introduction to using tag helpers in forms in ASP.NET Core</span></span>

<span data-ttu-id="eb9db-105">Durch [Rick Anderson](https://twitter.com/RickAndMSFT), [Dave Paquette](https://twitter.com/Dave_Paquette), und [Jerrie Pelser](https://github.com/jerriep)</span><span class="sxs-lookup"><span data-stu-id="eb9db-105">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Dave Paquette](https://twitter.com/Dave_Paquette), and [Jerrie Pelser](https://github.com/jerriep)</span></span>

<span data-ttu-id="eb9db-106">Dieses Dokument veranschaulicht, arbeiten mit Formularen und die HTML-Elemente, die häufig in einem Formular verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="eb9db-106">This document demonstrates working with Forms and the HTML elements commonly used on a Form.</span></span> <span data-ttu-id="eb9db-107">Der HTML-Code [Formular](https://www.w3.org/TR/html401/interact/forms.html) -Element stellt die primäre Mechanismus Web-apps verwenden zum Bereitstellen von Daten an den Server bereit.</span><span class="sxs-lookup"><span data-stu-id="eb9db-107">The HTML [Form](https://www.w3.org/TR/html401/interact/forms.html) element provides the primary mechanism web apps use to post back data to the server.</span></span> <span data-ttu-id="eb9db-108">Die meisten dieses Dokuments beschreibt [Tag Hilfsprogramme](tag-helpers/intro.md) und wie sie produktiv Erstellung robustere HTML-Formularen helfen können.</span><span class="sxs-lookup"><span data-stu-id="eb9db-108">Most of this document describes [Tag Helpers](tag-helpers/intro.md) and how they can help you productively create robust HTML forms.</span></span> <span data-ttu-id="eb9db-109">Sie sollten [Einführung in die Tag-Hilfsprogrammen](tag-helpers/intro.md) , bevor Sie dieses Dokument zu lesen.</span><span class="sxs-lookup"><span data-stu-id="eb9db-109">We recommend you read [Introduction to Tag Helpers](tag-helpers/intro.md) before you read this document.</span></span>

<span data-ttu-id="eb9db-110">In vielen Fällen HTML-Hilfsmethoden bieten eine alternative Methode in einer bestimmten Tag-Hilfsprogramm, aber es ist wichtig zu wissen, dass die Tag-Hilfsprogrammen HTML-Hilfsmethoden nicht ersetzen, und es ist keines Tag-Hilfsprogramms für jedes HTML-Hilfsobjekt.</span><span class="sxs-lookup"><span data-stu-id="eb9db-110">In many cases, HTML Helpers provide an alternative approach to a specific Tag Helper, but it's important to recognize that Tag Helpers do not replace HTML Helpers and there is not a Tag Helper for each HTML Helper.</span></span> <span data-ttu-id="eb9db-111">Wenn eine HTML-Hilfsobjekt Alternative vorhanden ist, wird er erwähnt.</span><span class="sxs-lookup"><span data-stu-id="eb9db-111">When an HTML Helper alternative exists, it is mentioned.</span></span>

<a name="my-asp-route-param-ref-label"></a>

## <a name="the-form-tag-helper"></a><span data-ttu-id="eb9db-112">Das Form-Tag-Hilfsprogramm</span><span class="sxs-lookup"><span data-stu-id="eb9db-112">The Form Tag Helper</span></span>

<span data-ttu-id="eb9db-113">Die [Formular](https://www.w3.org/TR/html401/interact/forms.html) Helper kennzeichnen:</span><span class="sxs-lookup"><span data-stu-id="eb9db-113">The [Form](https://www.w3.org/TR/html401/interact/forms.html) Tag Helper:</span></span>

* <span data-ttu-id="eb9db-114">Den HTML-Code generiert [ \<Formular >](https://www.w3.org/TR/html401/interact/forms.html) `action` Attributwert für ein MVC-Controller-Aktion oder eine benannte Route</span><span class="sxs-lookup"><span data-stu-id="eb9db-114">Generates the HTML [\<FORM>](https://www.w3.org/TR/html401/interact/forms.html) `action` attribute value for a MVC controller action or named route</span></span>

* <span data-ttu-id="eb9db-115">Generiert ein ausgeblendetes [Anforderung Überprüfung Token](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) um websiteübergreifende anforderungsfälschung zu verhindern (bei Verwendung mit der `[ValidateAntiForgeryToken]` Attribut in der Aktionsmethode HTTP Post)</span><span class="sxs-lookup"><span data-stu-id="eb9db-115">Generates a hidden [Request Verification Token](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) to prevent cross-site request forgery (when used with the `[ValidateAntiForgeryToken]` attribute in the HTTP Post action method)</span></span>

* <span data-ttu-id="eb9db-116">Stellt die `asp-route-<Parameter Name>` -Attribut, auf dem `<Parameter Name>` die Routenwerte hinzugefügt wird.</span><span class="sxs-lookup"><span data-stu-id="eb9db-116">Provides the `asp-route-<Parameter Name>` attribute, where `<Parameter Name>` is added to the route values.</span></span> <span data-ttu-id="eb9db-117">Die `routeValues` Parameter `Html.BeginForm` und `Html.BeginRouteForm` bieten eine ähnliche Funktionalität.</span><span class="sxs-lookup"><span data-stu-id="eb9db-117">The  `routeValues` parameters to `Html.BeginForm` and `Html.BeginRouteForm` provide similar functionality.</span></span>

* <span data-ttu-id="eb9db-118">Verfügt über Alternative HTML-Hilfsobjekt `Html.BeginForm` und`Html.BeginRouteForm`</span><span class="sxs-lookup"><span data-stu-id="eb9db-118">Has an HTML Helper alternative `Html.BeginForm` and `Html.BeginRouteForm`</span></span>

<span data-ttu-id="eb9db-119">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="eb9db-119">Sample:</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/RegisterFormOnly.cshtml)]

<span data-ttu-id="eb9db-120">Das Form-Tag-Hilfsobjekt oben wird der folgenden HTML-Code generiert:</span><span class="sxs-lookup"><span data-stu-id="eb9db-120">The Form Tag Helper above generates the following HTML:</span></span>

```HTML
<form method="post" action="/Demo/Register">
     <!-- Input and Submit elements -->
     <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
    </form>
```

<span data-ttu-id="eb9db-121">Der MVC-Laufzeit generiert die `action` -Attributwert aus der Form-Tag-Helper-Attribute `asp-controller` und `asp-action`.</span><span class="sxs-lookup"><span data-stu-id="eb9db-121">The MVC runtime generates the `action` attribute value from the Form Tag Helper attributes `asp-controller` and `asp-action`.</span></span> <span data-ttu-id="eb9db-122">Das Form-Tag-Hilfsprogramm generiert außerdem ein ausgeblendetes [Anforderung Überprüfung Token](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) um websiteübergreifende anforderungsfälschung zu verhindern (bei Verwendung mit der `[ValidateAntiForgeryToken]` Attribut in der Aktionsmethode HTTP Post).</span><span class="sxs-lookup"><span data-stu-id="eb9db-122">The Form Tag Helper also generates a hidden [Request Verification Token](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) to prevent cross-site request forgery (when used with the `[ValidateAntiForgeryToken]` attribute in the HTTP Post action method).</span></span> <span data-ttu-id="eb9db-123">Schützen ein reines HTML-Formular aus websiteübergreifende anforderungsfälschung ist schwierig, die Form-Tag-Hilfsprogramm stellt diesen Dienst für Sie.</span><span class="sxs-lookup"><span data-stu-id="eb9db-123">Protecting a pure HTML Form from cross-site request forgery is difficult, the Form Tag Helper provides this service for you.</span></span>

### <a name="using-a-named-route"></a><span data-ttu-id="eb9db-124">Verwenden eine benannte route</span><span class="sxs-lookup"><span data-stu-id="eb9db-124">Using a named route</span></span>

<span data-ttu-id="eb9db-125">Die `asp-route` Tag Helper-Attribut kann auch Markup für den HTML-Code generieren `action` Attribut.</span><span class="sxs-lookup"><span data-stu-id="eb9db-125">The `asp-route` Tag Helper attribute can also generate markup for the HTML `action` attribute.</span></span> <span data-ttu-id="eb9db-126">Eine Anwendung mit einem [Route](../../fundamentals/routing.md) mit dem Namen `register` konnte das folgende Markup für die Seite "Registrierung" verwenden:</span><span class="sxs-lookup"><span data-stu-id="eb9db-126">An app with a [route](../../fundamentals/routing.md)  named `register` could use the following markup for the registration page:</span></span>

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterRoute.cshtml)]

<span data-ttu-id="eb9db-127">Viele der Ansichten in der *Ansichten/Konto* Ordner (generiert, wenn Sie eine neue Web-app erstellen *einzelne Benutzerkonten*) enthalten die [Asp-Route-Returnurl](https://docs.microsoft.com/aspnet/core/mvc/views/working-with-forms) Attribut:</span><span class="sxs-lookup"><span data-stu-id="eb9db-127">Many of the views in the *Views/Account* folder (generated when you create a new web app with *Individual User Accounts*) contain the [asp-route-returnurl](https://docs.microsoft.com/aspnet/core/mvc/views/working-with-forms) attribute:</span></span>

```cshtml
<form asp-controller="Account" asp-action="Login"
     asp-route-returnurl="@ViewData["ReturnUrl"]"
     method="post" class="form-horizontal" role="form">
```

>[!NOTE]
><span data-ttu-id="eb9db-128">Mit den integrierten Vorlagen `returnUrl` wird nur automatisch aufgefüllt, wenn Sie versuchen, eine autorisierte Ressource zuzugreifen, aber nicht authentifiziert oder autorisiert.</span><span class="sxs-lookup"><span data-stu-id="eb9db-128">With the built in templates, `returnUrl` is only populated automatically when you try to access an authorized resource but are not authenticated or authorized.</span></span> <span data-ttu-id="eb9db-129">Beim Versuch, eines nicht autorisierten Zugriffs, leitet die Middleware Sicherheit Sie zur Anmeldeseite mit der `returnUrl` festgelegt.</span><span class="sxs-lookup"><span data-stu-id="eb9db-129">When you attempt an unauthorized access, the security middleware redirects you to the login page with the `returnUrl` set.</span></span>

## <a name="the-input-tag-helper"></a><span data-ttu-id="eb9db-130">Das Hilfsobjekt Eingabetag</span><span class="sxs-lookup"><span data-stu-id="eb9db-130">The Input Tag Helper</span></span>

<span data-ttu-id="eb9db-131">Das Eingabe-Tag-Hilfsobjekt bindet ein HTML [ \<input >](https://www.w3.org/wiki/HTML/Elements/input) Element zu einem Modell Ausdruck in der Razor-Ansicht.</span><span class="sxs-lookup"><span data-stu-id="eb9db-131">The Input Tag Helper binds an HTML [\<input>](https://www.w3.org/wiki/HTML/Elements/input) element to a model expression in your razor view.</span></span>

<span data-ttu-id="eb9db-132">Syntax:</span><span class="sxs-lookup"><span data-stu-id="eb9db-132">Syntax:</span></span>

```HTML
<input asp-for="<Expression Name>" />
```

<span data-ttu-id="eb9db-133">Das Hilfsobjekt Eingabetag:</span><span class="sxs-lookup"><span data-stu-id="eb9db-133">The Input Tag Helper:</span></span>

* <span data-ttu-id="eb9db-134">Generiert die `id` und `name` HTML-Attribute für den im angegebenen Ausdruck ein die `asp-for` Attribut.</span><span class="sxs-lookup"><span data-stu-id="eb9db-134">Generates the `id` and `name` HTML attributes for the expression name specified in the `asp-for` attribute.</span></span> <span data-ttu-id="eb9db-135">`asp-for="Property1.Property2"` entspricht `m => m.Property1.Property2`.</span><span class="sxs-lookup"><span data-stu-id="eb9db-135">`asp-for="Property1.Property2"` is equivalent to `m => m.Property1.Property2`.</span></span> <span data-ttu-id="eb9db-136">Der Name des Ausdrucks ist Verwendungszwecks der `asp-for` Attributwert.</span><span class="sxs-lookup"><span data-stu-id="eb9db-136">The name of the expression is what is used for the `asp-for` attribute value.</span></span> <span data-ttu-id="eb9db-137">Finden Sie unter der [Ausdrucksnamen](#expression-names) Abschnitt, um zusätzliche Informationen.</span><span class="sxs-lookup"><span data-stu-id="eb9db-137">See the [Expression names](#expression-names) section for additional information.</span></span>

* <span data-ttu-id="eb9db-138">Legt den HTML-Code `type` -Attributwert basierend auf den Typ des Modells und [-datenanmerkung](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) Attribute der Modelleigenschaft angewendet werden</span><span class="sxs-lookup"><span data-stu-id="eb9db-138">Sets the HTML `type` attribute value based on the model type and  [data annotation](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) attributes applied to the model property</span></span>

* <span data-ttu-id="eb9db-139">Überschreibt den HTML-Code keine `type` -Attributwert aus, wenn ein solcher festgelegt wurde</span><span class="sxs-lookup"><span data-stu-id="eb9db-139">Will not overwrite the HTML `type` attribute value when one is specified</span></span>

* <span data-ttu-id="eb9db-140">Generiert [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) Überprüfung Attributen von [-datenanmerkung](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) Attribute Modelleigenschaften angewendet werden</span><span class="sxs-lookup"><span data-stu-id="eb9db-140">Generates [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5)  validation attributes from [data annotation](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) attributes applied to model properties</span></span>

* <span data-ttu-id="eb9db-141">Verfügt über eine HTML-Hilfsobjekt-Funktion mit überschneiden `Html.TextBoxFor` und `Html.EditorFor`.</span><span class="sxs-lookup"><span data-stu-id="eb9db-141">Has an HTML Helper feature overlap with `Html.TextBoxFor` and `Html.EditorFor`.</span></span> <span data-ttu-id="eb9db-142">Finden Sie unter der **HTML-Hilfsobjekt Alternativen zur Eingabe Tag Helper** Abschnitt Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="eb9db-142">See the **HTML Helper alternatives to Input Tag Helper** section for details.</span></span>

* <span data-ttu-id="eb9db-143">Stellt die starke Typisierung.</span><span class="sxs-lookup"><span data-stu-id="eb9db-143">Provides strong typing.</span></span> <span data-ttu-id="eb9db-144">Wenn der Name der der Eigenschaft ändert und Sie die Tag-Hilfsprogramm nicht aktualisieren erhalten Sie eine Fehlermeldung ähnlich der folgenden:</span><span class="sxs-lookup"><span data-stu-id="eb9db-144">If the name of the property changes and you don't update the Tag Helper you'll get an error similar to the following:</span></span>

```HTML
An error occurred during the compilation of a resource required to process
this request. Please review the following specific error details and modify
your source code appropriately.

Type expected
 'RegisterViewModel' does not contain a definition for 'Email' and no
 extension method 'Email' accepting a first argument of type 'RegisterViewModel'
 could be found (are you missing a using directive or an assembly reference?)
```

<span data-ttu-id="eb9db-145">Die `Input` Tag Helper legt den HTML-Code `type` Attribut basierend auf dem .NET.</span><span class="sxs-lookup"><span data-stu-id="eb9db-145">The `Input` Tag Helper sets the HTML `type` attribute based on the .NET type.</span></span> <span data-ttu-id="eb9db-146">Die folgende Tabelle enthält einige allgemeine .NET-Typen und die generierten HTML-Typ (nicht in jedem Typ .NET wird aufgeführt).</span><span class="sxs-lookup"><span data-stu-id="eb9db-146">The following table lists some common .NET types and generated HTML type (not every .NET type is listed).</span></span>

|<span data-ttu-id="eb9db-147">.NET-Typ</span><span class="sxs-lookup"><span data-stu-id="eb9db-147">.NET type</span></span>|<span data-ttu-id="eb9db-148">Eingabetyp</span><span class="sxs-lookup"><span data-stu-id="eb9db-148">Input Type</span></span>|
|---|---|
|<span data-ttu-id="eb9db-149">Bool</span><span class="sxs-lookup"><span data-stu-id="eb9db-149">Bool</span></span>|<span data-ttu-id="eb9db-150">Typ = "Checkbox"</span><span class="sxs-lookup"><span data-stu-id="eb9db-150">type=”checkbox”</span></span>|
|<span data-ttu-id="eb9db-151">Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="eb9db-151">String</span></span>|<span data-ttu-id="eb9db-152">Typ = "Text"</span><span class="sxs-lookup"><span data-stu-id="eb9db-152">type=”text”</span></span>|
|<span data-ttu-id="eb9db-153">DateTime</span><span class="sxs-lookup"><span data-stu-id="eb9db-153">DateTime</span></span>|<span data-ttu-id="eb9db-154">Typ "Datetime" =</span><span class="sxs-lookup"><span data-stu-id="eb9db-154">type=”datetime”</span></span>|
|<span data-ttu-id="eb9db-155">Byte</span><span class="sxs-lookup"><span data-stu-id="eb9db-155">Byte</span></span>|<span data-ttu-id="eb9db-156">Typ = "Number"</span><span class="sxs-lookup"><span data-stu-id="eb9db-156">type=”number”</span></span>|
|<span data-ttu-id="eb9db-157">Int</span><span class="sxs-lookup"><span data-stu-id="eb9db-157">Int</span></span>|<span data-ttu-id="eb9db-158">Typ = "Number"</span><span class="sxs-lookup"><span data-stu-id="eb9db-158">type=”number”</span></span>|
|<span data-ttu-id="eb9db-159">Single, Double</span><span class="sxs-lookup"><span data-stu-id="eb9db-159">Single, Double</span></span>|<span data-ttu-id="eb9db-160">Typ = "Number"</span><span class="sxs-lookup"><span data-stu-id="eb9db-160">type=”number”</span></span>|


<span data-ttu-id="eb9db-161">Die folgende Tabelle zeigt einige häufige [datenanmerkungen](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) Attribute, die das Hilfsprogramm Eingabetag bestimmte Eingabetypen zugeordnet werden kann (nicht jedes Validierungsattribut wird aufgeführt):</span><span class="sxs-lookup"><span data-stu-id="eb9db-161">The following table shows some common [data annotations](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) attributes that the input tag helper will map to specific input types (not every validation attribute is listed):</span></span>


|<span data-ttu-id="eb9db-162">Attribut</span><span class="sxs-lookup"><span data-stu-id="eb9db-162">Attribute</span></span>|<span data-ttu-id="eb9db-163">Eingabetyp</span><span class="sxs-lookup"><span data-stu-id="eb9db-163">Input Type</span></span>|
|---|---|
|<span data-ttu-id="eb9db-164">[EmailAddress]</span><span class="sxs-lookup"><span data-stu-id="eb9db-164">[EmailAddress]</span></span>|<span data-ttu-id="eb9db-165">Typ = "Email"</span><span class="sxs-lookup"><span data-stu-id="eb9db-165">type=”email”</span></span>|
|<span data-ttu-id="eb9db-166">[Url]</span><span class="sxs-lookup"><span data-stu-id="eb9db-166">[Url]</span></span>|<span data-ttu-id="eb9db-167">Typ = "Url"</span><span class="sxs-lookup"><span data-stu-id="eb9db-167">type=”url”</span></span>|
|<span data-ttu-id="eb9db-168">[HiddenInput]</span><span class="sxs-lookup"><span data-stu-id="eb9db-168">[HiddenInput]</span></span>|<span data-ttu-id="eb9db-169">Typ = "hidden"</span><span class="sxs-lookup"><span data-stu-id="eb9db-169">type=”hidden”</span></span>|
|<span data-ttu-id="eb9db-170">[Phone]</span><span class="sxs-lookup"><span data-stu-id="eb9db-170">[Phone]</span></span>|<span data-ttu-id="eb9db-171">Typ = "Tel."</span><span class="sxs-lookup"><span data-stu-id="eb9db-171">type=”tel”</span></span>|
|<span data-ttu-id="eb9db-172">[DataType(DataType.Password)]</span><span class="sxs-lookup"><span data-stu-id="eb9db-172">[DataType(DataType.Password)]</span></span>| <span data-ttu-id="eb9db-173">Typ = "Kennwort"</span><span class="sxs-lookup"><span data-stu-id="eb9db-173">type=”password”</span></span>|
|<span data-ttu-id="eb9db-174">[DataType(DataType.Date)]</span><span class="sxs-lookup"><span data-stu-id="eb9db-174">[DataType(DataType.Date)]</span></span>| <span data-ttu-id="eb9db-175">Typ "Date" =</span><span class="sxs-lookup"><span data-stu-id="eb9db-175">type=”date”</span></span>|
|<span data-ttu-id="eb9db-176">[DataType(DataType.Time)]</span><span class="sxs-lookup"><span data-stu-id="eb9db-176">[DataType(DataType.Time)]</span></span>| <span data-ttu-id="eb9db-177">Typ "Time" =</span><span class="sxs-lookup"><span data-stu-id="eb9db-177">type=”time”</span></span>|


<span data-ttu-id="eb9db-178">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="eb9db-178">Sample:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/RegisterViewModel.cs)]

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/RegisterInput.cshtml)]

<span data-ttu-id="eb9db-179">Der obige Code generiert den folgenden HTML-Code:</span><span class="sxs-lookup"><span data-stu-id="eb9db-179">The code above generates the following HTML:</span></span>

```HTML
  <form method="post" action="/Demo/RegisterInput">
       Email:
       <input type="email" data-val="true"
              data-val-email="The Email Address field is not a valid e-mail address."
              data-val-required="The Email Address field is required."
              id="Email" name="Email" value="" /> <br>
       Password:
       <input type="password" data-val="true"
              data-val-required="The Password field is required."
              id="Password" name="Password" /><br>
       <button type="submit">Register</button>
     <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
   </form>
```

<span data-ttu-id="eb9db-180">Die datenanmerkungen angewendet, um die `Email` und `Password` Eigenschaften Metadaten für das Modell generieren.</span><span class="sxs-lookup"><span data-stu-id="eb9db-180">The data annotations applied to the `Email` and `Password` properties generate metadata on the model.</span></span> <span data-ttu-id="eb9db-181">Das Eingabe-Tag-Hilfsobjekt nutzt die darin enthaltenen Modellmetadaten und erzeugt [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-val-*` Attribute (finden Sie unter [Modellvalidierung](../models/validation.md)).</span><span class="sxs-lookup"><span data-stu-id="eb9db-181">The Input Tag Helper consumes the model metadata and produces [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-val-*` attributes (see [Model Validation](../models/validation.md)).</span></span> <span data-ttu-id="eb9db-182">Diese Attribute beschreiben die Validierungssteuerelemente für die Verbindung zu den Eingabefeldern.</span><span class="sxs-lookup"><span data-stu-id="eb9db-182">These attributes describe the validators to attach to the input fields.</span></span> <span data-ttu-id="eb9db-183">Dies bietet unaufdringlichen HTML5 und [jQuery](https://jquery.com/) Überprüfung.</span><span class="sxs-lookup"><span data-stu-id="eb9db-183">This provides unobtrusive HTML5 and [jQuery](https://jquery.com/) validation.</span></span> <span data-ttu-id="eb9db-184">Die unaufdringlichen Attribute haben das Format `data-val-rule="Error Message"`, wobei der Regel der Name einer Validierungsregel ist (z. B. `data-val-required`, `data-val-email`, `data-val-maxlength`usw..) Wenn eine Fehlermeldung im Attribut bereitgestellt wird, wird angezeigt als Wert für die `data-val-rule` Attribut.</span><span class="sxs-lookup"><span data-stu-id="eb9db-184">The unobtrusive attributes have the format `data-val-rule="Error Message"`, where rule is the name of the validation rule (such as `data-val-required`, `data-val-email`, `data-val-maxlength`, etc.) If an error message is provided in the attribute, it is displayed as the value for the `data-val-rule` attribute.</span></span> <span data-ttu-id="eb9db-185">Es gibt auch Attribute des Formulars `data-val-ruleName-argumentName="argumentValue"` , die bieten weitere Details über die Regel ein, z. B. `data-val-maxlength-max="1024"` .</span><span class="sxs-lookup"><span data-stu-id="eb9db-185">There are also attributes of the form `data-val-ruleName-argumentName="argumentValue"` that provide additional details about the rule, for example, `data-val-maxlength-max="1024"` .</span></span>

### <a name="html-helper-alternatives-to-input-tag-helper"></a><span data-ttu-id="eb9db-186">HTML-Hilfsobjekt Alternativen zur Eingabe Tag-Hilfsprogramm</span><span class="sxs-lookup"><span data-stu-id="eb9db-186">HTML Helper alternatives to Input Tag Helper</span></span>

<span data-ttu-id="eb9db-187">`Html.TextBox`, `Html.TextBoxFor`, `Html.Editor` und `Html.EditorFor` überlappenden Funktionen mit der Eingabe-Tag-Hilfsmethode.</span><span class="sxs-lookup"><span data-stu-id="eb9db-187">`Html.TextBox`, `Html.TextBoxFor`, `Html.Editor` and `Html.EditorFor` have overlapping features with the Input Tag Helper.</span></span> <span data-ttu-id="eb9db-188">Das Eingabe-Tag-Hilfsobjekt werden automatisch festgelegt, die `type` -Attribut; `Html.TextBox` und `Html.TextBoxFor` geschieht dies nicht.</span><span class="sxs-lookup"><span data-stu-id="eb9db-188">The Input Tag Helper will automatically set the `type` attribute; `Html.TextBox` and `Html.TextBoxFor` will not.</span></span> <span data-ttu-id="eb9db-189">`Html.Editor`und `Html.EditorFor` behandeln, Sammlungen, komplexe Objekte und Vorlagen; die Eingabe-Tag-Hilfsprogramm nicht der Fall.</span><span class="sxs-lookup"><span data-stu-id="eb9db-189">`Html.Editor` and `Html.EditorFor` handle collections, complex objects and templates; the Input Tag Helper does not.</span></span> <span data-ttu-id="eb9db-190">Das Eingabe-Tag-Hilfsobjekt `Html.EditorFor` und `Html.TextBoxFor` sind stark typisiert (sie verwenden ein Lambda-Ausdrücke); `Html.TextBox` und `Html.Editor` nicht sind (sie verwenden die Namen von Gruppierungsausdrücken).</span><span class="sxs-lookup"><span data-stu-id="eb9db-190">The Input Tag Helper, `Html.EditorFor`  and  `Html.TextBoxFor` are strongly typed (they use lambda expressions); `Html.TextBox` and `Html.Editor` are not (they use expression names).</span></span>

### <a name="htmlattributes"></a><span data-ttu-id="eb9db-191">HtmlAttributes</span><span class="sxs-lookup"><span data-stu-id="eb9db-191">HtmlAttributes</span></span>

<span data-ttu-id="eb9db-192">`@Html.Editor()`und `@Html.EditorFor()` mithilfe eines speziellen `ViewDataDictionary` Eintrag namens `htmlAttributes` beim Ausführen ihrer Standardvorlagen.</span><span class="sxs-lookup"><span data-stu-id="eb9db-192">`@Html.Editor()` and `@Html.EditorFor()` use a special `ViewDataDictionary` entry named `htmlAttributes` when executing their default templates.</span></span> <span data-ttu-id="eb9db-193">Dieses Verhalten wird optional erweitert mit `additionalViewData` Parameter.</span><span class="sxs-lookup"><span data-stu-id="eb9db-193">This behavior is optionally augmented using `additionalViewData` parameters.</span></span> <span data-ttu-id="eb9db-194">Der Schlüssel "HtmlAttributes" ist die Groß-/Kleinschreibung.</span><span class="sxs-lookup"><span data-stu-id="eb9db-194">The key "htmlAttributes" is case-insensitive.</span></span> <span data-ttu-id="eb9db-195">Der Schlüssel "HtmlAttributes" erfolgt ähnlich wie die `htmlAttributes` -Objekt übergeben, für die Eingabe von Hilfsprogrammen wie `@Html.TextBox()`.</span><span class="sxs-lookup"><span data-stu-id="eb9db-195">The key "htmlAttributes" is handled similarly to the `htmlAttributes` object passed to input helpers like `@Html.TextBox()`.</span></span>

```HTML
@Html.EditorFor(model => model.YourProperty, 
  new { htmlAttributes = new { @class="myCssClass", style="Width:100px" } })
```

### <a name="expression-names"></a><span data-ttu-id="eb9db-196">Ausdrucksnamen</span><span class="sxs-lookup"><span data-stu-id="eb9db-196">Expression names</span></span>

<span data-ttu-id="eb9db-197">Die `asp-for` Attributwert ist eine `ModelExpression` und der rechten Seite eines Lambda-Ausdrucks.</span><span class="sxs-lookup"><span data-stu-id="eb9db-197">The `asp-for` attribute value is a `ModelExpression` and the right hand side of a lambda expression.</span></span> <span data-ttu-id="eb9db-198">Aus diesem Grund `asp-for="Property1"` wird `m => m.Property1` im generierten Code, daher wird, Sie müssen nicht mit dem Präfix `Model`.</span><span class="sxs-lookup"><span data-stu-id="eb9db-198">Therefore, `asp-for="Property1"` becomes `m => m.Property1` in the generated code which is why you don't need to prefix with `Model`.</span></span> <span data-ttu-id="eb9db-199">Können Sie das "@" Zeichen zu starten, einen Inlineausdruck vor dem Verschieben der `m.`:</span><span class="sxs-lookup"><span data-stu-id="eb9db-199">You can use the "@" character to start an inline expression and move before the `m.`:</span></span>

```HTML
@{
       var joe = "Joe";
   }
   <input asp-for="@joe" />
```

<span data-ttu-id="eb9db-200">Wird Folgendes generiert:</span><span class="sxs-lookup"><span data-stu-id="eb9db-200">Generates the following:</span></span>

```HTML
<input type="text" id="joe" name="joe" value="Joe" />
```

<span data-ttu-id="eb9db-201">Mit Auflistungseigenschaften `asp-for="CollectionProperty[23].Member"` generiert den gleichen Namen wie `asp-for="CollectionProperty[i].Member"` Wenn `i` hat den Wert `23`.</span><span class="sxs-lookup"><span data-stu-id="eb9db-201">With collection properties, `asp-for="CollectionProperty[23].Member"` generates the same name as `asp-for="CollectionProperty[i].Member"` when `i` has the value `23`.</span></span>

### <a name="navigating-child-properties"></a><span data-ttu-id="eb9db-202">Navigieren in der untergeordneten Eigenschaften</span><span class="sxs-lookup"><span data-stu-id="eb9db-202">Navigating child properties</span></span>

<span data-ttu-id="eb9db-203">Sie können auch auf die untergeordneten Eigenschaften, die mithilfe der Eigenschaftenpfad des Modells anzeigen navigieren.</span><span class="sxs-lookup"><span data-stu-id="eb9db-203">You can also navigate to child properties using the property path of the view model.</span></span> <span data-ttu-id="eb9db-204">Betrachten Sie eine komplexere Modellklasse, die ein untergeordnetes Element enthält `Address` Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="eb9db-204">Consider a more complex model class that contains a child `Address` property.</span></span>

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/AddressViewModel.cs?highlight=1,2,3,4&range=5-8)]

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/RegisterAddressViewModel.cs?highlight=8&range=5-13)]

<span data-ttu-id="eb9db-205">In der Ansicht der Bindung an `Address.AddressLine1`:</span><span class="sxs-lookup"><span data-stu-id="eb9db-205">In the view, we bind to `Address.AddressLine1`:</span></span>

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterAddress.cshtml?highlight=6)]

<span data-ttu-id="eb9db-206">Der folgende HTML-Code wird generiert, für `Address.AddressLine1`:</span><span class="sxs-lookup"><span data-stu-id="eb9db-206">The following HTML is generated for `Address.AddressLine1`:</span></span>

```HTML
<input type="text" id="Address_AddressLine1" name="Address.AddressLine1" value="" />
```

### <a name="expression-names-and-collections"></a><span data-ttu-id="eb9db-207">Ausdrucksnamen und Sammlungen</span><span class="sxs-lookup"><span data-stu-id="eb9db-207">Expression names and Collections</span></span>

<span data-ttu-id="eb9db-208">Beispiel für ein Modell, das ein Array von `Colors`:</span><span class="sxs-lookup"><span data-stu-id="eb9db-208">Sample, a model containing an array of `Colors`:</span></span>

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/Person.cs?highlight=3&range=5-10)]

<span data-ttu-id="eb9db-209">Der Action-Methode:</span><span class="sxs-lookup"><span data-stu-id="eb9db-209">The action method:</span></span>

```csharp
public IActionResult Edit(int id, int colorIndex)
   {
       ViewData["Index"] = colorIndex;
       return View(GetPerson(id));
   }
```

<span data-ttu-id="eb9db-210">Die folgenden Razor wird gezeigt, wie Sie einen bestimmten zugreifen `Color` Element:</span><span class="sxs-lookup"><span data-stu-id="eb9db-210">The following Razor shows how you access a specific `Color` element:</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/EditColor.cshtml)]

<span data-ttu-id="eb9db-211">Die *Views/Shared/EditorTemplates/String.cshtml* Vorlage:</span><span class="sxs-lookup"><span data-stu-id="eb9db-211">The *Views/Shared/EditorTemplates/String.cshtml* template:</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Shared/EditorTemplates/String.cshtml)]

<span data-ttu-id="eb9db-212">-Beispiels mit `List<T>`:</span><span class="sxs-lookup"><span data-stu-id="eb9db-212">Sample using `List<T>`:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/ToDoItem.cs?range=3-8)]

<span data-ttu-id="eb9db-213">Die folgenden Razor zeigt, wie eine Auflistung durchlaufen:</span><span class="sxs-lookup"><span data-stu-id="eb9db-213">The following Razor shows how to iterate over a collection:</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/Edit.cshtml)]

<span data-ttu-id="eb9db-214">Die *Views/Shared/EditorTemplates/ToDoItem.cshtml* Vorlage:</span><span class="sxs-lookup"><span data-stu-id="eb9db-214">The *Views/Shared/EditorTemplates/ToDoItem.cshtml* template:</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Shared/EditorTemplates/ToDoItem.cshtml)]


>[!NOTE]
><span data-ttu-id="eb9db-215">Verwenden Sie immer `for` (und *nicht* `foreach`) eine Liste durchlaufen.</span><span class="sxs-lookup"><span data-stu-id="eb9db-215">Always use `for` (and *not* `foreach`) to iterate over a list.</span></span> <span data-ttu-id="eb9db-216">Beim Auswerten eines Indexers in einer LINQ Ausdruck kann teuer sein und sollte minimiert werden.</span><span class="sxs-lookup"><span data-stu-id="eb9db-216">Evaluating an indexer in a LINQ expression can be expensive and should be minimized.</span></span>

&nbsp;

>[!NOTE]
><span data-ttu-id="eb9db-217">Kommentierte Beispielcode oben beschriebene wird gezeigt, wie Sie den Lambdaausdruck mit ersetzen würde die `@` Operator auf jede `ToDoItem` in der Liste.</span><span class="sxs-lookup"><span data-stu-id="eb9db-217">The commented sample code above shows how you would replace the lambda expression with the `@` operator to access each `ToDoItem` in the list.</span></span>

## <a name="the-textarea-tag-helper"></a><span data-ttu-id="eb9db-218">Das Textarea-Tag-Hilfsprogramm</span><span class="sxs-lookup"><span data-stu-id="eb9db-218">The Textarea Tag Helper</span></span>

<span data-ttu-id="eb9db-219">Die `Textarea Tag Helper` Tag Helper ist vergleichbar mit der Eingabe-Tag-Hilfsmethode.</span><span class="sxs-lookup"><span data-stu-id="eb9db-219">The `Textarea Tag Helper` tag helper is  similar to the Input Tag Helper.</span></span>

* <span data-ttu-id="eb9db-220">Generiert die `id` und `name` Attribute und die überprüfungsattribute Daten aus dem Modell für eine [ \<Textarea >](https://www.w3.org/wiki/HTML/Elements/textarea) Element.</span><span class="sxs-lookup"><span data-stu-id="eb9db-220">Generates the `id` and `name` attributes, and the data validation attributes from the model for a [\<textarea>](https://www.w3.org/wiki/HTML/Elements/textarea) element.</span></span>

* <span data-ttu-id="eb9db-221">Stellt die starke Typisierung.</span><span class="sxs-lookup"><span data-stu-id="eb9db-221">Provides strong typing.</span></span>

* <span data-ttu-id="eb9db-222">HTML-Hilfsobjekt Alternative:`Html.TextAreaFor`</span><span class="sxs-lookup"><span data-stu-id="eb9db-222">HTML Helper alternative: `Html.TextAreaFor`</span></span>

<span data-ttu-id="eb9db-223">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="eb9db-223">Sample:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/DescriptionViewModel.cs)]

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterTextArea.cshtml?highlight=4)]

<span data-ttu-id="eb9db-224">Der folgende HTML-Code wird generiert:</span><span class="sxs-lookup"><span data-stu-id="eb9db-224">The following HTML is generated:</span></span>

```HTML
<form method="post" action="/Demo/RegisterTextArea">
  <textarea data-val="true"
   data-val-maxlength="The field Description must be a string or array type with a maximum length of &#x27;1024&#x27;."
   data-val-maxlength-max="1024"
   data-val-minlength="The field Description must be a string or array type with a minimum length of &#x27;5&#x27;."
   data-val-minlength-min="5"
   id="Description" name="Description">
  </textarea>
  <button type="submit">Test</button>
  <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
</form>
```

## <a name="the-label-tag-helper"></a><span data-ttu-id="eb9db-225">Das Label-Tag-Hilfsprogramm</span><span class="sxs-lookup"><span data-stu-id="eb9db-225">The Label Tag Helper</span></span>

* <span data-ttu-id="eb9db-226">Die Beschriftung der Label-Komponente generiert und `for` -Attribut auf einen [ <label> ](https://www.w3.org/wiki/HTML/Elements/label) -Element für den Namen eines Ausdrucks</span><span class="sxs-lookup"><span data-stu-id="eb9db-226">Generates the label caption and `for` attribute on a [<label>](https://www.w3.org/wiki/HTML/Elements/label) element for an expression name</span></span>

* <span data-ttu-id="eb9db-227">Alternative HTML-Hilfsobjekt: `Html.LabelFor`.</span><span class="sxs-lookup"><span data-stu-id="eb9db-227">HTML Helper alternative: `Html.LabelFor`.</span></span>

<span data-ttu-id="eb9db-228">Die `Label Tag Helper` bietet die folgenden Vorteile über ein reines HTML Label-Element:</span><span class="sxs-lookup"><span data-stu-id="eb9db-228">The `Label Tag Helper`  provides the following benefits over a pure HTML label element:</span></span>

* <span data-ttu-id="eb9db-229">Sie erhalten automatisch den aussagekräftige Bezeichnung-Wert aus der `Display` Attribut.</span><span class="sxs-lookup"><span data-stu-id="eb9db-229">You automatically get the descriptive label value from the `Display` attribute.</span></span> <span data-ttu-id="eb9db-230">Verlauf der Zeit und die Kombination von kann sich der gewünschten Anzeigenamen ändern `Display` Attribut und Bezeichnung Tag Helper gelten die `Display` wird überall verwendet.</span><span class="sxs-lookup"><span data-stu-id="eb9db-230">The intended display name might change over time, and the combination of `Display` attribute and Label Tag Helper will apply the `Display` everywhere it's used.</span></span>

* <span data-ttu-id="eb9db-231">Weniger Markup im Quellcode</span><span class="sxs-lookup"><span data-stu-id="eb9db-231">Less markup in source code</span></span>

* <span data-ttu-id="eb9db-232">Starke Typisierung mit dem Modelleigenschaft.</span><span class="sxs-lookup"><span data-stu-id="eb9db-232">Strong typing with the model property.</span></span>

<span data-ttu-id="eb9db-233">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="eb9db-233">Sample:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/SimpleViewModel.cs)]

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterLabel.cshtml?highlight=4)]

<span data-ttu-id="eb9db-234">Der folgende HTML-Code wird generiert, für die `<label>` Element:</span><span class="sxs-lookup"><span data-stu-id="eb9db-234">The following HTML is generated for the `<label>` element:</span></span>

```HTML
<label for="Email">Email Address</label>
```

<span data-ttu-id="eb9db-235">Das Label-Tag-Hilfsobjekt generiert die `for` zugeordnete Attributwert von "Email", die ID ist der `<input>` Element.</span><span class="sxs-lookup"><span data-stu-id="eb9db-235">The Label Tag Helper generated the `for` attribute value of "Email", which is the ID associated with the `<input>` element.</span></span> <span data-ttu-id="eb9db-236">Die Tag-Hilfsprogramme generieren konsistent `id` und `for` Elemente, damit sie ordnungsgemäß zugeordnet werden können.</span><span class="sxs-lookup"><span data-stu-id="eb9db-236">The Tag Helpers generate consistent `id` and `for` elements so they can be correctly associated.</span></span> <span data-ttu-id="eb9db-237">Die Beschriftung in diesem Beispiel ergibt sich aus der `Display` Attribut.</span><span class="sxs-lookup"><span data-stu-id="eb9db-237">The caption in this sample comes from the `Display` attribute.</span></span> <span data-ttu-id="eb9db-238">Wenn das Modell hat nicht eine `Display` -Attribut, die Beschriftung wäre der Eigenschaftsname des Ausdrucks.</span><span class="sxs-lookup"><span data-stu-id="eb9db-238">If the model didn't contain a `Display` attribute, the caption would be the property name of the expression.</span></span>

## <a name="the-validation-tag-helpers"></a><span data-ttu-id="eb9db-239">Die Überprüfung Tag-Hilfsprogramme</span><span class="sxs-lookup"><span data-stu-id="eb9db-239">The Validation Tag Helpers</span></span>

<span data-ttu-id="eb9db-240">Es gibt zwei Überprüfung Tag Hilfsmethoden.</span><span class="sxs-lookup"><span data-stu-id="eb9db-240">There are two Validation Tag Helpers.</span></span> <span data-ttu-id="eb9db-241">Die `Validation Message Tag Helper` (dem zeigt einer validierungsmeldung an, für eine einzelne Eigenschaft für das Modell), und die `Validation Summary Tag Helper` (so wird eine Zusammenfassung der Überprüfungsfehler angezeigt wird).</span><span class="sxs-lookup"><span data-stu-id="eb9db-241">The `Validation Message Tag Helper` (which displays a validation message for a single property on your model), and the `Validation Summary Tag Helper` (which displays a summary of validation errors).</span></span> <span data-ttu-id="eb9db-242">Die `Input Tag Helper` HTML5 Client Side Validierungsattribute Elemente basierend auf Daten auf Ihren Modellklassen anmerkungsattribute Eingabespalten hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="eb9db-242">The `Input Tag Helper` adds HTML5 client side validation attributes to input elements based on data annotation attributes on your model classes.</span></span> <span data-ttu-id="eb9db-243">Überprüfung wird auch auf dem Server ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="eb9db-243">Validation is also performed on the server.</span></span> <span data-ttu-id="eb9db-244">Der Überprüfungshelfer Tag zeigt diese Fehlermeldungen an, wenn ein Validierungsfehler auftritt.</span><span class="sxs-lookup"><span data-stu-id="eb9db-244">The Validation Tag Helper displays these error messages when a validation error occurs.</span></span>

### <a name="the-validation-message-tag-helper"></a><span data-ttu-id="eb9db-245">Tag der Überprüfungshelfer-Nachricht</span><span class="sxs-lookup"><span data-stu-id="eb9db-245">The Validation Message Tag Helper</span></span>

* <span data-ttu-id="eb9db-246">Fügt der [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-valmsg-for="property"` -Attribut auf die [umfassen](https://developer.mozilla.org/docs/Web/HTML/Element/span) -Element, das die Überprüfungsfehlermeldungen auf das Eingabefeld der angegebenen Modelleigenschaft fügt.</span><span class="sxs-lookup"><span data-stu-id="eb9db-246">Adds the [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5)  `data-valmsg-for="property"` attribute to the [span](https://developer.mozilla.org/docs/Web/HTML/Element/span) element, which attaches the validation error messages on the input field of the specified model property.</span></span> <span data-ttu-id="eb9db-247">Wenn ein Client-Side-Überprüfungsfehler auftritt, [jQuery](https://jquery.com/) zeigt die Fehlermeldung in der `<span>` Element.</span><span class="sxs-lookup"><span data-stu-id="eb9db-247">When a client side validation error occurs, [jQuery](https://jquery.com/) displays the error message in the `<span>` element.</span></span>

* <span data-ttu-id="eb9db-248">Validierung findet auch auf dem Server.</span><span class="sxs-lookup"><span data-stu-id="eb9db-248">Validation also takes place on the server.</span></span> <span data-ttu-id="eb9db-249">Clients können JavaScript deaktiviert und eine Validierung kann nur auf der Serverseite ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="eb9db-249">Clients may have JavaScript disabled and some validation can only be done on the server side.</span></span>

* <span data-ttu-id="eb9db-250">HTML-Hilfsobjekt Alternative:`Html.ValidationMessageFor`</span><span class="sxs-lookup"><span data-stu-id="eb9db-250">HTML Helper alternative: `Html.ValidationMessageFor`</span></span>

<span data-ttu-id="eb9db-251">Die `Validation Message Tag Helper` wird verwendet, mit der `asp-validation-for` Attribut in einem HTML [umfassen](https://developer.mozilla.org/docs/Web/HTML/Element/span) Element.</span><span class="sxs-lookup"><span data-stu-id="eb9db-251">The `Validation Message Tag Helper`  is used with the `asp-validation-for` attribute on a HTML [span](https://developer.mozilla.org/docs/Web/HTML/Element/span) element.</span></span>

```HTML
<span asp-validation-for="Email"></span>
```

<span data-ttu-id="eb9db-252">Tag der Überprüfungshelfer-Nachricht wird der folgenden HTML-Code generiert:</span><span class="sxs-lookup"><span data-stu-id="eb9db-252">The Validation Message Tag Helper will generate the following HTML:</span></span>

```HTML
<span class="field-validation-valid"
  data-valmsg-for="Email"
  data-valmsg-replace="true"></span>
```

<span data-ttu-id="eb9db-253">Im Allgemeinen verwendet, die `Validation Message Tag Helper` nach einem `Input` Tag-Hilfsprogramm für die gleiche Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="eb9db-253">You generally use the `Validation Message Tag Helper`  after an `Input` Tag Helper for the same property.</span></span> <span data-ttu-id="eb9db-254">Auf diese Weise zeigt Validierungsfehlermeldungen in der Nähe der Eingabe, die den Fehler verursacht hat.</span><span class="sxs-lookup"><span data-stu-id="eb9db-254">Doing so displays any validation error messages near the input that caused the error.</span></span>

> [!NOTE]
> <span data-ttu-id="eb9db-255">Benötigen Sie eine Sicht mit der richtigen JavaScript-Code und [jQuery](https://jquery.com/) Skriptverweise für clientseitige Validierung eingerichtet.</span><span class="sxs-lookup"><span data-stu-id="eb9db-255">You must have a view with the correct JavaScript and [jQuery](https://jquery.com/) script references in place for client side validation.</span></span> <span data-ttu-id="eb9db-256">Finden Sie unter [Modellvalidierung](../models/validation.md) für Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="eb9db-256">See [Model Validation](../models/validation.md) for more information.</span></span>

<span data-ttu-id="eb9db-257">Bei der Überprüfung ein serverseitiger Fehler auftritt (z. B. Wenn Sie benutzerdefinierte Validierung oder die clientseitige Validierung deaktiviert ist), setzt MVC diese Fehlermeldung als Text der `<span>` Element.</span><span class="sxs-lookup"><span data-stu-id="eb9db-257">When a server side validation error occurs (for example when you have custom server side validation or client-side validation is disabled), MVC places that error message as the body of the `<span>` element.</span></span>

```HTML
<span class="field-validation-error" data-valmsg-for="Email"
            data-valmsg-replace="true">
   The Email Address field is required.
</span>
```

### <a name="the-validation-summary-tag-helper"></a><span data-ttu-id="eb9db-258">Der Überprüfungshelfer Summary (Tag)</span><span class="sxs-lookup"><span data-stu-id="eb9db-258">The Validation Summary Tag Helper</span></span>

* <span data-ttu-id="eb9db-259">Ziele `<div>` Elemente mit dem `asp-validation-summary` Attribut</span><span class="sxs-lookup"><span data-stu-id="eb9db-259">Targets `<div>` elements with the `asp-validation-summary` attribute</span></span>

* <span data-ttu-id="eb9db-260">HTML-Hilfsobjekt Alternative:`@Html.ValidationSummary`</span><span class="sxs-lookup"><span data-stu-id="eb9db-260">HTML Helper alternative: `@Html.ValidationSummary`</span></span>

<span data-ttu-id="eb9db-261">Die `Validation Summary Tag Helper` wird verwendet, um eine Zusammenfassung von validierungsmeldungen anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="eb9db-261">The `Validation Summary Tag Helper`  is used to display a summary of validation messages.</span></span> <span data-ttu-id="eb9db-262">Die `asp-validation-summary` Attributwert darf keines der folgenden sein:</span><span class="sxs-lookup"><span data-stu-id="eb9db-262">The `asp-validation-summary` attribute value can be any of the following:</span></span>

|<span data-ttu-id="eb9db-263">Zusammenfassung der ASP-Validierung</span><span class="sxs-lookup"><span data-stu-id="eb9db-263">asp-validation-summary</span></span>|<span data-ttu-id="eb9db-264">Validierungsmeldungen angezeigt</span><span class="sxs-lookup"><span data-stu-id="eb9db-264">Validation messages displayed</span></span>|
|--- |--- |
|<span data-ttu-id="eb9db-265">ValidationSummary.All</span><span class="sxs-lookup"><span data-stu-id="eb9db-265">ValidationSummary.All</span></span>|<span data-ttu-id="eb9db-266">Eigenschaft "und" Model-Ebene</span><span class="sxs-lookup"><span data-stu-id="eb9db-266">Property and model level</span></span>|
|<span data-ttu-id="eb9db-267">ValidationSummary.ModelOnly</span><span class="sxs-lookup"><span data-stu-id="eb9db-267">ValidationSummary.ModelOnly</span></span>|<span data-ttu-id="eb9db-268">Modell</span><span class="sxs-lookup"><span data-stu-id="eb9db-268">Model</span></span>|
|<span data-ttu-id="eb9db-269">ValidationSummary.None</span><span class="sxs-lookup"><span data-stu-id="eb9db-269">ValidationSummary.None</span></span>|<span data-ttu-id="eb9db-270">Keine</span><span class="sxs-lookup"><span data-stu-id="eb9db-270">None</span></span>|

### <a name="sample"></a><span data-ttu-id="eb9db-271">Beispiel</span><span class="sxs-lookup"><span data-stu-id="eb9db-271">Sample</span></span>

<span data-ttu-id="eb9db-272">Im folgenden Beispiel wird das Datenmodell mit versehenen `DataAnnotation` Attribute, die Validierungsfehlermeldungen generiert, auf die `<input>` Element.</span><span class="sxs-lookup"><span data-stu-id="eb9db-272">In the following example, the data model is decorated with `DataAnnotation` attributes, which generates validation error messages on the `<input>` element.</span></span>  <span data-ttu-id="eb9db-273">Tritt ein Validierungsfehler auf, wird das Tag Überprüfungshelfer die Fehlermeldung angezeigt:</span><span class="sxs-lookup"><span data-stu-id="eb9db-273">When a validation error occurs, the Validation Tag Helper displays the error message:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/RegisterViewModel.cs)]

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterValidation.cshtml?highlight=4,6,8&range=1-10)]

<span data-ttu-id="eb9db-274">Die generierten HTML-Codes (wenn das Modell gültig ist):</span><span class="sxs-lookup"><span data-stu-id="eb9db-274">The generated HTML (when the model is valid):</span></span>

```HTML
<form action="/DemoReg/Register" method="post">
  <div class="validation-summary-valid" data-valmsg-summary="true">
  <ul><li style="display:none"></li></ul></div>
  Email:  <input name="Email" id="Email" type="email" value=""
   data-val-required="The Email field is required."
   data-val-email="The Email field is not a valid e-mail address."
   data-val="true"> <br>
  <span class="field-validation-valid" data-valmsg-replace="true"
   data-valmsg-for="Email"></span><br>
  Password: <input name="Password" id="Password" type="password"
   data-val-required="The Password field is required." data-val="true"><br>
  <span class="field-validation-valid" data-valmsg-replace="true"
   data-valmsg-for="Password"></span><br>
  <button type="submit">Register</button>
  <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
</form>
```

## <a name="the-select-tag-helper"></a><span data-ttu-id="eb9db-275">Die Select-Tag-Hilfsprogramm</span><span class="sxs-lookup"><span data-stu-id="eb9db-275">The Select Tag Helper</span></span>

* <span data-ttu-id="eb9db-276">Generiert [wählen](https://www.w3.org/wiki/HTML/Elements/select) und zugehörigen [Option](https://www.w3.org/wiki/HTML/Elements/option) Elemente für die Eigenschaften des Modells.</span><span class="sxs-lookup"><span data-stu-id="eb9db-276">Generates [select](https://www.w3.org/wiki/HTML/Elements/select) and associated [option](https://www.w3.org/wiki/HTML/Elements/option) elements for properties of your model.</span></span>

* <span data-ttu-id="eb9db-277">Verfügt über Alternative HTML-Hilfsobjekt `Html.DropDownListFor` und`Html.ListBoxFor`</span><span class="sxs-lookup"><span data-stu-id="eb9db-277">Has an HTML Helper alternative `Html.DropDownListFor` and `Html.ListBoxFor`</span></span>

<span data-ttu-id="eb9db-278">Die `Select Tag Helper` `asp-for` gibt den Namen des Modell-Eigenschaft für die [wählen](https://www.w3.org/wiki/HTML/Elements/select) Element und `asp-items` gibt an, die [Option](https://www.w3.org/wiki/HTML/Elements/option) Elemente.</span><span class="sxs-lookup"><span data-stu-id="eb9db-278">The `Select Tag Helper` `asp-for` specifies the model property  name for the [select](https://www.w3.org/wiki/HTML/Elements/select) element  and `asp-items` specifies the [option](https://www.w3.org/wiki/HTML/Elements/option) elements.</span></span>  <span data-ttu-id="eb9db-279">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="eb9db-279">For example:</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Home/Index.cshtml?range=4)]

<span data-ttu-id="eb9db-280">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="eb9db-280">Sample:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryViewModel.cs)]

<span data-ttu-id="eb9db-281">Die `Index` Methode initialisiert die `CountryViewModel`des ausgewählten Landes festlegt und übergibt es an die `Index` anzeigen.</span><span class="sxs-lookup"><span data-stu-id="eb9db-281">The `Index` method initializes the `CountryViewModel`, sets the selected country and passes it to the `Index` view.</span></span>

[!code-csharp[Main](working-with-forms/sample/final/Controllers/HomeController.cs?range=114-119)]

<span data-ttu-id="eb9db-282">Die HTTP-POST `Index` Methode zeigt die Auswahl an:</span><span class="sxs-lookup"><span data-stu-id="eb9db-282">The HTTP POST `Index` method displays the selection:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/Controllers/HomeController.cs?range=15-27)]

<span data-ttu-id="eb9db-283">Die `Index` anzeigen:</span><span class="sxs-lookup"><span data-stu-id="eb9db-283">The `Index` view:</span></span>

[!code-cshtml[Main](working-with-forms/sample/final/Views/Home/Index.cshtml?highlight=4)]

<span data-ttu-id="eb9db-284">Die folgenden HTML-Code (mit "CA" ausgewählt) generiert:</span><span class="sxs-lookup"><span data-stu-id="eb9db-284">Which generates the following HTML (with "CA" selected):</span></span>

```html
<form method="post" action="/">
     <select id="Country" name="Country">
       <option value="MX">Mexico</option>
       <option selected="selected" value="CA">Canada</option>
       <option value="US">USA</option>
     </select>
       <br /><button type="submit">Register</button>
     <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
   </form>
```

> [!NOTE]
> <span data-ttu-id="eb9db-285">Es wird nicht empfohlen, mithilfe von `ViewBag` oder `ViewData` mit dem Tag-Helfer auswählen.</span><span class="sxs-lookup"><span data-stu-id="eb9db-285">We do not recommend using `ViewBag` or `ViewData` with the Select Tag Helper.</span></span> <span data-ttu-id="eb9db-286">Einem Ansichtsmodell ist unter Umständen stabiler MVC-Metadaten bereitstellen und in der Regel weniger problematisch.</span><span class="sxs-lookup"><span data-stu-id="eb9db-286">A view model is more robust at providing MVC metadata and generally less problematic.</span></span>

<span data-ttu-id="eb9db-287">Die `asp-for` Attributwert ist ein Sonderfall und erfordert keine `Model` als Präfix des Hilfsprogramm-Tag Attribute stimmen (z. B. `asp-items`)</span><span class="sxs-lookup"><span data-stu-id="eb9db-287">The `asp-for` attribute value is a special case and doesn't require a `Model` prefix, the other Tag Helper attributes do (such as `asp-items`)</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Home/Index.cshtml?range=4)]

### <a name="enum-binding"></a><span data-ttu-id="eb9db-288">Enum-Bindung</span><span class="sxs-lookup"><span data-stu-id="eb9db-288">Enum binding</span></span>

<span data-ttu-id="eb9db-289">Es ist häufig sinnvoll, verwenden Sie `<select>` mit einer `enum` Eigenschaft und generieren die `SelectListItem` Elemente aus der `enum` Werte.</span><span class="sxs-lookup"><span data-stu-id="eb9db-289">It's often convenient to use `<select>` with an `enum` property and generate the `SelectListItem` elements from the `enum` values.</span></span>

<span data-ttu-id="eb9db-290">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="eb9db-290">Sample:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryEnumViewModel.cs?range=3-7)]

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryEnum.cs)]

<span data-ttu-id="eb9db-291">Die `GetEnumSelectList` Methode generiert eine `SelectList` Objekt für eine Enumeration.</span><span class="sxs-lookup"><span data-stu-id="eb9db-291">The `GetEnumSelectList` method generates a `SelectList` object for an enum.</span></span>

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexEnum.cshtml?highlight=5)]

<span data-ttu-id="eb9db-292">Sie können die Enumeratorliste mit ergänzen die `Display` Attribut, um eine umfangreichere Benutzeroberfläche zu erhalten:</span><span class="sxs-lookup"><span data-stu-id="eb9db-292">You can decorate your enumerator list with the `Display` attribute to get a richer UI:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryEnum.cs?highlight=5,7)]

<span data-ttu-id="eb9db-293">Der folgende HTML-Code wird generiert:</span><span class="sxs-lookup"><span data-stu-id="eb9db-293">The following HTML is generated:</span></span>

```HTML
  <form method="post" action="/Home/IndexEnum">
         <select data-val="true" data-val-required="The EnumCountry field is required."
                 id="EnumCountry" name="EnumCountry">
             <option value="0">United Mexican States</option>
             <option value="1">United States of America</option>
             <option value="2">Canada</option>
             <option value="3">France</option>
             <option value="4">Germany</option>
             <option selected="selected" value="5">Spain</option>
         </select>
         <br /><button type="submit">Register</button>
         <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
    </form>
```

### <a name="option-group"></a><span data-ttu-id="eb9db-294">Optionsgruppe</span><span class="sxs-lookup"><span data-stu-id="eb9db-294">Option Group</span></span>

<span data-ttu-id="eb9db-295">Der HTML-Code [ \<Optgroup >](https://www.w3.org/wiki/HTML/Elements/optgroup) Element wird generiert, wenn das Ansichtsmodell, eine oder mehrere enthält `SelectListGroup` Objekte.</span><span class="sxs-lookup"><span data-stu-id="eb9db-295">The HTML  [\<optgroup>](https://www.w3.org/wiki/HTML/Elements/optgroup) element is generated when the view model contains one or more `SelectListGroup` objects.</span></span>

<span data-ttu-id="eb9db-296">Die `CountryViewModelGroup` Gruppen der `SelectListItem` Elemente in den Gruppen "North America" und "Europe":</span><span class="sxs-lookup"><span data-stu-id="eb9db-296">The `CountryViewModelGroup` groups the `SelectListItem` elements into the "North America" and "Europe" groups:</span></span>

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/CountryViewModelGroup.cs?highlight=5,6,14,20,26,32,38,44&range=6-56)]

<span data-ttu-id="eb9db-297">Die beiden Gruppen sind unten dargestellt:</span><span class="sxs-lookup"><span data-stu-id="eb9db-297">The two groups are shown below:</span></span>

![Beispiel für eine Option](working-with-forms/_static/grp.png)

<span data-ttu-id="eb9db-299">Die generierten HTML-Code:</span><span class="sxs-lookup"><span data-stu-id="eb9db-299">The generated HTML:</span></span>

```HTML
 <form method="post" action="/Home/IndexGroup">
      <select id="Country" name="Country">
          <optgroup label="North America">
              <option value="MEX">Mexico</option>
              <option value="CAN">Canada</option>
              <option value="US">USA</option>
          </optgroup>
          <optgroup label="Europe">
              <option value="FR">France</option>
              <option value="ES">Spain</option>
              <option value="DE">Germany</option>
          </optgroup>
      </select>
      <br /><button type="submit">Register</button>
      <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
 </form>
```

### <a name="multiple-select"></a><span data-ttu-id="eb9db-300">Wählen Sie mehrere</span><span class="sxs-lookup"><span data-stu-id="eb9db-300">Multiple select</span></span>

<span data-ttu-id="eb9db-301">Das Tag-Hilfsobjekt wählen generiert automatisch die [mehrere = "mehrere"](http://w3c.github.io/html-reference/select.html) Attribut, wenn die Eigenschaft in angegeben die `asp-for` -Attribut ist ein `IEnumerable`.</span><span class="sxs-lookup"><span data-stu-id="eb9db-301">The Select Tag Helper  will automatically generate the [multiple = "multiple"](http://w3c.github.io/html-reference/select.html)  attribute if the property specified in the `asp-for` attribute is an `IEnumerable`.</span></span> <span data-ttu-id="eb9db-302">Betrachten Sie z. B. die folgenden Modell:</span><span class="sxs-lookup"><span data-stu-id="eb9db-302">For example, given the following model:</span></span>

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/CountryViewModelIEnumerable.cs?highlight=6)]

<span data-ttu-id="eb9db-303">Mit der folgenden Ansicht:</span><span class="sxs-lookup"><span data-stu-id="eb9db-303">With the following view:</span></span>

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexMultiSelect.cshtml?highlight=4)]

<span data-ttu-id="eb9db-304">Generiert den folgenden HTML-Code:</span><span class="sxs-lookup"><span data-stu-id="eb9db-304">Generates the following HTML:</span></span>

```HTML
<form method="post" action="/Home/IndexMultiSelect">
    <select id="CountryCodes"
    multiple="multiple"
    name="CountryCodes"><option value="MX">Mexico</option>
<option value="CA">Canada</option>
<option value="US">USA</option>
<option value="FR">France</option>
<option value="ES">Spain</option>
<option value="DE">Germany</option>
</select>
    <br /><button type="submit">Register</button>
  <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
</form>
```

### <a name="no-selection"></a><span data-ttu-id="eb9db-305">Keine Auswahl getroffen wurde</span><span class="sxs-lookup"><span data-stu-id="eb9db-305">No selection</span></span>

<span data-ttu-id="eb9db-306">Wenn Sie selbst unter Verwendung der Option "nicht angegeben" auf mehreren Seiten gefunden haben, können Sie eine Vorlage, um zu vermeiden, wiederholen den HTML-Code erstellen:</span><span class="sxs-lookup"><span data-stu-id="eb9db-306">If you find yourself using the "not specified" option in multiple pages, you can create a template to eliminate repeating the HTML:</span></span>

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexEmptyTemplate.cshtml?highlight=5)]

<span data-ttu-id="eb9db-307">Die *Views/Shared/EditorTemplates/CountryViewModel.cshtml* Vorlage:</span><span class="sxs-lookup"><span data-stu-id="eb9db-307">The *Views/Shared/EditorTemplates/CountryViewModel.cshtml* template:</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Shared/EditorTemplates/CountryViewModel.cshtml)]

<span data-ttu-id="eb9db-308">Hinzufügen von HTML [ \<Option >](https://www.w3.org/wiki/HTML/Elements/option) Elemente gibt keine Beschränkung auf die *keine Auswahl* Fall.</span><span class="sxs-lookup"><span data-stu-id="eb9db-308">Adding HTML [\<option>](https://www.w3.org/wiki/HTML/Elements/option) elements is not limited to the *No selection* case.</span></span> <span data-ttu-id="eb9db-309">Die folgende Sicht und Aktion-Methode wird z. B. HTML ähnelt der obige Code generiert:</span><span class="sxs-lookup"><span data-stu-id="eb9db-309">For example, the following view and action method will generate HTML similar to the code above:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/Controllers/HomeController.cs?range=114-119)]

[!code-HTML[Main](working-with-forms/sample/final/Views/Home/IndexOption.cshtml)]

<span data-ttu-id="eb9db-310">Die richtige `<option>` Element ausgewählt werden (enthalten die `selected="selected"` Attribut) abhängig von der aktuellen `Country` Wert.</span><span class="sxs-lookup"><span data-stu-id="eb9db-310">The correct `<option>` element will be selected ( contain the `selected="selected"` attribute) depending on the current `Country` value.</span></span>

```HTML
 <form method="post" action="/Home/IndexEmpty">
      <select id="Country" name="Country">
          <option value="">&lt;none&gt;</option>
          <option value="MX">Mexico</option>
          <option value="CA" selected="selected">Canada</option>
          <option value="US">USA</option>
      </select>
      <br /><button type="submit">Register</button>
   <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
 </form>
 ```

## <a name="additional-resources"></a><span data-ttu-id="eb9db-311">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="eb9db-311">Additional Resources</span></span>

* [<span data-ttu-id="eb9db-312">Taghilfsprogramme</span><span class="sxs-lookup"><span data-stu-id="eb9db-312">Tag Helpers</span></span>](tag-helpers/intro.md)

* [<span data-ttu-id="eb9db-313">HTML-Formular-element</span><span class="sxs-lookup"><span data-stu-id="eb9db-313">HTML Form element</span></span>](https://www.w3.org/TR/html401/interact/forms.html)

* [<span data-ttu-id="eb9db-314">Überprüfung von Anforderungstoken</span><span class="sxs-lookup"><span data-stu-id="eb9db-314">Request Verification Token</span></span>](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages)

* [<span data-ttu-id="eb9db-315">Modellbindung</span><span class="sxs-lookup"><span data-stu-id="eb9db-315">Model Binding</span></span>](../models/model-binding.md)

* [<span data-ttu-id="eb9db-316">Modellvalidierung</span><span class="sxs-lookup"><span data-stu-id="eb9db-316">Model Validation</span></span>](../models/validation.md)

* [<span data-ttu-id="eb9db-317">datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="eb9db-317">data annotations</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter)

* <span data-ttu-id="eb9db-318">[Codeausschnitte für dieses Dokument](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/forms/sample).</span><span class="sxs-lookup"><span data-stu-id="eb9db-318">[Code snippets for this document](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/forms/sample).</span></span>