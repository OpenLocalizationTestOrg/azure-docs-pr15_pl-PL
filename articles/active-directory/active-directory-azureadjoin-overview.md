<properties
    pageTitle="Rozszerzanie możliwości chmury urządzeń systemu Windows 10 za pośrednictwem Azure Active Directory dołączanie | Microsoft Azure"
    description="Zawiera szczegółowe omówienie sposobu urządzeń z systemem Windows 10 mogą korzystać Azure AD dołączanie Aby zarejestrować się w usłudze Azure Active Directory."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""
    tags="azure-classic-portal"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>

# <a name="extending-cloud-capabilities-to-windows-10-devices-through-azure-active-directory-join"></a>Rozszerzanie możliwości chmury urządzeń systemu Windows 10 za pośrednictwem Azure Active Directory dołączanie

## <a name="what-is-azure-active-directory-join"></a>Co to jest Azure Active Directory dołączyć?
Azure Active Directory dołączanie (Azure AD dołączyć do) jest funkcji rejestruje urządzeniu należące do firmy w usługi Azure Active Directory, aby włączyć scentralizowane zarządzanie urządzeniem. Powoduje możliwe dla użytkowników, takich jak pracowników i studentów, aby nawiązać połączenie z chmurą enterprise za pośrednictwem usługi Azure Active Directory. Dzięki temu uproszczone wdrożeniach systemu Windows i dostęp do organizacji aplikacji i zasobów na dowolnym urządzeniu z systemem Windows, zarówno firmy posiadane i osobiście własnością (BYOD).

Azure AD sprzężenia jest przeznaczona dla przedsiębiorstw, które są chmury pierwszego chmury — tylko — zazwyczaj małych i średnich firmy, które nie mają infrastrukturę lokalnej usługi Active Directory systemu Windows Server. Czy wspomniane, Azure AD sprzężenia można i będą używane przez dużych organizacjach na urządzeniach, które nie, wykonując sprzężenia tradycyjnych domeny (urządzeń przenośnych, na przykład) lub dla użytkowników, którzy potrzebują przede wszystkim dostępu do usługi Office 365 lub inne aplikacje Azure AD władz akredytacji bezpieczeństwa.

Mimo że dołączanie tradycyjnych domeny nadal oferuje najlepsze lokalnego wystąpić na urządzeniach, które są w stanie dołączania domeny, Azure AD sprzężenia nadaje się do urządzenia, których nie można dołączyć do domeny. Azure AD sprzężenia również nadaje się do zarządzania użytkownikami w chmurze. Robi to za pomocą urządzenia przenośnego funkcje zarządzania zamiast przy użyciu tradycyjnych domeny narzędzi do zarządzania takimi jak zasad grupy i System Centrum konfiguracji Manager (SCCM).

![Omówienie firmowe i osobiste urządzenia z lokalnej usługi Active Directory i Azure AD](./media/active-directory-azureadjoin/active-directory-azureadjoin-overview.png)


## <a name="why-should-enterprises-adopt-azure-ad-join"></a>Dlaczego przedsiębiorstw przyjmuje Azure AD dołączyć?

* **Firmy, które są w znacznym stopniu w chmurze**: Jeśli zostały przeniesione lub przenieść do modelu, w której są zmniejszania za sieci lokalnej i chcesz, aby działać bardziej w chmurze, Azure AD dołączanie mogą korzystać użytkownik. Być może zostały utworzone konta Azure AD, ręcznie lub za pośrednictwem synchronizacji usługi Active Directory w lokalnej. W obu przypadkach użytkownik ma konto w Azure AD i służy do zalogowania się do systemu Windows 10. Użytkownicy mogą dołączyć do swoich komputerach do Azure AD za pomocą dowolnej z gotowych do uruchomienia systemu lub z menu Ustawienia. Po dołączeniu, użytkownicy będą cieszyć się pojedynczego logowania jednokrotnego (SSO) dostęp do chmury zasobów takich jak usługi Office 365 w przeglądarce lub w aplikacjach pakietu Office.

* **Instytucji edukacyjnych**: jeden scenariuszy, możemy otrzymywać informacje o często jest instytucji edukacyjnych dwa typy użytkownika: nauczycieli i studentów. Członkowie wykładowcy są traktowane jako długoterminowe członków organizacji, dlatego tworzenie lokalnych kont dla nich pożądane. Ale uczniów lub studentów shorter-term należących do organizacji, a więc można zarządzać w Azure AD. Oznacza to, że skala katalogów może zostać przeniesiony do chmury zamiast magazynowane lokalnie. Również oznacza, że uczniów lub studentów możesz zalogować się do systemu Windows z ich kontami Azure AD i uzyskać dostęp do zasobów usługi Office 365 w przeglądarce lub w aplikacjach pakietu Office.

* **Firmy detalicznej**: innym scenariuszu zadawane przez użytkowników o odbiorców jest chęci łatwiej zarządzać sezonowy pracowników.  Ponownie kont dla pracowników dłuższym okresie czasu, pełny etat zazwyczaj są tworzone jako konta na komputerach domeny lokalnego. Ale sezonowy pracownicy są shorter-term członków organizacji, co zarządzać nimi miejsce, w którym licencji użytkownika można łatwiej przenosić wokół. Tworzenie kont tych użytkowników w chmurze z licencji usługi Office 365 umożliwia użytkownikom korzystania z zalet logowanie się do systemu Windows i aplikacji pakietu Office za pomocą konta Azure AD. W międzyczasie większej elastyczności ich licencji możesz zachować po opuszczają.
* **Innych firm**: mimo że obsługa użytkowników w usłudze Active Directory w lokalnej, możesz nadal skorzystać z użytkowników być sprzężone AD Azure. Jest tak, ponieważ Azure AD oferuje uproszczone środowisko łączących, zarządzania urządzeniami wydajność, rejestrowania zarządzania automatyczne urządzenia przenośnego i funkcji rejestracji jednokrotnej dla Azure AD i lokalnych zasobów.  

## <a name="what-capabilities-does-azure-ad-join-offer"></a>Jakie możliwości oferuje Azure AD dołączyć?
W przypadku Azure AD dołączyć otrzymasz następujące czynności:

* **Własny inicjowania obsługi administracyjnej firmową posiadanych urządzeń**: Z systemem Windows 10, użytkownicy mogą skonfigurować nowym, porządnie zapakowane urządzenie doświadczenia w nowym polu, bez udziału IT.


* **Obsługa nowoczesny formularz czynniki**: Azure AD sprzężenia działa na urządzeniach, które nie mają domeny tradycyjnych dołączanie możliwości.  


* **Obsługa istniejącego konta organizacji**: użytkownicy nie muszą już do tworzenia i obsługi osobistego konta Microsoft, aby uzyskać najlepsze wyniki na urządzeniach wystawiony firmy, tak samo, jak do systemu Windows 8. Zamiast tego mogą używać swoich istniejących kont pracy w Azure AD. W przypadku wielu organizacji oznacza to, zasadniczo czy użytkownicy mogą skonfigurować i zaloguj się do systemu Windows przy użyciu tych samych poświadczeń, które umożliwiają dostęp do usługi Office 365.


* **Automatyczne urządzenia przenośnego zarządzania rejestrowania**: urządzeń można automatycznie zapisać zarządzania urządzeniami przenośnymi, gdy połączenia Azure AD. Ten proces działa z Intune firmy Microsoft i jej partnerów rozwiązania do zarządzania urządzenia przenośnego. Po zakończeniu zarządzania urządzeniami z Intune pozwala administratorom systemów informatycznych monitor i zarządzać Azure sprzężone AD urządzeń obok domeny urządzeń w konsoli zarządzania SCCM.


* **Rejestracji jednokrotnej do zasobów firmy**: użytkownicy korzystają rejestracji jednokrotnej z pulpitu systemu Windows do aplikacji i zasobów w chmurze, takich jak usługi Office 365 i tysiące aplikacje biznesowe, które korzystają z Azure AD dla uwierzytelniania za pośrednictwem [Azure AD Connect](active-directory-azureadjoin-deployment-aadjoindirect.md). Gdy urządzenie jest podłączony do sieci firmowej i z dowolnego miejsca, gdy te zasoby są dostępne za pośrednictwem [Serwera Proxy aplikacji Azure AD](https://msdn.microsoft.com/library/azure/Dn768219.aspx)należące do firmy urządzeń dołączonych do Azure AD również korzystać z logowania jednokrotnego do zasobów lokalnych.


* **Mobilnego stan systemu operacyjnego**: ustawienia ułatwień dostępu, witryn sieci Web, sieci Wi-Fi haseł i inne ustawienia są synchronizowane przez firmową posiadanych urządzeń bez konieczności osobistego konta Microsoft.


* **Gotowe Enterprise ze Sklepu Windows**: ze Sklepu Windows obsługuje nabycia aplikacji i Licencjonowanie z kontami Azure AD. Organizacje mogą aplikacje Licencja zbiorcza i uzyskiwać do nich dostęp do użytkowników w organizacji.

## <a name="how-do-different-devices-work-with-azure-ad-join"></a>Jak działają różnych urządzeń z Azure AD dołączyć?

| Urządzenie firmowe (dołączony do domeny lokalnej)                                                                                                                                                                                         | Urządzenie firmowe (połączone w chmurze)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | Osobiste urządzenia                                                                                                         |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------|
| Użytkownicy mogą logować do systemu Windows przy użyciu poświadczeń pracy (tak samo, jak dzisiaj).                                                                                                                                                                        | Użytkownicy mogli logować się do systemu Windows przy użyciu poświadczeń pracy, które są zarządzane w Azure AD. Jest to istotne dla urządzeń firmy w trzech przypadkach: 1) organizacji nie ma usługi Active Directory w środowisku lokalnym (na przykład mała firma). 2) organizacji nie zostanie utworzona wszystkich kont użytkowników w usłudze Active Directory (na przykład kont dla uczniów lub studentów, konsultantów lub sezonowy pracowników nie są tworzone w usłudze Active Directory). 3) organizacji skonfigurowano urządzeń firmy, które nie można przyłączyć do domeny (lokalnego), na przykład telefonów lub tabletów z systemem SKU Mobile (na przykład pomocnicze urządzenie podjęcie powierzchnia factory detalicznego). Azure AD sprzężenia obsługuje dołączania urządzeń firmy w przypadku organizacji, zarówno zarządzanych i federacyjnych. | Użytkownicy Zaloguj się do systemu Windows za pomocą ich osobistych poświadczenia konta Microsoft (bez zmian).                                                |
| Użytkownicy mają dostęp do ustawień mobilnych i przedsiębiorstwa ze Sklepu Windows. Te usługi działa z kontami pracy i nie wymagają osobistego konta Microsoft. W tym celu organizacje nawiązać Azure AD ich Active Directory w lokalnej.                                        | Użytkownicy mogą uzyskać Samoobsługowe konfiguracji. Są przydatne do pierwszego uruchomienia (FRX) za pomocą swojego konta pracy jako alternatywa, do których świadczenie IT urządzeń, jednak obie metody są obsługiwane.                                                                                                                                                                                                                                                                                                                                                                             | Użytkownicy mogą łatwo dodawać konta służbowego, które odbywa się w usłudze Active Directory lub Azure AD.                                                      |
| Użytkownicy mają możliwości logowania jednokrotnego z pulpitu pracy aplikacje, witryn i zasoby — w tym zarówno w zasobów lokalnych, jak i w chmurze aplikacje, które używają Azure AD dla uwierzytelniania.                                                                                                            | Urządzenia są automatycznie rejestrowane w katalogu organizacji (Azure AD) i automatycznie zarejestrowana w zarządzanie urządzeniami przenośnymi. (Funkcja azure AD Premium).                                                                                                                                                                                                                                                                                                                                                                                                                                                  | Użytkownicy mają możliwość logowania jednokrotnego w aplikacjach i witrynami sieci Web i zasobów za pomocą tego konta służbowego.                                              |
| Użytkownicy mogą dodawać ich osobistego konta Microsoft, aby uzyskać dostęp do ich własnych obrazów i plików bez wpływania dane na poziomie przedsiębiorstwa. (Ustawienia mobilnego kontynuować pracę z ich kontami prac.) Konto Microsoft umożliwia logowania jednokrotnego i nie jest już dyski mobilnego ustawienia.  | Użytkownicy mogą uzyskać Sklep internetowy w przypadku resetowania hasła (SSPR) na logowania, co oznacza, że ta osoba Resetowanie zapomnianego hasła. (Funkcja azure AD Premium).                                                                                                                                                                                                                                                                                                                                                                                                                                   | Użytkownicy mają dostęp do przedsiębiorstwa ze Sklepu Windows, aby mogą uzyskać i korzystanie z aplikacji z LOB w swoich osobistych urządzeń. |                                                               |


## <a name="additional-information"></a>Dodatkowe informacje
* [Windows 10 w przedsiębiorstwie: sposoby za pomocą urządzenia do pracy](active-directory-azureadjoin-windows10-devices-overview.md)
* [Rozszerzanie możliwości chmury urządzeń systemu Windows 10 za pośrednictwem Azure Active Directory dołączanie](active-directory-azureadjoin-user-upgrade.md)
* [Uwierzytelnianie tożsamości bez hasła za pomocą Microsoft Passport](active-directory-azureadjoin-passport.md)
* [Informacje na temat scenariusze użycia dla Azure AD dołączanie](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Podłącz urządzenia domeny do Azure AD dla środowiska systemu Windows 10](active-directory-azureadjoin-devices-group-policy.md)
* [Konfigurowanie Azure AD dołączanie](active-directory-azureadjoin-setup.md)
