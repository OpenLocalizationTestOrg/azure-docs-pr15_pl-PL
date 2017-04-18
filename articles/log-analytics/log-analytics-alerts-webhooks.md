<properties
   pageTitle="Przykładowy alert webhook analizy dziennika"
   description="Jedną z czynności, które można uruchamiać w odpowiedzi na alert analizy dziennika jest *webhook*, dzięki czemu można wywołać proces zewnętrzny za pomocą pojedynczego żądania HTTP. W tym artykule opisano przykład tworzeniu akcji webhook w alercie analizy dziennika, przy użyciu zapas czasu."
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
   ms.date="10/27/2016"
   ms.author="bwren" />

# <a name="webhooks-in-log-analytics-alerts"></a>Webhooks alertów analizy dziennika

Jedną z czynności, które można uruchamiać w odpowiedzi na [alert analizy dziennika](log-analytics-alerts.md) jest *webhook*, dzięki czemu można wywołać proces zewnętrzny za pomocą pojedynczego żądania HTTP.  Informacje o szczegóły dotyczące alertów i webhooks alertów [w analizy dziennika](log-analytics-alerts.md)

W tym artykule przedstawimy każde z przykładem tworzenia akcji webhook w alert analizy dziennika, przy użyciu zapas czasu, który jest usługą obsługi wiadomości.

>[AZURE.NOTE] Musisz mieć konto zapasu czasu, aby zakończyć w tym przykładzie.  Możesz zalogować bezpłatne konto u [slack.com](http://slack.com).

## <a name="step-1---enable-webhooks-in-slack"></a>Krok 1 — Włącz webhooks w zapas czasu
2.  Zaloguj się do zapas czasu na [slack.com](http://slack.com).
3.  W obszarze **kanały** w okienku po lewej stronie, wybierz kanał.  Jest to kanał, który wiadomość zostanie wysłana do.  Możesz wybrać jeden z kanałów domyślne takich jak **Ogólne** lub **losowe**.  W środowisku produkcyjnym należy prawdopodobnie utworzyć kanał specjalne, takie jak **criticalservicealerts**. <br>

    ![Kanały zapasu czasu](media/log-analytics-alerts-webhooks/oms-webhooks01.png)

3. Kliknij pozycję **Dodaj aplikację lub Niestandardowa integracja** Otwórz katalog aplikacji.
3.  W polu wyszukiwania wpisz *webhooks* , a następnie wybierz pozycję **WebHooks przychodzących**. <br>

    ![Kanały zapasu czasu](media/log-analytics-alerts-webhooks/oms-webhooks02.png)

4.  Kliknij przycisk **Zainstaluj** obok Twojej nazwy zespołu.
5.  Kliknij przycisk **Dodaj konfiguracji**.
6.  Wybierz kanał, który zamierzasz użyć w tym przykładzie, a następnie kliknij **integracji dodać WebHooks przychodzących**.  
6. Skopiuj adres **Webhook URL**.  Będzie można wklejania tekstu to do konfiguracji alertu. <br>

    ![Kanały zapasu czasu](media/log-analytics-alerts-webhooks/oms-webhooks05.png)

## <a name="step-2---create-alert-rule-in-log-analytics"></a>Krok 2 — Tworzenie alertu w analizy dziennika
1.  [Utwórz regułę alertu](log-analytics-alerts.md) z poniższych ustawień.
    - Kwerenda:```    Type=Event EventLevelName=error ```
    - Sprawdzanie dla tego alertu każdej: 5 minut
    - Liczba wyników wynosi: większe niż 10
    - W tym oknie: 60 minut
    - Wybierz pozycję **Tak** **Webhook** i **bez** innych akcji.
7. Wklej adres URL zapasu czasu w polu **Adres URL Webhook** .
8. Wybierz opcję, aby **uwzględnić niestandardowy ładunku JSON**.
9. Zapas oczekuje ładunku sformatowane w formacie JSON parametr o nazwie *tekstu*.  To jest tekst, który będzie wyświetlany w wiadomości, które tworzy.  Można użyć jednej lub większej liczby parametry alertu za pomocą *#* symboli, takich jak w poniższym przykładzie.

    ```
    {
    "text":"#alertrulename fired with #searchresultcount records which exceeds the over threshold of #thresholdvalue ."
    }
    ```

    ![przykład JSON ładunku](media/log-analytics-alerts-webhooks/oms-webhooks07.png)

9.  Kliknij przycisk **Zapisz** , aby zapisać regułę alertu.

10. Poczekaj wystarczającą ilość czasu na alert można utworzyć, a następnie zaznacz zapas czasu dla wiadomości, która będzie podobny do następującego.

    ![przykład webhook w zapas czasu](media/log-analytics-alerts-webhooks/oms-webhooks08.png)


### <a name="advanced-webhook-payload-for-slack"></a>Zaawansowane ładunku webhook dla zapas czasu

Możesz dostosować dokładnie wiadomości przychodzących z zapas czasu. Aby uzyskać więcej informacji zobacz [Webhooks przychodzące](https://api.slack.com/incoming-webhooks) w witrynie sieci Web zapasu czasu. Bardziej złożone ładunku, aby utworzyć wiadomość sformatowany przy użyciu formatowania jest następujący:

    {
        "attachments": [
            {
                "title":"OMS Alerts Custom Payload",
                "fields": [
                    {
                        "title": "Alert Rule Name",
                        "value": "#alertrulename"},
                    {
                        "title": "Link To SearchResults",
                        "value": "<#linktosearchresults|OMS Search Results>"},
                    {
                        "title": "Search Interval",
                        "value": "#searchinterval"},
                    {
                        "title": "Threshold Operator",
                        "value": "#thresholdoperator"},
                    {
                        "title": "Threshold Value",
                        "value": "#thresholdvalue"}
                ],
                "color": "#F35A00"
            }
        ]
    }


To samo wygenerowanie wiadomości w zapas czasu podobny do następującego.

![Przykładowy komunikat w zapas czasu](media/log-analytics-alerts-webhooks/oms-webhooks09.png)

## <a name="summary"></a>Podsumowanie

Przy użyciu tej reguły alertu w miejscu trzeba na wiadomość wysłaną do zapas czasu za każdym razem, gdy kryteria są spełnione.  

To jest tylko jeden przykład akcją, którą można utworzyć w odpowiedzi na alert.  Można utworzyć akcję webhook, która nawiąże połączenie z inną usługą zewnętrznych, akcja działań aranżacji, aby rozpocząć działań aranżacji w automatyzacji Azure lub akcję poczty e-mail do wysyłania poczty e-mail do siebie lub innych adresatów.   

## <a name="next-steps"></a>Następne kroki

- Dowiedz się więcej o [alerty w dzienniku analizy](log-analytics-alerts.md) , w tym inne akcje.
- [Tworzenie runbooks w automatyzacji Azure](../automation/automation-webhooks.md) nazywanym z webhook.
