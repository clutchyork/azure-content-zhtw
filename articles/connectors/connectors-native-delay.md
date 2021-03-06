<properties
	pageTitle="在 Logic Apps 中新增延遲 | Microsoft Azure"
	description="延遲和延遲到動作的概觀，以及如何搭配 Azure 邏輯應用程式使用。"
	services=""
	documentationCenter="" 
	authors="jeffhollan"
	manager="erikre"
	editor=""
	tags="connectors"/>

<tags
   ms.service="logic-apps"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na" 
   ms.date="07/18/2016"
   ms.author="jehollan"/>

# 開始使用延遲和延遲到動作

透過延遲和延遲到動作，您可以完成如下工作流程案例

- 等到工作日再透過電子郵件傳送狀態更新
- 延遲工作流程，直到 HTTP 呼叫有時間來完成，才繼續進行並擷取結果

若要使用邏輯應用程式中的延遲動作來開始作業，請參閱[建立邏輯應用程式](../app-service-logic/app-service-logic-create-a-logic-app.md)。

---

## 使用延遲動作

動作是由邏輯應用程式中定義的工作流程所執行的作業。[深入了解動作](connectors-overview.md)。

以下是如何在邏輯應用程式中使用延遲步驟的範例順序︰

1. 在新增觸發程序之後，按一下 [新增步驟] 以新增動作
1. 搜尋「延遲」以顯示延遲動作。在此範例中，我們將會選取**延遲**

	![延遲動作](./media/connectors-native-delay/using-action-1.png)

1. 完成任何動作屬性以設定延遲

	![延遲設定](./media/connectors-native-delay/using-action-2.png)

1. 按一下 [儲存] 來發佈及啟動邏輯應用程式

---

## 技術詳細資訊

以下是延遲和延遲到動作的詳細資料。

### 動作詳細資料

循環觸發程序具有下列可設定的屬性。

#### 延遲

讓執行延遲一段時間間隔。* 代表必要欄位。

|顯示名稱|屬性名稱|說明|
|---|---|---|
|計數 *|count|要延遲的時間單位數|
|單位 *|unit|時間單位 - `Second`、`Minute`、`Hour` 或 `Day`|
<br>

#### 延遲到

讓執行延遲到指定的日期/時間。* 代表必要欄位。

|顯示名稱|屬性名稱|說明|
|---|---|---|
|年 *|timestamp|要延遲到的年度 (GMT)|
|月 *|timestamp|要延遲到的月份 (GMT)|
|日 *|timestamp|要延遲到的日期 (GMT)|
<br>


## 後續步驟

以下是有關如何推動邏輯應用程式的詳細資料和我們的社群。

## 建立邏輯應用程式

立即試用平台和[建立邏輯應用程式](../app-service-logic/app-service-logic-create-a-logic-app.md)。您可以查看我們的 [API 清單](apis-list.md)，以探索邏輯應用程式中其他可用的連接器。

<!---HONumber=AcomDC_0727_2016-->