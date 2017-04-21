Wykonaj poniższe czynności, aby zainstalować i uruchomić na komputerze wirtualnych systemem CentOS Linux MongoDB.

> [AZURE.WARNING] Funkcje zabezpieczeń MongoDB, takie jak uwierzytelnianie i powiązanie adresu IP, nie są domyślnie włączone. Funkcje zabezpieczeń powinna być włączona przed wdrożeniem MongoDB środowisku produkcyjnym.  Aby uzyskać więcej informacji, zobacz [zabezpieczeń i uwierzytelniania](http://www.mongodb.org/display/DOCS/Security+and+Authentication) .

1. Konfigurowanie systemu zarządzania pakietu (YUM) tak, aby można było zainstalować MongoDB. Utwórz plik */etc/yum.repos.d/10gen.repo* do przechowywania informacji o repozytorium, a następnie dodaj następujące:

        [10gen]
        name=10gen Repository
        baseurl=http://downloads-distro.mongodb.org/repo/redhat/os/x86_64
        gpgcheck=0
        enabled=1

2. Zapisz plik repo, a następnie uruchom następujące polecenie, aby zaktualizować pakiet lokalny bazy danych:

        $ sudo yum update

3. Aby zainstalować pakiet, uruchom następujące polecenie, aby zainstalować najnowszą wersję MongoDB i powiązane z nim narzędzia:

        $ sudo yum install mongo-10gen mongo-10gen-server

    Czekaj MongoDB pobieranie i instalowanie.

4. Utwórz katalog danych. Domyślnie MongoDB są przechowywane dane w katalogu */data/db* , ale musisz utworzyć ten katalog. Aby utworzyć go, uruchom polecenie:

        $ sudo mkdir -p /srv/datadrive/data
        $ sudo chown `id -u` /srv/datadrive/data

    Aby uzyskać więcej informacji dotyczących instalowania MongoDB w systemie Linux, zobacz [Szybki Start systemu Unix][QuickstartUnix].

5. Aby utworzyć bazę danych, uruchom:

        $ mongod --dbpath /srv/datadrive/data --logpath /srv/datadrive/data/mongod.log

    Wszystkie wiadomości dziennika będzie przekierowywany do pliku */srv/datadrive/data/mongod.log* serwera MongoDB rozpoczęciu i preallocates pliki dziennika. Może potrwać kilka minut MongoDB do przydzielenia pliki dziennika i Zacznij słuchać połączeń.

6. Aby rozpocząć powłoki administracyjnej MongoDB, Otwórz w osobnym oknie SSH lub Kit i uruchom:

        $ mongo
        > db.foo.save ( { a:1 } )
        > db.foo.find()
        { _id : ..., a : 1 }
        > show dbs  
        ...
        > show collections  
        ...  
        > help  

    Baza danych została utworzona przez Wstaw.

7. Po zainstalowaniu MongoDB punktu końcowego należy skonfigurować tak, aby MongoDB może być dostępna zdalnie. W portalu zarządzania kliknij **maszyn wirtualnych**, a następnie kliknij nazwę komputera wirtualnych nowy, a następnie kliknij pozycję **punkty końcowe**.
    
    ![Punkty końcowe][Image7]

8. Kliknij przycisk **Dodaj punktu końcowego** w dolnej części strony.
    
    ![Punkty końcowe][Image8]

9. Dodawanie punktu końcowego za pomocą następujących ustawień:

 - **Nazwa**: Mongo
 - **Protokół**: TCP
 - **Port publiczny**: 27017
 - **Port prywatny**: 27017
 
 Dzięki temu będzie MongoDB ma być dostępna zdalnie.



[QuickStartUnix]: http://www.mongodb.org/display/DOCS/Quickstart+Unix


[Image7]: ./media/install-and-run-mongo-on-centos-vm/LinuxVmAddEndpoint.png
[Image8]: ./media/install-and-run-mongo-on-centos-vm/LinuxVmAddEndpoint2.png
