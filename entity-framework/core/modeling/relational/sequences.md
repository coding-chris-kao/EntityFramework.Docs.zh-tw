---
title: 序列-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 94f81a92-3c72-4e14-912a-f99310374e42
uid: core/modeling/relational/sequences
ms.openlocfilehash: b810caaffa329bb5ad6f3486145d0ade9287eada
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656120"
---
# <a name="sequences"></a>序列

> [!NOTE]  
> 本節中的組態一般適用於關聯式資料庫。 當您因共用 *Microsoft.EntityFrameworkCore.Relational* 套件而安裝關聯式資料庫提供者時，這裡顯示的擴充方法會變成可用。

序列會在資料庫中產生連續的數值。 順序不會與特定資料表相關聯。

## <a name="conventions"></a>慣例

依照慣例，順序不會在模型中引進。

## <a name="data-annotations"></a>資料註釋

您無法使用資料批註來設定順序。

## <a name="fluent-api"></a>Fluent API

您可以使用流暢的 API 在模型中建立序列。

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/Sequence.cs?name=Model&highlight=7)]

您也可以設定序列的其他層面，例如其架構、開始值和增量。

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/SequenceConfigured.cs?name=Sequence&highlight=7,8,9)]

一旦引進序列之後，您就可以用它來產生模型中的屬性值。 例如，您可以使用[預設值](default-values.md)來插入序列中的下一個值。

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/SequenceUsed.cs?name=Default&highlight=13)]
