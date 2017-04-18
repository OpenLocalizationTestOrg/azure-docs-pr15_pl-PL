<properties
    pageTitle="Tworzenie aplikacji sieci web PHP MySQL w usłudze Azure aplikacji i wdrażanie przy użyciu cyfra"
    description="Samouczek, którą przedstawiono sposób tworzenia aplikacji sieci web PHP, zawierający danych MySQL i wdrażania cyfra Azure za pomocą."
    services="app-service\web"
    documentationCenter="php"
    authors="rmcmurray"
    manager="wpickett"
    editor=""
    tags="mysql"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="PHP"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="create-a-php-mysql-web-app-in-azure-app-service-and-deploy-using-git"></a>Tworzenie aplikacji sieci web PHP MySQL w usłudze Azure aplikacji i wdrażanie przy użyciu cyfra

W tym samouczku pokazano sposób tworzenia aplikacji sieci web programu PHP MySQL oraz jak wdrożyć go do [Aplikacji usługi](http://go.microsoft.com/fwlink/?LinkId=529714) przy użyciu cyfra. Użyje [PHP][install-php], narzędzie wiersza polecenia MySQL (część [MySQL][install-mysql]), a [cyfra] [ install-git] zainstalowany na Twoim komputerze. Z instrukcjami podanymi w tym samouczku mogą występować we wszystkich systemach operacyjnych, łącznie z systemem Windows, Mac i Linux. Po wykonaniu tego przewodnika, konieczne będzie aplikacji sieci web PHP-MySQL działa Azure.

Dowiesz się:

* Jak utworzyć aplikację sieci web i bazą danych MySQL za pomocą [Azure Portal][management-portal]. Ponieważ PHP jest domyślnie włączona w [Aplikacjach sieci Web usługi aplikacji](http://go.microsoft.com/fwlink/?LinkId=529714) , nic się nie specjalnych jest wymagane do uruchomienia kodzie PHP.
* Jak opublikować i ponowne publikowanie aplikacji Azure za pomocą cyfra.
* Jak włączyć rozszerzenia Kompozytor zautomatyzować Kompozytor zadań w każdej `git push`.

Wykonując tego samouczka, zostanie utworzona w PHP aplikacji sieci web prosty rejestracji. Aplikacja będzie obsługiwana w aplikacjach sieci Web. Zrzut ekranu przedstawiający wypełniony wniosek jest poniżej:

![Azure PHP witryny sieci web][running-app]

## <a name="set-up-the-development-environment"></a>Konfigurowanie środowiska projektowego

Tego samouczka przyjęto założenie, masz [PHP][install-php], narzędzie wiersza polecenia MySQL (część [MySQL][install-mysql]), a [cyfra] [ install-git] zainstalowany na Twoim komputerze.

<a id="create-web-site-and-set-up-git"></a>
## <a name="create-a-web-app-and-set-up-git-publishing"></a>Tworzenie aplikacji sieci web i Konfigurowanie publikowania cyfra

Wykonaj poniższe czynności, aby utworzyć aplikację sieci web i bazą danych MySQL:

1. Zaloguj się do [portalu Azure][management-portal].
2. Kliknij ikonę **Nowy** .

3. Kliknij pozycję **Zobacz wszystkie** obok **Marketplace**. 

4. Kliknij pozycję **Web + Mobile**, a następnie **aplikacji + MySQL w sieci Web**. Następnie kliknij przycisk **Utwórz**.

4. Wprowadź prawidłową nazwę grupy zasobów.

    ![Nazwa grupy zasobów zestawu][resource-group]

5. Wprowadź wartości dla nowej aplikacji sieci web.

    ![Tworzenie aplikacji sieci web][new-web-app]

6. Wprowadź wartości dla nowej bazy danych, w tym zgodę na warunki prawne.

    ![Tworzenie nowej bazy danych MySQL][new-mysql-db]

7. Po utworzeniu aplikacji sieci web zostanie wyświetlony nowy karta aplikacji sieci web.

7. W obszarze **Ustawienia** kliknij pozycję **Ciągły**wdrożenia, a następnie wybierz polecenie _Konfiguruj wymagane ustawienia_.

    ![Konfigurowanie publikowania cyfra][setup-publishing]

8. Wybierz pozycję **Lokalny repozytorium cyfra** źródła.

    ![Konfigurowanie repozytorium cyfra][setup-repository]


9. Aby włączyć publikowanie cyfra, musisz podać nazwę użytkownika i hasło. Zanotuj nazwę użytkownika i hasło, które można utworzyć. (Jeśli masz skonfigurowane repozytorium cyfra przed, w tym kroku zostaną pominięte).

    ![Tworzenie publikacji poświadczeń][credentials]


## <a name="get-remote-mysql-connection-information"></a>Uzyskiwanie informacji o połączeniu zdalnym MySQL

Do połączenia z bazą danych MySQL, uruchomionym w aplikacjach sieci Web usługi będą potrzebne informacje o połączeniu. Aby uzyskać informacje o połączeniu MySQL, wykonaj następujące czynności:

1. Z grupy zasobów kliknij odpowiednią bazę danych:

    ![Wybierz bazę danych][select-database]

2. Bazy danych **Ustawienia**i wybierz polecenie **Właściwości**.

    ![Wybierz pozycję Właściwości][select-properties]

2. Zanotuj wartości `Database`, `Host`, `User Id`, i `Password`.

    ![Właściwości notatek][note-properties]

## <a name="build-and-test-your-app-locally"></a>Tworzenie i testowanie aplikacji lokalnie

Teraz, gdy użytkownik utworzył aplikację sieci web, można opracowywać aplikacji lokalnie, a następnie wdrożyć go po przetestowaniu.

Aplikacja rejestracji jest proste aplikacji PHP, która umożliwia rejestrowanie zdarzenia podając Twoja nazwa i adres e-mail. Informacje dotyczące poprzednich zarejestrowane są wyświetlane w tabeli. Rejestracja informacje są przechowywane w bazie danych MySQL. Aplikacja składa się z jednego pliku (skopiować i wkleić kod dostępne poniżej):

* **index.php**: zostanie wyświetlony formularz rejestracji i Tabela zawierająca informacje o rejestratorem.

Aby utworzyć i uruchomić aplikację lokalnie, wykonaj poniższe czynności. Należy zauważyć, że w tej procedurze założono PHP i narzędzie wiersza polecenia MySQL (część MySQL) na komputerze lokalnym i czy włączono [rozszerzenia chroniona nazwa pochodzenia MySQL][pdo-mysql].

1. Nawiązywanie połączenia z serwera zdalnego MySQL, za pomocą wartość w polu `Data Source`, `User Id`, `Password`, i `Database` którego został pobrany wcześniej:

        mysql -h{Data Source] -u[User Id] -p[Password] -D[Database]

2. Wiersz polecenia MySQL pojawią się:

        mysql>

3. Wklej w następujących `CREATE TABLE` polecenie, aby utworzyć `registration_tbl` tabeli w bazie danych:

        CREATE TABLE registration_tbl(id INT NOT NULL AUTO_INCREMENT, PRIMARY KEY(id), name VARCHAR(30), email VARCHAR(30), date DATE);

4. W katalogu głównym folderu aplikacji lokalnej Utwórz plik **index.php** .

5. Otwórz plik **index.php** w edytorze tekstów lub IDE, Dodaj następujący kod i wykonaj niezbędne zmiany, które są oznaczone symbolem `//TODO:` komentarze.


        <html>
        <head>
        <Title>Registration Form</Title>
        <style type="text/css">
            body { background-color: #fff; border-top: solid 10px #000;
                color: #333; font-size: .85em; margin: 20; padding: 20;
                font-family: "Segoe UI", Verdana, Helvetica, Sans-Serif;
            }
            h1, h2, h3,{ color: #000; margin-bottom: 0; padding-bottom: 0; }
            h1 { font-size: 2em; }
            h2 { font-size: 1.75em; }
            h3 { font-size: 1.2em; }
            table { margin-top: 0.75em; }
            th { font-size: 1.2em; text-align: left; border: none; padding-left: 0; }
            td { padding: 0.25em 2em 0.25em 0em; border: 0 none; }
        </style>
        </head>
        <body>
        <h1>Register here!</h1>
        <p>Fill in your name and email address, then click <strong>Submit</strong> to register.</p>
        <form method="post" action="index.php" enctype="multipart/form-data" >
              Name  <input type="text" name="name" id="name"/></br>
              Email <input type="text" name="email" id="email"/></br>
              <input type="submit" name="submit" value="Submit" />
        </form>
        <?php
            // DB connection info
            //TODO: Update the values for $host, $user, $pwd, and $db
            //using the values you retrieved earlier from the Azure Portal.
            $host = "value of Data Source";
            $user = "value of User Id";
            $pwd = "value of Password";
            $db = "value of Database";
            // Connect to database.
            try {
                $conn = new PDO( "mysql:host=$host;dbname=$db", $user, $pwd);
                $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );
            }
            catch(Exception $e){
                die(var_dump($e));
            }
            // Insert registration info
            if(!empty($_POST)) {
            try {
                $name = $_POST['name'];
                $email = $_POST['email'];
                $date = date("Y-m-d");
                // Insert data
                $sql_insert = "INSERT INTO registration_tbl (name, email, date)
                           VALUES (?,?,?)";
                $stmt = $conn->prepare($sql_insert);
                $stmt->bindValue(1, $name);
                $stmt->bindValue(2, $email);
                $stmt->bindValue(3, $date);
                $stmt->execute();
            }
            catch(Exception $e) {
                die(var_dump($e));
            }
            echo "<h3>Your're registered!</h3>";
            }
            // Retrieve data
            $sql_select = "SELECT * FROM registration_tbl";
            $stmt = $conn->query($sql_select);
            $registrants = $stmt->fetchAll();
            if(count($registrants) > 0) {
                echo "<h2>People who are registered:</h2>";
                echo "<table>";
                echo "<tr><th>Name</th>";
                echo "<th>Email</th>";
                echo "<th>Date</th></tr>";
                foreach($registrants as $registrant) {
                    echo "<tr><td>".$registrant['name']."</td>";
                    echo "<td>".$registrant['email']."</td>";
                    echo "<td>".$registrant['date']."</td></tr>";
                }
                echo "</table>";
            } else {
                echo "<h3>No one is currently registered.</h3>";
            }
        ?>
        </body>
        </html>

4.  W terminal przejdź do folderu aplikacji i wpisz następujące polecenie:

        php -S localhost:8000

Teraz można przejść do **8000-** Aby przetestować aplikację.


## <a name="publish-your-app"></a>Publikowanie aplikacji

Po przetestowaniu aplikacji lokalnie, możesz opublikować go do aplikacji sieci Web za pomocą cyfra. Zostaną zainicjować lokalnego repozytorium cyfra i opublikować aplikację.

> [AZURE.NOTE]
> Są to te same kroki, wyświetlane w Portal Azure na końcu tworzenie aplikacji sieci web i konfigurowanie konta cyfra publikowania sekcji powyżej.

1. (Opcjonalnie)  Jeśli zapomniano lub Zagubione adresu URL zdalnego repostitory cyfra, przejdź do właściwości aplikacji sieci web na Azure Portal.

1. Otwórz GitBash (lub terminal, jeśli cyfra znajduje się w swojej `PATH`), zmienianie katalogów katalog główny aplikacji i uruchom następujące polecenia:

        git init
        git add .
        git commit -m "initial commit"
        git remote add azure [URL for remote repository]
        git push azure master

    Pojawi się monit o podanie hasła, utworzony wcześniej.

    ![Początkowa wypychanych Azure za pośrednictwem cyfra][git-initial-push]

2. Przejdź do **http://[site name].azurewebsites.net/index.php** , aby rozpocząć korzystanie z aplikacji (te informacje będą przechowywane na pulpit nawigacyjny konta):

    ![Azure PHP witryny sieci web][running-app]

Po opublikowaniu aplikacji, możesz rozpocząć wprowadzanie zmian i publikować za pomocą cyfra.

## <a name="publish-changes-to-your-app"></a>Publikowanie zmian w aplikacji

Aby opublikować zmiany w aplikacji, wykonaj następujące czynności:

1. Wprowadź zmiany w aplikacji lokalnie.
2. Otwórz GitBash (lub terminal it cyfra jest w Twojej `PATH`), zmienianie katalogów katalog główny aplikacji i uruchom następujące polecenia:

        git add .
        git commit -m "comment describing changes"
        git push azure master

    Pojawi się monit o podanie hasła, utworzony wcześniej.

    ![Naciśnięcie zmiany witryny Azure za pośrednictwem cyfra][git-change-push]

3. Przejdź do **http://[site name].azurewebsites.net/index.php** aplikacji i wszelkie zmiany wprowadzone przez użytkownika może być:

    ![Azure PHP witryny sieci web][running-app]

>[AZURE.NOTE] Jeśli chcesz rozpocząć pracę z Azure aplikacji usługi przed utworzeniem konta dla konta Azure, przejdź do [Spróbuj aplikacji usługi](http://go.microsoft.com/fwlink/?LinkId=523751), którym natychmiast można utworzyć aplikację sieci web krótkotrwałe starter w aplikacji usługi. Nie kart kredytowych wymagane; nie zobowiązania.

<a name="composer"></a>
## <a name="enable-composer-automation-with-the-composer-extension"></a>Włączanie automatyzacji Kompozytor z rozszerzeniem Kompozytor

Domyślnie procesu wdrażania cyfra w aplikacji usługi nic nie robi z composer.json, jeśli istnieje w projekcie PHP. Możesz włączyć obsługę przetwarzania podczas composer.json `git push` , włączając rozszerzenie kompozytor.

1. W swojej PHP sieci web karta aplikacji w [Azure portal][management-portal], kliknij pozycję **Narzędzia** > **rozszerzenia**.

    ![Ustawienia rozszerzenia Kompozytor][composer-extension-settings]

2. Kliknij przycisk **Dodaj**, a następnie kliknij pozycję **Kompozytor**.

    ![Dodawanie numeru wewnętrznego Kompozytor][composer-extension-add]
    
3. Kliknij **przycisk OK** , aby zaakceptować warunki prawne. Kliknij przycisk **OK** ponownie, aby dodać rozszerzenia.

    Karta **rozszerzenia zainstalowane** pojawi się teraz na rozszerzenie kompozytor.  
    ![Kompozytor rozszerzenie widoku][composer-extension-view]
    
4. Teraz wykonać `git add`, `git commit`, i `git push` , takich jak w poprzedniej sekcji. Pojawi się teraz, że Kompozytor instaluje zależności zdefiniowane w composer.json.

    ![Kompozytor rozszerzenia sukcesu][composer-extension-success]

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji zobacz [Centrum deweloperów PHP](/develop/php/).

<!-- URL List -->

[install-php]: http://www.php.net/manual/en/install.php
[install-SQLExpress]: http://www.microsoft.com/download/details.aspx?id=29062
[install-Drivers]: http://www.microsoft.com/download/details.aspx?id=20098
[install-git]: http://git-scm.com/
[install-mysql]: http://dev.mysql.com/downloads/mysql/
[pdo-mysql]: http://www.php.net/manual/en/ref.pdo-mysql.php
[management-portal]: https://portal.azure.com
[sql-database-editions]: http://msdn.microsoft.com/library/windowsazure/ee621788.aspx

<!-- IMG List -->

[running-app]: ./media/web-sites-php-mysql-deploy-use-git/running_app_2.png
[new-website]: ./media/web-sites-php-mysql-deploy-use-git/new_website2.png
[custom-create]: ./media/web-sites-php-mysql-deploy-use-git/create_web_mysql.png
[new-mysql-db]: ./media/web-sites-php-mysql-deploy-use-git/create_db.png
[go-to-webapp]: ./media/web-sites-php-mysql-deploy-use-git/select_webapp.png
[setup-git-publishing]: ./media/web-sites-php-mysql-deploy-use-git/setup_git_publishing.png
[credentials]: ./media/web-sites-php-mysql-deploy-use-git/save_credentials.png
[resource-group]: ./media/web-sites-php-mysql-deploy-use-git/set_group.png
[new-web-app]: ./media/web-sites-php-mysql-deploy-use-git/create_wa.png
[setup-publishing]: ./media/web-sites-php-mysql-deploy-use-git/setup_deploy.png
[setup-repository]: ./media/web-sites-php-mysql-deploy-use-git/select_local_git.png
[select-database]: ./media/web-sites-php-mysql-deploy-use-git/select_database.png
[select-properties]: ./media/web-sites-php-mysql-deploy-use-git/select_properties.png
[note-properties]: ./media/web-sites-php-mysql-deploy-use-git/note-properties.png

[git-instructions]: ./media/web-sites-php-mysql-deploy-use-git/git-instructions.png
[git-change-push]: ./media/web-sites-php-mysql-deploy-use-git/php-git-change-push.png
[git-initial-push]: ./media/web-sites-php-mysql-deploy-use-git/php-git-initial-push.png

[composer-extension-settings]: ./media/web-sites-php-mysql-deploy-use-git/composer-extension-settings.png
[composer-extension-add]: ./media/web-sites-php-mysql-deploy-use-git/composer-extension-add.png
[composer-extension-view]: ./media/web-sites-php-mysql-deploy-use-git/composer-extension-view.png
[composer-extension-success]: ./media/web-sites-php-mysql-deploy-use-git/composer-extension-success.png
