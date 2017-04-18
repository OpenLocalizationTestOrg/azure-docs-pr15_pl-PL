<properties
   pageTitle="Szybkie uruchamianie rozwiązanie bazy danych Azure SQL | Microsoft Azure"
   description="Informacje o rozwiązaniach bazy danych Azure SQL"
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
   ms.workload="sqldb-quickstart"
   ms.date="09/06/2016"
   ms.author="carlrab"/>

# <a name="explore-azure-sql-database-solution-quick-starts"></a>Eksplorowanie Szybkie uruchamianie rozwiązanie bazy danych Azure SQL

Ten artykuł zawiera omówienie Azure SQL bazy danych rozwiązanie szybkiego uruchamiania. Te szybkiego uruchamiania znajdują się w repozytorium próbki GitHub programu SQL Server i przedstawimy sposób korzystania z bazy danych SQL w pełnym rozwiązaniem według rzeczywistych scenariuszy. Prosta samouczki krok po kroku, demonstrujących użycie funkcji określonego bazy danych SQL zobacz [samouczki Eksplorowanie bazy danych SQL Azure](sql-database-explore-tutorials.md).

## <a name="try-the-wingtiptickets-demo-and-hands-on-lab"></a>Spróbuj pokaz WingTipTickets i praktycznych ćwiczenia

Pokaz [WingTipTickets bazy danych SQL Azure](https://github.com/microsoft/wingtiptickets) i praktycznych ćwiczenia pokazują bazy danych SQL Azure a opartej na wyszukiwaniu Azure przykładowej aplikacji, używanym do sprzedaży biletów uzgodnieniu.


## <a name="collect-and-monitor-resource-usage-data-across-multiple-pools"></a>Zbieranie i monitorowanie danych dotyczących użycia zasobów w wielu pul

[Rozwiązanie Szybki Start: telemetrycznego elastyczne puli przy użyciu programu PowerShell](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools) rozwiązaniem jest do zbierania i monitorowania użycie zasobu bazy danych SQL w wielu pul w subskrypcji. Jeśli masz wiele baz danych w subskrypcji jest wygodna monitorowanie każdej elastyczne puli oddzielnie.

Aby rozwiązać ten problem, można połączyć poleceń cmdlet środowiska PowerShell bazy danych SQL i T-SQL kwerendy do zbierania danych dotyczących użycia zasobów z wielu pul i ich baz danych. Ułatwia monitorowanie i zwiększyć wydajność analizowanie użycie zasobu.

Szybki Start zawiera zestaw skryptów programu PowerShell i T-SQL kwerendy wraz z dokumentacją na działanie rozwiązanie i jak wdrażać go.

## <a name="get-started-with-elastic-database-in-an-saas-scenario"></a>Rozpoczynanie pracy z elastycznych bazy danych w scenariuszu władz akredytacji bezpieczeństwa

 [Rozwiązanie Szybki Start: elastyczne puli niestandardowe pulpit nawigacyjny do władz akredytacji bezpieczeństwa](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools-custom-dashboard) stanowi rozwiązanie scenariusza jako z oprogramowanie (władz akredytacji bezpieczeństwa), w której zastosowano funkcję elastyczne bazy danych SQL Database zapewnienie efektywnego pod względem kosztów i skalowalna bazy danych w wewnętrznej bazie danych dla aplikacji władz akredytacji bezpieczeństwa.

W tym rozwiązaniu prowadzi poprzez wdrożenie aplikacji sieci web. Ta aplikacja sieci web umożliwia wizualizowanie Załaduj utworzonego na elastyczne bazy danych przez generator ładowania używającej niestandardowych pulpitu nawigacyjnego uzupełniające Azure portal.

Szybki Start zawiera generator obciążenia i monitorowanie aplikacji sieci web oraz dokumentację dotyczącą działanie aplikacji i jak z niego korzystać.

## <a name="create-an-azure-sql-database-by-using-code-first-development-and-the-entity-framework"></a>Tworzenie bazy danych programu Azure SQL za pomocą kodu pierwszego rozwoju i struktury obiektu

Klip wideo i próbki w [Kod pierwszego do nowej bazy danych](https://msdn.microsoft.com/data/jj193542.aspx) zawiera wprowadzenie do rozwoju pierwszy kod, który jest przeznaczony dla nowej bazy danych. W tym scenariuszu jest przeznaczony dla bazy danych, który nie istnieje, ale której zostaną utworzone przez pierwszy kod. Możesz też tego scenariusza tworzy pustą bazę danych, do którego kod pierwszego dodaje nowych tabel.

Najpierw kod pozwala na zdefiniowanie modelu za pomocą klasy C# lub Visual Basic .NET. Opcjonalnie dodatkowe czynności konfiguracyjne można wykonywać przy użyciu atrybuty właściwości i klas lub za pomocą fluent interfejsu API.

## <a name="integrate-elastic-database-tools-into-an-entity-framework-application"></a>Integrowanie narzędzia elastyczne bazy danych aplikacji Framework jednostki

[Biblioteka klienta elastyczne bazy danych przy użyciu struktury jednostki](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md) pokazano zmian, które chcesz wprowadzić do aplikacji Framework jednostki zintegrować go przy użyciu [Narzędzia elastyczne bazy danych](sql-database-elastic-scale-get-started.md). Skoncentrować się na redagowania [shard mapowanie zarządzania](sql-database-elastic-scale-shard-map-management.md) i [Rozsyłanie zależne od danych](sql-database-elastic-scale-data-dependent-routing.md) przy użyciu obiektu ramach kod pierwszą metodę.

[Kod pierwszego do nowej próbki bazy danych dla EF](http://msdn.microsoft.com/data/jj193542.aspx) służy jako naszym przykładzie uruchomionego w tym przykładzie. Przykładowy kod, który towarzyszy tego dokumentu jest częścią zestawu narzędzi elastyczne bazy danych próbki w przykładach kodu programu Visual Studio.

## <a name="integrate-elastic-database-tools-with-row-level-security"></a>Integracja narzędzia elastyczne bazy danych przy użyciu zabezpieczeń na poziomie wiersza

[Multitenant aplikacji za pomocą narzędzia bazy danych elastycznych i zabezpieczenia na poziomie wiersza](sql-database-elastic-tools-multi-tenant-row-level-security.md) pokazuje zmiany należy wprowadzać w aplikacji Framework jednostki, aby zintegrować [Narzędzia elastyczne bazy danych](sql-database-elastic-scale-get-started.md) przy użyciu [zabezpieczeń na poziomie wiersza](https://msdn.microsoft.com/library/dn765131). Ten przykład przedstawia sposób przy użyciu tych technologii tworzenia aplikacji z obsługującego odłamki multitenant warstwa wysoce skalowalna danych.

W tym celu za pomocą klient ADO.NET SQL lub struktury obiektu. W tym przykładzie rozszerza [bibliotece klienta elastyczne bazy danych przy użyciu struktury obiektu](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md) , dodając obsługę multitenant shard baz danych.
Tworzenia aplikacji konsoli prosty do tworzenia blogów i wpisów, z czterech dzierżaw i dwie multitenant shard bazy danych.

## <a name="create-online-surveys-with-the-tailspin-surveys-application"></a>Tworzenie ankiety online przy użyciu aplikacji firma ankiet

Ten [ankiet firma przykładowej aplikacji](https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps/blob/master/docs/running-the-app.md) jest multitenant aplikacji sieci web, o nazwie ankiety, który umożliwia tworzenie ankiety online. Próbki rozwiązuje kilka kluczowych wątpliwości sposób zarządzania tożsamością użytkowników w multitenant aplikacji, w tym zapisów, uwierzytelnianie, autoryzacja i ról aplikacji.

## <a name="learn-about-the-latest-security-features-of-sql-database-with-the-contoso-clinic-demo-application"></a>Informacje o najnowszych funkcji zabezpieczeń bazy danych SQL z aplikacją pokaz Klinika Contoso

Ta [aplikacja pokaz Klinika Contoso](https://github.com/Microsoft/azure-sql-security-sample) ilustrację najnowszych funkcji zabezpieczeń bazy danych SQL.

## <a name="next-steps"></a>Następne kroki

[Eksplorowanie samouczki bazy danych SQL Azure](sql-database-explore-tutorials.md)
