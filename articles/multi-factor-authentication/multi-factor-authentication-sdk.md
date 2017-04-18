<properties 
    pageTitle="Integrowanie tożsamości lokalnych z usługi Azure Active Directory."
    description="To jest Azure AD Connect w tym artykule opisano, co to jest i dlaczego można użyć go."
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
    ms.topic="article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="building-multi-factor-authentication-into-custom-apps-sdk"></a>Uwierzytelnianie wieloskładnikowe konstrukcyjnych do niestandardowe aplikacje (SDK)

> [AZURE.IMPORTANT]  Jeśli chcesz pobrać zestaw SDK, należy utworzyć dostawcę uwierzytelnianie wieloskładnikowe Azure, nawet jeśli masz Azure MFA, AAD Premium lub EMS licencji.  Jeśli w tym celu utwórz dostawcą uwierzytelnianie wieloskładnikowe Azure i już licencji, będzie musiał utworzyć dostawcę z modelem **Na użytkownika z włączoną** i połączyć katalog zawierający Azure MFA, Azure AD Premium lub EMS licencji dostawcę.  Temu będziesz mieć pewność, że nie konta chyba że użytkownik ma więcej unikatowych użytkowników przy użyciu zestawu SDK niż liczba licencji użytkownika.

Azure wieloskładnikowe uwierzytelniania Software Development Kit (SDK) umożliwia tworzenie rozmowy telefonicznej i tekst wiadomości weryfikacji bezpośrednio do logowania lub transakcji procesów aplikacji w dzierżawie usługi Azure AD.

SDK uwierzytelnianie wieloskładnikowe jest dostępna dla C#, Visual Basic (.NET), Java, Perl, PHP i dopiskiem. Zestaw SDK zawiera cienkie opakowanie wokół uwierzytelnianie wieloskładnikowe. Zawiera wszystkie elementy potrzebne do pisania kodu, w tym pliki kodu źródłowego komentarzy, przykładowe pliki i szczegółowe pliku ReadMe. Każdy zestaw SDK zawiera również certyfikat i klucz prywatny szyfrowania transakcji, który jest unikatowy dla dostawcy uwierzytelnianie wieloskładnikowe. Ile masz dostawcę, możesz pobrać SDK w w wielu językach i formaty potrzebne.

Struktura interfejsów API w SDK uwierzytelnianie wieloskładnikowe jest bardzo proste. Możesz wykorzystać pojedynczą funkcję połączeń API z parametrami wieloskładnikowe opcji, takich jak tryb weryfikacji i dane użytkowników, na przykład numer telefonu lub numer PIN, aby sprawdzić poprawność. Za pośrednictwem interfejsów API tłumaczenie wywołaniu funkcji do żądania usługi sieci web do usługi opartej na chmurze uwierzytelnianie wieloskładnikowe Azure. Wszystkie połączenia musi zawierać odwołanie do prywatnych certyfikat, który znajduje się w każdej SDK.

Ponieważ za pośrednictwem interfejsów API nie mają dostępu do użytkowników zarejestrowanych w usłudze Azure Active Directory, musisz podać informacje o użytkowniku, takie jak numery telefonów i kodów numeru PIN, pliku lub bazy danych. Ponadto za pośrednictwem interfejsów API nie zawierają rejestrowania lub użytkownika funkcje zarządzania, potrzebne do utworzenia tych procesów do aplikacji.






## <a name="download-the-azure-multi-factor-authentication-sdk"></a>Pobierz Azure uwierzytelnianie wieloskładnikowe SDK

Pobieranie zestawu SDK wieloskładnikowe Azure wymaga [Dostawcy uwierzytelnianie wieloskładnikowe Azure](multi-factor-authentication-get-started-auth-provider.md).  Wymaga pełnego Azure subskrypcji, nawet jeśli należą Azure MFA, Azure AD Premium lub Enterprise Suite mobilności licencji.  Aby pobrać zestawu SDK, należy przejść do portalu zarządzania wieloskładnikowe korzystając z funkcji zarządzania dostawcy uwierzytelnianie wieloskładnikowe bezpośrednio lub klikając łącze **"Przejdź do portalu"** na stronie Ustawienia usługi MFA.


### <a name="to-download-the-azure-multi-factor-authentication-sdk-from-the-azure-portal"></a>Aby pobrać SDK uwierzytelnianie wieloskładnikowe Azure za pomocą portalu Azure


1. Zaloguj się do portalu Azure jako Administrator.
2. Po lewej stronie wybierz pozycję usługi Active Directory.
3. Na stronie usługi Active Directory u góry kliknij **Dostawców uwierzytelniania wieloskładnikowego**
4. U dołu kliknij pozycję **Zarządzaj**
5. Spowoduje to otwarcie nowej strony.  Z lewej strony, u dołu ekranu kliknij pozycję SDK.
<center>![Plik do pobrania](./media/multi-factor-authentication-sdk/download.png)</center>
6. Wybierz język i kliknij przycisk na jednym łączy do pobierania.
7. Zapisz plik do pobrania.



### <a name="to-download-the-azure-multi-factor-authentication-sdk-via-the-service-settings"></a>Aby pobrać SDK uwierzytelnianie wieloskładnikowe Azure za pomocą ustawień usługi


1. Zaloguj się do portalu Azure jako Administrator.
2. Po lewej stronie wybierz pozycję usługi Active Directory.
3. Kliknij dwukrotnie wystąpienia programu Azure AD.
4. U góry kliknij przycisk **Konfiguruj**
5. W obszarze uwierzytelnianie wieloskładnikowe wybierz pozycję **Ustawienia usługi Zarządzanie**
![Pobierz](./media/multi-factor-authentication-sdk/download2.png)
6. Na stronie Ustawienia usług u dołu ekranu kliknij przycisk **Przejdź do portalu**.
![Plik do pobrania](./media/multi-factor-authentication-sdk/download3a.png)
7. Spowoduje to otwarcie nowej strony.  Z lewej strony, u dołu ekranu kliknij pozycję SDK.
8. Wybierz język i kliknij przycisk na jednym łączy do pobierania.
9. Zapisz plik do pobrania.

## <a name="contents-of-the-azure-multi-factor-authentication-sdk"></a>Zawartość Azure uwierzytelnianie wieloskładnikowe SDK
Wewnątrz zestawu SDK znajdują się następujące elementy:

- **Plik README**. Opisano sposób użycia interfejsów API uwierzytelnianie wieloskładnikowe w nowym lub istniejącym aplikacji.
- **Pliki źródła** uwierzytelniania wieloskładnikowego
- **Certyfikat klienta** , który umożliwia komunikowanie się z usługą uwierzytelnianie wieloskładnikowe
- **Klucz prywatny** certyfikatu
- **Zadzwoń do wyników.** Lista kodów wynik połączenia. Aby otworzyć ten plik, skorzystaj z aplikacji z tekstem, formatowanie, na przykład WordPad. Aby przetestować i rozwiązywanie problemów z stosowania uwierzytelnianie wieloskładnikowe w aplikacji przy użyciu kodów wynik połączenia. Nie są one kody stanu uwierzytelniania.
- **Przykłady.** Przykładowy kod stosowania pracy podstawowe uwierzytelnianie wieloskładnikowe.


>[AZURE.WARNING]Certyfikat klienta jest unikatowy certyfikat prywatne, który został wygenerowany specjalnie dla Ciebie. Udostępnij lub nie utracić tego pliku. Jest kluczem do zapewnienia bezpieczeństwa komunikacji z usługą uwierzytelnianie wieloskładnikowe.

## <a name="code-sample-standard-mode-phone-verification"></a>Próbki kodu: Weryfikacja telefoniczna trybie standardowym

Ten przykładowy kod pokazano, jak dodać weryfikacji połączeń głosowych trybie standardowym do aplikacji przy użyciu interfejsów API w SDK uwierzytelnianie wieloskładnikowe Azure. Tryb standardowy jest połączenie telefoniczne, która odpowiada użytkownik, naciskając klawisz #.

W tym przykładzie użyto C# .NET 2.0 wieloskładnikowe uwierzytelniania SDK w podstawowych aplikacji ASP.NET z C# logiczny po stronie serwera, ale ten proces jest bardzo podobne prosty implementacji w innych językach. Ponieważ zestawu SDK zawiera pliki źródłowe, nie wykonywalny plików, można tworzyć pliki i ich wyszukiwanie lub je uwzględnić bezpośrednio w aplikacji.

>[AZURE.NOTE]Stosując uwierzytelnianie wieloskładnikowe stanowić dodatkowe czynniki weryfikacji pomocniczej lub wyższego uzupełnienie metody uwierzytelniania podstawowego. Metody te nie są przeznaczone do użycia jako metody uwierzytelniania podstawowego.

### <a name="code-sample-overview"></a>Przegląd próbki kodu
Ten przykład kodu dla aplikacji sieci web bez trudu pokaz używa rozmowy telefonicznej z odpowiedzią klucza # przeprowadzić uwierzytelnianie użytkownika. Ten czynnik połączenie telefoniczne jest nazywany w uwierzytelnianie wieloskładnikowe trybie standardowym.

Kod po stronie klienta nie zawiera żadnych elementów specyficznych uwierzytelnianie wieloskładnikowe. Ponieważ współczynniki dodatkowego uwierzytelniania są niezależne od uwierzytelniania podstawowego, możesz dodać je bez zmieniania istniejącego interfejsu logowania jednokrotnego. Interfejs API programu SDK wieloskładnikowe umożliwiają dostosowywanie sposobu pracy użytkownika, ale nie może być konieczne wprowadzanie zmian w ogóle.

Kod po stronie serwera dodaje uwierzytelniania w trybie standardowym w kroku 2. Tworzy obiekt PfAuthParams z parametrami, które są wymagane na potrzeby weryfikacji w trybie standardowym: nazwa użytkownika, telefonu numer oraz tryb i ścieżkę do certyfikatu klienta (CertFilePath), który jest wymagany w każdej konwersacji. Aby wyświetlić pokaz wszystkie parametry w PfAuthParams zobacz przykładowy plik w zestawie SDK.

Następnie kod przekazuje obiekt PfAuthParams funkcji pf_authenticate(). Zwracana wartość wskazuje sukces lub niepowodzenie uwierzytelniania. Parametry wyjściowe, callStatus i identyfikator błędu, zawierają informacje wynik dodatkowe połączenia. Kody wynik połączenia są rejestrowane w pliku wyników połączenia w zestawie SDK.

Ta implementacja minimalnego można napisać w kilku wierszy. Jednak w kodzie produkcyjnym, można dołączyć bardziej zaawansowane obsługi błędów, kod dodatkowej bazy danych i funkcji użytkownika.

### <a name="web-client-code"></a>Kod klienta sieci Web

Poniżej przedstawiono kod klienta sieci web dla strony pokaz.


    <%@ Page Language="C#" AutoEventWireup="true" CodeFile="Default.aspx.cs" Inherits="_Default" %>

    <!DOCTYPE html>

    <html xmlns="http://www.w3.org/1999/xhtml">
    <head runat="server">
    <title>Multi-Factor Authentication Demo</title>
    </head>
    <body>
    <h1>Azure Multi-Factor Authentication Demo</h1>
    <form id="form1" runat="server">

    <div style="width:auto; float:left">
    Username:&nbsp;<br />
    Password:&nbsp;<br />
    </div>

    <div">
    <asp:TextBox id="username" runat="server" width="100px"/><br />
    <asp:Textbox id="password" runat="server" width="100px" TextMode="password" /><br />
    </div>

    <asp:Button id="btnSubmit" runat="server" Text="Log in" onClick="btnSubmit_Click"/>

    <p><asp:Label ID="lblResult" runat="server"></asp:Label></p>

    </form>
    </body>
    </html>


### <a name="server-side-code"></a>Kod po stronie serwera

Poniższy kod po stronie serwera uwierzytelnianie wieloskładnikowe jest skonfigurowany i uruchamiania w kroku 2. Tryb standardowy (MODE_STANDARD) jest połączenie telefoniczne, do której użytkownik odpowiada, naciskając klawisz #.

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using System.Web.UI;
    using System.Web.UI.WebControls;

    public partial class _Default : System.Web.UI.Page
    {
        protected void Page_Load(object sender, EventArgs e)
        {
        }

        protected void btnSubmit_Click(object sender, EventArgs e)
        {
            // Step 1: Validate the username and password
            if (username.Text != "Contoso" || password.Text != "password")
            {
                lblResult.ForeColor = System.Drawing.Color.Red;
                lblResult.Text = "Username or password incorrect.";
            }
            else
            {
                // Step 2: Perform multi-factor authentication

                // Add call details from the user database.
                PfAuthParams pfAuthParams = new PfAuthParams();
                pfAuthParams.Username = username.Text;
                pfAuthParams.Phone = "9134884271";
                pfAuthParams.Mode = pf_auth.MODE_STANDARD;

                // Specify a client certificate
                // NOTE: This file contains the private key for the client
                // certificate. It must be stored with appropriate file
                // permissions.
                pfAuthParams.CertFilePath = "c:\\cert_key.p12";

                // Perform phone-based authentication
                int callStatus;
                int errorId;

                if(pf_auth.pf_authenticate(pfAuthParams, out callStatus, out errorId))
                {
                    lblResult.ForeColor = System.Drawing.Color.Green;
                    lblResult.Text = "Multi-Factor Authentication succeeded.";
                }
                else
                {
                    lblResult.ForeColor = System.Drawing.Color.Red;
                    lblResult.Text = " Multi-Factor Authentication failed.";
                }
            }

        }
    }
