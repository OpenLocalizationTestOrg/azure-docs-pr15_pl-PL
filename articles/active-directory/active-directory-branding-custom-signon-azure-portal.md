<properties
pageTitle="Dostosowywanie strony logowania w podglądzie usługi Azure Active Directory | Microsoft Azure"
description="Dowiedz się, jak dodać firmowe Azure stronę logowania"
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
ms.date="09/30/2016"
ms.author="curtand"/>

# <a name="add-company-branding-to-your-sign-in-page-in-the-azure-active-directory-preview"></a>Dodawanie logo do strony logowania w podglądzie usługi Azure Active Directory firmy

Aby uniknąć zamieszania, wiele firm chcesz zastosować spójny wygląd i działanie we wszystkich witryn sieci Web i usług, które zarządzają. Podgląd Azure Active Directory obsługuje tę funkcję, co pozwala dostosować wygląd strony logowania z logo firmy i niestandardowe schematy kolorów. [Co to jest w podglądzie?](active-directory-preview-explainer.md) Strona logowania to strony, które pojawia się po zalogowaniu się do usługi Office 365 lub inne aplikacje oparte na sieci web, korzystających z Azure AD jako dostawcy tożsamości. Użytkownik interakcji z tej strony, aby wprowadzić poświadczenia.

Jeśli chcesz wyświetlić wizerunku firmy, kolory i inne dostosowania elementów na tej stronie, zobacz opis różnic między dwoma środowiskami następujące obrazy.

Zrzut ekranu przedstawiający i przykład strona logowania w usłudze Office 365 na komputerze stacjonarnym **przed** dostosowanie następujących czynności:

![Strona usługi Office 365 logowania przed dostosowywania](./media/active-directory-branding-custom-signon-azure-portal/sign-in-page-before-customization.png)

Zrzut ekranu przedstawiający i przykład strona logowania w usłudze Office 365 na komputerze stacjonarnym **po** dostosowanie następujące czynności:

![Strona usługi Office 365 logowania po dostosowywania](./media/active-directory-branding-custom-signon-azure-portal/sign-in-page-after-customization.png)


## <a name="customizing-the-sign-in-page"></a>Dostosowywanie strony logowania

Zwykle Aby uzyskać dostęp do chmury aplikacji i usług, które subskrybuje organizacja za pośrednictwem przeglądarki, użyj strony logowania.

Zmiany zastosowanie do strony logowania go może trwać do godziny są wyświetlane zmiany.

Oznakowanych strony logowania jest wyświetlana tylko podczas odwiedzania usługi przy użyciu adresu URL specyficzne dla dzierżawy, takie jak https://outlook.com/**contoso**.com lub https://mail. **Contoso**. com.

Podczas odwiedzania usługi przy użyciu określonego adresu URL nie dzierżawy (przykład: https://mail.office365.com),-marki logowania zostanie wyświetlona strona. w tym przypadku znakowaniu pojawia się po wprowadzeniu swojego Identyfikatora użytkownika lub po zaznaczeniu kafelka użytkownika.

> [AZURE.NOTE]
>
- Nazwy domeny musi znajdować się jako "Aktywną" w części **domen** portal Azure, w którym skonfigurowano znakowania. Aby uzyskać więcej informacji zobacz [Dodawanie domeny niestandardowej nazwy](active-directory-domains-add-azure-portal.md).
- Strona logowania związane ze znakowaniem, nie przenoszenia do strony logowania dla klientów indywidualnych na stronie firmy Microsoft. Jeśli możesz zalogować się przy użyciu konta Microsoft, może wyświetlić listę oznakowanych Kafelki użytkownika przez Azure AD, ale znakowanie Twoja organizacja nie ma zastosowania do konta Microsoft logowania strony.

Na stronie logowania pole wyboru **Nie wylogowuj mnie** umożliwia użytkownikowi pozostaną zalogowania po zamknięciu i ponownym otwarciu przeglądarki. 

   ![Mnie zalogować](./media/active-directory-branding-custom-signon-azure-portal/01.png)

Nie będzie miało znaczenia istnienia sesji. Można wyświetlić wszystkie pola wyboru na stronie logowania usługi Azure Active Directory.
Czy są wyświetlane pola wyboru zależy od ustawienie **Nie wylogowuj wyłączone mnie**.

   ![Mnie zalogować](./media/active-directory-branding-custom-signon-azure-portal/02.png)


Aby ukryć pola wyboru, to ustawienie **Tak**skonfigurować. 

> [AZURE.NOTE] Niektóre funkcje programu SharePoint Online i Office 2010 są zależne od użytkowników możliwość zaznacz to pole wyboru. Konfigurując to ustawienie, aby ukryte, użytkownicy mogą być wyświetlane dodatkowe i nieoczekiwane monity o zalogowanie się.




**Aby dodać logo do katalogu firmy:**

1.  Zaloguj się do [portalu Azure](https://portal.azure.com) za pomocą konta, który jest administratorem globalnym katalogu.

2.  Wybierz pozycję **więcej usług**, wprowadź **użytkowników i grup** w polu tekstowym, a następnie naciśnij **klawisz Enter**.

    ![Otwieranie, zarządzanie użytkownikami](./media/active-directory-branding-custom-signon-azure-portal/user-management.png)

3. Na karta **Użytkownicy i grupy** wybierz **znakowania firmy**.

4. Na **użytkowników i grup — firmy związane ze znakowaniem** karta, wybierz polecenie **Edytuj** .

    ![Edytowanie niestandardowego związane ze znakowaniem](./media/active-directory-branding-custom-signon-azure-portal/edit-branding.png)

5. Modyfikowanie elementów, który chcesz dostosować. Wszystkie elementy są opcjonalne.

6. Kliknij przycisk **Zapisz**.

Może potrwać do godziny dla wszelkie zmiany wprowadzone do strony logowania związane ze znakowaniem się pojawić.

## <a name="next-steps"></a>Następne kroki

[Dodawanie logo firmy specyficzne dla języka](active-directory-branding-localize-azure-portal.md)
