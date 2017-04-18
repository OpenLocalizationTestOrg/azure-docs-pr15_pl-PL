<properties
    pageTitle="Konfigurowanie maszyny wirtualnej jako serwer notesu IPython | Microsoft Azure"
    description="Ustaw konto Azure maszyny wirtualnej do użycia w środowisku nauki danych z serwerem IPython dla zaawansowanej analizy."
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
    ms.date="09/19/2016"
    ms.author="xibingao;bradsev" />

# <a name="set-up-an-azure-virtual-machine-as-an-ipython-notebook-server-for-advanced-analytics"></a>Konfigurowanie Azure maszyn wirtualnych jako serwer notesu IPython zaawansowanej analizy

W tym temacie przedstawiono sposoby obsługi administracyjnej i konfigurowanie Azure maszyn wirtualnych dla zaawansowanej analizy, który można może być używany jako część środowiska nauki danych. Maszyny wirtualnej Windows skonfigurowano obsługi narzędzi, takich jak takich jak IPython notesu, Eksplorator magazynu Azure, AzCopy, jak również innych narzędzi, które są przydatne w przypadku projektów zaawansowanej analizy. Eksplorator magazynu Azure i AzCopy, na przykład zapewniają wygodny sposób przekazywania danych z magazynem obiektów blob platformy Azure z komputera lokalnego lub go pobrać na komputer lokalny z magazynem obiektów blob.

## <a name="create-vm"></a>Krok 1: Tworzenie uniwersalny Azure maszyn wirtualnych

Jeśli już masz Azure maszyn wirtualnych i po prostu chcesz skonfigurować na serwerze notes IPython nad nim, możesz pominąć ten krok i przejdź do [Krok 2: Dodawanie punkt końcowy dla notesów IPython istniejących maszyn wirtualnych](#add-endpoint).

Przed rozpoczęciem tworzenia maszyny wirtualnej Azure, musisz określić rozmiar komputera, na którym jest potrzebny do przetwarzania danych ich projektu. Mniejsze maszyn mają mniej pamięci i rdzeni Procesora mniej niż większe, ale są one również mniej drogich. Aby uzyskać listę typów komputera i cen zobacz stronę <a href="http://azure.microsoft.com/pricing/details/virtual-machines/" target="_blank">Ceny maszyn wirtualnych</a>

1. Zaloguj się na <a href="https://manage.windowsazure.com" target="_blank">Klasyczny Portal Azure</a>, a następnie kliknij przycisk **Nowy** w lewym dolnym rogu. Okno będzie wyświetlana. Wybierz pozycję **obliczenia** -> **maszyn wirtualnych** -> **Z GALERII**.

    ![Tworzenie obszaru roboczego][24]

2. Wybierz jedną z następujących obrazów:

    * Windows Server 2012 R2 w centrum danych
    * Środowisko podstawowe informacje dotyczące serwera systemu Windows (Windows Server 2012 R2)

    Następnie kliknij strzałkę wskazującą w prawo w prawym dolnym, aby przejść do następnej strony konfiguracji.

    ![Tworzenie obszaru roboczego][25]

3. Wprowadź nazwę dla maszyny wirtualnej chcesz utworzyć, wybierz odpowiedni rozmiar komputera (domyślny: A3) na podstawie rozmiar danych komputer ma proces i jak zaawansowanych ma komputera (rozmiar pamięci i liczby rdzeni obliczeń), wprowadź nazwę użytkownika i hasło dla komputera. Następnie kliknij strzałkę wskazującą w prawo, aby przejść do następnej strony konfiguracji.

    ![Tworzenie obszaru roboczego][26]

4. Wybierz **REGION i KOLIGACJI grupy i WIRTUALNEJ sieci** zawierającej **Konta miejsca do magazynowania** , który zamierzasz użyć tej maszyny wirtualnej, a następnie wybierz konto miejsca do magazynowania. Dodawanie punktu końcowego u dołu w polu **punkty końcowe** , wprowadzając Nazwa punktu końcowego ("IPython" poniżej). **Nazwa** punktu końcowego i dowolna liczba całkowita od 0 do 65536, **dostępne** jako **PORT PUBLICZNY**, można wybrać dowolny ciąg. **PORT prywatny** ma być **9999**. Użytkownicy w przypadku **unikanie** publicznej porty, które zostały już przypisane do usług internetowych. <a href="http://www.chebucto.ns.ca/~rakerman/port-table.html" target="_blank">Porty dla usług internetowych</a> zawiera listę portów, które zostały przypisane i należy unikać.

    ![Tworzenie obszaru roboczego][27]

5. Kliknij znacznik wyboru, aby rozpocząć proces tworzenia maszyny wirtualnej.

    ![Tworzenie obszaru roboczego][28]


Może upłynąć 15 25 minut maszyny wirtualnej inicjowania obsługi administracyjnej proces. Po utworzeniu maszyny wirtualnej stan tego komputera należy wyświetlić w formacie **uruchomiony**.

![Tworzenie obszaru roboczego][29]

## <a name="add-endpoint"></a>Krok 2: Dodanie punktu końcowego notesów IPython istniejących maszyn wirtualnych

Jeśli utworzono maszyny wirtualnej zgodnie z instrukcjami w kroku 1 punkt końcowy dla IPython Notes został już dodany i ten krok można pominąć.

Jeśli już istnieje maszyny wirtualnej i należy dodać punkt końcowy dla IPython notesu, który zostanie zainstalowany w kroku 3 poniżej pierwszy dziennik w klasycznym Portal Azure, wybierz maszyny wirtualnej, a następnie Dodaj punkt końcowy dla serwera IPython notesu. Poniższa ilustracja zawiera zrzut ekranu: portalu po dodaniu punkt końcowy dla notesu IPython maszyn wirtualnych systemu Windows.

![Tworzenie obszaru roboczego][17]

## <a name="run-commands"></a>Krok 3: Instalowanie IPython Notes i innych narzędzi do obsługi

Po utworzeniu maszyny wirtualnej logowania maszyn wirtualnych systemu Windows za pomocą protokołu RDP (Remote Desktop). Aby uzyskać instrukcje zobacz [jak zalogować się do maszyn wirtualnych uruchamiania systemu Windows Server](../virtual-machines/virtual-machines-windows-classic-connect-logon.md). Otwórz okno **wiersza polecenia** (**nie w oknie polecenia programu Powershell**) jako **Administrator** , a następnie uruchom następujące polecenie.

    set script='https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/MachineSetup/Azure_VM_Setup_Windows.ps1'

    @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString(%script%))"

Po zakończeniu instalacji, serwer IPython Notes jest uruchamiany automatycznie w *C:\\użytkowników\\\<nazwa użytkownika\>\\dokumenty\\notesów IPython* katalogu.

Gdy zostanie wyświetlony monit, wprowadź hasło dla notesu IPython i hasła administratora komputera. Dzięki temu Notes IPython, aby uruchomić jako usługa na komputerze.

## <a name="access"></a>Krok 4: Dostęp do IPython notesów z przeglądarki sieci web
Dostęp do serwera IPython Notes, otwórz sieć web przeglądarki i wprowadzania *Nazwa DNS komputera https://&#60;virtual >: & #60; numer portu publicznej >* w polu tekstowym adres URL. W tym miejscu *& #60; numer portu publicznej >* należy numer portu określony, dodania punkt końcowy IPython notesu.

*& #60; nazwy DNS maszyn wirtualnych >* można znaleźć w klasycznym Portal Azure. Po kliknięciu przycisku rejestrowanie w portalu Klasyczny **maszyn wirtualnych**, wybierz utworzony komputer, a następnie wybierz **Pulpit NAWIGACYJNY**, nazwy DNS będą wyświetlane w następujący sposób:

![Tworzenie obszaru roboczego][19]

Wystąpi ostrzegawczy informujący, że _występuje problem z certyfikatem zabezpieczeń tej witryny sieci Web_ (Internet Explorer) lub _połączenie jest prywatnych_ (Chrome), jak pokazano na poniższej ilustracji. Kliknij przycisk **Kontynuuj, aby ta witryna sieci Web (niezalecane)** (Internet Explorer) lub **Zaawansowane** , a następnie * *Przejdź do & #60;* Nazwa DNS*> (niebezpiecznych) ** (Chrome), aby kontynuować. Następnie wprowadź hasło określonej wcześniej, aby uzyskać dostęp do notesów IPython.

**Internet Explorer:**
![utworzenia obszaru roboczego][20]

**Chrome:**
![utworzenia obszaru roboczego][21]

Po zalogowaniu się do notesu IPython katalogu *DataScienceSamples* zostanie wyświetlona w przeglądarce. Ten katalog zawiera przykładowe IPython notesów, które są udostępniane przez firmę Microsoft, aby ułatwić użytkownikom przeprowadzanie zadań nauki danych. Poniższe przykładowe notesy IPython są wyewidencjonowany z [**repozytorium Github**](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/iPythonNotebooks) maszyn wirtualnych podczas na serwerze notes IPython proces. Firma Microsoft przechowuje i często aktualizuje tego repozytorium. Użytkownicy mogą odwiedzić repozytorium Github, aby uzyskać najnowsze notesów IPython próbki.
![Tworzenie obszaru roboczego][18]

## <a name="upload"></a>Krok 5: Przekazywanie istniejącego notesu IPython z komputera lokalnego na serwerze IPython notesu

Notesy IPython umożliwiają łatwe dla użytkowników przekazać istniejący notes IPython na swoich komputerach na serwerze IPython notesu w środowisku maszyn wirtualnych systemu. Po zalogowaniu się użytkowników do notesu IPython w przeglądarce sieci web kliknij pozycję do **katalogu** notesu IPython zostaną przekazane do. Zaznacz plik .ipynb IPython notesu, aby przekazać z komputera lokalnego w **Eksploratorze plików**i przeciągnij i upuść go do katalogu IPython notesu w przeglądarce sieci web. Kliknij przycisk **Przekaż** , aby przekazać plik .ipynb na serwerze IPython notesu. Inni użytkownicy następnie rozpocząć korzystanie z go z przeglądarek sieci web.

![Tworzenie obszaru roboczego][22]

![Tworzenie obszaru roboczego][23]


##<a name="shutdown"></a>Zamykanie i usunąć przydział maszyn wirtualnych nieużywane

Azure maszyn wirtualnych kosztują jako **zapłacić tylko w przypadku możesz użyć**. Aby upewnić się, że nie jest konta podczas nie przy użyciu komputera wirtualnych, musi ona być w stanie **zatrzymania (Deallocated)** nieużywane.

> [AZURE.NOTE] Po zamknięciu maszyny wirtualnej z wewnątrz maszyn wirtualnych (za pomocą opcji power systemu Windows), maszyn wirtualnych zatrzymaniu ale pozostaną przydzielone. Aby upewnić się, że nadal nie zostanie wystawiona, zawsze zatrzymać maszyn wirtualnych w [Klasycznym Portal Azure](http://manage.windowsazure.com/). Możesz też wyłączyć maszyn wirtualnych przy użyciu programu Powershell, dzwoniąc **ShutdownRoleOperation** z "PostShutdownAction" równa się "StoppedDeallocated".

Zamknij i cofnąć maszyny wirtualnej:

1. Zaloguj się do [Klasyczny Portal Azure](http://manage.windowsazure.com/) za pomocą konta.  

2. Na pasku nawigacyjnym po lewej stronie wybierz pozycję **maszyn wirtualnych** .

3. Na liście maszyn wirtualnych kliknij nazwę komputera wirtualnych, następnie przejdź do strony **pulpitu NAWIGACYJNEGO** .

4. U dołu strony kliknij przycisk **Zamknij**.

![Zamknięcie maszyn wirtualnych][15]

Maszyny wirtualnej umożliwia alokację, ale nie usunięte. Należy ponownie uruchomić komputer wirtualnych w dowolnym momencie z portalu klasyczny Azure.

## <a name="your-azure-vm-is-ready-to-use-whats-next"></a>Maszyn wirtualnych usługi Azure jest gotowy do użycia: co to jest dalej?

Komputer wirtualnych jest teraz gotowy do użycia w ćwiczeniach nauki do danych. Maszyny wirtualnej również jest gotowa do użycia jako serwer IPython notes badań i przetwarzania danych oraz innych zadań w połączeniu z Azure maszynowego uczenia się i proces nauki danych zespołu.

Następne kroki w procesie nauki danych zespołu są mapowane w [Ścieżka nauki](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) i może obejmować czynności, które przenoszenie danych do usługi HDInsight, procesu i przykładowe tak w przygotowanie do nauki z danych z nauki komputera Azure.


[15]: ./media/machine-learning-data-science-setup-virtual-machine/vmshutdown.png
[17]: ./media/machine-learning-data-science-setup-virtual-machine/add-endpoints-after-creation.png
[18]: ./media/machine-learning-data-science-setup-virtual-machine/sample-ipnbs.png
[19]: ./media/machine-learning-data-science-setup-virtual-machine/dns-name-and-host-name.png
[20]: ./media/machine-learning-data-science-setup-virtual-machine/browser-warning-ie.png
[21]: ./media/machine-learning-data-science-setup-virtual-machine/browser-warning.png
[22]: ./media/machine-learning-data-science-setup-virtual-machine/upload-ipnb-1.png
[23]: ./media/machine-learning-data-science-setup-virtual-machine/upload-ipnb-2.png
[24]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-1.png
[25]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-2.png
[26]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-3.png
[27]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-4.png
[28]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-5.png
[29]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-6.png
