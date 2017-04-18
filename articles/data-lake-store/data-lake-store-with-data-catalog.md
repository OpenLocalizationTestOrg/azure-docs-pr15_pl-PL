<properties
   pageTitle="Rejestrowanie danych z magazynu Lake danych w wykazie danych Azure | Microsoft Azure"
   description="Rejestrowanie danych z magazynu Lake danych w wykazie danych Azure"
   services="data-lake-store,data-catalog" 
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
   ms.date="10/28/2016"
   ms.author="nitinme"/>

# <a name="register-data-from-data-lake-store-in-azure-data-catalog"></a>Rejestrowanie danych z magazynu Lake danych w wykazie danych Azure

W tym artykule dowiesz się, jak zintegrować magazynu Lake Azure danych dzięki wykazowi danych Azure, aby dane odnajdowania w obrębie organizacji używającej integrowanie go z wykazu danych. Aby uzyskać więcej informacji o przyspieszy dane zobacz [Azure wykazu danych](../data-catalog/data-catalog-what-is-data-catalog.md). Aby zrozumieć scenariusze, w których można użyć wykazu danych, zobacz [Typowe scenariusze Azure wykazu danych](../data-catalog/data-catalog-common-scenarios.md).

## <a name="prerequisites"></a>Wymagania wstępne

Przed rozpoczęciem tego samouczka, musisz mieć następujące czynności:

- **Azure subskrypcji**. Zobacz [Azure pobrać bezpłatną wersję próbną](https://azure.microsoft.com/pricing/free-trial/).

- **Włączanie Azure subskrypcji** dla publicznej Podgląd Sklepu Lake danych. Zobacz [instrukcje](data-lake-store-get-started-portal.md#signup).

- **Konto azure magazynu Lake danych**. Postępuj zgodnie z instrukcjami na [Rozpoczynanie pracy z magazynu Lake danych Azure za pomocą portalu Azure](data-lake-store-get-started-portal.md). Ten samouczek Pozwól nam Utwórz konto magazynu Lake danych o nazwie **datacatalogstore**. 

    Po utworzeniu konta, przekazywanie zestawu danych przykładowych do niego. Ten samouczek Przekaż nam wszystkie pliki CSV w folderze **AmbulanceData** w [Repozytorium cyfra Lake danych Azure](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/). Za pomocą różnych klientów, takich jak [Eksplorator magazynu Azure](http://storageexplorer.com/), aby przekazać danych do kontenera obiektów blob.

- **Wykaz danych azure**. Twoja organizacja musi już Azure wykazu danych utworzone dla organizacji. Tylko jeden katalog jest dozwolone dla każdej organizacji.

## <a name="register-data-lake-store-as-a-source-for-data-catalog"></a>Rejestr magazynu Lake danych jako źródła wykazu danych

>[AZURE.VIDEO adcwithadl] 

1. Przejdź do pozycji `https://azure.microsoft.com/services/data-catalog`i kliknij pozycję **wprowadzenie**.

2. Zaloguj się do portalu Azure wykazu danych, a następnie kliknij przycisk **Publikuj danych**.

    ![Zarejestruj się źródła danych] (./media/data-lake-store-with-data-catalog/register-data-source.png "Zarejestruj się źródła danych")

3. Na następnej stronie kliknij **Uruchamianie aplikacji**. Spowoduje to pobrać plik manifestu aplikacji na komputerze. Kliknij dwukrotnie plik manifestu, aby uruchomić aplikację.

4. Na stronie powitalnej kliknij przycisk **Zaloguj**, a następnie wprowadź poświadczenia.

    ![Ekran powitalny] (./media/data-lake-store-with-data-catalog/welcome.screen.png "Ekran powitalny")

5. Na stronie Wybieranie źródła danych wybierz pozycję **Azure Lake danych**, a następnie kliknij **Dalej**.

    ![Wybierz źródło danych] (./media/data-lake-store-with-data-catalog/select-source.png "Wybierz źródło danych")

6. Na następnej stronie podaj nazwę konta magazynu Lake danych, które chcesz zarejestrować w wykazie danych. Pozostaw inne opcje jako domyślny, a następnie kliknij przycisk **Połącz**.

    ![Nawiązywanie połączenia ze źródłem danych] (./media/data-lake-store-with-data-catalog/connect-to-source.png "Nawiązywanie połączenia ze źródłem danych")

7. Następna strona można podzielić na następujące segmenty.

    . Pole **Hierarchii serwera** reprezentuje struktura folderów konta magazynu Lake danych. **$Root** reprezentuje głównego konta magazynu Lake danych, a **AmbulanceData** — folder utworzony w folderze głównym konta magazynu Lake danych.

    b. Pole **Dostępne obiekty** Wyświetla pliki i foldery w folderze **AmbulanceData** .

    c. **Obiekty, pole zarejestrowanych** lista plików i folderów, które chcesz zarejestrować w wykazie danych Azure.

    ![Struktura danych w widoku] (./media/data-lake-store-with-data-catalog/view-data-structure.png "Struktura danych w widoku")

8. Ten samouczek należy zarejestrować wszystkie pliki w katalogu. W tym kliknij przycisk (![przesuwanie](./media/data-lake-store-with-data-catalog/move-objects.png "przesuwanie")), aby przenieść wszystkie pliki do pola **obiekty rejestrowanie** . 

    Dane zostaną zarejestrowane w wykazie danych w całej organizacji, dlatego jest podejście zalecane w celu dodania niektórych metadanych, które później można szybko znaleźć dane. Na przykład możesz dodać adres e-mail właściciela danych (na przykład jeden, który jest przekazywania danych) lub dodać znacznik, aby zidentyfikować dane. Zrzut ekranu poniżej pokazano znacznika dodajemy do danych.

    ![Struktura danych w widoku] (./media/data-lake-store-with-data-catalog/view-selected-data-structure.png "Struktura danych w widoku")

    Kliknij przycisk **Zarejestruj**.

8. Następujące przechwycony ekran oznacza, że dane pomyślnie jest zarejestrowany w wykazie danych.

    ![Rejestracja zakończona] (./media/data-lake-store-with-data-catalog/registration-complete.png "Struktura danych w widoku")

9. Kliknij opcję **Portalu widoku** , aby powrócić do portalu wykaz danych i sprawdź, czy możesz teraz otwierać zarejestrowane dane z portalu. Aby wyszukać dane, używając znacznika, który został użyty podczas rejestrowania danych.

    ![Wyszukiwanie danych w katalogu] (./media/data-lake-store-with-data-catalog/search-data-in-catalog.png "Wyszukiwanie danych w katalogu")

10. Teraz można wykonywać operacje, takie jak dodawanie adnotacji i dokumentacji do danych. Aby uzyskać więcej informacji zobacz poniższe łącza.
    * [Dodawanie adnotacji źródeł danych w wykazie danych](../data-catalog/data-catalog-how-to-annotate.md)
    * [Źródła danych dokumentów w wykazie danych](../data-catalog/data-catalog-how-to-documentation.md)

## <a name="see-also"></a>Zobacz też

* [Dodawanie adnotacji źródeł danych w wykazie danych](../data-catalog/data-catalog-how-to-annotate.md)
* [Źródła danych dokumentów w wykazie danych](../data-catalog/data-catalog-how-to-documentation.md)
* [Integracja magazynu Lake danych z innych usług Azure](data-lake-store-integrate-with-other-services.md)
