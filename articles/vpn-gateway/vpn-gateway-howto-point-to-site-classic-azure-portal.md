<properties
   pageTitle="Konfigurowanie połączenie VPN punktu do witryny bramy wirtualną sieć Azure za pomocą portalu Azure | Microsoft Azure"
   description="Bezpieczne nawiązywanie połączenia z siecią wirtualną Azure, tworząc połączenie bramy sieci VPN punktu do witryny za pomocą portalu Azure."
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

# <a name="configure-a-point-to-site-connection-to-a-vnet-using-the-azure-portal"></a>Konfigurowanie połączenia punkt do witryny z VNet za pomocą portalu Azure

> [AZURE.SELECTOR]
- [Menedżer zasobów — Azure Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
- [Menedżer zasobów — programu PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
- [Klasyczny - Azure Portal](vpn-gateway-howto-point-to-site-classic-azure-portal.md)

W tym artykule opisano tworzenie VNet za pomocą połączenia punkt do witryny, w modelu Klasyczny wdrażania za pomocą portalu Azure. Konfiguracja punktu do witryny (P2S) pozwala utworzyć bezpiecznego połączenia z pojedynczym komputerem klienckim do wirtualnej sieci. Połączenie P2S jest przydatny, gdy chcesz się połączyć z VNet z lokalizacji zdalnej, takich jak z domu lub konferencji. Lub, jeśli masz tylko kilku klientów, którzy potrzebują nawiązania połączenia wirtualnej sieci.

Połączenia punkt-witryny nie wymagają urządzenia VPN lub adres IP publicznej do pracy. Połączenie VPN jest określany przez uruchamiania połączenia na komputerze klienckim. Aby uzyskać więcej informacji o połączeniach punktu do witryny zobacz [Bramy sieci VPN — często zadawane pytania](vpn-gateway-vpn-faq.md#point-to-site-connections) i [O Brama VPN](vpn-gateway-about-vpngateways.md#point-to-site).

### <a name="deployment-models-and-methods-for-p2s-connections"></a>Modeli wdrażania i metody P2S połączeń

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 

W poniższej tabeli przedstawiono dwa modele wdrożenia i metody wdrażania dostępne w przypadku konfiguracji P2S. Po udostępnieniu artykułu z kroków konfiguracji możemy łącze do niego bezpośrednio z tej tabeli.

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-table-point-to-site-include.md)] 

## <a name="basic-workflow"></a>Podstawowe przepływu pracy 

![Punkt do-— diagram witryny] (./media/vpn-gateway-howto-point-to-site-rm-ps/p2srm.png "punkt do witryny")


Poniżej znajdziesz szczegółową procedurę tworzenia bezpiecznego połączenia punkt do witryny z wirtualnej sieci. 

1. Tworzenie wirtualnych sieci i bramy sieci VPN
2. Generowanie certyfikatów
3. Przekaż plik cer
4. Generowanie pakietu konfiguracji klienta VPN
5. Skonfigurować komputer kliencki
6. Nawiązywanie połączenia z platformy Azure

### <a name="example-settings"></a>Przykład ustawień

Można wykonać następujące ustawienia przykładzie:

- **Nazwa: VNet1**
- **Przestrzeń adresowa: 192.168.0.0/16**
- **Nazwa podsieci: serwera sieci Web**
- **Zakres adresów podsieci: 192.168.1.0/24**
- **Subskrypcji:** Jeśli masz więcej niż jedną subskrypcję, upewnij się, że korzystasz z właściwą.
- **Grupa zasobów: TestRG**
- **Lokalizacja: Wschodniego w Stanach Zjednoczonych**
- **Typ połączenia: punkt do witryny**
- **Przestrzeni adres klienta: 172.16.201.0/24**. Klienci sieci VPN, łączących się VNet za pomocą tego połączenia punkt do witryny otrzymać adres IP od określonej puli.
- **GatewaySubnet: 192.168.200.0/24**. Podsieć bramy, należy użyć nazwy "GatewaySubnet".
- **Rozmiar:** Wybierz pozycję bramy SKU, którego chcesz użyć.
- **Typ routingu: dynamiczne**

## <a name="vnetvpn"></a>Sekcja 1 — Tworzenie wirtualnych sieci i bramą VPN

### <a name="createvnet"></a>Część 1: Tworzenie wirtualnych sieci

Jeśli nie masz jeszcze wirtualnej sieci, utwórz je. Zrzuty ekranu są dostarczane jako przykłady. Należy zastąpić własnym wartości. Aby utworzyć VNet za pomocą portalu Azure, wykonaj następujące czynności: 

1. Za pomocą przeglądarki, przejdź do [portalu Azure](http://portal.azure.com) i, jeśli to konieczne, zaloguj się przy użyciu konta Azure.

2. Kliknij przycisk **Nowy**. W polu **wyszukiwania rynku** wpisz "Wirtualnej sieci". Znajdź **Wirtualną sieć** na liście zwróconych i kliknij, aby otworzyć karta **Wirtualnej sieci** .

    ![Wyszukiwanie karta wirtualnej sieci] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/newvnetportal700.png "Wyszukiwanie karta wirtualnej sieci")

3. W dolnej części karta wirtualnej sieci, z listy **Wybierz model wdrożenia** wybierz **Klasyczny**, a następnie kliknij **Utwórz**.

    ![Wybierz model wdrożenia] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/selectmodel.png "Wybierz model wdrożenia")

4. Na karta **Tworzenie wirtualnych sieci** konfigurować ustawienia VNet. W tym karta możesz dodać pierwszego obszaru adres i zakres adresów pojedyncza podsieć. Po zakończeniu tworzenia VNet, możesz Przejdź wstecz i dodać dodatkowe podsieci i adres spacje.

    ![Tworzenie wirtualnych sieci karta] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/vnet125.png "Tworzenie wirtualnych sieci karta")

5. Sprawdź, czy **Subskrypcja** jest poprawny. Subskrypcje można zmienić przy użyciu listy rozwijanej.

6. Kliknij pozycję **Grupa zasobów** , a następnie wybierz istniejącej grupy zasobów lub Utwórz nową przez wpisanie nazwy nowej grupy zasobów. W przypadku tworzenia nowej grupy Nazwa grupy zasobów według wartości planowanej konfiguracji. Aby uzyskać więcej informacji dotyczących grup zasobów odwiedź stronę [Azure Omówienie Menedżera zasobów](azure-resource-manager/resource-group-overview.md#resource-groups).

7. Następnie wybierz **lokalizację** ustawień usługi VNet. Lokalizacja określa miejsce, w którym znajdują się zasoby, które wdrożeniem tego VNet.

8. Wybierz pozycję **Przypnij do pulpitu nawigacyjnego** , jeśli chcesz można było odnalezienie usługi VNet na pulpicie nawigacyjnym, a następnie kliknij przycisk **Utwórz**.
    
    ![Przypnij do pulpitu nawigacyjnego] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/pintodashboard150.png "Przypnij do pulpitu nawigacyjnego")


9. Po kliknięciu pozycji Utwórz, zostanie wyświetlony kafelka do pulpitu nawigacyjnego, odzwierciedlająca postępu usługi VNet. Kafelek zmieni VNet jest tworzony.

    ![Tworzenie wirtualnych sieci kafelków] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/deploying150.png "Tworzenie wirtualnych sieci kafelków")

10. Po utworzeniu wirtualnej sieci, możesz dodać adres IP serwera DNS w celu obsługi rozpoznawania nazw. Otwórz ustawień wirtualnych sieci, kliknij pozycję serwery DNS i Dodaj adres IP serwera DNS, który ma być używany. To ustawienie nie tworzy nowy serwer DNS. Pamiętaj dodać serwer DNS zasobów można komunikować się z.

Po utworzeniu wirtualna sieć zostanie wyświetlona **utworzono** wyświetlana w obszarze **stanu** na stronie sieci w portalu klasyczny Azure.

### <a name="gateway"></a>Część 2: Tworzenie podsieci bramy i dynamiczne bramy routingu

W tym kroku utworzysz podsieci bramy i bramę routingu dynamicznego. W portalu Azure modelu Klasyczny wdrożenia tworzenia podsieci bramy i bramy może być wykonywane przez samej karty konfiguracji.

1. W portalu przejdź do wirtualnej sieci, dla której chcesz utworzyć bramę.

2. Wybierz polecenie karta dla wirtualnych sieci, na karta **Przegląd** , w sekcji połączenia VPN **bramy**.

    ![Kliknij tutaj, aby utworzyć bramę] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/beforegw125.png "Kliknij tutaj, aby utworzyć bramę")


3. Na karta **Nowe połączenie VPN** wybierz punkt witryny ****.

    ![Typ połączenia P2S] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/newvpnconnect.png "Typ połączenia P2S")

4. W przypadku **Przestrzeni adres klienta**należy dodać zakres adresów IP. To zakres, z którego klientów sieci VPN zostanie wyświetlony adres IP, podczas nawiązywania połączenia. Usuń zakres wypełnione automatycznie i dodać własne.

    ![Przestrzeń adresów klienta] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clientaddress.png "Przestrzeń adresów klienta")

5. Zaznacz pole wyboru **Utwórz bramy natychmiast** .

    ![Tworzenie bramy natychmiast] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/creategwimm.png "Tworzenie bramy natychmiast")

6. Kliknij pozycję **konfiguracji opcjonalnie bramy** , aby otworzyć karta **konfiguracji bramy** .

    ![Kliknij pozycję Konfiguracja opcjonalnie bramy] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/optsubnet125.png "Kliknij pozycję Konfiguracja opcjonalnie bramy")

7. Kliknij opcję **Konfigurowanie podsieci wymagane ustawienia** , aby dodać **podsieci bramy**. Gdy użytkownik może utworzyć podsieć bramy jak /29, zaleca się utworzenie większych podsieci, która zawiera więcej adresów, zaznaczając co najmniej /28 lub /27. Dzięki temu będzie za mało adresów zezwalały na możliwe dodatkowych ustawień, które warto w przyszłości.

    >[AZURE.IMPORTANT] Podczas pracy z podsieci bramy, unikaj, kojarząc go z grupy zabezpieczeń sieci (NSG) do podsieci bramy. Kojarzenie grupy zabezpieczeń sieci do danej podsieci może spowodować bramy sieci VPN przestanie działać zgodnie z oczekiwaniami. Aby uzyskać więcej informacji na temat grup zabezpieczeń sieci zobacz [Co to jest grupa zabezpieczeń sieci?](../articles/virtual-network/virtual-networks-nsg.md)

    ![Dodawanie GatewaySubnet] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/gwsubnet125.png "Dodawanie GatewaySubnet")

8. Wybierz **rozmiar**bramy. Jest to bramy SKU, które będzie używane do tworzenia Centrum wirtualnej sieci. W portalu SKU domyślne to **Podstawowa**. Aby uzyskać więcej informacji na temat bramy wersji produktu zobacz [Temat ustawienia bramy sieci VPN](../articles/vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md#gwsku).

    ![rozmiar bramy](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/gwsize125.png)

9. Wybierz **Typ routingu** usługi bramy. Konfiguracje P2S wymagają typu routingu **dynamicznego** . Po zakończeniu konfigurowania to karta, kliknij **przycisk OK** .

    ![Konfigurowanie routingu typ] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/routingtype125.png "Konfigurowanie routingu typ")

10. Na karta **Nowe połączenie VPN** kliknij **przycisk OK** u dołu karta, aby rozpocząć tworzenie Centrum wirtualnej sieci. To może potrwać do 45 minut. 


## <a name="generatecerts"></a>Sekcja 2 — generowanie certyfikatów

Certyfikaty są używane przez Azure do uwierzytelniania klientów sieci VPN sieci VPN punktu do witryny. Jak Base 64 zakodowany plik cer X.509 z certyfikatu głównego wygenerowane przez rozwiązanie enterprise certyfikatu lub certyfikat główny podpisem eksportowania danych certyfikatu publicznego (nie klucz prywatny). Następnie zaimportować dane certyfikatu publicznego z certyfikatu głównego Azure. Ponadto należy wygenerować certyfikat klienta z certyfikatu głównego dla klientów. Każdy klient, którą chce się połączyć z wirtualnej sieci przy użyciu połączenia P2S musi mieć zainstalowany certyfikat klienta wygenerowany z certyfikatu głównego.

### <a name="cer"></a>Część 1: Uzyskaj plik cer dla certyfikatu głównego


Jeśli korzystasz z rozwiązania enterprise, możesz użyć do istniejącego łańcuch certyfikatów. Jeśli nie używasz jednostka rozwiązanie urzędu certyfikacji, możesz utworzyć certyfikat z podpisem własnym głównego. Jednym ze sposobów tworzenia podpisem certyfikatu jest makecert.

- Jeśli korzystasz z systemu certyfikat enterprise, uzyskaj plik cer dla certyfikat główny, który ma być używany. 

- Jeśli nie używasz rozwiązanie enterprise certyfikatu, należy wygenerować certyfikat główny podpisem. Instrukcje dotyczące systemu Windows 10 można używać odwołań do [pracy z certyfikatów głównych podpisem konfiguracji punktu do witryny](vpn-gateway-certificates-point-to-site.md).

1. Aby uzyskać plik cer z certyfikatu, otwórz **certmgr.msc** i odszukaj certyfikat główny. Kliknij prawym przyciskiem myszy certyfikat główny podpisem, kliknij pozycję **Wszystkie zadania**i kliknij przycisk **Eksportuj**. Spowoduje to otwarcie **Kreatora eksportu certyfikatów**.

2. W kreatorze kliknij przycisk **Dalej**, zaznacz **nie Eksportuj klucz prywatny**, a następnie kliknij przycisk **Dalej**.

3. Na stronie **Format pliku eksportu** wybierz **Base-64 zakodowany X.509 (. CER).** Następnie kliknij przycisk **Dalej**. 

4. W **pliku do wyeksportowania**, **Przejdź** do lokalizacji, do której chcesz wyeksportować certyfikat. W polu **Nazwa pliku**nadaj nazwę pliku certyfikatu. Następnie kliknij przycisk **Dalej**.

5. Kliknij przycisk **Zakończ** , aby wyeksportować certyfikat.


### <a name="genclientcert"></a>Część 2: Generowanie certyfikat klienta

Istnieje możliwość wygenerowania certyfikatu unikatowe dla każdego klienta, który będzie łączyć lub za pomocą tego samego certyfikatu na wielu klientów. Zaletą do generowania unikatowych certyfikatów jest możliwość odwoływanie pojedynczego certyfikatu, w razie potrzeby. W przeciwnym razie jeśli wszyscy użytkownicy korzystają z tego samego certyfikatu klienta i okaże się trzeba odwołać certyfikat dla jednego klienta, będzie konieczne wygenerować i instalowanie nowych certyfikatów dla wszystkich klientów, które służą do uwierzytelniania za pomocą tego certyfikatu.

- Jeśli korzystasz z rozwiązania certyfikat enterprise wygenerować certyfikat klienta z popularnym formatem wartość Nazwa 'name@yourdomain.com', zamiast format "domeny\nazwa". 

- Jeśli używasz certyfikatu z podpisem własnym, zobacz [Praca z certyfikatów głównych podpisem konfiguracji punktu do witryny](vpn-gateway-certificates-point-to-site.md) , aby wygenerować certyfikat klienta.

### <a name="exportclientcert"></a>Część 3: Eksportowanie certyfikatu klienta

Instalowanie certyfikatu klienta na każdym komputerze, który chcesz połączyć się z siecią wirtualną. Certyfikat klienta jest wymagany do uwierzytelniania. Instalowanie certyfikatu klienta można zautomatyzować, lub możesz zainstalować go ręcznie. Poniższe kroki przeprowadził Cię przez eksportowanie i ręczne instalowanie certyfikatu klienta.

1. Aby wyeksportować certyfikat klienta, możesz użyć *certmgr.msc*. Kliknij prawym przyciskiem myszy certyfikat klienta, który chcesz eksportu, kliknij polecenie **Wszystkie zadania**, a następnie kliknij polecenie **Eksportuj**.
2. Wyeksportuj certyfikat klienta z kluczem prywatnym. Jest to plik *pfx* . Upewnij się zarejestrować lub Zapamiętaj hasło (klucz) można ustawić ten certyfikat.

## <a name="upload"></a>Sekcja 3 — Przekaż plik cer certyfikatów głównych

Po utworzeniu bramy, możesz przekazać plik cer dla zaufanego certyfikatu głównego Azure. Możesz przekazać pliki maksymalnie 20 certyfikatów głównych. Nie przekazuj klucz prywatny certyfikatu głównego Azure. Po przesłaniu plik cer Azure używany do uwierzytelniania klientów łączących się z siecią wirtualną.

1. W sekcji **połączenia VPN** karta dla swojego VNet kliknij grafikę **klientów** , aby połączenie VPN Otwórz **punkt do witryny** karta.

    ![Klienci] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clients125.png "Klienci")

2. Połączenia punkt witryny **** karta, kliknij pozycję **Zarządzaj certyfikaty** otworzyć karta **Certyfikaty** .<br>

    ![Karta certyfikatów](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/ptsmanage.png "Certificates blade")<br><br>


3. Karta **Certyfikaty** polecenie **Przekaż** otworzyć karta **przekazywanie certyfikatu** .<br>

    ![Przekazywanie karta certyfikatów](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/uploadcerts.png "Upload certificates blade")<br>


4. Kliknij grafikę folderu, aby odszukać plik cer. Zaznacz plik, a następnie kliknij **przycisk OK**. Odśwież stronę, aby wyświetlić przekazane certyfikatu na karta **Certyfikaty** .

    ![Przekazywanie certyfikatu](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/upload.png "Upload certificate")<br>
    

## <a name="vpnclientconfig"></a>Sekcja 4 - generowanie pakietu konfiguracji klienta VPN

Aby nawiązać połączenie wirtualnej sieci, należy skonfigurować klienta VPN. Komputer kliencki wymaga zarówno certyfikat klienta i pisane z wielkiej litery pakiet konfiguracji klienta VPN w celu połączenia.

Pakiet klienta VPN zawiera informacje o konfiguracji skonfigurować oprogramowanie klienta VPN wbudowane w systemie Windows. Pakiet nie powoduje zainstalowania dodatkowego oprogramowania. Ustawienia są specyficzne dla wirtualnej sieci, którą chcesz nawiązać połączenie. Na liście systemów operacyjnych klientów, które są obsługiwane, zobacz połączenia punkt witryny [](vpn-gateway-vpn-faq.md#point-to-site-connections) sekcji często zadawane pytania VPN bramy. 

### <a name="to-generate-the-vpn-client-configuration-package"></a>Aby wygenerować pakietu konfiguracji klienta VPN

1. W portalu Azure karta **Przegląd** dla swojego VNet w oknie **połączenia VPN**kliknij grafikę klienta, aby połączenie VPN Otwórz **punkt do witryny** karta.
2. W górnej części **punkt do witryny sieci VPN połączenia** karta, kliknij pozycję Pobierz pakiet odpowiadający poziomowi, na którym zostanie zainstalowany system operacyjny klienta:

 - W przypadku klientów 64-bitowej wybierz **Klienta VPN (wersja 64-bitowa)**.
 - W przypadku klientów 32-bitową wybierz **Klienta VPN (32-bitowa)**.

    ![Pobierz pakiet konfiguracji klienta VPN](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/dlclient.png "Download VPN client configuration package")<br>


3. Pojawi się komunikat, że Azure generuje pakiet konfiguracji klienta VPN wirtualnej sieci. Po kilku minutach pakietu jest generowana i zostanie wyświetlony komunikat na komputerze lokalnym, że pakiet został pobrany. Zapisz plik pakietu konfiguracji. Spowoduje to zainstaluj na każdym komputerze klienckim, który będzie łączyć się wirtualnej sieci przy użyciu P2S.


## <a name="clientconfiguration"></a>Sekcja 5 - skonfigurować komputer kliencki

### <a name="part-1-install-the-client-certificate"></a>Część 1: Zainstaluj certyfikat klienta

Każdy komputer kliencki musi mieć certyfikat klienta w celu uwierzytelnienia. Podczas instalowania certyfikat klienta, konieczne będzie hasło, które zostały utworzone podczas wyeksportowano certyfikat klienta.

1. Skopiuj plik pfx na komputer kliencki.
2. Kliknij dwukrotnie plik PFX, aby go zainstalować. Nie należy modyfikować lokalizację instalacji.

### <a name="part-2-install-the-vpn-client-configuration-package"></a>Część 2: Zainstaluj pakiet konfiguracji klienta VPN

Umożliwia tego samego pakietu konfiguracji klienta VPN na każdym komputerze klienckim, pod warunkiem, że wersja zgodna Architektura klienta.

1. Skopiuj plik konfiguracji lokalnie na komputerze, na którym chcesz nawiązać połączenie z siecią wirtualną i kliknij dwukrotnie plik .exe. 

2. Po pakiet został zainstalowany, możesz rozpocząć połączenie VPN. Pakiet konfiguracji nie jest podpisany przez firmę Microsoft. Może chcesz włączać pakiet przy użyciu Twojej organizacji usługi podpisywania lub zaloguj się przy użyciu [SignTool]( http://go.microsoft.com/fwlink/p/?LinkId=699327). Jest przycisk OK, aby użyć pakietu bez konieczności logowania. Jednak jeśli pakiet nie jest zarejestrowany, zostanie wyświetlone ostrzeżenie podczas instalowania pakietu.

3. Na komputerze klienckim przejdź do **Ustawień sieci** i kliknij pozycję **sieć VPN**. Zostanie wyświetlona na liście połączenia. Nazwa wirtualną sieć jest wyświetlany połączy się i będzie wyglądać podobnie do następującej: 

    ![Klient sieci VPN] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/vpn.png "Klient sieci VPN VNet")

## <a name="connect"></a>Sekcja 6 - nawiązywanie połączenia z platformy Azure

### <a name="connect-to-your-vnet"></a>Nawiązywanie połączenia z VNet

1. Aby nawiązać połączenie z VNet na komputerze klienckim, przejdź do połączeń sieci VPN i Znajdź połączenie VPN utworzone przez Ciebie. Nazywa się taką samą nazwę jak wirtualnej sieci. Kliknij przycisk **Połącz**. Podręczne może pojawić się komunikat odwołujące się do za pomocą certyfikatu. W takim przypadku kliknij przycisk **Kontynuuj** , aby użyć podwyższonym poziomem uprawnień. 

2. Na stronie Stan **połączenia** kliknij przycisk **Połącz** w celu rozpoczęcia połączenia. Jeśli zostanie wyświetlony ekran **Wybierz certyfikat** , sprawdź, czy certyfikat klienta przedstawiający jest ten, który ma być używany do łączenia. Jeśli nie jest dostępne, użyj strzałkę listy rozwijanej, aby wybrać poprawnego certyfikatu, a następnie kliknij **przycisk OK**.

    ![Nawiązywanie połączenia] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clientconnect.png "Połączenie VPN klienta")

3. Teraz należy ustanowić połączenie.

    ![Utworzonej połączenia] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/connected.png "Nawiązać połączenia")

### <a name="verify-the-vpn-connection"></a>Sprawdź połączenie VPN

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