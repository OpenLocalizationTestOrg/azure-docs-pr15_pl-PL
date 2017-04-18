<properties
    pageTitle="Szczegółowe omówienie plany Azure aplikacji usługi | Microsoft Azure"
    description="Dowiedz się, jak plany aplikacji usługi Azure aplikacji usługi pracy, a korzyści do środowiska zarządzania."
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
    ms.date="10/13/2016"
    ms.author="byvinyal"/>

# <a name="azure-app-service-plans-in-depth-overview"></a>Szczegółowe omówienie plany Azure aplikacji usługi#

Plan usług aplikacji reprezentuje zestaw funkcji i możliwości, którą można udostępnić w wielu aplikacjach. Aplikacje sieci Web, aplikacji Mobile, funkcja, aplikacji lub aplikacji interfejsu API, w [Usłudze Azure aplikacji](http://go.microsoft.com/fwlink/?LinkId=529714) wszystkich Uruchom w planie aplikacji usługi. Plany te obsługuje pięciu poziomów cen: *wolny*, *udostępnione*, *podstawowe*, *Standard*i *Premium*. Każdą ma własne funkcje i możliwości. Aplikacje w samej subskrypcji i lokalizacji geograficznej można udostępniać planu. Wszystkie aplikacje, które współużytkują planu można używać wszystkich funkcji i funkcje, które są definiowane przez warstwa planu. Wszystkie aplikacje, które są skojarzone z planem uruchamiać na zasoby, które definiuje planu.

Na przykład jeśli plan jest skonfigurowany do używania dwa wystąpienia "małe" w warstwie standardowa usługa, wszystkie aplikacje, które są skojarzone z planu Uruchom w obu przypadkach i mieć dostęp do funkcji warstwa standardowa usługa. Wystąpienia plan, na których są uruchomione aplikacje są w pełni zarządzane i wysokiej dostępności.

Ten artykuł opisuje najważniejszych cech, takich jak warstwa i skali, plan usług aplikacji i sposobu ich powstawania do odtwarzania aplikacji i zarządzania nimi.

## <a name="apps-and-app-service-plans"></a>Aplikacje i plany aplikacji usługi

W aplikacji usługi aplikacji może być skojarzony tylko jeden plan aplikacji usług w dowolnym momencie.

Zarówno aplikacje i plany znajdują się w grupie zasobów. Grupa zasobów służy jako granicę cyklu życia dla każdego zasobu, który jest w nim. Grupy zasobów umożliwia zarządzanie razem wszystkich części aplikacji.

Ponieważ pojedynczej grupy zasobów może mieć wiele planów usług aplikacji, można przydzielić poszczególnych aplikacji do różnych zasobów fizycznych. Można na przykład oddzielnych zasobów między środowiskami deweloperów, badań i produkcji. O osobnych środowiskach produkcji i deweloperów/testowanie pozwala wyodrębnienia zasobów. W ten sposób ładowania testowanie przed nową wersję aplikacji powodują konfliktu dla tych samych zasobów co aplikacji produkcji, których używasz rzeczywistych klientów.

Jeśli masz wiele planów w pojedynczej grupy zasobów, można także zdefiniować aplikację, która obejmuje regionach geograficznych. Na przykład wysokiej dostępności aplikacji działa w dwóch regionów zawiera co najmniej dwa plany, jedną dla każdego regionu i jedną aplikację skojarzone z każdego planu. W takiej sytuacji wszystkie kopie aplikacji następnie znajdują się w pojedynczej grupy zasobów. Masz nową grupę zasobów o wiele planów i wielu aplikacji ułatwia zarządzanie, kontrolowanie i wyświetlanie kondycji aplikacji.

## <a name="create-an-app-service-plan-or-use-existing-one"></a>Utwórz plan usług aplikacji lub użyj istniejącego wzornika

Gdy tworzysz aplikację, warto rozważyć utworzenie grupy zasobów. Z drugiej strony jeśli aplikację, której zamierzasz utworzyć jest składnikiem większej aplikacji, ta aplikacja powinny być tworzone w grupie zasobu przydzielonej dla tej aplikacji większe.

Czy nowej aplikacji jest całkowicie nowej aplikacji lub jej części większego, możesz udostępnić go lub Utwórz nowy za pomocą istniejącego planu aplikacji usługi. Niniejsza decyzja jest bardziej pytanie pojemnością i oczekiwanych.

Jeśli to nowa aplikacja będzie korzystać z wielu zasobów i mają różne skalowania czynniki z innych aplikacji obsługiwany w istniejący plan, zalecamy wyodrębnienia w osobnym planu.

Podczas tworzenia planu, można przydzielić nowy zestaw zasobów dla aplikacji i uzyskać większą kontrolę nad alokacji zasobów, ponieważ każdy plan otrzymuje własny zestaw wystąpień.

Ponieważ aplikacje można przenieść w planach, możesz zmienić sposób, w jaki zasoby są przydzielone przez zastosowanie większego aplikacji.

Na koniec Jeśli chcesz utworzyć aplikację w innym regionie, a tego regionu nie ma istniejącego planu, Utwórz plan w danym regionie, aby móc udostępniać aplikacji.

## <a name="create-an-app-service-plan"></a>Tworzenie planu aplikacji usługi

>[AZURE.TIP] Jeśli masz środowisku usługi aplikacji możesz przejrzeć dokumentację specyficzne dla środowiska usługi aplikacji tutaj: [Tworzenie aplikacji usługi Planowanie w środowisku usługi aplikacji](../app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase.md#createplan)

Możesz utworzyć pusty plan usług aplikacji możliwości Przeglądaj plan usługi aplikacji lub w ramach tworzenia aplikacji.

W [portalu Azure](https://portal.azure.com), kliknij przycisk **Nowy** > **Web + mobile**, a następnie wybierz pozycję **Aplikacji sieci Web** lub innego typu aplikacji aplikacji usługi.
![Tworzenie aplikacji programu w portalu Azure.][createWebApp]

Można zaznaczyć lub tworzenie planu aplikacji usług dla nowej aplikacji.

 ![Tworzenie planu aplikacji usługi.][createASP]

Aby utworzyć nowy plan usług aplikacji, kliknij przycisk **[+] Utwórz nowe**, wpisz nazwę **plan usług aplikacji** , a następnie wybierz odpowiednią **lokalizację**. Kliknij pozycję **ceny warstwy**, a następnie wybierz odpowiedni poziom cennik usługi. Wybierz pozycję **Wyświetl wszystkie** , aby wyświetlić więcej opcji cennik, takich jak **bezpłatnego** i **udostępnione**. Po wybraniu cennik warstwy, kliknij przycisk **Wybierz** .

## <a name="move-an-app-to-a-different-app-service-plan"></a>Przenoszenie aplikacji do innego planu usługi aplikacji

Aplikację można przenosić do innej aplikacji planu usługi [Azure portal](https://portal.azure.com). Aplikacje można przenosić między planami, ile plany są w tej samej grupy zasobów i regionu geograficznego.

Aby przenieść aplikację do innego planu, przejdź do aplikacji, którą chcesz przenieść. W menu **Ustawienia** , poszukaj **Zmień aplikacji usługi Planowanie**.

**Plan usług aplikacji zmiana** zostanie otwarty selektor **plan usług aplikacji** . Na tym etapie można wybrać istniejący plan lub Utwórz nowy. Są wyświetlane tylko prawidłowe plany (w tej samej grupy zasobów i lokalizacji geograficznej).

![Selektor plan aplikacji usługi.][change]

Każdy plan ma własny ceny warstwy. Na przykład po umieszczeniu witryny z bezpłatnego warstwy do standardowego warstwy aplikacji teraz służy wszystkich funkcji i zasobów warstwie standardowy.

## <a name="clone-an-app-to-a-different-app-service-plan"></a>Klonowanie aplikacji do innego planu usługi aplikacji
Jeśli chcesz przejść do innego obszaru aplikacji, zamiast jednego to aplikacja klonowanie. Klonowanie tworzy kopię aplikacji w środowisku usługi aplikacji w dowolnym regionie lub nowym lub istniejącym plan usług aplikacji.

 ![Klonowanie aplikacji.][appclone]

**Aplikacja klonowanie** można znaleźć w menu **Narzędzia** .

Klonowanie ma pewne ograniczenia, które można znaleźć informacje w [aplikacji usługi aplikacji Azure klonowanie, za pomocą portalu Azure](../app-service-web/app-service-web-app-cloning-portal.md).

## <a name="scale-an-app-service-plan"></a>Skala plan usług aplikacji

Istnieją trzy sposoby skalowanie planu:

- **Zmienianie planu ceny warstwy**. Na przykład plan w warstwie podstawowe można przekonwertować na warstwie Standard lub Premium, a wszystkie aplikacje, które są teraz skojarzone z planu, korzystając z funkcji, które oferuje nowy poziom usługi.
- **Zmienianie rozmiaru wystąpienia planu**. Na przykład można zmienić plan w korzystającego z małych wystąpienia warstwie podstawowe używać dużych wystąpienia. Wszystkie aplikacje, które są skojarzone z tym plan teraz można użyć dodatkowej pamięci i zasoby Procesora, które udostępnia większy rozmiar wystąpienie.
- **Zmienianie licznik wystąpień planu**. Na przykład standardowy plan, który skalowania trzy wystąpienia można skalować 10 wystąpieniach. Premium plan można skalować się 20 wystąpieniach (pod warunkiem dostępności). Wszystkie aplikacje, które są teraz skojarzone z planu można użyć dodatkowa pamięć i zasoby Procesora, które zapewnia większą liczba wystąpienia.

Cennik warstwy i wystąpienie rozmiar można zmienić, klikając pozycję **Skala w górę** w obszarze Ustawienia aplikacji lub plan usług aplikacji. Zmiany dotyczą plan usług aplikacji i wpływa na wszystkie aplikacje, które obsługuje.

 ![Ustawianie wartości rozbudowy aplikacji.][pricingtier]

## <a name="app-service-plan-cleanup"></a>Oczyszczanie Plan usług aplikacji
**Plany aplikacji usługi** nie aplikacje skojarzony z nich nadal spowodować opłaty, ponieważ nadal rezerwowanie wydajności skonfigurowane we właściwościach skali plan aplikacji usługi.
Aby uniknąć nieoczekiwanych opłaty po usunięciu ostatniej aplikacji hostowanej w planie aplikacji usługi, otrzymane pusty plan usług aplikacji również zostanie usunięty.


## <a name="summary"></a>Podsumowanie

Plany aplikacji usługi reprezentuje zestaw funkcji i możliwości, którą można udostępnić w aplikacji. Plany aplikacji usługi zapewniają możliwość przydzielanie określonej aplikacji do wielu zasobów i następnie optymalizować wykorzystanie usługi Azure zasobów. W ten sposób, aby zaoszczędzić na środowiska testowego, możesz udostępnić planu w wielu aplikacjach. Można również maksymalizowanie przepustowości dla środowisku produkcyjnym przez skalowania go w wielu regionów i planów.

## <a name="whats-changed"></a>Informacje o zmianach

* Przewodnika do zmiany z witryn sieci Web do usługi aplikacji, zobacz: [Usługa Azure aplikacji i jego wpływ na istniejące usługi Azure](http://go.microsoft.com/fwlink/?LinkId=529714)

[pricingtier]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/appserviceplan-pricingtier.png
[assign]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/assing-appserviceplan.png
[change]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/change-appserviceplan.png
[createASP]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/create-appserviceplan.png
[createWebApp]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/create-web-app.png
[appclone]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/app-clone.png
