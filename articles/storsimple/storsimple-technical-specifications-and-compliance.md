<properties 
   pageTitle="Techniczne StorSimple | Microsoft Azure"
   description="W tym artykule opisano wymagania techniczne i przepisami standardy zgodności informacje o StorSimple sprzętu składników."
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
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="technical-specifications-and-compliance-for-the-storsimple-device"></a>Wymagania techniczne i zgodność urządzenia StorSimple

## <a name="overview"></a>Omówienie

Składniki sprzętu urządzenia Microsoft Azure StorSimple zgodny techniczne i przepisami standardy przedstawionych w tym artykule. Wymagania techniczne opis Power i chłodzenia modułów (PCMs), dysków, pojemność i załączniki. Informacje o zgodności obejmuje takich elementów, jak międzynarodowe normy, bezpieczeństwa i emisji i okablowania.


## <a name="power-and-cooling-module-specifications"></a>Specyfikacje zasilania i chłodzenia modułu  

Urządzenie StorSimple ma dwa 240 100 wachlarz podwójne, notacją SBB Power chłodzenia modułów (PCMs). Dzięki temu konfiguracja nadmiarowy. Jeśli PCM kończy się niepowodzeniem, urządzenie w dalszym ciągu normalnego działania na inne PCM, dopóki nie zostanie wymieniona modułu.  

Załącznik EBOD używa 580 W PCM, a załącznik podstawowego używa 764 W PCM. W poniższej tabeli wymieniono techniczne skojarzone z PCMs.

| Specyfikacja           | 580 W PCM (EBOD)                                    | 764 W PCM (podstawowa)                                |
|------------------------ | --------------------------------------------------- | -------------------------------------------------- |
| Power maksymalnej wydajności    | 580 W                                               | 764                                                |
| Częstość               | 50-60 Hz                                            | 50-60 Hz                                           |
| Zaznaczanie zakresu napięcia | Automatyczne zakresu: 90 – 264 V AC, 47-63 Hz               | Automatyczne zakresu: 90-264 V AC, 47-63 Hz               |
| Maksymalna liczba zasypania bieżącego  | 20 A                                                | 20 A                                               |
| Power współczynnik korekty | > nominalna napięcie wejściowe 95%                          | > nominalna napięcie wejściowe 95%                         |
| Składowe harmoniczne               | Spełnia EN61000-3-2                                   | Spełnia EN61000-3-2                                  |
| Wynik                  | 5 v wstrzymania napięcia @ 2.0 A                          | 5 v wstrzymania napięcia @ 2.7 odpowiedzi                         |
|                         | + 5 v @ 42 A                                          | + 5 v @ 40 A                                         |
|                         | + 12V @ 38 odpowiedzi                                         | + 12V @ 38 odpowiedzi                                        |
| Hot podłączany           |  Tak                                                | Tak                                                |
| Przełączniki i LED       | Przełączanie stan KR i wskaźnik stanu cztery LED     | Przełączanie stan KR i wskaźnik stanu sześć LED     |
| Załącznik chłodzenia       | Osiową, chłodzenia wentylatory kontrolki szybkości wentylatora zmiennych  | Osiową, chłodzenia wentylatory kontrolki szybkości wentylatora zmiennych |

 
## <a name="power-consumption-statistics"></a>Statystyki zużycie energii  

W poniższej tabeli przedstawiono typowe power dane dotyczące zużycia (rzeczywistych wartości, które mogą się różnić od publikowane) w różnych modelach StorSimple urządzenia. 
 
 Warunki | 240 V AC | 240 V AC | 240 V AC | 110 V AC | 110 V AC | 110 V AC 
 ---------- | -------- | -------- | -------- | -------- | -------- | -------- 
 Wentylatory wolno, dyski bezczynności | WYSOKOŚCI 1,45 A  |0.31 kW | 1057.76 BTU/godz. | 3.19 A | 0.34 kW | 1160.13 BTU/godz. 
 Wentylatory dyski wolno, uzyskiwanie dostępu do | 1.54 A | 0,33 kW | 1126.01 BTU/godz. | 3.27 A | 0.36 kW | 1228.37 BTU/godz. 
 Obsługiwane PSUs dwa wentylatory szybkim dyski bezczynności, | 2.14 A | 0.49 kW  | 1671.95 BTU/godz. | 4,99 A | 0.54 kW | 1842.56 BTU/godz. 
 Szybkie, wentylatory dyski bezczynności, jeden zasilania korzystająca jedną bezczynne | 2.05 A | 0.48 kW | 1637.83 BTU/godz. | 4.58 A | 0,50 kW | 1706.07 BTU/godz. 
 Szybkie, dyski wentylatory dostęp do dwóch PSUs włączony | 2.26 A | 0,51 kW | 1740.19 BTU/godz. | MASIE 4,95 A | 0.54 kW | 1842.56 BTU/godz. 
 Szybkie wentylatory, dyski uzyskiwać dostęp do, jeden zasilania korzystająca jedną bezczynne | 2.14 A |0.49 kW | 1671.95 BTU/godz. | 4.81 A  | 0.53 kW | 1808.44 BTU/godz. 

## <a name="disk-drive-specifications"></a>Specyfikacje dysku  

Urządzenie StorSimple obsługuje maksymalnie 12 dysków szeregowego dołączone SCSI (SA) współczynnik formularz 3,5 cala. Rzeczywisty dyski mogą być zarówno półprzewodnikowe dyski SSD lub dysków twardych (dysków twardych), w zależności od konfiguracji produktu. 12 gniazd dysku znajdują się w konfiguracji 3, 4 przed załącznik. Załącznik EBOD umożliwia dodatkowego miejsca do magazynowania dla innego 12 stacji dysków. Są one zawsze dysków twardych.  

## <a name="storage-specifications"></a>Specyfikacje miejsca do magazynowania
Urządzenia StorSimple mają dysków twardych i dyski półprzewodnikowe zarówno 8100 i 8600. Całkowita wydajność użyteczne 8100 i 8600 to około 15 TB i 38 TB odpowiednio. Poniższa tabela dokumenty szczegóły SSD, dysk twardy i zdolności cloud w kontekście możliwości rozwiązanie StorSimple.

| Modelu urządzenia / wydajności                         | 8100                                                    | 8600                                                    |
|------------------------------------------------|---------------------------------------------------------|---------------------------------------------------------|
| Liczba dysków twardych (dysków twardych)              |   8                                                     |  19                                                     |
| Liczba półprzewodnikowe dyski SSD            |   4                                                     |  5                                                      |
| Pojedynczy pojemność dysku twardego                            |   4 TB                                                  |  4 TB                                                   |
| Pojedynczy zdolności SSD                            |   400 GB                                                |  800 GB                                                 |
| Wydajność zamiennych                                 |   4 TB                                                  | 4 TB                                                    |
| Użyteczne pojemność dysku twardego                            | 14 TB                                                   | 36 TB                                                   |
| Użyteczne zdolności SSD                            | 800 GB                                                  | 2 TB                                                    |
| Suma użyteczne zdolności *                         | ~ 15 TB                                                 | ~ 38 TB                                                 |
| Wydajność rozwiązanie maksymalna (w tym chmury)    | 200 TB                                                  | 500 TB                                                  |


<sup> * </sup> -  *Pojemność użyteczne zawiera możliwości danych, metadanych i buffers.*

## <a name="enclosure-dimensions-and-weight-specifications"></a>Załącznik wymiary i specyfikacje weight (waga)  

W poniższej tabeli wymieniono różne specyfikacje załącznik wymiary i grubości.  

### <a name="enclosure-dimensions"></a>Wymiary załącznik
W poniższej tabeli przedstawiono wymiary załącznik w milimetry i w centymetrach.

|Załącznik |Milimetry |Cale |
|----------|------------|-------| 
| Wysokość |87,9 | 3,46 |
| Szerokość wielu kołnierza instalowanie | 483 | 19.02 |
| Szerokość w treści załącznika | 443 | 17.44 |
| Głębokości montażu do wierzchołka treści załącznik | 577 | 22.72 |
| Głębokości z poziomu panelu Operacje do najdalej wierzchołka załącznika | 630.5 | 24.82 |
| Głębokości od kołnierza instalowanie do najdalej wierzchołka załącznika |   603 | 23.74 |

### <a name="enclosure-weight"></a>Załącznik weight (waga)  

W zależności od konfiguracji pełni załadowany załącznik podstawowego można rozważyć od 21 do 33 kg i wymaga dwóch osób będzie przetwarzał. 
 
| Załącznik | Weight (waga) |
|-----------|--------| 
| Maksymalna waga (zależy od konfiguracji) |30 kg — 33 kg |
| Pusta (nie dyski zainstalowane) |21 – 23 kg |

## <a name="enclosure-environment-specifications"></a>Specyfikacje środowiska załącznik  

W tej sekcji przedstawiono specyfikacje dotyczące środowiska załącznik. Temperaturę, wilgotność, wysokość, wstrząsy, wibracji, orientację, bezpieczeństwa i zgodności elektromagnetycznej (EMC) znajdują się w tej kategorii.  

### <a name="temperature-and-humidity"></a>Temperatury i wilgotności

| Załącznik        | Zakres temperaturze  | Otaczającego wilgotność względna | Maksymalna liczba mokrej żarówka   |
|------------------|----------------------------|---------------------------|--------------------|
| Operacyjne      | 5-35° C (41° F - 95° F)    | 20 – 80% — kondensacją niebędącym | 28 C (82° F)        |
| Nie działa  | -40-70° C (40° F - 158° F) | 5 do 100% bez kondensacji  | 29 C (84° F)        |

### <a name="airflow-altitude-shock-vibration-orientation-safety-and-emc"></a>Powietrza, wysokość, wstrząsy, wibracji, orientację, bezpieczeństwa i EMC
 
| Załącznik          | Specyfikacje operacyjne                                                |
|--------------------|---------------------------------------------------------------------------| 
| Powietrza            | System powietrza jest od początku do tyłu. System musi działać z instalacją niskociśnieniowego, tyłu spalin. Utworzony przez drzwi stojaków i przeszkód ciśnienia nie powinna przekraczać 5 paskalach (0,5 mm słupa wody). |
| Wysokość operacyjne  | metr-30 do 3045 liczników (-100 stóp do 10 000 stóp) z maksymalna temperatura Anuluj ocenianie 5 oC powyżej 7000 stóp. |
| Wysokość, nie działa  | metr-305 do 12,192 liczników (stopa-1,000 do 40 000 stóp) |
| Wstrząsy operacyjne  | sinus 10 ms ½ 5g |
| Wstrząsy nie działa  | sinus 10 ms ½ 30g |
| Wibracji operacyjne  | 0.21g RMS 5 – 500 Hz losowe |
| Wibracji nie działa  | 1.04g RMS 2 – 200 Hz losowe |
| Wibracji, przeniesienie  | 3g 2 – 200 Hz sinus |
| Orientacja i instalowanie  | 19" stojaków instalacji (2 EIA jednostki) |
| Stojaku  | Aby dopasować głębokość minimalne 700 mm (31.50 cala) stojaki zgodne z IEC 297 |
| Bezpieczeństwo i zatwierdzenia  |   CE i UL EN 61000-3, IEC 61000-3, UL 61000-3 |
| EMC  | EN55022 (CISPR - A), FCC ODPOWIEDZI |

## <a name="international-standards-compliance"></a>Zgodność ze standardami międzynarodowe
Microsoft Azure StorSimple urządzenie spełnia następujące normy międzynarodowe:  

- CE - PL 60950-1  
- Raport banku centralnego IEC 60950-1  
- UL i cUL do UL 60950-1  

## <a name="safety-compliance"></a>Bezpieczeństwo zgodności  

Microsoft Azure StorSimple urządzenie spełnia następujące oceny bezpieczeństwa:  

- Zatwierdzanie typu produktu systemu: UL, cUL, CE  
- Zgodność bezpieczeństwa: UL 60950, IEC 60950, EN 60950  

## <a name="emc-compliance"></a>Zgodność EMC 

Microsoft Azure StorSimple urządzenie spełnia następujące oceny EMC.  

### <a name="emissions"></a>Emisji

Urządzenie jest zgodny z EMC dla poziomów emisji przeprowadzane i promieniowania.  

- Zjawiska emisji ograniczyć poziomach: CFR 47 część 15B klasy EN55022 klasy CISPR zajęć  
- Promieniowania ograniczyć poziomy: CFR 47 część 15B klasy EN55022 klasy CISPR zajęć   

### <a name="harmonics-and-flicker"></a>Składowe harmoniczne i Flickr  

Urządzenie spełnia EN61000-3-2-3.  

### <a name="immunity-limit-levels"></a>Poziomy limit odporności  
Urządzenie spełnia EN55024.  

## <a name="ac-power-cord-compliance"></a>KR power przewód zgodności
  
Wtyczki i pełny zestaw przewód muszą spełniać normy właściwe do kraju, w którym urządzenie jest używane, a muszą mieć pozwolenia związane z bezpieczeństwem, które są dopuszczalne w tym kraju. W poniższej tabeli wymieniono standardy dla Europy i USA.  

### <a name="ac-power-cords---usa-must-be-nrtl-listed"></a>Kable AC - USA (musi być NRTL na liście)

| Składnik       | Specyfikacja                                                     |
| --------------- | ----------------------------------------------------------------- | 
| Typ kabla       | Maksymalna długość 2.0 metr OHR lub SVT 18 minimum Trójprzewodowy, przewodowe 3 |
| Wtyczki            | Wtyczki uziemiający spełnia bardzo załącznikami 5-15P NEMA ocenianie 120 V 10 A; lub IEC 320 C14 250 V, 10 A |
| Socket          | IEC 320 C-13, 250 V 10 A                                         |

### <a name="ac-power-cords---europe"></a>Kable AC - Europe

| Składnik       | Specyfikacja                                                     |
| --------------- | ----------------------------------------------------------------- | 
| Typ kabla       | Ujednolicone H05-VVF-3G1.0                                         |
| Socket          | IEC 320 C-13, 250 V 10 A                                         |

## <a name="supported-network-cables"></a>Kable sieciowe obsługiwane  

10 GbE interfejsy danych 2 i 3 danych, można znaleźć [listę kable sieciowe obsługiwane i moduły](storsimple-supported-hardware-for-10-gbe-network-interfaces.md).

## <a name="next-steps"></a>Następne kroki

Teraz możesz przystąpić do wdrożenia urządzenie StorSimple w centrum danych. Aby uzyskać więcej informacji zobacz [Wdrażanie urządzenia lokalnego](storsimple-deployment-walkthrough-u2.md).  
