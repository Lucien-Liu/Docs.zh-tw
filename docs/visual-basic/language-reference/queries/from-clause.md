---
title: From 子句
ms.date: 07/20/2015
f1_keywords:
- vb.QueryFrom
- vb.QueryFromIn
- vb.QueryFromLet
helpviewer_keywords:
- queries [Visual Basic], From
- From clause [Visual Basic]
- From statement [Visual Basic]
ms.assetid: 83e3665e-68a0-4540-a3a3-3d777a0f95d5
ms.openlocfilehash: 120ba6da11bffc3a0e81873d1fd606633724723d
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/22/2020
ms.locfileid: "90875256"
---
# <a name="from-clause-visual-basic"></a>From 子句 (Visual Basic)

指定一或多個範圍變數和要查詢的集合。  
  
## <a name="syntax"></a>Syntax  
  
```vb  
From element [ As type ] In collection [ _ ]  
  [, element2 [ As type2 ] In collection2 [, ... ] ]  
```  
  
## <a name="parts"></a>組件  
  
|詞彙|定義|  
|---|---|  
|`element`|必要。 用來逐一查看集合元素的 *範圍變數* 。 `collection`當查詢逐一查看時，會使用範圍變數來參考的每個成員 `collection` 。 必須是可列舉的型別。|  
|`type`|選擇性。 `element` 的類型。 如果未 `type` 指定，則 `element` 會從推斷型別 `collection` 。|  
|`collection`|必要。 參考要查詢的集合。 必須是可列舉的型別。|  
  
## <a name="remarks"></a>備註  

 `From`子句是用來識別查詢的來源資料，以及用來參考來源集合中之元素的變數。 這些變數稱為 *範圍變數*。 `From`查詢需要子句，除非 `Aggregate` 使用子句來識別只傳回匯總結果的查詢。 如需詳細資訊，請參閱 [Aggregate 子句](aggregate-clause.md)。  
  
 您可以 `From` 在查詢中指定多個子句，以識別要聯結的多個集合。 當指定多個集合時，會個別反復查看它們，或者如果它們是相關的，您可以加入它們。 您可以使用子句來隱含聯結集合 `Select` ，或使用或子句來明確地聯結集合 `Join` `Group Join` 。 或者，您可以在單一子句中指定多個範圍變數和集合 `From` ，並以逗號分隔每個相關的範圍變數和集合。 下列程式碼範例顯示子句的兩個語法選項 `From` 。  
  
 [!code-vb[VbSimpleQuerySamples#21](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbSimpleQuerySamples/VB/QuerySamples1.vb#21)]  
  
 `From`子句會定義查詢的範圍，這類似于迴圈的範圍 `For` 。 因此， `element` 查詢範圍中的每個範圍變數都必須有唯一的名稱。 因為您可以為查詢指定多個 `From` 子句，所以後續 `From` 的子句可以參考子句中的範圍變數 `From` ，或者可以參考前一個子句中的範圍變數 `From` 。 例如，下列範例顯示一個嵌套子句， `From` 其中第二個子句中的集合是以第一個子句中範圍變數的屬性為基礎。  
  
 [!code-vb[VbSimpleQuerySamples#22](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbSimpleQuerySamples/VB/QuerySamples1.vb#22)]  
  
 每個 `From` 子句後面都可以加上其他查詢子句的任何組合，以精簡查詢。 您可以利用下列方式來精簡查詢：  
  
- 使用 `From` 和 `Select` 子句，或使用或子句明確地結合多個集合 `Join` `Group Join` 。  
  
- 使用 `Where` 子句來篩選查詢結果。  
  
- 使用子句來排序結果 `Order By` 。  
  
- 使用子句將類似的結果群組在一起 `Group By` 。  
  
- 您 `Aggregate` 可以使用子句來識別要評估整個查詢結果的彙總函式。  
  
- 使用 `Let` 子句來引進反覆運算變數，其值是由運算式而非集合所決定。  
  
- 使用 `Distinct` 子句來忽略重複的查詢結果。  
  
- 使用 `Skip` 、、 `Take` `Skip While` 和子句來識別要傳回的結果部分 `Take While` 。  
  
## <a name="example"></a>範例  

 下列查詢運算式使用 `From` 子句來為 `cust` 集合中的每個物件宣告範圍變數 `Customer` `customers` 。 `Where`子句會使用範圍變數，將輸出限制為來自指定區域的客戶。 `For Each`迴圈會顯示查詢結果中每個客戶的公司名稱。  
  
 [!code-vb[VbSimpleQuerySamples#23](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbSimpleQuerySamples/VB/QuerySamples1.vb#23)]  
  
## <a name="see-also"></a>另請參閱

- [查詢](index.md)
- [Visual Basic 中的 LINQ 簡介](../../programming-guide/language-features/linq/introduction-to-linq.md)
- [For Each...Next 陳述式](../statements/for-each-next-statement.md)
- [For...Next 陳述式](../statements/for-next-statement.md)
- [Select 子句](select-clause.md)
- [Where 子句](where-clause.md)
- [Aggregate Clause](aggregate-clause.md)
- [Distinct 子句](distinct-clause.md)
- [Join 子句](join-clause.md)
- [Group Join 子句](group-join-clause.md)
- [Order By 子句](order-by-clause.md)
- [Let 子句](let-clause.md)
- [Skip 子句](skip-clause.md)
- [Take 子句](take-clause.md)
- [Skip While 子句](skip-while-clause.md)
- [Take While 子句](take-while-clause.md)
