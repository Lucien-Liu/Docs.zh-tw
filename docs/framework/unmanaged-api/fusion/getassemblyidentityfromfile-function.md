---
title: GetAssemblyIdentityFromFile 函式
ms.date: 03/30/2017
api_name:
- GetAssemblyIdentityFromFile
api_location:
- fusion.dll
- clr.dll
- mscorwks.dll
api_type:
- COM
f1_keywords:
- GetAssemblyIdentityFromFile
helpviewer_keywords:
- GetAssemblyIdentityFromFile function [.NET Framework fusion]
ms.assetid: 2c32da53-76c7-4048-84d0-d05207333004
topic_type:
- apiref
ms.openlocfilehash: 9580dd3bc5a7279549e8deadac95d35a33da74f8
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/24/2020
ms.locfileid: "95724477"
---
# <a name="getassemblyidentityfromfile-function"></a>GetAssemblyIdentityFromFile 函式

取得物件的指標，該 `IUnknown` 物件具有在 `IID` 指定檔案路徑的元件中指定的。  
  
## <a name="syntax"></a>語法  
  
```cpp  
HRESULT GetAssemblyIdentityFromFile (  
    [in]  LPCWSTR   pwzFilePath,  
    [in]  REFIID    riid,  
    [out] IUnknown  **ppIdentity  
 );  
```  
  
## <a name="parameters"></a>參數  

 `pwzFilePath`  
 在所要求元件的有效路徑。  
  
 `riid`  
 在 `IID` 要傳回之介面的。  
  
 `ppIdentity`  
 擴展傳回的介面指標。  
  
## <a name="requirements"></a>需求  

 **平台：** 請參閱 [系統需求](../../get-started/system-requirements.md)。  
  
 **標頭：** 融合。h  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另請參閱

- [IUnknown](/cpp/atl/iunknown)
- [融合全域靜態函式](fusion-global-static-functions.md)
