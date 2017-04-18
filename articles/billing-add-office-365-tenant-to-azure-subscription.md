<properties
    pageTitle="Używanie dzierżawy usługi Office 365 z subskrypcją usługi Azure | Microsoft Azure"
    description="Dowiedz się, jak dodać katalog usługi Office 365 (dzierżawy) do subskrypcji usługi Azure, aby wprowadzić skojarzenia."
    services=""
    documentationCenter=""
    authors="JiangChen79"
    manager="mbaldwin"
    editor=""
    tags="billing,top-support-issue"/>

<tags
    ms.service="billing"
    ms.workload="na"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="cjiang"/>

# <a name="associate-an-office-365-tenant-with-an-azure-subscription"></a>Kojarzenie dzierżawy usługi Office 365 z subskrypcją usługi Azure
Jeśli zarówno Azure, jak i usługi Office 365 subskrypcji został zakupiony oddzielnie w przeszłości, a teraz chcesz mieć dostępu do dzierżawy usługi Office 365 z subskrypcji Azure, jest proste to zrobić. W tym artykule pokazano, jak.

> [AZURE.NOTE] Ten artykuł nie dotyczy to klientów umowy Enterprise (EA).

## <a name="quick-guidance"></a>Szybkie wskazówki
Aby skojarzyć dzierżawy usługi Office 365 z subskrypcją usługi Azure, konto Azure umożliwia dodawanie dzierżawy usługi Office 365, a następnie skojarzyć subskrypcji Azure dzierżawy usługi Office 365.

## <a name="detailed-steps"></a>Szczegółowe kroki
W tym scenariuszu według Kelley tablica jest użytkownik, który ma subskrypcję na koncie usługi Azure kelley.wall@outlook.com. Według Kelley też ma subskrypcję usługi Office 365 przy użyciu konta kelley.wall@contoso.onmicrosoft.com. Teraz według Kelley chce uzyskać dostęp do dzierżawy usługi Office 365 z subskrypcją Azure.

![Zrzut ekranu przedstawiający usługi Azure Active Directory stanu](./media/billing-add-office-365-tenant-to-azure-subscription/s31_msa-aad-status.png)

![Zrzut ekranu usługi Office 365 admin center aktywni użytkownicy](./media/billing-add-office-365-tenant-to-azure-subscription/s32_office-365-user.png)

### <a name="prerequisites"></a>Wymagania wstępne
Skojarzenia działał poprawnie konieczne są następujące wymagania:

- Poświadczenia administratora usługi Azure subskrypcji są wymagane. Administratorów współpracujących nie można wykonać podzbiór czynności.
- Poświadczenia administratora globalnego dzierżawy usługi Office 365 są wymagane.
- Adres e-mail administratora usługi nie mogą być zawarte w dzierżawie usługi Office 365.
- Adres e-mail administratora usługi nie są zgodne z dowolnego administratora globalnego usługi dzierżawy usługi Office 365.
- Jeśli obecnie korzystasz z adresu e-mail konta Microsoft i konto organizacji, tymczasowo zmienić administratora usługi Azure subskrypcji, aby użyć innego konta Microsoft. [Strona Tworzenie konta konto Microsoft](https://signup.live.com/), możesz utworzyć nowe konto Microsoft.


Aby zmienić administratora usługi, wykonaj następujące kroki:

1. Zaloguj się do [portalu zarządzania kontem](https://account.windowsazure.com/subscriptions).
2. Wybierz subskrypcję, którą chcesz zmienić.
3. Kliknij przycisk **Edytuj szczegóły subskrypcji**.

    ![Informacje o subskrypcji zrzut ekranu przedstawiający Azure, z "subskrypcji" wyróżnioną pozycją Edytuj szczegóły](./media/billing-add-office-365-tenant-to-azure-subscription/s33_azure-edit-subscription-details.png)

4. W oknie dialogowym **ADMINISTRATOR usługi** wprowadź adres e-mail nowego administratora usługi.

    ![Zrzut ekranu przedstawiający okno dialogowe "Edytowanie subskrypcji"](./media/billing-add-office-365-tenant-to-azure-subscription/s34_change-subscription-service-admin.png)

### <a name="associate-the-office-365-tenant-with-the-azure-subscription"></a>Kojarzenie dzierżawy usługi Office 365 z subskrypcją Azure
Aby skojarzyć dzierżawy usługi Office 365 z subskrypcją Azure, wykonaj następujące czynności:

1.  Zaloguj się do [portalu zarządzania kontem](https://account.windowsazure.com/subscriptions) przy użyciu poświadczeń administratora usługi.
2.  W okienku po lewej stronie wybierz pozycję **Usługi ACTIVE DIRECTORY**.

    ![Zrzut ekranu z usługą Active Directory wpisu](./media/billing-add-office-365-tenant-to-azure-subscription/s35-classic-portal-active-directory-entry.png)

    > [AZURE.NOTE] Nie powinna być widoczna dzierżawy usługi Office 365. Jeśli zostanie wyświetlony, pomiń krok dalej.

    ![Zrzut ekranu: domyślny katalog usługi Azure Active Directory](./media/billing-add-office-365-tenant-to-azure-subscription/s36-aad-tenant-default.png)

3. Dodawanie dzierżawy usługi Office 365 do subskrypcji usługi Azure.

    . Wybierz pozycję **Nowy** > **katalogu** > **Utwórz niestandardowe**.

    ![Tworzenie niestandardowych zrzut ekranu przedstawiający usługi Azure Active Directory](./media/billing-add-office-365-tenant-to-azure-subscription/s37-aad-custom-create.png)

    b. Na stronie **Dodawanie katalogów** w **katalogu**wybierz pozycję **Użyj istniejącego katalogu**. Następnie należy zaznaczyć **mogę wylogowano się teraz**i wybierz opcję **Pełna** ![wykonywanie ikona](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).

    ![Zrzut ekranu: "Użyj istniejącego katalogu"](./media/billing-add-office-365-tenant-to-azure-subscription/s39_add-directory-use-existing.png)

    c. Po wylogowano się, zaloguj się przy użyciu poświadczeń administratora globalnego dzierżawy usługi Office 365.

    ![Zrzut ekranu usługi Office 365 administratora globalnego logowania](./media/billing-add-office-365-tenant-to-azure-subscription/s310_sign-in-global-admin-office-365.png)

    d. Wybierz pozycję **Kontynuuj**.

    ![Zrzut ekranu przedstawiający weryfikacji](./media/billing-add-office-365-tenant-to-azure-subscription/s311_use-contoso-directory-azure-verify.png)

    e. Wybierz pozycję **Wyloguj się teraz**.

    ![Zrzut ekranu przedstawiający wyrejestrowywania](./media/billing-add-office-365-tenant-to-azure-subscription/s312_use-contoso-directory-azure-confirm-and-sign-out.png)

    f. Zaloguj się do [portalu zarządzania kontem](https://account.windowsazure.com/subscriptions) przy użyciu poświadczeń administratora usługi.

    ![Zrzut ekranu przedstawiający Azure logowanie](./media/billing-add-office-365-tenant-to-azure-subscription/s313_azure-sign-in-service-admin.png)

    g. Powinien zostać wyświetlony dzierżawy usługi Office 365 na pulpicie nawigacyjnym.

    ![Zrzut ekranu przedstawiający pulpit nawigacyjny](./media/billing-add-office-365-tenant-to-azure-subscription/s314_office-365-tenant-appear-in-azure.png)

4. Zmień katalog skojarzonego z subskrypcją Azure.

    . Wybierz pozycję **Ustawienia**.

    ![Zrzut ekranu przedstawiający Azure ikonę klasyczny ustawienia portalu](./media/billing-add-office-365-tenant-to-azure-subscription/s315_azure-classic-portal-settings-icon.png)

    b. Wybierz subskrypcję Azure, a następnie wybierz **Edytuj katalogu**.
    ![Zrzut ekranu przedstawiający Azure subskrypcji Edytuj katalogu](./media/billing-add-office-365-tenant-to-azure-subscription/s316_azure-subscription-edit-directory.png)

    c. Wybierz przycisk **Dalej** ![ikona dalej](./media/billing-add-office-365-tenant-to-azure-subscription/s317_next-icon.png).

    ![Zrzut ekranu: "Zmień katalogu skojarzone"](./media/billing-add-office-365-tenant-to-azure-subscription/s318_azure-change-associated-directory.png)

    > [AZURE.WARNING] Zostanie wyświetlone ostrzeżenie usunięcie wszystkich administratorów współpracujących.

    ![Azure potwierdzić katalogu — mapowania](./media/billing-add-office-365-tenant-to-azure-subscription/s322_azure-confirm-directory-mapping.png)

    >[AZURE.WARNING] Ponadto spowoduje również usunięcie wszystkich użytkowników [Kontrola dostępu oparta na rolach (RBAC)](./active-directory/role-based-access-control-configure.md) z dostępem przypisane w istniejących grup zasobów. Ostrzeżenie, które otrzymujesz wzmianki tylko usunięcie administratorów współpracujących.

    ![przypisane użytkowników — usunięte--grup zasobów](./media/billing-add-office-365-tenant-to-azure-subscription/s325_assigned-users-removed-resource-groups.png)

    d. Wybierz opcję **Pełna** ![wykonywanie ikona](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).

5. Teraz możesz dodać konta organizacji usługi Office 365 jako administratorów współpracujących do dzierżawy usługi Azure Active Directory.

    . Wybierz kartę **ADMINISTRATORZY** , a następnie wybierz pozycję **Dodaj**.

    ![Zrzut ekranu przedstawiający Azure klasycznym portalu Administratorzy ustawienia](./media/billing-add-office-365-tenant-to-azure-subscription/s319_azure-classic-portal-settings-administrators.png)

    b. Wprowadź konto organizacji dzierżawy usługi Office 365, wybierz subskrypcję, Azure, a następnie wybierz **Wykonano** ![wykonywanie ikona](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).

    ![Zrzut ekranu przedstawiający Azure Współtworzenie administratora, okno dialogowe Dodawanie](./media/billing-add-office-365-tenant-to-azure-subscription/s320_azure-add-co-administrator.png)

    c. Wróć do karty **ADMINISTRATORZY** . Konto organizacji wyświetlane jako administrator współpracujących powinna być widoczna.

    ![Zrzut ekranu przedstawiający kartę administratorów](./media/billing-add-office-365-tenant-to-azure-subscription/s321_azure-co-administrator-added.png)

6. Następny możesz przetestować programu access z administratorem Współtworzenie.

    . Wyloguj się do portalu zarządzania kontami.

    b. Otwórz [portal Zarządzanie kontami](https://account.windowsazure.com/subscriptions) lub [Azure portal](https://portal.azure.com/).

    c. Jeśli strona logowania jest Azure zawiera łącze **Zaloguj się przy użyciu konta organizacji**, wybierz łącze. W przeciwnym razie pomiń ten krok.

    ![Zrzut ekranu przedstawiający Azure strona logowania](./media/billing-add-office-365-tenant-to-azure-subscription/3-sign-in-to-azure.png)

    d. Wprowadź poświadczenia administratora Współtworzenie, a następnie wybierz, **Zaloguj się**.

    ![Zrzut ekranu przedstawiający Azure strona logowania](./media/billing-add-office-365-tenant-to-azure-subscription/s324_azure-sign-in-with-co-admin.png)

## <a name="next-steps"></a>Następne kroki
Scenariusze powiązanych obejmują:

- Masz już subskrypcji usługi Office 365 oraz są gotowe do subskrypcji usługi Azure, ale chcesz użyć istniejących kont użytkowników usługi Office 365 dla subskrypcji Azure.
- Jesteś użytkownikiem Azure subskrybenta i chcesz uzyskać subskrypcji usługi Office 365 dla użytkowników istniejącego wystąpienia usługi Azure Active Directory.

Aby dowiedzieć się, jak wykonać te zadania, zobacz [Korzystanie z istniejącego usługi Office 365 kont z subskrypcją usługi Azure lub odwrotnie](billing-use-existing-office-365-account-azure-subscription.md).
