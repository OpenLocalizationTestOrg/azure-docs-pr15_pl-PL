<properties 
   pageTitle="Typy działań aranżacji Azure automatyzacji"
   description="W tym artykule opisano różne typy runbooks używaną w automatyzacji Azure i zagadnienia, które należy wziąć pod uwagę podczas określania, którego typu użyć. "
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
   ms.author="bwren" />

# <a name="azure-automation-runbook-types"></a>Typy działań aranżacji automatyzacji Azure

Azure automatyzacji obsługuje cztery typy runbooks krótko opisane w poniższej tabeli.  W poniższych sekcjach dalszych informacji na temat każdego typu, łącznie z uwzględnieniem podczas korzystania z poszczególnych.


| Typ |  Opis |
|:---|:---|
| [Graficzne](#graphical-runbooks) | Na podstawie środowiska Windows PowerShell i utworzone i edytowane całkowicie edytora graficznego w Azure portal. | 
| [Graficzne programu PowerShell przepływu pracy](#graphical-runbooks) | Oparte na przepływu pracy programu Windows PowerShell i utworzone i edytowane całkowicie w edytorze graficznych w Azure portal. 
| [Programu PowerShell](#powershell-runbooks) | Tekst działań aranżacji według skryptu programu Windows PowerShell.
| [Przepływ pracy programu PowerShell](#powershell-workflow-runbooks) | Tekst działań aranżacji według przepływu pracy programu Windows PowerShell. |


## <a name="graphical-runbooks"></a>Graficzne runbooks

Runbooks [graficznej](automation-runbook-types.md#graphical-runbooks) i graficzne programu PowerShell przepływu pracy do tworzenia i edytowania za pomocą edytora graficznego w portalu Azure.  Można je wyeksportować do pliku, a następnie zaimportować je do innego konta automatyzacji, ale nie można utworzyć lub edytować je przy użyciu innego narzędzia.  Graficzne runbooks generowanie kodu programu PowerShell, ale nie można bezpośrednio wyświetlić lub zmodyfikować kod. Graficzne runbooks nie można przekonwertować na jeden z [formatów tekstu](automation-runbook-types.md)ani działań aranżacji tekstu można konwertować formacie graficznym. Graficzne runbooks runbooks graficzne programu PowerShell przepływu pracy można konwertować podczas importowania i na odwrót.

### <a name="advantages"></a>Zalety

- Tworzenie runbooks z minimalnymi wiedzy [programu PowerShell](automation-powershell-workflow.md).
- Wizualnego zaprezentowania procesów zarządzania.
- Dołączanie innych runbooks jako runbooks podrzędne można tworzyć wysokiego poziomu przepływy pracy.


### <a name="limitations"></a>Ograniczenia

- Nie można edytować działań aranżacji poza Azure portal.
- Może być wymagana działaniem kodu zawierającego kod programu PowerShell do wykonywania złożonych warunków logicznych.
- Nie można wyświetlać ani bezpośrednio edytować kod programu PowerShell, który jest tworzona przez graficzne przepływu pracy. Należy zauważyć, że można wyświetlać kod, który można tworzyć w dowolnej działania kodu.


## <a name="powershell-runbooks"></a>Runbooks programu PowerShell

Runbooks programu PowerShell są oparte na programie Windows PowerShell.  Możesz edytować bezpośrednio kod działań aranżacji przy użyciu edytora tekstu w portalu Azure.  Do automatyzacji Azure, można użyć dowolnego edytora tekstu w trybie offline i [zaimportować działań aranżacji](http://msdn.microsoft.com/library/azure/dn643637.aspx) .

### <a name="advantages"></a>Zalety

- Wdrożenie wszystkich logiczny złożonych z kodem programu PowerShell bez dodatkowych złożoności przepływu pracy programu PowerShell. 
- Działań aranżacji zaczyna się szybciej niż runbooks graficznej lub przepływu pracy programu PowerShell, ponieważ nie musi być tworzone przed uruchomieniem.

### <a name="limitations"></a>Ograniczenia

- Musisz znać skryptów programu PowerShell.
- Nie można używać [Przetwarzanie równoległe](automation-powershell-workflow.md#parallel-processing) do wykonywania wielu akcji równolegle.
- Nie można używać [punktów kontrolnych](automation-powershell-workflow.md#checkpoints) wznowić działań aranżacji w przypadku błędu.
- Runbooks przepływu pracy programu PowerShell i runbooks graficzny może zawierać tylko dołączone jako podrzędne runbooks przy użyciu polecenia cmdlet Start AzureAutomationRunbook, co spowoduje utworzenie nowego zadania.

### <a name="known-issues"></a>Znane problemy
Poniżej przedstawiono bieżącej znane problemy dotyczące programu PowerShell runbooks.

- Runbooks programu PowerShell nie nie można pobrać niezaszyfrowanym [trwałego zmiennych](automation-variables.md) o wartości null.
- Runbooks programu PowerShell nie można pobrać [zmiennej zawartości](automation-variables.md) z *~* w nazwie.
- Get-Process w pętli w programie PowerShell działań aranżacji może ulec awarii po około 80 iteracji. 
- Działań aranżacji programu PowerShell może zakończyć się niepowodzeniem, jeśli próbuje bardzo dużych ilości danych do zapisu strumienia wyjściowego jednocześnie.   Przez wyprowadzania tylko informacje potrzebne podczas pracy z obiektami dużych można zazwyczaj obejścia tego problemu.  Na przykład zamiast wyprowadzania podobną *Get-Process*, można wyprowadzania tylko wymagane pola z *Get-Process | Wybierz parametr, Procesor*.

## <a name="powershell-workflow-runbooks"></a>Runbooks przepływu pracy programu PowerShell

Przepływ pracy programu PowerShell runbooks są runbooks tekst według [Przepływu pracy programu Windows PowerShell](automation-powershell-workflow.md).  Możesz edytować bezpośrednio kod działań aranżacji przy użyciu edytora tekstu w portalu Azure.  Do automatyzacji Azure, można użyć dowolnego edytora tekstu w trybie offline i [zaimportować działań aranżacji](http://msdn.microsoft.com/library/azure/dn643637.aspx) .

### <a name="advantages"></a>Zalety

- Wdrożenie wszystkich logiczny złożonych z kodem przepływu pracy programu PowerShell.
- Aby wznowić działań aranżacji w przypadku błędu za pomocą [punktów kontrolnych](automation-powershell-workflow.md#checkpoints) .
- [Przetwarzanie równoległe](automation-powershell-workflow.md#parallel-processing) umożliwia wykonywanie wielu akcji równolegle.
- Mogą zawierać inne graficzne runbooks i runbooks przepływu pracy programu PowerShell jako runbooks podrzędne można tworzyć wysokiego poziomu przepływy pracy.


### <a name="limitations"></a>Ograniczenia

- Autor jest znajomość przepływu pracy programu PowerShell.
- Działań aranżacji musi dotyczyć dodatkowe złożoność przepływu pracy programu PowerShell, takich jak [rozszeregować obiektów](automation-powershell-workflow.md#code-changes).
- Działań aranżacji trwa dłużej, aby rozpocząć niż runbooks programu PowerShell, który musi zostać skompilowany przed uruchomieniem.
- Runbooks programu PowerShell można dołączyć jako runbooks podrzędne tylko za pomocą polecenia cmdlet Start AzureAutomationRunbook, co spowoduje utworzenie nowego zadania.


## <a name="considerations"></a>Zagadnienia dotyczące

Należy wziąć pod uwagę następujące kwestie dodatkowe podczas określania, którego typu użyć dla określonego działań aranżacji.

- Nie można przekonwertować typu tekstowych lub odwrotnie runbooks z graficznego.
- Istnieją pewne ograniczenia przy użyciu runbooks różnych typów jako działań aranżacji podrzędne.  Aby uzyskać więcej informacji, zobacz [runbooks podrzędny w automatyzacji Azure](automation-child-runbooks.md) .

  
## <a name="next-steps"></a>Następne kroki

- Aby dowiedzieć się więcej na temat tworzenia graficznych działań aranżacji, zobacz [Tworzenie graficznej w automatyzacji Azure](automation-graphical-authoring-intro.md)
- Aby poznać różnice między programu PowerShell i programu PowerShell przepływy pracy dla runbooks, zobacz [Przepływ pracy programu PowerShell Windows szkoleń](automation-powershell-workflow.md)
- Aby uzyskać więcej informacji na temat tworzenia lub importowania działań aranżacji zobacz [Tworzenie lub importowanie działań aranżacji](automation-creating-importing-runbook.md)



