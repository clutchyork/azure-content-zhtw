<properties
	pageTitle="Azure Mobile Engagement 的 Android SDK 整合"
	description="描述如何整合 Azure Mobile Engagement SDK 至 Android 應用程式"
	services="mobile-engagement"
	documentationCenter="mobile"
	authors="piyushjo"
	manager="erikre"
	editor="" />

<tags
	ms.service="mobile-engagement"
	ms.workload="mobile"
	ms.tgt_pltfrm="mobile-android"
	ms.devlang="Java"
	ms.topic="article"
	ms.date="05/17/2016"
	ms.author="piyushjo;ricksal" />

# Azure Mobile Engagement 的 Android SDK 整合

> [AZURE.SELECTOR]
- [Universal Windows](mobile-engagement-windows-store-sdk-overview.md)
- [Windows Phone Silverlight](mobile-engagement-windows-phone-sdk-overview.md)
- [iOS](mobile-engagement-ios-sdk-overview.md)
- [Android](mobile-engagement-android-sdk-overview.md)

此文件說明適用於 Azure Mobile Engagement Android SDK 的所有整合及組態選項。

## 必要條件

[AZURE.INCLUDE [必要條件](../../includes/mobile-engagement-android-prereqs.md)]

## 進階功能

### 報告功能

您可以新增這些功能：

1. [進階報告選項](mobile-engagement-android-advanced-reporting.md)
2. [位置報告選項](mobile-engagement-android-location-reporting.md)
3. [進階組態選項](mobile-engagement-android-advanced-configuration.md)

### 通知:
[如何將 Reach (通知) 整合在 Android 應用程式中](mobile-engagement-android-integrate-engagement-reach.md)

1. Google Cloud Messaging (GCM)：[如何整合 GCM 與 Mobile Engagement](mobile-engagement-android-gcm-integrate.md)

2. Amazon Device Messaging (ADM)：[如何整合 ADM 與 Mobile Engagement](mobile-engagement-android-adm-integrate.md)

### 標記計畫實作：
[如何在 Android 應用程式中使用進階 Mobile Engagement 標記 API](mobile-engagement-android-use-engagement-api.md)

## 版本資訊

### 4\.2.2 (2016/05/17)

- 穩定性改進。

### 4\.2.1 (2016/05/10)

- 安全性︰停用 Web 檢視本機檔案存取權限。
- 安全性︰移除 `EngagementPreferenceActivity` 類別，其可擴充過時且不安全的 `PreferenceActivity` 類別。
- 安全性︰現在會記載觸達活動以使用 `exported="false"`，此旗標也可用於先前的 SDK 版本。

### 4\.2.0 (03/11/2016)

- SDK 現在是在 MIT 中授權的。
- 允許在 SDK 初始化階段指定自訂裝置識別碼。

如需所有版本，請參閱[完整版本資訊](mobile-engagement-android-release-notes.md)。

## 升級程序

如果您已經將較舊版的 SDK 整合到應用程式中，請參閱[升級程序](mobile-engagement-android-upgrade-procedure.md)。

<!---HONumber=AcomDC_0518_2016-->