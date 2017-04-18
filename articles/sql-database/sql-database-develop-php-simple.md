<properties
    pageTitle="Nawiązywanie połączenia z bazą danych SQL za pomocą PHP w systemie Windows | Microsoft Azure"
    description="Przedstawia przykładowego programu PHP, który łączy do bazy danych SQL Azure z klientem systemu Windows, a znajdują się łącza do składniki odpowiednie oprogramowanie wymagane przez klienta."
    services="sql-database"
    documentationCenter=""
    authors="meet-bhagdev"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="drivers"
    ms.tgt_pltfrm="na"
    ms.devlang="php"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="meetb"/>


# <a name="connect-to-sql-database-by-using-php-on-windows"></a>Nawiązywanie połączenia z bazą danych SQL za pomocą PHP w systemie Windows


[AZURE.INCLUDE [sql-database-develop-includes-selector-language-platform-depth](../../includes/sql-database-develop-includes-selector-language-platform-depth.md)] 


W tym temacie przedstawiono, jak można nawiązać bazy danych SQL Azure z aplikacją kliencką napisana PHP, na którym jest uruchomiony w systemie Windows.

## <a name="step-1--configure-development-environment"></a>Krok 1: Konfigurowanie środowisko projektowania

[Konfigurowanie środowiska programowania rozwoju PHP](https://msdn.microsoft.com/library/mt720663.aspx)

## <a name="step-2-create-a-sql-database"></a>Krok 2: Tworzenie bazy danych SQL

Zobacz [Wprowadzenie do strony](sql-database-get-started.md) informacje o sposobie tworzenia przykładowej bazy danych.  Należy pamiętać, że tworzenie **szablonu bazy danych AdventureWorks**prowadnicy. Przykłady pokazano poniżej tylko Praca ze **schematem AdventureWorks**.


## <a name="step-3-get-connection-details"></a>Krok 3: Uzyskiwanie szczegóły połączenia

[AZURE.INCLUDE [sql-database-include-connection-string-details-20-portalshots](../../includes/sql-database-include-connection-string-details-20-portalshots.md)]


## <a name="step-4-run-sample-code"></a>Krok 4: Uruchom przykładowy kod

* [Nawiązywanie połączenia za pomocą PHP SQL zatwierdzania koncepcji](https://msdn.microsoft.com/library/mt720665.aspx)
* [Resiliently połączenia z serwerem SQL z PHP](https://msdn.microsoft.com/library/mt720667.aspx)


## <a name="next-steps"></a>Następne kroki

* Przejrzyj [Omówienie tworzenia bazy danych SQL](sql-database-develop-overview.md)
* Więcej informacji na temat [Sterownik PHP firmy Microsoft dla programu SQL Server](https://msdn.microsoft.com/library/dn865013.aspx)
* Aby uzyskać więcej informacji dotyczących instalacji PHP i zastosowania zobacz [Uzyskiwanie dostępu do serwera bazy danych programu SQL z PHP](http://social.technet.microsoft.com/wiki/contents/articles/1258.accessing-sql-server-databases-from-php.aspx).

## <a name="additional-resources"></a>Dodatkowe zasoby 

* [Desenie projektu dla wielu dzierżawy władz akredytacji bezpieczeństwa aplikacji z bazy danych Azure SQL](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* Eksplorowanie wszystkie [Funkcje bazy danych SQL](https://azure.microsoft.com/services/sql-database/)
