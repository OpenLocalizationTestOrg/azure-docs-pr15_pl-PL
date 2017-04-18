<properties
    pageTitle="Ciągły rozmieszczania Azure aplikacji usługi | Microsoft Azure"
    description="Dowiedz się, jak włączyć ciągły rozmieszczania Azure aplikacji usługi."
    services="app-service"
    documentationCenter=""
    authors="dariagrigoriu"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/28/2016"
    ms.author="dariagrigoriu"/>
    
# <a name="continuous-deployment-to-azure-app-service"></a>Ciągły rozmieszczania Azure aplikacji usługi

Ten samouczek pokazano, jak skonfigurować wdrożenie ciągły przepływ pracy dla aplikacji [Azure aplikacji usługi] . Integracja aplikacji usługi z BitBucket, GitHub i Visual Studio zespołu usługi (VSTS) umożliwia ciągły wdrożenie przepływu pracy miejsce, w którym Azure pobiera w najnowsze aktualizacje z Twojego projektu opublikowany w jednej z tych usług. Ciągły wdrożenia to świetne rozwiązanie w przypadku projektów miejsce, w którym wielu i częste wpłaty są włączone.

## <a name="overview"></a>Włączanie ciągły wdrażania

Aby włączyć ciągły wdrażania 

1. Publikowanie zawartości aplikacji repozytorium, używanym do wdrożenia ciągły.  
    Aby uzyskać więcej informacji o publikowaniu projektu do tych usług zobacz [Tworzenie repo (GitHub)], [Utwórz repo (BitBucket)]i [rozpocząć pracę z programu VSTS].

2. W swojej aplikacji menu karta w [Azure portal], kliknij przycisk **WDRAŻANIEM aplikacji > Opcje wdrażania**. Kliknij pozycję **Wybierz źródło**, a następnie wybierz źródło wdrożenia.  

    ![](./media/app-service-continuous-deployment/cd_options.png)
    
    > [AZURE.NOTE] Aby skonfigurować programu VSTS konta do wdrożenia aplikacji usługi Zobacz tego [samouczka](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App).
    
3. Wykonywanie autoryzacji przepływu pracy. 

4. W karta **źródła rozmieszczania** wybierz projektu i gałęzi Aby wdrożyć z. Gdy skończysz, kliknij **przycisk OK**.
  
    ![](./media/app-service-continuous-deployment/github_option.png)

    > [AZURE.NOTE] Podczas włączania ciągły wdrażania z GitHub lub BitBucket, pojawi się publicznych i prywatnych projektów.

    Aplikacji usługi tworzy skojarzenie z zaznaczonego repozytorium, w przypadku plików z określoną gałąź i obsługuje duplikat repozytorium dla aplikacji usługi aplikacji. Po skonfigurowaniu ciągły wdrożenia programu VSTS z portalu Azure integracja używa [Kudu aparatu wdrażania](https://github.com/projectkudu/kudu/wiki)aplikacji usługi, który już pozwala zautomatyzować tworzenie i wdrażanie zadań za pomocą każdej `git push`. Nie należy oddzielnie skonfigurować wdrożenie ciągły w programu VSTS. Po zakończeniu tego procesu, karta **Opcje wdrażania** aplikacji zostanie wyświetlona że powiodła się wdrożenia usługi active wskazujący wdrożenia.

5. Aby sprawdzić, czy pomyślnym wdrożeniu aplikacji, kliknij pozycję adres **URL** w górnej części aplikacji karta w portalu Azure. 

6. Aby potwierdzić, że ciągły wdrożenia występuje z repozytorium wyboru, należy przekazać zmiany do repozytorium. Aby odzwierciedlić zmiany krótko po zakończeniu wypychanych do repozytorium należy zaktualizować aplikacji. Można sprawdzić, czy występują pobierane w aktualizacji w karta **Opcje wdrażania** aplikacji.

## <a name="VSsolution"></a>Ciągły wdrożenie rozwiązania programu Visual Studio 

Naciśnięcie rozwiązania programu Visual Studio do Azure aplikacji usługi jest tak proste, jak naciśnięcie pliku prostego index.html. Proces wdrażania aplikacji usługi usprawnia wszystkie szczegóły, w tym Przywracanie NuGet zależności i tworzenie plików binarnych aplikacji. Źródło formantu z najważniejszymi wskazówkami utrzymania kod tylko w repozytorium cyfra i umożliwić przejmują pozostałych wdrażania aplikacji usługi.

Procedura naciśnięcie tego rozwiązania programu Visual Studio do aplikacji usługi są takie same jak w [poprzedniej sekcji](#overview), pod warunkiem, że warto skonfigurować rozwiązanie i repozytorium usługi w następujący sposób:

-   Za pomocą opcji sterowania źródła programu Visual Studio do generowania `.gitignore` plików, takie jak obraz poniżej lub ręcznie dodać `.gitignore` pliku w repozytorium główną zawartością podobne do [.gitignore próbki](https://github.com/github/gitignore/blob/master/VisualStudio.gitignore). 

    ![](./media/app-service-continuous-deployment/VS_source_control.png)
 
-   Dodawanie drzewa katalogów całego rozwiązanie do repozytorium, za pomocą pliku .sln w katalogu głównym repozytorium.

Masz Konfigurowanie repozytorium zgodnie z opisem, i skonfigurowano aplikacji platformy Azure ciągły publikowania z jednej z online repozytoria cyfra, możesz tworzenia aplikacji programu ASP.NET lokalnie w programie Visual Studio i stale wdrażanie kodu po prostu przez naciśnięcie zmiany do repozytorium cyfra online.

## <a name="disableCD"></a>Wyłączanie ciągły wdrażania

Aby wyłączyć ciągły wdrażania 

1. W swojej aplikacji menu karta w [Azure portal], kliknij przycisk **WDRAŻANIEM aplikacji > Opcje wdrażania**. Następnie kliknij przycisk **Rozłącz** w karta **Opcje wdrażania** .

    ![](./media/app-service-continuous-deployment/cd_disconnect.png)    

2. Po odebraniu, **Tak** Aby komunikat potwierdzający, można powrócić do karta aplikacji sieci i kliknij przycisk **WDRAŻANIEM aplikacji > Opcje wdrażania** Jeśli chcesz skonfigurować publikowanie z innego źródła.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Jak badanie typowych problemów z ciągłym wdrażania](https://github.com/projectkudu/kudu/wiki/Investigating-continuous-deployment)
* [Jak za pomocą programu PowerShell dla Azure]
* [Jak za pomocą narzędzia wiersza polecenia Azure dla komputerów Mac i Linux]
* [Dokumentacja cyfra]
* [Kudu projektu](https://github.com/projectkudu/kudu/wiki)

>[AZURE.NOTE] Jeśli chcesz rozpocząć pracę z Azure aplikacji usługi przed utworzeniem konta dla konta Azure, przejdź do [Spróbuj aplikacji usługi](http://go.microsoft.com/fwlink/?LinkId=523751), którym natychmiast można utworzyć aplikację sieci web krótkotrwałe starter w aplikacji usługi. Nie kart kredytowych wymagane; nie zobowiązania.

[Azure aplikacji usługi]: https://azure.microsoft.com/en-us/documentation/articles/app-service-changes-existing-services/ 
[Azure portal]: https://portal.azure.com
[VSTS Portal]: https://www.visualstudio.com/en-us/products/visual-studio-team-services-vs.aspx
[Installing Git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[Jak za pomocą programu PowerShell dla Azure]: ../articles/powershell-install-configure.md
[Jak za pomocą narzędzia wiersza polecenia Azure dla komputerów Mac i Linux]: ../articles/xplat-cli-install.md
[Dokumentacja cyfra]: http://git-scm.com/documentation

[Tworzenie repo (GitHub)]: https://help.github.com/articles/create-a-repo
[Tworzenie repo (BitBucket)]: https://confluence.atlassian.com/display/BITBUCKET/Create+an+Account+and+a+Git+Repo
[Wprowadzenie do programu VSTS]: https://www.visualstudio.com/get-started/overview-of-get-started-tasks-vs
[Continuous delivery to Azure using Visual Studio Team Services]: ../articles/cloud-services/cloud-services-continuous-delivery-use-vso.md
