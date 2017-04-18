<properties
   pageTitle="Rozwiązywanie problemów: Element usługi Active Directory jest brakujące lub nie są dostępne | Microsoft Azure "
   description="Co należy zrobić, gdy element menu usługi Active Directory nie są wyświetlane w portalu zarządzania Azure."
   services="active-directory"
   documentationCenter="na"
   authors="bryanla"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/16/2016"
   ms.author="mbaldwin"/>

# <a name="troubleshooting-active-directory-item-is-missing-or-not-available"></a>Rozwiązywanie problemów: Element usługi Active Directory jest lub jest niedostępna

Wiele instrukcje dotyczące korzystania z usługi Azure Active Directory, funkcji i usług zaczyna się od "Przejdź do portalu zarządzania Azure i kliknij pozycję **usługi Active Directory"**. Ale co zrobić, jeśli element menu lub wewnętrzny usługi Active Directory nie będzie wyświetlany lub jest oznaczony **Nie są dostępne**? W tym temacie ma na celu. Opisuje warunki **Usługi Active Directory** nie pojawia się lub jest niedostępny i wyjaśniono, jak kontynuować.

## <a name="active-directory-is-missing"></a>Brakuje usługi Active Directory

Zazwyczaj elementu **Usługi Active Directory** pojawia się w menu nawigacji po lewej stronie. Instrukcje usługi Azure Active Directory procedury przyjęto założenie, że ten element znajdujący się w widoku.

![Zrzut ekranu: usługi Active Directory platformy Azure](./media/active-directory-troubleshooting/typical-view.png)

Element Active Directory pojawia się w menu nawigacji po lewej stronie, jeśli jest spełniony jeden z następujących warunków. W przeciwnym razie element nie jest wyświetlane.

* Bieżący użytkownik zalogowani za pomocą konta Microsoft (wcześniej nazywanego identyfikatorem Windows Live ID).

    LUB

* Azure dzierżawy ma katalog i bieżącego konta jest administratorem katalogu.

    LUB

* Azure dzierżawy ma co najmniej jeden nazw kontrola Azure AD dostępu (ACS). Aby uzyskać więcej informacji zobacz [Namespace kontroli dostępu](https://msdn.microsoft.com/library/azure/gg185908.aspx).

    LUB

* Azure dzierżawy zawiera co najmniej jeden dostawca uwierzytelnianie wieloskładnikowe Azure. Aby uzyskać więcej informacji zobacz [Administrowanie dostawców uwierzytelniania wieloskładnikowego Azure](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md).

Aby utworzyć nazw kontrola dostępu lub dostawcę uwierzytelnianie wieloskładnikowe, kliknij pozycję **+ Nowe** > **Aplikacji usług** > **Usługi Active Directory**.

Aby uzyskać uprawnienia administracyjne do katalogu, poproś administratora, o przypisywać rolę administratora do swojego konta. Aby uzyskać szczegółowe informacje zobacz [Przypisywanie ról administratora](active-directory-assign-admin-roles.md).

## <a name="active-directory-is-not-available"></a>Usługa Active Directory nie jest dostępna

Po kliknięciu przycisku **+ Nowe** > **Aplikacji usługi** **Active Directory** element jest wyświetlany. W szczególności elementu usługi Active Directory jest wyświetlana po funkcji usługi Active Directory, na przykład katalogu, kontrola dostępu lub dostawcy uwierzytelnianie wieloskładnikowe, są dostępne dla bieżącego użytkownika.

Jednak podczas ładowania strony element jest wyszarzona i jest oznaczony jako **Nie jest dostępna**. Jest tymczasowy stan. Jeśli Poczekaj kilka sekund, element stał się dostępny. Jeśli opóźnienie zostanie przedłużony, odświeżanie strony sieci web często rozwiązuje ten problem.

![Zrzut ekranu: Usługa Active Directory nie jest dostępna](./media/active-directory-troubleshooting/not-available.png)
