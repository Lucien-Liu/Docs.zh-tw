---
title: 重大變更：預設 ActivityIdFormat 是 W3C
description: 瞭解在預設 ActivityIdFormat 為 W3C 的核心 .NET 程式庫中，.NET 5.0 的重大變更。
ms.date: 11/01/2020
ms.openlocfilehash: 77ee705ac18065c84ddeab3127e01b6a40c3b84d
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/24/2020
ms.locfileid: "95760722"
---
# <a name="default-activityidformat-is-w3c"></a>預設 ActivityIdFormat 是 W3C

活動 () 的預設識別碼格式 <xref:System.Diagnostics.Activity.DefaultIdFormat?displayProperty=nameWithType> 現在是 <xref:System.Diagnostics.ActivityIdFormat.W3C?displayProperty=nameWithType> 。

## <a name="change-description"></a>變更描述

W3C 活動識別碼格式是在 .NET Core 3.0 中引進，作為階層式識別碼格式的替代方案。 不過，為了保留相容性，W3C 格式不是預設值，除非是 .NET 5.0。 .NET 5.0 中的預設值已變更，因為 [W3C 格式已經過](https://www.w3.org/TR/trace-context/) 核准，並且在多個語言執行方面獲得了吸引。

如果您的應用程式是以 .NET 5.0 或更新版本以外的平臺為目標，則會遇到舊的行為，其中 <xref:System.Diagnostics.ActivityIdFormat.Hierarchical> 是預設格式。 此預設值適用于平臺 net45 +、netstandard 1.1 + 和 netcoreapp (1.x、2.x 和 3.x) 。 在 .NET 5.0 和更新版本中， <xref:System.Diagnostics.Activity.DefaultIdFormat?displayProperty=nameWithType> 會設定為 <xref:System.Diagnostics.ActivityIdFormat.W3C?displayProperty=nameWithType> 。

## <a name="version-introduced"></a>引進的版本

5.0

## <a name="recommended-action"></a>建議的動作

如果您的應用程式與用於分散式追蹤的識別碼無關，就不需要採取任何動作。 程式庫（例如 ASP.NET Core） <xref:System.Net.Http.HttpClient> 可以取用或傳播的兩個版本 <xref:System.Diagnostics.ActivityIdFormat> 。

如果您需要與現有系統之間的互通性，或是目前的系統依賴識別碼的格式，您可以將設定為，以保留舊的行為 <xref:System.Diagnostics.Activity.DefaultIdFormat> <xref:System.Diagnostics.ActivityIdFormat.Hierarchical?displayProperty=nameWithType> 。 或者，您可以使用下列三種方式之一來設定 AppCoNtext 交換器：

- 在專案檔中。

  ```xml
  <ItemGroup>
    <RuntimeHostConfigurationOption Include="System.Diagnostics.DefaultActivityIdFormatIsHierarchial" Value="true" />
  </ItemGroup>
  ```

- 在 *runtimeconfig.json* 檔案。

  ```json
  {
      "runtimeOptions": {
          "configProperties": {
              "System.Diagnostics.DefaultActivityIdFormatIsHierarchial": true
          }
      }
  }
  ```

- 透過環境變數。

  設定 `DOTNET_SYSTEM_DIAGNOSTICS_DEFAULTACTIVITYIDFORMATISHIERARCHIAL` 為 `true` 或1。

## <a name="affected-apis"></a>受影響的 API

- <xref:System.Diagnostics.Activity.DefaultIdFormat?displayProperty=fullName>

<!--

### Category

Core .NET libraries

### Affected APIs

- `P:System.Diagnostics.Activity.DefaultIdFormat`

-->
