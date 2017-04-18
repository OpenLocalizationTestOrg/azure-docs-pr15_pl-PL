 <properties
    pageTitle="Jak zwiększyć wydajność aplikacji bazy danych SQL Azure za pomocą tworzeniu partii"
    description="Temat dostarcza dowodów tej operacji łączenia we wsady bazy danych znacznie imroves szybkość i skalowalność aplikacji bazy danych SQL Azure. Mimo że te łączenia we wsady techniki pracy dla każdej bazy danych programu SQL Server, artykuł dotyczy Azure."
    services="sql-database"
    documentationCenter="na"
    authors="annemill"
    manager="jhubbard"
    editor="" />


<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-management"
    ms.date="07/12/2016"
    ms.author="annemill" />

# <a name="how-to-use-batching-to-improve-sql-database-application-performance"></a>Jak używać tworzeniu partii, aby zwiększyć wydajność aplikacji bazy danych SQL

Tworzeniu partii operacje, aby bazy danych SQL Azure znacznie zwiększa wydajność i skalowalność aplikacji. Aby zrozumieć korzyści, pierwsza część w tym artykule opisano niektóre przykładowe wyniki testów, ilustrujące porównanie żądania kolejnych i wsadowej z bazą danych SQL. W dalszej części artykułu zawiera technik, scenariusze i zagadnienia, które będą pomocne przy tworzeniu partii pomyślnie w aplikacjach Azure za pomocą.

**Autorzy**: Jason Roth, Silvano Coriani, Trent Swanson (łącznie skali 180)

**Recenzenci**: Conor Cunningham, Michael Thomassy

## <a name="why-is-batching-important-for-sql-database"></a>Dlaczego jest tworzeniu partii ważne bazy danych SQL?
Tworzeniu partii połączenia z usługą zdalny jest znane strategii zwiększenia wydajność i skalowalność. Są to stałe koszty przetworzenia wszelkich interakcji z zdalnego usługi, takiej jak szeregowania, transferu sieciowego i deserializacji. Wiele osobnych transakcje do jednej partii opakowań minimalizuje tych kosztów.

W tym dokumencie chcemy sprawdzenie tworzeniu partii strategii i scenariusze różnych bazy danych SQL. Strategie te również są ważne dla aplikacji lokalnej, korzystających z programu SQL Server, istnieje kilka możliwych powodów wyróżnianie stosowania tworzeniu partii bazy danych SQL:

- Istnieje potencjalnie większa opóźnień sieci na uzyskiwanie dostępu do bazy danych SQL, zwłaszcza jeśli uzyskujesz dostęp do bazy danych SQL z poza z tym samym centrum danych Microsoft Azure.
- Multitenant właściwości bazy danych SQL oznacza, że wydajność odpowiada warstwy dostępu do danych do ogólnej skalowalność bazy danych. Baza danych SQL musi uniemożliwiają przejęcie kontroli nad zasobów bazy danych w celu szkody z innych dzierżaw jeden dzierżawy i użytkownik. Wynikiem zastosowania powyżej wstępnie zdefiniowanych przydziałów bazy danych SQL można zmniejszyć przepustowość lub odpowiadanie za pomocą ograniczania wyjątki. Efektywności, takie jak tworzeniu partii, umożliwiają większą ilość pracy nad bazy danych SQL przed osiągnięciem te limity. 
- Tworzeniu partii obowiązuje również dla architektury, które używają wielu baz danych (sharding). Wydajność możesz komunikować się z każdej jednostki bazy danych jest nadal kluczowym elementem do ogólnej skalowalność. 

Jest jedną z zalet korzystania z bazy danych SQL, że nie trzeba zarządzać serwerami obsługującymi bazy danych. Jednak to zarządzane infrastruktury również oznacza, że do przemyślenia optymalizacji bazy danych w inny sposób. Nie można przeglądać do poprawy infrastruktury sprzętu lub sieci bazy danych. Microsoft Azure kontrolki tych środowiskach. Główny obszar, który można kontrolować jest interakcji aplikacji z bazą danych SQL. Tworzeniu partii jest jednym z tych optymalizacji. 

Pierwsza część papieru analizuje różne techniki łączenia we wsady .NET aplikacjom korzystanie z bazy danych SQL. Ostatnie dwie sekcje obejmują łączenia we wsady wskazówki i scenariuszy.

## <a name="batching-strategies"></a>Tworzeniu partii strategii

### <a name="note-about-timing-results-in-this-topic"></a>Należy pamiętać o chronometrażu wyników w tym temacie
>[AZURE.NOTE] Wyniki nie są stawek, ale są przeznaczone do pokazywania **wydajności względne**. Chronometraż są oparte na średnio o co najmniej 10 testów. Operacje są wstawiane do pustej tabeli. Testy zostały mierzone sprzed wersji 12, a nie zawsze odpowiadają przepustowości, które mogą wystąpić w bazie danych w wersji 12 przy użyciu nowego [warstwy usługi](sql-database-service-tiers.md). Względne korzyści, jakie łączenia we wsady metoda powinna być podobna.

### <a name="transactions"></a>Transakcje
Wydaje się dziwne, aby rozpocząć Przegląd tworzeniu partii według omawianie transakcje. Ale stosowanie transakcji po stronie klienta ma delikatne łączenia we wsady efekt po stronie serwera, który zwiększa wydajność. I transakcje można dodawać z tylko kilku wierszy kodu, tak umożliwiają szybkie Aby zwiększyć wydajność kolejnych operacji.

Warto rozważyć następujący C# kod, który zawiera sekwencji przedstawiający wstawianie i aktualizowanie operacji na prostej tabeli.

    List<string> dbOperations = new List<string>();
    dbOperations.Add("update MyTable set mytext = 'updated text' where id = 1");
    dbOperations.Add("update MyTable set mytext = 'updated text' where id = 2");
    dbOperations.Add("update MyTable set mytext = 'updated text' where id = 3");
    dbOperations.Add("insert MyTable values ('new value',1)");
    dbOperations.Add("insert MyTable values ('new value',2)");
    dbOperations.Add("insert MyTable values ('new value',3)");

Poniższy kod ADO.NET kolejno wykonuje następujące operacje.

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        conn.Open();
    
        foreach(string commandString in dbOperations)
        {
            SqlCommand cmd = new SqlCommand(commandString, conn);
            cmd.ExecuteNonQuery();                   
        }
    }

Najlepszym sposobem zoptymalizować kod jest zaimplementowania pewnych form po stronie klienta tworzeniu partii telefonów. Występuje w prosty sposób zwiększyć wydajność tego kodu, po prostu zawijanie kolejność połączeń w transakcji. Oto ten sam kod, która korzysta z transakcji.

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        conn.Open();
        SqlTransaction transaction = conn.BeginTransaction();
    
        foreach (string commandString in dbOperations)
        {
            SqlCommand cmd = new SqlCommand(commandString, conn, transaction);
            cmd.ExecuteNonQuery();
        }
    
        transaction.Commit();
    }

Transakcje są faktyczną używane w obu poniższych przykładach. W pierwszym przykładzie każdego poszczególne połączenia jest niejawnej transakcji. W drugim przykładzie jawne transakcji zawijany wszystkich połączeń. Na dokumentacji [zapisu dynamicznymi dziennik transakcji](https://msdn.microsoft.com/library/ms186259.aspx)rekordy dziennika jest opróżniany na dysku po zatwierdzeniu transakcji. Tak, w tym kolejnych połączeń w transakcji, zapisu w dzienniku transakcji może opóźnić aż transakcji zostanie zatwierdzony. W rzeczywistości są Włączanie tworzeniu partii dla zapisywania dziennik transakcji serwera.

W poniższej tabeli przedstawiono niektóre wyniki testów ad hoc. Przeprowadzić testy w tym samym wstawia sekwencyjne z i bez transakcji. Aby uzyskać więcej perspektywy pierwszy zestaw testów uruchomiono zdalnie z komputera przenośnego z bazą danych programu Microsoft Azure. Drugi zestaw testów uruchomiono z usługa w chmurze i bazy danych, że oba znajdowały się w tym samym centrum danych Microsoft Azure (Zachodnia Stany Zjednoczone). W poniższej tabeli przedstawiono czas trwania w milisekundach sekwencyjne wstawia z i bez transakcji.

**Lokalne Azure**:

| Operacje | Brak transakcji (ms) | Transakcji (ms) |
|---|---|---|
| 1 | 130 | 402 |
| 10 | 1208 | 1226 |
| 100 | 12662 | 10395 |
| 1000. | 128852 | 102917 |


**Azure do Azure (samego centrum danych)**:

| Operacje | Brak transakcji (ms) | Transakcji (ms) |
|---|---|---|
| 1 | 21 | 26 |
| 10 | 220 | 56 |
| 100 | 2145 | 341 |
| 1000. | 21479 | 2756 |

>[AZURE.NOTE] Wyniki nie są stawek. Zobacz [Uwaga dotycząca wyniki chronometraż, w tym temacie](#note-about-timing-results-in-this-topic).

W zależności od poprzedniego wyniki testu, faktycznie zawijanie jednej operacji w transakcji zmniejszenie wydajności. Ale zwiększenie liczby operacji w ramach jednej transakcji zwiększenie wydajności staje się bardziej oznaczony. Różnica wydajności jest bardziej zauważalnych również w przypadku, gdy wszystkie operacje występują w centrum danych Microsoft Azure. Zwiększenie opóźnienia używania bazy danych SQL z poza centrum danych Microsoft Azure overshadows wzmocnienia wydajności używania transakcje.

Stosowanie transakcji można zwiększyć wydajność, nadal [obserwować najważniejsze wskazówki dotyczące transakcji i połączenia](https://msdn.microsoft.com/library/ms187484.aspx). Zachowanie transakcji możliwie krótki i zamknij połączenie z bazą danych po zakończeniu pracy. Za pomocą instrukcji w poprzednim przykładzie gwarantuje, że połączenie jest zamknięte po ukończeniu blok kodu kolejne.

W poprzednim przykładzie pokazano, dodać transakcji lokalnej kodowi ADO.NET z dwoma wierszami. Transakcje umożliwiają szybkie zwiększyć wydajność kod, który służy do wstawiania sekwencyjne, aktualizowanie i usuwanie operacji. Jednak dla największą wydajność, należy rozważyć zmianę kodu w przypadku Wykorzystaj tworzeniu partii po stronie klienta, takie jak parametry, zwracających tabelę.

Aby uzyskać więcej informacji na temat transakcji w ADO.NET zobacz [Transakcji lokalnych w ADO.NET](https://msdn.microsoft.com/library/vstudio/2k2hy99x.aspx).

### <a name="table-valued-parameters"></a>Parametry zwracających tabelę
Parametry zwracających tabelę obsługuje typy tabeli zdefiniowane przez użytkownika jako parametrów w instrukcji Transact-SQL, procedur składowanych i funkcji. Ta metoda łączenia we wsady po stronie klienta pozwala na wysyłanie wielu wierszy danych w parametrze zwracających tabelę. Aby użyć parametrów zwracających tabelę, najpierw należy zdefiniować typ tabeli. Następującą instrukcję w języku Transact-SQL tworzy typ tabeli o nazwie **MyTableType**.

    CREATE TYPE MyTableType AS TABLE 
    ( mytext TEXT,
      num INT );
 

Kod możesz utworzyć **DataTable** z dokładnie takich samych nazwach i typów tabeli. Przekaż ten **DataTable** parametrów w kwerendzie tekstu lub przechowywane wywołanie procedury. W poniższym przykładzie pokazano tej techniki:

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        connection.Open();
    
        DataTable table = new DataTable();
        // Add columns and rows. The following is a simple example.
        table.Columns.Add("mytext", typeof(string));
        table.Columns.Add("num", typeof(int));    
        for (var i = 0; i < 10; i++)
        {
            table.Rows.Add(DateTime.Now.ToString(), DateTime.Now.Millisecond);
        }
    
        SqlCommand cmd = new SqlCommand(
            "INSERT INTO MyTable(mytext, num) SELECT mytext, num FROM @TestTvp",
            connection);
                    
        cmd.Parameters.Add(
            new SqlParameter()
            {
                ParameterName = "@TestTvp",
                SqlDbType = SqlDbType.Structured,
                TypeName = "MyTableType",
                Value = table,
            });
    
        cmd.ExecuteNonQuery();
    }

W poprzednim przykładzie, obiekt **SqlCommand** wstawia wiersze z parametrem zwracających tabelę **@TestTvp**. Wcześniej utworzony obiekt **DataTable** jest przypisana do tego parametru przy użyciu metody **SqlCommand.Parameters.Add** . Tworzeniu partii zostanie wstawiony w jednym wywołaniu znacznie zwiększa wydajność w kolejnych zostanie wstawiony.

Aby poprawić dodatkowo w poprzednim przykładzie, należy użyć procedury składowanej zamiast polecenia opartych na tekście. Poniższe polecenie w języku Transact-SQL tworzy procedura przechowywana, która przyjmuje parametr zwracających tabelę **SimpleTestTableType** .

    CREATE PROCEDURE [dbo].[sp_InsertRows] 
    @TestTvp as MyTableType READONLY
    AS
    BEGIN
    INSERT INTO MyTable(mytext, num) 
    SELECT mytext, num FROM @TestTvp
    END
    GO

Następnie zmień deklarację obiektu **SqlCommand** w poprzednim przykładzie kodu do następującego.

    SqlCommand cmd = new SqlCommand("sp_InsertRows", connection);
    cmd.CommandType = CommandType.StoredProcedure;

W większości przypadków zwracających tabelę parametry mieć równoważną lub lepszą wydajność niż w przypadku innych technik łączenia we wsady. Parametry przechowywanymi w tabeli są często zalecane, ponieważ są one bardziej elastyczne niż inne opcje. Na przykład innych technik, takich jak kopiowania zbiorczego SQL, tylko zezwolić na dodanie nowych wierszy. Z parametrami zwracających tabelę można warunków logicznych w procedury składowanej do określenia, które wiersze są aktualizacje, a które wstawia. Typ tabeli można zmodyfikować w taki sposób, aby zawiera kolumny "Operacja", która wskazuje, czy nad określonym wierszem powinna zostać wstawiony, zaktualizowane lub usunięte.

W poniższej tabeli pokazano wyniki testu ad hoc stosowania zwracających tabelę parametrów (w milisekundach).

| Operacje | Lokalne Azure (ms)  | Azure sam centrum danych (ms) |
|---|---|---|
| 1 | 124 | 32 |
| 10 | 131 | 25 |
| 100 | 338 | 51 |
| 1000. | 2615 | 382 |
| 10000 | 23830 | 3586 |

>[AZURE.NOTE] Wyniki nie są stawek. Zobacz [Uwagi dotyczące wyników chronometraż, w tym temacie](#note-about-timing-results-in-this-topic).

Wzmocnienia wydajności w tworzeniu partii są od razu widoczne. W teście sekwencyjne poprzedniej operacji 1000 zajęło 129 s poza centrum danych i 21 sekund z poziomu Centrum danych. Ale z parametrami zwracających tabelę operacje 1000 zajmuje tylko 2.6 sekund poza centrum danych i 0,4 sekund w centrum danych.

Aby uzyskać więcej informacji o parametrach zwracających tabelę zobacz [Table-Valued parametry](https://msdn.microsoft.com/library/bb510489.aspx).

### <a name="sql-bulk-copy"></a>Kopiowanie zbiorcze SQL
Kopiowania zbiorczego SQL jest kolejnym sposobem, aby wstawić dużych ilości danych do docelowej bazy danych. .NET aplikacje mogą używać klasy **SqlBulkCopy** do wykonywania operacji Wstawianie zbiorczo. **SqlBulkCopy** przypomina w funkcji narzędzie wiersza polecenia, **Bcp.exe**lub instrukcji Transact-SQL, **Wstawianie ZBIORCZO**. W poniższym przykładzie pokazano sposób do zbiorczego kopiowania wiersze w źródle **DataTable**, do miejsca docelowego w programie SQL Server Moja_tabela tabeli.

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        connection.Open();
    
        using (SqlBulkCopy bulkCopy = new SqlBulkCopy(connection))
        {
            bulkCopy.DestinationTableName = "MyTable";
            bulkCopy.ColumnMappings.Add("mytext", "mytext");
            bulkCopy.ColumnMappings.Add("num", "num");
            bulkCopy.WriteToServer(table);
        }
    }

Istnieje niektórych przypadkach, gdy kopiowania zbiorczego jest preferowane zwracających tabelę parametrów. Zobacz tabelę parametrów i WSTAWIĆ ZBIORCZO operacje w temacie [Parametrów Table-Valued](https://msdn.microsoft.com/library/bb510489.aspx)zwracających tabelę.

Następujące wyniki badania ad hoc pokazywania wydajności o tworzeniu partii z **SqlBulkCopy** (w milisekundach).

| Operacje | Lokalne Azure (ms)  | Azure samego centrum danych (ms) |
|---|---|---|
| 1 | 433 | 57 |
| 10 | 441 | 32 |
| 100 | 636 | 53 |
| 1000. | 2535 | 341 |
| 10000 | 21605 | 2737 |

>[AZURE.NOTE] Wyniki nie są stawek. Zobacz [Uwaga dotycząca wyniki chronometraż, w tym temacie](#note-about-timing-results-in-this-topic).

W partii mniejszych parametrów zwracających tabelę Użyj outperformed klasy **SqlBulkCopy** . Jednak **SqlBulkCopy** wykonywane szybciej niż zwracających tabelę parametrów 12-31% dla testów 1000 a 10 000 wierszy. Podobnie jak zwracających tabelę parametrów **SqlBulkCopy** jest dobrym rozwiązaniem dla wsadowej wstawia, zwłaszcza w porównaniu do wykonywania operacji przetwarzany wsadowo.

Aby uzyskać więcej informacji na kopiowania zbiorczego w ADO.NET zobacz [Operacji kopiowania zbiorczej w programie SQL Server](https://msdn.microsoft.com/library/7ek5da1a.aspx).

### <a name="multiple-row-parameterized-insert-statements"></a>Instrukcje sparametryzowane wstawianie wielu wierszy
Jeden zamiast małych partii jest sporządzenie dużych parametryczne instrukcja INSERT, która wstawia wiele wierszy. W poniższym przykładzie pokazano tej techniki.

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        connection.Open();
    
        string insertCommand = "INSERT INTO [MyTable] ( mytext, num ) " +
            "VALUES (@p1, @p2), (@p3, @p4), (@p5, @p6), (@p7, @p8), (@p9, @p10)";
    
        SqlCommand cmd = new SqlCommand(insertCommand, connection);
    
        for (int i = 1; i <= 10; i += 2)
        {
            cmd.Parameters.Add(new SqlParameter("@p" + i.ToString(), "test"));
            cmd.Parameters.Add(new SqlParameter("@p" + (i+1).ToString(), i));
        }
    
        cmd.ExecuteNonQuery();
    }
 

W tym przykładzie mają znaleźć się podstawowe pojęcia. Scenariusz bliższy czy pętli jednostki wymagane do utworzenia ciągu kwerendy i parametry polecenia jednocześnie. Jesteś może zawierać maksymalnie parametry kwerendy 2100, co ogranicza całkowitą liczbę wierszy, które mogą być przetwarzane w ten sposób.

Następujące wyniki badania ad hoc Pokaż wydajności tego typu instrukcję (w milisekundach).

| Operacje | Parametry zwracających tabelę (ms) | Wstawianie jednej instrukcji (ms) |
|---|---|---|
| 1 | 32 | 20 |
| 10 | 30 | 25 |
| 100 | 33 | 51 |

>[AZURE.NOTE] Wyniki nie są stawek. Zobacz [Uwagi dotyczące wyników chronometraż, w tym temacie](#note-about-timing-results-in-this-topic).

Tej metody można lepszą partii, które są mniej niż 100 wierszy. Chociaż poprawy jest mały, ta metoda jest odpowiednia inna opcja działa poprawnie rozwiązania określonej aplikacji.

### <a name="dataadapter"></a>Element DataAdapter
**Element DataAdapter** klasy umożliwia zmodyfikować obiekt **zestawu danych** , a następnie przesyłać zmiany, jak WSTAWIĆ, AKTUALIZOWANIE i usuwanie operacji. Jeśli korzystasz z **element DataAdapter** w ten sposób, jest należy zauważyć, że osobnych wywołań dla każdej różnych operacji. Aby zwiększyć wydajność, należy użyć właściwości **UpdateBatchSize** liczby operacji, które powinny być przetwarzany wsadowo naraz. Aby uzyskać więcej informacji zobacz [Wykonywanie DataAdapters przy użyciu operacji partię](https://msdn.microsoft.com/library/aadf8fk2.aspx).

### <a name="entity-framework"></a>Struktura jednostki
Struktura jednostki nie obsługuje obecnie tworzeniu partii. Różnych deweloperów we Wspólnocie próbowano wykazać obejścia, takie jak zastępowanie metody **SaveChanges** . Ale rozwiązania są zwykle złożone i dostosowany do aplikacji i modelu danych. Projekt witrynie codeplex Framework jednostki ma obecnie strony dyskusji na żądanie tej funkcji. Aby wyświetlić tej dyskusji, zobacz [Notatki ze spotkania projektu — 2 sierpnia 2012](http://entityframework.codeplex.com/wikipage?title=Design%20Meeting%20Notes%20-%20August%202%2c%202012).

### <a name="xml"></a>XML
Dla kompletności firma Microsoft uważa, że ważne jest, aby porozmawiać o XML jako łączenia we wsady strategii. Jednak stosowania XML zawiera nie nad innymi metodami wady i zalety kilka. To podejście jest podobne do parametrów zwracających tabelę, ale pliku XML lub ciąg przekazywana do funkcji procedura składowana zamiast tabeli zdefiniowane przez użytkownika. Procedura składowana analizuje polecenia procedura przechowywana.

Istnieje kilka wad tego rozwiązania:

- Praca z danymi XML może być kłopotliwe i podatne błędów.
- Analiza XML w bazie danych może być obciążenie Procesora.
- W większości przypadków ta metoda jest mniejsza niż parametrów zwracających tabelę.

Z tego powodu stosowania XML dla kwerend partii nie jest zalecane.

## <a name="batching-considerations"></a>Tworzeniu partii zagadnienia
W poniższych sekcjach przedstawiono więcej wskazówek dotyczących używania tworzeniu partii w aplikacjach bazy danych SQL.

### <a name="tradeoffs"></a>Kompromisów
W zależności od usługi architektura tworzeniu partii może obejmować zależnościami między wydajność i elastyczność. Na przykład można rozważyć tego scenariusza, gdzie Twoja rola nieoczekiwanie awarii. Jeśli zgubisz jeden wiersz danych wpływ jest mniejsza niż wpływ utraty partię dużych nieprzesłane wierszy. Istnieje większe ryzyko, gdy buforu wierszy przed wysłaniem ich do bazy danych w określonym przedziałem czasu.

Ze względu na to zależnościami jaki typ operacji tej partii możesz. Bardziej odważnie partii (większe partie i dłuższy czas systemu windows) z danymi, które jest mniej ważnych.

### <a name="batch-size"></a>Rozmiar partii
W naszych badań był zwykle nie daje korzyści dzielenie dużych partie na mniejsze fragmenty. W rzeczywistości często tę część pola spowodowało spadek wydajności niż przesyłanie jednej partii duży. Na przykład można rozważyć scenariusz miejsce, w którym chcesz wstawić wiersze 1000. W poniższej tabeli pokazano, jak długo trwa wstawianie wierszy 1000 po podzielone na mniejszych partie przy użyciu parametrów zwracających tabelę.

| Rozmiar partii | Liczba iteracji | Parametry zwracających tabelę (ms) |
| -------- | --- | --- |
| 1000. | 1 | 347 |
| 500 | 2 | 355 |
| 100 | 10 | 465 |
| 50 | 20 | 630 |

>[AZURE.NOTE] Wyniki nie są stawek. Zobacz [Uwagi dotyczące wyników chronometraż, w tym temacie](#note-about-timing-results-in-this-topic).

Można zobaczyć najlepszą wydajność dla wierszy 1000 ma wysłać je wszystkie naraz. W innych badań (tutaj niewidoczne) było wzmocnienia wydajność, aby podzielić partię 10000 wiersza na dwie partie 5000. Ale schemat tabeli testy jest stosunkowo proste, więc należy wykonać testy na określonych danych i rozmiarów partii, aby sprawdzić te ustalenia.

Inny współczynnik brać pod uwagę jest Jeśli partii sumy stanie się zbyt duży, bazy danych SQL może być ograniczenia oraz odmówić na zatwierdzenie partii. Aby uzyskać najlepsze wyniki przetestuj określonego rozwiązania do określenia, czy rozmiar idealny partii. Wprowadź wielkość partii można konfigurować w czasie rzeczywistym, aby umożliwić szybkie zmiany na podstawie wydajności lub błędy.

Na koniec saldo wielkości partii z ryzyka związanego z tworzeniu partii. Jeśli występują błędy przejściowych lub roli kończy się niepowodzeniem, należy rozważyć konsekwencje ponawianie operacji lub utraty danych w grupie.

### <a name="parallel-processing"></a>Przetwarzanie równoległe
Co zrobić, jeśli zajęła ujęciu zmniejszania rozmiaru partii, ale umożliwia wykonywanie pracy przez wiele wątków? Ponownie testów pokazano, że kilka mniejszych wielowątkowe partii zwykle wykonywane odpowiadającą niż jednej partii większe. Pozwala podjąć próbę wstawianie wierszy 1000 partiami jeden lub więcej równoległe następujący test. Ten test pokazano, jak więcej partie jednoczesnego faktycznie obniżenie wydajności.

| Rozmiar partii [iteracji] | Dwa wątki (ms) | Cztery wątki (ms) | Sześć wątków (ms) |
| -------- | --- | --- | --- |
| 1000 [1] | 277 | 315 | 266 |
| 500 [2] | 548 | 278 | 256 |
| 250 [4] | 405 | 329 | 265 |
| 100 [10] | 488 | 439 | 391 |

>[AZURE.NOTE] Wyniki nie są stawek. Zobacz [Uwagi dotyczące wyników chronometraż, w tym temacie](#note-about-timing-results-in-this-topic).

Istnieje kilka powodów potencjalne obniżenie wydajności z powodu równoległości:

- Istnieje wiele połączeń sieciowych jednoczesnego zamiast jednej.
- Wiele operacji na jednej tabeli może spowodować konfliktu i blokowanie.
- Istnieją koszty ogólne skojarzone z wielowątkowość.
- Wydatki o otwarcie wielu połączeń przewyższa korzyści, jakie przetwarzanie równoległe.

Jeśli możesz współpracować z różnych tabel lub baz danych, jest umożliwia wyświetlanie wydajności uzyskać z tej strategii. Sharding bazy danych lub federacje będzie scenariuszu dla tej metody. Sharding używa wielu baz danych i kieruje innych danych do każdej bazy danych. Jeśli każdej małych partii ma inną bazę danych, następnie operacji równolegle można zwiększyć jego wydajność. Wzmocnienia wydajności jest istotne za pomocą podstawę dla decyzji sharding bazy danych za pomocą rozwiązania.

W niektórych projektów równoległego mniejszych partii może spowodować ulepszone przepustowość żądania w układzie obciążeniu. W tym przypadku mimo że jest szybsze przetwarzania jednej partii większe, przetwarzanie wielu partii równolegle może być bardziej efektywne.

Jeśli używasz równoległego, należy rozważyć, czy sterowanie maksymalna liczba wątków. Mniejsza liczba może spowodować mniej konfliktu i krótszy czas realizacji. Rozważ też dodatkowe obciążenie, które w ten sposób w docelowej bazie danych zarówno w oknie połączenia i transakcje.

### <a name="related-performance-factors"></a>Czynniki wydajności pokrewne
Typowe wskazówki dotyczące wydajności bazy danych wpływa również na tworzeniu partii. Na przykład wstawić wydajność jest ograniczona do tabel, które mają duże klucza podstawowego lub wiele zbudowania indeksów.

Jeśli zwracających tabelę parametrów użyć procedury składowanej, służy polecenie **SET NOCOUNT ON** na początku procedury. Tej instrukcji pomija zwrotu liczba wierszy dotyczy w procedurze. Jednak w testów, użyj **SET NOCOUNT ON** nie miała wpływu albo obniżenie wydajności. Procedura przechowywana badania została prosty z jednym **Wstawianie** polecenia z parametru zwracających tabelę. Istnieje możliwość, że bardziej złożone procedur składowanych będzie korzystać z tej instrukcji. Ale nie przyjęto założenie, że automatyczne dodawanie **SET NOCOUNT ON** do Twojej procedura składowana zwiększa wydajność. Aby uzyskać efekt, przetestuj procedura składowana z i bez instrukcji **SET NOCOUNT ON** .

## <a name="batching-scenarios"></a>Tworzeniu partii scenariuszy
W poniższych sekcjach opisano sposób użycia parametrów zwracających tabelę w trzy scenariusze aplikacji. Pierwszy scenariusz pokazano, jak buforowania i tworzeniu partii można wspólnie pracować. Drugi scenariusz zwiększa wydajność, wykonując operacje szczegółów wzorca w trakcie rozmowy pojedynczej procedury składowanej. Scenariusz ostateczne pokazano, jak używać zwracających tabelę parametrów w operacji "UPSERT".

### <a name="buffering"></a>Buforowanie
Mimo że istnieje kilka scenariuszy, które są oczywiste candidate tworzeniu partii, istnieje wiele scenariuszy, które można wykorzystać tworzeniu partii przez przetwarzanie opóźnione. Przetwarzanie opóźnione przenosi także większe ryzyko, że dane zostaną utracone w przypadku wystąpił nieoczekiwany błąd. Ważne zrozumieć ten czynnik ryzyka i skutki należy rozważyć, czy jest.

Na przykład można rozważyć aplikacji sieci web, która śledzi historię nawigacji każdego użytkownika. Dla każdego żądania strony aplikację można utworzyć połączenie do rejestrowania widoku użytkownika bazy danych. Ale wyższa wydajność i skalowalność można osiągnąć przez buforowanie działania nawigacji użytkowników, a następnie wysłać te dane do bazy danych w seriach. Może wyzwolić aktualizacji bazy danych przez czas i/lub rozmiaru. Na przykład reguły można określić, że partia powinny być przetwarzane po 20 sekund lub gdy bufor osiągnie 1000 elementów.

W poniższym przykładzie użyto [Reaktywne rozszerzenia - odbierania](https://msdn.microsoft.com/data/gg577609) przetwarzania buforowane zdarzenia generowane przez monitorowania zajęć. Wstawia buforu lub w osiągnięciu limitu czasu, partii danych użytkownika są wysyłane do bazy danych z parametrem zwracających tabelę.

Poniższa klasa NavHistoryData modele szczegóły nawigacji użytkownika. Zawiera podstawowe informacje, takie jak identyfikator użytkownika, adres URL dostępne i czas dostępu.

    public class NavHistoryData
    {
        public NavHistoryData(int userId, string url, DateTime accessTime)
        { UserId = userId; URL = url; AccessTime = accessTime; }
        public int UserId { get; set; }
        public string URL { get; set; }
        public DateTime AccessTime { get; set; }
    }

Klasy NavHistoryDataMonitor jest odpowiedzialny za buforowanie danych nawigacji użytkownika do bazy danych. Zawiera metodę RecordUserNavigationEntry, która odpowiada przez zwiększenie zdarzenia **OnAdded** . Poniższy kod zawiera logiki konstruktora, która umożliwia tworzenie zbiór widocznych oparte na zdarzenie odbierania. Go następnie subskrybowanie kolekcji widocznych przy użyciu metody buforu. Przeciążeń Określa, że bufor powinny być wysyłane co 20 sekund lub 1000 wpisów.

    public NavHistoryDataMonitor()
    {
        var observableData =
            Observable.FromEventPattern<NavHistoryDataEventArgs>(this, "OnAdded");
    
        observableData.Buffer(TimeSpan.FromSeconds(20), 1000).Subscribe(Handler);           
    }

Program obsługi konwertuje wszystkie buforowane elementy typu zwracających tabelę, a następnie przekazuje ten typ procedury składowanej przetwarzający partii. Poniższy kod przedstawia definicję wykonane zarówno NavHistoryDataEventArgs, jak i klasy NavHistoryDataMonitor.

    public class NavHistoryDataEventArgs : System.EventArgs
    {
        public NavHistoryDataEventArgs(NavHistoryData data) { Data = data; }
        public NavHistoryData Data { get; set; }
    }
    
    public class NavHistoryDataMonitor
    {
        public event EventHandler<NavHistoryDataEventArgs> OnAdded;
    
        public NavHistoryDataMonitor()
        {
            var observableData =
                Observable.FromEventPattern<NavHistoryDataEventArgs>(this, "OnAdded");
    
            observableData.Buffer(TimeSpan.FromSeconds(20), 1000).Subscribe(Handler);           
        }
    
        public void RecordUserNavigationEntry(NavHistoryData data)
        {    
            if (OnAdded != null)
                OnAdded(this, new NavHistoryDataEventArgs(data));
        }
    
        protected void Handler(IList<EventPattern<NavHistoryDataEventArgs>> items)
        {
            DataTable navHistoryBatch = new DataTable("NavigationHistoryBatch");
            navHistoryBatch.Columns.Add("UserId", typeof(int));
            navHistoryBatch.Columns.Add("URL", typeof(string));
            navHistoryBatch.Columns.Add("AccessTime", typeof(DateTime));
            foreach (EventPattern<NavHistoryDataEventArgs> item in items)
            {
                NavHistoryData data = item.EventArgs.Data;
                navHistoryBatch.Rows.Add(data.UserId, data.URL, data.AccessTime);
            }
    
            using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
            {
                connection.Open();
    
                SqlCommand cmd = new SqlCommand("sp_RecordUserNavigation", connection);
                cmd.CommandType = CommandType.StoredProcedure;
    
                cmd.Parameters.Add(
                    new SqlParameter()
                    {
                        ParameterName = "@NavHistoryBatch",
                        SqlDbType = SqlDbType.Structured,
                        TypeName = "NavigationHistoryTableType",
                        Value = navHistoryBatch,
                    });
    
                cmd.ExecuteNonQuery();
            }
        }
    }

Aby użyć tej klasy buforowania, aplikacja tworzy statycznego obiektu NavHistoryDataMonitor. Zawsze użytkownik uzyskuje dostęp do strony, aplikacja wywołuje metodę NavHistoryDataMonitor.RecordUserNavigationEntry. Logika buforowania przechodzi do przejmują wysyłania te wpisy do bazy danych w seriach.

### <a name="master-detail"></a>Szczegóły wzorca
Parametry zwracających tabelę są przydatne w przypadku proste scenariusze Wstawianie. Jednak może być trudniejsze wstawia partii, które wymagają więcej niż jednej tabeli. Scenariusz "wzorca/szczegółu" jest dobrym przykładem. W głównym tabeli przedstawiono Encja podstawowa. Jedną lub więcej tabel szczegółów zawierają więcej danych o jednostce. W tym scenariuszu relacje kluczy obcych Wymuszaj relację szczegółów unikatowe obiektu wzorca. Należy rozważyć uproszczoną wersję tabeli PurchaseOrder i jego skojarzony tabeli OrderDetail. Języku Transact-SQL tworzy tabelę PurchaseOrder z czterema kolumnami: IDZamówienia, OrderDate Id_klienta i stan.

    CREATE TABLE [dbo].[PurchaseOrder](
    [OrderID] [int] IDENTITY(1,1) NOT NULL,
    [OrderDate] [datetime] NOT NULL,
    [CustomerID] [int] NOT NULL,
    [Status] [nvarchar](50) NOT NULL,
     CONSTRAINT [PrimaryKey_PurchaseOrder] 
    PRIMARY KEY CLUSTERED ( [OrderID] ASC ))

Poszczególnych zamówień zawiera co najmniej jeden produktu. Te informacje są przechwytywane w tabeli PurchaseOrderDetail. Języku Transact-SQL tworzy tabelę PurchaseOrderDetail o pięć kolumn: IDZamówienia, OrderDetailID ProductID, UnitPrice i OrderQty.

    CREATE TABLE [dbo].[PurchaseOrderDetail](
    [OrderID] [int] NOT NULL,
    [OrderDetailID] [int] IDENTITY(1,1) NOT NULL,
    [ProductID] [int] NOT NULL,
    [UnitPrice] [money] NULL,
    [OrderQty] [smallint] NULL,
     CONSTRAINT [PrimaryKey_PurchaseOrderDetail] PRIMARY KEY CLUSTERED 
    ( [OrderID] ASC, [OrderDetailID] ASC ))

Kolumny Identyfikator zamówienia w tabeli PurchaseOrderDetail muszą odwoływać zamówienie z tabeli PurchaseOrder. Następujące definicji klucza obcego wymusza to ograniczenie.

    ALTER TABLE [dbo].[PurchaseOrderDetail]  WITH CHECK ADD 
    CONSTRAINT [FK_OrderID_PurchaseOrder] FOREIGN KEY([OrderID])
    REFERENCES [dbo].[PurchaseOrder] ([OrderID])

Aby można było używać parametrów zwracających tabelę, musi mieć jeden typ tabeli zdefiniowane przez użytkownika dla każdej tabeli docelowej.

    CREATE TYPE PurchaseOrderTableType AS TABLE 
    ( OrderID INT,
      OrderDate DATETIME,
      CustomerID INT,
      Status NVARCHAR(50) );
    GO
    
    CREATE TYPE PurchaseOrderDetailTableType AS TABLE 
    ( OrderID INT,
      ProductID INT,
      UnitPrice MONEY,
      OrderQty SMALLINT );
    GO

Następnie zdefiniuj procedura składowana akceptującym tabel tego typu. Ta procedura umożliwia aplikacji lokalnie partii zestawu zamówienia i Szczegóły zamówień w jednym połączenia. Deklaracji wykonania procedury przechowywanej w tym przykładzie zamówienia zakupu znajdują się w języku Transact-SQL.

    CREATE PROCEDURE sp_InsertOrdersBatch (
    @orders as PurchaseOrderTableType READONLY,
    @details as PurchaseOrderDetailTableType READONLY )
    AS
    SET NOCOUNT ON;
    
    -- Table that connects the order identifiers in the @orders
    -- table with the actual order identifiers in the PurchaseOrder table
    DECLARE @IdentityLink AS TABLE ( 
    SubmittedKey int, 
    ActualKey int, 
    RowNumber int identity(1,1)
    );
     
          -- Add new orders to the PurchaseOrder table, storing the actual
    -- order identifiers in the @IdentityLink table   
    INSERT INTO PurchaseOrder ([OrderDate], [CustomerID], [Status])
    OUTPUT inserted.OrderID INTO @IdentityLink (ActualKey)
    SELECT [OrderDate], [CustomerID], [Status] FROM @orders ORDER BY OrderID;
    
    -- Match the passed-in order identifiers with the actual identifiers
    -- and complete the @IdentityLink table for use with inserting the details
    WITH OrderedRows As (
    SELECT OrderID, ROW_NUMBER () OVER (ORDER BY OrderID) As RowNumber 
    FROM @orders
    )
    UPDATE @IdentityLink SET SubmittedKey = M.OrderID
    FROM @IdentityLink L JOIN OrderedRows M ON L.RowNumber = M.RowNumber;
    
    -- Insert the order details into the PurchaseOrderDetail table, 
          -- using the actual order identifiers of the master table, PurchaseOrder
    INSERT INTO PurchaseOrderDetail (
    [OrderID],
    [ProductID],
    [UnitPrice],
    [OrderQty] )
    SELECT L.ActualKey, D.ProductID, D.UnitPrice, D.OrderQty
    FROM @details D
    JOIN @IdentityLink L ON L.SubmittedKey = D.OrderID;
    GO

W tym przykładzie zdefiniowane lokalnie @IdentityLink tabela przechowuje rzeczywiste wartości IDZamówienia z nowo wstawionych wierszy. Identyfikatory te kolejności różnią się od wartości tymczasowe IDZamówienia w @orders i @details parametrów zwracających tabelę. Z tego powodu @IdentityLink tabeli połączy wartości IDZamówienia z @orders parametr rzeczywistych wartości IDZamówienia dla nowych wierszy w tabeli PurchaseOrder. Po wykonaniu tego kroku @IdentityLink tabeli ułatwia, wstawiając Szczegóły zamówień z rzeczywisty identyfikator zamówienia, która spełnia ograniczenia klucza obcego.

Ta procedura składowana można używać z kodem lub innych połączeń języku Transact-SQL. Zobacz sekcję zwracających tabelę parametrów w tym dokumencie, na przykład kod. Języku Transact-SQL pokazano, jak połączenie sp_InsertOrdersBatch.

    declare @orders as PurchaseOrderTableType
    declare @details as PurchaseOrderDetailTableType
    
    INSERT @orders 
    ([OrderID], [OrderDate], [CustomerID], [Status])
    VALUES(1, '1/1/2013', 1125, 'Complete'),
    (2, '1/13/2013', 348, 'Processing'),
    (3, '1/12/2013', 2504, 'Shipped')
    
    INSERT @details
    ([OrderID], [ProductID], [UnitPrice], [OrderQty])
    VALUES(1, 10, $11.50, 1),
    (1, 12, $1.58, 1),
    (2, 23, $2.57, 2),
    (3, 4, $10.00, 1)
    
    exec sp_InsertOrdersBatch @orders, @details

Takie rozwiązanie pozwala każdej partii korzystanie z zestawu wartości Identyfikator zamówienia, które rozpoczynają się od 1. Te wartości tymczasowe IDZamówienia pokazują relacje w partii, ale rzeczywista wartości IDZamówienia są określane w czasie operacji wstawiania. Można uruchomić te same instrukcje w poprzednim przykładzie wielokrotnie i generuje unikatowy zamówień w bazie danych. Z tego powodu Rozważ dodanie logiki więcej kod lub bazy danych, która zapobiega zduplikowanych zamówienia, gdy dzięki tworzeniu partii metoda.

W tym przykładzie pokazano, że bardziej złożone operacji bazy danych, takich jak wzorzec szczegółów operacji, może być przetwarzany wsadowo zgodnie z parametrami zwracających tabelę.

### <a name="upsert"></a>UPSERT
Inny scenariusz łączenia we wsady wymaga jednocześnie aktualizowania istniejących wierszy i wstawianie nowych wierszy. Operacja czasami nosi nazwę operacji "UPSERT" (Aktualizuj + insert). Zamiast oddzielnych połączeń aby WSTAWIAĆ i AKTUALIZOWAĆ instrukcji korespondencji seryjnej najlepiej nadaje się do tego zadania. Instrukcja korespondencji seryjnej można wykonywać zarówno Wstawianie i aktualizowania operacji w jednym połączenia.

Aby wykonać aktualizacje i wstawienia instrukcję korespondencji seryjnej można użyć zwracających tabelę parametrów. Na przykład można rozważyć uproszczone tabelę pracowników, która zawiera następujące kolumny: pole Identyfikator pracownika, imię, nazwisko, SocialSecurityNumber:

    CREATE TABLE [dbo].[Employee](
    [EmployeeID] [int] IDENTITY(1,1) NOT NULL,
    [FirstName] [nvarchar](50) NOT NULL,
    [LastName] [nvarchar](50) NOT NULL,
    [SocialSecurityNumber] [nvarchar](50) NOT NULL,
     CONSTRAINT [PrimaryKey_Employee] PRIMARY KEY CLUSTERED 
    ([EmployeeID] ASC ))
 
W tym przykładzie można użyć fakt, że SocialSecurityNumber jest unikatowy seryjnej wielu pracowników. Najpierw należy utworzyć tabeli zdefiniowane przez użytkownika:

    CREATE TYPE EmployeeTableType AS TABLE 
    ( Employee_ID INT,
      FirstName NVARCHAR(50),
      LastName NVARCHAR(50),
      SocialSecurityNumber NVARCHAR(50) );
    GO

Następnie utwórz procedura składowana lub wpisz kod, który używa instrukcji korespondencji seryjnej do wykonywania aktualizacji i wstawianie. W poniższym przykładzie użyto instrukcji korespondencji seryjnej na parametrem zwracających tabelę @employees, typu EmployeeTableType. Zawartość @employees tabeli nie są wyświetlane w tym miejscu.

    MERGE Employee AS target
    USING (SELECT [FirstName], [LastName], [SocialSecurityNumber] FROM @employees) 
    AS source ([FirstName], [LastName], [SocialSecurityNumber])
    ON (target.[SocialSecurityNumber] = source.[SocialSecurityNumber])
    WHEN MATCHED THEN 
    UPDATE SET
    target.FirstName = source.FirstName, 
    target.LastName = source.LastName
    WHEN NOT MATCHED THEN
       INSERT ([FirstName], [LastName], [SocialSecurityNumber])
       VALUES (source.[FirstName], source.[LastName], source.[SocialSecurityNumber]);

Aby uzyskać więcej informacji zobacz dokumentację i przykłady dotyczące instrukcji korespondencji seryjnej. Mimo że taką samą pracę można wykonać w przechowywane wielu krokach wywołanie procedury z oddzielaj Wstawianie, a operacje aktualizacji, instrukcja korespondencji seryjnej jest bardziej efektywne. Kod bazy danych można także konstruować wywołania języku Transact-SQL, które bezpośrednio bez konieczności dwa połączenia bazy danych dla INSERT i UPDATE należy użyć instrukcji korespondencji seryjnej.

## <a name="recommendation-summary"></a>Zalecenie podsumowania

Poniższa lista zawiera podsumowanie zaleceń łączenia we wsady omówione w tym temacie:

- Aby zwiększyć wydajność i skalowalność aplikacji bazy danych SQL za pomocą buforowania i tworzeniu partii.
- Zrozumienie kompromisów między tworzeniu partii buforowanie i elastyczność. Podczas awarii roli ryzyko utraty nieprzetworzonych partii kluczowych danych biznesowych może być większe niż zaletą wydajności tworzeniu partii.
- Próba zachować wszystkie połączenia z bazą danych w jednym centrum danych w celu zmniejszenia opóźnienie.
- Jeśli wybierzesz pojedyncza technika łączenia we wsady zwracających tabelę parametrów oferują najlepszą wydajność i elastyczność.
- Dla największą wydajność Wstawianie przestrzegaj następujących zasad ogólnych, ale testowanie rozwiązania:
    - Dla wierszy < 100 za pomocą jednego polecenia Wstaw parametryczne.
    - Dla wierszy < 1000 parametry zwracających tabelę.
    - Aby uzyskać > = 1000 wierszy, za pomocą SqlBulkCopy.
- Aby uzyskać aktualizowanie i usuwanie operacji, parametry zwracających tabelę z logiki procedura składowana, która określa prawidłowe działanie w każdym wierszu w parametrze tabeli.
- Informacje dotyczące rozmiaru partii:
    - Za pomocą największa rozmiarów partii, które miały znaczenie dla aplikacji i wymagań.
    - Saldo wzmocnienia wydajności dużych partii z ryzyka związanego z błędów tymczasowe lub losowych. Co to jest wynikiem ponowne próby lub utraty danych w partii? 
    - Testowanie najdłuższego partii do weryfikacji bazy danych SQL go nie odrzucić.
    - Utwórz ustawienia konfiguracji tego formantu tworzeniu partii, takich jak rozmiar partii lub buforowania przedziału czasu. Te ustawienia zapewniają elastyczność. Możesz zmienić zachowanie łączenia we wsady produkcji bez ponownego wdrożenia usługi w chmurze.
- Unikaj równoległego partii, które działają w jednej tabeli w jednej bazie danych. Jeśli chcesz podzielić jednej partii przez wiele wątków, uruchom testy w celu określenia idealny liczba wątków. Po próg nieokreślonych więcej wątków spowoduje zmniejszenie wydajności zamiast powiększyć ją.
- Należy rozważyć, czy buforowanie na rozmiar i czas jako sposób stosowania tworzeniu partii dla scenariuszy.

## <a name="next-steps"></a>Następne kroki

W tym artykule dotyczyła jak projekt bazy danych i kodowania technik związanych z tworzeniu partii ulepszyć Twojej aplikacji wydajność i skalowalność. Ale jest to tylko jeden współczynnik w ogólnej strategii. Więcej sposobów zwiększyć wydajność i skalowalność zobacz [wskazówki dotyczące wydajności bazy danych SQL Azure dla pojedynczego baz danych](sql-database-performance-guidance.md) i [ceny i wydajności zagadnienia związane z puli elastyczne bazy danych](sql-database-elastic-pool-guidance.md).
