<properties
    pageTitle="Rejestracji jednokrotnej z serwera Proxy aplikacji | Microsoft Azure"
    description="Opisano, jak o podanie rejestracji jednokrotnej przy użyciu serwera Proxy aplikacji Azure AD."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="kgremban"/>


# <a name="single-sign-on-with-application-proxy"></a>Rejestracji jednokrotnej z serwera Proxy aplikacji

Rejestracji jednokrotnej jest kluczowym elementem Azure AD serwera Proxy aplikacji. Najlepszego środowiska pracy użytkownika zapewnia następujące czynności:

1. Użytkownik rejestruje w chmurze  
2. Sprawdzanie poprawności zabezpieczeń wszystkich pojawiają się w chmurze (było wstępnie uwierzytelnić)  
3. Gdy żądania są wysyłane do aplikacji lokalna, łącznik serwera Proxy aplikacji personifikuje użytkownika. Wewnętrznej bazy danych aplikacji uznaje, że jest to zwykły użytkownik pochodzące z urządzeniem domeny.

![Diagram programu Access z użytkownika końcowego za pośrednictwem serwera Proxy aplikacji, do sieci firmowej](./media/active-directory-application-proxy-sso-using-kcd/app_proxy_sso_diff_id_diagram.png)

Serwer Proxy aplikacji Azure AD pomaga zapewnić obsługi logowania jednokrotnego (SSO) dla użytkowników. Publikowanie aplikacji za pomocą logowania jednokrotnego, wykonaj następujące instrukcje:


## <a name="sso-for-on-prem-iwa-apps-using-kcd-with-application-proxy"></a>Rejestracji Jednokrotnej dla lokalna Zintegrowane aplikacje KCD za pomocą serwera Proxy aplikacji
Możesz włączyć logowania jednokrotnego aplikacji przy użyciu zintegrowanego uwierzytelniania systemu Windows (Zintegrowane) przez nadanie uprawnień łączników serwera Proxy aplikacji w usłudze Active Directory personifikacji użytkowników oraz wysyłanie i odbieranie tokeny w imieniu tej osoby.


### <a name="network-diagram"></a>Diagram sieciowy

Ten schemat wyjaśniono przepływ, gdy użytkownik próbuje uzyskać dostęp do aplikacji lokalna, która używa Zintegrowane.

![Diagram przepływu uwierzytelniania AAD firmy Microsoft](./media/active-directory-application-proxy-sso-using-kcd/AuthDiagram.png)

1. Użytkownik wprowadza adres URL umożliwiający dostęp do aplikacji lokalna za pośrednictwem serwera Proxy aplikacji.
2. Serwer Proxy aplikacji przekierowywane do usług uwierzytelniania Azure AD do preauthenticate. W tym momencie Azure AD stosuje wszelkie stosowne uwierzytelnianie i zasady autoryzacji, takich jak uwierzytelnianie wieloskładnikowe. Jeśli uwierzytelnieniu użytkownika Azure AD tworzy tokenu i wysyła go do użytkownika.
3. Użytkownik przekazuje token do serwera Proxy aplikacji.
4. Serwer Proxy aplikacji sprawdza tokenu pobiera użytkownika nazwy główne (UPN) z niego i przesyła żądanie, UPN i głównej nazwy usługi (SPN) do łącznika za pośrednictwem dually uwierzytelniony bezpiecznego kanału.
5. Łącznik wykonuje Kerberos ograniczone delegowanie (KCD) wymiany informacji z lokalna AD, użytkownik uzyskanie token Kerberos do aplikacji.
6. Usługi Active Directory wysyła tokenu Kerberos dla aplikacji do łącznika.
7. Łącznik wysyła oryginalne wezwanie na serwerze aplikacji przy użyciu tokenu Kerberos otrzymania z usługi Active Directory.
8. Aplikacji odsyła odpowiedź do łącznika, który jest zwracany do serwera Proxy aplikacji usługi i w końcu do użytkownika.

### <a name="prerequisites"></a>Wymagania wstępne

Przed rozpoczęciem rejestracji Jednokrotnej dla serwera Proxy aplikacji, upewnij się, że środowisko jest gotowa z następujących ustawień i konfiguracji:

- Aplikacji, takich jak aplikacje sieci Web programu SharePoint są ustawione na zintegrowanego uwierzytelniania systemu Windows. Aby uzyskać więcej informacji, zobacz [Włączanie obsługi uwierzytelniania Kerberos](https://technet.microsoft.com/library/dd759186.aspx), lub dla programu SharePoint, zobacz [Planowanie uwierzytelniania Kerberos w programie SharePoint 2013](https://technet.microsoft.com/library/ee806870.aspx).

- Twoje aplikacje mają główne nazwy usług.

- Łącznik serwerem i serwerem aplikacji są domeny sprzężone i część tej samej domeny lub ufanie domen. Aby uzyskać więcej informacji dotyczących dołączania do domeny Zobacz [Dołączanie do komputera do domeny](https://technet.microsoft.com/library/dd807102.aspx).

- Serwera z programem łącznik ma dostęp do odczytu TokenGroupsGlobalAndUniversal dla użytkowników. To ustawienie domyślne, które mogą być wpływ środowiska Wzmacnianie zabezpieczeń. Uzyskiwanie pomocy na temat to w [KB2009157](https://support.microsoft.com/en-us/kb/2009157).

### <a name="active-directory-configuration"></a>Konfiguracja usługi Active Directory

Konfiguracja usługi Active Directory są różne w zależności od tego, czy wybrany łącznik serwera Proxy aplikacji i opublikowanych na serwerze znajdują się w tej samej domeny lub nie.

#### <a name="connector-and-published-server-in-the-same-domain"></a>Łącznik i opublikowanych na serwerze w tej samej domeny

1. W usłudze Active Directory, przejdź do pozycji **Narzędzia** > **użytkowników i komputerów**.
2. Zaznacz łącznik serwerem.
3. Kliknij prawym przyciskiem myszy i wybierz polecenie **Właściwości** > **Delegowanie**.
4. Wybierz pozycję **Ufaj temu komputerowi w delegowanie tylko do określonych usług** , a następnie w obszarze **usług, do których to konto może przedstawiać delegowane poświadczenia**, Dodaj wartość dla tożsamości SPN serwera aplikacji.
5. Dzięki temu łącznika serwera Proxy aplikacji w celu personifikacji użytkowników w AD przed aplikacji określone na liście.

![Zrzut ekranu okna właściwości SVR łącznika](./media/active-directory-application-proxy-sso-using-kcd/Properties.jpg)

#### <a name="connector-and-published-server-in-different-domains"></a>Łącznik i opublikowanych na serwerze w różnych domenach

1. Aby uzyskać listę wymagania wstępne dotyczące pracy z KCD domen zobacz [Kerberos ograniczone delegowanie domen](https://technet.microsoft.com/library/hh831477.aspx).
2. W systemie Windows 2012 R2, za pomocą `principalsallowedtodelegateto` właściwości na serwerze łącznik, aby włączyć serwer Proxy aplikacji do pełnomocnika dla serwera łącznika, gdzie jest opublikowanych na serwerze `sharepointserviceaccount` i delegujące serwer jest `connectormachineaccount`.

        $connector= Get-ADComputer -Identity connectormachineaccount -server dc.connectordomain.com

        Set-ADComputer -Identity sharepointserviceaccount -PrincipalsAllowedToDelegateToAccount $connector

        Get-ADComputer sharepointserviceaccount -Properties PrincipalsAllowedToDelegateToAccount


>[AZURE.NOTE] `sharepointserviceaccount`może być SPS konto lub konto usługi, na którym jest uruchomiony SPS puli aplikacji.


### <a name="azure-classic-portal-configuration"></a>Azure klasyczny Konfiguracja portalu

1. Publikowanie aplikacji zgodnie z instrukcjami opisanych w [aplikacjach Publikuj z serwera Proxy aplikacji](active-directory-application-proxy-publish.md). Upewnij się wybrać **Usługi Azure Active Directory** jako **Metody było wstępnie uwierzytelnić**.
2. Gdy aplikacja pojawi się na liście aplikacji, zaznacz go i kliknij przycisk **Konfiguruj**.
3. W obszarze **Właściwości**ustaw **Wewnętrznych metody uwierzytelniania** **Zintegrowanego uwierzytelniania systemu Windows**.  
  ![Konfigurowanie zaawansowanych aplikacji](./media/active-directory-application-proxy-sso-using-kcd/cwap_auth2.png)  
4. Wprowadź **Wewnętrznych SPN aplikacji** serwera aplikacji. W tym przykładzie SPN dla naszych opublikowanej aplikacji jest http/lob.contoso.com.  

>[AZURE.IMPORTANT] Sieci lokalnej głównej i głównej nazwy użytkownika w usłudze Azure Active Directory nie są identyczne, należy skonfigurować [delegowane tożsamość logowania](#delegated-login-identity) , aby było wstępnie uwierzytelnić do pracy.

|  |  |
| --- | --- |
| Metody uwierzytelniania wewnętrznych | Jeśli używasz Azure AD dla było wstępnie uwierzytelnić, można ustawić metody uwierzytelniania wewnętrznych, aby umożliwić użytkownikom idzie, korzystanie z logowania jednokrotnego (SSO) do tej aplikacji. <br><br> Zaznacz **Zintegrowane uwierzytelnianie systemu Windows** (Zintegrowane), jeśli aplikacja używa Zintegrowane i można skonfigurować Kerberos ograniczone delegowanie (KCD) umożliwiające rejestracji Jednokrotnej dla tej aplikacji. Aplikacje, które używają Zintegrowane musi być skonfigurowany za pomocą KCD, w przeciwnym razie serwer Proxy aplikacji nie będą mogli publikować te aplikacje. <br><br> Wybierz pozycję **Brak** , jeśli aplikacji nie jest używane Zintegrowane. |
| Aplikacją wewnętrzną SPN | Jest to nazw kapitału usług (SPN) stosowania wewnętrznych, zgodnie z konfiguracją w lokalna Azure AD. Nazwa SPN jest używana przez łącznik serwera Proxy aplikacji do pobierania Kerberos tokenów dla aplikacji przy użyciu KCD. |


## <a name="sso-for-non-windows-apps"></a>Rejestracji Jednokrotnej dla aplikacji — Windows
Przepływ delegowanie Kerberos w Proxy aplikacji usług Azure AD uruchamianego po Azure AD uwierzytelnia użytkownika w chmurze. Po żądaniu przychodzi lokalnego Azure AD łącznik serwera Proxy aplikacji problemów bilet Kerberos w imieniu użytkownika przez interakcja z lokalną usługą Active Directory. Ten proces jest nazywany jako Kerberos ograniczone delegowanie (KCD). W fazie następnego żądania są wysyłane do wewnętrznej bazy danych aplikacji Ten bilet Kerberos. Istnieje szereg protokołów definiowanie sposobu wysyłania wniosków. Większość serwerów — Windows przewiduje Negotiate-SPNego teraz obsługiwaną na Azure AD serwera Proxy aplikacji.

![Diagram logowania jednokrotnego spoza systemu Windows](./media/active-directory-application-proxy-sso-using-kcd/app_proxy_sso_nonwindows_diagram.png)

### <a name="delegated-login-identity"></a>Tożsamość delegowanej logowania
Tożsamość logowania delegowanej pomoże Ci obsługiwać dwa scenariusze różnych logowania:

- Aplikacje spoza systemu Windows, które zwykle uzyskać tożsamość użytkownika w postaci nazwy użytkownika i nazwa konta SAM nie adres e-mail (username@domain).
- Konfiguracje login alternatywny miejsce, w którym UPN w Azure AD i głównej nazwy użytkownika w usłudze Active Directory w lokalnej są różne.

Z serwera Proxy aplikacji możesz wybrać tożsamość umożliwia uzyskanie bilet Kerberos. To ustawienie jest każdej aplikacji. Niektóre z tych opcji są odpowiednie dla systemów, które nie akceptują format adresu e-mail, inne osoby są przeznaczone dla login alternatywny.

![Zrzut ekranu parametr tożsamości delegowanych logowania](./media/active-directory-application-proxy-sso-using-kcd/app_proxy_sso_diff_id_upn.png)

Użycie tożsamość logowania delegowanej wartość nie muszą być unikatowe dla wszystkich domen lub lasów w Twojej organizacji. Aby uniknąć tego problemu, należy publikowania te aplikacje dwa razy za pomocą dwóch różnych grup łącznik. Ponieważ każda aplikacja ma dla odbiorców innego użytkownika, jego łączniki można dołączyć do innej domeny.


## <a name="working-with-sso-when-on-premises-and-cloud-identities-are-not-identical"></a>Praca z logowania jednokrotnego podczas lokalnego i chmura tożsamości nie są identyczne.
Chyba że skonfigurowane w inny sposób serwera Proxy aplikacji założono, że użytkownicy mają taką samą tożsamość, w chmurze i lokalnego. Można skonfigurować, dla każdej aplikacji tożsamość powinien być używany podczas wykonywania rejestracji jednokrotnej.  

Ta funkcja umożliwia wiele organizacji, które mają różne tożsamości lokalna i chmura konieczności logowania jednokrotnego z chmury lokalna aplikacji bez konieczności użytkownikom wprowadzanie różnych nazwy użytkowników i hasła. To dotyczy organizacji, która:

- Masz wiele domen wewnętrznie (joe@us.contoso.com, joe@eu.contoso.com) i pojedynczej domeny w chmurze (joe@contoso.com).

- Masz nazwy domeny bez obsługi routingu wewnętrznie (joe@contoso.usa) i prawne w chmurze.

- Nie należy używać nazwy domeny wewnętrznie (joe)

- Używanie różnych aliasów lokalna i w chmurze. Np. joe-johns@contoso.comw porównaniu z.joej@contoso.com  

Pomoże również z aplikacjami, które nie akceptuj w formularzu adresu e-mail, który jest bardzo typowy scenariusz dla serwerów — Windows wewnętrznej bazy danych.


### <a name="setting-sso-for-different-cloud-and-on-prem-identities"></a>Ustawianie rejestracji Jednokrotnej dla różnych tożsamości w chmurze i lokalna

1. Konfigurowanie ustawień Azure AD Connect tożsamość główna będzie adres e-mail (poczta). Jest to w ramach procesu Dostosuj, zmieniając pola **Nazwa zasady użytkownika** w obszarze Ustawienia synchronizacji. Uwaga Aby te ustawienia także określić, jak użytkownicy zalogować się do usługi Office 365, urządzeń Windows10 i inne aplikacje, które używają Azure AD jako ich Magazyn tożsamości.  
  ![Identyfikowanie użytkowników zrzut ekranu — Lista rozwijana głównej nazwy użytkownika](./media/active-directory-application-proxy-sso-using-kcd/app_proxy_sso_diff_id_connect_settings.png)  
2. W obszarze Ustawienia konfiguracji aplikacji dla aplikacji, którą chcesz zmodyfikować wybierz **Delegowane tożsamość logowania** może być używany:
  - Główna nazwa użytkownika:joe@contoso.com  
  - Alternatywny główna nazwa użytkownika:joed@contoso.local  
  - Nazwa użytkownika część główna nazwa użytkownika: joe  
  - Część nazwy użytkownika alternatywny główna nazwa użytkownika: joed  
  - Nazwa konta SAM lokalnego: zależności od konfiguracji kontrolera domeny lokalna

  ![Zrzut ekranu menu rozwijane tożsamości delegowanej logowania](./media/active-directory-application-proxy-sso-using-kcd/app_proxy_sso_diff_id_upn.png)  

### <a name="troubleshooting-sso-for-different-identities"></a>Rozwiązywanie problemów dotyczących rejestracji Jednokrotnej dla różnych tożsamości
Jeśli występuje błąd w procesie logowania jednokrotnego pojawi się w dzienniku zdarzeń komputera łącznik zgodnie z opisem w [temacie Rozwiązywanie problemów](active-directory-application-proxy-troubleshoot.md).
Ale w niektórych przypadkach żądanie zostanie pomyślnie wysłane do wewnętrznej bazy danych aplikacji podczas ta aplikacja będzie odpowiadał w różnych odpowiedzi HTTP. Rozwiązywanie problemów z tych przypadkach powinna rozpoczynać się sprawdzając numer zdarzenia 24029 na komputerze łącznika w dzienniku zdarzeń sesji serwera Proxy aplikacji. Tożsamość użytkownika, którego użyto do delegowania pojawi się w polu "użytkownika" w szczegóły zdarzenia. Aby włączyć dziennik sesji, wybierz **Wyświetlanie analityczny dzienników i debugowania** w menu Widok Podgląd zdarzeń.


## <a name="see-also"></a>Zobacz też

- [Publikowanie aplikacji za pomocą serwera Proxy aplikacji](active-directory-application-proxy-publish.md)
- [Rozwiązywanie problemów, jakie masz z serwera Proxy aplikacji](active-directory-application-proxy-troubleshoot.md)
- [Praca z aplikacjami pamiętać o oświadczeniach](active-directory-application-proxy-claims-aware-apps.md)
- [Włączanie dostępu warunkowego](active-directory-application-proxy-conditional-access.md)

Aby uzyskać najnowsze informacje i aktualizacje zapoznaj się z [serwera Proxy aplikacji blogu](http://blogs.technet.com/b/applicationproxyblog/)


<!--Image references-->
[1]: ./media/active-directory-application-proxy-sso-using-kcd/AuthDiagram.png
[2]: ./media/active-directory-application-proxy-sso-using-kcd/Properties.jpg
