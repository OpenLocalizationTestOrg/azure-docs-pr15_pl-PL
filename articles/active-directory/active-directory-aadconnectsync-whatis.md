<properties
    pageTitle="Azure AD Connect synchronizacją: opis i dostosowywanie synchronizacji | Microsoft Azure"
    description="Wyjaśniono, jak Azure AD Connect zsynchronizować działa oraz jak dostosować."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/29/2016"
    ms.author="markusvi;andkjell"/>


# <a name="azure-ad-connect-sync-understand-and-customize-synchronization"></a>Azure AD Connect synchronizacją: opis i dostosowywanie synchronizacji
Usługi Azure Active Directory łączenie synchronizacji (synchronizacja Azure AD Connect) jest główny składnik Azure AD Connect. Trwa go istotnych działań związanych z synchronizacją tożsamości między lokalnym środowiskiem a Azure AD. Azure AD Connect synchronizacja jest następnik DirSync, Azure AD Sync i Forefront Identity Manager z Azure łącznika usługi Active Directory skonfigurowane.

W tym temacie jest strona główna dla **synchronizacji Azure AD Connect** (zwanych również **aparatu synchronizacji**) i łącza do innych tematów związanych z nim list. Aby uzyskać łącza do Azure AD Connect zobacz [Integrowanie tożsamości lokalnych z usługą Azure Active Directory](active-directory-aadconnect.md).

Usługi synchronizacji składa się z dwóch części, składnik **synchronizacji Azure AD Connect** lokalnego i stronie usługi w Azure AD o nazwie **Narzędzie Azure AD Connect synchronizacji usługi**. Usługa jest często DirSync, Azure AD Sync i narzędzie Azure AD Connect.

## <a name="azure-ad-connect-sync-topics"></a>Tematy synchronizacji usługi Azure AD Connect

Temat | Co obejmuje, a kiedy do odczytu
----- | -----
**Podstawy synchronizacji usługi Azure AD Connect** |
[Omówienie architektury](active-directory-aadconnectsync-understanding-architecture.md) | Dla osób, które kto jesteś początkującym użytkownikiem aparat synchronizacji i chcesz dowiedzieć się o architektura i terminy używane.
[Pojęcia techniczne](active-directory-aadconnectsync-technical-concepts.md) | Terminy używane wyjaśniono, krótka wersja tematu architektura i zwięzły.
[Łączenie topologii dla Azure AD](active-directory-aadconnect-topologies.md) | Opis różnych topologii i scenariusze, który obsługuje aparat synchronizacji.
**Konfiguracja niestandardowa** |
[Ponownie uruchom Kreatora instalacji](active-directory-aadconnectsync-installation-wizard.md) | W tym miejscu wyjaśniono, jakie opcje są dostępne, gdy ponownie uruchom Kreatora instalacji narzędzie Azure AD Connect.
[Opis deklaracyjnych inicjowania obsługi administracyjnej](active-directory-aadconnectsync-understanding-declarative-provisioning.md)| W tym artykule opisano modelu konfiguracji o nazwie deklaracyjnych inicjowania obsługi administracyjnej.
[Opis deklaracyjnych wyrażeń obsługi administracyjnej](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md) | W tym artykule opisano składnię język wyrażenia w deklaracyjnych inicjowania obsługi administracyjnej.
[Opis konfiguracji domyślnej](active-directory-aadconnectsync-understanding-default-configuration.md)| W tym artykule opisano reguły w nowym polu i konfiguracji domyślnej. Opisano także reguł współdziałania scenariuszy z gotowych do pracy.
[Opis użytkowników i kontaktów](active-directory-aadconnectsync-understanding-users-and-contacts.md) | Nadal w poprzednim temacie oraz w tym artykule opisano, jak konfiguracja użytkowników i kontakty wyglądają łącznie, w szczególności w środowisku wielu las.
[Jak zmienić domyślną konfigurację](active-directory-aadconnectsync-change-the-configuration.md) | Przeprowadzi Cię przez proces typowych Konfiguracja z przepływem atrybut.
[Najważniejsze wskazówki dotyczące zmieniania konfiguracji domyślnej](active-directory-aadconnectsync-best-practices-changing-default-configuration.md) | Obsługuje ograniczenia i wprowadzania zmian w konfiguracji w nowym polu.
[Konfigurowanie filtrowania](active-directory-aadconnectsync-configure-filtering.md) | W tym artykule opisano różne opcje dotyczące ograniczyć, które obiekty są synchronizowane z Azure AD i krok po kroku konfigurowania tych opcji.
**Funkcje i scenariusze** |
[Zapobiegaj przypadkowym usunięciom](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md) | W tym artykule opisano funkcję *zapobiec przypadkowym usuwa* i jak go skonfigurować.
[Harmonogram](active-directory-aadconnectsync-feature-scheduler.md) | W tym artykule opisano wbudowanych harmonogram, importowanie, synchronizowanie i eksportowania danych.
[Implementowanie synchronizacji haseł](active-directory-aadconnectsync-implement-password-synchronization.md) | W tym artykule opisano, jak działa synchronizacja haseł, jak wdrażać i sposobu działania i rozwiązywanie problemów.
[Urządzenie wystornowaniem](active-directory-aadconnect-feature-device-writeback.md) | W tym artykule opisano, jak działa zapisu urządzenia w narzędzie Azure AD Connect.
[Rozszerzenia katalogu](active-directory-aadconnectsync-feature-directory-extensions.md) | Informacje dotyczące rozszerzania schematu Azure AD przy użyciu własnej atrybutami niestandardowymi.
**Usługi synchronizacji** |
[Funkcje usługi synchronizacji usługi Azure AD Connect](active-directory-aadconnectsyncservice-features.md) | W tym artykule opisano stronie usługi synchronizacji oraz jak zmienić ustawienia synchronizacji w Azure AD.
[Atrybut zduplikowany elastyczności](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md) | W tym artykule opisano, jak włączyć i używać elastyczność wartości zduplikowane atrybut **userPrincipalName** i **proxyAddresses** .
**Operacje i elementy interfejsu użytkownika** |
[Menedżer usługi synchronizacji](active-directory-aadconnectsync-service-manager-ui.md) | W tym artykule opisano interfejs użytkownika Menedżera usługi synchronizacji, w tym [operacji](active-directory-aadconnectsync-service-manager-ui-operations.md), [Łączniki](active-directory-aadconnectsync-service-manager-ui-connectors.md) [Metaverse Designer](active-directory-aadconnectsync-service-manager-ui-mvdesigner.md)i [Wyszukiwanie Metaverse](active-directory-aadconnectsync-service-manager-ui-mvsearch.md) karty.
[Zagadnienia dotyczące i zadań operacyjnych](active-directory-aadconnectsync-operations.md) | W tym artykule opisano operacyjne obaw, takich jak awarii.
**Jak...** |
[Resetowanie konta Azure AD](active-directory-aadconnectsync-howto-azureadaccount.md) | Jak zresetować poświadczenia konta usługi używany do łączenia z synchronizacją Azure AD Connect do Azure AD.
**Więcej informacji i odwołania** |
[Porty](active-directory-aadconnect-ports.md) | Lista porty, musisz otworzyć między aparat synchronizacji i katalogi w lokalnym a Azure AD.
[Atrybuty są synchronizowane z usługą Azure Active Directory](active-directory-aadconnectsync-attributes-synchronized.md) | Wyświetla listę wszystkich atrybutów synchronizowane między lokalnego AD i Azure AD.
[Funkcje odwołania](active-directory-aadconnectsync-functions-reference.md) | Lista wszystkich funkcji dostępnych w deklaracyjnych inicjowania obsługi administracyjnej.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Integrowanie tożsamości lokalnych z usługi Azure Active Directory](active-directory-aadconnect.md)
