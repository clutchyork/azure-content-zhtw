
<properties
	pageTitle="Azure CLI 搭配資源管理員 | Microsoft Azure"
	description="使用 Azure 命令列介面 (CLI) 將多個資源作為資源群組部署。"
	editor=""
	manager="timlt"
	documentationCenter=""
	authors="dlepow"
	services="azure-resource-manager"/>

<tags
	ms.service="azure-resource-manager"
	ms.workload="multiple"
	ms.tgt_pltfrm="vm-multiple"
	ms.devlang="na"
	ms.topic="article"
	ms.date="04/20/2016"
	ms.author="danlep"/>

# 搭配使用 Mac、Linux 和 Windows 適用的 Azure CLI 與 Azure 資源管理員

> [AZURE.SELECTOR]
- [入口網站](azure-portal/resource-group-portal.md)
- [Azure CLI](xplat-cli-azure-resource-manager.md)
- [Azure PowerShell](powershell-azure-resource-manager.md)
- [.NET](https://azure.microsoft.com/documentation/samples/resource-manager-dotnet-resources-and-groups/)
- [Java](https://azure.microsoft.com/documentation/samples/resources-java-manage-resource-group/)
- [節點](https://azure.microsoft.com/documentation/samples/resource-manager-node-resources-and-groups/)
- [Python](https://azure.microsoft.com/documentation/samples/resource-manager-python-resources-and-groups/)
- [Ruby](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-resources-and-groups/)


此文介紹在 Azure Resource Manager 模式中使用 Azure 命令列介面 (Azure CLI) 建立及管理 Azure 資源的常見方法。

>[AZURE.NOTE] 若要使用命令列建立與管理 Azure 資源，您必須有 Azure 訂用帳戶 ([免費 Azure 帳戶](https://azure.microsoft.com/free/))。您還需要[安裝 Azure CLI](xplat-cli-install.md)，並[登入才能使用與您帳戶相關聯的 Azure 資源](xplat-cli-connect.md)。如果上述事項都已完成，您就能開始進行了。

## Azure 資源

使用 Azure 資源管理員將一組「資源」(使用者管理的實體，例如虛擬機器、資料庫伺服器、資料庫或網站) 做為單一邏輯單位建立及管理，或做為「資源群組」建立及管理。

Azure Resource Manager 的優點之一就是您可以用「宣告」的方式建立 Azure 資源，亦即描述可在 JSON「範本」中部署的資源群組的結構與關係。範本會識別可在執行命令時內嵌的參數，或儲存於個別 JavaScript 物件標記法 (JSON) 的參數檔案中。如此一來，您只要提供不同的參數，就能使用相同的範本輕易建立新資源。例如，建立網站的範本將具有網站名稱的參數、網站將放置的區域，以及其他共同設定。

使用範本修改或建立群組時，會建立_部署_，接著將其套用至群組。如需 Azure 資源管理員的詳細資訊，請參閱 [Azure 資源管理員概觀](resource-group-overview.md)。

建立部署之後，您可以在命令列上以命令管理個別的資源，就如同在傳統部署模型中一樣。例如，使用 Resource Manager 模式中的 CLI 命令來啟動、停止或刪除資源，例如 [Azure Resource Manager 虛擬機器](./virtual-machines/virtual-machines-linux-cli-deploy-templates.md)。

## 驗證

若要透過 Azure CLI 使用 Azure Resource Manager，目前您必須使用 `azure login` 命令，然後指定由 Azure Active Directory 所管理的帳戶 － 公司或學校帳戶 (組織帳戶) 或 Microsoft 帳戶，讓 Microsoft Azure 進行驗證。透過 .publishsettings 檔案安裝的憑證無法在此模式中進行驗證。

如需 Microsoft Azure 驗證的詳細資訊，請參閱[從 Azure CLI 連線至 Azure 訂用帳戶](xplat-cli-connect.md)。

>[AZURE.NOTE] 當您使用由 Azure Active Directory 管理的帳戶時，您也可以使用 Azure 角色型存取控制 (RBAC) 管理 Azure 資源的存取與使用。如需詳細資料，請參閱[角色型存取控制](./active-directory/role-based-access-control-configure.md)。

## 設定資源管理員模式

因為 CLI 預設不是資源管理員模式，請使用下列命令來啟用 Azure CLI Resource Manager 命令。

	azure config mode arm

## 尋找位置

大部分的 Azure 資源管理員命令需要有可用來建立或尋找資源的有效位置。您可以使用下列命令來尋找不同 Azure 資源的所有可用位置。

	azure location list

這會列出適用的 Azure 區域，例如「美國西部」、「美國東部」等等。如需可用的資源提供者及其可用位置的詳細資料，請使用 `azure provider list` 命令後面接續 `azure provider show` 命令。例如，下列命令會列出 Azure 容器服務的位置︰

    azure provider show Microsoft.ContainerService 

## 建立資源群組

資源群組是網路、儲存體和計算資源等資源的邏輯群組。資源管理員模式中幾乎所有命令都需要資源群組。例如，您可以使用下列命令在美國西部區域建立名為「testRG」的資源群組。

	azure group create -n "testRG" -l "West US"

當使用範本來啟動 Ubuntu VM 時，您稍後將會部署到此「testRG」資源群組。一旦建立資源群組後，您便可以加入資源，例如虛擬機器和網路或儲存體。


## 使用資源群組範本

### 尋找和設定資源群組範本

使用範本時，您可以[建立自己的範本](resource-group-authoring-templates.md)，或使用其中一個社群貢獻的[快速入門範本](https://azure.microsoft.com/documentation/templates/) (也可以在 [GitHub](https://github.com/Azure/azure-quickstart-templates) 上取得範本)。

建立新範本已超出本文的範圍，所以我們就從使用可在[快速入門範本](https://azure.microsoft.com/documentation/templates/101-vm-simple-linux/)取得的「101-simple-vm-from-image」範本著手。根據預設，這會在具有單一子網路的新虛擬網路中，建立單一 Ubuntu 14.04.2-LTS 虛擬機器。您只需要指定資源群組及下列幾個參數即可使用此範本：

* VM 的系統管理員使用者名稱 = `adminUsername`
* 密碼 = `adminPassword`
* VM 的網域名稱 = `dnsLabelPrefix`

>[AZURE.TIP] 下列步驟將示範搭配 Azure CLI 使用 VM 範本的一種方法。如需其他範例，請參閱[使用 Azure 資源管理員範本和 Azure CLI 部署和管理虛擬機器](./virtual-machines/virtual-machines-linux-cli-deploy-templates.md)。

1. 遵循「深入了解 GitHub」連結從 [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-linux) 將 azuredeploy.json 和 azuredeploy.parameters.json 檔案下載到本機電腦上的工作資料夾。(請務必選取 GitHub 中每個檔案的「原始」格式。)

2. 在文字編輯器中開啟 azuredeploy.parameters.json 檔案，然後輸入您的環境適用的參數值 (**ubuntuOSVersion** 值保持不變)。

	```
			{
			  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
			  "contentVersion": "1.0.0.0",
			  "parameters": {
			    "adminUsername": {
			      "value": "azureUser"
			    },
			    "adminPassword": {
			      "value": "GEN-PASSWORD"
			    },
			    "dnsLabelPrefix": {
			      "value": "GEN-UNIQUE"
			    },
			    "ubuntuOSVersion": {
			      "value": "14.04.2-LTS"
			    }
			  }
			}

	```
3.  由於部署參數已經過修改，您將部署 Ubuntu VM 到您稍早建立的「testRG」資源群組。選擇部署的名稱 (在此範例中為「testRGDeploy」)，然後使用下列命令來啟動它。

	```
	azure group deployment create -f azuredeploy.json -e azuredeploy.parameters.json testRG testRGDeploy
	```

	`-e` 選項會指定您在上一個步驟中修改的 azuredeploy.parameters.json 檔案。`-f` 選項會指定 azuredeploy.json 範本檔案。

	上傳部署之後及將部署套用至群組中的資源之前，此命令會傳回 [確定]。

4. 若要檢查部署的狀態，請使用下列命令。

	```
	azure group deployment show testRG testRGDeploy
	```

	**ProvisioningState** 會顯示部署的狀態。

	如果部署成功，您會看到類似下列的輸出。

		azure-cli@0.8.0:/# azure group deployment show testRG testRGDeploy
		info:    Executing command group deployment show
		+ Getting deployments
		+ Getting deployments
		data:    DeploymentName     : testDeploy
		data:    ResourceGroupName  : testRG
		data:    ProvisioningState  : Succeeded
		data:    Timestamp          : 
		data:    Mode               : Incremental
		data:    Name                   Type          Value
		data:    ---------------------  ------------  ---------------------
		data:    adminUsername          String        MyUserName
		data:    adminPassword          SecureString  undefined
		data:    dnsLabelPrefix    String        MyDomainName
		data:    ubuntuOSVersion        String        14.04.2-LTS
		info:    group deployment show command OK

	>[AZURE.NOTE] 如果您認為您的設定不正確，並需要停止長時間執行的部署，請使用下列命令。
	>
	> `azure group deployment stop testRG testRGDeploy`
	>
	> 若未提供部署名稱，則會根據範本檔案的名稱自動建立一個名稱。`azure group create` 命令的輸出會傳回該名稱。

	現在您可以使用您指定的網域名稱，透過 SSH 連接到 VM。連接到 VM 時，您需要使用格式為 `<domainName>.<region>.cloudapp.azure.com` 的完整網域名稱，例如 `MyDomainName.westus.cloudapp.azure.com`。

5. 若要檢視群組，請使用下列命令。

		azure group show testRG

	此命令會傳回群組中資源的相關資訊。如果您有多個群組，可使用 `azure group list` 命令來擷取群組名稱清單，然後使用 `azure group show` 來檢視特定群組的詳細資料。

您也可以直接使用 [GitHub](https://github.com/Azure/azure-quickstart-templates) 中的範本，而不需將範本下載到電腦。若要執行此動作，可在命令中使用 **--template-uri** 選項，將 URL 傳遞至範本的 azuredeploy.json 檔。若要取得 URL，請以 [原始] 模式在 GitHub 上開啟 azuredeploy.json，然後複製出現在瀏覽器網址列中的 URL。接著，您可以使用類似下列的命令，直接使用此 URL 建立部署。

	azure group deployment create testRG testRGDeploy --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-linux/azuredeploy.json
系統會提示您輸入必要的範本參數。

> [AZURE.NOTE] 請務必以 [原始] 模式開啟 JSON 範本。瀏覽器網址列中出現的 URL，與一般模式中出現的不同。在 GitHub 上檢視檔案時，若要以 [原始] 模式開啟檔案，請按一下右上角的 [原始]。

## 使用資源

雖然範本可讓您宣告設定中整個群組的變更，但有時您只需要使用特定的資源。您可以使用 `azure resource` 命令執行此工作。

> [AZURE.NOTE] 使用 `list` 命令以外的 `azure resource` 命令時，必須指定您正透過 `-o` 參數使用之資源的 API 版本。如果不確定要使用的 API 版本，請咨詢範本檔案並尋找資源的 **apiVersion** 欄位。

1. 若要列出群組中的所有資源，請使用下列命令。

		azure resource list testRG

2. 若要檢視群組內的個別資源 (例如名為「MyUbuntuVM」的 VM)，請使用如下所示的命令。

		azure resource show testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15"

	請注意 **Microsoft.Compute/virtualMachines** 參數。這表示您正在要求哪類資源的資訊。如果您查看稍早下載的範本檔案，您會發現此相同的值可用來定義範本中說明的虛擬機器資源類型。

	此命令會傳回虛擬機器的相關資訊。

3. 檢視資源的詳細資料時，建議使用 `--json` 參數，更加實用。此參數可製作更具可讀性的輸出，其中部分值是巢狀結構或集合。下列範例示範如何以 JSON 文件傳回 **show** 命令的結果。

		azure resource show testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15" --json

	>[AZURE.NOTE] 您可以使用 &gt; 字元將輸出導向檔案，藉此將 JSON 資料儲存至檔案。例如：
	>
	> `azure resource show testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15" --json > myfile.json`

4. 若要刪除現有的資源，請使用如下所示的命令。

		azure resource delete testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15"

## 檢視群組記錄檔

若要檢視群組上執行之作業的記錄資訊，請使用 `azure group log show` 命令。依預設，這會列出在群組上執行的最後一個作業。若要檢視所有作業，請使用選用的 `--all` 參數。針對前次部署，使用 `--last-deployment`。針對特定部署，使用 `--deployment` 並指定部署名稱。下列範例會傳回針對群組「testRG」執行的所有作業記錄。

	azure group log show testRG --all
    
## 匯出資源群組作為範本

針對現有的資源群組，您可以檢視此資源群組的 Resource Manager 範本。匯出此範本有兩個優點︰

1. 因為所有基礎結構都已定義於範本中，所以您可以輕鬆地自動進行解決方案的未來部署。

2. 您可以查看代表您的解決方案的 JSON，藉此熟悉範本語法。

使用 Azure CLI，您可以匯出代表資源群組目前狀態的範本，或下載特定部署所用的範本。

* **匯出資源群組的範本** - 這在您已變更資源群組，而且需要擷取其目前狀態的 JSON 表示法時很有用。不過，產生的範本只包含最少的參數數目，但不包含任何變數。範本中大部分的值為硬式編碼。在部署所產生的範本之前，您可能想要將更多的值轉換成參數，以便針對不同的環境自訂部署。

    若要將資源群組範本匯出至本機目錄中，請執行 `azure group export` 命令，如下列範例所示。(取代適用於您作業系統環境的本機目錄。)

        azure group export testRG ~/azure/templates/

* **針對特定部署匯出範本** -- 這在您需要檢視用來部署資源的實際範本時很有用。範本會包含針對原始部署定義的所有參數和變數。不過，如果您組織中有人已變更非此範本中定義的資源群組，此範本並不會代表資源群組的目前狀態。

    若要將用於特定部署的範本下載至本機目錄，請執行 `azure group deployment template download` 命令。

        azure group deployment template download TestRG testRGDeploy ~/azure/templates/downloads/
 
>[AZURE.NOTE] 範本匯出處於預覽狀態，並非所有的資源類型目前都支援匯出範本。嘗試匯出範本時，您可能會看到一個錯誤，表示未匯出某些資源。如有需要，您可以在下載範本之後，在範本中手動定義這些資源。

## 後續步驟

* 如需透過 Azure PowerShell 來使用 Azure 資源管理員的相關資訊，請參閱[搭配使用 Azure PowerShell 與 Azure 資源管理員](powershell-azure-resource-manager.md)。
* 如需從 Azure 入口網站使用 Azure Resource Manager 的相關資訊，請參閱[使用 Azure 入口網站部署及管理 Azure 資源](./azure-portal/resource-group-portal.md)。

<!---HONumber=AcomDC_0720_2016-->