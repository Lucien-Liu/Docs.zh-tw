---
title: 如何：變更程序引數的值
ms.date: 07/20/2015
helpviewer_keywords:
- procedures [Visual Basic], arguments
- procedures [Visual Basic], parameters
- procedure arguments
- arguments [Visual Basic], passing by reference
- Visual Basic code, procedures
- arguments [Visual Basic], ByVal
- arguments [Visual Basic], passing by value
- procedure parameters
- arguments [Visual Basic], ByRef
- arguments [Visual Basic], changing value
ms.assetid: 6fad2368-5da7-4c07-8bf8-0f4e65a1be67
ms.openlocfilehash: 9960dacc053c5dc8d8cfdfd6eaa0ea3258ed0fea
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/23/2020
ms.locfileid: "91077068"
---
# <a name="how-to-change-the-value-of-a-procedure-argument-visual-basic"></a>如何：變更程序引數的值 (Visual Basic)

當您呼叫程式時，您所提供的每個引數都會對應到程式中定義的其中一個參數。 在某些情況下，程式程式碼可以變更呼叫程式碼中引數的基礎值。 在其他情況下，程式只能變更其引數的本機複本。  
  
 當您呼叫程式時，Visual Basic 會為每個傳遞了 [ByVal](../../../language-reference/modifiers/byval.md)的引數建立本機複本。 針對每個傳遞 [ByRef](../../../language-reference/modifiers/byref.md)的引數，Visual Basic 可讓程式碼直接參考呼叫程式碼中引數的基礎程式設計項目。  
  
 如果呼叫程式碼中的基礎專案是可修改的專案，而且傳遞引數 `ByRef` ，程式碼可以使用直接參考來變更呼叫程式碼中的元素值。  
  
## <a name="changing-the-underlying-value"></a>變更基礎值  
  
#### <a name="to-change-the-underlying-value-of-a-procedure-argument-in-the-calling-code"></a>變更呼叫程式碼中程式引數的基礎值  
  
1. 在程式聲明中，針對與引數對應的參數指定 [ByRef](../../../language-reference/modifiers/byref.md) 。  
  
2. 在呼叫程式碼中，傳遞可修改的程式設計項目作為引數。  
  
3. 在呼叫程式碼中，請勿將引數括在引數清單中的括弧內。  
  
4. 在程式程式碼中，使用參數名稱，將值指派給呼叫程式碼中的基礎元素。  
  
 如需示範，請參閱下方的範例。  
  
## <a name="changing-local-copies"></a>變更本機複本  

 如果呼叫程式碼中的基礎專案是不可修改的元素，或如果傳遞引數 `ByVal` ，程式就無法在呼叫程式碼中變更其值。 不過，程式可以變更其這類引數的本機複本。  
  
#### <a name="to-change-the-copy-of-a-procedure-argument-in-the-procedure-code"></a>在程式碼中變更程式引數的複本  
  
1. 在程式宣告中，針對與引數對應的參數指定 [ByVal](../../../language-reference/modifiers/byval.md) 。  
  
     -或-  
  
     在呼叫程式碼中，將引數括在引數清單中的括弧內。 這會強制 Visual Basic 以傳值方式傳遞引數，即使對應的參數指定 `ByRef` 。  
  
2. 在程式程式碼中，使用參數名稱將值指派給引數的本機複本。 呼叫程式碼中的基礎值不會變更。  
  
## <a name="example"></a>範例  

 下列範例顯示兩個採用陣列變數並對其元素操作的程式。 此程式 `increase` 只會將一個加入至每個元素。 程式會將 `replace` 新的陣列指派給參數 `a()` ，然後將它加入至每個專案。  
  
 [!code-vb[VbVbcnProcedures#35](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#35)]  
  
 [!code-vb[VbVbcnProcedures#36](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#36)]  
  
 [!code-vb[VbVbcnProcedures#37](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#37)]  
  
 第一個 `MsgBox` 呼叫會顯示「增加之後 (n) ：11，21，31，41」。 因為陣列 `n` 是參考型別，所以 `replace` 可以變更其成員，即使傳遞的機制是也一樣 `ByVal` 。  
  
 第二個 `MsgBox` 呼叫會顯示「Replace 之後 (n) ：101、201、301」。 因為 `n` 已傳遞 `ByRef` ，所以 `replace` 可以修改呼叫程式碼中的變數， `n` 並將新的陣列指派給它。 因為 `n` 是參考型別，所以 `replace` 也可以變更其成員。  
  
 您可以防止程式在呼叫程式碼中修改變數本身。 請參閱 [如何：根據值變更保護程式引數](./how-to-protect-a-procedure-argument-against-value-changes.md)。  
  
## <a name="compile-the-code"></a>編譯程式碼  

 當您以傳址方式傳遞變數時，您必須使用 `ByRef` 關鍵字來指定此機制。  
  
 Visual Basic 中的預設值是以傳值方式傳遞引數。 不過，最好的程式設計作法是在每個宣告的參數中包含 [ByVal](../../../language-reference/modifiers/byval.md) 或 [ByRef](../../../language-reference/modifiers/byref.md) 關鍵字。 這可讓您的程式碼更容易閱讀。  
  
## <a name="net-framework-security"></a>.NET Framework 安全性  

 在允許程式變更呼叫程式碼中引數的基礎值時，一律會有潛在的風險。 請確定您預期會變更此值，並在使用它之前準備好檢查其有效性。  
  
## <a name="see-also"></a>另請參閱

- [程序](./index.md)
- [程序參數和引數](./procedure-parameters-and-arguments.md)
- [如何：將引數傳遞至程序](./how-to-pass-arguments-to-a-procedure.md)
- [以傳值和傳址方式傳遞引數](./passing-arguments-by-value-and-by-reference.md)
- [可修改引數和不可修改引數之間的差異](./differences-between-modifiable-and-nonmodifiable-arguments.md)
- [以傳值或傳址方式傳遞引數的差別](./differences-between-passing-an-argument-by-value-and-by-reference.md)
- [如何：防止程序引數的值變更](./how-to-protect-a-procedure-argument-against-value-changes.md)
- [如何：強制以傳值方式傳遞引數](./how-to-force-an-argument-to-be-passed-by-value.md)
- [依位置和名稱傳遞引數](./passing-arguments-by-position-and-by-name.md)
- [Value Types and Reference Types](../data-types/value-types-and-reference-types.md)
