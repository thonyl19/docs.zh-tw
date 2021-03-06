---
ms.openlocfilehash: 53d2c989120c92f4e2d18f50ce4b364bd4c9b604
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/14/2020
ms.locfileid: "75901776"
---
### <a name="http-synchronous-io-disabled-in-all-servers"></a>HTTP：在所有伺服器上禁用同步 IO

從 ASP.NET 酷 3.0 開始，預設情況下禁用同步伺服器操作。

#### <a name="change-description"></a>變更描述

`AllowSynchronousIO`是每個伺服器中啟用或禁用同步 IO API 的選項，如`HttpRequest.Body.Read` `HttpResponse.Body.Write`。`Stream.Flush`和 。 長期以來，這些 API 一直是執行緒不足和應用程式掛起的來源。 從 ASP.NET 酷 3.0 預覽 3 中開始，預設情況下將禁用這些同步操作。

受影響的伺服器：

- Kestrel
- HttpSys
- IIS 在流程中
- 測試伺服器

預期錯誤類似于：

- `Synchronous operations are disallowed. Call ReadAsync or set AllowSynchronousIO to true instead.`
- `Synchronous operations are disallowed. Call WriteAsync or set AllowSynchronousIO to true instead.`
- `Synchronous operations are disallowed. Call FlushAsync or set AllowSynchronousIO to true instead.`

每個伺服器都有一`AllowSynchronousIO`個選項，用於控制此行為，並且所有伺服器的預設值現在`false`為 。

也可以根據請求重寫該行為作為臨時緩解。 例如：

```csharp
var syncIOFeature = HttpContext.Features.Get<IHttpBodyControlFeature>();
if (syncIOFeature != null)
{
    syncIOFeature.AllowSynchronousIO = true;
}
```

如果在 中調用同步`TextWriter`API 的或其他流時遇到問題`Dispose`，請改為調用`DisposeAsync`新的 API。

有關討論，請參閱[點網/阿斯平核心#7644](https://github.com/dotnet/aspnetcore/issues/7644)。

#### <a name="version-introduced"></a>介紹的版本

3.0

#### <a name="old-behavior"></a>舊的行為

`HttpRequest.Body.Read`，`HttpResponse.Body.Write`預設情況下`Stream.Flush`允許。

#### <a name="new-behavior"></a>新的行為

預設情況下不允許這些同步 API：

預期錯誤類似于：

- `Synchronous operations are disallowed. Call ReadAsync or set AllowSynchronousIO to true instead.`
- `Synchronous operations are disallowed. Call WriteAsync or set AllowSynchronousIO to true instead.`
- `Synchronous operations are disallowed. Call FlushAsync or set AllowSynchronousIO to true instead.`

#### <a name="reason-for-change"></a>更改原因

這些同步 API 長期以來一直是執行緒不足和應用程式掛起的來源。 從 ASP.NET 酷 3.0 預覽 3 中開始，預設情況下將禁用同步操作。

#### <a name="recommended-action"></a>建議的動作

使用方法的非同步版本。 也可以根據請求重寫該行為作為臨時緩解。

```csharp
var syncIOFeature = HttpContext.Features.Get<IHttpBodyControlFeature>();
if (syncIOFeature != null)
{
    syncIOFeature.AllowSynchronousIO = true;
}
```

#### <a name="category"></a>類別

ASP.NET Core

#### <a name="affected-apis"></a>受影響的 API

- <xref:System.IO.Stream.Flush%2A?displayProperty=nameWithType>
- <xref:System.IO.Stream.Read%2A?displayProperty=nameWithType>
- <xref:System.IO.Stream.Write%2A?displayProperty=nameWithType>

<!--

#### Affected APIs

- `Overload:System.IO.Stream.Flush`
- `Overload:System.IO.Stream.Read`
- `Overload:System.IO.Stream.Write`

-->
