---
title: EF 核心發佈計劃
author: ajcvickers
ms.date: 01/28/2020
uid: core/what-is-new/release_planning.md
ms.openlocfilehash: 71045b8d49c319a73f74443612bedd84ee33ab8a
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417333"
---
# <a name="release-planning-process"></a><span data-ttu-id="03cff-102">發行計劃程序</span><span class="sxs-lookup"><span data-stu-id="03cff-102">Release planning process</span></span>

<span data-ttu-id="03cff-103">在我們收到的問題中，經常有人問起我們如何決定將某功能加入特定的版本。</span><span class="sxs-lookup"><span data-stu-id="03cff-103">We often get questions about how we choose specific features to go into a particular release.</span></span>
<span data-ttu-id="03cff-104">本文檔概述了我們使用的過程。</span><span class="sxs-lookup"><span data-stu-id="03cff-104">This document outlines the process we use.</span></span>
<span data-ttu-id="03cff-105">隨著我們找到更好的規劃方法,這個過程在不斷發展,但一般想法保持不變。</span><span class="sxs-lookup"><span data-stu-id="03cff-105">The process is continually evolving as we find better ways to plan, but the general ideas remain the same.</span></span>

## <a name="different-kinds-of-releases"></a><span data-ttu-id="03cff-106">不同類型的版本</span><span class="sxs-lookup"><span data-stu-id="03cff-106">Different kinds of releases</span></span>

<span data-ttu-id="03cff-107">不同類型的版本包含不同類型的更改。</span><span class="sxs-lookup"><span data-stu-id="03cff-107">Different kinds of release contain different kinds of changes.</span></span>
<span data-ttu-id="03cff-108">這反過來意味著針對不同類型的版本,發佈計劃是不同的。</span><span class="sxs-lookup"><span data-stu-id="03cff-108">This in turn means that the release planning is different for different kinds of release.</span></span>

### <a name="patch-releases"></a><span data-ttu-id="03cff-109">修補程式版本</span><span class="sxs-lookup"><span data-stu-id="03cff-109">Patch releases</span></span>

<span data-ttu-id="03cff-110">修補程式版本僅更改版本的「修補程式」 部分。</span><span class="sxs-lookup"><span data-stu-id="03cff-110">Patch releases change only the "patch" part of the version.</span></span>
<span data-ttu-id="03cff-111">例如,EF 核心 3.1。**1**是修補程式 EF Core 3.1 中發現的問題的發佈。**0**.</span><span class="sxs-lookup"><span data-stu-id="03cff-111">For example, EF Core 3.1.**1** is a release that patches issues found in EF Core 3.1.**0**.</span></span>

<span data-ttu-id="03cff-112">修補程式版本旨在修復關鍵客戶 Bug。</span><span class="sxs-lookup"><span data-stu-id="03cff-112">Patch releases are intended to fix critical customer bugs.</span></span>
<span data-ttu-id="03cff-113">這意味著修補程式版本不包含新功能。</span><span class="sxs-lookup"><span data-stu-id="03cff-113">This means patch releases do not include new features.</span></span>
<span data-ttu-id="03cff-114">除非在特殊情況下,否則修補程式版本中不允許進行 API 更改。</span><span class="sxs-lookup"><span data-stu-id="03cff-114">API changes are not allowed in patch releases except in special circumstances.</span></span>

<span data-ttu-id="03cff-115">在修補程式版本中進行更改的柱線非常高。</span><span class="sxs-lookup"><span data-stu-id="03cff-115">The bar to make a change in a patch release is very high.</span></span>
<span data-ttu-id="03cff-116">這是因為修補程式版本不引入新 Bug 至關重要。</span><span class="sxs-lookup"><span data-stu-id="03cff-116">This is because it is critical that patch releases do not introduce new bugs.</span></span>
<span data-ttu-id="03cff-117">因此,決策過程強調高價值和低風險。</span><span class="sxs-lookup"><span data-stu-id="03cff-117">Therefore, the decision process emphasizes high value and low risk.</span></span>

<span data-ttu-id="03cff-118">如果出現:</span><span class="sxs-lookup"><span data-stu-id="03cff-118">We are more likely to patch an issue if:</span></span>
  * <span data-ttu-id="03cff-119">它影響多個客戶</span><span class="sxs-lookup"><span data-stu-id="03cff-119">It is impacting multiple customers</span></span>
  * <span data-ttu-id="03cff-120">它是從上一個版本的回歸</span><span class="sxs-lookup"><span data-stu-id="03cff-120">It is a regression from a previous release</span></span>
  * <span data-ttu-id="03cff-121">失敗導致資料損毀</span><span class="sxs-lookup"><span data-stu-id="03cff-121">The failure causes data corruption</span></span>

<span data-ttu-id="03cff-122">如果出現以下問題,我們不太可能修補問題:</span><span class="sxs-lookup"><span data-stu-id="03cff-122">We are less likely to patch an issue if:</span></span>
  * <span data-ttu-id="03cff-123">有合理的解決方法</span><span class="sxs-lookup"><span data-stu-id="03cff-123">There are reasonable workarounds</span></span>
  * <span data-ttu-id="03cff-124">修復有破壞其他東西的高風險</span><span class="sxs-lookup"><span data-stu-id="03cff-124">The fix has high risk of breaking something else</span></span>
  * <span data-ttu-id="03cff-125">錯誤處於拐角匣中</span><span class="sxs-lookup"><span data-stu-id="03cff-125">The bug is in a corner-case</span></span>

<span data-ttu-id="03cff-126">此條柱逐漸在[長期支援 (LTS)](https://dotnet.microsoft.com/platform/support/policy/dotnet-core)發佈的生存期內上升。</span><span class="sxs-lookup"><span data-stu-id="03cff-126">This bar gradually rises through the lifetime of a [long-term support (LTS)](https://dotnet.microsoft.com/platform/support/policy/dotnet-core) release.</span></span> <span data-ttu-id="03cff-127">這是因為 LTS 版本強調穩定性。</span><span class="sxs-lookup"><span data-stu-id="03cff-127">This is because LTS releases emphasize stability.</span></span>

<span data-ttu-id="03cff-128">關於是否修補問題的最終決定由微軟的 .NET 主管做出。</span><span class="sxs-lookup"><span data-stu-id="03cff-128">The final decision about whether or not to patch an issue is made by the .NET Directors at Microsoft.</span></span>

### <a name="minor-releases"></a><span data-ttu-id="03cff-129">次要版本</span><span class="sxs-lookup"><span data-stu-id="03cff-129">Minor releases</span></span>

<span data-ttu-id="03cff-130">次要版本僅更改版本的"次要"部分。</span><span class="sxs-lookup"><span data-stu-id="03cff-130">Minor releases change only the "minor" part of the version.</span></span>
<span data-ttu-id="03cff-131">例如,EF 核心 3。**1**.0 是改進 EF 核心 3 的發佈。**0**.0。</span><span class="sxs-lookup"><span data-stu-id="03cff-131">For example, EF Core 3.**1**.0 is a release that improves on EF Core 3.**0**.0.</span></span>

<span data-ttu-id="03cff-132">次要版本:</span><span class="sxs-lookup"><span data-stu-id="03cff-132">Minor releases:</span></span>
* <span data-ttu-id="03cff-133">旨在提高上一版本的品質和功能</span><span class="sxs-lookup"><span data-stu-id="03cff-133">Are intended to improve the quality and features of the previous release</span></span>
* <span data-ttu-id="03cff-134">通常包含錯誤修復與新功能</span><span class="sxs-lookup"><span data-stu-id="03cff-134">Typically contain bug fixes and new features</span></span>
* <span data-ttu-id="03cff-135">不包括故意中斷變更</span><span class="sxs-lookup"><span data-stu-id="03cff-135">Do not include intentional breaking changes</span></span>
* <span data-ttu-id="03cff-136">將一些預先預覽到 NuGet</span><span class="sxs-lookup"><span data-stu-id="03cff-136">Have a few prerelease previews pushed to NuGet</span></span>

### <a name="major-releases"></a><span data-ttu-id="03cff-137">主要版本</span><span class="sxs-lookup"><span data-stu-id="03cff-137">Major releases</span></span>

<span data-ttu-id="03cff-138">主要版本更改 EF"主要"</span><span class="sxs-lookup"><span data-stu-id="03cff-138">Major releases change the EF "major" version number.</span></span>
<span data-ttu-id="03cff-139">例如,EF Core **3**.0.0 是一個主要版本,在 EF Core 2.2.x 上向前邁出了一大步。</span><span class="sxs-lookup"><span data-stu-id="03cff-139">For example, EF Core **3**.0.0 is a major release that makes a big step forward over EF Core 2.2.x.</span></span>

<span data-ttu-id="03cff-140">主要版本:</span><span class="sxs-lookup"><span data-stu-id="03cff-140">Major releases:</span></span>
* <span data-ttu-id="03cff-141">旨在提高上一版本的品質和功能</span><span class="sxs-lookup"><span data-stu-id="03cff-141">Are intended to improve the quality and features of the previous release</span></span>
* <span data-ttu-id="03cff-142">通常包含錯誤修復與新功能</span><span class="sxs-lookup"><span data-stu-id="03cff-142">Typically contain bug fixes and new features</span></span>
  * <span data-ttu-id="03cff-143">一些新功能可能是 EF Core 工作方式的根本改變</span><span class="sxs-lookup"><span data-stu-id="03cff-143">Some of the new features may be fundamental changes to the way EF Core works</span></span>
* <span data-ttu-id="03cff-144">通常包括有意中斷的變更</span><span class="sxs-lookup"><span data-stu-id="03cff-144">Typically include intentional breaking changes</span></span>
  * <span data-ttu-id="03cff-145">在我們學習時,重大變革是不斷發展的 EF 核心的必要部分</span><span class="sxs-lookup"><span data-stu-id="03cff-145">Breaking changes are necessary part of evolving EF Core as we learn</span></span>
  * <span data-ttu-id="03cff-146">但是,由於潛在的客戶影響,我們非常仔細地考慮做出任何重大改變。</span><span class="sxs-lookup"><span data-stu-id="03cff-146">However, we think very carefully about making any breaking change because of the potential customer impact.</span></span> <span data-ttu-id="03cff-147">過去,我們可能過於積極地進行重大變革。</span><span class="sxs-lookup"><span data-stu-id="03cff-147">We may have been too aggressive with breaking changes in the past.</span></span> <span data-ttu-id="03cff-148">今後,我們將努力盡量減少破壞應用程式的更改,並減少中斷資料庫提供程式和擴展的更改。</span><span class="sxs-lookup"><span data-stu-id="03cff-148">Going forward, we will strive to minimize changes that break applications, and to reduce changes that break database providers and extensions.</span></span>
* <span data-ttu-id="03cff-149">將許多預先發行預覽推送到 NuGet</span><span class="sxs-lookup"><span data-stu-id="03cff-149">Have many prerelease previews pushed to NuGet</span></span>

## <a name="planning-for-majorminor-releases"></a><span data-ttu-id="03cff-150">主要/次要版本的規劃</span><span class="sxs-lookup"><span data-stu-id="03cff-150">Planning for major/minor releases</span></span>

### <a name="github-issue-tracking"></a><span data-ttu-id="03cff-151">GitHub 問題追蹤</span><span class="sxs-lookup"><span data-stu-id="03cff-151">GitHub issue tracking</span></span>

<span data-ttu-id="03cff-152">GitHub[https://github.com/dotnet/efcore](https://github.com/dotnet/efcore)( ) 是所有 EF 核心規劃的真諦源。</span><span class="sxs-lookup"><span data-stu-id="03cff-152">GitHub ([https://github.com/dotnet/efcore](https://github.com/dotnet/efcore)) is the source-of-truth for all EF Core planning.</span></span>

<span data-ttu-id="03cff-153">GitHub 上的問題有:</span><span class="sxs-lookup"><span data-stu-id="03cff-153">Issues on GitHub have:</span></span>

* <span data-ttu-id="03cff-154">狀態</span><span class="sxs-lookup"><span data-stu-id="03cff-154">A state</span></span>
  * <span data-ttu-id="03cff-155">[尚未解決未決](https://github.com/dotnet/efcore/issues)問題。</span><span class="sxs-lookup"><span data-stu-id="03cff-155">[Open](https://github.com/dotnet/efcore/issues) issues have not been addressed.</span></span>
  * <span data-ttu-id="03cff-156">[已解決已](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aclosed)解決的問題。</span><span class="sxs-lookup"><span data-stu-id="03cff-156">[Closed](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aclosed) issues have been addressed.</span></span>
    * <span data-ttu-id="03cff-157">已修復的所有問題都[標有閉合修復](https://github.com/dotnet/efcore/issues?q=is%3Aissue+label%3Aclosed-fixed+is%3Aclosed)。</span><span class="sxs-lookup"><span data-stu-id="03cff-157">All issues that have been fixed are [tagged with closed-fixed](https://github.com/dotnet/efcore/issues?q=is%3Aissue+label%3Aclosed-fixed+is%3Aclosed).</span></span> <span data-ttu-id="03cff-158">已修復並合併了標記為閉合修復的問題,但可能尚未釋放。</span><span class="sxs-lookup"><span data-stu-id="03cff-158">An issue tagged with closed-fixed is fixed and merged, but may not have been released.</span></span>
    * <span data-ttu-id="03cff-159">其他`closed-`標籤指示關閉問題的其他原因。</span><span class="sxs-lookup"><span data-stu-id="03cff-159">Other `closed-` labels indicate other reasons for closing an issue.</span></span> <span data-ttu-id="03cff-160">例如,重複項用閉複式標記。</span><span class="sxs-lookup"><span data-stu-id="03cff-160">For example, duplicates are tagged with closed-duplicate.</span></span>
* <span data-ttu-id="03cff-161">類型</span><span class="sxs-lookup"><span data-stu-id="03cff-161">A type</span></span>
  * <span data-ttu-id="03cff-162">[錯誤](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+label%3Atype-bug)表示 Bug。</span><span class="sxs-lookup"><span data-stu-id="03cff-162">[Bugs](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+label%3Atype-bug) represent bugs.</span></span>
  * <span data-ttu-id="03cff-163">[增強功能](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+label%3Atype-enhancement)表示現有功能中的新功能或更好的功能。</span><span class="sxs-lookup"><span data-stu-id="03cff-163">[Enhancements](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+label%3Atype-enhancement) represent new features or better functionality in existing features.</span></span>
* <span data-ttu-id="03cff-164">里程碑</span><span class="sxs-lookup"><span data-stu-id="03cff-164">A milestone</span></span>
  * <span data-ttu-id="03cff-165">團隊正在考慮[沒有里程碑的問題](https://github.com/dotnet/efcore/issues?q=is%3Aopen+is%3Aissue+no%3Amilestone)。</span><span class="sxs-lookup"><span data-stu-id="03cff-165">[Issues with no milestone](https://github.com/dotnet/efcore/issues?q=is%3Aopen+is%3Aissue+no%3Amilestone) are being considered by the team.</span></span> <span data-ttu-id="03cff-166">尚未就如何處理該問題作出決定,或正在考慮改變該決定。</span><span class="sxs-lookup"><span data-stu-id="03cff-166">The decision on what to do with the issue has not yet been made or a change in the decision is being considered.</span></span>
  * <span data-ttu-id="03cff-167">[積壓工作里程碑中的問題](https://github.com/dotnet/efcore/issues?q=is%3Aopen+is%3Aissue+milestone%3ABacklog),是 EF 團隊將考慮在將來的版本中處理的專案</span><span class="sxs-lookup"><span data-stu-id="03cff-167">[Issues in the Backlog milestone](https://github.com/dotnet/efcore/issues?q=is%3Aopen+is%3Aissue+milestone%3ABacklog) are items that the EF team will consider working on in a future release</span></span>
    * <span data-ttu-id="03cff-168">積壓工作中的問題可能會[標記為"考慮下一個版本](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+label%3Aconsider-for-next-release)",指示此工作項在下一個版本的清單中處於較高位置。</span><span class="sxs-lookup"><span data-stu-id="03cff-168">Issues in the backlog may be [tagged with consider-for-next-release](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+label%3Aconsider-for-next-release) indicating that this work item is high on the list for the next release.</span></span>
  * <span data-ttu-id="03cff-169">版本控制里程碑中的未打開問題是團隊計劃在這一版本中處理的專案。</span><span class="sxs-lookup"><span data-stu-id="03cff-169">Open issues in a versioned milestone are items that the team plans to work on in that version.</span></span> <span data-ttu-id="03cff-170">例如,[這些是我們計劃為 EF Core 5.0 解決的問題](https://github.com/dotnet/efcore/issues?q=is%3Aopen+is%3Aissue+milestone%3A5.0.0)。</span><span class="sxs-lookup"><span data-stu-id="03cff-170">For example, [these are the issues we plan to work on for EF Core 5.0](https://github.com/dotnet/efcore/issues?q=is%3Aopen+is%3Aissue+milestone%3A5.0.0).</span></span>
  * <span data-ttu-id="03cff-171">版本控制里程碑中的已關閉問題是該版本已完成的問題。</span><span class="sxs-lookup"><span data-stu-id="03cff-171">Closed issues in a versioned milestone are issues that are completed for that version.</span></span> <span data-ttu-id="03cff-172">請注意,該版本可能尚未發佈。</span><span class="sxs-lookup"><span data-stu-id="03cff-172">Note that the version may not yet have been released.</span></span> <span data-ttu-id="03cff-173">例如,[這些是 EF 核心 3.0 完成的問題](https://github.com/dotnet/efcore/issues?q=is%3Aissue+milestone%3A3.0.0+is%3Aclosed)。</span><span class="sxs-lookup"><span data-stu-id="03cff-173">For example, [these are the issues completed for EF Core 3.0](https://github.com/dotnet/efcore/issues?q=is%3Aissue+milestone%3A3.0.0+is%3Aclosed).</span></span>
* <span data-ttu-id="03cff-174">票!</span><span class="sxs-lookup"><span data-stu-id="03cff-174">Votes!</span></span>
  * <span data-ttu-id="03cff-175">投票是表明問題對您很重要的最佳方式。</span><span class="sxs-lookup"><span data-stu-id="03cff-175">Voting is the best way to indicate that an issue is important to you.</span></span>
  * <span data-ttu-id="03cff-176">要投票,只需為問題添加"豎起大拇指👍"。</span><span class="sxs-lookup"><span data-stu-id="03cff-176">To vote, just add a "thumbs-up" 👍 to the issue.</span></span> <span data-ttu-id="03cff-177">例如[,這些是投票率最高的問題](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+sort%3Areactions-%2B1-desc)</span><span class="sxs-lookup"><span data-stu-id="03cff-177">For example, [these are the top-voted issues](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+sort%3Areactions-%2B1-desc)</span></span>
  * <span data-ttu-id="03cff-178">如果您覺得這增加了價值,請還請註釋描述需要該功能的具體原因。</span><span class="sxs-lookup"><span data-stu-id="03cff-178">Please also comment describing specific reasons you need the feature if you feel this adds value.</span></span> <span data-ttu-id="03cff-179">註釋"+1"或類似值不會增加價值。</span><span class="sxs-lookup"><span data-stu-id="03cff-179">Commenting "+1" or similar does not add value.</span></span>

### <a name="the-planning-process"></a><span data-ttu-id="03cff-180">規劃程序</span><span class="sxs-lookup"><span data-stu-id="03cff-180">The planning process</span></span>

<span data-ttu-id="03cff-181">規劃過程涉及的不僅僅是從積壓工作中獲取請求最多的功能。</span><span class="sxs-lookup"><span data-stu-id="03cff-181">The planning process is more involved than just taking the top-most requested features from the backlog.</span></span>
<span data-ttu-id="03cff-182">這是因為我們以多種方式從多個利益相關者收集反饋。</span><span class="sxs-lookup"><span data-stu-id="03cff-182">This is because we gather feedback from multiple stakeholders in multiple ways.</span></span>
<span data-ttu-id="03cff-183">然後,我們根據:</span><span class="sxs-lookup"><span data-stu-id="03cff-183">We then shape a release based on:</span></span>

* <span data-ttu-id="03cff-184">從客戶輸入</span><span class="sxs-lookup"><span data-stu-id="03cff-184">Input from customers</span></span>
* <span data-ttu-id="03cff-185">其他利益相關者提供的意見</span><span class="sxs-lookup"><span data-stu-id="03cff-185">Input from other stakeholders</span></span>
* <span data-ttu-id="03cff-186">戰略方向</span><span class="sxs-lookup"><span data-stu-id="03cff-186">Strategic direction</span></span>
* <span data-ttu-id="03cff-187">可用資源</span><span class="sxs-lookup"><span data-stu-id="03cff-187">Resources available</span></span>
* <span data-ttu-id="03cff-188">排程</span><span class="sxs-lookup"><span data-stu-id="03cff-188">Schedule</span></span>

<span data-ttu-id="03cff-189">我們問的一些問題包括:</span><span class="sxs-lookup"><span data-stu-id="03cff-189">Some of the questions we ask are:</span></span>

1. <span data-ttu-id="03cff-190">**我們認為多少開發人員會使用此功能，而它讓他們的應用程式或體驗改善多少？**</span><span class="sxs-lookup"><span data-stu-id="03cff-190">**How many developers we think will use the feature and how much better will it make their applications or experience?**</span></span> <span data-ttu-id="03cff-191">為了回答這個問題，我們從許多來源收集意見反應 — 問題的留言與投票是那些來源之一。</span><span class="sxs-lookup"><span data-stu-id="03cff-191">To answer this question, we collect feedback from many sources — Comments and votes on issues is one of those sources.</span></span> <span data-ttu-id="03cff-192">與重要客戶的具體互動是另一回事。</span><span class="sxs-lookup"><span data-stu-id="03cff-192">Specific engagements with important customers is another.</span></span>

2. <span data-ttu-id="03cff-193">**如果我們尚未實現此功能,人們可以使用哪些解決方法?**</span><span class="sxs-lookup"><span data-stu-id="03cff-193">**What are the workarounds people can use if we don't implement this feature yet?**</span></span> <span data-ttu-id="03cff-194">舉例來說，許多開發人員在缺少原生多對多支援時，都會想到運用聯合資料表作為因應措施。</span><span class="sxs-lookup"><span data-stu-id="03cff-194">For example, many developers can map a join table to work around lack of native many-to-many support.</span></span> <span data-ttu-id="03cff-195">很明顯地，並非所有開發人員都想這麼做，但其中許多人都做得到，而這也是我們決策中所考量的因素。</span><span class="sxs-lookup"><span data-stu-id="03cff-195">Obviously, not all developers want to do it, but many can, and that counts as a factor in our decision.</span></span>

3. <span data-ttu-id="03cff-196">**實作此功能是否會涉及 EF Core 的架構，使我們更加需要實作其他功能？**</span><span class="sxs-lookup"><span data-stu-id="03cff-196">**Does implementing this feature evolve the architecture of EF Core such that it moves us closer to implementing other features?**</span></span> <span data-ttu-id="03cff-197">我們偏好可作為其他功能建置組塊的功能。</span><span class="sxs-lookup"><span data-stu-id="03cff-197">We tend to favor features that act as building blocks for other features.</span></span> <span data-ttu-id="03cff-198">比方說，屬性包實體可以協助我們邁向多對多的支援，而實體建構函式則促成了消極式載入支援。</span><span class="sxs-lookup"><span data-stu-id="03cff-198">For instance, property bag entities can help us move towards many-to-many support, and entity constructors enabled our lazy loading support.</span></span>

4. <span data-ttu-id="03cff-199">**這項功能是擴充點嗎？**</span><span class="sxs-lookup"><span data-stu-id="03cff-199">**Is the feature an extensibility point?**</span></span> <span data-ttu-id="03cff-200">因為擴充點使開發人員能夠輕鬆地依自己的習慣使用，並填補任何功能上的缺失，所以我們偏好擴充點勝過於一般功能。</span><span class="sxs-lookup"><span data-stu-id="03cff-200">We tend to favor extensibility points over normal features because they enable developers to hook their own behaviors and compensate for any missing functionality.</span></span>

5. <span data-ttu-id="03cff-201">**此功能與其他產品搭配使用時的綜效如何？**</span><span class="sxs-lookup"><span data-stu-id="03cff-201">**What is the synergy of the feature when used in combination with other products?**</span></span> <span data-ttu-id="03cff-202">我們偏好能促成或大幅改善 EF Core 搭配其他產品使用體驗的功能，例如 .NET Core、最新版本的 Visual Studio、Microsoft Azure 等等。</span><span class="sxs-lookup"><span data-stu-id="03cff-202">We favor features that enable or significantly improve the experience of using EF Core with other products, such as .NET Core, the latest version of Visual Studio, Microsoft Azure, and so on.</span></span>

6. <span data-ttu-id="03cff-203">**可用於處理某項功能的人員有哪些技能,我們如何才能最好地利用這些資源?**</span><span class="sxs-lookup"><span data-stu-id="03cff-203">**What are the skills of the people available to work on a feature, and how can we best leverage these resources?**</span></span> <span data-ttu-id="03cff-204">每位 EF 小組的成員及我們社群的參與者，在不同領域都各有不同程度的經驗，所以我們必須據此進行規劃。</span><span class="sxs-lookup"><span data-stu-id="03cff-204">Each member of the EF team and our community contributors has different levels of experience in different areas, so we have to plan accordingly.</span></span> <span data-ttu-id="03cff-205">即使我們希望將「所有人力」集中在像是 GroupBy 翻譯，或多對多關聯性等特定功能上，也無法實際付諸行動。</span><span class="sxs-lookup"><span data-stu-id="03cff-205">Even if we wanted to have "all hands on deck" to work on a specific feature like GroupBy translations, or many-to-many, that wouldn't be practical.</span></span>
