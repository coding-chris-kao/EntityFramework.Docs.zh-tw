---
title: 使用 WinForms 進行資料系結-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 80fc5062-2f1c-4dbd-ab6e-b99496784b36
ms.openlocfilehash: 4b3eee20ff238864b94ef4edfb97c1bae0713300
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181793"
---
# <a name="databinding-with-winforms"></a><span data-ttu-id="f9e9d-102">使用 WinForms 進行資料系結</span><span class="sxs-lookup"><span data-stu-id="f9e9d-102">Databinding with WinForms</span></span>
<span data-ttu-id="f9e9d-103">此逐步解說示範如何將 POCO 類型系結至「主要-詳細資料」表單中的 Window Forms （WinForms）控制項。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-103">This step-by-step walkthrough shows how to bind POCO types to Window Forms (WinForms) controls in a “master-detail" form.</span></span> <span data-ttu-id="f9e9d-104">應用程式會使用 Entity Framework，以資料庫中的資料填入物件、追蹤變更，並將資料保存至資料庫。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-104">The application uses Entity Framework to populate objects with data from the database, track changes, and persist data to the database.</span></span>

<span data-ttu-id="f9e9d-105">模型會定義兩個參與一對多關聯性的類型：類別目錄（主體\\主要）和產品（相依\\詳細資料）。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-105">The model defines two types that participate in one-to-many relationship: Category (principal\\master) and Product (dependent\\detail).</span></span> <span data-ttu-id="f9e9d-106">然後，Visual Studio 工具會用來將模型中定義的類型系結至 WinForms 控制項。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-106">Then, the Visual Studio tools are used to bind the types defined in the model to the WinForms controls.</span></span> <span data-ttu-id="f9e9d-107">WinForms 資料系結架構可讓您在相關物件之間流覽：選取主視圖中的資料列會使詳細資料檢視以對應的子資料進行更新。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-107">The WinForms data-binding framework enables navigation between related objects: selecting rows in the master view causes the detail view to update with the corresponding child data.</span></span>

<span data-ttu-id="f9e9d-108">本逐步解說中的螢幕擷取畫面和程式代碼清單取自 Visual Studio 2013，但您可以使用 Visual Studio 2012 或 Visual Studio 2010 來完成此逐步解說。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-108">The screen shots and code listings in this walkthrough are taken from Visual Studio 2013 but you can complete this walkthrough with Visual Studio 2012 or Visual Studio 2010.</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="f9e9d-109">先決條件</span><span class="sxs-lookup"><span data-stu-id="f9e9d-109">Pre-Requisites</span></span>

<span data-ttu-id="f9e9d-110">您必須安裝 Visual Studio 2013、Visual Studio 2012 或 Visual Studio 2010，才能完成此逐步解說。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-110">You need to have Visual Studio 2013, Visual Studio 2012 or Visual Studio 2010 installed to complete this walkthrough.</span></span>

<span data-ttu-id="f9e9d-111">如果您使用 Visual Studio 2010，您也必須安裝 NuGet。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-111">If you are using Visual Studio 2010, you also have to install NuGet.</span></span> <span data-ttu-id="f9e9d-112">如需詳細資訊，請參閱[安裝 NuGet](https://docs.nuget.org/docs/start-here/installing-nuget)。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-112">For more information, see [Installing NuGet](https://docs.nuget.org/docs/start-here/installing-nuget).</span></span>

## <a name="create-the-application"></a><span data-ttu-id="f9e9d-113">建立應用程式</span><span class="sxs-lookup"><span data-stu-id="f9e9d-113">Create the Application</span></span>

-   <span data-ttu-id="f9e9d-114">開啟 Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f9e9d-114">Open Visual Studio</span></span>
-   <span data-ttu-id="f9e9d-115">**檔案&gt; 新&gt; 專案 ...。**</span><span class="sxs-lookup"><span data-stu-id="f9e9d-115">**File -&gt; New -&gt; Project….**</span></span>
-   <span data-ttu-id="f9e9d-116">在左窗格中選取 [ **windows** ]，然後在右窗格中選取 [ **windows FormsApplication** ]</span><span class="sxs-lookup"><span data-stu-id="f9e9d-116">Select **Windows** in the left pane and **Windows FormsApplication** in the right pane</span></span>
-   <span data-ttu-id="f9e9d-117">輸入**WinFormswithEFSample**作為名稱</span><span class="sxs-lookup"><span data-stu-id="f9e9d-117">Enter **WinFormswithEFSample** as the name</span></span>
-   <span data-ttu-id="f9e9d-118">選取 [確定]</span><span class="sxs-lookup"><span data-stu-id="f9e9d-118">Select **OK**</span></span>

## <a name="install-the-entity-framework-nuget-package"></a><span data-ttu-id="f9e9d-119">安裝 Entity Framework NuGet 套件</span><span class="sxs-lookup"><span data-stu-id="f9e9d-119">Install the Entity Framework NuGet package</span></span>

-   <span data-ttu-id="f9e9d-120">在方案總管中，以滑鼠右鍵按一下**WinFormswithEFSample**專案</span><span class="sxs-lookup"><span data-stu-id="f9e9d-120">In Solution Explorer, right-click on the **WinFormswithEFSample** project</span></span>
-   <span data-ttu-id="f9e9d-121">選取 [**管理 NuGet 套件 ...** ]</span><span class="sxs-lookup"><span data-stu-id="f9e9d-121">Select **Manage NuGet Packages…**</span></span>
-   <span data-ttu-id="f9e9d-122">在 [管理 NuGet 套件] 對話方塊中，選取 [**線上**] 索引標籤，然後選擇 [ **EntityFramework** ] 套件</span><span class="sxs-lookup"><span data-stu-id="f9e9d-122">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package</span></span>
-   <span data-ttu-id="f9e9d-123">按一下 [**安裝**]</span><span class="sxs-lookup"><span data-stu-id="f9e9d-123">Click **Install**</span></span>  
    > [!NOTE]
    > <span data-ttu-id="f9e9d-124">除了 EntityFramework 元件之外，也會新增 System.workflow.componentmodel.activity. DataAnnotations 的參考。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-124">In addition to the EntityFramework assembly a reference to System.ComponentModel.DataAnnotations is also added.</span></span> <span data-ttu-id="f9e9d-125">如果專案具有 system.string 實體的參考，則會在安裝 EntityFramework 封裝時將它移除。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-125">If the project has a reference to System.Data.Entity, then it will be removed when the EntityFramework package is installed.</span></span> <span data-ttu-id="f9e9d-126">System.web 元件不再用於 Entity Framework 6 應用程式。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-126">The System.Data.Entity assembly is no longer used for Entity Framework 6 applications.</span></span>

## <a name="implementing-ilistsource-for-collections"></a><span data-ttu-id="f9e9d-127">為集合執行 IListSource</span><span class="sxs-lookup"><span data-stu-id="f9e9d-127">Implementing IListSource for Collections</span></span>

<span data-ttu-id="f9e9d-128">集合屬性必須執行 IListSource 介面，以在使用 Windows Forms 時，啟用具有排序的雙向資料系結。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-128">Collection properties must implement the IListSource interface to enable two-way data binding with sorting when using Windows Forms.</span></span> <span data-ttu-id="f9e9d-129">為了這麼做，我們將擴充 ObservableCollection 來新增 IListSource 功能。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-129">To do this we are going to extend ObservableCollection to add IListSource functionality.</span></span>

-   <span data-ttu-id="f9e9d-130">將**ObservableListSource**類別新增至專案：</span><span class="sxs-lookup"><span data-stu-id="f9e9d-130">Add an **ObservableListSource** class to the project:</span></span>
    -   <span data-ttu-id="f9e9d-131">以滑鼠右鍵按一下專案名稱</span><span class="sxs-lookup"><span data-stu-id="f9e9d-131">Right-click on the project name</span></span>
    -   <span data-ttu-id="f9e9d-132">選取 [**新增-&gt; 新專案**]</span><span class="sxs-lookup"><span data-stu-id="f9e9d-132">Select **Add -&gt; New Item**</span></span>
    -   <span data-ttu-id="f9e9d-133">選取 [**類別**]，然後輸入**ObservableListSource**做為類別名稱</span><span class="sxs-lookup"><span data-stu-id="f9e9d-133">Select **Class** and enter **ObservableListSource** for the class name</span></span>
-   <span data-ttu-id="f9e9d-134">將預設產生的程式碼取代為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="f9e9d-134">Replace the code generated by default with the following code:</span></span>

<span data-ttu-id="f9e9d-135">*這個類別會啟用雙向資料系結，以及排序。類別衍生自 ObservableCollection&lt;T&gt;，並加入 IListSource 的明確實作為。IListSource 的 GetList （）方法會實作為傳回 IBindingList，並與 ObservableCollection 保持同步。ToBindingList 所產生的 IBindingList 實作為支援排序。ToBindingList 擴充方法定義于 EntityFramework 元件中。*</span><span class="sxs-lookup"><span data-stu-id="f9e9d-135">*This class enables two-way data binding as well as sorting. The class derives from ObservableCollection&lt;T&gt; and adds an explicit implementation of IListSource. The GetList() method of IListSource is implemented to return an IBindingList implementation that stays in sync with the ObservableCollection. The IBindingList implementation generated by ToBindingList supports sorting. The ToBindingList extension method is defined in the EntityFramework assembly.*</span></span>

``` csharp
    using System.Collections;
    using System.Collections.Generic;
    using System.Collections.ObjectModel;
    using System.ComponentModel;
    using System.Diagnostics.CodeAnalysis;
    using System.Data.Entity;

    namespace WinFormswithEFSample
    {
        public class ObservableListSource<T> : ObservableCollection<T>, IListSource
            where T : class
        {
            private IBindingList _bindingList;

            bool IListSource.ContainsListCollection { get { return false; } }

            IList IListSource.GetList()
            {
                return _bindingList ?? (_bindingList = this.ToBindingList());
            }
        }
    }
```

## <a name="define-a-model"></a><span data-ttu-id="f9e9d-136">定義模型</span><span class="sxs-lookup"><span data-stu-id="f9e9d-136">Define a Model</span></span>

<span data-ttu-id="f9e9d-137">在此逐步解說中，您可以選擇使用 Code First 或 EF Designer 來執行模型。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-137">In this walkthrough you can chose to implement a model using Code First or the EF Designer.</span></span> <span data-ttu-id="f9e9d-138">完成下列兩節的其中一個。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-138">Complete one of the two following sections.</span></span>

### <a name="option-1-define-a-model-using-code-first"></a><span data-ttu-id="f9e9d-139">選項1：使用 Code First 定義模型</span><span class="sxs-lookup"><span data-stu-id="f9e9d-139">Option 1: Define a Model using Code First</span></span>

<span data-ttu-id="f9e9d-140">本節說明如何使用 Code First 建立模型及其相關聯的資料庫。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-140">This section shows how to create a model and its associated database using Code First.</span></span> <span data-ttu-id="f9e9d-141">如果您想要使用 Database First 從使用 EF 設計工具的資料庫還原模型，請跳到下一節（**選項2：使用 Database First 定義模型）**</span><span class="sxs-lookup"><span data-stu-id="f9e9d-141">Skip to the next section (**Option 2: Define a model using Database First)** if you would rather use Database First to reverse engineer your model from a database using the EF designer</span></span>

<span data-ttu-id="f9e9d-142">使用 Code First 開發時，您通常會從撰寫定義概念（網域）模型的 .NET Framework 類別開始著手。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-142">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span>

-   <span data-ttu-id="f9e9d-143">將新的**產品**類別加入至專案</span><span class="sxs-lookup"><span data-stu-id="f9e9d-143">Add a new **Product** class to project</span></span>
-   <span data-ttu-id="f9e9d-144">將預設產生的程式碼取代為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="f9e9d-144">Replace the code generated by default with the following code:</span></span>

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;

    namespace WinFormswithEFSample
    {
        public class Product
        {
            public int ProductId { get; set; }
            public string Name { get; set; }

            public int CategoryId { get; set; }
            public virtual Category Category { get; set; }
        }
    }
```

-   <span data-ttu-id="f9e9d-145">將**Category**類別加入至專案。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-145">Add a **Category** class to the project.</span></span>
-   <span data-ttu-id="f9e9d-146">將預設產生的程式碼取代為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="f9e9d-146">Replace the code generated by default with the following code:</span></span>

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;

    namespace WinFormswithEFSample
    {
        public class Category
        {
            private readonly ObservableListSource<Product> _products =
                    new ObservableListSource<Product>();

            public int CategoryId { get; set; }
            public string Name { get; set; }
            public virtual ObservableListSource<Product> Products { get { return _products; } }
        }
    }
```

<span data-ttu-id="f9e9d-147">除了定義實體以外，您還需要定義衍生自**DbCoNtext**的類別，並公開**DbSet&lt;TEntity&gt;** 屬性。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-147">In addition to defining entities, you need to define a class that derives from **DbContext** and exposes **DbSet&lt;TEntity&gt;** properties.</span></span> <span data-ttu-id="f9e9d-148">**DbSet**屬性可讓內容知道您想要包含在模型中的類型。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-148">The **DbSet** properties let the context know which types you want to include in the model.</span></span> <span data-ttu-id="f9e9d-149">**DbCoNtext**和**DbSet**類型定義于 EntityFramework 元件中。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-149">The **DbContext** and **DbSet** types are defined in the EntityFramework assembly.</span></span>

<span data-ttu-id="f9e9d-150">DbCoNtext 衍生類型的實例會在運行時間管理實體物件，其中包括以資料庫的資料填入物件、變更追蹤，以及將資料保存至資料庫。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-150">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

-   <span data-ttu-id="f9e9d-151">將新的**ProductCoNtext**類別加入至專案。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-151">Add a new **ProductContext** class to the project.</span></span>
-   <span data-ttu-id="f9e9d-152">將預設產生的程式碼取代為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="f9e9d-152">Replace the code generated by default with the following code:</span></span>

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Data.Entity;
    using System.Linq;
    using System.Text;

    namespace WinFormswithEFSample
    {
        public class ProductContext : DbContext
        {
            public DbSet<Category> Categories { get; set; }
            public DbSet<Product> Products { get; set; }
        }
    }
```

<span data-ttu-id="f9e9d-153">編譯專案。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-153">Compile the project.</span></span>

### <a name="option-2-define-a-model-using-database-first"></a><span data-ttu-id="f9e9d-154">選項2：使用 Database First 定義模型</span><span class="sxs-lookup"><span data-stu-id="f9e9d-154">Option 2: Define a model using Database First</span></span>

<span data-ttu-id="f9e9d-155">本節說明如何使用 Database First，從使用 EF 設計工具的資料庫，對模型進行反向工程。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-155">This section shows how to use Database First to reverse engineer your model from a database using the EF designer.</span></span> <span data-ttu-id="f9e9d-156">如果您已完成上一節（**選項1：使用 Code First 定義模型）** ，請略過本節，並直接移至 [消極式**載入**] 區段。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-156">If you completed the previous section (**Option 1: Define a model using Code First)**, then skip this section and go straight to the **Lazy Loading** section.</span></span>

#### <a name="create-an-existing-database"></a><span data-ttu-id="f9e9d-157">建立現有的資料庫</span><span class="sxs-lookup"><span data-stu-id="f9e9d-157">Create an Existing Database</span></span>

<span data-ttu-id="f9e9d-158">通常當您將目標設為現有的資料庫時，就會建立它，但在此逐步解說中，我們需要建立要存取的資料庫。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-158">Typically when you are targeting an existing database it will already be created, but for this walkthrough we need to create a database to access.</span></span>

<span data-ttu-id="f9e9d-159">隨 Visual Studio 安裝的資料庫伺服器會根據您安裝的 Visual Studio 版本而有所不同：</span><span class="sxs-lookup"><span data-stu-id="f9e9d-159">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="f9e9d-160">如果您使用 Visual Studio 2010，您將會建立 SQL Express 資料庫。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-160">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>
-   <span data-ttu-id="f9e9d-161">如果您使用 Visual Studio 2012，則您將會建立[LocalDB](https://msdn.microsoft.com/library/hh510202.aspx)資料庫。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-161">If you are using Visual Studio 2012 then you'll be creating a [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) database.</span></span>

<span data-ttu-id="f9e9d-162">讓我們繼續產生資料庫。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-162">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="f9e9d-163">**View&gt; 伺服器總管**</span><span class="sxs-lookup"><span data-stu-id="f9e9d-163">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="f9e9d-164">以滑鼠右鍵按一下 **資料連線-&gt; 新增連接 ...**</span><span class="sxs-lookup"><span data-stu-id="f9e9d-164">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="f9e9d-165">如果您還沒有從伺服器總管連接到資料庫，則必須選取 [Microsoft SQL Server] 做為資料來源</span><span class="sxs-lookup"><span data-stu-id="f9e9d-165">If you haven’t connected to a database from Server Explorer before you’ll need to select Microsoft SQL Server as the data source</span></span>

    ![變更資料來源](~/ef6/media/changedatasource.png)

-   <span data-ttu-id="f9e9d-167">連接到 LocalDB 或 SQL Express （視您安裝的版本而定），然後輸入**Products**作為資料庫名稱</span><span class="sxs-lookup"><span data-stu-id="f9e9d-167">Connect to either LocalDB or SQL Express, depending on which one you have installed, and enter **Products** as the database name</span></span>

    ![新增連接 LocalDB](~/ef6/media/addconnectionlocaldb.png)

    ![新增連接快速](~/ef6/media/addconnectionexpress.png)

-   <span data-ttu-id="f9e9d-170">選取 **[確定]** ，系統會詢問您是否要建立新的資料庫，然後選取 **[是]**</span><span class="sxs-lookup"><span data-stu-id="f9e9d-170">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>

    ![建立資料庫](~/ef6/media/createdatabase.png)

-   <span data-ttu-id="f9e9d-172">新的資料庫現在會出現在伺服器總管中，以滑鼠右鍵按一下它，然後選取 [追加**查詢**]</span><span class="sxs-lookup"><span data-stu-id="f9e9d-172">The new database will now appear in Server Explorer, right-click on it and select **New Query**</span></span>
-   <span data-ttu-id="f9e9d-173">將下列 SQL 複製到新的查詢中，然後以滑鼠右鍵按一下查詢並選取 [**執行**]。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-173">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

``` SQL
    CREATE TABLE [dbo].[Categories] (
        [CategoryId] [int] NOT NULL IDENTITY,
        [Name] [nvarchar](max),
        CONSTRAINT [PK_dbo.Categories] PRIMARY KEY ([CategoryId])
    )

    CREATE TABLE [dbo].[Products] (
        [ProductId] [int] NOT NULL IDENTITY,
        [Name] [nvarchar](max),
        [CategoryId] [int] NOT NULL,
        CONSTRAINT [PK_dbo.Products] PRIMARY KEY ([ProductId])
    )

    CREATE INDEX [IX_CategoryId] ON [dbo].[Products]([CategoryId])

    ALTER TABLE [dbo].[Products] ADD CONSTRAINT [FK_dbo.Products_dbo.Categories_CategoryId] FOREIGN KEY ([CategoryId]) REFERENCES [dbo].[Categories] ([CategoryId]) ON DELETE CASCADE
```

#### <a name="reverse-engineer-model"></a><span data-ttu-id="f9e9d-174">反向工程模型</span><span class="sxs-lookup"><span data-stu-id="f9e9d-174">Reverse Engineer Model</span></span>

<span data-ttu-id="f9e9d-175">我們將使用 Entity Framework Designer （隨附于 Visual Studio 的一部分）來建立我們的模型。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-175">We’re going to make use of Entity Framework Designer, which is included as part of Visual Studio, to create our model.</span></span>

-   <span data-ttu-id="f9e9d-176">**專案-&gt; 加入新專案 。**</span><span class="sxs-lookup"><span data-stu-id="f9e9d-176">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="f9e9d-177">從左側功能表中選取 [**資料**]，然後**ADO.NET 實體資料模型**</span><span class="sxs-lookup"><span data-stu-id="f9e9d-177">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="f9e9d-178">輸入**ProductModel**作為名稱，然後按一下 **[確定]**</span><span class="sxs-lookup"><span data-stu-id="f9e9d-178">Enter **ProductModel** as the name and click **OK**</span></span>
-   <span data-ttu-id="f9e9d-179">這會啟動**實體資料模型 Wizard**</span><span class="sxs-lookup"><span data-stu-id="f9e9d-179">This launches the **Entity Data Model Wizard**</span></span>
-   <span data-ttu-id="f9e9d-180">選取 [**從資料庫產生**]，然後按 **[下一步]**</span><span class="sxs-lookup"><span data-stu-id="f9e9d-180">Select **Generate from Database** and click **Next**</span></span>

    ![ChooseModelContents](~/ef6/media/choosemodelcontents.png)

-   <span data-ttu-id="f9e9d-182">選取您在第一個區段中建立之資料庫的連接，並輸入**ProductCoNtext**做為連接字串的名稱，然後按 **[下一步]**</span><span class="sxs-lookup"><span data-stu-id="f9e9d-182">Select the connection to the database you created in the first section, enter **ProductContext** as the name of the connection string and click **Next**</span></span>

    ![選擇您的連線](~/ef6/media/chooseyourconnection.png)

-   <span data-ttu-id="f9e9d-184">按一下 [資料表] 旁的核取方塊以匯入所有資料表，然後按一下 [完成]</span><span class="sxs-lookup"><span data-stu-id="f9e9d-184">Click the checkbox next to ‘Tables’ to import all tables and click ‘Finish’</span></span>

    ![選擇您的物件](~/ef6/media/chooseyourobjects.png)

<span data-ttu-id="f9e9d-186">反向工程程式完成之後，新的模型就會新增至您的專案，並開啟以供您在 Entity Framework Designer 中觀看。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-186">Once the reverse engineer process completes the new model is added to your project and opened up for you to view in the Entity Framework Designer.</span></span> <span data-ttu-id="f9e9d-187">App.config 檔案也已新增至您的專案，其中包含資料庫的連接詳細資料。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-187">An App.config file has also been added to your project with the connection details for the database.</span></span>

#### <a name="additional-steps-in-visual-studio-2010"></a><span data-ttu-id="f9e9d-188">Visual Studio 2010 中的其他步驟</span><span class="sxs-lookup"><span data-stu-id="f9e9d-188">Additional Steps in Visual Studio 2010</span></span>

<span data-ttu-id="f9e9d-189">如果您是在 Visual Studio 2010 中工作，則必須更新 EF 設計工具以使用 EF6 程式碼產生。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-189">If you are working in Visual Studio 2010 then you will need to update the EF designer to use EF6 code generation.</span></span>

-   <span data-ttu-id="f9e9d-190">在 EF 設計工具中，以滑鼠右鍵按一下模型的空白位置，然後選取 [**新增程式碼產生專案**...]。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-190">Right-click on an empty spot of your model in the EF Designer and select **Add Code Generation Item…**</span></span>
-   <span data-ttu-id="f9e9d-191">從左側功能表中選取 [**線上範本**]，然後搜尋**DbCoNtext**</span><span class="sxs-lookup"><span data-stu-id="f9e9d-191">Select **Online Templates** from the left menu and search for **DbContext**</span></span>
-   <span data-ttu-id="f9e9d-192">選取**適用于 C\#的 EF 6.X DbCoNtext**產生器，輸入**ProductsModel**做為名稱，然後按一下 [新增]</span><span class="sxs-lookup"><span data-stu-id="f9e9d-192">Select the **EF 6.x DbContext Generator for C\#,** enter **ProductsModel** as the name and click Add</span></span>

#### <a name="updating-code-generation-for-data-binding"></a><span data-ttu-id="f9e9d-193">更新資料系結的程式碼產生</span><span class="sxs-lookup"><span data-stu-id="f9e9d-193">Updating code generation for data binding</span></span>

<span data-ttu-id="f9e9d-194">EF 會使用 T4 範本從您的模型產生程式碼。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-194">EF generates code from your model using T4 templates.</span></span> <span data-ttu-id="f9e9d-195">隨附于 Visual Studio 或從 Visual Studio 資源庫下載的範本，主要是供一般用途使用。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-195">The templates shipped with Visual Studio or downloaded from the Visual Studio gallery are intended for general purpose use.</span></span> <span data-ttu-id="f9e9d-196">這表示從這些範本產生的實體具有簡單的 ICollection&lt;T&gt; 屬性。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-196">This means that the entities generated from these templates have simple ICollection&lt;T&gt; properties.</span></span> <span data-ttu-id="f9e9d-197">不過，在進行資料系結時，您需要有可執行 IListSource 的集合屬性。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-197">However, when doing data binding it is desirable to have collection properties that implement IListSource.</span></span> <span data-ttu-id="f9e9d-198">這就是為什麼我們會建立上面的 ObservableListSource 類別，而我們現在要修改範本來使用這個類別。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-198">This is why we created the ObservableListSource class above and we are now going to modify the templates to make use of this class.</span></span>

-   <span data-ttu-id="f9e9d-199">開啟**方案總管**並尋找**ProductModel .edmx**檔案</span><span class="sxs-lookup"><span data-stu-id="f9e9d-199">Open the **Solution Explorer** and find **ProductModel.edmx** file</span></span>
-   <span data-ttu-id="f9e9d-200">尋找**ProductModel.tt**檔案，該檔案將會在 ProductModel .edmx 檔案底下加以嵌套</span><span class="sxs-lookup"><span data-stu-id="f9e9d-200">Find the **ProductModel.tt** file which will be nested under the ProductModel.edmx file</span></span>

    ![產品型號範本](~/ef6/media/productmodeltemplate.png)

-   <span data-ttu-id="f9e9d-202">按兩下 ProductModel.tt 檔案，在 [Visual Studio 編輯器] 中開啟該檔案</span><span class="sxs-lookup"><span data-stu-id="f9e9d-202">Double-click on the ProductModel.tt file to open it in the Visual Studio editor</span></span>
-   <span data-ttu-id="f9e9d-203">尋找並以 "**ObservableListSource**" 取代 "**ICollection**" 的兩個出現專案。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-203">Find and replace the two occurrences of “**ICollection**” with “**ObservableListSource**”.</span></span> <span data-ttu-id="f9e9d-204">這些位置大約是296和484行。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-204">These are located at approximately lines 296 and 484.</span></span>
-   <span data-ttu-id="f9e9d-205">尋找並將第一個出現的 "**HashSet**" 取代為 "**ObservableListSource**"。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-205">Find and replace the first occurrence of “**HashSet**” with “**ObservableListSource**”.</span></span> <span data-ttu-id="f9e9d-206">這個事件位於大約第50行。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-206">This occurrence is located at approximately line 50.</span></span> <span data-ttu-id="f9e9d-207">**請**不要取代稍後在程式碼中找到的第二個出現的 HashSet。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-207">**Do not** replace the second occurrence of HashSet found later in the code.</span></span>
-   <span data-ttu-id="f9e9d-208">儲存 ProductModel.tt 檔案。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-208">Save the ProductModel.tt file.</span></span> <span data-ttu-id="f9e9d-209">這應該會導致重新產生實體的程式碼。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-209">This should cause the code for entities to be regenerated.</span></span> <span data-ttu-id="f9e9d-210">如果程式碼未自動重新產生，請以滑鼠右鍵按一下 [ProductModel.tt]，然後選擇 [執行自訂工具]。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-210">If the code does not regenerate automatically, then right click on ProductModel.tt and choose “Run Custom Tool”.</span></span>

<span data-ttu-id="f9e9d-211">如果您現在開啟 Category.cs 檔案（此檔案是以 ProductModel.tt 為基礎），您應該會看到 Products 集合的類型為**ObservableListSource&lt;產品&gt;** 。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-211">If you now open the Category.cs file (which is nested under ProductModel.tt) then you should see that the Products collection has the type **ObservableListSource&lt;Product&gt;**.</span></span>

<span data-ttu-id="f9e9d-212">編譯專案。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-212">Compile the project.</span></span>

## <a name="lazy-loading"></a><span data-ttu-id="f9e9d-213">消極式載入</span><span class="sxs-lookup"><span data-stu-id="f9e9d-213">Lazy Loading</span></span>

<span data-ttu-id="f9e9d-214">**Product**類別上**Category**類別和**category**屬性的**Products**屬性是導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-214">The **Products** property on the **Category** class and **Category** property on the **Product** class are navigation properties.</span></span> <span data-ttu-id="f9e9d-215">在 Entity Framework 中，導覽屬性會提供一種方式來導覽兩個實體類型之間的關聯性。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-215">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span>

<span data-ttu-id="f9e9d-216">當您第一次存取導覽屬性時，EF 可讓您選擇自動從資料庫載入相關實體。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-216">EF gives you an option of loading related entities from the database automatically the first time you access the navigation property.</span></span> <span data-ttu-id="f9e9d-217">使用這種類型的載入（稱為消極式載入）時，請注意，當您第一次存取每個導覽屬性時，如果內容尚未存在於環境中，則會針對資料庫執行個別的查詢。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-217">With this type of loading (called lazy loading), be aware that the first time you access each navigation property a separate query will be executed against the database if the contents are not already in the context.</span></span>

<span data-ttu-id="f9e9d-218">使用 POCO 實體類型時，EF 會在執行時間建立衍生 proxy 類型的實例，然後覆寫類別中的虛擬屬性來新增載入攔截，以達到消極式載入。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-218">When using POCO entity types, EF achieves lazy loading by creating instances of derived proxy types during runtime and then overriding virtual properties in your classes to add the loading hook.</span></span> <span data-ttu-id="f9e9d-219">若要取得相關物件的消極式載入，您必須將導覽屬性 getter 宣告為**public**和**virtual** （可在 Visual Basic 中覆**寫**），而且您的類別不得為**密封**（在 Visual Basic 中為**NotOverridable** ）。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-219">To get lazy loading of related objects, you must declare navigation property getters as **public** and **virtual** (**Overridable** in Visual Basic), and you class must not be **sealed** (**NotOverridable** in Visual Basic).</span></span> <span data-ttu-id="f9e9d-220">使用 Database First 導覽屬性會自動設為虛擬，以啟用消極式載入。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-220">When using Database First navigation properties are automatically made virtual to enable lazy loading.</span></span> <span data-ttu-id="f9e9d-221">在 [Code First] 區段中，我們選擇將導覽屬性設為 [虛擬]，原因如下</span><span class="sxs-lookup"><span data-stu-id="f9e9d-221">In the Code First section we chose to make the navigation properties virtual for the same reason</span></span>

## <a name="bind-object-to-controls"></a><span data-ttu-id="f9e9d-222">將物件系結至控制項</span><span class="sxs-lookup"><span data-stu-id="f9e9d-222">Bind Object to Controls</span></span>

<span data-ttu-id="f9e9d-223">將模型中定義的類別新增為此 WinForms 應用程式的資料來源。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-223">Add the classes that are defined in the model as data sources for this WinForms application.</span></span>

-   <span data-ttu-id="f9e9d-224">從主功能表中，選取 [**專案-&gt; 加入新的資料來源**...]</span><span class="sxs-lookup"><span data-stu-id="f9e9d-224">From the main menu, select **Project -&gt; Add New Data Source …**</span></span>
    <span data-ttu-id="f9e9d-225">（在 Visual Studio 2010 中，您需要選取 [**資料-&gt; 加入新的資料來源**...]）</span><span class="sxs-lookup"><span data-stu-id="f9e9d-225">(in Visual Studio 2010, you need to select **Data -&gt; Add New Data Source…**)</span></span>
-   <span data-ttu-id="f9e9d-226">在 [選擇資料來源類型] 視窗中，選取 [**物件**]，然後按 **[下一步]**</span><span class="sxs-lookup"><span data-stu-id="f9e9d-226">In the Choose a Data Source Type window, select **Object** and click **Next**</span></span>
-   <span data-ttu-id="f9e9d-227">在 [選取資料物件] 對話方塊中，展開**WinFormswithEFSample**兩次，然後選取 [**類別**]，而不需要選取 [產品] 資料來源，因為我們會透過類別目錄資料源上的產品屬性來取得它。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-227">In the Select the Data Objects dialog, unfold the **WinFormswithEFSample** two times and select **Category** There is no need to select the Product data source, because we will get to it through the Product’s property on the Category data source.</span></span>

    ![資料來源](~/ef6/media/datasource.png)

-   <span data-ttu-id="f9e9d-229">按一下 **[完成]** 。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-229">Click **Finish.**</span></span>
    <span data-ttu-id="f9e9d-230">如果 [資料來源] 視窗未顯示，請選取 [ **View-&gt; 其他 Windows&gt; 資料來源**]</span><span class="sxs-lookup"><span data-stu-id="f9e9d-230">If the Data Sources window is not showing up, select **View -&gt; Other Windows-&gt; Data Sources**</span></span>
-   <span data-ttu-id="f9e9d-231">按釘選圖示，讓 [資料來源] 視窗不會自動隱藏。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-231">Press the pin icon, so the Data Sources window does not auto hide.</span></span> <span data-ttu-id="f9e9d-232">如果視窗已經是可見的，您可能需要按 [重新整理] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-232">You may need to hit the refresh button if the window was already visible.</span></span>

    ![資料來源2](~/ef6/media/datasource2.png)

-   <span data-ttu-id="f9e9d-234">在方案總管中，按兩下**Form1.cs**檔案，以在 [設計師] 中開啟主要表單。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-234">In Solution Explorer, double-click the **Form1.cs** file to open the main form in designer.</span></span>
-   <span data-ttu-id="f9e9d-235">選取 [**類別**] 資料來源，並將它拖曳至表單上。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-235">Select the **Category** data source and drag it on the form.</span></span> <span data-ttu-id="f9e9d-236">根據預設，新的 DataGridView （**categoryDataGridView**）和導覽工具列控制項都會加入至設計工具。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-236">By default, a new DataGridView (**categoryDataGridView**) and Navigation toolbar controls are added to the designer.</span></span> <span data-ttu-id="f9e9d-237">這些控制項也會系結至所建立的 BindingSource （**categoryBindingSource**）和系結導覽器（**categoryBindingNavigator**）元件。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-237">These controls are bound to the BindingSource (**categoryBindingSource**) and Binding Navigator (**categoryBindingNavigator**) components that are created as well.</span></span>
-   <span data-ttu-id="f9e9d-238">編輯**categoryDataGridView**上的資料行。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-238">Edit the columns on the **categoryDataGridView**.</span></span> <span data-ttu-id="f9e9d-239">我們想要將 [**類別**清單] 資料行設定為唯讀。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-239">We want to set the **CategoryId** column to read-only.</span></span> <span data-ttu-id="f9e9d-240">在儲存資料之後，資料庫會產生 [**類別**] 屬性的值。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-240">The value for the **CategoryId** property is generated by the database after we save the data.</span></span>
    -   <span data-ttu-id="f9e9d-241">以滑鼠右鍵按一下 DataGridView 控制項，然後選取 [編輯資料行]。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-241">Right-click the DataGridView control and select Edit Columns…</span></span>
    -   <span data-ttu-id="f9e9d-242">選取 [類別 Id] 資料行，並將 ReadOnly 設定為 True</span><span class="sxs-lookup"><span data-stu-id="f9e9d-242">Select the CategoryId column and set ReadOnly to True</span></span>
    -   <span data-ttu-id="f9e9d-243">按 [確定]</span><span class="sxs-lookup"><span data-stu-id="f9e9d-243">Press OK</span></span>
-   <span data-ttu-id="f9e9d-244">從 [類別] [資料來源] 底下選取 [產品]，並將它拖曳至表單上。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-244">Select Products from under the Category data source and drag it on the form.</span></span> <span data-ttu-id="f9e9d-245">ProductDataGridView 和 productBindingSource 會新增至表單。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-245">The productDataGridView and productBindingSource are added to the form.</span></span>
-   <span data-ttu-id="f9e9d-246">編輯 productDataGridView 上的資料行。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-246">Edit the columns on the productDataGridView.</span></span> <span data-ttu-id="f9e9d-247">我們想要隱藏 [類別目錄] 和 [類別] 資料行，並將 [ProductId] 設定為唯讀。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-247">We want to hide the CategoryId and Category columns and set ProductId to read-only.</span></span> <span data-ttu-id="f9e9d-248">在儲存資料之後，資料庫會產生 ProductId 屬性的值。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-248">The value for the ProductId property is generated by the database after we save the data.</span></span>
    -   <span data-ttu-id="f9e9d-249">以滑鼠右鍵按一下 DataGridView 控制項，然後選取 [**編輯資料行**]。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-249">Right-click the DataGridView control and select **Edit Columns...**.</span></span>
    -   <span data-ttu-id="f9e9d-250">選取 [ **ProductId** ] 資料行，並將**ReadOnly**設定為**True**。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-250">Select the **ProductId** column and set **ReadOnly** to **True**.</span></span>
    -   <span data-ttu-id="f9e9d-251">選取 [**類別**清單] 資料行，然後按 [**移除**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-251">Select the **CategoryId** column and press the **Remove** button.</span></span> <span data-ttu-id="f9e9d-252">對 [**類別**] 資料行執行相同的動作。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-252">Do the same with the **Category** column.</span></span>
    -   <span data-ttu-id="f9e9d-253">按 [確定]。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-253">Press **OK**.</span></span>

    <span data-ttu-id="f9e9d-254">到目前為止，我們在設計工具中將 DataGridView 控制項與 BindingSource 元件相關聯。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-254">So far, we associated our DataGridView controls with BindingSource components in the designer.</span></span> <span data-ttu-id="f9e9d-255">在下一節中，我們將在程式碼後置中新增程式碼，將 categoryBindingSource 設定為 DbCoNtext 目前所追蹤的實體集合。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-255">In the next section we will add code to the code behind to set categoryBindingSource.DataSource to the collection of entities that are currently tracked by DbContext.</span></span> <span data-ttu-id="f9e9d-256">當我們從類別下拖放產品時，WinForms 會負責將 productsBindingSource 屬性設定為 categoryBindingSource，並將 productsBindingSource 屬性設為 Products。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-256">When we dragged-and-dropped Products from under the Category, the WinForms took care of setting up the productsBindingSource.DataSource property to categoryBindingSource and productsBindingSource.DataMember property to Products.</span></span> <span data-ttu-id="f9e9d-257">因為此系結，所以只有屬於目前所選分類的產品才會顯示在 productDataGridView 中。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-257">Because of this binding, only the products that belong to the currently selected Category will be displayed in the productDataGridView.</span></span>
-   <span data-ttu-id="f9e9d-258">按一下滑鼠右鍵並選取 [**已啟用**]，以啟用導覽工具列上的 [**儲存**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-258">Enable the **Save** button on the Navigation toolbar by clicking the right mouse button and selecting **Enabled**.</span></span>

    ![表單1設計工具](~/ef6/media/form1-designer.png)

-   <span data-ttu-id="f9e9d-260">按兩下按鈕，以新增 [儲存] 按鈕的事件處理常式。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-260">Add the event handler for the save button by double-clicking on the button.</span></span> <span data-ttu-id="f9e9d-261">這會新增事件處理常式，並帶您前往表單的程式碼後置。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-261">This will add the event handler and bring you to the code behind for the form.</span></span> <span data-ttu-id="f9e9d-262">下一節將新增**categoryBindingNavigatorSaveItem\_Click**事件處理常式的程式碼。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-262">The code for the **categoryBindingNavigatorSaveItem\_Click** event handler will be added in the next section.</span></span>

## <a name="add-the-code-that-handles-data-interaction"></a><span data-ttu-id="f9e9d-263">加入處理資料互動的程式碼</span><span class="sxs-lookup"><span data-stu-id="f9e9d-263">Add the Code that Handles Data Interaction</span></span>

<span data-ttu-id="f9e9d-264">我們現在會新增程式碼，以使用 ProductCoNtext 來執行資料存取。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-264">We'll now add the code to use the ProductContext to perform data access.</span></span> <span data-ttu-id="f9e9d-265">更新主表單視窗的程式碼，如下所示。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-265">Update the code for the main form window as shown below.</span></span>

<span data-ttu-id="f9e9d-266">程式碼會宣告 ProductCoNtext 的長時間執行實例。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-266">The code declares a long-running instance of ProductContext.</span></span> <span data-ttu-id="f9e9d-267">ProductCoNtext 物件可用來查詢和儲存資料至資料庫。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-267">The ProductContext object is used to query and save data to the database.</span></span> <span data-ttu-id="f9e9d-268">然後會從覆寫的 OnClosing 方法呼叫 ProductCoNtext 實例上的 Dispose （）方法。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-268">The Dispose() method on the ProductContext instance is then called from the overridden OnClosing method.</span></span> <span data-ttu-id="f9e9d-269">程式碼批註會提供程式碼用途的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-269">The code comments provide details about what the code does.</span></span>

``` csharp
    using System;
    using System.Collections.Generic;
    using System.ComponentModel;
    using System.Data;
    using System.Drawing;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;
    using System.Windows.Forms;
    using System.Data.Entity;

    namespace WinFormswithEFSample
    {
        public partial class Form1 : Form
        {
            ProductContext _context;
            public Form1()
            {
                InitializeComponent();
            }

            protected override void OnLoad(EventArgs e)
            {
                base.OnLoad(e);
                _context = new ProductContext();

                // Call the Load method to get the data for the given DbSet
                // from the database.
                // The data is materialized as entities. The entities are managed by
                // the DbContext instance.
                _context.Categories.Load();

                // Bind the categoryBindingSource.DataSource to
                // all the Unchanged, Modified and Added Category objects that
                // are currently tracked by the DbContext.
                // Note that we need to call ToBindingList() on the
                // ObservableCollection<TEntity> returned by
                // the DbSet.Local property to get the BindingList<T>
                // in order to facilitate two-way binding in WinForms.
                this.categoryBindingSource.DataSource =
                    _context.Categories.Local.ToBindingList();
            }

            private void categoryBindingNavigatorSaveItem_Click(object sender, EventArgs e)
            {
                this.Validate();

                // Currently, the Entity Framework doesn’t mark the entities
                // that are removed from a navigation property (in our example the Products)
                // as deleted in the context.
                // The following code uses LINQ to Objects against the Local collection
                // to find all products and marks any that do not have
                // a Category reference as deleted.
                // The ToList call is required because otherwise
                // the collection will be modified
                // by the Remove call while it is being enumerated.
                // In most other situations you can do LINQ to Objects directly
                // against the Local property without using ToList first.
                foreach (var product in _context.Products.Local.ToList())
                {
                    if (product.Category == null)
                    {
                        _context.Products.Remove(product);
                    }
                }

                // Save the changes to the database.
                this._context.SaveChanges();

                // Refresh the controls to show the values         
                // that were generated by the database.
                this.categoryDataGridView.Refresh();
                this.productsDataGridView.Refresh();
            }

            protected override void OnClosing(CancelEventArgs e)
            {
                base.OnClosing(e);
                this._context.Dispose();
            }
        }
    }
```

## <a name="test-the-windows-forms-application"></a><span data-ttu-id="f9e9d-270">測試 Windows Forms 應用程式</span><span class="sxs-lookup"><span data-stu-id="f9e9d-270">Test the Windows Forms Application</span></span>

-   <span data-ttu-id="f9e9d-271">編譯並執行應用程式，您可以測試功能。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-271">Compile and run the application and you can test out the functionality.</span></span>

    ![儲存前先表單1](~/ef6/media/form1beforesave.png)

-   <span data-ttu-id="f9e9d-273">儲存商店產生的金鑰之後，畫面上會顯示。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-273">After saving the store generated keys are shown on the screen.</span></span>

    ![儲存後的表單1](~/ef6/media/form1aftersave.png)

-   <span data-ttu-id="f9e9d-275">如果您使用 Code First，您也會看到為您建立**WinFormswithEFSample ProductCoNtext**資料庫。</span><span class="sxs-lookup"><span data-stu-id="f9e9d-275">If you used Code First, then you will also see that a **WinFormswithEFSample.ProductContext** database is created for you.</span></span>

    ![伺服器物件總管](~/ef6/media/serverobjexplorer.png)
