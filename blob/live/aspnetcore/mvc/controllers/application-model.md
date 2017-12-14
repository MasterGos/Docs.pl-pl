---
title: Praca z modelem aplikacji
author: ardalis
description: 
keywords: ASP.NET MVC Core Core,ASP.NET, model aplikacji
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 4eb7e52f-5665-41a4-a3e3-e348d07337f2
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/application-model
ms.openlocfilehash: 3c35184921dbe26cde100fd3d5124e38ea0d06cf
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="working-with-the-application-model"></a><span data-ttu-id="9695a-103">Praca z modelem aplikacji</span><span class="sxs-lookup"><span data-stu-id="9695a-103">Working with the Application Model</span></span>

<span data-ttu-id="9695a-104">Przez [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="9695a-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="9695a-105">Definiuje platformy ASP.NET Core MVC *model aplikacji* reprezentująca składniki aplikacji MVC.</span><span class="sxs-lookup"><span data-stu-id="9695a-105">ASP.NET Core MVC defines an *application model* representing the components of an MVC app.</span></span> <span data-ttu-id="9695a-106">Może odczytywać i manipulowania ten model, aby zmodyfikować zachowanie elementy MVC.</span><span class="sxs-lookup"><span data-stu-id="9695a-106">You can read and manipulate this model to modify how MVC elements behave.</span></span> <span data-ttu-id="9695a-107">Domyślnie program MVC niektórych z konwencjami ustalenie klas, które są uważane za kontrolery, które metody dla tych klas są działania i zachowanie parametrów i routing.</span><span class="sxs-lookup"><span data-stu-id="9695a-107">By default, MVC follows certain conventions to determine which classes are considered to be controllers, which methods on those classes are actions, and how parameters and routing behave.</span></span> <span data-ttu-id="9695a-108">Można dostosować to zachowanie do potrzeb aplikacji, tworząc własne konwencje i zastosowaniu ich globalnie lub jako atrybuty.</span><span class="sxs-lookup"><span data-stu-id="9695a-108">You can customize this behavior to suit your app's needs by creating your own conventions and applying them globally or as attributes.</span></span>

## <a name="models-and-providers"></a><span data-ttu-id="9695a-109">Modele i dostawców</span><span class="sxs-lookup"><span data-stu-id="9695a-109">Models and Providers</span></span>

<span data-ttu-id="9695a-110">Model aplikacji ASP.NET Core MVC obejmują zarówno interfejsami abstrakcyjne i konkretną implementację klasy, które opisują aplikacji MVC.</span><span class="sxs-lookup"><span data-stu-id="9695a-110">The ASP.NET Core MVC application model include both abstract interfaces and concrete implementation classes that describe an MVC application.</span></span> <span data-ttu-id="9695a-111">Ten model jest wynikiem odnajdywanie kontrolerów, akcji, parametrów akcji, tras i filtrów w zależności od domyślnych Konwencji aplikacji MVC.</span><span class="sxs-lookup"><span data-stu-id="9695a-111">This model is the result of MVC discovering the app's controllers, actions, action parameters, routes, and filters according to default conventions.</span></span> <span data-ttu-id="9695a-112">Praca z modelu aplikacji, można zmodyfikować aplikacji zgodne z konwencjami różnych z domyślnym zachowaniem MVC.</span><span class="sxs-lookup"><span data-stu-id="9695a-112">By working with the application model, you can modify your app to follow different conventions from the default MVC behavior.</span></span> <span data-ttu-id="9695a-113">Parametry, nazwy trasy i filtry używane jako dane konfiguracyjne dla akcji i kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="9695a-113">The parameters, names, routes, and filters are all used as configuration data for actions and controllers.</span></span>

<span data-ttu-id="9695a-114">Model aplikacji platformy ASP.NET Core MVC ma następującą strukturę:</span><span class="sxs-lookup"><span data-stu-id="9695a-114">The ASP.NET Core MVC Application Model has the following structure:</span></span>

* <span data-ttu-id="9695a-115">ApplicationModel</span><span class="sxs-lookup"><span data-stu-id="9695a-115">ApplicationModel</span></span>
    * <span data-ttu-id="9695a-116">Kontrolery (ControllerModel)</span><span class="sxs-lookup"><span data-stu-id="9695a-116">Controllers (ControllerModel)</span></span>
        * <span data-ttu-id="9695a-117">Akcje (ActionModel)</span><span class="sxs-lookup"><span data-stu-id="9695a-117">Actions (ActionModel)</span></span>
            * <span data-ttu-id="9695a-118">Parametry (ParameterModel)</span><span class="sxs-lookup"><span data-stu-id="9695a-118">Parameters (ParameterModel)</span></span>

<span data-ttu-id="9695a-119">Każdy poziom modelu ma dostęp do wspólnego `Properties` kolekcji i niższe poziomy dostępu i zastąpić wartości właściwości ustawione przez wyższego poziomu w hierarchii.</span><span class="sxs-lookup"><span data-stu-id="9695a-119">Each level of the model has access to a common `Properties` collection, and lower levels can access and overwrite property values set by higher levels in the hierarchy.</span></span> <span data-ttu-id="9695a-120">Właściwości są zapisywane `ActionDescriptor.Properties` utworzenia akcje.</span><span class="sxs-lookup"><span data-stu-id="9695a-120">The properties are persisted to the `ActionDescriptor.Properties` when the actions are created.</span></span> <span data-ttu-id="9695a-121">Następnie, jeśli żądanie jest obsługiwane, wszystkie właściwości Konwencji dodane lub zmodyfikowane jest możliwy za pośrednictwem `ActionContext.ActionDescriptor.Properties`.</span><span class="sxs-lookup"><span data-stu-id="9695a-121">Then when a request is being handled, any properties a convention added or modified can be accessed through `ActionContext.ActionDescriptor.Properties`.</span></span> <span data-ttu-id="9695a-122">Przy użyciu właściwości jest doskonałym sposobem skonfigurowania z filtrów, integratorów modeli itd. na podstawie-action.</span><span class="sxs-lookup"><span data-stu-id="9695a-122">Using properties is a great way to configure your filters, model binders, etc. on a per-action basis.</span></span>

> [!NOTE]
> <span data-ttu-id="9695a-123">`ActionDescriptor.Properties` Kolekcji nie jest wielowątkowość (w przypadku zapisów), po zakończeniu uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9695a-123">The `ActionDescriptor.Properties` collection is not thread safe (for writes) once app startup has finished.</span></span> <span data-ttu-id="9695a-124">Konwencje to najlepszy sposób, aby bezpiecznie dodać dane do tej kolekcji.</span><span class="sxs-lookup"><span data-stu-id="9695a-124">Conventions are the best way to safely add data to this collection.</span></span>

### <a name="iapplicationmodelprovider"></a><span data-ttu-id="9695a-125">IApplicationModelProvider</span><span class="sxs-lookup"><span data-stu-id="9695a-125">IApplicationModelProvider</span></span>

<span data-ttu-id="9695a-126">Platformy ASP.NET Core MVC ładuje modelu aplikacji przy użyciu wzorca dostawcy, zdefiniowane przez [IApplicationModelProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.iapplicationmodelprovider) interfejsu.</span><span class="sxs-lookup"><span data-stu-id="9695a-126">ASP.NET Core MVC loads the application model using a provider pattern, defined by the [IApplicationModelProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.iapplicationmodelprovider) interface.</span></span> <span data-ttu-id="9695a-127">W tej sekcji opisano niektóre szczegóły wewnętrznej implementacji tej funkcji dostawcy.</span><span class="sxs-lookup"><span data-stu-id="9695a-127">This section covers some of the internal implementation details of how this provider functions.</span></span> <span data-ttu-id="9695a-128">To jest temat zaawansowany — większość aplikacji, które wykorzystują model aplikacji należy to zrobić, Praca z Konwencji.</span><span class="sxs-lookup"><span data-stu-id="9695a-128">This is an advanced topic - most apps that leverage the application model should do so by working with conventions.</span></span>

<span data-ttu-id="9695a-129">Implementacje `IApplicationModelProvider` interfejsu "wrap", z każdego wywołania implementacji `OnProvidersExecuting` rosnąco na podstawie jego `Order` właściwości.</span><span class="sxs-lookup"><span data-stu-id="9695a-129">Implementations of the `IApplicationModelProvider` interface "wrap" one another, with each implementation calling `OnProvidersExecuting` in ascending order based on its `Order` property.</span></span> <span data-ttu-id="9695a-130">`OnProvidersExecuted` Wywoływana jest metoda następnie w odwrotnej kolejności.</span><span class="sxs-lookup"><span data-stu-id="9695a-130">The `OnProvidersExecuted` method is then called in reverse order.</span></span> <span data-ttu-id="9695a-131">Wielu dostawców są zdefiniowane w ramach:</span><span class="sxs-lookup"><span data-stu-id="9695a-131">The framework defines several providers:</span></span>

<span data-ttu-id="9695a-132">Pierwszy (`Order=-1000`):</span><span class="sxs-lookup"><span data-stu-id="9695a-132">First (`Order=-1000`):</span></span>

* [`DefaultApplicationModelProvider`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.internal.defaultapplicationmodelprovider)

<span data-ttu-id="9695a-133">Następnie (`Order=-990`):</span><span class="sxs-lookup"><span data-stu-id="9695a-133">Then (`Order=-990`):</span></span>

* [`AuthorizationApplicationModelProvider`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.internal.authorizationapplicationmodelprovider)
* [`CorsApplicationModelProvider`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.cors.internal.corsapplicationmodelprovider)

> [!NOTE]
> <span data-ttu-id="9695a-134">Kolejność, w których dwóch dostawców z taką samą wartość `Order` są nazywane jest niezdefiniowana i dlatego nie powinno być stosowane.</span><span class="sxs-lookup"><span data-stu-id="9695a-134">The order in which two providers with the same value for `Order` are called is undefined, and therefore should not be relied upon.</span></span>

> [!NOTE]
> <span data-ttu-id="9695a-135">`IApplicationModelProvider`to zaawansowane pojęcia dla autorów framework rozszerzenie.</span><span class="sxs-lookup"><span data-stu-id="9695a-135">`IApplicationModelProvider` is an advanced concept for framework authors to extend.</span></span> <span data-ttu-id="9695a-136">Ogólnie rzecz biorąc aplikacje powinny używać konwencji i platform, należy użyć dostawcy.</span><span class="sxs-lookup"><span data-stu-id="9695a-136">In general, apps should use conventions and frameworks should use providers.</span></span> <span data-ttu-id="9695a-137">Klucza różnica polega na tym, że dostawcy są zawsze uruchamiane przed Konwencji.</span><span class="sxs-lookup"><span data-stu-id="9695a-137">The key distinction is that providers always run before conventions.</span></span>

<span data-ttu-id="9695a-138">`DefaultApplicationModelProvider` Ustanawia wiele zachowania domyślne używane przez program ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="9695a-138">The `DefaultApplicationModelProvider` establishes many of the default behaviors used by ASP.NET Core MVC.</span></span> <span data-ttu-id="9695a-139">Jego obowiązki obejmują:</span><span class="sxs-lookup"><span data-stu-id="9695a-139">Its responsibilities include:</span></span>

* <span data-ttu-id="9695a-140">Dodawanie filtrów globalnych do kontekstu</span><span class="sxs-lookup"><span data-stu-id="9695a-140">Adding global filters to the context</span></span>
* <span data-ttu-id="9695a-141">Dodanie kontrolerów w kontekście</span><span class="sxs-lookup"><span data-stu-id="9695a-141">Adding controllers to the context</span></span>
* <span data-ttu-id="9695a-142">Dodawanie metod publicznych kontrolera jako akcje</span><span class="sxs-lookup"><span data-stu-id="9695a-142">Adding public controller methods as actions</span></span>
* <span data-ttu-id="9695a-143">Dodawanie parametrów metody akcji w kontekście</span><span class="sxs-lookup"><span data-stu-id="9695a-143">Adding action method parameters to the context</span></span>
* <span data-ttu-id="9695a-144">Stosowanie trasy i inne atrybuty</span><span class="sxs-lookup"><span data-stu-id="9695a-144">Applying route and other attributes</span></span>

<span data-ttu-id="9695a-145">Niektóre wbudowane zachowań implementowanych przez `DefaultApplicationModelProvider`.</span><span class="sxs-lookup"><span data-stu-id="9695a-145">Some built-in behaviors are implemented by the `DefaultApplicationModelProvider`.</span></span> <span data-ttu-id="9695a-146">Ten dostawca jest odpowiedzialny za konstruowanie [ `ControllerModel` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.controllermodel), który z kolei odwołuje się do [ `ActionModel` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.actionmodel#Microsoft_AspNetCore_Mvc_ApplicationModels_ActionModel), [ `PropertyModel` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.propertymodel), i [ `ParameterModel` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.parametermodel#Microsoft_AspNetCore_Mvc_ApplicationModels_ParameterModel) wystąpień.</span><span class="sxs-lookup"><span data-stu-id="9695a-146">This provider is responsible for constructing the [`ControllerModel`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.controllermodel), which in turn references [`ActionModel`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.actionmodel#Microsoft_AspNetCore_Mvc_ApplicationModels_ActionModel), [`PropertyModel`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.propertymodel), and [`ParameterModel`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.parametermodel#Microsoft_AspNetCore_Mvc_ApplicationModels_ParameterModel) instances.</span></span> <span data-ttu-id="9695a-147">`DefaultApplicationModelProvider` Klasa jest szczegółów implementacji wewnętrzną strukturę i zmieni się w przyszłości.</span><span class="sxs-lookup"><span data-stu-id="9695a-147">The `DefaultApplicationModelProvider` class is an internal framework implementation detail that can and will change in the future.</span></span> 

<span data-ttu-id="9695a-148">`AuthorizationApplicationModelProvider` Jest odpowiedzialny za stosowanie zachowania związanego z `AuthorizeFilter` i `AllowAnonymousFilter` atrybutów.</span><span class="sxs-lookup"><span data-stu-id="9695a-148">The `AuthorizationApplicationModelProvider` is responsible for applying the behavior associated with the `AuthorizeFilter` and `AllowAnonymousFilter` attributes.</span></span> <span data-ttu-id="9695a-149">[Dowiedz się więcej o tych atrybutów](xref:security/authorization/simple).</span><span class="sxs-lookup"><span data-stu-id="9695a-149">[Learn more about these attributes](xref:security/authorization/simple).</span></span>

<span data-ttu-id="9695a-150">`CorsApplicationModelProvider` Implementuje zachowania związanego z `IEnableCorsAttribute` i `IDisableCorsAttribute`i `DisableCorsAuthorizationFilter`.</span><span class="sxs-lookup"><span data-stu-id="9695a-150">The `CorsApplicationModelProvider` implements behavior associated with the `IEnableCorsAttribute` and `IDisableCorsAttribute`, and the `DisableCorsAuthorizationFilter`.</span></span> <span data-ttu-id="9695a-151">[Dowiedz się więcej o CORS](xref:security/cors).</span><span class="sxs-lookup"><span data-stu-id="9695a-151">[Learn more about CORS](xref:security/cors).</span></span>

## <a name="conventions"></a><span data-ttu-id="9695a-152">Konwencje</span><span class="sxs-lookup"><span data-stu-id="9695a-152">Conventions</span></span>

<span data-ttu-id="9695a-153">Model aplikacji definiuje abstrakcje Konwencji, które zapewniają prostszy sposób, aby dostosować zachowanie modeli niż zastępowanie całego modelu lub dostawcy.</span><span class="sxs-lookup"><span data-stu-id="9695a-153">The application model defines convention abstractions that provide a simpler way to customize the behavior of the models than overriding the entire model or provider.</span></span> <span data-ttu-id="9695a-154">Te obiekty abstrakcyjne są zalecanym sposobem modyfikowania zachowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9695a-154">These abstractions are the recommended way to modify your app's behavior.</span></span> <span data-ttu-id="9695a-155">Konwencje umożliwiają do pisania kodu, który będzie dynamicznie astosuj dostosowania.</span><span class="sxs-lookup"><span data-stu-id="9695a-155">Conventions provide a way for you to write code that will dynamically apply customizations.</span></span> <span data-ttu-id="9695a-156">Gdy [filtry](xref:mvc/controllers/filters) umożliwiają modyfikowanie zachowania w ramach, dostosowania umożliwiają sprawowanie kontroli nad jak razem przewodowej całej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9695a-156">While [filters](xref:mvc/controllers/filters) provide a means of modifying the framework's behavior, customizations let you control how the whole app is wired together.</span></span>

<span data-ttu-id="9695a-157">Dostępne są następujące konwencje:</span><span class="sxs-lookup"><span data-stu-id="9695a-157">The following conventions are available:</span></span>

* [`IApplicationModelConvention`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.iapplicationmodelconvention)
* [`IControllerModelConvention`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.icontrollermodelconvention)
* [`IActionModelConvention`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.iactionmodelconvention)
* [`IParameterModelConvention`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.iparametermodelconvention)

<span data-ttu-id="9695a-158">Konwencje są stosowane przez dodanie ich do opcji MVC lub implementując `Attribute`s i zastosowaniu ich do kontrolerów, akcji lub parametry akcji (podobnie jak [ `Filters` ](xref:mvc/controllers/filters)).</span><span class="sxs-lookup"><span data-stu-id="9695a-158">Conventions are applied by adding them to MVC options or by implementing `Attribute`s and applying them to controllers, actions, or action parameters (similar to [`Filters`](xref:mvc/controllers/filters)).</span></span> <span data-ttu-id="9695a-159">W przeciwieństwie do filtrów konwencje są wykonywane tylko podczas uruchamiania aplikacji, nie jako część każdego żądania.</span><span class="sxs-lookup"><span data-stu-id="9695a-159">Unlike filters, conventions are only executed when the app is starting, not as part of each request.</span></span>

### <a name="sample-modifying-the-applicationmodel"></a><span data-ttu-id="9695a-160">Przykład: Modyfikacja ApplicationModel</span><span class="sxs-lookup"><span data-stu-id="9695a-160">Sample: Modifying the ApplicationModel</span></span>

<span data-ttu-id="9695a-161">Następującej konwencji umożliwia dodawanie właściwości do modelu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9695a-161">The following convention is used to add a property to the application model.</span></span> 

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/ApplicationDescription.cs)]

<span data-ttu-id="9695a-162">Konwencje modelu aplikacji są stosowane jako opcje woluminowi MVC `ConfigureServices` w `Startup`.</span><span class="sxs-lookup"><span data-stu-id="9695a-162">Application model conventions are applied as options when MVC is added in `ConfigureServices` in `Startup`.</span></span>

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Startup.cs?name=ConfigureServices&highlight=5)]

<span data-ttu-id="9695a-163">Właściwości są dostępne z `ActionDescriptor` kolekcji właściwości w akcji kontrolera:</span><span class="sxs-lookup"><span data-stu-id="9695a-163">Properties are accessible from the `ActionDescriptor` properties collection within controller actions:</span></span>

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/AppModelController.cs?name=AppModelController)]

### <a name="sample-modifying-the-controllermodel-description"></a><span data-ttu-id="9695a-164">Przykład: Modyfikacja opis ControllerModel</span><span class="sxs-lookup"><span data-stu-id="9695a-164">Sample: Modifying the ControllerModel Description</span></span>

<span data-ttu-id="9695a-165">Tak jak w poprzednim przykładzie modelu kontrolera może być modyfikowany aby uwzględnić właściwości niestandardowe.</span><span class="sxs-lookup"><span data-stu-id="9695a-165">As in the previous example, the controller model can also be modified to include custom properties.</span></span> <span data-ttu-id="9695a-166">To spowoduje zastąpienie istniejącej właściwości o takiej samej nazwie, jak określono w modelu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9695a-166">These will override existing properties with the same name specified in the application model.</span></span> <span data-ttu-id="9695a-167">Następujący atrybut Konwencji dodano opis na poziomie kontrolera:</span><span class="sxs-lookup"><span data-stu-id="9695a-167">The following convention attribute adds a description at the controller level:</span></span>

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/ControllerDescriptionAttribute.cs)]

<span data-ttu-id="9695a-168">Konwencja jest stosowany jako atrybut na kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="9695a-168">This convention is applied as an attribute on a controller.</span></span>

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/DescriptionAttributesController.cs?name=ControllerDescription&highlight=1)]

<span data-ttu-id="9695a-169">Właściwość "opis" jest dostępny w taki sam sposób jak w poprzednich przykładach.</span><span class="sxs-lookup"><span data-stu-id="9695a-169">The "description" property is accessed in the same manner as in previous examples.</span></span>

### <a name="sample-modifying-the-actionmodel-description"></a><span data-ttu-id="9695a-170">Przykład: Modyfikacja opis ActionModel</span><span class="sxs-lookup"><span data-stu-id="9695a-170">Sample: Modifying the ActionModel Description</span></span>

<span data-ttu-id="9695a-171">Konwencja oddzielne atrybut można zastosować do poszczególnych działań, zastępowanie zachowania już stosowane na poziomie aplikacji lub kontrolera.</span><span class="sxs-lookup"><span data-stu-id="9695a-171">A separate attribute convention can be applied to individual actions, overriding behavior already applied at the application or controller level.</span></span>

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/ActionDescriptionAttribute.cs)]

<span data-ttu-id="9695a-172">Stosowania tego działania w ramach kontrolera w poprzednim przykładzie pokazano, jak zastępuje on Konwencji poziomie kontrolera:</span><span class="sxs-lookup"><span data-stu-id="9695a-172">Applying this to an action within the previous example's controller demonstrates how it overrides the controller-level convention:</span></span>

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/DescriptionAttributesController.cs?name=DescriptionAttributesController&highlight=9)]

### <a name="sample-modifying-the-parametermodel"></a><span data-ttu-id="9695a-173">Przykład: Modyfikacja ParameterModel</span><span class="sxs-lookup"><span data-stu-id="9695a-173">Sample: Modifying the ParameterModel</span></span>

<span data-ttu-id="9695a-174">Można zastosować następującą konwencją do parametrów akcji, aby zmodyfikować ich `BindingInfo`.</span><span class="sxs-lookup"><span data-stu-id="9695a-174">The following convention can be applied to action parameters to modify their `BindingInfo`.</span></span> <span data-ttu-id="9695a-175">Następującej konwencji wymaga parametru parametru trasy; inne potencjalne źródła powiązanie (na przykład wartości ciągu zapytania) są ignorowane.</span><span class="sxs-lookup"><span data-stu-id="9695a-175">The following convention requires that the parameter be a route parameter; other potential binding sources (such as query string values) are ignored.</span></span>

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/MustBeInRouteParameterModelConvention.cs)]

<span data-ttu-id="9695a-176">Ten atrybut można stosować do żadnego parametru akcji:</span><span class="sxs-lookup"><span data-stu-id="9695a-176">The attribute may be applied to any action parameter:</span></span>

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/ParameterModelController.cs?name=ParameterModelController&highlight=5)]

### <a name="sample-modifying-the-actionmodel-name"></a><span data-ttu-id="9695a-177">Przykład: Modyfikowanie nazwy ActionModel</span><span class="sxs-lookup"><span data-stu-id="9695a-177">Sample: Modifying the ActionModel Name</span></span>

<span data-ttu-id="9695a-178">Modyfikuje następującej konwencji `ActionModel` zaktualizować *nazwa* akcji, do którego jest stosowana.</span><span class="sxs-lookup"><span data-stu-id="9695a-178">The following convention modifies the `ActionModel` to update the *name* of the action to which it is applied.</span></span> <span data-ttu-id="9695a-179">Nowa nazwa jest podać jako parametr do atrybutu.</span><span class="sxs-lookup"><span data-stu-id="9695a-179">The new name is provided as a parameter to the attribute.</span></span> <span data-ttu-id="9695a-180">Ta nowa nazwa jest używany przez routingu, więc będzie miało wpływ na trasy do parametru tej metody akcji.</span><span class="sxs-lookup"><span data-stu-id="9695a-180">This new name is used by routing, so it will affect the route used to reach this action method.</span></span>

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/CustomActionNameAttribute.cs)]

<span data-ttu-id="9695a-181">Ten atrybut jest stosowany do metody akcji w `HomeController`:</span><span class="sxs-lookup"><span data-stu-id="9695a-181">This attribute is applied to an action method in the `HomeController`:</span></span>

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/HomeController.cs?name=ActionModelConvention&highlight=2)]

<span data-ttu-id="9695a-182">Mimo że nazwa metody jest `SomeName`, atrybut zastępuje przy użyciu nazwy metody z Konwencją MVC i zastępuje nazwę akcji z `MyCoolAction`.</span><span class="sxs-lookup"><span data-stu-id="9695a-182">Even though the method name is `SomeName`, the attribute overrides the MVC convention of using the method name and replaces the action name with `MyCoolAction`.</span></span> <span data-ttu-id="9695a-183">W związku z tym trasy używany w celu osiągnięcia tej akcji jest `/Home/MyCoolAction`.</span><span class="sxs-lookup"><span data-stu-id="9695a-183">Thus, the route used to reach this action is `/Home/MyCoolAction`.</span></span>

> [!NOTE]
> <span data-ttu-id="9695a-184">W tym przykładzie jest zasadniczo taki sam, jak za pomocą wbudowanych [Nazwa akcji](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.actionnameattribute) atrybutu.</span><span class="sxs-lookup"><span data-stu-id="9695a-184">This example is essentially the same as using the built-in [ActionName](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.actionnameattribute) attribute.</span></span>

### <a name="sample-custom-routing-convention"></a><span data-ttu-id="9695a-185">Przykład: Niestandardowy Routing Konwencji</span><span class="sxs-lookup"><span data-stu-id="9695a-185">Sample: Custom Routing Convention</span></span>

<span data-ttu-id="9695a-186">Można użyć `IApplicationModelConvention` dostosować działa jak routingu.</span><span class="sxs-lookup"><span data-stu-id="9695a-186">You can use an `IApplicationModelConvention` to customize how routing works.</span></span> <span data-ttu-id="9695a-187">Na przykład następującej konwencji dołączyć przestrzeni nazw kontrolerów do ich trasy, zastępując `.` w przestrzeni nazw z `/` w trasie:</span><span class="sxs-lookup"><span data-stu-id="9695a-187">For example, the following convention will incorporate Controllers' namespaces into their routes, replacing `.` in the namespace with `/` in the route:</span></span>

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/NamespaceRoutingConvention.cs)]

<span data-ttu-id="9695a-188">Konwencji jest dodawana jako opcję uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="9695a-188">The convention is added as an option in Startup.</span></span>

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Startup.cs?name=ConfigureServices&highlight=6)]

> [!TIP]
> <span data-ttu-id="9695a-189">Można dodać Konwencji do Twojej [oprogramowanie pośredniczące](xref:fundamentals/middleware) uzyskując dostęp do `MvcOptions` przy użyciu`services.Configure<MvcOptions>(c => c.Conventions.Add(YOURCONVENTION));`</span><span class="sxs-lookup"><span data-stu-id="9695a-189">You can add conventions to your [middleware](xref:fundamentals/middleware) by accessing `MvcOptions` using `services.Configure<MvcOptions>(c => c.Conventions.Add(YOURCONVENTION));`</span></span>

<span data-ttu-id="9695a-190">Ten przykład dotyczy tę Konwencję tras, które nie używają atrybutu routingu, gdy kontroler ma "Namespace" w nazwie.</span><span class="sxs-lookup"><span data-stu-id="9695a-190">This sample applies this convention to routes that are not using attribute routing where the controller has  "Namespace" in its name.</span></span> <span data-ttu-id="9695a-191">Następujący kontroler ilustruje tę Konwencję:</span><span class="sxs-lookup"><span data-stu-id="9695a-191">The following controller demonstrates this convention:</span></span>

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/NamespaceRoutingController.cs?highlight=7-8)]

## <a name="application-model-usage-in-webapicompatshim"></a><span data-ttu-id="9695a-192">Użycie modelu aplikacji w WebApiCompatShim</span><span class="sxs-lookup"><span data-stu-id="9695a-192">Application Model Usage in WebApiCompatShim</span></span>

<span data-ttu-id="9695a-193">Platformy ASP.NET Core MVC korzysta z innego zestawu Konwencji z ASP.NET Web API 2.</span><span class="sxs-lookup"><span data-stu-id="9695a-193">ASP.NET Core MVC uses a different set of conventions from ASP.NET Web API 2.</span></span> <span data-ttu-id="9695a-194">Konwencje niestandardowych można zmodyfikować zachowanie aplikacji ASP.NET Core MVC być zgodny z aplikacji interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="9695a-194">Using custom conventions, you can modify an ASP.NET Core MVC app's behavior to be consistent with that of a Web API app.</span></span> <span data-ttu-id="9695a-195">Microsoft jest dostarczany [WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim/) specjalnie do tego celu.</span><span class="sxs-lookup"><span data-stu-id="9695a-195">Microsoft ships the [WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim/) specifically for this purpose.</span></span>

> [!NOTE]
> <span data-ttu-id="9695a-196">Dowiedz się więcej o [migracji z interfejsu API sieci Web platformy ASP.NET](xref:migration/webapi).</span><span class="sxs-lookup"><span data-stu-id="9695a-196">Learn more about [migrating from ASP.NET Web API](xref:migration/webapi).</span></span>

<span data-ttu-id="9695a-197">Aby użyć podkładek zgodności interfejsu API sieci Web, należy dodać pakiet do projektu, a następnie dodaj konwencje do MVC, wywołując `AddWebApiConventions` w `Startup`:</span><span class="sxs-lookup"><span data-stu-id="9695a-197">To use the Web API Compatibility Shim, you need to add the package to your project and then add the conventions to MVC by calling `AddWebApiConventions` in `Startup`:</span></span>

```c#
services.AddMvc().AddWebApiConventions();
```

<span data-ttu-id="9695a-198">Konwencje pochodzącymi z podkładką są stosowane tylko do elementów aplikacji, które miały określonych atrybutów.</span><span class="sxs-lookup"><span data-stu-id="9695a-198">The conventions provided by the shim are only applied to parts of the app that have had certain attributes applied to them.</span></span> <span data-ttu-id="9695a-199">Czterech atrybutów umożliwiają określanie kontrolerów powinny mieć ich konwencje zmodyfikowane przez konwencje podkładki:</span><span class="sxs-lookup"><span data-stu-id="9695a-199">The following four attributes are used to control which controllers should have their conventions modified by the shim's conventions:</span></span>

* [<span data-ttu-id="9695a-200">UseWebApiActionConventionsAttribute</span><span class="sxs-lookup"><span data-stu-id="9695a-200">UseWebApiActionConventionsAttribute</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapiactionconventionsattribute)
* [<span data-ttu-id="9695a-201">UseWebApiOverloadingAttribute</span><span class="sxs-lookup"><span data-stu-id="9695a-201">UseWebApiOverloadingAttribute</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapioverloadingattribute)
* [<span data-ttu-id="9695a-202">UseWebApiParameterConventionsAttribute</span><span class="sxs-lookup"><span data-stu-id="9695a-202">UseWebApiParameterConventionsAttribute</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapiparameterconventionsattribute)
* [<span data-ttu-id="9695a-203">UseWebApiRoutesAttribute</span><span class="sxs-lookup"><span data-stu-id="9695a-203">UseWebApiRoutesAttribute</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapiroutesattribute)

### <a name="action-conventions"></a><span data-ttu-id="9695a-204">Konwencje akcji</span><span class="sxs-lookup"><span data-stu-id="9695a-204">Action Conventions</span></span>

<span data-ttu-id="9695a-205">`UseWebApiActionConventionsAttribute` Jest używany do mapowania akcji na podstawie ich nazwy metody HTTP (na przykład `Get` czy mapowania `HttpGet`).</span><span class="sxs-lookup"><span data-stu-id="9695a-205">The `UseWebApiActionConventionsAttribute` is used to map the HTTP method to actions based on their name (for instance, `Get` would map to `HttpGet`).</span></span> <span data-ttu-id="9695a-206">Dotyczy tylko akcje, które nie korzystają z atrybutu routingu.</span><span class="sxs-lookup"><span data-stu-id="9695a-206">It only applies to actions that do not use attribute routing.</span></span>

### <a name="overloading"></a><span data-ttu-id="9695a-207">Przeciążenie</span><span class="sxs-lookup"><span data-stu-id="9695a-207">Overloading</span></span>

<span data-ttu-id="9695a-208">`UseWebApiOverloadingAttribute` Służy do stosowania `WebApiOverloadingApplicationModelConvention` Konwencji.</span><span class="sxs-lookup"><span data-stu-id="9695a-208">The `UseWebApiOverloadingAttribute` is used to apply the `WebApiOverloadingApplicationModelConvention` convention.</span></span> <span data-ttu-id="9695a-209">Dodaje tę konwencję `OverloadActionConstraint` do procesu wyboru akcji, co ogranicza akcje kandydata do tych, dla których żądanie spełnia wszystkie parametry opcjonalne.</span><span class="sxs-lookup"><span data-stu-id="9695a-209">This convention adds an `OverloadActionConstraint` to the action selection process, which limits candidate actions to those for which the request satisfies all non-optional parameters.</span></span>

### <a name="parameter-conventions"></a><span data-ttu-id="9695a-210">Konwencje parametru</span><span class="sxs-lookup"><span data-stu-id="9695a-210">Parameter Conventions</span></span>

<span data-ttu-id="9695a-211">`UseWebApiParameterConventionsAttribute` Służy do stosowania `WebApiParameterConventionsApplicationModelConvention` Konwencji akcji.</span><span class="sxs-lookup"><span data-stu-id="9695a-211">The `UseWebApiParameterConventionsAttribute` is used to apply the `WebApiParameterConventionsApplicationModelConvention` action convention.</span></span> <span data-ttu-id="9695a-212">Konwencja określa, że proste typy używane jako parametry działania są powiązane z identyfikatora URI domyślnie podczas typy złożone są powiązane z treści żądania.</span><span class="sxs-lookup"><span data-stu-id="9695a-212">This convention specifies that simple types used as action parameters are bound from the URI by default, while complex types are bound from the request body.</span></span>

### <a name="routes"></a><span data-ttu-id="9695a-213">Trasy</span><span class="sxs-lookup"><span data-stu-id="9695a-213">Routes</span></span>

<span data-ttu-id="9695a-214">`UseWebApiRoutesAttribute` Formanty czy `WebApiApplicationModelConvention` jest stosowana Konwencja kontrolera.</span><span class="sxs-lookup"><span data-stu-id="9695a-214">The `UseWebApiRoutesAttribute` controls whether the `WebApiApplicationModelConvention` controller convention is applied.</span></span> <span data-ttu-id="9695a-215">Po włączeniu tę Konwencję umożliwia obsługę [obszarów](xref:mvc/controllers/areas) do trasy.</span><span class="sxs-lookup"><span data-stu-id="9695a-215">When enabled, this convention is used to add support for [areas](xref:mvc/controllers/areas) to the route.</span></span>

<span data-ttu-id="9695a-216">Oprócz zestawie Konwencji, pakiet zgodności zawiera `System.Web.Http.ApiController` podstawowa klasa, która zastępuje udostępniony przez interfejs API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="9695a-216">In addition to a set of conventions, the compatibility package includes a `System.Web.Http.ApiController` base class that replaces the one provided by Web API.</span></span> <span data-ttu-id="9695a-217">Dzięki temu kontrolerów napisane dla interfejsu API sieci Web i dziedziczenie z jego `ApiController` będzie działać zgodnie z zostały zaprojektowane tak, podczas uruchamiania na platformie ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="9695a-217">This allows your controllers written for Web API and inheriting from its `ApiController` to work as they were designed, while running on ASP.NET Core MVC.</span></span> <span data-ttu-id="9695a-218">Ta klasa podstawowa kontroler zostanie nadany wszystkie `UseWebApi*` atrybuty wymienione powyżej.</span><span class="sxs-lookup"><span data-stu-id="9695a-218">This base controller class is decorated with all of the `UseWebApi*` attributes listed above.</span></span> <span data-ttu-id="9695a-219">`ApiController` Udostępnia właściwości, metody i typy wyników, które są zgodne z tymi znaleziono w interfejsie API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="9695a-219">The `ApiController` exposes properties, methods, and result types that are compatible with those found in Web API.</span></span>

## <a name="using-apiexplorer-to-document-your-app"></a><span data-ttu-id="9695a-220">Przy użyciu ApiExplorer dokumentów aplikacji</span><span class="sxs-lookup"><span data-stu-id="9695a-220">Using ApiExplorer to Document Your App</span></span>

<span data-ttu-id="9695a-221">Udostępnia model aplikacji [ `ApiExplorer` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.apiexplorermodel) właściwości na każdym poziomie, który może służyć do przechodzenia struktury aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9695a-221">The application model exposes an [`ApiExplorer`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.apiexplorermodel) property at each level that can be used to traverse the app's structure.</span></span> <span data-ttu-id="9695a-222">Może to być używane do [generowania strony pomocy dla interfejsów API sieci Web za pomocą takich narzędzi jak Swagger](https://docs.microsoft.com/aspnet/core/tutorials/web-api-help-pages-using-swagger).</span><span class="sxs-lookup"><span data-stu-id="9695a-222">This can be used to [generate help pages for your Web APIs using tools like Swagger](https://docs.microsoft.com/aspnet/core/tutorials/web-api-help-pages-using-swagger).</span></span> <span data-ttu-id="9695a-223">`ApiExplorer` Ujawnia właściwości `IsVisible` właściwości, który można ustawić, aby określić części modelu aplikacji, które powinny zostać ujawnione.</span><span class="sxs-lookup"><span data-stu-id="9695a-223">The `ApiExplorer` property exposes an `IsVisible` property that can be set to specify which parts of your app's model should be exposed.</span></span> <span data-ttu-id="9695a-224">Można skonfigurować to ustawienie za pomocą Konwencji:</span><span class="sxs-lookup"><span data-stu-id="9695a-224">You can configure this setting using a convention:</span></span>

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/EnableApiExplorerApplicationConvention.cs)]

<span data-ttu-id="9695a-225">Przy użyciu tej metody (i konwencje dodatkowe, jeśli jest to wymagane), można włączyć lub wyłączyć widoczność interfejsu API na dowolnym poziomie w Twojej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9695a-225">Using this approach (and additional conventions if required), you can enable or disable API visibility at any level within your app.</span></span> 