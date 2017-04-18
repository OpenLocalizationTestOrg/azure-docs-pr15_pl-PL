<properties
    pageTitle="Przypisywanie użytkowników do domeny niestandardowej w usłudze Azure Active Directory | Microsoft Azure"
    description="Jak wypełnić domeny niestandardowej w usłudze Active Directory platformy Azure za pomocą kont użytkowników."
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

# <a name="assign-users-to-a-custom-domain"></a>Przypisywanie użytkowników do domeny niestandardowej

Po dodaniu domeny niestandardowej usługi Azure Active Directory, możesz dodać konta użytkowników dla tej domeny, dzięki czemu można rozpocząć uwierzytelnianiu ich.

## <a name="users-synced-in-from-a-directory-on-your-corporate-network"></a>Użytkownicy synchronizowane w z katalogu na tej samej sieci firmowej

Jeśli już skonfigurowano połączenie między z lokalnej usługi Active Directory i usługi Azure Active Directory, synchronizacji mogą dodawać konta. Aby uzyskać więcej informacji na temat do synchronizacji z usługą Active Directory w lokalnej usługi Azure Active Directory zobacz [Integrowanie tożsamości lokalnych z usługą Azure Active Directory](active-directory-aadconnect.md).

## <a name="users-added-and-managed-in-the-cloud"></a>Użytkownicy nie zostały dodane i zarządzanych w chmurze

Aby zmienić domenę dla istniejącego konta użytkownika:

1.  Otwórz Azure portal klasyczny przy użyciu konta administratora globalnego lub użytkownikami.

2.  Otwórz katalog.

3.  Wybierz kartę **użytkowników** .

4.  Wybierz użytkownika z listy.

5.  Zmienić domenę dla użytkownika, a następnie wybierz przycisk **Zapisz**.

Można to także zrobić za pomocą [Programu Microsoft PowerShell](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains) lub [Interfejsu API wykresu](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations).

## <a name="select-a-custom-domain-when-creating-a-new-user"></a>Wybierz domenę niestandardową, podczas tworzenia nowego użytkownika

1.  Otwieranie portalu klasyczny Azure przy użyciu konta, które administrator globalny lub administrator użytkownika.

2.  Otwórz katalog.

3.  Wybierz kartę **użytkowników** .

4.  Na pasku poleceń wybierz pozycję **Dodaj**.

5.  Po dodaniu nazwy użytkownika wybierz niestandardowej domeny z listy domen.

6.  Wybierz przycisk **Zapisz**.

## <a name="next-steps"></a>Następne kroki

-   [Używanie nazw domen niestandardowych w celu uproszczenia logowanie dla użytkowników](active-directory-add-domain.md)

-   [Zarządzanie nazwami domen niestandardowych](active-directory-add-manage-domain-names.md)

-   [Więcej informacji na temat pojęć związanych z zarządzaniem domeny w Azure AD](active-directory-add-domain-concepts.md)
