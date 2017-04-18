<properties
    pageTitle="Dodawanie rozwiązań analizy dziennika z galerii rozwiązań | Microsoft Azure"
    description="Rozwiązania analizy dziennika to zbiór logiczny, wizualizacji i uzyskiwanie danych reguł zapewniające metryki obraca wokół obszaru konkretnego problemu."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>

# <a name="add-log-analytics-solutions-from-the-solutions-gallery"></a>Dodawanie rozwiązań analizy dziennika z galerii rozwiązań

Rozwiązania analizy dziennika są to zbiór **logiczny**, **wizualizacji** i **reguł nabycia danych** dostarczających metryki obraca wokół obszaru konkretnego problemu. Ten artykuł rozwiązań list obsługiwane analizy dziennika i opisano, jak dodawać i usuwać je przy użyciu galerii rozwiązań.

Rozwiązania zezwolić szczegółowego wniosków do:

- Pomoc zbadać i szybciej rozwiązać problemy operacyjne
- zbieranie i przeniesionym różnych rodzajów danych komputera
- ułatwiają mogą zarządzać działań, takich jak planowanie wydajności, raportów o stanie poprawki i inspekcja zabezpieczeń.


>[AZURE.NOTE] Analizy dziennika zawiera funkcję wyszukiwania dziennika, więc nie musisz zainstalować rozwiązanie, aby ją włączyć. Jednak możesz uzyskać wizualizacji danych, sugerowane wyszukiwania i wniosków, dodając rozwiązań z galerii rozwiązań.

Po dodaniu rozwiązanie, dane są zebrane z serwerami w infrastrukturze i wysyłane do usługę. Przetwarzanie przez usługi OMS usługi zwykle trwa kilka minut na godzinę. Gdy usługa przetwarza dane, możesz wyświetlić go w usługi OMS.

Możesz łatwo usunąć rozwiązanie, gdy nie jest już potrzebna. Po usunięciu rozwiązanie, wraz z danymi nie są wysyłane do usługi OMS, co może zmniejszyć ilość danych używanych przez dzienny przydziału, jeśli istnieje.


## <a name="solutions-supported-by-the-microsoft-monitoring-agent"></a>Rozwiązania obsługiwane przez program Microsoft Agent monitorowania

W tej chwili serwerów połączonych z usługi OMS przy użyciu programu Microsoft Agent monitorowania służy najczęściej dostępne w tym rozwiązań:

- Ocena usługi Active Directory
- Zarządzania alertami (bez SCOM alertów)
- Ochrony przed złośliwym oprogramowaniem
- Śledzenie zmian
- Zabezpieczenia
- Ocena SQL
- Aktualizacje systemu

Jednak następujące rozwiązania jest *nie* obsługiwane przez program Microsoft Agent monitorowania i wymaganie agenta System Center Operations Manager (SCOM).

- Zarządzania alertami (w tym alerty SCOM)
- Zarządzanie wydajności
- Ocena konfiguracji

Aby uzyskać informacje o nawiązywaniu połączeń z agentem SCOM z dziennika analizy odwołują się do [Łączenia analizy dziennika programu Operations Manager](log-analytics-om-agents.md) .

### <a name="to-add-a-solution-using-the-solutions-gallery"></a>Aby dodać rozwiązanie przy użyciu galerii rozwiązań

1. Na stronie Przegląd w usługi OMS kliknij **Galerii rozwiązań** .    
    ![Galeria rozwiązań](./media/log-analytics-add-solutions/sol-gallery.png)
2. Na stronie galerii rozwiązań usługi OMS informacje na temat każdego z rozwiązań dostępne. Kliknij nazwę rozwiązanie, które chcesz dodać do usługi OMS.
3. Szczegółowe informacje na temat rozwiązanie jest wyświetlany na stronie wybranego rozwiązania. Kliknij przycisk **Dodaj**.
4. Nowy Kafelek rozwiązanie, które zostały dodane, pojawi się na przegląd strony w usługi OMS i można rozpoczęcie korzystania z niego po przetworzeniu danych przez usługę.

## <a name="to-configure-solutions"></a>Aby skonfigurować rozwiązań
1. Musisz skonfigurować kilka rozwiązań. Na przykład musisz skonfigurować automatyzacji, odzyskiwanie witryny Azure i kopii zapasowej, zanim będzie można ich używać.
2. Dla każdego z tych rozwiązań kliknij jego kafelka na stronie Przegląd.  
    ![Konfigurowanie rozwiązania](./media/log-analytics-add-solutions/configure-additional.png)
3. Następnie konfigurowanie rozwiązania niezbędne informacje, a następnie kliknij przycisk **Zapisz**.  
    ![Konfigurowanie rozwiązania](./media/log-analytics-add-solutions/configure.png)

### <a name="to-remove-a-solution-using-the-solutions-gallery"></a>Aby usunąć rozwiązanie przy użyciu galerii rozwiązań

1. Na stronie Przegląd w usługi OMS kliknij kafelka **Ustawienia** .
2. Na stronie Ustawienia na karcie rozwiązań kliknij przycisk **Usuń** dla rozwiązanie, które chcesz usunąć.
3. W oknie dialogowym potwierdzenia kliknij przycisk **Tak,** Aby usunąć rozwiązanie.

## <a name="data-collection-details-for-oms-features-and-solutions"></a>Szczegóły zbioru danych dla funkcji usługi OMS i rozwiązania

W poniższej tabeli przedstawiono metody zbioru danych i inne szczegóły dotyczące sposobu dane są zbierane dla usługi OMS funkcjach i rozwiązaniach. Bezpośrednie czynników i czynników SCOM są takie same, jednak bezpośredni agenta zawiera dodatkowe funkcje, aby umożliwić nawiązywanie połączenia z obszaru roboczego usługi OMS i jest rozsyłana za pośrednictwem serwera proxy. Jeśli używasz agenta SCOM, musi być przeprowadzone jako agenta usługi OMS można komunikować się z usługi OMS. Agenci SCOM w tej tabeli są usługi OMS czynników, które są połączone z SCOM. Aby uzyskać informacje o nawiązywaniu połączeń z istniejącym środowiskiem SCOM z usługi OMS, zobacz [Łączenie programu Operations Manager do analizy dziennika](log-analytics-om-agents.md) .

>[AZURE.NOTE] Typ agenta, którego używasz, określa, jak dane są przesyłane do usługi OMS, z następujących warunków:

- Możesz użyć bezpośredni agenta lub agenta dołączone SCOM usługi OMS.
- Jeśli SCOM jest wymagane, danych agenta SCOM rozwiązania są zawsze wysyłane do usługi OMS przy użyciu grupy zarządzania SCOM. Ponadto jeśli SCOM jest wymagane, agentem SCOM jest używana przez rozwiązanie.
- Gdy SCOM nie jest wymagane i w tabeli pokazano, że dane agenta SCOM jest wysyłana do usługi OMS przy użyciu grupy zarządzania, następnie SCOM agenta dane są zawsze wysyłane do usługi OMS przy użyciu grup zarządzania. Bezpośrednie pomijać grupie zarządzania i wysłać swoje dane bezpośrednio do usługi OMS.
- Gdy SCOM agenta dane nie są wysyłane za pomocą grupy zarządzania, następnie dane są wysyłane bezpośrednio do usługi OMS — pominięcie grupie zarządzania.


|Typ danych| platformy | Bezpośrednie agenta | SCOM agent | Azure miejsca do magazynowania | SCOM wymagane? | Dane agenta SCOM wysyłane za pośrednictwem grupy zarządzania | częstotliwość pobierania |
|---|---|---|---|---|---|---|---|
|Ocena AD|Systemu Windows|![Tak](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Tak](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Tak](./media/log-analytics-add-solutions/oms-bullet-green.png)|  7 dni|
|Stan replikacji AD|Systemu Windows|![Tak](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Tak](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)|5 dni|
|Alerty (Nagios)|Linux|![Tak](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)|na nadejście|
|Alerty (Zabbix)|Linux|![Tak](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)|1 minuta|
|Alerty (kierownik)|Systemu Windows|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Tak](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Tak](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Tak](./media/log-analytics-add-solutions/oms-bullet-green.png)|3 minut|
|Ochrony przed złośliwym oprogramowaniem|Systemu Windows|![Tak](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Tak](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Tak](./media/log-analytics-add-solutions/oms-bullet-green.png)| co godzina|
|Zarządzanie wydajności|Systemu Windows|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Tak](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Tak](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Tak](./media/log-analytics-add-solutions/oms-bullet-green.png)| co godzina|
|Śledzenie zmian|Systemu Windows|![Tak](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Tak](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Tak](./media/log-analytics-add-solutions/oms-bullet-green.png)| co godzina|
|Śledzenie zmian|Linux|![Tak](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)|co godzina|
|Ocena konfiguracji (Advisor starsze)|Systemu Windows|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Tak](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Tak](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Tak](./media/log-analytics-add-solutions/oms-bullet-green.png)| dwa razy dziennie|
|FUNKCJA ETW|Systemu Windows|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Tak](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)|5 minut|
|Dzienniki programu IIS|Systemu Windows|![Tak](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Tak](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Tak](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)|5 minut|
|Magazynami najważniejszych|Systemu Windows|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Tak](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)|10 minut|
|Sieć aplikacji bram|Systemu Windows|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Tak](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)|10 minut|
|Grupy zabezpieczeń sieci|Systemu Windows|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Tak](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)|10 minut|
|Usługi Office 365|Systemu Windows|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)|powiadomienia o|
|Liczniki wydajności|Systemu Windows|![Tak](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Tak](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)|zgodnie z harmonogramem, minimum 10 sekund|
|Liczniki wydajności|Linux|![Tak](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)|zgodnie z harmonogramem, minimum 10 sekund|
|Usługa tkaninie|Systemu Windows|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Tak](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)|5 minut|
|Ocena SQL|Systemu Windows|![Tak](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Tak](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Tak](./media/log-analytics-add-solutions/oms-bullet-green.png)| 7 dni|
|SurfaceHub|Systemu Windows|![Tak](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)|na nadejście|
|SYSLOG|Linux|![Tak](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)|z magazynu Azure: 10 minut; agenta: na nadejście|
|Aktualizacje systemu|Systemu Windows|![Tak](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Tak](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Tak](./media/log-analytics-add-solutions/oms-bullet-green.png)| co najmniej 2 razy na dzień i 15 minut po zainstalowaniu aktualizacji|
|Dzienniki zdarzeń zabezpieczeń systemu Windows|Systemu Windows|![Tak](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Tak](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Tak](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)| do przechowywania Azure: 10 minut; dla agenta: na nadejście|
|Dzienniki zapory systemu Windows|Systemu Windows|![Tak](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Tak](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)| na nadejście|
|Dzienniki zdarzeń systemu Windows|Systemu Windows|![Tak](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Tak](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Tak](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Tak](./media/log-analytics-add-solutions/oms-bullet-green.png)| do przechowywania Azure: 1 min; dla agenta: na nadejście|
|Przewodowy danych|Windows (2012 R2 / 8.1 lub nowszy)|![Tak](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Tak](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Brak](./media/log-analytics-add-solutions/oms-bullet-red.png)| co minutę|

## <a name="log-analytics-preview-solutions-and-features"></a>Funkcje i rozwiązań Podgląd analizy dziennika

Uruchamiając usługę i wskazówek devops możemy pod kątem współpracy z klientami opracowywaniu funkcjach i rozwiązaniach.

Podczas podglądu prywatne możemy przekazać do najwcześniejsze stosowania funkcji lub rozwiązanie do uzyskania opinii i ulepszania niewielkiej grupy klientów programu access. Ten szybkie wykonanie ma minimalny funkcji i możliwości działania.

Celem jest szybko wypróbować rozwiązań, więc możemy znaleźć, co działa, a czego nie działa. Firma Microsoft iterację ten proces aż opinii klientów prywatnych podglądu zostanie wyświetlona informacja, nam że Służymy publicznej podglądu.

Podczas publicznej podglądu udzielamy funkcji lub rozwiązanie dostępne dla wszystkich użytkowników uzyskiwanie opinii więcej i sprawdź poprawność naszych skalowania i efektywności. Podczas tej fazy:

- Funkcje podglądu pojawi się na karcie Ustawienia i mogą być włączone dla wszystkich użytkowników
- Można dodawać za pośrednictwem galerii rozwiązań Podgląd lub za pomocą skryptu opublikowanych

### <a name="what-should-i-know-about-preview-features-and-solutions"></a>Co należy wiedzieć o Podgląd funkcjach i rozwiązaniach?

Emocje o nowych funkcjach i rozwiązaniach i firma Microsoft lubisz pracy z użytkownikiem w celu ich rozwijania.

Podgląd funkcjach i rozwiązaniach nie są odpowiednie dla wszystkich osób, więc przed pytaniem dołączyć do prywatnych Podgląd i włączanie publicznej Podgląd upewnij się, że OK pracujesz z coś, co w fazie projektowania.

Podczas włączania funkcji podglądu za pośrednictwem portalu, które będzie wyświetlane ostrzeżenie o przypominający, że ta funkcja jest w podglądzie.

#### <a name="for-both-private-and-public-preview"></a>Aby uzyskać podgląd zarówno *prywatne* i *publiczne*

Poniższe informacje dotyczą publicznych i prywatnych podglądu:

- Co może nie zawsze działać poprawnie.
  - Zakres problemów przed pomocniczych określenia dokuczliwości za pośrednictwem na inną nie działa na wszystkich
- Istnieje możliwość Podgląd, aby mieć negatywny wpływ na systemu / środowiska
  - Firma Microsoft należy unikać ujemne co dzieje systemów korzystasz z usługi OMS, ale czasami nieoczekiwane wydarzenia
- Utratą danych / może spowodować uszkodzenie
- Firma Microsoft może wymagać zbieranie dzienników diagnostycznych lub inne dane, aby ułatwić rozwiązywanie problemów
- Funkcja lub rozwiązanie, mogą zostać usunięte (tymczasowo lub na stałe)
  - Według naszych learnings podczas podglądu, firma Microsoft może być nie Zwolnij funkcji lub rozwiązanie
- Podgląd może nie działać lub może być nie badany we wszystkich konfiguracjach, a firma Microsoft może ograniczyć:
  - Systemy operacyjne, które mogą być używane (np. funkcja może dotyczyć Linux znajduje się w podglądzie)
  - Typ agenta (MMA, SCOM), który może być używany (np. funkcja może nie działać z SCOM w wersji preview)  
- Podgląd rozwiązań i funkcje nie są objęte umowa dotycząca poziomu usług
- Użycie funkcji podglądu będzie powodowało opłaty za zużycie
- Funkcje i możliwości, że potrzebujesz przez funkcję / rozwiązanie użyteczne mogą zostać utracone lub niekompletna
- Funkcje / rozwiązań mogą nie być dostępne we wszystkich regionach
- Funkcje / rozwiązań mogą nie być zlokalizowane
- Funkcje / rozwiązań może być ograniczenie liczby odbiorców lub urządzenia, które mogą jej używać
- Może być konieczne do wykonywania konfiguracji i włączyć funkcję/rozwiązanie za pomocą skryptów
- Interfejs użytkownika (UI) będzie niepełne oraz mogą ulec zmianie z dnia na dzień
- Podgląd publicznej może nie być odpowiednie dla produkcji / krytyczne systemów

#### <a name="for-private-preview"></a>Aby uzyskać podgląd *prywatnych*

Oprócz powyższych elementów Oto specyficzne dla prywatnych podglądu:

- Oczekujemy Cię, aby przekazać nam o opinie dotyczące środowiska pracy użytkownika, dzięki czemu firma Microsoft można wprowadzić funkcję/rozwiązanie lepiej
- Firma Microsoft może skontaktować się z Tobą opinie przy użyciu ankiety, rozmów telefonicznych lub wiadomości e-mail
- Elementy zawsze będą działać poprawnie
- Firma Microsoft może wymagać bez ujawniania umowy (NDA) uczestnictwa lub może obejmować poufne zawartości
  - Przed blogu, tweeting lub w inny sposób komunikowania się z trzecich Sprawdź, czy z programu odpowiedzialną za podglądu zrozumienie ograniczeń dotyczących ujawnienia
- Nie będą uruchamiane produkcji i krytyczne systemów


### <a name="how-do-i-get-access-to-private-preview-features-and-solutions"></a>Jak uzyskać dostęp do funkcji podglądu prywatne i rozwiązania?

Zapraszamy klientom prywatne podglądy przez kilka różnych sposobów, w zależności od wersji preview.

- Odpowiadanie miesięczna Ankieta klienta i przekazanie uprawnień do Flaga monitująca Ci zwiększa szanse zapraszany prywatne Podgląd.
- Zespół obsługi klienta firmy Microsoft mogą wyznaczyć możesz.
- Możesz utworzyć konto oparte na szczegóły opublikowany w serwisie twitter [msopsmgmt](https://twitter.com/msopsmgmt)
- Możesz utworzyć konto oparte na szczegóły zdarzenia społeczności udostępnione — przeglądanie abyśmy spełniają zwiększa konferencji w społeczności online.


## <a name="next-steps"></a>Następne kroki

- [Dzienniki wyszukiwania](log-analytics-log-searches.md) , aby wyświetlić szczegółowe informacje zgromadzone przez rozwiązań.
