<properties
    pageTitle="Scenariusze zastosowania i zagadnienia dotyczące rozmieszczania dla Azure AD dołączanie | Microsoft Azure"
    description="W tym artykule wyjaśniono, jak Administratorzy mogą skonfigurować Azure AD dołączanie dla użytkowników końcowych (pracowników, uczniów lub studentów, innych użytkowników). Zawiera omówienie również różnych scenariuszach rzeczywistych dotyczące korzystania z platformy Azure AD dołączanie."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""
    tags="azure-classic-portal"/>

< tagi ms.service= "active directory" ms.workload="identity" ms.tgt_pltfrm="na" ms.devlang="na" ms.topic="article" ms.date="09/27/2016"

    ms.author="femila"/>

# <a name="usage-scenarios-and-deployment-considerations-for-azure-ad-join"></a>Scenariusze zastosowania i zagadnienia dotyczące rozmieszczania dla Azure AD dołączanie

## <a name="usage-scenarios-for-azure-ad-join"></a>Scenariusze zastosowania Azure AD dołączanie
### <a name="scenario-1-businesses-largely-in-the-cloud"></a>Scenariusz 1: Firmy głównie w chmurze

Azure Active Directory dołączanie (Azure AD dołączyć do) mogą korzystać użytkownik Jeśli obecnie działa i Zarządzanie tożsamościami dla swojej firmy w chmurze lub przenoszenie do chmury wkrótce. Za pomocą konta, które zostały utworzone w Azure AD do zalogowania się do systemu Windows 10. Proces [pierwszego uruchomienia środowiska (FRX)](active-directory-azureadjoin-user-frx.md)lub przez połączenie Azure AD w [menu Ustawienia](active-directory-azureadjoin-user-upgrade.md)użytkownicy mogą dołączyć do swoich urządzeń do Azure AD.  Użytkownicy również cieszyć się pojedynczego logowania jednokrotnego (SSO) dostęp do chmury zasobów takich jak usługi Office 365 w przeglądarce lub w aplikacjach pakietu Office.

### <a name="scenario-2-educational-institutions"></a>Scenariusz 2: Instytucji edukacyjnych

Instytucji edukacyjnych zazwyczaj dysponują dwa typy użytkownika: nauczycieli i studentów. Członkowie wykładowcy są traktowane jako długoterminowe członków organizacji. Tworzenie lokalnych kont dla nich jest pożądane. Ale uczniów lub studentów są shorter-term członków organizacji i ich kont można zarządzać w Azure AD. Oznacza to, że skala katalogu może zostać przeniesiony do chmury zamiast są magazynowane lokalnie. Oznacza również, że uczniów lub studentów będzie można zalogować się do systemu Windows z ich kontami Azure AD i uzyskać dostęp do usługi Office 365 zasobów w aplikacjach pakietu Office.

### <a name="scenario-3-retail-businesses"></a>Scenariusz 3: Detalicznej firmy

Firmy detalicznej mają sezonowy pracowników oraz długoterminowe pracowników. Zazwyczaj tworzone lokalnych kont i komputerów domeny za pomocą dla pracowników zatrudnionych długoterminowe. Ale sezonowy pracownicy są shorter-term członków organizacji, a istnieje potrzeba Zarządzanie swoje konta w miejsce, w którym licencji użytkownika mogą być łatwo przenoszone między. Po utworzeniu konta użytkownika w chmurze z licencji usługi Office 365 tych użytkowników uzyskać korzyści zapewnia logowanie się do systemu Windows i pakietu Office aplikacji przy użyciu konta Azure AD podczas Obsługa większej elastyczności ich licencji po opuszczają.

### <a name="scenario-4-additional-scenarios"></a>Scenariusz 4: Dodatkowe scenariusze

Wraz z zalet zostało to opisane wcześniej, możesz korzystać z konieczności użytkownikami połączenia urządzeń Azure AD ze względu na uproszczone środowisko łączących, zarządzania urządzeniami wydajność, automatyczne urządzenia przenośnego zarządzania rejestrowania i logowania jednokrotnego do Azure AD i lokalnych zasobów.  


## <a name="deployment-considerations-for-azure-ad-join"></a>Zagadnienia dotyczące rozmieszczania dla Azure AD dołączanie

### <a name="enable-your-users-to-join-a-company-owned-device-directly-to-azure-ad"></a>Umożliwić użytkownikom dołączanie należące do firmy urządzenie bezpośrednio do Azure AD


Przedsiębiorstw można udostępnić tylko do chmury konta partnera firmy i organizacje. Partnerów może łatwo uzyskać dostęp aplikacje firmy i zasobów z logowania jednokrotnego. Ten scenariusz dotyczy użytkowników, którzy uzyskać dostęp do zasobów przede wszystkim w chmurze, takich jak aplikacje usługi Office 365 lub władz akredytacji bezpieczeństwa, zależne od Azure AD dla uwierzytelniania.

### <a name="prerequisites"></a>Wymagania wstępne
**Na poziomie przedsiębiorstwa (administrator)**

*   Azure subskrypcję z usługi Azure Active Directory  

**Na poziomie użytkownika**

*   Windows 10 (wersje Professional i Enterprise)

### <a name="administrator-tasks"></a>Zadania administracyjne
* [Konfigurowanie Rejestracja urządzenia](active-directory-azureadjoin-setup.md)

### <a name="user-tasks"></a>Zadania użytkownika
* [Konfigurowanie nowego urządzenia systemu Windows 10 z usługą Azure Active Directory podczas instalacji](active-directory-azureadjoin-user-frx.md)
* [Konfigurowanie urządzenia systemu Windows 10 z usługą Azure Active Directory w menu Ustawienia](active-directory-azureadjoin-user-upgrade.md)
* [Dołączanie do osobistej urządzenia systemu Windows 10 dla Twojej organizacji](active-directory-azureadjoin-personal-device.md)



## <a name="enable-byod-in-your-organization-for-windows-10"></a>Włączanie BYOD w Twojej organizacji dla systemu Windows 10
Możesz skonfigurować użytkowników i pracowników korzystać ich osobistego urządzenia z systemem Windows (BYOD), aby uzyskać dostęp do aplikacji firmy i zasobów. Użytkownicy mogą dodawać kont Azure AD (konta służbowego) do osobistej urządzenie z systemem Windows uzyskiwanie dostępu do zasobów w sposób bezpieczne i zgodne.

### <a name="prerequisites"></a>Wymagania wstępne
**Na poziomie przedsiębiorstwa (administrator)**

*   Azure AD subskrypcji

**Na poziomie użytkownika**

*   Windows 10 (wersje Professional i Enterprise)


### <a name="administrator-tasks"></a>Zadania administracyjne

* [Konfigurowanie Rejestracja urządzenia](active-directory-azureadjoin-setup.md)

### <a name="user-tasks"></a>Zadania użytkownika
* [Dołączanie do osobistej urządzenia systemu Windows 10 dla Twojej organizacji](active-directory-azureadjoin-personal-device.md)


## <a name="additional-information"></a>Dodatkowe informacje
* [Windows 10 w przedsiębiorstwie: sposoby za pomocą urządzenia do pracy](active-directory-azureadjoin-windows10-devices-overview.md)
* [Rozszerzanie możliwości chmury urządzeń systemu Windows 10 za pośrednictwem Azure Active Directory dołączanie](active-directory-azureadjoin-user-upgrade.md)
* [Uwierzytelnianie tożsamości bez hasła za pomocą Microsoft Passport](active-directory-azureadjoin-passport.md)
* [Informacje na temat scenariusze użycia dla Azure AD dołączanie](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Podłącz urządzenia domeny do Azure AD dla środowiska systemu Windows 10](active-directory-azureadjoin-devices-group-policy.md)
* [Konfigurowanie Azure AD dołączanie](active-directory-azureadjoin-setup.md)
