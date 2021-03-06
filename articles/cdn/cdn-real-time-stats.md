<properties
	pageTitle="CDN - 即時統計資料"
	description="Microsoft Azure CDN 中的即時統計資料。將內容傳遞給您的用戶端時，即時統計資料可提供關於 CDN 效能的即時資料。"
	services="cdn"
	documentationCenter=".NET"
	authors="camsoper"
	manager="erikre"
	editor=""/>

<tags
	ms.service="cdn"
	ms.workload="tbd"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="05/11/2016"
	ms.author="casoper"/>

# Microsoft Azure CDN 中的即時統計資料

[AZURE.INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## 概觀

本文件說明 Microsoft Azure CDN 中的即時統計資料。將內容傳遞給您的用戶端時，此功能可提供關於 CDN 效能的即時資料。

檢視以 HTTP 為基礎的平台即時統計資料時，可以使用下列圖表：

* [頻寬](#bandwidth)
* [狀態碼](#status-codes)
* [快取狀態](#cache-statuses)
* [連線](#connections)

> [AZURE.NOTE] 上述每一個圖表都會顯示一段指定期間的即時統計資料。過了指定的時間之後，即會顯示資料的滑動視窗。這表示舊資料將從圖表中移除，以騰出空間給新的資料。您可以依圖表選項的時間範圍來設定這個滑動視窗的時間長度。

## 存取即時統計資料

1. 在 [CDN 設定檔] 刀鋒視窗中，按一下 [管理] 按鈕。

	![CDN 設定檔刀鋒視窗管理按鈕](./media/cdn-real-time-stats/cdn-manage-btn.png)

	CDN 管理入口網站隨即開啟。

2. 將滑鼠暫留在 [分析] 索引標籤上，然後暫留在 [即時統計資料] 飛出視窗上。按一下 [HTTP 大型平台]。

	報告選項隨即顯示。

## 頻寬

「頻寬」圖表會顯示在一段指定時間內，針對目前平台所使用的頻寬量。圖表的陰影部分表示頻寬使用量。目前使用的確切頻寬量會顯示於線條圖的正下方。

> [AZURE.NOTE] 用來報告頻寬使用量的單位有下列幾種：每秒位元數 (b/s)、每秒千位元數 (Kb/s)、每秒百萬位元數 (Mb/s) 或每秒 Giga 位元數 (Gb/s)。

## 狀態碼

「狀態碼」圖表是由以色彩標示的線條所組成，指出在指定的期間內發生 HTTP 回應碼的頻率。圖表左側 (y 軸) 指出針對要求傳回狀態碼的頻率，而圖表底部 (x 軸) 表示時間的進展。

狀態碼的清單會顯示於圖表正上方。此清單表示每個可包含於線條圖中的狀態碼，以及該狀態碼目前每秒的發生次數。根據預設，圖表中會針對這些狀態碼的每一個顯示一條線。不過，您可以選擇只監視對您的 CDN 組態具有特殊意義的狀態碼。這可藉由僅標示想要的狀態碼選項並清除所有其他選項來完成。當您滿意將顯示於圖表中的狀態碼之後，應該按一下 [重新整理圖表]。這將可防止圖表中包含已清除的狀態碼。

> [AZURE.NOTE] [重新整理圖表] 選項將會清除圖表。在那之後，它只會顯示選取的狀態碼。

每個狀態碼選項如下所述。

名稱 | 說明
-----|------------
每秒點擊總數 | 判斷目前平台的每秒要求總數是否將顯示於圖表中。您可以使用此選項做為基準指示器，來查看特定狀態碼組成的點擊總數百分比。
2xx 的每秒發生次數 | 判斷針對目前平台每秒發生的 2xx 狀態碼 (例如 200、201、202 等) 總數是否將顯示於圖表中。這種類型的狀態碼表示要求已順利傳遞到用戶端。
304 的每秒發生次數 | 判斷針對目前平台每秒發生的 304 狀態碼總數是否將顯示於圖表中。這個狀態碼表示要求的資產自上次擷取 HTTP 用戶端之後並未修改。
3xx 的每秒發生次數 | 判斷針對目前平台每秒發生的 3xx 狀態碼 (例如 300、301、302 等) 總數是否將顯示於圖表中。這個選項會排除 304 狀態碼的發生次數。這種類型的狀態碼表示要求會產生重新導向。
403 的每秒發生次數 | 判斷針對目前平台每秒發生的 403 狀態碼總數是否將顯示於圖表中。這個狀態碼表示要求被視為未經授權的。產生這個狀態碼的一個可能原因是在未經授權的使用者要求由權杖型驗證所保護的資產時。
404 的每秒發生次數 | 判斷針對目前平台每秒發生的 404 狀態碼總數是否將顯示於圖表中。這個狀態碼表示找不到要求的資產。
4xx 的每秒發生次數 | 判斷針對目前平台每秒發生的 4xx 狀態碼 (例如 400、401、402、405 等) 總數是否將顯示於圖表中。這個選項會排除 403 和 404 狀態碼的發生次數。這個狀態碼表示無法將要求的資產傳遞到用戶端。
5xx 的每秒發生次數 | 判斷針對目前平台每秒發生的 5xx 狀態碼 (例如 500、501、502 等) 總數是否將顯示於圖表中。
其他狀態的每秒發生次數 | 判斷所有其他狀態碼的總發生次數是否將報告於圖表中。

您也可以選擇暫時隱藏特定狀態碼的記錄資料。您可以從圖表正下方的區域中清除所需的狀態碼選項來執行此動作。選取的狀態碼將會立即從圖表中隱藏。標示該狀態碼選項將導致該選項再次顯示。

> [AZURE.NOTE] 圖表正下方以色彩標示的選項只會影響圖表中顯示的內容。它不會對圖表是否將持續追蹤該狀態碼產生影響。

## 快取狀態

「快取狀態」圖表是由以色彩標示的線條所組成，指出在指定的期間內發生特定類型之快取狀態的頻率。圖表左側 (y 軸) 指出針對要求傳回快取狀態的頻率，而圖表底部 (x 軸) 表示時間的進展。

快取狀態的清單會顯示於圖表正上方。此清單表示每個可包含於線條圖中的快取狀態，以及該快取狀態目前每秒的發生次數。根據預設，在圖表中會針對這些快取狀態的每一個顯示一條線。不過，您可以選擇只監視對您的 CDN 組態具有特殊意義的快取狀態。這可藉由僅標示想要的快取狀態選項並清除所有其他選項來完成。當您滿意將顯示於圖表中的快取狀態之後，應該按一下 [重新整理圖表]。這將可防止圖表中包含已清除的狀態碼。

> [AZURE.NOTE] [重新整理圖表] 選項將會清除圖表。在那之後，它只會顯示選取的快取狀態。

您也可以選擇暫時隱藏特定回應碼的記錄資料。您可以從圖表正下方的區域中清除所需的回應碼選項來執行此動作。選取的回應碼將會立即從圖表中隱藏。標示該回應碼選項將導致該選項再次顯示。

> [AZURE.NOTE] 圖表正下方以色彩標示的選項只會影響圖表中顯示的內容。它不會對圖表是否將持續追蹤該狀態碼產生影響。

## 連線

這個圖形表示每分鐘的平均連線數目，可讓您檢視已經建立多少連線至您的伺服器。連線是由每一個針對透過我們的 CDN 所傳遞之資產的要求所組成。

## 另請參閱
* [Azure CDN 概觀](cdn-overview.md)
* [使用規則引擎覆寫預設的 HTTP 行為](cdn-rules-engine.md)
* [進階的 HTTP 報表](cdn-advanced-http-reports.md)
* [分析邊緣效能](cdn-edge-performance.md)

<!---HONumber=AcomDC_0518_2016-->