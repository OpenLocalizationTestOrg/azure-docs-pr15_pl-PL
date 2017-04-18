<properties
   pageTitle="Nieobsługiwane w bazie danych Azure SQL T-SQL | Microsoft Azure"
   description="Instrukcje języka Transact-SQL, mniej niż w pełni obsługiwane w bazie danych SQL Azure"
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
   ms.date="08/30/2016"
   ms.author="rick.byham@microsoft.com"/>

# <a name="azure-sql-database-transact-sql-differences"></a>Różnice bazy danych SQL języku Transact-SQL Azure


Większość funkcji Transact-SQL, które aplikacje są zależne od są obsługiwane zarówno programu Microsoft SQL Server i bazy danych SQL Azure. Częściowy listę obsługiwanych funkcji aplikacji są następujące:

- Typy danych.
- Operatory.
- Funkcje ciąg, arytmetyczne logiczne i kursora.

Jednak bazy danych SQL Azure jest zaprojektowane w celu wyodrębnienia funkcji z dowolnego zależność **wzorca** bazy danych. W konsekwencji wiele działania na poziomie serwera nie mają zastosowania do bazy danych SQL i nie są obsługiwane. Funkcje, które są przestarzałe w programie SQL Server zwykle nie są obsługiwane w bazie danych SQL.

> [AZURE.NOTE]
> W tym temacie opisano funkcje, które są dostępne z bazą danych SQL po uaktualnieniu do bieżącej wersji; W wersji 12 bazy danych SQL. Aby uzyskać więcej informacji na temat wersji 12, zobacz [SQL bazy danych w wersji 12 co na nowy](sql-database-v12-whats-new.md).

W poniższych sekcjach wymieniono funkcje, które jest częściowo obsługiwane i funkcje, które nie są całkowicie obsługiwane.


## <a name="features-partially-supported-in-sql-database-v12"></a>Funkcje częściowo obsługiwane w wersji 12 bazy danych SQL

Baza danych SQL w wersji 12 obsługuje niektórych, ale nie wszystkie argumenty znajdują się w odpowiednich instrukcji SQL Server 2016 Transact-SQL. Na przykład instrukcja CREATE PROCEDURE jest dostępna, jednak wszystkich opcji CREATE PROCEDURE nie są dostępne. Zapoznaj się z tematów połączony składni szczegółowe informacje na temat obsługiwanych obszarów każdej instrukcji.

- Bazy danych: [Tworzenie](https://msdn.microsoft.com/library/dn268335.aspx )/[zmiany](https://msdn.microsoft.com/library/ms174269.aspx)
- DMVs są powszechnie dostępne dla funkcji, które są dostępne.
- Funkcje: [Tworzenie](https://msdn.microsoft.com/library/ms186755.aspx)/[zmienić funkcji](https://msdn.microsoft.com/library/ms186967.aspx)
- [SKASOWAĆ](https://msdn.microsoft.com/library/ms173730.aspx) 
- Logowania: [Tworzenie](https://msdn.microsoft.com/library/ms189751.aspx)/[zmiany logowania](https://msdn.microsoft.com/library/ms189828.aspx)
- Procedury składowane: [Tworzenie](https://msdn.microsoft.com/library/ms187926.aspx)/[ALTER PROCEDURE](https://msdn.microsoft.com/library/ms189762.aspx)
- Tabele: [Tworzenie](https://msdn.microsoft.com/library/dn305849.aspx)/[zmiany](https://msdn.microsoft.com/library/ms190273.aspx)
- Typy (niestandardowy): [Tworzenie typu](https://msdn.microsoft.com/library/ms175007.aspx)
- Użytkownicy: [Tworzenie](https://msdn.microsoft.com/library/ms173463.aspx)/[zmiany użytkownika](https://msdn.microsoft.com/library/ms176060.aspx)
- Widoki: [Tworzenie](https://msdn.microsoft.com/library/ms187956.aspx)/[zmiany WIDOKU](https://msdn.microsoft.com/library/ms173846.aspx)

## <a name="features-not-supported-in-sql-database"></a>Funkcje nieobsługiwane w bazie danych SQL

- Sortowanie obiektów systemu
- Połączenia dotyczące: punkt końcowy instrukcji ORIGINAL_DB_NAME. Baza danych SQL nie obsługuje uwierzytelnianie systemu Windows, ale obsługuje podobne uwierzytelniania usługi Azure Active Directory. Niektóre typy uwierzytelniania wymagają najnowszą wersję programu SSMS. Aby uzyskać więcej informacji zobacz [Nawiązywanie połączenia z bazą danych SQL lub SQL danych magazynu przy użyciu Azure uwierzytelnianie usługi Active Directory](sql-database-aad-authentication.md).
- Krzyżowe zapytań bazy danych za pomocą trzy lub cztery nazwy części. (Tylko do odczytu zapytania między bazami danych są obsługiwane za pomocą [kwerendy elastyczne bazy danych](sql-database-elastic-query-overview.md)).
- Tworzenie łańcucha własności między bazami danych, ustawienie w WIARYGODNY
- Modułów zbierających dane
- Diagramy bazy danych
- Poczta bazy danych
- DATABASEPROPERTY (zamiast tego użyj DATABASEPROPERTYEX)
- Logowania EXECUTE AS
- Szyfrowania: extensible zarządzania kluczami
- Obsługi zdarzeń: zdarzenia, powiadomień o zdarzeniach, powiadomienia o kwerendach
- Funkcje związane z położenie pliku bazy danych, rozmiar i pliki bazy danych, które są zarządzane automatycznie przez Microsoft Azure.
- Funkcje, które dotyczą wysokiej dostępności, która odbywa się za pomocą konta Microsoft Azure: kopii zapasowej, Przywracanie (AlwaysOn), odzwierciedlające bazy danych, wysyłanie, dziennika trybach odzyskiwania. Aby uzyskać więcej informacji, zobacz Kopia zapasowa bazy danych SQL Azure i przywracania.
- Funkcje, które opierają się na czytnika dziennika uruchomionych dla bazy danych SQL: Push replikacji, rejestrowania zmian danych.
- Funkcje, które opierają się na agenta programu SQL Server lub bazy danych MSDB: zadania, alerty i operatorów, na podstawie zasad zarządzania, bazy danych poczty, serwerów centralnego zarządzania.
- FILESTREAM
- Funkcje: fn_get_sql, fn_virtualfilestats fn_virtualservernodes
- Globalne tabele tymczasowe
- Ustawienia serwera związane ze sprzętem: pamięci wątków, koligacje Procesora, flag śledzenia, itp. Zamiast tego użyj poziomów usług.
- HAS_DBACCESS
- SKASOWAĆ STATYSTYKĘ ZADANIA
- Serwerów połączonych, OPENQUERY OPENROWSET, OPENDATASOURCE, wstawianie zbiorcze i czteroczęściowa nazw
- Serwery wzorca/docelowej
- .NET framework [CLR integracji z programem SQL Server](http://msdn.microsoft.com/library/ms254963.aspx)
- Gubernator zasobów
- Semantyczny wyszukiwania
- Poświadczenia serwera. Użyj bazy danych zamiast ograniczone poświadczeń.
- Elementy na poziomie serwera: serwer role, IS_SRVROLEMEMBER, sys.login_token. Uprawnienia na poziomie serwera nie są dostępne, ale niektóre są zastępowane przez uprawnienia na poziomie bazy danych. Niektóre DMVs poziomie serwera nie są dostępne, ale niektóre są zastępowane przez DMVs poziom bazy danych.
- Pliki express: localdb, wystąpienia użytkownika
- Usługa Service broker
- USTAWIANIE REMOTE_PROC_TRANSACTIONS
- ZAMKNIĘCIE
- sp_addmessage
- Opcje sp_configure i ponownej konfiguracji. Niektóre opcje są dostępne za pomocą [Zmiany konfiguracji występujące bazy danych](https://msdn.microsoft.com/library/mt629158.aspx).
- sp_helpuser
- sp_migrate_user_to_contained
- Inspekcja programu SQL Server. Za pomocą bazy danych SQL zamiast inspekcja.
- Program SQL Server Profiler
- Śledzenie programu SQL Server
- Śledzenie flagi. Niektóre elementy flagi śledzenia zostały przeniesione do trybem zgodności.
- Debugowanie Transact-SQL
- Wyzwalacze: O zakresie serwera lub Wyzwalacze logowania
- Używanie instrukcji: Aby zmienić kontekstu bazy danych na inną bazę danych, musisz wprowadzić nowe połączenie z bazą danych.


## <a name="full-transact-sql-reference"></a>Pełne odwołanie w języku Transact-SQL

Aby uzyskać więcej informacji na temat gramatyki języku Transact-SQL, zastosowania i przykłady zobacz [Odwołanie języku Transact-SQL (aparat bazy danych)](https://msdn.microsoft.com/library/bb510741.aspx) w dokumentacji SQL Server — książki Online. 

### <a name="about-the-applies-to-tags"></a>Informacje o znaczników "Dotyczy"

Odwołanie języku Transact-SQL zawiera tematów związanych z bieżącej wersji programu SQL Server 2008. Pod tytułem tematu jest ikona na pasku, lista cztery platformy SQL Server oraz wskazująca stosowanie. Na przykład grupy dostępności zostały wprowadzone w wersji SQL Server 2012. Temat [Utwórz GRUPĘ AVAILABILTY](https://msdn.microsoft.com/library/ff878399.aspx) wskazuje, czy dotyczy instrukcji ** programu SQL Server (począwszy od 2012). Nie dotyczy instrukcji SQL Server 2008, SQL Server 2008 R2, bazy danych SQL Azure, magazynu danych SQL Azure lub Parallel Data Warehouse.

W niektórych przypadkach temat tematu mogą być używane w produkt, ale istnieje niewielkie różnice między produktami. Różnice są oznaczone na punkty środkowe w tym temacie, zależnie od potrzeb.

