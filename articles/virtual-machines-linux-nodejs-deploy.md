<properties
   pageTitle="Wdrażanie aplikacji Node.js do maszyn wirtualnych Linux platformy Azure"
   description="Dowiedz się, jak wdrożyć aplikację Node.js do maszyn wirtualnych Linux platformy Azure."
   services=""
   documentationCenter="nodejs"
   authors="stepro"
   manager="dmitryr"
   editor=""/>

<tags
   ms.service="multiple"
   ms.devlang="nodejs"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="02/02/2016"
   ms.author="stephpr"/>

# <a name="deploy-a-nodejs-application-to-linux-virtual-machines-in-azure"></a>Wdrażanie aplikacji Node.js do maszyn wirtualnych Linux platformy Azure

Ten samouczek przedstawiono sposób przejdź do trybu Node.js i Wdroż działa Azure maszyn wirtualnych Linux. Z instrukcjami podanymi w tym samouczku mogą występować we wszystkich systemach operacyjnych, który jest możliwe uruchamianie Node.js.

Dowiesz się, jak:

- Rozwidlenia i klonowanie repozytorium GitHub zawierające prostej aplikacji zadania;
- Tworzenie i Konfigurowanie dwóch maszyn wirtualnych Linux w Azure, aby uruchomić aplikacji;
- Przejść na pasku aplikacji, naciskając aktualizacje maszyn wirtualnych sieci web serwera sieci Web.

> [AZURE.NOTE]
> Konieczne do wykonania tego samouczka, konto GitHub i konto Microsoft Azure i możliwość używania cyfra na komputerze rozwoju.

> Jeśli nie masz konta GitHub, możesz zalogować się [tutaj](https://github.com/join).

> Jeśli nie masz konta [Microsoft Azure](https://azure.microsoft.com/) , możesz Utwórz konto, dla bezpłatnej wersji próbnej [tutaj](https://azure.microsoft.com/pricing/free-trial/). To również przeprowadzi Cię przez proces tworzenia konta dla [Konta Microsoft](http://account.microsoft.com) Jeśli nie masz już jeden. Również w przypadku subskrybentów programu Visual Studio, można [aktywować swojej korzyści w witrynie MSDN](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

> Jeśli nie masz cyfra na tym komputerze rozwoju, jeśli korzystasz z komputera Macintosh lub Windows zainstalować cyfra, [tutaj](http://www.git-scm.com). Jeśli korzystasz z Linux, zainstaluj cyfra przy użyciu mechanizmu najbardziej odpowiedni, takich jak `sudo apt-get install git`.

## <a name="forking-and-cloning-the-todo-application"></a>Odpowiednio i klonowanie aplikacji zadania

Aplikacji zadania używane przez ten samouczek zawiera frontend prostej sieci web przez wystąpienie MongoDB, zawierający informacje o listy zadania. Po zalogowaniu się do GitHub, przejdź [tutaj](https://github.com/stepro/node-todo) znaleźć aplikacji i rozwidlenia go za pomocą łącza w prawym górnym rogu. To należy utworzyć repozytorium na koncie o nazwie /node-todo *Nazwa konta*.

Teraz na komputerze rozwoju klonowanie tego repozytorium:

    git clone https://github.com/accountname/node-todo.git

Użyjemy tej lokalne klonowanie repozytorium nieco później podczas wprowadzania zmian do kodu źródłowego.

## <a name="creating-and-configuring-the-linux-virtual-machines"></a>Tworzenie i konfigurowanie maszyn wirtualnych Linux

Azure obsługuje doskonałe dla nieprzetworzonych obliczeń przy użyciu maszyn wirtualnych Linux. Ta część samouczka pokazano, jak można łatwo aż dwóch maszyn wirtualnych Linux i wdrożyć aplikację zadania, uruchomionych web frontend jeden i wystąpienie MongoDB z drugiej strony.

### <a name="creating-virtual-machines"></a>Tworzenie maszyn wirtualnych

Najprostszym sposobem utworzenia nowej maszyny wirtualnej platformy Azure jest Azure Portal za pomocą. Kliknij [tutaj](https://portal.azure.com) zalogować się i uruchamianie Portal Azure w przeglądarce sieci web. Po załadowaniu Azure Portal, wykonaj następujące czynności:

- Kliknij łącze "+ nowe";
- Wybierz kategorię "Obliczeń", a następnie wybierz pozycję "Ubuntu serwera 14.04 KÓW";
- Wybierz odpowiedni model wdrażania "Menedżera zasobów", a następnie kliknij przycisk "Utwórz";
- Wypełnij podstawy następujące wskazówki:
  - Określ nazwę, którą można łatwo zidentyfikować później;
  - Ten samouczek wybierz pozycję uwierzytelniania hasła;
  - Tworzenie nowej grupy zasobów identyfikowalne nazwą.
- Rozmiar maszyn wirtualnych "A1 standardowy" w przypadku rozsądne wybór dla tego samouczka.
- Dodatkowe ustawienia upewnij się, że typ dysku jest "Standardowe" i Zaakceptuj wszystkie pozostałe ustawienia domyślne.
- Rozpoczynanie tworzenia na stronie podsumowania.

Wykonywanie powyższego procesu dwa razy, aby utworzyć dwa maszyn wirtualnych Linux, jedną dla serwera sieci web w sieci Web i jedną dla wystąpienia MongoDB. Tworzenie maszyn wirtualnych potrwa około 5-10 minut.

### <a name="assigning-a-dns-entry-for-virtual-machines"></a>Przypisywanie wpis DNS dla maszyn wirtualnych

Utworzony w Azure maszyn wirtualnych są domyślnie dostępne tylko publiczny adres IP, takich jak 1.2.3.4. Można wprowadzić maszyny identyfikacji przez przypisywanie ich wpisy DNS.

Gdy portalu wskazuje, czy zostały utworzone maszyn wirtualnych, kliknij łącze "Maszyn wirtualnych" w lewy pasek nawigacyjny i Znajdź komputery. Dla każdego komputera:

- Na karcie podstawowe informacje dotyczące odszukaj i kliknij pozycję publiczny adres IP;
- W publiczny adres IP Przypisz etykietę nazw DNS i Zapisz.

Portalu będziesz mieć pewność, że nazwa użytkownika jest dostępna. Po zapisaniu konfiguracji, maszyn wirtualnych będzie zawierać podobna do nazwy hosta `machinename.region.cloudapp.azure.com`.

### <a name="connecting-to-the-virtual-machines"></a>Nawiązywanie połączenia z maszyn wirtualnych

Gdy obsługa administracyjna została zainicjowana maszyn wirtualnych, były wstępnie skonfigurowany do zezwalania na połączenia zdalne na SSH. Jest to mechanizm, który użyjemy Konfigurowanie maszyn wirtualnych. Jeśli korzystasz z systemu Windows do rozwoju, będzie konieczne uzyskiwanie klienta SSH, jeśli nie masz już jeden. W tym miejscu typowych wybór jest Kit, który można pobrać [tutaj](http://www.chiark.greenend.org.uk/~sgtatham/putty/). Macintosh i systemów operacyjnych Linux okazać się przy użyciu wersji SSH preinstalowane.

### <a name="configuring-the-web-frontend-virtual-machine"></a>Konfigurowanie maszyny wirtualnej sieci Web serwera sieci Web

SSH do komputera serwera sieci Web w sieci web, utworzone za pomocą Kit, ssh wiersza polecenia lub innego Ulubione narzędzia SSH. Powinien zostać wyświetlony wiadomości powitalnej następuje wiersz polecenia.

Najpierw Przyjrzyjmy upewnij się, że cyfra i węzeł zainstalowano:

    sudo apt-get install -y git
    curl -sL https://deb.nodesource.com/setup_4.x | sudo -E bash -
    sudo apt-get install -y nodejs
    
Ponieważ aplikacji sieci web frontend zależy od niektóre natywnych moduły Node.js, potrzebujemy zainstalować podstawowe zestaw narzędzi kompilacji:

    sudo apt-get install -y build-essential

Na koniec załóżmy zainstalować aplikację Node.js o nazwie *zawsze*, co pozwala uruchamiania aplikacji serwera Node.js:

    sudo npm install -g forever
    
Są to wszystkie zależności potrzebne na tym komputerze wirtualnych, aby można było uruchomić aplikacji sieci web serwera sieci Web, więc korzystaj z systemem. W tym celu należy najpierw zostanie utworzony duplikat repozytorium GitHub, które wcześniej forked tak, aby łatwo publikować aktualizacje maszyn wirtualnych (opisane w tym scenariuszu aktualizacja później), a następnie klonowanie od zera klonowanie, aby zapewnić wersję repozytorium, które rzeczywiście mogą być wykonywane od zera.

Rozpoczynając od katalogu Narzędzia główne (~), uruchom następujące polecenia (zastępując *Nazwa konta* nazwę swojego konta użytkownika GitHub):

    git clone --bare https://github.com/accountname/node-todo.git
    git clone node-todo.git

Teraz wprowadź katalogu węzeł zadania i uruchom następujące polecenia:

    npm install
    forever start server.js
    
Działa teraz aplikacji sieci web serwera sieci Web, jednak jest jeszcze jeden krok, aby umożliwić dostęp do aplikacji z przeglądarki sieci web. Zasób Azure o nazwie *grupy zabezpieczeń sieci*, który został utworzony, gdy zainicjowano obsługę administracyjną maszyny wirtualnej maszyny wirtualnej utworzony jest chroniony. Obecnie tego zasobu umożliwia zewnętrznych żądania do portu 22 do maszyny wirtualnej, co umożliwia SSH komunikację z komputera, ale nic. Tak aby można było wyświetlić aplikacji zadania, która jest skonfigurowany do uruchomienia na porcie 8080, ten port także musi być otwarty w górę.

Wróć do Azure Portal i wykonaj następujące czynności:

- Polecenie "Grup zasobów" w lewy pasek nawigacyjny;
- Wybierz grupę zasobów, zawierającą komputera wirtualnych;
- W wyniku listę zasobów wybierz grupę zabezpieczeń sieciowych (jeden z ikoną tarczą);
- W oknie właściwości wybierz pozycję "reguły przychodzące zabezpieczeń";
- Na pasku narzędzi kliknij pozycję "Dodaj";
- Podaj nazwę like "domyślny Zezwalaj zadania";
- Ustawianie protokołu do "TCP";
- Ustaw zakres portu docelowego do "8080";
- Kliknij przycisk OK i poczekaj, aż reguły zabezpieczeń, który ma zostać utworzony.

Po utworzeniu tej reguły zabezpieczeń, aplikacja zadania jest widoczna publicznie w Internecie i możliwość przeglądania, na przykład przy użyciu adresu URL takich jak:

    http://machinename.region.cloudapp.azure.com:8080

Można zauważyć, że mimo że firma Microsoft nie zostały skonfigurowane maszyny wirtualnej MongoDB, aplikacja zadania wydaje dość. Jest tak, ponieważ repozytorium źródła jest ustalony, aby użyć wstępnie wdrożonym wystąpienie MongoDB. Gdy skonfigurowano możemy maszyny wirtualnej MongoDB, firma Microsoft przejdź wstecz i zmień kod źródłowy zamiast korzystanie z naszych prywatne wystąpienia MongoDB.

### <a name="configuring-the-mongodb-virtual-machine"></a>Konfigurowanie maszyny wirtualnej MongoDB

SSH na drugim komputerze, utworzone za pomocą Kit, ssh wiersza polecenia lub innego Ulubione narzędzia SSH. Przed wykonaniem wiadomości powitalnej i wiersz polecenia, zainstaluj MongoDB (te instrukcje zostały pobrane z [poniżej](https://docs.mongodb.org/master/tutorial/install-mongodb-on-ubuntu/)):

    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv EA312927
    echo "deb http://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.2.list
    sudo apt-get update
    sudo apt-get install -y mongodb-org

Domyślnie MongoDB skonfigurowano tak go jest możliwy tylko lokalnie. Ten samouczek możemy skonfiguruje MongoDB, są dostępne z maszyn wirtualnych aplikacji. W kontekście sudo, otwórz plik /etc/mongod.conf i Znajdź `# network interfaces` sekcji. Zmienianie `net.bindIp` wartość konfiguracji `0.0.0.0`.

> [AZURE.NOTE]
> Ta konfiguracja jest na potrzeby tego samouczka tylko. **Nie** jest ćwiczenia zabezpieczeń i nie może być używana w środowisku produkcyjnym.

Teraz zapewnia MongoDB usługa została uruchomiona:

    sudo service mongod restart

MongoDB działa przez port 27017 domyślnie. Tak w ten sam sposób firma Microsoft, aby otworzyć portu 8080 na komputerze wirtualnych sieci web serwera sieci Web, należy otworzyć port 27017 na komputerze wirtualnych MongoDB.

Wróć do Azure Portal i wykonaj następujące czynności:

* Polecenie "Grup zasobów" w lewy pasek nawigacyjny;
* Wybierz grupę zasobów, zawierającą maszyn wirtualnych MongoDB;
* W wyniku listę zasobów wybierz grupę zabezpieczeń sieciowych (jeden z ikoną tarczą) o takiej samej nazwie nadana maszyn wirtualnych MongoDB;
* W oknie właściwości wybierz pozycję "reguły przychodzące zabezpieczeń";
* Na pasku narzędzi kliknij pozycję "Dodaj";
* Podaj nazwę like "domyślny Zezwalaj mongo";
* Ustawianie protokołu do "TCP";
* Ustaw zakres portu docelowego do "27017";
* Kliknij przycisk OK i poczekaj, aż reguły zabezpieczeń, który ma zostać utworzony.

## <a name="iterating-on-the-todo-application"></a>Iterowanie na pasku aplikacji zadania
Pory, jest przygotowana dwóch maszyn wirtualnych Linux: jedną uruchomionym aplikacji sieci web serwera sieci Web i jedną która działa w przypadku wystąpienia MongoDB. Ale problemu — frontend sieci web nie jest faktycznie przy użyciu ustanawianie wystąpienia MongoDB jeszcze. Załóżmy rozwiązać ten problem, aktualizując kod frontend sieci web, aby użyć zmiennej środowiska zamiast wystąpieniem stałe.

### <a name="changing-the-todo-application"></a>Zmienianie aplikacji zadania

Na komputerze rozwoju, gdzie najpierw sklonowany repozytorium węzeł zadania, otwórz `node-todo/config/database.js` plik w edytorze Ulubione i zmień wartość adres url z stałe wartości, takie jak `mongodb://...` do `process.env.MONGODB`.

Zatwierdzić wprowadzone zmiany i push we wzorcu GitHub:

    git commit -am "Get MongoDB instance from env"
    git push origin master

Niestety to nie Publikuj zmiany maszyn wirtualnych sieci web serwera sieci Web. Na komputerze wirtualnych umożliwiające prostą, ale skutecznych mechanizm publikowanie aktualizacji, więc można szybko obserwować efekt zmian w środowisku produkcyjnym można wprowadzić kilka zmian więcej.

### <a name="configuring-the-web-frontend-virtual-machine"></a>Konfigurowanie maszyny wirtualnej sieci Web serwera sieci Web
Odwoływanie, możemy poprzednio utworzone od zera duplikat repozytorium węzeł zadania na komputerze wirtualnych sieci web serwera sieci Web. Okaże się utworzony za pomocą tej akcji zdalnego nowego cyfra, do której może być przyczyną zmiany. Jednak po prostu naciśnięcie do tym zdalnego dość nie udostępnia model szybkiego iteracji szukany deweloperów podczas pracy z kodu.

Chcemy można było zrobić to upewnij się, że w przypadku wystąpienia wypychanych zdalnego repozytorium na komputerze wirtualnej działającej aplikacji zadania jest automatycznie aktualizowana. Na szczęście jest łatwe do osiągnięcia z cyfra.

Cyfra udostępnia szereg haki, które są nazywane przez określony czas reakcji do działań podjętych w repozytorium. Są one wymienione w repozytorium za pomocą skryptów powłoki `hooks` folder. Hak, która jest najczęściej stosowana dla tego scenariusza automatycznej aktualizacji jest `post-update` zdarzenia.

W sesji SSH maszyn wirtualnych sieci web serwera sieci Web, zmień `~/node-todo.git/hooks` katalogu i dodaj następującą zawartość w pliku o nazwie `post-update` (zastępując `machinename` i `region` wraz z informacjami maszyn wirtualnych MongoDB):

    #!/bin/bash
    
    forever stopall
    unset 'GIT_DIR'
    export MONGODB="mongodb://machinename.region.cloudapp.azure.com:27017/tododb"
    cd ~/node-todo && git fetch origin && git pull origin master && npm install && forever start ~/node-todo/server.js
    exec git update-server-info
    
Upewnij się, że plik jest wykonywalny, uruchamiając następujące polecenie:

    chmod 755 post-update

Ten skrypt gwarantuje, że zatrzymaniu bieżącej aplikacji serwera, kod w repozytorium sklonowanym jest aktualizowana do najnowszych dowolnego zaktualizowanych zależności i ponownym uruchomieniu serwera. Zapewnia również, że środowisko został skonfigurowany w celu otrzymywania naszych pierwszej aktualizacji aplikacji przygotowania uzyskiwania wystąpienia MongoDB z zmiennej środowiska.

### <a name="configuring-your-development-machine"></a>Konfigurowanie komputera opracowywania
Teraz korzystaj komputera rozwoju podłączyć maszyn wirtualnych sieci web serwera sieci Web. Jest to prosty sposób dodawania od zera repozytorium na tym komputerze wirtualnych co zdalny. Uruchom następujące polecenie, aby wykonać tę czynność (zastępując *użytkownika* przy użyciu nazwy logowania maszyn wirtualnych serwera sieci Web w sieci web i *NazwaKomputera* i *region* , zależnie od potrzeb):

    git remote add azure user@machinename.region.cloudapp.azure.com:node-todo.git

To wszystko, potrzebne do Włącz naciśnięcie lub publikowania w celu zmiany maszyn wirtualnych sieci web serwera sieci Web.

### <a name="publishing-updates"></a>Publikowanie aktualizacji

Załóżmy opublikować jedną zmianę, której dokonano pory tak, aby aplikacja będzie korzystać własnego wystąpienia MongoDB:

    git push azure master

Powinien zostać wyświetlony wynik podobnie do następującej:

    Counting objects: 4, done.
    Delta compression using up to 4 threads.
    Compressing objects: 100% (3/3), done.
    Writing objects: 100% (4/4), 406 bytes | 0 bytes/s, done.
    Total 4 (delta 1), reused 0 (delta 0)
    remote: info:    Forever stopped processes:
    remote: data:        uid  command         script    forever pid  id logfile                         uptime
    remote: data:    [0] 0Lyh /usr/bin/nodejs server.js 9064    9301    /home/username/.forever/0Lyh.log 0:0:3:17.487
    remote: From /home/username/node-todo
    remote:    5f31fd7..5bc7be5  master     -> origin/master
    remote: From /home/username/node-todo
    remote:  * branch            master     -> FETCH_HEAD
    remote: Updating 5f31fd7..5bc7be5
    remote: Fast-forward
    remote:  config/database.js | 2 +-
    remote:  1 file changed, 1 insertion(+), 1 deletion(-)
    remote: npm WARN package.json node-todo@0.0.0 No repository field.
    remote: npm WARN package.json node-todo@0.0.0 No license field.
    remote: warn:    --minUptime not set. Defaulting to: 1000ms
    remote: warn:    --spinSleepTime not set. Your script will exit if it does not stay up for at least 1000ms
    remote: info:    Forever processing file: /home/username/node-todo/server.js
    To username@machinename.region.cloudapp.azure.com:node-todo.git
    5f31fd7..5bc7be5  master -> master

Po wykonaniu tego polecenia, Odśwież aplikacji w przeglądarce sieci web. Powinny być widoczne, że lista zadania, przedstawione w tym miejscu jest puste i nie jest już związany z udostępnionej wdrożonym wystąpieniem MongoDB.

Do zakończenia tego samouczka, Przejdźmy zmiany inny, widoczności. Na komputerze rozwoju Otwórz plik node-todo/public/index.html za pomocą edytora Ulubione. Znajdź nagłówka jumbotron i Zmień tytuł z "Jestem aholic zadania" do "Jestem aholic zadania Azure!".

Teraz Przyjrzyjmy zatwierdzenie:

    git commit -am "Azurify the title"

Ten czas, Przejdźmy publikowanie zmiana Azure przed naciśnięcie go, aby z powrotem do GitHub repo:

    git push azure master

Po wykonaniu tego polecenia Odśwież stronę sieci web i zmiany będą widoczne. Od ich wygląd, push zmiana powrót do początku zdalnego: 

    git push origin master

## <a name="next-steps"></a>Następne kroki
W tym artykule pokazano, jak pobrać aplikację Node.js i Wdroż Linux maszyn wirtualnych z systemem platformy Azure. Aby dowiedzieć się więcej na temat maszyn wirtualnych Linux platformy Azure, zobacz [Wprowadzenie do Linux Azure](/documentation/articles/virtual-machines-linux-introduction/).
    
Aby uzyskać więcej informacji na temat tworzenia aplikacji Node.js Azure, zobacz [Centrum deweloperów Node.js](/develop/nodejs/).
