<properties
    pageTitle="Azure AD języka Java wprowadzenie | Microsoft Azure"
    description="Jak utworzyć aplikację wiersza polecenia języka Java, która znaki użytkowników w, aby uzyskać dostęp do interfejsu API."
    services="active-directory"
    documentationCenter="java"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
  ms.tgt_pltfrm="na"
    ms.devlang="java"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="brandwe"/>


# <a name="using-java-command-line-app-to-access-an-api-with-azure-ad"></a>Dostęp do interfejsu API z Azure AD przy użyciu aplikacji wiersza polecenia języka Java

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Azure AD ułatwia niezwykle proste do zewnętrznej obsługi sposób zarządzania tożsamością aplikacji sieci web, dostarczając pojedynczego logowania i wyrejestrowywania z tylko kilku wierszy kodu.  W aplikacjach sieci web języka Java można to zrobić przy użyciu implementacji ADAL4J sterowanych społeczności firmy Microsoft.

  W tym miejscu użyjemy ADAL4J do:
- Zaloguj się użytkownika do aplikacji, używając Azure AD jako dostawcy tożsamości.
- Wyświetlanie niektóre informacje o użytkowniku.
- Zaloguj się użytkownik się z aplikacji.

Aby to zrobić, musisz:

1. Zarejestrowanie aplikacji z usługą Azure Active Directory
2. Konfigurowanie aplikacji, aby użyć biblioteki ADAL4J.
3. Wydać żądania logowania i wyrejestrowywania Azure AD przy użyciu biblioteki ADAL4J.
4. Wydrukować dane dotyczące użytkownika.

Aby rozpocząć korzystanie, [Pobierz szkielet aplikacji](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect/archive/skeleton.zip) lub [Pobierz ukończonej przykładowej](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect\/archive/complete.zip).  Konieczne będzie również dzierżawy Azure AD, w której chcesz zarejestrować aplikację.  Jeśli nie masz już konto, [Dowiedz się, jak kupić](active-directory-howto-tenant.md).

## <a name="1--register-an-application-with-azure-ad"></a>1. rejestrowania aplikacji z usługą Azure Active Directory
Aby włączyć aplikację do uwierzytelniania użytkowników, musisz najpierw zarejestrować nowej aplikacji w dzierżawie.

- Zaloguj się do portalu zarządzania Azure.
- W nawigacji po lewej kliknij **Usługi Active Directory**.
- Wybierz pozycję dzierżawy, w którym chcesz zarejestrować tę aplikację.
- Kliknij kartę **aplikacje** , a następnie kliknij przycisk Dodaj w szufladzie dołu.
- Postępuj zgodnie z monitami i tworzenie nowej **aplikacji sieci Web i/lub WebAPI**.
    - **Nazwa** aplikacji opisując aplikacji dla użytkowników końcowych
    - Podstawowy adres URL aplikacji jest używany adres **URL logowania jednokrotnego** .  Domyślnie szkielet `http://localhost:8080/adal4jsample/`.
    - **Aplikacja identyfikator URI** jest unikatowym identyfikatorem aplikacji.  Konwencji jest użycie `https://<tenant-domain>/<app-name>`, np.`http://localhost:8080/adal4jsample/`
- Po zakończeniu rejestracji AAD przypisze aplikacji unikatowego identyfikatora klienckiego.  Musisz tę wartość w następnej sekcji, aby skopiować go na karcie Konfigurowanie.

Raz w portalu dla aplikacji tworzenie **Hasła aplikacji** dla aplikacji i skopiuj go w dół.  Konieczne będzie ją wkrótce.


## <a name="2-set-up-your-app-to-use-adal4j-library-and-prerequisites-using-maven"></a>2. skonfigurować aplikację do używania biblioteki ADAL4J i przy użyciu środowiska Maven wymagania wstępne
W tym miejscu będzie pracujemy skonfigurować ADAL4J do korzystania z protokołu uwierzytelniania OpenID nawiązywanie połączenia.  ADAL4J będzie używana do żądania logowania i wyrejestrowywania problemu, zarządzania sesją użytkownika i uzyskaj informacje o użytkowniku, między innymi.

-   W katalogu głównym projektu otwórz lub Utwórz `pom.xml` i Znajdź `// TODO: provide dependencies for Maven` i zamienić na następujące czynności:

```Java
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>public-client-adal4j-sample</artifactId>
    <packaging>jar</packaging>
    <version>0.0.1-SNAPSHOT</version>
    <name>public-client-adal4j-sample</name>
    <url>http://maven.apache.org</url>
    <properties>
        <spring.version>3.0.5.RELEASE</spring.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>adal4j</artifactId>
            <version>1.1.2</version>
        </dependency>
        <dependency>
            <groupId>com.nimbusds</groupId>
            <artifactId>oauth2-oidc-sdk</artifactId>
            <version>4.5</version>
        </dependency>
        <dependency>
            <groupId>org.json</groupId>
            <artifactId>json</artifactId>
            <version>20090211</version>
        </dependency>
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>3.0.1</version>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
            <version>1.7.5</version>
        </dependency>
</dependencies>
    <build>
        <finalName>public-client-adal4j-sample</finalName>
        <plugins>
                <plugin>
            <groupId>org.codehaus.mojo</groupId>
            <artifactId>exec-maven-plugin</artifactId>
            <version>1.2.1</version>
            <configuration>
                <mainClass>PublicClient</mainClass>
            </configuration>
        </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <configuration>
                    <source>1.7</source>
                    <target>1.7</target>
                    <encoding>UTF-8</encoding>
                </configuration>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-dependency-plugin</artifactId>
                <executions>
                    <execution>
                        <id>install</id>
                        <phase>install</phase>
                        <goals>
                            <goal>sources</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-resources-plugin</artifactId>
                <version>2.5</version>
                <configuration>
                    <encoding>UTF-8</encoding>
                </configuration>
            </plugin>
            <plugin>
        <artifactId>maven-assembly-plugin</artifactId>
        <executions>
          <execution>
            <phase>package</phase>
            <goals>
              <goal>single</goal>
            </goals>
          </execution>
        </executions>
        <configuration>
          <descriptorRefs>
            <descriptorRef>jar-with-dependencies</descriptorRef>
          </descriptorRefs>
        </configuration>
      </plugin>
      <plugin>
  <groupId>org.apache.maven.plugins</groupId>
  <artifactId>maven-assembly-plugin</artifactId>
  <configuration>
    <archive>
      <manifest>
        <mainClass>PublicClient</mainClass>
      </manifest>
    </archive>
  </configuration>
</plugin>
        </plugins>
    </build>

</project>


```



## <a name="3-create-the-java-publicclient-file"></a>3. Utwórz plik PublicClient języka java

Zgodnie z powyższym, użyjemy interfejsu API wykresu mają zostać pobrane dane o zalogowany użytkownik. W tym celu można łatwo nam możemy utworzyć plik reprezentować **Obiektu katalogu** i osobne pliki reprezentować **użytkownika** , tak aby struktury OO Java mogą być używane.

1. Tworzenie pliku o nazwie `DirectoryObject.java` której użyjemy do przechowywania podstawowe dane na temat dowolnego DirectoryObject (użytkownik może swobodnie do późniejszego użycia to dla innych kwerend wykresu, możesz to zrobić). Możesz można Wytnij i Wklej to poniżej:

```Java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.Future;

import javax.naming.ServiceUnavailableException;

import com.microsoft.aad.adal4j.AuthenticationContext;
import com.microsoft.aad.adal4j.AuthenticationResult;

public class PublicClient {

    private final static String AUTHORITY = "https://login.microsoftonline.com/common/";
    private final static String CLIENT_ID = "2a4da06c-ff07-410d-af8a-542a512f5092";

    public static void main(String args[]) throws Exception {

        try (BufferedReader br = new BufferedReader(new InputStreamReader(
                System.in))) {
            System.out.print("Enter username: ");
            String username = br.readLine();
            System.out.print("Enter password: ");
            String password = br.readLine();

            AuthenticationResult result = getAccessTokenFromUserCredentials(
                    username, password);
            System.out.println("Access Token - " + result.getAccessToken());
            System.out.println("Refresh Token - " + result.getRefreshToken());
            System.out.println("ID Token - " + result.getIdToken());
        }
    }

    private static AuthenticationResult getAccessTokenFromUserCredentials(
            String username, String password) throws Exception {
        AuthenticationContext context = null;
        AuthenticationResult result = null;
        ExecutorService service = null;
        try {
            service = Executors.newFixedThreadPool(1);
            context = new AuthenticationContext(AUTHORITY, false, service);
            Future<AuthenticationResult> future = context.acquireToken(
                    "https://graph.windows.net", CLIENT_ID, username, password,
                    null);
            result = future.get();
        } finally {
            service.shutdown();
        }

        if (result == null) {
            throw new ServiceUnavailableException(
                    "authentication result was null");
        }
        return result;
    }
}


```


##<a name="compile-and-run-the-sample"></a>Skompilować i uruchomić próbki

Zmienianie ponownie się Twój katalog główny i uruchom następujące polecenie, aby utworzyć próbkę wystarczy umieścić współpraca przy użyciu `maven`. To użyje `pom.xml` plik zapisano dla zależności.

`$ mvn package`

Po wykonaniu `adal4jsample.war` plików w swojej `/targets` katalogu. Może być wdrażanie który w kontenerze usługi Tomcat i skorzystaj z adresu URL 

`http://localhost:8080/adal4jsample/`


> [AZURE.NOTE] 
Jest bardzo łatwa do wdrożenia też przy użyciu najnowszych serwerów Tomcat. Po prostu przejdź do `http://localhost:8080/manager/` i postępuj zgodnie z instrukcjami na przekazywanie usługi "adal4jsample.war" pliku. Autodeploy go zostanie automatycznie z właściwego punktu końcowego.

##<a name="next-steps"></a>Następne kroki

Gratulacje! Teraz masz pracy aplikacji Java, która ma możliwość bezpiecznego uwierzytelniania użytkowników, połączeń API sieci Web przy użyciu OAuth 2.0 i uzyskanie podstawowych informacji o użytkowniku.  Jeśli jeszcze tego nie zrobiono, należy teraz można wypełnić dzierżawy usługi z niektórych użytkowników.

Odwołanie ukończony przykładowy (bez wartości konfiguracji) [ma postać zip w tym miejscu](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect/archive/complete.zip), ale można klonowanie go z GitHub:

```git clone --branch complete https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect.git```

