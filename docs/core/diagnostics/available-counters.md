---
title: .NET 中的知名 EventCounters
description: 檢查 .NET 執行時間和程式庫所發佈的 EventCounters。
ms.topic: reference
ms.date: 12/17/2020
ms.openlocfilehash: 724e49ba616a7f656d629a0443ec5e322a51d6f5
ms.sourcegitcommit: 4b79862c5b41fbd86cf38f926f6a49516059f6f2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/18/2020
ms.locfileid: "97681958"
---
# <a name="well-known-eventcounters-in-net"></a>.NET 中的知名 EventCounters

.NET 執行時間和程式庫會執行併發布數個， [`EventCounter`](./event-counters.md) 可用於識別和診斷各種效能問題。 本檔是提供者的參考，可用來監視這些提供者 `EventCounters` 及其描述。

## <a name="systemruntime-counters"></a>System. Runtime 計數器

下列計數器會發佈為 .NET 執行時間的一部分 (CoreCLR) 並在中維護 [`RuntimeEventSource.cs`](https://github.com/dotnet/coreclr/blob/master/src/System.Private.CoreLib/src/System/Diagnostics/Eventing/RuntimeEventSource.cs) 。

| 計數器 | 描述 |
|--|--|
| :::no-loc text="% Time in GC since last GC"::: (`time-in-gc`) | 自上次 GC 之後 GC 的時間百分比 |
| :::no-loc text="Allocation Rate"::: (`alloc-rate`) | 每個更新間隔所配置的位元組數 |
| :::no-loc text="CPU Usage"::: (`cpu-usage`) | 進程的 CPU 使用量百分比 |
| :::no-loc text="Exception Count"::: (`exception-count`) | 已發生的例外狀況數目 |
| :::no-loc text="GC Heap Size"::: (`gc-heap-size`) | 考慮要配置的位元組數目（根據 <xref:System.GC.GetTotalMemory(System.Boolean)?displayProperty=nameWithType> |
| :::no-loc text="Gen 0 GC Count"::: (`gen-0-gc-count`) | Gen 0 每個更新間隔發生 GC 的次數 |
| :::no-loc text="Gen 0 Size"::: (`gen-0-size`) | Gen 0 GC 的位元組數目 |
| :::no-loc text="Gen 1 GC Count"::: (`gen-1-gc-count`) | 每個更新間隔的 Gen 1 發生 GC 的次數 |
| :::no-loc text="Gen 1 Size"::: (`gen-1-size`) | Gen 1 GC 的位元組數目 |
| :::no-loc text="Gen 2 GC Count"::: (`gen-2-gc-count`) | 每個更新間隔的 Gen 2 發生 GC 的次數 |
| :::no-loc text="Gen 2 Size"::: (`gen-2-size`) | Gen 2 GC 的位元組數目 |
| :::no-loc text="LOH Size"::: (`loh-size`) | 大型物件堆積的位元組數目 |
| :::no-loc text="POH Size"::: (`poh-size`) | 可在 .NET 5 和更新版本上使用的釘選物件堆積 (位元組數目)  |
| :::no-loc text="GC Fragmentation"::: (`gc-fragmentation`) | GC 堆積片段 (可在 .NET 5 和更新版本上使用)  |
| :::no-loc text="Monitor Lock Contention Count"::: (`monitor-lock-contention-count`) | 嘗試取得監視器鎖定時，會發生爭用的次數，根據 <xref:System.Threading.Monitor.LockContentionCount?displayProperty=nameWithType> |
| :::no-loc text="Number of Active Timers"::: (`active-timer-count`) | <xref:System.Threading.Timer>目前作用中的實例數目，根據<xref:System.Threading.Timer.ActiveCount?displayProperty=nameWithType> |
| :::no-loc text="Number of Assemblies Loaded"::: (`assembly-count`) | 在 <xref:System.Reflection.Assembly> 某個時間點載入進程中的實例數目 |
| :::no-loc text="ThreadPool Completed Work Item Count"::: (`threadpool-completed-items-count`) | 到目前為止已處理的工作專案數 <xref:System.Threading.ThreadPool> |
| :::no-loc text="ThreadPool Queue Length"::: (`threadpool-queue-length`) | 目前排入佇列中要處理的工作專案數 <xref:System.Threading.ThreadPool> |
| :::no-loc text="ThreadPool Thread Count"::: (`threadpool-thread-count`) | 目前存在於中的執行緒集區執行緒數目 <xref:System.Threading.ThreadPool> （根據 <xref:System.Threading.ThreadPool.ThreadCount?displayProperty=nameWithType> |
| :::no-loc text="Working Set"::: (`working-set`) | 在時間點基礎上對應至進程內容的實體記憶體數量 <xref:System.Environment.WorkingSet?displayProperty=nameWithType> |
| :::no-loc text="IL Bytes Jitted"::: (`il-bytes-jitted`) | 以位元組為單位 (可在 .NET 5 和更新版本上使用的最新 ILs 總大小)  |
| :::no-loc text="Method Jitted Count"::: (`method-jitted-count`) |  (可在 .NET 5 和更新版本上使用的 JIT 編譯方法數目)  |

## <a name="microsoftaspnetcorehosting-counters"></a>"AspNetCore. 裝載" 計數器

下列計數器會在 [ASP.NET Core](/aspnet/core) 中發行，並在中進行維護 [`HostingEventSource.cs`](https://github.com/dotnet/aspnetcore/blob/master/src/Hosting/Hosting/src/Internal/HostingEventSource.cs) 。

| 計數器 | 描述 |
|--|--|
| :::no-loc text="Current Requests"::: (`current-requests`) | 已啟動但尚未停止的要求總數 |
| :::no-loc text="Failed Requests"::: (`failed-requests`) | 應用程式存留期內發生的失敗要求總數 |
| :::no-loc text="Request Rate"::: (`requests-per-second`) | 每個更新間隔發生的要求數目 |
| :::no-loc text="Total Requests"::: (`total-requests`) | 應用程式存留期內發生的要求總數 |

## <a name="microsoftaspnetcorehttpconnections-counters"></a>"AspNetCore" 計數器

下列計數器會在 [ASP.NET Core SignalR](/aspnet/core/signalr/introduction) 中發佈，並在中進行維護 [`HttpConnectionsEventSource.cs`](https://github.com/dotnet/aspnetcore/blob/master/src/SignalR/common/Http.Connections/src/Internal/HttpConnectionsEventSource.cs) 。

| 計數器 | 描述 |
|--|--|
| :::no-loc text="Average Connection Duration"::: (`connections-duration`) | 連接的平均持續時間（以毫秒為單位） |
| :::no-loc text="Current Connections"::: (`current-connections`) | 已啟動但尚未停止的作用中連接數目 |
| :::no-loc text="Total Connections Started"::: (`connections-started`) | 已啟動的連接總數 |
| :::no-loc text="Total Connections Stopped"::: (`connections-stopped`) | 已停止的連接總數 |
| :::no-loc text="Total Connections Timed Out"::: (`connections-timed-out`) | 已超時的連接總數 |

## <a name="microsoft-aspnetcore-server-kestrel-counters"></a>"Microsoft AspNetCore-Kestrel" 計數器

下列計數器會發佈為 [ASP.NET Core Kestrel web 伺服器](/aspnet/core/fundamentals/servers/kestrel) 的一部分，並在中進行維護 [`KestrelEventSource.cs`](https://github.com/dotnet/aspnetcore/blob/master/src/Servers/Kestrel/Core/src/Internal/Infrastructure/KestrelEventSource.cs) 。

| 計數器 | 描述 |
|--|--|
| :::no-loc text="Connection Queue Length"::: (`connection-queue-length`) | 連接佇列的目前長度 |
| :::no-loc text="Connection Rate"::: (`connections-per-second`) | Web 服務器的每個更新間隔的連接數目 |
| :::no-loc text="Current Connections"::: (`current-connections`) | 目前與 web 伺服器的作用中連接數目 |
| :::no-loc text="Current TLS Handshakes"::: (`current-tls-handshakes`) | 目前的 TLS 交握數目 |
| :::no-loc text="Current Upgraded Requests (WebSockets)"::: (`current-upgraded-requests`) |  (Websocket 的目前升級的要求數目)  |
| :::no-loc text="Failed TLS Handshakes"::: (`failed-tls-handshakes`) | 失敗的 TLS 交握總數 |
| :::no-loc text="Request Queue Length"::: (`request-queue-length`) | 要求佇列的目前長度 |
| :::no-loc text="TLS Handshake Rate"::: (`tls-handshakes-per-second`) | 每個更新間隔的 TLS 交握數目 |
| :::no-loc text="Total Connections"::: (`total-connections`) | 與網頁伺服器的連接總數 |
| :::no-loc text="Total TLS Handshakes"::: (`total-tls-handshakes`) | 與網頁伺服器之間的 TLS 交握總數 |
