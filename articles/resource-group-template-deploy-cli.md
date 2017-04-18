<properties
   pageTitle="Wdrażanie zasobów za pomocą interfejsu wiersza polecenia Azure i szablonu | Microsoft Azure"
   description="Wdrażanie zasobów Azure za pomocą Menedżera zasobów Azure i polecenie Azure. Zasoby są definiowane w szablonie Menedżera zasobów."
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
   ms.date="08/15/2016"
   ms.author="tomfitz"/>

# <a name="deploy-resources-with-resource-manager-templates-and-azure-cli"></a>Wdrażanie zasobów z szablonami Menedżera zasobów i polecenie Azure

> [AZURE.SELECTOR]
- [Programu PowerShell](resource-group-template-deploy.md)
- [Polecenie Azure](resource-group-template-deploy-cli.md)
- [Portal](resource-group-template-deploy-portal.md)
- [INTERFEJSU API USŁUGI REST](resource-group-template-deploy-rest.md)

W tym temacie wyjaśniono, jak za pomocą interfejsu wiersza polecenia Azure z szablonami Menedżera zasobów wdrażanie zasobów Azure.  

> [AZURE.TIP] Aby uzyskać pomoc dotyczącą debugowania komunikat o błędzie podczas wdrażania zobacz:
>
> - [Wyświetlanie operacji wdrażania z polecenie Azure](resource-manager-troubleshoot-deployments-cli.md) , aby informacje o wprowadzenie informacji, które ułatwia rozwiązywanie problemów z błędu
> - [Rozwiązywanie typowych błędów podczas wdrażania zasobów Azure przy użyciu Menedżera zasobów Azure](resource-manager-common-deployment-errors.md) , aby dowiedzieć się, jak rozwiązać typowe błędy wdrażania

Szablon może być plik lokalny lub zewnętrznego pliku, który jest dostępny za pomocą identyfikatora URI. Gdy szablon znajduje się na koncie miejsca do magazynowania, można ograniczyć dostęp do szablonu i udostępniać token podpisu (SA) udostępnienia podczas wdrażania.

## <a name="quick-steps-to-deployment"></a>Szybkie kroki, aby wdrażania

W tym artykule opisano różne opcje dostępne podczas wdrażania. Jednak często wystarczy tylko dwa polecenia prosty. Aby szybko rozpocząć pracę z wdrażania, należy użyć następujących poleceń:

    azure group create -n ExampleResourceGroup -l "West US"
    azure group deployment create -f <PathToTemplate> -e <PathToParameterFile> -g ExampleResourceGroup -n ExampleDeployment

Aby uzyskać więcej informacji na temat opcji wdrożenia, które mogą być lepiej dostosowane do rozwiązania, nadal przeczytaniem tego artykułu.

[AZURE.INCLUDE [resource-manager-deployments](../includes/resource-manager-deployments.md)]

## <a name="deploy-with-azure-cli"></a>Rozmieszczanie za pomocą interfejsu wiersza polecenia Azure

Jeśli nie były już używane polecenie Azure przy użyciu Menedżera zasobów, zobacz [za pomocą interfejsu wiersza polecenia Azure dla komputerów Mac, Linux i systemu Windows z zarządzania zasobami Azure](xplat-cli-azure-resource-manager.md).

1. Zaloguj się do konta Azure. Po zapewnieniu poświadczenia, polecenie zwraca wynik nazwę użytkownika.

        azure login
  
        ...
        info:    login command OK

2. Jeśli masz wiele subskrypcji, podaj identyfikator subskrypcji, którego chcesz użyć do wdrożenia.

        azure account set <YourSubscriptionNameOrId>

3. Przełączanie do modułu Azure Menedżera zasobów. Otrzymasz potwierdzenie nowy tryb.

        azure config mode arm
   
        info:     New mode is arm

4. Jeśli nie masz istniejącej grupy zasobów, należy utworzyć grupy zasobów. Wprowadź nazwę grupy zasobów i lokalizacji, w której potrzebne do tego rozwiązania. Należy podać lokalizację, w której grupa zasobów, ponieważ grupa zasobów przechowuje metadane o zasobach. Ze względów zgodności można określić miejsce, w którym są przechowywane metadane. Ogólnie zaleca się, aby określić lokalizację, w której miejsce, w którym znajdują się najczęściej zasobów. Za pomocą tej samej lokalizacji może uprościć szablonu.

        azure group create -n ExampleResourceGroup -l "West US"

     Podsumowanie nowej grupy zasobów jest zwracana.
   
        info:    Executing command group create
        + Getting resource group ExampleResourceGroup
        + Creating resource group ExampleResourceGroup
        info:    Created resource group ExampleResourceGroup
        data:    Id:                  /subscriptions/####/resourceGroups/ExampleResourceGroup
        data:    Name:                ExampleResourceGroup
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags:
        data:
        info:    group create command OK

5. Sprawdź poprawność wdrożenia przed wykonaniem ją, uruchamiając polecenie **Sprawdź poprawność szablon azure grupy** . Podczas testowania wdrożenia, należy podać parametry dokładnie tak jak podczas wykonywania wdrożenia (jak pokazano w następnym kroku).

        azure group template validate -f <PathToTemplate> -p "{\"ParameterName\":{\"value\":\"ParameterValue\"}}" -g ExampleResourceGroup

5. Aby wdrożyć zasobów do grupy zasobów, uruchom następujące polecenie i podaj wymaganych parametrów. Parametry zawierają nazwę rozmieszczenia, nazwę swojej grupy zasobów, ścieżka lub adres URL szablonu i inne parametry potrzebną do rozwiązania. 
   
     Masz następujące trzy opcje umożliwiające wartości parametrów: 

     1. Używanie parametrów w tekście i lokalnych szablonu. Każdego parametru jest w formacie: `"ParameterName": { "value": "ParameterValue" }`. W poniższym przykładzie pokazano parametrów przy użyciu znaków kontrolnych.

            azure group deployment create -f <PathToTemplate> -p "{\"ParameterName\":{\"value\":\"ParameterValue\"}}" -g ExampleResourceGroup -n ExampleDeployment

     2. Używanie parametrów w tekście i łącza do szablonu.

            azure group deployment create --template-uri <LinkToTemplate> -p "{\"ParameterName\":{\"value\":\"ParameterValue\"}}" -g ExampleResourceGroup -n ExampleDeployment

     3. Za pomocą pliku parametrów. Aby uzyskać informacje o pliku szablonu zobacz [plik parametru](#parameter-file).
    
            azure group deployment create -f <PathToTemplate> -e <PathToParameterFile> -g ExampleResourceGroup -n ExampleDeployment

     Po zasobów zostały wdrożone za pomocą jednej z trzech metod powyżej, zostanie wyświetlony podsumowanie wdrożenia.
  
        info:    Executing command group deployment create
        + Initializing template configurations and parameters
        + Creating a deployment
        ...
        info:    group deployment create command OK

     Aby uruchomić pełną wdrożenia, ustaw **Tryb** **wykonane**. Należy zachować ostrożność, korzystając z tego trybu jako przypadkowego usunięcia zasoby, które nie są w szablonie.

        azure group deployment create --mode Complete -f <PathToTemplate> -e <PathToParameterFile> -g ExampleResourceGroup -n ExampleDeployment

6. Jeśli chcesz uzyskać dodatkowe informacje dotyczące rozmieszczania, które mogą ułatwić rozwiązywanie problemów z błędami dowolnego wdrażania, należy użyć parametru **Ustawienie debugowania** logowania. Można określić, że żądania zawartości i/lub zawartość odpowiedzi rejestrowane w działaniu wdrożenia.

        azure group deployment create --debug-setting All -f <PathToTemplate> -e <PathToParameterFile> -g ExampleResourceGroup -n ExampleDeployment

## <a name="deploy-template-from-storage-with-sas-token"></a>Wdrażanie szablonu z magazynu z token skojarzenia zabezpieczeń

Możesz dodać własne szablony do konta miejsca do magazynowania i łącza do ich podczas wdrażania przy użyciu tokenu skojarzeń zabezpieczeń.

> [AZURE.IMPORTANT] Wykonując poniższe czynności, blob zawierającym szablon jest dostępny do właściciela konta. Jednak podczas tworzenia skojarzenia zabezpieczeń tokenu dla obiektów blob to jest dostępne dla wszystkich osób z tego identyfikatora URI. Jeśli inny użytkownik przechwytuje identyfikator URI, użytkownik będzie mógł dostępu do szablonu. Przy użyciu tokenu skojarzeń zabezpieczeń jest dobrym sposobem ograniczanie dostępu do szablonów, ale nie powinna zawierać dane poufne, takie jak hasła bezpośrednio w szablonie.

### <a name="add-private-template-to-storage-account"></a>Dodawanie prywatne szablonu do konta miejsca do magazynowania

Konfigurowanie konta miejsca do magazynowania dla szablonów następujące czynności:

1. Tworzenie grupy zasobów.

        azure group create -n "ManageGroup" -l "westus"

2. Utwórz konto miejsca do magazynowania. Nazwę konta magazynu musi być unikatowa we Azure, dlatego podaj nazwę dla tego konta.

        azure storage account create -g ManageGroup -l "westus" --sku-name LRS --kind Storage storagecontosotemplates

3. Ustawianie zmiennych dla konta miejsca do magazynowania i klucza.

        export AZURE_STORAGE_ACCOUNT=storagecontosotemplates
        export AZURE_STORAGE_ACCESS_KEY={storage_account_key}

4. Tworzenie kontenera. Uprawnienia ma wartość **wyłączone** co oznacza, że kontener jest dostępna wyłącznie dla właściciela.

        azure storage container create --container templates -p Off 
        
4. Dodawanie szablonu do kontenera.

        azure storage blob upload --container templates -f c:\Azure\Templates\azuredeploy.json
        
### <a name="provide-sas-token-during-deployment"></a>Podaj token skojarzeń zabezpieczeń podczas wdrażania

Aby wdrożyć szablon prywatne na koncie miejsca do magazynowania, pobrać tokenu skojarzeń zabezpieczeń i dołączyć go w polu Identyfikator URI dla szablonu.

1. Tworzenie skojarzeń zabezpieczeń token z uprawnienia do odczytu i czas wygaśnięcia, aby ograniczyć dostęp. Ustaw wartość czasu wygaśnięcia, aby umożliwić wystarczającą ilość czasu ukończyć wdrażanie. Pobierz pełny identyfikator URI szablonie, w tym również tokenu skojarzeń zabezpieczeń.

        expiretime=$(date -I'minutes' --date "+30 minutes")
        fullurl=$(azure storage blob sas create --container templates --blob azuredeploy.json --permissions r --expiry $expiretimetime --json  | jq ".url")

2. Wdrażanie szablonu, dostarczając identyfikator URI, który zawiera token skojarzeń zabezpieczeń.

        azure group deployment create --template-uri $fullurl -g ExampleResourceGroup

Przykład tokenu skojarzeń zabezpieczeń przy użyciu szablonów połączone zobacz [Korzystanie z szablonów połączone z Menedżerem zasobów Azure](resource-group-linked-templates.md).

[AZURE.INCLUDE [resource-manager-parameter-file](../includes/resource-manager-parameter-file.md)]

## <a name="next-steps"></a>Następne kroki
- Na przykład wdrażania zasobów za pośrednictwem biblioteki .NET klienta zobacz [zasoby rozmieszczanie przy użyciu bibliotek .NET i szablonu](virtual-machines/virtual-machines-windows-csharp-template.md).
- Aby określić parametry w szablonie, zobacz [Narzędzia do tworzenia szablonów](resource-group-authoring-templates.md#parameters).
- Aby uzyskać wskazówki na temat wdrażania rozwiązania w różnych środowiskach zobacz [rozwoju i środowiskach testowych platformy Microsoft Azure](solution-dev-test-environments.md).
- Aby uzyskać szczegółowe informacje o korzystaniu z odwołaniem KeyVault do przekazania bezpiecznego wartości zobacz [przekazania bezpiecznego wartości podczas wdrażania](resource-manager-keyvault-parameter.md).

