<properties
    pageTitle="Wdrożenie lokalne cyfra usłudze Azure aplikacji"
    description="Dowiedz się, jak włączyć lokalnego wdrożenia cyfra usłudze Azure w aplikacji."
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
    ms.date="06/13/2016"
    ms.author="dariagrigoriu"/>
    
# <a name="local-git-deployment-to-azure-app-service"></a>Wdrożenie lokalne cyfra usłudze Azure aplikacji

Ten samouczek pokazano, jak wdrożyć aplikację na [Azure aplikacji usługi] z repozytorium cyfra na komputerze lokalnym. Usługa aplikacji obsługuje tej metody z opcją wdrożenia **Lokalnego cyfra** w [Azure Portal].  
Wiele poleceń cyfra opisane w tym artykule są wykonywane automatycznie podczas tworzenia aplikacji usługi aplikacji przy użyciu [interfejsu wiersza polecenia Azure] jako opisane [poniżej](app-service-web-get-started.md).

## <a name="prerequisites"></a>Wymagania wstępne

Aby użyć tego samouczka, należy następująco:

- Cyfra. Możesz pobrać instalacji binarne [tutaj](http://www.git-scm.com/downloads).  
- Podstawowa wiedza cyfra.
- Konto Microsoft Azure. Jeśli nie masz konta, możesz [Utwórz konto w bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial) lub [aktywowania programu Visual Studio subskrybentów korzyści](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details).

>[AZURE.NOTE] Jeśli chcesz rozpocząć pracę z Azure aplikacji usługi przed utworzeniem konta dla konta Azure, przejdź do [Spróbuj aplikacji usługi](http://go.microsoft.com/fwlink/?LinkId=523751), miejsce, w którym możesz od razu utworzyć aplikacji krótkotrwałe starter w aplikacji usługi. Nie kart kredytowych wymagane; nie zobowiązania.  

## <a name="Step1"></a>Krok 1: Tworzenie lokalnego repozytorium

Wykonywanie następujących zadań, aby utworzyć nowe repozytorium cyfra.

1. Uruchom narzędzie wiersza polecenia, takich jak **GitBash** (Windows) lub **urodzinową** (powłoki systemu Unix). W systemie OS X można korzystać z wiersza polecenia przy użyciu aplikacji **Terminal** .

2. Przejdź do katalogu, gdy zawartość do wdrożenia jest umieszczony.

3. Użyj następującego polecenia, aby zainicjować nowe repozytorium cyfra:

        git init

## <a name="Step2"></a>Krok 2: Zatwierdzanie zawartości

Usługa aplikacji obsługuje aplikacje utworzone w różnych językach programowania. 

1. Jeśli repozytorium zawiera już zawartości Pomiń ten punkt i Przenieś wskazywały 2 poniżej. Jeśli repozytorium jeszcze nie zawiera zawartości po prostu wypełnić z plikiem HTML statyczne w następujący sposób: 

    - Przy użyciu edytora tekstu, Utwórz nowy plik o nazwie **index.html** w katalogu głównym repozytorium cyfra
    - Dodaj następujący tekst, jak zawartość index.html plik i zapisz go: *powitalnych cyfra!*
        
2. W wierszu polecenia upewnij się, że jesteś w katalogu głównym repozytorium cyfra. Dodawanie plików do repozytorium użyć następujące polecenie:

        git add -A 

4. Następnie Zatwierdź zmiany w repozytorium przy użyciu następującego polecenia:

        git commit -m "Hello Azure App Service"

## <a name="Step3"></a>Krok 3: Włączanie repozytorium aplikacji aplikacji usługi

Wykonaj poniższe czynności, aby włączyć repozytorium cyfra aplikacji aplikacji usługi.

1. Zaloguj się do [portalu Azure].

2. Karta aplikacji usługi aplikacji kliknij **Ustawienia > Źródło rozmieszczania**. Kliknij pozycję **Wybierz źródło**, a następnie kliknij **Lokalnego repozytorium cyfra**, a następnie kliknij **przycisk OK**.  

    ![Lokalne repozytorium cyfra](./media/app-service-deploy-local-git/local_git_selection.png)

3. Jeśli jest to pierwszy konfigurujesz czas repozytorium platformy Azure trzeba tworzyć dla niej poświadczenia logowania. Użyje ich do zalogowania się do zmiany Azure repozytorium i wypychanych z lokalnego repozytorium cyfra. Karta Twojej aplikacji kliknij **Ustawienia > poświadczenia wdrożenia**, następnie skonfigurować wdrożenie nazwy użytkownika i hasła. Gdy skończysz, kliknij przycisk **Zapisz**.

    ![](./media/app-service-deploy-local-git/deployment_credentials.png)

## <a name="Step4"></a>Krok 4: Wdrażanie projektu

Publikowanie aplikacji usługi aplikacji przy użyciu cyfra lokalnych, wykonaj następujące czynności.

1. W swojej aplikacji karta w Azure Portal, kliknij przycisk **Ustawienia > właściwości** dla **Adresu URL cyfra**.

    ![](./media/app-service-deploy-local-git/git_url.png)

    Zdalne odwołanie do wdrożenia z lokalnego repozytorium jest używany adres **URL cyfra** . Ten adres URL, które będą używane w następującej procedurze.

2. Za pomocą wiersza polecenia, sprawdź, czy w katalogu lokalnym repozytorium cyfra.

3. Używanie `git remote` dodać zdalnego odwołania na liście w **Adresie URL cyfra** z kroku 1. Polecenie będzie wyglądać podobnie do następującej:

        git remote add azure https://<username>@localgitdeployment.scm.azurewebsites.net:443/localgitdeployment.git         
    > [AZURE.NOTE] Polecenia **zdalnego** dodaje nazwanych odwołanie do zdalnego repozytorium. W tym przykładzie tworzy odwołanie o nazwie "azure" repozytorium aplikacji sieci web.

4. Przekazać zawartość do aplikacji usługi przy użyciu nowego **azure** zdalnego została właśnie utworzona.

        git push azure master

    Pojawi się monit o podanie hasła, utworzony wcześniej po zresetowaniu poświadczeń wdrożenia w Azure Portal. Wprowadź hasło (należy zauważyć, że Gitbash nie są wyświetlane gwiazdki do konsoli pisania hasła). 
       
5. Wróć do aplikacji w Azure Portal. Wpis dziennika usługi wypychanych ostatnio powinny być wyświetlane w karta **wdrożenia** . 

    ![](./media/app-service-deploy-local-git/deployment_history.png)

6. Kliknij przycisk **Przeglądaj** w górnej części karta tej aplikacji, aby sprawdzić, czy został wdrożony zawartości. 
    
## <a name="Step5"></a>Rozwiązywanie problemów

Poniżej przedstawiono błędy lub problemy najczęściej występujące podczas korzystania cyfra publikowanie aplikacji usługi aplikacji platformy Azure:

****

**Symptom**: nie można dostępu [siteURL]: nie można nawiązać połączenia z [scmAddress]

**Przyczyna**: ten błąd może wystąpić, jeśli aplikacji nie jest i rozpocząć pracę.

**Rozdzielczość**: Uruchom aplikację w portalu Azure. Wdrożenie cyfra nie będzie działać, o ile nie jest uruchomiona aplikacja. 


****

**Symptom**: nie można rozwiązać hosta "hostname"

**Przyczyna**: ten błąd może wystąpić, jeśli niepoprawny adres wprowadzonej podczas tworzenia zdalnego azure.

**Rozdzielczość**: używanie `git remote -v` polecenie, aby wyświetlić listę wszystkich piloty, wraz z powiązanego adresu URL. Upewnij się, że adres URL "azure" zdalny jest poprawny. W razie potrzeby usuń i ponownie utworzyć ten pilota za pomocą poprawny adres URL.

****

**Symptom**: nie odwołania do wspólnej i nie określono; wykonując nic. Być może należy określić gałąź, takie jak "wzorzec".

**Przyczyna**: ten błąd może wystąpić, jeśli nie określisz oddział podczas wykonywania cyfra push operacji, a nie ustawiono wartość push.default używana przez cyfra.

**Rozdzielczość**: wykonywanie operacji push ponownie, określając gałąź wzorca. Na przykład:

    git push azure master

****

**Symptom**: nie pasuje do żadnego src refspec [branchname].

**Przyczyna**: ten błąd może wystąpić podczas próby push gałąź niż wzorzec Azure zdalnym.

**Rozdzielczość**: wykonywanie operacji push ponownie, określając gałąź wzorca. Na przykład:

    git push azure master

****

**Symptom**: Błąd — zmiany do zdalnego repozytorium, ale aplikacji sieci web nie zaktualizowane.

**Przyczyna**: ten błąd może wystąpić w przypadku wdrażania aplikacji dla Node.js plikiem package.json, określająca dodatkowe wymaganych modułów.

**Rozdzielczość**: dodatkowe wiadomości zawierającej "npm błąd!" powinny być zarejestrowane przed ten błąd, a może zapewnić dodatkowy kontekst o niepowodzeniu. Znane są następujące przyczyny tego błędu, a odpowiadające im "npm błąd!" Komunikat:

* **Plik utworzonym package.json**: npm błąd! Nie można odczytać zależności.

* **Natywne moduł nie ma binarne rozkładu dla systemu Windows**:

    * npm błąd! \`cmd "/ c" "gyp węzeł Odbuduj"\` nie powiodło się od 1

        LUB

    * npm błąd! [modulename@version]preinstalować: \`wprowadź || gmake\`


## <a name="additional-resources"></a>Dodatkowe zasoby

* [Dokumentacja cyfra](http://git-scm.com/documentation)
* [Dokumentacja Kudu programu Project](https://github.com/projectkudu/kudu/wiki)
* [Wdrożenie stałego usłudze Azure aplikacji](app-service-continuous-deployment.md)
* [Jak za pomocą programu PowerShell dla Azure](../powershell-install-configure.md)
* [Jak za pomocą interfejsu wiersza polecenia Azure](../xplat-cli-install.md)

[Azure aplikacji usługi]: https://azure.microsoft.com/documentation/articles/app-service-changes-existing-services/
[Azure Developer Center]: http://www.windowsazure.com/en-us/develop/overview/
[Azure Portal]: https://portal.azure.com
[Git website]: http://git-scm.com
[Installing Git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[Azure interfejsu wiersza polecenia]: https://azure.microsoft.com/en-us/documentation/articles/xplat-cli-azure-resource-manager/

[Using Git with CodePlex]: http://codeplex.codeplex.com/wikipage?title=Using%20Git%20with%20CodePlex&referringTitle=Source%20control%20clients&ProjectName=codeplex
[Quick Start - Mercurial]: http://mercurial.selenic.com/wiki/QuickStart
