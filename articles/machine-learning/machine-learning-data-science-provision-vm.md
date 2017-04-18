<properties 
    pageTitle="Inicjowanie obsługi maszyny wirtualnej nauki danych firmy Microsoft | Microsoft Azure" 
    description="Konfigurowanie i Utwórz maszyny wirtualnej nauki danych Azure analizy i urządzenia szkoleniowe." 
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
    ms.date="09/07/2016" 
    ms.author="bradsev" />


# <a name="provision-the-microsoft-data-science-virtual-machine"></a>Inicjowanie obsługi maszyny wirtualnej nauki danych firmy Microsoft

Wirtualna maszyna nauki danych firmy Microsoft to obraz Azure maszyn wirtualnych (maszyn wirtualnych) wstępnie zainstalowaniu i skonfigurowaniu kilku popularnych narzędzia, które są często używane do analizy danych i nauka komputera. Narzędzia uwzględniane są:

- Microsoft R Server Developer Edition
- Rozkład Python anaconda
- Jupyter Notes (R, Python jądra)
- Edition społeczności programu Visual Studio
- Power BI desktop
- SQL Server 2016 Developer Edition
- Nauka obrabiarek
    - [Zestaw narzędzi Sieć obliczeniowe (CNTK)](https://github.com/Microsoft/CNTK): głębokich, nauki narzędzi oprogramowania Microsoft Research.
    - [Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit): szybkie maszynowego uczenia systemu obsługującego technik, takie jak online, mieszania, allreduce, obniżki, learning2search, aktywne i nauka interakcyjne.
    - [XGBoost](https://xgboost.readthedocs.org/en/latest/): narzędzie dostarczając implementacji szybkie i precyzyjne zwiększane drzewa.
    - [Rattle](http://rattle.togaware.com/) (R analitycznych narzędzie do informacje łatwo): narzędzie, dzięki któremu wprowadzenie do analizy danych i maszynowego uczenia się w R łatwe przy użyciu graficznego interfejsu użytkownika danych badań i modelowania z automatyczne generowanie kodu R.
    - [mxnet](https://github.com/dmlc/mxnet): ramy głębokości szkoleniowe przeznaczone dla wydajność i elastyczność
- Używanie bibliotek w R i Python dla w Azure maszynowego uczenia się i innych usług Azure
- Cyfra tym imprezie cyfra do pracy z tym GitHub, Visual Studio Team Services repozytoria kod źródła
- Porty Windows kilka popularnych Linux narzędzi wiersza (w tym awk sed, perl, grep, Znajdź, wget, zwinięcie itp) dostępne za pośrednictwem wiersza polecenia. 


Wykonując nauki danych wymaga Iterowanie sekwencji zadań: wyszukiwaniu, ładowanie oraz wstępnie przetwarzania danych, tworzenia i testowania modeli i wdrażanie modeli zużycia w aplikacjach inteligentnego. Naukowców danych za pomocą wielu narzędzi do zakończenia tych zadań. Może być bardzo czasochłonne do znalezienia odpowiedniej wersji oprogramowania, a następnie Pobierz i zainstaluj je. Wirtualna maszyna nauki danych firmy Microsoft mogą ułatwiają ten ciężar podając obraz gotowych do użycia, który może zostać zainicjowany Azure wszystkich kilka narzędzi popularnych wstępnie zainstalowany i skonfigurowany. 

Wirtualna maszyna nauki danych firmy Microsoft jump-starts projektu analizy. Pozwala do pracy nad zadaniami w różnych językach, łącznie z R, Python SQL i C#. Program Visual Studio zawiera IDE do projektowania i testowania swój kod, który jest łatwy w użyciu. SDK Azure objęte maszyn wirtualnych umożliwia tworzenie aplikacji przy użyciu różnych usług na platformie chmury firmy Microsoft. 

Istnieją nie opłaty oprogramowania dla tego obrazu maszyn wirtualnych nauki danych. Tylko płatność opłat Azure zastosowania które w zależności od rozmiaru maszyny wirtualnej, które można dodawać. Więcej informacji na temat opłat obliczeń można znaleźć w sekcji szczegółów ceny na stronie [maszyn wirtualnych nauki danych](https://azure.microsoft.com/marketplace/partners/microsoft-ads/standard-data-science-vm/) . 


## <a name="prerequisites"></a>Wymagania wstępne

Aby można było tworzyć maszyny wirtualnej nauki danych firmy Microsoft, musisz mieć następujące czynności:

- **Subskrypcja Azure**: Aby uzyskać, zobacz [Azure pobrać bezpłatną wersję próbną](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

*   **Konta na platformie Azure miejsca do magazynowania**: Aby utworzyć ankietę, zobacz [Tworzenie konta usługi Azure miejsca do magazynowania](storage-create-storage-account.md#create-a-storage-account). Alternatywnie można utworzyć konta miejsca do magazynowania w ramach procesu tworzenia maszyn wirtualnych, jeśli nie chcesz używać istniejącego konta.


## <a name="create-your-microsoft-data-science-virtual-machine"></a>Tworzenie komputera wirtualnych nauki danych firmy Microsoft

Poniżej przedstawiono procedurę tworzenia wystąpienia programu Microsoft danych nauki Virtual Machine:

1.  Przejdź do maszyny wirtualnej, wskazując na [Azure portal](https://portal.azure.com/#create/microsoft-ads.standard-data-science-vmstandard-data-science-vm).
2.   Kliknij przycisk **Utwórz** u dołu, jakie należy uwzględnić kreatora. ![Konfigurowanie danych — nauki-maszyn wirtualnych](./media/machine-learning-data-science-provision-vm/configure-data-science-virtual-machine.png)
3.   Kreator użyte do utworzenia maszyny wirtualnej nauki danych Microsoft wymaga **danych wejściowych** dla każdej z nich **pięć kroków** wymienionych po prawej stronie na poniższym rysunku. Poniżej przedstawiono wartości wejściowych, potrzebne do skonfigurowania każdego z następujących czynności:
    
    1.   **Podstawowe informacje**
        1.   **Nazwa**: Nazwa serwera nauki danych tworzysz.
        2.   **Nazwa użytkownika**: identyfikator logowania konta administratora.
        3.   **Hasło**: hasło konta administratora.
        4.   **Subskrypcji**: Jeśli masz więcej niż jedną subskrypcję, wybierz język, którego komputer ma być utworzone i wystawiona.
        5.   **Grupa zasobów**: można utworzyć nową lub użyj istniejącej grupy.
        6.   **Lokalizacja**: wybierz pozycję Centrum danych, który jest najbardziej odpowiedni. Zazwyczaj jest centrum danych, które zawiera większość danych lub się najbliżej Twojej lokalizacji fizycznej najszybszy dostęp do sieci.
         
    2.   **Rozmiar**: Wybierz jeden z typów serwerów, które spełniają wymaganie funkcjonalne i ograniczeń kosztowych usługi. Możesz uzyskać więcej opcji rozmiarów maszyn wirtualnych, wybierając pozycję "Wyświetl wszystkie".
    
    3.   **Ustawienia**:
        1.   **Typ dysku**: wybierz pozycję Premium Jeśli wolisz dyskiem (SSD), inne wybierz pozycję "Standardowe".
        2.   **Konto miejsca do magazynowania**: można utworzyć nowe konto Azure miejsca do magazynowania w ramach subskrypcji lub użycie istniejącej w tej samej *lokalizacji* , który został wybrany w kroku **podstawy** kreatora.
        3.   **Inne parametry**: zwykle po prostu użyć wartości domyślnych. Po zatrzymaniu wskaźnika na informacyjna łącze, aby uzyskać pomoc dotyczącą określonych pól w przypadku, gdy chcesz rozważ zastosowanie wartości inne niż domyślne.

    4.   **Podsumowanie**: Sprawdź, czy wszystkie informacje wprowadzone są poprawne.
    5.   **Kupowanie**: kliknij przycisk **Kup** Rozpoczynanie inicjowania obsługi administracyjnej. Łącze znajduje się na warunki transakcji. Maszyn wirtualnych nie ma wszelkie dodatkowe opłaty poza obliczeń dla wybranego w kroku **rozmiar** rozmiaru serwera. 


>[AZURE.NOTE] Obsługa administracyjna trwa około od 10 do 20 minut. W portalu Azure jest wyświetlany stan inicjowania obsługi administracyjnej.

## <a name="how-to-access-the-microsoft-data-science-virtual-machine"></a>Jak uzyskać dostęp do nauki wirtualna maszyna danych firmy Microsoft

Po utworzeniu maszyn wirtualnych pulpitu zdalnego, można do niego przy użyciu poświadczeń konta administratora skonfigurowane w poprzedniej sekcji **podstawy** . 

Po swojej maszyn wirtualnych zostanie utworzona i obsługi administracyjnej, możesz przystąpić do rozpocząć korzystanie z narzędzia, które są zainstalowane i skonfigurowane na nim. Istnieją Kafelki menu start i ikony pulpitu dla wielu narzędzi. 

## <a name="how-to-create-a-strong-password-on-the-jupyter-notebook-server"></a>Jak utworzyć silne hasło na serwerze Jupyter notesu 

Aby utworzyć silne hasło dla serwera Notes Jupyter zainstalowany na komputerze, uruchom następujące polecenie w wierszu polecenia na komputerze wirtualnych nauki danych.

    c:\anaconda\python.exe -c "import IPython;print IPython.lib.passwd()"

Wybierz pozycję silne hasło po wyświetleniu monitu.

Zobacz skrótu hasła w formacie "sha1:xxxxxx", w wyniku kwerendy. Skopiuj tego skrótu hasła i Zastąp istniejące skrótu, który znajduje się w pliku konfiguracji notesu w lokalizacji: **C:\ProgramData\jupyter\jupyter_notebook_config.py** z nazwa parametru ***c.NotebookApp.password***.

Zamienić istniejący wartości skrótu, który znajduje się w obrębie cudzysłowu tylko część (xxxxxx). Oferty i ***sha1:*** prefiksu dla wartości parametru oba należy zachować.

Na koniec należy zatrzymać i ponownie uruchomić serwera Jupyter, który jest uruchomiony na maszyn wirtualnych jako zaplanowane zadania systemu windows o nazwie **Start_IPython_Notebook**. Jeśli hasło nie zostanie zaakceptowany po ponownym uruchomieniu tego zadania, spróbuj zabijania wszystkie uruchomione procesy python z Menedżera zadań, a następnie ponownie uruchom zaplanowane zadanie. Ewentualnie spróbuj ponownemu wirtualnych.

## <a name="tools-installed-on-the-microsoft-data-science-virtual-machine"></a>Narzędzia zainstalowane na danych naukowych wirtualna maszyna firmy Microsoft

### <a name="microsoft-r-server-developer-edition"></a>Microsoft R Server Developer Edition
Jeśli chcesz używać R do analiz maszyn wirtualnych ma Microsoft R Server Developer edition zainstalowany. Microsoft R Server jest platformą analizy ogólnie możliwością rozmieszczania klasy korporacyjnej oparte na R, który jest obsługiwany, skalowalna i bezpieczny. Obsługa różnych statystyki duży danych i modelowania przewidywanych maszynowego uczenia możliwości, R serwer obsługuje gamę analizy — badań, analizy, wizualizacji i modelowania. Używając i rozszerzanie Otwórz źródło R, R serwera jest w pełni zgodny z skryptów R, funkcji i pakietów CRAN, do analizowania danych w skali przedsiębiorstwa. Dotyczy również ograniczenia w pamięci Otwórz źródło R, dodając równoległych i fragmentarycznego przetwarzania danych. Dzięki temu będzie można przeprowadzić analizę danych znacznie większy niż co mieści się w pamięci głównej.  Program Visual Studio społeczności Edition włączone maszyn wirtualnych zawiera narzędzia R do rozszerzenie programu Visual Studio, która zapewnia pełną IDE dotyczące pracy z R. Możesz również pobrać i za pomocą innych IDEs także takich jak [RStudio](http://www.rstudio.com). 

### <a name="python"></a>Python
Rozwoju przy użyciu Python rozkładu Anaconda Python 2.7 i 3.5 została zainstalowana. Rozkład ten zawiera podstawowy Python wraz z około 300 najpopularniejszych pakietów analizy matematyczne, inżynieria i danych. Można użyć narzędzia Python dla programu Visual Studio (PTVS) zainstalowanej w tej wersji programu Visual Studio 2015 społeczności lub jeden z IDEs powiązane z Anaconda, takie jak bezczynności lub Spyder. Możesz uruchomić jedną z następujących przez wyszukiwanie na pasku wyszukiwania (**Win** + klawisz**S** ).

>[AZURE.NOTE] Aby wskazać narzędzia Python programu Visual Studio Anaconda Python 2.7 i 3.5, należy utworzyć niestandardowe środowiskach dla każdej wersji. Aby ustawić te ścieżki środowiska Visual Studio Edition społeczności 2015, przejdź do **Narzędzia** -> **Narzędzia Python** -> **Środowiskach Python** , a następnie kliknij pozycję **+ niestandardowe**. 

Anaconda Python 2.7 jest zainstalowany w obszarze C:\Anaconda i Anaconda Python 3.5 jest instalowany w obszarze c:\Anaconda\envs\py35. Aby uzyskać szczegółowe instrukcje, zobacz [dokumentację PTVS](https://github.com/Microsoft/PTVS/wiki/Selecting-and-Installing-Python-Interpreters#hey-i-already-have-an-interpreter-on-my-machine-but-ptvs-doesnt-seem-to-know-about-it) . 

### <a name="jupyter-notebook"></a>Jupyter notesu
Rozkład anaconda dołączany Notes Jupyter, środowiska do udostępniania kod i analizy. Wstępnie skonfigurowano serwer notesu Jupyter z Python 2, Python 3 i jądra R. Ma ikony pulpitu o nazwie "Jupyter Notes, aby uruchomić przeglądarkę, aby uzyskać dostęp do serwera Notes. Jeśli korzystasz z maszyn wirtualnych za pomocą pulpitu zdalnego, możesz również odwiedzić [https://localhost:9999-](https://localhost:9999/) dostępu do serwera Notes Jupyter po zalogowany do maszyn wirtualnych.
 
>[AZURE.NOTE] Kontynuuj, jeśli zostanie wyświetlony ostrzeżenia certyfikatu. 

Firma Microsoft jest dostarczana przykładowe notesy w Python i R. Notesy Jupyter pokazano, jak pracować z serwera R, SQL Server 2016 R Services (w bazie danych analizy), Python i inne technologie Azure po zalogowaniu się do Jupyter. Łącze do próbki można zobaczyć na stronie głównej notesu, po uwierzytelnienia do notesu Jupyter przy użyciu hasła, utworzony w poprzednim kroku. 

### <a name="visual-studio-2015-community-edition"></a>Program Visual Studio 2015 społeczności edition
Program Visual Studio społeczności edition zainstalowany na maszyn wirtualnych. Jest bezpłatna wersja popularne IDE firmy Microsoft, którego można używać na potrzeby obliczeń i dla małych zespołów. Zapoznaj się z postanowień licencyjnych [w tym miejscu](https://www.visualstudio.com/support/legal/mt171547).  Otwórz program Visual Studio, dwukrotne kliknięcie ikony na pulpicie lub w **Start** menu. Możesz również wyszukiwać programy uruchomione z **systemu Windows i** + **S** i wprowadzanie "Visual Studio". Gdy można było tworzyć projekty w języków, takich jak C# Python, R, node.js. Wtyczki są również instalowane składających wygodny do pracy z usługi Azure, takie jak Azure wykazu danych, usługa Azure HDInsight (Hadoop, iskrowy) i Lake danych Azure. 

>[AZURE.NOTE] Może zostać wyświetlony komunikat z informacją, że minął okres próbny. Wprowadź swoje poświadczenia konta Microsoft lub Utwórz nowe konto bezpłatne, aby uzyskać dostęp do programu Visual Studio Edition społeczności. 

### <a name="sql-server-2016-developer-edition"></a>SQL Server 2016 Developer edition
Deweloper wersję programu SQL Server 2016 z usługami R, aby uruchomić analizę w bazie danych znajduje się na maszyn wirtualnych. R usług stanowią platformę opracowywanie i wdrażanie aplikacji inteligentnego. Język R zaawansowanych i zaawansowanych i wiele pakietów ze społeczności służy do tworzenia modeli i wygeneruj przewidywań danych programu SQL Server. Umożliwia zachowanie analizy zbliżony danych, ponieważ R usług (w database) integracja języka R z programu SQL Server. Dzięki temu kosztów i zagrożeń bezpieczeństwa związanych z przenoszenia danych.

>[AZURE.NOTE] Program SQL Server 2016 developer edition można używać tylko do celów badania i rozwój. Potrzebujesz licencji, aby go uruchomić produkcji. 

Masz dostęp do programu SQL server, uruchamiając **Program SQL Server Management Studio**. Nazwy maszyn wirtualnych jest wpisywany jako nazwy serwera. Za pomocą uwierzytelniania systemu Windows, gdy zalogowany jako administrator w systemie Windows. Po wejściu SQL Server Management Studio można utworzyć inni użytkownicy, tworzenie baz danych, importowanie danych i uruchomić zapytania SQL. 

Aby włączyć analiz w bazie danych za pomocą programu Microsoft R, uruchom następujące polecenie jako raz akcji w programie SQL Server management studio po zalogowaniu się jako administrator serwera. 

        CREATE LOGIN [%COMPUTERNAME%\SQLRUserGroup] FROM WINDOWS 
        
        (Please replace the %COMPUTERNAME% with your VM name)


### <a name="azure"></a>Azure 
Kilka narzędzi Azure są zainstalowane na maszyn wirtualnych:

- Istnieje skrót pulpitu, aby uzyskać dostęp do dokumentacji zestawu Azure SDK. 
- **AzCopy**: umożliwia przenoszenie danych do i z konta Microsoft Azure miejsca do magazynowania. Aby wyświetlić zastosowania, wpisz **Azcopy** w wierszu polecenia, aby wyświetlić obciążenie. 
- **Eksplorator magazynu usługi Microsoft Azure**: umożliwia przeglądanie obiektów, które są przechowywane w danych konta na platformie Azure i przełączania do i z miejsca do magazynowania Azure. Można wpisać **Eksplorator magazynu** w wyszukiwaniu lub je znaleźć w menu Start systemu Windows, aby uzyskać dostęp do tego narzędzia. 
- **Adlcopy**: umożliwia przenoszenie danych do Lake danych Azure. Aby wyświetlić zastosowania, wpisz **adlcopy** w wierszu polecenia. 
- **dtui**: umożliwia przenoszenie danych do i z DocumentDB Azure, NoSQL bazy danych w chmurze. Wpisz **dtui** w wierszu polecenia. 
- **Brama zarządzania danymi firmy Microsoft**: umożliwia przenoszenie danych między lokalnych źródeł danych i w chmurze. Są one używane w ramach narzędzi, takich jak Azure danych Factory. 
- **Microsoft Azure programu Powershell**: narzędzie do administrowania Azure zasobów w programie Powershell, języka skryptowego jest również zainstalowana na swojej maszyn wirtualnych. 

###<a name="power-bi"></a>Power BI

Ułatwia tworzenie pulpitów nawigacyjnych i profesjonalnych wizualizacji, **Power BI Desktop** została zainstalowana. To narzędzie do pobrania danych z różnych źródeł, aby móc tworzyć raporty i pulpity nawigacyjne usługi i publikować w chmurze. Aby uzyskać informacje odwiedź witrynę [Usługi Power BI](http://powerbi.microsoft.com) . Power BI desktop można znaleźć w Start menu. 

>[AZURE.NOTE] Potrzebne jest konto usługi Office 365, aby uzyskać dostęp do usługi Power BI. 

## <a name="additional-microsoft-development-tools"></a>Dodatkowe narzędzia rozwoju firmy Microsoft
Odnajdowanie i Pobierz innych narzędzi dla deweloperów programu Microsoft umożliwia [**Instalatora platformy sieci Web firmy Microsoft**](https://www.microsoft.com/web/downloads/platform.aspx) . Istnieje także skrót służący do tego narzędzia na pulpicie maszyn wirtualnych nauki danych firmy Microsoft.  

## <a name="important-directories-on-the-vm"></a>Ważne katalogów na maszyn wirtualnych

| Element                          | Katalogu |
| ------------------------------| ---------------- |
|Jupyter konfiguracji serwera notesu | C:\ProgramData\jupyter |
|Katalogu macierzystego próbki Jupyter notesu| c:\dsvm\notebooks|
|Inne przykłady | c:\dsvm\samples|
| Anaconda (domyślny: Python 2.7) | c:\Anaconda |
| Środowisko Python 3.5 anaconda | c:\Anaconda\envs\py35|
|Katalog wystąpienia R serwer autonomiczny (wystąpienia domyślne R) | C:\Program Files\Microsoft SQL Server\130\R_SERVER |
| Katalog wystąpienia w bazie danych serwera R | C:\Program Files\Microsoft SQL Server\MSSQL13. MSSQLSERVER\R_SERVICES |
| Różne narzędzia | c:\dsvm\tools|

>[AZURE.NOTE] Wystąpienia programu Microsoft danych nauki maszyny wirtualnej utworzone przed 1.5.0 (przed 3 września 2016) używane strukturę katalogów nieco inną niż określonego w powyższej tabeli. 

## <a name="next-steps"></a>Następne kroki
Poniżej przedstawiono możliwe dalsze kroki, aby kontynuować do nauki i badań. 

* Eksplorowanie różne narzędzia nauki danych dotyczących nauki danych maszyn wirtualnych, klikając start menu i wyewidencjonowywanie wymieniony w menu Narzędzia.
* Przejdź do **C:\Program Files\Microsoft SQL Server\130\R_SERVER\library\RevoScaleR\demoScripts** dla próbki za pomocą biblioteki RevoScaleR w R obsługującego analizę danych w skali przedsiębiorstwa.  
* Przeczytaj artykuł: [10 rzeczy, które można wykonywać nauki danych maszyn wirtualnych](http://aka.ms/dsvmtenthings)
* Dowiedz się, jak tworzyć kompleksowe rozwiązania analitycznych systematycznie przy użyciu [Procesu nauki danych zespołu](https://azure.microsoft.com/documentation/learning-paths/data-science-process/).
* Można znaleźć w [Galerii analizy Cortana](http://gallery.cortanaintelligence.com) dla maszynowego uczenia się i danych analizy próbki korzystających z pakietu analizy Cortana. Ikona także zostały zamieszczone w **Start** menu i na pulpicie maszyny wirtualnej do tej galerii.

