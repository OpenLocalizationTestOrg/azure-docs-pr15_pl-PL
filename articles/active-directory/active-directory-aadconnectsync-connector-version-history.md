<properties
   pageTitle="Historia wersji wersji łącznik | Microsoft Azure"
   description="Ten temat zawiera listę wszystkich wersji programu łączników Forefront Menedżer tożsamości f i Microsoft tożsamości menedżera z wieloma Uczestnikami"
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
   ms.date="08/17/2016"
   ms.author="billmath"/>

# <a name="connector-version-release-history"></a>Historia wersji wersji łącznika
Łączniki Forefront Menedżer tożsamości f i Microsoft tożsamości menedżera z wieloma Uczestnikami są często aktualizowane.

>[AZURE.NOTE]
W tym temacie jest tylko na FIM i z wieloma Uczestnikami. Te łączniki nie są obsługiwane na narzędzie Azure AD Connect.

W tym temacie wymieniono wszystkie wersje łączniki, które zostały wydane.

Łącza pokrewne:

- [Pobierz najnowszą łączników](http://go.microsoft.com/fwlink/?LinkId=717495)
- [Rodzajowy łącznik LDAP](active-directory-aadconnectsync-connector-genericldap.md) dokumentacji
- [Rodzajowy łącznik SQL](active-directory-aadconnectsync-connector-genericsql.md) dokumentacji
- [Łącznik usług sieci Web](http://go.microsoft.com/fwlink/?LinkID=226245) dokumentacji
- [Łącznik programu PowerShell](active-directory-aadconnectsync-connector-powershell.md) dokumentacji
- Dokumentacji [Programu Lotus Domino łącznika](active-directory-aadconnectsync-connector-domino.md)

## <a name="111170"></a>1.1.117.0
Wydany: 2016 marzec

**Nowy łącznik**  
W wersji początkowej [Ogólnego łącznik SQL](active-directory-aadconnectsync-connector-genericsql.md).

**Nowe funkcje:**

- Łącznik ogólnego LDAP:
    - Dodano obsługę importowanie różnicy z Isode.
- Łącznik usług sieci Web:
    - Zaktualizowane csEntryChangeResult aktywności i aktywności setImportErrorCode umożliwia błędów poziomu obiektów ma być zwrócony aparat synchronizacji.
    - Zaktualizowane szablony SAP6 i SAP6User korzystania z nowych funkcji błędem poziomu obiektu.
- Łącznik Domino Lotus:
    - Eksportuj należy jeden certifier na książki adresowej. Za pomocą tego samego hasła dla wszystkich wydających świadectwa można teraz ułatwić zarządzanie.

**Stały problemów:**

- Łącznik ogólnego LDAP:
    - Dla IBM Tivoli DS nie wykryto poprawnie niektóre atrybuty odwołania.
    - Do obsługi protokołu LDAP otwarty podczas importowania różnicy spacje na początku i końcu ciągów obcięto.
    - Novell i NetIQ eksportu, który przeniesiony obiekt między organizacyjnych i kontenerów i jednocześnie zmienić nazwy obiektu nie powiodło się.
- Łącznik usług sieci Web:
    - Jeśli usługa sieci web ma wielu punkty końcowe w tym samym oprawienia, następnie łącznik nie poprawnie wykrył poniższe punkty końcowe.
- Łącznik Domino Lotus:
    - Eksport atrybut imię i nazwisko z bazą danych poczta nie działa.
    - Eksportowanie, które zarówno dodane i usunąć członka z grupy eksportowane tylko dodane elementy członkowskie.
    - Jeśli dokument notatek jest nieprawidłowy (isValid atrybut równa FAŁSZ), a następnie łącznik kończy się niepowodzeniem.

## <a name="older-releases"></a>Starszych wersji
Przed 2016 marca łączników opublikowano tematów pomocy technicznej.

**Rodzajowy LDAP**

- [KB3078617](https://support.microsoft.com/kb/3078617) - 1.0.0597, września 2015 r.
- [KB3044896](https://support.microsoft.com/kb/3044896) - 1.0.0549, marca 2015 r.
- [KB3031009](https://support.microsoft.com/kb/3031009) - 1.0.0534, stycznia 2015 r.
- [KB3008177](https://support.microsoft.com/kb/3008177) - 1.0.0419, września 2014 r.
- [KB2936070](https://support.microsoft.com/kb/2936070) - 4.3.1082, marzec 2014 r.

**Usługi sieci Web**

- [KB3008178](https://support.microsoft.com/kb/3008178) - 1.0.0419, września 2014 r.

**Programu PowerShell**

- [KB3008179](https://support.microsoft.com/kb/3008179) - 1.0.0419, września 2014 r.

**Lotus Domino**

- [KB3096533](https://support.microsoft.com/kb/3096533) - 1.0.0597, września 2015 r.
- [KB3044895](https://support.microsoft.com/kb/3044895) - 1.0.0549, marca 2015 r.
- [KB2977286](https://support.microsoft.com/kb/2977286) - 5.3.0712, sierpnia 2014 r.
- [KB2932635](https://support.microsoft.com/kb/2932635) - 5.3.1003, luty 2014 r.  
- [KB2899874](https://support.microsoft.com/kb/2899874) - 5.3.0721, październik 2013
- [KB2875551](https://support.microsoft.com/kb/2875551) - 5.3.0534, sierpień 2013

## <a name="next-steps"></a>Następne kroki
Dowiedz się, jak [zsynchronizować Azure AD Connect](active-directory-aadconnectsync-whatis.md) konfiguracji.

Dowiedz się więcej o [integrowaniu lokalnego tożsamości z usługą Azure Active Directory](active-directory-aadconnect.md).
