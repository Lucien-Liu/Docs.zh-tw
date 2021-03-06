---
title: 只能從不含引數的簡單或限定名稱來推斷範圍變數名稱
ms.date: 07/20/2015
f1_keywords:
- vbc36599
- bc36599
helpviewer_keywords:
- BC36599
ms.assetid: 17763dbe-f74f-4ccb-8086-cb7e45ec4d12
ms.openlocfilehash: 63990b7d37388057ff2cdb430d29878a1c7b39ba
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2020
ms.locfileid: "92162358"
---
# <a name="bc36599-range-variable-name-can-be-inferred-only-from-a-simple-or-qualified-name-with-no-arguments"></a>BC36599：只能從簡單或限定名稱（沒有引數）推斷範圍變數名稱

LINQ 查詢中包含接受一或多個引數的程式設計項目。 編譯器無法從該程式設計項目推斷範圍變數。

**錯誤識別碼：** BC36599

## <a name="to-correct-this-error"></a>更正這個錯誤

提供程式設計項目的明確變數名稱，如下列程式碼所示：

```vb
Dim query = From var1 In collection1
            Select VariableAlias= SampleFunction(var1), var1
```

## <a name="see-also"></a>另請參閱

- [Visual Basic 中的 LINQ 簡介](../../programming-guide/language-features/linq/introduction-to-linq.md)
- [Select 子句](../queries/select-clause.md)
