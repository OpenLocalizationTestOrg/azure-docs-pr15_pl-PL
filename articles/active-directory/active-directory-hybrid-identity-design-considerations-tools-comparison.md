<properties
    pageTitle="Tożsamość hybrydowych: Integracja z katalogiem narzędzia Porównanie | Microsoft Azure"
    description="To jest strona zwiera pełna tabeli porównano różne narzędzia integracji katalogu, które mogą być używane do integracji katalogów."
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/08/2016"
    ms.author="billmath"/>

# <a name="hybrid-identity-directory-integration-tools-comparison"></a>Integracja katalogu tożsamości hybrydowych narzędzia porównania

Narzędzia integracji katalogu mają latach przeznaczonych i utworzony na.  Ten dokument jest w celu uzyskania skonsolidowany widok te narzędzia i porównanie funkcje, które są dostępne w każdym.

<!-- The hardcoded link is a workaround for campaign ids not working in acom links-->

>[AZURE.NOTE] Narzędzie Azure AD Connect zawiera funkcje wcześniej wydane jako Dirsync i AAD Sync i składników. Te narzędzia są już opublikowanej pojedynczo i wszystkich przyszłych ulepszeń zostaną uwzględnione w aktualizacji narzędzie Azure AD Connect, tak, aby zawsze wiedzieć, gdzie można korzystać z najnowszych funkcji.
>
>DirSync i Azure AD Sync są przestarzałe. Więcej informacji można znaleźć w [tym miejscu](active-directory-aadconnect-dirsync-deprecated.md).


Za pomocą następującego klucza dla każdej tabeli.

● = Teraz dostępna  
FR = przyszłej wersji  
PP = publicznej Preview  

## <a name="on-premises-to-cloud-synchronization"></a>Lokalne do chmury synchronizacji

| Funkcja  | Łączenie usługi Azure Active Directory  | Usługi synchronizacji usługi Azure Active Directory (AAD Sync) | Narzędzie do synchronizacji usługi Azure Active Directory (DirSync)| Produkt Forefront Identity Manager 2010 R2 (FIM) |Menedżer tożsamości 2016 (z wieloma Uczestnikami)|
| :-------- |:--------:|:--------:|:--------:|:--------:|:--------:
| Nawiązywanie połączenia z jednym las AD lokalnego | ● | ● | ● | ● |● |
| Nawiązywanie połączenia z wielu lasów AD lokalnego |●  | ● |  | ● |● |
| Nawiązywanie połączenia z wielu lokalnego serwera Exchange organizacjach | ● |  |  |  | |
| Nawiązywanie połączenia z jednym lokalnego katalogu LDAP | FR |  |  | ● |● |
| Nawiązywanie połączenia z wielu katalogów LDAP lokalnego |FR  |  |  | ● |● |
| Nawiązywanie połączenia z lokalnym AD i lokalnych katalogów LDAP |FR  |  |  | ● |● |
| Nawiązywanie połączenia z systemów niestandardowe (to znaczy SQL, Oracle, MySQL itp.) | FR |  |  | ● |● |
| Synchronizowanie atrybuty odbiorcy zdefiniowane (Katalog rozszerzenia) | ● |  |  |  |  |
|Nawiązywanie połączenia z lokalnym HR (to znaczy SAP, Oracle eBusiness PeopleSoft)| FR |  |  | ● |● |
|Obsługuje łączniki i reguły synchronizacji FIM umożliwiający ustanawianie systemów lokalnego.|  |  |  | ● |● |

## <a name="cloud-to-on-premises-synchronization"></a>Chmurze do lokalnych synchronizacji

| Funkcja  | Łączenie usługi Azure Active Directory  | Usługi synchronizacji usługi Azure Active Directory | Narzędzie do synchronizacji usługi Azure Active Directory (DirSync) | Produkt Forefront Identity Manager 2010 R2 (FIM) |Menedżer tożsamości 2016 (z wieloma Uczestnikami)|
| :-------- |:--------:|:--------:|:--------:|:--------:|:--------:
| Wystornowania urządzeń | ● |  | ● |  ||
| Atrybut zapisu (w przypadku wdrożenia hybrydowego programu Exchange) | ● | ● | ● | ● |● |
| Zapisu obiekty użytkowników i grup |  ●|  | |  ||
| Zapisu haseł (od Resetowanie samodzielne hasła (SSPR) i zmiana hasła). |  ● | ● |  |  ||



## <a name="authentication-feature-support"></a>Obsługa funkcji uwierzytelniania

| Funkcja  | Łączenie usługi Azure Active Directory | Usługi synchronizacji usługi Azure Active Directory | Narzędzie do synchronizacji usługi Azure Active Directory (DirSync) | Produkt Forefront Identity Manager 2010 R2 (FIM) |Menedżer tożsamości 2016 (z wieloma Uczestnikami)|
| :-------- |:--------:|:--------:|:--------:|:--------:|:--------:
| Synchronizacji haseł dla pojedynczego las AD lokalnego | ● | ● | ● |  ||
| Synchronizacji haseł dla wielu lasów AD lokalnego |  ●| ● |  |  ||
| Rejestracji jednokrotnej z funkcją Federacji | ● | ● | ● | ● |● |
| Wystornowania haseł (od zmian SSPR i hasła) |●  | ● |  |  ||



## <a name="set-up-and-installation"></a>Instalacja i Konfiguracja

| Funkcja  | Łączenie usługi Azure Active Directory  | Usługi synchronizacji usługi Azure Active Directory | Narzędzie do synchronizacji usługi Azure Active Directory (DirSync) | Menedżer tożsamości 2016 (z wieloma Uczestnikami) |
| :-------- |:--------:|:--------:|:--------:|:--------:
| Obsługuje instalację na kontrolerze domeny | ● | ● | ● |  |
| Obsługa instalacji przy użyciu programu SQL Express | ● | ● | ● |  |
| Łatwe uaktualnieniu DirSync |● |  |  |  |
| Lokalizacja interfejsu użytkownika administratora dla języków w systemie Windows Server | ● | ● | ● |  |
|Lokalizacji użytkownika końcowego interfejsu użytkownika dla języków w systemie Windows Server| |  |  |● |
| Pomoc techniczna dla systemu Windows Server 2008 i Windows Server 2008 R2 | ● do synchronizacji, nie dla Federacji| ● | ●  | ● |
| Pomoc techniczna dla systemu Windows Server 2012 i Windows Server 2012 R2 | ● | ● | ● | ● |

## <a name="filtering-and-configuration"></a>Filtrowanie i konfiguracji

Funkcja  | Łączenie usługi Azure Active Directory | Usługi synchronizacji usługi Azure Active Directory | Narzędzie do synchronizacji usługi Azure Active Directory (DirSync) | Produkt Forefront Identity Manager 2010 R2 (FIM)| Menedżer tożsamości 2016 (z wieloma Uczestnikami)
:-------- |:--------:|:--------:|:--------:|:--------:|:--------:|
Filtr w domen i jednostek organizacyjnych | ● | ● | ● | ●  | ●
Filtrowanie według wartości atrybutu obiektów | ● | ● | ● | ●| ●
Zezwól programowi minimalny zestaw atrybutów, które mają być synchronizowane (MinSync) | ● | ● |  ||
Zezwalanie innej usługi szablony mają być stosowane do przepływów atrybut |●  | ● |  ||
Zezwalaj na odebranie atrybuty wynikających z AD do Azure AD | ● | ● |  |  |
Zezwalaj na zaawansowane dostosowanie przepływów atrybut | ● | ● |  | ●  | ●

## <a name="next-steps"></a>Następne kroki
Dowiedz się więcej o [integrowaniu lokalnego tożsamości z usługą Azure Active Directory](active-directory-aadconnect.md).
