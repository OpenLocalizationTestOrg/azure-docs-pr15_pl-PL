<properties
    pageTitle="Ciągły dostarczania z cyfra i Visual Studio Team Services platformy Azure | Microsoft Azure"
    description="Dowiedz się, jak skonfigurować projektów zespołu Visual Studio Team Services za pomocą cyfra automatycznie Utwórz i Wdroż funkcji aplikacji sieci Web w usługach Azure aplikacji usługi lub w chmurze."
    services="cloud-services"
    documentationCenter=".net"
    authors="mlearned"
    manager="douge"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="07/06/2016"
    ms.author="mlearned"/>

# <a name="continuous-delivery-to-azure-using-visual-studio-team-services-and-git"></a>Ciągły dostarczenia Azure za pomocą programu Visual Studio Team Services i cyfra

Za pomocą programu Visual Studio Team Services zespołu projektów hosta repozytorium cyfra kod źródłowy, automatyczne tworzenie i wdrażanie aplikacji sieci web Azure i usług w chmurze przy każdym push zatwierdzania do repozytorium.

Musisz Visual Studio 2013 i SDK Azure zainstalowany. Jeśli nie masz jeszcze Visual Studio 2013, pobierz ją, wybierając pozycję **wprowadzenie bezpłatnie** link u [www.visualstudio.com](http://www.visualstudio.com). Zainstaluj Azure SDK [tutaj](http://go.microsoft.com/fwlink/?LinkId=239540).


> [AZURE.NOTE]Potrzebne jest konto programu Visual Studio Team Services do użycia tego samouczka: możesz [otworzyć konto programu Visual Studio Team Services bezpłatnie](http://go.microsoft.com/fwlink/p/?LinkId=512979).

Aby skonfigurować usługi w chmurze spowoduje automatyczne utworzenie i wdrażanie Azure za pomocą programu Visual Studio Team Services, wykonaj następujące kroki.

## <a name="1-create-a-git-repository"></a>1: Tworzenie repozytorium cyfra

1. Jeśli nie masz jeszcze konta programu Visual Studio Team Services, możesz ją uzyskać w jeden [poniżej](http://go.microsoft.com/fwlink/?LinkId=397665). Po utworzeniu projektu zespołu wybierz cyfra w systemie źródło formantu. Postępuj zgodnie z instrukcjami, aby połączyć Visual Studio do zespołu projektu.

2. W **Eksploratorze zespołu**wybierz link **klonowanie tego repozytorium** .

    ![][3]

3. Określ lokalizację kopii lokalnej, a następnie wybierz przycisk **klonowanie** .

## <a name="2-create-a-project-and-commit-it-to-the-repository"></a>2: Tworzenie projektu i przekazać je do repozytorium

1. W **Eksploratorze zespołu**, w sekcji **rozwiązań** wybierz link **Nowy** , aby utworzyć nowy projekt w lokalnym repozytorium.

    ![][4]

2. Wykonując czynności opisane w tym instruktażu, należy wdrożyć aplikacji sieci web lub usługi w chmurze (aplikacja Azure). Tworzenie nowego projektu usługi w chmurze Azure lub nowego projektu ASP.NET MVC. Upewnij się, że projekt jest przeznaczony dla programu .NET Framework 4 lub nowszy. Jeśli tworzysz projekt usługi cloud Dodawanie ról w sieci web programu ASP.NET MVC i roli pracownika.
Jeśli chcesz utworzyć aplikację sieci web, wybierz szablon projektu w **Aplikacji sieci Web programu ASP.NET** , a następnie wybierz **MVC**. Aby uzyskać więcej informacji, zobacz [Tworzenie aplikacji sieci web programu ASP.NET w usłudze Azure aplikacji](../app-service-web/web-sites-dotnet-get-started.md) .

3. Otwieranie menu skrótów dla rozwiązanie, a następnie wybierz pozycję **Zatwierdzenie**.

    ![][7]

4. Jeśli wykorzystano cyfra w Visual Studio Team Services po raz pierwszy, musisz informacje do identyfikowania użytkownika w Cyfra. W obszarze **Oczekiwanie** **Eksplorator zespołów**wprowadź Twoja nazwa użytkownika i adres e-mail. Wprowadź komentarz na zatwierdzenie, a następnie wybierz przycisk **Zatwierdź** .

    ![][8]

5. Uwaga opcje, aby uwzględnić lub wykluczyć określone zmiany wprowadzone po zaewidencjonowaniu. Żądane zmiany są wyłączone, jeśli **Zawiera wszystkie**.

6. Zostały teraz Projekt zatwierdzony zmiany w kopii lokalnej repozytorium. Następnie zsynchronizować te zmiany na serwerze, wybierając pozycję łącze **synchronizacji** .

## <a name="3-connect-the-project-to-azure"></a>3: Łączenie projektu Azure

1. Teraz, gdy masz repozytorium cyfra Visual Studio Team Services przy użyciu kodu źródłowego w nim, możesz przystąpić do nawiązać repozytorium cyfra Azure.  W [portalu klasyczny Azure](http://go.microsoft.com/fwlink/?LinkID=213885)wybierz chmury aplikacji usług lub sieci web lub utworzyć nową, wybierając ikonę + w lewym dolnym rogu i wybierając pozycję **Usługa w chmurze** lub **Aplikacji sieci Web** , a następnie **Szybkiego tworzenia**.

    ![][9]

3. Dla usług w chmurze wybierz link **skonfigurować publikowanie przy użyciu programu Visual Studio Team Services** . W przypadku aplikacji sieci web wybierz link **Konfigurowanie rozmieszczania z kontrolki źródła** .

    ![][10]

2. W kreatorze wpisz nazwę swojego konta programu Visual Studio Team Services w polu tekstowym i wybierz łącze **Autoryzuj teraz** . Może być zostanie poproszony o zalogowanie się.

    ![][11]

3. W **Żądania połączenia** okno podręczne wybierz pozycję **Zaakceptuj** , aby autoryzować Azure, aby skonfigurować zespołu projektu w programie Visual Studio zespołu Services.

    ![][12]

4. Po pomyślnym autoryzacji zobaczysz listę rozwijaną, która zawiera projektów zespołu Visual Studio Team Services.  Wybierz nazwę zespołu projektu, utworzonym w poprzednich krokach, a następnie wybierz przycisk znacznika wyboru kreatora.

    ![][13]

    Przy następnym push zatwierdzania do repozytorium, Visual Studio Team Services tworzenie i wdrażanie projektu Azure.

## <a name="4-trigger-a-rebuild-and-redeploy-your-project"></a>4: wyzwalanie odtwarzania i ponownie wdróż projektu

1. W programie Visual Studio otwarcia pliku i zmień go. Na przykład zmienić plik `_Layout.cshtml` pod widokami\\folder udostępniony w roli MVC sieci web.

    ![][17]

2. Edytuj tekst stopki witryny i Zapisz plik.

    ![][18]

3. W **Eksploratorze rozwiązań**Otwórz menu skrótów dla węzła rozwiązanie, węzeł projektu lub plik, który został zmieniony, a następnie wybierz **Zatwierdzenie**.

4. Wpisz komentarz i wybierz pozycję **Zatwierdzenie**.

    ![][20]

5. Wybierz link **synchronizacji** .

    ![][38]

6. Wybierz link **wypychanych** , aby przekazać do zatwierdzenia do repozytorium w Visual Studio Team Services. (Umożliwia także przycisk **Synchronizuj** skopiować do zatwierdzenia do repozytorium. Różnica polega na że **synchronizacji** również pobiera najnowsze zmiany z repozytorium.)

    ![][39]

7. Wybieranie przycisku **Strona główna** , aby powrócić do strony głównej **Explorer zespołu** .

    ![][21]

8. Wybierz pozycję **tworzy** , aby wyświetlić kompilacjach w toku.

    ![][22]

    **Eksplorator zespołów** pokazano, że została wyzwolona kompilacji do wyboru w.

    ![][23]

9. Aby wyświetlić szczegółowy dziennik postępem kompilacji, kliknij dwukrotnie nazwę kompilacji w toku.

10. Gdy kompilacja jest w toku, zapoznaj się definicję kompilacji, które zostały utworzone podczas kreatora jest używany do łączenia Azure.  Otwieranie menu skrótów dla definicji kompilacji i wybierz pozycję **Edycja definicji tworzenie**.

    ![][25]

11. Na karcie **wyzwalacza** pojawi się, że definicji kompilacji jest domyślnie dla każdej ewidencjonowanie. (Dla usługi w chmurze, Visual Studio Team Services tworzy i wdraża wzorca gałąź środowiska wzorcowego automatycznie. Nadal masz w celu zrealizowania ręcznego kroku wdrażania na stronie. Dla aplikacji sieci web, która nie zawiera tymczasowej środowiska wdraża wzorca gałąź bezpośrednio na stronie.

    ![][26]

1. Na karcie **proces** możesz sprawdzić, czy środowiska wdrażania jest ustawiona na nazwę aplikacji chmury usługi lub sieci web.

    ![][27]

1. Określ wartości właściwości, jeśli chcesz, aby wartości innego niż domyślne. Właściwości publikowania Azure znajdują się w sekcji **wdrażania** , a także może być konieczne ustawianie parametrów MSBuild. Na przykład w chmurze projektu usługi, aby określić konfigurację usługi niż "Chmury", Ustaw parametry MSbuild `/p:TargetProfile=[YourProfile]` Jeśli *[YourProfile]* odpowiada pliku konfiguracji usługi pod nazwą, takich jak ServiceConfiguration. *YourProfile*.cscfg.

    W poniższej tabeli przedstawiono dostępne właściwości w sekcji **wdrażania** :

  	|Właściwość|Wartość domyślna|
  	|---|---|
  	|Zezwalaj na niezaufanych certyfikatów|Jeśli ma wartość FAŁSZ, certyfikatów SSL muszą być zalogowani przy główny urząd certyfikacji.|
  	|Zezwalaj na uaktualnianie|Umożliwia wdrożenie, aby zaktualizować istniejące wdrożenia zamiast tworzenia nowej witryny. Zachowuje znaki z adresem IP.|
  	|Nie usuwaj|Jeśli wartość true, należy zastępować istniejącego wdrożenia niepowiązane (dozwolone uaktualnienia).|
  	|Ścieżkę dostępu do ustawień rozmieszczania|Ścieżka do pliku .pubxml dla aplikacji sieci web, względem folderu głównego repo. Ignorowane dla usług w chmurze.|
  	|Środowisko wdrażania programu SharePoint|Taki sam, jak nazwa usługi.|
  	|Środowisko wdrażania Azure|Nazwa sieci web app lub w chmurze usługi.|

1. Przy tym razem z kompilacji należy ukończona pomyślnie.

    ![][28]

1. Dwukrotne kliknięcie nazwy kompilacji programu Visual Studio zawiera **Podsumowanie tworzenie**, włączając dowolne wyniki testu skojarzone projektów test.

    ![][29]

1. [Portal Azure klasyczny](http://go.microsoft.com/fwlink/?LinkID=213885)możesz wyświetlić skojarzone wdrożenia na karcie **wdrożeń** po zaznaczeniu środowiska wzorcowego.

    ![][30]

1.  Przejdź do adresu URL witryny. Dla aplikacji sieci web wybierz przycisk **Przeglądaj** w portalu. Dla usługi w chmurze wybierz adres URL w sekcji **Szybkie skrócie** przedstawiający środowiska przemieszczania strony **pulpitu nawigacyjnego** .

    Wdrożenia z ciągłym integracji dla usług w chmurze są publikowane w środowisku przemieszczania domyślnie. Można to zmienić, ustawiając właściwość **Alternatywny środowisko usługi w chmurze** z **produkcją**. Oto, gdzie jest adres URL witryny na stronie pulpitu nawigacyjnego usługi w chmurze.

    ![][31]

    Aby odsłonić uruchomionego witryny zostanie otwarty nowej karcie w przeglądarce.

    ![][32]

1.  Jeśli wprowadzisz zmiany w projekcie, możesz wyzwalacza więcej tworzy, a zostanie zebrać wielu wdrożeń. Najnowszego jest oznaczony jako aktywny.

    ![][33]

## <a name="5-redeploy-an-earlier-build"></a>5: ponownie wdróż wcześniejszą kompilację

Ten krok jest opcjonalny. W portalu klasyczny Azure wybierz wcześniejszych wdrożenia, a następnie wybierz pozycję **ponownie wdróż** przewijanie do tyłu witryny do wcześniejszej ewidencjonowania. Zauważ, że to będzie wyzwalanie nową kompilację w programie TFS i tworzenie nowego wpisu w historii wdrożenia.

![][34]

## <a name="6-change-the-production-deployment"></a>6: zmienianie rozmieszczenia produkcji

Gdy skończysz, można awansować środowiska tymczasowego w środowisku produkcyjnym, wybierając pozycję **Zamień** w portalu klasycznym Azure. Nowo wdrożonej środowiska przemieszczania jest podwyższany do produkcji i poprzednich środowisku produkcyjnym, staje się środowiska przemieszczania. Aktywne wdrożenia mogą być inne niż dotyczące produkcji i przemieszczenia środowiskach, ale wdrożenia historii ostatnich wersjach jest taki sam, niezależnie od tego środowiska.

![][35]

## <a name="7-deploy-from-a-working-branch"></a>7: wdrażanie z gałęzią pracy.

Użycie cyfra, zazwyczaj wprowadzeniu zmian w gałęzi pracy i integrowanie wzorca gałąź po projektowaniu osiągnie stan Gotowe. Podczas fazy opracowywania projektu warto Utwórz i Wdroż Azure gałąź pracy.

1. W **Eksploratorze zespołów**wybierz przycisk **Start** , a następnie wybierz przycisk **oddziałów** .

    ![][40]

2. Wybierz link **Nowego oddziału** .

    ![][41]

3. Wprowadź nazwę gałęzi, na przykład "pracuję," i wybierz pozycję **Utwórz gałąź**. Spowoduje to utworzenie nowego oddziału lokalny.

    ![][42]

4. Publikowanie gałęzi. Wybierz nazwę gałęzi w **gałęziami wycofanych**i wybierz pozycję **Publikuj**.

    ![][44]

6. Domyślnie tylko zmiany do wzorca gałęzi wyzwalanie ciągły kompilacji. Aby skonfigurować ciągły kompilacji dla gałąź pracy, wybierz stronę **tworzy** w **Eksploratorze zespołów**i wybierz **Edycja definicji tworzenie**.

7. Otwórz kartę **Ustawienia źródeł** . W obszarze **monitorowane Przechodzenie do integracji ciągły i kompilacji**wybierz pozycję **kliknij tutaj, aby dodać nowy wiersz**.

    ![][47]

8. Określ gałęzi, która została utworzona, takich jak odwołań głowy i pracy.

    ![][48]

9. Zmiany w kodzie, otwórz menu skrótów dla zmienionego pliku, a następnie wybierz **Zatwierdzenie**.

    ![][43]

10. Wybierz łącze **Niezsynchronizowane zatwierdzenia** , a następnie wybierz przycisk **Synchronizuj** lub **Push** łącze, aby skopiować zmiany do kopii gałąź pracy w Visual Studio Team Services.

    ![][45]

11. Przejdź do widoku **tworzy** i Znajdź kompilacji, powodującej tylko masz dotyczącego gałęzi pracy.

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej porad na temat korzystania z programu Visual Studio Team Services cyfra, zobacz [opracowanie i udostępnianie kodu w Cyfra przy użyciu programu Visual Studio](http://www.visualstudio.com/get-started/share-your-code-in-git-vs.aspx) i uzyskać informacji o używaniu repozytorium cyfra, które nie są zarządzane przez program Visual Studio Team Services publikowanie Azure, zobacz [Ciągły rozmieszczania Azure aplikacji usługi](../app-service-web/app-service-continuous-deployment.md). Aby uzyskać więcej informacji dotyczących programu Visual Studio Team Services zobacz [Visual Studio Team Services](http://go.microsoft.com/fwlink/?LinkId=253861).

[0]: ./media/cloud-services-continuous-delivery-use-vso/tfs0.PNG
[1]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateTeamProjectInGit.PNG
[2]: ./media/cloud-services-continuous-delivery-use-vso/tfs2.png
[3]: ./media/cloud-services-continuous-delivery-use-vso-git/CloneThisRepository.PNG
[4]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateNewSolutionInClonedRepo.PNG
[7]: ./media/cloud-services-continuous-delivery-use-vso-git/CommitMenuItem.PNG
[8]: ./media/cloud-services-continuous-delivery-use-vso-git/CommitAChange2.PNG
[9]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateCloudService.PNG
[10]: ./media/cloud-services-continuous-delivery-use-vso-git/SetUpPublishingWithVSO.PNG
[11]: ./media/cloud-services-continuous-delivery-use-vso-git/AuthorizeConnection.PNG
[12]: ./media/cloud-services-continuous-delivery-use-vso-git/ConnectionRequest.PNG
[13]: ./media/cloud-services-continuous-delivery-use-vso-git/ChooseARepo3.PNG
[14]: ./media/cloud-services-continuous-delivery-use-vso/tfs14.png
[15]: ./media/cloud-services-continuous-delivery-use-vso/tfs15.png
[16]: ./media/cloud-services-continuous-delivery-use-vso/tfs16.png
[17]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs17.png
[18]: ./media/cloud-services-continuous-delivery-use-vso-git/MakeACodeChange.PNG
[20]: ./media/cloud-services-continuous-delivery-use-vso-git/CommitAChange2.PNG
[21]: ./media/cloud-services-continuous-delivery-use-vso-git/TeamExplorerHome.png
[22]: ./media/cloud-services-continuous-delivery-use-vso-git/TeamExplorerBuilds.PNG
[23]: ./media/cloud-services-continuous-delivery-use-vso-git/BuildInQueue.png
[24]: ./media/cloud-services-continuous-delivery-use-vso/tfs24.png
[25]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs25.png
[26]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs26.png
[27]: ./media/cloud-services-continuous-delivery-use-vso-git/ProcessTab.PNG
[28]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs28.png
[29]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs29.png
[30]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs30.png
[31]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs31.png
[32]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs32.png
[33]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs33.png
[34]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs34.png
[35]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs35.png
[36]: ./media/cloud-services-continuous-delivery-use-vso/tfs36.PNG
[37]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateANewAccount.PNG
[38]: ./media/cloud-services-continuous-delivery-use-vso-git/SyncChanges2.PNG
[39]: ./media/cloud-services-continuous-delivery-use-vso-git/PushCurrentBranch.PNG
[40]: ./media/cloud-services-continuous-delivery-use-vso-git/BranchesInTeamExplorer.PNG
[41]: ./media/cloud-services-continuous-delivery-use-vso-git/NewBranch.PNG
[42]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateBranch.PNG
[43]: ./media/cloud-services-continuous-delivery-use-vso-git/CommitAChange2.PNG
[44]: ./media/cloud-services-continuous-delivery-use-vso-git/PublishBranch.PNG
[45]: ./media/cloud-services-continuous-delivery-use-vso-git/SyncChanges2.PNG
[47]: ./media/cloud-services-continuous-delivery-use-vso-git/SourceSettingsPage.PNG
[48]: ./media/cloud-services-continuous-delivery-use-vso-git/IncludeWorkingBranch.PNG
