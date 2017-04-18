<properties
    pageTitle="Nawiązywanie połączenia z bazą danych SQL przy użyciu dopiskiem | Microsoft Azure"
    description="Nadaj próbki kodu dopiskiem fonetycznym uruchomienia nawiązywania połączenia z bazą danych SQL Azure."
    services="sql-database"
    documentationCenter=""
    authors="ajlam"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="drivers"
    ms.tgt_pltfrm="na"
    ms.devlang="ruby"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="andrela"/>


# <a name="connect-to-sql-database-by-using-ruby"></a>Nawiązywanie połączenia z bazą danych SQL przy użyciu dopiskiem 

[AZURE.INCLUDE [sql-database-develop-includes-selector-language-platform-depth](../../includes/sql-database-develop-includes-selector-language-platform-depth.md)] 

W tym temacie przedstawiono sposób łączenia i kwerendy bazy danych SQL Azure za pomocą dopiskiem. W tym przykładzie można uruchamiać z platformy systemu Windows, Ubuntu Linux lub Mac.

## <a name="step-1-configure-development-environment"></a>Krok 1: Konfigurowanie środowisko projektowania

[Wymagania wstępne dotyczące za pomocą sterownika dopiskiem TinyTDS dla programu SQL Server](https://msdn.microsoft.com/library/mt711041.aspx)

## <a name="step-2-create-a-sql-database"></a>Krok 2: Tworzenie bazy danych SQL

Zobacz [Wprowadzenie do strony](sql-database-get-started.md) informacje o sposobie tworzenia przykładowej bazy danych.  Należy pamiętać, że tworzenie **szablonu bazy danych AdventureWorks**prowadnicy. Przykłady pokazano poniżej tylko Praca ze **schematem AdventureWorks**.

## <a name="step-3-get-connection-details"></a>Krok 3: Uzyskiwanie szczegóły połączenia

[AZURE.INCLUDE [sql-database-include-connection-string-details-20-portalshots](../../includes/sql-database-include-connection-string-details-20-portalshots.md)]

## <a name="step-4-run-sample-code"></a>Krok 4: Uruchom przykładowy kod

[Nawiązywanie połączenia przy użyciu dopiskiem SQL zatwierdzania koncepcji](http://msdn.microsoft.com/library/mt715797.aspx)

## <a name="next-steps"></a>Następne kroki

* Przejrzyj [Omówienie tworzenia bazy danych SQL](sql-database-develop-overview.md)
* Więcej informacji na temat [Sterownik dopiskiem firmy Microsoft dla programu SQL Server](https://msdn.microsoft.com/library/mt691981.aspx)

## <a name="additional-resources"></a>Dodatkowe zasoby 

* [Desenie projektu dla wielu dzierżawy władz akredytacji bezpieczeństwa aplikacji z bazy danych Azure SQL](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* Eksplorowanie wszystkie [Funkcje bazy danych SQL](https://azure.microsoft.com/services/sql-database/)
