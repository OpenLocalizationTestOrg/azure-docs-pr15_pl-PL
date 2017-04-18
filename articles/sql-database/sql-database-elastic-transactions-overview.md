<properties
   pageTitle="Transakcje rozproszone w chmurze baz danych"
   description="Przegląd transakcji elastyczne bazy danych z bazy danych Azure SQL"
   services="sql-database"
   documentationCenter=""
   authors="torsteng"
   manager="jhubbard"
   editor="torsteng"/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="sql-database"
   ms.date="05/27/2016"
   ms.author="torsteng"/>

# <a name="distributed-transactions-across-cloud-databases"></a>Transakcje rozproszone w chmurze baz danych

Transakcje elastyczne bazy danych do bazy danych SQL Azure (bazy danych SQL) pozwala na uruchamianie transakcje obejmujące wiele baz danych w bazie danych SQL. Transakcje elastyczne bazy danych do bazy danych SQL są dostępne dla aplikacji .NET przy użyciu ADO .NET i integracji z znajome środowisko programowania przy użyciu klas [System.Transaction](https://msdn.microsoft.com/library/system.transactions.aspx) . Aby wziąć udział w bibliotece, zobacz [.NET Framework 4.6.1 (Instalatora sieci Web)](https://www.microsoft.com/download/details.aspx?id=49981).

W środowisku lokalnym takim przypadku zazwyczaj wymagane działa transakcji Koordynator MSDTC (Microsoft Distributed). Ponieważ MSDTC nie jest dostępny dla aplikacji platformy jako usługi platformy Azure, możliwość koordynowanie transakcje rozproszone teraz zostały bezpośrednio wbudowane do bazy danych SQL. Aplikacje można nawiązać dowolną bazą danych SQL, aby uruchomić transakcje rozproszone, a jeden z bazy danych będzie przezroczysty będą dopasowane transakcji rozproszonej, jak pokazano na poniższym rysunku. 

  ![Transakcje rozproszone z bazy danych SQL Azure za pomocą transakcji elastyczne bazy danych ][1]

## <a name="common-scenarios"></a>Typowe scenariusze

Transakcje elastyczne bazy danych do bazy danych SQL umożliwiają aplikacjom Atomowej zmieniać dane przechowywane w kilku różnych baz danych SQL. Podgląd omówiono środowiska opracowywania po stronie klienta w C# i .NET. Środowisko po stronie serwera za pomocą T-SQL jest planowane później.  
Transakcje elastyczne bazy danych jest przeznaczony dla następujących scenariuszy:

* Aplikacje wiele baz danych platformy Azure: W tym scenariuszu danych jest pionowo podzielona przez kilka baz danych w bazie danych SQL tak, aby różnych rodzajów danych znajdują się na różnych baz danych. Niektóre operacje wymagają zmiany danych, który jest przechowywany w dwóch lub więcej baz danych. Aplikacja używa transakcje bazy danych elastyczne koordynowanie zmiany w bazach danych i upewnij się, niepodzielność.

* Aplikacje sharded baz danych platformy Azure: W tym scenariuszu warstwy danych korzysta z [biblioteki klienta elastyczne bazy danych](sql-database-elastic-database-client-library.md) lub sharding automatycznej na poziomie dzielenia danych na wiele baz danych w bazie danych SQL. Jeden przypadek użycia widocznym jest potrzebna do wykonania zmian częściowych sharded aplikacji wielu dzierżawy, gdy zmiany zakresu dzierżaw. Można traktować, na przykład przeniesienia z jednej dzierżawy do innego, oba znajdujących się na różnych baz danych. Drugim przypadku jest szerokiego sharding tak, aby zezwalały wymagana pojemność dużych dzierżawy, co z kolei zwykle oznacza, że niektóre operacje Atomowej należy rozciągnąć na kilka baz danych na potrzeby tej samej dzierżawy. Trzecia sprawa jest Atomowej aktualizacje, aby odwołać się dane, które są replikowane za pomocą bazy danych. Operacje Atomowej, transakcji, na tych wierszy można teraz koordynowany przez kilka baz danych przy użyciu preview.
Transakcje elastyczne bazy danych za pomocą dwuetapowa Zatwierdź zapewnienia niepodzielności transakcji całej bazy danych. Jest to dobry rozmiar transakcje dotyczące mniej niż 100 baz danych w czasie w ramach jednej transakcji. Limity te nie są wymuszane, ale jedną się spodziewać wydajności i stawki sukcesu transakcji bazy danych elastyczne istotnie, gdy przekraczają limity te.


## <a name="installation-and-migration"></a>Instalacji i migracji

Możliwości transakcji elastyczne bazy danych w bazie danych SQL są dostarczane za pomocą funkcji Aktualizacje do bibliotek .NET System.Data.dll i System.Transactions.dll. Gdy upewnij się, że tego dwuetapowa Zatwierdź jest używana w przypadku gdy jest to niezbędne w celu zapewnienia niepodzielności. Aby rozpocząć tworzenie aplikacji przy użyciu transakcji elastyczne bazy danych, należy zainstalować [.NET Framework 4.6.1](https://www.microsoft.com/download/details.aspx?id=49981) lub nowszej wersji. Zainstalowanego w starszej wersji programu .NET framework transakcji zakończy się niepowodzeniem zapewnić promocję do transakcji rozproszonej i będzie podniesioną wyjątek.

Po zakończeniu instalacji można transakcji rozproszonej interfejsy API w System.Transactions z połączeniami do bazy danych SQL. Jeśli masz istniejące aplikacje MSDTC za pomocą tych interfejsów API po zainstalowaniu 4.6.1 po prostu odbudowanie aplikacji istniejących .NET 4.6 Framework. Jeśli projektów skierować .NET 4.6, automatycznie użyje zaktualizowane dll z nowej wersji struktury i teraz powiedzie transakcji rozproszonej, które wywołania interfejsu API w połączeniu z połączeniami do bazy danych SQL.

Należy pamiętać, że transakcje elastyczne bazy danych nie wymagają instalowania MSDTC. Zamiast tego transakcje elastyczne bazy danych bezpośrednio do zarządzania przez i w ramach bazy danych SQL. Ponieważ wdrożeniu MSDTC nie jest konieczne użycie transakcje rozproszone z bazy danych SQL znacznie upraszcza scenariusze chmury. 4 sekcji bardziej szczegółowo wyjaśniono sposoby wdrażanie transakcje bazy danych elastycznych i wymagane .NET framework razem z aplikacji chmura Azure.

## <a name="development-experience"></a>Proces tworzenia

### <a name="multi-database-applications"></a>Aplikacje wiele baz danych

Następujący kod używa znajome środowisko programowania z .NET System.Transactions. Klasy elementu TransactionScope ustala otaczającego transakcji w .NET. ("Transakcji otoczeniu" jest taki, który znajduje się w bieżącym wątku). Wszystkie połączenia otwarty w obrębie elementu TransactionScope bierz udział w transakcji. Jeśli wziąć udział różnych baz danych, transakcji jest automatycznie zwiększany do transakcji rozproszonej. Wynik transakcji zależy od ustawienia zakresu, który należy wykonać, aby wskazać zatwierdzania.

    using (var scope = new TransactionScope())
    {
        using (var conn1 = new SqlConnection(connStrDb1))
        {
            conn1.Open();
            SqlCommand cmd1 = conn1.CreateCommand();
            cmd1.CommandText = string.Format("insert into T1 values(1)");
            cmd1.ExecuteNonQuery();
        }

        using (var conn2 = new SqlConnection(connStrDb2))
        {
            conn2.Open();
            var cmd2 = conn2.CreateCommand();
            cmd2.CommandText = string.Format("insert into T2 values(2)");
            cmd2.ExecuteNonQuery();
        }

        scope.Complete();
    }

### <a name="sharded-database-applications"></a>Aplikacje sharded baz danych
 
Transakcje elastyczne bazy danych do bazy danych SQL obsługują także koordynowanie transakcje rozproszone miejsce, w którym otworzyć połączenia danych skalować się za pomocą metody OpenConnectionForKey Biblioteka klienta elastyczne bazy danych warstwy. Należy rozważyć przypadkach wymagających zagwarantowania transakcji spójności zmian przez kilka różnych sharding wartości klucza. Połączenia z odłamki hostingu wartości klucza różnych sharding są brokered przy użyciu OpenConnectionForKey. W przypadku ogólnym połączenia może być różnych odłamki tak, aby zapewnienie transakcji gwarancje wymaga transakcji rozproszonej. Poniższy przykład kodu ilustruje tej metody. Przyjęto założenie, że zmienną o nazwie shardmap jest używany do przedstawiania mapy shard z biblioteki klienta elastyczne bazy danych:

    using (var scope = new TransactionScope())
    {
        using (var conn1 = shardmap.OpenConnectionForKey(tenantId1, credentialsStr))
        {
            conn1.Open();
            SqlCommand cmd1 = conn1.CreateCommand();
            cmd1.CommandText = string.Format("insert into T1 values(1)");
            cmd1.ExecuteNonQuery();
        }
        
        using (var conn2 = shardmap.OpenConnectionForKey(tenantId2, credentialsStr))
        {
            conn2.Open();
            var cmd2 = conn2.CreateCommand();
            cmd2.CommandText = string.Format("insert into T1 values(2)");
            cmd2.ExecuteNonQuery();
        }

        scope.Complete();
    }


## <a name="net-installation-for-azure-cloud-services"></a>Instalacja programu .NET dla usług w chmurze Azure

Azure udostępnia kilka ofert do obsługi aplikacji .NET. Porównanie różnych ofert jest dostępna w [usłudze Azure aplikacji, usług w chmurze i porównania maszyn wirtualnych](../app-service-web/choose-web-site-cloud-service-vm.md). Gość OS oferty jest mniejsza niż .NET 4.6.1 wymagane transakcji elastyczne, musisz uaktualnić system operacyjny gościa do 4.6.1. 

Usługi aplikacji Azure uaktualnienia dla gości OS nie są obecnie obsługiwane. Dla maszyn wirtualnych Azure po prostu zalogować się do maszyn wirtualnych i uruchom Instalatora dla najnowszej .NET framework. Azure usług w chmurze należy uwzględnić instalacji nowszą wersją .NET do uruchamiania zadań wdrożenia. Pojęcia i kroki opisano w [Instalacji .NET z uprawnieniami usługi w chmurze](../cloud-services/cloud-services-dotnet-install-dotnet.md).  

Uwaga Instalatora dla środowiska .NET 4.6.1 mogą wymagać dodatkowego miejsca do magazynowania tymczasowych w procesie bootstrapping na usług w chmurze Azure niż Instalator dla .NET 4.6. Aby zapewnić pomyślną instalację, musisz zwiększyć tymczasowe miejsca do magazynowania dla usługi Azure cloud w pliku ServiceDefinition.csdef w sekcji LocalResources i ustawień środowiska uruchamiania zadania, jak pokazano w poniższym przykładzie:

    <LocalResources>
    ...
        <LocalStorage name="TEMP" sizeInMB="5000" cleanOnRoleRecycle="false" />
        <LocalStorage name="TMP" sizeInMB="5000" cleanOnRoleRecycle="false" />
    </LocalResources>
    <Startup>
        <Task commandLine="install.cmd" executionContext="elevated" taskType="simple">
            <Environment>
        ...
                <Variable name="TEMP">
                    <RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='TEMP']/@path" />
                </Variable>
                <Variable name="TMP">
                    <RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='TMP']/@path" />
                </Variable>
            </Environment>
        </Task>
    </Startup>
    
## <a name="transactions-across-multiple-servers"></a>Transakcje wielu serwerów

Transakcje elastyczne bazy danych są obsługiwane na różnych serwerach logicznych w bazie danych SQL Azure. Gdy transakcje ograniczenia logiczne serwera, serwerów najpierw należy można wprowadzić w relacji wzajemnej komunikacji. Po ustaleniu relacji komunikacji, dowolnej bazy danych w jednym z dwóch serwerów mogą uczestniczyć w elastyczną transakcje z bazami danych z innego serwera. Transakcji obejmujące więcej niż dwa serwery logicznych relacji komunikacji musi się znajdować w miejscu dla każdej pary serwerów logicznych.

Zarządzanie relacjami komunikacji między serwerami transakcji elastyczne bazy danych za pomocą następujące polecenia cmdlet programu PowerShell:

* **Nowy AzureRmSqlServerCommunicationLink**: tworzenie nowych relacji komunikacji między dwoma serwerami logicznych w bazie danych SQL Azure za pomocą tego polecenia cmdlet. Relacja jest symetrycznej, co oznacza, że oba serwery można inicjowania transakcji z serwerem programu.
* **Get-AzureRmSqlServerCommunicationLink**: pobieranie istniejące relacje komunikacji i ich właściwości za pomocą tego polecenia cmdlet.
* **Usuń AzureRmSqlServerCommunicationLink**: Użyj tego polecenia cmdlet, aby usunąć istniejącą relacją komunikacji. 


## <a name="monitoring-transaction-status"></a>Stan monitorowania

Umożliwia dynamiczne widoki zarządzania (DMVs) w bazie danych SQL postęp transakcji trwających elastyczne bazy danych i monitoruje stan. DMVs wszystkie związane z transakcje są odpowiednie dla transakcje rozproszone w bazie danych SQL. Możesz znaleźć odpowiedniej listy DMVs tutaj: [funkcje (Transact-SQL) i widoki pokrewne zarządzania dynamiczne transakcji](https://msdn.microsoft.com/library/ms178621.aspx).
 
Te DMVs są szczególnie przydatne:

* **sys.dm\_transakcji\_active\_transakcje**: Wyświetla aktualnie aktywne transakcje i ich stanu. Kolumna UOW (jednostki pracy) można określić transakcji różnych podrzędne, które należą do tej samej transakcji rozłożone. Wszystkich transakcji w obrębie tej samej transakcji rozłożone wykonać tę samą wartość UOW. Zapoznaj się z [dokumentacją DMV](https://msdn.microsoft.com/library/ms174302.aspx) uzyskać więcej szczegółowych informacji.
* **sys.dm\_transakcji\_bazy danych\_transakcje**: udostępnia dodatkowe informacje dotyczące transakcji, takie jak położenie transakcji w dzienniku. Zapoznaj się z [dokumentacją DMV](https://msdn.microsoft.com/library/ms186957.aspx) uzyskać więcej szczegółowych informacji.
* **sys.dm\_transakcji\_blokady**: informacje na temat blokad, które są obecnie posiadanych przez trwających transakcji. Zapoznaj się z [dokumentacją DMV](https://msdn.microsoft.com/library/ms190345.aspx) uzyskać więcej szczegółowych informacji.

## <a name="limitations"></a>Ograniczenia 

Następujące ograniczenia dotyczą obecnie transakcje elastyczne bazy danych w bazie danych SQL:

* Obsługiwane są tylko transakcje między bazami danych w bazie danych SQL. Inne [X / Otwórz XA](https://en.wikipedia.org/wiki/X/Open_XA) dostawców zasobów i baz danych poza bazy danych SQL nie mogą uczestniczyć w transakcji elastyczne bazy danych. Oznacza to, że transakcje elastyczne bazy danych nie można rozciągnąć na lokalnie, SQL Server i bazy danych programu SQL Azure. Transakcje rozproszone lokalnie nadal korzystać MSDTC. 
* Tylko klient koordynowany transakcje z aplikacji .NET są obsługiwane. Obsługa po stronie serwera T-SQL, takich jak rozpocząć DISTRIBUTED TRANSACTION jest planowane, ale nie są jeszcze dostępne. 
* Obsługiwane są tylko baz danych w wersji 12 bazy danych SQL Azure.
* Transakcje w usługach WCF nie są obsługiwane. Na przykład masz metody usługi WCF, który wykonuje transakcji. Załączenie połączenia w zakresie transakcji zakończy się niepowodzeniem jako [System.ServiceModel.ProtocolException](https://msdn.microsoft.com/library/system.servicemodel.protocolexception).

## <a name="additional-resources"></a>Dodatkowe zasoby

Jeszcze nie za pomocą możliwości elastyczne bazy danych dla aplikacji Azure? Zapoznaj się z naszą [Mapa dokumentacji](https://azure.microsoft.com/documentation/learning-paths/sql-database-elastic-scale/). Pytania należy bliższy kontakt z nami na [forum bazy danych SQL](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted) i funkcji, dodaj je do [forum opinii bazy danych SQL](https://feedback.azure.com/forums/217321-sql-database/).

<!--Image references-->
[1]: ./media/sql-database-elastic-transactions-overview/distributed-transactions.png



