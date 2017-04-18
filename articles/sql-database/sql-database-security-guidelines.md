<properties
   pageTitle="Wytyczne dotyczące zabezpieczeń bazy danych Azure SQL i ograniczenia | Microsoft Azure"
   description="Informacje na temat zasady Microsoft Azure SQL Database i ograniczenia związane z zabezpieczeniami."
   services="sql-database"
   documentationCenter=""
   authors="BYHAM"
   manager="jhubbard"
   editor=""
   tags=""/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="10/18/2016"
   ms.author="rickbyh"/>

# <a name="azure-sql-database-security-guidelines-and-limitations"></a>Wytyczne dotyczące zabezpieczeń bazy danych SQL Azure i ograniczenia

W tym temacie opisano wskazówki Microsoft Azure SQL Database i ograniczenia związane z zabezpieczeniami. Podczas zarządzania zabezpieczeniami baz danych SQL Azure, należy uwzględnić następujące punkty.

## <a name="access-to-the-virtual-master-database"></a>Dostęp do wirtualnego wzorca bazy danych

Zazwyczaj tylko administratorzy muszą mieć dostęp do wzorca bazy danych. Procedury dostęp do bazy danych każdego użytkownika należy za pośrednictwem użytkownicy niebędący administratorami zawartych w bazie danych utworzone w każdej bazy danych. Użycie użytkowników zawartych bazy danych, nie musisz tworzyć logowania do wzorca bazy danych. Aby uzyskać więcej informacji zobacz [Zawarte bazy danych użytkowników — nawiązywanie i bazy danych przenośny](https://msdn.microsoft.com/library/ff929188.aspx).


## <a name="firewall"></a>Zapory

Zapora programu SQL Server, która jest ograniczone do całego programu SQL Server Azure zazwyczaj jest skonfigurowany za pośrednictwem portalu i tylko należy wpuść adresy IP używane przez administratorów. Aby utworzyć reguły zapory występujące bazy danych, którą otwiera niezbędne zakres adresów IP dla każdej bazy danych, połączenie z bazą danych użytkownika, a następnie użyj [sp_set_database_firewall_rule](https://msdn.microsoft.com/library/dn270010.aspx) instrukcji Transact-SQL.

Usługa bazy danych SQL Azure są dostępne tylko w TCP port 1433. Aby uzyskać dostęp do bazy danych SQL z komputera, upewnij się, że zapory komputera klienta umożliwia wychodzących komunikacji TCP na TCP port 1433. Jeśli nie jest potrzebny dla innych aplikacji, blokowanie połączeń przychodzących na TCP port 1433. 

W ramach procesu połączenia połączenia z Azure maszyn wirtualnych przekierowanie do innego adresu IP oraz portu, unikatowe dla każdej roli pracownika. Numer portu znajduje się w zakresie od 11000 do 11999. Aby uzyskać więcej informacji na temat portów TCP zobacz [porty poza 1433 4,5 ADO.NET i wersji 12 bazy danych SQL](sql-database-develop-direct-route-ports-adonet-v12.md).


## <a name="authentication"></a>Uwierzytelnianie

Użyj uwierzytelniania usługi Active Directory (zabezpieczenia zintegrowane) zawsze, gdy jest to możliwe. Aby uzyskać informacje na temat konfigurowania uwierzytelniania AD zobacz [Nawiązywanie połączenia z usługą SQL bazy danych przy użyciu Azure uwierzytelnianie usługi Active Directory](sql-database-aad-authentication.md)i [wybierając tryb uwierzytelniania](https://msdn.microsoft.com/library/ms144284.aspx) w dokumentacji SQL Server — książki Online. 

Użycia użytkowników bazy danych, aby zwiększyć skalowalność. Aby uzyskać więcej informacji zobacz [Zawarte użytkowników bazy danych — nawiązywanie i bazy danych Portable](https://msdn.microsoft.com/library/ff929188.aspx), [Utwórz użytkownika (Transact-SQL)](https://technet.microsoft.com/library/ms173463.aspx)i [Zawarte baz danych](https://technet.microsoft.com/library/ff929071.aspx).

Aparat bazy danych zamyka połączenia, które pozostają bezczynne przez ponad 30 minut. Połączenie musi Zaloguj się ponownie, zanim będzie można go używać.

Stale aktywnego połączenia z bazą danych SQL wymagają reauthorization (wykonywane przez aparat bazy danych) co najmniej raz 10 godzin. Aparat bazy danych pozwala podjąć próbę reauthorization przy użyciu hasła pierwotnie przesłanych i dane wejściowe użytkownika nie jest wymagane. Ze względu na wydajność, po zresetowaniu hasła w bazie danych SQL, połączenie jest nie być następuje ponowne uwierzytelnianie, nawet jeśli połączenie jest zresetowane ze względu na buforowanie połączeń. Różni się od zachowania lokalnego programu SQL Server. Jeśli hasło zostało zmienione, ponieważ połączenie zostało początkowo autoryzowane, połączenie musi zostać zakończone i nowe połączenie przy użyciu nowego hasła. Użytkownik mający uprawnienia zakończyć połączenie z bazą danych można jawnego zakończenia połączenia z bazą danych SQL przy użyciu polecenia [SKASOWAĆ](https://msdn.microsoft.com/library/ms173730.aspx) .

## <a name="logins-and-users"></a>Logowania i użytkowników

Podczas zarządzania logowania i użytkowników w bazie danych SQL, ma ograniczeń.


- Musisz mieć połączenie z **wzorcem** bazy danych podczas wykonywania ``CREATE/ALTER/DROP DATABASE`` instrukcji. -Użytkownika bazy danych wzorca odpowiadające logowania głównym poziomie serwera bazy danych nie można zmienić lub usunięte. 
- Angielski jest domyślny język logowania głównym poziomie serwera.
- Tylko Administratorzy (logowania głównym poziomie serwera lub administrator Azure AD) i członkowie **dbmanager** roli bazy danych w bazie danych **wzorca** mają uprawnienia do wykonania ``CREATE DATABASE`` i ``DROP DATABASE`` instrukcji.
- Musisz mieć połączenie z wzorcem bazy danych podczas wykonywania ``CREATE/ALTER/DROP LOGIN`` instrukcji. Jednak za pomocą logowania nie jest zalecane. Zamiast tego użyj użytkowników zawartych bazy danych.
- Aby połączyć się z bazą danych użytkownika, musisz podać nazwę bazy danych w parametrach połączenia.
- Tylko do logowania głównym poziomie serwera i członkowie **loginmanager** roli bazy danych w bazie danych **wzorcowej** mają uprawnienia do wykonania ``CREATE LOGIN``, ``ALTER LOGIN``, i ``DROP LOGIN`` instrukcji.
- Podczas wykonywania ``CREATE/ALTER/DROP LOGIN`` i ``CREATE/ALTER/DROP DATABASE`` instrukcje w aplikacji ADO.NET, za pomocą poleceń parametryczne nie jest dozwolone. Aby uzyskać więcej informacji zobacz [poleceń i parametrów](https://msdn.microsoft.com/library/ms254953.aspx).
- Podczas wykonywania ``CREATE/ALTER/DROP DATABASE`` i ``CREATE/ALTER/DROP LOGIN`` tych instrukcji instrukcji musi być tylko instrukcją w partii języku Transact-SQL. W przeciwnym razie wystąpi błąd. Na przykład w języku Transact-SQL sprawdza, czy istnieje bazy danych. Jeśli istnieje, ``DROP DATABASE`` jest nazwana usuwania bazy danych. Ponieważ ``DROP DATABASE`` instrukcja nie jest tylko instrukcji w partii, wykonywanie następujących instrukcji Transact-SQL powoduje błąd.

```
IF EXISTS (SELECT [name]
           FROM   [sys].[databases]
           WHERE  [name] = N'database_name')
     DROP DATABASE [database_name];
GO
```

- Podczas wykonywania ``CREATE USER`` instrukcji z ``FOR/FROM LOGIN`` opcji, musi istnieć tylko instrukcją w partii języku Transact-SQL.
- Podczas wykonywania ``ALTER USER`` instrukcji z ``WITH LOGIN`` opcji, musi istnieć tylko instrukcją w partii języku Transact-SQL.
- Aby ``CREATE/ALTER/DROP`` , użytkownik musi mieć ``ALTER ANY USER`` uprawnienie do bazy danych.
- Podczas dodawania lub usuwania innego użytkownika bazy danych z tej roli bazy danych lub właściciela roli bazy danych, może wystąpić następujący błąd: **użytkownika lub rolę "Nazwa" nie istnieje w bazie danych.** Ten błąd występuje, ponieważ użytkownik nie jest widoczny dla właściciela. Aby rozwiązać ten problem, należy udzielić właściciela roli ``VIEW DEFINITION`` uprawnienia użytkownika. 

Aby uzyskać więcej informacji na temat logowania i użytkowników zobacz [Zarządzanie bazami danych i logowania w bazie danych SQL Azure](sql-database-manage-logins.md).


## <a name="see-also"></a>Zobacz też

[Zapora bazy danych Azure SQL](sql-database-firewall-configure.md)

[Jak: Konfigurowanie ustawień zapory (baza danych SQL Azure)](sql-database-configure-firewall-settings.md)

[Zarządzanie bazami danych i logowania w bazie danych SQL Azure](sql-database-manage-logins.md)

[Centrum zabezpieczeń dla aparatu bazy danych programu SQL Server i baza danych SQL Azure](https://msdn.microsoft.com/library/bb510589)
