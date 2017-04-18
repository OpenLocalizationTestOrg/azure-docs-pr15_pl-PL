

<properties
    pageTitle="Dołączanie do osobistej urządzenia do organizacji | Microsoft Azure"
    description="W tym miejscu wyjaśniono, jak użytkownicy będą mogli zarejestrować swoich osobistych urządzeń systemu Windows 10 z siecią firmową i przedstawiono czynności wdrażania scenariusza BYOD."
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
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>

# <a name="join-a-personal-device-to-your-organization"></a>Dołączanie do osobistej urządzenia do organizacji

## <a name="to-join-a-windows-10-device-to-your-organization"></a>Aby dołączyć do urządzenia systemu Windows 10 dla Twojej organizacji

1.  W **Start** menu wybierz pozycję **Ustawienia**.
2.  Wybierz pozycję **konta**, a następnie kliknij **swoje konto**.
3.  Kliknij pozycję **Dodaj pracy lub konta służbowego**, a następnie wpisz w konta swojej organizacji.
4.  Na stronie logowania dla Twojej organizacji wprowadź nazwę użytkownika i hasło, a następnie kliknij **przycisk OK**.
5.  Zostanie wyświetlony monit dla uwierzytelnianie wieloskładnikowe wezwanie. (Problem jest konfigurowany przez administratora IT).
6.  Azure Active Directory (Azure AD) sprawdza, czy urządzenie wymaga rejestrowania zarządzania urządzenia przenośnego.
7.  System Windows rejestruje urządzenia w książce telefonicznej organizacji w Azure AD i rejestruje w zarządzanie urządzeniami przenośnymi, w razie potrzeby.
8.  Jeśli jesteś użytkownikiem zarządzanych Windows umożliwia przejście do pulpitu za pośrednictwem automatycznego logowania.
9.  Jeśli jesteś użytkownikiem federacyjnych, nastąpi przekierowanie do ekranu logowania systemu Windows o podanie poświadczeń.

## <a name="additional-information"></a>Dodatkowe informacje
* [Windows 10 w przedsiębiorstwie: sposoby za pomocą urządzenia do pracy](active-directory-azureadjoin-windows10-devices-overview.md)
* [Rozszerzanie możliwości chmury urządzeń systemu Windows 10 za pośrednictwem Azure Active Directory dołączanie](active-directory-azureadjoin-user-upgrade.md)
* [Uwierzytelnianie tożsamości bez hasła za pomocą Microsoft Passport](active-directory-azureadjoin-passport.md)
* [Informacje na temat scenariusze użycia dla Azure AD dołączanie](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Podłącz urządzenia domeny do Azure AD dla środowiska systemu Windows 10](active-directory-azureadjoin-devices-group-policy.md)
* [Konfigurowanie Azure AD dołączanie](active-directory-azureadjoin-setup.md)
