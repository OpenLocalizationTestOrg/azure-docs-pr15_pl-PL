<properties
pageTitle="Dodawanie logo do strony logowania w podglądzie usługi Azure Active Directory firmy specyficzne dla języka | Microsoft Azure"
description="Dowiedz się, jak dodać język firmy związane ze znakowaniem, obrazy i tekst Azure stronę logowania"
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

# <a name="add-language-specific-company-branding-to-your-sign-in-page-in-the-azure-active-directory-preview"></a>Dodawanie logo do strony logowania w podglądzie usługi Azure Active Directory firmy specyficzne dla języka

Aby uniknąć zamieszania, wiele firm chcesz zastosować spójny wygląd i działanie we wszystkich witryn sieci Web i usług, które zarządzają. Podgląd Azure Active Directory obsługuje tę funkcję, co pozwala dostosować wygląd strony logowania z logo firmy i niestandardowe schematy kolorów. [Co to jest w podglądzie?](active-directory-preview-explainer.md) Strona logowania to strony, które pojawia się po zalogowaniu się do usługi Office 365 lub inne aplikacje oparte na sieci web, korzystających z Azure AD jako dostawcy tożsamości. Użytkownik interakcji z tej strony, aby wprowadzić poświadczenia.

## <a name="customizing-the-sign-in-page-for-another-language"></a>Dostosowywanie strony logowania w innym języku

Możesz dodać elementy specyficzne dla języka do niestandardowej strony logowania tylko wtedy, gdy zostały już utworzone niestandardowej strony logowania jako opisano w sekcji [Dodawanie firmowe do strony logowania](active-directory-branding-custom-signon-azure-portal.md). Strona logowania jednego każdego katalogu można skonfigurować za pomocą domyślnego zestawu elementów, które można dostosować. Po skonfigurowaniu domyślny zestaw elementów strony można skonfigurować więcej wersji dla innych języków. Można również mieszać i dopasowywać różnych elementów. Na przykład możesz:

- Tworzenie domyślnego **logowania obrazu strony** , który działa w przypadku wszystkich kultury, a następnie utwórz określonych wersjach dla języka angielskiego i francuski. Po ustawieniu przeglądarek na jeden z tych dwóch języków pojawi się obraz specyficzne dla języka, podczas ilustracji domyślny jest wyświetlany dla wszystkich innych języków.

- Konfigurowanie różnych logo dla Twojej organizacji (na przykład wersje japoński lub hebrajski).

Zaleca się pozostawienie liczbę odmiany języka niska wydajność i konserwacja powodów.

**Aby dodać logo do katalogu firmy:**

1.  Zaloguj się do [portalu Azure](https://portal.azure.com) za pomocą konta, który jest administratorem globalnym katalogu.

2.  Wybierz pozycję **więcej usług**, wprowadź **użytkowników i grup** w polu tekstowym, a następnie naciśnij **klawisz Enter**.

    ![Otwieranie, zarządzanie użytkownikami](./media/active-directory-branding-localize-azure-portal/user-management.png)

3. Na karta **Użytkownicy i grupy** wybierz **znakowania firmy**.

4. Na **użytkowników i grup — firmy związane ze znakowaniem** karta, wybierz polecenie **Dodaj język** .

    ![Dodawanie elementów marki specyficzne dla języka](./media/active-directory-branding-localize-azure-portal/add-language.png)

5. Modyfikowanie elementów, który chcesz dostosować. Wszystkie elementy są opcjonalne.

6. Kliknij przycisk **Zapisz**.

Może potrwać do godziny dla wszelkie zmiany wprowadzone do strony logowania związane ze znakowaniem się pojawić.

## <a name="next-steps"></a>Następne kroki

[Dodawanie logo do strony logowania firmy](active-directory-branding-custom-signon-azure-portal.md)
