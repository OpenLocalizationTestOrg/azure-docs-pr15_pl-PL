<properties
   pageTitle="Jak aplikacje są dodawane do usługi Azure Active Directory."
   description="W tym artykule opisano, jak aplikacje są dodawane do wystąpienia usługi Azure Active Directory."
   services="active-directory"
   documentationCenter=""
   authors="shoatman"
   manager="kbrint"
   editor=""/>

   <tags
      ms.service="active-directory"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="identity"
      ms.date="02/09/2016"
      ms.author="shoatman"/>

# <a name="how-and-why-applications-are-added-to-azure-ad"></a>Jak i dlaczego aplikacje są dodawane do Azure AD

Jedną z wstępnie nieoczekiwane czynności podczas przeglądania listy aplikacji pakietu wystąpienia programu usługi Azure Active Directory jest opis, skąd pochodzą aplikacje i dlaczego są dostępne.  Ten artykuł zawiera wysokiego poziomu omówienie jak aplikacje znajdują się w katalogu i udostępnić kontekstu, który pomoże Ci zrozumieć, jak aplikacja została dostarczona znajdować się w katalogu.

## <a name="what-services-does-azure-ad-provide-to-applications"></a>Jakie usługi Azure AD są dostępne w aplikacji?

Aplikacje są dodawane do Azure AD je wykorzystać co najmniej jedną z usług, które znajdują się w nim.  Te usługi są:

* Aplikacja uwierzytelniania i autoryzacji
* Uwierzytelniania użytkowników i autoryzacji
* Logowania jednokrotnego (SSO) za pomocą hasła lub Federacji
* Przypisywanie użytkowników i synchronizacji
* Kontrola dostępu oparta na rolach; Użyj katalogu, aby zdefiniować ról aplikacji do pełnienia ról podstawie sprawdzanie autoryzacji w aplikacji.
* oAuth autoryzacji usług (używanych przez usługi Office 365 i innych aplikacji firmy Microsoft, aby zezwolić na dostęp do interfejsów API i zasobów.)
* Publikowanie aplikacji i serwer proxy; Publikowanie aplikacji z sieci prywatnej w Internecie

## <a name="how-are-applications-represented-in-the-directory"></a>Jak aplikacje są przedstawiane w katalogu

Aplikacje są przedstawione w Azure AD przy użyciu obiektów 2: obiekt aplikacji i obiektem głównej usługi.  Jest jeden obiekt aplikacji, zarejestrowane w "domu" i "właściciel" lub "publikowania" katalogu i co najmniej jednym kapitału obiektów reprezentujących aplikacji w każdym katalogu, w którym działa usługa.  

Obiekt aplikacji w tym artykule opisano aplikację, aby Azure AD (usługa wielu dzierżawy) i może zawierać dowolne z następujących czynności: (*Uwaga*: nie jest pełna lista.)

* Nazwa, Logo i programu Publisher
* Hasła (symetrycznej i/lub asymetrycznym klawisze używane do uwierzytelnienia aplikacji)
* Interfejs API współzależności (oAuth)
* Interfejsy API i zasoby i zakresów opublikowanych (oAuth)
* Role aplikacji (RBAC)
* Metadane logowania jednokrotnego i konfiguracji (SSO)
* Użytkownik inicjowania obsługi administracyjnej metadanych i konfiguracji
* Metadane serwera proxy i Konfiguracja

Wartość kapitału usługi jest rekordem aplikacji w każdym katalogu, w której aplikacja działa, łącznie z jego katalogu macierzystego.  Wartość kapitału usługi:

* Odwołuje się do obiektu aplikacji za pomocą właściwości identyfikator aplikacji
* Użytkownik lokalny rekordów i przypisania ról aplikacji grupy
* Rekordy lokalnych uprawnień użytkowników i administratorów przyznane do aplikacji
    * Na przykład: uprawnień dla aplikacji uzyskać dostęp do poczty e-mail poszczególnych użytkowników
* Zasady lokalne rekordów, w tym zasady dostępu warunkowego
* Rekordy alternatywny lokalne ustawienia lokalne dla aplikacji
    * Roszczeń transformacja reguł
    * Mapowania atrybutów (przypisywanie użytkowników)
    * Dzierżawa role konkretnej aplikacji (Jeśli aplikacja obsługuje role niestandardowe)
    * Nazwa i Logo

### <a name="a-diagram-of-application-objects-and-service-principals-across-directories"></a>Diagram obiektów aplikacji oraz usług główne wielu katalogów

![Diagram pokazujący, jak aplikacja obiektów i świadczenie usług podmiotów istniejących z wystąpieniami Azure AD.][apps_service_principals_directory]

Jak widać na powyższym diagramie.  Firma Microsoft udostępnia dwa poziomy wewnętrznie (po lewej) za pomocą publikowania aplikacji.

* Jeden dla Microsoft Apps (katalog usług firmy Microsoft)
* Jeden dla wstępnie zintegrowane aplikacje innych firm 3 (katalog aplikacji galerii)

Aplikacja wydawcy i dostawców, którzy integrację z usługą Azure Active Directory muszą mieć publikowania katalogu.  (Niektóre katalog władz akredytacji bezpieczeństwa).

Aplikacje, które możesz dodać siebie obejmują:

* Aplikacje, które zostały uzyskane przez użytkownika (zintegrowany z AAD)
* Aplikacje połączony od-jednokrotnej
* Aplikacje, które opublikowano przy użyciu serwera proxy aplikacji Azure AD.

### <a name="a-couple-of-notes-and-exceptions"></a>Kilka notatek i wyjątki

* Nie wszystkie podmioty usługi ponownie wskaż obiekty aplikacji.  Huh? Gdy Azure AD został pierwotnie utworzony usług udostępnionych aplikacji były bardziej ograniczone i wystawcy usługi były wystarczające ustalania tożsamości aplikacji.  Oryginalny głównej usługi był w kształcie bliżej konta usługi katalogowej Active Server systemu Windows.  Z tego powodu jest nadal można tworzyć przy użyciu programu PowerShell Azure AD bez tworzenia obiekt aplikacji główne usługi.  Interfejs API Graph wymaga obiekt aplikacji przed utworzeniem głównej usługi.
* Obecnie są nie ze wszystkich informacji opisanych powyżej prezentowane programowy.  Następujące czynności są dostępne tylko w interfejsie użytkownika:
    * Roszczeń transformacja reguł
    * Mapowania atrybutów (przypisywanie użytkowników)
* Aby uzyskać więcej szczegółowych informacji spłaty kapitału usługi i obiekty aplikacji należy zapoznaj się z dokumentacją odwołanie interfejsu API usługi REST Azure AD wykresu.  *Wskazówka*: Dokumentacja interfejsu API Azure AD wykres jest najbliższy kroki, aby odwołania do schematu dla Azure AD, która jest obecnie dostępna.  
    * [Aplikacji](https://msdn.microsoft.com/library/azure/dn151677.aspx)
    * [Wystawcy usługi](https://msdn.microsoft.com/library/azure/dn194452.aspx)


## <a name="how-are-apps-added-to-my-azure-ad-instance"></a>Jak aplikacje są dodawane do mojej wystąpienia Azure AD
Istnieje wiele sposobów, które można dodać aplikację do Azure AD:

* Dodawanie aplikacji z [Galerii aplikacji Azure Active Directory](https://azure.microsoft.com/updates/azure-active-directory-over-1000-apps/)
* Znak w górę/w 3rd aplikacji firmy zintegrowany z usługi Azure Active Directory (na przykład: [Smartsheet](https://app.smartsheet.com/b/home) lub [DocuSign](https://www.docusign.net/member/MemberLogin.aspx))
    * Podczas logowania wzrost-użytkownicy są proszeni o nadać uprawnienie do aplikacji, aby uzyskać dostęp do jej profilu i inne uprawnienia.  Pierwsza osoba zgody powoduje wystawcy usługi reprezentującą aplikacji mają zostać dodane do katalogu.
* Zaloguj się w górę/w witrynie Microsoft online services, takich jak [usługi Office 365](http://products.office.com/)
    * Gdy subskrypcji usługi Office 365 lub rozpocząć wersji próbnej jeden lub więcej głównych usługi są tworzone w katalogu reprezentującą różnych usług, które są używane do prezentowania wszystkie funkcje związane z usługą Office 365.
    * Niektóre usługi Office 365, takie jak SharePoint tworzyć główne usługi na podstawie bieżących umożliwia bezpiecznej komunikacji między składnikami, łącznie z przepływów pracy.
* Dodawanie aplikacji projektujesz Zobacz Portal Azure zarządzania: https://msdn.microsoft.com/library/azure/dn132599.aspx
* Dodaj aplikację, którą projektujesz przy użyciu programu Visual Studio, zobacz:
    * [Metody uwierzytelniania ASP.Net](http://www.asp.net/visual-studio/overview/2013/creating-web-projects-in-visual-studio#orgauthoptions)
    * [Usługi połączone](http://blogs.msdn.com/b/visualstudio/archive/2014/11/19/connecting-to-cloud-services.aspx)
* Dodawanie aplikacji za pomocą za pomocą [Serwera Proxy aplikacji Azure AD](https://msdn.microsoft.com/library/azure/dn768219.aspx)
* Połącz aplikację dla rejestracji jednokrotnej przy użyciu SAML lub hasło logowania jednokrotnego
* Wielu innych zawierające różne środowiska Deweloper Azure i w Eksploratorze interfejsu API napotkania przez dewelopera centra

## <a name="who-has-permission-to-add-applications-to-my-azure-ad-instance"></a>Kto ma uprawnienia do dodawania aplikacji do mojej wystąpienia Azure AD?

Tylko administrator globalny wykonywać następujące czynności:

* Dodawanie aplikacji z galerii aplikacji Azure AD (wstępnie zintegrowane aplikacje innych firm 3)
* Publikowanie aplikacji za pomocą serwera Proxy aplikacji usługi AD Azure

Wszyscy użytkownicy w katalogu mają uprawnienia do dodawania aplikacji, które są one opracowywania i uznania przez aplikacje, które są udostępnianie i nadawania dostępu do swoich danych organizacji.  *Należy pamiętać o logowania użytkownika, Strzałka w górę/w aplikacji i udzielanie uprawnień może spowodować naliczenie wystawcy usługi tworzony.*

Ten wstępnie dźwięku dotyczące mogą być, ale zachowaj następujące pamiętać:

* Aplikacje uzyskały możliwość korzystania z usługi Active Directory systemu Windows Server do uwierzytelnienia użytkownika przez wiele lat bez konieczności aplikacji zostać zarejestrowane i zapisane w katalogu.  Teraz organizacji będzie ulepszona widzą dokładnie ile aplikacje używają katalogu i dla what.
* Bez potrzeby dla administratorów zmiennych procesu publikowania i rejestracji aplikacji.  Z usług federacyjnych Active Directory było prawdopodobne, administrator trzeba dodać aplikację jako uzależnioną imieniu deweloperów.  Teraz deweloperów możliwe samodzielnego.
* Użytkownicy logowanie/do aplikacji do celów biznesowych za pomocą konta organizacji jest to użyteczna.  Następnie opuszczają organizacji spowoduje utratę dostępu do konta w aplikacji, które były używane.
* O wykaz danych został udostępniony, z której aplikacji jest to użyteczna.  Dane są bardziej przenośne niż kiedykolwiek i konieczności wyczyść rejestr osobę, która udostępniła danych z aplikacji, które jest przydatne.
* Aplikacje, którzy korzystają z platformy Azure AD dla oAuth Określ dokładnie jakie uprawnienia, który użytkownicy będą mogli udzielać do aplikacji, które wymagają uprawnienia administratora, aby zaakceptować.  Należy go przechodzenie bez informujący, że tylko administratorzy mogą przyznać zakresów większych i bardziej znaczące uprawnienia.
* Użytkowników, dodając i zezwolenia na dostęp do danych aplikacji to inspekcji zdarzeń, aby można było wyświetlić raporty inspekcji w portalu zarządzania Azure, aby określić, jak aplikacja została dodana do katalogu.

**Uwaga:** *Samej firmie Microsoft działał przy użyciu konfiguracji domyślnej teraz wiele miesięcy.*

Z wszystkimi tego said istnieje możliwość uniemożliwić użytkownikom w katalogu Dodawanie aplikacji i wykonywać uznania przez tego, jakie informacje są udostępnianie z aplikacjami za pomocą modyfikowania konfiguracji katalogu w portalu zarządzania Azure.  Następujące są dostępne w portalu zarządzania Azure na karcie "Konfigurowanie" usługi katalogowej.

![Zrzut ekranu przedstawiający interfejs użytkownika do konfigurowania ustawień zintegrowanych aplikacji][app_settings]


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Następne kroki

Dowiedz się, jak informacje na temat dodać aplikacje do Azure AD i konfigurowanie usług dla aplikacji.

* Deweloperów: [Dowiedz się, jak zintegrować aplikację z AAD](https://msdn.microsoft.com/library/azure/dn151122.aspx)
* Deweloperów: [Recenzja przykładowy kod aplikacje zintegrowany z usługi Azure Active Directory na Github](https://github.com/AzureADSamples)
* Deweloperów i informatyków: [Przejrzyj dokumentację interfejsu API usługi REST w przypadku Azure Active Directory wykres interfejsu API](https://msdn.microsoft.com/library/azure/hh974478.aspx)
* Informatycy:, [Dowiedz się, jak używać wstępnie zintegrowanych aplikacji usługi Azure Active Directory z galerii aplikacji](https://msdn.microsoft.com/library/azure/dn308590.aspx)
* Informatycy: [znaleźć samouczki dotyczące konfigurowaniu określonego wstępnie zintegrowane aplikacje](https://msdn.microsoft.com/library/azure/dn893637.aspx)
* Informatycy:, [Dowiedz się, jak opublikować aplikację za pomocą Azure Active Directory serwer Proxy aplikacji](https://msdn.microsoft.com/library/azure/dn768219.aspx)

## <a name="see-also"></a>Zobacz też

- [Artykuł indeks do zarządzania aplikacjami w usłudze Azure Active Directory](active-directory-apps-index.md)

<!--Image references-->
[apps_service_principals_directory]:media/active-directory-how-applications-are-added/HowAppsAreAddedToAAD.jpg
[app_settings]:media/active-directory-how-applications-are-added/IntegratedAppSettings.jpg
