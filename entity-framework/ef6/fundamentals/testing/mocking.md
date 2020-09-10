---
title: 使用模擬架構進行測試-EF6
description: 在 Entity Framework 6 中使用模擬 framework 進行測試
author: divega
ms.date: 10/23/2016
ms.assetid: bd66a638-d245-44d4-8e71-b9c6cb335cc7
uid: ef6/fundamentals/testing/mocking
ms.openlocfilehash: 01890ab3bb8dbf0caa7b3eff797e53b06bc8ec9b
ms.sourcegitcommit: 7c3939504bb9da3f46bea3443638b808c04227c2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/09/2020
ms.locfileid: "89618297"
---
# <a name="testing-with-a-mocking-framework"></a>使用模擬架構進行測試
> [!NOTE]
> **僅限 EF6 及更新版本** - Entity Framework 6 已引進此頁面中所討論的功能及 API 等等。 如果您使用的是較早版本，則不適用部分或全部的資訊。  

當您為應用程式撰寫測試時，通常會想要避免叫用資料庫。  Entity Framework 可讓您藉由建立內容（具有您的測試所定義的行為）來達成此目的，以利用記憶體內部資料。  

## <a name="options-for-creating-test-doubles"></a>建立測試雙精度浮點數的選項  

有兩種不同的方法可以用來建立內容的記憶體內版本。  

- **建立您自己的測試雙精度** 浮點數–此方法牽涉到撰寫您自己的內容和 DbSets 的記憶體內部執行。 這可讓您充分掌控類別的行為，但可能牽涉到撰寫和擁有合理的程式碼數量。  
- **使用模擬架構來建立測試雙精度** 浮點數（使用模擬架構 (例如) Moq），您可以擁有內容的記憶體內部執行，並在執行時間以動態方式建立的集合。  

本文將使用模擬架構來處理。 若要建立您自己的測試，請參閱 [使用自己的測試進行測試](xref:ef6/fundamentals/testing/writing-test-doubles)。  

為了示範如何搭配使用 EF 與模擬架構，我們將使用 Moq。 取得 Moq 最簡單的方式是 [從 NuGet 安裝 Moq 套件](https://nuget.org/packages/Moq/)。  

## <a name="testing-with-pre-ef6-versions"></a>使用預先 EF6 的版本進行測試  

本文所示的案例取決於我們在 EF6 中進行 DbSet 的一些變更。 若要使用 EF5 和更早版本進行測試，請參閱 [使用假內容進行測試](https://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/)。  

## <a name="limitations-of-ef-in-memory-test-doubles"></a>EF in memory 測試雙精度浮點數的限制  

記憶體內部測試雙精度浮點數，可提供應用程式中使用 EF 之位的單元測試層級涵蓋範圍。 但是，當您這麼做時，您會使用 LINQ to Objects 針對記憶體中的資料執行查詢。 這可能會導致不同的行為，而不是使用 EF 的 LINQ 提供者 (LINQ to Entities) 將查詢轉譯成針對您的資料庫執行的 SQL。  

這種差異的其中一個範例是載入相關資料。 如果您建立一系列的 Blog，其中每個都有相關的貼文，則在使用記憶體內部資料時，一律會針對每個 Blog 載入相關的文章。 不過，針對資料庫執行時，只有當您使用 Include 方法時，才會載入資料。  

基於這個理由，建議您一律包含部分端對端測試 (除了您的單元測試) ，以確保您的應用程式能針對資料庫正確運作。  

## <a name="following-along-with-this-article"></a>遵循本文  

本文提供完整的程式代碼清單，可讓您視需要複製到 Visual Studio 以進行追蹤。 最簡單的方式是建立 **單元測試專案** ，您必須將 **.NET Framework 4.5** 為目標，才能完成使用 async 的區段。  

## <a name="the-ef-model"></a>EF 模型  

我們即將測試的服務會使用由 BloggingCoNtext 和 Blog 和 Post 類別組成的 EF 模型。 這段程式碼可能由 EF 設計工具產生，或為 Code First 模型。  

``` csharp
using System.Collections.Generic;
using System.Data.Entity;

namespace TestingDemo
{
    public class BloggingContext : DbContext
    {
        public virtual DbSet<Blog> Blogs { get; set; }
        public virtual DbSet<Post> Posts { get; set; }
    }

    public class Blog
    {
        public int BlogId { get; set; }
        public string Name { get; set; }
        public string Url { get; set; }

        public virtual List<Post> Posts { get; set; }
    }

    public class Post
    {
        public int PostId { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }

        public int BlogId { get; set; }
        public virtual Blog Blog { get; set; }
    }
}
```  

### <a name="virtual-dbset-properties-with-ef-designer"></a>使用 EF 設計工具的虛擬 DbSet 屬性  

請注意，內容上的 DbSet 屬性會標示為虛擬。 這可讓模擬架構衍生自我們的內容，並使用模擬的執行覆寫這些屬性。  

如果您使用 Code First 則可以直接編輯類別。 如果您使用 EF 設計工具，則必須編輯產生內容的 T4 範本。 開啟 \<model_name\> 。內嵌在您 edmx 檔案底下的 CoNtext.tt 檔案，尋找下列程式碼片段，並新增至虛擬關鍵字，如下所示。  

``` csharp
public string DbSet(EntitySet entitySet)
{
    return string.Format(
        CultureInfo.InvariantCulture,
        "{0} virtual DbSet\<{1}> {2} {{ get; set; }}",
        Accessibility.ForReadOnlyProperty(entitySet),
        _typeMapper.GetTypeName(entitySet.ElementType),
        _code.Escape(entitySet));
}
```  

## <a name="service-to-be-tested"></a>要測試的服務  

為了示範如何使用記憶體內部測試進行測試，我們將為 BlogService 撰寫幾項測試。 服務能夠建立新的 blog (AddBlog) 並傳回依名稱 (GetAllBlogs) 排序的所有 Blog。 除了 GetAllBlogs 之外，我們也提供了一種方法，會以非同步方式取得依名稱排序的所有 blog (GetAllBlogsAsync) 。  

``` csharp
using System.Collections.Generic;
using System.Data.Entity;
using System.Linq;
using System.Threading.Tasks;

namespace TestingDemo
{
    public class BlogService
    {
        private BloggingContext _context;

        public BlogService(BloggingContext context)
        {
            _context = context;
        }

        public Blog AddBlog(string name, string url)
        {
            var blog = _context.Blogs.Add(new Blog { Name = name, Url = url });
            _context.SaveChanges();

            return blog;
        }

        public List<Blog> GetAllBlogs()
        {
            var query = from b in _context.Blogs
                        orderby b.Name
                        select b;

            return query.ToList();
        }

        public async Task<List<Blog>> GetAllBlogsAsync()
        {
            var query = from b in _context.Blogs
                        orderby b.Name
                        select b;

            return await query.ToListAsync();
        }
    }
}
```  

## <a name="testing-non-query-scenarios"></a>測試非查詢案例  

這就是開始測試非查詢方法所需的一切。 下列測試會使用 Moq 來建立內容。 然後，它會建立 DbSet \<Blog\> ，並將它從內容的 blog 屬性傳回。 接下來，此內容會用來建立新的 BlogService，然後用來建立新的 blog （使用 AddBlog 方法）。 最後，測試會確認服務已在內容上新增新的 Blog 和呼叫 SaveChanges。  

``` csharp
using Microsoft.VisualStudio.TestTools.UnitTesting;
using Moq;
using System.Data.Entity;

namespace TestingDemo
{
    [TestClass]
    public class NonQueryTests
    {
        [TestMethod]
        public void CreateBlog_saves_a_blog_via_context()
        {
            var mockSet = new Mock<DbSet<Blog>>();

            var mockContext = new Mock<BloggingContext>();
            mockContext.Setup(m => m.Blogs).Returns(mockSet.Object);

            var service = new BlogService(mockContext.Object);
            service.AddBlog("ADO.NET Blog", "http://blogs.msdn.com/adonet");

            mockSet.Verify(m => m.Add(It.IsAny<Blog>()), Times.Once());
            mockContext.Verify(m => m.SaveChanges(), Times.Once());
        }
    }
}
```  

## <a name="testing-query-scenarios"></a>測試查詢案例  

為了能夠對我們的 DbSet 測試執行查詢，我們必須設定 IQueryable 的執行。 第一個步驟是建立一些記憶體內部資料，我們使用的是清單 \<Blog\> 。 接下來，我們會建立內容，然後 DBSet \<Blog\> DBSet 的 IQueryable 執行，它們只是委派給搭配 List 的 LINQ to Objects 提供者 \<T\> 。  

接著，我們可以根據測試的雙精度浮點數來建立 BlogService，並確保從 GetAllBlogs 取回的資料會依名稱排序。  

``` csharp
using Microsoft.VisualStudio.TestTools.UnitTesting;
using Moq;
using System.Collections.Generic;
using System.Data.Entity;
using System.Linq;

namespace TestingDemo
{
    [TestClass]
    public class QueryTests
    {
        [TestMethod]
        public void GetAllBlogs_orders_by_name()
        {
            var data = new List<Blog>
            {
                new Blog { Name = "BBB" },
                new Blog { Name = "ZZZ" },
                new Blog { Name = "AAA" },
            }.AsQueryable();

            var mockSet = new Mock<DbSet<Blog>>();
            mockSet.As<IQueryable<Blog>>().Setup(m => m.Provider).Returns(data.Provider);
            mockSet.As<IQueryable<Blog>>().Setup(m => m.Expression).Returns(data.Expression);
            mockSet.As<IQueryable<Blog>>().Setup(m => m.ElementType).Returns(data.ElementType);
            mockSet.As<IQueryable<Blog>>().Setup(m => m.GetEnumerator()).Returns(data.GetEnumerator());

            var mockContext = new Mock<BloggingContext>();
            mockContext.Setup(c => c.Blogs).Returns(mockSet.Object);

            var service = new BlogService(mockContext.Object);
            var blogs = service.GetAllBlogs();

            Assert.AreEqual(3, blogs.Count);
            Assert.AreEqual("AAA", blogs[0].Name);
            Assert.AreEqual("BBB", blogs[1].Name);
            Assert.AreEqual("ZZZ", blogs[2].Name);
        }
    }
}
```  

### <a name="testing-with-async-queries"></a>使用非同步查詢進行測試

Entity Framework 6 引進了一組擴充方法，可用來以非同步方式執行查詢。 這些方法的範例包括 ToListAsync、FirstAsync、ForEachAsync 等等。  

因為 Entity Framework 的查詢會使用 LINQ，所以會在 IQueryable 和 IEnumerable 上定義擴充方法。 不過，因為它們只設計用來搭配 Entity Framework 如果您嘗試在不是 Entity Framework 查詢的 LINQ 查詢上使用它們，您可能會收到下列錯誤：

> 來源 IQueryable 不會執行 IDbAsyncEnumerable {0} 。 只有執行 IDbAsyncEnumerable 的來源可以用於 Entity Framework 非同步作業。 如需詳細資訊，請參閱 [http://go.microsoft.com/fwlink/?LinkId=287068](https://go.microsoft.com/fwlink/?LinkId=287068) 。  

雖然非同步方法只有在針對 EF 查詢執行時才受支援，但您可能會想要在針對 DbSet 的記憶體中測試雙精度執行時，在單元測試中使用這些方法。  

若要使用非同步方法，我們必須建立記憶體中的 DbAsyncQueryProvider 來處理非同步查詢。 雖然您可以使用 Moq 來設定查詢提供者，但在程式碼中建立測試雙精度更容易。 這項實行的程式碼如下所示：  

``` csharp
using System.Collections.Generic;
using System.Data.Entity.Infrastructure;
using System.Linq;
using System.Linq.Expressions;
using System.Threading;
using System.Threading.Tasks;

namespace TestingDemo
{
    internal class TestDbAsyncQueryProvider<TEntity> : IDbAsyncQueryProvider
    {
        private readonly IQueryProvider _inner;

        internal TestDbAsyncQueryProvider(IQueryProvider inner)
        {
            _inner = inner;
        }

        public IQueryable CreateQuery(Expression expression)
        {
            return new TestDbAsyncEnumerable<TEntity>(expression);
        }

        public IQueryable<TElement> CreateQuery<TElement>(Expression expression)
        {
            return new TestDbAsyncEnumerable<TElement>(expression);
        }

        public object Execute(Expression expression)
        {
            return _inner.Execute(expression);
        }

        public TResult Execute<TResult>(Expression expression)
        {
            return _inner.Execute<TResult>(expression);
        }

        public Task<object> ExecuteAsync(Expression expression, CancellationToken cancellationToken)
        {
            return Task.FromResult(Execute(expression));
        }

        public Task<TResult> ExecuteAsync<TResult>(Expression expression, CancellationToken cancellationToken)
        {
            return Task.FromResult(Execute<TResult>(expression));
        }
    }

    internal class TestDbAsyncEnumerable<T> : EnumerableQuery<T>, IDbAsyncEnumerable<T>, IQueryable<T>
    {
        public TestDbAsyncEnumerable(IEnumerable<T> enumerable)
            : base(enumerable)
        { }

        public TestDbAsyncEnumerable(Expression expression)
            : base(expression)
        { }

        public IDbAsyncEnumerator<T> GetAsyncEnumerator()
        {
            return new TestDbAsyncEnumerator<T>(this.AsEnumerable().GetEnumerator());
        }

        IDbAsyncEnumerator IDbAsyncEnumerable.GetAsyncEnumerator()
        {
            return GetAsyncEnumerator();
        }

        IQueryProvider IQueryable.Provider
        {
            get { return new TestDbAsyncQueryProvider<T>(this); }
        }
    }

    internal class TestDbAsyncEnumerator<T> : IDbAsyncEnumerator<T>
    {
        private readonly IEnumerator<T> _inner;

        public TestDbAsyncEnumerator(IEnumerator<T> inner)
        {
            _inner = inner;
        }

        public void Dispose()
        {
            _inner.Dispose();
        }

        public Task<bool> MoveNextAsync(CancellationToken cancellationToken)
        {
            return Task.FromResult(_inner.MoveNext());
        }

        public T Current
        {
            get { return _inner.Current; }
        }

        object IDbAsyncEnumerator.Current
        {
            get { return Current; }
        }
    }
}
```  

現在我們已經有了非同步查詢提供者，我們可以為新的 GetAllBlogsAsync 方法撰寫單元測試。  

``` csharp
using Microsoft.VisualStudio.TestTools.UnitTesting;
using Moq;
using System.Collections.Generic;
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Linq;
using System.Threading.Tasks;

namespace TestingDemo
{
    [TestClass]
    public class AsyncQueryTests
    {
        [TestMethod]
        public async Task GetAllBlogsAsync_orders_by_name()
        {

            var data = new List<Blog>
            {
                new Blog { Name = "BBB" },
                new Blog { Name = "ZZZ" },
                new Blog { Name = "AAA" },
            }.AsQueryable();

            var mockSet = new Mock<DbSet<Blog>>();
            mockSet.As<IDbAsyncEnumerable<Blog>>()
                .Setup(m => m.GetAsyncEnumerator())
                .Returns(new TestDbAsyncEnumerator<Blog>(data.GetEnumerator()));

            mockSet.As<IQueryable<Blog>>()
                .Setup(m => m.Provider)
                .Returns(new TestDbAsyncQueryProvider<Blog>(data.Provider));

            mockSet.As<IQueryable<Blog>>().Setup(m => m.Expression).Returns(data.Expression);
            mockSet.As<IQueryable<Blog>>().Setup(m => m.ElementType).Returns(data.ElementType);
            mockSet.As<IQueryable<Blog>>().Setup(m => m.GetEnumerator()).Returns(data.GetEnumerator());

            var mockContext = new Mock<BloggingContext>();
            mockContext.Setup(c => c.Blogs).Returns(mockSet.Object);

            var service = new BlogService(mockContext.Object);
            var blogs = await service.GetAllBlogsAsync();

            Assert.AreEqual(3, blogs.Count);
            Assert.AreEqual("AAA", blogs[0].Name);
            Assert.AreEqual("BBB", blogs[1].Name);
            Assert.AreEqual("ZZZ", blogs[2].Name);
        }
    }
}
```  
