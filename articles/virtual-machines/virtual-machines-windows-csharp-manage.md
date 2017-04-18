<properties
    pageTitle="Zarządzanie maszyny wirtualne za pomocą Menedżera zasobów Azure i C# | Microsoft Azure"
    description="Zarządzanie maszyn wirtualnych za pomocą Menedżera zasobów Azure i C#."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="davidmu"/>

# <a name="manage-azure-virtual-machines-using-azure-resource-manager-and-c"></a>Zarządzanie maszyn wirtualnych Azure za pomocą Menedżera zasobów Azure i C#  

Zadania w tym artykule pokazano, jak zarządzać maszyn wirtualnych, takich jak uruchamianie, zatrzymywanie i aktualizowanie. Maszyny wirtualnej, musi istnieć w grupie zasobów do wykonania zadań w tym artykule.

Aby wykonać zadania w tym artykule, należy następująco:

- [Programu Visual Studio](http://msdn.microsoft.com/library/dd831853.aspx)
- [Token uwierzytelniania](../resource-group-authenticate-service-principal.md)

## <a name="create-a-visual-studio-project-and-install-packages"></a>Tworzenie projektu programu Visual Studio i zainstalować pakiety

Pakiety NuGet są najłatwiejszym sposoby instalowania bibliotek, które są potrzebne do ukończenia zadań w tym artykule. Bibliotek, które można zainstalować w tym artykule są Azure Active Directory Authentication Library i Biblioteka obliczyć dostawcy zasobów. Wykonaj poniższe czynności, aby uzyskać bibliotek w programie Visual Studio:

1. Kliknij pozycję **plik** > **Nowy** > **projektu**.

2. W **szablonach** > **Visual C#**, wybierz **Aplikację konsoli**, wprowadź nazwę i lokalizację projektu, a następnie kliknij **przycisk OK**.

3. Kliknij prawym przyciskiem myszy nazwę projektu w Eksploratorze rozwiązanie, a następnie kliknij **Zarządzanie pakietów NuGet**.

4. Typ *Usługi Active Directory* w polu wyszukiwania, kliknij przycisk **Zainstaluj** pakietu Active Directory Authentication Library, a następnie postępuj zgodnie z instrukcjami, aby zainstalować pakiet.

5. W górnej części strony wybierz pozycję **Obejmują wstępną**. Typ *Microsoft.Azure.Management.Compute* w polu wyszukiwania, kliknij przycisk **Zainstaluj** dla bibliotek .NET obliczenia, a następnie postępuj zgodnie z instrukcjami, aby zainstalować pakiet.

Teraz możesz rozpocząć korzystanie z narzędzia bibliotek do zarządzania maszyn wirtualnych.

## <a name="set-up-the-project"></a>Konfigurowanie projektu

Teraz, gdy aplikacja zostanie utworzona i bibliotek są zainstalowane, możesz utworzyć tokenu przy użyciu informacji o aplikacji. Ten token jest używany do uwierzytelniania żądań do Menedżera zasobów Azure.

1. Otwórz plik Plik Program.cs dla projektu, który został utworzony, a następnie dodaj tych przy użyciu instrukcji na początek pliku:

        using Microsoft.Azure;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Azure.Management.Compute;
        using Microsoft.Azure.Management.Compute.Models;
        using Microsoft.Rest;
        
2. Dodać zmienne metody główne klasy programu Określ nazwę grupy zasobów, a Nazwa maszyny wirtualnej i identyfikatora subskrypcji:

        var groupName = "resource group name";
        var vmName = "virtual machine name";  
        var subscriptionId = "subsciption id";

    Identyfikator subskrypcji można znaleźć, uruchamiając Get-AzureRmSubscription.
    
3. Aby uzyskać token, który jest potrzebne do utworzenia poświadczeń, należy dodać tej metody można użyć do klasy programu:

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
    
4. Aby utworzyć poświadczeń, Dodaj kod do metody głównego w plik Program.cs:

        var token = GetAccessTokenAsync();
        var credential = new TokenCredentials(token.Result.AccessToken);

5. Zapisz plik Plik Program.cs.

## <a name="display-information-about-a-virtual-machine"></a>Wyświetlanie informacji o maszyny wirtualnej

1. Dodawanie tej metody można użyć do klasy programu w programie project, uprzednio utworzony:

        public static async void GetVirtualMachineAsync(
          TokenCredentials credential, 
          string groupName, 
          string vmName, 
          string subscriptionId)
        {
          Console.WriteLine("Getting information about the virtual machine...");

          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          var vmResult = await computeManagementClient.VirtualMachines.GetAsync(
            groupName, 
            vmName, 
            InstanceViewTypes.InstanceView);

          Console.WriteLine("hardwareProfile");
          Console.WriteLine("   vmSize: " + vmResult.HardwareProfile.VmSize);

          Console.WriteLine("\nstorageProfile");
          Console.WriteLine("  imageReference");
          Console.WriteLine("    publisher: " + vmResult.StorageProfile.ImageReference.Publisher);
          Console.WriteLine("    offer: " + vmResult.StorageProfile.ImageReference.Offer);
          Console.WriteLine("    sku: " + vmResult.StorageProfile.ImageReference.Sku);
          Console.WriteLine("    version: " + vmResult.StorageProfile.ImageReference.Version);
          Console.WriteLine("  osDisk");
          Console.WriteLine("    osType: " + vmResult.StorageProfile.OsDisk.OsType);
          Console.WriteLine("    name: " + vmResult.StorageProfile.OsDisk.Name);
          Console.WriteLine("    createOption: " + vmResult.StorageProfile.OsDisk.CreateOption);
          Console.WriteLine("    uri: " + vmResult.StorageProfile.OsDisk.Vhd.Uri);
          Console.WriteLine("    caching: " + vmResult.StorageProfile.OsDisk.Caching);

          Console.WriteLine("\nosProfile");
          Console.WriteLine("  computerName: " + vmResult.OsProfile.ComputerName);
          Console.WriteLine("  adminUsername: " + vmResult.OsProfile.AdminUsername);
          Console.WriteLine("  provisionVMAgent: " + vmResult.OsProfile.WindowsConfiguration.ProvisionVMAgent.Value);
          Console.WriteLine("  enableAutomaticUpdates: " + vmResult.OsProfile.WindowsConfiguration.EnableAutomaticUpdates.Value);

          Console.WriteLine("\nnetworkProfile");
          foreach (NetworkInterfaceReference nic in vmResult.NetworkProfile.NetworkInterfaces)
          {
            Console.WriteLine("  networkInterface id: " + nic.Id);
          }

          Console.WriteLine("\nvmAgent");
          Console.WriteLine("  vmAgentVersion" + vmResult.InstanceView.VmAgent.VmAgentVersion);
          Console.WriteLine("    statuses");
          foreach (InstanceViewStatus stat in vmResult.InstanceView.VmAgent.Statuses)
          {
            Console.WriteLine("    code: " + stat.Code);
            Console.WriteLine("    level: " + stat.Level);
            Console.WriteLine("    displayStatus: " + stat.DisplayStatus);
            Console.WriteLine("    message: " + stat.Message);
            Console.WriteLine("    time: " + stat.Time);
          }

          Console.WriteLine("\ndisks");
          foreach (DiskInstanceView idisk in vmResult.InstanceView.Disks)
          {
            Console.WriteLine("  name: " + idisk.Name);
            Console.WriteLine("  statuses");
            foreach (InstanceViewStatus istat in idisk.Statuses)
            {
              Console.WriteLine("    code: " + istat.Code);
              Console.WriteLine("    level: " + istat.Level);
              Console.WriteLine("    displayStatus: " + istat.DisplayStatus);
              Console.WriteLine("    time: " + istat.Time);
            }
          }

          Console.WriteLine("\nVM general status");
          Console.WriteLine("  provisioningStatus: " + vmResult.ProvisioningState);
          Console.WriteLine("  id: " + vmResult.Id);
          Console.WriteLine("  name: " + vmResult.Name);
          Console.WriteLine("  type: " + vmResult.Type);
          Console.WriteLine("  location: " + vmResult.Location);
          Console.WriteLine("\nVM instance status");
          foreach (InstanceViewStatus istat in vmResult.InstanceView.Statuses)
          {
            Console.WriteLine("\n  code: " + istat.Code);
            Console.WriteLine("  level: " + istat.Level);
            Console.WriteLine("  displayStatus: " + istat.DisplayStatus);
          }
          
        }

2. Aby zadzwonić do metody, który właśnie został dodany, Dodaj ten kod metody główne:

        GetVirtualMachineAsync(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();
    
3. Zapisz plik Plik Program.cs.

4. Kliknij przycisk **Start** , w programie Visual Studio, a następnie zaloguj się do Azure AD przy użyciu tę samą nazwę użytkownika i hasło, którego używasz z subskrypcji.

    Po uruchomieniu tej metody, powinien zostać wyświetlony coś w tym przykładzie:
    
        Getting information about the virtual machine...
        hardwareProfile
          vmSize: Standard_A0

        storageProfile
          imageReference
            publisher: MicrosoftWindowsServer
            offer: WindowsServer
            sku: 2012-R2-Datacenter
            version: latest
          osDisk
            osType: Windows
            name: myosdisk
            createOption: FromImage
            uri: http://store1.blob.core.windows.net/vhds/myosdisk.vhd
            caching: ReadWrite

          osProfile
            computerName: vm1
            adminUsername: account1
            provisionVMAgent: True
            enableAutomaticUpdates: True

          networkProfile
            networkInterface 
              id: /subscriptions/{subscription-id}
                /resourceGroups/rg1/providers/Microsoft.Network/networkInterfaces/nc1

          vmAgent
            vmAgentVersion2.7.1198.766
            statuses
            code: ProvisioningState/succeeded
            level: Info
            displayStatus: Ready
            message: GuestAgent is running and accepting new configurations.
            time: 4/13/2016 8:35:32 PM

          disks
            name: myosdisk
            statuses
              code: ProvisioningState/succeeded
              level: Info
              displayStatus: Provisioning succeeded
              time: 4/13/2016 8:04:36 PM

          VM general status
            provisioningStatus: Succeeded
            id: /subscriptions/{subscription-id}
              /resourceGroups/rg1/providers/Microsoft.Compute/virtualMachines/vm1
            name: vm1
            type: Microsoft.Compute/virtualMachines
            location: centralus

          VM instance status
            code: ProvisioningState/succeeded
              level: Info
              displayStatus: Provisioning succeeded
            code: PowerState/running
              level: Info
              displayStatus: VM running

## <a name="stop-a-virtual-machine"></a>Zatrzymywanie maszyny wirtualnej

Możesz wyłączyć maszyny wirtualnej na dwa sposoby. Można zatrzymać maszyny wirtualnej i zachować wszystkie jej ustawienia, ale nadal mają być pobierane za go lub można zatrzymać maszyny wirtualnej i cofnąć go. Maszyny wirtualnej jest alokację, wszystkie zasoby skojarzone z nim są również alokację i rozliczeń punktów końcowych go.

1. Komentarz kodu wcześniej dodane metodę głównym z wyjątkiem kod, aby odczytać poświadczenia.

2. Dodawanie tej metody można użyć do klasy programu:

        public static async void StopVirtualMachineAsync(
          TokenCredentials credential, 
          string groupName, 
          string vmName, 
          string subscriptionId)
        {
          Console.WriteLine("Stopping the virtual machine...");
          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          await computeManagementClient.VirtualMachines.PowerOffAsync(groupName, vmName);
        }

    Jeśli chcesz cofnąć maszyny wirtualnej, należy zmienić połączenie wyłączania zasilania w trybie do tego kodu:

        computeManagementClient.VirtualMachines.Deallocate(groupName, vmName);

3. Aby zadzwonić do metody, który właśnie został dodany, Dodaj ten kod metody główne:

        StopVirtualMachineAsync(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();

4. Zapisz plik Plik Program.cs.

5. Kliknij przycisk **Start** , w programie Visual Studio, a następnie zaloguj się do Azure AD przy użyciu tę samą nazwę użytkownika i hasło, którego używasz z subskrypcji.

    Powinien zostać wyświetlony stan zmiana maszyn wirtualnych zatrzymania. Jeśli po uruchomieniu wybranej metody zatrzymaniu Deallocate połączeń, stan (alokację).

## <a name="start-a-virtual-machine"></a>Rozpoczynanie maszyny wirtualnej

1. Komentarz kodu wcześniej dodane metodę głównym z wyjątkiem kod, aby odczytać poświadczenia.

2. Dodawanie tej metody można użyć do klasy programu:

        public static async void StartVirtualMachineAsync(
          TokenCredentials credential, 
          string groupName, 
          string vmName, 
          string subscriptionId)
        {
          Console.WriteLine("Starting the virtual machine...");
          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          await computeManagementClient.VirtualMachines.StartAsync(groupName, vmName);
        }

3. Aby zadzwonić do metody, który właśnie został dodany, Dodaj ten kod metody główne:

        StartVirtualMachineAsync(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();

4. Zapisz plik Plik Program.cs.

5. Kliknij przycisk **Start** , w programie Visual Studio, a następnie zaloguj się do Azure AD przy użyciu tę samą nazwę użytkownika i hasło, którego używasz z subskrypcji.

    Stan maszyny wirtualnej Zmienianie do pracy powinna być widoczna.

## <a name="restart-a-running-virtual-machine"></a>Uruchom ponownie uruchomionej maszyny wirtualnej

1. Komentarz kodu wcześniej dodane metodę głównym z wyjątkiem kod, aby odczytać poświadczenia.

2. Dodawanie tej metody można użyć do klasy programu:

        public static async void RestartVirtualMachineAsync(
          TokenCredentials credential,
          string groupName,
          string vmName,
          string subscriptionId)
        {
          Console.WriteLine("Restarting the virtual machine...");
          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          await computeManagementClient.VirtualMachines.RestartAsync(groupName, vmName);
        }

3. Aby zadzwonić do metody, który właśnie został dodany, Dodaj ten kod metody główne:

        RestartVirtualMachineAsync(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();

4. Zapisz plik Plik Program.cs.

5. Kliknij przycisk **Start** , w programie Visual Studio, a następnie zaloguj się do Azure AD przy użyciu tę samą nazwę użytkownika i hasło, którego używasz z subskrypcji.

## <a name="resize-a-virtual-machine"></a>Zmienianie rozmiaru maszyny wirtualnej

W tym przykładzie pokazano, jak zmienić rozmiar uruchomionej maszyny wirtualnej.

1. Komentarz kodu wcześniej dodane metodę głównym z wyjątkiem kod, aby odczytać poświadczenia.

2. Dodawanie tej metody można użyć do klasy programu:

        public static async void UpdateVirtualMachineAsync(
          TokenCredentials credential, 
          string groupName, 
          string vmName, 
          string subscriptionId)
        {
          Console.WriteLine("Updating the virtual machine...");
          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          var vmResult = await computeManagementClient.VirtualMachines.GetAsync(groupName, vmName);
          vmResult.HardwareProfile.VmSize = "Standard_A1";
          await computeManagementClient.VirtualMachines.CreateOrUpdateAsync(groupName, vmName, vmResult);
        }

3. Aby zadzwonić do metody, który właśnie został dodany, Dodaj ten kod metody główne:

        UpdateVirtualMachineAsync(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();

4. Zapisz plik Plik Program.cs.

5. Kliknij przycisk **Start** , w programie Visual Studio, a następnie zaloguj się do Azure AD przy użyciu tę samą nazwę użytkownika i hasło, którego używasz z subskrypcji.

    Powinien zostać wyświetlony rozmiar zmiana maszyn wirtualnych Standard_A1.

## <a name="add-a-data-disk-to-a-virtual-machine"></a>Dodawanie dysku danych maszyn wirtualnych

W tym przykładzie pokazano, jak dodać dysk danych uruchomionego maszyn wirtualnych.

1. Komentarz kodu wcześniej dodane metodę głównym z wyjątkiem kod, aby odczytać poświadczenia.

2. Dodawanie tej metody można użyć do klasy programu:

        public static async void AddDataDiskAsync(
          TokenCredentials credential, 
          string groupName, 
          string vmName, 
          string subscriptionId)
        {
          Console.WriteLine("Adding the disk to the virtual machine...");
          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          var vmResult = await computeManagementClient.VirtualMachines.GetAsync(groupName, vmName);
          vmResult.StorageProfile.DataDisks.Add(
            new DataDisk
              {
                Lun = 0,
                Name = "mydatadisk1",
                Vhd = new VirtualHardDisk
                  {
                    Uri = "https://mystorage1.blob.core.windows.net/vhds/mydatadisk1.vhd"
                  },
                CreateOption = DiskCreateOptionTypes.Empty,
                DiskSizeGB = 2,
                Caching = CachingTypes.ReadWrite
              });
          await computeManagementClient.VirtualMachines.CreateOrUpdateAsync(groupName, vmName, vmResult);
        }

3. Aby zadzwonić do metody, który właśnie został dodany, Dodaj ten kod metody główne:

        AddDataDiskAsync(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();

4. Zapisz plik Plik Program.cs.

5. Kliknij przycisk **Start** , w programie Visual Studio, a następnie zaloguj się do Azure AD przy użyciu tę samą nazwę użytkownika i hasło, którego używasz z subskrypcji.

## <a name="delete-a-virtual-machine"></a>Usuwanie maszyny wirtualnej

1. Komentarz kodu wcześniej dodane metodę głównym z wyjątkiem kod, aby odczytać poświadczenia.

2. Dodawanie tej metody można użyć do klasy programu:

        public static async void DeleteVirtualMachineAsync(
          TokenCredentials credential, 
          string groupName, 
          string vmName, 
          string subscriptionId)
        {
          Console.WriteLine("Deleting the virtual machine...");
          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          await computeManagementClient.VirtualMachines.DeleteAsync(groupName, vmName);
        }

3. Aby zadzwonić do metody, który właśnie został dodany, Dodaj ten kod metody główne:

        DeleteVirtualMachineAsync(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();

4. Zapisz plik Plik Program.cs.

5. Kliknij przycisk **Start** , w programie Visual Studio, a następnie zaloguj się do Azure AD przy użyciu tę samą nazwę użytkownika i hasło, którego używasz z subskrypcji.

## <a name="next-steps"></a>Następne kroki

W przypadku problemów z wdrożeniu może przeglądać [wdrożeń grupa zasobów Rozwiązywanie problemów z Azure portal](../resource-manager-troubleshoot-deployments-portal.md)
