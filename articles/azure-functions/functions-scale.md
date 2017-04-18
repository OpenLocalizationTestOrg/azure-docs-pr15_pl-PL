<properties
   pageTitle="Jak przeskalować Azure funkcje | Microsoft Azure"
   description="Zrozumieć, jak funkcje Azure skalowanie do potrzeb obciążenie pracą z sterowane zdarzeniami."
   services="functions"
   documentationCenter="na"
   authors="dariagrigoriu"
   manager="erikre"
   editor=""
   tags=""
   keywords="Azure funkcje, funkcje przetwarzania, webhooks, dynamiczne obliczeń, pliki architektura"/>

<tags
   ms.service="functions"
   ms.devlang="multiple"
   ms.topic="reference"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="08/03/2016"
   ms.author="dariagrigoriu"/>

# <a name="how-to-scale-azure-functions"></a>Jak skalowanie funkcje Azure

## <a name="introduction"></a>Wprowadzenie

Zaletą funkcji Azure jest, że zasoby komputerowe są używane tylko w razie potrzeby. Oznacza to, nie zapłacić za pośrednictwem bezczynne SMS lub rezerwacji zdolności kiedy może być konieczne jej. Zamiast tego platformie przydziela power obliczeń po kodzie jest uruchomiony, skale w górę, w razie potrzeby do obsługi obciążenia i następnie skale w dół, gdy kod nie jest uruchomiony.

Mechanizm dla tej nowej funkcji jest plan usług dynamiczne.  

Ten artykuł zawiera omówienie sposobu działania plan usług dynamiczne, a także jak platformie na żądanie, aby uruchomić kod.

Jeśli nie jesteś jeszcze znanych z funkcjami Azure, upewnij się sprawdzić artykuł [Omówienie funkcji Azure](functions-overview.md) , aby lepiej zrozumieć możliwości.

## <a name="configure-azure-functions"></a>Konfigurowanie funkcji Azure

Dwa główne ustawienia dotyczą skalowania:

* [Plan usług aplikacji Azure](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) lub dynamicznego planu usługi
* Rozmiar pamięci w środowisku wykonanie

Koszt funkcji zmienia się w zależności od wybranej plan usług. Z planu usługi dynamiczne koszt jest to funkcja czas wykonywania, rozmiar pamięci i liczba wykonania. Opłaty są naliczane tylko wtedy, gdy faktycznie jest uruchomiony kodu.

Plan usług aplikacji obsługuje funkcje na istniejące maszyny wirtualne, które mogą być również uruchomić inny kod. Po zapłaceniu dla te maszyny wirtualne każdego miesiąca, są bezpłatne dodatkowe funkcje wykonanie nad nimi.

## <a name="choose-a-service-plan"></a>Wybierz plan usług

Po utworzeniu funkcje możesz wybrać, aby uruchomić te aplikacje na plan usługi dynamiczne lub [plan usług aplikacji](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).
W planie aplikacji usługi funkcje będzie działać w dedykowanej Głosowa, tak jak pracy aplikacji sieci web o Basic, Standard lub Premium wersji produktu.
Ten dedykowane maszyn wirtualnych przydzielonego do aplikacji i funkcje i jest zawsze dostępna czy kod jest aktywnie wykonywane lub nie. Jest dobrym rozwiązaniem, jeśli masz istniejących, w obszarze wykorzystane maszyny wirtualne uruchomione inny kod lub jeśli chcesz uruchomić funkcje przez cały czas lub prawie przez cały czas. Maszyny oddzielono koszt od rozmiaru zarówno środowisko uruchomieniowe i pamięci. Dzięki temu można ograniczyć koszt wiele funkcji długim kosztów maszyny wirtualne co najmniej jeden, które są uruchamiane w.

[AZURE.INCLUDE [Dynamic Service plan](../../includes/functions-dynamic-service-plan.md)]
