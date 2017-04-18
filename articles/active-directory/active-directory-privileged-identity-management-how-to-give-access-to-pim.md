<properties
   pageTitle="Jak udzielić dostępu do PIM | Microsoft Azure"
   description="Dowiedz się, jak dodawać role użytkowników z rozszerzeniem Azure Active Directory uprzywilejowanych Zarządzanie tożsamościami, aby zarządzają PIM."
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
   ms.date="09/22/2016"
   ms.author="kgremban"/>

# <a name="how-to-give-access-to-manage-azure-ad-privileged-identity-management"></a>Jak udzielić dostępu do zarządzania Azure AD uprzywilejowanych Zarządzanie tożsamościami

Administrator globalny, który pozwala automatycznie Azure AD uprzywilejowanych tożsamości zarządzanie dla organizacji Uzyskaj przypisania ról i dostęp do PIM. Nikt otrzymuje zapisu domyślnie, jednak w tym innych administratorów globalnych. Inne globalnej Administratorzy, Administratorzy zabezpieczeń i czytników zabezpieczeń mają dostęp tylko do odczytu, aby Azure AD PIM. Aby udzielić dostępu do PIM, pierwszego użytkownika można przypisywać innym użytkownikom do roli **administratora uprzywilejowanych roli** . Przypisania musi być wykonywane za pomocą programu PIM się i nie można zmienić przy użyciu programu PowerShell lub innych portali.

> [AZURE.NOTE] Zarządzanie Azure AD PIM wymaga Azure MFA. Ponieważ nie można rejestrować konta serwera Microsoft Azure MFA, użytkownik logujący się przy użyciu konta Microsoft nie ma dostępu Azure AD PIM.

Upewnij się, jest zawsze co najmniej dwóch użytkowników w roli administratora uprzywilejowanych ról w przypadku, gdy jeden użytkownik jest zablokowane lub ich konto zostanie usunięte.

## <a name="give-another-user-access-to-manage-pim"></a>Udzielanie innym dostępu użytkownika do zarządzania PIM

1. Zaloguj się do [portalu Azure](https://portal.azure.com/) i wybierz aplikację **Azure AD uprzywilejowanych Zarządzanie tożsamościami** , na pulpicie nawigacyjnym.
2. Wybierz pozycję **Zarządzanie rolami uprzywilejowanych** > **uprzywilejowanych roli administratora** > **Dodaj**.

    ![Dodawanie administratorów uprzywilejowanych roli — zrzut ekranu][1]

4. Na Dodaj karta zarządzanych użytkowników krok 1 jest już zakończone. Wybierz krok 2, **Zaznacz użytkowników** i wyszukiwania dla użytkownika, którego chcesz dodać.

    ![Wybierz użytkowników — zrzut ekranu][2]

6. Wybierz użytkownika z wyników wyszukiwania, a następnie kliknij przycisk **Gotowe**.
7. Kliknij **przycisk OK** , aby zapisać wybrane. Użytkownik, który wybrano pojawi się na liście administratorów uprzywilejowanych roli.

    - Gdy przypisujesz nowej roli osoby, automatycznie ustawienia pomocy kwalifikują się aby aktywować dla roli. Jeśli chcesz były stałe w roli, kliknij użytkownika, na liście. Wybierz opcję **Wprowadź uprawnienie** menu informacji użytkownika.

8. Wyślij użytkownika łącza do [wprowadzenie Azure AD uprzywilejowanych Zarządzanie tożsamościami](active-directory-privileged-identity-management-getting-started.md).


## <a name="remove-another-users-access-rights-for-managing-pim"></a>Usuwanie praw dostępu do innego użytkownika do zarządzania PIM

Zawsze przed usunąć kogoś z roli administratora uprzywilejowanych roli, upewnij się, nadal będzie dwóch użytkowników do niego przypisana.

1. Na pulpicie nawigacyjnym PIM wybierz polecenie roli **uprzywilejowanych roli administratora**.  Zostanie wyświetlona lista użytkowników obecnie w tej roli.
2. Kliknij użytkownika na liście użytkowników.
3. Wybierz polecenie **Usuń**.  Zostanie wyświetlony komunikat z potwierdzeniem.
4. Kliknij przycisk **Tak,** Aby usunąć użytkownika z roli.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Następne kroki
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-how-to-give-access-to-pim/PIM_add_PRA.png
[2]: ./media/active-directory-privileged-identity-management-how-to-give-access-to-pim/PIM_select_users.png
