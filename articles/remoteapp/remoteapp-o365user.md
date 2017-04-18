
<properties 
    pageTitle="Jak używać Azure RemoteApp z kont użytkowników usługi Office 365 | Microsoft Azure"
    description="Dowiedz się, jak funkcja RemoteApp platformy Azure za pomocą kont użytkowników usługi Office 365"
    services="remoteapp"
    documentationCenter="" 
    authors="piotrci" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />



# <a name="how-to-use-azure-remoteapp-with-office-365-user-accounts"></a>Jak używać Azure RemoteApp z kont użytkowników usługi Office 365

> [AZURE.IMPORTANT]
> Azure RemoteApp jest są przerywane. W tym artykule [anons](https://go.microsoft.com/fwlink/?linkid=821148) , aby uzyskać szczegółowe informacje.

Jeśli masz subskrypcję usługi Office 365 masz usługi Azure Active Directory przechowującego swojej nazwy użytkownika i hasła używanego do uzyskiwania dostępu usługi Office 365. Na przykład gdy użytkownicy aktywowanie usługi Office 365 ProPlus ich uwierzytelnić Azure AD, aby sprawdzić, czy licencje. Większość klientów chce używać tego samego katalogu z Azure RemoteApp.

W przypadku wdrażania Azure RemoteApp najprawdopodobniej używasz subskrypcji usługi Azure, który jest skojarzony z różnych Azure AD. Aby korzystać z katalogu usługi Office 365, będzie konieczne przeniesienie Azure subskrypcji do tego katalogu.

Aby uzyskać informacje na temat instalacji aplikacji klienckich usługi Office 365 zobacz [jak korzystać z subskrypcji usługi Office 365 z Azure RemoteApp](remoteapp-officesubscription.md).
 
## <a name="phase-1-register-your-free-office-365-azure-active-directory-subscription"></a>Faza 1: Rejestrowanie bezpłatna subskrypcja usługi Office 365 usługi Azure Active Directory
Jeśli korzystasz z portalem klasyczny Azure, wykonaj czynności w [rejestrze bezpłatna subskrypcja usługi Azure Active Directory](https://technet.microsoft.com/library/dn832618.aspx) , aby uzyskać dostęp administracyjny do usługi Azure AD przy użyciu portalu zarządzania Azure. W wyniku tego procesu można zalogować się do portalu Azure i zobacz katalogu — w tym momencie nie zobaczysz bardziej ponieważ pełnego Azure subskrypcję, którą za pomocą Azure RemoteApp jest w innym katalogu.

Zapamiętaj nazwę i hasło konta administratora utworzonego w tym kroku — będą potrzebne w etap 2.

Jeśli korzystasz z portalem Azure, zapoznaj się z [Jak rejestrować i aktywowanie bezpłatnej usługi Azure Active Directory za pomocą portalu usługi Office 365](http://azureblogger.com/2016/01/how-to-register-and-activate-a-free-azure-active-directory-using-office-365-portal/).

## <a name="phase-2-change-the-azure-ad-associated-with-your-azure-subscription"></a>Faza 2: Zmienianie Azure AD skojarzonego z subskrypcją usługi Azure.
Firma Microsoft zamiar zmienić subskrypcję Azure z jego bieżącego katalogu do katalogu usługi Office 365, nad którymi pracował z możemy na etapie 1.

Postępuj zgodnie z instrukcjami opisano w temacie [Zmienianie dzierżawy usługi Azure Active Directory w Azure RemoteApp](remoteapp-changetenant.md). Należy zwrócić szczególną uwagę na następujące czynności:

- Krok #1: Jeśli wdrożono Azure RemoteApp (ARA) w tej subskrypcji, upewnij się, że usunięciu wszystkich kont użytkowników Azure AD z jakieś zbiory ARA najpierw przed próbą nic innego. Ponadto można uznać, usuwając wszelkie istniejące zbiory.
- Krok #2: To jest krytyczne. Należy za pomocą konta Microsoft (np. @outlook.com) jako Administrator usługi subskrypcji; to, ponieważ nie udostępniamy kont użytkowników z istniejących Azure AD dołączone do subskrypcji — przypadku firma Microsoft nie będzie można przenieść go do różnych Azure AD.
- Krok #4: Podczas dodawania istniejącego katalogu, system wyświetli monit o Zaloguj się przy użyciu konta administratora dla tego katalogu. Upewnij się użyć konta administratora z etap 1.
- Krok #5: Zmienianie katalogu nadrzędnego subskrypcji do katalogu usługi Office 365. Wynik końcowy należy, że w obszarze Ustawienia -> Subskrypcje subskrypcji listy katalogu usługi Office 365. 
![Zmienianie katalogu nadrzędnego subskrypcji](./media/remoteapp-o365user/settings.png)
 

W tym momencie subskrypcji Azure RemoteApp jest skojarzony z usługi Office 365 Azure AD; Możesz używać istniejących kont użytkowników usługi Office 365 z Azure RemoteApp!




