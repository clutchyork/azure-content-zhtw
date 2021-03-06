### <a name="noconnection"></a>如何新增或移除沒有 VPN 閘道連線的首碼

- **若要新增** 其他位址首碼到您建立的區域網路閘道，但還沒有 VPN 閘道連線，請使用下列範例。

		$local = Get-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg
		Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24')


- 若要從沒有 VPN 連線的區域網路閘道 **移除** 位址首碼，請使用以下範例。省略您不再需要的首碼。此範例中不再需要首碼 20.0.0.0/24 (來自先前的範例)，因此我們將更新區域網路閘道，並排除該首碼。

		$local = Get-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg
		Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local -AddressPrefix @('10.0.0.0/24','30.0.0.0/24')

### <a name="withconnection"></a>如何新增或移除有 VPN 閘道連線的首碼

如果您已建立 VPN 連線並想要新增或移除包含在區域網路閘道中的 IP 位址首碼，您必須依序執行下列步驟。這會導致您 VPN 連線的停機時間。更新首碼時會先移除連線、修改首碼，然後建立新的連線。

>[AZURE.IMPORTANT] 請勿刪除 VPN 閘道。如果您這樣做，必須再次進行這些步驟來重新建立它，並以新的設定重新設定您的內部部署路由器。
 
1. 指定變數。

		$gateway1 = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
		$local = Get-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg

2. 移除連線。

		Remove-AzureRmVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg

3. 修改首碼。

		$local = Get-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg `
		Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
		-AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24')

4. 建立連線。在此範例中，我們要設定 IPsec 連線類型。對於其他連線類型，請參閱 [PowerShell Cmdlet](https://msdn.microsoft.com/library/mt603611.aspx) 頁面。
	
		New-AzureRmVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg `
		-Location 'West US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local -ConnectionType IPsec `
		-RoutingWeight 10 -SharedKey 'abc123'

<!---HONumber=AcomDC_0803_2016-->