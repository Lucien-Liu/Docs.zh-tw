---
title: 使用接續工作來連結工作
description: 瞭解如何使用 .NET 中的接續工作來連鎖工作。 接續工作是另一項工作所叫用的非同步工作。
ms.date: 07/20/2020
dev_langs:
- csharp
- vb
helpviewer_keywords:
- tasks, continuations
ms.assetid: 0b45e9a2-de28-46ce-8212-1817280ed42d
ms.openlocfilehash: c7afbc1bb22a1743aed8e4ebd0a06c4f3fb57e86
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/18/2020
ms.locfileid: "94830606"
---
# <a name="chaining-tasks-using-continuation-tasks"></a>使用接續工作來連結工作

在非同步程式設計中，一個非同步作業（完成時）通常會叫用第二個作業。 接續可讓 decedent 作業取用第一項作業的結果。 傳統上，會使用回呼方法完成接續。 在工作平行程式庫中，由 _「接續工作」_(continuation task) 提供相同的功能。 接續工作 (也稱為接續) 是由另一項工作所叫用的非同步工作，也就是在上一次完成時由另一 _項工作所_ 叫用。

接續相對容易使用，但卻非常強大且有彈性。 例如，您可以：

- 將資料從前項傳遞至接續。
- 指定叫用或不叫用接續的精確條件。
- 在接續開始之前或是在以合作方式執行時取消接續。
- 提供有關該如何排定接續的提示。
- 從相同前項叫用多個接續。
- 當所有或多個前項之任何一項完成時，請叫用一個接續。
- 鏈結接續可一個接著一個直到任意長度。
- 使用接續處理前項所擲回的例外狀況。

## <a name="about-continuations"></a>關於接續

接續是在 <xref:System.Threading.Tasks.TaskStatus.WaitingForActivation> 狀態建立的工作。 當其前項工作完成時，它便會自動啟動。 在使用者程式碼中的接續呼叫 <xref:System.Threading.Tasks.Task.Start%2A?displayProperty=nameWithType> 會擲回 <xref:System.InvalidOperationException?displayProperty=nameWithType> 例外狀況。

接續本身是 <xref:System.Threading.Tasks.Task> ，啟動時並不會封鎖執行緒。 呼叫 <xref:System.Threading.Tasks.Task.Wait%2A?displayProperty=nameWithType> 方法來封鎖，直到完成接續工作。

## <a name="create-a-continuation-for-a-single-antecedent"></a>針對單一 antecedent 建立接續

您可建立當前項藉由呼叫 <xref:System.Threading.Tasks.Task.ContinueWith%2A?displayProperty=nameWithType> 方法完成時執行的接續。 下列範例顯示其基本模式 (為求清楚明瞭，省略例外狀況處理)。 它會執行前項工作 `taskA`，這會傳回表示今天是星期幾之名稱的 <xref:System.DayOfWeek> 物件。 當前項完成時，接續工作 `continuation` 由其前項傳遞，並顯示包含結果的字串。

> [!NOTE]
> 此發行項中的 C# 範例使用 `Main` 方法上的 `async` 修飾詞。 C# 7.1 及更新版本有提供該功能。 [`CS5001`](../../csharp/misc/cs5001.md)編譯此範例程式碼時，會產生先前的版本。 您必須將語言版本設定為 C# 7.1 或更新版本。 如需了解如何設定發行項中的語言版本，請參閱[設定語言版本](../../csharp/language-reference/configure-language-version.md)。

:::code language="csharp" source="snippets/cs/simple1.cs":::

[!code-vb[TPL_Continuations#1](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_continuations/vb/simple1.vb#1)]

## <a name="create-a-continuation-for-multiple-antecedents"></a>建立多個前項的接續

您也可以建立會在任何或所有群組工作完成時執行的接續。 當所有前項工作都完成後，若要執行接續，您可以呼叫靜態`Shared` 方法 (在 Visual Basic 中為 <xref:System.Threading.Tasks.Task.WhenAll%2A?displayProperty=nameWithType> ) 或執行個體 <xref:System.Threading.Tasks.TaskFactory.ContinueWhenAll%2A?displayProperty=nameWithType> 方法。 當任何前項工作完成後，若要執行接續，您可以呼叫靜態`Shared` 方法 (在 Visual Basic 中為 <xref:System.Threading.Tasks.Task.WhenAny%2A?displayProperty=nameWithType> ) 或執行個體 <xref:System.Threading.Tasks.TaskFactory.ContinueWhenAny%2A?displayProperty=nameWithType> 方法。

和多載的呼叫 <xref:System.Threading.Tasks.Task.WhenAll%2A?displayProperty=nameWithType> <xref:System.Threading.Tasks.Task.WhenAny%2A?displayProperty=nameWithType> 不會封鎖呼叫執行緒。 不過，您通常會呼叫所有的 <xref:System.Threading.Tasks.Task.WhenAll%28System.Collections.Generic.IEnumerable%7BSystem.Threading.Tasks.Task%7D%29?displayProperty=nameWithType> 和 <xref:System.Threading.Tasks.Task.WhenAll%28System.Threading.Tasks.Task%5B%5D%29?displayProperty=nameWithType> 方法，以取出傳回的 <xref:System.Threading.Tasks.Task%601.Result%2A?displayProperty=nameWithType> 屬性，而此屬性會封鎖呼叫執行緒。

下列範例會呼叫 <xref:System.Threading.Tasks.Task.WhenAll%28System.Collections.Generic.IEnumerable%7BSystem.Threading.Tasks.Task%7D%29?displayProperty=nameWithType> 方法來建立接續工作，這會反映其 10 個前項工作的結果。 每個前項工作計算介於 1 到 10 之間的索引值平方。 如果順利完成前項 (其 <xref:System.Threading.Tasks.Task.Status%2A?displayProperty=nameWithType> 屬性是 <xref:System.Threading.Tasks.TaskStatus.RanToCompletion?displayProperty=nameWithType>)，則接續的 <xref:System.Threading.Tasks.Task%601.Result%2A?displayProperty=nameWithType> 屬性是每個前項所傳回的 <xref:System.Threading.Tasks.Task%601.Result%2A?displayProperty=nameWithType> 值之陣列。 此範例會加入它們，以計算介於 1 到 10 之間所有數字的平方和。

:::code language="csharp" source="snippets/cs/whenall1.cs":::

[!code-vb[TPL_Continuations#5](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_continuations/vb/whenall1.vb#5)]

## <a name="continuation-options"></a>接續選項

當您建立單一工作接續時，您可以使用 <xref:System.Threading.Tasks.Task.ContinueWith%2A> 多載，這會採用 <xref:System.Threading.Tasks.TaskContinuationOptions?displayProperty=nameWithType> 列舉值，以指定啟動接續的條件。 例如，您可以指定只有當前項已順利完成，或只有當它在發生錯誤的狀態下完成時，才執行接續。 當前項已準備好叫用接續時，若此條件不成立，則此接續會直接轉換成 <xref:System.Threading.Tasks.TaskStatus.Canceled?displayProperty=nameWithType> 狀態，而且無法啟動。

一些多工接續方法，例如 <xref:System.Threading.Tasks.TaskFactory.ContinueWhenAll%2A?displayProperty=nameWithType> 方法的多載，也包含 <xref:System.Threading.Tasks.TaskContinuationOptions?displayProperty=nameWithType> 參數。 不過，只有所有 <xref:System.Threading.Tasks.TaskContinuationOptions?displayProperty=nameWithType> 列舉類型成員的子集有效。 您可以指定 <xref:System.Threading.Tasks.TaskContinuationOptions?displayProperty=nameWithType> 值，該值具有 <xref:System.Threading.Tasks.TaskCreationOptions?displayProperty=nameWithType> 列舉程式中的對應項目，例如 <xref:System.Threading.Tasks.TaskContinuationOptions.AttachedToParent?displayProperty=nameWithType>、 <xref:System.Threading.Tasks.TaskContinuationOptions.LongRunning?displayProperty=nameWithType>和 <xref:System.Threading.Tasks.TaskContinuationOptions.PreferFairness?displayProperty=nameWithType>。 如果您指定具有多工接續的任何 `NotOn` 或 `OnlyOn` 選項，則會在執行階段擲回 <xref:System.ArgumentOutOfRangeException> 例外狀況。

如需有關工作接續選項的詳細資訊，請參閱 <xref:System.Threading.Tasks.TaskContinuationOptions> 主題。

## <a name="pass-data-to-a-continuation"></a>將資料傳遞至接續

<xref:System.Threading.Tasks.Task.ContinueWith%2A?displayProperty=nameWithType> 方法將做為引數傳遞前項的參考至接續的使用者委派。 如果前項是 <xref:System.Threading.Tasks.Task%601?displayProperty=nameWithType> 物件，且工作執行直到它已完成，則接續可以存取 <xref:System.Threading.Tasks.Task%601.Result%2A?displayProperty=nameWithType> 工作的屬性。

<xref:System.Threading.Tasks.Task%601.Result%2A?displayProperty=nameWithType> 屬性會封鎖，直到完成工作。 不過，如果工作已取消，或發生錯誤，則嘗試存取 <xref:System.Threading.Tasks.Task%601.Result%2A> 屬性則會擲回 <xref:System.AggregateException> 例外狀況。 請使用 <xref:System.Threading.Tasks.TaskContinuationOptions.OnlyOnRanToCompletion> 選項來避免這個問題，如下列範例所示。

:::code language="csharp" source="snippets/cs/result1.cs":::

[!code-vb[TPL_Continuations#2](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_continuations/vb/result1.vb#2)]

如果您想執行接續，即使前項並未執行到順利完成，您也必須防範例外狀況。 其中一個方法是測試前項的 <xref:System.Threading.Tasks.Task.Status%2A?displayProperty=nameWithType> 屬性，而且只嘗試存取 <xref:System.Threading.Tasks.Task%601.Result%2A> 屬性 (若狀態不是 <xref:System.Threading.Tasks.TaskStatus.Faulted> 或 <xref:System.Threading.Tasks.TaskStatus.Canceled>)。 您也可以檢查前項的 <xref:System.Threading.Tasks.Task.Exception%2A> 屬性。 如需詳細資訊，請參閱[例外狀況處理](exception-handling-task-parallel-library.md)。 下列範例會修改上一個範例來存取前項的 <xref:System.Threading.Tasks.Task%601.Result%2A?displayProperty=nameWithType> 屬性 (僅在當其狀態為 <xref:System.Threading.Tasks.TaskStatus.RanToCompletion?displayProperty=nameWithType>之時)。

:::code language="csharp" source="snippets/cs/result2.cs":::

[!code-vb[TPL_Continuations#7](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_continuations/vb/result2.vb#7)]

## <a name="cancel-a-continuation"></a>取消接續

在下列情況中，接續的 <xref:System.Threading.Tasks.Task.Status%2A?displayProperty=nameWithType> 屬性會被設定為 <xref:System.Threading.Tasks.TaskStatus.Canceled?displayProperty=nameWithType> ：

- 它會擲回 <xref:System.OperationCanceledException> 例外狀況以回應取消要求。 如同任何工作，如果例外狀況包含傳遞給接續的相同標記，則會將它視為合作式取消的認可。
- 接續傳遞 <xref:System.Threading.CancellationToken?displayProperty=nameWithType> ，其 <xref:System.Threading.CancellationToken.IsCancellationRequested%2A> 屬性是 `true`。 在此案例中，接續未啟動，而且會轉換為 <xref:System.Threading.Tasks.TaskStatus.Canceled?displayProperty=nameWithType> 狀態。
- 此接續絕不會執行，因為不符合 <xref:System.Threading.Tasks.TaskContinuationOptions> 引數設定的條件。 例如，如果前項進入 <xref:System.Threading.Tasks.TaskStatus.Faulted?displayProperty=nameWithType> 狀態，則不會執行其已傳遞 <xref:System.Threading.Tasks.TaskContinuationOptions.NotOnFaulted?displayProperty=nameWithType> 選項的接續，但會轉換成 <xref:System.Threading.Tasks.TaskStatus.Canceled> 狀態。

如果工作及其接續代表相同的邏輯作業的兩個部分，則您可傳遞相同的取消語彙基元給這兩項工作，如下列範例所示。 這包含會產生可由 33 整除的整數清單之前項，這會傳遞給接續。 接續會輪流顯示此清單。 前項和接續會定期暫停隨機一段時間。 此外，在 5 秒的逾時時間間隔後， <xref:System.Threading.Timer?displayProperty=nameWithType> 物件會用來執行 `Elapsed` 方法。 此範例會呼叫 <xref:System.Threading.CancellationTokenSource.Cancel%2A?displayProperty=nameWithType> 方法，導致目前正在執行的工作去呼叫 <xref:System.Threading.CancellationToken.ThrowIfCancellationRequested%2A?displayProperty=nameWithType> 方法。 當前項或其接續執行時，是否呼叫 <xref:System.Threading.CancellationTokenSource.Cancel%2A?displayProperty=nameWithType> 方法取決於隨機產生的暫停持續期間。 如果取消前項，將不會啟動接續。 如果未取消前項，則語彙基元仍可用於取消接續。

:::code language="csharp" source="snippets/cs/cancellation1.cs":::

[!code-vb[TPL_Continuations#3](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_continuations/vb/cancellation1.vb#3)]

若在不需要提供接續的取消語彙基元之情況下取消前項，則當您建立接續時，也可藉由指定 <xref:System.Threading.Tasks.TaskContinuationOptions.NotOnCanceled?displayProperty=nameWithType> 選項來防止接續執行。 以下是簡單的範例。

:::code language="csharp" source="snippets/cs/cancellation2.cs":::

[!code-vb[TPL_Continuations#8](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_continuations/vb/cancellation2.vb#8)]

在接續進入 <xref:System.Threading.Tasks.TaskStatus.Canceled> 狀態之後，它可能會影響後續的接續，這取決於為了接續而指定的 <xref:System.Threading.Tasks.TaskContinuationOptions> 。

已處置的接續將不會啟動。

## <a name="continuations-and-child-tasks"></a>接續和子工作

直到接續和所有附加的子工作完成後，前項才會執行。 接續不會等候中斷連結的子工作完成。 下列兩個範例說明會附加和卸離建立接續之前項的子工作。 在下列範例中，僅在所有子工作都已完成後，才會執行接續。且每次執行此範例多次都會產生相同的輸出。 因為根據預設，<xref:System.Threading.Tasks.Task.Run%2A?displayProperty=nameWithType> 方法會建立父工作，其預設工作建立選項是 <xref:System.Threading.Tasks.TaskCreationOptions.DenyChildAttach?displayProperty=nameWithType>，所以此範例會藉由呼叫 <xref:System.Threading.Tasks.TaskFactory.StartNew%2A?displayProperty=nameWithType> 方法來啟動前項。

:::code language="csharp" source="snippets/cs/attached1.cs":::

[!code-vb[TPL_Continuations#9](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_continuations/vb/attached1.vb#9)]

如果子工作從前項中斷，但只要前項已經終止，接續就會執行，不論子工作的狀態為何。 因此，下列範例中的多個回合會產生變數輸出，這些會取決於工作排程器如何處理每項子工作。

:::code language="csharp" source="snippets/cs/detached1.cs":::

[!code-vb[TPL_Continuations#10](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_continuations/vb/detached1.vb#10)]

前項工作的最終狀態取決於任何附加子工作的最終狀態。 中斷連結的子工作狀態不會影響父代。 如需詳細資訊，請參閱[附加與中斷連結的子工作](attached-and-detached-child-tasks.md)。

## <a name="associate-state-with-continuations"></a>將狀態與接續關聯

您可以將任意的狀態與工作接續相關聯。 <xref:System.Threading.Tasks.Task.ContinueWith%2A> 方法提供多載的版本，每個都會擷取代表接續狀態的 <xref:System.Object> 值。 稍後您可以使用 <xref:System.Threading.Tasks.Task.AsyncState%2A?displayProperty=nameWithType> 屬性來存取這個狀態物件。 如果您未提供值，則此狀態物件為 `null` 。

當您為使用 TPL 而轉換現有之使用 [非同步程式設計模型 (APM)](../asynchronous-programming-patterns/asynchronous-programming-model-apm.md) 的程式碼時，接續狀態會很有用。 在 APM 中，您通常會在 **Begin**_Method_ 方法中提供物件狀態，並在稍後使用 <xref:System.IAsyncResult.AsyncState%2A?displayProperty=nameWithType> 屬性存取該狀態。 藉由使用 <xref:System.Threading.Tasks.Task.ContinueWith%2A> 方法，當您轉換使用 APM 的程式碼為使用 TPL 時，您可以保留此狀態。

當您使用 Visual Studio 偵錯工具中的 <xref:System.Threading.Tasks.Task> 物件時，接續狀態也相當有用。 例如，在 [平行工作]  視窗中，[工作]  資料行會顯示每項工作的狀態物件之字串表示。 如需 [平行工作] 視窗的詳細資訊，請參閱[使用工作視窗](/visualstudio/debugger/using-the-tasks-window)。

下列範例示範如何使用接續狀態。 它會建立接續工作的鏈結。 每個工作會為 <xref:System.DateTime> 方法的 `state` 參數提供目前的時間，此為 <xref:System.Threading.Tasks.Task.ContinueWith%2A> 物件。 每個 <xref:System.DateTime> 物件代表建立接續工作時的時間。 做為其結果，每一項工作會產生第二個 <xref:System.DateTime> 物件，表示工作完成時的時間。 所有工作都完成之後，此範例會顯示建立時間，以及顯示每個接續工作完成的時間。

:::code language="csharp" source="snippets/cs/continuationstate.cs":::

[!code-vb[TPL_ContinuationState#1](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_continuationstate/vb/continuationstate.vb#1)]

## <a name="continuations-that-return-task-types"></a>傳回工作類型的接續

有時您可能需要鏈出會傳回型別的接續 <xref:System.Threading.Tasks.Task> 。 這些都稱為「嵌套工作」（nested task），而且很常見。 當父工作呼叫時 <xref:System.Threading.Tasks.Task%601.ContinueWith%2A?displayProperty=nameWithType> ，會提供一個工作，讓 `continuationFunction` 您呼叫 <xref:System.Threading.Tasks.TaskExtensions.Unwrap%2A> 來建立代表 `<Task<Task<T>>>` 或 `Task(Of Task(Of T))` (Visual Basic) 之非同步作業的 proxy 工作。

下列範例會示範如何使用接續來包裝額外的工作傳回函式。 每個接續都可以解除包裝，以公開已包裝的內部工作。

:::code language="csharp" source="snippets/cs/unwrap.cs":::
:::code language="vb" source="snippets/vb/unwrap.vb":::

如需使用的詳細資訊 <xref:System.Threading.Tasks.TaskExtensions.Unwrap%2A> ，請參閱 [如何：解除包裝嵌套的工作](how-to-unwrap-a-nested-task.md)。

## <a name="handle-exceptions-thrown-from-continuations"></a>處理延續擲回的例外狀況

前項與接續的關聯性並非父子式關聯性。 接續擲回的例外狀況並不會傳播至前項。 因此，就如同您在其他工作中一樣地處理接續所擲回的例外狀況，如下所示：

- 您可以使用 <xref:System.Threading.Tasks.Task.Wait%2A>、 <xref:System.Threading.Tasks.Task.WaitAll%2A>或 <xref:System.Threading.Tasks.Task.WaitAny%2A> 方法或是其泛型對應項目，用來等待接續。 您可以在相同的 `try` 陳述式中等待前項和其接續，如下列範例所示。

:::code language="csharp" source="snippets/cs/exception1.cs":::

[!code-vb[TPL_Continuations#6](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_continuations/vb/exception1.vb#6)]

- 您可以使用第二個接續來觀察第一個接續的 <xref:System.Threading.Tasks.Task.Exception%2A> 屬性。 在下列範例中，工作會嘗試讀取不存在的檔案。 然後接續會顯示有關前項工作中例外狀況的資訊。

:::code language="csharp" source="snippets/cs/exception2.cs" id="example":::

[!code-vb[TPL_Continuations#4](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_continuations/vb/exception2.vb#4)]

因為它以 <xref:System.Threading.Tasks.TaskContinuationOptions.OnlyOnFaulted?displayProperty=nameWithType> 選項執行，所以僅當前項中發生例外狀況時，接續才會執行，也因此它可能會假設前項的 <xref:System.Threading.Tasks.Task.Exception%2A> 屬性不是 `null`。 如果不論前項中是否擲回例外狀況都執行接續，則可能會檢查是否前項的 <xref:System.Threading.Tasks.Task.Exception%2A> 屬性不是 `null` ，之後才嘗試處理例外狀況，如下列程式碼片段所示。

:::code language="csharp" source="snippets/cs/exception2.cs" id="exception":::

[!code-vb[TPL_Continuations#11](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_continuations/vb/exception2.vb#11)]

如需詳細資訊，請參閱[例外狀況處理](exception-handling-task-parallel-library.md)。

- 如果接續是使用 <xref:System.Threading.Tasks.TaskContinuationOptions.AttachedToParent?displayProperty=nameWithType> 選項建立的附加子工作，則其父代會將例外狀況傳播回到呼叫的執行緒，如同任何附加之子系的情況。 如需詳細資訊，請參閱[附加與中斷連結的子工作](attached-and-detached-child-tasks.md)。

## <a name="see-also"></a>請參閱

- [工作平行程式庫 (TPL)](task-parallel-library-tpl.md)
