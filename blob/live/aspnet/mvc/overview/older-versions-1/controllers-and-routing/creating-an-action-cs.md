---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-cs
title: "Создание действия (C#) | Документы Microsoft"
author: microsoft
description: "Дополнительные сведения о добавлении нового действия для контроллера ASP.NET MVC. Дополнительные сведения о требованиях для метода для действия."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: cb33b28c-3025-4bd1-a1fa-eaa3af7bb56f
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-cs
msc.type: authoredcontent
ms.openlocfilehash: 8b751dc7e34951be33e7c27a3429c383a3e1e1c7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="creating-an-action-c"></a><span data-ttu-id="6b3bb-104">Создание действия (C#)</span><span class="sxs-lookup"><span data-stu-id="6b3bb-104">Creating an Action (C#)</span></span>
====================
<span data-ttu-id="6b3bb-105">по [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="6b3bb-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="6b3bb-106">Дополнительные сведения о добавлении нового действия для контроллера ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="6b3bb-106">Learn how to add a new action to an ASP.NET MVC controller.</span></span> <span data-ttu-id="6b3bb-107">Дополнительные сведения о требованиях для метода для действия.</span><span class="sxs-lookup"><span data-stu-id="6b3bb-107">Learn about the requirements for a method to be an action.</span></span>


<span data-ttu-id="6b3bb-108">Целью данного учебника является объясняется, как можно создать новое действие контроллера.</span><span class="sxs-lookup"><span data-stu-id="6b3bb-108">The goal of this tutorial is to explain how you can create a new controller action.</span></span> <span data-ttu-id="6b3bb-109">Дополнительные сведения о требованиях к метода действия.</span><span class="sxs-lookup"><span data-stu-id="6b3bb-109">You learn about the requirements of an action method.</span></span> <span data-ttu-id="6b3bb-110">Вы также узнаете, как предотвратить раскрытие в качестве действия метода.</span><span class="sxs-lookup"><span data-stu-id="6b3bb-110">You also learn how to prevent a method from being exposed as an action.</span></span>

## <a name="adding-an-action-to-a-controller"></a><span data-ttu-id="6b3bb-111">Добавление действия к контроллеру</span><span class="sxs-lookup"><span data-stu-id="6b3bb-111">Adding an Action to a Controller</span></span>

<span data-ttu-id="6b3bb-112">Добавьте новое действие к контроллеру, путем добавления нового метода к контроллеру.</span><span class="sxs-lookup"><span data-stu-id="6b3bb-112">You add a new action to a controller by adding a new method to the controller.</span></span> <span data-ttu-id="6b3bb-113">Например контроллер в листинге 1 содержит действия с именем Index() и действие с именем SayHello().</span><span class="sxs-lookup"><span data-stu-id="6b3bb-113">For example, the controller in Listing 1 contains an action named Index() and an action named SayHello().</span></span> <span data-ttu-id="6b3bb-114">Оба метода доступны как действия.</span><span class="sxs-lookup"><span data-stu-id="6b3bb-114">Both methods are exposed as actions.</span></span>

<span data-ttu-id="6b3bb-115">**Листинг 1 - Controllers\HomeController.cs**</span><span class="sxs-lookup"><span data-stu-id="6b3bb-115">**Listing 1 - Controllers\HomeController.cs**</span></span>

[!code-csharp[Main](creating-an-action-cs/samples/sample1.cs)]

<span data-ttu-id="6b3bb-116">Чтобы предоставляться вселенной как действие, метод должны соответствовать определенным требованиям:</span><span class="sxs-lookup"><span data-stu-id="6b3bb-116">In order to be exposed to the universe as an action, a method must meet certain requirements:</span></span>

- <span data-ttu-id="6b3bb-117">Метод должен быть открытым.</span><span class="sxs-lookup"><span data-stu-id="6b3bb-117">The method must be public.</span></span>
- <span data-ttu-id="6b3bb-118">Метод не может быть статическим.</span><span class="sxs-lookup"><span data-stu-id="6b3bb-118">The method cannot be a static method.</span></span>
- <span data-ttu-id="6b3bb-119">Метод не может быть методом расширения.</span><span class="sxs-lookup"><span data-stu-id="6b3bb-119">The method cannot be an extension method.</span></span>
- <span data-ttu-id="6b3bb-120">Метод не может быть конструктор, метод получения или задания.</span><span class="sxs-lookup"><span data-stu-id="6b3bb-120">The method cannot be a constructor, getter, or setter.</span></span>
- <span data-ttu-id="6b3bb-121">Метод не может иметь открытые универсальные типы.</span><span class="sxs-lookup"><span data-stu-id="6b3bb-121">The method cannot have open generic types.</span></span>
- <span data-ttu-id="6b3bb-122">Метод не является методом базового класса контроллера.</span><span class="sxs-lookup"><span data-stu-id="6b3bb-122">The method is not a method of the controller base class.</span></span>
- <span data-ttu-id="6b3bb-123">Этот метод не может содержать **ref** или **out** параметров.</span><span class="sxs-lookup"><span data-stu-id="6b3bb-123">The method cannot contain **ref** or **out** parameters.</span></span>

<span data-ttu-id="6b3bb-124">Обратите внимание, что нет никаких ограничений на тип возвращаемого значения действия контроллера.</span><span class="sxs-lookup"><span data-stu-id="6b3bb-124">Notice that there are no restrictions on the return type of a controller action.</span></span> <span data-ttu-id="6b3bb-125">Действие контроллера может вернуть строку, DateTime, экземпляр класса Random или void.</span><span class="sxs-lookup"><span data-stu-id="6b3bb-125">A controller action can return a string, a DateTime, an instance of the Random class, or void.</span></span> <span data-ttu-id="6b3bb-126">Платформа ASP.NET MVC будет преобразовать любой тип возврата, не результат действия в строку и просмотру строки в браузере.</span><span class="sxs-lookup"><span data-stu-id="6b3bb-126">The ASP.NET MVC framework will convert any return type that is not an action result into a string and render the string to the browser.</span></span>

<span data-ttu-id="6b3bb-127">При добавлении любого метода, который не нарушает эти требования к контроллеру, метод указывается в виде действия контроллера.</span><span class="sxs-lookup"><span data-stu-id="6b3bb-127">When you add any method that does not violate these requirements to a controller, the method is exposed as a controller action.</span></span> <span data-ttu-id="6b3bb-128">Здесь следует соблюдать осторожность.</span><span class="sxs-lookup"><span data-stu-id="6b3bb-128">Be careful here.</span></span> <span data-ttu-id="6b3bb-129">Действие контроллера может вызываться все, кто подключен к Интернету.</span><span class="sxs-lookup"><span data-stu-id="6b3bb-129">A controller action can be invoked by anyone connected to the Internet.</span></span> <span data-ttu-id="6b3bb-130">Нет, например, создать действие DeleteMyWebsite() контроллера.</span><span class="sxs-lookup"><span data-stu-id="6b3bb-130">Do not, for example, create a DeleteMyWebsite() controller action.</span></span>

## <a name="preventing-a-public-method-from-being-invoked"></a><span data-ttu-id="6b3bb-131">Предотвращение открытый метод вызова</span><span class="sxs-lookup"><span data-stu-id="6b3bb-131">Preventing a Public Method from Being Invoked</span></span>

<span data-ttu-id="6b3bb-132">Если необходимо создать открытый метод в класс контроллера, и вы не хотите предоставлять метод как действие контроллера можно предотвратить метод вызова с помощью атрибута [NonAction].</span><span class="sxs-lookup"><span data-stu-id="6b3bb-132">If you need to create a public method in a controller class and you don't want to expose the method as a controller action then you can prevent the method from being invoked by using the [NonAction] attribute.</span></span> <span data-ttu-id="6b3bb-133">Например контроллер в списке 2 содержит открытый метод с именем CompanySecrets(), отмеченный атрибутом [NonAction].</span><span class="sxs-lookup"><span data-stu-id="6b3bb-133">For example, the controller in Listing 2 contains a public method named CompanySecrets() that is decorated with the [NonAction] attribute.</span></span>

<span data-ttu-id="6b3bb-134">**Листинг 2 - Controllers\WorkController.cs**</span><span class="sxs-lookup"><span data-stu-id="6b3bb-134">**Listing 2 - Controllers\WorkController.cs**</span></span>

[!code-csharp[Main](creating-an-action-cs/samples/sample2.cs)]

<span data-ttu-id="6b3bb-135">При попытке вызвать действие контроллера CompanySecrets(), введя в адресной строке браузера /Work/CompanySecrets вы сможете получить сообщение об ошибке на рис. 1.</span><span class="sxs-lookup"><span data-stu-id="6b3bb-135">If you attempt to invoke the CompanySecrets() controller action by typing /Work/CompanySecrets into the address bar of your browser then you'll get the error message in Figure 1.</span></span>


<span data-ttu-id="6b3bb-136">[![Вызов метода NonAction](creating-an-action-cs/_static/image1.jpg)](creating-an-action-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="6b3bb-136">[![Invoking a NonAction method](creating-an-action-cs/_static/image1.jpg)](creating-an-action-cs/_static/image1.png)</span></span>

<span data-ttu-id="6b3bb-137">**На рисунке 01**: вызов метода NonAction ([Просмотр полноразмерное изображение](creating-an-action-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="6b3bb-137">**Figure 01**: Invoking a NonAction method([Click to view full-size image](creating-an-action-cs/_static/image2.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="6b3bb-138">[Назад](creating-a-controller-cs.md)
[Вперед](asp-net-mvc-routing-overview-vb.md)</span><span class="sxs-lookup"><span data-stu-id="6b3bb-138">[Previous](creating-a-controller-cs.md)
[Next](asp-net-mvc-routing-overview-vb.md)</span></span>