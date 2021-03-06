<properties
	 pageTitle="使用 Azure 入口網站管理 IoT 中樞 | Microsoft Azure"
	 description="如何透過 Azure 入口網站建立和管理 Azure IoT 中樞的概觀"
	 services="iot-hub"
	 documentationCenter=""
	 authors="nasing"
	 manager="timlt"
	 editor=""/>

<tags
	 ms.service="iot-hub"
	 ms.devlang="na"
	 ms.topic="article"
	 ms.tgt_pltfrm="na"
	 ms.workload="na"
	 ms.date="06/28/2016"
	 ms.author="nasing"/>

# 透過 Azure 入口網站管理 IoT 中樞

## 簡介

本文描述如何透過 Azure 入口網站開始使用 Azure IoT 中樞、如何尋找 IoT 中樞以及如何建立和管理 IoT 中樞。

## 哪裡可以找到 IoT 中樞

有幾個您可以在其中找到 IoT 中樞的地方。

1. **+ 新增**：Azure IoT 中樞是 IoT 服務，且可以在 [+ 新增] 底下的 [物聯網] 類別中找到，類似於其他服務。

2. IoT 中樞也可以透過 Marketplace 存取為 [物聯網] 底下的主服務。

## 建立 IoT 中樞

您可以使用下列方法建立 IoT 中樞。

1. 透過 [+ 新增] 選項建立 IoT 中樞會導致刀鋒視窗在下一個螢幕擷取畫面中顯示。透過這個方法以及透過 marketplace 建立 IoT 中樞的步驟完全相同。

2. 透過 Marketplace 建立 IoT 中樞：按一下 [建立] 可開啟刀鋒視窗，和先前 [+ 新增] 體驗的刀鋒視窗相同。要建立 IoT 中樞包含以下各節所列的幾個步驟。

### 選擇 IoT 中樞的名稱

若要建立 IoT 中樞，您必須命名中樞。請注意，此名稱必須是全中樞唯一的。後端不允許中樞重複，所以建議此中樞的名稱盡可能是唯一的。

### 選擇定價層。

您可以從 4 個層中做選擇：**免費**、**標準 1**、**標準 2** 和 ** S3**。免費層只允許 500 個裝置連接至 IoT 中樞，每天最多 8,000 封訊息。

**標準 S1**：IoT 中樞 S1 版本的設計對象為具有大量裝置，但每個裝置產生的資料量卻非常少的 IoT 解決方案。S1 版本的每個單位可讓您跨所有連接的裝置，每天傳輸高達 400,000 封訊息。

**標準 S2**：IoT 中樞 S2 版本的設計對象是裝置會在其中產生大量資料的 IoT 解決方案。S2 版本的每個單位可讓您跨所有連接的裝置，每天傳輸高達 6 百萬封訊息。

**標準 S3**：IoT 中樞 S3 版本是針對產生大量資料的 IoT 解決方案所設計。S3 版本的每個單位可讓您跨所有連接的裝置，每天傳輸高達 3 億封訊息。

![][4]

> [AZURE.NOTE] IoT 中樞只允許每個訂用帳戶有一個免費中樞。

### IoT 中樞單位

一個 IoT 中樞單位包含每日特定數目的訊息，所以選擇 IoT 單位數目表示此中樞支援的訊息總數等於單位數乘以該層每日的訊息數目。例如，如果您想要 IoT 中樞支援 700,000 封訊息的輸入，您可以選擇 S1 層的 2 個單位

### 裝置到雲端分割及資源群組

您可以變更 IoT 中樞的分割數目。預設的分割設定為 4 個。不過，您可以從下拉式清單選擇不同的分割數目。

對資源群組而言，您不需要明確建立空的資源群組。您可以在建立新群組時，選擇建立新的資源群組，或使用現有的資源群組。

![][5]

### 選擇訂用帳戶

Azure IoT 中樞會自動顯示使用者帳戶所連結的訂用帳戶清單。您可以在此選擇其中一個選項，將 IoT 中樞與該訂用帳戶產生關聯。

### 選擇位置

位置選項提供可在其中使用 IoT 中樞的區域清單。IoT 中樞可在下列地區部署︰澳大利亞東部、澳大利亞東南部、東亞、東南亞、北歐、西歐、日本東部、日本西部、美國東部和美國西部。

### 建立 IoT 中樞

完成上述的所有步驟之後，現在已經準備好要建立 IoT 中樞。按一下 [建立] 會啟動利用特定選項建立此 IoT 中樞的後端程序，並將其部署到指定的位置。

請注意，建立 IoT 中樞可能需要幾分鐘的時間，因為要在適當的位置伺服器中進行後端部署需要時間。

## 變更 IoT 中樞的設定

您可以在建立 IoT 中樞後變更現有 IoT 中樞的設定。按一下 IoT 中樞名稱來開啟設定頁面。

![][8]

**共用存取原則**：這些原則可定義裝置與服務連接至 IoT 中樞的權限。您可以按一下 [設定] 之下的 [共用存取原則] 來存取這些原則。在這個刀鋒視窗中，您可以修改現有的原則或新增原則。

### 建立新的原則

- 按一下 [新增] 來開啟刀鋒視窗，您可以在其中輸入新的原則名稱以及您想要與此原則產生關聯的權限，如在下一個圖中所示。

	有許多權限可與這些共用原則產生關聯。前兩個原則，**登錄讀取**和**登錄寫入**，適用於授與讀取和寫入存取權給裝置身分識別存放區或身分識別登錄。請注意，選擇寫入選項就會自動選擇讀取選項。

 	**服務連接**原則會授與存取雲端端點的權限，例如服務取用者群組可連接到 IoT 中樞，而**裝置連接**原則會授與在 IoT 中樞的裝置端端點上傳送和接收訊息的權限。

- 按一下 [建立] 將此新建立的原則新增至現有的清單。

![][10]

## 訊息

按一下 [傳訊] 原則以顯示正在修改之 IoT 中樞的傳訊屬性清單。有兩種主要的屬性類型可供修改或複製：**雲端到裝置**和**裝置到雲端**。

- **雲端到裝置設定**：具有 2 個子設定：訊息的**雲端到裝置 TTL** (存留時間) 和**保留時間**。第一次建立 IoT 中樞時，這些設定會在預設值 1 小時內建立。不過，您可以使用滑桿或直接輸入值來自訂這些設定。

- **裝置到雲端設定**：有數個子設定，其中有一些會在建立 IoT 中樞時命名/指派，並且只能複製到其他可自訂的子設定。這些子設定都會在下一節列出。

**分割**：這個值會在 IoT 中樞建立時設定，並且可以透過這項設定變更。

**事件中樞相容的名稱和端點**：建立 IoT 中樞時，事件中樞會在內部建立，而使用者在某些情況下可能需要其存取權。此事件中樞名稱和端點無法自訂，但可透過 [複製] 按鈕使用。

**保留時間**：預設為 1 天，但可以使用下拉式清單自訂為其他值。請注意，這個值對於「裝置到雲端」而言是以天為單位，而不是像「雲端到裝置」那樣以小時為單位進行設定。

**取用者群組**：取用者群組是類似於其他傳訊系統的設定，可以利用特定方式提取資料以將其他應用程式或服務連接到 IoT 中樞。每個 IoT 中樞都是使用預設取用者群組建立的。不過，您可以在 IoT 中樞新增或刪除取用者群組。

> [AZURE.NOTE] 預設的取用者群組無法編輯或刪除。

![][11]

## 檔案上傳

若要使用「IoT 中樞」中的檔案上傳功能，您必須先將「Azure 儲存體」帳戶與您的中樞建立關聯。選取 [檔案上傳] 設定，即可顯示正在修改之 IoT 中樞的檔案上傳屬性清單。

**儲存體容器**︰使用入口網站在您目前的訂用帳戶中選取儲存體帳戶中的 Blob 容器，來與您的「IoT 中樞」建立關聯。如果有需要，您可以在 [儲存體帳戶] 刀鋒視窗上建立新的儲存體帳戶，並在 [容器] 刀鋒視窗上建立新的 Blob 容器。IoT 中樞會自動產生具有此 Blob 容器寫入權限的 SAS URI，以供裝置上傳檔案時使用。

![][14]

**接收已上傳檔案的通知**︰透過切換來啟用或停用檔案上傳通知。

**SAS TTL**︰這個設定是「IoT 中樞」傳回給裝置之 SAS URI 的存留時間。預設為 1 小時，但可以使用滑桿來自訂成其他值。

**檔案通知設定預設 TTL**：檔案上傳通知到期前的存留時間。預設為 1 天，但可以使用滑桿來自訂成其他值。

**檔案通知最大傳遞計數**︰「IoT 中樞」將嘗試傳遞檔案上傳通知的次數。預設為 10，但可以使用滑桿來自訂成其他值。

![][13]

## 價格和調整

現有 IoT 中樞的價格可透過 [價格] 設定變更，但是有下列例外狀況：

- 在目前的實作中，具有免費 SKU 的 IoT 中樞無法變更為付費 SKU 層，反之亦然。
- 在訂用帳戶中只能有一個免費層 IoT 中樞。

![][12]

只有在當天傳送的訊息數目不會衝突時，才允許從較高的層 (S2 或 S3) 移至較低層 (S1 或 S2)。例如，如果每一天的訊息量超過 400,000 封，則可變更 IoT 中樞的層級；但如果您變更為 S1 層，那中樞將會在那一天進行節流。

## 刪除 IoT 中樞

您可以藉由按一下 [瀏覽]，然後選擇要刪除的適當中樞，即可瀏覽至您想要刪除的 IoT 中樞。按一下中樞名稱下方的 [刪除] 按鈕會刪除該中樞。

## 後續步驟

遵循下列連結以深入了解如何管理 Azure IoT 中樞：

- [大量管理 IoT 裝置][lnk-bulk]
- [用量度量][lnk-metrics]
- [作業監視][lnk-monitor]
- [管理 IoT 中樞的存取權][lnk-itpro]

若要進一步探索 IoT 中樞的功能，請參閱︰

- [設計您的解決方案][lnk-design]
- [開發人員指南][lnk-devguide]
- [使用範例 UI 探索裝置管理][lnk-dmui]
- [使用閘道 SDK 模擬裝置][lnk-gateway]
- [徹底保護您的 IoT 解決方案][lnk-securing]


  [4]: ./media/iot-hub-manage-through-portal/create-iothub.png
  [5]: ./media/iot-hub-manage-through-portal/location1.png
  [8]: ./media/iot-hub-manage-through-portal/portal-settings.png
  [10]: ./media/iot-hub-manage-through-portal/shared-access-policies.png
  [11]: ./media/iot-hub-manage-through-portal/messaging-settings.png
  [12]: ./media/iot-hub-manage-through-portal/pricing-error.png
  [13]: ./media/iot-hub-manage-through-portal/file-upload-settings.png
  [14]: ./media/iot-hub-manage-through-portal/file-upload-container-selection.png

[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[What is Azure IoT Hub?]: iot-hub-what-is-iot-hub.md

[lnk-bulk]: iot-hub-bulk-identity-mgmt.md
[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md
[lnk-itpro]: iot-hub-itpro-info.md

[lnk-design]: iot-hub-guidance.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-dmui]: iot-hub-device-management-ui-sample.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
[lnk-securing]: iot-hub-security-ground-up.md

<!---HONumber=AcomDC_0727_2016-->