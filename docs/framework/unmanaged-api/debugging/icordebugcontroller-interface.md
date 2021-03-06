---
title: ICorDebugController 介面
ms.date: 03/30/2017
api_name:
- ICorDebugController
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugController
helpviewer_keywords:
- ICorDebugController interface [.NET Framework debugging]
ms.assetid: dbb1c4dc-269a-459b-ab1d-6c70788782ce
topic_type:
- apiref
ms.openlocfilehash: 1ca9e55a2183ca4293d30607496b588cbf21d6dd
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/24/2020
ms.locfileid: "95679943"
---
# <a name="icordebugcontroller-interface"></a>ICorDebugController 介面

表示可以控制程式碼執行內容的範圍 (<xref:System.Diagnostics.Process> 或 <xref:System.AppDomain> 其中一項)。  
  
## <a name="methods"></a>方法  
  
|方法|描述|  
|------------|-----------------|  
|`ICorDebugController::CanCommitChanges`|這個方法已過時。|  
|`ICorDebugController::CommitChanges`|這個方法已過時。|  
|[Continue 方法](icordebugcontroller-continue-method.md)|在呼叫 [ICorDebugController：： Stop](icordebugcontroller-stop-method.md)之後，繼續執行 managed 執行緒。|  
|[Detach 方法](icordebugcontroller-detach-method.md)|卸離進程或應用程式域中的偵錯工具。|  
|[EnumerateThreads 方法](icordebugcontroller-enumeratethreads-method.md)|取得進程中使用中 managed 執行緒的列舉值。|  
|[HasQueuedCallbacks 方法](icordebugcontroller-hasqueuedcallbacks-method.md)|取得值，這個值表示是否目前已針對指定的執行緒將任何受控回呼排入佇列。|  
|[IsRunning 方法](icordebugcontroller-isrunning-method.md)|取得值，這個值表示進程中的執行緒目前是否可自由執行。|  
|[SetAllThreadsDebugState 方法](icordebugcontroller-setallthreadsdebugstate-method.md)|設定進程中所有 managed 執行緒的偵錯工具狀態。|  
|[Stop 方法](icordebugcontroller-stop-method.md)|在進程中執行 managed 程式碼的所有線程上執行合作式停止。|  
|[Terminate 方法](icordebugcontroller-terminate-method.md)|使用指定的結束代碼終止進程。|  
  
## <a name="remarks"></a>備註  

 如果 `ICorDebugController` 控制進程，則範圍會包含進程的所有線程。 如果 `ICorDebugController` 是控制應用程式域，則範圍僅包含該特定應用程式域的執行緒。  
  
> [!NOTE]
> 這個介面不支援跨電腦或跨處理序的遠端呼叫。  
  
## <a name="requirements"></a>需求  

 **平台：** 請參閱 [系統需求](../../get-started/system-requirements.md)。  
  
 **標頭：** CorDebug.idl、CorDebug.h  
  
 **程式庫：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另請參閱

- [偵錯介面](debugging-interfaces.md)
