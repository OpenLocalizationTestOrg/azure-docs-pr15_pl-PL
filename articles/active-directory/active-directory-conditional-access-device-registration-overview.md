<properties
    pageTitle="Omówienie usługi Azure Active Directory Rejestracja urządzenia | Microsoft Azure"
    description="jest podstawą scenariusze oparte na urządzeniu dostępie warunkowym. Po zarejestrowaniu urządzenia Azure Active Directory Rejestracja urządzenia przepisy urządzenia z tożsamości, który jest używany do uwierzytelniania na urządzeniu, gdy użytkownik znaki w."
    services="active-directory"
    keywords="Rejestracja urządzenia, Włącz rejestracja, Rejestracja urządzenia i urządzenia MDM"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/27/2016"
    ms.author="Markvi"/>

# <a name="get-started-with-azure-active-directory-device-registration"></a>Rozpoczynanie pracy z Azure Active Directory Rejestracja urządzenia

Azure Rejestracja urządzenia Active Directory jest podstawą scenariusze oparte na urządzeniu dostępie warunkowym. Po zarejestrowaniu urządzenia Azure Active Directory Rejestracja urządzenia zawiera urządzenia z tożsamości, który jest używany do uwierzytelniania na urządzeniu, gdy użytkownik znaki w. Następnie można uwierzytelnionych urządzenie i atrybuty urządzenia, wymuszanie stosowania zasad dostępu warunkowego dla aplikacji, które są przechowywane w chmurze i lokalnego.

W połączeniu z rozwiązanie management(MDM) urządzenia przenośnego, takich jak Microsoft Intune, atrybuty urządzeń w usłudze Azure Active Directory są aktualizowane dodatkowe informacje o urządzeniu. Umożliwia tworzenie reguł dostępu warunkowego, wymuszające dostęp za pomocą urządzeń standardów dla zabezpieczenia i zgodność. Aby uzyskać więcej informacji na rejestrowanie urządzeń w Intune firmy Microsoft zobacz [urządzenia rejestrowanie zarządzania w Intune](https://docs.microsoft.com/intune/deploy-use/enroll-devices-in-microsoft-intune).

## <a name="scenarios-enabled-by-azure-active-directory-device-registration"></a>Scenariusze włączone przez Azure Active Directory Rejestracja urządzenia

Azure Rejestracja urządzenia Active Directory obsługuje iOS, Android i Windows urządzenia. Poszczególne scenariusze, które są używane rejestracja Azure AD urządzenia może być bardziej szczegółowe wymagania i platformy pomocy technicznej. Dostępne są następujące sytuacje:

- **Warunkowe dostęp do aplikacji, które są obsługiwane w lokalnej**: można używać urządzeń zarejestrowanych z zasady dostępu dla aplikacji, które są skonfigurowane do korzystania z systemu Windows Server 2012 R2 usług AD FS. Aby uzyskać więcej informacji na temat konfigurowania dostępu warunkowego dla lokalnego zobacz [Konfigurowanie warunkowego dostępu do lokalnego przy użyciu Azure Active Directory Rejestracja urządzenia](active-directory-conditional-access-on-premises-setup.md).

- **Warunkowe dostępu dla aplikacji usługi Office 365 za pomocą usługi Microsoft** : Administratorzy umożliwia obsługę zasady urządzenia dostępu warunkowego zabezpieczania zasobów firmy przy jednoczesnym zezwalania na zgodny z urządzenia, aby uzyskać dostęp do usług pracowników przetwarzających informacje. Aby uzyskać więcej informacji zobacz [Zasady warunkowe urządzenia dostępu dla usług Office 365](active-directory-conditional-access-device-policies.md).

##<a name="setting-up-azure-active-directory-device-registration"></a>Aby skonfigurować Azure Active Directory Rejestracja urządzenia

Musisz włączyć rejestracja Azure AD urządzenia w Azure Portal, dzięki czemu urządzeniach przenośnych mogą one znaleźć usługę, szukając znanego rekordy DNS. Musisz skonfigurować firmy DNS, tak aby urządzeń z systemem Windows 10, Windows 8.1, Windows 7, Android i iOS można wykryć i korzystać z usługi.
Możesz wyświetlać i włączanie i wyłączanie zarejestrowanych urządzeń za pomocą portalu administratora w usłudze Azure Active Directory.

>[AZURE.NOTE]
 Aby uzyskać najnowsze instrukcje dotyczące konfigurowania rejestracji urządzenia automatycznego Zobacz, [jak skonfigurować automatyczne rejestracji domeny systemu Windows sprzężone urządzeń z usługą Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md).

### <a name="enable-azure-active-directory-device-registration-service"></a>Włączanie usługi rejestracji urządzenia Azure Active Directory

1. Zaloguj się do portalu Microsoft Azure jako Administrator.
2. W okienku po lewej stronie wybierz pozycję **Usługi Active Directory**.
3. Na karcie **katalog** zaznacz katalogu.
4. Wybierz kartę **Konfiguruj** .
5. Przewiń do sekcji o nazwie **urządzenia**.
6. Zaznacz **wszystkich** **użytkowników może być urządzeń sprzężenia miejscu pracy**.
7. Wybierz pozycję maksymalna liczba urządzeń, które chcesz zezwolić na użytkownika.

>[AZURE.NOTE]
>Rejestrowanie za pomocą programu Microsoft Intune lub zarządzanie urządzeniami przenośnymi dla usługi Office 365 wymaga, dołączanie do obszaru roboczego. Jeśli skonfigurowano jednej z tych usług została wybrana opcja wszystkie, a następnie przycisk Brak jest wyłączony.

Domyślnie uwierzytelnianie dwuskładnikowe nie włączono usługę. Jednak uwierzytelnianie dwuskładnikowe zaleca się podczas rejestrowania urządzenia.

- Wymagają uwierzytelnianie dwuskładnikowe dla tej usługi, należy skonfigurować dostawcę uwierzytelnianie dwuskładnikowe w usłudze Azure Active Directory i konfigurowanie uwierzytelniania wieloskładnikowego kont użytkowników, zobacz [Dodawanie uwierzytelnianie wieloskładnikowe usługi Azure Active Directory](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md)

- Jeśli korzystasz z usług AD FS z systemem Windows Server 2012 R2, należy skonfigurować moduł uwierzytelnianie dwuskładnikowe w usług AD FS, zobacz [Przy użyciu uwierzytelnianie wieloskładnikowe z usług federacyjnych Active Directory](../multi-factor-authentication/multi-factor-authentication-get-started-server.md).

## <a name="configure-azure-active-directory-device-registration-discovery"></a>Konfigurowanie odnajdowania Azure Active Directory Rejestracja urządzenia
Urządzenia systemu Windows 7 i Windows 8.1 wykryje usługi rejestracji urządzenia, łącząc nazwę konta użytkownika, dobrze znane nazwy serwera rejestracji urządzenia.

Musisz utworzyć rekord DNS CNAME wskazującego na rekord skojarzonego z usługą Azure Active Directory Rejestracja urządzenia. Rekord CNAME, należy użyć enterpriseregistration znanego prefiks następuje sufiksu głównej nazwy użytkownika, używane przez kont użytkowników w organizacji. Jeśli organizacja korzysta z wielu sufiksy, wielu rekordów CNAME muszą zostać utworzone w systemie DNS.

Na przykład, jeśli korzystasz z dwóch sufiksy w firmie o nazwie @contoso.com i @region.contoso.com, utworzysz poniższych rekordów DNS.

| Wpis                                     | Typ  | Adres                            |
|-------------------------------------------|-------|------------------------------------|
| enterpriseregistration.contoso.com        | CNAME | enterpriseregistration.Windows.NET |
| enterpriseregistration.region.contoso.com | CNAME | enterpriseregistration.Windows.NET |

## <a name="view-and-manage-device-objects-in-azure-active-directory"></a>Wyświetlanie i zarządzanie obiektami urządzenia w usłudze Active Directory platformy Azure
1. Z portalu administratora Azure można wyświetlanie, blokowanie i odblokowywanie urządzenia. Urządzenie jest zablokowane nie jest już uzyskuje dostęp do aplikacji, które są skonfigurowane tak, aby tylko zarejestrowanych urządzenia.
2. Zaloguj się do portalu Microsoft Azure jako Administrator.
3. W okienku po lewej stronie wybierz pozycję **Usługi Active Directory**.
4. Wybierz pozycję katalogu.
5. Wybierz kartę **użytkowników** . Następnie wybierz użytkownika w celu wyświetlania ich urządzeń
6. Wybierz kartę **urządzenia** .
7. Z menu rozwijanego wybierz pozycję **Zarejestrowane urządzenia** .
8. W tym miejscu można wyświetlać, blokowanie i odblokowywanie urządzeń zarejestrowanych użytkowników.

## <a name="additional-topics"></a>Dodatkowe tematy

Można zarejestrować urządzenia systemu Windows 7 i Windows 8.1 domeny połączone z rejestracja Azure AD urządzenia. Następujące tematy znajdziesz więcej informacji o wymaganiach wstępnych i kroki wymagane do skonfigurowania Rejestracja urządzenia na urządzeniach z systemem Windows 7 i Windows 8.1.

- [Rejestracja urządzenia automatyczne z usługą Azure Active Directory dla domeny urządzenia z systemem Windows](active-directory-conditional-access-automatic-device-registration.md)
- [Konfigurowanie rejestracji urządzenia automatycznego dla urządzeń domeny sprzężone systemu Windows 7](active-directory-conditional-access-automatic-device-registration-windows7.md)
- [Konfigurowanie rejestracji urządzenia automatycznego dla urządzeń domeny sprzężone Windows 8.1](active-directory-conditional-access-automatic-device-registration-windows-8-1.md)
- [Rejestracja urządzenia automatyczne z urządzeniami domeny Azure Active Directory dla systemu Windows 10](active-directory-azureadjoin-devices-group-policy.md)
