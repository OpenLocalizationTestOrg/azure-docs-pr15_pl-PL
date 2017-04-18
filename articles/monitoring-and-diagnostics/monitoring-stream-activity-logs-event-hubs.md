<properties
    pageTitle="Przesyłanie strumieniowe Azure dziennik do koncentratorów zdarzenia | Microsoft Azure"
    description="Dowiedz się, jak przesyłać strumieniowo Azure dziennik do koncentratorów wydarzenie."
    authors="johnkemnetz"
    manager="rboucher"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="johnkem"/>

# <a name="stream-the-azure-activity-log-to-event-hubs"></a>Strumień Azure dziennik do koncentratorów zdarzenia
[**Dziennik Azure**](./monitoring-overview-activity-logs.md) można strumieniowo w czasie rzeczywistym najbliższego do dowolnej aplikacji przy użyciu wbudowanych opcji "Eksportuj", w portalu lub, umożliwiając identyfikator reguły Bus usługi w profilu dziennika przy użyciu poleceń cmdlet środowiska PowerShell Azure lub polecenie Azure.

## <a name="what-you-can-do-with-the-activity-log-and-event-hubs"></a>Co można zrobić z dziennika aktywności i koncentratory zdarzenia
Oto kilka sposobów, można użyć funkcji strumieniowych w dzienniku aktywności:

- **Strumienia systemów rejestrowania i telemetrycznego innych firm** — czasem koncentratory zdarzenia streaming będzie mechanizmu potoku dziennika aktywności do innych firm SIEMs i zalogować się rozwiązań analizy.
- **Tworzenie niestandardowego telemetrycznego i rejestrowanie platformy** — Jeśli już masz platformy telemetrycznego niestandardowej lub są tylko Planowanie tworzenia jedną, wysoce skalowalna publikowanie Subskrybuj rodzaj zdarzenia koncentratory umożliwia elastycznie mogły zjeść tej ostatniej dziennik. [Zobacz Przewodnik po Rosanova Paweł za pomocą koncentratorów zdarzenia w skali globalnej platformy telemetrycznego tutaj.](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/)

## <a name="enable-streaming-of-the-activity-log"></a>Włącz przesyłanie strumieniowe dziennika aktywności
Możesz włączyć streaming dziennika aktywności programowo lub za pośrednictwem portalu. W obu przypadkach wybierz Namespace Bus usługi i zasadę dostępu udostępnione dla tego obszaru nazw i Centrum zdarzeń jest tworzona w tym nazw pierwszego nowego zdarzenia dziennika aktywności. Jeśli nie masz Namespace Bus usługi, należy najpierw utworzyć. Jeśli masz już strumieniowo dziennik zdarzeń Namespace Bus tej usługi, Centrum zdarzenia, który został wcześniej utworzony zostanie ponownie. Zasady dostępu udostępnione określa uprawnienia, które ma mechanizmu przesyłanie strumieniowe. Dzisiaj streaming do koncentratorów zdarzenia musi mieć uprawnienie **Zarządzanie**, **czytania**i **wysyłania** . Czy tworzenie lub modyfikowanie zasady dostępu Namespace Bus usług udostępnionych w portalu klasyczny na karcie "Konfigurowanie" dla swojego Namespace Bus usługi. Aby zaktualizować profil dziennika dziennika aktywności, aby uwzględnić streaming, użytkownik wprowadzi tę zmianę musi mieć uprawnienie ListKey dla tej reguły autoryzacji Bus usługi.

### <a name="via-azure-portal"></a>Za pośrednictwem portalu Azure 
1. Przejdź do karta **Dziennik** przy użyciu menu po lewej stronie portalu.

    ![Przejdź do dziennika aktywności w portalu](./media/monitoring-overview-activity-logs/activity-logs-portal-navigate.png)
2. Kliknij przycisk **Eksportuj** w górnej części karta.

    ![Przycisk Eksportuj w portalu](./media/monitoring-overview-activity-logs/activity-logs-portal-export.png)
3. W kartę, która jest wyświetlana można wybrać regionów, dla których chcesz strumienia wydarzeń i Namespace Bus usługę, w którym chcesz koncentratora wydarzenie ma zostać utworzony przesyłania strumieniowego te zdarzenia.

    ![Karta dziennik eksportu](./media/monitoring-overview-activity-logs/activity-logs-portal-export-blade.png)
4. Kliknij przycisk **Zapisz** , aby zapisać ustawienia. Ustawienia są natychmiast zastosowane do swojej subskrypcji.


### <a name="via-powershell-cmdlets"></a>Za pomocą poleceń cmdlet programu PowerShell
Jeśli profil dziennika, który już istnieje, należy najpierw usunąć tego profilu.

1. Używanie `Get-AzureRmLogProfile` Aby ustalić, czy istnieje profil dziennika
2. Jeśli tak, za pomocą `Remove-AzureRmLogProfile` go usunąć.
3. Używanie `Set-AzureRmLogProfile` utworzyć profil:

```
Add-AzureRmLogProfile -Name my_log_profile -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Locations global,westus,eastus -RetentionInDays 90 -Categories Write,Delete,Action
```

Identyfikator reguły Bus usługi jest ciągiem o następującym formacie: {usługi identyfikator zasobu bus} /authorizationrules/ {nazwa klucza}, na przykład 

### <a name="via-azure-cli"></a>Za pomocą interfejsu wiersza polecenia Azure
Jeśli profil dziennika, który już istnieje, należy najpierw usunąć tego profilu.

1. Używanie `azure insights logprofile list` Aby ustalić, czy istnieje profil dziennika
2. Jeśli tak, za pomocą `azure insights logprofile delete` go usunąć.
3. Używanie `azure insights logprofile add` utworzyć profil:

```
azure insights logprofile add --name my_log_profile --storageId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/my_storage --serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey --locations global,westus,eastus,northeurope --retentionInDays 90 –categories Write,Delete,Action
```

Identyfikator reguły Bus usługi jest ciągiem o następującym formacie: `{service bus resource ID}/authorizationrules/{key name}`.
 
## <a name="how-do-i-consume-the-log-data-from-event-hubs"></a>Jak korzystać z danych dziennika z koncentratorów zdarzenia?
[Schemat dziennika aktywności jest dostępna w tym miejscu](./monitoring-overview-activity-logs.md). Każde zdarzenie jest w tablicy obiektów blob JSON o nazwie "rekordów".

## <a name="next-steps"></a>Następne kroki
- [Archiwizowanie dziennika aktywności na koncie miejsca do magazynowania](./monitoring-archive-activity-log.md)
- [Zapoznanie się z omówieniem dziennik Azure](./monitoring-overview-activity-logs.md)
- [Konfigurowanie alertów na podstawie zdarzenia dziennika aktywności](./insights-auditlog-to-webhook-email.md)
