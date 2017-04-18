<properties
    pageTitle="Wdrażanie maszyny przy użyciu języka C# i szablonu Menedżera zasobów | Microsoft Azure"
    description="Dowiedz się, jak za pomocą języka C# i szablonu Menedżera zasobów do wdrażania maszyn wirtualnych Azure."
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
    ms.date="10/10/2016"
    ms.author="davidmu"/>

# <a name="deploy-an-azure-virtual-machine-using-c-and-a-resource-manager-template"></a>Wdrażanie maszyn wirtualnych Azure za pomocą języka C# i szablonu Menedżera zasobów

Przy użyciu grup zasobów i szablony, możesz zarządzać wszystkie zasoby razem obsługujące aplikacji. W tym artykule pokazano, jak za pomocą programu Visual Studio i C# Konfigurowanie uwierzytelniania, Utwórz szablon, a następnie wdrożyć Azure zasobów za pomocą utworzony szablon.

Musisz najpierw upewnij się, że po wykonaniu kroków konfiguracyjnych:

- Instalowanie [programu Visual Studio](http://msdn.microsoft.com/library/dd831853.aspx)
- Weryfikacja instalacji [systemu Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) lub [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855)
- Pobierz [token uwierzytelniania](../resource-group-authenticate-service-principal.md)
- Tworzenie grupy zasobów przy użyciu [Programu PowerShell Azure](../resource-group-template-deploy.md), [Polecenie Azure](../resource-group-template-deploy-cli.md)lub [Azure portal](../resource-group-template-deploy-portal.md).

Trwa około 30 minut wykonaj następujące kroki.
    
## <a name="step-1-create-the-visual-studio-project-the-template-file-and-the-parameters-file"></a>Krok 1: Tworzenie projektu programu Visual Studio, plik szablonu i pliku parametrów

### <a name="create-the-template-file"></a>Utwórz plik szablonu

Szablon programu Menedżer zasobów Azure umożliwia wdrażać i zarządzania zasobami Azure razem. Szablon jest opis JSON zasobów i parametrów skojarzone wdrożenia.

W programie Visual Studio wykonaj następujące czynności:

1. Kliknij pozycję **plik** > **Nowy** > **projektu**.

2. W **szablonach** > **Visual C#**, wybierz **Aplikację konsoli**, wprowadź nazwę i lokalizację projektu, a następnie kliknij **przycisk OK**.

3. Kliknij prawym przyciskiem myszy nazwę projektu w Eksploratorze rozwiązań, kliknij przycisk **Dodaj** > **Nowy element**.

4. Kliknij pozycję sieć Web, zaznacz plik JSON, wprowadź nazwę *VirtualMachineTemplate.json* , a następnie kliknij przycisk **Dodaj**.

5. W polu otwierający i zamykający nawias pliku VirtualMachineTemplate.json dodać elementu wymaganego schematu oraz element wymagane contentVersion:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
        }

6. [Parametry](../resource-group-authoring-templates.md#parameters) nie są zawsze wymagane, ale umożliwiają do wprowadzania wartości po wdrożeniu szablonu. Dodawanie elementu parametry i jej elementów podrzędnych po elemencie contentVersion:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUserName": { "type": "string" },
            "adminPassword": { "type": "securestring" }
          },
        }

7. [Zmienne](../resource-group-authoring-templates.md#variables) może służyć w szablonie, aby określić wartości, które mogą się zmienić często lub wartości, które muszą zostać utworzone z kombinacji wartości parametrów. Dodaj element zmienne po sekcji Parametry:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUsername": { "type": "string" },
            "adminPassword": { "type": "securestring" }
          },
          "variables": {
            "vnetID":"[resourceId('Microsoft.Network/virtualNetworks','myvn1')]",
            "subnetRef": "[concat(variables('vnetID'),'/subnets/mysn1')]"  
          },
        }

8. [Zasoby](../resource-group-authoring-templates.md#resources) takie jak maszyna wirtualna, wirtualną sieć i konta miejsca do magazynowania są definiowane dalej w szablonie. Dodawanie sekcji zasoby po sekcji zmiennych:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUsername": { "type": "string" },
            "adminPassword": { "type": "securestring" }
          },
          "variables": {
            "vnetID":"[resourceId('Microsoft.Network/virtualNetworks','myvn1')]",
            "subnetRef": "[concat(variables('vnetID'),'/subnets/mysn1')]"
          },
          "resources": [
            {
              "type": "Microsoft.Storage/storageAccounts",
              "name": "mystorage1",
              "apiVersion": "2015-06-15",
              "location": "[resourceGroup().location]",
              "properties": { "accountType": "Standard_LRS" }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Network/publicIPAddresses",
              "name": "myip1",
              "location": "[resourceGroup().location]",
              "properties": {
                "publicIPAllocationMethod": "Dynamic",
                "dnsSettings": { "domainNameLabel": "mydns1" }
              }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Network/virtualNetworks",
              "name": "myvnet1",
              "location": "[resourceGroup().location]",
              "properties": {
                "addressSpace": { "addressPrefixes": [ "10.0.0.0/16" ] },
                "subnets": [ {
                  "name": "mysn1",
                  "properties": { "addressPrefix": "10.0.0.0/24" }
                } ]
              }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Network/networkInterfaces",
              "name": "mync1",
              "location": "[resourceGroup().location]",
              "dependsOn": [
                "Microsoft.Network/publicIPAddresses/myip1",
                "Microsoft.Network/virtualNetworks/myvn1"
              ],
              "properties": {
                "ipConfigurations": [ {
                  "name": "ipconfig1",
                  "properties": {
                    "privateIPAllocationMethod": "Dynamic",
                    "publicIPAddress": {
                      "id": "[resourceId('Microsoft.Network/publicIPAddresses', 'myip1')]"
                    },
                    "subnet": { "id": "[variables('subnetRef')]" }
                  }
                } ]
              }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Compute/virtualMachines",
              "name": "myvm1",
              "location": "[resourceGroup().location]",
              "dependsOn": [
                "Microsoft.Network/networkInterfaces/mync1",
                "Microsoft.Storage/storageAccounts/mystorage1"
              ],
              "properties": {
                "hardwareProfile": { "vmSize": "Standard_A1" },
                "osProfile": {
                  "computerName": "myvm1",
                  "adminUsername": "[parameters('adminUsername')]",
                  "adminPassword": "[parameters('adminPassword')]"
                },
                "storageProfile": {
                  "imageReference": {
                    "publisher": "MicrosoftWindowsServer",
                    "offer": "WindowsServer",
                    "sku": "2012-R2-Datacenter",
                    "version" : "latest"
                  },
                  "osDisk": {
                    "name": "myosdisk1",
                    "vhd": {
                      "uri": "https://mystorage1.blob.core.windows.net/vhds/myosdisk1.vhd"
                    },
                    "caching": "ReadWrite",
                    "createOption": "FromImage"
                  }
                },
                "networkProfile": {
                  "networkInterfaces" : [ {
                    "id": "[resourceId('Microsoft.Network/networkInterfaces','mync1')]"
                  } ]
                }
              }
            } ]
          }
      
9. Zapisz plik szablonu, który został utworzony.

### <a name="create-the-parameters-file"></a>Tworzenie pliku parametrów

Aby określić wartości parametrów zasobów, które zostały zdefiniowane w szablonie, możesz utworzyć plik parametry, który zawiera wartości, które są używane po wdrożeniu szablonu. W programie Visual Studio wykonaj następujące czynności:

1. Kliknij prawym przyciskiem myszy nazwę projektu w Eksploratorze rozwiązań, kliknij przycisk **Dodaj** > **Nowy element**.

2. Kliknij pozycję sieć Web, zaznacz plik JSON, wprowadź nazwę *Parameters.json* , a następnie kliknij przycisk **Dodaj**.

3. Otwórz plik parameters.json, a następnie dodaj tę zawartość JSON:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUserName": { "value": "mytestacct1" },
            "adminPassword": { "value": "mytestpass1" }
          }
        }

    >[AZURE.NOTE] W tym artykule tworzy maszyny wirtualnej wersją systemu operacyjnego Windows Server. Aby dowiedzieć się więcej o wybieraniu innych obrazów, zobacz [Nawigacja i wybierz pozycję Azure maszyn wirtualnych obrazów za pomocą programu Windows PowerShell i polecenie Azure](virtual-machines-linux-cli-ps-findimage.md).

4. Zapisz plik parametry, który został utworzony.

## <a name="step-2-install-the-libraries"></a>Krok 2: Instalowanie bibliotek

Pakiety NuGet są najprostszym sposobem instalowania bibliotek, potrzebnymi do zakończenia tego samouczka. Potrzebujesz Biblioteka zarządzania zasobów Azure i Azure Active Directory Authentication Library, aby utworzyć zasoby. Aby uzyskać te biblioteki w programie Visual Studio, wykonaj następujące czynności:

1. Kliknij prawym przyciskiem myszy nazwę projektu w Eksploratorze rozwiązań, kliknij pozycję **Zarządzaj pakiety NuGet**, a następnie kliknij przycisk Przeglądaj.

2. Typ *Usługi Active Directory* w polu wyszukiwania, kliknij przycisk **Zainstaluj** pakietu Active Directory Authentication Library, a następnie postępuj zgodnie z instrukcjami, aby zainstalować pakiet.

4. W górnej części strony wybierz pozycję **Obejmują wstępną**. Typ *Microsoft.Azure.Management.ResourceManager* w polu wyszukiwania, kliknij przycisk **Zainstaluj** dla biblioteki zarządzania Microsoft Azure zasobu, a następnie postępuj zgodnie z instrukcjami, aby zainstalować pakiet.

Teraz możesz rozpocząć tworzenie aplikacji przy użyciu bibliotek.

## <a name="step-3-create-the-credentials-that-are-used-to-authenticate-requests"></a>Krok 3: Tworzenie poświadczeń, które służą do uwierzytelniania żądania

Utworzono aplikacji usługi Azure Active Directory i biblioteki uwierzytelniania jest zainstalowany. Teraz możesz formatować informacji o aplikacji do poświadczeń używanych do uwierzytelniania żądań do Menedżera zasobów Azure.

1. Otwórz plik Plik Program.cs dla projektu, który został utworzony, a następnie dodaj tych przy użyciu instrukcji na początek pliku:

        using Microsoft.Azure;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Azure.Management.ResourceManager;
        using Microsoft.Azure.Management.ResourceManager.Models;
        using Microsoft.Rest;
        using System.IO;

2.  Dodawanie tej metody można użyć do klasy programu w celu uzyskania tokenu potrzebne do utworzenia poświadczeń:

        private static async Task<AuthenticationResult> GetAccessTokenAsync()
        {
          var cc = new ClientCredential("{client-id}", "{client-secret}");
          var context = new AuthenticationContext("https://login.windows.net/{tenant-id}");
          var token = await context.AcquireTokenAsync("https://management.azure.com/", cc);
          if (token == null)
          {
            throw new InvalidOperationException("Could not get the token.");
          }
          return token;
        }

    Zamień {identyfikator klienta} identyfikator usługi Azure Active Directory aplikacji, {klienta tajny} przy użyciu klucza dostępu AD aplikacji i {dzierżawy identyfikator} identyfikatorem dzierżawy dla subskrypcji. Identyfikator dzierżawy można znaleźć, uruchamiając Get-AzureRmSubscription. Klawisz dostępu można znaleźć za pomocą portalu Azure.

3. Aby utworzyć poświadczeń, Dodaj kod do metody głównego w pliku Plik Program.cs:

        var token = GetAccessTokenAsync();
        var credential = new TokenCredentials(token.Result.AccessToken);

4. Zapisz plik Plik Program.cs.

## <a name="step-4-deploy-the-template"></a>Krok 4: Wdrażanie szablonu

W tym kroku użyć grupa zasobów, uprzednio utworzony, ale można również utworzyć grupę zasobów przy użyciu klas [ResourceManagementClient](https://msdn.microsoft.com/library/azure/microsoft.azure.management.resources.resourcemanagementclient.aspx) i [Grupa zasobów](https://msdn.microsoft.com/library/azure/microsoft.azure.management.resources.models.resourcegroup.aspx) .

1. Dodać zmienne metody główne klasy programu do określania nazw zasobów, które poprzednio utworzone, nazwę wdrożenia i identyfikatora subskrypcji:

        var groupName = "resource group name";
        var subscriptionId = "subsciption id";
        var deploymentName = "deployment name";

    Zamień wartość grupy Nazwa grupy zasobów. Zamień wartość deploymentName nazwę, która ma być używany do wdrożenia. Identyfikator subskrypcji można znaleźć, uruchamiając Get-AzureRmSubscription.

2. Dodawanie tej metody można użyć do klasy programu wdrożenia zasobów do grupy zasobów przy użyciu określonego szablonu:

        public static async Task<DeploymentExtended> CreateTemplateDeploymentAsync(
          TokenCredentials credential,
          string groupName,
          string deploymentName,
          string subscriptionId)
        {
          Console.WriteLine("Creating the template deployment...");
          var deployment = new Deployment();
          deployment.Properties = new DeploymentProperties
          {
            Mode = DeploymentMode.Incremental,
            Template = File.ReadAllText("..\\..\\VirtualMachineTemplate.json"),
            Parameters = File.ReadAllText("..\\..\\Parameters.json")
          };
          var resourceManagementClient = new ResourceManagementClient(credential) 
            { SubscriptionId = subscriptionId };
          return await resourceManagementClient.Deployments.CreateOrUpdateAsync(
            groupName,
            deploymentName,
            deployment);
        }

    Jeśli chcesz wdrożyć szablonu z konta miejsca do magazynowania, możesz zastąpić właściwości szablonu właściwości TemplateLink.

3. Aby zadzwonić do metody, który właśnie został dodany, Dodaj ten kod metody główne:

        var dpResult = CreateTemplateDeploymentAsync(
          credential,
          groupName,
          deploymentName,
          subscriptionId);
        Console.WriteLine(dpResult.Result.Properties.ProvisioningState);
        Console.ReadLine();

## <a name="step-5-delete-the-resources"></a>Krok 5: Usuwanie zasobów

Ponieważ jest naliczany dla zasobów używanych w Azure, zawsze jest dobrym rozwiązaniem jest usunięcie zasoby, które nie są już potrzebne. Nie musisz osobno usunąć każdego zasobu z grupy zasobów. Usuwanie grupy zasobów i wszystkie jego zasoby są automatycznie usuwane.

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

2.  Aby zadzwonić do metody, który właśnie został dodany, Dodaj ten kod metody główne:

        DeleteResourceGroupAsync(
          credential,
          groupName,
          subscriptionId);
        Console.ReadLine();

##<a name="step-6-run-the-console-application"></a>Krok 6: Uruchomić aplikację konsoli

1.  Aby uruchomić aplikację konsoli, kliknij przycisk **Start** , w programie Visual Studio, a następnie zaloguj się do Azure AD przy użyciu tych samych poświadczeń, korzystające z subskrypcją.

2.  Po wyświetleniu stanu zaakceptowane, naciśnij klawisz **Enter** .

    Należy trwa około pięć minut dla tej aplikacji konsoli uruchomić całkowicie od początku do końca. Przed naciśnięciem klawisza Enter, aby uruchomić usuwania zasobów, może potrwać kilka minut, aby sprawdzić tworzenia zasobów w portalu Azure przed ich usuwanie.

3. Aby wyświetlić stan zasobów, przejdź do dzienników inspekcji w portalu Azure:

    ![Przeglądanie dzienników inspekcji w Azure portal](./media/virtual-machines-windows-csharp-template/crpportal.png)

## <a name="next-steps"></a>Następne kroki

- W przypadku problemów z wdrożenia następnym krokiem jest przeglądać [wdrożeń grupa zasobów Rozwiązywanie problemów z Azure portal](../resource-manager-troubleshoot-deployments-portal.md).
- Dowiedz się, jak zarządzać maszyny wirtualnej utworzone przez Ciebie, przeglądając artykuł [Zarządzanie maszyn wirtualnych za pomocą Menedżera zasobów Azure i programu PowerShell](virtual-machines-windows-csharp-manage.md).
