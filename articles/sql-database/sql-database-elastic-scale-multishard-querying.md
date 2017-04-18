<properties 
    pageTitle="Kwerenda wielu shard | Microsoft Azure" 
    description="Uruchamianie kwerend przez odłamki przy użyciu Biblioteka klienta elastyczne bazy danych." 
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
    ms.date="04/12/2016" 
    ms.author="torsteng"/>

# <a name="multi-shard-querying"></a>Kwerenda shard wielu elementów

## <a name="overview"></a>Omówienie

Przy użyciu [Narzędzia elastyczne bazy danych](sql-database-elastic-scale-introduction.md)możesz utworzyć rozwiązania bazodanowe sharded. **Kwerenda wielu shard** jest używana do zadań takich jak zbioru i raportowania danych, które wymagają, wykonywanie kwerendy, która rozciągnąć przez kilka odłamki. (Kontrast to do [kierowania zależne od danych](sql-database-elastic-scale-data-dependent-routing.md), który wykonuje całą pracę na jednym shard.) 

## <a name="overview"></a>Omówienie

1. Uzyskiwanie [**RangeShardMap**](https://msdn.microsoft.com/library/azure/dn807318.aspx) lub [**ListShardMap**](https://msdn.microsoft.com/library/azure/dn807370.aspx) przy użyciu [**TryGetRangeShardMap**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetrangeshardmap.aspx), [**TryGetListShardMap**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetlistshardmap.aspx)lub metodę [**GetShardMap**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.getshardmap.aspx) . Zobacz [**Konstruowanie ShardMapManager**](sql-database-elastic-scale-shard-map-management.md#constructing-a-shardmapmanager) i [**uzyskać RangeShardMap lub ListShardMap**](sql-database-elastic-scale-shard-map-management.md#get-a-rangeshardmap-or-listshardmap).
2. Tworzenie obiektu **[MultiShardConnection](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardconnection.aspx)** .
2. Tworzenie **[MultiShardCommand](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardcommand.aspx)**. 
3. Ustaw **[Właściwości CommandText](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardcommand.commandtext.aspx#P:Microsoft.Azure.SqlDatabase.ElasticScale.Query.MultiShardCommand.CommandText)** polecenia T-SQL.
3. Wykonywanie polecenia **[metody ExecuteReader](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardcommand.executereader.aspx)**.
4. Wyświetlanie wyników za pomocą **[klasy MultiShardDataReader](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multisharddatareader.aspx)**. 

## <a name="example"></a>Przykład

Poniższy kod ilustruje użycie wielu shard kwerendy przy użyciu danej **ShardMap** o nazwie *myShardMap*. 

    using (MultiShardConnection conn = new MultiShardConnection( 
                                        myShardMap.GetShards(), 
                                        myShardConnectionString) 
          ) 
    { 
    using (MultiShardCommand cmd = conn.CreateCommand())
           { 
            cmd.CommandText = "SELECT c1, c2, c3 FROM ShardedTable"; 
            cmd.CommandType = CommandType.Text; 
            cmd.ExecutionOptions = MultiShardExecutionOptions.IncludeShardNameColumn; 
            cmd.ExecutionPolicy = MultiShardExecutionPolicy.PartialResults; 

            using (MultiShardDataReader sdr = cmd.ExecuteReader()) 
                { 
                    while (sdr.Read())
                        { 
                            var c1Field = sdr.GetString(0); 
                            var c2Field = sdr.GetFieldValue<int>(1); 
                            var c3Field = sdr.GetFieldValue<Int64>(2);
                        } 
                } 
           } 
    } 

 
Kluczowe różnica polega na budowie shard wiele połączeń. W przypadku, gdy **SqlConnection** działa w jednej bazie danych, **MultiShardConnection** ma ***zbiór odłamki*** jako własnych danych wejściowych. Wypełnij zbiór odłamki z mapy shard. Następnie wykonać kwerendy, w zbiorze odłamki za pomocą **UNION ALL** znaczeń właściwych do zebrania pojedynczy wynik ogólny. Opcjonalnie można dodawać do wyników przy użyciu właściwości **ExecutionOptions** polecenie nazwę shard, z której pochodzi wiersz. 

Uwaga połączenie **myShardMap.GetShards()**. Ta metoda pobiera wszystkie odłamki z mapy shard, a także łatwy sposób, aby uruchomić kwerendę przez wszystkie odpowiednie bazy danych. Zbiór odłamki dla zapytania wielu shard może być Elegancja dodatkowo wykonując kwerendę LINQ na kolekcji zwrócony z połączenie **myShardMap.GetShards()**. W połączeniu z zasad częściowe wyniki opracowano bieżących możliwości wyszukiwania wielu shard sprawdza się w przypadku dziesiątki maksymalnie setki odłamki.

Ograniczenie z wielu shard kwerenda jest obecnie brak sprawdzania poprawności dla odłamki i shardlets, które będą proszeni. Gdy routingu zależne od danych sprawdza, czy dany shard część mapy shard w czasie wykonywania kwerend, shard wielu zapytań nie sprawdzaj. To może prowadzić do kwerendy wielu shard uruchamianie w bazach danych, które zostały usunięte z mapy shard.

## <a name="multi-shard-queries-and-split-merge-operations"></a>Shard wielu kwerend i operacji Scal podziału

Shard wielu zapytań zrobić Sprawdź, czy shardlets kwerendy bazy danych uczestniczą w trwających operacji Scal podziału. (Zobacz [Skalowanie przy użyciu narzędzia do korespondencji seryjnej podziału elastyczne bazy danych](sql-database-elastic-scale-overview-split-and-merge.md)). Może to prowadzić do niespójności miejsce, w którym wiersze z tym samym shardlet Pokaż dla wielu baz danych w tej samej kwerendzie wiele shard. Należy pamiętać o te ograniczenia i należy rozważyć, czy opróżniania trwających operacji Scal podziału i zmiany do mapy shard podczas wykonywania kwerend wielu shard.

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

## <a name="see-also"></a>Zobacz też
Klasy **[System.Data.SqlClient](http://msdn.microsoft.com/library/System.Data.SqlClient.aspx)** i metod.


Zarządzanie odłamki za pośrednictwem [biblioteki klienta elastyczne bazy danych](sql-database-elastic-database-client-library.md). Zawiera przestrzeń nazw o nazwie [Microsoft.Azure.SqlDatabase.ElasticScale.Query](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.aspx) , który umożliwia wielu odłamki przy użyciu jednego kwerendy i wyników kwerendy. Zapewnia on podczas badania abstrakcji przez zbiór odłamki. Umożliwia także zasady uruchamiania alternatywny, w szczególności częściowe wyniki, na zajęcie się błędy podczas badania nad wiele odłamki.  

 