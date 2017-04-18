<properties 
   pageTitle="Zarządzanie zasobami Azure za pomocą Eksploratora chmury | Microsoft Azure"
   description="Dowiedz się, jak przeglądać i zarządzać nimi Azure zasobów w programie Visual Studio za pomocą Eksploratora chmury."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="managing-azure-resources-with-cloud-explorer"></a>Zarządzanie zasobami Azure za pomocą Eksploratora chmury

##<a name="overview"></a>Omówienie

Eksplorator chmurze jest przeznaczony do pozwalają szybko i łatwo przeglądać i zarządzanie nimi Azure zasobów w programie Visual Studio IDE. Można, na przykład go używać, aby otworzyć aplikację sieci Web, w [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) lub w przeglądarce lub dołączanie debugowania, lub możesz wyświetlić właściwości kontenera obiektów blob i otwórz go w edytorze kontenera obiektów Blob.

Eksplorator chmurze jest wbudowany w stosie Menedżera zasobów Azure, tak jak [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040). Siebie zasobów, takich jak grup zasobów Azure i Azure usług, takich jak aplikacje logiki i aplikacje interfejsu API i obsługuje [Kontrola dostępu oparta na rolach](./active-directory/role-based-access-control-configure.md) (RBAC). Aby wyświetlić Azure zasoby, które zostały dodane lub zmienione, wybierz przycisk **Odśwież** na pasku narzędzi Eksploratora chmury.

Eksplorator chmurze jest instalowana jako część programu Visual Studio Tools dla Azure SDK 2.7. 

## <a name="prerequisites"></a>Wymagania wstępne

- Visual Studio RTM 2015 r.

- Narzędzia programu Visual Studio do Azure SDK. 
- Musi być konto Azure i być zalogowany go, aby wyświetlić Azure zasobów w Eksploratorze chmury. Jeśli nie masz, możesz utworzyć konta na kilka minut. Jeśli masz subskrypcję MSDN, zobacz [Azure korzyści dla subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/). W przeciwnym razie zobacz [Utwórz bezpłatne konto wersji próbnej](https://azure.microsoft.com/pricing/free-trial/).

- Jeśli Eksploratora chmury nie jest widoczne, można wyświetlić go, wybierając pozycję **Widok**, **Inne okna** **Eksploratora chmury** na pasku menu.

## <a name="manage-azure-accounts-and-subscriptions"></a>Zarządzanie kontami Azure i subskrypcji

Aby wyświetlić Azure zasobów w Eksploratorze w chmurze, musisz Zaloguj się na konto Azure z jedną lub więcej subskrypcji aktywna. Jeśli masz więcej niż jedno konto Azure, możesz je dodać w Eksploratorze w chmurze, a następnie wybierz subskrypcje, które chcesz uwzględnić w widoku zasobów Eksploratora chmury.

Jeśli nie była wcześniej używana Azure przed lub potrzebne konta nie zostały dodane do programu Visual Studio, zostanie wyświetlony monit, aby to zrobić.

## <a name="to-add-azure-accounts-to-cloud-explorer"></a>Aby dodać Azure kont do chmury Eksploratora

1. Wybierz ikonę ustawienia na pasku narzędzi Eksploratora chmury.

1. Wybierz pozycję Dodaj **konto** . Zaloguj się do konta Azure zasobów, którego chcesz przejść. Konto, które właśnie dodanego powinny być zaznaczone na liście rozwijanej wybór konta. Subskrypcje dla tego konta są wyświetlane w obszarze konta.

    ![Dodawanie Azure subskrypcji](./media/vs-azure-tools-resources-managing-with-cloud-explorer/IC819514.png)

    ![Wybieranie Azure subskrypcji](./media/vs-azure-tools-resources-managing-with-cloud-explorer/IC819515.png)

1. Zaznacz pola wyboru dla subskrypcji konta, który chcesz przeglądać, a następnie kliknij przycisk **Zastosuj** .

    Azure zasoby dla wybrane subskrypcje są wyświetlane w Eksploratorze chmury.

## <a name="to-remove-an-azure-account"></a>Aby usunąć konto Azure

1. Wybierz pozycję **plik**, **Ustawienia kont** na pasku menu.

1. W sekcji **Wszystkie konta** w oknie dialogowym **Ustawienia kont** wybierz polecenie **Usuń** obok konta, którą chcesz usunąć. Uwaga, że to polecenie usuwa tylko konta z programu Visual Studio — it nie wpływa na samo konto Azure.

## <a name="view-resource-types-or-groups"></a>Wyświetlanie typów zasobów lub grup

Aby wyświetlić Azure zasobów, możesz wybrać **Typów zasobów** lub widok **Grup zasobów** .

![Lista rozwijana widoku zasobów](./media/vs-azure-tools-resources-managing-with-cloud-explorer/IC819516.png)

- Widok **Typów zasobów** , w którym jest również typowych widok używany w [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040), zawiera Azure zasobów posortowane według typu, takich jak aplikacje sieci web, kont miejsca do magazynowania i maszyn wirtualnych. To jest podobne do zasobów jak Azure są wyświetlane w Eksploratorze serwera.

- Widok grup zasobów kategoryzuje Azure zasobów według grup zasobów Azure, z którymi są one związane z.

 
    Grupa zasobów to pakiet Azure zasobów, zazwyczaj używany przez określoną aplikację. Aby dowiedzieć się więcej o grupach zasobów Azure, zobacz [Omówienie Menedżera zasobów Azure](./resource-group-overview.md).

## <a name="view-and-navigate-resources"></a>Wyświetlanie i nawigowanie zasobów

Przejdź do zasobu Azure i wyświetlać informacje w Eksploratorze chmury, rozwiń typu elementu lub grupy zasobów, a następnie wybierz pozycję zasób. Po wybraniu zasobu informacje wyświetlane w dwóch kart u dołu Eksploratora chmury.

![Wybierz widok zasobów](./media/vs-azure-tools-resources-managing-with-cloud-explorer/IC819517.png)

- Na karcie **Akcje** zawiera akcje, które można wykonać w Eksploratorze w chmurze dla wybranego zasobu. Dostępne akcje można też wyświetlić w menu skrótów zasobu.

- Na karcie **Właściwości** są wyświetlane właściwości zasobu, takie jak jego typ, ustawień regionalnych i zasobów grupy, którą jest skojarzony.

Każdy zasób ma **otwarty w portalu**akcji. Po wybraniu tej akcji w chmurze Eksplorator jest wyświetlana wybranego zasobu [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040). Ta funkcja jest szczególnie przydatne do przechodzenia do mocno zagnieżdżone zasobów.

Dodatkowe akcje i wartości właściwości mogą również są wyświetlane na podstawie Azure zasobów. Na przykład aplikacji web apps i aplikacjach logiczny mieć także akcji, **Otwórz w przeglądarce** i **Dołącz debugowania** oprócz **otwarty w portalu**. Akcje, aby otworzyć edytory są wyświetlane po wybraniu obiektów blob konta miejsca do magazynowania, kolejki lub tabelę. Azure aplikacji jest właściwości **adresu URL** i **Stan** , podczas zasobów magazynowania jest właściwości ciągu klucza i połączenia.

## <a name="search-resources"></a>Zasoby wyszukiwania

Aby zlokalizować zasoby o określonej nazwie w subskrypcji konto Azure, wprowadź nazwę w polu wyszukiwania w Eksploratorze chmury.

![Znajdowanie zasobów w Eksploratorze chmury](./media/vs-azure-tools-resources-managing-with-cloud-explorer/IC820394.png)

Podczas wpisywania znaków w polu wyszukiwania w drzewie zasobów są wyświetlane tylko te zasoby, które są zgodne z tych znaków.

