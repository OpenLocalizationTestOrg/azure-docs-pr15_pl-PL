<properties
   pageTitle="Azure AD Connect synchronizacją: Katalog rozszerzenia | Microsoft Azure"
   description="W tym temacie opisano funkcję rozszerzenia katalog w narzędzie Azure AD Connect."
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/19/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-sync-directory-extensions"></a>Azure AD Connect synchronizacją: rozszerzenia katalogu
Katalog rozszerzenia pozwala na rozszerzanie schematu na Azure AD przy użyciu własnej atrybutów z lokalnej usługi Active Directory. Ta funkcja umożliwia tworzenie aplikacji LOB przez inne atrybuty nadal zarządzać lokalnego. Następujące atrybuty mogą zostać użyte do [rozszerzenia katalogu Azure AD wykresu](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions) lub [Programu Microsoft Graph](https://graph.microsoft.io/). Można zobaczyć atrybuty dostępne odpowiednio za pomocą [Eksploratora Azure AD wykresu](https://graphexplorer.cloudapp.net) i [Eksploratora programu Microsoft Graph](https://graphexplorer2.azurewebsites.net/) .

Obecnie nie obciążenie pracą usługi Office 365 korzysta z tych atrybutów.

Konfigurowanie dodatkowych atrybutów, które chcesz zsynchronizować w ścieżce ustawienia niestandardowe w Kreatorze instalacji.
![Kreator rozszerzenia schematu](./media/active-directory-aadconnectsync-feature-directory-extensions/extension2.png) instalacji zawiera następujące atrybuty, które są prawidłowe kandydatami:

- Typy obiektów użytkowników i grup
- Atrybuty pojedynczej wartości: ciąg, wartość logiczna, liczba całkowita, binarny
- Atrybuty wielowartościowe: ciąg, binarny

Lista atrybutów jest odczytu z pamięci podręcznej tworzone podczas instalacji narzędzie Azure AD Connect. Jeśli masz rozszerzenia schematu usługi Active Directory przy użyciu dodatkowych atrybutów, [muszą zostać odświeżone schematu](active-directory-aadconnectsync-installation-wizard.md#refresh-directory-schema) przed tych nowych atrybutów są widoczne.

Do obiektu można dodać do 100 atrybutów rozszerzenia katalogu. Maksymalna długość wynosi 250 znaków. Jeśli wartość atrybutu jest dłuższa, jego wartość zostanie obcięta przez aparat synchronizacji.

Podczas instalacji narzędzie Azure AD Connect aplikacji jest zarejestrowana, gdzie są dostępne następujące atrybuty. Można zobaczyć tej aplikacji w portalu Azure.  
![Schemat rozszerzenia aplikacji](./media/active-directory-aadconnectsync-feature-directory-extensions/extension3.png)

Te atrybuty są teraz dostępne za pośrednictwem wykresu:  
![Wykres](./media/active-directory-aadconnectsync-feature-directory-extensions/extension4.png)

Atrybuty są prefiksem rozszerzenia\_{AppClientId}\_. AppClientId ma taką samą wartość dla wszystkich atrybutów w katalogu Azure AD.

## <a name="next-steps"></a>Następne kroki
Dowiedz się, jak [zsynchronizować Azure AD Connect](active-directory-aadconnectsync-whatis.md) konfiguracji.

Dowiedz się więcej o [integrowaniu lokalnego tożsamości z usługą Azure Active Directory](active-directory-aadconnect.md).
