<properties
    pageTitle="Logowanie przerzucania klucza Azure AD | Microsoft Azure"
    description="W tym artykule omówiono najważniejsze wskazówki przerzucania klucza podpisywania dla usługi Azure Active Directory"
    services="active-directory"
    documentationCenter=".net"
    authors="gsacavdm"
    manager="krassk"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/18/2016"
    ms.author="gsacavdm"/>

# <a name="signing-key-rollover-in-azure-active-directory"></a>Podpisywanie przerzucania klucza w usłudze Active Directory platformy Azure

W tym temacie omówiono, co należy wiedzieć o klucze publiczne, używanych w usłudze Azure Active Directory (Azure AD) do podpisania tokenów zabezpieczających. Należy pamiętać, że te najazd klawisze w regularnych odstępach czasu i w takiej sytuacji, może obniżyć natychmiast. Wszystkie aplikacje, które używają Azure AD powinno być możliwe programowo obsługiwać ten proces przerzucania klucza lub ustanowienia procesu okresowych najazd ręcznego. Wrócić do czytania, aby dowiedzieć się, jak działają klawisze sposobu oceny wpływu najazd do aplikacji i zaktualizować aplikacji lub ustanowienia procesu okresowe najazd ręcznego obsługę przerzucania klucza, jeśli to konieczne.

## <a name="overview-of-signing-keys-in-azure-ad"></a>Omówienie: logowanie klawiszy Azure AD

Azure AD używa szyfrowania kluczy publicznych oparty na standardami ustanawiania zaufania między sobą i aplikacje, które używają go. W praktyce działa to w następujący sposób: Azure AD przy użyciu klucza podpisywania, składający się z pary kluczy publicznych i prywatnych. Gdy użytkownik zaloguje się do aplikacji, która używa Azure AD dla uwierzytelniania, Azure AD tworzy tokenu zabezpieczającego, która zawiera informacje o użytkowniku. Ten token został podpisany przez Azure AD przy użyciu kluczem prywatnym przed wysłaniem powrót do aplikacji. Aby sprawdzić, czy token jest prawidłowy i faktycznie udzielonych z usługi Azure Active Directory, aplikacja musi zostać sprawdzony podpisu tokenu przy użyciu klucza publicznego ujawnionego przez Azure AD znajdującej się w dzierżawy [Łączenie OpenID odnajdowania](http://openid.net/specs/openid-connect-discovery-1_0.html) lub SAML-WS-Fed [Federacji metadanych dokumentu](active-directory-federation-metadata.md).

Ze względów bezpieczeństwa Azure AD podpisywania klucza zwojach w regularnych odstępach czasu, a w przypadku awaryjnego może obniżyć od razu. Dowolną aplikację, którą można zintegrować z usługą Azure Active Directory należy przygotować się do obsługi zdarzeń przerzucania klucza, niezależnie od tego, jak często mogą wystąpić. Jeśli nie, a aplikacja próbuje zweryfikować podpis token przy użyciu klucza wygasłe, żądanie logowania nie powiedzie się.

Istnieje więcej niż jeden prawidłowego klucza dostępne w dokumencie odnajdywania łączenie OpenID i dokument metadanych federacji. Aplikacja powinna być w stanie za pomocą klawiszy określony w dokumencie, ponieważ jeden klawisz może być szybko wdrażania, innej może być jej duplikat i tak dalej.

## <a name="how-to-assess-if-your-application-will-be-affected-and-what-to-do-about-it"></a>Jak w celu ustalenia, czy wpłynie aplikacji i jak go

Jak aplikacja obsługuje przerzucania klucza w zależności od czynników, takich jak typ aplikacji lub użyto jakie protokół tożsamości i biblioteki. Poniższe sekcje ocenić, czy najbardziej typowych aplikacji ma wpływ przerzucania klucza oraz wskazówki dotyczące aktualizowania aplikacji do obsługi najazd automatyczne lub ręczne aktualizowanie klucz.

* [Uzyskiwanie dostępu do zasobów aplikacji klientami](#nativeclient)
* [Aplikacje internetowe / interfejsy API uzyskiwania dostępu do zasobów](#webclient)
* [Aplikacje internetowe / interfejsy API chroni zasoby i utworzony przy użyciu usługi aplikacji Azure](#appservices)
* [Aplikacje internetowe / interfejsów API ochrony zasobów za pomocą .NET OWIN OpenID połączyć, WS Fed lub WindowsAzureActiveDirectoryBearerAuthentication pośredniczącym](#owin)
* [Aplikacje internetowe / interfejsów API ochrony zasobów za pomocą połączenia programu .NET Core OpenID lub JwtBearerAuthentication pośredniczącym](#owincore)
* [Aplikacje internetowe / interfejsów API ochrony zasobów za pomocą modułu passport azure ad Node.js](#passport)
* [Aplikacje internetowe / interfejsy API chroni zasoby i utworzone za pomocą programu Visual Studio 2015 r.](#vs2015)
* [Aplikacje sieci Web ochrona zasobów i utworzonych w programie Visual Studio 2013](#vs2013)
* [Interfejsy API sieci Web ochrona zasobów i utworzonych w programie Visual Studio 2013](#vs2013_webapi)
* [Aplikacje sieci Web ochrona zasobów i utworzone za pomocą programu Visual Studio 2012](#vs2012)
* [Aplikacje sieci Web ochrona zasobów i utworzone za pomocą programu Visual Studio 2010, 2008 o użyciu programu Windows Foundation tożsamości](#vs2010)
* [Aplikacje internetowe / interfejsy API chroni zasoby przy użyciu innych bibliotek lub ręcznie implementacją obsługiwane protokoły](#other)

Te wskazówki jest **nie** dotyczy:

* Aplikacje dodane Azure AD aplikacji galerii (w tym niestandardowe) mają oddzielne wytyczne dotyczące podpisywania. [Więcej informacji.](active-directory-sso-certs.md)
* Lokalne aplikacji publikowanych za pośrednictwem serwera proxy aplikacji nie musisz się martwić podpisywania.

### <a name="nativeclient"></a>Uzyskiwanie dostępu do zasobów aplikacji klientami

Aplikacje, które są tylko dostęp do zasobów (znaczy Microsoft Graph, KeyVault, interfejs API programu Outlook i innych APIs Microsoft) zazwyczaj tylko uzyskać tokenu i przekazać go wzdłuż do właściciela zasobów. Zakładając, że są one nie chroni wszystkie zasoby, nie inspekcja tokenu i dlatego nie ma potrzeby upewnij się, że jest prawidłowo podpisany.

Programy klientami stacjonarnym lub przenośnym, do tej kategorii, a więc nie dotyczy najazd.

### <a name="webclient"></a>Aplikacje internetowe / interfejsy API uzyskiwania dostępu do zasobów

Aplikacje, które są tylko dostęp do zasobów (znaczy Program Microsoft Graph, KeyVault, interfejs API programu Outlook i innych APIs Microsoft) zazwyczaj tylko uzyskać tokenu i przekazać go wzdłuż do właściciela zasobu. Zakładając, że są one nie chroni wszystkie zasoby, nie inspekcja tokenu i dlatego nie ma potrzeby upewnij się, że jest prawidłowo podpisany.

Aplikacji i sieci web interfejsów API, które używają przepływu tylko do aplikacji (poświadczeń klienta-certyfikat klienta), do tej kategorii i dlatego nie dotyczy najazd.

### <a name="appservices"></a>Aplikacje internetowe / interfejsy API chroni zasoby i utworzony przy użyciu usługi aplikacji Azure

Uwierzytelniania Azure usług aplikacji / funkcji autoryzacji (EasyAuth) ma już potrzeby logiki do obsługi przerzucania klucza automatycznie.

### <a name="owin"></a>Aplikacje internetowe / interfejsów API ochrony zasobów za pomocą .NET OWIN OpenID połączyć, WS Fed lub WindowsAzureActiveDirectoryBearerAuthentication pośredniczącym

Jeśli aplikacja korzysta z .NET OWIN OpenID połączyć, WS Fed lub pośredniczącym WindowsAzureActiveDirectoryBearerAuthentication ma już potrzeby logiki do obsługi przerzucania klucza automatycznie.

Możesz potwierdzić, że aplikacja korzysta dowolną z następujących, szukając dowolną z następujących wstawki Startup.cs lub Startup.Auth.cs w aplikacji

```
app.UseOpenIdConnectAuthentication(
     new OpenIdConnectAuthenticationOptions
     {
         // ...
     });
```
```
app.UseWsFederationAuthentication(
    new WsFederationAuthenticationOptions
    {
     // ...
    });
```
```
 app.UseWindowsAzureActiveDirectoryBearerAuthentication(
     new WindowsAzureActiveDirectoryBearerAuthenticationOptions
     {
     // ...
     });
```

### <a name="owincore"></a>Aplikacje internetowe / interfejsów API ochrony zasobów za pomocą pośredniczącym .NET Core OpenID połączenia lub JwtBearerAuthentication

Jeśli aplikacja korzysta pośredniczącym .NET Core OWIN OpenID połączenia lub JwtBearerAuthentication, ma już potrzeby logiki do obsługi przerzucania klucza automatycznie.

Możesz potwierdzić, że aplikacja korzysta dowolną z następujących, szukając dowolną z następujących wstawki Startup.cs lub Startup.Auth.cs w aplikacji

```
app.UseOpenIdConnectAuthentication(
     new OpenIdConnectAuthenticationOptions
     {
         // ...
     });
```
```
app.UseJwtBearerAuthentication(
    new JwtBearerAuthenticationOptions
    {
     // ...
    });
```

### <a name="passport"></a>Aplikacje internetowe / interfejsów API ochrony zasobów za pomocą modułu passport azure ad Node.js

Jeśli aplikacja używa modułu passport ad Node.js, ma już potrzeby logiki do obsługi przerzucania klucza automatycznie.

Potwierdzić, że usługi aplikacji passport ad, wyszukując poniższy fragment w app.js aplikacji

```
var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

passport.use(new OIDCStrategy({
    //...
));
```

### <a name="vs2015"></a>Aplikacje internetowe / interfejsy API chroni zasoby i utworzone za pomocą programu Visual Studio 2015 r.

Zaznaczone **Konta służbowego i Praca** z menu **Zmień uwierzytelniania** aplikacji został utworzony przy użyciu szablonu aplikacji sieci web w Visual Studio 2015, ma już potrzeby logiki do obsługi przerzucania klucza automatycznie. Tę logikę osadzony w pośredniczącym łączenie OpenID OWIN pobiera buforowanie kluczy z dokumentu odnajdowania łączenie OpenID i okresowo odświeża je.

Jeśli ręcznie dodane uwierzytelniania do rozwiązania, aplikacja może nie ma logiczny niezbędne przerzucania klucza. Konieczne będzie pisanie siebie i postępuj zgodnie z instrukcjami [aplikacji / interfejsy API przy użyciu innych bibliotek lub ręcznie implementacją protokołów.](#other).

### <a name="vs2013"></a>Aplikacje sieci Web ochrona zasobów i utworzonych w programie Visual Studio 2013

Jeśli zaznaczono **Konta organizacji** z menu **Zmień uwierzytelniania** aplikacji został utworzony przy użyciu szablonu aplikacji sieci web w Visual Studio 2013, już potrzeby logiki do obsługi przerzucania klucza automatycznie. Tę logikę przechowuje unikatowy identyfikator organizacji i podpisywania najważniejsze informacje w dwóch tabelach bazy danych skojarzonego z projektem. Parametry połączenia dla bazy danych można znaleźć w pliku Web.config projektu.

Jeśli ręcznie dodane uwierzytelniania do rozwiązania, aplikacja może nie ma logiki niezbędne przerzucania klucza. Konieczne będzie pisanie siebie i postępuj zgodnie z instrukcjami [aplikacji / interfejsy API przy użyciu innych bibliotek lub ręcznie implementacją protokołów.](#other).

Poniższe czynności pomoże Ci Sprawdź, czy wartość działa poprawnie w aplikacji.

1. W Visual Studio 2013 i otwórz rozwiązanie, a następnie kliknij na karcie **Server Explorer** w prawym okienku.
2. Rozwiń **połączeń danych**, **DefaultConnection**i **tabel**. Zlokalizować tabelę, **IssuingAuthorityKeys** , kliknij go prawym przyciskiem myszy, a następnie kliknij **Pokaż dane w tabeli**.
3. W tabeli **IssuingAuthorityKeys** będzie co najmniej jeden wiersz, który odpowiada wartość klucza. Usuń wszystkie wiersze w tabeli.
4. Kliknij prawym przyciskiem myszy tabelę **dzierżaw** , a następnie kliknij **Pokaż dane w tabeli**.
5. W tabeli **dzierżaw** będzie co najmniej jeden wiersz, który odpowiada identyfikator dzierżawy unikatowy katalog. Usuń wszystkie wiersze w tabeli. Jeśli nie możesz usunąć wiersze w tabeli **dzierżaw** i tabeli **IssuingAuthorityKeys** , otrzymasz komunikat o błędzie w czasie rzeczywistym.
6. Tworzenie i uruchamianie aplikacji. Gdy użytkownik jest zalogowany do konta, możesz wyłączyć aplikację.
7. Wróć do **Server Explorer** i obejrzeć wartości w tabeli **IssuingAuthorityKeys** i **dzierżaw** . Można zauważyć, że zostały one automatycznie zapełnienia odpowiednie informacje z dokumentu metadanych federacji.

### <a name="vs2013"></a>Interfejsy API sieci Web ochrona zasobów i utworzonych w programie Visual Studio 2013

Jeśli został utworzony interfejs API aplikacji sieci web programu Visual Studio 2013, przy użyciu szablonu interfejsu API sieci Web i zaznaczona **Konta organizacji** z menu **Zmień uwierzytelniania** , masz już potrzeby logiki w aplikacji.

Jeśli skonfigurowano ręcznie uwierzytelniania, postępuj zgodnie z instrukcjami poniżej, aby dowiedzieć się, jak skonfigurować interfejs API usługi sieci Web, aby automatycznie zaktualizować swoje informacje o kluczu.

Poniższy fragment kodu przedstawia sposób uzyskać najnowszą klucze z dokumentu metadanych Federacji, a następnie użyj [Programu obsługi tokenów JWT](https://msdn.microsoft.com/library/dn205065.aspx) do sprawdzania poprawności tokenu. Wstawkę kodu założono, że będzie używany własny mechanizm buforowania dla utrzymuje klucz do sprawdzania poprawności przyszłych tokenów z Azure AD, czy jest w bazie danych, plik konfiguracji lub innej lokalizacji.

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.IdentityModel.Tokens;
using System.Configuration;
using System.Security.Cryptography.X509Certificates;
using System.Xml;
using System.IdentityModel.Metadata;
using System.ServiceModel.Security;
using System.Threading;

namespace JWTValidation
{
    public class JWTValidator
    {
        private string MetadataAddress = "[Your Federation Metadata document address goes here]";

        // Validates the JWT Token that's part of the Authorization header in an HTTP request.
        public void ValidateJwtToken(string token)
        {
            JwtSecurityTokenHandler tokenHandler = new JwtSecurityTokenHandler()
            {
                // Do not disable for production code
                CertificateValidator = X509CertificateValidator.None
            };

            TokenValidationParameters validationParams = new TokenValidationParameters()
            {
                AllowedAudience = "[Your App ID URI goes here, as registered in the Azure Classic Portal]",
                ValidIssuer = "[The issuer for the token goes here, such as https://sts.windows.net/68b98905-130e-4d7c-b6e1-a158a9ed8449/]",
                SigningTokens = GetSigningCertificates(MetadataAddress)

                // Cache the signing tokens by your desired mechanism
            };

            Thread.CurrentPrincipal = tokenHandler.ValidateToken(token, validationParams);
        }

        // Returns a list of certificates from the specified metadata document.
        public List<X509SecurityToken> GetSigningCertificates(string metadataAddress)
        {
            List<X509SecurityToken> tokens = new List<X509SecurityToken>();

            if (metadataAddress == null)
            {
                throw new ArgumentNullException(metadataAddress);
            }

            using (XmlReader metadataReader = XmlReader.Create(metadataAddress))
            {
                MetadataSerializer serializer = new MetadataSerializer()
                {
                    // Do not disable for production code
                    CertificateValidationMode = X509CertificateValidationMode.None
                };

                EntityDescriptor metadata = serializer.ReadMetadata(metadataReader) as EntityDescriptor;

                if (metadata != null)
                {
                    SecurityTokenServiceDescriptor stsd = metadata.RoleDescriptors.OfType<SecurityTokenServiceDescriptor>().First();

                    if (stsd != null)
                    {
                        IEnumerable<X509RawDataKeyIdentifierClause> x509DataClauses = stsd.Keys.Where(key => key.KeyInfo != null && (key.Use == KeyType.Signing || key.Use == KeyType.Unspecified)).
                                                             Select(key => key.KeyInfo.OfType<X509RawDataKeyIdentifierClause>().First());

                        tokens.AddRange(x509DataClauses.Select(token => new X509SecurityToken(new X509Certificate2(token.GetX509RawData()))));
                    }
                    else
                    {
                        throw new InvalidOperationException("There is no RoleDescriptor of type SecurityTokenServiceType in the metadata");
                    }
                }
                else
                {
                    throw new Exception("Invalid Federation Metadata document");
                }
            }
            return tokens;
        }
    }
}
```

### <a name="vs2012"></a>Aplikacje sieci Web ochrona zasobów i utworzone za pomocą programu Visual Studio 2012

Jeśli aplikacja została wbudowana Visual Studio 2012, prawdopodobnie używany tożsamości i Access narzędzie do konfigurowania aplikacji. Jest również prawdopodobieństwo, że używasz [Sprawdzania poprawności wystawcy nazwa rejestru (VINR)](https://msdn.microsoft.com/library/dn205067.aspx). VINR jest odpowiedzialny za zachowanie informacji o zaufanych dostawców tożsamości (Azure AD) i klucze używane do sprawdzania poprawności tokenów wydawanych przez nich. VINR ułatwia także automatycznie aktualizować informacje o kluczu przechowywane w pliku Web.config, pobierając najnowszą wersję dokumentu metadanych Federacji skojarzony z katalogu, sprawdzanie, czy konfiguracja jest nieaktualna w porównaniu z najnowszą wersję dokumentu i aktualizowanie aplikacji przy użyciu nowego klucza stosownie do potrzeb.

Jeśli utworzono aplikację za pomocą przykłady kodu lub dokumentacji instruktażu dostarczane przez firmę Microsoft, logiczny przerzucania klucza jest już uwzględniona w projekcie. Można zauważyć, że kod poniżej już istnieje w projekcie. Jeśli aplikacji nie ma jeszcze tę logikę, wykonaj następujące czynności, aby dodać go i sprawdzić, czy działa poprawnie.

1. W **Eksploratorze rozwiązań**Dodaj odwołanie do zestawu **System.IdentityModel** dla odpowiedni projekt.
2. Otwórz plik **Global.asax.cs** i Dodaj następujący za pomocą dyrektywy:
```
using System.Configuration;
using System.IdentityModel.Tokens;
```
3. Można dodać do pliku **Global.asax.cs** metodę następujące czynności:
```
protected void RefreshValidationSettings()
{
    string configPath = AppDomain.CurrentDomain.BaseDirectory + "\\" + "Web.config";
    string metadataAddress =
                  ConfigurationManager.AppSettings["ida:FederationMetadataLocation"];
    ValidatingIssuerNameRegistry.WriteToConfig(metadataAddress, configPath);
}
```
4. Wywołać metodę **RefreshValidationSettings()** w metodzie **Application_Start()** w **Global.asax.cs** , jak pokazano:
```
protected void Application_Start()
{
    AreaRegistration.RegisterAllAreas();
    ...
    RefreshValidationSettings();
}
```

Po wykonaniu tych czynności Web.config aplikacji zostaną zaktualizowane przy użyciu najnowszych informacji z Federacji metadanych dokumentu, w tym najnowsze kluczy. Tej aktualizacji wystąpi każdorazowo odtwarza do puli aplikacji w programie IIS; Domyślnie usług IIS ustawiono odtwarzać aplikacje co 29 godziny.

Wykonaj poniższe czynności, aby zweryfikować logiczny przerzucania klucza.

1. Po upewnieniu się, czy aplikacja używa kodu powyżej, otwórz plik **Web.config** i przejdź do **<issuerNameRegistry>** bloku, a w szczególności szukasz następujące kilka wierszy:
```
<issuerNameRegistry type="System.IdentityModel.Tokens.ValidatingIssuerNameRegistry, System.IdentityModel.Tokens.ValidatingIssuerNameRegistry">
        <authority name="https://sts.windows.net/ec4187af-07da-4f01-b18f-64c2f5abecea/">
          <keys>
            <add thumbprint="3A38FA984E8560F19AADC9F86FE9594BB6AD049B" />
          </keys>
```
2. W **<add thumbprint=””>** Ustawianie, zmienianie wartość, zamieniając dowolny znak inny. Zapisz plik **Web.config** .

3. Tworzenie aplikacji, a następnie uruchom go. Aby ukończyć proces logowania, aplikacja pomyślnie aktualizuje klucz, pobierając wymaganych informacji z usługi katalogowej Federacji metadanych dokumentu. Jeśli występują problemy z zalogowaniem się, upewnij się, zmiany w aplikacji są poprawne, czytając temat [Dodawania logowania jednokrotnego na używanie aplikacji sieci Web Azure AD](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) lub pobierając i sprawdzanie Poniższy przykładowy kod: [Wielu dzierżawy chmury aplikacji dla usługi Azure Active Directory](https://code.msdn.microsoft.com/multi-tenant-cloud-8015b84b).


### <a name="vs2010"></a>Aplikacje sieci Web ochrona zasobów i utworzone za pomocą programu Visual Studio 2008 lub Outlook 2010 i 1.0 Windows Identity Foundation (WIF) dla .NET 3.5

Jeśli skonstruowaniu aplikacji na WIF 1.0 jest podany mechanizm automatycznego odświeżania konfiguracji aplikacji za pomocą nowego klucza.

- *Najprostszym sposobem* Za pomocą narzędzia FedUtil zawarte w zestaw SDK WIF, który można pobrać najnowszą wersję dokumentu metadanych i zaktualizowanie konfiguracji.
- Aktualizowanie aplikacji do 4,5 .NET, zawierający najnowsza wersja WIF znajduje się w obszarze nazw systemu. Następnie za pomocą [Sprawdzania poprawności wystawcy nazwa rejestru (VINR)](https://msdn.microsoft.com/library/dn205067.aspx) do przeprowadzania aktualizacji automatycznej konfiguracji aplikacji.
- Uruchamianie ręczne najazd zgodnie z instrukcjami na końcu tego dokumentu wskazówki.

Instrukcje za pomocą FedUtil zaktualizowanie konfiguracji:

1. Upewnij się, że masz 1.0 WIF SDK zainstalowana na tym komputerze rozwoju dla programu Visual Studio 2008 lub Outlook 2010. Możesz [pobrać ją z tego miejsca](https://www.microsoft.com/en-us/download/details.aspx?id=4451) , jeśli nie zainstalowano jeszcze go.
2. W programie Visual Studio Otwórz rozwiązanie, kliknij prawym przyciskiem myszy stosowne projektu i wybierz opcję **Aktualizuj metadanych Federacji**. Jeśli ta opcja nie jest dostępna, FedUtil i/lub 1.0 WIF SDK nie został zainstalowany.
3. W wierszu wybierz pozycję **Aktualizuj** , aby rozpocząć aktualizowania metadanych usługi federacyjne. Jeśli masz dostęp do środowiska serwera miejsce, w którym znajduje się aplikacja, można używać w FedUtil [metadanych automatyczne aktualizowanie harmonogramu](https://msdn.microsoft.com/library/ee517272.aspx).
4. Kliknij przycisk **Zakończ** , aby ukończyć proces aktualizacji.

### <a name="other"></a>Aplikacje internetowe / interfejsy API chroni zasoby przy użyciu innych bibliotek lub ręcznie implementacją protokołów

Jeśli używasz niektórych innej biblioteki lub ręcznie wdrożonej dowolnego z obsługiwanych protokołów, należy przejrzeć biblioteki lub implementacją, aby upewnić się, że klucz jest pobierana z dokumentu odnajdowania łączenie OpenID lub dokument metadanych federacji. Jednym ze sposobów sprawdzenia tego jest wyszukać w kodzie lub kodu biblioteki wezwań się do dokumentu odnajdowania OpenID lub dokument metadanych federacji.

Jeśli ich klucza jest jest przechowywana lub ustalony w aplikacji, możesz ręcznie pobrać klucz i Aktualizuj odpowiednio przez wykonuje ręcznego najazd zgodnie z instrukcjami na końcu tego dokumentu wskazówki. **Jest zalecamy, aby zwiększyć aplikacji do obsługi najazd automatyczne** za pomocą dowolnej z metod konspektu, w tym artykule, aby uniknąć zakłócenia w przyszłości i obciążenie, jeśli zwiększa Azure AD jest cadence najazd, lub ma alarmowych przerzucania w nowym oknie grupy.

## <a name="how-to-test-your-application-to-determine-if-it-will-be-affected"></a>Jak przetestować aplikację do określenia, czy jej wpłynie

Można sprawdzić, czy aplikacja obsługuje automatycznego przerzucania klucza, pobierając skrypty i postępując zgodnie z instrukcjami [to repozytorium GitHub.](https://github.com/AzureAD/azure-activedirectory-powershell-tokenkey)

## <a name="how-to-perform-a-manual-rollover-if-you-application-does-not-support-automatic-rollover"></a>Jak przeprowadzić ręcznego najazd, jeśli możesz aplikacji nie obsługuje automatycznego najazd

Jeśli aplikacja jest **nie** najazd automatyczne pomocy technicznej, będzie konieczne uznania tego procesu, który okresowo monitoruje Azure AD klucze podpisywania i w związku z tym wykonuje ręcznego przerzucania. [Repozytorium to GitHub](https://github.com/AzureAD/azure-activedirectory-powershell-tokenkey) zawiera skrypty i instrukcje dotyczące wykonywania tego zadania.
