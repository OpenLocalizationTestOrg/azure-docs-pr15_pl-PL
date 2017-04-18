<properties
   pageTitle="Centrum zabezpieczeń Azure zabezpieczania danych | Microsoft Azure"
   description="W tym dokumencie omówiono sposobu zarządzania i w Centrum zabezpieczeń Azure danych."
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
   ms.date="10/25/2016"
   ms.author="yurid"/>

# <a name="azure-security-center-data-security"></a>Centrum zabezpieczeń Azure zabezpieczania danych
Aby pomóc klientom zapobiegania, wykrywania i odpowiadanie na zagrożenia, Centrum zabezpieczeń Azure gromadzi i przetwarza dane na temat usługi Azure zasobów, w tym informacje o konfiguracji, metadane, dzienniki zdarzeń ulec awarii pliki zrzutu i nie tylko. Udzielamy silnych zobowiązań ochrony prywatności i zabezpieczanie danych. Microsoft jest zgodny ściśle zabezpieczenia i zgodność z wytycznymi — z kodowania do działania usługi. 

W tym artykule wyjaśniono, jak zarządzać i w Centrum zabezpieczeń Azure danych.

## <a name="data-sources"></a>Źródła danych

Centrum zabezpieczeń Azure analizy danych z następujących źródeł:

- Usługi Azure: Odczytuje informacje o konfiguracji usług Azure wdrożono przez komunikowania się z dostawcą zasobów tej usługi.
- Ruch sieciowy: Odczyt pobrane sieci metadanych ruchu z infrastruktury firmy Microsoft, takich jak źródłowego i docelowego adresu IP/portu, rozmiar pakietu i protokół sieci.
- Rozwiązania partnerów: Zbiera alertów zabezpieczeń z rozwiązań partnerów zintegrowane, takie jak zapory i ochrony przed złośliwym oprogramowaniem rozwiązań. Te dane są przechowywane w magazynie Centrum zabezpieczeń Azure, obecnie znajduje się w Stanach Zjednoczonych.
- Maszyn wirtualnych: Centrum zabezpieczeń Azure może zbierać informacje o konfiguracji i informacje dotyczące zdarzenia zabezpieczeń, takich jak zdarzeń systemu Windows i inspekcji dzienniki, usług IIS dzienniki syslog wiadomości i pliki zrzutu awaryjnego z maszyn wirtualnych przy użyciu agentów zbioru danych. Zobacz sekcję "Zarządzanie zbierania danych" poniżej, aby uzyskać dodatkowe informacje.  

Ponadto informacje dotyczące alertów zabezpieczeń, zalecenia i stanu kondycji zabezpieczeń są przechowywane w magazynie Centrum zabezpieczeń Azure obecnie znajduje się w Stanach Zjednoczonych. Te informacje mogą zawierać informacje o konfiguracji powiązanych i zdarzeń zabezpieczeń zbierane z maszyn wirtualnych, aby udostępnić alertu zabezpieczeń, zalecenia lub stanu kondycji zabezpieczeń.

## <a name="data-protection"></a>Ochrona danych

**Podziału danych**: dane są przechowywane logicznie oddzielnie dla każdego składnika w usłudze. Wszystkie dane są oznakowane dla organizacji. Znakowanie ten będzie nadal występował w całym cyklu życia danych, a została wprowadzona w każdej warstwie usługi. Ponadto dane zebrane z maszyn wirtualnych znajduje się w konta na potrzeby miejsca do magazynowania.

**Dostęp do danych**: w celu zapewnienia zalecenia dotyczące zabezpieczeń i badanie potencjalnych zagrożeniach, personelowi firmy Microsoft mogą uzyskać dostęp do informacji zbieranych lub analizowane przez usługi Azure, w tym pliki zrzutu awaryjnego. Pliki zrzutu awaryjnego i proces tworzenia zdarzeń przez przypadek może zawierać dane klientów i danych osobistych w środowisku maszyn wirtualnych systemu. Firma Microsoft zgodny [Postanowienia dotyczące usług Online firmy Microsoft](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31) i [Zasady zachowania poufności informacji](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx), których stan firmy Microsoft będzie nie korzystanie z danych klienta lub uzyskania informacji z niego w celach komercyjnych ogłoszeń i podobne. Firma Microsoft tylko przy użyciu klienta danych stosownie do potrzeb umożliwiają usługi Azure, w tym celów zgodny z ich dostarczania tych usług. Możesz zachować wszystkie prawa do danych klientów.

**Używanie danych**: Firma Microsoft używa wzorców i analizy zagrożenia widoczne przez kilka dzierżaw rozszerza możliwości naszych zapobiegania i wykrywania; Firma Microsoft zrobić zgodnie z zobowiązań prywatności opisanych w nasze [Zasady zachowania poufności informacji](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx).

**Lokalizacja danych**: określono konta miejsca do magazynowania dla każdego regionu, w której działa maszyn wirtualnych. Pozwala na przechowywanie danych w tym samym regionie jako maszyny wirtualnej, w którym dane są zbierane. Tych danych, w tym pliki zrzutu awaryjnego, będą stale przechowywane na Twoim koncie miejsca do magazynowania. Usługa przechowuje również informacji na temat alertów zabezpieczeń, w tym alerty z rozwiązań partnerów zintegrowane, zalecenia i stanu kondycji zabezpieczeń w Centrum zabezpieczeń Azure miejscem do magazynowania, obecnie znajduje się w Stanach Zjednoczonych.

## <a name="managing-data-collection-from-virtual-machines"></a>Zarządzanie zbierania danych z maszyn wirtualnych

Jeśli chcesz umożliwić Centrum zabezpieczeń Azure zbierania danych jest włączona dla każdej subskrypcji. Możesz wyłączyć funkcję zbioru danych w sekcji "Zasady zabezpieczeń" Pulpit nawigacyjny Centrum zabezpieczeń Azure. Po włączeniu zbierania danych przepisów Centrum zabezpieczeń Azure agenta monitorowania Azure na wszystkich istniejących obsługiwane maszyn wirtualnych i nowych plików, które zostały utworzone. Monitorowanie zabezpieczeń Azure rozszerzenia skanowanie w poszukiwaniu różnych zabezpieczeń konfiguracji i zdarzeń do [Śledzenia zdarzeń dla systemu Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx) (ETW) służy do śledzenia. Ponadto system operacyjny będzie wywołania zdarzeń dziennika zdarzeń w trakcie uruchamiania komputera. Przykłady takich danych: typ systemu operacyjnego i wersji operacyjnym dzienniki systemu (dzienniki zdarzeń systemu Windows), systemem procesów, nazwa komputera, adresy IP, rejestrowane w użytkownika i identyfikator dzierżawy. Agent monitorowania Azure odczytuje wpisy dziennika zdarzeń i ETW służy do śledzenia i kopiuje je do swojego konta miejsca do magazynowania na potrzeby analizy. 

Konto miejsca do magazynowania jest określona dla każdego regionu, w którym masz maszyn wirtualnych z systemem, w którym dane zbierane w środowisku maszyn wirtualnych systemu w tym samym regionie jest przechowywany. Ułatwia umożliwiające zachowanie danych w tym samym obszarze geograficznym do celów suwerenności danych i prywatności. W sekcji "Zasady zabezpieczeń" Pulpit nawigacyjny Centrum zabezpieczeń Azure można skonfigurować konta miejsca do magazynowania dla każdego regionu.

Agent monitorowania Azure również skopiowanie pliki zrzutu awaryjnego do swojego konta miejsca do magazynowania.  Centrum zabezpieczeń Azure zbiera tymczasowych kopii plików zrzutu awaryjnego i analizuje je dowodów prób wykorzystać i pomyślnego kompromisów.  Centrum zabezpieczeń Azure wykonuje tej analizy w ramach tego samego regionu geograficznego jako konto miejsca do magazynowania i usunięcie tymczasowych kopii po zakończeniu analizy.

Możesz wyłączyć zbieranie danych w środowisku maszyn wirtualnych systemu w dowolnym czasie, co spowoduje usunięcie czynników monitorowania poprzednio zainstalowane przez Centrum zabezpieczeń Azure.


## <a name="see-also"></a>Zobacz też

W tym dokumencie wiesz, jak zarządzać i w Centrum zabezpieczeń Azure danych. Aby dowiedzieć się więcej na temat Centrum zabezpieczeń Azure, zobacz:

- [Planowanie Centrum zabezpieczeń Azure i przewodnik](security-center-planning-and-operations-guide.md) — Dowiedz się, jak planowanie i opis zagadnienia projektowe przyjęcie Centrum zabezpieczeń Azure.
- [Prawidłowość zabezpieczeń monitorowania w Centrum zabezpieczeń Azure](security-center-monitoring.md) — Dowiedz się, jak monitorowanie kondycji Azure zasobów
- [Zarządzanie notatkami i odpowiadanie na alerty zabezpieczeń w Centrum zabezpieczeń Azure](security-center-managing-and-responding-alerts.md) — Dowiedz się, jak zarządzać i odpowiadanie na alerty zabezpieczeń
- [Monitorowanie rozwiązań partnerów z Centrum zabezpieczeń Azure](security-center-partner-solutions.md) — Dowiedz się, jak można monitorować stan kondycji rozwiązań partnerów.
- [Często zadawane pytania dotyczące Azure zabezpieczeń Centrum](security-center-faq.md) — znajdowanie często zadawane pytania dotyczące korzystania z usługi
- [Blog dotyczący zabezpieczeń Azure](http://blogs.msdn.com/b/azuresecurity/) — znajdowanie blogu wpisy o Azure zabezpieczenia i zgodność
