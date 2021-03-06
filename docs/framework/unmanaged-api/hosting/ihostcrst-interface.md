---
title: IHostCrst 介面
ms.date: 03/30/2017
api_name:
- IHostCrst
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IHostCrst
helpviewer_keywords:
- IHostCrst interface [.NET Framework hosting]
ms.assetid: ac298ebd-0815-47e4-a823-30b31baab903
topic_type:
- apiref
ms.openlocfilehash: 350af708456914c73929d2b8887173cf784d4294
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/24/2020
ms.locfileid: "95680550"
---
# <a name="ihostcrst-interface"></a>IHostCrst 介面

作為執行緒的重要區段的主機標記法。  
  
## <a name="methods"></a>方法  
  
|方法|描述|  
|------------|-----------------|  
|[Enter 方法](ihostcrst-enter-method.md)|進入重要區段。|  
|[Leave 方法](ihostcrst-leave-method.md)|離開重要區段。|  
|[SetSpinCount 方法](ihostcrst-setspincount-method.md)|設定重要區段的微調計數。|  
|[TryEnter 方法](ihostcrst-tryenter-method.md)|嘗試進入重要區段，並立即報告成功或失敗。|  
  
## <a name="remarks"></a>備註  

 `IHostCrst` 允許 common language runtime (CLR) 直接與主控制項的重要區段表示進行通訊，而不是使用 Win32 函數（例如或） `EnterCriticalSection` `LeaveCriticalSection` 。  
  
## <a name="requirements"></a>需求  

 **平台：** 請參閱 [系統需求](../../get-started/system-requirements.md)。  
  
 **標頭：** Mscoree.dll  
  
 連結 **庫：** 以資源的形式包含在 MSCorEE.dll 中  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另請參閱

- [ICLRSyncManager 介面](iclrsyncmanager-interface.md)
- [IHostSyncManager 介面](ihostsyncmanager-interface.md)
- [裝載介面](hosting-interfaces.md)
