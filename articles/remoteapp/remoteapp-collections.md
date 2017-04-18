<properties 
    pageTitle="Jakiego rodzaju zbioru należy Azure RemoteApp? | Microsoft Azure" 
    description="Informacje na temat typów kolekcji dostępnych z Azure RemoteApp." 
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



# <a name="what-kind-of-collection-do-you-need-for-azure-remoteapp"></a>Jakiego rodzaju zbioru należy Azure RemoteApp?

> [AZURE.IMPORTANT]
> Azure RemoteApp jest są przerywane. W tym artykule [anons](https://go.microsoft.com/fwlink/?linkid=821148) , aby uzyskać szczegółowe informacje.

Funkcja RemoteApp Azure umożliwia udostępnianie aplikacji i zasobów użytkownikom na dowolnym urządzeniu. Firma Microsoft to zrobić, tworząc kolekcje do przechowywania te aplikacje i zasoby, a następnie Udostępnij zbiorów użytkownikom. Istnieją 2 różnych opcji, z inną siecią i opcje uwierzytelniania — co jest dla Ciebie?

Przejdźmy przez różne aspekty i opcji, które chcesz wprowadzić jak najlepiej wykorzystać kolekcji Azure RemoteApp. 


## <a name="quick-differences-between-the-collection-types"></a>Szybkie różnic między typami kolekcji

|           | Chmury | Hybrydowe |
|-----------|-------|--------|
|Używanie istniejących VNET| Tak| Tak|
|Wymaga połączonych AD kont (DirSync)| Brak| Tak|
|Wymaga Dołączanie domeny| Brak| Tak|
|Wymaga dostępne dla VNET kontrolera domeny| Brak| Tak|

## <a name="cloud-collections"></a>Kolekcje chmury
- Szybkie tworzenie - kolekcji jest szybkie obsługi administracyjnej, co oznacza aplikacji użytkownikom szybsze uzyskiwanie.
- Przesuń do własnych aplikacji lub udostępnić nasze. Można użyć obraz niestandardowy (wbudowany z maszyn wirtualnych Azure) lub jeden z obrazów dołączonych do subskrypcji.
- Nie musisz skonfigurować połączenie między kolekcji i domeny lokalnej.
- Ale można używać własnych VNET Azure umożliwia dostęp do środowiska lokalnego udostępniania danych lub, aby użyć uwierzytelniania — do zasobów, takich jak program SQL Server (przy użyciu funkcji uwierzytelniania bazy danych).


Przycisk OK jak jedną utworzyć?

- Tylko w chmurze? Tworzenie z opcją **Szybkiego tworzenia** w portalu.
- Chmura + VNET? Tworzenie za pomocą opcji **Utwórz z VNET** , ale nie zostanie dołączony do domeny.

## <a name="hybrid-collections"></a>Kolekcje hybrydowego
- Podaj pełny dostęp do sieci lokalnej + Azure VNET.
- Zapewnia dostęp sprzężenia domeny dla aplikacji i danych. Zdalny aplikacje mogą uwierzytelniania usługi Active Directory w lokalnej — one następnie uzyskać dostęp do swojej domeny zasobów.
- Włącz zaawansowane monitorowania i zarządzania z istniejących rozwiązań System Center i zasad grupy systemu Windows (za pomocą obraz niestandardowy, oparty na systemie Windows Server 2012 R2)
- Obsługuje [ExpressRoute](https://azure.microsoft.com/services/expressroute/) , aby nawiązać połączenie z lokalnym VNET VNET usługi Azure.

Tworzenie za pomocą opcji **Utwórz z VNET** i wybierz pozycję dołączyć do domeny.

## <a name="authentication-options"></a>Opcje uwierzytelniania
Funkcja RemoteApp Azure obsługuje konta Microsoft i konta usługi Azure Active Directory, ale nie wszystkich zbiorów obsługuje wszystkie metody. 

| W obszarze Typ konta                      |                                                             | Chmury | Chmura + VNET | Hybrydowe |
|-----------------------------------|-------------------------------------------------------------|-------|--------------|--------|
| Konto Microsoft                 |                                                             | Tak   | Tak          | Brak     |
| Azure Active Directory (Azure AD) |                                                             |       |              |        |
|                                   | Tylko Azure AD                                               | Tak   | Tak          | Brak     |
|                                   | Połącz AD z synchronizacji haseł                               | Tak   | Tak          | Tak    |
|                                   | AD Connect bez synchronizacji haseł                            | Tak   | Tak          | Brak     |
|                                   | AD połączyć się z usług AD FS                                       | Tak   | Tak          | Tak    |
|                                   | dostawcy tożsamości obsługiwane Azure 3rd firmy (na przykład polecenie Ping) | Tak   | Tak          | Tak    |
| Uwierzytelnianie wieloskładnikowe       |                                                             | Tak   | Tak          | Tak    |



### <a name="cloud-and-cloud--vnet"></a>Chmura i chmury + VNET 
Z kolekcji w chmurze można użyć konta Microsoft, Azure AD konta lub połączenie dwóch. Za pomocą konta, które najlepiej dla użytkowników.

Istnieją określone wymagania dotyczące korzystania z konta Microsoft. 

Jeśli chcesz używać Azure AD kont, musisz upewnij się, że dzierżawy usługi Azure AD pasuje do jednej skojarzonego z subskrypcją usługi. Po utworzeniu subskrypcji Azure RemoteApp dzierżawy Azure AD, który był używany był automatycznie skojarzone z subskrypcją. Dowolny użytkownik Azure AD, które nadać uprawnienie musi być tej samej dzierżawy. W razie potrzeby możesz [zmienić dzierżawy Azure AD](remoteapp-changetenant.md) skojarzonego z subskrypcją usługi.
 
### <a name="hybrid-or-cloud--azure-ad--ad"></a>Hybrydowe (lub w chmurze, jak + Azure AD + AD)

Za pomocą Azure AD + lokalnej usługi Active Directory jest wymagane w przypadku zbioru hybrydowych. Należy użyć AD Connect do integracji katalogów dwóch. Ale masz kilka wybór, jeśli chodzi o sposób konfigurowania AD Connect. 

Istnieją 2 AD Połącz scenariusze — za pomocą synchronizacji haseł lub przy użyciu AD federacji. Zapoznaj się z [informacji AD Connect](../active-directory/active-directory-aadconnect.md) ustalanie, które z poniższych sprawdza się najlepiej dla Ciebie.

Azure AD + AD można również używać z kolekcji chmury. Upewnij się, że taka sama procedura konfiguracji obserwować.

Zapoznaj się z [Azure AD + wymagania usługi Active Directory dla Azure RemoteApp](remoteapp-ad.md) kroki wymagane do skonfigurowania Azure AD i usługi Active Directory.

## <a name="go-create-your-collection"></a>Przejdź tworzenie kolekcji!
Przycisk OK myślę, że firma Microsoft została znalezienia go teraz, dlatego tylko jeden element w lewo, aby wykonać — Tworzenie pierwszego kolekcji Azure RemoteApp.

[Tworzenie zbioru chmury](remoteapp-create-cloud-deployment.md) lub [utworzyć zbiór hybrydowych](remoteapp-create-hybrid-deployment.md) - uzyskiwanie tworzenie.
