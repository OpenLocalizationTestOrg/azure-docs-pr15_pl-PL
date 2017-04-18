<properties
    pageTitle="Programowo przy przekwalifikowaniu modeli maszynowego uczenia | Microsoft Azure"
    description="Dowiedz się, jak programowo przy przekwalifikowaniu modelu i zaktualizować usługi sieci web używać modelu nowo przeszkolony w Azure maszynowego uczenia."
    services="machine-learning"
    documentationCenter=""
    authors="raymondlaghaeian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="raymondl;garye;v-donglo"/>


# <a name="retrain-machine-learning-models-programmatically"></a>Programowo przy przekwalifikowaniu maszynowego uczenia modeli  

W tym instruktażu dowiesz się, jak programowo przy przekwalifikowaniu Azure maszynowego uczenia usługi sieci Web przy użyciu języka C# i usługa maszynowego uczenia wsadowe.

Gdy masz retrained modelu, instruktaży następujące pokazująca, jak zaktualizować model w usłudze przewidywanych sieci web:

- Jeśli wdrożono usługi sieci web klasyczny w portalu usługi sieci Web uczenia komputera, zobacz [przekwalifikować usługi sieci web klasyczny](machine-learning-retrain-a-classic-web-service.md). 
- Jeśli wdrożono Nowa usługa sieci web, zobacz [przekwalifikować usługi sieci web przy użyciu polecenia cmdlet zarządzania nauki komputera](machine-learning-retrain-new-web-service-using-powershell.md).

Aby uzyskać omówienie procesu przekwalifikowania zobacz [przy przekwalifikowaniu modelu nauki komputera](machine-learning-retrain-machine-learning-model.md).

Jeśli chcesz zacząć z usługi sieci web nowego Menedżera zasobów Azure podstawie istniejących, zobacz [przekwalifikować usługi sieci web przewidywanych istniejącego](machine-learning-retrain-existing-resource-manager-based-web-service.md).

## <a name="create-a-training-experiment"></a>Tworzenie eksperyment szkolenia
 
W tym przykładzie użyjesz "Przykładowe 5: Szacowanie pociągu, testowanie, klasyfikacji binarne: dorosłego zestawu danych" z próbki Microsoft Azure maszynowego uczenia. 
    
Aby utworzyć doświadczenia:

1.  Zaloguj się do programu Microsoft Azure komputera nauki Studio. 
2.  W prawym dolnym rogu pulpitu nawigacyjnego kliknij przycisk **Nowy**.
3.  Samples firmy Microsoft zaznacz przykładowy 5.
4.  Aby zmienić nazwę doświadczenia w górnej części obszaru roboczego doświadczenia wybierz nazwę doświadczenia "Przykładowe 5: Szacowanie pociągu, testowanie, klasyfikacji dwójkową: dorosłego zestawu danych".
5.  Wpisz dane demograficzne Model.
6.  W dolnej części doświadczenia obszaru roboczego, kliknij polecenie **Uruchom**.
7.  Kliknij pozycję **Konfiguruj usługi sieci web** i wybierz **Retraining usługi sieci web**. 

    ![Wstępnego doświadczenia.][2]

Diagram 2: Wstępne doświadczenia.

## <a name="create-a-predictive-experiment-and-publish-as-a-web-service"></a>Tworzenie eksperyment przewidywanych i Publikuj jako usługi sieci web  

Następnie możesz utworzyć eksperyment Predicative.

1.  U dołu obszaru roboczego doświadczenia kliknij **Konto usługi sieci Web** i wybierz **Przewidywanych usługi sieci Web**. A zapisywane w modelu jako Model przeszkolony dodaje modułów dane wejściowe i wyjściowe usługi sieci web. 
2.  Kliknij przycisk **Uruchom**. 
3.  Po zakończeniu pracy doświadczenia kliknij **Wdrażanie usługi sieci Web [klasyczny]** lub **Wdrażanie usługi sieci Web [nowy]**.

## <a name="deploy-the-training-experiment-as-a-training-web-service"></a>Wdrażanie doświadczenia szkolenie jako usługi sieci web szkolenia

Aby przy przekwalifikowaniu przeszkolony modelu, należy wdrożyć doświadczenia szkoleniowego utworzony jako usługi sieci web Retraining. Ta usługa sieci web wymaga moduł *Wyjściowych usługi sieci Web* połączony z * [Pociągu modelu] [ train-model] * moduł, aby mieć możliwość utworzenia nowych modeli przeszkolony.

1. Aby powrócić do doświadczenia szkolenia, kliknij ikonę doświadczenia w okienku po lewej stronie kliknij pozycję doświadczenia o nazwie dane demograficzne modelu.  
2. W polu wyszukiwania elementy doświadczenia wyszukiwania wpisz usługi sieci web. 
3. Przeciągnij moduł *Wprowadzania usługi sieci Web* na kanwie doświadczenia i połącz dane wyjściowe w module *Oczyść brakujących danych* .  Dzięki temu, że przekwalifikowania danych jest przetwarzana w taki sam sposób jak oryginalne dane szkolenia.
4. Przeciągnij dwa moduły *usługi sieci web dane wyjściowe* na kanwie doświadczenia. Wynik modułu *Modelu pociągu* nawiązać jeden, a wynik moduł *Oceny modelu* do drugiego. Dane wyjściowe usługi sieci web dla **Modelu pociągu** daje nowy model przeszkolony. Wynik dołączone do **Oceny modelu** zwraca wynik tego modułu, czyli wyniki.
5. Kliknij przycisk **Uruchom**. 

Następnie należy wdrożyć doświadczenia szkolenie jako usługi sieci web, dająca modelu przeszkolony i wyniki obliczeń przy użyciu modelu. W tym celu do następnego zestawu czynności są zależne od tego, czy pracujesz z usługi sieci web klasyczny lub nowy usługi sieci web.  
  
**Usługa sieci web klasyczny**

U dołu obszaru roboczego doświadczenia kliknij **Konto usługi sieci Web** i wybierz **Wdrażanie usługi sieci Web [klasyczny]**. Usługa sieci Web **pulpitu nawigacyjnego** zostanie wyświetlony klucz interfejsu API i strona pomocy interfejsu API dla wsadowe. Metodę wsadowe służy do tworzenia modeli przeszkolony.

**Nowa usługa sieci web**

U dołu obszaru roboczego doświadczenia kliknij **Konto usługi sieci Web** i wybierz **Wdrażanie usługi sieci Web [nowy]**. Portal usługi sieci Web nauki komputera Azure usługi sieci Web zostanie wyświetlona na stronie usługi sieci web rozmieszczanie. Wpisz nazwę dla tej usługi sieci web i wybierz plan płatności, a następnie kliknij pozycję **Rozmieść**. Metodę wsadowe może być używany do tworzenia przeszkolony modeli

W obu przypadkach po doświadczenia w pracy, wyniku przepływu pracy powinna wyglądać następująco:

![Wynikowa przepływu pracy, po uruchomieniu.][4]

Diagram 3: Wynikowe przepływu pracy po uruchomieniu.

## <a name="retrain-the-model-with-new-data-using-bes"></a>Ponowne próbkowanie modelu przy użyciu nowych danych przy użyciu BES

W tym przykładzie używasz C# tworzenia przekwalifikowania aplikacji. Za pomocą kodu przykładowego Python lub R, aby wykonać to zadanie.

Aby zadzwonić do interfejsów API przeszkolenie:

1. Tworzenie aplikacji konsoli C# w programie Visual Studio (Nowy -> projektu -> Windows pulpitu -> aplikacji konsoli).
2.  Zaloguj się do portalu usługi sieci Web uczenia.
3.  Jeśli pracujesz z usługi sieci web klasyczny, kliknij pozycję **Klasyczny usług sieci Web**.
    1.  Kliknij pozycję Usługa sieci web, którą pracujesz.
    2.  Kliknij pozycję domyślny punkt końcowy.
    3.  Kliknij pozycję **korzystać**.
    4.  U dołu strony **Consume** w sekcji **Przykładowy kod** kliknij **partii**.
    5.  Przejdź do kroku 5 tej procedury.
4.  Jeśli pracujesz z nowej usługi sieci web, kliknij pozycję **Usług sieci Web**.
    1.  Kliknij pozycję Usługa sieci web, którą pracujesz.
    2.  Kliknij pozycję **korzystać**.
    3.  U dołu strony Consume w sekcji **Przykładowy kod** kliknij **partii**.
5.  Skopiuj przykładowe C# kod wsadowe i wklej je do pliku Plik Program.cs zagwarantować, że obszar nazw pozostanie niezmieniona.

Dodawanie pakietu Nuget Microsoft.AspNet.WebApi.Client zgodnie z ustawieniami w komentarzach. Aby dodać odwołanie do Microsoft.WindowsAzure.Storage.dll, może być najpierw należy zainstalować Biblioteka klienta usługi Microsoft Azure miejsca do magazynowania. Aby uzyskać więcej informacji zobacz [Usługi Magazyn systemu Windows](https://www.nuget.org/packages/WindowsAzure.Storage).

### <a name="update-the-apikey-declaration"></a>Aktualizowanie deklaracji apikey

Znajdź deklaracji **apikey** .

    const string apiKey = "abc123"; // Replace this with the API key for the web service

W sekcji **informacje podstawowe zużycie** na stronie **Consume** Znajdź klucz podstawowy i skopiować go do deklaracji **apikey** .

### <a name="update-the-azure-storage-information"></a>Aktualizowanie informacji o magazyn Azure

Przykładowy kod BES przekazywanie pliku z dysku lokalnego (na przykład "C:\temp\CensusIpnput.csv") do magazynu Azure, przetwarza je i zapisuje wyniki ponownie Magazyn Azure.  

Aby wykonać to zadanie, należy pobrać miejsca do magazynowania nazwa, klucz i kontener informacje o koncie dla Twojego konta miejsca do magazynowania z klasyczny portal Azure i Aktualizuj odpowiadających sobie wartości w kodzie. 

1. Zaloguj się do portalu Azure klasyczny.
1. W kolumnie nawigacji po lewej stronie kliknij pozycję **miejsca do magazynowania**.
1. Z listy kont miejsca do magazynowania wybierz jeden do przechowywania retrained modelu.
1. U dołu strony kliknij pozycję **Zarządzaj klawiszy dostępu**.
1. Kopiowanie i zapisać **Klucza podstawowego programu Access** i zamknij okno dialogowe. 
1. W górnej części strony kliknij pozycję **kontenerów**.
1. Wybierz istniejące kontener lub Utwórz nowy, a następnie zapisz nazwę.

Znajdź deklaracji *StorageAccountName*, *StorageAccountKey*i *StorageContainerName* i zaktualizować wartości zapisane z portalu Azure.

    const string StorageAccountName = "mystorageacct"; // Replace this with your Azure Storage Account name
    const string StorageAccountKey = "a_storage_account_key"; // Replace this with your Azure Storage Key
    const string StorageContainerName = "mycontainer"; // Replace this with your Azure Storage Container name
            
Możesz również należy upewnić się, że wprowadzania plik znajduje się w lokalizacji, zadanej w kodzie. 

### <a name="specify-the-output-location"></a>Określ lokalizację danych wyjściowych

Określając lokalizację danych wyjściowych w ładunku żądania rozszerzenia pliku określonego w *RelativeLocation* musi być określony jako ilearner. 

Zobacz przykład poniżej:

    Outputs = new Dictionary<string, AzureBlobDataReference>() {
        {
            "output1",
            new AzureBlobDataReference()
            {
                ConnectionString = storageConnectionString,
                RelativeLocation = string.Format("{0}/output1results.ilearner", StorageContainerName) /*Replace this with the location you would like to use for your output file, and valid file extension (usually .csv for scoring results, or .ilearner for trained models)*/
            }
        },

>[AZURE.NOTE] Nazwy lokalizacji dane wyjściowe mogą być inne niż w tym instruktażu według kolejności, w której dodane modułów danych wyjściowych usługi sieci web. Ponieważ skonfigurowaniu tego doświadczenia szkolenie z dwóch wyjściowe wyniki obejmują informacje o lokalizacji miejsca do magazynowania dla obu z nich.  

![Przeszkolenie wyjścia][6]

Diagram 4: Przeszkolenie dane wyjściowe.

## <a name="evaluate-the-retraining-results"></a>Ocena przekwalifikowania wyników
 
Po uruchomieniu aplikacji wynik zawiera token skojarzeń zabezpieczeń i adres URL to konieczne uzyskać dostęp do wyników oceny.

Wyniki retrained modelu widać, łączenie z wynik dla *output2* *BaseLocation*, *RelativeLocation*i *SasBlobToken* (jak pokazano w powyższej przekwalifikowania obraz dane wyjściowe) i wklejając pełnego adresu URL na pasku adresu przeglądarki.  

Sprawdzenie wyników w celu określenia, czy nowo przeszkolony modelu wystarczająco również wykonuje Aby zamienić istniejący.

Skopiuj *BaseLocation*, *RelativeLocation*i *SasBlobToken* z wynik, będzie używać ich w procesie przekwalifikowania.

## <a name="next-steps"></a>Następne kroki

Jeśli wdrożono usługę przewidywanych sieci web, klikając pozycję **Wdrażanie usługi sieci Web [klasyczny]**, zobacz [przekwalifikować usługi sieci web klasyczny](machine-learning-retrain-a-classic-web-service.md).

Jeśli wdrożono usługę przewidywanych sieci web, klikając pozycję **Wdrażanie usługi sieci Web [nowy]**, zobacz [przekwalifikować usługi sieci web przy użyciu polecenia cmdlet zarządzania nauki komputera](machine-learning-retrain-new-web-service-using-powershell.md).

<!-- Retrain a New web service using the Machine Learning Management REST API -->


[1]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE01.png
[2]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE02.png
[3]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE03.png
[4]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE04.png
[5]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE05.png
[6]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE06.png
[7]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE07.png


<!-- Module References -->
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/