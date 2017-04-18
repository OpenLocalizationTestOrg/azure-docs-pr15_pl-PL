<properties 
   pageTitle="Zarządzanie danymi automatyzacji Azure | Microsoft Azure"
   description="Ten artykuł zawiera wiele tematów związanych z zarządzaniem środowiskiem automatyzacji Azure.  Obecnie zawiera przechowywanie danych i tworzenie kopii zapasowych Azure automatyzacji awarii w automatyzacji Azure."
   services="automation"
   documentationCenter=""
   authors="SnehaGunda"
   manager="stevenka"
   editor="tysonn" />
<tags 
   ms.service="automation"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="05/02/2016"
   ms.author="bwren;sngun" />

# <a name="managing-azure-automation-data"></a>Zarządzanie danymi automatyzacji Azure

Ten artykuł zawiera wiele tematów związanych z zarządzaniem środowiskiem automatyzacji Azure.

## <a name="data-retention"></a>Przechowywanie danych

Po usunięciu zasobu w automatyzacji Azure jest zachowywana 90 dni na potrzeby inspekcji przed trwale usuwany.  Nie widać lub użyj zasobu w tym czasie.  Zasada ta dotyczy również zasoby, które należą do automatyzacji konta, które zostanie usunięty.

Azure automatyzacji automatycznie usuwa i trwale zadania starsze niż 90 dni.

W poniższej tabeli przedstawiono zasady przechowywania dla różnych zasobów.

|Dane|Zasady|
|:---|:---|
|Konta|Trwale usuwane 90 dni po usunięciu konta przez użytkownika.|
|Składniki majątku|Trwale usuwane 90 dni po usunięciu elementu przez użytkownika lub 90 dni po konta, które zawiera elementu jest usuniętych przez użytkownika.|
|Moduły|Trwale usuwane 90 dni po usunięciu modułu przez użytkownika lub 90 dni po koncie, w której znajduje się, że moduł zostanie usunięty przez użytkownika.|
|Runbooks|Trwale usuwane 90 dni po usunięciu zasobu przez użytkownika lub 90 dni po konto, z którego zawierające zasobu zostanie usunięty przez użytkownika.|
|Zadania|Usunięcie i trwale usuwane 90 dni po ostatni modyfikowany. Być może po zakończeniu zadania, zatrzymaniu lub zostało zawieszone.|
|Pliki konfiguracji MOF węzeł| Stare węzeł Konfiguracja zostanie usunięty 90 dni po nowej konfiguracji węzeł jest generowany.|
|Węzły DSC| Trwale usuwane 90 dni po węzeł jest zarejestrowany z konta automatyzacji za pomocą Azure portal lub polecenia cmdlet [Unregister AzureRMAutomationDscNode](https://msdn.microsoft.com/library/mt603500.aspx) w programie Windows PowerShell. Węzły są również trwale usuwane 90 dni po koncie, w której znajduje się, że węzeł zostanie usunięty przez użytkownika. |
|Węzeł raportów| Trwale usuwane 90 dni po Generowanie nowego raportu dla tego węzła|

Zasady przechowywania dotyczą wszystkich użytkowników i obecnie nie można dostosować.

## <a name="backing-up-azure-automation"></a>Wykonywanie kopii zapasowej automatyzacji Azure

Po usunięciu konta automatyzacji platformy Microsoft Azure wszystkich obiektów w oknie konta są usuwane, łącznie z runbooks, moduły, konfiguracji, ustawienia, zadań i zasobów. Nie można odzyskać obiektów, po usunięciu konta.  Za pomocą następujących informacji wykonywania kopii zapasowej zawartości konta automatyzacji, zanim zostanie usunięta. 

### <a name="runbooks"></a>Runbooks

Usługi runbooks można eksportować do plików skryptów za pomocą portalu zarządzania Azure lub polecenia cmdlet [Get-AzureAutomationRunbookDefinition](https://msdn.microsoft.com/library/dn690269.aspx) w programie Windows PowerShell.  Te pliki skryptów można zaimportować do innego konta automatyzacji zgodnie z opisem w [Tworzenie lub importowanie działań aranżacji](https://msdn.microsoft.com/library/dn643637.aspx).


### <a name="integration-modules"></a>Moduły integracji

Nie można eksportować moduły integracji z automatyzacji Azure.  Należy się upewnić, że są one dostępne poza konta automatyzacji.

### <a name="assets"></a>Składniki majątku

Nie można wyeksportować [składniki majątku](https://msdn.microsoft.com/library/dn939988.aspx) z automatyzacji Azure.  Za pomocą portalu zarządzania Azure, należy zaznaczyć szczegóły zmiennych, poświadczeń, certyfikatów, połączenia i harmonogramów.  Następnie trzeba ręcznie utworzyć dowolny aktywów, które są używane przez runbooks, którego możesz zaimportować do innego automatyzacji.

Za pomocą [poleceń cmdlet Azure](https://msdn.microsoft.com/library/dn690262.aspx) Pobieranie szczegółów niezaszyfrowanym składniki majątku i zapisać je do użytku w przyszłości lub utworzyć równoważne trwałe na innym koncie automatyzacji.

Nie można pobrać wartości zmiennych zaszyfrowane lub pole hasła poświadczeń przy użyciu poleceń cmdlet.  Jeśli nie znasz tych wartości, następnie można je odzyskać z działań aranżacji przy użyciu działań [Get-AutomationVariable](https://msdn.microsoft.com/library/dn940012.aspx) i [Get-AutomationPSCredential](https://msdn.microsoft.com/library/dn940015.aspx) .

Nie można eksportować certyfikaty z automatyzacji Azure.  Należy się upewnić, że wszystkie certyfikaty są dostępne poza Azure.

### <a name="dsc-configurations"></a>DSC konfiguracji

Konfiguracje można eksportować do plików skryptów za pomocą portalu zarządzania Azure lub polecenia cmdlet [AzureRmAutomationDscConfiguration eksportu](https://msdn.microsoft.com/library/mt603485.aspx) w programie Windows PowerShell. Tę konfigurację można zaimportować i używać na innym koncie automatyzacji.


##<a name="geo-replication-in-azure-automation"></a>Replikacja Geo w automatyzacji Azure

Replikacja Geo, standardowe w konta automatyzacji Azure kopię zapasową danych konta do innego regionu geograficznego dla nadmiarowości. Możesz wybrać region podstawowego podczas konfigurowania konta, a następnie pomocniczej region jest do niego przypisana automatycznie. Pomocniczego kopiowane z obszaru podstawowego są stale aktualizowane w przypadku utraty danych.  

W poniższej tabeli przedstawiono par dostępne głównego i pomocniczego region.

|Podstawowy            |Pomocnicze
| ---------------   |----------------
|Południowej centralnej Stany Zjednoczone   |Ameryka Północna centralnej w Stanach Zjednoczonych
|Stany Zjednoczone Wschodniej 2          |Centralny Stany Zjednoczone
|Europa Zachodnia        |Europa Północna
|Południowej wschodnioazjatyckie    |Wschodnioazjatyckie
|Japonia wschód         |Japonia zachód

W przypadku prawdopodobne, że są tracone dane podstawowy obszar Microsoft próbuje go odzyskać. Jeśli nie można odzyskać dane podstawowe, oznacza jest wykonywana w przypadku awarii geo i użytkowników, których dotyczy będą otrzymywać powiadomienia o tym przez swoją subskrypcję.

