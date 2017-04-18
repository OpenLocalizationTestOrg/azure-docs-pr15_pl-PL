<properties
    pageTitle="Azure poleceń cmdlet Podgląd Active Directory programu PowerShell do zarządzania grupy w Azure AD | Microsoft Azure"
    description="Ta strona zawiera przykłady programu PowerShell, aby ułatwić zarządzanie grup w usłudze Active Directory platformy Azure"
    keywords="Azure AD, usługi Azure Active Directory programu PowerShell, Zarządzanie grupami, grupami"
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/29/2016"
    ms.author="curtand"/>

# <a name="azure-active-directory-preview-cmdlets-for-group-management"></a>Azure poleceń cmdlet Podgląd usługi Active Directory do zarządzania grupy

> [AZURE.SELECTOR]
- [Azure portal](active-directory-groups-create-azure-portal.md)
- [Portal Azure klasyczny](active-directory-accessmanagement-manage-groups.md)
- [Programu PowerShell](active-directory-accessmanagement-groups-settings-v2-cmdlets.md)

Następujący dokument będzie dostępna z przykładami jak za pomocą programu PowerShell do zarządzania grup w usłudze Azure Active Directory (Azure AD).  Zawiera także informacje dotyczące sposobu skonfigurowania programu moduł Podgląd Azure AD programu PowerShell. Musisz najpierw [pobrać moduł Azure AD programu PowerShell](http://go.microsoft.com/fwlink/p/?LinkId=828627).

## <a name="installing-the-azure-ad-powershell-module"></a>Instalowanie modułu programu PowerShell usługi Azure AD

Aby zainstalować moduł programu AzureAD PowerShell Podgląd, należy użyć następujących poleceń:

    PS C:\Windows\system32> install-module azureadpreview

Aby sprawdzić, czy moduł Podgląd została zainstalowana, użyj następującego polecenia:

    PS C:\Windows\system32> get-module azureadpreview

    ModuleType Version    Name                                ExportedCommands
    ---------- -------    ----                                ----------------
    Binary     1.1.146.0  azureadpreview                      {Add-AzureADAdministrati...}

Teraz możesz rozpocząć korzystanie z apletów poleceń w module. Pełny opis poleceń cmdlet w module AzureAD podglądu można znaleźć w [dokumentacji online](https://msdn.microsoft.com/library/azure/mt757216.aspx).

## <a name="connecting-to-the-directory"></a>Nawiązywanie połączenia z katalogu
Przed rozpoczęciem Zarządzanie grupami za pomocą Azure AD Podgląd poleceń cmdlet umożliwia nawiązanie połączenia sesji programu PowerShell do katalogu, którą chcesz zarządzać. Aby to zrobić, użyj następującego polecenia:

    PS C:\Windows\system32> Connect-AzureAD -Force

Polecenie cmdlet wyświetli monit o poświadczenia, których chcesz używać do uzyskiwania dostępu do katalogu. W tym przykładzie użyto karen@drumkit.onmicrosoft.com uzyskać dostępu do katalogu pokaz. Polecenie cmdlet zwróci potwierdzenia pokazać sesji był pomyślnie podłączony do katalogu:

    Account                       Environment Tenant
    -------                       ----------- ------
    Karen@drumkit.onmicrosoft.com AzureCloud  85b5ff1e-0402-400c-9e3c-0f…

Teraz można uruchomić przy użyciu poleceń cmdlet Podgląd AzureAD do zarządzania grupami w katalogu.

## <a name="retrieving-groups"></a>Pobieranie grup
Aby pobrać istniejących grup z katalogu można użyć polecenia cmdlet Get-AzureADGroups. Aby pobrać wszystkie grupy w katalogu, należy użyć polecenia cmdlet bez parametrów:

    PS C:\Windows\system32> get-azureadgroup

Polecenie cmdlet zwróci wszystkie grupy w katalogu połączonego.

Aby pobrać określonej grupy, dla której należy określić identyfikator obiektu grupy, można użyć parametru - identyfikator obiektu:

    PS C:\Windows\system32> get-azureadgroup -ObjectId e29bae11-4ac0-450c-bc37-6dae8f3da61b

Polecenie cmdlet zwróci teraz grupy, których identyfikator obiektu pasują do wartości parametru wprowadzone:

    DeletionTimeStamp            :
    ObjectId                     : e29bae11-4ac0-450c-bc37-6dae8f3da61b
    ObjectType                   : Group
    Description                  :
    DirSyncEnabled               :
    DisplayName                  : Pacific NW Support
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 9bb4139b-60a1-434a-8c0d-7c1f8eee2df9
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

Możesz wyszukiwać konkretnej grupy przy użyciu parametru - filtru. Ten parametr przyjmuje klauzula filtru ODATA i zwraca wszystkie grupy, które są zgodne z filtru, tak jak w poniższym przykładzie:

    PS C:\Windows\system32> Get-AzureADGroup -Filter "DisplayName eq 'Intune Administrators'"


    DeletionTimeStamp            :
    ObjectId                     : 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df
    ObjectType                   : Group
    Description                  : Intune Administrators
    DirSyncEnabled               :
    DisplayName                  : Intune Administrators
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 4dd067a0-6515-4f23-968a-cc2ffc2eff5c
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

Należy zauważyć, że polecenia cmdlet programu AzureAD PowerShell zaimplementować standardowe zapytania OData, więcej informacji można znaleźć [w tym miejscu](https://msdn.microsoft.com/library/gg309461.aspx#BKMK_filter).

## <a name="creating-groups"></a>Tworzenie grup
Aby utworzyć nową grupę w katalogu, należy użyć polecenia cmdlet New-AzureADGroup. To polecenie cmdlet tworzy nową grupę zabezpieczeń o nazwie "Marketing":

    PS C:\Windows\system32> New-AzureADGroup -Description "Marketing" -DisplayName "Marketing" -MailEnabled $false -SecurityEnabled $true -MailNickName "Marketing"

## <a name="updating-groups"></a>Aktualizowanie grup
Aby zaktualizować istniejącej grupy, należy użyć polecenia cmdlet Set-AzureADGroup. W tym przykładzie możemy chcesz zmienić właściwość DisplayName grupy "Administratorzy Intune." Najpierw pracujemy znajdujesz grupy przy użyciu polecenia cmdlet Get-AzureADGroup i filtrować przy użyciu atrybutu DisplayName:

    PS C:\Windows\system32> Get-AzureADGroup -Filter "DisplayName eq 'Intune Administrators'"


    DeletionTimeStamp            :
    ObjectId                     : 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df
    ObjectType                   : Group
    Description                  : Intune Administrators
    DirSyncEnabled               :
    DisplayName                  : Intune Administrators
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 4dd067a0-6515-4f23-968a-cc2ffc2eff5c
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

Następnie możemy zmieniasz właściwości Opis na nową wartość "Administratorzy urządzenia Intune":

    PS C:\Windows\system32> Set-AzureADGroup -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -Description "Intune Device Administrators"

Jeśli możemy znaleźć grupy widzimy teraz właściwości Opis zostaje zaktualizowany w celu odzwierciedlenia nowej wartości:

    PS C:\Windows\system32> Get-AzureADGroup -Filter "DisplayName eq 'Intune Administrators'"


    DeletionTimeStamp            :
    ObjectId                     : 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df
    ObjectType                   : Group
    Description                  : Intune Device Administrators
    DirSyncEnabled               :
    DisplayName                  : Intune Administrators
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 4dd067a0-6515-4f23-968a-cc2ffc2eff5c
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

## <a name="deleting-groups"></a>Usuwanie grupy
Aby usunąć grupy z katalogu, należy użyć polecenia cmdlet Usuń AzureADGroup w następujący sposób:

    PS C:\Windows\system32> Remove-AzureADGroup -ObjectId b11ca53e-07cc-455d-9a89-1fe3ab24566b

## <a name="managing-members-of-groups"></a>Zarządzanie członkami grupy
Jeśli chcesz dodać nowych członków do grupy, należy użyć polecenia cmdlet AzureADGroupMember Dodaj. To polecenie dodaje członka do grupy Administratorzy Intune użytym w poprzednim przykładzie:

    PS C:\Windows\system32> Add-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -RefObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

Parametr - identyfikator obiektu jest identyfikator obiektu grupy, do której chcemy dodać członka, a RefObjectId — jest identyfikator obiektu użytkownika, którego chcemy dodać jako członka do grupy.

Aby uzyskać istniejący członkowie grupy, użyj polecenia cmdlet Get-AzureADGroupMember, jak w poniższym przykładzie:

    PS C:\Windows\system32> Get-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df

    DeletionTimeStamp ObjectId                             ObjectType
    ----------------- --------                             ----------
                        72cd4bbd-2594-40a2-935c-016f3cfeeeea User
                        8120cc36-64b4-4080-a9e8-23aa98e8b34f User

Aby usunąć członka, którego wcześniej dodano do grupy, należy użyć polecenia cmdlet Usuń AzureADGroupMember, jak pokazano poniżej:

    PS C:\Windows\system32> Remove-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -MemberId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

Aby sprawdzić membership(s) grupy użytkownika, należy użyć polecenia cmdlet wybierz AzureADGroupIdsUserIsMemberOf. To polecenie cmdlet przyjmuje jako jej parametrów identyfikator obiektu użytkownika, który chcesz sprawdzić członkostwa w grupach i listę grup, który chcesz sprawdzić członkostwa. Lista grup muszą być w formie zespolonej zmiennej typu "Microsoft.Open.AzureAD.Model.GroupIdsForMembershipCheck", więc możemy najpierw utworzyć zmienną z danym typem:

    PS C:\Windows\system32> $g = new-object Microsoft.Open.AzureAD.Model.GroupIdsForMembershipCheck

Następnie firma Microsoft udostępnia wartości dla identyfikatory GroupID sprawdzić atrybutu "Identyfikatory GroupID" tej zmiennej zespolonej:

    PS C:\Windows\system32> $g.GroupIds = "b11ca53e-07cc-455d-9a89-1fe3ab24566b", "31f1ff6c-d48c-4f8a-b2e1-abca7fd399df"

Teraz Jeśli chcemy sprawdzić członkostwa w grupach użytkownika z 72cd4bbd-2594-40a2-935c-016f3cfeeeea identyfikator obiektu względem grup w $g należy stosujemy:

    PS C:\Windows\system32> Select-AzureADGroupIdsUserIsMemberOf -ObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea -GroupIdsForMembershipCheck $g

    OdataMetadata                                                                                               Value
    -------------                                                                                               -----
    https://graph.windows.net/85b5ff1e-0402-400c-9e3c-0f9e965325d1/$metadata#Collection(Edm.String)             {31f1ff6c-d48c-4f8a-b2e1-abca7fd399df}


Wartość zwracana jest lista grup, których członkiem jest ten użytkownik. Możesz też ubiegać się ta metoda do sprawdzania członkostwa kontakty, grup lub podmiotów usługi dla danej listy grup przy użyciu AzureADGroupIdsContactIsMemberOf wybierz, wybierz pozycję AzureADGroupIdsGroupIsMemberOf lub wybierz AzureADGroupIdsServicePrincipalIsMemberOf

## <a name="managing-owners-of-groups"></a>Zarządzanie właścicieli grupy
Aby dodać właścicieli grupy, należy użyć polecenia cmdlet AzureADGroupOwner Dodaj:

    PS C:\Windows\system32> Add-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -RefObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

Parametr - identyfikator obiektu jest identyfikator obiektu grupy, do której chcemy dodać właściciela, a RefObjectId — jest identyfikator obiektu użytkownika, którego chcemy dodać jako właściciela grupy.

Aby pobrać właścicieli grupy, należy użyć Get-AzureADGroupOwner:

    PS C:\Windows\system32> Get-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df

Polecenie cmdlet zwróci listę właścicieli dla określonej grupy:

    DeletionTimeStamp ObjectId                             ObjectType
    ----------------- --------                             ----------
                        e831b3fd-77c9-49c7-9fca-de43e109ef67 User

Aby usunąć właściciela z grupy, należy użyć AzureADGroupOwner Usuń:

    PS C:\Windows\system32> remove-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -OwnerId e831b3fd-77c9-49c7-9fca-de43e109ef67

## <a name="next-steps"></a>Następne kroki

Więcej dokumentację Azure Active Directory programu PowerShell można znaleźć w [Poleceń cmdlet Azure Active Directory](http://go.microsoft.com/fwlink/p/?LinkId=808260).

* [Zarządzanie dostępem do zasobów z grupami usługi Azure Active Directory](active-directory-manage-groups.md)

* [Integrowanie tożsamości lokalnych z usługi Azure Active Directory](active-directory-aadconnect.md)
