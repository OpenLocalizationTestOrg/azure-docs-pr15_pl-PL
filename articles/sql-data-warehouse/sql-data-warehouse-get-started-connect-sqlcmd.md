<properties
   pageTitle="Kwerenda SQL Azure, Data Warehouse (sqlcmd) | Microsoft Azure"
   description="Kwerenda magazynu danych SQL Azure z wiersza polecenia narzędzia sqlcmd."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/06/2016"
   ms.author="barbkess;sonyama"/>

# <a name="query-azure-sql-data-warehouse-sqlcmd"></a>Kwerenda SQL Azure, Data Warehouse (sqlcmd)

> [AZURE.SELECTOR]
- [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
- [Nauka Azure komputera](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
- [Programu Visual Studio](sql-data-warehouse-query-visual-studio.md)
- [sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 

W tym instruktażu używa wiersza polecenia narzędzia [sqlcmd][] kwerendy magazynu danych SQL Azure.  

## <a name="1-connect"></a>1. łączenie

Aby rozpocząć pracę z [sqlcmd][], Otwórz okno wiersza polecenia i wprowadź **sqlcmd** następuje parametry połączenia dla bazy danych SQL magazynu danych. Parametry połączenia wymaga następujących parametrów:

+ **Server (-S):** Serwer w formularzu `<`nazwa serwera`>`. database.windows.net
+ **Bazy danych (-d):** Nazwa bazy danych.
+ **Włącz identyfikatorów (-I):** Musi być włączony identyfikatorów w cudzysłowach nawiązywania połączenia z wystąpieniem magazynu danych SQL.

Aby użyć uwierzytelnianie programu SQL Server, musisz dodać parametry nazwy użytkownika i hasła:

+ **Użytkownika (-P):** Serwer użytkownika w formularzu `<`użytkownika`>`
+ **Hasło (-P):** Hasło skojarzone z danym użytkownikiem.

Na przykład ciąg połączenia może wyglądać następująco:

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I
```

Aby użyć Azure Active Directory zintegrowanego uwierzytelniania, musisz dodać parametry usługi Azure Active Directory:

+ **Uwierzytelnianie usługi azure Active Directory (– G):** uwierzytelniania za pomocą usługi Azure Active Directory

Na przykład ciąg połączenia może wyglądać następująco:

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -G -I
```

> [AZURE.NOTE] Musisz [włączyć uwierzytelnianie usługi Azure Active Directory](sql-data-warehouse-authentication.md) do uwierzytelniania za pomocą usługi Active Directory.

## <a name="2-query"></a>2. kwerendy

Po połączeniu możesz problemów z dowolnego obsługiwanego instrukcji Transact-SQL względem wystąpienia.  W tym przykładzie kwerendy są przesyłane w trybie interakcyjnym.

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I
1> SELECT name FROM sys.tables;
2> GO
3> QUIT
```

W tych przykładach następnej pokazano, sposób wykonywania zapytań w trybie partii za pomocą opcji -Q lub połączeń rurowych programu SQL do sqlcmd.

```sql
sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I -Q "SELECT name FROM sys.tables;"
```

```sql
"SELECT name FROM sys.tables;" | sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I > .\tables.out
```

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji na temat szczegółowe informacje na temat opcji dostępnych w sqlcmd, zobacz [dokumentację sqlcmd][sqlcmd] .

<!--Image references-->

<!--Article references-->

<!--MSDN references--> 
[sqlcmd]: https://msdn.microsoft.com/library/ms162773.aspx
[Azure portal]: https://portal.azure.com

<!--Other Web references-->
