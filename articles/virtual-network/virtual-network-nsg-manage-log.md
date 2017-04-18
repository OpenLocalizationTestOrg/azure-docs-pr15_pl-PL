<properties
   pageTitle="Monitorowanie operacji, zdarzeń i liczniki dla NSGs | Microsoft Azure"
   description="Dowiedz się, jak włączyć liczniki, zdarzeń i rejestrowanie operacyjnych dla NSGs"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-resource-manager"
/>
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="07/14/2016"
   ms.author="jdial" />

#<a name="log-analytics-for-network-security-groups-nsgs"></a>Dziennik analizy grup zabezpieczeń sieci (NSGs)

Różne typy dzienników Azure umożliwia zarządzanie i rozwiązywanie problemów z NSGs. Niektóre z tych dzienników są dostępne za pośrednictwem portalu, a wszystkie dzienniki można wyodrębnić z magazynem obiektów blob Azure i wyświetlać na różnych narzędzi, takich jak [Analizy dziennika](../log-analytics/log-analytics-azure-networking-analytics.md), Excel i PowerBI. Więcej informacji o różnych typów dzienników z poniższej listy można znaleźć.

- **Dzienników inspekcji:** Za pomocą [Dzienników inspekcji Azure](../monitoring-and-diagnostics/insights-debugging-with-events.md) (wcześniej nazywanego dzienniki operacyjne) aby wyświetlić wszystkie operacje przesyłany do subskrypcjach Azure i ich stanu. Dzienniki inspekcji są domyślnie włączone i mogą być wyświetlane w portal Azure preview.
- **Dzienniki zdarzeń:** Ten dziennik umożliwia wyświetlanie, jakie NSG reguły są stosowane do maszyny wirtualne i ról wystąpienia na podstawie adresu MAC. Stan te reguły są zbierane co 60 sekund.
- **Licznik dzienniki:** Ten dziennik umożliwia wyświetlanie, ile razy każda reguła NSG została zastosowana do odmówić lub zezwalanie na ruch.

>[AZURE.WARNING] Dzienniki są dostępne tylko dla zasobów wdrożony w modelu wdrożenia Menedżera zasobów. Nie można użyć dzienników zasobów w modelu Klasyczny wdrożenia. W celu lepszego zrozumienia dwóch modeli, odwoływać się do tego artykułu [wdrożenia opis Menedżera zasobów i wdrażania klasyczny](../resource-manager-deployment-model.md) .

##<a name="enable-logging"></a>Włączanie rejestrowania
Rejestrowanie inspekcji jest włączany automatycznie w każdej chwili dla każdego zasobu Menedżera zasobów. Musisz włączyć zdarzeń i rejestrowanie, aby rozpocząć pobieranie danych dostępne za pośrednictwem tych dzienników liczników. Aby włączyć rejestrowanie, wykonaj poniższe czynności.

1.  Zaloguj się do [portalu Azure](https://portal.azure.com). Jeśli nie masz już istniejącej grupy zabezpieczeń sieci, przed kontynuowaniem [Utwórz NSG](virtual-networks-create-nsg-arm-ps.md) .

2.  W portalu Podgląd, kliknij przycisk **Przeglądaj** >> **grup zabezpieczeń sieci**.

    ![Portal Preview — grupy zabezpieczeń sieci](./media/virtual-network-nsg-manage-log/portal-enable1.png)

3. Wybierz istniejącą grupę zabezpieczeń sieci.

    ![Portal Preview — ustawienia grupy zabezpieczeń sieci](./media/virtual-network-nsg-manage-log/portal-enable2.png)

4. W karta **Ustawienia** kliknij pozycję **Diagnostyka**, a następnie w okienku **diagnostyki** obok **stanu**, kliknij **na**
5. Karta **Ustawienia** kliknij **Konto miejsca do magazynowania**i albo wybierz istniejące magazynowania konta lub utworzyć nowe.  

>[AZURE.INFORMATION] dzienników inspekcji nie wymaga konta oddzielnie. Wykorzystanie miejsca do magazynowania dla zdarzeń i rejestrowanie reguła będzie powodowało opłat za usługi.

6. Na liście rozwijanej tuż pod **Uwagę miejsca do magazynowania**Określ, czy chcesz dziennika zdarzeń i liczników, a następnie kliknij przycisk **Zapisz**.

    ![Portal Preview — dzienniki diagnostyczne](./media/virtual-network-nsg-manage-log/portal-enable3.png)

## <a name="audit-log"></a>Dziennik inspekcji
Dziennik (wcześniej nazywanego "Dziennik operacji") jest generowany przez Azure domyślnie.  Dzienniki są zachowywane przez 90 dni w sklepie dzienniki zdarzeń Azure firmy. Więcej informacji o tych dzienników, czytając artykuł [Wyświetlanie zdarzeń i dzienników inspekcji](../monitoring-and-diagnostics/insights-debugging-with-events.md) .

## <a name="counter-log"></a>Dziennik liczników
Dziennik jest generowana tylko, jeśli został włączony na zasadzie na NSG zgodnie z powyższym. Dane są przechowywane w określonym przez Ciebie podczas włączone rejestrowanie rachunku miejsca do magazynowania. Każda reguła stosowane do zasobów jest rejestrowany w formacie JSON, jak pokazano poniżej.

    {
        "time": "2015-09-11T23:14:22.6940000Z",
        "systemId": "e22a0996-e5a7-4952-8e28-4357a6e8f0c5",
        "category": "NetworkSecurityGroupRuleCounter",
        "resourceId": "/SUBSCRIPTIONS/D763EE4A-9131-455F-8C5E-876035455EC4/RESOURCEGROUPS/INSIGHTOBONRP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/NSGINSIGHTOBONRP",
        "operationName": "NetworkSecurityGroupCounters",
        "properties": {
            "vnetResourceGuid":"{DD0074B1-4CB3-49FA-BF10-8719DFBA3568}",
            "subnetPrefix":"10.0.0.0/24",
            "macAddress":"001517D9C43C",
            "ruleName":"DenyAllOutBound",
            "direction":"Out",
            "type":"block",
            "matchedConnections":0
            }
    }

## <a name="event-log"></a>Dziennik zdarzeń
Dziennik jest generowana tylko, jeśli został włączony na zasadzie na NSG zgodnie z powyższym. Dane są przechowywane w określonym przez Ciebie podczas włączone rejestrowanie rachunku miejsca do magazynowania. Jest rejestrowany poniższe dane:

    {
        "time": "2015-09-11T23:05:22.6860000Z",
        "systemId": "e22a0996-e5a7-4952-8e28-4357a6e8f0c5",
        "category": "NetworkSecurityGroupEvent",
        "resourceId": "/SUBSCRIPTIONS/D763EE4A-9131-455F-8C5E-876035455EC4/RESOURCEGROUPS/INSIGHTOBONRP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/NSGINSIGHTOBONRP",
        "operationName": "NetworkSecurityGroupEvents",
        "properties": {
            "vnetResourceGuid":"{DD0074B1-4CB3-49FA-BF10-8719DFBA3568}",
            "subnetPrefix":"10.0.0.0/24",
            "macAddress":"001517D9C43C",
            "ruleName":"AllowVnetOutBound",
            "direction":"Out",
            "priority":65000,
            "type":"allow",
            "conditions":{
                "destinationPortRange":"0-65535",
                "sourcePortRange":"0-65535",
                "destinationIP":"10.0.0.0/8,172.16.0.0/12,169.254.0.0/16,192.168.0.0/16,168.63.129.16/32",
                "sourceIP":"10.0.0.0/8,172.16.0.0/12,169.254.0.0/16,192.168.0.0/16,168.63.129.16/32"
            }
        }
    }

## <a name="view-and-analyze-the-audit-log"></a>Wyświetlanie i analizowanie dziennika inspekcji
Można wyświetlać i analizowanie danych dziennika inspekcji przy użyciu jednej z następujących metod:

- **Azure narzędzia:** Pobieraj informacje z dzienników inspekcji za pośrednictwem programu PowerShell Azure, interfejs wiersza polecenia Azure (polecenie), interfejsu API usługi REST Azure lub portal Azure preview.  Instrukcje krok po kroku dla każdej metody wyszczególniono w artykule [inspekcji operacji przy użyciu Menedżera zasobów](../resource-group-audit.md) .
- **Power BI:** Jeśli nie masz jeszcze konta [Usługi Power BI](https://powerbi.microsoft.com/pricing) , możesz spróbować je bezpłatnie. Za pomocą [dzienników inspekcji Azure zawartości pakiet dla usługi Power BI](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-audit-logs/) podczas analizowania danych za pomocą wstępnie skonfigurowane pulpity nawigacyjne, które są dostępne jako-jest lub dostosowywanie.

## <a name="view-and-analyze-the-counter-and-event-log"></a>Wyświetlanie i analizowanie licznik i dziennika zdarzeń

Azure [Analizy dziennika](../log-analytics/log-analytics-azure-networking-analytics.md) może zbierać licznik i dziennika zdarzeń pliki z Twojego konta magazyn obiektów Blob i zawiera wizualizacji i szerokie możliwości wyszukiwania do analizowania dzienników.

Można także połączyć się z kontem miejsca do magazynowania i pobrać pozycje dziennika JSON dzienniki zdarzeń i licznik. Po pobraniu plików JSON, możesz je przekonwertować CSV i widoku w programie Excel, PowerBI lub innego narzędzia do wizualizacji danych.

>[AZURE.TIP] Użytkownicy zaznajomieni z programu Visual Studio i podstawowe pojęcia zmianę wartości stałych i zmiennych w języku C#, można użyć [Narzędzia konwertera dziennika](https://github.com/Azure-Samples/networking-dotnet-log-converter) dostępnych w Github.

## <a name="next-steps"></a>Następne kroki

- Wizualizowanie licznik i dzienniki zdarzeń z [Analizy dziennika](../log-analytics/log-analytics-azure-networking-analytics.md)
- Wpis w blogu [Wizualizacja Azure dzienników inspekcji dzięki usłudze Power BI](http://blogs.msdn.com/b/powerbi/archive/2015/09/30/monitor-azure-audit-logs-with-power-bi.aspx) .
- [Wyświetlanie i analizowanie Azure dzienników inspekcji w usłudze Power BI i innych](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/) wpis w blogu.
