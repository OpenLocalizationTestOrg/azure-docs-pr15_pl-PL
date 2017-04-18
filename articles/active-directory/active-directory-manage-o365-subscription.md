<properties
   pageTitle="Zarządzanie katalogu dla subskrypcji usługi Office 365, platformy Azure | Microsoft Azure"
   description="Zarządzanie katalogu subskrypcji usługi Office 365 przy użyciu usługi Azure Active Directory i portalu klasyczny Azure"
   services="active-directory"
   documentationCenter=""
   authors="curtand"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/23/2016"
   ms.author="curtand"/>

# <a name="manage-the-directory-for-your-office-365-subscription-in-azure"></a>Zarządzanie katalogu dla subskrypcji usługi Office 365, platformy Azure

W tym artykule opisano, jak zarządzać katalogu, w którym została utworzona dla subskrypcji usługi Office 365 za pomocą portalu klasyczny Azure. Musisz być administratorem usługi lub Współtworzenie administratora subskrypcji usługi Azure, aby zalogować się do portalu klasyczny Azure. Jeśli nie masz jeszcze Azure subskrypcji, możesz Załóż [bezpłatne 30-dniową wersję próbną](https://azure.microsoft.com/trial/get-started-active-directory/) już dziś i wdrażanie rozwiązania pierwszej cloud w obszarze 5 minut, przy użyciu tego łącza. Pamiętaj za pomocą konta służbowego, którego używasz, aby zalogować się do usługi Office 365.

Po zakończeniu Azure subskrypcji, możesz zalogować się do portalu klasyczny Azure i uzyskać dostęp do usług Azure. Kliknij pozycję rozszerzenie usługi Active Directory do zarządzania tym samym katalogu, który uwierzytelnia użytkowników usługi Office 365.

Jeśli masz subskrypcję usługi Azure, procedura przejęcia zarządzania dodatkowe katalogu jest również bardzo proste. Na przykład Kit Michael może być subskrypcji usługi Office 365 dla Contoso.com. Ma on utworzył konta w usłudze przy użyciu swojego konta Microsoft Azure subskrypcji msmith@hotmail.com. W tym przypadku zarządza dwa poziomy.

  Subskrypcji |  Usługi Office 365  |  Azure
  -------------- | ------------- | -------------------------------
  Nazwa wyświetlana |  Contoso  |     Domyślny katalog usługi Azure Active Directory (Azure AD)
  Nazwa domeny  |  contoso.com  | msmithhotmail.onmicrosoft.com

Chce zarządzanie tożsamością użytkowników w katalogu firmy Contoso, podczas jego jest zalogowany przy użyciu swojego konta Microsoft Azure tak, aby on Włączanie funkcji Azure AD, takich jak uwierzytelnianie wieloskładnikowe. Aby przedstawić proces może pomóc w poniższym diagramie.

![Diagram, aby zarządzać dwóch niezależnych katalogów](./media/active-directory-manage-o365-subscription/AAD_O365_03.png)

W tym przypadku dwa katalogi są niezależne od siebie.

## <a name="to-manage-two-independent-directories"></a>Aby zarządzać dwóch niezależnych katalogów
Aby Kit Michael Zarządzanie obu katalogów, gdy jest on zalogowany Azure jako msmith@hotmail.com, on musi wykonać następujące czynności:

> [AZURE.NOTE]
> Tylko wtedy, gdy użytkownik jest zalogowany przy użyciu konta Microsoft, można wykonać te czynności. Jeśli użytkownik jest zalogowany przy użyciu usługi lub konta służbowego, opcja **Użyj istniejącego katalogu** nie jest dostępna. Konto służbowe może zostać uwierzytelniony tylko przez jego katalogu macierzystego (oznacza to, że katalog miejsce, w którym znajduje się konto służbowe, po czym firma lub szkoła właścicielem).

1.  Zaloguj się do [portalu klasyczny Azure](https://manage.windowsazure.com) jako msmith@hotmail.com.
2.  Kliknij przycisk **Nowy** > **aplikacji usług** > **Usługi Active Directory** > **katalogu** > **Utworzyć niestandardowe**.
3.  Kliknij pozycję Użyj istniejącego katalogu, a następnie zaznacz pole wyboru **mogę wylogowano się teraz** .
4.  Zaloguj się do portalu klasyczny Azure jako administrator globalny: Contoso.onmicrosoft.com (na przykład msmith@contoso.com).
5.  Po wyświetleniu monitu o **katalogu firmy Contoso za pomocą Azure?**, kliknij przycisk **Kontynuuj**.
6.  Kliknij pozycję **Wyloguj się teraz**.
7.  Zaloguj się do portalu klasyczny Azure jako msmith@hotmail.com. Z katalogu firmy Contoso i domyślny katalog są wyświetlane w rozszerzenie usługi Active Directory.

Po wykonaniu tych czynności msmith@hotmail.com administratora globalnego w katalogu firmy Contoso.

## <a name="to-administer-resources-as-the-global-admin"></a>Administrowanie zasobami jako administrator globalny
Teraz załóżmy, że Anna Nowak wymaga administrowanie witrynami sieci Web i bazy danych zasobów, które są skojarzone z subskrypcją Azure dla msmith@hotmail.com. Przed Anna można to zrobić, Michael Kit musi wykonać następujące dodatkowe kroki:

1.  Zaloguj się do [portalu klasyczny Azure](https://manage.windowsazure.com) przy użyciu konta administratora usługi Azure subskrypcji (w tym przykładzie msmith@hotmail.com).
2.  Przełączanie subskrypcji do katalogu firmy Contoso: kliknij pozycję **Ustawienia** > **Subskrypcje** > Wybierz subskrypcję > **Edytuj katalogu** > Zaznacz **Contoso (Contoso.com)**. W ramach transferu, pracy lub szkole konta, które są administratorów współpracujących subskrypcji zostaną usunięte.
3.  Dodawanie Anna Nowak administrator Współtworzenie subskrypcji: kliknij pozycję **Ustawienia** > **Administratorzy** > Wybierz subskrypcję > **Dodaj** > typ **JohnDoe@Contoso.com**.

## <a name="next-steps"></a>Następne kroki
Aby uzyskać więcej informacji na temat relacji między subskrypcje i katalogów zobacz [jak subskrypcji jest skojarzony z katalogu](active-directory-how-subscriptions-associated-directory.md).
