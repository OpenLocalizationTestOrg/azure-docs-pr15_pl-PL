<properties 
    pageTitle="Zarządzanie kontami usługi multimediów Azure przy użyciu programu PowerShell" 
    description="Dowiedz się, jak zarządzać kontami usługi multimediów Azure przy użyciu poleceń cmdlet programu PowerShell." 
    authors="Juliako" 
    manager="erikre" 
    editor="" 
    services="media-services" 
    documentationCenter=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/03/2016"
    ms.author="juliako"/>


#<a name="manage-azure-media-services-accounts-with-powershell"></a>Zarządzanie kontami usługi multimediów Azure przy użyciu programu PowerShell

> [AZURE.SELECTOR]
- [Portal](media-services-portal-create-account.md)
- [Programu PowerShell](media-services-manage-with-powershell.md)
- [POZOSTAŁE](http://msdn.microsoft.com/library/azure/dn194267.aspx)

> [AZURE.NOTE] Aby można było utworzyć konto usługi multimediów Azure, musi mieć konto Azure. Jeśli nie masz konta, możesz utworzyć bezpłatne konto wersji próbnej na kilka minut. Aby uzyskać szczegółowe informacje zobacz <a href="http://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A8A8397B5" target="_blank">Azure bezpłatnej wersji próbnej</a>.

##<a name="overview"></a>Omówienie 

Ten artykuł zawiera listę poleceń cmdlet Azure programu PowerShell dla usługi multimediów Azure (AMS) w ramach Azure Menedżera zasobów. Polecenia cmdlet istnieje w obszarze nazw **Microsoft.Azure.Commands.Media** .

## <a name="versions"></a>Wersje

**ApiVersion**: "2015-10-01"
               

## <a name="new-azurermmediaservice"></a>Nowy AzureRmMediaService

Tworzy usługę multimediów.

### <a name="syntax"></a>W składni

Ustawianie parametrów: StorageAccountIdParamSet

    New-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string> [-Location] <string> [-StorageAccountId] <string> [-Tags <hashtable>]  [<CommonParameters>]

Ustawianie parametrów: StorageAccountsParamSet

    New-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string> [-Location] <string> [-StorageAccounts] <PSStorageAccount[]> [-Tags <hashtable>]  [<CommonParameters>]

### <a name="parameters"></a>Parametry

**-ResourceGroupName &lt;ciąg&gt;**

Nazwa grupy zasobów, do której należy ta usługa multimediów.

Aliasy | Brak
---|---
Wymagane?   |  wartość PRAWDA.
Położenie?   |  0
Wartość domyślna |Brak
Zaakceptuj wprowadzania potok? |True(ByPropertyName)
Zaakceptuj symboli wieloznacznych?  |FAŁSZ

**Nazwa konta - &lt;ciąg&gt;**

Nazwa usługi multimediów.

Aliasy |Nazwa
---|---
Wymagane? |wartość PRAWDA.
Położenie? |1
Wartość domyślna |Brak
Zaakceptuj wprowadzania potok? |FAŁSZ
Zaakceptuj symboli wieloznacznych? |FAŁSZ

**-Lokalizacja &lt;ciąg&gt;**

Określa lokalizację zasobu usługi multimediów.

Aliasy |Brak
---|---
Wymagane? |wartość PRAWDA.
Położenie? |2
Wartość domyślna  |Brak
Zaakceptuj wprowadzania potok? |True(ByPropertyName)
Zaakceptuj symboli wieloznacznych? |FAŁSZ

**-StorageAccountId &lt;ciąg&gt;**

Określa konto podstawowy, które skojarzone z usługą multimediów.

- Nowego miejsca do magazynowania konta (utworzonego za pomocą interfejsu API Menedżera zasobów) obsługiwany tylko przez system.

- Konto miejsca do magazynowania, musi istnieć i ma tej samej lokalizacji za pomocą usługi multimediów.

Aliasy |Brak
---|---
Wymagane? |wartość PRAWDA.
Położenie? |3
Wartość domyślna  |Brak
Zaakceptuj wprowadzania potok? |True(ByPropertyName)
Nazwa zestawu parametrów |StorageAccountIdParamSet
Zaakceptuj symboli wieloznacznych?|FAŁSZ

**-StorageAccounts &lt;PSStorageAccount\[\]&gt;**

Umożliwia określenie miejsca do magazynowania kontom skojarzonego z usługą multimediów.

- Nowego miejsca do magazynowania konta (utworzonego za pomocą interfejsu API Menedżera zasobów) obsługiwany tylko przez system.

- Konto miejsca do magazynowania, musi istnieć i ma tej samej lokalizacji za pomocą usługi multimediów.

- Tylko z jednym klientem miejsca do magazynowania można określić jako podstawowy.

Aliasy |Brak
---|---
Wymagane?  |wartość PRAWDA.
Położenie?  |3
Wartość domyślna |Brak
Zaakceptuj wprowadzania potok? |True(ByPropertyName)
Nazwa zestawu parametrów |StorageAccountsParamSet
Zaakceptuj symboli wieloznacznych? |FAŁSZ

**-Znaczniki &lt;skrótów&gt;**

Umożliwia określenie tabeli mieszania znaczniki, które są skojarzone z usługą multimediów.

- Przykład:@{"tag1"="value1";"tag2"=:value2"}

Aliasy |Brak
---|---
Wymagane?  |FAŁSZ
Położenie?  |o nazwie
Wartość domyślna |Brak
Zaakceptuj wprowadzania potok? |FAŁSZ
Zaakceptuj symboli wieloznacznych? |FAŁSZ

**&lt;Parametry_polecenia&gt;**

To polecenie cmdlet obsługuje wspólne parametry:-debugowanie - ErrorAction, - ErrorVariable, - InformationAction - InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - pełne, - WarningAction i - WarningVariable.

### <a name="inputs"></a>Wartości wejściowych

Typ danych wejściowych jest typem obiektów, które można potoku do polecenia cmdlet.

### <a name="outputs"></a>Wyjściowe

Typ danych wyjściowych jest typem obiektów, które emituje polecenia cmdlet.

## <a name="set-azurermmediaservice"></a>Ustawianie AzureRmMediaService

Aktualizacje usługi multimediów.

### <a name="syntax"></a>W składni

    Set-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string> [-Tags <hashtable>] [-StorageAccounts <PSStorageAccount[]>]  [<CommonParameters>]

### <a name="parameters"></a>Parametry

**-ResourceGroupName &lt;ciąg&gt;**

Nazwa grupy zasobów, do której należy ta usługa multimediów.

Aliasy |Brak
---|---
Wymagane?  |wartość PRAWDA.
Położenie?  |0
Wartość domyślna |Brak
Zaakceptuj wprowadzania planowana? |True(ByPropertyName)
Zaakceptuj symboli wieloznacznych? |FAŁSZ

**Nazwa konta - &lt;ciąg&gt;**

Nazwa usługi multimediów.

Aliasy |Nazwa
---|---
Wymagane? |PRAWDA
Położenie? |1
Wartość domyślna |Brak
Zaakceptuj wprowadzania potok? |True(ByPropertyName)
Zaakceptuj symboli wieloznacznych? |FAŁSZ

**-StorageAccounts &lt;PSStorageAccount\[\]&gt;**

Umożliwia określenie miejsca do magazynowania kontom skojarzonego z usługą multimediów.

- Nowego miejsca do magazynowania konta (utworzonego za pomocą interfejsu API Menedżera zasobów) obsługiwany tylko przez system.

- Konto miejsca do magazynowania, musi istnieć i ma tej samej lokalizacji za pomocą usługi multimediów.

- Tylko z jednym klientem miejsca do magazynowania można określić jako podstawowy.

Aliasy |Brak
---|---
Wymagane? |FAŁSZ
Położenie? |O nazwie
Wartość domyślna |Brak
Zaakceptuj wprowadzania potok? |True(ByPropertyName)
Nazwa zestawu parametrów |StorageAccountsParamSet
Zaakceptuj symboli wieloznacznych? |FAŁSZ

**-Znaczniki &lt;skrótów&gt;**

Umożliwia określenie tabeli mieszania znaczniki, które są skojarzone z tej usługi multimediów.

- Znaczniki, które są skojarzone z usługą multimediów są zastępowane wartość określona przez klienta.

Aliasy |Brak
---|---
Wymagane? |FAŁSZ
Położenie?  |O nazwie
Wartość domyślna |Brak
Zaakceptuj wprowadzania potok? |True(ByPropertyName)
Zaakceptuj symboli wieloznacznych? |FAŁSZ

**&lt;Parametry_polecenia&gt;**

To polecenie cmdlet obsługuje wspólne parametry:-debugowanie - ErrorAction, - ErrorVariable, - InformationAction - InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - pełne, - WarningAction i - WarningVariable.

### <a name="inputs"></a>Wartości wejściowych

Typ danych wejściowych jest typem obiektów, które można potoku do polecenia cmdlet.

### <a name="outputs"></a>Wyjściowe

Typ danych wyjściowych jest typem obiektów, które emituje polecenia cmdlet.

## <a name="remove-azurermmediaservice"></a>Usuń AzureRmMediaService

Usuwa usługę multimediów.

### <a name="syntax"></a>W składni

    Remove-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string>  [<CommonParameters>]

### <a name="parameters"></a>Parametry

**-ResourceGroupName &lt;ciąg&gt;**

Nazwa grupy zasobów, do której należy ta usługa multimediów.

Aliasy |Brak
---|---
Wymagane? |wartość PRAWDA.
Położenie? |0
Wartość domyślna |Brak
Zaakceptuj wprowadzania potok? |True(ByPropertyName)
Zaakceptuj symboli wieloznacznych? |FAŁSZ

**Nazwa konta - &lt;ciąg&gt;**

Nazwa usługi multimediów.

Aliasy |Brak
---|---
Wymagane? |wartość PRAWDA.
Położenie? |2
Wartość domyślna |Brak
Zaakceptuj wprowadzania potok?  |True(ByPropertyName)
Zaakceptuj symboli wieloznacznych? |FAŁSZ

**&lt;Parametry_polecenia&gt;**

To polecenie cmdlet obsługuje wspólne parametry:-debugowanie - ErrorAction, - ErrorVariable, - InformationAction - InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - pełne, - WarningAction i - WarningVariable.

### <a name="inputs"></a>Wartości wejściowych

Typ danych wejściowych jest typem obiektów, które można potoku do polecenia cmdlet.

### <a name="outputs"></a>Wyjściowe

Typ danych wyjściowych jest typem obiektów, które emituje polecenia cmdlet.

## <a name="get-azurermmediaservice"></a>Get-AzureRmMediaService

Pobiera wszystkie usługi multimediów w grupie zasobów lub usługi multimediów o podanej nazwie.

### <a name="syntax"></a>W składni

ParameterSet: ResourceGroupParameterSet

    Get-AzureRmMediaService [-ResourceGroupName] <string>  [<CommonParameters>] 

ParameterSet: AccountNameParameterSet

    Get-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string>  [<CommonParameters>]

### <a name="parameters"></a>Parametry

**-ResourceGroupName &lt;ciąg&gt;**

Nazwa grupy zasobów, do której należy ta usługa multimediów.

Aliasy |Brak
---|---
Wymagane? |wartość PRAWDA.
Położenie?  |0
Wartość domyślna |Brak
Zaakceptuj wprowadzania potok? |True(ByPropertyName)
Nazwa zestawu parametrów |ResourceGroupParameterSet, AccountNameParameterSet
Zaakceptuj symboli wieloznacznych?   FAŁSZ

**Nazwa konta - &lt;ciąg&gt;**

Nazwa usługi multimediów.

Aliasy |Brak
---|---
Wymagane? |wartość PRAWDA.
Położenie?  |1
Wartość domyślna |Brak
Zaakceptuj wprowadzania potok? |True(ByPropertyName)
Nazwa zestawu parametrów  |AccountNameParameterSet
Zaakceptuj symboli wieloznacznych? |FAŁSZ

**&lt;Parametry_polecenia&gt;**

To polecenie cmdlet obsługuje wspólne parametry:-debugowanie - ErrorAction, - ErrorVariable, - InformationAction - InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - pełne, - WarningAction i - WarningVariable.

### <a name="inputs"></a>Wartości wejściowych

Typ danych wejściowych jest typem obiektów, które można potoku do polecenia cmdlet.

### <a name="outputs"></a>Wyjściowe

Typ danych wyjściowych jest typem obiektów, które emituje polecenia cmdlet.

## <a name="get-azurermmediaservicekeys"></a>Get-AzureRmMediaServiceKeys

Otrzymuje kluczy usługi multimediów.

### <a name="syntax"></a>W składni

    Get-AzureRmMediaServiceKeys [-ResourceGroupName] <string> [-AccountName] <string>  [<CommonParameters>]

### <a name="parameters"></a>Parametry

**-ResourceGroupName &lt;ciąg&gt;**

Nazwa grupy zasobów, do której należy ta usługa multimediów.

Aliasy |Brak
---|---
Wymagane? |wartość PRAWDA.
Położenie?  |0
Wartość domyślna |Brak
Zaakceptuj wprowadzania planowana? |True(ByPropertyName)
Zaakceptuj symboli wieloznacznych? |FAŁSZ

**Nazwa konta - &lt;ciąg&gt;**

Nazwa usługi multimediów.

Aliasy |Brak
---|---
Wymagane? |wartość PRAWDA.
Położenie? |1
Wartość domyślna |Brak
Zaakceptuj wprowadzania potok? |True(ByPropertyName)
Zaakceptuj symboli wieloznacznych? |FAŁSZ

**&lt;Parametry_polecenia&gt;**

To polecenie cmdlet obsługuje wspólne parametry:-debugowanie - ErrorAction, - ErrorVariable, - InformationAction - InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - pełne, - WarningAction i - WarningVariable.

### <a name="inputs"></a>Wartości wejściowych

Typ danych wejściowych jest typem obiektów, które można potoku do polecenia cmdlet.

### <a name="outputs"></a>Wyjściowe

Typ danych wyjściowych jest typem obiektów, które emituje polecenia cmdlet.

## <a name="set-azurermmediaservicekey"></a>Ustawianie AzureRmMediaServiceKey

Ponownie wygenerować klucz podstawowy i pomocniczy usługi multimediów.

### <a name="syntax"></a>W składni

    Set-AzureRmMediaServiceKey [-ResourceGroupName] <string> [-AccountName] <string> [-KeyType] <KeyType> {Primary | Secondary}  [<CommonParameters>]

### <a name="parameters"></a>Parametry

**-ResourceGroupName &lt;ciąg&gt;**

Nazwa grupy zasobów, do której należy ta usługa multimediów.

Aliasy |Brak
---|---
Wymagane?  |wartość PRAWDA.
Położenie?  |0
Wartość domyślna |Brak
Zaakceptuj wprowadzania potok?  |True(ByPropertyName)
Zaakceptuj symboli wieloznacznych? |FAŁSZ

**Nazwa konta - &lt;ciąg&gt;**

Nazwa usługi multimediów.

Aliasy |Brak
---|---
Wymagane? |wartość PRAWDA.
Położenie?  |1
Wartość domyślna |Brak
Zaakceptuj wprowadzania potok?   |True(ByPropertyName)
Zaakceptuj symboli wieloznacznych? |FAŁSZ

**-KeyType &lt;KeyType&gt;**

Określa typ klucza usługi multimediów.

- Podstawowy i pomocniczy

Aliasy |Brak
---|---
Wymagane?  |wartość PRAWDA.
Położenie?  |2
Wartość domyślna |Brak
Zaakceptuj wprowadzania potok? |FAŁSZ
Zaakceptuj symboli wieloznacznych? |FAŁSZ

**&lt;Parametry_polecenia&gt;**

To polecenie cmdlet obsługuje wspólne parametry:-debugowanie - ErrorAction, - ErrorVariable, - InformationAction - InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - pełne, - WarningAction i - WarningVariable.

### <a name="inputs"></a>Wartości wejściowych

Typ danych wejściowych jest typem obiektów, które można potoku do polecenia cmdlet.

### <a name="outputs"></a>Wyjściowe

Typ danych wyjściowych jest typem obiektów, które emituje polecenia cmdlet.

## <a name="sync-azurermmediaservicestoragekeys"></a>Synchronizowanie AzureRmMediaServiceStorageKeys

Synchronizowanie klawiszy konta miejsca do magazynowania dla konta magazynu skojarzonego z usługą multimediów.

### <a name="syntax"></a>W składni

    Sync-AzureRmMediaServiceStorageKeys [-ResourceGroupName] <string> [-MediaServiceAccountName] <string>    [-StorageAccountId] <string>  [<CommonParameters>]

### <a name="parameters"></a>Parametry

**-ResourceGroupName &lt;ciąg&gt;**

Nazwa grupy zasobów, do której należy ta usługa multimediów.

Aliasy |Brak
---|---
Wymagane? |wartość PRAWDA.
Położenie? |0
Wartość domyślna |Brak
Zaakceptuj wprowadzania potok? |True(ByPropertyName)
Zaakceptuj symboli wieloznacznych? |FAŁSZ

**Nazwa konta - &lt;ciąg&gt;**

Nazwa usługi multimediów.

Aliasy |Brak
---|---
Wymagane? |wartość PRAWDA.
Położenie? |1
Wartość domyślna |Brak
Zaakceptuj wprowadzania potok? |True(ByPropertyName)
Zaakceptuj symboli wieloznacznych? |FAŁSZ

**-StorageAccountId &lt;ciąg&gt;**

Określa konto miejsca do magazynowania skojarzonego z usługą multimediów.

Aliasy |Identyfikator
---|---
Wymagane? |wartość PRAWDA.
Położenie?  |2
Wartość domyślna |Brak
Zaakceptuj wprowadzania potok? |      True(ByPropertyName)
Zaakceptuj symboli wieloznacznych? |FAŁSZ

**&lt;Parametry_polecenia&gt;**

To polecenie cmdlet obsługuje wspólne parametry:-debugowanie - ErrorAction, - ErrorVariable, - InformationAction - InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - pełne, - WarningAction i - WarningVariable.

### <a name="inputs"></a>Wartości wejściowych

Typ danych wejściowych jest typem obiektów, które można potoku do polecenia cmdlet.

### <a name="outputs"></a>Wyjściowe

Typ danych wyjściowych jest typem obiektów, które emituje polecenia cmdlet.

## <a name="next-step"></a>Następny krok 

Zapoznaj się z usługi multimediów ścieżki szkoleniowe dla użytkowników.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

 
