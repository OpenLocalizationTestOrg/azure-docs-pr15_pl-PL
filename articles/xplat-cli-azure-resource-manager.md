
<properties
    pageTitle="Zarządzanie zasobami za pomocą interfejsu wiersza polecenia Azure | Microsoft Azure"
    description="Zarządzanie Azure zasoby i grupy za pomocą interfejsu wiersza polecenia Azure (polecenie)"
    editor=""
    manager="timlt"
    documentationCenter=""
    authors="dlepow"
    services="azure-resource-manager"/>

<tags
    ms.service="azure-resource-manager"
    ms.workload="multiple"
    ms.tgt_pltfrm="vm-multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="danlep"/>

# <a name="use-the-azure-cli-to-manage-azure-resources-and-resource-groups"></a>Zarządzanie Azure zasobów i grup zasobów za pomocą interfejsu wiersza polecenia Azure


> [AZURE.SELECTOR]
- [Portal](azure-portal/resource-group-portal.md) 
- [Polecenie Azure](xplat-cli-azure-resource-manager.md)
- [Azure programu PowerShell](powershell-azure-resource-manager.md)
- [INTERFEJSU API USŁUGI REST](resource-manager-rest-api.md)


Interfejs wiersza polecenia Azure (polecenie Azure) jest jednym z kilku narzędzi, które umożliwia wdrażanie i zarządzanie zasobami za pomocą Menedżera zasobów. W tym artykule przedstawiono typowe sposoby zarządzania Azure zasobów i grup zasobów za pomocą interfejsu wiersza polecenia Azure w trybie Menedżera zasobów. Aby dowiedzieć się, jak wdrożyć zasobów za pomocą interfejsu wiersza polecenia zobacz [zasoby rozmieszczanie z szablonami Menedżera zasobów i polecenie Azure](resource-group-template-deploy-cli.md). Tło o zasobach Azure i Menedżera zasobów można znaleźć w [Azure Omówienie Menedżera zasobów](azure-resource-manager/resource-group-overview.md).

>[AZURE.NOTE] Aby zarządzać zasobami Azure za pomocą interfejsu wiersza polecenia Azure, należy [zainstalować polecenie Azure](xplat-cli-install.md)i [Zaloguj się do Azure](xplat-cli-connect.md) za pomocą `azure login` polecenia. Upewnij się, polecenie jest w trybie Menedżera zasobów (Uruchom `azure config mode arm`). Jeśli po wykonaniu kwestie, o których jesteś gotowa do wysłania.



## <a name="get-resource-groups-and-resources"></a>Pobieranie grupy zasobów i zasoby

### <a name="resource-groups"></a>Grupy zasobów

Aby uzyskać listę wszystkich grup zasobów w subskrypcji i ich lokalizacji, uruchom polecenie.

    azure group list
    

### <a name="resources"></a>Zasoby
 Aby wyświetlić listę wszystkich zasobów w grupie, takie jak jedną z nazw *testRG*, użyj następującego polecenia.

    azure resource list testRG

Aby wyświetlić poszczególnych zasobów w grupie, takie jak maszyny o nazwie *MyUbuntuVM*, należy użyć polecenia podobnej do następującej.

    azure resource show testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15"
    
Zwróć uwagę, parametr **Microsoft.Compute/virtualMachines** . Ten parametr wskazuje typ zasobu, do której chcesz uzyskać informacje na.
    
>[AZURE.NOTE]Korzystając z poleceń **azure zasobów** innych niż polecenia **list** , musisz określić wersji interfejsu API zasobu przy użyciu parametru **-o** . Jeśli nie wiesz, o wersji interfejsu API, można znaleźć w pliku szablonu, a następnie znajdź pole apiVersion dla zasobu. Aby uzyskać więcej informacji o wersji interfejsu API w Menedżerze zasobów zobacz [dostawców Menedżera zasobów, regiony, wersje interfejsu API i schematów](resource-manager-supported-services.md).

Podczas wyświetlania szczegółów na zasób, warto często za pomocą `--json` parametru. Ten parametr powoduje, że dane wyjściowe bardziej czytelny, ponieważ niektóre wartości są struktury zagnieżdżone lub zbiory. Poniższy przykład pokazuje zwracająca wyniki polecenia **Pokaż** jako dokument JSON.

    azure resource show testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15" --json

>[AZURE.NOTE] Jeśli chcesz, Zapisz dane JSON do pliku przy użyciu &gt; znaku skierowania danych wyjściowych do pliku. Na przykład:
>
> `azure resource show testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15" --json > myfile.json`

### <a name="tags"></a>Znaczniki

[AZURE.INCLUDE [resource-manager-tag-resources-cli](../includes/resource-manager-tag-resources-cli.md)]

## <a name="manage-resources"></a>Zarządzanie zasobami


Aby dodać zasób, takie jak konto miejsca do magazynowania do grupy zasobów, uruchom polecenie podobne do:

    azure resource create testRG MyStorageAccount "Microsoft.Storage/storageAccounts" "westus" -o "2015-06-15" -p "{\"accountType\": \"Standard_LRS\"}"
    
Oprócz określanie wersji interfejsu API zasobu przy użyciu parametru **-o** , należy użyć parametru **-p** w celu przekazania ciąg sformatowany JSON z wszystkie wymagane lub dodatkowe właściwości.
    
    
Aby usunąć istniejący zasób, takich jak zasób maszyny wirtualnej, użyj następującego polecenia.

    azure resource delete testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15"

Aby przenieść istniejących zasobów do innej grupy zasobów lub subskrypcji, użyj polecenia **Przenieś azure zasobów** . W poniższym przykładzie pokazano, jak przenieść pamięci podręcznej Redis do nowej grupy zasobów. W parametrze **-i** Opis Podaj jest rozdzielaną średnikami listę identyfikator zasobu, aby przenieść.


    azure resource move -i "/subscriptions/{guid}/resourceGroups/OldRG/providers/Microsoft.Cache/Redis/examplecache" -d "NewRG"

## <a name="control-access-to-resources"></a>Kontrolowanie dostępu do zasobów

Polecenie Azure umożliwia tworzenie i zarządzanie zasadami kontrolowanie dostępu do zasobów Azure. Aby uzyskać ogólne informacje o definicje zasad i przypisywanie zasad do zasobów zobacz [zasady zarządzania zasobami i kontrolowania dostępu za pomocą](resource-manager-policy.md).

Na przykład Definiowanie następującą zasadę odmówić wszystkie żądania miejsce, w którym lokalizacja jest zachód USA lub Ameryka Północna centralnej US i zapisanie go w policy.json pliku definicji zasad:

    {
    "if" : {
        "not" : {
        "field" : "location",
        "in" : ["westus" ,  "northcentralus"]
        }
    },
    "then" : {
        "effect" : "deny"
    }
    }

Następnie uruchom polecenie **utworzenie definicji zasad** :

    azure policy definition create MyPolicy -p c:\temp\policy.json
    
Polecenie to wyświetla wynik podobny do następującego.

    + Tworzenie zasad definicji danych MyPolicy: Nazwa_zasady: MyPolicy danych: PolicyDefinitionId: /subscriptions/###-###-###-###-###/providers/Microsoft.Authorization/policyDefinitions/MyPolicy

    dane: element PolicyType: dane niestandardowej: DisplayName: zdefiniowane danych: Opis: zdefiniowane danych: PolicyRule: pole = lokalizacji, w = [westus, northcentralus], efektu = odmówić

 Aby przypisać zasady w zakresie, należy użyć **PolicyDefinitionId** zwrócony przez polecenie poprzednie. W poniższym przykładzie ten zakres jest subskrypcji, ale można ograniczyć do grup zasobów lub poszczególnych zasobów:

    Tworzenie przypisania Azure zasad MyPolicyAssignment -p /subscriptions/###-###-###-###-###/providers/Microsoft.Authorization/policyDefinitions/MyPolicy -s /subscriptions/###-###-###-###-###-

Można uzyskać, zmienić lub usunąć definicje zasad przy użyciu polecenia **Pokaż definicji zasad**, **Ustawianie zasad definicji**i **Usuwanie definicji zasad** .

Podobnie można uzyskać, zmienianie lub usuwanie przypisań zasad za pomocą polecenia **Pokaż przypisania zasad**, **Ustawianie przydziałów zasad**i **Usuwanie przypisania zasad** .


## <a name="export-a-resource-group-as-a-template"></a>Eksportowanie grupa zasobów jako szablonu

W przypadku istniejącej grupy zasobów można wyświetlać szablonu Menedżera zasobów dla grupy zasobów. Eksportowanie szablon oferuje dwie korzyści:

1. Można łatwo zautomatyzować przyszłych wdrożeń rozwiązanie, ponieważ wszystkie infrastruktury jest zdefiniowana w szablonie.

2. Czy zapoznanie się ze składni szablonu sprawdzając JSON, reprezentująca rozwiązania.

Za pomocą interfejsu wiersza polecenia Azure, możesz można wyeksportować szablonu, który odpowiada bieżącemu stanowi grupy zasobów lub pobrać szablon, którego użyto do określonego wdrożenia.

* **Eksportowanie szablonu dla grupy zasobów** — jest to pomocne, gdy wprowadzasz zmiany w grupie zasobów, a następnie należy pobrać reprezentacją JSON jego bieżącym stanie. Jednak wygenerowane szablon zawiera tylko z minimalnej liczby parametrów i Brak zmiennych. Większość wartości w szablonie są stałe. Przed wdrożeniem wygenerowane szablonu, możesz przekonwertować więcej wartości parametrów, możesz dostosować wdrożenia w różnych środowiskach.

    Aby wyeksportować szablon dla grupy zasobów do katalogu lokalnego, uruchom `azure group export` polecenia, jak pokazano w poniższym przykładzie. (Podstaw katalogu lokalnego odpowiednie dla środowiska systemu operacyjnego).

        azure group export testRG ~/azure/templates/

* **Pobieranie szablonu dla danego wdrożenia** — jest to pomocne, gdy zachodzi potrzeba wyświetlenia rzeczywisty szablon, którego użyto do wdrożenia zasobów. Szablon zawiera wszystkie parametry i zmienne zdefiniowane wdrożenia oryginalny. Jednak w inną osobę w organizacji wprowadzili zmiany w grupie zasobów poza definicji w szablonie, ten szablon nie zawiera bieżący stan grupy zasobów.

    Aby pobrać szablon używany dla danego wdrożenia do katalogu lokalnego, uruchom `azure group deployment template download` polecenia. Na przykład:

        azure group deployment template download TestRG testRGDeploy ~/azure/templates/downloads/
 
>[AZURE.NOTE] Eksportowanie szablonu będzie na podglądzie, a nie wszystkich typów zasobów jest obecnie obsługiwany eksportowania szablonu. Podczas próby eksportowania szablonu, może zostać wyświetlony komunikat o błędzie informujący, że niektóre zasoby nie zostały wyeksportowane. Jeśli to konieczne, ręcznie zdefiniować tych zasobów w szablonie po pobraniu go.



## <a name="next-steps"></a>Następne kroki

* Aby uzyskać szczegółowe informacje o operacji rozmieszczania i rozwiązywanie problemów z błędami wdrażania z polecenie Azure, zobacz [Wyświetlanie operacji wdrażania z polecenie Azure](resource-manager-troubleshoot-deployments-cli.md).
* Jeśli chcesz skonfigurować aplikację lub skrypt dostępu do zasobów za pomocą interfejsu wiersza polecenia, zobacz [Używanie polecenie Azure utworzyć usługi kapitału dostępu do zasobów](resource-group-authenticate-service-principal-cli.md).


