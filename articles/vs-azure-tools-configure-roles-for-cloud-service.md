<properties
   pageTitle="Konfigurowanie ról dla usługi w chmurze Azure z programem Visual Studio | Microsoft Azure"
   description="Dowiedz się, jak utworzyć i skonfigurować ról dla usług w chmurze Azure za pomocą programu Visual Studio."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="configure-the-roles-for-an-azure-cloud-service-with-visual-studio"></a>Konfigurowanie ról dla usługi w chmurze Azure z programem Visual Studio

Usługa w chmurze Azure może mieć pracownik lub role w sieci web. Dla każdej roli należy określić, jak skonfigurować tej roli, a także skonfigurować działania tej roli. Aby uzyskać więcej informacji o rolach w usług w chmurze, zobacz [Wprowadzenie do usług w chmurze Azure](https://channel9.msdn.com/Series/Windows-Azure-Cloud-Services-Tutorials/Introduction-to-Windows-Azure-Cloud-Services)wideo. Informacje dotyczące usługi cloud są przechowywane w następujących plików:

- **ServiceDefinition.csdef**

    Plik definicji usługi określa ustawienia środowisko uruchomieniowe usługi w chmurze tym, jakie role są wymagane, punkty końcowe i rozmiar maszyn wirtualnych. Brak danych przechowywanych w tym pliku mogą być zmieniane, gdy Twoja rola jest uruchomiony.

- **ServiceConfiguration.cscfg**

    Plik konfiguracyjny usługi konfiguruje ile wystąpień roli są uruchamiane i wartości ustawień zdefiniowanych dla roli. Dane przechowywane w tym pliku można zmienić po uruchomieniu roli użytkownika.

Aby umożliwić przechowywanie różnych wartości tych ustawień sposobu działania roli użytkownika, możesz mieć kilka konfiguracji usługi. Za pomocą konfiguracji innej usłudze w przypadku każdego środowiska wdrażania. Na przykład można ustawić ciąg połączenia konta miejsca do magazynowania, tak aby emulatora lokalne przechowywanie Azure za pomocą w konfiguracji lokalnej usługi i tworzenie Konfiguracja usługi innej użyć Azure magazynu w chmurze.

Po utworzeniu nowej usługi w chmurze Azure w programie Visual Studio, domyślnie są tworzone dwa konfiguracje usługi. Tę konfigurację zostaną dodane do Azure projektu. Konfiguracje są nazywane:

- ServiceConfiguration.Cloud.cscfg

- ServiceConfiguration.Local.cscfg

## <a name="configure-an-azure-cloud-service"></a>Konfigurowanie usługi w chmurze Azure

Usługa w chmurze Azure w Eksploratorze rozwiązań można konfigurować w programie Visual Studio, jak pokazano na poniższej ilustracji.

![Konfigurowanie usługi w chmurze](./media/vs-azure-tools-configure-roles-for-cloud-service/IC713462.png)

### <a name="to-configure-an-azure-cloud-service"></a>Aby skonfigurować usługę Azure chmury

1. Aby skonfigurować poszczególnych ról w projekcie Azure w **Eksploratorze rozwiązanie**, otwórz menu skrótów dla roli Azure projektu, a następnie wybierz **Właściwości**.

    W Edytorze Visual Studio zostanie wyświetlona strona z nazwą roli. Strony są wyświetlane pola na karcie **Konfiguracja** .

1. Na liście **Konfiguracja usługi** wybierz nazwę konfiguracji usługi, którą chcesz edytować.

    Jeśli chcesz wprowadzić zmiany do wszystkich konfiguracje usługi dla tej roli, możesz wybrać **Wszystkich konfiguracji**.

    >[AZURE.IMPORTANT] Jeśli wybierzesz konfiguracji określonej usługi, niektóre właściwości są wyłączone, ponieważ ta osoba można ustawić tylko dla wszystkich konfiguracji. Aby edytować te właściwości, możesz wybrać wszystkich konfiguracji.

    Teraz możesz wybrać kartę, aby zaktualizować wszystkie właściwości enabled tego widoku.

## <a name="change-the-number-of-role-instances"></a>Zmienianie liczby wystąpień roli

Aby zwiększyć wydajność usługi w chmurze, możesz zmienić liczbę wystąpień roli, które są uruchomione na podstawie liczby użytkowników lub obciążenia dla określonej roli. Oddzielne maszyny wirtualnej powoduje utworzenie dla każdego wystąpienia roli działa usługa w chmurze w Azure. Wpłynie to rozliczeń wdrażania usługi w chmurze. Aby uzyskać więcej informacji na temat rozliczeń zobacz [Opis rachunku dla platformy Microsoft Azure](billing/billing-understand-your-bill.md).

### <a name="to-change-the-number-of-instances-for-a-role"></a>Aby zmienić liczbę wystąpień dla roli

1. Wybierz kartę **Konfiguracja** .

1. Na liście **Konfiguracja usługi** wybierz pozycję Konfiguracja usługi, którą chcesz zaktualizować.

    >[AZURE.NOTE] Można ustawić licznik wystąpień konfiguracji usługi określonych lub wszystkich konfiguracji usługi.

1. W polu tekstowym **wystąpienie liczba** wprowadź liczbę wystąpień, które chcesz uruchomić dla tej roli.

    >[AZURE.NOTE] Każde wystąpienie działa na osobne maszyny wirtualnej podczas publikowania usługi w chmurze Azure.

1. Wybierz przycisk **Zapisz** na pasku narzędzi, aby zapisać zmiany w pliku konfiguracji usługi.

## <a name="manage-connection-strings-for-storage-accounts"></a>Zarządzanie parametry połączenia dla kont miejsca do magazynowania

Można dodawać, usuwanie lub modyfikowanie parametry połączenia dla konfiguracji usługi. W przypadku konfiguracji innej usługi może być innymi ciągami połączenia. Na przykład może być parametry połączenia lokalnego konfiguracji lokalna usługa, która ma wartość `UseDevelopmentStorage=true`. Można także konfigurację usługi cloud, która korzysta z konta magazynu platformy Azure.

>[AZURE.WARNING] Po wprowadzeniu Azure magazynowania najważniejszych informacji o koncie dla parametrów połączenia konta miejsca do magazynowania, te informacje są przechowywane lokalnie w pliku konfiguracji usługi. Jednak tych informacji obecnie nie znajduje się jako zaszyfrowany tekst.

Korzystając z inną wartość dla każdej konfiguracji usług, nie ma używać innymi ciągami połączenia w usłudze w chmurze lub modyfikować kodu podczas publikowania usługi w chmurze Azure. Za pomocą tej samej nazwie w ciągu połączenia w kodzie, a wartość może się różnić, na podstawie konfiguracji usługę wybranego podczas tworzenia usługi w chmurze lub podczas jego publikowania.

### <a name="to-manage-connection-strings-for-storage-accounts"></a>Aby zarządzać parametry połączenia dla kont miejsca do magazynowania

1. Wybierz kartę **Ustawienia** .

1. Na liście **Konfiguracja usługi** wybierz pozycję Konfiguracja usługi, którą chcesz zaktualizować.

    >[AZURE.NOTE] Możesz zaktualizować parametry połączenia dla konfiguracji określonej usługi, ale jeśli chcesz dodać lub usunąć parametry połączenia po zaznaczeniu wszystkich konfiguracji.

1. Aby dodać parametry połączenia, wybierz przycisk **Dodaj ustawienie** . Nowy wpis jest dodawany do listy.

1. W polu tekstowym **Nazwa** wpisz nazwę, której chcesz użyć dla parametrów połączenia.

1. Na liście rozwijanej **Typ** wybierz pozycję **Parametry połączenia**.

1. Aby zmienić wartość w polu Parametry połączenia, wybierz przycisk wielokropka (...). Zostanie wyświetlone okno dialogowe **Tworzenie parametrów połączenia miejsca do magazynowania** .

1. Aby użyć emulatora konta magazynu lokalnego, wybierz opcję **emulatora magazynu platformy Microsoft Azure** , a następnie wybierz przycisk **OK** .

1. Za pomocą konta magazynu platformy Azure, przycisk opcji **subskrypcji** i wybierz przycisk konta odpowiednie miejsca do magazynowania.

1. Aby użyć niestandardowej poświadczeń, kliknij przycisk Opcje **ręcznie wprowadzić poświadczenia** . Wprowadź nazwę konta magazynu i klucz podstawowy lub drugiego. Aby uzyskać informacje na temat Tworzenie konta miejsca do magazynowania i wprowadź szczegóły konta miejsca do magazynowania w oknie dialogowym **Tworzenie parametrów połączenia miejsca do magazynowania** zobacz [Przygotowanie do publikowania i wdrożyć aplikację Azure z programu Visual Studio](vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md).

1. Aby usunąć parametry połączenia, wybierz parametry połączenia, a następnie wybierz przycisk **Usuń ustawienia** .

1. Wybierz ikonę **Zapisz** na pasku narzędzi, aby zapisać zmiany w pliku konfiguracji usługi.

1. Aby wyświetlić parametry połączenia w pliku konfiguracji usługi, należy najpierw uzyskać wartość ustawienia konfiguracji. Poniższy kod przedstawia przykład, w której jest tworzona magazyn obiektów blob i dane przekazane przy użyciu parametrów połączenia `MyConnectionString` z pliku konfiguracji usługi, gdy użytkownik wybierze **Button1** na stronie Default.aspx w roli sieci web dla usługi w chmurze Azure. Dodaj następujący przy użyciu instrukcji w celu Default.aspx.cs:

    ```
    using Microsoft.WindowsAzure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.ServiceRuntime;
    ```

1. Otwórz Default.aspx.cs w widoku projektu, a następnie dodać przycisk z przybornika. Dodaj następujący kod do `Button1_Click` metody. Ten kod zawiera `GetConfigurationSettingValue` uzyskiwania wartość z pliku konfiguracji usługi dla parametrów połączenia. Następnie obiektów blob jest tworzony na koncie miejsca do magazynowania, do których są zawarte w parametrach połączenia `MyConnectionString` i na końcu program dodaje tekstu do obiektu blob.

    ```
    protected void Button1_Click(object sender, EventArgs e)
    {
        // Setup the connection to Azure Storage
        var storageAccount = CloudStorageAccount.Parse(RoleEnvironment.GetConfigurationSettingValue("MyConnectionString"));
        var blobClient = storageAccount.CreateCloudBlobClient();
        // Get and create the container
        var blobContainer = blobClient.GetContainerReference("quicklap");
        blobContainer.CreateIfNotExists();
        // upload a text blob
        var blob = blobContainer.GetBlockBlobReference(Guid.NewGuid().ToString());
        blob.UploadText("Hello Azure");

    }
    ```

## <a name="add-custom-settings-to-use-in-your-azure-cloud-service"></a>Dodawanie ustawień niestandardowych do użycia w usłudze Azure chmury

Niestandardowe ustawienia pliku konfiguracji usługi umożliwiają dodawanie nazwy i wartości ciągu konfiguracji określonej usługi. Można to ustawienie służy do skonfigurowania funkcji w usłudze w chmurze czytając wartość ustawienia i kontrolowanie reguł w kodzie za pomocą tej wartości. Możesz zmienić te wartości konfiguracji usługi bez konieczności odbudowanie pakietu usługi lub gdy jest uruchomiona usługa w chmurze. Powiadomienia o kodzie można sprawdzić podczas zmiany ustawień. Zobacz [Zdarzenia RoleEnvironment.Changing](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changing.aspx).

Można dodawać, usuwanie lub modyfikowanie ustawień niestandardowych dla usług konfiguracji. Warto różne wartości dla tych ciągów dla różnych usług konfiguracji.

Przy użyciu inną wartość dla każdej konfiguracji usługi, nie trzeba używać różnych ciągów w usłudze w chmurze ani modyfikować kodu, podczas publikowania usługi w chmurze Azure. Za pomocą tej samej nazwie w ciągu w kodzie, a wartość może się różnić, na podstawie konfiguracji usługę wybranego podczas tworzenia usługi w chmurze lub podczas jego publikowania.

### <a name="to-add-custom-settings-to-use-in-your-azure-cloud-service"></a>Aby dodać niestandardowe ustawienia mają być używane w usłudze Azure chmury

1. Wybierz kartę **Ustawienia** .

1. Na liście **Konfiguracja usługi** wybierz pozycję Konfiguracja usługi, którą chcesz zaktualizować.

    >[AZURE.NOTE] Możesz zaktualizować ciągów konfiguracji określonej usługi, ale jeśli chcesz dodać lub usunąć ciągu, należy zaznaczyć **Wszystkich konfiguracji**.

1. Aby dodać ciąg, kliknij przycisk **Dodaj ustawienie** . Nowy wpis jest dodawany do listy.

1. W polu tekstowym **Nazwa** wpisz nazwę, której chcesz użyć ciągu.

1. Na liście rozwijanej **Typ** wybierz pozycję **ciągu**.

1. Aby dodać lub zmienić wartość ciągu, w polu **wartość** wpisz nową wartość.

1. Aby usunąć ciągu, wybierz ciąg, a następnie wybierz przycisk **Usuń ustawienia** .

1. Wybierz przycisk **Zapisz** na pasku narzędzi, aby zapisać zmiany w pliku konfiguracji usługi.

1. Aby uzyskać dostęp do ciągu w pliku konfiguracji usługi, należy najpierw uzyskać wartość ustawienia konfiguracji.

    Musisz upewnij się, że następujące przy użyciu instrukcji zostały już dodane do Default.aspx.cs, tak jak w poprzedniej procedurze.

    ```
    using Microsoft.WindowsAzure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.ServiceRuntime;
    ```

1. Dodaj następujący kod do `Button1_Click` metody, aby uzyskać dostęp do tego ciągu w taki sam sposób uzyskiwania dostępu do parametrów połączenia. Kod można wykonywać kodu określonych na podstawie wartości ciągu ustawienia pliku konfiguracji usługi, który jest używany.

    ```
    var settingValue = RoleEnvironment.GetConfigurationSettingValue("MySetting");
    if (settingValue == “ThisValue”)
    {
    // Perform these lines of code
    }
    ```

## <a name="manage-local-storage-for-each-role-instance"></a>Zarządzanie magazynu lokalnego dla każdego wystąpienia roli

Możesz dodać plik lokalny system miejsca do magazynowania dla każdego wystąpienia roli. Mogą zawierać lokalne dane poniżej nie muszą być dostępne dla innych ról. Wszystkie dane, które nie trzeba zapisać w tabeli, obiektów blob lub przechowywania bazy danych SQL mogą być przechowywane w tym miejscu. Na przykład może użyć tego magazynu lokalnego do danych z pamięci podręcznej, który może być konieczne może być używany ponownie. Ten przechowywane dane nie są dostępne dla innych wystąpień roli. 

Ustawienia lokalne przechowywanie dotyczą wszystkich konfiguracji usługi. Można tylko dodawanie, usuwanie lub modyfikowanie magazynu lokalnego dla wszystkich konfiguracji usługi.

### <a name="to-manage-local-storage-for-each-role-instance"></a>Aby zarządzać magazynu lokalnego dla każdego wystąpienia roli

1. Wybierz kartę **Magazynu lokalnego** .

1. Na liście **Konfiguracja usługi** wybierz **Wszystkich konfiguracji**.

1. Aby dodać wpis magazynu lokalnego, kliknij przycisk **Dodaj magazynu lokalnego** . Nowy wpis jest dodawany do listy.

1. W polu tekstowym **Nazwa** wpisz nazwę, której chcesz użyć dla tego magazynu lokalnego.

1. W polu tekstowym **rozmiar** wpisz rozmiar w MB, które są potrzebne do tego magazynu lokalnego.

1. Aby usunąć dane w tym magazynu lokalnego, gdy maszyny wirtualnej do tej roli zostanie odtworzony, zaznacz pole wyboru **Odtwórz Oczyszczanie roli** .

1. Aby edytować istniejący wpis magazynu lokalnego, wybierz pozycję Wiersz, który chcesz zaktualizować. Następnie można edytować pól, zgodnie z opisem w poprzednich krokach.

1. Aby usunąć wpis magazynu lokalnego, wybrać pozycję miejsca do magazynowania na liście, a następnie wybierz przycisk **Usuń magazynu lokalnego** .

1. Aby zapisać zmiany w pliku konfiguracji usługi, wybierz ikonę **Zapisz** na pasku narzędzi.

1. Aby uzyskać dostęp do magazynu lokalnego dodanego w pliku konfiguracji usługi, należy najpierw uzyskać wartość ustawienia konfiguracji zasobów lokalnych. Następujące wiersze kodu umożliwia dostęp do tej wartości Tworzenie pliku o nazwie **MyStorageTest.txt** i zapisywać wiersza testowymi danymi do tego pliku. Możesz dodać go w `Button_Click` metodę używane w poprzedniej procedurze:

1. Musisz upewnij się, że następujące przy użyciu instrukcji zostaną dodane do Default.aspx.cs:

    ```
    using System.IO;
    using System.Text;
    ```

1. Dodaj następujący kod do `Button1_Click` metody. To powoduje utworzenie pliku w magazynie lokalnym i zapisuje test dane do tego pliku.

    ```
    // Retrieve an object that points to the local storage resource
    LocalResource localResource = RoleEnvironment.GetLocalResource("LocalStorage1");

    //Define the file name and path
    string[] paths = { localResource.RootPath, "MyStorageTest.txt" };
    String filePath = Path.Combine(paths);

    using (FileStream writeStream = File.Create(filePath))
    {
          Byte[] textToWrite = new UTF8Encoding(true).GetBytes("Testing Web role storage");
          writeStream.Write(textToWrite, 0, textToWrite.Length);
    }
    ```

1. (Opcjonalnie) Aby wyświetlić ten plik, który został utworzony podczas korzystania z usługi w chmurze lokalnie, wykonaj następujące czynności:

  1. Uruchom ról w sieci web i wybierz **Button1** , aby upewnić się, że kod wewnątrz `Button1_Click` jest wywoływana.

  1. W obszarze powiadomień Otwórz menu skrótów dla ikony Azure i wybierz polecenie **Pokaż obliczyć emulatora elementy interfejsu użytkownika**. Zostanie wyświetlone okno dialogowe **Emulatora obliczyć Azure** .

  1. Wybierz rolę, sieci web.

  1. Na pasku menu wybierz pozycję **Narzędzia**, **Otwórz magazynu lokalnego**. Zostanie wyświetlone okno Eksploratora Windows.

  1. Na pasku menu wprowadź **MyStorageTest.txt** w polu tekstowym **Wyszukaj** , a następnie wybierz pozycję **Enter** , aby uruchomić wyszukiwanie.

    Plik jest wyświetlany w wynikach wyszukiwania.

  1. Aby wyświetlić zawartość pliku, otwórz menu skrótów dla pliku i wybierz pozycję **Otwórz**.

## <a name="collect-cloud-service-diagnostics"></a>Zbieranie diagnostyki usługi w chmurze

Może zbierać dane diagnostyki tej usługi Azure chmury. Ten dane zostaną dodane do konta miejsca do magazynowania. W przypadku konfiguracji innej usługi może być innymi ciągami połączenia. Na przykład może być konta lokalnego magazynu konfiguracji lokalna usługa, która ma wartość UseDevelopmentStorage = true. Można także konfigurację usługi cloud, która korzysta z konta magazynu platformy Azure. Aby uzyskać więcej informacji o diagnostyce Azure Zobacz zbieranie rejestrowania danych przy użyciu narzędzia diagnostyczne Azure.

>[AZURE.NOTE] Konfiguracja usługi lokalnej jest już skonfigurowany do używania zasobów lokalnych. Użycie Konfiguracja usługi w chmurze do publikowania usługi Azure chmury, chyba że określono parametry połączenia parametry połączenia, które można określić podczas publikowania również służy do diagnostyki parametry połączenia. Jeśli pakiet do usługi w chmurze przy użyciu programu Visual Studio, parametry połączenia w konfiguracji usługi nie ulega zmianie.

### <a name="to-collect-cloud-service-diagnostics"></a>Zbieranie diagnostyki usługi w chmurze

1. Wybierz kartę **Konfiguracja** .

1. Na liście **Konfiguracja usługi** wybierz pozycję Konfiguracja usługi, którą chcesz zaktualizować, lub wybierz **Wszystkich konfiguracji**.

    >[AZURE.NOTE] Możesz zaktualizować konta miejsca do magazynowania dla konfiguracji określonej usługi, ale jeśli chcesz włączyć lub wyłączyć diagnostyki należy wybrać wszystkich konfiguracji.

1. Aby włączyć diagnostyki, zaznacz pole wyboru **Włącz diagnostyki** .

1. Aby zmienić wartość w polu Konto miejsca do magazynowania, kliknij przycisk wielokropka (...).

    Zostanie wyświetlone okno dialogowe **Tworzenie parametrów połączenia miejsca do magazynowania** .

1. Aby użyć parametrów połączenia lokalnego, wybierz opcję emulatora Azure miejsca do magazynowania, a następnie wybierz przycisk **OK** .

1. Za pomocą konta magazynu skojarzonego z subskrypcją usługi Azure, wybierz opcję **subskrypcji** .

1. Za pomocą konta miejsca do magazynowania dla parametrów połączenia lokalnego, wybierz opcję **ręcznie wprowadzić poświadczenia** .

    Aby uzyskać więcej informacji na temat utworzyć konto miejsca do magazynowania i wprowadź szczegóły konta miejsca do magazynowania w oknie dialogowym **Tworzenie parametrów połączenia miejsca do magazynowania** zobacz [Przygotowanie do publikowania i wdrożyć aplikację Azure z programu Visual Studio](vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md).

1. Wybierz konto miejsca do magazynowania, który ma być używany w polu **Nazwa konta**.

    W przypadku ręcznego wprowadzania magazynu poświadczeń konta, skopiuj lub wpisz klucza podstawowego w polu **klucz konta**. Ten klucz może zostać skopiowany z [portal Azure klasyczny](http://go.microsoft.com/fwlink/?LinkID=213885). Aby skopiować ten klucz, wykonując poniższe czynności w widoku **Kont miejsca do magazynowania** w [portal Azure klasyczny](http://go.microsoft.com/fwlink/?LinkID=213885):
    
  1. Wybierz konto miejsca do magazynowania, który ma być używany dla usługi w chmurze.

  1. Kliknij przycisk **Zarządzaj klawiszy dostępu** znajduje się w dolnej części ekranu. Zostanie wyświetlone okno dialogowe **Zarządzanie klawiszy dostępu** .

  1. Aby skopiować kod dostępu, kliknij przycisk **Kopiuj do Schowka** . Można teraz wkleić ten klucz w polu **klucz konta** .

1. Aby użyć konta miejsca do magazynowania, które podasz, jako parametry połączenia dla diagnostyki (i pamięci podręcznej) podczas publikowania usługi w chmurze Azure, zaznacz pole wyboru **Aktualizuj opracowywania miejsca do magazynowania parametry połączenia dla narzędzia diagnostyczne i buforowanie Azure przechowywania poświadczeń podczas publikowania Azure konta** .

1. Wybierz przycisk **Zapisz** na pasku narzędzi, aby zapisać zmiany w pliku konfiguracji usługi.

## <a name="change-the-size-of-the-virtual-machine-used-for-each-role"></a>Zmienianie rozmiaru maszyny wirtualnej używane dla poszczególnych ról

Można także ustawić rozmiar maszyn wirtualnych dla poszczególnych ról. Można ustawić tylko tego rozmiaru wszystkich konfiguracji usługi. Jeśli wybierzesz mniejszy rozmiar komputera, następnie mniej rdzenie Procesora, pamięci i dysku miejsca do magazynowania przydzielonego. Przydzielonej przepustowości również jest mniejszy. Aby uzyskać więcej informacji na temat rozmiary i zasoby przydzielone zobacz [rozmiarów usług w chmurze](cloud-services/cloud-services-sizes-specs.md).

Zasoby wymagane dla każdej maszyny wirtualnej platformy Azure wpływa na koszt uruchamiania usługi w chmurze w Azure. Aby uzyskać więcej informacji na temat rozliczeń Azure zobacz [Opis rachunku dla platformy Microsoft Azure](billing/billing-understand-your-bill.md).

### <a name="to-change-the-size-of-the-virtual-machine"></a>Aby zmienić rozmiar maszyny wirtualnej

1. Wybierz kartę **Konfiguracja** .

1. Na liście **Konfiguracja usługi** wybierz **Wszystkich konfiguracji**.

1. Aby wybrać odpowiedni rozmiar maszyny wirtualnej dla tej roli, wybierz odpowiedni rozmiar z listy **rozmiar pamięci Wirtualnej** .

1. Wybierz przycisk **Zapisz** na pasku narzędzi, aby zapisać zmiany w pliku konfiguracji usługi.

## <a name="manage-endpoints-and-certificates-for-your-roles"></a>Zarządzanie punkty końcowe i certyfikaty dla poszczególnych ról

Można skonfigurować sieci punkty końcowe, określając protokołu, numer portu, a dla HTTPS, informacje na temat certyfikatu SSL. Wersjach przed czerwca 2012 obsługi protokołu HTTP, HTTPS i TCP. Wersja czerwca 2012 obsługuje te protokoły i UDP. Nie można użyć UDP wprowadzania punkty końcowe w emulatorze obliczeń. Za pomocą protokołu tylko dla wewnętrznych punktów końcowych.

Aby zwiększyć bezpieczeństwo usługi Azure chmurze, możesz utworzyć punkty końcowe, które korzystają z protokołu HTTPS. Na przykład jeśli jest używany przez klientów do zamówienia zakupu usługi w chmurze, chcesz upewnij się, że ich informacje są bezpieczne przy użyciu protokołu SSL.

Można również dodać punkty końcowe, które mogą być używane wewnętrznie lub zewnętrznie. Zewnętrzne punkty końcowe są nazywane wprowadzania punktów końcowych. Punkt końcowy wprowadzania umożliwia innym punkt dostępu użytkowników do usługi w chmurze. Jeśli masz Usługa WCF może chcesz udostępnić punkt końcowy wewnętrznych dla ról w sieci web za pomocą korzystać z tej usługi.

>[AZURE.IMPORTANT] Można aktualizować tylko punkty końcowe dla wszystkich konfiguracji usługi.

Jeśli dodasz punkty końcowe HTTPS, należy użyć certyfikat SSL. W tym celu można skojarzyć certyfikaty z roli użytkownika w przypadku wszystkich usług konfiguracji i skorzystaj z poniższych dla punktów końcowych.

>[AZURE.IMPORTANT] Te certyfikaty nie są dostarczane z tej usługi. Własnych certyfikatów należy przekazać oddzielnie Azure za pomocą [portal Azure klasyczny](http://go.microsoft.com/fwlink/?LinkID=213885).

Jakichkolwiek certyfikatów zarządzania, które można skojarzyć z konfiguracji usługi zastosowanie tylko wtedy, gdy działa usługa w chmurze w Azure. Po uruchomieniu usługi w chmurze w środowisku lokalnym programowania to standardowy certyfikat zarządza emulatora Azure obliczeń jest używana.

### <a name="to-add-a-certificate-to-a-role"></a>Dodawanie certyfikatu do roli

1. Wybierz kartę **Certyfikaty** .

1. Na liście **Konfiguracja usługi** wybierz **Wszystkich konfiguracji**.

    >[AZURE.NOTE] Aby dodać lub usunąć certyfikaty, możesz wybrać wszystkich konfiguracji. W razie potrzeby możesz zaktualizować nazwę i odcisku palca konfiguracji określonej usługi.

1. Aby dodać certyfikatu do tej roli, kliknij przycisk **Dodaj certyfikat** . Nowy wpis jest dodawany do listy.

1. W polu tekstowym **Nazwa** wprowadź nazwę certyfikatu.

1. Na liście **Lokalizacji przechowywania** wybierz lokalizację certyfikat, który chcesz dodać.

1. Na liście **Nazwa magazynu** wybierz magazyn, którego chcesz użyć do wybierz certyfikat.

1. Aby dodać certyfikat, kliknij przycisk wielokropka (...). Zostanie wyświetlone okno dialogowe **Zabezpieczenia systemu Windows** .

1. Wybierz certyfikat, który chcesz użyć na liście, a następnie wybierz przycisk **OK** .

    >[AZURE.NOTE] Po dodaniu certyfikatu z magazynu certyfikatów wszelkie pośrednie certyfikaty są automatycznie dodawane ustawienia konfiguracji. Aby można było poprawnie skonfigurować usługi dla protokołu SSL, również można przekazać te certyfikaty pośrednie Azure.

1. Aby usunąć certyfikatu, wybierz certyfikat, a następnie wybierz przycisk **Usuń certyfikat** .

1. Wybierz ikonę **Zapisz** na pasku narzędzi, aby zapisać zmiany w pliku konfiguracji usługi.

### <a name="to-manage-endpoints-for-a-role"></a>Aby zarządzać punkty końcowe dla roli

1. Wybierz kartę **punktów końcowych** .

1. Na liście **Konfiguracja usługi** wybierz **Wszystkich konfiguracji**.

1. Aby dodać punktu końcowego, kliknij przycisk **Dodaj punkt końcowy** . Nowy wpis jest dodawany do listy.

1. W polu tekstowym **Nazwa** wpisz nazwę, której chcesz użyć dla tego punktu końcowego.

1. Wybierz typ punktu końcowego, należy na liście **Typ** .

1. Wybierz protokół dla punktu końcowego, należy na liście **Protocol (protokół)** .

1. Jeśli jest punktu końcowego wprowadzania danych w polu tekstowym **Portu publicznej** wprowadź publicznej portu używać.

1. W polu tekstowym **Portu prywatne** wpisz prywatne portu, aby użyć.

1. Jeśli punkt końcowy wymaga protokołu https, na liście **Nazwa certyfikatu SSL** wybierz certyfikat.

    >[AZURE.NOTE] Ta lista zawiera certyfikatów, które zostały dodane do tej roli w kartę **Certyfikaty** .

1. Wybierz przycisk **Zapisz** na pasku narzędzi, aby zapisać zmiany w pliku konfiguracji usługi.

## <a name="next-steps"></a>Następne kroki
Dowiedz się więcej o Azure projekty w programie Visual Studio, czytając [Konfigurowanie projektu Azure](vs-azure-tools-configuring-an-azure-project.md). Dowiedz się więcej o schematu usługi w chmurze, czytając [Schemacie](https://msdn.microsoft.com/library/azure/dd179398).
