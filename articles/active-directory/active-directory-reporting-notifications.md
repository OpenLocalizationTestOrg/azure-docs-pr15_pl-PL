<properties
    pageTitle="Powiadomienia raportowania Azure Active Directory"
    description="Jak używać usługi Azure Active Directory raportowania powiadomienia o podejrzanych znak dodatki."
    services="active-directory"
    documentationCenter=""
    authors="dhanyahk"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="03/07/2016"
    ms.author="dhanyahk"/>

# <a name="azure-active-directory-reporting-notifications"></a>Powiadomienia raportowania Azure Active Directory

## <a name="what-reports-generate-email-notifications"></a>Jakie raporty generowanie powiadomień e-mail

W tej chwili powiadomienia e-mail o nieregularnym kształcie Zaloguj wyzwalaczy raport aktywności.

## <a name="what-is-an-irregular-sign-in"></a>Co to jest "o nieregularnym kształcie logowania"?

O nieregularnym kształcie dodatki logowania to te, które zostały określone przez naszych maszynowego uczenia algorytmy, na podstawie warunku "niemożliwe podróży" razem z lokalizacji anomalous logowania i urządzenia. Może to oznaczać, że haker próbował zalogować się przy użyciu tego konta.

## <a name="who-receives-the-email-notifications"></a>Kto otrzymuje powiadomienia e-mail?

Wiadomości e-mail są wysyłane do wszystkich administratorów globalnych, które zostały przypisane licencji usługi Active Directory Premium. Aby upewnić się, że został dostarczony, możemy Wyślij go do administratorów alternatywny adres E-mail, a także. Administratorzy powinien zawierać aad-alerts-noreply@mail.windowsazure.com na liście bezpiecznych nadawców, aby nie były przeoczyć wiadomości e-mail.

## <a name="how-often-are-these-emails-sent"></a>Jak często są te wiadomości e-mail wysyłane?

Wiadomości e-mail są wysyłane w ciągu ostatnich 30 dni występują 10 nowych o nieregularnym kształcie logowania działań, czy od czasu ostatniego wiadomość e-mail została wysłana, ta wartość jest mniejsza.

## <a name="how-do-i-access-the-report-mentioned-in-the-email"></a>Jak uzyskać dostęp do raportu wymienionych w wiadomości e-mail

Po kliknięciu łącza będą nastąpi przekierowanie do strony raportu w portalu klasyczny Azure. Aby uzyskać dostęp do raportu, musisz być:

- Administrator lub administratorami co subskrypcji usługi Azure

- Administrator globalny w katalogu i przypisanych licencji usługi Active Directory Premium. Aby uzyskać więcej informacji zobacz [wersji usługi Azure Active Directory](active-directory-editions.md).

## <a name="can-i-turn-off-these-emails"></a>Czy mogę wyłączyć te wiadomości e-mail?

Tak, aby wyłączyć powiadomienia związane z anomalous dodatki logowania w portalu klasyczny Azure, kliknij przycisk **Konfiguruj**, a następnie wybierz **wyłączone** w sekcji **powiadomienia** .

## <a name="whats-next"></a>Co to jest dalej
- Zastanawiasz się, jakie zabezpieczeń, inspekcji, a raporty aktywności są dostępne? Zapoznaj się z [Azure AD zabezpieczenia, inspekcji i raporty aktywności](active-directory-view-access-usage-reports.md)
- [Wprowadzenie do programu Azure Active Directory Premium](active-directory-get-started-premium.md)
- [Dodawanie logo do Zaloguj się i Panel dostępu stron firmy](active-directory-add-company-branding.md)
