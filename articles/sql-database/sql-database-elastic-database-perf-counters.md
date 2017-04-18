<properties
    pageTitle="Liczniki wydajności Menedżera map shard"
    description="ShardMapManager klas i danych zależne routingu liczniki wydajności"
    services="sql-database"
    documentationCenter=""
    manager="jhubbard"
    authors="SilviaDoomra"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/23/2016"
    ms.author="SilviaDoomra"/>

# <a name="performance-counters-for-shard-map-manager"></a>Liczniki wydajności dla menedżera map shard

Wydajność [Menedżera map shard](sql-database-elastic-scale-shard-map-management.md), można przechwycić, zwłaszcza w przypadku, gdy korzystania z [danych zależne routingu](sql-database-elastic-scale-data-dependent-routing.md). Liczniki są tworzone przy użyciu metody klasy Microsoft.Azure.SqlDatabase.ElasticScale.Client.  

Liczniki są używane do śledzenia wydajności operacji [zależnych rozsyłanie danych](sql-database-elastic-scale-data-dependent-routing.md) . Te liczniki są dostępne w Monitorze wydajności, w kategorii "Zarządzanie bazą danych Shard: elastyczne".

**Najnowszą wersję:** Przejdź do [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/). Zobacz też [uaktualnić aplikację przy użyciu najnowszych biblioteki klienta elastyczne bazy danych](sql-database-elastic-scale-upgrade-client-library.md).

## <a name="prerequisites"></a>Wymagania wstępne

* Aby utworzyć kategorii wydajności i liczniki, użytkownik musi być częścią lokalnej grupy **Administratorzy** na komputer, na którym aplikacji.  

* Aby utworzyć wystąpienia licznika wydajności i zaktualizować liczniki, użytkownik musi być członkiem grupy **Administratorzy** lub **Użytkownicy monitora wydajności** . 

## <a name="create-performance-category-and-counters"></a>Tworzenie kategorii wydajności i liczników 

Aby utworzyć liczniki, wywołaj metodę CreatePeformanceCategoryAndCounters [klasy ShardMapManagmentFactory](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.aspx). Tylko administrator może wykonać metodę: 

    ShardMapManagerFactory.CreatePerformanceCategoryAndCounters()  

Umożliwia także [tego](https://gallery.technet.microsoft.com/scriptcenter/Elastic-DB-Tools-for-Azure-17e3d283) skryptu programu PowerShell Aby wykonać metodę. Ta metoda tworzy następujące liczniki wydajności:  

* **Mapowania buforowana**: liczba mapowania pamięci podręcznej dla shard map.
*  **Operacje DDR na sekundę**: liczba zależne routingu operacje na danych mapy shard. Licznik jest aktualizowana podczas połączenia powoduje [OpenConnectionForKey()](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkey.aspx) połączenia z programem shard miejsca docelowego. 
*  **Mapowanie trafień w pamięci podręcznej wyszukiwania na sekundę**: liczba operacji wyszukiwania pomyślnego pamięci podręcznej dla mapowań na mapie shard. 
*  **Mapowanie Chybienia w pamięci podręcznej wyszukiwania na sekundę**: liczba operacji wyszukiwania nie powiodło się pamięci podręcznej dla mapowań na mapie shard.
*  **Mapowania dodane lub zaktualizowane w pamięci podręcznej na sekundę**: szybkość mapowania, które są dodawane lub zaktualizowana w pamięci podręcznej dla shard map. 
*  **Mapowania usuwane z pamięci podręcznej/s**: stawek, w którym mapowania są usuwane z pamięci podręcznej mapy shard. 

Liczniki wydajności są tworzone dla każdej mapy shard pamięci podręcznej na proces.  


## <a name="notes"></a>Notatki
Poniżej wymieniono zdarzenia wyzwolić tworzenia liczników wydajności:  

* Inicjowanie [ShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx) z [wprowadzającego ładowania](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerloadpolicy.aspx), jeśli ShardMapManager zawiera wszystkie mapy shard. Należą do [GetSqlShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager.aspx?f=255&MSPPError=-2147217396#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.ShardMapManagerFactory.GetSqlShardMapManager%28System.String,Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.ShardMapManagerLoadPolicy%29) i metody [TryGetSqlShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.trygetsqlshardmapmanager.aspx) .
* Wyszukiwanie pomyślnego mapy shard (za pomocą [GetShardMap()](https://msdn.microsoft.com/library/azure/dn824215.aspx), [GetListShardMap()](https://msdn.microsoft.com/library/azure/dn824212.aspx) lub [GetRangeShardMap()](https://msdn.microsoft.com/library/azure/dn824173.aspx)). 

* Utworzenia mapę shard CreateShardMap().

Wszystkie operacje pamięci podręcznej wykonywane na mapie shard i mapowania zostaną zaktualizowane liczników wydajności. Pomyślne usunięcie mapy shard usunięcie wystąpienia liczników wydajności za pomocą DeleteShardMap reults ().  

## <a name="best-practices"></a>Najważniejsze wskazówki

* Tworzenie kategorii wydajności i liczników należy wykonać tylko raz przed utworzeniem obiektu ShardMapManager. Co wykonanie polecenia CreatePerformanceCategoryAndCounters() Czyści poprzedniego liczniki (utraty danych przekazywanych przez wszystkich wystąpień) i tworzy nowe.  

* Wystąpienia liczników wydajności są tworzone na proces. Awarie aplikacji ani usuwania mapy shard z pamięci podręcznej spowoduje usunięcie wystąpienia liczników wydajności.  

### <a name="see-also"></a>Zobacz też

[Elastyczne Przegląd funkcji bazy danych](sql-database-elastic-scale-introduction.md)  

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Anchors-->
<!--Image references-->

