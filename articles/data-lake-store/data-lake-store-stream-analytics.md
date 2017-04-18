<properties
   pageTitle="Przesyłanie strumieniowe dane z analizy strumieniu do magazynu Lake danych | Azure"
   description="Za pomocą analizy strumieniu Azure transmisji danych do magazynu Lake danych Azure"
   services="data-lake-store,stream-analytics" 
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
   ms.date="07/07/2016"
   ms.author="nitinme"/>

# <a name="stream-data-from-azure-storage-blob-into-data-lake-store-using-azure-stream-analytics"></a>Strumień danych z obiektów Blob miejsca do magazynowania Azure do magazynu Lake danych za pomocą analizy strumieniu Azure

W tym artykule dowiesz się, jak za pomocą magazynu Lake danych Azure jako wynik dla zadania Azure analizy strumieniu. W tym artykule przedstawiono prosty scenariusz, który odczytuje danych Azure magazyn obiektów blob (wejście) i zapisuje dane do magazynu Lake danych (wyjście).

>[AZURE.NOTE] W tym momencie tworzenia i konfigurowania magazynu Lake dane wyjściowe dla analizy strumieniu jest obsługiwana tylko w [Klasycznym Portal Azure](https://manage.windowsazure.com). W związku z tym niektóre części tego samouczka będzie korzystać z portalu klasyczny Azure.

## <a name="prerequisites"></a>Wymagania wstępne

Przed rozpoczęciem tego samouczka, musisz mieć następujące czynności:

- **Azure subskrypcji**. Zobacz [Azure pobrać bezpłatną wersję próbną](https://azure.microsoft.com/pricing/free-trial/).

- **Włączanie Azure subskrypcji** dla publicznej Podgląd Sklepu Lake danych. Zobacz [instrukcje](data-lake-store-get-started-portal.md#signup).

- **Magazyn azure konta**. Kontener obiektów blob z tego konta użyje do wprowadzania danych dla zadania analizy strumieniu. Ten samouczek założono, że utworzenie konta miejsca do magazynowania o nazwie **datalakestoreasa** i kontener w ramach konta o nazwie **datalakestoreasacontainer**. Po utworzeniu kontenera, Przekaż przykładowy plik danych do niego. Przykładowy plik danych można przejść z [Repozytorium cyfra Lake danych Azure](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/Drivers.txt). Za pomocą różnych klientów, takich jak [Eksplorator magazynu Azure](http://storageexplorer.com/), aby przekazać danych do kontenera obiektów blob.

    >[AZURE.NOTE] Po utworzeniu konta z Azure Portal, upewnij się, że została utworzona z modelem **Klasyczny** wdrożenia. Dzięki temu, że konto przestrzeni dyskowej są dostępne z klasyczny Portal Azure, ponieważ jest to, co stosujemy, aby utworzyć zadanie analizy strumieniu. Aby uzyskać instrukcje dotyczące tworzenia konta miejsca do magazynowania z Portal Azure za pomocą klasyczny zobacz [Tworzenie konta usługi Azure miejsca do magazynowania](../storage/storage-create-storage-account/#create-a-storage-account).
    >
    > Lub można utworzyć konto miejsca do magazynowania z portalu klasyczny Azure.

- **Konto azure magazynu Lake danych**. Postępuj zgodnie z instrukcjami na [Rozpoczynanie pracy z magazynu Lake danych Azure za pomocą portalu Azure](data-lake-store-get-started-portal.md).  


## <a name="create-a-stream-analytics-job"></a>Utwórz zadanie analizy strumieniu

Rozpoczyna się od utworzenia zadania analizy strumieniu zawierającego wprowadzania źródłowa i docelowa dane wyjściowe. Ten samouczek źródłem jest kontenerem obiektów blob platformy Azure i miejscem docelowym jest magazynu Lake danych.

1. Logowanie do [portalu klasyczny Azure](https://manage.windowsazure.com).

2. W lewym dolnym rogu ekranu kliknij przycisk **Nowy**, **Usług danych**, **Analizy strumieniu**, **Szybkie tworzenie**. Zawierają wartości, tak jak pokazano poniżej, a następnie kliknij przycisk **Utwórz zadanie analizy strumieniu**.

    ![Utwórz zadanie analizy strumieniu] (./media/data-lake-store-stream-analytics/create.job.png "Utwórz zadanie analizy strumieniu")

## <a name="create-a-blob-input-for-the-job"></a>Tworzenie obiektów Blob wprowadzania dla zadania

1. Otwórz stronę dla zadania analizy strumieniu, kliknij kartę **danych wejściowych** , a następnie kliknij **Dodaj dane wejściowe** , aby uruchomić kreatora.

2. Na stronie **Dodawanie dane wejściowe do zadania** wybierz **strumienia danych**, a następnie kliknij strzałkę do przodu.

    ![Dodaj dane wejściowe do zadania] (./media/data-lake-store-stream-analytics/create.input.1.png "Dodaj dane wejściowe do zadania")

3. Na stronie **Dodawanie strumienia danych do zadania** wybierz **Magazyn obiektów Blob**, a następnie kliknij strzałkę do przodu.

    ![Dodaj strumienia danych do zadania] (./media/data-lake-store-stream-analytics/create.input.2.png "Dodaj strumienia danych do zadania")

4. Na stronie **Ustawienia magazyn obiektów Blob** należy podać szczegóły dotyczące magazyn obiektów blob, który będzie używany jako źródło danych wejściowych.

    ![Podaj to miejsca do magazynowania] (./media/data-lake-store-stream-analytics/create.input.3.png "Podaj to miejsca do magazynowania")

    * **Wprowadź alias wprowadzania danych**. To jest unikatową nazwę podana dla zadania wprowadzania.
    * **Wybierz konto miejsca do magazynowania**. Upewnij się, konto miejsca do magazynowania znajduje się w tym samym regionie jako zadanie analizy strumieniu lub będzie powodowało dodatkowe koszty przenoszenie danych między regionami.
    * **Podaj kontenera magazynu**. Możesz utworzyć nowy kontener lub zaznacz istniejącego kontenera.

    Kliknij strzałkę do przodu.

5. Na stronie **Ustawienia szeregowania** Ustawianie formatu szeregowania jako **CSV**, ogranicznika w postaci **karty**kodowania **UTF8**, a następnie kliknij znacznik wyboru.

    ![Podać ustawienia szeregowania] (./media/data-lake-store-stream-analytics/create.input.4.png "Podać ustawienia szeregowania")

6. Po zakończeniu przy użyciu kreatora, wprowadzania obiektów blob zostaną dodane na karcie **danych wejściowych** , a w kolumnie **diagnozowania** należy wyświetlany **przycisk OK**. Możesz również jawnie przetestować połączenie danych wejściowych przy użyciu przycisku **Testuj połączenie** na dole.

## <a name="create-a-data-lake-store-output-for-the-job"></a>Tworzenie magazynu Lake danych wyjściowych zadania

1. Otwórz stronę dla zadania analizy strumieniu, kliknij kartę **wyników** , a następnie kliknij **Dodaj wynik** , aby uruchomić kreatora.

2. Na stronie **Dodawanie wyjścia do zadania** wybierz **Magazyn Lake danych**, a następnie kliknij strzałkę do przodu.

    ![Dodaj dane wyjściowe do zadania] (./media/data-lake-store-stream-analytics/create.output.1.png "Dodaj dane wyjściowe do zadania")

3. Na stronie **połączenie Autoryzuj** Jeśli utworzono już konto magazynu Lake danych, kliknij przycisk **Autoryzuj teraz**. W przeciwnym razie kliknij pozycję **Utwórz konto teraz** , aby utworzyć nowe konto. Ten samouczek Załóżmy już masz konto magazynu Lake danych utworzony (wymieniony w wymagań wstępnych). Możesz zostanie automatycznie upoważniona przy użyciu poświadczeń, w których logujesz się do portalu klasycznym Azure.

    ![Autoryzuj magazynu Lake danych] (./media/data-lake-store-stream-analytics/create.output.2.png "Autoryzuj magazynu Lake danych")

4. Na stronie **Ustawienia sklepu Lake danych** wprowadź informacje, jak pokazano poniżej obrazu przechwytywania ekranu.

    ![Ustawienia magazynu Lake danych określ] (./media/data-lake-store-stream-analytics/create.output.3.png "Ustawienia magazynu Lake danych określ")

    * **Wprowadź alias dane wyjściowe**. To jest unikatową nazwę oferowanych wyników zadania.
    * **Określanie konta magazynu Lake danych**. Czy zostały już utworzone, wymienione w wymagania wstępne.
    * **Określanie ścieżka wzorzec prefiksu**. Jest to wymagane do identyfikowania plików dane wyjściowe, które są zapisywane w magazynie Lake danych przez zadanie analizy strumieniu. Ponieważ tytuły wyjściowe napisane przez zadania są dostępne w formacie GUID, łącznie z prefiksem pomoże zidentyfikować wyjściowy pisanych. Jeśli chcesz uwzględnić sygnatury daty i godziny jako część prefiks upewnij się, możesz umieścić `{date}/{time}` w strukturze prefiks. Jeśli dołączysz, **Format daty **i **Godziny Format** pola są włączone i można wybrać format wybór.

    Kliknij strzałkę do przodu.

5. Na stronie **Ustawienia szeregowania** Ustawianie formatu szeregowania jako **CSV**, ogranicznika w postaci **karty**kodowania **UTF8**, a następnie kliknij znacznik wyboru.

    ![Określ format wyjściowy] (./media/data-lake-store-stream-analytics/create.output.4.png "Określ format wyjściowy")

6. Po zakończeniu przy użyciu kreatora, wynik magazynu Lake danych zostaną dodane na karcie **wyników** , a w kolumnie **diagnostyki** należy wyświetlany **przycisk OK**. Możesz również jawnie przetestować połączenie danych wyjściowych przy użyciu przycisku **Testuj połączenie** na dole.

## <a name="run-the-stream-analytics-job"></a>Uruchom zadanie analizy strumieniu

Aby uruchomić zadanie analizy strumieniu, należy uruchomić kwerendę z kartą kwerenda. Ten samouczek zostanie uruchomiona kwerenda próbki, zamieniając symboli zastępczych zadania wprowadzania i wyjściowe aliasy, jak pokazano poniżej obrazu przechwytywania ekranu.

![Uruchamianie kwerendy] (./media/data-lake-store-stream-analytics/run.query.png "Uruchamianie kwerendy")

Kliknij przycisk **Zapisz** u dołu ekranu, a następnie kliknij polecenie **Uruchom**. W oknie dialogowym wybierz **Czas niestandardowe**, a następnie wybierz datę z przeszłości, takie jak **2016-1-1**. Kliknij znacznik wyboru, aby uruchomić zadanie. Go może potrwać kilka minut Aby uruchomić zadanie.

![Ustawianie czasu zadania] (./media/data-lake-store-stream-analytics/run.query.2.png "Ustawianie czasu zadania")

Po uruchomieniu zadania, kliknij kartę **Monitor** , aby zobaczyć, jak została przetworzona danych.

![Monitorowanie zadania] (./media/data-lake-store-stream-analytics/run.query.3.png "Monitorowanie zadania")

Na koniec umożliwia [Azure Portal](https://portal.azure.com) Otwórz konta magazynu Lake danych i sprawdź, czy dane zostały pomyślnie zapisane na koncie.

![Sprawdź wynik] (./media/data-lake-store-stream-analytics/run.query.4.png "Sprawdź wynik")

W okienku Eksplorator danych należy zauważyć, że dane wyjściowe są zapisywane w folderze zgodnie z ustawieniami w magazynie Lake danych ustawienia wyjściowe (`streamanalytics/job/output/{date}/{time}`).  

## <a name="see-also"></a>Zobacz też

* [Utwórz klaster HDInsight w celu używania magazynu Lake danych](data-lake-store-hdinsight-hadoop-use-portal.md)
