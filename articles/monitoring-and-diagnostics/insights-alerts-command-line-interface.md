<properties
    pageTitle="Tworzenie alertów dla usług Azure za pomocą interfejsu wiersza polecenia (polecenie między platformami) | Microsoft Azure"
    description="Tworzenie alertów Azure, które może spowodować powiadomienia za pomocą interfejsu wiersza polecenia lub automatyzacji, określając warunki są spełnione."
    authors="rboucher"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="robb"/>

# <a name="use-the-cross-platform-command-line-interface-cli-to-create-alerts-for-azure-services"></a>Tworzenie alertów dla usług Azure za pomocą interfejsu wiersza polecenia i platform (polecenie)

> [AZURE.SELECTOR]
- [Portal](insights-alerts-portal.md)
- [Programu PowerShell](insights-alerts-powershell.md)
- [POLECENIE](insights-alerts-command-line-interface.md)

## <a name="overview"></a>Omówienie

W tym artykule pokazano, jak skonfigurować alerty Azure za pomocą interfejsu wiersza polecenia (polecenie).

>[AZURE.NOTE] Monitorowanie Azure jest nową nazwę proponowaną "Azure wniosków" do 25-wrz 2016. Jednak obszarów nazw, a więc poniżej poleceń nadal zawierać "wniosków".

Możesz otrzymywać alertu na podstawie monitorowania metryki dla lub zdarzeniami na usługi Azure.

- **Wartości metryki** - wyzwalaczy alertu, gdy wartość określonej metryki przecina progu, które można przypisać w dowolnym kierunku. Oznacza to, że wyzwalane oba po spełnienia warunku, a następnie później gdy warunek jest już nie są spełnione.    
- **Zdarzeń dziennika aktywności** — alertu można uruchamiać na *każde* zdarzenie lub, tylko w przypadku, gdy występują pewne zdarzenia.

Możesz skonfigurować alert, wykonaj następujące czynności, gdy go wyzwalane:

- Wysyłanie powiadomienia e-mail do administratora usługi i administratorów współpracujących
- Wysyłanie wiadomości e-mail do kolejnych wiadomości przez użytkownika.
- połączenie webhook
- rozpoczęcia realizacji Azure działań aranżacji (tylko z portal Azure w tej chwili)

Można skonfigurować i uzyskiwanie informacji na temat reguł alertów przy użyciu

- [Azure portal](insights-alerts-portal.md)
- [Programu PowerShell](insights-alerts-powershell.md)
- [Interfejs wiersza polecenia (polecenie)](insights-alerts-command-line-interface.md)
- [Monitorowanie Azure interfejsu API usługi REST](https://msdn.microsoft.com/library/azure/dn931945.aspx)


Zawsze możesz odbierać pomoc dotyczącą polecenia, wpisując polecenia i — pomoc na końcu. Na przykład:

    ```console
    azure insights alerts -help
    azure insights alerts actions email create -help
    ```

## <a name="create-alert-rules-using-the-cli"></a>Tworzenie reguł alertów za pomocą interfejsu wiersza polecenia

1. Wykonywanie wymagania wstępne i zaloguj się do Azure. Zobacz [Przykłady polecenie Monitor Azure](insights-cli-samples.md). Krótko mówiąc Zainstaluj polecenie i uruchom następujące polecenia. Ich pomogą Ci zalogowany, pokazywanie posiadana subskrypcja, a przygotować się do uruchomienia polecenia Azure Monitor.


    ```console
    azure login
    azure account show
    azure config mode arm

    ```

2.  Aby wyświetlić listę istniejących reguł w grupie zasobów, za pomocą następujących formularza **azure wniosków, że alerty reguły listy** *[opcje] &lt;grupa zasobów&gt; *

    ```console
    azure insights alerts rule list myresourcegroupname

    ```
3. Aby utworzyć regułę, musisz najpierw mieć kilka ważnych elementów informacji.
    - **Identyfikator zasobu** dla zasobu, do którego chcesz ustawić alert dotyczący
    - **Definicje metryczne** dostępnych dla tego zasobu

    Jednym ze sposobów uzyskać identyfikator zasobu jest Azure portal za pomocą. Zakładając, że zasobu został już utworzony, wybierz go w portalu. W następnym karta, wybierz pozycję *Właściwości* w sekcji *Ustawienia* . *Identyfikator ZASOBU* to pole w następnym karta. Innym sposobem jest za pomocą [Eksploratora zasobów Azure](https://resources.azure.com/).

    Przykład identyfikator zasobów dla aplikacji sieci web

    ```console
    /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename
    ```

    Aby uzyskać listę dostępnych metryki i jednostki dla tych metryki w poprzednim przykładzie zasobów, użyj polecenia następujące polecenie:  

    ```console
    azure insights metrics list /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename PT1M
    ```

    _PT1M_ jest szczegółowości dostępne miary (1-minutowy). Za pomocą różnych stopnie szczegółowości umożliwia różne opcje metryczne.


4. Aby utworzyć regułę alertu metryczne, użyj polecenia następującą postać:

    **wartość metryki reguły alertów wniosków Azure** *[opcje] &lt;Nazwa_reguły&gt; &lt;lokalizacji&gt; &lt;grupa zasobów&gt; &lt;Rozmiar_okna&gt; &lt;operator&gt; &lt;próg&gt; &lt;targetResourceId&gt; &lt;metricName&gt; &lt;timeAggregationOperator&gt; *

    Poniższy przykład przedstawia się alert na zasób witryny sieci web. Alert wyzwalacze przy każdym spójne odbiera ruch 5 minut i ponownie po odebraniu żaden ruch przez 5 minut.

    ```console
    azure insights alerts rule metric set myrule eastus myreasourcegroup PT5M GreaterThan 2 /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename BytesReceived Total

    ```

5. Aby utworzyć webhook i wysyłanie wiadomości e-mail po alertu, należy najpierw utworzyć wiadomość e-mail i/lub webhooks. Następnie utwórz regułę natychmiast później. Nie można skojarzyć webhook lub wiadomości e-mail z już utworzone reguły przy użyciu interfejsu wiersza polecenia.

    ```console
    azure insights alerts actions email create --customEmails myemail@contoso.com

    azure insights alerts actions webhook create https://www.contoso.com

    azure insights alerts rule metric set myrulewithwebhookandemail eastus myreasourcegroup PT5M GreaterThan 2 /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename BytesReceived Total
    ```


6. Aby utworzyć alert, który jest uruchamiana, na określony warunek w dzienniku aktywności, za pomocą formularza:

    **reguły alertów wniosków logowania zestawu** *[opcje] &lt;Nazwa_reguły&gt; &lt;lokalizacji&gt; &lt;grupa zasobów&gt; &lt;operationName&gt; *

    Na przykład

    ```console
    azure insights alerts rule log set myActivityLogRule eastus myresourceGroupName Microsoft.Storage/storageAccounts/listKeys/action
    ```

    OperationName odnosi się do typu zdarzenia wpisu w dzienniku aktywności. Jako przykład można wymienić *Microsoft.Compute/virtualMachines/delete* i *microsoft.insights/diagnosticSettings/write*.

    Za pomocą polecenia programu PowerShell [Get-AzureRmProviderOperation](https://msdn.microsoft.com/library/mt603720.aspx) Aby uzyskać listę możliwych operationNames. Alternatywnie służy Azure portal kwerendy dziennika aktywności i znajdowanie określonych wcześniejszych operacji, które chcesz utworzyć alert dla. Operacje wyświetlane w widoku dziennika grafikę przyjazne nazwy. Szukanie JSON wpisu i wysunąć wartość OperationName.   

7. Można sprawdzić, czy alerty zostały utworzone prawidłowo, wyświetlając w poszczególnych reguł.

    ```console
    azure insights alerts rule list myresourcegroup --ruleName myrule
    ```

8. Aby usunąć reguły, użyj polecenia formularza:

    **Usuwanie reguły alertów wniosków** [opcje] &lt;grupa zasobów&gt; &lt;Nazwa_reguły&gt;

    Te polecenia Usuń reguły utworzone wcześniej w tym artykule.

    ```console
    azure insights alerts rule delete myresourcegroup myrule
    azure insights alerts rule delete myresourcegroup myrulewithwebhookandemail
    azure insights alerts rule delete myresourcegroup myActivityLogRule
    ```



## <a name="next-steps"></a>Następne kroki

* [Zapoznaj się z omówieniem Azure monitorowania](monitoring-overview.md) , w tym typów informacji można zbierać i monitorować.
* Dowiedz się więcej na temat [konfigurowania webhooks alertów](insights-webhooks-alerts.md).
* Dowiedz się więcej na temat [Runbooks automatyzacji Azure](..\automation\automation-starting-a-runbook.md).
* Zapoznaj się z [omówieniem zbierania dzienników diagnostycznych](monitoring-overview-of-diagnostic-logs.md) zbieranie szczegółowe metryki wysokiej częstotliwości od usługi.
* Zapoznaj się z [Omówienie metryki zbioru](insights-how-to-customize-monitoring.md) , aby upewnić się, że usługi jest dostępny i odpowiada.
