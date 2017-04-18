<properties
    pageTitle="Jak utworzyć i wdrażanie usługi w chmurze | Microsoft Azure"
    description="Dowiedz się, jak tworzyć i wdrażać usługi w chmurze za pomocą portalu Azure."
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
    ms.date="10/11/2016"
    ms.author="adegeo"/>




# <a name="how-to-create-and-deploy-a-cloud-service"></a>Jak utworzyć i wdrażanie usługi w chmurze

> [AZURE.SELECTOR]
- [Azure portal](cloud-services-how-to-create-deploy-portal.md)
- [Portal Azure klasyczny](cloud-services-how-to-create-deploy.md)

Azure portal udostępnia dwa sposoby umożliwiają tworzenie i wdrażanie usługi w chmurze: *Szybkie tworzenie* i *Utwórz niestandardowe*.

W tym artykule wyjaśniono, jak metoda szybkiego tworzenia umożliwia tworzenie nowej usługi w chmurze, a następnie użyj **Przekaż** , aby przekazać i wdrażanie pakietu usługi cloud platformy Azure. Korzystając z tej metody, Azure portal sprawia, że dostępne wygodne łącza do wykonania wszystkich wymagań na bieżąco. Jeśli możesz już przystąpić do wdrożenia usługi w chmurze, po jego utworzeniu, należy zarówno w tym samym czasie za pomocą tworzyć niestandardowe.

> [AZURE.NOTE] Jeśli zamierzasz publikowania usługi cloud z programu Visual Studio zespołu usługi (VSTS) za pomocą szybkiego tworzenia, a następnie skonfiguruj publikowania programu VSTS z Azure Szybki Start lub pulpitu nawigacyjnego. Aby uzyskać więcej informacji, zobacz temat [Delivery ciągły Azure przez przy użyciu programu Visual Studio Team Services][TFSTutorialForCloudService], lub zobacz Pomoc dotyczącą strony **Szybki Start** .

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

- Jeśli chcesz wdrożyć usługi w chmurze używający Secure Sockets Layer (SSL) do szyfrowania danych, [skonfigurować aplikację](cloud-services-configure-ssl-certificate-portal.md#modify) dla protokołu SSL.

- Jeśli chcesz skonfigurować połączenie pulpitu zdalnego do roli wystąpienia, [Konfigurowanie ról](cloud-services-role-enable-remote-desktop.md) dla pulpitu zdalnego. To jest możliwe tylko w portalu klasycznego.

- Jeśli chcesz skonfigurować pełne, monitorowanie usługi w chmurze, Włącz diagnostyki Azure dla usług w chmurze. *Monitorowanie minimalnego* (ustawienie domyślne monitorowania poziomu) używa liczniki wydajności zebrane z hosta systemów operacyjnych dla roli wystąpień (maszyn wirtualnych). *Monitorowanie pełne* zbiera dodatkowe metryki na podstawie danych wydajności w wystąpienia roli, aby umożliwić dokładniejsze analizę problemów występujących podczas przetwarzania aplikacji. Aby dowiedzieć się, jak włączyć diagnostyki Azure, zobacz [Włączanie diagnostyki platformy Azure](cloud-services-dotnet-diagnostics.md).

Aby utworzyć usługi w chmurze z wdrożenia programu role w sieci web lub role pracownika, należy [utworzyć pakiet usługi](cloud-services-model-and-package.md#servicepackagecspkg).

## <a name="before-you-begin"></a>Przed rozpoczęciem

- Jeśli jeszcze nie zainstalowano Azure SDK, kliknij pozycję **Zainstaluj SDK Azure** do otwierania [strony pobierania Azure](https://azure.microsoft.com/downloads/), a następnie Pobierz SDK dla języka, w którym chcesz opracowanie kodu. (Będziesz mieć możliwość to zrobić później).

- Jeśli wszystkie wystąpienia roli wymagają certyfikatu, Utwórz certyfikaty. Usług w chmurze wymagają plik pfx z kluczem prywatnym. [Możesz przekazać certyfikaty Azure]() podczas tworzenie i wdrażanie usług w chmurze.

- Jeśli planujesz wdrożenie usługi w chmurze do grupy koligacji, Utwórz grupę koligacji. Grupa koligacji umożliwia wdrażanie usługi w chmurze i innych usług Azure w tej samej lokalizacji w regionie. W obszarze **sieci** Azure klasyczny portalu na stronie **grupy koligacji** , możesz utworzyć grupę koligacji.


## <a name="create-and-deploy"></a>Tworzenie i wdrażanie

1. Zaloguj się do [portalu Azure](https://portal.azure.com/).
2. Kliknij pozycję **Nowy > maszyn wirtualnych**, a następnie przewiń w dół i kliknij pozycję **Usługa w chmurze**.

    ![Publikowanie usługi w chmurze](media/cloud-services-how-to-create-deploy-portal/create-cloud-service.png)

3. U dołu strony informacje, które są wyświetlane kliknij przycisk **Utwórz**. 
4. W nowej karta **Usługi w chmurze** wprowadź wartość dla **nazwy DNS**.
5. Tworzenie nowej **Grupy zasobów** , lub wybierz istniejący.
6. Wybierz **lokalizację**.
7. Kliknij **pakiet**. Spowoduje to otwarcie karta **przekazywanie pakietu** . Wypełnij wymagane pola.  

     Jeśli dowolny z poszczególnych ról zawierają jedno wystąpienie, upewnij się, że wybrano **Wdrażanie, nawet jeśli jedną lub więcej ról zawierają jedno wystąpienie** .

8. Upewnij się, że jest zaznaczona opcja **rozpoczęcia wdrożenia** .
9. Kliknij przycisk **OK** , co spowoduje zamknięcie karta **przekazywanie pakietu** .
10. Jeśli nie masz wszystkie certyfikaty, aby dodać, kliknij przycisk **Utwórz**.

    ![Publikowanie usługi w chmurze](media/cloud-services-how-to-create-deploy-portal/select-package.png)

## <a name="upload-a-certificate"></a>Przekazywanie certyfikatu

Jeśli pakietu wdrażania została [skonfigurowana do korzystania z certyfikatów](cloud-services-configure-ssl-certificate-portal.md#modify), możesz teraz przekazać certyfikat.

1. Zaznacz **Certyfikaty**i na karta **certyfikatów Dodaj** wybierz plik PFX certyfikat SSL, a następnie przekaż **hasła** dla certyfikatu
2. Kliknij pozycję **Dołącz certyfikat**, a następnie kliknij **przycisk OK** na karta **certyfikatów Dodaj** .
3. Kliknij przycisk **Utwórz** na karta **Usługi w chmurze** . Podczas rozmieszczania ze stanem **gotowa** , można przystąpić do następnych kroków.

    ![Publikowanie usługi w chmurze](media/cloud-services-how-to-create-deploy-portal/attach-cert.png)


## <a name="verify-your-deployment-completed-successfully"></a>Sprawdź wdrożenia ukończona pomyślnie

1. Kliknij wystąpienie usługi w chmurze.

    Stan należy Pokaż, że usługa jest **uruchomiony**.

2. W obszarze **podstawowe**kliknij **Adres URL witryny** , aby otworzyć usługi w chmurze w przeglądarce sieci web.

    ![CloudServices_QuickGlance](./media/cloud-services-how-to-create-deploy-portal/running.png)


[TFSTutorialForCloudService]: http://go.microsoft.com/fwlink/?LinkID=251796

## <a name="next-steps"></a>Następne kroki

* [Ogólne Konfiguracja usługi w chmurze](cloud-services-how-to-configure-portal.md).
* Skonfiguruj [niestandardową nazwę domeny](cloud-services-custom-domain-name-portal.md).
* [Zarządzanie usługi w chmurze](cloud-services-how-to-manage-portal.md).
* Konfigurowanie [certyfikatów ssl](cloud-services-configure-ssl-certificate-portal.md).