<properties
   pageTitle="Dodawanie runbooks Azure automatyzacji do planów odzyskiwania | Microsoft Azure"
   description="W tym artykule opisano, jak Odzyskiwanie witryny Azure umożliwia teraz rozszerzanie planów odzyskiwania wykonywanie złożonych zadań podczas odzyskiwania Azure przy użyciu automatyzacji Azure"
   services="site-recovery"
   documentationCenter=""
   authors="ruturaj"
   manager="gauravd"
   editor=""/>

<tags
   ms.service="site-recovery"
   ms.devlang="powershell"
   ms.tgt_pltfrm="na"
   ms.topic="article"
   ms.workload="required"
   ms.date="10/23/2016"
   ms.author="ruturajd@microsoft.com"/>


# <a name="add-azure-automation-runbooks-to-recovery-plans---classic"></a>Dodawanie runbooks Azure automatyzacji do odzyskiwania planów — klasyczny


Ten przewodnik opisuje, jak Odzyskiwanie witryny Azure można zintegrować z automatyzacji Azure zapewnienie rozszerzeń do planów odzyskiwania. Plany odzyskiwania można dodać akompaniament odzyskiwania maszyn wirtualnych chronione za pomocą Odzyskiwanie witryny Azure zarówno replikacji w chmurze pomocniczej i replikacji Azure scenariuszy. Pomagają także dokonując odzyskiwania **spójne dokładności**, **powtarzalnych**i **Automatyczne**. Jeśli błędy nad maszyn wirtualnych Azure Integracja z automatyzacji Azure rozszerza planów odzyskiwania i udostępni Ci możliwość wykonywania runbooks, umożliwiając zaawansowane automatyzację zadań.

Jeśli masz nie wiesz o automatyzacji Azure jeszcze, Utwórz konto [w tym miejscu](https://azure.microsoft.com/services/automation/) i pobieranie ich przykładowe skrypty [tutaj](https://azure.microsoft.com/documentation/scripts/). Więcej informacji o [Odzyskiwanie witryny Azure](https://azure.microsoft.com/services/site-recovery/) oraz jak dodać akompaniament odzyskiwania Azure za pomocą planów odzyskiwania [tutaj](https://azure.microsoft.com/blog/?p=166264).

W tym samouczku krótki firma Microsoft będzie wyglądać w sposób można zintegrować runbooks automatyzacji Azure planów odzyskiwania. Firma Microsoft będzie zautomatyzować prostych zadań, które wcześniej wymagane ręczne i zobacz, jak przekonwertować odzyskiwania kroku wielu akcji odzyskiwania jednym kliknięciem. Firma Microsoft będzie również Przyjrzyj się jak można rozwiązać prosty skrypt, jeśli ulegnie problem.

## <a name="protect-the-application-to-azure"></a>Ochrona aplikacji Azure

Pozwól nam zaczynają się od prostej aplikacji składający się z dwóch maszyn wirtualnych. W tym miejscu mamy HRweb stosowania Fabrikam. Fabrikam-HRweb-frontend i Fabrikam-Hrweb-wewnętrznej bazy danych są dwie maszyn wirtualnych chroniony Azure za pomocą Odzyskiwanie witryny Azure. Aby chronić maszyn wirtualnych przy użyciu Azure Odzyskiwanie witryny, wykonaj poniższe czynności.

1.  Włączanie ochrony dla maszyn wirtualnych.

2.  Upewnij się, że maszyn wirtualnych została ukończona replikacji początkowej i są replikacji.

3.  Poczekaj do ukończenia replikacji początkowej i stan replikacji z opisem chroniony.

![](media/site-recovery-runbook-automation/01.png)
---------------------

W tym samouczku zostanie utworzony planu odzyskiwania stosowania Fabrikam HRweb pracy awaryjnej aplikacji Azure. Następnie możemy Integracja z go z działań aranżacji, utworzony punkt końcowy na nie powiodło się nad Azure maszyny wirtualnej do obsługi stron sieci web na porcie 80.

Najpierw Tworzenie planu odzyskiwania dla naszych aplikacji.

## <a name="create-the-recovery-plan"></a>Tworzenie planu odzyskiwania

Aby odzyskać aplikacji Azure, musisz utworzyć plan odzyskiwania.
Za pomocą planu odzyskiwania, które można określić kolejność odzyskiwania maszyn wirtualnych. Maszyny wirtualnej umieszczony w grupie 1 będzie odzyskać i Rozpocznij pierwszy i postępuj maszyny wirtualnej w grupie 2.

Tworzenie planu odzyskiwania, który wygląda jak poniżej.

![](media/site-recovery-runbook-automation/12.png)

Dowiedzieć się więcej o planach odzyskiwania, zapoznaj się z dokumentacją [tutaj](https://msdn.microsoft.com/library/azure/dn788799.aspx "tutaj").

Następnie tworzenie niezbędne artefaktów w automatyzacji Azure.

## <a name="create-the-automation-account-and-its-assets"></a>Tworzenie konta automatyzacji i jego składników majątku

Potrzebne jest konto Azure automatyzacji, aby utworzyć runbooks. Jeśli nie masz konta, przejdź do karty automatyzacji Azure oznaczona ![](media/site-recovery-runbook-automation/02.png)i Utwórz nowe konto.

1.  Określ nazwę identyfikującą z konta.

2.  Określ miejsce, w którym chcesz umieścić konta regionu geograficznego.

Zalecane jest, aby umieścić konta w tym samym regionie jako magazynu automatycznego odzyskiwania systemu.

![](media/site-recovery-runbook-automation/03.png)

Następnie należy utworzyć następujące zasoby w oknie konta.

### <a name="add-a-subscription-name-as-asset"></a>Dodaj nazwę subskrypcji jako elementów zawartości

1.  Dodaj nowe ustawienie ![](media/site-recovery-runbook-automation/04.png) w Azure automatyzacji aktywa i wybierz, aby![](media/site-recovery-runbook-automation/05.png)

2.  Wybierz typ zmiennej jako **ciąg**

3.  Określ nazwę zmiennej jako **AzureSubscriptionName**

    ![](media/site-recovery-runbook-automation/06.png)

4.  Określ nazwę subskrypcji Azure rzeczywistych jako wartość zmiennej.

    ![](media/site-recovery-runbook-automation/07_1.png)

Można określić nazwę swojej subskrypcji z poziomu strony ustawień Twojego konta portalu Azure.

### <a name="add-an-azure-login-credential-as-asset"></a>Dodawanie poświadczeń logowania Azure jako elementów zawartości

Azure automatyzacji używa Azure programu PowerShell do łączenia się z subskrypcją i działa na brak artefaktów. W tym należy przeprowadzać uwierzytelnianie za pomocą konta Microsoft lub konta służbowego.
Poświadczenia konta można przechowywać w środka trwałego bezpieczne używanego przez działań aranżacji.

1.  Dodaj nowe ustawienie ![](media/site-recovery-runbook-automation/04.png) w Azure automatyzacji aktywa i wybierz pozycję![](media/site-recovery-runbook-automation/09.png)

2.  Wybierz typ poświadczeń jako **Poświadczenia systemu Windows PowerShell**

3.  Określ nazwę jako **AzureCredential**

    ![](media/site-recovery-runbook-automation/10.png)

4.  Określ nazwę użytkownika i hasło, aby zalogować się przy.

Teraz oba te ustawienia są dostępne w aktywów.

![](media/site-recovery-runbook-automation/11.png)

Więcej informacji na temat sposobu łączenia się z subskrypcji przy użyciu programu PowerShell znajduje się [poniżej](../powershell-install-configure.md).

Następnie utworzy działań aranżacji w automatyzacji Azure, które można dodać punktu końcowego dla zewnętrzną maszyny wirtualnej po przełączeniu.

## <a name="azure-automation-context"></a>Kontekst Azure automatyzacji

Automatycznego odzyskiwania systemu przekazuje zmiennej kontekstu do działań aranżacji ułatwia pisanie deterministyczne skryptów. Jeden może argumentowało nazwy usługi w chmurze i maszyny wirtualnej są przewidywalne, że się dzieje, że nie zawsze jest wielkość liter, ze względu na określonych scenariuszach, takich jak to miejsce, w którym mógł zmienić nazwę maszyn wirtualnych z powodu nieobsługiwanego znaków w Azure. W związku z tym te informacje są przesyłane do planu odzyskiwania automatycznego odzyskiwania systemu w ramach *kontekstu*.

Poniżej przedstawiono przykładowy wygląd zmiennej kontekstu.

        {"RecoveryPlanName":"hrweb-recovery",

        "FailoverType":"Test",

        "FailoverDirection":"PrimaryToSecondary",

        "GroupId":"1",

        "VmMap":{"7a1069c6-c1d6-49c5-8c5d-33bfce8dd183":

                {"CloudServiceName":"pod02hrweb-Chicago-test",

                "RoleName":"Fabrikam-Hrweb-frontend-test"}

                }

        }


Poniższa tabela zawiera nazwę i opis dla każdej zmiennej w kontekście.

**Nazwa zmiennej** | **Opis**
---|---
RecoveryPlanName | Nazwa planu uruchomione. Ułatwia wykonywanie akcji na podstawie nazwy przy użyciu samej skryptu
FailoverType | Określa, czy tym przełączeniu jest test, planowanego lub niezaplanowane.
FailoverDirection | Określ, czy odzyskiwania jest podstawowy i pomocniczy
Identyfikator grupy | Określ liczbę grupy w ramach planu odzyskiwania, gdy jest uruchomiony planu
VmMap | Tablica maszyn wirtualnych w grupie
Klucz VMMap | Unikatowy klucz (GUID) dla każdego maszyn wirtualnych. Jest taki sam, jak identyfikator VMM maszyny wirtualnej w stosownych przypadkach.
RoleName | Nazwa maszyn wirtualnych Azure, który jest ich odzyskania
CloudServiceName | W obszarze który maszyny wirtualnej jest tworzona Azure nazwa usługi w chmurze.


Do identyfikowania klucza VmMap w kontekście możesz również przejdź do strony właściwości maszyn wirtualnych w automatycznego odzyskiwania systemu i przeglądać właściwość GUID maszyn wirtualnych.

![](media/site-recovery-runbook-automation/13.png)

## <a name="author-an-automation-runbook"></a>Autor działań aranżacji automatyzacji

Teraz można utworzyć działań aranżacji, aby otworzyć portu 80 zewnętrzną maszyny wirtualnej.

1.  Tworzenie nowych działań aranżacji na koncie Azure automatyzacji o nazwie **OpenPort80**

    ![](media/site-recovery-runbook-automation/14.png)

2.  Przejdź do widoku Autor zestawu działań aranżacji i przejść do trybu wersję roboczą.

3.  Określ najpierw zmiennej ma zostać użyte jako kontekst planu odzyskiwania

    ```
        param (
            [Object]$RecoveryPlanContext
        )

    ```

4.  Następnie nawiązywanie połączenia z subskrypcji przy użyciu nazwy poświadczeń i subskrypcji

    ```
        $Cred = Get-AutomationPSCredential -Name 'AzureCredential'

        # Connect to Azure
        $AzureAccount = Add-AzureAccount -Credential $Cred
        $AzureSubscriptionName = Get-AutomationVariable –Name ‘AzureSubscriptionName’
        Select-AzureSubscription -SubscriptionName $AzureSubscriptionName
    ```

    Uwaga Użycie Azure składniki majątku — **AzureCredential** i **AzureSubscriptionName** w tym miejscu.

5.  Teraz Określ szczegóły punktu końcowego i identyfikator GUID maszyny wirtualnej, dla której chcesz udostępnić punkt końcowy. W tym przypadku zewnętrzną maszyny wirtualnej.

    ```
        # Specify the parameters to be used by the script
        $AEProtocol = "TCP"
        $AELocalPort = 80
        $AEPublicPort = 80
        $AEName = "Port 80 for HTTP"
        $VMGUID = "7a1069c6-c1d6-49c5-8c5d-33bfce8dd183"
    ```

    Określa protokół Azure punktu końcowego, port lokalny na maszyn wirtualnych i jego zamapowanych publicznej. Zmienne są parametry wymagane przez Azure polecenia, które Dodawanie punktów końcowych do maszyny wirtualne. VMGUID zawiera identyfikator GUID maszyny wirtualnej, potrzebne do działania na.

6.  Skrypt teraz wyodrębnianie kontekst dla danego GUID maszyn wirtualnych i tworzenie punktu końcowego na komputerze wirtualnych odwołuje się go.

    ```
        #Read the VM GUID from the context
        $VM = $RecoveryPlanContext.VmMap.$VMGUID

        if ($VM -ne $null)
        {
            # Invoke pipeline commands within an InlineScript

            $EndpointStatus = InlineScript {
                # Invoke the necessary pipeline commands to add a Azure Endpoint to a specified Virtual Machine
                # Commands include: Get-AzureVM | Add-AzureEndpoint | Update-AzureVM (including parameters)

                $Status = Get-AzureVM -ServiceName $Using:VM.CloudServiceName -Name $Using:VM.RoleName | `
                    Add-AzureEndpoint -Name $Using:AEName -Protocol $Using:AEProtocol -PublicPort $Using:AEPublicPort -LocalPort $Using:AELocalPort | `
                    Update-AzureVM
                Write-Output $Status
            }
        }
    ```

7. Po zakończeniu naciśnij Publikuj ![](media/site-recovery-runbook-automation/20.png) umożliwia skrypt był dostępny podczas wykonywania.

Kompletny skrypt poniżej podano dla odwołania

```
  workflow OpenPort80
  {
    param (
        [Object]$RecoveryPlanContext
    )

    $Cred = Get-AutomationPSCredential -Name 'AzureCredential'

    # Connect to Azure
    $AzureAccount = Add-AzureAccount -Credential $Cred
    $AzureSubscriptionName = Get-AutomationVariable –Name ‘AzureSubscriptionName’
    Select-AzureSubscription -SubscriptionName $AzureSubscriptionName

    # Specify the parameters to be used by the script
    $AEProtocol = "TCP"
    $AELocalPort = 80
    $AEPublicPort = 80
    $AEName = "Port 80 for HTTP"
    $VMGUID = "7a1069c6-c1d6-49c5-8c5d-33bfce8dd183"

    #Read the VM GUID from the context
    $VM = $RecoveryPlanContext.VmMap.$VMGUID

    if ($VM -ne $null)
    {
        # Invoke pipeline commands within an InlineScript

        $EndpointStatus = InlineScript {
            # Invoke the necessary pipeline commands to add an Azure Endpoint to a specified Virtual Machine
            # This set of commands includes: Get-AzureVM | Add-AzureEndpoint | Update-AzureVM (including necessary parameters)

            $Status = Get-AzureVM -ServiceName $Using:VM.CloudServiceName -Name $Using:VM.RoleName | `
                Add-AzureEndpoint -Name $Using:AEName -Protocol $Using:AEProtocol -PublicPort $Using:AEPublicPort -LocalPort $Using:AELocalPort | `
                Update-AzureVM
            Write-Output $Status
        }
    }
  }
```

## <a name="add-the-script-to-the-recovery-plan"></a>Dodaj skrypt do planu odzyskiwania

Gdy skrypt będzie gotowa, możesz dodać je do planu odzyskiwania, który został utworzony wcześniej.

1.  W utworzony plan odzyskiwania wybierz Dodawanie skryptu po grupie 2. ![](media/site-recovery-runbook-automation/15.png)

2.  Określ nazwę skryptu. To jest po prostu przyjazną nazwę dla tego skryptu dla przedstawiający w ramach planu odzyskiwania.

3.  W tym przełączeniu skryptom Azure — wybierz nazwę konta na platformie Azure automatyzacji.

4.  W Azure Runbooks wybierz działań aranżacji Twojego autorstwa.

![](media/site-recovery-runbook-automation/16.png)

## <a name="primary-side-scripts"></a>Strona podstawowego skryptów

Podczas wykonywania awarię na Azure, możesz również wybrać wykonywanie skryptów strony podstawowego. Skrypty te będą uruchamiane na serwerze VMM podczas pracy awaryjnej.
Skryptów podstawowego po stronie są dostępne tylko w przypadku poprzedzające zamknięcie tylko i Publikuj etapów zamknięcia. Jest to spowodowane oczekujemy podstawowej witryny jest zazwyczaj niedostępna, gdy trafiał awarii.
Podczas nieplanowanego przełączania awaryjnego tylko, jeśli wybierzesz w operacji podstawowej witryny próbuje uruchomić skrypty strony podstawowego. Jeśli nie są one dostępne lub przekroczenia limitu czasu, w tym przełączeniu będzie odzyskać maszyn wirtualnych.
Skryptów podstawowego po stronie są nie dostępne dla witryn VMware-fizycznych i funkcji Hyper-v bez VMM chroniony Azure - podczas przełączanie możesz awaryjne do Azure.
Jednak, gdy możesz awarii z platformy Azure do lokalnego, skryptów podstawowego po stronie (Runbooks) może być używany dla wszystkich elementów docelowych z wyjątkiem VMware.

## <a name="test-the-recovery-plan"></a>Testowanie planu odzyskiwania

Po dodaniu działań aranżacji do planu można zainicjować trybie awaryjnym test i ją wyświetlić w akcji. Zalecane jest zawsze uruchomić awarię test na testowanie aplikacji i planowanie odzyskiwania, aby upewnić się, że nie ma żadnych błędów.

1.  Wybierz plan, odzyskiwania i zainicjować trybie awaryjnym test.

2.  Podczas wykonywania planu widać, czy wykonała działań aranżacji nie za pomocą jego stan.

    ![](media/site-recovery-runbook-automation/17.png)

3.  Można też wyświetlić stan wykonania szczegółowe działań aranżacji w stronie Azure automatyzację zadań dla działań aranżacji.

    ![](media/site-recovery-runbook-automation/18.png)

4.  Po zakończeniu pracy tym przełączeniu oprócz wyniku wykonywanie działań aranżacji widać, czy wykonanie zakończyło się pomyślnie lub nie, odwiedź stronę Azure maszyn wirtualnych i przeglądająca punkty końcowe.

![](media/site-recovery-runbook-automation/19.png)

## <a name="sample-scripts"></a>Przykładowe skrypty

Gdy rezygnacja za pośrednictwem jednego automatyzowanie często używanych zadanie dodawania punktu końcowego Azure maszyn wirtualnych w tym samouczku, można zrobić na kilka innych zaawansowanych automatyzację zadań przy użyciu automatyzacji Azure. Firma Microsoft i społeczności automatyzacji Azure opisowe, runbooks próbki, które mogą pomóc rozpocząć tworzenie własnych rozwiązań i runbooks narzędzie, którego można użyć jako bloków konstrukcyjnych dla większych automatyzację zadań. Rozpocznij korzystanie z nich z galerii i tworzenie zaawansowanych odzyskiwania jednym kliknięciem plany dla aplikacji przy użyciu Odzyskiwanie witryny Azure.

## <a name="additional-resources"></a>Dodatkowe zasoby

[Omówienie Azure automatyzacji] (http://msdn.microsoft.com/library/azure/dn643629.aspx "Omówienie Azure automatyzacji")

[Przykładowe skrypty Azure automatyzacji] (http://gallery.technet.microsoft.com/scriptcenter/site/search?f[0].Type=User&f[0].Value=SC%20Automation%20Product%20Team&f[0].Text=SC%20Automation%20Product%20Team "Przykładowe skrypty Azure automatyzacji")
