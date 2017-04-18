<properties
   pageTitle="Projekty Visual Studio grupa zasobów Azure | Microsoft Azure"
   description="Visual Studio umożliwia tworzenie projektu grupa zasobów Azure i wdrażanie zasobów Azure."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn" />
<tags
   ms.service="azure-resource-manager"
   ms.devlang="multiple"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/20/2016"
   ms.author="tomfitz" />

# <a name="creating-and-deploying-azure-resource-groups-through-visual-studio"></a>Tworzenie i wdrażanie grupy zasobów Azure za pomocą programu Visual Studio

Z programu Visual Studio i [Azure SDK](https://azure.microsoft.com/downloads/)możesz utworzyć projekt, który wdraża infrastruktury i kod usługi Azure. Na przykład można zdefiniować hosta sieci web, witryny sieci web i bazy danych dla aplikacji i wdrażanie infrastruktury wraz z kodem. Lub zdefiniować maszyn wirtualnych, wirtualną sieć i konta miejsca do magazynowania i wdrażanie infrastruktury wraz z skrypt, który jest wykonywane na komputerze wirtualnych. **Grupa zasobów Azure** projektu wdrażania umożliwia wdrażanie wszystkich wymaganych zasobów w jednym, powtarzalnych operacji. Aby uzyskać więcej informacji na temat wdrażania i zarządzanie zasobami zobacz [Omówienie Menedżera zasobów Azure](azure-resource-manager/resource-group-overview.md).

Projekty Azure grupa zasobów zawierają szablonów Azure JSON Menedżera zasobów, które zasoby, które mają być rozmieszczone Azure zdefiniować. Aby uzyskać informacje o elementach szablonu Menedżera zasobów, zobacz [Szablony do tworzenia Azure Menedżera zasobów](resource-group-authoring-templates.md). Visual Studio umożliwia edycję tych szablonów i udostępnia narzędzia, które uprościć pracy z szablonami.

W tym temacie należy wdrożyć aplikację sieci web i baza danych SQL. Jednak wykonać czynności prawie tak samo dla wszystkich typów zasobów. Można jako łatwo rozmieścić maszyny wirtualnej i jego zasoby pokrewne. Programu Visual Studio zawiera wiele różnych starter szablonów do wdrażania typowych scenariuszy.

Ten artykuł zawiera Visual Studio 2015 aktualizacji 2 i Microsoft Azure SDK dla .NET 2.9. Jeśli używasz programu Visual Studio 2013 z Azure SDK 2.9 środowiska jest głównie taka sama. Przy użyciu wersji SDK Azure z 2.6 lub później; Jednak środowiska interfejsu użytkownika mogą być inne niż przedstawione w tym artykule interfejsu użytkownika. Zaleca się zainstalować najnowszą wersję pakietu [Azure SDK](https://azure.microsoft.com/downloads/) przed rozpoczęciem czynności. 

## <a name="create-azure-resource-group-project"></a>Tworzenie grupy zasobów Azure projektu

W tej procedurze utworzysz projektu grupa zasobów Azure za pomocą szablonu **aplikacji Web app + SQL** .

1. W programie Visual Studio, wybierz pozycję **plik**, **Nowy projekt**, wybierz pozycję **C#** lub **Visual Basic**. Następnie wybierz pozycję **chmurze**, a następnie wybierz projekt **Azure grupa zasobów** .

    ![Chmura wdrożenia projektu](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/create-project.png)

1. Wybierz szablon, który chcesz wdrożyć do Menedżera zasobów Azure. Powiadomienia są wielu różnych opcji, w zależności od typu projektu, którą chcesz wdrożyć. Dla tego tematu wybierz szablon **aplikacji Web app + SQL** .

    ![Wybieranie szablonu](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/select-project.png)

    Szablon, który możesz wybrać jest po prostu punktu początkowego; można dodawać i usuwać zasobów w celu spełnienia rozwiązania.

    >[AZURE.NOTE] Program Visual Studio pobiera listę dostępnych szablonów w trybie online. Listy mogą ulec zmianie.

    Programu Visual Studio umożliwia utworzenie projektu wdrożenia grupa zasobów dla aplikacji sieci web i baza danych SQL.

1. Aby wyświetlić został utworzony, rozwiń węzły w programie project wdrożenia.

    ![Pokazywanie węzły](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-items.png)

    Ponieważ Wybraliśmy aplikacji Web app + szablonu SQL w tym przykładzie zostanie wyświetlone następujące pliki: 

  	|Nazwa pliku|Opis|
  	|---|---|
  	|Wdrażanie AzureResourceGroup.ps1|Skrypt programu PowerShell, która wywołuje polecenia programu PowerShell do wdrożenia do Menedżera zasobów Azure.<br />**Uwaga** Program Visual Studio używa tego skryptu programu PowerShell do wdrożenia szablonu. Wszelkie zmiany wprowadzone do tego skryptu wpływa na wdrożenie programu Visual Studio, dlatego należy zachować ostrożność.|
  	|WebSiteSQLDatabase.json|Szablon Menedżera zasobów, który definiuje infrastruktury, które mają Wdroż Azure i parametry, które umożliwiają podczas wdrażania. Definiuje zależności między zasoby, aby Menedżer zasobów wdraża zasobów w odpowiedniej kolejności.|
  	|WebSiteSQLDatabase.parameters.json|Plik parametry, który zawiera wartości wymagane w szablonie. Możesz przekazać wartości parametrów dostosowywania każdego rozmieszczenia.|

    Wszystkich projektów wdrożenia grupa zasobów zawierają te pliki podstawowe. Innych projektów może zawierać dodatkowe pliki do innych funkcji.

## <a name="customize-the-resource-manager-template"></a>Dostosowywanie szablonu Menedżera zasobów

Wdrożenia projektu można dostosować, zmieniając szablony JSON, które opisują zasoby, które chcesz wdrożyć. JSON oznacza JavaScript Object Notation i to format danych seryjnych jest łatwe do pracy z. Pliki JSON za pomocą schematu odwołanie w górnej części każdego pliku. Jeśli chcesz opis schematu, możesz pobrać i analizować je. Schemat określa, jakie elementy są prawidłowe, typy i formaty pól, możliwe wartości wyliczenia wartości i tak dalej. Aby uzyskać informacje o elementach szablonu Menedżera zasobów, zobacz [Szablony do tworzenia Azure Menedżera zasobów](resource-group-authoring-templates.md).

Aby pracować nad szablonu, otwórz **WebSiteSQLDatabase.json**.

Edytor Visual Studio zawiera narzędzia do pomocne w przypadku edytowanie szablonu Menedżera zasobów. Okno **Konspektu JSON** można łatwo zobaczyć elementy zdefiniowane w szablonie.

![Wyświetlanie konspektu JSON](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-json-outline.png)

Wybierając jeden z elementów w konspekcie przejście do tej części szablonu i wyróżnienie odpowiedniego JSON.

![Nawigowanie JSON](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/navigate-json.png)

Zasób można dodać, wybierając przycisk **Dodaj zasobu** w górnej części okna konspektu JSON lub klikając prawym przyciskiem myszy **zasobów** i wybierając **Dodawanie nowego zasobu**.

![Dodawanie zasobu](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-resource.png)

Ten samouczek zaznacz **Konto miejsca do magazynowania** , a następnie nadaj plikowi nazwę. Podaj nazwę jest nie więcej niż 11 znaków, która zawiera tylko małe litery i cyfry.

![Dodawanie miejsca do magazynowania](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-storage.png)

Należy zauważyć, że nie tylko został dodany zasobu, ale również parametr typ konta miejsca do magazynowania i zmiennej nazwę konta magazynu.

![Wyświetlanie konspektu](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-new-items.png)

Parametr **storageType** jest wstępnie zdefiniowanych z dozwolonych typów i typ domyślny. Możesz pozostawić te wartości lub edytowanie ich rozwiązania. Jeśli nie chcesz każdemu wdrażanie do konta miejsca do magazynowania **Premium_LRS** przy użyciu tego szablonu, należy je usunąć z dozwolonych typów. 

    "storageType": {
      "type": "string",
      "defaultValue": "Standard_LRS",
      "allowedValues": [
        "Standard_LRS",
        "Standard_ZRS",
        "Standard_GRS",
        "Standard_RAGRS"
      ]
    }

Program Visual Studio umożliwia intellisense, aby lepiej zrozumieć, jakie właściwości są dostępne podczas edytowania szablonu. Na przykład aby edytować właściwości aplikacji usługi planu, przejdź do zasobu **HostingPlan** i Dodaj wartość **Właściwości**. Zwróć uwagę, tym intellisense zawiera dostępne wartości oraz opis tej wartości.

![Pokazywanie technologii intellisense](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-intellisense.png)

**NumberOfWorkers** można ustawić na wartość 1.

    "properties": {
      "name": "[parameters('hostingPlanName')]",
      "numberOfWorkers": 1
    }

## <a name="deploy-the-resource-group-project-to-azure"></a>Wdrażanie projektu grupa zasobów Azure

Teraz możesz przystąpić do wdrożenia projektu. Podczas wdrażania projektu Azure grupa zasobów, należy wdrożyć go do grupy Azure zasobów. Grupa zasobów jest logiczna grupa zasobów, które współużytkują typowe cyklu życia.

1. W menu skrótów węzła projektu wdrożenia, wybierz pozycję **Deploy** > **Nowego wdrożenia**.

    ![Wdrażanie wdrożenia nowy element menu](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/deploy.png)

    Zostanie wyświetlone okno dialogowe **Rozmieszczanie do grupy zasobów** .

    ![Wdrażanie grupa zasobów, okno dialogowe](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-deployment.png)

1. W polu listy rozwijanej **Grupa zasobów** wybierz istniejącej grupy zasobów lub Utwórz nową. Aby utworzyć grupę zasobów, Otwórz pole listy rozwijanej **Grupa zasobów** i wybierz polecenie **Utwórz nowy**.

    ![Wdrażanie grupa zasobów, okno dialogowe](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/create-new-group.png)

    Zostanie wyświetlone okno dialogowe **Tworzenie grupy zasobów** . Nadaj grupie nazwę i lokalizację, a następnie wybierz przycisk **Utwórz** .

    ![Grupa zasobów, okno dialogowe Tworzenie](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/create-resource-group.png)
   
1. Edytowanie parametrów wdrożenia, wybierając przycisk **Edytuj parametry** .

    ![Obraz przycisku parametrów](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/edit-parameters.png)

1. Podaj wartości parametrów puste i wybierz przycisk **Zapisz** . Puste parametry są **hostingPlanName**, **administratorLogin**, **administratorLoginPassword**i **databaseName**.

    **hostingPlanName** nazwa [plan usług aplikacji](./app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) do tworzenia. 
    
    **administratorLogin** Określa nazwę użytkownika dla administratora programu SQL Server. Nie należy używać typowych nazwy administratora, jak **sa** lub **Administrator**. 
    
    **AdministratorLoginPassword** Określa hasło administratora programu SQL Server. Opcja **Zapisz hasła jako zwykły tekst w pliku parametrów** nie jest bezpieczna; w związku z tym nie należy zaznaczać tej opcji. Ponieważ hasło nie zostanie zapisana jako zwykły tekst, będzie konieczne hasło ponownie podczas wdrażania. 
    
    **databaseName** Nazwa bazy danych utworzyć. 

    ![Okno dialogowe Parametry edytowanie](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/provide-parameters.png)
    
1. Wybierz przycisk **Deploy** wdrożyć projektu Azure. Konsoli programu PowerShell zostanie otwarty poza wystąpienie programu Visual Studio. Wprowadź hasło administratora programu SQL Server w konsoli programu PowerShell po wyświetleniu monitu. **Konsoli programu PowerShell może być ukryty za inne elementy lub zminimalizowane na pasku zadań.** Poszukaj konsoli i zaznacz ją o podanie hasła.

    >[AZURE.NOTE] Programu Visual Studio może wymagać instalowanie polecenia cmdlet programu PowerShell Azure. Potrzebujesz cmdlet Azure programu PowerShell w celu pomyślnego wdrożenia grup zasobów. Jeśli zostanie wyświetlony monit, zainstaluj je.
    
1. Wdrażanie może potrwać kilka minut. W oknie **dane wyjściowe** zobaczysz stan wdrożenia. Po zakończeniu rozmieszczania ostatnią wiadomość wskazuje pomyślnego wdrożenia z podobnej do:

        ... 
        18:00:58 - Successfully deployed template 'c:\users\user\documents\visual studio 2015\projects\azureresourcegroup1\azureresourcegroup1\templates\websitesqldatabase.json' to resource group 'DemoSiteGroup'.


1. W przeglądarce otwórz [Azure portal](https://portal.azure.com/) , a następnie zaloguj się do swojego konta. Aby wyświetlić grupa zasobów, wybierz **grup zasobów** i rozmieszczany do grupy zasobów.

    ![Wybierz grupę](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/select-group.png)

1. Zobacz wszystkie wdrożonym zasoby. Zauważ, że nazwę konta magazynu jest dokładnie określony podczas dodawania tego zasobu. Konto miejsca do magazynowania musi być unikatowa. Szablon automatycznie dodaje ciąg znaków do nazwy, pod warunkiem, aby zapewnić unikatową nazwę. 

    ![Pokaż zasoby](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-deployed-resources.png)

1. Wprowadź zmiany i chcesz ponownie rozmieścić projektu, jeśli istniejącej grupy zasobów z menu skrótów projekt grupy Azure zasobów. W menu skrótów wybierz polecenie **Rozmieść**, a następnie wybierz grupę zasobów, która została wdrożona.

    ![Grupa zasobów Azure wdrożony](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/redeploy.png)

## <a name="deploy-code-with-your-infrastructure"></a>Wdrażanie kodu z infrastruktury

Na tym etapie wdrożono infrastrukturę dla aplikacji, ale nie rzeczywisty kod używany z projektem. W tym temacie przedstawiono sposób wdrożyć aplikację sieci web i tabele bazy danych SQL podczas wdrażania. W przypadku rozmieszczania maszyny wirtualnej zamiast aplikacji sieci web chcesz uruchomić kodu na komputerze w ramach wdrożenia. Proces wdrażania kodu dla aplikacji sieci web lub konfigurowania maszyny wirtualnej jest prawie tak samo.

1. Dodawanie projektu do rozwiązania programu Visual Studio. Kliknij prawym przyciskiem myszy rozwiązanie, a następnie wybierz pozycję **Dodaj** > **Nowego projektu**.

    ![Dodaj projekt](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-project.png)

1. Dodawanie **aplikacji sieci Web ASP.NET**. 

    ![Dodawanie aplikacji sieci web](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-app.png)
    
1. Zaznacz **MVC** , a następnie wyczyść pole hosta **w chmurze** , ponieważ projekt grupa zasobów wykonanie tego zadania.

    ![Wybierz pozycję MVC](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/select-mvc.png)
    
1. Gdy Visual Studio utworzy aplikacji sieci web, pojawi się oba projekty w rozwiązaniu.

    ![Pokaż projekty](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-projects.png)

1. Teraz musisz upewnij się, że projektu grupa zasobów zna nowego projektu. Wróć do projektu grupa zasobów (AzureResourceGroup1). Kliknij prawym przyciskiem myszy **odwołania** , a następnie wybierz pozycję **Dodaj odwołanie**.

    ![Dodawanie odwołania](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-new-reference.png)

1. Wybierz projekt aplikacji sieci web, utworzone przez Ciebie.

    ![Dodawanie odwołania](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-reference.png)
    
    Dodając odwołanie, łącze do projektu grupy z zasobami projektu aplikacji sieci web, a automatyczne ustawianie trzy kluczowe właściwości. Zobacz tych właściwości w oknie **dialogowym właściwości** dla odwołania.

      ![Zobacz informacje dotyczące](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/see-reference.png)
    
    Dostępne są następujące właściwości:

    - **Dodatkowe właściwości** zawiera tymczasowej lokalizacji, w której zostanie przypisany do magazynowania Azure pakietu wdrażania sieci web. Uwaga folderu (ExampleApp) i pliku (package.zip). Te wartości zapewni jako parametrów podczas wdrażania aplikacji. 
    - **Ścieżka pliku zawiera** zawiera ścieżkę, w której jest tworzona pakietu. **Dołączanie elementów docelowych** zawiera polecenia, które wykonuje wdrożenia. 
    - Tworzenie wartości domyślnej **; Pakiet** umożliwia wdrożenie do tworzenia i tworzenie pakietu wdrażania sieci web (package.zip).  
    
    Niepotrzebne profilu publikowania podczas rozmieszczania pobiera niezbędne informacje z właściwości, aby utworzyć pakiet.
      
1. Dodawanie zasobu do szablonu.

    ![Dodawanie zasobu](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-resource-2.png)

1. Zaznacz **Wdrażanie sieci Web dla aplikacji sieci Web**. 

    ![Dodawanie web wdrażanie](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-web-deploy.png)
    
1. Ponownie wdróż projektu grupa zasobów do grupy zasobów. Tym razem istnieje kilka nowych parametrów. Nie musisz podać wartości dla **_artifactsLocation** lub **_artifactsLocationSasToken** , ponieważ program Visual Studio umożliwia automatyczne generowanie te wartości. Możesz jednak ustawić folder i nazwę pliku ścieżkę, która zawiera pakietu wdrażania (pokazane jako **ExampleAppPackageFolder** i **ExampleAppPackageFileName** na poniższej ilustracji). Zawierają wartości, który został wyświetlony wcześniej we właściwościach odwołanie (**ExampleApp** i **package.zip**).

    ![Dodawanie web wdrażanie](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/set-new-parameters.png)
    
    **Konta miejsca do magazynowania artefaktu**wybierz ten, który został wdrożony w tej grupie zasobów.
    
1. Po zakończeniu rozmieszczania wybierz aplikacji sieci web w portalu. Zaznacz adres URL, aby przejść do witryny.

    ![Przeglądanie witryny](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/browse-site.png)

1. Zwróć uwagę, zostały pomyślnie wdrożone domyślnej aplikacji ASP.NET.

    ![Pokazywanie wdrożonej aplikacji](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-deployed-app.png)

## <a name="next-steps"></a>Następne kroki

- Aby uzyskać informacje o zarządzanie zasobami za pośrednictwem portalu, zobacz [za pomocą portalu Azure do zarządzania zasobami Azure](./azure-portal/resource-group-portal.md).
- Aby dowiedzieć się więcej na temat szablonów, zobacz [Szablony do tworzenia Azure Menedżera zasobów](resource-group-authoring-templates.md).
