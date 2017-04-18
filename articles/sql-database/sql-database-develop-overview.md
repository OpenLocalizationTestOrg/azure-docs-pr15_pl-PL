<properties
    pageTitle="Baza danych SQL opracowywanie omówienie | Microsoft Azure"
    description="Więcej informacji na temat bibliotek dostępne łączności i najważniejsze wskazówki dotyczące aplikacji nawiązywanie połączenia z bazą danych SQL."
    services="sql-database"
    documentationCenter=""
    authors="annemill"
    manager="jhubbard"
    editor="genemi"/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/17/2016"
    ms.author="annemill"/>

# <a name="sql-database-development-overview"></a>Omówienie tworzenia bazy danych SQL
W tym artykule opisano podstawowe zagadnienia Deweloper należy pamiętać podczas pisania kodu nawiązywania połączenia z bazą danych SQL Azure.

## <a name="language-and-platform"></a>Język i platformy
Przykłady kodu są dostępne dla różnych języków programowania i platformy. Można znaleźć linki do przykłady kodu na: 

* Więcej informacji: [Biblioteki połączeń dla bazy danych SQL i programu SQL Server](sql-database-libraries.md)

## <a name="resource-limitations"></a>Ograniczenia dotyczące zasobów
Baza danych SQL Azure zarządza zasobów dostępnych dla bazy danych przy użyciu dwa różne mechanizmy: zarządzania zasobami i wymuszania ograniczenia.

* Więcej informacji: [Limity zasobów bazy danych SQL Azure](sql-database-resource-limits.md)

## <a name="security"></a>Zabezpieczenia
Baza danych SQL Azure zawiera zasoby dotyczące ograniczanie dostępu, ochrona danych i monitorowanie aktywności w bazie danych SQL.

* Więcej informacji: [Zabezpieczanie bazy danych SQL](sql-database-security.md)

## <a name="authentication"></a>Uwierzytelnianie
* Baza danych SQL Azure obsługuje użytkownicy uwierzytelnianie programu SQL Server i logowania, a także [usługi Azure Active Directory uwierzytelniania](sql-database-aad-authentication.md) użytkowników i logowania.
* Należy określić konkretnej bazy danych, zamiast domyślnie z *wzorcem* bazy danych.
* Za pomocą instrukcji Transact-SQL, **UŻYJ myDatabaseName;** w bazie danych SQL nie można przełączyć się do innej bazy danych.
* Więcej informacji: [zabezpieczeń bazy danych SQL: Zarządzanie zabezpieczeniami bazy danych programu access i logowania](sql-database-manage-logins.md)

## <a name="resiliency"></a>Elastyczność
W przypadku wystąpienia błędu przejściowych podczas nawiązywania połączenia z bazą danych SQL kodu należy ponownie połączenie.  Zalecamy który ponawiania logiczny Użyj wycofywania, tak, aby nie zasypać bazy danych SQL z wieloma klientami ponawianie próby jednocześnie.

* Przykłady kodu: przykłady kodu, ilustrujące ponowić próbę logicznych, można znaleźć w próbkach języka wyboru u: [biblioteki połączeń dla bazy danych SQL i programu SQL Server](sql-database-libraries.md)
* Więcej informacji: [komunikaty o błędach dla programów klienckich bazy danych SQL](sql-database-develop-error-messages.md)

## <a name="managing-connections"></a>Zarządzanie połączeniami
* W logiki połączenia klienta zastąpić domyślny limit czasu do 30 sekund.  Domyślnie 15 sekundach jest zbyt krótka dla połączeń, zależnych w Internecie.
* Jeśli korzystasz z [puli połączeń](http://msdn.microsoft.com/library/8xx3tyca.aspx), należy zamknąć połączenie moment program nie jest korzysta się aktywnie z go, a nie przygotowanie do jego ponownego użycia.

## <a name="network-considerations"></a>Uwagi dotyczące sieciowych
* Na komputerze, na którym znajduje się program Klient upewnij się, że zapora zezwala wychodzących komunikacji TCP na porcie 1433.  Więcej informacji: [Konfigurowanie zapory bazy danych SQL Azure](sql-database-configure-firewall-settings.md)
* Jeśli używany program klient nawiązuje połączenie bazy danych SQL w wersji 12 Klient uruchomiony na Azure maszyn wirtualnych (maszyn wirtualnych), należy otworzyć określone zakresy portów na maszyn wirtualnych. Więcej informacji: [porty inne niż 1433 4,5 ADO.NET i wersji 12 bazy danych SQL](sql-database-develop-direct-route-ports-adonet-v12.md)
* Czasami klienta połączenia z bazą danych SQL Azure w wersji 12 pominąć serwer proxy i pracować bezpośrednio na bazę danych. Porty inne niż 1433 stają się ważne. Więcej informacji: [porty inne niż 1433 4,5 ADO.NET i wersji 12 bazy danych SQL](sql-database-develop-direct-route-ports-adonet-v12.md)

## <a name="data-sharding-with-elastic-scale"></a>Sharding danych ze skalą elastyczne
Elastyczne skali upraszcza proces skalowania się (a w). 

* [Desenie projektu dla wielu dzierżawy władz akredytacji bezpieczeństwa aplikacji z bazy danych Azure SQL](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* [Dane zależne routingu](sql-database-elastic-scale-data-dependent-routing.md)
* [Rozpoczynanie pracy z podglądem elastyczne skali bazy danych Azure SQL](sql-database-elastic-scale-get-started.md)

## <a name="next-steps"></a>Następne kroki

Zapoznaj się wszystkie [Funkcje bazy danych SQL](https://azure.microsoft.com/services/sql-database/).
