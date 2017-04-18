<properties
    pageTitle="Używanie bazy danych MySQL jako PaaS stosu Azure | Microsoft Azure"
    description="Opis Szybkie kroki, aby wdrożyć MySQL dostawcy zasobów i podaj MySQL jako usługa Azure stosu."
    services="azure-stack"
    documentationCenter=""
    authors="Dumagar"
    manager="bradleyb"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="dumagar"/>

# <a name="use-mysql-databases-as-paas-on-azure-stack"></a>Używanie bazy danych MySQL jako PaaS stosu Azure

> [AZURE.NOTE] Poniższe informacje dotyczą tylko wdrożeniach TP1 stos Azure.

Można wdrażać dostawcę zasobu MySQL Azure stosu. Po wdrożeniu dostawcy zasobów, możesz utworzyć serwer MySQL i baz danych za pomocą Menedżera zasobów Azure rozmieszczania szablonów, a następnie przekazać bazy danych MySQL jako usługi. Bazy danych MySQL, które występują w witrynach sieci web, obsługuje wiele platform witryny sieci Web. Po wdrożeniu dostawcy zasobów, możesz utworzyć WordPress witryn sieci Web z platformy Azure aplikacje sieci Web jako dodatek service (PaaS) Azure stosu.

## <a name="quick-steps-to-deploy-the-resource-provider"></a>Szybkie kroki, aby wdrożyć dostawcy zasobów
Wykonaj poniższe kroki, jeśli już znasz stos Azure. Jeśli potrzebujesz dodatkowych informacji, skorzystaj z łączy w każdej sekcji lub przejść bezpośrednio do [rozmieszczanie karta dostawcy zasobów bazy danych MySQL na Zapewnić stos Azure](azure-stack-mysql-rp-deploy-long.md).

1.  Upewnij się, że [wykonania kroków wszystkich konfigurowania](azure-stack-mysql-rp-deploy-long.md#set-up-steps-before-you-deploy) przed wdrożeniem dostawcy zasobów:

    - Struktura .NET 3.5 jest już skonfigurowane w obrazie podstawowym Windows Server. (Jeśli pobrany bitów stos Azure po 23 lutego 2016, możesz pominąć ten krok).
    - [Zainstalowano wersję programu PowerShell Azure zgodny z stos Azure](http://aka.ms/azStackPsh).
    - W ustawieniach zabezpieczeń programu Internet Explorer na ClientVM, [Ulepszone zabezpieczenia jest wyłączona w programie Internet Explorer i pliki cookie są włączone](azure-stack-mysql-rp-deploy-long.md#Turn-off-IE-enhanced-security-and-enable-cookies).

2. [Pobierz plik plików binarnych dostawcy zasobów MySQL](http://aka.ms/masmysqlrp) i wyodrębnij ją na ClientVM w stos Azure dowód koncepcji.

3. [Uruchamianie bootstrap.cmd i skryptów](azure-stack-mysql-rp-deploy-long.md#Bootstrap-the-resource-provider-deployment-PowerShell-and-Prepare-for-deployment).

    Zestaw skryptów pogrupowany według dwóch głównych kart zostanie otwarty w PowerShell zintegrowane skrypty środowiska (ISE). Wykonywanie wszystkich załadowanego skryptów w kolejności od lewej do prawej na każdej z kart.

    1. Uruchamianie skryptów na karcie **Przygotuj** od lewej do prawej, aby:

        - Tworzenie certyfikatu z symboli wieloznacznych do zabezpieczania komunikacji między dostawcy zasobów i Azure Menedżera zasobów.
        - Zaakceptuj warunki umowy licencyjnej MySQL i pobieranie plików binarnych MySQL.
        - Przekaż certyfikaty i wszystkie inne artefaktów do konta usługi Azure stos miejsca do magazynowania.
        - Publikowanie pakietów galerii można wdrażać zasobów MySQL za pośrednictwem galerii.

        > [AZURE.IMPORTANT] Jeśli dowolny z skrypty zawieszają się bez widocznego powodu po przesłaniu dzierżawy usługi Azure Active Directory ustawienia zabezpieczeń może blokować DLL, wymaganego do wdrożenia do uruchomienia. Aby rozwiązać ten problem, poszukaj Microsoft.AzureStack.Deployment.Telemetry.Dll w folderze dostawcy zasobów, kliknij prawym przyciskiem myszy go, kliknij polecenie **Właściwości**, a następnie na karcie **Ogólne** zaznacz **Odblokuj** .

    2. Uruchamianie skryptów na karcie **Rozmieszczanie** od lewej do prawej, aby:

        - [Rozmieszczanie maszyny wirtualnej (maszyn wirtualnych)](azure-stack-mysql-rp-deploy-long.md#Deploy-the-MySQLResource-Provider-VM) , który będzie obsługiwać zarówno dostawcy zasobów, MySQL serwerów i baz danych, które będą wystąpienia. Ten skrypt odwołuje się do pliku parametrów JSON konieczna jest aktualizacja z niektórych wartości, przed uruchomieniem skryptu.
        - [Zarejestruj lokalne rekordu DNS](azure-stack-mysql-rp-deploy-long.md#Update-the-local-DNS) mapujące do dostawcy zasobów maszyn wirtualnych.
        - [Zarejestruj się z dostawcą zasobów](azure-stack-mysql-rp-deploy-long.md#Register-the-MySQL-RP-Resource-Provider) z lokalnego Menedżera zasobów Azure.

        > [AZURE.IMPORTANT] Wszystkie skrypty przyjęto założenie, że obraz podstawowy system operacyjny spełnia wymagania wstępne (.NET 3.5 zainstalowany, JavaScript i plików cookie włączone ClientVM i najnowszą wersję programu PowerShell Azure zainstalowany). Jeśli zostanie wyświetlony błędy podczas uruchamiania skryptów, upewnij się, że spełnione wymagania wstępne.

5. Aby [przetestować dostawcy zasobów MySQL](/azure-stack-MySql-rp-deploy-long.md#create-your-first-mysql-database-to=test-your-deployment)wdrożyć bazą danych MySQL z portalu stos Azure. Kliknij przycisk **Utwórz** &gt; **Niestandardowy** &gt; **serwer MySQL i bazy danych**.

Należy to uzyskać dostawcy zasobów MySQL w górę i uruchomione w około 25 minut.
