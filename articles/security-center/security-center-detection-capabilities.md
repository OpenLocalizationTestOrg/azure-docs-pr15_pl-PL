<properties
   pageTitle="Funkcje wykrywania dostępne w Centrum zabezpieczeń Azure | Microsoft Azure"
   description="Ten dokument pomaga sposób działania funkcji wykrywania Centrum zabezpieczeń Azure."
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
   ms.date="09/22/2016"
   ms.author="yurid"/>

# <a name="azure-security-center-detection-capabilities"></a>Możliwości wykrywania Centrum zabezpieczeń Azure
Centrum zabezpieczeń Azure wykrywania zaawansowane funkcje, które ułatwia identyfikowanie zagrożeń aktywne kierowanie zasobów Microsoft Azure i udostępnia wniosków niezbędne do odpowiadania szybko omówione w tym dokumencie.

> [AZURE.NOTE] Zaawansowane wykryć są dostępne w standardowej warstwa z Azure Centrum zabezpieczeń. Bezpłatną wersję próbną 90 dni jest dostępna. Można też uaktualnić z poziomu ceny zaznaczenia w [Zasady zabezpieczeń](security-center-policies.md). Odwiedź [stronę Centrum zabezpieczeń](https://azure.microsoft.com/pricing/details/security-center/) , aby dowiedzieć się więcej o ceny. 


## <a name="responding-to-todays-threats"></a>Odpowiadanie na bieżącą zagrożeń
Są istotne zmiany w orientacji poziomej zagrożenie przez ostatnie lata 20. W przeszłości firmy zazwyczaj tylko sprzedał martwić się o defacement witryny sieci web przez poszczególne atakami, które zostały głównie zainteresować oglądanie "co on może robić". Ataki dzisiejszą są bardziej zaawansowane i organizację. Często mają one konkretne cele finansowych i strategic. Również mają więcej zasobów dostępnych dla nich, jak mogą być finansowane przez Państwa kraju lub zorganizowaną przestępczością.

Tej metody doprowadziło do poziomu bezprecedensowej profesjonalizmu w rangę atakująca. Nie są już one chcą atak skutkujący zmianą zawartości sieci web. Teraz są one chcą kradzieżom informacji, konta i dane prywatne — co mogą również używać do generowania gotówkowych na rynku lub, aby korzystać z określonego rodzaju, Stanach lub wojskowych położenie. Dotyczące jeszcze więcej niż tych intruzów finansowe celu atakami, które naruszenia sieci do uszkodzenia infrastruktury i osób.

W odpowiedzi organizacje często wdrożyć różne rozwiązania punktów, które skoncentrować się na obrony przedsiębiorstwa lub punktów końcowych, szukając znane atakiem podpisy. Te rozwiązania zwykle prowadzi do generowania dużej liczby alertów niskiej jakości, które wymagają analityka zabezpieczenia sprawdzać i badanie. Większość organizacji brakuje czasu i wiedzy reagować na tych alertów — tak dużo podróży unaddressed.  W międzyczasie intruzów usprawnionych sposobów subvert wiele zabezpieczenia oparte na podpisu i [dostosować do środowiska chmury](https://azure.microsoft.com/blog/detecting-threats-with-azure-security-center/). Nowe podejście są wymagane do szybciej identyfikowanie zagrożeniami i usprawnić wykrywanie i odpowiedzi. 

## <a name="how-azure-security-center-detects-and-responds-to-threats"></a>Jak Centrum zabezpieczeń Azure wykryje i odpowiada zagrożeń

Pracowników badawczych zabezpieczeń firmy Microsoft są stale szukać zagrożeń. Mają dostęp do lepszego zbiór telemetrycznego zdobyte obecności globalnej firmy Microsoft w chmurze i lokalnego. Ta kolekcja możliwość kontaktowania sieci i różnych zestawów danych umożliwia firmie Microsoft Odkryj nowe ataki wzorców i trendów w jego lokalnymi produktami dla klientów indywidualnych i przedsiębiorstw, a także jego usług online. W wyniku Centrum zabezpieczeń szybko zaktualizować jego algorytmy wykrywania jak ataki Zwolnij nowych i bardziej zaawansowane oprogramowanie. Ta metoda ułatwia się szybkie przenoszenie środowiska zagrożenie. 

Centrum zabezpieczeń zagrożenie wykrywania polega na automatyczne zbieranie informacji o zabezpieczeniach od Azure zasobów, sieci i rozwiązań partnerów połączonego. Analizę te informacje, często korelacji informacje z wielu źródeł do identyfikowania zagrożeń. Alerty zabezpieczeń są priorytety w Centrum zabezpieczeń wraz z zaleceniami dotyczącymi sposobu korygowania zagrożenia.

![Zbieranie danych Centrum zabezpieczeń i prezentacji](./media/security-center-detection-capabilities/security-center-detection-capabilities-fig1.png)

Centrum zabezpieczeń wykorzystuje analizy zabezpieczeniami zaawansowanymi, które wykraczają poza rozwiązań opartych na podpis. Osiągnięć w technologiach [nauki komputera](https://azure.microsoft.com/blog/machine-learning-in-azure-security-center/) i duży danych są osobie ma być obliczona wydarzeń na tkaninie całą chmurą — wykrywania zagrożeń, których nie można wysyłać do identyfikowania przy użyciu metod ręcznego i przewidywania rozwoju atakami. Te analizy zabezpieczeń obejmują: 

- **Zagrożenia zintegrowane analizy**: wyszukuje znane aktorów ciągów bad wykorzystując analizy zagrożenia globalnej z produktów firmy Microsoft i usług, jednostkę zbrodni cyfrowego (DCU) firmy Microsoft, zabezpieczeń odpowiedzi centrum MSRC (Microsoft) i zewnętrznych źródeł danych.
- **Funkcjonalne analizy**: dotyczy znane wzorców do znalezienia złośliwe działanie. 
- **Wykrywania anomalii**: używa profilowanie statystyczne do utworzenia historycznych według planu bazowego. Alert na odchylenia od ustalonej plany bazowe, które są zgodne z potencjalnymi atakiem.


### <a name="threat-intelligence"></a>Zagrożenia analizy
Firma Microsoft udostępnia olbrzymi ilość zagrożenie globalnej analizy. Telemetrycznego przepływał w z wielu źródeł, takich jak Azure, Office 365, Microsoft CRM online, Microsoft Dynamics AX, outlook.com, MSN.com, Microsoft cyfrowego zbrodni jednostek (DCU) i zabezpieczeń odpowiedzi centrum MSRC (Microsoft). Pracowników badawczych także otrzymywać informacje analizy zagrożenia współużytkowany dostawców usług w chmurze głównych i subskrybowanie źródła analizy zagrożenia od osób trzecich. Centrum zabezpieczeń Azure umożliwia powiadomienia o zagrożeniach ze znanych aktorów ciągów bad te informacje. Przykłady:

- **Komunikacji wychodzącej złośliwy adres IP**: ruchu wychodzącego znane botnet lub darknet prawdopodobnie oznacza, że zostało naruszone zasób atakującej go próby wykonania poleceń systemu lub exfiltrate danych. Centrum zabezpieczeń Azure różni się ruch sieciowy do bazy danych globalnej zagrożenie firmy Microsoft i ostrzega, jeśli wykryje komunikacji złośliwy adres IP.

## <a name="behavioral-analytics"></a>Analizy funkcjonalne

Funkcjonalne analizy to technika, która analizuje i porównuje dane do kolekcji deseni znane. Jednak tych deseni nie jest proste podpisy. Są one ustalane przez złożone maszynowego uczenia algorytmy, które są stosowane do dużych zestawów danych. Są również zależą od za pośrednictwem dokładnej analizy złośliwy zachowania analityków ekspertów. Centrum zabezpieczeń Azure umożliwia określenie złamany zasobów według analizy dzienników maszyn wirtualnych, wirtualną sieć urządzenia dzienniki, dzienniki tkaninie, zrzuty awaryjne i innych źródeł funkcjonalne analizy. 

Ponadto jest korelacji z innych sygnały, aby sprawdzić obsługi dowodów szerokie kampanii. Ten korelacji ułatwia identyfikowanie zdarzenia, które są zgodne z wskazanych wskaźniki ujawnienia. Przykłady:

- **Wykonanie procesu Suspicious**: intruzów zastosować kilka technik wykonywanie złośliwego oprogramowania bez wykrywania. Na przykład atakującej może nadać przed złośliwym oprogramowaniem takich samych nazwach jako godny zaufania system plików, ale umieść te pliki w innej lokalizacji, użyj nazwy, która jest bardzo podobne do pliku niegroźne lub maski rozszerzenie PRAWDA. Centrum zabezpieczeń modeli zachowania procesów i monitorów procesu wykonania wykrywanie wartości odstających, takie jak te.  
- **Ukryte przed złośliwym oprogramowaniem i wykorzystywania prób**: zaawansowanych złośliwego oprogramowania jest możliwość uniknięcia tradycyjnych ochrony przed złośliwym oprogramowaniem produkty według nigdy nie zapisywanie na dysku lub szyfrowania składników oprogramowania zapisany na dysku.  Jednak tych przed złośliwym oprogramowaniem można wykryć przy użyciu analizy pamięci jako złośliwego oprogramowania należy pozostawić śledzenia w pamięci do działania. Gdy ulega awarii oprogramowania, zrzut awaryjny przechwytuje część pamięci w czasie awarii.  Analizując pamięci w zrzut awaryjny Azure Centrum zabezpieczeń może wykryć techniki wykorzystać luk w oprogramowaniu, uzyskiwać dostęp do danych poufnych i podglądanie pozostają w komputerze złamany bez wpływ na wydajność komputera.
- **Boczny przepływu i wewnętrznych rozpoznawczych**: pozostać na złamany sieci i Znajdź zbiorów danych przydatne, ataki często będą próbować poruszać się po bokach z złamany komputera innym osobom w tej samej sieci. Centrum zabezpieczeń monitoruje proces i logowania działania w celu wykrywania prób, aby rozwinąć stopień osoby w sieci, takich jak badanie sieci wykonanie polecenia zdalnego i wyliczanie kont.
- **Złośliwych skryptów programu PowerShell**: programu PowerShell jest używany przez intruzów do wykonania złośliwy kod w przypadku maszyn wirtualnych docelowej dla różnych celów. Centrum zabezpieczeń sprawdza działanie programu PowerShell dla dowodów podejrzane działania. 
- **Ataki wychodzącej**: intruzów często docelowe chmurze zasobów w celu przy użyciu tych zasobów, aby zainstalować dodatkowe atakami. Złamany maszyn wirtualnych, na przykład może służyć do uruchamianie atakami względem pozostałych maszyn wirtualnych, wysyłanie wiadomości-ŚMIECI lub skanowania otwarte porty i innych urządzeń w Internecie. Stosując nauki maszynowego do ruch sieciowy, Centrum zabezpieczeń może wykryć komunikacji wychodzącej przekracza normę. W przypadku wiadomości-ŚMIECI, Centrum zabezpieczeń również skorelowany jest ruch nietypowych wiadomości e-mail z analiz z usługi Office 365, aby ustalić, czy wiadomość jest prawdopodobnie nefarious lub wynik kampanii wiarygodnych wiadomości e-mail.  

### <a name="anomaly-detection"></a>Wykrywania anomalii

Centrum zabezpieczeń Azure również używana wykrywania anomalii zagrożeń. W przeciwieństwie do analizy funkcjonalne, (która zależy od znane wzorców pochodzących z dużych zestawów danych), wykrywania anomalii jest bardziej "spersonalizowane" i omówiono plany bazowe, które są specyficzne dla wdrożeń. Maszynowego uczenia jest stosowana do określenia normalnego działania w przypadku wdrożeń programu, a następnie reguły są generowane określają warunki odstające, reprezentujących zdarzeń zabezpieczeń. Oto przykład:

- **Ruch przychodzący RDP-SSH atakami wymusić atakami**: Wdrażanie może być zajęty maszyn wirtualnych z dużą ilością logowania do każdego dnia i innych maszyn wirtualnych, które mają bardzo mało lub dowolny logowania. Centrum zabezpieczeń Azure można określić działania logowania według planu bazowego dla tych maszyn wirtualnych i używać maszynowego uczenia definiującej poza logowania normalnego działania. Jeśli liczba logowania lub godzinę logowania lub lokalizacji, z której wymagane są logowania lub inne cechy związane z logowaniem się znacznie różni się od planu bazowego, można wygenerować alertu. Ponownie maszynowego uczenia Określa, co to jest znaczące.

## <a name="continuous-threat-intelligence-monitoring"></a>Monitorowanie analizy zagrożenie

Centrum zabezpieczeń Azure działa zabezpieczeń badań i danych naukowych zespoły, które stale monitorować zmiany w orientacji poziomej zagrożenie. Ta opcja uwzględnia inicjatywy następujące czynności:

- **Monitorowanie analizy zagrożenia**: zagrożenie analizy zawiera mechanizmy, wskaźniki, wpływ i sankcji porady dotyczące istniejących lub pojawiających się zagrożeń dla. Te informacje mają być udostępniane we Wspólnocie zabezpieczeń i Microsoft nieustannie monitoruje kanałów analizy zagrożenia z wewnętrznych i zewnętrznych źródeł.
- **Udostępnianie sygnału**: wniosków z zespołów zabezpieczeń całej szerokiej oferty firmy Microsoft cloud lokalnego usług, serwerów i klientów punktu końcowego urządzeń i są udostępnione i analizy. 
- **Specjalistów zabezpieczeń firmy Microsoft**: trwających zaangażowania w zespołach u firmy Microsoft, które działają w pól zabezpieczeń specjalnych, takich jak forensics oraz wykrywania atakiem w sieci web.
- **Dostosowywanie wykrywania**: algorytmów są uruchamiane przed zestawów danych rzeczywistych klienta i pracowników badawczych zabezpieczeń Praca z klientami, aby sprawdzić poprawność wyników. PRAWDA i FAŁSZ wyniki dodatnie są używane do uściślić algorytmów nauki komputera.

Te połączonych działań kulminacyjny w wykryć nowych i ulepszonych funkcji, które mogą korzystać z natychmiast — istnieje akcja możesz rozpocząć.

## <a name="see-also"></a>Zobacz też
W tym dokumencie wiesz do Centrum zabezpieczeń Azure wykrywania możliwości działania. Aby dowiedzieć się więcej na temat Centrum zabezpieczeń, zobacz następujące artykuły:

- [Planowanie Centrum zabezpieczeń Azure i przewodnik](security-center-planning-and-operations-guide.md)
- [Zarządzanie i odpowiadanie na alerty zabezpieczeń w Centrum zabezpieczeń Azure](security-center-managing-and-responding-alerts.md)
- [Alerty zabezpieczeń według typu w Centrum zabezpieczeń Azure](security-center-alerts-type.md)
- [Prawidłowość zabezpieczeń monitorowania w Centrum zabezpieczeń Azure](security-center-monitoring.md) — Dowiedz się, jak monitorowanie kondycji Azure zasobów.
- [Monitorowanie rozwiązań partnerów z Centrum zabezpieczeń Azure](security-center-partner-solutions.md) — Dowiedz się, jak można monitorować stan kondycji rozwiązań partnerów.
- [Często zadawane pytania dotyczące Azure zabezpieczeń Centrum](security-center-faq.md) — znajdowanie często zadawane pytania dotyczące korzystania z usługi.
- [Blog dotyczący zabezpieczeń Azure](http://blogs.msdn.com/b/azuresecurity/) — znajdowanie blogu wpisy o Azure zabezpieczenia i zgodność.
