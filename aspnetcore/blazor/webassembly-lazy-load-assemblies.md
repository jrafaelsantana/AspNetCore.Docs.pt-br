---
title: Assemblies de carga lenta no ASP.NET CoreBlazor WebAssembly
author: guardrex
description: Descubra como carregar com lentas os assemblies em ASP.NET Core Blazor WebAssembly aplicativos.
monikerRange: '>= aspnetcore-5.0'
ms.author: riande
ms.custom: mvc
ms.date: 07/16/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/webassembly-lazy-load-assemblies
ms.openlocfilehash: 0fb744b4e9d44e6b8136123fddfb75ace8901d52
ms.sourcegitcommit: 84150702757cf7a7b839485382420e8db8e92b9c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/05/2020
ms.locfileid: "87819937"
---
# <a name="lazy-load-assemblies-in-aspnet-core-no-locblazor-webassembly"></a><span data-ttu-id="2369f-103">Assemblies de carga lenta no ASP.NET CoreBlazor WebAssembly</span><span class="sxs-lookup"><span data-stu-id="2369f-103">Lazy load assemblies in ASP.NET Core Blazor WebAssembly</span></span>

<span data-ttu-id="2369f-104">Por [Safia Abdalla](https://safia.rocks) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="2369f-104">By [Safia Abdalla](https://safia.rocks) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="2369f-105">Blazor WebAssemblyo desempenho de inicialização do aplicativo pode ser melhorado ao adiar o carregamento de alguns assemblies de aplicativo até que eles sejam necessários, o que é chamado de *carregamento lento*.</span><span class="sxs-lookup"><span data-stu-id="2369f-105">Blazor WebAssembly app startup performance can be improved by deferring the loading of some application assemblies until they are required, which is called *lazy loading*.</span></span> <span data-ttu-id="2369f-106">Por exemplo, os assemblies que são usados somente para processar um único componente podem ser configurados para carregar somente se o usuário navegar para esse componente.</span><span class="sxs-lookup"><span data-stu-id="2369f-106">For example, assemblies that are only used to render a single component can be set up to load only if the user navigates to that component.</span></span> <span data-ttu-id="2369f-107">Após o carregamento, os assemblies são armazenados em cache no lado do cliente e estão disponíveis para todas as navegações futuras.</span><span class="sxs-lookup"><span data-stu-id="2369f-107">After loading, the assemblies are cached client-side and are available for all future navigations.</span></span>

<span data-ttu-id="2369f-108">Blazoro recurso de carregamento lento do permite que você marque os assemblies de aplicativo para carregamento lento, que carrega os assemblies durante o tempo de execução quando o usuário navega para uma rota específica.</span><span class="sxs-lookup"><span data-stu-id="2369f-108">Blazor's lazy loading feature allows you to mark app assemblies for lazy loading, which loads the assemblies during runtime when the user navigates to a particular route.</span></span> <span data-ttu-id="2369f-109">O recurso consiste em alterações no arquivo do projeto e alterações no roteador do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="2369f-109">The feature consists of changes to the project file and changes to the application's router.</span></span>

> [!NOTE]
> <span data-ttu-id="2369f-110">O carregamento lento do assembly não beneficia Blazor Server aplicativos porque assemblies não são baixados para o cliente em um Blazor Server aplicativo.</span><span class="sxs-lookup"><span data-stu-id="2369f-110">Assembly lazy loading doesn't benefit Blazor Server apps because assemblies aren't downloaded to the client in a Blazor Server app.</span></span>

## <a name="project-file"></a><span data-ttu-id="2369f-111">Arquivo de projeto</span><span class="sxs-lookup"><span data-stu-id="2369f-111">Project file</span></span>

<span data-ttu-id="2369f-112">Marque assemblies para carregamento lento no arquivo de projeto do aplicativo ( `.csproj` ) usando o `BlazorWebAssemblyLazyLoad` Item.</span><span class="sxs-lookup"><span data-stu-id="2369f-112">Mark assemblies for lazy loading in the app's project file (`.csproj`) using the `BlazorWebAssemblyLazyLoad` item.</span></span> <span data-ttu-id="2369f-113">Use o nome do assembly sem a `.dll` extensão.</span><span class="sxs-lookup"><span data-stu-id="2369f-113">Use the assembly name without the `.dll` extension.</span></span> <span data-ttu-id="2369f-114">A Blazor estrutura impede que os assemblies especificados por esse grupo de itens sejam carregados na inicialização do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="2369f-114">The Blazor framework prevents the assemblies specified by this item group from loading at app launch.</span></span> <span data-ttu-id="2369f-115">O exemplo a seguir marca um assembly personalizado grande ( `GrantImaharaRobotControls.dll` ) para carregamento lento.</span><span class="sxs-lookup"><span data-stu-id="2369f-115">The following example marks a large custom assembly (`GrantImaharaRobotControls.dll`) for lazy loading.</span></span> <span data-ttu-id="2369f-116">Se um assembly marcado para carregamento lento tiver dependências, eles também deverão ser marcados para carregamento lento no arquivo de projeto.</span><span class="sxs-lookup"><span data-stu-id="2369f-116">If an assembly that's marked for lazy loading has dependencies, they must also be marked for lazy loading in the project file.</span></span>

```xml
<ItemGroup>
  <BlazorWebAssemblyLazyLoad Include="GrantImaharaRobotControls" />
</ItemGroup>
```

<span data-ttu-id="2369f-117">Somente os assemblies que são usados pelo aplicativo podem ser carregados lentamente.</span><span class="sxs-lookup"><span data-stu-id="2369f-117">Only assemblies that are used by the app can be lazily loaded.</span></span> <span data-ttu-id="2369f-118">O vinculador retira assemblies não utilizados da saída publicada.</span><span class="sxs-lookup"><span data-stu-id="2369f-118">The linker strips unused assemblies from published output.</span></span>

## <a name="router-component"></a><span data-ttu-id="2369f-119">componente `Router`</span><span class="sxs-lookup"><span data-stu-id="2369f-119">`Router` component</span></span>

<span data-ttu-id="2369f-120">Blazor`Router`o componente do designa quais assemblies Blazor pesquisam componentes roteáveis.</span><span class="sxs-lookup"><span data-stu-id="2369f-120">Blazor's `Router` component designates which assemblies Blazor searches for routable components.</span></span> <span data-ttu-id="2369f-121">O `Router` componente também é responsável por renderizar o componente para a rota onde o usuário navega.</span><span class="sxs-lookup"><span data-stu-id="2369f-121">The `Router` component is also responsible for rendering the component for the route where the user navigates.</span></span> <span data-ttu-id="2369f-122">O `Router` componente dá suporte a um `OnNavigateAsync` recurso que pode ser usado em conjunto com o carregamento lento.</span><span class="sxs-lookup"><span data-stu-id="2369f-122">The `Router` component supports an `OnNavigateAsync` feature that can be used in conjunction with lazy loading.</span></span>

<span data-ttu-id="2369f-123">No componente do aplicativo `Router` ( `App.razor` ):</span><span class="sxs-lookup"><span data-stu-id="2369f-123">In the app's `Router` component (`App.razor`):</span></span>

* <span data-ttu-id="2369f-124">Adicione um `OnNavigateAsync` retorno de chamada.</span><span class="sxs-lookup"><span data-stu-id="2369f-124">Add an `OnNavigateAsync` callback.</span></span> <span data-ttu-id="2369f-125">O `OnNavigateAsync` manipulador é invocado quando o usuário:</span><span class="sxs-lookup"><span data-stu-id="2369f-125">The `OnNavigateAsync` handler is invoked when the user:</span></span>
  * <span data-ttu-id="2369f-126">Visita uma rota pela primeira vez navegando diretamente de seu navegador.</span><span class="sxs-lookup"><span data-stu-id="2369f-126">Visits a route for the first time by navigating to it directly from their browser.</span></span>
  * <span data-ttu-id="2369f-127">Navega para uma nova rota usando um link ou uma <xref:Microsoft.AspNetCore.Components.NavigationManager.NavigateTo%2A?displayProperty=nameWithType> invocação.</span><span class="sxs-lookup"><span data-stu-id="2369f-127">Navigates to a new route using a link or a <xref:Microsoft.AspNetCore.Components.NavigationManager.NavigateTo%2A?displayProperty=nameWithType> invocation.</span></span>
* <span data-ttu-id="2369f-128">Se os assemblies com carregamento lento contiverem componentes roteáveis, adicione uma [lista](xref:System.Collections.Generic.List%601) \<<xref:System.Reflection.Assembly>> (por exemplo, nomeada `lazyLoadedAssemblies` ) ao componente.</span><span class="sxs-lookup"><span data-stu-id="2369f-128">If lazy-loaded assemblies contain routable components, add a [List](xref:System.Collections.Generic.List%601)\<<xref:System.Reflection.Assembly>> (for example, named `lazyLoadedAssemblies`) to the component.</span></span> <span data-ttu-id="2369f-129">Os assemblies são passados de volta para a <xref:Microsoft.AspNetCore.Components.Routing.Router.AdditionalAssemblies> coleção caso os assemblies contenham componentes roteáveis.</span><span class="sxs-lookup"><span data-stu-id="2369f-129">The assemblies are passed back to the <xref:Microsoft.AspNetCore.Components.Routing.Router.AdditionalAssemblies> collection in case the assemblies contain routable components.</span></span> <span data-ttu-id="2369f-130">A estrutura pesquisa os assemblies em busca de rotas e atualiza a coleção de rotas se alguma nova rota for encontrada.</span><span class="sxs-lookup"><span data-stu-id="2369f-130">The framework searches the assemblies for routes and updates the route collection if any new routes are found.</span></span>

```razor
@using System.Reflection

<Router AppAssembly="@typeof(Program).Assembly" 
    AdditionalAssemblies="@lazyLoadedAssemblies" OnNavigateAsync="@OnNavigateAsync">
    ...
</Router>

@code {
    private List<Assembly> lazyLoadedAssemblies = new List<Assembly>();

    private async Task OnNavigateAsync(NavigationContext args)
    {
    }
}
```

<span data-ttu-id="2369f-131">Se o `OnNavigateAsync` retorno de chamada lançar uma exceção sem tratamento, a [ Blazor interface do usuário de erro](xref:blazor/fundamentals/handle-errors#detailed-errors-during-development) será invocada.</span><span class="sxs-lookup"><span data-stu-id="2369f-131">If the `OnNavigateAsync` callback throws an unhandled exception, the [Blazor error UI](xref:blazor/fundamentals/handle-errors#detailed-errors-during-development) is invoked.</span></span>

### <a name="assembly-load-logic-in-onnavigateasync"></a><span data-ttu-id="2369f-132">Lógica de carga de assembly em`OnNavigateAsync`</span><span class="sxs-lookup"><span data-stu-id="2369f-132">Assembly load logic in `OnNavigateAsync`</span></span>

<span data-ttu-id="2369f-133">`OnNavigateAsync`tem um `NavigationContext` parâmetro que fornece informações sobre o evento de navegação assíncrona atual, incluindo o caminho de destino ( `Path` ) e o token de cancelamento ( `CancellationToken` ):</span><span class="sxs-lookup"><span data-stu-id="2369f-133">`OnNavigateAsync` has a `NavigationContext` parameter that provides information about the current asynchronous navigation event, including the target path (`Path`) and the cancellation token (`CancellationToken`):</span></span>

* <span data-ttu-id="2369f-134">A `Path` propriedade é o caminho de destino do usuário em relação ao caminho base do aplicativo, como `/robot` .</span><span class="sxs-lookup"><span data-stu-id="2369f-134">The `Path` property is the user's destination path relative to the app's base path, such as `/robot`.</span></span>
* <span data-ttu-id="2369f-135">O `CancellationToken` pode ser usado para observar o cancelamento da tarefa assíncrona.</span><span class="sxs-lookup"><span data-stu-id="2369f-135">The `CancellationToken` can be used to observe the cancellation of the asynchronous task.</span></span> <span data-ttu-id="2369f-136">`OnNavigateAsync`Cancela automaticamente a tarefa de navegação em execução no momento quando o usuário navega para uma página diferente.</span><span class="sxs-lookup"><span data-stu-id="2369f-136">`OnNavigateAsync` automatically cancels the currently running navigation task when the user navigates to a different page.</span></span>

<span data-ttu-id="2369f-137">No `OnNavigateAsync` , implemente a lógica para determinar os assemblies a serem carregados.</span><span class="sxs-lookup"><span data-stu-id="2369f-137">Inside `OnNavigateAsync`, implement logic to determine the assemblies to load.</span></span> <span data-ttu-id="2369f-138">As opções incluem:</span><span class="sxs-lookup"><span data-stu-id="2369f-138">Options include:</span></span>

* <span data-ttu-id="2369f-139">Verificações condicionais dentro do `OnNavigateAsync` método.</span><span class="sxs-lookup"><span data-stu-id="2369f-139">Conditional checks inside the `OnNavigateAsync` method.</span></span>
* <span data-ttu-id="2369f-140">Uma tabela de pesquisa que mapeia rotas para nomes de assembly, injetada no componente ou implementada dentro do [`@code`](xref:mvc/views/razor#code) bloco.</span><span class="sxs-lookup"><span data-stu-id="2369f-140">A lookup table that maps routes to assembly names, either injected into the component or implemented within the [`@code`](xref:mvc/views/razor#code) block.</span></span>

<span data-ttu-id="2369f-141">`LazyAssemblyLoader`é um serviço singleton fornecido pela estrutura para carregar assemblies.</span><span class="sxs-lookup"><span data-stu-id="2369f-141">`LazyAssemblyLoader` is a framework-provided singleton service for loading assemblies.</span></span> <span data-ttu-id="2369f-142">Injetar `LazyAssemblyLoader` no `Router` componente:</span><span class="sxs-lookup"><span data-stu-id="2369f-142">Inject `LazyAssemblyLoader` into the `Router` component:</span></span>

```razor
...
@using Microsoft.AspNetCore.Components.WebAssembly.Services
@inject LazyAssemblyLoader assemblyLoader

...
```

<span data-ttu-id="2369f-143">O `LazyAssemblyLoader` fornece o `LoadAssembliesAsync` método que:</span><span class="sxs-lookup"><span data-stu-id="2369f-143">The `LazyAssemblyLoader` provides the `LoadAssembliesAsync` method that:</span></span>

* <span data-ttu-id="2369f-144">Usa a interoperabilidade JS para buscar assemblies por meio de uma chamada de rede.</span><span class="sxs-lookup"><span data-stu-id="2369f-144">Uses JS interop to fetch assemblies via a network call.</span></span>
* <span data-ttu-id="2369f-145">Carrega assemblies no tempo de execução em execução no Webassembly no navegador.</span><span class="sxs-lookup"><span data-stu-id="2369f-145">Loads assemblies into the runtime executing on WebAssembly in the browser.</span></span>

> [!NOTE]
> <span data-ttu-id="2369f-146">A implementação de carregamento lento da estrutura dá suporte à renderização no servidor.</span><span class="sxs-lookup"><span data-stu-id="2369f-146">The framework's lazy loading implementation supports prerendering on the server.</span></span> <span data-ttu-id="2369f-147">Durante o pré-processamento, todos os assemblies, incluindo os marcados para carregamento lento, são considerados carregados.</span><span class="sxs-lookup"><span data-stu-id="2369f-147">During prerendering, all assemblies, including those marked for lazy loading, are assumed to be loaded.</span></span>

### <a name="user-interaction-with-navigating-content"></a><span data-ttu-id="2369f-148">Interação do usuário com `<Navigating>` conteúdo</span><span class="sxs-lookup"><span data-stu-id="2369f-148">User interaction with `<Navigating>` content</span></span>

<span data-ttu-id="2369f-149">Durante o carregamento de assemblies, que pode levar vários segundos, o `Router` componente pode indicar ao usuário que uma transição de página está ocorrendo:</span><span class="sxs-lookup"><span data-stu-id="2369f-149">While loading assemblies, which can take several seconds, the `Router` component can indicate to the user that a page transition is occurring:</span></span>

* <span data-ttu-id="2369f-150">Adicione uma [`@using`](xref:mvc/views/razor#using) diretiva para o <xref:Microsoft.AspNetCore.Components.Routing?displayProperty=fullName> namespace.</span><span class="sxs-lookup"><span data-stu-id="2369f-150">Add an [`@using`](xref:mvc/views/razor#using) directive for the <xref:Microsoft.AspNetCore.Components.Routing?displayProperty=fullName> namespace.</span></span>
* <span data-ttu-id="2369f-151">Adicione uma `<Navigating>` marca ao componente com marcação a ser exibida durante eventos de transição de página.</span><span class="sxs-lookup"><span data-stu-id="2369f-151">Add a `<Navigating>` tag to the component with markup to display during page transition events.</span></span>

```razor
...
@using Microsoft.AspNetCore.Components.Routing
...

<Router ...>
    <Navigating>
        <div style="...">
            <p>Loading the requested page&hellip;</p>
        </div>
    </Navigating>
</Router>

...
```

### <a name="handle-cancellations-in-onnavigateasync"></a><span data-ttu-id="2369f-152">Manipular cancelamentos em`OnNavigateAsync`</span><span class="sxs-lookup"><span data-stu-id="2369f-152">Handle cancellations in `OnNavigateAsync`</span></span>

<span data-ttu-id="2369f-153">O `NavigationContext` objeto passado para o `OnNavigateAsync` retorno de chamada contém um `CancellationToken` que é definido quando um novo evento de navegação ocorre.</span><span class="sxs-lookup"><span data-stu-id="2369f-153">The `NavigationContext` object passed to the `OnNavigateAsync` callback contains a `CancellationToken` that's set when a new navigation event occurs.</span></span> <span data-ttu-id="2369f-154">O `OnNavigateAsync` retorno de chamada deve gerar quando esse token de cancelamento é definido para evitar continuar a executar o `OnNavigateAsync` retorno de chamada em uma navegação desatualizada.</span><span class="sxs-lookup"><span data-stu-id="2369f-154">The `OnNavigateAsync` callback must throw when this cancellation token is set to avoid continuing to run the `OnNavigateAsync` callback on a outdated navigation.</span></span>

<span data-ttu-id="2369f-155">Se um usuário navegar para rotear a e, em seguida, imediatamente para a rota B, o aplicativo não deverá continuar executando o `OnNavigateAsync` retorno de chamada para a rota a:</span><span class="sxs-lookup"><span data-stu-id="2369f-155">If a user navigates to Route A and then immediately to Route B, the app shouldn't continue running the `OnNavigateAsync` callback for Route A:</span></span>

```razor
@inject HttpClient Http
@inject ProductCatalog Products

<Router AppAssembly="@typeof(Program).Assembly" 
    OnNavigateAsync="@OnNavigateAsync">
    ...
</Router>

@code {
    private async Task OnNavigateAsync(NavigationContext context)
    {
        if (context.Path == "/about") 
        {
            var stats = new Stats = { Page = "/about" };
            await Http.PostAsJsonAsync("api/visited", stats, context.CancellationToken);
        }
        else if (context.Path == "/store")
        {
            var productIds = [345, 789, 135, 689];

            foreach (var productId in productIds) 
            {
                context.CancellationToken.ThrowIfCancellationRequested();
                Products.Prefetch(productId);
            }
        }
    }
}
```

> [!NOTE]
> <span data-ttu-id="2369f-156">Não lançar se o token de cancelamento no `NavigationContext` for cancelado pode resultar em um comportamento indesejado, como a renderização de um componente de uma navegação anterior.</span><span class="sxs-lookup"><span data-stu-id="2369f-156">Not throwing if the cancellation token in `NavigationContext` is canceled can result in unintended behavior, such as rendering a component from a previous navigation.</span></span>

### <a name="complete-example"></a><span data-ttu-id="2369f-157">Exemplo completo</span><span class="sxs-lookup"><span data-stu-id="2369f-157">Complete example</span></span>

<span data-ttu-id="2369f-158">O componente completo a seguir `Router` demonstra como carregar o `GrantImaharaRobotControls.dll` assembly quando o usuário navega para `/robot` .</span><span class="sxs-lookup"><span data-stu-id="2369f-158">The following complete `Router` component demonstrates loading the `GrantImaharaRobotControls.dll` assembly when the user navigates to `/robot`.</span></span> <span data-ttu-id="2369f-159">Durante as transições de página, uma mensagem com estilo é exibida para o usuário.</span><span class="sxs-lookup"><span data-stu-id="2369f-159">During page transitions, a styled message is displayed to the user.</span></span>

```razor
@using System.Reflection
@using Microsoft.AspNetCore.Components.Routing
@using Microsoft.AspNetCore.Components.WebAssembly.Services
@inject LazyAssemblyLoader assemblyLoader

<Router AppAssembly="@typeof(Program).Assembly" 
    AdditionalAssemblies="@lazyLoadedAssemblies" OnNavigateAsync="@OnNavigateAsync">
    <Navigating>
        <div style="padding:20px;background-color:blue;color:white">
            <p>Loading the requested page&hellip;</p>
        </div>
    </Navigating>
    <Found Context="routeData">
        <RouteView RouteData="@routeData" DefaultLayout="@typeof(MainLayout)" />
    </Found>
    <NotFound>
        <LayoutView Layout="@typeof(MainLayout)">
            <p>Sorry, there's nothing at this address.</p>
        </LayoutView>
    </NotFound>
</Router>

@code {
    private List<Assembly> lazyLoadedAssemblies = new List<Assembly>();

    private async Task OnNavigateAsync(NavigationContext args)
    {
        try
        {
            if (args.Path.EndsWith("/robot"))
            {
                var assemblies = await assemblyLoader.LoadAssembliesAsync(
                    new List<string>() { "GrantImaharaRobotControls.dll" });
                lazyLoadedAssemblies.AddRange(assemblies);
            }
        }
        catch (Exception ex)
        {
            ...
        }
    }
}
```

## <a name="troubleshoot"></a><span data-ttu-id="2369f-160">Solução de problemas</span><span class="sxs-lookup"><span data-stu-id="2369f-160">Troubleshoot</span></span>

* <span data-ttu-id="2369f-161">Se ocorrer uma renderização inesperada (por exemplo, um componente de uma navegação anterior é renderizado), confirme se o código será gerado se o token de cancelamento for definido.</span><span class="sxs-lookup"><span data-stu-id="2369f-161">If unexpected rendering occurs (for example, a component from a previous navigation is rendered), confirm that the code throws if the cancellation token is set.</span></span>
* <span data-ttu-id="2369f-162">Se os assemblies ainda estiverem carregados no início do aplicativo, verifique se o assembly está marcado como carregado como lento no arquivo de projeto.</span><span class="sxs-lookup"><span data-stu-id="2369f-162">If assemblies are still loaded at application start, check that the assembly is marked as lazy loaded in the project file.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2369f-163">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="2369f-163">Additional resources</span></span>

* <xref:blazor/webassembly-performance-best-practices>