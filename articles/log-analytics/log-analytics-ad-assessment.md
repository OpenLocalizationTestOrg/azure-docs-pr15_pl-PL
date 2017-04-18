<properties
    pageTitle="Optymalizowanie środowiska z rozwiązanie Active Directory oceny w dzienniku analizy | Microsoft Azure"
    description="Rozwiązanie Active Directory oceny można stosować do oceniania ryzyka i kondycji w środowiskach serwer w regularnych interwałach."
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

# <a name="optimize-your-environment-with-the-active-directory-assessment-solution-in-log-analytics"></a>Optymalizowanie środowiska z rozwiązanie Active Directory oceny do analizy dziennika

Rozwiązanie Active Directory oceny można stosować do oceniania ryzyka i kondycji w środowiskach serwer w regularnych interwałach. W tym artykule pomoże Ci Instalowanie i używanie rozwiązanie, tak aby można było działań naprawczych dla potencjalnych problemów.

To rozwiązanie zawiera listę priorytetów zalecenia dotyczące infrastruktury wdrożonym serwera. Zalecenia są podzielone na cztery obszary fokusu, które ułatwiają szybkie zrozumienie ryzyka i wykonywanie akcji.

Zalecenia są oparte na wiedzy i doświadczeniach inżynierów firmy Microsoft z tysiące wizyty klienta. Każdy rekomendacji zawiera wskazówki dotyczące Dlaczego problemu może być ma znaczenia i jak wdrażać sugerowane zmiany.

Możesz wybrać fokusu obszarów, które są najbardziej istotne dla Twojej organizacji i śledzenia postępów uruchomionym środowiskiem bezpłatnych i prawidłowy ryzyka.

Po dodaniu rozwiązanie i ocena jest ukończony, podsumowanie informacje dotyczące obszarów fokusu są wyświetlane na pulpicie nawigacyjnym **Ocena AD** infrastruktury w środowisku. W poniższych sekcjach opisano sposób używania informacji na pulpicie nawigacyjnym **AD oceny** , gdzie można przeglądać i następnie wykonać czynności zalecane infrastruktury serwerowej usługi Active Directory.

![Obraz kafelka ocena SQL](./media/log-analytics-ad-assessment/ad-tile.png)

![Obraz pulpitu nawigacyjnego ocena SQL](./media/log-analytics-ad-assessment/ad-assessment.png)


## <a name="installing-and-configuring-the-solution"></a>Instalowanie i konfigurowanie rozwiązanie
Korzystając z następujących informacji, aby zainstalować i skonfigurować rozwiązania.

- Czynniki musi być zainstalowany na kontrolerach domeny, które są członkami tej domeny ma zostać obliczone.
- Rozwiązanie Active Directory oceny wymaga programu .NET Framework 4 zainstalowane na każdym komputerze, który ma agenta usługi OMS.
- Dodaj rozwiązanie Active Directory oceny do obszaru roboczego usługi OMS przy użyciu procesu opisanego w [rozwiązań Dodawanie analizy dziennika z galerii rozwiązań](log-analytics-add-solutions.md).  Nie jest wymagana żadna konfiguracja dalsze.

    >[AZURE.NOTE] Po dodaniu rozwiązanie plik AdvisorAssessment.exe zostanie dodany do serwerów czynnikami. Dane konfiguracji jest więcej, a następnie wysłany usługę w chmurze do przetwarzania. Logika jest stosowana do odbierane dane i usług w chmurze rekordów danych.

## <a name="active-directory-assessment-data-collection-details"></a>Szczegóły zbioru danych w usłudze Active ocena katalogu

Ocena Active Directory zbiera dane WMI, dane rejestru i dane dotyczące wydajności przy użyciu czynników, które są włączone.

W poniższej tabeli przedstawiono metody zbioru danych dla agentami, czy Operations Manager (SCOM) jest wymagane i jak często dane są zbierane przez agenta.

| platformy | Bezpośrednie agenta | SCOM agent | Azure miejsca do magazynowania | SCOM wymagane? | Dane agenta SCOM wysyłane za pośrednictwem grupy zarządzania | częstotliwość pobierania |
|---|---|---|---|---|---|---|
|Systemu Windows|![Tak](./media/log-analytics-ad-assessment/oms-bullet-green.png)|![Tak](./media/log-analytics-ad-assessment/oms-bullet-green.png)|![Brak](./media/log-analytics-ad-assessment/oms-bullet-red.png)|   ![Brak](./media/log-analytics-ad-assessment/oms-bullet-red.png)|![Tak](./media/log-analytics-ad-assessment/oms-bullet-green.png)| 7 dni|


## <a name="understanding-how-recommendations-are-prioritized"></a>Opis sposobu priorytety zalecenia

Co zalecenia, wyznaczona wartość wagi, która określa względną ważność zalecenia. 10 najważniejszych zalecenia są wyświetlane.

### <a name="how-weights-are-calculated"></a>Sposób obliczania grubości

Wagi są wartości zagregowanych na podstawie trzech czynników kluczy:

- *Prawdopodobieństwo* , że problemu może powodować problemy. Większe prawdopodobieństwo jest równa większe ogólny wynik dla zalecenia.

- *Wpływ* tego problemu w Twojej organizacji, jeśli powoduje problem. Większy wpływ jest równa większe ogólny wynik dla zalecenia.

- *Nakładu* wymagany do wykonania zalecenia. Wyższa nakładu równa mniejszych ogólny wynik dla zalecenia.

Waga dla każdego rekomendacji jest wyrażony w procentach wynik sumy dostępne dla każdego obszaru fokus. Na przykład jeśli rekomendacji w obszarze zabezpieczenia i zgodność fokus ma wynik 5%, wykonania tego zalecenia wzrośnie do ogólnej zabezpieczenia i zgodność wynik przez 5%.

### <a name="focus-areas"></a>Obszary fokusu

**Zabezpieczenia i zgodność** — w tym obszarze fokusu przedstawiono zalecenia dotyczące potencjalnych zagrożeniach i naruszenia, zasad firmowych i technicznych, prawem i przepisami spełnianie wymagań dotyczących.

**Dostępności i ciągłości** — w tym obszarze fokusu przedstawiono zalecenia dotyczące dostępności usługi, elastyczność infrastruktury i ochrona firm.

**Wydajność i skalowalność** — w tym obszarze fokusu przedstawiono zalecenia pomagające w Twojej organizacji infrastruktury informatycznej powiększanie, upewnij się, infrastrukturą informatyczną spełnia wymagania bieżącej i jest w stanie odpowiedzieć zmieniających się potrzeb infrastruktury.

**Uaktualnianie, wdrażania i migracji** - ten obszar fokusu zawiera zalecenia ułatwiające uaktualnienie, migrowania i wdrażanie usługi Active Directory do istniejącej infrastruktury.

### <a name="should-you-aim-to-score-100-in-every-focus-area"></a>Powinny mieć wynik 100% w każdym obszarze fokusu?

Nie musi to być. Zalecenia są oparte na wiedzy i doświadczeniach inżynierowie firmy Microsoft przez tysiące wizyty klienta. Jednak nie infrastruktury dwóch serwera są takie same, a konkretne zalecenia może być bardziej lub mniej ważne dla użytkownika. Na przykład niektóre zalecenia dotyczące zabezpieczeń może być mniej istotne maszyn wirtualnych nie są dostępne w Internecie. Niektóre zalecenia dostępność może być mniej odpowiednie dla usług, które zapewniają o niskim priorytecie spontanicznych zbieranie i raportowanie danych. Problemy z ważnych dojrzałych firm może być mniej ważne do uruchamiania. Być może zechcesz określenia, które obszary fokusu są priorytetów użytkownika, a następnie spójrz na jak wyniki zmian w czasie.

Co rekomendacji zawiera wskazówki dotyczące dlaczego jest ważne. Czy implementacji zalecenie jest odpowiednia, danej rodzaj usług informatycznych i potrzeb biznesowych w Twojej organizacji, należy użyć tych wskazówek.

## <a name="use-assessment-focus-area-recommendations"></a>Skorzystaj z rekomendacji obszar fokusu oceny

Zanim będzie można używać rozwiązanie oceny w usługi OMS, musi być zainstalowane oprogramowanie. Aby uzyskać więcej informacji na temat instalowania rozwiązań, zobacz [Dodawanie analizy dziennika rozwiązań z galerii rozwiązań](log-analytics-add-solutions.md). Po jego zainstalowaniu, możesz wyświetlić podsumowanie zalecenia przy użyciu kafelka oceny na stronie Omówienie w usługi OMS.

Wyświetlanie ocen podsumowane zgodności usługi infrastruktury, a następnie zwijania i rozwijania szczegółów do zalecenia.


### <a name="to-view-recommendations-for-a-focus-area-and-take-corrective-action"></a>Aby wyświetlić zalecenia dotyczące obszaru fokus i podejmowania działań naprawczych

1. Na stronie **Przegląd** kliknij **Ocena** infrastruktury serwerowej.
2. Na stronie **Ocena** przejrzeć informacje podsumowujące w jednym z karty warstwowy fokusu, a następnie kliknij jeden-do-wyświetlanie zalecenia dla danego obszaru fokus.
3. Na dowolnej stronie obszaru fokusu możesz wyświetlić priorytetów zalecenia w środowisku usługi. Kliknij przycisk zalecenia w obszarze **Obiekty zależne** Aby wyświetlić szczegółowe informacje o Dlaczego wprowadzone zalecenia.  
    ![Obraz oceny zalecenia](./media/log-analytics-ad-assessment/ad-focus.png)
4. Możesz wykonać sugerowane w **Akcjach sugerowanych**działań naprawczych. Jeśli element został rozwiązany, później ocen jest rejestrowany które zalecane akcje zostały pobrane i zwiększa wynik zgodności. Poprawiony elementy są wyświetlane jako **Przekazywane obiektów**.

## <a name="ignore-recommendations"></a>Ignoruj zalecenia

Jeśli masz zalecenia, które chcesz zignorować, możesz utworzyć plik tekstowy, który użyje usługi OMS aby zapobiec zalecenia być wyświetlana w wynikach oceny.

### <a name="to-identify-recommendations-that-you-will-ignore"></a>Aby zidentyfikować zalecenia, które będzie ignorować

1.  Zaloguj się do obszaru roboczego i otwórz przeszukiwanie dziennika. Dla komputerów w środowisku usługi, należy użyć następującej kwerendy do zalecenia dotyczące list, które nie powiodło się.

    ```
    Type=ADAssessmentRecommendation RecommendationResult=Failed | select  Computer, RecommendationId, Recommendation | sort  Computer
    ```

    Poniżej przedstawiono zrzut ekranu przedstawiający kwerendy wyszukiwania dziennika: ![nie powiodło się zalecenia](./media/log-analytics-ad-assessment/ad-failed-recommendations.png)

2.  Wybierz pozycję zalecenia, które chcesz zignorować. Wartości będzie używana dla RecommendationId w następnej procedurze.


### <a name="to-create-and-use-an-ignorerecommendationstxt-text-file"></a>Tworzenie i używanie plik tekstowy IgnoreRecommendations.txt

1.  Tworzenie pliku o nazwie IgnoreRecommendations.txt.
2.  Wklej lub wpisz każdego RecommendationId dla każdego rekomendacji odpowiednią analizy dziennika, aby zignorować w osobnym wierszu, a następnie zapisz i zamknij plik.
3.  Należy umieścić plik w następującym folderze na każdym komputerze, na którym chcesz usługi OMS, aby zignorować zalecenia.
    - Na komputerach z programem Microsoft monitorowania Agent (połączony bezpośrednio lub za pośrednictwem programu Operations Manager) - *Dysk_systemowy*: \Program Files\Microsoft Agent\Agent monitorowania
    - Na serwerze zarządzania programu Operations Manager — *Dysk_systemowy*: \Program Files\Microsoft Manager\Server R2\Operations 2012 Centrum systemu

### <a name="to-verify-that-recommendations-are-ignored"></a>Aby zweryfikować, że są ignorowane zalecenia

Po Następne zaplanowane oceny zostanie uruchomiona, domyślnie co 7 dni, określonego zalecenia są oznaczane *zignorowane* i nie będą wyświetlane na pulpicie nawigacyjnym oceny.

1. Za pomocą następujących kwerend wyszukiwania dziennika do wyświetlania listy do zaleceń ignorowane.

    ```
    Type=ADAssessmentRecommendation RecommendationResult=Ignored | select  Computer, RecommendationId, Recommendation | sort  Computer
    ```

2.  Jeśli po pewnym czasie chcesz wyświetlić zalecenia zignorowane, Usuń wszystkie pliki IgnoreRecommendations.txt lub RecommendationIDs można usunąć z nich.

## <a name="ad-assessment-solutions-faq"></a>AD ocena rozwiązań — często zadawane pytania

*Jak często ocenę czy działa?*
- Ocena uruchamia co 7 dni.

*Czy istnieje sposób, aby skonfigurować częstotliwość uruchamiania oceny?*
- W tym czasie.

*Jeśli innego serwera w celu stwierdzenia po został dodany rozwiązanie oceny, jej oceni?*
- Tak, gdy okaże się, że jest ustalana na to, co 7 dni.

*Jeśli serwer jest zlikwidować, gdy zostanie on usunięty z oceny?*
- Jeśli serwer nie przesyłanie danych przez 3 tygodnie, zostanie usunięty.

*Co to jest nazwa procesu, który ma zbierania danych?*
- AdvisorAssessment.exe

*Jak długo trwa zbierania danych?*
- W zbiorze danych rzeczywistych na serwerze ma około 1 godzina. Może trwać dłużej na serwerach, które mają dużej liczby serwerów usługi Active Directory.

*Jakiego rodzaju dane są zbierane?*
- Zbierane są następujące typy danych:
    - WMI
    - Rejestru
    - Liczniki wydajności

*Czy istnieje możliwość skonfigurowania, gdy dane są zbierane?*
- W tym czasie.

*Dlaczego są wyświetlane tylko zalecenia 10 głównych?*
- Zamiast daje to utrudnione Pełna lista zadań, zaleca się, że skoncentrowanie się na najpierw adresowania priorytetów zalecenia. Po zajmij się nimi, dodatkowe zalecenia staną się dostępne. Jeśli wolisz wyświetlić listę szczegółowe, możesz wyświetlić wszystkie zalecenia w wyszukiwaniu dziennika.

*Czy istnieje sposób, aby zignorować zalecenie?*
- Tak, zobacz [zalecenia Ignoruj](#ignore-recommendations) sekcji powyżej.


## <a name="next-steps"></a>Następne kroki

- Umożliwia wyświetlanie szczegółowych danych AD oceny i zalecenia [wyszukiwania dziennika analizy dziennika](log-analytics-log-searches.md) .
