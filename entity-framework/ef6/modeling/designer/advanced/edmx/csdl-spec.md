---
title: CSDL 規格-EF6
description: Entity Framework 6 中的 CSDL 規格
author: divega
ms.date: 10/23/2016
ms.assetid: c54255f4-253f-49eb-bec8-ad7927ac2fa3
uid: ef6/modeling/designer/advanced/edmx/csdl-spec
ms.openlocfilehash: 0137eba39bd719b8987dad2eb16645475a00d94c
ms.sourcegitcommit: 7c3939504bb9da3f46bea3443638b808c04227c2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/09/2020
ms.locfileid: "89620633"
---
# <a name="csdl-specification"></a>CSDL 規格
概念結構定義語言 (CSDL) 是 XML架構語言，可描述組成資料驅動應用程式之概念模型的實體、關聯性和函式。 Entity Framework 或 WCF Data Services 可以使用這個概念模型。 Entity Framework 使用 CSDL 描述的中繼資料，將概念模型中定義的實體和關聯性對應至資料來源。 如需詳細資訊，請參閱 [SSDL 規格](xref:ef6/modeling/designer/advanced/edmx/ssdl-spec) 和 [MSL 規格](xref:ef6/modeling/designer/advanced/edmx/msl-spec)。

CSDL 是實體資料模型的 Entity Framework 執行。

在 Entity Framework 應用程式中，概念模型中繼資料是從 csdl 檔案載入， (以 CSDL) 寫入至 EdmItemCollection 的實例中，而且可以使用 system.string 類別中的方法來加以存取。 Entity Framework 使用概念模型中繼資料，將針對概念模型的查詢轉譯成資料來源特有的命令。

EF 設計工具會在設計階段將概念模型資訊儲存在 .edmx 檔案中。 在組建階段，EF 設計工具會使用 .edmx 檔案中的資訊，建立在執行時間 Entity Framework 所需的 csdl 檔案。

CSDL 的版本可藉由 XML 命名空間來區別。

| CSDL 版本 | XML 命名空間                                |
|:-------------|:---------------------------------------------|
| CSDL v1      | https://schemas.microsoft.com/ado/2006/04/edm |
| CSDL v2      | https://schemas.microsoft.com/ado/2008/09/edm |
| CSDL v3      | https://schemas.microsoft.com/ado/2009/11/edm |

 
## <a name="association-element-csdl"></a>Association 項目 (CSDL)

Association 元素會定義兩個實體類型之間的 **關聯** 性。 關聯 (Association) 必須指定關聯性 (relationship) 中的相關實體類型，以及關聯性每一端可能的實體類型數量 (也就是「多重性」)。 關聯 end 的多重性可以有一個 (1) 、零個或一個 (0 ..1) 或許多 () 中的值。 \* 這項資訊會在兩個 End 子項目中指定。

關聯其中一端的實體類型執行個體可透過導覽屬性或外部索引鍵來存取 (若在實體類型上公開)。

在應用程式中，關聯的執行個體代表實體類型之間的特定關聯。 關聯執行個體會在邏輯上群組於關聯集中。

**Association**元素可以依照所列的順序， (下列子項目) ：

-   Documentation (零或一個項目)
-   End (兩個項目)
-   ReferentialConstraint (零或一個項目)
-   Annotation 項目 (零或多個項目)

### <a name="applicable-attributes"></a>適用的屬性

下表說明可套用至 **Association** 專案的屬性。

| 屬性名稱 | 必要 | 值                        |
|:---------------|:------------|:-----------------------------|
| **名稱**       | 是         | 關聯的名稱。 |

 

> [!NOTE]
> 您可以將任何數目的注釋屬性 (自訂 XML 屬性，) 可以套用至 **Association** 元素。 不過，自訂屬性不可屬於任何 XML 命名空間，這是保留給 CSDL 之用。 任兩個自訂屬性的完整名稱不能相同。

 

### <a name="example"></a>範例

下列範例顯示在**Customer**和**Order**實體類型上未公開外鍵時，定義 **>customerorders**關聯的**association**元素。 關聯的每個**端點**的**多重性**值表示許多**訂單**可以與**客戶**相關聯，但只有一個**客戶**可以與**訂單**相關聯。 此外， **OnDelete**元素表示如果刪除**客戶**，則會刪除所有與特定**客戶**相關的**訂單**，並將其載入 ObjectCoNtext。

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" >
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
 </Association>
```
 

下列範例顯示在**Customer**和**Order**實體類型上公開外鍵時，定義 **>customerorders**關聯的**association**元素。 公開外鍵之後，實體之間的關聯性就會以 **ReferentialConstraint** 元素來管理。 對應此關聯與資料來源時，不需要對應的 AssociationSetMapping 項目。

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" >
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
   <ReferentialConstraint>
        <Principal Role="Customer">
            <PropertyRef Name="Id" />
        </Principal>
        <Dependent Role="Order">
             <PropertyRef Name="CustomerId" />
         </Dependent>
   </ReferentialConstraint>
 </Association>
```
 

 

## <a name="associationset-element-csdl"></a>AssociationSet 項目 (CSDL)

概念結構定義語言 (CSDL) 中的 **AssociationSet** 元素是相同類型之關聯實例的邏輯容器。 關聯集提供群組關聯執行個體的定義，執行個體才能對應至資料來源。  

**AssociationSet**專案可以有下列子項目， (依列出的順序) ：

-   Documentation (可允許零或一個項目)
-   End (只需兩個項目)
-   Annotation 項目 (允許零或多個項目)

**Association**屬性會指定關聯集包含的關聯類型。 組成關聯集端點的實體集會以剛好兩 **個子項目** 來指定。

### <a name="applicable-attributes"></a>適用的屬性

下表說明可套用至 **AssociationSet** 專案的屬性。

| 屬性名稱  | 必要 | 值                                                                                                                                                             |
|:----------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **名稱**        | 是         | 實體集的名稱。 **Name**屬性的值不能與**Association**屬性的值相同。                                 |
| **協會** | 是         | 關聯執行個體 (由關聯集包含) 之關聯的完整名稱。 關聯必須存在與關聯集相同的命名空間中。 |

 

> [!NOTE]
> 任何數目的注釋屬性 (自訂 XML 屬性) 都可以套用至 **AssociationSet** 元素。 不過，自訂屬性不可屬於任何 XML 命名空間，這是保留給 CSDL 之用。 任兩個自訂屬性的完整名稱不能相同。

 

### <a name="example"></a>範例

下列範例顯示具有兩個**AssociationSet**元素的**EntityContainer**元素：

``` xml
 <EntityContainer Name="BooksContainer" >
   <EntitySet Name="Books" EntityType="BooksModel.Book" />
   <EntitySet Name="Publishers" EntityType="BooksModel.Publisher" />
   <EntitySet Name="Authors" EntityType="BooksModel.Author" />
   <AssociationSet Name="PublishedBy" Association="BooksModel.PublishedBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Publisher" EntitySet="Publishers" />
   </AssociationSet>
   <AssociationSet Name="WrittenBy" Association="BooksModel.WrittenBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Author" EntitySet="Authors" />
   </AssociationSet>
 </EntityContainer>
```
 

 

## <a name="collectiontype-element-csdl"></a>CollectionType 項目 (CSDL)

概念結構定義語言 (CSDL 中的 **CollectionType** 元素) 指定函式參數或函式傳回型別為集合。 **CollectionType**元素可以是 Parameter 專案的子系，或 ReturnType (函式) 專案。 您可以使用 **type** 屬性或下列其中一個子項目來指定集合的類型：

-   **CollectionType**
-   ReferenceType
-   RowType
-   TypeRef

> [!NOTE]
> 如果集合的型別同時以 **型** 別屬性和子專案指定，模型就不會驗證。

 

### <a name="applicable-attributes"></a>適用的屬性

下表說明可套用至 **CollectionType** 專案的屬性。 請注意， **DefaultValue**、 **MaxLength**、 **FixedLength**、 **Precision**、 **Scale**、 **Unicode**和定 **序** 屬性只適用于 **EDMSimpleTypes**的集合。

| 屬性名稱                                                          | 必要 | 值                                                                                                                                                                                                                            |
|:------------------------------------------------------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **類型**                                                                | 否          | 集合的型別。                                                                                                                                                                                                      |
| **可為 Null**                                                            | 否          | **True** (預設值) 或 **False**，取決於屬性是否可以具有 null 值。 <br/> [!NOTE]                                                                                                                 |
| > 在 CSDL v1 中，複雜型別屬性必須具有 `Nullable="False"` 。 |             |                                                                                                                                                                                                                                  |
| **值**                                                        | 否          | 屬性的預設值。                                                                                                                                                                                               |
| **MaxLength**                                                           | 否          | 屬性值的長度上限                                                                                                                                                                                        |
| **FixedLength**                                                         | 否          | **True** 或 **False** ，取決於屬性值是否會儲存為固定長度字串。                                                                                                                           |
| **有效位數**                                                           | 否          | 屬性值的有效位數。                                                                                                                                                                                             |
| **縮放比例**                                                               | 否          | 屬性值的小數位數。                                                                                                                                                                                                 |
| **SRID**                                                                | 否          | 空間系統參考識別碼。 只對空間類型的屬性有效。如需詳細資訊，請參閱 [SRID](https://en.wikipedia.org/wiki/SRID) 和 [SRID (SQL Server) ](https://msdn.microsoft.com/library/bb964707.aspx) |
| **Unicode**                                                             | 否          | **True** 或 **False** ，取決於屬性值是否會儲存為 Unicode 字串。                                                                                                                                |
| **定序**                                                           | 否          | 指定要在資料來源使用之定序的字串。                                                                                                                                                    |

 

> [!NOTE]
> 任何數目的注釋屬性 (自訂 XML 屬性) 都可以套用至 **CollectionType** 元素。 不過，自訂屬性不可屬於任何 XML 命名空間，這是保留給 CSDL 之用。 任兩個自訂屬性的完整名稱不能相同。

 

### <a name="example"></a>範例

下列範例示範模型定義的函式，該函式會使用 **CollectionType** 專案來指定函式傳回 **Person** 實體類型的集合 (如) **ElementType** 屬性所指定。

``` xml
 <Function Name="LastNamesAfter">
        <Parameter Name="someString" Type="Edm.String"/>
        <ReturnType>
             <CollectionType  ElementType="SchoolModel.Person"/>
        </ReturnType>
        <DefiningExpression>
             SELECT VALUE p
             FROM SchoolEntities.People AS p
             WHERE p.LastName >= someString
        </DefiningExpression>
 </Function>
```
 

下列範例示範模型定義函式，該函式會使用 **CollectionType** 專案來指定函式傳回資料列集合 (如 **RowType** 元素) 中所指定。

``` xml
 <Function Name="LastNamesAfter">
   <Parameter Name="someString" Type="Edm.String" />
   <ReturnType>
    <CollectionType>
      <RowType>
        <Property Name="FirstName" Type="Edm.String" Nullable="false" />
        <Property Name="LastName" Type="Edm.String" Nullable="false" />
      </RowType>
    </CollectionType>
   </ReturnType>
   <DefiningExpression>
             SELECT VALUE ROW(p.FirstName, p.LastName)
             FROM SchoolEntities.People AS p
             WHERE p.LastName &gt;= somestring
   </DefiningExpression>
 </Function>
```
 

下列範例示範模型定義的函式，該函式會使用 **CollectionType** 專案來指定函式接受做為參數的 **部門** 實體類型集合。

``` xml
 <Function Name="GetAvgBudget">
      <Parameter Name="Departments">
          <CollectionType>
             <TypeRef Type="SchoolModel.Department"/>
          </CollectionType>
           </Parameter>
       <ReturnType Type="Collection(Edm.Decimal)"/>
       <DefiningExpression>
             SELECT VALUE AVG(d.Budget) FROM Departments AS d
       </DefiningExpression>
 </Function>
```
 

 

## <a name="complextype-element-csdl"></a>ComplexType 項目 (CSDL)

**ComplexType**元素定義由**EdmSimpleType**屬性或其他複雜類型組成的資料結構。複雜類型可以是實體類型的屬性或另一個複雜類型的屬性。 複雜類型與實體類型相似之處在於複雜類型會定義資料。 不過，複雜型別和實體類型之間還是有些重大的差異：

-   複雜型別不具有識別 (或索引鍵)，因此無法獨立存在。 複雜型別只能以實體類型或其他複雜型別的屬性形式存在。
-   複雜類型不可參與關聯。 關聯的兩個端點皆不可為複雜型別，因此不可為複雜型別定義導覽屬性。
-   雖然複雜型別的純量屬性可能每個都設為 null，但複雜型別屬性不可以有 null 值。

**ComplexType**元素可以依照所列的順序)  (下列子項目：

-   Documentation (零或一個項目)
-   Property (零或多個項目)
-   Annotation 項目 (零或多個項目)

下表說明可套用至 **ComplexType** 專案的屬性。

| 屬性名稱                                                                                                 | 必要 | 值                                                                                                                                                                               |
|:---------------------------------------------------------------------------------------------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 名稱                                                                                                           | 是         | 複雜類型的名稱。 複雜類型的名稱不可與其他複雜類型、實體類型或模型範圍內之關聯的名稱相同。 |
| BaseType                                                                                                       | 否          | 其他複雜類型的名稱是即將定義之複雜類型的基底型別。 <br/> [!NOTE]                                                                     |
| > 這個屬性在 CSDL v1 中不適用。 該版本不支援複雜類型的繼承。 |             |                                                                                                                                                                                     |
| 摘要                                                                                                       | 否          | **True** 或 **False** (預設值) ，視複雜型別是否為抽象型別而定。 <br/> [!NOTE]                                                                  |
| > 這個屬性在 CSDL v1 中不適用。 該版本的複雜類型不可以是抽象型別。         |             |                                                                                                                                                                                     |

 

> [!NOTE]
> 任何數目的注釋屬性 (自訂 XML 屬性) 都可以套用至 **ComplexType** 元素。 不過，自訂屬性不可屬於任何 XML 命名空間，這是保留給 CSDL 之用。 任兩個自訂屬性的完整名稱不能相同。

 

### <a name="example"></a>範例

下列範例顯示具有**EdmSimpleType**屬性**StreetAddress**、 **City**、 **StateOrProvince**、 **Country**和**郵遞區號**的複雜類型**Address**。

``` xml
 <ComplexType Name="Address" >
   <Property Type="String" Name="StreetAddress" Nullable="false" />
   <Property Type="String" Name="City" Nullable="false" />
   <Property Type="String" Name="StateOrProvince" Nullable="false" />
   <Property Type="String" Name="Country" Nullable="false" />
   <Property Type="String" Name="PostalCode" Nullable="false" />
 </ComplexType>
```
 

若要定義上述的複雜類型 **位址** () 為實體類型的屬性，您必須在實體類型定義中宣告屬性型別。 下列範例會在 (**發行者**) 的實體類型上，將**Address**屬性顯示為複雜類型：

``` xml
 <EntityType Name="Publisher">
       <Key>
         <PropertyRef Name="Id" />
       </Key>
       <Property Type="Int32" Name="Id" Nullable="false" />
       <Property Type="String" Name="Name" Nullable="false" />
       <Property Type="BooksModel.Address" Name="Address" Nullable="false" />
       <NavigationProperty Name="Books" Relationship="BooksModel.PublishedBy"
                           FromRole="Publisher" ToRole="Book" />
     </EntityType>
```
 

 

## <a name="definingexpression-element-csdl"></a>DefiningExpression 項目 (CSDL)

概念結構定義語言 (CSDL) 中的 **DefiningExpression** 元素包含定義概念模型中之函式的 Entity SQL 運算式。  

> [!NOTE]
> 基於驗證目的， **DefiningExpression** 元素可以包含任意內容。 但是，如果 **DefiningExpression** 的元素不包含有效的 Entity SQL，Entity Framework 會在執行時間擲回例外狀況。

 

### <a name="applicable-attributes"></a>適用的屬性

任何數目的注釋屬性 (自訂 XML 屬性) 都可以套用至 **DefiningExpression** 元素。 不過，自訂屬性不可屬於任何 XML 命名空間，這是保留給 CSDL 之用。 任兩個自訂屬性的完整名稱不能相同。

### <a name="example"></a>範例

下列範例會使用 **DefiningExpression** 專案來定義函式，該函式會傳回發行書籍之後的年數。 **DefiningExpression**元素的內容是以 Entity SQL 撰寫。

``` xml
 <Function Name="GetYearsInPrint" ReturnType="Edm.Int32" >
       <Parameter Name="book" Type="BooksModel.Book" />
       <DefiningExpression>
         Year(CurrentDateTime()) - Year(cast(book.PublishedDate as DateTime))
       </DefiningExpression>
     </Function>
```
 

 

## <a name="dependent-element-csdl"></a>Dependent 項目 (CSDL)

概念結構定義語言 (CSDL) 中的 **相依** 元素是 ReferentialConstraint 專案的子專案，並定義參考條件約束的相依端點。 **ReferentialConstraint**元素會定義類似于關係資料庫中的參考完整性條件約束的功能。 同樣地，資料庫資料表的資料行 (或多個資料行) 可以參考其他資料表的主索引鍵，實體類型的屬性 (或多個屬性) 可以參考其他實體類型的實體索引鍵。 參考的實體類型稱為條件約束的 *主要端點* 。 參考主體端的實體類型稱為條件約束的 *相依端點* 。 **PropertyRef** 元素可用來指定參考主體端的索引鍵。

相依專案可以有下列子項目， (**依** 列出的順序) ：

-   PropertyRef (一或多個項目)
-   Annotation 項目 (零或多個項目)

### <a name="applicable-attributes"></a>適用的屬性

下表說明可套用至 **相依** 專案的屬性。

| 屬性名稱 | 必要 | 值                                                                |
|:---------------|:------------|:---------------------------------------------------------------------|
| **角色**       | 是         | 位於關聯之相依端點的實體類型名稱。 |

 

> [!NOTE]
> 任何數目的注釋屬性 (自訂 XML 屬性) 都可以套用至 **相依** 元素。 不過，自訂屬性不可屬於任何 XML 命名空間，這是保留給 CSDL 之用。 任兩個自訂屬性的完整名稱不能相同。

 

### <a name="example"></a>範例

下列範例顯示在**PublishedBy**關聯定義中使用的**ReferentialConstraint**元素。 **Book**實體類型的**PublisherId**屬性組成參考條件約束的相依端點。

``` xml
 <Association Name="PublishedBy">
   <End Type="BooksModel.Book" Role="Book" Multiplicity="*" >
   </End>
   <End Type="BooksModel.Publisher" Role="Publisher" Multiplicity="1" />
   <ReferentialConstraint>
     <Principal Role="Publisher">
       <PropertyRef Name="Id" />
     </Principal>
     <Dependent Role="Book">
       <PropertyRef Name="PublisherId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```
 

 

## <a name="documentation-element-csdl"></a>Documentation 項目 (CSDL)

概念結構定義語言 (CSDL) 中的 **檔** 元素可用來提供在父元素中定義之物件的相關資訊。 在 .edmx 檔案中， **當檔專案** 是專案的子系時，在 EF 設計工具的設計介面上會顯示為物件 (例如實體、關聯或屬性) 時， **檔** 元素的內容會出現在物件的 [Visual Studio **屬性** ] 視窗中。

**檔**元素可以依照列出的順序， (下列子項目) ：

-   **摘要**：父元素的簡短描述。 (零或一個項目)
-   **LongDescription**：父元素的廣泛描述。 (零或一個項目)
-   Annotation 項目. (零或多個項目)

### <a name="applicable-attributes"></a>適用的屬性

任何數目的注釋屬性 (自訂 XML 屬性) 都可以套用至 **檔** 元素。 不過，自訂屬性不可屬於任何 XML 命名空間，這是保留給 CSDL 之用。 任兩個自訂屬性的完整名稱不能相同。

### <a name="example"></a>範例

下列範例會將 **檔** 專案顯示為 EntityType 元素的子項目。 如果下面的程式碼片段位於 .edmx 檔的 CSDL 內容中，則當您按一下實體類型時， **摘要** 和 **LongDescription** 專案的內容就會出現在 Visual Studio 的 [ **屬性** ] 視窗中 `Customer` 。

``` xml
 <EntityType Name="Customer">
    <Documentation>
      <Summary>Summary here.</Summary>
      <LongDescription>Long description here.</LongDescription>
    </Documentation>
    <Key>
      <PropertyRef Name="CustomerId" />
    </Key>
    <Property Type="Int32" Name="CustomerId" Nullable="false" />
    <Property Type="String" Name="Name" Nullable="false" />
 </EntityType>
```
 

 

## <a name="end-element-csdl"></a>End 項目 (CSDL)

概念結構定義語言 (CSDL) 的 **End** 元素可以是 Association 元素或 AssociationSet 專案的子系。 在每個案例中， **End** 元素的角色都不同，而且適用的屬性會不同。

### <a name="end-element-as-a-child-of-the-association-element"></a>End 項目為 Association 項目的子項目

**End**專案 (為**association**專案的子系) 識別關聯一端的實體類型，以及可存在於關聯之一端的實體類型實例數目。 關聯 End 會定義為關聯的部分。關聯必須具有兩個關聯 End。 關聯其中一端的實體型別執行個體可透過巡覽屬性或外部索引鍵來存取 (若在實體型別上公開)。  

**End**專案可以有下列子項目， (依所列的順序) ：

-   Documentation (零或一個項目)
-   OnDelete (零或一個項目)
-   Annotation 項目 (零或多個項目)

#### <a name="applicable-attributes"></a>適用的屬性

下表描述可套用至 **End** 專案的屬性（當它是 **Association** 元素的子系時）。

| 屬性名稱   | 必要 | 值                                                                                                                                                                                                                                                                                                                                                                                                              |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **型別**         | 是         | 其中一個關聯 End 的實體類型名稱。                                                                                                                                                                                                                                                                                                                                                         |
| **角色**         | 否          | 關聯 End 的名稱。 如果未提供任何名稱，則會使用關聯 End 上之實體類型的名稱。                                                                                                                                                                                                                                                                                           |
| **多重性** | 是         | **1**、 **0 或 1**，或 **\*** 視關聯的結尾可以是實體類型實例的數目而定。 <br/> **1** 表示關聯 end 只存在一個實體類型實例。 <br/> **0 ..1** 表示關聯 end 有零個或一個實體類型實例。 <br/> **\*** 指出關聯 end 存在零個、一個或多個實體類型實例。 |

 

> [!NOTE]
> 任何數目的注釋屬性 (自訂 XML 屬性) 都可以套用至 **End** 元素。 不過，自訂屬性不可屬於任何 XML 命名空間，這是保留給 CSDL 之用。 任兩個自訂屬性的完整名稱不能相同。

 

#### <a name="example"></a>範例

下列範例會顯示定義 **>customerorders**關聯的**association**元素。 關聯的每個**端點**的**多重性**值表示許多**訂單**可以與**客戶**相關聯，但只有一個**客戶**可以與**訂單**相關聯。 此外， **OnDelete**元素表示如果刪除**客戶**，則會刪除所有與特定**客戶**相關的訂單，以及已載入 ObjectCoNtext 的所有**訂單**。

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" />
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*">
         <OnDelete Action="Cascade" />
   </End>
 </Association>
```
 

### <a name="end-element-as-a-child-of-the-associationset-element"></a>End 項目為 AssociationSet 項目的子項目

**End**元素會指定關聯集的一端。 **AssociationSet**元素必須包含兩個**End**元素。 **End**元素中包含的資訊會用於將關聯集對應至資料來源。

**End**專案可以有下列子項目， (依所列的順序) ：

-   Documentation (零或一個項目)
-   Annotation 項目 (零或多個項目)

> [!NOTE]
> Annotation 項目必須出現在所有其他子項目之後。 只有 CSDL v2 和更新版本中才允許注釋元素。

 

#### <a name="applicable-attributes"></a>適用的屬性

下表描述可套用至 **End** 專案的屬性（當它是 **AssociationSet** 專案的子專案時）。

| 屬性名稱 | 必要 | 值                                                                                                                                                                                                                 |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **EntitySet**  | 是         | 定義父**AssociationSet**專案一端的**EntitySet**元素名稱。 **EntitySet**元素必須在與父**AssociationSet**元素相同的實體容器中定義。 |
| **角色**       | 否          | 關聯集 End 的名稱。 如果未使用 **Role** 屬性，關聯集的名稱 end 將會是實體集的名稱。                                                                   |

 

> [!NOTE]
> 任何數目的注釋屬性 (自訂 XML 屬性) 都可以套用至 **End** 元素。 不過，自訂屬性不可屬於任何 XML 命名空間，這是保留給 CSDL 之用。 任兩個自訂屬性的完整名稱不能相同。

 

#### <a name="example"></a>範例

下列範例顯示具有兩個**AssociationSet**專案的**EntityContainer**元素，每個元素都有兩個**End**元素：

``` xml
 <EntityContainer Name="BooksContainer" >
   <EntitySet Name="Books" EntityType="BooksModel.Book" />
   <EntitySet Name="Publishers" EntityType="BooksModel.Publisher" />
   <EntitySet Name="Authors" EntityType="BooksModel.Author" />
   <AssociationSet Name="PublishedBy" Association="BooksModel.PublishedBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Publisher" EntitySet="Publishers" />
   </AssociationSet>
   <AssociationSet Name="WrittenBy" Association="BooksModel.WrittenBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Author" EntitySet="Authors" />
   </AssociationSet>
 </EntityContainer>
```
 

 

## <a name="entitycontainer-element-csdl"></a>EntityContainer 項目 (CSDL)

概念結構定義語言 (CSDL) 中的 **EntityContainer** 元素是實體集、關聯集和函式匯入的邏輯容器。 概念模型實體容器透過 EntityContainerMapping 項目對應於儲存體模型實體容器。 儲存體模型實體容器描述資料的結構：實體集描述資料表、關聯集描述外部索引建條件約束，而函式匯入則描述資料庫中的預存程序。

**EntityContainer**元素可以有零個或一個檔元素。 如果有 **檔** 元素存在，它必須在所有 **EntitySet**、 **AssociationSet**和 **FunctionImport** 元素之前。

**EntityContainer**元素可以有零或多個下列子項目， (依所列的順序) ：

-   EntitySet
-   AssociationSet
-   FunctionImport
-   Annotation 項目

您可以擴充 **EntityContainer** 元素，以包含相同命名空間內另一個 **EntityContainer** 的內容。 若要加入另一個 **entitycontainer**的內容，請在參考的 **entitycontainer** 專案中，將 [ **擴充** 屬性] 的值設定為您想要包含之 **EntityContainer** 元素的名稱。 包含之 **entitycontainer** 專案的所有子項目都會被視為參考 **EntityContainer** 元素的子項目。

### <a name="applicable-attributes"></a>適用的屬性

下表說明可套用至 **Using** 元素的屬性。

| 屬性名稱 | 必要 | 值                                                           |
|:---------------|:------------|:----------------------------------------------------------------|
| **名稱**       | 是         | 實體容器的名稱。                               |
| **延伸**    | 否          | 相同命名空間中另一個實體容器的名稱。 |

 

> [!NOTE]
> 任何數目的注釋屬性 (自訂 XML 屬性) 都可以套用至 **EntityContainer** 元素。 不過，自訂屬性不可屬於任何 XML 命名空間，這是保留給 CSDL 之用。 任兩個自訂屬性的完整名稱不能相同。

 

### <a name="example"></a>範例

下列範例顯示定義三個實體集和兩個關聯集的 **EntityContainer** 元素。

``` xml
 <EntityContainer Name="BooksContainer" >
   <EntitySet Name="Books" EntityType="BooksModel.Book" />
   <EntitySet Name="Publishers" EntityType="BooksModel.Publisher" />
   <EntitySet Name="Authors" EntityType="BooksModel.Author" />
   <AssociationSet Name="PublishedBy" Association="BooksModel.PublishedBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Publisher" EntitySet="Publishers" />
   </AssociationSet>
   <AssociationSet Name="WrittenBy" Association="BooksModel.WrittenBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Author" EntitySet="Authors" />
   </AssociationSet>
 </EntityContainer>
```
 

 

## <a name="entityset-element-csdl"></a>EntitySet 項目 (CSDL)

概念結構定義語言中的 **EntitySet** 元素是實體類型實例的邏輯容器，以及衍生自該實體類型之任何類型的實例。 實體類型和實體集之間的關聯性，類似於關聯式資料庫中資料列與資料表的關係。 實體類型和資料列一樣可以定義相關的資料集，而實體集則和資料表一樣可以包含該定義的執行個體。 實體集提供群組實體類型執行個體的建構，以便將它們對應至資料來源中的相關資料結構。  

您可以為特定的實體類型定義多個實體集。

> [!NOTE]
> EF 設計工具不支援包含每個類型之多個實體集的概念模型。

 

**EntitySet**元素可依所列的順序)  (下列子項目：

-   Documentation 項目 (允許零或一個項目)
-   Annotation 項目 (允許零或多個項目)

### <a name="applicable-attributes"></a>適用的屬性

下表說明可套用至 **EntitySet** 專案的屬性。

| 屬性名稱 | 必要 | 值                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------|
| **名稱**       | 是         | 實體集的名稱。                                                              |
| **EntityType** | 是         | 實體類型 (實體集包含其執行個體) 的完整名稱。 |

 

> [!NOTE]
> 任何數目的注釋屬性 (自訂 XML 屬性) 都可以套用至 **EntitySet** 元素。 不過，自訂屬性不可屬於任何 XML 命名空間，這是保留給 CSDL 之用。 任兩個自訂屬性的完整名稱不能相同。

 

### <a name="example"></a>範例

下列範例顯示具有三個**EntitySet**元素的**EntityContainer**元素：

``` xml
 <EntityContainer Name="BooksContainer" >
   <EntitySet Name="Books" EntityType="BooksModel.Book" />
   <EntitySet Name="Publishers" EntityType="BooksModel.Publisher" />
   <EntitySet Name="Authors" EntityType="BooksModel.Author" />
   <AssociationSet Name="PublishedBy" Association="BooksModel.PublishedBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Publisher" EntitySet="Publishers" />
   </AssociationSet>
   <AssociationSet Name="WrittenBy" Association="BooksModel.WrittenBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Author" EntitySet="Authors" />
   </AssociationSet>
 </EntityContainer>
```
 

您可以定義每個類型的多重實體集 (MEST)。 下列範例會針對 **Book** 實體類型定義具有兩個實體集的實體容器：

``` xml
 <EntityContainer Name="BooksContainer" >
   <EntitySet Name="Books" EntityType="BooksModel.Book" />
   <EntitySet Name="FictionBooks" EntityType="BooksModel.Book" />
   <EntitySet Name="Publishers" EntityType="BooksModel.Publisher" />
   <EntitySet Name="Authors" EntityType="BooksModel.Author" />
   <AssociationSet Name="PublishedBy" Association="BooksModel.PublishedBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Publisher" EntitySet="Publishers" />
   </AssociationSet>
   <AssociationSet Name="BookAuthor" Association="BooksModel.BookAuthor">
     <End Role="Book" EntitySet="Books" />
     <End Role="Author" EntitySet="Authors" />
   </AssociationSet>
 </EntityContainer>
```
 

 

## <a name="entitytype-element-csdl"></a>EntityType 項目 (CSDL)

**EntityType**元素代表概念模型中最上層概念的結構，例如客戶或訂單。 實體類型是應用程式中實體類型之執行個體的範本。 每個範本包含下列資訊：

-   唯一名稱。 (必要項。)
-   實體索引鍵是由一個或多個屬性定義。 (必要項。)
-   包含資料的屬性。 (選擇性。)
-   導覽屬性允許從關聯的一端巡覽至另一端。 (選擇性。)

在應用程式中，實體類型的執行個體代表特定的物件 (例如特定的客戶或訂單)。 實體類型的每一個執行個體都必須在實體集中有唯一的實體索引鍵。

如果兩個實體類型執行個體屬於相同類型，而且索引鍵的值也相同，則會將這兩個執行個體視為相等。

**EntityType**專案可以有下列子項目， (依所列的順序) ：

-   Documentation (零或一個項目)
-   Key(零或一個項目)
-   Property (零或多個項目)
-   NavigationProperty (零或多個項目)
-   Annotation 項目 (零或多個項目)

### <a name="applicable-attributes"></a>適用的屬性

下表說明可套用至 **EntityType** 元素的屬性。

| 屬性名稱                                                                                                                                  | 必要 | 值                                                                                            |
|:------------------------------------------------------------------------------------------------------------------------------------------------|:------------|:-------------------------------------------------------------------------------------------------|
| **名稱**                                                                                                                                        | 是         | 實體類型的名稱。                                                                     |
| **BaseType**                                                                                                                                    | 否          | 另一個實體類型的名稱，而此實體類型是要定義之實體類型的基底類型。  |
| **摘要**                                                                                                                                    | 否          | **True** 或 **False**，取決於實體型別是否為抽象型別。                 |
| **OpenType**                                                                                                                                    | 否          | **True** 或 **False** ，取決於實體型別是否為開放式實體型別。 <br/> [!NOTE] |
| > **OpenType** 屬性只適用于概念模型中定義的實體類型，這些模型會與 ADO.NET 資料服務搭配使用。 |             |                                                                                                  |

 

> [!NOTE]
> 任何數目的注釋屬性 (自訂 XML 屬性) 都可以套用至 **EntityType** 元素。 不過，自訂屬性不可屬於任何 XML 命名空間，這是保留給 CSDL 之用。 任兩個自訂屬性的完整名稱不能相同。

 

### <a name="example"></a>範例

下列範例顯示具有三個**Property**元素和兩個**NavigationProperty**元素的**EntityType**專案：

``` xml
 <EntityType Name="Book">
   <Key>
     <PropertyRef Name="ISBN" />
   </Key>
   <Property Type="String" Name="ISBN" Nullable="false" />
   <Property Type="String" Name="Title" Nullable="false" />
   <Property Type="Decimal" Name="Revision" Nullable="false" Precision="29" Scale="29" />
   <NavigationProperty Name="Publisher" Relationship="BooksModel.PublishedBy"
                       FromRole="Book" ToRole="Publisher" />
   <NavigationProperty Name="Authors" Relationship="BooksModel.WrittenBy"
                       FromRole="Book" ToRole="Author" />
 </EntityType>
```
 

 

## <a name="enumtype-element-csdl"></a>CSDL)  (EnumType 元素

**EnumType**元素代表列舉型別。

**EnumType**元素可以依照列出的順序 (下列子項目) ：

-   Documentation (零或一個項目)
-   成員 (零或多個元素) 
-   Annotation 項目 (零或多個項目)

### <a name="applicable-attributes"></a>適用的屬性

下表說明可套用至 **EnumType** 元素的屬性。

| 屬性名稱     | 必要 | 值                                                                                                                                                                                         |
|:-------------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **名稱**           | 是         | 實體類型的名稱。                                                                                                                                                                  |
| **IsFlags**        | 否          | **True** 或 **False**，取決於列舉型別是否可以當做一組旗標使用。 預設值為**False。**                                                                     |
| **UnderlyingType** | 否          | **Edm**、Edm、 **Int16**、 **edm**、 **edm** 或 **edm。 SByte** 定義型別的值範圍。 列舉元素的預設基礎類型為 **Edm**。 |

 

> [!NOTE]
> 任何數目的注釋屬性 (自訂 XML 屬性) 都可以套用至 **EnumType** 元素。 不過，自訂屬性不可屬於任何 XML 命名空間，這是保留給 CSDL 之用。 任兩個自訂屬性的完整名稱不能相同。

 

### <a name="example"></a>範例

下列範例顯示具有三個**成員**元素的**EnumType**元素：

``` xml
 <EnumType Name="Color" IsFlags=”false” UnderlyingTyp=”Edm.Byte”>
   <Member Name="Red" />
   <Member Name="Green" />
   <Member Name="Blue" />
 </EntityType>
```
 

 

## <a name="function-element-csdl"></a>Function 屬性 (CSDL)

概念結構定義語言 (CSDL) 中的 **Function** 元素用來定義或宣告概念模型中的函式。 函式是使用 DefiningExpression 項目 來定義的。  

函式專案可以有下列 **子項目，** (依列出的順序) ：

-   Documentation (零或一個項目)
-   Parameter (零或多個項目)
-   DefiningExpression (零或一個項目)
-   ReturnType (函式)  (零或一個元素) 
-   Annotation 項目 (零或多個項目)

函式的傳回型別必須以 **returntype** (函式) 元素或 **returntype** 屬性指定 (請參閱下文) ，但不能同時使用這兩者。 可能的傳回型別包括任何 EdmSimpleType、實體類型、複雜類型、資料列型別或 ref 型別 (或這些類型其中之一的集合)。

### <a name="applicable-attributes"></a>適用的屬性

下表說明可套用至 **Function** 元素的屬性。

| 屬性名稱 | 必要 | 值                              |
|:---------------|:------------|:-----------------------------------|
| **名稱**       | 是         | 函式的名稱。          |
| **ReturnType** | 否          | 此函式傳回的型別。 |

 

> [!NOTE]
> 任何數目的注釋屬性 (自訂 XML 屬性) 都可以套用至 **Function** 元素。 不過，自訂屬性不可屬於任何 XML 命名空間，這是保留給 CSDL 之用。 任兩個自訂屬性的完整名稱不能相同。

 

### <a name="example"></a>範例

下列範例會使用 **function** 元素來定義函式，該函式會傳回在講師雇用之後的年數。

``` xml
 <Function Name="YearsSince" ReturnType="Edm.Int32">
   <Parameter Name="date" Type="Edm.DateTime" />
   <DefiningExpression>
     Year(CurrentDateTime()) - Year(date)
   </DefiningExpression>
 </Function>
```
 

 

## <a name="functionimport-element-csdl"></a>FunctionImport 項目 (CSDL)

概念結構定義語言中的 **FunctionImport** 元素 (CSDL) 代表資料來源中定義的函式，但可透過概念模型取得物件。 例如，儲存體模型中的 Function 項目 可以用來代表資料庫中的預存程序。 概念模型中的 **FunctionImport** 元素代表 Entity Framework 應用程式中對應的函式，並使用 FunctionImportMapping 專案對應至儲存體模型函數。 在應用程式中呼叫函式時，對應的預存程序會在資料庫中執行。

**FunctionImport**元素可依所列的順序)  (下列子項目：

-   Documentation (可允許零或一個項目)
-   Parameter (可允許零或多個項目)
-   Annotation 項目 (允許零或多個項目)
-   ReturnType (FunctionImport)  (零或多個允許的元素) 

應針對函式所接受的每個參數定義一個 **參數** 元素。

函式的傳回型別必須以 **returntype** (FunctionImport) 元素或 **returntype** 屬性指定 (請參閱下文) ，但不能同時使用這兩者。 傳回型別值必須是 EdmSimpleType、EntityType 或 ComplexType 的集合。

### <a name="applicable-attributes"></a>適用的屬性

下表說明可套用至 **FunctionImport** 元素的屬性。

| 屬性名稱   | 必要 | 值                                                                                                                                                                                                 |
|:-----------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **名稱**         | 是         | 匯入函式的名稱。                                                                                                                                                                    |
| **ReturnType**   | 否          | 函式傳回的型別。 如果函數沒有傳回值，請勿使用這個屬性。 否則，此值必須是 ComplexType、EntityType 或 EDMSimpleType 的集合。        |
| **EntitySet**    | 否          | 如果函式傳回實體類型的集合，則 **EntitySet** 的值必須是集合所屬的實體集。 否則，不得使用 **EntitySet** 屬性。 |
| **IsComposable** | 否          | 如果值設定為 true，則函數可組合 (資料表值函式) ，而且可以在 LINQ 查詢中使用。預設值為 **false**。                                                           |

 

> [!NOTE]
> 任何數目的注釋屬性 (自訂 XML 屬性) 都可以套用至 **FunctionImport** 元素。 不過，自訂屬性不可屬於任何 XML 命名空間，這是保留給 CSDL 之用。 任兩個自訂屬性的完整名稱不能相同。

 

### <a name="example"></a>範例

下列範例會顯示一個 **FunctionImport** 元素，此專案會接受一個參數並傳回實體類型的集合：

``` xml
 <FunctionImport Name="GetStudentGrades"
                 EntitySet="StudentGrade"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
        <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```
 

 

## <a name="key-element-csdl"></a>Key 項目 (CSDL)

**Key**專案是 EntityType 專案的子專案，可定義*實體索引鍵* (屬性或實體類型的一組屬性，以決定身分識別) 。 構成實體索引鍵的屬性是在設計階段選取的。 實體索引鍵屬性的值必須在執行階段的實體集中，單獨識別實體類型執行個體。 您應選取構成實體索引鍵的屬性，以保證執行個體在實體集中的唯一性。 **Key**元素會藉由參考實體類型的一或多個屬性來定義實體索引鍵。

**Key**元素可以有下列子項目：

-   PropertyRef (一或多個項目)
-   Annotation 項目 (零或多個項目)

### <a name="applicable-attributes"></a>適用的屬性

任何數目的注釋屬性 (自訂 XML 屬性) 都可以套用至索引 **鍵** 元素。 不過，自訂屬性不可屬於任何 XML 命名空間，這是保留給 CSDL 之用。 任兩個自訂屬性的完整名稱不能相同。

### <a name="example"></a>範例

下列範例會定義名為 **Book**的實體類型。 實體索引鍵是透過參考實體類型的 **ISBN** 屬性來定義。

``` xml
 <EntityType Name="Book">
   <Key>
     <PropertyRef Name="ISBN" />
   </Key>
   <Property Type="String" Name="ISBN" Nullable="false" />
   <Property Type="String" Name="Title" Nullable="false" />
   <Property Type="Decimal" Name="Revision" Nullable="false" Precision="29" Scale="29" />
   <NavigationProperty Name="Publisher" Relationship="BooksModel.PublishedBy"
                       FromRole="Book" ToRole="Publisher" />
   <NavigationProperty Name="Authors" Relationship="BooksModel.WrittenBy"
                       FromRole="Book" ToRole="Author" />
 </EntityType>
```
 

**Isbn**屬性是實體索引鍵的理想選擇，因為國際標準書籍號碼 (ISBN) 可唯一識別書籍。

下列範例顯示 (**作者**) 的實體類型，此實體索引鍵包含兩個屬性： **名稱** 和 **位址**。

``` xml
 <EntityType Name="Author">
   <Key>
     <PropertyRef Name="Name" />
     <PropertyRef Name="Address" />
   </Key>
   <Property Type="String" Name="Name" Nullable="false" />
   <Property Type="String" Name="Address" Nullable="false" />
   <NavigationProperty Name="Books" Relationship="BooksModel.WrittenBy"
                       FromRole="Author" ToRole="Book" />
 </EntityType>
```
 

使用實體索引鍵的 **名稱** 和 **位址** 是合理的選擇，因為兩個相同名稱的作者不太可能存留在相同的位址。 不過，針對實體索引鍵所做的這個選擇不能絕對保證實體集中的唯一實體索引鍵。 在此情況下，建議您加入可用於唯一識別作者的屬性，**例如作者。**

 

## <a name="member-element-csdl"></a>CSDL)  (成員元素

**Member**元素是 EnumType 專案的子專案，並定義列舉型別的成員。

### <a name="applicable-attributes"></a>適用的屬性

下表說明可套用至 **FunctionImport** 元素的屬性。

| 屬性名稱 | 必要 | 值                                                                                                                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **名稱**       | 是         | 成員的名稱。                                                                                                                                                                  |
| **ReplTest1**      | 否          | 成員的值。 依預設，第一個成員的值為0，而每個後續列舉值的值會遞增1。 有多個具有相同值的成員可以存在。 |

 

> [!NOTE]
> 任何數目的注釋屬性 (自訂 XML 屬性) 都可以套用至 **FunctionImport** 元素。 不過，自訂屬性不可屬於任何 XML 命名空間，這是保留給 CSDL 之用。 任兩個自訂屬性的完整名稱不能相同。

 

### <a name="example"></a>範例

下列範例顯示具有三個**成員**元素的**EnumType**元素：

``` xml
 <EnumType Name="Color">
   <Member Name="Red" Value=”1”/>
   <Member Name="Green" Value=”3” />
   <Member Name="Blue" Value=”5”/>
 </EntityType>
```
 

 

## <a name="navigationproperty-element-csdl"></a>NavigationProperty 項目 (CSDL)

**NavigationProperty**元素會定義導覽屬性，以提供關聯另一端的參考。 與使用 Property 項目 定義的屬性不同，導覽屬性不會定義資料的圖案和特性。 他們提供巡覽兩個實體類型間之關聯的方式。

請注意，在關聯各端點的實體類型上，導覽屬性是選擇性的。 如果您在關聯其中一個端點的實體類型上定義導覽屬性，就不必在關聯另一個端點的實體類型上定義導覽屬性。

導覽屬性傳回的資料類型是由其遠端關聯端點的多重性所判斷。 例如，假設有一個導覽屬性 **OrdersNavProp**存在於 **customer** 實體型別上，並且導覽 **customer** 和 **Order**之間的一對多關聯性。 因為導覽屬性的遠端關聯 end 具有多重性許多 (\*) ，所以其資料類型是 **Order**)  (的集合。 同樣地，如果 **CustomerNavProp**的導覽屬性存在於 **Order** 實體型別上，其資料類型就會是 **Customer** ，因為遠端 end 的多重性是一個 (1) 。

**NavigationProperty**專案可以有下列子項目， (依所列的順序) ：

-   Documentation (零或一個項目)
-   Annotation 項目 (零或多個項目)

### <a name="applicable-attributes"></a>適用的屬性

下表說明可套用至 **NavigationProperty** 專案的屬性。

| 屬性名稱   | 必要 | 值                                                                                                                                                                                                                                            |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **名稱**         | 是         | 導覽屬性的名稱。                                                                                                                                                                                                             |
| **關聯性** | 是         | 關聯的名稱在模型的範圍之內。                                                                                                                                                                                |
| **ToRole**       | 是         | 位於導覽端點的關聯端點。 **ToRole**屬性的值必須與 AssociationEnd 元素) 中定義的其中一個關聯 end (定義的其中一個**角色**屬性值相同。       |
| **FromRole**     | 是         | 開始導覽的關聯端點。 **FromRole**屬性的值必須與 AssociationEnd 元素) 中定義的其中一個關聯 end (定義的其中一個**角色**屬性值相同。 |

 

> [!NOTE]
> 任何數目的注釋屬性 (自訂 XML 屬性) 都可以套用至 **NavigationProperty** 元素。 不過，自訂屬性不可屬於任何 XML 命名空間，這是保留給 CSDL 之用。 任兩個自訂屬性的完整名稱不能相同。

 

### <a name="example"></a>範例

下列範例會使用兩個導覽屬性（ (**PublishedBy**和**WrittenBy**) ）來定義實體類型 (**Book**) ：

``` xml
 <EntityType Name="Book">
   <Key>
     <PropertyRef Name="ISBN" />
   </Key>
   <Property Type="String" Name="ISBN" Nullable="false" />
   <Property Type="String" Name="Title" Nullable="false" />
   <Property Type="Decimal" Name="Revision" Nullable="false" Precision="29" Scale="29" />
   <NavigationProperty Name="Publisher" Relationship="BooksModel.PublishedBy"
                       FromRole="Book" ToRole="Publisher" />
   <NavigationProperty Name="Authors" Relationship="BooksModel.WrittenBy"
                       FromRole="Book" ToRole="Author" />
 </EntityType>
```
 

 

## <a name="ondelete-element-csdl"></a>OnDelete 項目 (CSDL)

概念結構定義語言 (CSDL) 中的 **OnDelete** 元素會定義與關聯連接的行為。 如果 **Action** 屬性在關聯的一端設定為 **Cascade** ，則在刪除第一端的實體類型時，會刪除關聯另一端上的相關實體類型。 如果兩個實體類型之間的關聯是主鍵與主鍵之間的關聯性，則不論 **OnDelete** 規格為何，當關聯的另一端上的主體物件都被刪除時，都會刪除載入的相依物件。  

> [!NOTE]
> **OnDelete**元素只會影響應用程式的執行時間行為;它不會影響資料來源中的行為。 資料來源中定義的行為應與應用程式中定義的行為相同。

 

**OnDelete**元素可以依照列出的順序 (下列子項目) ：

-   Documentation (零或一個項目)
-   Annotation 項目 (零或多個項目)

### <a name="applicable-attributes"></a>適用的屬性

下表說明可套用至 **OnDelete** 元素的屬性。

| 屬性名稱 | 必要 | 值                                                                                                                                                                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **動作**     | 是         | **Cascade**串聯或**無**。 如果 **Cascade**，則在刪除主體實體類型時，將會刪除相依實體類型。 如果 **沒有**，當刪除主體實體類型時，不會刪除相依實體類型。 |

 

> [!NOTE]
> 您可以將任何數目的注釋屬性 (自訂 XML 屬性，) 可以套用至 **Association** 元素。 不過，自訂屬性不可屬於任何 XML 命名空間，這是保留給 CSDL 之用。 任兩個自訂屬性的完整名稱不能相同。

 

### <a name="example"></a>範例

下列範例會顯示定義 **>customerorders**關聯的**association**元素。 **OnDelete**元素指出當刪除**客戶**時，會刪除與特定**客戶**相關且已載入 ObjectCoNtext 的所有**訂單**。

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1">
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
 </Association>
```
 

 

## <a name="parameter-element-csdl"></a>Parameter 項目 (CSDL)

概念結構定義語言 (CSDL) 中的 **參數** 專案可以是 FunctionImport 元素或 Function 元素的子系。

### <a name="functionimport-element-application"></a>FunctionImport 項目的應用

**參數**專案 (為**FunctionImport**元素的子系) 用來定義 CSDL 中宣告之函式匯入的輸入和輸出參數。

**Parameter**專案可以有下列子項目， (依列出的順序) ：

-   Documentation (可允許零或一個項目)
-   Annotation 項目 (允許零或多個項目)

#### <a name="applicable-attributes"></a>適用的屬性

下表描述可套用至 **Parameter** 專案的屬性。

| 屬性名稱 | 必要 | 值                                                                                                                                                                                                                           |
|:---------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **名稱**       | 是         | 參數名稱。                                                                                                                                                                                                      |
| **型別**       | 是         | 參數類型。 此值必須是 **EDMSimpleType** 或是模型範圍內的複雜類型。                                                                                                             |
| **模式**       | 否          | **In**、 **Out**或 **InOut** ，取決於參數是輸入、輸出或輸入/輸出參數。                                                                                                                |
| **MaxLength**  | 否          | 允許的參數長度上限。                                                                                                                                                                                    |
| **有效位數**  | 否          | 參數的精確度。                                                                                                                                                                                                 |
| **縮放比例**      | 否          | 參數的小數位數。                                                                                                                                                                                                     |
| **SRID**       | 否          | 空間系統參考識別碼。 只對空間類型的參數有效。 如需詳細資訊，請參閱 [SRID](https://en.wikipedia.org/wiki/SRID) 和 [SRID (SQL Server) ](https://msdn.microsoft.com/library/bb964707.aspx)。 |

 

> [!NOTE]
> 任何數目的注釋屬性 (自訂 XML 屬性) 都可以套用至 **Parameter** 元素。 不過，自訂屬性不可屬於任何 XML 命名空間，這是保留給 CSDL 之用。 任兩個自訂屬性的完整名稱不能相同。

 

#### <a name="example"></a>範例

下列範例顯示具有一個**參數**子項目的**FunctionImport**元素。 函式接受一個輸入參數，然後傳回實體類型的集合。

``` xml
 <FunctionImport Name="GetStudentGrades"
                 EntitySet="StudentGrade"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
        <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```
 

### <a name="function-element-application"></a>Function 項目的應用

**參數**專案 (為函式**專案的子**系) 定義在概念模型中定義或宣告之函式的參數。

**Parameter**專案可以有下列子項目， (依列出的順序) ：

-   Documentation (零或一個項目)
-   CollectionType (零或一個項目)
-   ReferenceType (零或一個項目)
-   RowType (零或一個項目)

> [!NOTE]
> 只有其中一個 **CollectionType**、 **ReferenceType**或 **RowType** 專案可以是 **Property** 元素的子項目。

 

-   Annotation 項目 (允許零或多個項目)

> [!NOTE]
> Annotation 項目必須出現在所有其他子項目之後。 只有 CSDL v2 和更新版本中才允許注釋元素。

 

#### <a name="applicable-attributes"></a>適用的屬性

下表描述可套用至 **Parameter** 專案的屬性。

| 屬性名稱   | 必要 | 值                                                                                                                                                                                                                           |
|:-----------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **名稱**         | 是         | 參數名稱。                                                                                                                                                                                                      |
| **類型**         | 否          | 參數類型。 參數可以是下列任何一種型別 (或這些型別的集合)： <br/> **EdmSimpleType** <br/> Entity Type - 實體類型 <br/> 複雜類型 <br/> 資料列型別 <br/> 參考類型                             |
| **可為 Null**     | 否          | **True** (預設值) 或 **False** ，取決於屬性是否可以有 **null** 值。                                                                                                                          |
| **值** | 否          | 屬性的預設值。                                                                                                                                                                                              |
| **MaxLength**    | 否          | 屬性值的長度上限                                                                                                                                                                                       |
| **FixedLength**  | 否          | **True** 或 **False** ，取決於屬性值是否會儲存為固定長度字串。                                                                                                                          |
| **有效位數**    | 否          | 屬性值的有效位數。                                                                                                                                                                                            |
| **縮放比例**        | 否          | 屬性值的小數位數。                                                                                                                                                                                                |
| **SRID**         | 否          | 空間系統參考識別碼。 只對空間類型的屬性有效。 如需詳細資訊，請參閱 [SRID](https://en.wikipedia.org/wiki/SRID) 和 [SRID (SQL Server) ](https://msdn.microsoft.com/library/bb964707.aspx)。 |
| **Unicode**      | 否          | **True** 或 **False** ，取決於屬性值是否會儲存為 Unicode 字串。                                                                                                                               |
| **定序**    | 否          | 指定要在資料來源使用之定序的字串。                                                                                                                                                   |

 

> [!NOTE]
> 任何數目的注釋屬性 (自訂 XML 屬性) 都可以套用至 **Parameter** 元素。 不過，自訂屬性不可屬於任何 XML 命名空間，這是保留給 CSDL 之用。 任兩個自訂屬性的完整名稱不能相同。

 

#### <a name="example"></a>範例

下列範例 **顯示的函** 式專案會使用一個 **參數** 子項目來定義函數參數。

``` xml
 <Function Name="GetYearsEmployed" ReturnType="Edm.Int32">
 <Parameter Name="Instructor" Type="SchoolModel.Person" />
   <DefiningExpression>
   Year(CurrentDateTime()) - Year(cast(Instructor.HireDate as DateTime))
   </DefiningExpression>
 </Function>
```

 

## <a name="principal-element-csdl"></a>Principal 項目 (CSDL)

概念結構定義語言 (CSDL) 中的 **主體** 專案是 ReferentialConstraint 專案的子專案，可定義參考條件約束的主要端點。 **ReferentialConstraint**元素會定義類似于關係資料庫中的參考完整性條件約束的功能。 同樣地，資料庫資料表的資料行 (或多個資料行) 可以參考其他資料表的主索引鍵，實體類型的屬性 (或多個屬性) 可以參考其他實體類型的實體索引鍵。 參考的實體類型稱為條件約束的 *主要端點* 。 參考主體端的實體類型稱為條件約束的 *相依端點* 。 **PropertyRef** 元素可用來指定相依端所參考的索引鍵。

**主體**專案可以依) 列出的順序 (下列子項目：

-   PropertyRef (一或多個項目)
-   Annotation 項目 (零或多個項目)

### <a name="applicable-attributes"></a>適用的屬性

下表說明可套用至 **主體** 專案的屬性。

| 屬性名稱 | 必要 | 值                                                                |
|:---------------|:------------|:---------------------------------------------------------------------|
| **角色**       | 是         | 位於關聯之主要端點的實體類型名稱。 |

 

> [!NOTE]
> 任何數目的注釋屬性 (自訂 XML 屬性) 都可以套用至 **主體** 元素。 不過，自訂屬性不可屬於任何 XML 命名空間，這是保留給 CSDL 之用。 任兩個自訂屬性的完整名稱不能相同。

 

### <a name="example"></a>範例

下列範例顯示 **ReferentialConstraint** 元素，此專案屬於 **PublishedBy** 關聯的定義一部分。 **發行者**實體類型的**Id**屬性組成參考條件約束的主要端點。

``` xml
 <Association Name="PublishedBy">
   <End Type="BooksModel.Book" Role="Book" Multiplicity="*" >
   </End>
   <End Type="BooksModel.Publisher" Role="Publisher" Multiplicity="1" />
   <ReferentialConstraint>
     <Principal Role="Publisher">
       <PropertyRef Name="Id" />
     </Principal>
     <Dependent Role="Book">
       <PropertyRef Name="PublisherId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```
 

 

## <a name="property-element-csdl"></a>Property 項目 (CSDL)

概念結構定義語言 (CSDL) 中的 **Property** 元素可以是 EntityType 元素、ComplexType 專案或 RowType 專案的子系。

### <a name="entitytype-and-complextype-element-applications"></a>EntityType 和 ComplexType 項目的應用程式

**屬性** 元素 (為 **EntityType** 或 **ComplexType** 專案的子系) 定義實體類型實例或複雜類型實例將包含之資料的形狀和特性。 概念模型中的屬性類似類別中定義的屬性。 如同類別上的屬性可定義類別的圖形並包含關於物件的資訊，概念模型的屬性可定義實體類別的圖形，並包含關於實體類型執行個體的資訊。

**Property**專案可以有下列子項目， (依照列出的順序) ：

-   Documentation 項目 (允許零或一個項目)
-   Annotation 項目 (允許零或多個項目)

下列 facet 可以套用至 **Property** 元素： **Nullable**、 **DefaultValue**、 **MaxLength**、 **FixedLength**、 **Precision**、 **Scale**、 **Unicode**、定 **序**、 **ConcurrencyMode**。 Facet 是 XML 屬性 (attribute)，提供關於屬性 (property) 值如何儲存在資料存放區資訊。

> [!NOTE]
> Facet 只能套用至 **EDMSimpleType**類型的屬性。

 

#### <a name="applicable-attributes"></a>適用的屬性

下表描述可套用至 **Property** 專案的屬性。

| 屬性名稱                                                         | 必要 | 值                                                                                                                                                                                                                           |
|:-----------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **名稱**                                                               | 是         | 屬性的名稱。                                                                                                                                                                                                       |
| **型別**                                                               | 是         | 屬性值的類型。 屬性值類型必須是 **EDMSimpleType** ，或是模型範圍內的複雜類型 (以完整名稱表示)。                                                 |
| **可為 Null**                                                           | 否          | **True** (預設值) 或 <strong>False</strong>，取決於屬性是否可以具有 null 值。 <br/> [!NOTE]                                                                                                   |
| CSDL v1 中的 >，複雜型別屬性必須具有 `Nullable="False"` 。 |             |                                                                                                                                                                                                                                 |
| **值**                                                       | 否          | 屬性的預設值。                                                                                                                                                                                              |
| **MaxLength**                                                          | 否          | 屬性值的長度上限                                                                                                                                                                                       |
| **FixedLength**                                                        | 否          | **True** 或 **False** ，取決於屬性值是否會儲存為固定長度字串。                                                                                                                          |
| **有效位數**                                                          | 否          | 屬性值的有效位數。                                                                                                                                                                                            |
| **縮放比例**                                                              | 否          | 屬性值的小數位數。                                                                                                                                                                                                |
| **SRID**                                                               | 否          | 空間系統參考識別碼。 只對空間類型的屬性有效。 如需詳細資訊，請參閱 [SRID](https://en.wikipedia.org/wiki/SRID) 和 [SRID (SQL Server) ](https://msdn.microsoft.com/library/bb964707.aspx)。 |
| **Unicode**                                                            | 否          | **True** 或 **False** ，取決於屬性值是否會儲存為 Unicode 字串。                                                                                                                               |
| **定序**                                                          | 否          | 指定要在資料來源使用之定序的字串。                                                                                                                                                   |
| **ConcurrencyMode**                                                    | 否          | **None** (預設值) 或 **Fixed**。 如果值設為 **Fixed**，則屬性值將用於開放式並行存取檢查中。                                                                                  |

 

> [!NOTE]
> 任何數目的注釋屬性 (自訂 XML 屬性) 都可以套用至 **Property** 元素。 不過，自訂屬性不可屬於任何 XML 命名空間，這是保留給 CSDL 之用。 任兩個自訂屬性的完整名稱不能相同。

 

#### <a name="example"></a>範例

下列範例顯示具有三個**Property**元素的**EntityType**元素：

``` xml
 <EntityType Name="Book">
   <Key>
     <PropertyRef Name="ISBN" />
   </Key>
   <Property Type="String" Name="ISBN" Nullable="false" />
   <Property Type="String" Name="Title" Nullable="false" />
   <Property Type="Decimal" Name="Revision" Nullable="false" Precision="29" Scale="29" />
   <NavigationProperty Name="Publisher" Relationship="BooksModel.PublishedBy"
                       FromRole="Book" ToRole="Publisher" />
   <NavigationProperty Name="Authors" Relationship="BooksModel.WrittenBy"
                       FromRole="Book" ToRole="Author" />
 </EntityType>
```
 

下列範例顯示具有五個**屬性**元素的**ComplexType**元素：

``` xml
 <ComplexType Name="Address" >
   <Property Type="String" Name="StreetAddress" Nullable="false" />
   <Property Type="String" Name="City" Nullable="false" />
   <Property Type="String" Name="StateOrProvince" Nullable="false" />
   <Property Type="String" Name="Country" Nullable="false" />
   <Property Type="String" Name="PostalCode" Nullable="false" />
 </ComplexType>
```
 

### <a name="rowtype-element-application"></a>RowType 項目的應用程式

**屬性** 專案 (為 **RowType** 專案的子系) 定義可傳遞至模型定義函數或從模型定義函數傳回之資料的圖形和特性。  

**Property**元素只能有下列其中一個子項目：

-   CollectionType
-   ReferenceType
-   RowType

**屬性**元素可以有任意數目的子批註元素。

> [!NOTE]
> 只有 CSDL v2 和更新版本中才允許注釋元素。

 

#### <a name="applicable-attributes"></a>適用的屬性

下表描述可套用至 **Property** 專案的屬性。

| 屬性名稱                                                     | 必要 | 值                                                                                                                                                                                                                           |
|:-------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **名稱**                                                           | 是         | 屬性的名稱。                                                                                                                                                                                                       |
| **型別**                                                           | 是         | 屬性值的類型。                                                                                                                                                                                                 |
| **可為 Null**                                                       | 否          | **True** (預設值) 或 **False**，取決於屬性是否可以具有 null 值。 <br/> [!NOTE]                                                                                                                |
| CSDL v1 中的 >，複雜型別屬性必須具有 `Nullable="False"` 。 |             |                                                                                                                                                                                                                                 |
| **值**                                                   | 否          | 屬性的預設值。                                                                                                                                                                                              |
| **MaxLength**                                                      | 否          | 屬性值的長度上限                                                                                                                                                                                       |
| **FixedLength**                                                    | 否          | **True** 或 **False** ，取決於屬性值是否會儲存為固定長度字串。                                                                                                                          |
| **有效位數**                                                      | 否          | 屬性值的有效位數。                                                                                                                                                                                            |
| **縮放比例**                                                          | 否          | 屬性值的小數位數。                                                                                                                                                                                                |
| **SRID**                                                           | 否          | 空間系統參考識別碼。 只對空間類型的屬性有效。 如需詳細資訊，請參閱 [SRID](https://en.wikipedia.org/wiki/SRID) 和 [SRID (SQL Server) ](https://msdn.microsoft.com/library/bb964707.aspx)。 |
| **Unicode**                                                        | 否          | **True** 或 **False** ，取決於屬性值是否會儲存為 Unicode 字串。                                                                                                                               |
| **定序**                                                      | 否          | 指定要在資料來源使用之定序的字串。                                                                                                                                                   |

 

> [!NOTE]
> 任何數目的注釋屬性 (自訂 XML 屬性) 都可以套用至 **Property** 元素。 不過，自訂屬性不可屬於任何 XML 命名空間，這是保留給 CSDL 之用。 任兩個自訂屬性的完整名稱不能相同。

 

#### <a name="example"></a>範例

下列範例顯示用來定義模型定義函式之傳回型別圖形的 **屬性** 專案。

``` xml
 <Function Name="LastNamesAfter">
   <Parameter Name="someString" Type="Edm.String" />
   <ReturnType>
    <CollectionType>
      <RowType>
        <Property Name="FirstName" Type="Edm.String" Nullable="false" />
        <Property Name="LastName" Type="Edm.String" Nullable="false" />
      </RowType>
    </CollectionType>
   </ReturnType>
   <DefiningExpression>
             SELECT VALUE ROW(p.FirstName, p.LastName)
             FROM SchoolEntities.People AS p
             WHERE p.LastName &gt;= somestring
   </DefiningExpression>
 </Function>
```
 

 

## <a name="propertyref-element-csdl"></a>PropertyRef 項目 (CSDL)

概念結構定義語言 (CSDL 中的 **PropertyRef** 專案) 參考實體類型的屬性，以表示屬性將執行下列其中一個角色：

-   實體索引鍵的一部分 (可判斷識別之實體類型的屬性或屬性集)。 您可以使用一或多個 **PropertyRef** 元素來定義實體索引鍵。
-   參考條件約束的相依端點和主要端點。

**PropertyRef**元素只能將 annotation 元素 (零或多個) 做為子項目。

> [!NOTE]
> 只有 CSDL v2 和更新版本中才允許注釋元素。

 

### <a name="applicable-attributes"></a>適用的屬性

下表說明可套用至 **PropertyRef** 元素的屬性。

| 屬性名稱 | 必要 | 值                                |
|:---------------|:------------|:-------------------------------------|
| **名稱**       | 是         | 參考屬性的名稱。 |

 

> [!NOTE]
> 任何數目的注釋屬性 (自訂 XML 屬性) 都可以套用至 **PropertyRef** 元素。 不過，自訂屬性不可屬於任何 XML 命名空間，這是保留給 CSDL 之用。 任兩個自訂屬性的完整名稱不能相同。

 

### <a name="example"></a>範例

下列範例會定義 (**Book**) 的實體類型。 實體索引鍵是透過參考實體類型的 **ISBN** 屬性來定義。

``` xml
 <EntityType Name="Book">
   <Key>
     <PropertyRef Name="ISBN" />
   </Key>
   <Property Type="String" Name="ISBN" Nullable="false" />
   <Property Type="String" Name="Title" Nullable="false" />
   <Property Type="Decimal" Name="Revision" Nullable="false" Precision="29" Scale="29" />
   <NavigationProperty Name="Publisher" Relationship="BooksModel.PublishedBy"
                       FromRole="Book" ToRole="Publisher" />
   <NavigationProperty Name="Authors" Relationship="BooksModel.WrittenBy"
                       FromRole="Book" ToRole="Author" />
 </EntityType>
```
 

在下一個範例中，會使用兩個 **PropertyRef** 元素來指出 (**識別碼** 和 **PublisherId**) 的兩個屬性是參考條件約束的主體和相依端點。

``` xml
 <Association Name="PublishedBy">
   <End Type="BooksModel.Book" Role="Book" Multiplicity="*" >
   </End>
   <End Type="BooksModel.Publisher" Role="Publisher" Multiplicity="1" />
   <ReferentialConstraint>
     <Principal Role="Publisher">
       <PropertyRef Name="Id" />
     </Principal>
     <Dependent Role="Book">
       <PropertyRef Name="PublisherId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```
 

 

## <a name="referencetype-element-csdl"></a>ReferenceType 項目 (CSDL)

概念結構定義語言 (CSDL) 中的 **ReferenceType** 元素會指定實體類型的參考。 **ReferenceType**元素可以是下列元素的子系：

-   ReturnType (函式) 
-   參數
-   CollectionType

定義函數的參數或傳回類型時，會使用 **ReferenceType** 元素。

**ReferenceType**元素可以依照列出的順序 (下列子項目) ：

-   Documentation (零或一個項目)
-   Annotation 項目 (零或多個項目)

### <a name="applicable-attributes"></a>適用的屬性

下表說明可套用至 **ReferenceType** 元素的屬性。

| 屬性名稱 | 必要 | 值                                         |
|:---------------|:------------|:----------------------------------------------|
| **型別**       | 是         | 參考之實體類型的名稱。 |

 

> [!NOTE]
> 任何數目的注釋屬性 (自訂 XML 屬性) 都可以套用至 **ReferenceType** 元素。 不過，自訂屬性不可屬於任何 XML 命名空間，這是保留給 CSDL 之用。 任兩個自訂屬性的完整名稱不能相同。

 

### <a name="example"></a>範例

下列範例顯示在模型定義函式中，用來做為**Parameter**元素子系的**ReferenceType**專案，該函式會接受**Person**實體類型的參考：

``` xml
 <Function Name="GetYearsEmployed" ReturnType="Edm.Int32">
   <Parameter Name="instructor">
     <ReferenceType Type="SchoolModel.Person" />
   </Parameter>
   <DefiningExpression>
   Year(CurrentDateTime()) - Year(cast(instructor.HireDate as DateTime))
   </DefiningExpression>
 </Function>
```
 

下列範例顯示在模型定義函式中，用來做為**ReturnType**)  (函式之子系的**ReferenceType**專案，該函數會傳回**Person**實體類型的參考：

``` xml
 <Function Name="GetPersonReference">
     <Parameter Name="p" Type="SchoolModel.Person" />
     <ReturnType>
         <ReferenceType Type="SchoolModel.Person" />
     </ReturnType>
     <DefiningExpression>
           REF(p)
     </DefiningExpression>
 </Function>
```
 

 

## <a name="referentialconstraint-element-csdl"></a>ReferentialConstraint 項目 (CSDL)

概念結構定義語言 (CSDL) 中的 **ReferentialConstraint** 專案會定義類似于關係資料庫中的參考完整性條件約束的功能。 同樣地，資料庫資料表的資料行 (或多個資料行) 可以參考其他資料表的主索引鍵，實體類型的屬性 (或多個屬性) 可以參考其他實體類型的實體索引鍵。 參考的實體類型稱為條件約束的 *主要端點* 。 參考主體端的實體類型稱為條件約束的 *相依端點* 。

如果在某個實體類型上公開的外鍵參考另一個實體類型上的屬性，則 **ReferentialConstraint** 元素會定義這兩個實體類型之間的關聯。 由於 **ReferentialConstraint** 元素會提供兩個實體型別的相關資訊，對應規格語言 (MSL) 中不需要對應的 AssociationSetMapping 元素。 未公開外鍵的兩個實體類型之間的關聯，必須有對應的 **AssociationSetMapping** 專案，才能將關聯資訊對應至資料來源。

如果未在實體類型上公開外鍵，則 **ReferentialConstraint** 元素只能定義實體類型與另一個實體類型之間的 primary key to primary key 條件約束。

**ReferentialConstraint**元素可以依照列出的順序 (下列子項目) ：

-   Documentation (零或一個項目)
-   Principal (只有一個項目)
-   Dependent (只有一個項目)
-   Annotation 項目 (零或多個項目)

### <a name="applicable-attributes"></a>適用的屬性

**ReferentialConstraint**元素可以有任意數目的注釋屬性 (自訂 XML 屬性) 。 不過，自訂屬性不可屬於任何 XML 命名空間，這是保留給 CSDL 之用。 任兩個自訂屬性的完整名稱不能相同。

### <a name="example"></a>範例

下列範例顯示在**PublishedBy**關聯定義中使用的**ReferentialConstraint**元素。

``` xml
 <Association Name="PublishedBy">
   <End Type="BooksModel.Book" Role="Book" Multiplicity="*" >
   </End>
   <End Type="BooksModel.Publisher" Role="Publisher" Multiplicity="1" />
   <ReferentialConstraint>
     <Principal Role="Publisher">
       <PropertyRef Name="Id" />
     </Principal>
     <Dependent Role="Book">
       <PropertyRef Name="PublisherId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```
 

 

## <a name="returntype-function-element-csdl"></a>ReturnType (函式) 元素 (CSDL) 

在概念結構定義語言 (CSDL 中， **ReturnType** (函式) 元素) 指定函式專案中定義之函式的傳回型別。 函數傳回型別也可以使用 **ReturnType** 屬性來指定。

傳回型別可以是任何 **EdmSimpleType**、實體類型、複雜型別、資料列類型、ref 型別或其中一個類型的集合。

您可以使用**ReturnType** (函式) 專案的**型**別屬性，或使用下列其中一個子項目來指定函式的傳回型別：

-   CollectionType
-   ReferenceType
-   RowType

> [!NOTE]
> 如果您指定的函式傳回型別同時具有**ReturnType** (函式的**類型**屬性) 專案和其中一個子項目，則模型將不會驗證。

 

### <a name="applicable-attributes"></a>適用的屬性

下表描述可套用至 **ReturnType** (函式) 專案的屬性。

| 屬性名稱 | 必要 | 值                              |
|:---------------|:------------|:-----------------------------------|
| **ReturnType** | 否          | 此函式傳回的型別。 |

 

> [!NOTE]
> 任何數目的注釋屬性 (自訂 XML 屬性) 可以套用至 **ReturnType** (函數) 專案。 不過，自訂屬性不可屬於任何 XML 命名空間，這是保留給 CSDL 之用。 任兩個自訂屬性的完整名稱不能相同。

 

### <a name="example"></a>範例

下列範例會使用 **function** 專案來定義函式，該函式會傳回書籍列印的年數。 請注意，傳回型別是由**ReturnType** (函式) 元素的**type**屬性所指定。

``` xml
 <Function Name="GetYearsInPrint">
   <ReturnType Type=="Edm.Int32">
   <Parameter Name="book" Type="BooksModel.Book" />
   <DefiningExpression>
    Year(CurrentDateTime()) - Year(cast(book.PublishedDate as DateTime))
   </DefiningExpression>
 </Function>
```
 

 

## <a name="returntype-functionimport-element-csdl"></a>ReturnType (FunctionImport) 元素 (CSDL) 

在概念結構定義語言 (CSDL 中， **ReturnType** (FunctionImport) 元素) 指定 FunctionImport 元素中定義之函式的傳回型別。 函數傳回型別也可以使用 **ReturnType** 屬性來指定。

傳回類型可以是任何實體類型、複雜類型或 **EdmSimpleType**的集合。

函式的傳回型別是以**ReturnType** (FunctionImport) 專案的**型**別屬性（attribute）所指定。

### <a name="applicable-attributes"></a>適用的屬性

下表描述可套用至 **ReturnType** (FunctionImport) 專案的屬性。

| 屬性名稱 | 必要 | 值                                                                                                                                                                                                 |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **類型**       | 否          | 函式傳回的型別。 值必須是 ComplexType、EntityType 或 EDMSimpleType 的集合。                                                                                      |
| **EntitySet**  | 否          | 如果函式傳回實體類型的集合，則 **EntitySet** 的值必須是集合所屬的實體集。 否則，不得使用 **EntitySet** 屬性。 |

 

> [!NOTE]
> 任何數目的注釋屬性 (自訂 XML 屬性) 可能會套用至 **ReturnType** (FunctionImport) 元素。 不過，自訂屬性不可屬於任何 XML 命名空間，這是保留給 CSDL 之用。 任兩個自訂屬性的完整名稱不能相同。

 

### <a name="example"></a>範例

下列範例會使用會傳回書籍和發行者的 **FunctionImport** 。 請注意，此函數會傳回兩個結果集，因此會指定兩個 **ReturnType** (FunctionImport) 元素。

``` xml
 <FunctionImport Name="GetBooksAndPublishers">
   <ReturnType Type=="Collection(BooksModel.Book )" EntitySet=”Books”>
   <ReturnType Type=="Collection(BooksModel.Publisher)" EntitySet=”Publishers”>
 </FunctionImport>
```
 

 

## <a name="rowtype-element-csdl"></a>RowType 項目 (CSDL)

概念結構定義語言 (CSDL 中的 **RowType** 專案) 會將未命名的結構定義為概念模型中定義之函式的參數或傳回型別。

**RowType**元素可以是下列元素的子系：

-   CollectionType
-   參數
-   ReturnType (函式) 

**RowType**元素可以依照列出的順序 (下列子項目) ：

-   屬性 (一個或多個)
-   註釋項目 (零或多個)

### <a name="applicable-attributes"></a>適用的屬性

任何數目的注釋屬性 (自訂 XML 屬性) 都可以套用至 **RowType** 元素。 不過，自訂屬性不可屬於任何 XML 命名空間，這是保留給 CSDL 之用。 任兩個自訂屬性的完整名稱不能相同。

### <a name="example"></a>範例

下列範例示範模型定義函式，該函式會使用 **CollectionType** 專案來指定函式傳回資料列集合 (如 **RowType** 元素) 中所指定。

``` xml
 <Function Name="LastNamesAfter">
   <Parameter Name="someString" Type="Edm.String" />
   <ReturnType>
    <CollectionType>
      <RowType>
        <Property Name="FirstName" Type="Edm.String" Nullable="false" />
        <Property Name="LastName" Type="Edm.String" Nullable="false" />
      </RowType>
    </CollectionType>
   </ReturnType>
   <DefiningExpression>
             SELECT VALUE ROW(p.FirstName, p.LastName)
             FROM SchoolEntities.People AS p
             WHERE p.LastName &gt;= somestring
   </DefiningExpression>
 </Function>
```

## <a name="schema-element-csdl"></a>Schema 項目 (CSDL)

**Schema**元素是概念模型定義的根項目。 其中包含組成概念模型的物件、函式和容器等定義。

**Schema**元素可能包含零或多個下列子項目：

-   使用
-   EntityContainer
-   EntityType
-   EnumType
-   關聯
-   ComplexType
-   函式

**架構**專案可能包含零個或一個 Annotation 元素。

> [!NOTE]
> 只有 CSDL v2 和更新版本中才允許使用 **Function** 元素和 annotation 元素。

 

**Schema**專案使用**namespace**屬性來定義概念模型中的實體類型、複雜型別和關聯物件的命名空間。 在命名空間中，兩個物件不能有相同的名稱。 命名空間可以橫跨多個 **架構** 元素和多個 csdl 檔案。

概念模型命名空間與 **架構** 元素的 XML 命名空間不同。 概念模型命名空間 (如 **命名空間** 屬性所定義) 是實體類型、複雜類型和關聯類型的邏輯容器。 **Schema**專案的**xmlns**屬性) 所指出的 XML 命名空間 (是**schema**專案的子項目和屬性的預設命名空間。 格式 (的 XML 命名空間， https://schemas.microsoft.com/ado/YYYY/MM/edm 其中 YYYY 和 MM 分別代表年份和月份，) 會保留給 CSDL。 自訂項目和屬性不能出現在擁有此格式的命名空間中。

### <a name="applicable-attributes"></a>適用的屬性

下表說明可套用至 **Schema** 專案的屬性。

| 屬性名稱 | 必要 | 值                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|:---------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Namespace**  | 是         | 概念模型的命名空間。 **Namespace**屬性的值是用來形成型別的完整名稱。 例如，如果名為*Customer*的**entitytype**位於簡單的. 範例. 模型命名空間中，則**Entitytype**的完整名稱是 simpleexamplemodel.customer。 <br/> 下列字串不能當做 Namespace 屬性（ **Namespace** ）的值使用： **System**、 **暫時性**或 **Edm**。 **Namespace**屬性的值不能與 SSDL Schema 元素中**namespace**屬性的值相同。 |
| **別名**      | 否          | 用來取代命名空間名稱的識別項。 例如，如果名為*Customer*的**EntityType**位於簡單的. 模型命名空間中，而**Alias**屬性的值為*Model*，則您可以使用 model. Customer 作為 EntityType 的完整名稱 **。**                                                                                                                                                                                                                                                                                                     |

 

> [!NOTE]
> 任何數目的注釋屬性 (自訂 XML 屬性) 都可以套用至 **Schema** 元素。 不過，自訂屬性不可屬於任何 XML 命名空間，這是保留給 CSDL 之用。 任兩個自訂屬性的完整名稱不能相同。

 

### <a name="example"></a>範例

下列範例顯示包含**EntityContainer**元素、兩個**EntityType**專案和一個**Association**元素的**Schema**專案。

``` xml
 <Schema xmlns="https://schemas.microsoft.com/ado/2009/11/edm"
      xmlns:cg="https://schemas.microsoft.com/ado/2009/11/codegeneration"
      xmlns:store="https://schemas.microsoft.com/ado/2009/11/edm/EntityStoreSchemaGenerator"
       Namespace="ExampleModel" Alias="Self">
         <EntityContainer Name="ExampleModelContainer">
           <EntitySet Name="Customers"
                      EntityType="ExampleModel.Customer" />
           <EntitySet Name="Orders" EntityType="ExampleModel.Order" />
           <AssociationSet
                       Name="CustomerOrder"
                       Association="ExampleModel.CustomerOrders">
             <End Role="Customer" EntitySet="Customers" />
             <End Role="Order" EntitySet="Orders" />
           </AssociationSet>
         </EntityContainer>
         <EntityType Name="Customer">
           <Key>
             <PropertyRef Name="CustomerId" />
           </Key>
           <Property Type="Int32" Name="CustomerId" Nullable="false" />
           <Property Type="String" Name="Name" Nullable="false" />
           <NavigationProperty
                    Name="Orders"
                    Relationship="ExampleModel.CustomerOrders"
                    FromRole="Customer" ToRole="Order" />
         </EntityType>
         <EntityType Name="Order">
           <Key>
             <PropertyRef Name="OrderId" />
           </Key>
           <Property Type="Int32" Name="OrderId" Nullable="false" />
           <Property Type="Int32" Name="ProductId" Nullable="false" />
           <Property Type="Int32" Name="Quantity" Nullable="false" />
           <NavigationProperty
                    Name="Customer"
                    Relationship="ExampleModel.CustomerOrders"
                    FromRole="Order" ToRole="Customer" />
           <Property Type="Int32" Name="CustomerId" Nullable="false" />
         </EntityType>
         <Association Name="CustomerOrders">
           <End Type="ExampleModel.Customer"
                Role="Customer" Multiplicity="1" />
           <End Type="ExampleModel.Order"
                Role="Order" Multiplicity="*" />
           <ReferentialConstraint>
             <Principal Role="Customer">
               <PropertyRef Name="CustomerId" />
             </Principal>
             <Dependent Role="Order">
               <PropertyRef Name="CustomerId" />
             </Dependent>
           </ReferentialConstraint>
         </Association>
       </Schema>
```
 

 

## <a name="typeref-element-csdl"></a>TypeRef 項目 (CSDL)

概念結構定義語言 (CSDL 中的 **TypeRef** 元素) 提供現有命名類型的參考。 **TypeRef**元素可以是 CollectionType 專案的子系，用來指定函式的集合為參數或傳回型別。

**TypeRef**專案可以有下列子項目， (依所列的順序) ：

-   Documentation (零或一個項目)
-   Annotation 項目 (零或多個項目)

### <a name="applicable-attributes"></a>適用的屬性

下表說明可套用至 **TypeRef** 元素的屬性。 請注意， **DefaultValue**、 **MaxLength**、 **FixedLength**、 **Precision**、 **Scale**、 **Unicode**和定 **序** 屬性僅適用于 **EDMSimpleTypes**。

| 屬性名稱                                                     | 必要 | 值                                                                                                                                                                                                                           |
|:-------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **類型**                                                           | 否          | 所參考的型別名稱。                                                                                                                                                                                          |
| **可為 Null**                                                       | 否          | **True** (預設值) 或 **False**，取決於屬性是否可以具有 null 值。 <br/> [!NOTE]                                                                                                                |
| CSDL v1 中的 >，複雜型別屬性必須具有 `Nullable="False"` 。 |             |                                                                                                                                                                                                                                 |
| **值**                                                   | 否          | 屬性的預設值。                                                                                                                                                                                              |
| **MaxLength**                                                      | 否          | 屬性值的長度上限                                                                                                                                                                                       |
| **FixedLength**                                                    | 否          | **True** 或 **False** ，取決於屬性值是否會儲存為固定長度字串。                                                                                                                          |
| **有效位數**                                                      | 否          | 屬性值的有效位數。                                                                                                                                                                                            |
| **縮放比例**                                                          | 否          | 屬性值的小數位數。                                                                                                                                                                                                |
| **SRID**                                                           | 否          | 空間系統參考識別碼。 只對空間類型的屬性有效。 如需詳細資訊，請參閱 [SRID](https://en.wikipedia.org/wiki/SRID) 和 [SRID (SQL Server) ](https://msdn.microsoft.com/library/bb964707.aspx)。 |
| **Unicode**                                                        | 否          | **True** 或 **False** ，取決於屬性值是否會儲存為 Unicode 字串。                                                                                                                               |
| **定序**                                                      | 否          | 指定要在資料來源使用之定序的字串。                                                                                                                                                   |

 

> [!NOTE]
> 任何數目的注釋屬性 (自訂 XML 屬性) 都可以套用至 **CollectionType** 元素。 不過，自訂屬性不可屬於任何 XML 命名空間，這是保留給 CSDL 之用。 任兩個自訂屬性的完整名稱不能相同。

 

### <a name="example"></a>範例

下列範例示範模型定義的函式，此函式會使用 **TypeRef** 元素 (作為 **CollectionType** 專案的子系) 指定函式接受 **部門** 實體類型的集合。

``` xml
 <Function Name="GetAvgBudget">
      <Parameter Name="Departments">
          <CollectionType>
             <TypeRef Type="SchoolModel.Department"/>
          </CollectionType>
           </Parameter>
       <ReturnType Type="Collection(Edm.Decimal)"/>
       <DefiningExpression>
             SELECT VALUE AVG(d.Budget) FROM Departments AS d
       </DefiningExpression>
 </Function>
```
 

 

## <a name="using-element-csdl"></a>Using 項目 (CSDL)

概念結構定義語言 (CSDL 中的 **Using** 元素) 會匯入存在於不同命名空間中之概念模型的內容。 藉由設定 **Namespace** 屬性的值，您可以參考實體類型、複雜類型，以及在另一個概念模型中定義的關聯類型。 一個以上的 **Using** 元素可以是 **Schema** 元素的子系。

> [!NOTE]
> CSDL 中的 **using** 元素不像程式語言中的 **using** 語句一樣運作。 藉由在程式設計語言中使用 **using** 語句匯入命名空間，您就不會影響原始命名空間中的物件。 在 CSDL 中，匯入的命名空間可以包含實體類型，該實體類型是衍生自原始命名空間中的實體類型。 這會影響在原始命名空間中宣告的實體集。

 

**Using**元素可以有下列子項目：

-   Documentation (可允許零或一個項目)
-   Annotation 項目 (允許零或多個項目)

### <a name="applicable-attributes"></a>適用的屬性

下表說明屬性可以套用至 **Using** 元素。

| 屬性名稱 | 必要 | 值                                                                                                                                                                              |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Namespace**  | 是         | 匯入的命名空間名稱。                                                                                                                                                |
| **別名**      | 是         | 用來取代命名空間名稱的識別項。 雖然這個屬性是必要的，但是不必使用此屬性取代命名空間名稱，來限定物件名稱。 |

 

> [!NOTE]
> 您可以將任何數目的注釋屬性 (自訂 XML 屬性，) 可以套用至 **Using** 元素。 不過，自訂屬性不可屬於任何 XML 命名空間，這是保留給 CSDL 之用。 任兩個自訂屬性的完整名稱不能相同。

 

### <a name="example"></a>範例

下列範例示範用來匯入在其他地方定義之命名空間的 **Using** 元素。 請注意，顯示的 **架構** 元素命名空間為 `BooksModel` 。 `Address`EntityType 上的屬性 `Publisher` **EntityType**是在命名空間中定義的複雜型別， `ExtendedBooksModel` (**使用**) 所匯入的 Using 元素。

``` xml
 <Schema xmlns="https://schemas.microsoft.com/ado/2009/11/edm"
           xmlns:cg="https://schemas.microsoft.com/ado/2009/11/codegeneration"
           xmlns:store="https://schemas.microsoft.com/ado/2009/11/edm/EntityStoreSchemaGenerator"
           Namespace="BooksModel" Alias="Self">

     <Using Namespace="BooksModel.Extended" Alias="BMExt" />

 <EntityContainer Name="BooksContainer" >
       <EntitySet Name="Publishers" EntityType="BooksModel.Publisher" />
     </EntityContainer>

 <EntityType Name="Publisher">
       <Key>
         <PropertyRef Name="Id" />
       </Key>
       <Property Type="Int32" Name="Id" Nullable="false" />
       <Property Type="String" Name="Name" Nullable="false" />
       <Property Type="BMExt.Address" Name="Address" Nullable="false" />
     </EntityType>

 </Schema>
```
 

 

## <a name="annotation-attributes-csdl"></a>註釋屬性 (CSDL)

概念結構描述定義語言 (CSDL) 中的附註屬性是概念模型中自訂的 XML 屬性。 除了擁有有效的 XML 結構外，下列附註屬性的條件必須成立：

-   附註屬性不能在任何 XML 命名空間之中，這是保留供 CSDL 之用。
-   超過一個以上的附註屬性可以套用至給定的 CSDL 項目。
-   任兩個 Annotation 屬性的完整名稱不能相同。

附註屬性可用來提供與概念模型中之項目有關的額外中繼資料。 您可以使用 system.string 命名空間中的類別，在執行時間存取批註專案中包含的中繼資料。

### <a name="example"></a>範例

下列範例顯示具有 annotation 屬性 (**CustomAttribute**) 的**EntityType**元素。 下列範例也顯示套用至實體類型項目的 Annotation 項目。

``` xml
 <Schema Namespace="SchoolModel" Alias="Self"
         xmlns:annotation="https://schemas.microsoft.com/ado/2009/02/edm/annotation"
         xmlns="https://schemas.microsoft.com/ado/2009/11/edm">
   <EntityContainer Name="SchoolEntities" annotation:LazyLoadingEnabled="true">
     <EntitySet Name="People" EntityType="SchoolModel.Person" />
   </EntityContainer>
   <EntityType Name="Person" xmlns:p="http://CustomNamespace.com"
               p:CustomAttribute="Data here.">
     <Key>
       <PropertyRef Name="PersonID" />
     </Key>
     <Property Name="PersonID" Type="Int32" Nullable="false"
               annotation:StoreGeneratedPattern="Identity" />
     <Property Name="LastName" Type="String" Nullable="false"
               MaxLength="50" Unicode="true" FixedLength="false" />
     <Property Name="FirstName" Type="String" Nullable="false"
               MaxLength="50" Unicode="true" FixedLength="false" />
     <Property Name="HireDate" Type="DateTime" />
     <Property Name="EnrollmentDate" Type="DateTime" />
     <p:CustomElement>
       Custom metadata.
     </p:CustomElement>
   </EntityType>
 </Schema>
```
 

下列程式碼會擷取附註屬性中的中繼資料，並且將它撰寫至主控台：

``` xml
 EdmItemCollection collection = new EdmItemCollection("School.csdl");
 MetadataWorkspace workspace = new MetadataWorkspace();
 workspace.RegisterItemCollection(collection);
 EdmType contentType;
 workspace.TryGetType("Person", "SchoolModel", DataSpace.CSpace, out contentType);
 if (contentType.MetadataProperties.Contains("http://CustomNamespace.com:CustomAttribute"))
 {
     MetadataProperty annotationProperty =
         contentType.MetadataProperties["http://CustomNamespace.com:CustomAttribute"];
     object annotationValue = annotationProperty.Value;
     Console.WriteLine(annotationValue.ToString());
 }
```
 

以上程式碼假設 `School.csdl` 檔位於專案的輸出目錄中，而且您已將下列 `Imports` 和 `Using` 陳述式加入至專案：

``` csharp
 using System.Data.Metadata.Edm;
```
 

 

## <a name="annotation-elements-csdl"></a>註釋項目 (CSDL)

概念結構定義語言 (CSDL) 中的 Annotation 項目是概念模型中的自訂 XML 項目。 除了擁有有效的 XML 結構外，下列 Annotation 項目的條件也必須成立：

-   Annotation 項目不能存在於保留供 CSDL 使用的任何 XML 命名空間中。
-   多個 Annotation 項目可以同時為指定 CSDL 項目的子系。
-   任兩個 Annotation 項目的完整名稱不能相同。
-   Annotation 項目必須出現在指定 CSDL 項目的所有其他子項目之後。

Annotation 項目可用來提供與概念模型中之項目有關的額外中繼資料。 從 .NET Framework 第4版開始，可以使用 system.string 命名空間中的類別，在執行時間存取批註專案中包含的中繼資料。

### <a name="example"></a>範例

下列範例示範具有 annotation 元素 (**CustomElement**) 的**EntityType**元素。 該範例也顯示套用至實體類型項目的 annotation 屬性。

``` xml
 <Schema Namespace="SchoolModel" Alias="Self"
         xmlns:annotation="https://schemas.microsoft.com/ado/2009/02/edm/annotation"
         xmlns="https://schemas.microsoft.com/ado/2009/11/edm">
   <EntityContainer Name="SchoolEntities" annotation:LazyLoadingEnabled="true">
     <EntitySet Name="People" EntityType="SchoolModel.Person" />
   </EntityContainer>
   <EntityType Name="Person" xmlns:p="http://CustomNamespace.com"
               p:CustomAttribute="Data here.">
     <Key>
       <PropertyRef Name="PersonID" />
     </Key>
     <Property Name="PersonID" Type="Int32" Nullable="false"
               annotation:StoreGeneratedPattern="Identity" />
     <Property Name="LastName" Type="String" Nullable="false"
               MaxLength="50" Unicode="true" FixedLength="false" />
     <Property Name="FirstName" Type="String" Nullable="false"
               MaxLength="50" Unicode="true" FixedLength="false" />
     <Property Name="HireDate" Type="DateTime" />
     <Property Name="EnrollmentDate" Type="DateTime" />
     <p:CustomElement>
       Custom metadata.
     </p:CustomElement>
   </EntityType>
 </Schema>
```
 

下列程式碼會擷取 annotation 項目中的中繼資料，並且將它撰寫至主控台：

``` csharp
 EdmItemCollection collection = new EdmItemCollection("School.csdl");
 MetadataWorkspace workspace = new MetadataWorkspace();
 workspace.RegisterItemCollection(collection);
 EdmType contentType;
 workspace.TryGetType("Person", "SchoolModel", DataSpace.CSpace, out contentType);
 if (contentType.MetadataProperties.Contains("http://CustomNamespace.com:CustomElement"))
 {
     MetadataProperty annotationProperty =
         contentType.MetadataProperties["http://CustomNamespace.com:CustomElement"];
     object annotationValue = annotationProperty.Value;
     Console.WriteLine(annotationValue.ToString());
 }
```
 

以上程式碼假設 School.csdl 檔位於專案的輸出目錄中，而且您已將下列 `Imports` 和 `Using` 陳述式加入至專案：

``` csharp
 using System.Data.Metadata.Edm;
```
 

 

## <a name="conceptual-model-types-csdl"></a>概念模型類型 (CSDL)

概念結構定義語言 (CSDL) 支援一組稱為 **EDMSimpleTypes**的抽象基本資料類型，可定義概念模型中的屬性。 **EDMSimpleTypes** 是儲存或裝載環境中支援之基本資料類型的 proxy。

下表列出 CSDL 所支援的基本資料型別。 此表格也會列出可以套用至每個 **EDMSimpleType**的 facet。

| EDMSimpleType                    | 描述                                                | 適用的 Facet                                                        |
|:---------------------------------|:-----------------------------------------------------------|:-------------------------------------------------------------------------|
| **Edm.Binary**                   | 包含二進位資料。                                      | MaxLength、FixedLength、Nullable、Default                                |
| **Edm.Boolean**                  | 包含 **true** 或 **false**值。                  | Nullable、Default                                                        |
| **Edm.Byte**                     | 包含不帶正負號的 8 位元整數值。                  | Precision、Nullable、Default                                             |
| **Edm.DateTime**                 | 表示日期和時間。                                | Precision、Nullable、Default                                             |
| **Edm.DateTimeOffset**           | 包含與 GMT 的日期和時間時差 (以分鐘為單位)。 | Precision、Nullable、Default                                             |
| **Edm.Decimal**                  | 包含固定有效位數和小數位數的數值。   | Precision、Nullable、Default                                             |
| **Edm.Double**                   | 包含具有15位數精確度的浮點數   | Precision、Nullable、Default                                             |
| **Edm. Float**                    | 包含具有 7 位數精確度的浮點數。   | Precision、Nullable、Default                                             |
| **Edm.Guid**                     | 包含 16 位元組的唯一識別碼。                      | Precision、Nullable、Default                                             |
| **Edm.Int16**                    | 包含帶正負號的 16 位元整數值。                    | Precision、Nullable、Default                                             |
| **Edm.Int32**                    | 包含帶正負號的 32 位元整數值。                    | Precision、Nullable、Default                                             |
| **Edm.Int64**                    | 包含帶正負號的 64 位元整數值。                    | Precision、Nullable、Default                                             |
| **Edm.SByte**                    | 包含帶正負號的 8 位元整數值。                     | Precision、Nullable、Default                                             |
| **Edm.String**                   | 包含字元資料。                                   | Unicode、FixedLength、MaxLength、Collation、Precision、Nullable、Default |
| **Edm.Time**                     | 包含一天的時間。                                    | Precision、Nullable、Default                                             |
| **Edm. Geography**                |                                                            | Nullable、Default、SRID                                                  |
| **Edm.GeographyPoint**           |                                                            | Nullable、Default、SRID                                                  |
| **>geographylinestring**      |                                                            | Nullable、Default、SRID                                                  |
| **Edm.geographypolygon**         |                                                            | Nullable、Default、SRID                                                  |
| **>geographymultipoint**      |                                                            | Nullable、Default、SRID                                                  |
| **>geographymultilinestring** |                                                            | Nullable、Default、SRID                                                  |
| **>geographymultipolygon**    |                                                            | Nullable、Default、SRID                                                  |
| **GeographyCollection**      |                                                            | Nullable、Default、SRID                                                  |
| **Edm. Geometry**                 |                                                            | Nullable、Default、SRID                                                  |
| **>geometrypoint**            |                                                            | Nullable、Default、SRID                                                  |
| **>geometrylinestring**       |                                                            | Nullable、Default、SRID                                                  |
| **>geometrypolygon**          |                                                            | Nullable、Default、SRID                                                  |
| **>geometrymultipoint**       |                                                            | Nullable、Default、SRID                                                  |
| **>geometrymultilinestring**  |                                                            | Nullable、Default、SRID                                                  |
| **>geometrymultipolygon**     |                                                            | Nullable、Default、SRID                                                  |
| **>geometrycollection**       |                                                            | Nullable、Default、SRID                                                  |

## <a name="facets-csdl"></a>Facet (CSDL)

概念結構定義語言 (CSDL) 中的 Facet 表示實體型別和複雜型別屬性上的條件約束。 Facet 在下列 CSDL 元素上會以 XML 屬性的形式出現：

-   屬性
-   TypeRef
-   參數

下表說明 CSDL 中支援的 Facet。 所有的 Facet 都是選擇性的。 從概念模型產生資料庫時，Entity Framework 會使用下面所列的部分 facet。

> [!NOTE]
> 如需概念模型中資料類型的詳細資訊，請參閱 (CSDL) 的概念模型類型。

| Facet               | 描述                                                                                                                                                                                                                                                   | 適用於                                                                                                                                                                                                                                                                                                                                                                           | 用於產生資料庫 | 由執行階段所使用 |
|:--------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------|:--------------------|
| **定序**       | 在執行比較和排序屬性值的作業時，指定要使用的定序順序 (或排序順序)。                                                                                                               | **Edm.String**                                                                                                                                                                                                                                                                                                                                                                       | 是                              | 否                  |
| **ConcurrencyMode** | 表示屬性值應用於開放式並行存取檢查。                                                                                                                                                                    | 所有 **EDMSimpleType** 屬性                                                                                                                                                                                                                                                                                                                                                     | 否                               | 是                 |
| **預設值**         | 執行個體化時如果沒有提供值，請指定屬性的預設值。                                                                                                                                                                       | 所有 **EDMSimpleType** 屬性                                                                                                                                                                                                                                                                                                                                                     | 是                              | 是                 |
| **FixedLength**     | 指定屬性值的長度是否可以變更。                                                                                                                                                                                                  | **Edm. 二進位**、 **edm. 字串**                                                                                                                                                                                                                                                                                                                                                       | 是                              | 否                  |
| **MaxLength**       | 指定屬性值的最大長度。                                                                                                                                                                                                           | **Edm. 二進位**、 **edm. 字串**                                                                                                                                                                                                                                                                                                                                                       | 是                              | 否                  |
| **可為 Null**        | 指定屬性是否可以有 **null** 值。                                                                                                                                                                                                     | 所有 **EDMSimpleType** 屬性                                                                                                                                                                                                                                                                                                                                                     | 是                              | 是                 |
| **有效位數**       | 針對 **Decimal**類型的屬性，指定屬性值可擁有的位數。 針對 **Time**、 **DateTime**和 **DateTimeOffset**類型的屬性，指定屬性值的秒數小數部分的位數。 | **Edm. 日期時間**、 **edm**、DateTimeOffset **、edm。****時間**                                                                                                                                                                                                                                                                                                              | 是                              | 否                  |
| **縮放比例**           | 指定屬性值小數點右邊的位數。                                                                                                                                                                      | **Edm.Decimal**                                                                                                                                                                                                                                                                                                                                                                      | 是                              | 否                  |
| **SRID**            | 指定空間系統參考系統識別碼。 如需詳細資訊，請參閱 [SRID](https://en.wikipedia.org/wiki/SRID) 和 [SRID (SQL Server) ](https://msdn.microsoft.com/library/bb964707.aspx)。                                                              | **GeographyPoint，edm. >geographylinestring，Edm. Edm.geographypolygon，Edm. >geographymultipoint，Edm. >geographymultilinestring，Edm. >geographymultipolygon，Edm. GeographyCollection，Edm. >geometrypoint，edm. >geometrylinestring，Edm. >geometrypolygon，edm. >geometrymultipoint，edm. >geometrymultilinestring，edm. >geometrymultipolygon** | 否                               | 是                 |
| **Unicode**         | 指出屬性值是否儲存為 Unicode。                                                                                                                                                                                                    | **Edm.String**                                                                                                                                                                                                                                                                                                                                                                       | 是                              | 是                 |

>[!NOTE]
> 從概念模型產生資料庫時，如果屬性專案在下列命名空間中，[產生資料庫] 將會辨識該**屬性**專案的**StoreGeneratedPattern**屬性值： https://schemas.microsoft.com/ado/2009/02/edm/annotation 。 屬性支援的值為 **Identity** 和 **計算**。 **Identity**的值會產生資料庫資料行，此資料行具有在資料庫中產生的識別值。 **計算**的值將會產生一個資料行，其中包含在資料庫中計算的值。

### <a name="example"></a>範例

下列範例顯示 Facet 套用至實體類型的屬性：

``` xml
 <EntityType Name="Product">
   <Key>
     <PropertyRef Name="ProductId" />
   </Key>
   <Property Type="Int32"
             Name="ProductId" Nullable="false"
             a:StoreGeneratedPattern="Identity"
    xmlns:a="https://schemas.microsoft.com/ado/2009/02/edm/annotation" />
   <Property Type="String"
             Name="ProductName"
             Nullable="false"
             MaxLength="50" />
   <Property Type="String"
             Name="Location"
             Nullable="true"
             MaxLength="25" />
 </EntityType>
```
