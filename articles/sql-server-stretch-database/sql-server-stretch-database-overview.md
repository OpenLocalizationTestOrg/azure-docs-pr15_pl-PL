<properties
    pageTitle="Rozciąganie omówienie bazy danych | Microsoft Azure"
    description="Dowiedz się, jak bazy danych rozciąganie migruje zimnej danych przezroczyste i bezpiecznie w chmurze Microsoft Azure."
    services="sql-server-stretch-database"
    documentationCenter=""
    authors="douglaslMS"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-server-stretch-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="06/27/2016"
    ms.author="douglasl"/>

# <a name="stretch-database-overview"></a>Rozciąganie omówienie bazy danych

Rozciąganie bazy danych migruje zimnej danych przezroczyste i bezpiecznie w chmurze Microsoft Azure.

Jeśli chcesz rozpocząć pracę z bazy danych Rozciąganie od razu, zobacz [Wprowadzenie, uruchamiając Włączanie bazy danych, rozciąganie kreatora](sql-server-stretch-database-wizard.md).

## <a name="what-are-the-benefits-of-stretch-database"></a>Jakie są zalety rozciąganie bazy danych?
Rozciąganie bazy danych ma następujące zalety:

### <a name="provides-cost-effective-availability-for-cold-data"></a>Zawiera koszt\-skutecznych dostępność zimnej danych
Rozciąganie ciepłej i zimnej transakcji dane dynamiczne z programu SQL Server do Microsoft Azure z bazą danych programu SQL Server rozciąganie. W przeciwieństwie do przechowywania typowych zimnej danych dane są zawsze online i jest dostępny dla kwerendy. Można udostępnić dłużej osi czasu przechowywania danych bez przerywania banku dużych tabel, takich jak Historia zamówień klientów. Korzyści z niski koszt Azure zamiast skalowania drogich na\-pomieszczenia miejsca do magazynowania. Wybieranie poziomu cen i konfigurowanie ustawień w Portal Azure, aby zachować kontrolę nad kosztów. Skalowanie w górę lub w dół, stosownie do potrzeb. Odwiedź stronę [Ceny bazy danych rozciąganie programu SQL Server](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/) , aby uzyskać szczegółowe informacje.

### <a name="doesnt-require-changes-to-queries-or-applications"></a>Nie wymaga zmiany kwerendy lub aplikacji
Dostęp do danych programu SQL Server bezproblemowo niezależnie od tego, czy jest\-pomieszczenia lub rozciągnięciu w chmurze.  Możesz ustawić zasady, która określa miejsce, w którym dane są przechowywane i SQL Server obsługuje przenoszenia danych w tle. Cała tabela jest zawsze online i opcję uwzględniana w kwerendach. I rozciąganie bazy danych nie wymaga żadnych zmian w istniejących zapytań lub aplikacji — danych znajduje się zupełnie przezroczysty do aplikacji.

### <a name="streamlines-on-premises-data-maintenance"></a>Usprawnia na\-pomieszczenia Obsługa danych
Zmniejszanie na\-pomieszczenia konserwacji i miejsca do magazynowania dla określonego typu danych. Kopie zapasowe dla swojego na\-lokalnej danych działa szybciej i Zakończ w oknie konserwacji. Kopie zapasowe chmury części danych są uruchamiane automatycznie. Usługi na\-znacznie ograniczono wymagań przechowywania lokalnej. Azure magazynem może być 80% drogich mniej niż dodanie za\-pomieszczenia SSD.

### <a name="keeps-your-data-secure-even-during-migration"></a>Chroni dane nawet podczas migracji
Ciesz wyróżniane jako rozciąganie najważniejszych aplikacji bezpieczne do chmury. Program SQL Server zawsze szyfrowane zapewnia szyfrowanie danych w ruchu. Wiersz poziom zabezpieczeń kontrola dostępu i inne zaawansowane funkcje zabezpieczeń programu SQL Server również działają z rozciąganie bazy danych w celu ochrony danych.

## <a name="what-does-stretch-database-do"></a>Do czego służy rozciąganie bazy danych?
Po włączeniu rozciąganie bazy danych w przypadku wystąpienia programu SQL Server, bazy danych i co najmniej jedną tabelę, przeprowadzić migrację danych zimnej Azure rozpoczyna się trybie cichym rozciąganie bazy danych.

-   Jeśli zimnej dane są przechowywane w osobnej tabeli, możesz przeprowadzić migrację całej tabeli.

-   Jeśli tabela zawiera zarówno ciepłej i zimnej dane, możesz określić funkcję Filtr zaznacz wiersze, aby przeprowadzić migrację.

**Nie musisz zmienić istniejące kwerendy i aplikacje klienta.** Nadal masz Bezproblemowa dostępu do danych lokalnych i zdalnych, nawet podczas migracji danych. Istnieje niewielka ilość opóźnienie dla zdalnych kwerend, ale tylko wystąpienia tego opóźnienie kwerendy zimnej danych.

**Rozciąganie bazy danych gwarantuje, że dane nie zostaną utracone** , jeśli wystąpi błąd podczas migracji. Ponadto wprowadzono ponów próbę logicznych powinien obsługiwać połączenia problemy, które mogą wystąpić podczas migracji. Widok dynamiczne zarządzanie udostępnia stan migracji.

Rozwiązywanie problemów z lokalnego serwera lub maksymalizowanie dostępna przepustowość sieci **można wstrzymać migracji danych** .

![Omówienie rozciąganie bazy danych][StretchOverviewImage1]

## <a name="is-stretch-database-for-you"></a>Rozciąganie bazy danych jest dla Ciebie?
Jeśli można wykonać następujące instrukcje, rozciąganie bazy danych może pomóc Twojej wymagania i rozwiązywanie problemów.

|Jeśli jesteś maker decyzja|Jeśli administrator|
|------------------------------|-------------------|
|Mam zachowanie danych transakcji przez dłuższy czas.|Rozmiar Moje tabele jest efektywnego z kontrolki.|
|Czasami muszę kwerendy zimnej danych.|Na przykład użytkownicy mają dostęp do danych zimnej, że używają rzadko.|
|Mam aplikacje, w tym starsze aplikacje, których nie chcesz, aby zaktualizować.|Muszę zachować Kupując i dodawanie dodatkowego miejsca do magazynowania.|
|Chcę, aby znaleźć sposób zaoszczędzić ilość miejsca do magazynowania.|Nie kopia zapasowa i przywracanie takie dużych tabel w umowie SLA.|

## <a name="what-kind-of-databases-and-tables-are-candidates-for-stretch-database"></a>Jakiego rodzaju bazy danych i tabele są kandydatami rozciąganie bazy danych?
Rozciąganie bazy danych elementów docelowych transakcji bazy danych z dużą ilością zimnej danych, zwykle przechowywanych w małej liczby tabel. W poniższych tabelach może zawierać więcej niż miliardów wierszy.

Jeśli korzystasz z funkcji tymczasowa tabela programu SQL Server 2016, użyj rozciąganie bazy danych, aby przeprowadzić migrację całości lub części tabeli historii skojarzone kosztów\-skutecznych miejsca do magazynowania w Azure. Aby uzyskać więcej informacji zobacz [Zarządzanie przechowywania z historycznych danych w tabelach czasowy wersji systemu](https://msdn.microsoft.com/library/mt637341.aspx).

Użyj rozciąganie Advisor bazy danych, funkcja SQL Server 2016 poradami dotyczącymi uaktualnienia, aby zidentyfikować bazy danych i tabele rozciąganie bazy danych. Aby uzyskać więcej informacji zobacz [Identyfikowanie baz danych i tabel dla bazy danych rozciąganie](sql-server-stretch-database-identify-databases.md). Aby dowiedzieć się więcej o potencjalnych problemach blokowania, zobacz [ograniczenia dotyczące rozciąganie bazy danych](sql-server-stretch-database-limitations.md).

## <a name="test-drive-stretch-database"></a>Przetestuj rozciąganie bazy danych
**Przetestuj rozciąganie bazy danych za pomocą przykładowe bazy danych AdventureWorks.** Aby uzyskać przykładowe bazy danych AdventureWorks, Pobierz co najmniej pliku bazy danych i plików próbki i skryptów [tutaj](https://www.microsoft.com/download/details.aspx?id=49502). Po przywróceniu przykładowej bazy danych do wystąpienia programu SQL Server 2016 Rozpakuj plik próbki i Otwórz plik rozciąganie DB próbki z folderu DB rozciąganie. Uruchomić skrypty w tym pliku, aby sprawdzić ilość miejsca zajmowaną przez danych przed i po włączeniu rozciąganie bazy danych, aby śledzić postęp migracji danych i upewnij się, że możesz nadal kwerendy istniejących danych, a następnie Wstaw nowe dane podczas oraz po migracji danych.

## <a name="next-step"></a>Następny krok
**Identyfikowanie bazy danych i tabele, które są kandydatami rozciąganie bazy danych.** Pobierz program SQL Server 2016 Upgrade Advisor i uruchomić rozciąganie Advisor bazy danych do identyfikowania bazy danych i tabele, które są kandydatami rozciąganie bazy danych. Rozciąganie Advisor bazy danych identyfikuje również problemów z blokowaniem. Aby uzyskać więcej informacji zobacz [Identyfikowanie baz danych i tabel dla bazy danych rozciąganie](sql-server-stretch-database-identify-databases.md).

<!--Image references-->
[StretchOverviewImage1]: ./media/sql-server-stretch-database-overview/StretchDBOverview.png
[StretchOverviewImage2]: ./media/sql-server-stretch-database-overview/StretchDBOverview1.png
[StretchOverviewImage3]: ./media/sql-server-stretch-database-overview/StretchDBOverview2.png
