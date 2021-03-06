<properties
   pageTitle="BGP 與 Azure VPN 閘道概觀 | Microsoft Azure"
   description="本文提供 BGP 與 Azure VPN 閘道的概觀。"
   services="vpn-gateway"
   documentationCenter="na"
   authors="yushwang"
   manager="rossort"
   editor=""
   tags=""/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="06/16/2016"
   ms.author="yushwang"/>

# BGP 與 Azure VPN 閘道概觀

這篇文章提供 Azure VPN 閘道中的 BGP (邊界閘道協定) 支援概觀。

## 有關 BGP

BGP 是常用於網際網路的標準路由通訊協定，可交換兩個或多個網路之間的路由和可執行性資訊。在 Azure 虛擬網路的內容中使用時，BGP 會啟用 Azure VPN 閘道，以及內部部署 VPN 裝置 (稱為 BGP 對等互連或鄰近項目) 來交換「路由」，其會通知這兩個閘道對要通過閘道的首碼或所涉及之路由器的可用性和可執行性。BGP 也可以傳播從一個 BGP 對等互連到所有其他 BGP 對等所識別的 BGP 閘道，來啟用多個網路之間的傳輸路由。
 
### 為何要使用 BGP？

BGP 是選用功能，可供您與 Azure 路由 VPN 閘道搭配使用。您也應該要先確定您的內部部署 VPN 裝置支援 BGP 後，再啟用此功能。您可以無需 BGP 即可繼續使用 Azure VPN 閘道和您的內部部署 VPN 裝置。它相當於使用靜態路由 (不含 BGP) 與在您的網路和 Azure 之間使用具有 BGP 的動態路由。

BGP 具有數個優點和新功能：

#### 支援自動和彈性的前置詞更新

利用 BGP，您只需要透過 IPsec S2S VPN 通道宣告特定 BGP 對等互連的最小前置詞。它也可以小至內部部署 VPN 裝置的 BGP 對等互連 IP 位址的主機前置詞 (/ 32)。您可以控制您要公告至 Azure 以允許 Azure 虛擬網路存取的內部部署網路首碼。
	
您也可以公告可能包含一些您的 VNet 位址首碼的較大首碼，例如預大型私人 IP 位址空間 (例如 10.0.0.0/8)。可是請注意，首碼無法與您的任何一個 VNet 首碼相同。與您的 VNet 首碼相同的路由將會遭到拒絕。

>[AZURE.IMPORTANT] 目前，公告 Azure VPN 閘道的預設路由 (0.0.0.0/0) 將會遭到封鎖。一旦這項功能，將會提供進一步的更新。

#### 根據 BGP 使用自動容錯移轉，在 VNet 和內部部署站台之間支援多個通道

您可以在 Azure VNet 與內部部署 VPN 裝置之間的相同位置中建立多個連接。這項功能在主動-主動組態中的兩個網路之間提供多個通道 (路徑)。如果其中一個通道已中斷連接，對應的路由會透過 BGP 撤回，且流量會自動轉換到其餘的通道。
	
下圖顯示這項高可用性設定的簡單範例︰
	
![多個作用中路徑](./media/vpn-gateway-bgp-overview/multiple-active-tunnels.png)

#### 支援內部部署網路與多個 Azure Vnet 之間的傳輸路由

BGP 可讓多個閘道識別及傳播來自不同網路的首碼，無論這些網路是直接或間接連線。這可以使用內部部署站台之間的 Azure VPN 閘道，或跨多個 Azure 虛擬網路啟用傳送路由。
	
下圖顯示具有多個路徑的多重躍點拓撲的範例，這些路徑可透過Microsoft 網路內的 Azure VPN 閘道來傳輸兩個內部部署網路之間的流量︰

![多重躍點傳輸](./media/vpn-gateway-bgp-overview/full-mesh-transit.png)

## BGP 常見問題集


[AZURE.INCLUDE [vpn-gateway-bgp-faq-include](../../includes/vpn-gateway-bpg-faq-include.md)]




## 後續步驟

請參閱 [開始使用 Azure VPN 閘道上的 BGP](./vpn-gateway-bgp-resource-manager-ps.md)，以了解設定跨單位與 VNet 對 VNet 連線 BGP 的步驟。

<!---HONumber=AcomDC_0622_2016-->