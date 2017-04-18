<properties 
    pageTitle="Monitorowanie i zarządzanie nimi procesy Factory danych Azure" 
    description="Dowiedz się, jak za pomocą monitorowania i zarządzania aplikacji monitorowanie i zarządzanie danymi Azure fabryki procesy." 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/06/2016" 
    ms.author="spelluru"/>

# <a name="monitor-and-manage-azure-data-factory-pipelines-using-new-monitoring-and-management-app"></a>Monitorowanie i zarządzanie nimi procesy Factory danych Azure za pomocą nowych monitorowania i zarządzania aplikacji
> [AZURE.SELECTOR]
- [Za pomocą Azure portalu Azure](data-factory-monitor-manage-pipelines.md)
- [Za pomocą monitorowania i zarządzania aplikacji](data-factory-monitor-manage-app.md)

W tym artykule opisano, jak można monitorować, zarządzanie i debugowania usługi procesy oraz tworzenia alertów otrzymywanie powiadomień o na błędy przy użyciu **monitorowania i zarządzania aplikacji**. Możesz też obejrzeć następujące klip wideo, aby informacje o korzystaniu z monitorowania i zarządzania aplikacji.
   

> [AZURE.VIDEO azure-data-factory-monitoring-and-managing-big-data-piplines]
      
## <a name="launching-the-monitoring-and-management-app-a"></a>Uruchamianie monitorowania i zarządzania aplikacji
Aby uruchomić Monitor i zarządzania aplikacji, kliknij Kafelek **monitorowania i zarządzania** na karta **FACTORY danych** dla firmie danych.

![Monitorowanie kafelków na stronie głównej Factory danych](./media/data-factory-monitor-manage-app/MonitoringAppTile.png) 

Powinien pojawić się ekran monitorowania i zarządzania aplikacji uruchomione w osobnym karty i oknie.  

![Monitorowania i zarządzania aplikacji](./media/data-factory-monitor-manage-app/AppLaunched.png)

> [AZURE.NOTE] Jeśli zobaczysz, że przeglądarki sieci web zatrzymuje się na "Autoryzowanie...", wyłącz i wyczyść pole wyboru ustawienie **Blokowanie plików cookie innych firm i witryny danych** (lub) pozostaw je włączony i utworzyć wyjątek **login.microsoftonline.com** , a następnie spróbuj ponownie uruchamianie aplikacji.


Jeśli działanie systemu windows na liście u dołu nie jest widoczna, kliknij przycisk **Odśwież** na pasku narzędzi, aby odświeżyć listę. Ponadto ustaw właściwych wartości dla filtrów **czas rozpoczęcia** i **czas zakończenia** .  


## <a name="understanding-the-monitoring-and-management-app"></a>Opis monitorowania i zarządzania aplikacji
Dostępne są trzy karty (**Eksploratora zasobów**, **Monitorowanie widoków**i **alertów**) po lewej stronie i pierwszą kartę (Explorer zasobu) jest zaznaczona domyślnie. 

### <a name="resource-explorer"></a>Eksplorator zasobów
Pojawi się następujące czynności: 

- Eksplorator zasobów **Widok drzewa** w okienku po lewej stronie.
- **Widok diagramu** u góry.
- Lista **Windows aktywności** u dołu w środkowym okienku.
- **Właściwości**/**Aktywności okna Eksploratora** karty w okienku po prawej stronie. 

W Eksploratorze zasobów widać wszystkie zasoby factory danych w widoku drzewa (procesy, zestawy danych połączonych usług). Po wybraniu obiektu w Eksploratorze zasobu okaże się następujące czynności: 

- wyróżniona skojarzonej jednostce Factory danych w widoku diagramu.
- skojarzony aktywności systemu windows (kliknij [tutaj](data-factory-scheduling-and-execution.md) Aby uzyskać informacje o aktywności systemu windows) są wyróżniane na liście Windows aktywności u dołu.  
- właściwości zaznaczonego obiektu w oknie właściwości w okienku po prawej stronie. 
- Definicja JSON zaznaczonego obiektu w razie potrzeby. Na przykład: połączone usługi lub zestawu danych lub potok. 

![Eksplorator zasobów](./media/data-factory-monitor-manage-app/ResourceExplorer.png)

Zobacz artykuł [planowania i wykonanie](data-factory-scheduling-and-execution.md) , aby szczegółowe informacje dotyczące aktywności okna pojęć związanych. 

### <a name="diagram-view"></a>Widok diagramu
Widok diagramu factory danych zawiera jednego okienka szkła do monitorowania i zarządzania factory danych i jego aktywów. Po wybraniu jednostki Factory danych (zestawu danych i planowana) w widoku diagramu okaże się następujące czynności:
 
- jednostki factory danych jest zaznaczony w widoku drzewa
- windows skojarzonego działania są wyróżniane na liście Windows aktywności.
- właściwości zaznaczonego obiektu w oknie dialogowym właściwości

Po włączeniu proces (nie w stanie wstrzymania) są wyświetlane zieloną linią. 

![Uruchamianie procesu](./media/data-factory-monitor-manage-app/PipelineRunning.png)

Zwróć uwagę, że znajdują się trzy przyciski poleceń procesu w widoku diagramu. Drugi przycisk umożliwia wstrzymać proces. Wstrzymywanie zakończenie działania uruchomione i nie umożliwiać przejdź do zakończenia. Trzeci przycisk można wstrzymać proces i kończy istniejące wykonywanie działań. Pierwszy przycisk życiorysy proces. Po zatrzymaniu wskaźnika planowaną sprzedażą, można zauważyć zmiana koloru procesu sąsiadująco w następujący sposób.

![Wstrzymaj/Wznów na kafelku](./media/data-factory-monitor-manage-app/SuspendResumeOnTile.png)

Możesz zaznacz wiele co najmniej dwa procesy (za pomocą klawiszy CTRL) i za pomocą przycisków pasek polecenia Wstrzymaj/Wznów procesy wiele w danej chwili.

![Wstrzymaj/Wznów na pasku poleceń](./media/data-factory-monitor-manage-app/SuspendResumeOnCommandBar.png)

Wszystkie działania w potoku, widać kafelków planowana prawym przyciskiem myszy, a następnie klikając polecenie **Otwórz potoku**.

![Otwórz menu planowana](./media/data-factory-monitor-manage-app/OpenPipelineMenu.png)

W widoku otwarty potok widać wszystkie działania w potoku. W tym przykładzie jest tylko jedno działanie: kopiowanie działanie. Aby wrócić do poprzedniego widoku, kliknij nazwę factory danych w menu łączy do stron nadrzędnych u góry.

![Otwarty potok](./media/data-factory-monitor-manage-app/OpenedPipeline.png)

W widoku planowana po kliknięciu zestaw danych wyjściowych lub gdy umieść wskaźnik myszy na dataset wynik zobaczysz wyskakujące aktywności systemu Windows dla tego zestawu danych.

![Menu podręczne Windows aktywności](./media/data-factory-monitor-manage-app/ActivityWindowsPopup.png)

Możesz kliknąć okno aktywności, aby wyświetlić szczegółowe informacje dla niej w oknie **Właściwości** w okienku po prawej stronie. 

![Właściwości okna aktywności](./media/data-factory-monitor-manage-app/ActivityWindowProperties.png)

W okienku po prawej stronie przejdź do karty **Aktywności okna Eksploratora** , aby wyświetlić więcej szczegółów.

![Działania okno Eksploratora](./media/data-factory-monitor-manage-app/ActivityWindowExplorer.png) 

Umożliwia wyświetlenie **rozpoznawania zmiennych** dla każdej serii aktywności będą próbować w sekcji **prób** . 

![Zmienne rozwiązany](./media/data-factory-monitor-manage-app/ResolvedVariables.PNG)

Przejdź na kartę **skrypt** , aby zobaczyć definicję skrypt JSON dla zaznaczonego obiektu.   

![Karta skryptu](./media/data-factory-monitor-manage-app/ScriptTab.png)

Możesz zobaczyć aktywności systemu windows w trzech miejscach:

- Menu podręczne Windows aktywności w widoku diagramu (środkowe okienko).
- Działania okna Eksploratora w okienku po prawej stronie.
- Lista Windows działań w dolnym okienku.

W podręcznym Windows aktywności i aktywności okna Eksploratora możesz przewinąć do poprzedniego tygodnia i następny tydzień za pomocą strzałek lewą i prawą.

![Działania okna Eksploratora od lewej i prawej strzałki](./media/data-factory-monitor-manage-app/ActivityWindowExplorerLeftRightArrows.png)

W dolnej części widoku diagramu zobaczysz przyciski Powiększ Powiększ, Pomniejsz, w celu dopasowania, powiększenie 100%, zablokować układ. Przycisk układ blokowania zapobiega przypadkowo przenoszenie tabel i procesy w widoku diagramu i jest domyślnie włączone. Możesz wyłączyć to ustawienie i przenoszenie obiektów w schemacie. Możesz wyłączyć tę OPCJĘ, możesz korzystać przycisku ostatniej ma automatycznie tabel i procesy. Można również powiększyć / Pomniejsz przy użyciu kółka myszy.

![Polecenia Powiększenie widoku diagramu](./media/data-factory-monitor-manage-app/DiagramViewZoomCommands.png)


### <a name="activity-windows-list"></a>Lista czynności systemu Windows
Lista windows działań w dolnej części okienka drugie imię Wyświetla wszystkie okna aktywności dla zestawu danych, wybrane w Eksploratorze zasobów lub widok diagramu. Domyślnie lista jest w kolejności malejącej, co oznacza, że wyświetlone najnowsze okno aktywności u góry. 

![Lista czynności systemu Windows](./media/data-factory-monitor-manage-app/ActivityWindowsList.png)

Tej listy nie odświeżać automatycznie, dlatego skorzystaj przycisk Odśwież na pasku narzędzi, aby ręcznie odświeżyć go.  


Windows aktywności może być w jednym z następujących statusów:

<table>
<tr>
    <th align="left">Stan</th><th align="left">Podstanu</th><th align="left">Opis</th>
</tr>
<tr>
    <td rowspan="8">Oczekiwanie</td><td>ScheduleTime</td><td>W oknie aktywności uruchomić nie nadszedł czas.</td>
</tr>
<tr>
<td>DatasetDependencies</td><td>Zależności nadrzędny nie są gotowe.</td>
</tr>
<tr>
<td>ComputeResources</td><td>Zasoby komputerowe nie są dostępne.</td>
</tr>
<tr>
<td>ConcurrencyLimit</td> <td>Wszystkie wystąpienia aktywności są zajęte, z systemem windows innych działań.</td>
</tr>
<tr>
<td>ActivityResume</td><td>Działanie została wstrzymana i nie można uruchomić windows aktywności, dopóki nie jest wznawiane.</td>
</tr>
<tr>
<td>Spróbuj ponownie</td><td>Wykonanie działania zostanie wykonana ponownie.</td>
</tr>
<tr>
<td>Sprawdzanie poprawności</td><td>Sprawdzanie poprawności nie została jeszcze rozpoczęta.</td>
</tr>
<tr>
<td>ValidationRetry</td><td>Oczekiwanie na sprawdzania poprawności ponowienia.</td>
</tr>
<tr>
<tr
<td rowspan="2">Trakcie wykonywania</td><td>Sprawdzanie poprawności</td><td>Sprawdzanie poprawności informacji o postępie.</td>
</tr>
<td></td>
<td>Okno aktywności jest przetwarzana.</td>
</tr>
<tr>
<td rowspan="4">Nie powiodła się</td><td>Upłynął limit czasu</td><td>Wykonanie trwała dłużej niż którego jest dozwolony przez to działanie.</td>
</tr>
<tr>
<td>Anulowane</td><td>Anulowana przez działanie użytkownika.</td>
</tr>
<tr>
<td>Sprawdzanie poprawności</td><td>Sprawdzanie poprawności nie powiodło się.</td>
</tr>
<tr>
<td></td><td>Nie można wygenerować i/lub sprawdzić poprawność oknie czynność.</td>
</tr>
<td>Gotowe</td><td></td><td>Okno aktywności jest gotowy do zużycie.</td>
</tr>
<tr>
<td>Pominięte</td><td></td><td>Okno działanie nie jest przetwarzana.</td>
</tr>
<tr>
<td>Brak</td><td></td><td>Okno działania, które używane z inny stan, ale zostało zresetowane.</td>
</tr>
</table>


Po kliknięciu okna działania na liście zobaczysz szczegółowe informacje o go w oknie **Eksploratora Windows aktywności** lub **Właściwości** po prawej stronie.

![Działania okno Eksploratora](./media/data-factory-monitor-manage-app/ActivityWindowExplorer-2.png)

### <a name="refresh-activity-windows"></a>Odświeżanie aktywności systemu windows  
Szczegółowe informacje nie są automatycznie odświeżane, aby używać **odświeżanie** przycisk (drugi przycisk) na pasku poleceń, aby ręcznie odświeżyć listę działania systemu windows.  
 

### <a name="properties-window"></a>Okno właściwości
Okno właściwości znajduje się w prawej części aplikacji monitorowania i zarządzania. 

![Okno właściwości](./media/data-factory-monitor-manage-app/PropertiesWindow.png)

Wyświetla właściwości elementu wybranego w Eksploratorze zasobów (widok drzewa) (lub) diagram widoku (lub) lista windows czynności. 

### <a name="activity-window-explorer"></a>Działania okno Eksploratora

**Działania okna** Eksplorator znajduje się w okienku prawej monitorowania i zarządzania aplikacji. Wyświetla szczegółowe informacje o oknie czynność, wybrane w podręcznym Windows aktywności lub na liście aktywności systemu Windows. 

![Działania okno Eksploratora](./media/data-factory-monitor-manage-app/ActivityWindowExplorer-3.png)

Do innego okna aktywności można przełączać, klikając go w widoku Kalendarz u góry. Można też użyć **Strzałka w lewo**/przyciski**strzałki w prawo** u góry aktywności systemu Windows, z poprzedniego/następnego tygodnia.

Umożliwia przyciski paska narzędzi w dolnym okienku, aby **ponownie uruchomić** okno działań lub **Odśwież** dane w okienku. 

### <a name="script"></a>Skrypt 
Karta **skrypt** do wyświetlania definicji JSON wybranego obiektu Factory danych (usługi połączone, zestawu danych i planowana). 

![Karta skryptu](./media/data-factory-monitor-manage-app/ScriptTab.png)

## <a name="using-system-views"></a>Korzystanie z widoków systemu
Monitorowania i zarządzania aplikacji zawiera widoków gotowych system (**ostatniej aktywności systemu windows**, **windows aktywności nie powiodło się**, **W trakcie działania systemu windows**), które umożliwia wyświetlenie ostatnio używane-nie powiodło się w trakcie działania systemu windows dla firmie danych. 

Przejdź na kartę **Monitorowania widoków** po lewej stronie, klikając go. 

![Karta widoki monitorowania](./media/data-factory-monitor-manage-app/MonitoringViewsTab.png)

Obecnie są obsługiwane trzy widoki systemowe. Wybierz odpowiednią opcję, aby wyświetlić ostatnie aktywności systemu windows (lub) systemu windows nie powiodło się aktywności (lub) systemu windows w trakcie wykonywania działania na liście aktywności Windows (u dołu okienka drugie imię). 

Po zaznaczeniu opcji **ostatniej aktywności systemu windows** , zobacz ostatniej aktywności okien w kolejności malejącej **Ostatnia próba czasu**. 

Za pomocą widoku **nie powiodło się działania w systemie windows** Aby wyświetlić wszystkie okna aktywności nie powiodło się na liście. Zaznacz okno aktywności nie powiodło się na liście, aby wyświetlić szczegółowe informacje o go w oknie **Właściwości** (lub) **Aktywności okna Eksploratora**. Możesz również pobrać wszelkie dzienniki okna aktywności nie powiodło się. 


## <a name="sorting-and-filtering-activity-windows"></a>Sortowanie i filtrowanie aktywności systemu windows
Zmienianie ustawień **czas rozpoczęcia** i **czas zakończenia** na pasku poleceń do systemu windows działania filtru. Po zmianie godzinę rozpoczęcia i zakończenia, kliknij przycisk obok czas zakończenia, aby odświeżyć listę działania systemu Windows.

![Czas rozpoczęcia i zakończenia](./media/data-factory-monitor-manage-app/StartAndEndTimes.png)

> [AZURE.NOTE] Obecnie zawsze są dostępne w formacie UTC w monitorowania i zarządzania aplikacji. 

Na **liście aktywności systemu Windows**kliknij nazwę kolumny (na przykład: stanu). 

![Menu kolumny listy Windows aktywności](./media/data-factory-monitor-manage-app/ActivityWindowsListColumnMenu.png)

Możesz wykonać następujące czynności:

- Sortowanie w kolejności rosnącej.
- Sortowanie w kolejności malejącej.
- Filtrowanie według jednej lub wielu wartości (gotowe, oczekiwania itp.)

Po określeniu filtru w kolumnie jest widoczny przycisk Filtr dla tej kolumny wskazać, że wartości w kolumnie są filtrowane wartości. 

![Filtrowanie w kolumnie listy aktywności systemu Windows](./media/data-factory-monitor-manage-app/ActivityWindowsListFilterInColumn.png)

Za pomocą okna podręcznego samej zostać wyczyszczone filtry. Aby wyczyścić wszystkie filtry na liście aktywności systemu windows, kliknij przycisk Wyczyść filtr na pasku poleceń. 

![Czyszczenie wszystkich filtrów na liście aktywności systemu Windows](./media/data-factory-monitor-manage-app/ClearAllFiltersActivityWindowsList.png)


## <a name="performing-batch-actions"></a>Wykonywanie akcji partii

### <a name="rerun-selected-activity-windows"></a>Uruchom ponownie wybrane działanie systemu windows
Wybierz polecenie Okno aktywności, kliknij strzałkę w dół obok przycisku paska pierwszego polecenia i wybierz pozycję **Uruchom ponownie** / **ponownie z rzeki w potoku**. Po zaznaczeniu opcji **Uruchom ponownie z rzeki w potoku** umożliwia ponowne wykonanie także wszystkie okna nadrzędnego aktywności. 
    ![Uruchom ponownie okno aktywności](./media/data-factory-monitor-manage-app/ReRunSlice.png)

Możesz również zaznacz wiele okien działania na liście i ponownie je w tym samym czasie. Chcesz filtrować aktywności systemu windows na podstawie stanu (na przykład: **Niepowodzenie**) a następnie ponownie uruchom systemu windows nie powiodło się aktywności po usunięciu problem, który powoduje windows działanie kończy się niepowodzeniem. Zobacz następną sekcję, aby uzyskać szczegółowe informacje o filtrowaniu aktywności systemu windows na liście.  

### <a name="pauseresume-multiple-pipelines"></a>Wstrzymaj/Wznów wielu procesy
Możesz zaznacz wiele co najmniej dwa procesy (za pomocą klawiszy CTRL) i za pomocą przycisków pasek polecenia (wyróżnione w czerwonym prostokącie na poniższej ilustracji) Wstrzymaj/Wznów je pojedynczo.

![Zawieszenia-Życiorys na pasku poleceń](./media/data-factory-monitor-manage-app/SuspendResumeOnCommandBar.png)

## <a name="creating-alerts"></a>Tworzenie alertów 
Na stronie Alerty pozwala utworzyć alert, wyświetlanie i edytowanie i usuwanie istniejące alerty. Możesz też wyłączyć i włączyć alert. Aby wyświetlić stronę alertów, kliknij kartę alerty.

![Karta alertów](./media/data-factory-monitor-manage-app/AlertsTab.png)

### <a name="to-create-an-alert"></a>Aby utworzyć alert

1. Kliknij przycisk **Dodaj Alert** , aby dodać alert. Zostanie wyświetlona strona szczegóły. 

    ![Tworzenie alertów — Strona Szczegóły](./media/data-factory-monitor-manage-app/CreateAlertDetailsPage.png)
1. Określ **nazwę** i **Opis** alertu, a następnie kliknij przycisk **Dalej**. Zobacz stronę **Filtry** .

    ![Tworzenie alertów - filtry strony](./media/data-factory-monitor-manage-app/CreateAlertFiltersPage.png)

2. Zaznacz **zdarzenie**, **status**i **podstanu** (opcjonalnie) na którym ma zostać usługę Factory danych alert, a następnie kliknij przycisk **Dalej**. Zobacz stronę **adresatów** .

    ![Tworzenie alertów — strona adresatów](./media/data-factory-monitor-manage-app/CreateAlertRecipientsPage.png) 
3. Wybierz opcję **Administratorzy subskrypcję poczty E-mail** i/lub wprowadź **dodatkowe administratora poczty e-mail**, a następnie kliknij przycisk **Zakończ**. Powinien zostać wyświetlony alert na liście. 
    
    ![Lista alertów](./media/data-factory-monitor-manage-app/AlertsList.png)

Na liście alertów za pomocą przycisków skojarzone z alertem aby Edycja i Usuń i włączyć/wyłączyć alert. 

### <a name="eventstatussubstatus"></a>Zdarzenia i stan/podstanu
Poniższa tabela zawiera listę dostępnych zdarzenia statusy (i podstany).

Nazwa zdarzenia | Stan | Stan Sub
-------------- | ------ | ----------
Uruchamianie wprowadzenie aktywności | Wprowadzenie | Uruchamianie
Uruchamianie gotowego aktywności | Zakończyła się pomyślnie | Zakończyła się pomyślnie 
Uruchamianie gotowego aktywności | Nie powiodła się| Alokacja zasobów nie powiodło się<br/><br/>Wykonanie nie powiodło się<br/><br/>Przekroczono limit czasu<br/><br/>Sprawdzanie poprawności nie powiodło się<br/><br/>Porzucone.
Tworzenie wprowadzenie klaster HDI na żądanie | Wprowadzenie | &nbsp; |
Klaster HDI na żądanie został utworzony pomyślnie | Zakończyła się pomyślnie | &nbsp; |
Klaster HDI na żądanie usunięte | Zakończyła się pomyślnie | &nbsp; |
### <a name="to-editdeletedisable-an-alert"></a>Aby Edytuj lub usuń/wyłączyć alertu


![Przyciski alertów](./media/data-factory-monitor-manage-app/AlertButtons.png)



    
 


