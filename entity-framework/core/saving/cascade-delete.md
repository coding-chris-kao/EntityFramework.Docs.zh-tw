---
title: 串聯刪除 - EF Core
description: 在刪除主體實體時設定相關實體的刪除行為
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ee8e14ec-2158-4c9c-96b5-118715e2ed9e
uid: core/saving/cascade-delete
ms.openlocfilehash: cf17e6586b89376b2d7fcc0f9cbfc8e1c4f6ba58
ms.sourcegitcommit: 7c3939504bb9da3f46bea3443638b808c04227c2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/09/2020
ms.locfileid: "89617400"
---
# <a name="cascade-delete"></a><span data-ttu-id="a05a7-103">串聯刪除</span><span class="sxs-lookup"><span data-stu-id="a05a7-103">Cascade Delete</span></span>

<span data-ttu-id="a05a7-104">串聯刪除在資料庫術語中常用來描述一種允許在刪除一個資料列時自動觸發刪除相關資料列的特性。</span><span class="sxs-lookup"><span data-stu-id="a05a7-104">Cascade delete is commonly used in database terminology to describe a characteristic that allows the deletion of a row to automatically trigger the deletion of related rows.</span></span> <span data-ttu-id="a05a7-105">EF Core 刪除行為也涵蓋一個密切相關的概念，就是在子實體與父系的關聯性已被切斷時便自動刪除子實體，這也通稱為「刪除失去關聯的項目」。</span><span class="sxs-lookup"><span data-stu-id="a05a7-105">A closely related concept also covered by EF Core delete behaviors is the automatic deletion of a child entity when it's relationship to a parent has been severed--this is commonly known as "deleting orphans".</span></span>

<span data-ttu-id="a05a7-106">EF Core 實作數種不同的刪除行為，並允許設定個別關聯性的刪除行為。</span><span class="sxs-lookup"><span data-stu-id="a05a7-106">EF Core implements several different delete behaviors and allows for the configuration of the delete behaviors of individual relationships.</span></span> <span data-ttu-id="a05a7-107">EF Core 也實作一些慣例，可根據[關聯性的必要性](xref:core/modeling/relationships#required-and-optional-relationships)，為每個關聯性自動設定實用的預設刪除行為。</span><span class="sxs-lookup"><span data-stu-id="a05a7-107">EF Core also implements conventions that automatically configure useful default delete behaviors for each relationship based on the [requiredness of the relationship](xref:core/modeling/relationships#required-and-optional-relationships).</span></span>

## <a name="delete-behaviors"></a><span data-ttu-id="a05a7-108">刪除行為</span><span class="sxs-lookup"><span data-stu-id="a05a7-108">Delete behaviors</span></span>

<span data-ttu-id="a05a7-109">定義刪除行為時，是在 *DeleteBehavior* 列舉程式類型中定義，並可傳遞給 *OnDelete* Fluent API，以控制刪除主體/父系實體或切斷與相依/子系實體的關聯性是否應對相依/子系實體造成副作用。</span><span class="sxs-lookup"><span data-stu-id="a05a7-109">Delete behaviors are defined in the *DeleteBehavior* enumerator type and can be passed to the *OnDelete* fluent API to control whether the deletion of a principal/parent entity or the severing of the relationship to dependent/child entities should have a side effect on the dependent/child entities.</span></span>

<span data-ttu-id="a05a7-110">當主體/父系實體被刪除或與子系的關聯性被切斷時，EF 可以採取三個動作：</span><span class="sxs-lookup"><span data-stu-id="a05a7-110">There are three actions EF can take when a principal/parent entity is deleted or the relationship to the child is severed:</span></span>

* <span data-ttu-id="a05a7-111">可以刪除子系/相依項</span><span class="sxs-lookup"><span data-stu-id="a05a7-111">The child/dependent can be deleted</span></span>
* <span data-ttu-id="a05a7-112">可以將子系的外部索引鍵設定為 Null</span><span class="sxs-lookup"><span data-stu-id="a05a7-112">The child's foreign key values can be set to null</span></span>
* <span data-ttu-id="a05a7-113">子系保持不變</span><span class="sxs-lookup"><span data-stu-id="a05a7-113">The child remains unchanged</span></span>

> [!NOTE]  
> <span data-ttu-id="a05a7-114">只有在使用 EF Core 來刪除主體實體且已將相依實體載入記憶體中 (亦即針對所追蹤的相依項) 的情況下，才會套用 EF Core 模型中所設定的刪除行為。</span><span class="sxs-lookup"><span data-stu-id="a05a7-114">The delete behavior configured in the EF Core model is only applied when the principal entity is deleted using EF Core and the dependent entities are loaded in memory (that is, for tracked dependents).</span></span> <span data-ttu-id="a05a7-115">必須在資料庫中設定對應的串聯行為，才能確保沒有受到內容追蹤的資料會套用必要的動作。</span><span class="sxs-lookup"><span data-stu-id="a05a7-115">A corresponding cascade behavior needs to be setup in the database to ensure data that is not being tracked by the context has the necessary action applied.</span></span> <span data-ttu-id="a05a7-116">如果您使用 EF Core 來建立資料庫，將會為您設定此串聯行為。</span><span class="sxs-lookup"><span data-stu-id="a05a7-116">If you use EF Core to create the database, this cascade behavior will be setup for you.</span></span>

<span data-ttu-id="a05a7-117">就上述第二個動作而言，如果外部索引鍵值不可為 Null，將其設定為 Null 就會無效。</span><span class="sxs-lookup"><span data-stu-id="a05a7-117">For the second action above, setting a foreign key value to null is not valid if foreign key is not nullable.</span></span> <span data-ttu-id="a05a7-118"> (不可為 null 的外鍵相當於必要的關聯性。 ) 在這些情況下，EF Core 會追蹤外鍵屬性是否已標示為 null，直到呼叫 SaveChanges 為止，此時會擲回例外狀況，因為變更無法保存到資料庫。</span><span class="sxs-lookup"><span data-stu-id="a05a7-118">(A non-nullable foreign key is equivalent to a required relationship.) In these cases, EF Core tracks that the foreign key property has been marked as null until SaveChanges is called, at which time an exception is thrown because the change cannot be persisted to the database.</span></span> <span data-ttu-id="a05a7-119">這與從資料庫收到條件約束違規類似。</span><span class="sxs-lookup"><span data-stu-id="a05a7-119">This is similar to getting a constraint violation from the database.</span></span>

<span data-ttu-id="a05a7-120">如下表中所列，有四種刪除行為。</span><span class="sxs-lookup"><span data-stu-id="a05a7-120">There are four delete behaviors, as listed in the tables below.</span></span>

### <a name="optional-relationships"></a><span data-ttu-id="a05a7-121">組織關係</span><span class="sxs-lookup"><span data-stu-id="a05a7-121">Optional relationships</span></span>

<span data-ttu-id="a05a7-122">針對選擇性關聯性 (可為空值的外部索引鍵)，您「可以」__ 儲存 Null 外部索引鍵值，這會造成下列影響：</span><span class="sxs-lookup"><span data-stu-id="a05a7-122">For optional relationships (nullable foreign key) it _is_ possible to save a null foreign key value, which results in the following effects:</span></span>

| <span data-ttu-id="a05a7-123">行為名稱</span><span class="sxs-lookup"><span data-stu-id="a05a7-123">Behavior Name</span></span>               | <span data-ttu-id="a05a7-124">對記憶體中相依項/子系的影響</span><span class="sxs-lookup"><span data-stu-id="a05a7-124">Effect on dependent/child in memory</span></span>    | <span data-ttu-id="a05a7-125">對資料庫中相依項/子系的影響</span><span class="sxs-lookup"><span data-stu-id="a05a7-125">Effect on dependent/child in database</span></span>  |
|:----------------------------|:---------------------------------------|:---------------------------------------|
| <span data-ttu-id="a05a7-126">**級 聯**</span><span class="sxs-lookup"><span data-stu-id="a05a7-126">**Cascade**</span></span>                 | <span data-ttu-id="a05a7-127">將實體刪除</span><span class="sxs-lookup"><span data-stu-id="a05a7-127">Entities are deleted</span></span>                   | <span data-ttu-id="a05a7-128">將實體刪除</span><span class="sxs-lookup"><span data-stu-id="a05a7-128">Entities are deleted</span></span>                   |
| <span data-ttu-id="a05a7-129">**ClientSetNull** (預設值)</span><span class="sxs-lookup"><span data-stu-id="a05a7-129">**ClientSetNull** (Default)</span></span> | <span data-ttu-id="a05a7-130">將外部索引鍵屬性設定為 Null</span><span class="sxs-lookup"><span data-stu-id="a05a7-130">Foreign key properties are set to null</span></span> | <span data-ttu-id="a05a7-131">無</span><span class="sxs-lookup"><span data-stu-id="a05a7-131">None</span></span>                                   |
| <span data-ttu-id="a05a7-132">**SetNull**</span><span class="sxs-lookup"><span data-stu-id="a05a7-132">**SetNull**</span></span>                 | <span data-ttu-id="a05a7-133">將外部索引鍵屬性設定為 Null</span><span class="sxs-lookup"><span data-stu-id="a05a7-133">Foreign key properties are set to null</span></span> | <span data-ttu-id="a05a7-134">將外部索引鍵屬性設定為 Null</span><span class="sxs-lookup"><span data-stu-id="a05a7-134">Foreign key properties are set to null</span></span> |
| <span data-ttu-id="a05a7-135">**限制**</span><span class="sxs-lookup"><span data-stu-id="a05a7-135">**Restrict**</span></span>                | <span data-ttu-id="a05a7-136">無</span><span class="sxs-lookup"><span data-stu-id="a05a7-136">None</span></span>                                   | <span data-ttu-id="a05a7-137">無</span><span class="sxs-lookup"><span data-stu-id="a05a7-137">None</span></span>                                   |

### <a name="required-relationships"></a><span data-ttu-id="a05a7-138">必要關係</span><span class="sxs-lookup"><span data-stu-id="a05a7-138">Required relationships</span></span>

<span data-ttu-id="a05a7-139">針對必要關聯性 (不可為空值的外部索引鍵)，您「無法」__ 儲存 Null 外部索引鍵值，這會造成下列影響：</span><span class="sxs-lookup"><span data-stu-id="a05a7-139">For required relationships (non-nullable foreign key) it is _not_ possible to save a null foreign key value, which results in the following effects:</span></span>

| <span data-ttu-id="a05a7-140">行為名稱</span><span class="sxs-lookup"><span data-stu-id="a05a7-140">Behavior Name</span></span>         | <span data-ttu-id="a05a7-141">對記憶體中相依項/子系的影響</span><span class="sxs-lookup"><span data-stu-id="a05a7-141">Effect on dependent/child in memory</span></span> | <span data-ttu-id="a05a7-142">對資料庫中相依項/子系的影響</span><span class="sxs-lookup"><span data-stu-id="a05a7-142">Effect on dependent/child in database</span></span> |
|:----------------------|:------------------------------------|:--------------------------------------|
| <span data-ttu-id="a05a7-143">**Cascade** (預設值)</span><span class="sxs-lookup"><span data-stu-id="a05a7-143">**Cascade** (Default)</span></span> | <span data-ttu-id="a05a7-144">將實體刪除</span><span class="sxs-lookup"><span data-stu-id="a05a7-144">Entities are deleted</span></span>                | <span data-ttu-id="a05a7-145">將實體刪除</span><span class="sxs-lookup"><span data-stu-id="a05a7-145">Entities are deleted</span></span>                  |
| <span data-ttu-id="a05a7-146">**ClientSetNull**</span><span class="sxs-lookup"><span data-stu-id="a05a7-146">**ClientSetNull**</span></span>     | <span data-ttu-id="a05a7-147">SaveChanges 擲回例外狀況</span><span class="sxs-lookup"><span data-stu-id="a05a7-147">SaveChanges throws</span></span>                  | <span data-ttu-id="a05a7-148">無</span><span class="sxs-lookup"><span data-stu-id="a05a7-148">None</span></span>                                  |
| <span data-ttu-id="a05a7-149">**SetNull**</span><span class="sxs-lookup"><span data-stu-id="a05a7-149">**SetNull**</span></span>           | <span data-ttu-id="a05a7-150">SaveChanges 擲回例外狀況</span><span class="sxs-lookup"><span data-stu-id="a05a7-150">SaveChanges throws</span></span>                  | <span data-ttu-id="a05a7-151">SaveChanges 擲回例外狀況</span><span class="sxs-lookup"><span data-stu-id="a05a7-151">SaveChanges throws</span></span>                    |
| <span data-ttu-id="a05a7-152">**限制**</span><span class="sxs-lookup"><span data-stu-id="a05a7-152">**Restrict**</span></span>          | <span data-ttu-id="a05a7-153">無</span><span class="sxs-lookup"><span data-stu-id="a05a7-153">None</span></span>                                | <span data-ttu-id="a05a7-154">無</span><span class="sxs-lookup"><span data-stu-id="a05a7-154">None</span></span>                                  |

<span data-ttu-id="a05a7-155">在上表中，「無」\*\* 會造成條件約束違規。</span><span class="sxs-lookup"><span data-stu-id="a05a7-155">In the tables above, *None* can result in a constraint violation.</span></span> <span data-ttu-id="a05a7-156">例如，如果將主體/子系實體刪除，但未採取任何動作來變更相依項/子系的外部索引鍵，資料庫將可能因外部條件約速違規而在 SaveChanges 時擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="a05a7-156">For example, if a principal/child entity is deleted but no action is taken to change the foreign key of a dependent/child, then the database will likely throw on SaveChanges due to a foreign constraint violation.</span></span>

<span data-ttu-id="a05a7-157">概括而言：</span><span class="sxs-lookup"><span data-stu-id="a05a7-157">At a high level:</span></span>

* <span data-ttu-id="a05a7-158">如果您有父系不存在便無法存在的實體，而想要讓 EF 負責自動刪除子系，則請使用 *Cascade*。</span><span class="sxs-lookup"><span data-stu-id="a05a7-158">If you have entities that cannot exist without a parent, and you want EF to take care for deleting the children automatically, then use *Cascade*.</span></span>
  * <span data-ttu-id="a05a7-159">父系不存在便無法存在的實體通常會使用必要關聯性，就此關聯性而言，預設值為 *Cascade*。</span><span class="sxs-lookup"><span data-stu-id="a05a7-159">Entities that cannot exist without a parent usually make use of required relationships, for which *Cascade* is the default.</span></span>
* <span data-ttu-id="a05a7-160">如果您有父系為可有可無的實體，而想要讓 EF 負責為您將外部索引鍵設定為 Null，則請使用 *ClientSetNull*</span><span class="sxs-lookup"><span data-stu-id="a05a7-160">If you have entities that may or may not have a parent, and you want EF to take care of nulling out the foreign key for you, then use *ClientSetNull*</span></span>
  * <span data-ttu-id="a05a7-161">即使父系不存在也能存在的實體通常會使用選擇性關聯性，就此關聯性而言，預設值為 *ClientSetNull*。</span><span class="sxs-lookup"><span data-stu-id="a05a7-161">Entities that can exist without a parent usually make use of optional relationships, for which *ClientSetNull* is the default.</span></span>
  * <span data-ttu-id="a05a7-162">如果您想要讓資料庫在即使未載入子系實體的情況下，也嘗試將 Null 值傳播到子系外部索引鍵，則請使用 *SetNull*。</span><span class="sxs-lookup"><span data-stu-id="a05a7-162">If you want the database to also try to propagate null values to child foreign keys even when the child entity is not loaded, then use *SetNull*.</span></span> <span data-ttu-id="a05a7-163">不過，請注意，資料庫必須要能夠支援此設定，以這種方式設定資料庫可能造成其他限制，這在實務上常常會造成此選項不切實際。</span><span class="sxs-lookup"><span data-stu-id="a05a7-163">However, note that the database must support this, and configuring the database like this can result in other restrictions, which in practice often makes this option impractical.</span></span> <span data-ttu-id="a05a7-164">這就是為什麼 *SetNull* 並非預設值的原因。</span><span class="sxs-lookup"><span data-stu-id="a05a7-164">This is why *SetNull* is not the default.</span></span>
* <span data-ttu-id="a05a7-165">如果您不想要讓 EF Core 自動刪除實體或自動將外部索引鍵設定為 Null，則請使用 *Restrict*。</span><span class="sxs-lookup"><span data-stu-id="a05a7-165">If you don't want EF Core to ever delete an entity automatically or null out the foreign key automatically, then use *Restrict*.</span></span> <span data-ttu-id="a05a7-166">請注意，這會要求您的程式碼手動將子系實體與其外部索引鍵值保持同步，否則將會擲回條件約束例外狀況。</span><span class="sxs-lookup"><span data-stu-id="a05a7-166">Note that this requires that your code keep child entities and their foreign key values in sync manually otherwise constraint exceptions will be thrown.</span></span>

> [!NOTE]
> <span data-ttu-id="a05a7-167">不同於 EF6，在 EF Core 中，串聯影響不會立即發生，而是只有在呼叫 SaveChanges 時才會發生。</span><span class="sxs-lookup"><span data-stu-id="a05a7-167">In EF Core, unlike EF6, cascading effects do not happen immediately, but instead only when SaveChanges is called.</span></span>

> [!NOTE]  
> <span data-ttu-id="a05a7-168">**EF Core 2.0 中的變更：** 在舊版中，*Restrict* 會導致將所追蹤相依實體中的選擇性外部索引鍵屬性設定為 Null，而且是選擇性關聯性的預設刪除行為。</span><span class="sxs-lookup"><span data-stu-id="a05a7-168">**Changes in EF Core 2.0:** In previous releases, *Restrict* would cause optional foreign key properties in tracked dependent entities to be set to null, and was the default delete behavior for optional relationships.</span></span> <span data-ttu-id="a05a7-169">在 EF Core 2.0 中，則導入了 *ClientSetNull* 來代表該行為，並成為選擇性關聯性的預設值。</span><span class="sxs-lookup"><span data-stu-id="a05a7-169">In EF Core 2.0, the *ClientSetNull* was introduced to represent that behavior and became the default for optional relationships.</span></span> <span data-ttu-id="a05a7-170">*Restrict* 的行為在調整後變成一律不會對相依實體產生任何副作用。</span><span class="sxs-lookup"><span data-stu-id="a05a7-170">The behavior of *Restrict* was adjusted to never have any side effects on dependent entities.</span></span>

## <a name="entity-deletion-examples"></a><span data-ttu-id="a05a7-171">實體刪除範例</span><span class="sxs-lookup"><span data-stu-id="a05a7-171">Entity deletion examples</span></span>

<span data-ttu-id="a05a7-172">對於可供下載並執行的[範例](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/CascadeDelete/)，以下程式碼是其中的一部分。</span><span class="sxs-lookup"><span data-stu-id="a05a7-172">The code below is part of a [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/CascadeDelete/) that can be downloaded and run.</span></span> <span data-ttu-id="a05a7-173">此範例示範刪除父系實體時，選擇性和必要關聯性的每個刪除行為會發生什麼情況。</span><span class="sxs-lookup"><span data-stu-id="a05a7-173">The sample shows what happens for each delete behavior for both optional and required relationships when a parent entity is deleted.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/CascadeDelete/Sample.cs#DeleteBehaviorVariations)]

<span data-ttu-id="a05a7-174">讓我們逐步進行每個變化來了解會發生什麼情況。</span><span class="sxs-lookup"><span data-stu-id="a05a7-174">Let's walk through each variation to understand what is happening.</span></span>

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a><span data-ttu-id="a05a7-175">與必要或選擇性關聯性搭配的 DeleteBehavior.Cascade</span><span class="sxs-lookup"><span data-stu-id="a05a7-175">DeleteBehavior.Cascade with required or optional relationship</span></span>

```console
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1

  After SaveChanges:
    Blog '1' is in state Detached with 2 posts referenced.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
      Post '2' is in state Detached with FK '1' and no reference to a blog.
```

* <span data-ttu-id="a05a7-176">部落格會標示為 Deleted</span><span class="sxs-lookup"><span data-stu-id="a05a7-176">Blog is marked as Deleted</span></span>
* <span data-ttu-id="a05a7-177">文章一開始會維持 Unchanged，因為串聯會等到執行 SaveChanges 時才發生</span><span class="sxs-lookup"><span data-stu-id="a05a7-177">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="a05a7-178">SaveChanges 會先針對相依項/子系 (文章) 再針對主體/父系 (部落格) 傳送刪除</span><span class="sxs-lookup"><span data-stu-id="a05a7-178">SaveChanges sends deletes for both dependents/children (posts) and then the principal/parent (blog)</span></span>
* <span data-ttu-id="a05a7-179">儲存之後，會將所有實體都中斷連結，因為現在已將它們從資料庫中刪除</span><span class="sxs-lookup"><span data-stu-id="a05a7-179">After saving, all entities are detached since they have now been deleted from the database</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a><span data-ttu-id="a05a7-180">與必要關聯性搭配的 DeleteBehavior.ClientSetNull 或 DeleteBehavior.SetNull</span><span class="sxs-lookup"><span data-stu-id="a05a7-180">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with required relationship</span></span>

``` output
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1

  SaveChanges threw DbUpdateException: Cannot insert the value NULL into column 'BlogId', table 'EFSaving.CascadeDelete.dbo.Posts'; column does not allow nulls. UPDATE fails. The statement has been terminated.
```

* <span data-ttu-id="a05a7-181">部落格會標示為 Deleted</span><span class="sxs-lookup"><span data-stu-id="a05a7-181">Blog is marked as Deleted</span></span>
* <span data-ttu-id="a05a7-182">文章一開始會維持 Unchanged，因為串聯會等到執行 SaveChanges 時才發生</span><span class="sxs-lookup"><span data-stu-id="a05a7-182">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="a05a7-183">SaveChanges 會嘗試將文章 FK 設定為 Null，但這會失敗，因為 FK 不可為空值</span><span class="sxs-lookup"><span data-stu-id="a05a7-183">SaveChanges attempts to set the post FK to null, but this fails because the FK is not nullable</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a><span data-ttu-id="a05a7-184">與選擇性關聯性搭配的 DeleteBehavior.ClientSetNull 或 DeleteBehavior.SetNull</span><span class="sxs-lookup"><span data-stu-id="a05a7-184">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with optional relationship</span></span>

``` output
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1

  After SaveChanges:
    Blog '1' is in state Detached with 2 posts referenced.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
      Post '2' is in state Unchanged with FK 'null' and no reference to a blog.
```

* <span data-ttu-id="a05a7-185">部落格會標示為 Deleted</span><span class="sxs-lookup"><span data-stu-id="a05a7-185">Blog is marked as Deleted</span></span>
* <span data-ttu-id="a05a7-186">文章一開始會維持 Unchanged，因為串聯會等到執行 SaveChanges 時才發生</span><span class="sxs-lookup"><span data-stu-id="a05a7-186">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="a05a7-187">SaveChanges 會先嘗試將相依項/子系 (文章) 的 FK 都設定為 Null，再刪除主體/父系 (部落格)</span><span class="sxs-lookup"><span data-stu-id="a05a7-187">SaveChanges attempts sets the FK of both dependents/children (posts) to null before deleting the principal/parent (blog)</span></span>
* <span data-ttu-id="a05a7-188">儲存之後，會刪除主體/父系 (部落格)，但仍然會追蹤相依項/子系 (文章)</span><span class="sxs-lookup"><span data-stu-id="a05a7-188">After saving, the principal/parent (blog) is deleted, but the dependents/children (posts) are still tracked</span></span>
* <span data-ttu-id="a05a7-189">所追蹤相依項/子系 (文章) 的 FK 值現在會是 Null，且系統已移除它們對已刪除主體/父系 (部落格) 的參照</span><span class="sxs-lookup"><span data-stu-id="a05a7-189">The tracked dependents/children (posts) now have null FK values and their reference to the deleted principal/parent (blog) has been removed</span></span>

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a><span data-ttu-id="a05a7-190">與必要或選擇性關聯性搭配的 DeleteBehavior.Restrict</span><span class="sxs-lookup"><span data-stu-id="a05a7-190">DeleteBehavior.Restrict with required or optional relationship</span></span>

``` output
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
  SaveChanges threw InvalidOperationException: The association between entity types 'Blog' and 'Post' has been severed but the foreign key for this relationship cannot be set to null. If the dependent entity should be deleted, then setup the relationship to use cascade deletes.
```

* <span data-ttu-id="a05a7-191">部落格會標示為 Deleted</span><span class="sxs-lookup"><span data-stu-id="a05a7-191">Blog is marked as Deleted</span></span>
* <span data-ttu-id="a05a7-192">文章一開始會維持 Unchanged，因為串聯會等到執行 SaveChanges 時才發生</span><span class="sxs-lookup"><span data-stu-id="a05a7-192">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="a05a7-193">由於 *Restrict* 會告知 EF 不要自動將 FK 設定為 Null，因此它會保持非 Null，而 SaveChanges 則會擲回例外狀況而不進行儲存</span><span class="sxs-lookup"><span data-stu-id="a05a7-193">Since *Restrict* tells EF to not automatically set the FK to null, it remains non-null and SaveChanges throws without saving</span></span>

## <a name="delete-orphans-examples"></a><span data-ttu-id="a05a7-194">刪除失去關聯的項目範例</span><span class="sxs-lookup"><span data-stu-id="a05a7-194">Delete orphans examples</span></span>

<span data-ttu-id="a05a7-195">對於可供下載並執行的[範例](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/CascadeDelete/)，以下程式碼是其中的一部分。</span><span class="sxs-lookup"><span data-stu-id="a05a7-195">The code below is part of a [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/CascadeDelete/) that can be downloaded and run.</span></span> <span data-ttu-id="a05a7-196">此範例示範切斷父系/主體與其子系/相依項之間的關聯性時，選擇性和必要關聯性的每個刪除行為會發生什麼情況。</span><span class="sxs-lookup"><span data-stu-id="a05a7-196">The sample shows what happens for each delete behavior for both optional and required relationships when the relationship between a parent/principal and its children/dependents is severed.</span></span> <span data-ttu-id="a05a7-197">在此範例中，會藉由從主體/父系 (部落格) 上的集合導覽屬性移除相依項/子系 (文章) 來切斷關聯性。</span><span class="sxs-lookup"><span data-stu-id="a05a7-197">In this example, the relationship is severed by removing the dependents/children (posts) from the collection navigation property on the principal/parent (blog).</span></span> <span data-ttu-id="a05a7-198">不過，如果改為將相依項/子系對主體/父系的參考設定為 Null，行為也會相同。</span><span class="sxs-lookup"><span data-stu-id="a05a7-198">However, the behavior is the same if the reference from dependent/child to principal/parent is instead nulled out.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/CascadeDelete/Sample.cs#DeleteOrphansVariations)]

<span data-ttu-id="a05a7-199">讓我們逐步進行每個變化來了解會發生什麼情況。</span><span class="sxs-lookup"><span data-stu-id="a05a7-199">Let's walk through each variation to understand what is happening.</span></span>

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a><span data-ttu-id="a05a7-200">與必要或選擇性關聯性搭配的 DeleteBehavior.Cascade</span><span class="sxs-lookup"><span data-stu-id="a05a7-200">DeleteBehavior.Cascade with required or optional relationship</span></span>

``` output
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK '1' and no reference to a blog.
      Post '2' is in state Modified with FK '1' and no reference to a blog.

  Saving changes:
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2

  After SaveChanges:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
      Post '2' is in state Detached with FK '1' and no reference to a blog.
```

* <span data-ttu-id="a05a7-201">文章會標示為 Modified，因為切斷關聯性造成將 FK 標示為 Null</span><span class="sxs-lookup"><span data-stu-id="a05a7-201">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="a05a7-202">如果 FK 不可為 Null，則即使將其標示為 Null，實際值也不會變更</span><span class="sxs-lookup"><span data-stu-id="a05a7-202">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="a05a7-203">SaveChanges 會針對相依項/子系 (文章) 傳送刪除</span><span class="sxs-lookup"><span data-stu-id="a05a7-203">SaveChanges sends deletes for dependents/children (posts)</span></span>
* <span data-ttu-id="a05a7-204">儲存之後，會將相依項/子系 (文章) 中斷連結，因為現在已將它們從資料庫中刪除</span><span class="sxs-lookup"><span data-stu-id="a05a7-204">After saving, the dependents/children (posts) are detached since they have now been deleted from the database</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a><span data-ttu-id="a05a7-205">與必要關聯性搭配的 DeleteBehavior.ClientSetNull 或 DeleteBehavior.SetNull</span><span class="sxs-lookup"><span data-stu-id="a05a7-205">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with required relationship</span></span>

``` output
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.
      Post '2' is in state Modified with FK 'null' and no reference to a blog.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1

  SaveChanges threw DbUpdateException: Cannot insert the value NULL into column 'BlogId', table 'EFSaving.CascadeDelete.dbo.Posts'; column does not allow nulls. UPDATE fails. The statement has been terminated.
```

* <span data-ttu-id="a05a7-206">文章會標示為 Modified，因為切斷關聯性造成將 FK 標示為 Null</span><span class="sxs-lookup"><span data-stu-id="a05a7-206">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="a05a7-207">如果 FK 不可為 Null，則即使將其標示為 Null，實際值也不會變更</span><span class="sxs-lookup"><span data-stu-id="a05a7-207">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="a05a7-208">SaveChanges 會嘗試將文章 FK 設定為 Null，但這會失敗，因為 FK 不可為空值</span><span class="sxs-lookup"><span data-stu-id="a05a7-208">SaveChanges attempts to set the post FK to null, but this fails because the FK is not nullable</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a><span data-ttu-id="a05a7-209">與選擇性關聯性搭配的 DeleteBehavior.ClientSetNull 或 DeleteBehavior.SetNull</span><span class="sxs-lookup"><span data-stu-id="a05a7-209">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with optional relationship</span></span>

``` output
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.
      Post '2' is in state Modified with FK 'null' and no reference to a blog.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 2

  After SaveChanges:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
      Post '2' is in state Unchanged with FK 'null' and no reference to a blog.
```

* <span data-ttu-id="a05a7-210">文章會標示為 Modified，因為切斷關聯性造成將 FK 標示為 Null</span><span class="sxs-lookup"><span data-stu-id="a05a7-210">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="a05a7-211">如果 FK 不可為 Null，則即使將其標示為 Null，實際值也不會變更</span><span class="sxs-lookup"><span data-stu-id="a05a7-211">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="a05a7-212">SaveChanges 會將相依項/子系 (文章) 的 FK 都設定為 Null</span><span class="sxs-lookup"><span data-stu-id="a05a7-212">SaveChanges sets the FK of both dependents/children (posts) to null</span></span>
* <span data-ttu-id="a05a7-213">儲存之後，相依項/子系 (文章) 的 FK 值現在會是 Null，且系統已移除它們對已刪除之主體/父系 (部落格) 的參照</span><span class="sxs-lookup"><span data-stu-id="a05a7-213">After saving, the dependents/children (posts) now have null FK values and their reference to the deleted principal/parent (blog) has been removed</span></span>

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a><span data-ttu-id="a05a7-214">與必要或選擇性關聯性搭配的 DeleteBehavior.Restrict</span><span class="sxs-lookup"><span data-stu-id="a05a7-214">DeleteBehavior.Restrict with required or optional relationship</span></span>

``` output
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK '1' and no reference to a blog.
      Post '2' is in state Modified with FK '1' and no reference to a blog.

  Saving changes:
  SaveChanges threw InvalidOperationException: The association between entity types 'Blog' and 'Post' has been severed but the foreign key for this relationship cannot be set to null. If the dependent entity should be deleted, then setup the relationship to use cascade deletes.
```

* <span data-ttu-id="a05a7-215">文章會標示為 Modified，因為切斷關聯性造成將 FK 標示為 Null</span><span class="sxs-lookup"><span data-stu-id="a05a7-215">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="a05a7-216">如果 FK 不可為 Null，則即使將其標示為 Null，實際值也不會變更</span><span class="sxs-lookup"><span data-stu-id="a05a7-216">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="a05a7-217">由於 *Restrict* 會告知 EF 不要自動將 FK 設定為 Null，因此它會保持非 Null，而 SaveChanges 則會擲回例外狀況而不進行儲存</span><span class="sxs-lookup"><span data-stu-id="a05a7-217">Since *Restrict* tells EF to not automatically set the FK to null, it remains non-null and SaveChanges throws without saving</span></span>

## <a name="cascading-to-untracked-entities"></a><span data-ttu-id="a05a7-218">串聯至未追蹤的實體</span><span class="sxs-lookup"><span data-stu-id="a05a7-218">Cascading to untracked entities</span></span>

<span data-ttu-id="a05a7-219">當您呼叫 *SaveChanges* 時，串聯刪除規則將會套用至內容所追蹤的所有實體。</span><span class="sxs-lookup"><span data-stu-id="a05a7-219">When you call *SaveChanges*, the cascade delete rules will be applied to any entities that are being tracked by the context.</span></span> <span data-ttu-id="a05a7-220">這是上述所有範例中發生的情況，也是為何產生 SQL 來同時刪除主體/父系 (部落格) 及所有相依項/子系 (文章) 的原因：</span><span class="sxs-lookup"><span data-stu-id="a05a7-220">This is the situation in all the examples shown above, which is why SQL was generated to delete both the principal/parent (blog) and all the dependents/children (posts):</span></span>

```sql
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

<span data-ttu-id="a05a7-221">如果只載入主體 (例如對部落格進行查詢時，未使用可一併包含文章的 `Include(b => b.Posts)`)，則 SaveChanges 將只產生 SQL 來刪除主體/父系：</span><span class="sxs-lookup"><span data-stu-id="a05a7-221">If only the principal is loaded--for example, when a query is made for a blog without an `Include(b => b.Posts)` to also include posts--then SaveChanges will only generate SQL to delete the principal/parent:</span></span>

```sql
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

<span data-ttu-id="a05a7-222">只有已在資料庫設定對應的串聯行為時，才會刪除相依項/子系 (文章)。</span><span class="sxs-lookup"><span data-stu-id="a05a7-222">The dependents/children (posts) will only be deleted if the database has a corresponding cascade behavior configured.</span></span> <span data-ttu-id="a05a7-223">如果您使用 EF 來建立資料庫，將會為您設定此串聯行為。</span><span class="sxs-lookup"><span data-stu-id="a05a7-223">If you use EF to create the database, this cascade behavior will be setup for you.</span></span>
