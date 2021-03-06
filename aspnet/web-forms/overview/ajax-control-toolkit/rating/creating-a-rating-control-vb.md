---
uid: web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-vb
title: Создание элемента управления Rating (VB) | Документация Майкрософт
author: wenz
description: Многие веб-узлы, от электронной коммерции на сайты сообщества, предлагают пользователям скорость статьи или элементов. Обычно для этого требуется некоторые усилия на написание кода, но у нас есть...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 6d0d70f4-725e-4258-8ae8-24a6ba1ddbf7
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-vb
msc.type: authoredcontent
ms.openlocfilehash: b229d0155fbab0437c644b41424164e4c655f5e5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41839093"
---
<a name="creating-a-rating-control-vb"></a>Создание элемента управления Rating (VB)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Скачать код](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/rating0.vb.zip) или [скачать PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/rating0VB.pdf)

> Многие веб-узлы, от электронной коммерции на сайты сообщества, предлагают пользователям скорость статьи или элементов. Обычно для этого требуется некоторые усилия на написание кода, но у нас есть набор элементов управления в своем распоряжении.


## <a name="overview"></a>Обзор

Многие веб-узлы, от электронной коммерции на сайты сообщества, предлагают пользователям скорость статьи или элементов. Обычно для этого требуется некоторые усилия на написание кода, но у нас есть набор элементов управления в своем распоряжении.

## <a name="steps"></a>Шаги

Во-первых, необходимо (минимум) два типа образов: один для заполненного Оценка элемента и один для оценки пустой элемент. Оценка элемент – обычно типа "звезда" или смайлик. В этом сценарии поиск трех файлов, smiley.png и empty.png и done.png смайлик как часть загрузки исходного кода, в этом руководстве.

Затем создайте новый файл ASP.NET и начните с добавления `ScriptManager` элемента управления к нему:

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample1.aspx)]

Затем добавьте `Rating` элемента управления ASP.NET AJAX Control Toolkit. Следующие атрибуты должны быть заданы в этом примере:

- `CurrentRating` Начальная оценка для использования
- `MaxRating` Максимальный рейтинг
- `EmptyStarCssClass` класс CSS для использования при пустом элемент рейтинг (типа "звезда")
- `FilledStarCssClass` класс CSS для использования при заполнении элемента рейтинг (типа "звезда")
- `StarCssClass` класс CSS, используемый для видимой stat
- `WaitingStarCssClass` класс CSS для использования, пока число звезд в рейтинге отправляется на сервер

А вот разметки, который создает элемент управления rating с пятью элементами (smileys), которых нет заполняется изначально:

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample2.aspx)]

Теперь три упоминаемого классы CSS необходимо показать файлы подходящий образ, это легко сделать с помощью CSS:

[!code-css[Main](creating-a-rating-control-vb/samples/sample3.css)]

Убедитесь, что указываемые ширину и высоту три изображения, в противном случае может выглядеть немного, насколько сильно система отображения.

И, наконец результат оценки должно быть отображаемое пользователю (или, по крайней мере сохранены в базе данных). Поэтому добавьте метку для вывода текстовое сообщение и кнопка отправки обратной передачи формы оценки на сервер:

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample4.aspx)]

В коде на стороне сервера, доступ через элемент управления Rating его `ID` и затем получить доступ к его `CurrentRating` свойство, которое является число элементов выбранной оценки, в нашем примере значение в диапазоне от 0 до 5.

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample5.aspx)]

Сохраните страницу и загрузить его в адресную строку браузера. Происходит при наведении указателя мыши на элементы (изначально пуста) рейтинг эффекта JavaScript: изменения рейтинга. При щелчке набор звезд, сохраняется текущая оценка. Наконец при отправке формы, серверный код выводит выбранные оценка.


[![Создание оценки системы с минимальным использованием кода](creating-a-rating-control-vb/_static/image2.png)](creating-a-rating-control-vb/_static/image1.png)

Создание оценки системы с минимальным использованием кода ([Просмотр полноразмерного изображения](creating-a-rating-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Назад](creating-a-rating-control-cs.md)
