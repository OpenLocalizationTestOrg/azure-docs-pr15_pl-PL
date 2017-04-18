<properties 
    pageTitle="Tworzenie aplikacji sieci web PHP MySQL w usłudze Azure aplikacji i wdrażanie przy użyciu FTP" 
    description="Samouczek, który przedstawia sposób tworzenia aplikacji sieci web PHP są przechowywane dane w MySQL i używanie wdrożenia FTP Azure." 
    services="app-service\web" 
    documentationCenter="php" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="PHP" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>


#<a name="create-a-php-mysql-web-app-in-azure-app-service-and-deploy-using-ftp"></a>Tworzenie aplikacji sieci web PHP MySQL w usłudze Azure aplikacji i wdrażanie przy użyciu FTP

W tym samouczku pokazano, jak utworzyć aplikację sieci web PHP MySQL i jak wdrożyć go przy użyciu FTP. Tego samouczka przyjęto założenie, masz [PHP][install-php], [MySQL][install-mysql], serwer sieci web, a klient FTP zainstalowany na Twoim komputerze. Z instrukcjami podanymi w tym samouczku mogą występować we wszystkich systemach operacyjnych, łącznie z systemem Windows, Mac i Linux. Po wykonaniu tego przewodnika, konieczne będzie aplikacji sieci web PHP-MySQL działa Azure.
 
Dowiesz się:

* Jak utworzyć aplikację sieci web i bazą danych MySQL przy użyciu Azure Portal. Ponieważ PHP jest domyślnie włączona w aplikacjach sieci Web, nic się nie specjalnych jest wymagane do uruchomienia kodzie PHP.
* Jak publikować aplikacji Azure za pomocą protokołu FTP.
 
Wykonując tego samouczka, zostanie utworzona w PHP aplikacji sieci web prosty rejestracji. Aplikacja będzie obsługiwana w aplikacji sieci Web. Zrzut ekranu przedstawiający wypełniony wniosek jest poniżej:

![Azure PHP witryny sieci Web][running-app]

>[AZURE.NOTE] Jeśli chcesz rozpocząć pracę z Azure aplikacji usługi przed utworzeniem konta dla konta, przejdź do [Spróbuj aplikacji usługi](http://go.microsoft.com/fwlink/?LinkId=523751), którym natychmiast można utworzyć aplikację sieci web krótkotrwałe starter w aplikacji usługi. Nie kart kredytowych wymagane, nie zobowiązań. 


##<a name="create-a-web-app-and-set-up-ftp-publishing"></a>Tworzenie aplikacji sieci web i konfigurowanie FTP do publikowania

Wykonaj poniższe czynności, aby utworzyć aplikację sieci web i bazą danych MySQL:

1. Zaloguj się do [portalu Azure][management-portal].
2. Kliknij ikonę **+ Nowa** w górnym lewym rogu Azure Portal.

    ![Tworzenie nowej witryny sieci Web Azure][new-website]

3. W polu Wyszukaj wpisz **aplikacji Web app + MySQL** , a następnie kliknij przycisk w **aplikacji sieci Web + MySQL**.

    ![Tworzenie niestandardowego nowej witryny sieci Web][custom-create]

4. Kliknij przycisk **Utwórz**. Wprowadź nazwę usługi aplikacji unikatowe, prawidłową nazwę grupy zasobów i nowy plan usług.

    ![Nazwa grupy zasobów zestawu][resource-group]


6. Wprowadź wartości dla nowej bazy danych, w tym zgodę na warunki prawne.

    ![Tworzenie nowej bazy danych MySQL][new-mysql-db]
    
7. Po utworzeniu aplikacji sieci web zostanie wyświetlony nowy karta usługi aplikacji.


6. Kliknij pozycję **Ustawienia** > **wdrażania poświadczeń**. 

    ![Ustaw poświadczenia wdrażania][set-deployment-credentials]

7. Aby włączyć publikowanie FTP, musisz podać nazwę użytkownika i hasło. Zapisywanie poświadczeń i zanotuj nazwę użytkownika i hasło, które można utworzyć.

    ![Tworzenie publikacji poświadczeń][portal-ftp-username-password]

##<a name="build-and-test-your-app-locally"></a>Tworzenie i testowanie aplikacji lokalnie

Aplikacja rejestracji jest proste aplikacji PHP, która umożliwia rejestrowanie zdarzenia podając Twoja nazwa i adres e-mail. Informacje dotyczące poprzednich zarejestrowane są wyświetlane w tabeli. Rejestracja informacje są przechowywane w bazie danych MySQL. Aplikacja składa się z dwóch plików:

* **index.php**: zostanie wyświetlony formularz rejestracji i Tabela zawierająca informacje o rejestratorem.
* **createtable.php**: tworzy tabelę MySQL dla aplikacji. Ten plik będzie używana tylko raz.

Aby utworzyć i uruchomić aplikację lokalnie, wykonaj poniższe czynności. Należy zauważyć, że w tej procedurze założono, masz PHP, MySQL, i na serwerze sieci web na komputerze lokalnym, a włączona [chroniona nazwa pochodzenia rozszerzenie MySQL][pdo-mysql].

1. Tworzenie bazy danych MySQL o nazwie `registration`. Można to zrobić z wiersza polecenia MySQL przy użyciu tego polecenia:

        mysql> create database registration;

2. W katalogu głównym serwera sieci web, Utwórz folder o nazwie `registration` i utworzyć dwa pliki w nim - jeden o nazwie `createtable.php` i jeden o nazwie `index.php`.

3. Otwórz `createtable.php` plik w edytorze tekstów lub IDE i dodawanie kodu poniżej. Kod, jaki będzie można używać do tworzenia `registration_tbl` w tabeli `registration` bazy danych.

        <?php
        // DB connection info
        $host = "localhost";
        $user = "user name";
        $pwd = "password";
        $db = "registration";
        try{
            $conn = new PDO( "mysql:host=$host;dbname=$db", $user, $pwd);
            $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );
            $sql = "CREATE TABLE registration_tbl(
                        id INT NOT NULL AUTO_INCREMENT, 
                        PRIMARY KEY(id),
                        name VARCHAR(30),
                        email VARCHAR(30),
                        date DATE)";
            $conn->query($sql);
        }
        catch(Exception $e){
            die(print_r($e));
        }
        echo "<h3>Table created.</h3>";
        ?>

    > [AZURE.NOTE] 
    > Należy zaktualizować wartości <code>$user</code> i <code>$pwd</code> z lokalnym MySQL nazwę użytkownika i hasło.

4. Otwórz przeglądarkę sieci web i przejdź do [http://localhost/registration/createtable.php][localhost-createtable]. Spowoduje to utworzenie `registration_tbl` tabeli w bazie danych.

5. Otwórz plik **index.php** w edytorze tekstów lub IDE i Dodaj podstawowe kod HTML i arkuszy CSS na stronie (kod PHP zostanie dodany w późniejszym czynności).

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

        ?>
        </body>
        </html>

6. W obrębie tagów PHP Dodaj kod PHP nawiązywania połączenia z bazą danych.

        // DB connection info
        $host = "localhost";
        $user = "user name";
        $pwd = "password";
        $db = "registration";
        // Connect to database.
        try {
            $conn = new PDO( "mysql:host=$host;dbname=$db", $user, $pwd);
            $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );
        }
        catch(Exception $e){
            die(var_dump($e));
        }

    > [AZURE.NOTE]
    > Ponownie, należy zaktualizować wartości <code>$user</code> i <code>$pwd</code> z lokalnym MySQL nazwę użytkownika i hasło.

7. Po kod połączenia bazy danych Dodaj kod służące do wstawiania informacji o rejestracji do bazy danych.

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

8. Ponadto po kodzie powyżej, Dodaj kod do pobierania danych z bazy danych.

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

Teraz można przejść do [http://localhost/registration/index.php] [ localhost-index] do testowania aplikacji.

##<a name="get-mysql-and-ftp-connection-information"></a>Uzyskiwanie informacji o połączeniu FTP i MySQL

Do połączenia z bazą danych MySQL, uruchomionym w aplikacjach sieci Web usługi będą potrzebne informacje o połączeniu. Aby uzyskać informacje o połączeniu MySQL, wykonaj następujące czynności:

1. Karta aplikacji sieci web z aplikacji usługi kliknij łącze grupa zasobów:

    ![Wybieranie grupy zasobów][select-resourcegroup]

1. Z grupy zasobów kliknij odpowiednią bazę danych:

    ![Wybierz bazę danych][select-database]

2. Podsumowanie bazy danych i wybierz pozycję **Ustawienia** > **Właściwości**.

    ![Wybierz pozycję Właściwości][select-properties]
    
2. Zanotuj wartości `Database`, `Host`, `User Id`, i `Password`.

    ![Właściwości notatek][note-properties]

3. W przypadku aplikacji sieci web kliknij łącze **Pobierz Publikowanie profilu** , w prawym dolnym rogu strony:

    ![Plik do pobrania Publikuj profil][download-publish-profile]

4. Otwórz `.publishsettings` plik w edytorze XML. 

3. Znajdowanie `<publishProfile >` element z `publishMethod="FTP"` wygląda podobnie do następującej:

        <publishProfile publishMethod="FTP" publishUrl="ftp://[mysite].azurewebsites.net/site/wwwroot" ftpPassiveMode="True" userName="[username]" userPWD="[password]" destinationAppUrl="http://[name].antdf0.antares-test.windows-int.net" 
            ...
        </publishProfile>
    
Zanotuj `publishUrl`, `userName`, i `userPWD` atrybutów.

##<a name="publish-your-app"></a>Publikowanie aplikacji

Po przetestowaniu aplikacji lokalnie, możesz opublikować go do aplikacji sieci web przy użyciu FTP. Jednak należy najpierw zaktualizuj informacje o połączeniu bazy danych w aplikacji. Za pomocą informacji o połączeniu bazy danych, które zostały uzyskane wcześniej (w sekcji **Uzyskiwanie MySQL oraz informacje o połączeniu FTP** ) zaktualizuj następujące informacje w **zarówno** `createdatabase.php` i `index.php` pliki z odpowiednimi wartościami:

    // DB connection info
    $host = "value of Data Source";
    $user = "value of User Id";
    $pwd = "value of Password";
    $db = "value of Database";

Teraz możesz przystąpić do opublikowania aplikacji przy użyciu FTP.

1. Otwórz klienta FTP wybranym.

2. Wprowadź *część nazwy hosta* z `publishUrl` atrybut powyższą do klienta FTP.

3. Wprowadź `userName` i `userPWD` do klienta FTP bez zmian atrybutów wspomniano powyżej.

4. Nawiąż połączenie.

Po połączeniu można przekazywanie i pobieranie plików w razie potrzeby. Upewnij się, że są przekazywanie plików do katalogu głównego, który jest `/site/wwwroot`.

Po przekazaniu oba `index.php` i `createtable.php`, przejdź do **http://[site name].azurewebsites.net/createtable.php** , aby utworzyć tabelę MySQL dla aplikacji, a następnie przejdź do **http://[site name].azurewebsites.net/index.php** , aby rozpocząć korzystanie z aplikacji.
 
## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji zobacz [Centrum deweloperów PHP](/develop/php/).

[install-php]: http://www.php.net/manual/en/install.php
[install-mysql]: http://dev.mysql.com/doc/refman/5.6/en/installing.html
[pdo-mysql]: http://www.php.net/manual/en/ref.pdo-mysql.php
[localhost-createtable]: http://localhost/tasklist/createtable.php
[localhost-index]: http://localhost/tasklist/index.php
[running-app]: ./media/web-sites-php-mysql-deploy-use-ftp/running_app_2.png
[new-website]: ./media/web-sites-php-mysql-deploy-use-ftp/new_website2.png
[custom-create]: ./media/web-sites-php-mysql-deploy-use-ftp/create_web_mysql.png
[website-details]: ./media/web-sites-php-web-site-mysql-deploy-use-ftp/website_details.jpg
[new-mysql-db]: ./media/web-sites-php-mysql-deploy-use-ftp/create_db.png
[go-to-webapp]: ./media/web-sites-php-mysql-deploy-use-ftp/select_webapp.png
[set-deployment-credentials]: ./media/web-sites-php-mysql-deploy-use-ftp/set_credentials.png
[portal-ftp-username-password]: ./media/web-sites-php-mysql-deploy-use-ftp/save_credentials.png
[resource-group]: ./media/web-sites-php-mysql-deploy-use-ftp/set_group.png
[new-web-app]: ./media/web-sites-php-mysql-deploy-use-ftp/create_wa.png
[select-database]: ./media/web-sites-php-mysql-deploy-use-ftp/select_database.png
[select-resourcegroup]: ./media/web-sites-php-mysql-deploy-use-ftp/select_resourcegroup.png
[select-properties]: ./media/web-sites-php-mysql-deploy-use-ftp/select_properties.png
[note-properties]: ./media/web-sites-php-mysql-deploy-use-ftp/note-properties.png

[connection-string-info]: ./media/web-sites-php-web-site-mysql-deploy-use-ftp/connection_string_info.png
[management-portal]: https://portal.azure.com
[download-publish-profile]: ./media/web-sites-php-mysql-deploy-use-ftp/download_publish_profile_3.png
 
