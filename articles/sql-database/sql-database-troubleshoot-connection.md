<properties
    pageTitle="Bazy danych na serwerze nie jest obecnie dostępny, łączenie z bazą danych SQL | Microsoft Azure"
    description="Rozwiązywanie problemów z bazy danych na serwer nie jest obecnie dostępny błąd podczas Aplikacja nawiązuje połączenie z bazą danych SQL."
    services="sql-database"
    documentationCenter=""
    authors="dalechen"
    manager="felixwu"
    editor=""
    keywords="bazy danych na serwerze nie jest obecnie dostępny, łączenie z bazą danych sql"/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/21/2016"
    ms.author="daleche"/>

# <a name="error-database-on-server-is-not-currently-available-when-connecting-to-sql-database"></a>Błąd "Bazy danych na serwerze nie jest obecnie niedostępna" podczas nawiązywania połączenia z bazą danych sql
[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

Aplikacja nawiązuje połączenie z bazą danych Azure SQL, pojawi się następujący komunikat o błędzie:

```
Error code 40613: "Database <x> on server <y> is not currently available. Please retry the connection later. If the problem persists, contact customer support, and provide them the session tracing ID of <z>"
```

> [AZURE.NOTE] Ten komunikat o błędzie jest zazwyczaj przejściowych (krótkotrwałe).

Ten błąd występuje, gdy jest Azure bazy danych przeniesiony (lub skonfigurować) i aplikacja utraty połączenia z bazą danych SQL. Zdarzeń zmiany konfiguracji bazy danych SQL występuje ze względu na planowane zdarzenie (na przykład uaktualnienie oprogramowania) lub zdarzenie niezaplanowane (na przykład awaria procesu lub równoważenia obciążenia). Większość zdarzeń zmiany konfiguracji są zwykle krótkotrwałe i powinny być wypełnione mniejszy niż 60 sekund co najwyżej. Jednak tych zdarzeń od czasu do czasu może trwać dłużej zakończyć, na przykład gdy dużych transakcji powoduje odzyskiwania długim.

## <a name="steps-to-resolve-transient-connectivity-issues"></a>Kroki, aby rozwiązać problemy z łącznością przejściowych
1.  Sprawdź [Pulpit nawigacyjny Microsoft Azure usługi](https://azure.microsoft.com/status) dla dowolnego znane dostawie, które wystąpiły w czasie, w którym błędy zanotowano przez aplikację.
2. Aplikacje łączące do usługi w chmurze, takich jak bazy danych SQL Azure należy się spodziewać zdarzeń okresowych ponowna konfiguracja i wdrożenie ponawiania do obsługi tych błędów, zamiast wyświetlanie tych błędów aplikacji do użytkowników. Przeglądanie sekcji [przejściowych błędy](sql-database-connectivity-issues.md) i najważniejsze wskazówki dotyczące projektu, że wytyczne w [Omówienie tworzenia bazy danych SQL](sql-database-develop-overview.md) , aby uzyskać więcej informacji i ogólne ponów próbę strategii. Następnie zobacz przykłady kodu w [Biblioteki połączeń dla bazy danych SQL i programu SQL Server](sql-database-libraries.md) dla szczegóły.
3.  Jak bazy danych osiąga limit zasobów, może wydawać się przejściowych łącznością. Zobacz [Rozwiązywanie problemów z wydajnością](sql-database-troubleshoot-performance.md).
4.  Jeśli problemy z łącznością nadal, lub jeśli czas trwania, dla której aplikacja wystąpi błąd jest większa niż 60 sekund lub jeśli zostanie wyświetlony wielu wystąpień błędu w danym dniu, pliku żądanie obsługi Azure, wybierając pozycję **Uzyskiwanie pomocy technicznej** w witrynie [Pomocy technicznej Azure](https://azure.microsoft.com/support/options) .

## <a name="next-steps"></a>Następne kroki
- Jeśli komunikat o błędzie różnych, ocenić [komunikat o błędzie](sql-database-develop-error-messages.md) dla wskazówek dotyczących przyczyny.
- Jeśli problem został trwałych, odwiedź wskazówek w [Rozwiązywanie typowych problemów połączenia z bazą danych SQL Azure](sql-database-troubleshoot-common-connection-issues.md).
