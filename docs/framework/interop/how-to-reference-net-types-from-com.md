---
title: 作法：參考 COM 的 .NET 類型
description: 參考 COM 的 .NET 類型。 VB 用戶端可以在物件瀏覽器中查看 .NET 物件，但是 c + + 用戶端必須使用 import 指示詞來參考 .TLB 檔案 \# 。
ms.date: 03/30/2017
dev_langs:
- cpp
helpviewer_keywords:
- importing type library
- COM interop, referencing .NET types
- interoperation with unmanaged code, referencing .NET types
- referencing .NET types
- interoperation with unmanaged code, importing type library
- type libraries
- COM interop, importing type library
ms.assetid: 54917f6f-cb18-4103-b622-856b55da93f3
ms.openlocfilehash: 4e6e9f13364241517167c341a04883a199e9d6c2
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/26/2020
ms.locfileid: "96267022"
---
# <a name="how-to-reference-net-types-from-com"></a>作法：參考 COM 的 .NET 類型

從用戶端和伺服器程式碼的觀點來看，COM 和 .NET Framework 之間的差異大部分是無形的。 Microsoft Visual Basic 用戶端可以在物件瀏覽器中檢視 .NET 物件，這會公開物件方法和語法、屬性及欄位，完全如同它是任何其他 COM 物件一樣。  
  
 匯入型別程式庫的程序對 C++ 的用戶端而言稍嫌複雜，不過您可以使用相同的工具，將中繼資料匯出至 COM 類型程式庫。 若要從 Unmanaged C++ 用戶端參考 .NET 物件成員，請使用 **#import** 指示詞參考 TLB 檔案 (使用 Tlbexp.exe 產生)。 從 C++ 參考型別程式庫時，您必須指定 **raw_interfaces_only** 選項，或匯入基底類別程式庫 Mscorlib.tlb 中的定義。  
  
### <a name="to-import-a-library"></a>匯入程式庫  
  
- 在 **#import** 指示詞中指定 **raw_interfaces_only** 選項。 例如：  
  
    ```cpp  
    #import "..\LoanLib\LoanLib.tlb" raw_interfaces_only  
    ```  
  
     -或-  
  
- 包括 Mscorlib.tlb 的 #import 指示詞。 例如：  
  
    ```cpp  
    #import "mscorlib.tlb"  
    #import "..\LoanLib\LoanLib.tlb"  
    ```  
  
## <a name="see-also"></a>另請參閱

- [將 .NET Framework 元件公開給 COM](exposing-dotnet-components-to-com.md)
- [向 COM 註冊組件](registering-assemblies-with-com.md)
- [呼叫 .NET 物件](/previous-versions/dotnet/netframework-4.0/8hw8h46b(v=vs.100))
- [部署供 COM 存取的應用程式](/previous-versions/dotnet/netframework-4.0/c2850st8(v=vs.100))
