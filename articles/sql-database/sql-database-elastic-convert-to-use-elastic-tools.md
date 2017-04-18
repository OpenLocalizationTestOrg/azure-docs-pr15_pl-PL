<properties
   pageTitle="Migrowanie istniejącej bazy danych w celu poza skalowanie | Microsoft Azure"
   description="Konwertowanie sharded baz danych, tworząc shard menedżera map za pomocą narzędzi elastyczne bazy danych"
   services="sql-database"
   documentationCenter=""
   authors="ddove"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management"
   ms.date="10/24/2016"
   ms.author="ddove"/>

# <a name="migrate-existing-databases-to-scale-out"></a>Migrowanie istniejącej bazy danych w celu skala w nowym oknie

Łatwe zarządzanie istniejących skalowanej sharded baz danych za pomocą narzędzia bazy danych bazy danych SQL Azure (takie jak [Biblioteka klienta elastyczne bazy danych](sql-database-elastic-database-client-library.md)). Należy najpierw przekonwertować istniejący zestaw baz danych za pomocą [Menedżera mapowanie shard](sql-database-elastic-scale-shard-map-management.md). 

## <a name="overview"></a>Omówienie
Aby przeprowadzić migrację istniejącej bazy danych sharded: 

1. Przygotowywanie [bazy danych Menedżera mapy shard](sql-database-elastic-scale-shard-map-management.md).
2. Tworzenie mapy shard.
3. Przygotowywanie poszczególnych odłamki.  
2. Dodawanie mapowania do mapy shard.

Skorzystanie z tych metod może być realizowana za pomocą [biblioteki klienta .NET Framework](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/)lub skryptów programu PowerShell podanych w [DB SQL Azure - skryptów narzędzia elastyczne bazy danych](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db). Przykłady w tym miejscu za pomocą skryptów programu PowerShell.

Aby uzyskać więcej informacji o ShardMapManager zobacz [Shard mapowanie zarządzania](sql-database-elastic-scale-shard-map-management.md). Omówienie narzędzia elastyczne bazy danych zobacz [Omówienie funkcji elastyczne bazy danych](sql-database-elastic-scale-introduction.md).

## <a name="prepare-the-shard-map-manager-database"></a>Przygotowywanie shard Menedżera mapy z bazy danych

Menedżer mapy shard jest specjalne bazy danych, która zawiera dane, których chcesz zarządzać skalowanej baz danych. Możesz użyć istniejącej bazy danych lub Utwórz nową bazę danych. Należy zauważyć, że działają jako Menedżer mapy shard bazy danych nie powinny być tę samą bazę danych jako shard. Należy również zauważyć, że skrypt programu PowerShell nie utworzyć bazę danych dla Ciebie. 

## <a name="step-1-create-a-shard-map-manager"></a>Krok 1: tworzenie shard menedżera map

    # Create a shard map manager. 
    New-ShardMapManager -UserName '<user_name>' 
    -Password '<password>' 
    -SqlServerName '<server_name>' 
    -SqlDatabaseName '<smm_db_name>' 
    #<server_name> and <smm_db_name> are the server name and database name 
    # for the new or existing database that should be used for storing 
    # tenant-database mapping information.

### <a name="to-retrieve-the-shard-map-manager"></a>Aby pobrać menedżera map shard

Po utworzeniu można pobrać menedżera map shard przy użyciu tego polecenia cmdlet. Ten krok jest potrzebny za każdym razem, musisz użyć obiektu ShardMapManager.

    # Try to get a reference to the Shard Map Manager  
    $ShardMapManager = Get-ShardMapManager -UserName '<user_name>' 
    -Password '<password>' 
    -SqlServerName '<server_name>' 
    -SqlDatabaseName '<smm_db_name>' 

  
## <a name="step-2-create-the-shard-map"></a>Krok 2: Tworzenie mapy shard

Po zaznaczeniu typu shard mapa, aby utworzyć. Wybór zależy od architektura tej bazy danych: 

1. Pojedynczy dzierżawy na bazę danych (terminów, zobacz [Słownik](sql-database-elastic-scale-glossary.md)). 
2. Kilka dzierżaw na bazę danych (dwa typy):
    3. Mapowanie list
    4. Mapowanie zakresu
 

Dla modelu pojedyncze dzierżawy Utwórz mapę shard **Mapowanie list** . Model pojedynczego dzierżawy przypisuje jedną bazę danych na dzierżawcę. Jest to skuteczne modelu dla deweloperów władz akredytacji bezpieczeństwa go upraszcza zarządzanie.

![Mapowanie list][1]

Model wielu dzierżawy przypisuje kilka dzierżaw jednej bazie danych (i rozpowszechniania grup dzierżaw przez wiele baz danych). Za pomocą tego modelu można się spodziewać każdego dzierżawie potrzebują małych danych. W tym modelu możemy przypisać zakres dzierżaw bazy danych przy użyciu **mapowania zakresu**. 
 

![Mapowanie zakresu][2]

Lub można zaimplementować modelu bazy danych z wielu dzierżawy za pomocą *mapowania listy* , aby przypisać kilka dzierżaw do jednej bazie danych. Na przykład Degresywna 1 służy do przechowywania informacji na temat identyfikator dzierżawy 1 i 5 i DB2 są przechowywane dane dla dzierżawy 7 i dzierżawy 10. 

![Wielu dzierżaw na jednym bazy danych][3] 

**W zależności od wyboru, wybierz jedną z następujących opcji:**

### <a name="option-1-create-a-shard-map-for-a-list-mapping"></a>Opcja 1: Tworzenie mapy shard do mapowania listy
Tworzenie mapy shard przy użyciu obiektu ShardMapManager. 

    # $ShardMapManager is the shard map manager object. 
    $ShardMap = New-ListShardMap -KeyType $([int]) 
    -ListShardMapName 'ListShardMap' 
    -ShardMapManager $ShardMapManager 
 
 
### <a name="option-2-create-a-shard-map-for-a-range-mapping"></a>Opcja 2: Tworzenie mapy shard do mapowania zakresu

Należy zauważyć, że korzystanie z tego modelu mapowania, identyfikator dzierżawy musi być ciągły zakresy wartości i zaakceptować mają przerwy w zakresach, po prostu pomijanie zakres podczas tworzenia bazy danych.

    # $ShardMapManager is the shard map manager object 
    # 'RangeShardMap' is the unique identifier for the range shard map.  
    $ShardMap = New-RangeShardMap 
    -KeyType $([int]) 
    -RangeShardMapName 'RangeShardMap' 
    -ShardMapManager $ShardMapManager 

### <a name="option-3-list-mappings-on-a-single-database"></a>Opcja 3: Lista mapowania w jednej bazie danych
Tworzenie mapy listy również konfigurowania tego wzorca wymaga, jak pokazano w kroku 2, opcja 1.

## <a name="step-3-prepare-individual-shards"></a>Krok 3: Przygotowywanie poszczególne odłamki

Dodawanie poszczególnych shard (baza danych) do Menedżera mapy shard. To przygotowanie pojedyncze bazy danych do przechowywania informacji mapowania. Wykonanie tej metody można użyć w każdej shard.
     
    Add-Shard 
    -ShardMap $ShardMap 
    -SqlServerName '<shard_server_name>' 
    -SqlDatabaseName '<shard_database_name>'
    # The $ShardMap is the shard map created in step 2.
 

## <a name="step-4-add-mappings"></a>Krok 4: Dodawanie mapowania

Dodawanie mapowania zależy od rodzaju shard mapę, która została utworzona. Jeśli utworzono mapy listy, możesz dodać mapowania listy. Jeśli utworzono mapę zakres, możesz dodać mapowania zakresu.

### <a name="option-1-map-the-data-for-a-list-mapping"></a>Opcja 1: mapowania danych do mapowania listy

Mapowania danych przez dodanie mapowanie listy dla każdego dzierżawy.  

    # Create the mappings and associate it with the new shards 
    Add-ListMapping 
    -KeyType $([int]) 
    -ListPoint '<tenant_id>' 
    -ListShardMap $ShardMap 
    -SqlServerName '<shard_server_name>' 
    -SqlDatabaseName '<shard_database_name>' 

### <a name="option-2-map-the-data-for-a-range-mapping"></a>Opcja 2: mapowania danych do mapowania zakresu

Dodawanie mapowania zakresu wszystkie dzierżawy identyfikator zakresu — skojarzeń bazy danych:

    # Create the mappings and associate it with the new shards 
    Add-RangeMapping 
    -KeyType $([int]) 
    -RangeHigh '5' 
    -RangeLow '1' 
    -RangeShardMap $ShardMap 
    -SqlServerName '<shard_server_name>' 
    -SqlDatabaseName '<shard_database_name>' 


### <a name="step-4-option-3-map-the-data-for-multiple-tenants-on-a-single-database"></a>Krok 4 opcja 3: mapowania danych dla wielu dzierżaw w jednej bazie danych

Dla każdego dzierżawy, uruchom ListMapping Dodaj (opcja 1, powyżej). 


## <a name="checking-the-mappings"></a>Sprawdzanie mapowania

Informacje o istniejących odłamki i mapowania skojarzonych z nimi mogą być wysyłane kwerendy, korzystając z następujących poleceń:  

    # List the shards and mappings 
    Get-Shards -ShardMap $ShardMap 
    Get-Mappings -ShardMap $ShardMap 

## <a name="summary"></a>Podsumowanie

Po zakończeniu instalacji, możesz rozpocząć korzystanie z biblioteki klienta elastyczne bazy danych. Umożliwia także [zależne routing danych](sql-database-elastic-scale-data-dependent-routing.md) i [shard wielu kwerend](sql-database-elastic-scale-multishard-querying.md).

## <a name="next-steps"></a>Następne kroki


Pobieranie skryptów programu PowerShell z [sripts Narzędzia bazy danych elastyczne bazy danych SQL Azure](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).

Narzędzia są również na GitHub: [Azure i elastyczne db-tools](https://github.com/Azure/elastic-db-tools).

Aby przenieść dane z modelu wielu dzierżawy lub model jednej dzierżawy, użyj narzędzia do korespondencji seryjnej podziału. Zobacz [Dzielenie narzędzie korespondencji seryjnej](sql-database-elastic-scale-get-started.md).

## <a name="additional-resources"></a>Dodatkowe zasoby

Aby uzyskać informacji na temat typowych wzorców Architektura danych aplikacji bazy danych (władz akredytacji bezpieczeństwa) oprogramowania jako usługi wielu dzierżawy zobacz [Desenie projektu dla wielu dzierżawy władz akredytacji bezpieczeństwa aplikacji z bazy danych SQL Azure](sql-database-design-patterns-multi-tenancy-saas-applications.md).

## <a name="questions-and-feature-requests"></a>Pytania i funkcji

Pytania należy bliższy kontakt z nami na [forum bazy danych SQL](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted) i funkcji, dodaj je do [forum opinii bazy danych SQL](https://feedback.azure.com/forums/217321-sql-database/).

<!--Image references-->
[1]: ./media/sql-database-elastic-convert-to-use-elastic-tools/listmapping.png
[2]: ./media/sql-database-elastic-convert-to-use-elastic-tools/rangemapping.png
[3]: ./media/sql-database-elastic-convert-to-use-elastic-tools/multipleonsingledb.png
 
