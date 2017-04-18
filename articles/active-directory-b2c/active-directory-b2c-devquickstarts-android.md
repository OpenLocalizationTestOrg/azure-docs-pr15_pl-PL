<properties
    pageTitle="Azure Active Directory B2C: Połączenie interfejsu API sieci web z systemem Android aplikacji | Microsoft Azure"
    description="W tym artykule opisano, jak utworzyć Android "listy zadań do wykonania" aplikację, która wymaga Node.js interfejsu API sieci web przy użyciu tokenów okaziciela OAuth 2.0. Zarówno Android aplikacji, jak i interfejsu API sieci web umożliwia Azure Active Directory B2C uwierzytelniania użytkowników i zarządzanie nimi tożsamości użytkownika."
    services="active-directory-b2c"
    documentationCenter="android"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="07/22/2016"
    ms.author="brandwe"/>

# <a name="azure-ad-b2c-call-a-web-api-from-an-android-application"></a>Azure AD B2C: Połączenie interfejsu API sieci web z systemem Android aplikacji

> [AZURE.WARNING] Ten samouczek wymaga niektóre ważne aktualizacje, w szczególności, aby usunąć stosowania ADAL Android B2C.  Firma Microsoft będą publikować świeży instrukcje dotyczące używania Azure AD B2C w aplikacje w następnym tygodniu, a zalecamy przytrzymując do tego czasu.  Ale jeśli chcesz wypróbować rozwiązań się, zachęcamy do kontynuować artykuł poniżej.



Za pomocą B2C usługi Azure Active Directory (Azure AD), można dodać funkcje zarządzania zaawansowanych tożsamości Samoobsługowe aplikacje i interfejsów API sieci web w kilku krokach krótki. W tym artykule opisano, jak utworzyć Android "listy zadań do wykonania" aplikację, która wymaga Node.js interfejsu API sieci web przy użyciu tokenów okaziciela OAuth 2.0. Zarówno Android aplikacji, jak i interfejsu API sieci web umożliwia Azure AD B2C uwierzytelniania użytkowników i zarządzanie nimi tożsamości użytkownika.

Ten szybki start wymaga wykupienia u operatora sieci web interfejs API, który jest chroniony Azure AD przy użyciu B2C do pracy w pełni. Firma Microsoft zaprojektowany jedną dla .NET i Node.js do użycia. Ten samouczek przyjęto, że próbki Node.js interfejsu API sieci web jest konfigurowany. Aby uzyskać więcej informacji zobacz [Azure AD B2C interfejsu API samouczek Node.js sieci web](active-directory-b2c-devquickstarts-api-node.md).

W przypadku Android klientów, którzy potrzebują dostępu do chronionych zasobów Azure AD udostępnia biblioteki uwierzytelniania Active Directory (ADAL). Wyłącznie w celu ADAL jest ułatwiają aplikacji uzyskanie tokeny dostępu. Aby udowodnić, jak łatwo jest, w tym przewodniku będzie budowy aplikacji listy zadań do wykonania Android który:

- Otrzymuje dostęp tokeny połączenie listy zadań do wykonania interfejsu API przy użyciu [protokołu uwierzytelniania OAuth 2.0](https://msdn.microsoft.com/library/azure/dn645545.aspx).
- Otrzymuje listy zadań do wykonania użytkowników.
- Znaki użytkowników.

> [AZURE.NOTE] W tym artykule nie omówiono jak wdrażać logowania, zapisywania i zarządzanie profilami przy użyciu Azure AD B2C. Poświęcony wywoływanie interfejsów API sieci web po uwierzytelnieniu użytkownika. Jeśli jeszcze tego nie zrobiono, możesz powinna rozpoczynać się od [.NET web app Samouczek wprowadzający](active-directory-b2c-devquickstarts-web-dotnet.md) poznać podstawowe zagadnienia dotyczące Azure AD B2C.

## <a name="get-an-azure-ad-b2c-directory"></a>Pobieranie katalogu Azure AD B2C

Zanim będzie można używać Azure AD B2C, należy utworzyć katalogu lub dzierżawa. Katalog jest kontenerem dla wszystkich użytkowników, aplikacje, grupy i. Jeśli nie masz już, [Utwórz katalog B2C](active-directory-b2c-get-started.md) przed Przejdź w tym przewodniku.

## <a name="create-an-application"></a>Tworzenie aplikacji

Następnie należy utworzyć aplikację w katalogu B2C. Dzięki temu Azure AD informacji wymaganych do bezpiecznego komunikowania się z aplikacji. Aplikacja i interfejsu API sieci web są reprezentowane przez pojedynczy **Identyfikator aplikacji** w tym przypadku ponieważ one obejmować jedną aplikację logiczne. Aby utworzyć aplikację, wykonaj [następujące czynności](active-directory-b2c-app-registration.md). Pamiętaj, aby:

- Dołączanie dla **aplikacji sieci web**/**interfejsu API sieci web** w aplikacji.
- Wprowadź `urn:ietf:wg:oauth:2.0:oob` jako **adres URL odpowiedź**. Jest domyślny adres URL, aby ten przykład kodu.
- Tworzenie **hasła aplikacji** dla aplikacji i skopiuj je. Konieczne będzie ją później. Należy zauważyć, że wartość ta musi być [wyjściowym XML](https://www.w3.org/TR/2006/REC-xml11-20060816/#dt-escape) , zanim użyjesz go.
- Skopiuj **Identyfikator aplikacji** jest przypisana do aplikacji. Musisz także to później.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Tworzenie zasad

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

W Azure AD B2C co środowisko użytkownika jest zdefiniowany przez [zasady](active-directory-b2c-reference-policies.md). Ta aplikacja zawiera trzy środowiska tożsamości: Utwórz konto, zaloguj się i zaloguj się przy użyciu Facebook.  Należy utworzyć jedną zasadę każdego typu, zgodnie z opisem w [artykule referencyjnym zasad](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy). Po utworzeniu trzy zasad, należy:

- Wybierz **nazwę wyświetlaną** i inne atrybuty zapisów w zapisów zasad.
- Wybierz **nazwę wyświetlaną** i **Identyfikator obiektu** oświadczeń aplikacji w każdej zasady. Możesz także inne roszczenia.
- Skopiuj **nazwy** każdej zasady po jego utworzeniu. Prefiks powinien mieć `b2c_1_`.  Te nazwy zasady będzie potrzebna później.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Po utworzeniu tych trzech zasad, możesz już przystąpić do tworzenia aplikacji.

Należy zauważyć, że w tym artykule nie omówiono sposobu użycia zasad właśnie utworzony. Aby dowiedzieć się, jak działają zasady w Azure AD B2C, zacznij [.NET web app Wprowadzenie — samouczek](active-directory-b2c-devquickstarts-web-dotnet.md).

## <a name="download-the-code"></a>Pobieranie kodu

Kod tego samouczka [jest przechowywana na GitHub](https://github.com/AzureADQuickStarts/B2C-NativeClient-Android). Aby utworzyć próbki w trakcie pracy, możesz [pobrać szkielet projektu jako pliku zip](https://github.com/AzureADQuickStarts/B2C-NativeClient-Android/archive/skeleton.zip). Można również klonowanie szkielet:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-NativeClient-Android.git
```

> [AZURE.NOTE] **Należy pobrać szkielet do użycia tego samouczka.** Ze względu na złożoność stosowania pełni funkcjonalny aplikacji Android szkielet zawiera kod interfejsu użytkownika, który będzie działać po ukończeniu tego samouczka. Jest to miara zaoszczędzić czas dla deweloperów. Kod interfejsu użytkownika jest germane z tematem Dodawanie B2C Android aplikacji.

Ukończony aplikacji jest również [dostępne w pliku zip](https://github.com/AzureADQuickStarts/B2C-NativeClient-Android/archive/complete.zip) lub na `complete` gałąź tym samym repozytorium.

Aby utworzyć ze środowiska Maven, możesz użyć `pom.xml` na najwyższym poziomie.

  1. Postępuj zgodnie z instrukcjami w [sekcji wymagania wstępne dotyczące konfigurowania środowiska Maven dla systemu Android](https://github.com/MSOpenTech/azure-activedirectory-library-for-android/wiki/Setting-up-maven-environment-for-Android).
  2. Konfigurowanie emulatora z SDK 21.
  3. Przejdź do folderu głównego, w którym sklonowany repo.
  4. Uruchom polecenie `mvn clean install`.
  5. Zmienianie katalogu próby Szybki Start `cd samples\hello`.
  6. Uruchom polecenie `mvn android:deploy android:run`.

Uruchamianie aplikacji powinna być widoczna. Wprowadź poświadczenia użytkownika test to wypróbować.

Pakiety języka Java archiwum (SŁOIK) zostaną przesłane również obok pakietu archiwum Android (AAR).

## <a name="download-the-android-adal-and-add-it-to-your-android-studio-workspace"></a>Pobierz Android ADAL i dodać je do obszaru roboczego Android Studio

Dostępne są opcje jak używać tej biblioteki w projekcie Android:

* Kod źródłowy umożliwia importowanie biblioteki Zaćmienie i łączy do aplikacji.
* Jeśli korzystasz z systemem Android Studio, można użyć formatu pakietu AAR i odwołanie plików binarnych.

### <a name="option-1-binaries-via-gradle-recommended"></a>Opcja 1: Plików binarnych za pośrednictwem Gradle (zalecane)

Możesz wyświetlić plików binarnych z repo centralnej środowiska Maven. Pakiet AAR mogą być zawarte w projekcie w Android Studio (na przykład w `build.gradle`) w ten sposób:

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
    compile('com.microsoft.aad:adal:2.0.1-alpha') {
        exclude group: 'com.android.support'
    } // Recent version is 2.0.1-alpha
}
```

### <a name="option-2-aar-via-maven"></a>Opcja 2: AAR za pośrednictwem środowiska Maven

Jeśli korzystasz z `m2e` dodatek w Zaćmienie, możesz określić zależności w swojej `pom.xml` pliku:

```xml
<dependency>
    <groupId>com.microsoft.aad</groupId>
    <artifactId>adal</artifactId>
    <version>2.0.1-alpha</version>
    <type>aar</type>
</dependency>
```

### <a name="option-3-source-via-git-last-resort"></a>Opcja 3: Źródło za pośrednictwem cyfra (ostatnia możliwości)

Aby uzyskać kod źródłowy SDK za pośrednictwem cyfra, wprowadź:

    git clone git@github.com:AzureAD/azure-activedirectory-library-for-android.git
    cd ./azure-activedirectory-library-for-android/src

Użyj oddziału **zbliżania.**

## <a name="set-up-your-configuration-file"></a>Konfigurowanie pliku konfiguracji

Używanie konfiguracji, który skonfigurowano wcześniej w portalu B2C, aby skonfigurować Android projektu.

Otwórz `helpes/Constants.java` i wypełnij wartości dla następujących czynności:

```

package com.microsoft.aad.taskapplication.helpers;

import com.microsoft.aad.adal.AuthenticationResult;

public class Constants {

    public static final String SDK_VERSION = "1.0";
    public static final String UTF8_ENCODING = "UTF-8";
    public static final String HEADER_AUTHORIZATION = "Authorization";
    public static final String HEADER_AUTHORIZATION_VALUE_PREFIX = "Bearer ";

    // -------------------------------AAD
    // PARAMETERS----------------------------------
    public static String AUTHORITY_URL = "https://login.microsoftonline.com/hypercubeb2c.onmicrosoft.com/";
    public static String CLIENT_ID = "<your application id>";
    public static String[] SCOPES = {"<your application id>"};
    public static String[] ADDITIONAL_SCOPES = {""};
    public static String REDIRECT_URL = "<redirect uri>";
    public static String CORRELATION_ID = "";
    public static String USER_HINT = "";
    public static String EXTRA_QP = "";
    public static String FB_POLICY = "B2C_1_<your policy>";
    public static String EMAIL_SIGNIN_POLICY = "B2C_1_<your policy>";
    public static String EMAIL_SIGNUP_POLICY = "B2C_1_<your policy>";
    public static boolean FULL_SCREEN = true;
    public static AuthenticationResult CURRENT_RESULT = null;
    // Endpoint we are targeting for the deployed WebAPI service
    public static String SERVICE_URL = "http://localhost:3000/tasks";

    // ------------------------------------------------------------------------------------------

    static final String TABLE_WORKITEM = "WorkItem";
    public static final String SHARED_PREFERENCE_NAME = "com.example.com.test.settings";


}


```
- `SCOPES`: Znaki zakresów są przekazywane do serwera, który chcesz zażądać z serwera, gdy użytkownik w. W polu Podgląd B2C należy przekazać `client_id`. Jednak oczekuje się zmienić na `read scopes` w przyszłości. Ten dokument zostanie zaktualizowana, gdy występuje.
- `ADDITIONAL_SCOPES`: Zakresy dodatkowe, których chcesz użyć dla aplikacji. Powinny być używane w przyszłości.
- `CLIENT_ID`: Identyfikator aplikacji, które masz w portalu.
- `REDIRECT_URL`: Przekieruj miejsce, w którym można się spodziewać token, która zostanie umieszczona Wstecz.
- `EXTRA_QP`: Żadnych dodatkowych chcesz przekazać na serwerze w formacie zakodowany adres URL.
- `FB_POLICY`: Zasady, które jest. Jest to część najważniejszych dla ten samouczek.
- `EMAIL_SIGNIN_POLICY`: Zasady, które jest. Jest to część najważniejszych dla ten samouczek.
- `EMAIL_SIGNUP_POLICY`: Zasady, które jest. Jest to część najważniejszych dla ten samouczek.

## <a name="add-references-to-android-adal-to-your-project"></a>Dodawanie odwołania do Android ADAL do projektu

> [AZURE.NOTE]  ADAL dla systemu Android używają modelu opartego na przeznaczenie wywołać uwierzytelniania. Opcje "Określanie" aplikacji do pracy. W tym przykładzie całego, a wszystkie ADAL dla systemu Android, centra na temat przekazywania informacji między nimi i zarządzanie nimi opcje.

Najpierw Opisz Android układu aplikacji, w tym opcje, których chcesz używać. Te opcje będą omówione szczegółowo w dalszej części tego samouczka.

Aktualizowanie projektu `AndroidManifest.xml` plik, aby uwzględnić wszystkie opcje usługi:

```
   <?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.microsoft.aad.taskapplication"
    android:versionCode="1"
    android:versionName="1.0" >

    <uses-sdk
        android:minSdkVersion="11"
        android:targetSdkVersion="19" />

    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />

    <application
        android:allowBackup="true"
        android:icon="@drawable/ic_launcher"
        android:label="@string/app_name"
        android:theme="@style/AppTheme" >
        <activity
            android:name="com.microsoft.aad.adal.AuthenticationActivity"
            android:configChanges="orientation|keyboardHidden|screenSize"
            android:exported="true"
            android:label="@string/title_login_hello_app" >
        </activity>
        <activity
            android:name="com.microsoft.aad.taskapplication.LoginActivity"
            android:configChanges="orientation|keyboardHidden|screenSize"
            android:label="@string/app_name" >
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
        <activity
            android:name="com.microsoft.aad.taskapplication.UsersListActivity"
            android:label="@string/title_activity_feed" >
        </activity>
        <activity
            android:name="com.microsoft.aad.taskapplication.SettingsActivity"
            android:label="@string/title_activity_settings" >
        </activity>
        <activity
            android:name="com.microsoft.aad.taskapplication.AddTaskActivity"
            android:label="@string/title_activity_settings" >
        </activity>
        <activity
            android:name="com.microsoft.aad.taskapplication.ToDoActivity"
            android:label="@string/app_name" >
        </activity>
    </application>

</manifest>    
```

Jak widać, należy zdefiniować pięć działań. Wszystkie te będą używane.

- `AuthenticationActivity`: Pochodzi z ADAL i zapewnia widoku logowania w sieci web.
- `LoginActivity`Wyświetla: zasady logowania i przyciski dla każdej z zasad.
- `SettingsActivity`: Umożliwia zmienianie ustawień aplikacji w czasie rzeczywistym.
- `AddTaskActivity`: Umożliwia dodawanie zadań do swojego interfejsu API usługi REST, które są chronione za pomocą Azure AD.
- `ToDoActivity`: To działanie głównym, które są wyświetlane zadania.

## <a name="create-the-sign-in-activity"></a>Utwórz działanie logowania

Tworzenie głównej działalności i nadaj jej `LoginActivity`.

Tworzenie pliku o nazwie `LoginActivity.java`.

Należy zainicjować działania i dodaj kilka przycisków, której nastąpi interfejsu użytkownika. Jest to znane, jeśli napisaniu Android kod przed.

```
import android.app.Activity;
import android.app.ProgressDialog;
import android.content.Intent;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.Toast;

import com.microsoft.aad.adal.AuthenticationContext;
import com.microsoft.aad.taskapplication.helpers.Constants;
import com.microsoft.aad.taskapplication.helpers.WorkItemAdapter;

/**
 * Created by brwerner on 9/17/15.
 */
public class LoginActivity extends Activity {

    private final static String TAG = "ToDoActivity";

    private AuthenticationContext mAuthContext;

    /**
     * Adapter to sync the items list with the view
     */
    private WorkItemAdapter mAdapter = null;

    /**
     * Show this dialog when activity first launches to check if user has login
     * or not.
     */
    private ProgressDialog mLoginProgressDialog;

    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_signin);
        Toast.makeText(getApplicationContext(), TAG + "LifeCycle: OnCreate", Toast.LENGTH_SHORT)
                .show();

        Button button = (Button) findViewById(R.id.signin_facebook);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(LoginActivity.this, ToDoActivity.class);
                intent.putExtra("thePolicy", Constants.FB_POLICY);
                startActivity(intent);

            }
        });

        button = (Button) findViewById(R.id.signin_email);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(LoginActivity.this, ToDoActivity.class);
                intent.putExtra("thePolicy", Constants.EMAIL_SIGNIN_POLICY);
                startActivity(intent);

            }
        });

        button = (Button) findViewById(R.id.signup_email);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(LoginActivity.this, ToDoActivity.class);
                intent.putExtra("thePolicy", Constants.EMAIL_SIGNUP_POLICY);
                startActivity(intent);

            }
        });

    }

}


```
Teraz utworzono przyciski, które połączenie z `ToDoActivity` przeznaczenie (który dzwoni ADAL przeszukanie tokenu). Są to zrobić przy użyciu swojej aktywności, a odwołanie dodatkowy parametr. Ten dodatkowy parametr zostanie przekazany `intent.putExtra()` metody. Do definiowania `"thePolicy"` przy użyciu określonego w `Constants.java`. To informuje zamiarem zasady, które do wywołania podczas uwierzytelniania.

## <a name="create-the-settings-activity"></a>Utwórz działanie ustawień

To działanie, które wypełnia ustawień interfejsu użytkownika.

Tworzenie pliku o nazwie `SettingsActivity.java` dla prostych tworzenie, przeczytaj, aktualizowanie i usuwanie operacji (OBSŁUGIWAŁ).

```
 package com.microsoft.aad.taskapplication;

import android.app.Activity;
import android.content.SharedPreferences;
import android.content.SharedPreferences.Editor;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.CheckBox;
import android.widget.Switch;
import android.widget.TextView;

import com.microsoft.aad.taskapplication.helpers.Constants;

/**
 * Settings page to try broker by options
 */
public class SettingsActivity extends Activity {

    //private CheckBox checkboxAskBroker, checkboxCheckBroker;
    private Switch fullScreenSwitch;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_settings);

        loadSettings();
//      checkboxAskBroker = (CheckBox) findViewById(R.id.askInstall);
//      checkboxCheckBroker = (CheckBox) findViewById(R.id.useBroker);

        Button save = (Button) findViewById(R.id.settingsSave);

        save.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                TextView textView = (TextView)findViewById(R.id.authority);
                Constants.AUTHORITY_URL = textView.getText().toString();

                textView = (TextView)findViewById(R.id.resource);
            //    Constants.SCOPES = textView.getText().toString();

                textView = (TextView)findViewById(R.id.clientId);
                Constants.CLIENT_ID = textView.getText().toString();

                textView = (TextView)findViewById(R.id.extraQueryParameters);
                Constants.EXTRA_QP = textView.getText().toString();

                textView = (TextView)findViewById(R.id.redirectUri);
                Constants.REDIRECT_URL = textView.getText().toString();

                textView = (TextView)findViewById(R.id.serviceUrl);
                Constants.SERVICE_URL = textView.getText().toString();

                textView = (TextView)findViewById(R.id.serviceUrl);
                textView.setText(Constants.SERVICE_URL);

                textView = (TextView)findViewById(R.id.fb_signin);
                textView.setText(Constants.FB_POLICY);

                textView = (TextView)findViewById(R.id.email_signin);
                textView.setText(Constants.EMAIL_SIGNIN_POLICY);

                textView = (TextView)findViewById(R.id.email_signup);
                textView.setText(Constants.EMAIL_SIGNUP_POLICY);

                textView = (TextView)findViewById(R.id.correlationId);
                textView.setText(Constants.CORRELATION_ID);

                fullScreenSwitch = (Switch)findViewById(R.id.fullScreen);
                Constants.FULL_SCREEN = fullScreenSwitch.isChecked();

                finish();
            }
        });


    }

    private void loadSettings() {
        TextView textView = (TextView)findViewById(R.id.authority);
        textView.setText(Constants.AUTHORITY_URL);
        textView = (TextView)findViewById(R.id.resource);
        textView.setText(Constants.SCOPES[0]);
        textView = (TextView)findViewById(R.id.clientId);
        textView.setText(Constants.CLIENT_ID);
        textView = (TextView)findViewById(R.id.extraQueryParameters);
        textView.setText(Constants.EXTRA_QP);
        textView = (TextView)findViewById(R.id.redirectUri);
        textView.setText(Constants.REDIRECT_URL);
        textView = (TextView)findViewById(R.id.serviceUrl);
        textView.setText(Constants.SERVICE_URL);
        textView = (TextView)findViewById(R.id.fb_signin);
        textView.setText(Constants.FB_POLICY);
        textView = (TextView)findViewById(R.id.email_signin);
        textView.setText(Constants.EMAIL_SIGNIN_POLICY);
        textView = (TextView)findViewById(R.id.email_signup);
        textView.setText(Constants.EMAIL_SIGNUP_POLICY);
        textView = (TextView)findViewById(R.id.correlationId);
        textView.setText(Constants.CORRELATION_ID);

        fullScreenSwitch = (Switch)findViewById(R.id.fullScreen);
        fullScreenSwitch.setChecked(Constants.FULL_SCREEN);
    }

    private void saveSettings(String key, boolean value) {
        SharedPreferences prefs = SettingsActivity.this.getSharedPreferences(
                Constants.SHARED_PREFERENCE_NAME, Activity.MODE_PRIVATE);
        Editor prefsEditor = prefs.edit();
        prefsEditor.putBoolean(key, value);
        prefsEditor.commit();
    }
}
```

## <a name="create-the-add-task-activity"></a>Utwórz działanie dodawanie zadań

To działanie umożliwia dodawanie zadania do punktu końcowego usługi interfejsu API usługi REST.

Tworzenie pliku o nazwie `AddTaskActivity.java` i wpisz następujące czynności.

```
package com.microsoft.aad.taskapplication;

import android.app.Activity;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;

import com.microsoft.aad.taskapplication.helpers.Constants;
import com.microsoft.aad.taskapplication.helpers.TodoListHttpService;

public class AddTaskActivity extends Activity {

    EditText textField;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_add_task);
        textField = (EditText) findViewById(R.id.taskToAdd);
        Button button = (Button) findViewById(R.id.postTaskbutton);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                if (textField.getText().toString() != null
                        && !textField.getText().toString().trim().isEmpty()
                        && Constants.CURRENT_RESULT != null) {

                    TodoListHttpService service = new TodoListHttpService();
                    try {
                        service.addItem(textField.getText().toString(), Constants.CURRENT_RESULT.getAccessToken());
                    } catch (Exception e) {
                        SimpleAlertDialog.showAlertDialog(getApplicationContext(), "Exception caught", e.getMessage());
                    }
                    finish();
                }
            }
        });
    }

}

```

## <a name="create-the-to-do-list-activity"></a>Tworzenie działania listy zadań do wykonania

To jest najbardziej istotne aktywności. Możesz stosować go, aby uzyskać tokenu z Azure AD przy użyciu zasad i połączeń serwera interfejsu API usługi REST zadań za pomocą token.

Tworzenie pliku o nazwie `ToDoActivity.java` i wpisz następujące czynności. (Połączenia opisano później.)

```

package com.microsoft.aad.taskapplication;

import android.app.Activity;
import android.app.AlertDialog;
import android.app.ProgressDialog;
import android.content.Intent;
import android.os.Build;
import android.os.Bundle;
import android.view.View;
import android.view.Window;
import android.webkit.CookieManager;
import android.webkit.CookieSyncManager;
import android.widget.ArrayAdapter;
import android.widget.Button;
import android.widget.ListView;
import android.widget.TextView;
import android.widget.Toast;

import com.microsoft.aad.adal.AuthenticationCallback;
import com.microsoft.aad.adal.AuthenticationContext;
import com.microsoft.aad.adal.AuthenticationResult;
import com.microsoft.aad.adal.AuthenticationSettings;
import com.microsoft.aad.adal.UserIdentifier;
import com.microsoft.aad.adal.PromptBehavior;
import com.microsoft.aad.taskapplication.helpers.Constants;
import com.microsoft.aad.taskapplication.helpers.InMemoryCacheStore;
import com.microsoft.aad.taskapplication.helpers.TodoListHttpService;
import com.microsoft.aad.taskapplication.helpers.Utils;
import com.microsoft.aad.taskapplication.helpers.WorkItemAdapter;


import java.io.UnsupportedEncodingException;
import java.net.MalformedURLException;
import java.net.URL;
import java.security.NoSuchAlgorithmException;
import java.security.spec.InvalidKeySpecException;
import java.util.ArrayList;
import java.util.List;
import java.util.UUID;

public class ToDoActivity extends Activity {



    private final static String TAG = "ToDoActivity";

    private AuthenticationContext mAuthContext;
    private static AuthenticationResult sResult;

    /**
     * Adapter to sync the items list with the view
     */
    private WorkItemAdapter mAdapter = null;

    /**
     * Show this dialog when activity first launches to check if user has login
     * or not.
     */
    private ProgressDialog mLoginProgressDialog;


    /**
     * Initializes the activity
     */
    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_list_todo_items);
        CookieSyncManager.createInstance(getApplicationContext());
        Toast.makeText(getApplicationContext(), TAG + "LifeCycle: OnCreate", Toast.LENGTH_SHORT)
                .show();

        // Clear previous sessions
        clearSessionCookie();
        try {
            // Provide key info for Encryption
            if (Build.VERSION.SDK_INT < 18) {
                Utils.setupKeyForSample();
            }

        Button button = (Button) findViewById(R.id.addTaskButton);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(ToDoActivity.this, AddTaskActivity.class);
                startActivity(intent);
            }
        });

        button = (Button) findViewById(R.id.appSettingsButton);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(ToDoActivity.this, SettingsActivity.class);
                startActivity(intent);

            }
        });

        button = (Button) findViewById(R.id.switchUserButton);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(ToDoActivity.this, LoginActivity.class);
                startActivity(intent);

            }
        });


        final TextView name = (TextView)findViewById(R.id.userLoggedIn);


        mLoginProgressDialog = new ProgressDialog(this);
        mLoginProgressDialog.requestWindowFeature(Window.FEATURE_NO_TITLE);
        mLoginProgressDialog.setMessage("Login in progress...");
        mLoginProgressDialog.show();
        // Ask for token and provide callback
        try {
            mAuthContext = new AuthenticationContext(ToDoActivity.this, Constants.AUTHORITY_URL,
                    false);
            String policy = getIntent().getStringExtra("thePolicy");

            if(Constants.CORRELATION_ID != null &&
                    Constants.CORRELATION_ID.trim().length() !=0){
                mAuthContext.setRequestCorrelationId(UUID.fromString(Constants.CORRELATION_ID));
            }

            AuthenticationSettings.INSTANCE.setSkipBroker(true);

            mAuthContext.acquireToken(ToDoActivity.this, Constants.SCOPES, Constants.ADDITIONAL_SCOPES, policy, Constants.CLIENT_ID,
                    Constants.REDIRECT_URL, getUserInfo(), PromptBehavior.Always,
                    "nux=1&" + Constants.EXTRA_QP,
                    new AuthenticationCallback<AuthenticationResult>() {

                        @Override
                        public void onError(Exception exc) {
                            if (mLoginProgressDialog.isShowing()) {
                                mLoginProgressDialog.dismiss();
                            }
                            SimpleAlertDialog.showAlertDialog(ToDoActivity.this,
                                    "Failed to get token", exc.getMessage());
                        }

                        @Override
                        public void onSuccess(AuthenticationResult result) {
                            if (mLoginProgressDialog.isShowing()) {
                                mLoginProgressDialog.dismiss();
                            }

                            if (result != null && !result.getToken().isEmpty()) {
                                setLocalToken(result);
                                updateLoggedInUser();
                                getTasks();
                                ToDoActivity.sResult = result;
                                Toast.makeText(getApplicationContext(), "Token is returned", Toast.LENGTH_SHORT)
                                        .show();

                                if (sResult.getUserInfo() != null) {
                                    name.setText(result.getUserInfo().getDisplayableId());
                                    Toast.makeText(getApplicationContext(),
                                            "User:" + sResult.getUserInfo().getDisplayableId(), Toast.LENGTH_SHORT)
                                            .show();
                                }
                            } else {
                                //TODO: popup error alert
                            }
                        }
                    });
        } catch (Exception e) {
            SimpleAlertDialog.showAlertDialog(ToDoActivity.this, "Exception caught", e.getMessage());
        }
        Toast.makeText(ToDoActivity.this, TAG + "done", Toast.LENGTH_SHORT).show();
    } catch (InvalidKeySpecException e) {
            e.printStackTrace();
        } catch (NoSuchAlgorithmException e) {
            e.printStackTrace();
        } catch (UnsupportedEncodingException e) {
            e.printStackTrace();
        }}

```


 Zwróć uwagę, że to korzysta z metod, które nie zostały jeszcze zapisane. Są to następujące opcje `updateLoggedInUser()`, `clearSessionCookie()`, i `getTasks()`. Będzie zapisać te w dalszej części tego przewodnika. Można zignorować błędy w Android Studio teraz.

Objaśnienie parametry:

  - `SCOPES`: Wymagane, próbujesz żądanie dostępu do zakresów. W polu Podgląd B2C jest taka sama, jak `client_id`, ale to może się zmienić w przyszłości.
  - `POLICY`: Zasady gdy chcesz do uwierzytelnienia użytkownika.
  - `CLIENT_ID`: Wymagane, pochodzi z portal Azure AD.
  - `redirectUri`: Można skonfigurować jako nazwy pakietu. Nie jest wymagane przewidziany do `acquireToken` połączeń.
  - `getUserInfo()`: Sposób wyszukująca czy użytkownik jest już w pamięci podręcznej. Parametr także opisano Monituj, jeśli nie można odnaleźć tego użytkownika lub tokenu dostępu jest nieprawidłowy. Ta metoda zostaną zapisane w dalszej części tego przewodnika.
  - `PromptBehavior.always`: Pomaga w celu wyświetlania monitu o poświadczenia, aby pominąć pamięci podręcznej i plików cookie.
  - `Callback`: Wywołane po kod autoryzacji jest wymieniona na tokenu. Będzie miał obiektu `AuthenticationResult`, który zawiera token dostępu, datę wygaśnięcia i identyfikator tokenu informacji.

> [AZURE.NOTE]  Aplikacja portalu firmy Microsoft Intune zawiera składnik brokera i może być zainstalowany na urządzeniu użytkownika. Aplikację udostępnia pojedynczej logowania jednokrotnego (SSO) we wszystkich aplikacjach na urządzeniu. Deweloperów być przygotowanym umożliwiające Intune. ADAL dla systemu Android będzie korzystać z konta brokera, jeśli istnieje jedno konto użytkownika utworzone w uwierzytelnienia. Aby użyć brokera, należy zarejestrować specjalnego projektanta `redirectUri` dla brokera korzystać. `redirectUri`jest w formacie msauth://packagename/Base64UrlencodedSignature. Możesz uzyskać `redirectUri` dla aplikacji za pomocą skryptu `brokerRedirectPrint.ps1` lub za pomocą połączenia interfejsu API `mContext.getBrokerRedirectUri()`. Podpis jest powiązana z własnych certyfikatów podpisywania ze sklepu Google Play.

 Możesz pominąć użytkownika broker za pomocą:

    ```java
     AuthenticationSettings.Instance.setSkipBroker(true);
    ```
> [AZURE.NOTE] W celu zmniejszenia złożoności ten szybki start B2C, możemy uczestniczą pominąć broker w naszym przykładzie.

Następnie należy utworzyć Pomocnik metod, których uzyskania tokenu tylko podczas uwierzytelniania połączeń z zadaniem interfejsu API.

W tym samym `ToDoActivity.java` plików, pisanie poniższe czynności.

```
    private void getToken(final AuthenticationCallback callback) {

        String policy = getIntent().getStringExtra("thePolicy");

        // one of the acquireToken overloads
        mAuthContext.acquireToken(ToDoActivity.this, Constants.SCOPES, Constants.ADDITIONAL_SCOPES,
                policy, Constants.CLIENT_ID, Constants.REDIRECT_URL, getUserInfo(),
                PromptBehavior.Always, "nux=1&" + Constants.EXTRA_QP, callback);
    }
```

Również dodać metod, które będą "kot" i "Pobierz" `AuthenticationResult` (który ma token) do szablonu globalnego `Constants`. Mimo że `ToDoActivity.java` korzysta z `sResult` w jego przepływów, musisz dodać te metody. W przeciwnym wypadku innych działań będzie można uzyskać dostępu do tokenu do pracy (takie jak dodawanie zadanie `AddTaskActivity.java`).

```

    private AuthenticationResult getLocalToken() {
        return Constants.CURRENT_RESULT;
    }

    private void setLocalToken(AuthenticationResult newToken) {


        Constants.CURRENT_RESULT = newToken;
    }


```
## <a name="create-a-method-to-return-a-user-identifier"></a>Tworzenie metody zwraca identyfikator użytkownika

ADAL dla systemu Android reprezentuje użytkownika w formie `UserIdentifier` obiektu. Zarządzania użytkownika. Określ, czy użytkownik jest używany w połączeń umożliwia obiektu. Za pomocą tych informacji, można korzystają z pamięci podręcznej, zamiast nowego połączenia z serwerem. Aby ułatwić, możemy utworzyć `getUserInfo()` metodę, która zwraca `UserIdentifier`. Za pomocą tego z `acquireToken()`. Także utworzony `getUniqueId()` metodę, która zwraca identyfikator `UserIdentifier` w pamięci podręcznej.

```
  private String getUniqueId() {
        if (sResult != null && sResult.getUserInfo() != null
                && sResult.getUserInfo().getUniqueId() != null) {
            return sResult.getUserInfo().getUniqueId();
        }

        return null;
    }

    private UserIdentifier getUserInfo() {

        final TextView names = (TextView)findViewById(R.id.userLoggedIn);
        String name = names.getText().toString();
        return new UserIdentifier(name, UserIdentifier.UserIdentifierType.OptionalDisplayableId);
    }

```

### <a name="write-helper-methods"></a>Pisanie metod Pomocnik

Następnie napisać niektóre metody Pomocnik ułatwiające wyczyść pliki cookie i podaj `AuthenticationCallback`. Te metody są używane wyłącznie dla próbki, aby upewnić się, że jesteś w czystym stanie podczas połączenia z `ToDo` aktywności.

W tym samym pliku o nazwie `ToDoActivity.java`, pisanie poniższe czynności.

```

    private void clearSessionCookie() {

        CookieManager cookieManager = CookieManager.getInstance();
        cookieManager.removeSessionCookie();
        CookieSyncManager.getInstance().sync();
    }
```

```
    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        mAuthContext.onActivityResult(requestCode, resultCode, data);
    }

```   

## <a name="call-the-task-api"></a>Połączenie zadań interfejsu API

Po umieszczeniu gotowe przyciągnąć tokeny aktywności, można napisać z interfejsu API do uzyskiwania dostępu do serwera zadań.

`getTasks`zawiera tablicę, która reprezentuje zadań na serwerze.

Rozpoczynanie z `getTasks`.

W tym samym pliku o nazwie `ToDoActivity.java`, pisanie poniższe czynności.

```
    private void getTasks() {
        if (sResult == null || sResult.getToken().isEmpty())
            return;

        List<String> items = new ArrayList<>();
        try {
            items = new TodoListHttpService().getAllItems(sResult.getToken());
        } catch (Exception e) {
            items = new ArrayList<>();
        }

        ListView listview = (ListView) findViewById(R.id.listViewToDo);
        ArrayAdapter<String> adapter = new ArrayAdapter<String>(this,
                android.R.layout.simple_list_item_1, android.R.id.text1, items);
        listview.setAdapter(adapter);
    }

```

Również napisać metodę, która zostanie zainicjowany tabel przy pierwszym uruchomieniu.

W tym samym pliku o nazwie `ToDoActivity.java`, pisanie poniższe czynności.

```
    private void initAppTables() {
        try {
            ListView listViewToDo = (ListView) findViewById(R.id.listViewToDo);
            listViewToDo.setAdapter(mAdapter);

        } catch (Exception e) {
            createAndShowDialog(new Exception(
                    "There was an error creating the Mobile Service. Verify the URL"), "Error");
        }
    }

```

Widać, że kod wymaga pewne dodatkowe metody do swojej pracy. Napisz tych dalej.

### <a name="create-an-endpoint-url-generator"></a>Tworzenie generator adres URL punktu końcowego

Należy wygenerować adres URL punktu końcowego, który będzie łączyć się z. To zrobić w tym samym pliku zajęć.

**W tym samym pliku** o nazwie `ToDoActivity.java`, pisanie poniższe czynności.

 ```
    private URL getEndpointUrl() {
        URL endpoint = null;
        try {
            endpoint = new URL(Constants.SERVICE_URL);
        } catch (MalformedURLException e) {
            e.printStackTrace();
        }
        return endpoint;
    }

 ```


Zwróć uwagę, możesz dodać token dostępu na żądanie w kodzie omówiony w następnej sekcji.

## <a name="write-some-ux-methods"></a>Pisanie niektóre metody interfejsu użytkownika

Android wymaga do obsługi niektórych zwrotne w celu korzystania z aplikacji. Są to `createAndShowDialog` i `onResume()`. Jest to znane, jeśli napisaniu Android kod przed.

W tym samym pliku o nazwie `ToDoActivity.java`, pisanie poniższe czynności.

```
    @Override
    public void onResume() {
        super.onResume(); // Always call the superclass method first

        updateLoggedInUser();
        // User can click logout, it will come back here
        // It should refresh list again
        getTasks();
    }


```

Następnie Zarządzaj usługi zwrotne okno dialogowe.

W tym samym pliku o nazwie `ToDoActivity.java`, pisanie poniższe czynności.

```
    /**
     * Creates a dialog and shows it
     *
     * @param exception The exception to show in the dialog
     * @param title     The dialog title
     */
    private void createAndShowDialog(Exception exception, String title) {
        createAndShowDialog(exception.toString(), title);
    }

    /**
     * Creates a dialog and shows it
     *
     * @param message The dialog message
     * @param title   The dialog title
     */
    private void createAndShowDialog(String message, String title) {
        AlertDialog.Builder builder = new AlertDialog.Builder(this);

        builder.setMessage(message);
        builder.setTitle(title);
        builder.create().show();
    }

```

Po wykonaniu `ToDoActivity.java` pliku, który powoduje kompilację. Cały projekt należy również skompilować na tym etapie.

## <a name="run-the-sample-app"></a>Uruchom aplikację próbki

Na koniec tworzenie i uruchamianie aplikacji Android Studio lub Zaćmienie. Konta lub zalogowaniu w aplikacji. Tworzenie zadań dla zalogowany użytkownik. Wyloguj się i zaloguj się ponownie jako inny użytkownik, a następnie utwórz zadań dla tego użytkownika.

Zauważyć, że zadania są przechowywane dla każdego użytkownika w interfejsie API, ponieważ API wyodrębnia tożsamość użytkownika z otrzymywanych token dostępu.

Odwołanie ukończonej przykładowej [znajduje się w pliku zip](https://github.com/AzureADQuickStarts/B2C-NativeClient-Android/archive/complete.zip). Można również klonowanie z GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/B2C-NativeClient-Android```


## <a name="important-information"></a>Ważne informacje


### <a name="encryption"></a>Szyfrowanie

ADAL są szyfrowane tokenów i przechowywać w `SharedPreferences` domyślnie. Możesz obejrzeć `StorageHelper` zajęć, aby wyświetlić szczegóły. Android wprowadzona **AndroidKeyStore dla 4.3(API18)** bezpieczne przechowywanie kluczy prywatnych. ADAL używa, który dla API18 i powyżej. Jeśli chcesz użyć ADAL dla dolnym wersji zestawu SDK, należy podać klucz tajny na `AuthenticationSettings.INSTANCE.setSecretKey`.

### <a name="session-cookies-in-web-view"></a>Pliki cookie sesji w widoku sieci web

Widok android sieci web nie czyści pliki cookie sesji po zamknięciu aplikacji. Problem można rozwiązać z następujący kod.
```
CookieSyncManager.createInstance(getApplicationContext());
CookieManager cookieManager = CookieManager.getInstance();
cookieManager.removeSessionCookie();
CookieSyncManager.getInstance().sync();
```
Aby [uzyskać więcej informacji dotyczących plików cookie](http://developer.android.com/reference/android/webkit/CookieSyncManager.html).
