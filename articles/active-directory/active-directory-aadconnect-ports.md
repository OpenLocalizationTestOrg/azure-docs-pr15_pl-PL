<properties
    pageTitle="Azure AD Connect: Porty | Microsoft Azure"
    description="Ta strona jest stroną techniczne dla portów, które muszą być otwarte dla Azure AD Connect"
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
    ms.date="08/25/2016"
    ms.author="billmath"/>

# <a name="hybrid-identity-required-ports-and-protocols"></a>Tożsamość hybrydowych wymagane porty i protokoły
Następujący dokument jest techniczne przekazywania informacji o wymagane porty i protokoły, które są wymagane do wdrożenia hybrydowego rozwiązania tożsamości. Użyj poniższej ilustracji i odwołują się do odpowiedniej tabeli.

![Co to jest Azure AD Connect](./media/active-directory-aadconnect-ports/required1.png)

## <a name="table-1---azure-ad-connect-and-on-premises-ad"></a>Tabela 1 — Azure AD łączenie i lokalnych AD
W poniższej tabeli opisano porty i protokoły, które są wymagane na potrzeby komunikacji między serwer Azure AD Connect i AD lokalnego.

Protocol (protokół) | Porty | Opis
--------- | --------- |---------
DNS|53 (PORT TCP/UDP)| Wyszukiwanie DNS las docelowy.
Protokół Kerberos|88 (PORT TCP/UDP)| Uwierzytelnianie Kerberos las AD.
MS-RPC |135 (PORT TCP/UDP)| Używany podczas początkowej konfiguracji kreatora Azure AD Connect podczas nawiązywania połączenia las AD.
LDAP|389 (PORT TCP/UDP)| Na potrzeby importowania danych z usługi Active Directory. Dane są szyfrowane przy użyciu protokołu Kerberos logowania i zamknięcia.
LDAP/SSL|636 (PORT TCP/UDP)| Na potrzeby importowania danych z usługi Active Directory. Transfer danych jest zalogowany i szyfrowane. Używane, jeśli korzystasz z protokołu SSL.
RPC |Od od 49152 do 65535 (losowe wysoka RPC Port)(TCP/UDP)| Używany podczas początkowej konfiguracji Azure AD Connect podczas nawiązywania połączenia lasów AD. Aby uzyskać więcej informacji, zobacz [KB929851](https://support.microsoft.com/kb/929851), [KB832017](https://support.microsoft.com/kb/832017)i [KB224196](https://support.microsoft.com/kb/224196) .

## <a name="table-2---azure-ad-connect-and-azure-ad"></a>Łączenie tabeli 2 - Azure AD i Azure AD
W poniższej tabeli opisano porty i protokoły, które są wymagane na potrzeby komunikacji między komputerami serwerów Azure AD Connect i Azure AD.

Protocol (protokół) |Porty |Opis
--------- | --------- |---------
HTTP|80 (PORT TCP/UDP)| Służy do pobierania list odwołań certyfikatów (Certificate Revocation Lists) do weryfikacji certyfikatów SSL.
PROTOKÓŁ HTTPS|443(TCP/UDP)| Używane do synchronizacji z usługą Azure Active Directory.

Aby uzyskać listę adresów URL i IP adresy, musisz otworzyć w zaporze, zobacz [adresy URL usługi Office 365 i zakresy adresów IP](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2).

## <a name="table-3---azure-ad-connect-and-federation-serverswap"></a>Łączenie tabeli 3 — Azure AD i federacyjnych serwerów WAP
W poniższej tabeli opisano porty i protokoły, które są wymagane na potrzeby komunikacji między serwer Azure AD Connect i serwerów federacyjnych-WAP.  

Protocol (protokół) |Porty |Opis
--------- | --------- |---------
HTTP|80 (PORT TCP/UDP)| Służy do pobierania list odwołań certyfikatów (Certificate Revocation Lists) do weryfikacji certyfikatów SSL.
PROTOKÓŁ HTTPS|443(TCP/UDP)| Używane do synchronizacji z usługą Azure Active Directory.
WinRM|5985| Odbiornik usługi WinRM

## <a name="table-4---wap-and-federation-servers"></a>Tabela 4 - WAP i serwerów federacyjnych
W poniższej tabeli opisano porty i protokoły, które są wymagane na potrzeby komunikacji między serwerami federacyjnymi i serwery WAP.

Protocol (protokół) |Porty |Opis
--------- | --------- |---------
PROTOKÓŁ HTTPS|443(TCP/UDP)| Używany do uwierzytelniania.

## <a name="table-5---wap-and-users"></a>Tabela 5 - WAP i użytkowników
W poniższej tabeli opisano porty i protokoły, które są wymagane na potrzeby komunikacji między użytkownikami a serwerami WAP.

Protocol (protokół) |Porty |Opis
--------- | --------- |--------- |
PROTOKÓŁ HTTPS|443(TCP/UDP)| Używany do uwierzytelniania urządzenia.
PORT TCP|49443 (TCP)| Używany do uwierzytelniania certyfikatu.

## <a name="table-6a--6b---azure-ad-connect-health-agent-for-ad-fssync-and-azure-ad"></a>Tabela 6a i 6b - agent Azure AD łączenie kondycji dla (AD FS i synchronizacja) i Azure AD
W poniższych tabelach opisano punkty końcowe, porty i protokoły, które są wymagane na potrzeby komunikacji między agentami Azure AD łączenie kondycji i Azure AD

### <a name="table-6a---ports-and-protocols-for-azure-ad-connect-health-agent-for-ad-fssync-and-azure-ad"></a>Tabela 6a - porty i protokoły dla agenta Azure AD łączenie kondycji dla (AD FS i synchronizacja) i Azure AD
W poniższej tabeli opisano następujące ruchu wychodzącego porty i protokoły, które są wymagane na potrzeby komunikacji między agentami Azure AD łączenie kondycji i Azure AD.  

Protocol (protokół) |Porty  |Opis
--------- | --------- |--------- |
PROTOKÓŁ HTTPS|443(TCP/UDP)| Wychodzące
Usługa Azure Bus|5671 (PORT TCP/UDP)| Wychodzące

### <a name="6b---endpoints-for-azure-ad-connect-health-agent-for-ad-fssync-and-azure-ad"></a>6b - punkty końcowe dla agenta Azure AD kondycji połączenia do (AD FS i synchronizacja) i Azure AD
Aby uzyskać listę punktów końcowych zobacz [sekcję wymagania dotyczące agenta Azure AD łączenie kondycji](active-directory-aadconnect-health-agent-install.md#requirements).
