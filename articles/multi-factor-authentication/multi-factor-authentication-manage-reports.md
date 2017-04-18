<properties
    pageTitle="Raporty Azure uwierzytelnianie wieloskładnikowe"
    description="To opisano, jak korzystać z funkcji uwierzytelnianie wieloskładnikowe Azure — raportów."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="reports-in-azure-multi-factor-authentication"></a>Raporty w Azure uwierzytelnianie wieloskładnikowe

Uwierzytelnianie wieloskładnikowe Azure udostępnia kilka raportów, które mogą być używane przez Ciebie i organizacji. Te raporty są dostępne za pośrednictwem portalu zarządzania uwierzytelnianie wieloskładnikowe, co wymaga, czy masz licencję dostawcy MFA Azure, lub Azure MFA, Azure AD Premium lub Enterprise Suite mobilności. Poniżej przedstawiono listę dostępnych raportów.

Masz dostęp do raportów za pośrednictwem portalu zarządzania Azure.

Nazwa| Opis
:------------- | :------------- |
Użycie | Obciążenie raporty zawierają informacje ogólne wykorzystanie, użytkownika Podsumowanie i szczegóły użytkownika.
Stan serwera|Ten raport przedstawia stan serwerów uwierzytelnianie wieloskładnikowe skojarzonego z kontem.
Lista zablokowanych historię|Te raporty Pokaż historię żądania blokowanie i odblokowywanie użytkowników.
Historia pomijanych użytkownika|Historia żądań, aby pominąć uwierzytelnianie wieloskładnikowe dla numeru telefonu użytkownika.
Alert oszustwa|Historia alertów oszustwa przesłane w zakresie dat, określonej.
W kolejce|Raporty list w kolejce przetwarzania i ich stanu. Łącze do pobrania lub wyświetlić raport znajduje się po zakończeniu raportu.

## <a name="to-view-reports"></a>Wyświetlanie raportów

1.  Zaloguj się do http://azure.microsoft.com
2.  Po lewej stronie wybierz pozycję usługi Active Directory.
3.  Wybierz jedną z następujących opcji:
    - **Opcja 1**: kliknij kartę dostawców uwierzytelniania wieloskładnikowego. Wybierz dostawcę MFA, a następnie kliknij przycisk Zarządzaj u dołu.
    - **Opcja 2**: wybierz pozycję katalogu, a następnie kliknij kartę Konfigurowanie. W sekcji uwierzytelnianie wieloskładnikowe wybierz pozycję Ustawienia usługi Zarządzanie. U dołu strony Ustawienia usługi MFA kliknij przycisk Przejdź do portalu łącza.
4.  W portalu zarządzania uwierzytelnianie wieloskładnikowe Azure widzą sekcji raportu w nawigacji po lewej stronie widoku. W tym miejscu możesz wybrać raporty opisany powyżej.

<center>![Chmury](./media/multi-factor-authentication-manage-reports/report.png)</center>


**Dodatkowe zasoby**

* [Dla użytkowników](./end-user/multi-factor-authentication-end-user.md)
* [Uwierzytelnianie wieloskładnikowe Azure w witrynie MSDN](https://msdn.microsoft.com/library/azure/dn249471.aspx)
