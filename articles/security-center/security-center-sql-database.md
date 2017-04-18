<properties
   pageTitle="Usługa Azure Centrum zabezpieczeń i bazy danych SQL Azure | Microsoft Azure"
   description="W tym artykule pokazano, jak Centrum zabezpieczeń może pomóc bezpiecznego baz danych w bazie danych SQL Azure."
   services="sql-database"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/18/2016"
   ms.author="terrylan"/>

# <a name="azure-security-center-and-azure-sql-database-service"></a>Usługa Azure Centrum zabezpieczeń i bazy danych SQL Azure

[Centrum zabezpieczeń Azure](https://azure.microsoft.com/documentation/services/security-center/) ułatwia zapobieganie, wykrywanie i odpowiadanie na zagrożenia. Zapewnia zabezpieczenia zintegrowane monitorowania i zasad zarządzania w subskrypcjach usługi Azure, pomaga wykrywać zagrożenia, które w przeciwnym razie przejdź niezauważalnie i współpraca z Szeroki ekosystem rozwiązania zabezpieczeń.

W tym artykule pokazano, jak Centrum zabezpieczeń może pomóc bezpiecznego baz danych w bazie danych SQL Azure.

## <a name="why-use-security-center"></a>Dlaczego warto używać Centrum zabezpieczeń?

Centrum zabezpieczeń ułatwia ochronę danych w bazie danych SQL przez wgląd zabezpieczeń wszystkich serwerów i baz danych. W Centrum zabezpieczeń możesz wykonać następujące czynności:

- Definiowanie zasad dla szyfrowanie bazy danych SQL i inspekcja.
- Monitorowanie zabezpieczeń zasobów bazy danych SQL przez wszystkich subskrypcji.
- Szybkie identyfikowanie i korygowanie problemach z zabezpieczeniami.
- Integracja alertów z [bazy danych SQL Azure zagrożenie wykrywania](../sql-database/sql-database-threat-detection-get-started.md).

Oprócz ochrona zasobów bazy danych SQL, Centrum zabezpieczeń zawiera także zabezpieczeń monitorowania i zarządzania Azure maszyn wirtualnych, usług w chmurze, aplikacji usług i wirtualnych sieci. Dowiedz się więcej o Centrum zabezpieczeń [w tym miejscu](security-center-intro.md).

## <a name="prerequisites"></a>Wymagania wstępne

Aby rozpocząć pracę z Centrum zabezpieczeń, musi mieć subskrypcję usługi Microsoft Azure. Bezpłatne warstwa Centrum zabezpieczeń jest włączone przy użyciu swojej subskrypcji. Aby uzyskać więcej informacji na standardowy warstwy i wolnym Centrum zabezpieczeń zobacz [Ceny Centrum zabezpieczeń](https://azure.microsoft.com/pricing/details/security-center/).

Centrum zabezpieczeń obsługuje oparta na rolach programu access. Aby dowiedzieć się więcej na temat kontrola dostępu oparta na rolach (RBAC) platformy Azure, zobacz [Kontrola dostępu oparta na rolach katalogowej Active Azure](../active-directory/role-based-access-control-configure.md). Często zadawane pytania Centrum zabezpieczeń zawiera informacje dotyczące [obsługi uprawnienia w Centrum zabezpieczeń](security-center-faq.md#how-are-permissions-handled-in-azure-security-center).

## <a name="access-security-center"></a>Centrum zabezpieczeń programu Access

Masz dostęp do Centrum zabezpieczeń z [Azure portal](https://azure.microsoft.com/features/azure-portal/). [Zaloguj się do portalu](https://portal.azure.com/) i wybierz **opcję Centrum zabezpieczeń**.

![Opcja Centrum zabezpieczeń][1]

Zostanie wyświetlona karta **Centrum zabezpieczeń** .
![Karta Centrum zabezpieczeń][2]

## <a name="set-security-policy"></a>Ustawianie zasad zabezpieczeń

Zasady zabezpieczeń określa zestawu kontrolek, które są zalecane dla zasobów w określonej subskrypcji lub grupa zasobów. W Centrum zabezpieczeń do definiowania zasad dla Twojej subskrypcji lub grup zasobów według potrzeb zabezpieczeń firmy i typ aplikacji lub charakter danych w każdej subskrypcji.

Można ustawić zasady, aby wyświetlić zalecenia dotyczące inspekcji SQL i szyfrowania przezroczysty danych SQL (TDE).

- Po włączeniu **inspekcji SQL i wykrywania zagrożenie**Centrum zabezpieczeń zaleca dostępu do bazy danych Azure włączenia inspekcji w celu zapewnienia zgodności zaawansowane wykrywanie i badanie celów.
- Po włączeniu **szyfrowania przezroczysty danych SQL**, Centrum zabezpieczeń zaleca tego szyfrowanie w pozostałych można włączyć dla bazy danych SQL Azure, skojarzone wykonywania kopii zapasowych i plików dziennika transakcji.

Aby ustawić zasady zabezpieczeń, wybierz Kafelek **zasad** na karta Centrum zabezpieczeń. Na karta **Zasady zabezpieczeń** Wybierz subskrypcję, na którym chcesz włączyć zasady zabezpieczeń. Wybieranie **zasad zapobiegania** i **Włącz zalecenia dotyczące zabezpieczeń, które ma być używany dla tej subskrypcji** .
![Zasady zabezpieczeń][3]

Aby uzyskać więcej informacji, zobacz [Ustawianie zasad zabezpieczeń](security-center-policies.md).

## <a name="manage-security-recommendation"></a>Zarządzanie rekomendacji zabezpieczeń

Centrum zabezpieczeń okresowo analizuje stan zabezpieczeń Azure zasobów. Gdy Centrum zabezpieczeń identyfikuje słabych zabezpieczeń, tworzy zalecenia. Zalecenia pomagają Konfigurowanie potrzebnych formantów.

Po ustawieniu zasad zabezpieczeń, Centrum zabezpieczeń analizuje stan zabezpieczeń zasobów do identyfikowania słabych. Zalecenia są wyświetlane w formacie tabeli, gdzie każdy wiersz reprezentuje jeden zaleceń. Skorzystaj z poniższej tabeli jako odwołanie, aby ułatwić zrozumienie dostępne zalecenia dotyczące bazy danych SQL Azure i każdego rekomendacji działanie w przypadku zastosowania. Wybieranie zalecenie umożliwia przejście do artykułu, w którym wyjaśniono, jak wdrażać zalecenie w Centrum zabezpieczeń.

| Zalecenia | Opis |
| ----- | ----- |
| [Włącz wykrywanie Inspekcja i zagrożenia na serwerach SQL](security-center-enable-auditing-on-sql-servers.md) | Zaleca się włączenie inspekcji i zagrożenie wykrywania dla serwerów bazy danych SQL. (Tylko usługa bazy danych SQL. Nie zawiera programu Microsoft SQL Server uruchomionym w środowisku maszyn wirtualnych systemu.) |
| [Włącz wykrywanie Inspekcja i zagrożenia w bazach danych SQL](security-center-enable-auditing-on-sql-databases.md) | Zaleca się włączenie inspekcji i zagrożenie wykrywania dla baz danych bazy danych SQL. (Tylko usługa bazy danych SQL. Nie zawiera programu Microsoft SQL Server uruchomionym w środowisku maszyn wirtualnych systemu.) |
| [Włączanie szyfrowania danych przezroczystości](security-center-enable-transparent-data-encryption.md) | Zaleca się włączenie szyfrowanie bazy danych SQL. (Tylko usługa bazy danych SQL). |

Aby uzyskać zalecenia dotyczące Azure zasobów, wybierz Kafelek **zalecenia** na karta Centrum zabezpieczeń. Na karta **zalecenia** wybierz rekomendacji, aby wyświetlić szczegółowe informacje. W tym przykładzie po zaznaczeniu **Włączanie inspekcji i wykrywania zagrożenia na serwerach SQL**.

![Zalecenia][4]

Jak pokazano poniżej, Centrum zabezpieczeń pokazuje serwerów SQL, gdzie wykrywania inspekcji i zagrożenie nie są włączone. Po włączeniu inspekcji, można skonfigurować ustawienia wykrywania zagrożenia i ustawienia poczty e-mail, aby otrzymywać alerty zabezpieczeń. Wykrywanie zagrożenie powiadamia, gdy wykrywa działania anomalous bazy danych, które wskazują potencjalnych zagrożeniach do bazy danych. Alerty są wyświetlane na pulpicie nawigacyjnym Centrum zabezpieczeń.
![Wykrywanie inspekcji i zagrożenia][5]

Postępuj zgodnie z instrukcjami [wprowadzenie wykrywania zagrożenie bazy danych SQL](../sql-database/sql-database-threat-detection-get-started.md) , aby włączyć i skonfigurować wykrywania zagrożenia i skonfigurować listę wiadomości e-mail, które będą otrzymywać alerty zabezpieczeń w przypadku wykrycia anomalous działań.

Aby dowiedzieć się więcej na temat zalecenia, zobacz [Zarządzanie zalecenia dotyczące zabezpieczeń](security-center-recommendations.md).

## <a name="monitor-security-health"></a>Monitorowanie kondycji zabezpieczeń

Po włączeniu [zasad zabezpieczeń](security-center-policies.md) dla zasobów subskrypcji, Centrum zabezpieczeń będzie analizowanie zabezpieczeń zasobów do identyfikowania słabych.  Możesz wyświetlać stan zabezpieczeń zasobów na kafelku **prawidłowość zabezpieczeń zasobów** . Po kliknięciu **dane** na kafelku **prawidłowość zabezpieczeń zasobów** zostanie otwarta karta **Zasoby danych** z SQL zalecenia dotyczące problemów, takie jak inspekcji i przejrzysty szyfrowania danych nie włączono. Ponadto wprowadzono zalecenia dotyczące stanu kondycji ogólne bazy danych.
![Prawidłowość zabezpieczeń zasobów][6]

Aby uzyskać więcej informacji, zobacz [Monitorowanie kondycji zabezpieczeń](security-center-monitoring.md).

## <a name="manage-and-respond-to-security-alerts"></a>Zarządzanie i odpowiadanie na alerty zabezpieczeń

Centrum zabezpieczeń automatycznie gromadzi, analizuje i integruje danych dziennika z [Wykrywania zagrożenie SQL Azure](../sql-database/sql-database-threat-detection-get-started.md), a także inne zasoby Azure do wykrywania zagrożeń rzeczywistą i zmniejszyć wyniki fałszywie dodatnie. Lista alertów zabezpieczeń priorytetem jest wyświetlany w Centrum zabezpieczeń wraz z informacjami o, które trzeba szybko badanie problemu i zalecenia dotyczące korygowania atakiem.

Aby wyświetlić alerty, wybierz Kafelek **alertów zabezpieczeń** na karta Centrum zabezpieczeń. Na karta **alertów zabezpieczeń** wybierz alert, aby dowiedzieć się więcej o zdarzenia, które uruchomienie alertu i co, jeśli jakieś czynności należy wykonać, aby korygowania atakiem. W tym przykładzie po zaznaczeniu **uruchomienie potencjalne SQL**.
![Alerty zabezpieczeń][7]

Jak pokazano poniżej, Centrum zabezpieczeń zawiera dodatkowe informacje, które oferują wgląd co wyzwalane alert zasobu docelowego, stosownych źródłowy adres IP i zalecenia dotyczące sposobu korygowania.
![Potencjalne uruchomienie SQL][8]

Aby uzyskać więcej informacji, zobacz [Zarządzanie i odpowiadanie na alerty zabezpieczeń](security-center-managing-and-responding-alerts.md).

## <a name="next-steps"></a>Następne kroki

- [Centrum zabezpieczeń — często zadawane pytania](security-center-faq.md) — znajdowanie często zadawane pytania dotyczące korzystania z usługi.
- [Przewodnik po planowaniu i operacji Centrum zabezpieczeń](security-center-planning-and-operations-guide.md) - Obserwuj zbiór kroki i zadania, aby zoptymalizować korzystanie z Centrum zabezpieczeń na podstawie organizacji wymagania dotyczące zabezpieczeń i model zarządzania chmury.
- [Dane Centrum zabezpieczeń](security-center-data-security.md) — Dowiedz się, jak Centrum zabezpieczeń zbiera i przetwarza dane na temat usługi Azure zasobów, w tym informacje o konfiguracji, metadane, dzienniki zdarzeń i pliki zrzutu awaryjnego.
- [Obsługa zdarzeń](security-center-incident.md) — Dowiedz się, jak za pomocą możliwości alertów zabezpieczeń w Centrum zabezpieczeń, aby pomóc w obsłudze zdarzeń.

<!--Image references-->
[1]: ./media/security-center-sql-database/security-center.png
[2]: ./media/security-center-sql-database/security-center-blade.png
[3]: ./media/security-center-sql-database/security-policy.png
[4]: ./media/security-center-sql-database/recommendation.png
[5]: ./media/security-center-sql-database/turn-on-auditing.png
[6]: ./media/security-center-sql-database/monitor-health.png
[7]: ./media/security-center-sql-database/alert.png
[8]: ./media/security-center-sql-database/sql-injection.png
