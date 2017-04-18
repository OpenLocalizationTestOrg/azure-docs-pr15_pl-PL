<properties
    pageTitle="Serwer Azure MFA za pomocą usług AD FS 2.0 | Microsoft Azure"
    description="To jest strona uwierzytelnianie wieloskładnikowe Azure opisujący sposób rozpocząć pracę z Azure MFA i usług AD FS 2.0."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/14/2016"
    ms.author="kgremban"/>

# <a name="secure-cloud-and-on-premises-resources-using-azure-multi-factor-authentication-server-with-ad-fs-20"></a>Zapewnienie chmury i lokalnych zasobów serwera uwierzytelniania wieloskładnikowego Azure za pomocą usług AD FS 2.0

Ten artykuł jest dla organizacji, które należą do Federacji usługi Azure Active Directory i chcesz, aby bezpiecznego zasoby, które są w lokalnej i w chmurze. Ochrona zasobów przy użyciu serwera uwierzytelnianie wieloskładnikowe Azure i skonfigurować ją do działania z usług AD FS, dlatego weryfikacja dwuetapowa zostanie wywołany przez duża liczba punktów końcowych.

Ta dokumentacja obejmuje serwer uwierzytelniania wieloskładnikowego Azure za pomocą usług AD FS 2.0.  Uzyskaj więcej informacji na temat [Zabezpieczanie chmury i lokalnych zasobów za pomocą serwera uwierzytelniania wieloskładnikowego Azure z systemem Windows Server 2012 R2 AD FS](multi-factor-authentication-get-started-adfs-w2k12.md).


## <a name="secure-ad-fs-20-with-a-proxy"></a>Zabezpieczanie usług AD FS 2.0 z serwera proxy
Aby zabezpieczyć usług AD FS 2.0 z serwera proxy, zainstaluj serwer uwierzytelniania wieloskładnikowego Azure na serwerze proxy usług ADFS i konfigurowanie serwera.

### <a name="configure-iis-authentication"></a>Konfigurowanie uwierzytelniania usług IIS

1. Na serwerze uwierzytelnianie wieloskładnikowe Azure kliknij ikonę **Uwierzytelnianie usług IIS** w menu po lewej stronie.
2. Kliknij kartę **Oparte na formularzach** .
3. Kliknij przycisk **Dodaj** przycisk.
<center>![Konfiguracja](./media/multi-factor-authentication-get-started-adfs-adfs2/setup1.png)</center>
4. Automatyczne wykrywanie nazwę użytkownika, hasło i zmienne domeny, wprowadź adres URL logowania (na przykład https://sso.contoso.com/adfs/ls) w oknie dialogowym Auto-Configure Form-Based witryny sieci Web, a następnie kliknij przycisk OK.
5. Zaznacz pole **Uwzględnij użytkownika wymaga uwierzytelniania wieloskładnikowego Azure** wszystkich użytkowników zostały lub zostaną zaimportowane na serwerze i objęte Weryfikacja dwuetapowa. Jeśli znacząca liczba użytkowników nie został zaimportowany na serwerze i/lub będzie Wyklucz z Weryfikacja dwuetapowa, nie zaznaczaj pola wyboru. Aby uzyskać dodatkowe informacje na temat tej funkcji zobacz plik pomocy.
6. Jeśli nie można automatycznie wykryć zmienne strony, kliknij pozycję **Określ ręcznie...** przycisk w oknie dialogowym Auto-Configure Form-Based witryny sieci Web.
7. W oknie dialogowym Add Form-Based witryny sieci Web wprowadź adres URL strony logowania ADFS w polu Prześlij adres URL (na przykład https://sso.contoso.com/adfs/ls) i wprowadź nazwę aplikacji (opcjonalnie). Nazwa aplikacji jest wyświetlana w raportach uwierzytelnianie wieloskładnikowe Azure i mogą być wyświetlane w wiadomości SMS lub aplikacji Mobile wiadomości uwierzytelniania. Zobacz plik pomocy, aby uzyskać więcej informacji na adres URL przesyłanie.
8. Ustawianie formatu wezwanie "OPUBLIKOWAĆ lub uzyskaj".
9. Wprowadź zmienną Username (ctl00$ ContentPlaceHolder1$ UsernameTextBox) i zmiennej hasła (ctl00$ ContentPlaceHolder1$ PasswordTextBox). Jeśli stronę logowania oparte na formularzach są wyświetlane pola tekstowego domeny, wprowadź także zmienną domeny. Może być konieczne, przejdź do strony logowania w przeglądarce sieci web, kliknij prawym przyciskiem myszy stronę i wybierz pozycję **Pokaż źródło** , aby znaleźć nazwę pola wprowadzania danych na stronie logowania.
10. Zaznacz pole **Uwzględnij użytkownika wymaga uwierzytelniania wieloskładnikowego Azure** wszystkich użytkowników zostały lub zostaną zaimportowane na serwerze i objęte Weryfikacja dwuetapowa. Jeśli znacząca liczba użytkowników nie został zaimportowany na serwerze i/lub będzie Wyklucz z Weryfikacja dwuetapowa, nie zaznaczaj pola wyboru.
<center>![Konfiguracja](./media/multi-factor-authentication-get-started-adfs-adfs2/manual.png)</center>
11. Kliknij przycisk **Zaawansowane...** przycisk, aby przejrzeć ustawienia zaawansowane. Można skonfigurować ustawienia, łącznie z możliwością wybierz plik odmowa niestandardowej strony do buforowania pomyślnego uwierzytelnienia do witryny sieci Web przy użyciu plików cookie i wybrać sposób uwierzytelniania podstawowego.
12. Ponieważ serwer proxy usług ADFS nie mogą być dołączane do domeny, można użyć LDAP nawiązywania połączenia z kontrolera domeny dla użytkownika i wstępnego uwierzytelniania. W oknie dialogowym Advanced Form-Based witryny sieci Web kliknij kartę **Uwierzytelniania podstawowego** , a następnie wybierz **Wiązania LDAP** typ uwierzytelniania wstępnego uwierzytelniania.
13. Po zakończeniu kliknij przycisk **OK** , aby powrócić do okna dialogowego Add Form-Based witryny sieci Web. Zobacz plik pomocy, aby uzyskać więcej informacji na temat zaawansowanych ustawień.
14. Kliknij przycisk **OK** , aby zamknąć okno dialogowe.
15. Po zmienne adresu URL i strony zostały wykryte lub wprowadzono, dane witryn sieci Web zostanie wyświetlone w panelu oparte na formularzach.
16. Kliknij kartę **Modułu natywne** i wybierz serwer, witryny sieci Web, którym serwer proxy usług ADFS działa w obszarze (takie jak "Domyślna witryna sieci Web") lub aplikacji serwera proxy usług ADFS organizacji (na przykład "ls" w obszarze "adfs") Aby włączyć Internetowe usługi informacyjne wtyczki na żądany poziom.
17. Kliknij pole **Uwierzytelnianie Włączanie usług IIS** w górnej części ekranu.
18. Uwierzytelnianie usług IIS jest teraz włączone.

### <a name="configure-directory-integration"></a>Konfigurowanie ustawień integracji katalogów

Jeśli włączono uwierzytelnianie usług IIS, ale do wykonania przed uwierzytelniania do usługi Active Directory (AD) za pomocą LDAP należy skonfigurować połączenie LDAP do kontrolera domeny.

1. Kliknij ikonę **Integracji katalogów** .
2. Na karcie Ustawienia zaznacz opcję **Użyj określonych konfiguracji LDAP** .
<center>![Konfiguracja](./media/multi-factor-authentication-get-started-adfs-adfs2/ldap1.png)</center>
3. Kliknij pozycję **Edytuj** przycisk.
4. W oknie dialogowym Edytowanie konfiguracji LDAP Wypełnij pola informacje wymagane do nawiązania kontrolera domeny AD. W poniższej tabeli znajdują się opisy pól. Te informacje są również uwzględniane w pliku pomocy serwera uwierzytelniania wieloskładnikowego Azure.
5. Testowanie połączenia LDAP, klikając przycisk **Test** .
<center>![Konfiguracja](./media/multi-factor-authentication-get-started-adfs-adfs2/ldap2.png)</center>
6. Jeśli LDAP testowanie połączenia zakończyło się pomyślnie, kliknij przycisk **OK** .

### <a name="configure-company-settings"></a>Konfigurowanie ustawień firmy

1. Następnie kliknij ikonę **Ustawienia firmy** i wybierz kartę **Rozpoznawanie nazwy użytkownika** .
2. Wybierz przycisk radiowy **atrybut Unikatowy identyfikator LDAP użycia dla pasujących użytkowników** .
3. Jeśli użytkownicy będą wprowadzać ich nazwy użytkownika w formacie "domena\nazwa_użytkownika", serwer trzeba będzie można usunąć domeny poza nazwa użytkownika, podczas tworzenia kwerendy LDAP. Którą można zrobić za pomocą ustawienia rejestru.
4. Otwórz Edytor rejestru i przejdź do HKEY_LOCAL_MACHINE i oprogramowania-Wow6432Node-dodatnie sieci i PhoneFactor na serwerze 64-bitowej. Jeśli na serwerze 32-bitową, skorzystaj z "Wow6432Node" poza ścieżkę. Utwórz klucz rejestru DWORD o nazwie "UsernameCxz_stripPrefixDomain", a następnie ustaw wartość 1. Uwierzytelnianie wieloskładnikowe Azure jest teraz Zabezpieczanie serwera proxy usług ADFS organizacji.

Upewnij się, że użytkownicy zostały zaimportowane z usługi Active Directory do serwera. Jeśli chcesz listy sprawdzonej adresów IP, aby Weryfikacja dwuetapowa nie jest wymagane, po zalogowaniu się do witryny sieci Web z tych lokalizacji można znaleźć w [sekcji zaufane adresy IP](#trusted-ips) .

<center>![Konfiguracja](./media/multi-factor-authentication-get-started-adfs-adfs2/reg.png)</center>

## <a name="ad-fs-20-direct-without-a-proxy"></a>Usług AD FS 2.0 bezpośredni bez serwera proxy

Można zabezpieczyć AD FS, gdy nie jest używany serwer proxy usług AD FS. Zainstaluj serwer uwierzytelniania wieloskładnikowego Azure na serwerze usług AD FS i skonfigurować na serwerze na następujące czynności:

1. W obrębie serwera uwierzytelnianie wieloskładnikowe Azure kliknij ikonę **Uwierzytelnianie usług IIS** w menu po lewej stronie.
2. Kliknij kartę **HTTP** .
3. Kliknij przycisk **Dodaj** przycisk.
4. W okno dialogowe Dodawanie podstawowy adres URL wprowadź adres URL miejsce, w którym HTTP uwierzytelniania (na przykład https://sso.domain.com/adfs/ls/auth/integrated) w polu podstawowy adres URL witryny sieci Web usług ADFS organizacji. Wpisz nazwę aplikacji (opcjonalnie). Nazwa aplikacji jest wyświetlana w raportach uwierzytelnianie wieloskładnikowe Azure i mogą być wyświetlane w wiadomości SMS lub aplikacji Mobile wiadomości uwierzytelniania.
5. W razie potrzeby dostosuj limit czasu bezczynności i maksymalnej długości sesji.
6. Zaznacz pole **Uwzględnij użytkownika wymaga uwierzytelniania wieloskładnikowego Azure** wszystkich użytkowników zostały lub zostaną zaimportowane na serwerze i objęte Weryfikacja dwuetapowa. Jeśli znacząca liczba użytkowników nie został zaimportowany na serwerze i/lub będzie Wyklucz z Weryfikacja dwuetapowa, nie zaznaczaj pola wyboru. Aby uzyskać dodatkowe informacje na temat tej funkcji zobacz plik pomocy.
7. Zaznacz pole pamięci podręcznej plików cookie w razie potrzeby.
<center>![Konfiguracja](./media/multi-factor-authentication-get-started-adfs-adfs2/noproxy.png)</center>
8. Kliknij przycisk **OK** .
9. Kliknij kartę **Modułu natywne** i zaznacz serwer, witryny sieci Web (takie jak "Domyślna witryna sieci Web") lub aplikacji usług ADFS organizacji (na przykład "ls" w obszarze "adfs"), aby włączyć Internetowe usługi informacyjne wtyczki na żądany poziom.
10. Kliknij pole **Uwierzytelnianie Włączanie usług IIS** w górnej części ekranu. Uwierzytelnianie wieloskładnikowe Azure jest teraz zabezpieczanie usług ADFS organizacji.

Upewnij się, że użytkownicy zostały zaimportowane z usługi Active Directory do serwera. Zobacz sekcję zaufane adresy IP, jeśli chcesz listy sprawdzonej adresów IP, aby Weryfikacja dwuetapowa nie jest wymagane, po zalogowaniu się do witryny sieci Web z tych lokalizacji.


## <a name="trusted-ips"></a>Adresy IP zaufanych
Adresy IP zaufanych umożliwia użytkownikom pomijanie uwierzytelnianie wieloskładnikowe Azure dla witryny sieci Web żądań pochodzących z określonych adresów IP lub podsieci. Na przykład być może zechcesz wyklucz użytkowników na potrzeby weryfikacji dwuetapowej po zalogowaniu się w z pakietu office. W tym należy określić podsieci pakietu office jako pozycję Zaufane adresy IP.

### <a name="to-configure-trusted-ips"></a>Aby skonfigurować zaufane adresy IP


1. W sekcji uwierzytelnianie usług IIS kliknij kartę **Zaufane adresy IP** .
1. Kliknij przycisk **Dodaj** przycisk.
1. Gdy pojawi się okno dialogowe Dodaj adresy IP zaufane, wybierz jeden z przycisków radiowych **Pojedynczej IP**, **zakres adresów IP**lub **podsieci** .
1. Wprowadź adres IP, zakres adresów IP lub podsieci, który powinien być whitelisted. Jeśli wprowadzanie podsieci, zaznacz odpowiednie maski i kliknij przycisk **OK** . Zaufane IP został dodany.


<center>![Konfiguracja](./media/multi-factor-authentication-get-started-adfs-adfs2/trusted.png)</center>
