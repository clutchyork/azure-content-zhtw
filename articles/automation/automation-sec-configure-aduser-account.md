<properties
   pageTitle="設定 Azure AD 使用者帳戶 | Microsoft Azure"
   description="本文說明如何在 Azure 自動化中為 Runbook 設定 Azure AD 使用者帳戶認證以針對 ARM 和 ASM 進行驗證。"
   services="automation"
   documentationCenter=""
   authors="MGoedtel"
   manager="jwhit"
   editor="tysonn"
   keywords="azure active directory 使用者, azure 服務管理, azure ad 使用者帳戶" />
<tags
   ms.service="automation"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="05/10/2016"
   ms.author="magoedte" />

# 使用 Azure 服務管理和 Resource Manager 驗證 Runbook

本文說明要為針對 Azure 服務管理 (ASM) 或 Azure Resource Manager (ARM) 資源執行的 Azure 自動化 Runbook 設定 Azure AD 使用者帳戶，所必須執行的步驟。雖然這仍是您 ARM 型 Runbook 支援的驗證身分識別，但建議的方法是使用新的 Azure 執行身分帳戶。

## 建立新的 Azure Active Directory 使用者

1. 以您想要管理的 Azure 訂用帳戶的服務系統管理員身分登入 Azure 傳統入口網站。
2. 選取 [Active Directory]，然後選取貴組織目錄的名稱。
3. 選取 [使用者] 索引標籤，然後在命令區域中選取 [新增使用者]。
4. 在 [告訴我們這位使用者] 頁面上，於 [使用者類型] 底下選取 [您組織中的新使用者]。
5. 輸入使用者稱。  
6. 在 [Active Directory] 頁面上選取與您的 Azure 訂用帳戶相關聯的目錄名稱。
7. 在 [使用者設定檔] 頁面上，提供姓氏和名字、使用者易記名稱，並從 [角色] 清單中選擇使用者。請勿 [啟用 Multi-Factor Authentication]。
8. 請記下使用者的完整名稱和暫時密碼。
9. 選取 [設定 > 系統管理員 > 加入]。
10. 輸入您所建立之使用者的完整使用者名稱。
11. 選取您想讓使用者管理的訂用帳戶。
12. 登出 Azure，然後使用您剛才建立的帳戶登入。將提示您變更使用者的密碼。


## 在 Azure 傳統入口網站中建立自動化帳戶
在本節中，您將執行下列步驟以在 Azure 入口網站中建立將與您用來在 ASM 和 ARM 模式中管理資源的 Runbook 搭配使用的新 Azure 自動化帳戶。

>[AZURE.NOTE] 使用 Azure 傳統入口網站建立的自動化帳戶可以透過 Azure 傳統入口網站和 Azure 入口網站以及任一組 Cmdlet 來管理。帳戶一旦建立，您在帳戶中建立和管理資源的方式就沒有差別。如果您打算繼續使用 Azure 傳統入口網站，您應該使用它代替 Azure 入口網站來建立任何的自動化帳戶。


1. 以您想要管理的 Azure 訂用帳戶的服務系統管理員身分登入 Azure 傳統入口網站。
2. 選取 [自動化]。
3. 在 [自動化] 頁面上，選取 [建立自動化帳戶]。
4. 在 [建立自動化帳戶] 方塊中輸入新的自動化帳戶的名稱，然後在下拉式清單中選取 [區域]。  
5. 按一下 [確定] 接受您的設定並建立帳戶。
6. 帳戶建立之後就會列在 [自動化] 頁面上。
7. 按一下該帳戶，您便會前往 [儀表板] 頁面。  
8. 在 [自動化儀表板] 頁面上，選取 [資產]。
9. 在 [資產] 頁面上，選取位於頁面底部的 [新增設定]。
10. 在 [新增設定] 頁面上選取 [新增認證]。
11. 在 [定義認證] 頁面的 [認證類型] 下拉式清單中選取 [Windows PowerShell 認證]，並提供認證名稱。
12. 在下列 [定義認證] 頁面中，於 [使用者名稱] 欄位中輸入稍早建立之 AD 使用者帳戶的使用者名稱，並於 [密碼] 和 [確認密碼] 欄位中輸入密碼。按一下 [確定] 儲存變更。

## 在 Azure 入口網站中建立自動化帳戶

在本節中，您將執行下列步驟以在 Azure 入口網站中建立將與您用來在 ARM 模式中管理資源的 Runbook 搭配使用的新 Azure 自動化帳戶。

1. 以您想要管理的 Azure 訂用帳戶的服務系統管理員身分登入 Azure 入口網站。
2. 選取 [自動化帳戶]。
3. 在 [自動化帳戶] 刀鋒視窗中，按一下 [新增]。<br>![加入自動化帳戶](media/automation-sec-configure-azure-runas-account/add-automation-acct-properties.png)
2. 在 [加入自動化帳戶] 刀鋒視窗的 [名稱] 方塊中，輸入新的自動化帳戶的名稱。
5. 如果您有多個訂用帳戶，請為新的自動化帳戶指定其中一個訂用帳戶，並指定新的或現有的 [資源群組] 和 Azure 資料中心的 [位置]。
3. 在 [建立 Azure 執行身分帳戶] 選項中選取 [否] 這個值，然後按一下 [建立] 按鈕。  

    >[AZURE.NOTE] 如果您選取選項 [否] 以選擇不要建立執行身分帳戶，則會在 [加入自動化帳戶] 刀鋒視窗中看到一則警告訊息。雖然會建立帳戶並將其指派給訂用帳戶中的 [參與者] 角色，但帳戶在訂用帳戶的目錄服務內不會有對應的驗證身分識別，因此在訂用帳戶中沒有存取資源。這將導致參考此帳戶的任何 Runbook 無法進行驗證並針對 ARM 資源執行工作。

    ![加入自動化帳戶警告](media/automation-sec-configure-azure-runas-account/add-automation-acct-properties-error.png)

4. 在 Azure 建立自動化帳戶時，您可以在功能表的 [通知] 底下追蹤進度。

認證建立完成後，您便需要建立可讓自動化帳戶與稍早建立之 AD 使用者帳戶產生關聯的認證資產。請記住，我們只建立了自動化帳戶，但它並未與驗證身分識別產生關聯。執行 [Azure 自動化中的認證資產](../automation/automation-credentials.md#creating-a-new-credential)一文所述的步驟，並以**網域\\使用者**格式輸入 [使用者名稱] 值。

## 在 Runbook 中使用認證

您可以在 Runbook 中使用 [Get-AutomationPSCredential](http://msdn.microsoft.com/library/dn940015.aspx) 活動來擷取認證，然後將它搭配 [Add-AzureAccount](http://msdn.microsoft.com/library/azure/dn722528.aspx) 來連接到您的 Azure 訂用帳戶。如果認證是多個 Azure 訂用帳戶的管理員，則您也應該使用 [Select-AzureSubscription](http://msdn.microsoft.com/library/dn495203.aspx) 來指定正確的一個。這會在以下的 Windows PowerShell 範例中顯示，通常會出現在大部分 Azure 自動化 Runbook 的頂端。

    $cred = Get-AutomationPSCredential –Name "myuseraccount.onmicrosoft.com"
	Add-AzureAccount –Credential $cred
	Select-AzureSubscription –SubscriptionName "My Subscription"

您應該在您的 Runbook 中的任何[檢查點](http://technet.microsoft.com/library/dn469257.aspx#bk_Checkpoints)後重複這幾行。如果 Runbook 暫止然後在另一個背景工作上繼續執行，則它必須重新執行驗證。

## 後續步驟
* 在接下來的 [Azure 自動化 Runbook 類型](../automation/automation-runbook-types.md)一文中，檢閱不同的 Runbook 類型和用於建立自己的 Runbook 的步驟

<!---HONumber=AcomDC_0511_2016-->