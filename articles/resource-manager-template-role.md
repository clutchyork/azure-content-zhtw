<properties
   pageTitle="角色指派的資源管理員範本 | Microsoft Azure"
   description="說明可透過範本部署角色指派的資源管理員結構描述。"
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/05/2016"
   ms.author="tomfitz"/>

# 角色指派範本結構描述

在指定的範圍內為使用者、群組，或服務主體指派角色。

## 結構描述格式

若要建立角色指派，請在範本的資源區段中加入下列結構描述。
    
    {
        "type": "Microsoft.Authorization/roleAssignments",
        "apiVersion": "2014-10-01-preview",
        "name": string,
        "dependsOn": [ array values ],
        "properties":
        {
            "roleDefinitionId": string,
            "principalId": string,
            "scope": string
        }
    }

## 值

下表描述您在結構描述中必須設定的值。

| 名稱 | 值 |
| ---- | ---- |
| 類型 | 列舉<br />必要<br />**Microsoft.Authorization/roleAssignments**<br /><br />要建立的資源類型。 |
| apiVersion | 列舉<br />必要<br />**2014-10-01-preview**<br /><br />要用來建立資源的應用程式開發介面 (API) 版本。 |  
| 名稱 | 字串<br />必要<br />**全域唯一識別碼**<br /><br />新角色指派的識別碼。 |
| dependsOn | 陣列<br />選用<br />以逗號分隔的資源名稱或資源唯一識別碼清單。<br /><br />此角色指派所相依的資源集合。如果指派角色的範圍為資源，且該資源部署在相同的範本中，則將該資源的名稱包含在此元素中，以確保會優先部署資源。 | 
| 屬性 | 物件<br />必要<br />[屬性物件](#properties)<br /><br />識別角色定義、主體和範圍的物件。 |  

<a id="properties" />
### 屬性物件

| 名稱 | 值 |
| ------- | ---- |
| roleDefinitionId | 字串<br />必要<br /> **/subscriptions/{subscription-id}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}**<br /><br />要在角色指派中使用的現有角色定義識別碼。 |
| principalId | 字串<br />必要<br />**全域唯一識別碼**<br /><br />現有主體的識別碼。這會對應到目錄中的識別碼，並且可指向使用者、服務主體，或安全性群組。 |
| scope | 字串<br />必要<br />**/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}** (適用於資源群組) 或<br />**/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/{provider-namespace}/{resource-type}/{resource-name}** (適用於資源)<br /><br />套用此角色指派的範圍。 |


## 如何使用角色指派資源

當您在部署期間需要將使用者、群組或服務主體新增至角色時，請將角色指派新增到您的範本中。角色指派繼承自更高層級的範圍，因此若您在訂用帳戶層級已經新增主體到角色，您不需要為資源群組或資源重新指派它。

您可以使用以下方式針對 **name** 產生新的識別碼：

    PS C:\> $name = [System.Guid]::NewGuid().toString()

您可以使用以下方式針對角色定義擷取全域唯一識別碼：

    PS C:\> $roledef = (Get-AzureRmRoleDefinition -Name Reader).id

您可以使用下列其中一個命令擷取主體的識別碼。

針對名為 **Auditors** 的群組：

    PS C:\> $principal = (Get-AzureRmADGroup -SearchString Auditors).id

針對名為 **exampleperson** 的使用者：

    PS C:\> $principal = (Get-AzureRmADUser -SearchString exampleperson).id

針對名為 **exampleapp** 的服務主體：

    PS C:\> $principal = (Get-AzureRmADServicePrincipal -SearchString exampleapp).id 
 

## 範例

下列範例會針對資源群組指派群組至角色。

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "roleDefinitionId": {
                "type": "string"
            },
            "roleAssignmentId": {
                "type": "string"
            },
            "principalId": {
                "type": "string"
            }
        },
        "resources": [
            {
              "type": "Microsoft.Authorization/roleAssignments",
              "apiVersion": "2015-07-01",
              "name": "[parameters('roleAssignmentId')]",
              "properties":
              {
                "roleDefinitionId": "[concat(subscription().id, '/providers/Microsoft.Authorization/roleDefinitions/', parameters('roleDefinitionId'))]",
                "principalId": "[parameters('principalId')]",
                "scope": "[concat(subscription().id, '/resourceGroups/', resourceGroup().name)]"
              }
            }
        ],
        "outputs": {}
    }

## 快速入門範本

下列範本會示範如何使用角色指派資源：

- [將內建角色指派給資源群組](https://azure.microsoft.com/documentation/templates/101-rbac-builtinrole-resourcegroup)
- [將內建角色指派給現有 VM](https://azure.microsoft.com/documentation/templates/101-rbac-builtinrole-virtualmachine)
- [將內建角色指派給多個現有 VM](https://azure.microsoft.com/documentation/templates/201-rbac-builtinrole-multipleVMs)

## 後續步驟

- 如需範本結構的相關資訊，請參閱[編寫 Azure 資源管理員範本](resource-group-authoring-templates.md)。
- 如需角色型存取控制的詳細資訊，請參閱 [Azure Active Directory 角色型存取控制](active-directory/role-based-access-control-configure.md)。

<!---HONumber=AcomDC_0406_2016-->