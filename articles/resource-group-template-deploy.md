<properties
   pageTitle="Wdrażanie zasobów przy użyciu programu PowerShell i szablon | Microsoft Azure"
   description="Wdrażanie zasobów Azure za pomocą Menedżera zasobów Azure i Azure programu PowerShell. Zasoby są definiowane w szablonie Menedżera zasobów."
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

# <a name="deploy-resources-with-resource-manager-templates-and-azure-powershell"></a>Wdrażanie zasobów z szablonami Menedżera zasobów i Azure programu PowerShell

> [AZURE.SELECTOR]
- [Programu PowerShell](resource-group-template-deploy.md)
- [Polecenie Azure](resource-group-template-deploy-cli.md)
- [Portal](resource-group-template-deploy-portal.md)
- [INTERFEJSU API USŁUGI REST](resource-group-template-deploy-rest.md)

W tym temacie wyjaśniono, jak za pomocą programu PowerShell Azure z szablonami Menedżera zasobów wdrażanie zasobów Azure.  

> [AZURE.TIP] Aby uzyskać pomoc dotyczącą debugowania komunikat o błędzie podczas wdrażania zobacz:
>
> - [Wyświetlanie operacji wdrażania przy użyciu programu PowerShell Azure](resource-manager-troubleshoot-deployments-powershell.md) , aby informacje o wprowadzenie informacji, które ułatwia rozwiązywanie problemów z błędu
> - [Rozwiązywanie typowych błędów podczas wdrażania zasobów Azure przy użyciu Menedżera zasobów Azure](resource-manager-common-deployment-errors.md) , aby dowiedzieć się, jak rozwiązać typowe błędy wdrażania

Szablon może być plik lokalny lub zewnętrznego pliku, który jest dostępny za pomocą identyfikatora URI. Gdy szablon znajduje się na koncie miejsca do magazynowania, można ograniczyć dostęp do szablonu i udostępniać token podpisu (SA) udostępnienia podczas wdrażania.

## <a name="quick-steps-to-deployment"></a>Szybkie kroki, aby wdrażania

W tym artykule opisano różne opcje dostępne podczas wdrażania. Jednak często wystarczy tylko dwa polecenia prosty. Aby szybko rozpocząć pracę z wdrażania, należy użyć następujących poleceń:

    New-AzureRmResourceGroup -Name ExampleResourceGroup -Location "West US"
    New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate> -TemplateParameterFile <PathToParameterFile>

Aby uzyskać więcej informacji na temat opcji wdrożenia, które mogą być lepiej dostosowane do rozwiązania, nadal przeczytaniem tego artykułu.

[AZURE.INCLUDE [resource-manager-deployments](../includes/resource-manager-deployments.md)]

## <a name="deploy-with-powershell"></a>Rozmieszczanie za pomocą programu PowerShell

1. Zaloguj się do konta Azure.

        Add-AzureRmAccount

     Podsumowanie konta, jest zwracany.

        Environment : AzureCloud
        Account     : someone@example.com
        ...

2. Jeśli masz wiele subskrypcji, podaj identyfikator subskrypcji mają być używane do wdrożenia przy użyciu polecenia **AzureRmContext zestawu** . 

        Set-AzureRmContext -SubscriptionID <YourSubscriptionId>

3. Zazwyczaj wdrażając nowy szablon, którego chcesz utworzyć grupa zasobów zawierają zasobów. Jeśli masz istniejącej grupy zasobów, którą chcesz wdrożyć, można pominąć ten krok i używać danej grupy zasobów. 

     Aby utworzyć grupę zasobów, podaj nazwę i lokalizację dla grupy zasobów. Należy podać lokalizację, w której grupa zasobów, ponieważ grupa zasobów przechowuje metadane o zasobach. Ze względów zgodności można określić miejsce, w którym są przechowywane metadane. Ogólnie zaleca się, aby określić lokalizację, w której miejsce, w którym znajdują się najczęściej zasobów. Za pomocą tej samej lokalizacji może uprościć szablonu.

        New-AzureRmResourceGroup -Name ExampleResourceGroup -Location "West US"
   
     Podsumowanie nowej grupy zasobów jest zwracana.
   
        ResourceGroupName : ExampleResourceGroup
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
             Actions  NotActions
             =======  ==========
             *
        ResourceId        : /subscriptions/######/resourceGroups/ExampleResourceGroup

4. Przed wykonaniem wdrożenia, sprawdź poprawność ustawień wdrożenia. Polecenia cmdlet **Test AzureRmResourceGroupDeployment** umożliwia znajdowanie problemów przed utworzeniem zasoby rzeczywiste. W poniższym przykładzie pokazano sposób sprawdzania poprawności wdrożeniu.

        Test-AzureRmResourceGroupDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate>

5. Aby wdrożyć zasobów do grupy zasobów, uruchom polecenie **Nowy AzureRmResourceGroupDeployment** i podaj wymaganych parametrów. Parametry zawierają nazwę rozmieszczenia, nazwę swojej grupy zasobów, ścieżka lub adres URL, aby utworzony szablon i inne parametry potrzebną do rozwiązania. Jeśli parametr **Tryb** nie zostanie określony, jest używana wartość domyślna **przyrostowe** . Aby uruchomić pełną wdrożenia, ustaw **Tryb** **wykonane**. Uważaj, aby podczas korzystania z trybu wykonane, zgodnie z przypadkowego usunięcia zasoby, które nie są w szablonie.

     Aby wdrożyć lokalne szablon, można użyć parametru **TemplateFile** :

        New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate>

     Aby wdrożyć zewnętrznych szablon, można użyć parametru **TemplateUri** :

        New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateUri <LinkToTemplate>
   
     Dostępne są następujące opcje umożliwiające wartości parametrów: 
   
     1. Używanie parametrów w tekście.

            New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate> -myParameterName "parameterValue"

     2. Za pomocą obiektu parametru.

            $parameters = @{"<ParameterName>"="<Parameter Value>"}
            New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate> -TemplateParameterObject $parameters

     3. Za pomocą pliku lokalnego parametrów. Aby uzyskać informacje o pliku szablonu zobacz [plik parametru](#parameter-file).

            New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate> -TemplateParameterFile <PathToParameterFile>

     4. Za pomocą pliku zewnętrznych parametrów. Aby uzyskać informacje o pliku szablonu zobacz [plik parametru](#parameter-file). 

            New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateUri <LinkToTemplate> -TemplateParameterUri <LinkToParameterFile>

        Korzystając z pliku parametrów zewnętrznych, nie można przekazać innych wartości albo w tekście lub plik lokalny. Aby uzyskać więcej informacji zobacz [pierwszeństwo parametru](#parameter-precendence).

     Po wdrożeniu zasobów zostanie wyświetlona podsumowanie wdrożenia.

        DeploymentName    : ExampleDeployment
        ResourceGroupName : ExampleResourceGroup
        ProvisioningState : Succeeded
        Timestamp         : 4/14/2015 7:00:27 PM
        Mode              : Incremental
        ...

     Jeśli szablon zawiera parametr o takiej samej nazwie jak jeden z parametrów w polecenia programu PowerShell, zostanie wyświetlony monit o podanie wartości parametru. Parametr od szablonu będzie zawierać przyrostek **FromTemplate**. Na przykład parametr o nazwie **ResourceGroupName** w konflikcie z szablonu z parametrem **ResourceGroupName** w polecenia cmdlet [New-AzureRmResourceGroupDeployment](https://msdn.microsoft.com/library/azure/mt679003.aspx) . Zostanie wyświetlony monit o podanie wartości dla **ResourceGroupNameFromTemplate**. Ogólnie należy unikać ten błąd, nie nadając parametrów taką samą nazwę jak parametry używane do operacji rozmieszczania.

6. Jeśli chcesz uzyskać dodatkowe informacje dotyczące rozmieszczania, które mogą ułatwić rozwiązywanie problemów z błędami dowolnego wdrażania, należy użyć parametru **DeploymentDebugLogLevel** logowania. Można określić, że żądania zawartości i/lub zawartość odpowiedzi rejestrowane w działaniu wdrożenia.

        New-AzureRmResourceGroupDeployment -Name ExampleDeployment -DeploymentDebugLogLevel All -ResourceGroupName ExampleResourceGroup -TemplateFile <PathOrLinkToTemplate>
        
     Aby uzyskać więcej informacji o korzystaniu z tej zawartości debugowania rozwiązywać wdrożeń zobacz [wdrożenia grupa zasobów Rozwiązywanie problemów z programem PowerShell Azure](resource-manager-troubleshoot-deployments-powershell.md).

## <a name="deploy-template-from-storage-with-sas-token"></a>Wdrażanie szablonu z magazynu z token skojarzenia zabezpieczeń

Możesz dodać własne szablony do konta miejsca do magazynowania i łącza do ich podczas wdrażania przy użyciu tokenu skojarzeń zabezpieczeń.

> [AZURE.IMPORTANT] Wykonując poniższe czynności, blob zawierającym szablon jest dostępny do właściciela konta. Jednak podczas tworzenia skojarzenia zabezpieczeń tokenu dla obiektów blob to jest dostępne dla wszystkich osób z tego identyfikatora URI. Jeśli inny użytkownik przechwytuje identyfikator URI, użytkownik będzie mógł dostępu do szablonu. Przy użyciu tokenu skojarzeń zabezpieczeń jest dobrym sposobem ograniczanie dostępu do szablonów, ale nie powinna zawierać dane poufne, takie jak hasła bezpośrednio w szablonie.

### <a name="add-private-template-to-storage-account"></a>Dodawanie prywatne szablonu do konta miejsca do magazynowania

Konfigurowanie konta miejsca do magazynowania dla szablonów następujące czynności:

1. Tworzenie grupy zasobów.

        New-AzureRmResourceGroup -Name ManageGroup -Location "West US"

2. Utwórz konto miejsca do magazynowania. Nazwę konta magazynu musi być unikatowa we Azure, dlatego podaj nazwę dla tego konta.

        New-AzureRmStorageAccount -ResourceGroupName ManageGroup -Name storagecontosotemplates -Type Standard_LRS -Location "West US"

3. Ustawianie konta miejsca do magazynowania.

        Set-AzureRmCurrentStorageAccount -ResourceGroupName ManageGroup -Name storagecontosotemplates

4. Tworzenie kontenera. Uprawnienia ma wartość **wyłączone** co oznacza, że kontener jest dostępna wyłącznie dla właściciela.

        New-AzureStorageContainer -Name templates -Permission Off
        
5. Dodawanie szablonu do kontenera.

        Set-AzureStorageBlobContent -Container templates -File c:\Azure\Templates\azuredeploy.json
        
### <a name="provide-sas-token-during-deployment"></a>Podaj token skojarzeń zabezpieczeń podczas wdrażania

Aby wdrożyć szablon prywatne na koncie miejsca do magazynowania, pobrać tokenu skojarzeń zabezpieczeń i dołączyć go w polu Identyfikator URI dla szablonu.

1. Zmiana konta miejsca do magazynowania Ustaw dysk zawierający Szablony rachunku bieżącego miejsca do magazynowania.

        Set-AzureRmCurrentStorageAccount -ResourceGroupName ManageGroup -Name storagecontosotemplates

2. Tworzenie skojarzeń zabezpieczeń token z uprawnienia do odczytu i czas wygaśnięcia, aby ograniczyć dostęp. Pobierz pełny identyfikator URI szablonie, w tym również tokenu skojarzeń zabezpieczeń.

        $templateuri = New-AzureStorageBlobSASToken -Container templates -Blob azuredeploy.json -Permission r -ExpiryTime (Get-Date).AddHours(2.0) -FullUri

3. Wdrażanie szablonu, dostarczając identyfikator URI, który zawiera token skojarzeń zabezpieczeń.

        New-AzureRmResourceGroupDeployment -ResourceGroupName ExampleResourceGroup -TemplateUri $templateuri

Przykład tokenu skojarzeń zabezpieczeń przy użyciu szablonów połączone zobacz [Korzystanie z szablonów połączone z Menedżerem zasobów Azure](resource-group-linked-templates.md).

[AZURE.INCLUDE [resource-manager-parameter-file](../includes/resource-manager-parameter-file.md)]

## <a name="parameter-precedence"></a>Pierwszeństwo parametru

Za pomocą parametrów w tekście i pliku lokalnego parametrów w tej samej operacji wdrożenia. Na przykład można określić kilka wartości w pliku lokalnego parametrów i dodawanie innych wartości w tekście podczas wdrażania. Jeśli podasz wartości parametru w pliku parametrów lokalnych i w tekście, pierwszeństwo ma wartość w tekście.

Jednak nie można użyć parametrów w tekście z plikiem zewnętrznym parametru. Po określeniu pliku parametrów w parametrze **TemplateParameterUri** wszystkie parametry w tekście są ignorowane. Należy podać wszystkie wartości parametru w pliku zewnętrznym. Jeśli szablon zawiera wartość poufnych, która nie możesz umieścić w pliku parametrów, Dodaj tę wartość do klucza magazynu i odwołanie klucza magazynu w pliku zewnętrznych parametru albo dynamicznie zapewniają wszystkie w tekście wartości parametru.

Aby uzyskać szczegółowe informacje o korzystaniu z odwołaniem KeyVault do przekazania bezpiecznego wartości zobacz [przekazania bezpiecznego wartości podczas wdrażania](resource-manager-keyvault-parameter.md).

## <a name="next-steps"></a>Następne kroki
- Na przykład wdrażania zasobów za pośrednictwem biblioteki .NET klienta zobacz [zasoby rozmieszczanie przy użyciu bibliotek .NET i szablonu](virtual-machines/virtual-machines-windows-csharp-template.md).
- Aby określić parametry w szablonie, zobacz [Narzędzia do tworzenia szablonów](resource-group-authoring-templates.md#parameters).
- Aby uzyskać wskazówki na temat wdrażania rozwiązania w różnych środowiskach zobacz [rozwoju i środowiskach testowych platformy Microsoft Azure](solution-dev-test-environments.md).

