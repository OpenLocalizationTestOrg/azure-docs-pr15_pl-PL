<properties
    pageTitle="Przy przekwalifikowaniu istniejącej usługi sieci web przewidywanych | Microsoft Azure"
    description="Dowiedz się, jak przy przekwalifikowaniu modelu i usługi sieci web używać modelu nowo przeszkolony w Azure maszynowego uczenia aktualizacji."
    services="machine-learning"
    documentationCenter=""
    authors="vDonGlover"
    manager="raymondl"
    editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/06/2016"
    ms.author="v-donglo"/>


# <a name="retrain-an-existing-predictive-web-service"></a>Istniejąca usługa sieci web przewidywanych przekwalifikować

W tym dokumencie opisano przekwalifikowania proces następującym scenariuszu:

* Masz doświadczenia szkolenia i eksperyment przewidywanych, który został wdrożony jako usługi sieci web operationalized.
* Masz nowe dane odpowiednią Usługa sieci web przewidywanych umożliwiających wykonywanie jego wyników.

Począwszy od istniejącego usługi sieci web i doświadczeń usługi, musisz wykonaj następujące czynności:

1. Aktualizowanie modelu.
    1. Modyfikowanie usługi doświadczenia szkolenia, aby umożliwić w odniesieniu do nakładów usługi sieci web i.
    2. Wdrażanie doświadczenia szkolenie jako przekwalifikowania usługi sieci web.
    3. Ponowne próbkowanie modelu za pomocą doświadczenia szkolenie partii wykonanie usługi (BES).
2. Użyj polecenia cmdlet Azure maszynowego uczenia programu PowerShell, aby zaktualizować przewidywanych doświadczenia.
    1.  Zaloguj się do swojego konta Azure Menedżera zasobów.
    2.  Uzyskaj definicji usługi sieci web.
    3.  Eksportowanie definicji usługi sieci web jako JSON.
    4.  Zaktualizuj odwołanie do obiektów blob ilearner w formacie JSON.
    5.  Importowanie JSON do definicji usługi sieci web.
    6.  Aktualizowanie usługi sieci web przy użyciu nowych definicji usługi sieci web.

## <a name="deploy-the-training-experiment"></a>Wdrażanie doświadczenia szkolenia

Aby wdrożyć doświadczenia szkolenie jako przekwalifikowania usługi sieci web, możesz dodać wejść usługi sieci web i wyjść do modelu. Łącząc moduł *Wyjściowych usługi sieci Web* do doświadczenia * [Modelu pociągu] [ train-model] * moduł, możesz włączyć doświadczenia szkolenie do utworzenia nowego modelu przeszkolony, który możesz użyć w swojej przewidywanych doświadczenia. Jeśli masz moduł *Oceny modelu* można także dołączyć wyjściowych usługi sieci web w celu uzyskania wyników oceny jako wynik.

Aby zaktualizować usługi doświadczenia szkolenie:

1. Podłącz moduł *Wprowadzania danych usługi sieci Web* do do wprowadzania danych (na przykład moduł *Oczyść brakujących danych* ). Zazwyczaj chcesz mieć pewność, że wprowadzania danych jest przetwarzana w taki sam sposób jak oryginalne dane szkolenia.
2. Połącz moduł *Wyjściowych usługi sieci Web* do wyników modułu *Modelu pociągu* .
3. Jeśli masz moduł *Oceny modelu* i chcesz wyjściowy wyników oceny, nawiązać wynik modułu *Oceny modelu* moduł *Wyjściowych usługi sieci Web* .

Uruchamianie usługi doświadczenia.

Następnie należy wdrożyć doświadczenia szkolenie jako usługi sieci web, dająca modelu przeszkolony i wyniki obliczeń przy użyciu modelu.  

U dołu obszaru roboczego doświadczenia kliknij pozycję **Konto usługi sieci Web**, a następnie wybierz **Wdrażanie usługi sieci Web [nowy]**. Portal usługi sieci Web uczenia maszynowego Azure po otwarciu strony **Wdrażanie usługi sieci Web** . Wpisz nazwę dla tej usługi sieci web, wybierz plan płatności, a następnie kliknij **Rozmieszczanie**. Metoda wsadowe służy tylko do tworzenia przeszkolony modeli.


## <a name="retrain-the-model-with-new-data-by-using-bes"></a>Ponowne próbkowanie modelu przy użyciu nowych danych przy użyciu BES

W tym przykładzie użyto programu C# tworzenia przekwalifikowania aplikacji. Za pomocą kodu przykładowe Python lub R, aby wykonać to zadanie.

Aby nawiązać połączenie za pośrednictwem interfejsów API przekwalifikowania:

1. Tworzenie aplikacji konsoli C# w programie Visual Studio (**Nowy** > **projektu** > **Komputerze z systemem Windows** > **Aplikacji konsoli**).
2.  Zaloguj się do portalu usługi sieci Web uczenia komputera.
3.  Kliknij pozycję Usługa sieci web, którą pracujesz.
2.  Kliknij pozycję **korzystać**.
3.  U dołu strony **Consume** w sekcji **Przykładowy kod** kliknij **partii**.
5.  Skopiuj przykładowe C# kod wsadowe i wklej je do pliku Plik Program.cs. Upewnij się, że obszar nazw pozostanie niezmieniona.

Dodawanie pakietu NuGet Microsoft.AspNet.WebApi.Client, zgodnie z ustawieniami w komentarzach. Aby dodać odwołanie do Microsoft.WindowsAzure.Storage.dll, może być najpierw należy zainstalować [Biblioteka klienta usługi Magazyn Azure](https://www.nuget.org/packages/WindowsAzure.Storage).

Następujące zrzut ekranu przedstawia stronę **Consume** w portalu usługi sieci Web uczenia maszynowego Azure.

![Używanie strony][1]

### <a name="update-the-apikey-declaration"></a>Aktualizowanie deklaracji apikey

Znajdź deklaracji **apikey** :

    const string apiKey = "abc123"; // Replace this with the API key for the web service

W sekcji **informacje podstawowe zużycie** na stronie **Consume** Znajdź klucz podstawowy i skopiować go do deklaracji **apikey** .

### <a name="update-the-azure-storage-information"></a>Aktualizowanie informacji o magazyn Azure

Przykładowy kod BES przekazywanie pliku z dysku lokalnego (na przykład "C:\temp\CensusIpnput.csv") do magazynu Azure, przetwarza je i zapisuje wyniki ponownie Magazyn Azure.  

Aby zaktualizować informacje magazyn Azure, należy pobrać nazwę konta magazynu, klucz i informacje dotyczące kontenera dla Twojego konta miejsca do magazynowania z portalu klasyczny Azure, a następnie aktualizacji correspondi po uruchomieniu programu doświadczenia uzyskanego przepływu pracy powinny być podobny do następującego:

![Wynikowa przepływu pracy po uruchomieniu][4]wartości ng w kodzie.

1. Zaloguj się do portalu klasyczny Azure.
1. W kolumnie nawigacji po lewej stronie kliknij pozycję **miejsca do magazynowania**.
1. Z listy kont miejsca do magazynowania wybierz jeden do przechowywania retrained modelu.
1. U dołu strony kliknij pozycję **Zarządzaj klawiszy dostępu**.
1. Kopiowanie i zapisać **Klucza podstawowego programu Access** i zamknij okno dialogowe.
1. W górnej części strony kliknij pozycję **kontenerów**.
1. Zaznacz istniejącego kontenera, lub Utwórz nowy i Zapisz nazwę.

Odszukaj *StorageAccountName*, *StorageAccountKey*i *StorageContainerName* deklaracji i zaktualizować wartości zapisane z portalu klasyczny.

    const string StorageAccountName = "mystorageacct"; // Replace this with your Azure storage account name
    const string StorageAccountKey = "a_storage_account_key"; // Replace this with your Azure Storage key
    const string StorageContainerName = "mycontainer"; // Replace this with your Azure Storage container name

Również należy zagwarantować, że plik wejściowy jest dostępna w lokalizacji, określonej przez użytkownika w kodzie.

### <a name="specify-the-output-location"></a>Określ lokalizację danych wyjściowych

Po określeniu lokalizację danych wyjściowych w ładunku żądania rozszerzenia pliku, którą określono w *RelativeLocation* musi być określony jako `ilearner`. Zobacz przykład poniżej:

    Outputs = new Dictionary<string, AzureBlobDataReference>() {
        {
            "output1",
            new AzureBlobDataReference()
            {
                ConnectionString = storageConnectionString,
                RelativeLocation = string.Format("{0}/output1results.ilearner", StorageContainerName) /*Replace this with the location you want to use for your output file and a valid file extension (usually .csv for scoring results or .ilearner for trained models)*/
            }
        },

Oto przykład przekwalifikowania wynik: ![przeszkolenie wyjścia][6]

## <a name="evaluate-the-retraining-results"></a>Ocena przekwalifikowania wyników

Po uruchomieniu aplikacji wynik zawiera adres URL i token podpisów dostępu udostępnione są potrzebne uzyskać dostęp do wyników oceny.

Wyniki retrained modelu widać, łącząc *BaseLocation*, *RelativeLocation*i *SasBlobToken* z wynik dla *output2* (jak pokazano w powyższej przekwalifikowania obraz dane wyjściowe) i wklejanie pełnego adresu URL w pasku adresu przeglądarki.  

Sprawdzenie wyników w celu określenia, czy nowo przeszkolony modelu wystarczająco również wykonuje Aby zamienić istniejący.

Skopiuj *BaseLocation*, *RelativeLocation*i *SasBlobToken* z wynik.

## <a name="retrain-the-web-service"></a>Przekwalifikować usługi sieci web

Po przekwalifikować Nowa usługa sieci web, możesz zaktualizować definicji usługi sieci web przewidywanych do odwołania do nowego modelu przeszkolony. Definicji usługi sieci web jest reprezentacji wewnętrznej przeszkolony modelu usługi sieci web i nie można bezpośrednio modyfikować. Upewnij się, że są pobierane definicji usługi sieci web dla swojej przewidywanych doświadczenia i nie doświadczenia szkolenie.

## <a name="sign-in-to-azure-resource-manager"></a>Zaloguj się do Azure Menedżera zasobów

Musisz najpierw zalogować się na konto Azure z w środowisku programu PowerShell przy użyciu polecenia cmdlet [AzureRmAccount Dodaj](https://msdn.microsoft.com/library/mt619267.aspx) .

## <a name="get-the-web-service-definition-object"></a>Pobierz obiekt definicji usługi sieci Web

Następnie należy pobrać obiektu definicji usługi sieci Web, dzwoniąc do polecenia cmdlet [Get-AzureRmMlWebService](https://msdn.microsoft.com/library/mt619267.aspx) .

    $wsd = Get-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'

Aby określić nazwę grupy zasobów istniejące usługi sieci web, uruchom polecenie cmdlet Get-AzureRmMlWebService bez parametrów do wyświetlenia w ramach subskrypcji usługi sieci web. Znajdź usługi sieci web, a następnie spójrz na jej identyfikator sieci web usługi. Nazwa grupy zasobów jest czwartym elementu w polu Identyfikator zaraz po elemencie *resourceGroups* . W poniższym przykładzie nazwa grupy zasobów jest domyślne-MachineLearning-SouthCentralUS.

    Properties : Microsoft.Azure.Management.MachineLearning.WebServices.Models.WebServicePropertiesForGraph
    Id : /subscriptions/<subscription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237
    Name : RetrainSamplePre.2016.8.17.0.3.51.237
    Location : South Central US
    Type : Microsoft.MachineLearning/webServices
    Tags : {}

Możesz też ustalenie Nazwa grupy zasobów istniejące usługi sieci web, zaloguj się do portalu usługi sieci Web Azure maszynowego uczenia. Wybierz usługę sieci web. Nazwa grupy zasobów jest piątym element adresu URL usługi sieci web zaraz po elemencie *resourceGroups* . W poniższym przykładzie nazwa grupy zasobów jest domyślne-MachineLearning-SouthCentralUS.

    https://services.azureml.net/subscriptions/<subcription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237


## <a name="export-the-web-service-definition-object-as-json"></a>Eksportowanie obiektu definicji usługi sieci Web jako JSON

Modyfikowania definicji modelu przeszkolony używać nowo przeszkolony modelu, należy najpierw użyć polecenia cmdlet [AzureRmMlWebService eksportu](https://msdn.microsoft.com/library/azure/mt767935.aspx) aby go wyeksportować do pliku formatu JSON.

    Export-AzureRmMlWebService -WebService $wsd -OutputFile "C:\temp\mlservice_export.json"

## <a name="update-the-reference-to-the-ilearner-blob"></a>Aktualizowanie odwołanie do obiektów blob ilearner

W składniki majątku Znajdź [model przeszkolony], zaktualizować wartość *identyfikatora uri* w węźle *locationInfo* identyfikator URI obiektów blob ilearner. Identyfikator URI jest generowany przez połączenie *BaseLocation* i *RelativeLocation* z wyników BES przeszkolenie połączenia.

     "asset3": {
        "name": "Retrain Sample [trained model]",
        "type": "Resource",
        "locationInfo": {
          "uri": "https://mltestaccount.blob.core.windows.net/azuremlassetscontainer/baca7bca650f46218633552c0bcbba0e.ilearner"
        },
        "outputPorts": {
          "Results dataset": {
            "type": "Dataset"
          }
        }
      },

## <a name="import-the-json-into-a-web-service-definition-object"></a>Importowanie JSON do obiektu definicji usługi sieci Web

Aby przekonwertować zmodyfikowany plik JSON ponownie obiekt definicji usługi sieci Web, który umożliwia aktualizowanie predicative doświadczenia należy użyć polecenia cmdlet [AzureRmMlWebService importowanie](https://msdn.microsoft.com/library/azure/mt767925.aspx) .

    $wsd = Import-AzureRmMlWebService -InputFile "C:\temp\mlservice_export.json"


## <a name="update-the-web-service"></a>Aktualizowanie usługi sieci web

Na koniec użyć polecenia cmdlet [AzureRmMlWebService aktualizacji](https://msdn.microsoft.com/library/azure/mt767922.aspx) , aby zaktualizować przewidywanych doświadczenia.

    Update-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -

[1]: ./media/machine-learning-retrain-existing-arm-web-service/machine-learning-retrain-models-consume-page.png
[4]: ./media/machine-learning-retrain-existing-arm-web-service/machine-learning-retrain-models-programmatically-IMAGE04.png
[6]: ./media/machine-learning-retrain-existing-arm-web-service/machine-learning-retrain-models-programmatically-IMAGE06.png

<!-- Module References -->
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
