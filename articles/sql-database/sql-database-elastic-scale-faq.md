<properties 
    pageTitle="Skala elastyczne Azure SQL — często zadawane pytania | Microsoft Azure" 
    description="Często zadawane pytania dotyczące skali elastyczne bazy danych Azure SQL." 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="ddove" 
    editor=""/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/03/2016" 
    ms.author="ddove"/>

# <a name="elastic-database-tools-faq"></a>Narzędzia elastyczne bazy danych — często zadawane pytania 

#### <a name="if-i-have-a-single-tenant-per-shard-and-no-sharding-key-how-do-i-populate-the-sharding-key-for-the-schema-info"></a>Jak wypełnić klucz sharding, aby uzyskać informacje o schemacie mając dzierżawy pojedyncze na shard i żaden klucz sharding?

Obiekt informacji schematu tylko umożliwia dzielenie scenariusze korespondencji seryjnej. Jeśli aplikacja jest założenia pojedyncze dzierżawy, nie wymaga narzędzie podziału korespondencji seryjnej, a więc istnieje potrzeba do wypełnienia obiektu informacje schematu.

#### <a name="ive-provisioned-a-database-and-i-already-have-a-shard-map-manager-how-do-i-register-this-new-database-as-a-shard"></a>Mam już obsługi administracyjnej bazy danych i Menedżera Map Shard masz jeszcze, jak zarejestrować tej nowej bazy danych jako shard?

Zobacz **[Dodawanie shard do aplikacji przy użyciu Biblioteka klienta elastyczne bazy danych](sql-database-elastic-scale-add-a-shard.md)**. 

#### <a name="how-much-do-elastic-database-tools-cost"></a>Ile kosztuje narzędzia elastyczne bazy danych?

Korzystanie z biblioteki klienta elastyczne bazy danych nie spowodować koszty. Tylko w przypadku baz danych Azure SQL, które możesz odłamki i Menedżera Map Shard, a także role sieci web i pracownik obsługi administracyjnej dla narzędzia scalanie podziału naliczania kosztów.

#### <a name="why-are-my-credentials-not-working-when-i-add-a-shard-from-a-different-server"></a>Dlaczego moje poświadczenia nie działają podczas dodawania shard z innego serwera?
Nie należy używać poświadczeń w postaci "użytkownika ID=username@servername”, po prostu użyć" identyfikator użytkownika = nazwa_użytkownika ".  Upewnij się również, że logowania "nazwa_użytkownika" ma uprawnienia na shard.

#### <a name="do-i-need-to-create-a-shard-map-manager-and-populate-shards-every-time-i-start-my-applications"></a>Należy utworzyć Menedżera Map Shard i wypełnij odłamki każdym uruchomieniu aplikacje?

Nie — tworzenie Menedżera Map Shard (na przykład **[ShardMapManagerFactory.CreateSqlShardMapManager](http://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.createsqlshardmapmanager.aspx)**) jest to operacja jednorazowa.  Aplikacja powinna używać połączenia **[ShardMapManagerFactory.TryGetSqlShardMapManager()](http://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.trygetsqlshardmapmanager.aspx)** podczas uruchamiania aplikacji.  Należy tylko jeden takiego połączenia dla domeny aplikacji.

#### <a name="i-have-questions-about-using-elastic-database-tools-how-do-i-get-them-answered"></a>Pytania dotyczące korzystania z narzędzia elastyczne bazy danych, jak mogę się ich odpowiedzi? 

Sprawdź bliższy kontakt z nami na [forum bazy danych SQL Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted).

#### <a name="when-i-get-a-database-connection-using-a-sharding-key-i-can-still-query-data-for-other-sharding-keys-on-the-same-shard--is-this-by-design"></a>Gdy uzyskać połączenie z bazą danych przy użyciu klucza sharding, mogę nadal dane kwerendy dla pozostałych kluczy sharding na tym samym shard.  Jest to zamierzone?

Elastyczne API skali umożliwiają połączenie jest właściwa baza danych dla klucza sharding, ale nie zawierają sharding klucza filtrowania.  Dodaj **miejsce, w którym** klauzule do zapytania w celu ograniczenia zakresu do klucza sharding przedstawionych w razie potrzeby.

#### <a name="can-i-use-a-different-azure-database-edition-for-each-shard-in-my-shard-set"></a>Czy można używać innej wersji Azure bazy danych dla każdej shard w zestawu shard

Tak, shard jest bazy danych pojedyncze, a więc jeden shard może być na wersję Premium, być innej wersji standardowej. Ponadto wersji shard można skalować w górę lub dół wiele razy w czasie trwania shard.

#### <a name="does-the-split-merge-tool-provision-or-delete-a-database-during-a-split-or-merge-operation"></a>Czy należy narzędzie podziału korespondencji seryjnej (lub usunąć) bazy danych podczas operacji dzielenie i scalanie? 

Wartość nie. **Dzielenie** operacji docelowej bazy danych, musi istnieć ze schematem odpowiednie i można zarejestrować przy użyciu Menedżera Map Shard.  Dla operacji **korespondencji seryjnej** możesz usunąć shard z Menedżera map shard, a następnie Usuń bazę danych.

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]
 