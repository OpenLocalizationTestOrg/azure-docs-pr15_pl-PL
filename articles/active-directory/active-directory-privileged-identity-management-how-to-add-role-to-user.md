<properties
   pageTitle="Jak dodać lub usunąć roli użytkownika | Microsoft Azure"
   description="Dowiedz się, jak dodać role uprzywilejowanych tożsamości z aplikacją Azure Active Directory uprzywilejowanych Zarządzanie tożsamościami."
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
   ms.date="10/24/2016"
   ms.author="kgremban"/>

# <a name="azure-ad-privileged-identity-management-how-to-add-or-remove-a-user-role"></a>Azure AD uprzywilejowanych Zarządzanie tożsamościami: Jak dodawanie lub usuwanie roli użytkownika

Z Azure Active Directory (AD), administratora globalnego (lub administrator firmy) można aktualizować użytkowników, którzy są **trwale** przypisane do ról w Azure AD. To zrobić przy użyciu poleceń cmdlet programu PowerShell, takich jak `Add-MsolRoleMember` i `Remove-MsolRoleMember`. Lub mogą również używać portalu klasyczny Azure zgodnie z opisem w [temacie Przypisywanie ról administratora w usłudze Azure Active Directory](active-directory-assign-admin-roles.md).

Aplikacja Azure AD uprzywilejowanych Zarządzanie tożsamościami umożliwia administratorom uprzywilejowanych roli wprowadź przypisania roli trwały, a także. Ponadto uprzywilejowanych roli Administratorzy mogą wprowadzać użytkownicy **kwalifikuje się** do ról administratora. Administrator odpowiedniej można aktywować roli, gdy są potrzebne, a następnie ich uprawnienia wygasają po ich wprowadzeniu zmian.

## <a name="manage-roles-with-pim-in-the-azure-portal"></a>Zarządzanie rolami PIM w portalu Azure

W organizacji użytkowników można przypisać różne role administracyjne w Azure AD usługi Microsoft Office 365 i inne i aplikacje.  Więcej informacji na temat dostępnych ról można znaleźć w [ról w Azure AD PIM](active-directory-privileged-identity-management-roles.md).

Aby dodać lub usunąć użytkownika należącego do roli za pomocą uprzywilejowanych Zarządzanie tożsamościami, wywoływanie PIM pulpitu nawigacyjnego. Następnie kliknij przycisk **użytkowników w ról administratora** , lub zaznacz konkretnej roli (na przykład Administrator globalny) z tabeli role.

> [AZURE.NOTE] Jeśli jeszcze nie zostało włączone PIM w Azure portal, przejdź do [wprowadzenie Azure AD PIM](active-directory-privileged-identity-management-getting-started.md) , aby uzyskać szczegółowe informacje.

Jeśli chcesz udzielić innym dostępu użytkownika do PIM samej, ról, które PIM wymaga użytkownik miał opisano dalej w [jak udzielić dostępu do PIM](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).

## <a name="add-a-user-to-a-role"></a>Dodawanie użytkownika do roli

1. W [portalu Azure](https://portal.azure.com/)zaznacz opcję Podziel **Azure AD uprzywilejowanych Zarządzanie tożsamościami** na pulpicie nawigacyjnym.
2. Wybierz pozycję **Zarządzanie rolami uprzywilejowanych**.
3. W tabeli **roli podsumowanie** wybierz rolę, którą chcesz zarządzać.
4. W karta roli wybierz pozycję **Dodaj**.
5. Kliknij przycisk **Wybierz użytkowników** i wyszukiwania dla użytkowników karta **Wybierz pozycję Użytkownicy** .  
6. Wybierz użytkownika z listy wyników wyszukiwania, a następnie kliknij przycisk **Gotowe**.
4. Kliknij **przycisk OK** , aby zapisać wybrane. Użytkownik, który wybrano będą wyświetlane na liście jako kwalifikujące się do roli.

> [AZURE.NOTE]
>Nowych użytkowników w roli tylko kwalifikuje się do roli domyślnie. Jeśli chcesz wprowadzić trwałe roli, kliknij użytkownika, na liście. Informacje o użytkowniku pojawią się w nowych kart. Wybierz opcję **Wprowadź uprawnienie** menu informacji użytkownika.  
>Jeśli użytkownik nie może zarejestrować dla Azure uwierzytelnianie wieloskładnikowe (MFA) lub jest za pomocą konta Microsoft (zazwyczaj @outlook.com), należy je stałe w ich role. Administratorzy kwalifikuje się monit o zarejestrować MFA w trakcie procesu aktywacji.

Teraz, gdy użytkownik jest uprawniony do roli, powiadom tę osobę, że one go uaktywnić, zgodnie z instrukcjami podanymi w [temacie jak aktywować lub dezaktywować rolę](active-directory-privileged-identity-management-how-to-activate-role.md).

## <a name="remove-a-user-from-a-role"></a>Usuwanie użytkownika z roli

Można usunąć użytkowników z przypisania roli uprawniony, ale upewnij się, że jest zawsze co najmniej jednego użytkownika, trwały administratora globalnego.

Wykonaj poniższe czynności, aby usunąć użytkowników z roli:

1. Przejdź do roli na liście role, wybierając roli na pulpicie nawigacyjnym Azure AD PIM lub klikając przycisk **użytkowników w ról administratora** .
2. Kliknij użytkownika na liście użytkowników.
3. Kliknij przycisk **Usuń**. Wiadomość zostanie wyświetlony monit o potwierdzenie.
4. Kliknij przycisk **Tak,** Aby usunąć rolę użytkownika.

Jeśli nie masz pewności, które użytkownicy będą nadal ich przypisania roli, następnie można [uruchomić przeglądu dostęp dla roli](active-directory-privileged-identity-management-how-to-start-security-review.md).


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Następne kroki
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]
