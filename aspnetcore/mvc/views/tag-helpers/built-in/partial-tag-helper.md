---
title: "Pomocnik częściowe tagu w platformy ASP.NET Core"
author: scottaddie
description: "Dostęp do platformy ASP.NET Core częściowe Tag pomocnika i roli każdego z jego atrybuty odtwarzana renderowania widoku częściowego."
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 02/23/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/partial-tag-helper
ms.openlocfilehash: a9848539206892579501a39a9fce3044c6753948
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/15/2018
---
# <a name="partial-tag-helper-in-aspnet-core"></a><span data-ttu-id="fe2d0-103">Pomocnik częściowe tagu w platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fe2d0-103">Partial Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="fe2d0-104">Przez [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="fe2d0-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="fe2d0-105">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/built-in/samples) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="fe2d0-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/built-in/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="overview"></a><span data-ttu-id="fe2d0-106">Omówienie</span><span class="sxs-lookup"><span data-stu-id="fe2d0-106">Overview</span></span>

<span data-ttu-id="fe2d0-107">Częściowe pomocnika Tag jest używany do renderowania [widoku częściowego](xref:mvc/views/partial) w aplikacjach stron Razor i MVC.</span><span class="sxs-lookup"><span data-stu-id="fe2d0-107">The Partial Tag Helper is used for rendering a [partial view](xref:mvc/views/partial) in Razor Pages and MVC apps.</span></span> <span data-ttu-id="fe2d0-108">Należy wziąć pod uwagę że:</span><span class="sxs-lookup"><span data-stu-id="fe2d0-108">Consider that it:</span></span>

* <span data-ttu-id="fe2d0-109">Wymaga platformy ASP.NET Core 2.1 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="fe2d0-109">Requires ASP.NET Core 2.1 or later.</span></span>
* <span data-ttu-id="fe2d0-110">Jest to alternatywa dla [składni pomocnika kodu HTML](xref:mvc/views/partial#referencing-a-partial-view).</span><span class="sxs-lookup"><span data-stu-id="fe2d0-110">Is an alternative to [HTML Helper syntax](xref:mvc/views/partial#referencing-a-partial-view).</span></span>

<span data-ttu-id="fe2d0-111">Aby recap, dostępne są następujące opcje pomocnika kodu HTML do renderowania widoku częściowego:</span><span class="sxs-lookup"><span data-stu-id="fe2d0-111">To recap, the HTML Helper options for rendering a partial view include:</span></span>

* [<span data-ttu-id="fe2d0-112">@await Html.PartialAsync</span><span class="sxs-lookup"><span data-stu-id="fe2d0-112">@await Html.PartialAsync</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.partialasync)
* [<span data-ttu-id="fe2d0-113">@await Html.RenderPartialAsync</span><span class="sxs-lookup"><span data-stu-id="fe2d0-113">@await Html.RenderPartialAsync</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.renderpartialasync)
* [@Html.Partial](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.partial)
* [@Html.RenderPartial](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.renderpartial)

<span data-ttu-id="fe2d0-114">*Produktu* model jest używany w przykłady w tym dokumencie:</span><span class="sxs-lookup"><span data-stu-id="fe2d0-114">The *Product* model is used in samples throughout this document:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Models/Product.cs)]

<span data-ttu-id="fe2d0-115">Spis atrybutów częściowe pomocnika tagów jest zgodna.</span><span class="sxs-lookup"><span data-stu-id="fe2d0-115">An inventory of the Partial Tag Helper attributes follows.</span></span>

## <a name="name"></a><span data-ttu-id="fe2d0-116">nazwa</span><span class="sxs-lookup"><span data-stu-id="fe2d0-116">name</span></span>

<span data-ttu-id="fe2d0-117">`name` Atrybut jest wymagany.</span><span class="sxs-lookup"><span data-stu-id="fe2d0-117">The `name` attribute is required.</span></span> <span data-ttu-id="fe2d0-118">Wskazuje on, nazwa lub ścieżka widoku częściowego do renderowania.</span><span class="sxs-lookup"><span data-stu-id="fe2d0-118">It indicates the name or the path of the partial view to be rendered.</span></span> <span data-ttu-id="fe2d0-119">Jeśli podano nazwę widoku częściowego, [widok odnajdywania](xref:mvc/views/overview#view-discovery) proces jest inicjowany.</span><span class="sxs-lookup"><span data-stu-id="fe2d0-119">When a partial view name is provided, the [view discovery](xref:mvc/views/overview#view-discovery) process is initiated.</span></span> <span data-ttu-id="fe2d0-120">Ten proces jest pomijane, gdy została podana ścieżka jawna.</span><span class="sxs-lookup"><span data-stu-id="fe2d0-120">That process is bypassed when an explicit path is provided.</span></span>

<span data-ttu-id="fe2d0-121">Następujący kod używa jawne ścieżki, co oznacza, że *_ProductPartial.cshtml* ma zostać załadowane z *Shared* folderu.</span><span class="sxs-lookup"><span data-stu-id="fe2d0-121">The following markup uses an explicit path, indicating that *_ProductPartial.cshtml* is to be loaded from the *Shared* folder.</span></span> <span data-ttu-id="fe2d0-122">Przy użyciu [asp — dla](#asp-for) atrybut modelu są przekazywane do widoku częściowego dla powiązania.</span><span class="sxs-lookup"><span data-stu-id="fe2d0-122">Using the [asp-for](#asp-for) attribute, a model is passed to the partial view for binding.</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_Name)]

## <a name="asp-for"></a><span data-ttu-id="fe2d0-123">ASP — dla</span><span class="sxs-lookup"><span data-stu-id="fe2d0-123">asp-for</span></span>

<span data-ttu-id="fe2d0-124">`asp-for` Atrybutu przypisuje [ModelExpression](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.modelexpression) ma zostać obliczone dla bieżącego modelu.</span><span class="sxs-lookup"><span data-stu-id="fe2d0-124">The `asp-for` attribute assigns a [ModelExpression](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.modelexpression) to be evaluated against the current model.</span></span> <span data-ttu-id="fe2d0-125">A `ModelExpression` wnioskuje `@Model.` składni.</span><span class="sxs-lookup"><span data-stu-id="fe2d0-125">A `ModelExpression` infers the `@Model.` syntax.</span></span> <span data-ttu-id="fe2d0-126">Na przykład `asp-for="Product"` można użyć zamiast `asp-for="@Model.Product"`.</span><span class="sxs-lookup"><span data-stu-id="fe2d0-126">For example, `asp-for="Product"` can be used instead of `asp-for="@Model.Product"`.</span></span> <span data-ttu-id="fe2d0-127">To domyślne zachowanie wnioskowania zostanie zastąpiona przy użyciu `@` symbolu, aby zdefiniować wyrażenie wbudowanego.</span><span class="sxs-lookup"><span data-stu-id="fe2d0-127">This default inference behavior is overridden by using the `@` symbol to define an inline expression.</span></span>

<span data-ttu-id="fe2d0-128">Następujący kod znaczników ładuje *_ProductPartial.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="fe2d0-128">The following markup loads *_ProductPartial.cshtml*:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_AspFor)]

<span data-ttu-id="fe2d0-129">Widok częściowy jest powiązana z modelem skojarzonej strony `Product` właściwości:</span><span class="sxs-lookup"><span data-stu-id="fe2d0-129">The partial view is bound to the associated page model's `Product` property:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml.cs?highlight=8)]

## <a name="view-data"></a><span data-ttu-id="fe2d0-130">Wyświetlanie danych</span><span class="sxs-lookup"><span data-stu-id="fe2d0-130">view-data</span></span>

<span data-ttu-id="fe2d0-131">`view-data` Atrybutu przypisuje [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) do przekazania do widoku częściowego.</span><span class="sxs-lookup"><span data-stu-id="fe2d0-131">The `view-data` attribute assigns a [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) to pass to the partial view.</span></span> <span data-ttu-id="fe2d0-132">Następujący kod znaczników sprawia, że całą kolekcję ViewData dostępne do widoku częściowego:</span><span class="sxs-lookup"><span data-stu-id="fe2d0-132">The following markup makes the entire ViewData collection accessible to the partial view:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_ViewData&highlight=5-)]

<span data-ttu-id="fe2d0-133">W powyższym kodzie `IsNumberReadOnly` ma ustawioną wartość klucza `true` i dodać do kolekcji ViewData.</span><span class="sxs-lookup"><span data-stu-id="fe2d0-133">In the preceding code, the `IsNumberReadOnly` key value is set to `true` and added to the ViewData collection.</span></span> <span data-ttu-id="fe2d0-134">W rezultacie `ViewData["IsNumberReadOnly"]` jest udostępniane w widoku częściowego następujące:</span><span class="sxs-lookup"><span data-stu-id="fe2d0-134">Consequently, `ViewData["IsNumberReadOnly"]` is made accessible within the following partial view:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Shared/_ProductViewDataPartial.cshtml?highlight=5)]

<span data-ttu-id="fe2d0-135">W tym przykładzie wartość `ViewData["IsNumberReadOnly"]` Określa, czy *numer* pole jest wyświetlane jako tylko do odczytu.</span><span class="sxs-lookup"><span data-stu-id="fe2d0-135">In this example, the value of `ViewData["IsNumberReadOnly"]` determines whether the *Number* field is displayed as read only.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="fe2d0-136">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="fe2d0-136">Additional resources</span></span>

* [<span data-ttu-id="fe2d0-137">Widoki częściowe</span><span class="sxs-lookup"><span data-stu-id="fe2d0-137">Partial views</span></span>](xref:mvc/views/partial)
* [<span data-ttu-id="fe2d0-138">Lekko wprowadzonych danych (ViewData i obiekt ViewBag)</span><span class="sxs-lookup"><span data-stu-id="fe2d0-138">Weakly typed data (ViewData and ViewBag)</span></span>](xref:mvc/views/overview#weakly-typed-data-viewdata-and-viewbag)