<properties 
    pageTitle="教學課程：Azure Active Directory 與 Zoho Mail 整合 | Microsoft Azure" 
    description="了解如何使用 Zoho Mail 搭配 Azure Active Directory 來啟用單一登入、自動佈建和更多功能！" 
    services="active-directory" 
    authors="jeevansd"  
    documentationCenter="na" 
    manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="06/20/2016" 
    ms.author="markvi" />

#教學課程：Azure Active Directory 與 Zoho Mail 整合
  
本教學課程的目的是要示範 Azure 與 Zoho Mail 的整合。本教學課程中說明的案例假設您已經具有下列項目：

-   有效的 Azure 訂閱
-   Zoho Mail 租用戶
  
完成本教學課程之後，您指派給 Zoho Mail 的 Azure AD 使用者就能夠從您的 Zoho Mail 公司網站 (服務提供者起始登入)，或使用[存取面板](active-directory-saas-access-panel-introduction.md)來單一登入應用程式。
  
本教學課程中說明的案例由下列建置組塊組成：

1.  啟用 Zoho Mail 的應用程式整合
2.  設定單一登入
3.  設定使用者佈建
4.  指派使用者

![案例](./media/active-directory-saas-zoho-mail-tutorial/IC789600.png "案例")

##啟用 Zoho Mail 的應用程式整合
  
本節的目的是要說明如何啟用 Zoho Mail 的應用程式整合。

###若要啟用 Zoho Mail 的應用程式整合，請執行下列步驟：

1.  在 Azure 傳統入口網站中，按一下左方瀏覽窗格的 [Active Directory]。

    ![Active Directory](./media/active-directory-saas-zoho-mail-tutorial/IC700993.png "Active Directory")

2.  從 [目錄] 清單中，選取要啟用目錄整合的目錄。

3.  若要開啟應用程式檢視，請在目錄檢視中，按一下頂端功能表中的 [應用程式]。

    ![應用程式](./media/active-directory-saas-zoho-mail-tutorial/IC700994.png "應用程式")

4.  按一下頁面底部的 [新增]。

    ![新增應用程式](./media/active-directory-saas-zoho-mail-tutorial/IC749321.png "新增應用程式")

5.  在 [欲執行動作] 對話方塊中，按一下 [從資源庫加入應用程式]。

    ![從組件庫新增應用程式](./media/active-directory-saas-zoho-mail-tutorial/IC749322.png "從組件庫新增應用程式")

6.  在**搜尋方塊**中，輸入 **Zoho Mail**。

    ![應用程式庫](./media/active-directory-saas-zoho-mail-tutorial/IC789601.png "應用程式資源庫")

7.  在結果窗格中，選取 Zoho Mail，然後按一下 [完成] 以新增應用程式。

    ![Zoho Mail](./media/active-directory-saas-zoho-mail-tutorial/IC789602.png "Zoho Mail")

##設定單一登入
  
本節的目的是要說明如何依據 SAML 通訊協定來使用同盟，讓使用者能夠用自己的 Azure AD 帳戶在 Zoho Mail 中進行驗證。在此程序中，您必須建立 base-64 編碼的憑證檔案。如果您不熟悉這個程序，請參閱[如何將二進位憑證轉換成文字檔](http://youtu.be/PlgrzUZ-Y1o)。

###若要設定單一登入，請執行下列步驟：

1.  在 Azure 傳統入口網站的 [Zoho Mail] 應用程式整合頁面上，按一下 [設定單一登入] 來開啟 [設定單一登入] 對話方塊。

    ![設定單一登入](./media/active-directory-saas-zoho-mail-tutorial/IC789603.png "設定單一登入")

2.  在 [您希望使用者如何登入 Zoho Mail] 頁面上，選取 [Microsoft Azure AD 單一登入]，然後按 [下一步]。

    ![設定單一登入](./media/active-directory-saas-zoho-mail-tutorial/IC789604.png "設定單一登入")

3.  在 [設定應用程式 URL] 頁面上，執行下列步驟：

    ![設定應用程式 URL](./media/active-directory-saas-zoho-mail-tutorial/IC789605.png "設定應用程式 URL")

	a.在 [Zoho Mail 登入 URL] 文字方塊中，使用下列模式輸入您的 URL︰`http://<company name>.ZohoMail.com`

	b.按 [下一步]。


4.  在 [設定在 Zoho Mail 單一登入] 頁面上，按一下 [下載憑證]，然後將中繼資料檔儲存在您的電腦中。

    ![設定單一登入](./media/active-directory-saas-zoho-mail-tutorial/IC789606.png "設定單一登入")

5.  在不同的 Web 瀏覽器視窗中，以系統管理員身分登入您的 Zoho Mail 公司網站。

6.  移至 [控制台]。

    ![控制台](./media/active-directory-saas-zoho-mail-tutorial/IC789607.png "控制台")

7.  按一下 [SAML 驗證] 索引標籤。

    ![SAML 驗證](./media/active-directory-saas-zoho-mail-tutorial/IC789608.png "SAML 驗證")

8.  在 [SAML 驗證詳細資料] 區段中，執行下列步驟：

    ![SAML 驗證詳細資料](./media/active-directory-saas-zoho-mail-tutorial/IC789609.png "SAML 驗證詳細資料")

    1.  在 Azure 傳統入口網站的 [設定在Zoho Mail 單一登入] 對話方塊頁面上，複製 [遠端登入 URL] 值，然後將它貼入 [登入 URL] 文字方塊。
    2.  在 Azure 傳統入口網站的 [設定在Zoho Mail 單一登入] 對話方塊頁面上，複製 [遠端登出 URL] 值，然後將它貼入 [登出 URL] 文字方塊。
    3.  在 Azure 傳統入口網站的 [設定在Zoho Mail 單一登入] 對話方塊頁面上，複製 [變更密碼 URL] 值，然後將它貼入 [變更密碼 URL] 文字方塊。
    4.  從您下載的憑證建立「Base-64 編碼」檔案。  

        >[AZURE.TIP] 如需詳細資訊，請參閱[如何將二進位憑證轉換成文字檔](http://youtu.be/PlgrzUZ-Y1o)

    5.  在記事本中開啟 base-64 編碼的憑證，將其內容複製到剪貼簿，然後貼入 [公開金鑰] 文字方塊。
    6.  選取 [RSA] 做為 [演算法]。
    7.  按一下 [確定]。

9.  在 Azure 傳統入口網站上，選取單一登入設定確認，然後按一下 [完成] 來關閉 [設定單一登入] 對話方塊。

    ![設定單一登入](./media/active-directory-saas-zoho-mail-tutorial/IC789610.png "設定單一登入")

##設定使用者佈建
  
若要讓 Azure AD 使用者可以登入 Zoho Mail，必須將他們佈建到 Zoho Mail。Zoho Mail 需以手動的方式佈建。

###若要佈建使用者帳戶，請執行下列步驟：

1.  以系統管理員身分登入您的 **Zoho Mail** 公司網站。

2.  移至 [控制台] > [郵件和文件]。

3.  移至 [使用者詳細資料] > [新增使用者]。

    ![新增使用者](./media/active-directory-saas-zoho-mail-tutorial/IC789611.png "新增使用者")

4.  在 [新增使用者] 對話方塊上，執行下列步驟：

    ![新增使用者](./media/active-directory-saas-zoho-mail-tutorial/IC789612.png "新增使用者")

    1.  在相關的文字方塊中，輸入您要佈建之有效 Azure Active Directory 帳戶的 [名字]、[姓氏]、[電子郵件地址]、[密碼]。
    2.  按一下 [確定]。  

        >[AZURE.NOTE] Azure Active Directory 帳戶的持有者會收到一封包含連結的電子郵件，以在啟用帳戶前進行確認。

>[AZURE.NOTE] 您可以使用任何其他的 Zoho Mail 使用者帳戶建立工具或 Zoho Mail 提供的 API，佈建 AAD 使用者帳戶。

##指派使用者
  
若要測試您的組態，您需要指派使用者，授予存取權給您想要允許其使用您的應用程式存取設定的 Azure AD 使用者。

###若要指派使用者給 Zoho Mail，請執行下列步驟：

1.  在 Azure 傳統入口網站中建立測試帳戶。

2.  在 [Zoho Mail] 應用程式整合頁面上，按一下 [指派使用者]。

    ![指派使用者](./media/active-directory-saas-zoho-mail-tutorial/IC789613.png "指派使用者")

3.  選取測試使用者，按一下 [指派]，然後按一下 [是] 以確認指派。

    ![是](./media/active-directory-saas-zoho-mail-tutorial/IC767830.png "是")
  
如果要測試您的單一登入設定，請開啟存取面板。如需 [存取面板] 的詳細資訊，請參閱[存取面板簡介](active-directory-saas-access-panel-introduction.md)。

<!---HONumber=AcomDC_0622_2016-->