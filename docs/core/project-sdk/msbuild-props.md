---
title: 適用于 Microsoft .NET 的 MSBuild 屬性
description: .NET SDK 瞭解的 MSBuild 屬性和專案參考。
ms.date: 02/14/2020
ms.topic: reference
ms.custom: updateeachrelease
ms.openlocfilehash: 3b58fd080439c73ee30d5c8dc59c50c0410db164
ms.sourcegitcommit: 45c7148f2483db2501c1aa696ab6ed2ed8cb71b2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/08/2020
ms.locfileid: "96851571"
---
# <a name="msbuild-reference-for-net-sdk-projects"></a>.NET SDK 專案的 MSBuild 參考

此頁面是 MSBuild 屬性和專案的參考，您可以使用這些屬性來設定 .NET 專案。

> [!NOTE]
> 此頁面為進行中的工作，不會列出 .NET SDK 的所有實用 MSBuild 屬性。 如需常見 MSBuild 屬性的清單，請參閱 [一般的 msbuild 屬性](/visualstudio/msbuild/common-msbuild-project-properties)。

## <a name="framework-properties"></a>架構屬性

- [TargetFramework](#targetframework)
- [TargetFrameworks](#targetframeworks)
- [NetStandardImplicitPackageVersion](#netstandardimplicitpackageversion)

### <a name="targetframework"></a>TargetFramework

`TargetFramework`屬性會指定應用程式的目標 framework 版本。 如需有效的目標 framework 名字標記清單，請參閱 [SDK 樣式專案中的目標](../../standard/frameworks.md#supported-target-frameworks)framework。

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp3.1</TargetFramework>
</PropertyGroup>
```

如需詳細資訊，請參閱 [SDK 樣式專案中的目標 framework](../../standard/frameworks.md)。

### <a name="targetframeworks"></a>TargetFrameworks

`TargetFrameworks`當您想要讓應用程式以多個平臺為目標時，請使用屬性。 如需有效的目標 framework 名字標記清單，請參閱 [SDK 樣式專案中的目標](../../standard/frameworks.md#supported-target-frameworks)framework。

> [!NOTE]
> 如果 `TargetFramework` 指定了 (單數) ，就會忽略這個屬性。

```xml
<PropertyGroup>
  <TargetFrameworks>netcoreapp3.1;net462</TargetFrameworks>
</PropertyGroup>
```

如需詳細資訊，請參閱 [SDK 樣式專案中的目標 framework](../../standard/frameworks.md)。

### <a name="netstandardimplicitpackageversion"></a>NetStandardImplicitPackageVersion

> [!NOTE]
> 這個屬性只適用于使用 `netstandard1.x` 的專案。 它不適用於使用的專案 `netstandard2.x` 。

`NetStandardImplicitPackageVersion`當您想要指定的 framework 版本低於中繼套件版本時，請使用屬性。 下列範例中的專案檔會以 `netstandard1.3` 1.6.0 版本為目標，但會使用 `NETStandard.Library` 。

```xml
<PropertyGroup>
  <TargetFramework>netstandard1.3</TargetFramework>
  <NetStandardImplicitPackageVersion>1.6.0</NetStandardImplicitPackageVersion>
</PropertyGroup>
```

## <a name="package-properties"></a>封裝屬性

您可以指定屬性，例如 `PackageId` 、 `PackageVersion` 、 `PackageIcon` 、 `Title` 和， `Description` 以描述從專案建立的封裝。 如需這些屬性和其他屬性的詳細資訊，請參閱 [套件目標](/nuget/reference/msbuild-targets#pack-target)。

```xml
<PropertyGroup>
  ...
  <PackageId>ClassLibDotNetStandard</PackageId>
  <Version>1.0.0</Version>
  <Authors>John Doe</Authors>
  <Company>Contoso</Company>
</PropertyGroup>
```

## <a name="publish-properties-and-items"></a>發佈屬性和專案

- [CopyLocalLockFileAssemblies](#copylocallockfileassemblies)
- [RuntimeIdentifier](#runtimeidentifier)
- [RuntimeIdentifiers](#runtimeidentifiers)
- [TrimmerRootAssembly](#trimmerrootassembly)
- [UseAppHost](#useapphost)

### <a name="copylocallockfileassemblies"></a>CopyLocalLockFileAssemblies

`CopyLocalLockFileAssemblies`屬性適用于具有其他程式庫相依性的外掛程式專案。 如果您將此屬性設定為 `true` ，任何 NuGet 套件相依性都會複製到輸出目錄。 這表示您可以使用的輸出， `dotnet build` 在任何電腦上執行您的外掛程式。

```xml
<PropertyGroup>
  <CopyLocalLockFileAssemblies>true</CopyLocalLockFileAssemblies>
</PropertyGroup>
```

> [!TIP]
> 或者，您可以使用 `dotnet publish` 來發行類別庫。 如需詳細資訊，請參閱 [dotnet publish](../tools/dotnet-publish.md)。

### <a name="runtimeidentifier"></a>RuntimeIdentifier

`RuntimeIdentifier`屬性可讓您為專案指定[ (RID) ](../rid-catalog.md)的單一執行時間識別碼。 RID 允許發佈獨立式部署。

```xml
<PropertyGroup>
  <RuntimeIdentifier>ubuntu.16.04-x64</RuntimeIdentifier>
</PropertyGroup>
```

### <a name="runtimeidentifiers"></a>RuntimeIdentifiers

`RuntimeIdentifiers`屬性可讓您為專案指定以分號分隔的[執行時間識別碼清單， (rid) ](../rid-catalog.md) 。 如果您需要發行多個執行時間，請使用此屬性。 `RuntimeIdentifiers` 會在還原時使用，以確保圖表中有正確的資產。

> [!TIP]
> `RuntimeIdentifier` (單數) 可以在只需要單一執行時間時提供更快速的組建。

```xml
<PropertyGroup>
  <RuntimeIdentifiers>win10-x64;osx.10.11-x64;ubuntu.16.04-x64</RuntimeIdentifiers>
</PropertyGroup>
```

### <a name="trimmerrootassembly"></a>TrimmerRootAssembly

`TrimmerRootAssembly`專案可讓您從 [*修剪*](../deploying/trim-self-contained.md)中排除元件。 修剪是從封裝的應用程式中移除執行時間未使用部分的進程。 在某些情況下，修剪可能會不正確地移除必要的參考。

下列 XML 會將 `System.Security` 元件從修剪中排除。

```xml
<ItemGroup>
  <TrimmerRootAssembly Include="System.Security" />
</ItemGroup>
```

### <a name="useapphost"></a>UseAppHost

`UseAppHost`屬性控制是否為部署建立原生可執行檔。 獨立部署需要原生可執行檔。

在 .NET Core 3.0 和更新版本中，依預設會建立與 framework 相依的可執行檔。 將 `UseAppHost` 屬性設為， `false` 以停用產生可執行檔。

```xml
<PropertyGroup>
  <UseAppHost>false</UseAppHost>
</PropertyGroup>
```

如需部署的詳細資訊，請參閱 [.net 應用程式部署](../deploying/index.md)。

## <a name="compile-properties"></a>編譯屬性

- [EmbeddedResourceUseDependentUponConvention](#embeddedresourceusedependentuponconvention)
- [LangVersion](#langversion)

### <a name="embeddedresourceusedependentuponconvention"></a>EmbeddedResourceUseDependentUponConvention

`EmbeddedResourceUseDependentUponConvention`屬性定義是否從原始程式檔中的型別資訊產生資源資訊清單檔案名，而這些來源檔案與資源檔共存。 例如，如果 *Form1* 與 *Form1.cs* 位於相同的資料夾中，而且 `EmbeddedResourceUseDependentUponConvention` 設定為 `true` ，則產生的 *.resources* 檔會從 *Form1.cs* 中定義的第一個類型取得其名稱。 例如，如果 `MyNamespace.Form1` 是 *Form1.cs* 中所定義的第一個型別，則產生的檔案名會是 *MyNamespace。*

> [!NOTE]
> 如果 `LogicalName` 為 `ManifestResourceName` `DependentUpon` 專案指定、或中繼資料 `EmbeddedResource` ，則會改為以該中繼資料為基礎，產生該資源檔的資訊清單檔案名。

根據預設，在新的 .NET 專案中，這個屬性會設定為 `true` 。 如果設定為 `false` ，且 `LogicalName` 專案檔中的專案未指定任何、 `ManifestResourceName` 或 `DependentUpon` 中繼資料 `EmbeddedResource` ，則資源資訊清單檔案名會以專案的根命名空間和 .resx 檔案的相對檔案路徑為基礎 *。* 如需詳細資訊，請參閱 [資源資訊清單檔案的命名方式](../resources/manifest-file-names.md)。

```xml
<PropertyGroup>
  <EmbeddedResourceUseDependentUponConvention>true</EmbeddedResourceUseDependentUponConvention>
</PropertyGroup>
```

### <a name="langversion"></a>LangVersion

`LangVersion`屬性可讓您指定特定的程式設計語言版本。 例如，如果您想要存取 c # 預覽功能，請將設定 `LangVersion` 為 `preview` 。

```xml
<PropertyGroup>
  <LangVersion>preview</LangVersion>
</PropertyGroup>
```

如需詳細資訊，請參閱 [c # 語言版本控制](../../csharp/language-reference/configure-language-version.md#override-a-default)。

## <a name="code-analysis-properties"></a>程式碼分析屬性

### <a name="analysislevel"></a>AnalysisLevel

`AnalysisLevel`屬性可讓您指定程式碼分析層級。 例如，如果您想要存取預覽程式碼分析器，請將設定 `AnalysisLevel` 為 `preview` 。 預設值為 `latest`。

```xml
<PropertyGroup>
  <AnalysisLevel>preview</AnalysisLevel>
</PropertyGroup>
```

下表顯示可用的選項。

| 值 | 意義 |
|-|-|
| `latest` | 使用已發行的最新程式碼分析器。 此為預設值。 |
| `preview` | 使用最新的程式碼分析器，即使它們處於預覽狀態也一樣。 |
| `5.0` | 即使有較新的規則，也會使用針對 .NET 5.0 版本啟用的規則集。 |
| `5` | 即使有較新的規則，也會使用針對 .NET 5.0 版本啟用的規則集。 |

### <a name="analysismode"></a>AnalysisMode

從 .NET 5.0 開始，.NET SDK 隨附所有「CA」程式 [代碼品質規則](../../fundamentals/code-analysis/quality-rules/index.md)。 依預設，只 [會啟用部分規則](../../fundamentals/code-analysis/overview.md#enabled-rules) 做為組建警告。 `AnalysisMode`屬性可讓您自訂預設啟用的規則集。 您可以切換至更積極的 (退出) 分析模式或更保守的 (加入) 分析模式。 例如，如果您想要預設啟用所有規則作為組建警告，請將值設定為 `AllEnabledByDefault` 。

```xml
<PropertyGroup>
  <AnalysisMode>AllEnabledByDefault</AnalysisMode>
</PropertyGroup>
```

下表顯示可用的選項。

| 值 | 意義 |
|-|-|
| `Default` | 預設模式，其中某些規則會啟用為組建警告，某些規則會啟用為 Visual Studio IDE 建議，並停用其餘部分。 |
| `AllEnabledByDefault` | 積極或退出模式，預設會啟用所有規則作為組建警告。 您可以選擇性地 [退出宣告](../../fundamentals/code-analysis/configuration-options.md) 個別規則來停用它們。 |
| `AllDisabledByDefault` | 保守或加入宣告模式，預設會停用所有規則。 您可以選擇性地 [選擇](../../fundamentals/code-analysis/configuration-options.md) 加入個別規則來啟用它們。 |

### <a name="codeanalysistreatwarningsaserrors"></a>CodeAnalysisTreatWarningsAsErrors

`CodeAnalysisTreatWarningsAsErrors`屬性可讓您設定是否要將程式碼品質分析警告 (CAxxxx) 應該視為警告並中斷組建。 如果您在 `-warnaserror` 建立專案時使用旗標，則也會將 [.net 程式碼品質分析](../../fundamentals/code-analysis/overview.md#code-quality-analysis) 警告視為錯誤。 如果您不想將程式碼品質分析警告視為錯誤，您可以 `CodeAnalysisTreatWarningsAsErrors` 在專案檔中將 MSBuild 屬性設定為 `false` 。

```xml
<PropertyGroup>
  <CodeAnalysisTreatWarningsAsErrors>false</CodeAnalysisTreatWarningsAsErrors>
</PropertyGroup>
```

### <a name="enablenetanalyzers"></a>EnableNETAnalyzers

針對以 .NET 5.0 或更新版本為目標的專案，預設會啟用[.net 程式碼品質分析](../../fundamentals/code-analysis/overview.md#code-quality-analysis)。 您可以藉由將屬性設定為，為以舊版 .NET 為目標的專案啟用 .NET 程式碼分析 `EnableNETAnalyzers` `true` 。 若要在任何專案中停用程式碼分析，請將這個屬性設定為 `false` 。

```xml
<PropertyGroup>
  <EnableNETAnalyzers>true</EnableNETAnalyzers>
</PropertyGroup>
```

> [!TIP]
> 針對以 .net 5.0 之前的 .NET 版本為目標的專案啟用 .NET 程式碼分析的另一種方式，是將 [AnalysisLevel](#analysislevel) 屬性設定為 `latest` 。

### <a name="enforcecodestyleinbuild"></a>EnforceCodeStyleInBuild

> [!NOTE]
> 這項功能目前為實驗性，而且在 .NET 5 和 .NET 6 版本之間可能會變更。

根據預設， [.net 程式碼樣式分析](../../fundamentals/code-analysis/overview.md#code-style-analysis)會在所有 .net 專案的組建上停用。 您可以藉由將屬性設定為，啟用 .NET 專案的程式碼樣式分析 `EnforceCodeStyleInBuild` `true` 。

```xml
<PropertyGroup>
  <EnforceCodeStyleInBuild>true</EnforceCodeStyleInBuild>
</PropertyGroup>
```

所有 [設定](../../fundamentals/code-analysis/overview.md#code-style-analysis) 為警告或錯誤的程式碼樣式規則，都會在組建和報告違規時執行。

## <a name="run-time-configuration-properties"></a>執行時間設定屬性

您可以在應用程式的專案檔中指定 MSBuild 屬性，以設定一些執行時間行為。 如需設定執行時間行為之其他方式的詳細資訊，請參閱 [執行時間設定](../run-time-config/index.md)。

- [ConcurrentGarbageCollection](#concurrentgarbagecollection)
- [InvariantGlobalization](#invariantglobalization)
- [RetainVMGarbageCollection](#retainvmgarbagecollection)
- [ServerGarbageCollection](#servergarbagecollection)
- [ThreadPoolMaxThreads](#threadpoolmaxthreads)
- [ThreadPoolMinThreads](#threadpoolminthreads)
- [TieredCompilation](#tieredcompilation)
- [TieredCompilationQuickJit](#tieredcompilationquickjit)
- [TieredCompilationQuickJitForLoops](#tieredcompilationquickjitforloops)

### <a name="concurrentgarbagecollection"></a>ConcurrentGarbageCollection

`ConcurrentGarbageCollection`屬性會設定是否啟用[背景 (並行) 垃圾收集](../../standard/garbage-collection/background-gc.md)。 將值設定為 `false` ，以停用背景垃圾收集。 如需詳細資訊，請參閱 [背景 GC](../run-time-config/garbage-collector.md#background-gc)。

```xml
<PropertyGroup>
  <ConcurrentGarbageCollection>false</ConcurrentGarbageCollection>
</PropertyGroup>
```

### <a name="invariantglobalization"></a>InvariantGlobalization

`InvariantGlobalization`屬性會設定應用程式是否以 *全球化不變的* 模式執行，這表示它無法存取特定文化特性的資料。 將值設定為 `true` ，以在全球化非變異模式中執行。 如需詳細資訊，請參閱不 [變模式](../run-time-config/globalization.md#invariant-mode)。

```xml
<PropertyGroup>
  <InvariantGlobalization>true</InvariantGlobalization>
</PropertyGroup>
```

### <a name="retainvmgarbagecollection"></a>RetainVMGarbageCollection

屬性會將 `RetainVMGarbageCollection` 垃圾收集行程設定為將已刪除的記憶體區段放置於待命清單上，以供日後使用或釋放它們。 將此值設定為可 `true` 告知垃圾收集行程將區段放在待命清單上。 如需詳細資訊，請參閱 [保留 VM](../run-time-config/garbage-collector.md#retain-vm)。

```xml
<PropertyGroup>
  <RetainVMGarbageCollection>true</RetainVMGarbageCollection>
</PropertyGroup>
```

### <a name="servergarbagecollection"></a>ServerGarbageCollection

屬性會設定 `ServerGarbageCollection` 應用程式是使用 [工作站垃圾收集或伺服器垃圾收集](../../standard/garbage-collection/workstation-server-gc.md)。 將值設定為，以 `true` 使用伺服器垃圾收集。 如需詳細資訊，請參閱 [工作站與伺服器](../run-time-config/garbage-collector.md#workstation-vs-server)。

```xml
<PropertyGroup>
  <ServerGarbageCollection>true</ServerGarbageCollection>
</PropertyGroup>
```

### <a name="threadpoolmaxthreads"></a>ThreadPoolMaxThreads

屬性會設定背景 `ThreadPoolMaxThreads` 工作執行緒集區的執行緒數目上限。 如需詳細資訊，請參閱 [最大執行緒數目](../run-time-config/threading.md#maximum-threads)。

```xml
<PropertyGroup>
  <ThreadPoolMaxThreads>20</ThreadPoolMaxThreads>
</PropertyGroup>
```

### <a name="threadpoolminthreads"></a>ThreadPoolMinThreads

`ThreadPoolMinThreads`屬性會為背景工作執行緒集區設定執行緒的最小數目。 如需詳細資訊，請參閱 [最小線程](../run-time-config/threading.md#minimum-threads)。

```xml
<PropertyGroup>
  <ThreadPoolMinThreads>4</ThreadPoolMinThreads>
</PropertyGroup>
```

### <a name="tieredcompilation"></a>TieredCompilation

屬性會設定 `TieredCompilation` 及時 (JIT) 編譯器是否使用 [分層式編譯](../whats-new/dotnet-core-3-0.md#tiered-compilation)。 將值設定為 `false` ，以停用階層式編譯。 如需詳細資訊，請參閱 [分層式編譯](../run-time-config/compilation.md#tiered-compilation)。

```xml
<PropertyGroup>
  <TieredCompilation>false</TieredCompilation>
</PropertyGroup>
```

### <a name="tieredcompilationquickjit"></a>TieredCompilationQuickJit

屬性會設定 `TieredCompilationQuickJit` jit 編譯器是否使用快速 jit。 將值設定為 `false` ，以停用快速 JIT。 如需詳細資訊，請參閱 [快速 JIT](../run-time-config/compilation.md#quick-jit)。

```xml
<PropertyGroup>
  <TieredCompilationQuickJit>false</TieredCompilationQuickJit>
</PropertyGroup>
```

### <a name="tieredcompilationquickjitforloops"></a>TieredCompilationQuickJitForLoops

`TieredCompilationQuickJitForLoops`屬性會設定 jit 編譯器是否在包含迴圈的方法上使用快速 jit。 將值設定為， `true` 以在包含迴圈的方法上啟用快速 JIT。 如需詳細資訊，請參閱 [快速 JIT for 迴圈](../run-time-config/compilation.md#quick-jit-for-loops)。

```xml
<PropertyGroup>
  <TieredCompilationQuickJitForLoops>true</TieredCompilationQuickJitForLoops>
</PropertyGroup>
```

## <a name="reference-properties-and-items"></a>參考屬性和專案

- [AssetTargetFallback](#assettargetfallback)
- [PackageReference](#packagereference)
- [專案參考](#projectreference)
- [參考](#reference)
- [還原相關的屬性](#restore-related-properties)

### <a name="assettargetfallback"></a>AssetTargetFallback

`AssetTargetFallback`屬性可讓您為專案參考和 NuGet 套件指定其他相容的架構版本。 例如，如果您使用指定套件相依性， `PackageReference` 但該套件未包含與您的專案相容的資產 `TargetFramework` ，則此 `AssetTargetFallback` 屬性會進入 play。 參考封裝的相容性是使用中所指定的每一個目標架構來間隔 `AssetTargetFallback` 。

您可以將 `AssetTargetFallback` 屬性設定為一或多個 [目標 framework 版本](../../standard/frameworks.md#supported-target-frameworks)。

```xml
<PropertyGroup>
  <AssetTargetFallback>net461</AssetTargetFallback>
</PropertyGroup>
```

### <a name="packagereference"></a>PackageReference

`PackageReference`專案會定義 NuGet 套件的參考。

`Include` 屬性會指定套件識別碼。 `Version`屬性會指定版本或版本範圍。 如需有關如何指定最小版本、最大版本、範圍或完全相符的詳細資訊，請參閱 [版本範圍](/nuget/concepts/package-versioning#version-ranges)。 您也可以將下列中繼資料加入至專案參考： `IncludeAssets` 、 `ExcludeAssets` 和 `PrivateAssets` 。

下列範例中的專案檔程式碼片段會參考 [system.object](https://www.nuget.org/packages/System.Runtime/) 套件。

```xml
<ItemGroup>
  <PackageReference Include="System.Runtime" Version="4.3.0" />
</ItemGroup>
```

如需詳細資訊，請參閱 [專案檔中的套件參考](/nuget/consume-packages/package-references-in-project-files)。

### <a name="projectreference"></a>專案參考

`ProjectReference`專案會定義另一個專案的參考。 參考的專案會新增為 NuGet 套件相依性，亦即，它會被視為與相同 `PackageReference` 。

`Include`屬性會指定專案的路徑。 您也可以將下列中繼資料加入至專案參考： `IncludeAssets` 、 `ExcludeAssets` 和 `PrivateAssets` 。

下列範例中的專案檔程式碼片段會參考名為的專案 `Project2` 。

```xml
<ItemGroup>
  <ProjectReference Include="..\Project2.csproj" />
</ItemGroup>
```

### <a name="reference"></a>參考

`Reference`專案會定義元件檔的參考。

`Include`屬性會指定檔案名，而 `HintPath` 中繼資料會指定元件的路徑。

```xml
<ItemGroup>
  <Reference Include="MyAssembly">
    <HintPath>..\..\Assemblies\MyAssembly.dll</HintPath>
  </Reference>
</ItemGroup>
```

### <a name="restore-related-properties"></a>還原相關的屬性

還原參考的封裝會安裝其所有直接相依性，以及這些相依性的所有相依性。 您可以藉由指定屬性（例如和）來自訂封裝還原 `RestorePackagesPath` `RestoreIgnoreFailedSources` 。 如需這些屬性和其他屬性的詳細資訊，請參閱 [還原目標](/nuget/reference/msbuild-targets#restore-target)。

```xml
<PropertyGroup>
  <RestoreIgnoreFailedSource>true</RestoreIgnoreFailedSource>
</PropertyGroup>
```

## <a name="hosting-properties-and-items"></a>裝載屬性和專案

- [EnableComHosting](#enablecomhosting)
- [EnableDynamicLoading](#enabledynamicloading)

### <a name="enablecomhosting"></a>EnableComHosting

`EnableComHosting`屬性工作表示元件提供 COM 伺服器。 將設定 `EnableComHosting` 為 `true` 也表示 [EnableDynamicLoading](#enabledynamicloading) 為 `true` 。

```xml
<PropertyGroup>
  <EnableComHosting>True</EnableComHosting>
</PropertyGroup>
```

如需詳細資訊，請參閱 [將 .net 元件公開至 COM](../native-interop/expose-components-to-com.md)。

### <a name="enabledynamicloading"></a>EnableDynamicLoading

`EnableDynamicLoading`屬性工作表示元件是動態載入的元件。 元件可以是 COM 連結 [庫](/windows/win32/com/the-component-object-model) 或可 [從原生主機使用](../tutorials/netcore-hosting.md)的非 com 程式庫。 將這個屬性設定為 `true` 具有下列效果：

- 會產生檔案 *上的.runtimeconfig.js* 。
- [向前](../whats-new/dotnet-core-3-0.md#major-version-runtime-roll-forward) 復原是設定為 `LatestMinor` 。
- NuGet 參考會在本機複製。

```xml
<PropertyGroup>
  <EnableDynamicLoading>true</EnableDynamicLoading>
</PropertyGroup>
```

## <a name="see-also"></a>另請參閱

- [MSBuild 架構參考](/visualstudio/msbuild/msbuild-project-file-schema-reference)
- [一般 MSBuild 屬性](/visualstudio/msbuild/common-msbuild-project-properties)
- [NuGet 套件的 MSBuild 屬性](/nuget/reference/msbuild-targets#pack-target)
- [NuGet 還原的 MSBuild 屬性](/nuget/reference/msbuild-targets#restore-properties)
- [自訂群組建](/visualstudio/msbuild/customize-your-build)
