<properties
  pageTitle="Efektywne używanie środowiskach DevOps dla aplikacji sieci web"
  description="Dowiedz się, jak za pomocą gniazd wdrożenia Konfigurowanie i zarządzanie nimi w wielu środowiskach opracowywania aplikacji"
  services="app-service\web"
  documentationCenter=""
  authors="sunbuild"
  manager="yochayk"
  editor=""/>

<tags
  ms.service="app-service"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="na"
  ms.workload="web"
  ms.date="10/24/2016"
  ms.author="sumuth"/>

# <a name="use-devops-environments-effectively-for-your-web-apps"></a>Efektywne używanie środowiskach DevOps dla aplikacji sieci web programu

W tym artykule pokazano, konfigurowania i Zarządzanie wdrażaniem aplikacji sieci web dla różnych wersji aplikacji, takich jak rozwój, tymczasowej pytania i odpowiedzi — i produkcji. W każdej wersji aplikacji można uznać za środowisko pracy dla określonych konieczności w ramach procesu wdrażania. Na przykład środowisko aparatu pytania i odpowiedzi mogą być używane przez zespołu deweloperów do testowania jakości aplikacji przed przekazać zmiany do produkcji.
Konfigurowanie wielu środowiskach programistycznych może być trudne zadania niezbędnej do śledzenia i wdrażanie kodu w środowiskach i zarządzanie nimi zasobów (obliczeń, aplikacji sieci web, bazy danych, pamięci podręcznej itp.).

## <a name="setting-up-a-non-production-environment-stagedevqa"></a>Konfigurowanie środowiska wartością produkcji (etap, deweloperów, pytania i odpowiedzi —)
Po utworzeniu aplikacji sieci web dla produkcji pracę, następnym krokiem jest utworzenie środowisku produkcyjnym nie. Aby można było używać gniazda wdrożenia upewnij się, że pracujesz w trybie plan **Standardowy** lub **Premium dla** aplikacji usługi. Wdrożenie gniazda są aplikacje web faktycznie live przy użyciu własnej nazwy hostów. Elementy zawartości i konfiguracji aplikacji sieci Web może się miejscami między dwa gniazda wdrażania, w tym przedział produkcji. Wdrażanie aplikacji przedział wdrożenia ma następujące zalety:

1. Zmiany aplikacji sieci web w tymczasowej przedział wdrażania można sprawdzić przed zamiana go z przedział produkcji.
2. Wdrażanie aplikacji sieci web do przedziału najpierw i wstawiany produkcji zapewnia rozgrzane wszystkie wystąpienia przedział przed są zamienione na produkcji. Dzięki temu przestoje podczas wdrażania aplikacji sieci web. Przekierowanie ruchu jest bezproblemowa i nie są wyświetlane żadne żądania z powodu operacje wymiany. Ten całego przepływu pracy można zautomatyzować przez skonfigurowanie [Automatycznego Zamień](web-sites-staged-publishing.md#configure-auto-swap-for-your-web-app) podczas sprawdzania poprawności przed wymiany nie jest potrzebna.
3. Po wymiany przedział przy użyciu aplikacji web wcześniej etapowej zawiera teraz poprzedniej aplikacji sieci web produkcji. W przypadku zmiany zamienione na przedział produkcji nie zgodnie z oczekiwaniami, można wykonywać samej wymiany aby "ostatniej znanej dobrej aplikacji sieci web" Wstecz.

Aby skonfigurować tymczasową przedział wdrażania, zobacz [Konfigurowanie tymczasowej środowiska dla aplikacji sieci web w usłudze Azure aplikacji](web-sites-staged-publishing.md). Co środowiska obejmuje własny zestaw zasobów, na przykład jeśli web app korzysta z bazy danych, a następnie zarówno produkcji, jak i tymczasowy aplikacji sieci web należy korzystać z różnych baz danych. Dodawanie tymczasowej rozwoju środowiska zasobów, takich jak bazy danych, miejsca do magazynowania lub ustawień środowiska programowania tymczasowej pamięci podręcznej.

## <a name="examples-of-using-multiple-development-environments"></a>Przykłady używania wielu środowiskach projektowania

Każdy projekt należy wykonać zarządzania kodu źródłowego z co najmniej dwóch środowiskach, rozwoju i produkcji środowiska, ale podczas używania systemów zarządzania zawartością, środowisk aplikacji itp możemy napotkać problemy której aplikacja nie obsługuje w tym scenariuszu poza pole. Dotyczy to dla niektórych popularnych RAM omówiony poniżej. Wiele pytań dostępne są podczas pracy z CMS i struktury takich jak

1. Jak podzielić je się na różnych środowiskach
2. Jakie pliki można zmieniać i nie wpływają na aktualizacji wersji framework
3. Jak zarządzać konfigurację dla środowiska
4. Jak zarządzać aktualizacji wersji moduły i wtyczek, aktualizacji wersji framework core

Istnieje wiele sposobów konfigurowania środowisku wielu projektu i poniższych przykładach są jeden jednej takie metody odpowiednich aplikacji.

### <a name="wordpress"></a>WordPress
W tej sekcji dowiesz się, jak skonfigurować wdrożenie przepływu pracy przy użyciu gniazda dla WordPress. WordPress, takie jak większość rozwiązań CMS nie obsługuje pracy z wielu środowiskach programistycznych po ich otrzymaniu. Usługa aplikacji Web Apps zawiera kilka funkcji ułatwiających przechowywać ustawienia konfiguracji poza kodu.

Przed utworzeniem tymczasowej przedział, skonfiguruj kodzie aplikacji do obsługi wielu środowiskach. Do obsługi wielu środowiskach w WordPress ma być edytowany `wp-config.php` w aplikacji sieci web rozwoju lokalnego Dodaj następujący kod na początku pliku. To umożliwia aplikacji wybierz poprawnej konfiguracji na podstawie zaznaczonego środowiska.

```
// Support multiple environments
// set the config file based on current environment
if (strpos($_SERVER['HTTP_HOST'],'localhost') !== false) {
// local development
 $config_file = 'config/wp-config.local.php';
}
elseif ((strpos(getenv('WP_ENV'),'stage') !== false) || (strpos(getenv('WP_ENV'),'prod' )!== false ))
//single file for all azure development environments
 $config_file = 'config/wp-config.azure.php';
}
$path = dirname(__FILE__). '/';
if (file_exists($path. $config_file)) {
// include the config file if it exists, otherwise WP is going to fail
require_once $path. $config_file;
```

Utwórz folder w katalogu głównym aplikacji sieci web o nazwie `config` i dodać plik dwa pliki: `wp-config.azure.php` i `wp-config.local.php` reprezentujących odpowiednio środowiska usługi azure i lokalnych.

Skopiuj następujące w `wp-config.local.php` :

```
<?php
// MySQL settings
/** The name of the database for WordPress */

define('DB_NAME', 'yourdatabasename');

/** MySQL database username */
define('DB_USER', 'yourdbuser');

/** MySQL database password */
define('DB_PASSWORD', 'yourpassword');

/** MySQL hostname */
define('DB_HOST', 'localhost');
/**
 * For developers: WordPress debugging mode.
 * * Change this to true to enable the display of notices during development.
 * It is strongly recommended that plugin and theme developers use WP_DEBUG
 * in their development environments.
 */
define('WP_DEBUG', true);

//Security key settings
define('AUTH_KEY', 'put your unique phrase here');
define('SECURE_AUTH_KEY','put your unique phrase here');
define('LOGGED_IN_KEY','put your unique phrase here');
define('NONCE_KEY', 'put your unique phrase here');
define('AUTH_SALT', 'put your unique phrase here');
define('SECURE_AUTH_SALT', 'put your unique phrase here');
define('LOGGED_IN_SALT', 'put your unique phrase here');
define('NONCE_SALT', 'put your unique phrase here');

/**
 * WordPress Database Table prefix.
 *
 * You can have multiple installations in one database if you give each a unique
 * prefix. Only numbers, letters, and underscores please!
 */
$table_prefix = 'wp_';
```

Ustawianie kluczy zabezpieczeń powyżej można pomoc jest naruszone przez hakera możesz uniemożliwić aplikacji sieci web, dlatego skorzystaj unikatowe wartości. Jeśli potrzebujesz Generuj ciąg dla kluczy zabezpieczeń wymienionych powyżej, możesz przejść do automatycznego generator, aby utworzyć nowe klucze i wartości przy użyciu tego [łącza] (https://api.wordpress.org/secret-key/1.1/salt)

Skopiuj poniższy kod w `wp-config.azure.php`:


``` <?php
    // MySQL settings
    /** The name of the database for WordPress */
    
    define('DB_NAME', getenv('DB_NAME'));
    
    /** MySQL database username */
    define('DB_USER', getenv('DB_USER'));
    
    /** MySQL database password */
    define('DB_PASSWORD', getenv('DB_PASSWORD'));
    
    /** MySQL hostname */
    define('DB_HOST', getenv('DB_HOST'));
    
    /**
    * For developers: WordPress debugging mode.
    *
    * Change this to true to enable the display of notices during development.
    * It is strongly recommended that plugin and theme developers use WP_DEBUG
    * in their development environments.
    * Turn on debug logging to investigate issues without displaying to end user. For WP_DEBUG_LOG to
    * do anything, WP_DEBUG must be enabled (true). WP_DEBUG_DISPLAY should be used in conjunction
    * with WP_DEBUG_LOG so that errors are not displayed on the page */
    
    */
    define('WP_DEBUG', getenv('WP_DEBUG'));
    define('WP_DEBUG_LOG', getenv('TURN_ON_DEBUG_LOG'));
    define('WP_DEBUG_DISPLAY',false);
    
    //Security key settings
    /** If you need to generate the string for security keys mentioned above, you can go the automatic generator to create new keys/values: https://api.wordpress.org/secret-key/1.1/salt **/
    define('AUTH_KEY',getenv('DB_AUTH_KEY'));
    define('SECURE_AUTH_KEY', getenv('DB_SECURE_AUTH_KEY'));
    define('LOGGED_IN_KEY', getenv('DB_LOGGED_IN_KEY'));
    define('NONCE_KEY', getenv('DB_NONCE_KEY'));
    define('AUTH_SALT', getenv('DB_AUTH_SALT'));
    define('SECURE_AUTH_SALT', getenv('DB_SECURE_AUTH_SALT'));
    define('LOGGED_IN_SALT',  getenv('DB_LOGGED_IN_SALT'));
    define('NONCE_SALT',  getenv('DB_NONCE_SALT'));
    
    /**
    * WordPress Database Table prefix.
    *
    * You can have multiple installations in one database if you give each a unique
    * prefix. Only numbers, letters, and underscores please!
    */
    $table_prefix = getenv('DB_PREFIX');
```

#### <a name="use-relative-paths"></a>Używanie ścieżek względnych
Jedyne ostatniej jest skonfigurowanie aplikacji WordPress do używanie ścieżek względnych. WordPress są przechowywane adresy URL w bazie danych. Dzięki temu przenoszenia zawartości z jednego środowiska do innego trudniejsze niezbędnej do aktualizacji bazy danych w każdym przenoszenia z lokalnym na etapie lub etapie do środowisku produkcyjnym. Aby zmniejszyć ryzyko problemów powodujących z wdrożeniem bazy danych za każdym razem, gdy wdrażanie z jednego środowiska do innego za pomocą [wtyczki łącza względne głównego](https://wordpress.org/plugins/root-relative-urls/) , które można zainstalować za pomocą pulpitu nawigacyjnego administratora WordPress lub ręcznie pobierać [tutaj](https://downloads.wordpress.org/plugin/root-relative-urls.zip).


Dodaj następujące wpisy do swojego `wp-config.php` pliku przed `That's all, stop editing!` komentarz:

```

  define('WP_HOME', 'http://'. filter_input(INPUT_SERVER, 'HTTP_HOST', FILTER_SANITIZE_STRING));
    define('WP_SITEURL', 'http://'. filter_input(INPUT_SERVER, 'HTTP_HOST', FILTER_SANITIZE_STRING));
    define('WP_CONTENT_URL', '/wp-content');
    define('DOMAIN_CURRENT_SITE', filter_input(INPUT_SERVER, 'HTTP_HOST', FILTER_SANITIZE_STRING));
```

Aktywowanie dodatek za pośrednictwem `Plugins` menu na pulpicie nawigacyjnym administratora WordPress. Zapisywanie ustawień łącze stałe WordPress aplikacji.

#### <a name="the-final-wp-configphp-file"></a>Ostatni `wp-config.php` pliku
Nie wpływa na wszystkie aktualizacje WordPress Core usługi `wp-config.php`, `wp-config.azure.php` i `wp-config.local.php` plików. Na końcu tego kroku `wp-config.php` plik będzie wyglądać następująco

```
<?php
/**
 * The base configurations of the WordPress.
 *
 * This file has the following configurations: MySQL settings, Table Prefix,
 * Secret Keys, and ABSPATH. You can find more information by visiting
 *
 * Codex page. You can get the MySQL settings from your web host.
 *
 * This file is used by the wp-config.php creation script during the
 * installation. You don't have to use the web web app, you can just copy this file
 * to "wp-config.php" and fill in the values.
 *
 * @package WordPress
 */

// Support multiple environments
// set the config file based on current environment
if (strpos($_SERVER['HTTP_HOST'],'localhost') !== false) { // local development
  $config_file = 'config/wp-config.local.php';
}
elseif ((strpos(getenv('WP_ENV'),'stage') !== false) ||(strpos(getenv('WP_ENV'),'prod' )!== false )){
  $config_file = 'config/wp-config.azure.php';
}


$path = dirname(__FILE__). '/';
if (file_exists($path. $config_file)) {
  // include the config file if it exists, otherwise WP is going to fail
  require_once $path. $config_file;
}

/** Database Charset to use in creating database tables. */
define('DB_CHARSET', 'utf8');

/** The Database Collate type. Don't change this if in doubt. */
define('DB_COLLATE', '');


/* That's all, stop editing! Happy blogging. */

define('WP_HOME', 'http://'. $_SERVER['HTTP_HOST']);
define('WP_SITEURL', 'http://'. $_SERVER['HTTP_HOST']);
define('WP_CONTENT_URL', '/wp-content');
define('DOMAIN_CURRENT_SITE', $_SERVER['HTTP_HOST']);

/** Absolute path to the WordPress directory. */
if ( !defined('ABSPATH') )
    define('ABSPATH', dirname(__FILE__). '/');

/** Sets up WordPress vars and included files. */
require_once(ABSPATH. 'wp-settings.php');
```

#### <a name="set-up-a-staging-environment"></a>Konfigurowanie środowiska przemieszczania
Wykonując jeszcze aplikacji sieci web WordPress uruchomionych dla sieci Web usługi Azure, zaloguj się do [portalu Azure zarządzania Podgląd](http://portal.azure.com) i przejdź do aplikacji sieci web WordPress. Jeśli aplikacji nie można utworzyć jeden z witryny marketplace. Aby dowiedzieć się więcej, [kliknij tutaj](web-sites-php-web-site-gallery.md).
Kliknij pozycję Ustawienia -> wdrożenia gniazda -> Dodaj, aby utworzyć przedział wdrażania z etapu nazwę. Przedział wdrożenia jest udostępniania tych samych zasobów co aplikacji sieci web podstawowego, utworzony w poprzednim przykładzie innej aplikacji sieci web.

![Tworzenie przedział wdrożenia etapu](./media/app-service-web-staged-publishing-realworld-scenarios/1setupstage.png)

Dodawanie innej bazy danych MySQL powiedz `wordpress-stage-db` do grupy zasobów `wordpressapp-group`.

 ![Dodawanie bazy danych MySQL do grupy zasobów](./media/app-service-web-staged-publishing-realworld-scenarios/2addmysql.png)

Aktualizowanie parametry połączenia dla swojego przedział wdrożenia etap wskazywały nowo utworzonego bazy danych, `wordpress-stage-db`. Uwaga, że produkcji web app, `wordpressprodapp` i tymczasowy web app `wordpressprodapp-stage` muszą wskazywać różnych baz danych.

#### <a name="configure-environment-specific-app-settings"></a>Konfigurowanie ustawień aplikacji specyficzne dla środowiska
Programiści mogą zawierać pary ciągów wartość klucza platformy Azure jako część informacji konfiguracji skojarzone z aplikacji sieci web o nazwie ustawienia aplikacji. W czasie wykonywania aplikacji sieci Web usługi automatycznie pobiera te wartości dla Ciebie i udostępnia je kodu działającego w aplikacji sieci web. Z papieru wartościowego perspektywie, która jest i po stronie korzystać od informacje poufne, takie jak parametry połączenia bazy danych przy użyciu hasła nigdy się pojawić jako zwykłego tekstu w pliku takich jak `wp-config.php`.

Ten proces określonych poniżej jest przydatny, gdy wykonywanych zawiera zarówno zmiany plików i zmian w bazie danych dla aplikacji WordPress:
- Uaktualnianie wersji WordPress
- Dodaj nowe lub edytować czy uaktualnić wtyczki
- Dodaj nowe lub edytować czy uaktualnić motywów

Konfigurowanie ustawień aplikacji:

- informacje o bazie danych
- Włączanie i wyłączanie rejestrowania WordPress
- Ustawienia zabezpieczeń WordPress

![Ustawienia aplikacji dla aplikacji sieci web Wordpress](./media/app-service-web-staged-publishing-realworld-scenarios/3configure.png)

Upewnij się, że dodano następujące ustawienia aplikacji dla przedział swojej produkcji w sieci web app i etap. Należy zauważyć, że produkcji aplikacji sieci web i aplikacji sieci web tymczasowego używają różnych baz danych.
Usuń zaznaczenie pola wyboru **Ustawienie przedział** dla wszystkich parametrów ustawienia z wyjątkiem WP_ENV. Spowoduje to Zamień konfiguracji dla aplikacji sieci web oraz zawartość pliku oraz bazy danych. Jeśli **Ustawienie przedział** jest **zaznaczone**, ustawienia aplikacji aplikacji sieci web i konfiguracja parametry połączenia nie zmienia położenia w środowiskach podczas wykonywania operacji wymiany, a więc jeśli istnieją zmiany bazy danych spowoduje to nie przerwanie produkcji aplikacji sieci web.

Wdrażanie aplikacji sieci web środowiska rozwoju lokalnego do etapu web app i bazy danych przy użyciu WebMatrix lub wykonaj wyboru takich jak FTP, cyfra lub PhpMyAdmin.

![Okno dialogowe publikowanie macierzy sieci Web dla aplikacji sieci web WordPress](./media/app-service-web-staged-publishing-realworld-scenarios/4wmpublish.png)

Przeglądanie i testowanie tymczasowy aplikacji sieci web. Uwzględniając scenariusz motyw aplikacji sieci web w przypadku aktualizacji Oto tymczasowy aplikacji sieci web.

![Przeglądanie tymczasowy aplikacji sieci web przed zamiana gniazda](./media/app-service-web-staged-publishing-realworld-scenarios/5wpstage.png)


 Jeśli wszystkie odpowiada Twoim potrzebom, kliknij przycisk **Zamień** w aplikacji sieci web tymczasowy, aby przenieść zawartość w środowisku produkcyjnym. W takim przypadku możesz zamienić aplikacji sieci web i bazy danych w środowiskach podczas każdej operacji **wymiany** .

![Zamień Podgląd zmiany na WordPress](./media/app-service-web-staged-publishing-realworld-scenarios/6swaps1.png)

 > [AZURE.NOTE]
 >Jeśli masz scenariusza, gdy trzeba się tylko wypychanych pliki (nie aktualizacji bazy danych), następnie **Sprawdź** **Ustawienie przedział** dla wszystkich baz danych zawartości związanej z *ustawień aplikacji* i *Ustawienia ciągów połączenia* w karta Ustawienia aplikacji sieci web w portal Azure podgląd przed wykonaniem transakcji. W tym przypadku DB_NAME, DB_HOST, DB_PASSWORD, DB_USER domyślnym ustawieniem parametry połączenia należy nie widoczne w podgląd zmian w trakcie **Zamień**. W tym razem po wykonaniu operacji **Zamień** aplikacji sieci web WordPress uzyskuje aktualizacje plików **tylko**.

Przed wykonaniem wymiany, Oto aplikacji sieci web WordPress produkcji ![produkcji aplikacji sieci web przed zamiana gniazda](./media/app-service-web-staged-publishing-realworld-scenarios/7bfswap.png)

Po zakończeniu operacji wymiany motyw został zaktualizowany w aplikacji sieci web produkcji.

![W przeglądarce produkcji po zamianie gniazda](./media/app-service-web-staged-publishing-realworld-scenarios/8afswap.png)

W sytuacji, gdy chcesz **przywrócić**, możesz można przejdź do ustawień aplikacji sieci web produkcji i kliknij przycisk **Zamień** , aby zamienić aplikacji sieci web i bazy danych z produkcji tymczasowy przedział. Ważne jest, aby pamiętać to, że jeśli zmian w bazie danych są dołączone do operacji **Zamień** w danym momencie, następnie przy następnym ponownie wdrożyć do przemieszczania aplikacji sieci web należy wdrożyć bazy danych zmienia się na bieżącej bazy danych dla tymczasowy aplikacji sieci web może to być poprzedniej produkcyjnej bazy danych lub etapu bazy danych.

#### <a name="summary"></a>Podsumowanie
Uogólniać procesu dla każdej aplikacji z bazą danych

1. Instalowanie aplikacji w środowisku lokalnym
2. Zawiera określone konfiguracji środowiska (lokalnego i Azure Web App)
3. Konfigurowanie usługi środowisk w aplikacji sieci Web usługi — przemieszczania produkcji
4. Jeśli masz aplikację produkcji już uruchomiony Azure synchronizowanie zawartości produkcji (pliki i kod + bazy danych) w środowisku lokalnym i tymczasowy.
5. Opracowanie aplikacji w środowisku lokalnym
6. Umieść zawartość z produkcji ze środowiskami tymczasowego i deweloperów aplikacji sieci web produkcji w obszarze konserwacji lub tryb zablokowane i synchronizacji bazy danych
7. Wdrożyć do przemieszczania środowiska i Test
8. Wdrażanie w środowisku produkcyjnym
9. Powtórz kroki od 4 do 6

### <a name="umbraco"></a>Umbraco
W tej sekcji dowiesz się, jak używane są Umbraco CMS modułu niestandardowych do wdrożenia na całym środowisku wielu DevOps. W tym przykładzie umożliwia innym sposobem zarządzania wielu środowiskach projektowania.

[Umbraco CMS](http://umbraco.com/) jest jednym z popular.NET CMS rozwiązania używane przez wielu programistów co pozwala na moduł [Courier2](http://umbraco.com/products/more-add-ons/courier-2) wdrożenia rozwoju tymczasowego do środowisku produkcyjnym. Można łatwo tworzyć środowiska lokalnego projektowania dla aplikacji sieci web programu Umbraco CMS przy użyciu programu Visual Studio lub WebMatrix.

1. Tworzenie aplikacji sieci web programu Umbraco przy użyciu programu Visual Studio, [kliknij tutaj](https://our.umbraco.org/documentation/Installation/install-umbraco-with-nuget).
2. Tworzenie aplikacji sieci web programu Umbraco z WebMatrix, [kliknij tutaj](http://umbraco.com/help-and-support/video-tutorials/getting-started/working-with-webmatrix).

Pamiętaj, aby usunąć zawsze `install` folder w obszarze aplikacji i nigdy nie przekazać go do etapu lub produkcji aplikacji sieci web. Ten samouczek można korzystać z WebMatrix

#### <a name="set-up-a-staging-environment"></a>Konfigurowanie środowiska tymczasowy
- Tworzenie przedział wdrażania, jak powyżej Umbraco CMS aplikacji web App, przy założeniu, że masz już aplikacji sieci web programu Umbraco CMS w górę i uruchamiania. Jeśli nie możesz utworzyć je z witryny marketplace.

- Zaktualizuj parametry połączenia dla swojego przedział wdrożenia etap wskazywały nowo utworzonego bazy danych, **umbraco etap db**. Usługi aplikacji sieci web produkcji (umbraositecms-1) i tymczasowy aplikacji sieci web (umbracositecms-1-etap) **muszą** wskazywać różnych baz danych.

![Aktualizowanie parametrów połączenia przemieszczania aplikacji sieci web z nową tymczasową bazę danych](./media/app-service-web-staged-publishing-realworld-scenarios/9umbconnstr.png)

- Kliknij przycisk **Ustawienia uzyskiwanie publikowanie** przedział wdrożenia **etapu**. To pobierze plik ustawień publikowania, który przechowywać wszystkie informacje wymagane przez Visual Studio lub macierzy w sieci Web do publikowania aplikacji z aplikacji sieci web rozwoju lokalnego Azure w przeglądarce.

 ![Uzyskiwanie publikowanie ustawienie tymczasowy aplikacji sieci web](./media/app-service-web-staged-publishing-realworld-scenarios/10getpsetting.png)

- Otwórz aplikację sieci web rozwoju lokalnego w **WebMatrix** lub **Visual Studio**. W tym samouczku korzystam z sieci Web macierzy i musisz najpierw zaimportować plik ustawienia publikowania dla aplikacji sieci web tymczasowy

![Importuj ustawienia publikowania Umbraco za pomocą macierzy w sieci Web](./media/app-service-web-staged-publishing-realworld-scenarios/11import.png)

- Przeglądanie zmian w oknie dialogowym i wdrażanie aplikacji sieci web lokalne do aplikacji sieci Azure web, *umbracositecms-1-sceny*. Przy umieszczaniu plików bezpośrednio do aplikacji sieci web tymczasowy zostanie pominięty, wszystkie pliki w `~/app_data/TEMP/` pracę jako zostaną one wygenerowane przy pierwszym etapie aplikacji sieci web, folderu. Należy również pominąć `~/app_data/umbraco.config` pliku jako tego, zostaną wygenerowane.

![Przeglądanie zmian Publikuj w macierzy w sieci web](./media/app-service-web-staged-publishing-realworld-scenarios/12umbpublish.png)

- Po pomyślnym opublikowaniu aplikacji sieci web lokalne Umbraco z tymczasową aplikacji sieci web, przejdź do tymczasowej aplikacji sieci web i wykonaj kilka testy, aby wykluczyć wszelkie problemy.

#### <a name="set-up-courier2-deployment-module"></a>Konfigurowanie Courier2 wdrażania modułu
Z modułem [Courier2](http://umbraco.com/products/more-add-ons/courier-2) można naciśnij zawartości, arkusze stylów, moduły rozwoju i wielu prostą kliknij prawym przyciskiem myszy z tymczasowy aplikacji sieci web do aplikacji sieci web produkcji więcej wdrożeniach bezpłatne problemów i ograniczanie zagrożeń złamanie aplikacji sieci web produkcji wdrażając aktualizacji.
Zakup licencji dla Courier2 dla domeny `*.azurewebsites.net` i domeny niestandardowej (na przykład http://abc.com) po zakupieniu licencji umieścić pobrane licencji (. Plik LIC) w `bin` folder.

![Upuść plik licencji w folderze Kosz](./media/app-service-web-staged-publishing-realworld-scenarios/13droplic.png)

Pobierz pakiet Courier2 [tutaj](https://our.umbraco.org/projects/umbraco-pro/umbraco-courier-2/). Zaloguj się do aplikacji sieci web scenę, powiedz http://umbracocms-site-stage.azurewebsites.net/umbraco i kliknij Menu **Deweloper** i wybierz pozycję **pakietów**. Wybierz polecenie **Zainstaluj** pakiet lokalny

![Instalator pakietu Umbraco](./media/app-service-web-staged-publishing-realworld-scenarios/14umbpkg.png)

Przekazywanie pakietu courier2 za pomocą Instalatora.

![Przekazywanie pakietu courier modułu](./media/app-service-web-staged-publishing-realworld-scenarios/15umbloadpkg.png)

Możesz skonfigurować musisz zaktualizować plik courier.config w folderze **konfiguracji** aplikacji sieci web.

```xml
<!-- Repository connection settings -->
 <!-- For each site, a custom repository must be configured, so Courier knows how to connect and authenticate-->
 <repositories>
    <!-- If a custom Umbraco Membership provider is used, specify login & password + set the passwordEncoding to clear: -->
    <repository name="production web app" alias="stage" type="CourierWebserviceRepositoryProvider" visible="true">
      <url>http://umbracositecms-1.azurewebsites.net</url>
      <user>0</user>
      <!--<login>user@email.com</login> -->
      <!-- <password>user_password</password>-->
      <!-- <passwordEncoding>Clear</passwordEncoding>-->
      </repository>
 </repositories>
 ```

W obszarze `<repositories>`, wprowadź informacje adres URL i użytkownika witryny produkcji. Jeśli korzystasz z domyślnego dostawcę członkostwa Umbraco, należy dodać identyfikator użytkownika Administracja w <user> sekcji. Jeśli korzystasz z niestandardowej Dostawca członkostwa Umbraco, za pomocą `<login>`,`<password>` do modułu Courier2 wiedzieć, jak połączyć się z witryną produkcji. Aby uzyskać więcej informacji przejrzyj [dokumentację](http://umbraco.com/help-and-support/customer-area/courier-2-support-and-download/developer-documentation) Courier modułu.

Podobnie zainstalować moduł Courier w witrynie produkcji i skonfiguruj go wskaż etapu aplikacji sieci web w pliku odpowiednich courier.config jak pokazano tutaj

```xml
 <!-- Repository connection settings -->
 <!-- For each site, a custom repository must be configured, so Courier knows how to connect and authenticate-->
 <repositories>
    <!-- If a custom Umbraco Membership provider is used, specify login & password + set the passwordEncoding to clear: -->
    <repository name="Stage web app" alias="stage" type="CourierWebserviceRepositoryProvider" visible="true">
      <url>http://umbracositecms-1-stage.azurewebsites.net</url>
      <user>0</user>
      </repository>
 </repositories>
```

Kliknij kartę Courier2 na pulpicie nawigacyjnym aplikacji sieci web Umbraco CMS i wybierz polecenie lokalizacje. Powinien zostać wyświetlony Nazwa repozytorium wymienionych w `courier.config`. W tym na produkcji i tymczasowej aplikacji sieci web.

![Repozytorium aplikacji sieci web docelowego widoku](./media/app-service-web-staged-publishing-realworld-scenarios/16courierloc.png)

Umożliwia teraz wdrażanie części zawartości z witryny tymczasową do witryny produkcji. Przejdź do zawartości i wybrać stronę istniejącą lub Utwórz nową stronę. Można będzie wybrać istniejącą stronę z mojej aplikacji sieci web, w której tytułu strony został zamieniony na **Wprowadzenie — nowe** i teraz kliknij pozycję **Zapisz i opublikuj**.

![Zmienianie tytułu strony i publikowanie](./media/app-service-web-staged-publishing-realworld-scenarios/17changepg.png)

TERa zaznacz zmodyfikowanej strony, a następnie *kliknij prawym przyciskiem myszy* , aby wyświetlić wszystkie opcje. Polecenie **Courier** , aby wyświetlić okno dialogowe wdrożenia. Kliknij pozycję na **Rozmieszczanie** , aby zainicjować wdrażania

![Okno dialogowe wdrażania moduł Courier](./media/app-service-web-staged-publishing-realworld-scenarios/18dialog1.png)

Przejrzyj zmiany i kliknij przycisk Kontynuuj.

![Courier modułu wdrożenia okno dialogowe Przejrzyj zmiany](./media/app-service-web-staged-publishing-realworld-scenarios/19dialog2.png)

Dziennik wdrożenia pokazuje, jeśli wdrażanie zakończyło się pomyślnie.

 ![Wyświetlanie dzienników wdrażania z modułu Courier](./media/app-service-web-staged-publishing-realworld-scenarios/20successdlg.png)

Przeglądaj w aplikacji sieci web produkcji, aby zobaczyć zmiany, zostaną one odzwierciedlone.

 ![Przeglądanie produkcji aplikacji sieci web](./media/app-service-web-staged-publishing-realworld-scenarios/21umbpg.png)

Aby dowiedzieć się więcej o używaniu Courier, należy zapoznać się z dokumentacją.

#### <a name="how-to-upgrade-umbraco-cms-version"></a>Jak przeprowadzić uaktualnienie wersji Umbraco CMS

Courier nie pomoże, rozmieszczanie za pomocą uaktualnienia z jednej wersji Umbraco CMS do innego. W przypadku uaktualniania wersji Umbraco CMS, należy sprawdzić dla niezgodności z niestandardowe moduły lub innych firm, moduły i bibliotek Umbraco Core. Zgodnie z zaleceniami dotyczącymi

1. ZAWSZE utworzyć kopię zapasową Twojej aplikacji sieci web i bazy danych przed wykonaniem uaktualnienia. W aplikacji sieci Web Azure, możesz skonfigurować automatyczne wykonywanie kopii zapasowych dla swojej witryny sieci Web przy użyciu kopii zapasowej funkcji i przywracanie witryny Jeśli wymagane przy użyciu przywrócić funkcję. Aby uzyskać więcej informacji zobacz [jak utworzyć kopię zapasową aplikacji sieci web](web-sites-backup.md) i [jak przywrócić aplikacji sieci web](web-sites-restore.md).

2. Sprawdź, czy pakiety innych firm, którego używasz są zgodne z wersji, którą w przypadku uaktualniania. Strony pobieranych pakietu, przejrzyj zgodności projektu z wersją Umbraco CMS.

Aby uzyskać więcej informacji na temat uaktualniania aplikacji sieci web w lokalnie postępuj zgodnie z zasadami jako wymienione [poniżej](https://our.umbraco.org/documentation/getting-started/set up/upgrading/general).

Gdy witryny rozwoju lokalnego zostanie uaktualniony, Opublikuj zmiany do przemieszczenia aplikacji sieci web. Testowanie aplikacji i jeśli wszystkie odpowiada Twoim potrzebom, użyj przycisk **Zamień** , aby **Zamień** tymczasowy witryny do aplikacji sieci web produkcji. Podczas wykonywania operacji **Zamień** , można wyświetlać zmiany, które będą mieć negatywny wpływ na konfiguracji aplikacji sieci web. Za pomocą tej operacji **Zamień** możemy są zamiana aplikacji sieci web i baz danych. Oznacza to, po wymiany aplikacji sieci web produkcji będzie wskazywać umbraco etap db bazy danych i tymczasowy aplikacji sieci web będzie wskazywać umbraco zlecenie db bazy danych.

![Zamiana podglądu do wdrażania Umbraco CMS](./media/app-service-web-staged-publishing-realworld-scenarios/22umbswap.png)

Zaletą zamiana aplikacji sieci web i bazy danych:
1. Umożliwia możesz przywrócić poprzednią wersję aplikacji sieci web przy użyciu innego **Zamień** w przypadku jakichkolwiek problemów z aplikacji.
2. W przypadku uaktualnienia należy wdrożyć pliki i bazy danych z tymczasowej aplikacji sieci web do produkcji web app i bazy danych. Istnieje wiele elementów, które może pójść źle wdrażając plików i bazy danych. Za pomocą funkcji **wymiany** czasu na start lub możemy zmniejszyć przestoje podczas uaktualniania i zmniejszyć ryzyko błędy, które mogą wystąpić podczas wdrażania zmian.
3. Umożliwia to zrobić **A / testowanie B** przy użyciu funkcji [Testowanie produkcji](https://azure.microsoft.com/documentation/videos/introduction-to-azure-websites-testing-in-production-with-galin-iliev/)

W tym przykładzie pokazano elastyczność platformy miejsce, w którym można tworzyć niestandardowe moduły podobny do Umbraco Courier moduł Zarządzanie wdrażaniem w środowiskach.

## <a name="references"></a>Odwołania
[Adaptacyjne programowania Azure aplikacji usługi](app-service-agile-software-development.md)

[Konfigurowanie tymczasowej środowiska dla aplikacji sieci web w usłudze Azure aplikacji](web-sites-staged-publishing.md)

[Jak blokować dostęp w sieci web do wdrożenia wartością produkcji gniazd](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/)
