<properties 
   pageTitle="Interfejsy sprzętu StorSimple 10 GbE | Microsoft Azure"
   description="W tym artykule opisano obsługiwane małych serwerowej podłączany odbiorcze (SFP), czy kable i przełączniki dla 10 GbE interfejsy na urządzeniu StorSimple."
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
   ms.date="09/21/2016"
   ms.author="alkohli" />

# <a name="supported-hardware-for-the-10-gbe-network-interfaces-on-your-storsimple-device"></a>Obsługa 10 interfejsów sieciowych GbE na urządzeniu z systemem StorSimple sprzętu

## <a name="overview"></a>Omówienie

Ten artykuł zawiera informacje o dodatkowych sprzęt działa z urządzeniem Microsoft Azure StorSimple.

## <a name="list-of-devices-tested-by-microsoft"></a>Listę urządzeń sprawdzone przez firmę Microsoft

Microsoft zostało przetestowane następujące małych serwerowej podłączany odbiorcze (SFP), kable i przełączniki, aby upewnić się, aby działały najlepiej z urządzenia. (Poniższe tabele zostaną zaktualizowane jako nowy sprzęt jest sprawdzany.)

### <a name="sfp-transceivers"></a>Odbiorcze SFP +

|Wprowadź|Model|
|---|---|
|Cisco|SFP-10G-SR|

### <a name="cables"></a>Czy kable

|S. Wartość nie. |Wprowadź|Model|
|---|---|---|
| 1.|Cisco|SFP-H10GB-CU1M|
| 2.|Cisco|SFP-H10GB-CU2M|
| 3.|Cisco|SFP-H10GB-CU3M|
| 4.|Tripp Lite|N820 - 05M (OM3)|

### <a name="switches"></a>Przełączniki

|S. Wartość nie.|Wprowadź|Model|
|---|---|---|
| 1. |Cisco|N3K-C3172PQ-10GE|
| 2. |Cisco|N3K-C3048-ZM-F|
| 3. |Cisco|N5K-C5596UP-FA|

## <a name="list-of-devices-tested-in-the-field"></a>Przetestowano listę urządzeń w polu

Ta sekcja zawiera listę urządzeń, które zostały pomyślnie wdrożone w polu przez klientów StorSimple. Te nie zostały sprawdzone przez firmę Microsoft, ale prawdopodobnie do pracy z urządzeniem StorSimple.
 
| Parametr                         | Wartość                                    |
|-----------------------------------|------------------------------------------|
| Przełączanie marki                     | Juniper                                  |
| Przełączanie modelu                    | ex4550 32F                               |
| Przełączanie wersji systemu operacyjnego | JunOS 12.3R9.4                           |
| Karta modelu                     | Porty podłączone (w zakresie 0)                    |
| Rozpoczynanie odbiorcze                  | Juniper                                  |
| Model odbiorcze               | Numer katalogowy 740 021308 <br></br> Numer katalogowy 740 030658                   |
| Wersja oprogramowania układowego odbiorcze    | Rev 01 wersji 0.0 (zgłoszone)            |
| Kabel modelu                     | Dwustronnego zworek LC-LC 50-125µ, OM3, LSZH |
| StorSimple model                | 8600                                     |
| Wersja oprogramowania StorSimple     | 6.3.9600.17491                           |


## <a name="list-of-devices-tested-by-oem-provider-mellanox"></a>Listę urządzeń sprawdzone przez producenta OEM dostawcy (Mellanox)  

Mellanox zostało przetestowane następujące małych serwerowej podłączany odbiorcze (SFP), czy kable i przełączniki, aby upewnić się, aby działały optymalnie interfejsy sieci Mellanox, takie jak 10 interfejsów sieciowych GbE na urządzeniu StorSimple.

### <a name="cables-and-modules-supported-by-mellanox"></a>Kable i moduły obsługiwanych przez Mellanox 

W poniższej tabeli wymieniono kable i moduły obsługiwane przez Mellanox. Te nie zostały sprawdzone przez firmę Microsoft, ale prawdopodobnie do pracy z urządzeniem StorSimple.

| S. Wartość nie. | Szybkość | Model                 | Opis                                            | Wprowadź                |
|--------|-------|-----------------------|--------------------------------------------------------|-----------------------|
| 1.     | 10 GbE| CAB-SFP-SFP - 1M        | pasywne kabla miedziane SFP + 10 Gb/s 1m                   | Arista                |
| 2.     | 10 GbE| CAB-SFP-SFP - 2M        | pasywne kabla miedziane SFP + 10 Gb/s 2m                   | Arista                |
| 3.     | 10 GbE| CAB-SFP-SFP - 3M        | pasywne kabla miedziane SFP + 10 Gb/s 3m                   | Arista                |
| 4.     | 10 GbE| CAB-SFP-SFP - 5M        | pasywne kabla miedziane SFP + 10 Gb/s 5m                   | Arista                |
| 5.     | 10 GbE| Cisco SFP-H10GBCU1M   | Cisco SFP + kabla                                       | Cisco                 |
| 6.     | 10 GbE| Cisco SFP-H10GBCU3M   | Cisco SFP + kabla                                       | Cisco                 |
| 7.     |10 GbE | Cisco SFP-H10GBCU5M   | Cisco SFP + kabla                                       | Cisco                 |
| 8.     | 10 GbE| J9281B HP X242 10 G    | SFP + SFP + 1 mln bezpośrednio Podłącz kabel miedziane             | HP                    |
| 9.     | 10 GbE| BLc HP 455883 B21     | 10Gb SR SFP + i                                       | HP                    |
| 10.    | 10 GbE| BLc HP 455886 B21     | 10Gb LR SFP + i                                       | HP                    |
| 11.    |10 GbE | BLc HP 487649 B21     | SFP + 0,5 m 10GbE miedziane kabla                           | HP                    |
| 12.    |10 GbE | BLc HP 487652 B21     | SFP + 1 mln 10GbE miedziane kabla                             | HP                    |
| 13.    |10 GbE | BLc HP 487655 B21     | SFP + 3m 10GbE miedziane kabla                             | HP                    |
| 14.    |10 GbE | BLc HP 487658 B21     | SFP + 7m 10GbE miedziane kabla                             | HP                    |
| 15.    |10 GbE | BLc HP 537963 B21     | SFP + 5m 10GbE miedziane kabla                             | HP                    |
| 16.    |10 GbE | AP784A HP             | 3m serii C pasywne miedziane SFP + kabla                  | HP                    |
| 17.    |10 GbE | AP785A HP             | Kabel serii C pasywne miedziane SFP + 5m                  | HP                    |
| 18.    |10 GbE | AP818A HP             | 1 mln B serii aktywnej miedziane SFP + kabla                   | HP                    |
| 19.    |10 GbE | AP819A HP             | 3m B serii aktywnej miedziane SFP + kabla                   | HP                    |
| 20.    |10 GbE | J9150A HP             | X132 10 G SFP + LC SR odbiorcze                        | HP                    |
| 21.    |10 GbE | J9151A HP             | X132 10 G SFP + LC LR odbiorcze                        | HP                    |
| 22.    |10 GbE | J9283B HP             | X242 10 G SFP + SFP + 3 m DAC kabla                        | HP                    |
| 23.    |10 GbE | J9285B HP             | X242 10 G SFP + SFP + 7 m DAC kabla                        | HP                    |
| 24.    | 10 GbE| JD095B HP             | X240 10 G SFP + SFP + 0.65 m DAC kabla                     | HP                    |
| 25.    | 10 GbE| JD096B HP             | X240 10 G SFP + SFP + 1, 2 m DAC kabla                      | HP                    |
| 26.    | 10 GbE| JD097B HP             | X240 10 G SFP + SFP + 3 m Tato kabla                        | HP                    |
| 27.    | 10 GbE| Mellanox MAM1Q00A QSA | QSFP SFP + karty                                   | Technologie Mellanox |
| 28.    | 10 GbE| MC2309124 006 Mt      | X SFP+ pasywne kabla miedziane 1 do 24awg 10 Gb/s QSFP 7 m   | Technologie Mellanox |
| 29.    | 10 GbE| MC2309124 007 Mt      | X SFP+ pasywne kabla miedziane 1 do 24awg 10 Gb/s QSFP 7 m   | Technologie Mellanox |
| 30.    | 10 GbE| MC2309130 003 Mt      | X SFP+ pasywne kabla miedziane 1 do 30awg 10 Gb/s QSFP 3 m   | Technologie Mellanox |
| 31.    | 10 GbE| Mt MC2309130 00A      | X SFP+ pasywne kabla miedziane 1 do 30awg 10 Gb/s QSFP 0,5 m | Technologie Mellanox |
| 32.    | 10 GbE| MC3309124-005 Mt      | Pasywne miedziane kabla 1 24awg 10 Gb/s x SFP+ 5 m           | Technologie Mellanox |
| 33.    | 10 GbE| MC3309124 007 Mt      | Pasywne miedziane kabla 1 24awg 10 Gb/s x SFP+ 7 m           | Technologie Mellanox |
| 34.    | 10 GbE| MC3309130 003 Mt      | Pasywne miedziane kabla 1 30awg 10 Gb/s x SFP+ 3 m           | Technologie Mellanox |
| 35.    | 10 GbE| Mt MC3309130 00A      | Pasywne miedziane kabla 1 30awg 10 Gb/s x SFP+ 0,5 m         | Technologie Mellanox |

### <a name="switches-supported-by-mellanox"></a>Parametrów obsługiwanych przez Mellanox 

W poniższej tabeli przedstawiono przełączniki obsługiwane przez Mellanox. Te nie zostały sprawdzone przez firmę Microsoft, ale prawdopodobnie do pracy z urządzeniem StorSimple.

| S. Wartość nie. | Szybkość | Model | Opis                                                         | Wprowadź              |
|--------|-------|-------------|---------------------------------------------------------------------|-------------|
| 1.     | 10GbE | 516733 B21  | Przełącznik karta Ethernet HP ProCurve 6120XG 10GbE                      | HP    |
| 2.     | 10GbE | 538113 B21  | Moduł przekazującą 10GbE HP (PTM)                                  | HP    |
| 3.     | 10GbE | EN4093      | IBM PureFlex System tkaninie EN4093 10 Gigabit skalowalna Przełącz modułu | IBM   |
| 4.     | 1GbE  | 3020        | Karta Przełącz 1GbE Catalyst 3020 firmy Cisco                               | Cisco |
| 5.     | 1GbE  | 3020 X       | Karta przełącznik Catalyst 3020 X 1GbE Cisco                              | Cisco |
| 6.     | 1GbE  | 438030 B21  | Moduł Przełącz 1GbE HP — przełącznik karta Ethernet GbE2c warstwy 2-3       | HP    |
| 7.     | 1GbE  | 6120G       | Karta Przełącz 1GbE HP ProCurve 6120G-XG                              | HP    |

## <a name="next-steps"></a>Następne kroki

[Dowiedz się więcej o StorSimple sprzętowej i stan](storsimple-monitor-hardware-status.md).
