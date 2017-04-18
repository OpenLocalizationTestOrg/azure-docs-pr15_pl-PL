<properties 
    pageTitle="Wprowadzenie do serwera uwierzytelniania wieloskładnikowego Azure"
    description="To jest strona uwierzytelnianie wieloskładnikowe Azure opisujący sposób rozpocząć pracę z serwerem MFA Azure."
    services="multi-factor-authentication"
    keywords="Uwierzytelnianie serwera azure czynniki uwierzytelniania aplikacji aktywacji wielostronicowy, uwierzytelniania serwera pobierania"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/15/2016"
    ms.author="kgremban"/>

# <a name="getting-started-with-the-azure-multi-factor-authentication-server"></a>Wprowadzenie do serwera uwierzytelniania wieloskładnikowego Azure




<center>![Chmury](./media/multi-factor-authentication-get-started-server/server2.png)</center>

Teraz, gdy mamy ustaleniu, czy używać lokalnego uwierzytelnianie wieloskładnikowe, korzystaj rozpoczęcie pracy. Ta strona obejmuje nowej instalacji serwera i wprowadzenie go ustawienia z usługą Active Directory w lokalnej. Jeśli został już zainstalowany Server PhoneFactor i odnaleźć Aby uaktualnić, zobacz [Uaktualnianie na serwerze wieloskładnikowe Azure](multi-factor-authentication-get-started-server-upgrade.md) lub jeśli szukasz informacji na temat instalowania tylko usługi sieci web zobacz [Wdrażanie usługi Azure wieloskładnikowe uwierzytelniania serwera Mobile aplikacji sieci Web](multi-factor-authentication-get-started-server-webservice.md).


## <a name="download-the-azure-multi-factor-authentication-server"></a>Pobierz Server Azure uwierzytelnianie wieloskładnikowe



Istnieją dwa sposoby pobrać serwer uwierzytelniania wieloskładnikowego Azure. Oba jest wykonywane przez Azure portal. Pierwsza jest korzystając z funkcji zarządzania dostawcy uwierzytelnianie wieloskładnikowe bezpośrednio. Druga jest za pomocą ustawień usługi. Jest to druga opcja wymaga dostawcy uwierzytelnianie wieloskładnikowe lub licencji usługi Azure MFA, Azure AD Premium lub pakietu mobilności przedsiębiorstwa.


### <a name="to-download-the-azure-multi-factor-authentication-server-from-the-azure-portal"></a>Aby pobrać serwera uwierzytelnianie wieloskładnikowe Azure za pomocą portalu Azure
--------------------------------------------------------------------------------

1. Zaloguj się do portalu Azure jako Administrator.
2. Po lewej stronie wybierz pozycję usługi Active Directory.
3. Na stronie usługi Active Directory u góry kliknij **Dostawców uwierzytelniania wieloskładnikowego**
4. U dołu kliknij pozycję **Zarządzaj**
5. Spowoduje to otwarcie nowej strony.  Kliknij pozycję **do pobrania.** 
 ![Pobierz](./media/multi-factor-authentication-sdk/download.png)
6. Nad **Wygenerować poświadczeń aktywacji**, kliknij **Pobieranie.** 
 ![Pobierz](./media/multi-factor-authentication-get-started-server/download4.png)
7. Zapisz plik do pobrania.



### <a name="to-download-the-azure-multi-factor-authentication-server-via-the-service-settings"></a>Aby pobrać z serwerem uwierzytelnianie wieloskładnikowe Azure za pomocą ustawień usługi


1. Zaloguj się do portalu Azure jako Administrator.
2. Po lewej stronie wybierz pozycję usługi Active Directory.
3. Kliknij dwukrotnie wystąpienia programu Azure AD.
4. U góry kliknij przycisk **Konfiguruj**
![Pobierz](./media/multi-factor-authentication-sdk/download2.png)
5. W obszarze uwierzytelnianie wieloskładnikowe wybierz pozycję **Ustawienia usługi Zarządzanie**
6. Na stronie Ustawienia usług u dołu ekranu kliknij przycisk **Przejdź do portalu**.
![Plik do pobrania](./media/multi-factor-authentication-get-started-server/servicesettings.png)
7. Spowoduje to otwarcie nowej strony. Kliknij pozycję **do pobrania.**
8. Nad **Wygenerować poświadczeń aktywacji**, kliknij **Pobieranie.**
9. Zapisz plik do pobrania.




## <a name="install-and-configure-the-azure-multi-factor-authentication-server"></a>Instalowanie i konfigurowanie serwera Azure uwierzytelnianie wieloskładnikowe
Teraz, gdy pobrano serwera można zainstalować i skonfigurować go.  Upewnij się, że serwer, z którym instalujesz na spełnia następujące wymagania:



Wymagania dotyczące serwera Azure uwierzytelnianie wieloskładnikowe|Opis|
:------------- | :------------- |
Sprzętowe|<li>200 MB miejsca na dysku twardym</li><li>procesor do x32 lub x64</li><li>1 GB lub większa pamięci RAM</li>
Oprogramowanie|<li>Windows Server 2008 lub nowszego, jeśli host jest serwerem systemu operacyjnego</li><li>Windows 7 lub nowszy, jeśli host jest klientem systemu operacyjnego</li><li>Program Microsoft .NET 4.0 Framework</li><li>Zestaw SDK programu IIS 7.0 lub nowszy, jeśli podczas instalowania usługi sieci web lub portalu użytkownika</li>

### <a name="azure-multi-factor-authentication-server-firewall-requirements"></a>Azure wymagań dotyczących zapory serwera uwierzytelniania wieloskładnikowego
--------------------------------------------------------------------------------
Każdy serwer MFA muszą mieć możliwość komunikowania się na porcie 443 wychodzące do następujących czynności:

- https://pfd.phonefactor.NET
- https://pfd2.phonefactor.NET
- https://CSS.phonefactor.NET

Jeśli ruchu wychodzącego zapory są ograniczone na porcie 443, muszą być otwarte z następujących zakresów adresów IP:

Podsieć adresów IP|Maska sieci|Zakres adresów IP
:------------- | :------------- | :------------- |
134.170.116.0/25|255.255.255.128|134.170.116.1 — 134.170.116.126
134.170.165.0/25|255.255.255.128|134.170.165.1 — 134.170.165.126
70.37.154.128/25|255.255.255.128|70.37.154.129 — 70.37.154.254

Jeśli nie używasz funkcji potwierdzenia zdarzenia uwierzytelnianie wieloskładnikowe Azure i jeśli użytkownicy nie mają uwierzytelniania w aplikacji dla urządzeń przenośnych uwierzytelnianie wieloskładnikowe z urządzeń w sieci firmowej IP zakresów można zmniejszyć do następującego:


Podsieć adresów IP|Maska sieci|Zakres adresów IP
:------------- | :------------- | :------------- |
134.170.116.72/29|255.255.255.248|134.170.116.72 — 134.170.116.79
134.170.165.72/29|255.255.255.248|134.170.165.72 — 134.170.165.79
70.37.154.200/29|255.255.255.248|70.37.154.201 — 70.37.154.206


### <a name="to-install-and-configure-the-azure-multi-factor-authentication-server"></a>Aby zainstalować i skonfigurować serwer Azure uwierzytelnianie wieloskładnikowe
--------------------------------------------------------------------------------


1. Kliknij dwukrotnie plik wykonywalny. Spowoduje to rozpocząć instalację.
2. Na ekranie Wybierz Folder instalacji upewnij się, że folder jest poprawny, a następnie kliknij przycisk Dalej.
3. Po ukończyć instalacji, kliknij przycisk Zakończ.  Zostanie uruchomiony Kreator konfiguracji.
4. W Kreatorze konfiguracji ekran powitalny, należy zaznaczyć **Pomiń za pomocą Kreatora konfiguracji uwierzytelniania** , a następnie kliknij przycisk **Dalej**.  To zamknąć kreatora i uruchomić serwer.
    ![Chmury](./media/multi-factor-authentication-get-started-server/skip2.png)
5. Ponownie na stronie, możemy pobranych serwera kliknij przycisk **Generuj poświadczeń aktywacji** .  Skopiuj te informacje do serwera MFA Azure w odpowiednich polach, a następnie kliknij przycisk **Aktywuj**.


Powyższe czynności Pokaż Instalacja ekspresowa przy użyciu Kreatora konfiguracji.  Można ponownie uruchomić Kreatora uwierzytelniania, wybierając go w menu Narzędzia na serwerze.



##<a name="import-users-from-active-directory"></a>Importowanie użytkowników z usługi Active Directory

Teraz, gdy serwer jest zainstalowana i skonfigurowana możesz szybko zaimportować użytkowników na serwerze MFA Azure.

### <a name="to-import-users-from-active-directory"></a>Aby zaimportować użytkowników z usługi Active Directory
--------------------------------------------------------------------------------


1. W polu Serwer MFA Azure po lewej stronie wybierz **użytkowników**.
2. U dołu wybierz pozycję **Importuj z usługi Active Directory**.
3. Teraz można wyszukiwać poszczególnych użytkowników lub wyszukiwania w katalogu AD dla OU użytkownikom w nich.  W tym przypadku określono użytkownikom jednostce Organizacyjnej.
4. Zaznacz wszystkich użytkowników po prawej stronie, a następnie kliknij przycisk **Importuj**.  Powinien zostać wyświetlony wyświetla komunikat informujący o tym, że możesz zostały pomyślnie.  Zamknij okno Importowanie.

![Chmury](./media/multi-factor-authentication-get-started-server/import2.png)

## <a name="send-users-an-email"></a>Wyślij do użytkowników wiadomość e-mail
Teraz, gdy użytkownicy zostały zaimportowane do serwera uwierzytelnianie wieloskładnikowe Azure, zaleca się Wyślij do użytkowników wiadomość e-mail, informujące o tym, że masz została zarejestrowana w uwierzytelnianie wieloskładnikowe.

Z serwerem uwierzytelnianie wieloskładnikowe Azure istnieją różne sposoby konfigurowania użytkowników dotyczące korzystania z uwierzytelnianie wieloskładnikowe.  Na przykład jeśli znasz numerów telefonów użytkowników lub zostały zaimportować numerów telefonów do serwera uwierzytelniania wieloskładnikowego Azure z katalogu firmy, wiadomość e-mail będzie użytkownicy wiedzieć, że zostały skonfigurowane za pomocą uwierzytelnianie wieloskładnikowe Azure, zawiera kilka instrukcji na temat korzystania z uwierzytelnianie wieloskładnikowe Azure i informować użytkownika o numer telefonu, który otrzymają ich uwierzytelnienia na.  

Zawartość tej wiadomości e-mail różnią się w zależności od metody uwierzytelniania, który został ustawiony dla użytkownika (przykład rozmowy telefonicznej, SMS, aplikacji dla urządzeń przenośnych).  Na przykład jeśli użytkownik jest wymagany podczas uwierzytelniania za pomocą numeru PIN, wiadomości e-mail informuje o ich co ich początkowego numeru PIN została ustawiona.  Użytkownicy muszą zazwyczaj zmianę ich numeru PIN podczas jego pierwszego uwierzytelniania.

Jeśli nie zostały skonfigurowane lub zaimportowane do serwera uwierzytelniania wieloskładnikowego Azure numerów telefonów użytkowników lub użytkowników jest wstępnie skonfigurowana do uwierzytelniania za pomocą aplikacji dla urządzeń przenośnych, możesz je Wyślij wiadomość e-mail, umożliwiającego informacją, że zostały skonfigurowane do uwierzytelniania wieloskładnikowego Azure i kierujące ich do wykonania ich rejestracji konta portalu użytkownika uwierzytelnianie wieloskładnikowe Azure.  Hiperłącza zostaną uwzględnione, który po kliknięciu w celu dostępu do portalu użytkownika. Gdy użytkownik kliknie hiperłącze, przeglądarki sieci web Otwórz i podjęcie Portal użytkownika uwierzytelnianie wieloskładnikowe Azure firmy.   


### <a name="configuring-email-and-email-templates"></a>Konfigurowanie poczty e-mail i szablony wiadomości e-mail

Klikając ikonę wiadomości e-mail po lewej stronie możesz skonfigurować ustawienia wysyłania tych wiadomości e-mail.  Jest to miejsce, w którym można wprowadzić informacje SMTP serwera poczty i umożliwia wysłanie zbiorcze wielu wiadomości e-mail, dodając wyboru do wysyłania wiadomości pocztowych pole wyboru użytkownicy.

![Ustawienia poczty e-mail](./media/multi-factor-authentication-get-started-server/email1.png)

Na karcie zawartości wiadomości E-mail będą widoczne wszystkie różne szablony wiadomości e-mail, które są dostępne do wyboru.  Aby w zależności od tego, jak skonfigurowano użytkownikom korzystanie z uwierzytelnianie wieloskładnikowe, można wybrać szablon najlepiej pasuje.

![Szablony wiadomości e-mail](./media/multi-factor-authentication-get-started-server/email2.png)

## <a name="how-the-azure-multi-factor-authentication-server-handles-user-data"></a>Jak serwer uwierzytelniania wieloskładnikowego Azure obsługuje dane użytkownika

Gdy używasz uwierzytelnianie wieloskładnikowe (MFA) lokalnego serwera danych użytkownika są przechowywane w lokalne serwery. Dane nie trwałych użytkownika są przechowywane w chmurze. Gdy użytkownik wykonuje uwierzytelnianie dwuskładnikowe, serwer MFA wysyła dane do usługi Azure MFA w chmurze do uwierzytelniania. Te żądania uwierzytelniania są wysyłane do usługi w chmurze, następujące pola są wysyłane żądanie i dzienniki, aby były dostępne w raportach uwierzytelniania zastosowania klienta. Niektóre pola są opcjonalne, więc może być włączona lub wyłączona na serwerze uwierzytelnianie wieloskładnikowe. Komunikacja z serwerem MFA usłudze w chmurze MFA używa SSL/TLS na porcie 443 ruchu wychodzącego. Te pola są:

- Unikatowy identyfikator — nazwa użytkownika lub identyfikator wewnętrzny serwera MFA
- Pierwszy i nazwisko — opcjonalne
- Adres e-mail — opcjonalne
- Numer telefonu - w trakcie rozmowy lub uwierzytelniania wiadomości SMS
- Token urządzenia — podczas uwierzytelniania aplikacji dla urządzeń przenośnych
- Tryb uwierzytelniania
- Wynik uwierzytelniania
- Nazwa serwera MFA
- Serwer MFA IP
- Klienta IP — Jeśli jest dostępna



Oprócz powyższych pól wynik uwierzytelniania (sukcesu odmowa) i powód dowolnego liczba odmów jest również przechowywane dane uwierzytelniania i dostępne za pośrednictwem uwierzytelniania i raporty.


## <a name="advanced-azure-multi-factor-authentication-server-configurations"></a>Zaawansowane konfiguracji serwera Azure uwierzytelnianie wieloskładnikowe
Aby uzyskać dodatkowe informacje na temat Zaawansowane ustawienia i informacje o konfiguracji skorzystaj z poniższej tabeli.

Metoda|Opis
:------------- | :------------- |
[Portal użytkownika](multi-factor-authentication-get-started-portal.md)|  Informacje o konfiguracji i konfigurowanie tym wdrożenia i użytkownika samoobsługowego portalu użytkownika.
[Usługi federacyjne Active Directory](multi-factor-authentication-get-started-adfs.md)|Informacje na temat konfigurowania uwierzytelnianie wieloskładnikowe Azure z usług AD FS.
[Uwierzytelnianie RADIUS](multi-factor-authentication-get-started-server-radius.md)|  Informacje dotyczące konfigurowania i konfigurowanie serwera MFA Azure o PROMIENIU.
[Uwierzytelnianie usług IIS](multi-factor-authentication-get-started-server-iis.md)|Informacje dotyczące konfigurowania i konfigurowanie serwera MFA Azure za pomocą usług IIS.
[Uwierzytelnianie systemu Windows](multi-factor-authentication-get-started-server-windows.md)|  Informacje dotyczące konfigurowania i konfigurowanie serwera MFA Azure za pomocą uwierzytelniania systemu Windows.
[Uwierzytelnianie LDAP](multi-factor-authentication-get-started-server-ldap.md)|Informacje dotyczące konfigurowania i konfigurowanie serwera MFA Azure za pomocą uwierzytelniania LDAP.
[Zdalny pulpit bramy i serwer uwierzytelniania wieloskładnikowego Azure za pomocą RADIUS](multi-factor-authentication-get-started-server-rdg.md)|  Informacje dotyczące konfigurowania i konfigurowanie serwera MFA Azure za pomocą bramy usług pulpitu zdalnego przy użyciu usługi RADIUS.
[Synchronizacja z usługą Active Directory systemu Windows Server](multi-factor-authentication-get-started-server-dirint.md)|Informacje dotyczące konfigurowania i Konfigurowanie synchronizacji między usługą Active Directory i serwer Azure MFA.
[Wdrażanie usługi sieci Web dla urządzeń przenośnych aplikacji Server Azure uwierzytelnianie wieloskładnikowe](multi-factor-authentication-get-started-server-webservice.md)|Informacje dotyczące konfigurowania i konfigurowanie usługi sieci web server Azure MFA.
