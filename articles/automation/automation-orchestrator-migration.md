<properties
   pageTitle="Migracja z Orchestrator do Azure automatyzacji | Microsoft Azure"
   description="W tym artykule opisano, jak przeprowadzić migrację pakiety runbooks oraz integracja z Orchestrator Centrum systemu do automatyzacji Azure."
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


# <a name="migrating-from-orchestrator-to-azure-automation-beta"></a>Migracja z Orchestrator do Azure automatyzacji (Beta)

Runbooks w [Systemie Centrum Orchestrator](http://technet.microsoft.com/library/hh237242.aspx) są oparte na działań z pakiety integracji, które zostały napisane specjalnie dla Orchestrator podczas runbooks w automatyzacji Azure są oparte na programie Windows PowerShell.  [Runbooks graficznych](automation-runbook-types.md#graphical-runbooks) w automatyzacji Azure mają wygląd podobny do Orchestrator runbooks z ich działań przedstawiająca polecenia cmdlet programu PowerShell, runbooks podrzędne i zasoby.

[Zestaw narzędzi do migracji systemu Centrum Orchestrator](http://www.microsoft.com/download/details.aspx?id=47323&WT.mc_id=rss_alldownloads_all) zawiera narzędzia, aby pomóc w konwertowania runbooks Orchestrator na automatyzacji Azure.  Oprócz konwertowanie runbooks, same, możesz przekonwertować pakiety integracji z działaniami, które używają runbooks do moduły integracji z poleceń cmdlet programu Windows PowerShell.  

Oto proces podstawowy konwertowania Orchestrator runbooks automatyzacji Azure.  Każdy z tych kroków opisano szczegółowo w poniższych sekcjach.

1.  Pobierz [Zestaw narzędzi do migracji systemu Centrum Orchestrator](http://www.microsoft.com/download/details.aspx?id=47323&WT.mc_id=rss_alldownloads_all) , który zawiera narzędzia i moduły omawiane w tym artykule.
2.  Zaimportuj [moduł standardowymi czynnościami](#standard-activities-module) do Azure automatyzacji.  Dotyczy to również przekonwertowane wersji standardowej czynności Orchestrator, które mogą być używane przez runbooks przekonwertowane.
3.  Importowanie [Moduły integracji Orchestrator Centrum systemu](#system-center-orchestrator-integration-modules) do automatyzacji Azure dla tych pakiety integrację używane przez runbooks, do którego dostęp System Center.
4.  Konwertowanie niestandardowy i innych firm pakiety integracji za pomocą [Konwertera Pack integracji](#integration-pack-converter) i zaimportować do automatyzacji Azure.
5.  Konwertowanie runbooks Orchestrator za pomocą [Konwertera działań aranżacji](#runbook-converter) i zainstalować automatyzacji Azure.
6.  Ręcznie utworzyć automatyzacji Azure wymagane aktywów Orchestrator, ponieważ konwertera działań aranżacji nie konwertuje te zasoby.
7.  Konfigurowanie [Hybrydowe działań aranżacji pracownika](#hybrid-runbook-worker) za pomocą Centrum danych lokalnych do uruchomienia przekonwertowanego runbooks, który będzie uzyskiwać dostęp do zasobów lokalnych.

## <a name="service-management-automation"></a>Usługa zarządzania automatyzacji

[Usługa zarządzania automatyzacji](http://technet.microsoft.com/library/dn469260.aspx) (SMA) są przechowywane i uruchamia runbooks za pomocą Centrum danych lokalnych, takich jak Orchestrator, a tym samym moduły integracji jest używana jako automatyzacji Azure. [Konwerter działań aranżacji](#runbook-converter) konwertuje Orchestrator runbooks runbooks graficzne jednak które nie są obsługiwane w SMA.  Do SMA nadal można zainstalować [Moduł standardowy działania](#standard-activities-module) i [Moduły integracji Orchestrator Centrum systemu](#system-center-orchestrator-integration-modules) , ale należy ręcznie [ponownie zapisać usługi runbooks](http://technet.microsoft.com/library/dn469262.aspx).

## <a name="hybrid-runbook-worker"></a>Hybrydowe działań aranżacji pracownika

Runbooks w Orchestrator są przechowywane na serwerze bazy danych i uruchamiać na serwerach działań aranżacji, zarówno za pomocą Centrum danych lokalnych.  Runbooks w automatyzacji Azure są przechowywane w chmurze Azure i może zostać uruchomiony za pomocą Centrum danych lokalnych przy użyciu [Hybrydowych działań aranżacji pracownika](automation-hybrid-runbook-worker.md).  Jest to, jak zwykle będzie uruchamiane runbooks przekonwertowane z Orchestrator, ponieważ są one przeznaczone do uruchomienia na serwerach lokalnych.

## <a name="integration-pack-converter"></a>Integracja z dodatkiem Service Pack konwertera

Integracja z dodatkiem Service Pack konwertera konwertuje pakiety integracji, które zostały utworzone przy użyciu [Narzędzi integracji Orchestrator (OIT)](http://technet.microsoft.com/library/hh855853.aspx) do integracji modułów opartych na programu Windows PowerShell, który można importować do automatyzacji Azure lub usługi zarządzania automatyzacji.  

Po uruchomieniu konwertera Integracja z dodatkiem Service Pack są prezentowane za pomocą kreatora, które umożliwi wybierz plik Integracja z dodatkiem Service pack (.oip).  Kreator następnie listy czynności związane z tym Integracja z dodatkiem Service pack i pozwala wybrać, które zostaną objęte migracją.  Po zakończeniu pracy z kreatorem tworzy integracji modułu, który zawiera odpowiednie polecenie cmdlet dla każdej z tych czynności w pakiecie integracji oryginalny.


### <a name="parameters"></a>Parametry

Wszystkie właściwości działania w pakiecie integracji są konwertowane na parametry odpowiednie polecenie cmdlet w module integracji.  Polecenia cmdlet programu Windows PowerShell mają zestaw [typowych parametrach](http://technet.microsoft.com/library/hh847884.aspx) , który może być używany z wszystkich poleceń cmdlet.  Na przykład pełne parametru - powoduje, że polecenia cmdlet celu wyprowadzenia szczegółowe informacje na temat działania.  Nie polecenia cmdlet może być parametrem taką samą nazwę jak typowych parametru.  Jeśli działanie ma właściwość o takiej samej nazwie jak typowych parametr, Kreator wyświetli monit o podanie inną nazwę parametru.

### <a name="monitor-activities"></a>Monitorowanie aktywności

Monitorowanie runbooks w Orchestrator Rozpocznij [Monitorowanie aktywności](http://technet.microsoft.com/library/hh403827.aspx) i uruchomić przez cały czas oczekiwania uznawane określonego zdarzenia.  Azure automatyzacji nie obsługuje monitor runbooks, więc nie jest konwertowana działań monitor w pakiecie integracji.  Zamiast tego polecenia cmdlet symbolu zastępczego zostanie utworzona w module integracji dla monitorowanie aktywności.  To polecenie cmdlet zawiera bez obsługi funkcji, ale umożliwia przekonwertowane działań aranżacji, korzystającej z niego musi być zainstalowany.  Ten działań aranżacji nie będzie można uruchomić w automatyzacji Azure, ale można zainstalować, tak aby można ją modyfikować.

### <a name="integration-packs-that-cannot-be-converted"></a>Pakiety integracji, których nie można przekonwertować

Nie można przekonwertować pakiety integracji, które nie zostały utworzone za pomocą OIT przy użyciu konwertera Integracja z dodatkiem Service Pack. Dostępne są także niektóre pakiety integracji dostarczane przez firmę Microsoft, którego nie można przekonwertować przez to narzędzie.  Przekonwertowane wersji tych pakietów integracji były [dostępne do pobrania](#system-center-orchestrator-integration-modules) , aby mógł zostać zainstalowany automatyzacji Azure lub usługi zarządzania automatyzacji.


## <a name="standard-activities-module"></a>Moduł standardowy działania

Orchestrator zawiera zestaw [standardowymi czynnościami](http://technet.microsoft.com/library/hh403832.aspx) nie zostaną dołączone do pakietu integracji, które są używane przez wiele runbooks.  Moduł standardowy działania jest integracji modułu, który zawiera polecenia cmdlet równoważne dla każdej z tych działalności.  Przed zaimportowaniem dowolnego przekonwertowane runbooks korzystające z działaniem standardowy, należy zainstalować ten moduł integracji w automatyzacji Azure.

Oprócz pomocniczych przekonwertowane runbooks, poleceń cmdlet w module standardowymi czynnościami może służyć przez znajomych z Orchestrator do utworzenia nowego runbooks w automatyzacji Azure.  Gdy funkcje wszystkich standardowymi czynnościami można wykonywać za pomocą polecenia cmdlet, może być działają inaczej.  Polecenia cmdlet w module przekonwertowanego standardowymi czynnościami działa tak samo, jak ich odpowiednich działań, a następnie za pomocą te same parametry.  To może pomóc w istniejącej autora działań aranżacji Orchestrator w ich przejścia do runbooks automatyzacji Azure.

## <a name="system-center-orchestrator-integration-modules"></a>Moduły integracji Orchestrator System Center

Firma Microsoft świadczy [pakiety integracji](http://technet.microsoft.com/library/hh295851.aspx) dotyczące tworzenia runbooks, aby zautomatyzować składniki System Center i innych produktów.  Niektóre z tych pakietów integracji obecnie są oparte na OIT, ale obecnie nie można przekonwertować na moduły integracji ze względu na znanych problemów.  [Moduły integracji Orchestrator Centrum systemu](https://www.microsoft.com/download/details.aspx?id=49555) zawiera przekonwertowane wersje tych pakietów integracji, które mogą być importowane do automatyzacji Azure i Usługa zarządzania automatyzacji.  

W wersji RTM tego narzędzia zostanie opublikowana zaktualizowane wersje pakiety integrację według OIT, który może zostać przekonwertowany przy użyciu konwertera Integracja z dodatkiem Service Pack.  Wskazówki dotyczące zapewnia się ułatwiający konwertowania runbooks przy użyciu działań pakiety integracji nie jest oparty na OIT.

## <a name="runbook-converter"></a>Konwerter działań aranżacji

Konwerter działań aranżacji konwertuje Orchestrator runbooks [runbooks graficzne](automation-runbook-types.md#graph-runbooks) , który można importować do automatyzacji Azure.  

Konwerter działań aranżacji jest zaimplementowana jako modułu programu PowerShell przy użyciu polecenia cmdlet o nazwie **ConvertFrom SCORunbook** wykonujące konwersji.  Po zainstalowaniu narzędzia utworzy skrót do sesji programu PowerShell, który ładuje polecenia cmdlet.   

Oto proces podstawowy, aby przekonwertować działań aranżacji Orchestrator i zaimportować automatyzacji Azure.  W poniższych sekcjach przedstawiono jeszcze bardziej szczegółowe informacje na przy użyciu narzędzia i pracy z runbooks przekonwertowane.

1. Eksportowanie runbooks co najmniej jeden z Orchestrator.
2. Uzyskanie moduły integracji do wszystkich działań w działań aranżacji.
3. Konwertowanie runbooks Orchestrator eksportowanego pliku.
4. Przejrzyj informacje w dziennikach, aby sprawdzić poprawność konwersji i w celu sprawdzenia wszystkich wymaganych zadań ręcznego.
4. Zaimportować przekonwertowane runbooks do automatyzacji Azure.
5. Utwórz wszystkie wymagane elementy zawartości w automatyzacji Azure.
6. Edytowanie działań aranżacji w automatyzacji Azure, aby zmodyfikować wszelkie wymagane czynności.

### <a name="using-runbook-converter"></a>Za pomocą konwertera działań aranżacji

Składnia **ConvertFrom SCORunbook** jest w następujący sposób:

    ConvertFrom-SCORunbook -RunbookPath <string> -Module <string[]> -OutputFolder <string> 

- RunbookPath - ścieżkę do pliku eksportu zawierającej runbooks do przekonwertowania.
- Moduł — lista przecinkami moduły integracji zawierające działania w runbooks.
- OutputFolder - ścieżka do folderu, aby utworzyć konwertowane runbooks graficznego. 


Następujące polecenie przykładowe konwertuje runbooks w pliku eksportu o nazwie **MyRunbooks.ois_export**.  Pakiety integracji usługi Active Directory i Menedżer ochrona danych za pomocą tych runbooks.

    ConvertFrom-SCORunbook -RunbookPath "c:\runbooks\MyRunbooks.ois_export" -Module c:\ip\SystemCenter_IntegrationModule_ActiveDirectory.zip,c:\ip\SystemCenter_IntegrationModule_DPM.zip -OutputFolder "c:\runbooks" 


### <a name="log-files"></a>Pliki dziennika

Konwerter działań aranżacji utworzy następujące pliki dziennika w tej samej lokalizacji co przekonwertowane działań aranżacji.  Jeśli pliki już istnieje, następnie zostaną one zastąpione na podstawie informacji z ostatniego konwersji.

| Plik | Zawartość |
|:---|:---|
| Konwerter działań aranżacji - Progress.log | Szczegółowe kroki konwersji, w tym informacje dla każdego działania pomyślnie przekonwertowane i ostrzeżenia dla każdej czynności nie konwertowany. |
| Konwerter działań aranżacji - Summary.log  | Podsumowanie ostatniego przekształcenia w tym ostrzeżenia i Flaga monitująca zadania, które należy wykonać, takich jak utworzenie zmiennej wymagane dla przekonwertowane działań aranżacji.  |

### <a name="exporting-runbooks-from-orchestrator"></a>Eksportowanie runbooks z Orchestrator

Konwerter działań aranżacji współdziała z pliku eksportu z Orchestrator, który zawiera co najmniej jeden runbooks.  Utworzy odpowiednich działań aranżacji automatyzacji Azure dla każdego działań aranżacji Orchestrator w pliku eksportu.  

Aby wyeksportować działań aranżacji z Orchestrator, kliknij prawym przyciskiem myszy nazwę zestawu działań aranżacji w Projektancie działań aranżacji i wybierz polecenie **Eksportuj**.  Aby wyeksportować wszystkie runbooks w folderze, kliknij prawym przyciskiem myszy nazwę folderu i wybierz polecenie **Eksportuj**.


### <a name="runbook-activities"></a>Działania działań aranżacji

Konwerter działań aranżacji konwertuje każdego działania w działań aranżacji Orchestrator odpowiednie działanie w automatyzacji Azure.  Dla tych działań, których nie można przekonwertować działaniem symbolu zastępczego zostanie utworzona w działań aranżacji z tekstem ostrzeżenie.  Po zaimportowaniu przekonwertowane działań aranżacji do automatyzacji Azure należy zamienić dowolną z następujących czynności na prawidłową czynności, które wykonywać wymaganą funkcjonalność.

Wszelkie działania Orchestrator [Moduł standardowy działania](#standard-activities-module) zostaną przekonwertowane.  Istnieje kilka standardowymi czynnościami Orchestrator, które nie znajdują się w tym module jednak i nie są konwertowane.  Na przykład **Wysyłanie zdarzenie platformy** ma nie automatyzacji Azure równoważne, ponieważ zdarzenie jest specyficzne dla Orchestrator.

[Monitorowanie aktywności](https://technet.microsoft.com/library/hh403827.aspx) nie są przekształcane, ponieważ brak jest odpowiednika do nich w automatyzacji Azure.  Wyjątkiem są monitor aktywności w [przekonwertowane pakiety integracji](#integration-pack-converter) której zostaną przekonwertowane na działanie symbol zastępczy.

Jeśli wprowadź ścieżkę do modułu integracji z parametrem **moduły** zostaną przekonwertowane czynność z [konwertowane Integracja z dodatkiem Service pack](#integration-pack-converter) .  System Centrum integracji pakietów możesz użyć [Moduły integracji Orchestrator Centrum systemu](#system-center-orchestrator-integration-modules).


### <a name="orchestrator-resources"></a>Zasoby orchestrator

Konwerter działań aranżacji konwertuje tylko runbooks, a nie inne zasoby Orchestrator takie jak liczniki, zmienne lub połączenia.  Liczniki nie są obsługiwane w automatyzacji Azure.  Zmienne i połączenia są obsługiwane, ale możesz utworzyć je ręcznie.  Pliki dziennika informujące, jeśli działań aranżacji wymaga takich zasobów, a następnie określ odpowiednie zasoby, które należy utworzyć w automatyzację Azure przekonwertowane działań aranżacji działać prawidłowo.

Na przykład działań aranżacji może użyć zmiennej do wypełnienia na konkretną wartość w działanie.  Przekonwertowane działań aranżacji będzie Konwertowanie działania i określ zbiór zmiennych w automatyzacji Azure o takiej samej nazwie jak zmienna Orchestrator.  Spowoduje to zauważyć, w pliku **Konwertera działań aranżacji - Summary.log** , który jest tworzona po konwersji.  Będzie konieczne ręczne tworzenie tego trwałego zmiennych w automatyzacji Azure przed użyciem działań aranżacji. 

 
### <a name="input-parameters"></a>Parametrów wejściowych

Runbooks w Orchestrator Zaakceptuj parametrów wejściowych z działaniem **Zainicjować danych** .  Jeśli działań aranżacji konwertowany zawiera to działanie, [parametru wejściowego](automation-graphical-authoring-intro.md#runbook-input-and-output) działań aranżacji automatyzacji Azure zostanie utworzony dla każdego parametru w działaniu.  Działanie [sterowania skrypt przepływu pracy](automation-graphical-authoring-intro.md#activities) zostanie utworzona w przekonwertowane działań aranżacji, która pobiera i zwraca każdego parametru.  Wszelkie wykonywanych w działań aranżacji użyć parametru wejściowego odwołują się do wyników z tego działania.

Przyczyny stosowanie tej strategii jest najlepiej lustrzane funkcję w działań aranżacji Orchestrator.  Działania w nowej runbooks graficzne powinny bezpośrednio odwołują się do wprowadzania parametrów przy użyciu źródła danych wejściowych działań aranżacji.


### <a name="invoke-runbook-activity"></a>Wywołaj działań aranżacji aktywności

Runbooks w Orchestrator rozpoczynać aktywności **Wywołania działań aranżacji** innych runbooks. Działań aranżacji konwertowany obejmuje to działanie, jest ustawiona opcja **czekać na zakończenie** działania działań aranżacji zostanie utworzona w dla niego w przekonwertowanego działań aranżacji.  Jeśli wybrano opcję **czekać na ukończenie** działania skryptu przepływu pracy zostanie utworzony używaną **Start AzureAutomationRunbook** do uruchomienia działań aranżacji.  Po zaimportowaniu przekonwertowane działań aranżacji do automatyzacji Azure należy zmodyfikować to działanie informacje wyszczególnione w działaniu.




## <a name="related-articles"></a>Artykuły pokrewne

- [System Centrum 2012 — Orchestrator](http://technet.microsoft.com/library/hh237242.aspx)
- [Usługa zarządzania automatyzacji](https://technet.microsoft.com/library/dn469260.aspx)
- [Hybrydowe działań aranżacji pracownika](automation-hybrid-runbook-worker.md)
- [Orchestrator standardowymi czynnościami](http://technet.microsoft.com/library/hh403832.aspx)
- [Zestaw narzędzi do migracji Orchestrator Centrum pobierania systemu](https://www.microsoft.com/en-us/download/details.aspx?id=47323)
 
