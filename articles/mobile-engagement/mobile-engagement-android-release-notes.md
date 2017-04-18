<properties
    pageTitle="Integracja Android SDK Azure zaangażowania urządzeń przenośnych"
    description="Najnowszych aktualizacji i procedury dla systemu Android SDK dla zaangażowania Mobile Azure"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="piyushjo" />

# <a name="release-notes"></a>Informacje o wersji

## <a name="423-08102016"></a>4.2.3 (2016-08-10)

- Nie więcej blokady sieci Wi-Fi.
- Naprawianie zakleszczenie podczas połączenia getDeviceId przed inicjowania (błąd wprowadzone w 4.2.0).

## <a name="422-05172016"></a>4.2.2 (2016-05-17)

- Ulepszenia.

## <a name="421-05102016"></a>4.2.1 (2016-05-10)

- Zabezpieczenia: Wyłącz dostęp do plików lokalnych widoku sieci web.
- Zabezpieczenia: usuwanie `EngagementPreferenceActivity` klasa przestarzałych i niezabezpieczoną `PreferenceActivity` zajęć.
- Zabezpieczenia: reach umożliwia teraz opisano działania `exported="false"`, ta flaga można także we wcześniejszych wersjach SDK.

## <a name="420-03112016"></a>4.2.0 (2016-03-11)

- Zestaw SDK jest teraz licencją MIT.
- Zezwalaj na podawania identyfikatora niestandardowe urządzenie podczas inicjowania SDK.

## <a name="415-02012016"></a>4.1.5 (2016-02-01)

- Ulepszenia.

## <a name="414-01262016"></a>4.1.4 (2016-01-26)

- Ulepszenia.

## <a name="413-1292015"></a>4.1.3 (2015-12-9)

- Ulepszenia.

## <a name="412-11252015"></a>4.1.2 (2015-11-25)

- Ulepszenia.

## <a name="411-11042015"></a>4.1.1 (2015-11-04)

- Ulepszenia.

## <a name="410-08252015"></a>4.1.0 (2015-08-25)

- Uchwyt nowy model uprawnień dla systemu Android M.
- Teraz można skonfigurować funkcje lokalizacji w czasie wykonywania zamiast `AndroidManifest.xml`.
- Naprawić błąd uprawnień: Jeśli korzystasz z `ACCESS_FINE_LOCATION`, następnie `ACCESS_COARSE_LOCATION` nie jest już potrzebna.
- Ulepszenia.

## <a name="400-07062015"></a>4.0.0 (2015-07-06)

-   Protokół zmiany, aby analizy i wypychanych niezawodne.
-   Natywne wypychanych (GCM-ADM) teraz służy do w powiadomienia aplikacji, musisz skonfigurować poświadczenia natywnych wypychanych dla każdego typu wypychanych kampanii.
-   Naprawianie przeglądu powiadomienie: były wyświetlane tylko 10s po zostanie przeniesiony.
-   Naprawianie błędu w widoku sieci web: kliknięcie łącza również wykonywania domyślny adres URL akcji.
-   Naprawianie awarii rzadkich związanych z zarządzaniem magazynu lokalnego.
-   Naprawianie zarządzania ciąg dynamicznej konfiguracji.
-   Aktualizowanie umowę licencyjną użytkownika oprogramowania.

## <a name="300-02172015"></a>3.0.0 (2015-02-17)

-   Początkowy wersji Azure zaangażowania urządzeń przenośnych
-   Identyfikator aplikacji konfiguracji zastępuje Konfiguracja parametry połączenia.
-   Usunięte interfejsu API do wysyłania i odbierania wiadomości XMPP dowolnego z dowolnego jednostek XMPP.
-   Usunięte interfejsu API do wysyłania i odbierania wiadomości między urządzeniami.
-   Ulepszenia zabezpieczeń.
-   Śledzenie Google Play i SmartAd usunięte.
