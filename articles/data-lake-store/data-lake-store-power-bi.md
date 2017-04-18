<properties
   pageTitle="Analizowanie danych w magazynie Lake danych za pomocą usługi Power BI | Microsoft Azure"
   description="Analizowanie danych przechowywanych w magazynie Lake danych Azure za pomocą usługi Power BI"
   services="data-lake-store" 
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/05/2016"
   ms.author="nitinme"/>

# <a name="analyze-data-in-data-lake-store-by-using-power-bi"></a>Analizowanie danych w magazynie Lake danych za pomocą usługi Power BI

W tym artykule dowiesz się, jak za pomocą Power BI Desktop analizowanie i wizualizowanie danych przechowywanych w magazynie Lake danych Azure.

## <a name="prerequisites"></a>Wymagania wstępne

Przed rozpoczęciem tego samouczka, musisz mieć następujące czynności:

- **Azure subskrypcji**. Zobacz [Azure pobrać bezpłatną wersję próbną](https://azure.microsoft.com/pricing/free-trial/).

- **Konto azure magazynu Lake danych**. Postępuj zgodnie z instrukcjami na [Rozpoczynanie pracy z magazynu Lake danych Azure za pomocą portalu Azure](data-lake-store-get-started-portal.md). W tym artykule założono, że zostały już utworzone konto magazynu Lake danych o nazwie **mybidatalakestore**i przekazane przykładowy plik danych (**Drivers.txt**). Ten przykładowy plik jest dostępna do pobrania w [Repozytorium cyfra Lake danych Azure](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/Drivers.txt).

- **Power BI Desktop**. To można pobrać z [Centrum pobierania Microsoft](https://www.microsoft.com/en-us/download/details.aspx?id=45331). 


## <a name="create-a-report-in-power-bi-desktop"></a>Tworzenie raportu w Power BI Desktop

1. Uruchom program Power BI Desktop na Twoim komputerze.

2. **Dla użytkowników domowych** na wstążce programu kliknij przycisk **Pobierz dane**, a następnie kliknij przycisk więcej. W oknie dialogowym **Pobieranie danych** kliknij **Azure**, kliknij pozycję **Magazyn Lake danych Azure**, a następnie kliknij przycisk **Połącz**.

    ![Nawiązywanie połączenia z magazynem Lake danych] (./media/data-lake-store-power-bi/get-data-lake-store-account.png "Nawiązywanie połączenia z magazynem Lake danych")

3. Jeśli zobaczysz okno dialogowe o łącznika w fazie projektowania, wybrać opcję, aby kontynuować.

4. W oknie dialogowym **Microsoft Azure danych Lake sklepu** Podaj adres URL do swojego konta magazynu Lake danych, a następnie kliknij **przycisk OK**.

    ![Adres URL magazynu Lake danych] (./media/data-lake-store-power-bi/get-data-lake-store-account-url.png "Adres URL magazynu Lake danych")

5. W następnym oknie dialogowym kliknij przycisk **Zaloguj** , aby zalogować się do konta magazynu Lake danych. Nastąpi przekierowanie do strony logowania Twojej organizacji. Postępuj zgodnie z instrukcjami, aby zalogować się do konta.

    ![Zaloguj się do magazynu Lake danych] (./media/data-lake-store-power-bi/get-data-lake-store-account-signin.png "Zaloguj się do magazynu Lake danych")

6. Po pomyślnym zapisaniu, kliknij przycisk **Połącz**.

    ![Nawiązywanie połączenia z magazynem Lake danych] (./media/data-lake-store-power-bi/get-data-lake-store-account-connect.png "Nawiązywanie połączenia z magazynem Lake danych")

7. Następne okno dialogowe zawiera plik, który przekazane do swojego konta magazynu Lake danych. Sprawdź informacje, a następnie kliknij przycisk **Załaduj**.

    ![Załaduj dane z magazynu Lake danych] (./media/data-lake-store-power-bi/get-data-lake-store-account-load.png "Załaduj dane z magazynu Lake danych")

8. Po pomyślnym załadowano dane do usługi Power BI, zobaczą następujące pola na karcie **pola** .

    ![Zaimportowane pola] (./media/data-lake-store-power-bi/imported-fields.png "Zaimportowane pola")

    Wizualizowanie i analizy danych, możemy woli jednak dane mają być dostępne na następujące pola

    ![Żądany pola] (./media/data-lake-store-power-bi/desired-fields.png "Żądany pola")

    W następnych kroków zostanie odpowiednio zaktualizowana zapytania, aby przekonwertować zaimportowanych danych w wybranym formacie.

9. **Dla użytkowników domowych** na wstążce programu kliknij pozycję **Edytuj kwerendy**.

    ![Edytowanie zapytania] (./media/data-lake-store-power-bi/edit-queries.png "Edytowanie zapytania")

10. W edytorze zapytań w kolumnie **zawartości** , kliknij opcję **binarna**.

    ![Edytowanie zapytania] (./media/data-lake-store-power-bi/convert-query1.png "Edytowanie zapytania")

11. Zostanie wyświetlona ikona pliku, reprezentujący plik **Drivers.txt** , które zostały przekazane. Kliknij prawym przyciskiem myszy plik, a następnie kliknij pozycję **CSV**.  

    ![Edytowanie zapytania] (./media/data-lake-store-power-bi/convert-query2.png "Edytowanie zapytania")

12. Wynik powinien być widoczny, tak jak pokazano poniżej. Dane są teraz dostępne w formacie, który umożliwia tworzenie wizualizacji.

    ![Edytowanie zapytania] (./media/data-lake-store-power-bi/convert-query3.png "Edytowanie zapytania")

13. Na wstążce **Narzędzia główne** kliknij pozycję **Zamknij i Zastosuj**, a następnie kliknij **Zamknij i Zastosuj**.

    ![Edytowanie zapytania] (./media/data-lake-store-power-bi/load-edited-query.png "Edytowanie zapytania")

14. Po zaktualizowaniu kwerendy na karcie **pola** zostanie wyświetlona nowe pola dostępne dla wizualizacji.

    ![Aktualizacja pól] (./media/data-lake-store-power-bi/updated-query-fields.png "Aktualizacja pól")

15. Pozwól nam Tworzenie wykresu kołowego reprezentować sterowniki w każdym mieście dla danego kraju. W tym celu należy wybrać następujące opcje.

    1. Na karcie wizualizacji kliknij symbol wykresu kołowego.

        ![Tworzenie wykresu kołowego] (./media/data-lake-store-power-bi/create-pie-chart.png "Tworzenie wykresu kołowego")

    2. Kolumny, które firma Microsoft będzie używany to **Kolumna 4** (nazwa miasta) i **7 kolumny** (nazwa kraju). Przeciągnij **pola** na karcie tych kolumn do karty **wizualizacji** tak jak pokazano poniżej.

        ![Tworzenie wizualizacji] (./media/data-lake-store-power-bi/create-visualizations.png "Tworzenie wizualizacji")

    3. Wykres kołowy teraz powinna wyglądać podobnie jak w przedstawionym poniżej.

        ![Wykres kołowy] (./media/data-lake-store-power-bi/pie-chart.png "Tworzenie wizualizacji")

16. Wybierając określonym kraju z filtrów poziomu stron, zobaczy liczbę czynników w każdym mieście wybranego kraju. Na przykład na karcie **wizualizacji** w obszarze **filtry na poziomie strony**, zaznacz **Brazylii**.

    ![Wybierz kraj] (./media/data-lake-store-power-bi/select-country.png "Wybierz kraj")

17. Wykres kołowy jest automatycznie aktualizowana do wyświetlania sterowniki w miastach Brazylii.

    ![Sterowniki w kraju] (./media/data-lake-store-power-bi/driver-per-country.png "Sterowniki dla każdego kraju")

18. W menu **plik** kliknij pozycję **Zapisz** do zapisania jako pliku Power BI Desktop wizualizacji.

## <a name="publish-report-to-power-bi-service"></a>Publikowanie raportu usługi Power BI

Po utworzeniu wizualizacji w Power BI Desktop, możesz udostępnić go innym osobom przez opublikowanie go w usłudze Power BI. Aby uzyskać instrukcje, jak to zrobić zobacz [Publikowanie z Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-upload-desktop-files/).

## <a name="see-also"></a>Zobacz też

* [Analizowanie danych w magazynie Lake danych za pomocą analizy Lake danych](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
