---
title: ISymUnmanagedScope 介面
ms.date: 03/30/2017
api_name:
- ISymUnmanagedScope
api_location:
- diasymreader.dll
api_type:
- COM
f1_keywords:
- ISymUnmanagedScope
helpviewer_keywords:
- ISymUnmanagedScope interface [.NET Framework debugging]
ms.assetid: 3db7a220-cfe9-4810-8ca8-a094bb8e0f5b
topic_type:
- apiref
ms.openlocfilehash: 9256342ad3a91e6770d6fd19d9d2f94fab267d3e
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/24/2020
ms.locfileid: "95725881"
---
# <a name="isymunmanagedscope-interface"></a>ISymUnmanagedScope 介面

表示方法內的語彙範圍。  
  
## <a name="methods"></a>方法  
  
|方法|描述|  
|------------|-----------------|  
|[GetChildren 方法](isymunmanagedscope-getchildren-method.md)|取得此範圍的子系。|  
|[GetEndOffset 方法](isymunmanagedscope-getendoffset-method.md)|取得此範圍的結束位移。|  
|[GetLocalCount 方法](isymunmanagedscope-getlocalcount-method.md)|取得在此範圍內定義的區域變數計數。|  
|[GetLocals 方法](isymunmanagedscope-getlocals-method.md)|取得在此範圍內定義的本機變數。|  
|[GetMethod 方法](isymunmanagedscope-getmethod-method.md)|取得包含此範圍的方法。|  
|[GetNamespaces 方法](isymunmanagedscope-getnamespaces-method.md)|取得在此範圍內使用的命名空間。|  
|[GetParent 方法](isymunmanagedscope-getparent-method.md)|取得此範圍的父範圍。|  
|[GetStartOffset 方法](isymunmanagedscope-getstartoffset-method.md)|取得此範圍的開始位移。|  
  
## <a name="requirements"></a>需求  

 **標頭：** CorSym .idl、CorSym。h  
  
## <a name="see-also"></a>另請參閱

- [診斷符號存放區介面](diagnostics-symbol-store-interfaces.md)
- [ISymUnmanagedScope2 介面](isymunmanagedscope2-interface.md)
