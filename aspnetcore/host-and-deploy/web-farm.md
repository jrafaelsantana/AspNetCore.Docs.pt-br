---
title: Hospedar o ASP.NET Core em um web farm
author: rick-anderson
description: Saiba como hospedar várias instâncias de um aplicativo ASP.NET Core com recursos compartilhados em um ambiente de web farm.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/13/2020
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
uid: host-and-deploy/web-farm
ms.openlocfilehash: 13f1ad5dcd4a230ec05b08c402f4ee9e455c3c29
ms.sourcegitcommit: 65add17f74a29a647d812b04517e46cbc78258f9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/19/2020
ms.locfileid: "88634131"
---
# <a name="host-aspnet-core-in-a-web-farm"></a>Hospedar o ASP.NET Core em um web farm

Por [Chris Ross](https://github.com/Tratcher)

Um *web farm* é um grupo de dois ou mais servidores web (ou *nós*) que hospedam várias instâncias de um aplicativo. Quando as solicitações de usuários chegam a um web farm, um *balanceador de carga* distribui as solicitações para os nós de farm da web. Os web farms melhoram:

* **Confiabilidade/disponibilidade**: quando um ou mais nós falham, o balanceador de carga pode rotear solicitações para outros nós de funcionamento para continuar processando solicitações.
* **Capacidade/desempenho**: vários nós podem processar mais solicitações do que um único servidor. O balanceador de carga equilibra a carga de trabalho ao distribuir as solicitações para os nós.
* **Escalabilidade**: quando é necessária mais ou menos capacidade, o número de nós ativos pode ser aumentado ou diminuído para corresponder à carga de trabalho. Tecnologias de plataforma de web farm, como o [Serviço de Aplicativo do Azure](https://azure.microsoft.com/services/app-service/), podem adicionar ou remover nós por solicitação do administrador do sistema ou automaticamente sem a intervenção humana.
* **Manutenção**: os nós de um Web farm podem contar com um conjunto de serviços compartilhados, o que resulta em um gerenciamento de sistema mais fácil. Por exemplo, os nós de um web farm podem contar com um servidor de banco de dados individual e um local de rede comum para recursos estáticos, como imagens e arquivos para download.

Este tópico descreve as configurações e dependências para aplicativos ASP.NET Core hospedados em um web farm que conta com recursos compartilhados.

## <a name="general-configuration"></a>Configuração geral

<xref:host-and-deploy/index>  
Aprenda como configurar ambientes de hospedagem e implantar aplicativos ASP.NET Core. Configure um gerenciador de processo em cada nó do web farm para automatizar o início e a reinicialização do aplicativo. Cada nó requer o runtime do ASP.NET Core. Para saber mais, confira os tópicos na área [Hospedar e implantar](xref:host-and-deploy/index) da documentação.

<xref:host-and-deploy/proxy-load-balancer>  
Saiba mais sobre a configuração para aplicativos hospedados por trás de servidores proxy e balanceadores de carga, o que muitas vezes oculta informações de solicitação importantes.

<xref:host-and-deploy/azure-apps/index>  
[Serviço de Aplicativo do Azure](https://azure.microsoft.com/services/app-service/) é um [serviço de plataforma de computação em nuvem da Microsoft](https://azure.microsoft.com/) para hospedar aplicativos Web, incluindo o ASP.NET Core. O Serviço de Aplicativo é uma plataforma totalmente gerenciada que fornece escalabilidade automática, balanceamento de carga, aplicação de patch e implantação contínua.

## <a name="app-data"></a>Dados do aplicativo

Quando um aplicativo é dimensionado para várias instâncias, pode haver um estado do aplicativo que exija o compartilhamento entre os nós. Se o estado for transitório, considere a possibilidade de compartilhar um [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache). Se o estado compartilhado exigir persistência, considere armazenar o estado compartilhado em um banco de dados.

## <a name="required-configuration"></a>Configuração necessária

O serviço de cache e a proteção de dados exigem uma configuração para aplicativos implantados em um web farm.

### <a name="data-protection"></a>Proteção de dados

O [sistema de proteção de dados do ASP.NET Core](xref:security/data-protection/introduction) é usado por aplicativos para proteger os dados. A proteção de dados baseia-se em um conjunto de chaves de criptografia armazenados em um *token de autenticação*. Quando o sistema de Proteção de dados é inicializado, ele aplica [as configurações padrão](xref:security/data-protection/configuration/default-settings) que armazenam localmente o token de autenticação. Sob a configuração padrão, um token de autenticação exclusivo é armazenado em cada nó do web farm. Consequentemente, cada nó do web farm não pode descriptografar os dados criptografados por um aplicativo em qualquer outro nó. A configuração padrão normalmente não é adequada para hospedagem de aplicativos em um web farm. Uma alternativa à implementação de um token de autenticação compartilhado é sempre rotear as solicitações do usuário para o mesmo nó. Para saber mais sobre a configuração do sistema de Proteção de dados para implantações de web farm, confira <xref:security/data-protection/configuration/overview>.

### <a name="caching"></a>Cache

Em um ambiente de web farm, o mecanismo de cache deve compartilhar os itens em cache pelos nós do web farm. O serviço de cache deve contar com um cache Redis comum, um banco de dados compartilhado do SQL Server ou uma implementação personalizada de cache que compartilha os itens em cache pelo web farm. Para obter mais informações, consulte <xref:performance/caching/distributed>.

## <a name="dependent-components"></a>Componentes dependentes

Os cenários a seguir não exigem configuração adicional, mas dependem de tecnologias que exigem configurações para web farms.

| Cenário | Depende de &hellip; |
| -------- | ------------------- |
| Autenticação | Proteção de dados (confira <xref:security/data-protection/configuration/overview>).<br><br>Para obter mais informações, consulte <xref:security/authentication/cookie> e <xref:security/cookie-sharing>. |
| Identity | Configuração e autenticação do banco de dados.<br><br>Para obter mais informações, consulte <xref:security/authentication/identity>. |
| Session | Proteção de dados (criptografados cookie ) (consulte <xref:security/data-protection/configuration/overview> ) e caching (consulte <xref:performance/caching/distributed> ).<br><br>Para obter mais informações, consulte [Gerenciamento de sessão e estado: estado da sessão](xref:fundamentals/app-state#session-state). |
| TempData | Proteção de dados (criptografados cookie ) (consulte <xref:security/data-protection/configuration/overview> ) ou sessão (consulte [Gerenciamento de sessão e estado: estado de sessão](xref:fundamentals/app-state#session-state)).<br><br>Para obter mais informações, consulte [Gerenciamento de sessão e estado: TempData](xref:fundamentals/app-state#tempdata). |
| Antifalsificação | Proteção de dados (confira <xref:security/data-protection/configuration/overview>).<br><br>Para obter mais informações, consulte <xref:security/anti-request-forgery>. |

## <a name="troubleshoot"></a>Solucionar problemas

### <a name="data-protection-and-caching"></a>Proteção de dados e cache

Quando a proteção de dados ou cache não está configurada para um ambiente de web farm, erros intermitentes ocorrem quando as solicitações são processadas. Isso acontece porque os nós não compartilham os mesmos recursos e as solicitações do usuário não são sempre roteadas para o mesmo nó.

Considere um usuário que entra no aplicativo usando a cookie autenticação do. O usuário entra no aplicativo em um nó do web farm. Se a próxima solicitação chegar ao mesmo nó em que se conectou, o aplicativo poderá descriptografar a autenticação cookie e permitir o acesso ao recurso do aplicativo. Se a próxima solicitação chegar em um nó diferente, o aplicativo não poderá descriptografar a autenticação cookie do nó em que o usuário se conectou e a autorização para o recurso solicitado falhará.

Quando qualquer um dos sintomas a seguir ocorre **intermitentemente**, o problema geralmente é rastreado para proteção de dados inadequada ou configuração de cache para um ambiente de Web farm:

* Quebras de autenticação: a autenticação cookie está configurada incorretamente ou não pode ser descriptografada. Falha de login OpenIdConnect ou OAuth (Facebook, Microsoft, Twitter) com o erro "Falha de correlação".
* Quebras de autorização: Identity são perdidas.
* O estado de sessão perde os dados.
* Os itens em cache desaparecem.
* O TempData falhará.
* As postagens falham: a verificação antifalsificação falha.

Para saber mais sobre a configuração de Proteção de dados para implantações de web farm, confira <xref:security/data-protection/configuration/overview>. Para saber mais sobre a configuração de cache para implantações de web farm, confira <xref:performance/caching/distributed>.

## <a name="obtain-data-from-apps"></a>Obter dados de aplicativos

Se os aplicativos do web farm forem capazes de responder às solicitações, obtenha as solicitações, conexão e dados adicionais dos aplicativos que usarem o middleware embutido de terminal. Para saber mais e obter um código de exemplo, consulte <xref:test/troubleshoot#obtain-data-from-an-app>.

## <a name="additional-resources"></a>Recursos adicionais

* [Extensão de script personalizado para Windows](/azure/virtual-machines/extensions/custom-script-windows): baixa e executa scripts em máquinas virtuais do Azure, que é útil para configuração de pós-implantação e instalação de software.
* <xref:host-and-deploy/proxy-load-balancer>
 