> [AZURE.SELECTOR]
- [Node.js](../articles/iot-hub/iot-hub-node-node-twin-how-to-configure.md)
- [C#](../articles/iot-hub/iot-hub-csharp-node-twin-how-to-configure.md)

## <a name="introduction"></a>Wprowadzenie

W [Wprowadzenie do Centrum IoT twins][lnk-twin-tutorial], jak ustawić urządzenie meta dane z swoje rozwiązanie back-end przy użyciu *tagów*, warunki urządzenia raportu z aplikacji urządzenia, za pomocą *Właściwości zgłoszonych*oraz kwerendy te informacje przy użyciu języka SQL like.

W tym samouczku dowiesz się jak używać twin *żądane właściwości* w połączeniu z *zgłaszane właściwości*, aby zdalnie skonfigurować aplikacje urządzenia. W szczególności ten samouczek przedstawia sposób zgłaszania twin i żądane właściwości Włącz konfigurację wieloetapowy urządzenia ustawienia aplikacji i wgląd do rozwiązania back-end stanu tej operacji na wszystkich urządzeniach.

Na wysokim poziomie ten samouczek następuje *wzór pożądanego stanu* dla zarządzania urządzeniami. Podstawowych idei tego wzorca jest rozwiązanie back-end Określ żądany stan dla zarządzanych urządzeń, zamiast wysyłać określonych poleceń. Spowoduje to umieszczenie urządzenia odpowiedzialne za ustanowienie najlepszym sposobem, aby osiągnąć pożądany stan (bardzo ważne w scenariuszach IoT, gdzie warunki określone urządzenie wpływa na możliwość natychmiast przeprowadzić określone polecenia), podczas raportowania stale do back-end bieżący stan i potencjalnych błędów. Wzór żądany stan jest zarządzaniem dużych zestawów urządzeń, ponieważ umożliwia back-end mają pełną widoczność stan procesu konfiguracji na wszystkich urządzeniach.
Więcej informacji na temat roli wzorca pożądanego stanu można znaleźć w zarządzanie urządzeniami w [Zarządzanie urządzeniami omówienie Azure Centrum IoT][lnk-dm-overview].

> [AZURE.NOTE] W scenariuszach, w przypadku gdy urządzenia są kontrolowane w sposób bardziej interaktywną (Włącz wentylator z aplikacji kontrolowane przez użytkownika), należy rozważyć użycie [urządzenia cloud metody][lnk-methods].

W tym samouczku zmian back-end aplikacji prowadnic urządzenia docelowego w wyniku tego, aplikacja urządzenia następuje wieloetapowy proces do zastosowania aktualizacji konfiguracji (np. wymagające ponownego uruchomienia modułu oprogramowania), który symuluje ten samouczek z opóźnieniem prosty).

Back-end przechowuje konfigurację w twin urządzenia żądane właściwości w następujący sposób:

        {
            ...
            "properties": {
                ...
                "desired": {
                    "telemetryConfig": {
                        "configId": "{id of the configuration}",
                        "sendFrequency": "{config}"
                    }
                }
                ...
            }
            ...
        }

> [AZURE.NOTE] Ponieważ konfiguracje mogą zostać złożone obiekty, są zwykle przypisywane unikatowe identyfikatory (wartości skrótu lub [identyfikatory GUID][lnk-guid]) do uproszczenia ich porównania.

Aplikacja urządzenia raportów aktualnej konfiguracji dublowanie żądaną właściwość **telemetryConfig** we właściwościach zgłoszonych:

        {
            "properties": {
                ...
                "reported": {
                    "telemetryConfig": {
                        "changeId": "{id of the current configuration}",
                        "sendFrequency": "{current configuration}",
                        "status": "Success",
                    }
                }
                ...
            }
        }

Należy zwrócić uwagę, jak zgłoszonych **telemetryConfig** ma dodatkowe właściwości **stanu**, używany do raportowania stan procesu aktualizacji konfiguracji.

Po odebraniu nowej konfiguracji żądaną aplikację urządzenia raportów oczekujące konfiguracji dzięki zmianie informacji:

        {
            "properties": {
                ...
                "reported": {
                    "telemetryConfig": {
                        "changeId": "{id of the current configuration}",
                        "sendFrequency": "{current configuration}",
                        "status": "Pending",
                        "pendingConfig": {
                            "changeId": "{id of the pending configuration}",
                            "sendFrequency": "{pending configuration}"
                        }
                    }
                }
                ...
            }
        }

Następnie na późniejszym etapie, aplikacja urządzenia zgłosi powodzenia niepowodzenia tej operacji aktualizując powyżej właściwości.
Należy zwrócić uwagę, jak back-end jest w stanie, w dowolnym momencie, aby sprawdzić stan procesu konfiguracji na wszystkich urządzeniach.

Ten samouczek pokazuje, jak do:

- Utworzyć symulowanego urządzenia, który odbiera aktualizacji konfiguracji z back-end i raportuje wiele aktualizacji jako *Właściwości zgłoszonych* na proces aktualizacji konfiguracji.
- Tworzenie aplikacji zaplecza, który aktualizuje żądaną konfiguracją urządzenia, a następnie kwerendę proces aktualizacji konfiguracji.

<!-- links -->

[lnk-methods]: ../articles/iot-hub/iot-hub-devguide-direct-methods.md
[lnk-dm-overview]: ../articles/iot-hub/iot-hub-device-management-overview.md
[lnk-twin-tutorial]: ../articles/iot-hub/iot-hub-node-node-twin-getstarted.md
[lnk-guid]: https://en.wikipedia.org/wiki/Globally_unique_identifier