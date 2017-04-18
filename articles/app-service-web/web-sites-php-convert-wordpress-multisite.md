<properties 
    pageTitle="Konwertowanie WordPress wielooddziałowość w usłudze Azure aplikacji" 
    description="Dowiedz się, jak wykonać istniejącej aplikacji sieci web WordPress utworzone za pomocą galerii platformy Azure i przekonwertuj ją wielooddziałowość WordPress" 
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



# <a name="convert-wordpress-to-multisite-in-azure-app-service"></a>Konwertowanie WordPress wielooddziałowość w usłudze Azure aplikacji

## <a name="overview"></a>Omówienie

*Przez [Lobaugh Jan][ben-lobaugh], [Inc. technologii Otwórz firmy Microsoft][ms-open-tech]*

W tym samouczku dowiesz się, jak wykonać istniejącej aplikacji sieci web WordPress utworzone za pomocą galerii platformy Azure i konwertować instalacji wielooddziałowość WordPress. Ponadto można będzie Dowiedz się, jak przypisać domeny niestandardowej do wszystkich podwitryn instalacją.

Przyjmuje się, że masz istniejącej instalacji programu WordPress. Jeśli nie, postępuj zgodnie z udostępnionymi wskazówkami w sekcji [Tworzenie witryny sieci web WordPress z galerii w Azure][website-from-gallery].

Konwertowanie istniejącego WordPress Zainstaluj jednej witryny, aby wielooddziałowość zazwyczaj jest całkiem prosty i wiele z tych początkowych kroków pochodzą bezpośrednio [Utworzyć sieć] [ wordpress-codex-create-a-network] strony w [WordPress Codex](http://codex.wordpress.org).

Rozpocznijmy pracę.

## <a name="allow-multisite"></a>Zezwalaj na wielooddziałowości

Najpierw należy włączyć wielooddziałowość za pośrednictwem `wp-config.php` pliku z **WP\_Zezwalaj\_WIELOODDZIAŁOWOŚĆ** stałej. Istnieją dwie metody edytowania plików aplikacji sieci web: pierwszego odbywa się za pośrednictwem FTP, a drugi do cyfra. Jeśli użytkownik nie zna konfigurowanie jednej z następujących metod, przeczytaj następujące samouczki:

* [Witryna sieci web PHP z MySQL i FTP][website-w-mysql-and-ftp-ftp-setup]

* [Witryny sieci web PHP z MySQL i cyfra][website-w-mysql-and-git-git-setup]

Otwórz `wp-config.php` plik za pomocą edytora wybrane i Dodaj następujący powyżej `/* That's all, stop editing! Happy blogging. */` linii.

    /* Multisite */

    define( 'WP_ALLOW_MULTISITE', true );

Pamiętaj zapisać plik i przekaż go do serwera!

## <a name="network-setup"></a>Konfiguracja sieci

Zaloguj się w do obszaru *Administrator wp* sieci web i aplikacji powinna być widoczna nowego elementu w menu **Narzędzia** , o nazwie **Ustawienia sieci**. Kliknij przycisk **Ustawienia sieci** i wprowadź szczegóły sieci.

![Ekran konfiguracji sieci][wordpress-network-setup]

Ten samouczek korzysta schematu witryny *podkatalogów* , ponieważ zawsze powinna działać i firma Microsoft będzie można Konfigurowanie domen niestandardowych dla każdej witryny podrzędnej w dalszej części samouczka. Jednak powinno być możliwe ustawienia poddomeny Zainstaluj Jeśli prawidłowo mapowane z domeną przez symbol wieloznaczny [Azure Portal](https://portal.azure.com) i ustawienia DNS.

Więcej informacji na temat poddomenę w porównaniu z podkatalogu ustawienia zobacz [typy wielooddziałowości sieci] [ wordpress-codex-types-of-networks] artykułu w witrynie WordPress Codex.

## <a name="enable-the-network"></a>Włączanie sieci

Sieć jest skonfigurowany w bazie danych, ale występuje jeden pozostałych czynności, aby włączyć funkcję sieci. Finalizowanie `wp-config.php` ustawienia i upewnij się, `web.config` poprawnie kieruje każdej witryny.


Po kliknięciu przycisku **Zainstaluj** na stronie *Konfiguracja sieci* , spróbuje zaktualizować WordPress `wp-config.php` i `web.config` plików. Zawsze należy jednak sprawdzić pliki, aby upewnić się, że aktualizacje zostały pomyślnie. W przeciwnym razie tego ekranu będą wyświetlane niezbędne aktualizacje. Edytowanie i zapisywanie plików.


Po dokonania te aktualizacje, które musisz wylogować się i zalogować się z powrotem do pulpitu nawigacyjnego administratora wp.

Teraz należy dodatkowe menu Administrator na pasku oznaczona etykietą **Witryn Moja witryna**. W tym menu pozwala na sterowanie sieci za pomocą pulpitu nawigacyjnego **Administratora sieci** .

## <a name="adding-custom-domains"></a>Dodawanie domen niestandardowych

[Mapowanie domeny MU WordPress] [ wordpress-plugin-wordpress-mu-domain-mapping] wtyczkę ułatwia błyskawicznie Dodawanie domen niestandardowych do dowolnej witryny w sieci. Aby dodatek działać poprawnie musisz wykonać pewne dodatkowe ustawienia w portalu, a także u rejestratora domeny.

## <a name="enable-domain-mapping-to-the-web-app"></a>Włącz mapowanie domeny do aplikacji sieci web

Tryb plan [Usługi aplikacji](http://go.microsoft.com/fwlink/?LinkId=529714) **wolnego** nie obsługuje dodawanie domen niestandardowych do aplikacji sieci Web. Należy przejść do trybu **udostępnione** lub **Standardowy** . Aby to zrobić:

* Zaloguj się do portalu Azure i Znajdź aplikacji sieci web. 
* Kliknij kartę **rozbudowy** w obszarze **Ustawienia**.
* W obszarze **Ogólne**wybierz pozycję *UDOSTĘPNIONE* lub *Standardowy*
* Kliknij przycisk **Zapisz**

Może zostać wyświetlony komunikat z pytaniem zweryfikować zmiany i Potwierdź, że aplikacji sieci web może teraz ponoszenia kosztów, w zależności od użycia i konfigurację ustawione.

Wystarczy kilka sekund przetwarzania nowe ustawienia, więc jest odpowiednim czasie, aby rozpocząć konfigurowanie domeny.

## <a name="verify-your-domain"></a>Weryfikowanie domeny

Przed Azure Web Apps umożliwia mapowanie domeny do witryny, należy najpierw sprawdź, czy masz autoryzacji do zamapować domeny. Aby to zrobić, możesz dodać nowy rekord CNAME do wpisu DNS.

* Zaloguj się do Menedżera DNS domeny
* Tworzenie nowego CNAME *awverify*
* Polecenie *awverify* *awverify. YOUR_DOMAIN.azurewebsites.NET*

Może upłynąć trochę czasu przejść do pełnego efektu, więc jeśli poniższe czynności nie działa, przejdź Filiżanka kawy, a następnie wróć i spróbuj ponownie zmiany DNS.

## <a name="add-the-domain-to-the-web-app"></a>Dodawanie domeny do aplikacji sieci web

Powróć do aplikacji sieci web przez Azure Portal, kliknij pozycję **Ustawienia**, a następnie kliknij **domen niestandardowych i SSL**.

Po wyświetleniu *Ustawienia protokołu SSL* zostanie wyświetlona pola miejsce, w którym będą wprowadzane wszystkie domeny, które chcesz przypisać do aplikacji sieci web. Jeśli domena nie ma na liście, nie będzie dostępny do mapowania wewnątrz WordPress, niezależnie od tego, jak domeny DNS jest skonfigurowana.

![Okno dialogowe domen niestandardowych Zarządzanie][wordpress-manage-domains]

Po wpisaniu domeny w polu tekstowym, Azure sprawdzi utworzonej wcześniej rekordu CNAME. Jeśli DNS nie jest w pełni propagowana, pojawi się czerwony wskaźnik. Jeśli go zakończyło się pomyślnie, zostanie wyświetlony zielony znacznik wyboru. 

Zwróć uwagę na adres IP na liście w dolnej części okna dialogowego. Należy tak skonfigurować rekord dla swojej domeny.

## <a name="setup-the-domain-a-record"></a>Konfigurowanie domeny rekordu

Jeśli kolejne kroki zostały pomyślnie, może teraz przypisać domeny do aplikacji sieci web Azure za pośrednictwem rekord A systemu DNS. 

Należy pamiętać, w tym miejscu że aplikacje Azure web zaakceptować zarówno rekordów CNAME i rekordów, jednak, *należy* użyć rekordu A, aby włączyć mapowanie domeny pisane z wielkiej litery. CNAME nie można przekazywać pod inny CNAME, czyli Azure utworzona dla Ciebie, za pomocą YOUR_DOMAIN.azurewebsites.net.

Za pomocą adresu IP w poprzednim kroku, wróć do Menedżera DNS i skonfigurować rekord, aby wskazywały tego adresów IP.


## <a name="install-and-setup-the-plugin"></a>Instalowanie i konfigurowanie wtyczkę

Wielooddziałowość WordPress nie ma obecnie wbudowaną metodą do zamapować domen niestandardowych. Istnieje jednak dodatek o nazwie [WordPress MU domeny mapowanie] [ wordpress-plugin-wordpress-mu-domain-mapping] które dodaje funkcji. Zaloguj się do administratora sieci części witryny i zainstaluj wtyczkę **WordPress MU domeny mapowanie** .

Po zainstalowaniu i aktywacji wtyczkę, odwiedź stronę **Ustawienia** > **Mapowanie domeny** , aby skonfigurować tę wtyczkę. W polu tekstowym pierwszy *Adres IP serwera*, wpisz adres IP umożliwia konfigurowanie rekordu A dla domeny. Ustaw dowolne *Opcje domeny* się (ustawienia domyślne często są poprawnie) i kliknij przycisk **Zapisz**.

## <a name="map-the-domain"></a>Mapowanie domeny

Odwiedź stronę **pulpitu nawigacyjnego** dla witryny, którą chcesz zamapować domeny. Kliknij menu **Narzędzia** > **Mapowanie domeny** i wpisz nową domenę w polu tekstowym i kliknij przycisk **Dodaj**.

Domyślnie w do domeny witryny wygenerowany automatycznie zostaną ponownie zapisać nową domenę. Jeśli chcesz mieć cały ruch wysyłane do nowej domeny, zaznacz pole *domenę podstawową dla tego blogu* przed zapisaniem. Możesz dodać dowolną liczbę domen w witrynie, ale może być tylko jeden podstawowy.

## <a name="do-it-again"></a>Zrobić to ponownie

Azure aplikacje sieci Web umożliwiają dodawanie nieograniczoną liczbę domen do aplikacji sieci web. Dodawanie innej domeny będzie trzeba wykonać w sekcjach **Weryfikowanie domeny** i **Konfigurowanie domeny rekord** dla każdej domeny.  

>[AZURE.NOTE] Jeśli chcesz rozpocząć pracę z Azure aplikacji usługi przed utworzeniem konta dla konta Azure, przejdź do [Spróbuj aplikacji usługi](http://go.microsoft.com/fwlink/?LinkId=523751), którym natychmiast można utworzyć aplikację sieci web krótkotrwałe starter w aplikacji usługi. Nie kart kredytowych wymagane; nie zobowiązania.

## <a name="whats-changed"></a>Informacje o zmianach
* Przewodnika do zmiany z witryn sieci Web do usługi aplikacji Zobacz: [Usługa Azure aplikacji i jego wpływ na istniejące usługi Azure](http://go.microsoft.com/fwlink/?LinkId=529714)

[ben-lobaugh]: http://ben.lobaugh.net
[ms-open-tech]: http://msopentech.com
[website-from-gallery]: https://www.windowsazure.com/develop/php/tutorials/website-from-gallery/
[wordpress-codex-create-a-network]: http://codex.wordpress.org/Create_A_Network
[website-w-mysql-and-ftp-ftp-setup]: https://www.windowsazure.com/develop/php/tutorials/website-w-mysql-and-ftp/#header-0
[website-w-mysql-and-git-git-setup]: https://www.windowsazure.com/develop/php/tutorials/website-w-mysql-and-git/#header-1
[wordpress-network-setup]: ./media/web-sites-php-convert-wordpress-multisite/wordpress-network-setup.png
[wordpress-codex-types-of-networks]: http://codex.wordpress.org/Before_You_Create_A_Network#Types_of_multisite_network
[wordpress-plugin-wordpress-mu-domain-mapping]: http://wordpress.org/extend/plugins/wordpress-mu-domain-mapping/

[wordpress-manage-domains]: ./media/web-sites-php-convert-wordpress-multisite/wordpress-manage-domains.png

 
