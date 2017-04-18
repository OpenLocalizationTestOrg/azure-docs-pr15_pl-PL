<properties
   pageTitle="Skonfigurować połączenie VPN punktu do witryny bramy wirtualną sieć Azure za pomocą portalu klasyczny | Microsoft Azure"
   description="Bezpieczne nawiązywanie połączenia z siecią wirtualną Azure, tworząc połączenie VPN punktu do witryny bramy."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/17/2016"
   ms.author="cherylmc"/>

# <a name="configure-a-point-to-site-connection-to-a-vnet-using-the-classic-portal"></a>Konfigurowanie połączenia punkt do witryny z VNet za pomocą portalu klasyczny

> [AZURE.SELECTOR]
- [Menedżer zasobów — Azure Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
- [Menedżer zasobów — programu PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
- [Klasyczny - Azure Portal](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
- [Klasyczny — klasyczny portalu](vpn-gateway-point-to-site-create.md)

Konfiguracja punktu do witryny (P2S) pozwala utworzyć bezpiecznego połączenia z pojedynczym komputerem klienckim do wirtualnej sieci. Połączenie P2S jest przydatne, gdy chcesz się połączyć z VNet z lokalizacji zdalnej, takich jak z domu lub konferencji, lub gdy masz tylko kilku klientów, którzy potrzebują nawiązania połączenia wirtualnej sieci.

W tym artykule opisano tworzenie VNet za pomocą połączenia punkt do witryny, w **modelu Klasyczny wdrażania** za pomocą **portalu klasyczny**.

Połączenia punkt-witryny nie wymagają urządzenia VPN lub adres IP publicznej do pracy. Połączenie VPN jest określany przez uruchamiania połączenia na komputerze klienckim. Aby uzyskać więcej informacji o połączeniach punktu do witryny zobacz [Bramy sieci VPN — często zadawane pytania](vpn-gateway-vpn-faq.md#point-to-site-connections) i [Planowanie i projektowanie](vpn-gateway-plan-design.md).


### <a name="deployment-models-and-methods-for-p2s-connections"></a>Modeli wdrażania i metody P2S połączeń

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 

W poniższej tabeli przedstawiono dwa modele wdrożenia i metody wdrażania dostępne w przypadku konfiguracji P2S. Po udostępnieniu artykułu z kroków konfiguracji pracujemy łącze do niego bezpośrednio z tej tabeli.

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-table-point-to-site-include.md)] 



## <a name="basic-workflow"></a>Podstawowe przepływu pracy

![Punkt do-— diagram witryny] (./media/vpn-gateway-point-to-site-create/p2sclassic.png "punkt do witryny")
 
Poniższe kroki szczegółową procedurę tworzenia bezpiecznego połączenia punkt do witryny z wirtualnej sieci. 

Konfiguracja połączenia punkt do witryny jest podzielony na sekcje cztery. Kolejność każdej z tych sekcji Konfigurowanie jest ważna. Nie Pomiń kroki lub przejść.

- **Sekcja 1** Tworzenie wirtualnych sieci i bramy sieci VPN.
- **Sekcja 2** Tworzenie certyfikatów używany do uwierzytelniania i przekaż je.
- **Sekcja 3** Eksportowanie i zainstaluj własnych certyfikatów klienta.
- **Sekcja 4** Konfigurowanie klienta VPN.

## <a name="vnetvpn"></a>Sekcja 1 — Tworzenie wirtualnych sieci i bramą VPN

### <a name="part-1-create-a-virtual-network"></a>Część 1: Tworzenie wirtualnych sieci

1. Zaloguj się do [portalu klasyczny Azure](https://manage.windowsazure.com/). Poniższe czynności za pomocą portalu klasyczny nie portal Azure. Obecnie nie można tworzyć połączenia P2S przy użyciu Azure portal.

2. W lewym dolnym rogu ekranu kliknij przycisk **Nowy**. W okienku nawigacji kliknij pozycję **Usługi sieciowe**, a następnie kliknij **Wirtualnej sieci**. Kliknij przycisk **Utwórz niestandardowy** , aby uruchomić Kreatora konfiguracji.

3. Na stronie **Szczegóły wirtualna sieć** wprowadź następujące informacje, a następnie kliknij strzałkę następnego w prawej dolnej.
    - **Nazwa**: Nazwa wirtualnej sieci. Na przykład "VNet1". Jest to nazwa, którego będą dotyczyć po wdrożeniu maszyny wirtualne ten VNet.
    - **Lokalizacja**: lokalizacji dotyczy bezpośrednio do lokalizacji fizycznej (region), w którym chcesz zasobów pośrednictwem (SMS) do znajdują się. Na przykład jeśli chcesz, aby maszyny wirtualne, które mają być rozmieszczone z tą siecią wirtualną się znajdować się w USA wschodniego, zaznacz tej lokalizacji. Nie można zmienić regionu skojarzone z siecią wirtualną po jego utworzeniu.

4. Na stronie **serwery DNS i połączenia VPN** wprowadź następujące informacje, a następnie kliknij strzałkę następnego w prawej dolnej.
    - **Serwery DNS**: Wprowadź nazwę serwera DNS i adres IP lub wybierz serwer DNS wcześniej zarejestrowanych w menu skrótów. To ustawienie nie tworzy serwera DNS. Umożliwia określenie serwerów DNS, które ma być używany do rozpoznawania nazw dla tej wirtualnej sieci. Do korzystania z usługi rozpoznawania nazw domyślnych Azure, w tej sekcji pozostaw puste.
    - **Konfigurowanie punktu do witryny VPN**: zaznacz pole wyboru.

5. Na stronie **Łączność punktu do witryny** Określ zakres adresów IP, z którego klientów sieci VPN zostanie wyświetlony adres IP podczas połączenia. Istnieje kilka zasady dotyczące zakresów adresów, które można określić. Należy zweryfikować, że określony zakres nie zachodzi z dowolnego z zakresów znajdujących się w sieci lokalnej.

6. Wprowadź następujące informacje, a następnie kliknij strzałkę w następnym.
 - **Przestrzeń adresów**: obejmuje IP początkowej i CIDR (liczba adresów).
 - **Dodaj adres miejsca**: Dodawanie przestrzeni adresów tylko wtedy, gdy jest wymagana dla projektu sieci.

7. Na stronie **Wirtualna sieć adres spacje** określić zakres adresów, który ma być używany dla wirtualnych sieci. Są to dynamiczne adresy IP (SPADKU) przypisanych do maszyny wirtualne i inne wystąpienia roli, które wdrażanie z tą siecią wirtualną.<br><br>Jest szczególnie ważne zaznaczyć zakres, aby nie nakładały się z dowolnego z zakresów, które są używane w sieci lokalnej. Musisz będą dopasowane do administratora sieci, który może być konieczne dotycząca szczególnych się zakres adresów IP z obszaru adres sieci lokalnej należy użyć na potrzeby wirtualnej sieci.

8. Wprowadź następujące informacje, a następnie kliknij znacznik wyboru, aby rozpocząć tworzenie wirtualnych sieci.
 - **Przestrzeń adresów**: Dodawanie wewnętrznych zakresu adresów IP, który ma być używany dla tej wirtualnej sieci, w tym uruchamianie IP i liczba. Należy zaznaczyć zakres, aby nie nakładały się z dowolnego z zakresów, które są używane w sieci lokalnej. 
 - **Dodaj podsieci**: dodatkowe podsieci nie są wymagane, ale warto utworzyć osobną podsieć dla maszyny wirtualne zawierające SPADKU statyczne. Możesz też w podsieci, która różni się od innych wystąpień roli jest pośrednictwem usługi SMS.
 - **Dodaj bramy podsieci**: podsieć bramy jest wymagane na potrzeby sieci VPN punktu do witryny. Kliknij, aby dodać podsieci bramy. Podsieci bramy jest używany tylko w przypadku Brama wirtualnej sieci.

9. Po utworzeniu wirtualna sieć zostanie wyświetlona **utworzono** wyświetlana w obszarze **stanu** na stronie sieci w portalu klasyczny Azure. Po utworzeniu wirtualnej sieci można tworzyć dynamiczne centrum routingu.

### <a name="part-2-create-a-dynamic-routing-gateway"></a>Część 2: Tworzenie dynamiczne bramy routingu

Typ bramy musi być skonfigurowany jako dynamiczne. Statyczne routingu bram nie działa z tą funkcją.

1. W portalu klasyczny Azure, na stronie **sieci** kliknij wirtualną sieć, która została utworzona i przejdź do strony **pulpitu nawigacyjnego** .

2. Kliknij przycisk **Utwórz bramy**, znajduje się w dolnej części strony **pulpitu nawigacyjnego** . Pojawi się monit **Czy chcesz utworzyć bramę dla wirtualnej sieci "VNet1"**. Kliknij przycisk **Tak,** aby rozpocząć tworzenie bramy. Może potrwać około 15 minut bramy utworzyć.

## <a name="generate"></a>Sekcja 2 — Generowanie i przekaż certyfikatów

Certyfikaty są używane do uwierzytelniania klientów sieci VPN sieci VPN punktu do witryny. Można użyć certyfikatu głównego wygenerowane przez rozwiązanie enterprise certyfikatu, lub można użyć certyfikatu z podpisem własnym. Możesz przekazać maksymalnie 20 certyfikatów głównych Azure. Po przesłaniu plik cer Azure umożliwia informacje zawarte w niej uwierzytelniania klientów, którzy mają zainstalowany certyfikat klienta. Certyfikat klienta musi być wygenerowany z tego samego certyfikatu, która reprezentuje plik cer.

W tej sekcji będą wykonaj następujące czynności:

- Uzyskaj plik cer dla certyfikatu głównego. To może być certyfikat z podpisem własnym lub można użyć systemu certyfikat przedsiębiorstwa.
- Przekaż plik cer Azure.
- Generowanie certyfikatów klienta.

### <a name="root"></a>Część 1: Uzyskaj plik cer dla certyfikatu głównego

Jeśli korzystasz z systemu certyfikat enterprise, uzyskaj plik cer dla certyfikat główny, który ma być używany. W [część 3](#createclientcert)możesz wygenerować certyfikaty klientów z certyfikatu głównego.

Jeśli nie używasz rozwiązanie enterprise certyfikatu, musisz wygenerować certyfikat główny podpisem. Instrukcje dotyczące systemu Windows 10 można używać odwołań do [pracy z certyfikatów głównych podpisem konfiguracji punktu do witryny](vpn-gateway-certificates-point-to-site.md). Artykuł przeprowadzi Cię przez przy użyciu makecert, aby wygenerować certyfikat z podpisem własnym, a następnie wyeksportuj plik cer.

### <a name="upload"></a>Część 2: Przekaż plik cer certyfikatów głównych do portalu klasyczny Azure

Dodawanie zaufanego certyfikatu Azure. Po dodaniu pliku zakodowany Base64 X.509 (cer) Azure sprawi, że Azure zaufania dla certyfikatu głównego, która reprezentuje plik.

1. W portalu klasyczny Azure, na stronie **Certyfikaty** wirtualnej sieci kliknij przycisk **Przekaż certyfikatu głównego**.

2. Na stronie **Przekazywanie certyfikatu** Przeglądaj w poszukiwaniu cer certyfikat główny, a następnie kliknij znacznik wyboru.

### <a name="createclientcert"></a>Część 3: Generowanie certyfikat klienta

Następnie generować certyfikaty klienta. Istnieje możliwość wygenerowania certyfikatu unikatowe dla każdego klienta, który będzie łączyć lub za pomocą tego samego certyfikatu na wielu klientów. Zaletą do generowania unikatowych certyfikatów jest możliwość odwoływanie pojedynczego certyfikatu, w razie potrzeby. W przeciwnym razie jeśli wszyscy użytkownicy korzystają z tego samego certyfikatu klienta i okaże się trzeba odwołać certyfikat dla jednego klienta, będzie konieczne wygenerować i instalowanie nowych certyfikatów dla wszystkich klientów, które służą do uwierzytelniania za pomocą certyfikatu.

- Jeśli korzystasz z rozwiązania certyfikat enterprise wygenerować certyfikat klienta z popularnym formatem wartość Nazwa 'name@yourdomain.com', zamiast formatowanie NetBIOS "Domena". 

- Jeśli używasz certyfikatu z podpisem własnym, zobacz [Praca z certyfikatów głównych podpisem konfiguracji punktu do witryny](vpn-gateway-certificates-point-to-site.md) , aby wygenerować certyfikat klienta.

## <a name="installclientcert"></a>Sekcja 3 — eksportowanie i zainstaluj certyfikat klienta

Instalowanie certyfikatu klienta na każdym komputerze, który chcesz połączyć się z siecią wirtualną. Certyfikat klienta jest wymagany do uwierzytelniania. Instalowanie certyfikatu klienta można zautomatyzować lub można ręcznie zainstalować. Poniższe kroki przeprowadził Cię przez eksportowanie i ręczne instalowanie certyfikatu klienta.

1. Aby wyeksportować certyfikat klienta, możesz użyć *certmgr.msc*. Kliknij prawym przyciskiem myszy certyfikat klienta, który chcesz eksportu, kliknij polecenie **Wszystkie zadania**, a następnie kliknij polecenie **Eksportuj**.
2. Wyeksportuj certyfikat klienta z kluczem prywatnym. Jest to plik *pfx* . Upewnij się zarejestrować lub Zapamiętaj hasło (klucz) można ustawić ten certyfikat.
3. Skopiuj plik *pfx* na komputer kliencki. Na komputerze klienckim kliknij dwukrotnie plik *pfx* , aby go zainstalować. Wprowadź hasło na żądanie. Nie należy modyfikować lokalizację instalacji.

## <a name="vpnclientconfig"></a>Sekcja 4 — Konfigurowanie klienta VPN

Aby nawiązać połączenie wirtualnej sieci, należy skonfigurować klienta VPN. Klient wymaga zarówno certyfikat klienta i pisane z wielkiej litery konfiguracji klienta VPN w celu połączenia. Aby skonfigurować klienta sieci VPN, wykonaj następujące czynności w kolejności.

### <a name="part-1-create-the-vpn-client-configuration-package"></a>Część 1: Tworzenie pakietu konfiguracji klienta VPN

1. W portalu klasyczny Azure, na stronie **pulpitu nawigacyjnego** wirtualnej sieci przejdź do menu Szybkie rzut w prawym rogu. Na liście systemów operacyjnych klientów, które są obsługiwane, zobacz połączenia punkt witryny [](vpn-gateway-vpn-faq.md#point-to-site-connections) sekcji często zadawane pytania VPN bramy. Pakiet klienta VPN zawiera informacje o konfiguracji skonfigurować oprogramowanie klienta VPN wbudowane w systemie Windows. Pakiet nie powoduje zainstalowania dodatkowego oprogramowania. Ustawienia są specyficzne dla wirtualnej sieci, którą chcesz nawiązać połączenie.<br><br>Wybierz pakiet do pobrania odpowiadający poziomowi, na którym zostanie zainstalowany system operacyjny klienta:
 - Dla klientów 32-bitową wybierz pozycję **Pobierz 32-bitowej klienta VPN**.
 - Dla klientów 64-bitowej wybierz pozycję **Pobierz pakiet sieci VPN klienta 64-bitowej**.

2. Wystarczy kilka minut, aby utworzyć pakietu klienta. Po zakończeniu pakietu mogą pobrać plik. Plik *.exe* , który można pobrać mogą być bezpiecznie przechowywane na komputerze lokalnym.

3. Po wygenerować i Pobierz pakiet klienta VPN z portalu klasyczny Azure, możesz zainstalować pakiet klienta na komputerze klienckim, z którego chcesz utworzyć połączenie z siecią wirtualną. Jeśli planujesz zainstalować pakiet klienta VPN na wielu komputerach klienckich, upewnij się, że każdy również jest zainstalowany certyfikat klienta.

### <a name="part-2-install-the-vpn-configuration-package-on-the-client"></a>Część 2: Zainstaluj pakiet konfiguracji sieci VPN klienta

1. Skopiuj plik konfiguracji lokalnie na komputerze, na którym chcesz nawiązać połączenie z siecią wirtualną i kliknij dwukrotnie plik .exe. 

2. Po pakiet został zainstalowany, możesz rozpocząć połączenie VPN. Pakiet konfiguracji nie jest podpisany przez firmę Microsoft. Może chcesz włączać pakiet przy użyciu Twojej organizacji usługi podpisywania lub zaloguj się przy użyciu [SignTool]( http://go.microsoft.com/fwlink/p/?LinkId=699327). Jest przycisk OK, aby użyć pakietu bez konieczności logowania. Jednak jeśli pakiet nie jest zarejestrowany, zostanie wyświetlone ostrzeżenie podczas instalowania pakietu.

3. Na komputerze klienckim przejdź do **Ustawień sieci** i kliknij pozycję **sieć VPN**. Zostanie wyświetlona na liście połączenia. Nazwa wirtualną sieć będzie widoczny będzie łączyć się i będzie wyglądać podobnie do następującej: 

    ![Klient sieci VPN] (./media/vpn-gateway-point-to-site-create/vpn.png "Klient sieci VPN")


### <a name="part-3-connect-to-azure"></a>Część 3: Nawiązywanie połączenia z platformy Azure

1. Aby nawiązać połączenie z VNet na komputerze klienckim, przejdź do połączeń sieci VPN i Znajdź połączenie VPN utworzone przez Ciebie. Nazywa się taką samą nazwę jak wirtualnej sieci. Kliknij przycisk **Połącz**. Podręczne może pojawić się komunikat odwołujący się do za pomocą certyfikatu. W takim przypadku kliknij przycisk **Kontynuuj** , aby użyć podwyższonym poziomem uprawnień. 

2. Na stronie Stan **połączenia** kliknij przycisk **Połącz** w celu rozpoczęcia połączenia. Jeśli zostanie wyświetlony ekran **Wybierz certyfikat** , sprawdź, czy certyfikat klienta przedstawiający jest ten, który ma być używany do łączenia. Jeśli nie jest dostępne, użyj strzałkę listy rozwijanej, aby wybrać poprawnego certyfikatu, a następnie kliknij **przycisk OK**.

    ![Klienta VPN 2] (./media/vpn-gateway-point-to-site-create/clientconnect.png "Połączenie VPN klienta")

3. Teraz należy ustanowić połączenie.

    ![Klient sieci VPN 3] (./media/vpn-gateway-point-to-site-create/connected.png "Połączenie VPN klienta 2")

### <a name="part-4-verify-the-vpn-connection"></a>Część 4: Sprawdź połączenie VPN

1. Aby sprawdzić, czy połączenie VPN jest aktywne, otwórz wiersz polecenia z podwyższonym poziomem uprawnień i uruchom *polecenie ipconfig/all*.
2. Wyświetlanie wyników. Zauważ, że adres IP otrzymany jest jednym z adresów w zakresie adresów łączności punktu do witryny określonej podczas tworzenia usługi VNet. Wyniki powinny być podobną do następującej:

Przykład:



    PPP adapter VNet1:
        Connection-specific DNS Suffix .:
        Description.....................: VNet1
        Physical Address................:
        DHCP Enabled....................: No
        Autoconfiguration Enabled.......: Yes
        IPv4 Address....................: 192.168.130.2(Preferred)
        Subnet Mask.....................: 255.255.255.255
        Default Gateway.................:
        NetBIOS over Tcpip..............: Enabled

## <a name="next-steps"></a>Następne kroki

Możesz dodać maszyn wirtualnych z siecią wirtualną. Dowiedz się, [jak utworzyć niestandardowe maszyny wirtualnej](../virtual-machines/virtual-machines-windows-classic-createportal.md).

Jeśli chcesz, aby uzyskać więcej informacji o wirtualnych sieciach, zobacz stronę [Dokumentacji wirtualnej sieci](https://azure.microsoft.com/documentation/services/virtual-network/) .
