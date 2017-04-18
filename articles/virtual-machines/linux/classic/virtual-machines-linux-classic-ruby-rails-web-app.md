<properties
    pageTitle="Udostępniać dopiskiem w witrynie sieci Web szyn na maszyny Linux | Microsoft Azure"
    description="Konfigurowanie i obsługiwanie dopiskiem witrynie sieci Web opartych na szynach Azure za pomocą maszyny wirtualnej Linux."
    services="virtual-machines-linux"
    documentationCenter="ruby"
    authors="rmcmurray"
    manager="wpickett"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="web"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="ruby"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="ruby-on-rails-web-application-on-an-azure-vm"></a>Dopiskiem w aplikacji sieci Web szyn na maszyn wirtualnych Azure

Ten samouczek pokazano, jak udostępniać dopiskiem w witrynie sieci Web szyny Azure za pomocą maszyny wirtualnej Linux.  

Ten samouczek został sprawdzony za pomocą KÓW 14.04 serwera Ubuntu. Jeśli używasz innego przydziału Linux, może być konieczne modyfikowanie kroki, aby zainstalować szyny.

> [AZURE.IMPORTANT] Azure występują dwa modele rozmieszczania służące do tworzenia i pracy z zasobami: [Menedżer zasobów i klasyczny](../../../resource-manager-deployment-model.md).  W tym artykule opisano, jak przy użyciu modelu Klasyczny wdrożenia. Firma Microsoft zaleca się, że większość nowych wdrożeń za pomocą modelu Menedżera zasobów.

## <a name="create-an-azure-vm"></a>Tworzenie Azure maszyn wirtualnych

Zacznij od tworzenia maszyn wirtualnych Azure z obrazem Linux.

Aby utworzyć maszyn wirtualnych, możesz użyć Azure portal klasyczny lub interfejsu wiersza polecenia Azure (polecenie).

### <a name="azure-management-portal"></a>Portal Azure zarządzania

1. Zaloguj się do [portalu klasyczny Azure](http://manage.windowsazure.com)
2. Kliknij przycisk **Nowy** > **obliczyć** > **maszyn wirtualnych** > **Szybkie tworzenie**. Wybierz obraz Linux.
3. Wprowadź hasło.

Po maszyn wirtualnych jest obsługi administracyjnej, kliknij nazwę maszyn wirtualnych, a następnie kliknij pozycję **pulpit nawigacyjny**. Znajdź punkt końcowy SSH, wymienione w obszarze **Szczegóły SSH**.

### <a name="azure-cli"></a>Polecenie Azure

Postępuj zgodnie z instrukcjami w sekcji [Tworzenie maszyn wirtualnych systemem Linux][vm-instructions].

Po maszyn wirtualnych jest obsługi administracyjnej, zostanie wyświetlony punkt końcowy SSH, uruchamiając następujące polecenie:

    azure vm endpoint list <vm-name>  

## <a name="install-ruby-on-rails"></a>Instalowanie dopiskiem przy użyciu prowadnic

1. Nawiązywanie połączenia z maszyn wirtualnych za pomocą SSH.

2. Od sesji SSH należy zainstalować dopiskiem na maszyn wirtualnych następujące polecenia:

        sudo apt-get update -y
        sudo apt-get upgrade -y
        sudo apt-get install ruby ruby-dev build-essential libsqlite3-dev zlib1g-dev nodejs -y

    Instalacja może potrwać kilka minut. Po zakończeniu, użyj następującego polecenia, aby sprawdzić, czy jest zainstalowany, dopiskiem:

        ruby -v

    To jest zwracana wersja dopiskiem, która została zainstalowana.

3. Aby zainstalować szyny, użyj następującego polecenia:

        sudo gem install rails --no-rdoc --no-ri -V

    Użyj — nie rdoc i — nie ri flagi, które należy pominąć instalowania dokumentacji, co jest szybsze.
    To polecenie prawdopodobnie zajmie dużo czasu na wykonanie, dodając -V zostaną wyświetlone informacje o postępie instalacji.

## <a name="create-and-run-an-app"></a>Tworzenie i uruchamianie aplikacji

Gdy nadal zalogowany za pośrednictwem SSH, uruchom następujące polecenia:

    rails new myapp
    cd myapp
    rails server -b 0.0.0.0 -p 3000

Polecenie [Nowy](http://guides.rubyonrails.org/command_line.html#rails-new) tworzy nową aplikację szyny. Polecenie [serwera](http://guides.rubyonrails.org/command_line.html#rails-server) uruchamia serwer sieci web WEBrick dołączonej szyny. (Komercyjny, czy prawdopodobnie chcesz użyć innego serwera, takie jak Unicorn lub osób.)

Powinien zostać wyświetlony wynik podobny do następującego.

    => Booting WEBrick
    => Rails 4.2.1 application starting in development on http://0.0.0.0:3000
    => Run `rails server -h` for more startup options
    => Ctrl-C to shutdown server
    [2015-06-09 23:34:23] INFO  WEBrick 1.3.1
    [2015-06-09 23:34:23] INFO  ruby 1.9.3 (2013-11-22) [x86_64-linux]
    [2015-06-09 23:34:23] INFO  WEBrick::HTTPServer#start: pid=27766 port=3000

## <a name="add-an-endpoint"></a>Dodawanie punktu końcowego

1. Przejdź do [portalu klasyczny Azure] [ management-portal] i wybierz pozycję usługi maszyn wirtualnych.

    ![Lista maszyn wirtualnych][vmlist]

2. Wybierz pozycję **punkty końcowe** w górnej części strony, a następnie kliknij **+ dodać punkt końcowy** w dolnej części strony.

    ![Strona punkty końcowe][endpoints]

3. W oknie dialogowym **Dodawanie punktu KOŃCOWEGO** wybierz pozycję "Dodaj punkt końcowy autonomicznego", a następnie kliknij przycisk **Następna** strzałka.

    ![okno dialogowe Nowy punkt końcowy][new-endpoint1]

3. Na następnej stronie okno dialogowe Wprowadź następujące informacje:

    * **Nazwa**: HTTP

    * **Protokół**: TCP

    * **PORT PUBLICZNY**: 80

    * **PORT prywatny**: 3000

    Spowoduje to utworzenie publicznej portu 80, który będzie skierować ruch do prywatnych portu 3000, gdzie oczekuje na serwerze szyny.

    ![okno dialogowe Nowy punkt końcowy][new-endpoint]

4. Kliknij znacznik wyboru, aby zapisać punkt końcowy.

5. Powinien pojawić się komunikat z informacją, **Aktualizacji w toku**. Po zniknięciu ten komunikat punkt końcowy jest aktywny. Można teraz test aplikacji, przechodząc do nazwy DNS komputera wirtualnych. Witryny sieci Web powinna wyglądać podobnie do następującej:

    ![domyślne strony szyny][default-rails-cloud]

## <a name="next-steps"></a>Następne kroki

W tym samouczku zarejestrowano większość czynności ręcznie. W środowisku produkcyjnym czy zapisać na komputerze opracowywania aplikacji i Wdroż maszyn wirtualnych Azure. Ponadto większość środowisku produkcyjnym hostem aplikacji szyny w połączeniu z inny proces serwera, takich jak Apache lub NginX, które uchwyty żądanie kierowaniu do wielu wystąpień aplikacji szyny i obsługujących statyczne zasobów. Aby uzyskać więcej informacji zobacz http://rubyonrails.org/deploy/.

Aby dowiedzieć się więcej na temat dopiskiem przy użyciu prowadnic, odwiedź stronę [dopiskiem na prowadnice szyny][rails-guides].

Aby korzystać z usług Azure z dopiskiem fonetycznym aplikacji, zobacz:

* [Przechowywanie niestrukturalne danych przy użyciu obiektów blob][blobs]

* [Pary klucz wartość magazynu przy użyciu tabel][tables]

* [Udostępniać zawartość dużej przepustowości sieci dostarczania zawartości][cdn-howto]

<!-- WA.com links -->
[blobs]: ../../../storage/storage-ruby-how-to-use-blob-storage.md
[cdn-howto]: https://azure.microsoft.com/develop/ruby/app-services/
[management-portal]: https://manage.windowsazure.com/
[tables]: ../../../storage/storage-ruby-how-to-use-table-storage.md
[vm-instructions]: ../../virtual-machines-linux-classic-createportal.md

<!-- External Links -->
[rails-guides]: http://guides.rubyonrails.org/
[sqlite3]: http://www.sqlite.org/

<!-- Images -->

[default-rails-cloud]: ./media/virtual-machines-linux-classic-ruby-rails-web-app/basicrailscloud.png
[vmlist]: ./media/virtual-machines-linux-classic-ruby-rails-web-app/vmlist.png
[endpoints]: ./media/virtual-machines-linux-classic-ruby-rails-web-app/endpoints.png
[new-endpoint]: ./media/virtual-machines-linux-classic-ruby-rails-web-app/newendpoint.png
[new-endpoint1]: ./media/virtual-machines-linux-classic-ruby-rails-web-app/newendpoint1.png
