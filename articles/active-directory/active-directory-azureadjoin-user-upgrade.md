<properties
    pageTitle="Konfigurowanie urządzenia systemu Windows 10 z usługą Azure Active Directory w obszarze Ustawienia | Microsoft Azure"
    description="W tym artykule wyjaśniono, jak użytkownicy mogą dołączyć do Azure AD przy użyciu menu Ustawienia."
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

# <a name="set-up-a-windows-10-device-with-azure-ad-from-settings"></a>Konfigurowanie urządzenia systemu Windows 10 z usługą Azure Active Directory z poziomu strony ustawień
Jeśli używasz systemu Windows 7 lub Windows 8, a komputerem lub urządzeniem została uaktualniona do systemu Windows 10, możesz dołączać do usługi Azure Active Directory (Azure AD) za pomocą menu Ustawienia.

## <a name="to-join-to-azure-ad-from-the-settings-menu"></a>Aby dołączyć do Azure AD w menu Ustawienia


1. W **Start** menu kliknij Panel **Ustawienia** .
2. W obszarze **Ustawienia**wybierz pozycję **System**->**o**->**Dołączanie Azure AD**.
<center>
![Dołączanie do Azure AD w menu ustawienia](./media/active-directory-azureadjoin/active-directory-azureadjoin-settings.png)</center>

3. Kliknij przycisk **Kontynuuj** w oknie wiadomości Azure AD dołączanie.
<center>
![W oknie wiadomości Dołącz Azure AD](./media/active-directory-azureadjoin/active-directory-azureadjoin-message.png)</center>
4. Podaj poświadczenia logowania. Ten logowanie zostaną uwzględnione wszystkie czynności, które są wymagane do wykonania uwierzytelniania. Jeśli część federacyjnych dzierżawy, administrator umożliwi klasy Federacji, która jest obsługiwana przez organizację.
<center>
![Poświadczenia logowania](./media/active-directory-azureadjoin/active-directory-azureadjoin-sign-in.png)</center>
5. Jeśli Twoja organizacja ma skonfigurowane uwierzytelnianie wieloskładnikowe Azure dołączania do Azure AD, należy podać współczynnik drugiego przed kontynuowaniem.
6. Na ekranie **Zezwalaj tego urządzenia, aby być zarządzane** , kliknij przycisk **Zaakceptuj** .
7. Powinien zostać wyświetlony komunikat "urządzenie teraz jest dołączony do organizacji w Azure AD".


## <a name="additional-information"></a>Dodatkowe informacje
* [Informacje na temat scenariusze użycia dla Azure AD dołączanie](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Podłącz urządzenia domeny do Azure AD dla środowiska systemu Windows 10](active-directory-azureadjoin-devices-group-policy.md)
* [Konfigurowanie Azure AD dołączanie](active-directory-azureadjoin-setup.md)
* [Uwierzytelnianie tożsamości bez hasła za pomocą Microsoft Passport](active-directory-azureadjoin-passport.md)
