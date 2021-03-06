---
title: 原托布夫保留字段 - gRPC 面向 WCF 開發人員
description: 瞭解跨版本相容性的保留字段。
ms.date: 09/09/2019
ms.openlocfilehash: bde658c671e970b7ec841d71d5b4284eb91195f0
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/12/2020
ms.locfileid: "79147941"
---
# <a name="protobuf-reserved-fields"></a>Protobuf 保留欄位

協定緩衝區 （Protobuf） 中的向後相容性保證依賴于始終表示相同資料項目的欄位編號。 如果從服務新版本中的消息中刪除了欄位，則不應重複使用該欄位編號。 您可以使用`reserved`關鍵字來強化此值。

如果`displayName`從`marketId`前面定義`Stock`的消息中刪除 和 欄位，則應保留其欄位編號，如以下示例所示。

```protobuf
syntax "proto3";

message Stock {

    reserved 3, 4;
    int32 id = 1;
    string symbol = 2;

}
```

您還可以將`reserved`關鍵字用作將來可能添加的欄位的預留位置。 您可以使用`to`關鍵字將連續欄位數表示為範圍。

```protobuf
syntax "proto3";

message Info {

    reserved 2, 9 to 11, 15;
    // ...
}
```

>[!div class="step-by-step"]
>[上一個](protobuf-repeated.md)
>[下一個](protobuf-any-oneof.md)
