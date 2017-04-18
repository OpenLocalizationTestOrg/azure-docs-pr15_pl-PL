<properties
    pageTitle="Co nowego w wersji 12 bazy danych SQL | Microsoft Azure"
    description="W tym artykule opisano, dlaczego korzyść systemów biznesowych, które używają bazy danych SQL Azure w chmurze przez uaktualnienie do wersji w wersji 12 jest teraz."
    services="sql-database"
    documentationCenter=""
    authors="MightyPen"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="genemi"/>


# <a name="whats-new-in-sql-database-v12"></a>Co nowego w wersji 12 bazy danych SQL


W tym temacie opisano wiele zalet, mające na wersji V11 nowej wersji wersji 12 bazy danych SQL Azure.


W dalszym ciągu Dodawanie funkcji do wersji 12. Dlatego zachęcamy odwiedź nasze aktualizacje usług sieci Web dla Azure i używania jej filtrów:


- Filtrowane, aby [Usługa bazy danych SQL](https://azure.microsoft.com/updates/?service=sql-database).
- Filtrowane, aby ogólnodostępną [Anonsy (GA)](http://azure.microsoft.com/updates/?service=sql-database&update-type=general-availability) funkcji bazy danych SQL.


Najnowsze informacje o limitach zasobu bazy danych SQL jest opisane w:<br/>[Limity dotyczące zasobów bazy danych azure SQL](sql-database-resource-limits.md).


## <a name="increased-application-compatibility-with-sql-server"></a>Zwiększona aplikacji zgodności z programem SQL Server


Podstawowym celem dla bazy danych SQL w wersji 12 jest poprawianie zgodności z programu Microsoft SQL Server 2014 i zachować zgodność podczas nowe wersje programu SQL Server. Wśród innych obszarów w wersji 12 uzyskuje zgodności z programu SQL Server w obszarze ważne programowania. Na przykład:

- [Obsługa JSON](https://msdn.microsoft.com/library/dn921897.aspx)

- [Funkcje okna](http://msdn.microsoft.com/library/ms189798.aspx), z [nad](http://msdn.microsoft.com/library/ms189461.aspx)

- [Indeksy XML](http://msdn.microsoft.com/library/bb934097.aspx) i [selektywnego indeksów XML](http://msdn.microsoft.com/library/jj670104.aspx)

- [Śledzenie zmian](http://msdn.microsoft.com/library/bb933875.aspx)

- [WYBIERZ... DO](http://msdn.microsoft.com/library/ms188029.aspx)

- [Wyszukiwanie pełnotekstowe](http://msdn.microsoft.com/library/ms142571.aspx)

- [ZMIANY konfiguracji POLU bazy danych (Transact-SQL)](http://msdn.microsoft.com/library/mt629158.aspx)

Zobacz [tutaj](sql-database-transact-sql-information.md) małego zestawu funkcje, które nie są jeszcze obsługiwane w bazie danych SQL.


### <a name="compatibility-level-130"></a>Poziom zgodności 130


> [AZURE.IMPORTANT] *Nowo* utworzony baz danych w wersji 12 bazy danych SQL Azure uruchamiany, w **czerwcu 2016**, mają poziom zgodności, Rozpocznij od 130, zostanie dodana do programu Microsoft SQL Server 2016 GA.
> 
> Możesz użyć `ALTER DATABASE YourDatabase SET COMPATIBILITY_LEVEL = 120` Jeśli wolisz.
> 
> Baz danych utworzonych przed czerwca 2016 nie mają poziom zgodności zmieniona przez tę zmianę domyślnego. Nie jest poziom zmienione przez uaktualnienie z V11 do wersji 12 bazy danych.



Aby uzyskać wyjaśnienie Porównanie zapytań najważniejszych między najnowsze informacje w stosunku do poprzedniego poziomu zgodności zobacz:

- [Ulepszone kwerendy wydajności za pomocą poziom zgodności 130 w bazie danych Azure SQL](sql-database-compatibility-level-query-performance-130.md)



## <a name="more-premium-performance-new-performance-levels"></a>Większa wydajność premium, nowe poziomy wydajności


W wersji 12 możemy zwiększona jednostki transakcji bazy danych (DTUs) przydzielona do wszystkich poziomów wydajności Premium o 25% bez dodatkowych opłat. Nowe funkcje, takie jak uzyskuje się jeszcze większą zwiększenie wydajności:


- Obsługa w pamięci [columnstore indeksy](http://msdn.microsoft.com/library/gg492153.aspx).
- [Tabela podziału według wierszy](http://msdn.microsoft.com/library/ms187802.aspx) z powiązanych ulepszenia [OBCIĄĆ tabelę](http://msdn.microsoft.com/library/ms177570.aspx).
- Dostępność dynamiczne zarządzanie widoków [(DMVs)](http://msdn.microsoft.com/library/ms188754.aspx) , aby umożliwić monitorowanie i dostosowywanie wydajności.


### <a name="reliable-performance"></a>Niezawodne wydajności


Jeśli używany program klient nawiązuje połączenie bazy danych SQL w wersji 12 Klient uruchomiony na Azure maszyn wirtualnych (maszyn wirtualnych), należy otworzyć z następujących zakresów portów na maszyn wirtualnych:

- 11000 11999
- 14000 14999


Kliknij [tutaj](sql-database-develop-direct-route-ports-adonet-v12.md) szczegółowe informacje na temat porty dla wersji 12 bazy danych SQL. Porty są wymagane przez ulepszenia wydajności w wersji 12 bazy danych SQL.


## <a name="better-support-for-cloud-saas-vendors"></a>Lepsza obsługa chmury dostawców władz akredytacji bezpieczeństwa


Tylko w wersji 12 firma Microsoft opublikowała nowy poziom wydajności standardowej S3 i publicznej Podgląd [pul elastyczne bazy danych](sql-database-elastic-pool.md). Pule elastyczne bazy danych jest rozwiązanie przeznaczone do chmury władz akredytacji bezpieczeństwa dostawców.  Za pomocą pul elastyczne bazy danych można:


- Udostępnianie DTUs między bazami danych w celu zmniejszenia kosztów dla dużej liczby baz danych.
- Wykonywanie [zadań elastyczne baz danych](sql-database-elastic-jobs-overview.md) do zarządzanie bazami danych w skali.


## <a name="security-enhancements"></a>Ulepszenia zabezpieczeń


Bezpieczeństwo jest podstawowy znaczenie dla każdego, kto jest uruchamiany ich działalności w chmurze. Najnowsze funkcje zabezpieczeń wydane w wersji 12:


- [Zabezpieczenia na poziomie wiersza](http://msdn.microsoft.com/library/dn765131.aspx) (KONTROLA DOSTĘPU)
- [Maskowanie danych dynamicznych](sql-database-dynamic-data-masking-get-started.md)
- [Zamkniętego baz danych](http://msdn.microsoft.com/library/ff929188.aspx)
- [Role aplikacji](http://msdn.microsoft.com/library/ms190998.aspx) zarządzane przy użyciu funkcji udzielanie, ODMÓW, ODWOŁYWANIE
- [Szyfrowanie danych przezroczystości](http://msdn.microsoft.com/library/0bf7e8ff-1416-4923-9c4c-49341e208c62.aspx) (TDE)
- [Nawiązywanie połączenia z bazą danych SQL za pomocą uwierzytelniania usługi Azure Active Directory](sql-database-aad-authentication.md)
 - Baza danych SQL obsługuje teraz uwierzytelniania usługi Azure Active Directory, mechanizmu nawiązywanie połączenia z bazą danych SQL przy użyciu tożsamości w usłudze Azure Active Directory (Azure AD). Uwierzytelnianie usługi Azure Active Directory możesz centralnie zarządzać tożsamości bazy danych użytkowników i innych usług firmy Microsoft w jednej centralnej lokalizacji.
- [Zawsze szyfrowane](https://msdn.microsoft.com/library/mt163865.aspx) (w wersji preview) powoduje szyfrowania przezroczysty aplikacji i umożliwia klientom do szyfrowania poufnych danych w aplikacjach klienckich bez udostępniania kluczy szyfrowania z bazą danych SQL.


## <a name="increased-business-continuity-when-recovery-is-needed"></a>Zwiększenie ciągłości, gdy jest wymagana odzyskiwania


W wersji 12 oferuje ulepszone odzyskiwania punktu cele (RPOs) i odzyskiwania szacowany czas (ERTs):


| Funkcja ciągłości biznesowej | Wcześniejszej wersji | W WERSJI 12 |
| :-- | :-- | :-- |
| Przywracanie Geo | • RPO < 24 godziny.<br/>• Wstaw < 12 godzin. | • RPO < 1 godzina.<br/>• Wstaw < 12 godzin. |
| Replikacja Geo Active | • RPO < 5 minut.<br/>• Wstaw < 1 godzina. | • RPO < 5 sekund.<br/>• Wstaw < 30 sekund. |


Aby uzyskać więcej informacji, zobacz [Zapewnianie ciągłości bazy danych SQL](sql-database-business-continuity.md) .


## <a name="more-reasons-to-upgrade-now"></a>Więcej powodów do uaktualnienia


Istnieje wiele powodów, dlaczego klientów należy uaktualnić teraz do wersji 12 bazy danych SQL Azure z V11:


- Baza danych SQL w wersji 12 występują o dużej liczbie funkcje oprócz funkcji V11.
- Kontynuować dodać nowe funkcje do wersji 12, ale nie nowych funkcji, które zostaną dodane do V11.
- Większość nowych funkcji opublikowane w wersji 12 bazy danych SQL przed wprowadzeniem jest dla programu Microsoft SQL Server.


## <a name="are-you-using-v12-already"></a>Używasz wersji 12 jest już?


Łatwym sposobem czy masz bazy danych lub logiczne serwerem w starszej wersji usługi bazy danych SQL jest wykonanie następujących czynności:


1. Przejdź do [portalu Azure](https://portal.azure.com/).
2. Kliknij przycisk **Przeglądaj**.
3. Kliknij pozycję **serwery SQL Server**.
4. Ikona obok serwera lub bazy danych zawiera artykuł:
 - ![Ikona server w wersji 12](./media/sql-database-v12-whats-new/v12_icon.png) **serwera logicznych w wersji 12**
 - ![Ikona wcześniejszych wersji serwera](./media/sql-database-v12-whats-new/earlier_icon.png) **wcześniejszych wersji serwera logicznych.**


Inną techniką ustalić wersji ma działać jako `SELECT @@version;` instrukcji w bazie danych oraz wyświetlić wyniki podobne do:


- **12**.0.2000.10 &nbsp; *(wersja wersji 12)*
- **11**.0.9228.18 &nbsp; *(wersja V11)*


W wersji 12 bazy danych może być obsługiwany tylko na serwerze logicznych w wersji 12. I serwer w wersji 12 może udostępniać tylko w wersji 12 baz danych.


Jeśli wersji 12 nie są jeszcze uruchomionych, można uaktualnić serwera logicznych, wykonując czynności opisane w [Uaktualnianie do wersji 12 bazy danych SQL w miejscu](sql-database-v12-plan-prepare-upgrade.md).


## <a name="V12AzureSqlDbPreviewGaTable"></a>Ogólne regionów dostępności


- Przez 31 lipca 2015 r. wszystkich regionów sprzedał zostały wyróżnione do (GA, General Availability).
- W wersji 12 został wydany w grudnia 2014, ale tylko w stan Podgląd.

[Dodatkowe warunki użytkowania podglądów platformy Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
