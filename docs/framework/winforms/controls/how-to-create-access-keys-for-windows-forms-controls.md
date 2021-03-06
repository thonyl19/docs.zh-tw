---
title: 建立控制項的便捷鍵
ms.date: 08/20/2019
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- controls [Windows Forms], access keys
- Button control [Windows Forms], access keys
- dialog box controls [Windows Forms], mnemonics
- access keys [Windows Forms], creating for controls
- mnemonics [Windows Forms], adding to dialog box controls
- mnemonics
- ampersand character in shortcut key
- Windows Forms controls, access keys
- examples [Windows Forms], controls
- Text property [Windows Forms], specifying access keys for controls
- keyboard shortcuts [Windows Forms], creating for controls
- access keys [Windows Forms], Windows Forms
- ALT key
ms.assetid: 4faa0991-28ec-4eca-91db-51dc2cd6a7ac
ms.openlocfilehash: 7f6b0a5838cacfc1189fba819a54b3423d567ea0
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2020
ms.locfileid: "76731166"
---
# <a name="how-to-create-access-keys-for-windows-forms-controls"></a>如何：建立 Windows Forms 控制項的存取金鑰

*存取金鑰*是功能表、功能表項目或控制項標籤的文字中加底線的字元，例如按鈕。 使用存取金鑰時，使用者可以按下 ALT 鍵並結合預先定義的存取金鑰，以「按一下」按鈕。 例如，如果按鈕執行列印表單的程式，因此它的 `Text` 屬性設定為 "Print"，則在字母 "P" 之前加上連字號，就會在執行時間的按鈕文字中加上字母 "P"。 使用者可以按 Alt + P，執行與按鈕相關聯的命令。

無法接收焦點的控制項不能有存取金鑰。

## <a name="programmatic"></a>程式設計

將 `Text` 屬性設定為字串，其中在將做為快捷方式的字母前面包含連字號（&）。

```vb
' Set the letter "P" as an access key.
Button1.Text = "&Print"
```

```csharp
// Set the letter "P" as an access key.
button1.Text = "&Print";
```

```cpp
// Set the letter "P" as an access key.
button1->Text = "&Print";
```

> [!NOTE]
> 若要在標題中使用連字號而不建立存取金鑰，請包含兩個 & （& &）。 標題中會顯示單一符號，而且不會有任何字元加上底線。

## <a name="designer"></a>設計師

在 Visual Studio 的 [**屬性**] 視窗中，將 [ **Text** ] 屬性設定為包含連字號（' & '）的字串，並在將成為存取金鑰的字母前面。 例如，若要將字母 "P" 設定為存取金鑰，請輸入 **& Print**。

## <a name="see-also"></a>另請參閱

- <xref:System.Windows.Forms.Button>
- [操作說明：回應 Windows Forms Button 按一下動作](how-to-respond-to-windows-forms-button-clicks.md)
- [操作說明：設定由 Windows Forms 控制項所顯示的文字](how-to-set-the-text-displayed-by-a-windows-forms-control.md)
- [標記個別 Windows Forms 控制項並提供其捷徑](labeling-individual-windows-forms-controls-and-providing-shortcuts-to-them.md)
