<properties 
    pageTitle="Pytania DocumentDB bazy danych — często zadawane pytania | Microsoft Azure" 
    description="Poznaj odpowiedzi na często zadawane pytania dotyczące Azure DocumentDB usługi NoSQL dokumentu bazy danych dla JSON. Odpowiedzi na pytania bazy danych o pojemności, poziomów wydajności oraz skalowanie." 
    keywords="Pytania bazy danych — często zadawane pytania, documentdb, azure, platformy Microsoft azure"
    services="documentdb" 
    authors="mimig1" 
    manager="jhubbard" 
    editor="monicar" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/03/2016" 
    ms.author="mimig"/>


#<a name="frequently-asked-questions-about-documentdb"></a>Często zadawane pytania dotyczące DocumentDB

## <a name="database-questions-about-microsoft-azure-documentdb-fundamentals"></a>Pytania bazy danych Microsoft Azure DocumentDB podstawy

### <a name="what-is-microsoft-azure-documentdb"></a>Co to jest Microsoft Azure DocumentDB? 
Microsoft Azure DocumentDB jest ogromną szybkie, skali planety NoSQL dokumentu bazy danych — jako a usługa, która oferuje zaawansowanych kwerend na dane schematu, zapewnia wydajność można konfigurować i niezawodne i umożliwia szybki rozwój przez zarządzanych platformy obsługiwane przez power osiągnięcia pakietu Microsoft Azure. DocumentDB to właściwe rozwiązanie dla urządzeń przenośnych, gier sieci web i aplikacji IoT przewidywalne przepustowość, wysoka dostępność, krótki czas oczekiwania i modelu danych bez schematu są kluczy wymagania. DocumentDB zapewnia elastyczność schematu i sformatowany indeksowania przy użyciu modelu danych JSON natywne i zawiera wiele dokumentów obsługę transakcji zintegrowane JavaScript.  
  
Więcej pytania bazy danych, odpowiedzi i instrukcje dotyczące instalowania i przy użyciu tej usługi zobacz [stronę dokumentacji DocumentDB](https://azure.microsoft.com/documentation/services/documentdb/).

### <a name="what-kind-of-database-is-documentdb"></a>Jakiego rodzaju bazy danych jest DocumentDB?
DocumentDB jest NoSQL dokumentu orientacji bazie danych, które są przechowywane dane w formacie JSON.  DocumentDB obsługuje struktury zagnieżdżone, włączenie contained danych, które można wyszukiwać za pomocą zaawansowanych DocumentDB [gramatyki zapytania SQL](documentdb-sql-query.md). DocumentDB zawiera wysokiej wydajności transakcji przetwarzania po stronie serwera JavaScript za pośrednictwem [przechowywanych procedur, wyzwalaczy i funkcji zdefiniowanych przez użytkownika](documentdb-programming.md). Bazy danych obsługuje także Deweloper poziomy ustawiane spójności z skojarzone [poziomy wydajności](documentdb-performance-levels.md).
 
### <a name="do-documentdb-databases-have-tables-like-a-relational-database-rdbms"></a>DocumentDB baz danych mają tabel, takich jak relacyjnej bazy danych (RDBMS)?
Nie, DocumentDB są przechowywane dane w zbiorach dokumentów JSON.  Aby uzyskać informacje o zasobach DocumentDB zobacz [DocumentDB zasobów modelu i pojęcia](documentdb-resources.md). Aby uzyskać więcej informacji na temat sposobu rozwiązania NoSQL, takich jak DocumentDB różnią się od relacyjnych rozwiązań zobacz [w porównaniu z NoSQL SQL](documentdb-nosql-vs-sql.md).

### <a name="do-documentdb-databases-support-schema-free-data"></a>DocumentDB baz danych obsługują dane schematu?
Tak, DocumentDB pozwala aplikacjom przechowywać dowolnego dokumentów JSON bez definicja schematu lub wskazówki. Dane są od razu dostępne dla kwerendy za pośrednictwem interfejsu zapytania DocumentDB SQL.   

### <a name="does-documentdb-support-acid-transactions"></a>DocumentDB obsługuje kwasu transakcje?
Tak, DocumentDB obsługuje transakcje pomiędzy dokumentami wyrażoną w postaci procedury JavaScript przechowywanych i wyzwalaczy. Transakcje są występujące w jedną partycją w obrębie każdego zbioru i wykonywać kwasu znaczenie jak wszystko lub nic samodzielnie z innymi jednocześnie Wykonywanie żądań kod i użytkownika.  Jeśli wyjątki są generowane przez wykonanie po stronie serwera kodu JavaScript aplikacji, cała transakcja jest wycofać. Aby uzyskać więcej informacji na temat transakcji zobacz [Transakcje bazy danych programu](documentdb-programming.md#database-program-transactions).

### <a name="what-are-the-typical-use-cases-for-documentdb"></a>Jakie są typowe zastosowanie sprawy DocumentDB?  
DocumentDB jest dobrym rozwiązaniem dla nowych gier urządzeń przenośnych, sieci web, a aplikacje IoT miejsce, w którym automatycznego skala przewidywalne wydajność, szybkie kolejności czas reakcji milisekund i możliwości kwerend na dane schematu jest ważne. DocumentDB pozwala na szybkie rozwoju i pomocniczych ciągły iteracji modeli danych aplikacji. Aplikacje, które zarządzania zawartością użytkownika wygenerowane i dane są [Typowe przypadki użycia dla DocumentDB](documentdb-use-cases.md).  

### <a name="how-does-documentdb-offer-predictable-performance"></a>Jak DocumentDB oferuje przewidywalne wydajności?
[Jednostka żądania](documentdb-request-units.md) jest miarą przepustowość w DocumentDB. 1 RU odpowiada przepustowości pobieranie dokumentu 1KB. Każdej operacji w DocumentDB, w tym Odczyt, zapis, zapytania SQL i wykonania procedura składowana ma wartość RU deterministyczne, oparte na przepustowość wymagany do wykonania tej operacji. Zamiast Myślę o Procesora, Jo i pamięci i mają wpływ na przepustowość Twojej aplikacji, możesz myśleć w jednym pomiarowe RU.

Każda z kolekcji DocumentDB można zarezerwować o ustanawianie przepustowości pod względem RUs przepustowość na sekundę. W przypadku aplikacji wszelkie skali testu wydajności indywidualne żądania do pomiaru ich wartości RU i zbiory należy obsługiwać liczba jednostek żądania dla wszystkich żądań. Możesz również rozbudowy lub skali przepustowość Twojej kolekcji jako na potrzeby usługi evolve aplikacji. Aby uzyskać więcej informacji o jednostkach żądania i uzyskać pomoc Określanie kolekcji musi, przeczytaj [Zarządzanie wydajnością i pojemnością](documentdb-manage.md) i użyj [Kalkulator przepustowości](https://www.documentdb.com/capacityplanner). 

### <a name="is-documentdb-hipaa-compliant"></a>DocumentDB HIPAA jest zgodny z?
Tak, DocumentDB jest zgodny z HIPAA. HIPAA ustala wymagania dotyczące używania ujawnienia i ochrony informacji pojedynczo identyfikowalne zdrowia. Aby uzyskać więcej informacji zobacz [Centrum zaufania programu Microsoft](https://www.microsoft.com/en-us/TrustCenter/Compliance/HIPAA).

### <a name="what-are-the-storage-limits-of-documentdb"></a>Co to są limitów miejsca do magazynowania DocumentDB? 
Nie ma żadnych teoretyczna limitu łączną liczbę w DocumentDB można przechowywać zbioru danych. Jeśli chcesz przechowywać ponad 250 GB danych w jednym zbiorze, zapoznaj się [kontakt z pomocą techniczną](documentdb-increase-limits.md) , aby mieć przydziału konta zwiększone. 

### <a name="what-are-the-throughput-limits-of-documentdb"></a>Jakie są ograniczenia przepustowości z DocumentDB? 
Jeśli z pracą mogą być rozpowszechniane około równomiernie między wystarczającej liczby klawiszy partycją jest nieograniczony teoretyczna łączną liczbę przepustowość obsługujące zbioru w DocumentDB. Jeśli chcesz przekracza żądanie 250 000 jednostek na sekundę każdego zbioru lub konta, zapoznaj się [kontakt z pomocą techniczną](documentdb-increase-limits.md) , aby mieć przydziału konta zwiększone. 

### <a name="how-much-does-microsoft-azure-documentdb-cost"></a>Ile kosztuje usługa Microsoft Azure DocumentDB?
Zajrzyj do strony [szczegółowe informacje o cenach DocumentDB](https://azure.microsoft.com/pricing/details/documentdb/) , aby uzyskać szczegółowe informacje. Opłaty za zużycie DocumentDB są uzależnione od liczba zbiorów w użyciu, liczbę godzin kolekcje były w trybie online, oraz zużyte miejsca do magazynowania i ustanawianie przepustowości dla każdego zbioru. 

### <a name="is-there-a-free-account-available"></a>Dostępna jest bezpłatne konto?
Jeśli jesteś nowym użytkownikiem Azure, można Załóż [bezpłatne konto Azure](https://azure.microsoft.com/free/), co pozwala na 30 dni i 200 zł do wypróbowania wszystkich usług Azure. Lub, jeśli masz subskrypcję usługi programu Visual Studio, kwalifikujesz się do [150 zł w bezpłatnej środków Azure miesięcznie](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) używać na wszystkie inne usługi Azure.  

### <a name="how-can-i-get-additional-help-with-documentdb"></a>Jak można uzyskać dodatkową pomoc dotyczącą DocumentDB?
Jeśli potrzebujesz pomocy dowolnego bliższy kontakt z nami na [Przepełnienie stosu](http://stackoverflow.com/questions/tagged/azure-documentdb), [Fora deweloperów MSDN DocumentDB Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureDocumentDB)lub Planowanie [rozmawianie 1:1 z zespół inżynierów DocumentDB](http://www.askdocdb.com/). Aby być na bieżąco najnowsze DocumentDB i funkcji, postępuj zgodnie z nami na [serwisu Twitter](https://twitter.com/DocumentDB).

## <a name="set-up-microsoft-azure-documentdb"></a>Konfigurowanie programu Microsoft Azure DocumentDB

### <a name="how-do-i-sign-up-for-microsoft-azure-documentdb"></a>Jak zapisów dla programu Microsoft Azure DocumentDB?
Microsoft Azure DocumentDB znajduje się w [Azure Portal][azure-portal].  Musisz najpierw utwórz konto, dla subskrypcji Microsoft Azure.  Po utworzeniu konta w subskrypcji Microsoft Azure, możesz dodać konto DocumentDB do subskrypcji usługi Azure. Aby uzyskać instrukcje dotyczące dodawania konta DocumentDB zobacz [Tworzenie konta DocumentDB bazy danych](documentdb-create-account.md).   

### <a name="what-is-a-master-key"></a>Co to jest klucz główny?
Klucz główny jest tokenu zabezpieczającego, aby uzyskać dostęp do wszystkich zasobów dla konta. Osoby, które mają klucz mają uprawnienia odczytu i zapisu do wszystkich zasobów w oknie konta bazy danych. Rozpowszechnianie kluczy głównych, należy zachować ostrożność. Klucz podstawowy wzorca i pomocniczą klucza głównego są dostępne w karta **klawiszy ** [Azure Portal][azure-portal]. Aby uzyskać więcej informacji na temat kluczy Zobacz [widoku, kopiowanie i klawisze dostępu wyniku](documentdb-manage-account.md#keys).

### <a name="how-do-i-create-a-database"></a>Jak utworzyć bazę danych?
Można utworzyć bazy danych za pomocą [Azure Portal]() , zgodnie z opisem w [Utwórz bazę danych DocumentDB](documentdb-create-database.md), jednej z [SDK DocumentDB](documentdb-sdk-dotnet.md)lub za pośrednictwem [Interfejsów API pozostałych](https://msdn.microsoft.com/library/azure/dn781481.aspx).  

### <a name="what-is-a-collection"></a>Co to jest zbiór?
Kolekcja jest kontenerem JSON dokumentów i skojarzone logiki kodu JavaScript w aplikacji. Kolekcja jest rozliczana jednostką, której [Koszt](documentdb-performance-levels.md) zależy od przepustowość i storaged używane. Kolekcje może obejmować jeden lub więcej partycje serwerów i można skalować obsługę praktycznie nieograniczoną ilości miejsca do magazynowania lub przepustowość.

Kolekcje są również podmioty rozliczeń dla DocumentDB. Każda z kolekcji jest wystawiona, co godzinę na podstawie ustanawianie przepustowość i miejsca do magazynowania. Aby uzyskać więcej informacji zobacz [DocumentDB ceny](https://azure.microsoft.com/pricing/details/documentdb/).  

### <a name="how-do-i-set-up-users-and-permissions"></a>Jak skonfigurować użytkownicy i uprawnienia?
Możesz utworzyć użytkownicy i uprawnienia przy użyciu jednego [SDK DocumentDB](documentdb-sdk-dotnet.md) lub za pośrednictwem [Interfejsów API pozostałych](https://msdn.microsoft.com/library/azure/dn781481.aspx).   

## <a name="database-questions-about-developing-against-microsoft-azure-documentdb"></a>Baza danych pytania dotyczące opracowywania przed Microsoft Azure DocumentDB

### <a name="how-to-do-i-start-developing-against-documentdb"></a>W jaki sposób należy rozpocząć opracowywania przed DocumentDB?
[SDK](documentdb-sdk-dotnet.md) są dostępne dla .NET, Python Node.js, JavaScript i Java.  Ponadto można używać [RESTful API HTTP](https://msdn.microsoft.com/library/azure/dn781481.aspx) do współdziałania z zasobami DocumentDB z różnych platform i języków. 

Próbki do DocumentDB [.NET](documentdb-dotnet-samples.md), [języka Java](https://github.com/Azure/azure-documentdb-java), [Node.js](documentdb-nodejs-samples.md)oraz SDK [Python](documentdb-python-samples.md) są dostępne w GitHub.

### <a name="does-documentdb-support-sql"></a>DocumentDB obsługuje SQL?
Język kwerend DocumentDB SQL jest rozszerzonego podzestaw funkcji kwerendy obsługiwanych przez SQL. Język kwerend DocumentDB SQL zawiera sformatowany hierarchii i operatory i rozszerzania przez użytkownika JavaScript podstawie określone funkcje (UDF). Gramatyki JSON umożliwia modelowanie JSON dokumentów jako drzew z etykietami jako węzły drzewa używaną przez zarówno technik automatycznego indeksowania DocumentDB, jak i SQL kwerendy przy wybranej opcji programu DocumentDB.  Aby uzyskać szczegółowe informacje dotyczące sposobu używania gramatyki SQL, zobacz [Kwerendy DocumentDB] [ query] artykuł.

### <a name="what-are-the-data-types-supported-by-documentdb"></a>Co to są typów danych obsługiwanych przez DocumentDB?
Pierwotnymi typami danych obsługiwane w DocumentDB są takie same jak JSON. System typu prostego składający się z ciągów, numery (IEEE754 Podwójna precyzja) i wartości logiczne — ma wartość PRAWDA, FAŁSZ oraz wartości null jest JSON.  Bardziej złożone typy danych, takie jak data/godzina, identyfikator Guid Int64 i geometrii można przedstawić JSON i DocumentDB do tworzenia zagnieżdżonych obiektów przy użyciu operatora {} i tablic przy użyciu operatora []. 

### <a name="how-does-documentdb-provide-concurrency"></a>W jaki sposób DocumentDB zapewnia współbieżności
DocumentDB obsługuje kontrolki optymistyczny współbieżności (OCC) za pośrednictwem protokołu HTTP jednostki znaczniki lub etags. Każdy zasób DocumentDB ma etag i etag jest ustawiona na serwerze każdej aktualizacji dokumentu. Nagłówek etag i bieżącą wartość znajdują się we wszystkich wiadomościach odpowiedzi. Etags można z nagłówkiem dopasowanie Jeżeli serwer mógł zdecydować, jeśli zostaną zaktualizowane zasobu. Wartość dopasowania jeżeli jest wartość etag, która ma być sprawdzany. Jeśli wartość etag odpowiada wartość etag serwera, zasób zostanie zaktualizowana. Jeśli etag już nie jest aktualny, serwer odrzuci operacji z "HTTP 412 wstępny błąd" Kod odpowiedzi. Klient zostaną refetch zasobów umożliwia uzyskanie bieżącą wartość etag dla zasobu. Ponadto etags można z nagłówkiem Jeżeli żaden-dopasowania do określenia, czy potrzebny jest ponowne pobranie zasobu. 

Aby użyć optymistyczny współbieżności w .NET, należy użyć klasy [AccessCondition](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.accesscondition.aspx) . Przykładowe .NET możesz znaleźć [Plik Program.cs](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/DocumentManagement/Program.cs) w próbce DocumentManagement na github.

### <a name="how-do-i-perform-transactions-in-documentdb"></a>Jak wykonać transakcji w DocumentDB?
DocumentDB obsługuje zintegrowane języka transakcje za pomocą procedury JavaScript przechowywane i wyzwalacze. Wszystkie operacje bazy danych wewnątrz skryptów są wykonywane w obszarze izolacji migawki występujące w kolekcji jest zbiór jedną partycją lub dokumenty o tej samej partition klucza wartości w zbiorze, jeśli kolekcji jest podzielona. Migawki wersji dokumentu (ETags) jest wykonywane na początku transakcji i Projekt zatwierdzony tylko wtedy, gdy skrypt zakończyło się powodzeniem. Jeśli kod JavaScript zgłasza błąd, transakcja jest wycofana. Aby uzyskać więcej informacji, zobacz [programowania DocumentDB po stronie serwera](documentdb-programming.md) .

### <a name="how-can-i-bulk-insert-documents-into-documentdb"></a>Jak zbiorczo Wstawianie dokumentów do DocumentDB 
Istnieją trzy sposoby zbiorczo Wstawianie dokumentów do DocumentDB:

- Narzędzie migracji danych, zgodnie z opisem w [oknie importowanie danych do DocumentDB](documentdb-import-data.md).
- Okno Eksploratora dokumentu w Portal Azure, zgodnie z opisem w [zbiorcze Dodawanie dokumentów przy użyciu Eksploratora dokumentu](documentdb-view-json-document-explorer.md#BulkAdd).
- Przechowywane procedury, zgodnie z opisem w [programowaniu DocumentDB po stronie serwera](documentdb-programming.md).

### <a name="does-documentdb-support-resource-link-caching"></a>DocumentDB obsługuje buforowanie łącze zasobu?
Tak, ponieważ DocumentDB jest RESTful usługi, łącza zasobów są niezmienne i mogą być buforowane. Klienci DocumentDB można określić nagłówkiem "Jeżeli żaden-dopasowania" Odczyt dla wszystkich zasobów, takich jak dokument lub zbiór i aktualizacji skopiowanie ich lokalnych zmienić tylko wtedy, gdy wersja serwera zawiera. 

### <a name="is-a-local-instance-of-documentdb-available"></a>Lokalne wystąpienie DocumentDB jest dostępna?
W tej chwili lokalnego wystąpienia DocumentDB nie jest dostępna. Możesz śledzić stan emulatora lokalnej i w głosowaniu dla niego na [forum opinii](https://feedback.azure.com/forums/263030-documentdb/suggestions/6328798-standalone-local-instance).


[azure-portal]: https://portal.azure.com
[query]: documentdb-sql-query.md
 
