<properties
    pageTitle="Platformy analizy: porównanie Burza Apache analizy strumieniu | Microsoft Azure"
    description="Uzyskaj wskazówki dotyczące wybierania platformy analizy chmurze za pomocą porównania Burza Apache analizy strumieniu. Opis funkcji i różnice."
    keywords="platformy analizy, platformy analizy, platformy analizy w chmurze, Burza porównania"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>

# <a name="help-choosing-a-streaming-analytics-platform-apache-storm-comparison-to-azure-stream-analytics"></a>Pomoc, wybierając pozycję przesyłanie strumieniowe platformy analizy: porównanie Burza Apache analizy strumieniu Azure

Uzyskaj wskazówki dotyczące wybierania platformy analizy chmurze przy użyciu tego porównania Burza Apache do analizy strumieniu Azure. Opis, że przypadki użycia atrakcyjne oferty z analizy strumieniu a burza Apache jako usługa zarządzanych na Azure HDInsight, aby wybrać właściwe rozwiązanie dla swojej firmy.

Obie analizy platformy zapewniają korzyści rozwiązania PaaS, istnieje kilka głównych wyróżniający możliwości, jakie odróżnienia ich. Funkcji oraz ograniczenia te usługi są wymienione poniżej ułatwiające widok jest otwierany na rozwiązanie potrzebne do osiągnięcia założonych celów.

## <a name="storm-comparison-to-stream-analytics-general-features"></a>Porównanie z analizy strumieniu Burza: funkcje ogólne ##
<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong> </strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
                    <strong>Analizy strumieniu Azure</strong>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
                    <strong>Burza Apache na HDInsight</strong>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Otwórz źródło</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Nie, analizy strumieniu Azure jest zastrzeżone Microsoft oferowanie.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Tak, Burza Apache to technologia Apache licencję.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Obsługiwane w programie Microsoft</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Tak </p>
            </td>
            <td width="246" valign="top">
                <p>
Tak </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Wymagania sprzętowe</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Istnieją wymagania sprzętowe. Azure analizy strumieniu to usługa Azure.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Istnieją wymagania sprzętowe. Burza Apache to usługa Azure.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Najwyższego poziomu jednostki</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Z kontrahentami Azure analizy strumieniu wdrażanie i Monitoruj przesyłanie strumieniowe zadania.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Burza Apache na HDInsight klientów wdrażanie i monitorowanie cały klaster, który może zawierać wiele zadań burza, a także inne obciążenia (z uwzględnieniem partii).
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Cena</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Analizy strumieniu kosztuje głośność danych przetwarzanych i wymagane liczba jednostek przesyłanie strumieniowe (za godzinę, którego zadanie jest uruchomione).
                </p>
                <p>
                    <a href="http://azure.microsoft.com/en-us/pricing/details/stream-analytics/">Dalsze informacje o cenach można znaleźć tutaj.</a>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Burza Apache na HDInsight jednostkę zakupu jest oparty na klaster i jest naliczany na podstawie czasu, który klaster jest uruchomiony, niezależnie od zadania wdrożony.
                </p>
                <p>
                    <a href="http://azure.microsoft.com/en-us/pricing/details/hdinsight/">Dalsze informacje o cenach można znaleźć tutaj.</a>
                </p>
            </td>
        </tr>
    </tbody>
</table>
##Do tworzenia na każdej platformie analytics##
<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong> </strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
                    <strong>Analizy strumieniu Azure</strong>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
                    <strong>Burza Apache na HDInsight</strong>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Możliwości: DSL SQL</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Tak, łatwy w użyciu obsługę języka SQL jest dostępna.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Nie, użytkownicy muszą pisanie kodu w Java C# lub przy użyciu interfejsów API Trident.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Możliwości: Operatory czasowy</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Okna agregacji i czasowy sprzężenia są obsługiwane po ich otrzymaniu.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Operatory czasowy musi być wprowadzane przez użytkownika.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Proces tworzenia</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Interakcyjne do tworzenia i debugowania przez Azure Portal na przykładowych danych.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Rozwój, debugowania i monitorowania środowiska są dostępne za pośrednictwem programu Visual Studio obsługi w przypadku użytkowników programu .NET dla języka Java i deweloperów innych języków mogą być używane IDE siebie.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Obsługa debugowania</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Analizy strumieniu oferuje stan zadania podstawowe i dzienników operacji sposób debugowania, ale obecnie nie oferuje elastyczne co/jak duża znajduje się w dziennikach ie: tryb pełny.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Szczegółowe dzienniki są dostępne na potrzeby debugowania. Istnieją dwa sposoby powierzchniowy dzienniki do użytkownika, przy użyciu programu visual Studio lub użytkownik może RDP w grupie, aby uzyskać dostęp do dzienników.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Obsługa UDF (funkcje zdefiniowane przez użytkownika)</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Jest obecnie nie obsługuje funkcji zdefiniowanych przez użytkownika.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Funkcji zdefiniowanych przez użytkownika można napisać C#, Java lub w wybranym języku.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Rozszerzalny - kodu niestandardowego</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Istnieje nie obsługuje extensible kod do analizy strumieniu.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Tak, jest dostępność, aby pisać kodu niestandardowego w C#, Java lub inne obsługiwane języki Burza.
                </p>
            </td>
        </tr>
    </tbody>
</table>
##Źródła danych i wyjściowe##
<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong> </strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
                    <strong>Analizy strumieniu Azure</strong>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
                    <strong>Burza Apache na HDInsight</strong>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                 <strong>Źródła danych wejściowych</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>Obsługiwanych źródeł danych wejściowych są koncentratory zdarzenia Azure i obiektów blob platformy Azure.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Łączniki są dostępne dla koncentratorów zdarzenia, usługa Bus, Kafka itp. Nieobsługiwane łączników mogą być wykonywane przy użyciu kodu niestandardowego.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Formaty danych wejściowych</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Obsługiwane formaty wprowadzania są Avro, JSON, CSV.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Dowolny format mogą być wykonywane przy użyciu kodu niestandardowego.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Wyjściowe</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Przesyłanie strumieniowe zadania może być wiele wyjść. Obsługiwane wyjściowe: Koncentratory Azure zdarzenia, magazyn obiektów Blob platformy Azure, Azure tabele, bazy danych Azure SQL i PowerBI.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Pomoc techniczna dla wielu obrazów w topologii każdy wynik może być logiki niestandardowej podrzędne przetwarzania. Okno Burza zawiera łączniki PowerBI, koncentratory zdarzenia Azure, magazyn obiektów Blob platformy Azure, Azure DocumentDB, SQL i HBase. Nieobsługiwane łączników mogą być wykonywane przy użyciu kodu niestandardowego.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Formaty kodowanie danych</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Analizy strumieniu wymaga UTF-8 format danych, aby można było użyć.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Dowolnego formatu kodowania danych mogą być wykonywane przy użyciu kodu niestandardowego.
                </p>
            </td>
        </tr>
    </tbody>
</table>
##Zarządzanie i operacje##
<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong> </strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
                    <strong>Analizy strumieniu Azure</strong>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
                    <strong>Burza Apache na HDInsight</strong>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Model wdrażania zadania</strong>
                </p>
                <p>
                    - <strong>Azure Portal</strong>
                </p>
                <p>
                    - <strong>Programu Visual Studio</strong>
                </p>
                <p>
                    - <strong>Programu PowerShell</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Wdrożenie jest zaimplementowana przez Azure Portal, programu PowerShell i interfejsów API pozostałych.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Depolyment jest zaimplementowana przez Azure Portal, programu PowerShell, Visual Studio i interfejsów API pozostałych.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Monitorowanie (statystykę, liczniki itp.)</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Monitorowanie jest zaimplementowana przez Azure Portal oraz interfejsy API pozostałych.
                </p>
                <p>
Użytkownik może również skonfigurować alerty Azure.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Monitorowanie jest zaimplementowana za pośrednictwem interfejsu użytkownika Burza oraz interfejsy API pozostałych.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Skalowalność</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Liczba jednostek Streaming dla każdego zadania. Przetwarza każdej jednostki Streaming maksymalnie 1 MB/s. Maksymalna liczba jednostek 50 domyślnie. Połączenie, aby zwiększyć limit.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Liczba węzłów w klastrze Burza HDI. Nie limitu liczby węzłów (górny zdefiniowany przez przydziału Azure). Połączenie, aby zwiększyć limit.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Limity dotyczące przetwarzania danych</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Użytkowników można skalować w górę lub w dół liczbę Streaming jednostek, aby zwiększyć przetwarzania danych lub Optymalizowanie kosztów.
                </p>
                <p>
Skalowanie do 1 GB/s </p>
            </td>
            <td width="246" valign="top">
                <p>
Użytkownika można skalować w górę lub w dół rozmiar klaster stosownie do potrzeb.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Zatrzymanie/życiorysu</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Zatrzymywanie i życiorys z ostatniego miejsca zatrzymane.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Zatrzymywanie i życiorys z ostatniego umieść przestał oparte na znak wodny.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Aktualizacja usługi i struktury</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Automatyczne poprawianie bez przestojów.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Automatyczne poprawianie bez przestojów.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Zapewnianie ciągłości za pośrednictwem wysoce dostępne usługi z gwarantowanej SLA</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
SLA 99,9% czas pracy </p>
                <p>
Autoodzyskiwanie z błędów </p>
                <p>
Odzyskiwanie stanowe operatorów czasowy jest wbudowany.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Umowa dotycząca poziomu usług 99,9% czasu pracy klaster Burza. Burza Apache jest odporność na uszkodzenia platformy przesyłanie strumieniowe, jednak odpowiada klientów upewnij się, że nieprzerwanie działać przesyłanie strumieniowe zadań.
                </p>
            </td>
        </tr>
    </tbody>
</table>
##Zaawansowane funkcje##
<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong> </strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
                    <strong>Analizy strumieniu Azure</strong>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
                    <strong>Burza Apache na HDInsight</strong>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Opóźnienia przylotów i obsługi kolejność zdarzeń</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Wbudowane można konfigurować zasady, aby zmienić kolejność, upuszczania lub dostosować czas zdarzenia.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Użytkownik musi spełnić logiki do obsługi tego scenariusza.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Dane źródłowe</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Odwołanie danych z obiektów blob platformy Azure maksymalny rozmiar 100 MB wyszukiwania w pamięci podręcznej. Odświeżanie danych źródłowych jest zarządzane przez usługę.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Nie ograniczenia rozmiaru danych. Dostępne dla HBase, DocumentDB, SQL Server i Azure łączników. Nieobsługiwane łączników mogą być wykonywane przy użyciu kodu niestandardowego.
                </p>
                <p>
Odświeżanie danych źródłowych musi być obsługiwane przez kodu niestandardowego.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Integracja z komputera szkoleniowe</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Konfigurując opublikowanych modeli Azure maszynowego uczenia jako funkcje podczas tworzenia zadania ASA <a href="http://blogs.msdn.com/b/streamanalytics/archive/2015/05/24/real-time-scoring-of-streaming-data-using-machine-learning-models.aspx">(Podgląd prywatnych)</a>.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Dostępne za pośrednictwem Burza tekst "Śruby".
                </p>
            </td>
        </tr>
    </tbody>
</table>
