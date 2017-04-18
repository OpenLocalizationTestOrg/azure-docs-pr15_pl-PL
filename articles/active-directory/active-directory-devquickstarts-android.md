<properties
    pageTitle="Azure AD systemu Android wprowadzenie | Microsoft Azure"
    description="Sposoby tworzenia Android aplikacji można zintegrować z usługą Azure Active Directory do zalogowania się i połączeń Azure AD chronionych za pomocą OAuth interfejsów API."
    services="active-directory"
    documentationCenter="android"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="brandwe"/>

# <a name="integrate-azure-ad-into-an-android-app"></a>Integrowanie Azure AD Android aplikacji

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)] 

Jeśli projektujesz aplikacji klasycznej Azure AD ułatwia niezwykle proste do uwierzytelnienia użytkownika przy użyciu swoich kont usługi Active Directory.  Umożliwia także aplikacji do bezpiecznego korzystania z dowolnego interfejsu API chroniony przez Azure AD, takich jak interfejsów API usługi Office 365 lub interfejsu API Azure w sieci web.

W przypadku Android klientów, którzy potrzebują dostępu do chronionych zasobów Azure AD udostępnia Active Directory Authentication Library lub ADAL.  Osoby ADAL wyłącznie w celu w życiu jest ułatwiają aplikacji uzyskanie tokeny dostępu.  Wykazać się, jak łatwo jest, w tym miejscu będzie budowy aplikacji Android listy zadań do wykonania która:

-   Pobiera dostęp tokeny do nawiązywania połączeń z interfejs API listy zadań do wykonania przy użyciu [protokołu uwierzytelniania OAuth 2.0](https://msdn.microsoft.com/library/azure/dn645545.aspx).
-   Otrzymuje listę zadań do wykonania przez użytkownika
-   Znaki użytkowników.

Aby rozpocząć pracę, musisz dzierżawę Azure AD, w którym można tworzyć użytkowników i rejestrowania aplikacji.  Jeśli nie masz jeszcze dzierżawy, [Dowiedz się, jak kupić](active-directory-howto-tenant.md).

> [AZURE.TIP] Wypróbuj Podgląd programu nasz nowy [dzięki portalowi deweloperów](https://identity.microsoft.com/Docs/Android) , która ułatwi rozpoczęcie pracy z usługą Azure Active Directory w ciągu kilku minut!  Dzięki portalowi deweloperów przeprowadzi Cię przez proces rejestracji aplikacji i integracji Azure AD do kodu.  Po zakończeniu, konieczne będzie prostej aplikacji, która może przeprowadzać uwierzytelnianie użytkowników w Twojej dzierżawy i wewnętrznej bazie danych, których można zaakceptować tokeny i sprawdzana poprawność. 

## <a name="step-1-download-and-run-the-nodejs-rest-api-todo-sample-server"></a>Krok 1: Pobrać i uruchomić na serwerze przykładowe Node.js pozostałych interfejsu API zadania

W tym przykładzie jest zapisywany w szczególności pracy przed próbie istniejących dotyczące tworzenia dzierżawy jednego interfejsu API usługi REST zadań do wykonania dla usługi Microsoft Azure Active Directory. To jest wstępnie wymagane dla Szybki Start.

Informacji na temat wybrać tę opcję można znaleźć w naszym istniejących próbki:

* [Usługi interfejsu API platformy Microsoft Azure Active Directory przykładowe pozostałych dla Node.js](active-directory-devquickstarts-webapi-nodejs.md)

## <a name="step-2-register-your-web-api-with-your-microsoft-azure-ad-tenant"></a>Krok 2: Rejestrowanie interfejsu API usługi sieci Web z dzierżawy usługi Microsoft Azure AD

**Co to jest wykonanie?**

*Microsoft Active Directory obsługuje dodawanie dwa typy aplikacji. Interfejsów API, które oferują usługi dla użytkowników i aplikacji (albo w sieci web lub uruchamiania aplikacji na urządzeniu), które uzyskiwać dostęp do tych interfejsów API sieci Web w sieci Web. W tym kroku rejestrujesz się interfejs API sieci Web używasz lokalnie testowania w tym przykładzie. Ten interfejs API sieci Web będzie zwykle usługi REST, który oferuje funkcje, które mają dostęp do aplikacji. Microsoft Azure Active Directory można chronić dowolny punkt końcowy!*

*W tym miejscu możemy są przy założeniu rejestrujesz się interfejsu API usługi REST zadania, do których odwołuje się powyżej, ale to sprawdza się we wszystkich API sieci Web możesz chcieć usługi Azure Active Directory w celu ochrony.*

Czynności, aby zarejestrować interfejs API sieci Web usługi Microsoft Azure AD

1. Zaloguj się do [portalu zarządzania Azure](https://manage.windowsazure.com).
2. Kliknij w usłudze Active Directory w funkcję lewej
3. Kliknij pozycję dzierżawy katalogu miejsce, w którym chcesz zarejestrować przykładowej aplikacji.
4. Kliknij kartę aplikacje.
5. W szufladzie kliknij przycisk Dodaj.
6. Kliknij przycisk "Dodaj aplikację, którą rozwija się w mojej organizacji".
7. Wprowadź przyjazną nazwę Wybierz aplikację, na przykład "TodoListService", "API sieci Web i/lub aplikacji sieci Web", a następnie kliknij przycisk Dalej.
8. Logowania jednokrotnego adresu URL, wprowadź podstawowy adres URL dla próbki, które domyślnie ma wartość `https://localhost:8080`.
9. Identyfikator URI identyfikator aplikacji, wprowadź `https://<your_tenant_name>/TodoListService`, zastąpienie `<your_tenant_name>` o nazwie dzierżawy usługi Azure AD.  Kliknij przycisk OK, aby zakończyć rejestracji.
10. Nadal w portalu Azure, kliknij kartę Konfigurowanie aplikacji.
11. **Znaleziona wartość Identyfikator klienta i skopiuj go należy zarezerwować**, konieczne będzie to później podczas konfigurowania aplikacji.

## <a name="step-3-register-the-sample-android-native-client-application"></a>Krok 3: Rejestrowanie przykładowej aplikacji Android Native Client

Rejestrowanie aplikacji sieci web jest pierwszym krokiem. Następnie musisz Opisz także aplikacji usługi Azure Active Directory. Dzięki aplikacji na komunikowanie się tylko zarejestrowanych interfejs API sieci Web

**Co to jest wykonanie?**  

*Jak już wspomniano Microsoft Azure Active Directory obsługuje dodawanie dwa typy aplikacji. Interfejsów API, które oferują usługi dla użytkowników i aplikacji (albo w sieci web lub uruchamiania aplikacji na urządzeniu), które uzyskiwać dostęp do tych interfejsów API sieci Web w sieci Web. W tym kroku rejestrujesz się aplikacji w tym przykładzie. Należy użyć w kolejności dla tej aplikacji mieć możliwość uzyskania interfejs API sieci Web po prostu zarejestrowane. Azure Active Directory będą odrzucać nawet umożliwia aplikacji w celu wyświetlania monitu o logowania, o ile nie jest zarejestrowany! To część zabezpieczenia modelu.*

*W tym miejscu możemy są przy założeniu rejestrujesz się ta aplikacja przykładowa wymienionych powyżej, ale to sprawdza się do dowolnej aplikacji, które tworzysz.*

**Dlaczego mam I umieszczenie zarówno aplikacji i interfejs API sieci Web w jednej dzierżawy?**

*Jak można może mieć odgadnąć, można utworzyć aplikację, która uzyskuje dostęp do zewnętrznego interfejsu API jest zarejestrowana w usłudze Azure Active Directory z innej dzierżawy. Jeśli do klientów wyświetli monit o zgodę na użycie interfejsu API w aplikacji. Część i jest, Active Directory Authentication Library dla systemu iOS trwa istotnych ten zgody dla Ciebie! Jak możemy uzyskać dostęp do bardziej zaawansowanych funkcji, zobaczysz, że jest to ważne część pracy, aby uzyskać dostęp do pakietu Microsoft APIs z Azure i pakietu Office, a także innych dostawców usług. Teraz ponieważ zarejestrowany obu interfejs API sieci Web i aplikacji w obszarze tej samej dzierżawy nie będą widoczne żadne monity o zgodę. To jest zazwyczaj są projektowanie aplikacji tylko do własnej firmy za pomocą.*

1. Zaloguj się do [portalu zarządzania Azure](https://manage.windowsazure.com).
2. Kliknij w usłudze Active Directory w funkcję lewej
3. Kliknij pozycję dzierżawy katalogu miejsce, w którym chcesz zarejestrować przykładowej aplikacji.
4. Kliknij kartę aplikacje.
5. W szufladzie kliknij przycisk Dodaj.
6. Kliknij przycisk "Dodaj aplikację, którą rozwija się w mojej organizacji".
7. Wpisz przyjazną nazwę aplikacji, na przykład "TodoListClient Android", wybierz opcję "natywnej aplikacji klienckiej" i kliknij przycisk Dalej.
8. Wprowadź URI przekierować `http://TodoListClient`.  Kliknij przycisk Zakończ.
9. Kliknij kartę Konfigurowanie aplikacji.
10. Znajdowanie wartości Identyfikator klienta i skopiuj go pomijając konieczne będzie to później podczas konfigurowania aplikacji.
11. "Uprawnienia do innych aplikacji" kliknij pozycję "Dodaj aplikacji".  Wybierz pozycję "Inne" na liście rozwijanej "Pokaż", a następnie kliknij górny znacznik wyboru.  Znajdź i wybierz polecenie TodoListService i kliknij znacznik wyboru dolnej, aby dodać aplikację.  Wybierz "Access TodoListService" z menu rozwijanego "Delegowane uprawnień", a następnie zapisać konfigurację.



Aby utworzyć ze środowiska Maven, możesz użyć pom.xml na najwyższym poziomie

  * Klonowanie ten repo w katalogu wyboru:

  `$ git clone git@github.com:AzureADSamples/NativeClient-Android.git`  

  * Postępuj zgodnie z instrukcjami w [sekcji Prerequests Konfigurowanie usługi środowiska maven dla systemu android](https://github.com/MSOpenTech/azure-activedirectory-library-for-android/wiki/Setting-up-maven-environment-for-Android)
  * Ustawienia emulatora z SDK 19
  * Przejdź do folderu głównego, w którym sklonowany repo
  * Uruchamianie polecenia: mvn czystej instalacji
  * Zmienianie katalogu próby Szybki Start: samples\hello dysk cd
  * Uruchamianie polecenia: mvn android: Wdrażanie systemu android: Uruchom
  * Powinien zostać wyświetlony uruchamianie aplikacji
  * Wprowadź poświadczenia użytkownika test wypróbować!

Pakiety słoik zostaną przesłane również obok pakietu aar.

### <a name="step-4-download-the-android-adal-and-add-it-to-your-eclipse-workspace"></a>Krok 4: Pobieranie Android ADAL i dodać je do obszaru roboczego Zaćmienie

Wprowadziliśmy go łatwo jest wiele opcji używać tej biblioteki w projekcie Android:

* Kod źródłowy umożliwia importowanie tej biblioteki do Zaćmienie i łączy do aplikacji.
* Jeśli z systemem Android Studio, można użyć formatu pakietu *aar* i odwołanie plików binarnych.

####<a name="option-1-source-zip"></a>Opcja 1: Źródło Zip

Aby pobrać kopię kodu źródłowego, kliknij przycisk "Pobierz ZIP" w prawej części strony lub kliknij [tutaj](https://github.com/AzureAD/azure-activedirectory-library-for-android/archive/v1.0.9.tar.gz).

####<a name="option-2-source-via-git"></a>Opcja 2: Źródła cyfra

Aby uzyskać po prostu wpisz kod źródłowy SDK za pośrednictwem cyfra:

    git clone git@github.com:AzureAD/azure-activedirectory-library-for-android.git
    cd ./azure-activedirectory-library-for-android/src

####<a name="option-3-binaries-via-gradle"></a>Opcja 3: Plików binarnych za pośrednictwem Gradle

Możesz wyświetlić plików binarnych z repo centralnej środowiska Maven. Pakiet AAR można następująco dołączone w projekcie AndroidStudio:

```gradle
repositories {
    mavenCentral()
    flatDir {
        dirs 'libs'
    }
    maven {
        url "YourLocalMavenRepoPath\\.m2\\repository"
    }
}
dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile('com.microsoft.aad:adal:1.1.1') {
        exclude group: 'com.android.support'
    } // Recent version is 1.1.1
}
```

####<a name="option-4-aar-via-maven"></a>Opcja 4: aar za pośrednictwem środowiska Maven

Jeśli korzystasz z wtyczki m2e w Zaćmienie, można określić zależności w pliku pom.xml:

```xml
<dependency>
    <groupId>com.microsoft.aad</groupId>
    <artifactId>adal</artifactId>
    <version>1.1.1</version>
    <type>aar</type>
</dependency>
```


####<a name="option-5-jar-package-inside-libs-folder"></a>Opcja 5: jar pakietu w folderze bibliotekami
Można pobrać pliku słoik ze środowiska maven repo i upuść do folderu *bibliotekami* w projekcie. Należy skopiować wymaganych zasobów do projektu także, ponieważ pakiety słoik nie zawierają je.


### <a name="step-5-add-references-to-android-adal-to-your-project"></a>Krok 5: Dodawanie odwołania do Android ADAL do projektu


2. Dodawanie odwołania do projektu i określić ją jako Android biblioteki. Jeśli masz pewności, jak to zrobić, [Kliknij tutaj, aby uzyskać więcej informacji] (http://developer.android.com/tools/projects/projects-eclipse.html)

3. Dodawanie współzależności projektu do debugowania w ustawień projektu

4. Aktualizowanie projektu AndroidManifest.xml plik do dołączenia:

    ```Java
      <uses-permission android:name="android.permission.INTERNET" />
      <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
      <application
            android:allowBackup="true"
            android:debuggable="true"
            android:icon="@drawable/ic_launcher"
            android:label="@string/app_name"
            android:theme="@style/AppTheme" >

            <activity
                android:name="com.microsoft.aad.adal.AuthenticationActivity"
                android:label="@string/title_login_hello_app" >
            </activity>
      ....
      <application/>
    ```

7. Tworzenie wystąpienie AuthenticationContext na głównym aktywności. Szczegółowe informacje o tym połączenia wykraczają poza zakres tego pliku Readme, ale można uzyskać dobre uruchamiania sprawdzając [Android próbce klienta](https://github.com/AzureADSamples/NativeClient-Android). Poniżej przedstawiono przykładowy:

    ```Java
    // Authority is in the form of https://login.windows.net/yourtenant.onmicrosoft.com
    mContext = new AuthenticationContext(MainActivity.this, authority, true); // This will use SharedPreferences as            default cache
    ```
  * Uwaga: mContext jest polem w swojej aktywności

8. Skopiuj ten blok kodu do obsługi na koniec AuthenticationActivity po wprowadzenie poświadczeń i otrzymuje kod autoryzacji:

    ```Java
     @Override
     protected void onActivityResult(int requestCode, int resultCode, Intent data) {
         super.onActivityResult(requestCode, resultCode, data);
         if (mContext != null) {
             mContext.onActivityResult(requestCode, resultCode, data);
         }
     }
    ```

9. Aby uzyskać tokenu, należy zdefiniować wywołanie zwrotne

    ```Java
    private AuthenticationCallback<AuthenticationResult> callback = new AuthenticationCallback<AuthenticationResult>() {

            @Override
            public void onError(Exception exc) {
                if (exc instanceof AuthenticationException) {
                    textViewStatus.setText("Cancelled");
                    Log.d(TAG, "Cancelled");
                } else {
                    textViewStatus.setText("Authentication error:" + exc.getMessage());
                    Log.d(TAG, "Authentication error:" + exc.getMessage());
                }
            }

            @Override
            public void onSuccess(AuthenticationResult result) {
                mResult = result;

                if (result == null || result.getAccessToken() == null
                        || result.getAccessToken().isEmpty()) {
                    textViewStatus.setText("Token is empty");
                    Log.d(TAG, "Token is empty");
                } else {
                    // request is successful
                    Log.d(TAG, "Status:" + result.getStatus() + " Expired:"
                            + result.getExpiresOn().toString());
                    textViewStatus.setText(PASSED);
                }
            }
        };
    ```
10. Na koniec poproś o tokenu za pomocą wywołanie zwrotne:

    ```Java
     mContext.acquireToken(MainActivity.this, resource, clientId, redirect, user_loginhint, PromptBehavior.Auto, "",
                    callback);
    ```

Objaśnienie parametry:

  * Zasób jest wymagane i zasób, do którego chcesz uzyskać dostęp.
  * ClientID jest wymagany i pochodzi z portalem AzureAD.
  * Możesz skonfigurować redirectUri jako swojego packagename. Nie jest wymagane dostarczanych do połączenia acquireToken.
  * PromptBehavior pomaga w celu wyświetlania monitu o poświadczenia, aby pominąć pamięci podręcznej i plików cookie.
  * Po kod autoryzacji jest wymieniona na tokenu zostanie wywołana zwrotnego.

  Wywołanie zwrotne ma obiektu AuthenticationResult, w której znajduje się accesstoken, datę wygasła i idtoken informacje.

Opcjonalnie: **acquireTokenSilent**

Możesz zadzwonić **acquireTokenSilent** obsługę pamięci podręcznej i Odśwież token. Umożliwia także wersji synchronizacji. Nazwa użytkownika jest przyjmuje jako parametr.

    ```java
     mContext.acquireTokenSilent(resource, clientid, userId, callback );
    ```

11. **Broker**: Aplikacja portalu firmy Microsoft Intune zapewni składnik broker. ADAL Użyj utworzone konta brokera, jeśli istnieje jednego konta użytkownika w tej uwierzytelnienia i deweloper wybierze nie chcesz pominąć. Deweloper można pominąć brokera użytkownikowi:

    ```java
     AuthenticationSettings.Instance.setSkipBroker(true);
    ```

 Deweloper musi zarejestrować specjalne redirectUri użycia broker. RedirectUri jest w formacie msauth://packagename/Base64UrlencodedSignature. Można uzyskać usługi redirecturi dla aplikacji za pomocą skryptu "brokerRedirectPrint.ps1" lub za pomocą interfejsu API mContext.getBrokerRedirectUri połączenia. Podpis jest powiązana z własnych certyfikatów podpisywania.

 Bieżący model broker jest dla jednego użytkownika. AuthenticationContext zapewnia metodę interfejsu API uzyskiwania użytkownika broker.

 ```java
 String brokerAccount =  mContext.getBrokerUser();
 ```
 Jeśli konto jest prawidłowe, zostanie zwrócony brokera użytkownika.

 Manifeście aplikacji ma uprawnienia do korzystania z konta AccountManager: http://developer.android.com/reference/android/accounts/AccountManager.html

 * GET_ACCOUNTS
 * USE_CREDENTIALS
 * MANAGE_ACCOUNTS


Za pomocą tego instruktażu, trzeba mieć co jest potrzebne do pomyślnie Integracja z usługi Azure Active Directory. Więcej przykładów tej pracy, odwiedź stronę AzureADSamples-repozytorium na GitHub.

## <a name="important-information"></a>Ważne informacje

### <a name="customization"></a>Dostosowywanie

Zasoby projektu biblioteki mogą być zastępowane zasobów aplikacji. Dzieje się tak, tworząc aplikację. Z tego powodu można dostosować układ działań tak jak chcesz. Potrzebujesz upewnić się zachować identyfikator kontrolek tego ADAL uses(Webview).

### <a name="broker"></a>Broker

Składnik brokera będą dostarczane z aplikacja portalu firmy Intune firmy Microsoft. W Menedżerze konto zostanie utworzone konto. W obszarze Typ konta jest "com.microsoft.workaccount". Umożliwia tylko jednego konta logowania jednokrotnego. Po zakończeniu wezwanie urządzenia dla jednej z aplikacji pakietu utworzy cookie rejestracji Jednokrotnej dla tego użytkownika.

### <a name="authority-url-and-adfs"></a>Adres Url Urząd i usług ADFS organizacji

ADFS nie jest rozpoznawany jako produkcji usługi STS, więc należy włączyć wystąpienie odnajdowania i przekazywanie FAŁSZ w Konstruktorze AuthenticationContext.

Adres url Urząd wymaga usługi STS wystąpienie oraz nazwę dzierżawy: https://login.windows.net/yourtenant.onmicrosoft.com

### <a name="querying-cache-items"></a>Kwerenda elementów pamięci podręcznej

ADAL zawiera pamięci podręcznej domyślne w SharedPreferences kilka prostych pamięci podręcznej funkcje kwerendy. Zostanie wyświetlony bieżący pamięci podręcznej z AuthenticationContext z:
```Java
 ITokenCacheStore cache = mContext.getCache();
```
Możesz także podać implementacji pamięci podręcznej, jeśli chcesz go dostosować.
```Java
mContext = new AuthenticationContext(MainActivity.this, authority, true, yourCache);
```

### <a name="promptbehavior"></a>PromptBehavior

ADAL dostępna opcja zachowania monitu zmiennoprzecinkową. PromptBehavior.Auto będzie wyskakujące interfejsu użytkownika, jeśli token odświeżania jest nieprawidłowy i wymagane są poświadczenia użytkownika. PromptBehavior.Always pominie użycie pamięci podręcznej i zawsze pokazuj interfejsu użytkownika.

### <a name="silent-token-request-from-cache-and-refresh"></a>Odbiorcze żądania tokenu z pamięci podręcznej i odświeżania

Ta metoda nie korzysta z interfejsu użytkownika pop w górę, a nie wymaga działanie. Może zwrócić token z pamięci podręcznej, jeśli jest dostępna. Jeśli wygasła token spróbuje odświeżenie go. Jeśli odświeżanie token wygasła lub nie powiodło się, zwróci AuthenticationException.

    ```Java
    Future<AuthenticationResult> result = mContext.acquireTokenSilent(resource, clientid, userId, callback );
    ```

Możesz również wykorzystać synchronizacji połączeń z tą metodą. Można ustawić wartości null do zwrotnego lub użyć acquireTokenSilentSync.

### <a name="diagnostics"></a>Narzędzia diagnostyczne

Poniżej przedstawiono podstawowego źródła informacji dotyczących diagnozowanie problemów:

+ Wyjątki
+ Dzienniki
+ Śledzenia sieci

Należy również zauważyć, że identyfikatory korelacji są podstawą diagnostyki w bibliotece. Możesz ustawić swojej korelacji identyfikatorów na zasadzie na żądanie, jeśli chcesz dostosować ADAL żądanie z innych operacji w kodzie. Jeśli nie ustawisz identyfikator korelacji, a następnie ADAL wygeneruje losowe jedną and wszystkich wiadomości dziennika i połączeń sieciowych będzie dołączana z identyfikatorem korelacji. Samodzielnie generowane zmiany identyfikatora dla każdego żądania.

#### <a name="exceptions"></a>Wyjątki

Oczywiście jest pierwszym diagnostyczne. Firma Microsoft próby zapewniają pomocne komunikaty. Jeśli znajdziesz co nie jest przydatne Zgłoś problem i trafić. Podaj informacje urządzenie, takie jak model i SDK # również.

#### <a name="logs"></a>Dzienniki

Można konfigurować biblioteki w celu wygenerowania wiadomości dziennika, które umożliwiają diagnozowanie problemów. Rejestrowanie można skonfigurować, wykonując następujące połączenie, aby skonfigurować wywołanie zwrotne, który ADAL będzie używany do strony poza każdej wiadomości dziennika jako jest generowany.


 ```Java
 Logger.getInstance().setExternalLogger(new ILogger() {
     @Override
     public void Log(String tag, String message, String additionalMessage, LogLevel level, ADALError errorCode) {
      ...
      // You can write this to logfile depending on level or errorcode.
      writeToLogFile(getApplicationContext(), tag +":" + message + "-" + additionalMessage);
     }
 }
 ```
Wiadomości można zapisywane w pliku dziennika niestandardowego, jak pokazano poniżej. Niestety nie istnieje standardowy sposób uzyskiwania dzienników z urządzenia. Istnieje kilka usług, które mogą pomóc w ten. Możesz można również magazynowa własny, taki jak wysłanie pliku na serwerze.

```Java
private syncronized void writeToLogFile(Context ctx, String msg) {
       File directory = ctx.getDir(ctx.getPackageName(), Context.MODE_PRIVATE);
       File logFile = new File(directory, "logfile");
       FileOutputStream outputStream = new FileOutputStream(logFile, true);
       OutputStreamWriter osw = new OutputStreamWriter(outputStream);
       osw.write(msg);
       osw.flush();
       osw.close();
}
```

##### <a name="logging-levels"></a>Poziomy rejestrowania

+ Error(Exceptions)
+ Warn(Warning)
+ Info (informacje o potrzeb)
+ Pełne (więcej szczegółów)

Należy ustawić poziom rejestrowania w następujący sposób:
```Java
Logger.getInstance().setLogLevel(Logger.LogLevel.Verbose);
 ```

 Wszystkie wiadomości dziennika są wysyłane do logcat oprócz dowolnego zwrotne dziennika niestandardowego.
Zostanie wyświetlony dziennik logcat formularza pliku jako wyświetlana belog:

 ```
  adb logcat > "C:\logmsg\logfile.txt"
 ```
 Więcej przykładów o adb cmds: https://developer.android.com/tools/debugging/debugging-log.html#startingLogcat

#### <a name="network-traces"></a>Śledzenia sieci

Za pomocą różnych narzędzi do przechwytywania ruchu HTTP generowany przez ADAL.  Jest to najbardziej użyteczne, jeżeli masz doświadczenie z protokołem OAuth lub jeśli musisz podać informacje diagnostyczne do firmy Microsoft ani innych kanałów pomocy technicznej.

Fiddler jest najprostszym narzędzia do śledzenia protokołu HTTP.  Użyj następujących łączy skonfigurować ją do poprawnie rekordu ADAL ruch sieciowy.  Aby być przydatne należy skonfigurować fiddler lub innego narzędzia, takie jak Charlesa, aby zarejestrować niezaszyfrowanym ruch SSL.  Uwaga: Śledzenia wygenerowane w ten sposób może zawierać wysoce uprzywilejowanych informacje, takie jak tokeny dostępu, nazwy użytkowników i hasła.  Jeśli korzystasz z konta produkcyjnej, nie są udostępniane ścieżki śledzenia strony 3.  Jeśli musisz podać śledzenia osobie, aby można było uzyskiwać pomoc techniczna prowadzą do wystąpienia problemu z kontem tymczasowych przy użyciu nazwy użytkownika i hasła, które nie przeszkadzają udostępniania.

+ [Aby skonfigurować Fiddler dla systemu Android](http://docs.telerik.com/fiddler/configure-fiddler/tasks/ConfigureForAndroid)
+ [Konfigurowanie reguł Fiddler dla ADAL](https://github.com/AzureAD/azure-activedirectory-library-for-android/wiki/How-to-listen-to-httpUrlConnection-in-Android-app-from-Fiddler)


### <a name="dialog-mode"></a>Okno dialogowe Tryb
Metoda acquireToken bez aktywności obsługuje okno dialogowe monit.

### <a name="encryption"></a>Szyfrowanie

ADAL są szyfrowane tokenów i przechowywać w SharedPreferences domyślnie. Zapoznanie się z klasą StorageHelper, aby wyświetlić szczegóły. Android wprowadzona AndroidKeyStore 4.3(API18) bezpiecznego magazynu kluczy prywatnych. ADAL używa, który dla API18 i powyżej. Jeśli chcesz użyć ADAL w dolnym wersjach SDK, należy podać klucz tajny na AuthenticationSettings.INSTANCE.setSecretKey

### <a name="oauth2-bearer-challenge"></a>Wyzwania okaziciela uwierzytelnianie Oauth2

Klasy AuthenticationParameters zapewnia funkcje uzyskiwania authorization_uri z uwierzytelnianie Oauth2 okaziciela wezwanie.

### <a name="session-cookies-in-webview"></a>Pliki cookie sesji w widoku sieci Web

Android widok sieci Web nie czyści pliki cookie sesji po zamknięciu aplikacji. Problem można rozwiązać z kodu przykładowego poniżej:
```java
CookieSyncManager.createInstance(getApplicationContext());
CookieManager cookieManager = CookieManager.getInstance();
cookieManager.removeSessionCookie();
CookieSyncManager.getInstance().sync();
```
Więcej informacji na temat plików cookie: http://developer.android.com/reference/android/webkit/CookieSyncManager.html

### <a name="resource-overrides"></a>Zastępowanie zasobów

Biblioteka ADAL zawiera angielskich ciągów dla następujących komunikatów ProgressDialog.

Aplikacja powinna je zastąpić życzenie zlokalizowanych ciągów.

```Java
<string name="app_loading">Loading...</string>
<string name="broker_processing">Broker is processing</string>
<string name="http_auth_dialog_username">Username</string>
<string name="http_auth_dialog_password">Password</string>
<string name="http_auth_dialog_title">Sign In</string>
<string name="http_auth_dialog_login">Login</string>
<string name="http_auth_dialog_cancel">Cancel</string>
```

=======

### <a name="ntlm-dialog"></a>Okno dialogowe NTLM
Wersja adal 1.1.0 obsługuje okno dialogowe NTLM, który jest przetwarzana przez zdarzenie onReceivedHttpAuthRequest z WebViewClient. Można dostosować układ okna dialogowego i ciągi znaków.

### <a name="cross-app-sso"></a>Aplikacja krzyżowe logowania jednokrotnego
Dowiedz się, [jak włączyć logowania jednokrotnego krzyżowe aplikacji w systemie Android przy użyciu ADAL](active-directory-sso-android.md)  


[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
