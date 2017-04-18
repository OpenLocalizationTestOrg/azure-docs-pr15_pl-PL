<properties
   pageTitle="Włącz szyfrowanie danych przezroczysty (TDE) dla rozciąganie bazy danych SQL Server na Azure TSQL | Microsoft Azure"
   description="Włącz szyfrowanie danych przezroczysty (TDE) dla rozciąganie bazy danych SQL Server na Azure TSQL"
   services="sql-server-stretch-database"
   documentationCenter=""
   authors="douglaslMS"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-server-stretch-database"
   ms.workload="data-management"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="06/14/2016"
   ms.author="douglaslMS"/>

# <a name="enable-transparent-data-encryption-tde-for-stretch-database-on-azure-transact-sql"></a>Włączanie szyfrowania przezroczysty danych (TDE) rozciąganie bazy danych Azure (Transact-SQL)
> [AZURE.SELECTOR]
- [Azure portal](sql-server-stretch-database-encryption-tde.md)
- [TSQL](sql-server-stretch-database-tde-tsql.md)

Przezroczyste szyfrowanie danych (TDE) ułatwia, ochronę przed zagrożenia złośliwych działań, wykonując w czasie rzeczywistym szyfrowanie i odszyfrowywanie bazy danych, skojarzony kopie zapasowe oraz pliki dziennika transakcji w stanie spoczynku bez wprowadzania zmian w aplikacji.

TDE są szyfrowane na przechowywanie całej bazy danych przy użyciu klucza symetrycznej o nazwie klucza szyfrowania bazy danych. Wbudowane certyfikat klucza szyfrowania bazy danych jest chroniony. Wbudowane certyfikat jest unikatowy dla każdego Azure serwera. Microsoft automatycznie przełącza te certyfikaty, co najmniej raz 90 dni. Aby zapoznać się z ogólnym opisem TDE zobacz [Przezroczysty szyfrowania danych (TDE)].

##<a name="enabling-encryption"></a>Włączanie szyfrowania

Aby włączyć TDE dla Azure bazy danych, która jest przechowywania danych migracji z obsługą rozciąganie program SQL Server, wykonaj następujące czynności:

1. Nawiązywanie połączenia z *główną* bazą danych w Azure serwera obsługującego bazy danych przy użyciu logowania, administrator lub należeć do roli **dbmanager** wzorca bazy danych
2. Wykonaj następującą instrukcję szyfrowanie bazy danych.

```sql
ALTER DATABASE [database_name] SET ENCRYPTION ON;
```

##<a name="disabling-encryption"></a>Wyłączanie szyfrowania

Aby wyłączyć TDE dla Azure bazy danych, która jest przechowywania danych migracji z obsługą rozciąganie program SQL Server, wykonaj następujące czynności:

1. Nawiązywanie połączenia z *wzorcem* bazy danych przy użyciu logowania, administrator lub należeć do roli **dbmanager** wzorca bazy danych
2. Wykonaj następującą instrukcję szyfrowanie bazy danych.

```sql
ALTER DATABASE [database_name] SET ENCRYPTION OFF;
```

##<a name="verifying-encryption"></a>Sprawdzanie, czy szyfrowania

Aby sprawdzić, czy stan szyfrowania Azure bazy danych, który jest przechowywania danych migracji z włączoną obsługą rozciąganie program SQL Server, wykonaj następujące czynności:

1. Nawiązywanie połączenia z bazy danych *wzorcowej* lub wystąpienia przy użyciu identyfikatora, do którego jest administrator lub należeć do roli **dbmanager** wzorca bazy danych
2. Wykonaj następującą instrukcję szyfrowanie bazy danych.

```sql
SELECT
    [name],
    [is_encrypted]
FROM
    sys.databases;
```

W wyniku ```1``` wskazuje zaszyfrowaną bazę danych, ```0``` wskazuje bazy danych nie są szyfrowane.


<!--Anchors-->
[Szyfrowanie danych przezroczysty (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx


<!--Image references-->

<!--Link references-->
