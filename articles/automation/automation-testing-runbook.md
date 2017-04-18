<properties 
    pageTitle="Testowanie działań aranżacji w automatyzacji Azure | Microsoft Azure"
    description="Przed opublikowaniem działań aranżacji w automatyzacji Azure istnieje możliwość przetestowania, aby upewnić się, że działa zgodnie z oczekiwaniami.  W tym artykule opisano, jak do testowania działań aranżacji i wyświetlania jej wyniki."
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

# <a name="testing-a-runbook-in-azure-automation"></a>Testowanie działań aranżacji w automatyzacji Azure
Podczas testowania działań aranżacji [Wersja robocza](automation-creating-importing-runbook.md#publishing-a-runbook) jest wykonywane i wykonywane są wszystkie akcje, które wykonuje. Zostanie utworzona nie historii zadań, ale strumienie [dane wyjściowe](automation-runbook-output-and-messages.md#output-stream) i [Ostrzeżenie i błąd](automation-runbook-output-and-messages.md#message-streams) są wyświetlane w teście wyjściowy okienka. Wiadomości w [strumieniu pełne](automation-runbook-output-and-messages.md#message-streams) są wyświetlane w okienku dane wyjściowe tylko wtedy, gdy [Zmienna $VerbosePreference](automation-runbook-output-and-messages.md#preference-variables) jest ustawiona Kontynuuj.

Mimo że trwa wersję roboczą, działań aranżacji nadal są zwykle wykonuje przepływu pracy i wykonuje wszystkie akcje w odniesieniu do zasobów w środowisku. Z tego powodu można tylko test runbooks u innych niż produkcji zasobów.

Tę procedurę, aby przetestować każdego [typu działań aranżacji](automation-runbook-types.md) jest taka sama, a nie różni się do testowania między edytorze tekstowych i graficznych edytora w portalu Azure.  


## <a name="to-test-a-runbook-in-the-azure-portal"></a>Aby przetestować działań aranżacji w portalu Azure

Możesz pracować z dowolnego [typu działań aranżacji](automation-runbook-types.md) w portalu Azure.

1. Otwórz wersję roboczą działań aranżacji w [Edytor tekstowy](automation-editing-a-runbook.md#Portal) lub [graficzny edytora](automation-graphical-authoring-intro.md).
2. Kliknij przycisk **Testuj** , aby otworzyć karta Test.
3. Jeśli działań aranżacji zawiera parametry, będą one wyświetlane w okienku po lewej stronie, w której można wprowadzić wartości, które mają być używane w teście.
4. Jeśli chcesz testu na [Hybrydowych działań aranżacji pracownik](automation-hybrid-runbook-worker.md), zmienić **Ustawienia Uruchom** pracownikowi **Hybrydowych** i wybierz nazwę grupy docelowej.  W przeciwnym razie pozostaw domyślne **Azure** testu w chmurze.
5. Kliknij przycisk **Start** , aby rozpocząć test.
6. W przypadku działań aranżacji [Przepływu pracy programu PowerShell](automation-runbook-types.md#powershell-workflow-runbooks) lub [graficznej](automation-runbook-types.md#graphical-runbooks), można zatrzymać lub wstrzymywane go jest jest sprawdzany przy użyciu przycisków pod okienkiem dane wyjściowe. Po wstrzymaniu działań aranżacji wykonuje bieżących działań przed wstrzymaniu. Po działań aranżacji zostało zawieszone, możesz przestać go lub uruchom go ponownie.
7. Sprawdź, czy dane wyjściowe działań aranżacji w okienku wynik.


## <a name="next-steps"></a>Następne kroki

- Aby dowiedzieć się, jak tworzenie lub importowanie działań aranżacji, zobacz [Tworzenie lub importowanie działań aranżacji w automatyzacji Azure](automation-creating-importing-runbook.md)
- Aby dowiedzieć się więcej na temat tworzenia graficznych, zobacz [Tworzenie graficznej w automatyzacji Azure](automation-graphical-authoring-intro.md)
- Aby rozpocząć pracę z runbooks przepływu pracy programu PowerShell, zobacz [Moje pierwszego działań aranżacji przepływu pracy programu PowerShell](automation-first-runbook-textual.md)
- Aby dowiedzieć się więcej o konfigurowaniu runboks zwraca komunikaty o stanie i błędów, w tym najważniejsze wskazówki, zobacz [dane wyjściowe działań aranżacji i wiadomości w automatyzacji Azure](automation-runbook-output-and-messages.md)