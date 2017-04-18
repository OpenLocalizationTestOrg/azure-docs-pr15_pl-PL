<properties
   pageTitle="Konfigurowanie połączenia VNet do VNet modelu Klasyczny wdrożenia | Microsoft Azure"
   description="Jak połączyć Azure wirtualnych sieci współpraca przy użyciu programu PowerShell i Azure portalu klasyczny."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/31/2016"
   ms.author="cherylmc"/>


# <a name="configure-a-vnet-to-vnet-connection-for-the-classic-deployment-model"></a>Konfigurowanie połączenia VNet do VNet modelu Klasyczny wdrażania

> [AZURE.SELECTOR]
- [Menedżer zasobów — Azure Portal](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
- [Menedżer zasobów — programu PowerShell](vpn-gateway-vnet-vnet-rm-ps.md)
- [Klasyczny — klasyczny portalu](virtual-networks-configure-vnet-to-vnet-connection.md)



W tym artykule opisano kroki tworzenia i połączenia wirtualnej sieci współpraca przy użyciu modelu Klasyczny wdrożenia (usługa Zarządzanie). W poniższej procedurze użyto portalu klasyczny Azure do tworzenia VNets i bram i programu PowerShell, aby skonfigurować połączenie VNet do VNet. Nie można skonfigurować połączenie w portalu.

![VNet do diagramu łączności VNet](./media/virtual-networks-configure-vnet-to-vnet-connection/v2vclassic.png)


### <a name="deployment-models-and-methods-for-vnet-to-vnet-connections"></a>Modeli wdrażania i metody VNet do VNet połączeń

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)]

W poniższej tabeli przedstawiono modeli jest obecnie dostępna wdrażania i metody VNet do VNet konfiguracji. Po udostępnieniu artykułu z kroków konfiguracji możemy łącze do niego bezpośrednio z tej tabeli.

[AZURE.INCLUDE [vpn-gateway-table-vnet-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)]

## <a name="about-vnet-to-vnet-connections"></a>Informacje o połączeniach VNet do VNet

Wirtualnej sieci nawiązywanie połączenia z innym wirtualną sieć (VNet-VNet) jest podobna do połączenia wirtualnej sieci lokalnej lokalizacja witryny. Oba typy łączności za pomocą bramy sieci VPN o podanie kanału bezpiecznego przy użyciu IPsec i IKE. 

W różnych subskrypcjach i różnych regionów może być VNets, w którym można się połączyć. Aby VNet komunikacji z konfiguracji wielu witryn można połączyć VNet. Pozwala ustanowić topologii sieci, łączące między lokalnej łączność z łącznością między wirtualnej sieci.


### <a name="why-connect-virtual-networks"></a>Dlaczego połączenia wirtualnej sieci?

Być może zechcesz połączenia wirtualnej sieci z następujących powodów:

- **Krzyżowe region geo nadmiarowości i geo obecności**
    - Możesz skonfigurować własne geo replikacji lub synchronizacji z bezpieczne połączenia bez przekroczony punkty końcowe w Internecie.
    - Przy użyciu usługi równoważenia obciążenia Azure i Microsoft lub innych firm klastrów technologii możesz skonfigurować wysokiej dostępności obciążenie pracą z nadmiarowości geo u wielu regionów Azure. Przykład ważne jest konfigurowanie SQL zawsze przy użyciu grup dostępność rozłożenie wielu regionów Azure.

- **Regionalnych zastosowań wielu granica silnych izolacji**
    - W tym samym regionie możesz skonfigurować aplikacje wielu z wielu VNets połączone razem z silnych izolacji i bezpieczna komunikacja między warstwy.

- **Krzyżowe subskrypcji, komunikacji między organizacji platformy Azure**
    - Jeśli masz wiele subskrypcji Azure, można połączyć obciążenie pracą z różnych subskrypcjach razem bezpieczne między wirtualnych sieci.
    - Dla przedsiębiorstw lub usługodawców możesz włączyć komunikacji między organizacji bezpiecznego technologii sieci VPN w Azure.

### <a name="vnet-to-vnet-faq-for-classic-vnets"></a>VNet do VNet często zadawane pytania dotyczące VNets klasyczny

- Wirtualnych sieci może być w takich samych lub różnych subskrypcjach.

- Wirtualnych sieci może być w regionach Azure takich samych lub różnych (lokalizacja).

- Usługi w chmurze lub punkt końcowy równoważenia obciążenia nie może obejmować wielu wirtualnych sieci, nawet jeśli są połączone.

- Łączenie wielu sieci wirtualnych nie wymaga wszystkich urządzeń sieci VPN.

- VNet-VNet obsługuje łączących Azure wirtualnych sieci. Nie obsługuje łączenia maszyn wirtualnych lub usług, które nie są wdrażane w wirtualnej sieci w chmurze.

- VNet do VNet wymaga dynamiczne routingu bramy. Azure statycznego routingu bram nie są obsługiwane.

- Połączenia wirtualnej sieci można używać jednocześnie z wielu witryn sieci VPN. Istnieje maksymalnie 10 tuneli VPN bramy sieci VPN wirtualną sieć nawiązywanie połączenia z innych wirtualnych sieci albo lokalnej witryny.

- Nie musisz zachodzi obszary adresów wirtualnych sieci i lokalnych witryn sieci lokalnej. Nakładające się spacje adres spowoduje, że tworzenie wirtualnych sieci lub przekazywania plików konfiguracji netcfg kończy się niepowodzeniem.

- Zbędne tunele między dwoma wirtualnych sieci nie są obsługiwane.

- Wszystkie tuneli VPN dla VNet, łącznie z P2S sieci VPN, udostępnianie przepustowości dla bramy sieci VPN i tym samym przestojów bramy sieci VPN SLA platformy Azure.

- Ruch VNet do VNet podróży w Azure szkieletowy.


## <a name="step1"></a>Krok 1 — Plan usługi zakresy adresów IP

Należy zdecydować, zakresy, które będą używane do konfigurowania usługi wirtualnych sieci. W tej konfiguracji należy się upewnić, brak zakresy VNet zachodzi ze sobą lub z dowolną sieci lokalnej, które łączą się.

Poniższa tabela zawiera przykład sposobu definiowania usługi VNets. Używanie zakresów tylko wskazówką. Zanotuj zakresów usługi wirtualnych sieci. Te informacje są potrzebne do dalszych krokach.

**Przykład ustawień**

|Wirtualna sieć  |Przestrzeń adresów               |Region      |Łączy do sieci lokalnej witryny|
|:----------------|:---------------------------|:-----------|:-----------------------------|
|VNet1            |VNet1 (10.1.0.0/16)         |Stany Zjednoczone zachód     |VNet2Local (10.2.0.0/16)      |
|VNet2            |VNet2 (10.2.0.0/16)         |Wschód Japonii  |VNet1Local (10.1.0.0/16)      |
  
## <a name="step-2---create-vnet1"></a>Krok 2 — Tworzenie VNet1

W tym kroku tworzymy VNet1. Podczas korzystania z tych przykładów, należy zastąpić własne wartości. Jeśli do VNet już istnieje, nie musisz wykonywać tej czynności. Jednak trzeba Sprawdź, czy zakresy adresów IP nie nakładały się przy użyciu zakresów dla swojego drugiego VNet lub dowolne inne VNets, do którego chcesz się połączyć.

1. Zaloguj się do [portalu klasyczny Azure](https://manage.windowsazure.com). W tym artykule używamy portalu klasyczny ponieważ niektóre ustawienia konfiguracji są jeszcze dostępne w portalu Azure.

2. W lewym dolnym rogu ekranu kliknij przycisk **Nowy** > **Usług sieciowych** > **Wirtualnej sieci** > **Utworzyć niestandardowe** , aby uruchomić Kreatora konfiguracji. Podczas przeglądania kreatora Dodawanie określonych wartości do każdej strony.

### <a name="virtual-network-details"></a>Szczegóły wirtualnej sieci

Na stronie Szczegóły wirtualna sieć wprowadź następujące informacje:

  ![Szczegóły wirtualnej sieci](./media/virtual-networks-configure-vnet-to-vnet-connection/IC736055.png)

  - **Nazwa** - nazwa wirtualnej sieci. Na przykład VNet1.
  - **Lokalizacja** — po utworzeniu wirtualnej sieci, możesz skojarzyć go z lokalizacji Azure (region). Na przykład z maszyny wirtualne wdrożone w wirtualnej sieci się znajdować się w USA Zachód, zaznacz tę lokalizację. Nie można zmienić lokalizację skojarzone z siecią wirtualną po jego utworzeniu.

### <a name="dns-servers-and-vpn-connectivity"></a>Serwery DNS i połączenia VPN

Na stronie serwery DNS i połączenia VPN wprowadź następujące informacje, a następnie kliknij strzałkę następnego w prawej dolnej.

  ![Serwery DNS i połączenia VPN](./media/virtual-networks-configure-vnet-to-vnet-connection/IC736056.jpg)  

- **Serwery DNS** - wprowadź nazwę serwera DNS i adres IP lub wybierz wcześniej zarejestrowanych serwera DNS z listy rozwijanej. To ustawienie nie tworzy serwera DNS. Umożliwia określenie serwerów DNS, które ma być używany do rozpoznawania nazw dla tej wirtualnej sieci. Jeśli chcesz mieć rozpoznawanie nazw między sieci wirtualnych, należy skonfigurować serwer DNS, a nie za pomocą rozpoznawania nazw, że pochodzi od Azure.
- Nie zaznaczaj pola wyboru P2S lub S2S łączności. Kliknij strzałkę w prawym dolnym, aby przejść do następnego ekranu.

### <a name="virtual-network-address-spaces"></a>Obszar adresów wirtualnych sieci

Na stronie wirtualna sieć adres spacje określić zakres adresów, który ma być używany dla wirtualnych sieci. Są to dynamiczne adresy IP (SPADKU), które są przypisywane do maszyny wirtualne i inne wystąpienia roli, które wdrażanie z tą siecią wirtualną. 

Jeśli tworzysz VNet, które też będą mieć połączenie z siecią w lokalnej jest szczególnie ważne zaznaczyć zakres, aby nie nakładały się z dowolnego z zakresów, które są używane w sieci lokalnej. W tym przypadku należy będą dopasowane do administratora sieci. Administrator sieci może być konieczne dotycząca szczególnych się zakres adresów IP z obszaru adres sieci lokalnej do użycia dla swojego VNet.

  ![Wirtualna strony obszar adresów sieciowych](./media/virtual-networks-configure-vnet-to-vnet-connection/IC736057.jpg)

  - **Przestrzeń adresów** - tym uruchamianie IP i liczba adresów. Upewnij się, że wybrane obszary adresów nie zachodzi z każdego z pomieszczeń adres znajdujących się w tej samej sieci lokalnej. W tym przykładzie używamy 10.1.0.0/16 dla VNet1.
  - **Dodaj podsieci** - tym uruchamianie IP i liczba adresów. Dodatkowe podsieci nie są wymagane, ale warto utworzyć osobną podsieć dla maszyny wirtualne zawierające SPADKU statyczne. Możesz też w podsieci, która różni się od innych wystąpień roli jest pośrednictwem usługi SMS.
 
Aby utworzyć rozpocznie się **kliknij znacznik wyboru** w prawym dolnym rogu strony i wirtualna sieć. Po zakończeniu, pojawi się, że w obszarze stanu "Utworzone" wyświetlane na stronie sieci.

## <a name="step-3---create-vnet2"></a>Krok 3 — Tworzenie VNet2

Następnie powtórz poprzednie kroki do utworzenia innego VNet. W krokach później łączą dwóch VNets. Może dotyczyć [przykładowych ustawień](#step1) w kroku 1. Jeśli do VNet już istnieje, nie musisz wykonywać tej czynności. Jednak należy sprawdzić, czy zakresy adresów IP nie nakładały się z jednym z innych VNets lub lokalnych sieci, które chcesz się połączyć.

## <a name="step-4---add-the-local-network-sites"></a>Krok 4 — Dodawanie witryn sieci lokalnej

Podczas tworzenia konfiguracji VNet do VNet, musisz skonfigurować witryn sieci lokalnej, które są wyświetlane na stronie **Sieci lokalnych** portalu. Azure używa ustawienia określone w każdej witrynie sieci, aby dowiedzieć się, jak skierować ruch między VNets. Należy określić nazwę, której chcesz użyć, aby odwołać się do każdej witryny sieci lokalnej. Najlepiej używać coś opisowe, wybierz wartość z listy rozwijanej w dalszych krokach.

Na przykład VNet1 połączy się z witryną sieci lokalnej, które są tworzone o nazwie "VNet2Local". Ustawienia VNet2Local zawierają prefiksów adresów dla VNet2 i publiczny adres IP bramy VNet2. VNet2 połączy się z witryną sieci lokalnej, które można tworzyć o nazwie "VNet1Local", zawierający prefiksów adresów VNet1 i publiczny adres IP bramy VNet1.

### <a name="localnet"></a>Dodawanie witryny sieci lokalnej VNet1Local

1. W lewym dolnym rogu ekranu kliknij przycisk **Nowy** > **Usług sieciowych** > **Wirtualnej sieci** > **Dodawanie sieci lokalnej**.

2. Na stronie **Określ szczegóły swojej sieci lokalnej** w polu **Nazwa**wprowadź nazwę, która ma reprezentować sieć, z którą chcesz się połączyć. W tym przykładzie, można użyć "VNet1Local" dotyczyć zakresy adresów IP i bramą VNet1.

3. W polu **adres IP urządzenia VPN (opcjonalnie)**Określ dowolny prawidłowy publiczny adres IP. Zazwyczaj należy użyć rzeczywisty adres IP zewnętrznego dla urządzenia sieci VPN. W przypadku konfiguracji VNet do VNet program publiczny adres IP, przypisany do bramy usługi VNet. Jednak zakładając, że brama jeszcze nie został utworzony, można określić dowolny prawidłowy publiczny adres IP jako symbol zastępczy. Nie pozostaw to pole puste — nie jest opcjonalny dla tej konfiguracji. W ostatnim kroku wróć do tych ustawień i skonfigurować je przy użyciu odpowiednich adresów IP bramy, po Azure generuje go. Kliknij strzałkę, aby przejść do następnego ekranu.

4. Na stronie **Określ strony adres**wprowadź IP adres zakres i adres liczbę dla VNet1. To musi odpowiadać dokładnie zakres, który jest skonfigurowany do VNet1. Azure używa zakresy adresów IP, określające aby skierować ruch przeznaczony do VNet1. Kliknij znacznik wyboru, aby utworzyć sieci lokalnej.

### <a name="add-the-local-network-site-vnet2local"></a>Dodawanie witryny sieci lokalnej VNet2Local

Skorzystaj z powyższych kroków, aby utworzyć witrynę sieci lokalnej "VNet2Local". W razie potrzeby można odwołują się do wartości, na [przykład ustawienia](#step1) w kroku 1.

### <a name="configure-each-vnet-to-point-to-a-local-network"></a>Konfigurowanie każdego VNet, aby wskazywały sieci lokalnej

Każdy VNet muszą wskazywać odpowiednich sieci lokalnej, który chcesz skierować ruch do. 

#### <a name="for-vnet1"></a>Aby uzyskać VNet1

1. Przejdź do strony **Konfiguruj** wirtualnej sieci **VNet1**. 
2. W obszarze łączność z witryny do witryny zaznacz pole wyboru "Połącz się z siecią lokalną", a następnie wybierz **VNet2Local** jako sieci lokalnej z listy rozwijanej. 
3. Zapisywanie ustawień.

#### <a name="for-vnet2"></a>Aby uzyskać VNet2

1. Przejdź do strony **Konfiguruj** wirtualnej sieci **VNet2**. 
2. W obszarze łączność z witryny do witryny zaznacz pole wyboru "Połącz się z siecią lokalną", a następnie wybierz **VNet1Local** z menu rozwijanego jako sieci lokalnej. 
3. Zapisywanie ustawień.

## <a name="step-5---configure-a-gateway-for-each-vnet"></a>Krok 5 — należy skonfigurować bramę dla każdego VNet

Konfigurowanie bramy Routing dynamiczny dla każdej wirtualnej sieci. Ta konfiguracja nie obsługuje Routing statyczny bramy. Jeśli korzystasz z VNets wcześniej zostały skonfigurowane i już mają routingu dynamiczne bram, nie musisz wykonywać tej czynności. W przypadku usługi bram Routing statyczny, należy je usunąć i utwórz je ponownie jako Routing dynamiczny bramy. Jeśli usuniesz bramy publiczny adres IP do niego przypisana otrzymuje wydane, a trzeba powrócić i skonfigurowanie dowolnego z sieci lokalnej i urządzeń sieci VPN z nowego publiczny adres IP dla nowych bram.

1. Na stronie **sieci** Zweryfikuj w kolumnie Stan dla wirtualnych sieci **utworzone**.

2. W kolumnie **Nazwa** kliknij nazwę wirtualnej sieci. W tym przykładzie używamy "VNet1".

3. Na stronie **pulpitu nawigacyjnego** Zwróć uwagę, że ten VNet nie ma jeszcze skonfigurowane bramy. Zobaczysz ten status, zmienianie podczas wykonywania kroków czynności, aby skonfigurować funkcję bramy.

4. U dołu strony kliknij pozycję **Utwórz bramy** i **Routing dynamiczny**. System zostanie wyświetlony monit o potwierdzenie zamiaru bramy utworzony, kliknij przycisk Tak.

    ![Typ bramy](./media/virtual-networks-configure-vnet-to-vnet-connection/IC717026.png)  

5. Tworząc Centrum, zwróć uwagę, grafika bramy na stronie zmieni się na żółtej a zostanie wyświetlony komunikat "Tworzenie bramy". Zazwyczaj trwa około 30 minut bramy utworzyć.

6. Powtórz te same kroki dla VNet2. Pierwszy bramy VNet do wykonania przed przystąpieniem do tworzenia bramy do innych VNet nie jest konieczne.

7. Gdy stan bramy zmieni się na "Łączenie", publiczny adres IP dla każdej bramy jest widoczny na pulpicie nawigacyjnym. Zanotuj adres IP, który odpowiada do każdego VNet, aby go nie mieszać je w górę. Są to adresy IP, które są używane podczas edytowania adresów IP symbol zastępczy dla urządzenia VPN dla każdej sieci lokalnej.

## <a name="step-6---edit-the-local-network"></a>Krok 6 - Edytuj sieci lokalnej

1. Na stronie **Sieci lokalnych** kliknij nazwę sieci lokalnej, które chcesz edytować, a następnie kliknij przycisk **Edytuj** w dolnej części strony. W polu **adres IP urządzenia VPN**wprowadzania adresu IP bramy, która odpowiada VNet. Na przykład dla VNet1Local, należy umieścić w polu adres IP bramy przypisane do VNet1. Następnie kliknij strzałkę u dołu strony.

2. Na stronie **Określ przestrzeni adresów** kliknij znacznik wyboru w prawej dolnej bez wprowadzania zmian.

## <a name="step-7---create-the-vpn-connection"></a>Krok 7 — Utwórz połączenie VPN

Po wykonaniu wszystkie poprzednie kroki Ustaw kluczy wstępnych IPsec/IKE i utworzyć połączenie. Tego zestawu czynności używa programu PowerShell i nie można skonfigurować w portalu. Aby uzyskać więcej informacji o instalowaniu i poleceń cmdlet programu PowerShell Azure Zobacz, [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md) . Upewnij się pobrać najnowszą wersję pakietu poleceń cmdlet usługi zarządzania (k). 

1. Otwieranie programu Windows PowerShell i zaloguj się.

        Add-AzureAccount

2. Wybierz subskrypcję, który z VNets znajduje się w.

        Get-AzureSubscription | Sort SubscriptionName | Select SubscriptionName
        Select-AzureSubscription -SubscriptionName "<Subscription Name>"

3. Tworzenie połączenia. W przykładach Zauważ, że klucz udostępniony jest dokładnie tak samo. Klucz udostępniony musi odpowiadać.


    VNet1 VNet2 połączenia

        Set-AzureVNetGatewayKey -VNetName VNet1 -LocalNetworkSiteName VNet2Local -SharedKey A1b2C3D4

    VNet2 VNet1 połączenia

        Set-AzureVNetGatewayKey -VNetName VNet2 -LocalNetworkSiteName VNet1Local -SharedKey A1b2C3D4

4. Poczekaj, aż połączeń zainicjować. Po bramy został zainicjowany, bramy wygląda na poniższym rysunku.

    ![Stan bramy — połączenie](./media/virtual-networks-configure-vnet-to-vnet-connection/IC736059.jpg)  

    [AZURE.INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)] 

## <a name="next-steps"></a>Następne kroki

Możesz dodać maszyn wirtualnych do sieci wirtualnej. Zapoznaj się z [dokumentacją maszyn wirtualnych](https://azure.microsoft.com/documentation/services/virtual-machines/) Aby uzyskać więcej informacji.



[1]: ../hdinsight-hbase-geo-replication-configure-vnets.md
[2]: http://channel9.msdn.com/Series/Getting-started-with-Windows-Azure-HDInsight-Service/Configure-the-VPN-connectivity-between-two-Azure-virtual-networks
 
