<properties 
   pageTitle="Wyświetlanie dzienników diagnostycznych Azure danych Lake analizy | Microsoft Azure" 
   description="Zrozumienie sposobu instalacji i uzyskać dostęp do dzienników diagnostycznych analiz Lake danych Azure " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="Blackmist" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="08/11/2016"
   ms.author="larryfr"/>

# <a name="accessing-diagnostic-logs-for-azure-data-lake-analytics"></a>Uzyskiwanie dostępu do dzienników diagnostycznych analiz Lake danych Azure

Informacje na temat sposobu włączyć rejestrowanie diagnostyczne dla Twojego konta analizy Lake danych oraz wyświetlanie dzienników zbierane dla Twojego konta.

Organizacje można włączyć rejestrowanie diagnostyczne dla swojego konta Azure danych Lake analizy do zbierania danych programu access inspekcji ścieżek. Te dzienniki zawierają informacje, takie jak:

* Lista użytkowników, których dostęp do danych.
* Jak często uzyskać dostępu do danych.
* Ile kosztuje dane są przechowywane w oknie konta.

## <a name="prerequisites"></a>Wymagania wstępne

- **Azure subskrypcji**. Zobacz [Azure pobrać bezpłatną wersję próbną](https://azure.microsoft.com/pricing/free-trial/).
- **Włączanie Azure subskrypcji** dla publicznej Podgląd analizy Lake danych. Zobacz [instrukcje](data-lake-analytics-get-started-portal.md#signup).
- **Konto azure danych Lake analizy**. Postępuj zgodnie z instrukcjami na [Wprowadzenie do analiz Lake danych Azure za pomocą portalu Azure](data-lake-analytics-get-started-portal.md).

## <a name="enable-logging"></a>Włączanie rejestrowania

1. Logowanie się do nowego [Azure portal](https://portal.azure.com).

2. Otwórz konto analizy Lake danych i karta konta usługi analizy Lake danych, kliknij pozycję **Ustawienia**, a następnie kliknij **Ustawień diagnostycznych**.

3. W **diagnostyczne** karta wprowadzić następujące zmiany, aby skonfigurować rejestrowanie diagnostyczne.

    ![Włącz rejestrowanie diagnostyczne] (./media/data-lake-analytics-diagnostic-logs/enable-diagnostic-logs.png "Włączanie dzienników diagnostycznych")

    * Ustaw **Stan** **na** Aby włączyć rejestrowanie diagnostyczne.
    * Istnieje możliwość-proces magazynu danych na dwa różne sposoby.
        * Zaznacz opcję **Eksportuj do koncentratora zdarzeń** do transmisji danych dziennika do koncentratora zdarzenia Azure. Użyj tej opcji, jeśli masz potok przetwarzania podrzędne do analizowania przychodzącego dzienników w czasie rzeczywistym. Jeśli wybierzesz tę opcję, musisz podać szczegóły koncentratora zdarzenia Azure, którego chcesz użyć.
        * Zaznacz opcję **Eksportowanie konta miejsca do magazynowania** do przechowywania dzienników z klientem magazyn Azure. Użyj tej opcji, jeśli chcesz zarchiwizować dane. Jeśli wybierzesz tę opcję, należy podać konto magazyn Azure, aby zapisać dzienniki do.
    * Określ, czy chcesz uzyskać dzienniki inspekcji lub dzienników żądanie lub oba.
    * Określ liczbę dni, dla których muszą być przechowywane dane.
    * Kliknij przycisk **Zapisz**.

Po włączeniu ustawień diagnostycznych możesz obejrzeć dzienniki na karcie **Dzienniki diagnostyczne** .

## <a name="view-logs"></a>Wyświetlanie dzienników

Istnieją dwa sposoby wyświetlania danych dziennika dla Twojego konta analizy Lake danych.

* W obszarze Ustawienia konta analizy Lake danych
* Z konta magazynu platformy Azure przechowywania danych

### <a name="using-the-data-lake-analytics-settings-view"></a>Przy użyciu widoku ustawienia analizy Lake danych

1. Karta **Ustawienia** konta usługi analizy Lake danych kliknij **Dzienniki diagnostyczne**.

    ![Rejestrowanie diagnostyczne widoku] (./media/data-lake-analytics-diagnostic-logs/view-diagnostic-logs.png "Wyświetlanie dzienników diagnostycznych") 

2. W karta **Dzienniki diagnostyczne** powinna być widoczna dzienniki posortowane według **Dzienniki inspekcji** i **Dzienniki żądanie**.
    * Dzienniki żądanie Przechwytywanie każdego żądania interfejsu API na rachunku analizy Lake danych.
    * Dzienniki inspekcji są podobne do Dzienniki żądań, ale zapewnia bardziej szczegółowego podziału operacji wykonywanych na koncie analizy Lake danych. Na przykład połączenie interfejsu API przekazywanych w dziennikach żądania może spowodować wiele operacji "Dołączanie" w dzienników inspekcji.

3. Kliknij łącze **Pobierz** dla pozycji dziennika pobrać pliki dziennika.

### <a name="from-the-azure-storage-account-that-contains-log-data"></a>Z konta magazynu platformy Azure, która zawiera dane dziennika

1. Otwórz karta konta magazynu platformy Azure skojarzone z analizy Lake danych do logowania, a następnie kliknij obiektów blob. Karta **obiektów Blob usługi** lista dwoma kontenerami.

    ![Rejestrowanie diagnostyczne widoku] (./media/data-lake-analytics-diagnostic-logs/view-diagnostic-logs-storage-account.png "Wyświetlanie dzienników diagnostycznych")

    * Kontener **wniosków dzienniki inspekcji** zawiera dzienników inspekcji.
    * Kontener **wniosków dzienniki żądania** zawiera dzienników żądanie.

2. W tych kontenerach Dzienniki znajdują się w obszarze następującą strukturę.

        resourceId=/
          SUBSCRIPTIONS/
            <<SUBSCRIPTION_ID>>/
              RESOURCEGROUPS/
                <<RESOURCE_GRP_NAME>>/
                  PROVIDERS/
                    MICROSOFT.DATALAKEANALYTICS/
                      ACCOUNTS/
                        <DATA_LAKE_ANALYTICS_NAME>>/
                          y=####/
                            m=##/
                              d=##/
                                h=##/
                                  m=00/
                                    PT1H.json
    
    > [AZURE.NOTE] `##` Wpisy w ścieżce zawierają rok, miesiąc, dzień i godzinę, w której został utworzony dziennik. Dane Lake analizy tworzy jeden plik co godzinę, więc `m=` zawsze znajduje się wartość `00`.

    Na przykład może być pełną ścieżkę do dziennika inspekcji:
    
        https://adllogs.blob.core.windows.net/insights-logs-audit/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/mydatalakeanalytics/y=2016/m=07/d=18/h=04/m=00/PT1H.json

    Podobnie może być pełną ścieżkę do dziennika żądań:
    
        https://adllogs.blob.core.windows.net/insights-logs-requests/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/mydatalakeanalytics/y=2016/m=07/d=18/h=14/m=00/PT1H.json

## <a name="log-structure"></a>Struktura dziennika

Dzienniki inspekcji i żądanie są dostępne w formacie JSON. W tej sekcji było zobaczyć strukturę JSON żądania i dzienników inspekcji.

### <a name="request-logs"></a>Dzienniki żądań

Oto wpis dziennika żądań sformatowane JSON. Każdy blob ma jeden obiekt główny nazywane **rekordów** , zawierający tablicę obiektów dziennika.

    {
    "records": 
      [     
        . . . .
        ,
        {
             "time": "2016-07-07T21:02:53.456Z",
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/<data_lake_analytics_account_name>",
             "category": "Requests",
             "operationName": "GetAggregatedJobHistory",
             "resultType": "200",
             "callerIpAddress": "::ffff:1.1.1.1",
             "correlationId": "4a11c709-05f5-417c-a98d-6e81b3e29c58",
             "identity": "1808bd5f-62af-45f4-89d8-03c5e81bac30",
             "properties": {
                 "HttpMethod":"POST",
                 "Path":"/JobAggregatedHistory",
                 "RequestContentLength":122,
                 "ClientRequestId":"3b7adbd9-3519-4f28-a61c-bd89506163b8",
                 "StartTime":"2016-07-07T21:02:52.472Z",
                 "EndTime":"2016-07-07T21:02:53.456Z"
                 }
        }
        ,
        . . . .
      ]
    }

#### <a name="request-log-schema"></a>Żądanie schemat dziennika

| Nazwa            | Typ   | Opis                                                                    |
|-----------------|--------|--------------------------------------------------------------------------------|
| czas            | Ciąg | Sygnatura czasowa (w czasie UTC) dziennika                                              |
| resourceId      | Ciąg | Identyfikator zasobu, która zajęła operacji umieść na                            |
| Kategoria        | Ciąg | Kategoria dziennika. Na przykład **żądania**.                                   |
| operationName   | Ciąg | Nazwa operacji, które są rejestrowane. Na przykład GetAggregatedJobHistory.              |
| resultType      | Ciąg | Stan operacji, na przykład 200.                                 |
| callerIpAddress | Ciąg | Adres IP klienta zgłaszającego żądanie                                |
| correlationId   | Ciąg | Identyfikator dziennika. Ta wartość może służyć do grupy zestawu wpisy dziennika pokrewne |
| tożsamości        | Obiekt | Tożsamość, który wygenerował dany dziennik                                            |
| właściwości      | JSON   | W następnej sekcji (żądanie dziennika właściwości schemat) Aby uzyskać szczegółowe informacje |

#### <a name="request-log-properties-schema"></a>Żądanie schemat właściwości dziennika

| Nazwa                 | Typ   | Opis                                               |
|----------------------|--------|-----------------------------------------------------------|
| Element HttpMethod           | Ciąg | Metoda HTTP używana dla tej operacji. Na przykład UZYSKAĆ. |
| Ścieżka                 | Ciąg | Ścieżka operacji została wykonana na                   |
| RequestContentLength | int    | Długość zawartości żądania HTTP                    |
| ClientRequestId      | Ciąg | Identyfikator, która jednoznacznie identyfikuje to żądanie              |
| Czas rozpoczęcia            | Ciąg | Godzinę, serwer otrzymania żądania         |
| Zakończenia              | Ciąg | Czas, co serwer wysyłać odpowiedzi              |

### <a name="audit-logs"></a>Dzienniki inspekcji

Oto wpis dziennika inspekcji sformatowane JSON. Każdy blob ma jeden obiekt główny nazywane **rekordów** , zawierający tablicę obiektów dziennika

    {
    "records": 
      [     
        . . . .
        ,
        {
             "time": "2016-07-28T19:15:16.245Z",
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/<data_lake_ANALYTICS_account_name>",
             "category": "Audit",
             "operationName": "JobSubmitted",
             "identity": "user@somewhere.com",
             "properties": {
                 "JobId":"D74B928F-5194-4E6C-971F-C27026C290E6",
                 "JobName": "New Job", 
                 "JobRuntimeName": "default",
                 "SubmitTime": "7/28/2016 7:14:57 PM"
                 }
        }
        ,
        . . . .
      ]
    }

#### <a name="audit-log-schema"></a>Schemat dziennika inspekcji

| Nazwa            | Typ   | Opis                                                                    |
|-----------------|--------|--------------------------------------------------------------------------------|
| czas            | Ciąg | Sygnatura czasowa (w czasie UTC) dziennika                                              |
| resourceId      | Ciąg | Identyfikator zasobu, która zajęła operacji umieść na                            |
| Kategoria        | Ciąg | Kategoria dziennika. Na przykład **inspekcji**.                                      |
| operationName   | Ciąg | Nazwa operacji, które są rejestrowane. Na przykład JobSubmitted.              |
| resultType | Ciąg | Podstanu dla stan zadania (operationName). |
| resultSignature | Ciąg | Aby uzyskać dodatkowe informacje o stanie zadań (operationName). |
| tożsamości      | Ciąg | Użytkownik, który zażądał operacji. Na przykład susan@contoso.com.                                 |
| właściwości      | JSON   | W następnej sekcji (schemat właściwości dziennika inspekcji) Aby uzyskać szczegółowe informacje |

> [AZURE.NOTE] __resultType__ __resultSignature__ zawierają informacje o wynik operacji i zawierać tylko wartość, jeśli operacja ukończona. Na przykład zawierają wartości, gdy __operationName__ zawiera wartość __JobStarted__ lub __JobEnded__.

#### <a name="audit-log-properties-schema"></a>Schemat właściwości dziennika inspekcji

| Nazwa       | Typ   | Opis                              |
|------------|--------|------------------------------------------|
| JobId | Ciąg | Identyfikator przypisany do zadania  |
| JobName | Ciąg | Nazwy udostępnionej dla zadania |
| JobRunTime | Ciąg | Środowisko uruchomieniowe, użytych do opracowania zadania |
| SubmitTime | Ciąg | Czas (w czasie UTC) przesłania zadania |
| Czas rozpoczęcia | Ciąg | Czas zadanie rozpoczęte po przesyłania (w czasie UTC). |
| Zakończenia | Ciąg | Czas zakończenia zadania. |
| Równoległości | Ciąg | Liczba jednostek danych Lake analizy wymagane dla tego zadania podczas przesyłania. |

> [AZURE.NOTE] __SubmitTime__, __czas rozpoczęcia__i __zakończenia__ __równoległości__ zawierają informacje o operacji i zawierać tylko wartość, jeśli operacji ma rozpoczęte lub zakończone. Na przykład __SubmitTime__ zawiera wartość po __operationName__ wskazuje __JobSubmitted__.

## <a name="process-the-log-data"></a>Przetwarzanie danych dziennika

Azure analizy Lake danych zawiera próbki dotyczące sposobu przetwarzania i analizowania danych dziennika. Próbki można znaleźć w [https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample](https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample). 


## <a name="next-steps"></a>Następne kroki

- [Omówienie analizy Lake Azure danych](data-lake-analytics-overview.md)


