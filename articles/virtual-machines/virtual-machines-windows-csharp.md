<properties
    pageTitle="Wdrażanie zasobów Azure za pomocą C# | Microsoft Azure"
    description="Dowiedz się, jak utworzyć zasoby Microsoft Azure za pomocą C# i Azure Menedżera zasobów."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/06/2016"
    ms.author="davidmu"/>

# <a name="deploy-azure-resources-using-c"></a>Wdrażanie zasobów Azure za pomocą C# 

W tym artykule pokazano, jak utworzyć zasoby Azure za pomocą C#.

Musisz najpierw upewnij się, że zostało zakończone następujących zadań:

- Instalowanie [programu Visual Studio](http://msdn.microsoft.com/library/dd831853.aspx)
- Weryfikacja instalacji [systemu Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) lub [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855)
- Pobierz [token uwierzytelniania](../resource-group-authenticate-service-principal.md)

Trwa około 30 minut wykonaj następujące kroki.

## <a name="step-1-create-a-visual-studio-project-and-install-the-libraries"></a>Krok 1: Tworzenie projektu programu Visual Studio i zainstalować bibliotek

Pakiety NuGet są najprostszym sposobem instalowania bibliotek, potrzebnymi do zakończenia tego samouczka. Aby uzyskać bibliotek, które są potrzebne w programie Visual Studio, wykonaj następujące czynności:

1. Kliknij pozycję **plik** > **Nowy** > **projektu**.

2. W **szablonach** > **Visual C#**, wybierz **Aplikację konsoli**, wprowadź nazwę i lokalizację projektu, a następnie kliknij **przycisk OK**.

3. Kliknij prawym przyciskiem myszy nazwę projektu w Eksploratorze rozwiązań, a następnie kliknij **Zarządzanie pakietów NuGet**.

4. Typ *Usługi Active Directory* w polu wyszukiwania, kliknij przycisk **Zainstaluj** pakietu Active Directory Authentication Library, a następnie postępuj zgodnie z instrukcjami, aby zainstalować pakiet.

5. W górnej części strony wybierz pozycję **Obejmują wstępną**. Typ *Microsoft.Azure.Management.Compute* w polu wyszukiwania, kliknij przycisk **Zainstaluj** dla bibliotek .NET obliczenia, a następnie postępuj zgodnie z instrukcjami, aby zainstalować pakiet.

6. Typ *Microsoft.Azure.Management.Network* w polu wyszukiwania, kliknij przycisk **Zainstaluj** dla bibliotek .NET sieci, a następnie postępuj zgodnie z instrukcjami, aby zainstalować pakiet.

7. Typ *Microsoft.Azure.Management.Storage* w polu wyszukiwania, kliknij przycisk **Zainstaluj** dla bibliotek .NET miejsca do magazynowania, a następnie postępuj zgodnie z instrukcjami, aby zainstalować pakiet.

8. W polu wyszukiwania wpisz *Microsoft.Azure.Management.ResourceManager* , kliknij przycisk **Zainstaluj** dla bibliotek, zarządzania zasobów.

Teraz możesz rozpocząć tworzenie aplikacji przy użyciu bibliotek.

## <a name="step-2-create-the-credentials-that-are-used-to-authenticate-requests"></a>Krok 2: Tworzenie poświadczeń, które służą do uwierzytelniania żądania

Teraz możesz formatować wcześniej utworzony do poświadczeń używanych do uwierzytelniania żądań do Menedżera zasobów Azure informacji o aplikacji.

1. Otwórz plik Plik Program.cs dla projektu, który został utworzony, a następnie dodaj tych przy użyciu instrukcji na początek pliku:

        using Microsoft.Azure;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Azure.Management.ResourceManager;
        using Microsoft.Azure.Management.ResourceManager.Models;
        using Microsoft.Azure.Management.Storage;
        using Microsoft.Azure.Management.Storage.Models;
        using Microsoft.Azure.Management.Network;
        using Microsoft.Azure.Management.Network.Models;
        using Microsoft.Azure.Management.Compute;
        using Microsoft.Azure.Management.Compute.Models;
        using Microsoft.Rest;

2. Aby utworzyć token, które są potrzebne, Dodaj tej metody można użyć do klasy programu:

        private static async Task<AuthenticationResult> GetAccessTokenAsync()
        {
          var cc = new ClientCredential("{client-id}", "{client-secret}");
          var context = new AuthenticationContext("https://login.windows.net/{tenant-id}");
          var token = await context.AcquireTokenAsync("https://management.azure.com/", cc);
          if (token == null)
          {
            throw new InvalidOperationException("Could not get the token");
          }
          return token;
        }

    Zamień {identyfikator klienta} identyfikator usługi Azure Active Directory aplikacji, {klienta tajny} przy użyciu klucza dostępu AD aplikacji i {dzierżawy identyfikator} identyfikatorem dzierżawy dla subskrypcji. Identyfikator dzierżawy można znaleźć, uruchamiając Get-AzureRmSubscription. Klawisz dostępu można znaleźć za pomocą portalu Azure.

3. Aby zadzwonić do metodę, która wcześniej zostały dodane, Dodaj ten kod metody głównego w pliku Plik Program.cs:

        var token = GetAccessTokenAsync();
        var credential = new TokenCredentials(token.Result.AccessToken);

4. Zapisz plik Plik Program.cs.

## <a name="step-3-register-the-resource-providers-and-create-the-resources"></a>Krok 3: Rejestrowanie dostawców zasobów i utworzyć zasoby

### <a name="register-the-providers-and-create-a-resource-group"></a>Zarejestruj dostawców i utworzyć grupę zasobów

Wszystkie zasoby muszą znajdować się w grupie zasobów. Zanim zasobów można dodać do grupy, Twoja subskrypcja musi być zarejestrowany z dostawcami zasobów.

1. Dodać zmienne metody główne klasy programu określone nazwy, które mają zostać zastosowana do zasobów:

        var groupName = "resource group name";
        var subscriptionId = "subsciption id";
        var location = "location name";
        var storageName = "storage account name";
        var ipName = "public ip name";
        var subnetName = "subnet name";
        var vnetName = "virtual network name";
        var nicName = "network interface name";
        var avSetName = "availability set name";
        var vmName = "virtual machine name";  
        var adminName = "administrator account name";
        var adminPassword = "administrator account password";
        
    Zamienianie wartości zmiennych z nazwami i identyfikator, który ma być używany. Identyfikator subskrypcji można znaleźć, uruchamiając Get-AzureRmSubscription.

2. Aby utworzyć grupę zasobów i zarejestrować dostawców, należy dodać tej metody można użyć do klasy programu:

        public static async Task<ResourceGroup> CreateResourceGroupAsync(
          TokenCredentials credential,
          string groupName,
          string subscriptionId,
          string location)
        {
          var resourceManagementClient = new ResourceManagementClient(credential)
            { SubscriptionId = subscriptionId };
            
          Console.WriteLine("Registering the providers...");
          var rpResult = resourceManagementClient.Providers.Register("Microsoft.Storage");
          Console.WriteLine(rpResult.RegistrationState);
          rpResult = resourceManagementClient.Providers.Register("Microsoft.Network");
          Console.WriteLine(rpResult.RegistrationState);
          rpResult = resourceManagementClient.Providers.Register("Microsoft.Compute");
          Console.WriteLine(rpResult.RegistrationState);
          
          Console.WriteLine("Creating the resource group...");
          var resourceGroup = new ResourceGroup { Location = location };
          return await resourceManagementClient.ResourceGroups.CreateOrUpdateAsync(groupName, resourceGroup);
        }

3. Aby zadzwonić do metodę, która wcześniej zostały dodane, Dodaj ten kod metody główne:

        var rgResult = CreateResourceGroupAsync(
          credential,
          groupName,
          subscriptionId,
          location);
        Console.WriteLine(rgResult.Result.Properties.ProvisioningState);
        Console.ReadLine();

### <a name="create-a-storage-account"></a>Tworzenie konta miejsca do magazynowania

[Konto miejsca do magazynowania](../storage/storage-create-storage-account.md) jest potrzebny do zapisania pliku wirtualny dysk twardy jest tworzony dla maszyny wirtualnej.

1. Aby utworzyć konto miejsca do magazynowania, należy dodać tej metody można użyć do klasy programu:

        public static async Task<StorageAccount> CreateStorageAccountAsync(
          TokenCredentials credential,       
          string groupName,
          string subscriptionId,
          string location,
          string storageName)
        {
          Console.WriteLine("Creating the storage account...");
          var storageManagementClient = new StorageManagementClient(credential)
            { SubscriptionId = subscriptionId };
          return await storageManagementClient.StorageAccounts.CreateAsync(
            groupName,
            storageName,
            new StorageAccountCreateParameters()
            {
              Sku = new Microsoft.Azure.Management.Storage.Models.Sku() 
                { Name = SkuName.StandardLRS},
              Kind = Kind.Storage,
              Location = location
            }
          );
        }

2. Aby zadzwonić do metodę, która wcześniej zostały dodane, Dodaj ten kod do metody główne klasy programu:

        var stResult = CreateStorageAccountAsync(
          credential,
          groupName,
          subscriptionId,
          location,
          storageName);
        Console.WriteLine(stResult.Result.ProvisioningState);  
        Console.ReadLine();

### <a name="create-the-public-ip-address"></a>Tworzenie publiczny adres IP

Publiczny adres IP jest potrzebny do komunikowania się z maszyny wirtualnej.

1. Aby utworzyć publiczny adres IP maszyny wirtualnej, należy dodać tej metody można użyć do klasy programu:

        public static async Task<PublicIPAddress> CreatePublicIPAddressAsync(
          TokenCredentials credential,  
          string groupName,
          string subscriptionId,
          string location,
          string ipName)
        {
          Console.WriteLine("Creating the public ip...");
          var networkManagementClient = new NetworkManagementClient(credential)
            { SubscriptionId = subscriptionId };
          return await networkManagementClient.PublicIPAddresses.CreateOrUpdateAsync(
            groupName,
            ipName,
            new PublicIPAddress
            {
              Location = location,
              PublicIPAllocationMethod = "Dynamic"
            }
          );
        }

2. Aby zadzwonić do metodę, która wcześniej zostały dodane, Dodaj ten kod do metody główne klasy programu:

        var ipResult = CreatePublicIPAddressAsync(
          credential,
          groupName,
          subscriptionId,
          location,
          ipName);
        Console.WriteLine(ipResult.Result.ProvisioningState);  
        Console.ReadLine();

### <a name="create-the-virtual-network"></a>Tworzenie wirtualnych sieci

Maszyny wirtualnej, która jest tworzone model wdrożenia Menedżera zasobów muszą znajdować się w wirtualnej sieci.

1. Aby utworzyć podsieć i wirtualnej sieci, należy dodać tej metody można użyć do klasy programu:

        public static async Task<VirtualNetwork> CreateVirtualNetworkAsync(
          TokenCredentials credential,
          string groupName,
          string subscriptionId,
          string location,
          string vnetName,
          string subnetName)
        {
          Console.WriteLine("Creating the virtual network...");
          var networkManagementClient = new NetworkManagementClient(credential)
            { SubscriptionId = subscriptionId };
          
          var subnet = new Subnet
          {
            Name = subnetName,
            AddressPrefix = "10.0.0.0/24"
          };
          
          var address = new AddressSpace {
            AddressPrefixes = new List<string> { "10.0.0.0/16" }
          };
          
          return await networkManagementClient.VirtualNetworks.CreateOrUpdateAsync(
            groupName,
            vnetName,
            new VirtualNetwork
            {
              Location = location,
              AddressSpace = address,
              Subnets = new List<Subnet> { subnet }
            }
          );
        }
        
2. Aby zadzwonić do metodę, która wcześniej zostały dodane, Dodaj ten kod do metody główne klasy programu:

        var vnResult = CreateVirtualNetworkAsync(
          credential,
          groupName,
          subscriptionId,
          location,
          vnetName,
          subnetName);
        Console.WriteLine(vnResult.Result.ProvisioningState);  
        Console.ReadLine();
        
### <a name="create-the-network-interface"></a>Tworzenie interfejsu sieciowego

Maszyny wirtualnej musi karty sieciowej można komunikować się w wirtualnej sieci.

1. Aby utworzyć karty sieciowej, należy dodać tej metody można użyć do klasy programu:

        public static async Task<NetworkInterface> CreateNetworkInterfaceAsync(
          TokenCredentials credential,
          string groupName,
          string subscriptionId,
          string location,
          string subnetName,
          string vnetName,
          string ipName,
          string nicName)
        {
          Console.WriteLine("Creating the network interface...");
          var networkManagementClient = new NetworkManagementClient(credential)
            { SubscriptionId = subscriptionId };
          var subnetResponse = await networkManagementClient.Subnets.GetAsync(
            groupName,
            vnetName,
            subnetName
          );
          var pubipResponse = await networkManagementClient.PublicIPAddresses.GetAsync(groupName, ipName);

          return await networkManagementClient.NetworkInterfaces.CreateOrUpdateAsync(
            groupName,
            nicName,
            new NetworkInterface
            {
              Location = location,
              IpConfigurations = new List<NetworkInterfaceIPConfiguration>
              {
                new NetworkInterfaceIPConfiguration
                {
                  Name = nicName,
                  PublicIPAddress = pubipResponse,
                  Subnet = subnetResponse
                }
              }
            }
          );
        }

2. Aby zadzwonić do metodę, która wcześniej zostały dodane, Dodaj ten kod do metody główne klasy programu:

        var ncResult = CreateNetworkInterfaceAsync(
          credential,
          groupName,
          subscriptionId,
          location,
          subnetName,
          vnetName,
          ipName,
          nicName);
        Console.WriteLine(ncResult.Result.ProvisioningState);  
        Console.ReadLine();

### <a name="create-an-availability-set"></a>Tworzenie zestawu dostępności

Zestawy dostępność ułatwiają zarządzanie konserwacji maszyn wirtualnych używane przez aplikację.

1. Aby utworzyć zestaw dostępność, Dodaj tej metody można użyć do klasy programu:

        public static async Task<AvailabilitySet> CreateAvailabilitySetAsync(
          TokenCredentials credential,
          string groupName,
          string subscriptionId,
          string location,
          string avsetName)
        {
          Console.WriteLine("Creating the availability set...");
          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          return await computeManagementClient.AvailabilitySets.CreateOrUpdateAsync(
            groupName,
            avsetName,
            new AvailabilitySet()
            {
              Location = location
            }
          );
        }

2. Aby zadzwonić do metodę, która wcześniej zostały dodane, Dodaj ten kod do metody główne klasy programu:

        var avResult = CreateAvailabilitySetAsync(
          credential,  
          groupName,
          subscriptionId,
          location,
          avSetName);
        Console.ReadLine();

### <a name="create-a-virtual-machine"></a>Tworzenie maszyny wirtualnej

Teraz, gdy utworzysz wszystkie zasoby pomocniczych, możesz utworzyć maszyny wirtualnej.

1. Aby utworzyć maszyny wirtualnej, należy dodać tej metody można użyć do klasy programu:

        public static async Task<VirtualMachine> CreateVirtualMachineAsync(
          TokenCredentials credential, 
          string groupName,
          string subscriptionId,
          string location,
          string nicName,
          string avsetName,
          string storageName,
          string adminName,
          string adminPassword,
          string vmName)
        {
          var networkManagementClient = new NetworkManagementClient(credential)
            { SubscriptionId = subscriptionId };
          var nic = networkManagementClient.NetworkInterfaces.Get(groupName, nicName);

          var computeManagementClient = new ComputeManagementClient(credential);
          computeManagementClient.SubscriptionId = subscriptionId;
          var avSet = computeManagementClient.AvailabilitySets.Get(groupName, avsetName);

          Console.WriteLine("Creating the virtual machine...");
          return await computeManagementClient.VirtualMachines.CreateOrUpdateAsync(
            groupName,
            vmName,
            new VirtualMachine
            {
              Location = location,
              AvailabilitySet = new Microsoft.Azure.Management.Compute.Models.SubResource
              {
                Id = avSet.Id
              },
              HardwareProfile = new HardwareProfile
              {
                VmSize = "Standard_A0"
              },
              OsProfile = new OSProfile
              {
                AdminUsername = adminName,
                AdminPassword = adminPassword,
                ComputerName = vmName,
                WindowsConfiguration = new WindowsConfiguration
                {
                  ProvisionVMAgent = true
                }
              },
              NetworkProfile = new NetworkProfile
              {
                NetworkInterfaces = new List<NetworkInterfaceReference>
                {
                  new NetworkInterfaceReference { Id = nic.Id }
                }
              },
              StorageProfile = new StorageProfile
              {
                ImageReference = new ImageReference
                {
                  Publisher = "MicrosoftWindowsServer",
                  Offer = "WindowsServer",
                  Sku = "2012-R2-Datacenter",
                  Version = "latest"
                },
                OsDisk = new OSDisk
                {
                  Name = "mytestod1",
                  CreateOption = DiskCreateOptionTypes.FromImage,
                  Vhd = new VirtualHardDisk
                  {
                    Uri = "http://" + storageName + ".blob.core.windows.net/vhds/mytestod1.vhd"
                  }
                }
              }
            }
          );
        }

    >[AZURE.NOTE] Ten samouczek tworzy maszyny wirtualnej wersją systemu operacyjnego Windows Server. Aby dowiedzieć się więcej o wybieraniu innych obrazów, zobacz [Nawigacja i wybierz pozycję Azure maszyn wirtualnych obrazów za pomocą programu Windows PowerShell i polecenie Azure](virtual-machines-linux-cli-ps-findimage.md).

2. Aby zadzwonić do metodę, która wcześniej zostały dodane, Dodaj ten kod metody główne:

        var vmResult = CreateVirtualMachineAsync(
          credential,
          groupName,
          subscriptionId,
          location,
          nicName,
          avSetName,
          storageName,
          adminName,
          adminPassword,
          vmName);
        Console.WriteLine(vmResult.Result.ProvisioningState);
        Console.ReadLine();

##<a name="step-4-delete-the-resources"></a>Krok 4: Usuwanie zasobów

Ponieważ jest naliczany dla zasobów używanych w Azure, zawsze jest dobrym rozwiązaniem jest usunięcie zasoby, które nie są już potrzebne. Jeśli chcesz usunąć maszyn wirtualnych i obsługi zasobów, wszystko, co trzeba zrobić jest usuwanie grupy zasobów.

1.  Aby usunąć grupę zasobów, należy dodać tej metody można użyć do klasy programu:

        public static async void DeleteResourceGroupAsync(
          TokenCredentials credential,
          string groupName,
          string subscriptionId)
        {
          Console.WriteLine("Deleting resource group...");
          var resourceManagementClient = new ResourceManagementClient(credential)
            { SubscriptionId = subscriptionId };
          await resourceManagementClient.ResourceGroups.DeleteAsync(groupName);
        }

2.  Aby zadzwonić do metodę, która wcześniej zostały dodane, Dodaj ten kod metody główne:

        DeleteResourceGroupAsync(
          credential,
          groupName,
          subscriptionId);
        Console.ReadLine();

## <a name="step-5-run-the-console-application"></a>Krok 5: Uruchomić aplikację konsoli

1. Aby uruchomić aplikację konsoli, kliknij przycisk **Start** , w programie Visual Studio, a następnie zaloguj się do Azure AD przy użyciu tę samą nazwę użytkownika i hasło, którego używasz z subskrypcji.

2. Po każdej kodu stanu są zwracane do tworzenia każdego zasobu, naciśnij klawisz **Enter** . Po utworzeniu maszyny wirtualnej następnego kroku przed naciskając klawisz Enter, aby usunąć wszystkie zasoby.

    Należy trwa około pięć minut dla tej aplikacji konsoli uruchomić całkowicie od początku do końca. Przed naciśnięciem klawisza Enter, aby uruchomić usuwania zasobów, może potrwać kilka minut, aby sprawdzić tworzenia zasobów w portalu Azure przed ich usuwanie.

3. Aby wyświetlić stan zasobów, przejdź do dzienników inspekcji w portalu Azure:

    ![Przeglądanie dzienników inspekcji w Azure portal](./media/virtual-machines-windows-csharp/crpportal.png)
    
## <a name="next-steps"></a>Następne kroki

- Korzystać z przy użyciu szablonu do utworzenia maszyny wirtualnej, korzystając z informacji w [Rozmieszczanie maszyn wirtualnych Azure za pomocą języka C# i szablonu Menedżera zasobów](virtual-machines-windows-csharp-template.md).
- Dowiedz się, jak zarządzać maszyny wirtualnej utworzone przez Ciebie, przeglądając artykuł [Zarządzanie maszyn wirtualnych za pomocą Menedżera zasobów Azure i programu PowerShell](virtual-machines-windows-csharp-manage.md).
