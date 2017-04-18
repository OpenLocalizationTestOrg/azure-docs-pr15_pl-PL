<properties
    pageTitle="Wdrażanie z Internetu równoważenia obciążenia rozwiązanie z protokołu IPv6 przy użyciu szablonu | Microsoft Azure"
    description="Jak wdrożyć obsługę protokołu IPv6 usługi równoważenia obciążenia Azure i równoważenia obciążenia maszyny wirtualne."
    services="load-balancer"
    documentationCenter="na"
    authors="sdwheeler"
    manager="carmonm"
    editor=""
    tags="azure-resource-manager"
    keywords="Protokół IPv6, równoważenia obciążenia azure, podwójne stos, publiczny adres ip, natywny protokół ipv6, mobile, iot"
/>
<tags
    ms.service="load-balancer"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="09/14/2016"
    ms.author="sewhee"
/>

# <a name="deploy-an-internet-facing-load-balancer-solution-with-ipv6-using-a-template"></a>Wdrażanie z Internetu równoważenia obciążenia rozwiązanie z protokołu IPv6 przy użyciu szablonu

> [AZURE.SELECTOR]
- [Programu PowerShell](./load-balancer-ipv6-internet-ps.md)
- [Polecenie Azure](./load-balancer-ipv6-internet-cli.md)
- [Szablon](./load-balancer-ipv6-internet-template.md)

Usługi równoważenia obciążenia Azure jest równoważenia obciążenia warstwy-4 (port TCP, UDP). Usługi równoważenia obciążenia zapewnia wysoką dostępność przekazując ruch przychodzący między wystąpień prawidłowy usługi w chmurze usług lub maszyn wirtualnych w zestawie równoważenia obciążenia. Azure równoważenia obciążenia można także przedstawić tych usług na wiele portów i/lub wiele adresów IP.

## <a name="example-deployment-scenario"></a>Przykładowy scenariusz wdrażania

Na poniższym diagramie przedstawiono rozwiązania do równoważenia obciążenia wdrażania przy użyciu szablonu przykład opisane w tym artykule.

![Scenariusz równoważenia obciążenia](./media/load-balancer-ipv6-internet-template/lb-ipv6-scenario.png)

W tym scenariuszu utworzysz Azure następujące zasoby:

- wirtualna sieciowej dla każdego maszyn wirtualnych z adresami IPv4 i IPv6 przypisane
- usługi równoważenia obciążenia z Internetu przy użyciu protokołu IPv4 i IPv6 publiczny adres IP
- dwa ładowanie równoważenia reguły do zamapować publicznej służącą do prywatnych punktów końcowych
- Ustawianie dostępności zawierający dwa maszyny wirtualne
- dwa wirtualnych maszyn

## <a name="deploying-the-template-using-the-azure-portal"></a>Wdrażanie szablonów za pomocą portalu Azure

Ten artykuł zawiera odwołanie do szablonu, opublikowaną w galerii [Szablonów Szybki Start Azure](https://azure.microsoft.com/documentation/templates/201-load-balancer-ipv6-create/) . Można pobrać szablon z galerii lub uruchamianie wdrożenia w Azure bezpośrednio z galerii. W tym artykule założono, że szablon został pobrany na komputer lokalny.

1. Otwórz Azure portal i zaloguj się przy użyciu konta z uprawnieniami do tworzenia maszyny wirtualne i zasoby sieciowe w ramach subskrypcji usługi Azure. Ponadto chyba że korzystasz z istniejących zasobów, konto musi uprawnień do tworzenia grupy zasobów i kontem miejsca do magazynowania.

2. Kliknij przycisk "+ nowy" z menu, a następnie wpisz "szablon" w polu wyszukiwania. Wybierz pozycję "Szablonu wdrażania" z wyników wyszukiwania.

    ![portal kroku2, kg protokołu ipv6 w-](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step2.png)

3. Na stronie wszystko karta, kliknij pozycję "Wdrożenie szablonu".

    ![portal step3, kg protokołu ipv6 w-](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step3.png)

4. Kliknij przycisk "Utwórz".

    ![portal step4, kg protokołu ipv6 w-](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step4.png)

5. Kliknij przycisk "Edytuj szablon". Usuń istniejącą zawartość i kopiowania i wklejania w całą zawartość plik szablonu (na tym rozpoczęcie i zakończenie {}), a następnie kliknij przycisk "Zapisz".

    > [AZURE.NOTE] Jeśli korzystasz z programu Microsoft Internet Explorer, możesz wkleić wyświetlane okno dialogowe z pytaniem, aby umożliwić dostęp do Schowka systemu Windows. Kliknij pozycję "Zezwalaj na dostęp".

    ![portal step5, kg protokołu ipv6 w-](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step5.png)

6. Kliknij przycisk "Edytuj parametry". W karta Parametry określić wartości dla każdego porad w sekcji Parametry szablonu, a następnie kliknij przycisk Zapisz, aby zamknąć karta Parametry. W karta wdrożenia niestandardowe wybierz subskrypcję, istniejącej grupy zasobów lub utwórz go. Jeśli tworzysz grupa zasobów, następnie wybierz lokalizację dla grupy zasobów. Następnie kliknij **warunki prawne**, a następnie kliknij przycisk **Kup** warunki prawne. Azure rozpoczyna wdrażanie zasobów. Wystarczy kilka minut wdrażanie wszystkich zasobów.

    ![portal step6, kg protokołu ipv6 w-](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step6.png)

    Aby uzyskać więcej informacji na temat tych parametrów zobacz sekcję [parametrów szablonu i zmiennych](#template-parameters-and-variables) w dalszej części tego artykułu.

7. Aby wyświetlić zasoby utworzone przez szablon, kliknij przycisk Przeglądaj, przewiń listę w dół, aż zostanie zobacz "Zasobów grupy", a następnie kliknij go.

    ![kg ipv6-portalu — krok 7](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step7.png)

8. Na karta grup zasobów kliknij nazwę grupy zasobów, które określony w kroku 6. Zostanie wyświetlona lista wszystkich zasobów wdrożone. Jeśli wszystko się również, należy powiedzieć "Powiodło się" w obszarze "Ostatniego wdrożenia". Jeśli nie, upewnij się, że konto, którego używasz ma uprawnienia do tworzenia niezbędnych zasobów.

    ![portal step8, kg protokołu ipv6 w-](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step8.png)

    > [AZURE.NOTE] Po przejściu do grup zasobów natychmiast po wykonaniu kroku 6 "Ostatniej wdrażania" będzie wyświetlany stan "Wdrażanie" podczas wdrażania zasobów.

9. Kliknij pozycję "myIPv6PublicIP", na liście zasobów. Zobaczysz ma adres IPv6 w polu adres IP i że jej nazwa DNS jest wartość, która określony w parametrze dnsNameforIPv6LbIP w kroku 6. Ten zasób jest publiczna IPv6 adresów i nazwa użytkownika, jest dostępny dla klientów internetowych.

    ![portal step9, kg protokołu ipv6 w-](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step9.png)

## <a name="validate-connectivity"></a>Sprawdź poprawność łączności

Po pomyślnym wdrożeniu szablonu można sprawdzić poprawność łączności, wykonując następujące czynności:

1. Zaloguj się do portalu Azure i ustanowienia połączenia z każdym maszyny wirtualne, utworzone przez wdrożenia szablonu. Wdrożenie systemu Windows Głosowa serwera, uruchom ipconfig/wszystko z wiersza polecenia. Zobaczysz, że maszyny wirtualne mają adresy IPv4 i IPv6. Jeśli wdrożono maszyny wirtualne Linux, musisz skonfigurować system operacyjny Linux do odbierania dynamiczne adresy IP protokołu IPv6 przy użyciu z instrukcjami dla swojego rozkładu Linux.
2. W kliencie podłączonego do Internetu IPv6 Rozpocznij połączenie publiczny adres IP protokołu IPv6 usługi równoważenia obciążenia. Aby potwierdzić, że równoważenia obciążenia jest równoważenia między dwoma maszyny wirtualne, serwer sieci web, takich jak Microsoft Internet Information Services (IIS) można zainstalować na każdym maszyny wirtualne. Domyślne strony sieci web na wszystkich serwerach może zawierać tekst "Server0" lub "Server1" jednoznacznie identyfikować go. Następnie otwórz przeglądarkę internetową na kliencie podłączonego do Internetu IPv6 i wskaż nazwę hosta określony w parametrze dnsNameforIPv6LbIP równoważenia obciążenia, aby potwierdzić zakończenia do końca łączności IPv6 z każdego maszyn wirtualnych. Jeśli zobaczysz tylko strony sieci web z serwera tylko jedna, może być konieczne Wyczyść pamięć podręczną przeglądarki. Otwieranie wielu prywatnej sesji przeglądania. Powinien zostać wyświetlony odpowiedzi z każdego serwera.
3. W kliencie podłączonego do Internetu IPv4 Rozpocznij połączenie publiczny adres IP protokołu IPv4 usługi równoważenia obciążenia. Aby potwierdzić, że równoważenia obciążenia jest dwóch maszyny wirtualne równoważenia obciążenia, można przetestować przy użyciu usług IIS, wyszczególnione w kroku 2.
4. Z każdym Głosowa Rozpocznij wychodzące połączenie z urządzeniem protokołu IPv6 lub połączenie IP protokołu IPv4 Internet. W obu przypadkach widoczne dla urządzenia docelowy adres IP źródła jest publiczny adres IP protokołu IPv4 lub IPv6 usługi równoważenia obciążenia.

>[AZURE.NOTE]
ICMP IPv4 i IPv6 jest zablokowany w sieci Azure. W wyniku narzędzia ICMP, takich jak ping zawsze kończą się niepowodzeniem. Aby przetestować łączność, użyj zamiast TCP, takich jak TCPing lub polecenia cmdlet programu PowerShell Test-NetConnection. Należy zauważyć, że adresy IP wyświetlane na diagramie przedstawiono przykłady wartości, które mogą zostać wyświetlone. Ponieważ adresy IP protokołu IPv6 są przypisywane dynamicznie, adresy, które otrzymujesz będą różne i zależą od regionu. Ponadto są często publiczny adres IP protokołu IPv6 w module równoważenia obciążenia, aby rozpocząć z prefiksem innego niż prywatne adresy IP protokołu IPv6 w puli wewnętrznej.

## <a name="template-parameters-and-variables"></a>Parametry szablonu i zmiennych

Szablon Azure Menedżera zasobów zawiera wiele zmiennych i parametry, które można dostosować do własnych potrzeb. Zmienne są używane stałe wartości, które nie mają użytkownikowi na zmienianie. Parametry są używane dla wartości, że użytkownik ma udzielać wdrażanie szablonu. Przykład szablonu jest skonfigurowany do scenariusza opisane w tym artykule. Można dostosować tak, aby wymagań dotyczących środowiska.

Szablon przykład używanych w tym artykule zawiera następujące zmienne i parametry:

| Parametr / zmiennych | Notatki |
|-----------|-------|
| adminUsername | Określ nazwę służący do logowania się do maszyn wirtualnych przy użyciu konta administratora. |
| adminPassword | Określ hasło do konta administratora służący do logowania się do maszyn wirtualnych z. |
| dnsNameforIPv4LbIP | Określ nazwę hosta DNS, który ma zostać przypisany jako nazwę publicznej usługi równoważenia obciążenia. Ta nazwa jest rozpoznawana jako publiczny adres IP protokołu IPv4 usługi równoważenia obciążenia. Nazwa musi być mała i dopasowanie regex: ^ [a-z][a-z0-9-]{1,61}[a-z0-9]$. |
| dnsNameforIPv6LbIP | Określ nazwę hosta DNS, który ma zostać przypisany jako nazwę publicznej usługi równoważenia obciążenia. Ta nazwa jest rozpoznawana jako publiczny adres IP protokołu IPv6 usługi równoważenia obciążenia. Nazwa musi być mała i dopasowanie regex: ^ [a-z][a-z0-9-]{1,61}[a-z0-9]$. Może to być taką samą nazwę jak adres IP protokołu IPv4. Gdy klient wysyła kwerendzie DNS dla tej nazwy, które zwróci Azure A i AAAA rekordów po udostępnieniu nazwę. |
| vmNamePrefix | Określ prefiks nazwy maszyn wirtualnych. Szablon dołącza numer (0, 1, itd.) do nazwy podczas tworzenia maszyny wirtualne. |
| nicNamePrefix | Określ prefiks nazwy interfejsu sieci. Szablon dołącza numer (0, 1, itd.) do nazwy podczas tworzenia interfejsów sieciowych. |
| storageAccountName | Wprowadź nazwę istniejącego konta miejsca do magazynowania lub określ nazwę nowej witryny ma zostać utworzony w szablonie. |
| availabilitySetName | Następnie wprowadź nazwę dostępności Ustawianie do używania z maszyny wirtualne |
| addressPrefix | Prefiks adresu używane do określania zakresu adresów wirtualnej sieci |
| subnetName | Nazwa podsieci w tworzona dla VNet |
| subnetPrefix | Prefiks adresu używane do określania zakresu adresów podsieci |
| vnetName | Określ nazwę dla VNet używane za pośrednictwem SMS. |
| ipv4PrivateIPAddressType | Metoda alokacji służąca do prywatny adres IP (statyczna lub dynamiczna) |
| ipv6PrivateIPAddressType | Metoda alokacji służąca do prywatny adres IP (dynamiczny). Protokół IPv6 obsługuje tylko przydzielania. |
| numberOfInstances | Liczba wystąpień równoważenia obciążenia wdrożony w szablonie |
| ipv4PublicIPAddressName | Określ nazwę DNS, który ma być używany w celu poinformowania publiczny adres IP protokołu IPv4 usługi równoważenia obciążenia. |
| ipv4PublicIPAddressType | Metoda alokacji służąca do publiczny adres IP (statyczna lub dynamiczna) |
| Ipv6PublicIPAddressName | Określ nazwę DNS, który ma być używany w celu poinformowania publiczny adres IP protokołu IPv6 usługi równoważenia obciążenia. |
| ipv6PublicIPAddressType | Metoda alokacji służąca do publiczny adres IP (dynamiczny). Protokół IPv6 obsługuje tylko przydzielania. |
| lbName | Określ nazwę usługi równoważenia obciążenia. Ta nazwa jest wyświetlana w portalu lub używany przy odwoływaniu się do niego za pomocą polecenia programu PowerShell lub interfejsu wiersza polecenia. |

Pozostałe zmienne w szablonie zawierają pochodnych wartości, które zostały przypisane podczas tworzenia Azure zasobów. Nie należy zmieniać tych zmiennych.
