---
title: 在 Fedora 30 - 套裝軟體管理器上安裝 .NET 核心 - .NET 核心
description: 使用包管理器在 Fedora 30 上安裝 .NET 核心 SDK 和運行時。
author: thraka
ms.author: adegeo
ms.date: 12/04/2019
ms.openlocfilehash: bce30c9fd3fad8b3a63ef938d7446c2516a756cc
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/14/2020
ms.locfileid: "76920788"
---
# <a name="fedora-30-package-manager---install-net-core"></a>Fedora 30 套裝軟體管理器 - 安裝 .NET 核心

[!INCLUDE [package-manager-switcher](./includes/package-manager-switcher.md)]

本文介紹如何使用包管理器在 Fedora 30 上安裝 .NET Core。 如果要安裝運行時，我們建議您安裝[ASP.NET核心運行時](#install-the-aspnet-core-runtime)，因為它包括 .NET Core 和 ASP.NET核心運行時。

## <a name="register-microsoft-key-and-feed"></a>註冊 Microsoft 金鑰和摘要

在安裝 .NET 之前，您需要：

- 註冊微軟金鑰。
- 註冊產品存儲庫。
- 安裝所需的依賴項。

每部電腦只需要執行這項作業一次。

打開終端並運行以下命令。

```bash
sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
sudo wget -q -O /etc/yum.repos.d/microsoft-prod.repo https://packages.microsoft.com/config/fedora/30/prod.repo
```

## <a name="install-the-net-core-sdk"></a>安裝 .NET Core SDK

更新可供安裝的產品，然後安裝 .NET 核心 SDK。 在終端中，運行以下命令。

```bash
sudo dnf install dotnet-sdk-3.1
```

## <a name="install-the-aspnet-core-runtime"></a>安裝ASP.NET核心運行時

更新可供安裝的產品，然後安裝ASP.NET運行時。 在終端中，運行以下命令。

```bash
sudo dnf install aspnetcore-runtime-3.1
```

## <a name="install-the-net-core-runtime"></a>安裝 .NET 核心運行時

更新可用於安裝的產品，然後安裝 .NET Core 運行時。 在終端中，運行以下命令。

```bash
sudo dnf install dotnet-runtime-3.1
```

## <a name="how-to-install-other-versions"></a>如何安裝其他版本

[!INCLUDE [package-manager-switcher](./includes/package-manager-heading-hack-pkgname.md)]

## <a name="troubleshoot-the-package-manager"></a>排除包管理器故障

本節提供有關在使用包管理器安裝 .NET Core 時可能得到的常見錯誤的資訊。

### <a name="failed-to-fetch"></a>無法提取

[!INCLUDE [package-manager-failed-to-fetch-rpm](includes/package-manager-failed-to-fetch-rpm.md)]
