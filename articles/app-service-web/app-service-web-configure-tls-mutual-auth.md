<properties 
    pageTitle="Jak skonfigurować TLS wzajemnego uwierzytelniania dla aplikacji sieci Web" 
    description="Dowiedz się, jak skonfigurować aplikacji sieci web umożliwia uwierzytelnianie klienta na podstawie certyfikatu na TLS." 
    services="app-service" 
    documentationCenter="" 
    authors="naziml" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/08/2016" 
    ms.author="naziml"/>    

# <a name="how-to-configure-tls-mutual-authentication-for-web-app"></a>Jak skonfigurować TLS wzajemnego uwierzytelniania dla aplikacji sieci Web

## <a name="overview"></a>Omówienie ##
Można ograniczyć dostęp do aplikacji sieci Azure web, umożliwiając różnych typów uwierzytelniania dla niego. Jednym ze sposobów jest uwierzytelnianie za pomocą certyfikatu klienta, gdy żądanie zostanie przez TLS/SSL. Mechanizm ten jest nazywany TLS wzajemnego uwierzytelniania lub certyfikat klienta, że uwierzytelnianie i ten artykuł zawiera szczegółowe konfigurowanie aplikacji sieci web, aby użyć uwierzytelnianie klienta na podstawie certyfikatu.

> **Uwaga:** Jeśli uzyskujesz dostęp do witryny przez HTTP i HTTPS nie, nie otrzymasz dowolny certyfikat klienta. Jeśli aplikacja wymaga certyfikatów klientów możesz należy nie pozwalają na żądania do aplikacji za pomocą protokołu HTTP.


[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="configure-web-app-for-client-certificate-authentication"></a>Konfigurowanie aplikacji sieci Web dla uwierzytelnianie klienta na podstawie certyfikatu ##
Konfigurowanie aplikacji sieci web, aby wymagać certyfikaty klientów należy dodać clientCertEnabled ustawienie witryny dla aplikacji sieci web i ustaw ją na wartość PRAWDA. To ustawienie nie jest obecnie dostępna za pośrednictwem środowiska zarządzania w portalu i interfejsu API usługi REST muszą być używane w tym celu.

Za pomocą [Narzędzia ARMClient](https://github.com/projectkudu/ARMClient) ułatwia jednostki połączenia interfejsu API usługi REST. Po zalogowaniu się przy użyciu narzędzia należy wykonać następujące polecenie:

    ARMClient PUT subscriptions/{Subscription Id}/resourcegroups/{Resource Group Name}/providers/Microsoft.Web/sites/{Website Name}?api-version=2015-04-01 @enableclientcert.json -verbose
    
zastępowanie wszystkich elementów w {} informacji dla aplikacji sieci web i tworzenia pliku enableclientcert.json z następujących JSON o nazwie zawartości:

> {"położenie": "Moja aplikacja lokalizacja w sieci Web"   
>   "właściwości": {  
>     "clientCertEnabled": PRAWDA}}  

Upewnij się zmienić wartość "położenie" wszędzie tam, gdzie znajduje się aplikacji sieci web np Północna centralnej US "lub" Zachód USA itp.

> **Uwaga:** Po uruchomieniu ARMClient z programu Powershell, będzie konieczne escape @ symbol plik JSON z powrotem osi ".

## <a name="accessing-the-client-certificate-from-your-web-app"></a>Uzyskiwanie dostępu do certyfikatu klienta aplikacji sieci Web ##
Jeśli używasz programu ASP.NET i konfigurowanie aplikacji umożliwia uwierzytelnianie klienta na podstawie certyfikatu, certyfikat będzie dostępna za pośrednictwem właściwości **HttpRequest.ClientCertificate** . Dla innych stosy aplikacji certyfikatu klienta będą dostępne w aplikacji przez wartość base64 zakodowany w nagłówku żądania "X-ARR-ClientCert". Aplikacja można utworzyć certyfikat z tej wartości i używaj go na potrzeby uwierzytelniania i autoryzacji w aplikacji.

## <a name="special-considerations-for-certificate-validation"></a>Uwagi dotyczące sprawdzania poprawności certyfikatów ##
Certyfikat klienta, które są wysyłane do aplikacji nie przechodzi przez poprawności przez platformy Azure aplikacji sieci Web. Sprawdzanie poprawności ten certyfikat spoczywa aplikacji sieci web. Poniżej przedstawiono przykładowy ASP.NET kod, który sprawdza właściwości certyfikatu na potrzeby uwierzytelniania.

    using System;
    using System.Collections.Specialized;
    using System.Security.Cryptography.X509Certificates;
    using System.Web;

    namespace ClientCertificateUsageSample
    {
        public partial class cert : System.Web.UI.Page
        {
            public string certHeader = "";
            public string errorString = "";
            private X509Certificate2 certificate = null;
            public string certThumbprint = "";
            public string certSubject = "";
            public string certIssuer = "";
            public string certSignatureAlg = "";
            public string certIssueDate = "";
            public string certExpiryDate = "";
            public bool isValidCert = false;

            //
            // Read the certificate from the header into an X509Certificate2 object
            // Display properties of the certificate on the page
            //
            protected void Page_Load(object sender, EventArgs e)
            {
                NameValueCollection headers = base.Request.Headers;
                certHeader = headers["X-ARR-ClientCert"];
                if (!String.IsNullOrEmpty(certHeader))
                {
                    try
                    {
                        byte[] clientCertBytes = Convert.FromBase64String(certHeader);
                        certificate = new X509Certificate2(clientCertBytes);
                        certSubject = certificate.Subject;
                        certIssuer = certificate.Issuer;
                        certThumbprint = certificate.Thumbprint;
                        certSignatureAlg = certificate.SignatureAlgorithm.FriendlyName;
                        certIssueDate = certificate.NotBefore.ToShortDateString() + " " + certificate.NotBefore.ToShortTimeString();
                        certExpiryDate = certificate.NotAfter.ToShortDateString() + " " + certificate.NotAfter.ToShortTimeString();
                    }
                    catch (Exception ex)
                    {
                        errorString = ex.ToString();
                    }
                    finally 
                    {
                        isValidCert = IsValidClientCertificate();
                        if (!isValidCert) Response.StatusCode = 403;
                        else Response.StatusCode = 200;
                    }
                }
                else
                {
                    certHeader = "";
                }
            }

            //
            // This is a SAMPLE verification routine. Depending on your application logic and security requirements, 
            // you should modify this method
            //
            private bool IsValidClientCertificate()
            {
                // In this example we will only accept the certificate as a valid certificate if all the conditions below are met:
                // 1. The certificate is not expired and is active for the current time on server.
                // 2. The subject name of the certificate has the common name nildevecc
                // 3. The issuer name of the certificate has the common name nildevecc and organization name Microsoft Corp
                // 4. The thumbprint of the certificate is 30757A2E831977D8BD9C8496E4C99AB26CB9622B
                //
                // This example does NOT test that this certificate is chained to a Trusted Root Authority (or revoked) on the server 
                // and it allows for self signed certificates
                //

                if (certificate == null || !String.IsNullOrEmpty(errorString)) return false;
                
                // 1. Check time validity of certificate
                if (DateTime.Compare(DateTime.Now, certificate.NotBefore) < 0 || DateTime.Compare(DateTime.Now, certificate.NotAfter) > 0) return false;
                
                // 2. Check subject name of certificate
                bool foundSubject = false;
                string[] certSubjectData = certificate.Subject.Split(new char[] { ',' }, StringSplitOptions.RemoveEmptyEntries);
                foreach (string s in certSubjectData)
                {
                    if (String.Compare(s.Trim(), "CN=nildevecc") == 0)
                    {
                        foundSubject = true;
                        break;
                    }
                }
                if (!foundSubject) return false;

                // 3. Check issuer name of certificate
                bool foundIssuerCN = false, foundIssuerO = false;
                string[] certIssuerData = certificate.Issuer.Split(new char[] { ',' }, StringSplitOptions.RemoveEmptyEntries);
                foreach (string s in certIssuerData)
                {
                    if (String.Compare(s.Trim(), "CN=nildevecc") == 0)
                    {
                        foundIssuerCN = true;
                        if (foundIssuerO) break;
                    }

                    if (String.Compare(s.Trim(), "O=Microsoft Corp") == 0)
                    {
                        foundIssuerO = true;
                        if (foundIssuerCN) break;
                    }
                }

                if (!foundIssuerCN || !foundIssuerO) return false;

                // 4. Check thumprint of certificate
                if (String.Compare(certificate.Thumbprint.Trim().ToUpper(), "30757A2E831977D8BD9C8496E4C99AB26CB9622B") != 0) return false;

                // If you also want to test if the certificate chains to a Trusted Root Authority you can uncomment the code below
                //
                //X509Chain certChain = new X509Chain();
                //certChain.Build(certificate);
                //bool isValidCertChain = true;
                //foreach (X509ChainElement chElement in certChain.ChainElements)
                //{
                //    if (!chElement.Certificate.Verify())
                //    {
                //        isValidCertChain = false;
                //        break;
                //    }
                //}
                //if (!isValidCertChain) return false;

                return true;
            }
        }
    }
