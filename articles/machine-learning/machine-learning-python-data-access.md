<properties 
    pageTitle="Dostęp do zestawów danych z biblioteką klienta maszynowego uczenia Python | Microsoft Azure" 
    description="Instalowanie i używanie Biblioteka klienta Python uzyskanie dostępu i zarządzanie danymi Azure maszynowego uczenia bezpieczne z lokalnego środowiska Python." 
    services="machine-learning" 
    documentationCenter="python" 
    authors="bradsev" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/12/2016" 
    ms.author="huvalo;bradsev" />


#<a name="access-datasets-with-python-using-the-azure-machine-learning-python-client-library"></a>Zestawy danych programu Access z używania biblioteki klienta Azure maszynowego uczenia Python Python 

Podgląd Biblioteka klienta programu Microsoft Azure maszynowego uczenia Python można włączyć bezpieczny dostęp do usługi Azure maszynowego uczenia zestawy danych z lokalnego środowiska Python i umożliwia tworzenie i zarządzanie zestawów danych w obszarze roboczym.

Ten temat zawiera instrukcje dotyczące:

* Instalowanie maszynowego uczenia Python Biblioteka klienta 
* dostęp i przekaż zestawy danych, w tym instrukcje dotyczące uzyskać do nich dostęp Azure maszynowego uczenia zestawy danych z lokalnego środowiska Python
*  pośrednie zestawy danych programu Access z doświadczeń
*  Wyliczanie zestawy danych, dostęp do metadanych, odczytanie zawartości zestawu danych, tworzenie nowych baz danych i zaktualizować istniejące zestawy danych za pomocą Python Biblioteka klienta

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

 
##<a name="prerequisites"></a>Wymagania wstępne

Biblioteka klienta Python przetestowano w obszarze następujących środowiskach:

 - Windows, Mac i Linux
 - 2.7 Python, 3.3 i 3.4

Zależność ma przesyłek następujące czynności:

 - żądania
 - Python dateutil
 - pandas

Zalecamy używanie zainstalowania rozkładu Python takie jak [Anaconda](http://continuum.io/downloads#all) lub [korony](https://store.enthought.com/downloads/), pochodzące z Python, IPython i trzy pakietów wymienionych powyżej. IPython nie jest ściśle wymagane, ale jest doskonałe środowisko do manipulowania i interakcyjne wizualizacji danych.


###<a name="installation"></a>Jak zainstalować Azure maszynowego uczenia Python Biblioteka klienta

Biblioteka klienta Azure maszynowego uczenia Python musi być zainstalowany również do wykonania zadania opisane w tym temacie. Jest ona dostępna z [Python pakiet indeksu](https://pypi.python.org/pypi/azureml). Aby zainstalować go w środowisku Python, uruchom następujące polecenie w środowisku lokalnym Python:

    pip install azureml

Można też kliknąć można pobrać i zainstalować ze źródeł w [github](https://github.com/Azure/Azure-MachineLearning-ClientLibrary-Python).

    python setup.py install

Jeśli masz zainstalowany na komputerze cyfra umożliwia pip zainstalować bezpośrednio z repozytorium cyfra:

    pip install git+https://github.com/Azure/Azure-MachineLearning-ClientLibrary-Python.git


##<a name="datasetAccess"></a>Uzyskiwanie dostępu do zestawów danych za pomocą wstawki kodu Studio

Biblioteka klienta Python umożliwia programowy dostęp do Twojej istniejących zestawów danych z doświadczeń, które zostało uruchomione.

Przy użyciu interfejsu sieci web Studio można wygenerować wstawki kodu, zawierające wszystkie informacje potrzebne do pobierania i deserializacji zestawy danych jako obiekty Pandas DataFrame na komputerze lokalizację.

### <a name="security"></a>Zabezpieczenia, aby uzyskać dostęp do danych

Fragmenty kodu przewidziane przez Studio używanej w połączeniu z biblioteki klienta Python zawiera swoją nazwę obszaru roboczego i autoryzacji token. Te udostępnia pełny dostęp do obszaru roboczego i muszą być chronione, takich jak hasło.

Ze względów bezpieczeństwa funkcji wstawkę kodu jest dostępna tylko dla użytkowników, których ich roli ustawiony jako **właściciel** obszaru roboczego. Twoja rola jest wyświetlana w Azure maszynowego uczenia Studio na stronie **Użytkownicy** w obszarze **Ustawienia**.

![Zabezpieczenia][security]

Jeśli Twoja rola nie jest ustawiona jako **właściciel**, możesz albo żądanie można reinvited jako właściciel lub poproś właściciela obszaru roboczego do zapewnienia wstawkę kodu.

Aby uzyskać token autoryzacji, wykonaj jedną z następujących czynności:



- Zapytanie dotyczące tokenu od właściciela. Właściciele dostęp do swoich tokenów autoryzacji z poziomu strony ustawień ich obszaru roboczego w Studio. W okienku po lewej stronie wybierz pozycję **Ustawienia** , a następnie kliknij pozycję **TOKENY autoryzacji** Zobacz tokeny głównego i pomocniczego.  Mimo że podstawowego lub tokeny pomocniczej autoryzacji mogą zostać użyte w wstawkę kodu, zaleca się, że właściciele tylko udostępnianie tokeny autoryzacji pomocniczą.

![](./media/machine-learning-python-data-access/ml-python-access-settings-tokens.png)

- Poproś mają być promowane do roli właściciela.  Aby to zrobić, bieżący właściciel obszaru roboczego należy najpierw usunąć użytkownika z obszaru roboczego, a następnie ponownie zaprosić użytkownika do niego jako właściciel.

Po deweloperów uzyskaniu identyfikatora obszaru roboczego i autoryzacji token, są one mieli dostęp do obszaru roboczego przy użyciu wstawkę kodu, niezależnie od ich roli.

Tokeny autoryzacji zarządza się na stronie **TOKENY autoryzacji** w obszarze **Ustawienia**. Można odtworzyć, ale tej procedury odwołuje dostęp do poprzedniego token.

### <a name="accessingDatasets"></a>Zestawy danych programu Access z aplikacji lokalnej Python

1. W Studio nauki komputera na pasku nawigacyjnym po lewej stronie kliknij pozycję **zestawy danych** .

2. Wybierz zestaw danych, do którego chcesz uzyskać dostęp. Można wybrać dowolny zestawy danych, z listy **Moje zestawy danych** lub na liście **próbki** .

3. Na pasku narzędzi u dołu kliknij pozycję **Generuj kod dostępu do danych**. Jeśli dane znajdują się w formacie zgodna z biblioteki klienta Python, ten przycisk jest wyłączony.

    ![Zestawy danych][datasets]

4. Wybierz wstawkę kodu w oknie wyświetlany, a następnie skopiować go do Schowka.

    ![Kod dostępu][dataset-access-code]

5. Wklej kod do notesu lokalnych aplikacji Python.

    ![Notes][ipython-dataset]

## <a name="accessingIntermediateDatasets"></a>Pośrednie zestawy danych programu Access z doświadczeń nauki komputera

Po uruchomieniu doświadczenia w Studio nauki komputera, prawdopodobnie dostęp pośrednie zestawy danych z węzły dane wyjściowe modułów. Zestawy danych pośrednie są dane, które zostały utworzone i na potrzeby pośredniej czynności po uruchomieniu narzędzia modelu.

Jak format danych jest zgodny z biblioteki klienta Python są dostępne pośrednie zestawy danych.

Obsługiwane są następujące formaty (stałe dla nich znajdują się w `azureml.DataTypeIds` klasy):

 - Zwykły tekst
 - GenericCSV
 - GenericTSV
 - GenericCSVNoHeader
 - GenericTSVNoHeader

Format można określić, przez umieszczenie wskaźnika myszy na węźle dane wyjściowe modułu. Jest wyświetlany wraz z nazwę węzła w etykietce narzędzia.

Niektóre z modułów, takich jak [Dzielenie] [ split] moduł, w formacie o nazwie `Dataset`, nie jest obsługiwane przez Python Biblioteka klienta.

![Formatowanie zestawu danych][dataset-format]

Należy użyć modułu konwersji, takich jak [przekonwertować CSV][convert-to-csv], aby uzyskać wydruk do formatu obsługiwanego.

![GenericCSV Format][csv-format]

Poniższe kroki pokazują przykładowi, który tworzy doświadczenia, uruchamia je i uzyskuje dostęp do pośredniej zestawu danych.

1. Tworzenie nowego doświadczenia.

2. Wstawianie modułu **dorosłego klasyfikacji binarne dochód spisu zestawu danych** .

3. Wstawianie [podziału] [ split] modułu i łączenie jej wprowadzania w wyniku modułu zestawu danych.

4. Wstawianie, [Konwertowanie CSV] [ convert-to-csv] modułu i łączenie jej dane wejściowe do jednego [podziału] [ split] Wyświetla modułu.

5. Zapisywanie doświadczenia, uruchom go i poczekaj, aż do jego zakończenia działania.

6. Kliknij węzeł dane wyjściowe na [Konwertowanie CSV] [ convert-to-csv] modułu.

7. Po wyświetleniu menu kontekstowego wybierz pozycję **Generuj kod dostępu do danych**.

    ![Menu kontekstowe][experiment]

8. Wybierz wstawkę kodu i skopiować go do Schowka w wyświetlonym oknie.

    ![Kod dostępu][intermediate-dataset-access-code]

9. Wklej kod w notesie.

    ![Notes][ipython-intermediate-dataset]

10. Czy wizualizacji danych przy użyciu matplotlib. Spowoduje to wyświetlenie w postaci histogramu w kolumnie wieku:

    ![Histogram][ipython-histogram]


##<a name="clientApis"></a>Biblioteka klienta Python nauki komputera za pomocą dostępu, przeczytaj, tworzyć i zarządzać nimi zestawy danych

### <a name="workspace"></a>Obszar roboczy

Obszar roboczy jest punktu wejścia w bibliotece Python klienta. Podaj `Workspace` zajęć przy użyciu swojego identyfikatora obszaru roboczego i autoryzacji token do utworzenia wystąpienia:

    ws = Workspace(workspace_id='4c29e1adeba2e5a7cbeb0e4f4adfb4df',
                   authorization_token='f4f3ade2c6aefdb1afb043cd8bcf3daf')


### <a name="enumerate-datasets"></a>Wyliczanie zestawy danych

Wyliczyć wszystkie zestawy danych w danym obszarze roboczym:

    for ds in ws.datasets:
        print(ds.name)

Wyliczyć tylko utworzone przez użytkownika zestawy danych:

    for ds in ws.user_datasets:
        print(ds.name)

Wyliczyć tylko zestawy danych przykład:

    for ds in ws.example_datasets:
        print(ds.name)

Można uzyskać dostęp do zestawu danych według nazwy (który jest uwzględniana wielkość liter):

    ds = ws.datasets['my dataset name']

Lub dostępem przez indeks:

    ds = ws.datasets[0]


### <a name="metadata"></a>Metadane

Zestawy danych mają metadane, oprócz zawartości. (Pośrednie zestawy danych są wyjątek od tej reguły i nie ma wszystkie metadane).

Niektóre wartości metadanych są przypisywane przez użytkownika w czasie tworzenia:

    print(ds.name)
    print(ds.description)
    print(ds.family_id)
    print(ds.data_type_id)

Inne osoby są przypisane ml Azure wartości:

    print(ds.id)
    print(ds.created_date)
    print(ds.size)

Zobacz `SourceDataset` zajęć, aby uzyskać więcej informacji na temat dostępnych metadanych.


### <a name="read-contents"></a>Odczytanie zawartości

Wstawki kodu dostarczony przez maszynowego uczenia Studio automatycznie pobierać i deserializacji zestawu danych do obiektu Pandas DataFrame. To jest wykonywane przy `to_dataframe` metody:

    frame = ds.to_dataframe()

Jeśli wolisz pobrać dane i wykonać deserializacji samodzielnie, który jest opcja. W momencie jest to jedyna opcja formaty takie jak "ARFF", którego nie można rozszeregować Python Biblioteka klienta.

Aby odczytać zawartość jako tekst:

    text_data = ds.read_as_text()

Aby odczytać zawartość jako binarne:

    binary_data = ds.read_as_binary()

Możesz też po prostu otworzyć strumienia do zawartości:

    with ds.open() as file:
        binary_data_chunk = file.read(1000)


### <a name="create-a-new-dataset"></a>Tworzenie nowego zestawu danych

Biblioteka klienta Python umożliwia przekazywanie zestawy danych z programu Python. Te zestawy danych będą dostępne do użycia w obszarze roboczym.

Jeśli masz dane w Pandas DataFrame, należy użyć następującego kodu:

    from azureml import DataTypeIds

    dataset = ws.datasets.add_from_dataframe(
        dataframe=frame,
        data_type_id=DataTypeIds.GenericCSV,
        name='my new dataset',
        description='my description'
    )

Jeśli dane są już seryjny, możesz użyć:

    from azureml import DataTypeIds

    dataset = ws.datasets.add_from_raw_data(
        raw_data=raw_data,
        data_type_id=DataTypeIds.GenericCSV,
        name='my new dataset',
        description='my description'
    )

Biblioteka klienta Python jest w stanie szeregować DataFrame Pandas do następujących formatów (stałe dla nich znajdują się w `azureml.DataTypeIds` klasy):

 - Zwykły tekst
 - GenericCSV
 - GenericTSV
 - GenericCSVNoHeader
 - GenericTSVNoHeader


### <a name="update-an-existing-dataset"></a>Aktualizowanie istniejącego zestawu danych

Jeśli spróbujesz Przekaż nowy zestaw danych przy użyciu nazwy, która jest zgodna z istniejącego zestawu danych, należy uzyskać błąd konfliktu.

Aby zaktualizować istniejący zestaw danych, należy najpierw odwołać się do istniejącego zestawu danych:

    dataset = ws.datasets['existing dataset']

    print(dataset.data_type_id) # 'GenericCSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up to jan 2015'

Następnie za pomocą `update_from_dataframe` szeregować i zastąpić zawartość zestawu danych Azure:

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(frame2)

    print(dataset.data_type_id) # 'GenericCSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up to jan 2015'

Jeśli chcesz szeregować danych na inny format, określ wartość opcjonalne `data_type_id` parametru.

    from azureml import DataTypeIds

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(
        dataframe=frame2,
        data_type_id=DataTypeIds.GenericTSV,
    )

    print(dataset.data_type_id) # 'GenericTSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up to jan 2015'

Opcjonalnie możesz ustawić nowy opis, określając wartość `description` parametru.

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(
        dataframe=frame2,
        description='data up to feb 2015',
    )

    print(dataset.data_type_id) # 'GenericCSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up to feb 2015'

Opcjonalnie możesz ustawić pod nową nazwą, określając wartość `name` parametru. Od tej pory będzie można pobrać zestawu danych przy użyciu nową nazwę. Poniższy kod aktualizacji danych, nazwa i opis.

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(
        dataframe=frame2,
        name='existing dataset v2',
        description='data up to feb 2015',
    )

    print(dataset.data_type_id)                    # 'GenericCSV'
    print(dataset.name)                            # 'existing dataset v2'
    print(dataset.description)                     # 'data up to feb 2015'

    print(ws.datasets['existing dataset v2'].name) # 'existing dataset v2'
    print(ws.datasets['existing dataset'].name)    # IndexError

`data_type_id`, `name` i `description` parametry są opcjonalne i domyślnie ich poprzedniej wartości. `dataframe` Parametr zawsze jest wymagany.

Jeśli dane są już seryjny, za pomocą `update_from_raw_data` zamiast `update_from_dataframe`. Jeśli po prostu przekazać w `raw_data` zamiast `dataframe`, działa w podobny sposób.



<!-- Images -->
[security]:./media/machine-learning-python-data-access/security.png
[dataset-format]:./media/machine-learning-python-data-access/dataset-format.png
[csv-format]:./media/machine-learning-python-data-access/csv-format.png
[datasets]:./media/machine-learning-python-data-access/datasets.png
[dataset-access-code]:./media/machine-learning-python-data-access/dataset-access-code.png
[ipython-dataset]:./media/machine-learning-python-data-access/ipython-dataset.png
[experiment]:./media/machine-learning-python-data-access/experiment.png
[intermediate-dataset-access-code]:./media/machine-learning-python-data-access/intermediate-dataset-access-code.png
[ipython-intermediate-dataset]:./media/machine-learning-python-data-access/ipython-intermediate-dataset.png
[ipython-histogram]:./media/machine-learning-python-data-access/ipython-histogram.png


<!-- Module References -->
[convert-to-csv]: https://msdn.microsoft.com/library/azure/faa6ba63-383c-4086-ba58-7abf26b85814/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
 
