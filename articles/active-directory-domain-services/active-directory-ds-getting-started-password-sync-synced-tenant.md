<properties
    pageTitle="Azure usługach domenowych AD: Włącz synchronizację hasło | Microsoft Azure"
    description="Wprowadzenie do usług domenowych Active Directory platformy Azure"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/20/2016"
    ms.author="maheshu"/>

# <a name="enable-password-synchronization-to-azure-ad-domain-services"></a>Włączanie synchronizacji haseł do usług domenowych AD Azure
W poprzednim zadania Jeśli włączono usługi Azure AD domen dla dzierżawy usługi Azure AD. Następnego zadania jest umożliwienie synchronizację haseł do usług domenowych AD Azure. Po skonfigurowaniu synchronizacji poświadczeń użytkownicy mogli logować się do zarządzanych domeny przy użyciu poświadczeń firmy.

Etapy różnią się według tego, czy Twoja organizacja ma tylko do chmury Azure AD dzierżawa lub jest ustawiony na synchronizację z katalogu lokalnego przy użyciu Azure AD Connect.

<br>

> [AZURE.SELECTOR]
- [Dzierżawa Azure AD tylko do chmury](active-directory-ds-getting-started-password-sync.md)
- [Synchronizowane dzierżawy Azure AD](active-directory-ds-getting-started-password-sync-synced-tenant.md)

<br>


## <a name="task-5-enable-password-synchronization-to-aad-domain-services-for-a-synced-azure-ad-tenant"></a>Zadanie 5: Włącz synchronizację hasło do usługi domenowe AAD zsynchronizowanej Azure AD dzierżawy
A synchronizowane Azure AD dzierżawy jest ustawiony na synchronizację z katalogu lokalnego organizacji przy użyciu Azure AD Connect. Narzędzie Azure AD Connect nie są synchronizowane NTLM i Kerberos poświadczeń mieszania do Azure AD domyślnie. Aby użyć usług domenowych AD Azure, musisz skonfigurować narzędzie Azure AD Connect do synchronizowania mieszania poświadczeń wymaganych do uwierzytelniania NTLM i Kerberos. Poniższe kroki Włącz synchronizację mieszania wymagane poświadczenia dla Twojej dzierżawy Azure AD.


### <a name="install-or-update-azure-ad-connect"></a>Instalowanie lub aktualizowanie Azure AD Connect
Najnowsza wersja release zalecane narzędzie Azure AD Connect zainstalować na komputerze przyłączonym do domeny. Jeśli masz istniejące wystąpienie Instalatora Azure AD Connect, musisz zaktualizować go, aby używać najnowszej wersji Azure AD Connect. Aby uniknąć znane problemy/usterek, które mogą mieć już naprawione, upewnij się, że zawsze używać najnowszej wersji Azure AD Connect.

**[Łączenie pobierania Azure AD](http://www.microsoft.com/download/details.aspx?id=47594)**

Zalecane wersji: **1.1.281.0** - publikowanymi 7 września 2016.

  > [AZURE.WARNING] NALEŻY zainstalować najnowszych wersji zalecane narzędzie Azure AD Connect umożliwiające poświadczeń starsze hasła (wymagany do uwierzytelniania Kerberos i NTLM) do synchronizacji do dzierżawy usługi Azure AD. Ta funkcja nie jest dostępna w poprzednich wersjach: narzędzie Azure AD Connect lub starsze narzędzia DirSync.

Instrukcje dotyczące Azure AD Connect instalacji są dostępne w artykule — [Wprowadzenie do aplikacji Narzędzie Azure AD Connect](../active-directory/active-directory-aadconnect.md)


### <a name="enable-synchronization-of-ntlm-and-kerberos-credential-hashes-to-azure-ad"></a>Włącz synchronizację NTLM i Kerberos mieszania poświadczeń, aby Azure AD
Uruchom następujący skrypt programu PowerShell na każdy las AD, aby wymusić synchronizacji haseł pełny i Włącz wszystkich użytkowników lokalnych poświadczeń mieszania synchronizację dzierżawy usługi Azure AD. Ten skrypt umożliwia mieszania poświadczenia wymagane dla uwierzytelniania Kerberos i NTLM do synchronizacji dzierżawcy usługi Azure AD.

```
$adConnector = "<CASE SENSITIVE AD CONNECTOR NAME>"  
$azureadConnector = "<CASE SENSITIVE AZURE AD CONNECTOR NAME>"  
Import-Module adsync  
$c = Get-ADSyncConnector -Name $adConnector  
$p = New-Object Microsoft.IdentityManagement.PowerShell.ObjectModel.ConfigurationParameter "Microsoft.Synchronize.ForceFullPasswordSync", String, ConnectorGlobal, $null, $null, $null
$p.Value = 1  
$c.GlobalParameters.Remove($p.Name)  
$c.GlobalParameters.Add($p)  
$c = Add-ADSyncConnector -Connector $c  
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $azureadConnector -Enable $false   
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $azureadConnector -Enable $true  
```

W zależności od rozmiaru katalogu (liczba użytkowników, grupy itp.), trwa synchronizacja mieszania poświadczeń do Azure AD. Hasła będzie możliwe w domenie usług domenowych AD Azure zarządzanych wkrótce po mieszania poświadczeń zostały zsynchronizowane na Azure AD.


<br>

## <a name="related-content"></a>Zawartość pokrewna

- [Włącz synchronizację hasło usługi domeny AAD tylko do chmury Azure AD katalogu](active-directory-ds-getting-started-password-sync.md)

- [Administrowanie Azure AD domenie usług domenowych zarządzanych](active-directory-ds-admin-guide-administer-domain.md)

- [Połączenia maszyny wirtualnej systemu Windows Azure AD domenie usług domenowych zarządzanych](active-directory-ds-admin-guide-join-windows-vm.md)

- [Dołączanie do maszyny wirtualnej Red funkcję Enterprise Linux w domenie usług domenowych AD Azure zarządzanych](active-directory-ds-admin-guide-join-rhel-linux-vm.md)
