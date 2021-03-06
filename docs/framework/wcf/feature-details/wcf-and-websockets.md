---
title: WCF 及 WebSockets
ms.date: 03/30/2017
ms.assetid: 1e53b49e-022c-49c7-8984-4b21b53c05b3
ms.openlocfilehash: 2ee27eef015ee8c2fbee8360b444343a1c757097
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/26/2020
ms.locfileid: "96281582"
---
# <a name="wcf-and-websockets"></a>WCF 及 WebSockets

.NET Framework 4.5 在 Windows Communication Foundation 中引入了 WebSockets 的支援。  WebSockets 是基於標準的高效率技術，可在標準 HTTP 通訊埠 80 和 443 上進行雙向通訊。 使用標準 HTTP 通訊埠允許 WebSockets 透過媒介在 Web 上進行通訊。  為了支援透過 WebSocket 傳輸所進行通訊，已加入兩個新的標準繫結。 <xref:System.ServiceModel.NetHttpBinding> 和 <xref:System.ServiceModel.NetHttpsBinding>。 您可以藉由存取屬性，在上設定 Websocket 特定的設定 <xref:System.ServiceModel.Channels.HttpTransportBindingElement> <xref:System.ServiceModel.Channels.HttpTransportBindingElement.WebSocketSettings%2A> 。
  
## <a name="in-this-section"></a>本節內容  

 [使用 NetHttpBinding](using-the-nethttpbinding.md)  
 討論 <xref:System.ServiceModel.NetHttpBinding> 及其設定方式。  
  
 [作法：建立會透過 WebSockets 進行通訊的 WCF 服務](how-to-create-a-wcf-service-that-communicates-over-websockets.md)  
 說明如何建立透過 WebSockets 進行通訊的 WCF 服務。  
  
## <a name="reference"></a>參考  
  
## <a name="related-sections"></a>相關章節
