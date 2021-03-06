---
title: CLR 預存程序
ms.date: 03/30/2017
ms.assetid: fd7eea9b-218a-4988-8c9a-8abcc6031c66
ms.openlocfilehash: ca0fd2d33f0ad56c96fb63530e6ba3f7f7890f81
ms.sourcegitcommit: 700ea803fb06c5ce98de017c7f76463ba33ff4a9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/19/2020
ms.locfileid: "77450358"
---
# <a name="clr-stored-procedures"></a>CLR 預存程序
預存程序是無法在純量運算式中使用的常式。 它們可將表格式結果及訊息傳回到用戶端、叫用資料定義語言 (DDL) 及資料操作語言 (DML) 陳述式，並傳回輸出參數。  
  
> [!NOTE]
> Microsoft Visual Basic 支援輸出參數的方式，與 Microsoft Visual C# 所使用的方式不同。 您必須指定以傳址方式傳遞參數，並套用 \<Out （） > 屬性，以表示輸出參數，如下所示：  
  
```vb
Public Shared Sub ExecuteToClient( <Out()> ByRef number As Integer)  
```
  
如需詳細資訊，請參閱您所使用之 SQL Server 版本的[SQL Server 檔](/sql)版本。
  
 **SQL Server 文件**

1. [CLR 預存程序](https://docs.microsoft.com/previous-versions/sql/sql-server-2008/ms131094(v=sql.100))  
  
## <a name="see-also"></a>另請參閱

- [在 Managed 程式碼中建立 SQL Server 2005 物件](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2008/6s0s2at1(v=vs.90))
- [ADO.NET 概觀](../ado-net-overview.md)
