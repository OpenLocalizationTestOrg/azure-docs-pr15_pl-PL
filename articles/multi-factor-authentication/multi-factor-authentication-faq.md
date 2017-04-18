<properties
    pageTitle="Uwierzytelnianie wieloskładnikowe Azure — często zadawane pytania"
    description="Zawiera listę często zadawane pytania i odpowiedzi dotyczące uwierzytelnianie wieloskładnikowe Azure. Uwierzytelnianie wieloskładnikowe jest metodę weryfikacji tożsamości użytkownika, który wymaga więcej niż nazwy użytkownika i hasła. Zapewnia dodatkową warstwę zabezpieczeń do logowania użytkownika i transakcji."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/13/2016"
    ms.author="kgremban"/>

# <a name="azure-multi-factor-authentication-faq"></a>Uwierzytelnianie wieloskładnikowe Azure — często zadawane pytania


Niniejszy artykuł zawiera odpowiedzi na często zadawane pytania dotyczące uwierzytelnianie wieloskładnikowe Azure i korzystania z usługi uwierzytelnianie wieloskładnikowe, w tym pytania dotyczące rozliczeń modelu i użyteczności.

## <a name="general"></a>Ogólne

**P: jak serwer uwierzytelniania wieloskładnikowego Azure obsługiwać dane użytkowników?**

Za pomocą serwera uwierzytelnianie wieloskładnikowe dane użytkownika znajduje się tylko na serwerach lokalnego. Dane nie trwałych użytkownika są przechowywane w chmurze. Gdy użytkownik wykona Weryfikacja dwuetapowa, serwer uwierzytelniania wieloskładnikowego wysyła dane do usług w chmurze uwierzytelnianie wieloskładnikowe Azure uwierzytelniania. Komunikacji między komputerami serwerów uwierzytelnianie wieloskładnikowe i usługi w chmurze uwierzytelnianie wieloskładnikowe używa Secure Sockets Layer (SSL) lub zabezpieczeń TLS (Transport Layer) na porcie 443 ruchu wychodzącego.

Gdy żądania uwierzytelniania są wysyłane do usługi w chmurze, dane są zbierane dla uwierzytelniania i zastosowania raportów. Pola danych, zawarte w dziennikach Weryfikacja dwuetapowa są następujące:

- **Unikatowy identyfikator** (albo nazwę lub lokalnego wieloskładnikowe uwierzytelniania serwera identyfikator użytkownika)
- **Imię i nazwisko** (opcjonalnie)
- **Adres e-mail** (opcjonalnie)
- **Numer telefonu** (w przypadku korzystania z połączenia głosowego lub uwierzytelniania usługi SMS)
- **Token urządzenia** (w przypadku korzystania z aplikacji dla urządzeń przenośnych uwierzytelniania)
- **Tryb uwierzytelniania**
- **Wynik uwierzytelniania**
- **Nazwa serwera uwierzytelnianie wieloskładnikowe**
- **Uwierzytelnianie wieloskładnikowe serwera adresów IP**
- **Klienta IP** (jeśli jest dostępne)

Pola opcjonalne można skonfigurować na serwerze uwierzytelnianie wieloskładnikowe.

Wynik weryfikacji (sukcesu lub odmowa), a przyczyny, jeśli ją odmowa, jest przechowywane razem z danymi uwierzytelniania i jest dostępna w raportach uwierzytelniania i użycia zasobów.


## <a name="billing"></a>Rozliczenia

Większość pytań rozliczeń można odpowiedzi, odwołując się do [ceny uwierzytelnianie wieloskładnikowe strony](https://azure.microsoft.com/pricing/details/multi-factor-authentication/).

**P: czy obciążona opłatą za połączenia telefoniczne i wiadomości SMS w organizacji jest używane do uwierzytelnienia moich użytkowników?**

Organizacje nie jest naliczany dla poszczególnych połączeń telefonicznych umieszczony lub tekstu wiadomości wysyłane do użytkowników za pomocą uwierzytelniania wieloskładnikowego Azure. Właściciele telefonu może zostać obciążony połączenia telefoniczne i wiadomości SMS, które otrzymują, zgodnie z ich osobistych telefoniczna.

**P: czy opłaty modelu rozliczeń użytkownika na podstawie liczba użytkowników, którzy są skonfigurowane do używania uwierzytelnianie wieloskładnikowe lub liczba użytkowników, którzy wykonują weryfikacji?**

Rozliczenia jest oparty na liczbę użytkowników skonfigurowany do używania uwierzytelnianie wieloskładnikowe.

**P: jak działa rozliczeń uwierzytelnianie wieloskładnikowe?**

Kiedy używać "na użytkownika" lub "uwierzytelniania" model, Azure MFA jest oparte na zużycie zasobów. Wszelkie opłaty są zafakturowane do subskrypcji Azure organizacji tak jak maszyn wirtualnych witryn sieci Web, itp.

Użycie modelu licencji, licencje uwierzytelnianie wieloskładnikowe Azure są zakupionych, a następnie przypisane do użytkowników, podobnie jak dla usługi Office 365 i innych produktów subskrypcji.

**P: czy istnieje bezpłatną wersję programu uwierzytelnianie wieloskładnikowe Azure dla administratorów?**

W niektórych przypadkach tak. Uwierzytelnianie wieloskładnikowe dla administratorów Azure udostępnia podzestaw funkcji Azure MFA bezpłatnie. Ta oferta dotyczy do członków grupy administratorów globalnych Azure w wystąpienia usługi Azure Active Directory, które nie są połączone z dostawcą oparte na zużycie uwierzytelnianie wieloskładnikowe Azure. Używanie dostawcy uwierzytelnianie wieloskładnikowe uaktualnienie wszystkich administratorów i użytkowników w katalogu, którzy są skonfigurowane do pełnej wersji programu uwierzytelnianie wieloskładnikowe Azure za pomocą uwierzytelnianie wieloskładnikowe.

**P: czy istnieje bezpłatną wersję programu uwierzytelnianie wieloskładnikowe Azure dla użytkowników usługi Office 365?**

W niektórych przypadkach tak. Uwierzytelnianie wieloskładnikowe usługi Office 365 oferuje podzestaw funkcji Azure MFA bezpłatnie. Ta oferta dotyczy użytkowników, którzy mają licencji usługi Office 365 przydzielone, gdy nie została połączona z dostawcą uwierzytelnianie wieloskładnikowe Azure oparte na zużycie do odpowiednich wystąpienia usługi Azure Active Directory. Korzystanie z dostawcy uwierzytelnianie wieloskładnikowe uaktualnienie wszystkich administratorów i użytkowników w katalogu, którzy są skonfigurowane do pełnej wersji programu uwierzytelnianie wieloskładnikowe Azure za pomocą uwierzytelnianie wieloskładnikowe.

**P: czy mojej organizacji przełączać się między użytkownika na uwierzytelniania zużycie rozliczeń modeli i w dowolnym momencie?**

Twoja organizacja wybiera modelu rozliczeń, podczas tworzenia zasobu. Nie można zmienić modelu rozliczeń, po zainicjowano obsługę administracyjną tego zasobu. Można jednak tworzyć innego zasobu uwierzytelnianie wieloskładnikowe, aby zamienić oryginał. Opcje konfiguracji i ustawień użytkownika nie można przenieść do nowego zasobu.

**P: czy mojej organizacji przełączać się między zużycie rozliczeń i model licencji w dowolnym momencie?**

Po dodaniu licencji do katalogu, w którym jest już dostawcą uwierzytelnianie wieloskładnikowe Azure użytkownika oparte na zużycie rozliczeń zostanie zmniejszona przez liczbę posiadanych licencji. Jeśli wszyscy użytkownicy skonfigurowany do używania uwierzytelnianie wieloskładnikowe mają przypisane licencje, administrator usunąć dostawcę uwierzytelnianie wieloskładnikowe Azure.

Nie można mieszać rozliczeń zużycie na uwierzytelniania z modelem licencji. Gdy dostawca uwierzytelnianie wieloskładnikowe na uwierzytelniania jest połączony z katalogu, organizacji jest wystawiona dla wszystkich żądań weryfikacji uwierzytelnianie wieloskładnikowe, bez względu na to żadnych posiadanych licencji.

**P: czy mojej organizacji są dostępne za pomocą i zsynchronizować tożsamości uwierzytelniania wieloskładnikowego Azure?**

Jeśli organizacja korzysta z modelu rozliczeń zużycie, usługi Azure Active Directory nie jest wymagane. Łączenie z dostawcą uwierzytelnianie wieloskładnikowe z katalogu jest opcjonalna. Jeśli Twoja organizacja nie jest połączony z katalogu, go wdrożyć serwer uwierzytelniania wieloskładnikowego Azure lub SDK uwierzytelnianie wieloskładnikowe Azure lokalnego.

Azure Active Directory jest wymagany dla modelu licencji, ponieważ licencje są dodawane do katalogu, gdy zakupu i przypisanie ich do użytkowników w katalogu.


## <a name="usability"></a>Użyteczności

**P: co użytkownika robi Jeśli ta osoba nie otrzyma odpowiedź na jej telefon lub jeśli telefon nie jest dostępne dla użytkownika?**

Jeśli użytkownik skonfigurował kopii zapasowej telefonu, one spróbuj ponownie i wybierz pozycję tego telefonu po wyświetleniu monitu na stronie logowania. Jeśli użytkownik nie ma formę skonfigurowane, administrator organizacji może zaktualizować numer podstawowy numer telefonu użytkownika.


**P: co administrator zrobić, jeśli administrator o konto użytkownika nie może uzyskać dostępu do kontaktów użytkownika?**

Administrator zresetować konta użytkownika, prosząc o przechodzą przez proces rejestracji ponownie. Dowiedz się więcej na temat [zarządzania użytkownika i ustawień urządzeń z uwierzytelnianie wieloskładnikowe Azure w chmurze](multi-factor-authentication-manage-users-and-devices.md).

**P: co administrator robi Jeśli telefonu użytkownika, który jest za pomocą hasła aplikacji zostanie utracony lub kradzież?**

Administrator może usunąć hasła aplikacji wszystkich użytkowników, aby zabezpieczyć przed nieautoryzowanym dostępem. Gdy użytkownik ma urządzenia zastępczego, użytkownika można odtworzyć hasła. Dowiedz się więcej na temat [zarządzania użytkownika i ustawień urządzeń z uwierzytelnianie wieloskładnikowe Azure w chmurze](multi-factor-authentication-manage-users-and-devices.md).

**P: co zrobić, jeśli użytkownik nie można zalogować się do aplikacji innych niż przeglądarki?**

Użytkownik, który jest skonfigurowany do używania uwierzytelnianie wieloskładnikowe wymaga hasła do zalogowania się do niektórych aplikacji innych niż przeglądarki. Użytkownik chce Wyczyść (Usuń) informacji o logowaniu, ponownie uruchom aplikację i zaloguj się przy użyciu ich nazwę i aplikacji hasła użytkownika.

Uzyskaj więcej informacji o tworzeniu hasła aplikacji i innych [pomocy hasła aplikacji](multi-factor-authentication-end-user-app-passwords.md).


>[AZURE.NOTE] Nowoczesna uwierzytelniania dla klientów pakietu Office 2013
>
> Klienci pakietu Office 2013 (w tym programu Outlook) obsługują nowe protokoły uwierzytelniania. Możesz skonfigurować pakiet Office 2013 do obsługi uwierzytelniania wieloskładnikowego. Po skonfigurowaniu pakietu Office 2013 hasła aplikacji nie są wymagane dla klientów pakietu Office 2013. Aby uzyskać więcej informacji, zobacz [powiadomienia publicznej Podgląd nowoczesny uwierzytelniania pakietu Office 2013](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/).

**P: co użytkownika robi Jeśli użytkownik nie odbiera wiadomości SMS, czy użytkownik odpowiada wiadomości SMS dwukierunkowe, ale Weryfikacja przekroczenie limitu czasu?**

Dostarczanie wiadomości tekstowych i otrzymania odpowiedzi w dwukierunkowe SMS nie jest gwarantowana, ponieważ nie można jej kontrolować czynników, które mogą mieć wpływ na niezawodność usługi. Następujących czynników obejmują kraju docelowym, carrier telefonu komórkowego i sygnał.

Użytkownicy, którzy występują trudności w wiarygodny sposób odbierania wiadomości SMS należy zamiast tego wybierz metodę urządzeń przenośnych aplikacji lub rozmowy telefonicznej. Aplikacji dla urządzeń przenośnych, można otrzymywać powiadomienia zarówno przez sieć komórkowa i sieci Wi-Fi połączenia. Ponadto aplikacji dla urządzeń przenośnych mogą powodować zwrócenie kody weryfikacji, nawet wtedy, gdy urządzenie nie ma w ogóle nie ma sygnału. Aplikacja Microsoft Authenticator jest dostępna dla [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072)i [IOS](http://go.microsoft.com/fwlink/?Linkid=825073).

Jeśli musisz użyć wiadomości SMS, zalecamy używanie jednokierunkowe SMS zamiast dwukierunkowe SMS, jeśli to możliwe. Jednokierunkowe SMS są bardziej niezawodne i uniemożliwia użytkownikom naliczania globalnej opłat SMS na odpowiadanie na wiadomości SMS wysłanej z innego kraju.


**P: czy mogę używać tokeny sprzętowe z serwerem uwierzytelnianie wieloskładnikowe Azure**

Jeśli korzystasz z serwera uwierzytelniania wieloskładnikowego Azure, można zaimportować tokeny hasło opartych na czasie, jednorazowego (TOTP) otwórz uwierzytelniania (PRZYSIĘGĄ) innych firm i używać ich na potrzeby weryfikacji dwuetapowej.

Możesz użyć tokeny ActiveIdentity, które są TOTP PRZYSIĘGĄ tokeny po umieszczeniu pliku klucza tajnego w pliku CSV i zaimportować do serwera uwierzytelniania wieloskładnikowego Azure. Możesz użyć PRZYSIĘGĄ tokenów z Active Directory Federation Services (ADFS), zdalnego uwierzytelniania telefonicznych użytkownika Service (RADIUS) po system klienta można przetwarzania odpowiedzi wezwanie programu access i uwierzytelniania opartego na formularzach Internet Information Server (IIS).

Możesz zaimportować innych TOTP PRZYSIĘGĄ tokeny w następujących formatach:  
- Portable kontenera kluczy symetrycznej (PSKC)  
- CSV, jeśli plik zawiera liczbę kolejną, klucz tajny w formacie Base 32 i przedział czasu  

**P: czy mogę używać serwera uwierzytelniania wieloskładnikowego Azure do zabezpieczenia usługi terminalowe**

Tak, ale jeśli przy użyciu systemu Windows Server 2012 R2 lub później, tylko za pomocą bramy usług pulpitu zdalnego (RD bramy).

Zmiany zabezpieczeń w systemie Windows Server 2012 R2 zmieniono sposób, w jaki serwer uwierzytelniania wieloskładnikowego Azure łączy do pakietu zabezpieczeń urząd zabezpieczeń lokalnych (LSA) w systemie Windows Server 2012 i starszych wersjach. Dla wersji usługi terminalowe w systemie Windows Server 2012 lub starszym możesz [zabezpieczyć aplikację z uwierzytelniania systemu Windows](multi-factor-authentication-get-started-server-windows.md#to-secure-an-application-with-windows-authentication-use-the-following-procedure). Jeśli korzystasz z systemu Windows Server 2012 R2, należy bramy usług pulpitu zdalnego.

**P: Dlaczego użytkownik otrzyma połączenia uwierzytelnianie wieloskładnikowe z anonimowy rozmówca po skonfigurowaniu identyfikator rozmówcy?**

Po umieszczeniu uwierzytelnianie wieloskładnikowe połączenia za pomocą publicznej sieci telefonicznej, czasami routingu przewoźnika, która nie obsługuje identyfikator rozmówcy. Z tego powodu identyfikator rozmówcy nie jest gwarantowana, mimo że zawsze wysyła system uwierzytelnianie wieloskładnikowe.


## <a name="errors"></a>Błędy

**P: co użytkownicy zrobić, jeśli zobaczą komunikat o błędzie "żądanie uwierzytelnienia jest aktywny konta" podczas korzystania z aplikacji dla urządzeń przenośnych powiadomienia?**

Udzielić im wykonaj poniższą procedurę, aby usunąć swoje konto z aplikacji dla urządzeń przenośnych, a następnie dodaj go ponownie:

1. Przejdź do [profilu portal Azure](https://account.activedirectory.windowsazure.com/profile/) i zaloguj się przy użyciu konta organizacyjnego.
2. Wybierz pozycję **Weryfikacja dodatkowe zabezpieczenia**.
4. Usuń istniejące konto z aplikacji dla urządzeń przenośnych.
5. Kliknij przycisk **Konfiguruj**, a następnie postępuj zgodnie z instrukcjami, aby skonfigurować aplikacji dla urządzeń przenośnych.


**P: co użytkownicy zrobić, jeśli zobaczą komunikat o błędzie 0x800434D4L podczas logowania się do przeglądarki?**

Obecnie użytkownika za pomocą dodatkowych Weryfikacja tylko aplikacji i usług, które dany użytkownik ma dostęp za pośrednictwem przeglądarki sieci. Aplikacje przeglądarki inne niż (nazywane również *klienta wzbogaconego aplikacji*), które są zainstalowane na komputerze lokalnym, na przykład programu Windows PowerShell, nie działa z konta, które wymagają dodatkowych Weryfikacja. W tym przypadku użytkownik może zostać wyświetlony aplikacji wygenerować błąd 0x800434D4L.

Obejść ten problem na to ma inny użytkownik konta dla związanych z administratora i działań niebędących administratorami. Później tak, aby Zaloguj się do programu Outlook przy użyciu konta administratora nie można połączyć skrzynek pocztowych między konta administratora i konta niebędących administratorami. Aby uzyskać więcej informacji na ten temat, Dowiedz się, jak [udzielić administrator możliwość otwierania i wyświetlania zawartości skrzynki pocztowej użytkownika](http://help.outlook.com/141/gg709759.aspx?sl=1).

## <a name="next-steps"></a>Następne kroki

Jeśli tutaj nie ma odpowiedzi na pytanie, pozostaw je w komentarzach w dolnej części strony. Lub, Oto niektóre dodatkowe opcje uzyskania pomocy:


**P: jak można uzyskać pomoc dotyczącą uwierzytelnianie wieloskładnikowe Azure?**

- Przeszukiwanie [bazy wiedzy pomocy technicznej firmy Microsoft](https://www.microsoft.com/en-us/Search/result.aspx?form=mssupport&q=phonefactor&form=mssupport) dla rozwiązań typowych problemów technicznych.

- Wyszukiwanie i przeglądanie pytania i odpowiedzi od społeczności lub Zadaj pytanie na [Forum usługi Azure Active Directory](https://social.msdn.microsoft.com/Forums/azure/newthread?category=windowsazureplatform&forum=WindowsAzureAD&prof=required).

- Jeśli jesteś klientem usługi starsze PhoneFactor i masz pytania lub potrzebujesz pomocy przy resetowania hasła, umożliwia otwieranie przypadek pomocy technicznej [do resetowania hasła](mailto:phonefactorsupport@microsoft.com) łącze.

- Skontaktuj się z pracownikiem pomocy technicznej do [obsługi serwera uwierzytelnianie wieloskładnikowe Azure (PhoneFactor)](https://support.microsoft.com/oas/default.aspx?prid=14947). Podczas kontaktowania się z nami, jest przydatne, jeśli mogą zawierać tyle informacje o swoim problemie, jak to możliwe. Informacje, które można dostarczyć zawiera strony, w którym został wyświetlony komunikat o błędzie, kod błędu, identyfikator określonej sesji i identyfikator użytkownika, który jest wyświetlony komunikat o błędzie.
