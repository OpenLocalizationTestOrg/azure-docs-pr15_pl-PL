<properties 
   pageTitle="Alerty w dzienniku analizy | Microsoft Azure"
   description="Alerty w dzienniku analizy identyfikowanie ważne informacje zawarte w repozytorium usługi OMS i można z wyprzedzeniem informujący o problemy lub wywołania akcje, aby spróbować usunąć je.  W tym artykule opisano, jak utworzyć regułę alertu i szczegóły innych działań, które można wykonać."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags 
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/22/2016"
   ms.author="bwren" />

# <a name="alerts-in-log-analytics"></a>Alerty w dzienniku analizy

Alerty w dzienniku analizy identyfikowanie ważne informacje zawarte w repozytorium usługi OMS.  Reguły alertów automatycznie uruchamiać wyszukiwania dziennika zgodnie z harmonogramem i utworzyć alert rekord, jeśli wyniki są zgodne z kryteriami.  Reguły następnie można automatycznie uruchomić jedną lub więcej akcji z wyprzedzeniem informujący o alercie lub wywołania inny proces.   

![Alerty analizy dziennika](media/log-analytics-alerts/overview.png)

## <a name="creating-an-alert-rule"></a>Tworzenie reguły alertu
Aby utworzyć regułę alertu, rozpoczyna się od utworzenia dziennika wyszukiwanie rekordów, które należy wywołać alert.  Przycisk **Alert** będzie dostępny, można utworzyć i skonfigurować regułę alertu.

1.  Na stronie Omówienie usługi OMS kliknij przycisk **Wyszukaj dziennika**.
2.  Utwórz nową kwerendę wyszukiwania dziennika albo wybierz pozycję Wyszukaj zapisany dziennik. 
3.  Kliknij pozycję **Alert** w górnej części strony Aby otworzyć ekran **Dodaj regułę alertu** .
4. Można znaleźć w poniższych tabelach szczegółowych informacji na temat opcji, aby skonfigurować alert.
5. Po udostępnieniu okna czasu dla reguły alertu pojawi się liczba istniejących rekordów, które spełniają kryteria wyszukiwania dla tego przedział czasu.  To mogą ułatwić podjęcie częstotliwości, które dają liczbę wyników, które można się spodziewać.
4.  Kliknij przycisk **Zapisz** do wykonania reguły alertu.  Zostanie uruchomiony, natychmiastowe uruchamianie.


![Dodawanie reguły alertu](media/log-analytics-alerts/add-alert-rule.png)

| Właściwość | Opis |
|:--|:--|
| **Informacje alertu** | |
| Nazwa |  Unikatową nazwę identyfikującą reguły alertu. |
| Ważności | Waga alertu, który jest tworzona przez tę regułę. |
| Kwerendy wyszukiwania | Wybierz opcję **Użyj bieżącej kwerendy wyszukiwania** , użyj bieżącej kwerendy lub wybierz istniejące zapisane wyszukiwanie z listy.  Składnia kwerendy znajduje się w polu tekstowym, w którym można modyfikować ją w razie potrzeby.  |
| Przedział czasu | Określa zakres czasu dla zapytania.  Kwerenda zwraca tylko te rekordy, które zostały utworzone w tym zakresie bieżącej godziny.  Może to być dowolna wartość od 5 minut do 24 godzin.  Należy większe niż lub równe częstotliwość alertów.  <br><br> Na przykład jeśli z przedziału czasu jest ustawiona na 60 minut i uruchomieniu kwerendy godzinie 13:15, zostaną zwrócone tylko te rekordy, które utworzone między 12:15 PM i 1:15 PM. |
| **Harmonogram** |     
| Progowa. | Kryteria dla kiedy utworzyć alert.  Alert jest tworzony, jeśli liczba rekordów zwróconych przez kwerendę tego kryteria. |
| Częstotliwość alertów | Określa, jak często należy uruchomić kwerendę.  Może mieć dowolną wartość od 5 minut do 24 godzin.  Powinna być równa lub mniejsza niż z przedziału czasu. |
| Pomiń alerty | Po włączeniu zwalczaniu dla reguły alertu akcje reguły są wyłączone dla określonej długości okresu po utworzeniu nowy alert.  Reguła jest nadal uruchomiony i utworzy alert rekordów, jeśli kryteria są spełnione.  To aby możliwe było czas, aby rozwiązać ten problem, bez uruchamiania zduplikowanych akcje. |
| **Akcje** | |
| Powiadomienia pocztą e-mail | Określ **Tak** , jeśli chcesz, aby wiadomości e-mail były wysyłane wyzwolenia alertu. |
| Temat    | Temat w wiadomości e-mail.  Nie można zmodyfikować treści wiadomości. |
| Adresaci | Adresy wszystkich adresatów wiadomości e-mail.  Jeśli użytkownik określi więcej niż jednego adresu, oddzielając adresy średnikami (;). |
| Webhook | Umożliwia określenie **Tak** , jeśli chcesz nawiązać połączenie webhook wyzwolenia alertu. |
| Adres URL Webhook | Adres URL webhook. |
| Dołączanie niestandardowych ładunku JSON | Zaznacz tę opcję zamienić ładunku domyślne ładunku niestandardowe. |
| Wprowadź swojego niestandardowego ładunku JSON | Niestandardowe ładunek dla webhook.  Zobacz poprzedniej sekcji podano szczegóły. |
| Działań aranżacji | Określ **Tak** rozpocząć działań aranżacji automatyzacji Azure wyzwolenia alertu. |
| Wybieranie działań aranżacji | Wybierz pozycję działań aranżacji zacząć od runbooks na koncie automatyzacji skonfigurowany w rozwiązaniu automatyzacji. |
| Uruchom na | Wybierz pozycję **Azure** do uruchomienia działań aranżacji w chmurze Azure.  Wybierz pozycję **Hybrydowych pracownika** do uruchomienia działań aranżacji na [Hybrydowych działań aranżacji pracownika](..\automation\automation-hybrid-runbook-worker.md) w środowisku lokalnym. |


## <a name="manage-alert-rules"></a>Zarządzaj regułami alertów
Możesz wyświetlić listę wszystkich reguł alertów w menu **alerty** w dzienniku analizy **Ustawienia**.  

![Zarządzanie alertami](./media/log-analytics-alerts/configure.png)

1. W konsoli usługi OMS wybierz kafelka **Ustawienia** .
2. Wybierz pozycję **alerty**.

W tym widoku, można wykonać kilka akcji.

- Wyłączanie reguły, wybierając **Wyłączanie** obok niej.
- Edytowanie reguły alertu, klikając ikonę ołówka obok niej.
- Usuwanie reguły alertu, klikając ikonę **X** obok niej. 


## <a name="setting-time-windows"></a>Ustawianie godziny w systemie windows 

### <a name="event-alerts"></a>Alerty zdarzenia

Zdarzenia zawierają źródeł danych, takich jak dzienniki zdarzeń systemu Windows, Syslog, i rejestruje niestandardowe.  Być może zechcesz utworzyć alert, gdy zdarzenie określonego błędu zostanie utworzony, lub gdy wiele zdarzeń błędów są tworzone w oknie określonego czasu.

Alert na pojedyncze zdarzenie, ustaw liczbę wyników na wartość większą niż 0 i częstotliwości i przedział czasu 5 minut.  Które będzie uruchomić kwerendę co 5 minut i sprawdzanie występowania pojedynczego zdarzenia, który został utworzony od czasu ostatniego uruchomienia kwerendy.  Częstotliwość dłużej może opóźnić czasu między wydarzenia są zbierane i alert tworzony.

Niektóre aplikacje mogą rejestrować rzadkie błąd, który nie powinny niekoniecznie podnieść alertu.  Na przykład aplikacja może ponowić próbę procesu, który utworzył zdarzenie błędu i powiodła się następnym razem.  W takim przypadku nie można utworzyć alert, chyba że wiele zdarzeń są tworzone w oknie określonego czasu.  

W niektórych przypadkach można utworzyć alert w przypadku braku zdarzenia.  Na przykład procesu mogą rejestrować zdarzeniami standardowymi oznaczającą, że działa poprawnie.  Jeśli nie rejestruje jedno z tych zdarzeń w oknie określonego czasu, powinny być tworzone alertu.  W tym przypadku chcesz ustawić próg *mniejsza niż*1.

### <a name="performance-alerts"></a>Alerty wydajności

[Dane dotyczące wydajności](log-analytics-data-sources-performance-counters.md) są przechowywane jako rekordy w repozytorium usługi OMS podobne do zdarzenia.  Wartość z każdego rekordu jest średnią mierzone w poprzednich 30 minut.  Jeśli chcesz alert licznika wydajności przekroczenia progu określonego tych progów powinien zostać uwzględniony w kwerendzie.

Na przykład, jeśli chcesz utworzyć alert po uruchomieniu procesor ponad 90% przez 30 minut, czy za pomocą kwerendy like *Typ = nazwa_obiektu wydajności = CounterName procesor = "% czasu procesora" równowartości > 90* i próg dla reguły alertu na wartość *większą niż 0*.  

 Ponieważ [rekordy wydajności](log-analytics-data-sources-performance-counters.md) są połączone co 30 minut, niezależnie od tego, częstotliwość zebrania każdego licznika, przedział czasu mniejszy niż 30 minut może zwracać żadnych rekordów.  Ustawianie przedziału czasu do 30 minut będzie mieć pewność, że jeden rekord dla każdego połączonego źródła, przedstawiającą średnią w tym czasie.

## <a name="alert-actions"></a>Akcje alertu

Oprócz tworzenia rekordu alertów, można skonfigurować reguły alertu, aby automatycznie uruchomić jedną lub więcej akcji.  Akcje można wyprzedzeniem informujący o alercie lub wywołać procesu próbującym rozwiązać ten problem, które zostało wykryte.  W poniższych sekcjach opisano działania, które są obecnie dostępne.

### <a name="email-actions"></a>Akcje dotyczące wiadomości e-mail
Akcje dotyczące wiadomości e-mail Wyślij wiadomość e-mail zawierająca szczegóły alertu do jednego lub wielu adresatów.  Możesz określić temat wiadomości e-mail, ale jego zawartość jest to standardowy format tworzona przez analizy dziennika.  Zawiera informacje podsumowujące, takie jak nazwa alertu oprócz szczegóły do dziesięciu rekordy zwrócone przez przeszukiwanie dziennika.  Zawiera również łącze do wyszukiwania dziennika w analizy dziennika, który zwróci cały zestaw rekordów z tego zapytania.   Nadawca wiadomości *zespół pakietu Microsoft operacje zarządzania &lt; noreply@oms.microsoft.com *. 


### <a name="webhook-actions"></a>Akcje Webhook

Akcje Webhook umożliwiają wywołania proces zewnętrzny za pomocą pojedynczego żądania HTTP POST.  Usługa wywoływana należy obsługuje webhooks i określanie sposobu używania dowolnego ładunku otrzyma.  Można również wywołać interfejsu API usługi REST, która specjalnie nie obsługuje webhooks, dopóki wniosek jest w formacie, który API rozumie.  Przykłady używania webhook w odpowiedzi na alerty o używają usług, takich jak [Zapas czasu](http://slack.com) , aby wysłać wiadomość zawierająca szczegóły alertu lub Tworzenie zdarzenia w usług, takich jak [PagerDuty](http://pagerduty.com/).  

Wykonano Przewodnik po utworzeniu reguły alertu o webhook nawiązać połączenie z usługą próbki jest dostępna w [Webhooks alertów analizy dziennika](log-analytics-alerts-webhooks.md).

Webhooks obejmują adresu URL i ładunku sformatowane w formacie, który jest dane wysyłane do zewnętrznej usługi JSON.  Domyślnie ładunek będzie zawierać wartości z poniższej tabeli.  Możesz zastąpić ten ładunek niestandardowe własny.  W takim przypadku służy zmiennych w tabeli dla każdego z parametrów do uwzględnienia ich wartości w swojej ładunku niestandardowe.


| Parametr | Zmienna | Opis |
|:--|:--|:--|
| AlertRuleName | #alertrulename | Nazwa reguły alertu. |
| AlertThresholdOperator | #thresholdoperator | Operator progu dla reguły alertu.  *Większe* lub *mniejsze niż*. |
| AlertThresholdValue | #thresholdvalue | Wartość progowa dla reguły alertu. |
| LinkToSearchResults | #linktosearchresults | Łącze do analizy dziennika dziennika wyszukiwania, która zwraca rekordy z utworzonym alertu kwerendy. |
| ResultCount  | #searchresultcount | Liczba rekordów w wynikach wyszukiwania. |
| SearchIntervalEndtimeUtc  | #searchintervalendtimeutc | Czas zakończenia dla zapytania w formacie UTC. |
| SearchIntervalInSeconds | #searchinterval | Przedział czasu dla reguły alertu. |
| SearchIntervalStartTimeUtc  | #searchintervalstarttimeutc | Godzina rozpoczęcia dla zapytania w formacie UTC. |
| SearchQuery | #searchquery | Kwerendy wyszukiwania Dziennik używany przez regułę alertu. |
| Wynikówwyszukiwania | Zobacz poniższą sekcję | Rekordy zwrócone przez kwerendę w formacie JSON.  Ograniczone do pierwszych 5000 rekordów. |
| WorkspaceID | #workspaceid | Identyfikator usługi OMS obszaru roboczego. |


Na przykład można określić następujące niestandardowe ładunku zawierającego jeden parametr o nazwie *tekstu*.  Usługa, która wymaga to webhook sposób oczekiwany ten parametr.

    {
        "text":"#alertrulename fired with #searchresultcount over threshold of #thresholdvalue."
    }

Ten przykład ładunku czy rozpoznawać podobnego do następującego po wysłaniu do webhook.

    {
        "text":"My Alert Rule fired with 18 records over threshold of 10 ."
    }

Aby uwzględnić wyniki wyszukiwania w niestandardowych ładunku, Dodaj poniższy wiersz jako właściwość najwyższego poziomu w ładunku json.  

    "IncludeSearchResults":true

Na przykład aby utworzyć niestandardowe ładunku zawierająca tylko nazwę alertów i wyników wyszukiwania, można następujące czynności. 

    {
       "alertname":"#alertrulename",
       "IncludeSearchResults":true
    }


Czy szczegółową pełny przykład tworzenia reguły alertu z webhook, aby rozpocząć zewnętrznej usługi u [analizy dziennika alert webhook próbki](log-analytics-alerts-webhooks.md).

### <a name="runbook-actions"></a>Akcje działań aranżacji

Akcje działań aranżacji Uruchom działań aranżacji w automatyzacji Azure.  Aby użyć tego typu akcji, musisz mieć [rozwiązanie automatyzacji](log-analytics-add-solutions.md) zainstalowaniu i skonfigurowaniu w obszarze roboczym usługi OMS.  Jeśli nie jest zainstalowany, podczas tworzenia nowej reguły alertu, łącza do jego instalacji jest wyświetlany.  Możesz wybrać runbooks na koncie automatyzacji, który skonfigurowano w rozwiązaniu automatyzacji.

Akcje działań aranżacji Rozpoczynanie działań aranżacji przy użyciu [webhook](../automation/automation-webhooks.md).  Po utworzeniu reguły alertu automatycznie utworzy nowy webhook dla działań aranżacji o nazwie **Korygowania Alert usługi OMS** następuje identyfikator GUID.  

Nie można bezpośrednio wypełnić parametry działań aranżacji, ale [parametru $WebhookData](../automation/automation-webhooks.md) będzie zawierać szczegóły alert, łącznie z wynikami wyszukiwania dziennika, w którym go utworzono.  Działań aranżacji należy zdefiniować **$WebhookData** jako parametru go, aby uzyskać dostęp do właściwości alertu.  Alert dane są dostępne w formacie json na jedną właściwość o nazwie **wynikówwyszukiwania** we właściwości **RequestBody** **$WebhookData**.  Ma to przy użyciu właściwości w poniższej tabeli.


| Węzeł | Opis |
|:--|:--|
| Identyfikator         | Ścieżka i identyfikator GUID wyszukiwania. |
| __metadata | Informacje na temat alertu w tym liczbę rekordów i stan wyników wyszukiwania. |
|  wartość     |  Oddzielne wpis dla każdego rekordu w wynikach wyszukiwania.  Szczegóły wpisu będą zgodne właściwości i wartości rekordu.   |

Na przykład następujących działań aranżacji wyodrębnić rekordy zwrócone przez przeszukiwanie dziennika i przypisać różne właściwości zależy od typu każdy rekord.  Zauważ, że działań aranżacji rozpoczyna się od konwertowania **RequestBody** json tak, aby go można pracować przy użyciu programu PowerShell jako obiekt.

    param ( 
        [object]$WebhookData
    )

    $RequestBody = ConvertFrom-JSON -InputObject $WebhookData.RequestBody
    $Records     = $RequestBody.SearchResults.value
    
    foreach ($Record in $Records)
    {
        $Computer = $Record.Computer
        
        if ($Record.Type -eq 'Event')
        {
            $EventNo    = $Record.EventID
            $EventLevel = $Record.EventLevelName
            $EventData  = $Record.EventData
        }
        
        if ($Record.Type -eq 'Perf')
        {
            $Object    = $Record.ObjectName
            $Counter   = $Record.CounterName
            $Instance  = $Record.InstanceName
            $Value     = $Record.CounterValue
        }
    }


## <a name="alert-records"></a>Alert rekordów

Alert rekordów utworzonych przez reguły alertów w dzienniku analizy mieć **Typ** **alertu** i **SourceSystem** z **usługi OMS**.  W poniższej tabeli mają właściwości.

| Właściwość | Opis |
|:--|:--|
| Typ          | *Alert* |
| SourceSystem  | *USŁUGI OMS* |
| AlertSeverity | Poziom ważności alertu. |
| AlertName     | Nazwa alertu. |
| Kwerendy         | Tekst kwerendy, które zostało uruchomione.  |
| QueryExecutionEndTime   | Koniec przedziału czasu dla zapytania. |
| QueryExecutionStartTime | Początek przedziału czasu dla zapytania.  |
| TimeGenerated | Data i godzina utworzenia alertu. |

Istnieją inne rodzaje alert rekordów utworzone przez [rozwiązanie do zarządzania alertu](log-analytics-solution-alert-management.md) i [eksportuje Power BI](log-analytics-powerbi.md).  Te wszystkie mają **Typ** **alertów** , ale są wyróżnione ich **SourceSystem**.




## <a name="next-steps"></a>Następne kroki

- Zainstaluj [rozwiązanie do zarządzania alertami](log-analytics-solution-alert-management.md) do analizowania alerty utworzone w analizy dziennika wraz z alertów, zebrane z systemu Centrum Operations Manager (SCOM).
- Dowiedz się więcej o wygenerować alerty [wyszukiwania dziennika](log-analytics-log-searches.md) .
- Wykonaj instrukcje dotyczące [konfigurowania webook](log-analytics-alerts-webhooks.md) przy użyciu reguły alertu.  
- Dowiedz się, jak napisać [runbooks w automatyzacji Azure](https://azure.microsoft.com/documentation/services/automation) do korygowania problemów oznaczane alertów.