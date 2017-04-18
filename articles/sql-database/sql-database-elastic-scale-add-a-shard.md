<properties 
    pageTitle="Dodawanie shard za pomocą narzędzia bazy danych elastyczne | Microsoft Azure" 
    description="Dodawanie nowego odłamki do shard za pomocą elastyczne skali interfejsy API zestawu." 
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
    ms.date="05/27/2016" 
    ms.author="ddove"/>

# <a name="adding-a-shard-using-elastic-database-tools"></a>Dodawanie shard za pomocą narzędzia elastyczne bazy danych

## <a name="to-add-a-shard-for-a-new-range-or-key"></a>Aby dodać shard do nowego zakresu lub klucza  

Aplikacje często konieczne jest po prostu Dodaj nowe odłamki obsługiwać dane, które jest planowane z nowych kluczy lub klucza zakresów, mapy shard, który już istnieje. Na przykład aplikacji sharded według identyfikatorów dzierżawy może być konieczne obsługi administracyjnej nowych shard nowej dzierżawy lub co miesiąc sharded danych może być konieczne nowe shard obsługi administracyjnej przed rozpoczęciem każdego nowego miesiąca. 

Jeśli nowy zakres wartości klucza nie jest już częścią istniejącego mapowania, jest bardzo proste dodać nowy shard i skojarzyć nowy klucz lub zakresu do tego shard. 

### <a name="example--adding-a-shard-and-its-range-to-an-existing-shard-map"></a>Przykład: Dodawanie shard i jego zakres do istniejącej mapy shard
W tym przykładzie użyto [TryGetShard](https://msdn.microsoft.com/library/azure/dn823929.aspx) [CreateShard](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.createshard.aspx)metod [CreateRangeMapping](https://msdn.microsoft.com/library/azure/dn807221.aspx#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.RangeShardMap`1.CreateRangeMapping(Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.RangeMappingCreationInfo{`0})) i tworzy wystąpienie klasy [ShardLocation](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardlocation.shardlocation.aspx#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.ShardLocation.) . W przykładzie poniżej utworzono bazy danych o nazwie **sample_shard_2** i wszystkie obiekty schematu niezbędne umieszczona go do przechowywania zakres [300, 400).  

    // sm is a RangeShardMap object.
    // Add a new shard to hold the range being added. 
    Shard shard2 = null; 

    if (!sm.TryGetShard(new ShardLocation(shardServer, "sample_shard_2"),out shard2)) 
    { 
        shard2 = sm.CreateShard(new ShardLocation(shardServer, "sample_shard_2"));  
    } 

    // Create the mapping and associate it with the new shard 
    sm.CreateRangeMapping(new RangeMappingCreationInfo<long> 
                            (new Range<long>(300, 400), shard2, MappingStatus.Online)); 


Alternatywnie programu Powershell umożliwia tworzenie nowego Menedżera mapy Shard. Przykład jest dostępny [tutaj](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).
## <a name="to-add-a-shard-for-an-empty-part-of-an-existing-range"></a>Aby dodać shard pustą część istniejącego zakresu  

W niektórych przypadkach może masz już zamapowane zakresu shard i częściowo wypełniona z danymi, ale teraz nadchodzące dane mają być kierowany do różnych shard. Na przykład możesz shard według określonego zakresu dzień i ma już przydzielonego 50 dni na shard, ale w dniu 24 przyszłych dane mają być ziemi w różnych shard. Elastyczne bazy danych [korespondencji seryjnej podziału narzędzie](sql-database-elastic-scale-overview-split-and-merge.md) można wykonać tej operacji, ale jeśli przenoszenia danych nie jest konieczne (na przykład dane dla zakresu dni [25, 50), to znaczy, dnia 25 włącznie do 50 wyłączności, jeszcze nie istnieje) można wykonać to całkowicie bezpośrednio przy użyciu interfejsy API zarządzania mapy Shard.

### <a name="example-splitting-a-range-and-assigning-the-empty-portion-to-a-newly-added-shard"></a>Przykład: dzielenie zakresu i przypisywanie pustą część do nowo dodanej shard

Utworzono bazy danych o nazwie "sample_shard_2" i wszystkie obiekty schematu niezbędne umieszczona go.  

 
    // sm is a RangeShardMap object.
    // Add a new shard to hold the range we will move 
    Shard shard2 = null; 

    if (!sm.TryGetShard(new ShardLocation(shardServer, "sample_shard_2"),out shard2)) 
    { 
    
        shard2 = sm.CreateShard(new ShardLocation(shardServer, "sample_shard_2"));  
    } 

    // Split the Range holding Key 25 

    sm.SplitMapping(sm.GetMappingForKey(25), 25); 

    // Map new range holding [25-50) to different shard: 
    // first take existing mapping offline 
    sm.MarkMappingOffline(sm.GetMappingForKey(25)); 
    // now map while offline to a different shard and take online 
    RangeMappingUpdate upd = new RangeMappingUpdate(); 
    upd.Shard = shard2; 
    sm.MarkMappingOnline(sm.UpdateMapping(sm.GetMappingForKey(25), upd)); 

**Ważne**: Użyj tej techniki, tylko jeśli masz pewność, że zakres zaktualizowane mapowanie jest pusta.  Powyższych metod nie należy zaznaczać dane dla zakresu przenoszony, więc warto uwzględnić testy w kodzie.  Jeśli istnieją wiersze w zakresie przenoszony, rzeczywistej dystrybucji danych nie będą zgodne mapy shard zaktualizowane. Użyj [Narzędzia korespondencji seryjnej podziału](sql-database-elastic-scale-overview-split-and-merge.md) operacji zamiast w takich przypadkach.  


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]
 
