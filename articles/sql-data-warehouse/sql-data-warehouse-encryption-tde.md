<properties
   pageTitle="Szyfrowanie danych przezroczyste w magazynie danych SQL (Portal) | Microsoft Azure"
   description="Szyfrowanie danych przezroczysty (TDE) w magazynie danych SQL"
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

# <a name="get-started-with-transparent-data-encryption-tde-in-sql-data-warehouse"></a>Rozpoczynanie pracy z przezroczysty danych szyfrowania (TDE) w magazynie danych SQL

> [AZURE.SELECTOR]
- [Omówienie zabezpieczeń](sql-data-warehouse-overview-manage-security.md)
- [Uwierzytelnianie](sql-data-warehouse-authentication.md)
- [Szyfrowanie (Portal)](sql-data-warehouse-encryption-tde.md)
- [Szyfrowanie (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)

## <a name="required-permssions"></a>Wymagane uprawnienia

Aby włączyć przezroczystość szyfrowania danych (TDE), musi być administratorem lub członkiem roli dbmanager.

## <a name="enabling-encryption"></a>Włączanie szyfrowania

Aby włączyć TDE dla magazynu danych SQL, wykonaj następujące czynności:

1. Otwórz bazę danych w [Azure portal](https://portal.azure.com)
2. W karta bazy danych kliknij przycisk **Ustawienia**
3. Wybierz opcję **szyfrowania danych przezroczystości**![][1]
4. Wybierz odpowiednie ustawienie **na**![][2]
5. Wybierz przycisk **Zapisz**
![][3]  

## <a name="disabling-encryption"></a>Wyłączanie szyfrowania

Aby wyłączyć TDE dla magazynu danych SQL, wykonaj następujące czynności:

1. Otwórz bazę danych w [Azure portal](https://portal.azure.com)
2. W karta bazy danych kliknij przycisk **Ustawienia**
3. Wybierz opcję **szyfrowania danych przezroczystości**![][1]
4. Wybierz odpowiednie ustawienie **wyłączone**![][4]
5. Wybierz przycisk **Zapisz**
![][5]  

## <a name="encryption-dmvs"></a>DMVs szyfrowania

Szyfrowanie można potwierdzić z DMVs następujące czynności:

- [sys.Databases]
- [sys.dm_pdw_nodes_database_encryption_keys]

<!--MSDN references-->
[Transparent Data Encryption (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx
[sys.Databases]: http://msdn.microsoft.com/library/ms178534.aspx
[sys.dm_pdw_nodes_database_encryption_keys]: https://msdn.microsoft.com/library/mt203922.aspx

<!--Image references-->
[1]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings.png
[2]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-on.png
[3]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-save.png
[4]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-off.png
[5]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-save2.png

<!--Link references-->
