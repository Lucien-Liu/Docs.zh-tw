---
title: 作法：列舉電腦上既有的時區
ms.date: 04/10/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- time zones [.NET], enumerating
- enumerating time zones [.NET]
ms.assetid: bb7a42ab-6bd9-4c5c-b734-5546d51f8669
ms.openlocfilehash: 276c13bb95685e9588e25238f1a6e45cd57a6c91
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/18/2020
ms.locfileid: "94817961"
---
# <a name="how-to-enumerate-time-zones-present-on-a-computer"></a>作法：列舉電腦上既有的時區

若要順利處理指定的時區，需要可供系統使用之時區的相關資訊。 Windows XP 和 Windows Vista 作業系統會將這項資訊儲存在登錄中。 不過，雖然全世界的時區總數很大，但是登錄只會包含其中一部分的資訊。 此外，登錄本身是內容很容易變成故意或意外變更的動態結構。 因此，應用程式無法永遠假設特定時區已定義且可在系統上使用。 許多使用時區資訊應用程式之應用程式的第一個步驟，為是否可在本機系統上使用必要時區，或將可從中選取的時區清單提供給使用者。 這需要應用程式列舉本機系統上所定義的時區。

> [!NOTE]
> 如果應用程式依賴可能無法在本機系統上定義的特定時區，應用程式可以將時區的相關資訊序列化和還原序列化，以確保其存在。 然後，您可以將時區新增至清單控制項，讓應用程式使用者可以選取它。 如需詳細資訊，請參閱 [如何：將時區儲存到內嵌資源](save-time-zones-to-an-embedded-resource.md) 和 [如何：從內嵌資源還原時區](restore-time-zones-from-an-embedded-resource.md)。

### <a name="to-enumerate-the-time-zones-present-on-the-local-system"></a>列舉本機系統上展示的時區

1. 呼叫 <xref:System.TimeZoneInfo.GetSystemTimeZones%2A?displayProperty=nameWithType> 方法。 方法會傳回物件的泛型 <xref:System.Collections.ObjectModel.ReadOnlyCollection%601> 集合 <xref:System.TimeZoneInfo> 。 集合中的專案會依其屬性排序 <xref:System.TimeZoneInfo.DisplayName%2A> 。 例如：

   [!code-csharp[System.TimeZone2.Concepts#1](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.TimeZone2.Concepts/CS/TimeZone2Concepts.cs#1)]
   [!code-vb[System.TimeZone2.Concepts#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.TimeZone2.Concepts/VB/TimeZone2Concepts.vb#1)]

2. <xref:System.TimeZoneInfo>使用 c # 中的迴圈 (來列舉集合中的個別物件 `foreach` ) 或 ... `For Each``Next` 迴圈 (Visual Basic) ，並在每個物件上執行任何必要的處理。 例如，下列程式碼會列舉 <xref:System.Collections.ObjectModel.ReadOnlyCollection%601> <xref:System.TimeZoneInfo> 步驟1中傳回的物件集合，並列出主控台上每個時區的顯示名稱。

   [!code-csharp[System.TimeZone2.Concepts#12](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.TimeZone2.Concepts/CS/TimeZone2Concepts.cs#12)]
   [!code-vb[System.TimeZone2.Concepts#12](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.TimeZone2.Concepts/VB/TimeZone2Concepts.vb#12)]

### <a name="to-present-the-user-with-a-list-of-time-zones-present-on-the-local-system"></a>向使用者顯示本機系統上的時區清單

1. 呼叫 <xref:System.TimeZoneInfo.GetSystemTimeZones%2A?displayProperty=nameWithType> 方法。 方法會傳回物件的泛型 <xref:System.Collections.ObjectModel.ReadOnlyCollection%601> 集合 <xref:System.TimeZoneInfo> 。

2. 將步驟1中傳回的集合指派給 `DataSource` Windows forms 或 ASP.NET 清單控制項的屬性。

3. 取出 <xref:System.TimeZoneInfo> 使用者已選取的物件。

此範例提供 Windows 應用程式的圖例。

## <a name="example"></a>範例

此範例會啟動 Windows 應用程式，以在清單方塊中顯示系統上定義的時區。 然後，此範例會顯示一個對話方塊，其中包含 <xref:System.TimeZoneInfo.DisplayName%2A> 使用者所選取之時區物件的屬性值。

[!code-csharp[System.TimeZone2.Concepts#2](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.TimeZone2.Concepts/CS/TimeZone2Concepts.cs#2)]
[!code-vb[System.TimeZone2.Concepts#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.TimeZone2.Concepts/VB/TimeZone2Concepts.vb#2)]

大部分的清單控制項 (例如 <xref:System.Windows.Forms.ListBox?displayProperty=nameWithType> 或 <xref:System.Web.UI.WebControls.BulletedList?displayProperty=nameWithType> 控制項) 可讓您將物件變數集合指派給其屬性，只要 `DataSource` 該集合會實作為介面即可 <xref:System.Collections.IEnumerable> 。  (泛型 <xref:System.Collections.ObjectModel.ReadOnlyCollection%601> 類別會執行這項工作。 ) 若要在集合中顯示個別物件，控制項會呼叫該物件的 `ToString` 方法，以解壓縮用來表示物件的字串。 在物件的情況下 <xref:System.TimeZoneInfo> ，方法會傳回 `ToString` 物件的 <xref:System.TimeZoneInfo> 顯示名稱 (其屬性的值 <xref:System.TimeZoneInfo.DisplayName%2A>) 。

> [!NOTE]
> 由於清單控制項會呼叫物件的 `ToString` 方法，因此您可以將物件集合指派 <xref:System.TimeZoneInfo> 給控制項、讓控制項顯示每個物件的有意義名稱，以及取得 <xref:System.TimeZoneInfo> 使用者選取的物件。 這樣就不需要針對集合中的每個物件解壓縮字串，而是將字串指派給集合，然後再指派給控制項的 `DataSource` 屬性，取出使用者已選取的字串，然後使用這個字串來解壓縮其所描述的物件。

## <a name="compiling-the-code"></a>編譯程式碼

這個範例需要：

- 匯入下列命名空間：

  <xref:System> C # 程式碼中的 () 

  <xref:System.Collections.ObjectModel>

## <a name="see-also"></a>請參閱

- [日期、時間和時區](index.md)
- [如何：將時區儲存到內嵌資源](save-time-zones-to-an-embedded-resource.md)
- [如何：從內嵌資源還原時區](restore-time-zones-from-an-embedded-resource.md)
