<properties
    pageTitle="Azure AD Connect i Rosyjska | Microsoft Azure"
    description="Ta strona jest to centralne miejsce dla wszystkich dokumentację dotyczącą usług AD FS operacji przy użyciu Azure AD Connect"
    services="active-directory"
    documentationCenter=""
    authors="anandyadavmsft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="anandy"/>


# <a name="azure-ad-connect-and-federation"></a>Narzędzie Azure AD Connect i Federacji

Narzędzie Azure AD Connect umożliwia konfigurowanie federacji z lokalnego AD FS i Azure AD. Znakiem Federacji na umożliwia użytkownikom logowanie do usług Azure AD podstawie haseł lokalnego i znajduje się w sieci firmowej, bez konieczności wprowadzania hasła ponownie. Opcja federacji z usług AD FS umożliwia wdrażanie nowego lub określić istniejących usług AD FS w systemie Windows Server 2012 R2 farma.

W tym temacie jest Narzędzia główne, aby uzyskać informacje na Federacji powiązanych funkcje Azure AD Connect i listy łączy do innych tematów związanych z nim. Aby uzyskać łącza do Azure AD Connect Zobacz integrowanie tożsamości lokalnych z usługi Azure Active Directory.

## <a name="azure-ad-connect---federation-topics"></a>Narzędzie Azure AD Connect — tematy Federacji

| Temat | Co obejmuje, a kiedy do odczytu |
|:------|:-----------|
| **Azure AD Connect logowania opcji użytkownika** ||
| [Opis opcji logowania użytkownika](active-directory-aadconnect-user-signin.md) | Opis różnych opcji logowania użytkowników i ich wpływu na środowisko Azure logowania użytkownika |
| **AD FS instalacji przy użyciu Azure AD Connect**||
| [Wymagania wstępne](active-directory-aadconnect-get-started-custom.md#ad-fs-configuration-pre-requisites) | Wymagania wstępne, instalacja zakończyła się pomyślnie usług AD FS przez narzędzie Azure AD Connect|
| [Konfigurowanie usług AD FS farmy](active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs) | Jak zainstalować nowej farmy serwerów usług AD FS za pomocą narzędzie Azure AD Connect |
| **Modyfikowanie konfiguracji usług AD FS** | |
| [Naprawianie Ufaj](active-directory-aadconnect-federation-management.md#reparing-the-trust) | Jak naprawić bieżącej zaufania między lokalnego usług AD FS i usługi Office 365 / Azure |
| [Dodawanie nowego serwera usług AD FS](active-directory-aadconnect-federation-management.md#adding-a-new-ad-fs-server) | Rozwijanie farmy usług AD FS z dodatkowych usług AD FS wpis początkowej instalacji serwera |
| [Dodawanie nowego serwera AD FS WAP](active-directory-aadconnect-federation-management.md#adding-a-new-wap-server) | Rozwijanie farmy usług AD FS z dodatkowych WAP wpis początkowej instalacji serwera |
| [Dodawanie nowej domeny federacyjnych](active-directory-aadconnect-federation-management.md#add-a-new-federated-domain) | Dodawanie innej domeny Federacja jest z usługą Azure Active Directory |
|**Zadania po instalacji**||
| [Dodawanie logo firmy niestandardowy i ilustracji](active-directory-aadconnect-federation-management.md#add-custom-company-logo-or-illustration)| Modyfikowanie obsługi logowania, określając niestandardowe logo, który ma być wyświetlany na stronie logowania usług AD FS |
| [Dodawanie opisu do logowania](active-directory-aadconnect-federation-management.md#add-sign-in-description) | Zmienianie opisu logowania na stronie logowania usług AD FS | 
| [Modyfikowanie usług AD FS rościć sobie reguł](active-directory-aadconnect-federation-management.md#modifying-ad-fs-claim-rules) | Modyfikowanie / Dodawanie roszczeń reguł w programie AD FS odpowiadające konfiguracji synchronizacji Azure AD Connect |


## <a name="additional-resources"></a>Dodatkowe zasoby

* [Integrowanie tożsamości lokalnych z usługi Azure Active Directory](active-directory-aadconnect.md)
* [AD FS wdrażania platformy Azure](active-directory-aadconnect-azure-adfs.md)
* [Wysoce geograficzne krzyżowe AD FS wdrożenia w Azure przy użyciu Menedżera ruch Azure](active-directory-adfs-in-azure-with-azure-traffic-manager.md)


