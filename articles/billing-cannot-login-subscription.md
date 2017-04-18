<properties
    pageTitle="Nie możesz się zalogować do subskrypcji usługi Azure | Microsoft Azure"
    description="Zawiera opis sposobu rozwiązywania typowych problemów dotyczących logowania Azure subskrypcji."
    services=""
    documentationCenter=""
    authors="genlin"
    manager="mbaldwin"
    editor=""
    tags="billing"
    />

<tags
    ms.service="billing"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="genli"/>

# <a name="i-cant-sign-in-to-manage-my-azure-subscription"></a>Nie mogę zalogować się do zarządzania Azure subskrypcji

Ten artykuł prowadzi użytkownika przez niektóre z najczęściej używanych metod rozwiązywania problemów logowania.

## <a name="page-hangs-in-the-loading-status"></a>Strona zawiesza się w stan ładowania

Jeśli zawiesza się po stronie przeglądarki internet, spróbuj każda z poniższych czynności, aż przejdziesz do [portalu Azure](https://portal.azure.com).

-   Odśwież stronę.
-   Użyj innej przeglądarki internetowej.
-   Jeśli korzystasz z programu Microsoft Internet Explorer, przejdź do portalu Azure, korzystając z trybu przeglądania InPrivate. 

    ODPOWIEDZI.  Kliknij pozycję **Narzędzia** ![przycisk narzędzia](./media/billing-cannot-login-subscription/Toolsbutton.png) > **bezpieczeństwa** > **Przeglądanie InPrivate**.

    B.  Przejdź do [portalu Azure](https://portal.azure.com), a następnie zaloguj się do portalu.

## <a name="error-message-no-subscriptions-found"></a>Komunikat o błędzie "Nie można odnaleźć subskrypcji"

Jeśli Twoje konto nie ma uprawnień wystarczających, może zostać wyświetlony komunikat o błędzie **nie odnaleziono subskrypcji** . Tylko administrator konta może uzyskać dostęp do [Centrum konta](https://account.windowsazure.com/)nie administratorów usługi (SA) ani administratorów współpracujących (CA).

**Scenariusz 1: Komunikat o błędzie odebranego [Azure portal](https://portal.azure.com)**

Aby rozwiązać ten problem, [Dodaj rolę Współtworzenie administratora lub właściciela](billing-add-change-azure-subscription-administrator.md) konta.

**Scenariusz 2: Otrzymano komunikat o błędzie w [Centrum konto Azure](https://account.windowsazure.com/Subscriptions)**

Sprawdzanie, czy konto, które zostało użyte jest administratorem konta. Aby sprawdzić, kto jest administratorem konta, wykonaj następujące czynności:

1.  Zaloguj się do [portalu Azure](https://portal.azure.com).
2.  W menu Centrum wybierz **subskrypcję**.
3.  Wybierz subskrypcję, którą chcesz sprawdzić, a następnie wybierz pozycję **Ustawienia**.
4.  Wybierz pozycję **Właściwości**. Administratorem konta subskrypcji jest wyświetlana w polu **Konto administratora** .

## <a name="you-are-automatically-signed-in-as-a-different-user"></a>Automatycznie zalogowano się jako inny użytkownik

Ten problem może wystąpić, jeśli używasz więcej niż jedno konto użytkownika w przeglądarce internetowej.

Aby rozwiązać ten problem, spróbuj wykonać jedną z następujących metod:

-   Czyszczenie pamięci podręcznej i Usuń pliki cookie. W programie Internet Explorer kliknij pozycję **Narzędzia** ![przycisk narzędzia](./media/billing-cannot-login-subscription/Toolsbutton.png) > **Opcje internetowe** > **usunąć**. Upewnij się, że zaznaczono pola wyboru dla tymczasowych plików, pliki cookie, hasło i historię przeglądania, a następnie kliknij pozycję Usuń.

-   Resetowanie ustawień programu Internet Explorer, aby przywrócić wszystkie ustawienia osobiste, które zostały wprowadzone. Kliknij pozycję **Narzędzia** ![przycisk narzędzia](./media/billing-cannot-login-subscription/Toolsbutton.png)> **Opcje internetowe** > **Zaawansowane** > zaznacz pole wyboru **Usuń ustawienia osobiste** > **Resetuj**.

-   Przejdź do portalu Azure w trybie przeglądania InPrivate. Kliknij pozycję **Narzędzia** ![przycisk narzędzia](./media/billing-cannot-login-subscription/Toolsbutton.png) > **bezpieczeństwa** > **Przeglądanie InPrivate**.

## <a name="need-help-contact-support"></a>Potrzebujesz pomocy? Kontakt z pomocą techniczną. 

Jeśli nadal potrzebujesz pomocy, [kontakt z pomocą techniczną](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) , aby uzyskać szybko rozwiązać problem. 