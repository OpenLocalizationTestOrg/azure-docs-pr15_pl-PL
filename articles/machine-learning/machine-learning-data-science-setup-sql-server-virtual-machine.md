<properties
    pageTitle="Konfigurowanie maszyny wirtualnej programu SQL Server jako serwer notesu IPython | Microsoft Azure"
    description="Ustaw w górę danych nauki maszyny wirtualnej z serwera IPython i SQL Server."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev" 
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="xibingao;bradsev" />

# <a name="set-up-an-azure-sql-server-virtual-machine-as-an-ipython-notebook-server-for-advanced-analytics"></a>Konfigurowanie maszyn wirtualnych Azure SQL Server jako serwer notesu IPython zaawansowanej analizy

W tym temacie przedstawiono sposoby obsługi administracyjnej i konfigurowanie maszyny wirtualnej programu SQL Server może być używany jako część środowiska nauki opartej na chmurze danych. Maszyny wirtualnej Windows skonfigurowano obsługi narzędzi, takich jak IPython notesu, Eksplorator magazynu Azure i AzCopy, jak również innych narzędzi, które są przydatne w przypadku projektów nauki danych. Eksplorator magazynu Azure i AzCopy, na przykład zapewniają wygodny sposób przekazywania danych z magazynem obiektów blob platformy Azure z komputera lokalnego lub go pobrać na komputer lokalny z magazynem obiektów blob.

Galeria Azure maszyn wirtualnych zawiera kilka obrazów, które zawierają programu Microsoft SQL Server. Wybierz obraz maszyn wirtualnych programu SQL Server, który jest odpowiedni dla potrzeb danych. Zalecane obrazy są:

- Program SQL Server 2012 z dodatkiem SP2 Enterprise dla małych i średnich danych rozmiary
- Program SQL Server 2012 z dodatkiem SP2 Enterprise zoptymalizowana pod kątem DataWarehousing obciążenie pracą danych duża, aby bardzo dużych rozmiarów

 > [AZURE.NOTE] SQL Server 2012 z dodatkiem SP2 Enterprise obraz **nie ma dysku danych**. Należy dodać i/lub dołączyć co najmniej jeden wirtualnych dysków twardych do przechowywania danych. Po utworzeniu Azure maszyn wirtualnych ma dysku systemu operacyjnego mapowane na dysk C i tymczasowe zamapowane na dysku D. Nie należy używać dysku D do przechowywania danych. Jak wskazuje nazwa, zawiera tylko tymczasowego przechowywania danych. Zapewnia on nie nadmiarowości lub kopii zapasowej ponieważ go nie znajdują się w magazynie Azure.


##<a name="Provision"></a>Nawiązywanie połączenia z portalem klasyczny Azure i obsługi administracyjnej maszyn wirtualnych programu SQL Server

1.  Zaloguj się do [Klasyczny Portal Azure](http://manage.windowsazure.com/) za pomocą konta.
    Jeśli nie masz konta usługi Azure, odwiedź [Azure bezpłatny okres próbny](https://azure.microsoft.com/pricing/free-trial/).

2.  W portalu klasyczny Azure, w lewej dolnej części strony sieci web kliknij pozycję **+ Nowy**, kliknij pozycję **obliczenia**, kliknij pozycję **maszyn wirtualnych**, a następnie kliknij **Z GALERII**.

3.  Na stronie **Tworzenie maszyny wirtualnej** wybierz obrazu maszyn wirtualnych w zależności od potrzeb danych programu SQL Server, a następnie kliknij strzałki w następnym znajdujący się w prawym dolnym rogu strony. Uzyskać najbardziej aktualne informacje dotyczące obsługiwanych obrazów programu SQL Server Azure zobacz [Wprowadzenie do programu SQL Server w maszyn wirtualnych Azure](http://go.microsoft.com/fwlink/p/?LinkId=294720) temat w dokumentacji programu [SQL Server w maszyn wirtualnych Azure](http://go.microsoft.com/fwlink/p/?LinkId=294719) Ustaw.

    ![Wybierz pozycję SQL Server maszyn wirtualnych][1]

4.  Na pierwszej stronie **Konfigurację maszyny wirtualnej** wprowadź następujące informacje:

    -   Podaj **nazwę maszyn wirtualnych**.
    -   W oknie dialogowym **Nowy nazwa użytkownika** wpisz unikatową nazwę użytkownika dla maszyn wirtualnych lokalnego konta administratora.
    -   W polu **Nowe hasło** wpisz silne hasło. Aby uzyskać więcej informacji zobacz [Silne hasła](http://msdn.microsoft.com/library/ms161962.aspx).
    -   W polu **POTWIERDŹ hasło** ponownie wpisz hasło.
    -   Wybierz odpowiedni **rozmiar** z listy rozwijanej.

     > [AZURE.NOTE] Rozmiar maszyny wirtualnej zostanie określony podczas inicjowania obsługi administracyjnej: komórka A2 zawiera najmniejszym rozmiarze zalecane dla obciążenia produkcji. Zalecany minimalny rozmiar maszyny wirtualnej w komórce A3 jest podczas korzystania z programu SQL Server Enterprise Edition. Wybierz pozycję A3 lub nowszy, podczas korzystania z programu SQL Server Enterprise Edition. Wybierz pozycję A4, podczas korzystania z programu SQL Server 2012 lub zoptymalizowane Enterprise 2014 transakcji obciążenia obrazów.
W przypadku korzystania z programu SQL Server 2012 lub zoptymalizowane Enterprise 2014 obrazów obciążenia magazynowanie danych, wybierz pozycję A7. Rozmiar wybrany ogranicza liczbę dyski danych, które można skonfigurować. Najbardziej aktualną informacji na temat dostępnych maszyn wirtualnych rozmiary i liczby dyski danych, które można dołączać do maszyny wirtualnej zobacz [Maszyn wirtualnych rozmiarów Azure](http://msdn.microsoft.com/library/azure/dn197896.aspx). Aby uzyskać informacje o cenach, zobacz [Wirtualnych ceny Macines](https://azure.microsoft.com/pricing/details/virtual-machines/).

    Kliknij strzałkę następnego w prawej dolnej, aby kontynuować.

    ![Konfiguracja maszyn wirtualnych][2]

5.  Na drugiej stronie **konfigurację maszyny wirtualnej** skonfigurować zasoby dla sieci, magazynowania i dostępność:

    -   W oknie dialogowym **Usługa w chmurze** wybierz pozycję **Utwórz nowe usługi w chmurze**.
    -   W polu **Nazwa DNS usługi w chmurze** Ustawianie pierwsza część nazwy DNS dokonanego wyboru, dzięki czemu projekt zostanie zrealizowany nazwę w formacie **TESTNAME.cloudapp.net**
    -   W oknie dialogowym **REGION i KOLIGACJI grupy i WIRTUALNEJ sieci** zaznacz obszar miejsce, w którym ten obraz wirtualnych będą obsługiwane.
    -   W oknie **Konta miejsca do magazynowania**wybierz istniejące konto miejsca do magazynowania lub automatycznie wygenerowanego jeden.
    -   W oknie dialogowym **Ustawianie dostępności** zaznacz pozycję **(Brak)**.
    -   Przeczytaj i zaakceptuj cennik informacji.

6.  W sekcji **punkty końcowe** kliknij pustą listę rozwijaną w obszarze **Nazwa**i wybierz pozycję **MSSQL** a następnie wpisz numer portu wystąpienia aparat bazy danych (**1433** dla domyślnego wystąpienia).

7.  Maszyn wirtualnych usługi SQL Server również może służyć jako IPython serwera Notes, który zostanie skonfigurowany w późniejszym etapie.
    Dodaj nowy punkt końcowy, aby określić port dla serwera IPython notesu. Wprowadź nazwę w kolumnie **Nazwa** , wybierz numer portu wyboru publicznej portu do 9999 portu prywatne.

    Kliknij strzałkę następnego w prawej dolnej, aby kontynuować.

    ![Wybierz porty MSSQL i IPython][3]

8.  Akceptowanie domyślna opcja **zainstalowania maszyn wirtualnych agenta** zaznaczone pole wyboru i kliknij znacznik wyboru w prawym dolnym rogu kreatora, aby ukończyć maszyn wirtualnych, proces inicjowania obsługi administracyjnej.

    `![Opcje ostateczne maszyn wirtualnych][4]

9.  Zaczekaj podczas Azure przygotowanie komputera wirtualnych. Oczekiwany stan maszyny wirtualnej umożliwia przejście do:

    -   Rozpoczynanie (inicjowania obsługi administracyjnej)
    -   Zatrzymano
    -   Rozpoczynanie (inicjowania obsługi administracyjnej)
    -   Uruchamianie (inicjowania obsługi administracyjnej)
    -   Uruchamianie


##<a name="RemoteDesktop"></a>Otwórz za pomocą pulpitu zdalnego i ukończyć konfigurację maszyny wirtualnej

1.  Po ukończeniu inicjowania obsługi administracyjnej, kliknij nazwę komputera wirtualnych, aby przejść do strony pulpitu NAWIGACYJNEGO. U dołu strony kliknij przycisk **Połącz**.

2.  Wybierz pozycję, aby otworzyć plik rpd przy użyciu programu Windows pulpitu zdalnego (`%windir%\system32\mstsc.exe`).

3.  W oknie dialogowym **Zabezpieczenia systemu Windows** należy podać hasło dla lokalnego konta administratora określone w poprzednim kroku.
    (Użytkownik może zostać wyświetlony monit o poświadczenia maszyny wirtualnej sprawdzać.)

4.  Podczas pierwszego logowania się do tej maszyny wirtualnej kilku procesów może być konieczne do wykonania, w tym ustawienia pulpitu, aktualizacji systemu Windows i zakończenia zadań początkowej konfiguracji systemu Windows (sysprep). Po zakończeniu Windows sysprep instalacji programu SQL Server wykonuje zadania konfiguracji. Te zadania może spowodować opóźnienie kilka minut, podczas ich wykonania. `SELECT @@SERVERNAME`nie może zwracać prawidłową nazwę, aż zakończeniu instalacji programu SQL Server i SQL Server Management Studio może nie być visable na stronie początkowej.

Po nawiązaniu połączenia maszyn wirtualnych za pomocą pulpitu zdalnego w systemie Windows maszyny wirtualnej działa podobnie jak inny komputer. Połącz się z domyślnym wystąpieniem programu SQL Server przy użyciu programu SQL Server Management Studio (uruchomione na komputerze wirtualnych) w normalny sposób.


##<a name="InstallIPython"></a>Instalowanie IPython Notes i innych narzędzi do obsługi

Aby skonfigurować nowy maszyn wirtualnych serwera SQL, aby służyć jako serwer IPython notesu i zainstalować dodatkowe narzędzia obsługi takich AzCopy, Eksplorator magazynu Azure przydatne pakietów Python nauki danych i inne osoby, skrypt dostosowywania specjalne są dostępne. Aby zainstalować:

- Kliknij prawym przyciskiem myszy ikonę **Start systemu Windows** , a następnie kliknij pozycję **Wiersz polecenia (Administrator)**
- Skopiuj następujące polecenia i Wklej w wierszu polecenia.

        set script='https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/MachineSetup/Azure_VM_Setup_Windows.ps1'
        @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString(%script%))"

- Po wyświetleniu monitu wprowadź hasło wybranych przez użytkownika dla serwera IPython notesu.
- Skrypt dostosowywania zautomatyzowanie kilku procedur po instalacji, które obejmują:
    + Instalacja i konfiguracja serwera IPython notesu
    + Otwieranie porty TCP w Zaporze systemu Windows dla punktów końcowych, utworzony wcześniej:
    + Dla połączenia zdalnego programu SQL Server
    + Dla połączenia zdalnego serwera IPython notesu
    + Pobieranie przykładowych IPython notesów i skrypty SQL
    + Pobieranie i instalowanie przydatne pakietów Python nauki danych
    + Pobieranie i instalowanie Azure narzędzi, takich jak AzCopy i Eksplorator magazynu platformy Azure  
<br>
- Może uzyskać dostęp do i uruchamianie notesu IPython z dowolnej przeglądarki lokalną lub zdalną przy użyciu adresu URL formularza `https://<virtual_machine_DNS_name>:<port>`, gdzie port jest IPython publicznej zaznaczone podczas inicjowania obsługi administracyjnej maszyny wirtualnej.
- Serwer notesu IPython działa jako usługa tła i zostanie automatycznie uruchomiona, po ponownym uruchomieniu komputera wirtualnych.

##<a name="Optional"></a>Dołączanie danych dysku, stosownie do potrzeb

Jeśli obraz maszyn wirtualnych nie zawiera danych dyski, to znaczy dyski niż dysk C (dysk systemu operacyjnego) i dysku D (dysku tymczasowym), musisz dodać jeden lub więcej dysków danych do przechowywania danych. Obraz maszyn wirtualnych dla programu SQL Server 2012 z dodatkiem SP2 Enterprise zoptymalizowana pod kątem obciążenia DataWarehousing jest wstępnie skonfigurowana z dodatkowych dysków w poszukiwaniu plików danych i dziennika programu SQL Server.

 > [AZURE.NOTE] Nie należy używać dysku D do przechowywania danych. Jak wskazuje nazwa, zawiera tylko tymczasowego przechowywania danych. Zapewnia on nie nadmiarowości lub kopii zapasowej ponieważ go nie znajdują się w magazynie Azure.

Aby dołączyć dodatkowe dane dyski, wykonaj kroki opisane w temacie [jak Dołącz dysku danych do maszyny wirtualnej systemu Windows](virtual-machines-windows-classic-attach-disk.md), który przeprowadzi Cię przez:

1. Dołączanie dysków pusty maszyn wirtualnych obsługi administracyjnej w poprzednich krokach
2. Inicjowanie nowej dyskach maszyny wirtualnej


##<a name="SSMS"></a>Nawiązywanie połączenia z programu SQL Server Management Studio i Włącz uwierzytelnianie w trybie mieszanym

Aparat bazy danych programu SQL Server nie można używać uwierzytelniania systemu Windows bez środowisku domeny. Aby połączyć aparat bazy danych z innego komputera, należy skonfigurować program SQL Server dla uwierzytelniania w trybie mieszanym. Uwierzytelnianie w trybie mieszanym umożliwia zarówno uwierzytelnianie programu SQL Server i uwierzytelniania systemu Windows. Tryb uwierzytelniania SQL jest wymagany do mogły zjeść tej ostatniej dane bezpośrednio z baz danych programu SQL Server w maszyn wirtualnych w [Azure maszynowego uczenia Studio](https://studio.azureml.net) za pomocą modułu importowanie danych.

1.  Łącząc maszyn wirtualnych przy użyciu pulpitu zdalnego, użyj okienka **wyszukiwania** z systemu Windows, a następnie wpisz **SQL Server Management Studio** (SMSS). Kliknij, aby uruchomić program SQL Server Management Studio (SSMS). Być może zechcesz dodać skrót do SSMS na pulpicie do użycia w przyszłości.

    ![Rozpoczynanie SSMS][5]

    Przy pierwszym otwarciu Management Studio musi utworzyć środowisko Management Studio użytkowników. To może potrwać kilka minut.

2.  Podczas otwierania, Management Studio przedstawia okno dialogowe **połączenie z serwerem** . W polu **Nazwa serwera** wpisz nazwę maszyny wirtualnej, aby nawiązać połączenie aparat bazy danych przy użyciu Eksploratora obiektów.
    (Zamiast imienia maszyn wirtualnych umożliwia także **(lokalny)** lub pojedynczego okresu jako **nazwy serwera**. Wybierz **Uwierzytelnianie systemu Windows**i pozostaw ** *z\_maszyn wirtualnych\_nazwę*\\z\_lokalne\_administrator** w polu **Nazwa użytkownika** . Kliknij przycisk **Połącz**.

    ![Nawiązywanie połączenia z serwerem][6]

    <br>

     > [AZURE.TIP] Możesz zmienić tryb uwierzytelniania programu SQL Server przy użyciu zmianach klucza rejestru systemu Windows lub SQL Server Management Studio. Aby zmienić tryb uwierzytelniania za pomocą zmianach klucza rejestru, tworzenia **Nowego zapytania** i wykonać następujący skrypt:

        USE master
        go

        EXEC xp_instance_regwrite N'HKEY_LOCAL_MACHINE', N'Software\Microsoft\MSSQLServer\MSSQLServer', N'LoginMode', REG_DWORD, 2
        go


    Aby zmienić tryb uwierzytelniania za pomocą programu SQL Server management Studio:

3.  W programie **SQL Server Management Studio obiektu Explorer**kliknij prawym przyciskiem myszy nazwę wystąpienia programu SQL Server (nazwa maszyn wirtualnych), a następnie kliknij **Właściwości**.

    ![Właściwości serwera][7]

4.  Na **stronie, w obszarze **Uwierzytelnianie serwera**** wybierz **tryb uwierzytelniania systemu Windows i programu SQL Server**, a następnie kliknij **przycisk OK**.

    ![Wybierz tryb uwierzytelniania][8]

5.  W oknie dialogowym **SQL Server Management Studio** kliknij **przycisk OK** , aby potwierdzić wymagania, aby ponownie uruchomić program SQL Server.

6.  W **Eksploratorze obiektów**kliknij prawym przyciskiem myszy serwer, a następnie kliknij **Uruchom ponownie**. (Jeśli jest uruchomiony program SQL Server Agent, go też należy ponownie.)

    ![Uruchom ponownie][9]

7.  W oknie dialogowym **SQL Server Management Studio** kliknij przycisk **Tak** , aby zgadza się, że chcesz ponownie uruchom program SQL Server.

##<a name="Logins"></a>Tworzenie logowania uwierzytelnianie programu SQL Server

Aby połączyć aparat bazy danych z innego komputera, możesz utworzyć co najmniej jedno logowanie uwierzytelnianie programu SQL Server.  

Możesz utworzyć nowy logowania programu SQL Server programowo lub przy użyciu programu SQL Server Management Studio. Aby utworzyć nowego użytkownika administratora systemu z uwierzytelnianiem SQL programowy, tworzenia **Nowego zapytania** i wykonać następujący skrypt. Zamienianie < nową nazwę użytkownika\> i < nowe hasło\> z wybraną *nazwę użytkownika* i *hasło*. 


    USE master
    go

    CREATE LOGIN <new user name> WITH PASSWORD = N'<new password>',
        CHECK_POLICY = OFF,
        CHECK_EXPIRATION = OFF;

    EXEC sp_addsrvrolemember @loginame = N'<new user name>', @rolename = N'sysadmin';


Dostosuj zasady haseł (przykładowy kod wyłącza zasad wygasania sprawdzanie i hasła). Aby uzyskać więcej informacji na temat logowania do programu SQL Server zobacz [Tworzenie identyfikator logowania](http://msdn.microsoft.com/library/aa337562.aspx).  

Aby utworzyć nowy logowania programu SQL Server przy użyciu programu SQL Server Management Studio:

1.  W programie **SQL Server Management Studio obiektu Explorer**rozwiń folder wystąpienia serwera, w którym chcesz utworzyć nowy identyfikator logowania.

2.  Kliknij prawym przyciskiem myszy folder **zabezpieczeń** , wskaż polecenie **Nowy**i wybierz pozycję **Zaloguj...**.

    ![Nowe logowanie][10]

3.  W oknie dialogowym **logowania — nowe** na stronie **Ogólne** wprowadź nazwę nowego użytkownika w polu **Nazwa logowania** .

4.  Wybierz opcję **Uwierzytelnianie programu SQL Server**.

5.  W polu **hasło** wprowadź hasło dla nowego użytkownika. Wprowadź ponownie hasło w polu **Potwierdź hasło** .

6.  Aby wymusić opcje zasad haseł dla złożoności i wymuszania, wybierz **zasady dotyczące haseł Wymuszaj** (zalecane). To jest domyślna opcja po zaznaczeniu uwierzytelnianie programu SQL Server.

7.  Aby wymusić opcje zasad haseł do wygaśnięcia, wybierz opcję **wygasania haseł Wymuszaj** (zalecane). Wymuszanie zasady dotyczące haseł musi być zaznaczone, aby włączyć to pole wyboru. To jest domyślna opcja po zaznaczeniu uwierzytelnianie programu SQL Server.

8.  Aby wymusić użytkownikowi Utwórz nowe hasło po raz kolejny używanych logowania, wybierz opcję **użytkownik musi zmienić hasło przy następnym logowaniu** (zalecane w przypadku tego logowania innym osobom za pomocą. W przypadku logowania się do własnych potrzeb, nie należy zaznaczać tej opcji.) Wymuszanie wygasania haseł musi być zaznaczone, aby umożliwić to pole wyboru. To jest domyślna opcja po zaznaczeniu uwierzytelnianie programu SQL Server.

9.  Na liście **domyślnej bazy danych** wybierz domyślną bazę danych podczas logowania. **główny** jest wartością domyślną dla tej opcji. Jeśli użytkownik bazy danych nie został jeszcze utworzony, pozostaw wartość do **wzorca**.

10. Na liście **język domyślny** pozostaw **domyślne** wartości.

    ![Właściwości logowania][11]

11. Jeśli jest to pierwszy logowania tworzonego można wyznaczyć tego zaloguj się jako administrator usługi programu SQL Server. Jeśli tak, na stronie **Ról serwera** zaznacz **administratora systemu**.

    > [AZURE.IMPORTANT] Członkowie roli administratora systemu stały serwera mają pełną kontrolę nad aparat bazy danych. Ze względów bezpieczeństwa należy dokładnie ograniczyć członkostwo w tej roli.

    ![grupy][12]

12. Kliknij przycisk OK.

##<a name="DNS"></a>Określanie nazwy DNS maszyny wirtualnej

Aby połączyć aparat bazy danych programu SQL Server z innego komputera, musisz znać nazwę systemu nazw domen (DNS) maszyny wirtualnej.

(Jest to nazwa używanego przez internet do identyfikowania maszyny wirtualnej. Można użyć adresu IP, ale adres IP może się zmienić po Azure przenosi zasobów nadmiarowości lub konserwacji. Nazwa DNS będą stabilny ponieważ mogą być przekierowywane do nowego adresu IP).

1.  W portalu klasyczny Azure (lub w poprzednim kroku) wybierz pozycję **maszyn wirtualnych**.

2.  Na stronie **Wystąpienia maszyn wirtualnych** w kolumnie **Nazwa DNS** Znajdź i skopiuj nazwę DNS maszyny wirtualnej, która jest wyświetlana poprzedzającą **http://**. (Interfejs użytkownika może nie być wyświetlany całą nazwę, ale kliknij prawym przyciskiem myszy go i wybierz polecenie Kopiuj).

##<a name="cde"></a>Nawiązywanie połączenia z aparatu bazy danych z innego komputera

1.  Na komputerze, połączenie z Internetem Otwórz program SQL Server Management Studio.

2.  W oknie dialogowym **połączenie z serwerem** lub **Nawiązywanie połączenia z aparatu bazy danych** , w polu **Nazwa serwera** wprowadź nazwę DNS maszyny wirtualnej (określona w poprzednim zadania) oraz publicznej punkt końcowy numer portu w formacie *Nazwa_dns, numer_portu* takich jak **tutorialtestVM.cloudapp.net,57500**.

3.  W oknie dialogowym **uwierzytelniania** wybierz **Uwierzytelnianie programu SQL Server**.

4.  W oknie dialogowym **logowania** wpisz nazwę logowania, utworzonego w starszej zadania.

5.  W polu **hasło** wpisz hasło logowania utworzonego w starszej zadania.

6.  Kliknij przycisk **Połącz**.

##<a name="amlconnect"></a>Nawiązywanie połączenia z aparatu bazy danych z komputera Azure nauki

W późniejszym etapie procesu nauki danych zespołu [Azure maszynowego uczenia Studio](https://studio.azureml.net) użyje do tworzenia i wdrażania maszynowego uczenia modeli. Aby mogły zjeść tej ostatniej dane z baz danych programu SQL Server w maszyn wirtualnych bezpośrednio do nauki maszynowego Azure szkoleń lub punktowanie, za pomocą modułu **Importowanie danych** w nowych doświadczeniu [Azure maszynowego uczenia Studio](https://studio.azureml.net) . W tym temacie omówiono więcej szczegółów przy użyciu łączy przewodnik proces nauki danych zespołu. Wprowadzenie, zobacz [Co to jest Azure maszynowego uczenia Studio?](machine-learning-what-is-ml-studio.md).

2.  W okienku **Właściwości** [modułu importowanie danych](https://msdn.microsoft.com/library/azure/dn905997.aspx)wybierz **Bazy danych SQL Azure** z listy rozwijanej **Źródło danych** .

3.  W polu tekstowym **Nazwa serwera bazy danych** wprowadź`tcp:<DNS name of your virtual machine>,1433`

4.  Wprowadź nazwę użytkownika SQL w polu tekstowym **Nazwa konta użytkownika serwera** .

5.  Wprowadź hasło użytkownika sql w polu tekstowym **hasło do konta użytkownika serwera** .

    ![Azure ML importowanie danych][13]

##<a name="shutdown"></a>Zamykanie i cofnąć maszyn wirtualnych nieużywane

Azure maszyn wirtualnych kosztują jako **zapłacić tylko w przypadku możesz użyć**. Aby upewnić się, że nie jest konta podczas nie przy użyciu komputera wirtualnych, musi ona być w stanie **zatrzymania (Deallocated)** .

> [AZURE.NOTE] Zamykanie maszyny wirtualnej od wewnątrz (za pomocą opcji power systemu Windows), zatrzymaniu maszyn wirtualnych, ale pozostaje przydzielone. Aby upewnić się, że użytkownik nie zostanie wystawiona, zawsze zatrzymać maszyn wirtualnych w [Klasycznym Portal Azure](http://manage.windowsazure.com/). Możesz też wyłączyć maszyn wirtualnych przy użyciu programu Powershell, dzwoniąc ShutdownRoleOperation z "PostShutdownAction" równa się "StoppedDeallocated".

Zamknij i cofnąć maszyny wirtualnej:

1. Zaloguj się do [Klasyczny Portal Azure](http://manage.windowsazure.com/) za pomocą konta.  

2. Na pasku nawigacyjnym po lewej stronie wybierz pozycję **maszyn wirtualnych** .

3. Na liście maszyn wirtualnych kliknij nazwę komputera wirtualnych, następnie przejdź do strony **pulpitu NAWIGACYJNEGO** .

4. U dołu strony kliknij przycisk **Zamknij**.

![Zamknięcie maszyn wirtualnych][15]

Maszyny wirtualnej umożliwia alokację, ale nie usunięte. Należy ponownie uruchomić komputer wirtualnych w dowolnym momencie z portalu klasyczny Azure.

## <a name="your-azure-sql-server-vm-is-ready-to-use-whats-next"></a>Maszyn wirtualnych usługi Azure SQL Server jest gotowy do użycia: co to jest dalej?

Komputer wirtualnych jest teraz gotowy do użycia w ćwiczeniach nauki do danych. Maszyny wirtualnej również jest gotowa do użycia jako serwer IPython notes badań i przetwarzania danych oraz innych zadań w połączeniu z Azure maszynowego uczenia się i proces nauki danych zespołu (TDSP).

Następne kroki w procesie nauki danych są mapowane w [Proces nauki danych zespołu](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) i może obejmować czynności, które przenoszenie danych do usługi HDInsight, procesu i przykładowe tak w przygotowanie do nauki z danych z nauki maszynowego Azure.


[1]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/selectsqlvmimg.png
[2]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/4vm-config.png
[3]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/sqlvmports.png
[4]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/vmpostopts.png
[5]:./media/machine-learning-data-science-setup-sql-server-virtual-machine/searchssms.png
[6]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/19connect-to-server.png
[7]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/20server-properties.png
[8]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/21mixed-mode.png
[9]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/22restart2.png
[10]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/23new-login.png
[11]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/24test-login.png
[12]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/25sysadmin.png
[13]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/amlreader.png
[15]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/vmshutdown.png
 
