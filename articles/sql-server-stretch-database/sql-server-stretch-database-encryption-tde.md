<properties
   pageTitle="Włącz szyfrowanie danych przezroczysty (TDE) dla rozciąganie bazy danych SQL Server Azure | Microsoft Azure"
   description="Włącz szyfrowanie danych przezroczysty (TDE) dla rozciąganie bazy danych SQL Server Azure"
   services="sql-server-stretch-database"
   documentationCenter=""
   authors="douglaslMS"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-server-stretch-database"
   ms.workload="data-management"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="06/14/2016"
   ms.author="douglaslMS"/>

# <a name="enable-transparent-data-encryption-tde-for-stretch-database-on-azure"></a>Włączanie szyfrowania przezroczysty danych (TDE) rozciąganie bazy danych Azure
> [AZURE.SELECTOR]
- [Azure portal](sql-server-stretch-database-encryption-tde.md)
- [TSQL](sql-server-stretch-database-tde-tsql.md)

Przezroczyste szyfrowanie danych (TDE) ułatwia, ochronę przed zagrożenia złośliwych działań, wykonując w czasie rzeczywistym szyfrowanie i odszyfrowywanie bazy danych, skojarzony kopie zapasowe oraz pliki dziennika transakcji w stanie spoczynku bez wprowadzania zmian w aplikacji.

TDE są szyfrowane na przechowywanie całej bazy danych przy użyciu klucza symetrycznej o nazwie klucza szyfrowania bazy danych. Wbudowane certyfikat klucza szyfrowania bazy danych jest chroniony. Wbudowane certyfikat jest unikatowy dla każdego Azure serwera. Microsoft automatycznie przełącza te certyfikaty, co najmniej raz 90 dni. Aby zapoznać się z ogólnym opisem TDE zobacz [Przezroczysty szyfrowania danych (TDE)].

##<a name="enabling-encryption"></a>Włączanie szyfrowania

Aby włączyć TDE dla Azure bazy danych, która jest przechowywania danych migracji z obsługą rozciąganie program SQL Server, wykonaj następujące czynności:

1. Otwórz bazę danych w [Azure portal](https://portal.azure.com)
2. W karta bazy danych kliknij przycisk **Ustawienia**
3. Wybierz opcję **szyfrowania danych przezroczystości**![][1]
4. Wybierz odpowiednie ustawienie **na** , a następnie wybierz **Zapisz**
![][2]


##<a name="disabling-encryption"></a>Wyłączanie szyfrowania

Aby wyłączyć TDE dla Azure bazy danych, która jest przechowywania danych migracji z obsługą rozciąganie program SQL Server, wykonaj następujące czynności:

1. Otwórz bazę danych w [Azure portal](https://portal.azure.com)
2. W karta bazy danych kliknij przycisk **Ustawienia**
3. Wybierz opcję **szyfrowania danych przezroczystości**
4. Wybierz odpowiednie ustawienie **wyłączone** , a następnie wybierz **Zapisz**




<!--Anchors-->
[Szyfrowanie danych przezroczysty (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx


<!--Image references-->
[1]: ./media/sql-server-stretch-database-encryption-tde/stretchtde1.png
[2]: ./media/sql-server-stretch-database-encryption-tde/stretchtde2.png


<!--Link references-->
