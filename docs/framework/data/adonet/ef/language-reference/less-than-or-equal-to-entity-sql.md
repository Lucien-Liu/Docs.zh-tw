---
title: '<= (小於或等於)  (Entity SQL) '
ms.date: 03/30/2017
ms.assetid: 7c46da5c-fa09-4d90-adcc-c7e1b769d8e6
ms.openlocfilehash: 09f2a7f53d00739b8542cb1149397f24d3b04f42
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/24/2020
ms.locfileid: "91161798"
---
# <a name="-less-than-or-equal-to-entity-sql"></a>\<= (小於或等於) (Entity SQL)

比較兩個運算式來判斷左運算式的值是否小於或等於右運算式。  
  
## <a name="syntax"></a>語法  
  
```sql  
expression <= expression  
```  
  
## <a name="arguments"></a>引數  

 `expression`  
 任何有效的運算式。 這兩個運算式的類型，都必須是可以隱含轉換的資料類型。  
  
## <a name="result-types"></a>結果類型  

 如果左運算式的值小於或等於右運算式則為`true` ；否則為 `false`。  
  
## <a name="example"></a>範例  

 下列 Entity SQL 查詢使用 <= 比較運算子來比較兩個運算式，以判斷左運算式的值是否小於或等於右運算式。 此查詢是根據 AdventureWorks Sales Model。 若要編譯及執行此查詢，請遵循以下步驟：  
  
1. 遵循 [How to: Execute a Query that Returns StructuralType Results](../how-to-execute-a-query-that-returns-structuraltype-results.md)中的程序進行。  
  
2. 將下列查詢當成引數，傳遞至 `ExecuteStructuralTypeQuery` 方法：  
  
 [!code-sql[DP EntityServices Concepts#LESS_OR_EQUALS](~/samples/snippets/tsql/VS_Snippets_Data/dp entityservices concepts/tsql/entitysql.sql#less_or_equals)]  
  
## <a name="see-also"></a>另請參閱

- [Entity SQL 參考](entity-sql-reference.md)
