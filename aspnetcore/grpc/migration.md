---
title: Migrar Serviços gRPC C-core para o ASP.NET Core
author: juntaoluo
description: Saiba como mover um aplicativo de gRPC baseados em C-core existente para ser executado na parte superior da pilha do ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 03/31/2019
uid: grpc/migration
ms.openlocfilehash: 47d74edd821124f0c8390d704ca7931b7eb6c4cd
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64895233"
---
# <a name="migrating-grpc-services-from-c-core-to-aspnet-core"></a><span data-ttu-id="7ba3a-103">Migrar Serviços gRPC C-core para o ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7ba3a-103">Migrating gRPC services from C-core to ASP.NET Core</span></span>

<span data-ttu-id="7ba3a-104">Por [John Luo](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="7ba3a-104">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="7ba3a-105">Devido à implementação da pilha subjacente, nem todos os recursos funcionam da mesma maneira entre [gRPC baseado em núcleo C](https://grpc.io/blog/grpc-stacks) aplicativos e aplicativos baseados no ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7ba3a-105">Due to the implementation of the underlying stack, not all features work in the same way between [C-core-based gRPC](https://grpc.io/blog/grpc-stacks) apps and ASP.NET Core-based apps.</span></span> <span data-ttu-id="7ba3a-106">Este documento destaca as diferenças principais para migrar entre as duas pilhas.</span><span class="sxs-lookup"><span data-stu-id="7ba3a-106">This document highlights the key differences for migrating between the two stacks.</span></span>

## <a name="grpc-service-implementation-lifetime"></a><span data-ttu-id="7ba3a-107">tempo de vida de implementação de serviço gRPC</span><span class="sxs-lookup"><span data-stu-id="7ba3a-107">gRPC service implementation lifetime</span></span>

<span data-ttu-id="7ba3a-108">Na pilha do ASP.NET Core, serviços de gRPC, por padrão, são criados com um [tempo de vida com escopo](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="7ba3a-108">In the ASP.NET Core stack, gRPC services, by default, are created with a [scoped lifetime](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="7ba3a-109">Em contraste, gRPC C-core por padrão é associado a um serviço com um [tempo de vida singleton](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="7ba3a-109">In contrast, gRPC C-core by default binds to a service with a [singleton lifetime](xref:fundamentals/dependency-injection#service-lifetimes).</span></span>

<span data-ttu-id="7ba3a-110">Um tempo de vida com escopo permite que a implementação do serviço resolver os outros serviços com tempos de vida com escopo definido.</span><span class="sxs-lookup"><span data-stu-id="7ba3a-110">A scoped lifetime allows the service implementation to resolve other services with scoped lifetimes.</span></span> <span data-ttu-id="7ba3a-111">Por exemplo, um tempo de vida com escopo também pode resolver `DBContext` do contêiner de DI por meio da injeção de construtor.</span><span class="sxs-lookup"><span data-stu-id="7ba3a-111">For example, a scoped lifetime can also resolve `DBContext` from the DI container through constructor injection.</span></span> <span data-ttu-id="7ba3a-112">Usando o tempo de vida com escopo definido:</span><span class="sxs-lookup"><span data-stu-id="7ba3a-112">Using scoped lifetime:</span></span>

* <span data-ttu-id="7ba3a-113">Uma nova instância da implementação do serviço é construída para cada solicitação.</span><span class="sxs-lookup"><span data-stu-id="7ba3a-113">A new instance of the service implementation is constructed for each request.</span></span>
* <span data-ttu-id="7ba3a-114">Não é possível compartilhar o estado entre as solicitações por meio de membros de instância no tipo de implementação.</span><span class="sxs-lookup"><span data-stu-id="7ba3a-114">It isn't possible to share state between requests via instance members on the implementation type.</span></span>
* <span data-ttu-id="7ba3a-115">A expectativa é armazenar estados compartilhados em um serviço singleton no contêiner de injeção de dependência.</span><span class="sxs-lookup"><span data-stu-id="7ba3a-115">The expectation is to store shared states in a singleton service in the DI container.</span></span> <span data-ttu-id="7ba3a-116">Os estados compartilhados armazenados são resolvidos no construtor da implementação do serviço gRPC.</span><span class="sxs-lookup"><span data-stu-id="7ba3a-116">The stored shared states are resolved in the constructor of the gRPC service implementation.</span></span>

<span data-ttu-id="7ba3a-117">Para obter mais informações sobre os tempos de vida do serviço, consulte <xref:fundamentals/dependency-injection#service-lifetimes>.</span><span class="sxs-lookup"><span data-stu-id="7ba3a-117">For more information on service lifetimes, see <xref:fundamentals/dependency-injection#service-lifetimes>.</span></span>

### <a name="add-a-singleton-service"></a><span data-ttu-id="7ba3a-118">Adicionar um serviço singleton</span><span class="sxs-lookup"><span data-stu-id="7ba3a-118">Add a singleton service</span></span>

<span data-ttu-id="7ba3a-119">Para facilitar a transição de uma implementação de C-core gRPC para ASP.NET Core, é possível alterar o tempo de vida da implementação do serviço do serviço com escopo para singleton.</span><span class="sxs-lookup"><span data-stu-id="7ba3a-119">To facilitate the transition from a gRPC C-core implementation to ASP.NET Core, it's possible to change the service lifetime of the service implementation from scoped to singleton.</span></span> <span data-ttu-id="7ba3a-120">Isso envolve a adição de uma instância da implementação do serviço para o contêiner de injeção de dependência:</span><span class="sxs-lookup"><span data-stu-id="7ba3a-120">This involves adding an instance of the service implementation to the DI container:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc();
    services.AddSingleton(new GreeterService());
}
```

<span data-ttu-id="7ba3a-121">No entanto, uma implementação de serviço com um tempo de vida singleton não é mais capaz de resolver os serviços com escopo por meio da injeção de construtor.</span><span class="sxs-lookup"><span data-stu-id="7ba3a-121">However, a service implementation with a singleton lifetime is no longer able to resolve scoped services through constructor injection.</span></span>

## <a name="configure-grpc-services-options"></a><span data-ttu-id="7ba3a-122">Configurar opções de serviços gRPC</span><span class="sxs-lookup"><span data-stu-id="7ba3a-122">Configure gRPC services options</span></span>

<span data-ttu-id="7ba3a-123">Em aplicativos baseados em C-core, configurações, como `grpc.max_receive_message_length` e `grpc.max_send_message_length` são configurados com `ChannelOption` quando [construindo a instância de servidor](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server__ctor_System_Collections_Generic_IEnumerable_Grpc_Core_ChannelOption__).</span><span class="sxs-lookup"><span data-stu-id="7ba3a-123">In C-core-based apps, settings such as `grpc.max_receive_message_length` and `grpc.max_send_message_length` are configured with `ChannelOption` when [constructing the Server instance](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server__ctor_System_Collections_Generic_IEnumerable_Grpc_Core_ChannelOption__).</span></span>

<span data-ttu-id="7ba3a-124">No ASP.NET Core, gRPC fornece configuração por meio de `GrpcServiceOptions` tipo.</span><span class="sxs-lookup"><span data-stu-id="7ba3a-124">In ASP.NET Core, gRPC provides configuration through the `GrpcServiceOptions` type.</span></span> <span data-ttu-id="7ba3a-125">Por exemplo, um gRPC serviço o tamanho máximo de mensagem de entrada pode ser configurado por meio de `AddGrpc`:</span><span class="sxs-lookup"><span data-stu-id="7ba3a-125">For example, a gRPC service's the maximum incoming message size can be configured via `AddGrpc`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc(options =>
    {
        options.ReceiveMaxMessageSize = 16384; // 16 MB
    });
}
```

<span data-ttu-id="7ba3a-126">Para obter mais informações sobre a configuração, consulte <xref:grpc/configuration>.</span><span class="sxs-lookup"><span data-stu-id="7ba3a-126">For more information on configuration, see <xref:grpc/configuration>.</span></span>

## <a name="logging"></a><span data-ttu-id="7ba3a-127">Registrando em log</span><span class="sxs-lookup"><span data-stu-id="7ba3a-127">Logging</span></span>

<span data-ttu-id="7ba3a-128">Aplicativos baseados em C-core contam com o `GrpcEnvironment` à [configurar o agente de log](https://grpc.io/grpc/csharp/api/Grpc.Core.GrpcEnvironment.html?q=size#Grpc_Core_GrpcEnvironment_SetLogger_Grpc_Core_Logging_ILogger_) para fins de depuração.</span><span class="sxs-lookup"><span data-stu-id="7ba3a-128">C-core-based apps rely on the `GrpcEnvironment` to [configure the logger](https://grpc.io/grpc/csharp/api/Grpc.Core.GrpcEnvironment.html?q=size#Grpc_Core_GrpcEnvironment_SetLogger_Grpc_Core_Logging_ILogger_) for debugging purposes.</span></span> <span data-ttu-id="7ba3a-129">A pilha do ASP.NET Core fornece essa funcionalidade por meio de [API de registro em log](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="7ba3a-129">The ASP.NET Core stack provides this functionality through the [Logging API](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="7ba3a-130">Por exemplo, um agente de log pode ser adicionado ao serviço gRPC por meio da injeção de construtor:</span><span class="sxs-lookup"><span data-stu-id="7ba3a-130">For example, a logger can be added to the gRPC service via constructor injection:</span></span>

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

## <a name="https"></a><span data-ttu-id="7ba3a-131">HTTPS</span><span class="sxs-lookup"><span data-stu-id="7ba3a-131">HTTPS</span></span>

<span data-ttu-id="7ba3a-132">Aplicativos baseados em C-core configurar HTTPS por meio de [Server.Ports propriedade](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server_Ports).</span><span class="sxs-lookup"><span data-stu-id="7ba3a-132">C-core-based apps configure HTTPS through the [Server.Ports property](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server_Ports).</span></span> <span data-ttu-id="7ba3a-133">Um conceito semelhante é usado para configurar os servidores no ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7ba3a-133">A similar concept is used to configure servers in ASP.NET Core.</span></span> <span data-ttu-id="7ba3a-134">Por exemplo, o Kestrel usa [configuração de ponto de extremidade](xref:fundamentals/servers/kestrel#endpoint-configuration) para essa funcionalidade.</span><span class="sxs-lookup"><span data-stu-id="7ba3a-134">For example, Kestrel uses [endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) for this functionality.</span></span>

## <a name="interceptors-and-middleware"></a><span data-ttu-id="7ba3a-135">Interceptores e Middleware</span><span class="sxs-lookup"><span data-stu-id="7ba3a-135">Interceptors and Middleware</span></span>

<span data-ttu-id="7ba3a-136">ASP.NET Core [middleware](xref:fundamentals/middleware/index) oferece funcionalidades semelhantes em comparação comparadas interceptores em aplicativos baseados em C-core gRPC.</span><span class="sxs-lookup"><span data-stu-id="7ba3a-136">ASP.NET Core [middleware](xref:fundamentals/middleware/index) offers similar functionalities compared to interceptors in C-core-based gRPC apps.</span></span> <span data-ttu-id="7ba3a-137">Middleware e interceptores são conceitualmente as mesmas como ambos são usados para construir um pipeline que manipula uma solicitação gRPC.</span><span class="sxs-lookup"><span data-stu-id="7ba3a-137">Middleware and interceptors are conceptually the same as both are used to construct a pipeline that handles a gRPC request.</span></span> <span data-ttu-id="7ba3a-138">Ambos permitem que o trabalho seja realizado antes ou após o próximo componente no pipeline.</span><span class="sxs-lookup"><span data-stu-id="7ba3a-138">They both allow work to be performed before or after the next component in the pipeline.</span></span> <span data-ttu-id="7ba3a-139">No entanto, o middleware do ASP.NET Core opera nas mensagens HTTP/2 subjacentes, enquanto interceptores operam na camada de abstração usando gRPC a [ServerCallContext](https://grpc.io/grpc/csharp/api/Grpc.Core.ServerCallContext.html).</span><span class="sxs-lookup"><span data-stu-id="7ba3a-139">However, ASP.NET Core middleware operates on the underlying HTTP/2 messages, while interceptors operate on the gRPC layer of abstraction using the [ServerCallContext](https://grpc.io/grpc/csharp/api/Grpc.Core.ServerCallContext.html).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7ba3a-140">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="7ba3a-140">Additional resources</span></span>

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/aspnetcore>
* <xref:tutorials/grpc/grpc-start>