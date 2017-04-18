<properties
   pageTitle="Raport analizy zagrożenia Centrum zabezpieczeń Azure | Microsoft Azure"
   description="Ten dokument pomaga znaleźć więcej informacji na temat alert zabezpieczeń za pomocą Azure Centrum zagrożenie inteligentnego raporty dotyczące zabezpieczeń podczas analizy."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/17/2016"
   ms.author="yurid"/>

# <a name="azure-security-center-threat-intelligence-report"></a>Raport analizy zagrożenia Centrum zabezpieczeń Azure
Ten dokument wyjaśniono, jak raporty inteligentnego zagrożenie Centrum zabezpieczeń Azure można dowiedzieć się więcej o zagrożenie, który wygenerował alert zabezpieczeń.

## <a name="what-is-a-threat-intelligence-report"></a>Co to jest zagrożenie raportu analizy?
Centrum zabezpieczeń zagrożenie wykrywania polega na monitorowanie informacje dotyczące zabezpieczeń w Azure zasobów, sieci i rozwiązań partnerów połączonego. Analizę te informacje, często korelacji informacje z wielu źródeł do identyfikowania zagrożeń. Ten proces jest częścią Centrum zabezpieczeń [możliwości wykrywania](security-center-detection-capabilities.md). 

Gdy Centrum zabezpieczeń identyfikuje zagrożenie, spowoduje [alertu zabezpieczeń](security-center-managing-and-responding-alerts.md), który zawiera szczegółowe informacje dotyczące określonego zdarzenia, łącznie z sugestii dotyczących działań naprawczych. Aby ułatwić zdarzenia odpowiedzi zespoły badanie i korygowanie zagrożenia, Centrum zabezpieczeń zawiera zagrożenie raportu analizy, który zawiera informacje o zagrożenie, które zostało wykryte, takich jak oraz informacje dotyczące: 

- Tożsamość osoby lub skojarzeń (Jeśli te informacje są dostępne)
- Cele ataki
- Ataki bieżących i historycznych kampanii (Jeśli te informacje są dostępne)
- Ataki taktyk, narzędzia i procedury
- Wskaźniki skojarzone ujawnienia (IoC), takie jak adresy URL i wartości skrótów plików
- Victimology, branżowe i geograficzne występowania ułatwiający określanie w przypadku Azure zasobów na ryzyko
- Informacje o łagodzenia i rozwiązywanie problemu

>[AZURE.NOTE] Zależą od ilości informacji w dowolnym konkretny raport; poziom szczegółów jest oparty na aktywności i występowania złośliwego oprogramowania.

Centrum zabezpieczeń zawiera trzy raporty zagrożenia, które mogą się różnić zgodnie z atakiem. Raporty dostępne są następujące:

- **Raport grupy aktywności**: zawiera głębokości dives na ataki, ich celów i taktyk.
- **Raport kampanii**: omówiono szczegóły kampanii określonych atakiem. 
- **Raport Podsumowanie zagrożenie**: obejmuje wszystkie elementy w poprzednich dwa raporty.

Tego typu danych jest bardzo przydatne podczas procesu [zdarzenia odpowiedzi](security-center-incident-response.md) przypadku trwającego dochodzenia opis źródła atakiem, motywacji jej i co należy zrobić w zmniejszeniu tego problemu, przechodzenie do przodu. 

## <a name="how-to-access-the-threat-intelligence-report"></a>Jak uzyskać dostęp do raportu analizy zagrożenie?

Możesz przejrzeć bieżącego alertów sprawdzając kafelków **alertów zabezpieczeń** . Otwórz Azure Portal i postępuj zgodnie z instrukcjami poniżej, aby wyświetlić więcej szczegółów dotyczących poszczególnych alertów:

1. Na pulpicie nawigacyjnym Centrum zabezpieczeń zostanie wyświetlona kafelków **alertów zabezpieczeń** .

2. Kliknij Kafelek, aby otworzyć kartę **alertów zabezpieczeń** , która zawiera szczegółowe informacje dotyczące alertów i kliknij w obszarze alertu zabezpieczeń, który chcesz uzyskać więcej informacji na temat.

    ![Alerty zabezpieczeń](./media/security-center-threat-report/security-center-threat-report-fig1.png)

3. W tym przypadku karta **podejrzanych proces wykonywane** przedstawia szczegółowe informacje dotyczące alertu, jak pokazano na poniższej ilustracji:

    ![Detais alert zabezpieczeń](./media/security-center-threat-report/security-center-threat-report-fig2.png)

4.  Ilość informacji, które są dostępne dla każdego alertu zabezpieczeń różnią się w zależności od typu alertu. W polu **Raporty** masz łącza do raportu analizy zagrożenia. Kliknij go, a inne okno przeglądarki będzie wyświetlana razem z pliku PDF.

    ![Zaznaczenie miejsca do magazynowania](./media/security-center-threat-report/security-center-threat-report-fig3.png)

W tym miejscu można pobrać plik PDF w celu ten raport i Przeczytaj więcej na temat problem z zabezpieczeniami, które zostało wykryte i wykonać czynności na podstawie informacji dostarczonych.

## <a name="see-also"></a>Zobacz też

W tym dokumencie wiesz, jak raporty inteligentnego zagrożenie Centrum zabezpieczeń Azure mogą ułatwić podczas analizy dotyczące alertów zabezpieczeń. Aby dowiedzieć się więcej na temat Centrum zabezpieczeń Azure, zobacz następujące artykuły:

- [Często zadawane pytania dotyczące Centrum zabezpieczeń azure](security-center-faq.md). Znajdowanie odpowiedzi na często zadawane pytania na temat korzystania z usługi.
- [Używanie Centrum zabezpieczeń Azure odpowiedź wystąpieniu zdarzenia](security-center-incident-response.md)
- [Możliwości wykrywania Centrum zabezpieczeń Azure](security-center-detection-capabilities.md)
- [Przewodnik po Centrum zabezpieczeń azure planowania i operacji](security-center-planning-and-operations-guide.md). Dowiedz się, jak planowanie i opis zagadnienia projektowe przyjęcie Centrum zabezpieczeń Azure.
- [Zarządzanie notatkami i odpowiadanie na alerty zabezpieczeń w Centrum zabezpieczeń Azure](security-center-managing-and-responding-alerts.md). Dowiedz się, jak zarządzać i odpowiadanie na alerty zabezpieczeń.
- [Obsługa zdarzenia zabezpieczeń w Centrum zabezpieczeń Azure](security-center-incident.md)
- [Blog dotyczący zabezpieczeń azure](http://blogs.msdn.com/b/azuresecurity/). Znajdź blogu wpisy o Azure zabezpieczenia i zgodność.
