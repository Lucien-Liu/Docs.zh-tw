---
title: 如何尋找根項目-LINQ to XML
description: '瞭解如何在 c # 和 Visual Basic 中使用 XPath 和 LINQ to XML，以尋找 XML 檔的根項目。'
ms.date: 07/20/2015
dev_langs:
- csharp
- vb
ms.assetid: 4fd824e0-4d39-429b-b092-f6a5c046ee6c
ms.openlocfilehash: 89247c19fc31add4e95f11742772cef1bdd2ce63
ms.sourcegitcommit: aa6d8a90a4f5d8fe0f6e967980b8c98433f05a44
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/16/2020
ms.locfileid: "90679463"
---
# <a name="how-to-find-the-root-element-linq-to-xml"></a>如何尋找 (LINQ to XML 的根項目) 

本文提供的範例示範如何使用 XPath 和 LINQ to XML （在 c # 和 Visual Basic 中）來尋找 XML 檔的根項目。

## <a name="example-find-the-root-element"></a>範例：尋找根項目

這個範例會使用 LINQ to XML query 和 XPath 來尋找 XML 檔範例 XML 檔中的根項目 [：多個訂單](sample-xml-file-multiple-purchase-orders.md)。 XPath 運算式為 `/PurchaseOrders`。

```csharp
XDocument po = XDocument.Load("PurchaseOrders.xml");

// LINQ to XML query
XElement el1 = po.Root;

// XPath expression
XElement el2 = po.XPathSelectElement("/PurchaseOrders");

if (el1 == el2)
    Console.WriteLine("Results are identical");
else
    Console.WriteLine("Results differ");
Console.WriteLine(el1.Name);
```

```vb
Dim po As XDocument = XDocument.Load("PurchaseOrders.xml")

' LINQ to XML query
Dim el1 As XElement = po.Root

' XPath expression
Dim el2 As XElement = po.XPathSelectElement("/PurchaseOrders")

If el1 Is el2 Then
    Console.WriteLine("Results are identical")
Else
    Console.WriteLine("Results differ")
End If
Console.WriteLine(el1.Name)
```

這個範例會產生下列輸出：

```output
Results are identical
PurchaseOrders
```

## <a name="see-also"></a>另請參閱

- [XPath 和 LINQ to XML 的比較](comparison-xpath-linq-xml.md)
