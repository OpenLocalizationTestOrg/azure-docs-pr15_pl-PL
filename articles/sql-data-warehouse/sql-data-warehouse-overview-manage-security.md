<properties
   pageTitle="Zabezpieczanie bazy danych w magazynie danych SQL | Microsoft Azure"
   description="Porady dotyczące Zabezpieczanie bazy danych w magazynie danych SQL Azure dla opracowania rozwiązań."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="ronortloff"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/24/2016"
   ms.author="rortloff;barbkess;sonyama"/>

# <a name="secure-a-database-in-sql-data-warehouse"></a>Zabezpieczanie bazy danych w magazynie danych SQL

> [AZURE.SELECTOR]
- [Omówienie zabezpieczeń](sql-data-warehouse-overview-manage-security.md)
- [Uwierzytelnianie](sql-data-warehouse-authentication.md)
- [Szyfrowanie (Portal)](sql-data-warehouse-encryption-tde.md)
- [Szyfrowanie (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)

W tym artykule opisano podstawy zabezpieczania bazy danych magazynu danych SQL Azure. W szczególności, w tym artykule spowoduje rozpoczęcie pracy z zasobów dla ograniczanie dostępu, ochrona danych i monitorowanie aktywności w bazie danych.

## <a name="connection-security"></a>Zabezpieczenia połączeń

Zabezpieczenia połączeń odwołuje się do jak ograniczać i bezpiecznego połączenia z bazą danych przy użyciu reguły zapory i szyfrowania połączenia.

Reguły zapory są używane zarówno przez serwer i bazę danych do odrzucenia próby nawiązania połączenia z adresów IP, które nie zostały jawnie whitelisted. Do zezwalania na połączenia z aplikacją lub publiczny adres IP komputera klienta, musisz najpierw utworzyć reguły zapory na poziomie serwera przy użyciu Azure Portal, interfejsu API usługi REST lub programu PowerShell. Zgodnie z zaleceniami dotyczącymi należy ograniczyć dozwolone przez zaporę serwera możliwie zakresów adresów IP.  Aby uzyskać dostęp do magazynu danych SQL Azure z komputera lokalnego, upewnij się, że zapory w sieci i komputera lokalnego umożliwia komunikacji wychodzącej na TCP port 1433.  Aby uzyskać więcej informacji zobacz [zapory bazy danych SQL Azure][], [sp_set_firewall_rule][]i [sp_set_database_firewall_rule][].

Połączenia z magazynu danych SQL są domyślnie szyfrowane.  Modyfikowanie ustawienia połączenia, aby wyłączyć szyfrowanie są ignorowane.

## <a name="authentication"></a>Uwierzytelnianie

Uwierzytelnianie odwołuje się do jak potwierdza tożsamość użytkownika podczas nawiązywania połączenia z bazą danych. Program SQL Data Warehouse obecnie obsługuje uwierzytelnianie programu SQL Server przy użyciu nazwy użytkownika i hasło, a także usługi Azure Active Directory. 

Po utworzeniu logiczne server dla bazy danych za pomocą nazwy użytkownika i hasła określić logowania "administrator serwera". Przy użyciu tych poświadczeń, może przeprowadzać uwierzytelnianie do dowolnej bazy danych na tym serwerze jako właściciel bazy danych lub "dbo" za pośrednictwem uwierzytelnianie programu SQL Server.

Jednak zgodnie z zaleceniami dotyczącymi użytkowników w organizacji powinien Użyj innego konta do uwierzytelnienia. W ten sposób można ograniczyć uprawnienia do aplikacji i zmniejszyć ryzyko złośliwych działań w przypadku, gdy występuje ataki uruchomienie SQL w kodzie aplikacji. 

Aby utworzyć użytkownika uwierzytelnianie programu SQL Server, nawiązywanie połączenia z **wzorcem** bazy danych na serwerze z logowanie administrator serwera, a następnie utworzyć nowy identyfikator logowania server.  Ponadto jest dobrym pomysłem jest utworzenie użytkownika w główną bazę danych użytkowników magazynu danych SQL Azure. Tworzenie użytkownika we wzorcu umożliwia użytkownikowi zalogować się za pomocą narzędzi, takich jak SSMS bez określenia nazwy bazy danych.  Umożliwia także wyświetlić za pomocą Eksploratora obiektów wszystkie bazy danych w programie SQL server.

```sql
-- Connect to master database and create a login
CREATE LOGIN ApplicationLogin WITH PASSWORD = 'Str0ng_password';
CREATE USER ApplicationUser FOR LOGIN ApplicationLogin;
```

Następnie należy nawiązać połączenie z **bazą danych SQL Data Warehouse** logowanie administrator serwera i Utwórz użytkownika bazy danych oparty na nowo utworzonej logowanie do serwera.

```sql
-- Connect to SQL DW database and create a database user
CREATE USER ApplicationUser FOR LOGIN ApplicationLogin;
```

Jeśli użytkownik będzie wykonanie dodatkowych operacji, takich jak tworzenie logowania lub tworzenia nowych baz danych, również muszą być przypisane do `Loginmanager` i `dbmanager` ról w głównym bazy danych. Aby uzyskać więcej informacji o tych rolach dodatkowe i uwierzytelnianie z bazą danych SQL zobacz [Zarządzanie bazami danych i logowania w bazie danych SQL Azure][].  Aby uzyskać więcej szczegółów dotyczących Azure AD dla magazynu danych SQL zobacz [Nawiązywanie połączenia z usługą SQL danych magazynu przy użyciu Azure uwierzytelnianie usługi Active Directory][].


## <a name="authorization"></a>Autoryzacja

Autoryzacja odwołuje się do co można zrobić w bazie danych magazynu danych SQL Azure, a to jest kontrolowane przez swoje konto użytkownika członkostwo ról i uprawnień. Zgodnie z zaleceniami dotyczącymi należy zezwolić użytkownikom co najmniej uprawnienia wymagane. Magazyn danych SQL Azure ułatwia zarządzanie rolami w T-SQL:

```sql
EXEC sp_addrolemember 'db_datareader', 'ApplicationUser'; -- allows ApplicationUser to read data
EXEC sp_addrolemember 'db_datawriter', 'ApplicationUser'; -- allows ApplicationUser to write data
```

Konta administratora serwera, którego łączysz się z jest członkiem db_owner, która ma uprawnienia do wykonywać żadnych w bazie danych. Zapisz to konto do wdrażania uaktualnień schematu i innych operacji zarządzania. Za pomocą konta "ApplicationUser" bardziej ograniczone uprawnienia do nawiązywania połączeń z poziomu aplikacji do bazy danych z co najmniej uprawnienia wymagane przez aplikację.

Istnieją sposoby zawęzić, co może wykonać użytkownik z bazy danych SQL Azure:

- Szczegółowego [uprawnienia][] umożliwiają sterowanie operacje możesz wykonać na poszczególnych kolumn, tabel, widoków, procedur i innych obiektów w bazie danych. Za pomocą szczegółowego uprawnienia do większości kontrolować i udzielanie minimalne wymagane uprawnienia. System szczegółowego uprawnień staje się nieco bardziej złożone i wymaga niektórych analizy efektywnie używać.
- [Role bazy danych][] inne niż db_datareader i db_datawriter umożliwia tworzenie bardziej zaawansowanych kont użytkowników aplikacji lub mniej zaawansowanych zarządzania kontami. Role wbudowanych stałej bazy danych z łatwością udzielanie uprawnień, ale może spowodować udzielanie uprawnień więcej niż jest to konieczne.
- [Procedury przechowywane][] umożliwia ograniczenie działań, które należy podjąć w bazie danych.

Zarządzanie bazami danych i serwerów logicznych z portalu klasyczny Azure lub za pomocą interfejsu API Menedżera zasobów Azure steruje przypisania roli konta użytkownika portalu. Aby uzyskać więcej informacji na ten temat zobacz [Kontrola dostępu oparta na rolach w Azure Portal][].

## <a name="encryption"></a>Szyfrowanie

Azure SQL danych magazynu przezroczysty danych szyfrowania (TDE) ułatwia ochronę przed zagrożenia złośliwych działań, wykonując w czasie rzeczywistym szyfrowania i odszyfrowywania danych spoczynku.  Szyfrowanie bazy danych, są szyfrowane i skojarzone kopie zapasowe plików dziennika transakcji bez konieczności zmiany wprowadzone w aplikacji. TDE są szyfrowane na przechowywanie całej bazy danych przy użyciu klucza symetrycznej o nazwie klucza szyfrowania bazy danych. W bazie danych SQL klucza szyfrowania bazy danych jest chroniony wbudowanych certyfikat. Wbudowane certyfikat jest unikatowy dla każdej bazy danych programu SQL server. Microsoft automatycznie przełącza te certyfikaty, co najmniej raz 90 dni. Algorytm szyfrowania używany przez program SQL Data Warehouse jest AES-256. Aby zapoznać się z ogólnym opisem TDE zobacz [Przezroczyste szyfrowanie danych][].

Można zaszyfrować bazę danych za pomocą [Azure Portal] [ Encryption with Portal] lub [T-SQL][Encryption with TSQL].

## <a name="next-steps"></a>Następne kroki

Aby uzyskać szczegółowe informacje i przykłady dotyczące łączenia się magazynu danych SQL z różnych protokołów zobacz [Łączenie z magazynu danych SQL][].

<!--Image references-->

<!--Article references-->
[Nawiązywanie połączenia z magazynem danych SQL]: ./sql-data-warehouse-connect-overview.md
[Encryption with Portal]: ./sql-data-warehouse-encryption-tde.md
[Encryption with TSQL]: ./sql-data-warehouse-encryption-tde-tsql.md
[Łączenie z magazynu danych SQL za pomocą uwierzytelniania usługi Azure Active Directory]: ./sql-data-warehouse-authentication.md

<!--MSDN references-->
[Zapora bazy danych SQL Azure]: https://msdn.microsoft.com/library/ee621782.aspx
[sp_set_firewall_rule]: https://msdn.microsoft.com/library/dn270017.aspx
[sp_set_database_firewall_rule]: https://msdn.microsoft.com/library/dn270010.aspx
[Role bazy danych]: https://msdn.microsoft.com/library/ms189121.aspx
[Zarządzanie bazami danych i logowania w bazie danych SQL Azure]: https://msdn.microsoft.com/library/ee336235.aspx
[Uprawnienia]: https://msdn.microsoft.com/library/ms191291.aspx
[Procedury składowane]: https://msdn.microsoft.com/library/ms190782.aspx
[Szyfrowanie danych przezroczystości]: https://msdn.microsoft.com/library/bb934049.aspx
[Azure portal]: https://portal.azure.com/

<!--Other Web references-->
[Kontrola dostępu oparta na rolach w Azure Portal]: https://azure.microsoft.com/documentation/articles/role-based-access-control-configure
