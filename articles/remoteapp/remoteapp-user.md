<properties
    pageTitle="Dodawanie użytkownika do kolekcji Azure RemoteApp | Microsoft Azure"
    description="Dowiedz się, jak dodawać użytkowników do kolekcji Azure RemoteApp"
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

# <a name="how-to-add-a-user-to-your-azure-remoteapp-collection"></a>Jak dodać użytkownika do kolekcji Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp jest są przerywane. W tym artykule [anons](https://go.microsoft.com/fwlink/?linkid=821148) , aby uzyskać szczegółowe informacje.

Przed użytkowników można wyświetlić i używać aplikacji w Azure RemoteApp, należy udzielić im dostępu do kolekcji. Jest to część łatwe: na karcie **Dostępu użytkowników** wprowadź informacje o koncie dla użytkownika, a następnie kliknij znacznik wyboru.

Jakie informacje o koncie są potrzebne? Która zależy od typu zbioru utworzonych (chmurze lub hybrydowymi połączeniami) i tego, czy korzystasz z usługi Office 365 ProPlus w tej kolekcji.

## <a name="supported-user-identities"></a>Tożsamości obsługiwanych użytkowników

Typy kolekcji różnych (chmury a hybrydowe) obsługuje przy użyciu tożsamości innemu użytkownikowi dostępu do aplikacji.  

Dla zbioru hybrydowych funkcja RemoteApp musisz skonfigurować infrastruktury domeny usługi Active Directory w lokalnej i dzierżawy usługi Azure Active Directory przy użyciu integracji katalogów z (i opcjonalnie pojedynczy logowania jednokrotnego). Ponadto należy utworzyć kilka obiektów usługi Active Directory w katalogu lokalnego.  

Na chmurze zbiór RemoteApp każdy użytkownik, który ma usługi Azure Active Directory obsługuje tożsamości można udzielić dostępu użytkowników do RemoteApp, aby uwzględnić Accounts Microsoft.  Zobacz poniższą tabelę.

Użytkownicy usługi Office 365 korzystają z usługi Azure Active Directory. Jeśli mają hybrydowego usługi Azure Active Directory katalogu synchronizowane konta, ich mogą być udzielane dostępu użytkowników we wdrożeniu hybrydowym RemoteApp.   

Za pomocą tej tabeli jako podręczna karta informacyjna, dla którego tożsamości jest obsługiwana w zbiorze i jakie są wymagania usługi Active Directory.

|Konta użytkowników |Chmury   |Hybrydowe|
|--------------|--------|------|
|Konto Microsoft|     Tak|    Brak|
|Azure Active Directory (Azure AD)| | |
|Azure chmury AD    |Tak    |Brak |
|Synchronizacja z synchronizacji haseł  |Tak    |Tak    |
|Synchronizacja bez synchronizacji haseł|  Tak |Brak |
|Synchronizacja z usług AD FS  |Tak    |Tak    |
|[Strona 3rd Azure obsługiwane dostawcy tożsamości](https://msdn.microsoft.com/library/azure/jj679342.aspx)  (przykład Ping) |Tak    |Tak|
|Uwierzytelnianie wieloskładnikowe    |Tak    |Tak    |

Zapoznaj się z [więcej informacji](remoteapp-ad.md) o konfigurowaniu usługi Active Directory dla RemoteApp.


> [AZURE.NOTE] Użytkownicy usługi Azure Active Directory muszą być z dzierżawy, który jest skojarzony z subskrypcją. (Można przeglądać i modyfikować swoją subskrypcję na karcie **Ustawienia** w portalu. Zobacz [Zmienianie dzierżawy usługi Azure Active Directory używane przez RemoteApp](remoteapp-changetenant.md) Aby uzyskać więcej informacji).

## <a name="office-365-proplus-user-account-information"></a>Informacje o koncie usługi Office 365 ProPlus użytkownika
Jeśli korzystasz z usługi Office 365 ProPlus obraz szablonu w zbiorze *lub* Jeśli utworzono obraz niestandardowy, która korzysta z usługi Office 365, są dozwolone tylko dodawanie użytkowników usługi Azure Active Directory, których subskrypcji usługi Office 365 dla domeny domyślnej subskrypcji. Aby uzyskać więcej informacji, zobacz [przy użyciu usługi Office 365 z Azure RemoteApp](remoteapp-o365.md) .
