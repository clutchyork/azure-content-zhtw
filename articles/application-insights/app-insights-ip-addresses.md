<properties 
	pageTitle="Application Insights 使用的 IP 位址 | Microsoft Azure"
	description="靜態位址的清單" 
	services="application-insights"
    documentationCenter=".net"
	authors="alancameronwills" 
	manager="douge"/>

<tags 
	ms.service="application-insights" 
	ms.workload="tbd" 
	ms.tgt_pltfrm="ibiza" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="06/22/2016" 
	ms.author="awills"/>
 
# Application Insights 使用的 IP 位址

[Visual Studio Application Insights](app-insights-overview.md) 服務使用很多個 IP 位址，您可能想要知道這些位址 - 例如，如果您正在監視防火牆後方執行的應用程式。

> [AZURE.NOTE] 雖然這些位址是靜態的，但可能隨時需要變更。


## 連出連接埠

您需要在伺服器防火牆開啟某些連出連接埠，以允許 Application Insights SDK 和/或狀態監視器將資料傳送至入口網站：

|目的|URL|IP|連接埠
|---|---|---|---
| 遙測|dc.services.visualstudio.com<br/>dc.applicationinsights.microsoft.com| 40\.114.241.141<br/>104.45.136.42<br/>40.84.189.107<br/>168.63.242.221|443
|LiveStream|rt.services.visualstudio.com<br/>rt.applicationinsights.microsoft.com |變動|443



+ 狀態監視器組態 - 進行變更時才需要：
 -	`management.core.windows.net:443`
 -	`management.azure.com:443`
 -	`login.windows.net:443`
 -	`login.microsoftonline.com:443`
 -	`secure.aadcdn.microsoftonline-p.com:443`
 -	`auth.gfx.ms:443`
 -	`login.live.com:443`
+ 狀態監視器安裝：
 +	`packages.nuget.org:443`

這份清單可能會隨時變更。

## Availability

這是用來執行[可用性 Web 測試](app-insights-monitor-web-app-availability.md)的位址清單。

開啟連接埠 80 (http) 和 443 (https)。

```

213.199.178.54
213.199.178.55
213.199.178.56
213.199.178.61
213.199.178.57
213.199.178.58
213.199.178.59
213.199.178.60
213.199.178.63
213.199.178.64
207.46.98.158
207.46.98.159
207.46.98.160
207.46.98.157
207.46.98.152
207.46.98.153
207.46.98.156
207.46.98.162
207.46.98.171
207.46.98.172
65.55.244.40
65.55.244.17
65.55.244.42
65.55.244.37
65.55.244.15
65.55.244.16
65.55.244.44
65.55.244.18
65.55.244.46
65.55.244.47
207.46.14.60
207.46.14.61
207.46.14.62
207.46.14.55
207.46.14.63
207.46.14.64
207.46.14.51
207.46.14.52
207.46.14.56
207.46.14.65
157.55.14.60
157.55.14.61
157.55.14.62
157.55.14.47
157.55.14.64
157.55.14.65
157.55.14.43
157.55.14.44
157.55.14.49
157.55.14.50
65.54.66.56
65.54.66.57
65.54.66.58
65.54.66.61
207.46.71.54
207.46.71.52
207.46.71.55
207.46.71.38
207.46.71.51
207.46.71.57
207.46.71.58
207.46.71.37
202.89.228.67
202.89.228.68
202.89.228.69
202.89.228.57
65.54.78.49
65.54.78.50
65.54.78.51
65.54.78.54
94.245.82.32
94.245.82.33
94.245.82.37
94.245.82.38
94.245.72.44
94.245.72.45
94.245.72.46
94.245.72.49
207.46.56.57
207.46.56.58
207.46.56.59
207.46.56.67
207.46.56.61
207.46.56.62
207.46.56.63
207.46.56.64
65.55.82.84
65.55.82.85
65.55.82.86
65.55.82.81
65.55.82.87
65.55.82.88
65.55.82.89
65.55.82.90
65.55.82.91
65.55.82.92
94.245.78.40
94.245.78.41
94.245.78.42
94.245.78.45
70.37.147.43
70.37.147.44
70.37.147.45
70.37.147.48
94.245.66.43
94.245.66.44
94.245.66.45
94.245.66.48

```  

## 資料存取 API



|URI|IP|連接埠
|---|---|---
|api.applicationinsights.io<br/>api1.applicationinsights.io<br/>api2.applicationinsights.io<br/>api3.applicationinsights.io<br/>api4.applicationinsights.io<br/>api5.applicationinsights.io|13\.82.26.252<br/>40.76.213.73|80,443
|dev.applicationinsights.io<br/>dev.applicationinsights.microsoft.com<br/>dev.aisvc.visualstudio.com<br/>www.applicationinsights.io<br/>www.applicationinsights.microsoft.com<br/>www.aisvc.visualstudio.com|13\.82.24.149<br/>40.114.82.10|80,443





 

<!---HONumber=AcomDC_0629_2016-->