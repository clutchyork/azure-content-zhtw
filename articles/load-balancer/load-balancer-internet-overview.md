
<properties 
   pageTitle="網際網路面向的負載平衡器概觀 |Microsoft Azure"
   description="網際網路面向的負載平衡器及其功能的概觀。負載平衡器如何作用於使用虛擬機器和雲端服務的 Azure。"
   services="load-balancer"
   documentationCenter="na"
   authors="joaoma"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="04/05/2016"
   ms.author="joaoma" />


# 多個虛擬機器或服務之間的網際網路面向負載平衡器

端點的其中一個用途是 Azure 負載平衡器的組態，該組態可在多個虛擬機器或服務之間散發特定類型的流量。例如，您可以將 Web 要求的流量負載分散在多個 Web 伺服器或 Web 角色。

Azure 負載平衡器將連入流量的公用 IP 位址和連接埠號碼對應至虛擬機器的私人 IP 位址和連接埠號碼，來自虛擬機器的回應流量也是如此。

>[AZURE.NOTE] Azure 負載平衡器會使用預設設定，在多個虛擬機器執行個體間提供雜湊散發網路流量 (如需雜湊散發的詳細資訊，請參閱[負載平衡器功能](load-balancer-overview.md#load-balancer-features))。如果您要尋找工作階段同質性，請參閱[負載平衡器分配模式](load-balancer-distribution-mode.md)。

對於包含 Web 角色或背景工作角色執行個體的雲端服務，您可以在服務定義 (.csdef) 中定義公用端點。
 
Servicedefinition.csdef 檔案將包含端點組態，而當一個 Web 或背景工作角色部署有多個角色執行個體時，將會為它設定負載平衡器。將執行個體加入至雲端部署的方法就是變更服務組態檔 (.csfg) 上的執行個體計數。

下圖顯示在三部虛擬機器中共用，且公用和私人 TCP 連接埠均為 443 的已加密 Web 流量負載平衡端點。這三部虛擬機器均位在負載平衡集合中。


![建立負載平衡器範例](./media/load-balancer-internet-overview/IC727496.png))

當網際網路用戶端傳送網頁要求至雲端服務的公用 IP 位址與 TCP 連接埠 443 時，Azure 負載平衡器會對負載平衡集合中，介於這三部虛擬機器之間的要求執行以雜湊為基礎的負載平衡。您可以在[負載平衡器概觀頁面](load-balancer-overview.md#load-balancer-features)取得負載平衡器演算法的詳細資訊。


## 後續步驟

了解網際網路面向的負載平衡器之後，您也可以閱讀[內部負載平衡器](load-balancer-internal-overview.md)一文，以確認更適合您的雲端部署使用的負載平衡器。

您也可以[開始建立網際網路面向的負載平衡器](load-balancer-get-started-internet-arm-ps.md)，以及為特定負載平衡器的網路流量行為設定何種[分配模式](load-balancer-distribution-mode.md)。

如果您的應用程式需要讓負載平衡器後方的伺服器保持連接狀態，您可以深入了解[負載平衡器的閒置 TCP 逾時設定](load-balancer-tcp-idle-timeout.md)。當您使用 Azure 負載平衡器時，該文章可幫助您了解閒置連接行為。


 

<!---HONumber=AcomDC_0427_2016-->