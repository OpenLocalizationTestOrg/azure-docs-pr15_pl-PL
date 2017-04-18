<properties
    pageTitle="Inicjowanie obsługi maszyny wirtualnej nauki danych Linux | Microsoft Azure"
    description="Konfigurowanie i Utwórz maszyny wirtualnej nauki danych Linux Azure analizy i urządzenia szkoleniowe."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"  />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/12/2016"
    ms.author="bradsev" />

# <a name="provision-the-linux-data-science-virtual-machine"></a>Inicjowanie obsługi maszyny wirtualnej nauki Linux danych

Linux danych nauki wirtualna maszyna jest Azure maszyny wirtualnej, która zawiera zestaw narzędzi zainstalowany. Narzędzia te są często używane do wykonywania analizy danych i komputera szkoleniowe. Składniki oprogramowania są:

- Microsoft R Server Developer Edition
- Rozkład Python anaconda (wersje 2.7 i 3.5), łącznie z bibliotekami analizy danych popularne
- JupyterHub - wielodostępna serwera Notes Jupyter obsługującego R, Python, jądra Julia
- Eksplorator magazynu platformy Azure
- Azure interfejs wiersza polecenia (polecenie) do zarządzania zasobami Azure
- PostgresSQL bazy danych
- Nauka obrabiarek
    - [Zestaw narzędzi Sieć obliczeniowe (CNTK)](https://github.com/Microsoft/CNTK): głębokich, nauki narzędzi oprogramowania Microsoft Research.
    - [Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit): szybkie maszynowego uczenia systemu obsługującego technik, takie jak online, mieszania, allreduce, obniżki, learning2search, aktywne i nauka interakcyjne.
    - [XGBoost](https://xgboost.readthedocs.org/en/latest/): narzędzie dostarczając implementacji szybkie i precyzyjne zwiększane drzewa.
    - [Rattle](http://rattle.togaware.com/) (R analitycznych narzędzie do informacje łatwo): narzędzie, dzięki któremu wprowadzenie do analizy danych i maszynowego uczenia się w R łatwe przy użyciu graficznego interfejsu użytkownika danych badań i modelowania z automatyczne generowanie kodu R.
- Azure SDK w Java, Python, node.js dopiskiem, PHP
- Używanie bibliotek w R i Python dla w Azure maszynowego uczenia się i innych usług Azure
- Narzędzi do tworzenia i edytory (Zaćmienie, Emacs, gedit, vi)

Wykonując nauki danych obejmuje Iterowanie sekwencji zadań:

1. Znajdowanie, ładowanie i wstępne przetwarzanie danych
2. Tworzenie i testowanie modeli
3. Wdrażanie modeli zużycia w aplikacjach inteligentne

Naukowców danych za pomocą różnych narzędzi do zakończenia tych zadań. Można być bardzo czasochłonne odpowiednich wersji programu, a następnie do pobrania, można skompilować i zainstalować tych wersji.

Linux danych nauki Virtual Machine można znacznie ułatwić ten ciężar. Służy do błyskawicznego rozpoczynania prac projektu analizy. Pozwala do pracy nad zadaniami w różnych językach, w tym R, Python SQL, Java i C++. Zaćmienie zawiera IDE do projektowania i testowania swój kod, który jest łatwy w użyciu. SDK Azure objęte maszyn wirtualnych umożliwia tworzenie aplikacji przy użyciu różnych usług na Linux platformy chmury firmy Microsoft. Ponadto masz dostęp do innych języków, takich jak dopiskiem, Perl PHP i node.js wstępnie również instalowane.

Istnieją nie opłaty oprogramowania dla tego obrazu maszyn wirtualnych nauki danych. Płatność tylko Azure sprzętu zastosowania opłat, które należy oceniać na podstawie rozmiaru maszyny wirtualnej, która obsługi administracyjnej z obrazem maszyn wirtualnych. Więcej informacji na temat opłat obliczeń można znaleźć w [maszyn wirtualnych, wskazując strony Azure Marketplace ](https://azure.microsoft.com/marketplace/partners/microsoft-ads/linux-data-science-vm/).


## <a name="prerequisites"></a>Wymagania wstępne

Przed utworzeniem maszyny wirtualnej nauki danych Linux musi mieć następujące czynności:

- **Subskrypcja Azure**: Aby uzyskać, zobacz [Azure pobrać bezpłatną wersję próbną](https://azure.microsoft.com/free/).
- **Konta na platformie Azure miejsca do magazynowania**: Aby utworzyć ankietę, zobacz [Tworzenie konta usługi Azure miejsca do magazynowania](storage-create-storage-account.md#create-a-storage-account). Możesz też konta magazynu mogą być tworzone w ramach procesu tworzenia Głosowa, jeśli nie chcesz używać istniejącego konta.


## <a name="create-your-linux-data-science-virtual-machine"></a>Tworzenie komputera wirtualnych nauki Linux danych

Poniżej przedstawiono procedurę tworzenia wystąpienie programu Linux danych nauki maszyny wirtualnej:

1.  Przejdź do maszyny wirtualnej, wskazując w [Azure portal](https://portal.azure.com/#create/microsoft-ads.linux-data-science-vmlinuxdsvm).
2.   Aby wyświetlić kreatora kliknij przycisk **Utwórz** (u dołu). ![Konfigurowanie danych — nauki-maszyn wirtualnych](./media/machine-learning-data-science-linux-dsvm-intro/configure-linux-data-science-virtual-machine.png)
3.   W poniższych sekcjach przedstawiono wartości wejściowych dla każdej z instrukcjami w Kreatorze (wymienione po prawej stronie kolejnej ilustracji) użyte do utworzenia maszyny wirtualnej nauki danych firmy Microsoft. Poniżej przedstawiono wartości wejściowych, potrzebne do skonfigurowania każdego z następujących czynności:

    . **Podstawy**:

  - **Nazwa**: Nazwa serwera nauki danych tworzysz.
  - **Nazwa użytkownika**: pierwsze konto logowania identyfikatora.
  - **Hasło**: pierwszego hasła do konta (zamiast hasła można użyć klucza publicznego SSH).
  - **Subskrypcji**: Jeśli masz więcej niż jedną subskrypcję, wybierz język, którego komputer ma być utworzone i wystawiona. Musisz mieć uprawnienia do tworzenia zasobów dla tej subskrypcji.
  - **Grupa zasobów**: można utworzyć nową lub użyj istniejącej grupy.
  - **Lokalizacja**: wybierz pozycję Centrum danych, który jest najbardziej odpowiedni. Zazwyczaj jest centrum danych, które zawiera większość danych lub się najbliżej Twojej lokalizacji fizycznej najszybszy dostęp do sieci.

    b. **Rozmiar**:

  - Wybierz jeden z typów serwerów, które spełniają wymaganie funkcjonalne i ograniczeń kosztowych usługi. Wybierz pozycję **Wyświetl wszystkie** , aby wyświetlić więcej opcji rozmiarów maszyn wirtualnych.

    c. **Ustawienia**:

  - **Typ dysku**: wybierz pozycję **Premium** , jeśli wolisz używać dysku stanie stałym (SSD). W przeciwnym razie wybierz pozycję **Standardowy**.
  - **Konto miejsca do magazynowania**: Utwórz nowe konto Azure miejsca do magazynowania w ramach subskrypcji lub użycie istniejącej w tej samej lokalizacji, który został wybrany w kroku **podstawy** kreatora.
  - **Inne parametry**: W większości przypadków, po prostu użyć wartości domyślnych. Aby uwzględnić wartości inne niż domyślne, umieść wskaźnik na informacyjna łącze, aby uzyskać pomoc dotyczącą określonych pól.

    d. **Podsumowanie**:

  - Upewnij się, że wszystkie wprowadzone informacje jest poprawny.

    e. **Kupowanie**:

  - Aby rozpocząć, inicjowania obsługi administracyjnej, kliknij przycisk **Kup**. Łącze znajduje się na warunki transakcji. Maszyn wirtualnych nie ma wszelkie dodatkowe opłaty poza obliczeń dla wybranego w kroku **rozmiar** rozmiaru serwera.

Obsługa administracyjna trwa około od 10 do 20 minut. W portalu Azure jest wyświetlany stan inicjowania obsługi administracyjnej.

## <a name="how-to-access-the-linux-data-science-virtual-machine"></a>Jak uzyskać dostęp do maszyny wirtualnej nauki danych Linux

Po utworzeniu maszyn wirtualnych, możesz się zalogować do niej przy użyciu SSH. Użyj poświadczeń konta utworzone w sekcji **podstawowe informacje o** kroku 3 interfejs powłoki tekstu. W systemie Windows możesz pobrać narzędzie klienta SSH, takie jak [Kit](http://www.putty.org). Jeśli wolisz graficzne pulpitu (X Windows System), można użyć X11 przesyłania dalej na Kit lub instalowanie klienta pakietu X2Go.

>[AZURE.NOTE] Klient X2Go wykonywane znacznie większą niż X11 przekazywanie do testowania. Zalecamy korzystanie z klienta X2Go graficznego interfejsu pulpitu.


## <a name="installing-and-configuring-x2go-client"></a>Instalowanie i konfigurowanie X2Go klienta

Maszyn wirtualnych Linux jest już ustanawianie z serwerem X2Go i może zaakceptować połączeń klienta. Aby połączyć na pulpicie graficzne maszyn wirtualnych Linux, wykonaj poniższe czynności na komputerze klienckim:

1. Pobieranie i instalowanie klienta X2Go dla swojej platformy klienta z [X2Go](http://wiki.x2go.org/doku.php/doc:installation:x2goclient).    
2. Uruchom klienta X2Go i wybierz **Nową sesję**. Otwiera okno Konfiguracja z wieloma kartami. Wprowadź poniższe parametry konfiguracji:
    * **Karta sesji**:
        - **Host**: Nazwa hosta lub adres IP maszyn wirtualnych usługi Linux danych naukowych.
        - **Zaloguj się**: nazwa użytkownika na maszyn wirtualnych Linux.
        - **SSH Port**: pozostaw 22, wartość domyślna.
        - **Typ sesji**: Zmień wartość na XFCE. Obecnie maszyn wirtualnych Linux obsługuje tylko XFCE pulpitu.
    * **Karta multimediów**: można wyłączyć dźwięk pomocy technicznej i klienta drukowania, jeśli nie chcesz ich używać.
    * **Foldery udostępnione**: Jeśli chcesz katalogów z programu komputerach klienckich umieszczony na maszyn wirtualnych Linux oraz dodawanie katalogów komputera klienta, które chcesz udostępnić maszyn wirtualnych na tej karcie.

Po zalogowaniu się do maszyn wirtualnych za pomocą klienta SSH lub XFCE graficzne pulpitu przy użyciu klienta X2Go, możesz przystąpić do korzystania z narzędzi, które są zainstalowane i skonfigurowane na maszyn wirtualnych. Na XFCE znajdują się skróty menu aplikacje i ikony pulpitu dla wielu narzędzi.


## <a name="tools-installed-on-the-linux-data-science-virtual-machine"></a>Narzędzia zainstalowane na Linux danych nauki maszyny wirtualnej

### <a name="microsoft-r-open"></a>Otwórz program Microsoft R
R jest jednym z najpopularniejszych języków do analizowania danych i nauka komputera. Jeśli chcesz używać R do analiz maszyn wirtualnych ma Microsoft R Otwórz (MRO) z biblioteki jądra matematyczne (MKL). MKL optymalizuje wspólny algorytmy analityczna operacji matematycznych. MRO jest zgodny z CRAN-R w 100 procentach i dowolne bibliotek R opublikowanych w CRAN, można zainstalować na MRO. Możesz edytować programy R w jednym z Redaktorzy domyślnych, takich jak vi, Emacs i gedit. Możesz również pobrać i za pomocą innych IDEs, takich jak [RStudio](http://www.rstudio.com). Dla wygody prosty skrypt (installRStudio.sh) znajduje się w katalogu **/dsvm/tools** , która jest instalowana RStudio. Jeśli korzystasz z edytora Emacs, notatki czy Emacs pakiet ESS (mówi Statystyka Emacs), który ułatwia Praca z plikami R w edytorze Emacs został zainstalowany.

Aby uruchomić R, wystarczy wpisać w powłoce **R** . Zostanie wyświetlone interaktywnym środowisku. Aby przygotować programu R, zazwyczaj przy użyciu edytora takie jak Emacs lub vi lub gedit, a następnie uruchom skrypty w R. Po zainstalowaniu RStudio masz pełni graficznego środowiska IDE opracowywaniu programu R.

Istnieje także skrypt R do zainstalowania [pakietów góry 20 R](http://www.kdnuggets.com/2015/06/top-20-r-packages.html) , jeśli chcesz. Skrypt może działać, gdy jesteś w interfejsie interakcyjnych R, którą można wprowadzić (wymienione), wpisując **R** w powłoce.  

### <a name="python"></a>Python
Rozwoju przy użyciu Python rozkładu Anaconda Python 2.7 i 3.5 została zainstalowana. Rozkład ten zawiera podstawowy Python wraz z około 300 najpopularniejszych pakietów analizy matematyczne, inżynieria i danych. Możesz użyć domyślnych edytory tekstu. Ponadto można Spyder, IDE Python, który jest dołączany do dystrybucji Anaconda Python. Spyder wymaga graficzne pulpitu lub X11 przekazywanie. Skrót do Spyder znajduje się na pulpicie graficznego.

Ponieważ mamy zarówno Python 2.7 i 3.5 należy specjalnie aktywowanie odpowiedniej wersji Python, który chcesz pracować w bieżącej sesji. Proces aktywacji ustawia zmienną PATH odpowiedniej wersji Python.

Aby aktywować Python 2.7, uruchom następujące w powłoce:

    source /anaconda/bin/activate root

Python 2.7 został zainstalowany w */anaconda/bin*.

Aby aktywować Python 3.5, uruchom następujące w powłoce:

    source /anaconda/bin/activate py35


Python 3.5 został zainstalowany w */anaconda/envs/py35/bin*.

Aby wywołania sesji interakcyjnych Python, wystarczy wpisać **python** w powłoce. Jeśli znajdują się na interfejsem graficznym, lub mają X11 przekazywanie zestawu w górę, należy wpisać **spyder** , aby uruchomić Python IDE.

### <a name="jupyter-notebook"></a>Jupyter notesu

Rozkład Anaconda dołączany Notes Jupyter, środowiska do udostępniania kod i analizy. Notes Jupyter jest dostępna za pośrednictwem JupyterHub. Zaloguj się przy użyciu lokalnego Linux oraz nazwę użytkownika i hasło.

Wstępnie skonfigurowano na serwerze notes Jupyter z Python 2, Python 3 i jądra R. Ma ikony pulpitu o nazwie "Jupyter Notes", aby uruchomić przeglądarkę, aby uzyskać dostęp do serwera Notes. Jeśli korzystasz z maszyn wirtualnych za pomocą klienta SSH lub X2Go, możesz również odwiedzić [https://localhost:8000-](https://localhost:8000/) dostępu do serwera Notes Jupyter.

>[AZURE.NOTE] Kontynuuj, jeśli zostanie wyświetlony ostrzeżenia certyfikatu.

Masz dostęp do serwera Notes Jupyter od dowolnego hosta. Wystarczy wpisać *https://\<DNS maszyn wirtualnych nazwę lub adres IP\>: 8000-*

>[AZURE.NOTE] Port 8000 zostanie otwarty w zaporze domyślnie po zainicjowano obsługę administracyjną maszyn wirtualnych.

Firma Microsoft jest dostarczana przykładowe notesów — Python w jednym, a druga w R. Łącze do próbki można zobaczyć na stronie głównej notesu, po notesu Jupyter uwierzytelniania za pomocą lokalnych Linux oraz nazwę użytkownika i hasło. Możesz utworzyć nowy notes, wybierając pozycję **Nowy**, a następnie jądrze odpowiedni język. Jeśli nie widzisz przycisku **Nowy** , kliknij ikonę **Jupyter** na górze w lewo, aby przejść do strony głównej serwera Notes.


### <a name="ides-and-editors"></a>IDEs i edytory

Dostępne są następujące opcje kilka Redaktorzy kodu. Ta opcja uwzględnia vi-VIM, Emacs, gEdit i Zaćmienie. gEdit Zaćmienie są edytory graficzne i konieczne jest zalogowanie się do pulpitu graficzne ich stosowania. Te edytory mają pulpitu i aplikacji menu skrótów do ich uruchamianie.

**VIM** i **Emacs** są edytory opartych na tekście. Na Emacs możemy został zainstalowany pakiet dodatku o nazwie Emacs mówi statystyki (ESS), który ułatwia pracę z R w edytorze Emacs. Więcej informacji można znaleźć w [ESS](http://ess.r-project.org/).

**Zaćmienie** jest Otwórz źródło extensible IDE, która obsługuje wiele języków. Edition deweloperów języka Java jest wystąpienie zainstalowanym maszyn wirtualnych. Są dostępne dla kilku popularnych języków, które można zainstalować rozszerzenie środowiska Zaćmienie wtyczki. Ponadto mamy wtyczki zainstalowanych w Zaćmienie o nazwie **Azure zestaw narzędzi dla Zaćmienie**. Umożliwia tworzenie, opracowywanie, testowanie i wdrażanie aplikacji Azure za pomocą środowisko projektowania Zaćmienie, która obsługuje języków, takich jak Java. Istnieje także **Azure SDK dla języka Java** zezwalające na dostęp do różnych usług Azure z w środowisku Java. Więcej informacji na temat narzędzi Azure dla programu Eclipse znajdują się u [Azure zestaw narzędzi dla Zaćmienie](../azure-toolkit-for-eclipse.md).

**Lateksu** jest instalowany za pomocą pakietu texlive wraz z pakietu [auctex](https://www.gnu.org/software/auctex/manual/auctex/auctex.html) dodatku Emacs, co ułatwia tworzenie dokumentów lateksu w Emacs.  

### <a name="databases"></a>Bazy danych

#### <a name="postgres"></a>Postgres
Otwórz źródło bazy danych **Postgres** jest dostępna na Głosowa, z usługami uruchomionymi i initdb już wykonane. Nadal potrzebne do utworzenia bazy danych i użytkowników. Aby uzyskać więcej informacji zapoznaj się z [dokumentacją Postgres](https://www.postgresql.org/docs/).  

####  <a name="graphical-sql-client"></a>Graficzne programu SQL client
Nawiązywanie połączenia z różnych baz danych (na przykład Microsoft SQL Server, Postgres i MySQL) i uruchamiania zapytania SQL dostarczono **SQuirrel SQL**, graficzne programu SQL client. To można uruchamiać z sesji pulpitu graficznych (na przykład za pomocą klienta X2Go). Aby wywołać SQuirrel SQL, możesz uruchomić je za pomocą ikony na pulpicie lub uruchom następujące polecenie w powłoce.

    /usr/local/squirrel-sql-3.7/squirrel-sql.sh

Przed pierwszym użyciu skonfigurować sterowniki i aliasy bazy danych. Sterowniki JDBC znajdują się w folderze:

*/usr/share/Java/jdbcdrivers*

Aby uzyskać więcej informacji zobacz [SQuirrel SQL](http://squirrel-sql.sourceforge.net/index.php?page=screenshots).

#### <a name="command-line-tools-for-accessing-microsoft-sql-server"></a>Narzędzia wiersza polecenia w celu uzyskania dostępu do programu Microsoft SQL Server

Pakiet sterownika ODBC dla programu SQL Server również udostępnia dwa narzędzia wiersza polecenia:

**BCP**: zbiorcze narzędzia bcp kopiuje danych między wystąpienia programu Microsoft SQL Server i plik danych w formacie zdefiniowane przez użytkownika. Narzędzie bcp może służyć do importowania dużej liczby nowych wierszy do tabel programu SQL Server lub do wyeksportowania danych z tabel do plików danych. Aby zaimportować dane do tabeli, należy użyć pliku formatu utworzonego dla tej tabeli lub zrozumieć strukturę tabeli i typy danych, które są prawidłowe dla kolumn.

Aby uzyskać więcej informacji zobacz [Łączenie z bcp](https://msdn.microsoft.com/library/hh568446.aspx).

**sqlcmd**: Wprowadź instrukcji Transact-SQL przy użyciu narzędzia sqlcmd, a także procedury systemu i pliki w wierszu polecenia skryptów. To narzędzie używa ODBC, aby wykonać partie języku Transact-SQL.

Aby uzyskać więcej informacji zobacz [Łączenie z sqlcmd](https://msdn.microsoft.com/library/hh568447.aspx).

>[AZURE.NOTE] Istnieją pewne różnice w to narzędzie między platformami systemu Windows i Linux. Zapoznaj się z dokumentacją, aby uzyskać szczegółowe informacje.


#### <a name="database-access-libraries"></a>Baza danych programu access bibliotek

Biblioteki są dostępne w R i Python baz danych programu access.

- W R **RODBC** pakietu lub pakietu **dplyr** umożliwia kwerendy lub wykonywanie instrukcji SQL na serwerze bazy danych.
- W Python biblioteki **pyodbc** zapewnia dostęp do bazy danych z ODBC jako warstwy podstawowej.  

Aby uzyskać dostęp do **Postgres**:

- Z R: Za pomocą pakietu **RPostgreSQL**.
- Z Python: Korzystanie z biblioteki **psycopg2** .


### <a name="azure-tools"></a>Narzędzia Azure
Następujące narzędzia Azure są zainstalowane na maszyn wirtualnych:

- **Azure interfejs wiersza polecenia**: polecenie Azure umożliwia tworzenie i zarządzanie zasobami Azure za pomocą poleceń powłoki. Aby wywołać narzędzia Azure, wystarczy wpisać **azure pomocy**. Aby uzyskać więcej informacji zobacz [polecenie Azure dokumentacji strony](../virtual-machines-command-line-tools.md).
- **Eksplorator magazynu usługi Microsoft Azure**: Eksplorator magazynu usługi Microsoft Azure to narzędzie graficzne, które służy do przeglądania obiektów, które są przechowywane na koncie Azure miejsca do magazynowania i do przekazywania i pobierania danych do i z obiektami blob Azure. Eksplorator magazynu dostępne za pomocą ikony skrót na pulpicie. Można go wywołać w wierszu powłoki, wpisując **StorageExplorer**. Musisz zalogować się w kliencie X2Go lub mieć X11 przekazywanie zestawu w górę.
- **Azure bibliotek**: Oto niektóre z preinstalowanymi bibliotek.

 - **Python**: związane z Azure bibliotek w Python, które są zainstalowane są **azure**, **azureml**, **pydocumentdb**i **pyodbc**. Pierwsze trzy bibliotek umożliwia dostęp do usług Azure magazynu, Azure maszynowego uczenia i DocumentDB Azure (NoSQL bazy danych w Azure). Czwarta biblioteki pyodbc (wraz z sterownik Microsoft ODBC dla programu SQL Server), umożliwiają dostęp do programu SQL Server, bazy danych SQL Azure i magazynu danych SQL Azure z Python przy użyciu interfejsu ODBC. Wprowadź **pip listy** , aby wyświetlić wszystkie wymienione biblioteki. Pamiętaj wykonać to polecenie zarówno w Python 2.7, jak i w środowisku 3,5.
 - **R**: związane z Azure bibliotek R instalowane są **AzureML** i **RODBC**.
 - **Java**: lista bibliotek Azure Java znajdują się w katalogu **/dsvm/sdk/AzureSDKJava** na maszyn wirtualnych. Kluczowe biblioteki są Azure sterowniki magazynowania i zarządzania interfejsy API, DocumentDB i JDBC dla programu SQL Server.  

[Azure portal](https://portal.azure.com) można korzystać ze zainstalowany przeglądarki Firefox. W portalu Azure można utworzyć, zarządzanie i monitorowanie Azure zasobów.

### <a name="azure-machine-learning"></a>Nauka Azure komputera

Azure nauka maszynowego jest usługa w chmurze w pełni zarządzane, umożliwiający budowania, wdrażania i udostępnianie rozwiązań przewidywanych analizy. Utworzyć doświadczeń i modele usługi z Azure maszynowego uczenia Studio. Gdzie są dostępne z przeglądarki sieci web na komputerze wirtualnych nauki danych, odwiedź witrynę [Microsoft Azure maszynowego uczenia](https://studio.azureml.net).

Po zalogowaniu się do Azure maszynowego uczenia Studio masz dostęp do obszaru roboczego eksperyment miejsce, w którym można tworzyć logiczne przepływ dla maszynowego uczenia algorytmów. Także mieć dostęp do notesu Jupyter hostowanej po otrzymaniu komputera Azure i można współpracuje z doświadczenia w Studio nauki komputera. Operationalize maszynowego uczenia modele utworzone przez zawijanie je w interfejs usługi sieci web. Dzięki temu napisane w dowolnym języku klientom wywołania przewidywań z maszynowego uczenia modeli. Aby uzyskać więcej informacji zapoznaj się z [dokumentacją maszynowego uczenia](https://azure.microsoft.com/documentation/services/machine-learning/).

Można również tworzyć modelach R lub Python na maszyn wirtualnych, a następnie wdrożyć produkcji po otrzymaniu komputera Azure. Firma Microsoft zainstalowali bibliotek R (**AzureML**) i Python (**azureml**), aby włączyć tę funkcję.

Aby uzyskać informacje na temat instalacji modeli w R i Python do nauki maszynowego Azure, zobacz [dziesięć rzeczy, które można wykonywać nauki danych maszyn wirtualnych](machine-learning-data-science-vm-do-ten-things.md) (w szczególności sekcji "tworzenie modeli przy użyciu R lub Python i Operationalize je przy użyciu Azure nauki Machine").

>[AZURE.NOTE] Te instrukcje zostały napisane dla wersji maszyn wirtualnych nauki danych dla systemu Windows. Ale informacje o wdrażaniu modeli do nauki maszynowego Azure jest dotyczą maszyn wirtualnych Linux.

### <a name="machine-learning-tools"></a>Nauka obrabiarek

Maszyn wirtualnych zawiera kilka maszynowego nauki korzystania z narzędzia i algorytmy, które zostały wstępnie skompilowany i wstępnie zainstalowany lokalnie. W tym:

* **CNTK** (Obliczeniowe narzędzi Sieć Microsoft Research): głębokich, nauki zestaw narzędzi.
* **Vowpal Wabbit**: algorytm szybkie internetowych.
* **xgboost**: narzędzie, która zapewnia zoptymalizowaną, zwiększane drzewa algorytmów.
* **Python**: Anaconda Python jest powiązany z algorytmów nauki komputera z bibliotekami, takie jak informacje Scikit. Można zainstalować innych bibliotek za pomocą `pip install` polecenia.
* **R**: Biblioteka zaawansowanych funkcji nauki komputera jest dostępne dla R. Niektóre bibliotek, które są wstępnie zainstalowane są lm, glm, randomForest, rpart. Inne biblioteki można zainstalować, uruchamiając:

        install.packages(<lib name>)

Oto niektóre dodatkowe informacje o pierwszej narzędzia trzy nauki komputera na liście.

#### <a name="cntk"></a>CNTK
Jest to Otwórz źródło, głębokości nauki zestaw narzędzi. To narzędzie wiersza polecenia (cntk) i znajduje się już w ŚCIEŻCE.

Aby uruchomić przykładowy podstawowy, wykonaj następujące polecenia w powłoce:

    # Copy samples to your home directory and execute cntk
    cp -r /dsvm/tools/CNTK-2016-02-08-Linux-64bit-CPU-Only/Examples/Other/Simple2d cntkdemo
    cd cntkdemo/Data
    cntk configFile=../Config/Simple.cntk

Model danych wyjściowych znajduje się w *~/cntkdemo/Output/Models*.

Aby uzyskać więcej informacji zobacz sekcję CNTK [GitHub](https://github.com/Microsoft/CNTK)i [CNTK typu wiki](https://github.com/Microsoft/CNTK/wiki).


#### <a name="vowpal-wabbit"></a>Vowpal Wabbit

Vowpal Wabbit jest maszynowego uczenia korzystającego z technik, takie jak online, mieszania, allreduce, obniżki, learning2search, aktywne i nauka interakcyjne.

Aby uruchomić narzędzie bardzo prosty przykład, wykonaj następujące czynności:

    cp -r /dsvm/tools/VowpalWabbit/demo vwdemo
    cd vwdemo
    vw house_dataset

Istnieją inne, większy pokazy w tym katalogu. Aby uzyskać więcej informacji o VW zobacz [w tej sekcji GitHub](https://github.com/JohnLangford/vowpal_wabbit)i [Vowpal Wabbit typu wiki](https://github.com/JohnLangford/vowpal_wabbit/wiki).

#### <a name="xgboost"></a>xgboost
To bibliotece, w której został zaprojektowany i zoptymalizowany pod kątem algorytmów zwiększane (drzewo). Celem tej biblioteki jest, aby przekazać ograniczenia obliczeń maszyn do intensywnej potrzebna do zapewnienia dużych drzewa zwiększenia, który jest skalowalna, przenośny i dokładności.

Znajduje się je jako wiersz polecenia, a także biblioteki R.

Aby użyć tej biblioteki w R, można rozpocząć sesję interakcyjnych R (przez wpisanie po prostu **R** w powłoce) i załadować biblioteki.

Oto prosty przykład, które można uruchamiać w wierszu R:

    library(xgboost)

    data(agaricus.train, package='xgboost')
    data(agaricus.test, package='xgboost')
    train <- agaricus.train
    test <- agaricus.test
    bst <- xgboost(data = train$data, label = train$label, max.depth = 2,
                    eta = 1, nthread = 2, nround = 2, objective = "binary:logistic")
    pred <- predict(bst, test$data)

Aby uruchomić wiersza polecenia xgboost, Oto poleceń do wykonania w powłoce:

    cp -r /dsvm/tools/xgboost/demo/binary_classification/ xgboostdemo
    cd xgboostdemo
    xgboost mushroom.conf


Plik .model jest zapisywany w katalogu określonym. Informacje dotyczące w tym przykładzie pokaz można znaleźć [na GitHub](https://github.com/dmlc/xgboost/tree/master/demo/binary_classification).

Aby uzyskać więcej informacji o xgboost zobacz [xgboost dokumentacji strony](https://xgboost.readthedocs.org/en/latest/)i [Github repozytorium](https://github.com/dmlc/xgboost).

#### <a name="rattle"></a>Rattle
Rattle ( **R** **A**nalytical **T**pismo odręczne — narzędzie **T**o **L**gromadzenie **E**asily) używa danych opartych na Graficznym badań i modelowania. Prezentowane przy jego użyciu statystycznych i wizualnego podsumowania danych, przekształcenia danych, który można łatwo modelowania, tworzy zarówno bez kontroli, jak i pod nadzorem modele danych, przedstawia wydajności modeli w formie graficznej, i zestawów wyników nowe dane. Generowanie kodu R, replikacji operacje w interfejsie użytkownika, które mogą być uruchamiane bezpośrednio w R lub używane jako punkt wyjścia do dalszej analizy.

Aby uruchomić Rattle, musisz być w graficzne logowania sesji pulpitu. Na terminal, wpisz ```R``` wprowadzanie środowiska R. Po wyświetleniu monitu R wpisz następujące polecenia:

    library(rattle)
    rattle()

Teraz interfejsem graficznym otwarty z karty. Poniżej przedstawiono kroki szybki start w Rattle, aby użyć zestawu danych przykładowych pogody i tworzenie modelu. W niektórych z poniższych kroków zostanie wyświetlony monit o automatycznie zainstalować i załadować niektóre wymagane pakiety R, które nie są już w systemie.

>[AZURE.NOTE] Jeśli nie masz dostępu do zainstalowania pakietu w katalogu systemu (ustawienie domyślne), można zobaczyć monit na okna konsoli programu R, aby zainstalować pakiety do swojej biblioteki osobistej. Jeśli zobaczysz monity, odpowiedzi *y* .

1. Kliknij przycisk **Wykonaj**.
2. Okno dialogowe dzwoni, oraz, jeśli chcesz użyć zbiór danych przykładowych pogody. Kliknij przycisk **Tak,** aby załadować w przykładzie.
3. Kliknij kartę **modelu** .
4. Kliknij pozycję **Wykonywanie** tworzenia drzewa decyzji.
5. Kliknij przycisk **Rysuj** , aby wyświetlić drzewa decyzji.
6. Kliknij przycisk radiowy **las** , a następnie kliknij przycisk **Execute** do utworzenia las losowe.
7. Kliknij kartę **Szacowanie** .
8. Kliknij przycisk radiowy **ryzyka** , a następnie kliknij przycisk **Wykonaj** , aby wyświetlić dwa wykresy wydajności ryzyka (skumulowany).
9. Kliknij kartę **dziennika** , aby wyświetlić kod Generuj R poprzedniej operacji.
(Ze względu na błąd w bieżącej wersji programu Rattle, należy wstawić *#* znak przed *Eksportowanie tego dziennika...* w polu Tekst dziennika.)
10. Kliknij przycisk **Eksportuj** , aby zapisać plik skryptu R o nazwie *weather_script. R* w folderze głównym.

Możesz opuścić Rattle i R. Teraz możesz zmodyfikować wygenerowany skrypt R lub używany tak, jak możesz uruchomić ją w dowolnym momencie w celu powtórz wszystkie elementy, które już zostały wykonane w Interfejsie użytkownika Rattle. Zwłaszcza dla początkujących w R to łatwy sposób szybko wykonać analizy i komputera nauki w interfejsem graficznym prosty podczas automatycznego generowania kodu w R, aby zmodyfikować i/lub Dowiedz się.


## <a name="next-steps"></a>Następne kroki
Oto jak możesz nadal do nauki i badań:

* Instruktaż [nauki danych na Linux danych nauki maszyny wirtualnej](machine-learning-data-science-linux-dsvm-walkthrough.md) pokazano, jak wykonać kilka typowych zadań nauki danych z Linux danych nauki maszyn wirtualnych obsługi administracyjnej tutaj. 
* Poznaj różne narzędzia nauki danych dotyczących nauki danych maszyn wirtualnych przy testujesz narzędzia opisane w tym artykule. Aby uzyskać więcej informacji o narzędziach zainstalowanym maszyn wirtualnych można również uruchomić *dsvm więcej info* na powłoki poziomu maszyny wirtualnej podstawowe wprowadzenie i wskaźniki.  
* Dowiedz się, jak tworzyć rozwiązania analitycznych zakończenia do końca systematycznie przy użyciu [Procesu nauki danych zespołu](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).
* Można znaleźć w [Galerii analizy Cortana](http://gallery.cortanaanalytics.com) dla maszynowego uczenia się i danych analizy próbki korzystających z pakietu analizy Cortana.
