---
title: 在 Ubuntu 19.04 套裝軟體管理器上安裝 .NET 內核 - .NET Core
description: 使用包管理器在 Ubuntu 19.04 上安裝 .NET 核心 SDK 和運行時。
author: thraka
ms.author: adegeo
ms.date: 12/04/2019
ms.openlocfilehash: c7b30d2760a0a83a0fdd7ff5fa35b2f3d490494f
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/14/2020
ms.locfileid: "76920671"
---
# <a name="ubuntu-1904-package-manager---install-net-core"></a>Ubuntu 19.04 套裝軟體管理器 - 安裝 .NET 核心

[!INCLUDE [package-manager-switcher](./includes/package-manager-switcher.md)]

本文介紹如何使用包管理器在 Ubuntu 19.04 上安裝 .NET Core。 如果要安裝運行時，我們建議您安裝[ASP.NET核心運行時](#install-the-aspnet-core-runtime)，因為它包括 .NET Core 和 ASP.NET核心運行時。

## <a name="register-microsoft-key-and-feed"></a>註冊 Microsoft 金鑰和摘要

在安裝 .NET 之前，您需要：

- 註冊微軟金鑰。
- 註冊產品存儲庫。
- 安裝所需的依賴項。

每部電腦只需要執行這項作業一次。

打開終端並運行以下命令。

```bash
wget -q https://packages.microsoft.com/config/ubuntu/19.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
```

## <a name="install-the-net-core-sdk"></a>安裝 .NET Core SDK

更新可供安裝的產品，然後安裝 .NET 核心 SDK。 在終端中，運行以下命令。

```bash
sudo apt-get update
sudo apt-get install apt-transport-https
sudo apt-get update
sudo apt-get install dotnet-sdk-3.1
```

> [!IMPORTANT]
> 如果收到類似于**找不到包 dotnet-sdk-3.1**的錯誤訊息，請參閱[對包管理器部分進行故障排除](#troubleshoot-the-package-manager)。

## <a name="install-the-aspnet-core-runtime"></a>安裝ASP.NET核心運行時

更新可用於安裝的產品，然後安裝ASP.NET核心運行時。 在終端中，運行以下命令。

```bash
sudo apt-get update
sudo apt-get install apt-transport-https
sudo apt-get update
sudo apt-get install aspnetcore-runtime-3.1
```

> [!IMPORTANT]
> 如果收到類似于**無法找到包 aspnetcore-運行時-3.1**的錯誤訊息，請參閱["包管理器疑難排解](#troubleshoot-the-package-manager)"部分。

## <a name="install-the-net-core-runtime"></a>安裝 .NET 核心運行時

更新可用於安裝的產品，然後安裝 .NET Core 運行時。 在終端中，運行以下命令。

```bash
sudo apt-get update
sudo apt-get install apt-transport-https
sudo apt-get update
sudo apt-get install dotnet-runtime-3.1
```

> [!IMPORTANT]
> 如果收到類似于**找不到包 dotnet-運行時-3.1**的錯誤訊息，請參閱[包管理器部分的故障排除](#troubleshoot-the-package-manager)。

## <a name="how-to-install-other-versions"></a>如何安裝其他版本

[!INCLUDE [package-manager-switcher](./includes/package-manager-heading-hack-pkgname.md)]

## <a name="troubleshoot-the-package-manager"></a>排除包管理器故障

本節提供有關在使用包管理器安裝 .NET Core 時可能得到的常見錯誤的資訊。

### <a name="unable-to-locate"></a>無法定位

如果收到類似于**無法找到包 [.NET Core 包]** 的錯誤訊息，請運行以下命令。

```bash
sudo dpkg --purge packages-microsoft-prod && sudo dpkg -i packages-microsoft-prod.deb
sudo apt-get update
sudo apt-get install {the .NET Core package}
```

如果這不起作用，則可以使用以下命令運行手動安裝。

```bash
sudo apt-get install -y gpg
wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor -o microsoft.asc.gpg
sudo mv microsoft.asc.gpg /etc/apt/trusted.gpg.d/
wget -q https://packages.microsoft.com/config/ubuntu/19.04/prod.list
sudo mv prod.list /etc/apt/sources.list.d/microsoft-prod.list
sudo chown root:root /etc/apt/trusted.gpg.d/microsoft.asc.gpg
sudo chown root:root /etc/apt/sources.list.d/microsoft-prod.list
sudo apt-get install -y apt-transport-https
sudo apt-get update
sudo apt-get install {the .NET Core package}
```

### <a name="failed-to-fetch"></a>無法提取

[!INCLUDE [package-manager-failed-to-fetch-deb](includes/package-manager-failed-to-fetch-deb.md)]
