<properties 
    pageTitle="Wdrażanie zasobów Azure za pomocą Azure portal | Microsoft Azure" 
    description="Wdrażanie zasobów za pomocą Azure portal i zarządzanie zasobu Azure." 
    services="azure-resource-manager,azure-portal" 
    documentationCenter="" 
    authors="tfitzmac" 
    manager="timlt" 
    editor="tysonn"/>

<tags 
    ms.service="azure-resource-manager" 
    ms.workload="multiple" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/15/2016" 
    ms.author="tomfitz"/>

# <a name="deploy-resources-with-resource-manager-templates-and-azure-portal"></a>Wdrażanie zasobów z szablonami Menedżera zasobów i Azure portal

> [AZURE.SELECTOR]
- [Programu PowerShell](resource-group-template-deploy.md)
- [Polecenie Azure](resource-group-template-deploy-cli.md)
- [Portal](resource-group-template-deploy-portal.md)
- [INTERFEJSU API USŁUGI REST](resource-group-template-deploy-rest.md)

W tym temacie przedstawiono sposób [Azure portal](https://portal.azure.com) za pomocą z [Menedżera zasobów Azure](azure-resource-manager/resource-group-overview.md) do wdrażania usługi Azure zasobów. Aby uzyskać informacje o zarządzaniu zasobów, zobacz [Zarządzanie Azure zasoby portalu](./azure-portal/resource-group-portal.md).

Obecnie nie każdy usługa obsługuje portalu lub Menedżer zasobów. Dla tych usług należy korzystać z [portalu klasyczny](https://manage.windowsazure.com). Stan każdej usługi zobacz [dostępność portal Azure wykresu](https://azure.microsoft.com/features/azure-portal/availability/).

## <a name="create-resource-group"></a>Tworzenie grupy zasobów

1. Aby utworzyć grupę zasobów puste, wybierz pozycję **Nowy** > **zarządzania** > **Grupa zasobów**.

    ![Tworzenie grupy zasobów puste](./media/resource-group-template-deploy-portal/create-empty-group.png)

2. Nadaj nazwy i lokalizacji, a w razie potrzeby wybierz subskrypcję. Należy podać lokalizację, w której grupa zasobów, ponieważ grupa zasobów przechowuje metadane o zasobach. Ze względów zgodności można określić miejsce, w którym są przechowywane metadane. Ogólnie zaleca się, aby określić lokalizację, w której miejsce, w którym znajdują się najczęściej zasobów. Za pomocą tej samej lokalizacji może uprościć szablonu.

    ![Ustawianie grupy wartości](./media/resource-group-template-deploy-portal/set-group-properties.png)

## <a name="deploy-resources-from-marketplace"></a>Wdrażanie zasobów z witryny Marketplace

Po utworzeniu grupy zasobów, należy wdrożyć zasobów do niego z witryny Marketplace. Rynku zawiera wstępnie zdefiniowanych rozwiązań typowych scenariuszy.

1. Aby wdrożeniu, wybierz pozycję **Nowy** , a typ zasobu, który chcesz wdrożyć. Następnie znajdź określonej wersji zasobów, które chcesz wdrożyć.

    ![Wdrażanie zasobów](./media/resource-group-template-deploy-portal/deploy-resource.png)

2. Jeśli nie widzisz danego rozwiązania, które chcesz wdrożyć, można wyszukiwać rynku go.

    ![Wyszukiwanie w witrynie marketplace](./media/resource-group-template-deploy-portal/search-resource.png)

3. W zależności od typu wybranego zasobu masz zbiór odpowiednie właściwości, aby ustawić przed wdrożeniem. Te opcje nie są wyświetlane w tym miejscu, zgodnie z ich różnią się w zależności od typu zasobu. Dla wszystkich typów możesz wybrać grupę zasobów miejsca docelowego. Na poniższej ilustracji przedstawiono sposób tworzenia aplikacji sieci web i Wdroż grupę zasobów, która została utworzona.

    ![Tworzenie grupy zasobów](./media/resource-group-template-deploy-portal/select-existing-group.png)

    Alternatywnie można zdecydować utworzyć grupę zasobu podczas wdrażania zasobów. Wybierz pozycję **Utwórz nowe** i nadaj nazwę grupie zasobów.

    ![Tworzenie nowej grupy zasobów](./media/resource-group-template-deploy-portal/select-new-group.png)

4. Rozpoczyna się wdrożenia. Wdrażanie może potrwać kilka minut. Po zakończeniu wdrażania, zobacz powiadomienia.

    ![Wyświetl powiadomienie](./media/resource-group-template-deploy-portal/view-notification.png)

5. Po wdrożeniu zasobów, możesz dodać więcej zasobów do grupy zasobów za pomocą polecenia **Dodaj** w karta Grupa zasobów.

    ![Dodawanie zasobu](./media/resource-group-template-deploy-portal/add-resource.png)

## <a name="deploy-resources-from-custom-template"></a>Wdrażanie zasoby z szablonu niestandardowego

Jeśli chcesz realizacji wdrożenia, ale nie używać tych szablonów na rynku, możesz utworzyć szablon niestandardowy, definiujące infrastruktury dla tego rozwiązania. Aby uzyskać informacje dotyczące tworzenia szablonów, zobacz [Szablony do tworzenia Azure Menedżera zasobów](resource-group-authoring-templates.md).

1. Aby wdrożyć szablon niestandardowy za pośrednictwem portalu, wybierz pozycję **Nowy**, a następnie rozpocząć wyszukiwanie **Rozmieszczania szablonu** , dopóki nie można wybierać opcje.

    ![wdrożenie szablonu wyszukiwania](./media/resource-group-template-deploy-portal/search-template.png)

2. Wybierz **Szablon wdrożenia** z dostępnych zasobów.

    ![Wybierz szablon wdrożenia](./media/resource-group-template-deploy-portal/select-template.png)

3. Po uruchomieniu rozmieszczania szablonu, otwórz pustego szablonu, który jest dostępny do dostosowywania.

    ![Tworzenie szablonu](./media/resource-group-template-deploy-portal/show-custom-template.png)

    W edytorze Dodaj składni JSON, która definiuje zasoby, które chcesz wdrożyć. Wybierz przycisk **Zapisz** , po zakończeniu. Aby uzyskać wskazówki dotyczące pisania składni JSON zobacz [Instruktaż szablonu Menedżera zasobów](resource-manager-template-walkthrough.md).

    ![Edytowanie szablonu](./media/resource-group-template-deploy-portal/edit-template.png)

4. Możesz również można wybrać istniejącego szablonu z [szablonów Azure Szybki Start](https://azure.microsoft.com/documentation/templates/). Te szablony są tworzone przez społeczność. Obejmują wiele typowych scenariuszy, a następnie ktoś dodaje szablonu, który jest podobny do próbujesz wdrażanie. Możesz wyszukać szablony w celu wyszukania tekstu, która jest zgodna z rozwiązania.

    ![Wybierz szablon Szybki Start](./media/resource-group-template-deploy-portal/select-quickstart-template.png)

    Wybrany szablon można wyświetlać w edytorze.

5. Po zapewnieniu wszystkie inne wartości, wybierz opcję **Utwórz** wdrożyć szablonu. 

    ![Wdrażanie szablonu](./media/resource-group-template-deploy-portal/create-custom-deploy.png)

## <a name="deploy-resources-from-a-template-saved-to-your-account"></a>Wdrażanie zasobów przy użyciu szablonu zapisany do swojego konta

Portal umożliwia zapisywanie szablonu do konta Azure i ponownie wdróż później. Aby uzyskać więcej informacji na temat pracy z tych zapisanych szablonów, [Rozpoczynanie pracy z szablonami prywatne w portalu Azure](./marketplace-consumer/mytemplates-getstarted.md).

1. Aby znaleźć szablony zapisane, wybierz pozycję **Przeglądaj** > **szablonów**.

    ![Przeglądaj szablony](./media/resource-group-template-deploy-portal/browse-templates.png)

2. Z listy szablonów zapisane do swojego konta wybierz ten, który chcesz pracować nad.

    ![Szablony zapisane](./media/resource-group-template-deploy-portal/saved-templates.png)

3. Wybierz pozycję **Rozmieść** ponownie rozmieścić tego zapisanego szablonu.

    ![Wdrażanie zapisanego szablonu](./media/resource-group-template-deploy-portal/deploy-saved-template.png)

## <a name="next-steps"></a>Następne kroki

- Wyświetlanie dzienników inspekcji, zobacz [Inspekcja operacji przy użyciu Menedżera zasobów](resource-group-audit.md).
- W rozwiązywaniu problemów dotyczących błędów wdrażania, zobacz [Rozwiązywanie problemów zasobów grupy wdrożeń Portal Azure](resource-manager-troubleshoot-deployments-portal.md).
- Aby pobrać szablon z wdrożenia lub grupa zasobów, zobacz [Eksportowanie Menedżera zasobów Azure szablonu z istniejących zasobów](resource-manager-export-template.md).





