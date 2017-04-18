<properties 
    pageTitle="Etap wdrażanie usługi cloud (Node.js) | Microsoft Azure" 
    description="Dowiedz się, jak wdrażanie aplikacji Azure środowisku wzorcowym, a następnie wdrożyć środowisku produkcyjnym przy użyciu wymiany wirtualnych IP (VIP)." 
    services="cloud-services" 
    documentationCenter="nodejs" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>



# <a name="staging-an-application-in-azure"></a>Przemieszczenia aplikacji platformy Azure

Środowiska wzorcowego platformy Azure sprawdzone przed przeniesieniem środowisku produkcyjnym, w której aplikacja jest dostępna w Internecie wdrożyć aplikację detaliczny. Środowiska wzorcowego jest dokładnie tak, jak środowisku produkcyjnym, ale są dostępne tylko etapowej aplikacji zaciemnionego adres URL, który jest generowany przez Azure. Po upewnieniu się, że aplikacja działa poprawnie, można wdrożony w środowisku produkcyjnym, wykonując wymiany wirtualnych IP (VIP).

> [AZURE.NOTE] Kroki opisane w tym artykule dotyczą tylko aplikacji węzeł znajdującej się jako usługa w chmurze Azure.

## <a name="step-1-stage-an-application"></a>Krok 1: Etapu aplikacji

To zadanie obejmuje sposób umieszczania aplikacji przy użyciu **Programu Microsoft Azure programu PowerShell**.

1.  Publikując usługę, po prostu przekazać **-przedział** parametr polecenia cmdlet **AzureServiceProject Publikuj** .

    ```powershell
    Publish-AzureServiceProject -Slot staging
    ```

2.  Zaloguj się do [portalu klasyczny Azure] i wybierz pozycję **Usług w chmurze**. Po utworzeniu usług w chmurze i stan kolumny **tymczasowego** zostały zaktualizowane w celu **uruchamiania**, kliknij nazwę usługi.

    ![Wyświetlanie uruchomioną usługę portalu][cloud-service]

3.  Wybierz pozycję **pulpit nawigacyjny**, a następnie wybierz **tymczasowego**.

    ![pulpit nawigacyjny usługi w chmurze][cloud-service-dashboard]

4. Zanotuj wartość we wpisie **Adres URL witryny** w prawo. Nazwa DNS jest zaciemnionego identyfikator wewnętrzny, który wygenerował Azure.

    ![adres url witryny][cloud-service-staging-url]

Teraz można sprawdzić, czy aplikacja działa poprawnie w środowisku tymczasowy przy użyciu tymczasowy adres URL witryny.

## <a name="step-2-upgrade-an-application-in-production-by-swapping-vips"></a>Krok 2: Uaktualnianie aplikacji przez służącą potrzeby wymiany

Po zweryfikowaniu uaktualnionej wersji aplikacji w środowisku tymczasowy można szybko udostępnić ją w produkcji przez wymiana wirtualne adresy IP (służącą) środowiskach roboczych i produkcyjnych.

> [AZURE.NOTE] W tym kroku założono, że masz już wdrożyć aplikację do produkcji i umieszczane uaktualnionej wersji aplikacji.

1.  Zaloguj się do [portalu klasyczny Azure], kliknij pozycję **Usług w chmurze** , a następnie wybierz nazwę usługi.

2.  Z **pulpitu nawigacyjnego**wybierz **tymczasowego**, a następnie kliknij **Zamień** w dolnej części strony. Spowoduje to otwarcie okna dialogowego zamienianie VIP.

    ![okno dialogowe Zamień VIP][vip-swap-dialog]

3.  Przejrzyj informacje, a następnie kliknij **przycisk OK**. Dwa wdrożeń rozpocząć aktualizowanie tymczasowy wdrożenia przełączy się do produkcji i wdrożenie produkcji powoduje przejście do tymczasowego.

Pomyślnie umieszczane wdrożeniu i uaktualnione wdrożenia produkcji przez zamiana służącą we wdrożeniu podczas przenoszenia.

## <a name="additional-resources"></a>Dodatkowe zasoby

- [Jak wdrożyć uaktualniania usługi produkcji przez zamiana służącą platformy Azure]

[Portal Azure klasyczny]: http://manage.windowsazure.com
[cloud-service]: ./media/cloud-services-nodejs-stage-application/staging-cloud-service-running.png
[cloud-service-dashboard]: ./media/cloud-services-nodejs-stage-application/cloud-service-dashboard-staging.png
[cloud-service-staging-url]: ./media/cloud-services-nodejs-stage-application/cloud-service-staging-url.png
[vip-swap-dialog]: ./media/cloud-services-nodejs-stage-application/vip-swap-dialog.png
[Jak wdrożyć uaktualniania usługi produkcji przez zamiana służącą platformy Azure]: cloud-services-how-to-manage.md#how-to-swap-deployments-to-promote-a-staged-deployment-to-production
