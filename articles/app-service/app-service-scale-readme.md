<properties
    pageTitle="Usługa Azure aplikacji: Skalowanie aplikacji aplikacji usługi"
    description="Dowiedz się, wszystko skalowania aplikacji w aplikacji usługi."
    keywords="Aplikacja usługi azure aplikacji usługi, Skaluj skalowalna, plan usług aplikacji, koszt usługi aplikacji"
    services="app-service"
    documentationCenter=""
    authors="btardif"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/07/2016"
    ms.author="byvinyal"/>

# <a name="azure-app-service-scaling-app-service-applications"></a>Usługa Azure aplikacji: Skalowanie aplikacji aplikacji usługi

Aplikacji znajdującej się w usłudze Azure aplikacji można [uzyskać ogromną skalę](https://azure.microsoft.com/blog/canadian-broadcasting-corporation-radio-canada-leverage-azure-for-smooth-election-coverage/).
Jednak skalowania aplikacji jest złożone problemu, który nie ma rozwiązanie "jednego rozmiaru dopasować wszystkie". Poprawnie skali aplikacji są 3 kluczowe obszary, które umożliwi sukcesu aplikacji:

1. Opis architektury danej aplikacji i jego słabych.
    * Jest Stateful do aplikacji? Bezstanowa?
    * Co to są wszystkie składniki aplikacji?
        * Gdzie są gardła w aplikacji?
    * Podczas ładowania zostanie zastosowany do aplikacji, co spowoduje przerwanie pierwszy?
2. Opis oczekiwanych wymagań obciążenia i wydajność.
    * Czy aplikacja muszą służyć tysiąc użytkowników? lub milion?
    * Ruch będą z jednej lokalizacji geograficznej globalnie?
    * Czy istnieją sezonowy odmiany? wartości ruch?
    * Szybkość aplikacji należy odpowiedzieć? 1 sekundy? 1 milisekundy?
3. Opis i poprawnie wykorzystać platformy hostingu aplikacji.
    * Jakie funkcje wykorzystać do osiągnięcia Moje cele skali

W tej sekcji pomoże zrozumieć czynników i pomocy opracowywania strategii, która korzysta z funkcji aplikacji usługi niezbędne, aby osiągnąć cele skalowalność.

[AZURE.INCLUDE [app-service-blueprint-scaling-app-service-applications](../../includes/app-service-blueprint-scaling-app-service-applications.md)]
