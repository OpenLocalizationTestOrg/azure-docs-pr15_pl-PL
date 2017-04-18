<properties
    pageTitle="Przesyłanie strumieniowe wyjściowe analizy: opcje miejscem do magazynowania, analizy | Microsoft Azure"
    description="Informacje na temat kierowanie analizy strumieniu dane wyjściowe z poleceniem Power BI dla wyniki analizy."
    keywords="Przekształcanie, wyniki analizy, opcje przechowywania danych"
    services="stream-analytics,documentdb,sql-database,event-hubs,service-bus,storage"
    documentationCenter="" 
    authors="jeffstokes72"
    manager="jhubbard" 
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>

# <a name="stream-analytics-outputs-options-for-storage-analysis"></a>Przesyłanie strumieniowe wyjściowe analizy: opcje miejscem do magazynowania, analizy

Podczas tworzenia zadania analizy strumieniu, należy rozważyć, jak zużycia danych. Jak będzie wyniki analizy strumieniu zadania i miejsce, w którym będzie należy zachować?

W celu umożliwienia różnych wzorców aplikacji, analizy strumieniu Azure ma inne opcje przechowywania danych wyjściowych i przeglądanie wyników analizy. Ta ułatwia wyświetlanie informacji zadania i zapewnia elastyczność zużycie i miejsca do magazynowania danych wyjściowych zadania dla danych składu i innych celów. Dowolne dane wyjściowe skonfigurowany w zadaniu musi istnieć rozpoczęcia zadania oraz zdarzenia rozpocząć ułożony. Na przykład jeśli używasz magazyn obiektów Blob jako wynik zadania nie utworzy konto miejsca do magazynowania automatycznie. Musi być utworzone przez użytkownika, przed rozpoczęciem zadania ASA.

## <a name="azure-data-lake-store"></a>Magazyn Lake danych Azure

Analizy strumieniu obsługuje [Magazynu Lake danych Azure](https://azure.microsoft.com/services/data-lake-store/). Przechowywania pozwala na przechowywanie danych dowolny rozmiar, typ i spożyciu szybkość działania i badawczych analizy. W tym momencie tworzenia i konfigurowania wyjściowe magazynu Lake danych jest obsługiwane tylko w portalu klasyczny Azure. Ponadto analizy strumieniu musi mieć możliwość dostępu do magazynu Lake danych. Szczegółowe informacje na temat w celu uzyskania podglądu sklepu Lake danych (w razie potrzeby) i autoryzacji omówiono w [artykule dane wyjściowe Lake danych](stream-analytics-data-lake-output.md).

### <a name="authorize-an-azure-data-lake-store"></a>Autoryzuj Lake magazynu danych platformy Azure

Po zaznaczeniu magazynowanie Lake danych jako wynik w portalu zarządzania Azure pojawi Aby autoryzować połączenie istniejący magazyn Lake danych.  

![Autoryzuj magazynu Lake danych](./media/stream-analytics-define-outputs/06-stream-analytics-define-outputs.png)  

Następnie wypełnij formularz właściwości wynik magazynu Lake danych, jak pokazano poniżej:

![Autoryzuj magazynu Lake danych](./media/stream-analytics-define-outputs/07-stream-analytics-define-outputs.png)  

Poniższa tabela zawiera listę nazw właściwości i opisami potrzebne przy tworzeniu wyjściowe magazynu Lake danych.

<table>
<tbody>
<tr>
<td><B>NAZWA WŁAŚCIWOŚCI</B></td>
<td><B>OPIS</B></td>
</tr>
<tr>
<td>Wyjściowy Alias</td>
<td>To jest używane w kwerendach w celu skierowania wyników kwerendy do tego magazynu Lake danych przyjazną nazwę.</td>
</tr>
<tr>
<td>Nazwa konta</td>
<td>Nazwa konta magazynowanie Lake danych, której wysyłasz danych wyjściowych. Zostanie wyświetlona z listy rozwijanej listy kont magazynu Lake danych, do których użytkownik zalogowany do portalu ma dostęp do.</td>
</tr>
<tr>
<td>Ścieżka prefiks wzorzec [<I>opcjonalne</I>]</td>
<td>Ścieżka pliku do zapisywania plików w ramach określonego konta magazynu Lake dane używane. <BR>{date} {time}<BR>Przykład 1: folder1/dzienniki / {dat} / {time}<BR>Przykład 2: folder1/dzienniki / {dat}</td>
</tr>
<tr>
<td>Format daty [<I>opcjonalne</I>]</td>
<td>Jeśli token Data jest używana w ścieżce prefiks, można wybrać format daty, w której są zorganizowane plików. Przykład: RRRR MM-DD</td>
</tr>
<tr>
<td>Format godziny [<I>opcjonalne</I>]</td>
<td>Jeśli token czasu jest używany w ścieżce prefiks, określ format czasu, w którym są zorganizowane plików. Jedyna obsługiwana wartość jest obecnie HH.</td>
</tr>
<tr>
<td>Format szeregowania zdarzenia</td>
<td>Format szeregowania danych wyjściowych. JSON, CSV i Avro są obsługiwane.</td>
</tr>
<tr>
<td>Kodowanie</td>
<td>Jeśli format CSV lub JSON, musi być określony kodowania. UTF-8 jest obsługiwany tylko format kodowania w tej chwili.</td>
</tr>
<tr>
<td>Ogranicznika</td>
<td>Stosuje się tylko do szeregowania CSV. Analizy strumieniu obsługuje wiele typowych ograniczniki szeregowania dane w formacie CSV. Obsługiwane są następujące wartości przecinek, średnik, odstęp, karta i pionowy pasek.</td>
</tr>
<tr>
<td>Formatowanie</td>
<td>Stosuje się tylko do szeregowania JSON. Linia ciągła rozdzielając określa formatowania wynik przez każdy obiekt JSON rozdzielając nowy wiersz. Tablica Określa, że dane wyjściowe mają zostać sformatowane jako tablicę obiektów JSON.</td>
</tr>
</tbody>
</table>

### <a name="renew-data-lake-store-authorization"></a>Odnawianie autoryzacji magazynu Lake danych

Konieczne będzie uwierzytelnienie konta magazynu Lake danych, jeśli hasło zostały zmienione, ponieważ utworzonej lub ostatnia uwierzytelniony zadania.

![Autoryzuj magazynu Lake danych](./media/stream-analytics-define-outputs/08-stream-analytics-define-outputs.png)  


## <a name="sql-database"></a>Baza danych SQL

[Bazy danych SQL Azure](https://azure.microsoft.com/services/sql-database/) może służyć jako wynik dla danych relacyjnych charakter lub aplikacje zależne od zawartości jest obsługiwany w relacyjnej bazie danych. Zadania analizy strumieniu zapisze do istniejącej tabeli w bazie danych SQL Azure.  Należy zauważyć, że schematu tabeli musi dokładnie odpowiadać pola i typy są dane wyjściowe zadania. Można również określić [Magazynu danych SQL Azure](https://azure.microsoft.com/documentation/services/sql-data-warehouse/) jako wynik za pośrednictwem bazy danych SQL dane wyjściowe opcji także (jest to funkcja podglądu). W poniższej tabeli wymieniono nazwy właściwości, a ich opisy związane z tworzeniem wyjściowe bazy danych SQL.

| Nazwa właściwości | Opis |
|---------------|-------------|
| Wyjściowy Alias | To przyjazną nazwę, która używane w kwerendach w celu skierowania wyniki kwerendy z tą bazą danych. |
| Bazy danych | Nazwa bazy danych, do której wysyłasz wydruku |
| Nazwa serwera | Nazwa serwera bazy danych SQL |
| Nazwa użytkownika | Nazwa użytkownika, który ma dostęp do zapisu w bazie danych |
| Hasło | Hasła w celu połączenia z bazą danych |
| Tabela | Nazwa tabeli, w której będą zapisywane dane wyjściowe. Nazwa tabeli jest uwzględniana wielkość liter i schemat tej tabeli powinny być zgodne dokładnie do liczby pól i ich typów generowany przez danych wyjściowych zadania. |

> [AZURE.NOTE] Obecnie oferująca bazy danych SQL Azure jest obsługiwana wyników zadania w analizy strumieniu. Maszyny wirtualnej Azure uruchomiony program SQL Server z bazą danych dołączone nie jest obsługiwane. To może ulec zmianie w przyszłych wersjach.

## <a name="blob-storage"></a>Magazyn obiektów blob

Magazyn obiektów blob oferuje efektywne pod względem kosztów i skalowalność rozwiązanie do przechowywania dużych ilości danych niestrukturalne w chmurze.  Wprowadzenie na magazyn obiektów Blob platformy Azure i jego zastosowania zobacz dokumentację w [sposób używania obiektów blob](../storage/storage-dotnet-how-to-use-blobs.md).

W poniższej tabeli wymieniono nazwy właściwości, a ich opisy związane z tworzeniem wyjściowe obiektów blob.

<table>
<tbody>
<tr>
<td>NAZWA WŁAŚCIWOŚCI</td>
<td>OPIS</td>
</tr>
<tr>
<td>Wyjściowy Alias</td>
<td>To jest używane w kwerendach w celu skierowania wyników kwerendy do tego magazynu obiektów blob przyjazną nazwę.</td>
</tr>
<tr>
<td>Konto miejsca do magazynowania</td>
<td>Nazwa konta miejsca do magazynowania, w której wysyłasz danych wyjściowych.</td>
</tr>
<tr>
<td>Klucz konta miejsca do magazynowania</td>
<td>Klucz tajny, skojarzone z kontem miejsca do magazynowania.</td>
</tr>
<tr>
<td>Kontener miejsca do magazynowania</td>
<td>Kontenery zapewniają logiczną grupę dla obiektów blob przechowywane w usłudze Microsoft Azure Blob. Gdy wysyłasz obiektów blob usługi obiektów Blob należy określić kontener dla tego obiektów blob.</td>
</tr>
<tr>
<td>Ścieżka prefiks wzorzec [opcjonalne]</td>
<td>Ścieżka pliku pisane z obiektami BLOB w określonym kontenerze.<BR>W ścieżce można określić częstotliwość, z jaką obiektów blob są zapisywane za pomocą jednego lub więcej wystąpień następujące zmienne 2:<BR>{date} {time}<BR>Przykład 1: cluster1/dzienniki / {dat} / {time}<BR>Przykład 2: cluster1/dzienniki / {dat}</td>
</tr>
<tr>
<td>Format daty [opcjonalne]</td>
<td>Jeśli token Data jest używana w ścieżce prefiks, można wybrać format daty, w której są zorganizowane plików. Przykład: RRRR MM-DD</td>
</tr>
<tr>
<td>Format godziny [opcjonalne]</td>
<td>Jeśli token czasu jest używany w ścieżce prefiks, określ format czasu, w którym są zorganizowane plików. Jedyna obsługiwana wartość jest obecnie HH.</td>
</tr>
<tr>
<td>Format szeregowania zdarzenia</td>
<td>Format szeregowania danych wyjściowych.  JSON, CSV i Avro są obsługiwane.</td>
</tr>
<tr>
<td>Kodowanie</td>
<td>Jeśli format CSV lub JSON, musi być określony kodowania. UTF-8 jest obsługiwany tylko format kodowania w tej chwili.</td>
</tr>
<tr>
<td>Ogranicznika</td>
<td>Stosuje się tylko do szeregowania CSV. Analizy strumieniu obsługuje wiele typowych ograniczniki szeregowania dane w formacie CSV. Obsługiwane są następujące wartości przecinek, średnik, odstęp, karta i pionowy pasek.</td>
</tr>
<tr>
<td>Formatowanie</td>
<td>Stosuje się tylko do szeregowania JSON. Linia ciągła oddzielone określa formatowania wynik przez każdy obiekt JSON, oddzielając je nowy wiersz. Tablica Określa, że dane wyjściowe mają zostać sformatowane jako tablicę obiektów JSON.</td>
</tr>
</tbody>
</table>

## <a name="event-hub"></a>Centrum zdarzenia

[Koncentratory zdarzenie](https://azure.microsoft.com/services/event-hubs/) jest wysoce skalowalna publikowanie Subskrybuj ingestor zdarzenia. Go może zbierać miliony wydarzeń na sekundę.  Jeden stosowania koncentratora zdarzenia jako wynik jest, gdy dane wyjściowe zadania analizy strumieniu będą wprowadzenia przesyłanie strumieniowe zadanie.

Istnieje kilka parametry, które są potrzebne do skonfigurowania strumienie danych Centrum zdarzenia jako wynik.

| Nazwa właściwości | Opis |
|---------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Wyjściowy Alias | To jest używane w kwerendach w celu skierowania wyników kwerendy do tego Centrum zdarzeń przyjazną nazwę. |
| Usługa Bus Namespace | Przestrzeń nazw Bus usługi jest kontenerem zestawu wiadomości podmiotów. Po utworzeniu nowego koncentratora zdarzenia utworzono nazw Bus usługi |
| Centrum zdarzenia | Nazwa danych wyjściowych Centrum zdarzenia |
| Nazwa zasady Centrum zdarzenia | Zasady dostępu do udostępnionego można tworzyć na karcie Konfigurowanie Centrum zdarzenia. Każdy zasady dostępu udostępnione będzie mieć nazwę uprawnienia, które można ustawić i klawisze dostępu |
| Klucz zasad Centrum zdarzenia | Klawisz dostępu udostępnione używane do uwierzytelnienia dostęp do nazw Bus usługi |
| Kolumny klucza partition [opcjonalne] | Ta kolumna zawiera klucz partition zdarzenia centrum danych wyjściowych. |
| Format szeregowania zdarzenia | Format szeregowania danych wyjściowych.  JSON, CSV i Avro są obsługiwane. |
| Kodowanie | CSV i JSON UTF-8 jest obecnie obsługiwany tylko format kodowania |
| Ogranicznika | Stosuje się tylko do szeregowania CSV. Analizy strumieniu obsługuje wiele typowych ograniczniki szeregowania danych w formacie CSV. Obsługiwane są następujące wartości przecinek, średnik, odstęp, karta i pionowy pasek. |
| Formatowanie | Dotyczy jedynie typ JSON. Linia ciągła oddzielone określa formatowania wynik przez każdy obiekt JSON, oddzielając je nowy wiersz. Tablica Określa, że dane wyjściowe mają zostać sformatowane jako tablicę obiektów JSON. |

## <a name="power-bi"></a>Power BI

[Power BI](https://powerbi.microsoft.com/) może służyć jako wynik dla zadania analizy strumieniu zapewniające zaawansowanych wizualizacji występowania wyniki analizy. Ta funkcja może służyć do działania pulpity nawigacyjne, Generowanie raportu i jednostki metryczne zmiennych raportowania.

### <a name="authorize-a-power-bi-account"></a>Autoryzuj konto usługi Power BI

1.  Po zaznaczeniu Power BI jako wynik w portalu zarządzania Azure pojawi się monit Aby autoryzować istniejącego użytkownika Power BI lub Utwórz nowe konto usługi Power BI.  

    ![Autoryzuj Power BI użytkownika](./media/stream-analytics-define-outputs/01-stream-analytics-define-outputs.png)  

2.  Tworzenie nowego konta, jeśli nie jeszcze istnieje, a następnie kliknij przycisk Autoryzuj teraz.  Ekran jak na następującym przykładzie przedstawiono.  

    ![Konto Azure Power BI](./media/stream-analytics-define-outputs/02-stream-analytics-define-outputs.png)  

3.  W tym kroku należy podać konto służbowe autoryzowanie wynik Power BI. Jeśli użytkownik nie już korzystasz z usługi Power BI, wybierz pozycję Zaloguj teraz. Konto służbowe, którego używasz usługi Power BI mogą być inne niż konto Azure subskrypcji, które użytkownik jest zalogowany przy użyciu.

### <a name="configure-the-power-bi-output-properties"></a>Konfigurowanie właściwości dane wyjściowe Power BI

Po utworzeniu konta usługi Power BI uwierzytelniony można konfigurować właściwości dla danych wyjściowych Power BI. W poniższej tabeli jest listę nazw właściwości i ich opisy, aby skonfigurować danych wyjściowych Power BI.

| Nazwa właściwości | Opis |
|---------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Wyjściowy Alias | To jest używane w kwerendach w celu skierowania wyników kwerendy do tego raportu PowerBI przyjazną nazwę. |
| Grupa obszaru roboczego | Aby włączyć udostępnianie danych innym użytkownikom usługi Power BI można zaznaczyć grupy wewnątrz konta usługi Power BI lub wybierz pozycję "Moje obszaru roboczego", jeśli nie chcesz zapisywać do grupy.  Aktualizowanie istniejącej grupy wymaga odnawiania uwierzytelniania usługi Power BI. | 
| Nazwa zestawu danych | Podaj nazwę zestawu danych wymaganego dla usługi Power BI danych wyjściowych |
| Nazwa tabeli | Podaj nazwę tabeli w obszarze zestaw danych wyjściowych Power BI. Obecnie Power BI wyniki analizy strumieniu zadania może mieć tylko jedną tabelę w zestawie danych |

Aby samouczek konfigurowania usługi Power BI dane wyjściowe i pulpitów nawigacyjnych zobacz artykuł [analizy strumieniu Azure i Power BI](stream-analytics-power-bi-dashboard.md) .

> [AZURE.NOTE] Nie jawnie Tworzenie zestawu danych i tabeli na pulpicie nawigacyjnym Power BI. Zestaw danych i tabeli zostanie wypełnione automatycznie podczas rozpoczęcia zadania i zadania Rozpoczęcie pompującego wyjściowego do usługi Power BI. Należy zauważyć, że jeśli kwerendy zadania nie generuje żadnych wyników, zestawu danych i tabeli nie zostanie utworzona. Pamiętaj również, że jeśli Power BI ma już zestawu danych i tabeli o takiej samej nazwie jak opisane w tym zadaniu analizy strumieniu, istniejące dane zostaną zastąpione.

### <a name="renew-power-bi-authorization"></a>Odnawianie Power BI autoryzacji

Konieczne będzie uwierzytelnienie konta usługi Power BI, jeśli hasło zostały zmienione, ponieważ utworzonej lub ostatnia uwierzytelniony zadania. Jeśli uwierzytelnianie wieloskładnikowe (MFA) jest skonfigurowana na dzierżawy usługi Azure Active Directory (AAD), należy także odnowić autoryzacji Power BI co 2 tygodnie. Objawem tego problemu jest nie wyjścia zadania i "uwierzytelniania użytkownika błędu" dzienniki operacji:

  ![Błąd tokenu Power BI odświeżania](./media/stream-analytics-define-outputs/03-stream-analytics-define-outputs.png)  

Aby rozwiązać ten problem, zatrzymanie uruchomionego zadania, a następnie przejdź do danych wyjściowych Power BI.  Kliknij łącze "Odnów autoryzacji", a następnie uruchom ponownie zadanie z zatrzymana podczas ostatniego Aby uniknąć utraty danych.

  ![Power BI odnowić autoryzacji](./media/stream-analytics-define-outputs/04-stream-analytics-define-outputs.png)  

## <a name="table-storage"></a>Magazyn tabel

[Magazyn tabel platformy Azure](../storage/storage-introduction.md) oferuje miejscem do magazynowania wysokiej dostępności, znacznym stopniu skalowalna, tak, aby aplikacja automatycznie można skalować zapotrzebowania użytkownika. Magazyn tabel jest magazyn atrybutu klucza NoSQL firmy Microsoft nich mogą korzystać z danych strukturalnych z ograniczeniami mniej w schemacie. Magazyn tabel platformy Azure może służyć do przechowywania danych dla trwałe i efektywne pobieranie.

W poniższej tabeli wymieniono nazwy właściwości, a ich opisy związane z tworzeniem wyjściowe tabeli.

| Nazwa właściwości | Opis |
|---------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Wyjściowy Alias | To jest używane w kwerendach w celu skierowania wyników kwerendy do tego magazynu tabeli przyjazną nazwę. |
| Konto miejsca do magazynowania | Nazwa konta miejsca do magazynowania, w której wysyłasz danych wyjściowych. |
| Klucz konta miejsca do magazynowania | Klawisz dostępu skojarzone z kontem miejsca do magazynowania. |
| Nazwa tabeli | Nazwa tabeli. Jeśli nie istnieje będzie utworzyć tabeli. |
| Klucz partition | Nazwa kolumny wyników zawierające klucz partycją. Klucz partycją jest unikatowy identyfikator partycją w danej tabeli będącej pierwsza część jednostki klucza podstawowego. Jest to wartość ciągu, który może być maksymalnie 1 KB. |
| Klucz wiersza | Nazwa kolumny wyników zawierające klucz wiersza. Klucz wiersza jest unikatowy identyfikator obiektu w danym partycją. Stanowi on drugiej części jednostki klucza podstawowego. Klucz wiersza jest wartość ciągu, który może być maksymalnie 1 KB. |
| Rozmiar partii | Liczba rekordów dla operacji partię. Zazwyczaj wartość domyślna to wystarczające dla większości zadań, zapoznaj się z [Specyfikacja operacja partii tabeli](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tablebatchoperation.aspx) , aby uzyskać więcej informacji na temat modyfikowania to ustawienie. |

## <a name="service-bus-queues"></a>Usługa Bus kolejki

[Kolejek Bus usług](https://msdn.microsoft.com/library/azure/hh367516.aspx) oferują pierwszego w dostarczenia wiadomości pierwszego się (FIFO) dla jednego lub więcej konkurencyjnych konsumentów. Zazwyczaj wiadomości mają odebrana i przetwarzana przez odbiorców w kolejności czasowy, w którym zostały dodane do kolejki, a każda wiadomość jest odbierane i przetwarzane przez konsumenta tylko jedną wiadomość.

Poniższa tabela zawiera listę nazw właściwości i ich opis tworzenia kolejki dane wyjściowe.

| Nazwa właściwości | Opis |
|----------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Wyjściowy Alias | To jest używane w kwerendach w celu skierowania wyników kwerendy do niej Bus usługi przyjazną nazwę. |
| Usługa Bus Namespace | Przestrzeń nazw Bus usługi jest kontenerem zestawu wiadomości podmiotów. |
| Nazwa kolejki | Nazwa kolejki Bus usługi. |
| Nazwa zasady kolejki | Po utworzeniu kolejki można także tworzyć zasady dostępu udostępnionym na karcie Konfigurowanie kolejki. Każdy zasady dostępu udostępnione będzie mieć nazwę uprawnienia, które można ustawić i klawisze dostępu. |
| Klucz zasad w kolejce | Klawisz dostępu udostępnione używane do uwierzytelnienia dostęp do nazw Bus usługi |
| Format szeregowania zdarzenia | Format szeregowania danych wyjściowych.  JSON, CSV i Avro są obsługiwane. |
| Kodowanie | CSV i JSON UTF-8 jest obecnie obsługiwany tylko format kodowania |
| Ogranicznika | Stosuje się tylko do szeregowania CSV. Analizy strumieniu obsługuje wiele typowych ograniczniki szeregowania danych w formacie CSV. Obsługiwane są następujące wartości przecinek, średnik, odstęp, karta i pionowy pasek. |
| Formatowanie | Dotyczy jedynie typ JSON. Linia ciągła oddzielone określa formatowania wynik przez każdy obiekt JSON, oddzielając je nowy wiersz. Tablica Określa, że dane wyjściowe mają zostać sformatowane jako tablicę obiektów JSON. |

## <a name="service-bus-topics"></a>Usługa Bus tematów

Gdy kolejek Bus usług zapewnić metodę komunikacji jeden-do-jednego od nadawcy, odbiorcy, [Usługa Bus](https://msdn.microsoft.com/library/azure/hh367516.aspx) tematach formularzem jeden do wielu komunikacji.

W poniższej tabeli wymieniono nazwy właściwości, a ich opisy związane z tworzeniem wyjściowe tabeli.

| Nazwa właściwości | Opis |
|----------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Wyjściowy Alias | To jest używane w kwerendach w celu skierowania wyników kwerendy do tego tematu Bus usługi przyjazną nazwę. |
| Usługa Bus Namespace | Przestrzeń nazw Bus usługi jest kontenerem zestawu wiadomości podmiotów. Po utworzeniu nowego koncentratora zdarzenia utworzono nazw Bus usługi |
| Nazwa tematu | Tematy są wiadomości jednostki, podobnie jak koncentratory zdarzeń i kolejek. Są one przeznaczone do zbierania strumieni zdarzeń z wielu różnych urządzeń i usług. Po utworzeniu tematu, wyznaczona również określonej nazwy. Wiadomości wysyłane do tematu nie będą dostępne, chyba że jest tworzony subskrypcji, to upewnij się, istnieją jedną lub więcej subskrypcji w obszarze tematu |
| Nazwa zasady tematu | Po utworzeniu tematu można także tworzyć zasady dostępu udostępnionym na karcie Konfigurowanie tematu. Każdy zasady dostępu udostępnione będzie mieć nazwę uprawnienia, które można ustawić i klawisze dostępu |
| Klucz zasad tematu | Klawisz dostępu udostępnione używane do uwierzytelnienia dostęp do nazw Bus usługi |
| Format szeregowania zdarzenia | Format szeregowania danych wyjściowych.  JSON, CSV i Avro są obsługiwane. |
| Kodowanie | Jeśli format CSV lub JSON, musi być określony kodowania. Obecnie UTF-8 jest obsługiwany tylko format kodowania |
| Ogranicznika | Stosuje się tylko do szeregowania CSV. Analizy strumieniu obsługuje wiele typowych ograniczniki szeregowania danych w formacie CSV. Obsługiwane są następujące wartości przecinek, średnik, odstęp, karta i pionowy pasek. |

## <a name="documentdb"></a>DocumentDB

[Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) jest w pełni zarządzane NoSQL dokumentu bazy danych usługi, która oferuje kwerendy i transakcji przez dane schematu, przewidywalne i niezawodne wydajności i szybki rozwój.

W poniższej tabeli zawiera listę nazw właściwości i opisami związane z tworzeniem wynik DocumentDB.

<table>
<tbody>
<tr>
<td>NAZWA WŁAŚCIWOŚCI</td>
<td>OPIS</td>
</tr>
<tr>
<td>Nazwa konta</td>
<td>Nazwa konta DocumentDB.  Może to być również punkt końcowy dla konta.</td>
</tr>
<tr>
<td>Klucz konta</td>
<td>Klawisz dostępu udostępnionej dla konta DocumentDB.</td>
</tr>
<tr>
<td>Bazy danych</td>
<td>Nazwa bazy danych DocumentDB.</td>
</tr>
<tr>
<td>Wzorca nazwy kolekcji</td>
<td>Nazwa zbioru wzorzec dla zbiorów ma być używany. Format nazwy zbioru może być wykonane przy użyciu tokenu opcjonalne {partition}, gdzie partycje zaczynać się od 0.<BR>Np. Następujących typów są prawidłowe wartości wejściowych:<BR>MyCollection {partition}<BR>MyCollection<BR>Należy zauważyć, że zbiory, musi istnieć zadanie analizy strumieniu jest uruchomiona i nie zostanie utworzony automatycznie.</td>
</tr>
<tr>
<td>Klucz partition</td>
<td>Nazwa pola wydarzeń dane wyjściowe używane do określania klucza do podziału dane wyjściowe między zbiorami.</td>
</tr>
<tr>
<td>Identyfikator dokumentu</td>
<td>Nazwa pola wydarzeń dane wyjściowe używane do określania klucza podstawowego, które Wstawianie lub aktualizowanie operacji są oparte na.</td>
</tr>
</tbody>
</table>


## <a name="get-help"></a>Uzyskiwanie pomocy
Aby uzyskać dodatkową pomoc spróbuj nasze [forum analizy strumieniu Azure](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Następne kroki
Zostały wprowadzone do analizy strumieniu, zarządzanych usług do przesyłania strumieniowego analizy danych z Internetu rzeczy. Aby dowiedzieć się więcej o tej usłudze, zobacz:

- [Wprowadzenie do korzystania z analizy strumieniu Azure](stream-analytics-get-started.md)
- [Skalowanie Azure analizy strumieniu zadania](stream-analytics-scale-jobs.md)
- [Dokumentacja języka kwerend analizy strumieniu Azure](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Strumień Azure analizy zarządzania pozostałych interfejsu API odwołania](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-get-started.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
