<properties 
    pageTitle="Edytowanie runbooks tekstowy w automatyzacji Azure"
    description="Ten artykuł zawiera różne procedury dotyczące pracy z programu PowerShell i przepływ pracy programu PowerShell runbooks w automatyzacji Azure za pomocą edytora tekstowych."
    services="automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="stevenka"
    editor="tysonn" />
<tags 
    ms.service="automation"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="02/23/2016"
    ms.author="magoedte;bwren" />

# <a name="editing-textual-runbooks-in-azure-automation"></a>Edytowanie runbooks tekstowy w automatyzacji Azure

Edytor tekstowy w automatyzacji Azure umożliwia edytowanie [runbooks programu PowerShell](automation-runbook-types.md#powershell-runbooks) i [runbooks przepływu pracy programu PowerShell](automation-runbook-types.md#powershell-workflow-runbooks). To zawiera typowe cechy inne edytory kod takie jak intellisense i kodowanie za pomocą kolorów z dodatkowe funkcje specjalne ułatwiający dostęp do zasobów wspólne dla runbooks.  Ten artykuł zawiera uzyskać szczegółowe instrukcje dotyczące wykonywania różne funkcje za pomocą tego edytora.

Edytor tekstowy zawiera funkcję zostać wstawiony kod działania, zasoby i runbooks podrzędne działań aranżacji. Zamiast wpisywać w kodzie samodzielnie, możesz wybrać z listy dostępnych zasobów i mają odpowiedni kod wstawione do działań aranżacji.

Każdy działań aranżacji w automatyzacji Azure występują dwie wersje robocze i opublikowane. Edytuj wersję roboczą działań aranżacji, a potem publikować w celu mogą być wykonywane. Nie można edytować opublikowana wersja. Aby uzyskać więcej informacji, zobacz [Publikowanie działań aranżacji](automation-creating-importing-runbook.md#publishing-a-runbook) .

Aby pracować z [Runbooks graficznych](automation-runbook-types.md#graphical-runbooks), zobacz [graficznej do tworzenia w automatyzacji Azure](automation-graphical-authoring-intro.md).

## <a name="to-edit-a-runbook-with-the-azure-portal"></a>Aby edytować działań aranżacji Portal Azure

Poniższa procedura umożliwia otwieranie działań aranżacji do edycji w edytorze tekstowych.

1. W portalu usługi Azure wybierz swoje konto automatyzacji.
2. Kliknij Kafelek **Runbooks** , aby otworzyć listę runbooks.
3. Kliknij nazwę zestawu działań aranżacji, który chcesz edytować, a następnie kliknij przycisk **Edytuj** .
6. Wykonaj wymagane edycji.
7. Po zakończeniu edycji, kliknij przycisk **Zapisz** .
8. Jeśli chcesz, aby najnowszą wersję roboczą działań aranżacji publikowane, kliknij pozycję **Publikuj** .

### <a name="to-insert-a-cmdlet-into-a-runbook"></a>Aby wstawić polecenia cmdlet do działań aranżacji

2. W kanwy edytor tekstowy umieść kursor w miejsce, w którym chcesz umieścić polecenia cmdlet.
3. Rozwiń węzeł **polecenia cmdlet** w formancie biblioteki. 
3. Rozwijanie moduł zawierający polecenia cmdlet, którego chcesz użyć.
4. Kliknij prawym przyciskiem myszy polecenia cmdlet, aby wstawić i wybierz polecenie **Dodaj do obszaru roboczego**.  Jeśli polecenie cmdlet zawiera więcej niż jeden parametr, ustaw, domyślny zestaw zostaną dodane.  Możesz także rozwinąć polecenia cmdlet, aby zaznaczyć zestaw różnych parametrów.
4. Kod polecenia cmdlet dodaje się z jego całą listę parametrów.
5. Podaj odpowiednią wartość zamiast typ danych ujęte w nawiasy klamrowe <> dla wszystkich wymaganych parametrów.  Usuń wszystkie parametry, których nie potrzebujesz.

### <a name="to-insert-code-for-a-child-runbook-into-a-runbook"></a>Aby wstawić kod działań aranżacji podrzędne do działań aranżacji

2. W kanwie edytor tekstowy umieść kursor w miejsce, w którym chcesz umieścić kod [działań aranżacji podrzędne](automation-child-runbooks.md).
3. Rozwiń węzeł **Runbooks** w formancie biblioteki. 
3. Kliknij prawym przyciskiem myszy działań aranżacji do wstawienia, a następnie wybierz pozycję **Dodaj do obszaru roboczego**.
4. Z wszystkimi symbolami zastępczymi dla wszystkich parametrów działań aranżacji zostanie wstawiony kod działań aranżacji podrzędne.
5. Zamień symbole zastępcze odpowiednie wartości dla każdego parametru.

### <a name="to-insert-an-asset-into-a-runbook"></a>Aby wstawić środka trwałego w działań aranżacji

2. W Kanwa edytor tekstowy umieść kursor w miejsce, w którym chcesz umieścić kod działań aranżacji podrzędne.
3. Rozwiń węzeł **składników majątku** w formancie biblioteki. 
4. Rozwiń węzeł dla typu zawartości, który ma.
3. Kliknij prawym przyciskiem myszy elementu do wstawienia, a następnie wybierz pozycję **Dodaj do obszaru roboczego**.  Dla [zmiennych składników majątku](automation-variables.md)wybierz **Dodaj "Pobierz zmiennej" do obszaru roboczego** lub **Dodaj "Ustaw zmienną" do obszaru roboczego** w zależności od tego, czy chcesz uzyskać lub ustawić zmiennej.
4. Kod dla trwałego zostanie wstawiona do działań aranżacji.



## <a name="to-edit-a-runbook-with-the-azure-portal"></a>Aby edytować działań aranżacji Portal Azure

Poniższa procedura umożliwia otwieranie działań aranżacji do edycji w edytorze tekstowych.

1. W portalu usługi Azure wybierz **automatyzacji** , a następnie kliknij pozycję w nazwę konta automatyzacji.
2. Wybierz kartę **Runbooks** .
3. Kliknij nazwę zestawu działań aranżacji, który chcesz edytować, a następnie wybierz kartę **autora** .
5. Kliknij przycisk **Edytuj** w dolnej części ekranu.
6. Wykonaj wymagane edycji.
7. Po zakończeniu edycji, kliknij przycisk **Zapisz** .
8. Jeśli chcesz, aby najnowszą wersję roboczą działań aranżacji publikowane, kliknij pozycję **Publikuj** .

### <a name="to-insert-an-activity-into-a-runbook"></a>Aby wstawić działanie do działań aranżacji

1. W Kanwa edytor tekstowy umieść kursor w miejsce, w którym chcesz umieścić działania.
1. U dołu ekranu kliknij pozycję **Wstaw** , a następnie **aktywności**.
1. W kolumnie **Modułu integracji** wybierz moduł, zawierającą działanie.
1. W okienku **aktywności** wybierz działanie.
1. W kolumnie **Opis** Zwróć uwagę na opis działania. Opcjonalnie możesz kliknąć pozycję Wyświetl szczegółową pomoc dotyczącą uruchomić pomocy dla działania w przeglądarce.
1. Kliknij strzałkę w prawo.  Jeśli to działanie ma parametrów, zostaną wyświetlone o podanie informacji.
1. Kliknij przycisk Sprawdź.  Umożliwia uruchomienie działania kodu zostanie wstawiona do działań aranżacji.
1. Jeśli działanie wymaga parametrów, wprowadź odpowiednią wartość zamiast ujęte w nawiasy klamrowe <> typu danych.

### <a name="to-insert-code-for-a-child-runbook-into-a-runbook"></a>Aby wstawić kod działań aranżacji podrzędne do działań aranżacji

1. W kanwy edytor tekstowy umieść kursor w miejsce, w którym chcesz umieścić [działań aranżacji podrzędne](automation-child-runbooks.md).
2. U dołu ekranu kliknij pozycję **Wstaw** , a następnie **działań aranżacji**.
3. Wybierz pozycję działań aranżacji, aby wstawić z środkową kolumnę, a następnie kliknij strzałkę w prawo.
4. Jeśli działań aranżacji ma parametrów, zostaną wyświetlone o podanie informacji.
5. Kliknij przycisk Sprawdź.  Umożliwia uruchomienie zaznaczonego działań aranżacji kodu zostanie wstawiona do bieżącego działań aranżacji.
7. Jeśli działań aranżacji wymaga parametrów, wprowadź odpowiednią wartość zamiast ujęte w nawiasy klamrowe <> typu danych.

### <a name="to-insert-an-asset-into-a-runbook"></a>Aby wstawić środka trwałego w działań aranżacji

1. W kanwie edytor tekstowy umieść kursor w miejsce, w którym chcesz umieścić aktywności do pobierania elementu.
1. U dołu ekranu kliknij pozycję **Wstaw** , a następnie **Ustawienie**.
1. W kolumnie **Akcja ustawienia** wybierz akcję, która ma być.
1. Wybierz z zasobów dostępnych w kolumnie Centrum.
1. Kliknij przycisk Sprawdź.  Kod, aby uzyskać lub ustawić elementu zostanie wstawiona do działań aranżacji.



## <a name="to-edit-an-azure-automation-runbook-using-windows-powershell"></a>Aby edytować działań aranżacji automatyzacji Azure za pomocą programu Windows PowerShell

Aby edytować działań aranżacji przy użyciu programu Windows PowerShell, użyj dowolnego edytora i zapisanie go w pliku .ps1. Za pomocą polecenia cmdlet [Get-AzureAutomationRunbookDefinition](http://aka.ms/runbookauthor/cmdlet/getazurerunbookdefinition) pobrać zawartość działań aranżacji, a następnie polecenia cmdlet [Set-AzureAutomationRunbookDefinition](http://aka.ms/runbookauthor/cmdlet/setazurerunbookdefinition) do modyfikacji zastąpić istniejący działań aranżacji wersja robocza.

### <a name="to-retrieve-the-contents-of-a-runbook-using-windows-powershell"></a>Aby pobrać zawartość działań aranżacji przy użyciu programu Windows PowerShell

Następujące polecenia przykładowych pokazano, jak pobrać skrypt do działań aranżacji i zapisanie go w pliku skryptu. W tym przykładzie są pobierane wersję roboczą. Użytkownik może również pobrać opublikowana wersja zestawu działań aranżacji, mimo że nie można zmienić tej wersji.

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Sample-TestRunbook"
    $scriptPath = "c:\runbooks\Sample-TestRunbook.ps1"
    
    $runbookDefinition = Get-AzureAutomationRunbookDefinition -AutomationAccountName $automationAccountName -Name $runbookName -Slot Draft
    $runbookContent = $runbookDefinition.Content

    Out-File -InputObject $runbookContent -FilePath $scriptPath

### <a name="to-change-the-contents-of-a-runbook-using-windows-powershell"></a>Aby zmienić zawartość działań aranżacji przy użyciu programu Windows PowerShell

Następujące polecenia przykładowych pokazano, jak zastąpić istniejącą zawartość działań aranżacji zawartość pliku skryptu. Zauważ, że to jest tę samą procedurę próbki, tak jak w [celu zaimportowania działań aranżacji z pliku skryptu programu Windows PowerShell](../automation-creating-or-importing-a-runbook#ImportRunbookScriptPS).

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Sample-TestRunbook"
    $scriptPath = "c:\runbooks\Sample-TestRunbook.ps1"

    Set-AzureAutomationRunbookDefinition -AutomationAccountName $automationAccountName -Name $runbookName -Path $scriptPath -Overwrite
    Publish-AzureAutomationRunbook –AutomationAccountName $automationAccountName –Name $runbookName

## <a name="related-articles"></a>Artykuły pokrewne

- [Tworzenie lub importowanie działań aranżacji w automatyzacji Azure](automation-creating-importing-runbook.md)
- [Przepływ pracy programu PowerShell szkoleniowe](automation-powershell-workflow.md)
- [Graficzne, tworzenia w automatyzacji Azure](automation-graphical-authoring-intro.md)
- [Certyfikaty](automation-certificates.md)
- [Połączenia](automation-connections.md)
- [Poświadczenia](automation-credentials.md)
- [Harmonogramów](automation-schedules.md)
- [Zmienne](automation-variables.md)