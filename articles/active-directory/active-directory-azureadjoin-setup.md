<properties
    pageTitle="Konfigurowanie Azure AD dołączanie dla użytkowników | Microsoft Azure"
    description="W tym artykule wyjaśniono, jak Administratorzy mogą skonfigurować Azure AD dołączanie do katalogu lokalnego i rejestracji urządzenia."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""
    tags="azure-classic-portal"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/27/2016"
    ms.author="femila"/>

# <a name="setting-up-azure-ad-join-in-your-organization"></a>Konfigurowanie Azure AD dołączanie w organizacji

Przed skonfigurowaniem Azure Active Directory dołączyć (Azure AD dołączyć do), należy ręcznie utworzyć konta zarządzane w Azure AD albo synchronizacji katalogu lokalnego użytkowników w chmurze.

Szczegółowe instrukcje dotyczące synchronizowania użytkownikom lokalnego Azure AD omówiono [Integrowanie tożsamości lokalnych z usługą Azure Active Directory](active-directory-aadconnect.md).


Aby ręcznie utworzyć i zarządzanie użytkownikami w Azure AD, zobacz [Zarządzanie użytkownikami w Azure AD](https://msdn.microsoft.com/library/azure/hh967609.aspx).

## <a name="set-up-device-registration"></a>Konfigurowanie Rejestracja urządzenia
1. Zaloguj się do portalu Azure jako administrator.
2. W okienku po lewej stronie wybierz pozycję **Usługi Active Directory**.
3. Na karcie **katalog** zaznacz katalogu.
4. Wybierz kartę **Konfiguruj** .
5. Przejdź do sekcji **urządzenia** .
6. Na karcie **urządzenia** ustaw następujące opcje:  
   * **Maksymalna liczba z urządzeń na użytkownika**: wybierz maksymalną liczbę urządzeń, które użytkownik może mieć w Azure AD.  Jeśli użytkownik osiągnie tego przydziału, nie będą możesz dodać dodatkowe urządzenia, aż do jednej lub większej liczby urządzeń istniejących zostaną usunięte.
   * **Wymaganie uwierzytelniania WIELOSKŁADNIKOWEGO do urządzenia sprzężenia**: Określ, czy użytkownicy muszą podać drugiego czynniki uwierzytelniania do połączenia urządzenia Azure AD. Aby uzyskać więcej informacji na uwierzytelnianie wieloskładnikowe Azure zobacz [Wprowadzenie do uwierzytelnianie wieloskładnikowe Azure w chmurze](..\multi-factor-authentication\multi-factor-authentication-get-started-cloud.md).
   * **Użytkownicy mogą AZURE AD sprzężenia urządzenia**: Wybieranie użytkowników i grup przeznaczonych do połączenia urządzeń Azure AD.
   * **Dodatkowych ADMINISTRATORÓW na AZURE AD sprzężone urządzenia**: Azure AD Premium lub pakietu mobilności przedsiębiorstwa (EMS), możesz wybrać użytkowników, którzy mają prawa administratora lokalnego na urządzeniu. Globalne Administratorzy i właściciele urządzenia udzielono prawa administratora lokalnego domyślnie.

<center>![Konfigurowanie urządzenia regisration](./media/active-directory-azureadjoin/active-directory-aadjoin-configure-devices.png)</center>

Po skonfigurowaniu Azure AD dołączanie dla użytkowników, można łączą się Azure AD przy użyciu swoich urządzeń firmową lub osobistą.

Oto trzy scenariusze, używanych w celu umożliwienia użytkownikom konfigurowanie Azure AD dołączanie:

- Użytkownicy dołączanie należące do firmy urządzenie bezpośrednio do Azure AD.
- Dołączanie domeny użytkowników należące do firmy na urządzeniu w lokalnej usłudze Active Directory, a następnie rozszerzyć urządzenia do Azure AD.
- Użytkownicy dodać konta służbowego do systemu Windows na urządzeniu osobistych

## <a name="additional-information"></a>Dodatkowe informacje
* [Windows 10 w przedsiębiorstwie: sposoby za pomocą urządzenia do pracy](active-directory-azureadjoin-windows10-devices-overview.md)
* [Rozszerzanie możliwości chmury urządzeń systemu Windows 10 za pośrednictwem Azure Active Directory dołączanie](active-directory-azureadjoin-user-upgrade.md)
* [Informacje na temat scenariusze użycia dla Azure AD dołączanie](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Podłącz urządzenia domeny do Azure AD dla środowiska systemu Windows 10](active-directory-azureadjoin-devices-group-policy.md)
* [Konfigurowanie Azure AD dołączanie](active-directory-azureadjoin-setup.md)
