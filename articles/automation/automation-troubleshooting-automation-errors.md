<properties
   pageTitle="Obsługa błędów Azure automatyzacji | Microsoft Azure"
   description="Ten artykuł zawiera podstawowe błąd obsługi kroki umożliwiające rozwiązywanie problemów i poprawianie typowych błędów automatyzacji Azure."
   services="automation"
   documentationCenter=""
   authors="mgoedtel"
   manager="stevenka"
   editor="tysonn"
   tags="top-support-issue"
   keywords="Błąd automatyzacji obsługi błędów"/>
<tags
   ms.service="automation"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="07/06/2016"
   ms.author="sngun; v-reagie"/>

# <a name="error-handling-tips-for-common-azure-automation-errors"></a>Porady dotyczące typowych błędów automatyzacji Azure obsługi błędów

W tym artykule opisano niektóre typowe błędy automatyzacji Azure mogą wystąpić i sugerowanych możliwy błąd obsługi czynności.

## <a name="troubleshoot-authentication-errors-when-working-with-azure-automation-runbooks"></a>Rozwiązywanie problemów z błędami uwierzytelniania, podczas pracy z runbooks automatyzacji Azure  

### <a name="scenario-sign-in-to-azure-account-failed"></a>Scenariusz: Zaloguj się do konta na platformie Azure nie powiodła się

**Błędu:** Otrzymujesz komunikat o błędzie "Unknown_user_type: Nieznany typ użytkownika" podczas pracy z polecenia cmdlet AzureAccount Dodaj lub AzureRmAccount logowania.

**Przyczynę błędu:** Ten błąd występuje, jeśli nazwa trwałego poświadczeń nie jest prawidłowa lub nazwę użytkownika i hasło, które zostało użyte do instalacji trwałego poświadczeń automatyzacji nie są prawidłowe.

**Porady dotyczące rozwiązywania problemów:** Aby określić, na czym polega problem, wykonaj następujące kroki:  

1. Upewnij się, że nie ma żadnych znaków specjalnych, łącznie z **@** znaków w nazwie trwałego poświadczeń automatyzacji, używanym do łączenia Azure.  

2. Sprawdź, czy można użyć nazwy użytkownika i hasła, które są przechowywane w poświadczeń automatyzacji Azure w Edytorze lokalnych środowiska PowerShell ISE. Można to zrobić, uruchamiając następujące polecenia cmdlet w środowiska PowerShell ISE:  

        $Cred = Get-Credential  
        #Using Azure Service Management   
        Add-AzureAccount –Credential $Cred  
        #Using Azure Resource Manager  
        Login-AzureRmAccount –Credential $Cred

3. Jeśli uwierzytelnienie nie powiedzie się lokalnie, oznacza to, że nie skonfigurowano poświadczenia usługi Azure Active Directory poprawnie. Zapoznaj się z [Authenticating Azure za pomocą usługi Azure Active Directory](https://azure.microsoft.com/blog/azure-automation-authenticating-to-azure-using-azure-active-directory/) blogu, aby uzyskać konto usługi Azure Active Directory poprawnie skonfigurowane.  


### <a name="scenario-unable-to-find-the-azure-subscription"></a>Scenariusz: Nie można odnaleźć Azure subskrypcji

**Błędu:** Pojawi się komunikat o błędzie "subskrypcji o nazwie ``<subscription name>`` nie można odnaleźć" podczas pracy z polecenia cmdlet wybierz AzureSubscription lub wybierz pozycję AzureRmSubscription.

**Przyczynę błędu:** Ten błąd występuje, jeśli nie jest prawidłową nazwę subskrypcji lub jeśli użytkownika usługi Azure Active Directory, która próbuje uzyskać szczegóły subskrypcji nie jest skonfigurowany jako administrator subskrypcji.

**Porady dotyczące rozwiązywania problemów:** Aby ustalić, czy poprawnie uwierzytelnionych Azure i masz dostęp do subskrypcji, którą chcesz zaznaczyć, wykonaj następujące kroki:  

1. Upewnij się, uruchom **AzureAccount Dodaj** przed uruchomieniem polecenia cmdlet **AzureSubscription wybierz** .  

2. Jeśli nadal widzisz ten komunikat o błędzie, zmodyfikować kodu, dodając polecenia cmdlet **Get-AzureSubscription** następującego polecenia cmdlet **AzureAccount Dodaj** , a następnie wykonaj kod.  Teraz upewnij się, jeśli wynik Get-AzureSubscription zawiera szczegóły Twojej subskrypcji.  
    * Jeśli nie widzisz Szczegóły subskrypcji w wyniku kwerendy, oznacza to, nie jest jeszcze zainicjowany subskrypcji.  
    * Jeśli widzisz Szczegóły subskrypcji w wyniku kwerendy, upewnij się, że używasz subskrypcji poprawne nazwę lub identyfikator przy użyciu polecenia cmdlet **AzureSubscription wybierz** .   


### <a name="scenario-authentication-to-azure-failed-because-multi-factor-authentication-is-enabled"></a>Scenariusz: Uwierzytelnianie Azure zakończone niepowodzeniem, ponieważ jest włączone uwierzytelnianie wieloskładnikowe

**Błędu:** Pojawi się komunikat o błędzie "Dodaj AzureAccount: AADSTS50079: rejestrowanie silnego uwierzytelniania (dowód do góry) jest wymagane" podczas uwierzytelniania Azure za pomocą usługi Azure nazwy użytkownika i hasła.

**Przyczynę błędu:** Jeśli masz uwierzytelnianie wieloskładnikowe na koncie Azure nie umożliwia służą do uwierzytelniania Azure użytkownika usługi Azure Active Directory.  Zamiast tego należy uwierzytelnienia Azure za pomocą certyfikatu lub głównej usługi.

**Porady dotyczące rozwiązywania problemów:** Aby użyć certyfikatu przy użyciu polecenia cmdlet zarządzania usługą Azure, zapoznaj się z [Tworzenie i dodawanie certyfikatu do zarządzania usług Azure.](http://blogs.technet.com/b/orchestrator/archive/2014/04/11/managing-azure-services-with-the-microsoft-azure-automation-preview-service.aspx) Aby użyć wystawcy usługi z poleceń cmdlet Menedżera zasobów Azure, odwołują się do [tworzenia wystawcy usługi za pomocą portalu Azure](./resource-group-create-service-principal-portal.md) i [uwierzytelniania wystawcy usługi przy użyciu Menedżera zasobów Azure.](./resource-group-authenticate-service-principal.md)


## <a name="troubleshoot-common-errors-when-working-with-runbooks"></a>Rozwiązywanie problemów z typowych błędów podczas pracy z runbooks

### <a name="scenario-runbook-fails-because-of-deserialized-object"></a>Scenariusz: Działań aranżacji kończy się niepowodzeniem z powodu rozszeregowanym obiektu

**Błędu:** Do działań aranżacji kończy się niepowodzeniem z powodu błędu "nie można powiązać parametr ``<ParameterName>``. Nie można przekonwertować ``<ParameterType>`` wartość typu Deserialized ``<ParameterType>`` wpisywanie ``<ParameterType>``".

**Przyczynę błędu:** Jeśli programu działań aranżacji jest przepływ pracy programu PowerShell, przechowuje złożonych obiektów w formacie rozszeregowanym było swojego stanu działań aranżacji utrwalenia, jeśli przepływu pracy zostało zawieszone.  

**Porady dotyczące rozwiązywania problemów:**  
Dowolny z następujących trzech rozwiązań będzie rozwiązać ten problem:

1. Jeśli są połączeń rurowych złożone obiekty z jednego polecenia cmdlet do innego, ujmij tych poleceń cmdlet w InlineScript.  
2. Przekazać nazwę lub wartość, która jest potrzebna z obiektem złożonych zamiast przechodzące cały obiekt.  

3. Za pomocą programu PowerShell działań aranżacji zamiast działań aranżacji przepływu pracy programu PowerShell.  


### <a name="scenario-runbook-job-failed-because-the-allocated-quota-exceeded"></a>Scenariusz: Zakończone niepowodzeniem, ponieważ przekroczono przydział przydzielonego zadania działań aranżacji

**Błędu:** Zadanie działań aranżacji kończy się niepowodzeniem, z powodu błędu "przydziału dla miesięczny zadania całkowity czas wykonywania osiągnięto dla tej subskrypcji".

**Przyczynę błędu:** Ten błąd występuje, gdy wykonywania zadań przekracza 500-minutowe bezpłatnego przydziału dla Twojego konta. Ten przydział dotyczy wszystkich typów wykonanie zadania, takie jak badania zadanie, uruchamiania zadania z poziomu portalu, wykonywanie zadania przy użyciu webhooks, a zadanie do wykonania za pomocą Azure portalu lub planowania w centrum danych. Aby uzyskać więcej informacji na temat ceny automatyzacji zobacz [ceny automatyzacji](https://azure.microsoft.com/pricing/details/automation/).

**Porady dotyczące rozwiązywania problemów:** Jeśli chcesz użyć więcej niż 500 minut przetwarzania miesięcznie trzeba będzie zmienić subskrypcję z wolnym warstwa w warstwie podstawowe. Można też uaktualnić do podstawowej warstwy, wykonując następujące czynności:  

1. Zaloguj się do subskrypcji usługi Azure  
2. Wybierz konto automatyzacji, które chcesz uaktualnić  
3. Kliknij pozycję **Ustawienia** > **poziomu ceny i zastosowania** > **poziomu ceny**  
4. Wybieranie **podstawowe** na karta **Wybierz z poziomu cen**    


### <a name="scenario-cmdlet-not-recognized-when-executing-a-runbook"></a>Scenariusz: Polecenie Cmdlet nierozpoznany podczas wykonywania działań aranżacji

**Błędu:** Zadanie działań aranżacji kończy się niepowodzeniem z powodu błędu "``<cmdlet name>``: termin ``<cmdlet name>`` nie jest rozpoznawana jako nazwa polecenia cmdlet, funkcja, plik skryptu lub program wykonywalny."

**Przyczynę błędu:** Ten błąd jest powodowany aparat programu PowerShell nie można znaleźć polecenia cmdlet, używanym w swojej działań aranżacji.  Może to być spowodowane moduł zawierający polecenia cmdlet nie ma konta, istnieje konflikt nazw pod nazwą działań aranżacji lub w innym module istnieje również polecenia cmdlet i automatyzacji nie może rozpoznać nazwę.

**Porady dotyczące rozwiązywania problemów:** Dowolny z następujących rozwiązań rozwiąże ten problem:  

- Upewnij się, że nazwa polecenia cmdlet zostały wprowadzone poprawnie.  

- Upewnij się, polecenia cmdlet istnieje na koncie automatyzacji i czy nie występują żadne konflikty. Aby sprawdzić, czy polecenia cmdlet znajduje się, otwórz działań aranżacji w trybie edycji i wyszukaj ma zostać znaleziona w bibliotece lub uruchom polecenie cmdlet **polecenie Get ``<CommandName>`` **.  Gdy jest sprawdzana poprawność które polecenia cmdlet jest dostępna dla konta, a nie występują żadne konflikty nazw z innymi poleceń cmdlet lub runbooks, dodaj go do obszaru roboczego i upewnij się, że używasz prawidłowego parametru w swojej działań aranżacji.  

- Jeśli masz konflikt nazw i polecenia cmdlet jest dostępne w dwóch różnych modułów, możesz rozwiązać ten problem przy użyciu w pełni kwalifikowaną nazwę polecenia cmdlet. Na przykład można użyć **ModuleName\CmdletName**.  

- Jeśli są wykonywane działań aranżacji lokalnego w grupie Pracownik hybrydowe, a następnie upewnij się, że moduł-polecenia cmdlet jest zainstalowany na komputerze, obsługującego pracownik hybrydowych.


### <a name="scenario-a-long-running-runbook-consistently-fails-with-the-exception-the-job-cannot-continue-running-because-it-was-repeatedly-evicted-from-the-same-checkpoint"></a>Scenariusz: Długotrwałych działań aranżacji spójne kończy się niepowodzeniem z powodu wyjątku: "zadanie nie może kontynuować ponieważ wielokrotnie został usunięty z tym samym punkt kontrolny".

**Przyczynę błędu:** Jest to zachowanie projektu z powodu monitorowania "Udostępnij projektu naukowego" procesy w automatyzacji Azure, która automatycznie zawiesza działań aranżacji, jeśli wykonywania dłużej niż 3 godziny. Komunikat o błędzie zwrócony zapewnia jednak "co dalej" Opcje. Działań aranżacji może zostać zawieszone dla z kilku powodów. Zawiesza się zdarzyć się to w większości z powodu błędów. Na przykład wyjątek nie przechwycony działań aranżacji, awarii sieci lub awarię na pracownika działań aranżacji uruchomiony działań aranżacji wszystkie spowoduje działań aranżacji zawieszone i Rozpocznij od jej ostatniego punktu kontrolnego po wznowieniu.

**Porady dotyczące rozwiązywania problemów:** Udokumentowane rozwiązanie, aby uniknąć tego problemu jest użycie punktów kontrolnych w przepływie pracy.  Aby dowiedzieć się więcej dotyczą [Przepływy pracy programu PowerShell nauki](automation-powershell-workflow.md#Checkpoints).  W tym artykule blogu [Przy użyciu punktów kontrolnych w Runbooks](https://azure.microsoft.com/en-us/blog/azure-automation-reliable-fault-tolerant-runbook-execution-using-checkpoints/)znajdują się bardziej szczegółowego wyjaśnienie "Udostępnij projektu naukowego" i punktów kontrolnych.


## <a name="troubleshoot-common-errors-when-importing-modules"></a>Rozwiązywanie problemów z typowych błędów podczas importowania modułów

### <a name="scenario-module-fails-to-import-or-cmdlets-cant-be-executed-after-importing"></a>Scenariusz: Moduł kończy się niepowodzeniem zaimportować lub polecenia cmdlet nie można wykonać po zaimportowaniu

**Błędu:** Moduł kończy się niepowodzeniem zaimportować lub importuje pomyślnie, ale nie polecenia cmdlet są wyodrębniane.

**Przyczynę błędu:** Najbardziej prawdopodobne przyczyny, które moduł nie może pomyślnie zaimportować automatyzacji Azure są:  

- Struktura jest niezgodna z automatyzacji powinien się w strukturze.  

- Moduł jest uzależniony od innego modułu, który nie został wdrożony do swojego konta automatyzacji.  

- Moduł brakuje jego zależności w folderze.  

- Polecenia cmdlet **New-AzureRmAutomationModule** jest używany do przekazywania moduł, a nie spowodowały ścieżkę pełny miejsca do magazynowania lub nie załadowane moduł przy użyciu publicznie adresu URL.  

**Porady dotyczące rozwiązywania problemów:**  
Dowolny z następujących rozwiązań rozwiąże ten problem:  

- Upewnij się, że moduł występuje następujący format:  
ModuleName.Zip **->** Nazwa_modułu lub numer wersji **->** (ModuleName.psm1, ModuleName.psd1)

- Otwórz plik .psd1 i sprawdź, czy moduł ma zależności.  Jeśli tak, należy przekazać te moduły na koncie automatyzacji.  

- Upewnij się, że wszelkie odwołania biblioteki znajdują się w folderze modułu.  


## <a name="troubleshoot-common-errors-when-working-with-desired-state-configuration-dsc"></a>Rozwiązywanie problemów z typowych błędów podczas pracy z konfiguracji Państwa potrzeby (DSC)  

### <a name="scenario-node-is-in-failed-status-with-a-not-found-error"></a>Scenariusz: Węzeł jest w stanie błędu z błędem "Nie znaleziono"

**Błędu:** Węzeł zawiera raport o stanie **Niepowodzenie** i komunikat o błędzie "próby uzyskania akcji z serwera https://``<url>``//accounts/``<account-id>``/Nodes(AgentId=``<agent-id>``)/GetDscAction się niepowodzeniem, ponieważ prawidłową konfigurację ``<guid>`` nie można odnaleźć."

**Przyczynę błędu:** Ten błąd występuje zazwyczaj, gdy węzeł przydzielono Nazwa konfiguracji (na przykład ABC) zamiast węzeł Konfiguracja nazwę (na przykład ABC. Serwer_sieci_web).  

**Porady dotyczące rozwiązywania problemów:**  

- Upewnij się, że przypisujesz węzeł z "węzeł Konfiguracja nazwa" i nie "Nazwa konfiguracji".  

- Konfiguracja węzła można przypisać do węzła za pomocą portalu Azure lub przy użyciu polecenia cmdlet programu PowerShell.
    - Aby przypisać konfiguracji węzła do węzła za pomocą portalu Azure, otwórz karta **Węzły DSC** , a następnie wybierz węzeł i kliknij przycisk **Przypisz węzeł Konfiguracja** .  
    - Aby przypisać konfiguracji węzła do węzła przy użyciu polecenia cmdlet programu PowerShell, użyj polecenia cmdlet **Set-AzureRmAutomationDscNode**


### <a name="scenario--no-node-configurations-mof-files-were-produced-when-a-configuration-is-compiled"></a>Scenariusz: Nie ma żadnych konfiguracji węzeł (pliki MOF) zostały wyprodukowane podczas kompilowania konfiguracji

**Błędu:** Zadanie kompilacji DSC zawiesza się z powodu błędu: "pomyślnie wykonano kompilacji, ale nie węzeł Konfiguracja .mofs zostały wygenerowane".

**Przyczynę błędu:** Gdy zostaną utworzone poniższy wyrażenie, którego wynikiem jest słowo kluczowe **węzeł** w konfiguracji DSC $null, a następnie nie ma żadnych konfiguracji węzeł.    

**Porady dotyczące rozwiązywania problemów:**  
Dowolny z następujących rozwiązań rozwiąże ten problem:  

- Upewnij się, że wyrażenie obok kluczowego **węzeł** w definicji konfiguracji nie jest oceny do $null.  
- Jeśli ConfigurationData podczas kompilowania konfiguracji, upewnij się, że są przekazując oczekiwanych wartości, które wymaga konfiguracji z [ConfigurationData](automation-dsc-compile.md#configurationdata).


### <a name="scenario--the-dsc-node-report-becomes-stuck-in-progress-state"></a>Scenariusz: Raport węzeł DSC staje się zablokowany stan "w toku"

**Błędu:** DSC Agent Wyświetla "Nie odnaleziono wystąpienia z podanymi wartościami właściwości."

**Przyczynę błędu:** Została uaktualniona do wersji WMF i spowodować uszkodzenie WMI.  

**Porady dotyczące rozwiązywania problemów:** Postępuj zgodnie z instrukcjami [DSC znane problemy i ograniczenia](https://msdn.microsoft.com/powershell/wmf/limitation_dsc) we wpisie w blogu, aby rozwiązać ten problem.


### <a name="scenario--unable-to-use-a-credential-in-a-dsc-configuration"></a>Scenariusz: Nie można użyć poświadczenie w konfiguracji DSC

**Błędu:** DSC kompilacji zadanie zostało zawieszone z powodu błędu: "Błąd System.InvalidOperationException właściwość poświadczenie i typ przetwarzania ``<some resource name>``: konwertowanie i przechowywaniu zaszyfrowane hasło jako zwykły tekst jest dozwolone tylko wtedy, gdy PSDscAllowPlainTextPassword jest ustawiona na PRAWDA".

**Przyczynę błędu:** Użyto poświadczenie w konfiguracji, ale nie zapewniają pisane z wielkiej litery **ConfigurationData** , aby ustawić **PSDscAllowPlainTextPassword** PRAWDA dla każdej konfiguracji węzeł.  

**Porady dotyczące rozwiązywania problemów:**  
- Upewnij się, w celu przekazania w pisane z wielkiej litery **ConfigurationData** , aby ustawić **PSDscAllowPlainTextPassword** PRAWDA dla każdej konfiguracji węzeł wymienionych w konfiguracji. Aby uzyskać więcej informacji zapoznaj się z [środkami DSC automatyzacji Azure](automation-dsc-compile.md#assets).


## <a name="next-steps"></a>Następne kroki

Jeśli zostały wykonane powyższe kroki rozwiązywania problemów i potrzebujesz pomocy w dowolnym momencie, w tym artykule, należy wykonać następujące czynności:

- Uzyskaj pomoc od ekspertów Azure. Przesyłanie problemu do [forum w witrynie MSDN Azure lub przepełnienie stosu.](https://azure.microsoft.com/support/forums/)

- Plik do Azure pomocy technicznej zdarzenia. Przejdź do [witryny pomocy technicznej Azure](https://azure.microsoft.com/support/options/) i kliknij pozycję **Uzyskaj pomoc techniczną** w obszarze **pomocy technicznej i rozliczeń**.

- Jeśli szukasz rozwiązania działań aranżacji automatyzacji Azure lub modułu integracji wpis żądanie skryptów w [Centrum skryptów](https://azure.microsoft.com/documentation/scripts/) .

- Publikowanie żądania opinii lub funkcji automatyzacji Azure na [Głos użytkownika](https://feedback.azure.com/forums/34192--general-feedback).
