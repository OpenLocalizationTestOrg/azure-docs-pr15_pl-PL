< właściwości
    
    pageTitle="Upgrade to the latest elastic database client library | Microsoft Azure" 
    description="Upgrade apps and libraries using Nuget" 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="ddove"/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="ddove" />

# <a name="upgrade-an-app-to-use-the-latest-elastic-database-client-library"></a>Uaktualnienie aplikacji przy użyciu najnowszych biblioteki klienta elastyczne bazy danych

Nowe wersje [Biblioteka klienta elastyczne bazy danych](sql-database-elastic-database-client-library.md) są dostępne za pośrednictwem NuGetand interfejsu Menedżera NuGetPackage w programie Visual Studio. Uaktualnienie zawierają poprawki i pomocy technicznej dostępne dla nowych funkcji Biblioteka klienta.

**Najnowszą wersję:** Przejdź do [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/).

Odbudowywanie aplikacji z nowej biblioteki, a także zmienić istniejące metadane menedżera Map Shard przechowywanych w bazach danych programu SQL Azure o obsługę nowych funkcji.

Te kroki wykonywane w kolejności gwarantuje, że stare wersje Biblioteka klienta nie są już dostępne w środowisku po zaktualizowaniu obiektów metadanych, co oznacza, że obiekty metadanych starych wersji nie zostanie utworzona po uaktualnieniu.   

## <a name="upgrade-steps"></a>Kroki uaktualnienia

**1. uaktualnienia aplikacji.** W programie Visual Studio Pobierz i odwołanie najnowszą wersję biblioteki klienta do wszystkich projektów rozwoju korzystające z biblioteki; następnie odbudowanie i wdrażanie. 

 * Rozwiązania programu Visual Studio, wybierz pozycję **Narzędzia** --> **Menedżera pakietów NuGet** -->  **Zarządzanie pakietów NuGet rozwiązania**. 
 * (Visual Studio 2013) W panelu po lewej stronie wybierz **aktualizacje**, a następnie wybierz przycisk **Aktualizuj** na opakowaniu **Azure SQL bazy danych elastyczne skali Biblioteka klienta** , w której pojawia się w oknie.
 * (Visual Studio 2015) Ustaw **Uaktualnianie dostępne**pola filtru. Wybierz pakiet aktualizacji, a następnie kliknij przycisk **Aktualizuj** .
    
 
 * Tworzenie i wdrażanie. 

**2. Uaktualnij skryptów.** Jeśli używasz skryptów **programu PowerShell** do zarządzania odłamki, [Pobierz nową wersję biblioteki](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/) i skopiować go do katalogu, z którego wykonywanie skryptów. 

**3. uaktualnienie usługi korespondencji seryjnej podziału.** Jeśli używasz narzędzia korespondencji seryjnej podziału elastyczne bazy danych do dołu sharded danych, [pobrać i zainstalować najnowszą wersję narzędzia](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge/). Szczegółowe kroki uaktualnienia usługi można znaleźć [w tym miejscu](sql-database-elastic-scale-overview-split-and-merge.md). 

**4. uaktualnianie baz danych Menedżera Map Shard**. Uaktualnij metadane pomocniczych map Shard w bazie danych SQL Azure.  Istnieją dwa sposoby można to zrobić, przy użyciu programu PowerShell lub C#. Poniżej przedstawiono obu opcji.

***Opcja 1: Uaktualnianie metadanych przy użyciu programu PowerShell***

1. Pobierz najnowsze narzędzia wiersza polecenia dla NuGet [tutaj](http://nuget.org/nuget.exe) i Zapisz w folderze. 

2. Otwórz wiersz polecenia, przejdź do tego samego folderu, a wydaj polecenie:`nuget install Microsoft.Azure.SqlDatabase.ElasticScale.Client`

3. Przejdź do podfolderu zawierające nowa wersja klienta DLL, po prostu pobrany, na przykład:`cd .\Microsoft.Azure.SqlDatabase.ElasticScale.Client.1.0.0\lib\net45`

4. Pobierz scriptlet uaktualniania klienta elastyczne bazy danych z poziomu [Centrum skryptów](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-Database-Elastic-6442e6a9)i zapisz go do tego samego folderu zawierającego bibliotekę DLL.

5. Z tego folderu Uruchom "Programu PowerShell.\upgrade.ps1" w wierszu polecenia, a następnie postępuj zgodnie z instrukcjami.
 
***Opcja 2: Uaktualnianie metadanych za pomocą C#***

Możesz też utworzyć aplikację programu Visual Studio, która zostanie otwarta z ShardMapManager, wykonuje iterację na wszystkich odłamki i wykonuje uaktualnienie metadanych, dzwoniąc metod [UpgradeLocalStore](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.upgradelocalstore.aspx) i [UpgradeGlobalStore](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.upgradeglobalstore.aspx) , jak w poniższym przykładzie: 

    ShardMapManager smm =
       ShardMapManagerFactory.GetSqlShardMapManager
       (connStr, ShardMapManagerLoadPolicy.Lazy); 
    smm.UpgradeGlobalStore(); 
    
    foreach (ShardLocation loc in
     smm.GetDistinctShardLocations()) 
    {   
       smm.UpgradeLocalStore(loc); 
    } 

Techniki uaktualnienia metadanych można stosować wielokrotnie bez szkody. Na przykład jeśli starsza wersja klienta przypadkowo tworzy shard po zaktualizowaniu już, można uruchomić uaktualnienie ponownie przez wszystkich odłamki upewnij się, że najnowsza wersja metadanych ma infrastruktury informatycznej. 

**Uwaga:**  Nowe wersje Biblioteka klienta opublikowany do daty będą nadal działać we wcześniejszych wersjach programu metadanych Menedżera Map Shard na bazy danych SQL Azure i na odwrót.   Jednak aby móc skorzystać z niektórych nowych funkcji najnowszą wersję, metadanych wymaga uaktualnienia.   Należy zauważyć, że uaktualnienia metadanych nie wpływa na dane użytkownika ani dane specyficzne dla aplikacji, tylko obiekty tworzone i używane przez Menedżera Map Shard.  I aplikacje nadal działać przez sekwencji uaktualniania opisany powyżej. 

## <a name="elastic-database-client-version-history"></a>Historia wersji klienta elastyczne bazy danych 

Aby uzyskać historii wersji przejdź do [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/)


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]  


<!--Image references-->
[1]:./media/sql-database-elastic-scale-upgrade-client-library/nuget-upgrade.png
 