<properties 
   pageTitle="Wyświetlanie dzienników diagnostycznych dla magazynu Lake danych Azure | Microsoft Azure" 
   description="Opis sposobu instalacji i uzyskać dostęp do dzienników diagnostycznych dla magazynu Lake danych Azure " 
   services="data-lake-store" 
   documentationCenter="" 
   authors="nitinme" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="10/05/2016"
   ms.author="nitinme"/>

# <a name="accessing-diagnostic-logs-for-azure-data-lake-store"></a>Uzyskiwanie dostępu do dzienników diagnostycznych dla magazynu Lake danych Azure

Informacje na temat sposobu włączyć rejestrowanie diagnostyczne dla Twojego konta magazynu Lake danych oraz wyświetlanie dzienników zbierane dla Twojego konta.

Organizacje można włączyć rejestrowanie diagnostyczne dla swojego konta magazynu Lake Azure danych na potrzeby zbierania danych programu access inspekcji ścieżek, zawierającego informacje, takie jak listy użytkowników dostęp do danych, jak często uzyskiwania dostępu do danych, ile dane są przechowywane w konta itp.

## <a name="prerequisites"></a>Wymagania wstępne

- **Azure subskrypcji**. Zobacz [Azure pobrać bezpłatną wersję próbną](https://azure.microsoft.com/pricing/free-trial/).

- **Konto azure magazynu Lake danych**. Postępuj zgodnie z instrukcjami na [Rozpoczynanie pracy z magazynu Lake danych Azure za pomocą portalu Azure](data-lake-store-get-started-portal.md).

## <a name="enable-diagnostic-logging-for-your-data-lake-store-account"></a>Włącz rejestrowanie diagnostyczne dla Twojego konta magazynu Lake danych

1. Logowanie się do nowego [Azure Portal](https://portal.azure.com).

2. Otwórz konto magazynu Lake danych i karta konta usługi magazynu Lake danych, kliknij pozycję **Ustawienia**, a następnie kliknij **Ustawień diagnostycznych**.

3. W **diagnostyczne** karta wprowadzić następujące zmiany, aby skonfigurować rejestrowanie diagnostyczne.

    ![Włącz rejestrowanie diagnostyczne] (./media/data-lake-store-diagnostic-logs/enable-diagnostic-logs.png "Włączanie dzienników diagnostycznych")

    * Ustaw **Stan** **na** Aby włączyć rejestrowanie diagnostyczne.
    * Istnieje możliwość-proces magazynu danych na dwa różne sposoby.
        * Wybierz opcję, aby **wyeksportować do Centrum zdarzeń** do transmisji danych dziennika do koncentratora zdarzenia Azure. Najprawdopodobniej będzie Użyj tej opcji, jeśli masz potok przetwarzania podrzędne do analizowania przychodzących dzienniki w czasie rzeczywistym. Jeśli wybierzesz tę opcję, musisz podać szczegóły koncentratora zdarzenia Azure, którego chcesz użyć.
        * Wybierz opcję, aby **wyeksportować do konta miejsca do magazynowania** do przechowywania dzienników z klientem magazyn Azure. Użyj tej opcji, jeśli chcesz zarchiwizować dane, które będą przetwarzane partii w późniejszym terminie. Po wybraniu tej opcji należy podać konto Azure miejsca do magazynowania, aby zapisać dzienniki do.
    * Określ, czy chcesz uzyskać dzienniki inspekcji lub dzienników żądanie lub oba.
    * Określ liczbę dni, dla których muszą być przechowywane dane.
    * Kliknij przycisk **Zapisz**.

Po włączeniu ustawień diagnostycznych możesz obejrzeć dzienniki na karcie **Dzienniki diagnostyczne** .

## <a name="view-diagnostic-logs-for-your-data-lake-store-account"></a>Wyświetlanie dzienników diagnostycznych dla Twojego konta magazynu Lake danych

Istnieją dwa sposoby wyświetlania danych dziennika dla Twojego konta magazynu Lake danych.

* Z konta magazynu Lake danych ustawienia widoku
* Z konta magazynu platformy Azure przechowywania danych

### <a name="using-the-data-lake-store-settings-view"></a>Przy użyciu widoku ustawienia sklepu Lake danych

1. Karta **Ustawienia** konta usługi magazynu Lake danych kliknij **Dzienniki diagnostyczne**.

    ![Rejestrowanie diagnostyczne widoku] (./media/data-lake-store-diagnostic-logs/view-diagnostic-logs.png "Wyświetlanie dzienników diagnostycznych") 

2. W karta **Dzienniki diagnostyczne** powinna być widoczna dzienniki posortowane według **Dzienniki inspekcji** i **Dzienniki żądanie**.
    * Dzienniki żądanie Przechwytywanie każdego żądania interfejsu API na rachunku magazynu Lake danych.
    * Dzienniki inspekcji są podobne do Dzienniki żądań, ale zapewnia bardziej szczegółowy podział operacji wykonywanych na koncie magazynu Lake danych. Na przykład połączenie interfejsu API przekazywanych w dziennikach żądania może spowodować wiele operacji "Dołączanie" w dzienników inspekcji.

3. Kliknij łącze **Pobierz** dla każdej pozycji dziennika, aby pobrać pliki dziennika.

### <a name="from-the-azure-storage-account-that-contains-log-data"></a>Z konta magazynu platformy Azure, która zawiera dane dziennika

1. Otwórz karta konta magazynu platformy Azure skojarzone z magazynu Lake danych do logowania, a następnie kliknij blob. Karta **obiektów Blob usługi** lista dwoma kontenerami.

    ![Rejestrowanie diagnostyczne widoku] (./media/data-lake-store-diagnostic-logs/view-diagnostic-logs-storage-account.png "Wyświetlanie dzienników diagnostycznych")

    * Kontener **wniosków dzienniki inspekcji** zawiera dzienników inspekcji.
    * Kontener **wniosków dzienniki żądania** zawiera dzienników żądanie.

2. W tych kontenerach Dzienniki znajdują się w obszarze następującą strukturę.

    ![Rejestrowanie diagnostyczne widoku] (./media/data-lake-store-diagnostic-logs/view-diagnostic-logs-storage-account-structure.png "Wyświetlanie dzienników diagnostycznych")

    Na przykład może to być pełną ścieżkę do dziennika inspekcji`https://adllogs.blob.core.windows.net/insights-logs-audit/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/mydatalakestore/y=2016/m=07/d=18/h=04/m=00/PT1H.json`

    Podobnie, może być pełną ścieżkę do dziennika żądań`https://adllogs.blob.core.windows.net/insights-logs-requests/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/mydatalakestore/y=2016/m=07/d=18/h=14/m=00/PT1H.json`

## <a name="understand-the-structure-of-the-log-data"></a>Opis struktury danych dziennika

Dzienniki inspekcji i żądanie są dostępne w formacie JSON. W tej sekcji było zobaczyć strukturę JSON żądania i dzienników inspekcji.

### <a name="request-logs"></a>Dzienniki żądań

Oto wpis dziennika żądań sformatowane JSON. Każdy obiektów blob ma jeden obiekt główny nazywane **rekordów** , zawierający tablicę obiektów dziennika.

    {
    "records": 
      [     
        . . . .
        ,
        {
             "time": "2016-07-07T21:02:53.456Z",
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/<data_lake_store_account_name>",
             "category": "Requests",
             "operationName": "GETCustomerIngressEgress",
             "resultType": "200",
             "callerIpAddress": "::ffff:1.1.1.1",
             "correlationId": "4a11c709-05f5-417c-a98d-6e81b3e29c58",
             "identity": "1808bd5f-62af-45f4-89d8-03c5e81bac30",
             "properties": {"HttpMethod":"GET","Path":"/webhdfs/v1/Samples/Outputs/Drivers.csv","RequestContentLength":0,"ClientRequestId":"3b7adbd9-3519-4f28-a61c-bd89506163b8","StartTime":"2016-07-07T21:02:52.472Z","EndTime":"2016-07-07T21:02:53.456Z"}
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
| operationName   | Ciąg | Nazwa operacji, które są rejestrowane. Na przykład getfilestatus.              |
| resultType      | Ciąg | Stan operacji, na przykład 200.                                 |
| callerIpAddress | Ciąg | Adres IP klienta zgłaszającego żądanie                                |
| correlationId   | Ciąg | Identyfikator dziennika, który można używane do łączenia zestawu wpisy dziennika pokrewne |
| tożsamości        | Obiekt | Tożsamość, który wygenerował dany dziennik                                            |
| właściwości      | JSON   | Poniżej podano szczegółowe informacje                                                          |

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

Oto wpis dziennika inspekcji sformatowane JSON. Każdy obiektów blob ma jeden obiekt główny nazywane **rekordów** , zawierający tablicę obiektów dziennika

    {
    "records": 
      [     
        . . . .
        ,
        {
             "time": "2016-07-08T19:08:59.359Z",
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/<data_lake_store_account_name>",
             "category": "Audit",
             "operationName": "SeOpenStream",
             "resultType": "0",
             "correlationId": "381110fc03534e1cb99ec52376ceebdf;Append_BrEKAmg;25.66.9.145",
             "identity": "A9DAFFAF-FFEE-4BB5-A4A0-1B6CBBF24355",
             "properties": {"StreamName":"adl://<data_lake_store_account_name>.azuredatalakestore.net/logs.csv"}
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
| operationName   | Ciąg | Nazwa operacji, które są rejestrowane. Na przykład getfilestatus.              |
| resultType      | Ciąg | Stan operacji, na przykład 200.                                 |
| correlationId   | Ciąg | Identyfikator dziennika, który można używane do łączenia zestawu wpisy dziennika pokrewne |
| tożsamości        | Obiekt | Tożsamość, który wygenerował dany dziennik                                            |
| właściwości      | JSON   | Poniżej podano szczegółowe informacje                                                          |

#### <a name="audit-log-properties-schema"></a>Schemat właściwości dziennika inspekcji

| Nazwa       | Typ   | Opis                              |
|------------|--------|------------------------------------------|
| StreamName | Ciąg | Ścieżka operacji została wykonana na  |


## <a name="samples-to-process-the-log-data"></a>Przykłady przetwarzania danych dziennika

Azure magazynu Lake danych zawiera próbki dotyczące sposobu przetwarzania i analizowania danych dziennika. Próbki można znaleźć w [https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample](https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample). 


## <a name="see-also"></a>Zobacz też

- [Omówienie magazynu Lake danych Azure](data-lake-store-overview.md)
- [Zabezpieczanie danych w magazynie Lake danych](data-lake-store-secure-data.md)

