<properties
   pageTitle="Włączanie inspekcji na serwerach SQL Azure Centrum zabezpieczeń | Microsoft Azure"
   description="Ten dokument pokazano, jak wykonania zalecenia Centrum zabezpieczeń Azure **włączyć inspekcję na serwerach SQL**."
   services="security-center"
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
   ms.date="07/29/2016"
   ms.author="terrylan"/>

# <a name="enable-auditing-on-sql-servers-in-azure-security-center"></a>Włączanie inspekcji na serwerach SQL Azure Centrum zabezpieczeń

Centrum zabezpieczeń Azure zaleca, włączanie inspekcji dla wszystkich baz danych na serwerach Azure SQL, jeśli inspekcja nie został jeszcze włączony. Inspekcja może pomóc Obsługa zgodności z przepisami, opis działania bazy danych i lepszy wgląd w niezgodności i różnic w odniesieniu, które mogą wskazywać dotyczy firm lub podejrzane naruszenie zabezpieczeń.

Po włączeniu inspekcji można skonfigurować ustawienia wykrywania zagrożenia i wiadomości e-mail, aby otrzymywać alerty zabezpieczeń. Wykrywanie zagrożenie wykrywa działania anomalous bazy danych, wskazująca potencjalnych zagrożeniach do bazy danych. Umożliwia wykrywanie i odpowiadanie na potencjalne zagrożenia występujące.

Zalecenie to dotyczy usługa Azure SQL. nie zawiera programu SQL Server uruchomionych maszyn wirtualnych w usługach infrastruktury Azure (Azure IaaS).

> [AZURE.NOTE] Ten dokument wprowadza usługę przy użyciu Przykładowe wdrożenie.  Nie jest to przewodnik krok po kroku.

## <a name="implement-the-recommendation"></a>Wykonania zalecenia

1. W karta **zalecenia** wybierz opcję **Włącz inspekcji na serwerach SQL**.  Spowoduje to otwarcie karta **Włączanie inspekcji na serwerach SQL** .
![Włączanie inspekcji na serwerach SQL][1]

2. Wybierz pozycję programu SQL server, aby włączyć inspekcję w. Spowoduje to otwarcie karta **Ustawienia inspekcji** .
![Ustawienia inspekcji][2]
3. Na karta **Ustawienia inspekcji** zaznacz **w** obszarze **Inspekcja**.
![Włączanie ustawienia inspekcji][3]

4. Postępuj zgodnie z instrukcjami [wprowadzenie inspekcja bazy danych SQL](../sql-database/sql-database-auditing-get-started.md) do skonfigurowania magazynu przechowywania dzienników inspekcji. Subskrypcji konto miejsca do magazynowania dla zbioru danych jest domyślne konto miejsca do magazynowania.

5. Postępuj zgodnie z instrukcjami w [wprowadzenie wykrywania zagrożenie bazy danych SQL](../sql-database/sql-database-threat-detection-get-started.md) , aby włączyć i skonfigurować wykrywania zagrożenia i skonfigurować na liście wiadomości e-mail, które będą otrzymywać alerty zabezpieczeń w przypadku wykrycia anomalous działań.

## <a name="see-also"></a>Zobacz też

W tym artykule pokazano jak wdrażać rekomendacji Centrum zabezpieczeń "Włącz inspekcji na serwerach SQL". Aby dowiedzieć się więcej o zabezpieczaniu bazy danych SQL, zobacz następujące artykuły:

- [Zabezpieczanie bazy danych SQL](../sql-database/sql-database-security.md)

Aby dowiedzieć się więcej na temat Centrum zabezpieczeń, zobacz następujące artykuły:

- [Ustawianie zasad zabezpieczeń w Centrum zabezpieczeń Azure](security-center-policies.md) — Dowiedz się, jak skonfigurować zasady zabezpieczeń dla usługi Azure subskrypcje i grup zasobów.
- [Zarządzanie zalecenia dotyczące zabezpieczeń w Centrum zabezpieczeń Azure](security-center-recommendations.md) — Dowiedz się, jak zalecenia ułatwiają ochrony Azure zasobów.
- [Monitorowanie kondycji zabezpieczeń w Centrum zabezpieczeń Azure](security-center-monitoring.md) — Dowiedz się, jak monitorowanie kondycji Azure zasobów.
- [Zarządzanie notatkami i odpowiadanie na alerty zabezpieczeń w Centrum zabezpieczeń Azure](security-center-managing-and-responding-alerts.md) — Dowiedz się, jak zarządzać i odpowiadanie na alerty zabezpieczeń.
- [Monitorowanie rozwiązań partnerów z Centrum zabezpieczeń Azure](security-center-partner-solutions.md) — Dowiedz się, jak można monitorować stan kondycji rozwiązań partnerów.
- [Azure zabezpieczeń Centrum — często zadawane pytania](security-center-faq.md) — znajdowanie często zadawane pytania dotyczące korzystania z usługi.
- [Blog dotyczący zabezpieczeń azure](http://blogs.msdn.com/b/azuresecurity/) — uzyskiwanie najnowsze Azure zabezpieczeń i informacji.

<!--Image references-->
[1]: ./media/security-center-enable-auditing-on-sql-server/enable-auditing-on-sql-servers.png
[2]:./media/security-center-enable-auditing-on-sql-server/enable-auditing.png
[3]: ./media/security-center-enable-auditing-on-sql-server/auditing-settings-blade.png
