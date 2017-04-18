<properties 
    pageTitle="Uwierzytelnianie usług IIS i Azure serwera uwierzytelnianie wieloskładnikowe"
    description="Jest to strona uwierzytelnianie wieloskładnikowe Azure, który pomoże wdrażania uwierzytelnianie usług IIS i serwer uwierzytelniania wieloskładnikowego Azure."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="iis-authentication"></a>Uwierzytelnianie usług IIS

W sekcji uwierzytelnianie usług IIS serwer uwierzytelniania wieloskładnikowego Azure umożliwia włączanie i konfigurowanie uwierzytelniania IIS do integracji z aplikacjami sieci web programu Microsoft IIS. Serwer uwierzytelniania wieloskładnikowego Azure spowoduje zainstalowanie wtyczki, które można filtrować wniosków składanych na serwerze sieci web usług IIS, aby można było dodać uwierzytelnianie wieloskładnikowe Azure. Wtyczki IIS obsługuje uwierzytelnianie oparte na formularzach i zintegrowane uwierzytelnianie HTTP systemu Windows. Adresy IP można również skonfigurować do Wyklucz adresów IP z uwierzytelnianie dwuskładnikowe zaufanych.


![Uwierzytelnianie usług IIS](./media/multi-factor-authentication-get-started-server-iis/iis.png)


## <a name="using-form-based-iis-authentication-with-azure-multi-factor-authentication-server"></a>Uwierzytelnianie oparte na formularzach usług IIS za pomocą serwera Azure uwierzytelnianie wieloskładnikowe

Aby zabezpieczyć aplikacji sieci web usług IIS, w której jest używane uwierzytelnianie oparte na formularzach, zainstaluj serwer uwierzytelniania wieloskładnikowego Azure na serwerze sieci web usług IIS i skonfiguruj na serwerze na poniższą procedurę.

1. W obrębie serwera uwierzytelnianie wieloskładnikowe Azure kliknij ikonę uwierzytelnianie usług IIS w menu po lewej stronie.
2. Kliknij kartę oparte na formularzach.
3. Kliknij przycisk Dodaj. przycisk.
4. Wykrywanie nazwę użytkownika, hasło i domenę zmienne automatycznie, wprowadź adres URL logowania (na przykład https://localhost/contoso/auth/login.aspx) w oknie dialogowym Auto-Configure Form-Based witryny sieci Web i kliknij przycisk OK.
5. Zaznacz pole Uwzględnij użytkownika wymaga uwierzytelniania wieloskładnikowego wszystkich użytkowników zostały lub zostaną zaimportowane na serwerze i objęte uwierzytelnianie wieloskładnikowe. Jeśli znacząca liczba użytkowników nie został zaimportowany na serwerze i/lub będzie Wyklucz z uwierzytelnianie wieloskładnikowe, nie zaznaczaj pola wyboru.
6. Jeśli nie można automatycznie wykryć zmienne strony, kliknij przycisk Określ ręcznie. przycisk w oknie dialogowym Auto-Configure Form-Based witryny sieci Web.
7. W oknie dialogowym Add Form-Based witryny sieci Web wprowadź adres URL strony logowania w polu adres URL przesyłanie i wprowadź nazwę aplikacji (opcjonalnie). Nazwa aplikacji jest wyświetlana w raportach uwierzytelnianie wieloskładnikowe Azure i mogą być wyświetlane w wiadomości SMS lub aplikacji Mobile wiadomości uwierzytelniania. Zobacz plik pomocy, aby uzyskać więcej informacji na adres URL przesyłanie.
8. Wybierz poprawny wezwanie. To jest równa "WPIS lub Pobierz" dla większości aplikacji sieci web.
9. Wprowadź zmiennej nazwę użytkownika, hasło zmiennej i zmiennej domeny (jeśli jest wyświetlany na stronie logowania). Może być konieczne, przejdź do strony logowania w przeglądarce sieci web, kliknij prawym przyciskiem myszy stronę i wybierz pozycję "Pokaż źródło", aby odnaleźć nazwy pola wprowadzania danych na stronie.
10. Zaznacz pole Uwzględnij użytkownika wymaga uwierzytelniania wieloskładnikowego Azure wszystkich użytkowników zostały lub zostaną zaimportowane na serwerze i objęte uwierzytelnianie wieloskładnikowe. Jeśli znacząca liczba użytkowników nie został zaimportowany na serwerze i/lub będzie Wyklucz z uwierzytelnianie wieloskładnikowe, nie zaznaczaj pola wyboru. Zobacz plik pomocy, aby uzyskać dodatkowe informacje na temat tej funkcji.
11.  Kliknij przycisk Zaawansowane. przycisk, aby przejrzeć zaawansowanych ustawień, łącznie z możliwością wybierz plik odmowa niestandardowej strony pamięci podręcznej pomyślnego uwierzytelnienia do witryny sieci Web w okresie przy użyciu plików cookie i określ, czy do uwierzytelniania podstawowego przed domeny systemu Windows, katalogu LDAP lub serwera RADIUS. Po zakończeniu kliknij przycisk OK, aby powrócić do okna dialogowego Add Form-Based witryny sieci Web. Zobacz plik pomocy, aby uzyskać więcej informacji na temat zaawansowanych ustawień.
12. Kliknij przycisk OK.
13. Dane witryn sieci Web zmienne adresu URL i strony zostały wykryte lub wprowadzona, będzie wyświetlany w panelu oparte na formularzach.
14. Zobacz Włączanie usług IIS wtyczki dla bezpośrednio poniżej serwer uwierzytelniania wieloskładnikowego Azure aby ukończyć konfigurację uwierzytelniania usług IIS.

## <a name="using-integrated-windows-authentication-with-azure-multi-factor-authentication-server"></a>Przy użyciu zintegrowanego uwierzytelniania systemu Windows z serwerem Azure uwierzytelnianie wieloskładnikowe

Aby zabezpieczyć aplikacji sieci web usług IIS, w której jest używane uwierzytelnianie zintegrowane Windows HTTP, zainstaluj serwer uwierzytelniania wieloskładnikowego Azure na serwerze sieci web usług IIS i skonfigurować serwer na poniższej procedury.

1. W obrębie serwera uwierzytelnianie wieloskładnikowe Azure kliknij ikonę uwierzytelnianie usług IIS w menu po lewej stronie.
2. Kliknij kartę HTTP. Kliknij kartę oparte na formularzach.
3. Kliknij przycisk Dodaj. przycisk.
4. W okno dialogowe Dodawanie podstawowy adres URL wprowadź adres URL dla witryny sieci Web, gdzie uwierzytelniania protokołu HTTP jest wykonywane (np. http://localhost/owa) w polu podstawowy adres URL i wprowadź nazwę aplikacji (opcjonalnie). Nazwa aplikacji jest wyświetlana w raportach uwierzytelnianie wieloskładnikowe Azure i mogą być wyświetlane w wiadomości SMS lub aplikacji Mobile wiadomości uwierzytelniania.
5. Dostosować limit czasu bezczynności i maksymalnej długości sesji nie wystarcza domyślny.
6. Zaznacz pole Uwzględnij użytkownika wymaga uwierzytelniania wieloskładnikowego wszystkich użytkowników zostały lub zostaną zaimportowane na serwerze i objęte uwierzytelnianie wieloskładnikowe. Jeśli znacząca liczba użytkowników nie został zaimportowany na serwerze i/lub będzie Wyklucz z uwierzytelnianie wieloskładnikowe, nie zaznaczaj pola wyboru. Zobacz plik pomocy, aby uzyskać dodatkowe informacje na temat tej funkcji.
7. Zaznacz pole pamięci podręcznej plików cookie w razie potrzeby.
8. Kliknij przycisk OK.
9. W sekcji [Włączanie usług IIS wtyczki dla serwera uwierzytelnianie wieloskładnikowe Azure](#enable-iis-plug-ins-for-azure-multi-factor-authentication-server) bezpośrednio poniżej, aby ukończyć konfigurację uwierzytelniania usług IIS.


## <a name="enable-iis-plug-ins-for-azure-multi-factor-authentication-server"></a>Włączanie usług IIS wtyczki serwer Azure uwierzytelnianie wieloskładnikowe

Po skonfigurowaniu oparte na formularzach adresy URL uwierzytelniania protokołu HTTP i ustawień możesz wybrać lokalizacji, w której ładowane i włączone wtyczkami programu IIS uwierzytelnianie wieloskładnikowe Azure w programie IIS. Wykonaj poniższe kroki:

1. Jeśli uruchomionych usług IIS 6, kliknij kartę ISAPI i wybierz witrynę sieci Web, która aplikacji sieci web jest uruchomiony w obszarze (np. Domyślna witryna sieci Web), aby włączyć filtr ISAPI uwierzytelnianie wieloskładnikowe Azure wtyczki dla tej witryny.
2. Jeśli zainstalowany na usług IIS 7 lub nowszy, kliknij kartę modułu natywne i wybierz serwer, witryny sieci Web lub aplikacje, aby włączyć Internetowe usługi informacyjne wtyczki na odpowiednie poziomy.
3. Kliknij pole uwierzytelniania Włączanie usług IIS w górnej części ekranu. Uwierzytelnianie wieloskładnikowe Azure jest teraz zabezpieczanie wybranej aplikacji usług IIS. Upewnij się, że użytkownicy zostały zaimportowane do serwera. Zobacz poniższą sekcję zaufane adresy IP, jeśli chcesz listy sprawdzonej adresy IP wewnętrznych, że uwierzytelnianie dwuskładnikowe nie jest wymagane podczas logowania do witryny sieci Web z tych lokalizacji.


## <a name="trusted-ips"></a>Adresy IP zaufanych

Adresy IP zaufane umożliwia pomijanie uwierzytelnianie wieloskładnikowe Azure dla witryny sieci Web żądań pochodzących z określonych adresów IP lub podsieci. Na przykład być może zechcesz wyklucz użytkowników z uwierzytelnianie wieloskładnikowe Azure podczas logowania się z pakietu office. W tym należy określić podsieci pakietu office jako wpisu zaufane adresy IP. Aby skonfigurować zaufane adresy IP użyć następującej procedury:

1. W sekcji uwierzytelnianie usług IIS kliknij kartę zaufane adresy IP.
2. Kliknij przycisk Dodaj. przycisk.
3. Gdy pojawi się okno dialogowe Dodaj adresy IP zaufane, zaznacz pojedynczy IP, zakres adresów IP lub przycisk radiowy podsieć adresów.
4. Wprowadź adres IP, zakres adresów IP lub podsieci, który powinien być whitelisted. Jeśli wprowadzanie podsieci, zaznacz odpowiednie maski i kliknij przycisk OK. Listy sprawdzonej został dodany.
