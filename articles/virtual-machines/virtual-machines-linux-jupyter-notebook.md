<properties
    pageTitle="Tworzenie notesu Jupyter-IPython | Microsoft Azure"
    description="Dowiedz się, jak wdrożyć notesu Jupyter-IPython na komputerze wirtualnych Linux utworzone za pomocą modelu wdrożenia Menedżera zasobów w Azure."
    services="virtual-machines-linux"
    documentationCenter="python"
    authors="crwilcox"
    manager="wpickett"
    editor=""
    tags="azure-service-management,azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="python"
    ms.topic="article"
    ms.date="11/10/2015"
    ms.author="crwilcox"/>

# <a name="jupyter-notebook-on-azure"></a>Notes Jupyter Azure

[Projekt Jupyter](http://jupyter.org), [IPython projektu](http://ipython.org)udostępnia zestaw narzędzi dla naukowych korzystania z komputera za pomocą zaawansowanych interakcyjnych powłoki łączących wykonywanie kodu z tworzeniem live obliczeniowa dokumentu. Te pliki notesu może zawierać dowolnego tekstu, formuły matematyczne, podaj kod, wyniki, grafiki, klipów wideo i innych rodzajów multimediów, czy przeglądarki sieci web nowoczesny ma możliwość wyświetlania. Czy naprawdę znasz programu Python i Dowiedz się więcej w środowisku zabawa, interakcyjnych lub wykonaj niektóre poważne obliczeniowych równolegle i technicznych, Notes Jupyter to doskonałe rozwiązanie.

![Zrzut ekranu](./media/virtual-machines-linux-jupyter-notebook/ipy-notebook-spectral.png) pakiety przy użyciu SciPy i Matplotlib do analizowania struktury nagrywanie dźwięku.


## <a name="jupyter-two-ways-azure-notebooks-or-custom-deployment"></a>Jupyter dwa sposoby: Azure notesów lub niestandardowe wdrożenia
Azure udostępnia usługa, która umożliwia [Szybkie rozpoczynanie korzystania z Jupyter ](http://blogs.technet.com/b/machinelearning/archive/2015/07/24/introducing-jupyter-notebooks-in-azure-ml-studio.aspx).  Za pomocą usługi Azure notesu, można łatwo uzyskać dostęp do interfejsu dostępnych dla sieci web firmy Jupyter do scalable obliczeniowa zasoby z wszystkich możliwości Python i wiele bibliotek.  Ponieważ instalacja jest obsługiwana przez usługę, użytkownicy mogą otwierać te zasoby bez potrzeby zarządzania i konfiguracji przez użytkownika.

Jeśli usługa notesu nie działa w przypadku rozwiązania przejdź do w tym artykule, które będą procedurach pokazano, jak wdrożyć Notes Jupyter na Microsoft Azure za pomocą Linux wirtualnych maszyn.

[AZURE.INCLUDE [create-account-and-vms-note](../../includes/create-account-and-vms-note.md)]

## <a name="create-and-configure-a-vm-on-azure"></a>Tworzenie i konfigurowanie maszyn wirtualnych Azure

Pierwszym krokiem jest utworzenie uruchomionych Azure maszyny wirtualnej (maszyn wirtualnych).
Ten maszyn wirtualnych pełną system operacyjny w chmurze i będą używane do uruchamiania notesu Jupyter. Azure jest możliwe uruchamianie maszyn wirtualnych zarówno Linux i systemu Windows, a omówimy następujące zagadnienia konfiguracji Jupyter na obu rodzajów maszyn wirtualnych.

### <a name="create-a-linux-vm-and-open-a-port-for-jupyter"></a>Tworzenie maszyny Linux i otworzyć port dla Jupyter

Postępuj zgodnie z instrukcjami podane [tutaj] [ portal-vm-linux] do tworzenia maszyny wirtualnej rozkładu *Ubuntu* . Samouczku KÓW 14.04 serwera Ubuntu. Będzie przyjęto założenie, że nazwa użytkownika *azureuser*.

Po wdrożeniu jej maszyny wirtualnej trzeba otworzyć regułę zabezpieczeń w grupie zabezpieczeń sieci.  Z poziomu portalu Azure przejdź do **Grupy zabezpieczeń sieci** i otwórz kartę dla grupy zabezpieczeń odpowiadające do maszyn wirtualnych. Musisz dodać reguły przychodzące zabezpieczeń z następujących ustawień: **TCP** protokołu, **\*** portu źródłowego (publiczna) i **9999** portu docelowego (prywatnych).

![Zrzut ekranu](./media/virtual-machines-linux-jupyter-notebook/azure-add-endpoint.png)

W grupie zabezpieczeń sieci kliknij **Interfejsów sieciowych** i zwróć uwagę **Publiczny adres IP** , jak będą potrzebne aby nawiązać połączenie z maszyn wirtualnych w następnym kroku.

## <a name="install-required-software-on-the-vm"></a>Zainstalować oprogramowanie wymagane na maszyn wirtualnych

Aby uruchomić notesu Jupyter naszych maszyn wirtualnych, możemy najpierw zainstalować Jupyter i jego zależności. Nawiązywanie połączenia z linux maszyn wirtualnych przy użyciu ssh i parowanie nazwy użytkownika i hasła, możesz wybrać podczas tworzenia maszyn wirtualnych. W tym samouczku pracujemy za pomocą Kit i łączenie się z systemu Windows.

### <a name="installing-jupyter-on-ubuntu"></a>Instalowanie Jupyter na Ubuntu
Zainstaluj Anaconda rozkładzie python nauki popularne danych przy użyciu jednego z łączy dostępnych z [Począwszy analizy](https://www.continuum.io/downloads).  Pisania tego dokumentu, poniżej łącza są najbardziej aktualnej wersji daty.

#### <a name="anaconda-installs-for-linux"></a>Instalacje anaconda Linux
<table>
  <th>Python 3.4</th>
  <th>Python 2.7</th>
  <tr>
    <td>
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda3-2.3.0-Linux-x86_64.sh'>64-bitowej</href>
    </td>
    <td>
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda-2.3.0-Linux-x86_64.sh'>64-bitowej</href>
    </td>
  </tr>
  <tr>
    <td>
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda3-2.3.0-Linux-x86.sh'>32-bitowa</href>
    </td>
    <td>
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda-2.3.0-Linux-x86.sh'>32-bitowa</href>
    </td>  
  </tr>
</table>


#### <a name="installing-anaconda3-230-64-bit-on-ubuntu"></a>Instalowanie Anaconda3 2.3.0 64-bitowy Ubuntu
Na przykład to, jak można zainstalować Anaconda na Ubuntu

    # install anaconda
    cd ~
    mkdir -p anaconda
    cd anaconda/
    curl -O https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda3-2.3.0-Linux-x86_64.sh
    sudo bash Anaconda3-2.3.0-Linux-x86_64.sh -b -f -p /anaconda3

    # clean up home directory
    cd ..
    rm -rf anaconda/

    # Update Jupyter to the latest install and generate its config file
    sudo /anaconda3/bin/conda install jupyter -y
    /anaconda3/bin/jupyter-notebook --generate-config


![Zrzut ekranu](./media/virtual-machines-linux-jupyter-notebook/anaconda-install.png)


### <a name="configuring-jupyter-and-using-ssl"></a>Konfigurowanie Jupyter i przy użyciu protokołu SSL
Po zainstalowaniu trzeba chwilę Konfigurowanie plików konfiguracyjnych dla Jupyter. Jeśli wystąpią troubles konfiguracji Jupyter mogą być pomocne dla Szukaj w [Dokumentacji Jupyter usługami notesu](http://jupyter-notebook.readthedocs.org/en/latest/public_server.html).

Następny firma Microsoft `cd` do katalogu profilu naszych certyfikat SSL tworzyć i edytować plik konfiguracji profilów.

W systemie Linux Użyj następującego polecenia.

    cd ~/.jupyter

Użyj następującego polecenia, aby utworzyć certyfikat SSL (Linux i systemu Windows).

    openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mycert.pem -out mycert.pem

Należy zauważyć, że ponieważ jest tworzone podpisany certyfikat SSL, podczas łączenia się Notes przeglądarki zostaną wyświetlone ostrzeżenie o zabezpieczeniach.  Długotrwałe komercyjny można użyć certyfikatu z podpisem poprawnie skojarzone z Twoją organizacją.  Ponieważ Zarządzanie certyfikatem jest spoza zakresu ten pokaz, firma Microsoft będzie pokazywać certyfikat z podpisem własnym teraz.

Oprócz za pomocą certyfikatu, zapewniają również hasła w celu ochrony przed nieautoryzowanym użyciem notesu.  Ze względów bezpieczeństwa Jupyter wykorzystuje zaszyfrowanych haseł w pliku konfiguracji, więc musisz najpierw szyfrowanie hasła.  IPython udostępnia narzędzia do tego; w wierszu polecenia Uruchom następujące polecenie.

    /anaconda3/bin/python -c "import IPython;print(IPython.lib.passwd())"

Wyświetli monit o podanie hasła i potwierdzenia, a następnie zostaną wydrukowane hasło. Uwaga to następujące czynności.

    Enter password:
    Verify password:
    sha1:b86e933199ad:a02e9592e59723da722.. (elided the rest for security)

Następnie będzie edytować plik konfiguracyjny profilu, który jest `jupyter_notebook_config.py` plik w katalogu jest włączony.  Uwaga, że ten plik nie istnieje — wystarczy utworzyć, jeśli jest to wielkość liter.  Ten plik zawiera liczbę pól i domyślnie są wszystkie ujęta w komentarz.  Można otworzyć tego pliku przy użyciu dowolnego edytora tekstu do potrzeb, a następnie należy upewnić się, że zawiera co najmniej następujące wpisy. **Pamiętaj zamienić c.NotebookApp.password w konfiguracji z sha1 w poprzednim kroku**.

    c = get_config()

    # You must give the path to the certificate file.
    c.NotebookApp.certfile = u'/home/azureuser/.jupyter/mycert.pem'

    # Create your own password as indicated above
    c.NotebookApp.password = u'sha1:b86e933199ad:a02e9592e5 etc... '

    # Network and browser details. We use a fixed port (9999) so it matches
    # our Azure setup, where we've allowed traffic on that port
    c.NotebookApp.ip = '*'
    c.NotebookApp.port = 9999
    c.NotebookApp.open_browser = False

### <a name="run-the-jupyter-notebook"></a>Uruchamianie notesu Jupyter

W tym momencie możemy rozpocząć notesu Jupyter. Aby to zrobić, przejdź do katalogu, w którym ma do przechowywania notesów w i uruchomić serwer notesu Jupyter przy użyciu następującego polecenia.

    /anaconda3/bin/jupyter-notebook

Teraz powinno być możliwe dostęp do notesu Jupyter pod adresem `https://[PUBLIC-IP-ADDRESS]:9999`.

Po raz pierwszy uzyskujesz dostęp do notesu, strona logowania monituje o podanie hasła. I po zalogowaniu się pojawi się "Jupyter notesu pulpitu nawigacyjnego", która jest Centrum kontrolne dla wszystkie operacje notesu.  Na tej stronie można tworzyć nowe notesy i Otwórz istniejące.

![Zrzut ekranu](./media/virtual-machines-linux-jupyter-notebook/jupyter-tree-view.png)

### <a name="using-the-jupyter-notebook"></a>Korzystać z notesu Jupyter

Po kliknięciu przycisku **Nowy** zobaczysz następującą stronę otwarcia.

![Zrzut ekranu](./media/virtual-machines-linux-jupyter-notebook/jupyter-untitled-notebook.png)

Obszar oznaczone symbolem `In []:` monit jest obszarem wprowadzania danych, w tym miejscu możesz wpisać dowolny ważny kod Python i wykona naciśnięcie `Shift-Enter` lub kliknij ikonę "Odtwórz" (wskazującą w prawo trójkąt na pasku narzędzi).

## <a name="a-powerful-paradigm-live-computational-documents-with-rich-media"></a>Zaawansowane modelu: live obliczeniowa dokumentów przy użyciu multimediów

Należy uważa bardzo naturalne dla każdego, kto jest stosowane Python i tekstów, notesu, ponieważ jest na kilka sposobów mieszanych: można wykonywać bloki kodu Python, ale możesz też zachować notatek i innego tekstu, modyfikując styl komórki z "Kodu" do "Promocji cenowych" za pomocą menu rozwijanego na pasku narzędzi.

Jupyter to znacznie więcej niż tekstów, który zezwala mieszanie obliczeń i sformatowany multimediów (tekstu, grafiki, wideo i praktycznie coś nowoczesny przeglądarki można wyświetlać). Można mieszać, tekstu, kod, klipów wideo i innych!

![Zrzut ekranu](./media/virtual-machines-linux-jupyter-notebook/jupyter-editing-experience.png)

I z możliwości Python w wielu doskonałe biblioteki naukowych i technicznych komputerów w następujących zrzut ekranu, można wykonywać proste obliczenia za pomocą tak proste jak analiza sieć złożona wszystko w jednym środowisku.

Opartych na tym modelu mieszania możliwości nowoczesnego sieci web z obliczeń live oferuje wiele możliwości i doskonale nadaje się do chmury; Notes można używać:

* Jako obliczeniowa Notatnik nagrywać badawczych pracować nad problem.

* Aby udostępnić wyniki z innymi pracownikami w formularzu obliczeniowych "live" albo w formatach pisemnie (HTML, plik PDF).

* Rozpowszechnianie i prezentowanie materiały szkoleniowe live, wymagające obliczenia, więc uczniów lub studentów od razu możesz eksperymentować rzeczywisty kod, zmodyfikuj ją i ponownie wykonaj interakcyjnie.

* Aby zapewnić "wykonywalny dokumenty", które prowadzą wyników badań w taki sposób, aby można od razu przedstawionej, sprawdzane i rozszerzonego przez inne osoby.

* Jako platformy dla środowiska wspólnej: wielu użytkowników można zalogować się do tego samego serwera Notes do udostępnienia obliczeniowych sesji na żywo.


Jeśli przejdziesz do kodu źródłowego IPython [repozytorium][], znajdziesz w całym katalogu z przykładami notesu, które możesz pobrać i eksperymentować na własny maszyn wirtualnych Jupyter Azure.  Po prostu Pobierz `.ipynb` pliki z witryny i przekazać je do pulpitu nawigacyjnego notesu Azure maszyn wirtualnych (lub pobrać je bezpośrednio do maszyn wirtualnych).

## <a name="conclusion"></a>Wnioski

Notes Jupyter udostępnia interfejs zaawansowanych interakcyjnie dostępu do potęgi ekosystemie Python Azure.  Obejmuje szeroką gamę przypadków użycia tym prostych badań i Dowiedz się, Python, analizy i wizualizacji, symulacji i obliczanie równolegle. Otrzymane dokumenty notesu zawierają całych rekordów obliczeń, które są wykonywane i można je udostępnić innym użytkownikom Jupyter.  Notes Jupyter może być używany jako aplikacji lokalnej, ale jej doskonale nadaje się do wdrożenia chmura azure

Podstawowe funkcje programu Jupyter są również dostępne w programie Visual Studio za pomocą [Narzędzia Python programu Visual Studio][] (PTVS). PTVS jest bezpłatna i wtyczki firmy Microsoft, który staje się Visual Studio zaawansowane środowisko projektowania Python zawiera zaawansowany edytor za pomocą technologii IntelliSense, debugowanie, profilowanie i równoległych Otwórz źródło obliczania integracji.

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji zobacz [Centrum deweloperów Python](/develop/python/).

[portal-vm-linux]: https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-linux-tutorial-portal-rm/
[repozytorium]: https://github.com/ipython/ipython
[Narzędzia Python programu Visual Studio]: http://aka.ms/ptvs
