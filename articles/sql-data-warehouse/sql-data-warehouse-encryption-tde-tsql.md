<properties
   pageTitle="Szyfrowanie danych przezroczyste w magazynie danych SQL (T-SQL) | Microsoft Azure"
   description="Szyfrowanie danych przezroczysty (TDE) w magazynie danych SQL (T-SQL)"
   services="sql-data-warehouse"
   documentationCenter=""
   authors="ronortloff"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.workload="data-management"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="09/24/2016"
   ms.author="rortloff;barbkess;sonyama"/>

# <a name="get-started-with-transparent-data-encryption-tde"></a>Rozpoczynanie pracy z szyfrowania danych przezroczysty (TDE)


> [AZURE.SELECTOR]
- [Omówienie zabezpieczeń](sql-data-warehouse-overview-manage-security.md)
- [Uwierzytelnianie](sql-data-warehouse-authentication.md)
- [Szyfrowanie (Portal)](sql-data-warehouse-encryption-tde.md)
- [Szyfrowanie (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)

## <a name="required-permssions"></a>Wymagane uprawnienia

Aby włączyć przezroczystość szyfrowania danych (TDE), musi być administratorem lub członkiem roli dbmanager.

## <a name="enabling-encryption"></a>Włączanie szyfrowania

Wykonaj poniższe czynności, aby włączyć TDE dla magazynu danych SQL:

1. Nawiązywanie połączenia z *wzorcem* bazy danych na serwerze obsługującym bazę danych przy użyciu logowania, administrator lub należeć do roli **dbmanager** wzorca bazy danych
2. Wykonaj następującą instrukcję szyfrowanie bazy danych.

```sql
ALTER DATABASE [AdventureWorks] SET ENCRYPTION ON;
```

## <a name="disabling-encryption"></a>Wyłączanie szyfrowania

Wykonaj poniższe czynności, aby wyłączyć TDE dla magazynu danych SQL:

1. Nawiązywanie połączenia z *wzorcem* bazy danych przy użyciu logowania, administrator lub należeć do roli **dbmanager** wzorca bazy danych
2. Wykonaj następującą instrukcję szyfrowanie bazy danych.

```sql
ALTER DATABASE [AdventureWorks] SET ENCRYPTION OFF;
```

> [AZURE.NOTE] Wstrzymanie magazynu danych SQL musi wznowić przed wprowadzeniem zmian w ustawieniach TDE.

## <a name="verifying-encryption"></a>Sprawdzanie, czy szyfrowania

Aby sprawdzić stan szyfrowania dla magazynu danych SQL, wykonaj następujące czynności:

1. Nawiązywanie połączenia z bazy danych *wzorcowej* lub wystąpienia przy użyciu logowania, administrator lub należeć do roli **dbmanager** wzorca bazy danych
2. Wykonaj następującą instrukcję szyfrowanie bazy danych.

```sql
SELECT
    [name],
    [is_encrypted]
FROM
    sys.databases;
```

W wyniku ```1``` wskazuje zaszyfrowaną bazę danych, ```0``` wskazuje bazy danych nie są szyfrowane.

## <a name="encryption-dmvs"></a>DMVs szyfrowania  

- [sys.Databases][] 
- [sys.dm_pdw_nodes_database_encryption_keys][]


<!--Anchors-->
[Transparent Data Encryption (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx
[sys.Databases]: http://msdn.microsoft.com/library/ms178534.aspx  
[sys.dm_pdw_nodes_database_encryption_keys]: https://msdn.microsoft.com/library/mt203922.aspx  

<!--Image references-->

<!--Link references-->
