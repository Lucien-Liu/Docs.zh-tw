---
title: 使用 Entity Framework Core 實作基礎結構持續層
description: 容器化 .NET 應用程式的 .NET 微服務架構 |使用 Entity Framework Core 探索基礎結構持續性層的執行詳細資料。
ms.date: 01/30/2020
ms.openlocfilehash: 878d4d64e92ca92fd2393d60d496f1c5671e7029
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/24/2020
ms.locfileid: "91172348"
---
# <a name="implement-the-infrastructure-persistence-layer-with-entity-framework-core"></a>使用 Entity Framework Core 實作基礎結構持續層

當您使用例如 SQL Server、Oracle 或 PostgreSQL 等關聯式資料庫時，建議的方法是根據 Entity Framework (EF) 來實作持續層。 EF 支援 LINQ，並為您的模型提供強型別的物件，以及資料庫的簡化持續性。

Entity Framework 是 .NET Framework 的一部分，有著悠久的歷史。 當您使用 .NET Core 時，您也應該使用 Entity Framework Core，它在 Windows 或 Linux 上以與 .NET Core 相同的方式執行。 EF Core 是完全重寫的 Entity Framework，以較小的使用量和效能的重要改善來實行。

## <a name="introduction-to-entity-framework-core"></a>Entity Framework Core 簡介

Entity Framework (EF) Core 是常見 Entity Framework 資料存取技術的輕量型、可擴充且跨平台版本。 它是在 2016 年中在 .NET Core 引進。

因為 Microsoft 文件中已有 EF Core 的簡介，在這裡我們只是提供該資訊的連結。

### <a name="additional-resources"></a>其他資源

- **Entity Framework Core** \
  [https://docs.microsoft.com/ef/core/](/ef/core/)

- **開始使用 ASP.NET Core 和 Entity Framework Core 使用 Visual Studio** \
  [https://docs.microsoft.com/aspnet/core/data/ef-mvc/](/aspnet/core/data/ef-mvc/)

- **DbCoNtext 類別** \
  [https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbcontext](xref:Microsoft.EntityFrameworkCore.DbContext)

- **比較 EF Core & EF6** \
  [https://docs.microsoft.com/ef/efcore-and-ef6/index](/ef/efcore-and-ef6/index)

## <a name="infrastructure-in-entity-framework-core-from-a-ddd-perspective"></a>DDD 觀點的 Entity Framework Core 基礎結構

從 DDD 觀點來看，EF 的一項重要功能是能夠使用 POCO 網域實體，在 EF 術語中也稱為 POCO「程式碼優先的實體」**。 如果您使用 POCO 網域實體，您的網域模型類別會是持續性無知 (persistence-ignorant)，並遵循 [Persistence Ignorance](https://deviq.com/persistence-ignorance/) (持續性無知) 和 [Infrastructure Ignorance](https://ayende.com/blog/3137/infrastructure-ignorance) (基礎結構無知) 準則。

根據 DDD 模式，您應該將網域行為和規則封裝在實體類別本身內，讓它可以在存取任何集合時控制非變異項目、驗證及規則。 因此，在 DDD 中允許對子系實體或值物件之集合的公用存取權並不是一項好的做法。 相反地，您會想要公開可控制欄位和屬性集合更新方式與時機，以及發生這種情況時應發生哪些行為和動作的方法。

從 EF Core 1.1 開始，要滿足那些 DDD 需求，您可以在實體中使用純欄位，而不要使用公用屬性。 若不想讓實體欄位可供外部存取，只需建立屬性 (attribute) 或欄位，而不是屬性 (property)。 您也可以使用私用屬性設定程式。

同樣地，您現在可以使用類型為 `IReadOnlyCollection<T>` 的公用屬性唯讀地存取集合，其背後的支持是依賴 EF 以獲得持續性的實體中，集合的私用欄位成員 (例如 `List<T>`)。 舊版的 Entity Framework 需要屬性集合支援 `ICollection<T>`，這表示任何使用父實體類別的開發人員都可以透過屬性集合來新增或移除項目。 該項可能性違反了 DDD 中的建議模式。

您可以使用私用集合，同時公開唯讀 `IReadOnlyCollection<T>` 物件，如下列程式碼範例所示：

```csharp
public class Order : Entity
{
    // Using private fields, allowed since EF Core 1.1
    private DateTime _orderDate;
    // Other fields ...

    private readonly List<OrderItem> _orderItems;
    public IReadOnlyCollection<OrderItem> OrderItems => _orderItems;

    protected Order() { }

    public Order(int buyerId, int paymentMethodId, Address address)
    {
        // Initializations ...
    }

    public void AddOrderItem(int productId, string productName,
                             decimal unitPrice, decimal discount,
                             string pictureUrl, int units = 1)
    {
        // Validation logic...

        var orderItem = new OrderItem(productId, productName,
                                      unitPrice, discount,
                                      pictureUrl, units);
        _orderItems.Add(orderItem);
    }
}
```

`OrderItems`屬性只能使用的唯讀存取 `IReadOnlyCollection<OrderItem>` 。 此類型為唯讀，因此它會受到保護，可避免定期的外部更新。

EF Core 可用來將網域模型對應至實體資料庫，而不含「破壞」網域模型。 它是純粹的 POCO.NET 程式碼，因為對應動作在持續層中實作。 在該對應動作中，您需要設定欄位到資料庫的對應。 在下列來自 `OrderingContext` 和 `OrderEntityTypeConfiguration` 類別的 `OnModelCreating` 方法範例中，呼叫 `SetPropertyAccessMode` 通知 EF Core 透過其欄位存取 `OrderItems` 屬性。

```csharp
// At OrderingContext.cs from eShopOnContainers
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
   // ...
   modelBuilder.ApplyConfiguration(new OrderEntityTypeConfiguration());
   // Other entities' configuration ...
}

// At OrderEntityTypeConfiguration.cs from eShopOnContainers
class OrderEntityTypeConfiguration : IEntityTypeConfiguration<Order>
{
    public void Configure(EntityTypeBuilder<Order> orderConfiguration)
    {
        orderConfiguration.ToTable("orders", OrderingContext.DEFAULT_SCHEMA);
        // Other configuration

        var navigation =
              orderConfiguration.Metadata.FindNavigation(nameof(Order.OrderItems));

        //EF access the OrderItem collection property through its backing field
        navigation.SetPropertyAccessMode(PropertyAccessMode.Field);

        // Other configuration
    }
}
```

當您使用欄位而非屬性時， `OrderItem` 實體會保存為具有 `List<OrderItem>` 屬性的屬性。 不過，它會公開單一存取子，亦即 `AddOrderItem` 方法，將新的項目新增至訂單中。 因此，行為和資料會繫結在一起，且將在使用網域模型的任何應用程式程式碼中都一致。

## <a name="implement-custom-repositories-with-entity-framework-core"></a>使用 Entity Framework Core 實作自訂存放庫

在實作層級，存放庫只是具有資料持續性程式碼的類別，在執行更新時會由工作單位 (EF Core 中的 DBContext) 所協調，如下列類別中所示：

```csharp
// using directives...
namespace Microsoft.eShopOnContainers.Services.Ordering.Infrastructure.Repositories
{
    public class BuyerRepository : IBuyerRepository
    {
        private readonly OrderingContext _context;
        public IUnitOfWork UnitOfWork
        {
            get
            {
                return _context;
            }
        }

        public BuyerRepository(OrderingContext context)
        {
            _context = context ?? throw new ArgumentNullException(nameof(context));
        }

        public Buyer Add(Buyer buyer)
        {
            return _context.Buyers.Add(buyer).Entity;
        }

        public async Task<Buyer> FindAsync(string buyerIdentityGuid)
        {
            var buyer = await _context.Buyers
                .Include(b => b.Payments)
                .Where(b => b.FullName == buyerIdentityGuid)
                .SingleOrDefaultAsync();

            return buyer;
        }
    }
}
```

`IBuyerRepository`介面來自領域模型層作為合約。 不過，存放庫實作是在持續層和基礎結構層進行。

EF DbContext 是透過相依性插入而通過建構函式。 它會在相同 HTTP 要求範圍內的多個存放庫之間共用，這點受惠於 IoC 容器 (也可以明確使用 `services.AddDbContext<>` 設定) 中的預設存留期 (`ServiceLifetime.Scoped`)。

### <a name="methods-to-implement-in-a-repository-updates-or-transactions-versus-queries"></a>要在存放庫實作的方法 (更新或交易與查詢)

在每個存放庫類別中，您應該放置更新其相關彙總所包含之實體狀態的持續性方法。 請記住，彙總與它相關存放庫之間有一對一的關聯性。 請考慮彙總根實體物件在其 EF 圖形內可能會有內嵌的子實體。 例如，買方可能會有多個付款方法，作為相關的子實體。

由於 eShopOnContainers 中排序微服務的方法也是基於 CQS/CQRS，大多數的查詢不會在自訂存放庫中實作。 開發人員可以自由地為展示層建立所需的查詢和聯結，而且一般而言沒有彙總、關於彙總的自訂存放庫以及 DDD 等所加諸的限制。 本指南所建議的大部分自訂存放庫都有數個更新或交易式的方法，但只有查詢方法必須取得資料以便更新。 例如，BuyerRepository 存放庫會實作 FindAsync 方法，因為應用程式必須知道特定買方是否存在，然後才能建立與訂單相關的新買方。

不過，實際取得資料以便傳送至展示層或用戶端應用程式的查詢方法，會如所述地在根據使用 Dapper 之彈性查詢的 CQRS 查詢中實作。

### <a name="using-a-custom-repository-versus-using-ef-dbcontext-directly"></a>使用自訂存放庫與直接使用 EF DbContext

Entity Framework 的 DbCoNtext 類別是以工作單位和存放庫模式為基礎，而且可以直接從您的程式碼（例如從 ASP.NET Core MVC 控制器）使用。 工作單位和存放庫模式會產生最簡單的程式碼，如同 eShopOnContainers 中的 CRUD 目錄微服務。 當您想要盡可能簡單的程式碼時，可以直接使用 DbContext 類別，有許多開發人員都這麼做。

不過，實作自訂存放庫在實作更複雜的微服務或應用程式時提供數個優點。 工作單位和存放庫模式的目的是要封裝基礎結構持續性層，使其與應用程式和領域模型層分離。 實作這些模式，可以方便使用模擬資料庫存取權的模擬存放庫。

在圖7-18 中，您可以看到不使用存放庫 (直接使用 EF DbCoNtext) 與使用儲存機制之間的差異，讓您更輕鬆地模擬這些存放庫。

![此圖顯示兩個存放庫中的元件和資料流程。](./media/infrastructure-persistence-layer-implementation-entity-framework-core/custom-repo-versus-db-context.png)

**圖 7-18**。 使用自訂存放庫與一般 DbContext

圖7-18 顯示使用自訂存放庫時，您可以藉由模擬存放庫，新增可用來簡化測試的抽象層。 在模擬時有多種替代方式。 您可以只模擬存放庫，或是模擬整個工作單位。 通常只模擬存放庫便已足夠，通常不需要抽象與模擬整個工作單位的複雜性。

稍後，當我們將重點放在應用程式層上，您會看到相依性插入在 ASP.NET Core 中的運作方式，以及在使用存放庫時的實作方式。

簡單地說，自訂存放庫可讓您更輕鬆地使用不受資料層狀態影響的單元測試來測試程式碼。 如果您執行的測試也會透過 Entity Framework 存取實際資料庫，它們便不是單元測試而是整合測試，速度會慢上許多。

如果直接使用 DbContext，您可能必須模擬它，或使用記憶體內部的 SQL Server 和單元測試的可預測資料，來執行單元測試。 但是模擬 DbContext 或控制假資料，所需工作量比存放庫層級的模擬還大。 當然，您一律可以測試 MVC 控制器。

## <a name="ef-dbcontext-and-iunitofwork-instance-lifetime-in-your-ioc-container"></a>IoC 容器中的 EF DbContext 和 IUnitOfWork 執行個體存留期

`DbContext` 物件 (公開為 `IUnitOfWork` 物件) 應在相同 HTTP 要求範圍內的多個存放庫之間共用。 比方說，當正在執行的作業必須處理多個彙總時，或是因為您正在使用多個存放庫執行個體時，便是如此。 還有一點也很重要，`IUnitOfWork` 介面是網域層的一部分，不是 EF Core 類型。

為了這樣做，`DbContext` 物件的執行個體必須將其服務存留期設定為 ServiceLifetime.Scoped。 這是在您的 ASP.NET Core Web API 專案中，從 `Startup.cs` 檔案的 ConfigureServices 方法，在 IoC 容器中向 `services.AddDbContext` 登錄 `DbContext` 時的預設存留期。 下列程式碼會說明這一點。

```csharp
public IServiceProvider ConfigureServices(IServiceCollection services)
{
    // Add framework services.
    services.AddMvc(options =>
    {
        options.Filters.Add(typeof(HttpGlobalExceptionFilter));
    }).AddControllersAsServices();

    services.AddEntityFrameworkSqlServer()
      .AddDbContext<OrderingContext>(options =>
      {
          options.UseSqlServer(Configuration["ConnectionString"],
                               sqlOptions => sqlOptions.MigrationsAssembly(typeof(Startup).GetTypeInfo().
                                                                                    Assembly.GetName().Name));
      },
      ServiceLifetime.Scoped // Note that Scoped is the default choice
                             // in AddDbContext. It is shown here only for
                             // pedagogic purposes.
      );
}
```

DbContext 具現化模式不應該設定為 ServiceLifetime.Transient 或 ServiceLifetime.Singleton。

## <a name="the-repository-instance-lifetime-in-your-ioc-container"></a>IoC 容器中的存放庫執行個體存留期

以類似的方式，存放庫的存留期通常應該設定為 Autofac) 中 (Autofac 的範圍。 它也可能是暫時性 (Autofac 中的 InstancePerDependency)，但您的服務在使用已設定範圍的存留期時，在記憶體方面會更有效率。

```csharp
// Registering a Repository in Autofac IoC container
builder.RegisterType<OrderRepository>()
    .As<IOrderRepository>()
    .InstancePerLifetimeScope();
```

使用存放庫的 singleton 存留期，可能會在您的 DbCoNtext 設定為限域的 (Autofac) 存留期 (DBCoNtext) 的預設存留期時，造成嚴重的並行問題。

### <a name="additional-resources"></a>其他資源

- **在 ASP.NET MVC 應用程式中執行存放庫和工作單元模式** \
  <https://www.asp.net/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application>

- **Jonathan Allen。使用 Entity Framework、Dapper 和連鎖的儲存機制模式的實現策略** \
  <https://www.infoq.com/articles/repository-implementation-strategies>

- **Cesar de la Torre。比較 ASP.NET Core IoC 容器服務存留期與 Autofac IoC 容器實例範圍** \
  <https://devblogs.microsoft.com/cesardelatorre/comparing-asp-net-core-ioc-service-life-times-and-autofac-ioc-instance-scopes/>

## <a name="table-mapping"></a>資料表對應

資料表對應會識別要從中查詢的資料表資料，並儲存到資料庫。 先前您已看到如何使用網域實體 (例如產品或訂單網域) 來產生相關的資料庫結構描述。 EF 的設計高度圍繞著「慣例」** 的概念。 慣例會解決像是「資料表名稱是什麼？」的問題。 或「主要金鑰是什麼屬性？」 慣例通常是以傳統名稱為基礎。 例如，主要索引鍵通常是以結尾的屬性 `Id` 。

依照慣例，每個實體都會設定成對應至與 `DbSet<TEntity>` 屬性同名的資料表，該屬性會在衍生內容上公開實體。 如不為指定的實體提供 `DbSet<TEntity>` 值，即使用類別名稱。

### <a name="data-annotations-versus-fluent-api"></a>資料註解與 Fluent API

有許多其他的 EF Core 慣例，而且大部分可以使用資料註解或 Fluent API 來變更 (在 OnModelCreating 方法內實作)。

資料註解必須用於實體模型類別本身，這從 DDD 觀點來看是較多干擾的方法。 這是因為您會使用與基礎結構資料庫相關的資料註解破壞您的模型。 在另一方面，Fluent API 能夠方便地變更資料持續性基礎結構層內的大部分慣例和對應，所以實體模型會很清楚且與持續性基礎結構分開。

### <a name="fluent-api-and-the-onmodelcreating-method"></a>Fluent API 和 OnModelCreating 方法

如前所述，為了變更慣例和對應，您可以在 DbContext 類別中使用 OnModelCreating 方法。

eShopOnContainers 中的排序微服務，會視需要實作明確的對應和設定，如下列程式碼所示。

```csharp
// At OrderingContext.cs from eShopOnContainers
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
   // ...
   modelBuilder.ApplyConfiguration(new OrderEntityTypeConfiguration());
   // Other entities' configuration ...
}

// At OrderEntityTypeConfiguration.cs from eShopOnContainers
class OrderEntityTypeConfiguration : IEntityTypeConfiguration<Order>
{
    public void Configure(EntityTypeBuilder<Order> orderConfiguration)
    {
        orderConfiguration.ToTable("orders", OrderingContext.DEFAULT_SCHEMA);

        orderConfiguration.HasKey(o => o.Id);

        orderConfiguration.Ignore(b => b.DomainEvents);

        orderConfiguration.Property(o => o.Id)
            .UseHiLo("orderseq", OrderingContext.DEFAULT_SCHEMA);

        //Address value object persisted as owned entity type supported since EF Core 2.0
        orderConfiguration
            .OwnsOne(o => o.Address, a =>
            {
                a.WithOwner();
            });

        orderConfiguration
            .Property<int?>("_buyerId")
            .UsePropertyAccessMode(PropertyAccessMode.Field)
            .HasColumnName("BuyerId")
            .IsRequired(false);

        orderConfiguration
            .Property<DateTime>("_orderDate")
            .UsePropertyAccessMode(PropertyAccessMode.Field)
            .HasColumnName("OrderDate")
            .IsRequired();

        orderConfiguration
            .Property<int>("_orderStatusId")
            .UsePropertyAccessMode(PropertyAccessMode.Field)
            .HasColumnName("OrderStatusId")
            .IsRequired();

        orderConfiguration
            .Property<int?>("_paymentMethodId")
            .UsePropertyAccessMode(PropertyAccessMode.Field)
            .HasColumnName("PaymentMethodId")
            .IsRequired(false);

        orderConfiguration.Property<string>("Description").IsRequired(false);

        var navigation = orderConfiguration.Metadata.FindNavigation(nameof(Order.OrderItems));

        // DDD Patterns comment:
        //Set as field (New since EF 1.1) to access the OrderItem collection property through its field
        navigation.SetPropertyAccessMode(PropertyAccessMode.Field);

        orderConfiguration.HasOne<PaymentMethod>()
            .WithMany()
            .HasForeignKey("_paymentMethodId")
            .IsRequired(false)
            .OnDelete(DeleteBehavior.Restrict);

        orderConfiguration.HasOne<Buyer>()
            .WithMany()
            .IsRequired(false)
            .HasForeignKey("_buyerId");

        orderConfiguration.HasOne(o => o.OrderStatus)
            .WithMany()
            .HasForeignKey("_orderStatusId");
    }
}
```

您可以在相同的方法中設定所有的流暢 API 對應 `OnModelCreating` ，但建議您分割該程式碼，並擁有多個設定類別，每個實體一個，如範例所示。 尤其針對大型模型，建議使用不同的設定類別來設定不同的實體類型。

範例中的程式碼顯示一些明確宣告和對應。 不過，EF Core 慣例會自動執行許多對應，因此在您的案例中需要的實際程式碼可能會較小。

### <a name="the-hilo-algorithm-in-ef-core"></a>EF Core 中的 Hi/Lo 演算法

上述範例中的程式碼有一個有趣的層面是，它會使用[Hi/Lo 演算法](https://vladmihalcea.com/the-hilo-algorithm/)作為金鑰產生策略。

當您需要唯一索引鍵才能認可變更時，Hi/Lo 演算法十分有用。 總而言之，Hi-Lo 演算法會將唯一識別碼指派給資料表資料列，而不依賴將資料列立即儲存在資料庫中。 這可讓您立即開始使用識別碼，如同一般循序資料庫識別碼那樣。

Hi/Lo 演算法描述從相關的料庫序列取得一批唯一識別碼的機制。 這些識別碼沒有使用的安全顧慮，因為資料庫可保證唯一性，所以使用者之間不會發生衝突。 基於這些理由，這個演算法是有趣的：

- 它不會中斷工作單位模式。

- 它會分批取得序列識別碼，減少對資料庫的反覆存取。

- 它會產生人類可閱讀的識別碼，而不像使用 GUID 的技術。

EF Core 支援使用方法進行 [HiLo](https://stackoverflow.com/questions/282099/whats-the-hi-lo-algorithm) `UseHiLo` ，如上述範例所示。

### <a name="map-fields-instead-of-properties"></a>對應欄位，而不是對應屬性

利用此功能 (從 EF Core 1.1 開始提供)，您可以直接將資料行對應至欄位。 您可以不在實體類別中使用屬性，而只是將資料行從資料表對應至欄位。 常見的用途是不需要從實體外部存取之任何內部狀態的私用欄位。

您可以使用單一欄位或集合 (例如 `List<>` 欄位) 來這麼做。 當我們稍早討論模型化網域模型類別時已提到此點，但在這裡您可以藉由先前程式碼中反白顯示的 `PropertyAccessMode.Field`　組態，看到該對應的執行方式。

### <a name="use-shadow-properties-in-ef-core-hidden-at-the-infrastructure-level"></a>使用隱藏在基礎結構層之 EF Core 的陰影屬性

在 EF Core 中，陰影屬性是不存在於您實體類別模型的屬性。 這些屬性的值和狀態會單純地在基礎結構層的 [ChangeTracker](/ef/core/api/microsoft.entityframeworkcore.changetracking.changetracker) 類別中維護。

## <a name="implement-the-query-specification-pattern"></a>實作查詢規格模式

如稍早在設計一節中所介紹，查詢規格模式是網域導向的設計模式，設計成您可以放置查詢定義以及選擇性排序和分頁邏輯的位置。

查詢規格模式會定義物件中的查詢。 例如，若要封裝搜尋一些產品的分頁查詢，您可以建立 PagedProduct 規格來接受必要的輸入參數 (pageNumber、pageSize、filter 等)。 然後，在任何存放庫方法中 (通常是 List() 多載)，它會接受 IQuerySpecification，並根據該規格執行預期的查詢。

泛型規格介面的一個範例是來自 [eShopOnWeb](https://github.com/dotnet-architecture/eShopOnWeb) 的下列程式碼。

```csharp
// GENERIC SPECIFICATION INTERFACE
// https://github.com/dotnet-architecture/eShopOnWeb

public interface ISpecification<T>
{
    Expression<Func<T, bool>> Criteria { get; }
    List<Expression<Func<T, object>>> Includes { get; }
    List<string> IncludeStrings { get; }
}
```

然後，泛型規格基底類別的實作如下所示。

```csharp
// GENERIC SPECIFICATION IMPLEMENTATION (BASE CLASS)
// https://github.com/dotnet-architecture/eShopOnWeb

public abstract class BaseSpecification<T> : ISpecification<T>
{
    public BaseSpecification(Expression<Func<T, bool>> criteria)
    {
        Criteria = criteria;
    }
    public Expression<Func<T, bool>> Criteria { get; }

    public List<Expression<Func<T, object>>> Includes { get; } =
                                           new List<Expression<Func<T, object>>>();

    public List<string> IncludeStrings { get; } = new List<string>();

    protected virtual void AddInclude(Expression<Func<T, object>> includeExpression)
    {
        Includes.Add(includeExpression);
    }

    // string-based includes allow for including children of children
    // e.g. Basket.Items.Product
    protected virtual void AddInclude(string includeString)
    {
        IncludeStrings.Add(includeString);
    }
}
```

下列規格會載入單一購物籃實體，指定購物籃的識別碼或購物籃所屬買方的識別碼。 它會 [立即載入](/ef/core/querying/related-data) 購物籃的 `Items` 集合。

```csharp
// SAMPLE QUERY SPECIFICATION IMPLEMENTATION

public class BasketWithItemsSpecification : BaseSpecification<Basket>
{
    public BasketWithItemsSpecification(int basketId)
        : base(b => b.Id == basketId)
    {
        AddInclude(b => b.Items);
    }

    public BasketWithItemsSpecification(string buyerId)
        : base(b => b.BuyerId == buyerId)
    {
        AddInclude(b => b.Items);
    }
}
```

最後，您可以在下面看到泛型 EF 存放庫如何使用這類的規格來篩選與立即載入與指定實體類型 T 相關的資料。

```csharp
// GENERIC EF REPOSITORY WITH SPECIFICATION
// https://github.com/dotnet-architecture/eShopOnWeb

public IEnumerable<T> List(ISpecification<T> spec)
{
    // fetch a Queryable that includes all expression-based includes
    var queryableResultWithIncludes = spec.Includes
        .Aggregate(_dbContext.Set<T>().AsQueryable(),
            (current, include) => current.Include(include));

    // modify the IQueryable to include any string-based include statements
    var secondaryResult = spec.IncludeStrings
        .Aggregate(queryableResultWithIncludes,
            (current, include) => current.Include(include));

    // return the result of the query using the specification's criteria expression
    return secondaryResult
                    .Where(spec.Criteria)
                    .AsEnumerable();
}
```

除了封裝篩選邏輯，規格還可以指定要傳回的資料形式，包括要填入的屬性。

雖然我們不建議從存放庫傳回，但在存放 `IQueryable` 庫中使用它們來建立一組結果是不錯的方法。 您可以在上面的清單方法中看到這個方法，它會使用中繼 `IQueryable` 運算式來建立查詢的包含清單，然後再以最後一行上的規格準則執行查詢。

### <a name="additional-resources"></a>其他資源

- **資料表對應** \
  [https://docs.microsoft.com/ef/core/modeling/relational/tables](/ef/core/modeling/relational/tables)

- **使用 HiLo 來產生 Entity Framework Core 的金鑰** \
  <https://www.talkingdotnet.com/use-hilo-to-generate-keys-with-entity-framework-core/>

- **支援欄位** \
  [https://docs.microsoft.com/ef/core/modeling/backing-field](/ef/core/modeling/backing-field)

- **Steve Smith。Entity Framework Core 中封裝的集合** \
  <https://ardalis.com/encapsulated-collections-in-entity-framework-core>

- **陰影屬性** \
  [https://docs.microsoft.com/ef/core/modeling/shadow-properties](/ef/core/modeling/shadow-properties)

- **規格模式** \
  <https://deviq.com/specification-pattern/>

> [!div class="step-by-step"]
> [上一個](infrastructure-persistence-layer-design.md) 
> [下一步](nosql-database-persistence-infrastructure.md)
