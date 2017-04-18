<properties
    pageTitle="Raport użycia nielicencjonowanych | Microsoft Azure"
    description="Ułatwia raport bez licencji zastosowania identyfikowanie nielicencjonowanych użytkowników korzystających z wypłacane Azure AD funkcje."
    services="active-directory"
    documentationCenter=""
    authors="MarkusVi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="markvi"/>

# <a name="unlicensed-usage-report"></a>Raport użycia licencji

Ułatwia raport bez licencji zastosowania identyfikowanie nielicencjonowanych użytkowników korzystających z wypłacane Azure AD funkcje. Dzięki temu można ulepszyć za pomocą liczbę zakupionych licencji i do identyfikowania wiesz, kiedy może być konieczne dodatkowe licencje. 

Raport zawiera aktywne zastosowania funkcji płatnych w ciągu ostatnich 30 dni. 

## <a name="report-structure"></a>Struktura raportu
 
| Nazwa kolumny          |    Opis |
| :--                  | :--         |
| Bez licencji użytkownika      |    Nazwa użytkownika |
| Funkcja              | Nazwa funkcji. Na przykład: warunkowe programu access |
| Dostęp do aplikacji | Nazwa aplikacji, która jest dostępny za pomocą funkcji. Na przykład: Office 365 SharePoint Online |

 
> [AZURE.NOTE] Jeśli konto użytkownika zostało usunięte kolumny "Użytkownik bez licencji" zostanie wypełniona Identyfikatora, takich jak 1003000090D8B285


## <a name="conditional-access-feature"></a>Funkcja dostępu warunkowego

Gdy będą uzyskiwać dostęp do usługi, która ma zasady dostępu warunkowego stosowane, jeśli nie mają licencji usługi Azure AD Premium zostaną oflagowane nielicencjonowanych użytkowników. 

Dotyczy to MFA / lokalizacji zasady, a także urządzenia zasady używające Intune.
 

## <a name="see-also"></a>Zobacz też

- [Za pomocą dostępu warunkowego przy użyciu usługi Office 365 i inne usługi Azure Active Directory podłączonego aplikacji](active-directory-conditional-access.md)
- [Wprowadzenie do warunkowego dostępu do Azure AD](active-directory-conditional-access-azuread-connected-apps.md) 


