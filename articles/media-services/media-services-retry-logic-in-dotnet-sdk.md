<properties
    pageTitle="Ponów próbę logicznych w SDK usługi multimediów dla środowiska .NET | Microsoft Azure"
    description="Temat zawiera omówienie ponów próbę logicznych w SDK usługi multimediów dla środowiska .NET."
    authors="Juliako"
    manager="erikre"
    editor=""
    services="media-services"
    documentationCenter=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016" 
    ms.author="juliako"/>


# <a name="retry-logic-in-the-media-services-sdk-for-net"></a>Ponów próbę logicznych w SDK usługi multimediów dla środowiska .NET

Podczas pracy z usługi Microsoft Azure, może wystąpić błędy przejściowych. Jeśli wystąpi błąd przejściowych, w większości przypadków po kilku prób powiedzie się. Media SDK usług dla środowiska .NET wykonuje ponów próbę logiki do obsługi błędów przejściowych skojarzone z wyjątkiem i błędy, które są spowodowane żądania sieci web wykonywanie kwerend, zapisywanie zmian i operacje miejsca do magazynowania.  Domyślnie SDK usługi multimediów dla środowiska .NET wykonuje cztery ponowne próby przed ponownego generowania wyjątku do aplikacji. Kod w aplikacji następnie muszą prawidłowo obsługiwać ten wyjątek.  
  
 Oto krótkie wytyczne Web żądanie, miejsca do magazynowania, kwerendy i SaveChanges zasad:  
  
-   Zasady przechowywania jest używany dla operacji magazyn obiektów blob (przekazywania lub pobierania plików zasobów).  
  
-   Zasady żądań sieci Web jest używana do żądania rodzajowy sieci web (na przykład na wprowadzenie token uwierzytelniania i usuwania punkt końcowy klaster użytkowników).  
  
-   Zasady kwerendy jest używane do wykonywania zapytań jednostek z pozostałych (na przykład mediaContext.Assets.Where(...)).  
  
-   Zasady SaveChanges służy do wykonując wszystkie elementy, które zmiany danych w ramach usługi (na przykład tworzenia jednostka aktualizowanie jednostka wywołanie funkcji usługi dla operacji).  
  
 Ten temat zawiera listę typów wyjątku i kody błędów, które są obsługiwane przez Media SDK usług dla środowiska .NET ponów próbę logicznych.  
  
## <a name="exception-types"></a>Typy wyjątku  

W poniższej tabeli opisano wyjątki SDK usługi multimediów dla środowiska .NET obsługuje lub nie obsługuje niektórych operacji, które mogą powodować błędy przejściowych.  
  
Wyjątku|Żądanie sieci Web|Miejsca do magazynowania|Kwerendy|SaveChanges
----|------|----|---|---
WebException<br/>Aby uzyskać więcej informacji zobacz sekcję [kody stanu WebException](media-services-retry-logic-in-dotnet-sdk.md#WebExceptionStatus) .|Tak|Tak|Tak|Tak  
DataServiceClientException<br/> Aby uzyskać więcej informacji zobacz [kody stanu błędu HTTP](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode).|Brak|Tak|Tak|Tak
DataServiceQueryException<br/> Aby uzyskać więcej informacji zobacz [kody stanu błędu HTTP](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode).|Brak|Tak|Tak|Tak  
DataServiceRequestException<br/> Aby uzyskać więcej informacji zobacz [kody stanu błędu HTTP](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode).|Brak|Tak|Tak|Tak  
DataServiceTransportException|Brak|Brak|Tak|Tak
TimeoutException|Tak|Tak|Tak|Brak
SocketException|Tak|Tak|Tak|Tak  
StorageException|Brak|Tak|Brak|Brak 
IOException|Brak|Tak|Brak|Brak
  
###  <a name="WebExceptionStatus"></a>Kody stanu WebException  

W poniższej tabeli pokazano, dla których kody błędów WebException jest zaimplementowana Logika ponawiania. Wyliczenie [nie powiodło](http://msdn.microsoft.com/library/system.net.webexceptionstatus.aspx) definiuje kody stanu.  
  
Stan|Żądanie sieci Web|Miejsca do magazynowania|Kwerendy|SaveChanges  
-----|-----------------|-------------|-----------|----------  
ConnectFailure|Tak|Tak|Tak|Tak
NameResolutionFailure|Tak|Tak|Tak|Tak  
ProxyNameResolutionFailure|Tak|Tak|Tak|Tak  
SendFailure|Tak|Tak|Tak|Tak
PipelineFailure|Tak|Tak|Tak|Brak  
ConnectionClosed|Tak|Tak|Tak|Brak  
KeepAliveFailure|Tak|Tak|Tak|Brak  
UnknownError|Tak|Tak|Tak|Brak 
ReceiveFailure|Tak|Tak|Tak|Brak  
RequestCanceled|Tak|Tak|Tak|Brak  
Limit czasu|Tak|Tak|Tak|Brak
ProtocolError <br/>Ponów próbę na ProtocolError steruje obsługi kodu stanu HTTP. Aby uzyskać więcej informacji zobacz [kody stanu błędu HTTP](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode).|Tak|Tak|Tak|Tak|  
  
###  <a name="HTTPStatusCode"></a>Kody stanu błędu HTTP  

Gdy operacje kwerendy i SaveChanges Zgłoś DataServiceClientException, DataServiceQueryException lub DataServiceQueryException, zwracany jest kod stanu błędu HTTP we właściwości StatusCode.  W poniższej tabeli pokazano, dla których kody błędów jest zaimplementowana Logika ponawiania.  
  
 
Stan|Żądanie sieci Web|Miejsca do magazynowania|Kwerendy|SaveChanges 
---|----|----|----|----
401|Brak|Tak|Brak|Brak
403|Brak|Tak<br/>Obsługa prób z czeka dłużej.|Brak|Brak  
408|Tak|Tak|Tak|Tak
429|Tak|Tak|Tak|Tak  
500|Tak|Tak|Tak|Brak  
502|Tak|Tak|Tak|Brak  
503|Tak|Tak|Tak|Tak  
504|Tak|Tak|Tak|Brak  
  
Jeśli chcesz wykonać przyjrzeć się rzeczywisty stosowania SDK usługi multimediów dla środowiska .NET ponawiania, zobacz [azure sdk multimediów usług dla](https://github.com/Azure/azure-sdk-for-media-services/tree/dev/src/net/Client/TransientFaultHandling).

## <a name="next-steps"></a>Następne kroki

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
