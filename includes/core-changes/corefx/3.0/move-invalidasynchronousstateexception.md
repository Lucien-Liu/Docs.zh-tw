---
ms.openlocfilehash: 78678b4b352bb063d1521e9aee3492c0cee059b8
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/25/2020
ms.locfileid: "96032025"
---
### <a name="invalidasynchronousstateexception-moved-to-another-assembly"></a>System.componentmodel.invalidasynchronousstateexception 移至另一個元件

已 <xref:System.ComponentModel.InvalidAsynchronousStateException> 移動類別。

#### <a name="change-description"></a>變更描述

在 .NET Core 2.2 和較早的版本中， <xref:System.ComponentModel.InvalidAsynchronousStateException> 會在 *ComponentModel* 元件中找到類別。

從 .NET Core 3.0 開始，它會在 *ComponentModel* 元件中找到。

#### <a name="version-introduced"></a>引進的版本

3.0

#### <a name="recommended-action"></a>建議的動作

這項變更只會影響使用反映來載入的應用程式， <xref:System.ComponentModel.InvalidAsynchronousStateException> 方法是呼叫方法（例如）或的多載（ <xref:System.Reflection.Assembly.GetType%2A?displayProperty=nameWithType> <xref:System.Activator.CreateInstance%2A?displayProperty=nameWithType> 其會假設該型別在特定元件中）。 如果是這種情況，請更新方法呼叫中參考的元件，以反映類型的新元件位置。

#### <a name="category"></a>類別

Core .NET 程式庫

#### <a name="affected-apis"></a>受影響的 API

無。

<!--

#### Affected APIs

- Not detectable via API analysis

-->
