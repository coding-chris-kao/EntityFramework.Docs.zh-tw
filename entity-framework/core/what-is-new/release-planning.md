---
title: EF Core 發行規劃
description: 如何完成 Entity Framework Core 規劃和發行的資訊
author: ajcvickers
ms.date: 01/28/2020
uid: core/what-is-new/release-planning
ms.openlocfilehash: f84b8cef40a74245575df6013d94fcda5738e229
ms.sourcegitcommit: f3512e3a98e685a3ba409c1d0157ce85cc390cf4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2020
ms.locfileid: "94429139"
---
# <a name="release-planning-process"></a><span data-ttu-id="9fc7b-103">發行計劃程序</span><span class="sxs-lookup"><span data-stu-id="9fc7b-103">Release planning process</span></span>

<span data-ttu-id="9fc7b-104">在我們收到的問題中，經常有人問起我們如何決定將某功能加入特定的版本。</span><span class="sxs-lookup"><span data-stu-id="9fc7b-104">We often get questions about how we choose specific features to go into a particular release.</span></span>
<span data-ttu-id="9fc7b-105">本檔概述我們所使用的程式。</span><span class="sxs-lookup"><span data-stu-id="9fc7b-105">This document outlines the process we use.</span></span>
<span data-ttu-id="9fc7b-106">當我們找到更好的計畫方式，但一般構想保持不變時，此程式會持續演進。</span><span class="sxs-lookup"><span data-stu-id="9fc7b-106">The process is continually evolving as we find better ways to plan, but the general ideas remain the same.</span></span>

## <a name="different-kinds-of-releases"></a><span data-ttu-id="9fc7b-107">不同種類的版本</span><span class="sxs-lookup"><span data-stu-id="9fc7b-107">Different kinds of releases</span></span>

<span data-ttu-id="9fc7b-108">不同種類的版本包含不同類型的變更。</span><span class="sxs-lookup"><span data-stu-id="9fc7b-108">Different kinds of release contain different kinds of changes.</span></span>
<span data-ttu-id="9fc7b-109">這表示不同版本的發行計畫會有所不同。</span><span class="sxs-lookup"><span data-stu-id="9fc7b-109">This in turn means that the release planning is different for different kinds of release.</span></span>

### <a name="patch-releases"></a><span data-ttu-id="9fc7b-110">修補程式版本</span><span class="sxs-lookup"><span data-stu-id="9fc7b-110">Patch releases</span></span>

<span data-ttu-id="9fc7b-111">修補程式版本只會變更版本的「修補程式」部分。</span><span class="sxs-lookup"><span data-stu-id="9fc7b-111">Patch releases change only the "patch" part of the version.</span></span>
<span data-ttu-id="9fc7b-112">例如，EF Core 3.1。 **1** 是在 EF Core 3.1 中找到修補程式問題的版本。 **0** 。</span><span class="sxs-lookup"><span data-stu-id="9fc7b-112">For example, EF Core 3.1. **1** is a release that patches issues found in EF Core 3.1. **0**.</span></span>

<span data-ttu-id="9fc7b-113">修補程式版本旨在修正重要的客戶錯誤。</span><span class="sxs-lookup"><span data-stu-id="9fc7b-113">Patch releases are intended to fix critical customer bugs.</span></span>
<span data-ttu-id="9fc7b-114">這表示修補程式版本不包含新功能。</span><span class="sxs-lookup"><span data-stu-id="9fc7b-114">This means patch releases do not include new features.</span></span>
<span data-ttu-id="9fc7b-115">除了在特殊情況下，修補程式版本中不允許 API 變更。</span><span class="sxs-lookup"><span data-stu-id="9fc7b-115">API changes are not allowed in patch releases except in special circumstances.</span></span>

<span data-ttu-id="9fc7b-116">在修補程式版本中進行變更的橫條很高。</span><span class="sxs-lookup"><span data-stu-id="9fc7b-116">The bar to make a change in a patch release is very high.</span></span>
<span data-ttu-id="9fc7b-117">這是因為修補程式版本不會引進新的 bug。</span><span class="sxs-lookup"><span data-stu-id="9fc7b-117">This is because it is critical that patch releases do not introduce new bugs.</span></span>
<span data-ttu-id="9fc7b-118">因此，決策流程強調高價值和低風險。</span><span class="sxs-lookup"><span data-stu-id="9fc7b-118">Therefore, the decision process emphasizes high value and low risk.</span></span>

<span data-ttu-id="9fc7b-119">如果發生下列情況，比較可能會修補問題：</span><span class="sxs-lookup"><span data-stu-id="9fc7b-119">We are more likely to patch an issue if:</span></span>

* <span data-ttu-id="9fc7b-120">它會影響多個客戶</span><span class="sxs-lookup"><span data-stu-id="9fc7b-120">It is impacting multiple customers</span></span>
* <span data-ttu-id="9fc7b-121">這是先前版本的回歸</span><span class="sxs-lookup"><span data-stu-id="9fc7b-121">It is a regression from a previous release</span></span>
* <span data-ttu-id="9fc7b-122">失敗會造成資料損毀</span><span class="sxs-lookup"><span data-stu-id="9fc7b-122">The failure causes data corruption</span></span>

<span data-ttu-id="9fc7b-123">如果有下列情況，則不太可能修補問題：</span><span class="sxs-lookup"><span data-stu-id="9fc7b-123">We are less likely to patch an issue if:</span></span>

* <span data-ttu-id="9fc7b-124">有合理的解決方法</span><span class="sxs-lookup"><span data-stu-id="9fc7b-124">There are reasonable workarounds</span></span>
* <span data-ttu-id="9fc7b-125">修正程式有高風險中斷其他</span><span class="sxs-lookup"><span data-stu-id="9fc7b-125">The fix has high risk of breaking something else</span></span>
* <span data-ttu-id="9fc7b-126">Bug 位於角落案例中</span><span class="sxs-lookup"><span data-stu-id="9fc7b-126">The bug is in a corner-case</span></span>

<span data-ttu-id="9fc7b-127">此橫條會逐漸增加長期支援的存留期 [ (LTS) ](https://dotnet.microsoft.com/platform/support/policy/dotnet-core) 版本。</span><span class="sxs-lookup"><span data-stu-id="9fc7b-127">This bar gradually rises through the lifetime of a [long-term support (LTS)](https://dotnet.microsoft.com/platform/support/policy/dotnet-core) release.</span></span> <span data-ttu-id="9fc7b-128">這是因為 LTS 版本強調穩定性。</span><span class="sxs-lookup"><span data-stu-id="9fc7b-128">This is because LTS releases emphasize stability.</span></span>

<span data-ttu-id="9fc7b-129">關於修補問題的最後一項決定是由 Microsoft 的 .NET 主管所提出。</span><span class="sxs-lookup"><span data-stu-id="9fc7b-129">The final decision about whether or not to patch an issue is made by the .NET Directors at Microsoft.</span></span>

### <a name="minor-releases"></a><span data-ttu-id="9fc7b-130">次要版本</span><span class="sxs-lookup"><span data-stu-id="9fc7b-130">Minor releases</span></span>

<span data-ttu-id="9fc7b-131">次要版本只會變更版本的「次要」部分。</span><span class="sxs-lookup"><span data-stu-id="9fc7b-131">Minor releases change only the "minor" part of the version.</span></span>
<span data-ttu-id="9fc7b-132">例如，EF Core 3。 **1**.0 是 EF Core 3 改善的版本。 **0**. 0。</span><span class="sxs-lookup"><span data-stu-id="9fc7b-132">For example, EF Core 3. **1**.0 is a release that improves on EF Core 3. **0**.0.</span></span>

<span data-ttu-id="9fc7b-133">次要版本：</span><span class="sxs-lookup"><span data-stu-id="9fc7b-133">Minor releases:</span></span>

* <span data-ttu-id="9fc7b-134">旨在改善舊版的品質與功能</span><span class="sxs-lookup"><span data-stu-id="9fc7b-134">Are intended to improve the quality and features of the previous release</span></span>
* <span data-ttu-id="9fc7b-135">通常包含 bug 修正和新功能</span><span class="sxs-lookup"><span data-stu-id="9fc7b-135">Typically contain bug fixes and new features</span></span>
* <span data-ttu-id="9fc7b-136">請勿包含蓄意的重大變更</span><span class="sxs-lookup"><span data-stu-id="9fc7b-136">Do not include intentional breaking changes</span></span>
* <span data-ttu-id="9fc7b-137">將幾個發行前版本預覽推送至 NuGet</span><span class="sxs-lookup"><span data-stu-id="9fc7b-137">Have a few prerelease previews pushed to NuGet</span></span>

### <a name="major-releases"></a><span data-ttu-id="9fc7b-138">主要版本</span><span class="sxs-lookup"><span data-stu-id="9fc7b-138">Major releases</span></span>

<span data-ttu-id="9fc7b-139">主要版本會變更 EF 「主要」版本號碼。</span><span class="sxs-lookup"><span data-stu-id="9fc7b-139">Major releases change the EF "major" version number.</span></span>
<span data-ttu-id="9fc7b-140">例如，EF Core **3**. 0.0 是一種主要版本，可在 EF Core 2.2. x 上前進一大步。</span><span class="sxs-lookup"><span data-stu-id="9fc7b-140">For example, EF Core **3**.0.0 is a major release that makes a big step forward over EF Core 2.2.x.</span></span>

<span data-ttu-id="9fc7b-141">主要版本：</span><span class="sxs-lookup"><span data-stu-id="9fc7b-141">Major releases:</span></span>

* <span data-ttu-id="9fc7b-142">旨在改善舊版的品質與功能</span><span class="sxs-lookup"><span data-stu-id="9fc7b-142">Are intended to improve the quality and features of the previous release</span></span>
* <span data-ttu-id="9fc7b-143">通常包含 bug 修正和新功能</span><span class="sxs-lookup"><span data-stu-id="9fc7b-143">Typically contain bug fixes and new features</span></span>
  * <span data-ttu-id="9fc7b-144">某些新功能可能是 EF Core 運作方式的基本變更</span><span class="sxs-lookup"><span data-stu-id="9fc7b-144">Some of the new features may be fundamental changes to the way EF Core works</span></span>
* <span data-ttu-id="9fc7b-145">通常包含蓄意的重大變更</span><span class="sxs-lookup"><span data-stu-id="9fc7b-145">Typically include intentional breaking changes</span></span>
  * <span data-ttu-id="9fc7b-146">重大變更是不斷演進 EF Core 的一部分，因為我們學習</span><span class="sxs-lookup"><span data-stu-id="9fc7b-146">Breaking changes are necessary part of evolving EF Core as we learn</span></span>
  * <span data-ttu-id="9fc7b-147">不過，由於潛在的客戶影響，我們很謹慎地考慮進行任何中斷性變更。</span><span class="sxs-lookup"><span data-stu-id="9fc7b-147">However, we think very carefully about making any breaking change because of the potential customer impact.</span></span> <span data-ttu-id="9fc7b-148">我們可能過度積極變更了過去的重大變更。</span><span class="sxs-lookup"><span data-stu-id="9fc7b-148">We may have been too aggressive with breaking changes in the past.</span></span> <span data-ttu-id="9fc7b-149">接下來，我們將盡力減少中斷應用程式的變更，並減少中斷資料庫提供者和延伸模組的變更。</span><span class="sxs-lookup"><span data-stu-id="9fc7b-149">Going forward, we will strive to minimize changes that break applications, and to reduce changes that break database providers and extensions.</span></span>
* <span data-ttu-id="9fc7b-150">將許多發行前版本預覽推送至 NuGet</span><span class="sxs-lookup"><span data-stu-id="9fc7b-150">Have many prerelease previews pushed to NuGet</span></span>

## <a name="planning-for-majorminor-releases"></a><span data-ttu-id="9fc7b-151">規劃主要/次要版本</span><span class="sxs-lookup"><span data-stu-id="9fc7b-151">Planning for major/minor releases</span></span>

### <a name="github-issue-tracking"></a><span data-ttu-id="9fc7b-152">GitHub 問題追蹤</span><span class="sxs-lookup"><span data-stu-id="9fc7b-152">GitHub issue tracking</span></span>

<span data-ttu-id="9fc7b-153">GitHub ([https://github.com/dotnet/efcore](https://github.com/dotnet/efcore)) 是所有 EF Core 規劃的真實來源。</span><span class="sxs-lookup"><span data-stu-id="9fc7b-153">GitHub ([https://github.com/dotnet/efcore](https://github.com/dotnet/efcore)) is the source-of-truth for all EF Core planning.</span></span>

<span data-ttu-id="9fc7b-154">GitHub 上的問題有：</span><span class="sxs-lookup"><span data-stu-id="9fc7b-154">Issues on GitHub have:</span></span>

* <span data-ttu-id="9fc7b-155">狀態</span><span class="sxs-lookup"><span data-stu-id="9fc7b-155">A state</span></span>
  * <span data-ttu-id="9fc7b-156">尚未解決[開啟](https://github.com/dotnet/efcore/issues)的問題。</span><span class="sxs-lookup"><span data-stu-id="9fc7b-156">[Open](https://github.com/dotnet/efcore/issues) issues have not been addressed.</span></span>
  * <span data-ttu-id="9fc7b-157">已[解決已解決的問題。](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aclosed)</span><span class="sxs-lookup"><span data-stu-id="9fc7b-157">[Closed](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aclosed) issues have been addressed.</span></span>
    * <span data-ttu-id="9fc7b-158">所有已修正的問題都會 [標記為已關閉修正](https://github.com/dotnet/efcore/issues?q=is%3Aissue+label%3Aclosed-fixed+is%3Aclosed)。</span><span class="sxs-lookup"><span data-stu-id="9fc7b-158">All issues that have been fixed are [tagged with closed-fixed](https://github.com/dotnet/efcore/issues?q=is%3Aissue+label%3Aclosed-fixed+is%3Aclosed).</span></span> <span data-ttu-id="9fc7b-159">已修正併合並標記為已關閉的問題，但可能尚未發行。</span><span class="sxs-lookup"><span data-stu-id="9fc7b-159">An issue tagged with closed-fixed is fixed and merged, but may not have been released.</span></span>
    * <span data-ttu-id="9fc7b-160">其他 `closed-` 標籤表示關閉問題的其他原因。</span><span class="sxs-lookup"><span data-stu-id="9fc7b-160">Other `closed-` labels indicate other reasons for closing an issue.</span></span> <span data-ttu-id="9fc7b-161">例如，重複專案會標記為已關閉重複。</span><span class="sxs-lookup"><span data-stu-id="9fc7b-161">For example, duplicates are tagged with closed-duplicate.</span></span>
* <span data-ttu-id="9fc7b-162">類型</span><span class="sxs-lookup"><span data-stu-id="9fc7b-162">A type</span></span>
  * <span data-ttu-id="9fc7b-163">[Bug](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+label%3Atype-bug) 表示 bug。</span><span class="sxs-lookup"><span data-stu-id="9fc7b-163">[Bugs](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+label%3Atype-bug) represent bugs.</span></span>
  * <span data-ttu-id="9fc7b-164">[增強](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+label%3Atype-enhancement) 功能代表現有功能中的新功能或更好的功能。</span><span class="sxs-lookup"><span data-stu-id="9fc7b-164">[Enhancements](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+label%3Atype-enhancement) represent new features or better functionality in existing features.</span></span>
* <span data-ttu-id="9fc7b-165">里程碑</span><span class="sxs-lookup"><span data-stu-id="9fc7b-165">A milestone</span></span>
  * <span data-ttu-id="9fc7b-166">小組會考慮[沒有里程碑的問題](https://github.com/dotnet/efcore/issues?q=is%3Aopen+is%3Aissue+no%3Amilestone)。</span><span class="sxs-lookup"><span data-stu-id="9fc7b-166">[Issues with no milestone](https://github.com/dotnet/efcore/issues?q=is%3Aopen+is%3Aissue+no%3Amilestone) are being considered by the team.</span></span> <span data-ttu-id="9fc7b-167">關於此問題的決策尚未進行，或是考慮到決策的變更。</span><span class="sxs-lookup"><span data-stu-id="9fc7b-167">The decision on what to do with the issue has not yet been made or a change in the decision is being considered.</span></span>
  * <span data-ttu-id="9fc7b-168">[待處理專案里程碑中的問題](https://github.com/dotnet/efcore/issues?q=is%3Aopen+is%3Aissue+milestone%3ABacklog) 是 EF 小組在未來的版本中會考慮使用的專案</span><span class="sxs-lookup"><span data-stu-id="9fc7b-168">[Issues in the Backlog milestone](https://github.com/dotnet/efcore/issues?q=is%3Aopen+is%3Aissue+milestone%3ABacklog) are items that the EF team will consider working on in a future release</span></span>
    * <span data-ttu-id="9fc7b-169">待處理專案（backlog）中的問題可能會 [標記為下一版](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+label%3Aconsider-for-next-release) ，表示此工作專案在下一版的清單中會很高。</span><span class="sxs-lookup"><span data-stu-id="9fc7b-169">Issues in the backlog may be [tagged with consider-for-next-release](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+label%3Aconsider-for-next-release) indicating that this work item is high on the list for the next release.</span></span>
  * <span data-ttu-id="9fc7b-170">在已建立版本的里程碑中開啟問題是小組計畫在該版本中使用的專案。</span><span class="sxs-lookup"><span data-stu-id="9fc7b-170">Open issues in a versioned milestone are items that the team plans to work on in that version.</span></span> <span data-ttu-id="9fc7b-171">例如， [這些是我們打算針對 EF Core 5.0 進行處理的問題](https://github.com/dotnet/efcore/issues?q=is%3Aopen+is%3Aissue+milestone%3A5.0.0)。</span><span class="sxs-lookup"><span data-stu-id="9fc7b-171">For example, [these are the issues we plan to work on for EF Core 5.0](https://github.com/dotnet/efcore/issues?q=is%3Aopen+is%3Aissue+milestone%3A5.0.0).</span></span>
  * <span data-ttu-id="9fc7b-172">已建立版本之里程碑中的已關閉問題是該版本已完成的問題。</span><span class="sxs-lookup"><span data-stu-id="9fc7b-172">Closed issues in a versioned milestone are issues that are completed for that version.</span></span> <span data-ttu-id="9fc7b-173">請注意，版本可能尚未發行。</span><span class="sxs-lookup"><span data-stu-id="9fc7b-173">Note that the version may not yet have been released.</span></span> <span data-ttu-id="9fc7b-174">例如， [這些是 EF Core 3.0 完成的問題](https://github.com/dotnet/efcore/issues?q=is%3Aissue+milestone%3A3.0.0+is%3Aclosed)。</span><span class="sxs-lookup"><span data-stu-id="9fc7b-174">For example, [these are the issues completed for EF Core 3.0](https://github.com/dotnet/efcore/issues?q=is%3Aissue+milestone%3A3.0.0+is%3Aclosed).</span></span>
* <span data-ttu-id="9fc7b-175">票！</span><span class="sxs-lookup"><span data-stu-id="9fc7b-175">Votes!</span></span>
  * <span data-ttu-id="9fc7b-176">投票是指出問題對您很重要的最佳方式。</span><span class="sxs-lookup"><span data-stu-id="9fc7b-176">Voting is the best way to indicate that an issue is important to you.</span></span>
  * <span data-ttu-id="9fc7b-177">若要投票，請只在問題中新增「拇指向上」 👍 。</span><span class="sxs-lookup"><span data-stu-id="9fc7b-177">To vote, just add a "thumbs-up" 👍 to the issue.</span></span> <span data-ttu-id="9fc7b-178">例如， [這些是最常投票的問題](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+sort%3Areactions-%2B1-desc)</span><span class="sxs-lookup"><span data-stu-id="9fc7b-178">For example, [these are the top-voted issues](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+sort%3Areactions-%2B1-desc)</span></span>
  * <span data-ttu-id="9fc7b-179">如果您覺得這會增加價值，也請批註描述您需要此功能的特定原因。</span><span class="sxs-lookup"><span data-stu-id="9fc7b-179">Please also comment describing specific reasons you need the feature if you feel this adds value.</span></span> <span data-ttu-id="9fc7b-180">批註 "+ 1" 或類似的不會增加值。</span><span class="sxs-lookup"><span data-stu-id="9fc7b-180">Commenting "+1" or similar does not add value.</span></span>

### <a name="the-planning-process"></a><span data-ttu-id="9fc7b-181">規劃程序</span><span class="sxs-lookup"><span data-stu-id="9fc7b-181">The planning process</span></span>

<span data-ttu-id="9fc7b-182">規劃程式比起從待處理專案取得最上層要求的功能更多。</span><span class="sxs-lookup"><span data-stu-id="9fc7b-182">The planning process is more involved than just taking the top-most requested features from the backlog.</span></span>
<span data-ttu-id="9fc7b-183">這是因為我們會以多種方式收集來自多個專案關係人的意見反應。</span><span class="sxs-lookup"><span data-stu-id="9fc7b-183">This is because we gather feedback from multiple stakeholders in multiple ways.</span></span>
<span data-ttu-id="9fc7b-184">接著，我們會根據下列內容來塑造發行：</span><span class="sxs-lookup"><span data-stu-id="9fc7b-184">We then shape a release based on:</span></span>

* <span data-ttu-id="9fc7b-185">客戶的輸入</span><span class="sxs-lookup"><span data-stu-id="9fc7b-185">Input from customers</span></span>
* <span data-ttu-id="9fc7b-186">來自其他專案關係人的輸入</span><span class="sxs-lookup"><span data-stu-id="9fc7b-186">Input from other stakeholders</span></span>
* <span data-ttu-id="9fc7b-187">策略性方向</span><span class="sxs-lookup"><span data-stu-id="9fc7b-187">Strategic direction</span></span>
* <span data-ttu-id="9fc7b-188">可用的資源</span><span class="sxs-lookup"><span data-stu-id="9fc7b-188">Resources available</span></span>
* <span data-ttu-id="9fc7b-189">排程</span><span class="sxs-lookup"><span data-stu-id="9fc7b-189">Schedule</span></span>

<span data-ttu-id="9fc7b-190">我們提出的一些問題如下：</span><span class="sxs-lookup"><span data-stu-id="9fc7b-190">Some of the questions we ask are:</span></span>

1. <span data-ttu-id="9fc7b-191">**我們認為多少開發人員會使用此功能，而它讓他們的應用程式或體驗改善多少？**</span><span class="sxs-lookup"><span data-stu-id="9fc7b-191">**How many developers we think will use the feature and how much better will it make their applications or experience?**</span></span> <span data-ttu-id="9fc7b-192">為了回答這個問題，我們從許多來源收集意見反應 — 問題的留言與投票是那些來源之一。</span><span class="sxs-lookup"><span data-stu-id="9fc7b-192">To answer this question, we collect feedback from many sources — Comments and votes on issues is one of those sources.</span></span> <span data-ttu-id="9fc7b-193">與重要客戶相關的特定合作是另一種。</span><span class="sxs-lookup"><span data-stu-id="9fc7b-193">Specific engagements with important customers is another.</span></span>

2. <span data-ttu-id="9fc7b-194">**如果我們尚未執行這項功能，有哪些因應措施可供使用？**</span><span class="sxs-lookup"><span data-stu-id="9fc7b-194">**What are the workarounds people can use if we don't implement this feature yet?**</span></span> <span data-ttu-id="9fc7b-195">舉例來說，許多開發人員在缺少原生多對多支援時，都會想到運用聯合資料表作為因應措施。</span><span class="sxs-lookup"><span data-stu-id="9fc7b-195">For example, many developers can map a join table to work around lack of native many-to-many support.</span></span> <span data-ttu-id="9fc7b-196">很明顯地，並非所有開發人員都想這麼做，但其中許多人都做得到，而這也是我們決策中所考量的因素。</span><span class="sxs-lookup"><span data-stu-id="9fc7b-196">Obviously, not all developers want to do it, but many can, and that counts as a factor in our decision.</span></span>

3. <span data-ttu-id="9fc7b-197">**實作此功能是否會涉及 EF Core 的架構，使我們更加需要實作其他功能？**</span><span class="sxs-lookup"><span data-stu-id="9fc7b-197">**Does implementing this feature evolve the architecture of EF Core such that it moves us closer to implementing other features?**</span></span> <span data-ttu-id="9fc7b-198">我們偏好可作為其他功能建置組塊的功能。</span><span class="sxs-lookup"><span data-stu-id="9fc7b-198">We tend to favor features that act as building blocks for other features.</span></span> <span data-ttu-id="9fc7b-199">比方說，屬性包實體可以協助我們邁向多對多的支援，而實體建構函式則促成了消極式載入支援。</span><span class="sxs-lookup"><span data-stu-id="9fc7b-199">For instance, property bag entities can help us move towards many-to-many support, and entity constructors enabled our lazy loading support.</span></span>

4. <span data-ttu-id="9fc7b-200">**這項功能是擴充點嗎？**</span><span class="sxs-lookup"><span data-stu-id="9fc7b-200">**Is the feature an extensibility point?**</span></span> <span data-ttu-id="9fc7b-201">因為擴充點使開發人員能夠輕鬆地依自己的習慣使用，並填補任何功能上的缺失，所以我們偏好擴充點勝過於一般功能。</span><span class="sxs-lookup"><span data-stu-id="9fc7b-201">We tend to favor extensibility points over normal features because they enable developers to hook their own behaviors and compensate for any missing functionality.</span></span>

5. <span data-ttu-id="9fc7b-202">**此功能與其他產品搭配使用時的綜效如何？**</span><span class="sxs-lookup"><span data-stu-id="9fc7b-202">**What is the synergy of the feature when used in combination with other products?**</span></span> <span data-ttu-id="9fc7b-203">我們偏好能促成或大幅改善 EF Core 搭配其他產品使用體驗的功能，例如 .NET Core、最新版本的 Visual Studio、Microsoft Azure 等等。</span><span class="sxs-lookup"><span data-stu-id="9fc7b-203">We favor features that enable or significantly improve the experience of using EF Core with other products, such as .NET Core, the latest version of Visual Studio, Microsoft Azure, and so on.</span></span>

6. <span data-ttu-id="9fc7b-204">**可以用來處理某項功能的人員技能有哪些，以及如何充分利用這些資源？**</span><span class="sxs-lookup"><span data-stu-id="9fc7b-204">**What are the skills of the people available to work on a feature, and how can we best leverage these resources?**</span></span> <span data-ttu-id="9fc7b-205">每位 EF 小組的成員及我們社群的參與者，在不同領域都各有不同程度的經驗，所以我們必須據此進行規劃。</span><span class="sxs-lookup"><span data-stu-id="9fc7b-205">Each member of the EF team and our community contributors has different levels of experience in different areas, so we have to plan accordingly.</span></span> <span data-ttu-id="9fc7b-206">即使我們希望將「所有人力」集中在像是 GroupBy 翻譯，或多對多關聯性等特定功能上，也無法實際付諸行動。</span><span class="sxs-lookup"><span data-stu-id="9fc7b-206">Even if we wanted to have "all hands on deck" to work on a specific feature like GroupBy translations, or many-to-many, that wouldn't be practical.</span></span>
