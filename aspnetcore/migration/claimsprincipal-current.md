---
title: Migrar de ClaimsPrincipal. Current
author: mjrousos
description: Saiba como migrar para fora de ClaimsPrincipal. Current para recuperar a identidade e as declarações atuais do usuário autenticado no ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 03/26/2019
no-loc:
- ASP.NET Core Identity
- cookie
- Cookie
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: migration/claimsprincipal-current
ms.openlocfilehash: 426fd90374a460cb283d0d3ba921e1312fb17940
ms.sourcegitcommit: 65add17f74a29a647d812b04517e46cbc78258f9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/19/2020
ms.locfileid: "88634066"
---
# <a name="migrate-from-claimsprincipalcurrent"></a>Migrar de ClaimsPrincipal. Current

Em projetos ASP.NET 4. x, era comum usar [ClaimsPrincipal. Current](/dotnet/api/system.security.claims.claimsprincipal.current) para recuperar a identidade e as declarações atuais do usuário autenticado. No ASP.NET Core, essa propriedade não é mais definida. O código que estava dependendo dele precisa ser atualizado para obter a identidade do usuário autenticado atual por meio de um meio diferente.

## <a name="context-specific-state-instead-of-static-state"></a>Estado específico de contexto em vez de estado estático

Ao usar ASP.NET Core, os valores de ambos `ClaimsPrincipal.Current` e `Thread.CurrentPrincipal` não são definidos. Essas propriedades representam o estado estático, que ASP.NET Core geralmente evita. Em vez disso, ASP.NET Core usa [injeção de dependência](xref:fundamentals/dependency-injection) (di) para fornecer dependências como a identidade do usuário atual. Obter a identidade do usuário atual de DI também é mais testado, já que as identidades de teste podem ser facilmente injetadas.

## <a name="retrieve-the-current-user-in-an-aspnet-core-app"></a>Recuperar o usuário atual em um aplicativo ASP.NET Core

Há várias opções para recuperar os usuários autenticados atuais `ClaimsPrincipal` no ASP.NET Core em vez de `ClaimsPrincipal.Current` :

* **ControllerBase. User**. Os controladores MVC podem acessar o usuário autenticado atual com a propriedade [User](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.user) .
* **HttpContext. User**. Os componentes com acesso ao atual `HttpContext` (middleware, por exemplo) podem obter o usuário atual do `ClaimsPrincipal` [HttpContext. User](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user).
* **Passado do chamador**. As bibliotecas sem acesso ao atual `HttpContext` geralmente são chamadas de controladores ou componentes de middleware e podem ter a identidade do usuário atual passada como um argumento.
* **IHttpContextAccessor**. O projeto que está sendo migrado para ASP.NET Core pode ser muito grande para passar facilmente a identidade do usuário atual para todos os locais necessários. Nesses casos, [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) pode ser usado como uma solução alternativa. `IHttpContextAccessor` é capaz de acessar o atual `HttpContext` (se houver). Se DI estiver sendo usado, consulte <xref:fundamentals/httpcontext> . Uma solução de curto prazo para obter a identidade do usuário atual no código que ainda não foi atualizada para trabalhar com a arquitetura controlada por injeção de ASP.NET Core seria:

  * Torne `IHttpContextAccessor` disponível no contêiner de di chamando [AddHttpContextAccessor](https://github.com/aspnet/Hosting/issues/793) em `Startup.ConfigureServices` .
  * Obtenha uma instância do `IHttpContextAccessor` durante a inicialização e armazene-a em uma variável estática. A instância é disponibilizada para o código que estava recuperando anteriormente o usuário atual de uma propriedade estática.
  * Recupere o uso do usuário `ClaimsPrincipal` atual `HttpContextAccessor.HttpContext?.User` . Se esse código for usado fora do contexto de uma solicitação HTTP, o `HttpContext` será nulo.

A opção final, usando uma `IHttpContextAccessor` instância armazenada em uma variável estática, é contrária à ASP.NET Core princípio de referenciar dependências injetadas a dependências estáticas. Planeje eventualmente recuperar `IHttpContextAccessor` instâncias de di em vez disso. Um auxiliar estático pode ser uma ponte útil, porém, ao migrar grandes aplicativos ASP.NET existentes que usam o `ClaimsPrincipal.Current` .
