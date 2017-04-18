<properties 
   pageTitle="Runbooks podrzędny w automatyzacji Azure | Microsoft Azure"
   description="W tym artykule opisano różne metody od w automatyzacji Azure innego działań aranżacji działań aranżacji i udostępnianie informacji między nimi."
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
   ms.date="08/17/2016"
   ms.author="magoedte;bwren" />

# <a name="child-runbooks-in-azure-automation"></a>Runbooks podrzędny w automatyzacji Azure

Jest dobrym rozwiązaniem w automatyzacji Azure napisać do wielokrotnego użytku, moduły runbooks przy użyciu osobnego funkcji, które mogą być używane przez innych runbooks. Działań aranżacji nadrzędnej często nawiąże połączenie co najmniej jeden runbooks podrzędne do wykonywania wymaganej funkcji. Istnieją dwa sposoby nawiązywania połączeń działań aranżacji podrzędne i każda ma wyraźnych różnic, które należy zrozumieć tak, aby można określić, który będzie najlepiej z różnych scenariuszach.

##  <a name="invoking-a-child-runbook-using-inline-execution"></a>Wywoływanie działań aranżacji podrzędnych za pomocą wykonanie w tekście

Aby wywołać w tekście działań aranżacji z innego działań aranżacji, możesz użyć nazwy zestawu działań aranżacji i podaj wartości dla określonych parametrów dokładnie tak, jak można użyć działania lub polecenia cmdlet.  Wszystkie runbooks z tego samego konta automatyzacji są dostępne dla wszystkich innych może być używany w ten sposób. Działań aranżacji nadrzędne czeka działań aranżacji podrzędne do wykonania przed przejściem do następnego wiersza, a wszystkie dane wyjściowe są zwracane bezpośrednio do nadrzędnego.

Po rozpoczęciu w tekście działań aranżacji jest uruchamiany w to samo zadanie jako działań aranżacji nadrzędnej. Będzie oznak w historii zadań działań aranżacji podrzędny, która działa. Wyjątki i strumieniem dane wyjściowe działań aranżacji podrzędne będą skojarzone z elementem nadrzędnym. To powoduje mniej zadań i ułatwia ich do śledzenia i rozwiązywanie problemów, ponieważ wyjątki generowane przez działań aranżacji podrzędne i wszystkich jej wyniki strumienia są skojarzone z tym zadaniem nadrzędnej.

Po opublikowaniu działań aranżacji dowolnego runbooks podrzędne, które wywołuje już musi być opublikowany. Jest to spowodowane automatyzacji Azure tworzy skojarzenie z dowolnego runbooks podrzędne podczas kompilowania działań aranżacji. Nie są działań aranżacji nadrzędnej pojawią się publikowanie prawidłowo, ale wygeneruje wyjątek po uruchomieniu. W takim przypadku możesz ponownie opublikować działań aranżacji nadrzędnej Aby poprawnie odwołanie runbooks podrzędne. Nie musisz ponownie opublikować działań aranżacji nadrzędnej, jeśli jakieś runbooks podrzędne są zostały zmienione, ponieważ skojarzenie będzie zostały już utworzone.

Parametry działań aranżacji podrzędne o nazwie w tekście może być dowolnego typu danych, w tym skomplikowanych obiektów, a istnieje nie [szeregowania JSON](automation-starting-a-runbook.md#runbook-parameters) niezmieniona podczas uruchamiania działań aranżacji za pomocą portalu zarządzania Azure lub przy użyciu polecenia cmdlet Start AzureRmAutomationRunbook.


### <a name="runbook-types"></a>Typy działań aranżacji

Jakie typy można zadzwonić do siebie:

- [Działań aranżacji programu PowerShell](automation-runbook-types.md#powershell-runbooks) i [runbooks graficzny](automation-runbook-types.md#graphical-runbooks) może wywołać każdego innych tekście (oba wyrazy są programu PowerShell podstawie).
- Runbooks [działań aranżacji przepływu pracy programu PowerShell](automation-runbook-types.md#powershell-workflow-runbooks) i graficzne programu PowerShell przepływu pracy można zadzwonić każdego innych tekście (oba wyrazy są przepływ pracy programu PowerShell podstawie)
- Typy programu PowerShell i typu przepływu pracy programu PowerShell nie połączenie wzajemnie w tekście i Rozpocznij AzureRmAutomationRunbook, należy użyć.
    
Podczas publikowania sprawy kolejności:

- Kolejność Publikuj runbooks istotne znaczenie tylko w przypadku runbooks przepływ pracy programu PowerShell i graficzne programu PowerShell przepływu pracy.


Podczas połączenia graficznej lub przepływu pracy programu PowerShell podrzędne działań aranżacji przy użyciu wykonanie w tekście, po prostu użyć nazwy działań aranżacji.  Podczas połączenia działań aranżacji programu PowerShell podrzędny musi poprzedzony nazwy z *.\\ * Aby określić, że skrypt znajduje się w katalogu lokalnego. 

### <a name="example"></a>Przykład

W poniższym przykładzie wywoływane działań aranżacji podrzędne test, który akceptuje trzy parametry, złożonych obiektu, liczba całkowita i wartością logiczną. Wynik działań aranżacji podrzędne jest przypisana do zmiennej.  W tym przypadku działań aranżacji podrzędne jest działań aranżacji przepływu pracy programu PowerShell

    $vm = Get-AzureRmVM –ResourceGroupName "LabRG" –Name "MyVM"
    $output = PSWF-ChildRunbook –VM $vm –RepeatCount 2 –Restart $true

Oto przykład samej przy użyciu programu PowerShell działań aranżacji jako element podrzędny.

    $vm = Get-AzureRmVM –ResourceGroupName "LabRG" –Name "MyVM"
    $output = .\PS-ChildRunbook –VM $vm –RepeatCount 2 –Restart $true



##  <a name="starting-a-child-runbook-using-cmdlet"></a>Rozpoczynanie działań aranżacji podrzędny przy użyciu polecenia cmdlet

Można użyć polecenia cmdlet [Start AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603661.aspx) do uruchomienia działań aranżacji zgodnie z opisem w [do uruchomienia działań aranżacji przy użyciu programu Windows PowerShell](../automation-starting-a-runbook.md#starting-a-runbook-with-windows-powershell). Istnieją dwa tryby używania dla tego polecenia cmdlet.  W jednym trybie polecenia cmdlet zwraca identyfikator zadania, zaraz po utworzeniu zadania podrzędnego dla działań aranżacji podrzędne.  W innych trybie, który zostanie włączone, określając **— Oczekiwanie** parametru polecenia cmdlet będzie czekać podrzędne zadanie kończy i zwróci wynik z działań aranżacji podrzędne.

To zadanie z działań aranżacji podrzędne wprowadzenie polecenia cmdlet będzie działać w osobnych zadania z działań aranżacji nadrzędnej. To powoduje więcej zadań niż wywoływanie tekście działań aranżacji i utrudnia ich w celu śledzenia. Element nadrzędny można uruchomić wielu runbooks podrzędne asynchroniczne nie czekając dla każdej z nich do wykonania. Dla tego samego rodzaju równoległego nawiązywania połączeń w tekście runbooks podrzędne działań aranżacji nadrzędne należy za pomocą [równoległych słowo kluczowe](automation-powershell-workflow.md#parallel-processing).

Parametry działań aranżacji podrzędne wprowadzenie polecenia cmdlet służą jako tablicę skrótów zgodnie z opisem w [Parametrach działań aranżacji](automation-starting-a-runbook.md#runbook-parameters). Można używać tylko proste typy danych. Jeśli działań aranżacji ma parametr typu złożonych danych, następnie go musi zostać wywołana w tekście.

### <a name="example"></a>Przykład

W poniższym przykładzie zaczyna się działań aranżacji podrzędne parametry, a następnie oczekuje na do wykonania za pomocą Start AzureRmAutomationRunbook-czekać parametru. Po zakończeniu, jej wyniki są zbierane od działań aranżacji podrzędne.

    $params = @{"VMName"="MyVM";"RepeatCount"=2;"Restart"=$true} 
    $joboutput = Start-AzureRmAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Test-ChildRunbook" -ResouceGroupName "LabRG" –Parameters $params –wait


## <a name="comparison-of-methods-for-calling-a-child-runbook"></a>Porównanie metod do nawiązywania połączeń z działań aranżacji podrzędny

W poniższej tabeli podsumowano różnice między dwie metody nawiązywania połączeń działań aranżacji z innego działań aranżacji.

| | W tekście| Polecenie cmdlet|
|:---|:---|:---|
|Zadanie|Runbooks podrzędne są uruchamiane w to samo zadanie jako nadrzędny.|Oddzielne zadania jest tworzony dla działań aranżacji podrzędne.|
|Wykonanie|Nadrzędny działań aranżacji oczekuje działań aranżacji podrzędny zostanie zakończony.|Działań aranżacji nadrzędnej nadal natychmiast po uruchomieniu działań aranżacji podrzędny, że działań aranżacji nadrzędnej *lub* czeka na zakończenie zadania podrzędnego.|
|Wynik|Działań aranżacji nadrzędnej bezpośrednio uzyskać dane wyjściowe z działań aranżacji podrzędne.|Działań aranżacji nadrzędnej należy pobrać danych wyjściowych z podrzędne działań aranżacji zadania *lub* działań aranżacji nadrzędnej bezpośrednio uzyskiwać dane wyjściowe na działań aranżacji podrzędne.|
|Parametry|Wartości parametrów działań aranżacji podrzędne są określane oddzielnie i użyć dowolnego typu danych.|Wartości dla parametrów działań aranżacji podrzędny musi być połączone w jednym skrótów i może zawierać tylko proste, tablicy, a typy danych obiektu tego szeregowania JSON dźwigni.|
|Konto automatyzacji|Z tego samego konta automatyzacji działań aranżacji nadrzędne można używać tylko działań aranżacji podrzędne.|Jeśli masz połączenie z tym działań aranżacji nadrzędnej można użyć działań aranżacji podrzędne z dowolnym kontem automatyzacji z tej samej subskrypcji Azure i innej subskrypcji.|
|Publikowanie|Musi być opublikowany działań aranżacji podrzędny, przed opublikowaniem działań aranżacji nadrzędnej.|Działań aranżacji podrzędny musi być opublikowany w dowolnym momencie przed rozpoczęciem działań aranżacji nadrzędnej.|

## <a name="next-steps"></a>Następne kroki

- [Rozpoczynanie działań aranżacji w automatyzacji Azure](automation-starting-a-runbook.md)
- [Dane wyjściowe działań aranżacji i wiadomości w automatyzacji Azure](automation-runbook-output-and-messages.md)
