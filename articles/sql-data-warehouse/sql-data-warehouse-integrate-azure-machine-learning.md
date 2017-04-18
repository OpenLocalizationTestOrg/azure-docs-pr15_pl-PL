<properties
   pageTitle="Nauka Azure komputera za pomocą magazynu danych SQL | Microsoft Azure"
   description="Samouczek używanie Azure maszynowego uczenia z magazynu danych SQL Azure dla opracowania rozwiązań."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="kevinvngo"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/16/2016"
   ms.author="kevin;barbkess;sonyama"/>

# <a name="use-azure-machine-learning-with-sql-data-warehouse"></a>Nauka Azure komputera za pomocą magazynu danych SQL

Azure nauki komputera to usługa pełni zarządzany przewidywanych analizy, która umożliwia tworzenie modeli przewidywanych przed danych w magazynie danych SQL, a następnie opublikować jako gotowy używają sieci web usługi. Przedstawiono podstawowe informacje o przewidywanych analizy i maszynowego uczenia, czytając [Wprowadzenie do komputera nauki Azure][].  Możesz następnie dowiedzieć się, jak tworzyć, szkoleniem, wynik i przetestować modelu nauka komputera za pomocą [Tworzenie poeksperymentować samouczek][].

W tym artykule przedstawiono sposoby wykonaj poniższe czynności, przy użyciu [Azure maszynowego uczenia Studio][]:

- Odczytywanie danych z bazy danych do tworzenia, szkoleniem i wynik przewidywanych modelu
- Zapisywanie danych do bazy danych


## <a name="read-data-from-sql-data-warehouse"></a>Odczyt danych z magazynu danych SQL

Firma Microsoft odczyta dane z tabeli produktów w bazie danych AdventureWorksDW.

### <a name="step-1"></a>Krok 1

Uruchom nowe doświadczenia, klikając pozycję + Nowy w dolnej części okna maszynowego uczenia Studio wybierz doświadczenia, a następnie wybierz pusty poeksperymentować. Wybierz nazwę domyślną doświadczenia w górnej części obszaru roboczego i zmień jej nazwę na nieco opisową, na przykład rowerowy cena przewidywania.

### <a name="step-2"></a>Krok 2

Poszukaj moduł czytnika na palecie zestawy danych i moduły w lewej części obszaru roboczego doświadczenia. Przeciągnij moduł do obszaru roboczego doświadczenia.
![][drag_reader]

### <a name="step-3"></a>Krok 3

Wybierz moduł czytnika i wypełnij formularz w okienku właściwości.

1. Wybierz bazę danych Azure SQL jako źródła danych.
2. Nazwa serwera bazy danych: wpisz nazwę serwera. [Azure portal][] umożliwia znalezienie to.

![][server_name]

3. Nazwa bazy danych: wpisz nazwę bazy danych na serwerze tylko określonym.
4. Nazwa konta użytkownika serwera: wpisz nazwę użytkownika konta, które ma uprawnienia dostępu do bazy danych.
5. Hasło do konta użytkownika serwera: podania hasła określonego konta użytkownika.
6. Zaakceptuj dowolny certyfikat: Użyj tej opcji (mniej bezpieczne), jeśli chcesz pominąć przeglądanie certyfikatu witryny przed przeczytaniem danych.
7. Kwerendy bazy danych: wpisz instrukcję SQL, opisujące dane chcesz odczytać. W tym przypadku możemy odczyta dane z tabeli produktów przy użyciu następującej kwerendy.


```SQL
SELECT ProductKey, EnglishProductName, StandardCost,
        ListPrice, Size, Weight, DaysToManufacture,
        Class, Style, Color
FROM dbo.DimProduct;
```

![][reader_properties]

### <a name="step-4"></a>Krok 4

1. Uruchom doświadczenia, klikając pozycję Uruchom w obszarze roboczym doświadczenia.
2. Po zakończeniu doświadczenia moduł czytnika uzyskuje zielony znacznik wyboru, aby wskazać, że została pomyślnie ukończona. Zwróć uwagę, również zakończone bieżący stan w prawym górnym rogu.

![][run]

3. Aby wyświetlić zaimportowane dane, kliknij port wyjściowy w dolnej części samochodów zestaw danych i wybierz wizualizacja.


## <a name="create-train-and-score-a-model"></a>Tworzenie, szkolenia i wynik modelu

Teraz możesz użyć tego zestawu danych do:

- Tworzenie modelu: przetwarzanie danych i zdefiniować funkcje
- Przeszkolenie modelu: Wybierz i Zastosuj algorytm szkoleniowe
- Wynik i testowanie modelu: przewidywanie nowa cena rowerowy


![][model]

Aby dowiedzieć się więcej o tworzeniu, szkolenie, wynik i przetestować maszynowego uczenia Użyj modelu [Utwórz poeksperymentować samouczka][].

## <a name="write-data-to-azure-sql-data-warehouse"></a>Zapisywanie danych do magazynu danych SQL Azure

Firma Microsoft zapisze zestaw wyników do tabeli ProductPriceForecast AdventureWorksDW bazy danych.

### <a name="step-1"></a>Krok 1

Poszukaj moduł Writer na palecie zestawy danych i moduły w lewej części obszaru roboczego doświadczenia. Przeciągnij moduł do obszaru roboczego doświadczenia.

![][drag_writer]

### <a name="step-2"></a>Krok 2

Wybierz moduł Writer i wypełnij formularz w okienku właściwości.

1. Wybierz bazę danych Azure SQL jako miejsca docelowego danych.
2. Nazwa serwera bazy danych: wpisz nazwę serwera. [Azure portal][] umożliwia znalezienie to.
3. Nazwa bazy danych: wpisz nazwę bazy danych na serwerze tylko określonym.
4. Nazwa konta użytkownika serwera: wpisz nazwę użytkownika konta, które ma uprawnienia do zapisu bazy danych.
5. Hasło do konta użytkownika serwera: podania hasła określonego konta użytkownika.
6. Zaakceptuj wszystkie certyfikat (niebezpiecznej): Wybierz tę opcję, jeśli nie chcesz wyświetlić certyfikat.
7. Rozdzielaną średnikami listę kolumn do zapisania: Podaj listę zestawu danych lub wynik kolumn, które mają być wyjściowy.
8. Nazwa tabeli danych: Określ nazwę tabeli danych.
9. Rozdzielaną średnikami listę kolumn datatable: Określanie nazwy kolumn, które mają być używane w nowej tabeli. Nazwy kolumn może różnić się od nich w zestawie źródła danych, ale musi zawierać taką samą liczbę kolumn tutaj podany w tabeli wyników.
10. Liczba wierszy zapisywanych dla operacji platformy SQL Azure: można skonfigurować liczbę wierszy, które są zapisywane w bazie danych SQL w jednej operacji.

![][writer_properties]

### <a name="step-3"></a>Krok 3

1. Uruchom doświadczenia, klikając pozycję Uruchom w obszarze roboczym doświadczenia.
2. Po zakończeniu doświadczenia wszystkie moduły uzyskuje zielony znacznik wyboru, aby wskazać, że ta osoba ukończona pomyślnie.

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej porad rozwoju zobacz [Omówienie rozwoju magazynu danych SQL][].

<!--Image references-->

[drag_reader]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-drag-reader.png
[server_name]: ./media/sql-data-warehouse-integrate-azure-machine-learning/dw-server-name.png
[reader_properties]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-reader-properties.png
[run]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-finished-running.png
[model]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-create-train-score-model.png
[drag_writer]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-drag-writer.png
[writer_properties]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-writer-properties.png

<!--Article references-->

[Omówienie tworzenia magazynu danych SQL]: ./sql-data-warehouse-overview-develop.md
[Tworzenie doświadczenia samouczka]: https://azure.microsoft.com/documentation/articles/machine-learning-create-experiment/
[Wprowadzenie do komputera nauki Azure]: https://azure.microsoft.com/documentation/articles/machine-learning-what-is-machine-learning/
[Azure maszynowego uczenia Studio]: https://studio.azureml.net/Home
[Azure portal]: https://portal.azure.com/

<!--MSDN references-->

<!--Other Web references-->

[Azure Machine Learning documentation]: http://azure.microsoft.com/documentation/services/machine-learning/
