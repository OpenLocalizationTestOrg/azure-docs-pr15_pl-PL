<properties
    pageTitle="Jak zainstalować i skonfigurować Azure programu PowerShell"
    description="Dowiedz się, jak zainstalować i skonfigurować Azure programu PowerShell."
    editor="tysonn"
    manager="dongill"
    documentationCenter=""
    services=""
    authors="coreyp-at-msft"/>

<tags
    ms.service="multiple"
    ms.workload="multiple"
    ms.tgt_pltfrm="powershell"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/30/2016"
    ms.author="coreyp"/>

# <a name="how-to-install-and-configure-azure-powershell"></a>Jak zainstalować i skonfigurować Azure programu PowerShell

<div class="dev-center-tutorial-selector sublanding"><a href="/manage/install-and-configure-windows-powershell/" title="PowerShell" class="current">PowerShell</a><a href="/manage/install-and-configure-cli/" title="Azure CLI">polecenie Azure</a></div>

##<a name="what-is-azure-powershell"></a>Co to jest Azure programu PowerShell?
Azure PowerShell to zestaw modułów, które zapewniają polecenia cmdlet, aby zarządzać Azure przy użyciu programu Windows PowerShell. Można użyć poleceń cmdlet tworzyć, testowanie, wdrażanie i zarządzanie rozwiązaniach i usługach dostarczona platformy Azure. W większości przypadków cmdlet może służyć samego zadania, jak Portal Azure, takich jak tworzenie i konfigurowanie usług w chmurze, maszyn wirtualnych wirtualnych sieci i aplikacjach sieci web.

## <a name="how-versioning-works"></a>Jak działa przechowywanie wersji

Azure programu PowerShell używa semantyczny przechowywania wersji, co oznacza, że jeśli wersji A > wersji B, a następnie ma API najbardziej aktualną wersję. Ponadto go oznacza, że zmiany w średniej wersja główna podziału zmieniać w jeden lub więcej poleceń cmdlet.  Tak na przykład wersja 1.7.0 jest poprawki adresem najnowsze zmiany w wersji 1.x Azure programu PowerShell.

Aby uzyskać więcej informacji o wskazówkach semantyczny przechowywanie wersji w programie PowerShell Azure, zobacz semantyczny specyfikacji przechowywania wersji na: http://semver.org
 
Aby uzyskać najnowsze interfejsy API, należy użyć wersji 2.x. Ale jeśli masz skrypty napisane przed wersji 1.x i nie chcesz pochłania zmian podziału w wersji 2.x opisanego w 2.x [Wersji](https://github.com/Azure/azure-powershell/blob/dev/documentation/release-notes/migration-guide.2.0.0.md), a następnie należy zainstalować 1.7.0.

Niezgodność wersji może spowodować, jeśli jest zainstalowana najnowsza wersja modułu profilu, a następnie załadowaniu starszej wersji programu modułu, który zależy od niego. Najłatwiejszym sposobem, aby rozwiązać ten problem należy zainstalować z najnowszych msi. MSI automatycznie czyści starsze wersje modułów.
 
###<a name="installing-module-versions-side-by-side"></a>Instalowanie modułu wersji przez siebie

Wersja 2.1.0 (i wersja 1.2.6 dla AzureStack) pierwszego modułu przeznaczone do instalowania i wersje używane obok siebie. Ponieważ Azure programu PowerShell używa binarne moduły, musisz Otwieranie nowego okna programu PowerShell i zaimportować określonej wersji cmdlet AzureRM za pomocą **Modułu importu** :

    Import-Module AzureRM -RequiredVersion 2.1.0**

Wersje przed 2.1.0 (innych niż 1.2.6) nie działa również side-by-side z innymi wersjami modułu programu PowerShell Azure. Podczas ładowania starszej wersji programu Azure PowerShell modułów przy użyciu polecenia taki jak przedstawiony powyżej, niezgodne wersje moduł **AzureRM.Profile** zostanie załadowana, uzyskując cmdlet pytaniem zalogowanie się w każdym przypadku, gdy podczas wykonywania polecenia cmdlet, nawet po zalogowaniu.

Najłatwiejszym sposobem rozwiązania jest zainstalowanie najnowszej Azure programu PowerShell z WebPI kanału lub msi — spowoduje to usunięcie wcześniejszych wersji modułów zainstalowany z galerii. 

Należy zauważyć, że zarówno Azure i AzureRM moduły zależności wspólne, więc użycie obu moduły podczas aktualizowania jedną należy zaktualizować obie. Starsze niż moduł Azure mają ten sam problem z modułem obok siebie ładowania tego wcześniejszych wersji modułu AzureRM mają.

<a id="Install"></a>
## <a name="step-1-install"></a>Krok 1: Instalowanie

Poniżej przedstawiono dwie metody, za pomocą których możesz zainstalować Azure programu PowerShell. Możesz zainstalować go z galerii programu PowerShell lub WebPI.

###<a name="installing-azure-powershell-from-the-powershell-gallery"></a>Instalowanie programu PowerShell Azure z galerii programu PowerShell

Preferowany sposób jest przy użyciu galerii programu PowerShell. Potrzebujesz moduł PowerShellGet przy użyciu galerii programu PowerShell. Ta opcja jest dostępna tutaj: [PowerShellGallery.com](https://www.powershellgallery.com/)

Instalowanie programu PowerShell Azure 1.3.0 lub większą z galerii programu PowerShell przy użyciu programu Windows PowerShell lub środowisko zintegrowane skryptów w PowerShell (ISE) wierszu z pełnymi uprawnieniami za pomocą następujących poleceń:

    # Install the Azure Resource Manager modules from the PowerShell Gallery
    Install-Module AzureRM

    # Install the Azure Service Management module from the PowerShell Gallery
    Install-Module Azure

####<a name="more-about-these-commands"></a>Więcej informacji na temat tych poleceń

- **Zainstaluj moduł AzureRM** instaluje moduł zestawienia dla poleceń cmdlet Menedżera zasobów Azure. Moduł AzureRM zależy od 
- zakres określonej wersji dla każdego modułu Azure Menedżera zasobów. Zakres dołączonej wersji zapewnia bez zmian moduł dzielenia można dołączyć podczas instalowania modułów AzureRM z tej samej wersji głównej. Po zainstalowaniu modułu AzureRM dowolny moduł Menedżera zasobów Azure, który wcześniej nie został zainstalowany zostanie pobrany i zainstalowany z galerii programu PowerShell. Aby uzyskać więcej informacji o semantyczny przechowywania wersji używane przez moduły Azure programu PowerShell zobacz [semver.org](http://semver.org). 
- **Zainstaluj moduł Azure** instaluje moduł Azure. Moduł ten jest moduł Zarządzanie usługą z Azure programu PowerShell 0.9.x. Powinna być nie główne zmiany i być wymiennym poprzedniej wersji Azure modułu.

###<a name="installing-azure-powershell-from-webpi"></a>Instalowanie programu PowerShell Azure z WebPI

Instalowanie Azure PowerShell 1.0 i większą z WebPI działa tak samo jak był 0.9.x. Pobierz [Azure programu PowerShell](http://aka.ms/webpi-azps) i rozpocznij instalację. Jeśli masz Azure programu PowerShell 0.9.x zainstalowany, wersja 0.9.x będzie można odinstalować w ramach uaktualniania. Jeśli zainstalowano moduły Azure programu PowerShell z galerii programu PowerShell Instalatora pakietu są automatycznie usuwane moduły przed instalacją zapewniające spójne środowisko Azure programu PowerShell.

> [AZURE.NOTE] Jeśli wcześniej została zainstalowana Azure moduły z galerii programu PowerShell, Instalator automatycznie usunąć je. Uniknąć zamieszania, o które wersje modułu masz zainstalowane i gdzie się znajdują. Moduły galerii programu PowerShell zwykle zostaną zainstalowane w **%ProgramFiles%\WindowsPowerShell\Modules**. Natomiast Instalator WebPI zainstaluje Azure moduły w * *% ProgramFiles (x86) %\Microsoft SDKs\Azure\PowerShell\**. Jeśli wystąpi błąd podczas instalacji, możesz ręcznie usunąć Azure* foldery w folderze **%ProgramFiles%\WindowsPowerShell\Modules** i spróbuj ponownie instalację.

Po ukończeniu instalacji usługi ```$env:PSModulePath``` ustawienie powinien zawierać katalogi zawierające polecenia cmdlet programu PowerShell Azure.

> [AZURE.NOTE] Jest to znany problem z programem PowerShell **$env: PSModulePath** które mogą wystąpić podczas instalacji z WebPI. Jeśli na komputerze wymaga ponownego uruchomienia ze względu na aktualizacje systemu lub innych urządzeń, może spowodować aktualizacji **$env: PSModulePath** do nie dołączono ścieżkę dostępu, w którym zainstalowano Azure programu PowerShell. W takim przypadku podczas próby użycia poleceń cmdlet środowiska PowerShell Azure po instalacji lub uaktualniania może zostać wyświetlony komunikat "polecenia cmdlet nierozpoznany". W takim przypadku ponowne uruchomienie komputera należy rozwiązać ten problem.

Jeśli podczas próby ładowanie lub wykonywanie poleceń cmdlet zostanie wyświetlony komunikat podobny do następujących czynności:

```
    PS C:\> Get-AzureRmResource
    Get-AzureRmResource : The term 'Get-AzureRmResource' is not recognized as the name of a cmdlet, function,
    script file, or operable program. Check the spelling of the name, or if a path was included, verify that the path is
    correct and try again.
    At line:1 char:1
    + Get-AzureRmResource
    + ~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : ObjectNotFound: (get-azurermresourcefork:String) [], CommandNotFoundException
        + FullyQualifiedErrorId : CommandNotFoundException
```

Ten problem można rozwiązać przez ponowne uruchomienie komputera lub importowanie cmdlet C:\Program Files\WindowsPowerShell\Modules\Azure\XXXX\ następujący (gdzie XXXX jest wersja zainstalowanego programu PowerShell:

```
Import-Module "C:\Program Files\WindowsPowerShell\Modules\Azure\XXXX\azure.psd1"
Import-Module "C:\Program Files\WindowsPowerShell\Modules\Azure\XXXX\expressroute\expressroute.psd1"
```

## <a name="step-2-start"></a>Krok 2: rozpoczynanie
Polecenia cmdlet może zostać uruchomiony z konsoli programu Windows PowerShell standardowy lub z programu PowerShell zintegrowane skrypty środowiska (ISE).
Metodę, która umożliwia otwieranie albo konsoli zależy od wersji systemu Windows:

- Na komputerze z systemem co najmniej systemu Windows 8 lub Windows Server 2012, można użyć wbudowanego wyszukiwania. Na ekranie **startowym** zacznij wpisywać power. To zwraca zakresu listy aplikacji, która zawiera programu Windows PowerShell. Aby otworzyć konsolę, kliknij pozycję aplikacja. (Przypinanie aplikacji do ekranu **startowego** , kliknij prawym przyciskiem myszy ikonę.)

- Na komputerze z systemem w wersji wcześniejszej niż systemu Windows 8 lub Windows Server 2012 przy użyciu **Start menu**. W **Start** menu kliknij polecenie **Wszystkie programy**, kliknij pozycję **Akcesoria**, kliknij folder **Programu Windows PowerShell** , a następnie kliknij **Programu Windows PowerShell**.

Możesz również uruchomić **Windows PowerShell ISE** umożliwia elementy menu i skrótów klawiaturowych do wykonywania wielu zadań wykonywanych czy w konsoli programu Windows PowerShell. Aby użyć ISE w konsoli programu Windows PowerShell, Cmd.exe, lub w polu **Uruchom** , wpisz **powershell_ise.exe**.

###<a name="commands-to-help-you-get-started"></a>Polecenia ułatwiające rozpoczęcie pracy

    # To make sure the Azure PowerShell module is available after you install
    Get-Module –ListAvailable 
    
    # To log in to Azure Resource Manager
    Login-AzureRmAccount

    # You can also use a specific Tenant if you would like a faster log in experience
    # Login-AzureRmAccount -TenantId xxxx

    # To view all subscriptions for your account
    Get-AzureRmSubscription

    # To select a default subscription for your current session
    Get-AzureRmSubscription –SubscriptionName “your sub” | Select-AzureRmSubscription

    # View your current Azure PowerShell session context
    # This session state is only applicable to the current session and will not affect other sessions
    Get-AzureRmContext

    # To select the default storage context for your current session
    Set-AzureRmCurrentStorageAccount –ResourceGroupName “your resource group” –StorageAccountName “your storage account name”

    # View your current Azure PowerShell session context
    # Note: the CurrentStorageAccount is now set in your session context
    Get-AzureRmContext

    # To list all of the blobs in all of your containers in all of your accounts
    Get-AzureRmStorageAccount | Get-AzureStorageContainer | Get-AzureStorageBlob


## <a name="step-3-connect"></a>Krok 3: łączenie
Cmdlet muszą subskrypcji, aby umożliwić ich Zarządzanie usługami. Jeśli nie masz jeszcze jedną, możesz kupić subskrypcję usługi Azure. Aby uzyskać instrukcje zobacz [jak kupić Azure](http://go.microsoft.com/fwlink/p/?LinkId=320795).

1. Typ **AzureRmAccount logowania**

2. Wpisz adres e-mail i hasło skojarzone z kontem. Azure uwierzytelnia i zapisuje informacje poświadczeń, a następnie zamyka okno.

— LUB —

Zaloguj się do pracy konto służbowe:

    $cred = Get-Credential
    Login-AzureRmAccount -Credential $cred
> [AZURE.NOTE] Jeśli masz więcej niż jeden dzierżawy skojarzony z konta organizacyjnego, ustaw wartość parametru TenantId:

    $loadersubscription = Get-AzureRmSubscription -SubscriptionName $YourSubscriptionName -TenantId $YourAssociatedSubscriptionTenantId


> [AZURE.NOTE] Ten dziennik-interactive metody działa tylko przy użyciu konta służbowego. Konto służbowe to użytkownik, który jest zarządzane przez swojej pracy lub szkole i zdefiniowane w przypadku usługi Azure Active Directory w swojej pracy lub szkole. Jeśli obecnie nie ma konta służbowego i przy użyciu konta Microsoft w celu zalogowania do subskrypcji usługi Azure, można łatwo można utworzyć jedną wykonaj następujące czynności.

> 1. Zaloguj się do [portalu klasyczny Azure](https://manage.windowsazure.com), a następnie kliknij polecenie **Usługi Active Directory**.

> 2. Jeśli katalog nie istnieje, wybierz opcję **Utwórz katalogu** i wprowadź wymagane informacje.

> 3. Zaznacz katalogu i dodawanie nowego użytkownika. Ten nowy użytkownik zalogować się przy użyciu konta służbowego. Podczas tworzenia użytkownika będzie dostarczanych z obu adresu e-mail dla użytkownika i hasła tymczasowego. Zapisz te informacje, ponieważ jest używana w kroku 5 poniżej.

> 4. W portalu klasyczny Azure wybierz pozycję **Ustawienia** , a następnie wybierz **administratorów**. Wybierz przycisk **Dodaj**, a następnie dodać nowego użytkownika jako administratora współpracujących. Dzięki temu konto służbowe lub szkolne zarządzać subskrypcją Azure.

> 5. Na koniec Wyloguj się z portalem klasyczny Azure, a następnie zaloguj się ponownie przy użyciu pracy konto służbowe. Jeśli jest to pierwszy rejestrowania czasu przy użyciu tego konta, pojawi się zmienianie.

> Aby uzyskać więcej informacji na tworzący konto w usłudze Microsoft Azure za pomocą konta służbowego zobacz [konta w usłudze Microsoft Azure jako organizacji](./active-directory/sign-up-organization.md).

> Aby uzyskać więcej informacji o zarządzaniu uwierzytelniania i subskrypcji w Azure zobacz [Zarządzaj kontami, subskrypcji i role administracyjne](http://go.microsoft.com/fwlink/?LinkId=324796).

### <a name="view-account-and-subscription-details"></a>Wyświetlanie szczegółów konta i subskrypcji

Masz wiele kont i subskrypcje dostępne do użycia przez Azure programu PowerShell. Możesz dodać wiele kont, uruchamiając **AzureRmAccount Dodaj** więcej niż jeden raz.

Aby wyświetlić dostępne konta Azure, wpisz **Get-AzureAccount**.

Aby wyświetlić subskrypcji Azure, wpisz **Get-AzureRmSubscription**.

##<a id="Help"></a>Uzyskiwanie pomocy##

Te zasoby zapewnia pomocy określonego polecenia cmdlet:


-   Z poziomu konsoli, możesz użyć wbudowany system pomocy. Polecenia cmdlet **Get-Help** zapewnia dostęp do tego systemu. 

- Aby uzyskać pomoc od społeczności spróbuj wykonać poniższe popularne fora:

 - [Azure forum w witrynie MSDN]( http://go.microsoft.com/fwlink/p/?LinkId=320212)
 - [Zdarzeń StackOverflow](http://go.microsoft.com/fwlink/?LinkId=320213)

##<a name="learn-more"></a>Dowiedz się więcej


Zobacz następujące zasoby, aby dowiedzieć się więcej o korzystaniu z poleceń cmdlet:

Aby uzyskać podstawowe instrukcje na temat korzystania z programu Windows PowerShell zobacz [Przy użyciu programu Windows PowerShell](http://go.microsoft.com/fwlink/p/?LinkId=321939).

Aby uzyskać informacje dotyczące poleceń cmdlet zobacz [Informacje dotyczące poleceń Cmdlet Azure](https://msdn.microsoft.com/library/windowsazure/jj554330.aspx).

Aby uzyskać przykładowe skrypty i instrukcje, aby uzyskać więcej informacji, aby zarządzać Azure za pomocą skryptów zobacz [Centrum skryptów](http://go.microsoft.com/fwlink/p/?LinkId=321940).

