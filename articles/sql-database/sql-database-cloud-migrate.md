<properties
   pageTitle="Migracja bazy danych programu SQL Server do bazy danych SQL | Microsoft Azure"
   description="Dowiedz się, jak lokalnego programu SQL Server migracja bazy danych do bazy danych SQL Azure w chmurze. Aby sprawdzić zgodność przed migracją bazy danych za pomocą narzędzia do migracji bazy danych."
   keywords="bazy danych migracji, migracja bazy danych programu sql server, narzędzia do migracji bazy danych, migrowanie bazy danych, migrowanie bazy danych sql"
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="sqldb-migrate"
   ms.date="08/24/2016"
   ms.author="carlrab"/>

# <a name="sql-server-database-migration-to-sql-database-in-the-cloud"></a>Migracja bazy danych programu SQL Server do bazy danych SQL w chmurze

W tym artykule można Dowiedz się, jak przeprowadzić migrację lokalnego programu SQL Server 2005 lub nowszej wersji bazy danych do bazy danych SQL Azure. W tym procesie migracji bazy danych możesz przeprowadzić migrację schemat i danych z bazy danych programu SQL Server w środowisku bieżącego do bazy danych SQL. Aby pomyślnie istniejącej bazy danych należy najpierw przekazać test zgodności. Z [Bazy danych SQL w wersji 12](sql-database-v12-whats-new.md)istnieje kilka pozostałe problemy ze zgodnością, niż problem związany z operacje na poziomie serwera i między bazami danych. Bazy danych i aplikacje, które korzystają z [częściowo lub nieobsługiwane funkcje](sql-database-transact-sql-information.md) muszą niektóre ponownego projektowania ustalenie tych niezgodności przed bazy danych programu SQL Server, możesz przeprowadzić migrację.

Aby przeprowadzić migrację, Oto czynności można wykonać:

- **Test zgodności**: Sprawdzanie poprawności zgodności bazy danych z [Bazy danych SQL w wersji 12](sql-database-v12-whats-new.md). 
- **Rozwiązywanie problemów ze zgodnością, jeśli istnieją**: Jeśli sprawdzanie poprawności nie powiedzie się, musisz naprawić błędy sprawdzania poprawności.  
- **Przeprowadzanie migracji** Po zgodnego bazy danych możesz używać jedną lub kilka metod, aby przeprowadzić migrację. 

Program SQL Server udostępnia kilka sposobów wykonywania wszystkich zadań. W tym artykule omówiono metody dostępne dla każdego zadania. Na poniższym diagramie przedstawiono kroki i metod.

  ![Diagram migracji VSSSDT](./media/sql-database-cloud-migrate/03VSSSDTDiagram.png)
  
 > [AZURE.NOTE] Aby przeprowadzić migrację bazy danych serwera SQL, takich jak Microsoft Access, Sybase, MySQL Oracle i DB2 do bazy danych SQL Azure, zobacz [Asystenta migracji programu SQL Server](http://blogs.msdn.com/b/ssma/).

## <a name="database-migration-tools-test-sql-server-database-compatibility-with-sql-database"></a>Narzędzia do migracji bazy danych testowanie zgodności bazy danych programu SQL Server z bazą danych SQL

Aby przetestować problemy ze zgodnością bazy danych SQL przed uruchomieniem procesu migracji bazy danych, użyj jednej z następujących metod:

> [AZURE.SELECTOR]
- [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
- [SqlPackage](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md)
- [SSMS](sql-database-cloud-migrate-determine-compatibility-ssms.md)
- [Uaktualnianie Advisor](http://www.microsoft.com/download/details.aspx?id=48119)
- [SAMW](sql-database-cloud-migrate-fix-compatibility-issues.md)

- [Program SQL Server Data Tools for Visual Studio ("SSDT")](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md): SSDT używa najnowszych przepisów zgodności wykryto niezgodności wersji 12 bazy danych SQL. Jeśli zostaną wykryte niezgodności, można usunąć wykryte błędy bezpośrednio w tym narzędziu. Ta metoda jest zalecana metoda testowanie i rozwiązywanie problemów ze zgodnością wersji 12 bazy danych SQL. 
- [SqlPackage](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md): SqlPackage to narzędzie wiersza polecenia, które sprawdza zgodności problemów i generuje raport zawierający wykryto zgodnością. Jeśli używasz tego narzędzia, upewnij się, że przy użyciu najnowszych reguł zgodności za pomocą najnowszej wersji. Jeśli zostaną wykryte błędy, należy użyć innego narzędzia rozwiązać wszelkie problemy ze zgodnością wykryto - SSDT jest zalecane.  
- [Kreator aplikacji warstwy eksportowanie danych w programie SQL Server Management Studio](sql-database-cloud-migrate-determine-compatibility-ssms.md): Ten kreator wykrywa i zgłoszone błędy do ekranu. Jeśli nie zostaną wykryte żadne błędy, możesz kontynuować i przeprowadzić migrację do bazy danych SQL. Jeśli zostaną wykryte błędy, należy użyć innego narzędzia rozwiązać wszelkie problemy ze zgodnością wykryto - SSDT jest zalecane.
- [Microsoft SQL Server 2016 uaktualnienie Advisor Preview](http://www.microsoft.com/download/details.aspx?id=48119): to narzędzie autonomicznego znajdującego się w wersji preview, wykrywa i generuje raport niezgodności wersji 12 bazy danych SQL. To narzędzie nie ma jeszcze najnowszych przepisów zgodności. Jeśli zostaną wykryte żadne błędy, można kontynuować i przeprowadzić migrację do bazy danych SQL. Jeśli zostaną wykryte błędy, należy użyć innego narzędzia rozwiązać wszelkie problemy ze zgodnością wykryto - SSDT jest zalecane. 
- [Kreator migracji SQL Azure ("SAMW")](sql-database-cloud-migrate-fix-compatibility-issues.md): SAMW to narzędzie witrynie codeplex w korzystającego z regułami zgodności V11 bazy danych SQL Azure wykryto niezgodności wersji 12 bazy danych SQL Azure. Jeśli zostaną wykryte niezgodności, niektóre problemy dotyczące można ustalić bezpośrednio w tym narzędziu. To narzędzie może się okazać niezgodności, które nie powinny być ustalane. Pierwszej bazy danych SQL Azure pomocy narzędzia do migracji dostępne był on i aktywnie jest obsługiwana przez społeczność użytkowników programu SQL Server. Ponadto to narzędzie może wykonać migrację z poziomu samego narzędzia. 

## <a name="fix-database-migration-compatibility-issues"></a>Rozwiązywanie problemów ze zgodnością migracja bazy danych

Jeśli zostaną wykryte problemy ze zgodnością, należy je naprawić przed wykonaniem migracji bazy danych programu SQL Server. Istnieje szereg problemy ze zgodnością, które można napotkać, zależy od tego, na wersję programu SQL Server w bazie danych źródłowych i złożoności bazy danych są migrowane. Starsze wersje programu SQL Server ma więcej problemów ze zgodnością. Skorzystaj z następujących zasobów, oprócz wybranych wyszukiwania Internet, korzystając z wyszukiwarki opcji:

- [Funkcje bazy danych programu SQL Server nie jest obsługiwane w bazie danych SQL Azure](sql-database-transact-sql-information.md)
- [Funkcja aparat wycofane bazy danych programu SQL Server 2016](https://msdn.microsoft.com/library/ms144262%28v=sql.130%29)
- [Funkcja aparat wycofane bazy danych programu SQL Server 2014 r.](https://msdn.microsoft.com/library/ms144262%28v=sql.120%29)
- [Funkcja aparat wycofane bazy danych programu SQL Server 2012](https://msdn.microsoft.com/library/ms144262%28v=sql.110%29)
- [Funkcja aparat wycofane bazy danych w programie SQL Server 2008 R2](https://msdn.microsoft.com/library/ms144262%28v=sql.105%29)
- [Funkcja aparat wycofane bazy danych programu SQL Server 2005](https://msdn.microsoft.com/library/ms144262%28v=sql.90%29)

Oprócz wyszukiwania w Internecie i korzystanie z tych zasobów, użyj [Fora społeczności użytkowników programu SQL Server w witrynie MSDN](https://social.msdn.microsoft.com/Forums/sqlserver/home?category=sqlserver) lub [zdarzeń StackOverflow](http://stackoverflow.com/).

Aby rozwiązać problemy wykryte, użyj jednej z następujących narzędzi migracji bazy danych:

> [AZURE.SELECTOR]
- [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
- [SSMS](sql-database-cloud-migrate-fix-compatibility-issues-ssms.md)
- [SAMW](sql-database-cloud-migrate-fix-compatibility-issues.md)

- Korzystanie z [Programu SQL Server Data Tools for Visual Studio ("SSDT")](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md): Aby użyć SSDT, możesz zaimportować schemat bazy danych do programu SQL Server Data Tools for Visual Studio "SSDT") i tworzenie projektu do wdrożenia wersji 12 bazy danych SQL. Następnie rozwiązać wszelkie problemy ze zgodnością wykryto w SSDT. Po zakończeniu synchronizacji zmian do bazy danych źródłowych (lub kopii bazy danych źródłowych. SSDT jest obecnie zaleca się testowanie i rozwiązywanie problemów ze zgodnością wersji 12 bazy danych SQL. Kliknięcie łącza [Samouczek przy użyciu SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md).
- Za pomocą [Programu SQL Server Management Studio ("SSMS")](sql-database-cloud-migrate-fix-compatibility-issues-ssms.md): umożliwia SSMS wykonanie polecenia języku Transact-SQL poprawiać błędy przy użyciu innego narzędzia. Ta metoda jest przede wszystkim dla zaawansowanych użytkowników zmodyfikować schemat bazy danych bezpośrednio w bazie danych źródłowych. 
- Za pomocą [Kreatora migracji Azure SQL ("SAMW")](sql-database-cloud-migrate-fix-compatibility-issues.md): umożliwia SAMW Generuj skrypt języka Transact-SQL bazy danych źródłowych. Skrypt, jeśli to możliwe zapewnić zgodność z bazy danych SQL w wersji 12 schematu przy użyciu kreatora. Po zakończeniu SAMW można nawiązać wykonać skryptu w wersji 12 bazy danych SQL. To narzędzie analizuje także pliki śledzenia, aby określić problemy ze zgodnością. Skrypt można wygenerować ze schematem tylko lub mogą zawierać dane w formacie BCP.

## <a name="migrate-a-compatible-sql-server-database-to-sql-database"></a>Migrowanie zgodne bazy danych programu SQL Server do bazy danych SQL

Aby przeprowadzić migrację zgodne bazy danych programu SQL Server, firma Microsoft udostępnia kilka metod migracji dla różnych scenariuszy. Wybranej metody zależy od usługi na uszkodzenia przestoje, rozmiar i złożoność bazy danych programu SQL Server i łączność z chmurą Microsoft Azure.  

> [AZURE.SELECTOR]
- [Kreator migracji SSMS](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md)
- [Eksportowanie do pliku PLECAK](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
- [Importowanie z pliku PLECAK](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [Transakcji replikacji](sql-database-cloud-migrate-compatible-using-transactional-replication.md)

Aby wybrać metodę migracji, pierwszy pytanie, aby zadać jest, jeśli można możesz przenieść bazę danych z produkcji podczas migracji. Migrowanie bazy danych, gdy występuje aktywnych transakcji może spowodować niespójności bazy danych i uszkodzenie bazy danych. Istnieje wiele metod, aby przełączanie w stan spoczynku bazy danych, z wyłączenie łączność z klientami do tworzenia [migawki bazy danych](https://msdn.microsoft.com/library/ms175876.aspx).

Aby przeprowadzić migrację z minimalnymi przestoje, replikacji [programu SQL Server transakcji](sql-database-cloud-migrate-compatible-using-transactional-replication.md) bazy danych, która spełnia wymagania dotyczące replikacji transakcji. Jeśli można sobie niektóre przestoje lub nowszy migracji przeprowadzasz migrację testową produkcji bazy danych, rozważ jedną z następujących trzech metod:

- [Kreator migracji SSMS](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md): dla małych i średnich baz danych, migracja zgodne SQL Server 2005 lub nowszej wersji bazy danych jest tak proste, jak działa [Kreator bazy danych Microsoft Azure wdrażanie bazy danych](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md) programu SQL Server Management Studio.
- [Eksportowanie do pliku PLECAK](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md) i [Importuj z pliku PLECAK](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md): Jeśli masz problemy łączności (nie łączności, niskiej przepustowości lub przekroczono limit czasu problemy), a dla średnich i dużych baz danych, za pomocą pliku [PLECAK](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) . Przy użyciu tej metody możesz wyeksportować schematu programu SQL Server i dane do pliku PLECAK. Następnie zaimportuj plik PLECAK do bazy danych SQL Kreator eksportu danych warstwy aplikacji w programie SQL Server Management Studio lub narzędzia wiersza polecenia [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) .
- Przy użyciu PLECAK BCP razem: za pomocą pliku [PLECAK](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) i [BCP](https://msdn.microsoft.com/library/ms162802.aspx) znacznie większy bazach danych dla uzyskanie większej parallelization dla zwiększa wydajność, chociaż bardziej złożone. Ta metoda migracji schemat i dane oddzielnie.
 - [Eksportuj schemat tylko do pliku PLECAK](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md).
 - [Importowanie schematu tylko z pliku PLECAK](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md) do bazy danych SQL.
 - Aby wyodrębnić dane do plików prostych, a następnie [Załaduj równoległe](https://technet.microsoft.com/library/dd425070.aspx) te pliki do bazy danych SQL Azure za pomocą [BCP](https://msdn.microsoft.com/library/ms162802.aspx) .

     ![Program SQL Server bazy danych migracji - Migrowanie bazy danych SQL w chmurze.](./media/sql-database-cloud-migrate/01SSMSDiagram_new.png)

## <a name="next-steps"></a>Następne kroki

- [Podgląd uaktualnienia Advisor programu Microsoft SQL Server 2016](http://www.microsoft.com/download/details.aspx?id=48119)
- [Najnowsza wersja SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [Najnowsza wersja programu SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)

##<a name="additional-resources"></a>Dodatkowe zasoby

- [Baza danych SQL w wersji 12](sql-database-v12-whats-new.md)
[języku Transact-SQL częściowo lub nieobsługiwane funkcje](sql-database-transact-sql-information.md)
- [Migrowanie bazy danych serwera SQL za pomocą Asystenta migracji programu SQL Server](http://blogs.msdn.com/b/ssma/)
