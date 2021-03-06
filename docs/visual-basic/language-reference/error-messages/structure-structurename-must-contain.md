---
title: 結構 '<structurename>' 至少必須包含一個執行個體成員變數，或者至少要有一個執行個體事件宣告未標記為 'Custom'
ms.date: 07/20/2015
f1_keywords:
- bc30941
- vbc30941
helpviewer_keywords:
- BC30941
ms.assetid: 7054cc1e-bac3-4c3d-82f3-35772bd8dd3b
ms.openlocfilehash: 4e7ef82659c43be08ee444eaf3f4df663f7aaa53
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2020
ms.locfileid: "92159647"
---
# <a name="bc30941-structure-structurename-must-contain-at-least-one-instance-member-variable-or-at-least-one-instance-event-declaration-not-marked-custom"></a>BC30941：結構 ' \<structurename> ' 必須至少包含一個實例成員變數或至少一個實例事件宣告未標記為 ' Custom '

結構定義不包含任何非共用變數或非共用的 noncustom 事件。

 每個結構都必須有變數或套用至每個特定實例的事件 (非共用的) ，而不是 ([共用](../modifiers/shared.md)) 共同的所有實例。 非共用的常數、屬性和程式不符合這項需求。 此外，如果沒有非共用的變數，而且只有一個非共用的事件，該事件就不能是 `Custom` 事件。

 **錯誤識別碼：** BC30941

## <a name="to-correct-this-error"></a>更正這個錯誤

- 至少定義一個不是的變數或事件 `Shared` 。 如果您只定義一個事件，它必須是 noncustom 以及非共用的。

## <a name="see-also"></a>另請參閱

- [結構](../../programming-guide/language-features/data-types/structures.md)
- [作法：宣告結構](../../programming-guide/language-features/data-types/how-to-declare-a-structure.md)
- [Structure 陳述式](../statements/structure-statement.md)
