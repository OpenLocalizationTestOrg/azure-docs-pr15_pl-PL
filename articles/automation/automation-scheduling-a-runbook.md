<properties 
   pageTitle="Planowanie działań aranżacji w automatyzacji Azure | Microsoft Azure"
   description="W tym temacie opisano umożliwiające utworzenie harmonogramu w automatyzacji Azure tak, aby automatycznie rozpocząć działań aranżacji w określonym czasie lub cyklicznego zgodnie z harmonogramem."
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
   ms.date="08/05/2016"
   ms.author="bwren" />

# <a name="scheduling-a-runbook-in-azure-automation"></a>Planowanie działań aranżacji w automatyzacji Azure

Aby zaplanować działań aranżacji w automatyzacji Azure, aby rozpocząć w określonym czasie, możesz go połączyć jeden lub więcej harmonogramów. Harmonogram może być skonfigurowany do uruchomienia raz lub co pojawiał co godzinę lub codziennie harmonogramu dla runbooks w portalu klasyczny Azure i runbooks w portalu Azure, można także zaplanować je na tygodniowy, miesięczny, określone dni tygodnia lub dni miesiąca lub określony dzień miesiąca.  Działań aranżacji można połączyć z wielu harmonogramów i zgodnie z harmonogramem może mieć wiele runbooks połączone z nim.


## <a name="creating-a-schedule"></a>Tworzenie harmonogramu

Możesz utworzyć nowy harmonogram runbooks w portalu Azure w portalu klasyczny lub przy użyciu programu Windows PowerShell. Masz również możliwość tworzenia nowego harmonogramu podczas łączenia działań aranżacji do harmonogramu przy użyciu portalu Azure klasyczny lub Azure.

>[AZURE.NOTE] Po możesz skojarzyć harmonogram ze działań aranżacji automatyzacji przechowuje bieżących wersji modułów na swoim koncie i ich odwołuje się do tego harmonogramu.  Oznacza to, że jeśli wcześniej używano moduł z wersji 1.0 na koncie po utworzeniu harmonogramu i następnie zaktualizuj moduł do wersji 2.0 harmonogramu nadal będzie korzystać 1.0.  Aby można było korzystać z wersji zaktualizowanej moduł, możesz utworzyć nowy arkusz. 

### <a name="to-create-a-new-schedule-in-the-azure-classic-portal"></a>Tworzenie nowego harmonogramu w portalu klasyczny Azure

1. W portalu klasyczny Azure wybierz automatyzacji, a następnie wybierz pozycję w nazwę konta automatyzacji.
1. Wybierz kartę **zasoby** .
1. U dołu okna kliknij pozycję **Dodaj ustawienie**.
1. Kliknij przycisk **Dodaj harmonogramu**.
1. Wpisz **nazwę** i opcjonalnie **Opis** nowego harmonogramu schedule.your będzie działać **Jeden raz**, **co godzinę**, **Dzienny**, **Tygodniowy**lub **Miesięczny**.
1. Określ **Czas rozpoczęcia** i inne opcje, w zależności od typu planu wybranego.

### <a name="to-create-a-new-schedule-in-the-azure-portal"></a>Tworzenie nowego harmonogramu w portalu Azure

1. W portalu Azure, z Twojego konta automatyzacji kliknij Kafelek **elementy zawartości** , aby otworzyć karta **zasoby** .
2. Kliknij Kafelek **harmonogramów** , aby otworzyć karta **harmonogramów** .
3. Kliknij przycisk **Dodaj harmonogramu** w górnej części karta.
4. Na karta **Nowy harmonogram** wpisz **nazwę** i opcjonalnie **Opis** nowego harmonogramu.
5. Wybierz harmonogram, spowoduje uruchomienie jeden raz, czy reoccurring zgodnie z harmonogramem, wybierając **jeden raz** lub **cyklu**.  Jeśli zostanie wybrana **raz** określić **czas rozpoczęcia** , a następnie kliknij przycisk **Utwórz**.  Jeśli wybierzesz **Cykl**, określić **czas rozpoczęcia** i częstotliwość, jak często program działań aranżacji Powtarzaj - przez **godzinę**, **dzień**, **Tydzień**lub według **miesiąca**.  Po wybraniu z listy rozwijanej **Tydzień** lub **miesiąc** , **opcja cyklu** pojawi się karta według wyboru, zostanie wyświetlona karta **opcja cyklu** i możesz wybrać dzień tygodnia, w przypadku wybrania **tygodnia**.  Jeśli wybrano **miesiąca**, można wybrać według **dni tygodnia** lub określone dni miesiąca w kalendarzu, a na koniec wykonaj chcesz uruchomić ostatniego dnia miesiąca lub nie, a następnie kliknij **przycisk OK**.   

### <a name="to-create-a-new-schedule-with-windows-powershell"></a>Tworzenie nowego harmonogramu przy użyciu programu Windows PowerShell

Za pomocą [AzureAutomationSchedule nowy](http://msdn.microsoft.com/library/azure/dn690271.aspx) polecenia cmdlet, aby utworzyć nowy harmonogram w automatyzacji Azure dla runbooks klasyczny lub [Nowy AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603577.aspx) polecenia cmdlet dla runbooks w portalu Azure. Należy określić czas rozpoczęcia harmonogramu i częstotliwości, który ma być uruchamiany.

Następujące polecenia przykładowe Pokaż umożliwiające utworzenie nowego harmonogramu uruchamianej każdego dnia na 3:30 PM począwszy od 20 stycznia 2015 polecenia cmdlet zarządzania usługą Azure.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-DailySchedule"
    New-AzureAutomationSchedule –AutomationAccountName $automationAccountName –Name `
    $scheduleName –StartTime "1/20/2016 15:30:00" –DayInterval 1

Następujące polecenia przykładowych pokazano, jak umożliwiające utworzenie harmonogramu 15 i 30 dzień miesiąca przy użyciu polecenia cmdlet Azure Menedżera zasobów.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
    New-AzureRMAutomationSchedule –AutomationAccountName $automationAccountName –Name `
    $scheduleName -StartTime "7/01/2016 15:30:00" -MonthInterval 1 `
    -DaysOfMonth Fifteenth,Thirtieth -ResourceGroupName "ResourceGroup01"
    

## <a name="linking-a-schedule-to-a-runbook"></a>Łączenie harmonogramu działań aranżacji

Działań aranżacji można połączyć z wielu harmonogramów i zgodnie z harmonogramem może mieć wiele runbooks połączone z nim. Jeśli działań aranżacji zawiera parametry, można podać wartości dla nich. Należy podać wartości parametrów obowiązkowe i może dostarczyć wartości opcjonalnych parametrów.  Te wartości będą używane zawsze, gdy działań aranżacji jest uruchamiany przez tego harmonogramu.  Można dołączyć samej działań aranżacji do innego arkusza i określić wartości różnych parametrów.


### <a name="to-link-a-schedule-to-a-runbook-with-the-azure-classic-portal"></a>Aby połączyć harmonogramu działań aranżacji Portal Azure klasyczny

1. W portalu klasyczny Azure wybierz **automatyzacji** , a następnie kliknij pozycję w nazwę konta automatyzacji.
2. Wybierz kartę **Runbooks** .
3. Kliknij nazwę zestawu działań aranżacji planowania.
4. Kliknij kartę **Harmonogram** .
5. Jeśli działań aranżacji nie jest obecnie połączony z serii rozłożonych w czasie, następnie można będzie mieć możliwość **łącze do nowego harmonogramu** lub **łącza do istniejącego harmonogramu**.  Jeśli działań aranżacji jest aktualnie połączona z harmonogramu, kliknij **łącze** u dołu okna, aby uzyskać dostęp do tych opcji.
6. Jeśli działań aranżacji zawiera parametry, zostanie wyświetlony monit dla ich wartości.  

### <a name="to-link-a-schedule-to-a-runbook-with-the-azure-portal"></a>Aby połączyć harmonogramu działań aranżacji Portal Azure

1. W portalu Azure, z Twojego konta automatyzacji kliknij Kafelek **Runbooks** , aby otworzyć karta **Runbooks** .
2. Kliknij nazwę zestawu działań aranżacji planowania.
3. Jeśli działań aranżacji nie jest obecnie połączony z serii rozłożonych w czasie, następnie użytkownik otrzyma opcję, aby utworzyć nowy harmonogram lub łącza do istniejącego harmonogramu.  
4. Jeśli działań aranżacji ma parametrów, można wybrać opcję **Modyfikuj uruchamianie ustawienia (domyślny: Azure)** i karta **Parametry** są prezentowane, gdzie można wprowadzić informacje w związku z tym.  

### <a name="to-link-a-schedule-to-a-runbook-with-windows-powershell"></a>Aby połączyć harmonogramu działań aranżacji przy użyciu programu Windows PowerShell

[Rejestr AzureAutomationScheduledRunbook](http://msdn.microsoft.com/library/azure/dn690265.aspx) umożliwia łączenie harmonogramu klasyczny działań aranżacji lub polecenia cmdlet [Rejestru AzureRmAutomationScheduledRunbook](https://msdn.microsoft.com/library/mt603575.aspx) dla runbooks w portalu Azure.  Możesz określić wartości parametrów działań aranżacji z parametrem parametry. Aby uzyskać więcej informacji na temat określania wartości parametrów, zobacz [Rozpoczynanie działań aranżacji w automatyzacji Azure](automation-starting-a-runbook.md) .

Następujące polecenia przykładowych pokazano, jak utworzyć łącze harmonogramu przy użyciu polecenia cmdlet zarządzania usługą Azure z parametrami.

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Test-Runbook"
    $scheduleName = "Sample-DailySchedule"
    $params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
    Register-AzureAutomationScheduledRunbook –AutomationAccountName $automationAccountName `
    –Name $runbookName –ScheduleName $scheduleName –Parameters $params

Następujące polecenia przykładowych pokazano, jak utworzyć łącze harmonogramu działań aranżacji przy użyciu polecenia cmdlet Menedżera zasobów Azure z parametrami.

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Test-Runbook"
    $scheduleName = "Sample-DailySchedule"
    $params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
    Register-AzureRmAutomationScheduledRunbook –AutomationAccountName $automationAccountName `
    –Name $runbookName –ScheduleName $scheduleName –Parameters $params `
    -ResourceGroupName "ResourceGroup01"

## <a name="disabling-a-schedule"></a>Wyłączenie harmonogramu

Po wyłączeniu harmonogramu dowolnego runbooks połączone z nim nie będzie działać zgodnie z tym harmonogramem. Możesz ręcznie wyłączenia harmonogramu lub ustawianie czasu wygaśnięcia dla harmonogramów z częstotliwością po ich utworzeniu. Po osiągnięciu czas wygaśnięcia, harmonogram zostanie wyłączony.

### <a name="to-disable-a-schedule-from-the-azure-classic-portal"></a>Aby wyłączyć rozłożonych w czasie z poziomu portalu klasyczny Azure

Możesz wyłączyć harmonogramu w portalu klasyczny Azure na stronie szczegółów harmonogramu do harmonogramu.

1. W portalu klasyczny Azure wybierz automatyzacji, a następnie kliknij pozycję w nazwę konta automatyzacji.
1. Wybierz kartę zasoby.
1. Kliknij nazwę arkusza, aby otworzyć swoją stronę szczegółów.
2. Zmienianie **włączone** na wartość **nie**.

### <a name="to-disable-a-schedule-from-the-azure-portal"></a>Aby wyłączyć rozłożonych w czasie z poziomu portalu Azure

1. W portalu Azure, z Twojego konta automatyzacji kliknij Kafelek **elementy zawartości** , aby otworzyć karta **zasoby** .
2. Kliknij Kafelek **harmonogramów** , aby otworzyć karta **harmonogramów** .
2. Kliknij nazwę arkusza, aby otworzyć Karta Szczegóły.
3. Zmienianie **włączone** na wartość **nie**.

### <a name="to-disable-a-schedule-with-windows-powershell"></a>Aby wyłączyć harmonogram za pomocą programu Windows PowerShell

Za pomocą polecenia cmdlet [Set-AzureAutomationSchedule](http://msdn.microsoft.com/library/azure/dn690270.aspx) Aby zmienić właściwości istniejącego harmonogramu działań aranżacji klasyczny lub polecenia cmdlet [Set-AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603566.aspx) dla runbooks w portalu Azure. Aby wyłączyć harmonogram, określ **wartość false** parametru **IsEnabled** .

Następujące polecenia przykładowych pokazano, jak wyłączenia harmonogramu przy użyciu polecenia cmdlet zarządzania usługą Azure.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-DailySchedule"
    Set-AzureAutomationSchedule –AutomationAccountName $automationAccountName `
    –Name $scheduleName –IsEnabled $false

Następujące polecenia przykładowe pokazująca, jak wyłączyć harmonogram działań aranżacji przy użyciu polecenia cmdlet Azure Menedżera zasobów.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
    Set-AzureRmAutomationSchedule –AutomationAccountName $automationAccountName `
    –Name $scheduleName –IsEnabled $false -ResourceGroupName "ResourceGroup01"


## <a name="next-steps"></a>Następne kroki

- Aby uzyskać więcej informacji na temat pracy z harmonogramów, zobacz [Planowanie środkami automatyzacji Azure](http://msdn.microsoft.com/library/azure/dn940016.aspx)
- Aby rozpocząć pracę z runbooks w automatyzacji Azure, zobacz [Rozpoczynanie działań aranżacji w automatyzacji Azure](automation-starting-a-runbook.md) 