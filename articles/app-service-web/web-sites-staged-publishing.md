<properties
    pageTitle="Konfigurowanie tymczasowej środowiska dla aplikacji sieci web w usłudze Azure aplikacji"
    description="Dowiedz się, jak używać etapowej publikowania dla aplikacji sieci web w usłudze Azure aplikacji."
    services="app-service"
    documentationCenter=""
    authors="cephalin"
    writer="cephalin"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="03/09/2016"
    ms.author="cephalin"/>

# <a name="set-up-staging-environments-for-web-apps-in-azure-app-service"></a>Konfigurowanie przemieszczenia środowiska dla aplikacji sieci web w usłudze Azure aplikacji
<a name="Overview"></a>

Podczas wdrażania aplikacji sieci web do [Usługi aplikacji](http://go.microsoft.com/fwlink/?LinkId=529714), można wdrażać na przedział osobnych wdrożenia, zamiast przedział produkcji domyślne podczas pracy w trybie plan **Standardowy** lub **Premium dla** aplikacji usługi. Wdrożenie gniazda są aplikacje web faktycznie live przy użyciu własnej nazwy hostów. Elementy zawartości i konfiguracji aplikacji sieci Web może się miejscami między dwa gniazda wdrażania, w tym przedział produkcji. Wdrażanie aplikacji przedział wdrożenia ma następujące zalety:

- Zmiany aplikacji sieci web w tymczasowej przedział wdrażania można sprawdzić przed zamiana go z przedział produkcji.

- Wdrażanie aplikacji sieci web do przedziału najpierw i wstawiany produkcji zapewnia rozgrzane wszystkie wystąpienia przedział przed są zamienione na produkcji. Dzięki temu przestoje podczas wdrażania aplikacji sieci web. Bezproblemowa jest przekierowanie ruchu i żadne żądania są usuwane w wyniku operacji wymiany. Ten całego przepływu pracy można zautomatyzować przez skonfigurowanie [Automatycznego Zamień](#configure-auto-swap-for-your-web-app) podczas sprawdzania poprawności przed wymiany nie jest potrzebna.

- Po wymiany przedział przy użyciu aplikacji web wcześniej etapowej zawiera teraz poprzedniej aplikacji sieci web produkcji. W przypadku zmiany zamienione na przedział produkcji nie zgodnie z oczekiwaniami, można wykonywać samej wymiany aby "ostatniej znanej dobrej witryny" Wstecz.

Poszczególnych trybach plan aplikacji usługi obsługuje wiele różnych czasu na start lub wdrożenia. Aby dowiedzieć się, liczba gniazd obsługuje tryb aplikacji sieci web, zobacz artykuł [Cennik usługi aplikacji](/pricing/details/app-service/).

- Gdy aplikacji sieci web ma wiele gniazda, nie można zmienić sposób.

- Skalowania nie jest dostępna dla gniazd innych niż produkcji.

- Zarządzanie zasobami połączone nie jest obsługiwane dla gniazd innych niż produkcji. W [Azure Portal](http://go.microsoft.com/fwlink/?LinkId=529715) tylko można uniknąć ten potencjalny wpływ na przedział produkcji, przenosząc tymczasowo przedział wartością produkcji do innego trybu plan aplikacji usługi. Należy zauważyć, że przedział produkcji nie musi ponownie udostępnić ten sam tryb przedział produkcji przed możesz zamienić dwa gniazda.


[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

<a name="Add"></a>
## <a name="add-a-deployment-slot-to-a-web-app"></a>Dodawanie przedział wdrażania aplikacji sieci web ##

Musi być zainstalowany aplikacji sieci web w trybie **Standard** lub **Premium** w kolejności, aby włączyć wiele gniazda wdrożenia.

1. Otwórz [Azure Portal](https://portal.azure.com/)karta aplikacji sieci web.
2. Kliknij pozycję **Ustawienia**, a następnie kliknij **gniazda wdrożenia**. Następnie karta **gniazda wdrażania** , kliknij **Dodaj przedział**.

    ![Dodawanie nowego przedział wdrażania][QGAddNewDeploymentSlot]

    > [AZURE.NOTE]
    > Jeśli aplikacji sieci web nie jest już w trybie **Standard** lub **Premium** , otrzymasz komunikat z informacją obsługiwane tryby umożliwiających etapowej publikowania. W tym momencie masz opcji wybierz pozycję **uaktualnienia** i przejdź na kartę **Skala** aplikacji sieci web przed kontynuowaniem.

2. W karta **Dodaj przedział** nazwę przedział i wybierz, czy klonowanie Konfiguracja aplikacji sieci web z innego istniejącego przedział wdrożenia. Kliknij znacznik wyboru, aby kontynuować.

    ![Źródło konfiguracji][ConfigurationSource1]

    Po raz pierwszy Dodawanie przedział, masz tylko dwie opcje: klonowanie konfigurację z przedział domyślne produkcji lub wcale.

    Po utworzeniu kilka gniazd będą mogli klonowanie konfigurację z przedział innych niż w produkcji:

    ![Konfiguracja źródła][MultipleConfigurationSources]

5. Karta **gniazda wdrożenia** kliknij przedział wdrażania, aby otworzyć kartę dla niej, przy użyciu zestawu metryk i konfiguracji, tak jak dowolnej innej aplikacji sieci web. **Your-Web-App-name-Deployment-Slot-name** pojawi się w górnej części karta przypomnienia, że wyświetlasz przedział wdrożenia.

    ![Tytuł przedział wdrażania][StagingTitle]

5. Kliknij pozycję Adres URL aplikacji karta przedział. Zwróć uwagę, przedział wdrożenia ma własny hostname i jest również live aplikacji. Aby ograniczyć dostępie do przedziału wdrażania, zobacz [Aplikacji usługi Web App — blok web access w celu wdrożenia wartością produkcji gniazda](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/).

Po utworzeniu przedział wdrożenia jest bez zawartości. Należy wdrożyć do przedziału z gałęzią inne repozytorium lub zupełnie innego repozytorium. Możesz również zmienić konfigurację portów. Użyj poświadczeń profilu lub wdrożenia Publikuj skojarzony z przedział wdrożenia aktualizacji zawartości.  Na przykład możesz [opublikować ten przedział z cyfra](app-service-deploy-local-git.md).

<a name="AboutConfiguration"></a>
## <a name="configuration-for-deployment-slots"></a>Konfiguracja gniazda wdrażania ##
Gdy klonowanie konfigurację z innego przedział wdrożenia, sklonowanym konfiguracji może być edytowany. Ponadto niektóre elementy konfiguracji są zgodne zawartości przez wymiany (nie przedział określonych) podczas innych elementów konfiguracji pozostaną w tym samym przedział po wymiany (przedział określone). Poniższa lista umożliwia wyświetlenie konfiguracji, która będą się zmieniały Zamień gniazda.

**Ustawienia, które są zamieniane**:

- Ustawienia ogólne — na przykład sockets Web 32-64-bitowa wersja, framework
- Ustawienia aplikacji (może być skonfigurowane do osadzania przedział się)
- Parametry połączenia (może być skonfigurowane do osadzania przedział się)
- Mapowania obsługi
- Ustawienia monitorowania i diagnostyczne
- Zawartość WebJobs

**Ustawienia, które nie są zamieniane**:

- Punkty końcowe publikowania
- Niestandardowe nazwy domen
- Certyfikaty SSL i powiązania
- Ustawienia skali
- Pracownikom WebJobs

Aby skonfigurować aplikację ustawienie lub ciąg połączenia trzymać przedział (nie miejscami), dostęp do karta **Ustawienia aplikacji** dla określonych przedział, a następnie zaznacz pole wyboru **Ustawienie przedział** dla elementów konfiguracji, które powinny pokazywać przedział. Należy zauważyć, że oznaczanie element konfiguracji jako przedział określonych skutkuje ustanowienie tego elementu jako nie zapewniają gniazda wdrożenia skojarzony z aplikacji sieci web.

![Ustawienia przedział][SlotSettings]

<a name="Swap"></a>
## <a name="to-swap-deployment-slots"></a>Aby zamienić gniazda wdrażania ##

>[AZURE.IMPORTANT] Zanim możesz zamienić aplikacji sieci web z przedział wdrożenia do produkcji, upewnij się, że wszystkie ustawienia określone przedział nie skonfigurowano dokładnie tak, jak ma być w celu zamiany.

1. Aby zamienić gniazda wdrażania, kliknij przycisk **Zamień** na pasku poleceń aplikacji sieci web lub na pasku poleceń przedział wdrożenia. Upewnij się, że wymiany źródłowej i docelowej wymiany są prawidłowo. Zazwyczaj docelowej wymiany może być przedział produkcji.  

    ![Przycisk Zamień][SwapButtonBar]

3. Kliknij **przycisk OK** , aby zakończyć operację. Po zakończeniu operacji, zostały zamieniono gniazda wdrożenia.

## <a name="configure-auto-swap-for-your-web-app"></a>Konfigurowanie automatycznego Zamień dla aplikacji sieci web ##

Automatyczne zamienianie usprawnia scenariusze DevOps miejsce, w którym chcesz stale wdrażanie aplikacji sieci web z zero zimnej rozpoczęcia i zero przestoje dla klientów końcowych aplikacji sieci web. Jeśli przedział wdrożenia jest skonfigurowany do automatycznej wymiany do produkcji, każdorazowo push aktualizację kodu do tego przedziału, aplikacji usługi automatycznie Zamień aplikacji sieci web do produkcji po jego ma już rozgrzane do otworu.

>[AZURE.IMPORTANT] Po włączeniu automatycznego Zamień dla przedział, upewnij się, że konfiguracja przedział jest dokładnie konfiguracji przeznaczone dla niej docelowej (zazwyczaj przedział produkcji).

Konfigurowanie automatycznego Zamień przedział jest łatwe. Wykonaj poniższe czynności:

1. W karta **Gniazda wdrażania** Wybierz przedział wartością produkcji kliknij pozycję **Wszystkie ustawienia** dla tego przedział karta.  

    ![][Autoswap1]

2. Kliknij pozycję **Ustawienia aplikacji**. Zaznacz **na** **Automatyczne Zamień**, wybierz przedział wybrany element docelowy **Przedział Zamień automatycznej**i kliknij przycisk **Zapisz** na pasku poleceń. Upewnij się, że konfiguracja dla niej jest dokładnie konfiguracji przeznaczone dla niej docelowej.

    Na karcie **powiadomienia** zostaną flash zielony **SUKCESU** po wykonaniu operacji.

    ![][Autoswap2]

    >[AZURE.NOTE] Aby przetestować Zamień automatycznej dla aplikacji sieci web, można najpierw zaznaczyć przedział docelowej nie produkcji **Przedział Zamień automatyczne** zapoznać się z funkcji.  

3. Wykonywanie naciśnij kodu, aby ten przedział wdrożenia. Automatyczne zamienianie wydarzy po pewnym czasie i Aktualizuj zostaną odzwierciedlone pod adresem URL usługi przedział docelowej.

<a name="Multi-Phase"></a>
## <a name="use-multi-phase-swap-for-your-web-app"></a>Używanie wielu fazy wymiany dla aplikacji sieci web ##

Zamienianie wielu fazy jest dostępna w celu uproszczenia poprawności w kontekście elementy konfiguracji pokazywać przedział, takie jak parametry połączenia. W takich przypadkach może być przydatne do stosowanie takich elementów konfiguracji docelowej wymiany w źródle wymiany i sprawdzania poprawności przed wymiany rzeczywistości działa. Gdy elementy konfiguracji docelowej wymiany są stosowane do źródła wymiany dostępne akcje są wykonywania wymiany lub Przywracanie oryginalnej konfiguracji źródła wymiany, który ma wpływ anulowania transakcji. Próbki do polecenia cmdlet programu PowerShell Azure dostępne dla wielu fazy wymiany są uwzględniane w poleceń cmdlet programu PowerShell Azure sekcji gniazda wdrożenia.

<a name="Rollback"></a>
## <a name="to-rollback-a-production-app-after-swap"></a>Aby ustawić aplikacji dla produkcji po wymiany ##

Jeśli jakiekolwiek błędy, są oznaczone w produkcji po wymiany przedział, wycofać otwory do stanu przed wymiany przez zamiana samym dwa gniazda natychmiast.

<a name="Warm-up"></a>
## <a name="custom-warm-up-before-swap"></a>Niestandardowe rozgrzania przed wymiany ##

Niektóre aplikacje mogą wymagać rozgrzania niestandardowych akcji. Element konfiguracji applicationInitialization w pliku web.config umożliwia określenie niestandardowej inicjowanie akcje do wykonania przed otrzymaniem żądania. Operacja wymiany będzie czekać na tym rozgrzania niestandardowych do wykonania. Poniżej przedstawiono przykładowy fragment web.config.

    <applicationInitialization>
        <add initializationPage="/" hostName="[web app hostname]" />
        <add initializationPage="/Home/About" hostname="[web app hostname]" />
    </applicationInitialization>

<a name="Delete"></a>
## <a name="to-delete-a-deployment-slot"></a>Aby usunąć przedział wdrażania##

W karta dla przedział wdrażania kliknij przycisk **Usuń** na pasku poleceń.  

![Usuwanie przedział wdrażania][DeleteStagingSiteButton]

<!-- ======== AZURE POWERSHELL CMDLETS =========== -->

<a name="PowerShell"></a>
##Azure poleceń cmdlet programu PowerShell dla gniazd wdrażania

Azure PowerShell to modułu, który zawiera polecenia cmdlet, aby zarządzać Azure za pomocą programu Windows PowerShell, w tym obsługę zarządzania gniazda wdrażania aplikacji sieci web w usłudze Azure aplikacji.

- Aby uzyskać informacje o instalowaniu i konfigurowaniu Azure programu PowerShell, a następnie na uwierzytelniania Azure programu PowerShell z subskrypcją usługi Azure zobacz [jak zainstalować i skonfigurować Microsoft Azure programu PowerShell](../powershell-install-configure.md).  

----------

### <a name="create-web-app"></a>Tworzenie aplikacji sieci web

```
New-AzureRmWebApp -ResourceGroupName [resource group name] -Name [web app name] -Location [location] -AppServicePlan [app service plan name]
```

----------

### <a name="create-a-deployment-slot-for-a-web-app"></a>Tworzenie przedział wdrażania aplikacji sieci web

```
New-AzureRmWebAppSlot -ResourceGroupName [resource group name] -Name [web app name] -Slot [deployment slot name] -AppServicePlan [app service plan name]
```

----------

### <a name="initiate-multi-phase-swap-and-apply-target-slot-configuration-to-source-slot"></a>Inicjowanie wymiany wielu fazy i Zastosuj konfiguracji przedział docelowej przedział źródła

```
$ParametersObject = @{targetSlot  = "[slot name – e.g. “production”]"}
Invoke-AzureRmResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [web app name]/[slot name] -Action applySlotConfig -Parameters $ParametersObject -ApiVersion 2015-07-01
```

----------

### <a name="revert-the-first-phase-of-multi-phase-swap-and-restore-source-slot-configuration"></a>Pierwszy etap wielu fazy wymiany zostaną przywrócone i przywracanie konfiguracji przedział źródła

```
Invoke-AzureRmResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [web app name]/[slot name] -Action resetSlotConfig -ApiVersion 2015-07-01
```

----------

### <a name="swap-deployment-slots"></a>Zamiana gniazda wdrażania

```
$ParametersObject = @{targetSlot  = "[slot name – e.g. “production”]"}
Invoke-AzureRmResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [web app name]/[slot name] -Action slotsswap -Parameters $ParametersObject -ApiVersion 2015-07-01
```

----------

### <a name="delete-deployment-slot"></a>Usuwanie przedział wdrażania

```
Remove-AzureRmResource -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots –Name [web app name]/[slot name] -ApiVersion 2015-07-01
```

----------

<!-- ======== Azure CLI =========== -->

<a name="CLI"></a>
##Azure poleceń interfejsu wiersza polecenia (polecenie Azure) dla gniazd wdrażania

Polecenie Azure zawiera polecenia i platform do pracy z Azure, w tym obsługę zarządzania gniazda wdrażania aplikacji sieci Web.

- Aby uzyskać instrukcje dotyczące instalowania i konfigurowania polecenie Azure, w tym informacje dotyczące łączenia polecenie Azure do subskrypcji usługi Azure, zobacz [Instalowanie i konfigurowanie polecenie Azure](../xplat-cli-install.md).

-  Aby wyświetlić listę poleceń dostępnych dla usługi aplikacji Azure w polecenie Azure, zadzwoń do `azure site -h`.

----------
### <a name="azure-site-list"></a>Lista witryn Azure
**Lista witryn azure**, tak jak w poniższym przykładzie połączeń informacji na temat aplikacji web apps w bieżącej subskrypcji.

`azure site list webappslotstest`

----------
### <a name="azure-site-create"></a>Tworzenie witryny Azure
Aby utworzyć przedział wdrożenia, połączenie, **Tworzenie witryny azure** i określ nazwę istniejącej aplikacji sieci web i nazwę przedział, aby utworzyć, tak jak w poniższym przykładzie.

`azure site create webappslotstest --slot staging`

Aby włączyć kontrolę źródła dla nowych przedział, za pomocą opcji **— cyfra** , tak jak w poniższym przykładzie.

`azure site create --git webappslotstest --slot staging`

----------
### <a name="azure-site-swap"></a>zamienianie Azure witryny
Aby wprowadzić wdrażania aktualizacji gniazdo aplikacji produkcji, wykonywanie operacji wymiany, tak jak w poniższym przykładzie za pomocą polecenia **Zamień azure witryny** . Aplikacja produkcji nie będzie działać z dowolną przestoje ani podlegają zimnej start.

`azure site swap webappslotstest`

----------
### <a name="azure-site-delete"></a>Usuwanie witryny Azure
Aby usunąć przedział wdrożenia, który nie jest już potrzebna, za pomocą polecenia **Usuń azure witryny** tak jak w poniższym przykładzie.

`azure site delete webappslotstest --slot staging`

----------

>[AZURE.NOTE] Jeśli chcesz rozpocząć pracę z Azure aplikacji usługi przed utworzeniem konta dla konta Azure, przejdź do [Spróbuj aplikacji usługi](http://go.microsoft.com/fwlink/?LinkId=523751), którym natychmiast można utworzyć aplikację sieci web krótkotrwałe starter w aplikacji usługi. Nie kart kredytowych wymagane; nie zobowiązania.

## <a name="next-steps"></a>Następne kroki ##
[Azure usługi aplikacji Web App — blok web access w celu wdrożenia wartością produkcji gniazda](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/)

[Bezpłatnej wersji próbnej platformy Microsoft Azure](/pricing/free-trial/)

## <a name="whats-changed"></a>Informacje o zmianach
* Przewodnika do zmiany z witryn sieci Web do usługi aplikacji Zobacz: [Usługa Azure aplikacji i jego wpływ na istniejące usługi Azure](http://go.microsoft.com/fwlink/?LinkId=529714)

<!-- IMAGES -->
[QGAddNewDeploymentSlot]:  ./media/web-sites-staged-publishing/QGAddNewDeploymentSlot.png
[AddNewDeploymentSlotDialog]: ./media/web-sites-staged-publishing/AddNewDeploymentSlotDialog.png
[ConfigurationSource1]: ./media/web-sites-staged-publishing/ConfigurationSource1.png
[MultipleConfigurationSources]: ./media/web-sites-staged-publishing/MultipleConfigurationSources.png
[SiteListWithStagedSite]: ./media/web-sites-staged-publishing/SiteListWithStagedSite.png
[StagingTitle]: ./media/web-sites-staged-publishing/StagingTitle.png
[SwapButtonBar]: ./media/web-sites-staged-publishing/SwapButtonBar.png
[SwapConfirmationDialog]:  ./media/web-sites-staged-publishing/SwapConfirmationDialog.png
[DeleteStagingSiteButton]: ./media/web-sites-staged-publishing/DeleteStagingSiteButton.png
[SwapDeploymentsDialog]: ./media/web-sites-staged-publishing/SwapDeploymentsDialog.png
[Autoswap1]: ./media/web-sites-staged-publishing/AutoSwap01.png
[Autoswap2]: ./media/web-sites-staged-publishing/AutoSwap02.png
[SlotSettings]: ./media/web-sites-staged-publishing/SlotSetting.png
 
