<properties
    pageTitle="Azure MFA i AD FS | Microsoft Azure"
    description="To jest strona uwierzytelnianie wieloskładnikowe Azure opisujący sposób rozpocząć pracę z Azure MFA i usług AD FS."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na" ms.topic="get-started-article"
    ms.date="10/17/2016"
    ms.author="kgremban"/>

# <a name="getting-started-with-azure-multi-factor-authentication-and-active-directory-federation-services"></a>Wprowadzenie do usług federacyjnych Active Directory i uwierzytelnianie wieloskładnikowe Azure



<center>![Chmury](./media/multi-factor-authentication-get-started-adfs/adfs.png)</center>

Jeśli Twoja organizacja ma federacji z lokalnej usługi Active Directory z usługi Azure Active Directory przy użyciu usług AD FS, istnieją dwie opcje, przy użyciu funkcji uwierzytelniania wieloskładnikowego Azure.

- Zapewnienie zasobów chmurze przy użyciu uwierzytelnianie wieloskładnikowe Azure lub usługi federacyjne Active Directory
- Zabezpieczanie chmury i lokalnych zasobów za pomocą serwera uwierzytelniania wieloskładnikowego Azure

W poniższej tabeli wymieniono środowiska weryfikacji między zabezpieczanie zasobów z uwierzytelnianie wieloskładnikowe Azure i usług AD FS

|Weryfikacja obsługi - obsługiwanymi Przeglądaj|Środowisko Weryfikacja — nie opartych na przeglądarce aplikacje
:------------- | :------------- | :------------- |
Zabezpieczanie zasobów Azure AD przy użyciu funkcji uwierzytelniania wieloskładnikowego Azure |<li>Pierwszym krokiem weryfikacji jest wykonywane przy użyciu usług AD FS lokalnego.</li> <li>Drugim krokiem jest to metoda telefoniczna przeprowadzane przy użyciu funkcji uwierzytelniania chmury.</li>|Użytkownicy końcowi umożliwia hasła aplikacji logowania.
Zabezpieczanie zasobów Azure AD przy użyciu usług federacyjnych Active Directory |<li>Pierwszym krokiem weryfikacji jest wykonywane przy użyciu usług AD FS lokalnego.</li><li>Drugim krokiem jest wykonywana w lokalnej przy zachowaniu roszczenia.</li>|Użytkownicy końcowi umożliwia hasła aplikacji logowania.

Ostrzeżenia przy użyciu hasła aplikacji federacyjnych użytkowników:

- Hasła aplikacji są sprawdzane przy użyciu uwierzytelniania w chmurze, więc ich pomijać federacji. Federacja tylko aktywnie jest używana podczas konfigurowania hasła.
- Ustawienia kontroli dostępu klienta lokalnego nie są uznane za hasła aplikacji.
- Tracisz lokalnego możliwości rejestrowania uwierzytelniania hasła aplikacji.
- Usuwanie Wyłącz konta może potrwać do trzech godzin dla synchronizacji katalogów, opóźnianie Wyłącz/usuwanie hasła aplikacji w chmurze tożsamości.

## <a name="next-steps"></a>Następne kroki

Aby uzyskać informacje dotyczące konfigurowania uwierzytelnianie wieloskładnikowe Azure lub serwerze uwierzytelnianie wieloskładnikowe Azure za pomocą usług AD FS, zobacz następujące artykuły:

- [Zabezpieczanie zasobów chmurze za pomocą uwierzytelnianie wieloskładnikowe Azure i usług AD FS](multi-factor-authentication-get-started-adfs-cloud.md)
- [Zapewnienie chmury i lokalnych zasobów serwera uwierzytelniania wieloskładnikowego Azure za pomocą programu Windows Server 2012 R2 AD FS](multi-factor-authentication-get-started-adfs-w2k12.md)
- [Zapewnienie chmury i lokalnych zasobów serwera uwierzytelniania wieloskładnikowego Azure za pomocą usług AD FS 2.0](multi-factor-authentication-get-started-adfs-adfs2.md)
