<properties
    pageTitle="Narzędzie Azure AD Connect: Rozwiązywanie problemów z łącznością | Microsoft Azure"
    description="W tym miejscu wyjaśniono, jak rozwiązywać problemy z łącznością z Azure AD Connect."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/27/2016"
    ms.author="billmath"/>

# <a name="troubleshoot-connectivity-issues-with-azure-ad-connect"></a>Rozwiązywanie problemów z łącznością z Azure AD Connect
W tym artykule wyjaśniono, jak działa łączność między Azure AD Connect i Azure AD oraz jak rozwiązywać problemy z łącznością. Te problemy są najczęściej są widoczne w środowisku z serwera proxy.

## <a name="troubleshoot-connectivity-issues-in-the-installation-wizard"></a>Rozwiązywanie problemów z łącznością w Kreatorze instalacji
Narzędzie Azure AD Connect za pomocą nowoczesny uwierzytelniania (za pomocą bibliotece ADAL) do uwierzytelniania. Kreator instalacji i pisane z wielkiej litery aparatu synchronizacji wymagają machine.config należy skonfigurować poprawnie, ponieważ są one aplikacji .NET.

W tym artykule pokazano, sposób łączenia Fabrikam Azure AD za pośrednictwem jego serwera proxy. Serwer proxy o nazwie fabrikamproxy i korzysta z portu 8080.

Najpierw trzeba upewnij się, że [**plik machine.config**](active-directory-aadconnect-prerequisites.md#connectivity) jest poprawnie skonfigurowany.  
![machineconfig](./media/active-directory-aadconnect-troubleshoot-connectivity/machineconfig.png)

>[AZURE.NOTE]
W niektórych blogi firmy Microsoft jest opisane że należy wprowadzić zmiany do miiserver.exe.config zamiast tego. Jednak ten plik jest zastępowany na wszystkie uaktualnienia, nawet jeśli sprawdza się podczas instalacji początkowej, system przestanie działać w czasie pierwszego uaktualniania. Z tego powodu zaleca się zamiast tego zaktualizować plik machine.config.

Serwer proxy musi mieć także odpowiednie adresy URL otwarty. Lista oficjalnym jest zawarta na [adresy URL usługi Office 365 i zakresy adresów IP ](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2).

Z powyższych poniższej tabeli jest bezwzględne minimalne czynności, aby można było połączyć się Azure AD na wszystkich. Ta lista nie zawiera funkcje opcjonalne, takich jak hasło zapisu lub Azure AD łączenie kondycji. Jest on opisanych tutaj, aby ułatwić rozwiązywanie problemów początkowej konfiguracji.

ADRES URL | Port | Opis
---- | ---- | ----
mscrl.microsoft.com | HTTP/80 | Umożliwia pobieranie listy CRL.
\*. verisign.com | HTTP/80 | Umożliwia pobieranie listy CRL.
\*. entrust.com | HTTP/80 | Umożliwia pobieranie listy CRL do uwierzytelniania MFA.
\*. windows.net | PROTOKÓŁ HTTPS I 443 | Używane do zalogowania się do Azure AD.
Secure.aadcdn.microsoftonline p.com | PROTOKÓŁ HTTPS I 443 | Używany do uwierzytelniania MFA.
\*. microsoftonline.com | PROTOKÓŁ HTTPS I 443 | Służy do konfigurowania katalogu Azure AD oraz import/eksport danych.

## <a name="errors-in-the-wizard"></a>Błędy w Kreatorze
Kreator instalacji korzysta z dwóch różnych kontekstach zabezpieczeń. Na stronie **Nawiązywanie połączenia z Azure AD** korzysta obecnie zalogowania użytkownika. Na stronie **Konfigurowanie** zmienia się do [konta usługi dla aparatu synchronizacji](active-directory-aadconnect-accounts-permissions.md#azure-ad-connect-sync-service-accounts). Konfiguracji serwera proxy udzielamy są globalny na tym komputerze, dlatego w przypadku problemu, ten problem zostanie większość prawdopodobnie już na stronie **Nawiązywanie połączenia z Azure AD** w kreatorze.

Są to najbardziej typowe błędy, które będą wyświetlane w Kreatorze instalacji.

### <a name="the-installation-wizard-has-not-been-correctly-configured"></a>Kreator instalacji nie został poprawnie skonfigurowany
Ten błąd będzie wyglądała Kreator nie może się połączyć serwer proxy.
![nomachineconfig](./media/active-directory-aadconnect-troubleshoot-connectivity/nomachineconfig.png)

- Jeśli zostanie wyświetlony, sprawdź, czy [plik machine.config](active-directory-aadconnect-prerequisites.md#connectivity) został poprawnie skonfigurowany.
- Jeśli który wygląda poprawnie, postępuj zgodnie z instrukcjami w usłudze [łączności serwera proxy Sprawdź](#verify-proxy-connectivity) Aby sprawdzić, czy problem znajduje się poza także kreatora.

### <a name="the-mfa-endpoint-cannot-be-reached"></a>Punkt końcowy MFA nie można się z Tobą skontaktować
Ten błąd pojawia się, gdy punkt końcowy **https://secure.aadcdn.microsoftonline-p.com** nie można się z Tobą i usługi Administrator globalny ma MFA włączone.  
![nomachineconfig](./media/active-directory-aadconnect-troubleshoot-connectivity/nomicrosoftonlinep.png)

- Jeśli zostanie wyświetlony, upewnij się, że punkt końcowy secure.aadcdn.microsoftonline-p.com został dodany do serwera proxy.

### <a name="the-password-cannot-be-verified"></a>Nie można sprawdzić hasła
Jeśli Kreator instalacji zostanie przeprowadzone pomyślnie podczas łączenia się Azure AD, ale samego hasła nie może zostać zweryfikowane, zobaczysz to: ![badpassword](./media/active-directory-aadconnect-troubleshoot-connectivity/badpassword.png)

- Jest to hasło tymczasowe hasło i musi zostać zmieniony? Jest to faktycznie poprawne hasło? Spróbuj zalogować się do https://login.microsoftonline.com (na innym komputerze niż serwer Azure AD Connect) i sprawdź, czy konto jest użyteczne.

### <a name="verify-proxy-connectivity"></a>Weryfikowanie łączności serwera proxy
Aby sprawdzić, czy serwer Azure AD Connect rzeczywisty łączność z serwera Proxy i Internet użyjemy niektórych programu PowerShell użytkownikom, jeśli serwer proxy zezwala żądania sieci web lub nie. W wierszu polecenia programu PowerShell Uruchom `Invoke-WebRequest -Uri https://adminwebservice.microsoftonline.com/ProvisioningService.svc`. (Technicznego pierwszego połączenia jest https://login.microsoftonline.com i to działa także, ale inne identyfikator URI jest szybciej reagować.)

PowerShell przy użyciu konfiguracji w pliku machine.config skontaktuj się z serwera proxy. Ustawienia w winhttp-netsh nie powinna wpływu tych poleceń cmdlet.

Jeśli serwer proxy jest poprawnie skonfigurowany, otrzymasz stanu sukcesu: ![proxy200](./media/active-directory-aadconnect-troubleshoot-connectivity/invokewebrequest200.png)

Jeśli zostanie wyświetlony **nie można połączyć się z serwerem zdalnym** następnie programu PowerShell próbuje nawiązać połączenie bezpośrednie bez użycia serwera proxy lub DNS nie jest poprawnie skonfigurowany. Upewnij się, że plik **machine.config** jest poprawnie skonfigurowany.
![unabletoconnect](./media/active-directory-aadconnect-troubleshoot-connectivity/invokewebrequestunable.png)

Jeśli serwer proxy nie jest poprawnie skonfigurowany, firma Microsoft będzie wyświetlany komunikat o błędzie: ![proxy200](./media/active-directory-aadconnect-troubleshoot-connectivity/invokewebrequest403.png)
![proxy407](./media/active-directory-aadconnect-troubleshoot-connectivity/invokewebrequest407.png)

Błąd | Tekst błędu | Komentarz
---- | ---- | ---- |
403 | Dostęp zabroniony | Serwer proxy nie został otwarty żądanego adresu URL. Odwiedzaj konfiguracji serwera proxy i upewnij się, [adresy URL](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2) zostały otwarte.
407 | Wymagane jest uwierzytelnianie serwera proxy | Serwer proxy wymagane logowanie i brak podano. Jeśli serwer proxy wymaga uwierzytelniania, pamiętaj o tej skonfigurowane w machine.config. Upewnij się, że używasz konta domeny dla użytkownika Kreatora oraz jak w przypadku konta usługi również.

## <a name="the-communication-pattern-between-azure-ad-connect-and-azure-ad"></a>Wzór komunikacji między Azure AD Connect i Azure AD
Jeśli zostały wykonane wszystkie te kroki powyżej i nadal nie można połączyć może w tym momencie rozpocząć przeglądająca dzienniki sieci. W tej sekcji jest dokumentowanie deseń łączności normalnym i powiedzie. Wyświetlanie jest również listy typowych śledzi czerwonego, które można je zignorować podczas czytania dzienniki sieci.

- Będzie połączenia do https://dc.services.visualstudio.com. Nie jest wymagane do otwartych na serwerze proxy dla instalacji powiodła się i tych można je zignorować.
- Zobaczysz, że rozpoznawania nazw dns znajduje się lista rzeczywisty hosts, aby być w nsatc.net obszar nazw DNS i innych obszarach nazw nie w obszarze microsoftonline.com. Jednak nie będzie żądań usługi sieci web na nazwy serwerów rzeczywiste i nie trzeba dodawać do serwera proxy.
- Punkty końcowe adminwebservice i provisioningapi (patrz poniżej w dziennikach) są punkty końcowe odnajdowania i umożliwia znalezienie rzeczywisty punkt końcowy do użyć i będzie różnił się w zależności od regionu.

### <a name="reference-proxy-logs"></a>Dzienniki serwera proxy odwołania
Poniżej przedstawiono zrzut z dziennika rzeczywisty serwera proxy i strona kreatora instalacji, z której pochodzi (zduplikowane pozycje samego punktu końcowego zostały usunięte). To może służyć jako odwołanie własne dzienników serwera proxy i sieci. Rzeczywista punkty końcowe mogą się różnić w środowisku (w szczególności znajdującymi się w *Kursywa*).

**Nawiązywanie połączenia z Azure AD**

Czas | ADRES URL
--- | ---
2016-1-11 8:31 | Connect://login.microsoftonline.com:443
2016-1-11 8:31 | Connect://adminwebservice.microsoftonline.com:443
2016-1-11 8:32 | łączenie: / /*bba800 kotwicy*. microsoftonline.com:443
2016-1-11 8:32 | Connect://login.microsoftonline.com:443
2016-1-11 8:33 | Connect://provisioningapi.microsoftonline.com:443
2016-1-11 8:33 | łączenie: / /*bwsc02 przekazywania*. microsoftonline.com:443

**Konfigurowanie**

Czas | ADRES URL
--- | ---
2016-1-11 8:43 | Connect://login.microsoftonline.com:443
2016-1-11 8:43 | łączenie: / /*bba800 kotwicy*. microsoftonline.com:443
2016-1-11 8:43 | Connect://login.microsoftonline.com:443
2016-1-11 8:44 | Connect://adminwebservice.microsoftonline.com:443
2016-1-11 8:44 | łączenie: / /*bba900 kotwicy*. microsoftonline.com:443
2016-1-11 8:44 | Connect://login.microsoftonline.com:443
2016-1-11 8:44 | Connect://adminwebservice.microsoftonline.com:443
2016-1-11 8:44 | łączenie: / /*bba800 kotwica*. microsoftonline.com:443
2016-1-11 8:44 | Connect://login.microsoftonline.com:443
2016-1-11 8:46 | Connect://provisioningapi.microsoftonline.com:443
2016-1-11 8:46 | łączenie: / /*bwsc02 przekazywania*. microsoftonline.com:443

**Początkowej synchronizacji**

Czas | ADRES URL
--- | ---
2016-1-11 8:48 | Connect://login.Windows.NET:443
2016-1-11 8:49 | Connect://adminwebservice.microsoftonline.com:443
2016-1-11 8:49 | łączenie: / /*bba900 kotwicy*. microsoftonline.com:443
2016-1-11 8:49 | łączenie: / /*bba800 kotwicy*. microsoftonline.com:443

## <a name="authentication-errors"></a>Błędy uwierzytelniania
W tej sekcji opisano błędy, które mogą zostać zwrócone z ADAL (Biblioteka uwierzytelniania używane przez narzędzie Azure AD Connect) i programu PowerShell. Komunikat o błędzie omówienie powinna ułatwić Ci w zrozumienie dalsze kroki.

### <a name="invalid-grant"></a>Udzielanie nieprawidłowe
Nieprawidłowa nazwa użytkownika lub hasło. Aby uzyskać więcej informacji, zobacz [nie można zweryfikować hasła](#the-password-cannot-be-verified) .

### <a name="unknown-user-type"></a>Typ Nieznany użytkownik
Katalogu Azure AD nie może znaleźć lub rozpoznać. Być może próbujesz zalogować się przy użyciu nazwy użytkownika w domenie niezweryfikowany?

### <a name="user-realm-discovery-failed"></a>Odnajdowanie obszaru użytkownika nie powiodła się
Problemy z konfiguracją sieci lub serwera proxy. Sieć nie może być osiągnięte, zobacz [Rozwiązywanie problemów z łącznością w Kreatorze instalacji](#troubleshoot-connectivity-issues-in-the-installation-wizard).

### <a name="user-password-expired"></a>Hasło użytkownika wygasła
Poświadczenia wygasły. Zmień hasło.

### <a name="authorizationfailure"></a>AuthorizationFailure
Nieznany problem.

### <a name="authentication-cancelled"></a>Uwierzytelnianie anulowana
Największym wyzwaniem uwierzytelnianie wieloskładnikowe (MFA) zostało anulowane.

### <a name="connecttomsonline"></a>ConnectToMSOnline
Uwierzytelnianie zakończyło się powodzeniem, ale Azure AD programu PowerShell wystąpił problem uwierzytelniania.

### <a name="azurerolemissing"></a>AzureRoleMissing
Uwierzytelnianie zakończyło się pomyślnie. Nie jesteś administratorem globalnym.

### <a name="privilegedidentitymanagement"></a>PrivilegedIdentityManagement
Uwierzytelnianie zakończyło się pomyślnie. Zarządzanie tożsamościami uprzywilejowanych został włączony i obecnie nie jesteś administratorem globalnym. Aby uzyskać więcej informacji, zobacz [Zarządzanie tożsamościami uprzywilejowanych](active-directory-privileged-identity-management-getting-started.md) .

### <a name="companyinfounavailable"></a>CompanyInfoUnavailable
Uwierzytelnianie zakończyło się pomyślnie. Nie można pobrać informacji o firmie z usługi Azure Active Directory.

### <a name="retrievedomains"></a>RetrieveDomains
Uwierzytelnianie zakończyło się pomyślnie. Nie można pobrać informacji domeny z usługi Azure Active Directory.

## <a name="troubleshooting-steps-for-previous-releases"></a>Procedura rozwiązywania problemów z poprzednimi wersjami.
W wersjach począwszy od numeru kompilacji 1.1.105.0 (wydane 2016 luty) została wycofana Asystenta logowania w witrynie. W tej sekcji i konfiguracji nie może być wymagane, ale są przechowywane jako odwołania.

Aby logowania jednokrotnego w Asystencie do pracy należy skonfigurować winhttp. Można to zrobić przy użyciu [**netsh**](active-directory-aadconnect-prerequisites.md#connectivity).  
![netsh](./media/active-directory-aadconnect-troubleshoot-connectivity/netsh.png)

### <a name="the-sign-in-assistant-has-not-been-correctly-configured"></a>Nie został poprawnie skonfigurowany Asystenta logowania w witrynie
Ten błąd widoczne po Asystenta logowania w witrynie nie może się połączyć serwera proxy lub serwera proxy nie zezwala na żądanie.
![nonetsh](./media/active-directory-aadconnect-troubleshoot-connectivity/nonetsh.png)

- Jeśli zostanie wyświetlony, obejrzyj konfiguracji serwera proxy w [netsh](active-directory-aadconnect-prerequisites.md#connectivity) i upewnij się, że jest poprawny.
![netshshow](./media/active-directory-aadconnect-troubleshoot-connectivity/netshshow.png)
- Jeśli który wygląda poprawnie, postępuj zgodnie z instrukcjami w usłudze [łączności serwera proxy Weryfikuj](#verify-proxy-connectivity) Aby sprawdzić, czy problem znajduje się poza także kreatora.

## <a name="next-steps"></a>Następne kroki
Dowiedz się więcej o [integrowaniu lokalnego tożsamości z usługą Azure Active Directory](active-directory-aadconnect.md).
