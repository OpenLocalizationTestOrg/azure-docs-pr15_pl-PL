

W tym artykule pokazano, jak wdrożyć serwer Azure maszyn wirtualnych Skala Ustaw za pomocą programu Visual Studio zasobu grupy.


[Zestawy skali maszyn wirtualnych Azure](https://azure.microsoft.com/blog/azure-vm-scale-sets-public-preview/) są zasób obliczyć Azure wdrażać i zarządzanie zbiorem podobne maszyn wirtualnych z opcjami łatwo zintegrowane automatyczne skalowanie i równoważenia obciążenia. Można dodawać i wdrażanie zestawów skali maszyn wirtualnych przy użyciu [szablonów Azure Menedżera zasobów (ARM)](https://github.com/Azure/azure-quickstart-templates). Szablony ARM mogą być rozmieszczone za pomocą usługi REST polecenie Azure, programu PowerShell, a także bezpośrednio z programu Visual Studio. Program Visual Studio zawiera zestaw przykładowe szablony, które mogą być rozmieszczone w ramach projektu wdrażania grupa zasobów Azure.

Grupa zasobów Azure wdrożeń służą do grupowania i publikowanie zestawu Azure zasoby pokrewne w operacji pojedynczy wdrożenia. Więcej informacji o nich tutaj: [Tworzenie i wdrażanie grupy zasobów Azure za pomocą programu Visual Studio](../vs-azure-tools-resource-groups-deployment-projects-create-deploy/).

## <a name="pre-requisites"></a>Wymagania wstępne

Aby rozpocząć wdrażanie zestawów skali maszyn wirtualnych w programie Visual Studio są potrzebne następujące elementy:

- Visual Studio 2013 lub 2015 r.
- Azure SDK 2.7 lub liczbę 2,8

Uwaga: Te instrukcje założono, że używasz programu Visual Studio 2015 z [2,8 SDK Azure](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/).

## <a name="creating-a-project"></a>Tworzenie projektu

1. Tworzenie nowego projektu w Visual Studio 2015, wybierając pozycję **pliku | Nowy | Projekt**.

    ![Nowy plik][file_new]

2. W obszarze **Visual C# | Chmura**, wybierz pozycję **Menedżer zasobów Azure** do tworzenia projektu do wdrażania szablonu ARM.

    ![Tworzenie projektu][create_project]

3.  Z listy szablonów wybierz Linux albo szablon Ustawianie skali maszyn wirtualnych systemu Windows.

    ![Wybierz szablon][select_Template]

4. Po utworzeniu projektu pojawi się skrypty wdrożenia programu PowerShell, szablonu Menedżera zasobów Azure i pliku parametrów dla zestawu skali maszyn wirtualnych.

    ![Eksplorator rozwiązań][solution_explorer]

## <a name="customize-your-project"></a>Dostosowywanie projektu

Teraz można edytować szablon, aby go dostosować do potrzeb aplikacji, takich jak dodawanie właściwości rozszerzenia maszyn wirtualnych lub edytowanie reguły równoważenia obciążenia. Domyślnie szablony zestawu skali maszyn wirtualnych są skonfigurowane do wdrożenia na rozszerzenie AzureDiagnostics, co pozwala na łatwe dodawanie reguły autoscale. Rozmieszcza także równoważenia obciążenia przy użyciu publicznego adresu IP, skonfigurowano reguły przychodzące translatora adresów Sieciowych pozwalające nawiązać wystąpienia maszyn wirtualnych SSH (Linux) lub RDP (Windows) — zakres portów fronton zaczyna się od 50 000 co oznacza w przypadku Linux, jeśli użytkownik SSH do portu 50000 publiczny adres IP (lub nazwa domeny) zostanie skierowana do portu 22 pierwszego maszyn wirtualnych w zestawie Skala. Nawiązywanie połączenia z portem 50001 będą kierowane do portu 22 drugiego maszyn wirtualnych i tak dalej.

 Dobrym sposobem na edytowanie szablonów z programem Visual Studio jest organizowanie parametry, zmienne i zasobów za pomocą konspektu JSON. Opis schematu programu Visual Studio, można wskazać błędy w szablonie przed wdrożeniem tego.

![Eksplorator JSON][json_explorer]

## <a name="deploy-the-project"></a>Wdrażanie projektu

6. Wdrażanie szablonu ARM Azure, aby utworzyć zasób zestaw skali maszyn wirtualnych. Kliknij prawym przyciskiem myszy węzeł projektu, wybierz pozycję **Rozmieszczanie | Wdrożenie nowych**.

    ![Wdrażanie szablonu][5deploy_Template]

7. Wybierz subskrypcję, w oknie dialogowym "Wdrażanie do grupy zasobów".

    ![Wdrażanie szablonu][6deploy_Template]

8. W tym miejscu można także tworzyć nowej grupy zasobów Azure wdrożenia szablon.

    ![Nowa grupa zasobów][new_resource]

9. Następnie wybierz przycisk **Edytuj parametry** , aby wprowadzić parametry, które zostaną przekazane do szablonu, niektóre wartości, takie jak nazwa użytkownika i hasła dla systemu operacyjnego są wymagane do utworzenia wdrożenia.

    ![Edytowanie parametrów][edit_parameters]

10. Kliknij przycisk **Deploy**. W oknie **dane wyjściowe** są wyświetlane postęp rozmieszczania. Należy zauważyć, że akcja jest wykonanie skryptu **AzureResourceGroup.ps1 wdrażanie** .

    ![Okno dane wyjściowe][output_window]

## <a name="exploring-your-vm-scale-set"></a>Poznawanie zestawu skali maszyn wirtualnych

Po wykonaniu wdrażanie możesz wyświetlać zestawu skali maszyn wirtualnych w programie Visual Studio **Chmury Eksploratora** (odświeżanie listy). Eksplorator chmury umożliwia zarządzanie Azure zasobów w programie Visual Studio podczas tworzenia aplikacji. Ustawianie skali maszyn wirtualnych można także wyświetlić w Azure Portal i Azure zasobów Eksploratora.

![Eksplorator chmury][cloud_explorer]

 Najlepszym sposobem wizualnie zarządzanie infrastrukturą Azure za pomocą przeglądarki sieci web podczas Eksploratora zasobów Azure umożliwia łatwe do Eksploratora i debugowanie Azure zasobów, dając okno "Wyświetl wystąpienie" i widoczną polecenia programu PowerShell dla zasobów, którą chcesz odnaleźć w znajdują się w portalu. Zestawy skali maszyn wirtualnych w czasie podglądu, Eksploratora zasobów zostanie wyświetlona najwięcej szczegółów dla swojego zestawów skali maszyn wirtualnych.

## <a name="next-steps"></a>Następne kroki

Po pomyślnym zostały rozmieszczeniu zestawy skali maszyn wirtualnych za pośrednictwem programu Visual Studio można dostosować projektu do własnych potrzeb aplikacji. Na przykład ustawienie autoscale Dodawanie zasobu wniosków, dodając infrastruktury do szablonu, takich jak autonomicznego maszyny wirtualne lub wdrażania aplikacji przy użyciu rozszerzenia skryptu niestandardowego. Dobrym źródłem przykładowe szablony można znaleźć w repozytorium GitHub [Azure Szybki Start szablony](https://github.com/Azure/azure-quickstart-templates) (wyszukiwanie "vmss").

[file_new]: ./media/virtual-machines-common-scale-sets-visual-studio/1-FileNew.png
[create_project]: ./media/virtual-machines-common-scale-sets-visual-studio/2-CreateProject.png
[select_Template]: ./media/virtual-machines-common-scale-sets-visual-studio/3b-SelectTemplateLin.png
[solution_explorer]: ./media/virtual-machines-common-scale-sets-visual-studio/4-SolutionExplorer.png
[json_explorer]: ./media/virtual-machines-common-scale-sets-visual-studio/10-JsonExplorer.png
[5deploy_Template]: ./media/virtual-machines-common-scale-sets-visual-studio/5-DeployTemplate.png
[6deploy_Template]: ./media/virtual-machines-common-scale-sets-visual-studio/6-DeployTemplate.png
[new_resource]: ./media/virtual-machines-common-scale-sets-visual-studio/7-NewResourceGroup.png
[edit_parameters]: ./media/virtual-machines-common-scale-sets-visual-studio/8-EditParameter.png
[output_window]: ./media/virtual-machines-common-scale-sets-visual-studio/9-Output.png
[cloud_explorer]: ./media/virtual-machines-common-scale-sets-visual-studio/12-CloudExplorer.png