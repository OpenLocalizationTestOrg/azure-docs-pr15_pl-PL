<properties
    pageTitle="Zarządzanie nazwami domen niestandardowych w usłudze Active Directory platformy Azure | Microsoft Azure"
    description="Pojęć związanych z zarządzaniem i kwestie dotyczące zarządzania niestandardowej domeny w usłudze Active Directory platformy Azure"
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

# <a name="managing-custom-domain-names-in-your-azure-active-directory"></a>Zarządzanie nazwami domen niestandardowych w usłudze Active Directory platformy Azure

Nazwa domeny jest ważną częścią identyfikator wielu zasobów katalogu: jest częścią użytkownika nazwy lub adresy e-mail dla użytkownika, część adresu w grupie, a może być częścią aplikacji identyfikator URI dla aplikacji. Zasób w usłudze Azure Active Directory (Azure AD) mogą zawierać nazwę domeny, którą jest już zweryfikowana należeć do katalogu zawierającego zasobu. Tylko administrator globalny wykonywać zadania związane z zarządzaniem domeny w Azure AD.

## <a name="set-the-primary-domain-name-for-your-azure-ad-directory"></a>Skonfiguruj nazwę domeny podstawowej dla katalogu Azure AD

Po utworzeniu katalogu pierwotną nazwę domeny, takich jak "contoso.onmicrosoft.com," jest również nazwa domeny podstawowej dla katalogu. Domena podstawowa jest domyślną nazwę domeny dla nowego użytkownika, podczas tworzenia nowego użytkownika w [portal Azure klasyczny](https://manage.windowsazure.com/)lub innych portalach społecznościowych, takich jak portalu administracyjnego usługi Office 365. To upraszcza proces dla administratora utworzyć nowych użytkowników w portalu.

Aby zmienić nazwę domeny podstawowej katalogu:

1.  Zaloguj się do [portalu klasyczny Azure](https://manage.windowsazure.com/) przy użyciu konta użytkownika, który jest administratorem globalnym katalogu Azure AD.

2.  Na pasku nawigacyjnym po lewej stronie wybierz pozycję **Usługi Active Directory** .

3.  Otwórz katalog.

4.  Wybierz kartę **domeny** .

5.  Kliknij przycisk **Zmień podstawowy** na pasku poleceń.

6.  Wybierz domenę, która ma być nowe domenę podstawową dla katalogu.

Możesz zmienić nazwę domeny podstawowej katalogu zostać zweryfikowane domeny niestandardowej, nie federacji. Zmiana domenę podstawową dla katalogu nie spowoduje zmiany nazw użytkowników dla wszystkich istniejących użytkowników.

## <a name="add-custom-domain-names-to-your-azure-ad"></a>Dodawanie niestandardowe nazwy domen do usługi Azure AD

Możesz dodać maksymalnie 900 niestandardowe nazwy domen do każdego katalogu Azure AD. Proces, aby [dodać dodatkowe niestandardowej nazwy domeny](active-directory-add-domain.md) jest taki sam dla pierwszej nazwy domeny niestandardowej.

## <a name="add-subdomains-of-a-custom-domain"></a>Dodać poddomen domeny niestandardowej

Jeśli chcesz dodać do katalogu nazwy domeny trzeciego poziomu, takie jak "europe.contoso.com", należy najpierw dodać i zweryfikować domenę drugiego poziomu, na przykład contoso.com. Poddomena będzie automatycznie weryfikowane przez Azure AD. Aby wyświetlić zweryfikowano poddomeny, który właśnie został dodany, Odśwież stronę w przeglądarce, która zawiera listę domen w katalogu.

## <a name="what-to-do-if-you-change-the-dns-registrar-for-your-custom-domain-name"></a>Co zrobić, jeśli zmienisz DNS rejestratora dla niestandardowej nazwy domeny

Jeśli zmienisz DNS rejestratora dla niestandardowej nazwy domeny, możesz nadal korzystać niestandardowej nazwy domeny z Azure AD, sam bez zakłóceń i dodatkowe zadania konfiguracyjne. Jeśli używasz niestandardowej nazwy domeny z usługi Office 365, Intune lub innymi usługami, zależne od nazwy domeny niestandardowej w Azure AD można znaleźć w dokumentacji tych usług.

## <a name="delete-a-custom-domain-name"></a>Usuwanie niestandardowej nazwy domeny

Jeśli organizacja korzysta już z tej nazwy domeny lub jeśli musisz użyć tej nazwy domeny z inną usługą Azure Active Directory, możesz usunąć niestandardowej nazwy domeny z usługi Azure AD.

Aby usunąć niestandardowej nazwy domeny, najpierw upewnij się, że żaden zasób w katalogu zależeć nazwę domeny. Nie można usunąć nazwy domeny w katalogu, jeśli:

-   Każdy użytkownik ma nazwę użytkownika, adres e-mail lub adres serwera proxy, która zawiera nazwę domeny.

-   Każda grupa ma adres e-mail lub adres serwera proxy, która zawiera nazwę domeny.

-   Aplikacje w usługi Azure AD zawiera aplikację identyfikator URI, który zawiera nazwę domeny.

Należy zmienić lub usunąć takie zasobu w katalogu Azure AD, przed usunięciem nazwy domeny niestandardowej.

## <a name="use-powershell-or-graph-api-to-manage-domain-names"></a>Zarządzanie nazwami domen przy użyciu programu PowerShell lub interfejsu API wykresu

Większość zadań zarządzania dla nazwy domeny w usłudze Azure Active Directory można również przeprowadzić przy użyciu programu Microsoft PowerShell lub programowo przy użyciu interfejsu API Azure AD wykresu (w podglądzie publicznej).

-   [Zarządzanie nazwami domen w Azure AD przy użyciu programu PowerShell](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)

-   [Zarządzanie nazwami domen w Azure AD przy użyciu interfejsu API wykresu](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations)

## <a name="next-steps"></a>Następne kroki

-   [Informacje o nazwach domen w Azure AD](active-directory-add-domain-concepts.md)

-   [Zarządzanie nazwami domen niestandardowych](active-directory-add-manage-domain-names.md)
