<properties
    pageTitle="Azure AD Federacji zgodności listy"
    description="Ta strona zawiera dostawców tożsamości innych firm, które mogą być używane w celu wdrożenia logowania jednokrotnego."
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/12/2016"
    ms.author="billmath"/>

# <a name="azure-ad-federation-compatibility-list"></a>Azure AD Federacji zgodności listy
Azure Active Directory zawiera logowania jednokrotnego po oraz ulepszonych zabezpieczeń programu access aplikacji dla usługi Office 365 i innych usług Microsoft Online hybrydowych i tylko do chmury implementacji bez konieczności dowolne rozwiązanie firmy Microsoft. Usługa Office 365, takie jak większość usługami Online firmy Microsoft, jest zintegrowany z usługi Azure Active Directory dla usług katalogowych, uwierzytelniania i autoryzacji. Azure Active Directory udostępnia również rejestracji jednokrotnej do tysięcy aplikacji władz akredytacji bezpieczeństwa i aplikacji sieci web w lokalnej. Zobacz obsługiwane aplikacje władz akredytacji bezpieczeństwa w galerii aplikacji usługi Azure Active Directory.

Dla organizacji, które mają zainwestowanego w rozwiązań Federacji firmy Microsoft ten temat zawiera wytyczne dotyczące konfigurowania rejestracji jednokrotnej dla swoich użytkowników usługi Active Directory systemu Windows Server z usługami Microsoft Online za pomocą dostawców tożsamości innych firm z "Azure Active Directory federation zgodności"poniższej listy. 


![](./media/active-directory-aadconnect-federation-compatibility/oxford2.jpg)   
[Grupa komputerów Oxford](http://oxfordcomputergroup.com/)innej firmy, w imieniu firmy Microsoft, testowanych pojedynczej funkcji logowania jednokrotnego dostawców tożsamości innych firm z zestawem przypadków użycia typowych za pomocą usługi Azure Active Directory.

Informacji na temat jak można uzyskać dostawcy tożsamości innych firm na liście, skontaktuj się z Oxford komputera Group pod adresem [idp@oxfordcomputergroup.com](mailto:idp@oxfordcomputergroup.com).

>[AZURE.IMPORTANT] Grupa komputerów Oxford przetestowane tylko funkcje Federacji pojedynczego logowania jednokrotnego scenariuszom. Grupa komputerów Oxford nie testowanie dowolnego synchronizacji, uwierzytelnianie dwuskładnikowe, części itd., pojedynczego logowania jednokrotnego scenariuszom.

>Korzystanie z logowania za pomocą Identyfikatora alternatywny do głównej nazwy użytkownika nie jest również sprawdzany w tym programie.



- [Azure Active Directory](#azure-active-directory)
- [Usługi federacyjne serwera wirtualnego tożsamości optymalnego IDM](#optimal-idm-virtual-identity-server-federation-services) 
- [PingFederate 6.11](#pingfederate-611) 
- [PingFederate 7.2](#pingfederate-72) 
- [PingFederate 8.x](#pingfederate-8x)
- [Centrify](#centrify) 
- [IBM Tivoli federacji tożsamości menedżera 6.2.2](#ibm-tivoli-federated-identity-manager-622) 
- [SecureAuth protokołu IdP 7.2.0](#secureauth-idp-720) 
- [Urząd certyfikacji SiteMinder 12.52](#ca-siteminder-1252-sp1-cumulative-release-4) 
- [RadiantOne CFS 3.0](#radiantone-cfs-30) 
- [Okta](#okta) 
- [OneLogin](#onelogin) 
- [Menedżer dostępu NetIQ 4.0.1](#netiq-access-manager-401) 
- [BIG IP przy użyciu Menedżera zasad dostępu BIG IP wersja 11.3 x — 11.6](#big-ip-with-access-policy-manager-big-ip-ver-113x-116x) 
- [Portalu obszaru roboczego VMware wersji 2.1](#vmware-workspace-portal-version-21) 
- [Zaloguj się i przejdź 5.3](#signampgo-53) 
- [Federacja IceWall w wersji 3.0](#icewall-federation-version-30) 
- [Chmura bezpieczny urzędu certyfikacji](#ca-secure-cloud) 
- [V7.1 Menedżera dostępu chmury tożsamości Dell z nich](#dell-one-identity-cloud-access-manager-v71) 
- [Zaloguj się pojedynczego AuthAnvil na 4,5](#authavil-single-sign-on-45) 

>[AZURE.IMPORTANT] Ponieważ są one produkty innych firm, firma Microsoft nie udziela pomocy wdrożenia, konfiguracji, rozwiązywania problemów, najważniejsze wskazówki, itd. problemy i pytania dotyczące tych dostawców tożsamości. Obsługę i pytania dotyczące tych dostawców tożsamości skontaktuj się bezpośrednio z obsługiwanych firm.

>Ci dostawcy tożsamości innych firm zostały przetestowane współdziałanie z usługami w chmurze firmy Microsoft przy użyciu federacyjnych i tylko protokoły WS zaufania. Testowanie nie zawiera przy użyciu protokołu SAML.

## <a name="azure-active-directory"></a>Azure Active Directory 
Azure Active Directory może przeprowadzać uwierzytelnianie użytkowników przez federacyjnego z sieci lokalnej usługi Active Directory lub bez lokalnego serwera Federacji za pomocą synchronizacji haseł. 

Poniżej przedstawiono diagram obsługi scenariusz dla tego działania logowania jednokrotnego: 


| Klienta |Pomoc techniczna  |Wyjątki|
| --------- | --------- |--------- |
| Klienci sieci Web, tacy jak dostępu do sieci Web programu Exchange i usługi SharePoint Online | Obsługiwane |Brak|
| Aplikacje klienckie sformatowany przykład programu Lync, subskrypcji pakietu Office, CRM |  Obsługiwane |Brak|
| Klientów bogate w wiadomości e-mail, takie jak Outlook i ActiveSync |  Obsługiwane |Brak|
|Nowoczesna aplikacji przy użyciu ADAL, takich jak pakiet Office 2016| Obsługiwane|Brak|

Aby uzyskać więcej informacji na temat usługi Azure Active Directory za pomocą usług AD FS zobacz [Active Directory Federation Services (ADFS)](active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs)

Aby uzyskać więcej informacji o korzystaniu z usługi Azure Active Directory z synchronizacji haseł zobacz [Narzędzie Azure AD Connect](active-directory-aadconnect.md).


## <a name="optimal-idm-virtual-identity-server-federation-services"></a>Usługi federacyjne serwera wirtualnego tożsamości optymalnego IDM 
Optymalne IDM wirtualną tożsamości serwera Federation Services może przeprowadzać uwierzytelnianie użytkowników, których znajdują się w katalogach aktywnego lokalnego klientów.

Oto tego scenariusza obsługuje macierzy jednego środowiska tego logowania jednokrotnego:


| Klienta |Pomoc techniczna  |Wyjątki|
| --------- | --------- |--------- |
| Klienci sieci Web, tacy jak dostępu do sieci Web programu Exchange i usługi SharePoint Online | Obsługiwane |Brak|
| Aplikacje klienckie sformatowany przykład programu Lync, subskrypcji pakietu Office, CRM |  Obsługiwane |Zintegrowane uwierzytelnianie systemu Windows|
| Klientów bogate w wiadomości e-mail, takie jak Outlook i ActiveSync |  Obsługiwane |Aby uzyskać więcej informacji na temat dostępu klienta zasady zobacz [Ograniczanie dostępu do usługi Office 365 usług na podstawie lokalizacji klienta.](https://technet.microsoft.com/library/hh526961.aspx)|



## <a name="pingfederate-611"></a>PingFederate 6.11 

PingFederate 6.11 wykonuje powszechnie używany standard tożsamości federacyjnych WS o podanie logowania jednokrotnego i strukturą atrybutów programu exchange.

Scenariusz macierzy pomocy technicznej dla tego jednego działania logowania jednokrotnego jest następująca:


| Klienta |Pomoc techniczna  |Wyjątki|
| --------- | --------- |--------- |
| Klienci sieci Web, tacy jak dostępu do sieci Web programu Exchange i usługi SharePoint Online | Obsługiwane |Brak|
| Aplikacje klienckie sformatowany przykład programu Lync, subskrypcji pakietu Office, CRM |  Obsługiwane |Brak (starszych wersjach, musisz uaktualnić do 6.11|
| Klientów bogate w wiadomości e-mail, takie jak Outlook i ActiveSync |  Obsługiwane |Brak|

Aby uzyskać instrukcje dotyczące PingFederate dotyczące konfigurowania to usługi STS, aby zapewnić pojedynczy obsługi logowania użytkowników usługi Active Directory, pobrać plik pdf [tutaj.](http://go.microsoft.com/fwlink/?LinkID=266321)

## <a name="pingfederate-72"></a>PingFederate 7.2 
PingFederate 7.2 wprowadza powszechnie używany standard tożsamości WS-WS-relacja zaufania federacji o podanie logowania jednokrotnego i strukturą atrybutów programu exchange.

Scenariusz diagram obsługi w przypadku pojedynczego obsługi tego logowania jednokrotnego jest następująca:


| Klienta |Pomoc techniczna  |Wyjątki|
| --------- | --------- |--------- |
| Klienci sieci Web, tacy jak dostępu do sieci Web programu Exchange i usługi SharePoint Online | Obsługiwane |Brak|
| Aplikacje klienckie sformatowany przykład programu Lync, subskrypcji pakietu Office, CRM |  Obsługiwane |Brak|
| Klientów bogate w wiadomości e-mail, takie jak Outlook i ActiveSync |  Obsługiwane |Brak|

Aby uzyskać PingFederate instrukcje dotyczące konfigurowania tej usługi STS zapewnić pojedynczy obsługi logowania użytkowników usługi Active Directory, zobacz [tutaj.](http://documentation.pingidentity.com/display/PF72/PingFederate+7.2)

## <a name="pingfederate-8x"></a>PingFederate 8.x 
PingFederate 8.x wykonuje powszechnie używany standard tożsamości WS-WS-relacja zaufania federacji o podanie logowania jednokrotnego i strukturą atrybutów programu exchange.

Scenariusz diagram obsługi w przypadku pojedynczego obsługi tego logowania jednokrotnego jest następująca:


| Klienta |Pomoc techniczna  |Wyjątki|
| --------- | --------- |--------- |
| Klienci sieci Web, tacy jak dostępu do sieci Web programu Exchange i usługi SharePoint Online | Obsługiwane |Brak|
| Aplikacje klienckie sformatowany przykład programu Lync, subskrypcji pakietu Office, CRM |  Obsługiwane |Brak|
| Klientów bogate w wiadomości e-mail, takie jak Outlook i ActiveSync |  Obsługiwane |Brak|

Aby uzyskać PingFederate instrukcje dotyczące konfigurowania tej usługi STS zapewnić pojedynczej obsługi logowania użytkowników usługi Active Directory, zobacz [tutaj.](http://documentation.pingidentity.com/display/PFS/SSO+to+Office+365+Introduction)

## <a name="centrify"></a>Centrify 
Centrify pomaga zapewnić federacyjnych pojedynczego logowania jednokrotnego obsługi dla usługi Office 365 bez wymagania obsługującego lokalnego serwera federacji.

Scenariusz macierzy pomocy technicznej dla tego jednego działania logowania jednokrotnego jest następująca:


| Klienta |Pomoc techniczna  |Wyjątki|
| --------- | --------- |--------- |
| Klienci sieci Web, tacy jak dostępu do sieci Web programu Exchange i usługi SharePoint Online | Obsługiwane |Brak|
| Aplikacje klienckie sformatowany przykład programu Lync, subskrypcji pakietu Office, CRM |  Obsługiwane |Brak|
| Klientów bogate w wiadomości e-mail, takie jak Outlook i ActiveSync |  Obsługiwane |Kontrola dostępu klienta nie jest obsługiwane. 

Aby uzyskać więcej informacji o Centrify, zobacz [tutaj.](http://www.centrify.com/cloud/apps/single-sign-on-for-office-365.asp)|

## <a name="ibm-tivoli-federated-identity-manager-622"></a>IBM Tivoli federacji tożsamości menedżera 6.2.2 
IBM Tivoli federacji tożsamości Menedżer 6.2.2 z IBM zabezpieczeń programu Access dla 1.4 aplikacji Microsoft wykonuje powszechnie używany standard tożsamości WS-WS-relacja zaufania federacji o podanie logowania jednokrotnego i strukturą atrybutów programu exchange.

Scenariusz macierzy pomocy technicznej dla tego jednego działania logowania jednokrotnego jest następująca: 

| Klienta |Pomoc techniczna  |Wyjątki|
| --------- | --------- |--------- |
| Klienci sieci Web, tacy jak dostępu do sieci Web programu Exchange i usługi SharePoint Online | Obsługiwane |Brak|
| Aplikacje klienckie sformatowany przykład programu Lync, subskrypcji pakietu Office, CRM |  Obsługiwane |Brak|
| Klientów bogate w wiadomości e-mail, takie jak Outlook i ActiveSync |  Obsługiwane |Brak|

Aby uzyskać więcej informacji na temat IBM Tivoli federacji tożsamości menedżera, zobacz [IBM zabezpieczeń programu Access Manager for Microsoft Applications.](http://www-01.ibm.com/support/docview.wss?uid=swg24029517)

## <a name="secureauth-idp-720"></a>SecureAuth protokołu IdP 7.2.0 
SecureAuth protokołu IdP 7.2.0 wykonuje powszechnie używany standard tożsamości WS-WS-relacja zaufania federacji o podanie obsługi rejestracji jednokrotnej i strukturą atrybutów programu exchange.

Scenariusz macierzy pomocy technicznej dla tego jednego działania logowania jednokrotnego jest następująca: 

| Klienta |Pomoc techniczna  |Wyjątki|
| --------- | --------- |--------- |
| Klienci sieci Web, tacy jak dostępu do sieci Web programu Exchange i usługi SharePoint Online | Obsługiwane |Brak|
| Aplikacje klienckie sformatowany przykład programu Lync, subskrypcji pakietu Office, CRM |  Obsługiwane |Brak|
| Klientów bogate w wiadomości e-mail, takie jak Outlook i ActiveSync |  Obsługiwane |Brak|

Aby uzyskać więcej informacji o SecureAuth zobacz [Protokołu IdP SecureAuth](http://go.microsoft.com/?linkid=9845293).

## <a name="ca-siteminder-1252-sp1-cumulative-release-4"></a>Urząd certyfikacji SiteMinder 12.52 z dodatkiem SP1 skumulowana w wersji 4
Urząd certyfikacji SiteMinder Federacja 12.52 wykonuje powszechnie używany standard tożsamości WS-WS-relacja zaufania federacji o podanie logowania jednokrotnego i strukturą atrybutów programu exchange.

Scenariusz macierzy pomocy technicznej dla tego jednego działania logowania jednokrotnego jest następująca: 

| Klienta |Pomoc techniczna  |Wyjątki|
| --------- | --------- |--------- |
| Klienci sieci Web, tacy jak dostępu do sieci Web programu Exchange i usługi SharePoint Online | Obsługiwane |Brak|
| Aplikacje klienckie sformatowany przykład programu Lync, subskrypcji pakietu Office, CRM |  Obsługiwane |Brak|
| Klientów bogate w wiadomości e-mail, takie jak Outlook i ActiveSync |  Obsługiwane |Brak|

Aby uzyskać więcej informacji o SiteMinder urzędu certyfikacji, zobacz [Federacja SiteMinder urząd certyfikacji.](http://www.ca.com/us/products/ca-single-sign-on.html) 

## <a name="radiantone-cfs-30"></a>RadiantOne CFS 3.0 
Usługi federacyjne chmury RadiantOne (CFS) 3.0 wprowadza powszechnie używany standard tożsamości WS-WS-relacja zaufania federacji o podanie logowania jednokrotnego i strukturą atrybutów programu exchange.

Scenariusz macierzy pomocy technicznej dla tego jednego działania logowania jednokrotnego jest następująca: 

| Klienta |Pomoc techniczna  |Wyjątki|
| --------- | --------- |--------- |
| Klienci sieci Web, tacy jak dostępu do sieci Web programu Exchange i usługi SharePoint Online | Obsługiwane |Brak|
| Aplikacje klienckie sformatowany przykład programu Lync, subskrypcji pakietu Office, CRM |  Obsługiwane |Zintegrowane uwierzytelnianie systemu Windows|
| Klientów bogate w wiadomości e-mail, takie jak Outlook i ActiveSync |  Obsługiwane |Brak|

Aby uzyskać więcej informacji o RadiantOne CFS, zobacz [RadiantOne CFS.](http://www.radiantlogic.com/products/radiantone-cfs/)


## <a name="okta"></a>Okta 
Okta wykonuje powszechnie używany standard tożsamości WS-WS-relacja zaufania federacji o podanie logowania jednokrotnego i strukturą atrybutów programu exchange.

Scenariusz macierzy pomocy technicznej dla tego jednego działania logowania jednokrotnego jest następująca: 


| Klienta |Pomoc techniczna  |Wyjątki|
| --------- | --------- |--------- |
| Klienci sieci Web, tacy jak dostępu do sieci Web programu Exchange i usługi SharePoint Online | Obsługiwane |Zintegrowane uwierzytelnianie systemu Windows wymaga konfiguracji serwera dodatkowe sieci web i aplikacji Okta.|
| Aplikacje klienckie sformatowany przykład programu Lync, subskrypcji pakietu Office, CRM |  Obsługiwane |Zintegrowane uwierzytelnianie systemu Windows|
| Klientów bogate w wiadomości e-mail, takie jak Outlook i ActiveSync |  Obsługiwane |Brak|

Aby uzyskać więcej informacji o Okta, zobacz [Okta.](https://www.okta.com/)
 
## <a name="onelogin"></a>OneLogin 
OneLogin jak badania w maja 2014 wykonuje powszechnie używany standard tożsamości WS-WS-relacja zaufania federacji o podanie logowania jednokrotnego i strukturą atrybutów programu exchange.

Scenariusz diagram obsługi w przypadku pojedynczego obsługi tego logowania jednokrotnego jest następująca: 

| Klienta |Pomoc techniczna  |Wyjątki|
| --------- | --------- |--------- |
| Klienci sieci Web, tacy jak dostępu do sieci Web programu Exchange i usługi SharePoint Online | Obsługiwane |Zintegrowane uwierzytelnianie systemu Windows|
| Aplikacje klienckie sformatowany przykład programu Lync, subskrypcji pakietu Office, CRM |  Obsługiwane |Zintegrowane uwierzytelnianie systemu Windows|
| Klientów bogate w wiadomości e-mail, takie jak Outlook i ActiveSync |  Obsługiwane |Brak|

Aby uzyskać więcej informacji o OneLogin, zobacz [OneLogin.](https://www.onelogin.com/)

## <a name="netiq-access-manager-401"></a>Menedżer dostępu NetIQ 4.0.1 
Menedżer dostępu NetIQ 4.0.1 wykonuje powszechnie używany standard tożsamości WS-WS-relacja zaufania federacji o podanie logowania jednokrotnego i strukturą atrybutów programu exchange.

Scenariusz diagram obsługi w przypadku pojedynczego obsługi tego logowania jednokrotnego jest następująca:

| Klienta |Pomoc techniczna  |Wyjątki|
| --------- | --------- |--------- |
| Klienci sieci Web, tacy jak dostępu do sieci Web programu Exchange i usługi SharePoint Online | Obsługiwane |* Obsługiwane umów Kerberos|
| Aplikacje klienckie sformatowany przykład programu Lync, subskrypcji pakietu Office, CRM |  Obsługiwane |Zintegrowane uwierzytelnianie systemu Windows nie jest obsługiwane.|
| Klientów bogate w wiadomości e-mail, takie jak Outlook i ActiveSync |  Obsługiwane |Brak|

* NetIQ obsługi uwierzytelniania Kerberos za pośrednictwem konfiguracji umowy Kerberos.  Aby uzyskać pomoc dotyczącą tej konfiguracji skontaktuj się z firmą NetIQ lub wyświetlić przewodnik konfiguracji. Aby uzyskać więcej informacji na temat Menedżera dostępu NetIQ, zobacz [Menedżera dostępu NetIQ.](https://www.netiq.com/documentation/netiqaccessmanager4/identityserverhelp/data/b12iqp0m.html)

## <a name="big-ip-with-access-policy-manager-big-ip-ver-113x--116x"></a>BIG IP przy użyciu Menedżera zasad dostępu BIG IP wersja 11.3 x — 11.6 
IP BIG za pomocą programu Access zasad Menedżera (APM) wersja BIG IP x 11,3 — 11.6 x wprowadza powszechnie używany standard tożsamości SAML o podanie obsługi rejestracji jednokrotnej i strukturą atrybutów programu exchange.

Scenariusz diagram obsługi w przypadku pojedynczego obsługi tego logowania jednokrotnego jest następująca: 

| Klienta |Pomoc techniczna  |Wyjątki|
| --------- | --------- |--------- |
| Klienci sieci Web, tacy jak dostępu do sieci Web programu Exchange i usługi SharePoint Online | Obsługiwane |Brak|
| Aplikacje klienckie sformatowany przykład programu Lync, subskrypcji pakietu Office, CRM |  Brak obsługi |Brak obsługi|
| Klientów bogate w wiadomości e-mail, takie jak Outlook i ActiveSync |  Obsługiwane |Brak|

Aby uzyskać więcej informacji na temat Menedżera zasady dostępu BIG IP, zobacz [Menedżera zasady dostępu BIG IP.](https://f5.com/products/modules/access-policy-manager) 

Aby uzyskać instrukcje dotyczące Menedżera zasady dostępu BIG IP dotyczące konfigurowania to STS zapewnienie jednego środowiska logowania jednokrotnego usługi Active Directory Users pobrać plik pdf [tutaj.](http://www.f5.com/pdf/deployment-guides/microsoft-office-365-idp-dg.pdf)

## <a name="vmware--workspace-portal-version-21"></a>Portalu obszaru roboczego VMware wersji 2.1 
Portalu obszaru roboczego VMware wersji 2.1 wykonuje powszechnie używany standard tożsamości WS-WS-relacja zaufania federacji o podanie logowania jednokrotnego i strukturą atrybutów programu exchange.

Scenariusz diagram obsługi w przypadku pojedynczego obsługi tego logowania jednokrotnego jest następująca:

| Klienta |Pomoc techniczna  |Wyjątki|
| --------- | --------- |--------- |
| Klienci sieci Web, tacy jak dostępu do sieci Web programu Exchange i usługi SharePoint Online | Obsługiwane |Zintegrowane uwierzytelnianie systemu Windows nie jest obsługiwane.|
| Aplikacje klienckie sformatowany przykład programu Lync, subskrypcji pakietu Office, CRM | Obsługiwane |Zintegrowane uwierzytelnianie systemu Windows nie jest obsługiwane.|
| Klientów bogate w wiadomości e-mail, takie jak Outlook i ActiveSync |  Obsługiwane |Brak|

Aby uzyskać więcej informacji na temat portalu obszaru roboczego VMware wersji 2.1 pobrać plik pdf [tutaj.](http://pubs.vmware.com/workspace-portal-21/topic/com.vmware.ICbase/PDF/workspace-portal-21-resource.pdf)

## <a name="signgo-53"></a>Zaloguj się i przejdź 5.3 
Zaloguj się i przejdź 5.3 narzędzi powszechnie używany WS-WS-relacja zaufania federacji tożsamości standardowe o podanie logowania jednokrotnego i strukturą atrybutów programu exchange.

Scenariusz diagram obsługi w przypadku pojedynczego obsługi tego logowania jednokrotnego jest następująca:

| Klienta |Pomoc techniczna  |Wyjątki|
| --------- | --------- |--------- |
| Klienci sieci Web, tacy jak dostępu do sieci Web programu Exchange i usługi SharePoint Online | Obsługiwane |Obsługiwane umów Kerberos |
| Aplikacje klienckie sformatowany przykład programu Lync, subskrypcji pakietu Office, CRM | Obsługiwane |Brak|
| Klientów sformatowanych poczty e-mail, takie jak Outlook i ActiveSync |  Obsługiwane |Brak|


Znak & Przejdź 5.3 obsługuje uwierzytelnianie Kerberos za pośrednictwem konfiguracji umowy Kerberos.  Aby uzyskać pomoc dotyczącą tej konfiguracji, skontaktuj się z Ilex lub wyświetlić przewodnik konfiguracji [tutaj.](http://www.ilex-international.com/docs/sign&go_wsfederation_en.pdf)


## <a name="icewall-federation-version-30"></a>Federacja IceWall w wersji 3.0 
Federacja IceWall w wersji 3.0 wprowadza powszechnie używany standard tożsamości WS-WS-relacja zaufania federacji o podanie logowania jednokrotnego i strukturą atrybutów programu exchange.

Scenariusz diagram obsługi w przypadku pojedynczego obsługi tego logowania jednokrotnego jest następująca:

| Klienta |Pomoc techniczna  |Wyjątki|
| --------- | --------- |--------- |
| Klienci sieci Web, tacy jak dostępu do sieci Web programu Exchange i usługi SharePoint Online | Obsługiwane |Zintegrowane uwierzytelnianie systemu Windows nie jest obsługiwane.|
| Aplikacje klienckie sformatowany przykład programu Lync, subskrypcji pakietu Office, CRM | Obsługiwane |Zintegrowane uwierzytelnianie systemu Windows nie jest obsługiwane.|
| Klientów bogate w wiadomości e-mail, takie jak Outlook i ActiveSync |  Obsługiwane |Brak|

Aby uzyskać więcej informacji na temat Federacji IceWall zobacz [poniżej](http://h50146.www5.hp.com/products/software/security/icewall/eng/federation/) i [tutaj.](http://h50146.www5.hp.com/products/software/security/icewall/federation/office365.html)

## <a name="ca-secure-cloud"></a>Chmura bezpiecznego urzędu certyfikacji 

Urząd certyfikacji bezpiecznego chmury wykonuje powszechnie używany standard tożsamości WS-WS-relacja zaufania federacji o podanie logowania jednokrotnego i strukturą atrybutów programu exchange.

Scenariusz diagram obsługi w przypadku pojedynczego obsługi tego logowania jednokrotnego jest następująca:

| Klienta |Pomoc techniczna  |Wyjątki|
| --------- | --------- |--------- |
| Klienci sieci Web, tacy jak dostępu do sieci Web programu Exchange i usługi SharePoint Online | Obsługiwane |Zintegrowane uwierzytelnianie systemu Windows nie jest obsługiwane.|
| Aplikacje klienckie sformatowany przykład programu Lync, subskrypcji pakietu Office, CRM | Obsługiwane |Zintegrowane uwierzytelnianie systemu Windows nie jest obsługiwane.|
| Klientów bogate w wiadomości e-mail, takie jak Outlook i ActiveSync |  Obsługiwane |Brak|

Aby uzyskać więcej informacji na temat chmury bezpiecznego urzędu certyfikacji, zobacz [urząd certyfikacji bezpiecznego chmury.](http://www.ca.com/us/products/security-as-a-service.aspx)

## <a name="dell-one-identity-cloud-access-manager-v71"></a>V7.1 Menedżera dostępu chmury tożsamości Dell z nich 
Menedżer dostępu chmury tożsamości Dell jeden wykonuje powszechnie używany standard tożsamości WS-WS-relacja zaufania federacji o podanie logowania jednokrotnego i strukturą atrybutów programu exchange.

Scenariusz diagram obsługi w przypadku pojedynczego obsługi tego logowania jednokrotnego jest następująca:

| Klienta |Pomoc techniczna  |Wyjątki|
| --------- | --------- |--------- |
| Klienci sieci Web, tacy jak dostępu do sieci Web programu Exchange i usługi SharePoint Online | Obsługiwane |Brak|
| Aplikacje klienckie sformatowany przykład programu Lync, subskrypcji pakietu Office, CRM |  Obsługiwane |Brak|
| Klientów bogate w wiadomości e-mail, takie jak Outlook i ActiveSync |  Obsługiwane |Brak|

Aby uzyskać więcej informacji o Dell jeden tożsamości chmury dostęp menedżera, zobacz [Dell jednego tożsamości chmury dostępu do menedżera.](http://software.dell.com/products/cloud-access-manager)

 Aby uzyskać instrukcje dotyczące konfigurowania tej usługi STS zapewnić pojedynczy obsługi logowania użytkowników usługi Office 365, zobacz [Konfigurowanie usługi Office 365 użytkowników.](http://documents.software.dell.com/dell-one-identity-cloud-access-manager/7.1/how-to-configure-microsoft-office-365) 

## <a name="authanvil-single-sign-on-45"></a>Zaloguj się pojedynczego AuthAnvil na 4,5 
AuthAnvil pojedynczy znak na 4,5 wykonuje powszechnie używany standard tożsamości WS-WS-relacja zaufania federacji o podanie logowania jednokrotnego i strukturą atrybutów programu exchange.

Scenariusz diagram obsługi w przypadku pojedynczego obsługi tego logowania jednokrotnego jest następująca:

| Klienta |Pomoc techniczna  |Wyjątki|
| --------- | --------- |--------- |
| Klienci sieci Web, tacy jak dostępu do sieci Web programu Exchange i usługi SharePoint Online | Obsługiwane |Zintegrowane uwierzytelnianie systemu Windows nie jest obsługiwane.|
| Aplikacje klienckie sformatowany przykład programu Lync, subskrypcji pakietu Office, CRM | Obsługiwane |Zintegrowane uwierzytelnianie systemu Windows nie jest obsługiwane.|
| Klientów bogate w wiadomości e-mail, takie jak Outlook i ActiveSync |  Obsługiwane |Brak|


Aby uzyskać więcej informacji, zobacz [AuthAnvil rejestracji jednokrotnej.](https://help.scorpionsoft.com/entries/26538603-How-can-I-Configure-Single-Sign-On-for-Office-365-)
