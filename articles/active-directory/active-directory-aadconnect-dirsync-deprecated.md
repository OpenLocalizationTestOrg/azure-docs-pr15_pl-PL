<properties
    pageTitle="Uaktualnienie DirSync i Azure AD Sync | Microsoft Azure"
    description="W tym artykule opisano, jak przeprowadzić uaktualnienie z DirSync i Azure AD Sync do Azure AD Connect."
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


# <a name="upgrade-windows-azure-active-directory-sync-dirsync-and-azure-active-directory-sync-azure-ad-sync"></a>Uaktualnianie systemu Windows Azure Active Directory synchronizacji ("DirSync") oraz synchronizacji usługi Azure Active Directory ("Azure AD Sync")
Narzędzie Azure AD Connect jest najlepszy sposób nawiązywania połączenia z usługą Azure Active Directory katalogu lokalnego i usługi Office 365. Jest to doskonałe czasu przeprowadzić uaktualnienie do Azure AD Connect z systemu Windows Azure Active Directory Sync (DirSync) lub Azure AD Sync, zgodnie z tych narzędzi są teraz przestarzałe i górny koniec pomocy technicznej na 13 kwietnia 2017.

Narzędzia synchronizacji dwóch tożsamości, które są przestarzałe były dostępne dla klientów jeden las (DirSync) oraz dla wielu las i inne zaawansowane klientów (Azure AD Sync). Te narzędzia starsze zostały zastąpione pojedyncze rozwiązanie, który jest dostępny dla wszystkich scenariuszy: narzędzie Azure AD Connect. Oferuje nowe funkcje, ulepszenia funkcji oraz obsługę nowych scenariuszy. Aby można było kontynuować synchronizacji danych tożsamości lokalnych w celu Azure AD i usługi Office 365, zalecamy uaktualnienie do Azure AD Connect.

Ostatni wersji DirSync została wydana w lipiec 2014 i ostatniej wersji Azure AD Sync została wydana w maj 2015 r.

## <a name="what-is-azure-ad-connect"></a>Co to jest Azure AD Connect
Narzędzie Azure AD Connect to nowa wersja DirSync i Azure AD Sync. Scenariusze wszystkich łączy te dwa obsługiwane. Więcej informacji o w [Integrowanie tożsamości lokalnych z usługą Azure Active Directory](active-directory-aadconnect.md).

## <a name="deprecation-schedule"></a>Oczekiwany harmonogramu

Data | Komentarz
 --- | ---
13 kwietnia 2016 | Systemu Windows Azure Active Directory Sync ("DirSync") i synchronizacji katalogów Active Microsoft Azure ("Azure AD Sync") są ogłaszane jako niezalecane.
13 kwietnia 2017 | Kończy się z pomocą techniczną. Klienci nie będzie można otworzyć przypadek pomocy technicznej bez uaktualniania do Azure AD Connect najpierw.

## <a name="how-to-transition-to-azure-ad-connect"></a>Jak przejść do Azure AD Connect
Jeśli używasz DirSync istnieją dwa sposoby można uaktualnić: Wdrażanie uaktualnienia i równoległych w miejscu. Uaktualnienie w miejscu jest zalecane dla większości klientów i czy masz ostatnią operacyjnego i mniejsza niż 50 000 obiektów. W innych przypadkach zaleca się wdrożenia równoległe miejsce, w którym konfiguracji DirSync jest przenoszony do nowego serwera uruchomione narzędzie Azure AD Connect.

Jeśli korzystasz z platformy Azure AD Sync, zaleca się uaktualnienie w miejscu. Jeśli chcesz, to można zainstalować serwer Azure AD Connect równolegle i przeprowadzić migrację zasięg na serwerze Azure AD Sync do Azure AD Connect.

Rozwiązanie | Scenariusz
----- | -----
[Uaktualnienie DirSync](./connect/active-directory-aadconnect-dirsync-upgrade-get-started.md) | <li>Jeśli masz już uruchomiony istniejący serwer DirSync.</li>
[Uaktualnienie Azure AD Sync](active-directory-aadconnect-upgrade-previous-version.md)| <li>Jeśli chcesz przenieść z Azure AD Sync.</li>

Jeśli chcesz sprawdzić, jak przeprowadzić uaktualnienie w miejscu z DirSync Azure AD Connect, następnie zobacz ten klip wideo kanału 9:

> [AZURE.VIDEO azure-active-directory-connect-in-place-upgrade-from-legacy-tools]

## <a name="faq"></a>FAQ
**P: I otrzymali powiadomienie e-mail od zespołu Azure i/lub wiadomości z Centrum wiadomości usługi Office 365, ale jest używany Połącz.**  
Powiadomienie została wysłana do klientów korzystających z Azure AD Connect o numerze kompilacji 1.0. \*.0 (przy użyciu wersji 1.1 sprzed). Firma Microsoft zaleca klientom aktualne informacje o wersjach narzędzie Azure AD Connect. Z 1.1, które funkcję [automatyczne uaktualnienie](active-directory-aadconnect-feature-automatic-upgrade.md) , spowoduje to naprawdę łatwe zawsze ma najnowszą wersję Azure AD Connect zainstalowany.

**P: będzie DirSync-Azure AD Sync zatrzymać pracy nad 13 kwietnia 2017?**  
Wartość nie. Termin po tych nie będzie mógł komunikować się z usługą Azure Active Directory zostanie ogłoszone w późniejszym terminie. Będzie można znaleźć te informacje w tym temacie, gdy są dostępne.

**P: jakie wersje DirSync czy uaktualnić pakiet?**  
Aby uaktualnić z dowolną wersją DirSync obecnie używana jest obsługiwana.

**P: jak łącznik Azure AD dla kodu FIM-z wieloma Uczestnikami?**  
Łącznik Azure AD dla kodu FIM-z wieloma Uczestnikami **nie** zostanie ogłoszone jako niezalecane. Znajduje się na **Blokowanie funkcji**; dodaje się żadne nowe funkcje i otrzyma nie poprawki. Firma Microsoft zaleca używanie go do planowania przenieść z jego Azure AD Connect klientów. Zdecydowanie zaleca się uruchomienie wszelkie nowe wdrożeń korzystania z niego. Ten łącznik zostanie ogłoszone przestarzałe w przyszłości.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Integrowanie tożsamości lokalnych z usługi Azure Active Directory](active-directory-aadconnect.md)
