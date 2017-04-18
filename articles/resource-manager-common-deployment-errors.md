<properties
   pageTitle="Rozwiązywanie typowych błędów Azure wdrożenia | Microsoft Azure"
   description="W tym artykule opisano, jak rozwiązać typowe błędy po wdrożeniu zasobów Azure za pomocą Menedżera zasobów Azure."
   services="azure-resource-manager"
   documentationCenter=""
   tags="top-support-issue"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"
   keywords="Błąd wdrażania azure wdrożenie wdrażanie Azure"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="tomfitz"/>

# <a name="troubleshoot-common-azure-deployment-errors-with-azure-resource-manager"></a>Rozwiązywanie problemów z typowych błędów Azure wdrażania przy użyciu Menedżera zasobów Azure

W tym temacie opisano, jak można usunąć niektóre typowe błędy wdrożenia Azure mogą wystąpić. Jeśli potrzebujesz więcej informacji o czym polega problem z wdrożeniem wyświetlenie [operacji rozmieszczania widoku](resource-manager-troubleshoot-deployments-portal.md) , a następnie powrotu do tego artykułu, aby uzyskać pomoc dotyczącą usuwania błędów. Sekcje w tym temacie listy, który zostanie wyświetlony kod błędu.

## <a name="invalid-template"></a>Nieprawidłowy szablon

Ten błąd może spowodować z kilku różnych typów błędów. 

### <a name="syntax-error"></a>Błąd składni

Jeśli zostanie wyświetlony komunikat o błędzie wskazujący sprawdzania poprawności nie powiodło się szablonu, może być problem składni w szablonie. 

    Code=InvalidTemplate 
    Message=Deployment template validation failed

Ten błąd jest łatwo popełnić ponieważ wyrażeń szablonu może być skomplikowanych. Na przykład następującego przypisania nazwę konta magazynu zawiera jeden zestaw nawiasów, trzy funkcje trzy zestawy nawiasów, jeden zbiór apostrofy i jednej właściwości:

    "name": "[concat('storage', uniqueString(resourceGroup().id))]",

Jeśli składnia dopasowania nie zostanie określona, szablon daje w wyniku wartość, która jest inny niż masz zamiar.

Gdy zostanie wyświetlony ten typ błędu, dokładnie przejrzyj składni wyrażeń. Warto rozważyć użycie edytora JSON, na przykład [Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) lub [Visual Studio kod](resource-manager-vs-code.md), który może zostać wyświetlone ostrzeżenie o występują błędy składniowe. 

### <a name="incorrect-segment-lengths"></a>Niepoprawne części długości

Inny błąd nieprawidłowego szablonu występuje, gdy nazwa zasobu nie jest poprawny.

    Code=InvalidTemplate
    Message=Deployment template validation failed: 'The template resource {resource-name}' 
    for type {resource-type} has incorrect segment lengths.

Zasób poziomu katalogu głównego musi mieć jeden mniej segment w nazwie niż w polu Typ zasobu. Każdy segment jest zróżnicowane przez kreski ułamkowej. W poniższym przykładzie Typ ma dwa segmenty, a nazwa ma jednego segmentu, aby była **prawidłową nazwę**.

    {
      "type": "Microsoft.Web/serverfarms",
      "name": "myHostingPlanName",
      ...
    }

Ale następnym przykładzie jest **Nieprawidłowa nazwa** , ponieważ ma taką samą liczbę segmentów jako typ.

    {
      "type": "Microsoft.Web/serverfarms",
      "name": "appPlan/myHostingPlanName",
      ...
    }

Dla zasobów podrzędny typ i nazwa mieć taką samą liczbę segmentów. Liczba segmentów ma sens, ponieważ pełną nazwę i typ podrzędne zawiera nazwę nadrzędnego i typ. W związku z tym imię i nazwisko nadal zawiera jednej części mniej niż pełny typ. 

    "resources": [
        {
            "type": "Microsoft.KeyVault/vaults",
            "name": "contosokeyvault",
            ...
            "resources": [
                {
                    "type": "secrets",
                    "name": "appPassword",
                    ...
                }
            ]
        }
    ]

Wprowadzenie do segmentów prawym może być kłopotliwe z typami Menedżera zasobów, które są stosowane przez dostawców zasobów. Na przykład zastosowanie Blokada zasobu do witryny sieci web wymaga typu z czterech segmentów. W związku z tym nazwa jest trzy segmenty:

    {
        "type": "Microsoft.Web/sites/providers/locks",
        "name": "[concat(variables('siteName'),'/Microsoft.Authorization/MySiteLock')]",
        ...
    }

### <a name="copy-index-is-not-expected"></a>Nie przewiduje się kopii indeksu

Ten błąd **InvalidTemplate** wystąpienia po zastosowaniu element **kopii** w części szablonu, która nie obsługuje tego elementu. Kopiuj element można stosować tylko typ zasobu. Kopiowanie nie można zastosować do właściwości w ramach typu zasobu. Na przykład możesz zastosować kopii maszyn wirtualnych, ale nie dotyczą dysków systemu operacyjnego dla maszyny wirtualnej. W niektórych przypadkach można przekonwertować zasobu podrzędne do zasobu nadrzędnej do utworzenia pętli Kopiuj. Aby uzyskać więcej informacji o korzystaniu z kopii zobacz [Tworzenie wielu wystąpień zasobów w Menedżerze zasobów Azure](resource-group-create-multiple.md).

### <a name="parameter-is-not-valid"></a>Parametr jest nieprawidłowy

Jeśli szablon Określa dozwolone wartości parametru i podać wartość, która nie jest jedną z tych wartości, zostanie wyświetlony komunikat podobny do następującego błędu:

    Code=InvalidTemplate; 
    Message=Deployment template validation failed: 'The provided value {parameter vaule} 
    for the template parameter {parameter name} is not valid. The parameter value is not 
    part of the allowed value(s)

Podwójny Dozwolona wartość w szablonie i poda podczas wdrażania.

## <a name="not-found-or-resource-not-found"></a>Nie można odnaleźć (lub nie można znaleźć zasobu)

Jeśli szablon zawiera nazwę zasobu, którego nie można rozwiązać, jest wyświetlany komunikat o błędzie podobny do:

    Code=NotFound;
    Message=Cannot find ServerFarm with name exampleplan.

Jeśli próbujesz wdrażanie brakuje zasobów w szablonie, sprawdź, czy trzeba dodać zależności. Menedżer zasobów optymalizuje wdrożenia, tworząc zasobów równolegle, jeśli to możliwe. Jeden zasób należy wdrożyć po inny zasób, należy użyć elementu **dependsOn** do szablonu do utworzenia współzależności innego zasobu. Na przykład podczas wdrażania aplikacji sieci web, musi istnieć plan usług aplikacji. Jeśli nie określono że aplikacji sieci web w zależności od planu usług aplikacji, Menedżer zasobów tworzy oba zasoby w tym samym czasie. Otrzymujesz komunikat o błędzie informujący, że nie można odnaleźć zasobu plan aplikacji usługi, ponieważ nie istnieje jeszcze podczas próby ustawienia właściwości w aplikacji sieci web. Ten błąd uniemożliwia przez ustawienie zależności w aplikacji sieci web.

    {
      "apiVersion": "2015-08-01",
      "type": "Microsoft.Web/sites",
      ...
      "dependsOn": [
        "[variables('hostingPlanName')]"
      ],
      ...
    }
 
Możesz także wyświetlać ten błąd zasobu istnieje w różnych grupach zasobów niż ten, który jest używany. W tym przypadku należy użyć [funkcji resourceId](resource-group-template-functions.md#resourceid) uzyskanie w pełni kwalifikowana nazwa zasobu.

    "properties": {
        "name": "[parameters('siteName')]",
        "serverFarmId": "[resourceId('plangroup', 'Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
    } 

Jeśli próbujesz użyć funkcji [Odwołanie](resource-group-template-functions.md#referenc) lub [listKeys](resource-group-template-functions.md#listkeys) z zasobem, którego nie można rozwiązać, pojawi się następujący komunikat o błędzie:

    Code=ResourceNotFound;
    Message=The Resource 'Microsoft.Storage/storageAccounts/{storage name}' under resource 
    group {resource group name} was not found.

Poszukaj wyrażenia, która obejmuje funkcję **odwołania** . Sprawdź, czy wartości parametru są poprawne.

## <a name="storage-account-already-exists-or-already-taken"></a>Istnieje już konto miejsca do magazynowania (lub już)

W przypadku kont miejsca do magazynowania musisz podać nazwę zasobu, który jest unikatowy w całej Azure. Jeśli nie podasz unikatową nazwę, zostanie wyświetlony komunikat o błędzie, takich jak:

    Code=StorageAccountAlreadyTaken 
    Message=The storage account named mystorage is already taken.

Możesz utworzyć unikatową nazwę, łącząc Konwencja nazewnictwa z wynikiem funkcji [uniqueString](resource-group-template-functions.md#uniquestring) .

    "name": "[concat('contosostorage', uniqueString(resourceGroup().id))]",
    "type": "Microsoft.Storage/storageAccounts",

Jeśli wdrażanie konto miejsca do magazynowania z taką samą nazwę jak istniejącego konta miejsca do magazynowania w ramach subskrypcji, ale podasz innej lokalizacji, otrzymujesz komunikat o błędzie wskazujący, że istnieje już konto miejsca do magazynowania w innej lokalizacji. Usuń istniejące konto miejsca do magazynowania lub podaj tej samej lokalizacji co istniejącego konta miejsca do magazynowania.

## <a name="account-name-invalid"></a>Nieprawidłowa nazwa konta

Zostanie wyświetlony błąd **AccountNameInvalid** podczas próby nazwę konta magazynu zawierającego niedozwolone znaki. Nazwy kont miejsca do magazynowania musi należeć do przedziału od 3 do 24 znaków i używanie tylko małe litery i cyfry.

## <a name="no-registered-provider-found"></a>Nie znaleziono zarejestrowanych dostawcy

Wdrażając zasobów, może zostać wyświetlony następujący kod błędu i komunikat:

    Code: NoRegisteredProviderFound
    Message: No registered resource provider found for location {ocation} 
    and API version {api-version} for type {resource-type}.

Dla jednej z trzech powodów, dla których jest wyświetlany następujący błąd:

1. Lokalizacja nie jest obsługiwane dla tego typu zasobu
2. Wersja API nie jest obsługiwane dla tego typu zasobu
3. Nie zarejestrowano dostawcy zasobów dla subskrypcji

Komunikat o błędzie powinien podać Ci sugestie dotyczące obsługiwanych lokalizacji i wersje interfejsu API. Możesz zmienić swój szablon do jednej z sugerowanych wartości. Większość dostawców są rejestrowane automatycznie przez Azure portal lub interfejsu wiersza polecenia, którego używasz, ale nie wszystkie. Jeśli korzystasz z dostawcą danego zasobu przed, może być konieczne zarejestrować tego dostawcy. Więcej informacji na temat dostawców zasobów za pomocą programu PowerShell lub interfejsu wiersza polecenia Azure można odkryć.

### <a name="powershell"></a>Programu PowerShell

Aby wyświetlić swój status rejestracji, za pomocą **Get-AzureRmResourceProvider**.

    Get-AzureRmResourceProvider -ListAvailable

Aby zarejestrować dostawcę, za pomocą **Rejestru AzureRmResourceProvider** i podaj nazwę dostawcy zasobów, które chcesz zarejestrować.

    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Cdn

Aby uzyskać obsługiwanych lokalizacji dla określonego typu zasobów, należy użyć:

    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).Locations

Aby uzyskać obsługiwanych wersji interfejsu API dla określonego typu zasobów, należy użyć:

    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).ApiVersions

### <a name="azure-cli"></a>Polecenie Azure

Aby sprawdzić, czy dostawca jest zarejestrowany, użyj `azure provider list` polecenia.

    azure provider list

Aby zarejestrować dostawcy zasobów, za pomocą `azure provider register` polecenia, a następnie określ *przestrzeń nazw* , aby zarejestrować.

    azure provider register Microsoft.Cdn

Aby wyświetlić lokalizacje obsługiwanych i wersje interfejsu API dla dostawcy zasobów, należy użyć:

    azure provider show -n Microsoft.Compute --json > compute.json

## <a name="operation-not-allowed"></a>Operacja nie jest dozwolona

Może być problemy podczas wdrażania przekracza normę, może to być na Grupa zasobów, subskrypcje, kont i innych zakresów. Na przykład subskrypcji może być skonfigurowany do ograniczania liczby rdzeni w danym regionie. Jeśli spróbujesz wdrażanie maszyny wirtualnej z rdzeni niż dozwolonych kwota, otrzymujesz komunikat o błędzie informujący, że została przekroczona.
Informacje o przydziałach pełną uzyskać [Azure subskrypcji i limity dotyczące usługi, przydziały oraz ograniczenia](azure-subscription-service-limits.md).

Aby zbadać Twoja subskrypcja przydziałów dla rdzenie, możesz użyć `azure vm list-usage` w polecenie Azure. Poniższy przykład pokazuje, czy przydział core bezpłatne konto wersji próbnej 4:

    azure vm list-usage

Która zwraca:

    info:    Executing command vm list-usage
    Location: westus
    data:    Name   Unit   CurrentValue  Limit
    data:    -----  -----  ------------  -----
    data:    Cores  Count  0             4
    info:    vm list-usage command OK

Po wdrożeniu szablonu, który tworzy więcej niż cztery rdzenie w regionie Zachód Stanów Zjednoczonych, zostanie wyświetlony błąd wdrożenia, który wygląda tak:

    Code=OperationNotAllowed
    Message=Operation results in exceeding quota limits of Core. 
    Maximum allowed: 4, Current in use: 4, Additional requested: 2.

Lub w programie PowerShell, można użyć polecenia cmdlet **Get-AzureRmVMUsage** .

    Get-AzureRmVMUsage

Która zwraca:

    ...
    CurrentValue : 0
    Limit        : 4
    Name         : {
                     "value": "cores",
                     "localizedValue": "Total Regional Cores"
                   }
    Unit         : null
    ...

W tych przypadkach należy przejdź do portalu i plików problem pomocy technicznej, aby podnieść przydziału dla danego regionu, w którym chcesz wdrożyć.

> [AZURE.NOTE] Należy pamiętać, że dla grup zasobów, przydziału dla każdego regionu poszczególnych, nie dla całej subskrypcji. Należy zainstalować 30 rdzenie zachód w USA, musisz uzyskać 30 rdzenie Menedżera zasobów w USA Zachód. Jeśli chcesz wdrożyć 30 rdzenie w każdym z regionów, do których masz dostęp, należy poprosić dla 30 rdzenie Menedżera zasobów we wszystkich regionach.

## <a name="invalid-content-link"></a>Nieprawidłowe łącze zawartości

Gdy pojawi się komunikat o błędzie:

    Code=InvalidContentLink
    Message=Unable to download deployment content from ...

Prawdopodobnie próbowano łącze do zagnieżdżony szablon, który nie jest dostępny. Sprawdź identyfikator URI przewidziane zagnieżdżony szablon. Jeśli szablon istnieje na koncie miejsca do magazynowania, upewnij się, że identyfikator URI jest dostępny. Konieczne może być przekazywane tokenu skojarzeń zabezpieczeń. Aby uzyskać więcej informacji zobacz [Korzystanie z szablonów połączone z Menedżerem zasobów Azure](resource-group-linked-templates.md).

## <a name="authorization-failed"></a>Autoryzacja nie powiodła się

Może zostać wyświetlony komunikat o błędzie podczas wdrażania, ponieważ konto lub usługę kapitału dołącza do wdrożenia zasobów nie ma dostępu do tych zadań. Azure Active Directory umożliwia, lub z administratorem, aby kontrolować, które tożsamości dostępem zasobów doskonałe stopień dokładności. Na przykład jeśli konta jest przypisana do roli czytnika, tracisz możliwość tworzenia zasobów. W takim przypadku pojawi się komunikat o błędzie wskazujący, że autoryzacja nie powiodła się.

Aby uzyskać więcej informacji na temat kontrola dostępu oparta na rolach zobacz [Kontrola dostępu Azure Role-Based](./active-directory/role-based-access-control-configure.md).

Oprócz kontrola dostępu oparta na rolach czynności wdrożenia może być ograniczone przez zasady dla tej subskrypcji. Za pomocą zasad administrator można wymusić konwencje wszystkich zasobów wdrożony w subskrypcji. Na przykład administrator może wymagać podania wartości określonego znacznika dla typu zasobu. Jeśli nie spełnia wymagań zasad, otrzymujesz komunikat o błędzie podczas wdrażania. Aby uzyskać więcej informacji na temat zasad zobacz [Używanie zasad zarządzania zasobami i kontrolować dostęp](resource-manager-policy.md).

## <a name="troubleshooting-virtual-machines"></a>Rozwiązywanie problemów z maszyn wirtualnych

| Błąd | Artykuły |
| -------- | ----------- |
| Błędy rozszerzenia skryptu niestandardowego | [Błędy rozszerzenia maszyn wirtualnych systemu Windows](./virtual-machines/virtual-machines-windows-extensions-troubleshoot.md)<br />lub<br />[Błędy rozszerzenia Linux maszyn wirtualnych](./virtual-machines/virtual-machines-linux-extensions-troubleshoot.md) |
| Obraz systemu operacyjnego błędy inicjowania obsługi administracyjnej | [Nowe błędy maszyn wirtualnych systemu Windows](./virtual-machines/virtual-machines-windows-troubleshoot-deployment-new-vm.md)<br />lub<br />[Nowe błędy Linux maszyn wirtualnych](./virtual-machines/virtual-machines-linux-troubleshoot-deployment-new-vm.md ) |
| Błędy alokacji | [Błędy alokacji maszyn wirtualnych systemu Windows](./virtual-machines/virtual-machines-windows-allocation-failure.md)<br />lub<br />[Błędy alokacji Linux maszyn wirtualnych](./virtual-machines/virtual-machines-linux-allocation-failure.md) |
| Secure Shell (SSH) błędów podczas próby połączenia | [Bezpieczne połączenia powłoki do maszyn wirtualnych Linux](./virtual-machines/virtual-machines-linux-troubleshoot-ssh-connection.md) |
| Nawiązywanie połączenia z aplikacją uruchomionych maszyn wirtualnych błędów | [Aplikacja na maszyn wirtualnych systemu Windows](./virtual-machines/virtual-machines-windows-troubleshoot-app-connection.md)<br />lub<br />[Aplikacja na maszyny Linux](./virtual-machines/virtual-machines-linux-troubleshoot-app-connection.md) |
| Podłączanie pulpitu zdalnego błędów | [Połączenia pulpitu zdalnego do maszyn wirtualnych systemu Windows](./virtual-machines/virtual-machines-windows-troubleshoot-rdp-connection.md) |
| Błędy połączeń rozwiązany, ponowne rozmieszczanie | [Ponownie wdróż maszyn wirtualnych do nowego węzła Azure](./virtual-machines/virtual-machines-windows-redeploy-to-new-node.md) |
| Błędy usługi w chmurze | [Problemy z chmury usługi wdrażania](./cloud-services/cloud-services-troubleshoot-deployment-problems.md) |

## <a name="troubleshooting-other-services"></a>Rozwiązywanie problemów z innymi usługami

Poniższa tabela jest pełną listę tematów dotyczących rozwiązywania problemów dla Azure. Zamiast tego omówiono w kwestiach związanych z wdrożeniem lub konfigurowania zasobów. Jeśli potrzebujesz pomocy, rozwiązywanie problemów z konfiguracją wykonywalna zasobów, zobacz dokumentację w tej usłudze Azure.

| Usługa | Artykuł |
| -------- | -------- |
| Automatyzacji | [Porady dotyczące rozwiązywania problemów dla typowych błędów w automatyzacji Azure](./automation/automation-troubleshooting-automation-errors.md) |
| Stos Azure | [Rozwiązywanie problemów z Microsoft Azure stos](./azure-stack/azure-stack-troubleshooting.md) |
| Stos Azure | [Stos Azure i aplikacji sieci Web](./azure-stack/azure-stack-webapps-troubleshoot-known-issues.md ) |
| Factory danych | [Rozwiązywanie problemów Factory danych](./data-factory/data-factory-troubleshoot.md) |
| Usługa tkaninie | [Rozwiązywanie typowych problemów podczas wdrażania usługi na tkaninie usługi Azure](./service-fabric/service-fabric-diagnostics-troubleshoot-common-scenarios.md) |
| Odzyskiwanie witryny | [Monitorowanie i rozwiązywanie problemów z ochroną dla maszyn wirtualnych i serwerów fizycznych](./site-recovery/site-recovery-monitoring-and-troubleshooting.md) |
| Miejsca do magazynowania | [Monitorowanie, diagnozowanie i rozwiązywanie problemów z magazynem tabel platformy Microsoft Azure](./storage/storage-monitoring-diagnosing-troubleshooting.md) |
| StorSimple | [Rozwiązywanie problemów wdrożenia urządzenia StorSimple](./storsimple/storsimple-troubleshoot-deployment.md) |
| Baza danych SQL | [Rozwiązywania problemów z połączeniem z bazą danych SQL Azure](./sql-database/sql-database-troubleshoot-common-connection-issues.md) |
| Magazyn danych SQL | [Rozwiązywanie problemów z magazynu danych Azure SQL](./sql-data-warehouse/sql-data-warehouse-troubleshoot.md) |

## <a name="understand-when-a-deployment-is-ready"></a>Opis po wdrożeniu jest gotowy

Azure Menedżera zasobów raportów pomyślnego wdrożenia podczas wszystkich dostawców zwrotu z wdrożenia pomyślnie. Jednak ten komunikat niekoniecznie oznacza, że grupy zasobów jest "aktywne i gotowe do użytkowników." Na przykład wdrożeniu może być konieczne Pobierz uaktualnienia, poczekaj na zasoby-template lub zainstalować skrypty złożone. Menedżer zasobów nie wiedzieć, kiedy te zadania ukończyć, ponieważ nie są one działania, które śledzi dostawcę. W takich przypadkach może być trochę czasu, zanim zasoby są gotowe do użycia rzeczywistych. W wyniku należy oczekiwać, że stanu rozmieszczania zakończyło się powodzeniem trochę czasu, zanim będzie można używać wdrożenia.

Aby zapobiec Azure raportowania pomyślnego wdrożenia, tworząc skryptu niestandardowego szablonu niestandardowego. Skrypt wie, jak monitorowanie całego wdrażanie gotowości systemowe i zwraca pomyślnie tylko wtedy, gdy użytkownicy mogą korzystać z całego wdrożenia. Jeśli chcesz upewnić się, że numer wewnętrzny jest ostatnia uruchomić, należy użyć właściwości **dependsOn** w szablonie.

## <a name="next-steps"></a>Następne kroki

- Aby dowiedzieć się o inspekcji akcje, zobacz [inspekcji operacji przy użyciu Menedżera zasobów](resource-group-audit.md).
- Aby dowiedzieć się o działania w celu określenia błędy podczas wdrażania, zobacz [Wyświetlanie wdrażania operacji](resource-manager-troubleshoot-deployments-portal.md).
