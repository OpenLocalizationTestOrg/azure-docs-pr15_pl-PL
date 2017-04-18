<properties 
   pageTitle="Informować użytkowników o dane otrzymane od czujniki lub inne systemy | Microsoft Azure"
   description="Informacje dotyczące używania koncentratory zdarzenia informować użytkowników o danych czujnika."
   services="event-hubs"
   documentationCenter="na"
   authors="spyrossak"
   manager="timlt"
   editor="" />
<tags 
   ms.service="event-hubs"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/25/2016"
   ms.author="spyros;sethm" />

# <a name="notify-users-of-data-received-from-sensors-or-other-systems"></a>Informować użytkowników o dane otrzymane od czujniki lub inne systemy

Załóżmy, że masz aplikację, która monitoruje danych w czasie rzeczywistym i tworzy raporty zgodnie z harmonogramem. Podczas przeglądania w sieci Web, na którym są wyświetlane te w czasie rzeczywistym wykresów lub raportów, może zostać wyświetlony coś, co wymaga wykonania czynności. Co zrobić, jeśli musisz być dotyczy alert do tych sytuacji, zamiast uzależnionej na dokonywaniu sprawdzanie witryny sieci Web? Załóżmy, że masz światło zwiększania w cieplarnianych i należy od razu wiedzieć, jeśli uproszczonej przechodzi. Jednym ze sposobów formatowania będzie z uproszczonej czujnikiem szklarni, rozmieszczanie do wysłania wiadomości e-mail, jeśli uproszczonej jest wyłączona.

![][1]

W innym scenariuszu Załóżmy że Uruchom domowych pomieszczenia wejściowej, a użytkownik musi otrzymywać alerty o niskiej dostawy zapasów. Na przykład możesz rozmieścić wysłania wiadomości SMS w systemie ERP usługi inwentarz jedzenie PSA ma stanie się do poziomu krytyczne. 

![][2]

Problem przedstawiono sposób uzyskiwania ważnych informacji, gdy są spełnione określone warunki, nie, gdy pojawi się do wyewidencjonowania statyczne raportu. Jeśli korzystasz z [Koncentratora zdarzenia Azure][] lub [Centrum IoT Azure][] do odbierania danych z urządzeń i aplikacji przedsiębiorstwa, takich jak [Dynamics AX][], masz kilka opcji sposobu ich przetwarzania. Ich wyświetlania w witrynie sieci Web, można analizować, można przechowywać i używać ich wyzwalać poleceń, aby zrobić coś. Do tego służy zaawansowanych narzędzi, takich jak [Azure witryn sieci Web][], [Platformy SQL Azure][], [HDInsight][], [Cortany analizy pakiet][], [Pakiet IoT][], [Logiki aplikacji][]lub [Koncentratory powiadomienie Azure][]. Jednak czasami wszystko, co chcesz zrobić jest wysyłanie danych do kogoś z co najmniej ogólnych. Aby pokazać, jak to zrobić przy użyciu tylko niewielki kod, udostępniamy nowej próbce, [AppToNotifyUsers][]. Opcje są wiadomości e-mail (SMTP), usługi SMS i telefonu.

## <a name="application-structure"></a>Struktury aplikacji

Aplikacja jest zapisywany w języku C#, a plik readme w próbce zawiera wszystkie informacje, które należy zmodyfikować, tworzenie i publikowanie aplikacji. Omówienie działanie aplikacji można znaleźć w poniższych sekcjach.

Możemy uruchomić przy założeniu, że jest przypisany do koncentratora zdarzenia Azure lub Centrum IoT zdarzenia krytyczne. Wszelkie Centrum wykona, ile masz dostęp do niego i znać parametry połączenia.

Jeśli nie masz już Centrum zdarzenie lub IoT koncentratora, łatwo można skonfigurować badawczym z tarczą Arduino i Pi malina, postępując zgodnie z instrukcjami w programie project [Łączenie kropki](https://github.com/Azure/connectthedots) . Czujnik światła na tarczą Arduino wysyła uproszczonej poziomy za pośrednictwem Pi do [Centrum zdarzeń Azure][] (**ehdevices**), a zadanie [Analizy strumieniu Azure](https://azure.microsoft.com/services/stream-analytics/) umieszcza alerty do drugiego koncentratora zdarzenia (**ehalerts**), jeśli poziom oświetlenia odebrana poniżej pewnego poziomu.

Po uruchomieniu **AppToNotify** odczytuje plik konfiguracji (App.config), aby uzyskać adres URL i poświadczenia dla Centrum zdarzeń otrzymywania alertów. Proces stale monitorować, że Centrum zdarzenia dla każdej wiadomości, która zawiera za pośrednictwem — o ile masz dostęp do adresu URL Centrum zdarzenie lub IoT koncentratora i prawidłowe poświadczenia, kod czytnika koncentratory zdarzenia odczyta przez cały czas, o nadchodzących jej następnie spawns. Podczas uruchamiania aplikacji odczytuje również adresu URL i poświadczenia dla usługi SMS (poczta e-mail, SMS, telefon) ma być używany, a nazwa i adres nadawcy i lista adresatów.

Gdy monitor Centrum zdarzeń wykryje wiadomość, uruchamia proces wysyłania wiadomości przy użyciu metody określonej w pliku konfiguracji. Zauważ, że wysyła każdej wiadomości, które wykryje. Jeśli ustawisz monitor, aby wskazywały do koncentratora zdarzenia, które otrzymuje dziesięć wiadomości na sekundę, nadawca wyśle dziesięć wiadomości na sekundę — dziesięć wiadomości na sekundę, wiadomości SMS dziesięć na sekundę, dziesięć połączenia telefoniczne na sekundę. Z tego powodu upewnij się, monitorowanie koncentratora zdarzenia, tylko do odbierania alertów, które muszą być wysyłane się nie koncentratora zdarzenia odbierająca nieprzetworzonych danych z czujników lub aplikacji.

## <a name="applicability"></a>Stosowanie

W tym przykładzie tylko kod sposobu monitorować wydarzenie koncentratory i połączeń zewnętrznych usług wiadomości, w przypadku, gdy chcesz dodać tę funkcję w aplikacji. Należy zauważyć, że to rozwiązanie DIY, oparte na Deweloper przykład tylko. Nie dotyczy wymagania przedsiębiorstwa, takie jak nadmiarowości, przełączaniem, uruchom ponownie po błąd itd. Więcej rozwiązań kompleksowe i produkcji zobacz następujące artykuły:

- Za pomocą łączników lub powiadomień wypychanych przy użyciu usługi [Azure logiczny aplikacji](../app-service-logic/app-service-logic-connectors-list.md) .
- Za pomocą [Koncentratorów powiadomienie Azure](https://msdn.microsoft.com/library/azure/jj927170.aspx), zgodnie z opisem blogu [powiadomienia wypychane emisji do milionów urządzeń przenośnych za pomocą koncentratorów powiadomienie Azure](http://weblogs.asp.net/scottgu/broadcast-push-notifications-to-millions-of-mobile-devices-using-windows-azure-notification-hubs). 

## <a name="next-steps"></a>Następne kroki

Jest proste utworzyć usługa zwykłego powiadomienia wysyłane do adresatów wiadomości e-mail i wiadomości SMS lub połączenia je do przekazywania danych odebrana przez Centrum zdarzenie lub Centrum IoT. Aby wdrożyć rozwiązanie informować użytkowników na podstawie odebranych przez te koncentratory, odwiedź stronę [AppToNotifyUsers][].

Aby uzyskać więcej informacji o tych koncentratory zobacz następujące artykuły:

- [Koncentratory Azure zdarzenia]
- [Centrum IoT Azure]
- Rozpoczynanie pracy z [samouczka koncentratory zdarzenia].
- Ukończono [przykładowej aplikacji, która korzysta z koncentratorów zdarzenia].

Aby wdrożyć rozwiązanie informować użytkowników na podstawie danych odebranych przez te koncentratory, odwiedź stronę:

- [AppToNotifyUsers][]

[Samouczek koncentratory zdarzenia]: event-hubs-csharp-ephcs-getstarted.md
[Centrum IoT Azure]: https://azure.microsoft.com/services/iot-hub/
[Koncentratory Azure zdarzenia]: https://azure.microsoft.com/services/event-hubs/
[Centrum Azure zdarzenia]: https://azure.microsoft.com/services/event-hubs/
[Przykładowa aplikacja korzystającego z koncentratorów zdarzenia]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[AppToNotifyUsers]: https://github.com/Azure-Samples/event-hubs-dotnet-user-notifications
[Dynamics AX]: http://www.microsoft.com/dynamics/erp-ax-overview.aspx
[Azure witryn sieci Web]: https://azure.microsoft.com/services/app-service/web/
[Platformy SQL Azure]: https://azure.microsoft.com/services/sql-database/
[Usługa HDInsight]: https://azure.microsoft.com/services/hdinsight/
[Cortana analizy pakietu]: http://www.microsoft.com/server-cloud/cortana-analytics-suite/Overview.aspx?WT.srch=1&WT.mc_ID=SEM_lLFwOJm3&bknode=BlueKai
[Pakiet IoT]: https://azure.microsoft.com/solutions/iot-suite/
[Aplikacje warunków logicznych]: https://azure.microsoft.com/services/app-service/logic/
[Koncentratory Azure powiadomień]: https://azure.microsoft.com/services/notification-hubs/
[Azure Stream Analytics]: https://azure.microsoft.com/services/stream-analytics/
 
[1]: ./media/event-hubs-sensors-notify-users/event-hubs-sensor-alert.png
[2]: ./media/event-hubs-sensors-notify-users/event-hubs-erp-alert.png