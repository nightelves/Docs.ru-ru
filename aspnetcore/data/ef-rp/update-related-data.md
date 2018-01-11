---
title: "Страниц Razor с основными EF - обновления связанных данных - 7, 8."
author: rick-anderson
description: "В этом учебнике будет обновить связанные данные, обновив внешних ключевых полей и свойств навигации."
keywords: "Присоединяет ASP.NET Core, Entity Framework Core, связанные данные"
ms.author: riande
manager: wpickett
ms.date: 11/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/update-related-data
ms.openlocfilehash: f07a33c19ba1be623fae14228f8fbc909d766816
ms.sourcegitcommit: 6e46abd65973dea796d364a514de9ec2e3e1c1ed
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/02/2017
---
# <a name="updating-related-data---ef-core-razor-pages-7-of-8"></a><span data-ttu-id="d8b39-104">Обновление связанных данных - страниц Razor EF Core (7, 8)</span><span class="sxs-lookup"><span data-stu-id="d8b39-104">Updating related data - EF Core Razor Pages (7 of 8)</span></span>

<span data-ttu-id="d8b39-105">По [Tom Dykstra](https://github.com/tdykstra), и [Рик Андерсон](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d8b39-105">By [Tom Dykstra](https://github.com/tdykstra), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="d8b39-106">В этом учебнике показано обновление связанных данных.</span><span class="sxs-lookup"><span data-stu-id="d8b39-106">This tutorial demonstrates updating related data.</span></span> <span data-ttu-id="d8b39-107">Если возникли проблемы, не удается устранить, загрузите [завершенное приложение для данного этапа](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part7).</span><span class="sxs-lookup"><span data-stu-id="d8b39-107">If you run into problems you can't solve, download the [completed app for this stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part7).</span></span>

<span data-ttu-id="d8b39-108">На следующих рисунках показаны некоторые завершенной страницы.</span><span class="sxs-lookup"><span data-stu-id="d8b39-108">The following illustrations shows some of the completed pages.</span></span>

<span data-ttu-id="d8b39-109">![Страница изменения курса](update-related-data/_static/course-edit.png)
![инструктора изменения страницы](update-related-data/_static/instructor-edit-courses.png)</span><span class="sxs-lookup"><span data-stu-id="d8b39-109">![Course Edit page](update-related-data/_static/course-edit.png)
![Instructor Edit page](update-related-data/_static/instructor-edit-courses.png)</span></span>

<span data-ttu-id="d8b39-110">Проверьте и тестирование Создание и редактирование страниц курса.</span><span class="sxs-lookup"><span data-stu-id="d8b39-110">Examine and test the Create and Edit course pages.</span></span> <span data-ttu-id="d8b39-111">Создание нового курса.</span><span class="sxs-lookup"><span data-stu-id="d8b39-111">Create a new course.</span></span> <span data-ttu-id="d8b39-112">Подразделение выбирается по первичному ключу (целое число), не его имя.</span><span class="sxs-lookup"><span data-stu-id="d8b39-112">The department is selected by its primary key (an integer), not its name.</span></span> <span data-ttu-id="d8b39-113">Изменение нового курса.</span><span class="sxs-lookup"><span data-stu-id="d8b39-113">Edit the new course.</span></span> <span data-ttu-id="d8b39-114">После завершения тестирования удалите нового курса.</span><span class="sxs-lookup"><span data-stu-id="d8b39-114">When you have finished testing, delete the new course.</span></span>

## <a name="create-a-base-class-to-share-common-code"></a><span data-ttu-id="d8b39-115">Создание базового класса для совместного использования кода</span><span class="sxs-lookup"><span data-stu-id="d8b39-115">Create a base class to share common code</span></span>

<span data-ttu-id="d8b39-116">Курсы/Create курсы или изменение страницы и каждого требуется список названий отделов.</span><span class="sxs-lookup"><span data-stu-id="d8b39-116">The Courses/Create and Courses/Edit pages each need a list of department names.</span></span> <span data-ttu-id="d8b39-117">Создание *Pages/Courses/DepartmentNamePageModel.cshtml.cs* базовый класс для создания и редактирования страниц:</span><span class="sxs-lookup"><span data-stu-id="d8b39-117">Create the *Pages/Courses/DepartmentNamePageModel.cshtml.cs* base class for the Create and Edit pages:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/DepartmentNamePageModel.cshtml.cs?highlight=9,11,20-21)]

<span data-ttu-id="d8b39-118">Приведенный выше код создает [SelectList](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.rendering.selectlist?view=aspnetcore-2.0) будет содержать список имен отдела.</span><span class="sxs-lookup"><span data-stu-id="d8b39-118">The preceding code creates a [SelectList](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.rendering.selectlist?view=aspnetcore-2.0) to contain the list of department names.</span></span> <span data-ttu-id="d8b39-119">Если `selectedDepartment` указан, этот отдел выбран в `SelectList`.</span><span class="sxs-lookup"><span data-stu-id="d8b39-119">If `selectedDepartment` is specified, that department is selected in the `SelectList`.</span></span>

<span data-ttu-id="d8b39-120">Создание и изменение классы модели страницы, производной от `DepartmentNamePageModel`.</span><span class="sxs-lookup"><span data-stu-id="d8b39-120">The Create and Edit page model classes will derive from `DepartmentNamePageModel`.</span></span>

## <a name="customize-the-courses-pages"></a><span data-ttu-id="d8b39-121">Настройка страниц курсов</span><span class="sxs-lookup"><span data-stu-id="d8b39-121">Customize the Courses Pages</span></span>

<span data-ttu-id="d8b39-122">При создании новой сущности курса, он должен иметь отношение к отдела.</span><span class="sxs-lookup"><span data-stu-id="d8b39-122">When a new course entity is created, it must have a relationship to an existing department.</span></span> <span data-ttu-id="d8b39-123">Чтобы добавить подразделение при создании курса, базовый класс для создания и редактирования содержит раскрывающегося списка для выбора подразделения.</span><span class="sxs-lookup"><span data-stu-id="d8b39-123">To add a department while creating a course, the base class for Create and Edit contains a drop-down list for selecting the department.</span></span> <span data-ttu-id="d8b39-124">В раскрывающемся списке устанавливается `Course.DepartmentID` свойство внешний ключ (FK).</span><span class="sxs-lookup"><span data-stu-id="d8b39-124">The drop-down list sets the `Course.DepartmentID` foreign key (FK) property.</span></span> <span data-ttu-id="d8b39-125">EF Core использует `Course.DepartmentID` внешнего ключа для загрузки `Department` свойство навигации.</span><span class="sxs-lookup"><span data-stu-id="d8b39-125">EF Core uses the `Course.DepartmentID` FK to load the `Department` navigation property.</span></span>

![Создание курса](update-related-data/_static/ddl.png)

<span data-ttu-id="d8b39-127">Обновление создать модель страницы с помощью следующего кода:</span><span class="sxs-lookup"><span data-stu-id="d8b39-127">Update the Create page model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Create.cshtml.cs?highlight=7,18,32-)]

<span data-ttu-id="d8b39-128">Предыдущий код:</span><span class="sxs-lookup"><span data-stu-id="d8b39-128">The preceding code:</span></span>

* <span data-ttu-id="d8b39-129">Происходит от `DepartmentNamePageModel`.</span><span class="sxs-lookup"><span data-stu-id="d8b39-129">Derives from `DepartmentNamePageModel`.</span></span>
* <span data-ttu-id="d8b39-130">Использует `TryUpdateModelAsync` для предотвращения [оверпостинга](xref:data/ef-rp/crud#overposting).</span><span class="sxs-lookup"><span data-stu-id="d8b39-130">Uses `TryUpdateModelAsync` to prevent [overposting](xref:data/ef-rp/crud#overposting).</span></span>
* <span data-ttu-id="d8b39-131">Заменяет `ViewData["DepartmentID"]` с `DepartmentNameSL` (от базового класса).</span><span class="sxs-lookup"><span data-stu-id="d8b39-131">Replaces `ViewData["DepartmentID"]` with `DepartmentNameSL` (from the base class).</span></span>

<span data-ttu-id="d8b39-132">`ViewData["DepartmentID"]`заменяется со строгой типизацией `DepartmentNameSL`.</span><span class="sxs-lookup"><span data-stu-id="d8b39-132">`ViewData["DepartmentID"]` is replaced with the strongly typed `DepartmentNameSL`.</span></span> <span data-ttu-id="d8b39-133">Строго типизированные моделей предпочтительные через слабо типизированным.</span><span class="sxs-lookup"><span data-stu-id="d8b39-133">Strongly typed models are preferred over weakly typed.</span></span> <span data-ttu-id="d8b39-134">Дополнительные сведения см. в разделе [слабо типизированных данных (ViewData и ViewBag)](xref:mvc/views/overview#VD_VB).</span><span class="sxs-lookup"><span data-stu-id="d8b39-134">For more information, see [Weakly typed data (ViewData and ViewBag)](xref:mvc/views/overview#VD_VB).</span></span>

### <a name="update-the-courses-create-page"></a><span data-ttu-id="d8b39-135">Страница «обновление» создайте курсов</span><span class="sxs-lookup"><span data-stu-id="d8b39-135">Update the Courses Create page</span></span>

<span data-ttu-id="d8b39-136">Обновление *Pages/Courses/Create.cshtml* следующей разметкой:</span><span class="sxs-lookup"><span data-stu-id="d8b39-136">Update *Pages/Courses/Create.cshtml* with the following markup:</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Create.cshtml?highlight=29-34)]

<span data-ttu-id="d8b39-137">Предыдущей разметки вносит следующие изменения:</span><span class="sxs-lookup"><span data-stu-id="d8b39-137">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="d8b39-138">Изменяет заголовок из **DepartmentID** для **отдел**.</span><span class="sxs-lookup"><span data-stu-id="d8b39-138">Changes the caption from **DepartmentID** to **Department**.</span></span>
* <span data-ttu-id="d8b39-139">Заменяет `"ViewBag.DepartmentID"` с `DepartmentNameSL` (от базового класса).</span><span class="sxs-lookup"><span data-stu-id="d8b39-139">Replaces `"ViewBag.DepartmentID"` with `DepartmentNameSL` (from the base class).</span></span>
* <span data-ttu-id="d8b39-140">Добавлен параметр «Выберите отдела».</span><span class="sxs-lookup"><span data-stu-id="d8b39-140">Adds the "Select Department" option.</span></span> <span data-ttu-id="d8b39-141">Это изменение отображает «Select отдела» вместо первого отдела.</span><span class="sxs-lookup"><span data-stu-id="d8b39-141">This change renders "Select Department" rather than the first department.</span></span>
* <span data-ttu-id="d8b39-142">Добавляет сообщение проверки, если отдел не выбран.</span><span class="sxs-lookup"><span data-stu-id="d8b39-142">Adds a validation message when the department is not selected.</span></span>

<span data-ttu-id="d8b39-143">На странице Razor используется [выберите тег вспомогательный](xref:mvc/views/working-with-forms#the-select-tag-helper):</span><span class="sxs-lookup"><span data-stu-id="d8b39-143">The Razor Page uses the [Select Tag Helper](xref:mvc/views/working-with-forms#the-select-tag-helper):</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Create.cshtml?range=28-35&highlight=3-6)]

<span data-ttu-id="d8b39-144">Проверьте страницы создания.</span><span class="sxs-lookup"><span data-stu-id="d8b39-144">Test the Create page.</span></span> <span data-ttu-id="d8b39-145">На странице создания имеется название отдела, а не идентификатор отдела.</span><span class="sxs-lookup"><span data-stu-id="d8b39-145">The Create page displays the department name rather than the department ID.</span></span>

### <a name="update-the-courses-edit-page"></a><span data-ttu-id="d8b39-146">Страница «обновление» изменить курсы.</span><span class="sxs-lookup"><span data-stu-id="d8b39-146">Update the Courses Edit page.</span></span>

<span data-ttu-id="d8b39-147">Обновление модели редактирования страниц с помощью следующего кода:</span><span class="sxs-lookup"><span data-stu-id="d8b39-147">Update the edit page model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Edit.cshtml.cs?highlight=8,28,35,36,40,47-)]

<span data-ttu-id="d8b39-148">Изменения, аналогичны внесенные в модель создания страницы.</span><span class="sxs-lookup"><span data-stu-id="d8b39-148">The changes are similar to those made in the Create page model.</span></span> <span data-ttu-id="d8b39-149">В приведенном выше коде `PopulateDepartmentsDropDownList` пройден в Идентификаторе отдела, что подразделение, указанное в раскрывающемся списке выберите.</span><span class="sxs-lookup"><span data-stu-id="d8b39-149">In the preceding code, `PopulateDepartmentsDropDownList` passes in the department ID, which select the department specified in the drop-down list.</span></span>

<span data-ttu-id="d8b39-150">Обновление *Pages/Courses/Edit.cshtml* следующей разметкой:</span><span class="sxs-lookup"><span data-stu-id="d8b39-150">Update *Pages/Courses/Edit.cshtml* with the following markup:</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Edit.cshtml?highlight=17-20,32-35)]

<span data-ttu-id="d8b39-151">Предыдущей разметки вносит следующие изменения:</span><span class="sxs-lookup"><span data-stu-id="d8b39-151">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="d8b39-152">Отображает идентификатор курса</span><span class="sxs-lookup"><span data-stu-id="d8b39-152">Displays the course ID.</span></span> <span data-ttu-id="d8b39-153">Обычно первичный ключ (PK) сущности не отображаются.</span><span class="sxs-lookup"><span data-stu-id="d8b39-153">Generally the Primary Key (PK) of an entity is not displayed.</span></span> <span data-ttu-id="d8b39-154">Первичные ключи не обычно имеют смысла для пользователей.</span><span class="sxs-lookup"><span data-stu-id="d8b39-154">PKs are usually meaningless to users.</span></span> <span data-ttu-id="d8b39-155">В этом случае первичного ключа является номером курса.</span><span class="sxs-lookup"><span data-stu-id="d8b39-155">In this case, the PK is the course number.</span></span>
* <span data-ttu-id="d8b39-156">Изменяет заголовок из **DepartmentID** для **отдел**.</span><span class="sxs-lookup"><span data-stu-id="d8b39-156">Changes the caption from **DepartmentID** to **Department**.</span></span>
* <span data-ttu-id="d8b39-157">Заменяет `"ViewBag.DepartmentID"` с `DepartmentNameSL` (от базового класса).</span><span class="sxs-lookup"><span data-stu-id="d8b39-157">Replaces `"ViewBag.DepartmentID"` with `DepartmentNameSL` (from the base class).</span></span>
* <span data-ttu-id="d8b39-158">Добавлен параметр «Выберите отдела».</span><span class="sxs-lookup"><span data-stu-id="d8b39-158">Adds the "Select Department" option.</span></span> <span data-ttu-id="d8b39-159">Это изменение отображает «Select отдела» вместо первого отдела.</span><span class="sxs-lookup"><span data-stu-id="d8b39-159">This change renders "Select Department" rather than the first department.</span></span>
* <span data-ttu-id="d8b39-160">Добавляет сообщение проверки, если отдел не выбран.</span><span class="sxs-lookup"><span data-stu-id="d8b39-160">Adds a validation message when the department is not selected.</span></span>

<span data-ttu-id="d8b39-161">На этой странице содержатся скрытое поле (`<input type="hidden">`) для номера курса.</span><span class="sxs-lookup"><span data-stu-id="d8b39-161">The page contains a hidden field (`<input type="hidden">`) for the course number.</span></span> <span data-ttu-id="d8b39-162">Добавление `<label>` тег вспомогательное приложение с помощью `asp-for="Course.CourseID"` не устраняют необходимость в скрытое поле.</span><span class="sxs-lookup"><span data-stu-id="d8b39-162">Adding a `<label>` tag helper with `asp-for="Course.CourseID"` doesn't eliminate the need for the hidden field.</span></span> <span data-ttu-id="d8b39-163">`<input type="hidden">`требуется для должны быть включены в данные, когда пользователь щелкает номер **Сохранить**.</span><span class="sxs-lookup"><span data-stu-id="d8b39-163">`<input type="hidden">` is required for the course number to be included in the posted data when the user clicks **Save**.</span></span>

<span data-ttu-id="d8b39-164">Проверьте обновленный код.</span><span class="sxs-lookup"><span data-stu-id="d8b39-164">Test the updated code.</span></span> <span data-ttu-id="d8b39-165">Создание, изменение и удаление курса.</span><span class="sxs-lookup"><span data-stu-id="d8b39-165">Create, edit, and delete a course.</span></span>

## <a name="add-asnotracking-to-the-details-and-delete-page-models"></a><span data-ttu-id="d8b39-166">Добавьте сведения об AsNoTracking и удаление моделей страницы</span><span class="sxs-lookup"><span data-stu-id="d8b39-166">Add AsNoTracking to the Details and Delete page models</span></span>

<span data-ttu-id="d8b39-167">[AsNoTracking](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) может повысить производительность, если трассировка не требуется.</span><span class="sxs-lookup"><span data-stu-id="d8b39-167">[AsNoTracking](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) can improve performance when tracking is not required.</span></span> <span data-ttu-id="d8b39-168">Добавить `AsNoTracking` Delete и сведения о модели страницы.</span><span class="sxs-lookup"><span data-stu-id="d8b39-168">Add `AsNoTracking` to the Delete and Details page model.</span></span> <span data-ttu-id="d8b39-169">В следующем коде показано обновленной модели страницы Delete:</span><span class="sxs-lookup"><span data-stu-id="d8b39-169">The following code shows the updated Delete page model:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Delete.cshtml.cs?name=snippet&highlight=21,23,40,41)]

<span data-ttu-id="d8b39-170">Обновление `OnGetAsync` метод в *Pages/Courses/Details.cshtml.cs* файла:</span><span class="sxs-lookup"><span data-stu-id="d8b39-170">Update the `OnGetAsync` method in the *Pages/Courses/Details.cshtml.cs* file:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Details.cshtml.cs?name=snippet)]

### <a name="modify-the-delete-and-details-pages"></a><span data-ttu-id="d8b39-171">Изменять страницы, Delete и подробные сведения</span><span class="sxs-lookup"><span data-stu-id="d8b39-171">Modify the Delete and Details pages</span></span>

<span data-ttu-id="d8b39-172">Обновление страницы Razor удалить следующую разметку:</span><span class="sxs-lookup"><span data-stu-id="d8b39-172">Update the Delete Razor page with the following markup:</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Delete.cshtml?highlight=15-20)]

<span data-ttu-id="d8b39-173">Внесения изменений на страницу сведений.</span><span class="sxs-lookup"><span data-stu-id="d8b39-173">Make the same changes to the Details page.</span></span>

### <a name="test-the-course-pages"></a><span data-ttu-id="d8b39-174">Тестирование страниц курса</span><span class="sxs-lookup"><span data-stu-id="d8b39-174">Test the Course pages</span></span>

<span data-ttu-id="d8b39-175">Тест создать, изменить сведения и удалить.</span><span class="sxs-lookup"><span data-stu-id="d8b39-175">Test create, edit, details, and delete.</span></span>

## <a name="update-the-instructor-pages"></a><span data-ttu-id="d8b39-176">Обновление страниц инструктора</span><span class="sxs-lookup"><span data-stu-id="d8b39-176">Update the instructor pages</span></span>

<span data-ttu-id="d8b39-177">Следующие разделы обновить инструктора страницы.</span><span class="sxs-lookup"><span data-stu-id="d8b39-177">The following sections update the instructor pages.</span></span>

### <a name="add-office-location"></a><span data-ttu-id="d8b39-178">Добавить расположение office</span><span class="sxs-lookup"><span data-stu-id="d8b39-178">Add office location</span></span>

<span data-ttu-id="d8b39-179">При изменении записи инструктора, может потребоваться обновить назначение инструктора office.</span><span class="sxs-lookup"><span data-stu-id="d8b39-179">When editing an instructor record, you may want to update the instructor's office assignment.</span></span> <span data-ttu-id="d8b39-180">`Instructor` Сущность имеет отношение "один к нулю или одному" с `OfficeAssignment` сущности.</span><span class="sxs-lookup"><span data-stu-id="d8b39-180">The `Instructor` entity has a one-to-zero-or-one relationship with the `OfficeAssignment` entity.</span></span> <span data-ttu-id="d8b39-181">Код инструктора должен обрабатывать:</span><span class="sxs-lookup"><span data-stu-id="d8b39-181">The instructor code must handle:</span></span>

* <span data-ttu-id="d8b39-182">Если пользователь удаляет назначение office, удалите `OfficeAssignment` сущности.</span><span class="sxs-lookup"><span data-stu-id="d8b39-182">If the user clears the office assignment, delete the `OfficeAssignment` entity.</span></span>
* <span data-ttu-id="d8b39-183">Если пользователь вводит назначения office, и он был пуст, создайте новый `OfficeAssignment` сущности.</span><span class="sxs-lookup"><span data-stu-id="d8b39-183">If the user enters an office assignment and it was empty, create a new `OfficeAssignment` entity.</span></span>
* <span data-ttu-id="d8b39-184">Если пользователь изменяет office назначения, обновить `OfficeAssignment` сущности.</span><span class="sxs-lookup"><span data-stu-id="d8b39-184">If the user changes the office assignment, update the `OfficeAssignment` entity.</span></span>

<span data-ttu-id="d8b39-185">Обновление модели страницы инструкторов изменить следующий код:</span><span class="sxs-lookup"><span data-stu-id="d8b39-185">Update the instructors Edit page model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Edit1.cshtml.cs?name=snippet&highlight=20-23,32,39-)]

<span data-ttu-id="d8b39-186">Предыдущий код:</span><span class="sxs-lookup"><span data-stu-id="d8b39-186">The preceding code:</span></span>

- <span data-ttu-id="d8b39-187">Возвращает текущий `Instructor` сущностей из базы данных, используя упреждающую для `OfficeAssignment` свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="d8b39-187">Gets the current `Instructor` entity from the database using eager loading for the `OfficeAssignment` navigation property.</span></span>
- <span data-ttu-id="d8b39-188">Обновляет извлеченного `Instructor` сущности значениями из связывателя модели.</span><span class="sxs-lookup"><span data-stu-id="d8b39-188">Updates the retrieved `Instructor` entity with values from the model binder.</span></span> <span data-ttu-id="d8b39-189">`TryUpdateModel`предотвращает [оверпостинга](xref:data/ef-rp/crud#overposting).</span><span class="sxs-lookup"><span data-stu-id="d8b39-189">`TryUpdateModel` prevents [overposting](xref:data/ef-rp/crud#overposting).</span></span>
- <span data-ttu-id="d8b39-190">Если отсутствует в расположении офиса, задает `Instructor.OfficeAssignment` значение null.</span><span class="sxs-lookup"><span data-stu-id="d8b39-190">If the office location is blank, sets `Instructor.OfficeAssignment` to null.</span></span> <span data-ttu-id="d8b39-191">Когда `Instructor.OfficeAssignment` равен null, связанной строки в `OfficeAssignment` удалить таблицу.</span><span class="sxs-lookup"><span data-stu-id="d8b39-191">When `Instructor.OfficeAssignment` is null, the related row in the `OfficeAssignment` table is deleted.</span></span>

### <a name="update-the-instructor-edit-page"></a><span data-ttu-id="d8b39-192">Обновление страницы правки инструктора</span><span class="sxs-lookup"><span data-stu-id="d8b39-192">Update the instructor Edit page</span></span>

<span data-ttu-id="d8b39-193">Обновление *Pages/Instructors/Edit.cshtml* с расположением office:</span><span class="sxs-lookup"><span data-stu-id="d8b39-193">Update *Pages/Instructors/Edit.cshtml* with the office location:</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Instructors/Edit1.cshtml?highlight=29-33)]

<span data-ttu-id="d8b39-194">Убедитесь, что можно изменить расположение инструкторов office.</span><span class="sxs-lookup"><span data-stu-id="d8b39-194">Verify you can change an instructors office location.</span></span>

## <a name="add-course-assignments-to-the-instructor-edit-page"></a><span data-ttu-id="d8b39-195">Добавление назначения курса инструктора страницы правки</span><span class="sxs-lookup"><span data-stu-id="d8b39-195">Add Course assignments to the instructor Edit page</span></span>

<span data-ttu-id="d8b39-196">Преподаватели могут обучить любое количество курсов.</span><span class="sxs-lookup"><span data-stu-id="d8b39-196">Instructors may teach any number of courses.</span></span> <span data-ttu-id="d8b39-197">В этом разделе добавьте возможность изменения курса назначения.</span><span class="sxs-lookup"><span data-stu-id="d8b39-197">In this section, you add the ability to change course assignments.</span></span> <span data-ttu-id="d8b39-198">Ниже приведен инструктора обновленные страницы правки:</span><span class="sxs-lookup"><span data-stu-id="d8b39-198">The following image shows the updated instructor Edit page:</span></span>

![Страница изменения инструктора курсы](update-related-data/_static/instructor-edit-courses.png)

<span data-ttu-id="d8b39-200">`Course`и `Instructor` имеет отношение многие ко многим.</span><span class="sxs-lookup"><span data-stu-id="d8b39-200">`Course` and `Instructor` has a many-to-many relationship.</span></span> <span data-ttu-id="d8b39-201">Чтобы добавить и удалить связи, можно добавлять и удалять сущности из `CourseAssignments` join набора сущностей.</span><span class="sxs-lookup"><span data-stu-id="d8b39-201">To add and remove relationships, you add and remove entities from the `CourseAssignments` join entity set.</span></span>

<span data-ttu-id="d8b39-202">Флажки производить изменения курсов, присвоенного инструктор.</span><span class="sxs-lookup"><span data-stu-id="d8b39-202">Check boxes enable changes to courses an instructor is assigned to.</span></span> <span data-ttu-id="d8b39-203">Флажок отображается для каждого курса в базе данных.</span><span class="sxs-lookup"><span data-stu-id="d8b39-203">A check box is displayed for every course in the database.</span></span> <span data-ttu-id="d8b39-204">Курсы, которые назначены инструктора проверяются.</span><span class="sxs-lookup"><span data-stu-id="d8b39-204">Courses that the instructor is assigned to are checked.</span></span> <span data-ttu-id="d8b39-205">Пользователь может установите или снимите флажки, чтобы изменить назначения курса.</span><span class="sxs-lookup"><span data-stu-id="d8b39-205">The user can select or clear check boxes to change course assignments.</span></span> <span data-ttu-id="d8b39-206">Если количество курсов были гораздо выше:</span><span class="sxs-lookup"><span data-stu-id="d8b39-206">If the number of courses were much greater:</span></span>

* <span data-ttu-id="d8b39-207">Возможно, используется другой пользовательский интерфейс для отображения курсов.</span><span class="sxs-lookup"><span data-stu-id="d8b39-207">You'd probably use a different user interface to display the courses.</span></span>
* <span data-ttu-id="d8b39-208">Метод управления сущности объединения можно создавать и удалять связи не будут изменяться.</span><span class="sxs-lookup"><span data-stu-id="d8b39-208">The method of manipulating a join entity to create or delete relationships would not change.</span></span>

### <a name="add-classes-to-support-create-and-edit-instructor-pages"></a><span data-ttu-id="d8b39-209">Добавлять классы для поддержки создания и редактирования страниц инструктора</span><span class="sxs-lookup"><span data-stu-id="d8b39-209">Add classes to support Create and Edit instructor pages</span></span>

<span data-ttu-id="d8b39-210">Создание *SchoolViewModels/AssignedCourseData.cs* следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="d8b39-210">Create *SchoolViewModels/AssignedCourseData.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/AssignedCourseData.cs)]

<span data-ttu-id="d8b39-211">`AssignedCourseData` Класс содержит данные, создавать флажки для назначенного курсов по инструктор.</span><span class="sxs-lookup"><span data-stu-id="d8b39-211">The `AssignedCourseData` class contains data to create the check boxes for assigned courses by an instructor.</span></span>

<span data-ttu-id="d8b39-212">Создание *Pages/Instructors/InstructorCoursesPageModel.cshtml.cs* базового класса:</span><span class="sxs-lookup"><span data-stu-id="d8b39-212">Create the *Pages/Instructors/InstructorCoursesPageModel.cshtml.cs* base class:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/InstructorCoursesPageModel.cshtml.cs)]

<span data-ttu-id="d8b39-213">`InstructorCoursesPageModel` Является базовым классом, будет использоваться для изменения и создавать модели страницы.</span><span class="sxs-lookup"><span data-stu-id="d8b39-213">The `InstructorCoursesPageModel` is the base class you will use for the Edit and Create page models.</span></span> <span data-ttu-id="d8b39-214">`PopulateAssignedCourseData`Считывает все `Course` сущностей для заполнения `AssignedCourseDataList`.</span><span class="sxs-lookup"><span data-stu-id="d8b39-214">`PopulateAssignedCourseData` reads all `Course` entities to populate `AssignedCourseDataList`.</span></span> <span data-ttu-id="d8b39-215">Для каждого курса код задает `CourseID`, заголовок и ли инструктора назначается курса.</span><span class="sxs-lookup"><span data-stu-id="d8b39-215">For each course, the code sets the `CourseID`, title, and whether or not the instructor is assigned to the course.</span></span> <span data-ttu-id="d8b39-216">Объект [HashSet](https://docs.microsoft.com/dotnet/api/system.collections.generic.hashset-1) используется для создания эффективного поиска.</span><span class="sxs-lookup"><span data-stu-id="d8b39-216">A [HashSet](https://docs.microsoft.com/dotnet/api/system.collections.generic.hashset-1) is used to create efficient lookups.</span></span>

### <a name="instructors-edit-page-model"></a><span data-ttu-id="d8b39-217">Модель страницы редактирование инструкторов</span><span class="sxs-lookup"><span data-stu-id="d8b39-217">Instructors Edit page model</span></span>

<span data-ttu-id="d8b39-218">Обновление модели страницы инструктора изменить следующий код:</span><span class="sxs-lookup"><span data-stu-id="d8b39-218">Update the instructor Edit page model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Edit.cshtml.cs?name=snippet&highlight=1,20-24,30,34,41-)]

<span data-ttu-id="d8b39-219">Предыдущий код обрабатывает изменения в назначения office.</span><span class="sxs-lookup"><span data-stu-id="d8b39-219">The preceding code handles office assignment changes.</span></span>

<span data-ttu-id="d8b39-220">Обновление представления Razor инструктора:</span><span class="sxs-lookup"><span data-stu-id="d8b39-220">Update the instructor Razor View:</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Instructors/Edit.cshtml?highlight=34-59)]

<a id="notepad"></a>
> [!NOTE]
> <span data-ttu-id="d8b39-221">При вставке кода в Visual Studio способом, нарушающим код изменяются разрывы строки.</span><span class="sxs-lookup"><span data-stu-id="d8b39-221">When you paste the code in Visual Studio, line breaks are changed in a way that breaks the code.</span></span> <span data-ttu-id="d8b39-222">Нажмите один раз сочетание клавиш Ctrl + Z для отмены автоматического форматирования.</span><span class="sxs-lookup"><span data-stu-id="d8b39-222">Press Ctrl+Z one time to undo the automatic formatting.</span></span> <span data-ttu-id="d8b39-223">CTRL + Z устраняет разрывы строки, чтобы они выглядят как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="d8b39-223">Ctrl+Z fixes the line breaks so that they look like what you see here.</span></span> <span data-ttu-id="d8b39-224">Отступы не должен быть идеальным, но `@</tr><tr>`, `@:<td>`, `@:</td>`, и `@:</tr>` строки должны быть в одной строке, как показано.</span><span class="sxs-lookup"><span data-stu-id="d8b39-224">The indentation doesn't have to be perfect, but the `@</tr><tr>`, `@:<td>`, `@:</td>`, and `@:</tr>` lines must each be on a single line as shown.</span></span> <span data-ttu-id="d8b39-225">С блоком выбран новый код нажмите клавишу Tab трижды Чтобы выровнять новый код с существующим кодом.</span><span class="sxs-lookup"><span data-stu-id="d8b39-225">With the block of new code selected, press Tab three times to line up the new code with the existing code.</span></span> <span data-ttu-id="d8b39-226">Проголосовать или просмотр состояния этой ошибки [с этой ссылкой](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).</span><span class="sxs-lookup"><span data-stu-id="d8b39-226">Vote on or review the status of this bug [with this link](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).</span></span>

<span data-ttu-id="d8b39-227">Приведенный выше код создает таблицу HTML, который содержит три столбца.</span><span class="sxs-lookup"><span data-stu-id="d8b39-227">The preceding code creates an HTML table that has three columns.</span></span> <span data-ttu-id="d8b39-228">Каждый столбец имеет флажок и заголовок, содержащий номер и название.</span><span class="sxs-lookup"><span data-stu-id="d8b39-228">Each column has a check box and a caption containing the course number and title.</span></span> <span data-ttu-id="d8b39-229">Все флажки имеют одно и то же имя («selectedCourses»).</span><span class="sxs-lookup"><span data-stu-id="d8b39-229">The check boxes all have the same name ("selectedCourses").</span></span> <span data-ttu-id="d8b39-230">С тем же именем информирует связывателя модели, обрабатывать их как группу.</span><span class="sxs-lookup"><span data-stu-id="d8b39-230">Using the same name informs the model binder to treat them as a group.</span></span> <span data-ttu-id="d8b39-231">Присвоено недопустимое значение атрибута каждого флажка `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="d8b39-231">The value attribute of each check box is set to `CourseID`.</span></span> <span data-ttu-id="d8b39-232">При отправке страницы связывателя модели передает массив, состоящий из `CourseID` только флажки, выбранные значения.</span><span class="sxs-lookup"><span data-stu-id="d8b39-232">When the page is posted, the model binder passes an array that consists of the `CourseID` values for only the check boxes that are selected.</span></span>

<span data-ttu-id="d8b39-233">При первоначальном отображении флажки курсов, назначенные инструктора проверки атрибутов.</span><span class="sxs-lookup"><span data-stu-id="d8b39-233">When the check boxes are initially rendered, courses assigned to the instructor have checked attributes.</span></span>

<span data-ttu-id="d8b39-234">Запустите приложение и проверить обновленные инструкторов страницы правки.</span><span class="sxs-lookup"><span data-stu-id="d8b39-234">Run the app and test the updated instructors Edit page.</span></span> <span data-ttu-id="d8b39-235">Измените некоторые назначения курса.</span><span class="sxs-lookup"><span data-stu-id="d8b39-235">Change some course assignments.</span></span> <span data-ttu-id="d8b39-236">Изменения отражаются на странице индекса.</span><span class="sxs-lookup"><span data-stu-id="d8b39-236">The changes are reflected on the Index page.</span></span>

<span data-ttu-id="d8b39-237">Примечание: Подходом, который используется здесь для изменения данных курса инструктора удобно использовать, когда имеется ограниченное число курсов.</span><span class="sxs-lookup"><span data-stu-id="d8b39-237">Note: The approach taken here to edit instructor course data works well when there is a limited number of courses.</span></span> <span data-ttu-id="d8b39-238">Для коллекций, которые являются гораздо большего размера более готовый к применению и эффективный будет другой пользовательский Интерфейс и другой метод обновления.</span><span class="sxs-lookup"><span data-stu-id="d8b39-238">For collections that are much larger, a different UI and a different updating method would be more useable and efficient.</span></span>

### <a name="update-the-instructors-create-page"></a><span data-ttu-id="d8b39-239">Обновление страницы создания инструкторов</span><span class="sxs-lookup"><span data-stu-id="d8b39-239">Update the instructors Create page</span></span>

<span data-ttu-id="d8b39-240">Обновление модели инструктора создать страницу следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="d8b39-240">Update the instructor Create page model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Create.cshtml.cs)]

<span data-ttu-id="d8b39-241">Предыдущий код аналогичен *Pages/Instructors/Edit.cshtml.cs* кода.</span><span class="sxs-lookup"><span data-stu-id="d8b39-241">The preceding code is similar to the *Pages/Instructors/Edit.cshtml.cs* code.</span></span>

<span data-ttu-id="d8b39-242">Обновление страницы инструктора Razor, создайте следующую разметку:</span><span class="sxs-lookup"><span data-stu-id="d8b39-242">Update the instructor Create Razor page with the following markup:</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Instructors/Create.cshtml?highlight=32-62)]

<span data-ttu-id="d8b39-243">Тестовая страница создания инструктора.</span><span class="sxs-lookup"><span data-stu-id="d8b39-243">Test the instructor Create page.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="d8b39-244">Обновление страницы удаления</span><span class="sxs-lookup"><span data-stu-id="d8b39-244">Update the Delete page</span></span>

<span data-ttu-id="d8b39-245">Обновление модели страницы удаления со следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="d8b39-245">Update the Delete page model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Delete.cshtml.cs?highlight=5,40-)]

<span data-ttu-id="d8b39-246">Приведенный выше код выполняет следующие изменения:</span><span class="sxs-lookup"><span data-stu-id="d8b39-246">The preceding code makes the following changes:</span></span>

* <span data-ttu-id="d8b39-247">Использует упреждающую для `CourseAssignments` свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="d8b39-247">Uses eager loading for the `CourseAssignments` navigation property.</span></span> <span data-ttu-id="d8b39-248">`CourseAssignments`должен быть включен или они не удаляются при удалении инструктора.</span><span class="sxs-lookup"><span data-stu-id="d8b39-248">`CourseAssignments` must be included or they aren't deleted when the instructor is deleted.</span></span> <span data-ttu-id="d8b39-249">Чтобы избежать необходимости их прочитать, настройте каскадное удаление в базе данных.</span><span class="sxs-lookup"><span data-stu-id="d8b39-249">To avoid needing to read them, configure cascade delete in the database.</span></span>

* <span data-ttu-id="d8b39-250">Если администратор любые отделы назначается инструктора для удаления, удаляет инструктора назначения из тех отделов.</span><span class="sxs-lookup"><span data-stu-id="d8b39-250">If the instructor to be deleted is assigned as administrator of any departments, removes the instructor assignment from those departments.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="d8b39-251">[Назад](xref:data/ef-rp/read-related-data)
[Вперед](xref:data/ef-rp/concurrency)</span><span class="sxs-lookup"><span data-stu-id="d8b39-251">[Previous](xref:data/ef-rp/read-related-data)
[Next](xref:data/ef-rp/concurrency)</span></span>