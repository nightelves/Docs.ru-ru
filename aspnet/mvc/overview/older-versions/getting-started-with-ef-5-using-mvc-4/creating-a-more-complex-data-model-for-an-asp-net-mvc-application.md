---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-a-more-complex-data-model-for-an-asp-net-mvc-application
title: Создание более сложной модели данных для приложения ASP.NET MVC (4 из 10) | Документация Майкрософт
author: tdykstra
description: Пример веб-приложение университета Contoso демонстрирует создание приложения ASP.NET MVC 4, с помощью Entity Framework 5 Code First и Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: f81f3d80-3674-4d8e-a9b1-87feed1a93c9
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-a-more-complex-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: cfb01742c3921c24c71fd3fa4a14a9f71fac1ac1
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838441"
---
<a name="creating-a-more-complex-data-model-for-an-aspnet-mvc-application-4-of-10"></a>Создание более сложной модели данных для приложения ASP.NET MVC (4 из 10)
====================
по [том Дайкстра](https://github.com/tdykstra)

[Скачать завершенный проект](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Пример веб-приложение университета Contoso демонстрирует создание приложения ASP.NET MVC 4, используя Entity Framework 5 Code First и Visual Studio 2012. Сведения о серии руководств см. в [первом руководстве серии](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Серии руководств можно начать с самого начала или [Загрузите начальный проект для этой главы](building-the-ef5-mvc4-chapter-downloads.md) и начните здесь.
> 
> > [!NOTE] 
> > 
> > Если вы столкнулись с проблемами, не удается устранить, [скачать завершенного глава](building-the-ef5-mvc4-chapter-downloads.md) и попробуйте воспроизвести проблему. Обычно можно найти решение проблемы, сравнивая код, чтобы полный код. Некоторые распространенные ошибки и способы их устранения, см. в разделе [ошибки и способы их устранения.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


В предыдущих руководствах вы работали с простой модели данных, состоящей из трех сущностей. В этом руководстве вы добавите дополнительные сущности и связи, а также настроите модель данных путем указания форматирования, проверки и правила сопоставления базы данных. Вы увидите два способа настройки модели данных: путем добавления атрибутов к классам сущностей и добавив код в класс контекста базы данных.

По завершении работы классы сущностей сформируют готовую модель данных, приведенную на следующем рисунке:

![School_class_diagram](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image1.png)

## <a name="customize-the-data-model-by-using-attributes"></a>Настройка модели данных с использованием атрибутов

В этом разделе вы узнаете, как настроить модель данных с помощью атрибутов, которые указывают правила форматирования, проверки и сопоставления базы данных. Затем в нескольких следующих разделах вы создадите полную `School` модели данных, добавив атрибуты к классам уже создана и создание новых классов для остальных типов сущностей в модели.

### <a name="the-datatype-attribute"></a>Атрибут DataType

Сейчас для дат зачисления студентов учащихся все веб-страницы отображают время и дату, хотя для этого поля достаточно одной даты. Используя атрибуты заметок к данным, вы можете внести в код одно изменение, позволяющее исправить формат отображения в каждом представлении, где отображаются эти данные. Чтобы рассмотреть соответствующий пример, вы добавите атрибут в свойство `EnrollmentDate` класса `Student`.

В *Models\Student.cs*, добавьте `using` инструкции для `System.ComponentModel.DataAnnotations` пространства имен и добавьте `DataType` и `DisplayFormat` атрибуты `EnrollmentDate` свойства, как показано в следующем примере:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,13-14)]

[DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) атрибут используется для указания типа данных, с более точным определением относительно встроенного типа базы данных. В этом случае требуется отслеживать только дату, а не дату и время. [Перечисление DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) представлено множество типов данных, таких как *даты, времени, PhoneNumber, Currency, EmailAddress* и многое другое. Атрибут `DataType` также обеспечивает автоматическое предоставление функций для определенных типов в приложении. Например `mailto:` связи могут создаваться для [DataType.EmailAddress](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx), и можно предоставить селектор даты [DataType.Date](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) в браузерах, поддерживающих [HTML5](http://html5.org/). [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) создает атрибуты HTML 5 [данных —](http://ejohn.org/blog/html-5-data-attributes/) (произносится *dash данных*) атрибуты, которые HTML 5. [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) атрибуты не предназначены для проверки.

`DataType.Date` не задает формат отображаемой даты. По умолчанию поле данных отображается в соответствии с использованием на сервере форматов [CultureInfo](https://msdn.microsoft.com/library/vstudio/system.globalization.cultureinfo(v=vs.110).aspx).

С помощью атрибута `DisplayFormat` можно явно указать формат даты:


[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample2.cs)]


`ApplyFormatInEditMode` Параметр указывает, что заданное форматирование также должны быть применены при отображении значения в текстовом поле для редактирования. (Вы не хотите, чтобы для некоторых полей, например, для денежных значений не можно символ валюты в текстовом поле для редактирования.)

Можно использовать [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) атрибут отдельно, однако он обычно имеет смысл использовать [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) также атрибут. `DataType` Атрибут передает *семантику* данных как и в отличие от способа их вывода на экран и обеспечивает следующие преимущества по сравнению с `DisplayFormat`:

- Браузер можно включить функции HTML5 (например для отображения элемента управления календарем, языковому символа валюты, ссылок электронной почты и т. д.).
- По умолчанию браузер будет обрабатывать данные, используя правильный формат на основе вашего [языкового стандарта](https://msdn.microsoft.com/library/vstudio/wyzd2bce.aspx).
- [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) атрибута можно включить модель MVC может выбрать подходящий шаблон поля для отображения данных ( [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) при отдельном использовании базируется на строковом шаблоне). Дополнительные сведения см. в разделе Брэд Вилсон [ASP.NET MVC 2 Templates](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html). (Хотя написана для MVC 2, эта статья по-прежнему относится к текущей версии ASP.NET MVC.)

Если вы используете `DataType` атрибут с полем даты, вам нужно будет указать `DisplayFormat` атрибут также, чтобы гарантировать, что и поле правильно отображаются в браузерах Chrome. Дополнительные сведения см. в разделе [цепочке обсуждений StackOverflow](http://stackoverflow.com/questions/12633471/mvc4-datatype-date-editorfor-wont-display-date-value-in-chrome-fine-in-ie).

Снова запустите на страницу указателя учащихся и обратите внимание на то, что время для дат зачисления больше не отображается. Аналогичная ситуация будет наблюдаться в любом представлении, которое использует `Student` модели.

![Students_index_page_with_formatted_date](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image2.png)

### <a name="the-stringlengthattribute"></a>StringLengthAttribute

Также можно указать правила проверки данных и сообщения с помощью атрибутов. Предположим, вы хотите сделать так, чтобы пользователи не вводили больше 50 символов для имени. Чтобы добавить это ограничение, добавьте [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) атрибуты к `LastName` и `FirstMidName` свойства, как показано в следующем примере:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample3.cs?highlight=10,12)]

[StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) атрибута не помешает пользователю достаточно ввести пробел для имени. Можно использовать [RegularExpression](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) атрибутов для применения ограничений к входным данным. Например следующий код требует первый символ был прописной, а остальные символы были буквенными:

`[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]`

[MaxLength](https://msdn.microsoft.com/library/System.ComponentModel.DataAnnotations.MaxLengthAttribute.aspx) атрибут предоставляет функции, аналогичные [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) атрибута, но не предоставляет клиентской проверки.

Запустите приложение и нажмите кнопку **учащихся** вкладки. Вы получите следующую ошибку:

*Модель, поддерживающая контекст «SchoolContext» изменилось с момента создания базы данных. Рассмотрите возможность использования Code First Migrations для обновления базы данных ([https://go.microsoft.com/fwlink/?LinkId=238269](https://go.microsoft.com/fwlink/?LinkId=238269)).*

Модель базы данных был изменен способом, который требует изменения в схему базы данных, и платформа Entity Framework обнаружил, что. Вы используете миграции для обновления схемы без потери данных, которые вы добавили в базу данных с помощью пользовательского интерфейса. При изменении данных, которая была создана с `Seed` метод, который будет изменен обратно в исходное состояние из-за [AddOrUpdate](https://msdn.microsoft.com/library/hh846520(v=vs.103).aspx) метод, который вы используете в `Seed` метод. ([AddOrUpdate](https://msdn.microsoft.com/library/hh846520(v=vs.103).aspx) эквивалентен операцией «вставки-обновления» в терминологии связанных баз данных.)

Введите в консоли диспетчера пакетов (PMC) следующие команды:

[!code-console[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample4.cmd)]

`add-migration MaxLengthOnNames` Команда создает файл с именем  *&lt;timeStamp&gt;\_MaxLengthOnNames.cs*. Этот файл содержит код, который обновит базу данных в соответствии с текущей моделью данных. Метка времени, добавляемый к имени файла миграций Entity Framework используется для упорядочения миграций. После создания несколько миграций, если вы удалите базу данных или при развертывании проекта с помощью миграций, все миграции применяются в порядке, в котором они были созданы.

Запустите **создать** и введите любое имя длиннее 50 символов. Как только вы превышать 50 символов, проверка на стороне клиента, немедленно отобразится сообщение об ошибке.

![Ошибка val, на стороне клиента](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image3.png)

### <a name="the-column-attribute"></a>Атрибут столбца

Вы также можете использовать атрибуты, чтобы управлять сопоставлением классов и свойств с базой данных. Предположим, что вы использовали имя `FirstMidName` для поля имени, так как это поле также может содержать отчество. Но вам нужно, чтобы столбец базы данных назывался `FirstName`, так как к этому имени привыкли пользователи, которые будут составлять нерегламентированные запросы к базе данных. Чтобы выполнить это сопоставление, можно использовать атрибут `Column`.

Атрибут `Column` указывает, что при создании базы данных столбец таблицы `Student`, сопоставляемый со свойством `FirstMidName`, будет называться `FirstName`. Другими словами, когда ваш код ссылается на `Student.FirstMidName`, данные будут браться из столбца `FirstName` таблицы `Student` или обновляться в нем. Если не указать имена столбцов, им присваивается то же имя, что имя свойства.

Добавить с помощью инструкции для [System.ComponentModel.DataAnnotations.Schema](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.aspx) и атрибут имени столбца `FirstMidName` свойства, как показано в следующем выделенном коде:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample5.cs?highlight=4,14)]

Добавление [атрибут столбца](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute.aspx) изменяет модель резервного SchoolContext, поэтому она не будет соответствовать базе данных. Введите следующие команды в PMC, чтобы создать другую миграцию:

[!code-console[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample6.cmd)]

В **обозревателя серверов** (**обозреватель баз данных** при использовании Express для Web), дважды щелкните *учащихся* таблицы.

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image4.png)

Ниже показано имя исходного столбца, как это было до применения двух первых миграций. В дополнение к имени столбца, изменение с `FirstMidName` для `FirstName`, два столбца были изменены относительно `MAX` длиной до 50 символов.

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image5.png)

Вы также можете базы данных с помощью изменения сопоставления [Fluent API](https://msdn.microsoft.com/data/jj591617), как вы увидите далее в этом руководстве.

> [!NOTE]
> Если вы попытаетесь выполнить компиляцию до создания всех этих классов сущностей, могут возникнуть ошибки компилятора.


## <a name="create-the-instructor-entity"></a>Создание сущности Instructor

![Instructor_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image6.png)

Создание *Models\Instructor.cs*, заменив код шаблона следующим кодом:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample7.cs)]

Обратите внимание, что некоторые свойства являются одинаковыми в сущностях `Student` и `Instructor`. В [реализации наследования](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md) руководством далее в этой серии вы выполните рефакторинг с использованием наследования для устранения этой избыточности.

### <a name="the-required-and-display-attributes"></a>Обязательный и атрибуты отображения

Атрибуты `LastName` свойство указывать, что это обязательное поле, что заголовки для текстового поля должны иметь вид «Last Name» (а не имя свойства, которое будет «LastName» без пробела), и что значение не может быть длиннее 50 символов.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample8.cs)]

[Атрибут StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) задает максимальную длину в базе данных и предоставляет клиента на стороне сервера и проверки для ASP.NET MVC. В этом атрибуте также можно указать минимальную длину строки, но это минимальное значение не влияет на схему базы данных. [Обязательный атрибут](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) не требуется для типов значений, таких как DateTime, int, double и число с плавающей запятой. Типы значений нельзя назначить значение null, поэтому они являются обязательными. Можно удалить [обязательный атрибут](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) и заменить параметром минимальной длины для `StringLength` атрибут:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample9.cs?highlight=2)]

Несколько атрибутов можно расположить на одной строке, поэтому можно также написать класс instructor следующим образом:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample10.cs)]

### <a name="the-fullname-calculated-property"></a>Полное имя вычисляемого свойства

`FullName` — это вычисляемое свойство, которое возвращает значение, созданное путем объединения двух других свойств. Поэтому он имеет только `get` метод доступа и нет `FullName` столбец создается в базе данных.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

### <a name="the-courses-and-officeassignment-navigation-properties"></a>Курсы и свойства навигации OfficeAssignment

`Courses` и `OfficeAssignment` — это свойства навигации. Как было описано ранее, обычно они определяются как [виртуального](https://msdn.microsoft.com/library/9fkccyh4(v=vs.110).aspx) таким образом, они могут использовать Entity Framework, называемого [отложенная загрузка](https://msdn.microsoft.com/magazine/hh205756.aspx). Кроме того, если свойство навигации может содержать несколько сущностей, его тип должен реализовывать [ICollection&lt;T&gt; ](https://msdn.microsoft.com/library/92t2ye13.aspx) интерфейс. (Например [IList&lt;T&gt; ](https://msdn.microsoft.com/library/5y536ey6.aspx) , но не определяет [IEnumerable&lt;T&gt; ](https://msdn.microsoft.com/library/9eekhta0.aspx) поскольку `IEnumerable<T>` не реализует [добавить ](https://msdn.microsoft.com/library/63ywd54z.aspx).

Преподаватель может проводить любое число курсов, поэтому `Courses` определен как коллекция `Course` сущностей. Наши бизнес-правила состояние преподаватель может иметь не более одного кабинета, поэтому `OfficeAssignment` определяется как один `OfficeAssignment` сущности (которые могут быть `null` Если кабинет не назначен).

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

## <a name="create-the-officeassignment-entity"></a>Создание сущности OfficeAssignment

![OfficeAssignment_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image7.png)

Создание *Models\OfficeAssignment.cs* следующим кодом:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample13.cs)]

Построить проект, который сохраняет изменения и проверяет, что не было выполнено любое копирование и вставка ошибки, которые компилятор может перехватить.

### <a name="the-key-attribute"></a>Ключевой атрибут

Устанавливается отношение "один к нулю или одному" между `Instructor` и `OfficeAssignment` сущностей. Назначение кабинета существует только связи с преподавателем, он назначен, и поэтому его первичный ключ также является внешним ключом для `Instructor` сущности. Но платформа Entity Framework не распознает автоматически `InstructorID` роль первичного ключа этой сущности, так как его имя не соответствует соглашению `ID` или *classname* `ID` соглашение об именовании. Таким образом, атрибут `Key` используется для определения ее в качестве ключа:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample14.cs)]

Можно также использовать `Key` атрибута, если сущность имеет собственный первичный ключ, но требуется имя свойства, отличные от `classnameID` или `ID`. По умолчанию EF считает ключ созданным не базы данных, так как столбец предназначен для идентифицирующего отношения.

### <a name="the-foreignkey-attribute"></a>Атрибут ForeignKey

При отсутствии связи один к нулю или одному или отношение "один к одному" между двумя сущностями (таких как между `OfficeAssignment` и `Instructor`), EF не может работать какому концу отношения — это субъект, и зависит от какой из конечных элементов. Один к одному связи имеют свойства навигации ссылки в каждом классе в другой класс. [Атрибута ForeignKey](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute.aspx) могут применяться к классу зависимые для установления связи. Если опустить [атрибута ForeignKey](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute.aspx), возникает следующая ошибка при попытке создания миграции:

Не удалось определить основной конец ассоциации между типами «ContosoUniversity.Models.OfficeAssignment» и «ContosoUniversity.Models.Instructor». Основной конец этой ассоциации должны быть настроены явным с помощью заметок к данным или fluent API связи.

Далее в этом руководстве будет показано, как настроить эту связь с помощью текучего API.

### <a name="the-instructor-navigation-property"></a>Свойство навигации Instructor

`Instructor` Сущность имеет значение необязательной определенности `OfficeAssignment` свойство навигации (поскольку преподавателя может не быть назначения кабинета) и `OfficeAssignment` сущность имеет не допускающим `Instructor` свойство навигации (так как назначение кабинета не может существовать без преподавателя — `InstructorID` не допускает значение NULL). Когда `Instructor` сущность имеет связанный с ним `OfficeAssignment` сущности, каждая из них имеет ссылку на другое в своем свойстве навигации.

Можно поместить `[Required]` атрибут в свойство навигации Instructor, чтобы указать, что должен существовать связанный преподаватель, но не нужно сделать, поскольку InstructorID внешний ключ (который также является ключом для этой таблицы), не допускающие значения NULL.

## <a name="modify-the-course-entity"></a>Изменение сущности Course

![Course_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image8.png)

В *Models\Course.cs*, замените код, добавленный ранее, следующим кодом:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample15.cs)]

Сущность курса имеет свойство внешнего ключа `DepartmentID` которого указывает на связанную `Department` сущности, и `Department` свойство навигации. Платформа Entity Framework не требует добавлять свойство внешнего ключа в модель данных при наличии свойства навигации для связанной сущности. EF автоматически создает внешние ключи в базе данных, где они необходимы. Однако наличие внешнего ключа в модели данных позволяет сделать обновления проще и эффективнее. Например, при извлечении сущности course для редактирования, `Department` сущности имеет значение null, если вы ее не загружаете, поэтому при обновлении сущности course, пришлось бы сначала получить `Department` сущности. Когда свойство внешнего ключа `DepartmentID` включается в модели данных, не нужно получить `Department` сущности перед обновлением.

### <a name="the-databasegenerated-attribute"></a>Атрибут DatabaseGenerated

[Атрибут DatabaseGenerated](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute.aspx) с [None](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedoption(v=vs.110).aspx) параметр на `CourseID` свойство указывает, что значения первичного ключа предоставленный пользователем, а не созданных базой данных.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample16.cs)]

По умолчанию Entity Framework предполагает, что значения первичного ключа создаются базой данных. Именно это и требуется для большинства сценариев. Однако для `Course` сущности, вы будете использовать номер курса, определяемое пользователем, например серия 1000 для одной кафедры, Серия 2000 для другой отдел и т. д.

### <a name="foreign-key-and-navigation-properties"></a>FOREIGN Key и свойства навигации

Свойства внешнего ключа и свойства навигации в `Course` сущности отражают следующие связи:

- Курс назначается одной кафедре, поэтому по указанным выше причинам имеется внешний ключ `DepartmentID` и свойство навигации `Department`. 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample17.cs)]
- На курс может быть зачислено любое количество учащихся, поэтому свойство навигации `Enrollments` является коллекцией: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample18.cs)]
- Курс могут вести несколько преподавателей, поэтому свойство навигации `Instructors` является коллекцией: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample19.cs)]

## <a name="creating-the-department-entity"></a>Создание сущности Department

![Department_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image9.png)

Создание *Models\Department.cs* следующим кодом:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample20.cs)]

### <a name="the-column-attribute"></a>Атрибут столбца

Ранее вы использовали [атрибут столбца](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute.aspx) Чтобы изменить сопоставление имени столбца. В коде для `Department` сущности, `Column` атрибут используется для изменения сопоставления типов данных SQL, чтобы столбец будет определяться с помощью SQL Server [деньги](https://msdn.microsoft.com/library/ms179882.aspx) тип в базе данных:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample21.cs)]

Сопоставление столбцов обычно не является обязательным, так как платформа Entity Framework обычно выбирает соответствующий тип данных SQL Server, на основе типа CLR, определяемое для свойства. Тип `decimal` среды CLR сопоставляется с типом `decimal` SQL Server. Но в этом случае вы знаете, что столбец будет содержать суммы в валюте и [деньги](https://msdn.microsoft.com/library/ms179882.aspx) для этого лучше подходит тип данных.

### <a name="foreign-key-and-navigation-properties"></a>FOREIGN Key и свойства навигации

Свойства внешнего ключа и навигации отражают следующие связи:

- Кафедра может иметь или не иметь администратора, и администратор всегда является преподавателем. Таким образом `InstructorID` свойство включается как внешний ключ к `Instructor` сущность и знак вопроса добавляется после `int` введите обозначения свойство как допускающие значение NULL. Свойство навигации называется `Administrator` , но содержит `Instructor` сущности: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample22.cs)]
- Кафедра может иметь несколько курсов, поэтому `Courses` свойство навигации: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample23.cs)]

  > [!NOTE]
  > По соглашению Entity Framework разрешает каскадное удаление для внешних ключей, не допускающих значение null, и связей многие ко многим. Это может привести к циклическим правилам каскадного удаления, которые будут вызывать исключение во время выполнения кода инициализатор. Например, если вы не `Department.InstructorID` свойство допускает значение NULL, можно получить следующее сообщение об исключении при выполняется инициализатор: «ссылочную связь приведет к циклическая ссылка, не допускается.» Если вашей бизнес-правила требуют `InstructorID` свойство как не допускающие значения NULL, пришлось бы использовать следующие текучий API, чтобы отключить каскадное удаление для связи: 

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample24.cs)]


## <a name="modifying-the-student-entity"></a>Изменение сущности Student

![Student_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image10.png)

В *Models\Student.cs*, замените код, добавленный ранее, следующим кодом. Изменения выделены.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample25.cs?highlight=12,15,24-27)]

## <a name="the-enrollment-entity"></a>Сущность Enrollment

 В *Models\Enrollment.cs*, замените код, добавленный ранее, следующим кодом

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample26.cs?highlight=16)]

### <a name="foreign-key-and-navigation-properties"></a>FOREIGN Key и свойства навигации

Свойства внешнего ключа и навигации отражают следующие связи:

- Запись зачисления предназначена для одного курса, поэтому доступно свойство первичного ключа `CourseID` и свойство навигации `Course`: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample27.cs)]
- Запись зачисления предназначена для одного учащегося, поэтому доступно свойство первичного ключа `StudentID` и свойство навигации `Student`: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample28.cs)]

### <a name="many-to-many-relationships"></a>Связи многие ко многим

Устанавливается отношение "многие ко многим" между `Student` и `Course` сущности и `Enrollment` сущности функции в качестве таблицы соединения многие ко многим *с полезными данными* в базе данных. Это означает, что `Enrollment` таблица содержит дополнительные данные, кроме внешних ключей для присоединяемых таблиц (в данном случае первичный ключ и `Grade` свойство).

На следующем рисунке показано, как выглядят эти связи на схеме сущностей. (Эта схема была создана с помощью [Entity Framework Power Tools](https://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d); Создание схемы не является частью учебника, — он просто используется здесь в качестве примера.)

![Учащихся Course_many для many_relationship](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image11.png)

Каждая линия связи имеет 1 на одном конце и звездочку (\*) на другом, указывающее отношение один ко многим.

Если `Enrollment` таблицы не были включены сведения об оценках, ей потребуется только содержат два внешних ключа `CourseID` и `StudentID`. В этом случае он будет соответствовать таблица соединения многие ко многим *без полезных данных* (или *чистой соединяемой таблицей*) в базе данных, и вам не пришлось создать класс модели для него вообще. `Instructor` И `Course` сущности имеют такого рода многие ко-многим, и как вы видите, между ними отсутствует класс сущности:

![Преподаватель Course_many для many_relationship](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image12.png)

В таблице необходим в базе данных, тем не менее, как показано на следующей схеме базы данных:

![Преподаватель Course_many для many_relationship_tables](https://asp.net/media/2577802/Windows-Live-Writer_Creating-a.NET-MVC-Application-4-of-10h1_B662_Instructor-Course_many-to-many_relationship_tables_03e042cf-db89-4b4c-985a-e458351ada76.png)

Платформа Entity Framework автоматически создает `CourseInstructor` таблицы и для чтения, так и для обновления его косвенно путем чтения и обновления `Instructor.Courses` и `Course.Instructors` свойства навигации.

## <a name="entity-diagram-showing-relationships"></a>Схема сущностей, показывающая связи

Ниже показана схема, создаваемая средствами Entity Framework Power Tools для завершенной модели School.

![School_data_model_diagram](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image13.png)

Помимо линиях связи многие ко многим (\* для \*) и связей один ко многим (1, чтобы \*), здесь можно увидеть на линию связи один к нулю или одному (1 к 0.. 1) между `Instructor` и `OfficeAssignment` сущности и на линию связи нуль или один ко многим (0.. 1 to \*) между сущностями Instructor и Department.

## <a name="customize-the-data-model-by-adding-code-to-the-database-context"></a>Настроить модель данных, добавив код в контексте базы данных

Далее вы добавите новые сущности для `SchoolContext` и настройте некоторые сопоставления с помощью [fluent API](https://msdn.microsoft.com/data/jj591617) вызовов. (Этот API доступен «текучим», так как она часто используется серии вызовов методов друг с другом в одной инструкции.)

В этом руководстве текучий API используется только для сопоставления базы данных, которое невозможно выполнить с помощью атрибутов. Однако текучий API позволяет задать большинство правил форматирования, проверки и сопоставления, которые можно указать с помощью атрибутов. Некоторые атрибуты, такие как `MinimumLength`, невозможно применить с текучим API. Как упоминалось ранее, `MinimumLength` не изменяет схему, он действует только правило проверки на стороне клиента и сервера

Некоторые разработчики предпочитают использовать текучий API монопольно, чтобы оставить свои классы сущностей "чистыми". Атрибуты и текучий API можно смешивать, и существует несколько конфигураций, которые можно реализовать только с помощью текучего API. На практике рекомендуется выбрать один из этих двух подходов и использовать его максимально согласованно.

Чтобы добавить новые сущности для данных модели и сопоставления базы данных, не были выполнены с помощью атрибутов, замените код в *DAL\SchoolContext.cs* следующим кодом:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample29.cs)]

Оператор new в [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx) метод настраивает таблицы соединения многие ко многим:

- Для связи "многие ко многим" между `Instructor` и `Course` сущностей, код задает имена таблиц и столбцов для таблицы соединения. Код сначала можно настроить многие ко многим для вас без этого кода, но если вы не будете вызывать ее, вы получите имена по умолчанию такие как `InstructorInstructorID` для `InstructorID` столбца.

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample30.cs)]

Приведенный ниже пример как вы мог бы использовать текучий API вместо атрибутов для указания отношений между `Instructor` и `OfficeAssignment` сущностей:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample31.cs)]

Сведения о действия инструкций «fluent API» за кулисами, см. в разделе [Fluent API](https://blogs.msdn.com/b/aspnetue/archive/2011/05/04/entity-framework-code-first-tutorial-supplement-what-is-going-on-in-a-fluent-api-call.aspx) записи блога.

## <a name="seed-the-database-with-test-data"></a>Заполнение базы данных тестовыми данными

Замените код в *Migrations\Configuration.cs* файла следующий код, чтобы предоставить начальные данные для создания новых сущностей.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample32.cs)]

Как было показано в первом руководстве, основная часть кода просто обновляет или создает объекты сущностей и загружает демонстрационные данные в свойства, необходимые для тестирования. Обратите внимание, каким образом `Course` сущность, которая имеет отношение многие ко многим с `Instructor` сущности, обрабатывается:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample33.cs)]

При создании `Course` объекта, необходимо инициализировать `Instructors` свойство навигации как пустую коллекцию с помощью кода `Instructors = new List<Instructor>()`. Это делает возможным добавление `Instructor` сущностей, которые относятся к этому `Course` с помощью `Instructors.Add` метод. Если вы не создавали пустой список, невозможно добавить эти связи, так как `Instructors` свойство будет иметь значение null и не пришлось бы `Add` метод. Инициализация списка можно также добавить в конструктор.

## <a name="add-a-migration-and-update-the-database"></a>Добавьте миграцию и обновления базы данных

В PMC введите `add-migration` команды:

`PM> add-Migration Chap4`

Если вы попытаетесь обновить базу данных на этом этапе, появится следующая ошибка:

*Конфликт инструкции ALTER TABLE с ограничением FOREIGN KEY «FK\_dbo. Курс\_dbo. Отдел\_DepartmentID». Конфликт произошел в базе данных «ContosoUniversity», таблица «dbo. Отдел», столбце «DepartmentID».*

Изменить &lt; *timestamp&gt;\_Chap4.cs* файла и внесите следующие изменения в код (вы добавите инструкцию SQL и изменить `AddColumn` инструкции):

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample34.cs?highlight=14-18)]

(Убедитесь, что закомментируйте или удалите существующие `AddColumn` строке при добавлении нового или вы получите ошибку при вводе `update-database` команды.)

Иногда при выполнении миграций с существующими данными необходимо вставить данные-заглушки в базу данных для соблюдения ограничений внешнего ключа, и что делаем сейчас. Созданный код добавляет не допускающим `DepartmentID` внешний ключ к `Course` таблицы. Если уже имеются строки в `Course` таблицы при выполнении кода, `AddColumn` операция завершалась сбоем, так как SQL Server не знает, какое значение поставить в столбце, который не может иметь значение null. Таким образом вы изменили код, чтобы присвоить новому столбцу значение по умолчанию, и вы создали кафедру-заглушку с именем «Temp» в качестве подразделения по умолчанию. В результате, если существуют `Course` строках, если этот код запускается, они будут быть связаны с кафедрой «Temp».

При `Seed` выполнения метода, он будет вставлять строки в `Department` таблицы и он будет связать существующие `Course` строк для этих новых `Department` строк. Если вы еще не добавили все курсы в пользовательском Интерфейсе, затем больше не потребуется кафедру «Temp» или значение по умолчанию на `Course.DepartmentID` столбца. Чтобы разрешить к тому, что кто-то возможно, добавили курсы с помощью приложения, также следует обновить `Seed` код метода, чтобы убедиться, что все `Course` строк (не только те, которые вставлены в ходе предыдущих запусков из `Seed` метод) имеют Допустимые `DepartmentID` значения, прежде чем удалить значение по умолчанию значение из столбца и удалить кафедру «Temp».

После завершения редактирования &lt; *timestamp&gt;\_Chap4.cs* файл, введите `update-database` команду в PMC для выполнения миграции.

> [!NOTE]
> Это можно получить другие ошибки при переносе данных и внесения изменений схемы. Если вы получаете ошибки миграции, не удается устранить, можно либо изменить строку подключения в *Web.config* файл или удалить базу данных. Самым простым подходом является переименовать базу данных, в *Web.config* файл. Например, изменить имя базы данных на CU\_протестировать, как показано ниже:
> 
> [!code-xml[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample35.xml?highlight=1-2)]
> 
>  В новой базе данных нет данных для переноса и `update-database` команда является гораздо большей долей вероятности завершится без ошибок. Инструкции о том, как удалить базу данных, см. в разделе [как удалить базу данных из Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/).


Открыть базу данных в **обозревателя серверов** как вы делали это раньше и разверните **таблиц** узел, чтобы увидеть, что все таблицы были созданы. (Если у вас есть **обозревателя серверов** откройте из более ранних времени, щелкните **обновить** кнопки.)

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image14.png)

Вы создали класс модели для `CourseInstructor` таблицы. Как упоминалось ранее, это таблица соединения для связи "многие ко многим" между `Instructor` и `Course` сущностей.

Щелкните правой кнопкой мыши `CourseInstructor` таблицы и выберите **Показать таблицу данных** для убедитесь в наличии данных в ее результате `Instructor` сущностей, добавленных к `Course.Instructors` свойство навигации.

![Table_data_in_CourseInstructor_table](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image15.png)

## <a name="summary"></a>Сводка

Теперь у вас есть более сложная модель данных и соответствующая база данных. В следующем руководстве вы узнаете о различных способах доступа к соответствующим данным.

Ссылки на другие ресурсы Entity Framework можно найти в [схема содержимого для доступа к данным ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Назад](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [Вперед](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
