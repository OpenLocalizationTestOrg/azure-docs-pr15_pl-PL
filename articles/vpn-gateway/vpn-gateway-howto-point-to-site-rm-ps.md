<properties 
   pageTitle="Konfigurowanie połączenie VPN punktu do witryny Brama wirtualnej sieci przy użyciu modelu wdrożenia Menedżera zasobów | Microsoft Azure"
   description="Bezpieczne nawiązywanie połączenia z siecią wirtualną Azure, tworząc połączenie VPN punktu do witryny bramy."
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

# <a name="configure-a-point-to-site-connection-to-a-vnet-using-powershell"></a>Konfigurowanie połączenia punkt do witryny z VNet przy użyciu programu PowerShell

> [AZURE.SELECTOR]
- [Menedżer zasobów — Azure Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
- [Menedżer zasobów — programu PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
- [Klasyczny - Azure Portal](vpn-gateway-howto-point-to-site-classic-azure-portal.md)

Konfiguracja punktu do witryny (P2S) pozwala utworzyć bezpiecznego połączenia z pojedynczym komputerem klienckim do wirtualnej sieci. Połączenie P2S jest przydatne, gdy chcesz się połączyć z VNet z lokalizacji zdalnej, takich jak z domu lub konferencji, lub gdy masz tylko kilku klientów, którzy potrzebują nawiązania połączenia wirtualnej sieci. 

Połączenia punkt-witryny nie wymagają urządzenia VPN lub adres IP publicznej do pracy. Połączenie VPN jest określany przez uruchamiania połączenia na komputerze klienckim. Aby uzyskać więcej informacji o połączeniach punktu do witryny zobacz [Bramy sieci VPN — często zadawane pytania](vpn-gateway-vpn-faq.md#point-to-site-connections) i [Planowanie i projektowanie](vpn-gateway-plan-design.md). 

W tym artykule opisano tworzenie VNet za pomocą połączenia punkt do witryny w modelu wdrożenia Menedżera zasobów przy użyciu programu PowerShell.

### <a name="deployment-models-and-methods-for-p2s-connections"></a>Modeli wdrażania i metody P2S połączeń

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 

W poniższej tabeli przedstawiono dwa modele wdrożenia i metody wdrażania dostępne w przypadku konfiguracji P2S. Po udostępnieniu artykułu z kroków konfiguracji możemy łącze do niego bezpośrednio z tej tabeli.

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-table-point-to-site-include.md)] 

## <a name="basic-workflow"></a>Podstawowe przepływu pracy 

![Punkt do-— diagram witryny] (./media/vpn-gateway-howto-point-to-site-rm-ps/p2srm.png "punkt do witryny")

W tym scenariuszu utworzysz wirtualną sieć za pomocą połączenia punkt do witryny. Instrukcje również pomoże Ci generowanie certyfikatów, które są wymagane do tej konfiguracji. Połączenie P2S składa się z następujących elementów: VNet z bramą VPN, plik cer certyfikatów głównych (klucz publiczny) certyfikat klienta i konfiguracja sieci VPN na komputerze klienckim. 

Firma Microsoft korzysta z następujących wartości dla tej konfiguracji. Firma Microsoft ustawienie zmiennych w sekcji [1](#declare) tego artykułu. Można albo wykonaj czynności podane jako samouczek i użyj wartości z pól bez zmieniania ich lub zmienić ich, aby odzwierciedlała środowiska. 

### <a name="example"></a>Przykładowe wartości

- **Nazwa: VNet1**
- **Przestrzeń adresowa: 192.168.0.0/16** i **10.254.0.0/16**<br>Na przykład firma Microsoft korzysta więcej niż jedna spacja adres, aby przedstawić, że ta konfiguracja będzie współdziałać z wielu obszarach adresów. Jednak wielokrotnymi odstępami adres nie jest wymagane dla tej konfiguracji.
- **Nazwa podsieci: serwera sieci Web**
    - **Zakres adresów podsieci: 192.168.1.0/24**
- **Nazwa podsieci: wewnętrznej bazy danych**
    - **Zakres adresów podsieci: 10.254.1.0/24**
- **Nazwa podsieci: GatewaySubnet**<br>Nazwa podsieci *GatewaySubnet* jest wymagany dla bramy sieci VPN do pracy.
    - **Zakres adresów podsieci: 192.168.200.0/24** 
- **Pula adresów klienta VPN: 172.16.201.0/24**<br>Klienci sieci VPN, łączących się VNet za pomocą tego połączenia punkt do witryny otrzymają adres IP z puli adresów klienta VPN.
- **Subskrypcji:** Jeśli masz więcej niż jedną subskrypcję, upewnij się, że korzystasz z właściwą.
- **Grupa zasobów: TestRG**
- **Lokalizacja: Wschodniego w Stanach Zjednoczonych**
- **Serwera DNS: adres IP** serwera DNS, który ma być używany do rozpoznawania nazw.
- **Nazwa GW: Vnet1GW**
- **Nazwa publiczna IP: VNet1GWPIP**
- **VpnType: RouteBased**

## <a name="before-beginning"></a>Przed rozpoczęciem

- Upewnij się, że masz subskrypcję usługi Azure. Jeśli nie masz jeszcze subskrypcji usługi Azure, można aktywować swoje [korzyści subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) lub Utwórz konto [bezpłatne konto](https://azure.microsoft.com/pricing/free-trial/).
    
- Zainstaluj najnowszą wersję pakietu poleceń cmdlet programu PowerShell Menedżera zasobów Azure. Aby uzyskać więcej informacji o instalowaniu poleceń cmdlet programu PowerShell, zobacz [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md) . Podczas pracy z programem PowerShell dla tej konfiguracji, upewnij się, że działają jako administrator. 

## <a name="declare"></a>Część 1 — dziennika w i zestaw zmiennych

W tej sekcji Zaloguj się i zadeklarować wartości używane dla tej konfiguracji. Wartości są używane w przykładach skryptów. Zmień wartości tak, aby odzwierciedlała środowisku. Lub można użyć wartości i polecamy wykonanie kroków w charakterze ćwiczenia.

1. W konsoli programu PowerShell Zaloguj się do konta Azure. To polecenie cmdlet monituje o poświadczenia logowania dla Twojego konta Azure. Po zalogowaniu, go do pobrania ustawień konta, aby były dostępne dla Azure programu PowerShell.

        Login-AzureRmAccount 

2. Pobierz listę Azure subskrypcji.

        Get-AzureRmSubscription

3. Określ subskrypcję, do której chcesz użyć. 

        Select-AzureRmSubscription -SubscriptionName "Name of subscription"

4. Deklarowanie zmiennych, których chcesz użyć. Za pomocą Poniższy przykładowy podstawianie wartości do własnego, gdy jest to konieczne.

        $VNetName  = "VNet1"
        $FESubName = "FrontEnd"
        $BESubName = "Backend"
        $GWSubName = "GatewaySubnet"
        $VNetPrefix1 = "192.168.0.0/16"
        $VNetPrefix2 = "10.254.0.0/16"
        $FESubPrefix = "192.168.1.0/24"
        $BESubPrefix = "10.254.1.0/24"
        $GWSubPrefix = "192.168.200.0/26"
        $VPNClientAddressPool = "172.16.201.0/24"
        $RG = "TestRG"
        $Location = "East US"
        $DNS = "8.8.8.8"
        $GWName = "VNet1GW"
        $GWIPName = "VNet1GWPIP"
        $GWIPconfName = "gwipconf"
        
## <a name="ConfigureVNet"></a>Część 2 - Konfigurowanie VNet 

1. Tworzenie grupy zasobów.

        New-AzureRmResourceGroup -Name $RG -Location $Location

2. Tworzenie konfiguracji podsieci dla sieci wirtualnej, nadawanie im nazw *serwera sieci Web*, *wewnętrznej bazy danych*i *GatewaySubnet*. Tych prefiksów musi być częścią przestrzeni adresów VNet, które zostały zadeklarowane.

        $fesub = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName -AddressPrefix $FESubPrefix
        $besub = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName -AddressPrefix $BESubPrefix
        $gwsub = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName -AddressPrefix $GWSubPrefix

3. Tworzenie wirtualnych sieci. Serwer DNS określony należy serwera DNS, który można rozpoznawania nazw zasobów, które jest nawiązywane. W tym przykładzie użyliśmy publiczny adres IP. Pamiętaj użyć własnego wartości.
    
        New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG -Location $Location -AddressPrefix $VNetPrefix1,$VNetPrefix2 -Subnet $fesub, $besub, $gwsub -DnsServer $DNS

4. Określ zmienne wirtualnej sieci utworzony.

        $vnet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG
        $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet

5. Żądanie przypisywany dynamicznie publiczny adres IP. Ten adres IP jest konieczne bramy działał poprawnie. Później połączyć bramę do konfiguracji IP bramy.
        
        $pip = New-AzureRmPublicIpAddress -Name $GWIPName -ResourceGroupName $RG -Location $Location -AllocationMethod Dynamic
        $ipconf = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName -Subnet $subnet -PublicIpAddress $pip

## <a name="Certificates"></a>Część 3 — certyfikaty

Certyfikaty są używane przez Azure do uwierzytelniania klientów sieci VPN sieci VPN punktu do witryny. Jak Base 64 zakodowany plik cer X.509 z certyfikatu głównego wygenerowane przez rozwiązanie enterprise certyfikatu lub certyfikat główny podpisem eksportowania danych certyfikatu publicznego (nie klucz prywatny). Następnie zaimportować dane certyfikatu publicznego z certyfikatu głównego Azure. Ponadto należy wygenerować certyfikat klienta z certyfikatu głównego dla klientów. Każdy klient, którą chce się połączyć z wirtualnej sieci przy użyciu połączenia P2S musi mieć zainstalowany certyfikat klienta wygenerowany z certyfikatu głównego.

### <a name="cer"></a>1. plik cer dla certyfikatu głównego uzyskać

Należy uzyskać dane certyfikatu publicznego certyfikat główny, który ma być używany.

- Jeśli korzystasz z systemu certyfikat enterprise, uzyskaj plik cer dla certyfikat główny, który ma być używany. 

- Jeśli nie używasz rozwiązanie enterprise certyfikatu, należy wygenerować certyfikat główny podpisem. Instrukcje dotyczące systemu Windows 10 można używać odwołań do [pracy z certyfikatów głównych podpisem konfiguracji punktu do witryny](vpn-gateway-certificates-point-to-site.md).


1. Aby uzyskać plik cer z certyfikatu, otwórz **certmgr.msc** i odszukaj certyfikat główny. Kliknij prawym przyciskiem myszy certyfikat główny podpisem, kliknij pozycję **Wszystkie zadania**i kliknij przycisk **Eksportuj**. Spowoduje to otwarcie **Kreatora eksportu certyfikatów**.

2. W kreatorze kliknij przycisk **Dalej**, zaznacz **nie Eksportuj klucz prywatny**, a następnie kliknij przycisk **Dalej**.

3. Na stronie **Format pliku eksportu** wybierz **Base-64 zakodowany X.509 (. CER).** Następnie kliknij przycisk **Dalej**. 

4. W **pliku do wyeksportowania**, **Przejdź** do lokalizacji, do której chcesz wyeksportować certyfikat. W polu **Nazwa pliku**nadaj nazwę pliku certyfikatu. Następnie kliknij przycisk **Dalej**.

5. Kliknij przycisk **Zakończ** , aby wyeksportować certyfikat.

### <a name="generate"></a>2. certyfikat klienta wygenerować

Następnie generować certyfikaty klienta. Istnieje możliwość wygenerowania certyfikatu unikatowe dla każdego klienta, który będzie łączyć lub za pomocą tego samego certyfikatu na wielu klientów. Zaletą do generowania unikatowych certyfikatów jest możliwość odwoływanie pojedynczego certyfikatu, w razie potrzeby. W przeciwnym razie jeśli wszyscy użytkownicy korzystają z tego samego certyfikatu klienta i okaże się trzeba odwołać certyfikat dla jednego klienta, będzie konieczne wygenerować i instalowanie nowych certyfikatów dla wszystkich klientów, które służą do uwierzytelniania za pomocą certyfikatu. Certyfikaty klienta są zainstalowane na każdym komputerze klienckim w dalszej części tego ćwiczenia.

- Jeśli korzystasz z rozwiązania certyfikat enterprise wygenerować certyfikat klienta z popularnym formatem wartość Nazwa 'name@yourdomain.com', zamiast formatowanie NetBIOS "Domena". 

- Jeśli używasz certyfikatu z podpisem własnym, zobacz [Praca z certyfikatów głównych podpisem konfiguracji punktu do witryny](vpn-gateway-certificates-point-to-site.md) , aby wygenerować certyfikat klienta.

### <a name="exportclientcert"></a>3. Wyeksportuj certyfikat klienta

Certyfikat klienta jest wymagany do uwierzytelniania. Po Generowanie certyfikat klienta, wyeksportuj je. Certyfikat klienta, które możesz wyeksportować zostanie później zainstalowana na każdym komputerze klienckim.

1. Aby wyeksportować certyfikat klienta, możesz użyć *certmgr.msc*. Kliknij prawym przyciskiem myszy certyfikat klienta, który chcesz eksportu, kliknij polecenie **Wszystkie zadania**, a następnie kliknij polecenie **Eksportuj**.
2. Wyeksportuj certyfikat klienta z kluczem prywatnym. Jest to plik *pfx* . Upewnij się zarejestrować lub Zapamiętaj hasło (klucz) można ustawić ten certyfikat.

### <a name="upload"></a>4. przekazać plik cer certyfikatów głównych

Deklarowanie zmiennej nazwę certyfikatu, zamieniając wartość własnych:

        $P2SRootCertName = "Mycertificatename.cer"

Dodaj dane certyfikatu publicznego certyfikatu głównego Azure. Możesz przekazać pliki maksymalnie 20 certyfikatów głównych. Nie przekazuj klucz prywatny certyfikatu głównego Azure. Po przesłaniu plik cer Azure używany do uwierzytelniania klientów łączących się z siecią wirtualną. 

Zastąpić własnym ścieżka do pliku, a następnie uruchom polecenia cmdlet.
    
        $filePathForCert = "C:\cert\Mycertificatename.cer"
        $cert = new-object System.Security.Cryptography.X509Certificates.X509Certificate2($filePathForCert)
        $CertBase64 = [system.convert]::ToBase64String($cert.RawData)
        $p2srootcert = New-AzureRmVpnClientRootCertificate -Name $P2SRootCertName -PublicCertData $CertBase64

## <a name="creategateway"></a>Część 4 — Tworzenie bramy sieci VPN

Konfigurowanie i Tworzenie bramy wirtualną sieć dla swojego VNet. *-GatewayType* musi być **Vpn** i *-VpnType* musi być **RouteBased**. To może potrwać do 45 minut.

        New-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG `
        -Location $Location -IpConfigurations $ipconf -GatewayType Vpn `
        -VpnType RouteBased -EnableBgp $false -GatewaySku Standard `
        -VpnClientAddressPool $VPNClientAddressPool -VpnClientRootCertificates $p2srootcert

## <a name="clientconfig"></a>Część 5 — Pobierz pakiet konfiguracji klienta VPN

Klientów łączących się Azure za pomocą P2S musi mieć jednocześnie certyfikat klienta i zainstalowany pakiet konfiguracji klienta VPN. Pakiety konfiguracji klienta VPN są dostępne dla klientów z systemem Windows. Pakiet klienta VPN zawiera informacji w celu skonfigurowania oprogramowania klienta VPN, wbudowana w systemie Windows, które są specyficzne dla sieci VPN, którą chcesz nawiązać połączenie. Pakiet nie powoduje zainstalowania dodatkowego oprogramowania. Zobacz [Bramy sieci VPN — często zadawane pytania](vpn-gateway-vpn-faq.md#point-to-site-connections) , aby uzyskać więcej informacji.

1. Po utworzeniu bramy, możesz pobrać pakiet konfiguracji klienta. W tym przykładzie do pobrania pakietu dla klientów 64-bitowej. Jeśli chcesz pobrać 32-bitowy klient, Zamień "Amd64" na "x86". Aby pobrać klienta VPN, należy Azure portal.

        Get-AzureRmVpnClientPackage -ResourceGroupName $RG `
        -VirtualNetworkGatewayName $GWName -ProcessorArchitecture Amd64

2. Polecenia cmdlet programu PowerShell zwraca adres URL łącza. Oto przykład jakie zwracany adres URL wygląda:

        "https://mdsbrketwprodsn1prod.blob.core.windows.net/cmakexe/4a431aa7-b5c2-45d9-97a0-859940069d3f/amd64/4a431aa7-b5c2-45d9-97a0-859940069d3f.exe?sv=2014-02-14&sr=b&sig=jSNCNQ9aUKkCiEokdo%2BqvfjAfyhSXGnRG0vYAv4efg0%3D&st=2016-01-08T07%3A10%3A08Z&se=2016-01-08T08%3A10%3A08Z&sp=r&fileExtension=.exe"

3. Skopiuj i Wklej łącze, które są zwracane do przeglądarki sieci web, aby pobrać pakiet. Następnie należy zainstalować pakiet na komputerze klienckim. Jeśli zostanie wyświetlony podręcznego SmartScreen, kliknij przycisk **więcej informacji**, następnie **Uruchom mimo to** , aby można było zainstalować pakiet.

4. Na komputerze klienckim przejdź do **Ustawień sieci** i kliknij pozycję **sieć VPN**. Zostanie wyświetlona na liście połączenia. Nazwa wirtualnej sieci, który będzie łączyć się i wygląd będzie widoczny podobnie jak w tym przykładzie: 

    ![Klient sieci VPN] (./media/vpn-gateway-howto-point-to-site-rm-ps/vpn.png "Klient sieci VPN")


## <a name="clientcertificate"></a>Część 6 - Zainstaluj certyfikat klienta

Każdy komputer kliencki musi mieć certyfikat klienta w celu uwierzytelnienia. Podczas instalowania certyfikat klienta, konieczne będzie hasło, które zostały utworzone podczas wyeksportowano certyfikat klienta.

1. Skopiuj plik pfx na komputer kliencki.
2. Kliknij dwukrotnie plik PFX, aby go zainstalować. Nie należy modyfikować lokalizację instalacji.

## <a name="connect"></a>Część 7 - nawiązywanie połączenia z platformy Azure

1. Aby nawiązać połączenie z VNet na komputerze klienckim, przejdź do połączeń sieci VPN i Znajdź połączenie VPN utworzone przez Ciebie. Nazywa się taką samą nazwę jak wirtualnej sieci. Kliknij przycisk **Połącz**. Podręczne może pojawić się komunikat odwołujące się do za pomocą certyfikatu. W takim przypadku kliknij przycisk **Kontynuuj** , aby użyć podwyższonym poziomem uprawnień. 

2. Na stronie Stan **połączenia** kliknij przycisk **Połącz** w celu rozpoczęcia połączenia. Jeśli zostanie wyświetlony ekran **Wybierz certyfikat** , sprawdź, czy certyfikat klienta przedstawiający jest ten, który ma być używany do łączenia. Jeśli nie jest dostępne, użyj strzałkę listy rozwijanej, aby wybrać poprawnego certyfikatu, a następnie kliknij **przycisk OK**.

    ![Połączenie VPN klienta] (./media/vpn-gateway-howto-point-to-site-rm-ps/clientconnect.png "Połączenie VPN klienta")

3. Teraz należy ustanowić połączenie.

    ![Nawiązać połączenia] (./media/vpn-gateway-howto-point-to-site-rm-ps/connected.png "Nawiązać połączenia")

## <a name="verify"></a>Część 8 - Sprawdź połączenie

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

## <a name="addremovecert"></a>Aby dodać lub usunąć zaufanego certyfikatu głównego

Certyfikaty są używane do uwierzytelniania klientów sieci VPN sieci VPN punktu do witryny. Poniższe kroki przeprowadził Cię przez dodawanie i usuwanie certyfikatów głównych. Po dodaniu pliku zakodowany Base64 X.509 (cer) Azure sprawi, że Azure zaufania dla certyfikatu głównego, która reprezentuje plik. 

Można dodać lub usunąć zaufanych certyfikatów głównych przy użyciu programu PowerShell lub w portalu Azure. Jeśli chcesz to zrobić za pomocą portalu Azure, przejdź do swojej **bramy wirtualną sieć > Ustawienia > Konfiguracja punktu do witryny > główne certyfikaty**. Poniższe kroki szczegółową tych zadań przy użyciu programu PowerShell. 

### <a name="add-a-trusted-root-certificate"></a>Dodawanie zaufanego certyfikatu głównego

Możesz dodać maksymalnie 20 pliki cer certyfikatów Zaufane główne Azure. Wykonaj poniższe czynności, aby dodać certyfikat główny.

1. Tworzenie i przygotować nowy certyfikat główny, który spowoduje dodanie Azure. Eksportowanie klucz publiczny, jak Base 64 zakodowany X.509 (. CER) i otwórz go w edytorze tekstów. Następnie skopiuj sekcji, jak pokazano poniżej. 
 
    Skopiuj wartości, jak pokazano w poniższym przykładzie:

    ![certyfikat] (./media/vpn-gateway-howto-point-to-site-rm-ps/copycert.png "certyfikat")
    
2. Określ nazwę certyfikatu i informacje o kluczu jako zmienna. Zamień dane na własny, jak pokazano w poniższym przykładzie:

        $P2SRootCertName2 = "ARMP2SRootCert2.cer"
        $MyP2SCertPubKeyBase64_2 = "MIIC/zCCAeugAwIBAgIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAMBgxFjAUBgNVBAMTDU15UDJTUm9vdENlcnQwHhcNMTUxMjE5MDI1MTIxWhcNMzkxMjMxMjM1OTU5WjAYMRYwFAYDVQQDEw1NeVAyU1Jvb3RDZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAyjIXoWy8xE/GF1OSIvUaA0bxBjZ1PJfcXkMWsHPzvhWc2esOKrVQtgFgDz4ggAnOUFEkFaszjiHdnXv3mjzE2SpmAVIZPf2/yPWqkoHwkmrp6BpOvNVOpKxaGPOuK8+dql1xcL0eCkt69g4lxy0FGRFkBcSIgVTViS9wjuuS7LPo5+OXgyFkAY3pSDiMzQCkRGNFgw5WGMHRDAiruDQF1ciLNojAQCsDdLnI3pDYsvRW73HZEhmOqRRnJQe6VekvBYKLvnKaxUTKhFIYwuymHBB96nMFdRUKCZIiWRIy8Hc8+sQEsAML2EItAjQv4+fqgYiFdSWqnQCPf/7IZbotgQIDAQABo00wSzBJBgNVHQEEQjBAgBAkuVrWvFsCJAdK5pb/eoCNoRowGDEWMBQGA1UEAxMNTXlQMlNSb290Q2VydIIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAA4IBAQA223veAZEIar9N12ubNH2+HwZASNzDVNqspkPKD97TXfKHlPlIcS43TaYkTz38eVrwI6E0yDk4jAuPaKnPuPYFRj9w540SvY6PdOUwDoEqpIcAVp+b4VYwxPL6oyEQ8wnOYuoAK1hhh20lCbo8h9mMy9ofU+RP6HJ7lTqupLfXdID/XevI8tW6Dm+C/wCeV3EmIlO9KUoblD/e24zlo3YzOtbyXwTIh34T0fO/zQvUuBqZMcIPfM1cDvqcqiEFLWvWKoAnxbzckye2uk1gHO52d8AVL3mGiX8wBJkjc/pMdxrEvvCzJkltBmqxTM6XjDJALuVh16qFlqgTWCIcb7ju"


3. Dodawanie nowego certyfikatu głównego. W danej chwili można dodawać tylko jeden certyfikat.

        Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName2 -VirtualNetworkGatewayname "VNet1GW" -ResourceGroupName "TestRG" -PublicCertData $MyP2SCertPubKeyBase64_2


4. Można sprawdzić, czy nowego certyfikatu zostały poprawnie dodane przy użyciu następującego polecenia cmdlet.

        Get-AzureRmVpnClientRootCertificate -ResourceGroupName "TestRG" `
        -VirtualNetworkGatewayName "VNet1GW"

### <a name="to-remove-a-trusted-root-certificate"></a>Aby usunąć zaufany certyfikat główny

Zaufany certyfikat główny można usunąć z Azure. Po usunięciu zaufanego certyfikatu certyfikaty klientów, które zostały wygenerowane z certyfikatu głównego nie będzie mógł połączyć Azure za pośrednictwem punktu do witryny. Klientom na nawiązywanie połączeń, muszą zainstalować nowy certyfikat klienta, który jest generowany na podstawie certyfikatu, który jest zaufany w Azure.

1. Aby usunąć zaufany certyfikat główny, należy zmodyfikować poniższy przykładowy:

    Deklarowanie zmiennych.

        $P2SRootCertName2 = "ARMP2SRootCert2.cer"
        $MyP2SCertPubKeyBase64_2 = "MIIC/zCCAeugAwIBAgIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAMBgxFjAUBgNVBAMTDU15UDJTUm9vdENlcnQwHhcNMTUxMjE5MDI1MTIxWhcNMzkxMjMxMjM1OTU5WjAYMRYwFAYDVQQDEw1NeVAyU1Jvb3RDZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAyjIXoWy8xE/GF1OSIvUaA0bxBjZ1PJfcXkMWsHPzvhWc2esOKrVQtgFgDz4ggAnOUFEkFaszjiHdnXv3mjzE2SpmAVIZPf2/yPWqkoHwkmrp6BpOvNVOpKxaGPOuK8+dql1xcL0eCkt69g4lxy0FGRFkBcSIgVTViS9wjuuS7LPo5+OXgyFkAY3pSDiMzQCkRGNFgw5WGMHRDAiruDQF1ciLNojAQCsDdLnI3pDYsvRW73HZEhmOqRRnJQe6VekvBYKLvnKaxUTKhFIYwuymHBB96nMFdRUKCZIiWRIy8Hc8+sQEsAML2EItAjQv4+fqgYiFdSWqnQCPf/7IZbotgQIDAQABo00wSzBJBgNVHQEEQjBAgBAkuVrWvFsCJAdK5pb/eoCNoRowGDEWMBQGA1UEAxMNTXlQMlNSb290Q2VydIIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAA4IBAQA223veAZEIar9N12ubNH2+HwZASNzDVNqspkPKD97TXfKHlPlIcS43TaYkTz38eVrwI6E0yDk4jAuPaKnPuPYFRj9w540SvY6PdOUwDoEqpIcAVp+b4VYwxPL6oyEQ8wnOYuoAK1hhh20lCbo8h9mMy9ofU+RP6HJ7lTqupLfXdID/XevI8tW6Dm+C/wCeV3EmIlO9KUoblD/e24zlo3YzOtbyXwTIh34T0fO/zQvUuBqZMcIPfM1cDvqcqiEFLWvWKoAnxbzckye2uk1gHO52d8AVL3mGiX8wBJkjc/pMdxrEvvCzJkltBmqxTM6XjDJALuVh16qFlqgTWCIcb7ju"

2. Usuwanie certyfikatu.

        Remove-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName2 -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -PublicCertData $MyP2SCertPubKeyBase64_2
 
3. Użyj następującego polecenia cmdlet, aby sprawdzić, certyfikat został usunięty pomyślnie. 

        Get-AzureRmVpnClientRootCertificate -ResourceGroupName "TestRG" `
        -VirtualNetworkGatewayName "VNet1GW"

## <a name="revoke"></a>Aby zarządzać listą odwołania certyfikatów

Aby odebrać certyfikaty klienta. Listy odwołań certyfikatów umożliwia selektywne odmówić połączenia punkt do witryny, w oparciu o certyfikaty klienta. Jeśli usuniesz cer certyfikatów głównych z Azure odwołuje dostępu dla wszystkich certyfikatów klienta wygenerowane podpisane przez certyfikat został odwołany główny. Jeśli chcesz odwołania certyfikatu określonego klienta, nie katalogu głównego, możesz to zrobić. Dzięki temu inne certyfikatów, które zostały wygenerowane z certyfikatu głównego będą nadal jest ważna.

Typowe sesji ćwiczeń jest zarządzanie dostępem poziomie zespołu lub organizacji, podczas korzystania z certyfikatów klienta odwołania do kontroli dostępu obowiązują w poszczególnych użytkowników za pomocą certyfikatu głównego.

### <a name="revoke-a-client-certificate"></a>Odwołania certyfikatu klienta

1. Uzyskaj odcisku palca certyfikatu klienta, aby odwołać.

        $RevokedClientCert1 = "ClientCert1"
        $RevokedThumbprint1 = "‎ef2af033d0686820f5a3c74804d167b88b69982f"

2. Dodawanie odcisku palca do listy odwołania odcisku palca.

        Add-AzureRmVpnClientRevokedCertificate -VpnClientRevokedCertificateName $RevokedClientCert1 `
        -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -Thumbprint $RevokedThumbprint1

3. Upewnij się, że odcisku palca został dodany do listy odwołań certyfikatów. Należy dodać jeden odcisku palca naraz.

        Get-AzureRmVpnClientRevokedCertificate -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG

### <a name="reinstate-a-client-certificate"></a>Przywrócić certyfikat klienta

Certyfikat klienta można przywrócić, usuwając odcisku palca na liście odwołania certyfikatów.

1.  Usuwanie odcisku palca na liście odcisku palca certyfikat został odwołany klienta.

        Remove-AzureRmVpnClientRevokedCertificate -VpnClientRevokedCertificateName $RevokedClientCert1 `
        -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -Thumbprint $RevokedThumbprint1

2. Sprawdzanie, jeśli odcisku palca zostanie usunięty z listy odwołania.

        Get-AzureRmVpnClientRevokedCertificate -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG

## <a name="next-steps"></a>Następne kroki

Możesz dodać maszyny wirtualnej z siecią wirtualną. Instrukcje, zobacz temat [Tworzenie maszyny wirtualnej](../virtual-machines/virtual-machines-windows-hero-tutorial.md) .