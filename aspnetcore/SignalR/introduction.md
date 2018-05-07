---
title: Введение в ASP.NET Core SignalR
author: rachelappel
description: Узнайте, как библиотека ASP.NET Core SignalR упрощает добавление функциональности в режиме реального времени для приложения.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 04/25/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/introduction
ms.openlocfilehash: 190dfe9eac95be646b458870ac4ee95f681f45d7
ms.sourcegitcommit: 2ab550f8c46e1a8a5d45e58be44d151c676af256
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2018
---
# <a name="introduction-to-aspnet-core-signalr"></a><span data-ttu-id="c788d-103">Введение в ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="c788d-103">Introduction to ASP.NET Core SignalR</span></span>

<span data-ttu-id="c788d-104">Автор: [Рэйчел Аппель](https://twitter.com/rachelappel) (Rachel Appel)</span><span class="sxs-lookup"><span data-stu-id="c788d-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>


[!INCLUDE [2.1 preview notice](~/includes/2.1.md)]

## <a name="what-is-signalr"></a><span data-ttu-id="c788d-105">Что такое SignalR</span><span class="sxs-lookup"><span data-stu-id="c788d-105">What is SignalR?</span></span>

<span data-ttu-id="c788d-106">SignalR ASP.NET Core — это библиотека, упрощает создание новых функций реального времени для приложения.</span><span class="sxs-lookup"><span data-stu-id="c788d-106">ASP.NET Core SignalR is a library that simplifies adding real-time web functionality to apps.</span></span> <span data-ttu-id="c788d-107">В режиме реального времени веб-функций позволяет коду серверные push-содержимое клиентам мгновенно.</span><span class="sxs-lookup"><span data-stu-id="c788d-107">Real-time web functionality enables server-side code to push content to clients instantly.</span></span>

<span data-ttu-id="c788d-108">Хорошими кандидатами для SignalR:</span><span class="sxs-lookup"><span data-stu-id="c788d-108">Good candidates for SignalR:</span></span>

* <span data-ttu-id="c788d-109">Приложения, которые требуют высокой частотой обновления с сервера.</span><span class="sxs-lookup"><span data-stu-id="c788d-109">Apps that require high frequency updates from the server.</span></span> <span data-ttu-id="c788d-110">Примерами являются игры, социальные сети, с правом голоса, аукционов, схемы и GPS приложений.</span><span class="sxs-lookup"><span data-stu-id="c788d-110">Examples are gaming, social networks, voting, auction, maps, and GPS apps.</span></span>
* <span data-ttu-id="c788d-111">Панели мониторинга и мониторинга приложений.</span><span class="sxs-lookup"><span data-stu-id="c788d-111">Dashboards and monitoring apps.</span></span> <span data-ttu-id="c788d-112">Пример: информационные панели компании, мгновенного обновления продаж, или передаваться предупреждения.</span><span class="sxs-lookup"><span data-stu-id="c788d-112">Examples include company dashboards, instant sales updates, or travel alerts.</span></span>
* <span data-ttu-id="c788d-113">Приложения для совместной работы.</span><span class="sxs-lookup"><span data-stu-id="c788d-113">Collaborative apps.</span></span> <span data-ttu-id="c788d-114">Доска приложений и группы программного обеспечения являются примерами приложений для совместной работы.</span><span class="sxs-lookup"><span data-stu-id="c788d-114">Whiteboard apps and team meeting software are examples of collaborative apps.</span></span>
* <span data-ttu-id="c788d-115">Приложения, требующие уведомления.</span><span class="sxs-lookup"><span data-stu-id="c788d-115">Apps that require notifications.</span></span> <span data-ttu-id="c788d-116">Уведомления о использовать социальных сетей, электронной почты, чат, игры, движения предупреждения и другие приложения.</span><span class="sxs-lookup"><span data-stu-id="c788d-116">Social networks, email, chat, games, travel alerts, and many other apps use notifications.</span></span>

<span data-ttu-id="c788d-117">SignalR предоставляет API-Интерфейс для создания клиента server [удаленных вызовов процедур (RPC)](https://wikipedia.org/wiki/Remote_procedure_call).</span><span class="sxs-lookup"><span data-stu-id="c788d-117">SignalR provides an API for creating server-to-client [remote procedure calls (RPC)](https://wikipedia.org/wiki/Remote_procedure_call).</span></span> <span data-ttu-id="c788d-118">RPC вызывают функции JavaScript на клиентах из серверного кода .NET Core.</span><span class="sxs-lookup"><span data-stu-id="c788d-118">The RPCs call JavaScript functions on clients from server-side .NET Core code.</span></span>

<span data-ttu-id="c788d-119">SignalR для ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="c788d-119">SignalR for ASP.NET Core:</span></span>

* <span data-ttu-id="c788d-120">Автоматически обрабатывает управление соединениями.</span><span class="sxs-lookup"><span data-stu-id="c788d-120">Handles connection management automatically.</span></span>
* <span data-ttu-id="c788d-121">Включает широковещательная рассылка сообщений для всех подключенных клиентов одновременно.</span><span class="sxs-lookup"><span data-stu-id="c788d-121">Enables broadcasting messages to all connected clients simultaneously.</span></span> <span data-ttu-id="c788d-122">Например участников чата.</span><span class="sxs-lookup"><span data-stu-id="c788d-122">For example, a chat room.</span></span>
* <span data-ttu-id="c788d-123">Разрешает передачу сообщений для конкретных клиентов или групп клиентов.</span><span class="sxs-lookup"><span data-stu-id="c788d-123">Enables sending messages to specific clients or groups of clients.</span></span>
* <span data-ttu-id="c788d-124">Имеет открытый исходный код в [GitHub](https://github.com/aspnet/signalr).</span><span class="sxs-lookup"><span data-stu-id="c788d-124">Is open-sourced at [GitHub](https://github.com/aspnet/signalr).</span></span>
* <span data-ttu-id="c788d-125">Масштабируемость.</span><span class="sxs-lookup"><span data-stu-id="c788d-125">Scalable.</span></span>

<span data-ttu-id="c788d-126">Соединение между клиентом и сервером является постоянным, в отличие от HTTP-соединение.</span><span class="sxs-lookup"><span data-stu-id="c788d-126">The connection between the client and server is persistent, unlike an HTTP connection.</span></span>

## <a name="transports"></a><span data-ttu-id="c788d-127">Транспорты</span><span class="sxs-lookup"><span data-stu-id="c788d-127">Transports</span></span>

<span data-ttu-id="c788d-128">Краткие описания SignalR через ряд методов для построения в режиме реального времени веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="c788d-128">SignalR abstracts over a number of techniques for building real-time web applications.</span></span> <span data-ttu-id="c788d-129">[WebSockets](https://tools.ietf.org/html/rfc7118) является оптимальной транспортом, но другие методы, как события Server-Sent и Long опроса можно использовать при тех недоступны.</span><span class="sxs-lookup"><span data-stu-id="c788d-129">[WebSockets](https://tools.ietf.org/html/rfc7118) is the optimal transport, but other techniques like Server-Sent Events and Long Polling can be used when those aren't available.</span></span> <span data-ttu-id="c788d-130">SignalR автоматически обнаружит и инициализации транспорта на возможности, поддерживаемые на сервере и клиенте.</span><span class="sxs-lookup"><span data-stu-id="c788d-130">SignalR will automatically detect and initialize the appropriate transport based on features supported on the server and client.</span></span>

## <a name="hubs"></a><span data-ttu-id="c788d-131">Концентраторы</span><span class="sxs-lookup"><span data-stu-id="c788d-131">Hubs</span></span>

<span data-ttu-id="c788d-132">SignalR использует концентраторы для взаимодействия между клиентами и серверами.</span><span class="sxs-lookup"><span data-stu-id="c788d-132">SignalR uses hubs to communicate between clients and servers.</span></span>

<span data-ttu-id="c788d-133">Концентратор является высокого уровня конвейера, который позволяет клиенту и серверу вызывать методы друг от друга.</span><span class="sxs-lookup"><span data-stu-id="c788d-133">A hub is a high-level pipeline that allows your client and server to call methods on each other.</span></span> <span data-ttu-id="c788d-134">SignalR обрабатывает доставку за пределами границ машины автоматически, позволив клиентам вызывать методы на сервере, как легко, как локальные методы и наоборот.</span><span class="sxs-lookup"><span data-stu-id="c788d-134">SignalR handles the dispatching across machine boundaries automatically, allowing clients to call methods on the server as easily as local methods, and vice versa.</span></span> <span data-ttu-id="c788d-135">Концентраторы разрешить передачи строго типизированных параметров в методы, что позволяет привязки модели.</span><span class="sxs-lookup"><span data-stu-id="c788d-135">Hubs allow passing strongly-typed parameters to methods, which enables model binding.</span></span> <span data-ttu-id="c788d-136">SignalR обеспечивает два встроенных концентратора протокола: протокол текст на основе JSON и двоичный протокол на основе [MessagePack](https://msgpack.org/).</span><span class="sxs-lookup"><span data-stu-id="c788d-136">SignalR provides two built-in hub protocols: a text protocol based on JSON and a binary protocol based on [MessagePack](https://msgpack.org/).</span></span>  <span data-ttu-id="c788d-137">MessagePack обычно создает сообщения меньше, чем при использовании JSON.</span><span class="sxs-lookup"><span data-stu-id="c788d-137">MessagePack generally creates smaller messages than when using JSON.</span></span> <span data-ttu-id="c788d-138">Должен поддерживать старых браузерах [XHR уровня 2](https://caniuse.com/#feat=xhr2) для обеспечения поддержки протокола MessagePack.</span><span class="sxs-lookup"><span data-stu-id="c788d-138">Older browsers must support [XHR level 2](https://caniuse.com/#feat=xhr2) to provide MessagePack protocol support.</span></span>

<span data-ttu-id="c788d-139">Концентраторы вызывать код на стороне клиента, отправка сообщений с помощью активный транспорт.</span><span class="sxs-lookup"><span data-stu-id="c788d-139">Hubs call client-side code by sending messages using the active transport.</span></span> <span data-ttu-id="c788d-140">Сообщения содержат имя и параметры клиентского метода.</span><span class="sxs-lookup"><span data-stu-id="c788d-140">The messages contain the name and parameters of the client-side method.</span></span> <span data-ttu-id="c788d-141">Объекты передаются как параметры метода десериализуются с помощью настроенных протокола.</span><span class="sxs-lookup"><span data-stu-id="c788d-141">Objects sent as method parameters are deserialized using the configured protocol.</span></span> <span data-ttu-id="c788d-142">Клиент пытается соответствовать имени метода в код на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="c788d-142">The client tries to match the name to a method in the client-side code.</span></span> <span data-ttu-id="c788d-143">При появлении совпадения, метод клиента выполняется с использованием данных десериализованный параметра.</span><span class="sxs-lookup"><span data-stu-id="c788d-143">When a match happens, the client method runs using the deserialized parameter data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c788d-144">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="c788d-144">Additional resources</span></span>

* [<span data-ttu-id="c788d-145">Начало работы с SignalR для ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c788d-145">Get started with SignalR for ASP.NET Core</span></span>](xref:signalr/get-started)
* [<span data-ttu-id="c788d-146">Поддерживаемые платформы</span><span class="sxs-lookup"><span data-stu-id="c788d-146">Supported Platforms</span></span>](xref:signalr/supported-platforms)
* [<span data-ttu-id="c788d-147">Центры</span><span class="sxs-lookup"><span data-stu-id="c788d-147">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="c788d-148">Клиент JavaScript</span><span class="sxs-lookup"><span data-stu-id="c788d-148">JavaScript client</span></span>](xref:signalr/javascript-client)