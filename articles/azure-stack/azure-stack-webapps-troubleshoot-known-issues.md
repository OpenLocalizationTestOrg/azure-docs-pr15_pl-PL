<properties
    pageTitle="Aplikacje na stos Azure — znane problemy i rozwiązywanie problemów z siecią Web | Microsoft Azure"
    description="Szczegółowe wskazówki dotyczące wdrażania aplikacji Web Apps w stos Azure"
    services="azure-stack"
    documentationCenter=""
    authors="apwestgarth"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="app-service"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="anwestg"/>
    
# <a name="web-apps-resource-provider---known-issues-and-troubleshooting"></a>Dostawcy zasobów aplikacji sieci Web — znane problemy i rozwiązywanie problemów

> [AZURE.NOTE] Poniższe informacje dotyczą tylko wdrożeniach TP1 stos Azure.

Jeśli występują problemy podczas wdrażania lub korzystania z aplikacji Web Apps na stos Azure zajrzyj do poniższych wskazówek.

## <a name="azure-stack-web-apps-not-available-in-the-portal"></a>Azure stos sieci Web aplikacji nie są dostępne w portalu

![Tworzenie aplikacji sieci Web Azure stos nowej aplikacji sieci Web][1]

Po zakończeniu [rejestracji dostawcy aplikacji sieci Web stos Azure](azure-stack-webapps-deploy.md#register-the-newly-deployed-azure-stack-web-apps-provider-with-arm) , jeśli nie można znaleźć w sieci Web + karta urządzeń przenośnych a następnie spróbuj wykonać następujące czynności:
* **Wyloguj się z portalem** i **Zamknij przeglądarkę** , a następnie w **Nowe logowanie okna przeglądarki z portalem** i spróbuj ponownie.

Jeśli nadal nie widać sieci web i urządzeń przenośnych karta, spróbuj wykonać następujące czynności:

1.  Zlokalizuj **xRPVM** i **Łączenie** maszyn wirtualnych na **Komputerze obsługującym stos Azure** w **Menedżerze funkcji Hyper-V** .
2.  Otwórz **Wiersz polecenia jako Administrator** i uruchom **polecenie IISRESET**
3.  Wróć do **ClientVM** i otwieranie **nowego okna przeglądarki**, **Zaloguj się do portalu** i spróbuj ponownie.

## <a name="deployment-fails-when-creating-a-web-app-in-a-new-resource-group"></a>Wdrożenie kończy się niepowodzeniem, podczas tworzenia aplikacji sieci Web w nowej grupy zasobów

W pierwszym z sieci Web aplikacji w wersji Preview **Wszystkie aplikacje sieci Web** muszą zostać utworzone w tej samej grupy zasobów jako dostawca zasobów aplikacji sieci Web (**Lokalny AppService**).  Jest to znany problem i będzie można rozwiązać w przyszłości Podgląd.

## <a name="other-issues"></a>Inne problemy

Jeśli występują inne problemy z aplikacji Web Apps na stos Azure Opublikuj [na forum stos Azure] (https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack) miejsce, w którym członkowie nasz zespół monitorowania wpisów.


<!--Image references-->
[1]: ./media/azure-stack-webapps-troubleshoot-known-issues/NewWebandMobile.png



<!--Links-->
[Azure_Stack_App_Service_preview_installer]: http://go.microsoft.com/fwlink/?LinkID=717531
[WebAppsDeployment]: http://go.microsoft.com/fwlink/?LinkId=723982
[AppServiceHelperScripts]: http://go.microsoft.com/fwlink/?LinkId=733525
