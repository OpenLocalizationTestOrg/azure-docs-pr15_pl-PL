<properties
   pageTitle="Ograniczenia ogólne bazy danych Azure SQL i wskazówki"
   description="Na tej stronie opisano pewne ograniczenia ogólne dla bazy danych SQL Azure, a także obszarów współdziałania i obsługa techniczna."
   services="sql-database"
   documentationCenter="na"
   authors="CarlRabeler"
   manager="jhubbard"
   editor="monicar" />
<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="09/06/2016"
   ms.author="carlrab" />

# <a name="azure-sql-database-general-limitations-and-guidelines"></a>Ograniczenia ogólne bazy danych Azure SQL i wskazówki

Ten temat zawiera ogólne ograniczenia i wskazówki dla bazy danych SQL Azure. Ukończono zrozumieć przydziałów, zarządzania zasobami i pomocy technicznej, zobacz [dodatkowe zasoby](#additional-guidelines) na końcu tego tematu.

## <a name="connectivity-and-authentication"></a>Łączność i uwierzytelniania

  - Uwierzytelnianie systemu Windows nie jest obsługiwane. Zobacz [Zarządzanie bazami danych i logowania w bazie danych Azure SQL](sql-database-manage-logins.md). Jednak uwierzytelnianie usługi Azure Active Directory jest obsługiwana przez pewne ograniczenia. Zobacz [Nawiązywanie połączenia z bazą danych SQL Azure Active Directory uwierzytelniania](sql-database-aad-authentication.md).

  - Microsoft Azure SQL Database obsługuje dane tabelaryczne strumienia (TDS) protokół klienta w wersji 7.3 lub nowszej.

  - Dozwolone są tylko połączenia TCP/IP.

  - Przeglądarka programu SQL Server 2008 SQL Server nie jest obsługiwana, ponieważ Microsoft Azure SQL Database nie ma portów dynamicznych tylko port 1433.

## <a name="sql-server-agentjobs"></a>Zadań agenta serwera SQL

Jednak elastyczne zadania umożliwia wykonywanie zadań przez jeden-do-wielu baz danych Microsoft Azure SQL Database nie obsługuje agenta programu SQL Server. Aby uzyskać więcej informacji o zadaniach elastyczne zobacz [elastyczne zadania](sql-database-elastic-jobs-overview.md).

## <a name="sql-server-collation-support"></a>Obsługa sortowania serwera SQL

Domyślne sortowanie bazy danych używane przez Microsoft Azure SQL Database jest **SQL_LATIN1_GENERAL_CP1_CI_AS**, gdzie **LATIN1_GENERAL** jest angielski (Stany Zjednoczone), **CP1** to kod strony 1252 **CI** jest uwzględniana wielkość liter i **akcenty znajduje się** . Nie jest możliwa sortowanie baz danych w wersji 12. Aby uzyskać więcej informacji o konfigurowaniu sortowania zobacz [SORTUJ (Transact-SQL)](https://msdn.microsoft.com/library/ms184391.aspx).

## <a name="naming-requirements"></a>Wymagania dotyczące nazewnictwa

Niektóre nazwy użytkownika nie są dozwolone ze względów bezpieczeństwa. Nie można używać następujących nazw:

 - **Administrator**
 - **Administrator**
 - **gościa**
 - **główny**
 - **SA**

Nazwy dla wszystkich nowych obiektów muszą być zgodne z regułami programu SQL Server dla identyfikatorów. Aby uzyskać więcej informacji zobacz [identyfikatory](https://msdn.microsoft.com/library/ms175874.aspx).

Ponadto logowania i nazwy nie mogą zawierać \ znaków (uwierzytelnianie systemu Windows nie jest obsługiwany).

## <a name="additional-guidelines"></a>Dodatkowe wskazówki

- Oprócz ogólne ograniczenia przedstawionych w tym artykule baza danych SQL ma przydziałów zasobów określonych i ograniczenia zgodnie z **poziomu usługi**. Aby uzyskać omówienie warstwy usługi zobacz [warstwy usługi bazy danych SQL](sql-database-service-tiers.md).

- Dla innych ograniczeń bazy danych SQL zobacz [Limity zasobów bazy danych SQL Azure](sql-database-resource-limits.md).

- Aby uzyskać zabezpieczeń pokrewne wskazówki, zobacz [wskazówki dotyczące zabezpieczeń bazy danych SQL Azure](sql-database-security-guidelines.md).

- Innego obszaru powiązanych otacza zgodności, która bazy danych SQL Azure z lokalnego wersje programu SQL Server, takich jak program SQL Server 2014 i SQL Server 2016. Najnowszą wersję wersji 12 bazy danych SQL Azure wprowadził wiele udoskonaleń w tym obszarze. Aby uzyskać więcej informacji zobacz [Co nowego w wersji 12 bazy danych SQL](sql-database-v12-whats-new.md).

- Aby uzyskać informacji na temat dostępności sterowników oraz obsługę bazy danych SQL zobacz [Biblioteki połączeń dla bazy danych SQL i programu SQL Server](sql-database-libraries.md).
