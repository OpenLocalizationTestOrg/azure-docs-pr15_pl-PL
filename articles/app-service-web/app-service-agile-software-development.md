<properties
    pageTitle="Adaptacyjne programowania Azure aplikacji usługi"
    description="Dowiedz się, jak tworzenie złożonych aplikacji wysokiej klasy z Azure aplikacji usługi w sposób, który obsługuje adaptacyjne programowania."
    services="app-service"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/01/2016"
    ms.author="cephalin"/>


# <a name="agile-software-development-with-azure-app-service"></a>Adaptacyjne programowania Azure aplikacji usługi #

W tym samouczku dowiesz się, jak utworzyć złożone aplikacje wysokiej klasy z [Azure aplikacji usługi](/services/app-service/) w sposób, który obsługuje [adaptacyjne programowania](https://en.wikipedia.org/wiki/Agile_software_development). Przyjęto założenie, że już wiesz, jak [wdrożyć złożone aplikacje właściwie platformy Azure](app-service-deploy-complex-application-predictably.md).

Ograniczenia w procesy techniczne można często stanowi przeszkody dla pomyślnego wdrożenia adaptacyjne metodologii. Azure aplikacji usługi przy użyciu funkcji, takich jak [Publikowanie ciągły](app-service-continuous-deployment.md), [tymczasowej środowiskach](web-sites-staged-publishing.md) (gniazd) i [monitorowania](web-sites-monitor.md), gdy rozsądnie uzupełniona aranżacji i zarządzanie wdrożenia w [Menedżera zasobów Azure](../azure-resource-manager/resource-group-overview.md), może być częścią doskonałe rozwiązanie dla deweloperów, którzy obejmować adaptacyjne programowania.

Poniższa tabela jest krótka lista wymagań związanych z adaptacyjne rozwoju i jak Azure services umożliwia ich.

| Wymaganie | Jak umożliwia Azure |
|---------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| — Tworzenie z każdej Zatwierdź<br>-Automatycznie utworzyć i to szybko | Przy zastosowaniu ciągły wdrażania, usługa Azure aplikacji może działać jako kompilacjach live pracy według gałęzi deweloperów. Zawsze kod zostanie przypisany do gałęzi, jest automatycznie wbudowane i rozpocząć pracę żywo platformy Azure.|
| -Upewnij tworzy własny testowania | Ładowanie testów, testy sieci web, itd., mogą być wdrażane za pomocą szablonu Azure Menedżera zasobów.|
| -Wykonania testów na sklonuj środowisku produkcyjnym | Azure szablony Menedżera zasobów umożliwia tworzenie klony środowisku produkcyjnym Azure (w tym ustawień aplikacji, szablony parametry połączenia, skalowania, itp.) do testowania szybko i właściwie.|
| — Wyświetlanie wyników najnowszej kompilacji łatwo | Ciągły rozmieszczania Azure z repozytorium oznacza, że można przetestować nowy kod w aplikacji live natychmiast po zatwierdzić wprowadzone zmiany. |
| -Zatwierdzić gałąź głównym każdego dnia<br>-Zautomatyzowanie wdrożenia. | Ciągły integracji aplikacji produkcji z gałęzią głównym repozytorium automatycznie wdraża każdej Zatwierdź/korespondencji seryjnej do głównego gałęzi produkcji. |

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="what-you-will-do"></a>Co będzie zrobić ##

Typowy przepływ pracy deweloperów test etap produkcji będzie szczegółową w celu opublikowania nowych zmian do aplikacji przykładowych [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) składa się z dwóch [aplikacji sieci web](/services/app-service/web/), z których jedna jest serwera sieci Web (ran), a drugi interfejs API sieci Web wewnętrznej bazy danych (Belgia) i bazą [bazy danych SQL](/services/sql-database/). Będzie współdziałać architektura wdrażania, jak pokazano poniżej:

![](./media/app-service-agile-software-development/what-1-architecture.png)

Aby umieścić obraz na wyrazy:

-   Architektura wdrażania podzielono trzech odrębnych środowiskach (lub [grup zasobów](../azure-resource-manager/resource-group-overview.md) w Azure), każda z własną [plan usług aplikacji](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md), ustawień [skalowania](web-sites-scale.md) i baza danych SQL. 
-   Każdego środowiska mogą być zarządzane oddzielnie. Nawet mogą istnieć w różnych subskrypcjach.
-   Jako dwa gniazda samej aplikacji usługi aplikacji są stosowane przemieszczania i produkcji. Gałąź wzorca jest ustawiony na potrzeby ciągły Integracja z tymczasową przedział.
-   Po zweryfikowaniu Zatwierdź gałąź wzorca na tymczasowy przedział (z danymi produkcji) zatwierdzonych aplikacji tymczasowy jest zamienione na produkcji przedział [bez przestojów](web-sites-staged-publishing.md).

Środowisko produkcji i przygotowanie jest definiowana w szablonie na [ * &lt;repository_root >*/ARMTemplates/ProdandStage.json](https://github.com/azure-appservice-samples/ToDoApp/blob/master/ARMTemplates/ProdAndStage.json).

W środowiskach deweloperów i przetestuj są definiowane przez szablon w [ * &lt;repository_root >*/ARMTemplates/Dev.json](https://github.com/azure-appservice-samples/ToDoApp/blob/master/ARMTemplates/Dev.json).

Typowe strategii rozgałęziania, będzie również używać z kodu przenoszone ze gałąź deweloperów maksymalnie oddział test, następnie do wzorca gałęzi (przesunięcie w górę jakości, więc do speak).

![](./media/app-service-agile-software-development/what-2-branches.png) 

## <a name="what-you-will-need"></a>Co będzie potrzebne ##

-   Konto Azure
-   Konto [GitHub](https://github.com/)
-   Powłoki cyfra (instalowana z [GitHub dla systemu Windows](https://windows.github.com/)) — pozwala na uruchomienie poleceń cyfra i programu PowerShell w tej samej sesji 
-   Najnowsze bitów [Azure programu PowerShell](https://github.com/Azure/azure-powershell/releases/download/0.9.4-June2015/azure-powershell.0.9.4.msi)
-   Podstawy z następujących czynności:
    -   [Menedżer zasobów Azure](../azure-resource-manager/resource-group-overview.md) rozmieszczania szablonu (również patrz [Rozmieszczanie złożonej aplikacji właściwie platformy Azure](app-service-deploy-complex-application-predictably.md))
    -   [Cyfra](http://git-scm.com/documentation)
    -   [Programu PowerShell](https://technet.microsoft.com/library/bb978526.aspx)

> [AZURE.NOTE] Potrzebne jest konto Azure do użycia tego samouczka:
> + Możesz [otworzyć konto Azure bezpłatnie](/pricing/free-trial/) — pobranie środków Umożliwia wypróbowanie płatnych usług Azure, a nawet w przypadku, gdy są używane w górę możesz zachować konta i użyj wolny Azure usług, takich jak aplikacje sieci Web.
> + Można [aktywować korzyści subskrybentów Visual Studio](/pricing/member-offers/msdn-benefits-details/) — subskrypcji i Visual Studio umożliwia środków co miesiąc, używanej usługi Azure płatnej.
>
> Jeśli chcesz rozpocząć pracę z Azure aplikacji usługi przed utworzeniem konta dla konta Azure, przejdź do [Spróbuj aplikacji usługi](http://go.microsoft.com/fwlink/?LinkId=523751), którym natychmiast można utworzyć aplikację sieci web krótkotrwałe starter w aplikacji usługi. Nie kart kredytowych wymagane; nie zobowiązania.

## <a name="set-up-your-production-environment"></a>Konfigurowanie środowisku produkcyjnym ##

>[AZURE.NOTE] Skrypt używane w tym samouczku automatycznie skonfiguruje ciągły publikowania z repozytorium GitHub. Wymaga to, że poświadczenia GitHub znajdują się już w Azure, w przeciwnym razie rozmieszczenia z użyciem skryptów nie powiedzie się podczas próby skonfigurowania ustawień kontroli źródła dla aplikacji sieci web. 
>
>Przechowywanie poświadczeń GitHub w Azure, tworzenie aplikacji sieci web w [Azure Portal](https://portal.azure.com/) i [skonfigurować wdrożenie GitHub](app-service-continuous-deployment.md). Wystarczy zrobić to tylko raz. 

Typowy scenariusz DevOps masz aplikację, która jest uruchomiony żywo platformy Azure, a chcesz wprowadzić zmiany do publikowania ciągły. W tym scenariuszu masz szablonu, który opracowanych, przetestowane i wdrażane środowisku produkcyjnym. Będzie został on skonfigurowany w tej sekcji.

1.  Tworzenie własnych rozwidlenie repozytorium [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) . Informacje na temat tworzenia usługi rozwidlenie zobacz [rozwidlenie Repo](https://help.github.com/articles/fork-a-repo/). Po utworzeniu usługi rozwidlenie widać go w przeglądarce.
 
    ![](./media/app-service-agile-software-development/production-1-private-repo.png)

2.  Otwórz sesję powłoki cyfra. Jeśli nie masz jeszcze powłoki cyfra, zainstaluj teraz [GitHub dla systemu Windows](https://windows.github.com/) .

3.  Tworzenie lokalnej klonowanie usługi rozwidlenia, wykonując następujące polecenie:

        git clone https://github.com/<your_fork>/ToDoApp.git 

4.  Po umieszczeniu swojej lokalnej klonowanie, przejdź do * &lt;repository_root >*\ARMTemplates i uruchom deploy.ps1 skryptów w następujący sposób:

        .\deploy.ps1 –RepoUrl https://github.com/<your_fork>/todoapp.git

4.  Po wyświetleniu monitu wpisz odpowiednią nazwę użytkownika i hasło dostępu do bazy danych.

    Powinien zostać wyświetlony obsługi administracyjnej postępu różnych Azure zasobów. Po zakończeniu wdrożenia skrypt uruchamianie aplikacji w przeglądarce i zapewniają przyjazny dźwiękowy.

    ![](./media/app-service-agile-software-development/production-2-app-in-browser.png)
 
    >[AZURE.TIP] Zapoznaj się * &lt;repository_root >*\ARMTemplates\Deploy.ps1, aby zobaczyć, jak generuje zasoby z unikatowych identyfikatorów. To samo podejście umożliwia tworzenie klony samej wdrażania, nie martwiąc się powodujące konflikt nazw zasobów.
 
6.  Ponownie podczas sesji powłoki cyfra Uruchom:

        .\swap –Name ToDoApp<unique_string>master

    ![](./media/app-service-agile-software-development/production-4-swap.png)

7.  Po zakończeniu działania skrypt, wróć do przejdź do adresu serwera sieci Web (http://ToDoApp*&lt;unique_string >*master.azurewebsites.net/) do Zobacz aplikacja działa w produkcji.
 
5.  Zaloguj się do usługi [Azure Portal](https://portal.azure.com/) i zapoznaj się co to jest tworzona.

    Powinny być widoczne dwie aplikacje sieci web w tej samej grupy zasobów, z `Api` sufiks w polu Nazwa. Jeśli możesz obejrzeć widoku grupa zasobów, pojawi się także z bazą danych SQL i serwer, plan usług aplikacji i tymczasową gniazda dla aplikacji sieci web. Przeglądanie różnych zasobów i porównaj je z * &lt;repository_root >*\ARMTemplates\ProdAndStage.json aby zobaczyć, jak są skonfigurowane w szablonie.

    ![](./media/app-service-agile-software-development/production-3-resource-group-view.png)

Teraz masz skonfigurowane środowisku produkcyjnym. Następnie będzie rozpocząć nowe aktualizacje do aplikacji.

## <a name="create-dev-and-test-branches"></a>Tworzenie deweloperów i testowanie oddziałów ##

Teraz, gdy masz złożonej aplikacji działa w produkcji w Azure spowoduje aktualizację do aplikacji zgodnie z adaptacyjne metodologii. W tej sekcji utworzysz deweloperów i przetestować oddziałów, które muszą być wymagane aktualizacje.

1.  Najpierw utwórz środowisku testowym. W sesji powłoki cyfra, uruchom następujące polecenia w celu utworzenia środowiska dla nowego oddziału o nazwie **NewUpdate**. 

        git checkout -b NewUpdate
        git push origin NewUpdate 
        .\deploy.ps1 -TemplateFile .\Dev.json -RepoUrl https://github.com/<your_fork>/ToDoApp.git -Branch NewUpdate

1.  Po wyświetleniu monitu wpisz odpowiednią nazwę użytkownika i hasło dostępu do bazy danych. 

    Po zakończeniu wdrożenia skrypt uruchamianie aplikacji w przeglądarce i zapewniają przyjazny dźwiękowy. I tak jak to teraz masz nowy Rozgałęzienie własnej środowisku testowym. Poświęć chwilę czasu, aby zapoznać się z kilkoma kwestiami o tym środowisku testowym:

    -   W przypadku subskrypcji Azure można go utworzyć. Oznacza to, że środowisku produkcyjnym można zarządzać w środowisku testowym oddzielnie.
    -   Środowisku testowym działa live platformy Azure.
    -   Środowisku testowym jest identyczny z środowisku produkcyjnym, z wyjątkiem tymczasowy gniazda i ustawień skalowania. Można to wiedzieć, ponieważ są one tylko różnice między ProdandStage.json i Dev.json.
    -   Pozwala zarządzać środowisku testowym w osobnym planu aplikacji usługi, warstwa inną cenę (na przykład **wolny**).
    -   Usuwanie tego środowiska testowego będzie prosty sposób usuwania grup zasobów. Znajdziesz się, jak zrobić to [później](#delete).

2.  Przejdź do utwórz gałąź deweloperów, uruchamiając następujące polecenia:

        git checkout -b Dev
        git push origin Dev
        .\deploy.ps1 -TemplateFile .\Dev.json -RepoUrl https://github.com/<your_fork>/ToDoApp.git -Branch Dev

3.  Po wyświetleniu monitu wpisz odpowiednią nazwę użytkownika i hasło dostępu do bazy danych. 

    Poświęć chwilę czasu, aby zapoznać się z kilkoma kwestiami o tym środowisku testowym: 

    -   Środowiska deweloperów ma identyczny z środowisku testowym konfigurację, ponieważ jest wdrożony przy użyciu tego samego szablonu.
    -   Każdy środowisku testowym mogą być tworzone dla deweloperów Azure subskrypcji, pozostawiając środowisku testowym oddzielnie zarządzane.
    -   Środowiska deweloperów działa live platformy Azure.
    -   Usunięcie środowisku testowym jest tak proste, jak usuwanie grupy zasobów. Znajdziesz się, jak zrobić to [później](#delete).

>[AZURE.NOTE] Jeśli masz wiele deweloperów pracujących na podstawie nowych aktualizacji, każdy z nich można łatwo tworzyć gałęzi i środowisku testowym dedykowane, wykonując następujące czynności:
>
>1. Tworzenie własnych rozwidlenie repozytorium w GitHub (zobacz [rozwidlenia Repo](https://help.github.com/articles/fork-a-repo/)).
>2. Klonowanie rozwidlenie na komputerze lokalnym
>3. Uruchom takie same polecenia do tworzenia własnych gałąź deweloperów i środowiska.

Po wykonaniu tych usługi rozwidlenie GitHub powinien mieć trzy gałęzie:

![](./media/app-service-agile-software-development/test-1-github-view.png)

I sześć aplikacji sieci web (trzy zestawy dwóch) powinny być w trzech oddzielnych grup zasobów:

![](./media/app-service-agile-software-development/test-2-all-webapps.png)
 
>[AZURE.NOTE] Należy zauważyć, że ProdandStage.json określa środowisku produkcyjnym, aby użyć **standardowe** ceny poziom, który jest odpowiedni dla skalowalność aplikacji produkcji.

## <a name="build-and-test-every-commit"></a>Tworzenie i testowanie wszystkich zatwierdzeń ##

Pliki szablonów, ProdAndStage.json i Dev.json wyszczególniają parametry kontroli źródła, która domyślnie konfiguruje ciągły publikowania dla aplikacji sieci web. W związku z tym co Zatwierdź gałąź GitHub uaktywnia automatyczne wdrażanie Azure z tego oddziału. Zobaczmy, jak ustawienia działa teraz.

1.  Upewnij się, że jesteś w gałęzi deweloperów lokalnego repozytorium. Aby to zrobić, uruchom następujące polecenie w powłoce cyfra:

        git checkout Dev

2.  Wprowadzanie zmian w prostej do warstwy interfejsu użytkownika aplikacji, zmieniając kodu w celu użycia list [początkowego](http://getbootstrap.com/components/) . Otwórz * &lt;repository_root >*\src\MultiChannelToDo.Web\index.cshtml i uzupełnić wyróżniony Zmień poniżej:

    ![](./media/app-service-agile-software-development/commit-1-changes.png)

    >[AZURE.NOTE] Jeśli nie możesz odczytać obraz powyżej: 
    >
    >- W wierszu 18 zmienianie `check-list` do `list-group`.
    >- W wierszu 19, zmienianie `class="check-list-item"` do `class="list-group-item"`.

3.  Zapisz zmiany. Ponownie w powłoce cyfra, uruchom następujące polecenia:

        cd <repository_root>
        git add .
        git commit -m "changed to bootstrap style"
        git push origin Dev
 
    Te polecenia cyfra są podobne do "Sprawdzanie w kodzie" w innej systemu kontroli źródła, takie jak TFS. Po uruchomieniu `git push`, nowy Zatwierdź wyzwalane wypychanych automatyczne kod Azure, która następnie odbudowywania aplikacji w celu odzwierciedlenia zmian w środowisku testowym.

4.  Aby sprawdzić, że wystąpił ten wypychanych kod w środowisku deweloperów, przejdź do karta aplikacji sieci web w środowisku deweloperów i wygląd w ramach **wdrożenia** . Czy można zobaczyć Twojej najnowsze wiadomości Zatwierdź.

    ![](./media/app-service-agile-software-development/commit-2-deployed.png)

5.  W tym miejscu kliknij przycisk **Przeglądaj** , aby wyświetlić nowe zmiany w aplikacji live Azure.

    ![](./media/app-service-agile-software-development/commit-3-webapp-in-browser.png)

    To jest bardzo nieznaczną zmianę w aplikacji. Jednak ma wiele razy nowych zmian do aplikacji sieci web złożone niezamierzonych i niepożądane efekty strony. Możliwość łatwego testowania każdej transakcji w wersjach żywo pozwala na efektywnej tych problemów klientów, aby zobaczyć je.

W razie należy doświadczenia w realizacji, deweloperzy w projekcie **NewUpdate** będzie mógł łatwo utworzyć środowisku testowym dla siebie, tworzenie każdej Zatwierdź, a następnie przetestuj każdy kompilacji.

## <a name="merge-code-into-test-environment"></a>Scalanie kodu w środowisku testowym ##

Gdy zechcesz push kodu z gałęzią deweloperów maksymalnie gałąź NewUpdate, jest proces standardowy cyfra:

1.  Wszelkie nowe zatwierdzenia do NewUpdate można scalić gałąź deweloperów w GitHub, takich jak zatwierdzenia tworzone przez innych. Wszelkie nowe Zatwierdź na GitHub wyzwalanie wypychanych kod, a następnie tworzenia w środowisku testowym. Można następnie upewnij się, że kod w gałęzi deweloperów wciąż działa przy użyciu najnowszych bitów z gałęzią NewUpdate.

2.  Scalanie wszystkich nowych zatwierdzenia z gałęzią deweloperów w gałąź NewUpdate na GitHub. Ta akcja uruchamia kod wypychanych i kompilacji w środowisku testowym. 

Pamiętaj, ponieważ ciągły wdrożenia jest już skonfigurowana przy użyciu tych oddziałów cyfra, należy nie podejmować żadnych działań, takich jak działa integracji tworzy. Wystarczy wykonać wskazówki sterowania standardowego źródła przy użyciu cyfra i Azure wykona te procesy kompilacji dla Ciebie.

Teraz Przyjrzyjmy push kodu do gałęzi **NewUpdate** . W powłoce cyfra uruchom następujące polecenia:

    git checkout NewUpdate
    git pull origin NewUpdate
    git merge Dev
    git push origin NewUpdate

To wszystko! 

Przejdź do karta aplikacji sieci web w środowisku usługi test wyświetlić do nowego Zatwierdź (scalone gałąź NewUpdate) teraz przypisany do środowisku testowym. Następnie kliknij przycisk **Przeglądaj,** Aby wyświetlić zmiany stylu działa teraz live platformy Azure.

## <a name="deploy-update-to-production"></a>Wdrażanie aktualizacji produkcji ##

Naciśnięcie kodu do środowiska roboczych i produkcyjnych należy uważa nie różni się od czego czynności zostały już wykonane po naciśnięciu kodu do środowiska testowego. Jest wyjątkowo proste. 

W powłoce cyfra uruchom następujące polecenia:

    git checkout master
    git pull origin master
    git merge NewUpdate
    git push origin master

Należy pamiętać, że oparte na sposób środowiska roboczych i produkcyjnych ustawienia w ProdandStage.json, nowy kod zostanie przypisany do **przemieszczenia** przedział i działa. Aby w przypadku przejścia do adresu URL tymczasowy przedział pojawi się nowy kod działa. Aby to zrobić, uruchom `Show-AzureWebsite` polecenia cmdlet w powłoce cyfra.

    Show-AzureWebsite -Name ToDoApp<unique_string>master -Slot Staging
 
I teraz po zweryfikowaniu aktualizacji przedział tymczasowy jest jedynym elementem pozostałej do wykonania można się przełączyć ją do produkcji. W powłoce cyfra po prostu uruchom następujące polecenia:

    cd <repository_root>\ARMTemplates
    .\swap.ps1 -Name ToDoApp<unique_string>master

Gratulacje! Nowe aktualizacje zostały pomyślnie opublikowany w aplikacji sieci web produkcji. Co więcej jest po prostu została wykonana przez łatwo tworzyć deweloperów, a następnie przetestuj środowiskach i tworzenia i testowania każdej transakcji. Są to ważnych bloków konstrukcyjnych rozwoju adaptacyjne oprogramowania.

<a name="delete"></a>
## <a name="delete-dev-and-test-enviroments"></a>Usuwanie deweloperów i przetestować enviroments ##

Ponieważ celowo efekt do środowisk deweloperów i test należy grup zasobów samodzielne, jest bardzo łatwo je usunąć. Aby usunąć te tworzone w ramach tego samouczka, oddziały GitHub i artefakty Azure, po prostu uruchom następujące polecenia w powłoce cyfra:

    git branch -d Dev
    git push origin :Dev
    git branch -d NewUpdate
    git push origin :NewUpdate
    Remove-AzureRmResourceGroup -Name ToDoApp<unique_string>dev-group -Force -Verbose
    Remove-AzureRmResourceGroup -Name ToDoApp<unique_string>newupdate-group -Force -Verbose

## <a name="summary"></a>Podsumowanie ##

Tworzenie adaptacyjne oprogramowania jest musi ma wielu firm, którzy chcą przyjąć Azure jako platformy aplikacji. W tym samouczku zapoznaniu do tworzenia i usunąć repliki dokładnie w dół lub w pobliżu repliki środowisku produkcyjnym z łatwością, nawet w przypadku skomplikowanych aplikacji. Również zapoznaniu je wykorzystać ten możliwość tworzenia procesu opracowywania, który można utworzyć i przetestować każdej pojedynczej Zatwierdź w Azure. Ten samouczek hopefully pokazuje, możesz jak najlepiej umożliwia Azure aplikacji usługi i Menedżera zasobów Azure razem utworzyć rozwiązanie DevOps przeznaczoną do metodologii adaptacyjne. Można następnie tworzyć w tym scenariuszu, wykonując zaawansowanych technik DevOps, takich jak [badania produkcji](app-service-web-test-in-production-get-start.md). Aby uzyskać typowy scenariusz badań produkcji zobacz [wdrożenia Flighting (testowania wersji beta) w usłudze Azure w aplikacji](app-service-web-test-in-production-controlled-test-flight.md).

## <a name="more-resources"></a>Więcej zasobów ##

-   [Wdrażanie złożonej aplikacji właściwie platformy Azure](app-service-deploy-complex-application-predictably.md)
-   [Adaptacyjne rozwoju w praktyce: porady i wskazówki dotyczące cyklu opracowywania zmodernizowanej](http://channel9.msdn.com/Events/Ignite/2015/BRK3707)
-   [Strategie wdrażania zaawansowane dla aplikacji sieci Web Azure za pomocą szablonów Menedżera zasobów](http://channel9.msdn.com/Events/Build/2015/2-620)
-   [Tworzenie szablonów Azure Menedżera zasobów](../resource-group-authoring-templates.md)
-   [JSONLint - JSON moduł sprawdzania poprawności](http://jsonlint.com/)
-   [ARMClient — Konfigurowanie GitHub publikowania do witryny](https://github.com/projectKudu/ARMClient/wiki/Setup-GitHub-publishing-to-Site)
-   [Cyfra gałęzi — podstawowe gałęzi i scalanie](http://www.git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging)
-   [David Ebbo blogu](http://blog.davidebbo.com/)
-   [Azure programu PowerShell](../powershell-install-configure.md)
-   [Azure narzędzi wiersza polecenia i Platform](../xplat-cli-install.md)
-   [Tworzenie lub edytowanie użytkowników w Azure AD](https://msdn.microsoft.com/library/azure/hh967632.aspx#BKMK_1)
-   [Projekt Kudu stron typu Wiki](https://github.com/projectkudu/kudu/wiki)
