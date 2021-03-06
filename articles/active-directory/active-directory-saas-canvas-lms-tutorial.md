<properties
    pageTitle="教學課程：Azure Active Directory 與 Canvas 整合 | Microsoft Azure" 
    description="了解如何使用 Canvas LMS 搭配 Azure Active Directory 來啟用單一登入、自動化佈建和更多功能！" 
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
    ms.date="07/11/2016" 
    ms.author="jeedes" />

#教學課程：Azure Active Directory 與 Canvas LMS 整合

本教學課程的目的是要示範 Azure 與 Canvas 的整合。本教學課程中說明的案例假設您已經具有下列項目：

-   有效的 Azure 訂閱
-   Canvas 租用戶

完成本教學課程之後，您指派給 Canvas 的 Azure AD 使用者就能夠單一登入您 Canvas 公司網站 (服務提供者起始登入) 的應用程式，或是使用[存取面板簡介](active-directory-saas-access-panel-introduction.md)。

本教學課程中說明的案例由下列建置組塊組成：

1.  啟用 Canvas 的應用程式整合
2.  設定單一登入
3.  設定使用者佈建
4.  指派使用者

![案例](./media/active-directory-saas-canvas-lms-tutorial/IC775984.png "案例")
##啟用 Canvas 的應用程式整合

本節的目的是要說明如何啟用 Canvas 的應用程式整合。

###若要啟用 Canvas 的應用程式整合，請執行下列步驟：

1.  在 Azure 傳統入口網站中，按一下左方瀏覽窗格的 [Active Directory]。

    ![Active Directory](./media/active-directory-saas-canvas-lms-tutorial/IC700993.png "Active Directory")

2.  從 [目錄] 清單中，選取要啟用目錄整合的目錄。

3.  若要開啟應用程式檢視，請在目錄檢視中，按一下頂端功能表中的 [應用程式]。

    ![應用程式](./media/active-directory-saas-canvas-lms-tutorial/IC700994.png "應用程式")

4.  按一下頁面底部的 [新增]。

    ![新增應用程式](./media/active-directory-saas-canvas-lms-tutorial/IC749321.png "新增應用程式")

5.  在 [欲執行動作] 對話方塊中，按一下 [從資源庫加入應用程式]。

    ![從組件庫新增應用程式](./media/active-directory-saas-canvas-lms-tutorial/IC749322.png "從組件庫新增應用程式")

6.  在**搜尋方塊**中，輸入 **Canvas**。

    ![應用程式庫](./media/active-directory-saas-canvas-lms-tutorial/IC775985.png "應用程式庫")

7.  在結果窗格中，選取 [Canvas]，然後按一下 [完成] 以加入應用程式。

    ![畫布](./media/active-directory-saas-canvas-lms-tutorial/IC775986.png "畫布")
##設定單一登入

本節的目的是要說明如何依據 SAML 通訊協定來使用同盟，讓使用者能夠用自己的 Azure AD 帳戶驗證至 Canvas。您必須從憑證擷取指紋值，才能設定 Canvas 的單一登入。如果您不熟悉這個程序，請參閱[如何擷取憑證的指紋值](http://youtu.be/YKQF266SAxI)

###若要設定單一登入，請執行下列步驟：

1.  在 Azure 傳統入口網站的 [Canvas] 應用程式整合頁面上，按一下 [設定單一登入] 來開啟 [設定單一登入] 對話方塊。

    ![設定單一登入](./media/active-directory-saas-canvas-lms-tutorial/IC771709.png "設定單一登入")

2.  在 [要如何讓使用者登入 Canvas] 頁面上，選取 [Microsoft Azure AD 單一登入]，然後按 [下一步]。

    ![設定單一登入](./media/active-directory-saas-canvas-lms-tutorial/IC775987.png "設定單一登入")

3.  在 [設定應用程式 URL] 頁面的 [Canvas 登入 URL] 文字方塊中，使用下列模式輸入您的 URL：`https://<tenant-name>.instructure.com`，然後按 [下一步]。

    ![設定應用程式 URL](./media/active-directory-saas-canvas-lms-tutorial/IC775988.png "設定應用程式 URL")

4.  於 [在 Canvas 設定單一登入] 頁面上，按 [下載憑證] 以下載您的憑證，然後在本機電腦上儲存憑證檔案。

    ![設定單一登入](./media/active-directory-saas-canvas-lms-tutorial/IC775989.png "設定單一登入")

5.  在不同的網頁瀏覽器視窗中，以系統管理員身分登入您的 Canvas 公司網站。

6.  移至 [課程] > [受管理帳戶] > [Microsoft]。

    ![畫布](./media/active-directory-saas-canvas-lms-tutorial/IC775990.png "畫布")

7.  在左側瀏覽窗格中，選取 [驗證]，然後按一下 [加入新的 SAML 設定]。

    ![驗證](./media/active-directory-saas-canvas-lms-tutorial/IC775991.png "驗證")

8.  在 [目前的整合] 頁面上，執行下列步驟：

    ![目前的整合](./media/active-directory-saas-canvas-lms-tutorial/IC775992.png "目前的整合")

    1.  在 Azure 傳統入口網站的 [在 Canvas 設定單一登入] 對話頁面上，複製 [實體識別碼] 值，然後將它貼至 [IdP 實體識別碼] 文字方塊中。
    2.  在 Azure 傳統入口網站的 [設定在 Canvas 單一登入] 對話頁面上，複製 [遠端登入 URL] 值，然後將它貼至 [登入 URL] 文字方塊中。
    3.  在 Azure 傳統入口網站的 [設定在 Canvas 單一登入] 對話頁面上，複製 [遠端登入 URL] 值，然後將它貼至 [登出 URL] 文字方塊中。
    4.  在 Azure 傳統入口網站的 [在 Canvas 設定單一登入] 對話頁面上，複製 [變更密碼 URL] 值，然後將它貼至 [變更密碼連結] 文字方塊中。
    5.  從匯出的憑證複製 [指紋] 值，然後將它貼入 [憑證指紋] 文字方塊。

        >[AZURE.TIP] 如需詳細資訊，請參閱[如何擷取憑證的指紋值](http://youtu.be/YKQF266SAxI)

    6.  從 [登入屬性] 清單中選取 [NameID]。
    7.  從 [識別碼格式] 清單中選取 [emailAddress]。
    8.  按一下 [儲存驗證設定]。

9.  在 Azure 傳統入口網站上，選取單一登入設定確認，然後按一下 [完成] 來關閉 [設定單一登入] 對話方塊。

    ![設定單一登入](./media/active-directory-saas-canvas-lms-tutorial/IC775993.png "設定單一登入")
##設定使用者佈建

若要讓 Azure AD 使用者可以登入 Canvas，必須將他們佈建到 Canvas。Canvas 需以手動方式佈建。

###若要佈建使用者帳戶，請執行下列步驟：

1.  登入您的 **Canvas** 租用戶。

2.  移至 [課程] > [受管理帳戶] > [Microsoft]。

    ![畫布](./media/active-directory-saas-canvas-lms-tutorial/IC775990.png "畫布")

3.  按一下 [使用者]。

    ![使用者](./media/active-directory-saas-canvas-lms-tutorial/IC775995.png "使用者")

4.  按一下 [新增使用者]。

    ![使用者](./media/active-directory-saas-canvas-lms-tutorial/IC775996.png "使用者")

5.  在 [新增使用者] 對話頁面上，執行下列步驟：

    ![新增使用者](./media/active-directory-saas-canvas-lms-tutorial/IC775997.png "新增使用者")

    1.  在 [全名] 文字方塊中，輸入使用者的名稱。
    2.  在 [電子郵件] 文字方塊中，輸入使用者的電子郵件地址。
    3.  在 [登入] 文字方塊中，輸入使用者的 Azure AD 電子郵件地址。
    4.  選取 [以電子郵件通知使用者有關這個帳戶的建立]。
    5.  按一下 [加入使用者]。

>[AZURE.NOTE] 您可以使用任何其他的 Canvas 使用者帳戶建立工具或 Canvas 提供的 API 來佈建 AAD 使用者帳戶。

##指派使用者

若要測試您的組態，則需指派您所允許使用您應用程式的 Azure AD 使用者，藉此授予其存取組態的權限。

###若要將使用者指派給 Canvas，請執行下列步驟：

1.  在 Azure 傳統入口網站中建立測試帳戶。

2.  在 [Canvas] 應用程式整合頁面上，按一下 [指派使用者]。

    ![指派使用者](./media/active-directory-saas-canvas-lms-tutorial/IC775998.png "指派使用者")

3.  選取測試使用者，按一下 [指派]，然後按一下 [是] 以確認指派。

    ![是](./media/active-directory-saas-canvas-lms-tutorial/IC767830.png "是")

如果要測試您的單一登入設定，請開啟存取面板。如需存取面板的詳細資訊，請參閱[存取面板簡介](active-directory-saas-access-panel-introduction.md)。

<!---HONumber=AcomDC_0713_2016-->