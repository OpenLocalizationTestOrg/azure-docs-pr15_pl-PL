<properties
    pageTitle="Jak utworzyć i wdrażanie usługi w chmurze | Microsoft Azure"
    description="Dowiedz się, jak tworzyć i wdrażać usługi w chmurze przy użyciu metody szybkie tworzenie platformy Azure."
    services="cloud-services"
    documentationCenter=""
    authors="Thraka"
    manager="timlt"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="adegeo"/>




# <a name="how-to-create-and-deploy-a-cloud-service"></a>Jak utworzyć i wdrażanie usługi w chmurze

> [AZURE.SELECTOR]
- [Azure portal](cloud-services-how-to-create-deploy-portal.md)
- [Portal Azure klasyczny](cloud-services-how-to-create-deploy.md)

Portalu klasycznym Azure udostępnia dwa sposoby umożliwiają tworzenie i wdrażanie usługi w chmurze: **Szybkie tworzenie** i **Utwórz niestandardowe**.

W tym temacie wyjaśniono, jak metoda szybkiego tworzenia umożliwia tworzenie nowej usługi w chmurze, a następnie użyj **Przekaż** , aby przekazać i wdrażanie pakietu usługi cloud platformy Azure. Korzystając z tej metody, portalu klasyczny Azure sprawia, że dostępne wygodne łącza do wykonania wszystkich wymagań na bieżąco. Jeśli możesz już przystąpić do wdrożenia usługi w chmurze, po jego utworzeniu, należy zarówno w tym samym czasie za pomocą **Tworzyć niestandardowe**.

> [AZURE.NOTE] Jeśli planujesz publikowania usługi cloud z programu Visual Studio Team usług (VSTS), za pomocą szybkiego tworzenia, a następnie skonfiguruj publikowania programu VSTS z **Szybki Start** lub pulpitu nawigacyjnego. Aby uzyskać więcej informacji, zobacz temat [Delivery ciągły Azure przez przy użyciu programu Visual Studio Team Services][TFSTutorialForCloudService], lub zobacz Pomoc dotyczącą strony **Szybki Start** .

## <a name="concepts"></a>Pojęcia
Aby wdrożyć aplikację jako usługi w chmurze platformy Azure są wymagane trzy elementy:

- **Definicji usługi**  
  Plik definicji usługi cloud (.csdef) określa modelu usługi, w tym liczbę ról.

- **Konfiguracja usługi**  
  Plik konfiguracyjny usługi cloud (.cscfg) zawiera ustawienia konfiguracji dla chmury usługi i poszczególnych ról, w tym liczbę wystąpień roli.

- **Pakiet usług**  
  Pakiet usług (.cspkg) zawiera kod aplikacji i konfiguracji i plik definicji usługi.
  
Dowiedz się więcej o tych i jak utworzyć pakiet [tutaj](cloud-services-model-and-package.md).

## <a name="prepare-your-app"></a>Przygotowywanie aplikacji
Przed wdrożeniem usługi w chmurze, możesz utworzyć pakietu z chmury (.cspkg) w kodzie aplikacji i plik konfiguracyjny usługi cloud (.cscfg). Azure SDK udostępnia narzędzia do przygotowania te pliki wymagane wdrożenia. Możesz zainstalować zestawu SDK ze strony [Pobierania Azure](https://azure.microsoft.com/downloads/) , w języku, w którym chcesz opracowanie kodu aplikacji.

Trzy funkcje usługi w chmurze wymagają specjalnych konfiguracji przed wyeksportowaniem pakietu usługi:

- Jeśli chcesz wdrożyć usługi w chmurze używający Secure Sockets Layer (SSL) do szyfrowania danych, [skonfigurować aplikację](cloud-services-configure-ssl-certificate.md#step-2-modify-the-service-definition-and-configuration-files) dla protokołu SSL.

- Jeśli chcesz skonfigurować połączenie pulpitu zdalnego do roli wystąpienia, [Konfigurowanie ról](cloud-services-role-enable-remote-desktop.md) dla pulpitu zdalnego.

- Jeśli chcesz skonfigurować pełne, monitorowanie usługi w chmurze, Włącz diagnostyki Azure dla usług w chmurze. *Monitorowanie minimalnego* (ustawienie domyślne monitorowania poziomu) używa liczniki wydajności zebrane z hosta systemów operacyjnych dla roli wystąpień (maszyn wirtualnych). "Pełne monitorowania * zbiera dodatkowe metryki na podstawie danych wydajności w wystąpienia roli, aby umożliwić dokładniejsze analizę problemów występujących podczas przetwarzania aplikacji. Aby dowiedzieć się, jak włączyć diagnostyki Azure, zobacz [Włączanie diagnostyki platformy Azure](cloud-services-dotnet-diagnostics.md).

Aby utworzyć usługi w chmurze z wdrożeniach role w sieci web lub role pracownika, należy [utworzyć pakiet usługi](cloud-services-model-and-package.md#servicepackagecspkg).

## <a name="before-you-begin"></a>Przed rozpoczęciem

- Jeśli jeszcze nie zainstalowano Azure SDK, kliknij pozycję **Zainstaluj SDK Azure** do otwierania [strony pobierania Azure](https://azure.microsoft.com/downloads/), a następnie Pobierz SDK dla języka, w którym chcesz opracowanie kodu. (Będziesz mieć możliwość to zrobić później).

- Jeśli wszystkie wystąpienia roli wymagają certyfikatu, Utwórz certyfikaty. Usługami w chmurze wymagają plik pfx z kluczem prywatnym. Możesz [przekazać certyfikaty Azure](cloud-services-configure-ssl-certificate.md#step-3-upload-a-certificate) , jak tworzenie i wdrażanie usług w chmurze.

- Jeśli planujesz wdrożenie usługi w chmurze do grupy koligacji, Utwórz grupę koligacji. Grupa koligacji umożliwia wdrażanie usługi w chmurze i innych usług Azure w tej samej lokalizacji w regionie. W obszarze **sieci** Azure klasyczny portalu na stronie **grupy koligacji** , możesz utworzyć grupę koligacji.


## <a name="how-to-create-a-cloud-service-using-quick-create"></a>Jak: Tworzenie usługi w chmurze za pomocą szybkiego tworzenia

1. W [portalu klasyczny Azure](http://manage.windowsazure.com/), kliknij przycisk **Nowy**>**obliczyć**>**Usługi w chmurze**>**Szybkiego tworzenia**.

    ![CloudServices_QuickCreate](./media/cloud-services-how-to-create-deploy/CloudServices_QuickCreate.png)

2. W **adres URL**wprowadź nazwę poddomeny do użycia w publiczny adres URL umożliwiający dostęp do usługi cloud we wdrożeniach produkcji. Format adresu URL wdrożeniach produkcyjnej jest: http://*myURL*. cloudapp.net.

3. **Regionu lub koligacji grupy**zaznacz regionu geograficznego lub grupy koligacji wdrożenia usługi w chmurze do. Wybierz grupę koligacji, jeśli chcesz wdrożyć usługi w chmurze do tej samej lokalizacji co inne usługi Azure w regionie.

4. Kliknij przycisk **Utwórz usługi w chmurze**.

    ![CloudServices_Region](./media/cloud-services-how-to-create-deploy/CloudServices_Regionlist.png)

    Można monitorować stan procesu w obszarze wiadomości u dołu okna.

    Zostanie otwarty obszar **Usług w chmurze** z nowej usługi w chmurze wyświetlane. Gdy zostanie zmieniony na wartość utworzone, tworzenia usługi w chmurze zakończyła się pomyślnie.

    ![CloudServices_CloudServicesPage](./media/cloud-services-how-to-create-deploy/CloudServices_CloudServicesPage.png)


## <a name="how-to-upload-a-certificate-for-a-cloud-service"></a>Jak: przekazywanie certyfikatu usługi w chmurze

1. W [portalu klasyczny Azure](http://manage.windowsazure.com/)kliknij **Usług w chmurze**, kliknij nazwę usługi w chmurze, a następnie kliknij **Certyfikaty**.

    ![CloudServices_QuickCreate](./media/cloud-services-how-to-create-deploy/CloudServices_EmptyDashboard.png)


2. Kliknij pozycję **Przekaż certyfikatu** lub **przekazać**.

3. W **pliku**użyj **przycisku Przeglądaj** , aby wybrać certyfikat (plik pfx).

4. W polu **hasło**wprowadź klucz prywatny certyfikatu.

5. Kliknij przycisk **OK** (znacznik wyboru).

    ![CloudServices_AddaCertificate](./media/cloud-services-how-to-create-deploy/CloudServices_AddaCertificate.png)

    Możesz obejrzeć postęp przekazywania w obszarze wiadomości, jak pokazano poniżej. Po zakończeniu przekazywania certyfikat zostanie dodany do tabeli. W obszarze wiadomości kliknij przycisk OK, aby zamknąć komunikat.

    ![CloudServices_CertificateProgress](./media/cloud-services-how-to-create-deploy/CloudServices_CertificateProgress.png)

## <a name="how-to-deploy-a-cloud-service"></a>Jak: Wdrażanie usługi w chmurze

1. W [portal Azure klasycznym](http://manage.windowsazure.com/)kliknij **Usług w chmurze**, kliknij nazwę usługi w chmurze, a następnie kliknij **pulpitu nawigacyjnego**.

2. Kliknij pozycję **Przekaż nowy wdrożenia produkcji** lub **przekazać**.

3. **Wdrożenie etykiety**wprowadź nazwę nowego wdrożenie — na przykład MyCloudServicev4.

3. W **pakiecie**wybierz plik pakietu usługi (.cspkg) do użycia przy użyciu **przycisku Przeglądaj** .

4. W **konfiguracji**wybierz plik konfiguracji usługi (.cscfg) do użycia przy użyciu **przycisku Przeglądaj** .

5. Jeśli usługa w chmurze będzie zawierać ról przy użyciu tylko jednego wystąpienia, zaznacz pole wyboru **Wdrażanie, nawet jeśli jedną lub więcej ról zawierają jedno wystąpienie** , aby włączyć do kontynuowania wdrażania.

    Co rola ma co najmniej dwa wystąpienia Azure tylko może zagwarantować procent 99,95 dostęp do usług w chmurze podczas aktualizowania konserwacji i obsługi. W razie potrzeby możesz dodać dodatkowe roli wystąpienia na stronie **skali** po wdrożeniu usługi w chmurze. Aby uzyskać więcej informacji zobacz [Świadczeniu](https://azure.microsoft.com/support/legal/sla/).

6. Kliknij **przycisk OK** (znacznik wyboru), aby rozpocząć wdrażanie usługi w chmurze.

    ![CloudServices_UploadaPackage](./media/cloud-services-how-to-create-deploy/CloudServices_UploadaPackage.png)

    Można monitorować stan wdrożenia w obszarze wiadomości. Kliknij przycisk OK, aby ukryć wiadomości.

    ![CloudServices_UploadProgress](./media/cloud-services-how-to-create-deploy/CloudServices_UploadProgress.png)

## <a name="verify-your-deployment-completed-successfully"></a>Sprawdź wdrożenia ukończona pomyślnie

1. Kliknij pozycję **pulpit nawigacyjny**.

    Stan należy Pokaż, że usługa jest **uruchomiony**.

2. W obszarze **Szybkie rzut**kliknij adres URL witryny, aby otworzyć usługi w chmurze w przeglądarce sieci web.

    ![CloudServices_QuickGlance](./media/cloud-services-how-to-create-deploy/CloudServices_QuickGlance.png)


[TFSTutorialForCloudService]: cloud-services-continuous-delivery-use-vso.md
 
## <a name="next-steps"></a>Następne kroki

* [Ogólne Konfiguracja usługi w chmurze](cloud-services-how-to-configure.md).
* Skonfiguruj [niestandardową nazwę domeny](cloud-services-custom-domain-name.md).
* [Zarządzanie usługi w chmurze](cloud-services-how-to-manage.md).
* Konfigurowanie [certyfikatów ssl](cloud-services-configure-ssl-certificate.md).