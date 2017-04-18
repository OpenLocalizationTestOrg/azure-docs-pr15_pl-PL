<properties
    pageTitle="Identyfikowanie bazy danych i tabele rozciąganie bazy danych, uruchamiając Advisor bazy danych rozciąganie | Microsoft Azure"
    description="Dowiedz się, jak identyfikowanie bazy danych i tabele, które są kandydatami rozciąganie bazy danych."
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
    ms.topic="article"
    ms.date="06/14/2016"
    ms.author="douglasl"/>

# <a name="identify-databases-and-tables-for-stretch-database-by-running-stretch-database-advisor"></a>Identyfikowanie bazy danych i tabele rozciąganie bazy danych, uruchamiając rozciąganie Advisor bazy danych

Aby zidentyfikować bazy danych i tabele, które są kandydatami rozciąganie bazy danych, Pobierz program SQL Server 2016 uaktualnienie Advisor i uruchomić rozciąganie Advisor bazy danych. Rozciąganie Advisor bazy danych identyfikuje również problemów z blokowaniem.

## <a name="download-and-install-upgrade-advisor"></a>Pobieranie i instalowanie Upgrade Advisor
Pobierz i zainstaluj Upgrade Advisor [tutaj](http://go.microsoft.com/fwlink/?LinkID=613421). To narzędzie nie znajduje się na nośniku instalacji programu SQL Server.

## <a name="run-the-stretch-database-advisor"></a>Uruchamianie Advisor rozciąganie bazy danych

1.  Uruchom Upgrade Advisor.

2.  Wybierz pozycję **scenariuszy**, a następnie wybierz **Uruchamianie rozciąganie ADVISOR bazy danych**.

3.  Karta **Uruchamianie rozciąganie Advisor bazy danych** wybierz polecenie **Wybierz baz danych do ANALIZOWANIA**.

4.  Karta **Zaznacz bazy danych** wpisz lub wybierz nazwę serwera oraz informacje uwierzytelniania. Kliknij przycisk **Połącz**.

5.  Zostanie wyświetlona lista baz danych na wybranym serwerze. Wybierz pozycję baz danych, które chcesz przeanalizować. Kliknij przycisk **Wybierz**.

6.  Karta **Uruchamianie rozciąganie Advisor bazy danych** wybierz polecenie **Uruchom**.  Analiza działa.

## <a name="review-the-results"></a>Przejrzyj wyniki

1.  Po zakończeniu analizy na karta **Analyzed baz danych** , wybierz jeden z bazy danych, które można analizować je w celu wyświetlania karta **wyniki analizy** .

    Karta **wyników analizy** lista polecane tabele w wybranej bazy danych, spełniającym kryteria polu rekomendacji domyślne.

2.  Na liście Tabele na karta **wyników analizy** wybierz jedną z polecane tabele, aby wyświetlić karta **wyników tabeli** .

    Jeśli blokują problemy, karta **wyników tabeli** wymieniono problemy uniemożliwiające dla wybranej tabeli. Aby uzyskać informacji na temat problemów z blokowaniem wykryte przez rozciąganie Advisor bazy danych zobacz [ograniczenia dotyczące rozciąganie bazy danych](sql-server-stretch-database-limitations.md).

3.  Na liście problemów z blokowaniem na karta **wyników tabeli** , wybierz jedną z problemy, aby wyświetlić więcej informacji na temat wybranego problemu i proponuje łagodzenia czynności. Wdrożenie łagodzenia sugerowane czynności, jeśli chcesz skonfigurować zaznaczonej tabeli rozciąganie bazy danych.

## <a name="next-step"></a>Następny krok
Włącz rozciąganie bazy danych.

-   Aby włączyć rozciąganie bazy danych w bazą **bazy danych**, zobacz [Włączanie rozciąganie bazy danych do bazy danych](sql-server-stretch-database-enable-database.md).

-   Aby włączyć rozciąganie bazy danych w innej **tabeli**, gdy rozciąganie jest już włączone dla bazy danych, zobacz [Włączanie rozciąganie bazy danych dla tabeli](sql-server-stretch-database-enable-table.md).

## <a name="see-also"></a>Zobacz też

[Ograniczenia dotyczące rozciąganie bazy danych](sql-server-stretch-database-limitations.md)

[Włączanie rozciąganie bazy danych w bazie danych](sql-server-stretch-database-enable-database.md)

[Włączanie rozciąganie bazy danych dla tabeli](sql-server-stretch-database-enable-table.md)

[Wszystkich tematów dotyczących usługi bazy danych rozciąganie serwera SQL Azure](sql-server-stretch-database-index-all-articles.md)
