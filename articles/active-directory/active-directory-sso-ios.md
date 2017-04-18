<properties
    pageTitle="Jak włączyć logowania jednokrotnego krzyżowe aplikacji w systemie iOS przy użyciu ADAL | Microsoft Azure"
    description="Jak korzystać z funkcji ADAL zestawu SDK umożliwiające rejestracji jednokrotnej w aplikacji. "
    services="active-directory"
    documentationCenter=""
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="brandwe"/>


# <a name="how-to-enable-cross-app-sso-on-ios-using-adal"></a>Jak włączyć logowania jednokrotnego krzyżowe aplikacji w systemie iOS przy użyciu ADAL


Warunkiem pojedynczego logowania jednokrotnego (SSO) tak, aby użytkownicy są proszeni tylko raz wprowadź swoje poświadczenia i tych poświadczeń automatycznie pracy w całej aplikacji jest teraz planowane przez klientów. Trudności przy wprowadzaniu swojej nazwy użytkownika i hasła na małym ekranie, często razy połączone z dodatkowe czynniki (2FA) jak połączenie telefoniczne lub texted kod, wynikiem jest szybkie niezadowolenie, jeśli użytkownik ma w tym więcej niż jeden raz dla danego produktu.

Ponadto jeśli wykorzystać platformy tożsamości inne aplikacje mogą używać takich jak Accounts Microsoft lub konta służbowego z usługi Office 365, klienci oczekują, że te poświadczenia być dostępne dla wszystkich swoich aplikacji, niezależnie od dostawcy.

Platformy Microsoft Identity, wraz z naszych SDK tożsamości firmy Microsoft, nie ten wykonanej pracy dla Ciebie i daje możliwość doskonale dopasowanych do klientów dzięki logowania jednokrotnego w pakietu aplikacji lub, jak z możliwości brokera i aplikacje uwierzytelnienia, w całej urządzenia.

W tym instruktażu określić sposób konfigurowania naszych SDK w aplikacji mogła zapewniać te korzyści klientom.

W tym instruktażu dotyczy:

* Azure Active Directory
* B2C Azure Active Directory
* B2B Azure Active Directory
* Dostępu warunkowego Azure Active Directory


Zwróć uwagę, możesz mieć informacje o sposobach [aplikacji należy w portalu starsze dla usługi Azure Active Directory](./develop/active-directory-how-to-integrate.md) , a także jest zintegrowany aplikacji z [iOS tożsamości Microsoft SDK](https://github.com/AzureAD/azure-activedirectory-library-for-objc)przyjęto założenie dokument poniżej.

## <a name="sso-concepts-in-the-microsoft-identity-platform"></a>Pojęcia logowania jednokrotnego platformy Microsoft tożsamości

### <a name="microsoft-identity-brokers"></a>Przekazuje informacje dotyczące tożsamości firmy Microsoft

Microsoft zawiera aplikacje dla każdej platformy urządzeń przenośnych, umożliwiające łączące poświadczeń aplikacji z różnych dostawców także umożliwia specjalne rozszerzone funkcje, które wymagają pojedynczej bezpiecznym miejscu w przypadku do sprawdzania poprawności poświadczeń. Nazywamy tych **brokerów**. W systemie iOS i Android te są dostarczane za pomocą aplikacji do pobrania że klienci zainstalowana niezależnie lub może zostać przeniesiony do urządzenia zarządzającej niektóre lub wszystkie urządzenia ich pracownika firmy. Te brokerów obsługuje zarządzanie zabezpieczeń tylko dla niektórych aplikacji lub całego urządzenia, zależnie od najszybciej pozwala administratorom systemów informatycznych. W systemie Windows ta funkcja jest udostępniany przez wybór konta wbudowany w systemie operacyjnym, znane technicznego jako Broker uwierzytelniania sieci Web.

Aby dowiedzieć się, jak firma Microsoft korzysta z tych brokerów i jak klienci mogą one wyświetlane w ich przepływu logowania platformy Microsoft Identity Czytaj dalej, aby uzyskać więcej informacji.

### <a name="patterns-for-logging-in-on-mobile-devices"></a>Desenie do logowania na urządzeniach przenośnych

Dostęp do poświadczeń na urządzeniach wykonaj dwa podstawowe desenie platformy Microsoft Identity:

* Broker bez pomocy logowania
* Broker pomocy logowania

#### <a name="non-broker-assisted-logins"></a>Broker bez pomocy logowania

Logowania pomocy brokera osoby, które nie są środowiska logowania, które wystąpić w tekście z aplikacją i magazynu lokalnego na tym urządzeniu dla tej aplikacji. Przechowywania mogą być udostępniane między aplikacjami, ale poświadczenia są ściśle związane z aplikacji lub pakiet aplikacji za pomocą tego poświadczenia. To jest środowisko, który został prawdopodobnie wystąpił w wielu aplikacji dla urządzeń przenośnych, gdzie wprowadź nazwę użytkownika i hasło samej aplikacji.

Te logowania są następujące korzyści:

-  Środowisko użytkownika istnieje całkowicie w tej aplikacji.
-  Poświadczenia są udostępniane przez aplikacje, które zostały podpisane przez tego samego certyfikatu, dostarczając pojedynczy środowisko logowania jednokrotnego do pakietu aplikacji.
-  Kontrolka wokół obsługi logowania znajduje się do aplikacji przed i po logowania.

Te logowania są wady następujące czynności:

- Użytkownik nie możliwości logowania jednokrotnego na przez wszystkie aplikacje, które używają Identity Microsoft tylko w tych Microsoft Identities znajdujących się właścicielem i skonfigurowano aplikacji.
- Aplikacja nie można używać z bardziej zaawansowanych funkcji biznesowych, takich jak dostępu warunkowego lub użyj produktów pakietu InTune.
- Aplikacja nie obsługuje certyfikat oparty uwierzytelniania dla użytkowników biznesowych.

Oto reprezentację współpracy Microsoft SDK tożsamości z udostępnionej przestrzeni dyskowej aplikacji, aby włączyć logowania jednokrotnego:

```
+------------+ +------------+  +-------------+
|            | |            |  |             |
|   App 1    | |   App 2    |  |   App 3     |
|            | |            |  |             |
|            | |            |  |             |
+------------+ +------------+  +-------------+
| Azure SDK  | | Azure SDK  |  | Azure SDK   |
+------------+-+------------+--+-------------+
|                                            |
|            App Shared Storage              |
+--------------------------------------------+
```

#### <a name="broker-assisted-logins"></a>Broker pomocy logowania

Pomocy brokera logowania są środowiska logowania, które występują w aplikacji brokera i udostępnianie poświadczeń we wszystkich aplikacjach na tym urządzeniu, które używają platformy Microsoft Identity przy użyciu miejsca do magazynowania i zabezpieczenia brokera. Oznacza to, że aplikacje będzie zależeć brokera celu zaloguj użytkowników. W systemie iOS i Android te są dostarczane za pomocą aplikacji do pobrania że klienci zainstalowana niezależnie lub może zostać przeniesiony do urządzenia przez firmę, która zarządza urządzenia w celu ich użytkownika. Przykład ten typ aplikacji to aplikacja uwierzytelnienia Azure w systemie iOS. W systemie Windows ta funkcja jest udostępniany przez wybór konta wbudowany w systemie operacyjnym, znane technicznego jako Broker uwierzytelniania sieci Web.
Środowisko zależy od platformy i może być czasem przerw w pracy użytkowników w przeciwnym razie zarządzane poprawnie. Znasz prawdopodobnie najbardziej tego wzorca Jeśli masz zainstalowanej aplikacji Facebook i przy użyciu logowania Facebook w innej aplikacji. Platforma Microsoft Identity wykorzystuje takim samym wzorcem.

Dla systemu iOS, który prowadzi do "przejścia" to animacji, w której aplikacji jest wysyłana w tle podczas aplikacji uwierzytelnienia Azure jest na pierwszym planie dla użytkownika zaznaczyć którego konta, które mają być Zaloguj się przy użyciu.  

Dla systemu Android i systemu Windows wybór konta jest wyświetlana u góry aplikacji, która jest prostszy do użytkownika.

#### <a name="how-the-broker-gets-invoked"></a>Jak wywołać otrzymuje brokera

Jeśli zgodne broker jest zainstalowana na tym urządzeniu, jak w aplikacji uwierzytelnienia Azure SDK tożsamości programu Microsoft automatycznie wykona Praca wywoływanie brokera dla Ciebie, gdy użytkownik wskazuje, że chcą Zaloguj się przy użyciu dowolnego konta z platformy Microsoft Identity. Może to być osobistego Account Microsoft, utworu lub konta służbowego lub konta, które można wprowadzić i hosta w Azure za pomocą oferowanych produktów B2C i B2B. Przy użyciu bardzo bezpieczne algorytmy i szyfrowania firma Microsoft upewnij się, że poświadczenia są wymagane podanie i dostarczona do aplikacji w bezpieczny sposób. Dokładne szczegóły techniczne mechanizmy nie został opublikowany, ale opracowano z współpracy firmy Apple i Google.

**Deweloper ma wyboru, jeśli Microsoft SDK tożsamości połączeń brokera lub używa przepływu pomocy nie broker.** Jednak jeśli nie chcesz używać operatora brokera przepływu projektanta utracą zaletą używanie poświadczeń logowania jednokrotnego, że użytkownika zostało już dodane na tym urządzeniu, a także uniemożliwia ich zastosowanie od używany z funkcji firm, które firma Microsoft udostępnia klientom, takie jak dostępu warunkowego, funkcje zarządzania Intune i uwierzytelnianie na podstawie certyfikatu.

Te logowania są następujące korzyści:

-  Użytkownik występują logowania jednokrotnego we wszystkich aplikacjach ich niezależnie od dostawcy.
-  Aplikacja można korzystać z zaawansowanych funkcji biznesowych, takich jak dostępu warunkowego lub produktów pakietu InTune.
-  Aplikacja może obsługiwać podstawie certyfikatu uwierzytelniania dla użytkowników biznesowych.
- Bardziej bezpieczne logowanie tożsamością aplikacji i użytkownika są sprawdzane przez aplikacji brokera algorytmów dodatkowe zabezpieczenia i szyfrowania.

Te logowania są następujące wady:

- W systemie iOS użytkownika jest zmigrowanych możliwości aplikacji, gdy poświadczenia są wybrane.
- Utrata możliwość zarządzania logowania możliwości obsługi dla klientów w aplikacji.



Oto reprezentację działania Microsoft SDK tożsamości w aplikacjach brokera, aby włączyć logowania jednokrotnego:

```
+------------+ +------------+   +-------------+
|            | |            |   |             |
|   App 1    | |   App 2    |   |   Someone   |
|            | |            |   |    Else's   |
|            | |            |   |     App     |
+------------+ +------------+   +-------------+
| Azure SDK  | | Azure SDK  |   | Azure SDK   |
+-----+------+-+-----+------+-  +-------+-----+
      |              |                  |
      |       +------v------+           |
      |       |             |           |
      |       | Microsoft   |           |
      +-------> Broker      |^----------+
              | Application
              |             |
              +-------------+
              |             |
              |   Broker    |
              |   Storage   |
              |             |
              +-------------+
```

Wyposażyć z te dodatkowe informacje, które powinno być możliwe lepiej zrozumieć i wdrożenia logowania jednokrotnego w aplikacji przy użyciu platformy Microsoft Identity i SDK.


## <a name="enabling-cross-app-sso-using-adal"></a>Włączanie aplikacji między jednokrotnego logowania przy użyciu ADAL

W tym miejscu użyjemy ADAL iOS SDK do:

- Włączanie innych niż brokera pomocy rejestracji Jednokrotnej dla pakietu aplikacji
- Włączanie obsługi pomocy brokera logowania jednokrotnego

### <a name="turning-on-sso-for-non-broker-assisted-sso"></a>Włączanie rejestracji Jednokrotnej dla innych niż brokera pomocy logowania jednokrotnego

Dla innych niż brokera SSO pomocy w aplikacjach Microsoft SDK tożsamości Zarządzanie duża część złożoność logowania jednokrotnego dla Ciebie. Ta opcja uwzględnia Znajdowanie właściwego użytkownika w pamięci podręcznej i przechowywanie listy użytkowników zalogowany do kwerendy.

Aby włączyć logowania jednokrotnego w aplikacjach jesteś właścicielem, że musisz wykonać następujące czynności:

1. Upewnij się, użytkownika aplikacji tego samego identyfikator klienta lub identyfikator aplikacji.
* Upewnij się, dzięki czemu można udostępniać keychains wszystkie aplikacje udostępnianie tego samego certyfikatu podpisywania firmy Apple
* Żądanie uprawnienie Pęk kluczy dla każdej aplikacji.
* Informowanie o udostępnionej Pęk kluczy SDK tożsamości firmy Microsoft, czy chcesz, abyśmy za pomocą.

#### <a name="using-the-same-client-id--application-id-for-all-the-applications-in-your-suite-of-apps"></a>Przy użyciu tego samego Identyfikatora klienta / identyfikator aplikacji dla wszystkich aplikacji pakietu aplikacji

Aby platformy Microsoft Identity wiedzieć, że mogą być udostępnianie tokeny dla aplikacji poszczególnych aplikacji muszą udostępnić tego samego identyfikator klienta lub identyfikator aplikacji. To jest unikatowy identyfikator, który dostarczony podczas rejestrowania pierwszej aplikacji w portalu.

Być może zastanawiasz się jak należy określić poszczególnych aplikacji z usługą Microsoft Identity korzysta z tym samym identyfikator aplikacji. Odpowiedź jest **Przekieruj identyfikatory URI**. Każda aplikacja może mieć wiele URI przekierować zarejestrowane w portalu ułatwiającej rozpoczęcie korzystania. Każdej aplikacji w zestawie mają różne przekierowania URI. Jest przykładem to będzie wyglądać tak jak poniżej:

Przekierowanie App1 identyfikatora URI:`x-msauth-mytestiosapp://com.myapp.mytestapp`

Przekierowanie App2 identyfikatora URI:`x-msauth-mytestiosapp://com.myapp.mytestapp2`

App3 przekierowania URI:`x-msauth-mytestiosapp://com.myapp.mytestapp3`

....

Są one zagnieżdżone pod ten sam identyfikator klienta i identyfikator aplikacji i wyszukiwane oparte na Przekieruj zwrócić z nami w konfiguracji SDK identyfikatora URI.

```
+-------------------+
|                   |
|  Client ID        |
+---------+---------+
          |
          |           +-----------------------------------+
          |           |  App 1 Redirect URI               |
          +----------^+                                   |
          |           +-----------------------------------+
          |
          |           +-----------------------------------+
          +----------^+  App 2 Redirect URI               |
          |           |                                   |
          |           +-----------------------------------+
          |
          +----------^+-----------------------------------+
                      |  App 3 Redirect URI               |
                      |                                   |
                      +-----------------------------------+

```


*Należy zauważyć, że poniżej opisano format tych identyfikatorów przekierowywania. Można użyć identyfikatora URI przekierowywać chyba że chcesz obsługuje pośrednik, w tym przypadku ich musi wyglądać powyższego*



#### <a name="create-keychain-sharing-between-applications"></a>Tworzenie Pęk kluczy udostępnianie między aplikacjami

Włączanie udostępniania Pęk kluczy wykracza poza zakres tego dokumentu i objętych firmy Apple w jej dokumencie [Możliwości dodawania](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html). Co to jest ważne jest, możesz zdecydować, co ma być pęku kluczy i dodawać taką możliwość we wszystkich aplikacji.

W przypadku gdy dostępne poprawnie możesz skonfigurować uprawnień powinna być widoczna pliku w katalogu projektu prawo `entitlements.plist` zawiera coś, która wygląda następująco:

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>keychain-access-groups</key>
    <array>
        <string>$(AppIdentifierPrefix)com.myapp.mytestapp</string>
        <string>$(AppIdentifierPrefix)com.myapp.mycache</string>
    </array>
</dict>
</plist>
```

Po masz uprawnienie Pęk kluczy włączony w poszczególnych aplikacji i zechcesz za pomocą rejestracji Jednokrotnej, opisz Microsoft SDK tożsamości pęku kluczy za pomocą następujące ustawienia w swojej `ADAuthenticationSettings` z następującymi ustawieniami:

```
defaultKeychainSharingGroup=@"com.myapp.mycache";
```

> [AZURE.WARNING]
Po udostępnieniu Pęk kluczy w aplikacjach dowolnej aplikacji można usunąć użytkowników lub odpowiadającą usuwanie wszystkich tokenów wydawanych przez aplikację. To jest szczególnie katastrofalny, jeśli masz aplikacje korzystające z tokenów do pracy w tle. Udostępnianie pęku kluczy oznacza, że należy zwrócić szczególną uwagę na wszystkie usunąć operacji za pośrednictwem programu Microsoft SDK tożsamości.

To wszystko! Microsoft SDK tożsamości będzie teraz udostępnianie poświadczeń dla wszystkich aplikacji. Listy użytkowników również zostaną udostępnione w jego wystąpieniach aplikacji.

### <a name="turning-on-sso-for-broker-assisted-sso"></a>Włączanie rejestracji Jednokrotnej dla brokera pomocy logowania jednokrotnego

Możliwość aplikacja może używać dowolnego broker jest zainstalowana na tym urządzeniu jest **domyślnie wyłączona**. Aby użyć aplikacji brokera możesz wykonać kilka dodatkowych czynności konfiguracyjnych i dodawanie kodu do aplikacji.

Są Zrób tak:

1. Włączanie trybu broker w kodzie aplikacji połączenia do MS SDK.
2. Ustanów nowego przekierowania URI i zapewniają zarówno aplikacji, jak i rejestracji aplikacji.
3. Rejestrowanie schemat adresu URL.
4. iOS9 pomoc techniczna: Dodawanie uprawnień do pliku info.plist.


#### <a name="step-1-enable-broker-mode-in-your-application"></a>Krok 1: Włączenie trybu broker w aplikacji
Możliwość aplikacji używać brokera jest włączona, podczas tworzenia "kontekstu" lub początkowej konfiguracji obiektu uwierzytelniania. W tym przez ustawienie typu poświadczeń w kodzie:

```
/*! See the ADCredentialsType enumeration definition for details */
@propertyADCredentialsType credentialsType;
```
`AD_CREDENTIALS_AUTO` Ustawienie umożliwia Microsoft SDK tożsamości próby wyróżnienia do pośrednik, `AD_CREDENTIALS_EMBEDDED` uniemożliwi telefonicznej do brokera Microsoft SDK tożsamości.

#### <a name="step-2-registering-a-url-scheme"></a>Krok 2: Rejestrowanie schemat adresu URL
Platforma Microsoft Identity używa adresy URL, aby wywołania brokera, a następnie zwrócić sterowanie powrót do aplikacji. Aby zakończyć tej czasu Rundy konieczne schemat adresu URL zarejestrowane dla aplikacji platformy Microsoft Identity będą wiedzieć o. Może to być oprócz innych systemów aplikacji, który może ją wcześniej zarejestrować z aplikacją.

> [AZURE.WARNING]
Zalecamy nawiązywania schemat adresu URL dość unikatowe, aby zminimalizować prawdopodobieństwo innej aplikacji przy użyciu tego samego schematu adresu URL. Apple wymuszać unikatowość Schematy adresów URL, które są zarejestrowane w sklepie app store.

Poniżej przedstawiono przykładowy sposób wyświetlania w konfiguracji projektu. Można też wykonać to w XCode także:

```
<key>CFBundleURLTypes</key>
<array>
    <dict>
        <key>CFBundleTypeRole</key>
        <string>Editor</string>
        <key>CFBundleURLName</key>
        <string>com.myapp.mytestapp</string>
        <key>CFBundleURLSchemes</key>
        <array>
            <string>x-msauth-mytestiosapp</string>
        </array>
    </dict>
</array>
```

#### <a name="step-3-establish-a-new-redirect-uri-with-your-url-scheme"></a>Krok 3: Ustanowienia nowego przekierowania URI ze schematem adresu URL

W celu zapewnienia, firma Microsoft zawsze powrócić tokeny poświadczeń do prawidłowego stosowania, trzeba upewnij się, że możemy oddzwonić do aplikacji w taki sposób, aby sprawdzić system operacyjny iOS. System operacyjny iOS raportów dla aplikacji Microsoft brokera identyfikator pakietu aplikacji do nawiązywania połączeń z jego. To nie może być fałszywy przez aplikację rozpoczęcie. W związku z tym możemy wykorzystać to wraz z identyfikator URI naszych aplikacji brokera, aby upewnić się, że tokeny są zwracane do prawidłowego stosowania. Firma Microsoft wymaga ustanowienia tego przekierowania Unikatowy identyfikator URI obu w aplikacji i Ustaw jako Przekieruj identyfikatora URI w portalu Deweloper.

Usługi przekierowania URI muszą być w formie pisane z wielkiej litery:

`<app-scheme>://<your.bundle.id>`

ex: *x-msauth-mytestiosapp://com.myapp.mytestapp*

Ten identyfikator URI przekierowywać musi określonych w rejestracji aplikacji za pomocą [portal Azure klasyczny](https://manage.windowsazure.com/). Aby uzyskać więcej informacji o rejestracji aplikacji Azure AD zobacz [Integracja z usługą Azure Active Directory](./develop/active-directory-how-to-integrate.md).


##### <a name="step-3a-add-a-redirect-uri-in-your-app-and-dev-portal-to-support-certificate-based-authentication"></a>Krok 3a: Dodawanie przekierowania URI w portalu usługi aplikacji i deweloperów do obsługi uwierzytelniania na podstawie certyfikatu

Do obsługi certyfikatu uwierzytelniania opartego na które drugiego "msauth" muszą być zarejestrowane w [portal Azure klasycznej](https://manage.windowsazure.com/) i aplikacji do obsługi uwierzytelniania certyfikatu, jeśli chcesz dodać obsługujące w aplikacji.

`msauth://code/<broker-redirect-uri-in-url-encoded-form>`

ex: *msauth://code/x-msauth-mytestiosapp%3A%2F%2Fcom.myapp.mytestapp*


#### <a name="step-4-ios9-add-a-configuration-parameter-to-your-app"></a>Krok 4: iOS9: Dodawanie parametru konfiguracji do aplikacji

ADAL używa — canOpenURL: Aby sprawdzić, czy brokera jest zainstalowana na tym urządzeniu. W systemie iOS 9 Apple zablokowany co można wyszukiwać schematy aplikację. Należy dodać "msauth" do sekcji LSApplicationQueriesSchemes z `info.plist file`.

<key>LSApplicationQueriesSchemes</key>
<array>
     <string>msauth</string>
</array>

### <a name="youve-configured-sso"></a>Skonfigurowano logowania jednokrotnego!

Teraz Microsoft SDK tożsamości zostanie automatycznie zarówno udostępnianie poświadczeń dla aplikacji i wywołania brokera ma na jego urządzeniu.
