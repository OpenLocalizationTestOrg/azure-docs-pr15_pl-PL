<properties
    pageTitle="Rozwiązywanie typowych problemów połączenia z bazą danych SQL Azure"
    description="Procedura identyfikowanie i rozwiązywanie typowych błędów połączenia bazy danych SQL Azure."
    services="sql-database"
    documentationCenter=""
    authors="dalechen"
    manager="felixwu"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/31/2016"
    ms.author="daleche"/>

# <a name="troubleshoot-connection-issues-to-azure-sql-database"></a>Rozwiązywania problemów z połączeniem z bazą danych SQL Azure

Podczas połączenia z bazą danych SQL Azure kończy się niepowodzeniem, jest wyświetlany [komunikat o błędzie](sql-database-develop-error-messages.md). Ten artykuł dotyczy scentralizowane tematu, który ułatwia rozwiązywanie problemów z łącznością z bazy danych SQL Azure. Go wprowadza [wspólnej powoduje](#cause) problemy z połączeniem, zaleca się [Narzędzie do rozwiązywania problemów](#try-the-troubleshooter-for-azure-sql-database-connectivity-issues) , które ułatwia tożsamości problem i instrukcje rozwiązywania problemów rozwiązać [przejściowych błędy](#troubleshoot-transient-errors) i [błędy trwałych lub w innych niż zmiennych](#troubleshoot-the-persistent-errors). Na koniec zawiera listę [wszystkich odpowiednich artykułów problemów z łącznością z bazy danych SQL Azure](#all-topics-for-azure-sql-database-connection-problems).

Jeśli występują problemy z połączeniem, spróbuj Rozwiązywanie problemów z opisanych w tym artykule.
[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="cause"></a>Przyczyna

Problemy z połączeniem może być spowodowane jedną z następujących czynności:

- Nie można zastosować najważniejsze wskazówki i wytyczne dotyczące projektowania w procesie projektowania aplikacji.  Zobacz [Omówienie tworzenia bazy danych SQL](sql-database-develop-overview.md) , aby rozpocząć pracę.
- Azure SQL Database zmiany konfiguracji
- Ustawienia zapory
- Limit czasu połączenia
- Informacje o logowaniu niepoprawne
- Osiągnięto limit maksymalny na zasoby bazy danych SQL Azure

Ogólnie rzecz biorąc problemy z połączeniem z bazą danych SQL Azure można podzielić w następujący sposób:

- [Błędy przejściowych (krótkotrwałe lub przerw)](#troubleshoot-transient-errors)
- [Trwałe lub w innych niż zmiennych błędy (błędy, które powtarzają się regularnie)](#troubleshoot-the-persistent-errors)

## <a name="try-the-troubleshooter-for-azure-sql-database-connectivity-issues"></a>Wypróbuj narzędzie do rozwiązywania problemów dla problemów z łącznością z bazy danych SQL Azure

Jeśli wystąpi błąd określonego połączenia, wypróbuj [to narzędzie](https://support.microsoft.com/help/10085/troubleshooting-connectivity-issues-with-microsoft-azure-sql-database), które pozwalają szybko tożsamości i rozwiązania problemu.

## <a name="troubleshoot-transient-errors"></a>Rozwiązywanie problemów z błędami przejściowych
Jeśli aplikacja występują błędy przejściowych, przejrzyj następujące tematy, aby uzyskać porady dotyczące rozwiązywania i zmniejszyć częstotliwość tych błędów:

- [Rozwiązywanie problemów z bazy danych &lt;x&gt; na serwerze &lt;y&gt; jest niedostępny (błąd: 40613)](sql-database-troubleshoot-connection.md)
- [Rozwiązywanie problemów, diagnozowanie i uniknięcia błędów połączenia SQL i przejściowych błędów bazy danych SQL](sql-database-connectivity-issues.md)

<a id="troubleshoot-the-persistent-errors" name="troubleshoot-the-persistent-errors"></a>

## <a name="troubleshoot-persistent-errors-non-transient-errors"></a>Rozwiązywanie problemów z błędami trwałych (innych niż zmiennych błędy)

Jeśli aplikacja trwale nie może nawiązać połączenia z bazą danych SQL Azure, zwykle oznacza to, wystąpił problem z jedną z następujących czynności:

- Konfiguracja zapory. Zapora bazy danych lub po stronie klienta Azure SQL blokuje połączenia z bazą danych SQL Azure.
- Sieci ponowna konfiguracja po stronie klienta: na przykład nowy adres IP lub serwera proxy.
- Błąd użytkownika: na przykład błędne parametry połączenia, takie jak nazwa serwera w parametrach połączenia.

### <a name="steps-to-resolve-persistent-connectivity-issues"></a>Kroki, aby rozwiązać problemy z łącznością trwałych

1.  Konfigurowanie [reguły zapory](sql-database-configure-firewall-settings.md) , aby umożliwić adres IP klienta.
2.  Upewnij się, że port 1433 jest otwarty dla połączeń wychodzących na wszystkie zapory między klientem i Internet. Przejrzyj [Konfigurowanie Zapory systemu Windows, aby umożliwić dostępu programu SQL Server](https://msdn.microsoft.com/library/cc646023.aspx) dla dodatkowych wskazówek.
3.  Sprawdź ciąg połączenia oraz inne ustawienia połączenia. Zobacz sekcję parametry połączenia, w tym [temacie problemy łączności](sql-database-connectivity-issues.md#connections-to-azure-sql-database).
4.  Sprawdzanie kondycji usługi na pulpicie nawigacyjnym. Jeśli uważasz, że jest awaria regionalne, zobacz [Odzyskiwanie z awarii](sql-database-disaster-recovery.md) dla czynności do odzyskania nowy region.

## <a name="all-topics-for-azure-sql-database-connection-problems"></a>Wszystkie tematy do problemów z połączeniem bazy danych SQL Azure

W poniższej tabeli wymieniono każdej temat problem połączenia, który dotyczy bezpośrednio do usługi bazy danych SQL Azure.


| &nbsp; | Tytuł | Opis |
| --: | :-- | :-- |
| 1 | [Rozwiązywania problemów z połączeniem z bazą danych SQL Azure](sql-database-troubleshoot-common-connection-issues.md) | To jest strona początkowa w celu rozwiązywania problemów z łącznością w bazie danych SQL Azure. Ją opisano sposób identyfikowania i rozwiązywania błędów przejściowych i błędy trwałych lub innych niż zmiennych w bazie danych SQL Azure. |
| 2 | [Rozwiązywanie problemów, diagnozowanie i uniknięcia błędów połączenia SQL i przejściowych błędów bazy danych SQL](sql-database-connectivity-issues.md) | Dowiedz się, jak rozwiązywanie problemów, diagnozowanie i zapobiec błąd połączenia SQL lub przejściowych w bazie danych SQL Azure. |
| 3 | [Ogólne wskazówki przejściowych obsługi błędów](best-practices-retry-general.md) | Ten artykuł zawiera ogólne wskazówki obsługi błędów przejściowych podczas nawiązywania połączenia z bazą danych SQL Azure. |
| 4 | [Rozwiązywanie problemów z łącznością z platformy Microsoft Azure SQL Database](https://support.microsoft.com/help/10085/troubleshooting-connectivity-issues-with-microsoft-azure-sql-database) | To narzędzie ułatwia tożsamości błędy połączeń rozwiązywanie problemu. |
| 5 | [Rozwiązywanie problemów z "bazy danych &lt;x&gt; na serwerze &lt;y&gt; nie jest obecnie dostępny. Spróbuj ponownie później połączenia"Błąd](sql-database-troubleshoot-connection.md) | Informacje dotyczące identyfikowania i rozwiązywania 40613 o błędzie: "bazy danych &lt;x&gt; na serwerze &lt;y&gt; nie jest obecnie dostępny. Spróbuj ponownie później połączenia." |
| 6 | [Kody błędów SQL dla aplikacji klienckich bazy danych SQL: błąd połączenia i innych problemów z bazy danych](sql-database-develop-error-messages.md) | Zawiera informacje dotyczące kodów błędów SQL dla bazy danych SQL aplikacje klienckie, takie jak typowych błędów połączenia bazy danych, problemów kopii bazy danych i ogólnych błędów. |
| 7 | [Azure SQL Database wydajności wskazówki dotyczące pojedynczego baz danych](sql-database-performance-guidance.md) | Ten artykuł zawiera wskazówki ułatwiającymi, które warstwa usług po prawej stronie aplikacji. Udostępnia zalecenia dotyczące dostosowywania aplikację, aby jak najlepiej wykorzystać bazy danych SQL Azure. |
| 8 | [Omówienie tworzenia bazy danych SQL](sql-database-develop-overview.md) | Łącza do przykładów kodu dla różnych technologii, które można łączyć się i korzystać z bazy danych SQL Azure. |
| 9 | Uaktualnianie do bazy danych SQL Azure w wersji 12 strony ([Azure portal](sql-database-upgrade-server-portal.md), [programu PowerShell](sql-database-upgrade-server-powershell.md)) | Zawiera wskazówki dotyczące uaktualniania istniejącej serwery V11 bazy danych SQL Azure i baz danych do bazy danych SQL Azure w wersji 12 przy użyciu Azure portal lub programu PowerShell. |


## <a name="next-steps"></a>Następne kroki

- [Rozwiązywanie problemów z wydajnością bazy danych SQL Azure](sql-database-troubleshoot-performance.md)
- [Rozwiązywanie problemów uprawnienia bazy danych SQL Azure](sql-database-troubleshoot-permissions.md)
- [Zobacz wszystkich tematów dotyczących usługi bazy danych SQL Azure](sql-database-index-all-articles.md)
- [Wyszukiwanie dokumentację dotyczącą Microsoft Azure](http://azure.microsoft.com/search/documentation/)
- [Wyświetlanie najnowszych aktualizacji usługi bazy danych SQL Azure](http://azure.microsoft.com/updates/?service=sql-database)


## <a name="additional-resources"></a>Dodatkowe zasoby

- [Omówienie tworzenia bazy danych SQL](sql-database-develop-overview.md)
- [Ogólne wskazówki przejściowych obsługi błędów](../best-practices-retry-general.md)
- [Biblioteki połączeń dla bazy danych SQL i programu SQL Server](sql-database-libraries.md)
- [Ścieżka nauki dotyczące korzystania z bazy danych SQL Azure](https://azure.microsoft.com/documentation/learning-paths/sql-database-training-learn-sql-database)
- [Ścieżka nauki dotyczące korzystania z narzędzia i funkcje elastyczne bazy danych](https://azure.microsoft.com/documentation/learning-paths/sql-database-elastic-scale) 
