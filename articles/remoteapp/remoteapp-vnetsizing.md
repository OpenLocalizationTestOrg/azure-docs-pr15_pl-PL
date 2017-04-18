
<properties
    pageTitle="Zmiany rozmiaru i informacje dotyczące VNET w Azure RemoteApp | Microsoft Azure"
    description="Dowiedz się więcej o wymaganiach adres IP dotyczących korzystania z VNET RemoteApp Azure"
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />



# <a name="sizing-information-for-a-vnet-in-azure-remoteapp"></a>Informacje dotyczące zmiany rozmiaru dla VNET w Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp jest są przerywane. W tym artykule [anons](https://go.microsoft.com/fwlink/?linkid=821148) , aby uzyskać szczegółowe informacje.

Jeśli używasz Azure RemoteApp z wirtualną sieć (VNET) RemoteApp używa adresów IP w podsieci. W zależności od skali RemoteApp usługi, należy się upewnić, że danej podsieci ma za mało adresów IP dostępnych dla maszyn wirtualnych RemoteApp. Podczas tych wskazówek zmiany rozmiaru nie jest doskonałe danej jak funkcja RemoteApp dynamicznie obraca się i obraca się w dół maszyn wirtualnych w obrębie zbioru, pomoże Ci oszacować zakres podsieci. Jest to szczególnie ważne, podczas gdy funkcja RemoteApp usługi znajduje się w VNET, nie można zwiększyć rozmiar podsieci bez usuwania RemoteApp.

Dla każdego zbioru RemoteApp, który chcesz uruchomić z maksymalną wydajnością należy użyć 100 dostępnych adresów IP. Na przykład jeśli masz jeden zbiór RemoteApp w planie standardowy i chcesz mieć maksymalną 500 użytkowników, trzeba mieć 100 adresów IP dla tej kolekcji. Podobnie potrzebne 100 adresów IP dla zbioru RemoteApp podstawowy plan, który ma 800 użytkowników. Jeśli planujesz na komputerach użytkowników mniej (mniejsze niż maksimum), można zmniejszyć potrzebne na zbiór adresów IP. Wymagany rozmiar minimalny podsieci jest 30 adresów IP (/ 27).

Poniższe informacje, aby upewnić się, że usługi VNET jest skonfigurowany i działa propertly:

- [Migrowanie z osobistego VNET do VNET Azure](remoteapp-migratevnet.md)
- [Sprawdź poprawność VNET Azure do użytku z Azure RemoteApp](remoteapp-vnet.md)
