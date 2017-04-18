<properties 
    pageTitle="Jak przekierowywać USB urządzeń w Azure RemoteApp? | Microsoft Azure" 
    description="Dowiedz się, jak za pomocą przekierowywania dla urządzeń USB w Azure RemoteApp." 
    services="remoteapp" 
    documentationCenter="" 
    authors="lizap" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />



# <a name="how-do-you-redirect-usb-devices-in-azure-remoteapp"></a>Jak przekierowywać urządzenia USB w Azure RemoteApp?

> [AZURE.IMPORTANT]
> Azure RemoteApp jest są przerywane. W tym artykule [anons](https://go.microsoft.com/fwlink/?linkid=821148) , aby uzyskać szczegółowe informacje.

Przekierowywanie urządzeń umożliwia użytkownikom za pomocą urządzeń USB podłączonych do ich komputera lub tabletu, za pomocą aplikacji w Azure RemoteApp. Na przykład jeśli udostępniony programu Skype przy użyciu funkcji RemoteApp Azure, użytkownicy muszą być mógł korzystać z ich aparaty urządzenia.

Przed przejściem do dalszych, upewnij się, że przeczytaj informacje przekierowywania USB w [Przekierowanie używanie w Azure RemoteApp](remoteapp-redirection.md). Jednak zalecane nusbdevicestoredirect:s: * nie będzie działać dla kamer internetowych USB i może nie działać dla niektórych drukarek USB lub urządzenia wielofunkcyjne USB. Zgodnie z projektem i ze względów bezpieczeństwa administrator Azure RemoteApp musi włączyć funkcję przekierowywania identyfikator GUID klasy urządzenia lub identyfikator wystąpienia urządzenia, zanim użytkownicy mogą za pomocą tych urządzeń.

Mimo że ten artykuł zawiera informacje o przekierowywania aparatu fotograficznego w sieci web, umożliwia podobne podejście przekierowywania drukarek USB i inne urządzenia wielofunkcyjne USB, które nie są przekierowywane przez **nusbdevicestoredirect:s:*** polecenia.

## <a name="redirection-options-for-usb-devices"></a>Opcje przekierowania urządzeń USB
Azure RemoteApp używa mechanizmy bardzo podobne do przekierowywania urządzenia USB jako znaczników dostępnych dla usług pulpitu zdalnego. Technologia wewnętrzna umożliwia wybranie metody poprawne przekierowywania dla danego urządzenia, jak najpełniej zarówno wysokiego poziomu i urządzenie USB funkcja RemoteFX przy użyciu przekierowywania **usbdevicestoredirect:s:** polecenia. Istnieją cztery elementy do tego polecenia:

| Kolejność przetwarzania | Parametr           | Opis                                                                                                                |
|------------------|---------------------|----------------------------------------------------------------------------------------------------------------------------|
| 1                | *                   | Zaznacza wszystkie urządzenia, które nie są pobierane przez wysokiego poziomu przekierowywania. Uwaga: zgodnie z projektem * nie działa w przypadku kamer internetowych USB.  |
|                  | {Identyfikator GUID klasy urządzenia} | Zaznacza wszystkich urządzeń, zgodne klasę konfiguracji określonego urządzenia.                                                           |
|                  | USB\InstanceID      | Zaznacza urządzenie USB określone dla danego wystąpienia identyfikatora.                                                                  |
| 2                | -Identyfikator USB\Instance    | Usuwa ustawienia przekierowania dla określonego urządzenia.                                                                 |

## <a name="redirecting-a-usb-device-by-using-the-device-class-guid"></a>Przekierowywanie urządzenie USB przy użyciu identyfikator GUID klasy urządzenia
Istnieją dwa sposoby znajdowania klasę urządzeń identyfikator GUID, który może być używany do przekierowywania. 

Pierwszą opcję jest użycie [System-Defined urządzenia konfiguracji klasy dostępne dla dostawców](https://msdn.microsoft.com/library/windows/hardware/ff553426.aspx). Wybierz klasy, który najlepiej pasuje do urządzenia podłączonego do komputera lokalnego. Aparaty cyfrowe może to być klasy urządzenia do obrazowania lub urządzenie przechwytywanie wideo. Musisz wykonać kilka eksperymentowanie z klas urządzeń, aby znaleźć poprawne klasy GUID działającego w programie podłączonych lokalnie urządzenie USB (w przypadku naszych kamery internetowej).

Lepiej lub drugą opcję, jest wykonaj poniższe czynności, aby znaleźć identyfikator GUID klasy konkretnego urządzenia:

1. Otwórz Menedżera urządzeń, znajdź urządzenie, które będą przekierowywani i kliknij go prawym przyciskiem myszy, a następnie otwórz właściwości.
![Otwórz Menedżera urządzeń](./media/remoteapp-usbredir/ra-devicemanager.png)
2. Na karcie **Szczegóły** wybierz właściwość **Identyfikator Guid klasy**. Wartość, która jest wyświetlana jest identyfikator GUID klasy dla danego typu urządzenia.
![Właściwości kamery](./media/remoteapp-usbredir/ra-classguid.png)
3. Użyj wartości z pola Identyfikator Guid klasy, aby przekierować urządzenia, które są zgodne z jego.

Na przykład:

        Set-AzureRemoteAppCollection -CollectionName <collection name> -CustomRdpProperty "nusbdevicestoredirect:s:<Class Guid value>"

Można połączyć wiele przekierowania urządzenia w tym samym polecenia cmdlet. Na przykład: Aby przekierować lokalne przechowywanie i kamera internetowa USB, polecenia cmdlet wygląda następująco:

        Set-AzureRemoteAppCollection -CollectionName <collection name> -CustomRdpProperty "drivestoredirect:s:*`nusbdevicestoredirect:s:<Class Guid value>"

Po ustawieniu przekierowywania przez identyfikator GUID klasy przekierowanie wszystkich urządzeń, które są zgodne z tym identyfikator GUID klasy w określonej kolekcji. Na przykład w przypadku wielu komputerach w sieci lokalnej samej kamer internetowych USB można uruchamiać pojedynczego polecenia cmdlet, aby przekierować wszystkie kamer internetowych.

## <a name="redirecting-a-usb-device-by-using-the-device-instance-id"></a>Przekierowywanie urządzenie USB przy użyciu Identyfikatora wystąpienia urządzenia

Jeśli dokładniej określić szerokiego i chcesz zachować kontrolę nad przekierowania na urządzenie, można użyć parametru przekierowywania **USB\InstanceID** .

Najtrudniejsze część ta metoda jest znajdowanie identyfikatora wystąpienia urządzenie USB Musisz mieć dostęp do komputera i urządzenie USB. Następnie wykonaj następujące czynności:

1. Włączanie przekierowywania w sesji pulpitu zdalnego, zgodnie z opisem w [jak używać urządzeń i zasobów w sesji pulpitu zdalnego?](http://windows.microsoft.com/en-us/windows7/How-can-I-use-my-devices-and-resources-in-a-Remote-Desktop-session)
2. Otwórz Podłączanie pulpitu zdalnego i kliknij pozycję **Pokaż opcje**.
3. Kliknij pozycję **Zapisz jako** , aby zapisać bieżące ustawienia połączenia z plikiem RDP.  
    ![Zapisywanie ustawień jako plik RDP](./media/remoteapp-usbredir/ra-saveasrdp.png)
4. Wybierz nazwę pliku i lokalizację, na przykład "MyConnection.rdp" i "To PC\Documents" i Zapisz plik.
5. Otwórz plik MyConnection.rdp za pomocą edytora tekstów i Znajdź identyfikator wystąpienia urządzenia, które chcesz przekierować.

Teraz Użyj identyfikator wystąpienia w następujące polecenie cmdlet:

    Set-AzureRemoteAppCollection -CollectionName <collection name> -CustomRdpProperty "nusbdevicestoredirect:s: USB\<Device InstanceID value>"



### <a name="help-us-help-you"></a>Pomóż nam ułatwiają 
Czy wiesz, oprócz ocena niniejszego artykułu i wyrażania opinii w dół poniżej, umożliwia zmiany do tego samego artykułu? Coś Brak? Problem? Pisanie jest coś, co jest po prostu mylące czy? Przewijanie w górę, a następnie kliknij przycisk **Edytuj na GitHub** , aby wprowadzić zmiany — te ułatwiających kontakt z nami do recenzji, a następnie po możemy zalogować się nad nimi, zostanie wyświetlony do zmiany i ulepszenia w tym artykule.