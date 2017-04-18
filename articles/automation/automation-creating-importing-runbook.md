<properties
    pageTitle="Tworzenie lub importowanie działań aranżacji w automatyzacji Azure"
    description="Ten artykuł zawiera opis sposobu tworzenia nowych działań aranżacji w automatyzacji Azure lub importowanie z pliku."
    services="automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="jwhit"
    editor="tysonn" />
<tags
    ms.service="automation"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="09/12/2016"
    ms.author="magoedte;bwren" />

# <a name="creating-or-importing-a-runbook-in-azure-automation"></a>Tworzenie lub importowanie działań aranżacji w automatyzacji Azure

Możesz dodać działań aranżacji do automatyzacji Azure, albo [tworzenia nowej witryny](#creating-a-new-runbook) lub importując istniejących działań aranżacji z pliku lub z [Galerii działań aranżacji](automation-runbook-gallery.md). Ten artykuł zawiera informacje na temat tworzenia i importowania runbooks z pliku.  Możesz uzyskać wszystkich szczegółowych informacji na temat uzyskiwania dostępu do społeczności runbooks i moduły w [galeriach działań aranżacji i moduł automatyzacji Azure](automation-runbook-gallery.md).

## <a name="creating-a-new-runbook"></a>Tworzenie nowych działań aranżacji

Możesz utworzyć nowy działań aranżacji w automatyzacji Azure przy użyciu jednego z portali Azure lub środowiska Windows PowerShell. Po utworzeniu działań aranżacji można edytować przy użyciu informacji w [Przepływ pracy programu PowerShell szkoleń](automation-powershell-workflow.md) i [graficzne, tworzenia w automatyzacji Azure](automation-graphical-authoring-intro.md).

### <a name="to-create-a-new-azure-automation-runbook-with-the-azure-classic-portal"></a>Aby utworzyć nowy działań aranżacji automatyzacji Azure Portal Azure klasyczny

Użytkownik może pracować jedynie z [runbooks przepływu pracy programu PowerShell](automation-runbook-types.md#powershell-workflow-runbooks) w portalu Azure.

1. W portalu usługi Azure klasyczny kliknij, **Nowy**, a **aplikacji usług**, **automatyzacji**, **działań aranżacji** **Szybkie tworzenie**.
2. Wprowadź wymagane informacje, a następnie kliknij przycisk **Utwórz**. Nazwa działań aranżacji musi rozpoczynać się od litery i mogą mieć liter, cyfr, podkreślenia i kresek.
3. Jeśli chcesz edytować działań aranżacji teraz, a następnie kliknij pozycję **Edytuj działań aranżacji**. W przeciwnym razie kliknij **przycisk OK**.
4. Do nowych działań aranżacji pojawi się na karcie **Runbooks** .


### <a name="to-create-a-new-azure-automation-runbook-with-the-azure-portal"></a>Aby utworzyć nowy działań aranżacji automatyzacji Azure Portal Azure

1. W portalu Azure Otwórz konta automatyzacji.
2. Kliknij Kafelek **Runbooks** , aby otworzyć listę runbooks.
3. Kliknij przycisk **Dodaj działań aranżacji** , a następnie **Utwórz nowy działań aranżacji**.
2. Wpisz **nazwę** działań aranżacji i wybierz jego [typu](automation-runbook-types.md). Nazwa działań aranżacji musi rozpoczynać się od litery i mogą mieć liter, cyfr, podkreślenia i kresek.
3. Kliknij przycisk **Utwórz** , aby utworzyć działań aranżacji i otworzyć Edytor.


### <a name="to-create-a-new-azure-automation-runbook-with-windows-powershell"></a>Aby utworzyć nowy działań aranżacji automatyzacji Azure przy użyciu programu Windows PowerShell

Polecenia cmdlet [New-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt619376.aspx) umożliwia tworzenie pustego [działań aranżacji przepływu pracy programu PowerShell](automation-runbook-types.md#powershell-workflow-runbooks). Można określić parametr **Nazwa** w celu utworzenia pustego działań aranżacji, które można później edytować lub można określić parametr **ścieżkę** , aby zaimportować plik działań aranżacji. Parametr **typu** należy włączyć również określić jedną z czterech typów działań aranżacji.

Następujące polecenia przykładowe pokazano, jak utworzyć nowy pusty działań aranżacji.

    New-AzureRmAutomationRunbook -AutomationAccountName MyAccount `
    -Name NewRunbook -ResourceGroupName MyResourceGroup -Type PowerShell

## <a name="importing-a-runbook-from-a-file-into-azure-automation"></a>Importowanie z pliku Azure automatyzacji działań aranżacji

Możesz utworzyć nowy działań aranżacji w automatyzacji Azure, importując skrypt programu PowerShell lub przepływu pracy programu PowerShell (rozszerzenie .ps1) lub wyeksportowane graficzne działań aranżacji (.graphrunbook).  Musisz określić [Typ działań aranżacji](automation-runbook-types.md) utworzonym z importu biorąc pod uwagę następujące kwestie.

- Plik .graphrunbook można zaimportować tylko do nowych [działań aranżacji graficzne](automation-runbook-types.md#graphical-runbooks), a runbooks graficzne mogą być tworzone tylko z pliku .graphrunbook.
- Plik .ps1 zawierający przepływ pracy programu PowerShell można zaimportować tylko do [działań aranżacji przepływu pracy programu PowerShell](automation-runbook-types.md#powershell-workflow-runbooks).  Jeśli plik zawiera wiele przepływów pracy programu PowerShell, następnie importowanie zakończy się niepowodzeniem. Należy zapisać każdego przepływu pracy w osobnym pliku i importowanie każdej oddzielnie.
- Plik .ps1, który nie zawiera przepływu pracy można zaimportować do [programu PowerShell działań aranżacji](automation-runbook-types.md#powershell-runbooks) lub [działań aranżacji przepływu pracy programu PowerShell](automation-runbook-types.md#powershell-workflow-runbooks).  Po zaimportowaniu do działań aranżacji przepływu pracy programu PowerShell, następnie zostanie przekonwertowany do przepływu pracy, a komentarze zostaną uwzględnione w działań aranżacji Określanie zmian, które zostały wprowadzone.

### <a name="to-import-a-runbook-from-a-file-with-the-azure-classic-portal"></a>Aby zaimportować działań aranżacji z pliku Portal Azure klasyczny
Poniższa procedura umożliwia importowanie pliku skrypt do automatyzacji Azure.  Należy zauważyć, że można zaimportować tylko plik .ps1 do działań aranżacji przepływu pracy programu PowerShell za pomocą tego portalu.  Azure portal należy użyć dla innych typów.

1. W portalu zarządzania Azure wybierz **automatyzacji** , a następnie wybierz konto automatyzacji.
2. Kliknij przycisk **Importuj**.
3. Kliknij przycisk **Przeglądaj w poszukiwaniu pliku** , a następnie zlokalizuj plik skryptu do zaimportowania.
4. Jeśli chcesz edytować działań aranżacji teraz, a następnie kliknij pozycję **Edytuj działań aranżacji**. W przeciwnym razie kliknij przycisk OK.
5. Nowe działań aranżacji pojawią się na karcie **Runbooks** dla konta automatyzacji.
6. Należy [opublikować działań aranżacji](#publishing-a-runbook) , aby można było go uruchomić.


### <a name="to-import-a-runbook-from-a-file-with-the-azure-portal"></a>Aby zaimportować działań aranżacji z pliku Portal Azure
Poniższa procedura umożliwia importowanie pliku skrypt do automatyzacji Azure.  

>[AZURE.NOTE] Należy zauważyć, że można zaimportować tylko plik .ps1 do działań aranżacji przepływu pracy programu PowerShell za pomocą portalu.

1. W portalu Azure Otwórz konta automatyzacji.
2. Kliknij Kafelek **Runbooks** , aby otworzyć listę runbooks.
3. Kliknij przycisk **Dodaj działań aranżacji** , a następnie **Importuj**.
4. Kliknij **plik działań aranżacji** , aby zaznaczyć plik do zaimportowania
2. Jeśli pole **Nazwa** jest włączona, masz opcję, aby go zmienić.  Nazwa działań aranżacji musi rozpoczynać się od litery i mogą mieć liter, cyfr, podkreślenia i kresek.
3. [Typ działań aranżacji](automation-runbook-types.md) zostanie wybrana automatycznie, ale można zmienić typ po podjęciu stosownych ograniczeń do konta. 
3. Nowe działań aranżacji będzie wyświetlana na liście runbooks dla konta automatyzacji.
4. Należy [opublikować działań aranżacji](#publishing-a-runbook) , aby można było go uruchomić.

>[AZURE.NOTE] Po zaimportowaniu graficzne działań aranżacji lub graficzne działań aranżacji przepływu pracy programu PowerShell, użytkownik może przekonwertować na inny typ, jeśli chce. Nie można przekonwertować tekstowych.

### <a name="to-import-a-runbook-from-a-script-file-with-windows-powershell"></a>Aby zaimportować działań aranżacji z pliku skryptu programu Windows PowerShell

Polecenia cmdlet [AzureRMAutomationRunbook importowania](https://msdn.microsoft.com/library/mt603735.aspx) umożliwia importowanie pliku skryptu jako wersję roboczą działań aranżacji przepływu pracy programu PowerShell. Jeśli działań aranżacji już istnieje, importowanie zakończy się niepowodzeniem, chyba że używasz *-życie* parametru. 

Następujące polecenia przykładowych pokazano, jak zaimportować plik skryptu do działań aranżacji.

    $automationAccountName =  "AutomationAccount"
    $runbookName = "Sample_TestRunbook"
    $scriptPath = "C:\Runbooks\Sample_TestRunbook.ps1"
    $RGName = "ResourceGroup"

    Import-AzureRMAutomationRunbook -Name $runbookName -Path $scriptPath `
    -ResourceGroupName $RGName -AutomationAccountName $automationAccountName `
    -Type PowerShellWorkflow 


## <a name="publishing-a-runbook"></a>Publikowanie działań aranżacji

Podczas tworzenia lub importowania nowych działań aranżacji, musisz opublikować przed uruchomieniem go.  Każdy działań aranżacji w automatyzacji ma wersję roboczą i opublikowana wersja. Opublikowana wersja jest można używać i można edytować tylko wersję roboczą. Opublikowana wersja jest nie mają wpływu zmiany wersję roboczą. Jeśli wersja robocza powinny być udostępniane, następnie można publikować go, który zastępuje wersję opublikowaną wersję roboczą.

## <a name="to-publish-a-runbook-using-the-azure-classic-portal"></a>Aby opublikować działań aranżacji za pomocą portalu Azure klasyczny

1. Otwórz działań aranżacji w portalu Azure klasyczny.
1. W górnej części ekranu kliknij pozycję **autora**.
1. U dołu ekranu kliknij przycisk **Publikuj** , a następnie **Tak** , aby komunikat potwierdzający.

## <a name="to-publish-a-runbook-using-the-azure-portal"></a>Aby opublikować działań aranżacji za pomocą portalu Azure

1. Otwórz działań aranżacji w portalu Azure.
1. Kliknij przycisk **Edytuj** .
1. Kliknij przycisk **Publikuj** , a następnie **Tak** , aby komunikat potwierdzający.


## <a name="to-publish-a-runbook-using-windows-powershell"></a>Aby opublikować działań aranżacji przy użyciu programu Windows PowerShell

Publikowanie działań aranżacji przy użyciu programu Windows PowerShell, można użyć polecenia cmdlet [AzureRmAutomationRunbook Publikuj](https://msdn.microsoft.com/library/mt603705.aspx) . Następujące polecenia przykładowych pokazano, jak publikować działań aranżacji próbki.

    $automationAccountName =  AutomationAccount"
    $runbookName = "Sample_TestRunbook"
    $RGName = "ResourceGroup"

    Publish-AzureRmAutomationRunbook -AutomationAccountName $automationAccountName `
    -Name $runbookName -ResourceGroupName $RGName


## <a name="next-steps"></a>Następne kroki
- Aby dowiedzieć się, jak można korzystać z działań aranżacji i Galeria modułu programu PowerShell, zobacz [Galerie działań aranżacji i moduł automatyzacji Azure](automation-runbook-gallery.md)
- Aby dowiedzieć się więcej o edytowaniu programu PowerShell i przepływ pracy programu PowerShell runbooks za pomocą edytora tekstowych, zobacz [Edytowanie runbooks tekstowy w automatyzacji Azure](automation-edit-textual-runbook.md)
- Aby dowiedzieć się więcej na temat tworzenia graficznych działań aranżacji, zobacz [Tworzenie graficznej w automatyzacji Azure](automation-graphical-authoring-intro.md)
