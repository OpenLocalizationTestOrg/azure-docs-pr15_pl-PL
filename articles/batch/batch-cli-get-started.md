<properties
   pageTitle="Wprowadzenie do polecenie partii Azure | Microsoft Azure"
   description="Krótkie wprowadzenie do poleceń partii w polecenie Azure zarządzania zasobami usługi Azure wsadowe"
   services="batch"
   documentationCenter=""
   authors="mmacy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="batch"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="multiple"
   ms.workload="big-compute"
   ms.date="09/30/2016"
   ms.author="marsma"/>

# <a name="get-started-with-azure-batch-cli"></a>Wprowadzenie do programu polecenie partii Azure

Interfejs wiersza polecenia Azure i platform (polecenie Azure) umożliwia zarządzanie kontami partii i zasobów, takich jak pule, zadania i zadania w powłoki poleceń Linux, Mac i Windows. Polecenie Azure można wykonać, a skrypt wielu zadań, które przeprowadzić z interfejsów API partię, Azure portal i partii poleceń cmdlet.

Ten artykuł jest oparty na polecenie Azure wersji 0.10.5.

## <a name="prerequisites"></a>Wymagania wstępne

* [Instalowanie polecenie Azure](../xplat-cli-install.md)

* [Łączenie polecenie Azure do subskrypcji usługi Azure](../xplat-cli-connect.md)

* Przełącz do **trybu Menedżera zasobów**:`azure config mode arm`

>[AZURE.TIP] Firma Microsoft zaleca aktualizację instalacji usługi Azure polecenie często, aby korzystać z usługi Aktualizacje i rozszerzenia.

## <a name="command-help"></a>Pomoc dla polecenia

Tekst pomocy dla każdego polecenia można wyświetlać w polecenie Azure, dołączając `-h` jako dostępna tylko po poleceniu. Na przykład:

* Aby uzyskać pomoc dotyczącą `azure` polecenia, wprowadź:`azure -h`
* Aby uzyskać listę wszystkich poleceń partii w interfejsu wiersza polecenia, należy użyć:`azure batch -h`
* Aby uzyskać informacje na temat tworzenia konta partię, wprowadź:`azure batch account create -h`

W razie wątpliwości za pomocą `-h` opcji wiersza polecenia, aby uzyskać pomoc dotyczącą dowolnego polecenia polecenie Azure.

## <a name="create-a-batch-account"></a>Tworzenie konta partii

Sposób użycia:

    azure batch account create [options] <name>

Przykład:

    azure batch account create --location "West US"  --resource-group "resgroup001" "batchaccount001"

Tworzy nowe konto partii przy użyciu określonych parametrów. Należy określić co najmniej lokalizacji, grupa zasobów i nazwę konta. Jeśli nie masz jeszcze grupa zasobów, utwórz ją, uruchamiając `azure group create`i określ regionów Azure (na przykład "Zachód NAMI") dla `--location` opcji. Na przykład:

    azure group create --name "resgroup001" --location "West US"

> [AZURE.NOTE] Nazwa konta partii musi być unikatowa w obszarze Azure utworzone konta. Może zawierać tylko znaki alfanumeryczne małe litery, a musi być 3-24 znaków. Nie można używać znaków specjalnych, takich jak `-` lub `_` w nazwach kont partię.

### <a name="linked-storage-account-autostorage"></a>Konta połączone przestrzeni dyskowej (autostorage)

(Opcjonalnie) można połączyć konta **uniwersalny** miejsca do magazynowania do swojego konta partii po jego utworzeniu. Funkcja [pakietów aplikacji](batch-application-packages.md) partii używa magazyn obiektów blob w połączonych uniwersalny konta miejsca do magazynowania, podobnie jak biblioteki [.NET konwencje plik wsadowy](batch-task-output.md) . Te funkcje opcjonalne pomóc w wdrażania aplikacji, uruchomienia zadań partię, a utrzymuje dane, które oferują.

Aby połączyć istniejącego konta magazynu platformy Azure na nowe konto partii po jego utworzeniu, podać `--autostorage-account-id` opcji. Ta opcja wymaga identyfikator zasobu w pełni kwalifikowana konta miejsca do magazynowania.

Najpierw wyświetlić szczegółowe informacje o koncie usługi miejsca do magazynowania:

    azure storage account show --resource-group "resgroup001" "storageaccount001"

Wpisz wartość w polu **adres Url** `--autostorage-account-id` opcji. Wartość adresu Url zaczyna się od "/ subskrypcje /" i zawiera Twojej subskrypcji zasobów i identyfikator ścieżkę dostępu do konta miejsca do magazynowania:

    azure batch account create --location "West US"  --resource-group "resgroup001" --autostorage-account-id "/subscriptions/8ffffff8-4444-4444-bfbf-8ffffff84444/resourceGroups/resgroup001/providers/Microsoft.Storage/storageAccounts/storageaccount001" "batchaccount001"

## <a name="delete-a-batch-account"></a>Usuwanie konta partii

Sposób użycia:

    azure batch account delete [options] <name>

Przykład:

    azure batch account delete --resource-group "resgroup001" "batchaccount001"

Usuwa z określonego konta partię. Po wyświetleniu monitu o potwierdzenie usunąć konto (usuwania konta może zająć trochę czasu, aby ukończyć).

## <a name="manage-account-access-keys"></a>Zarządzanie kontem klawiszy dostępu

Potrzebujesz klawisz dostępu do [tworzenia i modyfikowania zasobów](#create-and-modify-batch-resources) na koncie partię.

### <a name="list-access-keys"></a>Klawisze dostępu listy

Sposób użycia:

    azure batch account keys list [options] <name>

Przykład:

    azure batch account keys list --resource-group "resgroup001" "batchaccount001"

Wyświetla klawisze konta dla danego konta partię.

### <a name="generate-a-new-access-key"></a>Generowanie nowego klucza dostępu

Sposób użycia:

    azure batch account keys renew [options] --<primary|secondary> <name>

Przykład:

    azure batch account keys renew --resource-group "resgroup001" --primary "batchaccount001"

Ponownie wygenerować klucz określonego konta dla danego konta partię.

## <a name="create-and-modify-batch-resources"></a>Tworzenie i modyfikowanie partii zasobów

Polecenie Azure umożliwia tworzenie, Odczyt, zaktualizować i usuwanie partii (OBSŁUGIWAŁ) zasobów, takich jak pule, obliczyć węzły, zadania i zadania. Te operacje OBSŁUGIWAŁ wymagają partii nazwę konta, klawisz dostępu i punkt końcowy. Można je określić z `-a`, `-k`, i `-u` opcji lub zestawu [zmiennych środowiska](#credential-environment-variables) , używanym przez polecenie automatycznie (jeśli wypełnione).

### <a name="credential-environment-variables"></a>Zmienne środowiska poświadczeń

Można ustawić `AZURE_BATCH_ACCOUNT`, `AZURE_BATCH_ACCESS_KEY`, i `AZURE_BATCH_ENDPOINT` zmienne środowiska zamiast określanie `-a`, `-k`, i `-u` opcje dla każdego polecenia, możesz wykonać w wierszu polecenia. Polecenie partii korzysta z tych zmiennych (jeśli Ustawianie) tak, aby można pominąć `-a`, `-k`, i `-u` opcje. W dalszej części tego artykułu przyjęto założenie, użyj tych zmiennych środowiska.

>[AZURE.TIP] Lista kluczy z `azure batch account keys list`i wyświetlić końcowy konta z `azure batch account show`.

### <a name="json-files"></a>Pliki JSON

Po utworzeniu partii zasoby takie jak pul i zadań można określić JSON plik zawierający konfiguracji nowego zasobu zamiast przekazywania jej parametrów jako opcjom. Na przykład:

`azure batch pool create my_batch_pool.json`

O ile można wykonać wiele operacji tworzenia zasobów, używając tylko opcji wiersza polecenia, niektóre funkcje wymagają plik w formacie JSON z danymi zasobów. Na przykład należy użyć pliku JSON, jeśli chcesz określić pliki zasobów do zadań Rozpocznij.

Aby znaleźć JSON wymagane do utworzenia zasobu, należy zapoznać się z [interfejsu API usługi REST partii odwołań] [ rest_api] dokumentacji w witrynie MSDN. Każdy temat "Dodaj *Typ zasobu"*zawiera przykład JSON związane z tworzeniem zasób, którego można użyć jako szablonów plików JSON. Na przykład JSON tworzenia puli znajdują się w [Dodaj puli z klientem][rest_add_pool].

>[AZURE.NOTE] Jeśli użytkownik określi plik JSON, podczas tworzenia zasobu, wszystkie inne parametry określające dla tego zasobu w wierszu polecenia są ignorowane.

## <a name="create-a-pool"></a>Tworzenie puli

Sposób użycia:

    azure batch pool create [options] [json-file]

Przykład (Konfiguracja maszyn wirtualnych):

    azure batch pool create --id "pool001" --target-dedicated 1 --vm-size "STANDARD_A1" --image-publisher "Canonical" --image-offer "UbuntuServer" --image-sku "14.04.2-LTS" --node-agent-id "batch.node.ubuntu 14.04"

Przykład (Konfiguracja usługi Cloud):

    azure batch pool create --id "pool002" --target-dedicated 1 --vm-size "small" --os-family "4"

Tworzy pulę węzłów obliczeń w usłudze partię.

Jak wskazano w [partii omówienie funkcji](batch-api-basics.md#pool), masz dwie opcje, po wybraniu system operacyjny dla węzłów w puli użytkownika: **Konfiguracja maszyn wirtualnych** i **Konfiguracja usług w chmurze**. Używanie `--image-*` opcji służących do tworzenia pul konfigurację maszyny wirtualnej i `--os-family` na tworzenie puli Konfiguracja usług w chmurze. Nie można określić oba `--os-family` i `--image-*` opcje.

Możesz określić puli [pakietów aplikacji](batch-application-packages.md) i wiersz polecenia [Uruchom zadanie](batch-api-basics.md#start-task). Aby określić pliki zasobów do zadań Rozpocznij, jednak należy używać [pliku JSON](#json-files).

Usuwanie puli z:

    azure batch pool delete [pool-id]

>[AZURE.TIP] Sprawdź wartości odpowiednie dla [listy obrazów maszyn wirtualnych](batch-linux-nodes.md#list-of-virtual-machine-images) `--image-*` opcje.

## <a name="create-a-job"></a>Tworzenie zadania

Sposób użycia:

    azure batch job create [options] [json-file]

Przykład:

    azure batch job create --id "job001" --pool-id "pool001"

Dodaje zadanie do konta partii i określić puli wykonać swoich zadań.

Usuwanie zadania z:

    azure batch job delete [job-id]

## <a name="list-pools-jobs-tasks-and-other-resources"></a>Pule listy, zadania, zadania i inne zasoby

Typ zasobu każdej partii obsługuje `list` polecenia, który konta partii kwerendy i wyświetla listę zasobów tego typu. Na przykład można wyświetlić listę pul konta i zadań w ramach zadania:

    azure batch pool list
    azure batch task list --job-id "job001"

### <a name="listing-resources-efficiently"></a>Efektywne listę zasobów

Do wykonywania kwerend szybsze można określić **Wybierz**, **filtrowania**i **Rozwiń** opcje klauzula dla `list` operacji. Użyj tych opcji, aby ograniczyć ilość danych zwróconych przez usługę partię. Ponieważ wszystkie filtry występuje po stronie serwera, tylko tych danych, który Cię interesuje przecina sieci. Zapisywanie przepustowości (i w związku z tym czas) za pomocą tych klauzul podczas wykonywania operacji na liście.

Na przykład ten zwróci tylko pul, których nazwy zaczynają się od "renderTask":

    azure batch task list --job-id "job001" --filter-clause "startswith(id, 'renderTask')"

Polecenie partii obsługuje wszystkie trzy klauzule obsługiwane przez usługę partii:

* `--select-clause [select-clause]`Zwraca podzbiór właściwości dla każdego obiektu
* `--filter-clause [filter-clause]`Zwraca tylko jednostki, które są zgodne z określonego wyrażenia OData
* `--expand-clause [expand-clause]`Uzyskiwać informacje jednostki w jednym źródłowych połączenia pozostałych. Klauzula rozwiń obsługuje tylko `stats` właściwości w tej chwili.

Aby uzyskać szczegółowe informacje na trzy klauzule i wykonywanie zapytań listy z nimi zobacz [wydajność kwerendy usługi Azure partię](batch-efficient-list-queries.md).

## <a name="application-package-management"></a>Zarządzanie aplikacjami pakietu

Pakiety aplikacji umożliwiają uproszczone wdrażanie aplikacji węzły obliczeń w puli usługi. Z polecenie Azure można przekazać pakietów aplikacji, zarządzanie wersjami pakietu i usuwać pakiety.

Aby utworzyć nową aplikację i dodać wersja pakietu:

**Tworzenie** aplikacji:

    azure batch application create "resgroup001" "batchaccount001" "MyTaskApplication"

**Dodawanie** pakietu aplikacji:

    azure batch application package create "resgroup001" "batchaccount001" "MyTaskApplication" "1.10-beta3" package001.zip

**Aktywuj** pakiet:

    azure batch application package activate "resgroup001" "batchaccount001" "MyTaskApplication" "1.10-beta3" zip

Ustawianie **domyślnej wersji** aplikacji:

    azure batch application set "resgroup001" "batchaccount001" "MyTaskApplication" --default-version "1.10-beta3"

### <a name="deploy-an-application-package"></a>Wdrażanie pakietu aplikacji

Po utworzeniu nowej puli, można określić jeden lub więcej pakietów aplikacji do wdrożenia. Po określeniu pakiet w czasie tworzenia puli jako pula sprzężenia węzeł zostanie wdrożony w każdym węźle. Pakiety również są wdrażane, gdy węzeł zostanie ponownie uruchomić lub reimaged.

Określanie `--app-package-ref` opcji po utworzeniu puli wdrożenia pakietu aplikacji w puli węzły podczas dołączania puli. `--app-package-ref` Opcja akceptuje rozdzielaną średnikami listę identyfikatorów aplikacji wdrożenia węzły obliczeń.

    azure batch pool create --pool-id "pool001" --target-dedicated 1 --vm-size "small" --os-family "4" --app-package-ref "MyTaskApplication"

Po utworzeniu puli przy użyciu opcji wiersza polecenia, obecnie nie można określić wersję pakietu aplikacji, *która* ma być Wdroż węzły obliczeń, na przykład "1.10-beta3". W związku z tym, najpierw musisz określić domyślną wersję aplikacji z `azure batch application set [options] --default-version <version-id>` przed utworzeniem puli (zobacz poprzedniej sekcji). Jeśli zostanie użyty [plik JSON](#json-files) zamiast opcje wiersza polecenia po utworzeniu puli, można jednak określić wersja pakietu puli.

Więcej informacji na temat pakietów aplikacji można znaleźć w [wdrażaniem aplikacji za pomocą pakietów aplikacji partii Azure](batch-application-packages.md).

>[AZURE.IMPORTANT] Należy [łącze konta magazynu platformy Azure](#linked-storage-account-autostorage) do swojego konta partii, aby skorzystać z pakietów aplikacji.

### <a name="update-a-pools-application-packages"></a>Aktualizowanie puli pakietów aplikacji

Aby zaktualizować aplikacje przypisane do istniejącej puli, problemów `azure batch pool set` polecenia z `--app-package-ref` opcję:

    azure batch pool set --pool-id "pool001" --app-package-ref "MyTaskApplication2"

Aby wdrożyć pakiet aplikacji do obliczenia węzły już w istniejącej puli, należy ponownie uruchomić lub reimage tych węzłów:

    azure batch node reboot --pool-id "pool001" --node-id "tvm-3105992504_1-20160930t164509z"

>[AZURE.TIP] Można uzyskać listę węzłów w puli, wraz z ich identyfikatory węzeł z `azure batch node list`.

Należy pamiętać, że należy już skonfigurować aplikację przy użyciu wersji domyślnej przed wdrożeniem (`azure batch application set [options] --default-version <version-id>`).

## <a name="troubleshooting-tips"></a>Porady dotyczące rozwiązywania problemów

Ta sekcja jest przeznaczona do zapewnienia zasoby, aby używać podczas rozwiązywania problemów, polecenie Azure. Nie rozwiązuje ona niekoniecznie wszystkie problemy, ale go może pomóc w zawężenia przyczyny i punkt zasoby pomocy.

* Używanie `-h` uzyskać **tekst pomocy** dla dowolnego polecenia interfejsu wiersza polecenia

* Używanie `-v` i `-vv` Wyświetla **szczegółowe** informacje polecenie; `-vv` jest "dodatkowe" pełne i wyświetla rzeczywiste żądania usługi REST i odpowiedzi. Te przełączniki są przydatne do wyświetlania pełnego komunikaty o błędach wyświetlane.

* Można wyświetlić **oznaczona jako JSON** z `--json` opcji. Na przykład `azure batch pool show "pool001" --json` Wyświetla właściwości osoby pool001 w formacie JSON. Można kopiować i modyfikowanie tego raportu, aby użyć w `--json-file` (zobacz [JSON pliki](#json-files) w tym artykule).

* [Partia forum w witrynie MSDN] [ batch_forum] jest zasobem Pomoc i jest monitorowane przez członków zespołów partię. Pamiętaj ogłaszać pytania, jeśli napotkasz problemy lub chcesz uzyskać pomoc dotyczącą określonej operacji.

* Nie wszystkie operacje zasobów partii jest obecnie obsługiwane przez polecenie Azure. Na przykład obecnie nie można określić pakietu aplikacji *w wersji* dla zestawu identyfikatora pakietu W takich przypadkach może być konieczne jest podanie `--json-file` dla polecenia zamiast opcje wiersza polecenia. Pamiętaj o bieżąco z najnowszą wersją interfejsu wiersza polecenia do pobrania ulepszenia przyszłych.

## <a name="next-steps"></a>Następne kroki

*  Zobacz [wdrażaniem aplikacji za pomocą pakietów aplikacji partii Azure](batch-application-packages.md) , aby dowiedzieć się, jak używać tej funkcji do zarządzania i wdrażanie aplikacji, których wykonanie w węzłach do uruchamiania partię.

* Zobacz [wydajność kwerendy usługi partii](batch-efficient-list-queries.md) dodatkowe informacje o zmniejszenie liczby elementów i typ informacji, które są zwracane dla kwerend do partii.

[batch_forum]: https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=azurebatch
[github_readme]: https://github.com/Azure/azure-xplat-cli/blob/dev/README.md
[rest_api]: https://msdn.microsoft.com/library/azure/dn820158.aspx
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx