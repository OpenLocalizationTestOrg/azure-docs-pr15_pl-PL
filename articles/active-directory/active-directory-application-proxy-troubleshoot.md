<properties
    pageTitle="Rozwiązywanie problemów z serwera Proxy aplikacji | Microsoft Azure"
    description="Opisano, jak rozwiązywać problemy z błędami w Azure AD serwera Proxy aplikacji."
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
    ms.date="08/19/2016"
    ms.author="kgremban"/>



# <a name="troubleshoot-application-proxy"></a>Rozwiązywanie problemów z serwera Proxy aplikacji

Jeśli wystąpią błędy w uzyskiwanie dostępu do opublikowanej aplikacji ani w aplikacjach publikowania, sprawdź poniższe opcje, aby zobaczyć, czy serwer Proxy aplikacji usług Microsoft Azure AD działa poprawnie:

- Otwórz konsolę usługi i sprawdź, czy jest włączony usługę **Microsoft AAD aplikacji serwera Proxy łącznika** oraz uruchamianie. Możesz spojrzeć na stronie właściwości serwera Proxy aplikacji usług jak pokazano na poniższej ilustracji:  
  ![Zrzut ekranu okna właściwości łącznika Microsoft AAD aplikacji serwera Proxy](./media/active-directory-application-proxy-troubleshoot/connectorproperties.png)

- Otwórz program Podgląd zdarzeń i odszukaj serwer Proxy aplikacji łącznika zdarzeń w **Dzienniki aplikacji i usług** > **Microsoft** > **AadApplicationProxy** > **Łącznik** > **administratora**.
- W razie potrzeby bardziej szczegółowe dzienniki są dostępne, włączając analizy debugowania dzienniki i włączenie dziennika sesji serwera Proxy aplikacji łącznika.

## <a name="the-page-is-not-rendered-correctly"></a>Strony nie jest poprawnie renderowana

Jeśli nie jest wyświetlany komunikat o błędzie, może być nadal masz problemy z aplikacją renderowania lub działać poprawnie. Przyczyną może być opublikowany Ścieżka artykułu, ale aplikacja wymaga zawartości, która istnieje poza tej ścieżki.

Na przykład jeśli publikowanie https://yourapp/app ścieżkę, ale aplikacja wywołuje obrazów w https://yourapp/media, ta osoba nie będą renderowane. Upewnij się, podczas publikowania aplikacji przy użyciu najwyższego poziomu ścieżkę należy uwzględnić wszystkie odpowiedniej zawartości. W tym przykładzie jest http://yourapp/.

Jeśli zmienisz ścieżce umieszczenie zawartości, której dotyczy odwołanie, ale nadal potrzebujesz użytkownikom grunt szczegółowego łącze w ścieżce, zobacz blogu [Ustawienie prawo łącze do aplikacji serwera Proxy aplikacji w panelu dostępu Azure AD i uruchamianie aplikacji usługi Office 365](https://blogs.technet.microsoft.com/applicationproxyblog/2016/04/06/setting-the-right-link-for-application-proxy-applications-in-the-azure-ad-access-panel-and-office-365-app-launcher/).

## <a name="general-errors"></a>Błędy ogólne

Błąd | Opis | Rozdzielczość
--- | --- | ---
Nie można uzyskać dostępu do tej aplikacji firmowej. Nie masz uprawnień dostępu do tej aplikacji. Autoryzacja nie powiodła się. Upewnij się przypisać użytkownikowi dostęp do tej aplikacji. | Być może nie przypisano użytkownika dla tej aplikacji. | Przejdź na kartę **aplikacji** , a następnie w obszarze **Użytkownicy i grupy**, przypisz tego użytkownika lub grupy użytkowników do tej aplikacji.
Nie można uzyskać dostępu do tej aplikacji firmowej. Nie masz uprawnień dostępu do tej aplikacji. Autoryzacja nie powiodła się. Upewnij się, że użytkownik ma licencję dla Azure Active Directory Premium lub Basic. | Ten błąd może występować użytkowników, próbując uzyskać dostęp do aplikacji, którą publikujesz, jeśli nie zostały one jawnie przypisane z licencją Premium i Basic przez administratora abonenta. | Przejdź do karty usługi Active Directory **licencji** abonenta i upewnij się, że tego użytkownika lub grupy użytkowników przypisano Premium lub podstawowe licencji.


## <a name="connector-troubleshooting"></a>Rozwiązywanie problemów z łącznika
Jeśli rejestracja nie powiedzie się podczas instalacji Kreatora łącznika, istnieją dwa sposoby wyświetlania przyczyny błędu. Szukaj w dzienniku zdarzeń **aplikacji i usług Logs\Microsoft\AadApplicationProxy\Connector\Admin**albo uruchom następujące polecenie programu Windows PowerShell.

    Get-EventLog application –source “Microsoft AAD Application Proxy Connector” –EntryType “Error” –Newest 1

| Błąd | Opis | Rozdzielczość |
| --- | --- | --- |
| Rejestracja łącznika nie powiodła się: Upewnij się, jeśli włączono serwer Proxy aplikacji w portalu zarządzania Azure i wprowadzone poprawnie usługi Active Directory, nazwę użytkownika i hasło. Błąd: "co najmniej jeden wystąpił błąd." | Okno Rejestracja jest zamknięty bez wykonywania logujesz się do Azure AD. | Ponownie uruchom Kreatora łącznika i zarejestrować łącznik. |
| Rejestracja łącznika nie powiodła się: Upewnij się, jeśli włączono serwer Proxy aplikacji w portalu zarządzania Azure i wprowadzone poprawnie usługi Active Directory, nazwę użytkownika i hasło. Błąd: "AADSTS50001: zasobów `https://proxy.cloudwebappproxy.net /registerapp` jest wyłączona,." | Serwer Proxy aplikacji jest wyłączona.  | Upewnij się, że możesz włączyć serwer Proxy aplikacji w portalu klasyczny Azure przed próby zarejestrowania łącznik. Aby uzyskać więcej informacji o włączaniu serwera Proxy aplikacji zobacz [Włączanie serwera Proxy aplikacji usług](active-directory-application-proxy-enable.md). |
| Rejestracja łącznika nie powiodła się: Upewnij się, jeśli włączono serwer Proxy aplikacji w portalu zarządzania Azure i wprowadzone poprawnie usługi Active Directory, nazwę użytkownika i hasło. Błąd: "co najmniej jeden wystąpił błąd." | Jeśli okno Rejestracja zostanie otwarty, a następnie natychmiast zamknięte bez zezwolenia na zalogowanie się w, prawdopodobnie otrzymają ten błąd. Ten błąd występuje, gdy występuje błąd sieci w systemie. | Upewnij się, że jest możliwe nawiązać połączenie przy użyciu przeglądarki publicznej witryny sieci Web i że porty są otwarte zgodnie z [wymagania wstępne dotyczące serwera Proxy aplikacji](active-directory-application-proxy-enable.md). |
| Rejestracja łącznika nie powiodła się: Upewnij się, że komputer jest połączony z Internetem. Błąd: "Wystąpił nie punkt końcowy w elemencie `https://connector.msappproxy.net :9090/register/RegisterConnector` którą może zaakceptować wiadomość. Jest to często spowodowane przez niepoprawny adres lub akcją protokołu SOAP. Jeśli istnieją, aby uzyskać więcej informacji, zobacz InnerException,. " | Jeśli logowanie się przy użyciu usługi Azure AD nazwy użytkownika i hasła, ale następnie wystąpi ten błąd, może być blokowanych wszystkie porty powyżej 8081. | Upewnij się, że porty niezbędne są otwarte. Aby uzyskać więcej informacji zobacz [wymagania wstępne dotyczące serwera Proxy aplikacji](active-directory-application-proxy-enable.md). |
| Wyczyść błędu są przedstawione w oknie rejestracji. Nie można kontynuować — tylko, aby zamknąć okno. | Wprowadzono nieprawidłowe nazwy użytkownika i hasła. | Próbuj ponownie. |
| Rejestracja łącznika nie powiodła się: Upewnij się, jeśli włączono serwer Proxy aplikacji w portalu zarządzania Azure i wprowadzone poprawnie usługi Active Directory, nazwę użytkownika i hasło. Błąd: "AADSTS50059: żadne informacje identyfikujące dzierżawy znaleziony w jednym żądania lub wynika z poświadczeń dostarczonych i wyszukiwanie według zasady usług URI nie powiodło się. | Próbujesz się zalogować się przy użyciu Account Microsoft i nie będącej częścią identyfikator organizacji katalogu, który próbujesz uzyskać dostęp do domeny. | Upewnij się, że administrator jest częścią tą samą nazwą domeny jako domena dzierżawy, na przykład w przypadku Azure AD domeny contoso.com, administrator powinien być admin@contoso.com. |
| Nie można pobrać bieżącej zasad wykonywania uruchamiania skryptów programu PowerShell. | Jeśli instalacja łącznik kończy się niepowodzeniem, sprawdź, upewnij się, że zasad wykonywania PowerShell nie jest wyłączona. | Otwórz Edytor zasad grupy. Przejdź do pozycji **Konfiguracja komputera** > **Szablony administracyjne** > **Składniki Windows** > **Programu Windows PowerShell** i kliknij dwukrotnie **włączyć wykonywanie skryptu**. Może to być równa **Nie skonfigurowano** lub **włączone**. Jeśli ustawienie zostanie **włączone**, upewnij się, że w obszarze Opcje zasad wykonywania ustawiono albo **Zezwalaj lokalny i zdalny podpisanego skryptów** lub zezwolić **skryptom wszystkie**. |
| Łącznik nie można pobrać konfiguracji. | Certyfikat klienta łącznika, który jest używany do uwierzytelniania, wygasła. Może to również wystąpić, jeśli masz zainstalowany za serwerem proxy łącznik. W tym przypadku łącznik nie może uzyskać dostęp do Internetu, a nie będą mogli zapewnić aplikacji użytkownikom zdalnym. | Odnawianie zaufania ręcznie przy użyciu `Register-AppProxyConnector` polecenia cmdlet w programie Windows PowerShell. Jeśli wybrany łącznik znajduje się za serwerem proxy, należy udzielić dostęp do Internetu z kontami łącznik "usługi sieciowe" i "system lokalny". Można to osiągnąć przyznając im dostępu do serwera Proxy lub ustawiając ich tak, aby pominąć serwer proxy. |
| Rejestracja łącznika nie powiodła się: Upewnij się, że jesteś administratorem globalnym usługi Active Directory, aby zarejestrować łącznik. Błąd: "rejestracji żądanie zostało odrzucone." | Alias, który próbujesz się zalogować przy użyciu nie jest administratorem w tej domenie. Wybrany łącznik jest zawsze instalowany do katalogu, który jest właścicielem domeny użytkownika. | Upewnij się, że administrator próbujesz się zalogować globalnej uprawnieniami do dzierżawy Azure AD.|


## <a name="kerberos-errors"></a>Błędy Kerberos

| Błąd | Opis | Rozdzielczość |
| --- | --- | --- |
| Nie można pobrać bieżącej zasad wykonywania uruchamiania skryptów programu PowerShell. | Jeśli instalacja łącznik kończy się niepowodzeniem, sprawdź, upewnij się, że zasad wykonywania PowerShell nie jest wyłączona. | Otwórz Edytor zasad grupy. Przejdź do pozycji **Konfiguracja komputera** > **Szablony administracyjne** > **Składniki Windows** > **Programu Windows PowerShell** i kliknij dwukrotnie **włączyć wykonywanie skryptu**. Może to być równa **Nie skonfigurowano** lub **włączone**. Jeśli ustawienie zostanie **włączone**, upewnij się, że w obszarze Opcje zasad wykonywania ustawiono albo **Zezwalaj lokalny i zdalny podpisanego skryptów** lub zezwolić **skryptom wszystkich**. |
| 12008 - azure AD Przekroczono maksymalną liczbę dozwolonych prób uwierzytelniania Kerberos na serwerze wewnętrznej bazy danych. | To zdarzenie może oznaczać niepoprawnej konfiguracji między Azure AD i serwera aplikacji wewnętrznej bazy danych lub innego problemu w konfiguracji daty i godziny na obu komputerach. | Bilet Kerberos utworzone przez Azure AD odrzucone z serwera wewnętrznej bazy danych. Sprawdź, że Azure AD i serwera wewnętrznej bazy danych aplikacji są skonfigurowane poprawnie. Upewnij się, że konfiguracja daty i godziny na Azure AD i są synchronizowane z serwerem aplikacji wewnętrznej bazy danych. |
| 13016 - azure AD nie można pobrać bilet Kerberos w imieniu użytkownika, ponieważ istnieje nie UPN w token krawędzi lub w pliku programu access. | Występuje problem z konfiguracją usługi STS. | Popraw konfiguracji roszczeń głównej nazwy użytkownika w STS. |
| 13019 - azure AD nie można pobrać bilet Kerberos w imieniu użytkownika ze względu na następujące ogólnych błędów API. | To zdarzenie może oznaczać niepoprawnej konfiguracji między Azure AD i serwer kontrolera domeny lub problem w konfiguracji daty i godziny na obu komputerach. | Kontroler domeny odrzucił bilet Kerberos utworzone przez Azure AD. Sprawdź tego Azure AD i serwera wewnętrznej bazy danych aplikacji są skonfigurowane poprawnie, zwłaszcza w konfiguracji SPN. Upewnij się, że Azure AD jest domeną dołączony do tej samej domeny jako kontrolera domeny, aby upewnić się, że kontroler domeny tworzy relację zaufania z usługą Azure Active Directory. Upewnij się, że konfiguracja daty i godziny na Azure AD i kontrolera domeny są synchronizowane. |
| 13020 - azure AD nie można pobrać bilet Kerberos w imieniu użytkownika, ponieważ nie zdefiniowano serwera wewnętrznej bazy danych SPN. | To zdarzenie może oznaczać niepoprawnej konfiguracji między Azure AD i serwer kontrolera domeny lub problem w konfiguracji daty i godziny na obu komputerach. | Kontroler domeny odrzucił bilet Kerberos utworzone przez Azure AD. Sprawdź tego Azure AD i serwera wewnętrznej bazy danych aplikacji są skonfigurowane poprawnie, zwłaszcza w konfiguracji SPN. Upewnij się, że Azure AD jest domeną dołączony do tej samej domeny jako kontrolera domeny, aby upewnić się, że kontrolera domeny tworzy relację zaufania z usługą Azure Active Directory. Upewnij się, że konfiguracja daty i godziny na Azure AD i kontrolera domeny są synchronizowane. |
| 13022 - azure AD nie uwierzytelnienia użytkownika, ponieważ serwer wewnętrznej bazy danych odpowiada prób uwierzytelniania Kerberos ze względu na błąd HTTP 401. | To zdarzenie może oznaczać niepoprawnej konfiguracji między Azure AD i serwera aplikacji wewnętrznej bazy danych lub innego problemu w konfiguracji daty i godziny na obu komputerach. | Bilet Kerberos utworzone przez Azure AD odrzucone z serwera wewnętrznej bazy danych. Sprawdź, że Azure AD i serwera wewnętrznej bazy danych aplikacji są skonfigurowane poprawnie. Upewnij się, że konfiguracja daty i godziny na Azure AD i są synchronizowane z serwerem aplikacji wewnętrznej bazy danych. |
| Witryny sieci Web nie można wyświetlić strony. | Ten błąd może występować użytkownika, próbując uzyskać dostęp do aplikacji, którą opublikowany Jeśli aplikacja jest aplikacją Zintegrowane. Mogą być nieprawidłowe zdefiniowanej SPN dla tej aplikacji. | W przypadku aplikacji Zintegrowane: Upewnij się, że SPN skonfigurowana dla tej aplikacji jest poprawny. |
| Witryny sieci Web nie można wyświetlić strony. | Ten błąd może występować użytkownika, próbując uzyskać dostęp do aplikacji, którą opublikowany w przypadku aplikacji aplikacji OWA. Może to być spowodowane jedną z następujących czynności: <br> SPN zdefiniowane dla tej aplikacji jest nieprawidłowy. <br> — Użytkownik próbował uzyskać dostęp do aplikacji jest za pomocą konta Microsoft zamiast właściwe konto firmowe, aby się zalogować, lub jeśli użytkownik jest gościa. <br> – Dla tej aplikacji na stronie lokalna użytkownik próbował uzyskać dostęp do aplikacji nie jest poprawnie zdefiniowany. | Kroki zmniejszeniu odpowiednio: <br> -Upewnij się, że jest poprawny SPN skonfigurowana dla tej aplikacji. <br> -Upewnij się, że użytkownik zaloguje się przy użyciu swojego konta firmowe odpowiadający domenie opublikowanej aplikacji. Użytkownicy Account firmy Microsoft i Gość nie ma dostępu Zintegrowane aplikacje. <br> -Upewnij się, że ten użytkownik ma odpowiednie uprawnienia określone dla tej aplikacji wewnętrznej bazy danych na komputerze lokalna. |
| Nie można uzyskać dostępu do tej aplikacji firmowej. Nie masz uprawnień dostępu do tej aplikacji. Autoryzacja nie powiodła się. Upewnij się przypisać użytkownikowi dostęp do tej aplikacji. | Ten błąd może występować użytkowników, próbując uzyskać dostęp do aplikacji, którą publikujesz, gdy korzystają z konta serwera Microsoft zamiast ich firmowe konto do zalogowania się. Użytkowników gości może zostać również wyświetlony ten błąd. | Użytkownicy Account Microsoft i goście nie ma dostępu Zintegrowane aplikacje. Upewnij się, użytkownik zaloguje się przy użyciu swojego konta firmowe odpowiadający domenie opublikowanej aplikacji. |
| Tej aplikacji firmowej nie są obecnie dostępne. Spróbuj ponownie później... Łącznik został przekroczony. | Ten błąd może występować użytkowników, próbując uzyskać dostęp do aplikacji, która została opublikowana, jeśli nie są poprawnie zdefiniowane dla tej aplikacji na stronie lokalna. | Upewnij się, że użytkownicy mają odpowiednie uprawnienia określone dla tej aplikacji wewnętrznej bazy danych na komputerze lokalna. |


## <a name="see-also"></a>Zobacz też

- [Włączanie serwera Proxy aplikacji dla usługi Azure Active Directory](active-directory-application-proxy-enable.md)
- [Publikowanie aplikacji za pomocą serwera Proxy aplikacji](active-directory-application-proxy-publish.md)
- [Włączanie rejestracji jednokrotnej](active-directory-application-proxy-sso-using-kcd.md)
- [Włączanie dostępu warunkowego](active-directory-application-proxy-conditional-access.md)

Aby uzyskać najnowsze informacje i aktualizacje zapoznaj się z [serwera Proxy aplikacji blogu](http://blogs.technet.com/b/applicationproxyblog/)


<!--Image references-->
[1]: ./media/active-directory-application-proxy-troubleshoot/connectorproperties.png
[2]: ./media/active-directory-application-proxy-troubleshoot/sessionlog.png
