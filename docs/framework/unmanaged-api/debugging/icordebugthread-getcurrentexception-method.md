---
title: ICorDebugThread::GetCurrentException 方法
ms.date: 03/30/2017
api_name:
- ICorDebugThread.GetCurrentException
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugThread::GetCurrentException
helpviewer_keywords:
- ICorDebugThread::GetCurrentException method [.NET Framework debugging]
- GetCurrentException method [.NET Framework debugging]
ms.assetid: 331ed465-a195-4359-8584-b82c6098b29b
topic_type:
- apiref
ms.openlocfilehash: 8082b2a3654f1605f18f3b68f54464dc83c8e60a
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2019
ms.locfileid: "73133482"
---
# <a name="icordebugthreadgetcurrentexception-method"></a>ICorDebugThread::GetCurrentException 方法
取得 ICorDebugValue 物件的介面指標，代表目前由 managed 程式碼擲回的例外狀況。  
  
## <a name="syntax"></a>語法  
  
```cpp  
HRESULT GetCurrentException (  
    [out] ICorDebugValue **ppExceptionObject  
);  
```  
  
## <a name="parameters"></a>參數  
 `ppExceptionObject`  
 脫銷`ICorDebugValue` 物件之位址的指標，代表目前由 managed 程式碼擲回的例外狀況。  
  
## <a name="remarks"></a>備註  
 例外狀況物件會從擲回例外狀況的時間開始，直到 `catch` 區塊結束為止。 函式評估是由 ICorDebugEval 方法執行，會在安裝時清除例外狀況物件，並在完成時將它還原。  
  
 例外狀況可以是嵌套的（例如，如果在篩選準則中擲回例外狀況或在函式評估中擲回），因此單一線程上可能會有多個未處理的例外狀況。 `GetCurrentException` 會傳回最新的例外狀況。  
  
 例外狀況物件和類型在例外狀況的整個生命週期中可能會變更。 例如，擲回 x 類型的例外狀況之後，common language runtime （CLR）可能會耗盡記憶體，並將它升級為記憶體不足的例外狀況。  
  
## <a name="requirements"></a>需求  
 **平台：** 請參閱[系統需求](../../../../docs/framework/get-started/system-requirements.md)。  
  
 **標頭：** CorDebug.idl、CorDebug.h  
  
 **程式庫：** CorGuids.lib  
  
 **.NET framework 版本：** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]
