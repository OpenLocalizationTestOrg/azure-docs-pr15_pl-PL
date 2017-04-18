<properties 
   pageTitle="Monitorowanie dzienników programu access i wydajności i wskaźniki bramy aplikacji | Microsoft Azure"
   description="Dowiedz się, jak włączyć i zarządzać dzienników programu Access i wydajności bramy aplikacji"
   services="application-gateway"
   documentationCenter="na"
   authors="amitsriva"
   manager="rossort"
   editor="tysonn"
   tags="azure-resource-manager"
/>
<tags 
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/26/2016"
   ms.author="amitsriva" />

# <a name="diagnostics-logging-and-metrics-for-application-gateway"></a>Rejestrowanie diagnostyczne i metryki dla bramy aplikacji

Azure umożliwia monitorowanie zasobów z rejestrowania i miar

[**Rejestrowanie**](#enable-logging-with-powershell) - umożliwia rejestrowanie wydajności, dostęp i inne dzienniki mają być zapisywane lub zużyte od zasobu do celów monitorowania.

[**Metryki**](#metrics) — obecnie bramy aplikacji ma jedną metrykę. Ta metryka środków wydajności bramy aplikacji w bajtach na sekundę.

Różne typy dzienników Azure umożliwia zarządzanie i rozwiązywanie problemów z bram aplikacji. Niektóre z tych dzienników są dostępne za pośrednictwem portalu, a wszystkie dzienniki można wyodrębnić z magazynem obiektów blob Azure i wyświetlać na różnych narzędzi, takich jak [Analizy dziennika](../log-analytics/log-analytics-azure-networking-analytics.md), Excel i PowerBI. Możesz można dowiedzieć się więcej na temat różnych typów dzienników z poniższej listy:

- **Dzienników inspekcji:** Za pomocą [Dzienników inspekcji Azure](../monitoring-and-diagnostics/insights-debugging-with-events.md) (wcześniej nazywanego dzienniki operacyjne) aby wyświetlić wszystkie operacje przesyłany do subskrypcji usługi Azure i ich stanu. Dzienniki inspekcji są domyślnie włączone i mogą być wyświetlane w portal Azure preview.
- **Dostęp do dzienników:** Za pomocą tego dziennika do wyświetlania wzorzec dostępu bramy aplikacji i analizowania ważne informacje o tym rozmówcy IP, adres URL żądany, opóźnienie odpowiedzi i zmniejszanie zwracają kod, bajtów. Dziennik dostępu są zbierane co 300 sekund. Dziennik zawiera jeden rekord dla każdego wystąpienia aplikacji bramy. Właściwość "identyfikator wystąpienia" cechuje aplikację bramy.
- **Dzienniki wydajności:** Ten dziennik umożliwia wyświetlanie, jak działają wystąpień bram w aplikacji. Dziennik znajdują się informacje dotyczące wydajności na podstawie jednego wystąpienia sumy żądania obsługiwane, w tym przepustowość w bajtach, całkowita żądania obsługiwane, licznik żądania nie powiodło się, liczba prawidłowy i nieprawidłowe wystąpienie wewnętrznej. Dziennik wydajności są zbierane co 60 sekund.
- **Zapory Dzienniki:** Ten dziennik umożliwia wyświetlanie żądań, które są rejestrowane w trybie wykrywania lub zapobiegania bramy aplikacji, który skonfigurowano zaporę aplikacji sieci web.

>[AZURE.WARNING] Dzienniki są dostępne tylko dla zasobów wdrożony w modelu wdrożenia Menedżera zasobów. Nie można użyć dzienników zasobów w modelu Klasyczny wdrożenia. W celu lepszego zrozumienia dwóch modeli, odwoływać się do tego artykułu [wdrożenia opis Menedżera zasobów i wdrażania klasyczny](../resource-manager-deployment-model.md) .

## <a name="enable-logging-with-powershell"></a>Włącz rejestrowanie przy użyciu programu PowerShell

Rejestrowanie inspekcji jest automatycznie włączana dla każdego zasobu Menedżera zasobów. Musisz włączyć dostęp i rejestrowania w celu rozpoczęcia zbierania danych dostępne za pośrednictwem tych dzienników wydajności. Aby włączyć rejestrowanie, zobacz następujące czynności: 

1. Zanotuj identyfikator zasobu konta miejsca do magazynowania, przechowywania danych dziennika. Będzie formularza: /subscriptions/\<subscriptionId\>/resourceGroups/\<Nazwa grupy zasobów\>/providers/Microsoft.Storage/storageAccounts/\<nazwę konta magazynu\>. Można używać żadnego z kont miejsca do magazynowania w ramach subskrypcji. Za pomocą portalu Podgląd znaleźć te informacje.

    ![Wyświetlanie podglądu portalu - diagnostyki bramy aplikacji](./media/application-gateway-diagnostics/diagnostics1.png)

2. Uwaga identyfikator zasobu Centrum aplikacji dla rejestrowania, które ma zostać włączona. Będzie formularza: /subscriptions/\<subscriptionId\>/resourceGroups/\<Nazwa grupy zasobów\>/providers/Microsoft.Network/applicationGateways/\<nazwa bramy aplikacji\>. Za pomocą portalu Podgląd znaleźć te informacje.

    ![Wyświetlanie podglądu portalu - diagnostyki bramy aplikacji](./media/application-gateway-diagnostics/diagnostics2.png)

3. Włącz rejestrowanie diagnostyczne przy użyciu następującego polecenia cmdlet programu powershell:

        Set-AzureRmDiagnosticSetting  -ResourceId /subscriptions/<subscriptionId>/resourceGroups/<resource group name>/providers/Microsoft.Network/applicationGateways/<application gateway name> -StorageAccountId /subscriptions/<subscriptionId>/resourceGroups/<resource group name>/providers/Microsoft.Storage/storageAccounts/<storage account name> -Enabled $true  

>[AZURE.INFORMATION] dzienników inspekcji nie wymaga konta oddzielnie. Wykorzystanie miejsca do magazynowania dla programu access i rejestrowanie wydajności opłatami usługi.

## <a name="enable-logging-with-azure-portal"></a>Włącz rejestrowanie Portal Azure

### <a name="step-1"></a>Krok 1

Przejdź do zasobu w portalu Azure. Kliknij pozycję **Dzienniki diagnostyczne**. Jeśli po raz pierwszy Konfigurowanie diagnostyki karta wygląda poniższej ilustracji:

Dla bramy aplikacji 3 dzienniki są dostępne.

- Dziennik dostępu
- Dziennik wydajności
- Dziennik zapory

Kliknij przycisk **Włącz Diagnostyka** , aby rozpocząć zbieranie danych.

![Karta Ustawienia diagnostyki][1]

### <a name="step-2"></a>Krok 2

Na karta **ustawień diagnostyki** ustawień jak dzienniki diagnostyczne są ustawione. W tym przykładzie dziennika analizy służy do przechowywania dzienników. W obszarze **Analizy dziennika** do konfigurowania obszaru roboczego, kliknij przycisk **Konfiguruj** . Aby zapisać dzienniki diagnostyczne także można koncentratory zdarzeń i konto miejsca do magazynowania.

![Karta Narzędzia diagnostyczne][2]

### <a name="step-3"></a>Krok 3

Wybierz pozycję z istniejącym obszarem roboczym usługi OMS lub Utwórz nowy. W tym przykładzie jest używany jeden z istniejących.

![obszary robocze usługi OMS][3]

### <a name="step-4"></a>Krok 4

Po zakończeniu Potwierdź ustawienia, a następnie kliknij przycisk **Zapisz** , aby zapisać ustawienia.

![Potwierdź wybór][4]

## <a name="audit-log"></a>Dziennik inspekcji

Dziennik (wcześniej nazywanego "Dziennik operacji") jest generowany przez Azure domyślnie.  Dzienniki są zachowywane przez 90 dni w sklepie dzienniki zdarzeń w Azure. Więcej informacji o tych dzienników, czytając artykuł [Wyświetlanie zdarzeń i dzienników inspekcji](../monitoring-and-diagnostics/insights-debugging-with-events.md) .

## <a name="access-log"></a>Dziennik dostępu

Dziennik jest generowana tylko, jeśli został włączony w na podstawa Application Gateway wyszczególnione w poprzednich krokach. Dane są przechowywane w określonym przez Ciebie podczas włączone rejestrowanie rachunku miejsca do magazynowania. Każdy dostęp bramy aplikacji jest rejestrowany w formacie JSON, jak pokazano w poniższym przykładzie:

    {
        "resourceId": "/SUBSCRIPTIONS/<subscription id>/RESOURCEGROUPS/<resource group name>/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/<application gateway name>",
        "operationName": "ApplicationGatewayAccess",
        "time": "2016-04-11T04:24:37Z",
        "category": "ApplicationGatewayAccessLog",
        "properties": {
            "instanceId":"ApplicationGatewayRole_IN_0",
            "clientIP":"37.186.113.170",
            "clientPort":"12345",
            "httpMethod":"HEAD",
            "requestUri":"/xyz/portal",
            "requestQuery":"",
            "userAgent":"-",
            "httpStatus":"200",
            "httpVersion":"HTTP/1.0",
            "receivedBytes":"27",
            "sentBytes":"202",
            "timeTaken":"359",
            "sslEnabled":"off"
        }
    }

## <a name="performance-log"></a>Dziennik wydajności

Dziennik jest generowana tylko, jeśli włączono na na aplikacji bramy podstawa wyszczególnione w poprzednich krokach. Dane są przechowywane w określonym przez Ciebie podczas włączone rejestrowanie rachunku miejsca do magazynowania. Jest rejestrowany poniższe dane:

    {
        "resourceId": "/SUBSCRIPTIONS/<subscription id>/RESOURCEGROUPS/<resource group name>/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/<application gateway name>",
        "operationName": "ApplicationGatewayPerformance",
        "time": "2016-04-09T00:00:00Z",
        "category": "ApplicationGatewayPerformanceLog",
        "properties": 
        {
            "instanceId":"ApplicationGatewayRole_IN_1",
            "healthyHostCount":"4",
            "unHealthyHostCount":"0",
            "requestCount":"185",
            "latency":"0",
            "failedRequestCount":"0",
            "throughput":"119427"
        }
    }


## <a name="firewall-log"></a>Dziennik zapory

Dziennik jest generowana tylko, jeśli włączono na na aplikacji bramy podstawa wyszczególnione w poprzednich krokach. Dziennik wymaga również zaporą aplikacji sieci web można skonfigurować na bramy aplikacji. Dane są przechowywane w określonym przez Ciebie podczas włączone rejestrowanie rachunku miejsca do magazynowania. Jest rejestrowany poniższe dane:

    {
        "resourceId": "/SUBSCRIPTIONS/<subscriptionId>/RESOURCEGROUPS/<resourceGroupName>/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/<applicationGatewayName>",
        "operationName": "ApplicationGatewayFirewall",
        "time": "2016-09-20T00:40:04.9138513Z",
        "category": "ApplicationGatewayFirewallLog",
        "properties":     {
            "instanceId":"ApplicationGatewayRole_IN_0",
            "clientIp":"108.41.16.164",
            "clientPort":1815,
            "requestUri":"/wavsep/active/RXSS-Detection-Evaluation-POST/",
            "ruleId":"OWASP_973336",
            "message":"XSS Filter - Category 1: Script Tag Vector",
            "action":"Logged",
            "site":"Global",
            "message":"XSS Filter - Category 1: Script Tag Vector",
            "details":{"message":" Warning. Pattern match "(?i)(<script","file":"/owasp_crs/base_rules/modsecurity_crs_41_xss_attacks.conf","line":"14"}}
    }

## <a name="view-and-analyze-the-audit-log"></a>Wyświetlanie i analizowanie dziennika inspekcji

Można wyświetlać i analizowanie danych dziennika inspekcji przy użyciu jednej z następujących metod:

- **Azure narzędzia:** Pobieraj informacje z dzienników inspekcji za pośrednictwem programu PowerShell Azure, interfejs wiersza polecenia Azure (polecenie), interfejsu API usługi REST Azure lub portal Azure preview.  Instrukcje krok po kroku dla każdej metody wyszczególniono w artykule [inspekcji operacji przy użyciu Menedżera zasobów](../resource-group-audit.md) .
- **Power BI:** Jeśli nie masz jeszcze konta [Usługi Power BI](https://powerbi.microsoft.com/pricing) , możesz spróbować je bezpłatnie. Za pomocą [dzienników inspekcji Azure zawartości pakiet dla usługi Power BI](https://powerbi.microsoft.com/en-us/documentation/powerbi-content-pack-azure-audit-logs/) podczas analizowania danych za pomocą wstępnie skonfigurowane pulpity nawigacyjne, które są dostępne jako-jest lub dostosowywanie.

## <a name="view-and-analyze-the-access-performance-and-firewall-log"></a>Wyświetlanie i analizowanie dziennika programu access, wydajności i zapory

Azure [Analizy dziennika](../log-analytics/log-analytics-azure-networking-analytics.md) może zbierać licznik i dziennika zdarzeń pliki z Twojego konta magazyn obiektów Blob i zawiera wizualizacji i szerokie możliwości wyszukiwania do analizowania dzienników.

Możesz połączyć się z kontem miejsca do magazynowania i pobrać pozycje dziennika JSON dzienników programu access i wydajności. Po pobraniu plików JSON, możesz je przekonwertować CSV i widoku w programie Excel, PowerBI lub innego narzędzia do wizualizacji danych.

>[AZURE.TIP] Użytkownicy zaznajomieni z programu Visual Studio i podstawowe pojęcia zmianę wartości stałych i zmiennych w języku C#, można użyć [Narzędzia konwertera dziennika](https://github.com/Azure-Samples/networking-dotnet-log-converter) dostępnych w Github.

## <a name="metrics"></a>Metryki

Metryki jest funkcją dla niektórych Azure zasobów, gdzie można przeglądać liczniki wydajności w portalu. Dla bramy aplikacji jeden metryki jest dostępna w momencie zapisywania w tym artykule. Ta metryka jest przepustowość i są widoczne w portalu. Przejdź do bramy aplikacji, a następnie kliknij pozycję **wskaźniki**.  Wybierz pozycję przepustowość w sekcji **Dostępne wskaźniki** , aby wyświetlić wartości. Na poniższej ilustracji widać przykład filtry, których można używać do wyświetlania danych w innym czasie zakresów.

Aby wyświetlić listę obecnie obsługują metryki, odwiedź stronę [metryki obsługiwane przy użyciu monitora Azure](../monitoring-and-diagnostics/monitoring-supported-metrics.md)

![Metryka widoku][5]

## <a name="alert-rules"></a>Reguły alertów

Reguły można uruchamiać z oparciu metryki na zasób. Oznacza to, że dla bramy aplikacji alertu można zadzwonić webhook lub poczty e-mail przez administratora, jeśli przepustowość bramy aplikacji jest powyżej, poniżej lub u progu przez określony czas.

Poniższy przykład przeprowadzi Cię przez proces tworzenia wysyłania wiadomości e-mail do administratora po progu przepustowość zostało naruszone reguły alertu.

### <a name="step-1"></a>Krok 1

Kliknij przycisk **Dodaj metryczne alert** , aby rozpocząć. Ta karta również można się z Tobą z karta metryki.

![Karta reguł alertów][6]

### <a name="step-2"></a>Krok 2

W karta **Dodaj regułę** Wypełnij nazwę warunku i powiadom sekcje, a następnie kliknij przycisk **OK** , gdy.

Selektor **warunek** umożliwia 4 wartości **większe niż**, **większe niż lub równe**, **mniejsze niż**lub **mniejsze niż lub równe**.

Selektor **okresu** umożliwia wybranie okres od 5 minut do 6 godzin.

Wybierając **Właściciele poczty E-mail, uczestników i czytników** wiadomości e-mail może być dynamiczne według użytkowników, którzy mają dostęp do tego zasobu. W przeciwnym razie przecinkami listy użytkowników może być udostępniana w polu tekstowym **email(s) dodatkowe administratora** .

![Dodawanie reguły karta][7]

Jeśli naruszenia progu wiadomości e-mail są dostarczane podobny do tego na poniższej ilustracji:

![Próg naruszenia poczty e-mail][8]

Lista alertów jest wyświetlana po metryczne alert została utworzona i zawiera omówienie reguł alertów.

![Wyświetlanie alertu][9]

Aby dowiedzieć się więcej na temat alertów, odwiedź stronę [odbieranie alertów](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

Aby dowiedzieć się więcej o webhooks oraz jak można używać ich z alertów, odwiedź stronę [Konfigurowanie webhook na alercie metryczne Azure](../monitoring-and-diagnostics/insights-webhooks-alerts.md)

## <a name="next-steps"></a>Następne kroki

- Wizualizowanie licznik i dzienniki zdarzeń z [Analizy dziennika](../log-analytics/log-analytics-azure-networking-analytics.md) 
- Wpis w blogu [Wizualizacja Azure dzienników inspekcji dzięki usłudze Power BI](http://blogs.msdn.com/b/powerbi/archive/2015/09/30/monitor-azure-audit-logs-with-power-bi.aspx) .
- [Wyświetlanie i analizowanie Azure dzienników inspekcji w usłudze Power BI i innych](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/) wpis w blogu.

[1]: ./media/application-gateway-diagnostics/figure1.png
[2]: ./media/application-gateway-diagnostics/figure2.png
[3]: ./media/application-gateway-diagnostics/figure3.png
[4]: ./media/application-gateway-diagnostics/figure4.png
[5]: ./media/application-gateway-diagnostics/figure5.png
[6]: ./media/application-gateway-diagnostics/figure6.png
[7]: ./media/application-gateway-diagnostics/figure7.png
[8]: ./media/application-gateway-diagnostics/figure8.png
[9]: ./media/application-gateway-diagnostics/figure9.png