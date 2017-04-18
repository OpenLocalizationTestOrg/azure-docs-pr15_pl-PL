<properties
    pageTitle="Ustawienia zasad i MDM grupy | Microsoft Azure"
    description="Informacje na temat zasad grupy i urządzenia przenośnego ustawienia zarządzania (MDM), które powinny być używane w firmowej posiadanych urządzeń. Te zasady są stosowane do całej urządzenia użytkownika."
    services="active-directory"
    keywords="Co to są grupy — zasady i ustawienia MDM dla mobilnego stan przedsiębiorstwa mobilny stan przedsiębiorstwa, chmury systemu windows"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor="curtand"/>

<tags
    ms.service="active-directory"  
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>

# <a name="group-policy-and-mdm-settings"></a>Ustawienia zasad grupy i MDM

Używać tych zasad grupy i ustawienia zarządzania (MDM) urządzenia przenośnego tylko w firmowej posiadanych urządzeń, ponieważ te zasady są stosowane do całej urządzenia użytkownika. Stosowanie zasad MDM, aby wyłączyć ustawienia synchronizacji dla osobiste należące do użytkownika urządzenia będą mieć negatywny wpływ korzystanie z tego urządzenia. Ponadto innych kont użytkowników na tym urządzeniu również dotyczy zasady.

Przedsiębiorstw, które chcesz zarządzać mobilność urządzeń osobistych (niezarządzanej) mogą korzystać z portalu Azure, aby włączyć lub wyłączyć mobilnego dostępu do ustawień, a nie za pomocą zasad grupy lub funkcją zarządzanie urządzeniami przenośnymi
W poniższych tabelach opisano ustawienia zasad dostępne.

## <a name="mdm-settings"></a>Ustawienia usługi MDM
Zastosuj ustawienia zasad MDM dla systemu Windows 10 i Windows 10 Mobile.  Pomoc techniczna systemu Windows 10 Mobile istnieje tylko w przypadku konta Microsoft podstawie mobilnego za pomocą konta usługi OneDrive.  Zajrzyj do sekcji "Urządzenia i punkty końcowe" szczegółowe informacje na temat synchronizowania Azure AD podstawie jakie urządzenia są obsługiwane.

| Nazwa                               | Opis                                                          |
|------------------------------------|----------------------------------------------------------------------|
| Zezwalaj na połączenia z kontem Microsoft | Umożliwia użytkownikom uwierzytelnianie za pomocą konta Microsoft na tym urządzeniu |
| Zezwalaj na moje ustawienia synchronizacji             | Umożliwia użytkownikom mobilny dostęp do ustawień systemu Windows i aplikacji danych; Wyłączenie tej zasady spowoduje wyłączenie synchronizacji, a także kopii zapasowych na urządzeniach przenośnych                  |

## <a name="group-policy-settings"></a>Ustawienia zasad grupy
Ustawienia zasad grupy dotyczą urządzeń systemu Windows 10, dołączone do domeny usługi Active Directory. Tabela zawiera również starsze ustawienia pojawiających zarządzać ustawieniami synchronizacji, ale nie działają przedsiębiorstwa stan mobilnego dla systemu Windows 10, które przedstawiono z "Nie należy używać" w opisie.

| Nazwa                                | Opis |
|-------------------------------------|-------------|
| Konta: Konta serwera Microsoft blok  |To ustawienie uniemożliwia użytkownikom dodawanie nowego konta serwera Microsoft na tym komputerze|
| Synchronizacji                         |Uniemożliwia użytkownikom mobilny dostęp do ustawień systemu Windows i dane aplikacji|
| Nie synchronizować personalizowanie             |Wyłącza synchronizowania grupa motywy|
| Nie Synchronizuj ustawienia przeglądarki        |Wyłącza synchronizację grupy programu Internet Explorer|
| Synchronizacji haseł               |Wyłącza synchronizacji haseł grupy|
| Nie synchronizować inne ustawienia systemu Windows  |Wyłącza synchronizację inne okna ustawień grupy|
| Nie zsynchronizować personalizacja pulpitu |Nie należy używać; nie działa|
| Nie można synchronizować na połączeń taryfowych  |Wyłącza mobilnego na taryfowe połączenia, takie jak transmisji 3 G|
| Nie Synchronizowanie aplikacji                    |Nie należy używać; nie działa|
|Nie Synchronizowanie ustawień aplikacji             |Wyłącza mobilnych danych aplikacji|
|Nie Synchronizuj ustawienia start           |Nie należy używać; nie działa|


## <a name="related-topics"></a>Tematy pokrewne
- [Omówienie mobilnych stan przedsiębiorstwa](active-directory-windows-enterprise-state-roaming-overview.md)
- [Włącz stan enterprise mobilnego dostępu do ustawień w usłudze Active Directory platformy Azure](active-directory-windows-enterprise-state-roaming-enable.md)
- [Ustawienia i dane mobilnych — często zadawane pytania](active-directory-windows-enterprise-state-roaming-faqs.md)
- [Odwołanie ustawienia mobilnego systemu Windows 10](active-directory-windows-enterprise-state-roaming-windows-settings-reference.md)
