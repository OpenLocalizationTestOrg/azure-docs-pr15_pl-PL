<properties 
   pageTitle="Przepływ pracy programu PowerShell nauki"
   description="Ten artykuł jest przeznaczony jako szybkie lekcji do autorów znanych z programu PowerShell opis określonych różnic między programu PowerShell i przepływ pracy programu PowerShell."
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

# <a name="learning-windows-powershell-workflow"></a>Przepływ pracy programu PowerShell Windows szkoleniowe

Runbooks w automatyzacji Azure są wykonywane jako przepływy pracy programu Windows PowerShell.  Przepływ pracy programu PowerShell systemu Windows jest podobna do skryptu programu Windows PowerShell, ale zawiera kilka istotnych różnic, które może być trudne do nowego użytkownika.  Ten artykuł jest przeznaczony dla użytkowników zna programu PowerShell i krótko opisano pojęcia jest wymagane, jeśli chcesz przekonwertować skrypt programu PowerShell do przepływu pracy programu PowerShell do użycia w działań aranżacji.  

Przepływ pracy jest sekwencji etapów zaprogramowanych, połączone, wykonywanie zadań długim lub wymagają można je zsynchronizować wiele kroków w wielu urządzeń lub węzły zarządzane. Przepływ pracy nad skrypt normalny zalety możliwość jednoczesnego wykonywać akcję przed wielu urządzeń i możliwości automatyczne odzyskiwanie z błędy. Przepływ pracy programu PowerShell systemu Windows jest skrypt programu Windows PowerShell, który korzysta z programu Windows Workflow Foundation. Gdy przepływ pracy jest napisane za pomocą programu Windows PowerShell składni i realizuje programu Windows PowerShell, jest przetwarzana przez Windows Workflow Foundation.

Aby uzyskać pełne informacje na tematy w tym artykule zobacz [Wprowadzenie do przepływów pracy programu Windows PowerShell](http://technet.microsoft.com/library/jj134242.aspx).

## <a name="types-of-runbook"></a>Typy działań aranżacji

Istnieją trzy typy działań aranżacji w automatyzacji Azure, *Przepływ pracy programu PowerShell*, *programu PowerShell* i *graficzne*.  Typ działań aranżacji po utworzeniu działań aranżacji i działań aranżacji nie można przekonwertować na inny typ po jego utworzeniu.

Runbooks przepływu pracy programu PowerShell i runbooks programu PowerShell dla użytkowników, którzy nie chce korzystać bezpośrednio z kodem programu PowerShell albo używają tekstowy edytora w automatyzacji Azure lub edytora trybu offline, takich jak środowiska PowerShell ISE. Jeśli tworzysz działań aranżacji przepływu pracy programu PowerShell zapoznaj się informacje w tym artykule. 

Graficzne runbooks umożliwiają tworzenie działań aranżacji przy użyciu tego samego działania i polecenia cmdlet, ale przy użyciu graficznego interfejsu, który spowoduje ukrycie złożoności źródłowych przepływu pracy programu PowerShell.  Pojęcia w tym artykule, takie jak punkty kontrolne i równoległego są nadal stosowane do runbooks graficzne, ale nie musisz martwić szczegółowe informacje o składni. 

## <a name="basic-structure-of-a-workflow"></a>Podstawowa struktura przepływu pracy

Pierwszy krok do konwertowania skrypt programu PowerShell przepływ pracy programu PowerShell jest umieszczając go ze słowem kluczowym **przepływu pracy** .  Przepływ pracy jest uruchamiany ze słowem kluczowym **przepływu pracy** następuje treści skrypt ujęte w nawiasy klamrowe. Nazwa przepływu pracy wykonuje słowo kluczowe **przepływu pracy** , jak pokazano w następującej składni. 

    Workflow Test-Workflow
    {
       <Commands>
    }

Nazwa przepływu pracy musi odpowiadać nazwę zestawu działań aranżacji automatyzacji. Jeśli importowania działań aranżacji pliku musi być zgodna z nazwą przepływu pracy i musi mieć .ps1.

Aby dodać parametry do przepływu pracy, należy użyć słowa kluczowego **parametrów** , tak jak do skryptu. 

## <a name="code-changes"></a>Zmiany kodu

Kod przepływu pracy programu PowerShell wygląda prawie taka sama, jak kod skryptu programu PowerShell z wyjątkiem kilku istotne zmiany.  W poniższych sekcjach opisano zmiany, które należy wprowadzić skrypt programu PowerShell dla niego do uruchamiania w przepływie pracy.

### <a name="activities"></a>Działania

Działanie to określonego zadania w przepływie pracy. Tak, jak skrypt składa się z jednego lub więcej poleceń, przepływ pracy składa się z co najmniej jeden działania, które są wykonywane w sekwencji. Przepływ pracy programu Windows PowerShell automatycznie konwertuje liczby poleceń cmdlet programu Windows PowerShell działania podczas uruchamiania przepływu pracy. Po określeniu jednej z tych poleceń cmdlet w swojej działań aranżacji odpowiednie działanie rzeczywiście jest uruchamiany przez Windows Workflow Foundation. Dla tych poleceń cmdlet bez odpowiednie działanie przepływu pracy programu Windows PowerShell automatyczne uruchomienie polecenia cmdlet działania [InlineScript](#inlinescript) . Istnieje zestaw poleceń cmdlet, które są wyłączone i nie można używać w przepływie pracy, chyba że wyraźnie je uwzględnić w bloku InlineScript. Szczegółowe informacje o tych pojęć zobacz [Przy użyciu działań w przepływach pracy skrypt](http://technet.microsoft.com/library/jj574194.aspx).

Działania przepływu pracy mają pewien zestaw typowych parametrów w celu skonfigurowania ich działania. Aby uzyskać szczegółowe informacje o typowych parametrach przepływu pracy zobacz [about_WorkflowCommonParameters](http://technet.microsoft.com/library/jj129719.aspx).

### <a name="positional-parameters"></a>Parametry pozycyjne

Nie można używać parametrów pozycyjne działania i poleceń cmdlet w przepływie pracy.  Oznacza to to, że należy użyć nazwy parametrów.

Na przykład można rozważyć następujący kod, który pobiera wszystkie uruchomione usługi.

     Get-Service | Where-Object {$_.Status -eq "Running"}

Jeśli próbujesz uruchomić ten sam kod w przepływie pracy, zostanie wyświetlony komunikat informujący o tym, jak "zestaw parametrów nie można rozwiązać przy użyciu określonych parametrów nazwanych."  Aby rozwiązać ten problem, wystarczy podać nazwę parametru w następującej.

    Workflow Get-RunningServices
    {
        Get-Service | Where-Object -FilterScript {$_.Status -eq "Running"}
    }

### <a name="deserialized-objects"></a>Rozszeregowanym obiektów

Obiekty w przepływach pracy są rozszeregować.  Oznacza to, że ich właściwości są nadal dostępne, ale nie ich metod.  Na przykład można rozważyć następujący kod programu PowerShell, który zatrzymuje usługę przy użyciu metody Zatrzymaj obiektu usługi.

    $Service = Get-Service -Name MyService
    $Service.Stop()

Jeśli próbujesz uruchomić to w przepływie pracy, zostanie wyświetlony komunikat o błędzie informujący "wywołania metody nie jest obsługiwane w przepływie pracy Windows PowerShell".  

Jedną z opcji jest, aby te dwa wiersze kodu w bloku [InlineScript](#InlineScript) w takim przypadku $Service jest obiektem usługi w bloku. 

    Workflow Stop-Service
    {
        InlineScript {
            $Service = Get-Service -Name MyService
            $Service.Stop()
        }
    } 

Innym rozwiązaniem jest polecenie cmdlet innego wykonujące te same funkcje co metody, jeśli jest dostępny.  W przypadku próby polecenia cmdlet usługi ciągłej zawiera te same funkcje co Metoda Stop, a przepływ pracy można użyć następujących czynności.

    Workflow Stop-MyService
    {
        $Service = Get-Service -Name MyService
        Stop-Service -Name $Service.Name
    }


## <a name="inlinescript"></a>InlineScript

Działanie **InlineScript** jest przydatny, gdy jest potrzebne do uruchamiania jedno lub więcej poleceń jako tradycyjny skrypt programu PowerShell zamiast przepływu pracy programu PowerShell.  Podczas poleceń w przepływie pracy są wysyłane do programu Windows Workflow Foundation przetwarzania, polecenia w bloku InlineScript są wykonywane przez programu Windows PowerShell. 

InlineScript używa składni, jak pokazano poniżej.

    InlineScript
    {
      <Script Block>
    } <Common Parameters>

Dane wyjściowe można przywrócić z InlineScript przydzielenie wyniku zmiennej. W poniższym przykładzie zatrzymuje usługę, a następnie wyświetla nazwę usługi.

    Workflow Stop-MyService
    {
        $Output = InlineScript {
            $Service = Get-Service -Name MyService
            $Service.Stop()
            $Service
        }

        $Output.Name
    }


Wartości można przekazać do bloku InlineScript, ale należy użyć modyfikujący zakres **$Using** .  Poniższy przykład jest identyczny z poprzedniego przykładu, ale nazwa usługi znajduje się za pomocą zmiennej. 

    Workflow Stop-MyService
    {
        $ServiceName = "MyService"
    
        $Output = InlineScript {
            $Service = Get-Service -Name $Using:ServiceName
            $Service.Stop()
            $Service
        }

        $Output.Name
    }


Działania InlineScript może być krytyczne w określonych kolejek czynności, nie obsługuje przepływu pracy konstrukcji i powinno być używane tylko w razie potrzeby z następujących powodów:

- Nie można używać [punktów kontrolnych](#Checkpoints) wewnątrz bloku InlineScript. Jeśli w przypadku wystąpienia błędu w bloku musi wznowieniu na początek bloku.
- Nie można używać [równoległego](#parallel-execution) wewnątrz InlineScriptBlock.
- InlineScript ma wpływ na skalowalność przepływu pracy, ponieważ przechowuje sesji programu Windows PowerShell dla całej długości bloku InlineScript.

Aby uzyskać szczegółowe informacje na temat korzystania z InlineScript zobacz [Uruchamianie poleceń programu Windows PowerShell w przepływie pracy](http://technet.microsoft.com/library/jj574197.aspx) i [about_InlineScript](http://technet.microsoft.com/library/jj649082.aspx).


## <a name="parallel-processing"></a>Przetwarzanie równoległe

Jedną z zalet przepływy pracy programu Windows PowerShell jest możliwość wykonywania zestaw poleceń równolegle zamiast kolejno podobnie jak w przypadku typowego skryptu. 

**Równoległe** słowo kluczowe umożliwia tworzenie blok skryptu z wieloma poleceniami, które będą działać jednocześnie. To jest używana składnia pokazano poniżej. W tym przypadku Activity1 i Activity2 rozpocznie się w tym samym czasie. Activity3 rozpocznie się dopiero po rozwiązaniu zarówno Activity1, jak i Activity2.

    Parallel
    {
      <Activity1>
      <Activity2>
    }
    <Activity3>


Na przykład można rozważyć następujące polecenia programu PowerShell, które kopiowanie wielu plików do miejsca docelowego sieci.  Te polecenia są uruchamiane po kolei, tak że jeden plik musi zakończyć kopiowanie przed rozpoczęciem następnego.     

    $Copy-Item -Path C:\LocalPath\File1.txt -Destination \\NetworkPath\File1.txt
    $Copy-Item -Path C:\LocalPath\File2.txt -Destination \\NetworkPath\File2.txt
    $Copy-Item -Path C:\LocalPath\File3.txt -Destination \\NetworkPath\File3.txt

Te same polecenia w następujących przepływ pracy jest uruchamiany równolegle tak, aby wszystkie zadania rozpoczynają kopiowanie w tym samym czasie.  Tylko wtedy, gdy są całkowicie kopiowana jest komunikat o zakończeniu wyświetlane.

    Workflow Copy-Files
    {
        Parallel 
        {
            $Copy-Item -Path "C:\LocalPath\File1.txt" -Destination "\\NetworkPath"
            $Copy-Item -Path "C:\LocalPath\File2.txt" -Destination "\\NetworkPath"
            $Copy-Item -Path "C:\LocalPath\File3.txt" -Destination "\\NetworkPath"
        }

        Write-Output "Files copied."
    }


Możesz użyć **ForEach-równoległe** konstrukcji do procesu poleceń dla każdego elementu w kolekcji jednocześnie. Elementy w zbiorze są przetwarzane równolegle podczas kolejno polecenia w bloku skryptów. To jest używana składnia pokazano poniżej. W tym przypadku Activity1 rozpocznie się w tym samym czasie dla wszystkich elementów w kolekcji. Dla każdego elementu Activity2 rozpocznie się po zakończeniu Activity1. Activity3 rozpocznie się dopiero po zarówno Activity1, jak i Activity2 została ukończona dla wszystkich elementów.

    ForEach -Parallel ($<item> in $<collection>)
    {
      <Activity1>
      <Activity2>
    }
    <Activity3>

Poniższy przykład jest podobny do poprzedniego przykładu kopiowanie plików równolegle.  W tym przypadku dla każdego pliku, jest wyświetlany komunikat, po skopiowaniu.  Tylko wtedy, gdy są całkowicie kopiowana jest zakończenia wyświetlona wiadomość.

    Workflow Copy-Files
    {
        $files = @("C:\LocalPath\File1.txt","C:\LocalPath\File2.txt","C:\LocalPath\File3.txt")

        ForEach -Parallel ($File in $Files) 
        {
            $Copy-Item -Path $File -Destination \\NetworkPath
            Write-Output "$File copied."
        }
        
        Write-Output "All files copied."
    }

> [AZURE.NOTE]  Zaleca się uruchomione runbooks podrzędne równolegle od widać tego do uzyskania mało wiarygodnych wyników.  Dane wyjściowe podrzędne działań aranżacji czasami nie będą widoczne i ustawień w jeden element podrzędny działań aranżacji może wpłynąć na inne runbooks równoległe podrzędny 


## <a name="checkpoints"></a>Punkty kontrolne

*Punkt kontrolny* jest migawkę bieżący stan przepływu pracy, która zawiera wartość bieżącą dla zmiennych i dowolne dane wyjściowe generowane do tego punktu. Jeśli przepływ pracy kończy się błędem lub zostało zawieszone, następnie następnym razem, gdy jest uruchamiany będzie uruchamiany z jego ostatniego punktu kontrolnego zamiast początku worfklow.  Punkt kontrolny można ustawić w przepływie pracy z działaniem **Punktu kontrolnego z przepływem pracy** .

Poniższy kod przykładowy wyjątek występuje po Activity2 powoduje zakończenia przepływu pracy. Po ponownym uruchomieniu przepływu pracy zostanie uruchomiony, uruchamiając Activity2, ponieważ jest to tylko po ostatniego punktu kontrolnego.

    <Activity1>
    Checkpoint-Workflow
    <Activity2>
    <Exception>
    <Activity3>

Po działania, które mogą być podatne na wyjątku i nie powinny być powtarzane, jeśli przepływ pracy jest wznawiane, należy ustawić punktów kontrolnych w przepływie pracy. Na przykład przepływu pracy może utworzyć maszyny wirtualnej. Można ustawić punkt kontrolny, przed i po polecenia służące do tworzenia maszyny wirtualnej. Jeśli tworzenie kończy się niepowodzeniem, polecenia chcesz powtórzyć po ponownym uruchomieniu przepływu pracy. Jeśli worfklow kończy się niepowodzeniem po pomyślnym utworzenia, a następnie maszyna wirtualna nie zostanie utworzona ponownie podczas wznowienia przepływu pracy.

Poniższy przykład kopiuje wielu plików do lokalizacji w sieci i zestawów punktu kontrolnego po każdego pliku.  Jeśli lokalizacji sieciowej został utracony, przepływ pracy zostanie kończą się błędu.  Po ponownym uruchomieniu, będzie życiorys w ostatni punkt kontrolny, co oznacza, że tylko te pliki, które już zostały skopiowane zostaną pominięte.

    Workflow Copy-Files
    {
        $files = @("C:\LocalPath\File1.txt","C:\LocalPath\File2.txt","C:\LocalPath\File3.txt")

        ForEach ($File in $Files) 
        {
            $Copy-Item -Path $File -Destination \\NetworkPath
            Write-Output "$File copied."
            Checkpoint-Workflow
        }
        
        Write-Output "All files copied."
    }

Ponieważ poświadczenia nazwy użytkownika nie są zachowywane po skontaktowaniu się z działania [Przepływu pracy wstrzymania](https://technet.microsoft.com/library/jj733586.aspx) lub po ostatniego punktu kontrolnego, należy ustawić poświadczenia wartość null, a następnie pobrać je ponownie z magazynu trwałego po nosi nazwę **Przepływu pracy wstrzymania** lub punkt kontrolny.  W przeciwnym razie może zostać wyświetlony następujący komunikat o błędzie: nie można wznowić *zadania przepływu pracy, albo ponieważ utrzymywanie danych może nie być w pełni zapisane lub zapisane utrzymywanie danych jest uszkodzona. Należy ponownie uruchomić przepływ pracy.*

Poniższy kod samej przedstawiono sposób rozwiązać w swojej runbooks przepływu pracy programu PowerShell.

       
    workflow CreateTestVms
    {
       $Cred = Get-AzureAutomationCredential -Name "MyCredential"
       $null = Add-AzureRmAccount -Credential $Cred

       $VmsToCreate = Get-AzureAutomationVariable -Name "VmsToCreate"

       foreach ($VmName in $VmsToCreate)
         {
          # Do work first to create the VM (code not shown)
        
          # Now add the VM
          New-AzureRmVm -VM $Vm -Location "WestUs" -ResourceGroupName "ResourceGroup01"

          # Checkpoint so that VM creation is not repeated if workflow suspends
          $Cred = $null
          Checkpoint-Workflow
          $Cred = Get-AzureAutomationCredential -Name "MyCredential"
          $null = Add-AzureRmAccount -Credential $Cred
         }
     } 


Nie jest wymagane jeśli są uwierzytelnianie przy użyciu konta Uruchom jako konfigurowana głównej usługi.  

Aby uzyskać więcej informacji o punktach kontrolnych zobacz [Dodawanie punktów kontrolnych do skryptu przepływu pracy](http://technet.microsoft.com/library/jj574114.aspx).


## <a name="next-steps"></a>Następne kroki

- Aby rozpocząć pracę z runbooks przepływu pracy programu PowerShell, zobacz [Moje pierwszego działań aranżacji przepływu pracy programu PowerShell](automation-first-runbook-textual.md) 
