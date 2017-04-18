<properties 
    pageTitle="Wskaźniki monitorowania StorSimple | Microsoft Azure" 
    description="W tym artykule opisano diody elektroluminescencyjne (LED) i dźwiękowe alarmy używana do monitorowania stanu urządzenia StorSimple."
    services="storsimple"
    documentationCenter="NA"
    authors="alkohli"
    manager="carmonm"
    editor="" />
 <tags 
    ms.service="storsimple"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="TBD"
    ms.date="08/18/2016"
    ms.author="alkohli" />

# <a name="use-storsimple-monitoring-indicators-to-manage-your-device"></a>Zarządzanie urządzenia za pomocą StorSimple wskaźniki monitorowania   

## <a name="overview"></a>Omówienie

Urządzenie StorSimple zawiera diody elektroluminescencyjne (LED) i alarmy, w której można monitorować moduły i ogólny stan urządzenia StorSimple. Wskaźniki monitorowania można znaleźć w części sprzętu urządzenia podstawowego załącznik i załącznik EBOD. Wskaźniki monitorowania może być LED lub dźwiękowe alarmów.

Istnieją trzy stany LED używany do oznaczania stanu modułu: zielony, migające zielony, czerwony bursztynowy lub bursztynowa czerwony.  

- Zielony LED reprezentują prawidłowy stan.  
- Migające zielonego na czerwony bursztynowy LED reprezentują obecności niekrytyczne warunki, które mogą wymagać udziału użytkownika.  
- Czerwony bursztynowy LED oznacza, że błąd krytyczne prezentowanie w module.  

W dalszej części tego artykułu opisano różne monitorowania LED wskaźnika, ich lokalizacji na tym urządzeniu StorSimple stan urządzenia, na podstawie stanach LED i wszystkie skojarzone alarmy sygnałem dźwiękowym.

## <a name="front-panel-indicator-leds"></a>Wskaźnik panelu przedniego LED

Panelu przedniego, nazywane także *panelu Operacje* lub *panelu ops*Wyświetla agregacji stan wszystkich modułów w systemie. Panelu przedniego jest taka sama na StorSimple podstawowego i załącznik EBOD i przedstawiono poniżej.  

   ![Urządzenie panelu przedniego][1]
 
Przednia panel zawiera następujących kwestii:  

1. Przycisk Wycisz
2. Wskaźnik Power LED (zielony czerwony bursztynowy)
3. Wskaźnik błędu modułu DOPROWADZIŁO (na czerwono bursztynowy wyłączona)
4. Wskaźnik błędów logicznych DOPROWADZIŁO (na czerwony bursztynowy wył.
5. Wyświetl identyfikator jednostki  

Główna różnica pomiędzy panelu przedniego LED urządzenia oraz dla załącznik EBOD jest **Numer identyfikacyjny jednostki System** podaną na wyświetlaczu LED. Identyfikator jednostki domyślne wyświetlanego na urządzeniu jest **00**domyślny identyfikator jednostki wyświetlane na załącznik EBOD jest **01**. Pozwala szybko rozróżnienie między urządzeniem i załącznik EBOD, gdy urządzenie jest włączona. Jeśli urządzenie jest wyłączone, użyj informacji podanych w [wyłączyć na nowym urządzeniu](storsimple-turn-device-on-or-off.md#turn-on-a-new-device) rozróżnienie na urządzeniu z załącznik EBOD.  

## <a name="front-panel-led-status"></a>Przednia panel LED stanu  

Aby zidentyfikować stan wskazywany przez LED na panelu przedniego urządzenia lub załącznik EBOD, skorzystaj z poniższej tabeli.  

|Power systemu | Moduł błędów | Błąd logicznych. | Alarmu | Stan|
|-------------|---------------|-----------------|-------|-------|
|Bursztynowy czerwony | WYŁĄCZANIE     | WYŁĄCZANIE | N/D! | Power AC utracone, działający na power kopii zapasowej lub power kr na i kontroler, który moduły zostały usunięte.|
|Zielony | WŁĄCZONE | WŁĄCZONE | N/D! | Stan testowanie OPS panel Włącz (5s)|
|Zielony | WYŁĄCZANIE | WYŁĄCZANIE | N/D! | Włącz wszystkie funkcje dobre|
|Zielony | WŁĄCZONE |N/D! | Błąd PCM LED, błędów wentylatora LED | Każde uszkodzenie PCM wentylatora błędów, powyżej lub poniżej temperatury|
| Zielony | WŁĄCZONE | N/D! | Moduł we/wy LED  | Każde uszkodzenie moduł kontrolera|
| Zielony | WŁĄCZONE | N/D! | N/D! | Błąd logiczny załącznik|
| Zielony | Flash | N/D! | Stan modułu LED na moduł kontrolera. Błąd PCM LED, błędów wentylatora LED | Typ modułu nieznany kontrolera zainstalowany, I2C bus błąd, błąd konfiguracji Kontroler modułu produktu danych (VPD) |

## <a name="power-cooling-module-pcm-indicator-leds"></a>Power chłodzenia modułu (PCM) wskaźnik LED   

Power chłodzenia modułu (PCM) wskaźnik LED znajdują się z tyłu podstawowego załącznik lub załącznik EBOD w każdym module PCM. W tym temacie omówiono sposób używania następujących LED monitorowanie stan urządzenia StorSimple.  

- Wskaźniki LED PCM podstawowego załącznik
- Wskaźniki LED PCM załącznik EBOD

## <a name="pcm-leds-for-the-primary-enclosure"></a>Wskaźniki LED PCM podstawowego załącznik  

Urządzenie StorSimple ma Moduł PCM 764W o dodatkowe baterii. Na poniższej ilustracji przedstawiono panelu LED urządzenia.  

   ![PCM LED podstawowego załącznik][2]

Legenda LED:

1. Błąd power AC
2. Błąd wentylatora
3. Uszkodzenia
4. PCM OK
5. Błąd kontrolera domeny
6. Warto baterii  

Stan PCM wskazuje na panelu LED. Panel PCM LED urządzenia zawiera sześć LED. Cztery z tych LED wyświetlany stan zasilania i wentylatora. Pozostałe dwa LED oznaczania stanu modułu baterią zapasową w PCM. Za pomocą następujących tabel do określenia stanu PCM.  

### <a name="pcm-indicator-leds-for-power-supply-and-fan"></a>Wskaźnik PCM LED zasilania i wentylatora
| Stan | OK PCM (zielony) | Niepowodzenie kr (żółte) | Niepowodzenie wentylatora (żółte) | Niepowodzenie kontrolera domeny (żółte) |
|--------|----------------|-----------------------|------------------|----------------------|
| Brak zasilania kr (aby załącznik) | WYŁĄCZANIE | WYŁĄCZANIE | WYŁĄCZANIE | WYŁĄCZANIE|
| Brak zasilania kr (tylko ten PCM) | WYŁĄCZANIE | WŁĄCZONE | WYŁĄCZANIE | WŁĄCZONE |
| KR prezentowanie na PCM - OK     | WŁĄCZONE | WYŁĄCZANIE | WYŁĄCZANIE | WYŁĄCZANIE |
| PCM fail (niepowodzenie wentylatora) | WYŁĄCZANIE | WYŁĄCZANIE | WŁĄCZONE | N/D! |
| Błąd PCM (nad amp nad napięcia na bieżący) | WYŁĄCZANIE | WŁĄCZONE | WŁĄCZONE | WŁĄCZONE |
| PCM (wentylatora poza uszkodzenia) | WŁĄCZONE | WYŁĄCZANIE | WYŁĄCZANIE | WŁĄCZONE |
| Wstrzymania | Migające | WYŁĄCZANIE | WYŁĄCZANIE | WYŁĄCZANIE |
| Pobieranie oprogramowania układowego PCM | WYŁĄCZANIE | Migające | Migające | Migające |

### <a name="pcm-indicator-leds-for-the-backup-battery"></a>Wskaźnik PCM LED baterii kopii zapasowej  

| Stan | Warto baterii (zielony) | Uszkodzenia (bursztynowy) |
|--------|----------------------|-----------------------|
| Nie ma baterii | WYŁĄCZANIE | WYŁĄCZANIE |
| Baterii obecne i opłaconych | WŁĄCZONE | WYŁĄCZANIE |
| Zrzut baterii ładowania lub konserwacji | Migające | WYŁĄCZANIE |
| "Wygładzania" uszkodzenia (odzyskania) | WYŁĄCZANIE | Migające |
| "Twardym" uszkodzenia (non odzyskania) | WYŁĄCZANIE | WŁĄCZONE |
| Pozbawionych baterii | Migające | WYŁĄCZANIE |

## <a name="pcm-leds-for-the-ebod-enclosure"></a>Wskaźniki LED PCM załącznik EBOD  

Załącznik EBOD ma 580W PCM i nie dodatkowych baterii. Panel PCM dla załącznik EBOD ma wskaźnik LED tylko w przypadku źródła energii i wentylatora. Na poniższej ilustracji przedstawiono te LED.

   ![Załącznik EBOD LED PCM][3] 
 
Poniższa tabela służy do określenia stanu PCM.  

| Stan | OK PCM (zielony) | Niepowodzenie kr (żółte) | Niepowodzenie wentylatora (żółte) | Niepowodzenie kontrolera domeny (żółte) |
|--------|---------------|------------------------|------------------|----------------------|
| Brak zasilania kr (aby załącznik) | WYŁĄCZANIE | WYŁĄCZANIE | WYŁĄCZANIE | WYŁĄCZANIE |
| Brak zasilania kr (tylko ten PCM) | WYŁĄCZANIE | WŁĄCZONE | WYŁĄCZANIE | WŁĄCZONE |
| KR prezentowanie na PCM — OK | WŁĄCZONE | WYŁĄCZANIE | WYŁĄCZANIE | WYŁĄCZANIE |
| PCM fail (niepowodzenie wentylatora) | WYŁĄCZANIE | WYŁĄCZANIE | WŁĄCZONE | X |
| Błąd PCM (nad amp nad napięcia na bieżący | WYŁĄCZANIE | WŁĄCZONE | WŁĄCZONE | WŁĄCZONE |
| PCM (wentylatora poza uszkodzenia) | WŁĄCZONE | WYŁĄCZANIE | WYŁĄCZANIE | WŁĄCZONE |
| Model wstrzymania | Migające | WYŁĄCZANIE | WYŁĄCZANIE | WYŁĄCZANIE |
| Pobieranie oprogramowania układowego PCM | WYŁĄCZANIE | Migające | Migające | Migające |

## <a name="controller-module-indicator-leds"></a>Kontroler modułu wskaźnik LED  

Urządzenie StorSimple zawiera LED w modułach kontroler EBOD i podstawowego kontrolera.   

### <a name="monitoring-leds-for-the-primary-controller"></a>Monitorowanie LED podstawowego kontrolera
Poniższa ilustracja pomaga zidentyfikować LED na kontrolerze podstawowym. (Wszystkie składniki znajduje się do pomocy w orientacji.)  

   ![Monitorowanie LED - podstawowego kontrolera][4]
 
Skorzystaj z poniższej tabeli, aby określić, czy moduł kontrolera działa poprawnie.  

### <a name="controller-indicator-leds"></a>Kontroler wskaźnik LED  

| LED | Opis                                                                            
|---- | ----------- |
| Identyfikator LED (niebieski) | Wskazuje, że moduł jest identyfikowany. Jeśli niebieski LED jest miganie na kontrolerze uruchomionego, kontrolera jest aktywne kontrolera, a druga jest wstrzymania kontrolera. Aby uzyskać więcej informacji zobacz [Identyfikowanie active kontrolera na urządzeniu](storsimple-controller-replacement.md#identify-the-active-controller-on-your-device). |
| Błąd LED (bursztynowy) | Wskazuje błędów na kontrolerze.        
| LED OK (zielony) | Zielony stabilny wskazuje, że kontroler jest przycisk OK. Migające zielony wskazuje kontrolerze VPD błąd konfiguracji. |
| Działania skojarzeń zabezpieczeń LED (zielony) | Zielony stabilny wskazuje połączenia z nie bieżących działań. Migające zielony wskazuje, że połączenie ma bieżącej aktywności. |
| Stan Ethernet LED | Po prawej stronie wskazuje aktywność sieć łącze: łącza (zielony stabilny) aktywne (błyskające zielony) aktywność sieciowa. Po lewej stronie wskazuje szybkość sieci: (żółte) 1000 Mb/s (zielony) 100 Mb/s i (wyłączone) 10 Mb/s. W zależności od modelu składnika powyższe może miganie, nawet jeśli nie włączono interfejsu sieciowego. |
| WPIS LED | Wskazuje postęp uruchamiania, gdy jest włączona przez administratora. Urządzenie StorSimple nie będzie uruchomiony, to LED pomoże Support Microsoft określić punkt w procesie uruchamiania, w którym wystąpił błąd. |

>[AZURE.IMPORTANT] 
Jeśli ten błąd LED jest włączone, występuje problem z modułem kontrolerze, który może zostać rozwiązany uruchamiając administratora. Jeśli ponowne uruchomienie na kontrolerze nie rozwiąże ten problem, skontaktuj się Support firmy Microsoft.  


### <a name="monitoring-leds-for-the-ebod-ebod-enclosure"></a>Monitorowanie LED EBOD (załącznik EBOD)  

Każda 6 kontroler Gb/s EBOD skojarzeń zabezpieczeń ma LED, które wskazują jego stan, jak pokazano na poniższej ilustracji.  

  ![Monitorowanie LED - EBOD załącznik][5]

Skorzystaj z poniższej tabeli, aby określić, czy moduł kontrolera EBOD normalnego działania.  

### <a name="ebod-controller-module-indicator-leds"></a>EBOD kontroler modułu wskaźnik LED  

|Stan | Moduł we/wy OK (zielony) | Błąd modułu we/wy (bursztynowy) | Aktywności portu hosta (zielony) |
|-------|----------------------|-------------------------------|----------------------------|
| Moduł kontrolera OK | WŁĄCZONE | WYŁĄCZANIE | - |
| Kontroler modułu błędów | WYŁĄCZANIE | WŁĄCZONE | - |
| Brak połączenia portu zewnętrznych hosta | - | - | WYŁĄCZANIE |
| Połączenia portu hosta zewnętrznych — Brak aktywności | - | - | WŁĄCZONE |
| Połączenie portu hosta zewnętrznych — aktywności | - | - | Migające |
| Błąd metadanych modułu kontrolera | Migające | - | - |

## <a name="disk-drive-indicator-leds-for-the-primary-enclosure-and-ebod-enclosure"></a>Dysk wskaźnik LED załącznik podstawowego i EBOD załączników

Urządzenie StorSimple ma dysków znajduje się w podstawowym załącznik i załącznik EBOD. Każdy dysk zawiera monitorowania LED wskaźnik opisane w tej sekcji. 

Dla stacji dysków stanu dysk jest wskazywana przez zielona LED i LED bursztynowy czerwony zainstalowany na pierwszej stronie każdym module carrier dysk. Na poniższej ilustracji przedstawiono te LED.

  ![LED dysku][6]
 
Skorzystaj z poniższej tabeli, aby określić stan każdego dysku, która z kolei wpływa na panelu ogólnej przedniego LED stanu.  

### <a name="disk-drive-indicator-leds-for-the-ebod-enclosure"></a>Dysk wskaźnik LED załącznik EBOD  

| Stan | Działania OK LED (zielony) | Błąd LED (czerwony bursztynowy) | Skojarzony ops panel LED |
|-------|--------------------------|----------------------|-------------------------|
| Nie dysku zainstalowany | WYŁĄCZANIE | WYŁĄCZANIE | Brak |
| Dysk zainstalowany i działający | Migające włączania/wyłączania z działaniem | X | Brak |
| Ustawianie tożsamości urządzenia SCSI załącznik usług (SE) | WŁĄCZONE | Migające 1 sekundy na-1 drugie wyłączone | Brak |
| Ustawiony bit błędów urządzenia SE | WŁĄCZONE | WŁĄCZONE | Błąd logiczne (czerwony) |
| Awaria zasilania układu sterowania Power | WYŁĄCZANIE | WŁĄCZONE | Moduł błędów (czerwony) |

## <a name="audible-alarms"></a>Alarmy dźwiękowe  

Urządzenie StorSimple zawiera dźwiękowe alarmy skojarzone z podstawowego załącznik i załącznik EBOD. Alarmu znajduje się na panelu przedniego (nazywane także panel ops) obu załączniki. Alarmu wskazuje, kiedy ma warunek błędu. Następujące warunki będzie aktywować alarmu:  

- Błąd wentylatora lub błąd
- Napięcie się poza zakresem
- Powyżej lub poniżej temperatury warunku
- Przekroczenie termicznego
- Błąd systemowy
- Błąd logicznych.
- Power obsługi błędów
- Usunięcie potęgi chłodzenia moduł (PCM)  

W poniższej tabeli opisano różne stany alarmu.  

### <a name="alarm-states"></a>Zjednoczone alarmu  

| Stan alarmu | Akcja | Akcja z naciśniętym przyciskiem Wycisz |
|------------|---------|---------------------------------|
| S0 | Normalny tryb: cichy | Dwa razy dźwięk |
| S1 | Tryb błędów: 1 sekundy na-1 drugie wyłączone | Przejście do S2 lub S3 (zobacz Uwagi) |
| S2 | Przypomnij tryb: przejściowymi dźwięk | Brak |
| S3 | Tryb wyciszony: cichy | Brak |
| S4 | Tryb krytycznych błędów: ciągła alarmu | Nie są dostępne: Wycisz nie jest aktywny |

> [AZURE.NOTE] 

>  - W stanie alarmu S1 Jeśli nie zostanie naciśnięty Wycisz w ciągu dwóch minut, stan automatycznie przejścia S2 i S3.  
>  - Alarmu Państw S1 S4 wrócić do S0, po warunek błędów jest wyczyszczone.  
>  - Stan krytycznych błędów S4 może być wprowadzane przez inne Państwo.  

Możesz również wyciszyć alarmu, naciskając przycisk Wycisz na panelu ops. Wyciszanie automatyczne będzie występować po dwie minuty, jeśli Wycisz przełącznik nie jest obsługiwana ręcznie. Po wyciszeniu alarmu, będzie dźwięk z krótkim przejściowymi dźwięków, aby wskazać, że problem nadal występuje. Alarmu będzie odbiorcze, gdy są usuwane wszystkie problemy.  

W poniższej tabeli opisano różne warunki alarmu.  

### <a name="alarm-conditions"></a>Warunki alarmu  

| Stan | Ważności | Alarmu | OPS panel LED |
|--------|---------|--------|----------------|
| Alert PCM — utrata zasilania kontrolera domeny z jednym PCM | Błąd — bez straty nadmiarowości | S1 | Moduł błędów|
|Alert PCM — utrata zasilania kontrolera domeny z jednym PCM | Błąd — utratę nadmiarowości | S1 | Moduł błędów |
| Niepowodzenie wentylatora PCM | Błąd — utratę nadmiarowości | S1 | Moduł błędów |
| Moduł SBB wykrywania błędów PCM | Błąd | S1 | Moduł błędów |
| Usunięte PCM | Błąd konfiguracji | Brak | Moduł błędów |
| Błąd konfiguracji załącznik | Błąd — krytyczne | S1 | Moduł błędów |
| Alert temperaturę ostrzeżenie o niskim | Ostrzeżenie | S1 | Moduł błędów |
| Alert temperaturę ostrzeżenie o wysokiej | Ostrzeżenie | S1 | Moduł błędów |
| Na temperaturę alarmu | Błąd — krytyczne | S1 | Moduł błędów |
| Błąd bus I2C | Błąd — utratę nadmiarowości | S1 | Moduł błędów |
| OPS panel błąd komunikacji (I2C) | Błąd — krytyczne     | S1 | Moduł błędów |
| Błąd kontrolera | Błąd — krytyczne | S1 | Moduł błędów |
| Błąd modułu interfejsu SBB | Błąd — krytyczne | S1 | Moduł błędów |
| Błąd modułu interfejsu SBB — nie działa moduły pozostała | Błąd — krytyczne | S4 | Moduł błędów |
| Moduł interfejsu SBB usunięte | Ostrzeżenie | Brak | Moduł błędów |
| Dysk power kontrola błędów | Ostrzeżenia — bez straty power dysk | S1 | Moduł błędów |
| Dysk power kontrola błędów | Błąd — krytyczne; utrata zasilania dysku | S1 | Moduł błędów |
| Usunięto dysk | Ostrzeżenie | Brak | Moduł błędów |
| Dostępna jest za mało moc | Ostrzeżenie | Brak | Moduł błędów |

## <a name="next-steps"></a>Następne kroki

Dowiedz się więcej o [StorSimple sprzętowej i stan](storsimple-monitor-hardware-status.md).

[1]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE01.png
[2]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE02.png
[3]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE03.png
[4]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE04.png
[5]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE05.png
[6]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE06.png

 
