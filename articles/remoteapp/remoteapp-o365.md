
<properties
    pageTitle="Pakiet Office za pomocą Azure RemoteApp | Microsoft Azure" 
    description="Dowiedz się, jak pakiet Office i Azure RemoteApp współpracują ze sobą"
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

# <a name="using-office-with-azure-remoteapp"></a>Pakiet Office za pomocą Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp jest są przerywane. W tym artykule [anons](https://go.microsoft.com/fwlink/?linkid=821148) , aby uzyskać szczegółowe informacje.

Masz dwie możliwości obsługi aplikacji pakietu Office w Azure RemoteApp: Office 365 ProPlus lub Office 2013 Professional Plus w wersji próbnej.

**Hej czy wiesz, że mamy nowy, lepiej szybko zamienić ten artykuł? Sprawdź się, [jak korzystać z subskrypcji usługi Office 365 z Azure RemoteApp](remoteapp-officesubscription.md). Obejmuje wszystkie informacje niezbędne do przy użyciu usługi Office 365 + Azure RemoteApp.**

## <a name="office-365-proplus"></a>Usługa Office 365 ProPlus
Możesz utworzyć zbiór RemoteApp przy użyciu usługi Office 365 ProPlus obraz szablonu. Ta opcja pozwala na rozszerzenie RemoteApp usługi Office 365. Musi być istniejącego planu subskrypcji i użytkowników musi mieć licencję usługi Office 365 ProPlus albo autonomicznej lub za pośrednictwem usługi Office 365 usługi planów.

Funkcja RemoteApp obsługuje aktywacji udostępnionego komputera w programie pakietu Office 365. Po uzyskania udostępnionego komputera i użyj [Narzędzia wdrażania pakietu Office](http://www.microsoft.com/download/details.aspx?id=36778) dla instalacji usługi Office 365 ProPlus jest instalowana bez aktywowana. Gdy znaki użytkownika do kolekcji, która zawiera usługi Office 365, Office sprawdza, jeśli użytkownik został zainicjowany dla usługi Office 365 ProPlus. Jeśli tak, Office tymczasowo uaktywnia usługi Office 365 ProPlus — Ta aktywacja będzie nadal występował do tego znaki użytkowników poza usługę.

Aby użyć Aktywacja udostępnionego komputera w programie pakietu Office 365, należy utworzyć [szablon niestandardowy](remoteapp-create-custom-image.md) i instalowanie usługi Office 365 ProPlus, po [te wskazówki](https://technet.microsoft.com/library/dn782858.aspx).

Możesz zarządzać licencji usługi Office 365 użytkowników w [Portalu administracyjnym usługi Office 365](https://portal.office365.com/). Przeczytaj więcej informacji o [planach usługi Office 365](http://technet.microsoft.com/library/office-365-plan-options.aspx).  


## <a name="office-2013-professional-plus-trial"></a>Wersja próbna usługi Office 2013 Professional Plus
Podczas 30-dniową wersję próbną funkcja RemoteApp obraz szablonu Professional Plus (okres próbny) pakietu Office 2013 umożliwia tworzenie zbioru RemoteApp. Można przypisywać użytkowników do kolekcji wersji próbnej, przy użyciu ich konta pracy usługi Azure Active Directory lub konta Microsoft. Wymagane jest bez dodatkowych subskrypcji.

Jest to doskonałe rozwiązanie do grzybków opony i uzyskać dobre wrażenie dla pakietu Office w RemoteApp. Jednak ta opcja jest przeznaczone do oceny i testowania tylko. Kolekcje RemoteApp utworzony przy użyciu obraz szablonu Professional Plus (okres próbny) pakietu Office 2013 nie są przenoszone do trybu produkcji i zostanie wyłączona na koniec okresu próbnego.

## <a name="switching-from-trial-to-production"></a>Przenoszenie się z wersji próbnej do produkcji
Po uruchomieniu bezpłatną wersję próbną usługi 30-dniową notatki w sekcji RemoteApp portalu informujący, jak długo trwa pozostałej w próbie przed przejścia do płatnej konta. Można aktywować konto i Przełącz do trybu produkcji za pomocą łącza w tej notatce.

Po aktywowaniu konta wpłynie to wszystkie zbiory RemoteApp na Twoim koncie.

- Zbiory z systemem Windows Server 2012 R2 lub usługi Office 365 ProPlus obrazów szablonów będą bezproblemowe Przechodzenie do produkcji. Wszystkie dane użytkownika i ustawień, takich jak trwających sesji, pozostaną niezmienione.
- Jeśli zostały przekazane obrazy szablonu niestandardowego, zbiory przy użyciu tych obrazów będzie również bezproblemowe.
- Obraz szablonu Professional Plus (okres próbny) pakietu Office 2013 jest przeznaczona dla oceny tylko. Kolekcje korzystania z tego szablonu obrazu nie zmigrowanych z produkcją. Umieści w stanie "wyłączona".


Jeśli nie przejść do trybu produkcji przez upływem okresu próbnego, kolekcji RemoteApp zostaną wyłączone. Nie martw się — ustawienia i dane użytkowników są zapisywane w innym 90 dni, dzięki czemu nadal aktywowanie usługi i Przełącz do trybu produkcji bez utraty danych.
