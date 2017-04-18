<properties
    pageTitle="Zbieranie dzienników i wskaźniki danych za pomocą analizy miejsca do magazynowania | Microsoft Azure"
    description="Analizy miejsca do magazynowania umożliwia śledzenie danych metryki dla wszystkich usług miejsca do magazynowania i zbieranie dzienników magazyn obiektów Blob, kolejki i tabeli."
    services="storage"
    documentationCenter=""
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="robinsh"/>

# <a name="storage-analytics"></a>Analizy miejsca do magazynowania

## <a name="overview"></a>Omówienie

Azure analizy miejsca do magazynowania wykonuje rejestrowanie, a także danych metryki konta miejsca do magazynowania. Za pomocą tych danych do śledzenia żądania, analizowania trendów zastosowania i diagnozowanie problemów za pomocą konta miejsca do magazynowania.

Aby użyć analizy przestrzeni dyskowej, należy włączyć je pojedynczo dla każdej usługi, które chcesz monitorować. Możesz ją włączyć [Azure Portal](https://portal.azure.com). Aby uzyskać szczegółowe informacje zobacz [Monitorowanie konto miejsca do magazynowania w Azure Portal](storage-monitor-storage-account.md). Można także włączyć analizy miejsca do magazynowania programowo za pośrednictwem interfejsu API usługi REST lub biblioteka klienta. Używać operacji [Uzyskaj właściwości usługi obiektów Blob](https://msdn.microsoft.com/library/hh452239.aspx), [Uzyskaj właściwości usługi kolejki](https://msdn.microsoft.com/library/hh452243.aspx) [Uzyskaj właściwości usługi tabeli](https://msdn.microsoft.com/library/hh452238.aspx)i [Uzyskaj plik właściwości usługi](https://msdn.microsoft.com/library/mt427369.aspx) umożliwiające analizy miejsca do magazynowania dla każdej usługi.

Zagregowane dane są przechowywane w znanych obiektów blob (w przypadku rejestrowanie) i w tabelach znanego (dla metryki), które mogą uzyskać dostęp za pomocą obiektów Blob usługi i tabeli interfejsów API.

Analizy magazynowania ma limit 20 TB od ilości przechowywanych danych, niezależny od całkowitego limitu dla Twojego konta przestrzeni dyskowej. Aby uzyskać więcej informacji na temat rozliczeń i zasady przechowywania danych zobacz [analizy przestrzeni dyskowej i rozliczeń](https://msdn.microsoft.com/library/hh360997.aspx). Aby uzyskać więcej informacji na temat limitów miejsca do magazynowania konta zobacz [skalowalność miejsca do magazynowania Azure i cele](storage-scalability-targets.md).

Szczegółowo przewodnika przy użyciu analizy miejsca do magazynowania i innych narzędzi do identyfikowania, diagnozowanie i rozwiązywanie problemów związanych z magazyn Azure zobacz [Monitorowanie diagnozowanie i rozwiązywanie problemów z magazynem tabel platformy Microsoft Azure](storage-monitoring-diagnosing-troubleshooting.md).


## <a name="about-storage-analytics-logging"></a>Informacje o rejestrowanie analizy miejsca do magazynowania

Analizy miejsca do magazynowania rejestruje szczegółowe informacje dotyczące żądania pomyślnego i nie powiodło się z usługą magazynu. Te informacje można monitorować poszczególne żądania i diagnozowanie problemów z usługą magazynu. Żądania są rejestrowane na zasadzie optymalny.

Wpisy dziennika są tworzone tylko wtedy, gdy jest wykonanie usługi miejsca do magazynowania. Na przykład jeśli konto miejsca do magazynowania ma aktywności w jego obiektów Blob usługi, ale nie w jego usługi tabeli lub kolejki, zostanie utworzony tylko dzienniki odnoszących się do usługi obiektów Blob.

Rejestrowanie analizy miejsca do magazynowania nie jest dostępna dla usługi Azure pliku.

### <a name="logging-authenticated-requests"></a>Żądania rejestrowania uwierzytelniony

Następujące typy żądań uwierzytelnionych są rejestrowane:

- Żądania pomyślnie.

- Nie można żądania, w tym przekroczenia limitu czasu, ograniczania sieci, autoryzacji i inne błędy.

- Za pomocą udostępnionego dostępu podpisu (SA), łącznie z żądania nie powiodło się i pomyślnego żądania.

- Żądania do analizy danych.

Wnioski analizy miejsca do magazynowania, takich jak tworzenie dziennika lub usunięcia, nie jest zalogowany. Pełną listę danych zarejestrowanych opisano w tematach [operacji rejestrowane analizy magazynu i komunikaty o stanie](https://msdn.microsoft.com/library/hh343260.aspx) i [Format dziennika analizy przestrzeni dyskowej](https://msdn.microsoft.com/library/hh343259.aspx) .

### <a name="logging-anonymous-requests"></a>Anonimowe żądania rejestrowania
Następujące typy anonimowe żądania są rejestrowane:

- Żądania pomyślnie.

- Błędy serwera.

- Błędy przekroczenia limitu czasu dla klienta i serwera.

- Nie powiodło się żądania pobierania z kodem błędu 304 (nie zmodyfikowane).

Inne niepowodzeniu żądania anonimowe nie jest zalogowany. Pełną listę danych zarejestrowanych opisano w tematach [operacje rejestrowane analizy miejsca do magazynowania i komunikaty o stanie](https://msdn.microsoft.com/library/hh343260.aspx) i [Format dziennika analizy miejsca do magazynowania](https://msdn.microsoft.com/library/hh343259.aspx) .

### <a name="how-logs-are-stored"></a>Sposób przechowywania dzienników
Wszystkie dzienniki są przechowywane w bloku BLOB w kontenerze o nazwie $logs, która powoduje automatyczne utworzenie analizy miejsca do magazynowania jest włączona dla konta miejsca do magazynowania. Kontener $logs znajduje się w obszarze nazw obiektów blob konta miejsca do magazynowania, na przykład: `http://<accountname>.blob.core.windows.net/$logs`. Ten kontener nie można usunąć po włączeniu analizy miejsca do magazynowania, ale jego zawartość można usunąć.

>[Azure.NOTE] Kontener $logs nie jest wyświetlana podczas kontenera listy operacji, takich jak metoda [ListContainers](https://msdn.microsoft.com/library/azure/dd179352.aspx) . Muszą być dostępne bezpośrednio. Na przykład metoda [ListBlobs](https://msdn.microsoft.com/library/azure/dd135734.aspx) umożliwia dostęp do obiektów blob w `$logs` kontener.
Jak żądania są rejestrowane, analizy miejsca do magazynowania będzie Prześlij pośrednie wyniki jako bloków konstrukcyjnych. Okresowo analizy magazynowania zatwierdzenie tych bloków i uzyskiwać do nich dostęp jako obiektów blob.

Zduplikowane rekordy może istnieć dzienników utworzone w tej samej godziny. Można określić, czy rekord jest to duplikat, zaznaczając pole wyboru numer **IdentyfikatorŻądania** i **działania** .

### <a name="log-naming-conventions"></a>Konwencje nazewnictwa dziennika
Każdy dziennika zostaną zapisane w następującym formacie.

    <service-name>/YYYY/MM/DD/hhmm/<counter>.log

W poniższej tabeli opisano każdego atrybutu w polu Nazwa dziennika.

| Atrybut         | Opis                                                                                                                                                                                   |
|----------------   |--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------   |
| < nazwa usługi >    | Nazwa Usługa magazynu. Na przykład: obiektów blob, tabeli lub kolejki.                                                                                                                          |
| RRRR              | Czterech cyfr roku w dzienniku. Na przykład: 2011.                                                                                                                                           |
| MM                | Miesiąc dwóch cyfr w dzienniku. Na przykład: 07.                                                                                                                                             |
| DD                | Miesiąc dwóch cyfr w dzienniku. Na przykład: 07.                                                                                                                                             |
| hh                | Godzina dwóch cyfr, który wskazuje godzinę rozpoczęcia dzienników w formacie UTC 24-godzinnym. Na przykład: 18.                                                                                     |
| mm                | Liczby dwucyfrowej wskazujący początkowe minutę dzienników. Ta wartość nie jest obsługiwana w bieżącej wersji analizy miejsca do magazynowania, a jej wartość będzie zawsze 00.     |
| <counter>         | Od zera licznik z sześciu cyfr wskazującą liczbę obiektów blob dziennika wygenerowany przez usługę magazynowania godziny przedziału czasu. Licznik zaczyna się od 000000. Na przykład: 000001.     |

Poniżej przedstawiono pełną Przykładowa nazwa dziennika, która łączy funkcje poprzednich przykładach.

    blob/2011/07/31/1800/000001.log

Poniżej przedstawiono przykładowy identyfikator URI, który umożliwia dostęp do poprzedniego dziennika.

    https://<accountname>.blob.core.windows.net/$logs/blob/2011/07/31/1800/000001.log

Po zarejestrowaniu żądania magazynu wynikowej nazwa dziennika skorelowany godziny zakończenia żądanej operacji. Na przykład Ukończono żądanie GetBlob godzinie 6:30 na 2011-7-31 dziennik powinny być zapisane z prefiksem następujące czynności:`blob/2011/07/31/1800/`

### <a name="log-metadata"></a>Metadanych dziennika
Wszystkie obiekty BLOB dziennika są przechowywane metadanymi, który może być używany do identyfikowania co rejestrowanie zawiera obiektów blob. W poniższej tabeli opisano każdy atrybut metadanych.

| Atrybut     | Opis                                                                                                                                                                                                                                                   |
|------------   |-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------    |
| LogType       | W tym artykule opisano, czy dziennik zawiera informacje dotyczące czytanie, pisanie i usuwanie operacji. Ta wartość może zawierać dla jednego typu lub kombinacja wszystkich trzech, rozdzielając je przecinkami. Przykład 1: pisanie; Przykład 2: Odczyt, zapis; Przykład 3: Odczyt, zapis, usuwanie.    |
| Czas rozpoczęcia     | Najwcześniejsza godzina wpisu w dzienniku w formularzu YYYY-MM-DDThh:mm:ssZ. Na przykład: 2011-07-31T18:21:46Z.                                                                                                                                             |
| Zakończenia       | Ostateczny termin wpis w dzienniku w formularzu YYYY-MM-DDThh:mm:ssZ. Na przykład: 2011-07-31T18:22:09Z.                                                                                                                                               |
| LogVersion    | Wersja formatu dziennika. Jedyna obsługiwana wartość jest obecnie 1.0.                                                                                                                                                                                     |

Poniższa lista zawiera metadane całą próbkę przy użyciu poprzednich przykładach.

- LogType = zapisu

- Czas rozpoczęcia = 2011-07-31T18:21:46Z

- Zakończenia = 2011-07-31T18:22:09Z

- LogVersion = 1.0

### <a name="accessing-logging-data"></a>Uzyskiwanie dostępu do rejestrowania danych

Wszystkie dane w `$logs` kontener może uzyskać do nich dostęp za pomocą usługi obiektów Blob interfejsy API, w tym za pośrednictwem interfejsów API .NET dostarczony przez Azure zarządzane biblioteki. Administratorem konta miejsca do magazynowania można przeczytać i usuwanie dzienników, ale nie Tworzenie lub aktualizowanie ich. Zarówno metadanych dziennika, jak i nazwa dziennika można używać podczas wykonywania kwerend dziennika. Istnieje możliwość dzienniki danej godziny do są wyświetlane w kolejności, ale metadanych zawsze określa przedziału czasu wpisy dziennika w dzienniku. W związku z tym można użyć kombinacji nazwy dzienników i metadanych podczas szukania określonych log.

## <a name="about-storage-analytics-metrics"></a>Informacje o metryki magazynowania analizy

Miejsca do magazynowania analizy mogą zawierać metryki zawierające zagregowane transakcji statystyki i pojemnością dane dotyczące żądania usługi Magazyn. Transakcje zgłasza się zarówno na poziomie operacja interfejsu API, a także na poziomie usługi miejsca do magazynowania, a wydajność jest zgłoszone na poziomie usługi miejsca do magazynowania. Metryki danych może służyć do przeanalizowania użycie usługi miejsca do magazynowania, diagnozowanie problemów z żądania usługi miejsca do magazynowania i zwiększyć wydajność aplikacji korzystających z usługi.

Aby użyć analizy miejsca do magazynowania, należy włączyć je pojedynczo dla każdej usługi, które chcesz monitorować. Możesz ją włączyć [Azure Portal](https://portal.azure.com). Aby uzyskać szczegółowe informacje zobacz [Monitorowanie konto miejsca do magazynowania w Azure Portal](storage-monitor-storage-account.md). Można także włączyć analizy miejsca do magazynowania programowo za pośrednictwem interfejsu API usługi REST lub biblioteka klienta. Używać operacji **Uzyskaj właściwości usługi** umożliwiające analizy miejsca do magazynowania dla każdej usługi.

### <a name="transaction-metrics"></a>Transakcji metryki

Zestaw danych został zapisany w odstępach co godzinę lub minuta dla każdej usługi miejsca do magazynowania i zażądał operacji interfejsu API, w tym ingress i wyjściowego, dostępność, błędy i podzielić żądania wartości procentowych. Można wyświetlić pełną listę szczegółów transakcji w temacie [Schematu tabeli metryki analizy miejsca do magazynowania](https://msdn.microsoft.com/library/hh343264.aspx) .

Dane transakcji są zapisywane na dwóch poziomach — poziomem usług i interfejsu API operacji. Na poziomie usługi statystyki podsumowywania wszystkich żądanych operacje interfejsu API są zapisywane na podmiot tabeli co godzinę, nawet jeśli nie żądań usługi. Na poziomie operacja interfejsu API statystyki są zapisywane tylko do jednostki, jeśli zażądano operacji w ramach danej godziny.

Na przykład po wykonaniu operacji **GetBlob** od usługi obiektów Blob metryki magazynowania analizy zarejestrować żądania i obejmować zagregowane dane zarówno obiektów Blob usługi, a także operacji **GetBlob** . Jednak jeśli żadnej operacji **GetBlob** żąda ciągu godziny, jednostka nie zostaną zapisane na `$MetricsTransactionsBlob` tej operacji.

Metryki transakcji są rejestrowane zarówno żądania użytkowników, jak i wnioski analizy miejsca do magazynowania, się. Na przykład wnioski analizy miejsca do magazynowania zapisać dzienniki i jednostki tabeli są rejestrowane. Aby uzyskać więcej informacji na temat sposobu wystawiona te żądania zobacz [analizy przestrzeni dyskowej i rozliczeń](https://msdn.microsoft.com/library/hh360997.aspx).

### <a name="capacity-metrics"></a>Wskaźniki wydajności

>[AZURE.NOTE] Obecnie metryki wydajność pracy są dostępne tylko dla obiektów Blob usługi. Metryki wydajności dla usługi tabeli i kolejki będzie dostępna w przyszłych wersjach analizy miejsca do magazynowania.

Wydajność dane są zapisywane codziennie konto magazyn obiektów Blob usługi, a dwa podmioty tabeli są zapisywane. Jeden podmiot zawiera statystyki danych przez użytkownika, a drugi zawiera statystyki dotyczące `$logs` kontenera obiektów blob używane przez analizy miejsca do magazynowania. `$MetricsCapacityBlob` Tabela zawiera następujące statystyki:

- **Wydajność**: ilość miejsca w magazynie używane przez konto magazyn obiektów Blob usługi, w bajtach.

- **ContainerCount**: liczba kontenerów obiektów blob konta magazyn obiektów Blob usługi.

- **ObjectCount**: liczba gwarantowanej i niezatwierdzonym blob blok lub na stronie konta magazyn obiektów Blob usługi.

Aby uzyskać więcej informacji na temat metryki wydajność zobacz [Schematu tabeli metryki analizy miejsca do magazynowania](https://msdn.microsoft.com/library/hh343264.aspx).

### <a name="how-metrics-are-stored"></a>Jak są przechowywane metryki

Wszystkie dane metryki dla każdej z usług przechowywania są przechowywane w trzech tabelach zastrzeżone w tej usłudze: jednej tabeli, aby uzyskać informacje o transakcji, jednej tabeli transakcji minuta informacji i pojemnością informacji z innej tabeli. Informacje o transakcji transakcji i minuty składa się z danymi i odpowiadania na wezwania i pojemnością zawiera miejsca do magazynowania danych dotyczących użycia. Metryki godzina, minuta metryki i pojemnością konto magazyn obiektów Blob usługi są dostępne w tabelach, które są nazywane zgodnie z opisem w poniższej tabeli.

| Poziom metryki                         | Nazwy tabel                                                                                                                   | Obsługiwane wersje                                                                                                                        |
|------------------------------------   |-----------------------------------------------------------------------------------------------------------------------------  |---------------------------------------------------------------------------------------------------------------------------------------------- |
| Metryka co godzinę, Lokalizacja podstawowa      |  $MetricsTransactionsBlob <br/>$MetricsTransactionsTable <br/> $MetricsTransactionsQueue                                                  | Wersje przed 2013-08-15 tylko. Podczas te nazwy są obsługiwani, zaleca się, że możesz przełączyć się przy użyciu poniższych tabel.  |
| Co godzinę metryki, Lokalizacja podstawowa      | $MetricsHourPrimaryTransactionsBlob <br/>$MetricsHourPrimaryTransactionsTable <br/>$MetricsHourPrimaryTransactionsQueue               | Wszystkie wersje, łącznie z 2013-08-15.                                                                                                               |
| Minuta metryki, Lokalizacja podstawowa      | $MetricsMinutePrimaryTransactionsBlob <br/>$MetricsMinutePrimaryTransactionsTable <br/>$MetricsMinutePrimaryTransactionsQueue         | Wszystkie wersje, łącznie z 2013-08-15.                                                                                                               |
| Metryka co godzinę, lokalizacji pomocniczej    | $MetricsHourSecondaryTransactionsBlob <br/>$MetricsHourSecondaryTransactionsTable <br/>$MetricsHourSecondaryTransactionsQueue         | Wszystkie wersje, łącznie z 2013-08-15. Musi być włączony replikacji zbędne geo dostęp do odczytu.                                                    |
| Minuta miar, lokalizacji pomocniczej    | $MetricsMinuteSecondaryTransactionsBlob <br/>$MetricsMinuteSecondaryTransactionsTable <br/>$MetricsMinuteSecondaryTransactionsQueue   | Wszystkie wersje, łącznie z 2013-08-15. Musi być włączony replikacji zbędne geo dostęp do odczytu.                                                    |
| Wydajność pracy (tylko usługa obiektów Blob)          | $MetricsCapacityBlob                                                                                                          | Wszystkie wersje, łącznie z 2013-08-15.                                                                                                               |


W poniższych tabelach są tworzone automatycznie po włączeniu analizy miejsca do magazynowania dla konta miejsca do magazynowania. Są one używane przy użyciu nazw konta przestrzeni dyskowej, na przykład:`https://<accountname>.table.core.windows.net/Tables("$MetricsTransactionsBlob")`

### <a name="accessing-metrics-data"></a>Uzyskiwanie dostępu do danych metryki

Wszystkie dane w tabelach metryki można uzyskać do nich dostęp za pomocą interfejsów API usługi tabeli, w tym za pośrednictwem interfejsów API .NET dostarczony przez Azure zarządzane biblioteki. Administratorem konta magazynu można przeczytać i usuwać jednostki tabeli, ale nie Tworzenie lub aktualizowanie ich.

## <a name="billing-for-storage-analytics"></a>Rozliczenia dla analizy miejsca do magazynowania

Analizy miejsca do magazynowania jest włączona przez właściciela konta miejsca do magazynowania; nie jest włączona domyślnie. Wszystkie dane metryki są zapisywane w usługach konta miejsca do magazynowania. W wyniku każdej operacji zapisu przez analizy magazynowania jest rozliczana. Ponadto ilość miejsca w magazynie używane przez metryki danych jest również rozliczaniu.

Fakturowanego są następujące akcje wykonywane przez analizy miejsca do magazynowania:

- Żądania utworzenia obiektów blob logowania. 

- Żądania utworzenia tabeli jednostek miar.

Jeśli masz skonfigurowane zasad przechowywania danych, nie zajmujesz transakcji Usuń podczas analizy miejsca do magazynowania powoduje usunięcie starych danych rejestrowania i wskaźniki. Jednak transakcje usuwania w kliencie jest rozliczana. Aby uzyskać więcej informacji na temat zasady przechowywania zobacz [Ustawianie zasad przechowywania danych analizy miejsca do magazynowania](https://msdn.microsoft.com/library/azure/hh343263.aspx).

### <a name="understanding-billable-requests"></a>Opis żądania fakturowanego

Każde żądanie do konta usługi miejsca do magazynowania jest rozliczana albo niepodlegający rozliczaniu. Analizy miejsca do magazynowania, gdy każdy indywidualne żądanie do usługi, łącznie z komunikatem o stanie wskazuje obsługi żądania. Podobnie analizy miejsca do magazynowania przechowuje metryki dla usługi i działania interfejsu API usługi, w tym wartości procentowe i liczba niektóre komunikaty o stanie. Razem tych funkcji może pomóc analizowanie wezwaniach fakturowanego, ulepszania w swojej aplikacji i diagnozowanie problemów z żądania usługi. Aby uzyskać więcej informacji na temat rozliczeń zobacz [Opis Azure miejsca do magazynowania rozliczenia - przepustowości, transakcje i wydajności](http://blogs.msdn.com/b/windowsazurestorage/archive/2010/07/09/understanding-windows-azure-storage-billing-bandwidth-transactions-and-capacity.aspx).

Podczas przeglądania danych analizy miejsca do magazynowania, do określenia, które żądania są fakturowanego za pomocą tabel w temacie [operacje rejestrowane analizy miejsca do magazynowania i komunikaty o stanie](https://msdn.microsoft.com/library/azure/hh343260.aspx) . Następnie możesz porównać dzienników i danych metryki komunikaty o stanie użytkownikom, jeśli naliczono dla określonego żądania usługi. Umożliwia także tabel w poprzednim temacie, aby sprawdzić dostępność usługi miejsca do magazynowania lub poszczególnych operacja interfejsu API.

## <a name="next-steps"></a>Następne kroki

### <a name="setting-up-storage-analytics"></a>Konfigurowanie analizy miejsca do magazynowania
- [Monitorowanie konto przestrzeni dyskowej w Azure Portal](storage-monitor-storage-account.md)
- [Włączanie i konfigurowanie przechowywania analizy](https://msdn.microsoft.com/library/hh360996.aspx)

### <a name="storage-analytics-logging"></a>Rejestrowanie analizy miejsca do magazynowania  
- [Informacje o rejestrowanie analizy miejsca do magazynowania](https://msdn.microsoft.com/library/hh343262.aspx)
- [Format dziennika analizy miejsca do magazynowania](https://msdn.microsoft.com/library/hh343259.aspx)
- [Analizy miejsca do magazynowania rejestrowane operacje i komunikaty o stanie](https://msdn.microsoft.com/library/hh343260.aspx)

### <a name="storage-analytics-metrics"></a>Metryki magazynowania analizy
- [Informacje o metryki magazynowania analizy](https://msdn.microsoft.com/library/hh343258.aspx)
- [Schematu tabeli metryki analizy miejsca do magazynowania](https://msdn.microsoft.com/library/hh343264.aspx)
- [Analizy miejsca do magazynowania rejestrowane operacje i komunikaty o stanie](https://msdn.microsoft.com/library/hh343260.aspx)  
