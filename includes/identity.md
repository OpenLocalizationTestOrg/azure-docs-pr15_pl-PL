Zarządzanie tożsamości jest równie ważne w chmurze publicznej niezmieniona w swojej siedzibie. Aby pomóc z tym, Azure obsługuje kilka różnych chmury tożsamości technologii. Te obejmują:

- Windows Server Active Directory (nazywanego dodatkiem tylko AD) można uruchamiać w chmurze przy użyciu maszyn wirtualnych utworzone za pomocą maszyn wirtualnych Azure. Ta metoda ma sens podczas korzystania z platformy Azure rozszerzenie lokalnego centrum danych do chmury.


- Za pomocą usługi Azure Active Directory udzielić Twojej użytkowników rejestracji jednokrotnej do oprogramowania [jako usługa (władz akredytacji bezpieczeństwa)](https://azure.microsoft.com/overview/what-is-saas/) . Firmy Microsoft Office 365 za pomocą tej technologii, na przykład i aplikacje uruchomione na innych platformach chmury lub Azure można także używać.


- Aplikacje w chmurze lub lokalnego umożliwia Azure Active Directory kontrola dostępu użytkownicy mogą Zaloguj się przy użyciu tożsamości z serwisem Facebook, Google, firmy Microsoft i innych dostawców tożsamości.


W tym artykule opisano wszystkie trzy opcje.

## <a name="table-of-contents"></a>Spis treści

- [Uruchamianie usługi Active Directory systemu Windows Server w środowisku maszyn wirtualnych systemu](#adinvm)

- [Za pomocą usługi Azure Active Directory](#ad)

- [Korzystanie z usługi Azure Active Directory kontroli dostępu](#ac)


## <a name="adinvm"></a>Uruchamianie usługi Active Directory systemu Windows Server w środowisku maszyn wirtualnych systemu

Z systemem Windows Server AD w środowisku maszyn wirtualnych Azure przypomina uruchomić je lokalnego. [Rysunek 1](#fig1) pokazuje typowy przykład to wygląd.

![Azure Active Directory w maszyn wirtualnych](./media/identity/identity_01_ADinVM.png)


<a name="Fig1"></a>Rysunek 1: Usługi Active Directory systemu Windows Server można uruchamiać w Azure maszyn wirtualnych połączenie przy użyciu Azure wirtualnej sieci lokalnej organizacji w centrum danych.

W pokazanym tu przykładzie Windows Server AD działa w maszyny wirtualne utworzony przy użyciu maszyn wirtualnych Azure, technologii IaaS platformy. Te maszyny wirtualne i kilku innych są pogrupowane w wirtualnej sieci połączenia lokalnego centrum danych, za pomocą Azure wirtualnej sieci. Wirtualna sieć Wycina się grupy maszyn wirtualnych chmury współpracujących z sieci lokalnej przez połączenia wirtualnej sieci prywatnej (VPN). Wykonując pozwala tych Azure maszyn wirtualnych wyglądać po prostu innej podsieci lokalnego centrum danych. Jak pokazano na ilustracji, obie te maszyny wirtualne działają kontrolerów domeny systemu Windows Server AD. Inne maszyn wirtualnych w wirtualnej sieci może działać aplikacjach, takich jak SharePoint lub używane w inny sposób, takich jak do projektowania i testowania. Centrum danych lokalnych działa również dwóch kontrolerów domeny systemu Windows Server AD.

Istnieje kilka opcji połączenia kontrolery w chmurze z systemami w środowisku lokalnym:

- Wprowadź każdy z nich części jednej domeny usługi Active Directory.

- Tworzenie osobnych AD domeny lokalnej i w chmurze, które są częścią samej las.

- Tworzenie osobny las AD w chmurze i lokalnych, a następnie połącz lasów za pomocą zaufania między las lub Windows Server Active Directory Federation Services (AD FS), które można także uruchamiać w środowisku maszyn wirtualnych systemu Azure.

Niezależnie od domyślny wybór, administrator upewnij się, że żądania uwierzytelniania użytkowników lokalnych przejdź do chmury kontrolery domeny tylko wtedy, gdy jest to konieczne, ponieważ łącze w chmurze jest może być mniejsza niż sieci lokalnej. Inny współczynnik brać pod uwagę w chmurze łączących i kontrolery domeny lokalnej jest ruch generowany przez replikacji. Kontrolery w chmurze zazwyczaj znajdują się w własne witryny AD, która umożliwia administratorom planowanie częstotliwości replikacji jest wykonywane. Azure opłaty za ruch wysyłany poza Azure centrum danych, mimo że nie bajtów wysłane w, które mogą mieć wpływ na opcje replikacji administratora. Warto również wskazujące, że podczas Azure własnej obsługi systemu nazw domen (DNS), ta usługa brakuje funkcje wymagane przez usługi Active Directory (na przykład pomocy technicznej dla rekordów DNS dynamiczne i SRV). Z tego powodu z systemem Windows Server AD Azure wymaga konfigurowania własnych serwerów DNS w chmurze.

Z systemem Windows Server AD w Azure maszyny wirtualne można przeanalizować w kilku różnych sytuacjach. Oto kilka przykładów:

- Jeśli używasz maszyn wirtualnych Azure jako rozszerzenie własnego centrum danych, może być uruchamianie aplikacji w chmurze, które wymagają lokalnych kontrolerów domeny do obsługi elementów takich jak żądania zintegrowanego uwierzytelniania systemu Windows lub kwerend LDAP. Program SharePoint, na przykład interakcji często z usługą Active Directory, a więc w trakcie można uruchomić farmy programu SharePoint Azure za pomocą katalogu lokalnego konfigurowania kontrolery w chmurze znacznie zwiększa wydajność. (Jest zdać sobie sprawę, że to nie musi być wymagane, jednak; dużo aplikacji można uruchamiać w pomyślnie w chmurze przy użyciu tylko kontrolery domeny lokalnego.)

- Załóżmy, że odległych oddziale brakuje zasobów, aby uruchomić kontrolerów własnej domeny. Dzisiaj użytkownicy muszą uwierzytelniania kontrolery po drugiej stronie świata — logowania są powolne. Uruchomionych w dokładniejsze centrum danych Microsoft Azure Active Directory może przyspieszyć ten bez konieczności większej liczby serwerów w oddziale.

- Organizacji, która używa Azure awarii mogą utrzymywać małego zestawu aktywnego maszyny wirtualne w chmurze, łącznie z kontrolera domeny. Następnie można go przygotować aby rozwinąć tę witrynę, aby przejąć błędy w innym miejscu.

Dostępne są także inne możliwości. Na przykład nie jest wymagany do nawiązania połączenia Windows Server AD w chmurze do lokalnych centrum danych. Jeśli chcesz uruchomić farmy programu SharePoint, które obsługiwane określony zestaw użytkowników, na przykład, z których chcesz zalogować się wyłącznie z tożsamościami opartej na chmurze, może utworzyć las autonomicznego Azure. Jak używać tej technologii zależy od tego, jakie są cele. (Aby uzyskać bardziej szczegółowe informacje na temat korzystania z systemu Windows Server AD przy użyciu Azure, [Zobacz tutaj](http://msdn.microsoft.com/library/windowsazure/jj156090.aspx).)

## <a name="ad"></a>Za pomocą usługi Azure Active Directory

Jak aplikacje władz akredytacji bezpieczeństwa staną się bardziej popularne, wzbudzają oczywiste pytanie: jakiego rodzaju usługi katalogowej należy używać tych aplikacji opartej na chmurze? Odpowiedź firmy Microsoft na to pytanie jest usługi Azure Active Directory.

Istnieją dwa główne opcje dotyczące korzystania z tej usługi katalogowej w chmurze:

- Usługi Azure Active Directory osób i organizacjami używającymi tylko te aplikacje, władz akredytacji bezpieczeństwa można traktować jako ich usługi katalogowej wyłączna.

- Organizacje z systemem Windows Server Active Directory można połączyć ich katalogu lokalnego usługi Azure Active Directory, a następnie go używać, aby nadać jej użytkowników logowania jednokrotnego aplikacjom władz akredytacji bezpieczeństwa.


[Rysunek 2](#fig2) przedstawia pierwszej te dwie opcje, w której usługi Azure Active Directory są wymaganego.

![Azure Active Directory w maszyn wirtualnych](./media/identity/identity_02_AD.png)

<a name="fig2"></a>Rysunek 2: Azure Active Directory powoduje organizacji użytkowników logowania jednokrotnego aplikacje władz akredytacji bezpieczeństwa, w tym usługi Office 365.

Jak pokazano na ilustracji, Azure AD to usługa wielu dzierżawy. Oznacza to, że jednocześnie może obsługiwać wiele różnych organizacji, przechowywania katalogu informacji o użytkownikach w każdej z nich. W tym przykładzie użytkownik w organizacji A próbuje uzyskać dostęp do aplikacji władz akredytacji bezpieczeństwa. Ta aplikacja może być częścią usługi Office 365, takich jak usługi SharePoint Online lub może być inny — aplikacje innych firm również mogą używać tej technologii. Ponieważ Azure AD obsługuje protokół SAML 2.0, wszystkie wymaganego z aplikacji jest możliwość interakcji, za pomocą tego standardowym. (W rzeczywistości aplikacje, które używają Azure AD można uruchamiać w dowolnym centrum danych, nie tylko Azure centrum danych.)

Proces zaczyna się, gdy użytkownik uzyskuje dostęp do aplikacji władz akredytacji bezpieczeństwa (krok 1). Aby używać tej aplikacji, użytkownik musi przedstawić token wystawiony przez Azure AD.

Token zawiera informacje identyfikujące użytkownika, a jest podpisane cyfrowo przez Azure AD. Aby uzyskać tokenu, użytkownik uwierzytelnia osobiście do Azure AD przez podanie nazwy użytkownika i hasła (krok 2). Następnie Azure AD zwraca token on musi (krok 3).

Ten token następnie są wysyłane do aplikacji władz akredytacji bezpieczeństwa (krok 4), która sprawdza podpis tokenu i korzysta z zawartością (krok 5). Zazwyczaj aplikacja będzie używać informacje tożsamości, które token zawiera zdecydować, jakie informacje użytkownik jest uprawniony do programu access, a także na inne sposoby.

Jeśli aplikacja wymaga więcej informacji na temat użytkownika niż co znajduje się w tokenów, go to żądanie bezpośrednio z Azure AD przy użyciu interfejsu API Azure AD wykresu (krok 6). W pierwszej wersji Azure AD schematu katalogu jest prosta: zawiera tylko użytkownicy i grupy i relacje między nimi. Aplikacje mogą używać te informacje, aby uzyskać informacje o połączenia między użytkownikami. Załóżmy, że aplikacja musi się dowiedzieć, kto jest menedżerem temu użytkownikowi zdecydować, czy ma on mogli uzyskiwać dostęp do niektórych fragmentów danych. Go można znaleźć przez badanie Azure AD za pośrednictwem interfejsu API wykresu.

Interfejs API Graph używa zwykłej protokołu RESTful, dzięki czemu proste korzystać w większości klientów, w tym urządzeń przenośnych. Interfejs API obsługuje także rozszerzenia zdefiniowanych przez OData, dodając innymi języka kwerend, aby umożliwić klientom dostępu do danych w sposób bardziej użyteczna. (Aby uzyskać więcej informacji o OData, zobacz [Wprowadzenie do OData](http://download.microsoft.com/download/E/5/A/E5A59052-EE48-4D64-897B-5F7C608165B8/IntroducingOData.pdf)). Ponieważ interfejs API Graph może służyć do informacje na temat relacji między użytkownikami, umożliwia aplikacji opis społecznościowych wykresu, który jest osadzony w schemacie Azure AD dla określonej organizacji (jest to, dlatego ta opcja nosi nazwę interfejsu API wykresu). I do samodzielnego uwierzytelnienia Azure AD dla interfejsu API wykres żądania, aplikacja używa OAuth 2.0.

Jeśli organizacja nie korzysta z usługi Active Directory systemu Windows Server - ma lokalne serwery ani domeny - i korzysta wyłącznie z chmury aplikacje, które używają Azure AD, przy użyciu tylko ten katalog chmury chcesz udzielić firmy użytkowników rejestracji jednokrotnej do nich wszystkich. Jeszcze podczas w tym scenariuszu otrzymuje najczęściej codziennie, większość organizacji używa nadal lokalnych domen utworzone za pomocą usługi Active Directory systemu Windows Server. Azure AD zawiera użyteczną funkcję odtwarzania tutaj również, jak pokazano na [rysunku 3](#fig3) .

![Azure Active Directory w maszyn wirtualnych](./media/identity/identity_03_AD.png)
<a id="fig3"></a>rysunek 3: organizacja może utworzyć Federację systemu Windows Server usługi Active Directory z usługi Azure Active Directory, aby nadać jej użytkowników logowania jednokrotnego aplikacjom władz akredytacji bezpieczeństwa.

W tym scenariuszu użytkownik w organizacji B chce uzyskać dostęp do aplikacji władz akredytacji bezpieczeństwa. Zanim jednak administratorów katalogu organizacji musi ustanowienia relacji federacji z Azure AD przy użyciu usług AD FS, jak to pokazano na ilustracji. Synchronizowanie danych między organizacji lokalnej Windows Server AD i Azure AD musisz również skonfigurować w tych administratorów. Spowoduje to automatyczne skopiowanie użytkownika i informacji o grupach z katalogu lokalnego do Azure AD. Zwróć uwagę, w ten sposób: W rzeczywistości organizacji rozszerza jego katalogu lokalnego w chmurze. Łączenie Windows Server AD i Azure AD w ten sposób zapewnia organizacji usługi katalogowej, którymi można zarządzać jako całość, przy zachowaniu rozmiaru zarówno w lokalnej i w chmurze.

Aby użyć Azure AD, użytkownik najpierw loguje się do swojej domeny usługi Active Directory w lokalnej w zwykły sposób (krok 1). Gdy użytkownik próbuje uzyskać dostęp do aplikacji władz akredytacji bezpieczeństwa (krok 2), proces Federacji wynikiem Azure AD wydawania jej tokenu dla tej aplikacji (krok 3). (Aby uzyskać więcej informacji na temat sposobu działania Federacji, zobacz [tożsamości opartych na oświadczeniach dla systemu Windows: technologie i scenariusze](http://www.davidchappell.com/writing/white_papers/Claims-Based_Identity_for_Windows_v3.0--Chappell.docx).) Jak wcześniej, token zawiera informacje identyfikujące użytkownika, a jest podpisane cyfrowo przez Azure AD. Ten token następnie są wysyłane do aplikacji władz akredytacji bezpieczeństwa (krok 4), która sprawdza podpis tokenu i korzysta z zawartością (krok 5). I znajdują się w poprzednim scenariuszu, władz akredytacji bezpieczeństwa aplikacji można użyć interfejsu API wykresu Aby dowiedzieć się więcej na temat tego użytkownika, jeśli konieczne (krok 6).

Obecnie Azure AD nie jest całkowite zastąpienie lokalnego w systemie Windows Server AD. Jak już wspomniano katalogu chmury ma znacznie upraszcza schemat, a także brakuje elementów, takich jak zasady grupy, przechowywanie informacji o komputerach i obsługi protokołu LDAP. (W rzeczywistości komputera systemu Windows nie można skonfigurować tak, aby umożliwić użytkownikom logowanie się do niego przy użyciu nic, ale Azure AD - nie jest to obsługiwany scenariusz.) Zamiast tego początkowej celów Azure AD zawiera co przedsiębiorstwa użytkownikom uzyskiwać dostęp do aplikacji w chmurze bez zachowania osobnej logowania i zwalnianie lokalnego katalogu administratorom Ręczne synchronizowanie ich katalogu lokalnego z wszystkich aplikacji władz akredytacji bezpieczeństwa, używanych w organizacji. Czasem jednak oczekiwać tej usługi katalogowej chmury adresem szerszej grupie scenariuszy.

## <a name="ac"></a>Korzystanie z usługi Azure Active Directory kontroli dostępu

Technologie opartej na chmurze tożsamości może służyć do rozwiązuje wiele problemów z. Azure Active Directory można zapewnić w organizacji użytkowników logowania jednokrotnego do wielu aplikacji władz akredytacji bezpieczeństwa, na przykład. Ale technologii tożsamości w chmurze można również w inny sposób.

Załóżmy na przykład, aplikacja chce umożliwić użytkownikom Zaloguj się przy użyciu tokenów wystawiony przez wielu *dostawców tożsamości (IdPs)*. Wiele dostawcy tożsamości różnych istnieje już dziś, takich jak Facebook, Google, firmy Microsoft i inne osoby, a aplikacje często użytkownicy Zaloguj się przy użyciu jednego z tych tożsamości. Dlaczego należy aplikacji odblokowane Obsługa własną listę użytkowników i hasła, gdy zamiast tego może korzystać tożsamości, które już istnieje? Akceptowanie istniejących tożsamości sprawia, że życia prostsze zarówno dla użytkowników, którzy mają jedną nazwę użytkownika i hasło do zapamiętania, jak i dla użytkowników, którzy utworzyć aplikację, która już nie potrzebujesz, aby zachować własne listy nazwy użytkowników i hasła.

Jednak podczas każdego dostawcy tożsamości problemy niektórych rodzajów token, tych tokenów nie są standardowe — każdy protokołu IdP ma własny format. Ponadto informacje znajdujące się w tych tokenów również nie jest standardowy. Aplikacja, z której chce zaakceptować tokenów wystawiony przez, na przykład, Facebook, Google i Microsoft jest wobec wyzwaniem pisania unikatowy kod dla każdej z tych formatach obsługiwać.

Ale to zrobić? Dlaczego nie zamiast tego utworzyć pośrednik generowany jeden format tokenów z typowych reprezentacją danych tożsamości? Tej metody spowodowałby życia prostsze dla deweloperów, którzy tworzą aplikacji, ponieważ muszą teraz obsługiwać tylko jeden rodzaj token. Azure kontrola dostępu Active Directory dokładnie powinien się tym zająć, dostarczając do pracy z różnych tokeny pośrednik w chmurze. [Rysunek 4](#fig4) pokazuje, jak to działa

![Azure Active Directory w maszyn wirtualnych](./media/identity/identity_04_IdentityProviders.png)
<a id="fig4"></a>rysunek 4: Azure Active Directory kontrola dostępu ułatwia aplikacje zaakceptować tokenów tożsamości wydany przez dostawców tożsamości różnych.

Proces zaczyna się, gdy użytkownik próbuje uzyskać dostęp do aplikacji w przeglądarce. Aplikacja przekierowuje jej do protokołu IdP jej wybranym (i aplikacja również zaufania). Anna uwierzytelnia sobie do tego protokołu IdP, takich jak przez wprowadzenie nazwy użytkownika i hasła (krok 1) i protokołu IdP zwraca token zawierający informacje na temat jej (krok 2).

Jak pokazano na ilustracji, kontrola dostępu obsługuje szereg różnych IdPs opartej na chmurze, w tym konta utworzone przez Google, Yahoo Facebook, Microsoft (wcześniej znany jako identyfikator Windows Live ID) i dowolnego dostawcy OpenID. Obsługuje także tożsamości utworzony przy użyciu usługi Azure Active Directory i za pośrednictwem federacji z usług AD FS, usługi Active Directory systemu Windows Server. Celem jest wykonanie najczęściej używane tożsamości dzisiaj, czy są one wystawiony przez IdPs w chmurze lub lokalnego.

Przeglądarka użytkownika ma token protokołu IdP z jej wybranego protokołu IdP, wysyła to tokenu do kontroli dostępu (krok 3). Kontrola dostępu sprawdza tokenu, upewniając się, że jej naprawdę wystawiony przez tego protokołu IdP, a następnie tworzy nowy token zgodnie z regułami, które zostały zdefiniowane dla tej aplikacji. Jak usługi Azure Active Directory kontrola dostępu jest usługą wielu dzierżawy, ale dzierżaw są aplikacje, a nie w organizacji klienta. Każdą z nich można uzyskać własnych nazw, jak to przedstawiono w rysunku, a można zdefiniować różne reguły dotyczące autoryzacji i nie tylko.

Te reguły pozwolić, aby określić, jak tokenów z różnych IdPs powinny być konwertowana token kontrola dostępu administratora każdej aplikacji. Na przykład jeśli IdPs różne używają różnych typów odpowiadające nazw użytkowników, reguły kontroli dostępu można przekształcać wszystkie te na wspólny typ nazwa_użytkownika. Kontrola dostępu przesyła ten nowy token powrót do przeglądarki (krok 4), która przesyła go do aplikacji (krok 5). Gdy został token kontrola dostępu aplikacji sprawdza, czy ten token naprawdę został wystawiony przez kontrola dostępu, a następnie używa informacje, które zawiera (krok 6).

Podczas tego procesu może wydawać się nieco skomplikowane, faktycznie umożliwia życia znacznie upraszcza Kreator aplikacji. Zamiast obsługi różnych tokeny zawierających różne informacje, aplikacja może zaakceptować tożsamości wystawiony przez wielu dostawców tożsamości podczas nadal pojawiają się pojedynczego token z informacjami o znanych. Ponadto zamiast wymagają każdej aplikacji, aby zaufać różnych IdPs należy skonfigurować, te relacje zaufania zamiast tego są obsługiwane przez kontrola dostępu — aplikacja potrzebny tylko Ufaj.

Warto wskazujące, że nic na temat kontrola dostępu jest związany z systemu Windows — po prostu także można aplikacji Linux, które zaakceptowały tylko tożsamości Google i serwisu Facebook. A nawet kontrola dostępu jest częścią rodziny usługi Azure Active Directory, można traktować go jako usługa całkowicie różnią się od co zostało opisane w poprzedniej sekcji. Gdy obie technologie pracować przy użyciu tożsamości, odnoszą się do bardzo różnych problemów (chociaż Microsoft ma said oczekuje zintegrować dwóch w pewnym momencie).

Praca z tożsamości jest ważna w niemal wszystkich aplikacji. Celem kontrola dostępu jest ułatwić dla deweloperów do tworzenia aplikacji akceptować tożsamości z dostawcy tożsamości różnorodną. Umieszczenie ta usługa w chmurze, firma Microsoft udostępniła go do dowolnej aplikacji działających na dowolnej platformie.

##<a name="about-the-author"></a>O autorze

David Chappell jest kapitału z Chappell & Associates [www.davidchappell.com](http://www.davidchappell.com) w San Francisco, Kalifornia.
