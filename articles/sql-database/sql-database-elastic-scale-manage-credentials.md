<properties 
    pageTitle="Zarządzanie poświadczeń w bibliotece klienta elastyczne bazy danych | Microsoft Azure" 
    description="Jak ustawić odpowiedni poziom poświadczeń, administrator, aby tylko do odczytu, w przypadku aplikacji elastyczną bazy danych" 
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

# <a name="credentials-used-to-access-the-elastic-database-client-library"></a>Poświadczenia używane na dostęp do biblioteki klienta elastyczne bazy danych

[Biblioteka klienta elastyczne bazy danych](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/) używa trzech różnych rodzajów poświadczeń, aby otworzyć [Menedżera map shard](sql-database-elastic-scale-shard-map-management.md). W zależności od potrzeb za pomocą poświadczeń najniższego poziomu dostępu możliwe.

* **Zarządzanie poświadczeniami**: do tworzenia lub modyfikowania menedżera map shard. (Zobacz [Słownik](sql-database-elastic-scale-glossary.md)). 
* **Poświadczenia dostępu**: Aby uzyskać dostęp do istniejącego menedżera map shard celu uzyskania informacji na temat odłamki.
* **Poświadczenia połączenia**: Aby nawiązać połączenie odłamki. 

Zobacz też [Zarządzanie bazami danych i logowania w bazie danych SQL Azure](sql-database-manage-logins.md). 
 
## <a name="about-management-credentials"></a>Informacje o poświadczenia zarządzania

Zarządzanie poświadczenia są używane do tworzenia obiektu [**ShardMapManager**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx) dla aplikacji, które manipulowania shard mapy. (Na przykład, zobacz [Dodawanie shard za pomocą narzędzia bazy danych elastycznych](sql-database-elastic-scale-add-a-shard.md) i [zależnych rozsyłanie danych](sql-database-elastic-scale-data-dependent-routing.md)) Użytkownik Biblioteka klienta elastyczne skali tworzy SQL użytkowników i logowania SQL i zapewnia, że każdy otrzymuje uprawnienia do odczytu/zapisu bazy danych mapy shard globalnej i wszystkie także shard bazy danych.. Te poświadczenia są używane do obsługi mapy shard globalnej i mapy lokalne shard podczas zmiany do mapy shard są wykonywane. Na przykład Użyj poświadczeń zarządzania, aby utworzyć obiekt menedżera mapy shard (za pomocą [**GetSqlShardMapManager**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager.aspx): 

    // Obtain a shard map manager. 
    ShardMapManager shardMapManager = ShardMapManagerFactory.GetSqlShardMapManager( 
            smmAdminConnectionString, 
            ShardMapManagerLoadPolicy.Lazy 
    ); 

Zmienna **smmAdminConnectionString** jest ciąg połączenia, który zawiera poświadczenia zarządzania. Identyfikator użytkownika i hasło zawiera odczytu/zapisu dostęp do bazy danych mapy shard i poszczególnych odłamki. Parametry połączenia zarządzania także nazwa serwera oraz nazwę bazy danych bazy danych mapy shard globalnej. Oto parametry połączenia typowe w tym celu:

     "Server=<yourserver>.database.windows.net;Database=<yourdatabase>;User ID=<yourmgmtusername>;Password=<yourmgmtpassword>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;” 

Nie należy używać wartości w postaci "username@server"—instead po prostu użyć wartości "Nazwa użytkownika".  Jest to spowodowane poświadczenia muszą działa przed bazy danych Menedżera mapy shard i poszczególnych odłamki, które mogą okazać się na różnych serwerach.

## <a name="access-credentials"></a>Poświadczenia dostępu
  
Podczas tworzenia shard menedżera map w aplikacji, która nie administrować mapy shard, Użyj poświadczeń, które mają uprawnienia tylko do odczytu na mapie shard globalnej. Informacje pobierane z mapy shard globalnej w obszarze te poświadczenia są używane do [routingu zależne od danych](sql-database-elastic-scale-data-dependent-routing.md) oraz do zapełnienia pamięci podręcznej mapy shard na komputerze klienckim. Poświadczenia są dostępne za pośrednictwem takim samym wzorcem połączenia do **GetSqlShardMapManager** , jak pokazano powyżej: 

    // Obtain shard map manager. 
    ShardMapManager shardMapManager = ShardMapManagerFactory.GetSqlShardMapManager( 
            smmReadOnlyConnectionString, 
            ShardMapManagerLoadPolicy.Lazy
    );  

Zwróć uwagę na zastosowanie **smmReadOnlyConnectionString** tak, aby odzwierciedlała używanie różnych poświadczeń dostęp imieniu użytkowników **niebędących administratorami** : tych poświadczeń nie powinien zawierać uprawnienia do zapisu na mapie shard globalnym. 

## <a name="connection-credentials"></a>Poświadczenia połączenia 

Dodatkowe poświadczenia są wymagane, gdy przy użyciu metody [**OpenConnectionForKey**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkey.aspx) , aby uzyskać dostęp do shard skojarzonego z kluczem sharding. Te poświadczenia potrzebne uprawnienia dostępu tylko do odczytu i tabelami mapy lokalne shard znajdujących się na shard. Jest to potrzebne do wykonywania połączeń sprawdzania poprawności dla zależne od danych routingu na shard. Ten wstawkę kodu umożliwia dostęp do danych w zakresie danych zależne routingu: 
 
    using (SqlConnection conn = rangeMap.OpenConnectionForKey<int>( 
    targetWarehouse, smmUserConnectionString, ConnectionOptions.Validate)) 

W tym przykładzie **smmUserConnectionString** zawiera parametry połączenia za pomocą poświadczeń użytkownika. Dla bazy danych SQL Azure Oto ciąg połączenia typowe dla poświadczeń użytkownika: 

    "User ID=<yourusername>; Password=<youruserpassword>; Trusted_Connection=False; Encrypt=True; Connection Timeout=30;”  

Podobnie jak w przypadku poświadczeń administratora nie wartości w postaci "username@server". Zamiast tego można użyć "Nazwa użytkownika".  Należy również zauważyć, że parametry połączenia nie zawiera nazwę serwera i nazwę bazy danych. Jest tak, ponieważ wywołanie **OpenConnectionForKey** zostanie automatycznie przekierowany połączenie shard poprawne, oparte na kluczu. W związku z tym nazwa bazy danych i nazwa serwera nie są dostarczane. 

## <a name="see-also"></a>Zobacz też
[Zarządzanie bazami danych i logowania w bazie danych SQL Azure](sql-database-manage-logins.md)

[Zabezpieczanie bazy danych SQL](sql-database-security.md)

[Wprowadzenie do zadań elastyczne baz danych](sql-database-elastic-jobs-getting-started.md)

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]
 