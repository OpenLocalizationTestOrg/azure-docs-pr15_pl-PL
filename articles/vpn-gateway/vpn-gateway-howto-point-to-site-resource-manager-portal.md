<properties 
   pageTitle="Konfigurowanie połączenie VPN punktu do witryny Brama wirtualnej sieci przy użyciu modelu wdrożenia Menedżera zasobów i Azure portal | Microsoft Azure"
   description="Bezpiecznie połączyć się z siecią wirtualną Azure, tworząc połączenie bramy sieci VPN punktu do witryny za pomocą Menedżera zasobów i Azure portal."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/17/2016"
   ms.author="cherylmc" />

# <a name="configure-a-point-to-site-connection-to-a-vnet-using-the-azure-portal"></a>Konfigurowanie połączenia punkt do witryny z VNet za pomocą portalu Azure

> [AZURE.SELECTOR]
- [Menedżer zasobów — Azure Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
- [Menedżer zasobów — programu PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
- [Klasyczny - Azure Portal](vpn-gateway-howto-point-to-site-classic-azure-portal.md)

Konfiguracja punktu do witryny (P2S) pozwala utworzyć bezpiecznego połączenia z pojedynczym komputerem klienckim do wirtualnej sieci. Połączenie P2S jest przydatne, gdy chcesz się połączyć z VNet z lokalizacji zdalnej, takich jak z domu lub konferencji, lub gdy masz tylko kilku klientów, którzy potrzebują nawiązania połączenia wirtualnej sieci. 

Połączenia punkt-witryny nie wymagają urządzenia VPN lub adres IP publicznej do pracy. Połączenie VPN jest określany przez uruchamiania połączenia na komputerze klienckim. Aby uzyskać więcej informacji o połączeniach punktu do witryny zobacz [Bramy sieci VPN — często zadawane pytania](vpn-gateway-vpn-faq.md#point-to-site-connections) i [Planowanie i projektowanie](vpn-gateway-plan-design.md).

W tym artykule opisano tworzenie VNet za pomocą połączenia punkt do witryny w modelu wdrożenia Menedżera zasobów za pomocą portalu Azure.

### <a name="deployment-models-and-methods-for-p2s-connections"></a>Modeli wdrażania i metody P2S połączeń

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 

W poniższej tabeli przedstawiono dwa modele wdrożenia i metody wdrażania dostępne w przypadku konfiguracji P2S. Po udostępnieniu artykułu z kroków konfiguracji możemy łącze do niego bezpośrednio z tej tabeli.

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-table-point-to-site-include.md)] 

## <a name="basic-workflow"></a>Podstawowe przepływu pracy 

![Punkt do-— diagram witryny] (./media/vpn-gateway-howto-point-to-site-rm-ps/p2srm.png "punkt do witryny")

### <a name="example"></a>Przykładowe wartości

- **Nazwa: VNet1**
- **Przestrzeń adresowa: 192.168.0.0/16**<br>W tym przykładzie używamy miejsca tylko jeden adres. Możesz mieć więcej niż jedna spacja adres dla swojej VNet.
- **Nazwa podsieci: serwera sieci Web**
- **Zakres adresów podsieci: 192.168.1.0/24**
- **Subskrypcji:** Jeśli masz więcej niż jedną subskrypcję, upewnij się, że korzystasz z właściwą.
- **Grupa zasobów: TestRG**
- **Lokalizacja: Wschodniego w Stanach Zjednoczonych**
- **GatewaySubnet: 192.168.200.0/24**
- **Nazwa bramy wirtualną sieć: VNet1GW**
- **Typ bramy: VPN**
- **Typ VPN: oparte na trasę**
- **Publiczny adres IP: VNet1GWpip**
- **Typ połączenia: punkt do witryny**
- **Pula adresów klienta: 172.16.201.0/24**<br>Klienci sieci VPN, łączących się VNet za pomocą tego połączenia punkt do witryny otrzymają adres IP z puli adresów klienta.

## <a name="before-beginning"></a>Przed rozpoczęciem

- Upewnij się, że masz subskrypcję usługi Azure. Jeśli nie masz jeszcze subskrypcji usługi Azure, można aktywować swoje [korzyści subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) lub Utwórz konto [bezpłatne konto](https://azure.microsoft.com/pricing/free-trial/).
    
## <a name="createvnet"></a>Część 1 — Tworzenie wirtualnych sieci

Jeśli tworzysz tej konfiguracji jako wykonywania może dotyczyć [Przykładowe wartości](#example).

[AZURE.INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-rm-portal-include.md)]  

### <a name="2-add-additional-address-space-and-subnets"></a>2. dodawanie dodatkowego adresu miejsca i podsieci

Można dodać przestrzeń dodatkowych adresów i podsieci usługi VNet po jego utworzeniu.

[AZURE.INCLUDE [vpn-gateway-additional-address-space](../../includes/vpn-gateway-additional-address-space-include.md)] 

### <a name="3-create-a-gateway-subnet"></a>3. Tworzenie podsieci bramy

Przed nawiązaniem połączenia wirtualnej sieci bramy, najpierw należy utworzyć podsieć Brama wirtualnej sieci, do którego chcesz się połączyć. Jeśli to możliwe najlepiej utworzyć podsieć bramy za pomocą CIDR /28 lub /27 w celu udostępniania za mało adresów IP dodatkowa konfiguracja przyszłe wymagania.

Zrzuty ekranu w tej sekcji znajdują się jako przykład odwołania. Pamiętaj za pomocą zakres adres GatewaySubnet, które odpowiadają wartości wymaganych dla danej konfiguracji.

#### <a name="to-create-a-gateway-subnet"></a>Aby utworzyć podsieć bramy

[AZURE.INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

### <a name="dns"></a>4. Określ serwer DNS (opcjonalnie)

[AZURE.INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]

## <a name="creategw"></a>Część 2 — Tworzenie bramy wirtualnej sieci

Połączenia punkt witryny wymagają następujące ustawienia:

- Typ bramy: VPN
- Typ VPN: oparte na trasę

### <a name="to-create-a-virtual-network-gateway"></a>Aby utworzyć bramę wirtualnej sieci

[AZURE.INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]

## <a name="generatecert"></a>Część 3 — generowanie certyfikatów

Certyfikaty są używane przez Azure do uwierzytelniania klientów sieci VPN sieci VPN punktu do witryny. Jak Base 64 zakodowany plik cer X.509 z certyfikatu głównego wygenerowane przez rozwiązanie enterprise certyfikatu lub certyfikat główny podpisem eksportowania danych certyfikatu publicznego (nie klucz prywatny). Następnie zaimportować dane certyfikatu publicznego z certyfikatu głównego Azure. Ponadto należy wygenerować certyfikat klienta z certyfikatu głównego dla klientów. Każdy klient, którą chce się połączyć z wirtualnej sieci przy użyciu połączenia P2S musi mieć zainstalowany certyfikat klienta wygenerowany z certyfikatu głównego.

### <a name="getcer"></a>1. plik cer dla certyfikatu głównego uzyskać

Jeśli korzystasz z rozwiązania enterprise, możesz użyć do istniejącego łańcuch certyfikatów. Jeśli nie używasz jednostka rozwiązanie urzędu certyfikacji, możesz utworzyć certyfikat z podpisem własnym głównego. Jednym ze sposobów tworzenia podpisem certyfikatu jest makecert.

- Jeśli korzystasz z systemu certyfikat enterprise, uzyskaj plik cer dla certyfikat główny, który ma być używany. 

- Jeśli nie używasz rozwiązanie enterprise certyfikatu, należy wygenerować certyfikat główny podpisem. Instrukcje dotyczące systemu Windows 10 można używać odwołań do [pracy z certyfikatów głównych podpisem konfiguracji punktu do witryny](vpn-gateway-certificates-point-to-site.md).

1. Aby uzyskać plik cer z certyfikatu, otwórz **certmgr.msc** i odszukaj certyfikat główny. Kliknij prawym przyciskiem myszy certyfikat główny podpisem, kliknij pozycję **Wszystkie zadania**i kliknij przycisk **Eksportuj**. Spowoduje to otwarcie **Kreatora eksportu certyfikatów**.

2. W kreatorze kliknij przycisk **Dalej**, zaznacz **nie Eksportuj klucz prywatny**, a następnie kliknij przycisk **Dalej**.

3. Na stronie **Format pliku eksportu** wybierz **Base-64 zakodowany X.509 (. CER).** Następnie kliknij przycisk **Dalej**. 

4. W **pliku do wyeksportowania**, **Przejdź** do lokalizacji, do której chcesz wyeksportować certyfikat. W polu **Nazwa pliku**nadaj nazwę pliku certyfikatu. Następnie kliknij przycisk **Dalej**.

5. Kliknij przycisk **Zakończ** , aby wyeksportować certyfikat.

### <a name="generateclientcert"></a>2. certyfikat klienta wygenerować

Istnieje możliwość wygenerowania certyfikatu unikatowe dla każdego klienta, który będzie łączyć lub za pomocą tego samego certyfikatu na wielu klientów. Zaletą do generowania unikatowych certyfikatów jest możliwość odwoływanie pojedynczego certyfikatu, w razie potrzeby. W przeciwnym razie jeśli wszyscy użytkownicy korzystają z tego samego certyfikatu klienta i okaże się trzeba odwołać certyfikat dla jednego klienta, będzie konieczne wygenerować i instalowanie nowych certyfikatów dla wszystkich klientów, które służą do uwierzytelniania za pomocą tego certyfikatu.

- Jeśli korzystasz z rozwiązania certyfikat enterprise wygenerować certyfikat klienta z popularnym formatem wartość Nazwa 'name@yourdomain.com', zamiast format "domeny\nazwa". 

- Jeśli używasz certyfikatu z podpisem własnym, zobacz [Praca z certyfikatów głównych podpisem konfiguracji punktu do witryny](vpn-gateway-certificates-point-to-site.md) , aby wygenerować certyfikat klienta.

### <a name="exportclientcert"></a>3. Wyeksportuj certyfikat klienta

Certyfikat klienta jest wymagany do uwierzytelniania. Po Generowanie certyfikat klienta, wyeksportuj je. Certyfikat klienta, które możesz wyeksportować zostanie później zainstalowana na każdym komputerze klienckim.

1. Aby wyeksportować certyfikat klienta, możesz użyć *certmgr.msc*. Kliknij prawym przyciskiem myszy certyfikat klienta, który chcesz eksportu, kliknij polecenie **Wszystkie zadania**, a następnie kliknij polecenie **Eksportuj**.
2. Wyeksportuj certyfikat klienta z kluczem prywatnym. Jest to plik *pfx* . Upewnij się zarejestrować lub Zapamiętaj hasło (klucz) można ustawić ten certyfikat.

## <a name="addresspool"></a>Część 4 — Dodawanie puli adresów klienta

1. Po utworzeniu bramy wirtualną sieć przejdź do sekcji **Ustawienia** karta bramy wirtualnej sieci. W sekcji **Ustawienia** kliknij pozycję Konfiguracja punktu witryny **** otworzyć karta **konfiguracji** .

    ![wskaż pozycję karta witryny] (./media/vpn-gateway-howto-point-to-site-resource-manager-portal/configuration.png "wskaż pozycję karta witryny")

2. **Pula adresów** jest puli adresów IP, z których klientów łączących się zostanie wyświetlony adres IP. Dodawanie puli adresów, a następnie kliknij przycisk **Zapisz**.

    ![Pula adresów klienta] (./media/vpn-gateway-howto-point-to-site-resource-manager-portal/addresspool.png "Pula adresów klienta")

## <a name="uploadfile"></a>Część 5 — przekazywać plik cer certyfikatów głównych

Po utworzeniu bramy, możesz przekazać plik cer dla zaufanego certyfikatu głównego Azure. Możesz przekazać pliki maksymalnie 20 certyfikatów głównych. Nie przekazuj klucz prywatny certyfikatu głównego Azure. Po przesłaniu plik cer Azure używany do uwierzytelniania klientów łączących się z siecią wirtualną.

1. Przejdź do konfiguracji punktu witryny **** karta. W sekcji **certyfikat główny** ta karta dodasz pliki cer.

    ![wskaż pozycję karta witryny] (./media/vpn-gateway-howto-point-to-site-resource-manager-portal/rootcert.png "wskaż pozycję karta witryny")

2. Upewnij się, wyeksportować certyfikat główny, ponieważ Base 64 zakodowany plik X.509 (cer). Należy wyeksportować w tym formacie, tak aby można było otworzyć certyfikat w edytorze tekstu.
3. Otwórz certyfikat w edytorze tekstów, takim jak Notatnik. Kopiowanie tylko następujących sekcji:

    ![dane certyfikatu] (./media/vpn-gateway-howto-point-to-site-resource-manager-portal/copycert.png "dane certyfikatu")

4. Wklej dane certyfikatów w sekcji **Danych publicznych certyfikat** portalu. Nazwa certyfikatu należy umieścić w obszarze **nazw** , a następnie kliknij przycisk **Zapisz**. Możesz dodać maksymalnie 20 zaufanych certyfikatów głównych.

    ![przekazywanie certyfikatu] (./media/vpn-gateway-howto-point-to-site-resource-manager-portal/uploadcert.png "przekazywanie certyfikatu")

## <a name="clientconfig"></a>Część 6 — Pobierz i zainstaluj pakiet konfiguracji klienta VPN

Nawiązywanie połączenia przy użyciu P2S Azure klientów musi mieć certyfikat klienta i zainstalowany pakiet konfiguracji klienta VPN. Pakiety konfiguracji klienta VPN są dostępne dla klientów z systemem Windows. 

Pakiet klienta VPN zawiera informacje, aby skonfigurować oprogramowanie klienckie sieci VPN, który jest wbudowany w systemie Windows. Konfiguracja jest specyficzne dla sieci VPN, którą chcesz nawiązać połączenie. Pakiet nie powoduje zainstalowania dodatkowego oprogramowania. Zobacz [Bramy sieci VPN — często zadawane pytania](vpn-gateway-vpn-faq.md#point-to-site-connections) , aby uzyskać więcej informacji.

1. Od konfiguracji punktu witryny **** karta, kliknij pozycję **Pobierz VPN klienta** do otwierania karta **klienta VPN pobieranie** .

    ![Pobierz klienta VPN] (./media/vpn-gateway-howto-point-to-site-resource-manager-portal/downloadclient.png "Pobierz klienta VPN")

2. Wybierz właściwy pakiet klienta, a następnie kliknij przycisk **Pobierz**. Dla klientów 64-bitowej wybierz **AMD64**. W przypadku klientów 32-bitową wybierz pozycję **x86**.

3. Zainstaluj pakiet na komputerze klienckim. Jeśli zostanie wyświetlony podręcznego SmartScreen, kliknij przycisk **więcej informacji**, następnie **Uruchom mimo to** , aby można było zainstalować pakiet.

4. Na komputerze klienckim przejdź do **Ustawień sieci** i kliknij pozycję **sieć VPN**. Zostanie wyświetlona na liście połączenia. Nazwa wirtualnej sieci, który będzie łączyć się i wygląd będzie widoczny podobnie jak w tym przykładzie: 

    ![Klient sieci VPN] (./media/vpn-gateway-howto-point-to-site-resource-manager-portal/vpn.png "Klient sieci VPN")

## <a name="installclientcert"></a>Część 7 - Instaluj certyfikat klienta

Każdy komputer kliencki musi mieć certyfikat klienta w celu uwierzytelnienia. Podczas instalowania certyfikat klienta, konieczne będzie hasło, które zostały utworzone podczas wyeksportowano certyfikat klienta.

1. Skopiuj plik pfx na komputer kliencki.
2. Kliknij dwukrotnie plik PFX, aby go zainstalować. Nie należy modyfikować lokalizację instalacji.

## <a name="connect"></a>Część 8 - nawiązywanie połączenia z platformy Azure

1. Aby nawiązać połączenie z VNet na komputerze klienckim, przejdź do połączeń sieci VPN i Znajdź połączenie VPN utworzone przez Ciebie. Nazywa się taką samą nazwę jak wirtualnej sieci. Kliknij przycisk **Połącz**. Podręczne może pojawić się komunikat odwołujące się do za pomocą certyfikatu. W takim przypadku kliknij przycisk **Kontynuuj** , aby użyć podwyższonym poziomem uprawnień. 

2. Na stronie Stan **połączenia** kliknij przycisk **Połącz** w celu rozpoczęcia połączenia. Jeśli zostanie wyświetlony ekran **Wybierz certyfikat** , sprawdź, czy certyfikat klienta przedstawiający jest ten, który ma być używany do łączenia. Jeśli nie jest dostępne, użyj strzałkę listy rozwijanej, aby wybrać poprawnego certyfikatu, a następnie kliknij **przycisk OK**.

    ![Klienta VPN 2] (./media/vpn-gateway-howto-point-to-site-rm-ps/clientconnect.png "Połączenie VPN klienta")

3. Teraz należy ustanowić połączenie.

    ![Klient sieci VPN 3] (./media/vpn-gateway-howto-point-to-site-rm-ps/connected.png "Połączenie VPN klienta 2")

## <a name="verify"></a>Część 9 — Sprawdź połączenie

1. Aby sprawdzić, czy połączenie VPN jest aktywne, otwórz wiersz polecenia z podwyższonym poziomem uprawnień i uruchom *polecenie ipconfig/all*.

2. Wyświetlanie wyników. Zauważ, że adres IP otrzymany jest jednym z adresów w puli adres klienta VPN punktu do witryny, określone w konfiguracji. Wyniki powinny być podobną do następującej:
    
        PPP adapter VNet1:
            Connection-specific DNS Suffix .:
            Description.....................: VNet1
            Physical Address................:
            DHCP Enabled....................: No
            Autoconfiguration Enabled.......: Yes
            IPv4 Address....................: 172.16.201.3(Preferred)
            Subnet Mask.....................: 255.255.255.255
            Default Gateway.................:
            NetBIOS over Tcpip..............: Enabled

## <a name="add"></a>Aby dodać lub usunąć zaufanych certyfikatów głównych

Zaufany certyfikat główny można usunąć z Azure. Po usunięciu zaufanego certyfikatu certyfikaty klientów, które zostały wygenerowane z certyfikatu głównego nie będzie mógł połączyć Azure za pośrednictwem punktu do witryny. Klientom na nawiązywanie połączeń, muszą zainstalować nowy certyfikat klienta, który jest generowany na podstawie certyfikatu, który jest zaufany w Azure.

Można zarządzać listą klienta odwołania certyfikatów w konfiguracji punktu witryny **** karta. Jest to karta, używany do [przekazywania zaufanego certyfikatu głównego](#uploadfile).

## <a name="revokeclient"></a>Aby zarządzać listą odwołania certyfikatów

Aby odebrać certyfikaty klienta. Listy odwołań certyfikatów umożliwia selektywne odmówić połączenia punkt do witryny, w oparciu o certyfikaty klienta. Jeśli usuniesz cer certyfikatów głównych z Azure odwołuje dostępu dla wszystkich certyfikatów klienta wygenerowane podpisane przez certyfikat został odwołany główny. Jeśli chcesz odwołania certyfikatu określonego klienta, nie katalogu głównego, możesz to zrobić. Dzięki temu inne certyfikatów, które zostały wygenerowane z certyfikatu głównego będą nadal jest ważna. 

Typowe sesji ćwiczeń jest zarządzanie dostępem poziomie zespołu lub organizacji, podczas korzystania z certyfikatów klienta odwołania do kontroli dostępu obowiązują w poszczególnych użytkowników za pomocą certyfikatu głównego.

Można zarządzać listą klienta odwołania certyfikatów w konfiguracji punktu witryny **** karta. Jest to karta, używany do [przekazywania zaufanego certyfikatu głównego](#uploadfile).


## <a name="next-steps"></a>Następne kroki

Możesz dodać maszyny wirtualnej z siecią wirtualną. Instrukcje, zobacz temat [Tworzenie maszyny wirtualnej](../virtual-machines/virtual-machines-windows-hero-tutorial.md) .