<properties
    pageTitle="Narzędzie Azure AD Connect: Synchronizowanie wystąpień usługi | Microsoft Azure"
    description="Ta strona dokumenty uwagi dotyczące wystąpienia Azure AD."
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
    ms.date="06/27/2016"
    ms.author="billmath"/>

# <a name="azure-ad-connect-special-considerations-for-instances"></a>Narzędzie Azure AD Connect: Uwagi dotyczące wystąpienia
Narzędzie Azure AD Connect jest najczęściej używane w całym świecie wystąpieniu programu Azure AD i usługi Office 365. Istnieją również inne wystąpienia a te mają różne wymagania dotyczące adresy URL i inne uwagi.

## <a name="microsoft-cloud-germany"></a>Niemcy chmury firmy Microsoft
[Niemcy chmury firmy Microsoft](http://www.microsoft.de/cloud-deutschland) jest chmurę suwerennych obsługiwana przez administratora niemieckim danych.

Ten chmurze jest obecnie w podglądzie. Wiele scenariuszy, które zwykle można wykonywać samodzielnie, takich jak zweryfikować domen, należy przeprowadzić operatorem. Skontaktuj się z lokalnym przedstawicielem firmy Microsoft, aby uzyskać więcej informacji na temat uczestniczyć w podglądzie.

Adresy URL, aby otworzyć w serwer proxy |
--- |
\*. microsoftonline.de |
\*. windows.net |
+ Listy odwołań certyfikatów |

Po zalogowaniu się do katalogu Azure AD należy użyć konta w tej domenie onmicrosoft.de.

Obecnie nie ma w programie Microsoft Cloud Niemcy funkcje:

- Azure AD łączenie kondycji nie jest dostępna.
- Aktualizacje automatyczne nie jest dostępna.
- Hasło zapisu nie jest dostępna.

## <a name="microsoft-azure-government-cloud"></a>Chmura Microsoft Azure dla instytucji rządowych
W [chmurze firmy Microsoft Azure dla instytucji rządowych](https://azure.microsoft.com/features/gov/) jest chmury dla Rządu Stanów Zjednoczonych.

Ten chmurze jest obsługiwane w starszych wersjach DirSync. Z kompilacji 1.1.180 narzędzie Azure AD Connect następnej Generowanie chmury jest obsługiwane. Generowania jest używany tylko do USA podstawie punkty końcowe i mieć różne listy adresów URL, aby otworzyć w serwera proxy.

Adresy URL, aby otworzyć w serwer proxy |
--- |
\*. microsoftonline.com |
\*. gov.us.microsoftonline.com |
+ Listy odwołań certyfikatów |

Narzędzie Azure AD Connect nie będzie automatycznie wykrywa, że katalogu Azure AD znajduje się w chmurze dla instytucji rządowych. Zamiast tego należy wykonać poniższe czynności podczas instalacji narzędzie Azure AD Connect.

1. Uruchom narzędzie Azure AD Connect instalację.
2. Jak wyświetlić pierwszej strony, której użytkownik ma Zaakceptuj Umowę licencyjną, nie Kontynuuj, ale pozostawić uruchomiony Kreator instalacji.
3. Uruchom polecenie regedit i zmienianie klucza rejestru `HKLM\SOFTWARE\Microsoft\Azure AD Connect\AzureInstance` na wartość `2`.
4. Przejdź do Kreatora instalacji narzędzie Azure AD Connect zaakceptować umowę licencyjną i kontynuować. Podczas instalacji upewnij się, ścieżka instalacji **niestandardowej konfiguracji** (a nie Instalacja ekspresowa). Następnie kontynuować instalację w zwykły sposób.

Obecnie nie ma w chmurze firmy Microsoft Azure dla instytucji rządowych funkcje:

- Azure AD łączenie kondycji nie jest dostępna.
- Aktualizacje automatyczne nie jest dostępna.
- Hasło zapisu nie jest dostępna.

## <a name="next-steps"></a>Następne kroki
Dowiedz się więcej o [integrowaniu lokalnego tożsamości z usługą Azure Active Directory](active-directory-aadconnect.md).
