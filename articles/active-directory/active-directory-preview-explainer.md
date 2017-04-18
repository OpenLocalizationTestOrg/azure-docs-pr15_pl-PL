<properties
    pageTitle="Azure Active Directory Podgląd explainer | Microsoft Azure"
    description="Temat objaśniający różnice między usługi Azure Active Directory w portalu klasyczny i Podgląd usługi Azure Active Directory w portalu Azure."
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/12/2016"
    ms.author="curtand"/>


# <a name="preview-of-the-azure-active-directory-management-experience-in-the-azure-portal"></a>Podgląd środowisko zarządzania usługi Azure Active Directory w portalu Azure

Środowisko zarządzania usługą Azure Active Directory (Azure AD) jest w podglądzie Azure portal. Możesz spróbować go pomniejszyć, logując się do [portalu Azure](https://portal.azure.com) jako administrator globalny katalogu. Następnie wybierz na liście usług usługi Azure Active Directory, jeśli jest on widoczny, lub wybierz pozycję **więcej usług** , aby wyświetlić listę wszystkich usług. Nie ma potrzeby Azure subskrypcję Azure AD zarządzania doświadczenia w portalu Azure.


## <a name="capabilities-of-the-preview-experience"></a>Możliwości obsługi podglądu

Środowisko Podgląd umożliwia zarządzanie wielu zasobów katalogu, takich jak użytkownicy, grupy i aplikacje, a także ustawienia katalogu, w portalu Azure. Ulepszamy to doświadczenie, aby uwzględnić wszystkie funkcje, które istnieją w Azure AD zarządzania doświadczenia w [portal Azure klasyczny](https://manage.windowsazure.com). Do tego czasu istnieje kilka zarządzania katalogu ukończenia zadania, które musisz nadal w portalu klasyczny.

## <a name="manage-the-same-azure-ad-tenants"></a>Zarządzanie tym samym dzierżaw Azure AD

Środowisko Podgląd odczytuje i zapisuje do tej samej dzierżawy usługi Azure Active Directory jako portalu klasyczny i Centrum administracyjnego usługi Office 365. Zmiany wprowadzone w każdym z tych portali zostaną one odzwierciedlone we wszystkich pozostałych.

## <a name="use-the-same-authorization-logic"></a>Użyj tej samej logiki autoryzacji

Środowisko Podgląd używa tej samej logiki autoryzacji jako istniejący klienci usługi Active Directory. Aby wprowadzić zmiany w katalogu zasobów w zależności od ich roli katalogu, takich jak administrator globalny, administrator użytkowników i hasła administratora autoryzowanych użytkowników. O roli dla zasobów Azure lub subskrypcję usługi Azure nie zezwalają na użytkownika w celu zarządzania zasobami katalogu. Aby uzyskać więcej informacji role związane z zarządzaniem Azure AD, zobacz [Przypisywanie ról administratora w usłudze Azure Active Directory](active-directory-assign-admin-roles.md). 

Środowisko preview jest zoptymalizowana pod kątem administratorów globalnych. Jeśli używasz obsługi podglądu podczas zalogowany jako użytkownik, który nie jest administratorem globalnym, może być ograniczone możliwości. Można na przykład wybierz przycisk umożliwiający można rozpocząć zadania, która nie będzie można ukończyć w katalogu. Wkrótce Ulepszamy tego środowiska.
 
## <a name="tell-us-what-you-think"></a>Przekaż nam swoje uwagi

Można przekazywać opinie na środowisko preview w sekcji portalu administracyjnego [Azure AD forum opinii](https://social.msdn.microsoft.com/Forums/home?forum=WindowsAzureAD&filter=alltypes&sort=lastpostdesc).
