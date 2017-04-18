<properties
   pageTitle="Włączanie szyfrowania przezroczysty danych w Centrum zabezpieczeń Azure | Microsoft Azure"
   description="Ten dokument pokazano, jak wykonania zalecenia Centrum zabezpieczeń Azure **Włącz przezroczyste szyfrowanie danych**."
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

# <a name="enable-transparent-data-encryption-in-azure-security-center"></a>Włączanie szyfrowania przezroczysty danych w Centrum zabezpieczeń Azure

Centrum zabezpieczeń Azure zaleca, włączyć przezroczysty szyfrowania danych (TDE) na bazy danych programu SQL, jeśli TDE nie jest już włączone. TDE chroni dane i ułatwia spełnia wymagania dotyczące zgodności przez szyfrowanie bazy danych, skojarzony wykonywania kopii zapasowych i pliki dziennika transakcji w stanie spoczynku, bez konieczności zmian w aplikacji. Aby dowiedzieć się więcej zobacz [Przezroczyste szyfrowanie danych z bazy danych SQL Azure](https://msdn.microsoft.com/library/dn948096).

Zalecenie to dotyczy usługa Azure SQL. nie zawiera SQL uruchomionych maszyn wirtualnych.

> [AZURE.NOTE] Ten dokument wprowadza usługę przy użyciu Przykładowe wdrożenie.  Nie jest to przewodnik krok po kroku.

## <a name="implement-the-recommendation"></a>Wykonania zalecenia

1. W karta **zalecenia** wybierz opcję **Włącz przezroczyste szyfrowanie danych**.
![Włączanie szyfrowania danych przezroczystości][1]

2. Spowoduje to otwarcie karta **Włącz przezroczyste szyfrowanie danych w bazach danych programu SQL** . Wybierz pozycję z bazą danych SQL, aby włączyć TDE.
![Wybieranie bazy danych SQL, aby włączyć TDE][2]
3. Na karta **szyfrowanie danych przezroczyste** w obszarze Szyfrowanie danych wybierz pozycję **Dalej** i wybierz przycisk **Zapisz** na Wstążce górny karta.
![Włączanie TDE][3]

  Po włączeniu TDE na wybranej bazy danych SQL **stanu szyfrowania** zmieni **szyfrowane**.    

  ![Stan szyfrowania][4]

## <a name="see-also"></a>Zobacz też

W tym artykule pokazano jak wdrażać rekomendacji Centrum zabezpieczeń "Włącz przezroczyste szyfrowanie danych". Aby dowiedzieć się więcej na temat SQL TDE, zobacz następujące artykuły:

- [Szyfrowanie przezroczysty danych z bazy danych Azure SQL](https://msdn.microsoft.com/library/dn948096)
- [Rozpoczynanie pracy z szyfrowania danych przezroczysty (TDE)](../sql-data-warehouse/sql-data-warehouse-encryption-tde.md)

Aby dowiedzieć się więcej na temat Centrum zabezpieczeń, zobacz następujące artykuły:

- [Ustawianie zasad zabezpieczeń w Centrum zabezpieczeń Azure](security-center-policies.md) — Dowiedz się, jak skonfigurować zasady zabezpieczeń dla usługi Azure subskrypcje i grup zasobów.
- [Zarządzanie zalecenia dotyczące zabezpieczeń w Centrum zabezpieczeń Azure](security-center-recommendations.md) — Dowiedz się, jak zalecenia ułatwiają ochrony Azure zasobów.
- [Monitorowanie kondycji zabezpieczeń w Centrum zabezpieczeń Azure](security-center-monitoring.md) — Dowiedz się, jak monitorowanie kondycji Azure zasobów.
- [Zarządzanie notatkami i odpowiadanie na alerty zabezpieczeń w Centrum zabezpieczeń Azure](security-center-managing-and-responding-alerts.md) — Dowiedz się, jak zarządzać i odpowiadanie na alerty zabezpieczeń.
- [Monitorowanie rozwiązań partnerów z Centrum zabezpieczeń Azure](security-center-partner-solutions.md) — Dowiedz się, jak można monitorować stan kondycji rozwiązań partnerów.
- [Azure zabezpieczeń Centrum — często zadawane pytania](security-center-faq.md) — znajdowanie często zadawane pytania dotyczące korzystania z usługi.
- [Blog dotyczący zabezpieczeń azure](http://blogs.msdn.com/b/azuresecurity/) — uzyskiwanie najnowsze Azure zabezpieczeń i informacji.

<!--Image references-->
[1]: ./media/security-center-enable-tde-on-sql-databases/enable-tde.png
[2]:./media/security-center-enable-tde-on-sql-databases/transparent-data-encryption-blade.png
[3]: ./media/security-center-enable-tde-on-sql-databases/turn-on-tde.png
[4]: ./media/security-center-enable-tde-on-sql-databases/encrypted.png
