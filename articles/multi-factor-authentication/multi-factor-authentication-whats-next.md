<properties
    pageTitle="Uwierzytelnianie wieloskładnikowe Azure — co to jest dalej"
    description="To jest strona uwierzytelnianie wieloskładnikowe Azure opisującą co robić dalej MFA.  W tym raporty, oszustwa alert, jednorazowego obejścia, wiadomości głosowych niestandardowe, buforowanie zaufane adresy IP i aplikacji hasła."
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
    ms.date="09/23/2016"
    ms.author="kgremban"/>

# <a name="configuring-azure-multi-factor-authentication"></a>Konfigurowanie uwierzytelniania wieloskładnikowego Azure

Ten artykuł ułatwia zarządzanie uwierzytelnianie wieloskładnikowe Azure się i rozpocząć pracę.  Obejmuje różne tematy, które pomagają w pełni wykorzystać uwierzytelnianie wieloskładnikowe Azure.  Nie wszystkie te funkcje są dostępne w każdej wersji uwierzytelnianie wieloskładnikowe Azure.

Konfiguracja dla niektórych funkcji poniżej znajduje się w portalu zarządzania uwierzytelnianie wieloskładnikowe Azure. Istnieją dwa sposoby których będziesz mieć dostęp do portalu zarządzania MFA, które wykonuje się przez Azure portal. Pierwsza jest korzystając z funkcji zarządzania dostawcy uwierzytelnianie wieloskładnikowe, korzystając z systemem zużycie MFA. Druga jest skorzystanie z ustawienia usługi MFA. Jest to druga opcja wymaga dostawcy uwierzytelnianie wieloskładnikowe lub licencji usługi Azure MFA, Azure AD Premium lub pakietu mobilności przedsiębiorstwa.

Aby uzyskać dostęp do portalu zarządzania MFA za pośrednictwem dostawcy uwierzytelnianie wieloskładnikowe Azure, zaloguj się do portalu Azure jako administrator i wybierz opcję usługi Active Directory. Kliknij kartę **Dostawców uwierzytelniania wieloskładnikowego** , a następnie wybierz pozycję katalogu, a następnie kliknij przycisk **Zarządzaj** u dołu.

Aby uzyskać dostęp do portalu zarządzania MFA za pośrednictwem strony Ustawienia usługi MFA, zaloguj się do portalu Azure jako administrator i wybierz opcję usługi Active Directory. Kliknij polecenie w katalogu, a następnie kliknij kartę **Konfigurowanie** . W sekcji uwierzytelnianie wieloskładnikowe wybierz pozycję **Ustawienia usługi Zarządzanie**. U dołu strony Ustawienia usługi MFA kliknij łącze **Przejdź do portalu** .


Funkcja| Opis| Co to jest objęta
:------------- | :------------- | :------------- |
[Alert oszustwa](#fraud-alert)|Alert oszustwa można skonfigurować i skonfigurować tak, aby użytkownicy mogą zgłaszać fałszywej próbami dostępu do swoich zasobów.|Jak skonfigurować, konfigurowanie i raport oszustwa
[Obejście jednorazowego](#one-time-bypass) |Jednorazowe obejścia umożliwia użytkownikowi uwierzytelnienia jeden raz pomijając"" uwierzytelnianie wieloskładnikowe.|Jak utworzyć i skonfigurować jednorazowego obejścia
[Wiadomości głosowych niestandardowe](#custom-voice-messages) |Wiadomości głosowych niestandardowe umożliwiają własne nagrania lub powitania za pomocą uwierzytelnianie wieloskładnikowe. |Jak utworzyć i skonfigurować niestandardowe życzenia i wiadomości
[Pamięci podręcznej](#caching-in-azure-multi-factor-authentication)|Buforowanie umożliwia ustawianie określonym czasie okresu, tak aby prób uwierzytelnienia kolejnych powiodła się automatycznie. |Jak utworzyć i skonfigurować pamięci podręcznej uwierzytelniania.
[Adresy IP zaufanych](#trusted-ips)|Zaufane, że adresy IP, to funkcja uwierzytelnianie wieloskładnikowe, który umożliwia administratorom dzierżawy zarządzanej lub federacyjnych możliwość obejścia uwierzytelnianie wieloskładnikowe dla użytkowników, którzy są zalogowaniu się z lokalny intranet firmy.|Konfigurowanie i konfigurowanie adresów IP, które nie podlegają uwierzytelniania wieloskładnikowego
[Hasła aplikacji](#app-passwords)|Hasła umożliwia aplikacji, która nie jest MFA-pamiętać, aby pominąć uwierzytelnianie wieloskładnikowe i kontynuować pracę.|Informacje o hasła aplikacji.
[Pamiętaj uwierzytelnianie wieloskładnikowe zapamiętane urządzenia i przeglądarki](#remember-multi-factor-authentication-for-devices-users-trust)|Umożliwia zapamiętać urządzeń na ustalona liczba dni po użytkownik został pomyślnie zalogowanie się za pomocą MFA.|Informacje o włączenie tej funkcji i konfigurując liczbę dni.
[Metody weryfikacji możliwe do wybrania](#selectable-verification-methods)|Umożliwia wybranie metody uwierzytelniania, które są dostępne dla użytkowników użyć.|Informacje dotyczące włączania lub wyłączania określonych metod uwierzytelniania takich jak połączenia lub wiadomości tekstowe.



## <a name="fraud-alert"></a>Alert oszustwa
Alert oszustwa można skonfigurować i skonfigurować tak, aby użytkownicy mogą zgłaszać fałszywej próbami dostępu do swoich zasobów.  Użytkownicy zgłosić oszustwa przy użyciu aplikacji dla urządzeń przenośnych lub za pośrednictwem telefonu.

### <a name="to-set-up-and-configure-fraud-alert"></a>Aby utworzyć i skonfigurować alert oszustwa

1.  Zaloguj się do http://azure.microsoft.com
2.  Przejdź do portalu zarządzania MFA zgodnie z instrukcjami przedstawionymi w górnej części tej strony.
3.  W portalu zarządzania uwierzytelnianie wieloskładnikowe Azure kliknij przycisk Ustawienia w sekcji Konfigurowanie.
4.  W sekcji oszustwa alertu na stronie ustawienia sprawdź Zezwalaj użytkownikom na przesyłanie wyboru oszustwa alertów.
5.  Jeśli użytkownik ma mieć możliwość zablokowane, gdy jest zgłaszany oszustwa, należy zaznaczyć Blokuj użytkownika po oszustwa jest zgłoszone.
6.  W polu tekstowym **Kodu do raportu oszustwa podczas początkowej pozdrowienia** wprowadź kod, który może być używany podczas połączenia weryfikacji. Jeśli użytkownik wprowadzi ten kod oraz # zamiast tylko znak #, alert oszustwa zostaną zgłoszone.
7.  U dołu kliknij przycisk Zapisz.

>[AZURE.NOTE]
>Powitań głosowej domyślne firmy Microsoft Poinstruuj użytkowników, aby naciśnij # 0, aby przesłać alert oszustwa. Jeśli chcesz użyć kodu inną niż 0, należy rekord i przekaż powitania własnego głosu niestandardowych odpowiednie instrukcje.


![Chmury](./media/multi-factor-authentication-whats-next/fraud.png)

### <a name="to-report-fraud-alert"></a>Aby alert oszustwa raportu
Alert oszustwa mogą zostać zgłoszone na dwa sposoby.  Lub za pośrednictwem aplikacji dla urządzeń przenośnych za pomocą telefonu.  

### <a name="to-report-fraud-alert-with-the-mobile-app"></a>Aby alert oszustwa raportu z aplikacji dla urządzeń przenośnych



1. Po weryfikacji jest wysyłana do telefonu, zaznacz je, aby uruchomić aplikację Microsoft Authenticator.
2. Raport oszustwa, kliknij przycisk Anuluj i oszustwa raportu. Spowoduje to wyświetlenie pola informacją, że zostaną powiadomieni personelu informatycznej pomocy technicznej Twojej organizacji.
3. Kliknij raport oszustwa.
4. W aplikacji kliknij przycisk Zamknij.

![Chmury](./media/multi-factor-authentication-whats-next/report1.png)


![Chmury](./media/multi-factor-authentication-whats-next/fraud2.png)

### <a name="to-report-fraud-alert-with-the-phone"></a>Aby alert oszustwa raportu z telefonem

1. Gdy połączenie weryfikacji połączy się z telefonu, udzielenie odpowiedzi.  
2. Aby zgłosić oszustwa, wprowadź kod, który został skonfigurowany do raportowania oszustwa za pośrednictwem telefonu, a następnie znak #. Otrzymasz powiadomienie przesłaniu alert oszustwa.
3. Zakończ połączenie.

### <a name="to-view-the-fraud-report"></a>Aby wyświetlić raport oszustwa

1. Zaloguj się do [http://azure.microsoft.com](https://azure.microsoft.com/)
2. Po lewej stronie wybierz pozycję usługi Active Directory.
3. U góry wybierz dostawców uwierzytelniania wieloskładnikowego. Spowoduje to wyświetlenie listy do dostawców uwierzytelniania wieloskładnikowego.
4. Jeśli masz więcej niż jednego dostawcę uwierzytelnianie wieloskładnikowe, wybierz ten, który chcesz wyświetlić raport alert oszustwami i kliknij pozycję Zarządzaj w dolnej części strony. Jeśli masz tylko jedną, kliknij przycisk Zarządzaj. Spowoduje to otwarcie portalu zarządzania uwierzytelnianie wieloskładnikowe Azure.
5. W portalu zarządzania uwierzytelnianie wieloskładnikowe na Azure, po lewej stronie w obszarze Wyświetl raport kliknij pozycję oszustwa Alert.
6. Określ zakres dat, który ma być wyświetlany w raporcie. Również można określić dowolną określonych użytkowników i numeru telefonu oraz stan użytkownika.
7. Kliknij przycisk Uruchom. Spowoduje to wyświetlenie raport podobny do tego poniżej. Możesz również kliknąć Eksportuj, aby CSV, jeśli chcesz wyeksportować raport.

## <a name="one-time-bypass"></a>Obejście jednorazowego

Jednorazowe obejścia umożliwia użytkownikowi uwierzytelnienia jeden raz pomijając"" uwierzytelnianie wieloskładnikowe. Obejście jest tymczasowy i wygasa po określonej liczbie sekund.  Aby w sytuacjach, gdy aplikacji dla urządzeń przenośnych lub telefon nie odbiera powiadomienie lub rozmowy telefonicznej, możesz włączyć jednorazowego obejścia, użytkownik może uzyskać dostęp do żądanego zasobu.

### <a name="to-create-a-one-time-bypass"></a>Aby utworzyć jednorazowego obejścia

1.  Zaloguj się do http://azure.microsoft.com
2.  Przejdź do portalu zarządzania MFA zgodnie z instrukcjami przedstawionymi w górnej części tej strony.
3.  W Azure wieloskładnikowe uwierzytelniania portalu zarządzania, jeśli nazwa dzierżawy lub dostawcy MFA Azure jest wyświetlana po lewej stronie z + obok niej, kliknij pozycję + Zobacz różnych grup replikacji serwera MFA i grupy domyślne Azure. Kliknij odpowiednią grupę.
4.  W obszarze Administracja użytkownika kliknij pozycję **One-Time obejścia**.
![Chmury](./media/multi-factor-authentication-whats-next/create1.png)
5.  Na stronie obejścia One-Time kliknij **Nowy One-Time obejścia**.
6.  Nazwa użytkownika użytkownika, liczbę sekund, które wystąpią obejścia, powód obejścia i kliknij przycisk **pomijać**.
![Chmury](./media/multi-factor-authentication-whats-next/create2.png)
7.  W tym momencie użytkownik musi się zarejestrować przed wygaśnięciem jednorazowego obejścia.



### <a name="to-view-the-one-time-bypass-report"></a>Aby wyświetlić raport obejścia jednorazowego

1. Zaloguj się do [http://azure.microsoft.com](https://azure.microsoft.com/)
2. Po lewej stronie wybierz pozycję usługi Active Directory.
3. U góry wybierz dostawców uwierzytelniania wieloskładnikowego. Spowoduje to wyświetlenie listy do dostawców uwierzytelniania wieloskładnikowego.
4. Jeśli masz więcej niż jednego dostawcę uwierzytelnianie wieloskładnikowe, wybierz ten, który chcesz wyświetlić raport alert oszustwami i kliknij pozycję Zarządzaj w dolnej części strony. Jeśli masz tylko jedną, kliknij przycisk Zarządzaj. Spowoduje to otwarcie portalu zarządzania uwierzytelnianie wieloskładnikowe Azure.
5. W portalu zarządzania uwierzytelnianie wieloskładnikowe na Azure, po lewej stronie w obszarze Wyświetl raport kliknij polecenie One-Time obejścia.
6. Określ zakres dat, który ma być wyświetlany w raporcie. Również można określić dowolną określonych użytkowników i numeru telefonu oraz stan użytkownika.
7. Kliknij przycisk Uruchom. Spowoduje to wyświetlenie raport podobny do tego poniżej. Możesz również kliknąć Eksportuj, aby CSV, jeśli chcesz wyeksportować raport.

<center>![Chmury](./media/multi-factor-authentication-whats-next/report.png)</center>

## <a name="custom-voice-messages"></a>Wiadomości głosowych niestandardowe

Wiadomości głosowych niestandardowe umożliwiają własne nagrania lub powitania za pomocą uwierzytelnianie wieloskładnikowe.  Te mogą być używane dodatkowo lub aby zamienić Microsoft rekordów.

Przed rozpoczęciem należy pamiętać o następujących czynności:

- Bieżący formaty plików obsługiwane są wav i MP3.
- Limit rozmiaru pliku jest 5 MB.
- Zaleca się, że dla uwierzytelniania wiadomości, które być dłuższa niż 20 sekund. Nic większa niż to spowodować, że weryfikacja kończy się niepowodzeniem, ponieważ użytkownik może nie odpowiadać przed kończy się wiadomość i weryfikacja przekroczenie limitu czasu.



### <a name="to-set-up-custom-voice-messages-in-azure-multi-factor-authentication"></a>Aby skonfigurować wiadomości głosowych niestandardowe w uwierzytelnianie wieloskładnikowe Azure
1.  Tworzenie wiadomości głosowych niestandardowych przy użyciu jednego z obsługiwanych formatów plików.
2.  Zaloguj się do http://azure.microsoft.com
3.  Przejdź do portalu zarządzania MFA zgodnie z instrukcjami przedstawionymi w górnej części tej strony.
4.  W portalu zarządzania uwierzytelnianie wieloskładnikowe Azure kliknij wiadomości głosowych w sekcji Konfigurowanie.
5.  W sekcji wiadomości głosowych kliknij pozycję **Nowe wiadomości głosowej**.
![Chmury](./media/multi-factor-authentication-whats-next/custom1.png)
6.  W konfiguracji: nowej wiadomości głosowych strony, kliknij pozycję **Zarządzaj pliki dźwiękowe**.
![Chmury](./media/multi-factor-authentication-whats-next/custom2.png)
7.  W konfiguracji: dźwięk pliki strony, kliknij pozycję **Przekaż plik dźwiękowy**.
![Chmury](./media/multi-factor-authentication-whats-next/custom3.png)
8.  W konfiguracji: przekazać plik dźwiękowy, kliknij przycisk **Przeglądaj** i przejdź do wiadomości głosowych, kliknij przycisk **Otwórz**.
![Chmury](./media/multi-factor-authentication-whats-next/custom4.png)
9.  Dodaj opis, a następnie kliknij pozycję Przekaż.
10. Po ukończeniu tej operacji komunikat potwierdzający, że plik został pomyślnie przesłany.
11. Po lewej stronie kliknij pozycję wiadomości głosowej.
12. W sekcji wiadomości głosowych kliknij pozycję nowe wiadomości głosowej.
13. W języku listę rozwijaną wybierz język.
14. Jeśli ten komunikat jest dla określonej aplikacji, określ wartość w polu Nazwa aplikacji.
15. Z typem wiadomości wybierz odpowiedni typ wiadomości zastąpienie z naszych nowych niestandardowym komunikatem.
16. W pliku dźwiękowego listę rozwijaną wybierz plik dźwiękowy.
17. Kliknij przycisk **Utwórz**. Komunikat potwierdzający, że pomyślnie utworzono wiadomości głosowej.
![Chmury](./media/multi-factor-authentication-whats-next/custom5.png)</center>



## <a name="caching-in-azure-multi-factor-authentication"></a>Pamięci podręcznej w Azure uwierzytelnianie wieloskładnikowe

Buforowanie umożliwia ustawianie określonym czasie okresu, tak aby prób uwierzytelnienia kolejnych powiodła się automatycznie. To jest używany głównie systemów lokalnego na przykład VPN wysyłania wiele żądań weryfikacji podczas pierwszego żądania jest nadal w toku. Dzięki temu kolejne żądania powiodła się automatycznie po pomyślnym użytkownika weryfikacji w toku. Należy zauważyć, że buforowanie nie ma ma być używany dla logowania dodatki do Azure AD.


### <a name="to-set-up-caching-in-azure-multi-factor-authentication"></a>Aby skonfigurować pamięci podręcznej w uwierzytelnianie wieloskładnikowe Azure

1.  Zaloguj się do http://azure.microsoft.com
2.  Przejdź do portalu zarządzania MFA zgodnie z instrukcjami przedstawionymi w górnej części tej strony.
3.  W portalu zarządzania uwierzytelnianie wieloskładnikowe Azure kliknij przycisk buforowanie w sekcji Konfigurowanie.
4.  W konfiguracji buforowania stron kliknij nową pamięć podręczną
5.  Wybierz żądany typ pamięci podręcznej i sekund pamięci podręcznej. Kliknij przycisk Utwórz.

<center>![Chmury](./media/multi-factor-authentication-whats-next/cache.png)</center>

## <a name="trusted-ips"></a>Adresy IP zaufanych

Zaufane, że adresy IP, to funkcja uwierzytelnianie wieloskładnikowe, który umożliwia administratorom dzierżawy zarządzanej lub federacyjnych możliwość obejścia uwierzytelnianie wieloskładnikowe dla użytkowników, którzy są zalogowaniu się z lokalny intranet firmy. Funkcje są dostępne dla dzierżaw Azure AD, które mają licencje Azure AD Premium, Enterprise mobilności Suite lub uwierzytelnianie wieloskładnikowe Azure.


Typ Azure AD dzierżawy| Dostępne opcje zaufane IP
:------------- | :------------- |
Zarządzany|Określone zakresy adresów IP — Administratorzy mogą określać zakres adresów IP, które mogą pomijać uwierzytelnianie wieloskładnikowe dla użytkowników, którzy są podpisywania intranecie firmy.
Federacji|<li>Wszyscy użytkownicy Sfederowane - wszystkich użytkowników federacyjnych podpisywania udostępnianego przez firmę wewnątrz organizacji pominie uwierzytelnianie wieloskładnikowe przy użyciu roszczenia wydawany przez usług AD FS.</li><li>Określone zakresy adresów IP — Administratorzy mogą określać zakres adresów IP, które mogą pomijać uwierzytelnianie wieloskładnikowe dla użytkowników, którzy są podpisywania intranecie firmy.

Pomijanie to działa tylko z w intranecie firmy. Tak na przykład, jeśli tylko niektóre wszystkich użytkowników federacyjnych, a użytkownik rejestruje z spoza sieci intranet, ten użytkownik ma do uwierzytelniania za pomocą uwierzytelnianie wieloskładnikowe, nawet jeśli użytkownik przedstawia roszczeń usług AD FS. W poniższej tabeli opisano, kiedy wieloskładnikowe uwierzytelniania i aplikacji hasła są wymagane, w corpnet Twojej organizacji i poza usługi corpnet, po włączeniu zaufane adresy IP.


|Adresy IP włączone zaufane| Zaufane adresy IP wyłączone
:------------- | :------------- | :------------- |
Wewnętrzna corpnet|Uwierzytelnianie wieloskładnikowe przepływów przeglądarki nie wymagane.|Wymagane uwierzytelnianie wieloskładnikowe przepływów przeglądarki
|W przypadku aplikacji klienta wzbogaconego zwykła haseł działają, jeśli użytkownik nie utworzył hasła aplikacji. Po utworzeniu hasła hasła aplikacji są wymagane.|W przypadku aplikacji klienta wzbogaconego wymagane hasła aplikacji
Poza corpnet|Dla przepływów przeglądarki uwierzytelnianie wieloskładnikowe wymagane.|Dla przepływów przeglądarki uwierzytelnianie wieloskładnikowe wymagane.
|W przypadku aplikacji klienta wzbogaconego wymagane hasła aplikacji.|W przypadku aplikacji klienta wzbogaconego wymagane hasła aplikacji.

### <a name="to-enable-trusted-ips"></a>Aby włączyć zaufany adresy IP

1. Zaloguj się do portalu klasyczny Azure.
2. Po lewej stronie kliknij pozycję usługi Active Directory.
3. W obszarze katalogu kliknij w katalogu, który chcesz skonfigurować zaufane IPsing na.
4. W katalogu jest zaznaczona, kliknij przycisk Konfiguruj.
5. W sekcji uwierzytelnianie wieloskładnikowe kliknij pozycję Ustawienia usługi Zarządzanie.
6. Na stronie Ustawienia usługi w obszarze Zaufane adresy IP wybierz jedną z opcji:

    - Dla żądania od Federacji użytkowników pochodzących z mojej sieci intranet — wszystkie Federacji użytkowników, którzy są zalogowaniu się z siecią firmową pominie uwierzytelnianie wieloskładnikowe przy użyciu roszczenia wydawany przez usług AD FS.
    - Żądania z określonego zakresu publiczne adresy IP — wprowadź w odpowiednich polach, przy użyciu notacji CIDR adresów IP. Na przykład: xxx.xxx.xxx.0/24 dla adresów IP xxx.xxx.xxx.1 zakres — xxx.xxx.xxx.254 lub xxx.xxx.xxx.xxx/32 dla pojedynczego adresu IP. Możesz wprowadzić maksymalnie 50 zakresy adresów IP.

7. Kliknij przycisk Zapisz.
8. Po zastosowaniu aktualizacji kliknij przycisk Zamknij.



![Adresy IP zaufanych](./media/multi-factor-authentication-whats-next/trustedips3.png)




## <a name="app-passwords"></a>Hasła aplikacji

W przypadku niektórych aplikacji, takich jak pakiet Office 2010 lub starszym i Apple Mail nie można używać uwierzytelnianie wieloskładnikowe.  Aby użyć te aplikacje, konieczne będzie używanie "hasła aplikacji" zamiast tradycyjnych hasło.  Hasło aplikacji umożliwia aplikacji pomijać uwierzytelnianie wieloskładnikowe i kontynuować pracę.

>[AZURE.NOTE] Nowoczesna uwierzytelniania dla klientów pakietu Office 2013
>
> Klienci pakietu Office 2013 (z programem Outlook) teraz obsługiwać nowe protokoły uwierzytelniania i mogą mieć dostęp do obsługi uwierzytelniania wieloskładnikowego.  Oznacza to, że po włączeniu hasła aplikacji nie są wymagane do użytku z klientami pakietu Office 2013.  Aby uzyskać więcej informacji wyświetlony [Podgląd publicznej nowoczesny uwierzytelniania pakietu Office 2013 ogłoszenia](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/).



### <a name="important-things-to-know-about-app-passwords"></a>Ważne co należy wiedzieć o hasła aplikacji

Poniżej przedstawiono listę ważnych rzeczy, które należy wiedzieć o hasła aplikacji.

- Użytkownicy mogą mieć wiele haseł aplikacji, co zwiększa powierzchni dla kradzieżą. Ponieważ można zapomnieć hasła aplikacji, może być Zachęć ich osobom zanotuj to. To nie jest zalecane i należy odrzucać ponieważ tylko jeden współczynnik jest wymagane do logowania przy użyciu hasła aplikacji.
- Aplikacje, które pamięci podręcznej hasła i używać go w lokalnej scenariusze może rozpocząć się niepowodzeniem, ponieważ hasło aplikacji jest nieznany poza identyfikator organizacji. Przykład jest wiadomości e-mail programu Exchange, które są w lokalnej, ale zarchiwizowane poczty jest w chmurze. To samo hasło nie działa.
- Rzeczywisty hasło jest generowane automatycznie i nie został dostarczony przez użytkownika. To jest ponieważ automatycznie wygenerowanego hasło jest trudniejsze intruz odgadnięcie i zabezpieczyć.
- Obecnie nie istnieje ograniczenie liczby 40 haseł dla poszczególnych użytkowników. Wyświetli monit o usunięcie jednego z istniejących haseł aplikacji w celu utworzenia nowego.
- Po włączeniu uwierzytelnianie wieloskładnikowe na koncie użytkownika hasła aplikacji można korzystać w większości klientów innych niż przeglądarki, takie jak Outlook i programu Lync, ale nie można wykonać działań administracyjnych za pomocą hasła aplikacji za pomocą aplikacji innych niż przeglądarki, takich jak środowiska Windows PowerShell, nawet jeśli użytkownik ma konta administratora.  Upewnij się, możesz utworzyć konto usługi przy użyciu silnego hasła w celu uruchamiania skryptów programu PowerShell i nie należy włączać tego konta uwierzytelniania wieloskładnikowego.

>[AZURE.WARNING]  Hasła aplikacji nie działają w środowiskach hybrydowych miejsce, w którym klientów komunikować się z obu lokalnego i wykrywania automatycznego punkty końcowe w chmurze. Jest to spowodowane hasła domeny są wymagane do uwierzytelnienia lokalnego i hasła aplikacji są wymagane do uwierzytelniania w chmurze.


### <a name="naming-guidance-for-app-passwords"></a>Nadawanie nazw wskazówki dotyczące haseł aplikacji
Zaleca się, że nazwy hasła aplikacji powinna odzwierciedlać urządzenia, na którym są używane. Na przykład jeśli masz komputer przenośny zawierającą aplikacji innych niż przeglądarki, takie jak Outlook, Word i Excel, wystarczy jedno hasło aplikacji o nazwie komputera przenośnego tworzenie i używanie tego hasła aplikacji we wszystkich tych aplikacjach. Możesz utworzyć osobne hasła dla wszystkie te aplikacje, jednak nie jest zalecane. Sposób zalecane jest użycie jednego hasła aplikacji na urządzeniu.


<center>![Chmury](./media/multi-factor-authentication-whats-next/naming.png)</center>


### <a name="federated-sso-app-passwords"></a>Hasła aplikacji federacyjnych (SSO)
Azure AD obsługuje Federację z lokalnego systemu Windows Server usługach domenowych Active Directory (AD DS). Jeśli Twoja organizacja jest federated(SSO) z usługą Azure Active Directory i użytkownik chce korzystać z uwierzytelnianie wieloskładnikowe Azure, a następnie poniżej jest ważne informacje, że należy pamiętać podczas korzystania z aplikacji hasła. Dotyczy tylko klientów federated(SSO).

- Hasło aplikacji został zweryfikowany przez Azure AD i w związku z tym pomija federacji. Federacja tylko aktywnie jest używana podczas konfigurowania hasło aplikacji.
- Dla użytkowników federated(SSO) firma Microsoft nigdy nie przejdź do dostawcy tożsamości (protokołu IdP) w przeciwieństwie do przepływu pasywne. Hasła znajdują się w polu Identyfikator organizacji. Jeśli użytkownik opuści firmy, tych informacji ma do identyfikator organizacji przy użyciu DirSync w czasie rzeczywistym. Usuwanie Wyłącz konta może potrwać do trzech godzin, aby zsynchronizować, opóźnienia Wyłącz/usuwanie hasła aplikacji w Azure AD.
- Ustawienia kontroli dostępu klienta lokalnego nie są uznane za hasła aplikacji
- Uwierzytelnianie lokalnego rejestrowania / inspekcja funkcja jest dostępna dla hasło aplikacji
- Więcej użytkowników końcowych education jest wymagana dla klienta programu Microsoft Lync 2013. Aby uzyskać procedurę wymagane Zobacz, jak zmienianie hasła w wiadomości e-mail do hasło aplikacji.
- Niektóre zaawansowane projektów architektura może wymagać przy użyciu kombinacji organizacji nazwy użytkownika i hasła oraz hasła aplikacji, używając uwierzytelnianie wieloskładnikowe z klientami, w zależności od tego, gdzie uwierzytelniania. Dla klientów, którzy uwierzytelnić infrastrukturę lokalnego należy użyć organizacji nazwy użytkownika i hasła. Dla klientów, którzy uwierzytelnić Azure AD należy użyć hasło aplikacji.

Załóżmy na przykład, że masz architekturę, która składa się z następujących czynności:

- Są federacyjnego wystąpienia w wersji lokalnej usługi Active Directory z usługą Azure Active Directory
- Korzystasz z programu Exchange online
- W przypadku korzystania z programu Lync, który jest szczególnie lokalnymi
- W przypadku korzystania z uwierzytelnianie wieloskładnikowe Azure


![Proofup](./media/multi-factor-authentication-whats-next/federated.png)

 W tych przypadkach wykonaj następujące czynności:

- Podczas logowania się do programu Lync, za pomocą nazwy użytkownika i hasła Twojej organizacji.
- Próbując uzyskać dostęp do książki adresowej przy użyciu klienta programu Outlook, który łączy się z programem Exchange online, za pomocą hasła.

### <a name="allowing-app-password-creation"></a>Zezwalanie na tworzenie hasła aplikacji
Domyślnie użytkownicy nie mogą tworzyć hasła aplikacji.  Ta funkcja musi być włączona.  Aby zezwolić użytkownikom możliwość tworzenia hasła aplikacji, wykonaj poniższą procedurę.

#### <a name="to-enable-users-to-create-app-passwords"></a>Aby umożliwić użytkownikom tworzenie hasła aplikacji



1. Zaloguj się do portalu klasyczny Azure.
2. Po lewej stronie kliknij pozycję usługi Active Directory.
3. W obszarze katalogu kliknij w katalogu dla użytkownika, które chcesz włączyć.
4. U góry kliknij pozycję Użytkownicy.
5. U dołu strony kliknij pozycję Zarządzaj uwierzytelniania wieloskładnikowego  
6. W górnej części strony uwierzytelnianie wieloskładnikowe kliknij pozycję Ustawienia usługi.
7. Upewnij się, że przycisk radiowy obok pozycji Zezwalaj użytkownikom na tworzenie hasła aplikacji, aby zalogować się do aplikacji przeglądarki nie jest zaznaczone.


![Tworzenie hasła aplikacji](./media/multi-factor-authentication-whats-next/trustedips3.png)

### <a name="creating-app-passwords"></a>Tworzenie hasła aplikacji
Użytkownicy mogą tworzyć hasła aplikacji podczas początkowej rejestracji.  Otrzymują one opcję na końcu procesu rejestracji, która pozwala na tworzenie ich.

Ponadto użytkownicy mogą również tworzyć hasła aplikacji później przez zmianę ustawień w portalu Azure portalu usługi Office 365 lub

### <a name="to-create-app-passwords-in-the-office-365-portal"></a>Tworzenie hasła aplikacji w portalu usługi Office 365
--------------------------------------------------------------------------------


1. Zaloguj się do portalu usługi Office 365
2. W prawym górnym rogu wybierz ustawienia elementu widget
3. Po lewej stronie wybierz dodatkowe Weryfikacja zabezpieczeń
4. Po prawej stronie wybierz opcję **Aktualizuj moje numery telefonów, używany do zabezpieczenia konta**
5. Na stronie proofup u góry wybierz pozycję hasła aplikacji
6. Kliknij przycisk **Utwórz**
7. Wprowadź nazwę hasło aplikacji, a następnie kliknij przycisk **Dalej**
8. Skopiuj hasło aplikacji do Schowka i wklej go w aplikacji.

<center>![Chmury](./media/multi-factor-authentication-whats-next/security.png)</center>


### <a name="to-create-app-passwords-in-the-azure-portal"></a>Tworzenie hasła aplikacji w portalu Azure
--------------------------------------------------------------------------------
1. Zaloguj się do portalu klasyczny Azure.
3. U góry kliknij prawym przyciskiem myszy nazwę użytkownika, a następnie zaznacz dodatkowe Weryfikacja zabezpieczeń.
5. Na stronie proofup u góry wybierz pozycję hasła aplikacji
6. Kliknij przycisk **Utwórz**
7. Wprowadź nazwę hasło aplikacji, a następnie kliknij przycisk **Dalej**
8. Skopiuj hasło aplikacji do Schowka i wklej go w aplikacji.


![Hasła aplikacji](./media/multi-factor-authentication-whats-next/app2.png)

### <a name="to-create-app-passwords-if-you-do-not-have-an-office-365-or-azure-subscription"></a>Tworzenie hasła aplikacji, jeśli nie masz subskrypcji usługi Office 365 lub Azure
--------------------------------------------------------------------------------
1. Zaloguj się do [https://myapps.microsoft.com](https://myapps.microsoft.com)
2. U góry wybierz profil.
3. Kliknij swoją nazwę użytkownika i wybierz dodatkowe Weryfikacja zabezpieczeń.
5. Na stronie proofup u góry wybierz pozycję hasła aplikacji
6. Kliknij przycisk **Utwórz**
7. Wprowadź nazwę hasło aplikacji, a następnie kliknij przycisk **Dalej**
8. Skopiuj hasło aplikacji do Schowka i wklej go w aplikacji.

![Hasła aplikacji](./media/multi-factor-authentication-whats-next/myapp.png)

## <a name="remember-multi-factor-authentication-for-devices-users-trust"></a>Pamiętaj uwierzytelnianie wieloskładnikowe zaufania użytkowników urządzeń

Uwierzytelnianie wieloskładnikowe dokonywaniu dla urządzenia i przeglądarki, że użytkownicy zaufania jest funkcją bezpłatne dla wszystkich użytkowników MFA.  Pozwala udostępnić użytkownikom możliwość obejścia MFA ustalona liczba dni po pomyślnym wykonaniu logowania przy użyciu MFA. Wysoka użyteczności dla użytkowników.

Jednak od użytkowników są dozwolone do zapamiętania MFA dla urządzeń z zaufanych, ta funkcja może zmniejszyć zabezpieczenia konta. Aby zapewnić bezpieczeństwo konta, należy przywrócić uwierzytelnianie wieloskładnikowe dla urządzeń dla jednej z następujących sytuacji:

- Jeśli złamane swoje konto firmowe
- Jeśli urządzenie do zapamiętania zostanie utracony lub kradzież

> [AZURE.NOTE] Ta funkcja jest zaimplementowana jako pamięci podręcznej plików cookie przeglądarki. Jeśli nie są włączone pliki cookie przeglądarki nie działa.

### <a name="how-to-enabledisable-remember-multi-factor-authentication"></a>Jak włączyć i wyłączyć uwierzytelnianie wieloskładnikowe Zapamiętaj

1. Zaloguj się do portalu klasyczny Azure.
2. Po lewej stronie kliknij pozycję usługi Active Directory.
3. W obszarze usługi Active Directory kliknij w katalogu, który chcesz skonfigurować Pamiętaj uwierzytelnianie wieloskładnikowe dla urządzeń.
4. W katalogu jest zaznaczona, kliknij przycisk Konfiguruj.
5. W sekcji uwierzytelnianie wieloskładnikowe kliknij pozycję Ustawienia usługi Zarządzanie.
6. Na stronie Ustawienia usługi w obszarze Zarządzaj ustawieniami urządzenia użytkownika, zaznacz/Usuń zaznaczenie **Zezwalaj użytkownikom do zapamiętania uwierzytelnianie wieloskładnikowe na urządzeniach, które ufać**.
![Należy pamiętać, urządzenia](./media/multi-factor-authentication-whats-next/remember.png)
8. Ustaw liczbę dni, które chcesz umożliwić zawieszenie. Wartość domyślna to 14 dni.
9. Kliknij przycisk Zapisz.
10. Kliknij przycisk Zamknij.


## <a name="selectable-verification-methods"></a>Metody weryfikacji możliwe do wybrania
W chmurze i lokalnych wersji możesz wybrać, które metody weryfikacji są dostępne dla użytkowników. Poniższa tabela zawiera krótkie omówienie każdej z tych metod.

Gdy użytkownicy zarejestrować kont MFA, ich wybierz ich metodę weryfikacji preferowanej zamknąć opcje, które włączono. Wskazówki dotyczące procesu rejestracji omówiono [Konfigurowanie mojego konta na potrzeby weryfikacji dwuetapowej](multi-factor-authentication-end-user-first-time.md)

Metoda|Opis
:------------- | :------------- |
Połączenie na telefon |  Umieszcza automatyczną rozmowy telefon uwierzytelniania. Użytkownik odbiera połączenie i naciska # na klawiaturze telefonu do uwierzytelnienia. Ten numer telefonu nie jest synchronizowane z usługą Active Directory w lokalnej.
Wiadomości SMS na telefonie | Wysyła wiadomość tekstowa zawierających kod weryfikacyjny użytkownikowi. Monit o albo odpowiadanie na wiadomości tekstowej kod weryfikacyjny lub wprowadź kod weryfikacyjny w interfejsie logowania.
Powiadomienie o za pośrednictwem aplikacji dla urządzeń przenośnych | W tym trybie aplikacji Microsoft Authenticator zapobiega możliwość uzyskania nieautoryzowanego dostępu do konta i przestaje transakcje fałszywe. Można to zrobić przy użyciu powiadomienia wypychane do Twojego telefonu lub urządzenia zarejestrowane. Po prostu Wyświetl powiadomienie, a jeśli jest godny zaufania naciśnij pozycję Weryfikuj. W przeciwnym razie może wybierz pozycję Odmów, lub wybierz opcję odmówić i report fałszywej powiadomienie. Aby uzyskać informacje na raportowanie fałszywej powiadomienia Zobacz jak używać funkcji oszustwa raportu i Odmów uwierzytelniania wieloskładnikowego.</br></br>Aplikacja Microsoft Authenticator jest dostępna dla [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072)i [IOS](http://go.microsoft.com/fwlink/?Linkid=825073).|
Kod weryfikacyjny z aplikacji dla urządzeń przenośnych | W tym trybie aplikacji Microsoft Authenticator można używać jako token oprogramowania do generowania PRZYSIĘGĄ kod weryfikacyjny. Następnie można wprowadzić ten kod weryfikacyjny wraz z nazwy użytkownika i hasło, aby zapewnić drugiego formularza uwierzytelniania.</li><br><p> Aplikacja Microsoft Authenticator jest dostępna dla [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072)i [IOS](http://go.microsoft.com/fwlink/?Linkid=825073).

### <a name="how-to-enabledisable-authentication-methods"></a>Jak włączyć i wyłączyć metody uwierzytelniania

1. Zaloguj się do portalu klasyczny Azure.
2. Po lewej stronie kliknij pozycję usługi Active Directory.
3. W obszarze usługi Active Directory kliknij w katalogu, który chcesz włączyć lub wyłączyć metody uwierzytelniania.
4. W katalogu jest zaznaczona, kliknij przycisk Konfiguruj.
5. W sekcji uwierzytelnianie wieloskładnikowe kliknij pozycję Ustawienia usługi Zarządzanie.
6. Na stronie Ustawienia usługi w obszarze Opcje weryfikacji Zaznacz/Usuń zaznaczenie opcji, którego chcesz użyć.</br></br>
![Opcje weryfikacji](./media/multi-factor-authentication-whats-next/authmethods.png)
9. Kliknij przycisk Zapisz.
10. Kliknij przycisk Zamknij.
