---
title: ICLRMetaHostPolicy::GetRequestedRuntime 方法
ms.date: 03/30/2017
api_name:
- ICLRMetaHostPolicy.GetRequestedRuntime
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRMetaHostPolicy::GetRequestedRuntime
helpviewer_keywords:
- GetRequestedRuntime method [.NET Framework hosting]
- ICLRMetaHostPolicy::GetRequestedRuntime method [.NET Framework hosting]
ms.assetid: 59ec1832-9cc1-4b5c-983d-03407e51de56
topic_type:
- apiref
ms.openlocfilehash: 1b07029990ef529ded57bc569beff1061ad0f938
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2019
ms.locfileid: "73140876"
---
# <a name="iclrmetahostpolicygetrequestedruntime-method"></a>ICLRMetaHostPolicy::GetRequestedRuntime 方法

提供 Common Language Runtime (CLR) 慣用版本的介面，這是根據裝載原則、Managed 組件、版本字串和組態資料流。 這個方法實際上並不會載入或啟動 CLR，而只會傳回代表原則結果的[ICLRRuntimeInfo](../../../../docs/framework/unmanaged-api/hosting/iclrruntimeinfo-interface.md)介面。 這個方法會取代[GetRequestedRuntimeInfo](../../../../docs/framework/unmanaged-api/hosting/getrequestedruntimeinfo-function.md)、 [GetRequestedRuntimeVersion](../../../../docs/framework/unmanaged-api/hosting/getrequestedruntimeversion-function.md)、 [CorBindToRuntimeHost](../../../../docs/framework/unmanaged-api/hosting/corbindtoruntimehost-function.md)、 [CorBindToRuntimeByCfg](../../../../docs/framework/unmanaged-api/hosting/corbindtoruntimebycfg-function.md)和[GetCORRequiredVersion](../../../../docs/framework/unmanaged-api/hosting/getcorrequiredversion-function.md)方法。

## <a name="syntax"></a>語法

```cpp
HRESULT GetRequestedRuntime(
    [in]  METAHOST_POLICY_FLAGS dwPolicyFlags,
    [in]  LPCWSTR pwzBinary,
    [in]  IStream *pCfgStream,
    [in, out, size_is(*pcchVersion)] LPWSTR pwzVersion,
    [in, out]  DWORD *pcchVersion,
    [out, size_is(*pcchImageVersion)] LPWSTR pwzImageVersion,
    [in, out]  DWORD *pcchImageVersion,
    [out] DWORD *pdwConfigFlags,
    [in]  REFIID  riid
    [out, iid_is(riid), retval] LPVOID *ppRuntime);
```

## <a name="parameters"></a>參數

|[屬性]|描述|
|----------|-----------------|
|`dwPolicyFlags`|[in] 必要項。 指定[METAHOST_POLICY_FLAGS](../../../../docs/framework/unmanaged-api/hosting/metahost-policy-flags-enumeration.md)列舉的成員，代表系結原則，以及任何數目的修飾詞。 目前唯一可用的原則是[METAHOST_POLICY_HIGHCOMPAT](../../../../docs/framework/unmanaged-api/hosting/metahost-policy-flags-enumeration.md)。<br /><br /> 修飾詞包括[METAHOST_POLICY_EMULATE_EXE_LAUNCH](../../../../docs/framework/unmanaged-api/hosting/metahost-policy-flags-enumeration.md)、 [METAHOST_POLICY_APPLY_UPGRADE_POLICY](../../../../docs/framework/unmanaged-api/hosting/metahost-policy-flags-enumeration.md)、 [METAHOST_POLICY_SHOW_ERROR_DIALOG](../../../../docs/framework/unmanaged-api/hosting/metahost-policy-flags-enumeration.md)、 [METAHOST_POLICY_USE_PROCESS_IMAGE_PATH](../../../../docs/framework/unmanaged-api/hosting/metahost-policy-flags-enumeration.md)和[METAHOST_POLICY_ENSURE_SKU_SUPPORTED](../../../../docs/framework/unmanaged-api/hosting/metahost-policy-flags-enumeration.md)。|
|`pwzBinary`|[in] 選用。 指定組件檔路徑。|
|`pCfgStream`|[in] 選用。 以 <xref:System.Runtime.InteropServices.ComTypes.IStream?displayProperty=nameWithType> 指定組態檔。|
|`pwzVersion`|[in, out] 選用。 指定或傳回要載入的慣用 CLR 版本。|
|`pcchVersion`|[in, out] 必要項。 指定 `pwzVersion` 做為輸入的預期大小，以避免緩衝區滿溢。 如果 `pwzVersion` 為 null，`pcchVersion` 包含 `GetRequestedRuntime` 傳回時 `pwzVersion` 的預期大小，以便預先配置，否則 `pcchVersion` 包含要寫入 `pwzVersion` 的字元數目。|
|`pwzImageVersion`|[out] 選用。 當 `GetRequestedRuntime` 傳回時，會包含對應至所傳回之[ICLRRuntimeInfo](../../../../docs/framework/unmanaged-api/hosting/iclrruntimeinfo-interface.md)介面的 CLR 版本。|
|`pcchImageVersion`|[in, out] 選用。 指定 `pwzImageVersion` 做為輸入的大小，以避免緩衝區滿溢。 如果 `pwzImageVersion` 為 null， `pcchImageVersion` 包含 `GetRequestedRuntime` 傳回時的 `pwzImageVersion` 必要大小，以便預先配置。|
|`pdwConfigFlags`|[out] 選用。 如果 `GetRequestedRuntime` 在系結程式期間使用設定檔，當它傳回時，`pdwConfigFlags` 會包含[METAHOST_CONFIG_FLAGS](../../../../docs/framework/unmanaged-api/hosting/metahost-config-flags-enumeration.md)值，指出[\<啟動 >](../../../../docs/framework/configure-apps/file-schema/startup/startup-element.md)專案是否已設定 `useLegacyV2RuntimeActivationPolicy` 屬性，而值為屬性。 將[METAHOST_CONFIG_FLAGS_LEGACY_V2_ACTI加值稅ION_POLICY_MASK](../../../../docs/framework/unmanaged-api/hosting/metahost-config-flags-enumeration.md) MASK 套用至 `pdwConfigFlags`，以取得與 `useLegacyV2RuntimeActivationPolicy`相關的值。|
|`riid`|在指定所要求之[ICLRRuntimeInfo](../../../../docs/framework/unmanaged-api/hosting/iclrruntimeinfo-interface.md)介面的介面識別碼 IID_ICLRRuntimeInfo。|
|`ppRuntime`|脫銷當 `GetRequestedRuntime` 傳回時，會包含對應[ICLRRuntimeInfo](../../../../docs/framework/unmanaged-api/hosting/iclrruntimeinfo-interface.md)介面的指標。|

## <a name="remarks"></a>備註

當這個方法成功時，它的副作用是會合併其他旗標與傳回之執行階段介面的目前預設啟動旗標，並且只在 `<configuration><runtime>` 區段內有一或多個下列項目存在於組態資料流時：

- `<gcServer enabled="true"/>` 會導致設定 `STARTUP_SERVER_GC`。

- `<etwEnable enabled="true"/>` 會導致設定 `STARTUP_ETW`。

- `<appDomainResourceMonitoring enabled="true"/>` 會導致設定 `STARTUP_ARM`。

產生的預設 `STARTUP_FLAGS` 值是從上述清單設定之值與預設啟動旗標值的位元 OR 合併。

## <a name="return-value"></a>傳回值

這個方法會傳回下列特定的 HRESULT，以及表示方法失敗的 HRESULT 錯誤。

|HRESULT|描述|
|-------------|-----------------|
|S_OK|已成功完成命令。|
|E_POINTER|`pwzVersion` 不是 null 且 `pcchVersion` 是 null。<br /><br /> -或-<br /><br /> `pwzImageVersion` 不是 null 且 `pcchImageVersion` 是 null。|
|E_INVALIDARG|`dwPolicyFlags` 未指定 `METAHOST_POLICY_HIGHCOMPAT`。|
|ERROR_INSUFFICIENT_BUFFER|配置給 `pwzVersion` 的記憶體不足。<br /><br /> -或-<br /><br /> 配置給 `pwzImageVersion` 的記憶體不足。|
|CLR_E_SHIM_RUNTIMELOAD|`dwPolicyFlags` 包括 METAHOST_POLICY_APPLY_UPGRADE_POLICY，且 `pwzVersion` 和 `pcchVersion` 都是 null。|

## <a name="requirements"></a>需求

**平台：** 請參閱[系統需求](../../../../docs/framework/get-started/system-requirements.md)。

**標頭：** MetaHost。h

連結**庫：** 包含為 Mscoree.dll 中的資源

**.NET framework 版本：** [!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]

## <a name="see-also"></a>請參閱

- [ICLRMetaHostPolicy 介面](../../../../docs/framework/unmanaged-api/hosting/iclrmetahostpolicy-interface.md)
- [.NET Framework 4 和 4.5 中新增的 CLR 裝載介面](../../../../docs/framework/unmanaged-api/hosting/clr-hosting-interfaces-added-in-the-net-framework-4-and-4-5.md)
- [裝載介面](../../../../docs/framework/unmanaged-api/hosting/hosting-interfaces.md)
- [裝載](../../../../docs/framework/unmanaged-api/hosting/index.md)
