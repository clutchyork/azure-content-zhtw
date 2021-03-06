<properties
	pageTitle="使用 Azure AD Connect 管理和自訂 Active Directory Federation Services | Microsoft Azure"
	description="使用 Azure AD Connect 進行 AD FS 管理，以及使用 Azure AD Connect 和 PowerShell 的使用者 AD FS 登入經驗的自訂。"
	services="active-directory"
	documentationCenter=""
	authors="anandyadavmsft"
	manager="stevenpo"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="05/04/2016"
	ms.author="anandy"/>

# 使用 Azure AD Connect 管理和自訂 Active Directory Federation Services

此文章詳述可以使用 Azure AD Connect 執行的各種 Active Directory Federation Services (AD FS) 相關工作，以及可能需要完整 AD FS 伺服器陣列組態的其他一般 AD FS 工作。

## AD FS 管理

Azure AD Connect 提供可以使用 Azure AD Connect 精靈，以最少使用者介入的形式執行的各種 AD FS 相關工作。執行精靈來完成安裝 Azure AD Connect 之後，您可以再次執行精靈，以執行其他工作。

### 修復信任

Azure AD Connect 可以檢查 AD FS 和 Azure ADtrust 目前的健全狀況，並採取適當的動作來修復信任。請遵循下列步驟來修復您的 Azure AD 和 AD FS 信任。

從可用的工作清單中選取 [修復 AAD 和 ADFS 信任]。

![](media\active-directory-aadconnect-federation-management\RepairADTrust1.PNG)

在 [連線到 Azure AD] 頁面上，提供 Azure AD 的全域系統管理員認證，然後按 [下一步]。

![](media\active-directory-aadconnect-federation-management\RepairADTrust2.PNG)

在 [遠端存取認證] 頁面上，提供網域系統管理員的認證

![](media\active-directory-aadconnect-federation-management\RepairADTrust3.PNG)

當您按 [下一步] 時，Azure AD Connect 會檢查憑證健康情況，並會顯示任何問題 (如果有)。

![](media\active-directory-aadconnect-federation-management\RepairADTrust4.PNG)

[準備設定] 頁面會顯示為了修復信任，將執行的動作清單。

![](media\active-directory-aadconnect-federation-management\RepairADTrust5.PNG)

按一下 [安裝] 以繼續進行，並修復信任。

>[AZURE.NOTE] Azure AD Connect 只可以對自我簽署的憑證修復/採取動作。Azure AD Connect 無法修復協力廠商憑證。

### 新增新的 AD FS 伺服器

> [AZURE.NOTE] Azure AD Connect 需要 PFX 憑證檔案，才能新增 AD FS 伺服器。因此，只有當您使用 Azure AD Connect 來設定 AD FS 伺服器陣列時，才可以執行這項作業。

選取 [部署其他同盟伺服器]，然後按 [下一步]。

![](media\active-directory-aadconnect-federation-management\AddNewADFSServer1.PNG)

在 [連線到 Azure AD] 頁面上，提供 Azure AD 的全域系統管理員認證，然後按 [下一步]。

![](media\active-directory-aadconnect-federation-management\AddNewADFSServer2.PNG)

在下一個頁面上提供網域系統管理員認證。

![](media\active-directory-aadconnect-federation-management\AddNewADFSServer3.PNG)

在下一個頁面上，Azure AD Connect 會要求您提供您在使用 Azure AD Connect 設定您的新 AD FS 伺服器陣列時所提供的 pfx 檔案的密碼。按一下 [輸入密碼] 以提供 PFX 檔案的密碼。

![](media\active-directory-aadconnect-federation-management\AddNewADFSServer4.PNG)

![](media\active-directory-aadconnect-federation-management\AddNewADFSServer5.PNG)

在下一個頁面上，提供要新增至 AD FS 伺服器陣列的其他伺服器名稱或 IP 位址。

![](media\active-directory-aadconnect-federation-management\AddNewADFSServer6.PNG)

按 [下一步]，並逐步進行到最終的 [設定] 頁面。Azure AD Connect 完成將伺服器新增至 AD FS 伺服器陣列之後，將提供您選項來驗證連線。

![](media\active-directory-aadconnect-federation-management\AddNewADFSServer7.PNG)

![](media\active-directory-aadconnect-federation-management\AddNewADFSServer8.PNG)

### 加入新的 AD FS Web 應用程式 Proxy 伺服器

> [AZURE.NOTE] Azure AD Connect 需要 PFX 憑證檔案，才能新增 Web 應用程式 Proxy 伺服器。因此，只有當您使用 Azure AD Connect 來設定 AD FS 伺服器陣列時，才可以執行這項作業。

從可用的工作清單中選取 [部署 Web 應用程式 Proxy]。

![](media\active-directory-aadconnect-federation-management\WapServer1.PNG)

在下一個頁面上，提供 Azure 全域系統管理員認證。

![](media\active-directory-aadconnect-federation-management\wapserver2.PNG)

接下來，您會看到 [指定 SSL 憑證] 頁面，您需要在此提供您在使用 Azure AD Connect 設定 AD FS 伺服器陣列時所提供 PFX 檔案的密碼。

![](media\active-directory-aadconnect-federation-management\WapServer3.PNG)

![](media\active-directory-aadconnect-federation-management\WapServer4.PNG)

在下一個頁面上，新增要做為 Web 應用程式 Proxy 的伺服器。因為 Web 應用程式 Proxy 伺服器可能會或可能不會新增網域，精靈會要求要新增之伺服器的系統管理認證。

![](media\active-directory-aadconnect-federation-management\WapServer5.PNG)

在 [Proxy 信任憑證] 頁面上，提供系統管理認證，以設定 Proxy 信任和存取 AD FS 伺服器陣列中的主要伺服器。

![](media\active-directory-aadconnect-federation-management\WapServer6.PNG)

在 [準備設定] 頁面上，精靈會顯示將執行的動作清單。

![](media\active-directory-aadconnect-federation-management\WapServer7.PNG)

按一下 [安裝] 以完成組態。組態完成之後，精靈會提供您選項，來驗證與伺服器的連線。按一下 [驗證] 來檢查連線能力。

![](media\active-directory-aadconnect-federation-management\WapServer8.PNG)

### 新增新的同盟網域

使用 Azure AD Connect 將新網域新增為 Azure AD 同盟很容易。Azure AD Connect 不僅會新增同盟的新網域，也會修改宣告規則，以在您有與 Azure AD 同盟的多個網域時正確反映發行者。

若要新增新的同盟網域，請選取工作 [新增其他 Azure AD 網域]。

![](media\active-directory-aadconnect-federation-management\AdditionalDomain1.PNG)

在精靈的下一個頁面上，提供 Azure AD 全域系統管理員認證。

![](media\active-directory-aadconnect-federation-management\AdditionalDomain2.PNG)

在遠端存取認證上，提供網域系統管理員認證。

![](media\active-directory-aadconnect-federation-management\additionaldomain3.PNG)

在精靈的下一個頁面上，將會提供 Azure AD 網域的清單，您要將網域與您的內部部署目錄建立同盟。從清單選擇網域。

![](media\active-directory-aadconnect-federation-management\AdditionalDomain4.PNG)

選擇網域之後，精靈會提供您關於精靈將採取的進一步動作和組態影響的適當資訊。在某些情況下，如果您選取尚未在 Azure AD 中驗證的網域，精靈將提供資訊協助您驗證網域。如需有關如何驗證您的網域的詳細資料，請參閱[將您的自訂網域名稱新增至 Azure Active Directory](active-directory-add-domain.md)。

按 [下一步]，[準備設定] 頁面會顯示 Azure AD Connect 即將執行的動作清單。按一下 [安裝] 以完成組態。

![](media\active-directory-aadconnect-federation-management\AdditionalDomain5.PNG)

## AD FS 自訂

下列各節提供要自訂您的 AD FS 登入頁面，可能必須執行的一些常見工作的詳細資料。

### 新增自訂公司標誌或圖例

若要變更登入頁面上顯示的公司標誌，請使用下列 Windows PowerShell Cmdlet 和語法。

> [AZURE.NOTE] 建議的標誌尺寸為 260x35 @ 96 dpi，檔案大小不超過 10 KB。

    Set-AdfsWebTheme -TargetName default -Logo @{path="c:\Contoso\logo.PNG"}

> [AZURE.NOTE] TargetName 是必要參數。隨著 AD FS 釋出的預設佈景主題為指定的預設值。
 

### 新增登入說明

若要在登入頁面中新增登入頁面描述，請使用下列 Windows PowerShell Cmdlet 和語法。

    Set-AdfsGlobalWebContent -SignInPageDescriptionText "<p>Sign-in to Contoso requires device registration. Click <A href='http://fs1.contoso.com/deviceregistration/'>here</A> for more information.</p>"

### 修改 AD FS 宣告規則

AD FS 提供選項來指定自訂規則，以發出宣告。它支援豐富的宣告語言，您可以用它來建立自訂宣告規則。如需詳細資訊，您可以查看[這裡](https://technet.microsoft.com/library/dd807118.aspx)的文章。

下列各節將詳細說明如何為關於 Azure AD 和 AD FS 同盟的一些案例撰寫自訂規則。

#### 屬性中的值出現固定 ID 條件

在物件將同步處理至 Azure AD 時，Azure AD Connect 可讓您指定要用作來源錨點的屬性。如果自訂屬性中的值不是空的，您可能想要根據情況發出固定 ID 宣告。就此例而言，請考慮您選取 ms-ds-consistencyguid 做為來源錨點的屬性，並且想要在屬性具有值時發出 ImmutableID 為 ms-ds-consistencyguid，否則發出 objectGuid 做為固定 ID。您可以建構自訂宣告規則的集合，如下所述︰

**規則 1 (查詢屬性)**

    c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"]
    => add(store = "Active Directory", types = ("http://contoso.com/ws/2016/02/identity/claims/objectguid", "http://contoso.com/ws/2016/02/identity/claims/msdsconcistencyguid"), query = "; objectGuid,ms-ds-consistencyguid;{0}", param = c.Value);

在此規則中，您只需從 Active Directory 查詢使用者的 ms-ds-consistencyguid 和 objectguid 值。請將存放區名稱變更為可在您的 ADFS 部署中使用的存放區名稱，以及將宣告類型變更為在您的同盟中對 objectGUID 和 ms-ds-consistencyguid 定義的適當宣告類型。我在我的測試環境中定義了自訂宣告類型。

此外，使用 ‘add’ 而非 ‘issue’，您可以避免為實體新增傳出問題，而只使用值做為中間值。一旦您建立要用作固定 ID 的值之後，您將在後續的規則中發出宣告

**規則 2：(檢查使用者是否存在 ms-ds-consistencyguid)**

    NOT EXISTS([Type == "http://contoso.com/ws/2016/02/identity/claims/msdsconcistencyguid"])
    => add(Type = "urn:anandmsft:tmp/idflag", Value = "useguid");

此規則只會定義暫時旗標 “idflag”，如果沒有為使用者填入 ms-ds-concistencyguid，則會設為 “useguid”。背後邏輯是實際上 ADFS 不允許空的宣告。所以，在規則 1 中新增宣告 http://contoso.com/ws/2016/02/identity/claims/objectguid 和 http://contoso.com/ws/2016/02/identity/claims/msdsconcistencyguid 時，只有在為使用者填入該值時，您才會得到 msdsconsistencyguid 宣告。如果未填入，ADFS 會發現結果為空值，然後會捨棄它。如您所知所有物件都有 ObjectGuid，因此執行規則 1 之後，宣告一律會在此處

**規則 3：發出 ms-ds-consistencyguid 為固定 ID (如果有的話)**

    c:[Type == "http://contoso.com/ws/2016/02/identity/claims/msdsconcistencyguid"]
    => issue(Type = "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier", Value = c.Value);

這是隱含的存在檢查。如果宣告的值存在，則發出做為固定 ID。請注意，我即將發出 nameidentifier 宣告。您必須為您的環境中的固定 ID 使用適當的宣告類型變更它。

**規則 4：發出物件 GUID 為固定 ID，如果未出現 ms-ds-consistencyGuid**

    c1:[Type == "urn:anandmsft:tmp/idflag", Value =~ "useguid"]
    && c2:[Type == "http://contoso.com/ws/2016/02/identity/claims/objectguid"]
    => issue(Type = "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier", Value = c2.Value);

在此規則中，您只是在檢查暫存的旗標 'idflag'，並根據值決定是否要發出宣告。

> [AZURE.NOTE] 這些規則的順序很重要。

#### 使用子網域 UPN 的 SSO

您可以新增多個要使用 Azure AD Connect 加入同盟的網域 ([加入新的同盟網域](active-directory-aadconnect-federation-management.md#add-a-new-federated-domain))。需要修改 UPN 宣告，讓簽發者識別碼對應至根網域，而不是子網域，因為同盟根網域也涵蓋子系。

根據預設，簽發者識別碼的宣告規則會設定為︰

	c:[Type 
	== “http://schemas.xmlsoap.org/claims/UPN“]

	=> issue(Type = “http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid“, Value = regexreplace(c.Value, “.+@(?<domain>.+)“, “http://${domain}/adfs/services/trust/“));

![預設簽發者識別碼宣告](media\active-directory-aadconnect-federation-management\issuer_id_default.png)

預設規則只是取得 UPN 尾碼，並用在簽發者識別碼宣告中。比方說，John 是 sub.contoso.com 中的使用者，而 contoso.com 與 Azure AD 同盟。John 在登入 Azure AD 時輸入 john@sub.contoso.com 做為使用者名稱，接著，AD FS 中的預設簽發者識別碼宣告規則依下列方式處理它︰

c:[Type == “http://schemas.xmlsoap.org/claims/UPN“]

=> issue(Type = “http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid“, Value = regexreplace(john@sub.contoso.com, “.+@(?<domain>.+)“, “http://${domain}/adfs/services/trust/“));

**宣告值：**http://sub.contoso.com/adfs/services/trust/

為了讓簽發者宣告值中只有根網域，請將宣告規則變更為︰

	c:[Type == “http://schemas.xmlsoap.org/claims/UPN“]

	=> issue(Type = “http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid“, Value = regexreplace(c.Value, “^((.*)([.|@]))?(?<domain>[^.]*[.].*)$”, “http://${domain}/adfs/services/trust/“));

## 後續步驟

深入了解[使用者登入選項](active-directory-aadconnect-user-signin.md)

<!---HONumber=AcomDC_0511_2016-->