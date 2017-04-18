<properties
    pageTitle="Udostępnianie pojedynczej dzierżawy Azure AD w subskrypcjach usługi Office 365 i Azure | Microsoft Azure"
    description="Informacje o udostępnianiu dzierżawy usługi Office 365 Azure AD i jej użytkowników z subskrypcją usługi Azure lub odwrotnie"
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
    ms.date="08/17/2016"
    ms.author="cjiang"/>

# <a name="use-an-existing-office-365-account-with-your-azure-subscription-or-vice-versa"></a>Użyj istniejącego konta usługi Office 365 z subskrypcją usługi Azure lub odwrotnie
Scenariusz: Już masz subskrypcję usługi Office 365 i są gotowe do subskrypcji usługi Azure, ale chcesz użyć istniejących kont użytkowników usługi Office 365 dla subskrypcji Azure. Możesz też są Azure subskrybenta i chcesz uzyskać subskrypcji usługi Office 365 dla użytkowników w swojej istniejącej usługi Azure Active Directory. W tym artykule pokazano, jak łatwo jest osiągnięcie obu.

> [AZURE.NOTE] Ten artykuł nie dotyczy to klientów umowy Enterprise (EA). Jeśli potrzebujesz dodatkowej pomocy w dowolnym momencie, w tym artykule, [kontakt z pomocą techniczną](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) uzyskanie szybko rozwiązać problem.


## <a name="quick-guidance"></a>Szybkie wskazówki

- Jeśli już masz subskrypcję usługi Office 365 i chcesz utworzyć konto Azure, za pomocą opcji **Zaloguj się przy użyciu konta organizacyjnego** . Następnie kontynuuj Azure proces tworzenia konta przy użyciu konta usługi Office 365. Zobacz [szczegółowe kroki w dalszej części tego artykułu](#s1).

- Jeśli już masz subskrypcję usługi Azure i chcesz uzyskać subskrypcji usługi Office 365, zaloguj się do usługi Office 365 za pomocą konta usługi Azure. Kontynuuj zapisów czynności. Po zakończeniu tworzenia konta subskrypcji usługi Office 365 zostaną dodane do tego samego wystąpienia usługi Azure Active Directory, należący do subskrypcji usługi Azure. Aby uzyskać więcej informacji zobacz sekcję [uzyskać szczegółowe instrukcje w dalszej części tego artykułu](#s2).

>[AZURE.NOTE] Aby uzyskać subskrypcji usługi Office 365, konta używasz na potrzeby tworzenia konta musi należeć do roli Administrator globalny lub administrator rozliczeń katalogów w dzierżawie usługi Azure Active Directory. [Dowiedz się, jak określić roli w usłudze Azure Active Directory](#how-to-know-your-role-in-your-azure-active-directory).

Aby dowiedzieć się, co się dzieje po dodaniu subskrypcji do konta, zobacz [informacje w dalszej części tego artykułu](#background-information).

## <a name="detailed-steps"></a>Szczegółowe kroki
<a id="s1"></a>
### <a name="scenario-1-office-365-users-who-plan-to-buy-azure"></a>Scenariusz 1: Użytkownicy usługi Office 365, którzy plan do kupienia Azure
W tym scenariuszu przyjęto założenie, że użytkownik ma subskrypcję usługi Office 365 i planuje subskrybowanie Azure jest według Kelley ścian. Istnieją dwie dodatkowe aktywni użytkownicy, Anna i Tricia. Konto i według Kelley jest admin@contoso.onmicrosoft.com.

![Centrum administracyjne usługi Office 365 użytkownika](./media/billing-use-existing-office-365-account-azure-subscription/1-office365-users-admin-center.png)

Aby utworzyć konto Azure, wykonaj następujące czynności:

1. Załóż konto Azure u [Azure.com](https://azure.microsoft.com/). Kliknij pozycję **Wypróbuj bezpłatnie**. Na następnej stronie kliknij pozycję **Rozpocznij teraz**.

    ![Wypróbuj bezpłatnie Azure.](./media/billing-use-existing-office-365-account-azure-subscription/2-azure-signup-try-free.png)

2. Kliknij pozycję **Zaloguj się przy użyciu konta organizacyjnego**.

    ![Zaloguj się do Azure.](./media/billing-use-existing-office-365-account-azure-subscription/3-sign-in-to-azure.png)

3. Zaloguj się przy użyciu konta usługi Office 365. W tym przypadku jest według Kelley na konto usługi Office 365.

    ![Zaloguj się przy użyciu konta usługi Office 365.](./media/billing-use-existing-office-365-account-azure-subscription/4-sign-in-with-org-account.png)

4. Wypełnij informacje i ukończyć proces tworzenia konta.

    ![Wypełnij informacje i Kończenie tworzenia konta.](./media/billing-use-existing-office-365-account-azure-subscription/5-azure-sign-up-fill-information.png)

    ![Kliknij przycisk Rozpocznij zarządzanie usługą Moje.](./media/billing-use-existing-office-365-account-azure-subscription/6-azure-start-managing-my-service.png)

Teraz wszystko jest gotowe. W portalu usługi Azure powinna być widoczna tym użytkownikom wyświetlane. Aby to sprawdzić, wykonaj następujące czynności:

1. Kliknij przycisk **Start, zarządzanie usługą Moje** na obrazie pokazano wcześniej.
2. Kliknij przycisk **Przeglądaj**, a następnie kliknij pozycję **Usługi Active Directory**.

    ![Kliknij przycisk Przeglądaj, a następnie kliknij pozycję usługi Active Directory.](./media/billing-use-existing-office-365-account-azure-subscription/7-azure-portal-browse-ad.png)

3. Kliknij pozycję **Użytkownicy**.

    ![Na karcie Użytkownicy](./media/billing-use-existing-office-365-account-azure-subscription/8-azure-portal-ad-users-tab.png)

4. Wszystkich użytkowników, w tym według Kelley, znajdują się zgodnie z oczekiwaniami.

    ![Listy użytkowników](./media/billing-use-existing-office-365-account-azure-subscription/9-azure-portal-ad-users.png)

<a id="s2"></a>
### <a name="scenario-2-azure-users-who-plan-to-buy-office-365"></a>Scenariusz 2: Azure użytkowników, którzy planowanie zakupu usługi Office 365

W tym scenariuszu według Kelley tablica jest użytkownik, który ma subskrypcję na koncie usługi Azure admin@contoso.onmicrosoft.com. Według Kelley chce subskrypcja usługi Office 365 i używanie tego samego katalogu, który ma już z platformy Azure.

>[AZURE.NOTE] Uzyskiwania subskrypcji usługi Office 365, konto, którego używasz do logowania musi należeć do roli Administrator globalny lub administrator rozliczeń katalogów w dzierżawie usługi Azure Active Directory. [Dowiedz się, jak ustalić roli w usługi Azure Active Directory](#how-to-know-your-role-in-your-azure-active-directory).

![Ustawienia subskrypcji portal Azure](./media/billing-use-existing-office-365-account-azure-subscription/10-azure-portal-settings-subscription.png)

![Azure portalu użytkowników usługi Active Directory](./media/billing-use-existing-office-365-account-azure-subscription/11-azure-portal-ads-users.png)

Aby zasubskrybować usługi Office 365, wykonaj następujące czynności:

1. Przejdź do [strony produktu usługi Office 365](https://products.office.com/business), a następnie wybierz plan, który jest odpowiedni dla Ciebie.
2. Po zaznaczeniu tego planu, zostanie wyświetlona strona następujące. Nie trzeba wypełniać formularz. W prawym górnym rogu strony kliknij przycisk **Zaloguj** .

    ![Strona wersji próbnej usługi Office 365](./media/billing-use-existing-office-365-account-azure-subscription/12-office-365-trial-page.png)

3. Zaloguj się przy użyciu poświadczeń konta. W tym przykładzie jest konto według Kelley osoby.

    ![Logowanie usługi Office 365](./media/billing-use-existing-office-365-account-azure-subscription/13-office-365-sign-in.png)

4. Kliknij pozycję **Wypróbuj teraz**.

    ![Potwierdź zamówienie dla usługi Office 365.](./media/billing-use-existing-office-365-account-azure-subscription/14-office-365-confirm-your-order.png)

5. Na stronie Potwierdzenie zamówienia kliknij przycisk **Kontynuuj**.

    ![Potwierdzenie zamówienia usługi Office 365](./media/billing-use-existing-office-365-account-azure-subscription/15-office-365-order-receipt.png)

Teraz wszystko jest gotowe. W Centrum administracyjnym usługi Office 365 powinna być widoczna użytkowników z katalogu firmy Contoso wyświetlane jako aktywni użytkownicy. Aby to sprawdzić, wykonaj następujące czynności:

1. Otwórz Centrum administracyjne usługi Office 365.
2. Rozwiń **użytkowników**, a następnie kliknij pozycję **Aktywni użytkownicy**.

    ![Użytkownicy Centrum administracyjnego usługi Office 365](./media/billing-use-existing-office-365-account-azure-subscription/16-office-365-admin-center-users.png)

### <a name="how-to-know-your-role-in-your-azure-active-directory"></a>Jak sprawdzić, Twoja rola w usłudze Active Directory platformy Azure

1. Zaloguj się do [portalu Azure](https://portal.azure.com/).
2. Kliknij przycisk **Przeglądaj**, a następnie kliknij pozycję **Usługi Active Directory**.

    ![Usługi Active Directory w portalu Azure](./media/billing-use-existing-office-365-account-azure-subscription/7-azure-portal-browse-ad.png)

3. Kliknij pozycję **Użytkownicy**.

    ![Użytkownicy usługi Active Directory Azure domyślnej portalu](./media/billing-use-existing-office-365-account-azure-subscription/17-azure-portal-default-ad-users.png)

4. Kliknij użytkownika. W tym przykładzie użytkownik jest według Kelley ścian.

    Zwróć uwagę, pole roli **Organizacji**.

    ![Tożsamość użytkownika portal Azure](./media/billing-use-existing-office-365-account-azure-subscription/18-azure-portal-user-identity.png)

## <a name="background-information-about-azure-and-office-365-subscriptions"></a>Informacje o subskrypcji Azure i usługi Office 365
Usługa Office 365 i Azure za pomocą usługi Azure Active Directory do zarządzania użytkownikami i subskrypcji. Należy rozważyć Azure katalogu jako kontenera, w którym można grupować subskrypcji i użytkowników. Aby użyć tego samego konta użytkownika dla subskrypcji Azure i usługi Office 365, należy upewnić się, że subskrypcje są tworzone w tym samym katalogu. Należy pamiętać następujące punkty:

- Subskrypcja zostanie utworzony w katalogu, nie na odwrót.
- Użytkownicy należą do katalogów, a nie na odwrót.
- Subskrypcja znajdzie się w katalogu użytkownik tworzący subskrypcji. W wyniku subskrypcji usługi Office 365 jest związany z tym samym kontem jako subskrypcji Azure podczas korzystania z tego konta do utworzenia subskrypcji usługi Office 365.

![Informacje ogólne](./media/billing-use-existing-office-365-account-azure-subscription/19-background-information.png)

Aby uzyskać więcej informacji zobacz [jak Azure subskrypcje są skojarzone z usługi Azure Active Directory](./active-directory/active-directory-how-subscriptions-associated-directory.md).

>[AZURE.NOTE] Subskrypcje Azure są własnością poszczególni użytkownicy w katalogu.

>[AZURE.NOTE] Subskrypcje usługi Office 365 są własnością samego katalogu. Jeśli użytkowników w katalogu ma wymagane uprawnienia, że mogą one działać na następujących subskrypcji.

## <a name="next-steps"></a>Następne kroki
Jeśli zarówno Azure, jak i usługi Office 365 subskrypcji został zakupiony oddzielnie w przeszłości i chcesz mieć dostępu do dzierżawy usługi Office 365 z Azure subskrypcji, zobacz [skojarzyć dzierżawy usługi Office 365 z subskrypcją usługi Azure](billing-add-office-365-tenant-to-azure-subscription.md).

> [AZURE.NOTE] Jeśli nadal masz pytania, [skontaktuj się z obsługą](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) uzyskanie szybko rozwiązać problem.
