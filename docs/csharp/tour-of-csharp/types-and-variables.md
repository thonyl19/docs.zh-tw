---
title: C# 型別和變數 - C# 語言教學課程
description: 了解如何在 C# 中定義類型和宣告變數
ms.date: 02/25/2020
ms.assetid: f8a8051e-0049-43f1-b594-9c84cc7b1224
ms.openlocfilehash: b2a5255a243c12543a1cd59b5724b6c826306e04
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/14/2020
ms.locfileid: "78159087"
---
# <a name="types-and-variables"></a>型別與變數

C# 中有兩種型別：*實值型別*和*參考型別*。 實值型別的變數直接包含其資料，而參考型別的變數則將參考儲存到其資料，後者即是物件。 對於參考型別，兩個變數可以引用同一物件，因此對一個變數的操作可能會影響另一個變數引用的物件。 對於數值型別，變數各自有自己的資料副本，並且對其中一個變數的操作無法影響另一個變數（和`ref``out`參數變數除外）。

C# 的實值型別可進一步細分為*簡單型別*、*列舉型別*、*結構型別*和*可為 Null 的實值型別*。 C# 的參考型別可進一步細分為*類別型別*、*介面型別*、*陣列型別*和*委派型別*。

以下大綱概述了 C# 的類型系統。

- [實值型別][ValueTypes]
  - [簡單型別][SimpleTypes]
    - 帶正負號的整數︰`sbyte`、`short`、`int`、`long`
    - 不帶正負號的整數︰`byte`、`ushort`、`uint`、`ulong`
    - Unicode 字元：`char`
    - IEEE 二進位浮點：`float`、`double`
    - 高精確度十進位浮點：`decimal`
    - 布林值：`bool`
  - [枚舉類型][EnumTypes]
    - 使用者定義型別，格式為 `enum E {...}`
  - [結構型別][StructTypes]
    - 使用者定義型別，格式為 `struct S {...}`
  - [可為 Null 的實值類型][NullableTypes]
    - 含有 `null` 值的所有其他數值型別的擴充
- [參考型別][ReferenceTypes]
  - [類別類型][ClassTypes]
    - 所有其他型別的基底類別︰`object`
    - Unicode 字串：`string`
    - 使用者定義型別，格式為 `class C {...}`
  - [介面型別][InterfaceTypes]
    - 使用者定義型別，格式為 `interface I {...}`
  - [陣列類型][ArrayTypes]
    - 單一維度和多維度，例如 `int[]` 和 `int[,]`
  - [委派型別][DelegateTypes]
    - 使用者定義型別，格式為 `delegate int D(...)`

[ValueTypes]: ../language-reference/builtin-types/value-types.md
[SimpleTypes]: ../language-reference/builtin-types/value-types.md#built-in-value-types
[EnumTypes]: ../language-reference/builtin-types/enum.md
[StructTypes]: ../language-reference/builtin-types/struct.md
[NullableTypes]: ../language-reference/builtin-types/nullable-value-types.md
[ReferenceTypes]: ../language-reference/keywords/reference-types.md
[ClassTypes]: ../language-reference/keywords/class.md
[InterfaceTypes]: ../language-reference/keywords/interface.md
[DelegateTypes]: ../language-reference/keywords/delegate.md
[ArrayTypes]: ../programming-guide/arrays/index.md

如需數值型別的詳細資訊，請參閱[整數型別](../language-reference/builtin-types/integral-numeric-types.md)和[浮點數型別表](../language-reference/builtin-types/floating-point-numeric-types.md)。

C# 的 `bool` 型別用來代表布林值 — 不是 `true` 或 `false` 的值。

C# 中的字元和字串處理使用 Unicode 編碼方式。 `char` 型別代表 UTF-16 程式碼單位，而 `string` 型別代表一系列的 UTF-16 程式碼單位。

C# 程式使用*型別宣告*來建立新型別。 型別宣告指定新型別的名稱成員。 可由使用者定義的五種 C# 型別類型︰類別型別、結構型別、介面型別、列舉型別及委派型別。

`class` 型別定義資料結構，其中包含資料成員 (欄位) 和函式成員 (方法、屬性及其他)。 類別型別支援單一繼承和多型，這些是可供衍生類別將基底類別延伸及特製化的機制。

`struct` 型別與類別型別相似，它代表具有資料成員和函式成員的結構。 但是，與類不同，結構是數值型別，通常不需要堆分配。 結構類型不支援使用者指定的繼承，並且所有結構類型都隱式從類型`object`繼承。

`interface` 型別將協定定義為一組具名的公用函式成員。 實作 `interface` 的 `class` 或 `struct` 必須提供介面的函式成員的實作。 `interface` 可以繼承自多個基底介面，`class` 或 `struct`可實作多個介面。

`delegate` 型別代表對方法的參考，其中含有特定參數清單與傳回型別。 委派讓您可將方法視為實體，而實體能指派給變數或當作參數來傳遞。 委派類似函式語言提供的函式型別。 它們也類似于在其他一些語言中找到的函數指標的概念。 與函數指標不同，委託是物件導向的和型別安全的。

、 和`delegate`類型所有都支援泛型，因此可以使用其他類型的泛型參數化。 `class` `struct` `interface`

`enum` 型別是包含具名常數的不同型別。 每個 `enum` 型別都具有一個基礎型別，其必須是八種整數型別之一。 `enum` 型別的值組與基礎型別的值組相同。

C# 支援任何型別的單一維度及多維陣列。 與上面列出的類型不同，陣列類型無需在使用之前進行聲明。 而陣列型別的建構方法，是在型別名稱之後加上方括弧。 例如，`int[]` 是 `int` 的單一維度陣列、`int[,]` 是 `int` 的雙維陣列，而 `int[][]` 是 `int` 的單一維度陣列的單一維度陣列。

在使用空數值型別之前，也不必聲明這些類型。 對於每個不可空數值型別`T`，都有一個相應的空數值型別`T?`，它可以保存一個附加值 。 `null` 比方說，`int?` 是可包含任一 32 位元整數或值 `null` 的型別。

C# 的型別系統已整合，任何型別值均可視為一個 `object`。 C# 中的每個型別都直接或間接衍生自 `object` 類別型別，而 `object` 是所有型別的基底類別。 參考型別的值之所以會視為物件，只是將這些值當作 `object` 型別來檢視。 數值型別的值之所以會視為物件，只是透過執行 *boxing* 和 *unboxing* 作業。 在下列範例中，`int` 值會轉換成 `object`，並再次轉換回 `int`。

[!code-csharp[Boxing](../../../samples/snippets/csharp/tour/types-and-variables/Program.cs#L1-L10)]

當實值型別的值轉換成 `object` 類型時，會配置一個 `object` 執行個體 (亦稱為「Box」) 以包含值，而值會複製到該方塊。 相反地，當 `object` 參考轉換為實值型別時，會檢查參考的 `object` 是否為數值型別正確的方塊，如果檢查成功，則會將方塊中的複製出來。

C# 的整合類型系統實際上表示實值型別可以「依需求」變成物件。 因為整合，使用 `object` 型別的一般用途程式庫就能搭配參考型別和實值型別使用。

C# 中有數種*變數*，包括欄位、陣列元素、區域變數和參數。 變數表示儲存位置，每個變數都有一種型別，其決定哪些值可以儲存在變數中，如下所示。

- 不可為 Null 的實值型別
  - 該型別的值
- 可為 Null 的實值型別
  - `null` 值或該型別的值
- 物件 (object)
  - `null` 參考、任一參考型別之物件的參考，或是任一實值型別的 Boxed 值的參考
- 類別型別
  - `null` 參考、該類別型別之執行個體的參考，或衍生自該類別型別之類別執行個體的參考
- 介面類型
  - `null` 參考、實作該介面型別之類別型別的執行個體的參考，或實作該介面型別之實值型別的 Boxed 值的參考
- 陣列型別
  - `null` 參考、該陣列型別之執行個體的參考，或相容的陣列型別之執行個體的參考
- 委派類型
  - `null` 參考或相容的委派類型之執行個體的參考

> [!div class="step-by-step"]
> [上一個](program-structure.md)
> [下一個](expressions.md)
