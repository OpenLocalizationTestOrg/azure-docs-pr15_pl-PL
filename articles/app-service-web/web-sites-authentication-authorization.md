<properties 
    pageTitle="Typ poświadczeń uwierzytelniania usługi Active Directory w lokalnej w aplikacji Azure | Microsoft Azure" 
    description="Więcej informacji na temat różnych opcji aplikacji linii firm w usłudze Azure aplikacji do uwierzytelnienia z usługą Active Directory w lokalnej" 
    services="app-service" 
    documentationCenter="" 
    authors="cephalin" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="web" 
    ms.date="08/31/2016" 
    ms.author="cephalin"/>

# <a name="authenticate-with-on-premises-active-directory-in-your-azure-app"></a>Typ poświadczeń uwierzytelniania usługi Active Directory w lokalnej w aplikacji Azure #

W tym artykule pokazano, jak do uwierzytelnienia z lokalnej usługi Active Directory (AD) w [Usłudze Azure aplikacji](../app-service/app-service-value-prop-what-is.md). Azure aplikacji są przechowywane w chmurze, ale istnieją pewne metody uwierzytelniania lokalnego użytkownicy bezpieczne. 

## <a name="authenticate-through-azure-active-directory"></a>Uwierzytelnianie za pomocą usługi Azure Active Directory
Dzierżawy usługi Azure Active Directory może być synchronizowane katalogu lokalnym programem AD. Ta metoda umożliwia użytkownikom AD uzyskać dostęp do aplikacji z Internetu i służą do uwierzytelniania przy użyciu poświadczeń lokalnego. Ponadto Azure aplikacji usługi zawiera [klucz Włącz rozwiązanie dla tej metody](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md). Za pomocą kilku kliknięć przycisku możesz włączyć uwierzytelnianie za pomocą dzierżawy synchronizacji katalogów dla aplikacji Azure. Ta metoda ma następujące zalety:

-   Nie wymaga dowolny kod uwierzytelniania w aplikacji. Poinformuj aplikacji usługi do uwierzytelniania dla Ciebie i poświęcać czasu na dostarczanie funkcji w aplikacji.
-   [Interfejs API Azure AD wykres](http://msdn.microsoft.com/library/azure/hh974476.aspx) umożliwia dostęp do danych katalogu z Twojej aplikacji Azure.
-   Przekazuje logowania jednokrotnego [wszystkie aplikacje obsługiwane przez usługi Azure Active Directory](/marketplace/active-directory/), w tym usługi Office 365, Dynamics CRM Online, Microsoft Intune i tysiące aplikacje w chmurze firmy Microsoft. 
-   Azure Active Directory obsługiwane kontrola dostępu oparta na rolach. Za pomocą wzorca [Authorize(Roles="X")] i minimalnych zmian w kodzie.

Aby zobaczyć, jak zapisać aplikację Azure z LOB, która uwierzytelnia z usługą Azure Active Directory, zobacz [Tworzenie aplikacji Azure z LOB, z uwierzytelnianiem usługi Azure Active Directory](web-sites-dotnet-lob-application-azure-ad.md).

## <a name="authenticate-through-an-on-premises-sts"></a>Uwierzytelnianie za pomocą usługi STS lokalnego
Jeśli masz lokalnego token Usługa bezpiecznego (STS) jak Active Directory Federation Services (AD FS), możesz go używać, aby utworzyć Federację uwierzytelniania dla aplikacji sieci Azure. Ta metoda jest najbardziej, gdy zasady firmy uniemożliwia są przechowywane w Azure AD danych. Jednak pamiętać:

-   Topologia usługi STS musi być wdrożony w lokalnym z ogólnych kosztów i zarządzania.
-   Tylko Administratorzy usług AD FS można skonfigurować [reguł roszczeń i uzależnionej zaufania firmy](http://technet.microsoft.com/library/dd807108.aspx), które mogą ograniczać opcje dewelopera. Z drugiej strony jest możliwe zarządzanie i dostosowywanie [roszczeń](http://technet.microsoft.com/library/ee913571.aspx) na podstawie aplikacji.
-   Dostęp do lokalnego AD danych wymaga oddzielnych rozwiązanie przez zaporę firmową.

Aby zobaczyć, jak zapisać aplikację Azure z LOB, która uwierzytelnia z lokalnej usługi STS, zobacz [Tworzenie aplikacji Azure z LOB, z uwierzytelnianiem usług AD FS](web-sites-dotnet-lob-application-adfs.md).
 
