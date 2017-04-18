<properties
    pageTitle="Inicjowanie obsługi i wdrażanie microservices właściwie platformy Azure"
    description="Dowiedz się, jak wdrożyć aplikację składający się z microservices w usłudze Azure aplikacji jako całość i przewidywalne sposób korzystania z szablonów grupy zasobów JSON i skryptów programu PowerShell."
    services="app-service"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor="jimbe"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="01/06/2016"
    ms.author="cephalin"/>


# <a name="provision-and-deploy-microservices-predictably-in-azure"></a>Inicjowanie obsługi i wdrażanie microservices właściwie platformy Azure #

Ten samouczek przedstawiono sposób obsługi administracyjnej i wdrożyć aplikację składający się z [microservices](https://en.wikipedia.org/wiki/Microservices) w [Usłudze Azure aplikacji](/services/app-service/) jako całość i przewidywalne sposób korzystania z szablonów grupy zasobów JSON i skryptów programu PowerShell. 

Podczas inicjowania obsługi administracyjnej i wdrażania aplikacji wysokiej klasy, które składają się z wysoce odłączona microservices, powtarzalności i przewidywalności są podstawą sukcesu. [Usługa Azure aplikacji](/services/app-service/) pozwala na tworzenie microservices zawierające aplikacji web apps, aplikacje dla urządzeń przenośnych, aplikacje interfejsu API i logiki aplikacji. [Menedżer zasobów Azure](../azure-resource-manager/resource-group-overview.md) umożliwia zarządzanie wszystkich microservices jako jednostki, razem z zależności zasobów, takich jak bazy danych i źródła ustawień sterujących. Teraz można także wdrożyć takich aplikacji przy użyciu szablonów JSON i prostych skryptów programu PowerShell. 

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="what-you-will-do"></a>Co będzie zrobić ##

W samouczku będzie wdrożyć aplikację, która zawiera:

-   Dwa sieci web apps (to znaczy dwóch microservices)
-   Baza danych SQL wewnętrznej bazie danych
-   Ustawienia aplikacji, parametry połączenia i kontrolki źródła
-   Wnioski aplikacji, alertów, ustawienia autoscaling

## <a name="tools-you-will-use"></a>Narzędzia, które będą używane ##

W tym samouczku będzie użyć następujących narzędzi. Ponieważ nie jest pełna dyskusji w narzędzia, nacisnę pokazywać tego scenariusza końcu do końca i tylko umożliwiają krótkie wprowadzenie do każdego, i gdzie można znaleźć więcej informacji na temat go. 

### <a name="azure-resource-manager-templates-json"></a>Azure szablony Menedżera zasobów (JSON) ###
 
Każdorazowo tworzysz aplikację sieci web w usłudze Azure aplikacji, na przykład Menedżer zasobów Azure używa JSON szablonu do utworzenia grupy cały zasób z zasobami składnika. Szablon złożonych z [Usługi Azure Marketplace](/marketplace) , takich jak aplikacja [Skalowalna WordPress](/marketplace/partners/wordpress/scalablewordpress/) mogą zawierać bazy danych MySQL, konta miejsca do magazynowania, plan usług aplikacji, aplikacji sieci web, reguły alertów, ustawień aplikacji, ustawienia autoscale i więcej, a wszystkie te szablony są dla Ciebie dostępne przy użyciu programu PowerShell. Aby uzyskać informacje na temat Pobieranie i używanie tych szablonów zobacz [Używanie Azure przy użyciu Menedżera zasobów Azure](../powershell-azure-resource-manager.md).

Aby uzyskać więcej informacji dotyczących szablonów programu Menedżer zasobów Azure zobacz [Do tworzenia szablony Menedżera zasobów Azure](../resource-group-authoring-templates.md)

### <a name="azure-sdk-26-for-visual-studio"></a>Azure SDK 2.6 programu Visual Studio ###

Najnowsza SDK zawiera ulepszenia do pomocy technicznej szablonu Menedżera zasobów w edytorze JSON. Można umożliwia szybkie tworzenie szablonu grupy zasobów od podstaw lub otwieranie istniejącego szablonu JSON (na przykład szablonu pobranego galerii) do modyfikacji, wypełnianie pliku parametrów, a nawet wdrażanie grupa zasobów bezpośrednio z rozwiązanie Azure grupa zasobów.

Aby uzyskać więcej informacji zobacz [Azure 2.6 zestaw SDK programu Visual Studio](/blog/2015/04/29/announcing-the-azure-sdk-2-6-for-net/).

### <a name="azure-powershell-080-or-later"></a>Azure programu PowerShell 0.8.0 lub nowsza ###

Począwszy od wersji 0.8.0, instalacja Azure programu PowerShell zawiera moduł Menedżera zasobów Azure oprócz Azure modułu. Ten nowy moduł umożliwia skrypt wdrożenia grup zasobów.

Aby uzyskać więcej informacji zobacz [Używanie Azure przy użyciu Menedżera zasobów Azure](../powershell-azure-resource-manager.md)

### <a name="azure-resource-explorer"></a>Eksplorator Azure zasobów ###

To [Narzędzie Podgląd](https://resources.azure.com) umożliwia przegląd definicji JSON wszystkich grup zasobów w subskrypcji i poszczególnych zasobów. W narzędziu można edytowanie definicji JSON zasobu, usuwanie całej hierarchii zasobów i tworzenia nowych zasobów.  Łatwo dostępne w tym narzędziu informacje jest szczególnie przydatne do tworzenia szablonu, ponieważ zawiera jakie właściwości należy ustawić dla określonego typu zasobów prawidłowe wartości, itp. Możesz nawet utworzenia grupy zasobów w [Azure Portal](https://portal.azure.com/), a następnie przeprowadź inspekcję jej definicje JSON w narzędziu explorer ułatwiają templatize grupa zasobów.

### <a name="deploy-to-azure-button"></a>Wdrażanie do przycisku Azure ###

Jeśli GitHub na potrzeby kontrolki źródła, można umieszczać [Rozmieszczanie do przycisku Azure](/blog/2014/11/13/deploy-to-azure-button-for-azure-websites-2/) w usługi — Plik README. MD, umożliwiający wdrożenie Włącz klucz interfejsu użytkownika umożliwiającego Azure. Czynności można wykonać dla dowolnej aplikacji web proste, można rozszerzyć tę opcję, aby włączyć wdrażanie grupa zasobów całego przez umieszczenie pliku azuredeploy.json w katalogu głównym repozytorium. Ten plik JSON, który zawiera szablon grupa zasobów, posłuży przez rozmieszczanie Azure przycisk Tworzenie grup zasobów. Na przykład zobacz próbki [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) , które będą używane w tym samouczku.

## <a name="get-the-sample-resource-group-template"></a>Uzyskiwanie przykładowy szablon grupa zasobów ##

Aby teraz korzystaj prawo, aby go.

1.  Przejdź do przykładowych aplikacji usługi [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) .

2.   W readme.md kliknij pozycję **Deploy Azure**.
 
3.  Jesteś przekierowanie do strony [Wdrażanie przez azure](https://deploy.azure.com) i poproszony o podanie parametrów wdrożenia. Zwróć uwagę, że większość pól są wypełnione o nazwie repozytorium i niektórych ciągach losową dla Ciebie. Wszystkie pola można zmieniać, jeśli chcesz, ale tylko rzeczy, które należy wprowadzić logowania administracyjne programu SQL Server i hasło, a następnie kliknij przycisk **Dalej**.
 
    ![](./media/app-service-deploy-complex-application-predictably/gettemplate-1-deploybuttonui.png)

4.  Następnie kliknij pozycję **Rozmieszczanie** , aby rozpocząć proces wdrożenia. Gdy proces działa do ukończenia, kliknij http://todoapp*XXXX*. azurewebsites.net łącze do przeglądania wdrożonej aplikacji. 

    ![](./media/app-service-deploy-complex-application-predictably/gettemplate-2-deployprogress.png)

    Interfejs użytkownika będzie wolno nieco podczas najpierw przechodzenia do niego ponieważ są tylko uruchamianie aplikacji, ale przekonać się, że jest w pełni funkcjonalny aplikacji.

5.  Ponownie na stronie Rozmieść kliknij łącze **Zarządzaj** , aby wyświetlić nowej aplikacji w Azure Portal.

6.  Na liście rozwijanej **Essentials** kliknij łącze grupy zasobów. Należy zauważyć, że aplikacji sieci web jest już podłączony do repozytorium GitHub w obszarze **Projekt zewnętrzny**. 

    ![](./media/app-service-deploy-complex-application-predictably/gettemplate-3-portalresourcegroup.png)
 
7.  W karta Grupa zasobów należy pamiętać, że już obiema aplikacjami sieci web i jednej bazy danych SQL w grupie zasobów.

    ![](./media/app-service-deploy-complex-application-predictably/gettemplate-4-portalresourcegroupclicked.png)
 
Wszystko, który został wyświetlony w ciągu kilku minut krótki aplikację w pełni wdrożone microservice dwóch z wszystkie składniki, zależności, ustawienia, bazy danych i publikowanie ciągły, skonfigurowany przez automatyczną aranżacji w Menedżerze zasobów Azure. To wszystko zostało to zrobione przez dwie czynności:

-   Rozmieszczanie do przycisku Azure
-   azuredeploy.JSON w katalogu głównym repo

Możesz wdrożyć tej samej aplikacji dziesiątki, setki lub tysiące razy i mieć taką samą konfigurację dokładnie zawsze. Powtarzalność i przewidywalności tej metody umożliwia wdrażanie aplikacji wysokiej klasy z ułatwienia i zaufania.

## <a name="examine-or-edit-azuredeployjson"></a>Sprawdzenie lub edytować AZUREDEPLOY. JSON ##

Teraz Przyjrzyjmy się konfiguracji repozytorium GitHub. Będzie przy użyciu edytora JSON w Azure .NET SDK, więc jeśli jeszcze nie zainstalowano [Azure .NET SDK 2.6](/downloads/), to zrobić.

1.  Klonowanie repozytorium [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) narzędzie cyfra Ulubione. Poniżej ekranu robimy to w Eksploratorze zespołu w Visual Studio 2013.

    ![](./media/app-service-deploy-complex-application-predictably/examinejson-1-vsclone.png)

2.  Na poziomie głównym repozytorium Otwórz azuredeploy.json w programie Visual Studio. Jeśli nie widzisz okienka konspektu JSON, należy zainstalować Azure .NET SDK.

    ![](./media/app-service-deploy-complex-application-predictably/examinejson-2-vsjsoneditor.png)

Nacisnę nie opisują każdej szczegółów JSON format, ale sekcja [Więcej zasobów](#resources) zawiera łącza do nauki język szablonu grupy zasobów. W tym miejscu po prostu nacisnę pokazują interesujące funkcje, które ułatwiają rozpoczęcie pracy dokonując szablonu niestandardowego do wdrożenia aplikacji.

### <a name="parameters"></a>Parametry ###

Zapoznaj się z sekcją parametrów czy większość z tych parametrów są co przycisku **Rozmieszczanie Azure** monituje. Witryna za przycisku **Rozmieszczanie Azure** wypełnia danych wejściowych przy użyciu parametrów zdefiniowanych w azuredeploy.json interfejsu użytkownika. Te parametry są używane w całym definicji zasobów, takich jak nazwy zasobów, wartości nieruchomości itp.

### <a name="resources"></a>Zasoby ###

W węźle zasobów widać, że są definiowane 4 zasobów najwyższego poziomu, w tym wystąpienie programu SQL Server, plan usług aplikacji i obiema aplikacjami sieci web. 

#### <a name="app-service-plan"></a>Plan usług aplikacji ####

Zacznijmy od prostej zasobu poziomie głównym w formacie JSON. W konspekcie JSON kliknij pozycję plan usług aplikacji o nazwie **[hostingPlanName]** , aby wyróżnić odpowiedni kod JSON. 

![](./media/app-service-deploy-complex-application-predictably/examinejson-3-appserviceplan.png)

Należy zauważyć, że `type` element określa ciąg (wywołania farmy serwerów długie, długi czas temu) planu aplikacji usługi i inne elementy i właściwości są wypełniane przy użyciu parametrów zdefiniowanych w pliku JSON i tego zasobu nie ma żadnych zagnieżdżonych zasobów.

>[AZURE.NOTE] Należy zauważyć, że wartość `apiVersion` informuje Azure, jaka wersja programu interfejsu API usługi REST, aby użyć JSON definicji zasobów z, a może mieć wpływ na sposób zasób ma mieć format wewnątrz `{}`. 

#### <a name="sql-server"></a>Program SQL Server ####

Następnie kliknij na zasób programu SQL Server o nazwie **SQL** w konspekcie JSON.

![](./media/app-service-deploy-complex-application-predictably/examinejson-4-sqlserver.png)
 
Należy pamiętać o następujących kwestiach związanych wyróżniony kod JSON:

-   Używanie parametrów zapewnia, że utworzone zasoby są o nazwie i skonfigurować w taki sposób, który sprawia, że zgodne ze sobą.
-   Zasób programu ma dwa zasoby zagnieżdżonych, każda ma inną wartość `type`.
-   Zasoby zagnieżdżone wewnątrz `“resources”: […]`, gdzie są definiowane bazy danych i reguły zapory, masz `dependsOn` element, który określa identyfikator zasobu zasobów SQL poziomie głównym. Menedżer zasobów Azure, to informuje "przed utworzeniem tego zasobu, która inny zasób, musi już istnieć; a jeśli tego innego zasobu jest zdefiniowana w szablonie, następnie utwórz co najpierw".

    >[AZURE.NOTE] Aby uzyskać szczegółowe informacje dotyczące sposobu używania `resourceId()` funkcja, zobacz [Funkcje szablonu Menedżera zasobów Azure](../resource-group-template-functions.md).

-   Efekt `dependsOn` element jest, że Menedżer zasobów Azure można ustalić zasoby, które mogą być tworzone równolegle i zasoby, które muszą zostać utworzone kolejno. 

#### <a name="web-app"></a>W przeglądarce ####

Teraz Przyjrzyjmy się rzeczywisty aplikacji web App, które są bardziej skomplikowana. Kliknij aplikację sieci web [variables('apiSiteName')] w konspekcie JSON, aby wyróżnić jego kod JSON. Należy zauważyć, że elementy są wprowadzenie bardziej interesujące. W tym celu będzie można porozmawiać o funkcjach kolejno:

##### <a name="root-resource"></a>Zasób katalogu głównego #####

Aplikacji sieci web, zależy od dwóch różnych zasobów. Oznacza to, że Menedżer zasobów Azure utworzy aplikacji sieci web, tylko wtedy, gdy są tworzone zarówno plan usług aplikacji i wystąpienie programu SQL Server.

![](./media/app-service-deploy-complex-application-predictably/examinejson-5-webapproot.png)

##### <a name="app-settings"></a>Ustawienia aplikacji #####

Ustawienia aplikacji można zdefiniować jako zasób zagnieżdżonych.

![](./media/app-service-deploy-complex-application-predictably/examinejson-6-webappsettings.png)

W `properties` elementu `config/appsettings`, masz dwie ustawień aplikacji w formacie `“<name>” : “<value>”`.

-   `PROJECT`to [Ustawienie KUDU](https://github.com/projectkudu/kudu/wiki/Customizing-deployments) zachęcający Azure wdrożenia projektu do użycia w rozwiązania programu Visual Studio wielu elementów projektu. I zostanie wyświetlona później, jak kontrolki źródła jest skonfigurowany, ale ponieważ kod ToDoApp jest w rozwiązania programu Visual Studio wielu elementów projektu, potrzebujemy to ustawienie.
-   `clientUrl`to, że korzysta po prostu aplikacji ustawienie kod aplikacji.

##### <a name="connection-strings"></a>Parametry połączenia #####

Parametry połączenia można zdefiniować jako zasób zagnieżdżonych.

![](./media/app-service-deploy-complex-application-predictably/examinejson-7-webappconnstr.png)

W `properties` elementu `config/connectionstrings`, każdy ciąg połączenia jest również zdefiniowany jako parę nazwa: wartość z określonym formatem `“<name>” : {“value”: “…”, “type”: “…”}`. Aby uzyskać `type` elementu, możliwe wartości są `MySql`, `SQLServer`, `SQLAzure`, i `Custom`.

>[AZURE.TIP] Aby ostateczne listę typów parametry połączenia, uruchom następujące polecenie w programie PowerShell Azure: \[Enum]::GetNames("Microsoft.WindowsAzure.Commands.Utilities.Websites.Services.WebEntities.DatabaseType")
    
##### <a name="source-control"></a>Formant źródła #####

Można zdefiniować ustawień kontroli źródła jako zasób zagnieżdżonych. Azure Menedżer zasobów używa tego zasobu, aby skonfigurować publikowanie ciągły (Zobacz ostrzeżenie na `IsManualIntegration` później), a także, aby rozpocząć wdrażanie kodu aplikacji automatycznie podczas przetwarzania pliku JSON.

![](./media/app-service-deploy-complex-application-predictably/examinejson-8-webappsourcecontrol.png)

`RepoUrl`i `branch` powinny być bardzo intuicyjny i powinny wskazywać repozytorium cyfra i nazwę gałęzi do opublikowania ze. Ponownie są one definiowane przez parametrów wejściowych. 

Uwaga w `dependsOn` element, aby oprócz zasobu aplikacji sieci web, `sourcecontrols/web` zależy od `config/appsettings` i `config/connectionstrings`. Dzieje się tak dlatego po `sourcecontrols/web` jest skonfigurowane, procesu wdrażania Azure automatycznie spróbuje wdrożenia, utworzyć i uruchomić kod aplikacji. W związku z tym wstawiając tę współzależność ułatwia upewnij się, że aplikacja ma dostęp do ustawień aplikacji wymagane i ciągów połączenia przed kod aplikacji. 

>[AZURE.NOTE] Należy zauważyć, że `IsManualIntegration` jest ustawiona na `true`. Ta właściwość jest konieczne w ramach tego samouczka, ponieważ nie masz faktycznie repozytorium GitHub, a więc nie faktycznie udzielić uprawnień Azure, aby skonfigurować ciągłe publikowanie z [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) (to znaczy powiadomienia push aktualizacje automatyczne repozytorium Azure). Można użyć wartości domyślnych `false` dla określonego repozytorium tylko wtedy, gdy skonfigurowano poświadczeń GitHub właściciela [Azure portal](https://portal.azure.com/) przed. Innymi słowy Jeśli masz skonfigurowane kontrolki źródła GitHub lub BitBucket dla dowolnej aplikacji w [Azure Portal](https://portal.azure.com/) wcześniej, przy użyciu poświadczeń użytkownika, następnie Azure będzie pamiętać poświadczenia i ich używać w przyszłości wdrażanie dowolnej aplikacji z GitHub lub BitBucket. Jednak jeśli jeszcze tego nie zrobiono tego wcześniej, wdrożenie szablonu JSON zakończy się niepowodzeniem Menedżera zasobów Azure próba Konfigurowanie ustawień sterowania źródła aplikacji sieci web, ponieważ nie można zalogować do GitHub lub BitBucket, przy użyciu poświadczeń właściciela repozytorium.

## <a name="compare-the-json-template-with-deployed-resource-group"></a>Porównanie szablon JSON z grupy wdrożonym zasobów ##

W tym miejscu możesz przejść do karty wszystkich aplikacji sieci web firmy w [Azure Portal](https://portal.azure.com/), ale istnieje innego narzędzia, która jest tak jak przydatne, jeśli nie. Przejdź do narzędzia Podgląd [Azure Eksploratora zasobów](https://resources.azure.com) , które zawiera reprezentację JSON wszystkich grup zasobów w subskrypcji, jakie istnieją faktycznie w Azure wewnętrznej bazy danych. Można też wyświetlić, jak grupa zasobów JSON hierarchii w Azure zgodny z hierarchii w pliku szablonu, który jest używany do jej utworzenia.

Na przykład gdy przejdź do narzędzia [Eksploratora zasobów Azure](https://resources.azure.com) i rozwiń węzły w Eksploratorze, widać grupy zasobów i zasoby głównego poziomu, które są zbierane w obszarze typy odpowiednich zasobów.

![](./media/app-service-deploy-complex-application-predictably/ARM-1-treeview.png)

Jeśli możesz Drąż w dół do aplikacji sieci web, powinno być możliwe wyświetlić szczegóły konfiguracji aplikacji sieci web podobne do poniżej zrzut ekranu:

![](./media/app-service-deploy-complex-application-predictably/ARM-2-jsonview.png)

Ponownie zagnieżdżonych zasobów powinna być bardzo podobne do tych w pliku szablonu JSON hierarchii, i powinien zostać wyświetlony ustawień aplikacji, parametry połączenia, itd., prawidłowo widoczna w okienku JSON. Brak opisane tutaj ustawienia może oznaczać problem z plikiem JSON i ułatwiają rozwiązywanie problemów z pliku szablonu JSON.

## <a name="deploy-the-resource-group-template-yourself"></a>Wdrażanie szablonu grupa zasobów, samodzielnie ##

Przycisk **Deploy Azure** doskonale nadaje się, ale umożliwia wdrażanie szablonu grupy zasobów w azuredeploy.json tylko wtedy, gdy masz już przypisany azuredeploy.json GitHub. Azure .NET SDK udostępnia narzędzia na wdrożenie dowolny plik szablonu JSON bezpośrednio z komputera lokalnego. Aby to zrobić, wykonaj następujące czynności:

1.  W programie Visual Studio, kliknij pozycję **plik** > **Nowy** > **projektu**.

2.  Kliknij przycisk **Visual C#** > **chmury** > **Azure grupa zasobów**, następnie kliknij **przycisk OK**.

    ![](./media/app-service-deploy-complex-application-predictably/deploy-1-vsproject.png)

3.  **Wybierz szablon Azure**zaznacz **Pustego szablonu** , a następnie kliknij **przycisk OK**.

4.  Przeciągnij azuredeploy.json do folderu **szablonu** z nowym projektem.

    ![](./media/app-service-deploy-complex-application-predictably/deploy-2-copyjson.png)

5.  W Eksploratorze rozwiązań otwórz skopiowany azuredeploy.json.

6.  Tylko dla demonstracji możesz dodać standardowe zasoby wglądu aplikacji do naszych pliku JSON, klikając pozycję **Dodaj zasobów**. Jeśli po prostu interesuje wdrażania pliku JSON, przejdź do czynności rozmieszczanie.

    ![](./media/app-service-deploy-complex-application-predictably/deploy-3-newresource.png)

7.  Wybierz pozycję **Wniosków aplikacji dla aplikacji sieci Web**, upewnij się, istniejącej usługi aplikacji planowanie i aplikacji sieci web jest zaznaczone, a następnie kliknij przycisk **Dodaj**.

    ![](./media/app-service-deploy-complex-application-predictably/deploy-4-newappinsight.png)

    Teraz możesz wyświetlić kilka nowych zasobów, które, w zależności od tego zasobu i działanie, umożliwiać zależności od planu usług aplikacji lub aplikacji sieci web. Te zasoby nie są włączone przez ich istniejącej definicji i użytkownik chce zmienić.

    ![](./media/app-service-deploy-complex-application-predictably/deploy-5-appinsightresources.png)
 
8.  W konspekcie JSON kliknij przycisk **appInsights AutoScale** , aby wyróżnić jego kod JSON. Jest to ustawienie skalowania dla Twojego planu usługi aplikacji.

9.  Wyróżnione kod JSON, Znajdź `location` i `enabled` właściwości i je tak jak pokazano poniżej.

    ![](./media/app-service-deploy-complex-application-predictably/deploy-6-autoscalesettings.png)

10. W konspekcie JSON kliknij przycisk **CPUHigh appInsights** , aby wyróżnić jego kod JSON. To jest alert.

11. Znajdź `location` i `isEnabled` właściwości i je tak jak pokazano poniżej. Zrób to samo dla innych alerty trzy (fioletowa bulw).

    ![](./media/app-service-deploy-complex-application-predictably/deploy-7-alerts.png)

12. Teraz możesz przystąpić do wdrożenia. Kliknij prawym przyciskiem myszy projektu i wybierz pozycję **Deploy** > **Nowego wdrożenia**.

    ![](./media/app-service-deploy-complex-application-predictably/deploy-8-newdeployment.png)

13. Zaloguj się do konta usługi Azure, jeśli jeszcze tego nie zrobiono.

14. Wybierz istniejącą grupę zasobów w ramach subskrypcji lub utworzyć nową, wybierz pozycję **azuredeploy.json**, a następnie kliknij **Edytuj parametry**.

    ![](./media/app-service-deploy-complex-application-predictably/deploy-9-deployconfig.png)

    Teraz możesz edytować wszystkie parametry zdefiniowane w pliku szablonu w tabeli i. Parametry, które definiują domyślne będzie już wartości domyślne i parametry określające listy dozwolonych wartości będą wyświetlane jako list rozwijanych.

    ![](./media/app-service-deploy-complex-application-predictably/deploy-10-parametereditor.png)

15. Wypełnij puste parametry, a następnie użyj [adresu repo GitHub ToDoApp](https://github.com/azure-appservice-samples/ToDoApp.git) w **repoUrl**. Następnie kliknij przycisk **Zapisz**.
 
    ![](./media/app-service-deploy-complex-application-predictably/deploy-11-parametereditorfilled.png)

    >[AZURE.NOTE] Autoscaling jest funkcją oferowane w warstwie **Standardowy** lub nowszy, a plan poziomu alertów są funkcje dostępne w warstwie **podstawowe** lub nowszy, musisz Ustaw parametr **sku** na **Standard** lub **Premium** , aby zobaczyć wszystkie do nowej aplikacji wniosków podświetlony zasobów.
    
16. Kliknij pozycję **wdrożenie**. Jeśli zaznaczono opcję **Zapisz hasła**, hasło zostaną zapisane w parametru pliku **w formacie zwykłego tekstu**. W przeciwnym razie zostanie wyświetlony monit wprowadzenia hasła bazy danych podczas procesu wdrażania.

To wszystko! Teraz wystarczy przejść do [Azure Portal](https://portal.azure.com/) i narzędzie [Azure Eksploratora zasobów](https://resources.azure.com) , aby wyświetlić nowe alerty i ustawienia autoscale dodane do usługi JSON wdrożyć aplikację.

Etapów w tej sekcji głównie wykonać następujące czynności:

1.  Przygotować plik szablonu
2.  Utworzony plik parametr z pliku szablonu
3.  Używany plik szablonu z pliku parametrów

Ostatnim krokiem jest łatwe wykonać polecenia cmdlet programu PowerShell. Aby wyświetlić Visual Studio działania po jego wdrożeniu aplikacji, otwórz Scripts\Deploy AzureResourceGroup.ps1. Istnieje wiele kodu, ale tylko przechodzę do wyróżnienia stosowne kodu, należy wdrożyć plik szablonu z pliku parametrów.

![](./media/app-service-deploy-complex-application-predictably/deploy-12-powershellsnippet.png)

Polecenia cmdlet ostatniego `New-AzureResourceGroup`, który faktycznie wykonuje akcję. To wszystko należy wykazać możesz, za pomocą narzędzia jest stosunkowo proste właściwie wdrażania aplikacji chmury. Każdorazowo po uruchomieniu polecenia cmdlet na tym samym szablonie z tego samego pliku parametr użytkownik chce uzyskać ten sam wynik.

## <a name="summary"></a>Podsumowanie ##

W DevOps powtarzalności i przewidywalności są klawiszy dowolnego pomyślnego wdrożenia aplikacji wysokiej klasy składający się z microservices. W tym samouczku wdrożono aplikację microservice dwa Azure jako pojedynczej grupy zasobów za pomocą tego szablonu Azure Menedżera zasobów. Hopefully jej udzielił Ci wiedzy, potrzebne w celu rozpocząć konwertowania aplikacji Azure szablonu można obsługi administracyjnej i wdrożyć go właściwie. 

## <a name="next-steps"></a>Następne kroki ##

Dowiedz się, jak [zastosować adaptacyjne metodologii i stale opublikowanie aplikacji microservices z łatwością](app-service-agile-software-development.md) i wdrażania zaawansowanych technik, takich jak [flighting wdrożenia](app-service-web-test-in-production-controlled-test-flight.md) łatwe.

<a name="resources"></a>
## <a name="more-resources"></a>Więcej zasobów ##

-   [Azure język szablonu Menedżera zasobów](../resource-group-authoring-templates.md)
-   [Tworzenie szablonów Azure Menedżera zasobów](../resource-group-authoring-templates.md)
-   [Azure funkcji szablonu Menedżera zasobów](../resource-group-template-functions.md)
-   [Wdrażanie aplikacji za pomocą szablonu Azure Menedżera zasobów](../resource-group-template-deploy.md)
-   [Przy użyciu programu PowerShell Azure przy użyciu Menedżera zasobów Azure](../powershell-azure-resource-manager.md)
-   [Rozwiązywanie problemów z Grupa zasobów wdrażania Azure](../resource-manager-troubleshoot-deployments-portal.md)




 
