<properties
    pageTitle="Zmienianie podpisu algorytm skrótu dla usługi Office 365 odpowiedzieć zaufania firmy | Microsoft Azure"
    description="Ta strona zawiera wskazówki dotyczące zmieniania Agent kondycji systemu algorytm relacji zaufania federacji z usługi Office 365"
    keywords="SHA1, SHA256, usługi Office 365, Federacja aadconnect, adfs, usług ad fs, zmień Agent kondycji systemu, zaufania federacji, opierając zaufania firmy"
    services="active-directory"
    documentationCenter=""
    authors="anandyadavmsft"
    manager="samueld"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/01/2016"
    ms.author="anandy"/>

# <a name="change-signature-hash-algorithm-for-office-365-replying-party-trust"></a>Zmienianie podpisu algorytmu skrótu dla usługi Office 365 odpowiadania zaufania firmy

## <a name="overview"></a>Omówienie

Azure Active Directory Federation Services (AD FS) podpisuje jego tokenów Microsoft Azure Active Directory w celu zapewnienia, nie może zostać zmieniona od. Podpis mogą być oparte na SHA1 lub SHA256. Azure Active Directory obsługuje teraz tokeny zalogowani za pomocą algorytmu SHA256 i zaleca się ustawienie algorytm podpisywania tokenu SHA256 najwyższego poziomu zabezpieczeń. W tym artykule opisano czynności, aby ustawić algorytm podpisywania tokenu zabezpieczyć SHA256 poziom.

## <a name="change-the-token-signing-algorithm"></a>Zmienianie algorytm podpisywania tokenu

Po ustawieniu algorytmu podpisu z jednego z dwóch poniższych procesów usług AD FS podpisuje tokeny dla usługi Office 365 Polegaj zaufania firmy z SHA256. Nie musisz wprowadzić zmiany w konfiguracji dodatkowe i ta zmiana nie ma wpływu na możliwości dostępu do usługi Office 365 lub inne aplikacje Azure AD.

### <a name="ad-fs-management-console"></a>Konsola zarządzania AD FS

1. Otwórz konsolę zarządzania usług AD FS na podstawowy serwer usług AD FS.
2. Rozwiń węzeł usług AD FS i kliknij pozycję **Strona Polegaj zaufanie**.
3. Kliknij prawym przyciskiem myszy usługi Office 365-Azure uzależnionej strony zaufania i wybierz polecenie **Właściwości**.
4. Wybierz kartę **Zaawansowane** i wybierz algorytmu mieszania bezpiecznego SHA256.
5. Kliknij **przycisk OK**.

![Algorytm podpisywania SHA256 — konsoli MMC](./media/active-directory-aadconnectfed-sha256guidance/mmc.png)

### <a name="ad-fs-powershell-cmdlets"></a>AD FS poleceń cmdlet

1. Na dowolnej serwer usług AD FS Otwórz programu PowerShell w obszarze uprawnienia administratora.
2. Ustawianie algorytmu mieszania bezpiecznego przy użyciu polecenia cmdlet **Set-AdfsRelyingPartyTrust** .

 <code>Set-AdfsRelyingPartyTrust -TargetName 'Microsoft Office 365 Identity Platform' -SignatureAlgorithm 'http://www.w3.org/2001/04/xmldsig-more#rsa-sha256'</code>

## <a name="also-read"></a>Zobacz temat

* [Naprawianie zaufania usługi Office 365 z Azure AD Connect](./active-directory-aadconnect-federation-management.md#repairing-the-trust)
