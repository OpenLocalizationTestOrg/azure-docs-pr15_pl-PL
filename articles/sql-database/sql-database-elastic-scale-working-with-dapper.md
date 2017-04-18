<properties 
    pageTitle="Biblioteka klienta elastyczne bazy danych za pomocą Dapper | Microsoft Azure" 
    description="Biblioteka klienta elastyczne bazy danych za pomocą Dapper." 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="torsteng"/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="torsteng"/>

# <a name="using-elastic-database-client-library-with-dapper"></a>Biblioteka klienta elastyczne bazy danych za pomocą Dapper 

Ten dokument jest dla deweloperów, którzy korzystają z Dapper do tworzenia aplikacji, ale chcesz również obejmować [elastyczne bazy danych narzędzie](sql-database-elastic-scale-introduction.md) do tworzenia aplikacji implementujących sharding do poziomu ich dane w nowym oknie Skala.  Ten dokument przedstawia zmiany w aplikacji Dapper, które są niezbędne do integracji z narzędzia elastyczne bazy danych. Nasze fokus jest na redagowania zarządzania shard elastyczne bazy danych i zależne od danych routingu z Dapper. 

**Przykładowy kod**: [Narzędzia elastyczne bazy danych dla Azure SQL Database - Dapper integracji](https://code.msdn.microsoft.com/Elastic-Scale-with-Azure-e19fc77f).
 
Integrowanie **Dapper** i **DapperExtensions** z biblioteki klienta elastyczne bazy danych do bazy danych SQL Azure jest łatwe. Aplikacje korzystać zależne od, routing przez zmianę tworzenie i otwieranie nowych obiektów [SqlConnection](http://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) danych za pomocą połączenia [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) z [biblioteki klienta](http://msdn.microsoft.com/library/azure/dn765902.aspx). Ogranicza zmiany w aplikacji w celu tylko w przypadku, gdy do tworzenia i otworzyć nowe połączenia. 

## <a name="dapper-overview"></a>Omówienie dapper
**Dapper** jest obiekt relacyjnych mapowania. Mapowania obiekty .NET z aplikacji w relacyjnej bazie danych (i odwrotnie). Pierwsza część przykładowy kod przedstawia, jak można zintegrować Biblioteka klienta elastyczne bazy danych przy użyciu aplikacji Dapper. Druga część przykładowy kod pokazano, jak zintegrować podczas korzystania z zarówno Dapper, jak i DapperExtensions.  

Funkcję mapowania w Dapper zawiera metody rozszerzenia dotyczące połączenia z bazą danych, które uprościć przesyłając informacje instrukcji T SQL wykonanie lub kwerendy bazy danych. Na przykład Dapper ułatwia mapowanie między obiektów programu .NET i parametry połączenia **Wykonywanie** instrukcji SQL lub używanie wyników kwerendy SQL na obiekty .NET przy użyciu połączeń **kwerendy** z Dapper. 

Gdy używasz DapperExtensions, nie potrzebujesz już zapewnienie instrukcji SQL. Metody rozszerzenia, takie jak **GetList** lub **Wstawianie** przez połączenie z bazą danych utwórz instrukcji SQL w tle.
 
Kolejną zaletą Dapper, a także DapperExtensions jest, że aplikacja kontroluje tworzenie połączenia z bazą danych. Dzięki temu interakcję z biblioteki klienta elastyczne bazy danych, która połączenia oparte na mapowanie shardlets do baz danych z bazą danych brokerów.

Aby uzyskać Dapper Assembly, zobacz [Dapper kropka netto](http://www.nuget.org/packages/Dapper/). Aby uzyskać Dapper rozszerzenia zobacz [DapperExtensions](http://www.nuget.org/packages/DapperExtensions).

## <a name="a-quick-look-at-the-elastic-database-client-library"></a>Krótki przegląd Biblioteka klienta elastyczne bazy danych

Z biblioteki klienta elastyczne bazy danych Definiowanie partycje dane aplikacji o nazwie *shardlets* , zamapowanie ich do baz danych i zidentyfikowania klucze *sharding*. Może mieć dowolną liczbę baz danych i rozłożenie usługi shardlets w tych baz danych. Mapowanie wartości klucza sharding z bazami danych są przechowywane przez mapę shard dostarczony przez interfejsy API biblioteki. Ta funkcja jest określana mianem **shard mapowanie zarządzania**. Mapa shard służy także brokera połączenia z bazą danych dla żądań zawierających klucz sharding. Ta funkcja jest określane jako **zależne routingu danych**.

![Mapy shard i zależnych rozsyłanie danych][1]

Menedżer mapy shard uniemożliwia użytkownikom niespójne widoków w shardlet dane, które mogą wystąpić podczas operacji zarządzania równoczesne shardlet są wykonywanych na bazy danych. W tym celu mapy shard broker połączeń bazy danych dla aplikacji utworzonej z biblioteką. Podczas operacji zarządzania shard mogą mieć wpływ shardlet, dzięki funkcji mapy shard automatycznie zakończyć połączenie z bazą danych. 

Zamiast używać tradycyjnych sposobem utworzenia połączenia dla Dapper, należy użyć [metody OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn824099.aspx). Dzięki temu odbywa się sprawdzanie poprawności, a połączenia są zarządzane poprawnie po dowolnych danych powoduje przejście między odłamki.

### <a name="requirements-for-dapper-integration"></a>Wymagania dotyczące integracji Dapper

Podczas pracy z biblioteki klienta elastyczne bazy danych i Dapper interfejsy API, chcemy zachować następujące właściwości:

* **Scaleout**: chcemy Dodawanie i usuwanie baz danych w warstwie danych stosowania sharded stosownie do potrzeb możliwości stosowania. 

-    **Zgodność**: ponieważ nasze aplikacji skalowania przy użyciu sharding, trzeba wykonać zależne routingu danych. Chcemy zrobić za pomocą zależne możliwości routingu danych biblioteki. W szczególności chcemy zachować sprawdzania poprawności i spójność gwarantuje, dostarczony przez połączenia, które są brokered za pomocą Menedżera mapy shard, aby uniknąć uszkodzenia lub nieprawidłowe wyniki. Dzięki temu, że połączenia z danym shardlet są odrzucone lub zatrzymane, jeśli (na przykład) shardlet obecnie są przenoszone do różnych shard, za pomocą interfejsów API podziału-korespondencji seryjnej.

-    **Mapowanie obiektu**: chcemy zachować wygody mapowania dostarczony przez Dapper na tłumaczenie między klas w aplikacji i podstawowej struktury bazy danych. 

Następująca sekcja zawiera wskazówki dotyczące te wymagania dotyczące aplikacji na podstawie **Dapper** i **DapperExtensions**.

## <a name="technical-guidance"></a>Wytyczne techniczne
### <a name="data-dependent-routing-with-dapper"></a>Zależne od, routing z Dapper danych 

Z Dapper aplikacja jest zazwyczaj odpowiedzialny za tworzenie i otwieranie połączenia z bazą danych źródłowych. Podany typ T przez aplikację, Dapper zwraca wyniki kwerendy z jako zbiory .NET typu T. Dapper wykonuje mapowanie z wierszy wynik T-SQL do obiektów typu T. Podobnie Dapper mapy obiekty .NET do wartości SQL lub parametrów instrukcji języka manipulowania danych. Dapper oferuje tę funkcję przy użyciu metody rozszerzenia na zwykły obiekt [SqlConnection](http://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) z bibliotek ADO .NET programu SQL Client. Połączenie SQL dla DDR zwracana przez elastyczne interfejsy API skali są również zwykłe obiektów [SqlConnection](http://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) . Pozwala na bezpośrednio przy użyciu rozszerzenia Dapper nad zwrócone przez API DDR Biblioteka klienta, jak również prostego połączenia programu SQL Client.

Uwagi te wprowadź proste korzystać z połączenia wykonaną za Biblioteka klienta elastyczne bazy danych pośrednictwem dla Dapper.

W tym przykładzie kodu (z próbki towarzyszące) przedstawia podejście miejsce, w którym klucz sharding jest dostarczany przez aplikację do biblioteki w celu broker połączenie prawym shard.   

    using (SqlConnection sqlconn = shardingLayer.ShardMap.OpenConnectionForKey(
                     key: tenantId1, 
                     connectionString: connStrBldr.ConnectionString, 
                     options: ConnectionOptions.Validate))
    {
        var blog = new Blog { Name = name };
        sqlconn.Execute(@"
                      INSERT INTO
                            Blog (Name)
                            VALUES (@name)", new { name = blog.Name }
                        );
    }

Połączenie [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) interfejsu API zastępuje domyślne tworzenie i otwieranie połączenia programu SQL Client. Połączenie [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) przyjmuje argumenty, które są wymagane do routingu zależne danych: 

-    Mapa shard, aby uzyskać dostęp do interfejsów routingu zależne danych
-    Klucz sharding do identyfikowania shardlet
-    Poświadczenia (nazwa użytkownika i hasło), aby nawiązać połączenie shard

Obiektu mapy shard tworzy połączenie shard, w której znajduje się shardlet klucza danej sharding. Klienta elastyczne bazy danych interfejsy API również oznaczać połączenia do wykonania jego gwarancje spójności. Ponieważ wywołanie [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) zwraca obiekt zwykłe połączenie programu SQL Client, kolejne wywołanie metody rozszerzenia **Wykonywanie** z Dapper wykonuje standardowy Dapper sesji ćwiczeń.

Bardzo podobny sposób pracy kwerend — najpierw otwórz połączenie za pomocą [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) w kliencie interfejsu API. Następnie użyj metody zwykła rozszerzenia Dapper mapować wyników zapytania SQL do obiekty .NET:

    using (SqlConnection sqlconn = shardingLayer.ShardMap.OpenConnectionForKey(
                    key: tenantId1, 
                    connectionString: connStrBldr.ConnectionString, 
                    options: ConnectionOptions.Validate ))
    {    
           // Display all Blogs for tenant 1
           IEnumerable<Blog> result = sqlconn.Query<Blog>(@"
                                SELECT * 
                                FROM Blog
                                ORDER BY Name");

           Console.WriteLine("All blogs for tenant id {0}:", tenantId1);
           foreach (var item in result)
           {
                Console.WriteLine(item.Name);
            }
    }

Należy zauważyć, że **przy użyciu** blok z połączenia DDR zakresy wszystkie operacje bazy danych w obrębie bloku do jednego shard, gdzie są przechowywane tenantId1. Kwerenda zwraca tylko blogów przechowywane w bieżącym shard, ale nie odpowiednie elementy, które są przechowywane na innych odłamki. 

## <a name="data-dependent-routing-with-dapper-and-dapperextensions"></a>Dane zależne routingu z Dapper i DapperExtensions

Dapper zawiera ekosystem dodatkowe rozszerzenia, które umożliwiają dalszej wygody i abstrakcji z bazy danych, podczas tworzenia bazy danych aplikacji. DapperExtensions jest na przykład. 

W aplikacji przy użyciu DapperExtensions nie zmienia sposób tworzenia i zarządzania nimi połączenia z bazą danych. Nadal jest zobowiązań aplikacji, aby otworzyć okno połączenia i zwykłe obiekty połączeń programu SQL Client przewidywań metodami rozszerzenia. Firma Microsoft może zależeć [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) zgodnie z powyższym opisem. Jak wyświetlić następujące przykłady kodu, jedyną zmianą jest firma Microsoft nie jest już pisać instrukcje T SQL:

    using (SqlConnection sqlconn = shardingLayer.ShardMap.OpenConnectionForKey(
                    key: tenantId2, 
                    connectionString: connStrBldr.ConnectionString, 
                    options: ConnectionOptions.Validate))
    {
           var blog = new Blog { Name = name2 };
           sqlconn.Insert(blog);
    }

A Oto przykładowy kod dla zapytania: 

    using (SqlConnection sqlconn = shardingLayer.ShardMap.OpenConnectionForKey(
                    key: tenantId2, 
                    connectionString: connStrBldr.ConnectionString, 
                    options: ConnectionOptions.Validate))
    {
           // Display all Blogs for tenant 2
           IEnumerable<Blog> result = sqlconn.GetList<Blog>();
           Console.WriteLine("All blogs for tenant id {0}:", tenantId2);
           foreach (var item in result)
           {
               Console.WriteLine(item.Name);
           }
    }

### <a name="handling-transient-faults"></a>Obsługa przejściowych błędów

Zespół Patterns firmy Microsoft i wskazówki dotyczące opublikowanych [Blok aplikacji obsługi błędów przejściowych](http://msdn.microsoft.com/library/hh680934.aspx) ułatwiające deweloperów aplikacji zmniejszeniu typowych warunków przejściowych błędów podczas uruchamiania w chmurze. Aby uzyskać więcej informacji, zobacz [Perseverance, tajny wszystkie sukcesy: za pomocą funkcji blokowania przejściowych aplikacji obsługi błędów](http://msdn.microsoft.com/library/dn440719.aspx).

Przykładowy kod zależy od biblioteki przejściowych błędów w celu ochrony przed przejściowych błędów. 

    SqlDatabaseUtils.SqlRetryPolicy.ExecuteAction(() =>
    {
       using (SqlConnection sqlconn = 
          shardingLayer.ShardMap.OpenConnectionForKey(tenantId2, connStrBldr.ConnectionString, ConnectionOptions.Validate))
          {
              var blog = new Blog { Name = name2 };
              sqlconn.Insert(blog);
          }
    });

**SqlDatabaseUtils.SqlRetryPolicy** w kodzie powyżej jest definiowana jako **SqlDatabaseTransientErrorDetectionStrategy** ponów próbę liczba 10 i 5 sekund czas oczekiwania między kolejnymi próbami. Jeśli korzystasz z transakcji, upewnij się, że zakres ponów próbę powrót do początku transakcji w przypadku przejściowych błędów.

## <a name="limitations"></a>Ograniczenia

Metody opisane w niniejszym dokumencie kilka ograniczeń z:

* Przykładowy kod dla tego dokumentu nie przedstawiają sposoby zarządzania schematu w odłamki.
* Podane żądanie, przyjęto założenie, że jego przetwarzania bazy danych znajduje się w jednym shard określonych klucza sharding dostarczonego przez żądanie. Jednak to założenie nie zawsze przytrzymaj klawisz, na przykład, gdy nie jest możliwe udostępnienie klucz sharding. Aby rozwiązać ten, Biblioteka klienta elastyczne bazy danych zawiera [klasy MultiShardQuery](http://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardexception.aspx). Klasy wykonuje abstrakcji połączenia do badania nad kilkoma odłamki. Za pomocą MultiShardQuery w połączeniu z Dapper wykracza poza zakres tego dokumentu.

## <a name="conclusion"></a>Wnioski

Aplikacji przy użyciu Dapper i DapperExtensions łatwo mogą korzystać z narzędzia elastyczne bazy danych do bazy danych SQL Azure. Kroki opisane w niniejszym dokumencie te aplikacje korzystać możliwości narzędzia zależne routingu przez zmianę tworzenie i otwieranie nowych obiektów [SqlConnection](http://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) danych za pomocą połączenia [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) biblioteki klienta elastyczne bazy danych. Ogranicza to zmian aplikacji wymaganych do tych miejscach, w którym utworzony i otworzyć nowe połączenia. 

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-working-with-dapper/dapperimage1.png
 