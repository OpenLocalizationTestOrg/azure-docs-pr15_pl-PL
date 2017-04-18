<properties
    pageTitle="Komputerach i urządzeniach nawiązać połączenie przy użyciu bramy usługi OMS usługi OMS | Microsoft Azure"
    description="Podłącz zarządzane usługi OMS urządzeń i monitorować programu Operations Manager komputerach z bramą usługi OMS wysyłanie danych do usługę, które nie mają dostęp do Internetu."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>
<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/26/2016"
    ms.author="banders"/>

# <a name="connect-computers-and-devices-to-oms-using-the-oms-gateway"></a>Komputerach i urządzeniach nawiązać połączenie przy użyciu bramy usługi OMS usługi OMS

W tym dokumencie opisano, jak zarządzane usługi OMS urządzenia i monitorować System Center Operations Manager SCOM komputerów mogą wysyłać dane do usługę gdy nie masz dostęp do Internetu. Brama usługi OMS można zbierania danych i wyślij ją do usługę w imieniu tej osoby.

Przesyłanie dalej serwer proxy HTTP obsługującego Tunelowanie HTTP przy użyciu polecenia POŁĄCZ HTTP jest bramy. Brama może obsługiwać maksymalnie 2000 urządzeń usługi OMS jednocześnie połączony uruchomienia na Procesora 4 core, 16 GB serwera z systemem Windows.

Na przykład przedsiębiorstwie lub dużej organizacji może być serwery z połączeniem w sieci, ale być może nie masz połączenia z Internetem. Inny przykład może być wiele punkt sprzedaży (punkt sprzedaży) urządzeń z środków monitorowania je bezpośrednio. I inny przykład programu Operations Manager mogą używać bramy usługi OMS jako serwer proxy. W tych przykładach bramy usługi OMS mogą przesyłać danych od czynników, które są zainstalowane na tych serwerach lub urządzeniami punktu sprzedaży do usługi OMS.

Zamiast każdego agenta poszczególnych wysyłaniu danych bezpośrednio do usługi OMS i wymaganie bezpośredniego połączenia z Internetem wszystkie dane agenta zamiast tego są wysyłane za pośrednictwem pojedynczego komputera połączenie internetowe. Ten komputer to miejsce, w którym Instalowanie i używanie bramy. W tym scenariuszu czynników można zainstalować na wszystkich komputerach, na której chcesz zbierać dane. Brama następnie przekaże danych od agentów usługi OMS bezpośrednio — bramy nie analizuje jakichkolwiek przesyłanych danych.

Aby monitorować bramy usługi OMS i analizowania wydajności i zdarzeń danych na serwerze, na którym jest zainstalowany, należy zainstalować agenta usługi OMS na komputerze również zainstalowanym bramy.

Brama musi mieć dostęp do Internetu, aby przekazać danych do usługi OMS. Każdy agent musi być łączność sieciowa do bramy, aby agentów automatycznie przenosić dane do i z bramy. Aby uzyskać najlepsze wyniki nie można zainstalować bramy na komputerze, na którym jest również kontrolera domeny.

Poniżej przedstawiono diagram przedstawiający przepływ danych czynnikami bezpośrednio do usługi OMS.

![diagram bezpośredni agenta](./media/log-analytics-oms-gateway/direct-agent-diagram.png)

Poniżej przedstawiono diagram przedstawiający przepływ danych z programu Operations Manager do usługi OMS.

![Diagram menedżera operacji](./media/log-analytics-oms-gateway/scom-mgt-server.png)

## <a name="install-the-oms-gateway"></a>Instalowanie usługi OMS bramy

Instalowanie tej bramy zastępuje poprzednich wersji bramy, że zainstalowano (analizy w dzienniku usługi przesyłania dalej).

Wymagania wstępne: .net Framework 4,5 w systemie Windows Server 2012 R2 z dodatkiem SP1 lub wyższej

1. Pobierz najnowszą wersję usługi OMS bramy w [Centrum pobierania Microsoft](http://download.microsoft.com/download/2/5/C/25CF992A-0347-4765-BD7D-D45D5B27F92C/OMS%20Gateway.msi).
2. Aby rozpocząć instalację, kliknij dwukrotnie **Usługi OMS Gateway.msi**.
3. Na stronie powitalnej **Dalej**.  
    ![Kreator konfiguracji bramy](./media/log-analytics-oms-gateway/gateway-wizard01.png)
4. Na stronie Umowa licencyjna wybierz **akceptuję postanowienia Umowy licencyjnej** , aby zaakceptować umowę licencyjną użytkownika oprogramowania, a następnie **Dalej**.
5. Na stronie adres portu i serwera proxy:
    1. Wpisz numer portu TCP, który ma być używany dla bramy. Konfiguracja zostanie otwarty ten numer portu z zapory systemu Windows. Wartość domyślna to 8080.
    Prawidłowe zakresy numer portu to od 1 do 65535. Jeśli dane wejściowe nie mieści się w tym zakresie, zostanie wyświetlony komunikat o błędzie.
    2. Opcjonalnie Jeśli serwer miejsce, w którym jest zainstalowana brama musi być serwer proxy, wpisz adres serwera proxy, gdzie należy połączyć bramy. Na przykład `http://myorgname.corp.contoso.com:80` Jeśli pole jest puste, bramy próbuje nawiązać połączenie z Internetem bezpośrednio. W przeciwnym razie bramy łączy do serwera proxy. Jeśli serwer proxy wymaga uwierzytelniania, wpisz nazwę użytkownika i hasło.
        ![Konfiguracji serwera proxy kreatora bramy](./media/log-analytics-oms-gateway/gateway-wizard02.png)  
    3. Kliknij przycisk **Dalej**
6. Jeśli nie masz Microsoft Updates włączone, zostanie wyświetlona strona Microsoft Update, gdzie można włączyć Microsoft Updates. Wybierz opcję, a następnie kliknij przycisk **Dalej**. W przeciwnym razie przejdź do następnego kroku.
7. Na stronie Folder docelowy pozostaw domyślny folder **%ProgramFiles%\OMS bramy** lub wpisz lokalizację, w której chcesz zainstalować bramę, a następnie kliknij przycisk **Dalej**.
8. Na karcie Gotowe do zainstalowania strony kliknij przycisk **Zainstaluj**. Kontrola konta użytkownika mogą być wyświetlane żądaniu uprawnień do zainstalowania. Jeśli tak, kliknij przycisk **Tak**.
9. Po zakończeniu instalacji kliknij przycisk **Zakończ**. Można sprawdzić, czy usługa jest uruchomiona, otwierając przystawki services.msc i sprawdź, czy **Usługi OMS brama** pojawia się na liście usług.  
    ![Usługi — bramy usługi OMS](./media/log-analytics-oms-gateway/gateway-service.png)

## <a name="install-an-agent-on-devices"></a>Instalowanie agenta na urządzeniach

Jeśli to konieczne, zobacz [Łączenie komputery do analizy dziennika](log-analytics-windows-agents.md) dowiedzieć się, jak zainstalować bezpośrednio połączonych czynników. W artykule opisano, jak można zainstalować agenta za pomocą Kreatora konfiguracji lub przy użyciu wiersza polecenia.

## <a name="configure-oms-agents"></a>Konfigurowanie wykluczonych agentów usługi OMS

Zobacz [Konfigurowanie ustawień serwera proxy i zapory z programem Microsoft Agent monitorowania](log-analytics-proxy-firewall.md) informacji o konfigurowaniu agenta w celu korzystania z serwera proxy, czyli w tym przypadku jest bramy.

Operacje Menedżera agentów Wyślij niektóre dane, takie jak programu Operations Manager alertów, ocena konfiguracji miejsca wystąpienia i pojemnością danych, za pośrednictwem serwera zarządzania. Inne dane dużych, takie jak dzienniki programu IIS, wydajność i bezpieczeństwo są wysyłane bezpośrednio do bramy usługi OMS. Pełną listę danych, które są wysyłane za pośrednictwem każdego kanału, zobacz [Dodawanie analizy dziennika rozwiązań z galerii rozwiązań](log-analytics-add-solutions.md) .

>[AZURE.NOTE]
Jeśli planujesz używać bramy z równoważenia obciążenia sieciowego, zobacz [Konfigurowanie opcjonalnie równoważenia obciążenia sieciowego](#optionally-configure-network-load-balancing).

## <a name="configure-a-scom-proxy-server"></a>Konfigurowanie serwera proxy SCOM

Konfigurowanie programu Operations Manager, aby dodać brama ma pełnić rolę serwera proxy. Gdy zaktualizujesz konfiguracji serwera proxy, konfiguracji serwera proxy zostanie automatycznie zastosowana do wszystkich agentów raportowania programu Operations Manager.

Aby użyć bramy do obsługi programu Operations Manager, muszą być:

- Monitorowanie agenta firmy Microsoft (wersja agenta — **8.0.10900.0** lub w nowszej wersji) zainstalowane na serwerze bramy i skonfigurowane dla usługi OMS obszarów roboczych, z którymi chcesz się komunikować.
- Brama musi mieć połączenie z Internetem lub być połączony z serwera proxy, które wykonuje.

### <a name="to-configure-scom-for-the-gateway"></a>Aby skonfigurować SCOM bramy

1. Otwieranie konsoli programu Operations Manager i w obszarze **Pakiet administracyjny operacji**, kliknij **połączenie** , a następnie kliknij przycisk **Konfiguracji serwera Proxy**:  
    ![Konfigurowanie programu Operations Manager — serwera Proxy](./media/log-analytics-oms-gateway/scom01.png)
2. Zaznacz pole wyboru **Użyj serwera proxy, aby uzyskać dostęp do pakietu zarządzania operacji** , a następnie wpisz adres IP serwera usługi OMS bramy. Upewnij się, że początkowych `http://` prefiks:  
    ![Programu Operations Manager — adres serwera proxy](./media/log-analytics-oms-gateway/scom02.png)
3. Kliknij przycisk **Zakończ**. Serwer programu Operations Manager jest podłączony do obszaru roboczego usługi OMS.

## <a name="configure-network-load-balancing"></a>Konfigurowanie równoważenia obciążenia sieciowego

Możesz skonfigurować bramę wysokiej dostępności za pomocą równoważenia obciążenia sieciowego za tworzenie klastrze. Klaster zarządza ruchu z czynnikami przekierowując wymagane połączenia czynnikami monitorowania Microsoft jego węzłów. Jeśli jeden serwer bramy przestanie działać, ruch przekierowaniem do innych węzłów.

1. Otwórz Menedżera równoważenia obciążenia sieciowego i utworzyć klaster.
2. Kliknij prawym przyciskiem myszy klaster przed dodaniem bram, a następnie wybierz pozycję **Właściwości klaster.** Skonfiguruj klaster adres IP:  
    ![Menedżer równoważenia obciążenia sieciowego — klaster adresów IP](./media/log-analytics-oms-gateway/nlb01.png)
3. Aby połączyć się z serwerem bramy usługi OMS z zainstalowaną monitorowania agenta firmy Microsoft, kliknij prawym przyciskiem myszy adres IP klaster, a następnie kliknij **Dodaj hosta do klastrów**.  
    ![Sieć ładowanie Menedżera równoważenia — Dodaj hosta do klastrów](./media/log-analytics-oms-gateway/nlb02.png)
4. Wprowadź adres IP serwera bramy, który chcesz się połączyć:  
    ![Sieci Menedżera równoważenia obciążenia — Dodaj hosta do klastrów: nawiązywanie połączenia](./media/log-analytics-oms-gateway/nlb03.png)
5. Na komputerach, które nie mają łączność z Internetem należy użyć adresu IP klaster podczas konfigurowania **Właściwości monitorowania agenta firmy Microsoft**:  
    ![Microsoft monitorowania właściwości agenta — ustawienia serwera Proxy](./media/log-analytics-oms-gateway/nlb04.png)

## <a name="configure-for-automation-hybrid-workers"></a>Konfigurowanie dla pracowników hybrydowych automatyzacji

Jeśli masz automatyzacji hybrydowych pracowników w środowisku, następujące czynności Podaj ręcznego, tymczasowy obejścia, aby skonfigurować bramę do ich obsługi.

W następującej procedurze należy wiedzieć obszaru Azure miejsce, w którym znajduje się konto automatyzacji. Aby znaleźć lokalizację:

1. Zaloguj się do [portalu Azure](https://portal.azure.com/).
2. Wybierz usługę Azure automatyzacji.
3. Wybierz odpowiednie konto Azure automatyzacji.
4. Wyświetlanie jego region w obszarze **Lokalizacja**.  
    ![Portal Azure — Lokalizacja konta automatyzacji](./media/log-analytics-oms-gateway/location.png)

Określ adres URL dla każdej lokalizacji za pomocą poniższych tabelach:

**Adresy URL usługi danych czasu wykonywania zadania**

| **Lokalizacja** | **ADRES URL** |
| --- | --- |
| Ameryka Północna centralnej w Stanach Zjednoczonych | ncus-jobruntimedata zlecenie su1.azure-automation.net |
| Europa Zachodnia | Firma Microsoft jobruntimedata zlecenie su1.azure-automation.net |
| Południowej centralnej Stany Zjednoczone | scus-jobruntimedata zlecenie su1.azure-automation.net |
| Wschodniej Stany Zjednoczone | eus-jobruntimedata zlecenie su1.azure-automation.net |
| Kanada centralnej | DW — jobruntimedata — zlecenie su1.azure-automation.net |
| Europa Północna | n jobruntimedata zlecenie su1.azure-automation.net |
| Południowej wschodnioazjatyckie | morzu jobruntimedata zlecenie su1.azure-automation.net |
| Indie centralnej | CID-jobruntimedata zlecenie su1.azure-automation.net |
| Japonia | jpe-jobruntimedata zlecenie su1.azure-automation.net |
| Australia | ASE-jobruntimedata zlecenie su1.azure-automation.net |

**Adresy URL usługi agenta**

| **Lokalizacja** | **ADRES URL** |
| --- | --- |
| Ameryka Północna centralnej w Stanach Zjednoczonych | ncus-agentservice zlecenie 1.azure-automation.net |
| Europa Zachodnia | Firma Microsoft agentservice zlecenie 1.azure-automation.net |
| Południowej centralnej Stany Zjednoczone | scus-agentservice zlecenie 1.azure-automation.net |
| Wschodniej Stany Zjednoczone | eus2-agentservice zlecenie 1.azure-automation.net |
| Kanada centralnej | DW — agentservice — zlecenie 1.azure-automation.net |
| Europa Północna | n agentservice zlecenie 1.azure-automation.net |
| Południowej wschodnioazjatyckie | morzu agentservice zlecenie 1.azure-automation.net |
| Indie centralnej | CID-agentservice zlecenie 1.azure-automation.net |
| Japonia | jpe-agentservice zlecenie 1.azure-automation.net |
| Australia | ASE-agentservice zlecenie 1.azure-automation.net |

Jeśli na komputerze jest zarejestrowany jako pracownik hybrydowych automatycznie dla poprawki przy użyciu rozwiązania do zarządzania aktualizacji, wykonaj następujące kroki:

1. Dodaj adresy URL usługi danych czasu wykonywania zadania do listy dozwolonych hosta bramy usługi OMS. Na przykład: `Add-OMSGatewayAllowedHost we-jobruntimedata-prod-su1.azure-automation.net`
2. Uruchom ponownie usługę bramy przy użyciu następującego polecenia cmdlet programu PowerShell:`Restart-Service OMSGatewayService`

Jeśli komputer jest na następować do automatyzacji Azure za pomocą polecenia cmdlet hybrydowych pracownik rejestracji, wykonaj następujące kroki:

1. Dodaj adres URL rejestracji usługi agenta do listy dozwolonych hosta bramy usługi OMS. Na przykład:`Add-OMSGatewayAllowedHost ncus-agentservice-prod-1.azure-automation.net`
2. Dodaj adresy URL usługi danych czasu wykonywania zadania do listy dozwolonych hosta bramy usługi OMS. Na przykład: `Add-OMSGatewayAllowedHost we-jobruntimedata-prod-su1.azure-automation.net`
3. Ponownie uruchom usługę bramy.
    `Restart-Service OMSGatewayService`

## <a name="useful-powershell-cmdlets"></a>Przydatne polecenia cmdlet programu PowerShell

Polecenia cmdlet może ułatwić wykonywanie zadań, które są potrzebne, aby zaktualizować ustawienia konfiguracji bramy usługi OMS. Przed ich użyciem, należy:

1. Instalowanie usługi OMS bramy (MSI).
2. Otwórz okno programu PowerShell.
3. Aby zaimportować moduł, wpisz następujące polecenie:`Import-Module OMSGateway`
4. Jeśli wystąpił błąd w poprzednim kroku, moduł zostały pomyślnie zaimportowane, a następnie można używać poleceń cmdlet. Typ`Get-Module OMSGateway`
5. Po wprowadzeniu zmian przy użyciu poleceń cmdlet, upewnij się, uruchom ponownie usługę bramy.

Jeśli zostanie wyświetlony komunikat o błędzie w kroku 3, moduł nie został zaimportowany. Ten błąd może wystąpić, gdy nie może odnaleźć modułu programu PowerShell. Możesz go znaleźć ścieżka instalacji bramy: C:\Program Files\Microsoft usługi OMS Gateway\PowerShell.

| **Polecenie cmdlet** | **Parametry** | **Opis** | **Przykłady** |
| --- | --- | --- | --- |
| `Set-OMSGatewayConfig` | Klucz (wymagany) <br> Wartość | Zmiany konfiguracji usługi | `Set-OMSGatewayConfig -Name ListenPort -Value 8080` |
| `Get-OMSGatewayConfig` | Klawisz | Otrzymuje konfiguracji usługi | `Get-OMSGatewayConfig` <br> <br> `Get-OMSGatewayConfig -Name ListenPort` |
| `Set-OMSGatewayRelayProxy` | Adres <br> Nazwa użytkownika <br> Hasło | Ustawia adres (i poświadczeń) przekazywania serwer proxy (nadrzędny) | 1. Ustaw serwer proxy Odpowiedz i poświadczenia:`Set-OMSGatewayRelayProxy -Address http://www.myproxy.com:8080 -Username user1 -Password 123` <br> <br> 2. Ustaw serwer proxy odpowiedź, która nie wymaga uwierzytelniania:`Set-OMSGatewayRelayProxy -Address http://www.myproxy.com:8080` <br> <br> 3. Wyczyść pole wyboru Odpowiedz serwer proxy, oznacza to, że nie ma potrzeby serwer proxy odpowiedzi:`Set-OMSGatewayRelayProxy -Address ""` |
| `Get-OMSGatewayRelayProxy` |   | Otrzymuje adresu serwera proxy (nadrzędny) przekazywania | `Get-OMSGatewayRelayProxy` |
| `Add-OMSGatewayAllowedHost` | Host (wymagany) | Dodaje hosta do listy dozwolonych | `Add-OMSGatewayAllowedHost -Host www.test.com` |
| `Remove-OMSGatewayAllowedHos`t | Host (wymagany) | Usuwa hosta z listy dozwolonych | `Remove-OMSGatewayAllowedHost -Host www.test.com` |
| `Get-OMSGatewayAllowedHost` |   | Otrzymuje obecnie dozwolonych hosta (lokalnie skonfigurowanej prawo tylko hosta, nie dołączaj automatycznie pobrany dozwolonych hostów) | `Get-OMSGatewayAllowedHost` |
| `Add-OMSGatewayAllowedClientCertificate` | Temat (wymagany) | Dodaje certyfikat klienta objęte listy dozwolonych | `Add-OMSGatewayAllowedClientCertificate -Subject mycert` |
| `Remove-OMSGatewayAllowedClientCertificate` | Temat (wymagany) | Usuwa podmiot certyfikatu klienta z listy dozwolonych | `Remove- OMSGatewayAllowedClientCertificate -Subject mycert` |
| `Get-OMSGatewayAllowedClientCertificat`e |   | Otrzymuje obecnie dozwolonych klienta tematów certyfikatu (tylko lokalnie skonfigurowanej dozwolonych tematu, nie dołączaj automatycznie pobrany tematów dozwolonych) | `Get-OMSGatewayAllowedClientCertificate` |

## <a name="troubleshoot"></a>Rozwiązywanie problemów

Zaleca się zainstalowanie agenta usługi OMS na komputerach, które mają zainstalowany bramy. Agent umożliwia następnie zbierać zdarzenia, które są rejestrowane przez bramę.

![Podgląd zdarzeń — usługi OMS bramy dziennika](./media/log-analytics-oms-gateway/event-viewer.png)

**Identyfikatory zdarzeń bramy usługi OMS i opisy**

W poniższej tabeli przedstawiono identyfikatory zdarzeń i opisy zdarzeń dziennika bramy usługi OMS.

| **IDENTYFIKATOR** | **Opis** |
| --- | --- |
| 400 | Błąd dowolnej aplikacji, której nie ma określony identyfikator |
| 401 | Konfiguracja problem. Na przykład: listenPort = "tekst" zamiast liczba całkowita |
| 402 | Wyjątku podczas analizowania wiadomości uzgadnianie TLS |
| 403 | Błąd sieci. Na przykład: nie można nawiązać połączenia z serwerem docelowym |
| 100 | Informacje ogólne |
| 101 | Usługa została uruchomiona |
| 102 | Usługa została zatrzymana |
| 103 | Otrzymano polecenie HTTP nawiązywanie połączenia z klienta |
| 104 | Nie polecenia Łączenie HTTP |
| 105 | Serwer docelowy nie ma na liście dozwolonych lub port docelowy nie jest bezpieczny portu (443) <br> <br> Upewnij się, że agent MMA na serwerze bramy i czynników komunikowania się z bramy są połączone ze sobą analizy dziennika.|
| 105 | Błąd TcpConnection — certyfikat klienta nieprawidłowe: CN = bramy <br><br> Upewnij się, że: <br> <br> & #149; Brama za pomocą numeru wersji 1.0.395.0 lub nowszej. <br> & #149; Agent MMA na serwerze bramy i czynników komunikowania się z bramy są połączone ze sobą analizy dziennika. |
| 106 | Dowolnego powodu, że sesja TLS podejrzanych i odrzucone |
| 107 | Sesja TLS została zweryfikowana. |

**Liczniki wydajności do**

W poniższej tabeli przedstawiono dostępne liczniki wydajności bramy usługi OMS. Możesz dodać liczniki przy użyciu Monitora wydajności.

| **Nazwa** | **Opis** |
| --- | --- |
| Połączenie klienta bramy i aktywne usługi OMS | Liczbę aktywnych klientów połączeń sieciowych (TCP) |
| Liczba bramy i błędów usługi OMS | Liczba błędów |
| Usługi OMS bramy i połączony klient | Liczba połączonych klientów |
| Liczba bramy i odrzucenia usługi OMS | Liczba odrzucenia ze względu na błąd sprawdzania poprawności dowolnego TLS |

![Liczniki wydajności usługi OMS bramy](./media/log-analytics-oms-gateway/counters.png)


## <a name="get-assistance"></a>Uzyskiwanie pomocy

Jeśli możesz już zalogowany do portalu Azure, możesz utworzyć wniosku o pomoc przy użyciu bramy usługi OMS lub innych usług Azure lub funkcji usługi.
Aby uzyskać pomoc, kliknij symbol znaku zapytania w prawym górnym rogu portalu, a następnie kliknij przycisk **Nowy obsługuje żądania**. Wypełnij formularz nowe żądanie pomocy technicznej.

![Nowe żądanie pomocy technicznej](./media/log-analytics-oms-gateway/support.png)

Można również pozostawić opinii na temat usługi OMS lub analizy dziennika na [forum Microsoft Azure opinii](https://feedback.azure.com/forums/267889).

## <a name="next-steps"></a>Następne kroki

- [Dodawanie źródła danych](log-analytics-data-sources.md) do zbierania danych z połączonych źródeł w obszarze roboczym usługi OMS i zapisać go w repozytorium usługi OMS.
