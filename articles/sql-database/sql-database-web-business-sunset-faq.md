<properties
   pageTitle="Azure SQL bazy danych w sieci Web i Business Edition zachód słońca — często zadawane pytania dotyczące | Microsoft Azure"
   description="Dowiedz się, po bazy danych sieci Web SQL Azure i małych firm zostanie wycofana i informacje na temat funkcji i funkcji nowe warstwy usługi."
   services="sql-database"
   documentationCenter="na"
   authors="stevestein"
   manager="jhubbard"
   editor="monicar" />
<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="08/08/2016"
   ms.author="sstein" />

# <a name="web-and-business-edition-sunset-faq"></a>Witryny sieci Web i Business Edition zachód słońca — często zadawane pytania

Azure baz danych sieci Web SQL i małych firm są teraz wycofana. Poziomy Basic, standardowe Premium i elastyczne Zamień Ustępujący baz danych sieci Web i małych firm.

Aby pomóc z uaktualnianie baz danych sieci Web i firm, odpowiednią usługę warstwy i wydajności poziomu (poziomu cen) dla każdej bazy danych na podstawie jego historycznych obciążenia zalecane usług bazy danych SQL.

**Aby uzyskać ceny zalecenia poziomu:**

- [Uaktualnianie do wersji 12 bazy danych SQL za pomocą portalu Azure](sql-database-upgrade-server-portal.md)
- [Uaktualnianie do wersji 12 bazy danych SQL przy użyciu programu PowerShell](sql-database-upgrade-server-powershell.md)
- [Zmienianie warstwie cennik bazy danych sieci Web lub firm](sql-database-service-tier-advisor.md)



## <a name="why-does-the-azure-portal-show-my-web-and-business-edition-databases-as-retired"></a>Dlaczego Azure portal wyświetlić mojej witryny sieci Web i Business edition baz danych wycofana?

Ponieważ bazy danych sieci Web i Business edition nie będą dostępne po września 2015 r., portalu etykiety baz danych sieci Web i małych firm podczas wycofana. Etykieta wycofanych także przypomnienie, że żadnej bazy danych sieci Web i małych firm powinny być uaktualnione do standardowego, Basic lub Premium. Aby uzyskać szczegółowe informacje na temat uaktualniania istniejącej bazy danych sieci Web lub firm do nowej warstwy usługi zobacz [Uaktualnianie do wersji 12 bazy danych SQL Azure](sql-database-upgrade-server-portal.md).

## <a name="which-new-service-tier-is-the-best-choice-to-upgrade-my-existing-web-or-business-database-to"></a>Które nowe warstwa usług jest to najlepszy wybór uaktualnienia mojej istniejącej sieci Web lub firm bazy danych?

Wybranie odpowiedniej nowej usługi warstwa i wydajności poziomu dla istniejącej bazy danych sieci Web lub firm zależy od określonych funkcji i wydajności wymagania aplikacji.

Aby pomóc w wyborze odpowiednich nowych warstwa usług, zobacz [Uaktualnianie do wersji 12 bazy danych SQL Azure](sql-database-upgrade-server-portal.md)za pomocą cennik zalecenia warstwa opisanych powyżej lub Aby uzyskać szczegółowe informacje.

## <a name="why-is-microsoft-introducing-new-service-tiers"></a>Dlaczego jest Microsoft wprowadzenie nowego warstwy usług

Na podstawie opinii klientów, bazy danych SQL Azure wprowadza nowe warstwy usługi ułatwiające klientom więcej łatwo obsługuje obciążenia relacyjnej bazy danych. Nowe poziomy są przeznaczone do przeprowadzania przewidywalne wydajności przez szeroką gamę siedem poziomy uproszczonej na potrzeby aplikacji transakcji ciężki. Ponadto nowe poziomy oferują wiele funkcji ciągłości firm, silniejsze SLA czas pracy, większych rozmiarów bazy danych dla mniej pieniędzy i ulepszone środowisko rozliczeń.

## <a name="where-can-i-learn-more-about-the-new-service-tiers"></a>Gdzie mogę uzyskać więcej informacji na temat nowej warstwy usługi?

Aby uzyskać szczegółowe informacje dotyczące nowego warstwy usługi i model wydajności Zobacz [warstwy usługi](sql-database-service-tiers.md). Aby uzyskać szczegółowe informacje o nowych warstwy usługi cenach, zobacz [ceny bazy danych SQL](https://azure.microsoft.com/pricing/details/sql-database/).

## <a name="what-features-or-functionality-will-not-be-available-in-basic-standard-and-premium"></a>Jakie funkcje lub funkcji nie będą dostępne w Basic, Standard i Premium?

Funkcja federacje będzie można usunąć z wersji sieci Web i małych firm. Klienci, którzy muszą swoich baz danych w nowym oknie Skala Zachęcamy, aby zamiast tego użyj lub Migrowanie do [Narzędzia elastyczne bazy danych](sql-database-elastic-scale-get-started.md) dla [Bazy danych SQL Azure](sql-database-elastic-scale-get-started.md), który ułatwia tworzenie i zarządzanie nim aplikacji korzystającej z sharding. Biblioteka klienta .NET pozwala aplikacjom na zdefiniowanie, jak dane są mapowane na żądania OLTP odłamki i trasy do odpowiednich baz danych. Aby obsługiwać operacji zarządzania, które skonfigurowanie rozkład danych między odłamki, szablonu usługi Azure chmury znajduje się hostować w Azure subskrypcji. Oprócz [Narzędzia elastyczne bazy danych](sql-database-elastic-scale-get-started.md)firma Microsoft będzie można tworzyć i publikować [wskazówki sharding niestandardowe desenie i wskazówki dotyczące](https://msdn.microsoft.com/library/azure/dn764977.aspx) oparte na learnings z pozyskiwaniu głębokości klienta. Nowych klientów, którzy muszą skali się funkcji należy zapoznaj się z [Narzędzia elastyczne bazy danych](sql-database-elastic-scale-get-started.md) i/lub skontaktuj się z programu Microsoft Support do oceny ich opcje.

Microsoft zmienia się również interfejsu kopii bazy danych z bazami danych Premium. Wcześniej premium bazy danych przydziału jest ograniczona tworzenie bazy danych... JAKO KOPIĘ w T-SQL utworzony zawieszone Premium bazy danych bez zarezerwowane zasoby, który został poniesiony na tym samym poziomie jak bazy danych dodatku Business. Jak przydział premium jest obecnie więcej bezpłatnie, zawieszone Premium nie jest już obsługiwana. Kopii bazy danych zostanie teraz utworzony przy użyciu samej wersji i poziom wydajności jako źródła i będą naliczane odpowiednio. Klienci mogą wybrać na starszą wersję skopiowany bazy danych do innej usługi poziom wydajności poziomu lub w celu zmniejszenia kosztów, w razie potrzeby. Istniejące zawieszone Premium bazy danych jest konwertowana na Business edition jako część tej wersji. Przewiduje się, że wprowadzenie [w chwili przywrócić](sql-database-recovery-using-backups.md#point-in-time-restore) zmniejszy potrzeba wykonania kopii bazy danych.

## <a name="how-does-basic-standard-and-premium-improve-my-billing-experience"></a>Jak Basic, Standard i Premium zwiększyć obsługi rozliczeń?

Podstawowe, standardowy, bazy danych programu SQL Azure Premium są wystawiona przez godzinę, a użytkownik ma możliwość skalowania każdej bazy danych w górę lub w dół. Konta na podstawie najwyższego poziomu i wydajności poziomu wybrany dla każdej godziny stałej stopy. Ponadto poziom wydajności (przykład: podstawowej, S1 i P2) są podzielone na rachunku, aby ułatwić wyświetlenie liczby bazy danych dni i godzin poniesione w jeden miesiąc dla każdego poziomu wydajności. Bazy danych sieci Web i małych firm nadal naliczane za pomocą jednostek bazy danych na podstawie rozmiaru bazy danych. Odwiedź [bazy danych SQL ceny strony](https://azure.microsoft.com/pricing/details/sql-database/) dowiedzieć się więcej o ceny i różnice między nowe warstwy usługi.


## <a name="see-also"></a>Zobacz też

[Baza danych SQL Azure](https://azure.microsoft.com/documentation/services/sql-database/)

[Sieci Web i ceny firm](https://azure.microsoft.com/pricing/details/sql-database/web-business/)

[Warstwy usługi](sql-database-service-tiers.md)

[Uaktualnianie do wersji 12 bazy danych Azure SQL](sql-database-upgrade-server-portal.md)
