<properties 
   pageTitle="Wgląd wydajności kwerendy bazy danych Azure SQL" 
   description="Monitorowanie wydajności zapytania identyfikuje większości używające Procesora kwerend dla bazy danych SQL Azure." 
   services="sql-database" 
   documentationCenter="" 
   authors="stevestein" 
   manager="jhubbard" 
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management" 
   ms.date="08/09/2016"
   ms.author="sstein"/>

# <a name="azure-sql-database-query-performance-insight"></a>Wgląd wydajności kwerendy bazy danych Azure SQL


Zarządzanie i dostosowywanie wydajności relacyjnych baz danych jest trudne zadania, która wymaga znaczną specjalizacji i czasu inwestycji. Wgląd wydajności zapytania umożliwia poświęcać mniej czasu rozwiązywania problemów wydajności bazy danych, zapewniając następujące czynności:

- Szczegółowego wgląd zużycie zasobów (DTU) do bazy danych. 
- Najpopularniejsze kwerendy według liczby procesorów/czas trwania-wykonywania, które mogą być potencjalnie dostosowanych do zwiększona wydajność.
- Możliwość przechodzić do szczegółów kwerendy, Wyświetl tekst i Historia wykorzystania zasobów. 
- Dostosowywanie adnotacje, które pokazują akcje wykonywane przez [Advisor bazy danych programu SQL Azure](sql-database-advisor.md) wydajności  



## <a name="prerequisites"></a>Wymagania wstępne

- Kwerendy wydajności wglądu jest dostępna tylko w wersji 12 bazy danych SQL Azure.
- Kwerendy wydajności wglądu wymaga aktywnej bazy danych [Sklepu kwerendy](https://msdn.microsoft.com/library/dn817826.aspx) . Jeśli Magazyn kwerendy nie jest uruchomiony, w portalu zostanie wyświetlony monit o włączenie jej.

 
## <a name="permissions"></a>Uprawnienia

Aby użyć kwerendy wydajności wglądu są wymagane następujące uprawnienia [Kontrola dostępu oparta na rolach](../active-directory/role-based-access-control-configure.md) : 

- Aby wyświetlić górny zasobów dużo kwerend i wykresy są wymagane uprawnienia **czytnika**, **właściciela**, **współautora**, **Współautora bazy danych SQL**lub **SQL Server współautorów** . 
- Aby wyświetlić tekst kwerendy są wymagane uprawnienia **właściciela**, **współautora**, **Współautora bazy danych SQL**lub **SQL Server współautorów** .



## <a name="using-query-performance-insight"></a>Za pomocą kwerendy wydajności wglądu

Łatwy w użyciu jest wglądu wydajności kwerend:

- Otwórz [Azure portal](https://portal.azure.com/) i Znajdź bazę danych, którą chcesz sprawdzić. 
  - W menu po lewej stronie w obszarze pomocy technicznej i rozwiązywania problemów wybierz "Kwerendy wydajności wglądu".
- Na karcie pierwszego Przejrzyj listę najczęstsze kwerendy używające zasobów.
- Wybierz poszczególne kwerendę, aby wyświetlić jego szczegóły.
- Otwórz [Advisor bazy danych programu SQL Azure](sql-database-advisor.md) i sprawdź, czy wszelkie zalecenia są dostępne.
- Za pomocą suwaków lub powiększyć ikony, aby zmienić interwał obserwowane.

    ![pulpit nawigacyjny wydajności](./media/sql-database-query-performance/performance.png)

> [AZURE.NOTE] Kilka godzin danych musi zostać uwzględniona w sklepie kwerendy bazy danych SQL o podanie wniosków wyników kwerendy. Jeśli baza danych nie ma żadnych działań lub sklepu zapytania nie była aktywna w pewnym okresie czasu, wykresy będzie pusta podczas wyświetlania tego okresu. Jeśli nie jest uruchomiony, może być Włącz magazyn kwerendy w dowolnym momencie.   



## <a name="review-top-cpu-consuming-queries"></a>Przejrzyj górny Procesora przez inne kwerendy

W [portalu](http://portal.azure.com) wykonaj następujące czynności:

1. Przejdź do bazy danych SQL, a następnie kliknij polecenie **wszystkie ustawienia** > **pomocy technicznej + rozwiązywanie problemów** > **wglądu wyników kwerendy**. 

    ![Kwerendy wydajności wglądu][1]

    Zostanie otwarty w widoku Najpopularniejsze kwerendy i przedstawiono najczęstsze kwerendy dużo Procesora.

1. Kliknij przycisk Wokoło wykresu, aby uzyskać szczegółowe informacje.<br>Górny wiersz zawiera ogólny DTU % w bazie danych, podczas pasków pokazać % CPU zużywanej przez wybrane zapytania w zaznaczonej przedziale czasu (na przykład, jeśli wybrano **ostatnim tygodniu** Każdy słupek oznacza jeden dzień).

    ![Najpopularniejsze kwerendy][2]

    Siatka dołu reprezentuje zagregowane informacje widoczne zapytania.

  - Identyfikator kwerendy — Unikatowy identyfikator zapytania w bazie danych.
  - Procesor dla kwerendy przedziale widocznych (w zależności od funkcji agregacji).
  - Czas trwania dla kwerendy (w zależności od funkcji agregacji).
  - Całkowita liczba wykonania określonego zapytania.

    Zaznacz lub wyczyść poszczególnych kwerend, aby uwzględnić lub wykluczyć je z wykresu za pomocą pól wyboru.

1. Jeśli dane staje się przestarzałe, kliknij przycisk **Odśwież** .
1. Możesz za pomocą suwaków i powiększanie przyciski, aby zmienić interwał obserwacji i badanie tych najwyższych wartościach:  ![ustawienia](./media/sql-database-query-performance/zoom.png)
1. Opcjonalnie Jeśli chcesz inny widok, możesz wybierz kartę **niestandardowe** i ustawić:
  
  - Metryka (CPU, czas trwania, liczba wykonanie)
  - Interwał (ostatni 24 godziny, ostatni tydzień, ostatni miesiąc). 
  - Liczba kwerend.
  - Funkcja agregacji.

    ![Ustawienia](./media/sql-database-query-performance/custom-tab.png)

## <a name="viewing-individual-query-details"></a>Wyświetlanie szczegółów pojedynczej kwerendy

Aby wyświetlić szczegóły zapytania:

1. Kliknij dowolną kwerendę na liście najczęstsze kwerendy.

    ![Szczegóły](./media/sql-database-query-performance/details.png)

1. Zostanie otwarty widok Szczegóły i liczba zużycie/czas trwania-wykonywania kwerend Procesora jest niepoprawna w czasie.
1. Kliknij przycisk Wokoło wykresu, aby uzyskać szczegółowe informacje.
  - Górny wykres przedstawia wiersz z ogólnego % DTU bazy danych, a pasków są % CPU zużywanej przez wybranego zapytania.
  - Drugi wykres przedstawia całkowity czas trwania według wybranego zapytania.
  - Wykres dołu pokazuje całkowitą liczbę wykonania przez wybranego zapytania.
    
    ![Szczegóły zapytania][3]

1. Opcjonalnie za pomocą suwaków, przyciski powiększania lub kliknij przycisk **Ustawienia** , aby dostosować sposób wyświetlania danych kwerendy lub, aby wybrać innego okresu.

## <a name="review-top-queries-per-duration"></a>Przejrzyj Najpopularniejsze kwerendy według czasu trwania

W ostatniej aktualizacji kwerendy wydajności wgląd, możemy wprowadzone dwa nowych metryk, które mogą ułatwić zidentyfikowanie potencjalnych gardeł: liczba czas trwania i wykonanie.<br>

Długotrwałe kwerend ze słowami największe możliwości blokowania zasobów dłużej, blokowania innych użytkowników i ograniczanie skalowalność. Są one również najbardziej odpowiednich do optymalizacji.<br>

Aby zidentyfikować długotrwałych kwerend:

1. Otwieranie karty **niestandardowe** w wglądu wydajności kwerend dla wybranej bazy danych
1. Bezproblemowa Zmień **czas trwania**
1. Wybierz liczbę kwerend i obserwacji interwału
1. Wybierz funkcję agregacji
  - **Suma** dodaje wszystkie czas wykonywania kwerendy przedziale cały obserwacji.
  - **Max** znajduje kwerend, które czas wykonywania została maksymalna interwałem całego obserwacji.
  - **Średnia** umożliwia znalezienie wyrazów Średni czas wykonania wszystkich wykonania kwerendy i pozwala góry poza te wartości średnich. 

    ![czas trwania kwerendy][4]

## <a name="review-top-queries-per-execution-count"></a>Przejrzyj najczęstsze kwerendy na wykonanie liczba

Duża liczba wykonania mogą nie mieć wpływ na samej bazy danych i użycie zasobów może być niska, ale aplikacja ogólnego może zostać wyświetlony działa wolno.

W niektórych przypadkach liczba bardzo wysoki wykonanie może prowadzić do wzrost sieci niepotrzebnej. Niepotrzebnej znacząco wpłynąć na wydajność. Są one objęte czasu oczekiwania w sieci i opóźnienie serwera podrzędnego. 

Na przykład wiele witryn sieci Web opartych na danych intensywnie dostęp do bazy danych dla każdego żądania użytkownika. Podczas połączenia buforowanie ułatwia, ruch sieciowy lepszą i przetwarzania obciążenia na serwerze bazy danych negatywnie wpływają na wydajność.  Ogólne porady należy zachować niepotrzebnej do minimum.

Aby zidentyfikować często wykonać kwerendy zapytania ("chatty"):

1. Otwieranie karty **niestandardowe** w wglądu wydajności kwerend dla wybranej bazy danych
1. Zmienianie metryki jako **Liczba wykonanie**
1. Wybierz liczbę kwerend i obserwacji interwału

    ![Liczba wykonywania kwerend][5]

## <a name="understanding-performance-tuning-annotations"></a>Opis adnotacje dostrajania wydajności 

Podczas badania z pracą w wglądu wydajności kwerend, można zauważyć ikony z pionowej linii na wykresie.<br>

Te ikony są adnotacje; reprezentują wydajności wpływających na akcje wykonywane przez [Advisor bazy danych programu SQL Azure](sql-database-advisor.md). Dzięki aktywowania adnotacji możesz uzyskać podstawowe informacje dotyczące akcji:

![Adnotacja kwerendy][6]

Jeśli chcesz dowiedzieć się więcej lub stosowanie advisor rekomendacji, kliknij ikonę. Zostanie otwarta szczegóły akcji. Jeśli jest aktywne rekomendacji można stosować go bezpośrednio z dala od komputera przy użyciu polecenia.

![Szczegóły zapytania adnotacji][7]

### <a name="multiple-annotations"></a>Wiele adnotacji. ###

Jest to możliwe, że ze względu na poziom powiększenia, będzie uzyskiwanie zwinięte adnotacje, które są blisko siebie do jednego. To będą reprezentowane przez ikonę specjalne, klikając go zostanie otwarte nowe karta miejsce, w którym lista pogrupowanych adnotacje będą wyświetlane.
Korelacji kwerendy i dostosowywanie akcje wydajności może pomóc w celu lepszego zrozumienia z pracą. 


##  <a name="optimizing-the-query-store-configuration-for-query-performance-insight"></a>Optymalizacja konfiguracji sklepu kwerendy wglądu wyników kwerendy

Podczas korzystania z kwerendy wydajności wgląd może wystąpić następujące komunikaty zapytania ze Sklepu:

- "Sklepu kwerendy nie jest poprawnie skonfigurowany na tej bazy danych. Kliknij tutaj, aby dowiedzieć się więcej."
- "Sklepu kwerendy nie jest poprawnie skonfigurowany na tej bazy danych. "Kliknij tutaj, aby zmienić ustawienia". 

Zazwyczaj pojawi się po sklepu kwerendy nie będzie mógł zbieranie nowych danych. 

Pierwszym przypadku polega sklepu kwerendy jest w stanie tylko do odczytu i optymalnie ustawić parametry. Można to naprawić, zwiększenie rozmiaru magazynu kwerendy lub czyszcząc sklepu kwerendy.

![przycisk qds][8]

Drugim przypadku polega sklepu kwerendy jest wyłączone lub parametry nie są ustawione optymalnie. <br>Możesz zmienić zasady przechowywania i przechwytywania i Włącz magazyn kwerendy, wykonując poniższe polecenia lub bezpośrednio w portalu:

![przycisk qds][9]

### <a name="recommended-retention-and-capture-policy"></a>Zalecane zasady przechowywania i przechwytywania

Istnieją dwa typy zasad przechowywania:

- Rozmiar podstawie — Jeśli ustawiono automatyczne jego wyczyści danych automatycznie, gdy w pobliżu maksymalny rozmiar osiągnięciu.
- Czas podstawie — domyślnie możemy zostanie ustawiony go do 30 dni, co oznacza, jeśli sklepu kwerendy zostanie uruchomiony brakować miejsca, spowoduje usunięcie kwerendami starsze niż 30 dni

Rejestrowanie można ustawić zasady:

- **Wszystkie** — przechwytuje wszystkie kwerendy.
- **Automatyczne** — rzadko i kwerend z czasem trwania nieznaczne kompilacji i wykonanie są ignorowane. Progi czas trwania kompilacji i środowisko uruchomieniowe liczba wykonywania wewnętrznie są określone. To jest domyślna opcja.
- **Brak** — sklepu kwerendy zatrzymuje, przechwytywanie nowej kwerendy, jednak statystykę wykonywania kwerend już przechwycony są nadal pobierane.
    
Zaleca się ustawienie zasad wszystkie AUTOMATYCZNIE i Oczyść zasad do 30 dni:

    ALTER DATABASE [YourDB] 
    SET QUERY_STORE (SIZE_BASED_CLEANUP_MODE = AUTO);
        
    ALTER DATABASE [YourDB] 
    SET QUERY_STORE (CLEANUP_POLICY = (STALE_QUERY_THRESHOLD_DAYS = 30));
    
    ALTER DATABASE [YourDB] 
    SET QUERY_STORE (QUERY_CAPTURE_MODE = AUTO);

Zwiększanie rozmiaru magazynu kwerendy. Może to być wykonywane przez połączenie z bazą danych i wydawania po kwerendy:

    ALTER DATABASE [YourDB]
    SET QUERY_STORE (MAX_STORAGE_SIZE_MB = 1024);

Stosowanie tych ustawień spowoduje ostatecznie sklepu kwerendy zbierania nowych kwerend, jednak jeśli nie chcesz czekać możesz wyczyścić magazyn kwerendy. 
> [AZURE.NOTE] Wykonywanie następujących zapytania usunie wszystkie informacje w magazynie kwerendy. 


    ALTER DATABASE [YourDB] SET QUERY_STORE CLEAR;


## <a name="summary"></a>Podsumowanie

Kwerendy wydajności wglądu pomaga zrozumieć wpływ z pracą kwerendy i jego powiązań z zużycie zasobów bazy danych. Za pomocą tej funkcji będą informacje o góry używające kwerendy i identyfikację nich, aby rozwiązać problem, zanim wystąpią problemy.




## <a name="next-steps"></a>Następne kroki

Aby uzyskać dodatkowe zalecenia dotyczące zwiększania wydajności bazy danych SQL kliknij przycisk [zalecenia](sql-database-advisor.md) na karta **Wglądu wyników kwerendy** .

![Wydajność Advisor](./media/sql-database-query-performance/ia.png)


<!--Image references-->
[1]: ./media/sql-database-query-performance/tile.png
[2]: ./media/sql-database-query-performance/top-queries.png
[3]: ./media/sql-database-query-performance/query-details.png
[4]: ./media/sql-database-query-performance/top-duration.png
[5]: ./media/sql-database-query-performance/top-execution.png
[6]: ./media/sql-database-query-performance/annotation.png
[7]: ./media/sql-database-query-performance/annotation-details.png
[8]: ./media/sql-database-query-performance/qds-off.png
[9]: ./media/sql-database-query-performance/qds-button.png

