<properties
    pageTitle="Ciągły integracji w usługach zespołu w PORÓWNANIU z używania projektów grupa zasobów Azure | Microsoft Azure"
    description="W tym artykule opisano, jak skonfigurować ciągły integracji programu Visual Studio Team Services przy użyciu projektów rozmieszczania Azure grupa zasobów w programie Visual Studio."
    services="visual-studio-online"
    documentationCenter="na"
    authors="mlearned"
    manager="erickson-doug"
    editor="" />

 <tags
    ms.service="azure-resource-manager"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="08/01/2016"
    ms.author="mlearned" />

# <a name="continuous-integration-in-visual-studio-team-services-using-azure-resource-group-deployment-projects"></a>Ciągły Integracja z usługami programu Visual Studio zespołu za pomocą projektów rozmieszczania Azure grupa zasobów

Aby wdrożyć Azure szablonu, trzeba wykonać zadania, aby przejść na poszczególnych etapach: konstrukcji, testowanie, Kopiuj Azure (nazywanych też "Tymczasowego") i wdrażanie szablonu.  Istnieją dwa różne sposoby, w tym w Visual Studio Team Services (usługi w PORÓWNANIU z zespołu). Obie metody zapewniają ten sam efekt, więc należy wybrać pozycję ten, który najlepiej pasuje do przepływu pracy.

-   Dodawanie Praca krokowa do swojej definicji kompilacji, uruchamianego skrypt programu PowerShell, który znajduje się w programie project wdrożenia Azure grupa zasobów (rozmieszczanie AzureResourceGroup.ps1). Skrypt kopiuje artefakty i wdrożeniu jej szablonu.
-   Dodawanie wielu usługach zespołu w PORÓWNANIU z kroki kompilacji, każdy z nich wykonywania zadania sceny.

W tym artykule przedstawiono sposób użycia pierwszą opcję (używaj definicji Konstruuj, aby uruchomić skrypt programu PowerShell). Jedną z zalet ta opcja jest skrypt, używany przez deweloperów w programie Visual Studio samej skrypt, który jest używany przez w PORÓWNANIU z zespołu usługi. W poniższej procedurze założono, że masz Projekt wdrożenia programu Visual Studio w PORÓWNANIU Team Services.

## <a name="copy-artifacts-to-azure"></a>Kopiowanie artefakty Azure 

Niezależnie od tego scenariusza Jeśli masz wszelkie artefakty, które są potrzebne do wdrożenia szablonu należy udzielić dostępu Azure Menedżera zasobów do nich. Tych efektów mogą zawierać pliki takie jak:

-   Zagnieżdżone szablony
-   Konfiguracja i skryptów DSC
-   Plików binarnych aplikacji

### <a name="nested-templates-and-configuration-scripts"></a>Zagnieżdżone szablony i skrypty konfiguracji
Kiedy używać szablonu dostępnego w programie Visual Studio (lub utworzona za pomocą programu Visual Studio wstawki) skrypt programu PowerShell nie tylko etapach artefaktów, również parameterizes identyfikator URI dla zasobów dla różnych wdrożeń. Skrypt, a następnie kopiuje artefaktów do kontenera bezpiecznej platformy Azure, tworzy tokenu skojarzenia zabezpieczeń do kontenera, a następnie przekazuje te informacje do wdrożenia szablonu. Zobacz [Tworzenie rozmieszczania szablonu](https://msdn.microsoft.com/library/azure/dn790564.aspx) , aby dowiedzieć się więcej na temat szablonów zagnieżdżonych.

## <a name="set-up-continuous-deployment-in-vs-team-services"></a>Konfigurowanie ciągłe rozmieszczania w PORÓWNANIU z Team Services

Aby zadzwonić do skrypt programu PowerShell w PORÓWNANIU z Team Services, musisz zaktualizować do definicji kompilacji. Krótko mówiąc kroki są: 

1.  Edytowanie definicji kompilacji.
1.  Konfigurowanie Azure autoryzacji w PORÓWNANIU z Team Services.
1.  Dodaj krok kompilacji programu PowerShell Azure odwołującego się skrypt programu PowerShell w programie project wdrożenia Azure grupa zasobów.
1.  Ustaw wartość parametru *- ArtifactsStagingDirectory* do pracy z projektem wbudowana w PORÓWNANIU z Team Services.

### <a name="detailed-walkthrough"></a>Uzyskać szczegółowy opis

Poniższe kroki przeprowadzi Cię przez kroki niezbędne do skonfigurowania wdrożenia ciągły w PORÓWNANIU z Team Services 

1.  Edytowanie definicji kompilacji w PORÓWNANIU z Team Services i Dodaj na etapie kompilacji Azure programu PowerShell. Wybierz definicję kompilacji w kategorii **Tworzenie definicji** , a następnie wybierz pozycję **Edytuj** łącze.

    ![][0]

1.  Dodaj nowy krok kompilacji **Azure programu PowerShell** do definicji kompilacji, a następnie wybierz pozycję **Dodaj tworzenie krok...** przycisk.

    ![][1]

1.  Wybierz kategorię **zadania rozmieszczania** , wybierz zadanie **Azure programu PowerShell** , a następnie wybierz jego przycisk **Dodaj** .

    ![][2]

1.  Wybierz krok kompilacji **Programu PowerShell Azure** , a następnie wypełnij wartości.

    1.  Jeśli masz już punktu końcowego usługi Azure dodane do usług zespołu w PORÓWNANIU z, wybierz subskrypcję w **Subskrypcji Azure** listy rozwijanej, a następnie przejdź do następnej sekcji. 

        Jeśli nie masz punktu końcowego usługi Azure w PORÓWNANIU z Team Services, musisz dodać jeden. Niniejszej podsekcji przejście przez proces. Jeśli Twoje konto Azure używa konta Microsoft (na przykład Hotmail), musisz wykonać następujące czynności, aby uzyskać uwierzytelniania wystawcy usługi.

    1.  Wybierz łącze **Zarządzaj** obok **Subskrypcji Azure** rozwijane pole listy.

        ![][3]

    1. Wybierz pole listy rozwijanej **Nowy punkt końcowy usługi** **Azure** .

        ![][4]

    1.  W oknie dialogowym **Dodawanie Azure subskrypcji** wybierz opcję **Wystawcy usługi** .

        ![][5]

    1.  Dodaj informacje o subskrypcji Azure do okna dialogowego **Dodawanie Azure subskrypcji** . Konieczne będzie podanie następujących elementów:
        -   Identyfikator subskrypcji
        -   Nazwy subskrypcji.
        -   Identyfikator wystawcy usługi
        -   Klucz wystawcy usługi
        -   Identyfikator dzierżawy

    1.  Dodawanie nazwy wybranych przez użytkownika w polu Nazwa **subskrypcji** . Wartość ta pojawi się w dalszej części **Subskrypcji Azure** listy rozwijanej w PORÓWNANIU z Team Services. 

    1.  Jeśli nie znasz swojego Identyfikatora subskrypcji Azure umożliwia jedno z następujących poleceń wykonywanie.
        
        W przypadku skryptów programu PowerShell Użyj:

        `Get-AzureRmSubscription`

        W przypadku polecenie Azure Użyj:

        `azure account show`
    

    1.  Aby uzyskać identyfikator wystawcy usługi, klucz wystawcy usługi i ID dzierżawy, wykonaj procedurę opisaną w [aplikacji tworzenie usługi Active Directory i wystawcy usługi za pomocą portalu](resource-group-create-service-principal-portal.md) i [uwierzytelniania wystawcy usługi przy użyciu Menedżera zasobów Azure](resource-group-authenticate-service-principal.md).

    1.  Dodaj identyfikator wystawcy usługi, klucz wystawcy usługi i identyfikator dzierżawy wartości do okna dialogowego **Dodawanie Azure subskrypcji** , a następnie kliknij przycisk **OK** .

        Teraz masz prawidłowy wystawcy usługi, aby uruchomić skrypt programu PowerShell Azure za pomocą.

1.  Edytowanie definicji kompilacji i wybierz pozycję krok kompilacji **Azure programu PowerShell** . Wybierz subskrypcję, w polu listy rozwijanym **Azure subskrypcji** . (Jeśli nie widać subskrypcji, wybierz przycisk **Odśwież** dalej łącze **Zarządzaj** ). 

    ![][8]

1.  Wprowadź ścieżkę do skrypt programu PowerShell AzureResourceGroup.ps1 wdrażanie. Aby to zrobić, wybierz przycisk wielokropka (...) obok pola **Ścieżka skryptu** , odszukaj skrypt programu PowerShell AzureResourceGroup.ps1 rozmieszczanie w folderze **skryptów** projektu, zaznacz go, a następnie wybierz przycisk **OK** . 

    ![][9]

1. Po zaznaczeniu skrypt, należy zaktualizować ścieżkę do skryptu tak, aby jest uruchamiana z Build.StagingDirectory (sam katalog ustawiany *ArtifactsLocation* ). Można to zrobić, dodając "$(Build.StagingDirectory)/"na początku ścieżki skryptów.

    ![][10]

1.  W oknie dialogowym **Argumenty skryptu** wprowadź następujące parametry (w jednym wierszu). Po uruchomieniu skryptu w programie Visual Studio, można zobaczyć, jak używane są w PORÓWNANIU z parametrów w oknie **dane wyjściowe** . Można to jako punkt wyjścia do ustawiania wartości parametrów w kroku do kompilacji.

  	| Parametr | Opis|
  	|---|---|
  	| -ResourceGroupLocation           | Wartość geo lokalizacji, którym grupa zasobów znajduje się, takich jak **eastus** lub **"US Wschodniej"**. (Dodaj apostrofy, jeśli istnieje spacji w nazwie). Aby uzyskać więcej informacji, zobacz [Obszary Azure](https://azure.microsoft.com/en-us/regions/) .|                                                                                                                                                                                                                              |
  	| -ResourceGroupName               | Nazwa grupy zasobów, używane do wdrażania.|                                                                                                                                                                                                                                                                                                                                                                                                                |
  	| -UploadArtifacts                 | Ten parametr, jeśli są dostępne, określa, że artefakty muszą do przekazania Azure z systemu lokalnego. Należy ustawić ten przełącznik, jeśli wdrożenia szablonu wymaga dodatkowych artefaktów, które mają być etapu za pomocą skryptu programu PowerShell (na przykład skrypty konfiguracji lub zagnieżdżone szablony).                                                                                                                                                                 |
  	| -StorageAccountName              | Nazwę konta magazynu umożliwia artefakty etapie tego wdrożenia. Ten parametr jest wymagany tylko wtedy, gdy kopiowaniu artefakty Azure. To konto miejsca do magazynowania nie jest automatycznie tworzona przez wdrażanie, musi już istnieć.|                                                                                                                                                                                                                     |
  	| -StorageAccountResourceGroupName | Nazwa grupy zasobów skojarzone z kontem miejsca do magazynowania. Ten parametr jest wymagany tylko wtedy, gdy kopiowaniu artefakty Azure.|                                                                                                                                                                                                                                                                                                                               |
  	| -TemplateFile                    | Ścieżka do pliku szablonu w programie project wdrożenia Azure grupa zasobów. Aby zwiększyć elastyczność, należy użyć ścieżki tego parametru jest lokalizację skrypt programu PowerShell zamiast ścieżką bezwzględną.|
  	| -TemplateParametersFile          | Ścieżka do pliku parametrów w programie project wdrożenia Azure grupa zasobów. Aby zwiększyć elastyczność, należy użyć ścieżki tego parametru jest lokalizację skrypt programu PowerShell zamiast ścieżką bezwzględną.|
  	| -ArtifactStagingDirectory        | Ten parametr umożliwia PowerShell skrypt znany jest folder, z której mają zostać skopiowane plików binarnych projektu. Ta wartość zastępuje wartość domyślna używana przez skrypt programu PowerShell. Dla a Team Services, ustaw wartość: $(Build.StagingDirectory) - ArtifactStagingDirectory                                                                                                                                                                                              |

    Oto przykład argumenty skryptu (linia niepoprawna w celu zwiększenia czytelności):

    ``` 
    -ResourceGroupName 'MyGroup' -ResourceGroupLocation 'eastus' -TemplateFile '..\templates\azuredeploy.json' 
    -TemplateParametersFile '..\templates\azuredeploy.parameters.json' -UploadArtifacts -StorageAccountName 'mystorageacct' 
    –StorageAccountResourceGroupName 'Default-Storage-EastUS' -ArtifactStagingDirectory '$(Build.StagingDirectory)' 
    ```

    Po zakończeniu, pole **Argumenty skryptu** powinien wyglądać w następujący sposób.

    ![][11]

1.  Po dodaniu, że wszystkie wymagane elementy, aby Azure programu PowerShell tworzenie kroku, wybierz pozycję **kolejki** tworzenie przycisk, aby utworzyć projekt. Na ekranie **Tworzenie** zostaną wyświetlone dane wyjściowe skrypt programu PowerShell.

## <a name="next-steps"></a>Następne kroki

Przeczytaj [Omówienie Menedżera zasobów Azure](azure-resource-manager/resource-group-overview.md) , aby dowiedzieć się więcej o Azure Menedżera zasobów i grup zasobów Azure.


[0]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough1.png
[1]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough2.png
[2]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough3.png
[3]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough4.png
[4]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough5.png
[5]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough6.png
[8]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough9.png
[9]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough10.png
[10]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough11b.png
[11]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough12.png
