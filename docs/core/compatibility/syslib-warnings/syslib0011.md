---
title: SYSLIB0011 警告
description: 瞭解產生編譯時期警告 SYSLIB0011 的 obsoletions。
ms.topic: reference
ms.date: 10/20/2020
ms.openlocfilehash: 36292cc5314e2b7677d705780880b7e25ae0dfb6
ms.sourcegitcommit: e301979e3049ce412d19b094c60ed95b316a8f8c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/16/2020
ms.locfileid: "97596473"
---
# <a name="syslib0011-binaryformatter-serialization-is-obsolete"></a>SYSLIB0011： BinaryFormatter 序列化已淘汰

由於中的 [安全性弱點](../../../standard/serialization/binaryformatter-security-guide.md#binaryformatter-security-vulnerabilities) <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter> ，下列 api 會標示為已淘汰，從 .net 5.0 開始。 在程式碼中使用它們會 `SYSLIB0011` 在編譯時期產生警告。

- <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter.Serialize%2A?displayProperty=nameWithType>
- <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter.Deserialize%2A?displayProperty=nameWithType>
- <xref:System.Runtime.Serialization.Formatter.Serialize(System.IO.Stream,System.Object)?displayProperty=nameWithType>
- <xref:System.Runtime.Serialization.Formatter.Deserialize(System.IO.Stream)?displayProperty=nameWithType>
- <xref:System.Runtime.Serialization.IFormatter.Serialize(System.IO.Stream,System.Object)?displayProperty=nameWithType>
- <xref:System.Runtime.Serialization.IFormatter.Deserialize(System.IO.Stream)?displayProperty=nameWithType>

## <a name="workarounds"></a>因應措施

請考慮使用 <xref:System.Text.Json.JsonSerializer> 或， <xref:System.Xml.Serialization.XmlSerializer> 而不是 <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter> 。

如需建議動作的詳細資訊，請參閱 [解決 BinaryFormatter obsoletion 和停用錯誤](https://aka.ms/binaryformatter)。

[!INCLUDE [suppress-syslib-warning](../../../../includes/suppress-syslib-warning.md)]

## <a name="see-also"></a>另請參閱

- [解決 BinaryFormatter obsoletion 和停用錯誤](https://aka.ms/binaryformatter)
- [ASP.NET apps 已淘汰且禁止 BinaryFormatter 序列化方法](../core-libraries/5.0/binaryformatter-serialization-obsolete.md)
