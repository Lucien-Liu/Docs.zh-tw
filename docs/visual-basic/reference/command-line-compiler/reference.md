---
title: -reference
ms.date: 03/13/2018
helpviewer_keywords:
- /reference compiler option [Visual Basic]
- r compiler option [Visual Basic]
- -reference compiler option [Visual Basic]
- /r compiler option [Visual Basic]
- reference compiler option [Visual Basic]
- -r compiler option [Visual Basic]
ms.assetid: 66bdfced-bbf6-43d1-a554-bc0990315737
ms.openlocfilehash: b489a164e56a5e3bdbf7e3cdf24ec330fadedf38
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/23/2020
ms.locfileid: "91097549"
---
# <a name="-reference-visual-basic"></a>-reference (Visual Basic) 

導致編譯器將指定元件中的類型資訊提供給您目前編譯的專案。  
  
## <a name="syntax"></a>語法  
  
```console  
-reference:fileList  
```

或

```console
-r:fileList  
```  
  
## <a name="arguments"></a>引數  
  
|詞彙|定義|  
|---|---|  
|`fileList`|必要。 以逗號分隔的組件檔案名稱清單。 如果檔案名稱包含空格，請用引號括住名稱。|  
  
## <a name="remarks"></a>備註  

 您匯入)  (s 必須包含元件中繼資料。 只有公用類型會顯示在元件外部。 [-Addmodule](addmodule.md)選項會從模組匯入中繼資料。  
  
 如果您參考元件 (元件 A) 該元件本身參考另一個元件 (元件 B) ，則在下列情況下，您必須參考元件 B：  
  
- 組件 A 的類型繼承自組件 B 的類型，或是實作組件 B 的介面。  
  
- 所叫用的欄位、屬性、事件或方法具有組件 B 的傳回型別或參數類型。  
  
 使用 [-libpath](libpath.md) 可指定一個或多個元件參考所在的目錄。  
  
 為了讓編譯器辨識元件中的型別 (不是模組) ，必須強制它解析型別。 您可以用來定義型別實例的範例之一。 您可以使用其他方式來解析編譯器元件中的型別名稱。 例如，如果您繼承自元件中的型別，則編譯器會知道型別名稱。  
  
 預設會使用會參考常用 .NET Framework 元件的 Vbc 回應檔。 `-noconfig`如果您不想讓編譯器使用 Vbc，請使用。  
  
 `-reference` 的簡短形式為 `-r`。  
  
## <a name="example"></a>範例  

 下列命令會從編譯原始 `Input.vb` 程式檔和參考元件 `Metad1.dll` ，並 `Metad2.dll` 產生 `Out.exe` 。  
  
```console
vbc -reference:metad1.dll,metad2.dll -out:out.exe input.vb  
```  
  
## <a name="see-also"></a>另請參閱

- [Visual Basic 命令列編譯器](index.md)
- [-noconfig](noconfig.md)
- [-目標 (Visual Basic) ](target.md)
- [公開](../../language-reference/modifiers/public.md)
- [編譯命令列的範例](sample-compilation-command-lines.md)
