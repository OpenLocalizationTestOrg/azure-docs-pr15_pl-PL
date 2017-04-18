<properties
   pageTitle="Role w PIM | Microsoft Azure"
   description="Dowiedz się, jakie role są używane do uprzywilejowanych tożsamości z rozszerzeniem uprzywilejowanych Zarządzanie tożsamościami Azure."
   services="active-directory"
   documentationCenter=""
   authors="kgremban"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="07/01/2016"
   ms.author="kgremban"/>

# <a name="roles-in-azure-ad-privileged-identity-management"></a>Role w uprawnieniach Azure AD Zarządzanie tożsamościami

<!-- **PLACEHOLDER: Need description of how this works. Azure PIM uses roles from MSODS objects.**-->

Można przypisywać użytkowników w organizacji w Azure AD różne role administracyjne. Takie przypisania ról kontrolować, które zadania, takie jak dodawanie lub usuwanie użytkowników i zmienianie ustawień usługi użytkownicy będą mogli wykonać na Azure AD usługi Office 365 i innych usług Microsoft Online Services i połączonych aplikacji.  

Administrator globalny można zaktualizować użytkowników, którzy są **trwale** przypisane do ról w Azure AD przy użyciu poleceń cmdlet programu PowerShell, takich jak `Add-MsolRoleMember` i `Remove-MsolRoleMember`, lub za pośrednictwem portalu klasyczny, jak opisano w sekcji [Przypisywanie ról administratora w usłudze Azure Active Directory](active-directory-assign-admin-roles.md).

Azure AD uprawnieniach tożsamości Zarządzanie zarządza zasady dostępu uprzywilejowanych dla użytkowników w Azure AD. PIM przypisuje użytkownikom jednego lub większej liczby ról w Azure AD i można przypisać komuś trwale w roli, lub jest uprawniony do roli. Kiedy użytkownika trwale jest przypisana do roli lub uaktywnia przypisanie odpowiedniej roli, a następnie ich zarządzanie usługi Azure Active Directory, usługi Office 365 i innych aplikacji z uprawnienia przypisane do ich ról.

Nie różni się w programie access, danej osobie na stałe i przypisanie roli uprawniony. Jedyna różnica polega na tym, że niektóre osoby nie muszą przez program access cały czas. Są uprawnione do roli i ją włączyć i wyłączyć zawsze, gdy jest konieczne.

## <a name="roles-managed-in-pim"></a>Role zarządzania w PIM

Zarządzanie tożsamościami uprzywilejowanych umożliwia przypisywanie użytkowników do typowe role administratora, w tym:


- **Administrator globalny** (nazywane także administrator firmy) ma dostęp do wszystkich funkcji administracyjnych. Masz więcej niż jeden administrator globalny w Twojej organizacji. Osoba, która rejestruje się do zakupu usługi Office 365 automatycznie staje się administratora globalnego.
- **Uprzywilejowanych roli administratora** zarządza Azure AD PIM i aktualizuje przypisania roli dla innych użytkowników.  
- **Administrator rozliczeń** dokonuje zakupów, zarządza subskrypcjami, zarządza biletami pomocy technicznej i monitoruje kondycję usług.
- **Administrator haseł** zresetowanie hasła, zarządza żądaniami usług i monitoruje kondycję usług. Administratorzy haseł są ograniczone do resetowania hasła dla użytkowników.
- **Administrator usługi** zarządza żądaniami usług i monitoruje kondycja usługi.

  > [AZURE.NOTE] Jeśli korzystasz z usługi Office 365, następnie przed przypisanie roli administratora usługi użytkownikowi, najpierw przypisać użytkownika uprawnienia administracyjne do usługi, takie jak usługi Exchange Online.

- **Administrator zarządzający użytkownikami** zresetowanie hasła, monitoruje stan usług i zarządza kont użytkowników, grup użytkowników i żądania obsługi. Administrator zarządzający użytkownikami nie można usunąć administratora globalnego, tworzenie innych ról administratora lub resetowanie haseł rozliczeń, globalnego ani administratorów usług.
- **Administrator programu Exchange** ma dostęp administracyjny do usługi Exchange Online przy użyciu Centrum administracyjnego programu Exchange (EAC), a następnie wykonać niemal każde zadanie w usłudze Exchange Online.
- **Administrator programu SharePoint** ma dostęp administracyjny do usługi SharePoint Online przy użyciu Centrum administracyjnego usługi SharePoint Online i może wykonać niemal każde zadanie w usłudze SharePoint Online.
- **W programie Skype dla firm administrator** ma dostęp administracyjny do programu Skype dla firm przy użyciu programu Skype dla firm Centrum administracyjne i może wykonać niemal każde zadanie w programie Skype dla firm Online.

Przeczytaj te artykuły, aby uzyskać więcej informacji na temat [Przypisywanie ról administratora w Azure AD](active-directory-assign-admin-roles.md) i [Przypisywanie ról administratora w usłudze Office 365](https://support.office.com/article/Assigning-admin-roles-in-Office-365-eac4d046-1afd-4f1a-85fc-8219c79e1504).

<!--**PLACEHOLDER: The above article may not be the one we want since PIM gets roles from places other that Office 365**-->


Z PIM można [przypisać te role do użytkownika](active-directory-privileged-identity-management-how-to-add-role-to-user.md) , tak, aby użytkownik może [aktywować dla roli w razie potrzeby](active-directory-privileged-identity-management-how-to-activate-role.md).

Jeśli chcesz udzielić innym dostępu użytkownika do zarządzania w PIM sam role, do których użytkownik miał wymaga PIM opisano dalej w [jak udzielić dostępu do PIM](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).


<!-- ## The PIM Security Administrator Role **PLACEHOLDER: Need description of the Security Administrator role.**-->

## <a name="roles-not-managed-in-pim"></a>Role nie zarządzaniu w PIM

Role w usłudze Exchange Online lub SharePoint Online, z wyjątkiem wymienione powyżej, nie znajdują się w Azure AD i dlatego nie są widoczne w PIM. Aby uzyskać więcej informacji na temat zmieniania przypisania roli obowiązują w tych usług Office 365 zobacz [uprawnienia w usłudze Office 365](https://support.office.com/article/Permissions-in-Office-365-da585eea-f576-4f55-a1e0-87090b6aaa9d).

Azure subskrypcje i grup zasobów również nie są reprezentowane w Azure AD. Zarządzanie subskrypcjami Azure, Dowiedz się, [jak dodać lub zmienić role administratora Azure](../billing-add-change-azure-subscription-administrator.md) i uzyskać więcej informacji na temat Azure RBAC zobacz [Kontrola dostępu Azure Role-Based](role-based-access-control-configure.md).

<!--**The above links might be replaced by ones that are from within this documentation repository **-->


## <a name="user-roles-and-signing-in"></a>Role użytkowników i logowanie się
Dla niektórych usług firmy Microsoft i aplikacje przypisanie użytkownika do roli może nie być wystarczające, aby umożliwić temu użytkownikowi uprawnienia administratora.

Dostęp do portalu klasyczny Azure wymaga użytkownik jest administratorem usługi lub Współtworzenie administratora na subskrypcji usługi Azure, nawet jeśli użytkownik nie jest konieczne zarządzanie subskrypcjami Azure.  Na przykład do zarządzania ustawieniami konfiguracji dla Azure AD w portalu klasyczny, użytkownik musi być administratorem globalnym Azure AD jak administrator Współtworzenie subskrypcji na subskrypcji usługi Azure.  Aby dowiedzieć się, jak dodawać użytkowników do subskrypcji Azure, zobacz, [jak dodać lub zmienić role administratora Azure](../billing-add-change-azure-subscription-administrator.md).

Dostęp do usług Microsoft Online Services może wymagać użytkownika można przypisać również licencję przed ich Otwórz portal usługi lub wykonywanie zadań administracyjnych.

## <a name="assign-a-license-to-a-user-in-azure-ad"></a>Przypisywanie licencji do użytkownika w Azure AD

1. Zaloguj się do [Azure portal klasyczny] (http://manage.windowsazure.com) za pomocą konta administratora globalnego lub Współtworzenie administratora.
2. Zaznaczanie **Wszystkich elementów** w menu głównym.
3. Wybierz katalog, w którym chcesz pracować z a, który zawiera licencji skojarzone z nim.
4. Wybierz pozycję **licencje**. Zostanie wyświetlona lista dostępnych licencji.
5. Wybierz plan licencji, który zawiera licencji, które chcesz udostępnić.
6. Wybierz pozycję **Przypisz użytkowników**.
7. Wybierz użytkownika, którego chcesz przypisać licencję.
8. Kliknij przycisk **Przypisz** .  Użytkownik może teraz Zaloguj się do Azure.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Następne kroki
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]
