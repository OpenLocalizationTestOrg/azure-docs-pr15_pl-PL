<properties
   pageTitle="Centrum zabezpieczeń ceny | Microsoft Azure"
   description="Ten artykuł zawiera informacje dotyczące cennik usługi Azure Centrum zabezpieczeń."
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
   ms.date="10/12/2016"
   ms.author="terrylan"/>

# <a name="azure-security-center-pricing"></a>Centrum zabezpieczeń Azure ceny

Centrum zabezpieczeń Azure ułatwia zapobieganie, wykrywanie i odpowiadanie na zagrożenia za pomocą lepszą wgląd i kontroli nad zabezpieczeniami Azure zasobów. Zapewnia zabezpieczenia zintegrowane monitorowania i zasad zarządzania w subskrypcjach usługi Azure, pomaga wykrywać zagrożenia, które w przeciwnym razie przejdź niezauważalnie i współpraca z Szeroki ekosystem rozwiązania zabezpieczeń.

## <a name="pricing-tiers"></a>Cennik warstwy

Centrum zabezpieczeń jest dostępne w dwóch poziomów:

- **Bezpłatne warstwa** jest automatycznie włączana dla wszystkich subskrypcji Azure. Bezpłatne warstwa zawiera wgląd stanu zabezpieczeń Azure zasobów, zasady zabezpieczeń podstawowe, zalecenia dotyczące zabezpieczeń i integracja z zabezpieczeń produktów i usług od partnerów.
- **Standardowa warstwa** dodaje funkcje wykrywania zagrożenie zaawansowane, w tym analizy zagrożenia, analizy funkcjonalne wykrywania anomalii, zdarzeń i zagrożenie ocen. **Bezpłatna wersja próbna 90 dni** jest dostępna dla warstwie standardowy.

Aby uzyskać więcej informacji zobacz Centrum zabezpieczeń [ceny strony](https://azure.microsoft.com/pricing/details/security-center/).

> [AZURE.NOTE] Centrum zabezpieczeń korzysta z magazynu Azure zapisywania danych zabezpieczeń wynikiem węzły chroniony. Koszty skojarzone z tego miejsca do magazynowania nie są uwzględniane w cenę usługi i są naliczane oddzielnie według zwykłe [stawki Azure miejsca do magazynowania](https://azure.microsoft.com/pricing/details/storage/blobs/). Miejsca do magazynowania opłata nawet podczas próby.

## <a name="try-standard-free-for-90-days"></a>Spróbuj standardowe bezpłatnie przez 90 dni

Bezpłatną wersję próbną 90 dni jest dostępna dla poziomu standardowy. Aby uzyskać bezpłatną wersję próbną standardowej warstwy, wybierz Kafelek **zasad** na karta **Centrum zabezpieczeń** . Wybierz subskrypcję, którą chcesz uaktualnić na standardowy. Na karta **zasad zabezpieczeń** wybierz **poziom ceny**. Na karta **Wybierz swojego cennik warstwa** wybierz **Standard — bezpłatnej wersji próbnej**.

![Bezpłatna wersja próbna][1]

Na końcu 90 dni należy wybrać nadal korzystać z usługi, firma Microsoft będzie automatycznie uruchamiany opłat za zastosowania.

## <a name="upgrade-to-standard"></a>Uaktualnianie do standardowego

Uaktualnianie do warstwie standardowy Dodawanie wykrywania zagrożenie zaawansowane. Aby uzyskać warstwie standardowy, wybierz Kafelek **zasad** na karta **Centrum zabezpieczeń** . Wybierz subskrypcję, którą chcesz uaktualnić na standardowy. Na karta **zasad zabezpieczeń** wybierz **poziom ceny**. Na karta **Wybierz swojego cennik warstwa** wybierz **Standardowy**.

![Standardowy warstwy][2]

## <a name="why-upgrade-to-standard"></a>Dlaczego warto dokonać uaktualnienia do standardowego?

Standardowy warstwa Centrum zabezpieczeń zawiera wszystkie funkcje bezpłatne warstwa oraz wykrywania zagrożenie zaawansowane. Zaawansowane zagrożenie wykrywania ułatwia zidentyfikowanie aktywne zagrożeń kierowanie Azure zasobów i umożliwia wniosków potrzeby szybkiej odpowiedzi.

Centrum zabezpieczeń wykorzystuje analizy zabezpieczeniami zaawansowanymi, które wykraczają poza rozwiązań opartych na podpis. Osiągnięć w big data i maszynowego uczenia technologie są osobie ma być obliczona wydarzeń na tkaninie całą chmurą — wykrywania zagrożeń, których nie można wysyłać do identyfikowania przy użyciu metod ręcznego i przewidywania rozwoju atakami.

Analizy zabezpieczeń, dostarczane z poziomu standardowej są:

- **Zagrożenia analizy** - wyszukuje znane aktorów ciągów bad przy użyciu analizy zagrożenia globalnej z produktów firmy Microsoft i usług, jednostki zbrodni cyfrowy firmy Microsoft, Microsoft Security Response Center i zewnętrznych źródeł danych
- **Analiza funkcjonalne** — dotyczy znane wzorców do znalezienia złośliwe działanie
- **Wykrywania anomalii** - używa profilowanie statystyczne do utworzenia historycznych według planu bazowego. Alert na odchylenia od ustalonej plany bazowe, które są zgodne z potencjalnymi atakiem

W poniższych karta **alertów zabezpieczeń** Centrum zabezpieczeń wykrył **zdarzeniem**bezpieczeństwa. Zdarzeniem bezpieczeństwa jest agregacją wszystkie alerty dla zasobu, które są dostosowane deseniami łańcuch funkcji. Wybieranie zdarzeniem bezpieczeństwa powoduje wyświetlenie więcej szczegółów dotyczących zdarzenia oraz listy powiązanych alertów. Wybieranie alertu zawiera więcej informacji na temat to wystąpienie.

![Zdarzeniem bezpieczeństwa][3]

Alert **komunikacji sieciowej** poniżej szczegółowe informacje na temat alertu. Szczegóły zawiera pełny opis, jego ważności, jego bieżącym stanie (który w tym przypadku jest odrzucone, co oznacza użytkownik miał działania w celu jego odrzucenia) zaatakowanych zasobów i czynności działań naprawczych. Istnieje także lista łączy do raportów analizy zagrożenia firmy Microsoft. Te raporty mogą być używane do celów obronnych i rozwiązywanie problemu zabezpieczeń.

![Szczegóły alertu zabezpieczeń][4]

## <a name="enable-data-collection"></a>Włącz zbieranie danych

Aby włączyć analizy funkcjonalne maszyn wirtualnych, musi być włączony zbioru danych. Może być konieczne włączenie zbierania danych po raz pierwszy dostępu Centrum zabezpieczeń lub podczas tworzenia zasady zabezpieczeń.

Aby sprawdzić, czy jest włączona zbierania danych, wybierz Kafelek **zasad** . Karta **Zasady zabezpieczeń** zostanie wyświetlona lista subskrypcji Azure. Wybierz subskrypcję. Jeśli **zbiór danych** jest wyłączona, zmień go za i zapisać zmiany. Zobacz [Włączanie zbierania danych w Centrum zabezpieczeń Azure](security-center-enable-data-collection.md).

## <a name="next-steps"></a>Następne kroki

- W tym dokumencie zostały wprowadzone do cen dla Centrum zabezpieczeń. Aby uzyskać dodatkowe informacje cennik zobacz Centrum zabezpieczeń [ceny strony](https://azure.microsoft.com/pricing/details/security-center/).
- Aby dowiedzieć się więcej o funkcjach zaawansowane wykrywania Centrum zabezpieczeń, zobacz [Centrum zabezpieczeń Azure wykrywania możliwości](security-center-detection-capabilities.md).
- Jeśli masz pytania dotyczące korzystania z Centrum zabezpieczeń, zobacz [Często zadawane pytania dotyczące Azure zabezpieczeń Centrum](security-center-faq.md).
- Jeśli nadal masz pytania dotyczące korzystania z Centrum zabezpieczeń lub Azure, odwiedź [Fora Azure](https://social.msdn.microsoft.com/Forums/home?forum=AzureSecurityCenter&filter=alltypes&sort=lastpostdesc).

<!--Image references-->
[1]: ./media/security-center-pricing/free-trial.png
[2]: ./media/security-center-pricing/standard.png
[3]: ./media/security-center-pricing/incident.png
[4]: ./media/security-center-pricing/network-alert.png
