---
title: .NET 程式庫的重大變更
description: 列出 .NET Core 1.0-3.0 版核心 .NET 程式庫中的重大變更。
ms.date: 07/27/2020
ms.openlocfilehash: 092ff36a5e07c9e226fe2a67d5e7cfd391e9d16b
ms.sourcegitcommit: fcbe432482464b1639decad78cc4dc8387c6269e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2020
ms.locfileid: "97366874"
---
# <a name="core-net-libraries-breaking-changes-in-net-core-10-30"></a>.NET Core 1.0-3.0 中的核心 .NET 程式庫重大變更

核心 .NET 程式庫提供 .NET Core 所使用的基本類型和其他一般類型。

此頁面記載了下列重大變更：

| 重大變更 | 引進的版本 |
| - | :-: |
| [將 GroupCollection 傳遞至採用 IEnumerable 的擴充方法需要去除混淆 \<T>](#passing-groupcollection-to-extension-methods-taking-ienumerablet-requires-disambiguation) | 3.0 |
| [報表版本現在會回報產品而非檔案版本的 Api](#apis-that-report-version-now-report-product-and-not-file-version) | 3.0 |
| [自訂 >encoderfallbackbuffer 實例無法遞迴地切換](#custom-encoderfallbackbuffer-instances-cannot-fall-back-recursively) | 3.0 |
| [浮點數格式化和剖析行為變更](#floating-point-formatting-and-parsing-behavior-changed) | 3.0 |
| [浮點數剖析作業不再失敗或擲回 >overflowexception](#floating-point-parsing-operations-no-longer-fail-or-throw-an-overflowexception) | 3.0 |
| [System.componentmodel.invalidasynchronousstateexception 移至另一個元件](#invalidasynchronousstateexception-moved-to-another-assembly) | 3.0 |
| [取代格式不正確的 UTF-8 位元組序列遵循 Unicode 指導方針](#replacing-ill-formed-utf-8-byte-sequences-follows-unicode-guidelines) | 3.0 |
| [TypeDescriptionProviderAttribute 移至另一個元件](#typedescriptionproviderattribute-moved-to-another-assembly) | 3.0 |
| [ZipArchiveEntry 不再以不一致的專案大小處理封存](#ziparchiveentry-no-longer-handles-archives-with-inconsistent-entry-sizes) | 3.0 |
| [FieldInfo 會針對靜態、僅限初始欄位擲回例外狀況](#fieldinfosetvalue-throws-exception-for-static-init-only-fields) | 3.0 |
| [加入至內建結構類型的私用欄位](#private-fields-added-to-built-in-struct-types) | 2.1 |
| [UseShellExecute 預設值的變更](#change-in-default-value-of-useshellexecute) | 2.1 |
| [MacOS 上的 OpenSSL 版本](#openssl-versions-on-macos) | 2.1 |
| [FileSystemInfo 擲回的 System.unauthorizedaccessexception。屬性](#unauthorizedaccessexception-thrown-by-filesysteminfoattributes) | 1.0 |
| [不支援處理損毀的進程狀態例外狀況](#handling-corrupted-state-exceptions-is-not-supported) | 1.0 |
| [UriBuilder 屬性不再加上前置字元](#uribuilder-properties-no-longer-prepend-leading-characters) | 1.0 |
| [StartInfo 會針對您未啟動的進程擲回 InvalidOperationException](#processstartinfo-throws-invalidoperationexception-for-processes-you-didnt-start) | 1.0 |

## <a name="net-core-30"></a>.NET Core 3.0

[!INCLUDE [disambiguate-generic-type-for-groupcollection](../../../includes/core-changes/corefx/3.0/disambiguate-generic-type-for-groupcollection.md)]

***

[!INCLUDE[APIs that report version now report product and not file version](~/includes/core-changes/corefx/3.0/version-information-changes.md)]

**_

[!INCLUDE[Custom EncoderFallbackBuffer instances cannot fall back recursively](~/includes/core-changes/corefx/3.0/custom-encoderfallbackbuffer-cannot-be-recursive.md)]

_*_

[!INCLUDE[Floating point formatting and parsing behavior changes](~/includes/core-changes/corefx/3.0/floating-point-changes.md)]

_*_

[!INCLUDE[Floating-point parsing operations no longer fail or throw an OverflowException](~/includes/core-changes/corefx/3.0/floating-point-parsing-does-not-overflow.md)]

_*_

[!INCLUDE[InvalidAsynchronousStateException moved to another assembly](~/includes/core-changes/corefx/3.0/move-invalidasynchronousstateexception.md)]

_*_

[!INCLUDE[NET Core 3.0 follows Unicode best practices when replacing ill-formed UTF-8 byte sequences](~/includes/core-changes/corefx/3.0/net-core-3-0-follows-unicode-utf8-best-practices.md)]

_*_

[!INCLUDE[TypeDescriptionProviderAttribute moved to another assembly](~/includes/core-changes/corefx/3.0/move-typedescriptionproviderattribute.md)]

_*_

[!INCLUDE[ZipArchiveEntry no longer handles archives with inconsistent entry sizes](~/includes/core-changes/corefx/3.0/ziparchiveentry-and-inconsistent-entry-sizes.md)]

_*_

[!INCLUDE [FieldInfo.SetValue throws exception for static, init-only fields](~/includes/core-changes/corefx/3.0/fieldinfo-setvalue-exception.md)]

_*_

## <a name="net-core-21"></a>.NET Core 2.1

[!INCLUDE[Private fields added to built-in struct types](~/includes/core-changes/corefx/2.1/instantiate-struct.md)]

_*_

[!INCLUDE[Change in default value of UseShellExecute](~/includes/core-changes/corefx/2.1/process-start-changes.md)]

_*_

[!INCLUDE [OpenSSL versions on macOS](../../../includes/core-changes/corefx/openssl-dependencies-macos.md)]

_*_

## <a name="net-core-10"></a>.NET Core 1.0

[!INCLUDE [UnauthorizedAccessException thrown by FileSystemInfo.Attributes](~/includes/core-changes/corefx/1.0/filesysteminfo-attributes-exceptions.md)]

_*_

[!INCLUDE [corrupted-state-exceptions](~/includes/core-changes/corefx/1.0/corrupted-state-exceptions.md)]

_*_

[!INCLUDE [uribuilder-behavior-changes](../../../includes/core-changes/corefx/1.0/uribuilder-behavior-changes.md)]

_*_

[!INCLUDE [startinfo-throws-exception](../../../includes/core-changes/corefx/1.0/startinfo-throws-exception.md)]

_**
