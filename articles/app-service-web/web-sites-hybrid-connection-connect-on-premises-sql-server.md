<properties
    pageTitle="Nawiązywanie połączenia z lokalnego programu SQL Server za pomocą aplikacji sieci web w usłudze aplikacji Azure za pomocą funkcji hybrydowych połączenia"
    description="Tworzenie aplikacji sieci web w programie Microsoft Azure i połączyć go z bazą danych programu SQL Server lokalnego"
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/09/2016"
    ms.author="cephalin"/>

# <a name="connect-to-on-premises-sql-server-from-a-web-app-in-azure-app-service-using-hybrid-connections"></a>Nawiązywanie połączenia z lokalnego programu SQL Server za pomocą aplikacji sieci web w usłudze aplikacji Azure za pomocą funkcji hybrydowych połączenia

Hybrydowe połączenia można nawiązać [Usługa Azure aplikacji](http://go.microsoft.com/fwlink/?LinkId=529714) Web Apps zasobów lokalnych, korzystających statyczne port TCP. Obsługiwane zasobów należą Microsoft SQL Server, MySQL, interfejsy API sieci Web HTTP, aplikacji usługi i większość niestandardowych usług sieci Web.

W tym samouczku dowiesz się, jak utworzyć aplikację sieci web aplikacji usługi w [Azure Portal](http://go.microsoft.com/fwlink/?LinkId=529715), połączenia z bazą danych programu SQL Server lokalnego lokalnego, za pomocą nowych funkcji hybrydowych połączenia aplikacji sieci web, tworzenie prostych aplikacji ASP.NET, za pomocą połączenia hybrydowych i wdrożyć aplikację do aplikacji usługi aplikacji sieci web. Aplikacji web wykonanych na Azure przechowywanych poświadczeń użytkownika w bazie danych członkostwa z lokalną. Samouczek przyjęto założenie, nie wcześniejszego doświadczenia w korzystaniu Azure lub ASP.NET.

>[AZURE.NOTE] Jeśli chcesz rozpocząć pracę z Azure aplikacji usługi przed utworzeniem konta dla konta Azure, przejdź do [Spróbuj aplikacji usługi](http://go.microsoft.com/fwlink/?LinkId=523751), którym natychmiast można utworzyć aplikację sieci web krótkotrwałe starter w aplikacji usługi. Nie kart kredytowych wymagane; nie zobowiązania.
>
>Aplikacje sieci Web część funkcji hybrydowych połączeń jest dostępne tylko w [Azure Portal](https://portal.azure.com). Aby utworzyć połączenie w usługach BizTalk, zobacz [Hybrydowych połączenia](http://go.microsoft.com/fwlink/p/?LinkID=397274).  

## <a name="prerequisites"></a>Wymagania wstępne ##

Aby użyć tego samouczka, musisz następujące produkty. Dostępne są wszystkie w bezpłatnej wersji, aby rozpocząć opracowywania dla Azure całkowicie bezpłatnie.

- **Azure subskrypcji** — bezpłatnej subskrypcji, zobacz [Azure bezpłatną wersję próbną](/pricing/free-trial/).

- **Visual Studio 2013** — Pobierz bezpłatną wersję próbną programu Visual Studio 2013, zobacz temat [Visual Studio — pliki do pobrania](http://www.visualstudio.com/downloads/download-visual-studio-vs). Zainstaluj to przed kontynuowaniem.

- **Dodatek Service Pack 1 dla programu Microsoft .NET Framework 3.5** — w przypadku systemu operacyjnego Windows 8.1, Windows Server 2012 R2, Windows 8, Windows Server 2012, Windows 7 lub Windows Server 2008 R2, możesz włączyć ten w Panelu sterowania > programy i funkcje > Włącz lub wyłącz funkcje systemu Windows. W przeciwnym razie możesz go pobrać z [Centrum pobierania Microsoft](http://www.microsoft.com/download/en/details.aspx?displaylang=en&id=22).

- **SQL Server 2014 Express narzędziami** — Pobierz program Microsoft SQL Server Express bezpłatnie na [stronie bazy danych platformy sieci Web firmy Microsoft](http://www.microsoft.com/web/platform/database.aspx). Wybierz wersję **Express** (nie LocalDB). Wersja **Express z narzędziami** zawiera program SQL Server Management Studio, które będą używane w tym samouczku.

- **Program SQL Server Management Studio Express** — jest on dołączony do programu SQL Server 2014 Express pobierania narzędzia wymienionych powyżej, ale jeśli chcesz zainstalować oddzielnie, możesz pobrać i zainstalować go z [programu SQL Server Express pobieranie strony](http://www.microsoft.com/web/platform/database.aspx).

Samouczek przyjęto założenie, że masz subskrypcję Azure, że zainstalowano program Visual Studio 2013 i zainstalowaniu lub włączony program .NET Framework 3.5. Samouczek pokazano, jak zainstalować programu SQL Server Express 2014 w konfiguracji, która odpowiada również za pomocą funkcji Azure hybrydowych połączenia (w przypadku wystąpienia domyślnego przy użyciu statycznego port TCP). Przed rozpoczęciem przerabiania samouczka należy pobrać programu SQL Server 2014 Express przy użyciu narzędzi z lokalizacji wymienionych powyżej, jeśli nie masz zainstalowanego programu SQL Server.

### <a name="notes"></a>Notatki ###
Aby użyć programu SQL Server lub SQL Server Express bazy danych lokalnych z połączeniem hybrydowe, TCP/IP musi być włączona na porcie statyczne. Wystąpienia domyślnego w programie SQL Server korzysta z portu statycznego 1433, nazwanych wystąpień nie.

Komputer, na którym jest instalowany agenta hybrydowych Connection Manager lokalnego:

- Musi mieć połączeń wychodzących Azure przez:

Port|Dlaczego
---|---
80|**Wymagane** dla port HTTP sprawdzania poprawności certyfikatów i opcjonalnie dla łączności danych.
443|**Opcjonalnie** dla łączności danych. Jeśli połączenie wychodzące 443 jest niedostępna, jest używany TCP port 80.
5671 i 9352|Opcjonalnie dla łączności danych, ale **zalecane** . Należy zauważyć, że w tym trybie ogół daje wyższej przepustowości. Jeśli wychodzące połączenie tych portów jest niedostępna, jest używany TCP port 443.
- Musi mieć możliwość uzyskania *hostname*:*numer_portu* zasobu lokalnego.

Kroki opisane w tym artykule założono, że używasz przeglądarki z komputera, na którym będzie przechowywana agenta połączenia hybrydowych lokalnego.

Jeśli masz już zainstalowany w konfiguracji i w środowisku, które spełniają warunki opisany powyżej programu SQL Server, możesz od razu przejść i rozpocząć [Tworzenie bazy danych programu SQL Server lokalnego](#CreateSQLDB).

<a name="InstallSQL"></a>
## <a name="a-install-sql-server-express-enable-tcpip-and-create-a-sql-server-database-on-premises"></a>ODPOWIEDZI. Zainstalowanie programu SQL Server Express, Włącz protokół TCP/IP i tworzenie lokalnej bazy danych programu SQL Server ##

W tej sekcji pokazano, jak zainstalować programu SQL Server Express, Włącz protokół TCP/IP i utworzyć bazę danych, aby umożliwić aplikacji sieci web Portal Azure.

### <a name="install-sql-server-express"></a>Instalowanie programu SQL Server Express ###

1. Aby zainstalować programu SQL Server Express, uruchom pobranego pliku **SQLEXPRWT_x64_ENU.exe** lub **SQLEXPR_x86_ENU.exe** . Zostanie wyświetlony Kreator Centrum instalacji programu SQL Server.

    ![Instalacja programu SQL Server][SQLServerInstall]

2. Wybierz pozycję **autonomicznej instalacji nowego programu SQL Server lub dodawanie funkcji do istniejącej instalacji**. Postępuj zgodnie z instrukcjami, akceptując ustawień domyślnych i ustawień, aż przejdziesz na stronie **Konfiguracja wystąpienia** .

3. Na stronie **Konfiguracja wystąpienia** wybierz **Domyślne wystąpienie**.

    ![Wybierz pozycję wystąpienia domyślnego][ChooseDefaultInstance]

    Domyślnie domyślne wystąpienie programu SQL Server odbiera żądania od klientów programu SQL Server na porcie statyczne 1433, czyli wymaga funkcji hybrydowych połączenia. Nazwane wystąpienia za pomocą dynamicznych portów i UDP, które nie są obsługiwane przez połączenia hybrydowych.

4. Zaakceptuj ustawienia domyślne na stronie **Konfiguracji serwera** .

5. Na stronie **Konfiguracja aparat bazy danych** w **Trybie uwierzytelniania**wybierz **Mieszanego (uwierzytelnianie programu SQL Server i uwierzytelniania systemu Windows)**, a podanie hasła.

    ![Wybierz tryb mieszanym][ChooseMixedMode]

    W tym samouczku będą używane uwierzytelnianie programu SQL Server. Należy zapamiętać hasło, które można wprowadzić, ponieważ będą potrzebne później.

6. Wyświetlanie kolejnych pozostałe kroki kreatora, aby ukończyć instalację.

### <a name="enable-tcpip"></a>Włącz protokół TCP/IP ###
Aby włączyć protokół TCP/IP, program serwera Menedżer konfiguracji programu SQL, który został zainstalowany podczas instalacji programu SQL Server Express. Postępuj zgodnie z instrukcjami [Włącz protokół dla programu SQL Server](http://technet.microsoft.com/library/hh231672%28v=sql.110%29.aspx) , przed kontynuowaniem.

<a name="CreateSQLDB"></a>
### <a name="create-a-sql-server-database-on-premises"></a>Tworzenie lokalnej bazy danych programu SQL Server ###

Aplikacja sieci web programu Visual Studio wymaga członkostwa w bazie danych, które mogą uzyskiwać dostęp do Azure. W tym celu bazy danych programu SQL Server lub SQL Server Express (nie LocalDB bazy danych, która domyślnie używa szablonu MVC), aby następnie utworzysz członkostwa w bazie danych.

1. Program SQL Server Management Studio nawiązanie połączenia z programu SQL Server został zainstalowany. (Jeśli **połączenie z serwerem** nie pojawi się okno dialogowe automatycznie, przejdź do **Eksploratora obiektów** w okienku po lewej stronie, kliknij pozycję **Połącz**, a następnie kliknij **Aparat bazy danych**).  ![Połączyć się z serwerem][SSMSConnectToServer]

    W obszarze **Typ serwera**wybierz **Aparat bazy danych**. W polu **Nazwa serwera**służy **hosta lokalnego** lub nazwę komputera, którego używasz. Wybierz **Uwierzytelnianie programu SQL Server**, a następnie zaloguj się przy użyciu nazwy użytkownika i hasła, który został utworzony wcześniej.

2. Aby utworzyć nową bazę danych przy użyciu programu SQL Server Management Studio, kliknij prawym przyciskiem myszy **baz danych** w Eksploratorze obiektów, a następnie kliknij **Nową bazę danych**.

    ![Tworzenie nowej bazy danych][SSMScreateNewDB]

3. W oknie dialogowym **Nowej bazy danych** wprowadź MembershipDB Nazwa bazy danych, a następnie kliknij **przycisk OK**.

    ![Podaj nazwę bazy danych][SSMSprovideDBname]

    Należy zauważyć, że zostaną nie wprowadzone zmiany do bazy danych na tym etapie. Po uruchomieniu informacji o członkostwie zostaną automatycznie później dodane przez aplikację sieci web.

4. W Eksploratorze obiektów Jeśli możesz rozwiń węzeł **bazy danych**, zobaczysz utworzenia bazy danych członków.

    ![MembershipDB utworzone][SSMSMembershipDBCreated]

<a name="CreateSite"></a>
## <a name="b-create-a-web-app-in-the-azure-portal"></a>B. Tworzenie aplikacji sieci web w Azure Portal ##

> [AZURE.NOTE] Jeśli utworzono już aplikacji sieci web w Portal Azure, który ma być używany dla tego samouczka, możesz od razu przejść do [tworzenia połączenia hybrydowych i usługą BizTalk](#CreateHC) i Kontynuuj stamtąd.

1. W [Azure Portal](https://portal.azure.com), kliknij przycisk **Nowy** > **Web + Mobile** > **aplikacji sieci Web**.

    ![Przycisk Nowy][New]

2. Konfigurowanie aplikacji sieci web, a następnie kliknij przycisk **Utwórz**.

    ![Nazwa witryny sieci Web][WebsiteCreationBlade]

3. Po chwili jest tworzona aplikacja sieci web i zostanie wyświetlony jego karta aplikacji sieci web. Karta jest pionowo przewijaniem pulpitu nawigacyjnego, która umożliwia zarządzanie aplikacji sieci web.

    ![Z witryny sieci Web][WebSiteRunningBlade]

    Aby sprawdzić, czy istnieje aplikacja sieci web, klikając ikonę **Przeglądaj** , aby wyświetlić domyślnej strony.

Następnie utworzysz, połączenie hybrydowych i usługi BizTalk dla aplikacji sieci web.

<a name="CreateHC"></a>
## <a name="c-create-a-hybrid-connection-and-a-biztalk-service"></a>C. Tworzenie połączenia funkcji hybrydowych i usługą BizTalk ##

1. Wstecz w portalu, przejdź do strony Ustawienia i kliknij pozycję **Networking** > **skonfigurować połączenie hybrydowego punktów końcowych**.

    ![Hybrydowe połączenia][CreateHCHCIcon]

2. Na hybrydowych karta połączeń, kliknij przycisk **Dodaj** > **nowe połączenie hybrydowych**.

3. Na karta **Tworzenie hybrydowych połączenia** :
    - W polu **Nazwa**Podaj nazwę dla tego połączenia.
    - **Nazwa hosta**wprowadź nazwę komputera, komputera-hosta programu SQL Server.
    - **Port**wprowadź 1433 (domyślny port dla programu SQL Server).
    - Kliknij pozycję **Usługa BizTalk** > **Nową usługę BizTalk** i wprowadź nazwę dla usługi BizTalk.

    ![Tworzenie połączenia funkcji hybrydowych][TwinCreateHCBlades]

5. Kliknij **przycisk OK** dwa razy.

    Po zakończeniu procesu, będzie obszar **powiadomień** flash zielony **SUKCESU** i będzie karta **połączenia hybrydowych** Pokaż nowe połączenie hybrydowych ze stanem jako **nie masz połączenia**.

    ![Połączenia hybrydowych jednego utworzonego][CreateHCOneConnectionCreated]

W tym momencie została ukończona ważną częścią infrastruktury połączenia hybrydowych chmury. Następnie utworzysz odpowiedniego fragmentu lokalnego.

<a name="InstallHCM"></a>
## <a name="d-install-the-on-premises-hybrid-connection-manager-to-complete-the-connection"></a>D. Instalowanie lokalnego hybrydowych Connection Manager, aby nawiązać połączenie ##

[AZURE.INCLUDE [app-service-hybrid-connections-manager-install](../../includes/app-service-hybrid-connections-manager-install.md)]

Teraz, gdy hybrydowych infrastruktury połączenie zostanie zakończone, możesz utworzyć aplikacji sieci web, który z nich korzysta.

<a name="CreateASPNET"></a>
## <a name="e-create-a-basic-aspnet-web-project-edit-the-database-connection-string-and-run-the-project-locally"></a>E. Tworzenie podstawowego projektu sieci web programu ASP.NET, edytowanie parametry połączenia bazy danych i uruchomić projekt lokalnie ##

### <a name="create-a-basic-aspnet-project"></a>Tworzenie podstawowego projektu programu ASP.NET ###
1. W programie Visual Studio w menu **plik** Tworzenie nowego projektu:

    ![Nowy projekt w programie Visual Studio][HCVSNewProject]

2. W sekcji **Szablony** w oknie dialogowym **Nowy projekt** wybierz **sieci Web** i wybierz **Aplikację sieci Web programu ASP.NET**, a następnie kliknij **przycisk OK**.

    ![Wybierz aplikację sieci Web programu ASP.NET][HCVSChooseASPNET]

3. W oknie dialogowym **Nowy projekt ASP.NET** wybierz **MVC**, a następnie kliknij **przycisk OK**.

    ![Wybierz pozycję MVC][HCVSChooseMVC]

4. Po utworzeniu projektu zostanie wyświetlona strona — plik readme aplikacji. Nie są jeszcze uruchamiane projektu sieci web.

    ![Strony — plik Readme][HCVSReadmePage]

### <a name="edit-the-database-connection-string-for-the-application"></a>Edytuj parametry połączenia bazy danych dla aplikacji ###

W tym kroku możesz edytować parametry połączenia, który wskazuje aplikacji, gdzie szukać lokalnej bazy danych programu SQL Server Express. Parametry połączenia znajduje się w pliku Web.config aplikacji, który zawiera informacje dotyczące konfiguracji aplikacji.

> [AZURE.NOTE] Aby upewnić się, że aplikacja używa bazy danych, która została utworzona w programu SQL Server Express, a nie do domyślnej Visual Studio LocalDB, należy wykonać ten krok, przed uruchomieniem projektu.

1. W oknie Eksplorator rozwiązań kliknij dwukrotnie plik Web.config.

    ![Web.config][HCVSChooseWebConfig]

2. Edytowanie sekcji **connectionStrings** wskaż bazy danych programu SQL Server na komputerze lokalnym, po składni w poniższym przykładzie:

    ![Parametry połączenia][HCVSConnectionString]

    Podczas tworzenia parametry połączenia, należy pamiętać, następujące czynności:

    - Jeśli łączysz się z nazwanym wystąpieniem zamiast w przypadku wystąpienia domyślnego (na przykład YourServer\SQLEXPRESS), musisz skonfigurować program SQL Server umożliwia statyczne porty. Informacje dotyczące konfigurowania statycznej porty Zobacz [jak skonfigurować program SQL Server, aby odsłuchać określony port](http://support.microsoft.com/kb/823938). Domyślnie nazwane wystąpienia za pomocą UDP i portów dynamicznych, które nie są obsługiwane przez połączenia hybrydowych.

    - Zalecane jest, aby określić port (1433 domyślnie, jak pokazano w przykładzie) w parametrach połączenia tak, aby użytkownik może upewnij się, że lokalnego serwera SQL ma TCP włączone i jest używany poprawny port.

    - Pamiętaj, aby połączyć, za pomocą uwierzytelnianie programu SQL Server określenie Identyfikatora użytkownika i hasła w ciągu połączenia.

3. Kliknij przycisk **Zapisz** w programie Visual Studio, aby zapisać plik Web.config.

### <a name="run-the-project-locally-and-register-a-new-user"></a>Uruchom projektu lokalnie i Zarejestruj nowego użytkownika ###

1. Teraz uruchamianie nowego projektu sieci web lokalnie, klikając przycisk Przeglądaj w obszarze debugowania. W tym przykładzie użyto programu Internet Explorer.

    ![Uruchamianie programu project][HCVSRunProject]

2. W prawym górnym rogu domyślnej strony sieci web wybierz pozycję **Zarejestruj** , aby zarejestrować nowe konto:

    ![Rejestrowanie nowego konta][HCVSRegisterLocally]

3. Wprowadź nazwę użytkownika i hasło:

    ![Wprowadź nazwę użytkownika i hasło][HCVSCreateNewAccount]

    Bazy danych zostanie automatycznie tworzy na serwerze SQL lokalnym, zawierający informacje o członkostwie aplikacji. Jedna z tabel (**dbo. AspNetUsers**) blokady sieci web poświadczeń użytkownika aplikacji, takich jak te, które zostały wprowadzone. W poniższej tabeli zostanie wyświetlona w dalszej części samouczka.

4. Zamknij okno przeglądarki domyślnej strony sieci web. Powoduje zatrzymanie aplikacji w programie Visual Studio.

Teraz możesz przystąpić do następnego kroku, czyli opublikować aplikacji Azure i należy je przetestować.

<a name="PubNTest"></a>
## <a name="f-publish-the-web-application-to-azure-and-test-it"></a>F. Publikowanie aplikacji sieci web z Azure i należy je przetestować ##

Teraz będą publikować aplikacji do aplikacji sieci web aplikacji usługi, a następnie przetestuj go, aby zobaczyć, jak połączenie hybrydowe, które skonfigurowano wcześniej jest używany do łączenia aplikacji sieci web z bazą danych na komputerze lokalnym.

### <a name="publish-the-web-application"></a>Publikowanie aplikacji sieci web ###

1. Możesz pobrać profilu publikowania dla aplikacji sieci web aplikacji usługi w portalu Azure. Na karta dla aplikacji sieci web kliknij przycisk **Pobierz Opublikuj profilu**, a następnie zapisz plik na komputerze.

    ![Plik do pobrania Publikuj profil][PortalDownloadPublishProfile]

    Następnie zaimportuje tego pliku do aplikacji sieci web programu Visual Studio.

2. W programie Visual Studio kliknij prawym przyciskiem myszy nazwę projektu w Eksploratorze rozwiązań i wybierz pozycję **Publikuj**.

    ![Wybierz pozycję Publikuj][HCVSRightClickProjectSelectPublish]

3. W oknie dialogowym **Publikowanie sieci Web** , na karcie **profil** wybierz polecenie **Importuj**.

    ![Importowanie][HCVSPublishWebDialogImport]

4. Przejdź do pobranego profilu publikowania, zaznacz go, a następnie kliknij **przycisk OK**.

    ![Przejdź do profilu][HCVSBrowseToImportPubProfile]

5. Publikowanie informacji jest importowana i są wyświetlane na karcie **połączenia** w oknie dialogowym.

    ![Kliknij przycisk Publikuj][HCVSClickPublish]

    Kliknij przycisk **Publikuj**.

    Po zakończeniu publikowania przeglądarce uruchamianie i Pokaż teraz znanych aplikacji ASP.NET — z wyjątkiem, że jest teraz żywo w chmurze Azure!

Za pomocą aplikacji sieci live web będzie dalej, zobacz połączenie hybrydowych w działaniu.

### <a name="test-the-completed-web-application-on-azure"></a>Testowanie aplikacji sieci web złożonym Azure ###

1. W górnym rogu strony sieci web, Azure, wybierz pozycję **Zaloguj**.

    ![Test Zaloguj][HCTestLogIn]

2. Aplikacji sieci web usługi aplikacji jest teraz połączone z bazą danych członkostwa w aplikacji sieci web na komputerze lokalnym. Aby to sprawdzić, zaloguj się przy użyciu tych samych poświadczeń, wprowadzone w bazie danych lokalnych wcześniej.

    ![Witaj pozdrowienia][HCTestHelloContoso]

3. Aby jeszcze bardziej przetestować nowe połączenie hybrydowego, wylogować się z aplikacji sieci Azure web i zarejestrować jako innego użytkownika. Podaj nową nazwę użytkownika i hasło, a następnie kliknij przycisk **Zarejestruj**.

    ![Test zarejestrować innego użytkownika][HCTestRegisterRelecloud]

4. Aby sprawdzić, czy poświadczenia nowego użytkownika były przechowywane w lokalnej bazy danych za pośrednictwem połączenia hybrydowe, otwórz SQL Management Studio na komputerze lokalnym. W Eksploratorze obiektów rozwiń **MembershipDB** bazy danych, a następnie rozwiń listę **tabel**. Kliknij prawym przyciskiem myszy **dbo. AspNetUsers** członkostwa w tabeli i wybierz polecenie **1000 góry Zaznacz wiersze** , aby wyświetlić wyniki.

    ![Wyświetlanie wyników][HCTestSSMSTree]

5. Tabela członkostwo lokalnej zostaną wyświetlone oba konta — ten, który został utworzony lokalnie i, w którym został utworzony w chmurze Azure. Ten, który został utworzony w chmurze został zapisany w lokalnej bazy danych za pomocą funkcji hybrydowych połączenia przez Azure.

    ![Zarejestrowanych użytkowników w lokalnej bazy danych][HCTestShowMemberDb]

Teraz masz już utworzone i wdrożone aplikacji sieci web ASP.NET korzystającego z hybrydowego połączenia między web app w chmurze Azure i lokalnej bazy danych programu SQL Server. Gratulacje!

## <a name="see-also"></a>Zobacz też ##
[Omówienie połączeń hybrydowego](http://go.microsoft.com/fwlink/p/?LinkID=397274)

[Tomasz dołączony wprowadza hybrydowych połączeń (wideo kanału 9)](http://channel9.msdn.com/Shows/Azure-Friday/Josh-Twist-introduces-hybrid-connections)

[Omówienie połączeń hybrydowego](/services/biztalk-services/)

[Usługi BizTalk: Karty pulpitu nawigacyjnego, Monitor, skali, konfigurowanie i połączenia hybrydowego](../biztalk-services/biztalk-dashboard-monitor-scale-tabs.md)

[Tworzenie w chmurze hybrydowego rzeczywistych z Bezproblemowa przenoszenia aplikacji (9 kanału wideo)](http://channel9.msdn.com/events/TechEd/NorthAmerica/2014/DCIM-B323#fbid=)

[Dostęp do zasobów lokalnych hybrydowych połączenia przy użyciu Azure aplikacji usługi](../app-service-web/web-sites-hybrid-connection-get-started.md)

[Omówienie tożsamości ASP.NET](http://www.asp.net/identity)

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]


<!-- IMAGES -->
[SQLServerInstall]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A01SQLServerInstall.png
[ChooseDefaultInstance]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A02ChooseDefaultInstance.png
[ChooseMixedMode]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A03ChooseMixedMode.png
[SSMSConnectToServer]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A04SSMSConnectToServer.png
[SSMScreateNewDB]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A05SSMScreateNewDBlh.png
[SSMSprovideDBname]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A06SSMSprovideDBname.png
[SSMSMembershipDBCreated]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A07SSMSMembershipDBCreated.png
[New]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B01New.png
[NewWebsite]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B02NewWebsite.png
[WebsiteCreationBlade]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B03WebsiteCreationBlade.png
[WebSiteRunningBlade]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B04WebSiteRunningBlade.png
[Browse]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B05Browse.png
[DefaultWebSitePage]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B06DefaultWebSitePage.png
[CreateHCHCIcon]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C01CreateHCHCIcon.png
[CreateHCAddHC]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C02CreateHCAddHC.png
[TwinCreateHCBlades]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C03TwinCreateHCBlades.png
[CreateHCCreateBTS]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C04CreateHCCreateBTS.png
[CreateBTScomplete]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C05CreateBTScomplete.png
[CreateHCSuccessNotification]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C06CreateHCSuccessNotification.png
[CreateHCOneConnectionCreated]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C07CreateHCOneConnectionCreated.png
[HCIcon]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D01HCIcon.png
[NotConnected]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D02NotConnected.png
[NotConnectedBlade]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D03NotConnectedBlade.png
[ClickListenerSetup]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D04ClickListenerSetup.png
[ClickToInstallHCM]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D05ClickToInstallHCM.png
[ApplicationRunWarning]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D06ApplicationRunWarning.png
[UAC]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D07UAC.png
[HCMInstalling]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D08HCMInstalling.png
[HCMInstallComplete]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D09HCMInstallComplete.png
[HCStatusConnected]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D10HCStatusConnected.png
[HCVSNewProject]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E01HCVSNewProject.png
[HCVSChooseASPNET]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E02HCVSChooseASPNET.png
[HCVSChooseMVC]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E03HCVSChooseMVC.png
[HCVSReadmePage]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E04HCVSReadmePage.png
[HCVSChooseWebConfig]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E05HCVSChooseWebConfig.png
[HCVSConnectionString]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E06HCVSConnectionString.png
[HCVSRunProject]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E06HCVSRunProject.png
[HCVSRegisterLocally]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E07HCVSRegisterLocally.png
[HCVSCreateNewAccount]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E08HCVSCreateNewAccount.png
[PortalDownloadPublishProfile]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F01PortalDownloadPublishProfile.png
[HCVSPublishProfileInDownloadsFolder]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F02HCVSPublishProfileInDownloadsFolder.png
[HCVSRightClickProjectSelectPublish]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F03HCVSRightClickProjectSelectPublish.png
[HCVSPublishWebDialogImport]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F04HCVSPublishWebDialogImport.png
[HCVSBrowseToImportPubProfile]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F05HCVSBrowseToImportPubProfile.png
[HCVSClickPublish]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F06HCVSClickPublish.png
[HCTestLogIn]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F07HCTestLogIn.png
[HCTestHelloContoso]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F08HCTestHelloContoso.png
[HCTestRegisterRelecloud]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F09HCTestRegisterRelecloud.png
[HCTestSSMSTree]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F10HCTestSSMSTree.png
[HCTestShowMemberDb]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F11HCTestShowMemberDb.png
