---
title: 資料表對應-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c807aa4c-7845-443d-b8d0-bfc9b42691a3
uid: core/modeling/relational/tables
ms.openlocfilehash: 474c49aca4c65cd5d58b184b1f3c2d30e7abff84
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656095"
---
# <a name="table-mapping"></a>資料表對應

> [!NOTE]  
> 本節中的組態一般適用於關聯式資料庫。 當您因共用 *Microsoft.EntityFrameworkCore.Relational* 套件而安裝關聯式資料庫提供者時，這裡顯示的擴充方法會變成可用。

資料表對應會識別應該在資料庫中從查詢和儲存哪些資料表資料。

## <a name="conventions"></a>慣例

依照慣例，每個實體都會設定成對應至與 `DbSet<TEntity>` 屬性同名的資料表，該屬性會在衍生內容上公開實體。 如果指定的實體未包含任何 `DbSet<TEntity>`，則會使用類別名稱。

## <a name="data-annotations"></a>資料註釋

您可以使用資料批註來設定型別所對應的資料表。

``` csharp
using System.ComponentModel.DataAnnotations.Schema;

[Table("blogs")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

您也可以指定資料表所屬的架構。

``` csharp
[Table("blogs", Schema = "blogging")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a>Fluent API

您可以使用流暢的 API 來設定型別對應的資料表。

``` csharp
using Microsoft.EntityFrameworkCore;

class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .ToTable("blogs");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

您也可以指定資料表所屬的架構。

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/TableAndSchema.cs?name=Table&highlight=2)]
