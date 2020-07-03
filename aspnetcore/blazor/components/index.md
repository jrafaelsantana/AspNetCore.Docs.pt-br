---
title: Criar e usar componentes de ASP.NET Core Razor
author: guardrex
description: Saiba como criar e usar Razor componentes, incluindo como associar dados, manipular eventos e gerenciar ciclos de vida do componente.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 06/25/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/components/index
ms.openlocfilehash: 02e3f7f5442a5abde0b13b7bba14d9d0f29c1de7
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/26/2020
ms.locfileid: "85399082"
---
# <a name="create-and-use-aspnet-core-razor-components"></a><span data-ttu-id="d3ef6-103">Criar e usar componentes de ASP.NET Core Razor</span><span class="sxs-lookup"><span data-stu-id="d3ef6-103">Create and use ASP.NET Core Razor components</span></span>

<span data-ttu-id="d3ef6-104">Por [Luke Latham](https://github.com/guardrex), [Daniel Roth](https://github.com/danroth27)e [Tobias Bartsch](https://www.aveo-solutions.com/)</span><span class="sxs-lookup"><span data-stu-id="d3ef6-104">By [Luke Latham](https://github.com/guardrex), [Daniel Roth](https://github.com/danroth27), and [Tobias Bartsch](https://www.aveo-solutions.com/)</span></span>

<span data-ttu-id="d3ef6-105">[Exibir ou baixar código de exemplo](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/../common/samples/) ([como baixar](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d3ef6-105">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/../common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

Blazor<span data-ttu-id="d3ef6-106">os aplicativos são criados usando *componentes*.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-106"> apps are built using *components*.</span></span> <span data-ttu-id="d3ef6-107">Um componente é uma parte independente da interface do usuário (IU), como uma página, uma caixa de diálogo ou um formulário.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-107">A component is a self-contained chunk of user interface (UI), such as a page, dialog, or form.</span></span> <span data-ttu-id="d3ef6-108">Um componente inclui marcação HTML e a lógica de processamento necessária para injetar dados ou responder a eventos da interface do usuário.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-108">A component includes HTML markup and the processing logic required to inject data or respond to UI events.</span></span> <span data-ttu-id="d3ef6-109">Os componentes são flexíveis e leves.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-109">Components are flexible and lightweight.</span></span> <span data-ttu-id="d3ef6-110">Eles podem ser aninhados, reutilizados e compartilhados entre projetos.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-110">They can be nested, reused, and shared among projects.</span></span>

## <a name="component-classes"></a><span data-ttu-id="d3ef6-111">Classes de componente</span><span class="sxs-lookup"><span data-stu-id="d3ef6-111">Component classes</span></span>

<span data-ttu-id="d3ef6-112">Os componentes são implementados em [Razor](xref:mvc/views/razor) arquivos de componente ( `.razor` ) usando uma combinação de C# e marcação HTML.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-112">Components are implemented in [Razor](xref:mvc/views/razor) component files (`.razor`) using a combination of C# and HTML markup.</span></span> <span data-ttu-id="d3ef6-113">Um componente no Blazor é conhecido formalmente como um \* Razor componente\*.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-113">A component in Blazor is formally referred to as a *Razor component*.</span></span>

### <a name="razor-syntax"></a><span data-ttu-id="d3ef6-114">Sintaxe de Razor</span><span class="sxs-lookup"><span data-stu-id="d3ef6-114">Razor syntax</span></span>

Razor<span data-ttu-id="d3ef6-115">os componentes em Blazor aplicativos usam extensivamente a Razor sintaxe.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-115"> components in Blazor apps extensively use Razor syntax.</span></span> <span data-ttu-id="d3ef6-116">Se você não estiver familiarizado com a Razor linguagem de marcação, recomendamos a leitura <xref:mvc/views/razor> antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-116">If you aren't familiar with the Razor markup language, we recommend reading <xref:mvc/views/razor> before proceeding.</span></span>

<span data-ttu-id="d3ef6-117">Ao acessar o conteúdo em Razor sintaxe, preste atenção especial às seguintes seções:</span><span class="sxs-lookup"><span data-stu-id="d3ef6-117">When accessing the content on Razor syntax, pay special attention to the following sections:</span></span>

* <span data-ttu-id="d3ef6-118">[Diretivas](xref:mvc/views/razor#directives): `@` -palavras-chave reservadas prefixadas que normalmente alteram a maneira como a marcação de componente é analisada ou funciona.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-118">[Directives](xref:mvc/views/razor#directives): `@`-prefixed reserved keywords that typically change the way component markup is parsed or function.</span></span>
* <span data-ttu-id="d3ef6-119">[Atributos de diretiva](xref:mvc/views/razor#directive-attributes): `@` -palavras-chave reservadas prefixadas que normalmente alteram a maneira como os elementos de componente são analisados ou funcionam.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-119">[Directive attributes](xref:mvc/views/razor#directive-attributes): `@`-prefixed reserved keywords that typically change the way component elements are parsed or function.</span></span>

### <a name="names"></a><span data-ttu-id="d3ef6-120">Names</span><span class="sxs-lookup"><span data-stu-id="d3ef6-120">Names</span></span>

<span data-ttu-id="d3ef6-121">O nome de um componente deve começar com um caractere maiúsculo.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-121">A component's name must start with an uppercase character.</span></span> <span data-ttu-id="d3ef6-122">Por exemplo, `MyCoolComponent.razor` é válido e `myCoolComponent.razor` é inválido.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-122">For example, `MyCoolComponent.razor` is valid, and `myCoolComponent.razor` is invalid.</span></span>

### <a name="routing"></a><span data-ttu-id="d3ef6-123">Roteamento</span><span class="sxs-lookup"><span data-stu-id="d3ef6-123">Routing</span></span>

<span data-ttu-id="d3ef6-124">O roteamento no Blazor é obtido fornecendo um modelo de rota para cada componente acessível no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-124">Routing in Blazor is achieved by providing a route template to each accessible component in the app.</span></span> <span data-ttu-id="d3ef6-125">Quando um Razor arquivo com uma [`@page`][9] diretiva é compilado, a classe gerada recebe um <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> especificando o modelo de rota.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-125">When a Razor file with an [`@page`][9] directive is compiled, the generated class is given a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span> <span data-ttu-id="d3ef6-126">Em tempo de execução, o roteador procura classes de componentes com um <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> e renderiza qualquer componente que tenha um modelo de rota que corresponda à URL solicitada.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-126">At runtime, the router looks for component classes with a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> and renders whichever component has a route template that matches the requested URL.</span></span> <span data-ttu-id="d3ef6-127">Para obter mais informações, consulte <xref:blazor/fundamentals/routing>.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-127">For more information, see <xref:blazor/fundamentals/routing>.</span></span>

```razor
@page "/ParentComponent"

...
```

### <a name="markup"></a><span data-ttu-id="d3ef6-128">Marcação</span><span class="sxs-lookup"><span data-stu-id="d3ef6-128">Markup</span></span>

<span data-ttu-id="d3ef6-129">A interface do usuário para um componente é definida usando HTML.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-129">The UI for a component is defined using HTML.</span></span> <span data-ttu-id="d3ef6-130">A lógica de renderização dinâmica (por exemplo, loops, condicionais, expressões) é adicionada usando uma sintaxe C# inserida chamada *Razor* .</span><span class="sxs-lookup"><span data-stu-id="d3ef6-130">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called *Razor*.</span></span> <span data-ttu-id="d3ef6-131">Quando um aplicativo é compilado, a marcação HTML e a lógica de renderização C# são convertidas em uma classe de componente.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-131">When an app is compiled, the HTML markup and C# rendering logic are converted into a component class.</span></span> <span data-ttu-id="d3ef6-132">O nome da classe gerada corresponde ao nome do arquivo.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-132">The name of the generated class matches the name of the file.</span></span>

<span data-ttu-id="d3ef6-133">Os membros da classe de componente são definidos em um [`@code`][1] bloco.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-133">Members of the component class are defined in an [`@code`][1] block.</span></span> <span data-ttu-id="d3ef6-134">No [`@code`][1] bloco, estado do componente (Propriedades, campos) é especificado com métodos para manipulação de eventos ou para definir outra lógica de componente.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-134">In the [`@code`][1] block, component state (properties, fields) is specified with methods for event handling or for defining other component logic.</span></span> <span data-ttu-id="d3ef6-135">Mais de um [`@code`][1] bloco é permitido.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-135">More than one [`@code`][1] block is permissible.</span></span>

<span data-ttu-id="d3ef6-136">Os membros do componente podem ser usados como parte da lógica de renderização do componente usando expressões C# que começam com `@` .</span><span class="sxs-lookup"><span data-stu-id="d3ef6-136">Component members can be used as part of the component's rendering logic using C# expressions that start with `@`.</span></span> <span data-ttu-id="d3ef6-137">Por exemplo, um campo C# é renderizado pela prefixação `@` para o nome do campo.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-137">For example, a C# field is rendered by prefixing `@` to the field name.</span></span> <span data-ttu-id="d3ef6-138">O exemplo a seguir avalia e renderiza:</span><span class="sxs-lookup"><span data-stu-id="d3ef6-138">The following example evaluates and renders:</span></span>

* <span data-ttu-id="d3ef6-139">`headingFontStyle`para o valor da propriedade de CSS para `font-style` .</span><span class="sxs-lookup"><span data-stu-id="d3ef6-139">`headingFontStyle` to the CSS property value for `font-style`.</span></span>
* <span data-ttu-id="d3ef6-140">`headingText`para o conteúdo do `<h1>` elemento.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-140">`headingText` to the content of the `<h1>` element.</span></span>

```razor
<h1 style="font-style:@headingFontStyle">@headingText</h1>

@code {
    private string headingFontStyle = "italic";
    private string headingText = "Put on your new Blazor!";
}
```

<span data-ttu-id="d3ef6-141">Depois que o componente é processado inicialmente, o componente regenera sua árvore de renderização em resposta a eventos.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-141">After the component is initially rendered, the component regenerates its render tree in response to events.</span></span> Blazor<span data-ttu-id="d3ef6-142">em seguida, compara a nova árvore de renderização com a anterior e aplica quaisquer modificações ao Modelo de Objeto do Documento do navegador (DOM).</span><span class="sxs-lookup"><span data-stu-id="d3ef6-142"> then compares the new render tree against the previous one and applies any modifications to the browser's Document Object Model (DOM).</span></span>

<span data-ttu-id="d3ef6-143">Os componentes são classes C# comuns e podem ser colocados em qualquer lugar dentro de um projeto.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-143">Components are ordinary C# classes and can be placed anywhere within a project.</span></span> <span data-ttu-id="d3ef6-144">Os componentes que produzem páginas da Web geralmente residem na `Pages` pasta.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-144">Components that produce webpages usually reside in the `Pages` folder.</span></span> <span data-ttu-id="d3ef6-145">Os componentes que não são de página são frequentemente colocados na `Shared` pasta ou em uma pasta personalizada adicionada ao projeto.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-145">Non-page components are frequently placed in the `Shared` folder or a custom folder added to the project.</span></span>

### <a name="namespaces"></a><span data-ttu-id="d3ef6-146">Namespaces</span><span class="sxs-lookup"><span data-stu-id="d3ef6-146">Namespaces</span></span>

<span data-ttu-id="d3ef6-147">Normalmente, o namespace de um componente é derivado do namespace raiz do aplicativo e do local do componente (pasta) no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-147">Typically, a component's namespace is derived from the app's root namespace and the component's location (folder) within the app.</span></span> <span data-ttu-id="d3ef6-148">Se o namespace raiz do aplicativo for `BlazorApp` e o `Counter` componente residir na `Pages` pasta:</span><span class="sxs-lookup"><span data-stu-id="d3ef6-148">If the app's root namespace is `BlazorApp` and the `Counter` component resides in the `Pages` folder:</span></span>

* <span data-ttu-id="d3ef6-149">O `Counter` namespace do componente é `BlazorApp.Pages` .</span><span class="sxs-lookup"><span data-stu-id="d3ef6-149">The `Counter` component's namespace is `BlazorApp.Pages`.</span></span>
* <span data-ttu-id="d3ef6-150">O nome do tipo totalmente qualificado do componente é `BlazorApp.Pages.Counter` .</span><span class="sxs-lookup"><span data-stu-id="d3ef6-150">The fully qualified type name of the component is `BlazorApp.Pages.Counter`.</span></span>

<span data-ttu-id="d3ef6-151">Para pastas personalizadas que contêm componentes, adicione uma [`@using`][2] diretiva ao componente pai ou ao arquivo do aplicativo `_Imports.razor` .</span><span class="sxs-lookup"><span data-stu-id="d3ef6-151">For custom folders that hold components, add a [`@using`][2] directive to the parent component or to the app's `_Imports.razor` file.</span></span> <span data-ttu-id="d3ef6-152">O exemplo a seguir torna os componentes na `Components` pasta disponíveis:</span><span class="sxs-lookup"><span data-stu-id="d3ef6-152">The following example makes components in the `Components` folder available:</span></span>

```razor
@using BlazorApp.Components
```

<span data-ttu-id="d3ef6-153">Os componentes também podem ser referenciados usando seus nomes totalmente qualificados, o que não exige a [`@using`][2] diretiva:</span><span class="sxs-lookup"><span data-stu-id="d3ef6-153">Components can also be referenced using their fully qualified names, which doesn't require the [`@using`][2] directive:</span></span>

```razor
<BlazorSample.Components.MyComponent />
```

<span data-ttu-id="d3ef6-154">O namespace de um componente criado com o Razor é baseado em (em ordem de prioridade):</span><span class="sxs-lookup"><span data-stu-id="d3ef6-154">The namespace of a component authored with Razor is based on (in priority order):</span></span>

* <span data-ttu-id="d3ef6-155">[`@namespace`][8]designação na Razor marcação File ( `.razor` ) ( `@namespace BlazorSample.MyNamespace` ).</span><span class="sxs-lookup"><span data-stu-id="d3ef6-155">[`@namespace`][8] designation in Razor file (`.razor`) markup (`@namespace BlazorSample.MyNamespace`).</span></span>
* <span data-ttu-id="d3ef6-156">O projeto `RootNamespace` no arquivo de projeto ( `<RootNamespace>BlazorSample</RootNamespace>` ).</span><span class="sxs-lookup"><span data-stu-id="d3ef6-156">The project's `RootNamespace` in the project file (`<RootNamespace>BlazorSample</RootNamespace>`).</span></span>
* <span data-ttu-id="d3ef6-157">O nome do projeto, extraído do nome de arquivo do arquivo de projeto ( `.csproj` ) e o caminho da raiz do projeto para o componente.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-157">The project name, taken from the project file's file name (`.csproj`), and the path from the project root to the component.</span></span> <span data-ttu-id="d3ef6-158">Por exemplo, a estrutura resolve `{PROJECT ROOT}/Pages/Index.razor` ( `BlazorSample.csproj` ) para o namespace `BlazorSample.Pages` .</span><span class="sxs-lookup"><span data-stu-id="d3ef6-158">For example, the framework resolves `{PROJECT ROOT}/Pages/Index.razor` (`BlazorSample.csproj`) to the namespace `BlazorSample.Pages`.</span></span> <span data-ttu-id="d3ef6-159">Os componentes seguem regras de associação de nome C#.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-159">Components follow C# name binding rules.</span></span> <span data-ttu-id="d3ef6-160">Para o `Index` componente neste exemplo, os componentes no escopo são todos os componentes:</span><span class="sxs-lookup"><span data-stu-id="d3ef6-160">For the `Index` component in this example, the components in scope are all of the components:</span></span>
  * <span data-ttu-id="d3ef6-161">Na mesma pasta, `Pages` .</span><span class="sxs-lookup"><span data-stu-id="d3ef6-161">In the same folder, `Pages`.</span></span>
  * <span data-ttu-id="d3ef6-162">Os componentes na raiz do projeto que não especificam explicitamente um namespace diferente.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-162">The components in the project's root that don't explicitly specify a different namespace.</span></span>

> [!NOTE]
> <span data-ttu-id="d3ef6-163">`global::`Não há suporte para a qualificação.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-163">The `global::` qualification isn't supported.</span></span>
>
> <span data-ttu-id="d3ef6-164">[`using`](/dotnet/csharp/language-reference/keywords/using-statement)Não há suporte para a importação de componentes com instruções com alias (por exemplo, `@using Foo = Bar` ).</span><span class="sxs-lookup"><span data-stu-id="d3ef6-164">Importing components with aliased [`using`](/dotnet/csharp/language-reference/keywords/using-statement) statements (for example, `@using Foo = Bar`) isn't supported.</span></span>
>
> <span data-ttu-id="d3ef6-165">Não há suporte para nomes parcialmente qualificados.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-165">Partially qualified names aren't supported.</span></span> <span data-ttu-id="d3ef6-166">Por exemplo, `@using BlazorSample` não há suporte para adicionar e referenciar o `NavMenu` componente ( `NavMenu.razor` ) com `<Shared.NavMenu></Shared.NavMenu>` .</span><span class="sxs-lookup"><span data-stu-id="d3ef6-166">For example, adding `@using BlazorSample` and referencing the `NavMenu` component (`NavMenu.razor`) with `<Shared.NavMenu></Shared.NavMenu>` isn't supported.</span></span>

### <a name="partial-class-support"></a><span data-ttu-id="d3ef6-167">Suporte de classe parcial</span><span class="sxs-lookup"><span data-stu-id="d3ef6-167">Partial class support</span></span>

Razor<span data-ttu-id="d3ef6-168">os componentes são gerados como classes parciais.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-168"> components are generated as partial classes.</span></span> Razor<span data-ttu-id="d3ef6-169">os componentes são criados usando uma das seguintes abordagens:</span><span class="sxs-lookup"><span data-stu-id="d3ef6-169"> components are authored using either of the following approaches:</span></span>

* <span data-ttu-id="d3ef6-170">O código C# é definido em um [`@code`][1] bloco com marcação e Razor código HTML em um único arquivo.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-170">C# code is defined in an [`@code`][1] block with HTML markup and Razor code in a single file.</span></span> Blazor<span data-ttu-id="d3ef6-171">os modelos definem seus Razor componentes usando essa abordagem.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-171"> templates define their Razor components using this approach.</span></span>
* <span data-ttu-id="d3ef6-172">O código C# é colocado em um arquivo code-behind definido como uma classe parcial.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-172">C# code is placed in a code-behind file defined as a partial class.</span></span>

<span data-ttu-id="d3ef6-173">O exemplo a seguir mostra o `Counter` componente padrão com um [`@code`][1] bloco em um aplicativo gerado a partir de um Blazor modelo.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-173">The following example shows the default `Counter` component with an [`@code`][1] block in an app generated from a Blazor template.</span></span> <span data-ttu-id="d3ef6-174">A marcação HTML, o Razor código e o código C# estão no mesmo arquivo:</span><span class="sxs-lookup"><span data-stu-id="d3ef6-174">HTML markup, Razor code, and C# code are in the same file:</span></span>

<span data-ttu-id="d3ef6-175">`Pages/Counter.razor`:</span><span class="sxs-lookup"><span data-stu-id="d3ef6-175">`Pages/Counter.razor`:</span></span>

```razor
@page "/counter"

<h1>Counter</h1>

<p>Current count: @currentCount</p>

<button class="btn btn-primary" @onclick="IncrementCount">Click me</button>

@code {
    private int currentCount = 0;

    void IncrementCount()
    {
        currentCount++;
    }
}
```

<span data-ttu-id="d3ef6-176">O `Counter` componente também pode ser criado usando um arquivo code-behind com uma classe parcial:</span><span class="sxs-lookup"><span data-stu-id="d3ef6-176">The `Counter` component can also be created using a code-behind file with a partial class:</span></span>

<span data-ttu-id="d3ef6-177">`Pages/Counter.razor`:</span><span class="sxs-lookup"><span data-stu-id="d3ef6-177">`Pages/Counter.razor`:</span></span>

```razor
@page "/counter"

<h1>Counter</h1>

<p>Current count: @currentCount</p>

<button class="btn btn-primary" @onclick="IncrementCount">Click me</button>
```

<span data-ttu-id="d3ef6-178">`Counter.razor.cs`:</span><span class="sxs-lookup"><span data-stu-id="d3ef6-178">`Counter.razor.cs`:</span></span>

```csharp
namespace BlazorApp.Pages
{
    public partial class Counter
    {
        private int currentCount = 0;

        void IncrementCount()
        {
            currentCount++;
        }
    }
}
```

<span data-ttu-id="d3ef6-179">Adicione quaisquer namespaces necessários ao arquivo de classe parcial, conforme necessário.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-179">Add any required namespaces to the partial class file as needed.</span></span> <span data-ttu-id="d3ef6-180">Os namespaces típicos usados pelos Razor componentes incluem:</span><span class="sxs-lookup"><span data-stu-id="d3ef6-180">Typical namespaces used by Razor components include:</span></span>

```csharp
using Microsoft.AspNetCore.Authorization;
using Microsoft.AspNetCore.Components;
using Microsoft.AspNetCore.Components.Authorization;
using Microsoft.AspNetCore.Components.Forms;
using Microsoft.AspNetCore.Components.Routing;
using Microsoft.AspNetCore.Components.Web;
```

### <a name="specify-a-base-class"></a><span data-ttu-id="d3ef6-181">Especificar uma classe base</span><span class="sxs-lookup"><span data-stu-id="d3ef6-181">Specify a base class</span></span>

<span data-ttu-id="d3ef6-182">A [`@inherits`][6] diretiva pode ser usada para especificar uma classe base para um componente.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-182">The [`@inherits`][6] directive can be used to specify a base class for a component.</span></span> <span data-ttu-id="d3ef6-183">O exemplo a seguir mostra como um componente pode herdar uma classe base, `BlazorRocksBase` ,, para fornecer as propriedades e os métodos do componente.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-183">The following example shows how a component can inherit a base class, `BlazorRocksBase`, to provide the component's properties and methods.</span></span> <span data-ttu-id="d3ef6-184">A classe base deve derivar de <xref:Microsoft.AspNetCore.Components.ComponentBase> .</span><span class="sxs-lookup"><span data-stu-id="d3ef6-184">The base class should derive from <xref:Microsoft.AspNetCore.Components.ComponentBase>.</span></span>

<span data-ttu-id="d3ef6-185">`Pages/BlazorRocks.razor`:</span><span class="sxs-lookup"><span data-stu-id="d3ef6-185">`Pages/BlazorRocks.razor`:</span></span>

```razor
@page "/BlazorRocks"
@inherits BlazorRocksBase

<h1>@BlazorRocksText</h1>
```

<span data-ttu-id="d3ef6-186">`BlazorRocksBase.cs`:</span><span class="sxs-lookup"><span data-stu-id="d3ef6-186">`BlazorRocksBase.cs`:</span></span>

```csharp
using Microsoft.AspNetCore.Components;

namespace BlazorSample
{
    public class BlazorRocksBase : ComponentBase
    {
        public string BlazorRocksText { get; set; } = 
            "Blazor rocks the browser!";
    }
}
```

### <a name="use-components"></a><span data-ttu-id="d3ef6-187">Usar componentes</span><span class="sxs-lookup"><span data-stu-id="d3ef6-187">Use components</span></span>

<span data-ttu-id="d3ef6-188">Os componentes podem incluir outros componentes, declarando-os usando a sintaxe do elemento HTML.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-188">Components can include other components by declaring them using HTML element syntax.</span></span> <span data-ttu-id="d3ef6-189">A marcação para uso de um componente é semelhante a uma marca HTML, em que o nome da marca é o tipo de componente.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-189">The markup for using a component looks like an HTML tag where the name of the tag is the component type.</span></span>

<span data-ttu-id="d3ef6-190">A seguinte marcação no `Pages/Index.razor` renderiza uma `HeadingComponent` instância:</span><span class="sxs-lookup"><span data-stu-id="d3ef6-190">The following markup in `Pages/Index.razor` renders a `HeadingComponent` instance:</span></span>

```razor
<HeadingComponent />
```

<span data-ttu-id="d3ef6-191">`Components/HeadingComponent.razor`:</span><span class="sxs-lookup"><span data-stu-id="d3ef6-191">`Components/HeadingComponent.razor`:</span></span>

[!code-razor[](index/samples_snapshot/HeadingComponent.razor)]

<span data-ttu-id="d3ef6-192">Se um componente contiver um elemento HTML com uma letra maiúscula ou minúscula que não corresponda a um nome de componente, um aviso será emitido indicando que o elemento tem um nome inesperado.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-192">If a component contains an HTML element with an uppercase first letter that doesn't match a component name, a warning is emitted indicating that the element has an unexpected name.</span></span> <span data-ttu-id="d3ef6-193">A adição de uma [`@using`][2] diretiva para o namespace do componente torna o componente disponível, o que resolve o aviso.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-193">Adding an [`@using`][2] directive for the component's namespace makes the component available, which resolves the warning.</span></span>

## <a name="parameters"></a><span data-ttu-id="d3ef6-194">Parâmetros</span><span class="sxs-lookup"><span data-stu-id="d3ef6-194">Parameters</span></span>

### <a name="route-parameters"></a><span data-ttu-id="d3ef6-195">Parâmetros de rota</span><span class="sxs-lookup"><span data-stu-id="d3ef6-195">Route parameters</span></span>

<span data-ttu-id="d3ef6-196">Os componentes podem receber parâmetros de rota do modelo de rota fornecido na [`@page`][9] diretiva.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-196">Components can receive route parameters from the route template provided in the [`@page`][9] directive.</span></span> <span data-ttu-id="d3ef6-197">O roteador usa parâmetros de rota para preencher os parâmetros de componente correspondentes.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-197">The router uses route parameters to populate the corresponding component parameters.</span></span>

<span data-ttu-id="d3ef6-198">`Pages/RouteParameter.razor`:</span><span class="sxs-lookup"><span data-stu-id="d3ef6-198">`Pages/RouteParameter.razor`:</span></span>

[!code-razor[](index/samples_snapshot/RouteParameter.razor?highlight=2,7-8)]

<span data-ttu-id="d3ef6-199">Não há suporte para parâmetros opcionais, portanto, duas [`@page`][9] diretivas são aplicadas no exemplo anterior.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-199">Optional parameters aren't supported, so two [`@page`][9] directives are applied in the preceding example.</span></span> <span data-ttu-id="d3ef6-200">O primeiro permite a navegação para o componente sem um parâmetro.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-200">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="d3ef6-201">A segunda [`@page`][9] diretiva recebe o `{text}` parâmetro de rota e atribui o valor à `Text` propriedade.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-201">The second [`@page`][9] directive receives the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

<span data-ttu-id="d3ef6-202">A sintaxe de parâmetro *catch-all* ( `*` / `**` ), que captura o caminho entre vários limites de pasta, **não** tem suporte nos Razor componentes ( `.razor` ).</span><span class="sxs-lookup"><span data-stu-id="d3ef6-202">*Catch-all* parameter syntax (`*`/`**`), which captures the path across multiple folder boundaries, is **not** supported in Razor components (`.razor`).</span></span>

### <a name="component-parameters"></a><span data-ttu-id="d3ef6-203">Parâmetros do componente</span><span class="sxs-lookup"><span data-stu-id="d3ef6-203">Component parameters</span></span>

<span data-ttu-id="d3ef6-204">Os componentes podem ter *parâmetros de componente*, que são definidos usando propriedades públicas na classe de componente com o [`[Parameter]`](xref:Microsoft.AspNetCore.Components.ParameterAttribute) atributo.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-204">Components can have *component parameters*, which are defined using public properties on the component class with the [`[Parameter]`](xref:Microsoft.AspNetCore.Components.ParameterAttribute) attribute.</span></span> <span data-ttu-id="d3ef6-205">Use atributos para especificar argumentos para um componente na marcação.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-205">Use attributes to specify arguments for a component in markup.</span></span>

<span data-ttu-id="d3ef6-206">`Components/ChildComponent.razor`:</span><span class="sxs-lookup"><span data-stu-id="d3ef6-206">`Components/ChildComponent.razor`:</span></span>

[!code-razor[](../common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=2,11-12)]

<span data-ttu-id="d3ef6-207">No exemplo a seguir do aplicativo de exemplo, o `ParentComponent` define o valor da `Title` Propriedade do `ChildComponent` .</span><span class="sxs-lookup"><span data-stu-id="d3ef6-207">In the following example from the sample app, the `ParentComponent` sets the value of the `Title` property of the `ChildComponent`.</span></span>

<span data-ttu-id="d3ef6-208">`Pages/ParentComponent.razor`:</span><span class="sxs-lookup"><span data-stu-id="d3ef6-208">`Pages/ParentComponent.razor`:</span></span>

[!code-razor[](index/samples_snapshot/ParentComponent.razor?highlight=5-6)]

> [!WARNING]
> <span data-ttu-id="d3ef6-209">Não crie componentes que gravam em seus próprios *parâmetros de componente*, use um campo particular em vez disso.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-209">Don't create components that write to their own *component parameters*, use a private field instead.</span></span> <span data-ttu-id="d3ef6-210">Para obter mais informações, consulte a seção [não criar componentes que gravam em suas próprias propriedades de parâmetro](#dont-create-components-that-write-to-their-own-parameter-properties) .</span><span class="sxs-lookup"><span data-stu-id="d3ef6-210">For more information, see the [Don't create components that write to their own parameter properties](#dont-create-components-that-write-to-their-own-parameter-properties) section.</span></span>

## <a name="child-content"></a><span data-ttu-id="d3ef6-211">Conteúdo filho</span><span class="sxs-lookup"><span data-stu-id="d3ef6-211">Child content</span></span>

<span data-ttu-id="d3ef6-212">Os componentes podem definir o conteúdo de outro componente.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-212">Components can set the content of another component.</span></span> <span data-ttu-id="d3ef6-213">O componente de atribuição fornece o conteúdo entre as marcas que especificam o componente de recebimento.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-213">The assigning component provides the content between the tags that specify the receiving component.</span></span>

<span data-ttu-id="d3ef6-214">No exemplo a seguir, o `ChildComponent` tem uma `ChildContent` propriedade que representa um <xref:Microsoft.AspNetCore.Components.RenderFragment> , que representa um segmento de interface do usuário a ser renderizado.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-214">In the following example, the `ChildComponent` has a `ChildContent` property that represents a <xref:Microsoft.AspNetCore.Components.RenderFragment>, which represents a segment of UI to render.</span></span> <span data-ttu-id="d3ef6-215">O valor de `ChildContent` é posicionado na marcação do componente onde o conteúdo deve ser renderizado.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-215">The value of `ChildContent` is positioned in the component's markup where the content should be rendered.</span></span> <span data-ttu-id="d3ef6-216">O valor de `ChildContent` é recebido do componente pai e renderizado dentro do painel de inicialização `panel-body` .</span><span class="sxs-lookup"><span data-stu-id="d3ef6-216">The value of `ChildContent` is received from the parent component and rendered inside the Bootstrap panel's `panel-body`.</span></span>

<span data-ttu-id="d3ef6-217">`Components/ChildComponent.razor`:</span><span class="sxs-lookup"><span data-stu-id="d3ef6-217">`Components/ChildComponent.razor`:</span></span>

[!code-razor[](../common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=3,14-15)]

> [!NOTE]
> <span data-ttu-id="d3ef6-218">A propriedade que recebe o <xref:Microsoft.AspNetCore.Components.RenderFragment> conteúdo deve ser nomeada `ChildContent` por convenção.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-218">The property receiving the <xref:Microsoft.AspNetCore.Components.RenderFragment> content must be named `ChildContent` by convention.</span></span>

<span data-ttu-id="d3ef6-219">O `ParentComponent` no aplicativo de exemplo pode fornecer conteúdo para renderizar o `ChildComponent` colocando o conteúdo dentro das `<ChildComponent>` marcas.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-219">The `ParentComponent` in the sample app can provide content for rendering the `ChildComponent` by placing the content inside the `<ChildComponent>` tags.</span></span>

<span data-ttu-id="d3ef6-220">`Pages/ParentComponent.razor`:</span><span class="sxs-lookup"><span data-stu-id="d3ef6-220">`Pages/ParentComponent.razor`:</span></span>

[!code-razor[](index/samples_snapshot/ParentComponent.razor?highlight=7-8)]

## <a name="attribute-splatting-and-arbitrary-parameters"></a><span data-ttu-id="d3ef6-221">Atributo nivelamento e parâmetros arbitrários</span><span class="sxs-lookup"><span data-stu-id="d3ef6-221">Attribute splatting and arbitrary parameters</span></span>

<span data-ttu-id="d3ef6-222">Os componentes podem capturar e renderizar atributos adicionais além dos parâmetros declarados do componente.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-222">Components can capture and render additional attributes in addition to the component's declared parameters.</span></span> <span data-ttu-id="d3ef6-223">Atributos adicionais podem ser capturados em um dicionário e, em seguida, *splatted* em um elemento quando o componente é renderizado usando a [`@attributes`][3] Razor diretiva.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-223">Additional attributes can be captured in a dictionary and then *splatted* onto an element when the component is rendered using the [`@attributes`][3] Razor directive.</span></span> <span data-ttu-id="d3ef6-224">Esse cenário é útil ao definir um componente que produz um elemento de marcação que dá suporte a uma variedade de personalizações.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-224">This scenario is useful when defining a component that produces a markup element that supports a variety of customizations.</span></span> <span data-ttu-id="d3ef6-225">Por exemplo, pode ser entediante definir atributos separadamente para um `<input>` que dê suporte a muitos parâmetros.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-225">For example, it can be tedious to define attributes separately for an `<input>` that supports many parameters.</span></span>

<span data-ttu-id="d3ef6-226">No exemplo a seguir, o primeiro `<input>` elemento ( `id="useIndividualParams"` ) usa parâmetros de componente individuais, enquanto o segundo `<input>` elemento ( `id="useAttributesDict"` ) usa o atributo nivelamento:</span><span class="sxs-lookup"><span data-stu-id="d3ef6-226">In the following example, the first `<input>` element (`id="useIndividualParams"`) uses individual component parameters, while the second `<input>` element (`id="useAttributesDict"`) uses attribute splatting:</span></span>

```razor
<input id="useIndividualParams"
       maxlength="@Maxlength"
       placeholder="@Placeholder"
       required="@Required"
       size="@Size" />

<input id="useAttributesDict"
       @attributes="InputAttributes" />

@code {
    [Parameter]
    public string Maxlength { get; set; } = "10";

    [Parameter]
    public string Placeholder { get; set; } = "Input placeholder text";

    [Parameter]
    public string Required { get; set; } = "required";

    [Parameter]
    public string Size { get; set; } = "50";

    [Parameter]
    public Dictionary<string, object> InputAttributes { get; set; } =
        new Dictionary<string, object>()
        {
            { "maxlength", "10" },
            { "placeholder", "Input placeholder text" },
            { "required", "required" },
            { "size", "50" }
        };
}
```

<span data-ttu-id="d3ef6-227">O tipo do parâmetro deve implementar `IEnumerable<KeyValuePair<string, object>>` com chaves de cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-227">The type of the parameter must implement `IEnumerable<KeyValuePair<string, object>>` with string keys.</span></span> <span data-ttu-id="d3ef6-228">Usar `IReadOnlyDictionary<string, object>` também é uma opção nesse cenário.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-228">Using `IReadOnlyDictionary<string, object>` is also an option in this scenario.</span></span>

<span data-ttu-id="d3ef6-229">Os elementos renderizados `<input>` usando ambas as abordagens são idênticos:</span><span class="sxs-lookup"><span data-stu-id="d3ef6-229">The rendered `<input>` elements using both approaches is identical:</span></span>

```html
<input id="useIndividualParams"
       maxlength="10"
       placeholder="Input placeholder text"
       required="required"
       size="50">

<input id="useAttributesDict"
       maxlength="10"
       placeholder="Input placeholder text"
       required="required"
       size="50">
```

<span data-ttu-id="d3ef6-230">Para aceitar atributos arbitrários, defina um parâmetro de componente usando o [`[Parameter]`](xref:Microsoft.AspNetCore.Components.ParameterAttribute) atributo com a <xref:Microsoft.AspNetCore.Components.ParameterAttribute.CaptureUnmatchedValues> propriedade definida como `true` :</span><span class="sxs-lookup"><span data-stu-id="d3ef6-230">To accept arbitrary attributes, define a component parameter using the [`[Parameter]`](xref:Microsoft.AspNetCore.Components.ParameterAttribute) attribute with the <xref:Microsoft.AspNetCore.Components.ParameterAttribute.CaptureUnmatchedValues> property set to `true`:</span></span>

```razor
@code {
    [Parameter(CaptureUnmatchedValues = true)]
    public Dictionary<string, object> InputAttributes { get; set; }
}
```

<span data-ttu-id="d3ef6-231">A <xref:Microsoft.AspNetCore.Components.ParameterAttribute.CaptureUnmatchedValues> Propriedade on [`[Parameter]`](xref:Microsoft.AspNetCore.Components.ParameterAttribute) permite que o parâmetro corresponda a todos os atributos que não correspondem a nenhum outro parâmetro.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-231">The <xref:Microsoft.AspNetCore.Components.ParameterAttribute.CaptureUnmatchedValues> property on [`[Parameter]`](xref:Microsoft.AspNetCore.Components.ParameterAttribute) allows the parameter to match all attributes that don't match any other parameter.</span></span> <span data-ttu-id="d3ef6-232">Um componente só pode definir um único parâmetro com <xref:Microsoft.AspNetCore.Components.ParameterAttribute.CaptureUnmatchedValues> .</span><span class="sxs-lookup"><span data-stu-id="d3ef6-232">A component can only define a single parameter with <xref:Microsoft.AspNetCore.Components.ParameterAttribute.CaptureUnmatchedValues>.</span></span> <span data-ttu-id="d3ef6-233">O tipo de propriedade usado com <xref:Microsoft.AspNetCore.Components.ParameterAttribute.CaptureUnmatchedValues> deve ser atribuível de `Dictionary<string, object>` com chaves de cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-233">The property type used with <xref:Microsoft.AspNetCore.Components.ParameterAttribute.CaptureUnmatchedValues> must be assignable from `Dictionary<string, object>` with string keys.</span></span> <span data-ttu-id="d3ef6-234">`IEnumerable<KeyValuePair<string, object>>`ou `IReadOnlyDictionary<string, object>` também são opções neste cenário.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-234">`IEnumerable<KeyValuePair<string, object>>` or `IReadOnlyDictionary<string, object>` are also options in this scenario.</span></span>

<span data-ttu-id="d3ef6-235">A posição de [`@attributes`][3] relativo à posição dos atributos do elemento é importante.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-235">The position of [`@attributes`][3] relative to the position of element attributes is important.</span></span> <span data-ttu-id="d3ef6-236">Quando [`@attributes`][3] são splatted no elemento, os atributos são processados da direita para a esquerda (último a primeiro).</span><span class="sxs-lookup"><span data-stu-id="d3ef6-236">When [`@attributes`][3] are splatted on the element, the attributes are processed from right to left (last to first).</span></span> <span data-ttu-id="d3ef6-237">Considere o exemplo a seguir de um componente que consome um `Child` componente:</span><span class="sxs-lookup"><span data-stu-id="d3ef6-237">Consider the following example of a component that consumes a `Child` component:</span></span>

<span data-ttu-id="d3ef6-238">`ParentComponent.razor`:</span><span class="sxs-lookup"><span data-stu-id="d3ef6-238">`ParentComponent.razor`:</span></span>

```razor
<ChildComponent extra="10" />
```

<span data-ttu-id="d3ef6-239">`ChildComponent.razor`:</span><span class="sxs-lookup"><span data-stu-id="d3ef6-239">`ChildComponent.razor`:</span></span>

```razor
<div @attributes="AdditionalAttributes" extra="5" />

[Parameter(CaptureUnmatchedValues = true)]
public IDictionary<string, object> AdditionalAttributes { get; set; }
```

<span data-ttu-id="d3ef6-240">O `Child` atributo do componente `extra` é definido à direita de [`@attributes`][3] .</span><span class="sxs-lookup"><span data-stu-id="d3ef6-240">The `Child` component's `extra` attribute is set to the right of [`@attributes`][3].</span></span> <span data-ttu-id="d3ef6-241">O `Parent` renderizado do componente `<div>` contém `extra="5"` quando passado pelo atributo adicional porque os atributos são processados da direita para a esquerda (último a primeiro):</span><span class="sxs-lookup"><span data-stu-id="d3ef6-241">The `Parent` component's rendered `<div>` contains `extra="5"` when passed through the additional attribute because the attributes are processed right to left (last to first):</span></span>

```html
<div extra="5" />
```

<span data-ttu-id="d3ef6-242">No exemplo a seguir, a ordem de `extra` e [`@attributes`][3] é invertida no `Child` `<div>` :</span><span class="sxs-lookup"><span data-stu-id="d3ef6-242">In the following example, the order of `extra` and [`@attributes`][3] is reversed in the `Child` component's `<div>`:</span></span>

<span data-ttu-id="d3ef6-243">`ParentComponent.razor`:</span><span class="sxs-lookup"><span data-stu-id="d3ef6-243">`ParentComponent.razor`:</span></span>

```razor
<ChildComponent extra="10" />
```

<span data-ttu-id="d3ef6-244">`ChildComponent.razor`:</span><span class="sxs-lookup"><span data-stu-id="d3ef6-244">`ChildComponent.razor`:</span></span>

```razor
<div extra="5" @attributes="AdditionalAttributes" />

[Parameter(CaptureUnmatchedValues = true)]
public IDictionary<string, object> AdditionalAttributes { get; set; }
```

<span data-ttu-id="d3ef6-245">O renderizado `<div>` no `Parent` componente contém `extra="10"` quando passado pelo atributo adicional:</span><span class="sxs-lookup"><span data-stu-id="d3ef6-245">The rendered `<div>` in the `Parent` component contains `extra="10"` when passed through the additional attribute:</span></span>

```html
<div extra="10" />
```

## <a name="capture-references-to-components"></a><span data-ttu-id="d3ef6-246">Capturar referências a componentes</span><span class="sxs-lookup"><span data-stu-id="d3ef6-246">Capture references to components</span></span>

<span data-ttu-id="d3ef6-247">As referências de componente fornecem uma maneira de fazer referência a uma instância de componente para que você possa emitir comandos para essa instância, como `Show` ou `Reset` .</span><span class="sxs-lookup"><span data-stu-id="d3ef6-247">Component references provide a way to reference a component instance so that you can issue commands to that instance, such as `Show` or `Reset`.</span></span> <span data-ttu-id="d3ef6-248">Para capturar uma referência de componente:</span><span class="sxs-lookup"><span data-stu-id="d3ef6-248">To capture a component reference:</span></span>

* <span data-ttu-id="d3ef6-249">Adicione um [`@ref`][4] atributo ao componente filho.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-249">Add an [`@ref`][4] attribute to the child component.</span></span>
* <span data-ttu-id="d3ef6-250">Defina um campo com o mesmo tipo do componente filho.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-250">Define a field with the same type as the child component.</span></span>

```razor
<MyLoginDialog @ref="loginDialog" ... />

@code {
    private MyLoginDialog loginDialog;

    private void OnSomething()
    {
        loginDialog.Show();
    }
}
```

<span data-ttu-id="d3ef6-251">Quando o componente é renderizado, o `loginDialog` campo é populado com a `MyLoginDialog` instância de componente filho.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-251">When the component is rendered, the `loginDialog` field is populated with the `MyLoginDialog` child component instance.</span></span> <span data-ttu-id="d3ef6-252">Em seguida, você pode invocar os métodos .NET na instância do componente.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-252">You can then invoke .NET methods on the component instance.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d3ef6-253">A `loginDialog` variável é populada apenas depois que o componente é renderizado e sua saída inclui o `MyLoginDialog` elemento.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-253">The `loginDialog` variable is only populated after the component is rendered and its output includes the `MyLoginDialog` element.</span></span> <span data-ttu-id="d3ef6-254">Até esse ponto, não há nada a fazer referência.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-254">Until that point, there's nothing to reference.</span></span> <span data-ttu-id="d3ef6-255">Para manipular referências de componentes após a conclusão da renderização do componente, use os [ `OnAfterRenderAsync` `OnAfterRender` métodos ou](xref:blazor/components/lifecycle#after-component-render).</span><span class="sxs-lookup"><span data-stu-id="d3ef6-255">To manipulate components references after the component has finished rendering, use the [`OnAfterRenderAsync` or `OnAfterRender` methods](xref:blazor/components/lifecycle#after-component-render).</span></span>

<span data-ttu-id="d3ef6-256">Para fazer referência a componentes em um loop, consulte [capturar referências para vários componentes filho semelhantes (dotNet/aspnetcore #13358)](https://github.com/dotnet/aspnetcore/issues/13358).</span><span class="sxs-lookup"><span data-stu-id="d3ef6-256">To reference components in a loop, see [Capture references to multiple similar child-components (dotnet/aspnetcore #13358)](https://github.com/dotnet/aspnetcore/issues/13358).</span></span>

<span data-ttu-id="d3ef6-257">Embora a captura de referências de componente use uma sintaxe semelhante à [captura de referências de elemento](xref:blazor/call-javascript-from-dotnet#capture-references-to-elements), ela não é um recurso de interoperabilidade do JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-257">While capturing component references use a similar syntax to [capturing element references](xref:blazor/call-javascript-from-dotnet#capture-references-to-elements), it isn't a JavaScript interop feature.</span></span> <span data-ttu-id="d3ef6-258">As referências de componente não são passadas para o código JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-258">Component references aren't passed to JavaScript code.</span></span> <span data-ttu-id="d3ef6-259">As referências de componente são usadas apenas no código .NET.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-259">Component references are only used in .NET code.</span></span>

> [!NOTE]
> <span data-ttu-id="d3ef6-260">Não **use referências** de componente para converter o estado dos componentes filho.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-260">Do **not** use component references to mutate the state of child components.</span></span> <span data-ttu-id="d3ef6-261">Em vez disso, use parâmetros declarativos normais para passar dados para componentes filho.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-261">Instead, use normal declarative parameters to pass data to child components.</span></span> <span data-ttu-id="d3ef6-262">O uso de parâmetros declarativos normais resulta em componentes filho que são reprocessados nos horários corretos automaticamente.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-262">Use of normal declarative parameters result in child components that rerender at the correct times automatically.</span></span>

## <a name="synchronization-context"></a><span data-ttu-id="d3ef6-263">Contexto de sincronização</span><span class="sxs-lookup"><span data-stu-id="d3ef6-263">Synchronization context</span></span>

Blazor<span data-ttu-id="d3ef6-264">usa um contexto de sincronização ( <xref:System.Threading.SynchronizationContext> ) para impor um único thread lógico de execução.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-264"> uses a synchronization context (<xref:System.Threading.SynchronizationContext>) to enforce a single logical thread of execution.</span></span> <span data-ttu-id="d3ef6-265">Os métodos de [ciclo de vida](xref:blazor/components/lifecycle) de um componente e quaisquer retornos de chamada de evento que são gerados pelo Blazor são executados no contexto de sincronização.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-265">A component's [lifecycle methods](xref:blazor/components/lifecycle) and any event callbacks that are raised by Blazor are executed on the synchronization context.</span></span>

Blazor Server<span data-ttu-id="d3ef6-266">o contexto de sincronização do tenta emular um ambiente de thread único para que ele corresponda ao modelo Webassembly no navegador, que é um thread único.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-266">'s synchronization context attempts to emulate a single-threaded environment so that it closely matches the WebAssembly model in the browser, which is single threaded.</span></span> <span data-ttu-id="d3ef6-267">Em qualquer momento determinado, o trabalho é executado em exatamente um thread, dando a impressão de um único thread lógico.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-267">At any given point in time, work is performed on exactly one thread, giving the impression of a single logical thread.</span></span> <span data-ttu-id="d3ef6-268">Duas operações não são executadas simultaneamente.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-268">No two operations execute concurrently.</span></span>

### <a name="avoid-thread-blocking-calls"></a><span data-ttu-id="d3ef6-269">Evitar chamadas de bloqueio de thread</span><span class="sxs-lookup"><span data-stu-id="d3ef6-269">Avoid thread-blocking calls</span></span>

<span data-ttu-id="d3ef6-270">Em geral, não chame os métodos a seguir.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-270">Generally, don't call the following methods.</span></span> <span data-ttu-id="d3ef6-271">Os métodos a seguir bloqueiam o thread e, portanto, impedem que o aplicativo retome o trabalho até que o subjacente <xref:System.Threading.Tasks.Task> seja concluído:</span><span class="sxs-lookup"><span data-stu-id="d3ef6-271">The following methods block the thread and thus block the app from resuming work until the underlying <xref:System.Threading.Tasks.Task> is complete:</span></span>

* <xref:System.Threading.Tasks.Task%601.Result%2A>
* <xref:System.Threading.Tasks.Task.Wait%2A>
* <xref:System.Threading.Tasks.Task.WaitAny%2A>
* <xref:System.Threading.Tasks.Task.WaitAll%2A>
* <xref:System.Threading.Thread.Sleep%2A>
* <xref:System.Runtime.CompilerServices.TaskAwaiter.GetResult%2A>

### <a name="invoke-component-methods-externally-to-update-state"></a><span data-ttu-id="d3ef6-272">Invocar métodos de componente externamente para atualizar o estado</span><span class="sxs-lookup"><span data-stu-id="d3ef6-272">Invoke component methods externally to update state</span></span>

<span data-ttu-id="d3ef6-273">No caso de um componente precisar ser atualizado com base em um evento externo, como um temporizador ou outras notificações, use o `InvokeAsync` método, que é expedido para o Blazor contexto de sincronização.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-273">In the event a component must be updated based on an external event, such as a timer or other notifications, use the `InvokeAsync` method, which dispatches to Blazor's synchronization context.</span></span> <span data-ttu-id="d3ef6-274">Por exemplo, considere um *serviço de notificação* que pode notificar qualquer componente de escuta do estado atualizado:</span><span class="sxs-lookup"><span data-stu-id="d3ef6-274">For example, consider a *notifier service* that can notify any listening component of the updated state:</span></span>

```csharp
public class NotifierService
{
    // Can be called from anywhere
    public async Task Update(string key, int value)
    {
        if (Notify != null)
        {
            await Notify.Invoke(key, value);
        }
    }

    public event Func<string, int, Task> Notify;
}
```

<span data-ttu-id="d3ef6-275">Registre o `NotifierService` como um singletion:</span><span class="sxs-lookup"><span data-stu-id="d3ef6-275">Register the `NotifierService` as a singletion:</span></span>

* <span data-ttu-id="d3ef6-276">No Blazor WebAssembly , registre o serviço no `Program.Main` :</span><span class="sxs-lookup"><span data-stu-id="d3ef6-276">In Blazor WebAssembly, register the service in `Program.Main`:</span></span>

  ```csharp
  builder.Services.AddSingleton<NotifierService>();
  ```

* <span data-ttu-id="d3ef6-277">No Blazor Server , registre o serviço no `Startup.ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="d3ef6-277">In Blazor Server, register the service in `Startup.ConfigureServices`:</span></span>

  ```csharp
  services.AddScoped<NotifierService>();
  ```

<span data-ttu-id="d3ef6-278">Use o `NotifierService` para atualizar um componente:</span><span class="sxs-lookup"><span data-stu-id="d3ef6-278">Use the `NotifierService` to update a component:</span></span>

```razor
@page "/"
@inject NotifierService Notifier
@implements IDisposable

<p>Last update: @lastNotification.key = @lastNotification.value</p>

@code {
    private (string key, int value) lastNotification;

    protected override void OnInitialized()
    {
        Notifier.Notify += OnNotify;
    }

    public async Task OnNotify(string key, int value)
    {
        await InvokeAsync(() =>
        {
            lastNotification = (key, value);
            StateHasChanged();
        });
    }

    public void Dispose()
    {
        Notifier.Notify -= OnNotify;
    }
}
```

<span data-ttu-id="d3ef6-279">No exemplo anterior, `NotifierService` invoca o método do componente `OnNotify` fora do contexto de Blazor sincronização do.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-279">In the preceding example, `NotifierService` invokes the component's `OnNotify` method outside of Blazor's synchronization context.</span></span> <span data-ttu-id="d3ef6-280">`InvokeAsync`é usado para alternar para o contexto correto e enfileirar uma renderização.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-280">`InvokeAsync` is used to switch to the correct context and queue a render.</span></span>

## <a name="use-key-to-control-the-preservation-of-elements-and-components"></a><span data-ttu-id="d3ef6-281">Use \@ a chave para controlar a preservação de elementos e componentes</span><span class="sxs-lookup"><span data-stu-id="d3ef6-281">Use \@key to control the preservation of elements and components</span></span>

<span data-ttu-id="d3ef6-282">Ao renderizar uma lista de elementos ou componentes e os elementos ou componentes subsequentemente mudam, o Blazor algoritmo de diferenciação do deve decidir quais elementos ou componentes anteriores podem ser retidos e como os objetos de modelo devem ser mapeados para eles.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-282">When rendering a list of elements or components and the elements or components subsequently change, Blazor's diffing algorithm must decide which of the previous elements or components can be retained and how model objects should map to them.</span></span> <span data-ttu-id="d3ef6-283">Normalmente, esse processo é automático e pode ser ignorado, mas há casos em que você talvez queira controlar o processo.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-283">Normally, this process is automatic and can be ignored, but there are cases where you may want to control the process.</span></span>

<span data-ttu-id="d3ef6-284">Considere o exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="d3ef6-284">Consider the following example:</span></span>

```csharp
@foreach (var person in People)
{
    <DetailsEditor Details="@person.Details" />
}

@code {
    [Parameter]
    public IEnumerable<Person> People { get; set; }
}
```

<span data-ttu-id="d3ef6-285">O conteúdo da `People` coleção pode ser alterado com entradas inseridas, excluídas ou reordenadas.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-285">The contents of the `People` collection may change with inserted, deleted, or re-ordered entries.</span></span> <span data-ttu-id="d3ef6-286">Quando o componente é rerenderizado, o `<DetailsEditor>` componente pode ser alterado para receber `Details` valores de parâmetro diferentes.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-286">When the component rerenders, the `<DetailsEditor>` component may change to receive different `Details` parameter values.</span></span> <span data-ttu-id="d3ef6-287">Isso pode causar um reprocessamento mais complexo do que o esperado.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-287">This may cause more complex rerendering than expected.</span></span> <span data-ttu-id="d3ef6-288">Em alguns casos, a rerenderização pode levar a diferenças de comportamento visíveis, como o foco de elemento perdido.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-288">In some cases, rerendering can lead to visible behavior differences, such as lost element focus.</span></span>

<span data-ttu-id="d3ef6-289">O processo de mapeamento pode ser controlado com o [`@key`][5] atributo de diretiva.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-289">The mapping process can be controlled with the [`@key`][5] directive attribute.</span></span> <span data-ttu-id="d3ef6-290">[`@key`][5]faz com que o algoritmo diff garanta a preservação de elementos ou componentes com base no valor da chave:</span><span class="sxs-lookup"><span data-stu-id="d3ef6-290">[`@key`][5] causes the diffing algorithm to guarantee preservation of elements or components based on the key's value:</span></span>

```csharp
@foreach (var person in People)
{
    <DetailsEditor @key="person" Details="@person.Details" />
}

@code {
    [Parameter]
    public IEnumerable<Person> People { get; set; }
}
```

<span data-ttu-id="d3ef6-291">Quando a `People` coleção é alterada, o algoritmo diff mantém a associação entre `<DetailsEditor>` instâncias e `person` instâncias:</span><span class="sxs-lookup"><span data-stu-id="d3ef6-291">When the `People` collection changes, the diffing algorithm retains the association between `<DetailsEditor>` instances and `person` instances:</span></span>

* <span data-ttu-id="d3ef6-292">Se um `Person` for excluído da `People` lista, somente a instância correspondente `<DetailsEditor>` será REmovida da interface do usuário.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-292">If a `Person` is deleted from the `People` list, only the corresponding `<DetailsEditor>` instance is removed from the UI.</span></span> <span data-ttu-id="d3ef6-293">Outras instâncias permanecem inalteradas.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-293">Other instances are left unchanged.</span></span>
* <span data-ttu-id="d3ef6-294">Se um `Person` for inserido em alguma posição na lista, uma nova `<DetailsEditor>` instância será inserida na posição correspondente.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-294">If a `Person` is inserted at some position in the list, one new `<DetailsEditor>` instance is inserted at that corresponding position.</span></span> <span data-ttu-id="d3ef6-295">Outras instâncias permanecem inalteradas.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-295">Other instances are left unchanged.</span></span>
* <span data-ttu-id="d3ef6-296">Se `Person` as entradas forem reordenadas, as `<DetailsEditor>` instâncias correspondentes serão preservadas e reordenadas na interface do usuário.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-296">If `Person` entries are re-ordered, the corresponding `<DetailsEditor>` instances are preserved and re-ordered in the UI.</span></span>

<span data-ttu-id="d3ef6-297">Em alguns cenários, o uso de [`@key`][5] minimiza a complexidade da rerenderização e evita possíveis problemas com partes com estado da alteração do dom, como a posição do foco.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-297">In some scenarios, use of [`@key`][5] minimizes the complexity of rerendering and avoids potential issues with stateful parts of the DOM changing, such as focus position.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d3ef6-298">As chaves são locais para cada elemento ou componente de contêiner.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-298">Keys are local to each container element or component.</span></span> <span data-ttu-id="d3ef6-299">As chaves não são comparadas globalmente ao longo do documento.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-299">Keys aren't compared globally across the document.</span></span>

### <a name="when-to-use-key"></a><span data-ttu-id="d3ef6-300">Quando usar a \@ chave</span><span class="sxs-lookup"><span data-stu-id="d3ef6-300">When to use \@key</span></span>

<span data-ttu-id="d3ef6-301">Normalmente, faz sentido usar [`@key`][5] sempre que uma lista é renderizada (por exemplo, em um bloco [foreach](/dotnet/csharp/language-reference/keywords/foreach-in) ) e um valor adequado existe para definir o [`@key`][5] .</span><span class="sxs-lookup"><span data-stu-id="d3ef6-301">Typically, it makes sense to use [`@key`][5] whenever a list is rendered (for example, in a [foreach](/dotnet/csharp/language-reference/keywords/foreach-in) block) and a suitable value exists to define the [`@key`][5].</span></span>

<span data-ttu-id="d3ef6-302">Você também pode usar [`@key`][5] para evitar a Blazor preservação de uma subárvore de elemento ou componente quando um objeto for alterado:</span><span class="sxs-lookup"><span data-stu-id="d3ef6-302">You can also use [`@key`][5] to prevent Blazor from preserving an element or component subtree when an object changes:</span></span>

```razor
<div @key="currentPerson">
    ... content that depends on currentPerson ...
</div>
```

<span data-ttu-id="d3ef6-303">Se `@currentPerson` forem alteradas, a [`@key`][5] diretiva de atributo forçará Blazor a descartar todo `<div>` e seus descendentes e recriará a subárvore dentro da interface do usuário com novos elementos e componentes.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-303">If `@currentPerson` changes, the [`@key`][5] attribute directive forces Blazor to discard the entire `<div>` and its descendants and rebuild the subtree within the UI with new elements and components.</span></span> <span data-ttu-id="d3ef6-304">Isso pode ser útil se você precisar garantir que nenhum estado da interface do usuário seja preservado quando houver `@currentPerson` alterações.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-304">This can be useful if you need to guarantee that no UI state is preserved when `@currentPerson` changes.</span></span>

### <a name="when-not-to-use-key"></a><span data-ttu-id="d3ef6-305">Quando não usar a \@ chave</span><span class="sxs-lookup"><span data-stu-id="d3ef6-305">When not to use \@key</span></span>

<span data-ttu-id="d3ef6-306">Há um custo de desempenho ao comparar com [`@key`][5] .</span><span class="sxs-lookup"><span data-stu-id="d3ef6-306">There's a performance cost when diffing with [`@key`][5].</span></span> <span data-ttu-id="d3ef6-307">O custo de desempenho não é grande, mas só especifica [`@key`][5] se controlar as regras de preservação de elementos ou componentes beneficiam o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-307">The performance cost isn't large, but only specify [`@key`][5] if controlling the element or component preservation rules benefit the app.</span></span>

<span data-ttu-id="d3ef6-308">Mesmo que [`@key`][5] não seja usado, Blazor o preserva as instâncias de elemento filho e de componente o máximo possível.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-308">Even if [`@key`][5] isn't used, Blazor preserves child element and component instances as much as possible.</span></span> <span data-ttu-id="d3ef6-309">A única vantagem de usar o [`@key`][5] é o controle sobre *como* as instâncias de modelo são mapeadas para as instâncias de componente preservadas, em vez do algoritmo diff, selecionando o mapeamento.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-309">The only advantage to using [`@key`][5] is control over *how* model instances are mapped to the preserved component instances, instead of the diffing algorithm selecting the mapping.</span></span>

### <a name="what-values-to-use-for-key"></a><span data-ttu-id="d3ef6-310">Quais valores usar para a \@ chave</span><span class="sxs-lookup"><span data-stu-id="d3ef6-310">What values to use for \@key</span></span>

<span data-ttu-id="d3ef6-311">Geralmente, faz sentido fornecer um dos seguintes tipos de valor para [`@key`][5] :</span><span class="sxs-lookup"><span data-stu-id="d3ef6-311">Generally, it makes sense to supply one of the following kinds of value for [`@key`][5]:</span></span>

* <span data-ttu-id="d3ef6-312">Instâncias de objeto de modelo (por exemplo, uma `Person` instância como no exemplo anterior).</span><span class="sxs-lookup"><span data-stu-id="d3ef6-312">Model object instances (for example, a `Person` instance as in the earlier example).</span></span> <span data-ttu-id="d3ef6-313">Isso garante a preservação com base na igualdade de referência de objeto.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-313">This ensures preservation based on object reference equality.</span></span>
* <span data-ttu-id="d3ef6-314">Identificadores exclusivos (por exemplo, valores de chave primária do tipo `int` , `string` ou `Guid` ).</span><span class="sxs-lookup"><span data-stu-id="d3ef6-314">Unique identifiers (for example, primary key values of type `int`, `string`, or `Guid`).</span></span>

<span data-ttu-id="d3ef6-315">Verifique se os valores usados para [`@key`][5] não conflitam.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-315">Ensure that values used for [`@key`][5] don't clash.</span></span> <span data-ttu-id="d3ef6-316">Se valores conflitantes forem detectados no mesmo elemento pai, Blazor o lançará uma exceção porque ele não pode mapear de forma determinística elementos ou componentes antigos para novos elementos ou componentes.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-316">If clashing values are detected within the same parent element, Blazor throws an exception because it can't deterministically map old elements or components to new elements or components.</span></span> <span data-ttu-id="d3ef6-317">Use apenas valores distintos, como instâncias de objeto ou valores de chave primária.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-317">Only use distinct values, such as object instances or primary key values.</span></span>

## <a name="dont-create-components-that-write-to-their-own-parameter-properties"></a><span data-ttu-id="d3ef6-318">Não crie componentes que gravam em suas próprias propriedades de parâmetro</span><span class="sxs-lookup"><span data-stu-id="d3ef6-318">Don't create components that write to their own parameter properties</span></span>

<span data-ttu-id="d3ef6-319">Os parâmetros são substituídos nas seguintes condições:</span><span class="sxs-lookup"><span data-stu-id="d3ef6-319">Parameters are overwritten under the following conditions:</span></span>

* <span data-ttu-id="d3ef6-320">O conteúdo de um componente filho é renderizado com um <xref:Microsoft.AspNetCore.Components.RenderFragment> .</span><span class="sxs-lookup"><span data-stu-id="d3ef6-320">A child component's content is rendered with a <xref:Microsoft.AspNetCore.Components.RenderFragment>.</span></span>
* <span data-ttu-id="d3ef6-321"><xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged%2A>é chamado no componente pai.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-321"><xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged%2A> is called in the parent component.</span></span>

<span data-ttu-id="d3ef6-322">Os parâmetros são redefinidos porque o componente pai é rerenderizado quando <xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged%2A> é chamado e novos valores de parâmetro são fornecidos ao componente filho.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-322">Parameters are reset because the parent component rerenders when <xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged%2A> is called and new parameter values are supplied to the child component.</span></span>

<span data-ttu-id="d3ef6-323">Considere o seguinte `Expander` componente que:</span><span class="sxs-lookup"><span data-stu-id="d3ef6-323">Consider the following `Expander` component that:</span></span>

* <span data-ttu-id="d3ef6-324">Renderiza conteúdo filho.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-324">Renders child content.</span></span>
* <span data-ttu-id="d3ef6-325">Alterna a exibição de conteúdo filho com um parâmetro de componente.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-325">Toggles showing child content with a component parameter.</span></span>

```razor
<div @onclick="@Toggle">
    Toggle (Expanded = @Expanded)

    @if (Expanded)
    {
        @ChildContent
    }
</div>

@code {
    [Parameter]
    public bool Expanded { get; set; }

    [Parameter]
    public RenderFragment ChildContent { get; set; }

    private void Toggle()
    {
        Expanded = !Expanded;
    }
}
```

<span data-ttu-id="d3ef6-326">O `Expander` componente é adicionado a um componente pai que pode chamar <xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged%2A> :</span><span class="sxs-lookup"><span data-stu-id="d3ef6-326">The `Expander` component is added to a parent component that may call <xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged%2A>:</span></span>

```razor
<Expander Expanded="true">
    <h1>Hello, world!</h1>
</Expander>

<Expander Expanded="true" />

<button @onclick="@(() => StateHasChanged())">
    Call StateHasChanged
</button>
```

<span data-ttu-id="d3ef6-327">Inicialmente, os `Expander` componentes se comportam de forma independente quando suas `Expanded` Propriedades são alternadas.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-327">Initially, the `Expander` components behave independently when their `Expanded` properties are toggled.</span></span> <span data-ttu-id="d3ef6-328">Os componentes filho mantêm seus Estados como esperado.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-328">The child components maintain their states as expected.</span></span> <span data-ttu-id="d3ef6-329">Quando <xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged%2A> é chamado no pai, o `Expanded` parâmetro do primeiro componente filho é redefinido de volta para seu valor inicial ( `true` ).</span><span class="sxs-lookup"><span data-stu-id="d3ef6-329">When <xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged%2A> is called in the parent, the `Expanded` parameter of the first child component is reset back to its initial value (`true`).</span></span> <span data-ttu-id="d3ef6-330">O `Expander` valor do segundo componente `Expanded` não é redefinido porque nenhum conteúdo filho é renderizado no segundo componente.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-330">The second `Expander` component's `Expanded` value isn't reset because no child content is rendered in the second component.</span></span>

<span data-ttu-id="d3ef6-331">Para manter o estado no cenário anterior, use um *campo particular* no `Expander` componente para manter seu estado de alternância.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-331">To maintain state in the preceding scenario, use a *private field* in the `Expander` component to maintain its toggled state.</span></span>

<span data-ttu-id="d3ef6-332">O seguinte `Expander` componente:</span><span class="sxs-lookup"><span data-stu-id="d3ef6-332">The following `Expander` component:</span></span>

* <span data-ttu-id="d3ef6-333">Aceita o `Expanded` valor do parâmetro de componente do pai.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-333">Accepts the `Expanded` component parameter value from the parent.</span></span>
* <span data-ttu-id="d3ef6-334">Atribui o valor do parâmetro de componente a um *campo privado* ( `expanded` ) no [evento OnInitialized](xref:blazor/components/lifecycle#component-initialization-methods).</span><span class="sxs-lookup"><span data-stu-id="d3ef6-334">Assigns the component parameter value to a *private field* (`expanded`) in the [OnInitialized event](xref:blazor/components/lifecycle#component-initialization-methods).</span></span>
* <span data-ttu-id="d3ef6-335">Usa o campo particular para manter seu estado de alternância interno.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-335">Uses the private field to maintain its internal toggle state.</span></span>

```razor
<div @onclick="@Toggle">
    Toggle (Expanded = @expanded)

    @if (expanded)
    {
        @ChildContent
    }
</div>

@code {
    [Parameter]
    public bool Expanded { get; set; }

    [Parameter]
    public RenderFragment ChildContent { get; set; }

    private bool expanded;

    protected override void OnInitialized()
    {
        expanded = Expanded;
    }

    private void Toggle()
    {
        expanded = !expanded;
    }
}
```

## <a name="apply-an-attribute"></a><span data-ttu-id="d3ef6-336">Aplicar um atributo</span><span class="sxs-lookup"><span data-stu-id="d3ef6-336">Apply an attribute</span></span>

<span data-ttu-id="d3ef6-337">Os atributos podem ser aplicados a Razor componentes com a [`@attribute`][7] diretiva.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-337">Attributes can be applied to Razor components with the [`@attribute`][7] directive.</span></span> <span data-ttu-id="d3ef6-338">O exemplo a seguir aplica o [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) atributo à classe de componente:</span><span class="sxs-lookup"><span data-stu-id="d3ef6-338">The following example applies the [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) attribute to the component class:</span></span>

```razor
@page "/"
@attribute [Authorize]
```

## <a name="conditional-html-element-attributes"></a><span data-ttu-id="d3ef6-339">Atributos de elemento HTML condicional</span><span class="sxs-lookup"><span data-stu-id="d3ef6-339">Conditional HTML element attributes</span></span>

<span data-ttu-id="d3ef6-340">Os atributos de elemento HTML são processados condicionalmente com base no valor do .NET.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-340">HTML element attributes are conditionally rendered based on the .NET value.</span></span> <span data-ttu-id="d3ef6-341">Se o valor for `false` ou `null` , o atributo não será renderizado.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-341">If the value is `false` or `null`, the attribute isn't rendered.</span></span> <span data-ttu-id="d3ef6-342">Se o valor é `true` , o atributo é processado minimizado.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-342">If the value is `true`, the attribute is rendered minimized.</span></span>

<span data-ttu-id="d3ef6-343">No exemplo a seguir, `IsCompleted` determina se `checked` é renderizado na marcação do elemento:</span><span class="sxs-lookup"><span data-stu-id="d3ef6-343">In the following example, `IsCompleted` determines if `checked` is rendered in the element's markup:</span></span>

```razor
<input type="checkbox" checked="@IsCompleted" />

@code {
    [Parameter]
    public bool IsCompleted { get; set; }
}
```

<span data-ttu-id="d3ef6-344">Se `IsCompleted` for `true` , a caixa de seleção será renderizada como:</span><span class="sxs-lookup"><span data-stu-id="d3ef6-344">If `IsCompleted` is `true`, the check box is rendered as:</span></span>

```html
<input type="checkbox" checked />
```

<span data-ttu-id="d3ef6-345">Se `IsCompleted` for `false` , a caixa de seleção será renderizada como:</span><span class="sxs-lookup"><span data-stu-id="d3ef6-345">If `IsCompleted` is `false`, the check box is rendered as:</span></span>

```html
<input type="checkbox" />
```

<span data-ttu-id="d3ef6-346">Para obter mais informações, consulte <xref:mvc/views/razor>.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-346">For more information, see <xref:mvc/views/razor>.</span></span>

> [!WARNING]
> <span data-ttu-id="d3ef6-347">Alguns atributos HTML, como [`aria-pressed`](https://developer.mozilla.org/docs/Web/Accessibility/ARIA/Roles/button_role#Toggle_buttons) , não funcionam corretamente quando o tipo .net é um `bool` .</span><span class="sxs-lookup"><span data-stu-id="d3ef6-347">Some HTML attributes, such as [`aria-pressed`](https://developer.mozilla.org/docs/Web/Accessibility/ARIA/Roles/button_role#Toggle_buttons), don't function properly when the .NET type is a `bool`.</span></span> <span data-ttu-id="d3ef6-348">Nesses casos, use um `string` tipo em vez de um `bool` .</span><span class="sxs-lookup"><span data-stu-id="d3ef6-348">In those cases, use a `string` type instead of a `bool`.</span></span>

## <a name="raw-html"></a><span data-ttu-id="d3ef6-349">HTML bruto</span><span class="sxs-lookup"><span data-stu-id="d3ef6-349">Raw HTML</span></span>

<span data-ttu-id="d3ef6-350">Normalmente, as cadeias de caracteres são renderizadas usando nós de texto DOM, o que significa que qualquer marcação que ela possa conter será ignorada e tratada como texto literal.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-350">Strings are normally rendered using DOM text nodes, which means that any markup they may contain is ignored and treated as literal text.</span></span> <span data-ttu-id="d3ef6-351">Para renderizar HTML bruto, empacote o conteúdo HTML em um `MarkupString` valor.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-351">To render raw HTML, wrap the HTML content in a `MarkupString` value.</span></span> <span data-ttu-id="d3ef6-352">O valor é analisado como HTML ou SVG e inserido no DOM.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-352">The value is parsed as HTML or SVG and inserted into the DOM.</span></span>

> [!WARNING]
> <span data-ttu-id="d3ef6-353">O processamento de HTML bruto construído a partir de qualquer fonte não confiável é um **risco à segurança** e deve ser evitado!</span><span class="sxs-lookup"><span data-stu-id="d3ef6-353">Rendering raw HTML constructed from any untrusted source is a **security risk** and should be avoided!</span></span>

<span data-ttu-id="d3ef6-354">O exemplo a seguir mostra como usar o `MarkupString` tipo para adicionar um bloco de conteúdo HTML estático à saída renderizada de um componente:</span><span class="sxs-lookup"><span data-stu-id="d3ef6-354">The following example shows using the `MarkupString` type to add a block of static HTML content to the rendered output of a component:</span></span>

```html
@((MarkupString)myMarkup)

@code {
    private string myMarkup = 
        "<p class='markup'>This is a <em>markup string</em>.</p>";
}
```

## <a name="razor-templates"></a>Razor<span data-ttu-id="d3ef6-355">modelo</span><span class="sxs-lookup"><span data-stu-id="d3ef6-355"> templates</span></span>

<span data-ttu-id="d3ef6-356">Os fragmentos de renderização podem ser definidos usando a Razor sintaxe do modelo.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-356">Render fragments can be defined using Razor template syntax.</span></span> Razor<span data-ttu-id="d3ef6-357">os modelos são uma maneira de definir um trecho de interface do usuário e assumir o seguinte formato:</span><span class="sxs-lookup"><span data-stu-id="d3ef6-357"> templates are a way to define a UI snippet and assume the following format:</span></span>

```razor
@<{HTML tag}>...</{HTML tag}>
```

<span data-ttu-id="d3ef6-358">O exemplo a seguir ilustra como especificar <xref:Microsoft.AspNetCore.Components.RenderFragment> e <xref:Microsoft.AspNetCore.Components.RenderFragment%601> valores e renderizar modelos diretamente em um componente.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-358">The following example illustrates how to specify <xref:Microsoft.AspNetCore.Components.RenderFragment> and <xref:Microsoft.AspNetCore.Components.RenderFragment%601> values and render templates directly in a component.</span></span> <span data-ttu-id="d3ef6-359">Os fragmentos de renderização também podem ser passados como argumentos para [componentes de modelo](xref:blazor/components/templated-components).</span><span class="sxs-lookup"><span data-stu-id="d3ef6-359">Render fragments can also be passed as arguments to [templated components](xref:blazor/components/templated-components).</span></span>

```razor
@timeTemplate

@petTemplate(new Pet { Name = "Rex" })

@code {
    private RenderFragment timeTemplate = @<p>The time is @DateTime.Now.</p>;
    private RenderFragment<Pet> petTemplate = (pet) => @<p>Pet: @pet.Name</p>;

    private class Pet
    {
        public string Name { get; set; }
    }
}
```

<span data-ttu-id="d3ef6-360">Saída renderizada do código anterior:</span><span class="sxs-lookup"><span data-stu-id="d3ef6-360">Rendered output of the preceding code:</span></span>

```html
<p>The time is 10/04/2018 01:26:52.</p>

<p>Pet: Rex</p>
```

## <a name="static-assets"></a><span data-ttu-id="d3ef6-361">Ativos estáticos</span><span class="sxs-lookup"><span data-stu-id="d3ef6-361">Static assets</span></span>

Blazor<span data-ttu-id="d3ef6-362">segue a Convenção de ASP.NET Core aplicativos que posicionam ativos estáticos na [ `web root (wwwroot)` pasta](xref:fundamentals/index#web-root)do projeto.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-362"> follows the convention of ASP.NET Core apps placing static assets under the project's [`web root (wwwroot)` folder](xref:fundamentals/index#web-root).</span></span>

<span data-ttu-id="d3ef6-363">Use um caminho relativo de base ( `/` ) para se referir à raiz da Web para um ativo estático.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-363">Use a base-relative path (`/`) to refer to the web root for a static asset.</span></span> <span data-ttu-id="d3ef6-364">No exemplo a seguir, `logo.png` está localizado fisicamente na `{PROJECT ROOT}/wwwroot/images` pasta:</span><span class="sxs-lookup"><span data-stu-id="d3ef6-364">In the following example, `logo.png` is physically located in the `{PROJECT ROOT}/wwwroot/images` folder:</span></span>

```razor
<img alt="Company logo" src="/images/logo.png" />
```

Razor<span data-ttu-id="d3ef6-365">os componentes **não** dão suporte a notação de barra de til ( `~/` ).</span><span class="sxs-lookup"><span data-stu-id="d3ef6-365"> components do **not** support tilde-slash notation (`~/`).</span></span>

<span data-ttu-id="d3ef6-366">Para obter informações sobre como definir o caminho base de um aplicativo, consulte <xref:blazor/host-and-deploy/index#app-base-path> .</span><span class="sxs-lookup"><span data-stu-id="d3ef6-366">For information on setting an app's base path, see <xref:blazor/host-and-deploy/index#app-base-path>.</span></span>

## <a name="tag-helpers-arent-supported-in-components"></a><span data-ttu-id="d3ef6-367">Não há suporte para auxiliares de marcas nos componentes</span><span class="sxs-lookup"><span data-stu-id="d3ef6-367">Tag Helpers aren't supported in components</span></span>

<span data-ttu-id="d3ef6-368">[`Tag Helpers`](xref:mvc/views/tag-helpers/intro)Não tem suporte em Razor componentes ( `.razor` arquivos).</span><span class="sxs-lookup"><span data-stu-id="d3ef6-368">[`Tag Helpers`](xref:mvc/views/tag-helpers/intro) aren't supported in Razor components (`.razor` files).</span></span> <span data-ttu-id="d3ef6-369">Para fornecer a funcionalidade do tipo auxiliar de marca no Blazor , crie um componente com a mesma funcionalidade que o auxiliar de marca e use o componente em vez disso.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-369">To provide Tag Helper-like functionality in Blazor, create a component with the same functionality as the Tag Helper and use the component instead.</span></span>

## <a name="scalable-vector-graphics-svg-images"></a><span data-ttu-id="d3ef6-370">Imagens SVG (gráficos vetoriais escaláveis)</span><span class="sxs-lookup"><span data-stu-id="d3ef6-370">Scalable Vector Graphics (SVG) images</span></span>

<span data-ttu-id="d3ef6-371">Como Blazor o renderiza as imagens html, com suporte para navegadores, incluindo imagens de vetores escalonáveis (SVG) `.svg` , há suporte por meio da `<img>` marca:</span><span class="sxs-lookup"><span data-stu-id="d3ef6-371">Since Blazor renders HTML, browser-supported images, including Scalable Vector Graphics (SVG) images (`.svg`), are supported via the `<img>` tag:</span></span>

```html
<img alt="Example image" src="some-image.svg" />
```

<span data-ttu-id="d3ef6-372">Da mesma forma, as imagens SVG têm suporte nas regras de CSS de um arquivo de folha de estilos ( `.css` ):</span><span class="sxs-lookup"><span data-stu-id="d3ef6-372">Similarly, SVG images are supported in the CSS rules of a stylesheet file (`.css`):</span></span>

```css
.my-element {
    background-image: url("some-image.svg");
}
```

<span data-ttu-id="d3ef6-373">No entanto, a marcação SVG embutida não tem suporte em todos os cenários.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-373">However, inline SVG markup isn't supported in all scenarios.</span></span> <span data-ttu-id="d3ef6-374">Se você posicionar uma `<svg>` marca diretamente em um arquivo de componente ( `.razor` ), a renderização básica da imagem terá suporte, mas muitos cenários avançados ainda não têm suporte.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-374">If you place an `<svg>` tag directly into a component file (`.razor`), basic image rendering is supported but many advanced scenarios aren't yet supported.</span></span> <span data-ttu-id="d3ef6-375">Por exemplo, as `<use>` marcas não são respeitadas atualmente e [`@bind`][10] não podem ser usadas com algumas marcas SVG.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-375">For example, `<use>` tags aren't currently respected, and [`@bind`][10] can't be used with some SVG tags.</span></span> <span data-ttu-id="d3ef6-376">Para obter mais informações, consulte [suporte a SVG em Blazor (dotNet/aspnetcore #18271)](https://github.com/dotnet/aspnetcore/issues/18271).</span><span class="sxs-lookup"><span data-stu-id="d3ef6-376">For more information, see [SVG support in Blazor (dotnet/aspnetcore #18271)](https://github.com/dotnet/aspnetcore/issues/18271).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d3ef6-377">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="d3ef6-377">Additional resources</span></span>

* <span data-ttu-id="d3ef6-378"><xref:blazor/security/server/threat-mitigation>: Inclui diretrizes sobre a criação de Blazor Server aplicativos que devem combater o esgotamento de recursos.</span><span class="sxs-lookup"><span data-stu-id="d3ef6-378"><xref:blazor/security/server/threat-mitigation>: Includes guidance on building Blazor Server apps that must contend with resource exhaustion.</span></span>

<!--Reference links in article-->
[1]: <xref:mvc/views/razor#code>
[2]: <xref:mvc/views/razor#using>
[3]: <xref:mvc/views/razor#attributes>
[4]: <xref:mvc/views/razor#ref>
[5]: <xref:mvc/views/razor#key>
[6]: <xref:mvc/views/razor#inherits>
[7]: <xref:mvc/views/razor#attribute>
[8]: <xref:mvc/views/razor#namespace>
[9]: <xref:mvc/views/razor#page>
[10]: <xref:mvc/views/razor#bind>