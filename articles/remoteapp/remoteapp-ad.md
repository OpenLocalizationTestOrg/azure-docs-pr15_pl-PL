
<properties 
    pageTitle="Azure AD + wymagania usługi Active Directory dla Azure RemoteApp | Microsoft Azure" 
    description="Dowiedz się, jak skonfigurować usługi Active Directory do pracy z Azure RemoteApp." 
    services="remoteapp" 
    documentationCenter="" 
    authors="lizap" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />



# <a name="azure-ad--active-directory-requirements-for-azure-remoteapp"></a>Azure AD + wymagania usługi Active Directory dla Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp jest są przerywane. W tym artykule [anons](https://go.microsoft.com/fwlink/?linkid=821148) , aby uzyskać szczegółowe informacje.


Zbioru hybrydowych Azure RemoteApp lub kolekcji chmury, który chcesz utworzyć Federację przy użyciu AD Connect należy wykonać następujące czynności.

### <a name="connect-azure-ad-and-active-directory"></a>Łączenie Azure AD i usługi Active Directory

Jeśli chcesz się połączyć dzierżawy usługi Azure AD i do środowisk usługi Active Directory w lokalnej, za pomocą AD Connect. Zajmie możesz tylko [4 kliknięć](https://blogs.technet.microsoft.com/enterprisemobility/2014/08/04/connecting-ad-and-azure-ad-only-4-clicks-with-azure-ad-connect/) łączenie dwóch katalogów.

Uwaga - synchronizacji katalogów jest wymagany dla zbiorów hybrydowych.

### <a name="make-sure-your-domaincom-match"></a>Upewnij się, z "@domain.com" zgodne
Przed rozpoczęciem, upewnij się, że nazwa UPN las lokalnego odpowiada sufiks domeny Azure AD. 

Po skonfigurowaniu sufiksu głównej nazwy użytkownika domeny w Azure AD wszystkim użytkownikom logowanie do Azure RemoteApp będzie Zaloguj się jako “user@ <the suffix you set up>". Upewnij się, że użytkownicy także zalogować się przy użyciu tak samo user@suffix do domeny lokalnej. W niektórych przypadkach możesz skonfigurować nazwy domeny w Azure AD podczas określania sufiksu inną domenę dla użytkownika na prem. W tym przypadku użytkownicy nie będą mogli połączyć się z dowolnej domeny komputerach lub zasobów przy użyciu funkcji RemoteApp Azure.

Na przykład po skonfigurowaniu usługi sufiksu domeny głównej nazwy użytkownika w AAD jako contoso.com, ale niektórych użytkowników w lokalnej usługi Active są skonfigurowane do logowania się @contoso.uk, tych użytkowników nie będzie można poprawnie zalogować się do zbioru ARA. Użytkownicy głównej nazwy użytkownika w AAD i AD musi być taka sama dla logowania się w celu umożliwienia"

### <a name="create-objects-for-azure-remoteapp"></a>Tworzenie obiektów dla Azure RemoteApp
Trzeba również tworzenie obiektów usługi Active Directory w następujących miejscach:

- Konto usługi, aby umożliwić dostęp do zasobów domeny dla programów RemoteApp przez połączenie RDSH punktów końcowych do domeny lokalnej.
- Organizacji jednostek (OU) zawiera obiekty komputera RemoteApp. Użyj jednostkę Organizacyjną jest zalecane (ale nie są wymagane) wykrywać kont i zasady, które będą używane z RemoteApp.

Potrzebne tych obiektów podczas tworzenia kolekcji RemoteApp, dlatego należy najpierw wykonaj następujące czynności.