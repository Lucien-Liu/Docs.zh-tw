---
title: 建立執行緒，並在啟動期間傳遞資料
description: 瞭解如何在 .NET 中的作業系統進程開始時建立執行緒和傳遞資料。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- threading [.NET], creating
- threading [.NET], passing data to threads
- threading [.NET], retrieving data from threads
ms.assetid: 52b32222-e185-4f42-91a7-eaca65c0ab6d
ms.openlocfilehash: 3f44eb9dabc9145cd0e6ec9bfd3d484c9079e803
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/18/2020
ms.locfileid: "94819925"
---
# <a name="creating-threads-and-passing-data-at-start-time"></a>建立執行緒，並在啟動期間傳遞資料

建立作業系統處理序時，作業系統會插入執行緒以在該處理序中執行程式碼，包括任何原始的應用程式定義域。 從那時起，不一定要建立或終結任何作業系統執行緒，就能建立和終結應用程式定義域。 如果正在執行的程式碼為受控碼，則可藉由擷取類型 <xref:System.Threading.Thread> 的靜態 <xref:System.Threading.Thread.CurrentThread%2A> 屬性，來取得正在目前應用程式定義域中執行之執行緒的 <xref:System.Threading.Thread> 物件。 本主題說明執行緒的建立，並討論將資料傳遞至執行緒程序的替代方案。  
  
## <a name="creating-a-thread"></a>建立執行緒

 建立新的 <xref:System.Threading.Thread> 物件，會建立新的受控執行緒。 <xref:System.Threading.Thread> 類別具有採用 <xref:System.Threading.ThreadStart> 委派或 <xref:System.Threading.ParameterizedThreadStart> 委派的建構函式；委派會包裝當您呼叫 <xref:System.Threading.Thread.Start%2A> 方法時新執行緒所叫用的方法。 呼叫 <xref:System.Threading.Thread.Start%2A> 超過一次，就會擲回 <xref:System.Threading.ThreadStateException>。  
  
 <xref:System.Threading.Thread.Start%2A> 方法會立即傳回，通常會在實際啟動新的執行緒之前。 您隨時都可使用 <xref:System.Threading.Thread.ThreadState%2A> 和 <xref:System.Threading.Thread.IsAlive%2A> 屬性來判斷執行緒的狀態，但不應使用這些屬性來進行執行緒活動的同步處理。  
  
> [!NOTE]
> 一旦啟動執行緒之後，就不需要保留對 <xref:System.Threading.Thread> 物件的參考。 執行緒會繼續執行，直到執行緒程序結束為止。  
  
 下列程式碼範例會建立兩個新的執行緒，來呼叫另一個物件上的執行個體和靜態方法。  
  
 [!code-cpp[System.Threading.ThreadStart2#2](../../../samples/snippets/cpp/VS_Snippets_CLR_System/system.Threading.ThreadStart2/CPP/source2.cpp#2)]
 [!code-csharp[System.Threading.ThreadStart2#2](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.Threading.ThreadStart2/CS/source2.cs#2)]
 [!code-vb[System.Threading.ThreadStart2#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.Threading.ThreadStart2/VB/source2.vb#2)]  
  
## <a name="passing-data-to-threads"></a>將資料傳遞到執行緒

<xref:System.Threading.ParameterizedThreadStart>當您呼叫時，委派提供簡單的方法，將包含資料的物件傳遞給執行緒 <xref:System.Threading.Thread.Start(System.Object)?displayProperty=nameWithType> 。 請參閱 <xref:System.Threading.ParameterizedThreadStart>，以取得程式碼範例。
  
 使用 <xref:System.Threading.ParameterizedThreadStart> 委派不是傳遞資料的型別安全方法，因為此 <xref:System.Threading.Thread.Start(System.Object)?displayProperty=nameWithType> 方法會接受任何物件。 替代方法是在協助程式類別中封裝執行緒程序和資料，並使用 <xref:System.Threading.ThreadStart> 委派來執行執行緒程序。 下列範例展示此技術：

 [!code-cpp[System.Threading.ThreadStart2#3](../../../samples/snippets/cpp/VS_Snippets_CLR_System/system.Threading.ThreadStart2/CPP/source3.cpp#3)]
 [!code-csharp[System.Threading.ThreadStart2#3](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.Threading.ThreadStart2/CS/source3.cs#3)]
 [!code-vb[System.Threading.ThreadStart2#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.Threading.ThreadStart2/VB/source3.vb#3)]  

因為沒有地方可從非同步呼叫傳回資料，所以 <xref:System.Threading.ThreadStart> 或 <xref:System.Threading.ParameterizedThreadStart> 委派都沒有傳回值。 若要擷取執行緒方法的結果，可使用回呼方法，如下一節所示。
  
## <a name="retrieving-data-from-threads-with-callback-methods"></a>使用回呼方法從執行緒擷取資料

 下列範例示範從執行緒擷取資料的回呼方法。 包含資料和執行緒方法之類別的建構函式也會接受代表回呼方法的委派；在執行緒方法結束之前，它會叫用回呼委派。  
  
 [!code-cpp[System.Threading.ThreadStart2#4](../../../samples/snippets/cpp/VS_Snippets_CLR_System/system.Threading.ThreadStart2/CPP/source4.cpp#4)]
 [!code-csharp[System.Threading.ThreadStart2#4](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.Threading.ThreadStart2/CS/source4.cs#4)]
 [!code-vb[System.Threading.ThreadStart2#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.Threading.ThreadStart2/VB/source4.vb#4)]  
  
## <a name="see-also"></a>請參閱

- <xref:System.Threading.Thread>
- <xref:System.Threading.ThreadStart>
- <xref:System.Threading.ParameterizedThreadStart>
- <xref:System.Threading.Thread.Start%2A?displayProperty=nameWithType>
- [執行緒](index.md)
- [使用執行緒和執行緒](using-threads-and-threading.md)
