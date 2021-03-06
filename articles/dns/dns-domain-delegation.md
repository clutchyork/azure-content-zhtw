<properties
   pageTitle="將網域委派給 Azure DNS |Microsoft Azure"
   description="了解如何變更網域委派及使用 Azure DNS 名稱伺服器提供網域主機代管。"
   services="dns"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="06/30/2016"
   ms.author="cherylmc"/>


# 將網域委派給 Azure DNS

Azure DNS 可讓您裝載 DNS 區域，並在 Azure 中管理網域的 DNS 記錄。網域必須從父系網域委派給 Azure DNS，該網域的 DNS 查詢才能送達 Azure DNS。請記住，Azure DNS 不是網域註冊機構。本文說明網域委派的運作方式，以及如何將網域委派給 Azure DNS。




## DNS 委派的運作方式

### 網域和區域

網域名稱系統是網域階層。階層從「根」網域開始，其名稱只是 ‘**.**’。下面接著最上層網域，例如 'com'、'net'、'org'、'uk' 或 'jp'。再往下是第二層網域，例如 'org.uk' 或 'co.jp'。依此類推。DNS 階層中的網域裝載於個別的 DNS 區域。這些區域遍布全球，由世界各地的 DNS 名稱伺服器所裝載。

**DNS 區域**

網域是網域名稱系統中的唯一名稱，例如 'contoso.com'。DNS 區域用來裝載特定網域的 DNS 記錄。例如，網域 'contoso.com' 可能包含許多的 DNS 記錄，例如 'mail.contoso.com' (用於郵件伺服器) 和 'www.contoso.com' (用於網站)。

**網域註冊機構**

網域註冊機構是指可以提供網際網路網域名稱的公司。他們會驗證您想要使用的網際網路網域是否可用，並允許您購買。一旦註冊網域名稱，您就成為該網域名稱的合法擁有者。如果您已經有網際網路網域，您將使用目前的網域註冊機構委派給 Azure DNS。

>[AZURE.NOTE] 若要了解誰擁有指定的網域名稱，或如需有關如何購買網域的詳細資訊，請參閱 [Azure AD 中的網際網路網域管理](https://msdn.microsoft.com/library/azure/hh969248.aspx)。

### 解析和委派

有兩種類型的 DNS 伺服器：

- _授權_ DNS 伺服器裝載 DNS 區域。它只會回答這些區域中的 DNS 記錄查詢。
- _遞迴_ DNS 伺服器不裝載 DNS 區域。它會呼叫授權 DNS 伺服器來收集所需的資料，以回答所有 DNS 查詢。

電腦或行動裝置中的 DNS 用戶端，通常會呼叫遞迴 DNS 伺服器，以執行用戶端應用程式需要的任何 DNS 查詢。

當遞迴 DNS 伺服器收到 DNS 記錄的查詢時，例如 'www.contoso.com'，就必須先找到裝載 'contoso.com' 網域的區域的名稱伺服器。在作法上，它會從根名稱伺服器開始，尋找裝載 'com' 區域的名稱伺服器。然後，查詢 'com' 名稱伺服器，尋找裝載 'contoso.com' 區域的名稱伺服器。最後，就能夠向這些名稱伺服器查詢 'www.contoso.com'。

這稱為 DNS 名稱解析。嚴格來說，DNS 解析還有其他步驟，例如追蹤 CNAME，但這對於了解 DNS 委派的運作方式並不重要。

上層區域如何「指向」子區域的名稱伺服器？ 作法是使用一種特殊的 DNS 記錄，稱為 NS 記錄 (NS 代表「名稱伺服器」)。例如，根區域包含 'com' 的 NS 記錄，並且會顯示 'com' 區域的名稱伺服器。接著，'com' 區域包含 'contoso.com' 的 NS 記錄，其中顯示 'contoso.com' 區域的名稱伺服器。在上層區域中設定子區域的 NS 記錄，稱為委派網域。


![Dns-nameserver](./media/dns-domain-delegation/image1.png)

每個委派實際上有兩份 NS 記錄：一份在上層區域中指向子區域，另一份在子區域本身。'contoso.com' 區域包含 'contoso.com' 的 NS 記錄 (除了 'com' 中的 NS 記錄之外)。這些稱為授權 NS 記錄，位於子區域的頂點。


## 將網域委派給 Azure DNS

一旦您在 Azure DNS 中建立 DNS 區域，您需要在上層區域中設定 NS 記錄，使 Azure DNS 成為您的區域的名稱解析授權來源。如果是從註冊機構購買網域，註冊機構會提供選項來設定這些 NS 記錄。

>[AZURE.NOTE] 您不必擁有網域，也能在 Azure DNS 中以該網域名稱建立 DNS 區域。不過，您必須擁有網域，才能在註冊機構中設定委派給 Azure DNS。

例如，假設您購買網域 'contoso.com'，並在 Azure DNS 中建立名稱為 'contoso.com' 的區域。身為網域的擁有者，註冊機構會提供選項，讓您設定網域的名稱伺服器位址 (亦即 NS 記錄)。註冊機構會將這些 NS 記錄儲存在父系網域中，在此例子中為 '.com'。然後，當世界各地的用戶端嘗試解析 'contoso.com' 中的 DNS 記錄時，將會導向至您在 Azure DNS 區域中的網域。

### 尋找名稱伺服器的名稱

在委派 DNS 區域給 Azure DNS 之前，您必須先知道區域的名稱伺服器名稱。每次建立區域時，Azure DNS 都會配置某個集區中的名稱伺服器。

若要查看指派給區域的名稱伺服器，最簡單的方式是透過 Azure 入口網站。在此範例引，區域 ‘contoso.net’ 已被指派名稱伺服器 ‘ns1-01.azure-dns.com’、‘ns2-01.azure-dns.net’、‘ns3-01.azure-dns.org’ 和 ‘ns4-01.azure-dns.info’：

 ![Dns-nameserver](./media/dns-domain-delegation/viewzonens500.png)

Azure DNS 會自動在包含指派的名稱伺服器的區域中，建立權威 NS 記錄。您只需要擷取這些記錄，就能透過 Azure PowerShell 或 Azure CLI 查看名稱伺服器的名稱。

使用 Azure PowerShell，就能如下所示擷取授權 NS 記錄。請注意，記錄名稱 "@" 是用來指出區域頂點的記錄。

	PS> $zone = Get-AzureRmDnsZone –Name contoso.net –ResourceGroupName MyResourceGroup
	PS> Get-AzureRmDnsRecordSet –Name “@” –RecordType NS –Zone $zone

	Name              : @
	ZoneName          : contoso.net
	ResourceGroupName : MyResourceGroup
	Ttl               : 3600
	Etag              : 5fe92e48-cc76-4912-a78c-7652d362ca18
	RecordType        : NS
	Records           : {ns1-01.azure-dns.com, ns2-01.azure-dns.net, ns3-01.azure-dns.org,
                        ns4-01.azure-dns.info}
	Tags              : {}

您也可以使用跨平台 Azure CLI 來擷取權威 NS 記錄，進而了解指派給區域的名稱伺服器︰

	C:\> azure network dns record-set show MyResourceGroup contoso.net @ NS
	info:    Executing command network dns record-set show
		+ Looking up the DNS Record Set "@" of type "NS"
	data:    Id                              : /subscriptions/.../resourceGroups/MyResourceGroup/providers/Microsoft.Network/dnszones/contoso.net/NS/@
	data:    Name                            : @
	data:    Type                            : Microsoft.Network/dnszones/NS
	data:    Location                        : global
	data:    TTL                             : 172800
	data:    NS records
	data:        Name server domain name     : ns1-01.azure-dns.com.
	data:        Name server domain name     : ns2-01.azure-dns.net.
	data:        Name server domain name     : ns3-01.azure-dns.org.
	data:        Name server domain name     : ns4-01.azure-dns.info.
	data:
	info:    network dns record-set show command OK

### 設定委派

每個註冊機構都有自己的 DNS 管理工具，可變更網域的名稱伺服器記錄。在註冊機構的 DNS 管理頁面中，請編輯 NS 記錄，並將 NS 記錄取代為 Azure DNS 建立的記錄。

委派網域給 Azure DNS 時，您必須使用 Azure DNS 提供的名稱伺服器名稱。不論您的網域名稱為何，您應該一律使用全部 4 個名稱伺服器名稱。網域委派不需要名稱伺服器名稱，即可使用相同的最上層網域做為您的網域。

您不應該使用「黏附記錄」指向 Azure DNS 名稱伺服器 IP 位址，因為這些 IP 位址日後可能變更。Azure DNS 目前不支援使用您區域中名稱伺服器名稱的委派 (有時稱為「虛名名稱伺服器」)。

### 確認名稱解析已正常運作

完成委派之後，您可以使用 'nslookup' 之類的工具來查詢您區域的 SOA 記錄 (這也是在建立區域時自動建立)，以確認名稱解析正常運作。

請注意，您不必指定 Azure DNS 名稱伺服器，因為，如果已正確設定委派，正常的 DNS 解析程序會自動尋找名稱伺服器。

	nslookup –type=SOA contoso.com

	Server: ns1-04.azure-dns.com
	Address: 208.76.47.4

	contoso.com
	primary name server = ns1-04.azure-dns.com
	responsible mail addr = msnhst.microsoft.com
	serial = 1
	refresh = 900 (15 mins)
	retry = 300 (5 mins)
	expire = 604800 (7 days)
	default TTL = 300 (5 mins)

## 在 Azure DNS 中委派子網域

如果您想要設定個別的子區域，您可以在 Azure DNS 中委派子網域。例如，假設您想要設定個別的子區域 'partners.contoso.com'，請在 Azure DNS 中設定及委派 'contoso.com'。

設定子網域的程序與一般委派類似。唯一的差異是在步驟 3 中，NS 記錄必須建立於 Azure DNS 的上層區域 'contoso.com' 中，而不是透過網域註冊機構進行設定。


1. 在 Azure DNS 中建立子區域 'partners.contoso.com'。
2. 查閱子區域中的權威 NS 記錄，來取得在 Azure DNS 中裝載子區域的名稱伺服器。
3. 在指向子區域的上層區域中設定 NS 記錄，以委派子區域。


### 委派子網域

下列 PowerShell 範例將示範其運作方式。透過 Azure 入口網站或跨平台 Azure CLI 也可執行相同的步驟。

#### 步驟 1.建立上層區域和子區域

首先，我們要建立上層區域和子區域這些區域可以位於相同資源群組或不同資源群組中。

	$parent = New-AzureRmDnsZone -Name contoso.com -ResourceGroupName RG1
	$child = New-AzureRmDnsZone -Name partners.contoso.com -ResourceGroupName RG1

#### 步驟 2.擷取 NS 記錄

接著，從子區域抓取權威 NS 記錄，如下一個範例所示。這包含指派給子區域的名稱伺服器。

	$child_ns_recordset = Get-AzureRmDnsRecordSet -Zone $child -Name "@" -RecordType NS

#### 步驟 3.委派子區域

在上層區域中建立對應的 NS 記錄集，才能完成委派。請注意，上層區域中的記錄集名稱會符合子區域名稱，在此案例中為 "partners"。

	$parent_ns_recordset = New-AzureRmDnsRecordSet -Zone $parent -Name "partners" -RecordType NS -Ttl 3600
	$parent_ns_recordset.Records = $child_ns_recordset.Records
	Set-AzureRmDnsRecordSet -RecordSet $parent_ns_recordset

### 確認名稱解析已正常運作

您可以透過查閱子區域的 SOA 記錄來確認一切都已正確設定。

	nslookup –type=SOA partners.contoso.com

	Server: ns1-08.azure-dns.com
	Address: 208.76.47.8

	partners.contoso.com
		primary name server = ns1-08.azure-dns.com
		responsible mail addr = msnhst.microsoft.com
		serial = 1
		refresh = 900 (15 mins)
		retry = 300 (5 mins)
		expire = 604800 (7 days)
		default TTL = 300 (5 mins)

## 後續步驟

[管理 DNS 區域](dns-operations-dnszones.md)

[管理 DNS 記錄](dns-operations-recordsets.md)

<!---HONumber=AcomDC_0706_2016-->