<properties
   pageTitle="Środowiska projektowego i testowego | Microsoft Azure"
   description="Dowiedz się, jak szybko i spójne tworzenie i usuwanie rozwoju i przetestować środowiskach przy użyciu szablonów Azure Menedżera zasobów."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="01/22/2016"
   ms.author="tomfitz"/>

# <a name="development-and-test-environments-in-microsoft-azure"></a>Środowiska projektowego i testowego Microsoft Azure

Niestandardowe aplikacje są zwykle wdrożony w wielu środowisk projektowych i testowych przed wdrożeniem do produkcji. Podczas tworzenia środowiska w środowisku lokalnym, obliczeniowych są kupowane lub przydzielone dla każdego środowiska dla każdej aplikacji. Środowiska często zawierają kilka fizycznej lub maszyn wirtualnych z określonej konfiguracji, które są rozmieszczane ręcznie lub skryptów złożonych automatyzacji. We wdrożeniach często zająć godzin i powodują niespójnej konfiguracji w środowiskach.

## <a name="scenario"></a>Scenariusz ##

Gdy przepisu rozwoju i środowiskach testowych platformy Microsoft Azure tylko płatność dla zasobów, których używasz.  W tym artykule wyjaśniono, jak szybko i spójne można tworzyć, zachować i Usuń rozwoju i przetestować środowiskach za pomocą Menedżera zasobów Azure szablony i pliki parametr, jak pokazano poniżej.

![Scenariusz](./media/solution-dev-test-environments/scenario.png)

Trzy środowisk projektowych i testowych są wyświetlane powyżej.  Każda ma aplikacji sieci web i zasoby bazy danych SQL, które są określane w postaci pliku szablonu.  Nazwy aplikacji i bazy danych w każdym środowisku są różne i są określane w postaci plików parametr unikatowe dla każdego środowiska.  

Jeśli nie znasz pojęcia Menedżera zasobów Azure, zalecane jest, przeczytaj artykuł [Omówienie Menedżera zasobów Azure](azure-resource-manager/resource-group-overview.md) przed przeczytaniem tego artykułu.

Warto najpierw należy przejść do opisanych w tym artykule wymienione bez czytania, z której dotyczy odwołanie artykułów, aby szybko uzyskać niektóre możliwości korzystania z szablonów Azure Menedżera zasobów. Po pozostajesz kroków raz, będziesz mieć możliwość odpowiedzi na większość pytań, które powstały imienia czasu za pośrednictwem Eksperymentując dalszych czynności i czytając artykuły, której dotyczy odwołanie.

## <a name="plan-azure-resource-use"></a>Planowanie użycia zasobów Azure
Po utworzeniu projektu wysokiego poziomu dla aplikacji można zdefiniować:

- Zasoby, które Azure aplikacja będzie zawierać. Możesz utworzyć aplikację i wdrażać go w aplikacji sieci Web programu Azure z bazy danych SQL Azure.  Może tworzyć aplikacji w środowisku maszyn wirtualnych za pomocą PHP i MySQL lub usług IIS i SQL Server lub inne składniki. [Azure aplikacji usługi, usług w chmurze i porównania maszyn wirtualnych]( app-service-web/choose-web-site-cloud-service-vm.md) artykuł ułatwia ustalenie Azure zasoby, które warto korzystać z aplikacji.
- Jakie usługi poziomu wymagania takich jak dostępność, bezpieczeństwo i skali spełniających aplikacji.

## <a name="download-an-existing-template"></a>Pobierz istniejącego szablonu
Szablon Menedżera zasobów Azure definiuje wszystkie zasoby Azure, które wykorzystuje aplikacji. Kilka szablonów już istnieje, można wdrożyć bezpośrednio w portalu Azure lub pobranie, modyfikować i zapisać w systemie kontroli źródła w kodzie aplikacji.  Wykonaj poniższe czynności, aby pobrać istniejącego szablonu.

1. Przeglądanie istniejących szablonów w repozytorium GitHub [Azure Szybki Start szablonów](https://github.com/Azure/azure-quickstart-templates/) . Na liście pojawi się folder "[201--aplikacji sql — baza danych sieci web](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-sql-database)". Ponieważ wiele aplikacji niestandardowe obejmują aplikacji sieci web i baza danych SQL, ten szablon jest używany jako przykład w dalszej części tego artykułu ułatwiające zrozumienie sposobu korzystania z szablonów. Jest spoza zakresu tego artykułu, aby w pełni wyjaśnić wszystko ten szablon umożliwia utworzenie i konfiguruje, ale jeśli planujesz za jej pomocą tworzyć rzeczywisty środowiskach w Twojej organizacji, warto lepiej zrozumieć, czytając artykuł [Obsługa administracyjna aplikacji sieci web z bazą danych SQL](app-service-web/app-service-web-arm-with-sql-database-provision.md) .
Uwaga: w tym artykule został zapisany w wersji grudnia 2015 szablonu [201--aplikacji sql — baza danych sieci web](https://github.com/Azure/azure-quickstart-templates/tree/3f24f7b7e1e377538d1d548eaa6eab2851a21810/201-web-app-sql-database) . Łączy poniżej wskaż szablon i plików z parametrami są tej wersji szablonu.
2. Kliknij plik [azuredeploy.json](https://github.com/Azure/azure-quickstart-templates/tree/3f24f7b7e1e377538d1d548eaa6eab2851a21810/201-web-app-sql-database/azuredeploy.json) w folderze 201--aplikacji sql — baza danych sieci web, aby wyświetlić jego zawartość. To jest plik szablonu Azure Menedżera zasobów. 
3. W trybie widoku kliknij przycisk "[Raw](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/3f24f7b7e1e377538d1d548eaa6eab2851a21810/201-web-app-sql-database/azuredeploy.json)". 
4. Przy użyciu myszy wybierz zawartość tego pliku, a następnie zapisać na komputerze, w pliku o nazwie "TestApp1 Template.json." 
5. Sprawdź zawartość szablonu i zwróć uwagę na następujące czynności:
 - **Zasoby** sekcji: w tej sekcji zdefiniowano typy wymaganych zasobów Azure utworzone za pomocą tego szablonu. Wśród innych typów zasobów ten szablon tworzy [Azure aplikacji sieci Web](app-service-web/app-service-web-overview.md) i [Bazy danych SQL Azure](sql-database/sql-database-technical-overview.md) zasoby. Jeśli wolisz uruchamianie i zarządzanie nimi w sieci web i serwery SQL Server w środowisku maszyn wirtualnych, można używać szablonów "[usług iis-2vm-sql-1vm](https://github.com/Azure/azure-quickstart-templates/tree/master/iis-2vm-sql-1vm)" lub "[światło aplikacji](https://github.com/Azure/azure-quickstart-templates/tree/master/lamp-app)", ale z instrukcjami podanymi w tym artykule są oparte na tym szablonie [201--aplikacji sql — baza danych sieci web](https://github.com/Azure/azure-quickstart-templates/tree/3f24f7b7e1e377538d1d548eaa6eab2851a21810/201-web-app-sql-database) .
 - Sekcja **Parametry** : w tej sekcji określa parametry, które można skonfigurować każdego zasobu. Niektóre parametry określone w szablonie mają właściwości "wartość domyślna", gdy inne osoby nie. Wdrażając Azure zasobów za pomocą szablonu, należy podać wartości dla wszystkich parametrów, które nie mają określonego w szablonie właściwości wartość domyślna.  Jeśli nie podasz wartości parametrów z właściwości wartość domyślna, jest używana wartość określony w parametrze defaultValue w szablonie.

Szablon określa Azure zasoby, które są tworzone i parametry poszczególnych zasobów można skonfigurować. Przedstawiono więcej informacji na temat szablonów i sposób projektowania własnych przez w artykule [Najważniejsze wskazówki dotyczące projektowania szablonów Azure Menedżera zasobów](best-practices-resource-manager-design-templates.md) .

## <a name="download-and-customize-an-existing-parameter-file"></a>Pobieranie i dostosowywanie istniejącego pliku parametru

Chociaż prawdopodobnie warto w *tym samym* Azure zasobów utworzone w poszczególnych środowisk, prawdopodobnie warto konfiguracji zasobów, które mają *różne* w poszczególnych środowisk.  To miejsce, w którym mają plików z parametrami. Tworzenie parametru plików, które zawierają unikatowe wartości w poszczególnych środowisk, wykonując poniższe czynności.   

1. Wyświetlanie zawartości pliku [azuredeploy.parameters.json](https://github.com/Azure/azure-quickstart-templates/tree/3f24f7b7e1e377538d1d548eaa6eab2851a21810/201-web-app-sql-database/azuredeploy.parameters.json) w folderze 201--aplikacji sql — baza danych sieci web. Jest to plik parametru pliku szablonu, który został zapisany w poprzedniej sekcji. 
2. W trybie widoku kliknij przycisk "[Raw](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/3f24f7b7e1e377538d1d548eaa6eab2851a21810/201-web-app-sql-database/azuredeploy.parameters.json)". 
3. Przy użyciu myszy wybierz zawartość tego pliku i zapisywanie ich w trzech osobnych plików na komputerze z następujących nazw:
 - TestApp1 — parametry Development.json
 - TestApp1 — parametry Test.json
 - TestApp1 — parametry Pre-Production.json

3. Za pomocą dowolnego tekstu lub edytora JSON, edytować plik parametr środowiska rozwój, który został utworzony w kroku 3, zamieniając wartości na liście po prawej stronie wartości parametrów w pliku o *wartości* na liście po prawej stronie **parametrów** poniżej: 
 - **Nazwa witryny**: *TestApp1DevApp*
 - **hostingPlanName**: *TestApp1DevPlan*
 - **siteLocation**: *Centralnej Stany Zjednoczone*
 - **nazwa_serwera**: *testapp1devsrv*
 - **serverLocation**: *Centralnej Stany Zjednoczone*
 - **administratorLogin**: *testapp1Admin*
 - **administratorLoginPassword**: *Zamień za pomocą hasła*
 - **databaseName**: *testapp1devdb*

4. Za pomocą dowolnego tekstu lub edytora JSON edytować plik parametr środowiska Test został utworzony w kroku 3, zamieniając wartości na liście po prawej stronie wartości parametrów w pliku o *wartości* na liście po prawej stronie **parametrów** poniżej:
 - **Nazwa witryny**: *TestApp1TestApp*
 - **hostingPlanName**: *TestApp1TestPlan*
 - **siteLocation**: *Centralnej Stany Zjednoczone*
 - **nazwa_serwera**: *testapp1testsrv*
 - **serverLocation**: *Centralnej Stany Zjednoczone*
 - **administratorLogin**: *testapp1Admin*
 - **administratorLoginPassword**: *Zamień za pomocą hasła*
 - **databaseName**: *testapp1testdb*

5. Za pomocą dowolnego tekstu lub edytora JSON, edytować plik przygotowań parametr, który został utworzony w kroku 3.  Co to jest poniżej zamienić całą zawartość plik:

        {
          "$schema" : "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
          "contentVersion" : "1.0.0.0",
          "parameters" : {
        "administratorLogin" : {
          "value" : "testApp1Admin"
        },
        "administratorLoginPassword" : {
          "value" : "replace with your password"
        },
        "databaseName" : {
          "value" : "testapp1preproddb"
        },
        "hostingPlanName" : {
          "value" : "TestApp1PreProdPlan"
        },
        "serverLocation" : {
          "value" : "Central US"
        },
        "serverName" : {
          "value" : "testapp1preprodsrv"
        },
        "siteLocation" : {
          "value" : "Central US"
        },
        "siteName" : {
          "value" : "TestApp1PreProdApp"
        },
        "sku" : {
          "value" : "Standard"
        },
            "requestedServiceObjectiveName" : {
              "value" : "S1"
        }
          }
        }

W pliku parametrów przygotowań powyżej parametry **sku** i **requestedServiceObjectiveName** zostały *dodane*, nie zostały one dodane w plikach parametrów projektowania i testowania. To, ponieważ nie istnieją wartości domyślne określone dla parametrów w szablonie i środowisk programowania i testowania używane są wartości domyślne, ale w fazie przygotowań są używane wartości inne niż domyślne środowisko dla tych parametrów.

Przyczyny, dla której wartości inne niż domyślne są używane dla parametrów w środowisku produkcyjnym przed służy do testowania wartości dla tych parametrów, które można wybrać dla środowisku produkcyjnym, również można zbadać.  Wszystkie parametry te dotyczą Azure [Plany hostingu aplikacji Web App](https://azure.microsoft.com/pricing/details/app-service/)lub **sku** i Azure [SQL Database](https://azure.microsoft.com/pricing/details/sql-database/)lub **requestedServiceObjectiveName** , które są używane przez aplikację.  Różne wersje produktu i nazwy była usług mają różne koszty i funkcje i obsługa techniczna metryki poziomu innej usługi.

Poniższa tabela zawiera listę wartości domyślnych dla tych parametrów określonych w szablonie i wartości, które są używane zamiast wartości domyślne w pliku przygotowań parametrów.

| Parametr | Wartość domyślna | Wartość parametru pliku |
|---|---|---|
| **Jednostka SKU** | Bezpłatne | Standardowe |
| **requestedServiceObjectiveName** | S0 | S1 |

## <a name="create-environments"></a>Tworzyć środowiska
Wszystkie zasoby Azure należy utworzyć w obrębie [Grupy zasobów Azure](azure-resource-manager/resource-group-overview.md). Grupy zasobów umożliwiają pogrupuj Azure zasobów, aby wspólnie zarządzane.  Tak, aby określonym osobom w organizacji może tworzenie, modyfikowanie, usuwanie lub wyświetlanie ich i zasoby w nich można przypisywać [uprawnienia](./active-directory/role-based-access-built-in-roles.md) do grup zasobów.  Alerty i informacje rozliczeniowe dla zasobów w grupie zasobów można wyświetlić w [Azure Portal](https://portal.azure.com). Grupy zasobów są tworzone w Azure [region](https://azure.microsoft.com/regions/).  W tym artykule wszystkie zasoby są tworzone w regionie centralnej USA. Po rozpoczęciu tworzenia rzeczywisty środowiskach, wybraniu regionu, który najlepiej odpowiada potrzebom. 

Tworzenie grup zasobów dla każdego środowiska za pomocą dowolnej z poniższych metod.  Wszystkie metody osiągnie taka sama.

###<a name="azure-command-line-interface-cli"></a>Azure interfejs wiersza polecenia (polecenie)

Upewnij się, że masz polecenie [zainstalowany](xplat-cli-install.md) albo systemu Windows, OS X lub Linux komputer, a które zostały [połączone](xplat-cli-connect.md) [konto Azure AD](./active-directory/active-directory-how-subscriptions-associated-directory.md) (zwanych również konto służbowe) do subskrypcji usługi Azure. W wierszu polecenia interfejsu wiersza polecenia wpisz poniższe polecenie, aby utworzyć grupę zasobów dla środowiska projektowego.

    azure group create "TestApp1-Development" "Central US"

Polecenie zwróci następujące czynności, jeśli skutku:

    info:    Executing command group create
    + Getting resource group TestApp1-Development
    + Creating resource group TestApp1-Development
    info:    Created resource group TestApp1-Development
    data:    Id:                  /subscriptions/uuuuuuuu-vvvv-wwww-xxxx-yyyy-zzzzzzzzzzzz/resourceGroups/TestApp1-Development
    data:    Name:                TestApp1-Development
    data:    Location:            centralus
    data:    Provisioning State:  Succeeded
    data:    Tags: null
    data:
    info:    group create command OK

Aby utworzyć grupę zasobów dla środowisku testowym, wpisz poniższe polecenie:

    azure group create "TestApp1-Test" "Central US"

Aby utworzyć grupę zasobów dla środowiska przygotowań, wpisz poniższe polecenie:

    azure group create "TestApp1-Pre-Production" "Central US"

###<a name="powershell"></a>Programu PowerShell

Upewnij się, że tekst 1.01 programu PowerShell Azure lub wyższej zainstalowany na komputerze z systemem Windows i połączono [konta Azure AD](./active-directory/active-directory-how-subscriptions-associated-directory.md) (zwanych również konto służbowe) do subskrypcji wyszczególnione w artykule [jak zainstalować i skonfigurować Azure programu PowerShell](powershell-install-configure.md) . W wierszu polecenia programu PowerShell wpisz poniższe polecenie, aby utworzyć grupę zasobów dla środowiska projektowego.

    New-AzureRmResourceGroup -Name TestApp1-Development -Location "Central US"

Polecenie zwróci następujące czynności, jeśli skutku:

    ResourceGroupName : TestApp1-Development
    Location          : centralus
    ProvisioningState : Succeeded
    Tags              :
    ResourceId        : /subscriptions/uuuuuuuu-vvvv-wwww-xxxx-yyyy-zzzzzzzzzzzz/resourceGroups/TestApp1-Development

Aby utworzyć grupę zasobów dla środowisku testowym, wpisz poniższe polecenie:

    New-AzureRmResourceGroup -Name TestApp1-Test -Location "Central US"

Aby utworzyć grupę zasobów dla środowiska przygotowań, wpisz poniższe polecenie:

    New-AzureRmResourceGroup -Name TestApp1-Pre-Production -Location "Central US"

###<a name="azure-portal"></a>Azure portal

1. Zaloguj się do [portalu Azure](https://portal.azure.com) za pomocą konta [Azure AD](./active-directory/active-directory-how-subscriptions-associated-directory.md) (nazywane również służbowe). Kliknij pozycję Nowy--> Zarządzanie--> Grupa zasobów i wprowadź "TestApp1 Development" w polu Nazwa zasobu grupy, wybierz swoją subskrypcję i wybierz "Centralnej z NAMI" w polu Lokalizacja grupa zasobów, jak pokazano na poniższym obrazie.
   ![Portal](./media/solution-dev-test-environments/rgcreate.png)
2. Kliknij przycisk Utwórz, aby utworzyć grupę zasobów.
3. Kliknij przycisk Przeglądaj, przewiń listę do grup zasobów i kliknij pozycję grupy zasobów tak jak pokazano poniżej.
   ![Portal](./media/solution-dev-test-environments/rgbrowse.png) 
4. Po kliknięciu grupy zasobów zostanie wyświetlona karta grup zasobów z nowej grupy zasobów.
   ![Portal](./media/solution-dev-test-environments/rgview.png)
5. Tworzenie teście TestApp1 i zasobów TestApp1 litery Pre produkcji grupy tak samo utworzonych grup zasobów rozwoju TestApp1 powyżej.

##<a name="deploy-resources-to-environments"></a>Wdrażanie zasobów do środowisk

Wdrażanie Azure zasobów do grup zasobów dla każdego środowiska przy użyciu pliku szablonu rozwiązania i plików z parametrami dla każdego środowiska przy użyciu jednej z następujących metod.  Obie metody osiągnie taka sama.

###<a name="azure-command-line-interface-cli"></a>Azure interfejs wiersza polecenia (polecenie)

W wierszu polecenia polecenie wpisz poniższe polecenie, aby wdrożyć zasobów do grupy zasobów utworzonej dla środowiska programowania, zamieniając [ścieżka] ścieżka do plików zapisanych w poprzednich krokach.

    azure group deployment create -g TestApp1-Development -n Deployment1 -f [path]TestApp1-Template.json -e [path]TestApp1-Parameters-Development.json 

Przed wykonaniem komunikat "Trwa oczekiwanie na wdrożenie do wykonania" przez kilka minut, polecenie zwróci następujące Jeśli skutku:

    info:    Executing command group deployment create
    + Initializing template configurations and parameters
    + Creating a deployment
    info:    Created template deployment "Deployment1"
    + Waiting for deployment to complete
    data:    DeploymentName     : Deployment1
    data:    ResourceGroupName  : TestApp1-Development
    data:    ProvisioningState  : Succeeded
    data:    Timestamp          : XXXX-XX-XXT20:20:23.5202316Z
    data:    Mode               : Incremental
    data:    Name                           Type          Value
    data:    -----------------------------  ------------  ----------------------------
    data:    siteName                       String        TestApp1DevApp
    data:    hostingPlanName                String        TestApp1DevPlan
    data:    siteLocation                   String        Central US
    data:    sku                            String        Free
    data:    workerSize                     String        0
    data:    serverName                     String        testapp1devsrv
    data:    serverLocation                 String        Central US
    data:    administratorLogin             String        testapp1Admin
    data:    administratorLoginPassword     SecureString  undefined
    data:    databaseName                   String        testapp1devdb
    data:    collation                      String        SQL_Latin1_General_CP1_CI_AS
    data:    edition                        String        Standard
    data:    maxSizeBytes                   String        1073741824
    data:    requestedServiceObjectiveName  String        S0
    info:    group deployment create command OKx

Jeśli to polecenie nie działa, komunikatami o błędach i spróbuj go ponownie.  Typowe problemy używają wartości parametrów, które nie są zgodne z ograniczeniami nazewnictwa Azure zasobów. Inne porady dotyczące rozwiązywania problemów można znaleźć w artykule [Rozwiązywanie problemów z zasobów grupy wdrażania Azure](./resource-manager-troubleshoot-deployments-cli.md) .

W wierszu polecenia polecenie wpisz poniższe polecenie, aby wdrożyć zasobów do grupy zasobów utworzonej dla środowiska Test, zamieniając [ścieżka] ścieżka do plików zapisanych w poprzednich krokach.

    azure group deployment create -g TestApp1-Test -n Deployment1 -f [path]TestApp1-Template.json -e [path]TestApp1-Parameters-Test.json

W wierszu polecenia polecenie wpisz poniższe polecenie, aby wdrożyć zasobów do grupy zasobów utworzonej dla środowiska przygotowań, zamieniając [ścieżka] ścieżka do plików zapisanych w poprzednich krokach.

    azure group deployment create -g TestApp1-Pre-Production -n Deployment1 -f [path]TestApp1-Template.json -e [path]TestApp1-Parameters-Pre-Production.json
  
###<a name="powershell"></a>Programu PowerShell

Z wiersza polecenia programu PowerShell Azure (wersja 1.01 lub nowszy) wpisz poniższe polecenie, aby wdrożyć zasobów do grupy zasobów utworzonej dla środowiska programowania, zamieniając [ścieżka] ścieżka do plików zapisanych w poprzednich krokach.

    New-AzureRmResourceGroupDeployment -ResourceGroupName TestApp1-Development -TemplateFile [path]TestApp1-Template.json -TemplateParameterFile [path]TestApp1-Parameters-Development.json -Name Deployment1 

Przed wykonaniem migający kursor przez kilka minut, polecenie zwróci następujące Jeśli skutku:

    DeploymentName    : Deployment1
    ResourceGroupName : TestApp1-Development
    ProvisioningState : Succeeded
    Timestamp         : XX/XX/XXXX 2:44:48 PM
    Mode              : Incremental
    TemplateLink      : 
    Parameters        : 
                        Name             Type                       Value     
                        ===============  =========================  ==========
                        siteName         String                     TestApp1DevApp
                        hostingPlanName  String                     TestApp1DevPlan
                        siteLocation     String                     Central US
                        sku              String                     Free      
                        workerSize       String                     0         
                        serverName       String                     testapp1devsrv
                        serverLocation   String                     Central US
                        administratorLogin  String                     testapp1Admin
                        administratorLoginPassword  SecureString                         
                        databaseName     String                     testapp1devdb
                        collation        String                     SQL_Latin1_General_CP1_CI_AS
                        edition          String                     Standard  
                        maxSizeBytes     String                     1073741824
                        requestedServiceObjectiveName  String                     S0        
                        
    Outputs           :

  Jeśli to polecenie nie działa, komunikatami o błędach i spróbuj go ponownie.  Typowe problemy używają wartości parametrów, które nie są zgodne z ograniczeniami nazewnictwa Azure zasobów. Inne porady dotyczące rozwiązywania problemów można znaleźć w artykule [Rozwiązywanie problemów z zasobów grupy wdrażania Azure](./resource-manager-troubleshoot-deployments-powershell.md) .

  W wierszu polecenia programu PowerShell wpisz poniższe polecenie, aby wdrożyć zasobów do grupy zasobów utworzonej dla środowiska Test, zamieniając [ścieżka] ścieżka do plików zapisanych w poprzednich krokach.

    New-AzureRmResourceGroupDeployment -ResourceGroupName TestApp1-Test -TemplateFile [path]TestApp1-Template.json -TemplateParameterFile [path]TestApp1-Parameters-Test.json -Name Deployment1

  W wierszu polecenia programu PowerShell wpisz poniższe polecenie, aby wdrożyć zasobów do grupy zasobów utworzonej dla środowiska przygotowań, zamieniając [ścieżka] ścieżka do plików zapisanych w poprzednich krokach.

    New-AzureRmResourceGroupDeployment -ResourceGroupName TestApp1-Pre-Production -TemplateFile [path]TestApp1-Template.json -TemplateParameterFile [path]TestApp1-Parameters-Pre-Production.json -Name Deployment1

Pliki szablonu i parametr może być wersji i obsługiwane w kodzie aplikacji w systemie kontroli źródła.  Można również zapisać powyżej, aby pliki skryptów i zapisywanie ich w kodzie także poleceń.

> [AZURE.NOTE] Szablon można wdrażać Azure bezpośrednio, klikając przycisk "Wdrażanie do Azure" w artykule [Obsługa administracyjna aplikacji sieci Web z bazą danych SQL](https://azure.microsoft.com/documentation/templates/201-web-app-sql-database/) .  Może być pomocne informacje na temat szablonów, ale zrobić tak nie umożliwiają edytowanie wersja i Zapisz do szablonu i wartości parametrów w kodzie aplikacji w celu dalszej szczegółowe informacje na temat ta metoda nie jest omówione w tym artykule.

## <a name="maintain-environments"></a>Obsługa środowiska
W całym rozwoju konfiguracji Azure zasobów w różnych środowiskach może być niespójnie zmieniony celowo lub przypadkowo.  Może to powodować niepotrzebne rozwiązywania problemów i rozwiązywanie problemów cyklu opracowywania aplikacji.

1. Zmienianie środowiska, otwierając [Azure portal](https://portal.azure.com). 
2. Zaloguj się do niego za pomocą tego samego konta, którego użyto do wykonaj powyższe kroki. 
3. Jak pokazano na obraz poniżej, kliknij przycisk Przeglądaj--> grupy zasobów (może być konieczne przewinięcie w dół w celu wyświetlenia grup zasobów).
   ![Portal](./media/solution-dev-test-environments/rgbrowse.png)
4. Po kliknięciu grup zasobów na powyższym obrazie, zostanie wyświetlona karta grup zasobów i grup zasobów trzy, utworzony w poprzednim kroku, jak pokazano na poniższym obrazie. Kliknij grupę zasobów TestApp1 rozwoju i pojawi się kartę, która zawiera listę zasobów utworzone za pomocą szablonu w ramach rozwoju TestApp1 wdrożenia grupa zasobów, wykonywane w poprzednim kroku.  Usuwanie zasobu TestApp1DevApp Web App, klikając TestApp1DevApp karta Grupa zasobów TestApp1 rozwoju i klikając polecenie Usuń na karta aplikacji sieci Web TestApp1DevApp.
   ![Portal](./media/solution-dev-test-environments/portal2.png)
5. Kliknij przycisk "Tak", gdy portalu zostanie wyświetlony monit, czy wiesz, że chcesz usunąć zasób. Karta Grupa zasobów rozwoju TestApp1 zamknięcie i ponowne otwarcie go teraz wyświetli ją bez aplikacji sieci Web, który został usunięty.  Zawartość grupy zasobów jest teraz inny niż należy je. Możesz dodatkowo wypróbować usuwania wielu zasobów z wielu grup zasobów lub nawet zmieniając ustawienia konfiguracji dla niektórych zasobów. Zamiast za pomocą portalu Azure, aby usunąć zasób z grupy zasobów, można użyć polecenia programu PowerShell [AzureResource Usuń](https://msdn.microsoft.com/library/azure/dn757676.aspx) lub lub polecenia "Usuń azure zasobu" z interfejsu wiersza polecenia w celu wykonania tego samego zadania.
6. Uzyskanie wszystkich zasobów i konfiguracji, która powinna być w grupach zasobów powrót do stanu, w którym należy je w zainstalowanie środowiska do grup zasobów za pomocą takie same polecenia, którego użyto w sekcji [Rozmieszczanie zasoby w środowiskach](#deploy-resources-to-environments) , ale zamiast "Deployment1" z "Deployment2."
7.  Jak pokazano w sekcji Podsumowanie karta TestApp1 rozwoju na obrazie pokazano w kroku 4, zobaczysz, że aplikacji sieci Web, które usunięte w portalu w poprzednim kroku istnieje ponownie, tak jak inne zasoby wybrany do usunięcia. Zmiana konfiguracji zasobów można również sprawdzić, czy ich ustawiono ponownie wartości w plikach parametru zbyt. Jedną z zalet wdrażanie usługi środowiskach dzięki szablonom Menedżera zasobów Azure to, że można łatwo ponownie rozmieścić środowiskach powrót do znanego stanu w dowolnym momencie.
8. Jeśli klikniesz na tekst w obszarze "Ostatniego wdrożenia" na poniższym obrazie, pojawi się kartę, która pokazuje historię wdrożenia dla grupy zasobów.  Ponieważ używasz nazwy "Deployment1" dla pierwszego wdrożenia i "Deployment2" do drugiego wdrożenia będą dostępne dwa wpisy.  Klikając wdrożeniu zostanie wyświetlona karta, która pokazuje wyniki dla każdego rozmieszczenia.
  ![Portal](./media/solution-dev-test-environments/portal3.png)



## <a name="delete-environments"></a>Usuwanie środowiska
Po zakończeniu korzystania ze środowiska chcesz ją usunąć, aby nie naliczone opłaty za zużycie zasobów Azure, którego nie używasz.  Usuwanie środowiskach jest jeszcze łatwiejsze niż ich tworzeniu.  W poprzednich krokach poszczególnych grup zasobów Azure zostały utworzone dla każdego środowiska, a następnie wdrożono zasobów do grupy zasobów. 

Usuwanie środowiska za pomocą dowolnej z poniższych metod.  Wszystkie metody osiągnie taka sama.

### <a name="azure-cli"></a>Polecenie Azure

W wierszu interfejsu wiersza polecenia wpisz następujące polecenie:

    azure group delete "TestApp1-Development"

Po wyświetleniu monitu wprowadź y, a następnie naciśnij klawisz enter, aby usunąć środowiska projektowego i wszystkie jego zasobów. Po upływie kilku minut polecenie zwróci następujące czynności:

    info:    group delete command OK

W wierszu polecenie wpisz poniższe czynności, aby usunąć pozostałe środowiskach:

    azure group delete "TestApp1-Test"
    azure group delete "TestApp1-Pre-Production"
  
### <a name="powershell"></a>Programu PowerShell

W wierszu polecenia programu PowerShell Azure (wersja 1.01 lub nowszy) wpisz poniższe polecenie, aby usunąć grupa zasobów i wszystkie jego zawartość.    

    Remove-AzureRmResourceGroup -Name TestApp1-Development

Gdy zostanie wyświetlony monit, jeśli wiesz, że chcesz usunąć grupa zasobów, wprowadź y, a następnie klawisz enter.

Wpisz poniższe czynności, aby usunąć pozostałe środowiskach:

    Remove-AzureRmResourceGroup -Name TestApp1-Test
    Remove-AzureRmResourceGroup -Name TestApp1-Pre-Production

### <a name="azure-portal"></a>Azure portal

1. W portalu Azure tak jak w poprzednich krokach przejdź do grup zasobów. 
2. Wybierz grupę zasobów TestApp1 rozwoju i kliknij pozycję Usuń w karta Grupa zasobów TestApp1 rozwoju. Zostanie wyświetlona karta nowy. Wprowadź nazwę grupy zasobów i kliknij przycisk Usuń.
![Portal](./media/solution-dev-test-environments/rgdelete.png)
3. Usuń teście TestApp1 i zasobów TestApp1 sprzed produkcji grupy tak samo usunięte rozwoju TestApp1 grupa zasobów.

Niezależnie od metody, którego używasz nie będą dostępne grupy zasobów i wszystkie zasoby, które są przechowywane i będzie już ponoszenia rozliczeń kosztów zasobów.  

Aby zminimalizować Azure wydatków wykorzystania zasobów naliczone podczas opracowywania aplikacji [automatyzacji Azure](automation/automation-intro.md) umożliwia planowanie zadań:

- Zatrzymaj maszyn wirtualnych na końcu każdego dnia i uruchom je na początku każdego dnia.
- Usuwanie całego środowiska na końcu każdego dnia i ponowne tworzenie na początku każdego dnia.
 
Teraz, gdy już wystąpił, jak łatwo jest tworzyć, zachować i Usuń rozwoju i przetestować środowisk, możesz dowiedzieć się więcej o tym, jakie po prostu czy dodatkowo eksperymentowanie z powyższych czynności i czytania odniesieniami zawartymi w tym artykule.

## <a name="next-steps"></a>Następne kroki

- [Delegowanie kontroli administracyjnej](./active-directory/role-based-access-control-configure.md) do różnych zasobów w poszczególnych środowisk przypisując Microsoft Azure AD grupom lub użytkownikom określone role, mające możliwość wykonywania podzbiór operacje Azure zasobów.
- [Przypisywanie znaczników](resource-group-using-tags.md) grup zasobów dla każdego środowiska i/lub poszczególnych zasobów. Może dodać znacznik "Środowisko" do grupy zasobów i ustawić jej wartość odpowiadające im nazwy środowiska. Tagi można szczególnie pomocne, gdy chcesz uporządkować zasoby dotyczące rozliczeń i zarządzania.
- Monitorowanie alertów i rozliczenia dla zasobów grupy zasobów w [Azure portal](https://portal.azure.com).
