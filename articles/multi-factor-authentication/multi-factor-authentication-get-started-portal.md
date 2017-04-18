<properties 
    pageTitle="Rozmieszczanie portalu użytkownika dla serwera uwierzytelniania wieloskładnikowego Azure"
    description="To jest strona uwierzytelnianie wieloskładnikowe Azure opisujący sposób rozpocząć pracę z Azure MFA i portalu użytkownika."
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
    ms.date="08/15/2016"
    ms.author="kgremban"/>

# <a name="deploying-the-user-portal-for-the-azure-multi-factor-authentication-server"></a>Rozmieszczanie portalu użytkownika dla serwera uwierzytelniania wieloskładnikowego Azure

Portal użytkownika umożliwia administratorowi zainstalować i skonfigurować portalu użytkownika uwierzytelnianie wieloskładnikowe Azure. Portal użytkownika jest witryny sieci web usług IIS, która umożliwia użytkownikom programu uwierzytelnianie wieloskładnikowe Azure i zachować swoje konta. Użytkownik może zmienić numer telefonu, zmienianie ich numeru PIN lub pominięcie uwierzytelnianie wieloskładnikowe Azure podczas jego następnego logowania na.

Użytkownicy będą zalogować się do portalu użytkownika przy użyciu ich normalnego nazwy użytkownika i hasła i będzie połączenia uwierzytelnianie wieloskładnikowe Azure albo odpowiedzi na pytania zabezpieczeń do wykonania ich uwierzytelniania. Jeśli rejestracja użytkownika jest dozwolony, użytkownik skonfiguruje jego numer telefonu i numeru PIN zalogowaniu się do portalu użytkownika po raz pierwszy.

Administratorzy portalu użytkowników może skonfigurować i udzielić uprawnień do dodawania nowych użytkowników i aktualizowania istniejących użytkowników.

<center>![Konfiguracja](./media/multi-factor-authentication-get-started-portal/install.png)</center>

## <a name="deploying-the-user-portal-on-the-same-server-as-the-azure-multi-factor-authentication-server"></a>Wdrażanie portalu użytkownika na tym samym serwerze co serwer uwierzytelnianie wieloskładnikowe Azure

Następujące wymagania wstępne są wymagane do instalacji portalu użytkownicy na tym samym serwerze co serwer uwierzytelnianie wieloskładnikowe Azure:

- Usług IIS należy zainstalować tym asp.net oraz zgodności podstawowej meta usług IIS 6 (dla usług IIS 7 lub nowszy)
- Zalogowany użytkownik musi mieć uprawnienia administratora dla komputera i domeny, w razie potrzeby.  Jest tak, ponieważ konto wymaga uprawnień do tworzenia grup zabezpieczeń usługi Active Directory.

### <a name="to-deploy-the-user-portal-for-the-azure-multi-factor-authentication-server"></a>Aby wdrożyć portalu użytkownika dla serwera uwierzytelnianie wieloskładnikowe Azure

1. W obrębie Azure serwera uwierzytelnianie wieloskładnikowe: kliknij ikonę portalu użytkownika w menu po lewej stronie, kliknij przycisk Zainstaluj portalu użytkownika.
1. Kliknij przycisk Dalej.
1. Kliknij przycisk Dalej.
1. Jeśli komputer jest dołączony do domeny i konfiguracji usługi Active Directory do zabezpieczania komunikacji między portalem użytkownika a usługą Azure uwierzytelnianie wieloskładnikowe są niepełne, pojawi się kroku Active Directory. Kliknij przycisk Dalej, aby automatycznie wypełnić tej konfiguracji.
1. Kliknij przycisk Dalej.
1. Kliknij przycisk Dalej.
1. Kliknij przycisk Zamknij.
1. Otwórz przeglądarkę sieci web z dowolnego komputera i przejdź do adresu URL, w którym zainstalowano portalu użytkownika (na przykład https://www.publicwebsite.com/MultiFactorAuth). Upewnij się, są wyświetlane nie ostrzeżenia certyfikatu lub błędy.

<center>![Konfiguracja](./media/multi-factor-authentication-get-started-portal/portal.png)</center>

## <a name="deploying-the-azure-multi-factor-authentication-server-user-portal-on-a-separate-server"></a>Wdrażanie Portal Azure uwierzytelnianie wieloskładnikowe Server użytkownika na oddzielnym serwerze

Aby można było używać aplikacji uwierzytelnianie wieloskładnikowe Azure, następujące czynności są wymagane aplikacja może pomyślnie połączyć się z portalu użytkownika:

Zobacz wymagania sprzętowe i programowe dla wymagania dotyczące sprzętu i oprogramowania:

- Należy używać 6.0 lub nowszym serwera uwierzytelnianie wieloskładnikowe Azure.
- Portal użytkownika musi być zainstalowana na serwerze sieci web z Internetu systemem Microsoft® Internet Information Services (IIS) 6.x, usług IIS 7.x lub nowszy.
- Podczas korzystania z usług IIS 6.x, upewnij się, ASP.NET v2.0.50727 jest zainstalowany, zarejestrowanych i dozwolonych.
- Wymagane usługi ról w przypadku korzystania z usług IIS 7.x lub nowszy obejmują ASP.NET i zgodność z metabazą usług IIS 6.
- Portal użytkownika powinny być zabezpieczone przy użyciu certyfikatu SSL.
- Zestaw SDK usługi sieci Web uwierzytelnianie wieloskładnikowe Azure musi być zainstalowana w programie IIS 6.x, usług IIS 7.x lub nowszy na serwerze, serwer uwierzytelniania wieloskładnikowego Azure na którym jest zainstalowany.
- Zestaw SDK usługi sieci Web uwierzytelnianie wieloskładnikowe Azure muszą być zabezpieczone przy użyciu certyfikatu SSL.
- Portal użytkownik musi mieć możliwość nawiązywania połączenia z zestawu SDK usługi sieci Web uwierzytelnianie wieloskładnikowe Azure przez SSL.
- Portal użytkownika muszą mieć możliwość uwierzytelnienia do Azure wieloskładnikowe uwierzytelniania sieci Web usługi SDK przy użyciu poświadczeń konta usługi, który jest członkiem grupy zabezpieczeń o nazwie "PhoneFactor administratorów". To konto usługi i grupy istnieje w usłudze Active Directory, jeśli serwer uwierzytelnianie wieloskładnikowe Azure działa na serwerze domeny. Na serwerze uwierzytelnianie wieloskładnikowe Azure istnieje lokalnie tego konta usługi i grupy, ponieważ nie jest dołączony do domeny.

Instalowanie portalu użytkownika na serwerze niż serwer uwierzytelniania wieloskładnikowego Azure wymaga trzech kroków:

1. Instalowanie usługi sieci web SDK
2. Instalowanie portalu użytkownika
3. Konfigurowanie ustawień portalu użytkownika na serwerze Azure uwierzytelnianie wieloskładnikowe


### <a name="install-the-web-service-sdk"></a>Instalowanie usługi sieci web SDK

Jeśli zestaw SDK usługi sieci Web uwierzytelnianie wieloskładnikowe Azure nie jest już zainstalowany na serwerze uwierzytelnianie wieloskładnikowe Azure, przejdź do tego serwera i Otwórz serwer uwierzytelniania wieloskładnikowego Azure. Kliknij ikonę SDK usługi sieci Web, kliknij pozycję SDK Instalowanie usługi sieci Web... przycisk i postępuj zgodnie z instrukcjami prezentowane. Muszą być zabezpieczone SDK usługi sieci Web przy użyciu certyfikatu SSL. Certyfikat z podpisem własnym jest poprawny w tym celu, ale musi być importowane do magazynu "Zaufane główne urzędy certyfikacji" komputera lokalnego konta na serwerze sieci web portalu użytkownika tak, aby zaufała ten certyfikat podczas inicjowania połączenia SSL.

<center>![Konfiguracja](./media/multi-factor-authentication-get-started-portal/sdk.png)</center>

### <a name="install-the-user-portal"></a>Instalowanie portalu użytkownika

Przed rozpoczęciem instalacji portalu użytkownika na serwerze osobne, należy pamiętać o następujących czynności:

- Warto Otwórz przeglądarkę sieci web na serwerze sieci web z Internetu i przejdź do adresu URL wprowadzony w pliku web.config SDK usługi sieci Web. Jeśli w przeglądarce można uzyskać dostęp do usługi sieci web pomyślnie, jej możesz Monituj o poświadczenia. Wprowadź nazwę użytkownika i hasło, które zostały wprowadzone w pliku web.config dokładnie tak, jak wygląda w pliku. Upewnij się, są wyświetlane nie ostrzeżenia certyfikatu lub błędy.
- Jeśli odwrotnej serwera proxy lub zapory znajduje się przed portalu użytkownika serwera sieci web i wykonywania odciążanie protokołu SSL, można edytować plik web.config portalu użytkownika i Dodaj następujący klucz do <appSettings> sekcji tak, aby zamiast https portalu użytkownika można użyć protokołu http. <add key="SSL_REQUIRED" value="false"/>

#### <a name="to-install-the-user-portal"></a>Aby zainstalować portalu użytkownika

1. Otwórz Eksploratora Windows na serwera uwierzytelnianie wieloskładnikowe Azure i przejdź do folderu, w którym zainstalowano serwer uwierzytelniania wieloskładnikowego Azure (przykład C:\Program Files\Multi czynniki uwierzytelniania serwera). Wybór 32-bitowej lub 64-bitowej wersji pakietu MultiFactorAuthenticationUserPortalSetup instalacji odpowiednio na serwerze portalu użytkownika zostanie zainstalowana na. Skopiuj plik instalacji na serwerze w Internecie.
2. Na serwerze sieci web w Internecie to plik Instalatora muszą być uruchamiane z uprawnieniami administratora. Najprostszym sposobem na tym jest Otwórz wiersz polecenia jako administrator i przejdź do lokalizacji, w której został skopiowany plik instalacji.
3. Uruchom plik instalacji MultiFactorAuthenticationUserPortalSetup64, Zmień nazwę witryny i katalogu wirtualnego, w razie potrzeby.
4. Po zakończeniu instalacji portalu użytkownika, przejdź do C:\inetpub\wwwroot\MultiFactorAuth (lub odpowiednim katalogu na podstawie nazwy katalogu wirtualnego) i edytowania pliku web.config.
5. Znajdź klucz USE_WEB_SERVICE_SDK i zmień wartość na wartość FAŁSZ na wartość PRAWDA. Odszukaj klawiszy WEB_SERVICE_SDK_AUTHENTICATION_USERNAME i WEB_SERVICE_SDK_AUTHENTICATION_PASSWORD i ustaw wartości na nazwę użytkownika i hasło konta usługi, które należy do grupy zabezpieczeń Administratorzy PhoneFactor grupy (zobacz sekcję wymagania powyżej). Pamiętaj wprowadzić nazwę użytkownika i hasło między znaki cudzysłowu na końcu wiersza (wartość = "" / >). Zaleca się używanie kwalifikowaną nazwę użytkownika (na przykład domena\nazwa_użytkownika lub machine\username)
6. Znajdź ustawienie pfup_pfwssdk_PfWsSdk i zmień wartość "http://localhost:4898/PfWsSdk.asmx" do określonego adresu URL SDK usługi sieci Web jest uruchomiony na serwerze uwierzytelnianie wieloskładnikowe Azure (np. https://computer1.domain.local/MultiFactorAuthWebServiceSdk/PfWsSdk.asmx). Ponieważ SSL jest używany dla tego połączenia, ponieważ certyfikat SSL zostanie wystawiony nazwa serwera i adres URL używany musi odpowiadać nazwie certyfikatu musi odwoływać się SDK usługi sieci Web, nazwa serwera, a nie adres IP. Jeśli nazwa serwera nie rozwiąże adresu IP na serwerze z Internetu, należy dodać wpis do pliku hosts na tym serwerze do zamapować nazwa serwera uwierzytelnianie wieloskładnikowe Azure na adres IP. Zapisz plik web.config, gdy zostaną wprowadzone zmiany.
7. Jeśli witryny sieci Web, że Portal użytkownik został zainstalowany w obszarze (np. Domyślna witryna sieci Web) nie został już binded z certyfikatem publicznie podpisane, zainstalować certyfikat na serwerze Jeśli nie jest już zainstalowany, otwórz Menedżera IIS i powiązać certyfikat z witryny sieci Web.
8. Otwórz przeglądarkę sieci web z dowolnego komputera i przejdź do adresu URL, w którym zainstalowano portalu użytkownika (na przykład https://www.publicwebsite.com/MultiFactorAuth). Upewnij się, są wyświetlane nie ostrzeżenia certyfikatu lub błędy.



## <a name="configure-the-user-portal-settings-in-the-azure-multi-factor-authentication-server"></a>Konfigurowanie ustawień portalu użytkownika na serwerze uwierzytelnianie wieloskładnikowe Azure
Teraz, gdy jest zainstalowana portalu, musisz skonfigurować serwer uwierzytelniania wieloskładnikowego Azure do pracy z portalem.

Serwer Azure uwierzytelnianie wieloskładnikowe oferuje kilka opcji portalu użytkownika.  Poniższa tabela zawiera listę tych opcji i explaination ich użycia.

Ustawienia portalu użytkownika|Opis|
:------------- | :------------- |
Adres URL portalu użytkownika| Pozwala na wprowadź adres URL miejsce, w którym znajduje się portalu.
Uwierzytelnianie podstawowe| Pozwala określić typ uwierzytelniania używany podczas logowania się do portalu.  Uwierzytelnianie systemu Windows, Radius lub LDAP.
Zezwalaj użytkownikom na logowanie się|Umożliwia użytkownikom wprowadź nazwę użytkownika i hasło na stronie logowania do portalu użytkownika.  Jeśli nie jest zaznaczone, pola będzie wyszarzona.
Zezwalaj na rejestracji użytkownika|Umożliwia użytkownikowi rejestracji w uwierzytelnianie wieloskładnikowe, nie podejmując je na ekranie konfiguracji, który monituje go, aby uzyskać dodatkowe informacje, takie jak numer telefonu.  Monit o telefonu umożliwia określenie numeru telefonu pomocniczej.  Monituj o innych firm token PRZYSIĘGĄ umożliwia użytkownikom określanie 3 token PRZYSIĘGĄ firmy.
Umożliwia użytkownikom inicjowanie obejścia One-Time| Dzięki temu użytkownicy zainicjować jednorazowego obejścia.  Jeśli zestawy użytkownika, które zajmie to konto go wpływają na kolejnym użytkownika znaki w.  Monituj o obejścia sekund zawiera użytkownika z polem, mogą zmienić domyślny 300 sekund.  W przeciwnym razie jednorazowego obejścia tylko będzie pasować do 300 sekund.
Zezwalaj użytkownikom na wybieranie metody| Umożliwia określenie ich podstawowe metody kontaktu.  Może to być rozmowy telefonicznej, wiadomości SMS, aplikacji dla urządzeń przenośnych lub token PRZYSIĘGĄ.
Zezwalaj użytkownikom na wybieranie języka|  Umożliwia użytkownikowi zmienić język używany na potrzeby rozmowy telefonicznej, wiadomości SMS, aplikacji dla urządzeń przenośnych lub token PRZYSIĘGĄ.
Zezwalaj użytkownikom na aktywowania aplikacji dla urządzeń przenośnych| Umożliwia generowanie kodu aktywacji, aby ukończyć proces aktywacji aplikacji dla urządzeń przenośnych, który jest używany na serwerze.  Można także ustawić liczbę urządzeń, który może to zrobić na.  Od 1 do 10.
Pytania dotyczące zabezpieczeń za pomocą jako bazowy|Pozwala na pytania dotyczące zabezpieczeń na wypadek uwierzytelnianie wieloskładnikowe kończy się niepowodzeniem.  Można określić liczbę pytania dotyczące zabezpieczeń, które należy odpowiedzieć pomyślnie.
Zezwalaj użytkownikom na skojarzony token PRZYSIĘGĄ innych firm| Umożliwia użytkownikom określanie tokenu PRZYSIĘGĄ innych firm.
Używanie token PRZYSIĘGĄ jako bazowy|Umożliwia wykorzystanie token PRZYSIĘGĄ w przypadku uwierzytelniania wieloskładnikowego kończy się niepowodzeniem.  Możesz także określić limit czasu sesji w minutach.
Włączanie rejestrowania|Umożliwia logowanie do portalu użytkownika.  Pliki dziennika znajdują się w: C:\Program Files\Multi czynniki uwierzytelniania Server\Logs.

Większość z tych ustawień są widoczne dla użytkowników po włączeniu i znaki użytkownika do portalu użytkownika.

![Ustawienia portalu użytkownika](./media/multi-factor-authentication-get-started-portal/portalsettings.png)



### <a name="to-configure-the-user-portal-settings-in-the-azure-multi-factor-authentication-server"></a>Aby skonfigurować ustawienia portalu użytkownika na serwerze uwierzytelnianie wieloskładnikowe Azure




1. W polu Serwer uwierzytelnianie wieloskładnikowe Azure kliknij ikonę portalu użytkownika. Na karcie Ustawienia wprowadź adres URL portalu użytkownika w polu tekstowym adres URL portalu użytkownika. Ten adres URL zostanie wstawiona do wiadomości e-mail, które są wysyłane do użytkowników, gdy zostaną zaimportowane na serwerze uwierzytelnianie wieloskładnikowe Azure Jeśli włączono funkcję wiadomości e-mail.
2. Wybierz pozycję Ustawienia, których chcesz użyć w portalu użytkownika. Na przykład jeśli użytkownicy mogą kontrolować ich metody uwierzytelniania, upewnij się, że jest zaznaczone pole wyboru Zezwalaj użytkownikom na wybieranie metody wraz z metod, których można wybierać.
3. Kliknij łącze Pomoc w prawym górnym rogu, aby uzyskać pomoc na temat dowolnego z ustawień wyświetlania.

<center>![Konfiguracja](./media/multi-factor-authentication-get-started-portal/config.png)</center>


## <a name="administrators-tab"></a>Karta administratorów
Ta karta umożliwia po prostu dodać użytkowników, którzy będą mieć uprawnienia administratora.  Podczas dodawania administrator, możesz precyzyjnie dostosować uprawnienia, które otrzymają.  W ten sposób można upewnić się, że tylko udzielić uprawnień wymaganych do administratora.  Po prostu kliknij przycisk Dodaj, a następnie wybierz pozycję i uprawnienia użytkowników i ich, a następnie kliknij przycisk Dodaj.

![Administratorzy portalu użytkownika](./media/multi-factor-authentication-get-started-portal/admin.png)


## <a name="security-questions"></a>Pytania dotyczące zabezpieczeń
Ta karta umożliwia określenie pytania dotyczące zabezpieczeń, potrzebne użytkownikom udzielić odpowiedzi na pytania dotyczące zabezpieczeń używanie alternatywnych opcji jest zaznaczone.  Azure wieloskładnikowe Authenticaton serwera zawiera domyślnych pytań, które są dostępne.  Możesz również zmienić kolejność lub dodając własne pytania.  Dodając własne pytania, możesz określić język chcesz tych pytanie, aby są także wyświetlane w.

![Pytania dotyczące zabezpieczeń portalu użytkownika](./media/multi-factor-authentication-get-started-portal/secquestion.png)


## <a name="passed-sessions"></a>Sesje przekazano

## <a name="saml"></a>SAML
Umożliwia konfigurowanie portalu użytkownika, aby zaakceptować roszczeń związanych z dostawcą tożsamości za pomocą SAML.  Możesz określić limit czasu sesji, Określ certyfikat weryfikacji i wylogowania przekierowania adresu URL.

![SAML](./media/multi-factor-authentication-get-started-portal/saml.png)

## <a name="trusted-ips"></a>Adresy IP usługi zaufane
Ta karta umożliwia określenie pojedynczych adresów IP lub zakresy adresów IP, które można dodać tak, że jeśli użytkownik jest zalogowaniu się z jednego z tych adresów IP, uwierzytelnianie wieloskładnikowe jest pomijane.

![Adresy IP zaufane portalu użytkownika](./media/multi-factor-authentication-get-started-portal/trusted.png)

## <a name="self-service-user-enrollment"></a>Rejestracja użytkownika
Jeśli chcesz, aby użytkownicy zalogować się i rejestracji musisz zaznaczyć Zezwalaj użytkownikom na identyfikator logowania w i Zezwalaj na opcje rejestrowania użytkownika. Pamiętaj, że wybrane ustawienia wpłynie na obsługi logowania użytkowników.

Na przykład gdy użytkownik loguje się do portalu użytkownika i kliknięciu przycisku Zaloguj, są one następnie przejść do strony Ustawienia użytkownika uwierzytelnianie wieloskładnikowe Azure.  W zależności od tego, jak skonfigurowano uwierzytelnianie wieloskładnikowe Azure użytkownik może być możliwe wybranie ich metody uwierzytelniania.  

Wybierz metodę uwierzytelniania połączeń głosowych lub zostały wstępnie skonfigurowana do korzystania z tej metody, strony wyświetli monit o wprowadzenie ich podstawowego numeru telefonu i numeru wewnętrznego w razie potrzeby.  Ta osoba może mieć również możliwość wprowadź numer telefonu.  

![Adresy IP zaufane portalu użytkownika](./media/multi-factor-authentication-get-started-portal/backupphone.png)

Jeśli użytkownik jest wymagany podczas uwierzytelniania za pomocą numeru PIN, strony będzie monitować ich tak, aby wprowadzić kod PIN.  Po wprowadzeniu ich numery telefonów i numeru PIN (jeśli dotyczy), użytkownik kliknie połączeń mnie teraz, aby przycisk uwierzytelniania.  Uwierzytelnianie wieloskładnikowe Azure wykona rozmowy telefonicznej uwierzytelniania użytkownika podstawowego numeru telefonu.  Użytkownik musi odbieranie połączeń telefonicznych i wprowadź ich numeru PIN (jeśli dotyczy) i naciśnij klawisz #, aby przejść do następnego kroku procesu rejestracji własny.   

Jeśli użytkownik wybiera metodę uwierzytelniania Tekst wiadomości SMS lub została wstępnie skonfigurowana do korzystania z tej metody, strony monituje użytkownika o jej numer telefonu komórkowego.  Jeśli użytkownik jest wymagany podczas uwierzytelniania za pomocą numeru PIN, strony będzie monitować ich tak, aby wprowadzić kod PIN.  Po wprowadzeniu jej numer telefonu i numeru PIN (jeśli dotyczy), użytkownik kliknie tekstu mnie teraz, aby przycisk uwierzytelniania.  Uwierzytelnianie wieloskładnikowe Azure wykona uwierzytelniania wiadomości SMS na telefon komórkowy użytkownika.  Użytkownik musi odbierać wiadomości SMS, zawierające jedną godzinę kodu (OTP) i odpowiadanie na wiadomości z tej OTP oraz ich numeru PIN, w razie potrzeby) aby przejść do następnego kroku procesu rejestracji własny.

![Portal użytkownika usługi SMS](./media/multi-factor-authentication-get-started-portal/text.png)   

Jeśli użytkownik wybiera metodę uwierzytelniania aplikacji dla urządzeń przenośnych lub została wstępnie skonfigurowana do korzystania z tej metody, strony monituje użytkownika do zainstalowania aplikacji uwierzytelnianie wieloskładnikowe Azure na jego urządzeniu i generowanie kodu aktywacji.  Po zainstalowaniu aplikacji uwierzytelnianie wieloskładnikowe Azure użytkownik kliknie przycisk Generuj kod aktywacji.    

>[AZURE.NOTE]Aby można było używać aplikacji uwierzytelnianie wieloskładnikowe Azure, użytkownik musi włączyć powiadomienia wypychane dla swoich urządzeń.

Na stronie zostanie wyświetlony kod aktywacji i adres URL wraz z obrazu kodu kreskowego.  Jeśli użytkownik jest wymagany podczas uwierzytelniania za pomocą numeru PIN, strony będzie monitować ich tak, aby wprowadzić kod PIN.  Użytkownik wprowadza kod aktywacji i adres URL do aplikacji uwierzytelnianie wieloskładnikowe Azure lub używa skanera kodów kreskowych, aby zeskanować obraz kodów kreskowych i kliknie przycisk Aktywuj.    

Po zakończeniu aktywacji użytkownik kliknie przycisk uwierzytelniania mnie teraz.  Uwierzytelnianie wieloskładnikowe Azure wykona uwierzytelniania użytkownika aplikacji dla urządzeń przenośnych.  Użytkownik musi wprowadź ich kod PIN (jeśli dotyczy) i naciśnij przycisk uwierzytelniania w ich aplikacji dla urządzeń przenośnych przejść do następnego kroku procesu rejestracji własny.  


Jeśli administrator skonfigurował serwer uwierzytelniania wieloskładnikowego Azure zbieranie zabezpieczeń pytań i odpowiedzi, użytkownik jest następnie przejść do strony pytania dotyczące zabezpieczeń.  Użytkownik musi wybierz cztery pytania dotyczące zabezpieczeń i podaj odpowiedzi na swoje pytania zaznaczonego.    

![Pytania dotyczące zabezpieczeń portalu użytkownika](./media/multi-factor-authentication-get-started-portal/secq.png)  

Rejestracja własny użytkownika zostało zakończone i użytkownik jest zalogowany do portalu użytkownika.  Użytkownicy mogą Zaloguj się ponownie do portalu użytkownika w dowolnym momencie w przyszłości, aby zmienić numery telefonów, numery PIN, metod uwierzytelniania i pytania dotyczące zabezpieczeń, jeśli jest to dozwolone przez administratorów.
