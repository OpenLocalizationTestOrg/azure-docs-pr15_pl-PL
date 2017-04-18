<properties 
    pageTitle="Wprowadzenie do analizy strumieniu | Microsoft Azure" 
    description="Informacje na temat analizy strumieniu, usługa zarządzanych, który ułatwia analizowanie danych strumieniowych z Internetu czynności (IoT) w czasie rzeczywistym." 
    keywords="analizy jako usługa zarządzanych usług, przetwarzanie strumienia, streaming analizy, co to jest analizy strumieniu"
    services="stream-analytics" 
    documentationCenter="" 
    authors="jeffstokes72" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="09/26/2016" 
    ms.author="jeffstok"/>


# <a name="what-is-stream-analytics"></a>Co to jest analizy strumieniu?

Azure analizy strumieniu jest w pełni zarządzane, wydajne kosztów zdarzeń w czasie rzeczywistym aparatu przetwarzania, który pozwala odblokować szczegółowej analizy danych. Analizy strumieniu ułatwia konfigurowanie w czasie rzeczywistym analityczny obliczeń na danych strumieniowego przesyłania z urządzenia, czujniki, witryn sieci web, media społecznościowe, aplikacji, systemów infrastruktury i więcej.

Za pomocą kilku kliknięć w portalu Azure zadanie analizy strumieniu określenia źródła wprowadzania danych strumieniowych sink dane wyjściowe wyników pracy, mogą tworzyć i przekształcania danych wyrażony jest w języku SQL przypominających. Można monitorować i Dostosuj Skala szybkość zadania portal Azure przeskalować z kilku kilobajtów do gigabajtów lub większej liczby zdarzeń przetwarzanych na sekundę.

Analizy strumieniu wykorzystuje latami pracy Microsoft Research podczas opracowywania wysoce dostosowanych streaming aparatów dla przetwarzania zależne od czasu, a także integracji języka dla intuicyjny specyfikacje takich.

## <a name="what-can-i-use-stream-analytics-for"></a>Co można używać analizy strumieniu?
Dużych ilości danych są już zbudowane prędkością wysoki przez sieć dzisiaj. Organizacje, których można przetwarzać i działać w tym strumieniowego przesyłania danych w czasie rzeczywistym można znacznie zwiększyć wydajność i rozróżnianie się na rynku. Scenariusze w czasie rzeczywistym przesyłanie strumieniowe analizy można znaleźć w branży wszystkie: spersonalizowanych, w czasie rzeczywistym obrót stock analizy i alerty oferowane przez firmy usług finansowych; wykrywanie w czasie rzeczywistym oszustwa; danych i tożsamości usługi ochrony; niezawodne spożyciu i analizowania danych generowanych przez czujniki i siłowniki osadzony w fizycznie obiektów (Internet rzeczy lub IoT); Web clickstream analytics; i aplikacje do zarządzania (klientami CRM) relacji klienta wydawania alerty po obniża się wrażenia określonego przedziału czasu. Sposób najbardziej elastyczne, godny zaufania i efektywne pod względem kosztów do takiej analizy danych w czasie rzeczywistym strumienia zdarzeń do pomyślnego na świecie wysoce konkurencyjności firm nowoczesny odnaleźć firmy.

## <a name="key-capabilities-and-benefits"></a>Kluczowe funkcje i zalety
-   **Łatwość użytkowania:** Analizy strumieniu obsługuje model prostej kwerendy deklaracyjnych opisu przekształcenia. Aby zoptymalizować pod kątem łatwość użytkowania, analizy strumieniu użyta wartość typu Wariant T-SQL i usuwa potrzebę klientów na zajęcie się techniczne złożoności strumienia systemów przetwarzania. Za pomocą [analizy strumieniu kwerendy języka](https://msdn.microsoft.com/library/azure/dn834998.aspx) w edytorze zapytań w przeglądarce, możesz uzyskać sens intelli autouzupełniania ułatwiające można szybko i łatwo wdrożyć czasu serii kwerend, w tym oparte na czasowy sprzężenia, okna agregacji czasowy filtrów i innych typowych operacji, takich jak sprzężenia, agregacje, planów i filtry. Ponadto testowania przed przykładowy plik danych kwerend w przeglądarce umożliwia szybki i iteracyjnego rozwoju.  

-   **Skalowalność:** Analizy strumieniu jest w stanie obsługi zdarzeń Wysoka przepustowość maksymalnie 1GB na sekundę. Integracja z [Koncentratory zdarzenia Azure](https://azure.microsoft.com/services/event-hubs/) i [Koncentratory IoT Azure](https://azure.microsoft.com/services/iot-hub/) umożliwiają rozwiązanie mogły zjeść tej ostatniej miliony zdarzeń według drugiej pochodzących z urządzeń podłączonych clickstreams i dzienniki kilka. Aby osiągnąć ten cel, analizy strumieniu wykorzystuje podziału możliwości biegami zdarzenia, które mogą spowodować 1 MB/s na partycją. Użytkownicy będą mogli partycje obliczeń na kilka czynności logicznych w definicji zapytania, każda z możliwością będzie dalej podzielona, aby zwiększyć skalowalność.  

-   **Niezawodności, powtarzalności i szybkiego odzyskiwania:** Usługa zarządzanych w chmurze, analizy strumieniu pomaga zapobiec utracie danych i udostępnia ciągłości wypadku awarii za pomocą funkcji wbudowanych odzyskiwania. Z możliwością wewnętrznie utrzymywania stanu Usługa udostępnia powtarzalnych wyników zyskujemy pewność, że istnieje możliwość archiwizowanie zdarzeń i ponowne stosowanie przetwarzania w przyszłości, zawsze wyświetlany ten sam efekt. Dzięki temu klientom Przejdź wstecz w czasie i badanie obliczeń w trakcie analizę przyczyn, analizy warunkowej itp.  

-   **Niski koszt:** Jako usługi w chmurze aby umożliwić użytkownikom bardzo niskie koszty rozpoczęcie i obsługa rozwiązań analizy w czasie rzeczywistym jest zoptymalizowana pod analizy strumieniu. Usługa jest przeznaczony dla możesz zapłacić są oparte na jednostkę Streaming użycia i ilość danych przetwarzanych przez system. Użycie jest określany na podstawie liczby wydarzeń przetwarzania i ilości power obliczeń obsługi administracyjnej w klastrze do obsługi odpowiednich zadań analizy strumieniu.  

-   **Odwołać danych:** Analizy strumieniu umożliwia użytkownikom określanie i korzystać z danych źródłowych. Może to być historycznych lub po prostu nie streaming danych, który zmienia się rzadziej w czasie. Systemie upraszcza korzystanie z danych źródłowych być traktowane podobnie jak wszystkie inne przychodzących strumienia zdarzenia do dołączanie za pomocą innych strumieni zdarzenia wchłonięte w czasie rzeczywistym, aby wykonać przekształcenia.  

-   **Funkcje zdefiniowane przez użytkownika:** Analizy strumieniu ma Integracja z Azure maszynowego uczenia Definiowanie wywołania funkcji w usłudze nauki komputera jako części kwerendy analizy strumieniu. To rozszerza możliwości analizy strumieniu je wykorzystać istniejących rozwiązań Azure maszynowego uczenia. Aby uzyskać więcej informacji na temat Przejrzyj [Samouczek integracji nauki komputera](stream-analytics-machine-learning-integration-tutorial.md).

-   **Łączności:** Analizy strumieniu łączy bezpośrednio do koncentratorów zdarzenia Azure i koncentratorów IoT Azure dla spożyciu strumienia i usługi obiektów Blob platformy Azure mogły zjeść tej ostatniej danych historycznych. Wyniki mogą być zapisywane z analizy strumieniu blob miejsca do magazynowania Azure lub tabel, bazy danych SQL Azure, Lake magazynów Azure, DocumentDB, koncentratory zdarzenia, tematy Bus usługi Azure lub kolejek i usługi Power BI, miejsce, w którym można następnie zwizualizować, dalszego przetwarzania przez przepływy pracy, używany do analizy partii za pośrednictwem [Usługi HDInsight Azure](https://azure.microsoft.com/services/hdinsight/) lub przetwarzane ponownie jako serię zdarzeń. Za pomocą koncentratorów wydarzenia prawdopodobnie Zredaguj wielu analizy strumieniu razem z innych źródeł danych i aparatów przetwarzanie bez utraty przesyłanie strumieniowe rodzaj obliczenia.  

## <a name="get-help"></a>Uzyskiwanie pomocy
Aby uzyskać dodatkową pomoc spróbuj nasze [forum analizy strumieniu Azure](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Następne kroki
Zostały wprowadzone do analizy strumieniu, zarządzanych usług do przesyłania strumieniowego analizy danych z Internetu rzeczy. Aby dowiedzieć się więcej o tej usłudze, zobacz:

- [Wprowadzenie do korzystania z analizy strumieniu Azure](stream-analytics-get-started.md)
- [Skalowanie Azure analizy strumieniu zadania](stream-analytics-scale-jobs.md)
- [Dokumentacja języka kwerend analizy strumieniu Azure](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Strumień Azure analizy zarządzania pozostałych interfejsu API odwołania](https://msdn.microsoft.com/library/azure/dn835031.aspx)

