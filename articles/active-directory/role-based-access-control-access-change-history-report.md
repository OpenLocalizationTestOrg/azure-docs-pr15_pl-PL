<properties
    pageTitle="Tworzenie raportu programu access Zmień Historia | Microsoft Azure"
    description="Generowanie raportu, który zawiera listę wszystkich zmian w programie access do subskrypcji Azure z oparta na rolach kontrola dostępu w ciągu ostatnich 90 dni."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="08/03/2016"
    ms.author="kgremban"/>

# <a name="create-an-access-change-history-report"></a>Tworzenie raportu programu access Zmień historii

Ilekroć ktoś udziela lub odwołuje dostęp w ramach subskrypcji, zmiany rejestrowane są Azure wydarzeń. Można tworzyć raporty historii zmian programu access, aby wyświetlić wszystkie zmiany w ciągu ostatnich 90 dni.

## <a name="create-a-report-with-azure-powershell"></a>Tworzenie raportu przy użyciu programu PowerShell Azure
Aby utworzyć raport programu access Zmień Historia w programie PowerShell, użyj `Get-AzureRMAuthorizationChangeLog` polecenia. Więcej informacji na temat tego polecenia cmdlet są dostępne w [Galerii programu PowerShell](https://www.powershellgallery.com/packages/AzureRM.Storage/1.0.6/Content/ResourceManagerStartup.ps1).

Podczas połączenia tego polecenia można określić, które właściwości przydziałów ma być widoczna wśród nich następujące:

| Właściwość | Opis |
| -------- | ----------- |
| **Akcja** | Czy udzielił lub odwołany programu access |
| **Rozmówcy** | Właściciel odpowiedzialny za Zmienianie dostępu |
| **Data** | Data i godzina modyfikacji programu access |
| **DirectoryName** | Azure Active directory |
| **PrincipalName** | Nazwa użytkownika, grupy lub aplikacji |
| **PrincipalType** | Czy była przydziału dla użytkownika, grupy lub aplikacji |
| **Ról** | Identyfikator GUID roli, która została udzielona lub odwołany |
| **RoleName** | Roli, która została udzielona lub odwołany |
| **Nazwa_zakresu** | Nazwę subskrypcji, grupa zasobów lub zasobów |
| **ScopeType** | Czy przydziału była do subskrypcji, grupa zasobów lub zakres zasobów |
| **SubscriptionId** | Identyfikator GUID Azure subskrypcji |
| **SubscriptionName** | Nazwę subskrypcji, Azure |

To polecenie przykładowe są wyświetlane wszystkie zmiany dostęp w ciągu ostatnich siedmiu dni subskrypcji:

```
Get-AzureRMAuthorizationChangeLog -StartTime ([DateTime]::Now - [TimeSpan]::FromDays(7)) | FT Caller,Action,RoleName,PrincipalType,PrincipalName,ScopeType,ScopeName
```

![Pobieranie programu PowerShell — AzureRMAuthorizationChangeLog — zrzut ekranu](./media/role-based-access-control-configure/access-change-history.png)

## <a name="create-a-report-with-azure-cli"></a>Tworzenie raportu z polecenie Azure
Aby utworzyć raport programu access Zmień Historia w Azure interfejs wiersza polecenia (polecenie), należy użyć `azure role assignment changelog list` polecenia.

## <a name="export-to-a-spreadsheet"></a>Eksportuj do arkusza kalkulacyjnego
Aby zapisać raport lub manipulować danych, wyeksportuj zmiany dostępu do pliku CSV. Następnie możesz wyświetlić raportu w arkuszu kalkulacyjnym do recenzji.

![Changelog wyświetlana jako arkusz kalkulacyjny — zrzut ekranu](./media/role-based-access-control-configure/change-history-spreadsheet.png)

## <a name="see-also"></a>Zobacz też
- Rozpoczynanie pracy z [Azure Role-Based kontroli dostępu](role-based-access-control-configure.md)
- Praca z [ról niestandardowe w Azure RBAC](role-based-access-control-custom-roles.md)
