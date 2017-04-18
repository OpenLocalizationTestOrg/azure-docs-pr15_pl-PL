<properties
    pageTitle="Omówienie niestandardowe nazwy domen w usłudze Active Directory platformy Azure | Microsoft Azure"
    description="W tym miejscu wyjaśniono koncepcyjny podstawę przy użyciu nazw domen niestandardowych w usłudze Azure Active directory, w tym Federacja logowania jednokrotnego"
    services="active-directory"
    documentationCenter=""
    authors="jeffsta"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="curtand;jeffsta"/>

# <a name="conceptual-overview-of-custom-domain-names-in-azure-active-directory"></a>Omówienie niestandardowe nazwy domen w usłudze Active Directory platformy Azure

Nazwa domeny jest ważną częścią identyfikator wielu zasobów katalogu: jest częścią użytkownika nazwy lub adresy e-mail dla użytkownika, część adresu w grupie, a może być częścią aplikacji identyfikator URI dla aplikacji. Zasób w usłudze Azure Active Directory (Azure AD) mogą zawierać nazwę domeny, którą jest już zweryfikowana należeć do katalogu zawierającego zasobu. Tylko administrator globalny wykonywać zadania związane z zarządzaniem domeny w Azure AD.

Nazwy domen w Azure AD są globalnie unikatowe. Nazwa domeny mogą być używane przez pojedynczy Azure AD. Jeśli jednego katalogu Azure AD sprawdziła nazwę domeny, nie katalogu Azure AD można sprawdzić lub użyć tej samej nazwy domeny.

## <a name="initial-and-custom-domain-names"></a>Nazwy domen początkowych i niestandardowe

Nazwa domeny jest każdej Azure AD jest pierwotną nazwę domeny lub niestandardowej nazwy domeny.

Co Azure AD zawiera pierwotną nazwę domeny w formularzu contoso.onmicrosoft.com. Ta nazwa trzecia poziomu domeny, w tym przykładzie "contoso.onmicrosoft.com," określono podczas tworzenia katalogu, zwykle przez administratora, który utworzył katalogu. Pierwotną nazwę domeny dla katalogu nie można zmienić ani usunąć. Pierwotną nazwę domeny, gdy jest w pełni funkcjonalny jest przeznaczona przede wszystkim, może być używany jako mechanizm bootstrapping, dopóki nie zostanie zweryfikowana niestandardowej nazwy domeny.

W większości środowisku produkcyjnym katalog ma co najmniej jeden zatwierdzonych domenę niestandardową, na przykład "contoso.com", i jest tej domeny niestandardowej, który jest widoczny dla użytkowników końcowych. Niestandardowej nazwy domeny jest nazwą domeny, jest własnością i używane przez tej organizacji, na przykład "contoso.com" do zastosowań, takich jak hostingu swojej witryny sieci web. Ta nazwa domeny jest znany pracownikom, ponieważ jest część nazwy użytkownika, który mogą używać do zalogowania się do sieci firmowej lub do wysyłania i pobierania poczty e-mail.

Przed mogą być używane przez Azure AD, nazwę domeny niestandardowej musi dodane do katalogu i zweryfikowana.

## <a name="verified-and-unverified-domain-names"></a>Sprawdzone i niezweryfikowany domen

Pierwotną nazwę domeny dla katalogu jest niejawnie szacowana jako zweryfikowany przez Azure AD. Gdy administrator doda niestandardowej nazwy domeny do Azure AD, jest początkowo w stanie niezweryfikowany. Azure AD uniemożliwi wszystkie zasoby katalogu w celu użycia nazwy domeny niezweryfikowany. Dzięki temu tylko jednego katalogu można używać nazwy domeny, a czy organizacja korzysta z nazwą domeny jest faktycznie właścicielem tej nazwy domeny.

Azure AD sprawdza własności nazwy domeny, szukając określonego wpisu w pliku strefy usługi (DNS) nazwy domeny dla nazwy domeny. Aby sprawdzić własności nazwy domeny, administrator pobiera wpis DNS z Azure AD Azure AD wyszuka i dodaje ten wpis do pliku strefy DNS dla nazwy domeny. Plik strefy DNS są obsługiwane przez rejestratora nazw domen dla tej domeny. Kroki w celu zweryfikowania domeny są przedstawione w artykule dodawania [domeny niestandardowej do katalogu Azure AD](active-directory-add-domain.md).

Dodawanie wpisu DNS do pliku strefy dla nazwy domeny nie wpływa na innych usług domeny, takich jak wiadomości e-mail lub Internecie.

## <a name="federated-and-managed-domain-names"></a>Nazwy domen federacyjnych i zarządzanie nimi

Niestandardowej nazwy domeny w Azure AD można skonfigurować udostępniający użytkownikom znak federacyjnych w oknie interfejs między lokalnej usługi Active Directory a Azure AD. Konfigurowanie domeny dla Federacji wymaga aktualizacji uprzywilejowanych zasobów w Azure AD, a także do systemu Windows Server usługi Active Directory. Konfigurowanie wykonywane federacyjnych domeny musi być z Azure AD Connect lub przy użyciu programu PowerShell. Nie można zainicjować federacyjnego domenę niestandardową z portalu klasyczny Azure. [Obejrzyj ten klip wideo, aby uzyskać informacje o konfigurowaniu usług AD FS dla użytkownika Zaloguj się przy użyciu Azure AD Connect](http://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Configuring-AD-FS-for-user-sign-in-with-Azure-AD-Connect).

Domeny, które nie należą do Federacji są nazywane zarządzanych domen. Domenę początkową dla katalogu Azure AD niejawnie jest szacowana jako zarządzaną domeny.

## <a name="primary-domain-names"></a>Nazwy domeny podstawowej

Nazwa domeny podstawowej katalogu jest nazwę domeny, którą jest wstępnie zaznaczone jako wartość domyślna "domena" część nazwy użytkownika, gdy administrator tworzy nowego użytkownika w [portal Azure klasyczny](https://manage.windowsazure.com/) lub innego portalu, takich jak portalu administracyjnego usługi Office 365. Katalog może mieć tylko jedną nazwę domeny podstawowej. Administrator może zmienić nazwa domeny podstawowej być dowolną zweryfikować domenę niestandardową, nie Federacji, lub na domenę początkową.

## <a name="domain-names-in-azure-ad-and-other-microsoft-online-services"></a>Nazwami domen w Azure AD i innych usług Microsoft Online Services

Nazwa domeny musi zostać zweryfikowane w Azure AD, zanim będzie można używać innej usługi Online firmy Microsoft, takich jak usługi Exchange Online, usłudze SharePoint Online i usługi. Inne usługi zwykle wymagają administrator dodać wpisy DNS, które usługi.

Azure w przeglądarce używa mechanizm w celu zweryfikowania własności domeny. Musi zostać zweryfikowane domenę do użytku z usługą Azure Active Directory, nawet jeśli został wcześniej sprawdzony do użycia przez aplikację sieci web Azure w subskrypcji, która zależy od tego Azure AD. Azure w przeglądarce można użyć nazwy domeny, która została zweryfikowana w innym katalogu z katalogu, które będą zabezpieczać aplikacji sieci web.

## <a name="managing-domain-names"></a>Zarządzanie nazwami domen

Zadania związane z zarządzaniem domeny mogą być wykonywane z portalem klasyczny Azure i z programu PowerShell. Wiele zadań można wykonać przy użyciu interfejsu API Azure AD wykresu (w publicznej wersja preview).

-   [Dodawanie i sprawdzanie niestandardowej nazwy domeny](active-directory-add-domain.md)

-   [Zarządzanie domenami w portalu klasyczny Azure](active-directory-add-manage-domain-names.md)

-   [Zarządzanie nazwami domen w Azure AD przy użyciu programu PowerShell](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)

-   [Zarządzanie nazwami domen w Azure AD przy użyciu interfejsu API Azure AD wykresu](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations)
