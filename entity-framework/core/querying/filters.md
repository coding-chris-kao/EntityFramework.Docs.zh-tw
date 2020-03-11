---
title: 全域查詢篩選條件 - EF Core
author: anpete
ms.date: 11/03/2017
uid: core/querying/filters
ms.openlocfilehash: 9262ff7970b0502945480c673315071cbc3f44b9
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417726"
---
# <a name="global-query-filters"></a><span data-ttu-id="61a57-102">全域查詢篩選條件</span><span class="sxs-lookup"><span data-stu-id="61a57-102">Global Query Filters</span></span>

> [!NOTE]
> <span data-ttu-id="61a57-103">此功能是在 EF Core 2.0 中引入。</span><span class="sxs-lookup"><span data-stu-id="61a57-103">This feature was introduced in EF Core 2.0.</span></span>

<span data-ttu-id="61a57-104">全域查詢篩選條件是中繼資料模型 (通常位於 *OnModelCreating*) 中套用到實體類型的 LINQ 查詢述詞 (布林運算式通常會傳遞到 LINQ *Where* 查詢運算子)。</span><span class="sxs-lookup"><span data-stu-id="61a57-104">Global query filters are LINQ query predicates (a boolean expression typically passed to the LINQ *Where* query operator) applied to Entity Types in the metadata model (usually in *OnModelCreating*).</span></span> <span data-ttu-id="61a57-105">這類篩選會自動套用至任何涉及這些「實體類型」(包含間接參考的「實體類型」) 的 LINQ 查詢 (例如，使用 Include 或直接導覽屬性參考)。</span><span class="sxs-lookup"><span data-stu-id="61a57-105">Such filters are automatically applied to any LINQ queries involving those Entity Types, including Entity Types referenced indirectly, such as through the use of Include or direct navigation property references.</span></span> <span data-ttu-id="61a57-106">此功能的一些常見應用如下：</span><span class="sxs-lookup"><span data-stu-id="61a57-106">Some common applications of this feature are:</span></span>

* <span data-ttu-id="61a57-107">**虛刪除**：實體類型會定義 *IsDeleted* 屬性。</span><span class="sxs-lookup"><span data-stu-id="61a57-107">**Soft delete** - An Entity Type defines an *IsDeleted* property.</span></span>
* <span data-ttu-id="61a57-108">**多租用戶**：實體類型會定義 *TenantId* 屬性。</span><span class="sxs-lookup"><span data-stu-id="61a57-108">**Multi-tenancy** - An Entity Type defines a *TenantId* property.</span></span>

## <a name="example"></a><span data-ttu-id="61a57-109">範例</span><span class="sxs-lookup"><span data-stu-id="61a57-109">Example</span></span>

<span data-ttu-id="61a57-110">下列範例示範如何使用全域查詢篩選條件，在簡單的部落格模型中實作虛刪除和多租用戶查詢行為。</span><span class="sxs-lookup"><span data-stu-id="61a57-110">The following example shows how to use Global Query Filters to implement soft-delete and multi-tenancy query behaviors in a simple blogging model.</span></span>

> [!TIP]
> <span data-ttu-id="61a57-111">您可以在 GitHub 上檢視此文章的[範例](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/QueryFilters) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="61a57-111">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/QueryFilters) on GitHub.</span></span>

<span data-ttu-id="61a57-112">首先，定義實體：</span><span class="sxs-lookup"><span data-stu-id="61a57-112">First, define the entities:</span></span>

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#Entities)]

<span data-ttu-id="61a57-113">請注意 _Blog_ 實體上 _tenantId_ 欄位的宣告。</span><span class="sxs-lookup"><span data-stu-id="61a57-113">Note the declaration of a _tenantId_ field on the _Blog_ entity.</span></span> <span data-ttu-id="61a57-114">這會用來將每個 Blog 執行個體與特定租用戶產生關聯。</span><span class="sxs-lookup"><span data-stu-id="61a57-114">This will be used to associate each Blog instance with a specific tenant.</span></span> <span data-ttu-id="61a57-115">此外，還定義了 _Post_ 實體類型上的 _IsDeleted_ 屬性。</span><span class="sxs-lookup"><span data-stu-id="61a57-115">Also defined is an _IsDeleted_ property on the _Post_ entity type.</span></span> <span data-ttu-id="61a57-116">這會用來追蹤是否已將 _Post_ 執行個體「虛刪除」。</span><span class="sxs-lookup"><span data-stu-id="61a57-116">This is used to keep track of whether a _Post_ instance has been "soft-deleted".</span></span> <span data-ttu-id="61a57-117">亦即，將執行個體標示為已刪除，而不實際移除底層資料。</span><span class="sxs-lookup"><span data-stu-id="61a57-117">That is, the instance is marked as deleted without physically removing the underlying data.</span></span>

<span data-ttu-id="61a57-118">接下來，使用 _API，在_OnModelCreating`HasQueryFilter` 中設定查詢篩選條件。</span><span class="sxs-lookup"><span data-stu-id="61a57-118">Next, configure the query filters in _OnModelCreating_ using the `HasQueryFilter` API.</span></span>

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#Configuration)]

<span data-ttu-id="61a57-119">傳遞到 _HasQueryFilter_ 呼叫的述詞運算式，現在將針對那些類型自動套用到任何 LINQ 查詢。</span><span class="sxs-lookup"><span data-stu-id="61a57-119">The predicate expressions passed to the _HasQueryFilter_ calls will now automatically be applied to any LINQ queries for those types.</span></span>

> [!TIP]
> <span data-ttu-id="61a57-120">請注意 DbContext 執行個體層級欄位的使用方式：`_tenantId` 用來設定目前的租用戶。</span><span class="sxs-lookup"><span data-stu-id="61a57-120">Note the use of a DbContext instance level field: `_tenantId` used to set the current tenant.</span></span> <span data-ttu-id="61a57-121">模型層級篩選將會使用正確內容執行個體 (亦即，執行查詢的執行個體) 中的值。</span><span class="sxs-lookup"><span data-stu-id="61a57-121">Model-level filters will use the value from the correct context instance (that is, the instance that is executing the query).</span></span>

> [!NOTE]
> <span data-ttu-id="61a57-122">目前不能在相同的實體上定義多個查詢篩選準則-只會套用最後一個。</span><span class="sxs-lookup"><span data-stu-id="61a57-122">It is currently not possible to define multiple query filters on the same entity - only the last one will be applied.</span></span> <span data-ttu-id="61a57-123">不過，您可以使用邏輯_AND_運算子（[中C#的`&&` ](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/boolean-logical-operators#conditional-logical-and-operator-)）來定義具有多個條件的單一篩選。</span><span class="sxs-lookup"><span data-stu-id="61a57-123">However, you can define a single filter with multiple conditions using the logical _AND_ operator ([`&&` in C#](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/boolean-logical-operators#conditional-logical-and-operator-)).</span></span>

## <a name="disabling-filters"></a><span data-ttu-id="61a57-124">停用篩選條件</span><span class="sxs-lookup"><span data-stu-id="61a57-124">Disabling Filters</span></span>

<span data-ttu-id="61a57-125">可能會使用 `IgnoreQueryFilters()` 運算子來停用個別 LINQ 查詢的篩選條件。</span><span class="sxs-lookup"><span data-stu-id="61a57-125">Filters may be disabled for individual LINQ queries by using the `IgnoreQueryFilters()` operator.</span></span>

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#IgnoreFilters)]

## <a name="limitations"></a><span data-ttu-id="61a57-126">限制</span><span class="sxs-lookup"><span data-stu-id="61a57-126">Limitations</span></span>

<span data-ttu-id="61a57-127">全域查詢篩選條件具有下列限制：</span><span class="sxs-lookup"><span data-stu-id="61a57-127">Global query filters have the following limitations:</span></span>

* <span data-ttu-id="61a57-128">只能針對繼承階層的根實體類型定義篩選條件。</span><span class="sxs-lookup"><span data-stu-id="61a57-128">Filters can only be defined for the root Entity Type of an inheritance hierarchy.</span></span>
