<properties
   pageTitle="使用 Resource Manager 範本建立 VNet 對等互連 | Microsoft Azure"
   description="了解如何在 Resource Manager 中使用範本建立虛擬網路對等互連。"
   services="virtual-network"
   documentationCenter=""
   authors="narayanannamalai"
   manager="jefco"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/15/2016"
   ms.author="telmos"/>

# 使用 Resource Manager 範本建立 VNet 對等互連

[AZURE.INCLUDE [virtual-networks-create-vnet-selectors-arm-include](../../includes/virtual-networks-create-vnetpeering-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnetpeering-intro-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-basic-include](../../includes/virtual-networks-create-vnetpeering-scenario-basic-include.md)]

若要使用 Resource Manager 範本建立 VNet 對等互連，請依照下列步驟︰

1. 如果您從未用過 Azure PowerShell，請參閱[如何安裝和設定 Azure PowerShell](../powershell-install-configure.md)，並遵循其中的所有指示登入 Azure，然後選取您的訂用帳戶。

注意︰用於管理 VNet 對等互連的 PowerShell Cmdlet 隨附於 [Azure PowerShell 1.6。](http://www.powershellgallery.com/packages/Azure/1.6.0)

2. 下節依據上述案例顯示 VNet1 至 VNet2 的 VNet 對等互連連結之定義。將這裡的內容複製到另一檔案，並儲存成檔案 VNetPeeringVNet1.json

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet1/LinkToVNet2",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "[resourceId('Microsoft.Network/virtualNetworks', 'vnet2')]"       
        }
            }
            }
        ]
        }
    

3. 下節依據上述案例顯示 VNet2 至 VNet1 的 VNet 對等互連連結之定義。將這裡的內容複製到另一檔案，並儲存成檔案 VNetPeeringVNet2.json

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet2/LinkToVNet1",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "[resourceId('Microsoft.Network/virtualNetworks', 'vnet1')]"       
                }
            }
            }
        ]
        }

如同上述範本，VNet 對等互連有幾個可設定的屬性︰

|選項|說明|預設值|
|:-----|:----------|:------|
|AllowVirtualNetworkAccess|對等 VNet 的位址空間是否包含做為 Virtual\_network 標籤內的一部分|是|
|AllowForwardedTraffic|允許接受或卸除非來自對等互連 VNet 的流量|否|
|AllowGatewayTransit|允許對等 VNet 使用您的 VNet 閘道|否|
|UseRemoteGateways|使用對等的 VNet 閘道。對等 VNet 必須設定閘道，並選取 AllowGatewayTransit。如果閘道已設定則無法使用此選項|否|

VNet 對等互連中每個連結都具有一組上述的屬性。例如，您可以為 VNet1 至 VNet2 的 VNet 對等互連連結將 AllowVirtualNetworkAccess 設為 True，並為另一個方向的 VNet 對等互連連結將其設為 False。


4. 若要部署範本檔案，您可以執行 Cmdlet New-AzureRmResourceGroupDeployment 以建立或更新部署。如需使用 Resource Manager 範本的詳細資訊，請參閱此 [文章](../resource-group-template-deploy.md)

        New-AzureRmResourceGroupDeployment -ResourceGroupName <resource group name> -TemplateFile <template file path> -DeploymentDebugLogLevel all

> [AZURE.NOTE] 請適當取代資源群組名稱和範本檔案。

根據上述案例的範例如下︰

        New-AzureRmResourceGroupDeployment -ResourceGroupName VNet101 -TemplateFile .\VNetPeeringVNet1.json -DeploymentDebugLogLevel all

輸出會顯示︰

        DeploymentName		: VNetPeeringVNet1
        ResourceGroupName	: VNet101
        ProvisioningState		: Succeeded
        Timestamp			: 7/26/2016 9:05:03 AM
        Mode			: Incremental
        TemplateLink		:
        Parameters			:
        Outputs			:
        DeploymentDebugLogLevel : RequestContent, ResponseContent

        New-AzureRmResourceGroupDeployment -ResourceGroupName VNet101 -TemplateFile .\VNetPeeringVNet2.json -DeploymentDebugLogLevel all

輸出會顯示︰

        DeploymentName		: VNetPeeringVNet2
        ResourceGroupName	: VNet101
        ProvisioningState		: Succeeded
        Timestamp			: 7/26/2016 9:07:22 AM
        Mode			: Incremental
        TemplateLink		:
        Parameters			:
        Outputs			:
        DeploymentDebugLogLevel : RequestContent, ResponseContent

5. 部署完成之後，您可以執行以下 Cmdlet 來檢視對等互連的狀態︰

        Get-AzureRmVirtualNetworkPeering -VirtualNetworkName VNet1 -ResourceGroupName VNet101 -Name linktoVNet2

    輸出會顯示︰

        Name			: LinkToVNet2
        Id				: /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/VNet101/providers/Microsoft.Network/virtualNetworks/VNet1/virtualNetworkPeerings/LinkToVNet2
        Etag			: W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName	: VNet101
        VirtualNetworkName	: VNet1
        ProvisioningState		: Succeeded
        RemoteVirtualNetwork	: {
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/VNet101/providers/Microsoft.Network/virtualNetworks/VNet2"
                                        }
        AllowVirtualNetworkAccess	: True
        AllowForwardedTraffic            : False
        AllowGatewayTransit              : False
        UseRemoteGateways                : False
        RemoteGateways                   : null
        RemoteVirtualNetworkAddressSpace : null

在此案例中建立對等互連之後，您應該可以將這兩個 Vnet 中的任一虛擬機器連接至另一虛擬機器。根據預設，AllowVirtualNetworkAccess 為 True，且 VNet 對等互連會佈建正確的 ACL 以允許 Vnet 之間的通訊。但您仍然可以套用 NSG 規則來封鎖例如特定子網路或虛擬機器之間的連線，以微調控制兩個虛擬網路之間的存取。如需建立 NSG 規則的詳細資訊，請參閱此 [文章](virtual-networks-create-nsg-arm-ps.md)。

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-crosssub-include](../../includes/virtual-networks-create-vnetpeering-scenario-crosssub-include.md)]

若要建立訂用帳戶之間的 VNet 對等互連，請依照下列步驟︰

1. 以訂用帳戶 A 的權限使用者 A 登入 Azure，執行 Cmdlet：

        New-AzureRmRoleAssignment -SignInName <UserB ID> -RoleDefinitionName "Network Contributor" -Scope /subscriptions/<Subscription-A-ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetwork/VNet5

這不是必要需求，即使使用者針對其個別的 Vnet 個別提出對等互連的要求，只要要求符合就可以建立對等互連。將另一個 VNet 的權限使用者新增為本機 VNet 使用者，可以更輕鬆進行設定。

2. 以訂用帳戶 B 的權限使用者 B 登入 Azure，執行 Cmdlet：

        New-AzureRmRoleAssignment -SignInName <UserA ID> -RoleDefinitionName "Network Contributor" -Scope /subscriptions/<Subscription-B-ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetwork/VNet3

3. 然後在使用者 A 的登入工作階段中，執行 Cmdlet

        New-AzureRmResourceGroupDeployment -ResourceGroupName VNet101 -TemplateFile .\VNetPeeringVNet3.json -DeploymentDebugLogLevel all

    以下是 JSON 檔案定義的方式。
    
        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet3/LinkToVNet5",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "/subscriptions/<Subscription-B-ID>/resourceGroups/<resource group name>/providers/Microsoft.Network/virtualNetworks/VNet5"
                }
            }
            }
        ]
        }
   
4. 然後在使用者 B 的登入工作階段中，執行 Cmdlet

        New-AzureRmResourceGroupDeployment -ResourceGroupName VNet101 -TemplateFile .\VNetPeeringVNet5.json -DeploymentDebugLogLevel all
   
   以下是 JSON 檔案定義的方式：

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet5/LinkToVNet3",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "/subscriptions/Subscription-A-ID /resourceGroups/<resource group name>/providers/Microsoft.Network/virtualNetworks/VNet3"
                }
            }
            }
        ]
        }
 
 在此案例中建立對等互連之後，您應可將不同訂用帳戶之間兩個 Vnet 中的任一虛擬機器連接至另一虛擬機器。

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-transit-include](../../includes/virtual-networks-create-vnetpeering-scenario-transit-include.md)]

1. 在此案例中，您可以部署以下範例範本以建立 VNet 對等互連，特別是您需要將 AllowForwardedTraffic 屬性設為 True，可讓已對等互連 VNet 中的網路虛擬設備傳送和接收流量。以下是建立從 HubVNet 至 VNet1 的 VNet 對等互連範本。請注意 AllowForwardedTraffic 設為 False

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "HubVNet/LinkToVNet1",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "[resourceId('Microsoft.Network/virtualNetworks', 'vnet1')]"       
                }
            }
            }
            }
        ]
        }

2. 以下是建立從 VNet1 至 HubVnet 的 VNet 對等互連範本。請注意 AllowForwardedTraffic 設為 True。

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet1/LinkToHubVNet",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": true,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "[resourceId('Microsoft.Network/virtualNetworks', 'HubVnet')]"       
                }
            }
            }
        ]
        }


3. 對等互連建立之後，您可以參考此 [文章](virtual-network-create-udr-arm-ps.md) 並定義使用者定義路徑(UDR)，透過虛擬設備將 VNet1 流量重新導向以使用其功能。當您在路徑中指定下個躍點位址時，可以將其設定為對等 VNet HubVNet 中的虛擬設備 IP 位址

<!---HONumber=AcomDC_0803_2016-->