<properties
   pageTitle="使用資源管理員及 PowerShell 建立和修改 ExpressRoute 線路 | Microsoft Azure"
   description="本文說明如何建立、佈建、驗證、更新、刪除和取消佈建 ExpressRoute 線路。"
   documentationCenter="na"
   services="expressroute"
   authors="ganesr"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="07/19/2016"
   ms.author="ganesr"/>


# 建立和修改 ExpressRoute 線路

> [AZURE.SELECTOR]
[Azure Portal - Resource Manager](expressroute-howto-circuit-portal-resource-manager.md)
[PowerShell - Resource Manager](expressroute-howto-circuit-arm.md)
[PowerShell - Classic](expressroute-howto-circuit-classic.md)


本文說明如何使用 Windows PowerShell Cmdlet 和 Azure Resource Manager 部署模型建立 Azure ExpressRoute 線路。本文也會示範如何檢查循環的狀態、加以更新，或是加以刪除及取消佈建。

**關於 Azure 部署模型**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## 開始之前


- 取得最新版的 Azure PowerShell 模組 (至少版本 1.0)。請遵循[如何安裝和設定 Azure PowerShell](../powershell-install-configure.md) 中的指示，來取得如何設定您的電腦以使用 PowerShell 模組的逐步指引。

- 開始設定之前，請檢閱[必要條件](expressroute-prerequisites.md)和[工作流程](expressroute-workflows.md)。

## 建立和佈建 ExpressRoute 線路

### 1\.登入您的 Azure 帳戶並且選取您的訂用帳戶

若要開始您的組態，請登入您的 Azure 帳戶。如需 PowerShell 的詳細資訊，請參閱[搭配使用 Windows PowerShell 與 Resource Manager](../powershell-azure-resource-manager.md)。使用下列範例來協助您連接：

	Login-AzureRmAccount

檢查帳戶的訂用帳戶：

	Get-AzureRmSubscription

選取您想要建立 ExpressRoute 線路的訂用帳戶：

	Select-AzureRmSubscription -SubscriptionId "<subscription ID>"

### 2\.取得支援的提供者、位置和頻寬清單

建立 ExpressRoute 線路之前，您需要有支援的連線提供者、位置和頻寬選項的清單。

PowerShell Cmdlet `Get-AzureRmExpressRouteServiceProvider` 會傳回此資訊，在稍後的步驟中將會用到：

	Get-AzureRmExpressRouteServiceProvider

請檢查是否列出您的連線服務提供者。記下下列資訊，因為您稍後在建立線路時將會用到：

- 名稱

- PeeringLocations

- BandwidthsOffered

您現在已經準備好建立 ExpressRoute 線路。

### 3\.建立 ExpressRoute 線路

若您還沒有資源群組，您必須在建立 ExpressRoute 線路之前建立一個。您可以執行下列命令來這麼做：


	New-AzureRmResourceGroup -Name "ExpressRouteResourceGroup" -Location "West US"


以下示範如何透過矽谷的 Equinix 建立 200-Mbps ExpressRoute 線路。如果您使用不同的提供者和不同的設定，請在您提出要求時替換成該資訊。下列是新服務金鑰的要求範例：

	New-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup" -Location "West US" -SkuTier Standard -SkuFamily MeteredData -ServiceProviderName "Equinix" -PeeringLocation "Silicon Valley" -BandwidthInMbps 200

請確定您指定正確的 SKU 層和 SKU 系列：

- SKU 層會判斷 ExpressRoute 標準或 ExpressRoute 高階附加元件是否啟用。您可以指定 [標準] 取得標準 SKU，或指定 [進階] 取得進階附加元件。

- SKU 系列決定計費類型。您可以指定 [已計量資料] 以採用計量付費數據傳輸方案，選取 [無限制資料] 以採用無限行動數據方案。請注意，您可以將計費類型從 [已計量資料] 變更為 [無限制資料]，但無法從 [無限制資料] 變更為 [已計量資料]。


>[AZURE.IMPORTANT] ExpressRoute 循環將會從發出服務金鑰時開始收費。請確定在連線提供者準備好佈建線路之後，再執行這項作業。

回應包含服務金鑰。您可以執行下列命令來取得所有參數的詳細描述：


	get-help New-AzureRmExpressRouteCircuit -detailed


### 4\.列出所有 ExpressRoute 循環

若要取得您已建立之所有 ExpressRoute 線路的清單，請執行 `Get-AzureRmExpressRouteCircuit` 命令：


	Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

回應看起來將會如下列範例所示：


	Name                             : ExpressRouteARMCircuit
	ResourceGroupName                : ExpressRouteResourceGroup
	Location                         : westus
	Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
	Etag                             : W/"################################"
	ProvisioningState                : Succeeded
	Sku                              : {
	                                     "Name": "Standard_MeteredData",
	                                     "Tier": "Standard",
	                                     "Family": "MeteredData"
	   		                           }
	CircuitProvisioningState          : Enabled
	ServiceProviderProvisioningState  : NotProvisioned
	ServiceProviderNotes              :
	ServiceProviderProperties         : {
	                                      "ServiceProviderName": "Equinix",
	                                      "PeeringLocation": "Silicon Valley",
	                                      "BandwidthInMbps": 200
	                                    }
	ServiceKey                        : **************************************
	Peerings                          : []

您隨時可以使用 `Get-AzureRmExpressRouteCircuit` Cmdlet 擷取此資訊。執行呼叫時，若未指定任何參數，將會列出所有線路。[服務金鑰] 欄位會列出您的服務金鑰。


	Get-AzureRmExpressRouteCircuit


回應看起來將會如下列範例所示：


	Name                             : ExpressRouteARMCircuit
	ResourceGroupName                : ExpressRouteResourceGroup
	Location                         : westus
	Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
	Etag                             : W/"################################"
	ProvisioningState                : Succeeded
	Sku                              : {
	                                     "Name": "Standard_MeteredData",
	                                     "Tier": "Standard",
	                                     "Family": "MeteredData"
	   		                           }
	CircuitProvisioningState         : Enabled
	ServiceProviderProvisioningState : NotProvisioned
	ServiceProviderNotes             :
	ServiceProviderProperties        : {
	                                     "ServiceProviderName": "Equinix",
	                                     "PeeringLocation": "Silicon Valley",
	                                     "BandwidthInMbps": 200
	   		                           }
	ServiceKey                       : **************************************
	Peerings                         : []


您可以執行下列命令來取得所有參數的詳細描述：


	get-help Get-AzureRmExpressRouteCircuit -detailed

### 5\.將服務金鑰傳送給連線提供者以進行佈建

[服務提供者佈建狀態] 提供服務提供者端目前的佈建狀態相關資訊。[狀態] 提供 Microsoft 端的狀態。如需線路佈建狀態的詳細資訊，請參閱[工作流程](expressroute-workflows.md#expressroute-circuit-provisioning-states)文章。

當您建立新的 ExpressRoute 線路時，線路會是下列狀態：


	ServiceProviderProvisioningState : NotProvisioned
	CircuitProvisioningState         : Enabled



當連線提供者正在為您啟用線路時，線路會變更為下列狀態：

	ServiceProviderProvisioningState : Provisioning
	Status                           : Enabled

若要能夠使用 ExpressRoute 線路，它必須處於下列狀態：

	ServiceProviderProvisioningState : Provisioned
	CircuitProvisioningState         : Enabled

### 6\.定期檢查循環金鑰的情況和狀態

檢查線路金鑰的情況和狀態將能讓您得知提供者是否已啟用您的線路。設定線路之後，[服務提供者佈建狀態] 會顯示為 [已佈建]，如下列範例所示：


	Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"


回應看起來將會如下列範例所示：


	Name                             : ExpressRouteARMCircuit
	ResourceGroupName                : ExpressRouteResourceGroup
	Location                         : westus
	Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
	Etag                             : W/"################################"
	ProvisioningState                : Succeeded
	Sku                              : {
	                                     "Name": "Standard_MeteredData",
	                                     "Tier": "Standard",
	                                     "Family": "MeteredData"
	                                   }
	CircuitProvisioningState         : Enabled
	ServiceProviderProvisioningState : Provisioned
	ServiceProviderNotes             :
	ServiceProviderProperties        : {
	                                     "ServiceProviderName": "Equinix",
	                                     "PeeringLocation": "Silicon Valley",
	                                     "BandwidthInMbps": 200
	   	                            }
	ServiceKey                       : **************************************
	Peerings                         : []

### 7\.建立路由組態

如需逐步指示，請參閱 [ExpressRoute 線路路由組態](expressroute-howto-routing-arm.md)一文以建立和修改線路對等。


>[AZURE.IMPORTANT] 這些指示只適用於由提供第 2 層連線服務的服務提供者所建立的線路。如果您使用的服務提供者是提供受管理的第 3 層服務 (通常是 IP VPN，如 MPLS)，您的連線提供者會為您設定和管理路由。

### 8\.將虛擬網路連結到 ExpressRoute 電路

接下來，將虛擬網路連結到 ExpressRoute 線路。當使用 Resource Manager 部署模型時，使用[將虛擬網路連結到 ExpressRoute 線路](expressroute-howto-linkvnet-arm.md)文章。

## 取得 ExpressRoute 線路的狀態

您隨時可以使用 `Get-AzureRmExpressRouteCircuit` Cmdlet 擷取此資訊。執行呼叫時，若未指定任何參數，將會列出所有線路。

	Get-AzureRmExpressRouteCircuit


回應將如以下範例所示：


	Name                             : ExpressRouteARMCircuit
	ResourceGroupName                : ExpressRouteResourceGroup
	Location                         : westus
	Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
	Etag                             : W/"################################"
	ProvisioningState                : Succeeded
	Sku                              : {
	                                     "Name": "Standard_MeteredData",
	                                     "Tier": "Standard",
	                                     "Family": "MeteredData"
	                                   }
	CircuitProvisioningState         : Enabled
	ServiceProviderProvisioningState : Provisioned
	ServiceProviderNotes             :
	ServiceProviderProperties        : {
	   		                             "ServiceProviderName": "Equinix",
	   		                             "PeeringLocation": "Silicon Valley",
	   		                             "BandwidthInMbps": 200
	   		                           }
	ServiceKey                       : **************************************
	Peerings                         : []


您可以透過將資源群組名稱和線路名稱做為參數傳遞至呼叫，取得特定 ExpressRoute 線路的資訊：


	Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"


回應看起來將會如下列範例所示：


	Name                             : ExpressRouteARMCircuit
	ResourceGroupName                : ExpressRouteResourceGroup
	Location                         : westus
	Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
	Etag                             : W/"################################"
	ProvisioningState                : Succeeded
	Sku                              : {
	                                     "Name": "Standard_MeteredData",
	   		                             "Tier": "Standard",
	   		                             "Family": "MeteredData"
	   		                           }
	CircuitProvisioningState         : Enabled
	ServiceProviderProvisioningState : Provisioned
	ServiceProviderNotes             :
	ServiceProviderProperties        : {
	                                     "ServiceProviderName": "Equinix",
	                                     "PeeringLocation": "Silicon Valley",
	                                     "BandwidthInMbps": 200
	   		                           }
	ServiceKey                       : **************************************
	Peerings                         : []


您可以執行下列命令來取得所有參數的詳細描述：

	get-help get-azurededicatedcircuit -detailed


## <a name="modify"></a>修改 ExpressRoute 線路

您可以修改 ExpressRoute 線路的某些屬性，而不會影響連線。

您可以執行下列工作，而無需中途停機：

- 啟用或停用 ExpressRoute 線路的 ExpressRoute 進階附加元件。
- 增加 ExpressRoute 線路的頻寬。請注意，不支援將循環的頻寬降級。
- 將計量方案從 [已計量資料] 變更為 [無限制資料]。請注意，不支援將計量方案從 [無限制資料] 變更為 [已計量資料]。
-  您可以啟用和停用 [允許傳統作業]。

如需限制的詳細資訊，請參閱 [ExpressRoute 常見問題集](expressroute-faqs.md)。

### 啟用 ExpressRoute 進階附加元件

您可以使用下列 PowerShell 程式碼片段，為現有的線路啟用 ExpressRoute 進階附加元件：

	$ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

	$ckt.Sku.Tier = "Premium"
	$ckt.sku.Name = "Premium_MeteredData"

	Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt


線路現在將啟用 ExpressRoute 進階附加功能。請注意，順利執行命令後，我們會開始向您收取進階附加元件功能的費用。

### 停用 ExpressRoute 進階附加元件

>[AZURE.IMPORTANT] 如果您使用的資源超出標準線路所允許的數量，這項作業可能會失敗。

請注意：

- 從進階降級為標準之前，您必須確定連結至線路的虛擬網路數目小於 10。如果您不這樣做，更新要求就會失敗，且我們會以進階費率計費。

- 您必須取消連結其他地理政治區域中的所有虛擬網路。如果您不這樣做，更新要求就會失敗，且我們會以進階費率計費。

- 就私用對等設定而言，路由表必須少於 4000 個路由。如果路由表大小超過 4000 個路由，BGP 工作階段將會中斷，而且在通告的首碼數目降到 4000 以下之前不會重新啟用。

您可以使用下列 PowerShell Cmdlet，為現有的線路停用 ExpressRoute 進階附加元件：


	$ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

	$ckt.Sku.Tier = "Standard"
	$ckt.sku.Name = "Standard_MeteredData"

	Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt


### 更新 ExpressRoute 線路頻寬

請查閱 [ExpressRoute 常見問題集](expressroute-faqs.md)，以取得提供者支援的頻寬選項。您可以挑選任何比現有線路規模還大的大小。

>[AZURE.IMPORTANT] 降低 ExpressRoute 線路的頻寬時必須中斷運作。頻寬降級需要取消佈建 ExpressRoute 線路，然後重新佈建新的 ExpressRoute 線路。

一旦決定需要的大小後，可以使用下列命令來重新調整線路大小。


	$ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

	$ckt.ServiceProviderProperties.BandwidthInMbps = 1000

	Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt


我們會在 Microsoft 端調整線路的大小。接下來，您必須連絡連線提供者，將他們的設定更新為符合這項變更。通知之後，我們會開始以更新的頻寬選項計費。


### 將 SKU 從計量移動為無限

您可以使用下列 PowerShell 程式碼片段變更 ExpressRoute 循環的 SKU：

	$ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

	$ckt.Sku.Family = "UnlimitedData"
	$ckt.sku.Name = "Premium_UnlimitedData"

	Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

### 控制對傳統和 Resource Manager 環境的存取  

請檢閱[將 ExpressRoute 線路從傳統部署模型移至 Resource Manager 部署模型](expressroute-howto-move-arm.md)中的指示。


## 刪除和取消佈建 ExpressRoute 線路

請注意：

- 您必須取消連結 ExpressRoute 循環的所有虛擬網路。如果此作業失敗，請確認是否有任何虛擬網路連結至循環。

- 如果已啟用 ExpressRoute 線路服務提供者佈建狀態，狀態會從已啟用狀態變成 [正在停用]。您必須與服務提供者一起合作，取消佈建他們那邊的線路。我們將繼續保留資源並向您收取費用，直到線路服務提供者完成取消佈建並通知我們。

- 若服務提供者在您執行上述 Cmdlet 之前已取消佈建線路 (服務提供者佈建狀態設定為 [未佈建])，我們將會取消佈建線路並停止向您收費。

您可以執行下列命令來刪除 ExpressRoute 線路：

	Remove-AzureRmExpressRouteCircuit -ResourceGroupName "ExpressRouteResourceGroup" -Name "ExpressRouteARMCircuit"



## 後續步驟

建立好線路後，請務必執行下列作業：

- [建立和修改 ExpressRoute 線路的路由](expressroute-howto-routing-arm.md)
- [將虛擬網路連結至 ExpressRoute 線路](expressroute-howto-linkvnet-arm.md)

<!---HONumber=AcomDC_0720_2016-->