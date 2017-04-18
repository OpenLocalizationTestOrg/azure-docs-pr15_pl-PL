<properties
   pageTitle="Przygotowywanie do publikowania i wdrożyć aplikację Azure z programu Visual Studio | Microsoft Azure"
   description="Dowiedz się, procedur, aby skonfigurować chmury i konto usługi magazynu i skonfigurować aplikację Azure."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="prepare-to-publish-or-deploy-an-azure-application-from-visual-studio"></a>Przygotowywanie do publikowania i wdrożyć aplikację Azure z programu Visual Studio

## <a name="overview"></a>Omówienie

Aby można było opublikować projekt usługi cloud, możesz skonfigurować następujące usługi:

- **Usługa w chmurze** , aby uruchomić poszczególnych ról w środowisku Azure

- **Konto miejsca do magazynowania** zapewniająca dostęp do usług obiektów Blob, kolejki i tabeli.

Skorzystaj z poniższych instrukcji konfigurowania tych usług i skonfigurować aplikację


## <a name="create-a-cloud-service"></a>Tworzenie usługi w chmurze

Aby opublikować usługi w chmurze Azure, musisz najpierw utworzyć usługi w chmurze, która działa poszczególnych ról w środowisku Azure. Możesz utworzyć usługi w chmurze w [portal Azure klasyczny](http://go.microsoft.com/fwlink/?LinkID=213885), zgodnie z opisem w sekcji **Tworzenie usługi w chmurze za pomocą portalu klasyczny Azure**, w dalszej części tego tematu. Można także tworzyć w programie Visual Studio usługi w chmurze za pomocą Kreatora publikowania.

### <a name="to-create-a-cloud-service-by-using-visual-studio"></a>Aby utworzyć usługi w chmurze przy użyciu programu Visual Studio

1. Otwórz menu skrótów dla Azure projektu, a następnie wybierz pozycję **Publikuj**.

    ![VST_PublishMenu](./media/vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio/vst-publish-menu.png)

1. Jeśli jeszcze tego nie zrobiono, zaloguj się przy użyciu nazwy użytkownika i hasła dla konta Microsoft lub konto organizacji, który jest skojarzony z subskrypcją usługi Azure.

1. Wybierz przycisk **Dalej** , aby przejść do strony **ustawień** .

    ![Ustawienia wspólne Kreatora publikowania](./media/vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio/publish-settings-page.png)

1. Na liście **Usług w chmurze** wybierz pozycję **Utwórz nowy**. Zostanie wyświetlone okno dialogowe **Tworzenie usługi Azure** .

1. Wprowadź nazwę usługi w chmurze. Nazwa stanowi część adresu URL usługi i dlatego muszą być unikatowe globalnie. Nazwa nie jest uwzględniana wielkość liter.

### <a name="to-create-a-cloud-service-by-using-the-azure-classic-portal"></a>Aby utworzyć usługi w chmurze za pomocą portalu klasyczny Azure

1. Zaloguj się do [portalu klasyczny Azure](http://go.microsoft.com/fwlink/?LinkId=253103) w witrynie sieci Web firmy Microsoft.

1. (opcjonalnie) Aby wyświetlić listę usługami w chmurze, które zostały już utworzone, wybierz link usług w chmurze, w lewej części strony.

1. Wybierz pozycję **+** ikonę w lewym dolnym rogu, a następnie w wyświetlonym menu wybierz pozycję **Usługa w chmurze** . Zostanie wyświetlony ekran innego z dwoma opcjami: **Szybkie tworzenie** i **Utworzyć niestandardowe**. Jeśli wybierzesz **Szybkie tworzenie**, możesz utworzyć usługi w chmurze, po prostu określając jego adres URL i region, w którym będzie fizycznie obsługiwana. Jeśli chcesz samodzielnie **Utworzyć niestandardowe**, możesz od razu opublikować usługi w chmurze, określając pakietu (plik .cspkg), pliku konfiguracji (.cscfg), a certyfikat. Tworzenie niestandardowej nie jest wymagane, jeśli ma zostać opublikowany usługi w chmurze za pomocą polecenia **Publikuj** w projekcie Azure. Polecenie **Publikuj** jest dostępne w menu skrótów dla Azure projektu.

1. Wybierz pozycję **Szybkie tworzenie** później Publikowanie usługi w chmurze przy użyciu programu Visual Studio.

1. Określ nazwę dla usługi w chmurze. Pełny adres URL zostanie wyświetlony obok nazwy.

1. Na liście Wybierz region, w którym większość użytkowników znajdują się.

1. W dolnej części okna wybierz link **Tworzenie usługi w chmurze** .

## <a name="create-a-storage-account"></a>Tworzenie konta miejsca do magazynowania

Konto miejsca do magazynowania zapewnia dostęp do usług obiektów Blob, kolejki i tabeli. Możesz utworzyć konta miejsca do magazynowania przy użyciu programu Visual Studio lub [portal Azure klasyczny](http://go.microsoft.com/fwlink/?LinkId=253103).

### <a name="to-create-a-storage-account-by-using-visual-studio"></a>Aby utworzyć konto miejsca do magazynowania przy użyciu programu Visual Studio

1. W **Eksploratorze rozwiązań**Otwórz menu skrótów dla węzła **miejsca do magazynowania** , a następnie wybierz **Utwórz konto miejsca do magazynowania**.

    ![Utwórz nowe konto Azure miejsca do magazynowania](./media/vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio/IC744166.png)

1. Wybierz lub wprowadź następujące informacje dotyczące nowego konta z miejsca do magazynowania w oknie dialogowym **Utwórz konto miejsca do magazynowania** .
    - Subskrypcja Azure, do którego chcesz dodać konto miejsca do magazynowania.
    - Nazwa, która ma być używany dla nowego konta miejsca do magazynowania.
    - Region lub grupa koligacji (na przykład zachód USA lub Azji Wschodniej).
    - Typ replikacji, którego chcesz użyć dla konta miejsca do magazynowania, takich jak Geo zbędne.

1. Gdy skończysz, wybierz pozycję **Utwórz**. Nowe konto miejsca do magazynowania zostanie wyświetlona na liście **miejsca do magazynowania** w **Eksploratorze serwera**.

### <a name="to-create-a-storage-account-by-using-the-azure-classic-portal"></a>Aby utworzyć konto miejsca do magazynowania przy użyciu portalu klasyczny Azure

1. Zaloguj się do [portalu klasyczny Azure](http://go.microsoft.com/fwlink/?LinkId=253103) w witrynie sieci Web firmy Microsoft.

1. (Opcjonalnie) Aby wyświetlić swoje konta miejsca do magazynowania, wybierz link **miejsca do magazynowania** w panelu po lewej stronie strony.

1. W lewym dolnym rogu strony wybierz **+** ikonę.

1. W wyświetlonym menu wybierz **miejsca do magazynowania**, a następnie wybierz pozycję **Szybkie tworzenie**.

1. Określ nazwę, która spowoduje unikatowy adres url konta miejsca do magazynowania.

1. Nadaj nazwę usługi w chmurze. Pełny adres URL zostanie wyświetlony obok nazwy.

1. Na liście regionów wybierz pozycję region, w którym znajdują się większość użytkowników.

1. Określ, czy chcesz włączyć geo replikacji. Po włączeniu replikacji geo dane zostaną zapisane w wielu miejscach, aby zmniejszyć ryzyko utraty. Ta funkcja umożliwia więcej miejsca do magazynowania, ale można zmniejszyć koszty, umożliwiając geo lokalizacji podczas tworzenia konta miejsca do magazynowania, zamiast dodawać funkcję później. Aby uzyskać więcej informacji zobacz [Geo replikacji](http://go.microsoft.com/fwlink/?LinkId=253108).

1. W dolnej części okna wybierz link **Utwórz konto miejsca do magazynowania** .

Po utworzeniu konta miejsca do magazynowania, zostaną wyświetlone adresy URL, które umożliwiają dostęp do zasobów w każdej z usług Azure magazynu i klawisze dostępu głównego i pomocniczego dla Twojego konta. Te klawisze do uwierzytelnienia wnioski przed usług magazynu.

>[AZURE.NOTE] Klawisz dostępu pomocniczej zawiera taki sam dostęp do swojego konta miejsca do magazynowania jako klucz podstawowy dostęp i jest generowany jako kopii zapasowej należy złamane klucz podstawowy dostęp. Ponadto zaleca się ponownie wygenerować kluczy dostępu na bieżąco. Możesz zmienić ustawienie parametrów połączenia za pomocą pomocniczej klucza podczas generowania klucza podstawowego, a następnie zmodyfikować, aby używać regenerowanej klucza podstawowego podczas generowania klucza pomocniczego.

## <a name="configure-your-app-to-use-services-provided-by-the-storage-account"></a>Konfigurowanie aplikacji do korzystania z usług dostarczony przez konta miejsca do magazynowania

Musisz skonfigurować dowolnej roli, który uzyskuje dostęp do usług magazynu, aby korzystać z usług Azure miejsca do magazynowania, które zostały utworzone. Za pomocą wielu konfiguracji usługi Azure projektu, w tym celu. Domyślnie dwa są tworzone w projekcie Azure. Przy użyciu wielu konfiguracji usługi, można użyć samego ciągu połączenia w kodzie, ale mają różne wartości dla parametrów połączenia w każdej konfiguracji usługi. Na przykład można użyć jednej Konfiguracja usługi Aby uruchomić i debugowanie aplikacji lokalnie, przy użyciu emulatora Azure miejsca do magazynowania i konfigurację innej usługi Publikowanie aplikacji Azure. Aby uzyskać więcej informacji na temat konfiguracji usługi zobacz [Konfigurowanie i Azure projektu przy użyciu wielu usług konfiguracji](vs-azure-tools-multiple-services-project-configurations.md).

### <a name="to-configure-your-application-to-use-services-that-the-storage-account-provides"></a>Aby skonfigurować aplikację do korzystania z usług, które znajdują się na koncie miejsca do magazynowania

1. W programie Visual Studio Otwórz Azure rozwiązania. W Eksploratorze rozwiązań Otwórz menu skrótów dla poszczególnych ról w projekcie Azure uzyskuje dostęp do usług magazynu i wybierz polecenie **Właściwości**. W Edytorze Visual Studio zostanie wyświetlona strona z nazwą roli. Strony są wyświetlane pola na karcie **Konfiguracja** .

1. Na stronach właściwości dla danej roli wybierz pozycję **Ustawienia**.

1. Na liście **Konfiguracja usługi** wybierz nazwę konfiguracji usługi, którą chcesz edytować. Jeśli chcesz wprowadzić zmiany do wszystkich konfiguracje usługi dla tej roli, możesz wybrać **Wszystkich konfiguracji**.  Aby uzyskać więcej informacji na temat aktualizowania konfiguracje usługi zobacz sekcję **Zarządzanie parametry połączenia dla kont miejsca do magazynowania** w temacie [Konfigurowanie ról w usłudze w chmurze Azure z programem Visual Studio](vs-azure-tools-configure-roles-for-cloud-service.md).

1. Aby zmodyfikować wszystkie ustawienia parametry połączenia, wybierz pozycję **...** przycisk obok to ustawienie. Zostanie wyświetlone okno dialogowe **Tworzenie parametrów połączenia miejsca do magazynowania** .

1. W polu **Połącz używając**wybierz opcję **subskrypcji** .

1. Na liście **Subskrypcja** Wybierz subskrypcję. Jeśli na liście subskrypcji nie zawiera, w którym chcesz, wybierz link **Pobierania ustawienia publikowania** .

1. Na liście **Nazwa konta** wybierz nazwę konta magazynu. Narzędzia Azure automatycznie uzyskuje magazynu poświadczeń konta przy użyciu tego pliku .publishsettings. Aby ręcznie określić swoje poświadczenia konta miejsca do magazynowania, wybierz opcję **ręcznie wprowadzić poświadczenia** , a następnie kontynuuj tę procedurę. [Portal Azure klasyczny](http://go.microsoft.com/fwlink/p/?LinkID=213885)uzyskiwać dostęp do nazwę konta magazynu i klucza podstawowego. Jeśli nie chcesz, aby określić magazyn ustawienia konta ręcznie, wybierz przycisk **OK** , aby zamknąć okno dialogowe.

1. Wybierz link **konta miejsca do magazynowania wprowadź** poświadczenia.

1. W polu **Nazwa konta** wpisz nazwę swojego konta miejsca do magazynowania.

    >[AZURE.NOTE] Zaloguj się do [portalu klasyczny Azure](http://go.microsoft.com/fwlink/?LinkID=213885), a następnie wybierz przycisk **miejsca do magazynowania** . Portalu zawiera listę kont miejsca do magazynowania. Jeśli wybierzesz konta, zostanie wyświetlona strona go. Możesz skopiować nazwę konta magazynu z tej strony. Jeśli używasz wcześniejszej wersji programu portalu klasyczny nazwę konta magazynu jest wyświetlany w widoku **Kont miejsca do magazynowania** . Aby skopiować tej nazwy, zaznacz ją w oknie **dialogowym właściwości** widoku, a następnie wybierz klawisze Ctrl + C. Aby wkleić nazwy do programu Visual Studio, wybierz pole tekstowe **Nazwa konta** , a następnie wybierz klawisze Ctrl + V.

1. W polu **klucz konta** wprowadź klucz podstawowy lub skopiuj i wklej go w [portal Azure klasyczny](http://go.microsoft.com/fwlink/?LinkID=213885).
    Aby skopiować ten klucz:

    1. U dołu strony konta odpowiednie miejsca do magazynowania kliknij przycisk **Narzędzia do zarządzania kluczami** .

    1. Na stronie **Zarządzanie klawisze dostępu** zaznacz tekst klucz podstawowy dostęp, a następnie wybierz klawisze Ctrl + C.

    1. W menu Narzędzia Azure Wklej klucz w polu **klucz konta** .

    1. Należy wybrać jedną z następujących opcji, aby określić, jak usługa będzie dostęp do konta miejsca do magazynowania:
        - **Użyj protokołu HTTP**. Jest to standardowy opcja. Na przykład `http://<account name>.blob.core.windows.net`.
        - **Protokół HTTPS Użyj** bezpiecznego połączenia. Na przykład `https://<accountname>.blob.core.windows.net`.
        - **Określanie niestandardowego punkty końcowe** dla każdej z trzech usług. Następnie możesz wpisać te punkty końcowe do pola dla określonej usługi.

        >[AZURE.NOTE] Jeśli tworzysz niestandardowych punktów końcowych, możesz utworzyć bardziej złożone parametry połączenia. Korzystając z tego formatu ciągu, możesz określić punkty końcowe usługi miejsca do magazynowania, zawierające nazwę domeny niestandardowej, zarejestrowany dla Twojego konta miejsca do magazynowania z usługą obiektów Blob. Możesz również przyznać dostęp tylko do zasobów obiektów blob w jeden kontener za pośrednictwem podpisu udostępniania. Aby uzyskać więcej informacji na temat tworzenia niestandardowych punktów końcowych zobacz [Konfigurowanie parametry połączenia miejsca do magazynowania Azure](storage-configure-connection-string.md).

1. Aby zapisać te zmiany parametry połączenia, wybierz przycisk **OK** , a następnie wybierz przycisk **Zapisz** na pasku narzędzi. Po zapisaniu tych zmian, zostanie wyświetlony wartość tego ciągu połączenia w kodzie za pomocą [GetConfigurationSettingValue](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.getconfigurationsettingvalue.aspx). Podczas publikowania aplikacji Azure, wybierz pozycję konfiguracji usługa, która zawiera konto Azure magazynowania parametry połączenia. Po opublikowaniu aplikacji, sprawdź, czy aplikacja działa zgodnie z oczekiwaniami przed usług Azure magazynu

## <a name="next-steps"></a>Następne kroki

Aby dowiedzieć się więcej na temat publikowania aplikacji Azure z programu Visual Studio, zobacz [Publikowanie usługi w chmurze za pomocą narzędzi Azure](vs-azure-tools-publishing-a-cloud-service.md).
