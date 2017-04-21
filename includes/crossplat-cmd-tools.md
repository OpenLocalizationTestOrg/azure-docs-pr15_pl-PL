#<a name="how-to-use-the-azure-command-line-tools-for-mac-and-linux"></a>Jak za pomocą narzędzia wiersza polecenia Azure dla komputerów Mac i Linux oraz

Ten przewodnik opisano, jak za pomocą narzędzia wiersza polecenia Azure dla komputerów Mac i Linux oraz tworzenie i zarządzanie nimi usługi Azure. Scenariusze, w których zawierać **Instalowanie narzędzi**, **Importowanie ustawień publikowania**, **Tworzenie i zarządzanie nimi Azure witryn sieci Web**i **Tworzenie i zarządzanie nimi w środowisku maszyn wirtualnych systemu Azure**. Dokumentacji pełna odwołania, zobacz [Azure narzędzia wiersza polecenia dla komputerów Mac i Linux oraz dokumentację][reference-docs]. 

##<a name="table-of-contents"></a>Spis treści
* [Co to są narzędzia wiersza polecenia Azure dla komputerów Mac i Linux oraz](#Overview)
* [Jak zainstalować Azure narzędzia wiersza polecenia dla komputerów Mac i Linux](#Download)
* [Jak utworzyć konto Azure](#CreateAccount)
* [Jak pobrać i Importuj ustawienia publikowania](#Account)
* [Jak utworzyć i zarządzanie witryną sieci Web Azure](#WebSites)
* [Jak tworzyć i zarządzać nimi maszyn wirtualnych Azure](#VMs)


<h2><a id="Overview"></a>Co to są narzędzia wiersza polecenia Azure dla komputerów Mac i Linux oraz</h2>

Azure narzędzia wiersza polecenia dla komputerów Mac i Linux to zbiór wiersza polecenia narzędzia do wdrażania i zarządzania usług Azure.
 
Zadania obsługiwane są następujące:

* Importuj ustawienia publikowania.
* Tworzenie i zarządzanie witrynami sieci Web Azure.
* Tworzenie i zarządzanie maszyn wirtualnych Azure.

Aby uzyskać pełną listę obsługiwanych poleceń, wpisz `azure -help` w wierszu polecenia po zainstalowaniu narzędzi, lub można znaleźć w [dokumentacji][reference-docs].

<h2><a id="Download">Jak zainstalować Azure narzędzia wiersza polecenia dla komputerów Mac i Linux</a></h2>

Poniższa lista zawiera informacje dotyczące instalowania narzędzi wiersza polecenia, zależnie od systemu operacyjnego:

* **Mac**: Pobieranie pakietu [Instalatora zestawu SDK Azure][mac-installer]. Otwórz plik pobrany .pkg i wykonaj kroki instalacji zostanie wyświetlony monit.

* **Linux**: Zainstaluj najnowszą wersję [Node.js] [ nodejs-org] (zobacz [Instalowanie Node.js za pomocą Menedżera pakietów][install-node-linux]), następnie uruchom następujące polecenie:

        npm install azure-cli -g

    **Uwaga**: może być konieczne tego polecenia z podwyższonym poziomem uprawnień:

        sudo npm install azure-cli -g

* **Windows**: Uruchom Instalatora Winows (plik msi), który jest dostępny tutaj: [Azure wiersza polecenia narzędzia][windows-installer].


Aby przetestować instalację, wpisz `azure` w wierszu polecenia. Jeśli instalacja zakończyła się pomyślnie, zobaczysz listę wszystkich dostępnych `azure` polecenia.

<h2><a id="CreateAccount"></a>Jak utworzyć konto Azure</h2>

Aby użyć narzędzia wiersza polecenia Azure dla komputerów Mac i Linux, konieczne będzie konto Azure.

Otwórz przeglądarkę sieci web i przejdź do [http://www.windowsazure.com] [ windowsazuredotcom] i kliknij pozycję **bezpłatną wersję próbną** w prawym górnym rogu.

![Azure witryny sieci Web][Azure Web Site]

Postępuj zgodnie z instrukcjami dotyczącymi tworzenia konta.

<h2><a id="Account"></a>Jak pobrać i Importuj ustawienia publikowania</h2>

Aby rozpocząć pracę, musisz najpierw pobrać i zaimportować do ustawienia publikowania. Dzięki temu będzie można korzystać z narzędzi do tworzenia i zarządzania usługi Azure. Aby pobrać z ustawienia publikowania, za pomocą `account download` polecenie:

    azure account download

To Otwórz przeglądarkę domyślne i monit o zalogować się do portalu zarządzania. Po zalogowaniu się, z `.publishsettings` można pobrać pliku. Zanotuj zapisywania tego pliku.

Następnie należy zaimportować `.publishsettings` pliku, uruchamiając następujące polecenie, zastępując `{path to .publishsettings file}` ścieżką do usługi `.publishsettings` pliku:

    azure account import {path to .publishsettings file}

Możesz usunąć wszystkie informacje przechowywane w <code>import</code> polecenia za pomocą <code>account clear</code> polecenie:

    azure account clear

Aby wyświetlić listę opcji dla `account` za pomocą polecenia `-help` opcję:

    azure account -help

Po zaimportowaniu programu Ustawienia publikowania, należy usunąć `.publishsettings` pliku ze względów bezpieczeństwa.

> [AZURE.NOTE] Po zaimportowaniu ustawienia publikowania, poświadczenia dostępu do subskrypcji usługi Azure znajdują się w Twojej `user` folder. Usługi `user` jest chroniony przez system operacyjny. Jednak zaleca się wykonanie dodatkowych czynności, aby zaszyfrować usługi `user` folder. Można to zrobić w następujący sposób:    
> 
> - W systemie Windows modyfikować właściwości folderu lub za pomocą funkcji BitLocker.
> - Na komputerze Mac Włącz funkcję FileVault folderu.
> - Na Ubuntu za pomocą funkcji katalogu główne szyfrowane. Inne dystrybucji Linux zapewniają równoważne funkcje.

Teraz możesz przystąpić do tworzenia i zarządzania Azure witryn sieci Web i maszyn wirtualnych Azure.  

<h2><a id="WebSites"></a>Jak utworzyć i zarządzanie witryną sieci Web Azure</h2>

###<a name="create-a-website"></a>Tworzenie witryny sieci Web

Aby utworzyć Azure witryny sieci Web, najpierw należy utworzyć pustego katalogu o nazwie `MySite` , a następnie przejdź do tego katalogu.

Następnie uruchom następujące polecenie:

    azure site create MySite --git

Dane wyjściowe tego polecenia będzie zawierać domyślny adres URL dla nowo utworzonego witryny sieci Web. `--git` Opcja umożliwia publikowanie witryny sieci Web, tworząc repozytoria cyfra w katalogu aplikacji lokalnej i w centrum danych witryny sieci Web przy użyciu cyfra. Należy zauważyć, że w przypadku lokalnego folderu jest już repozytorium cyfra, polecenie spowoduje dodanie nowego remote do istniejącego repozytorium, wskazująca repozytorium w centrum danych witryny sieci Web.

Należy zauważyć, że można wykonywać `azure site create` polecenia z dowolną z następujących opcji:

* `--location [location name]`. Ta opcja pozwala określić lokalizację centrum danych, w którym witryny sieci Web jest tworzona (np. "Zachód My"). Jeśli ta opcja zostanie pominięty, będzie promted, aby wybrać tę lokalizację.
* `--hostname [custom host name]`. Ta opcja umożliwia określenie niestandardowej nazwy hosta witryny sieci Web.

Następnie można dodać zawartość do katalogu witryny sieci Web. Za pomocą przepływu zwykła cyfra (`git add`, `git commit`) na zatwierdzenie zawartości. Użyj następującego polecenia cyfra, aby przekazać zawartość witryny sieci Web do Azure: 

    git push azure master

###<a name="set-up-publishing-from-github"></a>Konfigurowanie publikowania z GitHub

Aby skonfigurować ciągłe publikowania z repozytorium GitHub, za pomocą `--GitHub` opcji podczas tworzenia witryny:

    auzre site create MySite --github --githubusername username --githubpassword password --githubrepository githubuser/reponame

Jeśli masz lokalny duplikat repozytorium GitHub lub masz repozytorium z jednym zdalne odwołanie do repozytorium GitHub, to polecenie automatycznie Publikuj kod w repozytorium GitHub do witryny. Następnie wszelkie zmiany przypisany do repozytorium GitHub zostanie automatycznie opublikowana do witryny.

Po skonfigurowaniu publikowania z GitHub gałąź domyślną używany jest gałąź wzorca. Aby określić innej gałęzi, wykonaj następujące polecenie z lokalnego repozytorium:

    azure site repository <branch name>

###<a name="configure-app-settings"></a>Konfigurowanie ustawień aplikacji

Ustawienia aplikacji są pary klucz i wartości, które są dostępne w aplikacji w czasie rzeczywistym. Gdy ustawiony dla witryny sieci Web Azure, wartości ustawień aplikacji zastąpią ustawienia z tym samym kluczem zdefiniowanego w pliku Web.config witryny. W przypadku aplikacji Node.js oraz PHP ustawień aplikacji są dostępne jako zmienne środowiska. W poniższym przykładzie pokazano, jak ustawić pary klucz wartość:

    azure site config add <key>=<value> 

Aby wyświetlić listę wszystkich par klucz wartość, należy użyć następujących czynności:

    azure site config list 

Lub jeśli znasz klucz i chcesz wyświetlić wartość, możesz użyć:

    azure site config get <key> 

Jeśli chcesz zmienić wartość istniejący klucz należy najpierw usuń istniejący klucz, a następnie dodać je ponownie. Polecenie Wyczyść jest następujące:

    azure site config clear <key> 

###<a name="list-and-show-sites"></a>Listy i Pokaż witryn

Aby wyświetlić listę Twojej witryny sieci Web, użyj następującego polecenia:

    azure site list

Aby uzyskać szczegółowe informacje na temat witryny, użyj `site show` polecenia. W poniższym przykładzie pokazano szczegóły dotyczące `MySite`:

    azure site show MySite

###<a name="stop-start-or-restart-a-site"></a>Zatrzymywanie, rozpoczęcie lub ponowne uruchamianie witryny

Zatrzymywanie, Rozpoczynanie i uruchom ponownie z witryną `site stop`, `site start`, lub `site restart` polecenia:

    azure site stop MySite
    azure site start MySite
    azure site restart MySite

###<a name="delete-a-site"></a>Usuwanie witryny

Ponadto można usunąć z witryną `site delete` polecenie:

    azure site delete MySite

Należy zauważyć, że jeśli korzystasz z wymienionych powyżej poleceń znajdujących się w folderze, w którym zostało uruchomione `site create`, nie musisz podać nazwę witryny `MySite` jako ostatni parametr.

Aby wyświetlić pełną listę `site` za pomocą polecenia `-help` opcję:

    azure site -help 

<h2><a id="VMs"></a>Jak tworzyć i zarządzać nimi maszyn wirtualnych Azure</h2>

Maszyn wirtualnych Azure jest tworzona z obrazu maszyn wirtualnych (plik VHD) dostarczone lub dostępne w galerii obrazów. Aby wyświetlić obrazy, które są dostępne, użyj `vm image list` polecenie:

    azure vm image list

Można dodawać i rozpocząć maszyny wirtualnej z jednej z dostępnych obrazów z `vm create` polecenia. W poniższym przykładzie pokazano sposób tworzenia maszyny wirtualnej Linux (o nazwie `myVM`) z obrazu w galerii obrazu (CentOS 6.2). Głównej nazwy użytkownika i hasło dla maszyny wirtualnej są `myusername` oraz `Mypassw0rd` odpowiednio. (Należy zauważyć, że `--location` parametr określa centrum danych, w której maszyna wirtualna została utworzona. W przypadku pominięcia `--location` parametru, użytkownik zostanie wyświetlony monit o wybranie lokalizacji.)

    azure vm create myVM OpenLogic__OpenLogic-CentOS-62-20120509-en-us-30GB.vhd myusername --location "West US"

Można rozważyć przekazywanie `--ssh` flagi (Linux) lub `--rdp` flagę (Windows), aby `vm create` włączania połączeń zdalnych maszyn wirtualnych nowo utworzone.

Jeśli wolisz sposób obsługi administracyjnej maszyny wirtualnej z obraz niestandardowy, możesz utworzyć obraz z pliku VHD z `vm image create` polecenia, a następnie użyj `vm create` polecenia do zapewniania obsługi maszyny wirtualnej. W poniższym przykładzie pokazano sposób tworzenia obrazu Linux (o nazwie `myImage`) z pliku VHD lokalny. ( `--location` Parametr określa danych, w której jest przechowywany obrazu.)

    azure vm image create myImage /path/to/myImage.vhd --os linux --location "West US"

Zamiast tworzyć obrazu z lokalnym VHD, można utworzyć obraz z VHD przechowywane w magazynie obiektów Blob platformy Azure. W tym z `blob-url` parametr:

    azure vm image create myImage --blob-url <url to .vhd in Blob Storage> --os linux

Po utworzeniu obrazu umożliwia obsługę maszyny wirtualnej z obrazu przy użyciu `vm create`. Poniższe polecenie tworzy maszyny wirtualnej o nazwie `myVM` z obrazu utworzony w poprzednim przykładzie (`myImage`).

    azure vm create myVM myImage myusername --location "West US"

Po mają obsługi administracyjnej maszyny wirtualnej, warto utworzyć punkty końcowe, aby umożliwić dostęp zdalny do komputera wirtualnych (na przykład). W poniższym przykładzie użyto `vm create endpoint` polecenie, aby otworzyć port zewnętrznych 22 i lokalnych 22 w `myVM`:

    azure vm endpoint create myVM 22 22

Można uzyskać szczegółowe informacje na temat maszyny wirtualnej (w tym adres IP, nazwę DNS i informacje punktu końcowego) z `vm show` polecenie:

    azure vm show myVM

Aby rozpocząć, lub ponowne uruchomienie komputera wirtualnych, użyj jednej z następujących poleceń:

    azure vm shutdown myVM
    azure vm start myVM
    azure vm restart myVM

I na koniec, aby usunąć maszyn wirtualnych, należy użyć `vm delete` polecenie:

    azure vm delete myVM

Aby uzyskać pełną listę poleceń do tworzenia i zarządzania maszyn wirtualnych za pomocą `-h` opcję:

    azure vm -h

<!-- IMAGES -->
[Azure Web Site]: ./media/crossplat-cmd-tools/freetrial.png

<!-- LINKS -->
[nodejs-org]: http://nodejs.org/
[install-node-linux]: https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager
[mac-installer]: http://go.microsoft.com/fwlink/?LinkId=252249
[windows-installer]: http://go.microsoft.com/fwlink/?LinkID=275464
[reference-docs]: http://go.microsoft.com/fwlink/?LinkId=252246
[windowsazuredotcom]: http://www.windowsazure.com

