<properties
    pageTitle="Instalowanie las usługi Active Directory w Azure wirtualnej sieci | Microsoft Azure"
    description="Samouczek, w którym wyjaśniono, jak utworzyć nowy las usługi Active Directory na komputerze wirtualnych (maszyn wirtualnych) w wirtualnej sieci Azure."
    services="active-directory, virtual-network"
    keywords="maszyn wirtualnych usługi Active directory, zainstaluj las usługi active directory, usługi azure active directory klipów wideo "
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    tags=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="10/10/2016"
    ms.author="markusvi"/>


# <a name="install-a-new-active-directory-forest-on-an-azure-virtual-network"></a>Instalowanie nowej las usługi Active Directory w Azure wirtualnej sieci

W tym temacie przedstawiono sposób tworzenia nowego środowiska usługi Active Directory systemu Windows Server w Azure wirtualnej sieci na komputerze wirtualnych (maszyn wirtualnych) [Azure wirtualnej sieci](../virtual-network/virtual-networks-overview.md). W tym przypadku Azure wirtualną sieć nie jest połączony z siecią lokalnego.

Może też Cię w następujących tematach pokrewnych:

- Film przedstawiający te kroki Zobacz, [jak zainstalować nowy las usługi Active Directory w Azure wirtualnej sieci](http://channel9.msdn.com/Series/Microsoft-Azure-Tutorials/How-to-install-a-new-Active-Directory-forest-on-an-Azure-virtual-network)
- Opcjonalnie można [skonfigurować połączenie VPN witryn do witryny](../vpn-gateway/vpn-gateway-site-to-site-create.md) , a następnie albo zainstalować nowy las lub rozszerzania las lokalnej wirtualną siecią Azure. Dla tych kroków zobacz [Instalowanie replice Active Directory kontrolera domeny w wirtualnej sieci Azure](../active-directory/active-directory-install-replica-active-directory-domain-controller.md).
-  Koncepcyjna wytyczne dotyczące instalowania usługach domenowych Active Directory (AD DS) na Azure wirtualną sieć Zobacz [wskazówki dotyczące wdrażania systemu Windows Server usługi Active Directory na maszyn wirtualnych Azure](https://msdn.microsoft.com/library/azure/jj156090.aspx).

## <a name="scenario-diagram"></a>Scenariusz diagramu

W tym scenariuszu zewnętrznych użytkownicy będą uzyskiwać dostęp do aplikacji, które są uruchamiane na serwerach domeny. Maszyny wirtualne uruchamianych serwery aplikacji i maszyny wirtualne uruchamianych kontrolery są zainstalowaną w ich usłudze w chmurze w Azure wirtualnej sieci. Są one również zawarte w obrębie dostępność Ustaw ulepszone odporności na uszkodzenia.

![Las usługi Active Directory w przypadku maszyn wirtualnych w wirtualnej sieci Azure ][1] 7
## <a name="how-does-this-differ-from-on-premises"></a>Jak to różni się od lokalnego?

Jest wiele różnica między instalowania kontrolera domeny Azure i lokalnych. Główne różnice są wymienione w poniższej tabeli.

Aby skonfigurować...  | Lokalne  | Azure wirtualnej sieci
------------- | -------------  | ------------
**Adres IP kontrolera domeny**  | Przypisywanie statycznego adresu IP na właściwości karty sieci   | Uruchom polecenie cmdlet Set-AzureStaticVNetIP przypisać statyczny adres IP
**Program rozpoznawania nazw DNS klienta**  | Ustawianie adresu serwera preferowany i alternatywny serwer DNS w sieci właściwości karty członków domeny   | Ustawianie adresu serwera DNS na właściwości wirtualnej sieci
**Magazynu bazy danych Active Directory**  | Opcjonalnie można zmienić domyślnej lokalizacji przechowywania z C:\  | Aby zmienić domyślne miejsce przechowywania z C:\



## <a name="create-an-azure-virtual-network"></a>Tworzenie wirtualnych sieci Azure

1. Zaloguj się do portalu klasyczny Azure.
2. Tworzenie wirtualnych sieci. Kliknij przycisk **sieci** > **Tworzenie wirtualnych sieci**. Użyj wartości z poniższej tabeli, aby zakończyć działanie kreatora.

    Na tej stronie kreatora...  | Określ następujące wartości
    ------------- | -------------
    **Szczegóły wirtualnej sieci**  | <p>Nazwa: Wprowadź nazwę sieci wirtualnej</p><p>Region: Wybieranie najbliższym regionu</p>
    **I serwera DNS sieci VPN**  | <p>Puste serwera DNS</p><p>Nie zaznaczaj opcję VPN</p>
    **Obszar adresów wirtualnych sieci**  | <p>Nazwa podsieci: Wprowadź nazwę dla danej podsieci</p><p>Początkowy adres IP: <b>10.0.0.0</b></p><p>CIDR: <b>/24 (256)</b></p>



## <a name="create-vms-to-run-the-domain-controller-and-dns-server-roles"></a>Tworzenie maszyny wirtualne, aby uruchomić kontrolera domeny i role serwerów DNS

Powtórz następujące czynności, aby utworzyć maszyny wirtualne do obsługi roli kontrolera domeny, stosownie do potrzeb. Należy wdrożyć co najmniej dwa DCs wirtualnych zapewniające odporność na uszkodzenia i nadmiarowości. Jeśli Azure wirtualną sieć zawiera co najmniej dwa DCs, podobnie skonfigurowanych (oznacza to, że są one obu wykazów globalnych uruchamianie serwera DNS, i nie zawiera dowolny klawisz i tak dalej) umieść maszyny wirtualne uruchamianych tych DCs dostępności Ustaw ulepszone odporności na uszkodzenia.

Aby utworzyć maszyny wirtualne przy użyciu programu Windows PowerShell zamiast interfejsu użytkownika, zobacz [Używanie Azure utworzyć i wstępnie skonfigurować maszyn wirtualnych systemu Windows](../virtual-machines/virtual-machines-windows-classic-create-powershell.md).

1. W portalu klasyczny, kliknij przycisk **Nowy** > **obliczyć** > **maszyn wirtualnych** > **Z galerii**. Zastosuj następujące wartości, aby zakończyć działanie kreatora. Zaakceptuj wartości domyślne ustawienia, chyba że inną wartość jest sugerowane lub wymagane.

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

## <a name="install-windows-server-active-directory"></a>Instalowanie usługi Active Directory systemu Windows Server

Użyć samej procedury [instalowania usług AD DS](https://technet.microsoft.com/library/jj574166.aspx) , używanie lokalnego (oznacza to, że można interfejsu użytkownika, pliku odpowiedzi lub środowiska Windows PowerShell). Musisz podać poświadczenia administratora, aby zainstalować nowy las. Aby określić lokalizację bazy danych usługi Active Directory, dzienników i SYSVOL, zmienianie domyślnej lokalizacji przechowywania dysk systemu operacyjnego, dysk dodatkowe dane dołączone do maszyn wirtualnych.

Po zakończeniu instalacji kontrolera domeny, ponownie połączyć się maszyn wirtualnych i zaloguj się do kontrolera domeny. Pamiętaj określić poświadczenia domeny.

## <a name="reset-the-dns-server-for-the-azure-virtual-network"></a>Resetowanie serwera DNS Azure wirtualnej sieci

1. Resetowanie ustawienie usługi przesyłania dalej DNS na nowy serwer DNS kontrolera domeny.
  1. W Menedżerze serwera kliknij pozycję **Narzędzia** > **DNS**.
  2. W **Menedżerze DNS**kliknij prawym przyciskiem myszy nazwę serwera DNS, a następnie kliknij polecenie **Właściwości**.
  3. Na karcie **usługi przesyłania dalej** kliknij adres IP usługi przesyłania dalej, a następnie kliknij przycisk **Edytuj**.  Zaznacz adres IP, a następnie kliknij przycisk **Usuń**.
  4. Kliknij **przycisk OK** , aby zamknąć Edytor i **Ok** , aby zamknąć właściwości serwera DNS.
2. Zaktualizuj ustawienia serwera DNS dla sieci wirtualnej.
  1. Kliknij pozycję **Wirtualnych sieci** > kliknij dwukrotnie wirtualną sieć utworzono > **Konfiguruj** > **serwerów DNS**, wpisz nazwę i DIP jednego z maszyny wirtualne, które są wykonywane rola serwera DNS kontrolera domeny i kliknij przycisk **Zapisz**.
  2. Zaznacz maszyn wirtualnych, a następnie kliknij przycisk **Uruchom ponownie** , aby wyzwolić maszyn wirtualnych na konfigurowanie ustawień rozpoznawania nazw DNS z adresem IP nowego serwera DNS.


## <a name="create-vms-for-domain-members"></a>Tworzenie maszyny wirtualne dla członków domeny

1. Powtórz następujące czynności, aby utworzyć maszyny wirtualne do uruchamiania jako serwery aplikacji. Zaakceptuj wartości domyślne ustawienia, chyba że inną wartość jest sugerowane lub wymagane.

    Na tej stronie kreatora...  | Określ następujące wartości
    ------------- | -------------
    **Wybierz obraz**  | Windows Server 2012 R2 w centrum danych
    **Konfiguracja maszyn wirtualnych**  | <p>Nazwa maszyn wirtualnych: Wpisz nazwę pojedyncza etykieta (na przykład AppServer1).</p><p>Nowa nazwa użytkownika: Wpisz nazwę użytkownika. Ten użytkownik będzie członkiem lokalnej grupy Administratorzy na maszyn wirtualnych. Konieczne będzie tej nazwy, aby zalogować się do maszyn wirtualnych po raz pierwszy. Wbudowane konto o nazwie Administrator nie będzie działać.</p><p>Nowe hasło i Potwierdź: Hasło wpisz</p>
    **Konfiguracja maszyn wirtualnych**  | <p>Usługa w chmurze: **Tworzenie nowej usługi w chmurze** dla pierwszej maszyn wirtualnych i wybierz przycisk tej samej nazwy usługi w chmurze podczas tworzenia więcej maszyny wirtualne obsługujące aplikacji.</p><p>Nazwa DNS usługi w chmurze: Określ globalnie unikatową nazwę</p><p>Region/grupy koligacji/wirtualnej sieci: Określ nazwę wirtualną sieć (na przykład WestUSVNet).</p><p>Konto miejsca do magazynowania: Wybierz pozycję **Użyj konta automatycznie wygenerowanego miejsca do magazynowania** dla pierwszej maszyn wirtualnych, a następnie wybierz tę samą nazwę konta magazynu podczas tworzenia więcej maszyny wirtualne obsługujące aplikacji.</p><p>Konfigurowanie dostępności: Wybierz pozycję **Utwórz zestaw dostępności**.</p><p>Nazwa zestawu dostępność: wpisz nazwę dostępność ustawianie podczas tworzenie pierwszej maszyn wirtualnych, a następnie wybierz pozycję takie same nazwy podczas tworzenia więcej maszyny wirtualne.</p>
    **Konfiguracja maszyn wirtualnych**  | <p>Wybierz pozycję <b>Instalowanie agenta maszyn wirtualnych</b> i innych rozszerzeń, które są potrzebne.</p>
2. Po każdym maszyn wirtualnych jest obsługi administracyjnej, zaloguj się i dołączyć go do tej domeny. W oknie **Menedżer serwerów**kliknij **Lokalnego serwera** > **grupy roboczej** > **Zmień...** Wybierz **domenę** i wpisz nazwę domeny lokalnego. Poświadczenia użytkownika domeny, a następnie ponownie uruchom maszyn wirtualnych, aby zakończyć dołączanie domeny.

Aby utworzyć maszyny wirtualne przy użyciu programu Windows PowerShell zamiast interfejsu użytkownika, zobacz [Używanie Azure utworzyć i wstępnie skonfigurować maszyn wirtualnych systemu Windows](../virtual-machines/virtual-machines-windows-classic-create-powershell.md).

Aby uzyskać więcej informacji na temat korzystania z programu Windows PowerShell zobacz [Wprowadzenie do poleceń cmdlet Azure](https://msdn.microsoft.com/library/azure/jj554332.aspx) i [Informacje dotyczące poleceń Cmdlet Azure](https://msdn.microsoft.com/library/azure/jj554330.aspx).


## <a name="see-also"></a>Zobacz też

-  [Jak zainstalować nowy las usługi Active Directory w Azure wirtualnej sieci](http://channel9.msdn.com/Series/Microsoft-Azure-Tutorials/How-to-install-a-new-Active-Directory-forest-on-an-Azure-virtual-network)
-  [Wskazówki dotyczące wdrażania systemu Windows Server usługi Active Directory w przypadku Azure maszyn wirtualnych](https://msdn.microsoft.com/library/azure/jj156090.aspx)

-  [Konfigurowanie sieci VPN witryny do witryny](../vpn-gateway/vpn-gateway-site-to-site-create.md)
-  [Instalowanie replice Active Directory kontrolera domeny w Azure wirtualnej sieci](../active-directory/active-directory-install-replica-active-directory-domain-controller.md)
-  [Microsoft Azure IT Pro IaaS: podstawy maszyn wirtualnych (01)](http://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/01)
-  [Microsoft Azure IT Pro IaaS: (05) Tworzenie wirtualnych sieci i łączność między lokalne](http://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/05)
-  [Omówienie wirtualnych sieci](../virtual-network/virtual-networks-overview.md)
-  [Jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md)
-  [Azure programu PowerShell](https://msdn.microsoft.com/library/azure/jj156055.aspx)
-  [Informacje dotyczące poleceń Cmdlet Azure](https://msdn.microsoft.com/library/azure/jj554330.aspx)
-  [Ustawianie Azure maszyn wirtualnych statyczny adres IP](http://windowsitpro.com/windows-azure/set-azure-vm-static-ip-address)
-  [Jak przypisać statyczny adres IP maszyn wirtualnych Azure](http://www.bhargavs.com/index.php/2014/03/13/how-to-assign-static-ip-to-azure-vm/)
-  [Zainstaluj nowy las Active Directory](https://technet.microsoft.com/library/jj574166.aspx)
-  [Wprowadzenie do usługi Active Directory Domain Services (AD DS) wirtualizacji (poziom 100)](https://technet.microsoft.com/library/hh831734.aspx)

<!--Image references-->
[1]: ./media/active-directory-new-forest-virtual-machine/AD_Forest.png
