<properties
   pageTitle="Za pomocą łączników Azure zasobu w aplikacjach logiczny | Microsoft Azure aplikacji usługi"
   description="Jak utworzyć i skonfigurować aplikację łącznika zasobów Azure lub interfejsu API i używać go w aplikacji dla logiki Azure aplikacji usługi"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="stepsic-microsoft-com"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="09/01/2016"
   ms.author="stepsic"/>

# <a name="get-started-with-the-azure-resource-connector-and-add-it-to-your-logic-app"></a>Wprowadzenie do łącznika zasobów Azure i dodać go do aplikacji warunków logicznych
>[AZURE.NOTE] Tej wersji artykuł dotyczy wersji schematu 2014-12-01-podgląd aplikacji logika.

Łącznik zasobów Azure umożliwia łatwe zarządzanie zasobami Azure wewnątrz aplikacji logicznych.

## <a name="create-the-azure-resource-connector"></a>Tworzenie łącznika Azure zasobów
Aby użyć aplikacji interfejsu API łącznik zasobów Azure, należy najpierw utworzyć wystąpienie. Można to zrobić albo w tekście podczas tworzenia aplikacji logika lub wybierając aplikacji API łącznik menedżera zasobów Azure z usługi Azure Marketplace.

Aby skonfigurować jego, masz co jest potrzebne do ustawiania w górę wystawcy usługi z uprawnieniami do dowolnego elementu jest potrzebne do wykonania w Azure. Wszystkie połączenia zostaną wprowadzone w imieniu z tego podmiotu usługi skonfigurowanego. Dzięki temu umożliwiające zakresu łącznik, aby używać tylko dokładnie co ma być wykonaj i nic więcej.

David Ebbo został zapisany [Doskonałe blogu](http://blog.davidebbo.com/2014/12/azure-service-principal.html) na temat go skonfigurować. Wykonaj wszystkie instrukcje i Uzyskaj swój **Identyfikator dzierżawy**, **Identyfikator klienta** i **hasło**. Tych trzech pól oraz **Identyfikator subskrypcji**są, jakie są wymagane do skonfigurowania łącznik.

## <a name="using-the-azure-resource-connector-in-logic-apps-designer"></a>Używanie łącznika zasobów Azure w Projektancie aplikacji logika
### <a name="trigger"></a>Wyzwalacza
Istnieją dwa wyzwalaczy, które są obsługiwane w programie Connector:

Nazwa | Opis
---- | -----------
Zdarzenie | Wyzwalacza po wystąpieniu zdarzenia do zasobu w ramach subskrypcji.
Metryka przecina progowa. |  Wyzwalacz, gdy metryki spełnia określonej wartości progowej.

### <a name="action"></a>Akcja

Ponadto może zapewnić dużej liczby akcji wewnątrz Azure subskrypcji:

Dla **grup zasobów** można wykonywać następujące czynności:

Nazwa | Opis
---- | -----------
Lista grup zasobów | Lista wszystkich grup zasobów w subskrypcji.
Uzyskiwanie grupa zasobów | Aby uzyskać identyfikatora grupy zasobów.
Tworzenie grupy zasobów | Tworzenie lub aktualizowanie grupy zasobów.
Usuwanie grupy zasobów | Usuwanie grupy zasobów.

Dla **zasobów** można wykonywać następujące czynności:

Nazwa | Opis
---- | -----------
Gdy zasoby dla listy | Zasoby dla listy w ramach subskrypcji przez różne typy filtrów.
Pobieranie zasobu | Uzyskiwanie pojedynczego zasobu według jego zasobów identyfikatora.
Tworzenie lub aktualizowanie zasobów | Utwórz zasób lub zaktualizować istniejący zasób. Należy podać wszystkie właściwości dla tego zasobu.
Akcja zasobów |  Wykonywanie innych akcji na zasób. Musisz znać nazwę akcji i ładunek kierujące ta akcja (jeśli istnieje).
Usuwanie zasobu | Usuwanie zasobu.

Dla **Zasobu dostawców** można wykonywać następujące czynności:

Nazwa | Opis
---- | -----------
Lista dostawców zasobów | Lista wszystkich dostawców zasobów dostępnych w subskrypcji.

W przypadku **Wdrożeń grupy zasobów** można wykonywać następujące czynności:

Nazwa | Opis
---- | -----------
Lista wdrożenia | Lista wszystkich wdrożeń w grupie zasobów.
Uzyskiwanie wdrażania | Aby uzyskać rozmieszczania szablonu identyfikatora.
Tworzenie wdrażania | Tworzenie nowego wdrożenia grupa zasobów, definiując szablon.

Dla **zdarzenia** o zasobach można wykonywać następujące czynności:

Nazwa | Opis
---- | -----------
Zdarzenia | Uzyskaj zdarzeń w subskrypcji lub zasobu.

W przypadku **metryk** o zasobach można wykonywać następujące czynności:

Nazwa | Opis
---- | -----------
Uzyskiwanie metryki | Uzyskiwanie metryki dla zasobu identyfikatora.

## <a name="do-more-with-your-connector"></a>Więcej możliwości korzystania z tego łącznika
Teraz, gdy łącznik jest tworzony, możesz dodać go do przepływu firm przy użyciu aplikacji logicznych. Zobacz [Co to są aplikacje logika?](app-service-logic-what-are-logic-apps.md).

>[AZURE.NOTE] Jeśli chcesz rozpocząć pracę z aplikacji logika Azure przed utworzeniem konta dla konta Azure, przejdź do [aplikacji spróbuj logiczny](https://tryappservice.azure.com/?appservice=logic), miejsce, w którym możesz od razu utworzyć krótkotrwałe starter logiczny aplikację w aplikacji usługi. Nie kart kredytowych wymagane; nie zobowiązania.

Wyświetlanie odwołania Swagger interfejsu API usługi REST w [łączników i interfejsu API aplikacje odwołanie](http://go.microsoft.com/fwlink/p/?LinkId=529766).

<!--References -->

<!--Links -->
[Creating a Logic app]: app-service-logic-create-a-logic-app.md
