<properties
    pageTitle="Ciągły dostarczania przy użyciu programu Visual Studio Team Services platformy Azure | Microsoft Azure"
    description="Dowiedz się, jak skonfigurować projektów zespołu Visual Studio Team Services spowoduje automatyczne utworzenie i wdrażanie funkcji aplikacji sieci Web w usługach Azure aplikacji usługi lub w chmurze."
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

# <a name="continuous-delivery-to-azure-using-visual-studio-team-services"></a>Ciągły dostarczenia Azure za pomocą programu Visual Studio Team Services

Możesz skonfigurować projektów zespołów programu Visual Studio Team Services automatyczne tworzenie i wdrażanie aplikacji sieci web Azure i usług w chmurze.  (Aby uzyskać informacje dotyczące konfigurowania ciągły tworzenie i wdrażanie systemu przy użyciu *lokalnego* programu Team Foundation Server, zobacz [Ciągły dostawy dla usług w chmurze platformy Azure](cloud-services-dotnet-continuous-delivery.md)).

Tego samouczka przyjęto założenie, że masz Visual Studio 2013 i SDK Azure zainstalowany. Jeśli nie masz jeszcze Visual Studio 2013, pobierz ją, wybierając pozycję **wprowadzenie bezpłatnie** link u [www.visualstudio.com](http://www.visualstudio.com). Zainstaluj zestaw SDK Azure [tutaj](http://go.microsoft.com/fwlink/?LinkId=239540).

> [AZURE.NOTE]Potrzebne jest konto programu Visual Studio Team Services do użycia tego samouczka: możesz [otworzyć konto programu Visual Studio Team Services bezpłatnie](http://go.microsoft.com/fwlink/p/?LinkId=512979).

Aby skonfigurować usługi w chmurze spowoduje automatyczne utworzenie i wdrażanie Azure za pomocą programu Visual Studio Team Services, wykonaj następujące kroki.

## <a name="1-create-a-team-project"></a>1: Utworzenie zespołu projektu

Postępuj zgodnie z instrukcjami [poniżej](http://go.microsoft.com/fwlink/?LinkId=512980) do tworzenia projektu zespołu i połączyć go z programu Visual Studio. W tym instruktażu założono, że używasz Team Foundation wersji Control (TFVC) jako źródło formantu rozwiązania. Jeśli chcesz użyć cyfra kontroli wersji, zobacz [wersji cyfra tego instruktażu](http://go.microsoft.com/fwlink/p/?LinkId=397358).

## <a name="2-check-in-a-project-to-source-control"></a>2: zaewidencjonować projekt kontrolki źródła

1. W programie Visual Studio Otwórz rozwiązanie, które chcesz wdrożyć, lub Utwórz nowy.
Wykonując czynności opisane w tym instruktażu, należy wdrożyć aplikacji sieci web lub usługi w chmurze (aplikacja Azure).
Jeśli chcesz utworzyć nowe rozwiązanie, należy utworzyć nowy projekt usługi w chmurze Azure lub nowego projektu ASP.NET MVC. Upewnij się, że projekt jest przeznaczony dla programu .NET Framework 4 lub 4,5, a w przypadku tworzenia projektu usługi cloud Dodawanie ról w sieci web programu ASP.NET MVC i roli Pracownik i wybierz aplikację internetową dla ról w sieci web. Po wyświetleniu monitu wybierz **Aplikację internetową**.
Jeśli chcesz utworzyć aplikację sieci web, wybierz szablon projektu w aplikacji sieci Web programu ASP.NET, a następnie wybierz MVC. Zobacz [Tworzenie aplikacji sieci web programu ASP.NET w usłudze Azure aplikacji](../app-service-web/web-sites-dotnet-get-started.md).

    > [AZURE.NOTE] Visual Studio Team Services obsługuje tylko CI wdrożenia programu Visual Studio aplikacji sieci Web w tej chwili. Projekty witryny sieci Web są poza zakresem.

1. Otwórz menu kontekstowego dla rozwiązania i wybierz polecenie **Dodaj rozwiązanie do kontrolki źródła**.

    ![][5]

1. Akceptowanie lub zmienić ustawienia domyślne i wybierz przycisk **OK** . Po zakończeniu procesu, źródło formantu ikony pojawiają się w **Eksploratorze rozwiązań**.

    ![][6]

1. Otwórz menu skrótów dla rozwiązanie i wybierz polecenie **Zaewidencjonuj**.

    ![][7]

1. W obszarze **Oczekiwanie** **Eksplorator zespołów**wpisz komentarz dotyczący zaewidencjonowania i wybierz przycisk **Zaewidencjonuj** .

    ![][8]

    Uwaga opcje, aby uwzględnić lub wykluczyć określone zmiany wprowadzone po zaewidencjonowaniu. Żądane zmiany są wyłączone, jeśli **Zawiera wszystkie** łącza.

    ![][9]

## <a name="3-connect-the-project-to-azure"></a>3: Łączenie projektu Azure

1. Teraz, gdy masz w PORÓWNANIU z Team Services zespołu projektu przy użyciu kodu źródłowego w nim, możesz przystąpić Azure nawiązać połączenia do zespołu projektu.  W [portalu klasyczny Azure](http://go.microsoft.com/fwlink/?LinkID=213885)wybierz chmury aplikacji usług lub sieci web lub Utwórz nową, wybierając pozycję **+** ikonę w lewym dolnym rogu i wybierając pozycję **Usługa w chmurze** lub **Aplikacji sieci Web** , a następnie **Szybkiego tworzenia**. Wybierz link **skonfigurować publikowanie przy użyciu programu Visual Studio Team Services** .

    ![][10]

1. W kreatorze wpisz nazwę swojego konta programu Visual Studio Team Services w polu tekstowym i kliknij łącze **Autoryzuj teraz** . Może być zostanie poproszony o zalogowanie się.

    ![][11]

1. W **Żądania połączenia** okno podręczne wybierz przycisk **Zaakceptuj** , aby autoryzować Azure, aby skonfigurować zespołu projektu w PORÓWNANIU z Team Services.

    ![][12]

1. Po pomyślnym autoryzacji Zobacz listy rozwijanej zawierającą listę projektów zespołu Visual Studio Team Services. Wybierz nazwę zespołu projektu, utworzonym w poprzednich krokach, a następnie wybierz przycisk znacznika wyboru kreatora.

    ![][13]

1. Po projektu jest połączony, zobaczysz niektóre instrukcje dotyczące sprawdzania zmian do zespołu projektu programu Visual Studio Team Services.  Na swojej następnej ewidencjonowanie Visual Studio Team Services tworzenie i wdrażanie projektu Azure.  Wypróbuj teraz po kliknięciu łącza **Zaewidencjonuj, z programu Visual Studio** , a następnie łącze **Uruchamianie programu Visual Studio** (lub równoważne **Visual Studio** przycisk u dołu ekranu portalu).

    ![][14]

## <a name="4-trigger-a-rebuild-and-redeploy-your-project"></a>4: wyzwalanie odtwarzania i ponownie wdróż projektu

1. W programie Visual Studio **Eksplorator zespołów**wybierz link **Eksploratorze kontroli źródła** .

    ![][15]

1. Przejdź do pliku rozwiązanie, a następnie otwórz go.

    ![][16]

1. W **Eksploratorze rozwiązań**otwarcia pliku i zmień go. Na przykład zmienić plik `_Layout.cshtml` pod widokami\\folder udostępniony w roli MVC sieci web.

    ![][17]

1. Edytowanie logo witryny, a następnie wybierz pozycję **Klawisze Ctrl + S** , aby zapisać plik.

    ![][18]

1. W **Eksploratorze zespołu**wybierz link **Oczekujących zmian** .

    ![][19]

1. Wpisz komentarz, a następnie wybierz przycisk **Zaewidencjonuj** .

    ![][20]

1. Wybierz przycisk **Narzędzia główne** , aby powrócić do strony głównej **Eksplorator zespołów** .

    ![][21]

1. Wybierz pozycję **tworzy** łącze, aby wyświetlić kompilacjach w toku.

    ![][22]

    **Eksplorator zespołów** pokazano, że została wyzwolona kompilacji do wyboru w.

    ![][23]

1. Kliknij dwukrotnie nazwę kompilacji w toku, aby wyświetlić szczegółowy dziennik postępem kompilacji.

    ![][24]

1. Gdy kompilacja jest w toku, zapoznaj się definicję kompilacji, które zostały utworzone podczas połączone TFS Azure za pomocą kreatora.  Otwieranie menu skrótów dla definicji kompilacji i wybierz pozycję **Edycja definicji tworzenie**.

    ![][25]

    Na karcie **wyzwalacza** pojawi się, że definicji Konstruuj jest domyślnie dla każdej ewidencjonowania.

    ![][26]

    Na karcie **proces** możesz sprawdzić, czy środowiska wdrażania jest ustawiona na nazwę aplikacji chmury usługi lub sieci web. Jeśli pracujesz z aplikacjami sieci web, właściwości, które widzisz będą inne niż pokazano na ilustracji.

    ![][27]

1. Określ wartości właściwości, jeśli chcesz, aby wartości innego niż domyślne. Właściwości publikowania Azure znajdują się w sekcji **wdrożenia** .

    W poniższej tabeli przedstawiono dostępne właściwości w sekcji **wdrażania** :

  	|Właściwość|Wartość domyślna|
  	|---|---|
  	|Zezwalaj na niezaufanych certyfikatów|Jeśli ma wartość FAŁSZ, certyfikatów SSL muszą być zalogowani przy główny urząd certyfikacji.|
  	|Zezwalaj na uaktualnianie|Umożliwia wdrożenie, aby zaktualizować istniejące wdrożenia zamiast tworzenia nowej witryny. Zachowuje znaki z adresem IP.|
  	|Nie usuwaj|Jeśli wartość true, należy zastępować istniejącego wdrożenia niepowiązane (dozwolone uaktualnienia).|
  	|Ścieżkę dostępu do ustawień rozmieszczania|Ścieżka do pliku .pubxml dla aplikacji sieci web, względem folderu głównego repo. Ignorowane dla usług w chmurze.|
  	|Środowisko wdrażania programu SharePoint|Taki sam, jak nazwa usługi.|
  	|Środowisko wdrażania Azure|Nazwa sieci web app lub w chmurze usługi.|

1. Jeśli korzystasz z wielu usług konfiguracji (pliki .cscfg), możesz określić konfiguracji żądanej usługi ustawienie **tworzyć zaawansowane, argumenty MSBuild** . Na przykład, aby użyć ServiceConfiguration.Test.cscfg, argumentach MSBuild opcja wiersz `/p:TargetProfile=Test`.

    ![][38]

    Przy tym razem z kompilacji należy ukończona pomyślnie.

    ![][28]

1. Dwukrotne kliknięcie nazwy kompilacji programu Visual Studio zawiera **Podsumowanie tworzenie**, włączając dowolne wyniki testu skojarzone projektów test.

    ![][29]

1. [Portal Azure klasycznego](http://go.microsoft.com/fwlink/?LinkID=213885)możesz wyświetlić skojarzone wdrożenia na karcie **wdrożeń** po zaznaczeniu środowiska wzorcowego.

    ![][30]

1.  Przejdź do adresu URL witryny. Dla aplikacji sieci web kliknij przycisk **Przeglądaj** , na pasku poleceń. Dla usługi w chmurze wybierz adres URL w **Skrócie szybkie** części strony **pulpitu nawigacyjnego** , przedstawiający środowiska przemieszczania dla usługi w chmurze. Wdrożenia z ciągłym integracji dla usług w chmurze są publikowane w środowisku przemieszczania domyślnie. Można to zmienić, ustawiając właściwość **Alternatywny środowisko usługi w chmurze** z **produkcją**. To zrzut ekranu wskazuje miejsce, w którym adres URL witryny na stronie pulpitu nawigacyjnego usługi w chmurze.

    ![][31]

    Aby odsłonić uruchomionego witryny zostanie otwarty nowej karcie w przeglądarce.

    ![][32]

    Dla usług w chmurze Jeśli wprowadzisz zmiany w projekcie, możesz wyzwalacza więcej tworzy, a zostanie zebrać wielu wdrożeń. Najnowszego oznaczony jako aktywny.

    ![][33]

## <a name="5-redeploy-an-earlier-build"></a>5: ponownie wdróż wcześniejszą kompilację

Ten krok dotyczy usług w chmurze i jest opcjonalna. W portalu klasyczny Azure wybierz wcześniejszych wdrożenia, a następnie wybierz przycisk **ponownie wdróż** przewijanie do tyłu witryny do wcześniejszej ewidencjonowania.  Zauważ, że to będzie wyzwalanie nową kompilację w programie TFS i tworzenie nowego wpisu w historii wdrożenia.

![][34]

## <a name="6-change-the-production-deployment"></a>6: zmienianie rozmieszczenia produkcji

Ten krok dotyczy tylko usług w chmurze, nie aplikacji sieci web. Gdy skończysz, można awansować środowiska tymczasowego w środowisku produkcyjnym, wybierając przycisk **Zamień** w portalu klasyczny Azure. Nowo wdrożonym środowiska przemieszczania jest podwyższany do produkcji i poprzedniego środowisku produkcyjnym, jeśli istnieją, staje się środowiska przemieszczania. Aktywne wdrożenia mogą być różne dla produkcji i przygotowania środowiska, ale wdrożenia historii ostatnich wersjach jest taki sam, niezależnie od tego środowiska.

![][35]

## <a name="7-run-unit-tests"></a>7: Uruchom testy jednostek

Ten krok dotyczy tylko usługi sieci web apps, nie usług w chmurze. Aby umieścić brama jakości wdrożenia, możesz uruchomić testy jednostek, a jeżeli nie wpiszą, możesz wyłączyć wdrożenia.

1.  W programie Visual Studio umożliwia dodanie projektu badania jednostki.

    ![][39]

1.  Dodawanie odwołania projektu do projektu, który ma zostać sprawdzona.

    ![][40]

1.  Dodawanie niektóre testy jednostek. Aby rozpocząć pracę, spróbuj fikcyjna test, który będzie zawsze przekazać.

        ```
        using System;
        using Microsoft.VisualStudio.TestTools.UnitTesting;

        namespace UnitTestProject1
        {
            [TestClass]
            public class UnitTest1
            {
                [TestMethod]
                [ExpectedException(typeof(NotImplementedException))]
                public void TestMethod1()
                {
                    throw new NotImplementedException();
                }
            }
        }
        ```

1.  Edytowanie definicji kompilacji, wybierz kartę **proces** , a następnie rozwiń węzeł **Test** .

1.  **Tworzenie błędów w przypadku awarii test** ustawiona na PRAWDA. Oznacza to, że wdrażanie nie występują chyba że przekazać testów.

    ![][41]

1.  Kolejka nową kompilację.

    ![][42]

    ![][43]

1. Podczas kompilacji jest postępowania, sprawdź postęp.

    ![][44]

    ![][45]

1. Po zakończeniu kompilacji, sprawdź wyniki testu.

    ![][46]

    ![][47]

1.  Spróbuj utworzyć test, który nie powiedzie się. Dodaj nowe badania, kopiując pierwszego, zmienić jego nazwę, a komentarz wiersza kodu, informujący, że NotImplementedException jest oczekiwany wyjątek.

        ```
        [TestMethod]
        //[ExpectedException(typeof(NotImplementedException))]
        public void TestMethod2()
        {
            throw new NotImplementedException();
        }
        ```

1. Zaznacz w polu Zmień na kolejki nową kompilację.

    ![][48]

1. Wyświetlanie wyników test, aby wyświetlić szczegółowe informacje o błędzie.

    ![][49]

    ![][50]

## <a name="next-steps"></a>Następne kroki
Aby uzyskać więcej informacji o testowania w Visual Studio Team Services jednostek zobacz [Uruchom testy jednostek w swojej kompilacji](http://go.microsoft.com/fwlink/p/?LinkId=510474). Jeśli korzystasz z cyfra, zobacz [Udostępnianie kodu w Cyfra](http://www.visualstudio.com/get-started/share-your-code-in-git-vs.aspx) i [ciągły rozmieszczania Azure aplikacji usługi](../app-service-web/app-service-continuous-deployment.md).  Aby uzyskać więcej informacji na temat programu Visual Studio Team Services zobacz [Visual Studio Team Services](http://go.microsoft.com/fwlink/?LinkId=253861).

[0]: ./media/cloud-services-continuous-delivery-use-vso/tfs0.PNG
[1]: ./media/cloud-services-continuous-delivery-use-vso/tfs1.png
[2]: ./media/cloud-services-continuous-delivery-use-vso/tfs2.png

[5]: ./media/cloud-services-continuous-delivery-use-vso/tfs5.png
[6]: ./media/cloud-services-continuous-delivery-use-vso/tfs6.png
[7]: ./media/cloud-services-continuous-delivery-use-vso/tfs7.png
[8]: ./media/cloud-services-continuous-delivery-use-vso/tfs8.png
[9]: ./media/cloud-services-continuous-delivery-use-vso/tfs9.png
[10]: ./media/cloud-services-continuous-delivery-use-vso/tfs10.png
[11]: ./media/cloud-services-continuous-delivery-use-vso/tfs11.png
[12]: ./media/cloud-services-continuous-delivery-use-vso/tfs12.png
[13]: ./media/cloud-services-continuous-delivery-use-vso/tfs13.png
[14]: ./media/cloud-services-continuous-delivery-use-vso/tfs14.png
[15]: ./media/cloud-services-continuous-delivery-use-vso/tfs15.png
[16]: ./media/cloud-services-continuous-delivery-use-vso/tfs16.png
[17]: ./media/cloud-services-continuous-delivery-use-vso/tfs17.png
[18]: ./media/cloud-services-continuous-delivery-use-vso/tfs18.png
[19]: ./media/cloud-services-continuous-delivery-use-vso/tfs19.png
[20]: ./media/cloud-services-continuous-delivery-use-vso/tfs20.png
[21]: ./media/cloud-services-continuous-delivery-use-vso/tfs21.png
[22]: ./media/cloud-services-continuous-delivery-use-vso/tfs22.png
[23]: ./media/cloud-services-continuous-delivery-use-vso/tfs23.png
[24]: ./media/cloud-services-continuous-delivery-use-vso/tfs24.png
[25]: ./media/cloud-services-continuous-delivery-use-vso/tfs25.png
[26]: ./media/cloud-services-continuous-delivery-use-vso/tfs26.png
[27]: ./media/cloud-services-continuous-delivery-use-vso/tfs27.png
[28]: ./media/cloud-services-continuous-delivery-use-vso/tfs28.png
[29]: ./media/cloud-services-continuous-delivery-use-vso/tfs29.png
[30]: ./media/cloud-services-continuous-delivery-use-vso/tfs30.png
[31]: ./media/cloud-services-continuous-delivery-use-vso/tfs31.png
[32]: ./media/cloud-services-continuous-delivery-use-vso/tfs32.png
[33]: ./media/cloud-services-continuous-delivery-use-vso/tfs33.png
[34]: ./media/cloud-services-continuous-delivery-use-vso/tfs34.png
[35]: ./media/cloud-services-continuous-delivery-use-vso/tfs35.png
[36]: ./media/cloud-services-continuous-delivery-use-vso/tfs36.PNG
[37]: ./media/cloud-services-continuous-delivery-use-vso/tfs37.PNG
[38]: ./media/cloud-services-continuous-delivery-use-vso/AdvancedMSBuildSettings.PNG
[39]: ./media/cloud-services-continuous-delivery-use-vso/AddUnitTestProject.PNG
[40]: ./media/cloud-services-continuous-delivery-use-vso/AddProjectReferences.PNG
[41]: ./media/cloud-services-continuous-delivery-use-vso/EditBuildDefinitionForTest.PNG
[42]: ./media/cloud-services-continuous-delivery-use-vso/QueueNewBuild.PNG
[43]: ./media/cloud-services-continuous-delivery-use-vso/QueueBuildDialog.PNG
[44]: ./media/cloud-services-continuous-delivery-use-vso/BuildInTeamExplorer.PNG
[45]: ./media/cloud-services-continuous-delivery-use-vso/BuildRequestInfo.PNG
[46]: ./media/cloud-services-continuous-delivery-use-vso/BuildSucceeded.PNG
[47]: ./media/cloud-services-continuous-delivery-use-vso/TestResultsPassed.PNG
[48]: ./media/cloud-services-continuous-delivery-use-vso/CheckInChangeToMakeTestsFail.PNG
[49]: ./media/cloud-services-continuous-delivery-use-vso/TestsFailed.PNG
[50]: ./media/cloud-services-continuous-delivery-use-vso/TestsResultsFailed.PNG
