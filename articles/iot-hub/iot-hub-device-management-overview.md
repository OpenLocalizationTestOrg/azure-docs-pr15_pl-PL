<properties
 pageTitle="Omówienie zarządzania urządzenia Centrum IoT | Microsoft Azure"
 description="Ten artykuł zawiera omówienie zarządzania urządzeniami w Centrum IoT Azure: cykl życia urządzenia przedsiębiorstwa, uruchom ponownie komputer, fabryczne, aktualizacji oprogramowania układowego, konfiguracji, twins urządzenia, kwerendy, zadania"
 services="iot-hub"
 documentationCenter=""
 authors="bzurcher"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/03/2016"
 ms.author="bzurcher"/>

# <a name="overview-of-azure-iot-hub-device-management-preview"></a>Omówienie zarządzania urządzeniami Centrum IoT Azure (wersja preview)

## <a name="introduction"></a>Wprowadzenie

Centrum IoT Azure udostępnia funkcje i model rozszerzeń umożliwiające urządzenia i deweloperzy wewnętrznej mogą tworzyć niezawodne rozwiązania do zarządzania urządzenia IoT. IoT urządzeń zakres z ograniczeniami czujniki i mikrokontrolerów jednemu celowi do zaawansowanych bram, które powodują komunikacji dla grup urządzeń.  Ponadto przypadków użycia i wymagania dotyczące operatorów IoT różnią się znacznie w branży.  Mimo tego wariantu zarządzania urządzeniami Centrum IoT Azure zapewnia możliwości, wzorców oraz biblioteki kodu w celu zaspokojenia do zestawu różnych urządzeń i użytkowników końcowych.

Kluczowe część tworzenia enterprise pomyślnego rozwiązania IoT zapewnienie strategii jak operatory obsługiwać codziennego zarządzania ich zbioru urządzeń. Operatory IoT wymagają prosty i niezawodnych narzędzi i aplikacji, które umożliwia im skoncentrować się na bardziej strategic aspektów swoich zadań. Ten artykuł zawiera:

- Krótkie omówienie Centrum IoT Azure podejście do zarządzania urządzeniami.
- Opis wspólne zasady zarządzania urządzenia.
- Opis cyklu życia urządzenia.
- Omówienie typowe wzorce zarządzania urządzenia.

## <a name="iot-device-management-principles"></a>Zasady zarządzania IoT urządzenia

IoT sobie z nim unikatowy zestaw problemach zarządzania urządzenia i każdej rozwiązanie klasy korporacyjnej Musisz zająć się następujących zasad:

![Azure Centrum IoT urządzenia zarządzania zasadami grafiki][img-dm_principles]

- **Skala i automatyzacji**: rozwiązania IoT wymagają prostych narzędzi, które można zautomatyzować powtarzających się zadań, a następnie włączyć stosunkowo niewielką Operatorzy Zarządzanie miliony urządzenia. Bieżące, operatorów się spodziewać obsługiwać operacje urządzenia zdalnie, zbiorczo i tylko zbliżającym się pojawić problemy, które muszą się zająć bezpośredniego.

- **Przejrzystość i zgodności**: ekosystemu urządzenia IoT jest wyjątkowo zróżnicowany. Narzędzia do zarządzania muszą być dostosowane, aby zezwalały na wiele różnych klas urządzeń, platformy i protokoły. Operatory muszą mieć możliwość obsługi wielu typów urządzenia z najbardziej ograniczonym dostępem osadzony układów proces pojedyncze zaawansowanych i pełną funkcjonalność komputerach.

- **Kontekst Sygnalizacja dostępności**: środowiskach IoT są dynamiczne i zmieniania kiedykolwiek. Usługa niezawodność to najważniejsze. Operacje zarządzania urządzenia musi współczynnik SLA konserwacja systemu windows, sieci i power warunki w obsłudze i geolokalizacja urządzenia zapewnienie tego przestoje konserwacji nie wpływają na operacje krytycznego, biznesowego lub tworzenie niebezpiecznych warunków.

- **Obsługa wielu ról**: Obsługa unikatowe przepływów pracy i procesów IoT ról operacji są istotne. Operatorzy musi harmonijnego pracować nad danym ograniczeń wewnętrznych działów informatycznych.  Muszą one również znaleźć stałego sposoby powierzchniowy czasu rzeczywistego urządzenia operacje informacji do kierownicy i ról zarządzania innych firm.

## <a name="iot-device-lifecycle"></a>Cykl życia urządzenia IoT

Istnieje zestaw etapów zarządzania ogólny urządzenia, które są wspólne dla wszystkich projektów w przedsiębiorstwie IoT. W Azure IoT istnieje pięć etapów cyklu IoT urządzenia:

![Pięć etapów cyklu życia urządzenia Azure IoT: Planowanie, obsługi administracyjnej, konfigurowanie, monitorowanie, Anuluj][img-device_lifecycle]

W każdym z tych pięciu etapów istnieje kilka wymagań operator urządzenia, które powinny być spełnione zapewnienie kompleksowym rozwiązaniem:

- **Planowanie**: Włączanie operatorów utworzyć schemat metadanych urządzenia, który pozwala na łatwe dokładnie kwerendy, a grupę urządzeń operacji zarządzania zbiorcze. Podwójnego urządzenia służy do przechowywania metadanych urządzenia w postaci znaczników i właściwości.

    *Więcej informacji*: [Rozpoczynanie pracy z urządzenia twins][lnk-twins-getstarted], [Opis urządzenia twins][lnk-twins-devguide], [jak za pomocą właściwości podwójnego][lnk-twin-properties]

- **Obsługa administracyjna**: bezpieczne obsługi administracyjnej nowych urządzeniach do koncentratora IoT i Włącz operatorów od razu Poznawanie możliwości urządzenia.  Tworzenie tożsamości elastyczne urządzenia i poświadczeń przy użyciu rejestru urządzenia IoT Centrum i wykonania tej operacji zbiorczo przy użyciu zadania. Tworzenie urządzenia, aby ich funkcje i warunków za pośrednictwem właściwości urządzenia w podwójnego urządzenia raportów.

    *Więcej informacji*: [Zarządzanie tożsamościami urządzenia][lnk-identity-registry], [zbiorcze Zarządzanie tożsamościami urządzenia][lnk-bulk-identity], [jak za pomocą właściwości podwójnego][lnk-twin-properties]

- **Konfigurowanie**: ułatwienia zbiorczych zmian w konfiguracji i aktualizacji oprogramowania układowego urządzeń przy zachowaniu zarówno zdrowia i zabezpieczenia. Wykonaj te operacje zarządzania urządzenia zbiorczo za pomocą odpowiednie właściwości lub metod bezpośredni i emisji zadania.

    *Więcej informacji*: [bezpośredni metodami][lnk-c2d-methods], [Wywołaj metodę bezpośrednio na urządzeniu][lnk-methods-devguide], [jak za pomocą właściwości podwójnego][lnk-twin-properties], [harmonogramu i zadań emisji][lnk-jobs], [Planowanie zadań na wielu urządzeniach][lnk-jobs-devguide]

- **Monitorowanie**: monitorowanie kondycji ogólnego zbioru urządzenia, stan trwających operacji i alert operatorów problemów, które mogą wymagać ich działań.  Stosowanie podwójnego urządzenia umożliwia urządzenia do raportu czasu rzeczywistego warunki i stan operacji aktualizacji. Tworzenie zaawansowanych pulpitu nawigacyjnego raportów tej powierzchni największy problemów za pomocą urządzenia podwójnego kwerend.

    *Więcej informacji*: [jak za pomocą właściwości podwójnego][lnk-twin-properties], [języka kwerend twins i zadania][lnk-query-language]

- **Wycofaj**: Zamień lub zlikwidować urządzenia po awarii, uaktualnianie cyklu, lub na końcu ważności usługi.  Zachowaj informacje o urządzeniach, jeśli jest fizycznie urządzenia za pomocą urządzenia podwójnego zastąpiony lub archiwizowane, jeśli jest wycofana. Za pomocą rejestru urządzenia Centrum IoT bezpieczne cofnięcia tożsamości urządzenia i poświadczenia.

    *Więcej informacji*: [jak za pomocą właściwości podwójnego][lnk-twin-properties], [Zarządzanie tożsamościami urządzenia][lnk-identity-registry]

## <a name="iot-hub-device-management-patterns"></a>Centrum IoT urządzenia zarządzania wzorców

Centrum IoT udostępnia następujące zestaw wzorców zarządzania urządzenia.  [Samouczki dotyczące zarządzania urządzenia] [ lnk-get-started] przedstawiono bardziej szczegółowo oraz jak zwiększyć deseni w celu dopasowania dokładnego rozwiązania do projektowania nowej desenie na podstawie tych szablonów core.

- **Uruchom ponownie** - aplikacji wewnętrznej informuje urządzenie metodą bezpośredniego że została zainicjowana Uruchom ponownie komputer.  Urządzenie jest używane urządzenie podwójnego zgłoszone właściwości, aby zaktualizować stan ponownym uruchomieniu urządzenia.

    ![Zarządzanie urządzeniami w usłudze Azure Centrum IoT Uruchom ponownie wzorzec][img-reboot_pattern]

- **Resetowanie factory** - aplikacji wewnętrznej informuje urządzenie metodą bezpośredniego że została zainicjowana Resetowanie factory.  Urządzenie używa podwójnego urządzenia zgłoszoną właściwości, aby zaktualizować fabryki Resetuj stan urządzenia.

    ![Azure factory zarządzania urządzenia Centrum IoT Resetowanie grafiki deseniem][img-facreset_pattern]

- **Konfiguracja** — aplikacji wewnętrznej używa właściwości podwójnego potrzeby urządzenia do konfigurowania oprogramowanie na urządzeniu.  Urządzenie jest używane urządzenie podwójnego zgłoszone właściwości, aby zaktualizować stan konfiguracji urządzenia.

    ![Azure Centrum IoT urządzenia zarządzania konfiguracji deseniu grafiki][img-config_pattern]

- **Aktualizowanie oprogramowania układowego** — wewnętrznej aplikacji zostanie wyświetlona informacja, urządzenie metodą bezpośredniego że została zainicjowana aktualizacji oprogramowania układowego.  Urządzenie inicjuje procesem wieloetapowym Pobierz sprzętowego, zainstalowanie pakietu sprzętowego i na końcu ponownie nawiązać połączenie z usługą Centrum IoT.  W trakcie procesu krok mult urządzenie używane urządzenie podwójnego zgłoszone właściwości, aby zaktualizować postęp i stan urządzenia.

    ![Aktualizowanie Azure sprzętowego zarządzania urządzenia Centrum IoT wzorzec][img-fwupdate_pattern]

- **Raportowanie postępu i stanu** — wewnętrznej aplikacji obsługuje kwerendy podwójnego urządzenia, w zestawie urządzenia raport o stanie i postępu działań działa na urządzeniach.

    ![Azure zarządzania urządzeniami Centrum IoT raportowania postęp i stan grafiki deseniem][img-report_progress_pattern]

## <a name="next-steps"></a>Następne kroki

Za pomocą możliwości, wzorców oraz biblioteki kodu, dostępne w Centrum IoT Azure MDM, do tworzenia aplikacji IoT, które spełniają wymagania operator IoT przedsiębiorstwa w na każdym etapie cyklu życia urządzenia.

Aby kontynuować szkoleniowe na temat funkcji zarządzania urządzenia Centrum IoT Azure, zobacz [Wprowadzenie do zarządzania urządzeniami Azure IoT Centrum] [ lnk-get-started] samouczka.

<!-- Images and links -->
[img-dm_principles]: media/iot-hub-device-management-overview/image4.png
[img-device_lifecycle]: media/iot-hub-device-management-overview/image5.png
[img-config_pattern]: media/iot-hub-device-management-overview/configuration-pattern.png
[img-facreset_pattern]: media/iot-hub-device-management-overview/facreset-pattern.png
[img-fwupdate_pattern]: media/iot-hub-device-management-overview/fwupdate-pattern.png
[img-reboot_pattern]: media/iot-hub-device-management-overview/reboot-pattern.png
[img-report_progress_pattern]: media/iot-hub-device-management-overview/report-progress-pattern.png

[lnk-twins-devguide]: iot-hub-devguide-device-twins.md
[lnk-get-started]: iot-hub-device-management-get-started.md
[lnk-twins-getstarted]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-properties]: iot-hub-node-node-twin-how-to-configure.md
[lnk-hub-getstarted]: iot-hub-csharp-csharp-getstarted.md
[lnk-identity-registry]: iot-hub-devguide-identity-registry.md
[lnk-bulk-identity]: iot-hub-bulk-identity-mgmt.md
[lnk-query-language]: iot-hub-devguide-query-language.md
[lnk-c2d-methods]: iot-hub-c2d-methods.md
[lnk-methods-devguide]: iot-hub-devguide-direct-methods.md
[lnk-jobs]: iot-hub-schedule-jobs.md
[lnk-jobs-devguide]: iot-hub-devguide-jobs.md