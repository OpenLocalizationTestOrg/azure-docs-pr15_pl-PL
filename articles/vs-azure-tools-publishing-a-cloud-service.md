<properties
   pageTitle="Publikowanie usługi w chmurze za pomocą narzędzi Azure | Microsoft Azure"
   description="Informacje na temat sposobu publikowania Azure chmury usługi projektów przy użyciu programu Visual Studio."
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

# <a name="publishing-a-cloud-service-using-the-azure-tools"></a>Publikowanie usługi w chmurze za pomocą narzędzi Azure

Za pomocą narzędzi Azure dla programu Microsoft Visual Studio, można publikować Azure aplikacji bezpośrednio z programu Visual Studio. Program Visual Studio obsługuje zintegrowane publikowania do przemieszczania lub środowisku produkcyjnym: usługi w chmurze.

Aby można było opublikować Azure aplikacji, musisz mieć subskrypcję usługi Azure. Musisz również skonfigurować konto usługi i miejsca do magazynowania chmury może być używany przez aplikację. Możesz skonfigurować te w [portal Azure klasyczny](http://go.microsoft.com/fwlink/?LinkID=213885).

>[AZURE.IMPORTANT] Podczas publikowania, można wybrać środowiska wdrażania dla usługi w chmurze. Musisz również zaznaczyć konto miejsca do magazynowania, który służy do przechowywania pakiet aplikacji do wdrożenia. Po wdrożeniu pakiet aplikacji zostanie usunięta z konta miejsca do magazynowania.

Publikowanie zmian przyrostowo dla poszczególnych ról w sieci web w podczas tworzenia i testowania Azure aplikacji, można użyć wdrażanie sieci Web. Po opublikowaniu aplikacji w środowisku wdrażania wdrażanie sieci Web umożliwia wdrażanie zmiany bezpośrednio do maszyny wirtualnej, która jest uruchomiony ról w sieci web. Nie masz pakowanie i publikowanie całej aplikacji Azure zawsze, który chcesz zaktualizować swojej roli sieci web, aby przetestować zmiany. Tej metody można połączyć zmiany roli sieci web dostępna w chmurze do testowania nie czekając być opublikowany w środowisku wdrażania aplikacji.

Skorzystaj z poniższych instrukcji, aby opublikować Azure aplikacji i zaktualizować ról w sieci web przy użyciu wdrażanie sieci Web:

- Publikowanie lub pakiet aplikacji Azure za pomocą programu Visual Studio

- Aktualizowanie roli sieci web jako część rozwoju i testowania cyklu

## <a name="publish-or-package-an-azure-application-from-visual-studio"></a>Publikowanie lub pakiet aplikacji Azure za pomocą programu Visual Studio

Podczas publikowania Azure aplikacji, możesz wykonać jedną z następujących czynności:

- Tworzenie pakietu usługi: za pomocą tego pakietu i plik konfiguracyjny usługi Publikowanie aplikacji w środowisku wdrażania [Klasyczny Portal Azure](http://go.microsoft.com/fwlink/?LinkID=213885).

- Publikowanie projektu Azure z programu Visual Studio: publikowanie aplikacji bezpośrednio do Azure, można użyć Kreatora publikowania. Aby uzyskać informacje zobacz [Kreatora publikowania aplikacji Azure](vs-azure-tools-publish-azure-application-wizard.md).

### <a name="to-create-a-service-package-from-visual-studio"></a>Aby utworzyć pakiet usługi z programu Visual Studio

1. Gdy zechcesz opublikować aplikacji, otwórz Eksploratora rozwiązanie, otwieranie menu skrótów dla Azure projekt, który zawiera poszczególnych ról i wybierz pozycję Publikuj.

1. Aby utworzyć tylko pakiet usługi, wykonaj następujące kroki:  

  1. W menu skrótów dla Azure projektu wybierz **pakiet**.

  1. W oknie dialogowym **Pakiet Azure aplikacji** wybierz pozycję Konfiguracja usługi, dla której chcesz utworzyć pakiet, a następnie wybierz konfigurację kompilacji.

  1. (opcjonalnie) Aby włączyć pulpitu zdalnego dla usług w chmurze po opublikowaniu, zaznacz pole wyboru **Włącz pulpit zdalny dla wszystkich ról** , a następnie wybierz pozycję **Ustawienia** , aby skonfigurować Pulpit zdalny. Jeśli chcesz debugowania usługi w chmurze, po opublikowaniu, Włącz zdalne debugowanie, wybierając pozycję **Włącz zdalnego debugowania dla wszystkich ról**.

      Aby uzyskać więcej informacji zobacz [Za pomocą pulpitu zdalnego z rolami Azure](vs-azure-tools-remote-desktop-roles.md).

  1. Aby utworzyć pakiet, wybierz link **pakiet** .

      Eksplorator plików pokazuje lokalizację pliku nowo utworzonego pakietu. Możesz skopiować tej lokalizacji, aby można go używać z [portal Azure klasyczny](http://go.microsoft.com/fwlink/?LinkID=213885).

  1. Aby opublikować ten pakiet w środowisku wdrażania, musisz użyć tej lokalizacji jako lokalizacja pakietu podczas tworzenia usługi w chmurze i wdrażanie tego pakietu w środowisku [portal Azure klasyczny](http://go.microsoft.com/fwlink/?LinkID=213885).

1. (Opcjonalnie) Aby anulować proces wdrażania w menu skrótów dla pozycji w dzienniku aktywności wybierz pozycję **Anuluj i Usuń**. To zatrzymanie procesu wdrażania i usuwa środowiska rozmieszczania z Azure.

    >[AZURE.NOTE] Aby usunąć to środowisko rozmieszczania, po jej wdrożeniu, należy użyć [portal Azure klasyczny](http://go.microsoft.com/fwlink/?LinkID=213885).

1. (Opcjonalnie) Po uruchomieniu wystąpienia do roli mają Visual Studio automatycznie wyświetlana środowiska wdrażania w węźle **Usług w chmurze** w Eksploratorze serwera. W tym miejscu możesz wyświetlić stan wystąpienia poszczególnych ról. Zobacz [Zarządzanie Azure zasoby w Eksploratorze chmury](vs-azure-tools-resources-managing-with-cloud-explorer.md). Na poniższej ilustracji przedstawiono wystąpienia roli, gdy są one nadal w stanie inicjowanie:

    ![VST_DeployComputeNode](./media/vs-azure-tools-publishing-a-cloud-service/IC744134.png)

## <a name="update-a-web-role-as-part-of-the-development-and-testing-cycle"></a>Aktualizowanie roli sieci Web jako część rozwoju i testowania cyklu

Jeśli infrastruktura wewnętrznej bazy danych aplikacji sieci jest stabilny, ale ról w sieci web bardziej wymagają częstego aktualizowania, umożliwia wdrażanie Web aktualizowanie roli sieci web w projekcie. Może to być przydatne, gdy nie chcesz odbudować i ponownie wdróż role pracownika wewnętrznej bazy danych lub jeśli masz wiele ról w sieci web i chcesz zaktualizować tylko jedna z ról w sieci web.

### <a name="requirements"></a>Wymagania dotyczące

Poniżej przedstawiono wymagania dotyczące korzystania z sieci Web wdrażanie zaktualizować ról w sieci web:

- **Do projektowania i testowania tylko celów:** Zmiany zostały wprowadzone bezpośrednio do maszyny wirtualnej miejsce, w którym jest uruchomiony ról w sieci web. Jeśli tej maszyny wirtualnej musi być odtwarzane, zmiany zostaną utracone, ponieważ oryginalny pakiet opublikowaną jest używany do odtworzenia maszyny wirtualnej dla roli. Musisz ponownie opublikować aplikację, aby uzyskać najnowsze zmiany ról w sieci web.

- **Można aktualizować tylko role w sieci web:** Nie można zaktualizować ról pracownika. Ponadto nie można zaktualizować RoleEntryPoint w sieci web role.cs.

- **Może obsługiwać tylko jedno wystąpienie ról w sieci web:** Nie możesz mieć wielu wystąpień ról w sieci web w środowisku wdrażania. Jednak wielu sieci web role każdego z tylko jedno wystąpienie są obsługiwane.

- **Należy włączyć połączenia pulpitu zdalnego:** Jest to wymagane, tak aby wdrażanie sieci Web umożliwia użytkownika i hasło łączenie maszyn wirtualnych do wdrożenia zmiany na serwerze z systemem informacji usług internetowych. Ponadto może być konieczne połączyć się z komputerem wirtualnych, aby dodać zaufanego certyfikatu do usług IIS na tym komputerze wirtualnych. (Dzięki temu zdalnego połączenia w programie IIS jest używana przez wdrażanie sieci Web są bezpieczne.)

W poniższej procedurze przyjęto, że korzystasz z kreatora **Publikowania aplikacji Azure** .

### <a name="to-enable-web-deploy-when-you-publish-your-application"></a>Aby włączyć wdrażanie publikowanie aplikacji sieci Web

1. Aby **Włączyć wdrażanie Web** wszystkie pola wyboru ról w sieci web, musisz najpierw skonfigurować połączenia pulpitu zdalnego. Wybierz pozycję **Włącz pulpit zdalny** dla wszystkich ról, a następnie podaj poświadczenia, które będą używane w celu łączenia zdalnie w wyświetlonym oknie **Konfiguracja usług pulpitu zdalnego** . Aby uzyskać więcej informacji, zobacz [Za pomocą pulpitu zdalnego z rolami Azure](vs-azure-tools-remote-desktop-roles.md) .

1. Aby włączyć wdrażanie sieci Web dla wszystkich ról w aplikacji sieci web, wybierz pozycję **Włącz wdrażanie sieci Web dla wszystkich ról w sieci web**.

    Zostanie wyświetlony żółty trójkąt ostrzeżenia. Rozmieszczanie Web używa certyfikatu niezaufanych, podpisem domyślnie, co nie jest zalecane dla przekazywania ważnych danych. Jeśli chcesz zabezpieczyć tę procedurę dla ważnych danych, możesz dodać certyfikat SSL może być używany dla sieci Web Wdrażanie połączeń. Ten certyfikat musi być zaufany certyfikat. Aby dowiedzieć się, jak to zrobić zobacz sekcję **Do wprowadź wdrażanie bezpiecznego sieci Web** w dalszej części tego tematu.

1. Wybierz pozycję **Dalej** , aby wyświetlić ekran **podsumowania** , a następnie wybierz **Publikuj** , aby wdrożyć usługi w chmurze.

    Usługa w chmurze jest opublikowany. Maszyny wirtualnej, która jest tworzona ma zdalnego po włączeniu połączeń dla usług IIS, aby wdrażanie sieci Web można zaktualizować bez ponownego publikowania ich role w sieci web.

    >[AZURE.NOTE] Jeśli masz więcej niż jedno wystąpienie skonfigurowane dla ról w sieci web, pojawi się komunikat ostrzegawczy informujący, że poszczególnych ról w sieci web będzie ograniczona do jednego wystąpienia tylko w pakiecie, utworzonej w celu opublikowania aplikacji. Wybierz **przycisk OK** , aby kontynuować. Jak podano w sekcji wymagań, możesz mieć więcej niż jednej roli sieci web, ale tylko jedno wystąpienie poszczególnych ról.

### <a name="to-update-your-web-role-by-using-web-deploy"></a>Aby zaktualizować ról w sieci Web przy użyciu wdrażanie sieci Web

1. Aby użyć wdrażanie sieci Web, wprowadź kod zmiany projektu do dowolnego z role usługi sieci web w programie Visual Studio, który chcesz opublikować, a następnie kliknij prawym przyciskiem myszy ten węzeł projektu w rozwiązaniu i wskaż polecenie **Publikuj**. Zostanie wyświetlone okno dialogowe **Publikowanie sieci Web** .

1. (Opcjonalnie) Po dodaniu certyfikatu SSL od zaufanych używać dla połączeń zdalnych usług IIS można wyczyść pole wyboru **Zezwalaj na certyfikat niezaufanych** . Aby dowiedzieć się, jak dodać certyfikat, aby lepiej zabezpieczyć wdrażanie sieci Web Zobacz sekcję **Do wprowadź Web wdrażanie bezpiecznego** w dalszej części tego tematu.

1. Aby użyć wdrażanie sieci Web, mechanizmu Publikuj musi nazwę użytkownika i hasło, które można skonfigurować dla połączenia pulpitu zdalnego podczas pierwszej publikacji pakietu.

  1. W polu **Nazwa użytkownika**wprowadź nazwę użytkownika.

  1. W polu **hasło**wprowadź hasło.

  1. (Opcjonalnie) Jeśli chcesz Zapisz to hasło na ten profil, wybierz pozycję **Zapisz hasło**.

1. Aby opublikować zmiany ról w sieci web, wybierz pozycję **Publikuj**.

    Wiersz stanu jest wyświetlana wartość **uruchomiona Publikuj**. Po zakończeniu publikowania **Publikuj zakończyła się pomyślnie** pojawi się. Wprowadzone zmiany zostały wdrożone teraz do roli sieci web na komputerze wirtualnych. Teraz możesz rozpocząć Azure aplikacji w środowisku Azure, aby sprawdzić wprowadzone zmiany.

### <a name="to-make-web-deploy-secure"></a>Aby zabezpieczyć wdrażanie sieci Web

1. Rozmieszczanie Web używa certyfikatu niezaufanych, podpisem domyślnie, co nie jest zalecane dla przekazywania ważnych danych. Jeśli chcesz zabezpieczyć tę procedurę dla ważnych danych, możesz dodać certyfikat SSL może być używany dla sieci Web Wdrażanie połączeń. Ten certyfikat musi być zaufany certyfikat, który można uzyskać od urzędu certyfikacji (CA).

    Aby zabezpieczyć wdrażanie sieci Web dla każdej maszyny wirtualnej dla każdej role w sieci web, musisz je zaufanego certyfikatu, który ma być używany dla sieci web Wdroż [portal Azure klasyczny](http://go.microsoft.com/fwlink/?LinkID=213885). Dzięki temu dodania certyfikatu maszyn wirtualnych, utworzonego dla ról w sieci web podczas publikowania aplikacji.

1. Aby dodać zaufanego certyfikatu SSL do programu IIS, aby używać dla połączeń zdalnych, wykonaj następujące czynności:

  1. Aby połączyć maszyn wirtualnych, na którym działa ról w sieci web, wybierz wystąpienie ról w sieci web w **Chmurze Eksploratora** lub **Eksploratora serwera**, a następnie wybierz polecenie **Połącz z pulpitu zdalnego** . Aby uzyskać szczegółowe instrukcje na temat nawiązywania połączenia między maszyn wirtualnych zobacz [Za pomocą pulpitu zdalnego z rolami Azure](vs-azure-tools-remote-desktop-roles.md).

      Przeglądarka wyświetli monit o pobranie. Plik RDP.

  1. Aby dodać certyfikat SSL, otwórz usługę Zarządzanie w Menedżerze IIS. W Menedżerze IIS włączyć SSL, otwierając łącze **powiązania** w okienku **Akcja** . Zostanie wyświetlone okno dialogowe **Dodawanie wiązanie witryny** . Wybierz przycisk **Dodaj**, a następnie wybierz HTTPS na liście rozwijanej **Typ** . Na liście **certyfikat SSL** wybierz certyfikat SSL, że zostały podpisane przez urząd certyfikacji i zostały przekazane do [portalu klasyczny Azure](http://go.microsoft.com/fwlink/?LinkID=213885). Aby uzyskać więcej informacji zobacz [Konfigurowanie ustawienia połączenia z usługą zarządzania](http://go.microsoft.com/fwlink/?LinkId=215824).

      >[AZURE.NOTE] Jeśli dodasz zaufanego certyfikatu SSL żółtym trójkątnym symbolem ostrzeżenia przestanie być wyświetlane w **Kreatorze publikowania**.

## <a name="include-files-in-the-service-package"></a>Dołączanie plików w pakiecie usługi

Może być konieczne dołączenie określonymi plikami pakietu usługi tak, aby były dostępne na komputerze wirtualnych utworzonego dla roli. Na przykład można dodać .exe lub plik msi, który jest używany przez skrypt uruchamiania pakietu usługi. Można też dodać zestaw, który wymaga projektu sieci web roli lub pracownika roli. Aby dołączyć pliki musi zostać dodana do rozwiązania Azure aplikacji.

### <a name="to-include-files-in-the-service-package"></a>Aby dołączyć pliki w pakiecie usługi

1. Aby dodać zestaw do pakietu usługi, wykonaj następujące czynności:

  1. W **Eksploratorze rozwiązań** Otwórz węzeł projektu dla projektu, który ma zestaw, którego dotyczy odwołanie.

  1. Aby dodać zestaw do projektu, otwórz menu skrótów dla folderu **odwołania** , a następnie wybierz **Dodaj odwołanie**. Zostanie wyświetlone okno dialogowe Dodawanie odwołania.

  1. Wybierz odwołanie, które chcesz dodać, a następnie kliknij przycisk **OK** .

      Odwołanie zostanie dodany do listy w folderze **odwołania** .

  1. Otwieranie menu skrótów dla zestawu dodanego i wybierz polecenie **Właściwości**. Zostanie wyświetlone okno **Właściwości** .

      Aby uwzględnić tego zestawu w pakiecie usługi **kopii lokalnej listy** wybierz **wartość PRAWDA**.

1. W **Eksploratorze rozwiązań** Otwórz węzeł projektu dla projektu, który ma zestaw, którego dotyczy odwołanie.

1. Aby dodać zestaw do projektu, otwórz menu skrótów dla folderu **odwołania** , a następnie wybierz **Dodaj odwołanie**. Zostanie wyświetlone okno dialogowe **Dodawanie odwołania** .

1. Wybierz odwołanie, które chcesz dodać, a następnie kliknij przycisk **OK** .

    Odwołanie zostanie dodany do listy w folderze **odwołania** .

1. Otwieranie menu skrótów dla zestawu dodanego i wybierz polecenie **Właściwości**. Zostanie wyświetlone okno właściwości.

1. Aby uwzględnić tego zestawu w pakiecie usługi, na liście **Kopii lokalnej** wybierz **wartość PRAWDA**.

1. Aby dołączyć pliki w pakiecie usługi, które zostały dodane do projektu roli sieci web, otwórz menu skrótów dla pliku, a następnie wybierz polecenie **Właściwości**. W oknie **Właściwości** wybierz **zawartości** na liście **Tworzenie akcji** .

1. Aby dołączyć pliki w pakiecie usługi, które zostały dodane do projektu roli Pracownik, otwórz menu skrótów dla pliku, a następnie wybierz polecenie **Właściwości**. W oknie **Właściwości** wybierz **Kopiowanie Jeśli nowszego** w polu **Kopiuj do katalogu wyjściowego** listy.

## <a name="next-steps"></a>Następne kroki

Aby dowiedzieć się więcej na temat publikowania Azure z programu Visual Studio, zobacz [Kreatora publikowania aplikacji Azure](vs-azure-tools-publish-azure-application-wizard.md).
