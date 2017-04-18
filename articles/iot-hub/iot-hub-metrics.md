<properties
 pageTitle="Centrum IoT metryki diagnostyczne"
 description="Omówienie Centrum IoT Azure miar, umożliwiające użytkownikom oceny ogólnej kondycji ich zasobów"
 services="iot-hub"
 documentationCenter=""
 authors="nberdy"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="08/11/2016"
 ms.author="nberdy"/>

# <a name="introduction-to-diagnostic-metrics"></a>Wprowadzenie do wskaźników diagnostyczne

Metryki diagnostyczne umożliwiają lepsze dane o stanie zasobów Azure w ramach subskrypcji Azure. Metryki umożliwiają oceny ogólnej kondycji usługi i urządzeń podłączonych do niego. Statystyki widocznych dla użytkownika są ważne, ponieważ ułatwiają wyświetlanie, co się dzieje z IoT Centrum i pomoc przyczyn problemów związanych z bez konieczności kontakt z pomocą techniczną Azure.

Możesz włączyć diagnostyczne metryki z portalu Azure.

## <a name="how-to-enable-diagnostic-metrics"></a>Jak włączyć metryki diagnostyczne

1. Tworzenie Centrum IoT. Może znaleźć instrukcje na temat tworzenia Centrum IoT w [Wprowadzenie] [ lnk-get-started] przewodnika.

2. Otwórz karta z Twoim Centrum IoT. W tym miejscu kliknij pozycję **Diagnostyka**.

    ![][1]

3. Konfigurowanie usługi diagnostyki ustawienie stanu **na** i wybierając konta magazynu platformy Azure do przechowywania danych diagnostyki. Sprawdzanie **metryki**, a następnie naciśnij klawisz **Zapisywanie**. Należy zauważyć, że wyprzedzeniem musi zostać utworzone konto Azure miejsca do magazynowania i że jest naliczany oddzielnie do przechowywania. Możesz również wybrać wysyłanie danych diagnostyki do punktu końcowego koncentratory zdarzenia.

    ![][2]

4. Po skonfigurowaniu Diagnostyka powróć do karta Centrum IoT **Przegląd** . Metryki informacji znajduje się w sekcji **Monitorowanie** karta. Klikając wykres zostanie wyświetlona w okienku metryki miejsce, w którym można wyświetlić podsumowanie informacji metryki do koncentratora IoT i edytować zaznaczenia metryki wyświetlane na wykresie. Można również skonfigurować alerty na podstawie wartości metryki.

    ![][3]

## <a name="metrics-and-how-to-use-them"></a>Metryki i sposobach ich używania

Centrum IoT udostępnia kilka miar, aby zapewnić sobie omówienie kondycji usługi Centrum i łącznej liczby urządzeń podłączonych do niego. Można połączyć informacje z kilku metryk Aby rysować większy obraz stanu Centrum IoT. W poniższej tabeli opisano metryki, który śledzi każdego centrum IoT i jak każda Metryka odnosi się do Centrum IoT ogólny stan.

| Metryka | Opis metryczne | Metryka służy do |
| ---- | ---- | ---- |
| d2c.telemetry.ingress.allProtocol | Liczba wiadomości wysyłane na wszystkich urządzeniach | Omówienie danych na wysyła wiadomość |
| d2c.telemetry.ingress.SUCCESS | Liczba wszystkich wiadomości pomyślnego do koncentratora | Omówienie bryzgów pomyślnego wiadomości do koncentratora |
| c2d.Commands.egress.COMPLETE.SUCCESS | Liczba wszystkich wiadomości polecenie wykonane przez urządzenia odbieraniem na wszystkich urządzeniach | Razem z metryki na abandon i Odrzuć zestawienie ogólnego wskaźnika sukcesu polecenie C2D |
| c2d.Commands.egress.Abandon.SUCCESS | Liczba wszystkich wiadomości pomyślnie zrzeczenia się odbierania urządzenie na wszystkich urządzeniach | Jeśli wiadomości są wprowadzenie porzucone częściej niż przewidywano są wyróżniane potencjalne problemy |
| c2d.Commands.egress.Reject.SUCCESS | Liczba wszystkich wiadomości pomyślnie odrzucona przez urządzenia odbieraniem na wszystkich urządzeniach | Jeśli wiadomości zostały wprowadzenie odrzucone częściej niż oczekiwano są wyróżniane potencjalne problemy |
| devices.totalDevices | Średnia, minimum i maksymalna liczba urządzeń zarejestrowane w Centrum IoT | Liczba urządzeń zarejestrowanych do koncentratora |
| devices.connectedDevices.allProtocol | Średnia, minimum i maksimum liczby jednoczesnego urządzeń podłączonych | Omówienie liczby urządzeń podłączone do koncentratora |

## <a name="next-steps"></a>Następne kroki

Teraz, gdy przeczytane omówienie diagnostyczne metryki, zrób to łącze, aby dowiedzieć się więcej o zarządzaniu Centrum IoT Azure:

- [Operacje monitorowania][lnk-monitor]

Aby jeszcze bardziej Poznawanie możliwości Centrum IoT, zobacz:

- [Przewodnik dewelopera][lnk-devguide]
- [Symulowanie urządzenia z IoT SDK bramy][lnk-gateway]

<!-- Links and images -->
[1]: media/iot-hub-metrics/enable-metrics-1.png
[2]: media/iot-hub-metrics/enable-metrics-2.png
[3]: media/iot-hub-metrics/enable-metrics-3.png

[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[lnk-operations-monitoring]: iot-hub-operations-monitoring.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-dr]: iot-hub-ha-dr.md

[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
