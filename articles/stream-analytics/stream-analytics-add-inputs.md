<properties
    pageTitle="Dodawanie wprowadzania danych do zadań analizy strumieniu | Microsoft Azure"
    description="Dowiedz się, jak nawiązać połączenie źródła danych do zadania analizy strumieniu jako strumieniowe dane wejściowe z koncentratorów zdarzenie lub odwołania danych z magazynu w blogu."
    keywords="dane wejściowe, strumieniowego przesyłania danych"
    documentationCenter=""
    services="stream-analytics"
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"
/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"
/>


# <a name="add-a-streaming-data-input-or-reference-data-to-a-stream-analytics-job"></a>Dodawanie danych strumieniowego przesyłania danych wprowadzania lub odwołanie do zadania analizy strumieniu

Dowiedz się, jak nawiązać połączenie źródła danych do zadania analizy strumieniu jako strumieniowe dane wejściowe z koncentratorów zdarzenie lub odwołanie do danych z magazynem obiektów Blob.

Do wprowadzania danych jednego lub więcej, z których każdy będzie definiowane jest połączenie z istniejącego źródła danych można łączyć Azure analizy strumieniu zadania. Jak dane są wysyłane do tego źródła danych, jest używane przez zadanie analizy strumieniu i przetwarzane w czasie rzeczywistym jako strumieniowe danych. Analizy strumieniu ma pierwszej klasie Integracja z [Koncentratory zdarzenia Azure](https://azure.microsoft.com/services/event-hubs/) i [Magazyn obiektów Blob platformy Azure](../storage/storage-dotnet-how-to-use-blobs.md) w obrębie i poza subskrypcji tego zadania.

Ten artykuł dotyczy krok w [ścieżka nauki analizy strumieniu](/documentation/learning-paths/stream-analytics/).

## <a name="data-input-streaming-data-and-reference-data"></a>Wprowadzanie danych: strumieniowego przesyłania danych i danych źródłowych

Istnieją dwa typy odrębnych składników analizy strumieniu: strumieni danych oraz danych źródłowych.

- **Strumienie danych**: zadania analizy strumieniu musi zawierać co najmniej jeden dane wejściowe strumienia zużyte i przekształcania przez zadanie. Azure magazyn obiektów Blob i koncentratory zdarzenia Azure są obsługiwane jako źródeł danych wejściowych strumienia danych. Azure koncentratory wydarzenia są używane do gromadzenia strumienie wydarzenia z urządzeń podłączonych, usług i aplikacji. Magazyn obiektów Blob platformy Azure może służyć jako źródło wprowadzania dla ingesting zbiorcze danych jako strumienia.  
- **Dane źródłowe**: analizy strumieniu obsługuje drugi typ pomocnicze odwołanie nazywane wprowadzania danych.  W przeciwieństwie do danych w ruchu te dane są statyczne lub spowalniając zmianę.  Zwykle służy do wyszukania i korelacji z strumienie danych tworzenie bardziej rozbudowane zestawu danych.  Magazyn obiektów Blob platformy Azure jest obecnie obsługiwane tylko źródło wprowadzania dla danych źródłowych.  

Aby dodać dane wejściowe do analizy strumieniu zadania:

1. W portalu Azure kliknij **danych wejściowych** , a następnie kliknij przycisk **Dodaj dane wejściowe** w swojej pracy analizy strumieniu.

    ![Azure portalu klasyczny — Dodawanie danych wejściowych.](./media/stream-analytics-add-inputs/1-stream-analytics-add-inputs.png)  

    W portalu Azure kliknij **wartości wejściowych** w swojej pracy analizy strumieniu.  

    ![Portal Azure — Dodawanie danych wejściowych.](./media/stream-analytics-add-inputs/7-stream-analytics-add-inputs.png)  

2. Określ typ danych wejściowych: **strumienia danych** lub **danych źródłowych**.

    ![Dodawanie znajdują się poprawne dane wejściowe, strumieniowo lub odwołanie](./media/stream-analytics-add-inputs/2-stream-analytics-add-inputs.png)  

    ![Dodawanie znajdują się poprawne dane wejściowe, strumieniowo lub odwołanie](./media/stream-analytics-add-inputs/8-stream-analytics-add-inputs.png)  

3. W przypadku tworzenia wprowadzania strumienia danych, określ typ źródła danych wejściowych.  Ten krok można pominąć podczas tworzenia odwołania danych jako tylko obiektów Blob miejsca do magazynowania jest obsługiwane w tej chwili.

    ![Dodawanie danych strumienia danych wejściowych](./media/stream-analytics-add-inputs/3-stream-analytics-add-inputs.png)  

    ![Dodawanie danych strumienia danych wejściowych](./media/stream-analytics-add-inputs/9-stream-analytics-add-inputs.png)  

4. Podaj przyjazną nazwę dla tych danych wejściowych w polu Alias wprowadzania danych.  Ta nazwa będzie używana w kwerendzie zadania kopii później Aby odwołać się do wprowadzenia.

    Wypełnij pozostałą część właściwości wymagane połączenia, aby połączyć się ze źródłem danych. Te pola zależy od typu danych wejściowych i źródło i szczegółowo zdefiniowane [w tym miejscu](stream-analytics-create-a-job.md).  

    ![Dodawanie zdarzenia centrum danych wejściowych](./media/stream-analytics-add-inputs/4-stream-analytics-add-inputs.png)  

5. Określanie ustawień szeregowania dla danych wejściowych:
    - Aby upewnić się, kwerendach działają w ten sposób, których oczekujesz, określ **Format szeregowania zdarzenia** danych przychodzących.  Formatów szeregowania obsługiwane są JSON, CSV i Avro.
    - Sprawdź **kodowanie** danych.  UTF-8 jest obsługiwany tylko format kodowania w tej chwili.

    ![Ustawienia szeregowania danych w celu wprowadzania danych](./media/stream-analytics-add-inputs/5-stream-analytics-add-inputs.png)  

    ![Ustawienia szeregowania danych w celu wprowadzania danych](./media/stream-analytics-add-inputs/10-stream-analytics-add-inputs.png)  

6. Po zakończeniu tworzenia wprowadzania danych, analizy strumieniu sprawdzisz, czy można nawiązać źródła danych wejściowych.  Można wyświetlać stan operacji Testuj połączenie na Centrum powiadomienie.

    ![Testowanie połączenia przesyłanie strumieniowe wprowadzania danych](./media/stream-analytics-add-inputs/6-stream-analytics-add-inputs.png)  

    ![Testowanie połączenia przesyłanie strumieniowe wprowadzania danych](./media/stream-analytics-add-inputs/11-stream-analytics-add-inputs.png)  

## <a name="get-help-with-streaming-data-inputs"></a>Uzyskiwanie pomocy dotyczącej strumienia wartości danych wejściowych
Aby uzyskać dodatkową pomoc spróbuj nasze [forum analizy strumieniu Azure](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Następne kroki

- [Wprowadzenie do analizy strumieniu Azure](stream-analytics-introduction.md)
- [Wprowadzenie do korzystania z analizy strumieniu Azure](stream-analytics-get-started.md)
- [Skalowanie Azure analizy strumieniu zadania](stream-analytics-scale-jobs.md)
- [Dokumentacja języka kwerend analizy strumieniu Azure](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Strumień Azure analizy zarządzania pozostałych interfejsu API odwołania](https://msdn.microsoft.com/library/azure/dn835031.aspx)
