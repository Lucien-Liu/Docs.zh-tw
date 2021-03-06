---
title: 如何：定義運算子
ms.date: 07/20/2015
helpviewer_keywords:
- procedures [Visual Basic], defining
- Visual Basic code, procedures
- operators [Visual Basic], defining
- procedures [Visual Basic], operator
- Visual Basic code, operators
- syntax [Visual Basic], Operator procedures
- operators [Visual Basic], overloading
- operator procedures [Visual Basic], about operator procedures
- return values [Visual Basic], Operator procedures
- operator overloading
ms.assetid: d4b0e253-092a-4e6e-9fe2-01f562140a29
ms.openlocfilehash: 5acbd0439ddbb956b80d56e23d11cd5e152f37ff
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/23/2020
ms.locfileid: "91087397"
---
# <a name="how-to-define-an-operator-visual-basic"></a>如何：定義運算子 (Visual Basic)

如果您已定義類別或結構，則可以定義標準運算子的行為 (例如 `*` 、 `<>` 或 `And`) 當其中一個或兩個運算元屬於類別或結構的型別時。  
  
 將標準運算子定義為類別或結構內的運算子程式。 所有運算子程式都必須是 `Public` `Shared` 。  
  
 在類別或結構上定義運算子 *也稱為多* 載運算子。  
  
## <a name="example"></a>範例  

 下列範例會定義 `+` 名為之結構的運算子 `height` 。 結構會使用以英尺和英寸測量的高度。 1 *英寸* 是2.54，而一 *英尺* 是12英寸。 為了確保正規化值 (英寸 < 12.0) ，此函式會執行 *模數* 12 算術。 `+`運算子會使用此函式來產生標準化值。  
  
 [!code-vb[VbVbcnProcedures#25](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#25)]  
  
 您可以使用下列程式碼來測試結構 `height` 。  
  
 [!code-vb[VbVbcnProcedures#26](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#26)]  

## <a name="see-also"></a>另請參閱

- [運算子程序](./operator-procedures.md)
- [How to: Define a Conversion Operator](./how-to-define-a-conversion-operator.md)
- [如何：呼叫運算子程序](./how-to-call-an-operator-procedure.md)
- [如何：使用定義運算子的類別](./how-to-use-a-class-that-defines-operators.md)
- [Operator Statement](../../../language-reference/statements/operator-statement.md)
- [Structure 陳述式](../../../language-reference/statements/structure-statement.md)
- [作法：宣告結構](../data-types/how-to-declare-a-structure.md)
- [Mod 運算子](../../../language-reference/operators/mod-operator.md)
