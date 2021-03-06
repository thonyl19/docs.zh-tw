---
title: 外部別名 - C# 參考
ms.date: 07/20/2015
f1_keywords:
- alias_CSharpKeyword
helpviewer_keywords:
- extern alias keyword [C#]
- aliases [C#], extern keyword
- aliases, extern keyword
ms.assetid: f487bf4f-c943-4fca-851b-e540c83d9027
ms.openlocfilehash: 86202333484933d7449b0c4d8c5a3f1a63cd7775
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/14/2020
ms.locfileid: "75713553"
---
# <a name="extern-alias-c-reference"></a>外部別名 (C# 參考)
您可能必須參考兩個具有相同完整類型名稱的組件版本。 例如，您可能必須在相同的應用程式中使用兩個或多個組件版本。 藉由使用外部組件別名，來自每個組件的命名空間可包裝在別名所命名的根層級命名空間內，這樣即可讓它們在相同的檔案中使用。  
  
> [!NOTE]
> [extern](./extern.md) 關鍵字也可作為方法修飾詞，用於宣告以 Unmanaged 程式碼撰寫的方法。  
  
 若要參考兩個具有相同完整類型名稱的組件，則必須在命令提示字元中指定別名，如下所示：  
  
 `/r:GridV1=grid.dll`  
  
 `/r:GridV2=grid20.dll`  
  
 這會建立外部別名 `GridV1` 和 `GridV2`。 若要從程式內使用這些別名，請使用 `extern` 關鍵字參考別名。 例如：  
  
 `extern alias GridV1;`  
  
 `extern alias GridV2;`  
  
 每個外部別名宣告會引進另一個與全域命名空間平行 (但不在其內) 的根層級命名空間。 因此，來自每個組件的類型可使用其完整名稱 (源自適當的命名空間別名) 來參考，而不會有模擬兩可的情況。  
  
 在上述範例中，`GridV1::Grid` 是來自 `grid.dll` 的方格控制項，而 `GridV2::Grid` 是來自 `grid20.dll` 的方格控制項。  
  
## <a name="c-language-specification"></a>C# 語言規格  
 [!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]  
  
## <a name="see-also"></a>另請參閱

- [C# 參考](../index.md)
- [C# 程式設計指南](../../programming-guide/index.md)
- [C# 關鍵字](./index.md)
- [* 操作員](../operators/namespace-alias-qualifier.md)
- [-引用（C# 編譯器選項）](../compiler-options/reference-compiler-option.md)
