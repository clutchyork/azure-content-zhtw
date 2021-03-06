<properties
	pageTitle="平台支援的 IaaS 資源移轉 (從傳統移轉至 Azure Resource Manager) | Microsoft Azure"
	description="這篇文章提供平台支援之資源移轉 (從傳統移轉至 Azure Resource Manager) 的逐步解說"
	services="virtual-machines-windows"
	documentationCenter=""
	authors="mahthi"
	manager="drewm"
	editor=""
	tags="azure-resource-manager"/>

<tags
	ms.service="virtual-machines-windows"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="vm-windows"
	ms.devlang="na"
	ms.topic="article"
	ms.date="05/04/2016"
	ms.author="mahthi"/>

# 平台支援的 IaaS 資源移轉 (從傳統移轉至 Azure Resource Manager)

自我們宣佈在 Azure Resource Manager 之下支援虛擬機器算起已將近一年。您可以在這裡深入了解這段時間內的進展和它支援的其他功能。此外，我們也已提供指引，說明如何使用虛擬網路網站對網站閘道，讓來自兩個部署模型的資源做最佳連接並在您的訂用帳戶中並存。本文將說明如何讓基礎結構即服務 (IaaS) 資源從傳統移轉至 Resource Manager。

## 移轉目標

隨著新模型的發行，您將可以部署、管理及監視資源群組中的相關服務。Resource Manager 除了可讓您透過範本部署複雜的應用程式之外，還可使用 VM 擴充功能來設定虛擬機器，並且納入了存取管理和標記功能。它也可將虛擬機器的可調整平行部署納入至可用性設定組內。此外，新模型也針對計算、網路及儲存體個別提供生命週期管理功能。最後，將焦點放在藉由在虛擬網路中強制使用虛擬機器的方式，預設啟用安全性。

在 Azure Resource Manager 之下，針對來自傳統部署模型的幾乎所有功能，都有提供計算、網路及儲存體支援。由於 Azure Resource Manager 具備這項新的能力，而且部署基礎也不斷成長，因此我們希望客戶能夠移轉傳統部署模型中的現有部署。

## 移轉之後對自動化及工具的變更

在將資源從傳統模型移轉至 Resource Manager 模型的過程中，您將必須更新現有的自動化或工具，以確保它在移轉之後仍可繼續運作。

## 將 IaaS 資源從傳統移轉至 Resource Manager 的意義

在深入了解詳細資料之前，我們想要先簡短地說明 IaaS 資源的資料平面和管理平面作業之間的差異。了解這些差異非常重要，因為這可說明我們打算如何支援移轉。

- 「管理平面」說明進入管理平面或 API 以修改資源的呼叫。例如，建立 VM、重新啟動 VM 以及將虛擬網路更新成使用新子網路等作業皆可管理執行中的資源。它們並不直接影響對執行個體的連線。
- 「資料平面」(應用程式) 說明應用程式本身的執行階段，並牽涉到與不通過 Azure API 的執行個體進行互動。不論是存取您的網站，還是從執行中的 SQL Server 或 MongoDB 伺服器提取資料，皆會被視為是資料平面或應用程式互動。從儲存體帳戶複製 Blob 以及存取公用 IP 位址以透過 RDP 或 SSH 進入虛擬機器也屬於資料平面。這些作業會讓應用程式繼續跨計算、網路和儲存體執行。

>[AZURE.NOTE] 在一些移轉案例中，我們會將您的虛擬機器停止、解除配置及重新啟動。這會造成短暫的資料平面停機時間。

## 支援的移轉範圍

有三個移轉範圍，主要是針對計算、網路和儲存體。

### 移轉 (不在虛擬網路中的) 虛擬機器

在 Resource Manager 部署模型中，我們預設會實施應用程式安全性。在 Resource Manager 模型中，所有 VM 都必須在虛擬網路內。因此，在移轉過程中，我們會將 VM 重新啟動 (`Stop`、`Deallocate` 及 `Start`)。您有兩個虛擬網路選項：

- 您可以要求平台建立新的虛擬網路，然後將虛擬機器移轉到新的虛擬網路。
- 您也可以將虛擬機器移轉到 Resource Manager 中的現有虛擬網路。

>[AZURE.NOTE] 在此移轉範圍內，移轉期間可能會有一段時間不允許進行管理平面和資料平面作業。

### 移轉 (虛擬網路中的) 虛擬機器

在此範圍內，就大多數 VM 組態而言，我們只是在傳統部署模型和 Resource Manager 部署模型之間移轉中繼資料。基礎 VM 會在相同硬體、相同網路上，使用相同的儲存體來執行。因此，當我們提到將中繼資料從傳統移轉至 Resource Manager 時，移轉期間可能會有一段時間不允許進行管理平面作業。不過，資料平面將會繼續運作。也就是說，在 VM (傳統) 上執行的應用程式，不會在移轉期間造成停機時間。

目前來說，下列組態還不受支援。如果我們在未來新增對它們的支援，則此組態中的某些 VM 可能會造成停機時間 (將會經歷停止、解除配置及重新啟動作業)。

-	您在單一雲端服務中有多個可用性設定組。
-	您在單一雲端服務中有一或多個可用性設定組，以及不在可用性設定組中的 VM。

>[AZURE.NOTE] 在此移轉範圍內，移轉期間可能會有一段時間不允許進行管理平面作業。針對上述的某些組態，這將會造成資料平面停機時間。

### 儲存體帳戶移轉

為了讓移轉順暢進行，我們已啟用在傳統儲存體帳戶中部署 Resource Manager VM 的功能。透過這項功能，您就可以移轉計算和網路資源，且應該不受儲存體帳戶限制。移轉虛擬機器和虛擬網路後，您必須移轉儲存體帳戶以完成移轉程序。

>[AZURE.NOTE] Resource Manager 部署模型沒有傳統映像和磁碟的概念。移轉儲存體帳戶時，這些不會顯示在 Resource Manager 堆疊中，但是備份 VHD 將保留在儲存體帳戶中。

## 不支援的功能和組態

目前我們不支援某些功能和組態。下列各節說明我們對這些功能和組態的相關建議。

### 不支援的功能

目前不支援下列功能。您可以視需要移除這些設定、移轉 VM，然後再於 Resource Manager 部署模型中重新啟用這些設定。

資源提供者 | 功能
---------- | ------------
計算 | 未關聯的虛擬機器磁碟。
計算 | 虛擬機器映像。
網路 | 未關聯的保留 IP (如果未連接至 VM)。支援已連接至 VM 的保留 IP。
網路 | 未關聯的網路安全性群組 (如果未連接至虛擬網路或網路介面)。支援虛擬網路所參考的 NSG。
網路 | 端點 ACL。
網路 | 虛擬網路閘道 (網站間、Azure ExpressRoute、點對站)。

### 不支援的組態

目前不支援下列組態。

服務 | 組態 | 建議
---------- | ------------ | ------------
Resource Manager | 傳統資源的「角色型存取控制」(RBAC) | 由於資源的 URI 在移轉後會經過修改，因此建議您規劃需要在移轉後進行的 RBAC 原則更新。
計算 | 與 VM 關聯的多個子網路 | 您應該將子網路組態更新為只參考子網路。
計算 | 隸屬於虛擬網路但未獲指派明確子網路的虛擬機器。 | 您可以選擇刪除此 VM。
計算 | 具有警示、自動調整原則的虛擬機器 | 目前，移轉可以進行，但這些設定將遭到捨棄。因此強烈建議您在進行移轉之前先評估您的環境。或者，您也可以在移轉完成之後重新設定警示設定。
計算 | XML VM 擴充功能 (Visual Studio 偵錯工具、Web Deploy 及遠端偵錯) | 不支援此做法。建議您先從虛擬機器中移除這些擴充功能，再繼續移轉。
計算 | 使用進階儲存體進行開機診斷 | 請先停用 VM 的「開機診斷」功能，再繼續進行移轉。您可以在移轉完成之後，於 Resource Manager 堆疊中重新啟用開機診斷。此外，應該將用於快照和序列記錄檔的 Blob 刪除，這樣您就不再需要支付這些 Blob 的費用。
計算 | 包含 Web 角色/背景工作角色的雲端服務 | 目前不支援。
網路 | 包含虛擬機器和 Web 角色/背景工作角色的虛擬網路 | 目前不支援。
Azure App Service | 包含 App Service 環境的虛擬網路 | 目前不支援。
Azure HDInsight | 包含 HDInsight 服務的虛擬網路 | 目前不支援。
Microsoft Dynamics 週期服務 | 包含「Dynamics 週期服務」所管理之虛擬機器的虛擬網路 | 目前不支援。

## 移轉體驗

開始進行移轉體驗之前，強烈建議您遵循下列幾點：

- 確定您要移轉的資源未使用任何不支援的功能或組態。在大部分情況下，平台會偵測這些問題，並擲回錯誤。
- 如果您有不在虛擬網路中的 VM，則在準備作業過程中，會將這些 VM 停止並解除配置。如果您不想遺失公用 IP 位址，請在觸發準備作業之前，請先了解如何保留 IP 位址。不過，如果 VM 位於虛擬網路中，則不會予以停止並解除配置。
- 規劃非上班時間的移轉，以便因應移轉期間可能發生的任何非預期失敗。
- 使用 PowerShell、命令列介面 (CLI) 命令或 REST API 來下載現行的 VM 組態，以便在完成準備步驟之後能夠更容易進行驗證。
- 先更新用來處理 Resource Manager 部署模型的自動化/作業化指令碼，再開始移轉。當資源處於已準備就緒狀態時，您可以視需要執行 GET 作業。
- 在移轉完成之後，評估傳統 IaaS 資源上設定的 RBAC 原則並備妥計劃。

移轉工作流程如下

![顯示移轉工作流程的螢幕擷取畫面](./media/virtual-machines-windows-migration-classic-resource-manager/migration-workflow.png)

>[AZURE.NOTE] 下列各節描述的所有作業都是等冪的。如果您有不支援的功能或組態錯誤以外的任何問題，建議您重新嘗試準備、中止或認可作業。如此，平台就會重新嘗試該動作。

### 驗證

驗證作業是移轉程序的第一個步驟。此步驟的目標是要在背景針對進行移轉的資源執行資料分析，如果資源能夠進行移轉，便傳回成功/失敗。

您將選取要進行移轉驗證的虛擬網路或託管服務 (如果它不是虛擬網路)。

* 如果資源不能進行移轉，平台將會列出不支援移轉該資源的所有原因。

### 準備

準備作業是移轉程序的第二個步驟。這個步驟的目標是要模擬將 IaaS 資源從傳統資源轉換為 Resource Manager 資源的轉換過程，並以並排方式呈現此過程以供您用視覺化方式檢視。

您將選取要為移轉做準備的虛擬網路或託管服務 (如果它不是虛擬網路)。

* 如果資源不能進行移轉，平台將會停止移轉程序並列出準備作業失敗的原因。
* 如果資源能夠進行移轉，平台會先鎖定進行移轉之資源的管理平面作業。例如，您會無法將資料磁碟新增至進行移轉的 VM。

接著，平台會開始將移轉中資源的中繼資料從傳統移轉至 Resource Manager。

準備作業完成之後，您將可以選擇在傳統和 Resource Manager 中將資源視覺化。針對傳統部署模型中的每個雲端服務，我們將會建立採用 `cloud-service-name>-migrated` 模式的資源群組名稱。

>[AZURE.NOTE] 在這個移轉階段中，不在傳統虛擬網路中的虛擬機器將會停止解除配置。

### 檢查 (手動或透過指令碼)

在檢查步驟中，您可以視需要使用您稍早下載的組態來驗證移轉是否正確無誤。或者，您也可以登入入口網站並抽樣檢查屬性和資源，以驗證中繼資料移轉是否正確無誤。

如果您移轉的是虛擬網路，則虛擬機器的大部分組態將不會重新啟動。對於這些 VM 上的應用程式，您可以確認應用程式是否仍會啟動並執行。

您可以測試您的監視/自動化和作業指令碼，以查看 VM 是否如預期方式運作，以及更新後的指令碼是否正常運作。請注意，當資源處於已準備就緒的狀態時，只支援 GET 作業。

系統並未設定您必須在哪個期限之前認可移轉。您可以在此狀態停留任何長度的時間。但請注意，這些資源的管理平面將會處於鎖定狀態，直到您進行中止或認可為止。

如果您發現任何問題，您一律可以中止移轉，並返回傳統部署模型。在您返回之後，我們將會開放對資源的平面管理作業，讓您可以在傳統部署模型中繼續對這些 VM 進行一般作業。

### 中止

「中止」是選擇性步驟，您可以使用此步驟將您的變更還原至傳統部署模型並停止移轉。

>[AZURE.NOTE] 在您觸發了認可作業之後，就無法執行此作業。

### 認可

完成驗證之後，您便可以認可移轉。資源將不會再出現在傳統部署模型中，而只有在 Resource Manager 部署模型中才能使用這些資源。這也意謂著只能在新的入口網站中管理已移轉的資源。

>[AZURE.NOTE] 這是一種等冪作業。如果失敗，建議您重試幾次。如果持續發生失敗，請建立支援票證，或是在我們的 [VM 論壇](https://social.msdn.microsoft.com/Forums/azure/zh-TW/home?forum=WAVirtualMachinesforWindows)上建立一篇標籤為 ClassicIaaSMigration 的論壇文章。

## 常見問題集

**此移轉計劃是否會影響任何在 Azure 虛擬機器上執行的現有服務或應用程式？**

否。VM (傳統) 是在全面可用性方面完全受支援的服務。您可以繼續使用這些資源以擴展您在 Microsoft Azure 上的使用量。

**如果我最近沒有移轉的打算，我的 VM 會出現什麼狀況？**

我們並未要淘汰現有的傳統 API 和資源模型。我們想要將 Resource Manager 部署模型所提供的進階功能納入考量，讓移轉變簡單。強烈建議您檢閱 Resource Manager 下 IaaS 所包含的[一些進展](virtual-machines-windows-compare-deployment-models.md)。

**對於我現有的工具來說，此移轉計劃有何意義？**

將您的工具更新為 Resource Manager 部署模型，是您在移轉計畫中必須說明的最重要變更之一。

**管理平面的停機時間會持續多久？**

依您所移轉的資源數目而定。就較小型部署 (幾十個 VM) 而言，整個移轉作業應該不會超過一小時。如果是大規模部署 (數百個 VM)，則移轉可能需要花費幾個小時。

**在 Resource Manager 中認可移轉中的資源之後，是否還可以回復？**

只要資源處於已準備就緒狀態，您就可以中止移轉。透過認可作業成功移轉資源之後，即不支援復原。

**如果認可作業失敗，是否可以將移轉復原？**

如果認可作業失敗，就無法中止移轉。所有移轉作業 (包括認可作業) 都是等冪的。因此，建議您稍後再重試作業。如果仍然遇到錯誤，請建立支援票證，或是在我們的 [VM 論壇](https://social.msdn.microsoft.com/Forums/azure/zh-TW/home?forum=WAVirtualMachinesforWindows)上建立一篇標籤為 ClassicIaaSMigration 的論壇文章。

**如果我必須使用 Resource Manager 下的 IaaS，是否必須購買另一條 ExpressRoute 線路？**

否。我們最近已讓 [ExpressRoute 線路跨傳統和 Resource Manager 並存](../expressroute/expressroute-howto-coexist-resource-manager.md)。如果您已有 ExpressRoute 線路，就不需要購買新線路。

**如果我已為傳統 IaaS 資源設定角色型存取控制原則，該怎麼辦？**

在移轉期間，資源會從傳統轉換至 Resource Manager。因此，建議您規劃需要在移轉後進行的 RBAC 原則更新。

**如果我目前使用 Azure Site Recovery 或 Azure 備份，該怎麼辦？**

最近已針對 Resource Manager 下的 VM 新增 Azure Site Recovery 及「備份」支援。我們也正在努力啟用可支援將 VM 移轉至 Resource Manager 的功能。目前，如果您使用這些功能，則建議您先不要執行移轉。

**我是否可以驗證訂用帳戶或資源，以查看是否能夠移轉它們？**

是。在平台支援的移轉選項中，為移轉做準備的第一個步驟就是驗證資源是否能夠進行移轉。如果驗證作業失敗，您將會收到無法完成移轉的所有原因相關訊息。

**如果我在準備要移轉的 IaaS 資源時遇到配額錯誤，會發生什麼事？**

建議您中止移轉，然後登錄支援要求，以在您要移轉 VM 的區域中增加配額。在配額要求獲得核准之後，您便可以開始重新執行移轉步驟。

**如何回報問題？**

請在我們的 [VM 論壇](https://social.msdn.microsoft.com/Forums/azure/zh-TW/home?forum=WAVirtualMachinesforWindows)上，使用關鍵字 ClassicIaaSMigration 來張貼有關移轉的問題和疑惑。建議您將所有問題都張貼在此論壇上。如果您持有支援合約，也歡迎您登錄支援票證。

**如果我不喜歡平台在移轉期間選擇的資源名稱，該怎麼辦？**

您在傳統部署模型中明確提供名稱的所有資源，在移轉期間都會獲得保留。在某些情況下，則會建立新的資源。例如︰會為每個 VM 建立一個網路介面。此時此刻，我們不支援控制移轉期間所建立的新資源名稱。請在 [Azure 意見反應論壇](http://feedback.azure.com)上針對這項功能進行投票。

**我收到訊息*「VM 回報的整體代理程式狀態為「未就緒」。因此，無法移轉 VM。請確認 VM 代理程式回報的整體代理程式狀態為「就緒」」*，或*「VM 包含未從 VM 回報狀態的擴充功能。因此，無法移轉此 VM。」***

當 VM 無法連出到網際網路時，就會收到此訊息。VM 代理程式會使用連出連線來連接到 Azure 儲存體帳戶，來每隔 5 分鐘更新一次代理程式狀態。

## 後續步驟
既然您已了解如何將傳統 IaaS 資源移轉至 Resource Manager，您便可以開始移轉資源。

- [平台支援的從傳統移轉至 Azure Resource Manager 的技術深入探討](virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)
- [使用 PowerShell 將 IaaS 資源從傳統移轉至 Azure Resource Manager](virtual-machines-windows-ps-migration-classic-resource-manager.md)
- [使用 CLI 將 IaaS 資源從傳統移轉至 Azure Resource Manager](virtual-machines-linux-cli-migration-classic-resource-manager.md)
- [使用社群 PowerShell 指令碼將傳統虛擬機器複製到 Azure Resource Manager](virtual-machines-windows-migration-scripts.md)

<!---HONumber=AcomDC_0720_2016-->