<properties 
    pageTitle="Biblioteka klienta elastyczne bazy danych za pomocą struktury obiektu | Microsoft Azure" 
    description="Skorzystać z biblioteki klienta elastyczne bazy danych i struktury jednostki kodowania baz danych" 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="torsteng" 
    editor=""/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="torsteng"/>

# <a name="elastic-database-client-library-with-entity-framework"></a>Elastyczne bibliotece klienta bazy danych przy użyciu struktury obiektu 
 
Ten dokument zawiera zmiany w aplikacji Framework jednostki, które są potrzebne do integracji z [Narzędzia elastyczne bazy danych](sql-database-elastic-scale-introduction.md). Skoncentrować się na redagowania [shard mapowanie zarządzania](sql-database-elastic-scale-shard-map-management.md) i [Rozsyłanie zależne od danych](sql-database-elastic-scale-data-dependent-routing.md) przy użyciu struktury jednostki **Kodu pierwszą** metodę. Samouczek [Kod pierwszego — nowej bazy danych](http://msdn.microsoft.com/data/jj193542.aspx) dla EF służy jako naszym przykładzie uruchomionego w tym dokumencie. Przykładowy kod towarzyszących tego dokumentu jest częścią narzędzia elastyczne bazy danych, ustaw próbek w Visual Studio przykłady.
  
## <a name="downloading-and-running-the-sample-code"></a>Pobieranie i uruchamianie kodu próbki
Aby pobrać kod w tym artykule:

* Wymagany jest program Visual Studio 2012 lub nowszym. 
* Uruchom program Visual Studio. 
* W programie Visual Studio wybierz Plik -> Nowy projekt. 
* W oknie dialogowym "Nowy projekt" Przejdź do **Próbki Online** dla **programu Visual C#** i wpisz "elastyczne db" w polu wyszukiwania w prawym górnym rogu.
    
    ![Struktura jednostki i elastycznym bazy danych przykładowych aplikacji][1] 

    Zaznacz przykładowy o nazwie **Elastyczne narzędzia bazy danych SQL Azure — integracji Framework jednostki**. Po zaakceptowaniu licencji, ładuje próbki. 

Aby uruchomić próbki, należy utworzyć trzy puste bazy danych w bazie danych SQL Azure:

* Baza danych Menedżera Map shard
* Baza danych shard 1
* Baza danych shard 2

Po utworzeniu tych baz danych, Wypełnij symbole zastępcze w **Plik Program.cs** przy użyciu nazwy serwera bazy danych SQL Azure, nazwy bazy danych i poświadczenia nawiązywania połączenia z bazy danych. Tworzenie rozwiązania w programie Visual Studio. Program Visual Studio pobierze wymagane pakiety NuGet w bibliotece elastyczne bazy danych klienta Framework jednostki i przejściowych błędów obsługi w ramach procesu tworzenia. Upewnij się, że przywracanie pakietów NuGet jest włączone dla tego rozwiązania. To ustawienie zostanie włączone, klikając prawym przyciskiem myszy plik rozwiązania w Eksploratorze rozwiązań programu Visual Studio. 

## <a name="entity-framework-workflows"></a>Przepływy pracy Framework jednostki 

Jednostki Framework deweloperzy opierają się na jednym z następujących czterech przepływów pracy do tworzenia aplikacji i w celu zapewnienia utrzymywanie dla obiektów aplikacji: 

* **Pierwszy kod (nowej bazy danych)**: EF deweloper tworzy modelu w kodzie aplikacji, a następnie EF generuje bazy danych z niego. 
* **Pierwszy kod (istniejącej bazy danych)**: Projektant oferuje EF Generuj kod aplikacji modelu z istniejącej bazy danych.
* **Pierwszy modelu**: utworzenie przez dewelopera modelu w Projektancie EF, a następnie EF tworzy bazę danych z modelu.
* **Pierwszej bazy danych**: projektanta używa EF narzędzie rozpoznać model na podstawie istniejącej bazy danych. 

Tych metod zależne od klasy DbContext przezroczysty Zarządzanie połączenia z bazą danych i schemat bazy danych dla aplikacji. Jak będzie omówimy bardziej szczegółowo w dalszej części dokumentu, różnych konstruktory w klasie podstawowej DbContext pozwolić na różne poziomy kontroli nad utworzenia połączenia bazy danych, tworzenie schematów i ładowania. Problemy mogą pojawić się przede wszystkim z faktu, że zarządzanie połączeniami bazy danych dostarczony przez EF przecina z funkcjami zarządzania połączenia danych zależne routingu interfejsy dostępne Biblioteka klienta elastyczne bazy danych. 

## <a name="elastic-database-tools-assumptions"></a>Założenia narzędzia elastyczne bazy danych 

Definicje terminów zobacz [Słownik narzędzia elastyczne bazy danych](sql-database-elastic-scale-glossary.md).

Biblioteka klienta elastyczne bazy danych do definiowania partycje aplikacji danych o nazwie shardlets. Shardlets są oznaczane klucz sharding i są mapowane na baz danych. Aplikacji może mieć dowolną liczbę baz danych zgodnie z potrzebami i rozpowszechnianie shardlets zapewnienie możliwości lub wydajności danej bieżącego wymagań. Mapowanie wartości klucza sharding z bazami danych są przechowywane przez mapy shard dostarczony przez komputer kliencki elastyczne bazy danych interfejsów API. Nazywamy taką możliwość **Zarządzania mapy Shard**lub SMM dla krótkiej. Mapa shard służy także brokera połączenia z bazą danych dla żądań zawierających klucz sharding. Firma Microsoft odwołują się do tej funkcji jako **routingu zależne od danych**. 
 
Menedżer mapy shard uniemożliwia użytkownikom niespójne widoków w shardlet dane, które mogą wystąpić podczas operacji zarządzania równoczesne shardlet (na przykład przenoszenie danych z jednej shard do innego) są dzieje. W tym celu mapy shard zarządzane przez klienta brokera biblioteki połączeń bazy danych dla aplikacji. Dzięki temu funkcja mapy shard automatycznie zakończyć połączenie z bazą danych podczas operacji zarządzania shard mogą mieć wpływ shardlet, którego utworzono połączenie. Tej metody należy integracja z niektórych funkcji w EF, takich jak tworzenie nowych połączeń z istniejącej wyszukać istnienia bazy danych. Na ogół naszych obserwacji została działanie standardowy konstruktory DbContext tylko działają prawidłowo dla połączeń zamknięty bazy danych, które można było przeprowadzane bezpiecznie dla EF. Zasady projektowanie elastycznych bazy danych jest zamiast tego brokera otwarte połączenia. Jeden wydaje się, że zamknięciem połączenia wykonaną za Biblioteka klienta pośrednictwem przed przekazaniem ich do EF DbContext może rozwiązać ten problem. Jednak zamykanie połączenia i Polegaj na EF otworzyć go ponownie, jedną foregoes testy sprawdzania poprawności i spójność wykonywaną przez biblioteki. Funkcji migracji w EF, jednak używa tych połączeń do zarządzania schematem bazy danych źródłowych w sposób, który jest niewidoczny do aplikacji. Najlepiej, jeśli chcemy zachować i łączenie wszystkich tych funkcji z biblioteki klienta elastyczne bazy danych i EF w tej samej aplikacji. W poniższej sekcji omówiono te właściwości i wymagania bardziej szczegółowo. 


## <a name="requirements"></a>Wymagania dotyczące 

Podczas pracy z obiektu ramach API i Biblioteka klienta elastyczne bazy danych, chcemy zachować następujące właściwości: 

* **Poza skalowanie**: Aby dodać lub usunąć baz danych w warstwie danych stosowania sharded stosownie do potrzeb możliwości stosowania. Oznacza to, że kontrola nad tworzenia i usuwania baz danych i przy użyciu shard elastyczne bazy danych mapowanie interfejsy API Menedżera Zarządzanie bazami danych i mapowania shardlets. 

* **Zgodność**: aplikacja wykorzystuje sharding i używa zależne możliwości routingu danych Biblioteka klienta. Aby uniknąć uszkodzenia lub wyników kwerendy problem, połączenia są brokered za pomocą Menedżera mapy shard. Zachowuje również sprawdzania poprawności i spójność.
 
* **Kod pierwszego**: Aby zachować wygody i EF kod pierwszego modelu. W pierwszym kodu klas w aplikacji są mapowane przezroczysty na podstawowej struktury bazy danych. Kod aplikacji współdziała z DbSets, który maski większość aspektów przetwarzania bazy danych źródłowych.
 
* **Schemat**: jednostki Framework obsługuje tworzenia schematu początkowej bazy danych i schematu kolejne zmiany do migracji. Dzięki możliwości zachowania te funkcje, dostosowania aplikacji jest łatwe, jak rozwoju danych. 

Poniższe wskazówki powoduje, że jak spełnia tych wymagań dla pierwszej kod aplikacji za pomocą narzędzia elastyczne bazy danych. 

## <a name="data-dependent-routing-using-ef-dbcontext"></a>Zależne od, routing korzystanie z EF DbContext danych 

Połączenia z bazą danych przy użyciu struktury jednostki są zwykle zarządzana za pośrednictwem podklasy **DbContext**. Utwórz tych podklas, wynikających z **DbContext**. Jest to miejsce, w którym definiowanie usługi **DbSets** wykonania kopii bazy danych kolekcji obiektów CLR aplikacji. W kontekście routingu zależne danych można wskazano niektóre właściwości pomocne, które nie zawierają niekoniecznie dla innych pierwszego scenariuszy aplikacji EF kodu: 

* Baza danych już istnieje i została zarejestrowana w planie shard elastyczne bazy danych. 
* Już po wdrożeniu schematu aplikacji do bazy danych (wyjaśniono poniżej). 
* Połączenia routingu zależne od danych do bazy danych są wykonaną za mapy shard pośrednictwem. 

Aby zintegrować **DbContexts** z routing zależne od danych poza skalowanie:

1. Tworzenie połączeń fizycznej bazy danych za pośrednictwem interfejsów klienta elastyczne bazy danych Menedżera mapy shard, 
2. Zawijanie połączenie z klasy **DbContext**
3. Przekaż połączenie w dół do klas podstawowych **DbContext** , aby upewnić się, że wszystkie przetwarzania po stronie EF się dzieje, a także. 

W poniższym przykładzie przedstawiono tej metody. (Kod jest również towarzyszące projektu programu Visual Studio)

    public class ElasticScaleContext<T> : DbContext
    {
    public DbSet<Blog> Blogs { get; set; }
    …

        // C'tor for data dependent routing. This call will open a validated connection 
        // routed to the proper shard by the shard map manager. 
        // Note that the base class c'tor call will fail for an open connection
        // if migrations need to be done and SQL credentials are used. This is the reason for the 
        // separation of c'tors into the data-dependent routing case (this c'tor) and the internal c'tor for new shards.
        public ElasticScaleContext(ShardMap shardMap, T shardingKey, string connectionStr)
            : base(CreateDDRConnection(shardMap, shardingKey, connectionStr), 
            true /* contextOwnsConnection */)
        {
        }

        // Only static methods are allowed in calls into base class c'tors.
        private static DbConnection CreateDDRConnection(
        ShardMap shardMap, 
        T shardingKey, 
        string connectionStr)
        {
            // No initialization
            Database.SetInitializer<ElasticScaleContext<T>>(null);

            // Ask shard map to broker a validated connection for the given key
            SqlConnection conn = shardMap.OpenConnectionForKey<T>
                                (shardingKey, connectionStr, ConnectionOptions.Validate);
            return conn;
        }    

## <a name="main-points"></a>Główne punkty
* Konstruktora new zastępuje Konstruktor w klasy DbContext 
* Konstruktora new przyjmuje argumenty, które są wymagane do danych zależne routingu za pośrednictwem biblioteki klienta elastyczne bazy danych:
    * Mapa shard, aby uzyskać dostęp do interfejsów routingu zależne od danych
    * klucz sharding do identyfikowania shardlet,
    * Parametry połączenia przy użyciu poświadczeń połączenia routingu zależne od danych, aby shard. 
 
* Połączenie do konstruktora klasy podstawowej uwzględnia detour metodą statyczną, które będzie wykonywać wszystkie kroki niezbędne do routingu zależne od danych. 
   * Używa połączenia OpenConnectionForKey interfejsów klienta elastyczne bazy danych na mapie shard ustanawiania otwartego połączenia.
   * Mapa shard tworzy Otwieranie połączenia z shard, w której znajduje się shardlet klucza danej sharding.
   * To połączenie Otwórz przechodzi z powrotem do konstruktora klasy podstawowej DbContext, aby wskazać, że to połączenie ma być używane przez EF zamiast co EF automatycznie utworzyć nowe połączenie. W ten sposób połączenie oznakowanego przez interfejs API klienta elastyczne bazy danych, aby go zagwarantowania spójności w obszarze Operacje zarządzania shard mapy.
 
  
Za pomocą konstruktora new dla swojej klasy DbContext zamiast domyślnego konstruktora w kodzie. Oto przykład: 

    // Create and save a new blog.

    Console.Write("Enter a name for a new blog: "); 
    var name = Console.ReadLine(); 

    using (var db = new ElasticScaleContext<int>( 
                            sharding.ShardMap,  
                            tenantId1,  
                            connStrBldr.ConnectionString)) 
    { 
        var blog = new Blog { Name = name }; 
        db.Blogs.Add(blog); 
        db.SaveChanges(); 

        // Display all Blogs for tenant 1 
        var query = from b in db.Blogs 
                    orderby b.Name 
                    select b; 
     … 
    }

Konstruktora new otwiera połączenie shard, który zawiera dane dla shardlet określonego przez wartość **tenantid1**. Kod w bloku **za pomocą** pozostaje bez zmian, aby uzyskać dostęp do **DbSet** dla blogów za pomocą EF na shard dla **tenantid1**. Spowoduje to zmianę znaczeń właściwych dla kodu przy użyciu zablokować tak, aby wszystkie operacje bazy danych są teraz ograniczone do jednego shard, gdzie są przechowywane **tenantid1** . Na przykład zapytanie LINQ blogów **DbSet** zwróci tylko blogów przechowywane w bieżącym shard, ale nie odpowiednie elementy, które są przechowywane na innych odłamki.  

#### <a name="transient-faults-handling"></a>Błędy przejściowych obsługi
Zespół Patterns firmy Microsoft i wskazówki dotyczące opublikowanych [Przejściowych błędów obsługi aplikacji blok](https://msdn.microsoft.com/library/dn440719.aspx). Biblioteka jest używana z elastycznych skali Biblioteka klienta w połączeniu z EF. Jednak upewnij się, że każdy wyjątek przejściowych zwraca do miejsca miejsce, w którym możemy zapewnić, że konstruktora new jest używany po przejściowych błędów tak, aby wszystkie nowe próba nawiązania połączenia jest nawiązane za pomocą konstruktory, które mają możemy tweaked. W przeciwnym razie połączenia w celu poprawnego shard nie jest gwarantowana, a nie ma żadnych gwarancji, połączenie jest utrzymany zmianie do mapy shard. 

Poniższy przykład kodu ilustruje, jak można zasady ponów próbę SQL wokół nowe Konstruktory klasy **DbContext** : 

    SqlDatabaseUtils.SqlRetryPolicy.ExecuteAction(() => 
    { 
        using (var db = new ElasticScaleContext<int>( 
                                sharding.ShardMap,  
                                tenantId1,  
                                connStrBldr.ConnectionString)) 
            { 
                    var blog = new Blog { Name = name }; 
                    db.Blogs.Add(blog); 
                    db.SaveChanges(); 
            … 
            } 
        }); 

**SqlDatabaseUtils.SqlRetryPolicy** w kodzie powyżej jest definiowana jako **SqlDatabaseTransientErrorDetectionStrategy** ponów próbę liczba 10 i 5 sekund czas oczekiwania między kolejnymi próbami. Ta metoda jest podobna do wskazówki dotyczące EF i transakcje zainicjowany przez użytkownika (zobacz [ograniczenia ponawianie próby wykonania strategii (r EF6)](http://msdn.microsoft.com/data/dn307226). Obu sytuacjach wymagają, że program aplikacji określa zakres, do którego zwraca przejściowych wyjątku: ponowne otwarcie transakcji lub (jak pokazano) odtworzenie kontekście z Wielkiej konstruktora korzystającego z biblioteki klienta elastyczne bazy danych.

Potrzeba kontrolować, gdzie przejściowych wyjątki potrwać Wstecz w zakresie również wyklucza stosowanie wbudowanego **SqlAzureExecutionStrategy** dołączonej EF. **SqlAzureExecutionStrategy** chcesz ponownie otworzyć połączenie, ale nie za pomocą **OpenConnectionForKey** i w związku z tym pominąć wszystkie sprawdzania poprawności, które odbywa się w ramach połączenia **OpenConnectionForKey** . Zamiast tego przykładowy kod używa wbudowanych **DefaultExecutionStrategy** dołączany EF. W przeciwieństwie do **SqlAzureExecutionStrategy**działa poprawnie w połączeniu z zasad ponów próbę z przejściowych obsługi błędów. Zasady wykonywania jest ustawiona w klasie **ElasticScaleDbConfiguration** . Należy zauważyć, że firma Microsoft zdecydowała nie używa **DefaultSqlExecutionStrategy** , ponieważ sugeruje używać **SqlAzureExecutionStrategy** występują wyjątki przejściowych — co może prowadzić do problem zachowanie zgodnie z opisem. Aby uzyskać więcej informacji na różne ponów próbę zasady i EF zobacz [Elastyczność połączenia w EF](http://msdn.microsoft.com/data/dn456835.aspx).     

#### <a name="constructor-rewrites"></a>Konstruktor ponownego
Przykłady kodu powyżej przedstawienie domyślnego konstruktora ponownie zapisuje wymagane dla aplikacji usługi zależne od, routing Framework jednostki danych. Poniższa tabela generalizes to podejście do innych konstruktorów. 


Konstruktor bieżącego  | Konstruktor nowych danych | Konstruktor podstawowej | Notatki
---------- | ----------- | ------------|----------
MyContext() |ElasticScaleContext (ShardMap, TKey) |DbContext (połączenia DbConnection, wartość logiczna) |Połączenie musi być funkcją mapy shard i klucza routingu zależne od danych. Trzeba obejścia połączenie automatyczne tworzenie przez EF, a zamiast tego za pomocą shard broker połączenie. 
MyContext(string)|ElasticScaleContext (ShardMap, TKey) |DbContext (połączenia DbConnection, wartość logiczna) |Połączenie jest funkcja mapy shard i klucza routingu zależne od danych. Ciąg nazwy lub połączenie stałej bazy danych nie będzie działać w miarę ich obejścia sprawdzania poprawności w planie shard. 
MyContext(DbCompiledModel) |ElasticScaleContext (ShardMap, TKey, DbCompiledModel) |DbContext (połączenia DbConnection, DbCompiledModel, wartość logiczna) |Połączenie będzie utworzyć dla danego shard klucza mapy i sharding z modelem opisane. Skompilowany model zostaną przekazane do podstawowej c'tor.
MyContext (połączenia DbConnection, wartość logiczna) |ElasticScaleContext (ShardMap, TKey, wartość logiczna) |DbContext (połączenia DbConnection, wartość logiczna) |Połączenie musi być wnioskowane na podstawie mapy shard oraz klawiszy. Nie można przedstawić jako dane wejściowe, (chyba że te dane wejściowe został już za pomocą mapy shard oraz klawiszy). Wartość logiczna zostaną przekazane. 
MyContext (ciąg, DbCompiledModel) |ElasticScaleContext (ShardMap, TKey, DbCompiledModel) |DbContext (połączenia DbConnection, DbCompiledModel, wartość logiczna) |Połączenie musi być wnioskowane na podstawie mapy shard oraz klawiszy. Nie można przedstawić jako dane wejściowe, (chyba że te dane wejściowe został za pomocą mapy shard oraz klawiszy). Skompilowany model zostaną przekazane. 
MyContext (ObjectContext, wartość logiczna) |ElasticScaleContext (ShardMap, TKey, ObjectContext, wartość logiczna) |DbContext (ObjectContext, wartość logiczna) |Konstruktora new należy upewnić, że wszystkie połączenia w ObjectContext przekazany jako dane wejściowe jest przekierowane do połączenia zarządzane przez elastyczne Skala. Szczegółowe omówienie ObjectContexts wykracza poza zakres tego dokumentu.
MyContext (połączenia DbConnection, DbCompiledModel, wartość logiczna) |ElasticScaleContext (ShardMap, TKey, DbCompiledModel, wartość logiczna)| DbContext (połączenia DbConnection, DbCompiledModel, wartość logiczna); |Połączenie musi być wnioskowane na podstawie mapy shard oraz klawiszy. Nie można przedstawić połączenia jako dane wejściowe, (chyba że te dane wejściowe został już za pomocą mapy shard oraz klawiszy). Model i wartość logiczna są przekazywane do konstruktora klasy podstawowej. 

## <a name="shard-schema-deployment-through-ef-migrations"></a>Wdrożenie schematu shard za pośrednictwem EF migracji 

Zarządzanie schematem automatyczne jest wygody, dostępne w ramach jednostki. W kontekście aplikacji za pomocą narzędzia bazy danych elastyczne chcemy zachować tę możliwość automatycznie obsługi administracyjnej schematu do nowo utworzonego odłamki, po dodaniu do sharded aplikacji baz danych. Przypadek użycia podstawowy jest w celu zwiększenia pojemności na warstwie danych dla aplikacji sharded za pomocą EF. Polegaj na osoby EF funkcje zarządzania schematu zmniejsza nakład pracy administracji bazy danych z aplikacją sharded oparty na EF. 

Wdrożenia schematu w ramach migracji EF sprawdza się najlepiej w **zamkniętych połączeń**. To jest w odróżnieniu od scenariusz zależne od danych routingu korzystającego otwarte połączenie dostarczony przez interfejs API klienta elastyczne bazy danych. Inna różnica polega na wymagania spójności: podczas pożądane w celu zapewnienia zgodności dla wszystkich połączeń routingu zależne od danych w celu ochrony przed równoczesne shard Mapa manipulowania, nie jest problemem z wdrożenia wstępny schemat do nowej bazy danych, który ma jeszcze nie została zarejestrowana w planie shard i jeszcze nie zostały przydzielone do przechowywania shardlets. Firma Microsoft w związku z tym zależeć połączenia zwykłej bazie danych dla tej scenariuszy, zamiast routingu zależne od danych.  

Prowadzi to do metody miejsce, w którym wdrożenia schematu w ramach migracji EF jest ściśle związane z rejestracji nowej bazy danych jako shard na mapie shard aplikacji. To zależy od następujące wymagania: 

* Baza danych została utworzona. 
* Baza danych jest pusta — przechowuje Brak schematu użytkownika i bez danych użytkownika.
* Bazy danych jeszcze nie są dostępne za pośrednictwem interfejsów API klienta programu elastyczne bazy danych do routingu zależne od danych. 

Z te wymagania wstępne w miejscu możemy utworzyć zwykłą nie otwartego **SqlConnection** aby rozpocząć migracji EF wdrożenia schematu. Poniższy przykład kodu ilustruje tej metody. 

        // Enter a new shard - i.e. an empty database - to the shard map, allocate a first tenant to it  
        // and kick off EF intialization of the database to deploy schema 

        public void RegisterNewShard(string server, string database, string connStr, int key) 
        { 

            Shard shard = this.ShardMap.CreateShard(new ShardLocation(server, database)); 

            SqlConnectionStringBuilder connStrBldr = new SqlConnectionStringBuilder(connStr); 
            connStrBldr.DataSource = server; 
            connStrBldr.InitialCatalog = database; 

            // Go into a DbContext to trigger migrations and schema deployment for the new shard. 
            // This requires an un-opened connection. 
            using (var db = new ElasticScaleContext<int>(connStrBldr.ConnectionString)) 
            { 
                // Run a query to engage EF migrations 
                (from b in db.Blogs 
                    select b).Count(); 
            } 

            // Register the mapping of the tenant to the shard in the shard map. 
            // After this step, data-dependent routing on the shard map can be used 

            this.ShardMap.CreatePointMapping(key, shard); 
        } 
 

W tym przykładzie pokazano metody **RegisterNewShard** rejestruje shard na mapie shard, wdraża schematu za pomocą EF migracji i przechowuje mapowania klucza sharding do shard. Zależy konstruktora klasy **DbContext** (**ElasticScaleContext** w próbce) kierujące parametry połączenia SQL jako danych wejściowych. Kod ten konstruktor jest bezpośrednie, jak w poniższym przykładzie: 


        // C'tor to deploy schema and migrations to a new shard 
        protected internal ElasticScaleContext(string connectionString) 
            : base(SetInitializerForConnection(connectionString)) 
        { 
        } 

        // Only static methods are allowed in calls into base class c'tors 
        private static string SetInitializerForConnection(string connnectionString) 
        { 
            // We want existence checks so that the schema can get deployed 
            Database.SetInitializer<ElasticScaleContext<T>>( 
        new CreateDatabaseIfNotExists<ElasticScaleContext<T>>()); 

            return connnectionString; 
        } 
 
Jedna może być używana wersja konstruktora dziedziczone z klasą podstawową. Ale kod należy upewnić się, że inicjator domyślne EF jest używany podczas łączenia. W związku z tym krótkim detour do metody statyczne przed połączeniem do konstruktora klasy podstawowej przy użyciu parametrów połączenia. Zauważ, że rejestracji odłamki powinna działać w domenie innej aplikacji lub proces, aby upewnić się, że ustawienia inicjator EF nie powodują one konfliktu. 


## <a name="limitations"></a>Ograniczenia 

Metody opisane w niniejszym dokumencie kilka ograniczeń z: 

* EF aplikacje, które używają **LocalDb** najpierw należy przeprowadzić migrację do zwykłej bazie danych programu SQL Server przed użyciem Biblioteka klienta elastyczne bazy danych. Skalowanie zewnętrzne aplikację za pośrednictwem sharding ze skalą elastyczne nie jest możliwe z **LocalDb**. Należy zauważyć, że rozwój, nadal możesz używać **LocalDb**. 

* Wszelkie zmiany do aplikacji, które służą do przedstawiania zmiany schematu bazy danych należy obsłużone migracji EF na wszystkich odłamki. Przykładowy kod dla tego dokumentu nie pokazano, jak to zrobić. Rozważ użycie bazy danych aktualizacji z parametrem ConnectionString, aby przejść przez wszystkie odłamki; i wyodrębnianie skrypt T-SQL oczekiwanie migracji przy użyciu bazy danych aktualizacji — scenariusz opcji i dotyczą usługi odłamki skrypt T-SQL.  

* Podane żądanie, przyjmuje się, że wszystkie jego przetwarzania bazy danych znajduje się w jednym shard określonych klucza sharding dostarczonego przez żądanie. Jednak to założenie zawsze mają wartość PRAWDA. Na przykład, kiedy braku możliwości udostępnienie klucz sharding. Aby rozwiązać ten, Biblioteka klienta zawiera klasy **MultiShardQuery** , która wykonuje abstrakcji połączenia do badania nad kilkoma odłamki. Nauka korzystania **MultiShardQuery** w połączeniu z EF wykracza poza zakres tego dokumentu

## <a name="conclusion"></a>Wnioski

Kroki opisane w niniejszym dokumencie EF aplikacje mogą używać funkcji Biblioteka klienta elastyczne bazy danych dla danych zależne routingu przez Refaktoryzacja konstruktory podklasy **DbContext** używany w aplikacji EF. Ogranicza zmian wymaganych do tych miejsc, w której klasy **DbContext** już istnieją. Ponadto aplikacje EF nadal korzystać z wdrożenia schematu automatyczne łącząc czynności, które wywołania migracji konieczne EF z rejestracją odłamki nowych i mapowania na mapie shard. 


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-use-entity-framework-applications-visual-studio/sample.png
 