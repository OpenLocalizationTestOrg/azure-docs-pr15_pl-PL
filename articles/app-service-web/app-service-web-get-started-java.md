<properties 
    pageTitle="Wdrażanie pierwszego aplikacji sieci web Java Azure w pięć minut | Microsoft Azure" 
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
    
# <a name="deploy-your-first-java-web-app-to-azure-in-five-minutes"></a>Wdrażanie pierwszego aplikacji sieci web Java Azure w pięć minut

Ten samouczek ułatwia wdrażanie aplikacji sieci web dla prostych Java [Azure aplikacji usługi](../app-service/app-service-value-prop-what-is.md).
Za pomocą usługi aplikacji do tworzenia aplikacji sieci web, [aplikacji dla urządzeń przenośnych kopii zostanie zakończone](/documentation/learning-paths/appservice-mobileapps/)i [aplikacje interfejsu API](../app-service-api/app-service-api-apps-why-best-platform.md).

Konieczne będzie: 

- Tworzenie aplikacji sieci web w usłudze Azure aplikacji.
- Wdrażanie aplikacji Java próbki.
- Zobacz kodzie systemem live produkcji.

## <a name="prerequisites"></a>Wymagania wstępne

- Uzyskaj klienta FTP-FTPS, takich jak [FileZilla](https://filezilla-project.org/).
- Uzyskiwanie konta Microsoft Azure. Jeśli nie masz konta, możesz [Utwórz konto w bezpłatnej wersji próbnej](/pricing/free-trial/?WT.mc_id=A261C142F) lub [aktywowania programu Visual Studio subskrybentów korzyści](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

>[AZURE.NOTE] Możesz [Wypróbować aplikacji usługi](http://go.microsoft.com/fwlink/?LinkId=523751) bez Azure konta. Tworzenie aplikacji starter i odtwarzanie na temat do godziny — karta kredytowa nie wymagane, nie zobowiązań.

<a name="create"></a>
## <a name="create-a-web-app"></a>Tworzenie aplikacji sieci web

1. Zaloguj się do [portalu Azure](https://portal.azure.com) za pomocą konta usługi Azure.

2. Z menu po lewej stronie kliknij pozycję **Nowy** > **Web + Mobile** > **Aplikacji sieci Web**.

    ![](./media/app-service-web-get-started-languages/create-web-app-portal.png)

3. W aplikacji karta tworzenie Użyj poniższych ustawień dla nowej aplikacji:

    - **Nazwa aplikacji**: wpisz unikatową nazwę.
    - **Grupa zasobów**: wybierz pozycję **Utwórz nowe** i nadaj nazwę grupie zasobów.
    - **Plan Lokalizacja usługi aplikacji**: kliknij, aby skonfigurować, a następnie kliknij przycisk **Utwórz nowy** ustawić nazwę, lokalizację i cennik warstwa plan usług aplikacji. Zachęcamy do używanie **Free** ceny warstwy.

    Gdy skończysz, karta Tworzenie usługi aplikacji powinna wyglądać następująco:

    ![](./media/app-service-web-get-started-languages/create-web-app-settings.png)

3. Kliknij przycisk **Utwórz** u dołu. Kliknięcie ikony **powiadomień** u góry ekranu, aby wyświetlić postęp.

    ![](./media/app-service-web-get-started-languages/create-web-app-started.png)

4. Po zakończeniu wdrożenia powinien zostać wyświetlony ten komunikat powiadomienia. Kliknij wiadomość, aby otworzyć z wdrożeniem karta.

    ![](./media/app-service-web-get-started-languages/create-web-app-finished.png)

5. W karta **pomyślnie rozmieszczania** kliknij łącze **zasobów** , aby otworzyć karta z nowej aplikacji sieci web.

    ![](./media/app-service-web-get-started-languages/create-web-app-resource.png)

## <a name="deploy-a-java-app-to-your-web-app"></a>Wdrażanie aplikacji Java do aplikacji sieci web

Teraz Przyjrzyjmy wdrażanie aplikacji Java Azure za pomocą FTPS.

5. Karta aplikacji sieci web przewiń do pozycji **Ustawienia aplikacji** lub wyszukaj go, a następnie kliknij go. 

    ![](./media/app-service-web-get-started-languages/set-java-application-settings.png)

6. W **wersji języka Java**zaznacz **Java 8** , a następnie kliknij przycisk **Zapisz**.

    ![](./media/app-service-web-get-started-languages/set-java-application-settings.png)

    Gdy pojawi się powiadomienie **zaktualizowana ustawień aplikacji sieci web**, przejdź do http://*&lt;argumentu >*. azurewebsites.net, aby wyświetlić servlet JSP domyślne w działaniu.

7. Po powrocie do karta aplikacji sieci web przewiń w dół do **wdrożenia poświadczenia** lub wyszukaj go, a następnie kliknij go.

8. Ustaw poświadczenia, wdrażania i kliknij przycisk **Zapisz**.

7. Po powrocie do karta aplikacji sieci web kliknij przycisk **Podgląd**. Obok pozycji **FTP i wdrażania nazwy użytkownika** i **Nazwa hosta FTPS**kliknij przycisk **Kopiuj** , aby skopiować te wartości.

    ![](./media/app-service-web-get-started-languages/get-ftp-url.png)

    Teraz możesz przystąpić do wdrażania aplikacji Java z FTPS.

8. W kliencie FTP-FTPS Zaloguj się do aplikacji sieci web Azure serwera FTP przy użyciu wartości, który został skopiowany w ostatnim kroku. Przy użyciu hasła wdrożenia, który został utworzony wcześniej.

    Następujące zrzucie ekranu pokazano, logowanie się przy użyciu FileZilla.

    ![](./media/app-service-web-get-started-languages/filezilla-login.png)

    Może zostać wyświetlony ostrzeżeń dotyczących zabezpieczeń nierozpoznane certyfikatu SSL z platformy Azure. Zrealizuj i Kontynuuj.

9. Kliknij [to łącze,](https://github.com/Azure-Samples/app-service-web-java-get-started/raw/master/webapps/ROOT.war) Aby pobrać plik też na komputerze lokalnym.

9. W Twoim kliencie FTP-FTPS przejdź do **/site/wwwroot/webapps** zdalnej witryny i przeciągnij pobrany plik też na komputerze lokalnym do tego katalogu zdalnego.

    ![](./media/app-service-web-get-started-languages/transfer-war-file.png)

    Kliknij **przycisk OK** , aby zastąpić plik Azure.

    >[AZURE.NOTE] Zgodnie z jego Tomcat domyślne zachowanie filename **ROOT.war** w /site/wwwroot/webapps umożliwia głównej aplikacji sieci web (http://*&lt;argumentu >*. azurewebsites.net) i nazwę pliku ** * &lt;anyname >*.war** umożliwia aplikacji sieci web nazwanych (http://*&lt;argumentu >*.azurewebsites.net/*&lt;anyname >*).

To wszystko! Aplikacji Java działa teraz live platformy Azure. W przeglądarce przejdź do http://*&lt;argumentu >*. azurewebsites.net, aby ją wyświetlić w akcji. 

## <a name="make-updates-to-your-app"></a>Wprowadź aktualizacje aplikacji

Zawsze, gdy chcesz wprowadzić aktualizację, po prostu Przekaż nowy plik też do tego samego katalogu zdalnego z klienta FTP-FTPS.

## <a name="next-steps"></a>Następne kroki

[Tworzenie aplikacji sieci web Java przy użyciu szablonu w Azure Marketplace](web-sites-java-get-started.md#marketplace). Możesz uzyskać własnych w pełni dostosować kontenera Tomcat i uzyskać znany interfejs użytkownika menedżera. 

Debugowanie aplikacji sieci Azure web, bezpośrednio w [IntelliJ](app-service-web-debug-java-web-app-in-intellij.md) lub [Zaćmienie](app-service-web-debug-java-web-app-in-eclipse.md).

Lub więcej możliwości korzystania z pierwszej aplikacji sieci web. Na przykład:

- Wypróbuj [inne sposoby wdrażanie kodu Azure](../app-service-web/web-sites-deploy.md). 
- Trudniejsze zagadnienia Azure aplikacji. Uwierzytelnianie użytkownika. Skala on oparty na żądanie. Konfigurowanie niektóre alerty wydajności. Wszystkie za pomocą kilku kliknięć. Zobacz [Dodawanie funkcji do pierwszej aplikacji sieci web](app-service-web-get-started-2.md).

