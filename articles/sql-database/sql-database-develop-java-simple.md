<properties
    pageTitle="Nawiązywanie połączenia z bazą danych SQL przy użyciu języka Java JDBC w systemie Windows | Microsoft Azure"
    description="Przedstawia przykładowy kod języka Java, który umożliwia nawiązywanie połączenia z bazą danych SQL Azure. W przykładzie zastosowano JDBC i działa na komputerze klienckim systemu Windows."
    services="sql-database"
    documentationCenter=""
    authors="LuisBosquez"
    manager="jhubbard"
    editor="genemi"/>


<tags
    ms.service="sql-database"
    ms.workload="drivers"
    ms.tgt_pltfrm="na"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="lbosq"/>


# <a name="connect-to-sql-database-by-using-java-with-jdbc-on-windows"></a>Nawiązywanie połączenia z bazą danych SQL przy użyciu języka Java JDBC w systemie Windows


[AZURE.INCLUDE [sql-database-develop-includes-selector-language-platform-depth](../../includes/sql-database-develop-includes-selector-language-platform-depth.md)] 


W tym temacie przedstawiono przykładowy kod języka Java, który umożliwia nawiązywanie połączenia z bazą danych SQL Azure. Przykładowe Java oparte na Java Development Kit (JDK) wersji 1.8. Próbki nawiązuje połączenie z bazą danych SQL Azure za pomocą sterownika JDBC.

## <a name="step-1--configure-development-environment"></a>Krok 1: Konfigurowanie środowisko projektowania

[Konfigurowanie środowiska programowania rozwoju Java](https://msdn.microsoft.com/library/mt720658.aspx)

## <a name="step-2-create-a-sql-database"></a>Krok 2: Tworzenie bazy danych SQL

Zobacz [Wprowadzenie do strony](sql-database-get-started.md) Dowiedz się, jak utworzyć bazę danych.  

## <a name="step-3-get-connection-string"></a>Krok 3: Pobieranie parametrów połączenia

[AZURE.INCLUDE [sql-database-include-connection-string-jdbc-20-portalshots](../../includes/sql-database-include-connection-string-jdbc-20-portalshots.md)]

> [AZURE.NOTE] Jeśli używasz sterownika JTDS JDBC, a następnie należy dodać "ssl = wymagają" do adresu URL połączenia ciąg, jeśli jest konieczne do następujących opcji maszyny wirtualnej Java "-Djsse.enableCBCProtection=false". Ta opcja maszyny wirtualnej Java wyłącza poprawki luka w zabezpieczeniach, dlatego należy się upewnić, że wiesz, jakie zagrożenia należy zrobić przed ustawieniem tej opcji.

## <a name="step-4-run-sample-code"></a>Krok 4: Uruchom przykładowy kod

* [Nawiązywanie połączenia przy użyciu języka Java SQL zatwierdzania koncepcji](https://msdn.microsoft.com/library/mt720656.aspx)

## <a name="next-steps"></a>Następne kroki

* Odwiedź witrynę [Centrum deweloperów języka Java](/develop/java/).
* Przejrzyj [Omówienie tworzenia bazy danych SQL](sql-database-develop-overview.md)
* Więcej informacji na temat [Sterownik JDBC firmy Microsoft dla programu SQL Server](https://msdn.microsoft.com/library/mt484311.aspx)

## <a name="additional-resources"></a>Dodatkowe zasoby 

* [Desenie projektu dla wielu dzierżawy władz akredytacji bezpieczeństwa aplikacji z bazy danych Azure SQL](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* Eksplorowanie wszystkie [Funkcje bazy danych SQL](https://azure.microsoft.com/services/sql-database/)
