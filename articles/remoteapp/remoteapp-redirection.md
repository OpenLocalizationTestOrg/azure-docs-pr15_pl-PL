<properties
    pageTitle="Używanie przekierowywania w Azure RemoteApp | Microsoft Azure"
    description="Dowiedz się, jak konfigurowanie i używanie przekierowania w RemoteApp"
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

# <a name="using-redirection-in-azure-remoteapp"></a>Używanie przekierowywania w Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp jest są przerywane. W tym artykule [anons](https://go.microsoft.com/fwlink/?linkid=821148) , aby uzyskać szczegółowe informacje.

Przekierowywanie urządzeń umożliwia użytkownikom interakcję zdalnego aplikacji za pomocą urządzeń podłączonych do komputera lokalnego, telefonów i tabletów. Na przykład jeśli podano programu Skype przy użyciu funkcji RemoteApp Azure, użytkownika musi kamery zainstalowany na komputerze do pracy z programem Skype. To samo dotyczy dla drukarki, głośników monitorów i urządzeń zewnętrznych połączonych z zakresu USB.

Funkcja RemoteApp wykorzystuje protokół RDP (Remote Desktop) i funkcja RemoteFX do przekierowywania.

## <a name="what-redirection-is-enabled-by-default"></a>Jakie przekierowania jest domyślnie włączone?
Gdy używasz RemoteApp następujących przekierowań są domyślnie włączone. Informacje zawarte w nawiasach Pokaż ustawienie RDP.

- Odtwarzanie dźwięków na komputerze lokalnym (**odtwarzać na tym komputerze**). (audiomode:i:0)
- Przechwyć audio z komputera lokalnego i wysłać do komputera zdalnego (**rekord z tego komputera**). (audiocapturemode:i:1)
- Drukowanie do drukarek lokalnych (redirectprinters:i:1)
- Porty COM (redirectcomports:i:1)
- Karty inteligentnej urządzenia (redirectsmartcards:i:1)
- Schowek (możliwość kopiowania i wklejania) (redirectclipboard:i:1)
- Wyczyść wygładzanie czcionek typu (Zezwalaj na wygładzanie czcionek: i:1)
- Przekierowywanie urządzeń wszystkich obsługiwanych wtyczki and Play. (devicestoredirect:s: *)

## <a name="what-other-redirection-is-available"></a>Jakie inne przekierowania jest dostępna?
Dwie opcje przekierowania są domyślnie wyłączone:

- Przekierowywanie dysków (dysk mapowanie): zamapowanych dysków w sesji zdalnej są zamieniane na dyskach na komputerze lokalnym. Podczas pracy w zdalnej sesji w ten sposób zapisywania lub otwierać pliki z lokalnych dyskach twardych.
- Przekierowywanie USB: korzystając z urządzeń USB podłączonych do komputera lokalnego w zdalnej sesji.

## <a name="change-your-redirection-settings-in-remoteapp"></a>Zmienianie ustawień przekierowania w RemoteApp
Możesz zmienić ustawienia przekierowywania urządzeń dla zbioru przy użyciu programu Microsoft Azure PowerShell SDK. Po zainstalowaniu nowego programu PowerShell i zestaw SDK, skonfiguruj go, aby zarządzać subskrypcją, zgodnie z opisem w [temacie jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md).

Aby ustawić właściwości niestandardowe RDP użyć polecenia podobny do następującego:

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "drivestoredirect:s:*`nusbdevicestoredirect:s:*"

(Należy zauważyć, że ten *n* jest używany jako separator poszczególnych właściwości).

Aby uzyskać listę jakich niestandardowych właściwości RDP są skonfigurowane, uruchom następujące polecenie cmdlet. Zwróć uwagę, że tylko niestandardowe właściwości są wyświetlane jako wynik i nie domyślne właściwości:  

    Get-AzureRemoteAppCollection -CollectionName <collection name>

Po ustawieniu właściwości niestandardowe należy określić wszystkie właściwości niestandardowe zawsze; w przeciwnym razie ustawienie powraca do wyłączenia.   

### <a name="common-examples"></a>Przykłady typowych
Użyj następującego polecenia cmdlet, aby włączyć przekierowywanie dysków:  

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "drivestoredirect:s:*”

Użyj tego polecenia cmdlet, aby włączyć USB i dysk przekierowania:

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "drivestoredirect:s:*`nusbdevicestoredirect:s:*"

Użyj tego polecenia cmdlet, aby wyłączyć udostępnianie Schowka:  

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "redirectclipboard:i:0”

> [AZURE.IMPORTANT] Pamiętaj całkowicie wylogować wszystkich użytkowników w zbiorze (i nie tylko odłączyć) przed przetestować zmiany. Aby upewnić się, że użytkownicy są całkowicie wylogowany, przejdź na kartę **sesji** w kolekcji w portalu Azure i wyloguj się wszystkich użytkowników, którzy odłączona lub zalogowane. Czasami może potrwać kilka sekund lokalnych dyskach pokazywania w Eksploratorze w ramach tej sesji.

## <a name="change-usb-redirection-settings-on-your-windows-client"></a>Zmienianie ustawień przekierowywania USB na komputerze klienckim systemu Windows

Jeśli chcesz używać przekierowywania USB na komputerze łączącym się RemoteApp, istnieje 2 akcje, które mają mieć miejsce. 1 - Twój administrator musi włączyć funkcję przekierowywania USB na poziomie zbioru przy użyciu programu PowerShell Azure. 2 - na poszczególnych urządzeniach, której chcesz używać przekierowywania USB musisz włączyć zasady grupy, która pozwala. Ten krok należy wykonać te czynności dla każdego użytkownika, którego chce używać przekierowywania USB.

> [AZURE.NOTE] Przekierowywanie USB z Azure RemoteApp jest obsługiwana tylko dla komputerów z systemem Windows.

### <a name="enable-usb-redirection-for-the-remoteapp-collection"></a>Należy włączyć funkcję przekierowywania USB zbioru RemoteApp
Użyj następującego polecenia cmdlet, aby włączyć przekierowywanie USB na poziomie zbioru:

    Set-AzureRemoteAppCollection -CollectionName <collection_name> -CustomRdpProperty "nusbdevicestoredirect:s:*"

### <a name="enable-usb-redirection-for-the-client-computer"></a>Należy włączyć funkcję przekierowywania USB komputera klienckiego

Aby skonfigurować ustawienia przekierowywania USB na komputerze:

1. Otwórz Edytor lokalnych zasad grupy (GPEDIT. MSC). (Uruchamiane gpedit.msc w wierszu polecenia).
2. Otwórz **komputera Konfiguracja administracyjne\Składniki systemu Windows\Usługi pulpitu usług Podłączanie pulpitu Client\RemoteFX USB przekierowywania**.
3. Kliknij dwukrotnie **RDP Zezwalaj na przekierowywanie inne obsługiwane urządzenia USB funkcja RemoteFX z tego komputera**.
4. Wybierz opcję **włączone**, a następnie wybierz **administratorów i użytkowników w praw dostępu do przekierowywania USB funkcja RemoteFX**.
5. Otwórz wiersz polecenia z uprawnieniami administratora i uruchom następujące polecenie:

        gpupdate /force
6. Uruchom ponownie komputer.

Za pomocą narzędzia do zarządzania zasadami grupy tworzyć i stosować zasady przekierowywania USB na wszystkich komputerach w domenie:

1. Zaloguj się do kontrolera domeny jako administrator domeny.
2. Otwieranie konsoli zarządzania zasadami grupy. (Kliknij **Start > Narzędzia administracyjne > Zarządzanie zasadami grupy**.)
3. Przejdź do domeny lub jednostce organizacyjnej, dla której chcesz utworzyć zasady.
4. Kliknij prawym przyciskiem myszy **Domyślnych zasad domeny**, a następnie kliknij przycisk **Edytuj**.
5. Otwórz **komputera Konfiguracja administracyjne\Składniki systemu Windows\Usługi pulpitu usług Podłączanie pulpitu Client\RemoteFX USB przekierowywania**.
6. Kliknij dwukrotnie **RDP Zezwalaj na przekierowywanie inne obsługiwane urządzenia USB funkcja RemoteFX z tego komputera**.
7. Wybierz opcję **włączone**, a następnie wybierz **administratorów i użytkowników w praw dostępu do przekierowywania USB funkcja RemoteFX**.
8. Kliknij **przycisk OK**.  
