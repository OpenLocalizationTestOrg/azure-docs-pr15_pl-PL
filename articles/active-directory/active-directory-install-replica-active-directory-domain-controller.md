<properties
    pageTitle="Instalowanie replice kontrolera domeny usługi Active Directory platformy Azure | Microsoft Azure"
    description="Samouczek, w którym wyjaśniono, jak zainstalować kontrolera domeny z las usługi Active Directory w lokalnej na Azure maszyn wirtualnych."
    services="virtual-network"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="virtual-network"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/30/2016"
    ms.author="curtand"/>


# <a name="install-a-replica-active-directory-domain-controller-in-an-azure-virtual-network"></a>Instalowanie kontrolera domeny usługi Active Directory replice w Azure wirtualnej sieci

W tym temacie przedstawiono sposób zainstaluj dodatkowe kontrolery domeny (nazywane także replice DCs) dla domeny usługi Active Directory w lokalnej na Azure wirtualnych maszyn w Azure wirtualnej sieci.

Może też Cię w następujących tematach pokrewnych:

-  Opcjonalnie możesz zainstalować nowy las usługi Active Directory w Azure wirtualnej sieci. Dla tych kroków zobacz [Instalowanie nowy las usługi Active Directory w Azure wirtualnej sieci](../active-directory/active-directory-new-forest-virtual-machine.md).
-  Koncepcyjny wytyczne dotyczące instalowania usługach domenowych Active Directory (AD DS) na Azure wirtualną sieć Zobacz [wskazówki dotyczące wdrażania systemu Windows Server usługi Active Directory na maszyn wirtualnych Azure](https://msdn.microsoft.com/library/azure/jj156090.aspx).


## <a name="scenario-diagram"></a>Scenariusz diagramu

W tym scenariuszu użytkownicy zewnętrzni muszą uzyskiwać dostęp do aplikacji, które są uruchamiane na serwerach domeny. Maszyny wirtualne, które Uruchom serwery aplikacji i DCs replice są zainstalowane w Azure wirtualną sieć. Wirtualnej sieci mogą być połączone z sieci lokalnej przez połączenie [VPN witryny do witryny](../vpn-gateway/vpn-gateway-site-to-site-create.md) , jak pokazano na poniższym diagramie lub za pomocą [ExpressRoute](../expressroute/expressroute-locations-providers.md) szybsze połączenia.

Serwery aplikacji i DCs są wdrożone w ciągu usług w chmurze oddzielnych do rozpowszechniania przetwarzania obliczeń i [zestawy dostępność](../virtual-machines/virtual-machines-windows-manage-availability.md) ulepszone odporności na uszkodzenia.
DCs replikować ze sobą oraz z lokalnego DCs przy użyciu replikacji usługi Active Directory. Narzędzia do synchronizacji nie są wymagane.

![Diagram kontrolera domeny w usłudze Active Directory replice pf Azure vnet][1]

## <a name="create-an-active-directory-site-for-the-azure-virtual-network"></a>Utwórz witrynę usługi Active Directory dla Azure wirtualnej sieci

Jest dobrym pomysłem jest tworzenie witryny w usłudze Active Directory, reprezentujący region sieci odpowiadająca wirtualnej sieci. Który pomaga zoptymalizować uwierzytelniania, replikacji i innych operacji lokalizacji kontrolera domeny. Poniższe kroki wyjaśniono, jak tworzyć witryny i więcej, zobacz [Dodawanie nowej witryny](https://technet.microsoft.com/library/cc781496.aspx).

1. Otwórz Lokacje i usługi Active Directory: **Menedżer serwera** > **Narzędzia** > **Lokacje i usługi Active Directory**.
2. Tworzenie witryny reprezentować region, w którym utworzono Azure wirtualną sieć: kliknij pozycję **witryny** > **akcji** > **nowej witryny** > wpisz nazwę nowej witryny, takie jak Azure nam zachód > Wybieranie łącza do witryny > **OK**.
3. Utworzyć podsieć i skojarzyć z nowej witryny: kliknij dwukrotnie pozycję **witryny** > kliknij prawym przyciskiem myszy **podsieci** > **Nowa podsieć** > wpisz zakres adresów IP wirtualnej sieci (na przykład 10.1.0.0/16 na diagramie scenariusz) > Wybierz pozycję Nowa witryna Azure > **OK**.

## <a name="create-an-azure-virtual-network"></a>Tworzenie wirtualnych sieci Azure

1. W [portalu klasyczny Azure](https://manage.windowsazure.com), kliknij przycisk **Nowy** > **Usług sieciowych** > **Wirtualnej sieci** > **Niestandardowe tworzenie** i używanie następujące wartości, aby zakończyć działanie kreatora.

    Na tej stronie kreatora...  | Określ następujące wartości
    ------------- | -------------
    **Szczegóły wirtualnej sieci**  | <p>Nazwa: Wpisz nazwę sieci wirtualnej, takich jak WestUSVNet.</p><p>Region: Wybierz najbliższym region.</p>
    **Połączenie VPN i serwera DNS**  | <p>Serwery DNS: Podaj nazwę i adres IP jednego lub kilku serwerów DNS lokalnego.</p><p>Łączność: Wybierz pozycję **Konfiguruj VPN witryny do witryny**.</p><p>Sieci lokalnej: Określanie sieci lokalnej.</p><p>Jeśli korzystasz z ExpressRoute zamiast sieci VPN, zobacz [Konfigurowanie połączenia ExpressRoute za pośrednictwem dostawcy programu Exchange](../expressroute/expressroute-locations-providers.md).</p>
    **Łączność witryny do witryny**  | <p>Nazwa: Wpisz nazwę sieci lokalnej.</p><p>Adres IP urządzenia VPN: Określ publiczny adres IP urządzenia, który będzie łączyć się wirtualnej sieci. Urządzenia VPN nie może znajdować się za translatora adresów sieciowych.</p><p>Adres: Określ zakresy adresów w sieci lokalnej (na przykład 192.168.0.0/16 na diagramie scenariusz).</p>
    **Obszar adresów wirtualnych sieci**  | <p>Przestrzeń adresów: Określ zakres adresów IP dla maszyny wirtualne, które mają być uruchamiane w Azure wirtualną sieć (na przykład 10.1.0.0/16 na diagramie scenariusz). Ten zakres adresów nie nakładały się z zakresów adresów sieci lokalnej.</p><p>Podsieci: Określ nazwę i adres podsieci dla serwerów aplikacji (takich jak Frontend, 10.1.1.0/24) i DCs (takich jak wewnętrznej bazy danych, 10.1.2.0/24).</p><p>Kliknij przycisk **Dodaj podsieci bramy**.</p>

2. Zostanie następnie skonfiguruj bramy wirtualnej sieci w celu utworzenia bezpiecznego połączenia VPN witryny do witryny. Aby uzyskać instrukcje, zobacz temat [Konfigurowanie Brama wirtualnej sieci](../vpn-gateway/vpn-gateway-configure-vpn-gateway-mp.md) .
3. Utwórz połączenie VPN witryny do witryny między nowe wirtualnej sieci i urządzenia VPN lokalnego. Aby uzyskać instrukcje, zobacz temat [Konfigurowanie Brama wirtualnej sieci](../vpn-gateway/vpn-gateway-configure-vpn-gateway-mp.md) .


## <a name="create-azure-vms-for-the-dc-roles"></a>Tworzenie maszyny wirtualne Azure dla ról kontrolera domeny

Powtórz następujące czynności, aby utworzyć maszyny wirtualne do obsługi roli kontrolera domeny, stosownie do potrzeb. Należy wdrożyć co najmniej dwa DCs wirtualnych zapewniające odporność na uszkodzenia i nadmiarowości. Jeśli Azure wirtualną sieć zawiera co najmniej dwa DCs, podobnie skonfigurowanych (oznacza to, że są one obu wykazów globalnych uruchamianie serwera DNS, i nie zawiera dowolny klawisz i tak dalej) umieść maszyny wirtualne uruchamianych tych DCs dostępności Ustaw ulepszone odporności na uszkodzenia.
Aby utworzyć maszyny wirtualne przy użyciu programu Windows PowerShell zamiast interfejsu użytkownika, zobacz [Używanie Azure utworzyć i wstępnie skonfigurować maszyn wirtualnych systemu Windows](../virtual-machines/virtual-machines-windows-classic-create-powershell.md).

1. W [portalu klasyczny Azure](https://manage.windowsazure.com), kliknij przycisk **Nowy** > **obliczyć** > **maszyn wirtualnych** > **Z galerii**. Zastosuj następujące wartości, aby zakończyć działanie kreatora. Zaakceptuj wartości domyślne ustawienia chyba że inną wartość jest sugerowane lub wymagane.

    Na tej stronie kreatora...  | Określ następujące wartości
    ------------- | -------------
    **Wybierz obraz**  | Windows Server 2012 R2 w centrum danych
    **Konfiguracja maszyn wirtualnych**  | <p>Nazwa maszyn wirtualnych: Wpisz nazwę pojedyncza etykieta (na przykład AzureDC1).</p><p>Nowa nazwa użytkownika: Wpisz nazwę użytkownika. Ten użytkownik będzie członkiem lokalnej grupy Administratorzy na maszyn wirtualnych. Konieczne będzie tej nazwy, aby zalogować się do maszyn wirtualnych po raz pierwszy. Wbudowane konto o nazwie Administrator nie będzie działać.</p><p>Nowe hasło i Potwierdź: Hasło wpisz</p>
    **Konfiguracja maszyn wirtualnych**  | <p>Usługa w chmurze: <b>Tworzenie nowej usługi w chmurze</b> dla pierwszej maszyn wirtualnych i wybierz przycisk tej samej nazwy usługi w chmurze podczas tworzenia więcej maszyny wirtualne obsługujące roli kontrolera domeny.</p><p>Nazwa DNS usługi w chmurze: Określ globalnie unikatową nazwę</p><p>Region/grupy koligacji/wirtualnej sieci: Określ nazwę wirtualną sieć (na przykład WestUSVNet).</p><p>Konto miejsca do magazynowania: Wybierz pozycję <b>Użyj konta automatycznie wygenerowanego miejsca do magazynowania</b> dla pierwszej maszyn wirtualnych, a następnie wybierz tę samą nazwę konta magazynu podczas tworzenia więcej maszyny wirtualne obsługujące roli kontrolera domeny.</p><p>Konfigurowanie dostępności: Wybierz pozycję <b>Utwórz zestaw dostępności</b>.</p><p>Nazwa zestawu dostępność: wpisz nazwę dostępność ustawianie podczas tworzenie pierwszej maszyn wirtualnych, a następnie wybierz pozycję takie same nazwy podczas tworzenia więcej maszyny wirtualne.</p>
    **Konfiguracja maszyn wirtualnych**  | <p>Wybierz pozycję <b>Instalowanie agenta maszyn wirtualnych</b> i innych rozszerzeń, które są potrzebne.</p>
2. Dołączanie dysku do każdego maszyn wirtualnych uruchamiające rola serwera kontrolera domeny. Dodatkowy dysk jest potrzebny do przechowywania bazy danych, dzienników i SYSVOL AD. Określ wielkość dysku (na przykład 10 GB) i pozostaw zestaw **Brak** **Preferencje pamięci podręcznej hosta** . Aby uzyskać instrukcje zobacz [jak podłączyć dysk danych do maszyny wirtualnej systemu Windows](../virtual-machines/virtual-machines-windows-classic-attach-disk.md).
3. Po pierwszym zalogowaniu się do maszyn wirtualnych, otwórz **Menedżera serwera** > **plików i usługi magazynu** do utworzenia woluminu na dysku przy użyciu NTFS.
4. Rezerwowanie statycznego adresu IP dla maszyny wirtualne, które będą działać roli kontrolera domeny. Aby zarezerwować statyczny adres IP, Pobierz Instalatora platformy sieci Web firmy Microsoft i [Zainstaluj Azure programu PowerShell](../powershell-install-configure.md) i uruchom polecenie cmdlet Set-AzureStaticVNetIP. Na przykład:

    "AzureDC1 - NazwaUsługi get AzureVM-nazwa AzureDC1 | Ustawianie AzureStaticVNetIP - adres IP 10.0.0.4 | Aktualizacja AzureVM

Aby uzyskać więcej informacji o ustawianiu statycznego adresu IP Zobacz [Konfigurowanie wewnętrznych statyczny adres IP maszyny](../virtual-network/virtual-networks-reserved-private-ip.md).

## <a name="install-ad-ds-on-azure-vms"></a>Instalowanie usług AD DS na Azure maszyny wirtualne

Zaloguj się do maszyny i sprawdź, czy masz łączność przez połączenie VPN lub ExpressRoute witryny do witryny do zasobów w sieci lokalnej. Następnie zainstalować usług AD DS na maszyny wirtualne Azure. Możesz użyć tym samym procesem, który służy do instalowania dodatkowych kontrolera domeny w tej samej sieci lokalnej (interfejsu użytkownika, programu Windows PowerShell lub pliku odpowiedzi). Po zainstalowaniu usług AD DS, upewnij się, że określ wielkość nowej lokalizacji bazy danych, dzienników oraz SYSVOL AD. Jeśli potrzebujesz przypomnieć instalacji usług AD DS, zobacz [Instalowanie usług domenowych Active Directory (poziom 100)](https://technet.microsoft.com/library/hh472162.aspx) lub [Instalowanie kontrolera domeny replice systemu Windows Server 2012 do istniejącej domeny (poziom 200)](https://technet.microsoft.com/library/jj574134.aspx).

## <a name="reconfigure-dns-server-for-the-virtual-network"></a>Skonfigurowanie serwera DNS dla sieci wirtualnej

1. W [portalu klasyczny Azure](https://manage.windowsazure.com)kliknij nazwę wirtualnej sieci, a następnie kliknij kartę **Konfigurowanie** o [skonfigurowanie adresów IP serwerów DNS sieci wirtualnej](../virtual-network/virtual-networks-manage-dns-in-vnet.md) umożliwia statyczne adresy IP przypisane do replice DCs zamiast adresy IP lokalnych serwerów DNS.

2. Aby upewnić się, czy wszystkie replice maszyny wirtualne kontrolera domeny w wirtualnej sieci są skonfigurowane do korzystania z serwerów DNS w sieci wirtualnej, kliknij **maszyn wirtualnych**, kliknij kolumnę Stan dla każdego maszyn wirtualnych, a następnie kliknij przycisk **Uruchom**. Poczekaj, aż maszyn wirtualnych pokazuje stan **uruchomiony** , zanim spróbujesz Zaloguj się do niego.

## <a name="create-vms-for-application-servers"></a>Tworzenie maszyny wirtualne dla serwerów aplikacji

1. Powtórz następujące czynności, aby utworzyć maszyny wirtualne do uruchamiania jako serwery aplikacji. Zaakceptuj wartości domyślne ustawienia chyba że inną wartość jest sugerowane lub wymagane.

    Na tej stronie kreatora...  | Określ następujące wartości
    ------------- | -------------
    **Wybierz obraz**  | Windows Server 2012 R2 w centrum danych
    **Konfiguracja maszyn wirtualnych**  | <p>Nazwa maszyn wirtualnych: Wpisz nazwę pojedyncza etykieta (na przykład AppServer1).</p><p>Nowa nazwa użytkownika: Wpisz nazwę użytkownika. Ten użytkownik będzie członkiem lokalnej grupy Administratorzy na maszyn wirtualnych. Konieczne będzie tej nazwy, aby zalogować się do maszyn wirtualnych po raz pierwszy. Wbudowane konto o nazwie Administrator nie będzie działać.</p><p>Nowe hasło i Potwierdź: Hasło wpisz</p>
    **Konfiguracja maszyn wirtualnych**  | <p>Usługa w chmurze: **Tworzenie nowej usługi w chmurze** dla pierwszej maszyn wirtualnych i wybierz przycisk tej samej nazwy usługi w chmurze podczas tworzenia więcej maszyny wirtualne obsługujące aplikacji.</p><p>Nazwa DNS usługi w chmurze: Określ globalnie unikatową nazwę</p><p>Region/grupy koligacji/wirtualnej sieci: Określ nazwę wirtualną sieć (na przykład WestUSVNet).</p><p>Konto miejsca do magazynowania: Wybierz pozycję **Użyj konta automatycznie wygenerowanego miejsca do magazynowania** dla pierwszej maszyn wirtualnych, a następnie wybierz tę samą nazwę konta magazynu podczas tworzenia więcej maszyny wirtualne obsługujące aplikacji.</p><p>Konfigurowanie dostępności: Wybierz pozycję **Utwórz zestaw dostępności**.</p><p>Nazwa zestawu dostępność: wpisz nazwę dostępność ustawianie podczas tworzenie pierwszej maszyn wirtualnych, a następnie wybierz pozycję takie same nazwy podczas tworzenia więcej maszyny wirtualne.</p>
    **Konfiguracja maszyn wirtualnych**  | <p>Wybierz pozycję <b>Instalowanie agenta maszyn wirtualnych</b> i innych rozszerzeń, które są potrzebne.</p>

2. Po każdym maszyn wirtualnych jest obsługi administracyjnej, zaloguj się i dołączyć go do tej domeny. W **Menedżerze serwera**kliknij **Lokalnego serwera** > **grupy roboczej** > **Zmień...** Wybierz **domenę** i wpisz nazwę domeny lokalnego. Poświadczenia użytkownika domeny, a następnie ponownie uruchom maszyn wirtualnych, aby zakończyć dołączanie domeny.

Aby utworzyć maszyny wirtualne przy użyciu programu Windows PowerShell zamiast interfejsu użytkownika, zobacz [Używanie Azure utworzyć i wstępnie skonfigurować maszyn wirtualnych systemu Windows](../virtual-machines/virtual-machines-windows-classic-create-powershell.md).

Aby uzyskać więcej informacji na temat korzystania z programu Windows PowerShell zobacz [Wprowadzenie do poleceń cmdlet Azure](https://msdn.microsoft.com/library/azure/jj554332.aspx) i [Informacje dotyczące poleceń Cmdlet Azure](https://msdn.microsoft.com/library/azure/jj554330.aspx).

## <a name="additional-resources"></a>Dodatkowe zasoby

-  [Wskazówki dotyczące wdrażania systemu Windows Server usługi Active Directory w przypadku Azure maszyn wirtualnych](https://msdn.microsoft.com/library/azure/jj156090.aspx)
-  [Jak przekazywać istniejących kontrolerów domeny funkcji Hyper-V lokalnego Azure przy użyciu programu PowerShell Azure](http://support.microsoft.com/kb/2904015)
-  [Instalowanie nowej las usługi Active Directory w Azure wirtualnej sieci](../active-directory/active-directory-new-forest-virtual-machine.md)
-  [Azure wirtualnej sieci](../virtual-network/virtual-networks-overview.md)
-  [Microsoft Azure IT Pro IaaS: podstawy maszyn wirtualnych (01)](http://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/01)
-  [Microsoft Azure IT Pro IaaS: (05) Tworzenie wirtualnych sieci i łączność między lokalne](http://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/05)
-  [Azure programu PowerShell](https://msdn.microsoft.com/library/azure/jj156055.aspx)
-  [Polecenia cmdlet zarządzania Azure](https://msdn.microsoft.com/library/azure/jj152841)

<!--Image references-->
[1]: ./media/active-directory-install-replica-active-directory-domain-controller/ReplicaDCsOnAzureVNet.png
