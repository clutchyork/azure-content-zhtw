<properties
	pageTitle="在 Azure 入口網站中使用角色型存取控制 | Microsoft Azure"
	description="在 Azure 入口網站中使用角色型存取控制開始進行存取管理。使用角色指派將權限指派給您的資源。"
	services="active-directory"
	documentationCenter=""
	authors="kgremban"
	manager="femila"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.devlang="na"
	ms.topic="get-started-article"
	ms.tgt_pltfrm="na"
	ms.workload="identity"
	ms.date="07/21/2016"
	ms.author="kgremban"/>

# 使用角色指派來管理 Azure 訂用帳戶資源的存取權

Azure 角色型存取控制 (RBAC) 可以對 Azure 進行更細緻的存取權管理。使用 RBAC，您可以僅授與使用者執行其作業所需的存取權。本文將協助您在 Azure 入口網站中啟動並執行 RBAC。如果您需要有關 RBAC 如何協助您管理存取權的詳細資訊，請參閱[什麼是角色型存取控制](role-based-access-control-what-is.md)。

## 檢視存取權
您可以從 [Azure 入口網站](https://portal.azure.com)的主要刀鋒視窗中查看有誰可以存取資源、資源群組或訂用帳戶。例如，我們要查看有誰可以存取我們的其中一個資源群組︰

1. 選取左側導覽列中的 [資源群組] 圖示。![資源群組 - 圖示](./media/role-based-access-control-configure/resourcegroups_icon.png)
2. 從 [資源群組] 刀鋒視窗選取資源群組的名稱。
3. 選取 [資源群組] 刀鋒視窗右上方的 [使用者]。![使用者 - 圖示](./media/role-based-access-control-configure/users_icon.png)
4. [使用者] 刀鋒視窗會列出已獲得資源群組存取權的所有使用者、群組和應用程式。

	![使用者刀鋒視窗 - 繼承的存取權和指派的存取權螢幕擷取畫面](./media/role-based-access-control-configure/view-access.png)

請注意，[已指派] 存取權給有些使用者，而其他使用者則 [已繼承] 它。存取權不是特別指派給資源群組，就是繼承自父訂用帳戶的指派。

> [AZURE.NOTE] 傳統訂用帳戶管理員和共同管理員可被視為新 RBAC 模型中訂用帳戶的擁有者。


## 新增存取權
您可從角色指派範圍內的資源、資源群組或訂用帳戶授與存取權。

1. 在 [使用者] 刀鋒視窗上選取 [新增]。![新增 - 圖示](./media/role-based-access-control-configure/add_icon.png)
2. 從 [選取角色] 刀鋒視窗選取您要指派的角色。
3. 選取目錄中您要授與存取權的使用者、群組或應用程式。您可以使用顯示名稱、電子郵件地址和物件識別碼來搜尋目錄。

	![新增使用者刀鋒視窗 - 搜尋螢幕擷取畫面](./media/role-based-access-control-configure/grant-access2.png)

4. 選取 [確定] 以建立指派。[新增使用者] 快顯視窗會追蹤進度。![新增使用者進度列 - 螢幕擷取畫面](./media/role-based-access-control-configure/addinguser_popup.png)

成功新增角色指派之後，該指派會出現在 [使用者] 刀鋒視窗上。

## 移除存取權

1. 在 [使用者] 刀鋒視窗上選取角色指派。
2. 在 [指派詳細資料] 刀鋒視窗中選取 [移除]。![移除 - 圖示](./media/role-based-access-control-configure/remove_icon.png)
3. 選取 [是] 以確認移除。![使用者刀鋒視窗 - 從角色中移除螢幕擷取畫面](./media/role-based-access-control-configure/remove-access1.png)

無法移除已繼承的指派。請注意，在下圖中，[移除] 按鈕呈現灰色。請改為查看 [指派處] 詳細資料。移至該處所列的資源來移除角色指派。

![使用者刀鋒視窗 - 繼承的存取存停用 [移除] 按鈕螢幕擷取畫面](./media/role-based-access-control-configure/remove-access2.png)

## 其他用來管理存取權的工具
除了 Azure 入口網站以外，您可以使用工具中的 Azure RBAC 命令來指派角色及管理存取權。遵循下列連結，以深入了解 Azure RBAC 命令的先決條件並開始使用。

- [Azure PowerShell](role-based-access-control-manage-access-powershell.md)
- [Azure 命令列介面](role-based-access-control-manage-access-azure-cli.md)
- [REST API](role-based-access-control-manage-access-rest.md)

## 後續步驟
- [建立存取權變更歷程記錄報告](role-based-access-control-access-change-history-report.md)
- 請參閱 [RBAC 內建角色](role-based-access-built-in-roles.md)
- 為自己定義 [Azure RBAC 中的自訂角色](role-based-access-control-custom-roles.md)

<!---HONumber=AcomDC_0727_2016-->