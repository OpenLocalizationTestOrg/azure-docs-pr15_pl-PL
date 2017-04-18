<properties
    pageTitle="Co to jest baza danych SQL? Wprowadzenie do bazy danych SQL | Microsoft Azure"
    description="Wprowadzenie do bazy danych SQL: szczegóły techniczne i możliwości firmy Microsoft relacyjne bazy danych w systemie zarządzania (RDBMS) w chmurze."
    keywords="wprowadzenie do programu sql, wprowadzenie do sql, co to jest baza danych sql"
    services="sql-database"
    documentationCenter=""
    authors="shontnew"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="08/16/2016"
   ms.author="shkurhek"/>

# <a name="what-is-sql-database-introduction-to-sql-database"></a>Co to jest baza danych SQL? Wprowadzenie do bazy danych SQL

Baza danych SQL to usługa relacyjnej bazy danych w chmurze, oparty na rynku interlinii aparacie programu Microsoft SQL Server, z funkcjami krytyczne. Baza danych SQL zapewnia przewidywalne wydajność, skalowalność bez przestojów, ciągłości i ochrona danych — wszystko w administracji w pobliżu zera. Możesz skoncentrować się na opracowywania aplikacji szybkiego i przyspieszanie czasem rynku, zamiast zarządzania maszyn wirtualnych i infrastruktury. Ponieważ jest oparty na aparat [Programu SQL Server](https://msdn.microsoft.com/library/bb545450.aspx) , baza danych SQL obsługuje istniejącego programu SQL Server narzędzi, bibliotek i interfejsów API, dzięki czemu można łatwiej przenoszenie i rozciąganie w chmurze.

Ten artykuł dotyczy wprowadzenie do bazy danych SQL podstawowych koncepcji i funkcji związanych z wydajnością, skalowalność i zarządzanie łączami do eksplorowania danych. Jeśli możesz już przystąpić do przejść w, możesz [Tworzenie pierwszej bazy danych SQL](sql-database-get-started.md) lub [Tworzenie puli elastyczne bazy danych](sql-database-elastic-pool-create-portal.md) w minutach. Jeśli chcesz szczegółowego dive, obejrzyj ten klip wideo 30 minut.

> [AZURE.VIDEO azurecon-2015-get-started-with-azure-sql-database]

## <a name="adjust-performance-and-scale-without-downtime"></a>Dostosowywanie wydajności i skali bez przestojów

Bazy danych programu SQL jest dostępna w Basic, Standard i Premium *warstwy usługi*. Każdego poziomu usług oferuje [różne poziomy wydajności i możliwości](sql-database-service-tiers.md) obsługi lightweight do dużą obciążenia bazy danych. Można tworzyć pierwszej aplikacji na małych baz danych dla kilku bucks miesięcznie, następnie [Zmień warstwa usług](sql-database-scale-up.md) ręcznie lub programowo w dowolnym momencie podczas aplikacji przechodzi wirusa na całym świecie, bez przestoje aplikacji lub klientów.

Wiele firm i aplikacje można utworzyć bazy danych oraz wybieranie wydajności jednej bazie danych w górę lub w dół na żądanie wystarcza, zwłaszcza jeśli stosunkowo przewidywalne upodobania. Jednak jeśli masz nieprzewidywalny upodobania go mogą utrudnić do zarządzania kosztami i model firmy.

[Elastyczne pul](sql-database-elastic-pool.md) w bazie danych SQL rozwiązać ten problem. Koncepcja jest proste. Przydzielanie wydajności do puli, a zapłacić za ankiety wydajności pula zamiast wydajności jednej bazie danych. Nie musisz wybrać wydajności bazy danych, w górę lub w dół. Bazy danych w puli o nazwie *elastyczne baz danych*, automatyczne skalowanie w górę i w dół, aby spełniają żądanie. Elastyczne baz danych używają, ale nie przekraczają limity puli, aby koszty pozostaje przewidywalne, nawet w przypadku zastosowania bazy danych nie. Co więcej można [dodawać i usuwać baz danych do puli](sql-database-elastic-pool-manage-portal.md), skalowania aplikacji od kilku baz danych do tysięcy, wszystkie w ramach budżetu, który można sterować. Aby dowiedzieć się więcej o wzorców projektu dla aplikacji władz akredytacji bezpieczeństwa za pomocą pul elastyczne, zobacz [Desenie projektu dla wielu dzierżawy władz akredytacji bezpieczeństwa aplikacji z bazy danych SQL Azure](sql-database-design-patterns-multi-tenancy-saas-applications.md).

W obu przypadkach możesz przejść — jeden lub elastycznych — możesz nie jest zablokowany. Można wmieszać pojedynczy baz danych z pul elastyczne bazy danych, a następnie zmień warstwy usługi pojedynczy baz danych i pule do tworzenia projektów innowacyjnego. Ponadto przy użyciu dodatku i reach Azure, możesz mix i dopasowanie usługi Azure z bazy danych SQL nie spełnia określonych wymagań projektowanie unikatowych nowoczesny aplikacji, dysk efektywności zasobów i koszt i odblokowywanie nowych szans sprzedaży.

Ale jak możesz porównać działanie względne baz danych i pule bazy danych? Skąd wiadomo, kliknij prawym przyciskiem myszy Zatrzymaj podczas wybierania w górę i w dół? Odpowiedź jest jednostka transakcji bazy danych (DTU) dla pojedynczej baz danych i elastyczną DTU (eDTU) dla elastyczne baz danych i pule bazy danych. Zobacz [opcji bazy danych SQL i wydajności: opis, jakie opcje są dostępne w każdej warstwie usługi](sql-database-service-tiers.md) Aby uzyskać szczegółowe informacje.

## <a name="keep-your-app-and-business-running"></a>Organizowanie aplikacji i firmy

Azure w branży zera wiodące 99,99% dostępności Umowa dotycząca poziomu usług [(SLA)](http://azure.microsoft.com/support/legal/sla/)korzystająca z sieci globalnej zarządzany przez firmę Microsoft centrach danych pomaga zachować aplikacji z 24-7. Z każdej bazy danych SQL możesz korzystać z ochrony danych wbudowane, odporność na uszkodzenia i ochrona danych, które w przeciwnym razie należy zaprojektować, kupować, tworzenie i zarządzanie. Mimo tego w zależności od potrzeb firmy mogą wymagać dodatkowych warstw ochrony, aby upewnić się, że aplikacji i firmy można szybko odzyskać w przypadku awarii, komunikat o błędzie lub czegoś innego. W przypadku bazy danych SQL każdego poziomu usług oferuje menu różne funkcje, które umożliwiają szybkie uruchamianie i Zachowaj w ten sposób. Przywracanie w chwili służy do zwracania bazy danych do wcześniejszego stanu, od 35 dni. Ponadto jeśli Centrum danych, obsługi baz danych ulegnie awarii, możesz go przełączanie awaryjne do repliki bazy danych w innym regionie. Lub za pomocą repliki uaktualnienia lub przeniesienie do różnych obszarów.

![Geo Replikacja bazy danych SQL](./media/sql-database-technical-overview/azure_sqldb_map.png)


Aby uzyskać szczegółowe informacje o funkcjach ciągłości służbowych dostępne dla warstwy różnych usług, zobacz [Ciągłości](sql-database-business-continuity.md) .

## <a name="secure-your-data"></a>Zabezpieczanie danych
Program SQL Server ma tradycję zabezpieczania danych pełne, że baza danych SQL podtrzymuje z funkcjami ograniczysz dostęp, ochrony danych i ułatwiają monitorowanie aktywności. Aby uzyskać szybkie uwalnianie opcje zabezpieczeń, które masz w bazie danych SQL, zobacz [Zabezpieczanie bazy danych SQL](sql-database-security.md) . Odwiedź [Centrum zabezpieczeń dla aparatu bazy danych programu SQL Server i bazy danych SQL](https://msdn.microsoft.com/library/bb510589) dla widoku bardziej zaawansowane funkcje zabezpieczeń. I odwiedź witrynę [Centrum zaufania Azure](https://azure.microsoft.com/support/trust-center/security/) informacje na temat ochrony platformy Azure firmy.

## <a name="next-steps"></a>Następne kroki
Teraz, gdy została przeczytaj wprowadzenie do bazy danych SQL i udzielenie odpowiedzi na pytanie "Co to jest baza danych SQL?", będzie już gotowe do:

- Zobacz [ceny strony](https://azure.microsoft.com/pricing/details/sql-database/) i kalkulatory jednej bazie danych i porównanie kosztów elastyczne bazy danych.
- Informacje na temat [pul elastyczne](sql-database-elastic-pool.md).
- Aby rozpocząć pracę, [Tworzenie pierwszej bazy danych](sql-database-get-started.md).
- [Nawiązywanie połączenia i kwerendy z SSMS](sql-database-connect-query-ssms.md)
- Tworzenie pierwszej aplikacji w C#, Java, Node.js, PHP, Python lub dopiskiem: [biblioteki połączeń dla bazy danych SQL i programu SQL Server](sql-database-libraries.md)
- Zobacz indeks tytuły i opisy [wszystkich](sql-database-index-all-articles.md)tematów dotyczących usługi bazy danych sql Azure.
