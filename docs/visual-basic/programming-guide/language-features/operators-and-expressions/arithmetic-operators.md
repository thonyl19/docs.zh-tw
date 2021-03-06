---
title: 算術運算子
ms.date: 07/20/2015
helpviewer_keywords:
- type safety
- operators [Visual Basic], bitwise
- operators [Visual Basic], bit-shift
- bitwise operators [Visual Basic]
- bit-shift operators [Visual Basic]
- zero, division by zero
- operators [Visual Basic], arithmetic
- division [Visual Basic], by zero
- Visual Basic code, operators
- arithmetic operators [Visual Basic], about arithmetic operators
ms.assetid: 325dac7a-ea4f-41d5-8b48-f6e904211569
ms.openlocfilehash: 8ded8d7111bd37cf8762a202b728e814aa5a082b
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/22/2019
ms.locfileid: "74352663"
---
# <a name="arithmetic-operators-in-visual-basic"></a>Visual Basic 的算術運算子
算術運算子是用來執行許多常見的算數運算，其中包含常值、變數、其他運算式、函數和屬性呼叫和常數所表示的數值計算。 同時以算術運算子分類是位移位運算子，其作用於運算元的個別位層級，並將其位模式移至左側或右側。  
  
## <a name="arithmetic-operations"></a>算數運算  
 您可以使用[+ 運算子](../../../../visual-basic/language-reference/operators/addition-operator.md)，在運算式中加入兩個值，或使用[-operator （Visual Basic）](../../../../visual-basic/language-reference/operators/subtraction-operator.md)減一，如下列範例所示。  
  
 [!code-vb[VbVbalrOperators#57](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#57)]  
  
 否定也會使用[-Operator （Visual Basic）](../../../../visual-basic/language-reference/operators/subtraction-operator.md)，但只會使用一個運算元，如下列範例所示。  
  
 [!code-vb[VbVbalrOperators#58](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#58)]  
  
 乘法和除法分別使用[* 運算子](../../../../visual-basic/language-reference/operators/multiplication-operator.md)和[/運算子（Visual Basic）](../../../../visual-basic/language-reference/operators/floating-point-division-operator.md)，如下列範例所示。  
  
 [!code-vb[VbVbalrOperators#59](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#59)]  
  
 乘冪會使用[^ 運算子](../../../../visual-basic/language-reference/operators/exponentiation-operator.md)，如下列範例所示。  
  
 [!code-vb[VbVbalrOperators#60](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#60)]  
  
 使用[\ 運算子（Visual Basic）](../../../../visual-basic/language-reference/operators/integer-division-operator.md)來執行整數除法。 整數除法會傳回商，也就是代表除數可以分割成被除數的次數，而不考慮任何餘數的整數。 除數和被除數都必須是此運算子的整數類資料類型（`SByte`、`Byte`、`Short`、`UShort`、`Integer`、`UInteger`、`Long`和 `ULong`）。 所有其他類型都必須先轉換成整數類資料類型。 下列範例示範整數除法。  
  
 [!code-vb[VbVbalrOperators#61](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#61)]  
  
 模數算術會使用[Mod 運算子](../../../../visual-basic/language-reference/operators/mod-operator.md)來執行。 這個運算子會傳回將除數相除成除數為整數次之後的餘數。 如果除數和除數都是整數類資料類型，則傳回的值是整數。 如果除數和被除數為浮點類型，傳回的值也會是浮點。 下列範例示範此行為。  
  
 [!code-vb[VbVbalrOperators#62](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#62)]  
  
 [!code-vb[VbVbalrOperators#63](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#63)]  
  
### <a name="attempted-division-by-zero"></a>嘗試除數為零  
 零除的結果會根據相關的資料類型而有所不同。 在整數分割（`SByte`、`Byte`、`Short`、`UShort`、`Integer`、`UInteger`、`Long`、`ULong`）中，.NET Framework 會擲回 <xref:System.DivideByZeroException> 例外狀況。 在 `Decimal` 或 `Single` 資料類型的除法運算中，.NET Framework 也會擲回 <xref:System.DivideByZeroException> 例外狀況。  
  
 在涉及 `Double` 資料類型的浮點分割中，不會擲回任何例外狀況，而結果會是代表 <xref:System.Double.NaN>、<xref:System.Double.PositiveInfinity>或 <xref:System.Double.NegativeInfinity>的類別成員，視被除數而定。 下表摘要說明嘗試將 `Double` 值零除的各種結果。  
  
|被除數的資料類型|除數資料類型|被除數值|結果|  
|---|---|---|---|  
|`Double`|`Double`|0|<xref:System.Double.NaN> （不是以數學方式定義的數位）|  
|`Double`|`Double`|> 0|<xref:System.Double.PositiveInfinity>|  
|`Double`|`Double`|\< 0|<xref:System.Double.NegativeInfinity>|  
  
 當您攔截 <xref:System.DivideByZeroException> 例外狀況時，您可以使用其成員協助您處理它。 例如，<xref:System.Exception.Message%2A> 屬性會保存例外狀況的郵件內文。 如需詳細資訊，請參閱 [Try...Catch...Finally 陳述式](../../../../visual-basic/language-reference/statements/try-catch-finally-statement.md)。  
  
## <a name="bit-shift-operations"></a>位移位作業  
 位移位運算會在位模式上執行算術移位。 模式會包含在左邊的運算元中，而右邊的運算元則指定要移位模式的位置數目。 您可以使用[> > 運算子](../../../../visual-basic/language-reference/operators/right-shift-operator.md)或左邊加上[< 的 < 運算子](../../../../visual-basic/language-reference/operators/left-shift-operator.md)，將模式向右移位。  
  
 模式運算元的資料類型必須是 `SByte`、`Byte`、`Short`、`UShort`、`Integer`、`UInteger`、`Long`或 `ULong`。 移位量運算元的資料類型必須 `Integer` 或必須擴展為 `Integer`。  
  
 算術移位不是迴圈的，這表示不會在另一端重新進入從結果的一端移位的位。 由 shift 空出的位位置設定如下：  
  
- 0代表算術左移位  
  
- 0代表正數位的算術右移位  
  
- 0表示不帶正負號資料類型（`Byte`、`UShort`、`UInteger`、`ULong`）的算術右移位  
  
- 1代表負數的算術右移（`SByte`、`Short`、`Integer`或 `Long`）  
  
 下列範例會將 `Integer` 值向左和向右移動。  
  
 [!code-vb[VbVbalrOperators#64](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#64)]  
  
 算術移位絕不會產生溢位例外狀況。  
  
## <a name="bitwise-operations"></a>位運算  
 除了邏輯運算子之外，`Not`、`Or`、`And`和 `Xor` 也會在數值上使用時，執行位運算。 如需詳細資訊，請參閱[Visual Basic 的邏輯和位運算子](../../../../visual-basic/programming-guide/language-features/operators-and-expressions/logical-and-bitwise-operators.md)中的「位運算」。  
  
## <a name="type-safety"></a>型別安全  
 運算元通常應該屬於相同的類型。 例如，如果您要使用 `Integer` 變數進行加法，您應該將它加入另一個 `Integer` 變數中，而且也應該將結果指派給類型 `Integer` 的變數。  
  
 確保良好型別安全編碼做法的一種方法是使用[Option Strict 語句](../../../../visual-basic/language-reference/statements/option-strict-statement.md)。 如果您設定 `Option Strict On`，Visual Basic 會自動執行*型別安全*的轉換。 例如，如果您嘗試將 `Integer` 變數加入 `Double` 變數，並將值指派給 `Double` 變數，則作業會正常進行，因為 `Integer` 值可以轉換成 `Double` 而不會遺失資料。 另一方面，類型-unsafe 轉換會導致 `Option Strict On`的編譯器錯誤。 例如，如果您嘗試將 `Integer` 變數加入至 `Double` 變數，並將值指派給 `Integer` 變數，則會產生編譯器錯誤，因為 `Double` 變數無法隱含地轉換成類型 `Integer`。  
  
 不過，如果您設定 `Option Strict Off`，則 Visual Basic 允許隱含的縮小轉換進行，雖然可能會導致資料或精確度意外遺失。 基於這個理由，我們建議您在撰寫生產環境程式碼時使用 `Option Strict On`。 如需詳細資訊，請參閱 [Widening and Narrowing Conversions](../../../../visual-basic/programming-guide/language-features/data-types/widening-and-narrowing-conversions.md)。  
  
## <a name="see-also"></a>請參閱

- [算術運算子](../../../../visual-basic/language-reference/operators/arithmetic-operators.md)
- [位元移位運算子](../../../../visual-basic/language-reference/operators/bit-shift-operators.md)
- [Visual Basic 中的比較運算子](../../../../visual-basic/programming-guide/language-features/operators-and-expressions/comparison-operators.md)
- [Visual Basic 中的串連運算子](../../../../visual-basic/programming-guide/language-features/operators-and-expressions/concatenation-operators.md)
- [Visual Basic 中的邏輯和位運算子](../../../../visual-basic/programming-guide/language-features/operators-and-expressions/logical-and-bitwise-operators.md)
- [有效的運算子組合](../../../../visual-basic/programming-guide/language-features/operators-and-expressions/efficient-combination-of-operators.md)
