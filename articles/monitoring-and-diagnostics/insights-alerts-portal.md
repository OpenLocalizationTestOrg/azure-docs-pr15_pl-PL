<properties
    pageTitle="Tworzenie alertów dla usług Azure za pomocą Azure portal | Microsoft Azure"
    description="Azure portal umożliwia tworzenie alertów Azure, które może spowodować powiadomienia lub automatyzacji, gdy są spełnione określone warunki."
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
    ms.date="09/23/2016"
    ms.author="robb"/>

# <a name="use-azure-portal-to-create-alerts-for-azure-services"></a>Tworzenie alertów dla usług Azure za pomocą Azure portal

> [AZURE.SELECTOR]
- [Portal](insights-alerts-portal.md)
- [Programu PowerShell](insights-alerts-powershell.md)
- [POLECENIE](insights-alerts-command-line-interface.md)

## <a name="overview"></a>Omówienie

W tym artykule pokazano, jak skonfigurować alerty Azure za pomocą portalu Azure.   

Możesz otrzymywać alertu na podstawie monitorowania metryki dla lub zdarzeniami na usługi Azure.

- **Wartości metryki** - wyzwalaczy alertu, gdy wartość określonej metryki przecina progu, które można przypisać w dowolnym kierunku. Oznacza to, że wyzwalane oba po spełnienia warunku, a następnie później gdy warunek jest już nie są spełnione.    
- **Zdarzeń dziennika aktywności** — alert może spowodować na *każde* zdarzenie lub, tylko w przypadku, gdy występują pewne zdarzenia.


Możesz skonfigurować alert, wykonaj następujące czynności, gdy go wyzwalane:

- Wysyłanie powiadomienia e-mail do administratora usługi i administratorów współpracujących
- Wysyłanie wiadomości e-mail do kolejnych wiadomości przez użytkownika.
- połączenie webhook
- rozpoczęcia realizacji Azure działań aranżacji (tylko z Azure portal)

Można skonfigurować i uzyskiwanie informacji na temat reguł alertów przy użyciu

- [Azure portal](insights-alerts-portal.md)
- [Programu PowerShell](insights-alerts-powershell.md)
- [Interfejs wiersza polecenia (polecenie)](insights-alerts-command-line-interface.md)
- [Monitorowanie Azure interfejsu API usługi REST](https://msdn.microsoft.com/library/azure/dn931945.aspx)


## <a name="create-an-alert-rule-on-a-metric-with-the-azure-portal"></a>Utwórz regułę alertu na metryki Portal Azure

1. W [portalu](https://portal.azure.com/)Znajdź zasobów interesuje monitorowania i zaznacz go.

2. Zaznacz **alertów** i **reguł alertów** w sekcji monitorowania. Tekst i ikona mogą się nieco różnić dla różnych zasobów.  

    ![Monitorowanie](./media/insights-alerts-portal/AlertRulesButton.png)


3. Wybierz polecenie **Dodaj alert** , a następnie wypełnij pola.

    ![Dodawanie alertu dotyczącego](./media/insights-alerts-portal/AddAlertOnlyParamsPage.png)

4. **Nazwa** alertu reguły, a następnie wybierz pozycję **Opis**, który zawiera również w wiadomości e-mail z powiadomieniem.
5. Wybierz pozycję **metryczne** chcesz monitorować, a następnie wybierz **warunek** i **Próg** wartość metryki. Także wybrać **okres** czasu metryczne reguły muszą być spełnione przed wyzwalacze alertów. Więc na przykład jeśli używasz okresu "PT5M" i alertu wyszukuje Procesora ponad 80%, alert uaktywnia Procesora był stale powyżej 80% przez 5 minut. Po wystąpieniu pierwszego wyzwalacza go ponownie uaktywnia Procesora pozostaje poniżej 80% przez 5 minut. Miary Procesora występuje co minutę.   

6. Zaznacz **... właściciele poczty E-mail** , jeśli chcesz Administratorzy i administratorów współpracujących przesłane pocztą e-mail po alert.

7. Dodatkowe wiadomości e-mail, aby odbierać powiadomienie, gdy jest uruchamiana, alert, należy dodać je w polu **email(s) dodatkowe administratora** . Oddzielić średnikami — wiele wiadomości*email@contoso.com;email2@contoso.com*

8. Jeśli chcesz, że ta nosi nazwę po alercie, należy umieścić w prawidłowy identyfikator URI w polu **Webhook** .

9. Użycie automatyzacji Azure, możesz wybrać działań aranżacji ma być uruchamiany przy alertu.

10. Wybierz przycisk **OK** po zakończeniu utworzyć alert.   

W ciągu kilku minut alert jest aktywna, a wyzwalane w sposób opisany wcześniej.

## <a name="managing-your-alerts"></a>Zarządzanie alertami

Po utworzeniu alertu, możesz je wybrać i:

- Wyświetl wykres przedstawiający progu metryczne i rzeczywiste wartości z poprzedniego dnia.
- Edytuj lub usuń go.
- **Wyłączanie** lub **Włączanie** je zatrzymać Jeśli chcesz, aby tymczasowo lub Życiorys otrzymywania powiadomień dla tego alertu.



## <a name="next-steps"></a>Następne kroki

* [Zapoznaj się z omówieniem Azure monitorowania](monitoring-overview.md) , w tym typów informacji można zbierać i monitorować.
* Dowiedz się więcej na temat [konfigurowania webhooks alertów](insights-webhooks-alerts.md).
* Dowiedz się więcej na temat [Runbooks automatyzacji Azure](..\automation\automation-starting-a-runbook.md).
* Zapoznaj się z [omówieniem dzienniki diagnostyczne](monitoring-overview-of-diagnostic-logs.md) i zebrać szczegółowe metryki wysokiej częstotliwości od usługi.
* Zapoznaj się z [Omówienie metryki zbioru](insights-how-to-customize-monitoring.md) , aby upewnić się, że usługi jest dostępny i odpowiada.
