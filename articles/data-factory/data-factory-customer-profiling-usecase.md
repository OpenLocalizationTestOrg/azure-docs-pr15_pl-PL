<properties 
    pageTitle="Używanie liter - profilowania odbiorców" 
    description="Dowiedz się, jak Azure Factory danych służy do tworzenia opartych na danych przepływu pracy (planowana) do profilu gier klientów." 
    services="data-factory" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/06/2016" 
    ms.author="shlo"/>

# <a name="use-case---customer-profiling"></a>Używanie liter - profilowania odbiorców

Azure Factory danych jest jednym z wielu usług używane do wdrożenia pakietu analizy Cortana solution Accelerator.  Więcej informacji na temat analizy Cortany można znaleźć w [Pakiecie analizy Cortany](http://www.microsoft.com/cortanaanalytics). W tym dokumencie opisano przypadek użycia prosty ułatwiające rozpoczęcie pracy z opis jak Azure danych Factory można rozwiązywania typowych problemów z analizy.

Jest potrzebny dostęp do wypróbować ten przypadków użycia prosty [Azure subskrypcji](https://azure.microsoft.com/pricing/free-trial/).  Można wdrażać próbki korzystający z tym przypadków użycia, wykonując kroki opisane w artykule [próbki](data-factory-samples.md) .

## <a name="scenario"></a>Scenariusz

Contoso jest firmy gier, która tworzy gry dla wielu platform: gier, konsole, dłoni posiadanych urządzeń i komputery osobiste (PC). Jak odtwarzacze tych gier, dużej ilości danych dziennika jest tworzony śledzący upodobania, gier stylu i preferencji użytkownika.  W połączeniu z demograficzne, regionalne i danych produktów firmy Contoso mogą wykonywać analizy ułatwi temat rozszerza odtwarzacze i zakupów docelowej ich do uaktualnienia i w gry. 

Cel firmy Contoso jest określenie szanse sprzedaży wzrost sprzedaży i sprzedaży w oparciu o historię gier jego uczestników i dodać atrakcyjne funkcje do rozwoju firmy dysk i klientom lepiej. W przypadku użycia tego używamy firmy gier jako przykład przedsiębiorstwa. Firma chce zoptymalizować jej gry w oparciu o zachowanie graczami. Te zasady mają zastosowanie do każdej firmy, którą chce uczestniczyć klientom wokół jego towarów i usług i rozszerza swoich klientów.

## <a name="challenges"></a>Wyzwania


## <a name="solution-overview"></a>Omówienie rozwiązania

Ten przypadek użycia prostego może służyć jako przykład jak umożliwia Factory danych Azure mogły zjeść tej ostatniej, przygotowywanie Przekształcanie, analizowanie i publikowanie danych.

![Koniec do zakończenia przepływu pracy](./media/data-factory-customer-profiling-usecase/EndToEndWorkflow.png)

Ilustracja przedstawia, jak procesy dane wyświetlane w portalu Azure po zostały wdrożone.

1.  **PartitionGameLogsPipeline** odczytuje nieprzetworzonych zdarzenia gier z magazynem obiektów blob i tworzy partycje na podstawie rok, miesiąc i dzień.
2.  **EnrichGameLogsPipeline** łączy podzielone na partycje gier zdarzeń z danych źródłowych kodu geo i wzbogaca danych dzięki mapowaniu adresów IP do odpowiednich lokalizacji geo.
3.  Proces **AnalyzeMarketingCampaignPipeline** używa wzbogaconego danych i przetwarza je z danymi reklamami w celu utworzenia druku, która zawiera efektywność kampanii marketingowych.

W tym przykładzie Factory danych służy do dodać akompaniament działania, które kopiowanie danych wejściowych, przekształcania i proces dane i wyjściowe ostateczne dane do bazy danych SQL Azure.  Możesz również wizualizowanie sieci procesy danych, zarządzanie nimi i monitorować ich stanu z interfejsu użytkownika.

## <a name="benefits"></a>Zalety

Optymalizacja ich analizy profilu użytkownika i wyrównywania go do celów biznesowych, firma gier jest szybko zebrać upodobania i analizować efektywność jego kampanii marketingowych.




