---
title: 垃圾回收器配置設置
description: 瞭解用於配置垃圾回收器如何管理 .NET Core 應用記憶體的運行時設置。
ms.date: 01/09/2020
ms.topic: reference
ms.openlocfilehash: 044083d69601f5092724a46d358b2ee5673d428d
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/14/2020
ms.locfileid: "76733526"
---
# <a name="run-time-configuration-options-for-garbage-collection"></a>垃圾回收的運行時配置選項

此頁包含有關可在運行時更改的垃圾回收器 （GC） 設置的資訊。 如果您嘗試實現正在運行的應用的峰值性能，請考慮使用這些設置。 但是，預設值為大多數應用程式在典型情況下提供了最佳性能。

設置在此頁上按組排列。 每個組中的設置通常相互結合使用，以實現特定結果。

> [!NOTE]
>
> - 這些設置也可以由應用在運行時動態更改，因此您設置的任何運行時設置都可能被覆蓋。
> - 某些設置（如[延遲級別](../../standard/garbage-collection/latency.md)）通常僅在設計時通過 API 設置。 此頁省略了此類設置。
> - 對於數位值，對*運行時 config.json*檔中的設置使用十進位標記法，對環境變數設置使用十進位標記法。 對於十六進位值，可以使用或沒有"0x"首碼來指定它們。

## <a name="flavors-of-garbage-collection"></a>垃圾回收的味道

垃圾回收的兩個主要類型是工作站 GC 和伺服器 GC。 有關兩者之間的差異的詳細資訊，請參閱[垃圾回收的基礎知識](../../standard/garbage-collection/fundamentals.md#workstation-and-server-garbage-collection)。

垃圾回收的子口味是背景和非併發的。

使用以下設置選擇垃圾回收的味道：

### <a name="systemgcservercomplus_gcserver"></a>系統.GC.伺服器/COMPlus_gcServer

- 配置應用程式是使用工作站垃圾回收還是伺服器垃圾回收。
- 預設值：工作站垃圾回收 （）。`false`

| | 設定名稱 | 值 | 介紹的版本 |
| - | - | - | - |
| **運行時配置.json** | `System.GC.Server` | `false`- 工作站<br/>`true`- 伺服器 | .NET Core 1.0 |
| **MSBuild 屬性** | `ServerGarbageCollection` | `false`- 工作站<br/>`true`- 伺服器 | .NET Core 1.0 |
| **環境變數** | `COMPlus_gcServer` | `0`- 工作站<br/>`1`- 伺服器 | .NET Core 1.0 |
| **.NET 框架的應用程式佈建** | [GCServer](../../framework/configure-apps/file-schema/runtime/gcserver-element.md) | `false`- 工作站<br/>`true`- 伺服器 |  |

### <a name="examples"></a>範例

*運行時配置.json*檔：

```json
{
   "runtimeOptions": {
      "configProperties": {
         "System.GC.Server": true
      }
   }
}
```

專案檔：

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <ServerGarbageCollection>true</ServerGarbageCollection>
  </PropertyGroup>

</Project>
```

### <a name="systemgcconcurrentcomplus_gcconcurrent"></a>系統.GC.併發/COMPlus_gcConcurrent

- 配置是否啟用後臺（併發）垃圾回收。
- 預設值： 已`true`啟用 （ 。
- 有關詳細資訊，請參閱[後臺垃圾回收](../../standard/garbage-collection/fundamentals.md#background-workstation-garbage-collection)和[後臺伺服器垃圾回收](../../standard/garbage-collection/fundamentals.md#background-server-garbage-collection)。

| | 設定名稱 | 值 | 介紹的版本 |
| - | - | - | - |
| **運行時配置.json** | `System.GC.Concurrent` | `true`- 背景 GC<br/>`false`- 非併發 GC | .NET Core 1.0 |
| **MSBuild 屬性** | `ConcurrentGarbageCollection` | `true`- 背景 GC<br/>`false`- 非併發 GC | .NET Core 1.0 |
| **環境變數** | `COMPlus_gcConcurrent` | `true`- 背景 GC<br/>`false`- 非併發 GC | .NET Core 1.0 |
| **.NET 框架的應用程式佈建** | [gc 併發](../../framework/configure-apps/file-schema/runtime/gcconcurrent-element.md) | `true`- 背景 GC<br/>`false`- 非併發 GC |  |

### <a name="examples"></a>範例

*運行時配置.json*檔：

```json
{
   "runtimeOptions": {
      "configProperties": {
         "System.GC.Concurrent": false
      }
   }
}
```

專案檔：

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <ConcurrentGarbageCollection>false</ConcurrentGarbageCollection>
  </PropertyGroup>

</Project>
```

## <a name="manage-resource-usage"></a>管理資源使用方式

使用本節中描述的設置來管理垃圾回收器的記憶體和處理器使用方式。

有關其中一些設置的詳細資訊，請參閱[工作站和伺服器 GC 博客條目之間的中間地帶](https://devblogs.microsoft.com/dotnet/middle-ground-between-server-and-workstation-gc/)。

### <a name="systemgcheapcountcomplus_gcheapcount"></a>系統.GC.堆計數/COMPlus_GCHeapCount

- 限制垃圾回收器創建的堆數。
- 僅適用于伺服器垃圾回收。
- 如果啟用了 GC 處理器關聯（這是預設值，則堆計數設置將`n`GC 堆/執行緒關聯化為第一個`n`處理器）。 （使用關連遮罩或關聯範圍設置來準確指定要關聯處理器的處理器。
- 如果禁用 GC 處理器關聯，此設置將限制 GC 堆的數量。
- 有關詳細資訊，請參閱[GCHeapCount 注釋](../../framework/configure-apps/file-schema/runtime/gcheapcount-element.md#remarks)。

| | 設定名稱 | 值 | 介紹的版本 |
| - | - | - | - |
| **運行時配置.json** | `System.GC.HeapCount` | *十進位值* | .NET Core 3.0 |
| **環境變數** | `COMPlus_GCHeapCount` | *十六進位值* | .NET Core 3.0 |
| **.NET 框架的應用程式佈建** | [GCHeapCount](../../framework/configure-apps/file-schema/runtime/gcheapcount-element.md) | *十進位值* | .NET Framework 4.6.2 |

範例：

```json
{
   "runtimeOptions": {
      "configProperties": {
         "System.GC.HeapCount": 16
      }
   }
}
```

> [!TIP]
> 如果要在*運行時 config.json*中設置該選項，請指定一個小數值。 如果要將該選項設置為環境變數，請指定十六進位值。 例如，要將堆數限制為 16，JSON 檔的值將為 16，環境變數的值為 0x10 或 10。

### <a name="systemgcheapaffinitizemaskcomplus_gcheapaffinitizemask"></a>系統.GC.Heapaffinitize 遮罩/COMPlus_GCHeapAffinitizeMask

- 指定垃圾回收器執行緒應使用的確切處理器。
- 如果通過設置為`System.GC.NoAffinitize``true`禁用處理器關聯，則忽略此設置。
- 僅適用于伺服器垃圾回收。
- 該值是定義進程可用的處理器的位元遮罩。 例如，十進位值 1023（或十六進位值 0x3FF 或 3FF，如果您正在使用環境變數）是 0011 1111 1111 在二進位標記法中。 這指定將使用前 10 個處理器。 要指定接下來的 10 個處理器，即處理器 10-19，請指定 1047552（或十六進位值 0xFFC00 或 FFC00），這相當於二進位值 1111 1111 1100 00000000000。

| | 設定名稱 | 值 | 介紹的版本 |
| - | - | - | - |
| **運行時配置.json** | `System.GC.HeapAffinitizeMask` | *十進位值* | .NET Core 3.0 |
| **環境變數** | `COMPlus_GCHeapAffinitizeMask` | *十六進位值* | .NET Core 3.0 |
| **.NET 框架的應用程式佈建** | [GCHeapaffinitize 遮罩](../../framework/configure-apps/file-schema/runtime/gcheapaffinitizemask-element.md) | *十進位值* | .NET Framework 4.6.2 |

範例：

```json
{
   "runtimeOptions": {
      "configProperties": {
         "System.GC.HeapAffinitizeMask": 1023
      }
   }
}
```

### <a name="systemgcgcheapaffinitizerangescomplus_gcheapaffinitizeranges"></a>系統.GC.GCHeapaffinitize 範圍/COMPlus_GCHeapAffinitizeRanges

- 指定用於垃圾回收器執行緒的處理器清單。
- 此設置類似于`System.GC.HeapAffinitizeMask`，只不過它允許您指定超過 64 個處理器。
- 對於 Windows 作業系統，使用相應的[CPU 組](/windows/win32/procthread/processor-groups)對處理器編號或範圍進行首碼，例如，"0：1-10，0：12，1：50-52，1：70"。
- 如果通過設置為`System.GC.NoAffinitize``true`禁用處理器關聯，則忽略此設置。
- 僅適用于伺服器垃圾回收。
- 有關詳細資訊，請參閱在 Maoni Stephens 博客上[具有 > 64 個 CPU 的電腦上為 GC 提供更好的 CPU 配置](https://devblogs.microsoft.com/dotnet/making-cpu-configuration-better-for-gc-on-machines-with-64-cpus/)。

| | 設定名稱 | 值 | 介紹的版本 |
| - | - | - | - |
| **運行時配置.json** | `System.GC.GCHeapAffinitizeRanges` | 處理器編號或處理器編號範圍的逗號分隔清單。<br/>Unix 示例："1-10，12，50-52，70"<br/>視窗示例："0：1-10，0：12，1：50-52，1：70" | .NET Core 3.0 |
| **環境變數** | `COMPlus_GCHeapAffinitizeRanges` | 處理器編號或處理器編號範圍的逗號分隔清單。<br/>Unix 示例："1-10，12，50-52，70"<br/>視窗示例："0：1-10，0：12，1：50-52，1：70" | .NET Core 3.0 |

範例：

```json
{
   "runtimeOptions": {
      "configProperties": {
         "System.GC.GCHeapAffinitizeRanges": "0:1-10,0:12,1:50-52,1:70"
      }
   }
}
```

### <a name="complus_gccpugroup"></a>COMPlus_GCCpuGroup

- 配置垃圾回收器是否使用[CPU 組](/windows/win32/procthread/processor-groups)。

  當 64 位 Windows 電腦具有多個 CPU 組（即，超過 64 個處理器）時，啟用此元素會跨所有 CPU 組擴展垃圾回收。 垃圾回收器使用所有內核創建和平衡堆。

- 僅適用于 64 位 Windows 作業系統上的伺服器垃圾回收。
- 預設值： 已`0`禁用 （ 。
- 有關詳細資訊，請參閱在 Maoni Stephens 博客上[具有 > 64 個 CPU 的電腦上為 GC 提供更好的 CPU 配置](https://devblogs.microsoft.com/dotnet/making-cpu-configuration-better-for-gc-on-machines-with-64-cpus/)。

| | 設定名稱 | 值 | 介紹的版本 |
| - | - | - | - |
| **運行時配置.json** | N/A | N/A | N/A |
| **環境變數** | `COMPlus_GCCpuGroup` | `0`- 禁用<br/>`1`- 已啟用 | .NET Core 1.0 |
| **.NET 框架的應用程式佈建** | [GCCpu集團](../../framework/configure-apps/file-schema/runtime/gccpugroup-element.md) | `false`- 禁用<br/>`true`- 已啟用 |  |

> [!NOTE]
> 要將通用語言運行時 （CLR） 配置為將執行緒池中的執行緒分配到所有 CPU 組，請啟用[Thread_UseAllCpuGroups元素](../../framework/configure-apps/file-schema/runtime/thread-useallcpugroups-element.md)選項。 對於 .NET Core 應用，可以通過將`COMPlus_Thread_UseAllCpuGroups`環境變數的值設置為`1`來啟用此選項。

### <a name="systemgcnoaffinitizecomplus_gcnoaffinitize"></a>系統.GC.無資訊化/COMPlus_GCNoAffinitize

- 指定是否將垃圾回收執行緒與處理器*關聯*化。 關聯 GC 執行緒意味著它只能在其特定 CPU 上運行。 為每個 GC 執行緒創建一個堆。
- 僅適用于伺服器垃圾回收。
- 預設值：使用處理器 （） 對垃圾回收執行緒`false`進行 affinitit 化。

| | 設定名稱 | 值 | 介紹的版本 |
| - | - | - | - |
| **運行時配置.json** | `System.GC.NoAffinitize` | `false`- 親緣關係<br/>`true`-不要親和 | .NET Core 3.0 |
| **環境變數** | `COMPlus_GCNoAffinitize` | `0`- 親緣關係<br/>`1`-不要親和 | .NET Core 3.0 |
| **.NET 框架的應用程式佈建** | [GCNoaffinitize](../../framework/configure-apps/file-schema/runtime/gcnoaffinitize-element.md) | `false`- 親緣關係<br/>`true`-不要親和 | .NET Framework 4.6.2 |

範例：

```json
{
   "runtimeOptions": {
      "configProperties": {
         "System.GC.NoAffinitize": true
      }
   }
}
```

### <a name="systemgcheaphardlimitcomplus_gcheaphardlimit"></a>系統.GC.HeapHard 限制/COMPlus_GCHeapHardLimit

- 為 GC 堆和 GC 簿記指定最大提交大小（以位元組為單位）。

| | 設定名稱 | 值 | 介紹的版本 |
| - | - | - | - |
| **運行時配置.json** | `System.GC.HeapHardLimit` | *十進位值* | .NET Core 3.0 |
| **環境變數** | `COMPlus_GCHeapHardLimit` | *十六進位值* | .NET Core 3.0 |

範例：

```json
{
   "runtimeOptions": {
      "configProperties": {
         "System.GC.HeapHardLimit": 209715200
      }
   }
}
```

> [!TIP]
> 如果要在*運行時 config.json*中設置該選項，請指定一個小數值。 如果要將該選項設置為環境變數，請指定十六進位值。 例如，要指定 200 個乘位元組 （MiB） 的堆硬限制，JSON 檔的值將為 209715200，環境變數的值為 0xC800000 或 C800000。

### <a name="systemgcheaphardlimitpercentcomplus_gcheaphardlimitpercent"></a>系統.GC.HeapHard 限制百分比/COMPlus_GCHeapHardLimitPercent

- 將 GC 堆使用方式指定為總記憶體的百分比。

| | 設定名稱 | 值 | 介紹的版本 |
| - | - | - | - |
| **運行時配置.json** | `System.GC.HeapHardLimitPercent` | *十進位值* | .NET Core 3.0 |
| **環境變數** | `COMPlus_GCHeapHardLimitPercent` | *十六進位值* | .NET Core 3.0 |

範例：

```json
{
   "runtimeOptions": {
      "configProperties": {
         "System.GC.HeapHardLimitPercent": 30
      }
   }
}
```

> [!TIP]
> 如果要在*運行時 config.json*中設置該選項，請指定一個小數值。 如果要將該選項設置為環境變數，請指定十六進位值。 例如，要將堆用法限制為 30%，JSON 檔的值為 30，環境變數的值為 0x1E 或 1E。

### <a name="systemgcretainvmcomplus_gcretainvm"></a>系統.GC.保留VM/COMPlus_GCRetainVM

- 配置應刪除的段是放在備用清單中以備將來使用，還是釋放回作業系統 （OS）。
- 預設值：釋放段回作業系統 （）。`false`

| | 設定名稱 | 值 | 介紹的版本 |
| - | - | - | - |
| **運行時配置.json** | `System.GC.RetainVM` | `false`- 向作業系統發佈<br/>`true`- 置於待機狀態 | .NET Core 1.0 |
| **MSBuild 屬性** | `RetainVMGarbageCollection` | `false`- 向作業系統發佈<br/>`true`- 置於待機狀態 | .NET Core 1.0 |
| **環境變數** | `COMPlus_GCRetainVM` | `0`- 向作業系統發佈<br/>`1`- 置於待機狀態 | .NET Core 1.0 |

### <a name="examples"></a>範例

*運行時配置.json*檔：

```json
{
   "runtimeOptions": {
      "configProperties": {
         "System.GC.RetainVM": true
      }
   }
}
```

專案檔：

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <RetainVMGarbageCollection>true</RetainVMGarbageCollection>
  </PropertyGroup>

</Project>
```

## <a name="large-pages"></a>大頁

### <a name="complus_gclargepages"></a>COMPlus_GCLargePages

- 指定在設置堆硬限制時是否應使用大頁面。
- 預設值： 已`0`禁用 （ 。
- 這是一個實驗設置。

| | 設定名稱 | 值 | 介紹的版本 |
| - | - | - | - |
| **運行時配置.json** | N/A | N/A | N/A |
| **環境變數** | `COMPlus_GCLargePages` | `0`- 禁用<br/>`1`- 已啟用 | .NET Core 3.0 |

## <a name="large-objects"></a>大型物件

### <a name="complus_gcallowverylargeobjects"></a>COMPlus_gcAllowVeryLargeObjects

- 為總大小大於 2 GB 的陣列配置 64 位平臺上的垃圾回收器支援。
- 預設值： 已`1`啟用 （ 。
- 此選項可能會在 .NET 的未來版本中過時。

| | 設定名稱 | 值 | 介紹的版本 |
| - | - | - | - |
| **運行時配置.json** | N/A | N/A | N/A |
| **環境變數** | `COMPlus_gcAllowVeryLargeObjects` | `1`- 已啟用<br/> `0`- 禁用 | .NET Core 1.0 |
| **.NET 框架的應用程式佈建** | [gcAllow非常大的物件](../../framework/configure-apps/file-schema/runtime/gcallowverylargeobjects-element.md) | `1`- 已啟用<br/> `0`- 禁用 | .NET Framework 4.5 |

## <a name="large-object-heap-threshold"></a>大型物件堆閾值

### <a name="systemgclohthresholdcomplus_gclohthreshold"></a>系統.GC.LOH閾值/COMPlus_GCLOHThreshold

- 指定導致物件轉到大型物件堆 （LOH） 上的閾值大小（以位元組為單位）。
- 預設閾值為 85，000 位元組。
- 指定的值必須大於預設閾值。

| | 設定名稱 | 值 | 介紹的版本 |
| - | - | - | - |
| **運行時配置.json** | `System.GC.LOHThreshold` | *十進位值* | .NET Core 1.0 |
| **環境變數** | `COMPlus_GCLOHThreshold` | *十六進位值* | .NET Core 1.0 |
| **.NET 框架的應用程式佈建** | [GCLOH閾值](../../framework/configure-apps/file-schema/runtime/gclohthreshold-element.md) | *十進位值* | .NET Framework 4.8 |

範例：

```json
{
   "runtimeOptions": {
      "configProperties": {
         "System.GC.LOHThreshold": 120000
      }
   }
}
```

> [!TIP]
> 如果要在*運行時 config.json*中設置該選項，請指定一個小數值。 如果要將該選項設置為環境變數，請指定十六進位值。 例如，要設置 120，000 位元組的閾值大小，JSON 檔的值將為 120000，環境變數的值為 0x1D4C0 或 1D4C0。

## <a name="standalone-gc"></a>獨立 GC

### <a name="complus_gcname"></a>COMPlus_GCName

- 指定包含運行時打算載入的垃圾回收器的庫的路徑。
- 有關詳細資訊，請參閱獨立[GC 載入程式設計](https://github.com/dotnet/runtime/blob/master/docs/design/features/standalone-gc-loading.md)。

| | 設定名稱 | 值 | 介紹的版本 |
| - | - | - | - |
| **運行時配置.json** | N/A | N/A | N/A |
| **環境變數** | `COMPlus_GCName` | *string_path* | .NET Core 2.0 |
