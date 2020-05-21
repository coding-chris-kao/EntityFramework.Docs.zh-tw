---
title: EF Core 2.0 中的新功能 - EF Core
author: divega
ms.date: 02/20/2018
ms.assetid: 2CB5809E-0EFB-44F6-AF14-9D5BFFFBFF9D
uid: core/what-is-new/ef-core-2.0
ms.openlocfilehash: a3e066056fc67031060920f5f7763007bdc1d2d3
ms.sourcegitcommit: 59e3d5ce7dfb284457cf1c991091683b2d1afe9d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/20/2020
ms.locfileid: "83672878"
---
# <a name="new-features-in-ef-core-20"></a><span data-ttu-id="7ad52-102">EF Core 2.0 中的新功能</span><span class="sxs-lookup"><span data-stu-id="7ad52-102">New features in EF Core 2.0</span></span>

## <a name="net-standard-20"></a><span data-ttu-id="7ad52-103">.NET Standard 2.0</span><span class="sxs-lookup"><span data-stu-id="7ad52-103">.NET Standard 2.0</span></span>

<span data-ttu-id="7ad52-104">EF Core 現在的目標是 .NET Standard 2.0，表示它可以處理 .NET Core 2.0、.NET Framework 4.6.1 以及其他實作 .NET Standard 2.0 的程式庫。</span><span class="sxs-lookup"><span data-stu-id="7ad52-104">EF Core now targets .NET Standard 2.0, which means it can work with .NET Core 2.0, .NET Framework 4.6.1, and other libraries that implement .NET Standard 2.0.</span></span>
<span data-ttu-id="7ad52-105">如需所支援實作的詳細資料，請參閱[支援的 .NET 實作](../platforms/index.md)。</span><span class="sxs-lookup"><span data-stu-id="7ad52-105">See [Supported .NET Implementations](../platforms/index.md) for more details on what is supported.</span></span>

## <a name="modeling"></a><span data-ttu-id="7ad52-106">模型化</span><span class="sxs-lookup"><span data-stu-id="7ad52-106">Modeling</span></span>

### <a name="table-splitting"></a><span data-ttu-id="7ad52-107">資料表分割</span><span class="sxs-lookup"><span data-stu-id="7ad52-107">Table splitting</span></span>

<span data-ttu-id="7ad52-108">現在可以將兩個或多個實體類型對應至相同的資料表；其中，將會共用主要索引鍵資料行，而且每個資料列都會對應至兩個或多個實體。</span><span class="sxs-lookup"><span data-stu-id="7ad52-108">It is now possible to map two or more entity types to the same table where the primary key column(s) will be shared and each row will correspond to two or more entities.</span></span>

<span data-ttu-id="7ad52-109">若要使用資料表分割，必須在共用資料表的所有實體類型之間設定識別關聯性 (其中外部索引鍵屬性形成主索引鍵)：</span><span class="sxs-lookup"><span data-stu-id="7ad52-109">To use table splitting an identifying relationship (where foreign key properties form the primary key) must be configured between all of the entity types sharing the table:</span></span>

``` csharp
modelBuilder.Entity<Product>()
    .HasOne(e => e.Details).WithOne(e => e.Product)
    .HasForeignKey<ProductDetails>(e => e.Id);
modelBuilder.Entity<Product>().ToTable("Products");
modelBuilder.Entity<ProductDetails>().ToTable("Products");
```

<span data-ttu-id="7ad52-110">如需此功能的詳細資訊，請詳讀[資料表分割章節](xref:core/modeling/table-splitting)。</span><span class="sxs-lookup"><span data-stu-id="7ad52-110">Read the [section on table splitting](xref:core/modeling/table-splitting) for more information on this feature.</span></span>

### <a name="owned-types"></a><span data-ttu-id="7ad52-111">擁有的類型</span><span class="sxs-lookup"><span data-stu-id="7ad52-111">Owned types</span></span>

<span data-ttu-id="7ad52-112">擁有的實體類型可以與另一個擁有的實體類型共用相同的 .NET 類型，但因為它不能只由 .NET 類型識別，所以必須從另一個實體類型進行導覽。</span><span class="sxs-lookup"><span data-stu-id="7ad52-112">An owned entity type can share the same .NET type with another owned entity type, but since it cannot be identified just by the .NET type there must be a navigation to it from another entity type.</span></span> <span data-ttu-id="7ad52-113">包含定義導覽的實體是擁有者。</span><span class="sxs-lookup"><span data-stu-id="7ad52-113">The entity containing the defining navigation is the owner.</span></span> <span data-ttu-id="7ad52-114">查詢擁有者時，預設會包含擁有的類型。</span><span class="sxs-lookup"><span data-stu-id="7ad52-114">When querying the owner the owned types will be included by default.</span></span>

<span data-ttu-id="7ad52-115">依照慣例，將為擁有的類型建立陰影主索引鍵，而且會使用資料表分割將它對應至與擁有者相同的資料表。</span><span class="sxs-lookup"><span data-stu-id="7ad52-115">By convention a shadow primary key will be created for the owned type and it will be mapped to the same table as the owner by using table splitting.</span></span> <span data-ttu-id="7ad52-116">這允許使用擁有的類型，其類似在 EF6 中如何使用複雜類型：</span><span class="sxs-lookup"><span data-stu-id="7ad52-116">This allows to use owned types similarly to how complex types are used in EF6:</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, cb =>
    {
        cb.OwnsOne(c => c.BillingAddress);
        cb.OwnsOne(c => c.ShippingAddress);
    });

public class Order
{
    public int Id { get; set; }
    public OrderDetails OrderDetails { get; set; }
}

public class OrderDetails
{
    public StreetAddress BillingAddress { get; set; }
    public StreetAddress ShippingAddress { get; set; }
}

public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
}
```

<span data-ttu-id="7ad52-117">如需這項功能的詳細資訊，請閱讀[擁有的實體類型](xref:core/modeling/owned-entities)一節。</span><span class="sxs-lookup"><span data-stu-id="7ad52-117">Read the [section on owned entity types](xref:core/modeling/owned-entities) for more information on this feature.</span></span>

### <a name="model-level-query-filters"></a><span data-ttu-id="7ad52-118">模型層級查詢篩選</span><span class="sxs-lookup"><span data-stu-id="7ad52-118">Model-level query filters</span></span>

<span data-ttu-id="7ad52-119">EF Core 2.0 包含我們稱為「模型層級查詢篩選」的新功能。</span><span class="sxs-lookup"><span data-stu-id="7ad52-119">EF Core 2.0 includes a new feature we call Model-level query filters.</span></span> <span data-ttu-id="7ad52-120">此功能允許直接在中繼資料模型 (通常是在 OnModelCreating 中) 的「實體類型」上定義 LINQ 查詢述詞 (布林運算式通常會傳遞至 LINQ Where 查詢運算子)。</span><span class="sxs-lookup"><span data-stu-id="7ad52-120">This feature allows LINQ query predicates (a boolean expression typically passed to the LINQ Where query operator) to be defined directly on Entity Types in the metadata model (usually in OnModelCreating).</span></span> <span data-ttu-id="7ad52-121">這類篩選會自動套用至任何涉及這些「實體類型」(包含間接參考的「實體類型」) 的 LINQ 查詢 (例如，使用 Include 或直接導覽屬性參考)。</span><span class="sxs-lookup"><span data-stu-id="7ad52-121">Such filters are automatically applied to any LINQ queries involving those Entity Types, including Entity Types referenced indirectly, such as through the use of Include or direct navigation property references.</span></span> <span data-ttu-id="7ad52-122">此功能的一些常見應用如下：</span><span class="sxs-lookup"><span data-stu-id="7ad52-122">Some common applications of this feature are:</span></span>

- <span data-ttu-id="7ad52-123">虛刪除 -「實體類型」定義 IsDeleted 屬性。</span><span class="sxs-lookup"><span data-stu-id="7ad52-123">Soft delete - An Entity Types defines an IsDeleted property.</span></span>
- <span data-ttu-id="7ad52-124">多租用戶 -「實體類型」定義 TenantId 屬性。</span><span class="sxs-lookup"><span data-stu-id="7ad52-124">Multi-tenancy - An Entity Type defines a TenantId property.</span></span>

<span data-ttu-id="7ad52-125">以下簡單範例示範上述兩個案例的功能：</span><span class="sxs-lookup"><span data-stu-id="7ad52-125">Here is a simple example demonstrating the feature for the two scenarios listed above:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    public int TenantId { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Post>().HasQueryFilter(
            p => !p.IsDeleted
            && p.TenantId == this.TenantId);
    }
}
```

<span data-ttu-id="7ad52-126">我們會定義模型層級篩選，以實作 `Post` 實體類型執行個體的多租用戶和虛刪除。</span><span class="sxs-lookup"><span data-stu-id="7ad52-126">We define a model-level filter that implements multi-tenancy and soft-delete for instances of the `Post` Entity Type.</span></span> <span data-ttu-id="7ad52-127">請注意 `DbContext` 實例層級屬性的使用： `TenantId` 。</span><span class="sxs-lookup"><span data-stu-id="7ad52-127">Note the use of a `DbContext` instance-level property: `TenantId`.</span></span> <span data-ttu-id="7ad52-128">模型層級篩選會使用正確內容執行個體 (即執行查詢的內容執行個體詢) 中的值。</span><span class="sxs-lookup"><span data-stu-id="7ad52-128">Model-level filters will use the value from the correct context instance (that is, the context instance that is executing the query).</span></span>

<span data-ttu-id="7ad52-129">可能會使用 IgnoreQueryFilters() 運算子停用個別 LINQ 查詢的篩選。</span><span class="sxs-lookup"><span data-stu-id="7ad52-129">Filters may be disabled for individual LINQ queries using the IgnoreQueryFilters() operator.</span></span>

#### <a name="limitations"></a><span data-ttu-id="7ad52-130">限制</span><span class="sxs-lookup"><span data-stu-id="7ad52-130">Limitations</span></span>

- <span data-ttu-id="7ad52-131">不允許導覽參考。</span><span class="sxs-lookup"><span data-stu-id="7ad52-131">Navigation references are not allowed.</span></span> <span data-ttu-id="7ad52-132">此功能可能是根據意見所新增。</span><span class="sxs-lookup"><span data-stu-id="7ad52-132">This feature may be added based on feedback.</span></span>
- <span data-ttu-id="7ad52-133">篩選只能定義於階層的 root 實體類型上。</span><span class="sxs-lookup"><span data-stu-id="7ad52-133">Filters can only be defined on the root Entity Type of a hierarchy.</span></span>

### <a name="database-scalar-function-mapping"></a><span data-ttu-id="7ad52-134">資料庫純量函式對應</span><span class="sxs-lookup"><span data-stu-id="7ad52-134">Database scalar function mapping</span></span>

<span data-ttu-id="7ad52-135">EF Core 2.0 包含 [Paul Middleton](https://github.com/pmiddleton) 的重要比重，這會啟用資料庫純量函式與方法虛設常式的對應，以將它們用於 LINQ 查詢並轉譯為 SQL。</span><span class="sxs-lookup"><span data-stu-id="7ad52-135">EF Core 2.0 includes an important contribution from [Paul Middleton](https://github.com/pmiddleton) which enables mapping database scalar functions to method stubs so that they can be used in LINQ queries and translated to SQL.</span></span>

<span data-ttu-id="7ad52-136">以下簡短描述說明如何使用此功能：</span><span class="sxs-lookup"><span data-stu-id="7ad52-136">Here is a brief description of how the feature can be used:</span></span>

<span data-ttu-id="7ad52-137">在 `DbContext` 上宣告靜態方法，並將它加上 `DbFunctionAttribute` 註釋：</span><span class="sxs-lookup"><span data-stu-id="7ad52-137">Declare a static method on your `DbContext` and annotate it with `DbFunctionAttribute`:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    [DbFunction]
    public static int PostReadCount(int blogId)
    {
        throw new NotImplementedException();
    }
}
```

<span data-ttu-id="7ad52-138">這類方法會自動予以註冊。</span><span class="sxs-lookup"><span data-stu-id="7ad52-138">Methods like this are automatically registered.</span></span> <span data-ttu-id="7ad52-139">註冊後，LINQ 查詢中的方法呼叫可轉譯成 SQL 中的函式呼叫：</span><span class="sxs-lookup"><span data-stu-id="7ad52-139">Once registered, calls to the method in a LINQ query can be translated to function calls in SQL:</span></span>

``` csharp
var query =
    from p in context.Posts
    where BloggingContext.PostReadCount(p.Id) > 5
    select p;
```

<span data-ttu-id="7ad52-140">幾個注意事項：</span><span class="sxs-lookup"><span data-stu-id="7ad52-140">A few things to note:</span></span>

- <span data-ttu-id="7ad52-141">依照慣例，在產生 SQL 時，會使用方法的名稱做為函式的名稱（在此案例中為使用者定義函數），但是您可以在方法註冊期間覆寫名稱和架構。</span><span class="sxs-lookup"><span data-stu-id="7ad52-141">By convention the name of the method is used as the name of a function (in this case a user-defined function) when generating the SQL, but you can override the name and schema during method registration.</span></span>
- <span data-ttu-id="7ad52-142">目前僅支援純量函數。</span><span class="sxs-lookup"><span data-stu-id="7ad52-142">Currently only scalar functions are supported.</span></span>
- <span data-ttu-id="7ad52-143">您必須在資料庫中建立對應函式。</span><span class="sxs-lookup"><span data-stu-id="7ad52-143">You must create the mapped function in the database.</span></span> <span data-ttu-id="7ad52-144">EF Core 遷移並不會負責建立它。</span><span class="sxs-lookup"><span data-stu-id="7ad52-144">EF Core migrations will not take care of creating it.</span></span>

### <a name="self-contained-type-configuration-for-code-first"></a><span data-ttu-id="7ad52-145">Code First 的獨立類型組態</span><span class="sxs-lookup"><span data-stu-id="7ad52-145">Self-contained type configuration for code first</span></span>

<span data-ttu-id="7ad52-146">在 EF6 中，可能已封裝衍生自 *EntityTypeConfiguration* 之特定實體類型的 Code First 組態。</span><span class="sxs-lookup"><span data-stu-id="7ad52-146">In EF6 it was possible to encapsulate the code first configuration of a specific entity type by deriving from *EntityTypeConfiguration*.</span></span> <span data-ttu-id="7ad52-147">在 EF Core 2.0 中，我們將重新使用這種模式：</span><span class="sxs-lookup"><span data-stu-id="7ad52-147">In EF Core 2.0 we are bringing this pattern back:</span></span>

``` csharp
class CustomerConfiguration : IEntityTypeConfiguration<Customer>
{
    public void Configure(EntityTypeBuilder<Customer> builder)
    {
        builder.HasKey(c => c.AlternateKey);
        builder.Property(c => c.Name).HasMaxLength(200);
    }
}

...
// OnModelCreating
builder.ApplyConfiguration(new CustomerConfiguration());
```

## <a name="high-performance"></a><span data-ttu-id="7ad52-148">高效能</span><span class="sxs-lookup"><span data-stu-id="7ad52-148">High Performance</span></span>

### <a name="dbcontext-pooling"></a><span data-ttu-id="7ad52-149">DbContext 共用</span><span class="sxs-lookup"><span data-stu-id="7ad52-149">DbContext pooling</span></span>

<span data-ttu-id="7ad52-150">在 ASP.NET Core 應用程式中使用 EF Core 的基本模式，通常包含將自訂 DbContext 類型註冊到相依性插入系統，以及稍後透過控制器中的建構函式參數來取得該類型的執行個體。</span><span class="sxs-lookup"><span data-stu-id="7ad52-150">The basic pattern for using EF Core in an ASP.NET Core application usually involves registering a custom DbContext type into the dependency injection system and later obtaining instances of that type through constructor parameters in controllers.</span></span> <span data-ttu-id="7ad52-151">這表示會針對每個要求建立新的 DbCoNtext 實例。</span><span class="sxs-lookup"><span data-stu-id="7ad52-151">This means a new instance of the DbContext is created for each request.</span></span>

<span data-ttu-id="7ad52-152">在 2.0 版中，我們引進在相依性插入中註冊自訂 DbContext 類型的新方式，而相依性插入會以透明方式引進可重複使用 DbContext 執行個體的集區中。</span><span class="sxs-lookup"><span data-stu-id="7ad52-152">In version 2.0 we are introducing a new way to register custom DbContext types in dependency injection which transparently introduces a pool of reusable DbContext instances.</span></span> <span data-ttu-id="7ad52-153">若要使用 DbContext 共用，請在服務註冊期間使用 `AddDbContextPool`，而非 `AddDbContext`：</span><span class="sxs-lookup"><span data-stu-id="7ad52-153">To use DbContext pooling, use the `AddDbContextPool` instead of `AddDbContext` during service registration:</span></span>

``` csharp
services.AddDbContextPool<BloggingContext>(
    options => options.UseSqlServer(connectionString));
```

<span data-ttu-id="7ad52-154">如果使用這種方法，則在控制器要求 DbContext 執行個體時，我們會先檢查集區中是否有執行個體可用。</span><span class="sxs-lookup"><span data-stu-id="7ad52-154">If this method is used, at the time a DbContext instance is requested by a controller we will first check if there is an instance available in the pool.</span></span> <span data-ttu-id="7ad52-155">要求處理完成之後，會重設執行個體上的任何狀態，並將執行個體本身傳回到集區。</span><span class="sxs-lookup"><span data-stu-id="7ad52-155">Once the request processing finalizes, any state on the instance is reset and the instance is itself returned to the pool.</span></span>

<span data-ttu-id="7ad52-156">這在概念上類似連線共用如何在 ADO.NET 提供者中操作，而且優點是節省 DbContext 執行個體的一些初始化成本。</span><span class="sxs-lookup"><span data-stu-id="7ad52-156">This is conceptually similar to how connection pooling operates in ADO.NET providers and has the advantage of saving some of the cost of initialization of DbContext instance.</span></span>

### <a name="limitations"></a><span data-ttu-id="7ad52-157">限制</span><span class="sxs-lookup"><span data-stu-id="7ad52-157">Limitations</span></span>

<span data-ttu-id="7ad52-158">新的方法引進 DbContext 的 `OnConfiguring()` 方法中可進行作業的一些限制。</span><span class="sxs-lookup"><span data-stu-id="7ad52-158">The new method introduces a few limitations on what can be done in the `OnConfiguring()` method of the DbContext.</span></span>

> [!WARNING]  
> <span data-ttu-id="7ad52-159">如果您在不應該於要求間共用的衍生 DbContext 類別中維護自己的狀態 (例如私用欄位)，請避免使用 DbContext 共用。</span><span class="sxs-lookup"><span data-stu-id="7ad52-159">Avoid using DbContext Pooling if you maintain your own state (for example, private fields) in your derived DbContext class that should not be shared across requests.</span></span> <span data-ttu-id="7ad52-160">EF Core 在將 DbCoNtext 實例新增至集區之前，只會重設其感知的狀態。</span><span class="sxs-lookup"><span data-stu-id="7ad52-160">EF Core will only reset the state that it is aware of before adding a DbContext instance to the pool.</span></span>

### <a name="explicitly-compiled-queries"></a><span data-ttu-id="7ad52-161">明確地編譯查詢</span><span class="sxs-lookup"><span data-stu-id="7ad52-161">Explicitly compiled queries</span></span>

<span data-ttu-id="7ad52-162">這是第二個選擇加入的效能功能，專為高延展案例提供助益而設計。</span><span class="sxs-lookup"><span data-stu-id="7ad52-162">This is the second opt-in performance feature designed to offer benefits in high-scale scenarios.</span></span>

<span data-ttu-id="7ad52-163">舊版 EF 以及 LINQ to SQL 中已提供手動或明確已編譯的查詢 API，允許應用程式快取查詢的轉譯，這樣只要將它們計算一次，就能執行多次。</span><span class="sxs-lookup"><span data-stu-id="7ad52-163">Manual or explicitly compiled query APIs have been available in previous versions of EF and also in LINQ to SQL to allow applications to cache the translation of queries so that they can be computed only once and executed many times.</span></span>

<span data-ttu-id="7ad52-164">一般而言，雖然 EF Core 可以根據查詢運算式的雜湊表示法來自動編譯並快取查詢 ，但是可以使用這項機制透過不計算雜湊和快取查閱來提升少許效能，以允許應用程式透過委派叫用來使用已編譯的查詢。</span><span class="sxs-lookup"><span data-stu-id="7ad52-164">Although in general EF Core can automatically compile and cache queries based on a hashed representation of the query expressions, this mechanism can be used to obtain a small performance gain by bypassing the computation of the hash and the cache lookup, allowing the application to use an already compiled query through the invocation of a delegate.</span></span>

``` csharp
// Create an explicitly compiled query
private static Func<CustomerContext, int, Customer> _customerById =
    EF.CompileQuery((CustomerContext db, int id) =>
        db.Customers
            .Include(c => c.Address)
            .Single(c => c.Id == id));

// Use the compiled query by invoking it
using (var db = new CustomerContext())
{
   var customer = _customerById(db, 147);
}
```

## <a name="change-tracking"></a><span data-ttu-id="7ad52-165">變更追蹤</span><span class="sxs-lookup"><span data-stu-id="7ad52-165">Change Tracking</span></span>

### <a name="attach-can-track-a-graph-of-new-and-existing-entities"></a><span data-ttu-id="7ad52-166">附加可以追蹤全新和現有實體的圖形</span><span class="sxs-lookup"><span data-stu-id="7ad52-166">Attach can track a graph of new and existing entities</span></span>

<span data-ttu-id="7ad52-167">EF Core 支援透過不同的機制來自動產生索引鍵值。</span><span class="sxs-lookup"><span data-stu-id="7ad52-167">EF Core supports automatic generation of key values through a variety of mechanisms.</span></span> <span data-ttu-id="7ad52-168">使用此功能時，如果重要屬性是 CLR 預設值，則會產生值：通常是零或 Null。</span><span class="sxs-lookup"><span data-stu-id="7ad52-168">When using this feature, a value is generated if the key property is the CLR default--usually zero or null.</span></span> <span data-ttu-id="7ad52-169">這表示可以將實體的圖表傳遞至 `DbContext.Attach` 或 `DbSet.Attach`，而且 EF Core 會將已設定索引鍵的實體標示為 `Unchanged`，同時將未設定索引鍵的實體標示為 `Added`。</span><span class="sxs-lookup"><span data-stu-id="7ad52-169">This means that a graph of entities can be passed to `DbContext.Attach` or `DbSet.Attach` and EF Core will mark those entities that have a key already set as `Unchanged` while those entities that do not have a key set will be marked as `Added`.</span></span> <span data-ttu-id="7ad52-170">使用產生的索引鍵時，這可以輕鬆地附加混合全新和現有實體的圖形。</span><span class="sxs-lookup"><span data-stu-id="7ad52-170">This makes it easy to attach a graph of mixed new and existing entities when using generated keys.</span></span> <span data-ttu-id="7ad52-171">`DbContext.Update` 和 `DbSet.Update` 的運作方式相同，差別在於將設定索引鍵的實體標示為 `Modified`，而不是 `Unchanged`。</span><span class="sxs-lookup"><span data-stu-id="7ad52-171">`DbContext.Update` and `DbSet.Update` work in the same way, except that entities with a key set are marked as `Modified` instead of `Unchanged`.</span></span>

## <a name="query"></a><span data-ttu-id="7ad52-172">查詢</span><span class="sxs-lookup"><span data-stu-id="7ad52-172">Query</span></span>

### <a name="improved-linq-translation"></a><span data-ttu-id="7ad52-173">改良的 LINQ 轉譯</span><span class="sxs-lookup"><span data-stu-id="7ad52-173">Improved LINQ translation</span></span>

<span data-ttu-id="7ad52-174">讓更多查詢順利執行，並在資料庫中評估更多邏輯 (而不是在記憶體中) 以及從資料庫擷取較少的資料。</span><span class="sxs-lookup"><span data-stu-id="7ad52-174">Enables more queries to successfully execute, with more logic being evaluated in the database (rather than in-memory) and less data unnecessarily being retrieved from the database.</span></span>

### <a name="groupjoin-improvements"></a><span data-ttu-id="7ad52-175">GroupJoin 改善</span><span class="sxs-lookup"><span data-stu-id="7ad52-175">GroupJoin improvements</span></span>

<span data-ttu-id="7ad52-176">這項工作可改善針對群組聯結所產生的 SQL。</span><span class="sxs-lookup"><span data-stu-id="7ad52-176">This work improves the SQL that is generated for group joins.</span></span> <span data-ttu-id="7ad52-177">群組聯結最常是選擇性導覽屬性的子查詢結果。</span><span class="sxs-lookup"><span data-stu-id="7ad52-177">Group joins are most often a result of sub-queries on optional navigation properties.</span></span>

### <a name="string-interpolation-in-fromsql-and-executesqlcommand"></a><span data-ttu-id="7ad52-178">FromSql 和 ExecuteSqlCommand 中的字串插值</span><span class="sxs-lookup"><span data-stu-id="7ad52-178">String interpolation in FromSql and ExecuteSqlCommand</span></span>

<span data-ttu-id="7ad52-179">C# 6 已引進「字串插值」，此功能允許 C# 運算式直接內嵌在字串常值中，並提供不錯的方式在執行階段建置字串。</span><span class="sxs-lookup"><span data-stu-id="7ad52-179">C# 6 introduced String Interpolation, a feature that allows C# expressions to be directly embedded in string literals, providing a nice way of building strings at runtime.</span></span> <span data-ttu-id="7ad52-180">在 EF Core 2.0 中，我們已在接受原始 SQL 字串的兩個主要 API 中新增內插字串的特殊支援：`FromSql` and `ExecuteSqlCommand`.</span><span class="sxs-lookup"><span data-stu-id="7ad52-180">In EF Core 2.0 we added special support for interpolated strings to our two primary APIs that accept raw SQL strings: `FromSql` and `ExecuteSqlCommand`.</span></span> <span data-ttu-id="7ad52-181">這項新的支援可讓您以「安全」的方式使用 c # 字串插補。</span><span class="sxs-lookup"><span data-stu-id="7ad52-181">This new support allows C# string interpolation to be used in a "safe" manner.</span></span> <span data-ttu-id="7ad52-182">也就是說，可防止在執行階段動態建構 SQL 時可能發生的常見 SQL 插入錯誤。</span><span class="sxs-lookup"><span data-stu-id="7ad52-182">That is, in a way that protects against common SQL injection mistakes that can occur when dynamically constructing SQL at runtime.</span></span>

<span data-ttu-id="7ad52-183">範例如下：</span><span class="sxs-lookup"><span data-stu-id="7ad52-183">Here is an example:</span></span>

``` csharp
var city = "London";
var contactTitle = "Sales Representative";

using (var context = CreateContext())
{
    context.Set<Customer>()
        .FromSql($@"
            SELECT *
            FROM ""Customers""
            WHERE ""City"" = {city} AND
                ""ContactTitle"" = {contactTitle}")
            .ToArray();
  }
```

<span data-ttu-id="7ad52-184">在此範例中，有兩個以 SQL 格式字串內嵌的變數。</span><span class="sxs-lookup"><span data-stu-id="7ad52-184">In this example, there are two variables embedded in the SQL format string.</span></span> <span data-ttu-id="7ad52-185">EF Core 會產生下列 SQL：</span><span class="sxs-lookup"><span data-stu-id="7ad52-185">EF Core will produce the following SQL:</span></span>

```sql
@p0='London' (Size = 4000)
@p1='Sales Representative' (Size = 4000)

SELECT *
FROM ""Customers""
WHERE ""City"" = @p0
    AND ""ContactTitle"" = @p1
```

### <a name="effunctionslike"></a><span data-ttu-id="7ad52-186">EF.Functions.Like()</span><span class="sxs-lookup"><span data-stu-id="7ad52-186">EF.Functions.Like()</span></span>

<span data-ttu-id="7ad52-187">我們已新增 EF.Functions 屬性，而 EF Core 或提供者用來定義可對應至資料庫函式或運算子的方法，以在 LINQ 查詢 中予以叫用。</span><span class="sxs-lookup"><span data-stu-id="7ad52-187">We have added the EF.Functions property which can be used by EF Core or providers to define methods that map to database functions or operators so that those can be invoked in LINQ queries.</span></span> <span data-ttu-id="7ad52-188">這種方法的第一個範例是 Like()：</span><span class="sxs-lookup"><span data-stu-id="7ad52-188">The first example of such a method is Like():</span></span>

``` csharp
var aCustomers =
    from c in context.Customers
    where EF.Functions.Like(c.Name, "a%")
    select c;
```

<span data-ttu-id="7ad52-189">請注意，Like() 隨附記憶體中實作，這在處理記憶體中資料庫或用戶端需要進行述詞評估時都很方便使用。</span><span class="sxs-lookup"><span data-stu-id="7ad52-189">Note that Like() comes with an in-memory implementation, which can be handy when working against an in-memory database or when evaluation of the predicate needs to occur on the client side.</span></span>

## <a name="database-management"></a><span data-ttu-id="7ad52-190">資料庫管理</span><span class="sxs-lookup"><span data-stu-id="7ad52-190">Database management</span></span>

### <a name="pluralization-hook-for-dbcontext-scaffolding"></a><span data-ttu-id="7ad52-191">DbCoNtext 樣板的複數表示勾點</span><span class="sxs-lookup"><span data-stu-id="7ad52-191">Pluralization hook for DbContext scaffolding</span></span>

<span data-ttu-id="7ad52-192">EF Core 2.0 引進新的 *IPluralizer* 服務，以用來將實體類型名稱單數化，並將 DbSet 名稱複數化。</span><span class="sxs-lookup"><span data-stu-id="7ad52-192">EF Core 2.0 introduces a new *IPluralizer* service that is used to singularize entity type names and pluralize DbSet names.</span></span> <span data-ttu-id="7ad52-193">預設實作是不操作，因此這只是人員可以輕鬆插入其專屬 pluralizer 的攔截。</span><span class="sxs-lookup"><span data-stu-id="7ad52-193">The default implementation is a no-op, so this is just a hook where folks can easily plug in their own pluralizer.</span></span>

<span data-ttu-id="7ad52-194">以下是開發人員在其專屬 pluralizer 中攔截的方式：</span><span class="sxs-lookup"><span data-stu-id="7ad52-194">Here is what it looks like for a developer to hook in their own pluralizer:</span></span>

``` csharp
public class MyDesignTimeServices : IDesignTimeServices
{
    public void ConfigureDesignTimeServices(IServiceCollection services)
    {
        services.AddSingleton<IPluralizer, MyPluralizer>();
    }
}

public class MyPluralizer : IPluralizer
{
    public string Pluralize(string name)
    {
        return Inflector.Inflector.Pluralize(name) ?? name;
    }

    public string Singularize(string name)
    {
        return Inflector.Inflector.Singularize(name) ?? name;
    }
}
```

## <a name="others"></a><span data-ttu-id="7ad52-195">其他</span><span class="sxs-lookup"><span data-stu-id="7ad52-195">Others</span></span>

### <a name="move-adonet-sqlite-provider-to-sqlitepclraw"></a><span data-ttu-id="7ad52-196">將 ADO.NET SQLite 提供者移至 SQLitePCL.raw</span><span class="sxs-lookup"><span data-stu-id="7ad52-196">Move ADO.NET SQLite provider to SQLitePCL.raw</span></span>

<span data-ttu-id="7ad52-197">這可在 Microsoft.Data.Sqlite 中提供更穩固的方案，以將原生 SQLite 二進位檔散發到不同的平台。</span><span class="sxs-lookup"><span data-stu-id="7ad52-197">This gives us a more robust solution in Microsoft.Data.Sqlite for distributing native SQLite binaries on different platforms.</span></span>

### <a name="only-one-provider-per-model"></a><span data-ttu-id="7ad52-198">一個模型只會有一個提供者</span><span class="sxs-lookup"><span data-stu-id="7ad52-198">Only one provider per model</span></span>

<span data-ttu-id="7ad52-199">大幅增加提供者如何與模型互動，以及簡化慣例、註釋和 Fluent API 如何與不同的提供者搭配運作。</span><span class="sxs-lookup"><span data-stu-id="7ad52-199">Significantly augments how providers can interact with the model and simplifies how conventions, annotations and fluent APIs work with different providers.</span></span>

<span data-ttu-id="7ad52-200">EF Core 2.0 現在會為使用的每個不同提供者建置不同的 [IModel](https://github.com/aspnet/EntityFramework/blob/master/src/EFCore/Metadata/IModel.cs)。</span><span class="sxs-lookup"><span data-stu-id="7ad52-200">EF Core 2.0 will now build a different [IModel](https://github.com/aspnet/EntityFramework/blob/master/src/EFCore/Metadata/IModel.cs) for each different provider being used.</span></span> <span data-ttu-id="7ad52-201">應用程式通常可以看到這項作業。</span><span class="sxs-lookup"><span data-stu-id="7ad52-201">This is usually transparent to the application.</span></span> <span data-ttu-id="7ad52-202">這簡化了較低層級的中繼資料 Api，讓*一般關聯式中繼資料概念*的任何存取一律透過呼叫來進行 `.Relational` ，而不是 `.SqlServer` 、等等 `.Sqlite` 。</span><span class="sxs-lookup"><span data-stu-id="7ad52-202">This has facilitated a simplification of lower-level metadata APIs such that any access to *common relational metadata concepts* is always made through a call to `.Relational` instead of `.SqlServer`, `.Sqlite`, etc.</span></span>

### <a name="consolidated-logging-and-diagnostics"></a><span data-ttu-id="7ad52-203">匯總記錄和診斷</span><span class="sxs-lookup"><span data-stu-id="7ad52-203">Consolidated logging and diagnostics</span></span>

<span data-ttu-id="7ad52-204">記錄 (根據 ILogger) 和診斷 (根據 DiagnosticSource) 機制現在共用更多程式碼。</span><span class="sxs-lookup"><span data-stu-id="7ad52-204">Logging (based on ILogger) and Diagnostics (based on DiagnosticSource) mechanisms now share more code.</span></span>

<span data-ttu-id="7ad52-205">在 2.0 中，已經變更傳送至 ILogger 之訊息的事件識別碼。</span><span class="sxs-lookup"><span data-stu-id="7ad52-205">The event IDs for messages sent to an ILogger have changed in 2.0.</span></span> <span data-ttu-id="7ad52-206">在 EF Core 程式碼中，事件識別碼現在是唯一的。</span><span class="sxs-lookup"><span data-stu-id="7ad52-206">The event IDs are now unique across EF Core code.</span></span> <span data-ttu-id="7ad52-207">例如，這些訊息現在也遵循 MVC 所使用結構化記錄的標準模式。</span><span class="sxs-lookup"><span data-stu-id="7ad52-207">These messages now also follow the standard pattern for structured logging used by, for example, MVC.</span></span>

<span data-ttu-id="7ad52-208">記錄器類別也已經變更。</span><span class="sxs-lookup"><span data-stu-id="7ad52-208">Logger categories have also changed.</span></span> <span data-ttu-id="7ad52-209">現在已有一組類別可透過 [DbLoggerCategory](https://github.com/aspnet/EntityFramework/blob/master/src/EFCore/DbLoggerCategory.cs) 進行存取。</span><span class="sxs-lookup"><span data-stu-id="7ad52-209">There is now a well-known set of categories accessed through [DbLoggerCategory](https://github.com/aspnet/EntityFramework/blob/master/src/EFCore/DbLoggerCategory.cs).</span></span>

<span data-ttu-id="7ad52-210">DiagnosticSource 事件現在會使用相同的事件識別碼名稱作為對應的 `ILogger` 訊息。</span><span class="sxs-lookup"><span data-stu-id="7ad52-210">DiagnosticSource events now use the same event ID names as the corresponding `ILogger` messages.</span></span>
