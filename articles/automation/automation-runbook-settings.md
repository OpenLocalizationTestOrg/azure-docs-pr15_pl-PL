<properties 
   pageTitle="Ustawienia działań aranżacji"
   description="W tym artykule opisano ustawienia konfiguracji dla działań aranżacji w automatyzacji Azure i zmieniania ich za pomocą portalu zarządzania Azure i programu Windows PowerShell."
   services="automation"
   documentationCenter=""
   authors="bwren"
   manager="stevenka"
   editor="tysonn" />
<tags 
   ms.service="automation"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/09/2016"
   ms.author="bwren" />

# <a name="runbook-settings"></a>Ustawienia działań aranżacji

Każdy działań aranżacji w automatyzacji Azure oferuje wiele ustawienia, które ułatwić zidentyfikowanie i zmień jego zachowanie rejestrowanie. Każde z tych ustawień poniżej opisano następuje procedury dotyczące je zmodyfikować.

## <a name="settings"></a>Ustawienia

### <a name="name-and-description"></a>Nazwa i opis

Nie można zmienić nazwy działań aranżacji, po jego utworzeniu. Opis jest opcjonalny i może zawierać maksymalnie 512 znaków.

### <a name="tags"></a>Znaczniki

Znaczniki umożliwiają przypisywanie unikatowych wyrazów i fraz w celu identyfikowania działań aranżacji. Na przykład podczas przesyłania działań aranżacji w [Galerii działań aranżacji](https://msdn.microsoft.com/library/dn781422.aspx)Określ określonego znaczników w celu identyfikowania działań aranżacji powinny być wymienione w kategorii. Kilka znaczników dla działań aranżacji można określić, rozdzielając je przecinkami.

### <a name="logging"></a>Rejestrowanie

Domyślnie postęp i pełne rekordów nie są zapisywane w historii zadań. Możesz zmienić ustawienia dla określonego działań aranżacji logowania tych rekordów. Aby uzyskać więcej informacji o tych rekordów zobacz [dane wyjściowe działań aranżacji i wiadomości](https://msdn.microsoft.com/library/dn879148.aspx).

## <a name="changing-runbook-settings"></a>Zmienianie ustawień działań aranżacji

### <a name="changing-runbook-settings-with-the-azure-management-portal"></a>Zmienianie ustawień działań aranżacji za pomocą portalu zarządzania Azure

Możesz zmienić ustawienia działań aranżacji w portalu zarządzania Azure na stronie **Konfigurowanie** dla działań aranżacji.

1. W portalu zarządzania Azure wybierz **automatyzacji** , a następnie kliknij pozycję w nazwę konta automatyzacji.
1. Wybierz kartę **Runbooks** .
1. Kliknij nazwę działań aranżacji.
1. Wybierz kartę **Konfiguruj** .

### <a name="changing-runbook-settings-with-windows-powershell"></a>Zmienianie ustawień działań aranżacji przy użyciu programu Windows PowerShell

Za pomocą polecenia cmdlet [Set-AzureAutomationRunbook](https://msdn.microsoft.com/library/dn690275.aspx) Aby zmienić ustawienia dla działań aranżacji. Jeśli chcesz określić kilka znaczników, można albo udostępnić tablica lub jeden ciąg rozdzielany przecinkami wartości parametru znaczniki. Możesz uzyskać bieżące znaczniki z [Get-AzureAutomationRunbook](https://msdn.microsoft.com/library/dn690278.aspx).

Następujące polecenia przykładowych pokazano, jak ustawić właściwości działań aranżacji. W tym przykładzie dodaje trzy istniejące znaczniki i określa, że powinny być rejestrowane pełne rekordów.

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Sample-TestRunbook"
    $tags = (Get-AzureAutomationRunbook –AutomationAccountName $automationAccountName –Name $runbookName).Tags
    $tags += "Tag1,Tag2,Tag3"
    Set-AzureAutomationRunbook –AutomationAccountName $automationAccountName –Name $runbookName –LogVerbose $true –Tags $tags

## <a name="related-articles"></a>Artykuły pokrewne
- [Dane wyjściowe działań aranżacji i wiadomości](../automation-runbook-output-and-messages) 
- [Tworzenie lub importowanie działań aranżacji](https://msdn.microsoft.com/library/dn643637.aspx) 