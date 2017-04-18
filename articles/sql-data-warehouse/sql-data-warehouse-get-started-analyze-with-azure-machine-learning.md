<properties
   pageTitle="Analizowanie danych za pomocą Azure maszynowego uczenia | Microsoft Azure"
   description="Do utworzenia przewidywanych maszynowego uczenia modelu, na podstawie danych przechowywanych w magazynie danych SQL Azure za pomocą nauki maszynowego Azure."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="kevinvngo"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/14/2016"
   ms.author="kevin;barbkess;sonyama"/>

# <a name="analyze-data-with-azure-machine-learning"></a>Analizowanie danych za pomocą nauki maszynowego Azure

> [AZURE.SELECTOR]
- [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
- [Nauka Azure komputera](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
- [Programu Visual Studio](sql-data-warehouse-query-visual-studio.md)
- [sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 

Ten samouczek używa Azure maszynowego uczenia do utworzenia przewidywanych maszynowego uczenia modelu, na podstawie danych przechowywanych w magazynie danych SQL Azure. W szczególności to tworzy docelowej kampanii marketingowej dla Adventure Works sklep roweru przy przewidywania, jeśli klienta prawdopodobnie kupić roweru, czy nie.

> [AZURE.VIDEO integrating-azure-machine-learning-with-azure-sql-data-warehouse]


## <a name="prerequisites"></a>Wymagania wstępne
Aby kroków ten samouczek, należy następująco:

- Program SQL Data Warehouse wstępnie załadowane AdventureWorksDW przykładowych danych. Do zapewniania obsługi to, zobacz [Tworzenie magazynu danych SQL][] i wybierz pozycję, aby załadować dane przykładowe. Jeśli już masz magazynu danych, ale nie masz przykładowe dane, możesz [ręcznie ładowanie przykładowych danych][].

## <a name="1-get-data"></a>1. Pobierz dane
Dane znajdują się w widoku dbo.vTargetMail AdventureWorksDW bazy danych. Czytanie danych:

1. Zaloguj się do [nauki maszynowego Azure studio][] i kliknij pozycję Mój doświadczeniami.
2. Kliknij pozycję **+ Nowy** i wybierz pozycję **Pusty doświadczenia**.
3. Wprowadź nazwę dla swojego doświadczenia: przeznaczona marketingowych.
4. Przeciągnij moduł **czytnika** z poziomu okienka moduły na obszar roboczy.
5. Określ szczegóły bazy danych SQL magazynu danych w okienku właściwości.
6. Określanie bazy danych **kwerendy** do czytania wskazane dane.

```sql
SELECT [CustomerKey]
  ,[GeographyKey]
  ,[CustomerAlternateKey]
  ,[MaritalStatus]
  ,[Gender]
  ,cast ([YearlyIncome] as int) as SalaryYear
  ,[TotalChildren]
  ,[NumberChildrenAtHome]
  ,[EnglishEducation]
  ,[EnglishOccupation]
  ,[HouseOwnerFlag]
  ,[NumberCarsOwned]
  ,[CommuteDistance]
  ,[Region]
  ,[Age]
  ,[BikeBuyer]
FROM [dbo].[vTargetMail]
```

Uruchom doświadczenia, klikając pozycję **Uruchom** w obszarze roboczym doświadczenia.
![Uruchamianie doświadczenia][1]


Po zakończeniu doświadczenia uruchomiony pomyślnie kliknij port wyjściowy w dolnej części modułu czytnika i wybierz **Wizualizacja** , aby wyświetlić zaimportowane dane.
![Wyświetlanie zaimportowanych danych][3]


## <a name="2-clean-the-data"></a>2. Wyczyść dane
Aby wyczyścić dane, upuść kilka kolumn, które nie są odpowiednie dla modelu. Aby to zrobić:

1. Przeciągnij moduł **Projektu kolumny** do obszaru roboczego.
2. Kliknij **selektor kolumny uruchamianie** w okienku właściwości, aby określić kolumny, które chcesz usunąć.
![Kolumny projektu][4]

3. Wykluczanie dwóch kolumn: CustomerAlternateKey i ciąg Kluczgeografii.
![Usuń niepotrzebne kolumny][5]


## <a name="3-build-the-model"></a>3. Tworzenie modelu
Firma Microsoft będzie dzielenie danych 80-20: 80%, aby przeszkolenie maszynowego uczenia modelu i 20%, aby przetestować modelu. Firma tego problemu binarne klasyfikacji za pomocą algorytmów "Dwie klasy".

1. Przeciągnij moduł **podziału** na obszar roboczy.
2. Wprowadź 0,8 część wierszy w pierwszym zestawie danych wyjściowych danych w okienku właściwości.
![Dane można podzielić zestawu badania i szkolenia][6]
3. Przeciągnij moduł **Klasy dwóch zwiększane drzewa decyzji** na obszar roboczy.
4. Przeciągnij moduł **Modelu pociągu** na obszar roboczy i określ danych wejściowych. Następnie kliknij **Uruchamianie selektora kolumny** w okienku właściwości.
      - Najpierw wprowadzania: algorytm ML.
      - Drugi wprowadzania: danych do szkoleniem algorytmu na.
![Łączenie moduł pociągu modelu][7]
5. Zaznacz kolumnę **Nabywcaroweru** jako kolumnę do prognozowania.
![Zaznacz kolumnę do prognozowania][8]


## <a name="4-score-the-model"></a>4. wynik modelu
Będzie teraz przetestować jak modelu wykonuje test danych. Firma Microsoft będzie porównanie algorytmu nasz wybór z różnych algorytmu, aby zobaczyć, który wykonuje lepiej.

1. Przeciągnij moduł **Modelu wynik** do obszaru roboczego.
    Najpierw wprowadzania: szkolenie wprowadzania drugiego modelu: testowanie danych ![wynik modelu][9]
2. Przeciągnij **Maszyny punktu klasyfikatora Bayesa dwóch zajęć** na obszar roboczy doświadczenia. Firma Microsoft porównuje sposób wykonywania tego algorytmu w porównaniu z systemem drzewa decyzji zwiększane dwóch zajęć.
3. Kopiowanie i wklejanie moduły pociągu i wynik modelu w obszarze roboczym.
4. Przeciągnij moduł **Oceny modelu** na obszar roboczy, aby porównać dwie algorytmów.
5. **Uruchamianie** doświadczenia.
![Uruchamianie doświadczenia][10]
6. Kliknij port wyjściowy w dolnej części modułu oceny modelu i kliknij wizualizacja.
![Wizualizowanie wyników oceny][11]

Metryki dostępne są krzywej ROC, odwoływanie dokładności krzywej diagramu i dźwigu. Spojrzenie na tych metryki, są wyświetlane lepiej niż drugi wykonanej pierwszy model. Aby przyjrzeć się co pierwszy model przewidziane, kliknij port wyjściowy wynik modelu i kliknij pozycję wizualizacja.
![Wizualizowanie wynik wyników][12]

Zostanie wyświetlona dwóch kolumn dodane do swojego zestawu danych test.

- Uzyskał prawdopodobieństwa: prawdopodobieństwo, że klient jest nabywca roweru.
- Uzyskał etykiety: Klasyfikacja wykonywanej przez model — nabywca roweru (1) lub nie (0). Próg ten prawdopodobieństwo do oznaczania jest ustawiony na 50% i można je dopasowywać.

Porównanie programów kolumny Nabywcaroweru (rzeczywiste) z etykietami uzyskał (przewidywania), można zobaczyć jak przeprowadził modelu. Za pomocą tego modelu można wprowadzać przewidywań dla nowych klientów i publikowanie modelu jako usługi sieci web lub zapisywania wyników magazynu danych SQL, jako następne kroki.

## <a name="next-steps"></a>Następne kroki

Aby dowiedzieć się więcej o tworzeniu przewidywanych maszynowego uczenia modeli, skorzystaj z [Wprowadzenie do komputera nauki Azure][].

<!--Image references-->
[1]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img1_reader.png
[2]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img2_visualize.png
[3]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img3_readerdata.png
[4]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img4_projectcolumns.png
[5]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img5_columnselector.png
[6]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img6_split.png
[7]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img7_train.png
[8]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img8_traincolumnselector.png
[9]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img9_score.png
[10]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img10_evaluate.png
[11]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img11_evalresults.png
[12]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img12_scoreresults.png


<!--Article references-->
[Azure studio nauki komputera]:https://studio.azureml.net/
[Wprowadzenie do komputera nauki Azure]:https://azure.microsoft.com/documentation/articles/machine-learning-what-is-machine-learning/
[Ręczne ładowanie przykładowych danych]: sql-data-warehouse-load-sample-databases.md
[Tworzenie magazynu danych SQL]: sql-data-warehouse-get-started-provision.md
