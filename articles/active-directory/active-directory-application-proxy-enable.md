<properties
    pageTitle="Włączanie serwera Proxy aplikacji Azure AD | Microsoft Azure"
    description="Włączanie serwera Proxy aplikacji w portalu klasyczny Azure, a następnie zainstaluj łączników odwrotnej serwera proxy."
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
    ms.topic="get-started-article"
    ms.date="07/19/2016"
    ms.author="kgremban"/>

# <a name="enable-application-proxy-in-the-azure-portal"></a>Włączanie serwera Proxy aplikacji w portalu Azure

W tym artykule opisano czynności, aby włączyć Proxy aplikacji Microsoft Azure AD dla katalogu z chmury w Azure AD.

Jeśli nie wiesz, jakiego proxy aplikacji może ułatwić wykonać, Dowiedz się więcej na temat [sposobu bezpieczny dostęp zdalny do lokalnej aplikacji](active-directory-application-proxy-get-started.md).

## <a name="application-proxy-prerequisites"></a>Wymagania wstępne dotyczące aplikacji serwera Proxy
Aby można było włączyć i korzystać z serwera Proxy aplikacji usług, musi być:

- [Microsoft Azure AD podstawowego lub specjalnego subskrypcji](active-directory-editions.md) i katalog Azure AD, której jesteś administratorem globalnym.
- Serwer z systemem Windows Server 2012 R2 lub Windows 8.1 lub nowszego, na której można zainstalować łącznik serwera Proxy aplikacji. Serwer wysyła żądania do serwera Proxy aplikacji usług w chmurze, a niezbędne połączenia HTTP lub HTTPS do aplikacji, które są publikowane.

    - Dla rejestracji jednokrotnej dla poszczególnych opublikowanych aplikacji, ten komputer powinien być domeny — dołączony w tej samej domeny AD jako aplikacje, które są publikowane.

- Jeśli w ścieżce jest zapora, upewnij się, że jest otwarte, aby łącznik można wprowadzić żądania HTTPS (TCP) do serwera Proxy aplikacji. Łącznik te porty są używane razem z poddomen, które są częścią domen wysokiego poziomu msappproxy.net i servicebus.windows.net. Upewnij się, że Otwórz następujące porty do ruchu **wychodzącego** :

  	| Numer portu | Opis |
  	| --- | --- |
  	| 80 | Włączanie ruchu wychodzącego HTTP sprawdzania poprawności zabezpieczeń. |
  	| 443 | Włącz uwierzytelnianie użytkownika przed Azure AD (wymagane tylko w przypadku procesu rejestracji łącznik) |
  	| 10100 — 10120 | Włączanie odpowiedzi LOB HTTP wysyłane do serwera proxy |
  	| 9352, 5671 | Włączanie komunikacji między łącznik kierunku usługi Azure dla przychodzących żądań. |
  	| 9350 | Opcjonalnie umożliwić lepszą wydajność dla przychodzących żądań |
  	| 8080 | Włączanie łącznika sekwencji uruchamiania i automatyczna aktualizacja łącznika |
  	| 9090 | Włączyć rejestrowanie łącznik (wymagane tylko w przypadku procesu rejestracji łącznik) |
  	| 9091 | Włączanie automatycznego odnawiania certyfikatów zaufania łącznika |

    Jeśli Zapora wymusza ruch według pochodzącej użytkowników, otwórz następujące porty ruchu pochodzące z usługi Windows uruchomione usługi sieciowej. Upewnij się również włączyć portu 8080 dla NT\SYSTEM.

- Jeśli Twoja organizacja korzysta serwery proxy, aby nawiązać połączenie z Internetem, Poświęć spojrzenie na wpis w blogu [Praca z istniejących lokalne serwery proxy](https://blogs.technet.microsoft.com/applicationproxyblog/2016/03/07/working-with-existing-on-prem-proxy-servers-configuration-considerations-for-your-connectors/) Aby uzyskać szczegółowe informacje dotyczące sposobu ich konfigurowania.

## <a name="step-1-enable-application-proxy-in-azure-ad"></a>Krok 1: Włączenie serwera Proxy aplikacji Azure AD
1. Zaloguj się jako administrator w [portalu klasyczny Azure](https://manage.windowsazure.com/).
2. Przejdź do usługi Active Directory i wybierz katalogu, w którym chcesz włączyć serwer Proxy aplikacji.

    ![Usługa Active Directory - ikony](./media/active-directory-application-proxy-enable/ad_icon.png)

3. Wybierz pozycję **Konfiguruj** ze strony katalogu, a następnie przewiń w dół do **Serwera Proxy aplikacji**.
4. Przełącznik **Włącz usługi serwera Proxy aplikacji dla tego katalogu** **włączone**.

    ![Włączanie serwera Proxy aplikacji](./media/active-directory-application-proxy-enable/app_proxy_enable.png)

5. Wybierz pozycję **Pobierz teraz**. Powoduje przejście do **Azure AD aplikacji serwera Proxy łącznika pobieranie**. Przeczytaj i zaakceptuj postanowienia licencyjne, a następnie kliknij przycisk **Pobierz** do zapisania pliku Instalatora Windows (.exe) dla łącznika.

## <a name="step-2-install-and-register-the-connector"></a>Krok 2: Instalowanie i zarejestrować łącznik
1. Uruchom **AADApplicationProxyConnectorInstaller.exe** na serwerze, który przygotowane zgodnie z wymagania wstępne.
2. Postępuj zgodnie z instrukcjami w Kreatorze instalacji.
3. Podczas instalacji zostanie wyświetlony monit o zarejestrować łącznik z serwera Proxy aplikacji z dzierżawy usługi Azure AD.

  - Podaj poświadczenia administratora globalnego Azure AD. Dzierżawy usługi administratora globalnego mogą być inne niż poświadczenia Microsoft Azure.
  - Upewnij się, że administrator, który rejestruje łącznik znajduje się w tym samym katalogu, w którym włączono usługę serwera Proxy aplikacji. Na przykład w przypadku dzierżawy domeny contoso.com, administrator powinien być admin@contoso.com lub innych alias w tej domenie.
  - Jeśli **Konfiguracja zwiększonych zabezpieczeń programu Internet Explorer** jest ustawiona na **na** na serwerze podczas instalowania łącznika, może być zablokowany ekranie rejestracji. Postępuj zgodnie z instrukcjami wyświetlanymi w komunikacie o błędzie, aby umożliwić dostęp. Upewnij się, że zwiększonych zabezpieczeń programu Internet Explorer jest wyłączona.
  - Jeśli rejestracja łącznika kończy się niepowodzeniem, zobacz [Rozwiązywanie problemów z serwera Proxy aplikacji](active-directory-application-proxy-troubleshoot.md).  

4. Po zakończeniu instalacji, dwa nowe usługi są dodawane do swojego serwera:

    - **Microsoft AAD aplikacji serwera Proxy Connector** umożliwia połączenie
    - **Microsoft AAD aplikacji serwera Proxy łącznika aktualizacji** to usługa aktualizację automatyczną, która okresowo sprawdza, czy są nowe wersje łącznik i aktualizuje łącznik, stosownie do potrzeb.

    ![Aplikacja łącznika serwera Proxy usług — zrzut ekranu](./media/active-directory-application-proxy-enable/app_proxy_services.png)

5. Kliknij przycisk **Zakończ** w oknie instalacji.

Do celów wysokiej dostępności należy wdrożyć co najmniej dwa łączniki. Aby wdrożyć więcej łączników, powtórz kroki 2 i 3 powyżej. Każdy łącznik musi być zarejestrowania oddzielnie.

Jeśli chcesz odinstalować łącznik, odinstaluj zarówno usługi łącznik i aktualizacji. Uruchom ponownie komputer, aby całkowicie usunąć usługę.


## <a name="next-steps"></a>Następne kroki

Teraz możesz przystąpić do [publikowania aplikacji z serwera Proxy aplikacji](active-directory-application-proxy-publish.md).

Jeśli masz aplikacje znajdujące się na oddzielne sieci lub innej lokalizacji, umożliwia grup łącznik organizowanie różnych łączników za pomocą jednostek logicznych. Dowiedz się więcej na temat [pracy z serwera Proxy aplikacji łączników](active-directory-application-proxy-connectors.md).
