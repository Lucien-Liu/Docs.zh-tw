---
title: dotnet nuget locals 命令
description: dotnet nuget locals 命令會清除或列出本機 NuGet 資源，例如 http-request 快取、暫時快取，或整部電腦的全域套件資料夾。
author: karann-msft
ms.date: 02/14/2020
ms.openlocfilehash: 5b421b5058528a93c7be58eef2932937cc9cc12d
ms.sourcegitcommit: 927b7ea6b2ea5a440c8f23e3e66503152eb85591
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/16/2020
ms.locfileid: "81463537"
---
# <a name="dotnet-nuget-locals"></a>dotnet nuget locals

**本文適用於:✔️** .NET Core 2.x SDK 和更高版本

## <a name="name"></a>名稱

`dotnet nuget locals` - 清除或列出本機 NuGet 資源。

## <a name="synopsis"></a>概要

```dotnetcli
dotnet nuget locals <CACHE_LOCATION> [(-c|--clear)|(-l|--list)] [--force-english-output]

dotnet nuget locals -h|--help
```

## <a name="description"></a>描述

`dotnet nuget locals` 命令會清除或列出 http-request 快取、暫時快取，或整部電腦全域套件資料夾中的本機 NuGet 資源。

## <a name="arguments"></a>引數

- **`CACHE_LOCATION`**

  要列出或清除的快取位置。 它接受下列其中一個值：

  * `all` - 指出指定的作業會套用至所有快取類型：http-request 快取、全域套件快取和暫時快取。
  * `http-cache` - 指出指定的作業只套用至 http-request 快取。 其他快取位置不受影響。
  * `global-packages` - 指出指定的作業只套用至全域套件快取。 其他快取位置不受影響。
  * `temp` - 指出指定的作業只套用至暫時快取。 其他快取位置不受影響。

## <a name="options"></a>選項。

- **`--force-english-output`**

  強制使用非變異英文文化特性來執行應用程式。

- **`-h|--help`**

  印出命令的簡短說明。

- **`-c|--clear`**

  清除選項會針對指定的快取類型執行清除作業。 系統會以遞迴方式刪除快取目錄的內容。 執行的使用者/群組必須具有快取目錄中檔案的權限。 如果沒有權限，則會顯示錯誤，指出尚未清除的檔案/資料夾。

- **`-l|--list`**

  list 選項是用來顯示指定之快取類型的位置。

## <a name="examples"></a>範例

- 顯示所有本機快取目錄的路徑 (http-cache 目錄、global-packages 快取目錄及暫時快取目錄)：

  ```dotnetcli
  dotnet nuget locals all –l
  ```

- 顯示本機 http-cache 目錄的路徑：

  ```dotnetcli
  dotnet nuget locals http-cache --list
  ```

- 清除所有本機快取目錄的所有檔案 (http-cache 目錄、global-packages 快取目錄及暫時快取目錄)：

  ```dotnetcli
  dotnet nuget locals all --clear
  ```

- 清除本機 global-packages 快取目錄中的所有檔案：

  ```dotnetcli
  dotnet nuget locals global-packages -c
  ```

- 清除本機暫時快取目錄中的所有檔案：

  ```dotnetcli
  dotnet nuget locals temp -c
  ```

## <a name="troubleshooting"></a>疑難排解

如需使用 `dotnet nuget locals` 命令時常見的問題與錯誤資訊，請參閱[管理 NuGet 快取](/nuget/consume-packages/managing-the-nuget-cache)。
