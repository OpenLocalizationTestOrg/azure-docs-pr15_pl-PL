<properties 
    pageTitle="Instrukcje dotyczące konfiguracji pojazdu telemetrycznego analizy rozwiązanie szablon pulpitu nawigacyjnego PowerBI | Microsoft Azure" 
    description="Skorzystać z funkcji analizy Cortana uzyskanie wniosków w czasie rzeczywistym i przewidywanych ds pojazdu bo nawyków." 
    services="machine-learning" 
    documentationCenter="" 
    authors="bradsev" 
    manager="jhubbard" 
    editor="cgronlun" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/12/2016" 
    ms.author="bradsev" />


# <a name="vehicle-telemetry-analytics-solution-template-powerbi-dashboard-setup-instructions"></a>Pojazdu telemetrycznego analizy rozwiązanie szablon pulpitu nawigacyjnego PowerBI uzyskać instrukcje dotyczące konfiguracji

Ten **menu** linki do rozdziały w tym playbook. 

[AZURE.INCLUDE [cap-vehicle-telemetry-playbook-selector](../../includes/cap-vehicle-telemetry-playbook-selector.md)]


Rozwiązanie analizy telemetrycznego pojazdu ilustrację jak dealerów samochód, producentów samochodów i ubezpieczeń mogą korzystać z możliwości analizy Cortana uzyskanie w czasie rzeczywistym i przewidywanych wniosków na pojazdu zdrowie i obsługę kierowania nawyków ulepszenia w zakresie obszaru klienta, R & D i kampanii marketingowych. Ten dokument zawiera instrukcje krok po kroku dotyczące sposobu konfigurowania PowerBI raportów i pulpitów nawigacyjnych po wdrożeniu rozwiązanie w ramach subskrypcji. 


## <a name="prerequisites"></a>Wymagania wstępne
1.  Wdrażanie rozwiązania analizy telemetrycznego pojazdu, przechodząc do [https://gallery.cortanaanalytics.com/SolutionTemplate/Vehicle-Telemetry-Analytics-3](https://gallery.cortanaanalytics.com/SolutionTemplate/Vehicle-Telemetry-Analytics-3)  
2.  [Instalowanie programu Microsoft Power BI Desktop](http://www.microsoft.com/download/details.aspx?id=45331)
3.  [Azure subskrypcji](https://azure.microsoft.com/pricing/free-trial/). Jeśli nie masz subskrypcji usługi Azure, wprowadzenie Azure bezpłatnej subskrypcji
4.  Konto Microsoft PowerBI
    

## <a name="cortana-intelligence-suite-components"></a>Składniki pakietu analizy Cortana
Częścią szablonu rozwiązanie analizy telemetrycznego pojazdu następujących usług analiz Cortana wdrożonych w subskrypcji.

- **Koncentratory zdarzeń** dla ingesting miliony pojazdu telemetrycznego zdarzeń do Azure.
- **Analitycznym strumienia**s do uzyskania w czasie rzeczywistym wniosków na zdrowie pojazdu i będzie nadal występował te dane do długotrwałych miejsca do magazynowania dla bardziej rozbudowane analizy partię.
- **Nauka maszynowego** wykrywania anomalii w czasie rzeczywistym i przetwarzanie wsadowe uzyskanie wniosków przewidywanych.
- **Usługa HDInsight** jest użyć do przekształcania danych w większej skali
- **Factory danych** obsługuje aranżacji, Planowanie zarządzania zasobami i monitorowanie potoku przetwarzanie wsadowe.

**Power BI** zapewnia tego rozwiązania sformatowanego pulpitu nawigacyjnego w czasie rzeczywistym danych i wizualizacje przewidywanych analizy. 

Rozwiązanie korzysta z dwóch różnych źródeł danych: **sygnały symulowany pojazdu i diagnostyczne zestawu danych** i **pojazdu wykazu**.

Simulator telematyki pojazdu wchodzi w skład tego rozwiązania. Emituje informacje diagnostyczne, a sygnały odpowiadające stan pojazdu i prowadzenie deseniu w danym punkcie w czasie. 

Wykaz pojazdu jest odwołanie zestawu danych zawierających VIN mapowanie modelu


## <a name="powerbi-dashboard-preparation"></a>Przygotowanie PowerBI pulpitu nawigacyjnego

### <a name="deployment"></a>Wdrożenie

Po zakończeniu rozmieszczania powinien zostać wyświetlony poniższy diagram z wszystkie te elementy oznaczone na ZIELONO. 

- Aby przejść do odpowiedniego usług sprawdź, czy wszystkie te wdrożeniu pomyślnie, kliknij strzałkę w prawym górnym rogu zielony węzły.
- Aby pobrać pakiet simulator danych, kliknij strzałkę w prawym górnym bezpośrednio w węźle **Simulator telematyki pojazdu** . Zapisz i wyodrębnianie plików lokalnie na tym komputerze. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/1-deployed-components.png)

Teraz możesz przystąpić do konfigurowania pulpitu nawigacyjnego PowerBI z zaawansowanych wizualizacji uzyskanie w czasie rzeczywistym i nawyków przewidywanych rozeznanie pojazdu zdrowia i prowadzenie. Trwa około 45 minut na godzinę, aby utworzyć wszystkich raportów i skonfigurować pulpitu nawigacyjnego. 


### <a name="setup-power-bi-real-time-dashboard"></a>Pulpit nawigacyjny konfiguracji Power BI w czasie rzeczywistym

**Generowanie symulowany danych**

1. Na komputerze lokalnym przejdź do folderu, którego wyodrębnione pakietu Simulator telematyki pojazdu![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/2-vehicle-telematics-simulator-folder.png)
2.  Uruchom aplikację ***CarEventGenerator.exe***.
3.  Emituje informacje diagnostyczne, a sygnały odpowiadające stan pojazdu i prowadzenie deseniu w danym punkcie w czasie. To jest opublikowany wystąpienie Centrum zdarzeń Azure skonfigurowanego jako część wdrożenia.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/3-vehicle-telematics-diagnostics.png)
     
**Uruchom aplikację w czasie rzeczywistym pulpitu nawigacyjnego**

Rozwiązanie zawiera aplikację, która generuje w czasie rzeczywistym pulpitu nawigacyjnego w PowerBI. Ta aplikacja wykrywa wystąpienia zdarzenia Centrum, z której analizy strumieniu publikuje zdarzenia przez cały czas. Dla każdego zdarzenia, które otrzymuje tej aplikacji przetwarza danych przy użyciu punkt końcowy wyników nauki komputera żądanie-odpowiedź. Wynikowy zestaw danych jest opublikowany wypychanych PowerBI interfejsy API dla wizualizacji. 

Aby pobrać aplikację:

1.  Kliknij pozycję węzła PowerBI w widoku diagramu, a następnie kliknij pozycję **Pobierz aplikację pulpitu nawigacyjnego w czasie rzeczywistym**"łącza w okienku właściwości.![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard-new1.png)
2.  Wyodrębnianie i zapisywanie aplikacji lokalnie![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/4-real-time-dashboard-application.png)

3.  Uruchom aplikację **RealtimeDashboardApp.exe**
4.  Prawidłowe poświadczenia usługi Power BI, zaloguj się i kliknij przycisk **Zaakceptuj**
    
    ![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/5-sign-into-powerbi.png)
    
    ![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/6-powerbi-dashboard-permissions.png)


### <a name="configure-powerbi-reports"></a>Konfigurowanie raportów PowerBI
W czasie rzeczywistym raportów i pulpit nawigacyjny wykonanie zająć około 30-45 minut. Przejdź do [http://powerbi.com](http://powerbi.com) i logowania.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/6-1-powerbi-signin.png)

Nowy zestaw danych, jest generowany w usłudze Power BI. Kliknij pozycję **ConnectedCarsRealtime** zestawu danych.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/7-select-connected-cars-realtime-dataset.png)

Zapisz pusty raport, naciskając **klawisze Ctrl + s**.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/8-save-blank-report.png)

Podaj nazwę raportu *pojazdu telemetrycznego analizy w czasie rzeczywistym - raportów*.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/9-provide-report-name.png)

## <a name="real-time-reports"></a>Raporty w czasie rzeczywistym
Istnieją trzy raporty w czasie rzeczywistym w rozwiązaniu:

1.  Pojazdy w operacji
2.  Pojazdy wymagające konserwacji
3.  Statystyki dotyczące kondycji pojazdy

Możesz skonfigurować wszystkie trzy raporty w czasie rzeczywistym lub zatrzymywanie po każdym etapie i przejdź do następnej sekcji Konfigurowanie raportów partię. Zaleca się utworzenie trzy raporty wizualizacji pełny wniosków w czasie rzeczywistym ścieżki rozwiązanie.  

### <a name="1-vehicles-in-operation"></a>1. Operacja pojazdy
  
Kliknij dwukrotnie **Strona 1** , a następnie zmień jego nazwę na "Pojazdy w operacji"  
    ![Połączone samochody - pojazdy w operacji](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4a.png)  

Wybierz pole **vin** z **pola** i wybierz typ wizualizacji jako **"Karta"**.  

Wizualizacja karty jest tworzona, jak pokazano na rysunku.  
    ![Połączone samochody — wybierz vin](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4b.png)

Kliknij pusty obszar w celu dodania nowej wizualizacji.  

Wybieranie **pól Miasto** i **vin** z pól. Zmiana wizualizacji **"**mapę". Przeciągnij **vin** w obszarze wartości. Przeciągnij **Miasto** z pól do obszaru **legendy** .   
    ![Połączone samochody - wizualizacja karty](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4c.png)
  
Wybierz **format** sekcji z **wizualizacji**, kliknij **Tytuł** i zmień **tekst** na **"Pojazdy Operacja według miast"**.  
    ![Połączone samochody - pojazdy Operacja według miasta](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4d.png)   

Ostateczne wizualizacji wygląda jak pokazano na rysunku.    
    ![Samochody połączonego — wersja ostateczna wizualizacji](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4e.png)

Kliknij pusty obszar w celu dodania nowej wizualizacji.  

Wybieranie **pól Miasto** i **vin**, Zmień typ wizualizacji **Wykres kolumnowy grupowany**. Upewnij się, pole **Miasto** w **obszarze osi** i **vin** w **obszarze wartości**  

Sortowanie wykresu przez **"Liczba vin"**  
    ![Połączone samochody — liczba vin](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4f.png)  

Zmienianie **tytułu** wykresu na **"Pojazdy Operacja według miasta"**  

Kliknij sekcję, **Formatowanie** , a następnie wybierz **Kolory danych**, kliknij pozycję **"Na"** **Pokaż** wszystko  
    ![Połączone samochody — Pokaż wszystkie kolory danych](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4g.png)  

Zmienianie koloru pojedynczych miasta, klikając ikonę koloru.  
    ![Połączone samochody — Zmienianie kolorów](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4h.png)  

Kliknij pusty obszar w celu dodania nowej wizualizacji.  

Wybierz wizualizację **Wykres kolumnowy grupowany** z wizualizacji, przeciągnij pole **Miasto** w obszarze **osi** **modelu** w obszarze **legendy** i **vin** w obszarze **wartości** .  
    ![Połączone samochody — wykres kolumnowy grupowany](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4i.png)  
    ![Połączone samochody - renderowania](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4j.png)
  
Ponowne rozmieszczanie wszystkich wizualizacji na tej stronie, jak pokazano na rysunku.  
    ![Połączone samochody - wizualizacji](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4k.png)

Pomyślnie skonfigurowano raportu w czasie rzeczywistym "Pojazdy Operacja". Przejdź do następnego raportu w czasie rzeczywistym lub Zatrzymaj w tym miejscu i konfigurowanie pulpitu nawigacyjnego. 

### <a name="2-vehicles-requiring-maintenance"></a>2. pojazdy wymagające konserwacji
  
Kliknij pozycję ![Dodaj](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4add.png) Aby dodać nowy raport, zmień jego nazwę na **"pojazdy wymagające konserwacji"**

![Połączone samochody - pojazdy wymagające konserwacji](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4l.png)  

Zaznacz pole **vin** i zmienianie typu wizualizacji na **kartę**.  
    ![Połączone samochody - wizualizacja karty Vin](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4m.png)  

Mamy pole o nazwie "MaintenanceLabel" w zestawie danych. To pole może zawierać wartość "0" lub "1"." Ustawiana jest Azure maszynowego uczenia model obsługi administracyjnej jako część rozwiązania i zintegrowane ścieżką w czasie rzeczywistym. Wartość "1" oznacza, że pojazdu wymaga konserwacji. 

Aby dodać filtr **Poziom strony** do pokazywania pojazdy dane, które wymagają obsługi: 

1. Przeciągnij pole **"MaintenanceLabel"** do **Strony poziom filtry**.  
![Połączone samochody - filtry na poziomie strony](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n1.png)  

2. Kliknij menu **Podstawowe funkcje filtrowania** Prezentuj u dołu ekranu MaintenanceLabel strony poziom filtru.  
![Połączone samochody - podstawowe funkcje filtrowania](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n2.png)  

3.  Ustaw wartość filtru **"** 1"    
![Połączone samochody - wartości filtru](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n3.png)  


Kliknij pusty obszar w celu dodania nowej wizualizacji.  

Wybierz **Wykres kolumnowy grupowany** z wizualizacji  
![Połączone samochody - wizualizacja karty Vind](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4o.png)  
![Połączone samochody — wykres kolumnowy grupowany](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4p.png)

Przeciągnij pole **modelu** na obszar **oś** , **Vin** do obszaru **wartości** . Następnie Sortuj wizualizację według **liczby vin**.  Zmienianie **tytułu** wykresu na **"Pojazdy wymagające konserwacji przez model"**  

Przeciągnij pola **vin** do **Nasycenie kolorów** w **polach** ![pola](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4field.png) sekcja karcie **wizualizacji**  
![Połączone samochody - nasycenie kolorów](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4q.png)  

Zmienianie **Kolorów dane** w wizualizacjach z sekcji **Formatowanie**  
Zmienianie koloru Minimum do: **F2C812**  
Zmienianie koloru maksymalnie do: **FF6300**  
![Połączone samochody — zmiany kolorów](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4r.png)  
![Połączone samochody - nowych kolorów wizualizacji](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4s.png)  

Kliknij pusty obszar w celu dodania nowej wizualizacji.  

Wybierz **Wykres kolumnowy grupowany** z wizualizacji, przeciągnij pole **vin** do obszaru **wartości** , przeciągnij pole **Miasto** do obszaru **osi** . Sortowanie wykresu przez **"Liczba vin"**. Zmienianie **tytułu** wykresu na **"Pojazdy wymagające konserwacji według miasta"**   
![Połączone samochody - pojazdy wymagające konserwacji według miasta](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4t.png)  

Kliknij pusty obszar w celu dodania nowej wizualizacji.  

Wybierz pozycję wizualizacja **Karty wielu wierszy** z wizualizacji, przeciągnij **modelu** i **vin** do obszaru **pola** .  
![Połączone samochody — karta wielu wierszy](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4u.png)    

Zmienianie kolejności wszystkich wizualizacji, raport końcowy wygląda następująco:  
![Połączone samochody — karta wielu wierszy](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4v.png)  

Pomyślnie skonfigurowano raportu w czasie rzeczywistym "Pojazdy wymagające konserwacji". Przejdź do następnego raportu w czasie rzeczywistym lub Zatrzymaj w tym miejscu i konfigurowanie pulpitu nawigacyjnego. 

### <a name="3-vehicles-health-statistics"></a>3. statystyki dotyczące kondycji pojazdy
  
Kliknij pozycję ![Dodaj](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4add.png) Aby dodać nowy raport, zmień jego nazwę na **"pojazdy kondycji statystyki"**  

Wybierz wizualizację **Miernik** z wizualizacji, a następnie przeciągnij pole **szybkość** do obszarów **wartości, wartość minimalna, wartość maksymalna** .  
![Połączone samochody — karta wielu wierszy](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4w.png)  

Zmienianie agregacji domyślnej **szybkości** w **obszarze wartości** **średniej** 

Zmienianie agregacji domyślnej **szybkości** w **obszarze Minimum** **minimum**

Zmienianie agregacji domyślnej **szybkości** w **obszarze maksimum** **Maksymalna**

![Połączone samochody — karta wielu wierszy](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4x.png)  

Zmień **Tytuł miernik** **"Średniej prędkości"** 
 
![Połączone samochody - szerokość toru](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4y.png)  

Kliknij pusty obszar w celu dodania nowej wizualizacji.  

Podobnie Dodaj **Miernik** **oil aparat średnia**, **Średnia paliwo**i **Średnia aparat umiarkowanych**.  

Zmiana, która Agregacja domyślna pól w każdym miernik w powyżej procedura **"Średniej prędkości"** ocenić.

![Połączone samochody - mierniki](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4z.png)

Kliknij pusty obszar w celu dodania nowej wizualizacji.

Wybierz **linii i wykres kolumnowy grupowany** z wizualizacji, a następnie przeciągnij pole **Miasto** do **Udostępnionego osi**, przeciągnij **szybkości** **tirepressure i engineoil pól** do obszaru **Wartości w kolumnie** , zmień ich typ agregacji na **Średnia**. 

Przeciągnij pole **engineTemperature** do obszaru **Wartości wiersza** , Zmień typ agregacji **Średnia**. 

![Połączone samochody — pola wizualizacji](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4aa.png)

Zmienianie **tytułu** wykresu **"średniej prędkości, ciśnienia opony, oil aparat**i temperatury aparat".  

![Połączone samochody — pola wizualizacji](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4bb.png)

Kliknij pusty obszar w celu dodania nowej wizualizacji.

Wybierz wizualizację **mapy drzewa** z wizualizacji, przeciągnij pole **Model** do obszaru **grupy** , a następnie przeciągnij pole **MaintenanceProbability** do obszaru **wartości** .

Zmienianie **tytułu** wykresu na **"Modeli pojazdu wymagające konserwacji"**.

![Połączone samochody — Zmień tytuł wykresu](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4cc.png)

Kliknij pusty obszar w celu dodania nowej wizualizacji.

Wybierz pozycję **100% skumulowany wykres słupkowy** z wizualizacji, przeciągnij pole **Miasto** do obszaru **osi** i przeciągnij **MaintenanceProbability** **RecallProbability** pól do obszaru **wartości** .

![Połączone samochody — Dodawanie nowej wizualizacji](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4dd.png)

Kliknij przycisk **Formatuj**, wybierz **Kolory danych**i ustaw kolor **MaintenanceProbability** na wartość **"F2C80F"**.

Zmienianie **tytułu** wykresu **"prawdopodobieństwo z pojazdu konserwacji i odwoływanie według miasta"**.

![Połączone samochody — Dodawanie nowej wizualizacji](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ee.png)

Kliknij pusty obszar w celu dodania nowej wizualizacji.

Wybierz **Wykres warstwowy** z wizualizacji z wizualizacji, przeciągnij pole **Model** do obszaru **osi** , a następnie przeciągnij pola **engineOil, tirepressure, szybkość i MaintenanceProbability** do obszaru **wartości** . Zmień ich typ agregacji na **"Średni"**. 

![Połączone samochody - Zmień typ agregacji](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ff.png)

Zmienianie tytułu wykresu **"Obliczanie średniej wartości oil aparat, opony ciśnienia, szybkość i konserwacja prawdopodobieństwo przez model"**.

![Połączone samochody — Zmień tytuł wykresu](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4gg.png)

Kliknij pusty obszar w celu dodania nowej wizualizacji:

1. Wybierz wizualizację **Wykres punktowy** z wizualizacji.
2. Przeciągnij pole **Model** w obszarze **Szczegóły** i **Legenda** .
3. Przeciągnij pole **paliwo** do obszaru **osi x** , zmień agregację **Średnia**.
4. Przeciągnij **engineTemparature** na **obszar oś y**, zmień agregację **Średnia**
5. Przeciągnij pole **vin** do obszaru **rozmiar** .


![Połączone samochody — Dodawanie nowej wizualizacji](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4hh.png)

Zmienianie **tytułu** wykresu na **"Średnie paliwo, temperatury aparat przez Model"**.

![Połączone samochody — Zmień tytuł wykresu](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ii.png)

Raport końcowy będzie wyglądać tak jak pokazano poniżej.

![Połączony raport końcowy samochodów](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4jj.png)

### <a name="pin-visualizations-from-the-reports-to-the-real-time-dashboard"></a>Numer PIN wizualizacji z raportów w czasie rzeczywistym pulpitu nawigacyjnego
  
Utwórz pusty pulpit nawigacyjny, klikając ikonę plus obok pulpitów nawigacyjnych. Czy nazwa "Pojazdu telemetrycznego analizy pulpitu nawigacyjnego"

![Połączone samochody pulpitu nawigacyjnego](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.5.png)

Przypinanie wizualizacji z powyższych raportów do pulpitu nawigacyjnego. 
 
![Połączone samochody pulpitu nawigacyjnego](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.6.png)

Pulpit nawigacyjny powinna wyglądać następująco po trzy raporty są tworzone i odpowiadające im wizualizacji są przypięte do pulpitu nawigacyjnego. Jeśli nie utworzono wszystkich raportów, pulpitu nawigacyjnego może wyglądać inaczej. 

![Połączone samochody pulpitu nawigacyjnego](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-4.0.png)

Gratulacje! Pomyślnie utworzono w czasie rzeczywistym pulpitu nawigacyjnego. Podczas wykonywania CarEventGenerator.exe i RealtimeDashboardApp.exe, aktualizacje online powinny być widoczne na pulpicie nawigacyjnym. Należy trwa około 10 do 15 minut należy wykonać następujące czynności.

 
##  <a name="setup-power-bi-batch-processing-dashboard"></a>Konfigurowanie pulpitu nawigacyjnego przetwarzanie wsadowe Power BI

>[AZURE.NOTE] Trwa około dwie godziny (od pomyślnego wdrożenia) procesu przetwarzanie wsadowe kompleksowego zakończenia wykonanie i procesu warte roku wygenerowanych danych. Tak poczekać na zakończenie przetwarzania przed kontynuowaniem następnych kroków. 

**Pobierz plik projektanta PowerBI**

-   Wstępnie skonfigurowane plik projektanta PowerBI jest częścią wdrożenia
-   Kliknij pozycję węzła PowerBI w widoku diagramu, a następnie kliknij pozycję **Pobierz plik projektanta PowerBI** łącze w okienku właściwości![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/9.5-download-powerbi-designer.png)

-   Zapisywanie lokalnie

**Konfigurowanie raportów PowerBI**

-   Otwórz projektanta plik "VehicleTelemetryAnalytics - Report.pbix pulpitu" PowerBI pulpitu. Jeśli nie masz już, zainstaluj pulpitu PowerBI z [pulpitu PowerBI instalacji](http://www.microsoft.com/download/details.aspx?id=45331). 

-   Kliknij przycisk **Edytuj kwerendy**.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/10-edit-powerbi-query.png)

- Kliknij dwukrotnie **źródło**.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/11-set-powerbi-source.png)

- Zaktualizuj serwera parametry połączenia z serwerem Azure SQL, który został zainicjowany w ramach wdrożenia. Kliknij węzeł Azure SQL na diagramie, a następnie wyświetlać nazwę serwera w okienku właściwości.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/11.5-view-server-name.png)

- Pozostaw *connectedcar* **bazy danych** .

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/12-set-powerbi-database.png)

- Kliknij **przycisk OK**.
- Zostanie wyświetlona karta **poświadczenia systemu Windows** wybrane przez domyślny, zmień go do **bazy danych poświadczeń** , klikając na karcie **bazy danych** po prawej stronie służącym.
- Podanie **nazwy użytkownika** i **hasła** bazy danych SQL Azure określony podczas jego instalacji wdrożenia.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/13-provide-database-credentials.png)

- Kliknij przycisk **Połącz**
- Powtórz powyższe czynności dla każdej z trzech kwerend pozostały w prawym okienku, a następnie zaktualizuj szczegóły połączenia źródła danych.
- Kliknij pozycję **Zamknij i załaduj**. Zestawy Power BI Desktop pliku danych są połączone tabele bazy danych SQL Azure.
- **Zamknij** Plik programu Power BI Desktop.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/14-close-powerbi-desktop.png)

- Kliknij przycisk **Zapisz** , aby zapisać zmiany. 
 
Wszystkie raporty odpowiadające ścieżkę przetwarzanie wsadowe w rozwiązaniu została skonfigurowana. 


## <a name="upload-to-powerbicom"></a>Przekazywanie do *powerbi.com*
 
1.  Przejdź do portalu sieci web PowerBI w http://powerbi.com i zaloguj.
2.  Kliknij przycisk **Pobierz dane**  
3.  Przekaż plik Power BI Desktop.  
4.  Aby przekazać, kliknij pozycję **Pobieranie danych -> pliki -> plik lokalny**  
5.  Przejdź do **"VehicleTelemetryAnalytics — Report.pbix pulpitu"**  
6.  Po przekazaniu pliku zostanie wyświetlony ponownie do obszaru roboczego Power BI.  

Zestaw danych, raportów i pusty pulpit nawigacyjny zostanie utworzony dla Ciebie.  
 

Przypinanie wykresy do istniejących pulpitu nawigacyjnego **Pojazdu Telemetryczny analizy pulpit nawigacyjny** w **Usłudze Power BI**. Kliknij pozycję pusty pulpit nawigacyjny utworzony w poprzednim przykładzie, a następnie przejdź do sekcji **raportów** kliknij przekazany raportu.  

![PowerBI.com telemetrycznego pojazdu](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard1.png) 


**Należy zauważyć, że raport składa się z sześciu stron:**  
Strona 1: Gęstość pojazdu  
Strona 2: Pojazdu w czasie rzeczywistym kondycji  
Strona 3: Skrupulatnie zmiennych pojazdy   
Strona 4: Przypomnieć pojazdy  
5 strony: Wydajność pracy pojazdy paliwo  
6 strony: Logo firmy Contoso  

![PowerBI.com samochody połączonego](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard2.png)
 

**Strona 3-w**, numer pin następujące czynności:  

1.  Liczba VIN  
    ![PowerBI.com samochody połączonego](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard3.png) 

2.  Skrupulatnie zmiennych pojazdy przez model — wykres Wodospadowy  
    ![Telemetrycznego pojazdu - numer Pin wykresy 4](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard4.png)

**Z 5 strony**, numer pin następujące czynności: 
 
1.  Liczba vin    
    ![Telemetrycznego pojazdu - numer Pin wykresy 5](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard5.png)  
2.  Paliwo pojazdy wydajność przez model: wykres kolumnowy grupowany  
    ![Telemetrycznego pojazdu - numer Pin wykresy 6](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard6.png)

**Od strony 4**, numer pin następujące czynności:  

1.  Liczba vin  
    ![Telemetrycznego pojazdu - numer Pin wykresy 7](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard7.png) 

2.  Wzór odwołanej pojazdy według miasta,: Mapa drzewa  
    ![Telemetrycznego pojazdu - numer Pin wykresy 8](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard8.png)  

**Z 6 strony**, numer pin następujące czynności:  

1.  Logo firmy Contoso Motors  
    ![Telemetrycznego pojazdu - numer Pin wykresy 9](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard9.png)

**Organizowanie pulpitu nawigacyjnego**  

1.  Przejdź do pulpitu nawigacyjnego
2.  Umieść wskaźnik myszy nad każdym wykresu i Zmień nazwę, którą według nazw opisane poniżej obrazu Tworzenie pulpitu nawigacyjnego. Również poruszanie wykresy wyglądała tak jak poniżej pulpitu nawigacyjnego.  
    ![Telemetrycznego pojazdu - organizowanie pulpitu nawigacyjnego 2](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-organize-dashboard2.png)  
    ![PowerBI.com telemetrycznego pojazdu](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard.png)
3.  Jeśli użytkownik utworzył wszystkich raportów, wymienione w tym dokumencie, ostateczne ukończonego pulpitu nawigacyjnego powinna wyglądać podobnie do poniższej ilustracji. 

![Telemetrycznego pojazdu - organizowanie pulpitu nawigacyjnego 2](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-organize-dashboard3.png)

Gratulacje! Pomyślnie utworzono raportów i nawyków pulpitu nawigacyjnego w celu uzyskania w czasie rzeczywistym, przewidywanych i partii wniosków na pojazdu zdrowie i prowadzenie.  
