下表列出服務匯流排訊息的特定配額資訊。事件中樞限制包含在此資料表中，但如需有關事件中樞的其他特定資訊，請參閱[事件中樞價格](https://azure.microsoft.com/pricing/details/event-hubs/)。如需有關服務匯流排的價格及其他配額的詳細資訊，請參閱[服務匯流排價格](https://azure.microsoft.com/pricing/details/service-bus/)概觀。

|配額名稱|Scope|類型|超出時的行為|值|
|---|---|---|---|---|
| 每個 Azure 訂用帳戶的命名空間數目上限|命名空間|靜態|入口網站會拒絕後續對於更多命名空間的要求。|100|
|佇列/主題大小|實體|在建立佇列/主題時定義。|內送訊息將會遭到拒絕，而且呼叫端程式碼將會收到例外狀況。|1、2、3、4 或 5 GB。<br /><br />如果[分割](service-bus-partitioning.md)已啟用，佇列/主題大小上限是 80 GB。|
|命名空間上的並行連線數目|命名空間|靜態|後續對更多連接的要求將會遭到拒絕，而且呼叫端程式碼將會收到例外狀況。REST 作業不會計入並行 TCP 連線內。|NetMessaging: 1,000<br /><br />AMQP: 5,000|
|佇列/主題/訂用帳戶實體上的並行連線數目|實體|靜態|後續對更多連接的要求將會遭到拒絕，而且呼叫端程式碼將會收到例外狀況。REST 作業不會計入並行 TCP 連線內。|受限於每個命名空間的並行連線數限制。|
|佇列/主題/訂用帳戶實體上的並行接收要求數目|實體|靜態|後續接收要求將會遭到拒絕，而且呼叫端程式碼將會收到例外狀況。這個配額套用至一個主題的所有訂用帳戶的並行接收作業數目合計。|5,000|
|轉送上的並行接聽程式數目|實體|靜態|後續對更多連接的要求將會遭到拒絕，而且呼叫端程式碼將會收到例外狀況。|25|
|並行轉送接聽程式數目|全系統|靜態|後續對更多連接的要求將會遭到拒絕，而且呼叫端程式碼將會收到例外狀況。|2,000|
|服務命名空間中的所有轉送端點的並行轉送連線數目|全系統|靜態|-|5,000| 
|每個服務命名空間的轉送端點數目|全系統|靜態|-|10,000| 
|每個服務命名空間的主題/佇列數目|全系統|靜態|後續在服務命名空間上建立新主題或佇列的要求將遭到拒絕。因此，如果透過 [Azure 傳統入口網站][]設定，將會產生錯誤訊息。如果從管理 API 呼叫，呼叫端程式碼將收到例外狀況。|10,000<br /><br />服務命名空間中的主題和佇列總數必須小於或等於 10,000。| 
|每個服務命名空間的分割主題/佇列數目|全系統|靜態|後續在服務命名空間上建立新的分割主題或佇列的要求將遭到拒絕。因此，如果透過 [Azure 傳統入口網站][]設定，將會產生錯誤訊息。如果從管理 API 呼叫，呼叫端程式碼將收到 **QuotaExceededException** 例外狀況。|100<br /><br />每個分割佇列或主題計入每個命名空間 10,000 個實體的配額內。| 
|任何訊息實體路徑的大小上限：佇列或主題|實體|靜態|-|260 個字元| 
|任何訊息實體名稱的大小上限：命名空間、訂閱、訂閱規則或事件中樞|實體|靜態|-|50 個字元| 
|事件中樞事件的大小上限|全系統|靜態|-|256 KB| 
|佇列/主題/訂用帳戶實體的訊息大小|全系統|靜態|超過這些配額的內送訊息將會遭到拒絕，而且呼叫端程式碼將會收到例外狀況。|訊息大小上限：256KB。([標準層](../articles/service-bus/service-bus-premium-messaging.md)) / 1MB ([進階層](../articles/service-bus/service-bus-premium-messaging.md))。<br /><br />**注意** 考量到系統負擔，這項限制通常稍微小一些。<br /><br />標頭大小上限：64KB<br /><br />屬性包中的標頭屬性數目上限：**byte/int.MaxValue**<br /><br />屬性包中的屬性大小上限：沒有明確的限制。受限於標頭大小上限。| 
|[NetOnewayRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.netonewayrelaybinding.aspx) 和 [NetEventRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.neteventrelaybinding.aspx) 轉送的訊息大小|全系統|靜態|超過這些配額的內送訊息將會遭到拒絕，而且呼叫端程式碼將會收到例外狀況。|64KB 
|[HttpRelayTransportBindingElement](https://msdn.microsoft.com/library/microsoft.servicebus.httprelaytransportbindingelement.aspx) 和 [NetTcpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.nettcprelaybinding.aspx) 轉送的訊息大小|全系統|靜態|-|無限制| 
|佇列/主題/訂用帳戶實體的訊息屬性大小|全系統|靜態|產生 **SerializationException** 例外狀況。|每一個屬性的訊息屬性大小上限為 32K。所有屬性的累計大小不得超過 64K。這適用於 [BrokeredMessage](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.brokeredmessage.aspx) 的整個標頭，其具有使用者屬性與系統屬性 (例如 [SequenceNumber](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.brokeredmessage.sequencenumber.aspx)、[Label](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.brokeredmessage.label.aspx)、[MessageId](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx) 等等)。| 
|每個主題的訂用帳戶數目|全系統|靜態|後續對主題建立更多訂用帳戶的要求將會遭到拒絕。因此，如果透過入口網站設定，將會顯示錯誤訊息。如果從管理 API 呼叫，呼叫端程式碼將收到例外狀況。|2,000| 
|每個主題的 SQL 篩選數目|全系統|靜態|後續在主題上建立更多篩選的要求將會遭到拒絕，而且呼叫端程式碼將會收到例外狀況。|2,000| 
|每個主題的相互關聯篩選數目|全系統|靜態|後續在主題上建立更多篩選的要求將會遭到拒絕，而且呼叫端程式碼將會收到例外狀況。|100,000| 
|SQL 篩選/動作的大小|全系統|靜態|後續建立更多篩選的要求將會遭到拒絕，而且呼叫端程式碼將會收到例外狀況。|篩選條件字串的長度上限：1024 (1K)。<br /><br />規則動作字串的長度上限：1024 (1K)。<br /><br />每個規則動作的運算式數目上限：32。|

[Azure 傳統入口網站]: http://manage.windowsazure.com