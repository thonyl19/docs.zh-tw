---
title: Attribute (XElement 動態屬性)
ms.date: 10/22/2019
ms.topic: reference
ms.openlocfilehash: 2b645575a5b6876ffbb52a999c4e05a2de037493
ms.sourcegitcommit: 82f94a44ad5c64a399df2a03fa842db308185a76
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/25/2019
ms.locfileid: "72920913"
---
# <a name="attribute-xelement-dynamic-property"></a><span data-ttu-id="f27d2-102">Attribute (XElement 動態屬性)</span><span class="sxs-lookup"><span data-stu-id="f27d2-102">Attribute (XElement dynamic property)</span></span>

<span data-ttu-id="f27d2-103">取得用於擷取對應於指定擴充名稱之屬性執行個體的索引子 (Indexer)。</span><span class="sxs-lookup"><span data-stu-id="f27d2-103">Gets an indexer used to retrieve the attribute instance that corresponds to the specified expanded name.</span></span>

## <a name="syntax"></a><span data-ttu-id="f27d2-104">語法</span><span class="sxs-lookup"><span data-stu-id="f27d2-104">Syntax</span></span>

```xaml
elem.Attribute[{namespaceName}attribName]
```

## <a name="property-valuereturn-value"></a><span data-ttu-id="f27d2-105">屬性值/傳回值</span><span class="sxs-lookup"><span data-stu-id="f27d2-105">Property value/return value</span></span>

<span data-ttu-id="f27d2-106">`XAttribute Item(String expandedName)` 型別的索引子。</span><span class="sxs-lookup"><span data-stu-id="f27d2-106">An indexer of the type `XAttribute Item(String expandedName)`.</span></span> <span data-ttu-id="f27d2-107">如果沒有具有指定之名稱的屬性，這個索引子會採用指定之屬性的擴充名稱，並傳回對應的 <xref:System.Xml.Linq.XAttribute> 或 `null`。</span><span class="sxs-lookup"><span data-stu-id="f27d2-107">This indexer takes the expanded name of the specified attribute and returns the corresponding <xref:System.Xml.Linq.XAttribute>, or `null` if there is no attribute with the specified name.</span></span>

## <a name="remarks"></a><span data-ttu-id="f27d2-108">備註</span><span class="sxs-lookup"><span data-stu-id="f27d2-108">Remarks</span></span>

<span data-ttu-id="f27d2-109">這個屬性等同於 <xref:System.Xml.Linq.XElement.Attribute%2A> 類別的 <xref:System.Xml.Linq.XElement?displayProperty=fullName> 方法。</span><span class="sxs-lookup"><span data-stu-id="f27d2-109">This property is equivalent to the <xref:System.Xml.Linq.XElement.Attribute%2A> method of the <xref:System.Xml.Linq.XElement?displayProperty=fullName> class.</span></span>

## <a name="see-also"></a><span data-ttu-id="f27d2-110">請參閱</span><span class="sxs-lookup"><span data-stu-id="f27d2-110">See also</span></span>

- <xref:System.Xml.Linq.XElement.Attribute%2A?displayProperty=fullName>
- [<span data-ttu-id="f27d2-111">XElement 類別動態屬性</span><span class="sxs-lookup"><span data-stu-id="f27d2-111">XElement Class Dynamic Properties</span></span>](attribute-xelement-dynamic-property.md)
- [<span data-ttu-id="f27d2-112">值</span><span class="sxs-lookup"><span data-stu-id="f27d2-112">Value</span></span>](value-xattribute-dynamic-property.md)