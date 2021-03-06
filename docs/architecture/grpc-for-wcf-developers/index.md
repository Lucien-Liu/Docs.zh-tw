---
title: 適用于 WCF 開發人員的 ASP.NET Core gRPC-適用于 WCF 開發人員的 gRPC
description: 在 ASP.NET Core 3.0 中為 WCF 開發人員建立 gRPC 服務的簡介
ms.date: 09/02/2019
ms.openlocfilehash: c9cc5ef9c06d5262fb85850f8a3b178d46e5c6fa
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/24/2020
ms.locfileid: "95689273"
---
# <a name="aspnet-core-grpc-for-wcf-developers"></a>適用於 WCF 開發人員的 ASP.NET Core gRPC

![封面影像](./media/cover.png)

發行者

Microsoft 開發人員部門 .NET 和 Visual Studio 產品小組

Microsoft Corporation 部門

One Microsoft Way

Redmond, Washington 98052-6399

Copyright © 2019 by Microsoft Corporation

著作權所有，並保留一切權利。 本書內容的任何部分在未經過發行者書面許可下，不得以任何形式或透過任何方式進行重製或傳送。

本書依照「現況」提供，代表作者的觀點和意見。 本書中所述之觀點、意見與資訊 (包括 URL 及其他網際網路網站參考) 可能會隨時變更，恕不另行通知。

此處描述的一些範例僅供說明之用，純屬虛構。 並未影射或關聯任何真實的人、事、物。

Microsoft 與列於 <https://www.microsoft.com>「商標」網頁的商標是 Microsoft 集團的商標。

Docker whale 標誌是 Docker，Inc. 所用的注冊商標。

所有其他商標和標誌屬於其各自擁有者的財產。

作者：

> **Mark Rendle** -首席技術長- [視覺編碼](https://visualrecode.com)
>
> **Miranda Steiner** -技術作者

編輯器：

> **Maira Wenzel** -資深內容開發人員-Microsoft

## <a name="introduction"></a>簡介

gRPC 是一種現代化的架構，可用於建立網路服務和分散式應用程式。 想像 Windows Communication Foundation (WCF) NetTCP 系結的效能，並結合 SOAP 的跨平臺互通性。 gRPC 建基於 HTTP/2 和 Protobuf 訊息編碼通訊協定，以提供應用程式與服務之間的高效能、低頻寬通訊。 它支援跨最受歡迎的程式設計語言和平臺產生伺服器和用戶端程式代碼，包括 .NET、JAVA、Python、Node.js、Go 和 c + +。 使用 ASP.NET Core 3.0 中 gRPC 的第一層級支援，以及適用于 .NET Framework 4.x 的現有 gRPC 工具和程式庫，這是 WCF 的絕佳替代方案，適用于想要在組織中採用 .NET Core 的開發團隊。

## <a name="who-should-use-this-guide"></a>誰應該使用本指南

本指南是針對在先前使用 WCF 的 .NET Framework 或 .NET Core 中工作的開發人員，以及想要將其應用程式遷移至 .NET Core 3.0 和更新版本之新式 RPC 環境的開發人員所撰寫。 更常見的是，如果您要升級或考慮升級到 .NET Core 3.0，而且您想要使用內建的 gRPC 工具，本指南也很有用。

## <a name="how-you-can-use-this-guide"></a>此指南的使用方式

這是在 ASP.NET Core 3.0 中建立 gRPC 服務的簡短簡介，並且將 WCF 的特定參考視為類似的平臺。 它會說明 gRPC 的原則，並將每個概念與 WCF 的對等功能產生關聯，並提供將現有 WCF 應用程式遷移至 gRPC 的指引。 它也適用于具有 WCF 經驗的開發人員，而且想要學習 gRPC 來建立新的服務。 您可以使用範例應用程式做為您自己的專案的範本或參考，也可以自由地複製和重複使用書籍中的程式碼或其範例。

請隨意將本指南轉寄給您的小組，協助確保對這些考量和機會有共同的了解。 讓每個人都能從一組常見的條款和基礎原則中工作，有助於確保架構模式和實務的一致應用。

## <a name="references"></a>參考資料

- **gRPC 網站**
  <https://grpc.io>
- **針對伺服器應用程式在 .NET Core 和 .NET Framework 之間進行選擇**
  <https://docs.microsoft.com/dotnet/standard/choosing-core-framework-server>

>[!div class="step-by-step"]
>[下一步](introduction.md)
