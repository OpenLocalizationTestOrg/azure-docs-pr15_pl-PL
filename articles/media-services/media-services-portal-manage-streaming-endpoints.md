<properties 
    pageTitle="Zarządzanie przesyłanie strumieniowe punkty końcowe Portal Azure | Microsoft Azure" 
    description="W tym temacie przedstawiono sposoby zarządzania przesyłanie strumieniowe punkty końcowe Portal Azure." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    writer="juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/24/2016"
    ms.author="juliako"/>


#<a name="manage-streaming-endpoints-with-the-azure-portal"></a>Zarządzanie przesyłanie strumieniowe punkty końcowe Portal Azure

## <a name="overview"></a>Omówienie

> [AZURE.NOTE] Aby użyć tego samouczka, potrzebne jest konto Azure. Aby uzyskać szczegółowe informacje zobacz [Azure bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/). 

W usługi multimediów Azure firmy Microsoft, **Streaming punkt końcowy** reprezentuje przesyłanie strumieniowe usługa, która zapewnia zawartości bezpośrednio do aplikacji klienckiej odtwarzacza lub do zawartości dostawy sieci (CDN) do dalszej dystrybucji. Usługi multimediów są także integrację Azure CDN. Strumień ruchu wychodzącego z usługi StreamingEndpoint może być strumień na żywo lub klipy wideo na żądanie zawartości na koncie usługi multimediów.

Ponadto można kontrolować możliwości usługa strumieniowego przesyłania punktu końcowego do obsługi rosnącej wymagania dotyczące przepustowości za pomocą dostosowania przesyłanie strumieniowe jednostki. Zalecane jest przydzielić skali jeden lub więcej aplikacji w środowisku produkcyjnym. Przesyłanie strumieniowe jednostki umożliwiają z pojemnością dedykowane wyjściowym, którego można kupić w przyrostach 200 MB/s i dodatkowe funkcje, które zawiera: [dynamiczne opakowań](media-services-dynamic-packaging-overview.md), integracja CDN i Konfiguracja zaawansowana.

>[AZURE.NOTE]Dotyczy tylko są rozliczenie, gdy punkt końcowy Streaming znajduje się w stan uruchomiony.

W tym temacie przedstawiono omówienie funkcji głównym udostępnianych przez Streaming punktów końcowych. Temat również pokazano, jak za pomocą portalu Azure Zarządzanie przesyłanie strumieniowe punktów końcowych. Aby uzyskać informacje o skalowanie przesyłanie strumieniowe punktu końcowego, zobacz [w tym](media-services-portal-scale-streaming-endpoints.md) temacie.

## <a name="start-managing-streaming-endpoints"></a>Rozpocznij zarządzanie przesyłanie strumieniowe punkty końcowe

Aby rozpocząć, zarządzanie przesyłanie strumieniowe punkty końcowe dla Twojego konta, wykonaj następujące czynności.

1. W [portalu Azure](https://portal.azure.com/)wybierz swoje konto usługi multimediów Azure.
2. W oknie **Ustawienia** wybierz pozycję **Streaming punktów końcowych**.

    ![Przesyłanie strumieniowe punktu końcowego](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints1.png)

##<a name="adddelete-a-streaming-endpoint"></a>Dodawanie i usuwanie przesyłanie strumieniowe punktu końcowego

Na dodawanie/usuwanie przesyłanie strumieniowe punktu końcowego za pomocą portalu Azure, wykonaj następujące czynności:

1. Aby dodać punkt końcowy przesyłanie strumieniowe, kliknij znak **+ punkt końcowy** w górnej części strony. 
2. Aby usunąć punkt końcowy przesyłanie strumieniowe, naciśnij przycisk **Usuń** . 

    Nie można usunąć domyślnego streaming punktu końcowego.
2. Kliknij przycisk **Uruchom** , aby uruchomić przesyłanie strumieniowe punktu końcowego.

    ![Przesyłanie strumieniowe punktu końcowego](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints2.png)

Domyślnie możesz mieć do dwóch przesyłanie strumieniowe punktów końcowych. Jeśli potrzebujesz zażądać więcej, zobacz [przydziałów](media-services-quotas-and-limitations.md).
    
##<a id="configure_streaming_endpoints"></a>Konfigurowanie przesyłania strumieniowego punktu końcowego

Przesyłanie strumieniowe punktu końcowego umożliwia skonfigurować następujące właściwości, gdy masz co najmniej 1 jednostka skali: 

- Kontrola dostępu
- Kontrolka pamięci podręcznej
- Krzyżowe zasady dostępu do witryny

Aby uzyskać szczegółowe informacje na temat tych właściwości zobacz [StreamingEndpoint](https://msdn.microsoft.com/library/azure/dn783468.aspx).

Przesyłanie strumieniowe punktu końcowego można skonfigurować, wykonując następujące czynności:

1. Wybierz punkt końcowy przesyłanie strumieniowe, który chcesz skonfigurować.
1. Kliknij przycisk **Ustawienia**.
  
Krótki opis pola są uwzględniane.

![Przesyłanie strumieniowe punktu końcowego](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints4.png)
  
1. Zasady maksymalny pamięci podręcznej: umożliwia konfigurowanie okres istnienia pamięci podręcznej trwałych obsługiwane za pośrednictwem tego przesyłanie strumieniowe punktu końcowego. Jeśli jest ustawiona wartość nie, jest używana wartość domyślna. Domyślne wartości można także zdefiniować bezpośrednio w magazynie Azure. Po włączeniu Azure CDN przesyłanie strumieniowe punktu końcowego należy nie ustaw wartość zasad pamięci podręcznej na mniej niż 600 sekund.  

2. Dozwolonych adresów IP: można określić adresy IP, które będzie mógł połączyć się opublikowanych końcowy przesyłanie strumieniowe z. Jeśli nie adresy IP określone dowolny adres IP będzie mogła połączyć. Adresy IP można określić jako jeden adres IP (na przykład "10.0.0.1"), zakresu adresów IP przy użyciu adresu IP i maski podsieci CIDR (na przykład "10.0.0.1/22") lub zakresu adresów IP przy użyciu adresu IP i maski kropkowaną podsieci dziesiętne (na przykład "10.0.0.1(255.255.255.0)').

3. Konfiguracja uwierzytelniania nagłówka podpisu Akamai: można określić, jak skonfigurowano żądanie uwierzytelnienia podpis nagłówka z Akamai serwerów. Wygasanie jest w czasie UTC.



##<a id="enable_cdn"></a>Włącz integrację Azure CDN

Możesz określić, aby włączyć integrację Azure CDN dla punktu końcowego Streaming (go jest domyślnie wyłączona.)

Aby ustawić integracji Azure CDN PRAWDA:

- Przesyłanie strumieniowe punktu końcowego musi mieć co najmniej jedną jednostkę przesyłanie strumieniowe. Jeśli później chcesz ustawić skali 0, musisz najpierw wyłączyć integrację CDN. Domyślnie podczas tworzenia nowego strumieniowego przesyłania punkt końcowy jednej jednostki przesyłanie strumieniowe jest automatycznie ustawione.

- Przesyłanie strumieniowe punkt końcowy musi być w stanie zatrzymania. Po włączeniu otrzymuje CDN można uruchomić przesyłanie strumieniowe punktu końcowego. 

Może zająć do 90 min. integracji Azure CDN ustawiany dla.  Wystarczy dwie godziny zmian ma być aktywna wśród wszystkich CDN POP.

Integracja CDN jest włączona w wszystkich centrów danych Azure: nam Zachód, nam wschód, Europy Północnej, Europa Zachodnia, zachód Japonii, Wschodu Japonii, Południowej Azji Wschodniej i Azji Wschodniej.

Po włączeniu, konfiguracji **Kontrola dostępu** jest wyłączone.

![Przesyłanie strumieniowe punktu końcowego](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints5.png)

>[AZURE.IMPORTANT] Azure integracji usługi multimediów z sieci CDN Azure jest zaimplementowana w **Sieci CDN Azure z Verizon**.  Jeśli chcesz używać **CDN Azure z Akamai** dla usługi multimediów Azure, należy [ręcznie skonfigurować punkt końcowy](../cdn/cdn-create-new-endpoint.md).  Aby uzyskać więcej informacji o funkcjach Azure CDN zobacz [Omówienie CDN](../cdn/cdn-overview.md).

###<a name="additional-considerations"></a>Uwagi dodatkowe

- Po włączeniu CDN dla przesyłanie strumieniowe punktu końcowego klientów nie może żądać zawartości bezpośrednio od punktu początkowego. Jeśli potrzebna jest możliwość Przetestuj swoją zawartość z lub bez CDN, możesz utworzyć innego przesyłanie strumieniowe punktu końcowego, który nie jest włączony CDN.
- Usługi Przesyłanie strumieniowe hostname punkt końcowy zmienia się po włączeniu CDN. Nie musisz wprowadzić zmiany do przepływu pracy usługi multimediów po włączeniu CDN. Na przykład w przypadku usługi Przesyłanie strumieniowe hostname punktu końcowego strasbourg.streaming.mediaservices.windows.net, po włączeniu CDN, jest używana dokładnie samej nazwa hosta.
- Dla nowych punktów końcowych przesyłanie strumieniowe możesz włączyć CDN po prostu, tworząc nowy punkt końcowy; dla istniejących przesyłanie strumieniowe punktów końcowych należy najpierw zatrzymać punkt końcowy, a następnie włączyć CDN.
 

Aby uzyskać więcej informacji, zobacz [informowanie o usługi multimediów Azure Integracja z CDN Azure (sieć dostarczania zawartości)](http://azure.microsoft.com/blog/2015/03/17/announcing-azure-media-services-integration-with-azure-cdn-content-delivery-network/).


##<a name="next-steps"></a>Następne kroki

Przejrzyj ścieżki szkoleniowe dla użytkowników usługi multimediów.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
 
