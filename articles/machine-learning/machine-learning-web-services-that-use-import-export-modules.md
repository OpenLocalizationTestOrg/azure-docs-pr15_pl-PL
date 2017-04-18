<properties
    pageTitle="Wdrażanie usług sieci web Azure ML za pomocą modułów danych importu i eksportu danych | Microsoft Azure"
    description="Dowiedz się, jak używać modułów danych importowanie i eksportowanie danych do wysyłania i odbierania danych z usługi sieci web."
    services="machine-learning"
    documentationCenter=""
    authors="vDonGlover"
    manager="raymondlaghaeian"
    editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/12/2016"
    ms.author="v-donglo"/>



# <a name="deploying-azure-ml-web-services-that-use-data-import-and-data-export-modules"></a>Wdrażanie usług sieci web Azure ML za pomocą modułów danych importu i eksportu danych 

Po utworzeniu eksperyment przewidywanych zwykle dodasz sieci web usługi wejściowe i wyjściowe. Po wdrożeniu doświadczenia konsumentów można wysyłać i odbierać dane z usługi sieci web za pośrednictwem wejścia i wyjścia. W niektórych aplikacjach dla klientów indywidualnych danych mogą być dostępne ze strumieniowego źródła danych lub już znajdują się w źródle danych zewnętrznych, takich jak magazyn obiektów Blob platformy Azure. W takim przypadku nie potrzebują odczytywanie i zapisywanie danych przy użyciu sieci web usługi wejść i wyjść. Mogą, zamiast tego więcej danych ze źródła danych za pomocą modułu importowanie danych za pomocą usługi wykonanie partii (BES) i zapisać wyniki wyników do lokalizacji różnych danych przy użyciu modułu eksportowanie danych.

Importowanie danych i eksportowania danych modułów, można odczytywać i zapisywać do liczby dane, które zapewniają lokalizacjach na przykład adres URL sieci Web za pośrednictwem protokołu HTTP, kwerendy gałęzi, baza danych Azure SQL, Magazyn tabel platformy Azure, magazyn obiektów Blob platformy Azure, strumieniowego źródła danych lub w wersji lokalnej bazy danych SQL.

Używane w tym temacie "Przykładowy 5: Szacowanie pociągu, testowanie, klasyfikacji binarne: dorosłego zestawu danych" przykładowe i przyjęto założenie, zestawu danych został już załadowany do tabeli programu Azure SQL o nazwie censusdata.

## <a name="create-the-training-experiment"></a>Tworzenie doświadczenia szkolenia 
 
Po otwarciu "Przykładowe 5: Szacowanie pociągu, testowanie, klasyfikacji binarne: dorosłego zestawu danych" przykładowy używa zestawu danych dorosłego klasyfikacji binarne dochód spisu próbki. I doświadczenia w obszarze roboczym będzie wyglądać podobnie do poniższej ilustracji.

![Konfiguracja początkowa doświadczenia.](./media/machine-learning-web-services-that-use-import-export-modules/initial-look-of-experiment.png)
  

Aby odczytać dane z tabeli Azure SQL:

1.  Usuwanie moduł zestawu danych.
2.  W polu wyszukiwania składniki wpisz importu.
3.  Z listy wyników należy dodać moduł *Importowanie danych* do obszaru roboczego doświadczenia.
4.  Połącz dane wyjściowe modułu *Importowanie danych* dane wejściowe modułu *Oczyść brakujących danych* .
5.  W okienku właściwości wybierz **Bazy danych SQL Azure** na liście rozwijanej **Źródło danych** .
6.  W polu **Nazwa serwera bazy danych** **Nazwa bazy danych**, **nazwę użytkownika**i **hasło** pola, wprowadź odpowiednie informacje o bazie danych.
7.  W polu kwerendy bazy danych wprowadź poniższe zapytanie.

        select [age],
           [workclass],
           [fnlwgt],
           [education],
           [education-num],
           [marital-status],
           [occupation],
           [relationship],
           [race],
           [sex],
           [capital-gain],
           [capital-loss],
           [hours-per-week],
           [native-country],
           [income]
        from dbo.censusdata;

8.  W dolnej części doświadczenia obszaru roboczego, kliknij polecenie **Uruchom**.

## <a name="create-the-predictive-experiment"></a>Tworzenie przewidywanych doświadczenia

Następnie możesz skonfigurować przewidywanych doświadczenia, z którego będą wdrażanie usługi sieci web.

1.  U dołu obszaru roboczego doświadczenia kliknij **Konto usługi sieci Web** i wybierz **Przewidywanych usługi sieci Web [zalecane]**.
2.  Usuwanie *Danych wejściowych usługi sieci Web* i *moduły wyjściowych usługi sieci Web* z przewidywanych doświadczenia. 
3.  W polu wyszukiwania składniki wpisz eksportu.
4.  Z listy wyników należy dodać moduł *Eksportowanie danych* do obszaru roboczego doświadczenia.
5.  Połącz dane wyjściowe modułu *Wynik modelu* danych wejściowych modułu *Eksportowanie danych* . 
6.  W okienku właściwości wybierz **Bazy danych SQL Azure** na liście rozwijanej miejsca docelowego danych.
7.  W polach **Nazwa serwera bazy danych**, **nazwę bazy danych**, **nazwę konta użytkownika serwera**i **hasło do konta użytkownika serwera** wprowadź odpowiednie informacje o bazie danych.
8.  W polu **listy kolumn do zapisania oddzielone przecinkami** wpisz etykiety uzyskał.
9.  W **polu Nazwa tabeli danych**wpisz dbo. ScoredLabels. Jeśli tabela nie istnieje, jest on tworzony po uruchomieniu doświadczenia lub nosi nazwę usługi sieci web.
10. W polu **listy kolumn datatable oddzielone przecinkami** wpisz ScoredLabels.

Podczas pisania aplikacji, która wymaga usługi sieci web ostateczne można określić innej kwerendy wprowadzania lub tabelę docelową w czasie wykonywania. Aby skonfigurować te dane wejściowe i wyjściowe, funkcja parametrów usługi sieci Web umożliwia ustawianie właściwości *źródła danych* modułu *Importowanie danych* i właściwość destination danych trybu *Eksportowanie danych* .  Aby uzyskać więcej informacji o parametrach usługi sieci Web zobacz [wpisu AzureML parametry usługi sieci Web](https://blogs.technet.microsoft.com/machinelearning/2014/11/25/azureml-web-service-parameters/) na analizy Cortana i komputera nauki blogu.

Aby skonfigurować parametry usługi sieci Web dla zapytania importowanie i tabeli docelowej:

1.  W okienku właściwości modułu *Importowanie danych* kliknij ikonę u góry prawej strony pola **kwerendy bazy danych** i wybierz polecenie **Ustaw jako parametr usługi sieci web**.
2.  W okienku właściwości modułu *Eksportowanie danych* , kliknij ikonę u góry prawej strony pola **Nazwa tabeli danych** i wybierz polecenie **Ustaw jako parametr usługi sieci web**.
3.  W dolnej części okienka właściwości *Eksportowanie danych* modułu w sekcji **Parametry usługi sieci Web** kliknij zapytanie bazy danych i zmień jej nazwę kwerendy.
4.  Kliknij **nazwę tabeli danych** i Zmień nazwę **tabeli**.

Po wykonaniu tych czynności, z doświadczenia powinno wyglądać podobnie do poniższej ilustracji.
 
![Ostateczne wygląd doświadczenia.](./media/machine-learning-web-services-that-use-import-export-modules/experiment-with-import-data-added.png)

Teraz można wdrażać doświadczenia jako usługi sieci web.

## <a name="deploy-the-web-service"></a>Wdrażanie usługi sieci web 
Należy wdrożyć Classic lub Nowa usługa sieci web.

### <a name="deploy-a-classic-web-service"></a>Wdrażanie usługi sieci Web klasyczny

Wdrażanie jako klasyczny usługi sieci Web i utworzyć aplikację, aby być:

1.  U dołu obszaru roboczego doświadczenia kliknij pozycję Uruchom.
2.  Po zakończeniu uruchom kliknij **Wdrażanie usługi sieci Web** i wybierz **Wdrażanie usługi sieci Web [klasyczny]**.
3.  Na pulpicie nawigacyjnym usługi sieci web Znajdź swój klucz interfejsu API. Kopiowanie i zapisanie go w celu późniejszego użycia.
4.  W tabeli **Domyślny punkt końcowy** kliknij łącze **Wsadowe** , aby otworzyć stronę pomocy interfejsu API.
5.  W programie Visual Studio tworzenie aplikacji konsoli C#.
6.  Na stronie Pomoc interfejsu API Znajdź sekcję **Przykładowy kod** w dolnej części strony.
7.  Skopiuj i Wklej kod przykładowy C# do pliku Plik Program.cs i usunięcie wszystkich odwołań do magazyn obiektów blob.
8.  Zaktualizuj wartość zmiennej *apiKey* klucz interfejsu API wcześniej zapisany.
9.  Znajdź zgłoszenie żądania i zaktualizować wartości parametrów usługi sieci Web, które są przekazywane do modułów *Danych importowanie* i *Eksportowanie danych* . W tym przypadku będzie za pomocą kwerendy oryginalnego, ale Zdefiniuj nową nazwę tabeli.

        var request = new BatchExecutionRequest() 
        {   
            GlobalParameters = new Dictionary<string, string>() {
            { "Query", @"select [age], [workclass], [fnlwgt], [education], [education-num], [marital-status], [occupation], [relationship], [race], [sex], [capital-gain], [capital-loss], [hours-per-week], [native-country], [income] from dbo.censusdata" },
            { "Table", "dbo.ScoredTable2" },
            }
        };

10. Uruchom aplikację. 

Po zakończeniu uruchom nowa tabela jest dodawany do bazy danych zawierającej wyniki wyników.

### <a name="deploy-a-new-web-service"></a>Wdrażanie nowej usługi sieci Web

Wdrażanie jako nowa usługa sieci Web i utworzyć aplikację, aby być:

1.  W dolnej części doświadczenia obszaru roboczego, kliknij polecenie **Uruchom**.
2.  Po zakończeniu uruchom kliknij **Wdrażanie usługi sieci Web** i wybierz **Wdrażanie usługi sieci Web [nowy]**.
3.  Na stronie wdrażanie doświadczenia wprowadź nazwę dla danej usługi sieci web i wybierz plan cennik, a następnie kliknij pozycję **Rozmieść**.
4.  Na stronie **Szybki Start** kliknij pozycję **Consume**.
5.  W sekcji **Przykładowy kod** kliknij przycisk **partię**.
6.  W programie Visual Studio tworzenie aplikacji konsoli C#.
7.  Skopiuj i Wklej kod przykładowy C# do pliku Plik Program.cs.
8.  Zaktualizuj wartość zmiennej *apiKey* **Klucza podstawowego** znajdujący się w sekcji **informacje o podstawowych zużycie** .
9.  Znajdź deklaracji *scoreRequest* i zaktualizować wartości parametrów usługi sieci Web, które są przekazywane do modułów *Danych importowanie* i *Eksportowanie danych* . W tym przypadku będzie za pomocą kwerendy oryginalnego, ale Zdefiniuj nową nazwę tabeli.

        var scoreRequest = new
        {
            Inputs = new Dictionary<string, StringTable>()
            {
            },
            GlobalParameters = new Dictionary<string, string>() {
                 { "Query", @"select [age], [workclass], [fnlwgt], [education], [education-num], [marital-status], [occupation], [relationship], [race], [sex], [capital-gain], [capital-loss], [hours-per-week], [native-country], [income] from dbo.censusdata" },
                { "Table", "dbo.ScoredTable3" },
            }
        };

10. Uruchom aplikację. 
 

