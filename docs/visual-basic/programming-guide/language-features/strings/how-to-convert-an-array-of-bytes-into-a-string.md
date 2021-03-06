---
title: 如何：將位元組陣列轉換為字串
ms.date: 07/20/2015
helpviewer_keywords:
- string conversion [Visual Basic], arrays
- byte arrays [Visual Basic], converting to strings
- examples [Visual Basic], strings
- arrays [Visual Basic], converting to strings
ms.assetid: d0dc8317-9ab3-4324-99f7-3f5788c0e72a
ms.openlocfilehash: 49c1e454b845bd545d7a8dccb3a3d9a39ff2db21
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/23/2020
ms.locfileid: "91085655"
---
# <a name="how-to-convert-an-array-of-bytes-into-a-string-in-visual-basic"></a>如何：在 Visual Basic 中將位元組陣列轉換為字串

本主題說明如何將位元組陣列中的位元組轉換成字串。  
  
## <a name="example"></a>範例  

 這個範例會使用 <xref:System.Text.Encoding.GetString%2A> <xref:System.Text.Encoding.Unicode%2A?displayProperty=nameWithType> encoding 類別的方法，將位元組陣列中的所有位元組轉換成字串。  
  
 [!code-vb[VbVbalrStrings#72](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStrings/VB/Class2.vb#72)]  
  
 您可以從數種編碼選項中進行選擇，以將位元組陣列轉換成字串：  
  
- <xref:System.Text.Encoding.ASCII%2A?displayProperty=nameWithType>：取得 ASCII (7 位) 字元集的編碼方式。  
  
- <xref:System.Text.Encoding.BigEndianUnicode%2A?displayProperty=nameWithType>：使用位元組由大到小的順序，取得 UTF-16 格式的編碼方式。  
  
- <xref:System.Text.Encoding.Default%2A?displayProperty=nameWithType>：取得系統目前的 ANSI 字碼頁編碼方式。  
  
- <xref:System.Text.Encoding.Unicode%2A?displayProperty=nameWithType>：使用位元組由小到大的位元組順序，取得 UTF-16 格式的編碼方式。  
  
- <xref:System.Text.Encoding.UTF32%2A?displayProperty=nameWithType>：取得使用位元組由大到小的位元組順序的 32 UTF-8 格式編碼。  
  
- <xref:System.Text.Encoding.UTF7%2A?displayProperty=nameWithType>：取得 UTF-7 格式的編碼方式。  
  
- <xref:System.Text.Encoding.UTF8%2A?displayProperty=nameWithType>：取得 UTF-8 格式的編碼方式。  
  
## <a name="see-also"></a>另請參閱

- <xref:System.Text.Encoding?displayProperty=nameWithType>
- <xref:System.Text.Encoding.GetString%2A>
- [如何：在 Visual Basic 中將字串轉換為位元組陣列](how-to-convert-strings-into-an-array-of-bytes.md)
