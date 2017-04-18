<properties
    pageTitle="Konfigurowanie nowego urządzenia z usługą Azure Active Directory podczas instalacji | Microsoft Azure"
    description="Temat, którym wyjaśniono, jak konfigurowania użytkowników Azure AD dołączanie podczas jego pierwszego uruchomienia uruchamianie."
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

# <a name="set-up-a-new-device-with-azure-ad-during-setup"></a>Konfigurowanie nowego urządzenia z usługą Azure Active Directory podczas instalacji

W systemie Windows 10 użytkownicy mogą dołączyć do swoich urządzeń usługi Azure Active Directory (Azure AD) w pierwszego uruchomienia (FRX). Dzięki temu organizacji do dystrybucji porządnie zapakowane urządzeń pracownikom lub uczniów lub umożliwiać wybierz własne urządzenia (CYOD).
Jeśli wersji systemu Windows 10 Professional lub Enterprise 10 systemu Windows jest zainstalowana na urządzeniu, środowisko domyślnie procesu konfigurowania urządzeń należące do firmy.

## <a name="to-join-a-device-to-azure-ad"></a>Aby dołączyć do urządzenia do Azure AD


1. Po włączeniu nowym urządzeniu i rozpocząć proces instalacji, powinien zostać wyświetlony **Wprowadzenie gotowe** wiadomości. Postępuj zgodnie z instrukcjami, aby skonfigurować urządzenie.
2. Zacznij od Dostosowywanie region i język. Następnie zaakceptuj postanowienia licencyjne dotyczące oprogramowania firmy Microsoft.
![Dostosowywanie dla danego regionu](./media/active-directory-azureadjoin/active-directory-azureadjoin-customize-region.png)
3. Wybierz sieć, który ma być używany do łączenia się z Internetem.
4. Określ, czy korzystasz z urządzenia osobiste lub należące do firmy. Jeśli jest się właścicielem firmy, kliknij pozycję **to urządzenie należy do swojej organizacji**. Zostanie uruchomiony Azure AD dołączanie do obsługi. Oto ekranu, że zobaczysz, jeśli używasz systemu Windows 10 Professional.
<center>
![Do kogo należą ten ekran komputera](./media/active-directory-azureadjoin/active-directory-azureadjoin-who-owns-pc.png)

5.  Wprowadź poświadczenia, które zostały udostępnionych przez organizację.
<center>
![Ekran logowania](./media/active-directory-azureadjoin/active-directory-azureadjoin-sign-in.png)
6.  Po wprowadzeniu nazwy użytkownika pasujące dzierżawy znajduje się w Azure AD. Jeśli korzystasz z domeny sfederowanej, będzie nastąpi przekierowanie do lokalnego serwera bezpiecznego tokenu usługi (STS) — na przykład Active Directory Federation Services (AD FS).
7. Jeśli jesteś użytkownikiem w domenie niefederacyjnej, wprowadź poświadczenia, bezpośrednio na stronie AD hosted Azure. Jeśli został skonfigurowany logo firmy, będzie również zobaczyć logo Twojej organizacji i obsługuje tekstu.
8.  Zostanie wyświetlony monit o uwierzytelnianie wieloskładnikowe wezwanie. Ten test jest konfigurowany przez administratora IT.
9.  Azure AD sprawdza, czy to użytkownika i urządzenie wymaga rejestrowania w zarządzanie urządzeniami przenośnymi.
10. System Windows rejestruje urządzenia w książce telefonicznej organizacji w Azure AD i rejestruje w zarządzanie urządzeniami przenośnymi, w razie potrzeby.
11. Jeśli jesteś użytkownikiem zarządzanych Windows umożliwia przejście do pulpitu przez automatyczne proces logowania.
12. Jeśli jesteś użytkownikiem federacyjnych, są kierowane do ekran logowania systemu Windows, aby wprowadzić poświadczenia.

> [AZURE.NOTE] Dołączanie lokalną domeną usługi Active Directory systemu Windows Server doświadczenia w nowym polu systemu Windows nie jest obsługiwane. W związku z tym jeśli zamierzasz komputer jest dołączany do domeny, należy zaznaczyć łącze **Konfigurowanie systemu Windows z kontem lokalnym** zamiast tego. Można następnie dołączyć do domeny z poziomu ustawień na komputerze po wykonaniu przed.

## <a name="additional-information"></a>Dodatkowe informacje
* [Windows 10 w przedsiębiorstwie: sposoby za pomocą urządzenia do pracy](active-directory-azureadjoin-windows10-devices-overview.md)
* [Rozszerzanie możliwości chmury urządzeń systemu Windows 10 za pośrednictwem Azure Active Directory dołączanie](active-directory-azureadjoin-user-upgrade.md)
* [Uwierzytelnianie tożsamości bez hasła za pomocą Microsoft Passport](active-directory-azureadjoin-passport.md)
* [Informacje na temat scenariusze użycia dla Azure AD dołączanie](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Podłącz urządzenia domeny do Azure AD dla środowiska systemu Windows 10](active-directory-azureadjoin-devices-group-policy.md)
* [Konfigurowanie Azure AD dołączanie](active-directory-azureadjoin-setup.md)
