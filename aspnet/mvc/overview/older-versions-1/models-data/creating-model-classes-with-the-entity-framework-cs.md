---
uid: mvc/overview/older-versions-1/models-data/creating-model-classes-with-the-entity-framework-cs
title: "Создание классов модели с платформой Entity Framework (C#) | Документы Microsoft"
author: microsoft
description: "В этом учебнике вы узнаете, как использовать ASP.NET MVC с платформой Entity Framework корпорации Майкрософт. Вы научитесь использовать мастер для создания сущности ADO.NET Da..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 61644169-e8b1-45dd-bf96-9c2301b69879
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/creating-model-classes-with-the-entity-framework-cs
msc.type: authoredcontent
ms.openlocfilehash: 8a897f671de73d9991189e32a5d86b513051ef05
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="creating-model-classes-with-the-entity-framework-c"></a>Создание классов модели с платформой Entity Framework (C#)
====================
по [Microsoft](https://github.com/microsoft)

> В этом учебнике вы узнаете, как использовать ASP.NET MVC с платформой Entity Framework корпорации Майкрософт. Вы узнаете, как использовать мастер для создания модели EDM ADO.NET. В ходе этого учебника мы создаем веб-приложения, демонстрирующий выбора, вставки, обновления и удаления базы данных с помощью платформы Entity Framework.


Целью данного учебника является объясняется, как можно создать классы для доступа к данным с помощью платформы Entity Framework корпорации Майкрософт, при создании приложения ASP.NET MVC. В учебнике предполагается без знания платформы Entity Framework корпорации Майкрософт. В конце этого учебника будет понять, как использовать платформу Entity Framework для выбора, вставки, обновления и удаления записей базы данных.

Microsoft Entity Framework является средство объекта реляционного сопоставления (O/надежный обмен Сообщениями), которое позволяет автоматически создавать слой доступа к данным из базы данных. Платформа Entity Framework позволяет избежать трудоемкой работы построения классов доступа данных вручную.

Чтобы продемонстрировать, как можно использовать Microsoft Entity Framework с ASP.NET MVC, мы создадим простой пример приложения. Мы создадим приложение фильма базы данных, позволяет отображать и изменять записи фильма базы данных.

Этот учебник предполагается, что Visual Studio 2008 или Visual Web Developer 2008 с пакетом обновления 1. Требуется пакет обновления 1 для использования платформы Entity Framework. Пакет обновления 1 для Visual Studio 2008 или Visual Web Developer с пакетом обновления 1 можно загрузить из следующий адрес:

> [https://www.ASP.NET/downloads/](https://www.asp.net/downloads)


> [!NOTE] 
> 
> Отсутствует essential подключение между ASP.NET MVC и Entity Framework корпорации Майкрософт. Существует несколько альтернатив на платформу Entity Framework, который можно использовать с ASP.NET MVC. Например можно построить с помощью других средствах O/надежный обмен Сообщениями, таких как Microsoft LINQ to SQL, SubSonic и NHibernate классами модели MVC.


## <a name="creating-the-movie-sample-database"></a>Создание образца базы данных фильма

Приложение фильма базы данных использует таблицу базы данных с именем фильмы, содержит следующие столбцы:

| Имя столбца | Тип данных | Разрешить значения NULL? | Является первичным ключом? |
| --- | --- | --- | --- |
| Идентификатор | int | False | True |
| Заголовок | Nvarchar(100) | False | False |
| Директор | Nvarchar(100) | False | False |

В этой таблице можно добавить в проект ASP.NET MVC, выполните следующие действия:

1. Щелкните правой кнопкой мыши приложение\_папки данных в окне обозревателя решений и выберите пункт меню **добавить, новый элемент.**
2. Из **Добавление нового элемента** выберите **базы данных SQL Server**, введите имя MoviesDB.mdf базы данных и нажмите кнопку **добавить** кнопки.
3. Дважды щелкните файл MoviesDB.mdf, чтобы открыть окно обозревателя базы данных обозревателя серверов.
4. Разверните подключение к базе данных MoviesDB.mdf, щелкните правой кнопкой мыши папку «таблицы» и выберите пункт меню **добавить новую таблицу**.
5. В конструкторе таблиц добавьте столбцы Id, Title и директора.
6. Нажмите кнопку **Сохранить** (он имеет значок дискеты) кнопка Сохранить новую таблицу с именем фильмов.

После создания таблицы базы данных фильмы, следует добавить некоторые демонстрационные данные в таблицу. Щелкните правой кнопкой мыши таблицу фильмы и выбрать пункт меню **Показать таблицу данных**. Можно ввести данные фиктивное фильмов в появившейся сетке.

## <a name="creating-the-adonet-entity-data-model"></a>Создание модели EDM ADO.NET

Для использования платформы Entity Framework, необходимо создать модель EDM. Вы можете воспользоваться преимуществами Visual Studio *мастер моделей EDM* для автоматического создания модели EDM из базы данных.

Выполните следующие действия.

1. Щелкните правой кнопкой мыши папку модели в окне обозревателя решений и выбрать пункт меню **добавить, новый элемент**.
2. В **Добавление нового элемента** диалоговое окно, выберите категорию данных (см. рис. 1).
3. Выберите **ADO.NET Entity Data Model** шаблона, присвойте имя MoviesDBModel.edmx модели EDM и нажмите кнопку **добавить** кнопки. Щелкнув **добавить** кнопка запускает мастер модели данных.
4. В **Выбор содержимого модели** действии, выберите **создать из базы данных** и нажмите кнопку **Далее** кнопку (см. рис. 2).
5. В **Выбор подключения к данным** шаг, выберите подключение к базе данных MoviesDB.mdf, введите объекты имя MoviesDBEntities параметры подключения и нажмите кнопку **Далее** кнопку (см. рис. 3).
6. В **Выбор объектов базы данных** шагов, выберите в таблице базы данных фильма и нажмите кнопку **Готово** кнопку (см. рис. 4).

После выполнения этих действий, откроется конструктор модели EDM ADO.NET (конструктор сущностей).

**Рис. 1 – Создание новой модели EDM**

![clip_image002](creating-model-classes-with-the-entity-framework-cs/_static/image1.jpg)

**Рис. 2 – выберите шаг содержимое модели**

![clip_image004](creating-model-classes-with-the-entity-framework-cs/_static/image2.jpg)

**Рис. 3 – Выбор подключения базы данных**

![clip_image006](creating-model-classes-with-the-entity-framework-cs/_static/image3.jpg)

**Рис. 4 – Выбор объектов базы данных**

![clip_image008](creating-model-classes-with-the-entity-framework-cs/_static/image4.jpg)

#### <a name="modifying-the-adonet-entity-data-model"></a>Изменение модели EDM ADO.NET

После создания модели EDM, можно изменить модель, используя конструктор сущностей (см. рис. 5). Конструктор сущностей в любое время можно открыть двойным щелчком файла MoviesDBModel.edmx, содержащиеся в папке «Models» в окне обозревателя решений.

**Рисунок 5 – конструктор модели EDM ADO.NET**

![clip_image010](creating-model-classes-with-the-entity-framework-cs/_static/image5.jpg)

Например можно использовать конструктор сущностей изменять имена классов, которые создает мастер модели EDM. Мастер создал класс доступа к данным с именем фильмов. Другими словами мастер предлагает класс очень совпадает с именем таблицы базы данных. Так как этот класс будет использоваться для представления фильма конкретный экземпляр, мы должны Переименуйте класс фильмы фильмом.

Если требуется переименовать класс сущностей, можно дважды щелкнуть имя класса в конструкторе сущностей и введите новое имя (см. рис. 6). Кроме того можно изменить имя сущности в окне «Свойства» после выбора сущности в Entity Designer.

**Рис. 6 – изменение имени сущности**

![clip_image012](creating-model-classes-with-the-entity-framework-cs/_static/image6.jpg)

Не забудьте сохранить вашей модели EDM после внесения изменений, нажав кнопку «Сохранить» (значок дискеты). В фоновом Entity Designer создает набор классов C#. Эти классы можно просмотреть, открыв файл MoviesDBModel.Designer.cs в окне обозревателя решений.


Не следует изменять код в файле Designer.cs так, как изменения будут быть перезаписан в следующий раз, используйте конструктор сущностей. Если вы хотите расширить функциональные возможности для классов сущностей, определенных в файле Designer.cs, то можно создать *разделяемые классы* в отдельные файлы.


#### <a name="selecting-database-records-with-the-entity-framework"></a>При выборе записи базы данных с платформой Entity Framework

Давайте начнем построение нашего фильма базы данных приложения путем создания страницы со списком записей фильма. Контроллер Home в листинге 1 представляет действие с именем Index(). Действие Index() возвращает все записи фильма фильма таблицы базы данных, используя преимущества платформы Entity Framework.

**Перечисление Controllers\HomeController.cs. 1.**

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample1.cs)]

Обратите внимание, что контроллер в список 1 включает конструктор. Конструктор инициализирует поле уровня класса с именем \_базы данных. \_Поле db представляет сущности базы данных, созданные платформой Entity Framework корпорации Майкрософт. \_Поле db является экземпляром класса MoviesDBEntities, который был создан конструктором сущностей.


Чтобы использовать класс theMoviesDBEntities в контроллер Home, необходимо импортировать пространство имен MovieEntityApp.Models (*MVCProjectName*. Модели).


\_Db поле используется в действии Index() для получения записей из таблицы базы данных фильмов. Выражение \_db. MovieSet представляет все записи из таблицы базы данных фильмов. Метод ToList() используется для преобразования набора фильмов в универсальной коллекции объектов фильма (список&lt;фильма&gt;).

Записи фильма получены с помощью LINQ to Entities. Index() действие в список 1 использует LINQ *синтаксис метода* для извлечения набора записей базы данных. При желании можно использовать LINQ *синтаксис запроса* вместо него. Следующие две инструкции выполняют точно такой же функции:

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample2.cs)]

Используйте любой синтаксис LINQ — синтаксис метода или синтаксис запроса — найденной интуитивно. Нет разницы в производительности между двумя способами — единственное отличие заключается в стиль.

Представление в списке 2 используется для отображения записей фильма.

**Перечисление Views\Home\Index.aspx. 2.**

[!code-aspx[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample3.aspx)]

Представление в списке 2 содержит **foreach** цикл, который проходит по каждой записи фильма и отображает значения свойства Title и директора запись фильма. Обратите внимание, что изменить и удалить ссылку рядом с каждой записи. Кроме того, ссылку Добавить фильма отображается в нижней части представления (см. рис. 7).

**Рис. 7 представлении индекса**

![clip_image014](creating-model-classes-with-the-entity-framework-cs/_static/image7.jpg)

Просмотр индекса является *типизированного представления*. Содержит представление Index &lt;% @ Page %&gt; директивы *Inherits* атрибут, который приводит свойство модели для строго типизированной коллекции универсальный список объектов фильма (список&lt;фильма).

## <a name="inserting-database-records-with-the-entity-framework"></a>Вставка записи базы данных с платформой Entity Framework

Entity Framework позволяет упростить процесс для вставки новых записей в таблицу базы данных. Листинг 3 содержит два новых действий, добавленный в класс контроллера Home, можно использовать для вставки новых записей в таблице базы данных фильма.

**Листинг 3 – Controllers\HomeController.cs (Добавить методы)**

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample4.cs)]

Первое действие Add() просто возвращает представление. Представление содержит формы для добавления новой базы данных фильма записи (см. рис. 8). При отправке формы, второе действие Add() вызывается.

Обратите внимание, что второе действие Add() помечено атрибутом AcceptVerbs. Это действие может быть вызвана только при выполнении операции HTTP POST. Другими словами это действие может быть вызвана только при учете формы HTML.

Второе действие Add() создает новый экземпляр класса Entity Framework фильма с помощью метода ASP.NET MVC TryUpdateModel(). Метод TryUpdateModel() принимает полей в FormCollection, передаваемый в метод Add() и присваивает значения этих полей формы HTML в класс фильма.


При использовании платформы Entity Framework, необходимо указать «белый список» свойств при использовании методов TryUpdateModel или UpdateModel для обновления свойств класса сущностей.


После этого действие Add() выполняет проверку некоторые простой формы. Действие проверяет, что заголовок и директора свойства имеют значения. В случае ошибки проверки для ModelState добавляется сообщение об ошибке проверки.

Если нет ошибок проверки новой записи фильма добавляется в таблицу базы данных фильмов с помощью платформы Entity Framework. Новая запись добавляется в базу данных с две следующие строки кода:

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample5.cs)]

Первая строка кода добавляет новую сущность фильма набор фильмов, отслеживаемые платформой Entity Framework. Вторая строка кода сохраняет все изменения были внесены в кино, отслеживаемые в основной базе данных.

**Рис. 8 – добавить представление**

![clip_image016](creating-model-classes-with-the-entity-framework-cs/_static/image8.jpg)

#### <a name="updating-database-records-with-the-entity-framework"></a>Обновление записей базы данных с платформой Entity Framework

Можно выполнить почти тот же подход, чтобы изменить запись в базе данных с платформой Entity Framework, как подход, который мы следовали для вставки новой записи базы данных. Листинг 4 содержит два новых действий контроллера, с именем Edit(). Первое действие Edit() возвращает HTML-формы для редактирования запись фильма. Второе действие Edit() пытается обновить базу данных.

**Листинг 4 – Controllers\HomeController.cs (методы редактирования)**

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample6.cs)]

Второе действие Edit() начинается путем извлечения из базы данных, которая соответствует идентификатору фильма редактируемого запись фильма. Следующие LINQ to Entities инструкции извлекает первой записи базы данных, которая совпадает с определенным кодом:

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample7.cs)]

Затем метод TryUpdateModel() используется для присвоения значений полей формы HTML свойства сущности фильма. Обратите внимание, что список разрешенных предоставлены для указания точного свойств для обновления.

Далее некоторые простые проверка выполняется для проверки того, что название и директора свойства имеют значения. Если одно из свойств отсутствует значение, затем добавляется сообщение об ошибке проверки ModelState и ModelState.IsValid возвращает значение false.

Наконец в случае ошибки проверки не базовой таблицы базы данных фильмы затем обновляется любые изменения, путем вызова метода SaveChanges().

При изменении записи базы данных, необходимо передать идентификатор записи, в изменяемой действия контроллера, который выполняет обновление базы данных. В противном случае действие контроллера будет известно, какая запись для обновления в базе данных. Представление изменения, содержащиеся в листинге 5 включает в себя скрытое поле формы, представляющее идентификатор изменяемую запись базы данных.

**Листинг 5 – Views\Home\Edit.aspx**

[!code-aspx[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample8.aspx)]

## <a name="deleting-database-records-with-the-entity-framework"></a>Удаление записей базы данных с платформой Entity Framework

Операции окончательной базы данных, который нужно разбираться в этом учебнике выполняется удаление записи базы данных. Действия контроллера 6 со списком позволяет удалять записи определенной базы данных.

**Перечисление 6 — \Controllers\HomeController.cs (действие Delete)**

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample9.cs)]

Действие Delete() сначала извлекает фильм, сущность, которая соответствует идентификатору переданные в действие. Далее фильма удаляется из базы данных с помощью вызова метода DeleteObject(), а затем с помощью метода SaveChanges(). Наконец пользователь перенаправляется обратно в представление индекс.

## <a name="summary"></a>Сводка

Цель этого учебника было показано, как создать базы данных веб-приложения, используя преимущества ASP.NET MVC и Entity Framework корпорации Майкрософт. Вы узнали, как создать приложение, которое позволяет выбрать, вставки, обновления и удаления записей базы данных.

Во-первых мы рассмотрели, как можно использовать мастер моделей EDM для создания модели EDM из среды Visual Studio. Далее вы узнаете, как использовать LINQ to Entities для получения набора записей базы данных из таблицы базы данных. Наконец мы использовали Entity Framework для вставки, обновления и удаления записей базы данных.

>[!div class="step-by-step"]
[Вперед](creating-model-classes-with-linq-to-sql-cs.md)