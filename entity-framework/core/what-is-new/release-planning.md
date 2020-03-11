---
title: EF Core 版本規劃
author: ajcvickers
ms.date: 01/28/2020
uid: core/what-is-new/release_planning.md
ms.openlocfilehash: 71045b8d49c319a73f74443612bedd84ee33ab8a
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417333"
---
# <a name="release-planning-process"></a><span data-ttu-id="0b1a7-102">發行計劃程序</span><span class="sxs-lookup"><span data-stu-id="0b1a7-102">Release planning process</span></span>

<span data-ttu-id="0b1a7-103">在我們收到的問題中，經常有人問起我們如何決定將某功能加入特定的版本。</span><span class="sxs-lookup"><span data-stu-id="0b1a7-103">We often get questions about how we choose specific features to go into a particular release.</span></span>
<span data-ttu-id="0b1a7-104">本檔概述我們所使用的程式。</span><span class="sxs-lookup"><span data-stu-id="0b1a7-104">This document outlines the process we use.</span></span>
<span data-ttu-id="0b1a7-105">此程式會持續進化，因為我們找到更好的計畫方式，但一般的想法保持不變。</span><span class="sxs-lookup"><span data-stu-id="0b1a7-105">The process is continually evolving as we find better ways to plan, but the general ideas remain the same.</span></span>

## <a name="different-kinds-of-releases"></a><span data-ttu-id="0b1a7-106">不同種類的版本</span><span class="sxs-lookup"><span data-stu-id="0b1a7-106">Different kinds of releases</span></span>

<span data-ttu-id="0b1a7-107">不同類型的版本包含不同類型的變更。</span><span class="sxs-lookup"><span data-stu-id="0b1a7-107">Different kinds of release contain different kinds of changes.</span></span>
<span data-ttu-id="0b1a7-108">這也表示不同版本的發行計畫會有所不同。</span><span class="sxs-lookup"><span data-stu-id="0b1a7-108">This in turn means that the release planning is different for different kinds of release.</span></span>

### <a name="patch-releases"></a><span data-ttu-id="0b1a7-109">修補程式版本</span><span class="sxs-lookup"><span data-stu-id="0b1a7-109">Patch releases</span></span>

<span data-ttu-id="0b1a7-110">修補程式版本只會變更版本的「修補」部分。</span><span class="sxs-lookup"><span data-stu-id="0b1a7-110">Patch releases change only the "patch" part of the version.</span></span>
<span data-ttu-id="0b1a7-111">例如，EF Core 3.1。**1**是在 EF Core 3.1 中找到修補問題的發行版本。**0**。</span><span class="sxs-lookup"><span data-stu-id="0b1a7-111">For example, EF Core 3.1.**1** is a release that patches issues found in EF Core 3.1.**0**.</span></span>

<span data-ttu-id="0b1a7-112">修補程式版本主要是用來修正重要的客戶錯誤。</span><span class="sxs-lookup"><span data-stu-id="0b1a7-112">Patch releases are intended to fix critical customer bugs.</span></span>
<span data-ttu-id="0b1a7-113">這表示修補程式版本不包含新功能。</span><span class="sxs-lookup"><span data-stu-id="0b1a7-113">This means patch releases do not include new features.</span></span>
<span data-ttu-id="0b1a7-114">除了特殊情況以外，修補程式版本中不允許 API 變更。</span><span class="sxs-lookup"><span data-stu-id="0b1a7-114">API changes are not allowed in patch releases except in special circumstances.</span></span>

<span data-ttu-id="0b1a7-115">在修補程式版本中進行變更的橫條非常高。</span><span class="sxs-lookup"><span data-stu-id="0b1a7-115">The bar to make a change in a patch release is very high.</span></span>
<span data-ttu-id="0b1a7-116">這是因為修補程式發行不會引進新的 bug。</span><span class="sxs-lookup"><span data-stu-id="0b1a7-116">This is because it is critical that patch releases do not introduce new bugs.</span></span>
<span data-ttu-id="0b1a7-117">因此，決策流程強調高價值和低風險。</span><span class="sxs-lookup"><span data-stu-id="0b1a7-117">Therefore, the decision process emphasizes high value and low risk.</span></span>

<span data-ttu-id="0b1a7-118">在下列情況中，我們較可能會修補問題：</span><span class="sxs-lookup"><span data-stu-id="0b1a7-118">We are more likely to patch an issue if:</span></span>
  * <span data-ttu-id="0b1a7-119">它會影響多個客戶</span><span class="sxs-lookup"><span data-stu-id="0b1a7-119">It is impacting multiple customers</span></span>
  * <span data-ttu-id="0b1a7-120">這是來自前一版的回歸</span><span class="sxs-lookup"><span data-stu-id="0b1a7-120">It is a regression from a previous release</span></span>
  * <span data-ttu-id="0b1a7-121">失敗會導致資料損毀</span><span class="sxs-lookup"><span data-stu-id="0b1a7-121">The failure causes data corruption</span></span>

<span data-ttu-id="0b1a7-122">在下列情況中，我們較不可能修補問題：</span><span class="sxs-lookup"><span data-stu-id="0b1a7-122">We are less likely to patch an issue if:</span></span>
  * <span data-ttu-id="0b1a7-123">有合理的因應措施</span><span class="sxs-lookup"><span data-stu-id="0b1a7-123">There are reasonable workarounds</span></span>
  * <span data-ttu-id="0b1a7-124">這項修正有高風險，可能會中斷其他工作</span><span class="sxs-lookup"><span data-stu-id="0b1a7-124">The fix has high risk of breaking something else</span></span>
  * <span data-ttu-id="0b1a7-125">Bug 是在角落案例中</span><span class="sxs-lookup"><span data-stu-id="0b1a7-125">The bug is in a corner-case</span></span>

<span data-ttu-id="0b1a7-126">此橫條會在[長期支援（LTS）](https://dotnet.microsoft.com/platform/support/policy/dotnet-core)版本的存留期間逐漸上升。</span><span class="sxs-lookup"><span data-stu-id="0b1a7-126">This bar gradually rises through the lifetime of a [long-term support (LTS)](https://dotnet.microsoft.com/platform/support/policy/dotnet-core) release.</span></span> <span data-ttu-id="0b1a7-127">這是因為 LTS 版本強調了穩定性。</span><span class="sxs-lookup"><span data-stu-id="0b1a7-127">This is because LTS releases emphasize stability.</span></span>

<span data-ttu-id="0b1a7-128">關於修補問題的最後一項決定是由 Microsoft 的 .NET 主管所做出。</span><span class="sxs-lookup"><span data-stu-id="0b1a7-128">The final decision about whether or not to patch an issue is made by the .NET Directors at Microsoft.</span></span>

### <a name="minor-releases"></a><span data-ttu-id="0b1a7-129">次要版本</span><span class="sxs-lookup"><span data-stu-id="0b1a7-129">Minor releases</span></span>

<span data-ttu-id="0b1a7-130">次要版本只會變更版本的「次要」部分。</span><span class="sxs-lookup"><span data-stu-id="0b1a7-130">Minor releases change only the "minor" part of the version.</span></span>
<span data-ttu-id="0b1a7-131">例如，EF Core 3。**1**.0 是改善 EF Core 3 的版本。**0**. 0。</span><span class="sxs-lookup"><span data-stu-id="0b1a7-131">For example, EF Core 3.**1**.0 is a release that improves on EF Core 3.**0**.0.</span></span>

<span data-ttu-id="0b1a7-132">次要版本：</span><span class="sxs-lookup"><span data-stu-id="0b1a7-132">Minor releases:</span></span>
* <span data-ttu-id="0b1a7-133">旨在改善舊版的品質和功能</span><span class="sxs-lookup"><span data-stu-id="0b1a7-133">Are intended to improve the quality and features of the previous release</span></span>
* <span data-ttu-id="0b1a7-134">通常包含 bug 修正和新功能</span><span class="sxs-lookup"><span data-stu-id="0b1a7-134">Typically contain bug fixes and new features</span></span>
* <span data-ttu-id="0b1a7-135">不包含刻意的重大變更</span><span class="sxs-lookup"><span data-stu-id="0b1a7-135">Do not include intentional breaking changes</span></span>
* <span data-ttu-id="0b1a7-136">有幾個發行前版本預覽已推送至 NuGet</span><span class="sxs-lookup"><span data-stu-id="0b1a7-136">Have a few prerelease previews pushed to NuGet</span></span>

### <a name="major-releases"></a><span data-ttu-id="0b1a7-137">主要版本</span><span class="sxs-lookup"><span data-stu-id="0b1a7-137">Major releases</span></span>

<span data-ttu-id="0b1a7-138">主要版本會變更 EF 「主要」版本號碼。</span><span class="sxs-lookup"><span data-stu-id="0b1a7-138">Major releases change the EF "major" version number.</span></span>
<span data-ttu-id="0b1a7-139">例如，EF Core **3**. 0.0 是一種主要版本，可在 EF Core 2.2. x 之前進行一大進步。</span><span class="sxs-lookup"><span data-stu-id="0b1a7-139">For example, EF Core **3**.0.0 is a major release that makes a big step forward over EF Core 2.2.x.</span></span>

<span data-ttu-id="0b1a7-140">主要版本：</span><span class="sxs-lookup"><span data-stu-id="0b1a7-140">Major releases:</span></span>
* <span data-ttu-id="0b1a7-141">旨在改善舊版的品質和功能</span><span class="sxs-lookup"><span data-stu-id="0b1a7-141">Are intended to improve the quality and features of the previous release</span></span>
* <span data-ttu-id="0b1a7-142">通常包含 bug 修正和新功能</span><span class="sxs-lookup"><span data-stu-id="0b1a7-142">Typically contain bug fixes and new features</span></span>
  * <span data-ttu-id="0b1a7-143">部分新功能可能是 EF Core 運作方式的基本變更</span><span class="sxs-lookup"><span data-stu-id="0b1a7-143">Some of the new features may be fundamental changes to the way EF Core works</span></span>
* <span data-ttu-id="0b1a7-144">通常包含刻意的重大變更</span><span class="sxs-lookup"><span data-stu-id="0b1a7-144">Typically include intentional breaking changes</span></span>
  * <span data-ttu-id="0b1a7-145">中斷性變更是我們所學習到不斷演進 EF Core 的必要部分</span><span class="sxs-lookup"><span data-stu-id="0b1a7-145">Breaking changes are necessary part of evolving EF Core as we learn</span></span>
  * <span data-ttu-id="0b1a7-146">不過，我們認為非常謹慎地進行任何重大變更，因為潛在客戶的影響。</span><span class="sxs-lookup"><span data-stu-id="0b1a7-146">However, we think very carefully about making any breaking change because of the potential customer impact.</span></span> <span data-ttu-id="0b1a7-147">過去的重大變更，我們可能過於積極。</span><span class="sxs-lookup"><span data-stu-id="0b1a7-147">We may have been too aggressive with breaking changes in the past.</span></span> <span data-ttu-id="0b1a7-148">接下來，我們將努力盡可能減少中斷應用程式的變更，並減少中斷資料庫提供者和擴充功能的變更。</span><span class="sxs-lookup"><span data-stu-id="0b1a7-148">Going forward, we will strive to minimize changes that break applications, and to reduce changes that break database providers and extensions.</span></span>
* <span data-ttu-id="0b1a7-149">有許多發行前版本預覽已推送至 NuGet</span><span class="sxs-lookup"><span data-stu-id="0b1a7-149">Have many prerelease previews pushed to NuGet</span></span>

## <a name="planning-for-majorminor-releases"></a><span data-ttu-id="0b1a7-150">主要/次要版本規劃</span><span class="sxs-lookup"><span data-stu-id="0b1a7-150">Planning for major/minor releases</span></span>

### <a name="github-issue-tracking"></a><span data-ttu-id="0b1a7-151">GitHub 問題追蹤</span><span class="sxs-lookup"><span data-stu-id="0b1a7-151">GitHub issue tracking</span></span>

<span data-ttu-id="0b1a7-152">GitHub （[https://github.com/dotnet/efcore](https://github.com/dotnet/efcore)）是所有 EF Core 規劃的真實來源。</span><span class="sxs-lookup"><span data-stu-id="0b1a7-152">GitHub ([https://github.com/dotnet/efcore](https://github.com/dotnet/efcore)) is the source-of-truth for all EF Core planning.</span></span>

<span data-ttu-id="0b1a7-153">GitHub 上的問題有：</span><span class="sxs-lookup"><span data-stu-id="0b1a7-153">Issues on GitHub have:</span></span>

* <span data-ttu-id="0b1a7-154">狀態</span><span class="sxs-lookup"><span data-stu-id="0b1a7-154">A state</span></span>
  * <span data-ttu-id="0b1a7-155">尚未解決[開啟](https://github.com/dotnet/efcore/issues)的問題。</span><span class="sxs-lookup"><span data-stu-id="0b1a7-155">[Open](https://github.com/dotnet/efcore/issues) issues have not been addressed.</span></span>
  * <span data-ttu-id="0b1a7-156">已解決[關閉](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aclosed)的問題。</span><span class="sxs-lookup"><span data-stu-id="0b1a7-156">[Closed](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aclosed) issues have been addressed.</span></span>
    * <span data-ttu-id="0b1a7-157">已修正的所有問題都會[以封閉式-fixed 標記](https://github.com/dotnet/efcore/issues?q=is%3Aissue+label%3Aclosed-fixed+is%3Aclosed)。</span><span class="sxs-lookup"><span data-stu-id="0b1a7-157">All issues that have been fixed are [tagged with closed-fixed](https://github.com/dotnet/efcore/issues?q=is%3Aissue+label%3Aclosed-fixed+is%3Aclosed).</span></span> <span data-ttu-id="0b1a7-158">已修正併合並標記為封閉式的問題，但可能尚未發行。</span><span class="sxs-lookup"><span data-stu-id="0b1a7-158">An issue tagged with closed-fixed is fixed and merged, but may not have been released.</span></span>
    * <span data-ttu-id="0b1a7-159">其他 `closed-` 標籤會指出關閉問題的其他原因。</span><span class="sxs-lookup"><span data-stu-id="0b1a7-159">Other `closed-` labels indicate other reasons for closing an issue.</span></span> <span data-ttu-id="0b1a7-160">例如，重複專案會以已關閉的重複標記。</span><span class="sxs-lookup"><span data-stu-id="0b1a7-160">For example, duplicates are tagged with closed-duplicate.</span></span>
* <span data-ttu-id="0b1a7-161">類型</span><span class="sxs-lookup"><span data-stu-id="0b1a7-161">A type</span></span>
  * <span data-ttu-id="0b1a7-162">[Bug](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+label%3Atype-bug)代表 bug。</span><span class="sxs-lookup"><span data-stu-id="0b1a7-162">[Bugs](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+label%3Atype-bug) represent bugs.</span></span>
  * <span data-ttu-id="0b1a7-163">[增強](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+label%3Atype-enhancement)功能代表現有功能中的新功能或較佳的功能。</span><span class="sxs-lookup"><span data-stu-id="0b1a7-163">[Enhancements](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+label%3Atype-enhancement) represent new features or better functionality in existing features.</span></span>
* <span data-ttu-id="0b1a7-164">里程碑</span><span class="sxs-lookup"><span data-stu-id="0b1a7-164">A milestone</span></span>
  * <span data-ttu-id="0b1a7-165">小組不會考慮[任何里程碑的問題](https://github.com/dotnet/efcore/issues?q=is%3Aopen+is%3Aissue+no%3Amilestone)。</span><span class="sxs-lookup"><span data-stu-id="0b1a7-165">[Issues with no milestone](https://github.com/dotnet/efcore/issues?q=is%3Aopen+is%3Aissue+no%3Amilestone) are being considered by the team.</span></span> <span data-ttu-id="0b1a7-166">尚未進行此問題的決定，或考慮的變更也不是如此。</span><span class="sxs-lookup"><span data-stu-id="0b1a7-166">The decision on what to do with the issue has not yet been made or a change in the decision is being considered.</span></span>
  * <span data-ttu-id="0b1a7-167">待處理專案（ [Backlog）里程碑中的問題](https://github.com/dotnet/efcore/issues?q=is%3Aopen+is%3Aissue+milestone%3ABacklog)是 EF 小組在未來的版本中將考慮的專案</span><span class="sxs-lookup"><span data-stu-id="0b1a7-167">[Issues in the Backlog milestone](https://github.com/dotnet/efcore/issues?q=is%3Aopen+is%3Aissue+milestone%3ABacklog) are items that the EF team will consider working on in a future release</span></span>
    * <span data-ttu-id="0b1a7-168">待處理專案中的問題可能會標記為 [[考慮-下一版](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+label%3Aconsider-for-next-release)]，表示下一版的清單中此工作專案很高。</span><span class="sxs-lookup"><span data-stu-id="0b1a7-168">Issues in the backlog may be [tagged with consider-for-next-release](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+label%3Aconsider-for-next-release) indicating that this work item is high on the list for the next release.</span></span>
  * <span data-ttu-id="0b1a7-169">版本化里程碑中的開啟問題是小組計畫在該版本中處理的專案。</span><span class="sxs-lookup"><span data-stu-id="0b1a7-169">Open issues in a versioned milestone are items that the team plans to work on in that version.</span></span> <span data-ttu-id="0b1a7-170">例如，[這些是我們打算針對 EF Core 5.0 進行處理的問題](https://github.com/dotnet/efcore/issues?q=is%3Aopen+is%3Aissue+milestone%3A5.0.0)。</span><span class="sxs-lookup"><span data-stu-id="0b1a7-170">For example, [these are the issues we plan to work on for EF Core 5.0](https://github.com/dotnet/efcore/issues?q=is%3Aopen+is%3Aissue+milestone%3A5.0.0).</span></span>
  * <span data-ttu-id="0b1a7-171">已建立版本之里程碑中的已關閉問題是針對該版本完成的問題。</span><span class="sxs-lookup"><span data-stu-id="0b1a7-171">Closed issues in a versioned milestone are issues that are completed for that version.</span></span> <span data-ttu-id="0b1a7-172">請注意，版本可能尚未發行。</span><span class="sxs-lookup"><span data-stu-id="0b1a7-172">Note that the version may not yet have been released.</span></span> <span data-ttu-id="0b1a7-173">例如，[這些是針對 EF Core 3.0 而完成的問題](https://github.com/dotnet/efcore/issues?q=is%3Aissue+milestone%3A3.0.0+is%3Aclosed)。</span><span class="sxs-lookup"><span data-stu-id="0b1a7-173">For example, [these are the issues completed for EF Core 3.0](https://github.com/dotnet/efcore/issues?q=is%3Aissue+milestone%3A3.0.0+is%3Aclosed).</span></span>
* <span data-ttu-id="0b1a7-174">投票!</span><span class="sxs-lookup"><span data-stu-id="0b1a7-174">Votes!</span></span>
  * <span data-ttu-id="0b1a7-175">投票是指出問題對您很重要的最佳方式。</span><span class="sxs-lookup"><span data-stu-id="0b1a7-175">Voting is the best way to indicate that an issue is important to you.</span></span>
  * <span data-ttu-id="0b1a7-176">若要投票，只要在問題中加入「大拇指」 👍 即可。</span><span class="sxs-lookup"><span data-stu-id="0b1a7-176">To vote, just add a "thumbs-up" 👍 to the issue.</span></span> <span data-ttu-id="0b1a7-177">例如，[這些是最常投票的問題](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+sort%3Areactions-%2B1-desc)</span><span class="sxs-lookup"><span data-stu-id="0b1a7-177">For example, [these are the top-voted issues](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+sort%3Areactions-%2B1-desc)</span></span>
  * <span data-ttu-id="0b1a7-178">如果您覺得這會增加價值，請也將批註描述您需要此功能的特定原因。</span><span class="sxs-lookup"><span data-stu-id="0b1a7-178">Please also comment describing specific reasons you need the feature if you feel this adds value.</span></span> <span data-ttu-id="0b1a7-179">批註 "+ 1" 或類似的不會增加值。</span><span class="sxs-lookup"><span data-stu-id="0b1a7-179">Commenting "+1" or similar does not add value.</span></span>

### <a name="the-planning-process"></a><span data-ttu-id="0b1a7-180">規劃程序</span><span class="sxs-lookup"><span data-stu-id="0b1a7-180">The planning process</span></span>

<span data-ttu-id="0b1a7-181">規劃程式比只從待處理專案取得最上層要求的功能更多。</span><span class="sxs-lookup"><span data-stu-id="0b1a7-181">The planning process is more involved than just taking the top-most requested features from the backlog.</span></span>
<span data-ttu-id="0b1a7-182">這是因為我們會以多種方式收集來自多個專案關係人的意見反應。</span><span class="sxs-lookup"><span data-stu-id="0b1a7-182">This is because we gather feedback from multiple stakeholders in multiple ways.</span></span>
<span data-ttu-id="0b1a7-183">然後，我們會根據下列內容來塑造發行：</span><span class="sxs-lookup"><span data-stu-id="0b1a7-183">We then shape a release based on:</span></span>

* <span data-ttu-id="0b1a7-184">客戶的輸入</span><span class="sxs-lookup"><span data-stu-id="0b1a7-184">Input from customers</span></span>
* <span data-ttu-id="0b1a7-185">來自其他專案關係人的輸入</span><span class="sxs-lookup"><span data-stu-id="0b1a7-185">Input from other stakeholders</span></span>
* <span data-ttu-id="0b1a7-186">策略性方向</span><span class="sxs-lookup"><span data-stu-id="0b1a7-186">Strategic direction</span></span>
* <span data-ttu-id="0b1a7-187">可用的資源</span><span class="sxs-lookup"><span data-stu-id="0b1a7-187">Resources available</span></span>
* <span data-ttu-id="0b1a7-188">排程</span><span class="sxs-lookup"><span data-stu-id="0b1a7-188">Schedule</span></span>

<span data-ttu-id="0b1a7-189">我們提出的一些問題是：</span><span class="sxs-lookup"><span data-stu-id="0b1a7-189">Some of the questions we ask are:</span></span>

1. <span data-ttu-id="0b1a7-190">**我們認為多少開發人員會使用此功能，而它讓他們的應用程式或體驗改善多少？**</span><span class="sxs-lookup"><span data-stu-id="0b1a7-190">**How many developers we think will use the feature and how much better will it make their applications or experience?**</span></span> <span data-ttu-id="0b1a7-191">為了回答這個問題，我們從許多來源收集意見反應 — 問題的留言與投票是那些來源之一。</span><span class="sxs-lookup"><span data-stu-id="0b1a7-191">To answer this question, we collect feedback from many sources — Comments and votes on issues is one of those sources.</span></span> <span data-ttu-id="0b1a7-192">具有重要客戶的特定合作是另一個。</span><span class="sxs-lookup"><span data-stu-id="0b1a7-192">Specific engagements with important customers is another.</span></span>

2. <span data-ttu-id="0b1a7-193">**如果我們尚未實作這項功能，那有哪些因應措施可用？**</span><span class="sxs-lookup"><span data-stu-id="0b1a7-193">**What are the workarounds people can use if we don't implement this feature yet?**</span></span> <span data-ttu-id="0b1a7-194">舉例來說，許多開發人員在缺少原生多對多支援時，都會想到運用聯合資料表作為因應措施。</span><span class="sxs-lookup"><span data-stu-id="0b1a7-194">For example, many developers can map a join table to work around lack of native many-to-many support.</span></span> <span data-ttu-id="0b1a7-195">很明顯地，並非所有開發人員都想這麼做，但其中許多人都做得到，而這也是我們決策中所考量的因素。</span><span class="sxs-lookup"><span data-stu-id="0b1a7-195">Obviously, not all developers want to do it, but many can, and that counts as a factor in our decision.</span></span>

3. <span data-ttu-id="0b1a7-196">**實作此功能是否會涉及 EF Core 的架構，使我們更加需要實作其他功能？**</span><span class="sxs-lookup"><span data-stu-id="0b1a7-196">**Does implementing this feature evolve the architecture of EF Core such that it moves us closer to implementing other features?**</span></span> <span data-ttu-id="0b1a7-197">我們偏好可作為其他功能建置組塊的功能。</span><span class="sxs-lookup"><span data-stu-id="0b1a7-197">We tend to favor features that act as building blocks for other features.</span></span> <span data-ttu-id="0b1a7-198">比方說，屬性包實體可以協助我們邁向多對多的支援，而實體建構函式則促成了消極式載入支援。</span><span class="sxs-lookup"><span data-stu-id="0b1a7-198">For instance, property bag entities can help us move towards many-to-many support, and entity constructors enabled our lazy loading support.</span></span>

4. <span data-ttu-id="0b1a7-199">**這項功能是擴充點嗎？**</span><span class="sxs-lookup"><span data-stu-id="0b1a7-199">**Is the feature an extensibility point?**</span></span> <span data-ttu-id="0b1a7-200">因為擴充點使開發人員能夠輕鬆地依自己的習慣使用，並填補任何功能上的缺失，所以我們偏好擴充點勝過於一般功能。</span><span class="sxs-lookup"><span data-stu-id="0b1a7-200">We tend to favor extensibility points over normal features because they enable developers to hook their own behaviors and compensate for any missing functionality.</span></span>

5. <span data-ttu-id="0b1a7-201">**此功能與其他產品搭配使用時的綜效如何？**</span><span class="sxs-lookup"><span data-stu-id="0b1a7-201">**What is the synergy of the feature when used in combination with other products?**</span></span> <span data-ttu-id="0b1a7-202">我們偏好能促成或大幅改善 EF Core 搭配其他產品使用體驗的功能，例如 .NET Core、最新版本的 Visual Studio、Microsoft Azure 等等。</span><span class="sxs-lookup"><span data-stu-id="0b1a7-202">We favor features that enable or significantly improve the experience of using EF Core with other products, such as .NET Core, the latest version of Visual Studio, Microsoft Azure, and so on.</span></span>

6. <span data-ttu-id="0b1a7-203">**有哪些人員可以用來處理某項功能的技能，以及如何充分利用這些資源？**</span><span class="sxs-lookup"><span data-stu-id="0b1a7-203">**What are the skills of the people available to work on a feature, and how can we best leverage these resources?**</span></span> <span data-ttu-id="0b1a7-204">每位 EF 小組的成員及我們社群的參與者，在不同領域都各有不同程度的經驗，所以我們必須據此進行規劃。</span><span class="sxs-lookup"><span data-stu-id="0b1a7-204">Each member of the EF team and our community contributors has different levels of experience in different areas, so we have to plan accordingly.</span></span> <span data-ttu-id="0b1a7-205">即使我們希望將「所有人力」集中在像是 GroupBy 翻譯，或多對多關聯性等特定功能上，也無法實際付諸行動。</span><span class="sxs-lookup"><span data-stu-id="0b1a7-205">Even if we wanted to have "all hands on deck" to work on a specific feature like GroupBy translations, or many-to-many, that wouldn't be practical.</span></span>
