---
title: Table-Valued 函式 (Tvf) -EF6
description: 'Entity Framework 6 中的 Table-Valued 函式 (Tvf) '
author: ajcvickers
ms.date: 10/23/2016
uid: ef6/modeling/designer/advanced/tvfs
ms.openlocfilehash: 2c699cad3ace253842d17aca4eca4df2318c4795
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/14/2020
ms.locfileid: "92066196"
---
# <a name="table-valued-functions-tvfs"></a>Table-Valued 函式 (Tvf) 
> [!NOTE]
> **EF5 只** 會提供此 Entity Framework 頁面中所討論的功能、api 等等。 如果您使用的是較早版本，則不適用部分或全部的資訊。

影片和逐步解說示範如何使用 Entity Framework Designer，將資料表值函式 (Tvf) 對應。 它也會示範如何從 LINQ 查詢呼叫 TVF。

Database First 工作流程目前僅支援 Tvf。

TVF 支援是在 Entity Framework 第5版中引進。 請注意，若要使用新功能（例如資料表值函式、列舉和空間類型），您必須將目標設為 .NET Framework 4.5。 Visual Studio 2012 預設會以 .NET 4.5 為目標。

Tvf 與預存程式非常類似，但有一個主要差異： TVF 的結果是可組合的。 這表示 TVF 的結果可以在 LINQ 查詢中使用，而預存程式的結果則無法使用。

## <a name="watch-the-video"></a>觀賞影片

**提供者**： Julia Kornich

[WMV](https://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.wmv)  | [MP4](https://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-mp4video-tvf.m4v)的  | [WMV (ZIP) ](https://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.zip)

## <a name="pre-requisites"></a>必要條件

若要完成這個逐步解說，您需要：

- 安裝 [School 資料庫](xref:ef6/resources/school-database)。

- 具有最新版本的 Visual Studio

## <a name="set-up-the-project"></a>設定專案

1.  開啟 Visual Studio
2.  **在 [檔案**] 功能表上，指向 [**新增**]，然後按一下 [**專案**]。
3.  在左窗格中，按一下 [ **Visual \# C**]，然後選取**主控台**範本
4.  輸入 **TVF** 作為專案的名稱，然後按一下 **[確定]**

## <a name="add-a-tvf-to-the-database"></a>將 TVF 新增至資料庫

-   選取 **View- &gt; SQL Server 物件總管**
-   如果 LocalDB 不在伺服器清單中：以滑鼠右鍵按一下 [ **SQL Server** ]，然後選取 [ **新增] SQL Server** 使用預設的 **Windows 驗證** 連接到 LocalDB 伺服器
-   展開 LocalDB 節點
-   在 [資料庫] 節點底下，以滑鼠右鍵按一下 School 資料庫節點，然後選取 [追加 **查詢**]。
-   在 [T-sql 編輯器] 中，貼上下列 TVF 定義

``` SQL
CREATE FUNCTION [dbo].[GetStudentGradesForCourse]

(@CourseID INT)

RETURNS TABLE

RETURN
    SELECT [EnrollmentID],
           [CourseID],
           [StudentID],
           [Grade]
    FROM   [dbo].[StudentGrade]
    WHERE  CourseID = @CourseID
```

-   在 T-sql 編輯器上按一下滑鼠右鍵，然後選取 [ **執行**]
-   GetStudentGradesForCourse 函式會新增至 School 資料庫

 

## <a name="create-a-model"></a>建立模型

1.  在方案總管中的專案名稱上按一下滑鼠右鍵，指向 [**加入**]，然後按一下 [**新增專案**]。
2.  從左側功能表中選取 [**資料**]，然後選取 [**範本**] 窗格中的 [ **ADO.NET 實體資料模型**]
3.  輸入**TVFModel**的檔案名，然後按一下 [**新增**]。
4.  在 [選擇模型內容] 對話方塊中，選取 [ **從資料庫產生**]，然後按 **[下一步]** 。
5.  在 [伺服器名稱] 文字方塊中，按一下 [ **新增連接**] ** (localdb) \\ Mssqllocaldb**中，輸入**School**   作為資料庫名稱，按一下 **[確定]** 。
6.  在 [選擇您的資料庫物件] 對話方塊的 [ **資料表**   ] 節點底下，選取 [ **Person**]、[ **StudentGrade**] 和 [ **課程**   ] 資料表。
7.  選取位於 **預存程式和**函式節點附注下的 **GetStudentGradesForCourse**函式，該函式   會從 Visual Studio 2012 開始，Entity Designer 可讓您將預存程式和函式分批匯入
8.  按一下 **[完成]**
9.  隨即會顯示 Entity Designer，其提供用來編輯模型的設計介面。 在 [ **選擇您的資料庫物件**] 對話方塊中選取的所有物件   都會加入至模型中。
10. 根據預設，每個匯入的預存程式或函數的結果圖形都會自動成為實體模型中的新複雜型別。 但是，我們想要將 GetStudentGradesForCourse 函式的結果對應至 StudentGrade 實體：以滑鼠右鍵按一下設計介面，然後選取 [模型瀏覽器] 中的 [ **模型瀏覽器**] **，選取 [** 函式匯入]，然後按兩下 [編輯函式匯入] 對話方塊中的 **GetStudentGradesForCourse**函式，選取 **實體**，   然後選擇 **StudentGrade**

## <a name="persist-and-retrieve-data"></a>保存和取出資料

開啟定義主要方法的檔案。 將下列程式碼新增至 Main 函數。

下列程式碼示範如何建立使用資料表值函式的查詢。 此查詢會將結果投射至匿名型別，其中包含相關的課程標題和等級大於或等於3.5 的相關學生。

``` csharp
using (var context = new SchoolEntities())
{
    var CourseID = 4022;
    var Grade = 3.5M;

    // Return all the best students in the Microeconomics class.
    var students = from s in context.GetStudentGradesForCourse(CourseID)
                            where s.Grade >= Grade
                            select new
                            {
                                s.Person,
                                s.Course.Title
                            };

    foreach (var result in students)
    {
        Console.WriteLine(
            "Couse: {0}, Student: {1} {2}",
            result.Title,  
            result.Person.FirstName,  
            result.Person.LastName);
    }
}
```

編譯並執行應用程式。 此程式會產生下列輸出：

```console
Couse: Microeconomics, Student: Arturo Anand
Couse: Microeconomics, Student: Carson Bryant
```

## <a name="summary"></a>摘要

在這個逐步解說中，我們探討了如何使用 Entity Framework Designer (Tvf) 來對應資料表值函式。 它也示範了如何從 LINQ 查詢呼叫 TVF。
