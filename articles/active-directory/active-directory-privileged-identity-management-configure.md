<properties
    pageTitle="Zarządzanie tożsamościami Azure AD uprawnieniach | Microsoft Azure"
    description="Temat, w którym wyjaśniono, co to jest Azure AD uprzywilejowanych Zarządzanie tożsamościami i jak używać PIM, aby zwiększyć bezpieczeństwo chmury."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="kgremban"/>

# <a name="azure-ad-privileged-identity-management"></a>Zarządzanie tożsamościami uprawnieniach Azure AD

Zarządzanie tożsamościami uprzywilejowanych Azure Active Directory (AD) możesz zarządzać, control i monitorować dostęp w organizacji. Dotyczy to również dostęp do zasobów w Azure AD i inne usługi online firmy Microsoft, takich jak usługi Office 365 lub Intune firmy Microsoft.  

> [AZURE.NOTE] Zarządzanie tożsamościami uprzywilejowanych jest dostępne tylko w przypadku wersji Premium P2 usługi Azure Active Directory. Aby uzyskać więcej informacji zobacz [wersji usługi Azure Active Directory](active-directory-editions.md).

Organizacje chcesz ograniczyć liczbę osób, które mają dostęp do zabezpieczania informacji lub zasobów, ponieważ, która ogranicza możliwość złośliwy użytkownik uzyskiwanie dostępu. Jednak użytkownicy nadal będą do wykonywania operacji uprzywilejowanych w aplikacjach Azure, Office 365 lub władz akredytacji bezpieczeństwa. Organizacje przekazać użytkownikom uprzywilejowanych w Azure AD bez monitorowania, co robią tych użytkowników z ich uprawnieniami administratora. Zarządzanie tożsamościami usługi Azure AD uprawnieniach pozwala rozwiązać ten czynnik ryzyka.  

Zarządzanie tożsamościami usługi Azure AD uprawnieniach pomoże Ci:  

- Zobacz użytkowników, którzy są Administratorzy Azure AD
- Włączenie na żądanie "po prostu time" dostęp administracyjny do usługi Online firmy Microsoft, takich jak usługi Office 365 i usługi
- Pobieranie raportów dotyczących historię dostęp administratora i zmiany w przydziałów administratora
- Otrzymywać alerty o dostęp do roli uprzywilejowanych

Azure AD uprzywilejowanych Zarządzanie tożsamościami można zarządzać wbudowane Azure AD ról organizacji, w tym:  

- Administrator globalny
- Administrator rozliczeń
- Administrator usługi  
- Administrator użytkowników
- Administrator haseł

## <a name="just-in-time-administrator-access"></a>Tylko w dostęp administratora

Ze względów historycznych może przypisać użytkownikowi rolę administratora przez portal Azure klasyczny lub środowiska Windows PowerShell. W wyniku tego użytkownika staje się **Administrator trwały**, zawsze aktywne w przypisaną rolę. Zarządzanie tożsamościami usługi Azure AD uprawnieniach pojęcia **odpowiedniej administratora**. Administratorzy odpowiedniej powinny być użytkowników, którzy potrzebują uprzywilejowanych teraz, a następnie, ale nie każdego dnia. Rola jest nieaktywny, aż użytkownik musi mieć dostęp, następnie ukończyć proces aktywacji i Zostań administratorem aktywne dla ustalonej czasu.

## <a name="enable-privileged-identity-management-for-your-directory"></a>Włączanie uprzywilejowanych Zarządzanie tożsamościami dla katalogu

Możesz rozpocząć korzystanie z Azure AD uprzywilejowanych Zarządzanie tożsamościami w [Azure portal](https://portal.azure.com/).

>[AZURE.NOTE] Musisz być administratorem globalnym za pomocą konta organizacji (na przykład @yourdomain.com), nie konta Microsoft (na przykład @outlook.com), umożliwiający Azure AD uprzywilejowanych Zarządzanie tożsamościami dla katalogu.

1. Zaloguj się do [portalu Azure](https://portal.azure.com/) jako administrator globalny katalogu.
2. Jeśli Twoja organizacja ma więcej niż jednego katalogu, zaznacz nazwę użytkownika w prawym górnym rogu Azure portal. Wybierz miejsce, w którym będzie korzystać Azure AD uprzywilejowanych Zarządzanie tożsamościami katalog.
3. Wybierz pozycję **więcej usług** i wyszukiwanie **Azure AD uprzywilejowanych Zarządzanie tożsamościami**przy użyciu filtru pole tekstowe.
4. Sprawdzanie **numeru Pin do pulpitu nawigacyjnego** , a następnie kliknij przycisk **Utwórz**. Zostanie wyświetlona aplikacja uprzywilejowanych Zarządzanie tożsamościami.

Jeśli jesteś pierwszej osoby, aby użyć Azure AD uprzywilejowanych Zarządzanie tożsamościami w katalogu, następnie [Kreator zabezpieczeń](active-directory-privileged-identity-management-security-wizard.md) przeprowadzi Cię przez środowisko początkowy przydział. Po wykonaniu tej automatycznie staje się pierwszy **administratora zabezpieczeń** i **uprzywilejowanych roli administratora** katalogu.

Tylko administrator uprzywilejowanych roli zarządzać dostępem dla innych administratorów. Możesz [przekazać innym użytkownikom możliwość zarządzania w PIM](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).

## <a name="privileged-identity-management-dashboard"></a>Uprzywilejowanych Zarządzanie tożsamościami pulpitu nawigacyjnego

Azure AD uprawnieniach Identity Manager udostępnia pulpit nawigacyjny, który zawiera ważne informacje, takie jak:

- Alerty, które wskazać szanse sprzedaży w celu zwiększenia zabezpieczeń
- Liczba użytkowników, którzy są przypisane do poszczególnych uprzywilejowanych ról  
- Liczba odpowiedniej i trwały administratorów
- Przeglądy zapewnienia stałego dostępu

![Pulpit nawigacyjny PIM — zrzut ekranu][2]

## <a name="privileged-role-management"></a>Zarządzanie rolami uprzywilejowanych

Zarządzanie tożsamościami Azure AD uprzywilejowanych możesz zarządzać Administratorzy przez dodanie lub usunięcie Administratorzy stałych lub kwalifikuje się do poszczególnych ról.

![PIM Dodawanie i usuwanie administratorów — zrzut ekranu][3]

## <a name="configure-the-role-activation-settings"></a>Konfigurowanie ustawień aktywacji roli

Przy użyciu [Ustawienia roli](active-directory-privileged-identity-management-how-to-change-default-settings.md) można skonfigurować właściwości aktywacji odpowiedniej roli, w tym:

- Okres aktywacji roli
- Powiadomienie o aktywacji roli
- Informacje, które należy podać podczas procesu aktywacji roli użytkownika  

![Zrzut ekranu ustawienia — administrator Aktywacja — PIM][4]

Należy zauważyć, że na obrazie, przyciski **Uwierzytelnianie wieloskładnikowe** są wyłączone. Dla określonych, wysoce uprzywilejowane role, wymagana MFA podwyższonym ochrony.

## <a name="role-activation"></a>Aktywacja roli  

Aby [uaktywnić rolę](active-directory-privileged-identity-management-how-to-activate-role.md)administratorem odpowiedniej żądania czasu powiązanych "aktywacji" dla roli. Aktywacji można żądać przy użyciu opcji **Aktywuj mój roli** Azure AD uprzywilejowanych Zarządzanie tożsamościami.

Kto chce aktywować roli Administrator musi zainicjować Azure AD uprzywilejowanych Zarządzanie tożsamościami w portalu Azure.

Aktywacja roli jest można dostosować. W obszarze Ustawienia PIM można określić długość aktywacji i informacje, administrator musi zapewnienie, aby aktywować dla roli.

![Aktywacja roli PIM administrator żądanie — zrzut ekranu][5]

## <a name="review-role-activity"></a>Przegląd ról aktywności

Istnieją dwa sposoby do śledzenia, jak pracowników i Administratorzy używają uprzywilejowanych role. Pierwszą opcję korzysta z [inspekcji historii](active-directory-privileged-identity-management-how-to-use-audit-log.md). Historia inspekcji dzienniki śledzenia zmian w przypisania roli uprzywilejowanych i Historia aktywacji roli.

![Historia aktywacji PIM — zrzut ekranu][6]

Jest to druga opcja jest konfigurowanie zwykła, [program access sprawdza](active-directory-privileged-identity-management-how-to-start-security-review.md). Te przeglądy programu access mogą być wykonywane przez, a przydzielone recenzenta (na przykład Menedżer zespołu) lub przeglądać się pracowników. Jest to najlepszy sposób monitorowania, która nadal wymaga dostępu, a która już nie działa.


## <a name="next-steps"></a>Następne kroki
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-configure/PIM_EnablePim.png
[2]: ./media/active-directory-privileged-identity-management-configure/PIM_Dash.png
[3]: ./media/active-directory-privileged-identity-management-configure/PIM_AddRemove.png
[4]: ./media/active-directory-privileged-identity-management-configure/PIM_RoleActivationSettings.png
[5]: ./media/active-directory-privileged-identity-management-configure/PIM_RequestActivation.png
[6]: ./media/active-directory-privileged-identity-management-configure/PIM_ActivationHistory.png
