---
title: 減少與 project.json 的封裝相依性
description: 撰寫以 project.json 為基礎的程式庫時，請降低套件相依性。
author: cartermp
ms.date: 06/20/2016
ms.openlocfilehash: 48ba3ef578388fd98fe7cb830df313512d359483
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/14/2020
ms.locfileid: "75740821"
---
# <a name="reducing-package-dependencies-with-projectjson"></a>減少與 project.json 的封裝相依性

本文涵蓋撰寫 `project.json` 程式庫時，您需要了解的降低封裝相依性的內容。 在本文的最後，您會了解如何撰寫程式庫，令它只使用需要的相依性。

## <a name="why-its-important"></a>為何重要

.NET Core 是由 NuGet 封裝組成的產品。  必要的封裝是 [.NETStandard.Library 中繼封裝](https://www.nuget.org/packages/NETStandard.Library) \(英文\)，由 NuGet 封裝組成的其他封裝。 它為您提供了一組包，這些包保證適用于多個 .NET 實現，如 .NET 框架、.NET Core 和 Xamarin/Mono。

不過，您的程式庫有極大的可能不會使用其包含的每個單一封裝。  在撰寫程式庫並透過 NuGet 散發時，最佳的作法是將相依性「修剪」至實際使用的封裝。  這會減少 NuGet 封裝的整體使用量。

## <a name="how-to-do-it"></a>作法

目前，沒有官方`dotnet`命令來修剪包引用。  您必須改以手動作業。  一般程序如下所示︰

1. 參考您 `project.json` 的 `dependencies` 區段的 `NETStandard.Library` 版本 `1.6.0`。
2. 從命令列使用 `dotnet restore` ([請參閱附註](#dotnet-restore-note)) 還原套件。
3. 檢查 `project.lock.json` 檔案，找出 `NETStandard.Library` 區段。  它在靠近檔案開頭處。
4. 複製 `dependencies` 下列出的所有封裝。
5. 移除 `.NETStandard.Library` 參考，並以複製的封裝取而代之。
6. 移除您不需要的封裝參考。

您可以下列方法之一，找出不需要的封裝︰

1. 試驗與錯誤。 這牽涉到移除封裝、還原、查看程式庫是否仍在編譯，以及重複此程序。
2. 使用諸如 [ILSpy](https://github.com/icsharpcode/ILSpy#ilspy-------) 或 [.NET 反射程式](https://www.red-gate.com/products/dotnet-development/reflector)等工具預覽參考，查看程式碼實際使用的參考。 然後，您可以刪除與您正在使用的類型不對應的包。

## <a name="example"></a>範例

假設您編寫的庫為泛型集合類型提供了其他功能。 這類程式庫需要依賴如 `System.Collections` 的封裝，但可能完全不依賴如 `System.Net.Http` 的封裝。 如此，將封裝相依性修剪至此程式庫所需就很好！

若要修剪此程式庫，您可以從 `project.json` 檔案開始，將參考加入 `NETStandard.Library` 版本 `1.6.0`。

```json
{
    "version":"1.0.0",
    "dependencies":{
        "NETStandard.Library":"1.6.0"
    },
    "frameworks": {
        "netstandard1.0": {}
     }
}
```

接下來，使用 `dotnet restore` ([請參閱附註](#dotnet-restore-note)) 還原套件、檢查 `project.lock.json` 檔案，找出所有還原的 `NETStandard.Library` 套件。

以下是以 `netstandard1.0` 為目標時，`project.lock.json` 檔案中相關區段的樣貌：

```json
"NETStandard.Library/1.6.0":{
    "type": "package",
    "dependencies": {
        "Microsoft.NETCore.Platforms": "1.0.1",
        "Microsoft.NETCore.Runtime": "1.0.2",
        "System.Collections": "4.0.11",
        "System.Diagnostics.Debug": "4.0.11",
        "System.Diagnostics.Tools": "4.0.1",
        "System.Globalization": "4.0.11",
        "System.IO": "4.1.0",
        "System.Linq": "4.1.0",
        "System.Net.Primitives": "4.0.11",
        "System.ObjectModel": "4.0.12",
        "System.Reflection": "4.1.0",
        "System.Reflection.Extensions": "4.0.1",
        "System.Reflection.Primitives": "4.0.1",
        "System.Resources.ResourceManager": "4.0.1",
        "System.Runtime": "4.1.0",
        "System.Runtime.Extensions": "4.1.0",
        "System.Text.Encoding": "4.0.11",
        "System.Text.Encoding.Extensions": "4.0.11",
        "System.Text.RegularExpressions": "4.1.0",
        "System.Threading": "4.0.11",
        "System.Threading.Tasks": "4.0.11",
        "System.Xml.ReaderWriter": "4.0.11",
        "System.Xml.XDocument": "4.0.11"
    }
}
```

接下來，將封裝參考複製到程式庫 `project.json` 檔案的 `dependencies` 區段，取代 `NETStandard.Library` 參考︰

```json
{
    "version":"1.0.0",
    "dependencies":{
        "Microsoft.NETCore.Platforms": "1.0.1",
        "Microsoft.NETCore.Runtime": "1.0.2",
        "System.Collections": "4.0.11",
        "System.Diagnostics.Debug": "4.0.11",
        "System.Diagnostics.Tools": "4.0.1",
        "System.Globalization": "4.0.11",
        "System.IO": "4.1.0",
        "System.Linq": "4.1.0",
        "System.Net.Primitives": "4.0.11",
        "System.ObjectModel": "4.0.12",
        "System.Reflection": "4.1.0",
        "System.Reflection.Extensions": "4.0.1",
        "System.Reflection.Primitives": "4.0.1",
        "System.Resources.ResourceManager": "4.0.1",
        "System.Runtime": "4.1.0",
        "System.Runtime.Extensions": "4.1.0",
        "System.Text.Encoding": "4.0.11",
        "System.Text.Encoding.Extensions": "4.0.11",
        "System.Text.RegularExpressions": "4.1.0",
        "System.Threading": "4.0.11",
        "System.Threading.Tasks": "4.0.11",
        "System.Xml.ReaderWriter": "4.0.11",
        "System.Xml.XDocument": "4.0.11"
    },
    "frameworks":{
        "netstandard1.0": {}
    }
}
```

套件數目非常多，其中有許多對擴充集合類型完全沒必要。  您可以手動移除封裝，或使用 [ILSpy](https://github.com/icsharpcode/ILSpy#ilspy-------) 或 [.NET 反射程式](https://www.red-gate.com/products/dotnet-development/reflector/)等工具識別程式碼實際使用的封裝。

修剪過的封裝可能看起來像這樣︰

```json
{
    "dependencies":{
        "Microsoft.NETCore.Platforms": "1.0.1",
        "Microsoft.NETCore.Runtime": "1.0.2",
        "System.Collections": "4.0.11",
        "System.Linq": "4.1.0",
        "System.Runtime": "4.1.0",
        "System.Runtime.Extensions": "4.1.0",
        "System.Runtime.Handles": "4.0.1",
        "System.Runtime.InteropServices": "4.1.0",
        "System.Runtime.InteropServices.RuntimeInformation": "4.0.0",
        "System.Threading.Tasks": "4.0.11"
    },
    "frameworks":{
        "netstandard1.0": {}
     }
}
```

現在，它的使用量比之前依賴 `NETStandard.Library` 中繼封裝時小。

<a name="dotnet-restore-note"></a>
[!INCLUDE[DotNet Restore Note](~/includes/dotnet-restore-note.md)]
