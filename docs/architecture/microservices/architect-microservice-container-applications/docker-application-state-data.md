---
title: Docker 應用程式中的狀態和資料
description: Docker 應用程式中的狀態和資料管理。 微服務執行個體可消耗，但資料無法消耗；如何使用微服務處理它。
ms.date: 09/20/2018
ms.openlocfilehash: 9d7b0ff0e73267c6b80be2f1c956c3b4eae140e2
ms.sourcegitcommit: f20dd18dbcf2275513281f5d9ad7ece6a62644b4
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/30/2019
ms.locfileid: "68673125"
---
# <a name="state-and-data-in-docker-applications"></a><span data-ttu-id="018ec-104">Docker 應用程式中的狀態和資料</span><span class="sxs-lookup"><span data-stu-id="018ec-104">State and data in Docker applications</span></span>

<span data-ttu-id="018ec-105">在大多數情況下，您可以將容器視為處理序執行個體。</span><span class="sxs-lookup"><span data-stu-id="018ec-105">In most cases, you can think of a container as an instance of a process.</span></span> <span data-ttu-id="018ec-106">處理序不會保留持續性狀態。</span><span class="sxs-lookup"><span data-stu-id="018ec-106">A process doesn't maintain persistent state.</span></span> <span data-ttu-id="018ec-107">雖然容器可以寫入至其本機存放區，但假設執行個體會無限期存在，就像是假設記憶體中的某個位置會持久一樣。</span><span class="sxs-lookup"><span data-stu-id="018ec-107">While a container can write to its local storage, assuming that an instance will be around indefinitely would be like assuming that a single location in memory will be durable.</span></span> <span data-ttu-id="018ec-108">建議您假設容器映像與處理序相似，擁有多個執行個體，或是最後會遭到終止。</span><span class="sxs-lookup"><span data-stu-id="018ec-108">You should assume that container images, like processes, have multiple instances or will eventually be killed.</span></span> <span data-ttu-id="018ec-109">若它們是使用容器協調器進行管理，建議您假設它們可能會從一個節點或 VM 移動到另一個。</span><span class="sxs-lookup"><span data-stu-id="018ec-109">If they're managed with a container orchestrator, you should assume that they might get moved from one node or VM to another.</span></span>

<span data-ttu-id="018ec-110">下列解決方案可用來管理 Docker 應用程式中的持續性資料：</span><span class="sxs-lookup"><span data-stu-id="018ec-110">The following solutions are used to manage persistent data in Docker applications:</span></span>

<span data-ttu-id="018ec-111">從 Docker 主機，透過 [Docker Volumes](https://docs.docker.com/engine/admin/volumes/) (Docker 磁碟區)：</span><span class="sxs-lookup"><span data-stu-id="018ec-111">From the Docker host, as [Docker Volumes](https://docs.docker.com/engine/admin/volumes/):</span></span>

- <span data-ttu-id="018ec-112">**磁碟區**會儲存在主機檔案系統中由 Docker 管理的一個區域內。</span><span class="sxs-lookup"><span data-stu-id="018ec-112">**Volumes** are stored in an area of the host filesystem that's managed by Docker.</span></span>

- <span data-ttu-id="018ec-113">由於**繫結裝載**可對應到主機檔案系統內的任何資料夾，因此無法從 Docker 處理序管理存取，且可能會因為容器能存取敏感性 OS 資料夾，而帶來安全性風險。</span><span class="sxs-lookup"><span data-stu-id="018ec-113">**Bind mounts** can map to any folder in the host filesystem, so access can't be controlled from Docker process and can pose a security risk as a container could access sensitive OS folders.</span></span>

- <span data-ttu-id="018ec-114">**tmpfs 裝載**則與虛擬資料夾相似，只存在於主機的記憶體中，永遠不會寫入檔案系統。</span><span class="sxs-lookup"><span data-stu-id="018ec-114">**tmpfs mounts** are like virtual folders that only exist in the host's memory and are never written to the filesystem.</span></span>

<span data-ttu-id="018ec-115">從遠端存放：</span><span class="sxs-lookup"><span data-stu-id="018ec-115">From remote storage:</span></span>

- <span data-ttu-id="018ec-116">[Azure 儲存體](https://azure.microsoft.com/documentation/services/storage/)，備有異地分散式儲存體，以提供容器的長期持續性解決方案。</span><span class="sxs-lookup"><span data-stu-id="018ec-116">[Azure Storage](https://azure.microsoft.com/documentation/services/storage/), which provides geo-distributable storage, providing a good long-term persistence solution for containers.</span></span>

- <span data-ttu-id="018ec-117">遠端關聯式資料庫 (例如 [Azure SQL Database](https://azure.microsoft.com/services/sql-database/))、NoSQL 資料庫 (例如 [Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/introduction)) 或快取服務 (例如 [Redis](https://redis.io/))。</span><span class="sxs-lookup"><span data-stu-id="018ec-117">Remote relational databases like [Azure SQL Database](https://azure.microsoft.com/services/sql-database/) or NoSQL databases like [Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/introduction), or cache services like [Redis](https://redis.io/).</span></span>

<span data-ttu-id="018ec-118">從 Docker 容器：</span><span class="sxs-lookup"><span data-stu-id="018ec-118">From the Docker container:</span></span>

> <span data-ttu-id="018ec-119">Docker 提供一項功能，稱為「重疊檔案系統」  。</span><span class="sxs-lookup"><span data-stu-id="018ec-119">Docker provides a feature named the *overlay file system*.</span></span> <span data-ttu-id="018ec-120">這會實作寫入時複製工作，將已更新的資訊儲存至容器的根檔案系統。</span><span class="sxs-lookup"><span data-stu-id="018ec-120">This implements a copy-on-write task that stores updated information to the root file system of the container.</span></span> <span data-ttu-id="018ec-121">該資訊是容器所依據之原始映像以外的資訊。</span><span class="sxs-lookup"><span data-stu-id="018ec-121">That information is in addition to the original image on which the container is based.</span></span> <span data-ttu-id="018ec-122">如果從系統中刪除容器，這些變更將會遺失。</span><span class="sxs-lookup"><span data-stu-id="018ec-122">If the container is deleted from the system, those changes are lost.</span></span> <span data-ttu-id="018ec-123">因此，雖然有可能將容器的狀態儲存在其本機存放區，但以此為中心來設計系統會與容器設計的前提 (預設為無狀態) 衝突。</span><span class="sxs-lookup"><span data-stu-id="018ec-123">Therefore, while it's possible to save the state of a container within its local storage, designing a system around this would conflict with the premise of container design, which by default is stateless.</span></span>
>
> <span data-ttu-id="018ec-124">但是，先前引進的 Docker 磁碟區現在是處理本機資料 Docker 的慣用方式。</span><span class="sxs-lookup"><span data-stu-id="018ec-124">However, the previously introduced Docker Volumes is now the preferred way to handling local data Docker.</span></span> <span data-ttu-id="018ec-125">若您需要容器內儲存體的詳細資訊，請查看 [Docker storage drivers ](https://docs.docker.com/storage/storagedriver/select-storage-driver/) (Docker 儲存體驅動程式) 與 [About storage drivers](https://docs.docker.com/storage/storagedriver/) (關於儲存體驅動程式)。</span><span class="sxs-lookup"><span data-stu-id="018ec-125">If you need more information about storage in containers check on [Docker storage drivers](https://docs.docker.com/storage/storagedriver/select-storage-driver/) and [About storage drivers](https://docs.docker.com/storage/storagedriver/).</span></span>

<span data-ttu-id="018ec-126">下列提供這些選項的更多詳細資料：</span><span class="sxs-lookup"><span data-stu-id="018ec-126">The following provides more detail about these options:</span></span>

<span data-ttu-id="018ec-127">**磁碟區**是從主機 OS 對應至容器內目錄的目錄。</span><span class="sxs-lookup"><span data-stu-id="018ec-127">**Volumes** are directories mapped from the host OS to directories in containers.</span></span> <span data-ttu-id="018ec-128">當容器中的程式碼具有目錄的存取權時，該存取權實際上就是主機 OS 上的目錄存取權。</span><span class="sxs-lookup"><span data-stu-id="018ec-128">When code in the container has access to the directory, that access is actually to a directory on the host OS.</span></span> <span data-ttu-id="018ec-129">此目錄不會和容器本身的存留期繫結，且目錄會由 Docker 管理，且和主機電腦的核心功能隔離。</span><span class="sxs-lookup"><span data-stu-id="018ec-129">This directory is not tied to the lifetime of the container itself, and the directory is managed by Docker and isolated from the core functionality of the host machine.</span></span> <span data-ttu-id="018ec-130">因此，資料磁碟區設計成可無視於容器的存留期來保存資料。</span><span class="sxs-lookup"><span data-stu-id="018ec-130">Thus, data volumes are designed to persist data independently of the life of the container.</span></span> <span data-ttu-id="018ec-131">如果您從 Docker 主機中刪除容器或映像，則不會刪除保存在資料磁碟區中的資料。</span><span class="sxs-lookup"><span data-stu-id="018ec-131">If you delete a container or an image from the Docker host, the data persisted in the data volume isn't deleted.</span></span>

<span data-ttu-id="018ec-132">磁碟區可以具名，也可以是匿名 (預設)。</span><span class="sxs-lookup"><span data-stu-id="018ec-132">Volumes can be named or anonymous (the default).</span></span> <span data-ttu-id="018ec-133">具名磁碟區是**資料磁碟區容器**的演進，可輕鬆的在容器間共用資料。</span><span class="sxs-lookup"><span data-stu-id="018ec-133">Named volumes are the evolution of **Data Volume Containers** and make it easy to share data between containers.</span></span> <span data-ttu-id="018ec-134">磁碟區也支援磁碟區驅動程式，除了其他選項之外，也可讓您將資料儲存在遠端主機上。</span><span class="sxs-lookup"><span data-stu-id="018ec-134">Volumes also support volume drivers, that allow you to store data on remote hosts, among other options.</span></span>

<span data-ttu-id="018ec-135">**繫結裝載**則是從很久以前開始便持續存在的可用選項，允許將任何資料夾對應到容器中的掛接點。</span><span class="sxs-lookup"><span data-stu-id="018ec-135">**Bind mounts** are available since a long time ago and allow the mapping of any folder to a mount point in a container.</span></span> <span data-ttu-id="018ec-136">繫結裝載的限制比磁碟區更多，且具有某些重要的安全性問題，因此磁碟區是建議的選項。</span><span class="sxs-lookup"><span data-stu-id="018ec-136">Bind mounts have more limitations than volumes and some important security issues, so volumes are the recommended option.</span></span>

<span data-ttu-id="018ec-137">**tmpfs 裝載**基本上就是虛擬資料夾，只存在於主機的記憶體中，永遠不會寫入檔案系統。</span><span class="sxs-lookup"><span data-stu-id="018ec-137">**tmpfs mounts** are basically virtual folders that live only in the host's memory and are never written to the filesystem.</span></span> <span data-ttu-id="018ec-138">它們既快速又安全，但會使用記憶體，且僅適用於非持續性資料。</span><span class="sxs-lookup"><span data-stu-id="018ec-138">They are fast and secure but use memory and are only meant for non-persistent data.</span></span>

<span data-ttu-id="018ec-139">如圖 4-5 所示，一般 Docker 磁碟區可儲存在容器本身之外，但必須在主機伺服器或 VM 的實體界限內。</span><span class="sxs-lookup"><span data-stu-id="018ec-139">As shown in Figure 4-5, regular Docker volumes can be stored outside of the containers themselves but within the physical boundaries of the host server or VM.</span></span> <span data-ttu-id="018ec-140">不過，Docker 容器無法從一部主機伺服器或 VM 存取另一部主機伺服器或 VM 的磁碟區。</span><span class="sxs-lookup"><span data-stu-id="018ec-140">However, Docker containers can't access a volume from one host server or VM to another.</span></span> <span data-ttu-id="018ec-141">換句話說，使用這些磁碟區，將無法管理在不同 Docker 主機上執行容器間所共用的資料，雖然仍可透過支援遠端主機的磁碟區驅動程式來達到此目的。</span><span class="sxs-lookup"><span data-stu-id="018ec-141">In other words, with these volumes, it isn't possible to manage data shared between containers that run on different Docker hosts, although it could be achieved with a volume driver that supports remote hosts.</span></span>

![<span data-ttu-id="018ec-142">磁碟區可以在容器間共用，但僅限位於相同的主機，除非您使用支援遠端主機的遠端驅動程式。</span><span class="sxs-lookup"><span data-stu-id="018ec-142">Volumes can be shared between containers, but only in the same host, unless you use a remote driver that supports remote hosts.</span></span> ](./media/image5.png)

<span data-ttu-id="018ec-143">**圖 4-5**：</span><span class="sxs-lookup"><span data-stu-id="018ec-143">**Figure 4-5**.</span></span> <span data-ttu-id="018ec-144">容器式應用程式的磁碟區和外部資料來源</span><span class="sxs-lookup"><span data-stu-id="018ec-144">Volumes and external data sources for container-based applications</span></span>

<span data-ttu-id="018ec-145">此外，當 Docker 容器是由協調器所管理時，容器可能會根據叢集所執行的最佳化，在主機之間「移動」。</span><span class="sxs-lookup"><span data-stu-id="018ec-145">In addition, when Docker containers are managed by an orchestrator, containers might "move" between hosts, depending on the optimizations performed by the cluster.</span></span> <span data-ttu-id="018ec-146">因此，不建議您使用資料磁碟區來儲存商務資料。</span><span class="sxs-lookup"><span data-stu-id="018ec-146">Therefore, it isn't recommended that you use data volumes for business data.</span></span> <span data-ttu-id="018ec-147">但針對追蹤檔案、時態性檔案或不影響商務資料一致性的類似檔案而言，這會是不錯的機制。</span><span class="sxs-lookup"><span data-stu-id="018ec-147">But they're a good mechanism to work with trace files, temporal files, or similar that will not impact business data consistency.</span></span>

<span data-ttu-id="018ec-148">**遠端資料來源和快取**工具 (例如 Azure SQL Database、Azure Cosmos DB) 或遠端快取 (例如 Redis) 可用於容器化應用程式，就像是在沒有容器時用於開發一樣。</span><span class="sxs-lookup"><span data-stu-id="018ec-148">**Remote data sources and cache** tools like Azure SQL Database, Azure Cosmos DB, or a remote cache like Redis can be used in containerized applications the same way they are used when developing without containers.</span></span> <span data-ttu-id="018ec-149">此方法經過實證，可儲存商務應用程式資料。</span><span class="sxs-lookup"><span data-stu-id="018ec-149">This is a proven way to store business application data.</span></span>

<span data-ttu-id="018ec-150">**Azure 儲存體。**</span><span class="sxs-lookup"><span data-stu-id="018ec-150">**Azure Storage.**</span></span> <span data-ttu-id="018ec-151">商務資料通常需要放在外部資源或資料庫中，例如 Azure 儲存體。</span><span class="sxs-lookup"><span data-stu-id="018ec-151">Business data usually will need to be placed in external resources or databases, like Azure Storage.</span></span> <span data-ttu-id="018ec-152">具體來說，Azure 儲存體會提供下列雲端服務：</span><span class="sxs-lookup"><span data-stu-id="018ec-152">Azure Storage, in concrete, provides the following services in the cloud:</span></span>

- <span data-ttu-id="018ec-153">Blob 儲存體可儲存非結構化的物件資料。</span><span class="sxs-lookup"><span data-stu-id="018ec-153">Blob storage stores unstructured object data.</span></span> <span data-ttu-id="018ec-154">Blob 可以是任何類型的文字或二進位資料，例如文件或媒體檔案 (影像、音訊和視訊檔案)。</span><span class="sxs-lookup"><span data-stu-id="018ec-154">A blob can be any type of text or binary data, such as document or media files (images, audio, and video files).</span></span> <span data-ttu-id="018ec-155">Blob 儲存體也稱為物件儲存體。</span><span class="sxs-lookup"><span data-stu-id="018ec-155">Blob storage is also referred to as Object storage.</span></span>

- <span data-ttu-id="018ec-156">檔案儲存體為使用標準 SMB 通訊協定的舊版應用程式提供共用存放裝置。</span><span class="sxs-lookup"><span data-stu-id="018ec-156">File storage offers shared storage for legacy applications using standard SMB protocol.</span></span> <span data-ttu-id="018ec-157">Azure 虛擬機器和雲端服務可透過掛接共用，在應用程式元件之間共用檔案資料。</span><span class="sxs-lookup"><span data-stu-id="018ec-157">Azure virtual machines and cloud services can share file data across application components via mounted shares.</span></span> <span data-ttu-id="018ec-158">內部部署應用程式可透過檔案服務 REST API，來存取共用中的檔案資料。</span><span class="sxs-lookup"><span data-stu-id="018ec-158">On-premises applications can access file data in a share via the File service REST API.</span></span>

- <span data-ttu-id="018ec-159">表格儲存體可儲存結構化的資料集。</span><span class="sxs-lookup"><span data-stu-id="018ec-159">Table storage stores structured datasets.</span></span> <span data-ttu-id="018ec-160">表格儲存體是 NoSQL 索引鍵屬性資料存放區，可快速開發及存取大量資料。</span><span class="sxs-lookup"><span data-stu-id="018ec-160">Table storage is a NoSQL key-attribute data store, which allows rapid development and fast access to large quantities of data.</span></span>

<span data-ttu-id="018ec-161">**關聯式資料庫和 NoSQL 資料庫。**</span><span class="sxs-lookup"><span data-stu-id="018ec-161">**Relational databases and NoSQL databases.**</span></span> <span data-ttu-id="018ec-162">外部資料庫的選擇有許多，從關聯式資料庫 (例如 SQL Server、PostgreSQL、Oracle) 到 NoSQL 資料庫 (例如 Azure Cosmos DB、MongoDB) 等等。這些資料庫由於是完全不同的主題，因此不會在本指南中說明。</span><span class="sxs-lookup"><span data-stu-id="018ec-162">There are many choices for external databases, from relational databases like SQL Server, PostgreSQL, Oracle, or NoSQL databases like Azure Cosmos DB, MongoDB, etc. These databases are not going to be explained as part of this guide since they are in a completely different subject.</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="018ec-163">[上一頁](containerize-monolithic-applications.md)
>[下一頁](service-oriented-architecture.md)</span><span class="sxs-lookup"><span data-stu-id="018ec-163">[Previous](containerize-monolithic-applications.md)
[Next](service-oriented-architecture.md)</span></span>