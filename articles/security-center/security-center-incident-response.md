<properties
   pageTitle="Korzystanie z Centrum zabezpieczeń Azure zdarzenia odpowiedź | Microsoft Azure"
   description="Ten dokument wyjaśniono, jak za pomocą Centrum zabezpieczeń Azure w scenariuszu zdarzenia odpowiedź."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.topic="hero-article"
   ms.devlang="na"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/20/2016"
   ms.author="yurid"/>

# <a name="using-azure-security-center-for-an-incident-response"></a>Korzystanie z Centrum zabezpieczeń Azure zdarzenia odpowiedź
Wiele organizacji Dowiedz się, jak reagować z bezpieczeństwem dopiero po które atakiem. Aby zmniejszyć koszty i uszkodzenia, jest dostęp do zdarzenia odpowiedzi planu przed wprowadzeniem pakiecie. Za pomocą Centrum zabezpieczeń Azure w różnych etapach zdarzenia odpowiedź.

## <a name="incident-response-planning"></a>Planowanie reakcji wystąpieniu zdarzenia

Plan skutecznych zależy od trzy podstawowe możliwości: możliwość ochrony wykrywanie i odpowiadanie na zagrożenia. Ochrona jest zablokowania zdarzeń, wykrywanie jest o wczesne Identyfikowanie zagrożeń i odpowiedź jest o eksmitowania tej i przywracanie systemów zmniejszeniu skutki naruszenia.

W tym artykule użyje etapy zdarzenia odpowiedź zabezpieczeń z tego artykułu [Microsoft Azure zabezpieczeń odpowiedź w chmurze](https://gallery.technet.microsoft.com/Azure-Security-Response-in-dd18c678) , jak pokazano na poniższym rysunku:

![Cykl życia odpowiedzi wystąpieniu zdarzenia](./media/security-center-incident-response/security-center-incident-response-fig1.png)

Za pomocą Centrum zabezpieczeń etapach Wykryj, oceny i diagnozowanie. Poniżej przedstawiono przykłady jak Centrum zabezpieczeń mogą być przydatne w trzech etapach początkowej odpowiedzi zdarzenia:

- **Wykryj**: przeglądanie pierwsze wskazanie dochodzenia zdarzenia.
    - Przykład: Przegląd wstępnej weryfikacji, że alert zabezpieczeń Wysoki priorytet został dostarczony na pulpicie nawigacyjnym Centrum zabezpieczeń.
- **Oceny**: wykonywanie wstępnej oceny, aby uzyskać więcej informacji na temat podejrzane działania.
    - Przykład: Uzyskaj więcej informacji na temat alertu zabezpieczeń.
- **Diagnozowanie**: przeprowadzanie badania technicznego i określenie strategii zamknięcia, łagodzenia i rozwiązania.
    - Przykład: postępuj zgodnie z instrukcjami naprawy, opisanego przez Centrum zabezpieczeń, w tym obszarze alertu zabezpieczeń określonego.

Scenariusz znajdujący się pokazano, jak korzystać z Centrum zabezpieczeń etapach Wykryj, oceny i diagnozowanie i odpowiedzieć zdarzeniem bezpieczeństwa. W Centrum zabezpieczeń [zdarzeniem bezpieczeństwa](security-center-incident.md) jest agregacją wszystkie alerty dla zasobu, które są dostosowane deseniami [skasować łańcucha](https://blogs.technet.microsoft.com/office365security/addressing-your-cxos-top-five-cloud-security-concerns/) . Zdarzenia są wyświetlane w kafelku [alertów zabezpieczeń](security-center-managing-and-responding-alerts.md) i karta. Zdarzenia powoduje wyświetlenie listy powiązanych alertów, dzięki czemu można uzyskać więcej informacji na temat każdego wystąpienia. Centrum zabezpieczeń przedstawia także autonomicznej alertów zabezpieczeń, które można także śledzić podejrzane działania.

## <a name="scenario"></a>Scenariusz

Contoso ostatnio migracji niektórych ich zasobów lokalnych Azure, w tym niektórych oparte na maszyn wirtualnych systemu LOB obciążenia i baz danych programu SQL. Obecnie zdarzeniem odpowiedzi zespołu (CSIRT) bezpieczeństwa komputera Core firmy Contoso występuje problem badanie problemów z zabezpieczeniami ze względu na analizy zabezpieczeń nie jest zintegrowany z ich bieżącego narzędzia zdarzenia odpowiedź. Ten brak integracji wprowadza problem podczas etapu Wykryj (zbyt wiele wyniki fałszywie dodatnie), a także etapach oceny i diagnozowanie. W ramach tej migracji ich decyzję korzystania z Centrum zabezpieczeń ułatwić im rozwiązania tego problemu.

Pierwszy etap tej migracji zakończone po ich onboarded wszystkie zasoby i wszystkie zalecenia dotyczące zabezpieczeń w Centrum zabezpieczeń. Contoso CSIRT jest punkt centralny na sposób postępowania z bezpieczeństwem komputera. Zespół składa się z grupy osób odpowiedzialnych za sposób postępowania z dowolnym zdarzeniem bezpieczeństwa. Członkowie zespołu jasno zdefiniowane obowiązki aby znajdowała się bez obszaru odpowiedzi niepokrytych.

W tym scenariuszu w celu możemy zacząć fokusu ról personas następujące czynności, które są częścią Contoso CSIRT:

![Cykl życia odpowiedzi wystąpieniu zdarzenia](./media/security-center-incident-response/security-center-incident-response-fig2.png)

Jan znajduje się w operacji zabezpieczeń. Jej obowiązki obejmują:

- Monitorowanie i reagowanie na zagrożeniami całą dobę.
- Zamocowaniem chmury obciążenie pracą właściciela lub analityka zabezpieczeń stosownie do potrzeb.

Sam jest analityka zabezpieczeń i jego obowiązków obejmują:

- Analizuje atakami.
- Korygując alerty.
- Praca z pracą właścicieli określanie i stosowanie czynniki.

Jak widać, Jan i Sam mają różne obowiązki, a ich musi współpracować, aby udostępnić informacje o Centrum zabezpieczeń.

## <a name="recommended-solution"></a>Zalecane rozwiązanie

Ponieważ Jan i Sam mają różne role, one obsługiwać za pomocą różnych obszarów Centrum zabezpieczeń uzyskać odpowiednich informacji o ich codziennych czynności. Jan użyje **alertów zabezpieczeń** jako część jej codziennego monitorowania.

![Alerty zabezpieczeń](./media/security-center-incident-response/security-center-incident-response-fig3.png)

Jan użyje alertów zabezpieczeń etapach Wykryj i oceny. Po zakończeniu wstępnej oceny Jan Anna może Przekształcanie problem Sam, jeśli wymagane jest dodatkowe badania. W tym momencie Sam użyje informacji podanych przez Centrum zabezpieczeń, czasami w połączeniu z innych źródeł danych, aby przejść do etapu diagnozowanie.


## <a name="how-to-implement-this-solution"></a>Jak wdrażać tego rozwiązania

Aby zobaczyć, jak chcesz użyć Centrum zabezpieczeń Azure w scenariuszu zdarzenia odpowiedzi, firma Microsoft będzie procedury w Jan etapami Wykryj i oceny, a następnie sprawdź do diagnozowania problemu działanie Sam.

### <a name="detect-and-assess-incident-response-stages"></a>Wykrywanie i oceń etapów odpowiedzi wystąpieniu zdarzenia

Jan zalogowanie się do portalu Azure i działa w konsoli Centrum zabezpieczeń. W ramach swojego codziennie monitorowanie aktywności Anna pracę, Przeglądanie alertów zabezpieczeń Wysoki priorytet, wykonując następujące czynności:

1. Kliknij Kafelek **alertów zabezpieczeń** i uzyskać dostęp do karta **alertów zabezpieczeń** .
    ![Karta alert zabezpieczeń](./media/security-center-incident-response/security-center-incident-response-fig4.png)

    > [AZURE.NOTE] Do celów w tym scenariuszu Jan ma wykonywać ocenę na alert aktywności złośliwy SQL, jak pokazano na kolejnej ilustracji.
2. Kliknij alert, **złośliwy SQL aktywności** i przejrzyj zaatakowanych zasobów w karta **aktywności złośliwy SQL** :  ![szczegóły zdarzenia](./media/security-center-incident-response/security-center-incident-response-fig5.png)

    W tym karta Jan może potrwać uwagi dotyczące zaatakowanych zasobów, jak często ataki się stało, a kiedy zostało wykryte.
3. Kliknij pozycję **zaatakowany zasobów** , aby uzyskać więcej informacji o tym atakiem.

Po przeczytaniu opis, Jan jest przekonana, że nie jest to wynik fałszywie dodatni i że powinna ona przekształcanie tej litery na Sam.

### <a name="diagnose-incident-response-stage"></a>Diagnozowanie etapu odpowiedzi wystąpieniu zdarzenia

Sam odbiera wielkości liter od Jan i uruchamiania, recenzowanie czynności naprawy, które sugerowane Centrum zabezpieczeń.

![Cykl życia odpowiedzi wystąpieniu zdarzenia](./media/security-center-incident-response/security-center-incident-response-fig6.png)

### <a name="additional-resources"></a>Dodatkowe zasoby

Zespół zdarzenia odpowiedzi można również korzystać z możliwości [Usługi Power BI Centrum zabezpieczeń](security-center-powerbi.md) w celu wyświetlenia różnych typów raportów. Tych raportów może pomóc w ich trakcie dalszego dochodzenia do wizualizacji oraz ich analizowania i filtrowania zalecenia i alerty zabezpieczeń. W przypadku firm używające ich informacje dotyczące zabezpieczeń i rozwiązanie do zarządzania (SIEM) zdarzenia podczas procesu badania ich można także [zintegrować Centrum zabezpieczeń z ich rozwiązania](security-center-integrating-alerts-with-log-integration.md). Za pomocą [Narzędzia integracji Azure dziennika](https://blogs.msdn.microsoft.com/azuresecurity/2016/07/21/microsoft-azure-log-integration-preview/), można zintegrować dzienników inspekcji Azure i zdarzeń zabezpieczeń maszyn wirtualnych (maszyn wirtualnych). Aby zbadać atakiem, możesz użyć tych informacji w połączeniu z Centrum zabezpieczeń zawiera informacje.


## <a name="conclusion"></a>Wnioski

Montaż zespołu przed zdarzeniem jest bardzo ważne dla Twojej organizacji i skutecznie wpłynie obsługi zdarzeń. Masz odpowiednie narzędzia do monitorowania zasobów może pomóc zespołowi korygowania zdarzeniem bezpieczeństwa dokładne czynności. Centrum zabezpieczeń [możliwości wykrywania](security-center-detection-capabilities.md) może pomóc IT, aby szybko odpowiedzieć zdarzeń i korygowanie problemach z zabezpieczeniami.
