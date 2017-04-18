<properties
    pageTitle="Konfigurowanie Tomcat Apache na maszyny Linux | Microsoft Azure"
    description="Dowiedz się, jak skonfigurować Apache Tomcat7 przy użyciu Azure maszyny wirtualnej (maszyn wirtualnych) systemem Linux."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="NingKuang"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/15/2015"
    ms.author="ningk"/>

#<a name="how-to-set-up-tomcat7-on-a-linux-virtual-machine-with-microsoft-azure"></a>Jak skonfigurować Tomcat7 na komputerze wirtualnych Linux z platformy Microsoft Azure

Apache Tomcat (lub po prostu Tomcat, wcześniej również Jakarta Tomcat) jest serwer sieci web Otwórz źródło i kontener servlet opracowanych przez Apache Software Foundation (ASF). Tomcat wykonuje Java Servlet i specyfikacji JavaServer Pages (JSP) z Microsystems słońce i zapewnia wyłącznie Java HTTP środowisku serwera sieci web do uruchamiania kodu języka Java. W najprostszym konfiguracji Tomcat działa w proces składający się z jednego systemu operacyjnego. Ten proces jest uruchomiony środowiska Java (maszyny wirtualnej Java). Każdego żądania HTTP w przeglądarce, aby Tomcat jest przetwarzana jako oddzielnego wątku w procesie Tomcat.  

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


W tym przewodniku zainstalujesz tomcat7 na obrazie Linux i wdrożyć go w programie Microsoft Azure.  

Dowiesz się:  

-   Jak utworzyć maszyny wirtualnej platformy Azure.
-   Jak przygotować maszyny wirtualnej tomcat7.
-   Jak zainstalować tomcat7.

Przyjmuje się, że czytnik ma już subskrypcji usługi Azure.  Jeśli nie można utworzyć konto w bezpłatnej wersji próbnej u [http://azure.microsoft.com](https://azure.microsoft.com/). Jeśli masz subskrypcję MSDN, zobacz [Microsoft Azure specjalne ceny: MSDN, MPN i korzyści Bizspark](https://azure.microsoft.com/pricing/member-offers/msdn-benefits/?c=14-39). Aby dowiedzieć się więcej na temat Azure, zobacz [Co to jest Azure?](https://azure.microsoft.com/overview/what-is-azure/).

W tym temacie założono, że podstawowe znają tomcat i Linux.  

##<a name="phase-1-create-an-image"></a>Faza 1: Tworzenie obrazu
W tej fazie utworzysz maszyny wirtualnej platformy Azure za pomocą obrazu Linux.  

###<a name="step-1-generate-an-ssh-authentication-key"></a>Krok 1: Generowanie SSH klucz uwierzytelniania
SSH jest ważnym narzędziem dla administratorów. Jednak Konfigurowanie zabezpieczeń programu access na podstawie określona przez człowieka hasła nie jest dobrym rozwiązaniem. Złośliwych użytkowników można podzielić na podstawie nazwy użytkownika i hasła słabych systemu.

Dobra wiadomość jest taka, że sposób Pozostaw otwartą dostępu zdalnego i nie martwić się o hasła. Metoda polega na uwierzytelnianie za pomocą kryptograficzny asymetrycznym. Klucz prywatny użytkownika jest, w którym udziela uwierzytelniania. Można nawet zablokować konta użytkownika, aby uniemożliwić całkowicie uwierzytelniania hasła.

Zaletą ta metoda jest niepotrzebne różnych haseł do logowania się do różnych serwerów. Może przeprowadzać uwierzytelnianie za pomocą osobistych klucz prywatny na wszystkich serwerach, co uniemożliwi konieczności pamiętać kilka haseł.

Użytkownik może również logowania przy wymaganie podania hasła w ten sposób.

Wykonaj poniższe czynności, aby wygenerować klucz uwierzytelniania SSH.

1.  Pobieranie i instalowanie puttygen z następującej lokalizacji: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)
2.  Uruchom PUTTYGEN. EXE.
3.  Kliknij pozycję **Generuj** do generowania kluczy. W procesie można zwiększyć losowości, przesuwając kursor na pusty obszar w oknie.  
![][1]
4.  Po procesie Generuj Puttygen.exe zostanie wyświetlona klucza wygenerowane. Na przykład:  
![][2]
5.  Zaznacz i skopiuj klucz publiczny w **kluczu** i zapisz go w pliku o nazwie publicKey.pem. Nie klikaj **Zapisz klucz publiczny**, ponieważ format pliku zapisanego kluczem publicznym różni się od klucz publiczny, które będą.
6.  Kliknij przycisk **Zapisz klucz prywatny** , a następnie zapisz go w pliku o nazwie privateKey.ppk.

###<a name="step-2-create-the-image-in-the-azure-portal"></a>Krok 2: Tworzenie obrazu w Azure portal.
W [Azure portal](https://portal.azure.com/)kliknij przycisk **Nowy** na pasku zadań, aby utworzyć obraz, wybierając pozycję Obraz Linux, w zależności od potrzeb. W poniższym przykładzie użyto obrazu Ubuntu 14.04.
![][3]

W **Polu Nazwa hosta** Określ nazwę dla adresu URL, którego możesz i klientów internetowych użyje dostępu do tego komputera wirtualnych. Definiowanie w ostatniej części nazwę DNS, na przykład tomcatdemo i Azure wygeneruje adres URL jako tomcatdemo.cloudapp.net.  

**Klucz SSH uwierzytelniania**skopiuj wartość klucza z pliku **publicKey.pem** , który zawiera klucz publiczny wygenerowane przez puttygen.  
![][4]

Odpowiednio skonfigurować inne ustawienia, a następnie kliknij przycisk Utwórz.  

##<a name="phase-2-prepare-your-virtual-machine-for-tomcat7"></a>Faza 2: Przygotowywanie komputera wirtualnych Tomcat7
W tej fazy zostanie konfigurowania punktu końcowego dla ruchu tomcat, a następnie połącz się z nowym komputerze wirtualnych.
###<a name="step-1-open-the-http-port-to-allow-web-access"></a>Krok 1: Otwórz port HTTP, aby umożliwić dostęp w sieci web
Punkty końcowe w Azure składają się z protokołem (port TCP lub UDP), wraz z portem publiczne i prywatne. Port prywatny jest które nasłuchują na komputerze wirtualnych usługi. Port publicznej jest numer portu, którego usługa w chmurze Azure nasłuchują na zewnątrz ruchu przychodzącego, internetowych.  

TCP port 8080 jest domyślny numer portu, na które wykrywa tomcat. Otwieranie tego portu z punktem końcowym Azure umożliwia możesz i innych Internet klientom dostęp do tomcat stron.  

1.  W portalu Azure, kliknij przycisk **Przeglądaj** -> **maszyn wirtualnych**, a następnie kliknij pozycję maszyny wirtualnej, która została utworzona.  
![][5]
2.  Aby dodać punkt końcowy do komputera wirtualnych, kliknij pole **punktów końcowych** .
![][6]
3.  Kliknij przycisk **Dodaj**.  
    1.  **Punkt końcowy**wpisz nazwę dla punktu końcowego w punkt końcowy, a następnie wpisz 80 portu **Publicznej**.  

        Jeśli ustawisz do 80, nie musisz uwzględnić numer portu w adresie URL, który umożliwia dostęp do tomcat. Na przykład http://tomcatdemo.cloudapp.net.    

        Po ustawieniu na inną wartość, taka jak 81, musisz dodać numer portu do adres URL umożliwiający dostęp tomcat. Na przykład http://tomcatdemo.cloudapp.net:81 /.
    2.  Wpisz 8080 Port prywatny. Domyślnie tomcat wykrywa na TCP port 8080. Jeśli zmienisz odsłuchać domyślny port tomcat, należy zaktualizować Port prywatny jest taka sama, jak tomcat odsłuchać portu.  
    ![][7]

4.  Kliknij **przycisk OK** , aby dodać punkt końcowy do maszyn wirtualnych.



###<a name="step-2-connect-to-the-image-you-created"></a>Krok 2: Łączenie do obrazu, który został utworzony.
Możesz wybrać dowolne narzędzie SSH nawiązywania połączenia z komputera wirtualnych. W tym przykładzie używamy Kit.  

Najpierw uzyskać nazwy DNS komputera wirtualnych z portalu Azure. **Kliknij przycisk Przeglądaj,** -> **maszyn wirtualnych** -> Nazwa komputera wirtualnych -> **Właściwości**, a następnie w polu **Nazwa domeny** fragmentu **Właściwości** .  

Uzyskaj numer portu SSH połączenia z pola **SSH** . Oto przykład.  
![][8]

Pobierz Kit [tutaj](http://www.putty.org/) .  

Po pobraniu, kliknij plik wykonywalny Kit. EXE. Konfigurowanie opcji podstawowe o nazwie hosta i uzyskanego z właściwości komputera wirtualnych numer portu. Oto przykład:  
![][9]

W okienku po lewej stronie kliknij pozycję **połączenie** -> **SSH** -> **uwierzytelniania** , a następnie kliknij przycisk **Przeglądaj** , aby określić lokalizację pliku **privateKey.ppk** , który zawiera klucz prywatny wygenerowane przez puttygen na etapie 1: Tworzenie obrazu. Oto przykład:  
![][10]

Kliknij przycisk **Otwórz**. Użytkownik może otrzymywać alerty w oknie komunikatu. Jeśli skonfigurowano nazwę DNS i numer portu poprawnie, kliknij przycisk **Tak**.
![][11]  


Powinny zostać wyświetlone następujące czynności:  
![][12]

Wprowadź nazwę użytkownika, podczas tworzenia maszyny wirtualnej na etapie 1: Tworzenie obrazu. Zostanie wyświetlony podobną do następujących czynności:  
![][13]





##<a name="phase-3-install-software"></a>Faza 3: Instalowanie oprogramowania
W tej fazie instalacji języka Java, tomcat i inne składniki tomcat.  

###<a name="java-runtime-environment"></a>Języka Java
Tomcat jest napisany w języku Java. Istnieją dwa rodzaje Java Development Kit (JDKs) (OpenJDK i Oracle JDK), a następnie możesz wybrać ten, który ma być.  

>AZURE. Uwaga: Obu JDKs mieć niemal tego samego kodu klas w interfejsie API języka Java, ale jest faktycznie różny kod dla maszyny wirtualnej. Jeśli chodzi o bibliotekach, OpenJDK zwykle przy użyciu bibliotek otwarty podczas Oracle powoduje korzystanie z nich zamknięty. Ale Oracle JDK ma więcej klas i niektóre stałej usterek i Oracle JDK jest bardziej trwały niż OpenJDK.

Następujące polecenia Pobierz różnych JDKs.  

Otwórz jdk   

    sudo apt-get update  
    sudo apt-get install openjdk-7-jre  

Oracle jdk  

-   Aby pobrać JDK z witryny sieci Web programu Oracle:  

        wget --header "Cookie: oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/8u5-b13/jdk-8u5-linux-x64.tar.gz  

-   Aby utworzyć katalog pliki JDK:  

        sudo mkdir /usr/lib/jvm  

-   Aby wyodrębnić pliki JDK do katalogu/usr/biblioteki/maszyny wirtualnej Java /:  

        sudo tar -zxf jdk-8u5-linux-x64.tar.gz  -C /usr/lib/jvm/  

-   Aby ustawić Oracle JDK jako domyślną maszyny wirtualnej Java:  

        sudo update-alternatives --install /usr/bin/java java /usr/lib/jvm/jdk1.8.0_05/bin/java 100  
        sudo update-alternatives --install /usr/bin/javac javac /usr/lib/jvm/jdk1.8.0_05/bin/javac 100  

####<a name="test"></a>Test:
Za pomocą polecenia, tak aby sprawdzić, czy języka Java jest poprawnie zainstalowany:  

    java -version  

Jeśli zainstalowano jdk Otwórz powinien zostać wyświetlony komunikat podobny do następujących czynności:![][14]

Po zainstalowaniu programu oracle jdk powinien zostać wyświetlony komunikat podobny do następujących czynności:![][15]

###<a name="tomcat7"></a>Tomcat7
Aby zainstalować tomcat7 za pomocą następujące polecenie:  

    sudo apt-get install tomcat7  

Jeśli nie używasz tomcat7, za pomocą odpowiednich odmian tego polecenia.  

####<a name="test"></a>Test:

Aby sprawdzić, czy tomcat7 pomyślnie jest zainstalowany, przejdź do swojego serwera tomcat DNS nazwy (http://tomcatexample.cloudapp.net/ jest przykładowy adres URL w tym artykule). Jeśli zostanie wyświetlona strona tak, masz zainstalowany tomcat7 poprawne.
![][16]


###<a name="install-other-tomcat-components"></a>Instalowanie składników Tomcat
Istnieją inne składniki tomcat opcjonalne, które można również zainstalować.  

Aby wyświetlić wszystkie dostępne składniki za pomocą polecenia **sudo tomcat7 stanie pamięci podręcznej wyszukiwania** . Następujące polecenia są przykłady, aby zainstalować niektóre użyteczne części.  

    sudo apt-get install tomcat7-admin      #admin web applications
    sudo apt-get install tomcat7-user         #tools to create user instances  

##<a name="phase-4-configure-tomcat"></a>Faza 4: Konfigurowanie Tomcat
W tej fazie administrowania tomcat.
###<a name="start-and-stop-tomcat7"></a>Uruchamianie i zatrzymywanie tomcat7
Serwer tomcat7 rozpocznie się automatycznie po jego zainstalowaniu. Możesz również uruchomić ją samodzielnie przy użyciu następującego polecenia:   

    sudo /etc/init.d/tomcat7 start

Aby zatrzymać tomcat7:  

    sudo /etc/init.d/tomcat7 stop

Aby wyświetlić stan tomcat7:  

    sudo /etc/init.d/tomcat7 status

Aby ponownie uruchomić usługi tomcat:  

    sudo /etc/init.d/tomcat7 restart

###<a name="tomcat-administration"></a>Administracja Tomcat
Można edytować plik konfiguracji użytkownika Tomcat Konfigurowanie poświadczeń administratora przy użyciu następującego polecenia:  

    sudo vi  /etc/tomcat7/tomcat-users.xml   

Oto przykład:  
![][17]  

>AZURE. Uwaga: Tworzenie silnego hasła dla nazwy użytkownika administratora.  

Po zakończeniu edytowania tego pliku, należy ponownie uruchomić usługi tomcat7 przy użyciu następującego polecenia, aby upewnić się, że zmiany zostały wprowadzone:  

    sudo /etc/init.d/tomcat7 restart  

Otwórz przeglądarkę i wpisz adres URL **http://<your tomcat server DNS name>/Menedżera/html**. Na przykład w tym artykule adres URL jest http://tomcatexample.cloudapp.net/manager/html.  

Po nawiązaniu połączenia powinien zostać wyświetlony podobnie do następującej:  
![][18]

##<a name="common-issues"></a>Typowe problemy

###<a name="cant-access-the-virtual-machine-with-tomcat-and-moodle-from-the-internet"></a>Nie można uzyskać dostęp do maszyny wirtualnej z Tomcat i Moodle z Internetu

-   **Symptom**  
Tomcat jest uruchomiony, ale nie widać Tomcat domyślnej strony w przeglądarce.
-   **Możliwe główne wielkość liter**   
    1.  Port słuchać tomcat jest taki sam jak prywatne punktu końcowego komputera wirtualnych ruchu tomcat.  

        Sprawdź ustawienia punktu końcowego Port publicznych i prywatnych i upewnij się, że Port prywatny jest identyczny tomcat odsłuchać portu. Zobacz fazy 1: Tworzenie obrazu, aby uzyskać instrukcje dotyczące konfigurowania punktów końcowych do urządzenia wirtualnych.  

        Aby określić port słuchać tomcat, otwórz /etc/httpd/conf/httpd.conf (wersja release funkcję czerwony) lub /etc/tomcat7/server.xml (wersja Debian). Domyślnie port słuchać tomcat jest 8080. Oto przykład:  

            <Connector port="8080" protocol="HTTP/1.1"  connectionTimeout="20000"  URIEncoding="UTF-8"            redirectPort="8443" />  

        Jeśli używasz maszyny wirtualnej, takich jak Debian lub Ubuntu, jeśli chcesz zmienić domyślny port z Tomcat odsłuchać (na przykład 8081), należy również otworzyć portu dla systemu operacyjnego. Najpierw otwórz profilu:  

            sudo vi /etc/default/tomcat7  

        Następnie usuń komentarze ostatni wiersz i zmień "nie", "tak".  

            AUTHBIND=yes

    2.  Zapory wyłączył port słuchać tomcat.

        Jeśli widzisz Tomcat domyślnej strony z hosta lokalnego, problem jest prawdopodobnie, że portu, który jest słuchania przez tomcat jest blokowany przez zaporę. Narzędzie w3m do przeglądania stron sieci web. Następujące polecenia zainstalować w3m i przejdź do strony domyślnej Tomcat:  

            sudo yum  install w3m w3m-img
            w3m http://localhost:8080  

-   **Rozwiązanie**
    1. Jeśli odsłuchać tomcat port nie jest taki sam jak Port prywatny punktu końcowego ruchu maszyn wirtualnych, potrzebujesz zmienić Port prywatny jest taka sama, jak tomcat odsłuchać portu.   

    2.  Jeśli błąd jest powodowany przez zaporę i iptables, Dodaj następujące wiersze do /etc/sysconfig/iptables:  

            -A INPUT -p tcp -m tcp --dport 80 -j ACCEPT
            -A INPUT -p tcp -m tcp --dport 443 -j ACCEPT  

        Należy zauważyć, że drugi wiersz jest potrzebny tylko dla ruchu https.  

        Upewnij się, że jest to powyżej wszystkie wiersze, które globalnie chcesz ograniczyć dostęp, takie jak:  

            -A INPUT -j REJECT --reject-with icmp-host-prohibited  

        Aby ponownie załadować iptables, uruchom następujące polecenie:  

            service iptables restart  

        To przetestowano na CentOS 6.3.

###<a name="permission-denied-when-upload-you-project-files-to-varlibtomcat7webapps"></a>Odmowa uprawnień podczas przekazywania plików do /var/lib/tomcat7/webapps w projekcie /  

-   **Symptom**  
Korzystając z dowolnego klienta SFTP (na przykład FileZilla) nawiązać połączenie z tym komputerze wirtualnych i przejdź do /var/lib/tomcat7/webapps/do opublikowania witryny, zostanie wyświetlony komunikat o błędzie podobny do następującego:  

        status: Listing directory /var/lib/tomcat7/webapps
        Command:    put "C:\Users\liang\Desktop\info.jsp" "info.jsp"
        Error:  /var/lib/tomcat7/webapps/info.jsp: open for write: permission denied
        Error:  File transfer failed

-   **Możliwe główne wielkość liter** Nie masz uprawnień dostępu do folderu /var/lib/tomcat7/webapps.  
-   **Rozwiązanie**  
Potrzebujesz uzyskiwanie uprawnień z konta administratora. Możesz zmienić własności tego folderu z głównego w nazwie użytkownika, używanej podczas inicjowania obsługi administracyjnej na komputerze. Oto przykład nazwą konta azureuser:  

        sudo chown azureuser -R /var/lib/tomcat7/webapps

    Opcja -R zbyt stosowania uprawnień dla wszystkich plików w katalogu.  

    Zauważ, że to polecenie działa także w przypadku katalogów. Opcja -R zmienia uprawnienia wszystkie pliki i katalogi w katalogu. Oto przykład:  

        sudo chown -R username:group directory  

    To polecenie zmienia własności (użytkowników i grup) dla wszystkie pliki i katalogi w katalogu i samego katalogu.  

    Następujące polecenie tylko zmian uprawnień katalogu folderu, ale pozostawia pliki i foldery w katalogu.  

        sudo chown username:group directory



[1]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-01.png
[2]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-02.png
[3]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-03.png
[4]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-04.png
[5]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-05.png
[6]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-06.png
[7]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-07.png
[8]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-08.png
[9]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-09.png
[10]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-10.png
[11]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-11.png
[12]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-12.png
[13]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-13.png
[14]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-14.png
[15]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-15.png
[16]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-16.png
[17]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-17.png
[18]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-18.png
