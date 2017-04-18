<properties
   pageTitle="Internet najważniejsze wskazówki dotyczące zabezpieczeń czynności | Microsoft Azure"
   description="Artykuł zawiera listę curated Microsoft Internet najważniejsze wskazówki dotyczące zabezpieczeń czynności i zaleceń ogólne."
   services="security"
   documentationCenter="na"
   authors="TomShinder"
   manager="StevenPo"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/25/2016"
   ms.author="yurid"/>

# <a name="internet-of-things-security-best-practices"></a>Internet najważniejsze wskazówki dotyczące zabezpieczeń czynności

Zabezpieczanie infrastruktury Internet czynności (IoT) jest przedsiębiorstwo krytyczne dla odbiorców związane z IoT rozwiązań. Ze względu na liczbę urządzeń związane z i Rozproszony charakter tych urządzeń wpływ zdarzenie zabezpieczeń związanych z złamanie milionów urządzeń IoT jest wartością uproszczony i mogą mieć wpływ szerokie.

Z tego powodu zabezpieczeń IoT musi podejście zabezpieczeń w głębokości. Konieczne jest bezpieczny w chmurze danych i w sieciach prywatne i publiczne poruszający się. Metody muszą być w miejscu bezpieczne inicjowania obsługi urządzeń IoT się. Każdej warstwy, na urządzeniu z siecią do chmury wewnętrznej musi gwarancje silne zabezpieczenia.

Najważniejsze wskazówki IoT można podzielić w następujący sposób:

- Producent sprzętu IoT lub integrator
- Deweloper rozwiązanie IoT
- Narzędzie wdrażania rozwiązania IoT
- Operator rozwiązanie IoT

W tym artykule wymieniono [Internet z poleceń najważniejsze wskazówki dotyczące zabezpieczeń](../iot-suite/iot-security-best-practices.md). Zajrzyj do tego artykułu, aby uzyskać szczegółowe informacje.

## <a name="iot-hardware-manufacturer-or-integrator"></a>Producent sprzętu IoT lub integrator

Zgodne z najważniejszymi wskazówkami poniżej, w przypadku produkcji sprzętu IoT lub integrator sprzętu:

- **Zakres sprzętu, aby minimalne wymagania**: projektowanie sprzętu powinien zawierać funkcje minimalne wymagane do działania sprzętu i nic więcej. 
- **Wprowadź sprzętu modyfikować dowód**: tworzenie w mechanizmy wykrywanie fizycznego manipulowania sprzętu, takich jak otwieranie okładki urządzenia, usuwając część urządzenia, itp. 
- **Tworzenie wokół bezpiecznego sprzętu**: Jeśli [KWS](https://en.wikipedia.org/wiki/Cost_of_goods_sold) zezwalają, tworzenie funkcji zabezpieczeń, takich jak funkcje oparte na moduł platformy TPM uruchamiania i bezpiecznych i zaszyfrowanych magazynowania.
- **Rozpoczynanie uaktualnienia bezpiecznego**: uaktualnianie oprogramowania układowego okresie istnienia urządzenie jest nieunikniona.

## <a name="iot-solution-developer"></a>Deweloper rozwiązanie IoT

Jeśli jesteś deweloperem rozwiązanie IoT, wykonaj poniższe wskazówki:

- **Obserwuj bezpiecznego oprogramowania rozwój metodologii**: opracowywanie bezpiecznych oprogramowania wymaga podstaw Myślę o zabezpieczeniach od momentu rozpoczęcia projektu maksymalnie jego wykonania, testowanie i wdrażanie.
- **Wybierz pozycję Otwórz źródło oprogramowanie z rozwagą**: Otwórz źródło oprogramowania oferuje możliwość szybkiego tworzenia rozwiązań.
- **Integracja z rozwagą**: istnieje wiele wady zabezpieczeń oprogramowania na granicy bibliotek oraz interfejsy API. 

## <a name="iot-solution-deployer"></a>Narzędzie wdrażania rozwiązania IoT

W przypadku narzędzie wdrażania rozwiązania IoT, wykonaj poniższe wskazówki:

- **Wdrażanie sprzętu bezpieczne**: wdrożeń IoT może wymagać sprzętu, który ma być używany w lokalizacjach niezabezpieczoną, takich jak spacje publicznej lub bez kontroli ustawień regionalnych.
- **Zabezpieczyć kluczy uwierzytelniania**: podczas wdrażania, każde urządzenie wymaga identyfikatory urządzeń i skojarzone kluczy uwierzytelniania generowanych przez usługi w chmurze. Zabezpieczyć klucze fizycznie nawet po wdrożeniu. Każdy złamany klucz mogą być używane przez złośliwy urządzenia do zamaskowane pod jako istniejące urządzenie.

## <a name="iot-solution-operator"></a>Operator rozwiązanie IoT

Zgodne z najważniejszymi wskazówkami poniżej, jeśli jesteś operatora rozwiązanie IoT:

- **Aktualizowanie systemów**: Upewnij się, systemów operacyjnych urządzenia i wszystkie sterowniki zostaną zaktualizowane do najnowszej wersji. 
- **Ochrona przed złośliwych działań**: jeśli zezwala na system operacyjny, umieść najnowszych funkcji oprogramowania antywirusowego i złośliwym we wszystkich systemach operacyjnych urządzenia. 
- **Często inspekcji**: inspekcja IoT infrastruktury zabezpieczeń związanych z problemów jest klucz wysyłanej z bezpieczeństwem.
- **Fizycznie ochrony infrastruktury IoT**: zabezpieczenia najgorszego atakami infrastruktury IoT uruchamiania przy użyciu fizycznego dostępu do urządzenia.
- **Ochrona chmurze poświadczenia**: cloud uwierzytelniania poświadczenia używane do konfigurowania i obsługi wdrożenia usługi IoT są Najłatwiejszym sposobem uzyskania dostępu i złamanie IoT. 
