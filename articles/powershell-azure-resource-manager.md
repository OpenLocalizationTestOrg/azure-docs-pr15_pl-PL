<properties 
    pageTitle="Azure programu PowerShell przy użyciu Menedżera zasobów | Microsoft Azure" 
    description="Wprowadzenie do korzystania z programu PowerShell Azure do wdrożenia wielu zasobów jako grupa zasobów Azure." 
    services="azure-resource-manager" 
    documentationCenter="" 
    authors="tfitzmac" 
    manager="timlt" 
    editor="tysonn"/>

<tags 
    ms.service="azure-resource-manager" 
    ms.workload="multiple" 
    ms.tgt_pltfrm="powershell" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/18/2016" 
    ms.author="tomfitz"/>

# <a name="using-azure-powershell-with-azure-resource-manager"></a>Przy użyciu programu PowerShell Azure przy użyciu Menedżera zasobów Azure

> [AZURE.SELECTOR]
- [Portal](azure-portal/resource-group-portal.md) 
- [Polecenie Azure](xplat-cli-azure-resource-manager.md)
- [Azure programu PowerShell](powershell-azure-resource-manager.md)
- [INTERFEJSU API USŁUGI REST](resource-manager-rest-api.md)


Azure Menedżera zasobów zawiera nowoczesny podejście do kontroli cyklu życia Azure zasobów. Zamiast tworzyć i zarządzać poszczególnych zasobów, zacznij od obmyślająca całe rozwiązanie, takich jak blogu, Galeria fotografii, portalu programu SharePoint lub witryny typu wiki. Szablon — reprezentacja rozwiązania — umożliwia definiowanie grupa zasobów, która zawiera wszystkie potrzebne do obsługi rozwiązanie zasoby. Następnie wdrażanie i zarządzanie nią danej grupy zasobów jako jednostki logicznej. 

W tym samouczku dowiesz się, jak za pomocą programu PowerShell Azure z Menedżera zasobów Azure. Jego przeprowadzi Cię przez proces wdrażanie rozwiązania i Praca z rozwiązaniem. Aby wdrożyć użyje Azure programu PowerShell i szablon Menedżera zasobów:

- Program SQL server — do obsługi bazy danych
- Baza danych SQL — do przechowywania danych
- Reguły zapory — aby umożliwić aplikacji sieci web do połączenia z bazą danych
- Planowanie aplikacji usługi — definiowanie możliwości i koszt aplikacji sieci web
- Witryna sieci Web — do uruchamiania aplikacji sieci web
- Konfiguracja sieci Web — do przechowywania parametry połączenia z bazą danych 
- Reguły alertów — monitorowanie wydajności i błędów
- Wnioski aplikacji — automatyczne skalowanie ustawień

## <a name="prerequisites"></a>Wymagania wstępne

Aby użyć tego samouczka, należy następująco:

- Konto Azure
  + Możesz [otworzyć konto Azure bezpłatnie](/pricing/free-trial/?WT.mc_id=A261C142F): uzyskiwanie środków Umożliwia wypróbowanie płatnych usług Azure, a nawet w przypadku, gdy są używane nawet przechowujesz konta i użyj wolny Azure usług, takich jak witryny sieci Web. Karta kredytowa nigdy nie zostanie obciążona, chyba że jawnie Zmienianie ustawień i poproś o naliczane.
  
  + Można [aktywować korzyści subskrybentów MSDN](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F): subskrypcji MSDN i zapewnia środków co miesiąc, używanej usługi Azure płatnej.
- Azure programu PowerShell 1.0. Aby uzyskać informacje o tej wersji i jak ją zainstalować zobacz [jak zainstalować i skonfigurować Azure programu PowerShell](powershell-install-configure.md).

Ten samouczek jest przeznaczony dla początkujących programu PowerShell, ale przyjęto założenie, że wiesz, podstawowe pojęcia, takie jak moduły, polecenia cmdlet i sesji.

## <a name="get-help-for-cmdlets"></a>Uzyskiwanie pomocy dotyczącej poleceń cmdlet

Aby uzyskać szczegółową pomoc dla dowolnego polecenia cmdlet, który zostanie wyświetlony w tym samouczku, użyj polecenia cmdlet Get-Help. 

    Get-Help <cmdlet-name> -Detailed

Na przykład aby uzyskać pomoc dotyczącą polecenia cmdlet Get-AzureRmResource, należy wpisać:

    Get-Help Get-AzureRmResource -Detailed

Aby uzyskać listę poleceń cmdlet w module Zasoby z streszczenie pomocy, należy wpisać: 

    Get-Command -Module AzureRM.Resources | Get-Help | Format-Table Name, Synopsis

Wynik będzie wyglądać podobnie do poniższy fragment:

    Name                                   Synopsis
    ----                                   --------
    Find-AzureRmResource                   Searches for resources using the specified parameters.
    Find-AzureRmResourceGroup              Searches for resource group using the specified parameters.
    Get-AzureRmADGroup                     Filters active directory groups.
    Get-AzureRmADGroupMember               Get a group members.
    ...

Aby uzyskać pełną pomoc dla polecenia cmdlet, wpisz polecenie z formatem:

    Get-Help <cmdlet-name> -Full
  
## <a name="login-to-your-azure-account"></a>Zaloguj się do konta Azure

Przed rozpoczęciem pracy rozwiązanie, musisz najpierw zaloguj się do swojego konta.

Aby zalogować się do konta Azure, należy użyć polecenia cmdlet **AzureRmAccount Dodaj** .

    Add-AzureRmAccount

Polecenie cmdlet monituje o podanie poświadczeń logowania dla Twojego konta Azure. Po zalogowaniu, go do pobrania ustawień konta tak, aby były dostępne dla Azure programu PowerShell. 

Ustawienia konta wygasa, więc należy je od czasu do czasu odświeżyć. Aby odświeżyć ustawienia kont, uruchom ponownie **Dodaj AzureRmAccount** . 

>[AZURE.NOTE] Moduły Menedżera zasobów wymaga AzureRmAccount Dodaj. Nie wystarcza pliku ustawienia publikowania.     

Jeśli masz więcej niż jedną subskrypcję, podaj identyfikator subskrypcji, którego chcesz użyć do wdrożenia przy użyciu polecenia cmdlet **Set-AzureRmContext** .

    Set-AzureRmContext -SubscriptionID <YourSubscriptionId>

## <a name="create-a-resource-group"></a>Tworzenie grupy zasobów

Przed wdrożeniem wszystkie zasoby do subskrypcji, musisz utworzyć grupa zasobów, która będzie zawierać zasobów. 

Aby utworzyć grupę zasobów, należy użyć polecenia cmdlet **New-AzureRmResourceGroup** .

Polecenie parametr **Nazwa** Określ nazwę dla grupy zasobów i parametr **Lokalizacja** określić jego lokalizacji. W zależności od możemy wykryte w poprzedniej sekcji, użyjemy "USA Zachód" dla lokalizacji.

    New-AzureRmResourceGroup -Name TestRG1 -Location "West US"
    
Wynik będzie wyglądać podobnie do:

    ResourceGroupName : TestRG1
    Location          : westus
    ProvisioningState : Succeeded
    Tags              :
    ResourceId        : /subscriptions/{guid}/resourceGroups/TestRG1

Grupy zasobów została utworzona.

## <a name="deploy-your-solution"></a>Wdrażanie rozwiązania

W tym temacie nie pokazano, jak utworzyć szablon lub omówić strukturę szablonu. Dla tych informacji zobacz [Szablony do tworzenia Menedżera zasobów Azure](resource-group-authoring-templates.md) i [Instruktaż szablonu Menedżera zasobów](resource-manager-template-walkthrough.md). Zostanie wdrożony wstępnie zdefiniowanego szablonu [Obsługa administracyjna aplikacji sieci Web z bazą danych SQL](https://azure.microsoft.com/documentation/templates/201-web-app-sql-database/) przy użyciu [Szablonów Szybki Start Azure](https://azure.microsoft.com/documentation/templates/).

Masz grupy zasobów i masz szablonu, dlatego jest już gotowe do wdrożenia infrastruktury zdefiniowane w szablonie do grupy zasobów. Wdrażanie zasobów za pomocą polecenia cmdlet **New-AzureRmResourceGroupDeployment** . Szablon określa wiele wartości domyślne, które użyjemy, więc nie musisz podać wartości dla tych parametrów. Podstawowa składnia wygląda:

    New-AzureRmResourceGroupDeployment -ResourceGroupName TestRG1 -administratorLogin exampleadmin -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-sql-database/azuredeploy.json 

Należy określić grupa zasobów i lokalizację szablonu. Jeśli szablon jest plik lokalny, możesz użyć parametru **- TemplateFile** i określ ścieżkę do szablonu. Można ustawić **-Tryb** parametr **przyrostowe** lub **ukończone**. Domyślnie Menedżer zasobów wykonuje przyrostowe aktualizacji podczas wdrażania; w związku z tym, nie jest istotne, aby ustawić **-Tryb** na potrzeby **przyrostowe**. Aby zrozumieć różnice między tymi trybami wdrażania, zobacz [Wdrażanie aplikacji za pomocą szablonu Azure Menedżera zasobów](resource-group-template-deploy.md). 

###<a name="dynamic-template-parameters"></a>Parametry dynamicznego szablonu

Użytkownicy zaznajomieni z programem PowerShell, pamiętaj, że możesz przełączać się pomiędzy parametrów dostępnych dla polecenia cmdlet, wpisując znak minus (-), a następnie naciśnięcie klawisza TAB. To samo ma zastosowanie również z parametrami, które zostały zdefiniowane w szablonie. Po zainicjowaniu wpisz nazwę szablonu, polecenia cmdlet pobiera szablon, analizuje go i dynamicznie dodaje parametrów szablonu do polecenia. Dzięki temu bardzo łatwo, określ wartości parametrów szablonu.

Po wprowadzeniu polecenia, zostanie wyświetlony monit o Brak parametru obowiązkowe **administratorLoginPassword**. A po wpisaniu hasła wartość ciągu bezpiecznego jest zasłonięty. Ta strategia eliminuje ryzyko podania hasła w formacie zwykłego tekstu.

    cmdlet New-AzureRmResourceGroupDeployment at command pipeline position 1
    Supply values for the following parameters:
    (Type !? for Help.)
    administratorLoginPassword: ********

Jeśli szablon zawiera parametr o nazwie, który pasuje do jednej z parametrów w poleceniu wdrożyć szablonu (na przykład w tym parametr o nazwie **ResourceGroupName** w szablonie, który jest taki sam jak parametr **ResourceGroupName** w polecenia cmdlet [New-AzureRmResourceGroupDeployment](https://msdn.microsoft.com/library/azure/mt679003.aspx) ), zostanie wyświetlony monit o podanie wartości parametru z przyrostek **FromTemplate** (na przykład **ResourceGroupNameFromTemplate**). Ogólnie należy unikać ten błąd, nie nadając parametrów taką samą nazwę jak parametry używane do operacji rozmieszczania.

Polecenie działa i zwraca wiadomości jako zasoby są tworzone. Ostatecznie zostanie wyświetlony wynik wdrażania.

    DeploymentName    : azuredeploy
    ResourceGroupName : TestRG1
    ProvisioningState : Succeeded
    Timestamp         : 4/11/2016 7:26:11 PM
    Mode              : Incremental
    TemplateLink      :
                Uri            : https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-sql-database/azuredeploy.json
                ContentVersion : 1.0.0.0
    Parameters        :
                Name             Type                       Value
                ===============  =========================  ==========
                skuName          String                     F1
                skuCapacity      Int                        1
                administratorLogin  String                  exampleadmin
                administratorLoginPassword  SecureString
                databaseName     String                     sampledb
                collation        String                     SQL_Latin1_General_CP1_CI_AS
                edition          String                     Basic
                maxSizeBytes     String                     1073741824
                requestedServiceObjectiveName  String       Basic

    Outputs           :
                Name             Type                       Value
                ===============  =========================  ==========
                siteUri          String                     websites5wdai7p2k2g4.azurewebsites.net
                sqlSvrFqdn       String                     sqlservers5wdai7p2k2g4.database.windows.net
                    
    DeploymentDebugLogLevel :

W zaledwie kilku kroków możemy utworzone i wdrożone zasoby wymagane złożonej witryny sieci Web. 

### <a name="log-debug-information"></a>Informacje dziennika debugowania

Wdrażając szablonu, można rejestrować dodatkowe informacje dotyczące żądania i odpowiedzi, określając parametr **- DeploymentDebugLogLevel** , gdy działa **AzureRmResourceGroupDeployment nowy**. Te informacje mogą ułatwić rozwiązywanie problemów z błędami wdrożenia. Wartość domyślna to **Brak** co oznacza żądanie nie lub zawartości odpowiedzi jest rejestrowany. Możesz określić logowania zawartość z żądania i odpowiedzi.  Aby uzyskać więcej informacji na temat rozwiązywania problemów wdrożeń i rejestrowania informacji debugowania zobacz [wdrożenia grupa zasobów Rozwiązywanie problemów z programem PowerShell Azure](resource-manager-troubleshoot-deployments-powershell.md). Poniższy przykład rejestruje zawartości żądania i zawartości odpowiedzi do wdrożenia.

    New-AzureRmResourceGroupDeployment -ResourceGroupName TestRG1 -DeploymentDebugLogLevel All -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-sql-database/azuredeploy.json 

> [AZURE.NOTE] Ustawiając parametr DeploymentDebugLogLevel, więc rozważyć typ danych, który jest przesyłany w podczas wdrażania. Przez rejestrowanie informacji na temat żądania lub odpowiedzi może być ujawnienie danych poufnych, które są pobierane za pośrednictwem operacji rozmieszczania. 


## <a name="get-information-about-your-resource-groups"></a>Uzyskiwanie informacji na temat grup zasobów

Po utworzeniu grupy zasobów, poleceń cmdlet w module Menedżera zasobów umożliwia zarządzanie grup zasobów.

- Aby uzyskać grupa zasobów w ramach subskrypcji, użyj polecenia cmdlet **Get-AzureRmResourceGroup** :

        Get-AzureRmResourceGroup -ResourceGroupName TestRG1
    
    Która zwraca następujące informacje:

        ResourceGroupName : TestRG1
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/{guid}/resourceGroups/TestRG
        
        ...

    Jeśli nie określisz nazwę grupy zasobów, polecenia cmdlet zwraca wszystkich grup zasobów w ramach subskrypcji.

- Aby uzyskać zasoby w grupie zasobów, użyj polecenia cmdlet **AzureRmResource znajdowanie** i jego parametr **ResourceGroupNameContains** . Bez parametrów Znajdź AzureRmResource pobiera wszystkie zasoby w ramach subskrypcji Azure.

        Find-AzureRmResource -ResourceGroupNameContains TestRG1
    
     Listę zasobów, która zwraca sformatowane, takich jak:
        
        Name              : sqlservers5wdai7p2k2g4
        ResourceId        : /subscriptions/{guid}/resourceGroups/TestRG1/providers/Microsoft.Sql/servers/sqlservers5wdai7p2k2g4
        ResourceName      : sqlservers5wdai7p2k2g4
        ResourceType      : Microsoft.Sql/servers
        Kind              : v2.0
        ResourceGroupName : TestRG1
        Location          : westus
        SubscriptionId    : {guid}
        Tags              : {System.Collections.Hashtable}
        ...
            
- Za pomocą znaczników logicznie organizowanie zasobów w ramach subskrypcji i pobrać zasobów za pomocą polecenia cmdlet **AzureRmResource Znajdź** i **Znajdź AzureRmResourceGroup** .

        Find-AzureRmResource -TagName displayName -TagValue Website

        Name              : webSites5wdai7p2k2g4
        ResourceId        : /subscriptions/{guid}/resourceGroups/TestRG1/providers/Microsoft.Web/sites/webSites5wdai7p2k2g4
        ResourceName      : webSites5wdai7p2k2g4
        ResourceType      : Microsoft.Web/sites
        ResourceGroupName : TestRG1
        Location          : westus
        SubscriptionId    : {guid}
                
      There is much more you can do with tags. For more information, see [Using tags to organize your Azure resources](resource-group-using-tags.md).

## <a name="add-to-a-resource-group"></a>Dodawanie do grupy zasobów

Aby dodać zasób do grupy zasobów, można użyć polecenia cmdlet **New-AzureRmResource** . Jednak dodanie zasobu w ten sposób mogą powodować przyszłych zamieszania, ponieważ nowy zasób nie istnieje w szablonie. Jeśli ponownie wdrożono starego szablonu, czy wdrożyć rozwiązanie niekompletna. Jeśli wdrażanie często spowoduje znalezienie łatwiejsze i bardziej niezawodne Dodawanie nowego zasobu do szablonu i ponownie wdrożyć go.

## <a name="move-a-resource"></a>Przenoszenie zasobu

Możesz przenieść istniejące zasoby do nowej grupy zasobów. Przykłady zobacz [Przenoszenie zasoby do nowej grupy zasobów lub innej subskrypcji](resource-group-move-resources.md).

## <a name="export-template"></a>Eksportowanie szablonu

W przypadku istniejącej grupy zasobów (wdrożony za pomocą programu PowerShell lub jeden z innych metod, takich jak portalu) można wyświetlać szablonu Menedżera zasobów dla grupy zasobów. Eksportowanie szablon oferuje dwie korzyści:

1. Można łatwo zautomatyzować przyszłych wdrożeń rozwiązanie, ponieważ wszystkie infrastruktury jest zdefiniowana w szablonie.

2. Czy zapoznanie się ze składni szablonu, wyświetlając w notacji obiektu JavaScript (JSON), reprezentująca rozwiązania.

Przy użyciu programu PowerShell możesz wygenerować szablonu, który odpowiada bieżącemu stanowi grupy zasobów lub pobrać szablon, którego użyto do określonego wdrożenia.

Eksportowanie szablonu dla grupy zasobów jest przydatne, gdy do grupy zasobów zostały wprowadzone zmiany, a następnie należy pobrać reprezentacją JSON jego bieżącym stanie. Jednak wygenerowane szablon zawiera tylko z minimalnej liczby parametrów i Brak zmiennych. Większość wartości w szablonie są stałe. Przed wdrożeniem wygenerowane szablonu, możesz przekonwertować więcej wartości parametrów, możesz dostosować wdrożenia w różnych środowiskach.

Eksportowanie szablonu dla danego wdrożenia jest przydatne, gdy zachodzi potrzeba wyświetlenia rzeczywisty szablon, którego użyto do wdrożenia zasobów. Szablon będzie zawierać wszystkie parametry i zmienne zdefiniowane wdrożenia oryginalny. Jednak jeśli ma inną osobę w organizacji wprowadzone zmiany do grupy zasobów poza co to jest definiowana w szablonie, ten szablon nie będzie reprezentował bieżący stan grupy zasobów.

> [AZURE.NOTE] Funkcja szablonu eksportu jest w podglądzie, a nie we wszystkich typach zasobów jest obecnie obsługiwany eksportowania szablonu. Podczas próby eksportowania szablonu, może zostać wyświetlony komunikat o błędzie informujący, że niektóre zasoby nie zostały wyeksportowane. W razie potrzeby można ręcznie zdefiniować tych zasobów w szablonie, po pobraniu go.

###<a name="export-template-from-resource-group"></a>Eksportowanie szablonu z Grupa zasobów

Aby wyświetlić szablonu dla grupy zasobów, uruchom polecenie cmdlet **AzureRmResourceGroup eksportu** .

    Export-AzureRmResourceGroup -ResourceGroupName TestRG1 -Path c:\Azure\Templates\Downloads\TestRG1.json
    
###<a name="download-template-from-deployment"></a>Pobieranie szablonu z wdrażania

Aby pobrać szablon używany dla danego wdrożenia, uruchom polecenie cmdlet **AzureRmResourceGroupDeploymentTemplate Zapisz** .

    Save-AzureRmResourceGroupDeploymentTemplate -DeploymentName azuredeploy -ResourceGroupName TestRG1 -Path c:\Azure\Templates\Downloads\azuredeploy.json

## <a name="delete-resources-or-resource-group"></a>Usuwanie zasoby lub grupa zasobów

- Aby usunąć zasób z Grupa zasobów, należy użyć polecenia cmdlet **AzureRmResource Usuń** . To polecenie cmdlet Usuwa zasób, ale nie powoduje usunięcia grupy zasobów.

    To polecenie usuwa Witryna_testowa witryny sieci Web z grup zasobów TestRG1.

        Remove-AzureRmResource -Name TestSite -ResourceGroupName TestRG1 -ResourceType "Microsoft.Web/sites" -ApiVersion 2015-08-01

- Aby usunąć grupę zasobów, należy użyć polecenia cmdlet **AzureRmResourceGroup Usuń** . To polecenie cmdlet usuwa grupa zasobów i jego zasobów.

        Remove-AzureRmResourceGroup -Name TestRG1
        
    Zostanie wyświetlony monit o potwierdzenie usunięcia.
        
        Confirm
        Are you sure you want to remove resource group 'TestRG1'
        [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y

## <a name="deployment-script"></a>Skrypt wdrożenia

Wcześniejszych przykłady wdrożenia w tym temacie były wyświetlane tylko poszczególne polecenia cmdlet wymagane do wdrożenia zasoby Azure. W poniższym przykładzie pokazano skrypt wdrożenia, który tworzy grupę zasobów i wdrożeniu jej zasobów.

    <#
      .SYNOPSIS
      Deploys a template to Azure
      
      .DESCRIPTION
      Deploys an Azure Resource Manager template

      .PARAMETER subscriptionId
      The subscription id where the template will be deployed.

      .PARAMETER resourceGroupName
      The resource group where the template will be deployed. Can be the name of an existing or a new resource group.

      .PARAMETER resourceGroupLocation
      Optional, a resource group location. If specified, will try to create a new resource group in this location. If not specified, assumes resource group is existing.

      .PARAMETER deploymentName
      The deployment name.

      .PARAMETER templateFilePath
      Optional, path to the template file. Defaults to template.json.

      .PARAMETER parametersFilePath
      Optional, path to the parameters file. Defaults to parameters.json. If file is not found, will prompt for parameter values based on template.
    #>

    param(
      [Parameter(Mandatory=$True)]
      [string]
      $subscriptionId,

      [Parameter(Mandatory=$True)]
      [string]
      $resourceGroupName,

      [string]
      $resourceGroupLocation,

      [Parameter(Mandatory=$True)]
      [string]
      $deploymentName,

      [string]
      $templateFilePath = "template.json",

      [string]
      $parametersFilePath = "parameters.json"
    )

    #******************************************************************************
    # Script body
    # Execution begins here 
    #******************************************************************************
    $ErrorActionPreference = "Stop"

    # sign in
    Write-Host "Logging in...";
    Add-AzureRmAccount;

    # select subscription
    Write-Host "Selecting subscription '$subscriptionId'";
    Set-AzureRmContext -SubscriptionID $subscriptionId;

    #Create or check for existing resource group
    $resourceGroup = Get-AzureRmResourceGroup -Name $resourceGroupName -ErrorAction SilentlyContinue
    if(!$resourceGroup)
    {
      Write-Host "Resource group '$resourceGroupName' does not exist. To create a new resource group, please enter a location.";
      if(!$resourceGroupLocation) {
        $resourceGroupLocation = Read-Host "resourceGroupLocation";
      }
      Write-Host "Creating resource group '$resourceGroupName' in location '$resourceGroupLocation'";
      New-AzureRmResourceGroup -Name $resourceGroupName -Location $resourceGroupLocation
    }
    else{
      Write-Host "Using existing resource group '$resourceGroupName'";
    }

    # Start the deployment
    Write-Host "Starting deployment...";
    if(Test-Path $parametersFilePath) {
      New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateFile $templateFilePath -TemplateParameterFile $parametersFilePath;
    } else {
      New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateFile $templateFilePath;
    }

## <a name="next-steps"></a>Następne kroki

- Aby uzyskać informacje dotyczące tworzenia szablonów Menedżera zasobów, zobacz [Tworzenie szablonów Menedżera zasobów Azure](./resource-group-authoring-templates.md).
- Aby uzyskać informacje o wdrażanie szablonów, zobacz temat [Deploy aplikacji za pomocą szablonu Menedżera zasobów Azure](./resource-group-template-deploy.md).
- Aby uzyskać szczegółowy przykład wdrażania projektu zobacz [microservices rozmieszczanie właściwie platformy Azure](app-service-web/app-service-deploy-complex-application-predictably.md).
- Aby dowiedzieć się, rozwiązywanie problemów z wdrażaniem, które nie, zobacz [Rozwiązywanie problemów z zasobów grupy wdrażania Azure](./resource-manager-troubleshoot-deployments-powershell.md).

