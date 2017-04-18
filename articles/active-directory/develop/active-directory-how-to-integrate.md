<properties
   pageTitle="Jak zintegrować z usługą Azure Active Directory | Microsoft Azure"
   description="Przewodnik po zalet i zasoby dotyczące integracji z usługą Azure Active Directory."
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="bryanla"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/16/2016"
   ms.author="mbaldwin"/>

# <a name="integrating-with-azure-active-directory"></a>Integracja z usługą Azure Active Directory

[AZURE.INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Azure Active Directory zapewnia organizacjom, Zarządzanie tożsamościami klasy korporacyjnej dla aplikacje w chmurze.  Azure AD integracji zwiększa usprawniony obsługi logowania użytkowników i aplikacji są zgodne z zasad IT.

## <a name="how-to-integrate"></a>Jak zintegrować

Istnieje kilka sposobów aplikacji do integracji z usługą Azure Active Directory.  Korzystać z tyle lub jako mało scenariuszom jest odpowiedni dla aplikacji.

### <a name="support-azure-ad-as-a-way-to-sign-in-to-your-application"></a>Obsługuje Azure AD jako sposobu logowania do aplikacji

**Zmniejsz logowania tarcia i zmniejszyć koszty obsługi.** Za pomocą Azure AD, aby zalogować się do aplikacji, użytkownicy nie mają więcej nazw i hasło do zapamiętania.  Projektant będą dostępne mniej hasło do przechowywania i ochrona.  Brak konieczności obsługi resetuje zapomniane hasło może być znaczące oszczędności samodzielnie.  Azure AD uprawnień logowania dla niektórych na świecie najpopularniejszych aplikacje w chmurze, takich jak usługi Office 365 i Microsoft Azure.  Z setki milionów użytkowników z milionów organizacji, prawdopodobnie oznacza to użytkownika jest już zarejestrowana w programie Azure AD.  Dowiedz się więcej na temat [dodawania obsługę Azure AD Zaloguj](../active-directory-authentication-scenarios.md).

**Uprościć logowania się aplikacji.**  Podczas logowania się aplikacji Azure AD można wysłać podstawowe informacje o użytkowniku, dzięki czemu możesz wstępnie wypełnić informacje logowania formularza lub całkowicie wyeliminować.  Użytkownicy mogą tworzyć konta aplikacji przy użyciu swojego konta Azure AD przy użyciu środowisko znanych zgody podobne do profilów dostępnych w społecznościowych i aplikacji dla urządzeń przenośnych.  Dowolny użytkownik można utworzyć konto i zaloguj się do aplikacji, która jest zintegrowany z usługą Azure Active Directory bez konieczności zaangażowanie IT.  Więcej informacji o [logowaniu up jako aplikacji do logowania się do konta AD Azure](../../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md).

### <a name="browse-for-users-manage-user-provisioning-and-control-access-to-your-application"></a>Przeglądaj w poszukiwaniu użytkowników, zarządzanie przypisywanie użytkowników i kontrolowanie dostępu do aplikacji

**Przeglądaj w poszukiwaniu użytkowników w katalogu.**  Pomaganie użytkownikom wyszukiwanie i przeglądać dla innych osób w organizacji, gdy zapraszania innych osób za pomocą interfejsu API wykresu lub adresy udzielanie dostępu, a nie do wpisz adres e-mail.  Użytkownicy mogą przeglądać za pomocą interfejsu Styl książki znanych adres, łącznie z wyświetlania szczegółów hierarchii organizacji.  Dowiedz się więcej na temat [Interfejsu API wykres](../active-directory-graph-api.md).

**Ponowne używanie grup usługi Active Directory i listy dystrybucyjne, których klienta jest już używana do zarządzania.**  Azure AD zawiera nazwy grup, że klient jest już korzystasz do dystrybucji poczty e-mail i zarządzanie dostępem.  Ponownie za pomocą interfejsu API wykres, użyj tych grup zamiast klienta do tworzenia i zarządzania oddzielny zestaw grup w aplikacji.  Informacje o grupie mogą być również wysyłane do aplikacji w sign in tokeny.  Dowiedz się więcej na temat [Interfejsu API wykresu](../active-directory-graph-api.md).

**Za pomocą Azure AD w celu kontrolowania, kto ma dostęp do aplikacji.**  Administratorzy i właściciele aplikacji Azure AD można przypisywać dostępu aplikacjom określonym użytkownikom i grupom.  Za pomocą interfejsu API wykres, można przeczytać tej listy i używać go w celu kontrolowania inicjowania obsługi administracyjnej i Anuluj inicjowania obsługi administracyjnej zasobów i dostępu w aplikacji.

**Kontrola dostępu oparta na używanie Azure AD dla ról.**  Administratorzy i właściciele aplikacji można przypisać użytkownikom i grupom role definiujące podczas rejestrowania aplikacji w Azure AD.  Roli informacje są wysyłane do aplikacji zaloguj tokeny i także mogą być odczytywane za pomocą interfejsu API wykresu.  Dowiedz się więcej o [korzystaniu z platformy Azure AD zezwolenia](http://blogs.technet.com/b/ad/archive/2014/12/18/azure-active-directory-now-with-group-claims-and-application-roles.aspx).

### <a name="get-access-to-users-profile-calendar-email-contacts-files-and-more"></a>Uzyskiwanie dostępu do profilu użytkownika, kalendarza, poczty E-mail, kontaktów, plików i innych elementów

**Azure AD to serwer autoryzacji dla usługi Office 365 i innych usług firmy Microsoft.**  Jeśli obsługiwanych Azure AD do zalogowania się do aplikacji lub łączenia bieżącego kont użytkowników z kont użytkowników Azure AD przy użyciu OAuth 2.0 pomocy technicznej, możesz zażądać odczytu i zapisu do profilu użytkownika, kalendarza, poczty e-mail, kontaktów, pliki i inne informacje.  Można bezproblemowo zapisywać zdarzenia w kalendarzu użytkownika i odczytu lub zapisu plików do ich OneDrive.  Dowiedz się więcej na temat [uzyskiwania dostępu do interfejsów API usługi Office 365](https://msdn.microsoft.com/office/office365/howto/platform-development-overview).

### <a name="promote-your-application-in-the-azure-and-office-365-marketplaces"></a>Promowanie aplikacji Azure i rynku usługi Office 365

**Promowanie aplikacja miliony organizacji, którzy już korzystają z Azure AD.**  Użytkownicy, którzy przeszukiwać i przeglądać tych rynków już korzystają z jedną lub więcej usług w chmurze, dzięki czemu można je kwalifikowanym klienci usługi w chmurze.  Dowiedz się więcej o aplikacji [Usługi Azure Marketplace](https://azure.microsoft.com/marketplace/partner-program/).

**Gdy użytkownicy tworzą aplikacji, pojawią się w ich Azure AD panel dostępu i uruchamianie aplikacji usługi Office 365.**  Użytkownicy będą mogli szybko i łatwo powrócić do aplikacji później, poprawy zaangażowania użytkownika.  Dowiedz się więcej na temat [Azure AD uzyskać dostęp do panelu](../active-directory-saas-access-panel-introduction.md).

### <a name="secure-device-to-service-and-service-to-service-communication"></a>Bezpieczna komunikacja z urządzenia do usługi i do

**Za pomocą Azure AD dla Zarządzanie tożsamościami usług i urządzeń zmniejsza kod, który należy wpisać i włącza IT zarządzanie dostępem.**  Usług i urządzeń można uzyskać tokenów z Azure AD przy użyciu OAuth i uzyskać dostęp do interfejsów API sieci web za pomocą tych tokenów.  Za pomocą Azure AD można uniknąć pisania kodu złożonych uwierzytelniania.  Ponieważ tożsamości usług i urządzeń są przechowywane w Azure AD IT można zarządzać, klucze i odwołania w jednym miejscu zamiast w tym oddzielnie w aplikacji.

## <a name="benefits-of-integration"></a>Zalety integracji

Integracja z usługą Azure Active Directory zawiera korzyści, które nie wymagają napisać dodatkowy kod.

### <a name="integration-with-enterprise-identity-management"></a>Integracja z Zarządzanie tożsamościami przedsiębiorstwa

**Pomoc aplikacji są zgodne z niej zasady.**  Organizacje integracja systemów zarządzania tożsamości przedsiębiorstwa z Azure AD tak odchodzący organizacji automatycznie utracą dostęp do aplikacji bez konieczności dodatkowe czynności IT.  IT można zarządzać, kto może uzyskać dostęp do aplikacji i określenia, jakie zasady dostępu są wymagane - uwierzytelniania wieloskładnikowego przykład - zmniejszania Twoim potrzebom, pisanie kodu do wykonania złożonych zasad firmowych.  Azure AD udostępnia administratorom z dziennika inspekcji szczegółowe kto zalogowanie się do aplikacji tak IT można śledzić zastosowania.

**Azure AD rozszerza usługi Active Directory w chmurze, tak aby aplikacja można zintegrować z usługą Active Directory.**  Wiele organizacji na całym świecie ich kapitału logowania i systemu zarządzania tożsamości za pomocą usługi Active Directory i wymagają ich aplikacji do współdziałania z usługą Active Directory.  Integracja z usługą Azure Active Directory aplikacji można zintegrować z usługą Active Directory.

### <a name="advanced-security-features"></a>Zaawansowanych funkcji zabezpieczeń

**Uwierzytelnianie wieloskładnikowe.**  Azure AD udostępnia natywnego uwierzytelnianie wieloskładnikowe.  Administratorzy IT można wymagane uwierzytelnianie wieloskładnikowe, aby uzyskać dostęp do aplikacji, dzięki czemu nie trzeba samodzielnie kod tej pomocy technicznej.  Dowiedz się więcej na temat [Uwierzytelnianie wieloskładnikowe](https://azure.microsoft.com/documentation/services/multi-factor-authentication/).

**Anomalous logowania wykrywania.**  Azure AD przetwarza rejestrowaniu więcej niż miliardów dziennie, podczas korzystania z komputera nauki algorytmów wykryć podejrzane działania i powiadomienia pozwala administratorom systemów informatycznych o potencjalnych problemach.  Dzięki obsłudze logowania Azure AD, aplikacja otrzymuje korzyści, jakie ochrony. Dowiedz się więcej na temat [wyświetlania usługi Azure Active Directory dostęp do raportu](../active-directory-view-access-usage-reports.md).

**Dostępu warunkowego.**  Oprócz uwierzytelnianie wieloskładnikowe Administratorzy mogą wymagać przed użytkownicy mogli logować się do aplikacji być spełnione określone warunki.  Warunki, które mogą być ustawiane zawierać zakres adresów IP o urządzeniach klienckich, członkostwo w grupach określonych i stan urządzenia używane w programie access.  Więcej informacji o [dostępie warunkowym usługi Azure Active Directory](../active-directory-conditional-access.md).

### <a name="easy-development"></a>Łatwość programowania

**Standardowe protokoły branży.**  Firma Microsoft poświęca pomocniczych standardami.  Azure AD obsługuje protokoły uwierzytelniania SAML 2.0, OpenID łączenie 1.0, OAuth 2.0 i 1.2 federacyjnych.  Interfejs API wykres jest zgodny z 4.0 OData.  Jeśli aplikacja już obsługuje protokoły SAML 2.0 lub 1.0 łączenie OpenID dla federacyjnych logowania, dodawanie obsługę Azure AD może być proste.  Dowiedz się więcej na temat [Azure AD obsługiwane protokoły](../active-directory-authentication-protocols.md).

**Otwieranie biblioteki źródeł.**  Firma Microsoft udostępnia biblioteki w pełni obsługiwane Otwórz źródło dla języków popularne i platform szybkości rozwoju.  Kod źródłowy jest licencjonowany w obszarze Apache 2.0 i wolnych rozwidlenia i Współtworzenie projektów.  Więcej informacji o [bibliotekach uwierzytelniania Azure AD](../active-directory-authentication-libraries.md).

### <a name="worldwide-presence-and-high-availability"></a>Informacje o obecności na całym świecie i wysokiej dostępności

**Azure AD zostanie wdrożony w centrach danych na całym świecie zarządzane i monitorować całą dobę.**  Azure AD jest tożsamości systemu zarządzania Microsoft Azure i usługi Office 365 i wdrożeniu w 28 centrach danych na całym świecie.  Katalog danych jest gwarantowana być replikowane na co najmniej trzy centrach danych.  Urządzenia do równoważenia obciążenia globalnej zapewnienia dostępu użytkownikom najbliższym kopię Azure AD zawierające dane, a automatycznie przekierowuje żądania do innych centrach danych, jeśli zostanie wykryty problem.

## <a name="next-steps"></a>Następne kroki

[Wprowadzenie do pisania kodu](../active-directory-developers-guide.md#getting-started).

[Użytkownicy Zaloguj się przy użyciu Azure AD](../active-directory-authentication-scenarios.md)
