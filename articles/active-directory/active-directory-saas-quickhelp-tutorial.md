<properties
	pageTitle="教學課程：Azure Active Directory 與 QuickHelp 整合 | Microsoft Azure"
	description="了解如何設定 Azure Active Directory 與 QuickHelp 之間的單一登入。"
	services="active-directory"
	documentationCenter=""
	authors="jeevansd"
	manager="femila"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="05/25/2016"
	ms.author="jeedes"/>


# 教學課程：Azure Active Directory 與 QuickHelp 整合

本教學課程旨在說明如何整合 QuickHelp 與 Azure Active Directory (Azure AD)。QuickHelp 與 Azure AD 整合提供下列優點：

- 您可以在 Azure AD 中控制可存取 QuickHelp 的人員 
- 您可以讓使用者使用他們的 Azure AD 帳戶自動登入 QuickHelp (單一登入)
- 您可以在 Azure 傳統入口網站中集中管理您的帳戶

若您想了解 SaaS app 與 Azure AD 整合的更多詳細資訊，請參閱[什麼是搭配 Azure Active Directory 的應用程式存取和單一登入](active-directory-appssoaccess-whatis.md)。

## 必要條件 

若要設定 Azure AD 與 QuickHelp 整合，您需要下列項目：

- Azure AD 訂用帳戶
- 啟用 QuickHelp 單一登入的訂用帳戶


> [AZURE.NOTE] 若要測試本教學課程中的步驟，我們不建議使用生產環境。


若要測試本教學課程中的步驟，您應該遵循這些建議：

- 除非必要，否則您不應使用生產環境，。
- 如果您沒有 Azure AD 試用環境，您可以在[這裡](https://azure.microsoft.com/pricing/free-trial/)取得一個月試用。 

 
## 案例描述
此教學課程的目標是讓您在測試環境中測試 Azure AD 單一登入。本教學課程中說明的案例由二個主要建置組塊組成：

1. 從資源庫加入 QuickHelp 
2. 設定並測試 Azure AD 單一登入


## 從資源庫加入 QuickHelp
若要設定 QuickHelp 與 Azure AD 整合，您需要從資源庫將 QuickHelp 新增到受管理的 SaaS 應用程式清單。

**若要從資源庫新增 QuickHelp，請執行下列步驟：**

1. 在 **Azure 傳統入口網站**中，按一下左方瀏覽窗格的 [Active Directory]。 

	![Active Directory][1]

2. 從 [目錄] 清單中，選取要啟用目錄整合的目錄。

3. 若要開啟應用程式檢視，請在目錄檢視中，按一下頂端功能表中的 [應用程式]。

	![應用程式][2]

4. 按一下頁面底部的 [新增]。

	![應用程式][3]

5. 在 [欲執行動作] 對話方塊中，按一下 [從資源庫加入應用程式]。

	![應用程式][4]

6. 在搜尋方塊中，輸入 **QuickHelp**。

	![應用程式][5]

7. 在結果窗格中，選取 [QuickHelp]，然後按一下 [完成] 以加入應用程式。

	![應用程式][500]


##  設定並測試 Azure AD 單一登入
本節目標是說明如何以名為 "Britta Simon" 的測試使用者為基礎，使用 QuickHelp 來設定及測試 Azure AD 單一登入。


若要使用 QuickHelp 來設定並測試 Azure AD 單一登入，您需要完成下列建置組塊：

1. **[設定 Azure AD 單一登入](#configuring-azure-ad-single-single-sign-on)** - 讓您的使用者能夠使用此功能。
2. **[建立 Azure AD 測試使用者](#creating-an-azure-ad-test-user)** - 使用 Britta Simon 測試 Azure AD 單一登入。
4. **[建立 QuickHelp 測試使用者](#creating-a-quickhelp-test-user)** - 使 QuickHelp 中對應的 Britta Simon 連結到她在 Azure AD 中的代表項目。
5. **[指派 Azure AD 測試使用者](#assigning-the-azure-ad-test-user)** - 讓 Britta Simon 能夠使用 Azure AD 單一登入。
5. **[測試單一登入](#testing-single-sign-on)** - 驗證組態是否能運作。

### 設定 Azure AD 單一登入

本節目標是在 Azure 傳統入口網站啟用 Azure AD 單一登入，並在您的 QuickHelp 應用程式中設定單一登入。


**若要使用 QuickHelp 設定 Azure AD 單一登入，請執行下列步驟：**

1. 在 Azure 傳統入口網站的 [QuickHelp] 應用程式整合頁面上，按一下 [設定單一登入] 以開啟 [設定單一登入] 對話方塊。

	![設定單一登入][6]

2. 在 [您希望使用者如何登入 QuickHelp] 頁面上，選取 [Azure AD 單一登入]，然後按 [下一步]。

	![Azure AD 單一登入][7]

3. 在 [設定 App 設定] 對話方塊頁面執行下列步驟：

	![設定 App 設定][8]
 
     a.在 [登入 URL] 文字方塊中，輸入您的使用者用來登入 QuickHelp 網站的 URL (例如：*https://quickhelp.com/bsiazure/*))。

     > [AZURE.NOTE] 如果您不知道登入 URL 的值，請連絡您的 QuickHelp 支援小組。

     b.按 [下一步]。

 
4. 在 [設定在 QuickHelp 單一登入] 頁面上，執行下列步驟：按一下 [下載中繼資料]，然後將中繼資料檔儲存在您的本機電腦中。

	![何謂 Azure AD Connect][9]



1. 以系統管理員身分登入您的 QuickHelp 公司網站。

2. 在頂端的功能表中，按一下 [系統管理員]。

	![設定單一登入][21]


1. 在 [QuickHelp 系統管理員] 功能表上，按一下 [設定]。

	![設定單一登入][22]

1. 按一下 [驗證設定]。

1. 在 [驗證設定] 頁面上，執行下列步驟

	![設定單一登入][23]

    a.對 [SSO 類型] 選取 [WSFederation]。

    b.若要上傳您下載的 Azure 中繼資料檔案，請按一下 [瀏覽]、瀏覽至該檔案，然後按一下 [上傳中繼資料]。

    c.在 [電子郵件] 文字方塊中，輸入 **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**。

    d.在 [名字] 文字方塊中，輸入 **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**。

    e.在 [姓氏] 文字方塊中，輸入 **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**。

    f.在 [動作列] 中，按一下 [儲存]。







6. 在 Azure 傳統入口網站上，選取單一登入設定確認，然後按 [下一步]。

	![何謂 Azure AD Connect][10]

7. 在 [單一登入確認] 頁面上，按一下 [完成]。

	![何謂 Azure AD Connect][11]




### 建立 Azure AD 測試使用者
本節目標是在 Azure 傳統入口網站中建立名為 Britta Simon 的測試使用者。在 [使用者] 清單中，選取 [Britta Simon]。

![建立 Azure AD 使用者][20]

**若要在 Azure AD 中建立測試使用者，請執行下列步驟：**

1. 在 **Azure 傳統入口網站**中，按一下左方瀏覽窗格的 [Active Directory]。

	![建立 Azure AD 測試使用者](./media/active-directory-saas-quickhelp-tutorial/create_aaduser_02.png)

2. 從 [目錄] 清單中，選取要啟用目錄整合的目錄。

3. 若要顯示使用者清單，請按一下頂端功能表的 [使用者]。

	![建立 Azure AD 測試使用者](./media/active-directory-saas-quickhelp-tutorial/create_aaduser_03.png)
 
4. 若要開啟 [新增使用者] 對話方塊，請按一下底部工具列上的 [新增使用者]。

	![建立 Azure AD 測試使用者](./media/active-directory-saas-quickhelp-tutorial/create_aaduser_04.png)

5. 在 [告訴我們這位使用者] 對話方塊頁面上，執行以下步驟：
 
	![建立 Azure AD 測試使用者](./media/active-directory-saas-quickhelp-tutorial/create_aaduser_05.png)

    a.針對 [使用者類型]，選取 [您組織中的新使用者]。

    b.在 [使用者名稱] 文字方塊中，輸入 **BrittaSimon**。

    c.按 [下一步]。

6.  在 [使用者設定檔] 對話方塊頁面上，執行下列步驟：

	![建立 Azure AD 測試使用者](./media/active-directory-saas-quickhelp-tutorial/create_aaduser_06.png)
 
    a.在 [名字] 文字方塊中，輸入 **Britta**。

    b.在 [姓氏] 文字方塊中，輸入 **Simon**。

    c.在 [顯示名稱] 文字方塊中，輸入 **Britta Simon**。

    d.在 [角色] 清單中，選取 [使用者]。按 [下一步]。

7. 在 [取得暫時密碼] 對話方塊頁面上，按一下 [建立]。

![建立 Azure AD 測試使用者](./media/active-directory-saas-quickhelp-tutorial/create_aaduser_07.png)
 
8. 在 [取得暫時密碼] 對話方塊頁面上，執行下列步驟：

	![建立 Azure AD 測試使用者](./media/active-directory-saas-quickhelp-tutorial/create_aaduser_08.png)
  
    a.記下 [新密碼] 的值。

    b.按一下 [完成]。

  
 
### 建立 QuickHelp 測試使用者

本節目標是在 QuickHelp 中建立名為 Britta Simon 的使用者。若要讓單一登入運作，Azure AD 必須知道 QuickHelp 與 Azure AD 中互相對應的使用者。換句話說，必須在 Azure AD 使用者和 QuickHelp 中的相關使用者之間建立連結關聯性。

QuickHelp 支援 Just-in-Time 佈建。這表示如果需要，會在 QuickHelp 中自動建立使用者帳戶，並將該帳戶連結至 Azure AD 帳戶。

在這一節沒有您需要進行的動作項目。


### 指派 Azure AD 測試使用者

本節的目標是授與 Britta Simon 對 QuickHelp 的存取權，讓她能夠使用 Azure 單一登入。

![指派使用者][200]

**若要將 Britta Simon 指派到 QuickHelp，請執行以下步驟：**

1. 在 Azure 傳統入口網站中，若要開啟應用程式檢視，請在目錄檢視中，按一下頂端功能表中的 [應用程式]。

	![指派使用者][201]

2. 在應用程式清單中，選取 [QuickHelp]。

	![指派使用者][202]

1. 在頂端的功能表中，按一下 [使用者]。

	![指派使用者][203]

1. 在 [使用者] 清單中，選取 [Britta Simon]。

2. 在底部的工具列中，按一下 [指派]。

	![指派使用者][205]



### 測試單一登入

本節的目標是要使用存取面板來測試您的 Azure AD 單一登入組態。當您在存取面板中按一下 [QuickHelp] 磚時，應該會自動登入您的 QuickHelp 應用程式。


## 其他資源

* [如何與 Azure Active Directory 整合 SaaS 應用程式的教學課程清單](active-directory-saas-tutorial-list.md)
* [什麼是搭配 Azure Active Directory 的應用程式存取和單一登入？](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_01.png
[500]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_14.png


[6]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_02.png
[8]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_03.png
[9]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_04.png
[10]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_100.png
[21]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_05.png
[22]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_06.png
[23]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_07.png
[24]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_08.png
[25]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_09.png
[26]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_10.png
[27]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_11.png
[28]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_12.png

[200]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_13.png
[203]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_205.png


[400]: ./media/active-directory-saas-QuickHelp-tutorial/tutorial_QuickHelp_400.png
[401]: ./media/active-directory-saas-QuickHelp-tutorial/tutorial_QuickHelp_401.png
[402]: ./media/active-directory-saas-QuickHelp-tutorial/tutorial_QuickHelp_402.png

<!---HONumber=AcomDC_0601_2016-->