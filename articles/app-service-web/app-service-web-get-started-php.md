<properties 
    pageTitle="Wdrażanie pierwszego aplikacji sieci web PHP Azure w pięć minut | Microsoft Azure" 
    description="Dowiedz się, jak łatwo jest uruchamianie aplikacji sieci web w aplikacji usługi wdrażając aplikacji próbki. Rozpocznij szybkie wykonanie rzeczywistego rozwoju a wyniki są od razu." 
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""
/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="10/13/2016" 
    ms.author="cephalin"
/>
    
# <a name="deploy-your-first-php-web-app-to-azure-in-five-minutes"></a>Wdrażanie pierwszego aplikacji sieci web PHP Azure w pięć minut

Ten samouczek ułatwia wdrażanie pierwszej aplikacji sieci web PHP [Azure aplikacji usługi](../app-service/app-service-value-prop-what-is.md).
Za pomocą usługi aplikacji do tworzenia aplikacji sieci web, [aplikacji dla urządzeń przenośnych kopii zostanie zakończone](/documentation/learning-paths/appservice-mobileapps/)i [aplikacje interfejsu API](../app-service-api/app-service-api-apps-why-best-platform.md).

Konieczne będzie: 

- Tworzenie aplikacji sieci web w usłudze Azure aplikacji.
- Wdrażanie kodu PHP próbki.
- Zobacz kodzie systemem live produkcji.
- Aktualizowanie aplikacji sieci web tak samo jak [wypychania zatwierdza cyfra](https://git-scm.com/docs/git-push).

## <a name="prerequisites"></a>Wymagania wstępne

- [Cyfra](http://www.git-scm.com/downloads).
- [Polecenie azure](../xplat-cli-install.md).
- Konto Microsoft Azure. Jeśli nie masz konta, możesz [Utwórz konto w bezpłatnej wersji próbnej](/pricing/free-trial/?WT.mc_id=A261C142F) lub [aktywowania programu Visual Studio subskrybentów korzyści](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

>[AZURE.NOTE] Możesz [Wypróbować aplikacji usługi](http://go.microsoft.com/fwlink/?LinkId=523751) bez Azure konta. Tworzenie aplikacji starter i odtwarzanie na temat do godziny — karta kredytowa nie wymagane, nie zobowiązań.

## <a name="deploy-a-php-web-app"></a>Wdrażanie aplikacji sieci web PHP

1. Otwórz nowy wiersz polecenia systemu Windows, okno programu PowerShell, powłoki Linux lub terminal OS X. Uruchamianie `git --version` i `azure --version` Aby sprawdzić, czy cyfra i polecenie Azure są zainstalowane na komputerze.

    ![Testowanie instalacji narzędzia interfejsu wiersza polecenia dla aplikacji sieci web pierwszego platformy Azure](./media/app-service-web-get-started/1-test-tools.png)

    Jeśli jeszcze nie zainstalowano narzędzia, zobacz [wymagania wstępne](#Prerequisites) dotyczące łącza pobierania.

3. Zaloguj się do Azure w następujący sposób:

        azure login

    Postępuj zgodnie z komunikatem Pomocy, aby kontynuować proces logowania.

    ![Zaloguj się do Azure, aby utworzyć pierwszy aplikacji sieci web](./media/app-service-web-get-started/3-azure-login.png)

4. Zmienianie polecenie Azure w trybie ASM, a następnie ustaw użytkownika do wdrażania usługi aplikacji. Będzie wdrażanie kodu przy użyciu poświadczeń później.

        azure config mode asm
        azure site deployment user set --username <username> --pass <password>

1. Zmienianie do katalogu roboczego (`CD`) i klonowanie aplikacji próbki, tak jak poniżej:

        git clone https://github.com/Azure-Samples/app-service-web-php-get-started.git

2. Zmienianie do repozytorium aplikacji próbki. Na przykład:

        cd app-service-web-php-get-started

4. Tworzenie zasobów aplikacji usługi aplikacji platformy Azure o nazwie unikatowe aplikacji i użytkownika wdrażania, które skonfigurowano wcześniej. Gdy zostanie wyświetlony monit, określ liczbę odpowiedni region.

        azure site create <app_name> --git --gitusername <username>

    ![Tworzenie Azure zasobów dla aplikacji sieci web pierwszego platformy Azure](./media/app-service-web-get-started-languages/php-site-create.png)

    Aplikacji zostanie utworzona w Azure teraz. Ponadto bieżącego katalogu jest zainicjowany cyfra i połączony z nowej aplikacji usługi aplikacji jako cyfra zdalnego.
    Można przejść do adresu URL aplikacji (http://&lt;nazwa_aplikacji >. azurewebsites.net) Zobacz atrakcyjnych domyślnej strony HTML, ale przejdźmy do niej dostać kodzie teraz.

4. Wdrażanie kodu przykładowe Azure aplikacji tak samo push kodu z cyfra. Gdy zostanie wyświetlony monit, użyj hasła, które skonfigurowano wcześniej.

        git push azure master

    ![Kod push aplikacji sieci web pierwszego platformy Azure](./media/app-service-web-get-started-languages/php-git-push.png)

    `git push`nie tylko umieszcza kod w Azure, ale również wyzwalane zadania rozmieszczania w aparatu wdrażania. Można także  [włączyć rozszerzenie Kompozytor](web-sites-php-mysql-deploy-use-git.md#composer) automatyczne przetwarzanie plików composer.json w aplikacji PHP.

Gratulacje, wdrożono aplikacji do usługi aplikacji Azure.

## <a name="see-your-app-running-live"></a>Zobacz aplikacji uruchomiony live

Aby wyświetlić aplikacji uruchomiony live platformy Azure, uruchom następujące polecenie z dowolnego katalogu w repozytorium:

    azure site browse

## <a name="make-updates-to-your-app"></a>Wprowadź aktualizacje aplikacji

Za pomocą cyfra można teraz push z główną projektu (repozytorium) w dowolnym momencie dokonać aktualizacji działającej witryny. Jak ten sam sposób jak po wdrożeniu kodu po raz pierwszy. Na przykład każdorazowo chcesz push nowe zmianę, którą przetestowaniu lokalnie, po prostu uruchom następujące polecenia z główną projektu (repozytorium):

    git add .
    git commit -m "<your_message>"
    git push azure master

## <a name="next-steps"></a>Następne kroki

[Tworzenie, konfigurowanie i wdrażanie aplikacji sieci web Laravel Azure](app-service-web-php-get-started.md). Wykonując tym samouczku dowiesz się, podstawowe umiejętności potrzebne do uruchamiania dowolnej aplikacji sieci web PHP w Azure, takich jak:

- Tworzenie i konfigurowanie aplikacji platformy Azure z programu PowerShell i imprezie.
- Ustawianie wersji PHP.
- Za pomocą pliku start, który nie znajduje się w katalogu głównym aplikacji.
- Włącz Kompozytor automatyzacji.
- Zmienne specyficzne dla środowiska programu Access.
- Rozwiązywanie typowych problemów.

Lub więcej możliwości korzystania z pierwszej aplikacji sieci web. Na przykład:

- Wypróbuj [inne sposoby wdrażanie kodu Azure](../app-service-web/web-sites-deploy.md). Na przykład aby wdrożyć z jednej z repozytoria GitHub, po prostu wybierz **GitHub** zamiast **Lokalnego repozytorium cyfra** w obszarze **Opcje wdrażania**.
- Trudniejsze zagadnienia Azure aplikacji. Uwierzytelnianie użytkownika. Skala on oparty na żądanie. Konfigurowanie niektóre alerty wydajności. Wszystkie za pomocą kilku kliknięć. Zobacz [Dodawanie funkcji do pierwszej aplikacji sieci web](app-service-web-get-started-2.md).

