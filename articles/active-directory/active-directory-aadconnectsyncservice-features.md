<properties
    pageTitle="Funkcje usługi synchronizacji usługi Azure AD Connect i konfiguracji | Microsoft Azure"
    description="Omówienie funkcji po stronie usługi Azure AD Connect synchronizacji usługi."
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
    ms.date="08/22/2016"
    ms.author="andkjell;markvi"/>

# <a name="azure-ad-connect-sync-service-features"></a>Funkcje usługi synchronizacji usługi Azure AD Connect

Funkcja synchronizacji Azure AD Connect ma dwa składniki:

- Składnik lokalnego o nazwie **Narzędzie Azure AD Connect synchronizacji**, nazywany **aparatu synchronizacji**.
- Usługa znajdujących się w Azure AD nazywane także **usługi synchronizacji Azure AD Connect**

W tym temacie wyjaśniono, jak działają następujące funkcje **usługi synchronizacji Azure AD Connect** i jak można skonfigurować za pomocą programu Windows PowerShell.

Te ustawienia są konfigurowane przez [Azure Active Directory modułu dla programu Windows PowerShell](http://aka.ms/aadposh). Pobierz i zainstaluj je niezależnie od Azure AD Connect. Polecenia cmdlet, opisane w niniejszym dokumencie zostały wprowadzone w [2016 marca Zwolnij (kompilacja 9031.1)](http://social.technet.microsoft.com/wiki/contents/articles/28552.microsoft-azure-active-directory-powershell-module-version-release-history.aspx#Version_9031_1). Jeśli nie masz cmdlet opisane w niniejszym dokumencie lub ich nie daje taki sam wynik, upewnij się, że uruchomić najnowszą wersję.

Aby wyświetlić konfigurację w katalogu Azure AD, uruchom `Get-MsolDirSyncFeatures`.  
![Wynik Get-MsolDirSyncFeatures](./media/active-directory-aadconnectsyncservice-features/getmsoldirsyncfeatures.png)

Wiele z tych ustawień można zmienić tylko przez narzędzie Azure AD Connect.

Następujące ustawienia można skonfigurować za `Set-MsolDirSyncFeature`:

DirSyncFeature | Komentarz
--- | ---
[DuplicateProxyAddressResiliency<br/>DuplicateUPNResiliency](#duplicate-attribute-resiliency) | Umożliwia atrybut do kwarantannie, gdy jest to duplikat innego obiektu, a nie awarii całego obiektu podczas eksportowania.
[EnableSoftMatchOnUpn](#userprincipalname-soft-match) | Pozwala obiektów sprzężenia userPrincipalName oprócz główny adres SMTP.
[SynchronizeUpnForManagedUsers](#synchronize-userprincipalname-updates) | Umożliwia aparat synchronizacji zaktualizować atrybut userPrincipalName zarządzane licencjonowanych użytkowników (niefederacyjnej).

Po włączeniu funkcji nie można wyłączyć ponownie.

>[AZURE.NOTE] Z 24 sierpnia 2016 funkcję *duplikat elastyczność atrybut* jest domyślnie dla nowych Azure AD katalogów. Ta funkcja będzie również wdrażania i włączone katalogów utworzone przed tą datą. Otrzymasz powiadomienie e-mail, gdy katalogu ma uzyskać ta funkcja jest włączona.

Poniższe ustawienia są konfigurowane przez narzędzie Azure AD Connect i nie można zmodyfikować przez `Set-MsolDirSyncFeature`:

DirSyncFeature | Komentarz
--- | ---
DeviceWriteback | [Narzędzie Azure AD Connect: Włączanie zapisu urządzenia](active-directory-aadconnect-feature-device-writeback.md)
DirectoryExtensions | [Azure AD Connect synchronizacją: rozszerzenia katalogu](active-directory-aadconnectsync-feature-directory-extensions.md)
PasswordSync | [Implementowania synchronizacji haseł z synchronizacją Azure AD Connect](active-directory-aadconnectsync-implement-password-synchronization.md)
UnifiedGroupWriteback | [Podgląd: Grupa zapisu](active-directory-aadconnect-feature-preview.md#group-writeback)
UserWriteback | Nie są obecnie obsługiwane.

## <a name="duplicate-attribute-resiliency"></a>Atrybut zduplikowany elastyczności
Zamiast błędu udzielenia obiekty z zduplikowanych UPN / proxyAddresses, zduplikowany atrybut jest "kwarantannie" i przypisano tymczasowa wartość. Gdy konfliktu zostanie rozwiązany, tymczasowe UPN zostanie automatycznie zmieniona wartość pisane z wielkiej litery. Ten problem można włączyć dla głównej nazwy użytkownika i proxyAddress oddzielnie. Aby uzyskać więcej informacji zobacz [Synchronizacja tożsamości i elastyczność atrybut zduplikowany](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md).

## <a name="userprincipalname-soft-match"></a>Uwzględnij wygładzone UserPrincipalName
Jeśli ta funkcja jest włączona, wygładzone dopasowanie jest włączona dla głównej nazwy użytkownika oprócz [główny adres SMTP](https://support.microsoft.com/kb/2641663), który jest zawsze włączone. Uwzględnij wygładzone jest używany do dopasowywania istniejących użytkowników w chmurze w Azure AD użytkownikom lokalnego.

Jeśli musisz dopasowanie lokalnych kont AD z istniejącego konta utworzone w chmurze i nie używasz usługi Exchange Online, a następnie ta funkcja jest przydatna. W tym scenariuszu ogólnie nie masz powodu atrybut SMTP w chmurze.

Ta funkcja jest na domyślnie dla nowo utworzony Azure AD katalogów. Można wyświetlić, jeśli ta funkcja jest włączona dla Ciebie, uruchamiając:  
```
Get-MsolDirSyncFeatures -Feature EnableSoftMatchOnUpn
```

Jeśli ta funkcja nie jest włączona funkcja katalogu Azure AD, następnie możesz ją włączyć, uruchamiając:  
```
Set-MsolDirSyncFeature -Feature EnableSoftMatchOnUpn -Enable $true
```

## <a name="synchronize-userprincipalname-updates"></a>Synchronizowanie userPrincipalName aktualizacji
Ze względów historycznych aktualizacje atrybutu UserPrincipalName za pomocą usługi synchronizacji ze źródeł lokalnych zostało zablokowane, o ile nie są spełnione oba poniższe warunki:

- Użytkownik odbywa się (niefederacyjnej).
- Użytkownik nie przypisano licencji.

Aby uzyskać więcej informacji zobacz [nazwy użytkowników w usłudze Office 365, platformy Azure lub usługi nie są zgodne lokalnej głównej lub identyfikator logowania alternatywny](https://support.microsoft.com/kb/2523192).

Włączenie tej funkcji umożliwia aparat synchronizacji, aby zaktualizować userPrincipalName, gdy jest zmienione lokalnego i używanie synchronizacji haseł. Jeśli używasz Federacji, ta funkcja nie jest obsługiwana.

Ta funkcja jest na domyślnie dla nowo utworzony Azure AD katalogów. Można wyświetlić, jeśli ta funkcja jest włączona dla Ciebie, uruchamiając:  
```
Get-MsolDirSyncFeatures -Feature SynchronizeUpnForManagedUsers
```

Jeśli ta funkcja nie jest włączona funkcja katalogu Azure AD, następnie możesz ją włączyć, uruchamiając:  
```
Set-MsolDirSyncFeature -Feature SynchronizeUpnForManagedUsers -Enable $true
```

Po włączeniu tej funkcji, pozostanie istniejących wartości userPrincipalName-jest. Dla następnej zmiany userPrincipalName atrybut lokalne synchronizacji normalny różnicy użytkowników zaktualizuje UPN.  

## <a name="see-also"></a>Zobacz też

- [Synchronizowanie Azure AD Connect](active-directory-aadconnectsync-whatis.md)
- [Integrowanie tożsamości lokalnych z usługą Azure Active Directory](active-directory-aadconnect.md).
