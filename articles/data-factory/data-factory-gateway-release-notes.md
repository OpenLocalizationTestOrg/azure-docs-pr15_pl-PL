<properties 
    pageTitle="Informacje o wersji dla bramy zarządzania danymi | Factory Azure danych" 
    description="Informacje o wersji tory bramy zarządzania danymi" 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/26/2016" 
    ms.author="spelluru"/>

# <a name="release-notes-for-data-management-gateway"></a>Informacje o wersji dla bramy zarządzania danymi
Jedną z problemach integracji nowoczesny danych jest bezproblemowo przenoszenie danych do i z lokalnego do chmury. Factory danych powoduje, że integracja Bezproblemowa z bramy zarządzania danymi, czyli agenta zainstalowanie lokalnego umożliwiające hybrydowych przenoszenia danych.

Zobacz następujące artykuły, aby uzyskać szczegółowe informacje na temat brama zarządzania danymi i jak używać tej funkcji: 

- [Brama zarządzania danymi](data-factory-data-management-gateway.md)
- [Przenoszenie danych między lokalnym i w chmurze za pomocą Factory danych Azure](data-factory-move-data-between-onprem-and-cloud.md) 

## <a name="current-version-2260721"></a>Bieżąca wersja (2.2.6072.1)

- Obsługa ustawienia serwera proxy HTTP dla bramy za pomocą Menedżera konfiguracji bramy. Jeśli skonfigurowane, obiektów Blob platformy Azure, tabeli Azure Lake danych Azure i DB dokumentu są dostępne za pośrednictwem serwera proxy HTTP.
- Obsługa nagłówek obsługi TextFormat podczas kopiowania danych z obiektów Blob platformy Azure, Magazyn Lake danych Azure, System plików lokalnego i HDFS lokalnego.
- Kopiowanie danych z dołączanie obiektów Blob i obiektów Blob strony wraz z już obsługiwane Blob blok obsługuje.
- Wprowadza nowy stan bramy **Online (tylko)**, która wskazuje, że główne funkcje bramy działa z wyjątkiem obsługę operacji interakcyjnych kreatora kopiowania.
- Zwiększa niezawodności rejestracji bramy przy użyciu klucza rejestru.

## <a name="earlier-versions"></a>Wcześniejsze wersje

## <a name="2160401"></a>2.1.6040.1

- Sterownik DB2 będzie obejmować pakiet instalacyjny bramy teraz. Nie musisz zainstalować oddzielnie. 
- Sterownik DB2 obsługuje teraz z/OS DB2 dla i (AS / 400) wraz z już obsługiwane platformy (Linux, Unix i systemu Windows). 
- Obsługa przy użyciu DocumentDB jako źródła lub miejsca docelowego dla lokalnego magazynów
- Kopiowanie danych z/do zimnej hot obsługuje blob miejsca do magazynowania wraz z konta już obsługiwane ogólnego przeznaczenia przestrzeni dyskowej. 
- Umożliwia nawiązywanie połączenia z lokalnego programu SQL Server za pomocą bramy z uprawnieniami logowania zdalnego.  

## <a name="2060131"></a>2.0.6013.1

- Możesz wybrać języka/kultury ma być używany przez bramę podczas instalacji ręcznej.
- Brama nie działa zgodnie z oczekiwaniami, możesz wysłać dzienniki bramy ostatnich siedmiu dni do firmy Microsoft ułatwiające rozwiązywanie problemu. Jeśli bramy nie jest podłączony do usługi w chmurze, możesz zapisać i archiwizowanie dzienników bramy.  
- Ulepszenia interfejsu użytkownika Menedżer konfiguracji bramy:
    - Wyróżniać stan bramy na karcie Narzędzia główne.
    - Formanty zreorganizowanych i uproszczone.
- Dane można kopiować z magazynu przy użyciu [kodu bezpłatne narzędzia do kopiowania Podgląd](data-factory-copy-data-wizard-tutorial.md). Zobacz zazwyczaj [Umieszczane kopiowanie](data-factory-copy-activity-performance.md#staged-copy) szczegółowe informacje na temat tej funkcji. 
- Za pomocą bramy zarządzania danymi ingress danych bezpośrednio z bazy danych programu SQL Server lokalnego do nauki maszynowego Azure.
- Ulepszenia dotyczące wydajności
    - Zwiększanie wydajności na wyświetlanie schematu i podgląd przed programu SQL Server w narzędziu Podgląd kopii wolny kodu.



## <a name="11259531"></a>1.12.5953.1
- Poprawki

## <a name="11159181"></a>1.11.5918.1

- Maksymalny rozmiar dziennika zdarzeń bramy został zwiększony od 1 MB do 40 MB.
- Zostanie wyświetlone okno dialogowe z ostrzeżeniem, w przypadku ponownego uruchomienia jest potrzebna podczas automatycznej aktualizacji bramy. Możesz ponownie uruchomić prawo następnie lub nowszym. 
- W przypadku automatycznej aktualizacji nie powiedzie się, Instalatora bramy prób, automatyczne aktualizowanie trzy razy w maksimum.
- Ulepszenia dotyczące wydajności
    - Zwiększyć wydajność ładowania dużych tabel z lokalnego serwera w scenariuszu wolny kod Kopiuj.
- Poprawki

## <a name="11058921"></a>1.10.5892.1

- Ulepszenia dotyczące wydajności
- Poprawki

## <a name="1958652"></a>1.9.5865.2

- Funkcja aktualizacji automatyczne zero dotyku
- Nowa ikona obszar powiadomień z wskaźników stanu bramy
- Możliwość "Aktualizuj teraz" w kliencie
- Możliwość ustawienia czas harmonogram aktualizacji
- Przełączanie automatycznej aktualizacji włączania/wyłączania skrypt programu PowerShell
- Obsługa formatu JSON  
- Ulepszenia dotyczące wydajności
- Poprawki

## <a name="1858221"></a>1.8.5822.1

- Ulepsz proces rozwiązywania problemów
- Ulepszenia dotyczące wydajności
- Poprawki

### <a name="1757951"></a>1.7.5795.1

- Ulepszenia dotyczące wydajności
- Poprawki

### <a name="1757641"></a>1.7.5764.1

- Ulepszenia dotyczące wydajności
- Poprawki

### <a name="1657351"></a>1.6.5735.1

- Pomoc techniczna HDFS źródła Sink lokalnego
- Ulepszenia dotyczące wydajności
- Poprawki

### <a name="1656961"></a>1.6.5696.1

- Ulepszenia dotyczące wydajności
- Poprawki

### <a name="1656761"></a>1.6.5676.1

- Narzędzia diagnostyczne obsługi w Menedżerze konfiguracji
- Obsługa kolumn tabeli w przypadku tabelarycznych źródeł danych Factory danych Azure
- Obsługa SQL DW Azure danych Factory
- Obsługa Reclusive w BlobSource i FileSource Factory Azure danych
- Obsługa w BlobSink i FileSink z kopią binarne Factory Azure danych CopyBehavior — MergeFiles, PreserveHierarchy i FlattenHierarchy
- Obsługuje raportowanie postępu Factory danych Azure aktywności Kopiuj
- Sprawdzanie poprawności łączności źródła danych pomocy technicznej dla Factory Azure danych
- Poprawki


### <a name="1656721"></a>1.6.5672.1

- Obsługa Factory danych Azure Nazwa tabeli dla źródła danych ODBC
- Ulepszenia dotyczące wydajności
- Poprawki

### <a name="1656581"></a>1.6.5658.1

- Plik pomocy technicznej zlew dla Factory Azure danych
- Obsługa w kopii binarne Factory danych Azure zachowania hierarchii
- Obsługa kopii aktywności Idempotency Factory Azure danych
- Poprawki

### <a name="1656401"></a>1.6.5640.1

- Obsługa 3 większą liczbą źródeł danych Azure Factory danych (ODBC, OData, HDFS)
- Obsługa znak cudzysłowu w parsera csv Factory danych Azure
- Obsługa kompresji (BZip2)
- Poprawki

### <a name="1556121"></a>1.5.5612.1

- Obsługa Azure danych Factory (MySQL, PostgreSQL DB2, Teradata i Sybase) pięć relacyjnych baz danych
- Obsługa kompresji (Gzip i Deflate)
- Ulepszenia dotyczące wydajności
- Poprawki


### <a name="1455491"></a>1.4.5549.1

- Dodawanie obsługi źródła danych programu Oracle Factory danych Azure
- Ulepszenia dotyczące wydajności
- Poprawki

### <a name="1454921"></a>1.4.5492.1

- Ujednolicony binarny, która obsługuje usługi Microsoft Azure danych Factory jak Office 365 Power BI
- Uściślanie procesu konfiguracji interfejsu użytkownika i rejestracji
- Azure Factory danych — Azure Ingress i wyjściowego pomocy technicznej dostępne dla źródła danych programu SQL Server

### <a name="1253031"></a>1.2.5303.1

-   Rozwiązać problem przekroczenia limitu czasu do obsługi więcej połączenia ze źródłami danych czasochłonne. 
    
### <a name="1155268"></a>1.1.5526.8

- Wymaga .NET Framework 4.5.1 jako wstępny podczas instalacji.

### <a name="1051442"></a>1.0.5144.2

- Nie zmiany, które wpływają na scenariusze Azure danych Factory. 
