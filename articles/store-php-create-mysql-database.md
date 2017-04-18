<properties
    pageTitle="Tworzenie i nawiązywanie połączenia z bazą danych MySQL platformy Azure"
    description="Dowiedz się, jak utworzyć bazę danych MySQL, a następnie podłącz do niego za pomocą aplikacji sieci web PHP platformy Azure za pomocą portalu Azure."
    documentationCenter="php"
    services="app-service\web"
    authors="cephalin"
    manager="wpickett"
    editor=""
    tags="mysql"/>

<tags
    ms.service="multiple"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="PHP"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm;cephalin"/>

# <a name="create-and-connect-to-a-mysql-database-in-azure"></a>Tworzenie i nawiązywanie połączenia z bazą danych MySQL platformy Azure

W tym samouczku pokazano sposób tworzenia bazy danych MySQL w [Azure portal](https://portal.azure.com) (dostawcy jest [ClearDB](http://www.cleardb.com/)) i jak się z nim połączyć za pomocą aplikacji sieci web PHP działa [Usługa Azure aplikacji](./app-service/app-service-value-prop-what-is.md). 

> [AZURE.NOTE] Można także tworzyć bazą danych MySQL [szablonu aplikacji Marketplace](./app-service-web/app-service-web-create-web-app-from-marketplace.md).

## <a name="create-a-mysql-database-in-azure-portal"></a>Tworzenie bazy danych MySQL w Azure portal

Aby utworzyć bazę danych MySQL w portalu Azure, wykonaj następujące czynności:

1. Zaloguj się do [portalu Azure](https://portal.azure.com).

2. Z menu po lewej stronie kliknij pozycję **Nowy** > **danych + miejsca do magazynowania** > **Bazy danych MySQL**.

    ![Tworzenie bazy danych MySQL w Azure - rozpoczęcie](./media/store-php-create-mysql-database/create-db-1-start.png)

2. W nowej bazy danych MySQL [Karta](azure-portal-overview.md), skonfigurować nową bazę danych MySQL następująco (*Karta*: strona portalu, otwartym w poziomie):

    - **Nazwa bazy danych**: wpisz nazwę jednoznacznie zidentyfikować
    - **Subskrypcja**: Wybierz subskrypcję, aby użyć
    - **Typ bazy danych**: wybierz pozycję **udostępnione** dla poziomów kosztach lub bezpłatne lub **Dedicated** w celu uzyskania dedykowane zasobów. 
    - **Grupa zasobów**: Dodawanie danych MySQL do istniejącej [grupy zasobów](../azure-resource-manager/resource-group-overview.md) i umieszczanie go w nowe. Zasoby z tej samej grupy można łatwo zarządzać ze sobą.
    - **Lokalizacja**: Wybierz lokalizację Zamknij, możesz. Podczas dodawania do istniejącej grupy zasobów, jest zablokowany do lokalizacji grupa zasobów.
    - **Ceny warstwa**: kliknij **Cennik warstwy**, a następnie wybierz opcję cennik (**rtęci** warstwa jest bezpłatna,), a następnie kliknij przycisk **Wybierz**. 
    - **Warunki prawne**: kliknij pozycję **Warunki prawne**, przejrzyj szczegóły zakupu i kliknij przycisk **Kup**.
    - **Przypnij do pulpitu nawigacyjnego**: Wybierz, jeśli chcesz uzyskać dostęp do bezpośrednio z pulpitu nawigacyjnego. To jest szczególnie przydatne, jeśli użytkownik nie zna jeszcze portalu nawigacji.
    
    Następujące zrzut ekranu jest tylko przykładem sposobu konfigurowania bazy danych MySQL.  
    ![Tworzenie bazy danych MySQL platformy Azure — Konfigurowanie](./media/store-php-create-mysql-database/create-db-2-configure.png)

3. Po zakończeniu konfigurowania, kliknij przycisk **Utwórz**.

    ![Tworzenie bazy danych MySQL w Azure — tworzenie](./media/store-php-create-mysql-database/create-db-3-create.png)

    Zostanie wyświetlona zezwolenia wyskakujące powiadomienie, które znasz, rozpoczętej wdrożenia.

    ![Tworzenie bazy danych MySQL w Azure — w toku](./media/store-php-create-mysql-database/create-db-4-started-status.png)

    Zostanie wyświetlony innego podręcznym po pomyślnym wdrożenia. Portal usługi zostanie również otwarty karta bazy danych programu MySQL.

<a name="connect"></a>
## <a name="connect-to-your-mysql-database"></a>Nawiązywanie połączenia z bazą danych MySQL

Aby wyświetlić informacje o połączeniu dla nowej bazy danych MySQL, po prostu kliknij polecenie **Właściwości** w karta aplikacji sieci web.
    
![Tworzenie bazy danych MySQL w Azure — karta bazy danych MySQL](./media/store-php-create-mysql-database/create-db-5-finished-db-blade.png)

Za pomocą informacje o połączeniu można teraz w dowolnej aplikacji sieci web. Próbkę pokazano, jak użyć informacji o połączeniu za pomocą prostej aplikacji PHP jest dostępna [w tym miejscu](https://github.com/WindowsAzure/azure-sdk-for-php-samples/tree/master/tasklist-mysql).

## <a name="connect-a-laravel-web-app-from-the-php-get-started-tutorial"></a>Łączenie aplikacji sieci web Laravel (z samouczka Wprowadzenie get PHP)

Załóżmy, że po prostu koniec samouczka [Tworzenie, konfigurowanie i wdrażanie aplikacji sieci web PHP Azure](./app-service-web/app-service-web-php-get-started.md) i mieć aplikacji sieci web [Laravel](https://www.laravel.com/) uruchomione w Azure. Funkcje bazy danych można łatwo dodać do aplikacji Laravel. Po prostu wykonaj poniższe czynności:

>[AZURE.NOTE] Poniższe czynności przyjęto założenie, ukończono [Tworzenie, konfigurowanie i wdrażanie aplikacji sieci web PHP Azure](./app-service-web/app-service-web-php-get-started.md)samouczka.

1. Konfigurowanie aplikacji Laravel w środowisku lokalnym rozwoju, aby wskazywały z bazą danych MySQL. Aby to zrobić, otwórz `.env` z Twojej aplikacji Laravel katalogu głównego i skonfiguruj opcje bazy danych MySQL.

        DB_CONNECTION=mysql
        DB_HOST=<HOSTNAME_from_properties_blade>
        DB_PORT=<PORT_from_properties_blade>
        DB_DATABASE=<see_note_below>
        DB_USERNAME=<USERNAME_from_properties_blade>
        DB_PASSWORD=<PASSWORD_from_properties_blade>

    >[AZURE.NOTE] W karta **Właściwości** nazwę bazy danych programu MySQL może być lub może nie być wyświetlane w polu **Nazwa bazy danych** . Warto sprawdzić parametru bazy danych w polu **Parametry połączenia** . 
    >
    >![Tworzenie bazy danych MySQL w Azure — w toku](./media/store-php-create-mysql-database/connect-db-1-database-name.png)

2. Najszybszym sposobem Sprawdź mają dostęp MySQL jest za pomocą [rusztowania uwierzytelniania domyślnych w Laravel](https://laravel.com/docs/5.2/authentication#authentication-quickstart). W terminal wiersza polecenia Uruchom następujące polecenia z katalogu głównego aplikacji Laravel:

         php artisan migrate
         php artisan make:auth

    Pierwsze polecenie tworzy tabele platformy Azure według wstępnie zdefiniowanego migracji w `database/migrations` katalogu i drugie polecenie scaffolds podstawowe widoków i trasy do rejestracji użytkowników i uwierzytelniania.

3. Uruchom teraz na serwerze rozwoju:

        php artisan serve

4. W przeglądarce przejdź do 8000 i rejestrować nowego użytkownika, jak pokazano:

    ![Nawiązywanie połączenia z bazą danych MySQL platformy Azure - rejestruje użytkownika](./media/store-php-create-mysql-database/connect-db-2-development-server.png)

    Wykonaj pełną monit interfejsu użytkownika rejestracji. Po wykonaniu rejestracji będą rejestrowane w.
    
    ![Nawiązywanie połączenia z bazą danych MySQL platformy Azure - rejestruje użytkownika](./media/store-php-create-mysql-database/connect-db-3-registered-user.png)

    Możesz teraz opracowywania aplikacji z bazą danych MySQL platformy Azure.

5. Teraz wystarczy replikować do `.env` ustawienia aplikacji sieci Azure web. Uruchom następujące polecenia Azure polecenie:

        azure site appsetting add DB_CONNECTION=mysql
        azure site appsetting add DB_HOST=<HOSTNAME_from_properties_blade>
        azure site appsetting add DB_PORT=<PORT_from_properties_blade>
        azure site appsetting add DB_DATABASE=<Database_param_from_CONNECTION_INFO_from_properties_blade>
        azure site appsetting add DB_USERNAME=<USERNAME_from_properties_blade>
        azure site appsetting add DB_PASSWORD=<PASSWORD_from_properties_blade>

    Dowiedz się, jak to działa w [artykule Konfigurowanie aplikacji sieci Azure web](./app-service-web/app-service-web-php-get-started.md#configure).

6. Następnie Zatwierdź i push Azure lokalne zmiany wcześniej uruchomionej `php artisan make:auth`.

        git add .
        git commit -m "scaffold auth views and routes"
        git push azure master

7. Przejdź do aplikacji sieci Azure web.

        azure site browse

8. Zaloguj się przy użyciu poświadczeń użytkownika, który został utworzony wcześniej.

    ![Nawiązywanie połączenia z bazą danych MySQL platformy Azure — przejdź do aplikacji sieci web Azure](./media/store-php-create-mysql-database/connect-db-4-browse-azure-webapp.png)

    Po zalogowaniu się powinien być widoczny przyjazny ekranu po logowania.
    
    ![Nawiązywanie połączenia z bazą danych MySQL platformy Azure — zalogowany](./media/store-php-create-mysql-database/connect-db-5-logged-in.png)

    Gratulacje, aplikacji sieci web PHP platformy Azure teraz korzysta z danych z bazy danych MySQL. 

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji zobacz [Centrum deweloperów PHP](/develop/php/).
