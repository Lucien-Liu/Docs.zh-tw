---
title: 可檢視性模式
description: 雲端原生應用程式的可檢視性模式
ms.date: 08/10/2020
ms.openlocfilehash: 2a6ef0e58f1e20667167042614768b099f640858
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/15/2020
ms.locfileid: "90539849"
---
# <a name="observability-patterns"></a>可檢視性模式

就像是為了協助應用程式中的程式碼配置而開發的一樣，也會以可靠的方式來操作應用程式的模式。 維護應用程式的三個實用模式已出現： **記錄**、 **監視**和 **警示**。

## <a name="when-to-use-logging"></a>使用記錄的時機

無論我們有多小心，應用程式幾乎一律會在生產環境中以非預期的方式表現。 當使用者報告應用程式的問題時，可以在問題發生時查看應用程式的狀況。 當應用程式正在執行時，其中一種最嘗試的方法和真正的方法，就是讓應用程式寫下其執行的工作。 此進程稱為記錄。 在生產環境中發生任何失敗或問題時，目標應該是在非生產環境中重現發生失敗的情況。 備妥適當的記錄功能，可讓開發人員遵循此藍圖，以在可以測試和實驗的環境中重複發生問題。

### <a name="challenges-when-logging-with-cloud-native-applications"></a>使用雲端原生應用程式進行記錄時的挑戰

在傳統的應用程式中，記錄檔通常會儲存在本機電腦上。 事實上，在類似 Unix 的作業系統上，有一個資料夾結構定義來保存任何記錄，通常是在底下 `/var/log` 。

![在整合型應用程式中記錄至檔案。 ](./media/single-monolith-logging.png)
**圖 7-1**。 在整合型應用程式中記錄至檔案。

在單一機器上記錄至一般檔案的實用性，在雲端環境中會大幅降低。 產生記錄檔的應用程式可能無法存取本機磁片，或本機磁片可能會有高度暫時性的情況，因為容器會在實體機器周圍隨機進行。 即使是在多個節點之間輕鬆擴充整合型應用程式，也可能使找出適當的檔案型記錄檔變得很困難。

![在已調整的整合型應用程式中記錄至檔案。 ](./media/multiple-node-monolith-logging.png)
**圖 7-2**。 在已調整的整合型應用程式中記錄至檔案。

使用微服務架構開發的雲端原生應用程式也會對以檔案為基礎的記錄器造成一些挑戰。 使用者要求現在可能會跨越在不同電腦上執行的多個服務，而且可能包含無伺服器功能，完全無法存取本機檔案系統。 將來自使用者或會話的記錄與這些服務和機器之間的記錄相互關聯相當困難。

![記錄至微服務應用程式中的本機檔案。 ](./media/local-log-file-per-service.png)
**圖 7-3**。 記錄至微服務應用程式中的本機檔案。

最後，某些雲端原生應用程式中的使用者數目很高。 假設每位使用者在登入應用程式時，都會產生一百行記錄訊息。 在隔離的情況下，這是可管理的，但會將超過100000的使用者和記錄的數量變得夠大，因為需要特製化工具來支援有效使用記錄檔。

### <a name="logging-in-cloud-native-applications"></a>雲端原生應用程式中的記錄

每種程式設計語言都有可允許寫入記錄檔的工具，而寫入這些記錄的額外負荷通常很低。 許多記錄程式庫都提供記錄不同類型的嚴重性，可在執行時間進行調整。 例如，Serilog 連結 [庫](https://serilog.net/) 是適用于 .net 的熱門結構化記錄程式庫，可提供下列記錄層級：

* 「詳細資訊」
* 偵錯
* 資訊
* 警告
* 錯誤
* 嚴重

這些不同的記錄層級會在記錄中提供細微的資料細微性。 當應用程式在生產環境中正常運作時，可能會將它設定為只記錄重要的訊息。 當應用程式的行為不正常時，會增加記錄層級，以收集更詳細的記錄檔。 這會平衡效能，以簡化偵錯工具的效能。

記錄工具的高效能和詳細資訊的 tunability，應該鼓勵開發人員經常記錄。 許多人偏好記錄每個方法的進入和離開模式。 這種方法聽起來可能像麻煩，不過開發人員很少會想要進行較少的記錄。 事實上，為了將記錄新增至有問題的方法時，執行部署的唯一目的並不罕見。 錯誤記錄太多，而且沒有太少。 您可以使用某些工具來自動提供這類記錄。

由於在雲端原生應用程式中使用以檔案為基礎之記錄的相關挑戰，因此偏好使用集中式記錄。 記錄檔是由應用程式收集，並隨附于記錄和儲存記錄的中央記錄應用程式。 此類別的系統每天可以內嵌數十 gb 的記錄。

建立橫跨許多服務的記錄時，請遵循一些標準實務，也會很有説明。 例如，在冗長的互動開始時產生相互 [關聯識別碼](https://blog.rapid7.com/2016/12/23/the-value-of-correlation-ids/) ，然後將它記錄在與該互動相關的每個訊息中，可讓您更輕鬆地搜尋所有相關訊息。 其中一個只需要尋找單一訊息，然後將相互關聯識別碼解壓縮以找出所有相關訊息。 另一個範例是確保每個服務的記錄格式都相同，無論使用哪種語言或記錄程式庫。 這種標準化可讓您更輕鬆地讀取記錄。 圖7-4 示範微服務架構如何在其工作流程中運用集中式記錄。

![來自各種來源的記錄會內嵌到集中式記錄存放區。 ](./media/centralized-logging.png)
**圖 7-4**。 來自各種來源的記錄會內嵌到集中式記錄存放區。

## <a name="challenges-with-detecting-and-responding-to-potential-app-health-issues"></a>偵測及回應潛在應用程式健康情況問題的挑戰

有些應用程式不是要徑任務。 也許它們只能在內部使用，而當發生問題時，使用者可以聯繫負責的小組，而且可以重新開機應用程式。 不過，客戶通常會對他們取用的應用程式產生更高的期望。 您應該知道應用程式在使用者執行 *之前* ，或在使用者通知您之前發生問題。 否則，當您注意到在您的應用程式或甚至是您的組織中 deriding 的社交媒體貼文時，第一次知道問題的原因可能是您。

您可能需要考慮的一些案例包括：

- 您應用程式中的一項服務會保持失敗並重新啟動，因而導致間歇性的回應緩慢。
- 在當日的某些時間，您的應用程式回應時間很慢。
- 在最近的部署之後，資料庫上的負載就會 tripled。

適當地實行，監視可讓您知道將會導致問題的狀況，讓您在基礎條件產生任何重大的使用者影響之前解決這些問題。

### <a name="monitoring-cloud-native-apps"></a>監視雲端原生應用程式

某些集中式記錄系統採用其他角色來收集純記錄以外的遙測資料。 他們可以收集計量，例如執行資料庫查詢的時間、從 web 伺服器進行的平均回應時間，甚至是作業系統所報告的 CPU 負載平均和記憶體壓力。 這些系統可結合記錄，全面瞭解系統中節點的健全狀況，以及整體的應用程式。

監視工具的計量收集功能也可以從應用程式內以手動方式進行饋送。 特別感興趣的商務流程（例如註冊或訂購的新使用者）可以進行檢測，使其在中央監視系統中遞增計數器。 這會將監視工具解除鎖定，不僅能監視應用程式的健康情況，還能監視企業的健康情況。

您可以在記錄匯總工具中建立查詢，以尋找特定的統計資料或模式，然後以圖形形式顯示在自訂儀表板上。 小組通常會投資大量的現成顯示器，以旋轉與應用程式相關的統計資料。 如此一來，就可以輕鬆地在發生問題時加以查看。

無論應用程式是單一進程整合型應用程式或分散式微服務架構，雲端原生監視工具都能提供應用程式的即時遙測和見解。 其中包含的工具可讓您從應用程式收集資料，以及用來查詢和顯示應用程式健康情況相關資訊的工具。

## <a name="challenges-with-reacting-to-critical-problems-in-cloud-native-apps"></a>在雲端原生應用程式中回應重大問題的挑戰

如果您需要對應用程式的問題做出反應，您需要某種方式來警示適當的人員。 這是第三個雲端原生應用程式可檢視性模式，而且取決於記錄和監視。 您的應用程式必須準備好進行記錄，以允許診斷問題，而在某些情況下則會送入監視工具。 它需要監視，才能在單一位置匯總應用程式計量和健全狀況資料。 一旦建立之後，就可以建立規則，以在某些計量超出可接受的層級時觸發警示。

一般而言，警示會在監視之上分層，讓某些條件觸發適當的警示，以通知小組成員有緊急問題。 某些可能需要警示的案例包括：

- 您應用程式的其中一個服務在停機1分鐘後沒有回應。
- 您的應用程式傳回不成功的 HTTP 回應給超過1% 的要求。
- 您應用程式的金鑰端點平均回應時間超過2000毫秒。

### <a name="alerts-in-cloud-native-apps"></a>雲端原生應用程式中的警示

您可以針對監視工具製作查詢，以尋找已知的失敗狀況。 例如，查詢可以搜尋傳入的記錄，以取得 HTTP 狀態碼500的指示，這表示 web 伺服器有問題。 一旦偵測到上述其中一種情況，就可以將電子郵件或 SMS 傳送給原始服務的擁有者，以開始調查。

不過，通常單一500錯誤並不足以判斷是否發生問題。 這可能表示使用者輸入錯誤的密碼或輸入了一些格式不正確的資料。 當偵測到大於500錯誤的平均數目時，才會引發警示查詢。

警示中最具破壞性的模式之一，是引發過多的警示讓人們調查。 服務擁有者很快就會降低了戒心至先前調查的錯誤，並且發現無害。 然後，當發生 true 錯誤時，就會在數百個誤報的雜訊中遺失。 [完成 Wolf](https://en.wikipedia.org/wiki/The_Boy_Who_Cried_Wolf)的 parable 會經常被告知孩子，以警告他們這項危險。 請務必確定引發的警示會指出真正的問題。

>[!div class="step-by-step"]
>[上一個](monitoring-health.md) 
>[下一步](logging-with-elastic-stack.md)
