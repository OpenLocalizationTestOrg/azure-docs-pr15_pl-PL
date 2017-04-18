<properties
   pageTitle="W chmurze Cruiser i rozliczenia integrację interfejsu API platformy Microsoft Azure | Microsoft Azure"
   description="Udostępnia unikatową perspektywę od firmy Microsoft Azure rozliczenia partnera Cruiser chmury na swoimi doświadczeniami integrowanie API rozliczenia Azure produktu.  To jest szczególnie przydatne dla Azure i Cruiser chmury klientów, którzy chcą przy użyciu próbuje Cruiser chmury pakietu Microsoft Azure."
   services=""
   documentationCenter=""
   authors="BryanLa"
   manager="mbaldwin"
   editor=""
   tags="billing"
   />

<tags
   ms.service="billing"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="billing"
   ms.date="09/08/2016"
   ms.author="mobandyo;sirishap;bryanla"/>

# <a name="cloud-cruiser-and-microsoft-azure-billing-api-integration"></a>Chmury Cruiser i rozliczenia integracji interfejs API platformy Microsoft Azure

W tym artykule opisano, jak informacje zebrane z nowego Microsoft Azure rozliczenia API mogą zostać użyte w chmurze Cruiser symulacji koszt przepływu pracy i analizy.

## <a name="azure-ratecard-api"></a>Interfejs API Azure RateCard
Interfejs API RateCard udostępnia informacje o stopa Azure. Po uwierzytelnieniu za pomocą odpowiednich poświadczeń, można wysyłać kwerendy z interfejsu API do zbierania metadanych o dostępne usługi Azure, oraz stawki skojarzone z identyfikatora oferty

Oto przykładowe odpowiedzi z interfejsu API z cenami dla A0 wystąpienia (Windows):

    {
        "MeterId": "0e59ad56-03e5-4c3d-90d4-6670874d7e29",
        "MeterName": "Compute Hours",
        "MeterCategory": "Virtual Machines",
        "MeterSubCategory": "A0 VM (Windows)",
        "Unit": "Hours",
        "MeterRates":
        {
            "0": 0.029
        },
        "EffectiveDate": "2014-08-01T00:00:00Z",
        "IncludedQuantity": 0.0,
        "MeterStatus": "Active"
    },

### <a name="cloud-cruisers-interface-to-azure-ratecard-api"></a>Osoby Cruiser interfejsu RateCard Azure interfejsu API w chmurze
Cruiser chmury mogą korzystać z interfejsu API RateCard informacji na różne sposoby. W tym artykule pokazano, jak mogą być używane do łączenia się IaaS obciążenie pracą kosztów symulacji i analizy.

Wykazać ten przypadków użycia, załóżmy obciążenie pracą wielu wystąpień uruchomione w programie Microsoft Azure Pack (WAP). Celem jest symulować ten sam obciążenia Azure i oszacowanie kosztów prowadzenia takich migracji. Aby utworzyć ten symulacji, istnieją dwa główne zadania do wykonania:

1. **Importowanie i proces informacje o usłudze pobrane z interfejsu API RateCard.** To zadanie również odbywa się w skoroszytach, gdzie wyodrębnione z interfejsu API RateCard są przekształcane i opublikowane do nowego planu taryfowego. Ten nowy plan stawka będzie używany na symulacji do szacowania ceny Azure.

2. **Normalizowanie WAP usług i usługi Azure IaaS.** Domyślnie usługi WAP są oparte na poszczególnych zasobów (Procesora, rozmiar pamięci, rozmiar dysku itp.) podczas Azure usług na podstawie rozmiaru wystąpienia (A0 A1, A2, itp.). Ten pierwszego zadania można wykonywaną przez aparat ETL chmury Cruiser, o nazwie skoroszytów, gdzie dołączone te zasoby na rozmiary wystąpienie analogiczny do usług Azure wystąpienie.

### <a name="import-data-from-the-ratecard-api"></a>Importowanie danych z interfejsu API RateCard

Skoroszyty Cruiser chmury umożliwiają automatyczne gromadzenia i przetwarzania informacji z interfejsu API RateCard.  Skoroszyty (Wyodrębnij przekształcenie obciążenia) ETL umożliwiają konfigurowanie zbioru, przekształcania i publikowanie danych do bazy danych Cruiser chmury.

Każdy skoroszyt może zawierać jednej lub wielu zbiorów, co pozwala dostosować informacje z innych źródeł, aby uzupełnić lub powiększyć danych dotyczących użycia. Następujące dwa zrzuty ekranu pokazano, jak utworzyć nowy *zbiór* istniejący skoroszyt i importowania informacji do *kolekcji* z interfejsu API RateCard:

![Rysunek 1 — Tworzenie nowej kolekcji][1]

![Rysunek 2 — importowanie danych z nowej kolekcji][2]

Po zaimportowaniu danych do skoroszytu, użytkownik może utworzyć wiele czynności i procesów przekształcenie, modyfikowanie i modelowanie danych. W tym przykładzie ponieważ opiszemy tylko infrastruktury jako z usługi (IaaS), firma Microsoft wykonaj czynności przekształcenie, aby usunąć niepotrzebne wiersze lub rekordy, związane z usługami innych niż IaaS.

Następujące zrzut ekranu przedstawiono kroki transformacja użytych do opracowania danych zebranych z interfejsu API RateCard:

![Rysunek 3 – przekształcenie kroki w celu przetwarzania danych zebranych z interfejsu API RateCard][3]

### <a name="defining-new-services-and-rate-plans"></a>Określanie nowych usług i stawka plany

Istnieją różne sposoby, aby zdefiniować usług w chmurze Cruiser. Jedną z opcji jest importowanego usług danych dotyczących użycia. Ta metoda jest rzadko używana podczas pracy z publiczną chmury, gdzie usługi są już zdefiniowane przez dostawcę.

Stopa planu jest zestaw stawki lub cenach, które mogą być stosowane do różnych usług, na podstawie daty obowiązywania lub grupy odbiorców, między innymi. Plany taryfowe można także na chmurze Cruiser tworzenia symulacji lub scenariuszy "Co jeśli", aby dowiedzieć się, jak zmiany w usługach mogą wpływać na całkowity koszt obciążenie pracą.

W tym przykładzie użyjemy informacje o usłudze z interfejsu API RateCard do definiowania nowych usług w chmurze Cruiser. W taki sam sposób firma Microsoft korzysta stawki skojarzone z usługami, aby utworzyć nowy Plan stawka na chmurze Cruiser.

Na końcu procesu transformacji jest można utworzyć nowy krok i publikowanie danych z interfejsu API RateCard jako nowych usług i stawki.

![Rysunek 4 - publikowanie danych z interfejsu API RateCard nowych usług i stawek][4]

### <a name="verify-azure-services-and-rates"></a>Weryfikowanie usługi Azure i stawki

Po opublikowaniu usługi i stawki, można sprawdzić na liście zaimportowanych usług w chmurze Cruiser kartę *usługi* :

![Rysunek 5 - sprawdzanie nowych usług][5]

Na karcie *Plany stopa* możesz sprawdzić nowego planu taryfowego o nazwie "AzureSimulation" z stawki zaimportowane z interfejsu API RateCard.

![Rysunek 6 - weryfikowanie nowego kursu planowanie i skojarzone stawek][6]

### <a name="normalize-wap-and-azure-services"></a>Normalizowanie WAP i usługi Azure

Domyślnie WAP informacje zastosowania oparte na używanie obliczeń, pamięci i zasobów. W chmurze Cruiser, można zdefiniować usług podstawie bezpośrednio na przydział lub taryfowych zastosowania tych zasobów. Na przykład możesz ustawić podstawowe częstotliwość dla każdej godziny użycie Procesora lub opłatę GB pamięci przydzielone do wystąpienia.

W tym przykładzie Aby porównać koszty między WAP i Azure, trzeba agregowanie użycie zasobu na WAP do zestawy, które następnie można zamapować usług Azure. Tego transformacji można łatwo realizowane w skoroszytach:

![Rysunek 7 - przekształcania danych WAP, którą należy znormalizować usług][7]

Ostatnim krokiem w skoroszycie jest opublikować dane do bazy danych Cruiser chmury. W tym kroku danych dotyczących użycia jest teraz połączone w usługom (mapowanie usługi Azure) i powiązane domyślne stawki, aby utworzyć opłat.

Po zakończeniu pracy w skoroszycie, można zautomatyzować przetwarzania danych przez dodanie zadania w ramach harmonogramu zadań i określanie częstotliwości i godziny dla skoroszytu uruchomić.

![Rysunek 8 - planowania skoroszytu][8]

### <a name="create-reports-for-workload-cost-simulation-analysis"></a>Tworzenie raportów analizy symulacji kosztów obciążenie pracą

Po użycia są zbierane i opłaty są ładowane do bazy danych Cruiser w chmurze, możemy wykorzystać moduł wniosków Cruiser chmury tworzenie obciążenie pracą kosztów symulacji, który firma Microsoft najszybciej.

W celu pokazania w tym scenariuszu, utworzonych następujący raport:

![Porównanie kosztów][9]

Porównanie kosztów w górnym wykresie są wyświetlane usługach porównania ceny uruchomienia obciążenie pracą dla każdej usługi określonych między WAP (niebieski ciemny) i Azure (niebieski uproszczonej).

Na wykresie dołu są wyświetlane te same dane, ale podziałem działu. Spowoduje to wyświetlenie kosztów dla każdego działu do uruchamiania ich obciążenie pracą w WAP i Azure, oraz różnicę między nimi na pasku oszczędności (zielony).

## <a name="azure-usage-api"></a>Interfejs API zastosowania Azure


### <a name="introduction"></a>Wprowadzenie

Microsoft ostatnio wprowadzonych API zastosowania Azure, umożliwiając subskrybentów programowy uwzględniał danych dotyczących użycia uzyskanie wniosków do ich zużycie. Jest to świetna wiadomość Cruiser chmury klienci, którzy mogą korzystać z bardziej rozbudowane dostępne za pośrednictwem tego interfejsu API zestawu danych.

Chmura Cruiser mogą korzystać z integracji z interfejsem API zastosowania na kilka sposobów. Szczegółowości (co godzinę zastosowania informacje), a także zasobów metadane dostępne za pośrednictwem interfejsu API dataset niezbędne do obsługi elastyczne Showback lub obciążenia zwrotnego modeli. 

W tym samouczku firma Microsoft przedstawia przykładową jak Cruiser chmury mogą korzystać z informacji użycie interfejsu API. Dokładniej firma Microsoft będzie utworzyć grupę zasobów Azure, kojarzenie tagi struktury konta, a następnie opisują proces pobierania danych i przetwarzanie informacji znacznika w chmurze Cruiser.
 
Ostateczne celem jest będzie mógł tworzyć raporty podobny do następującego i możliwość analizowania kosztów i zużycie oparty na strukturze konta wypełniane znaczniki.

![Rysunek 10 - raport z awarii przy użyciu tagów][10]

### <a name="microsoft-azure-tags"></a>Znaczniki platformy Microsoft Azure

Dostępne za pośrednictwem interfejsu API zastosowania Azure danych zawiera nie tylko informacje o, ale również w tym wszystkie znaczniki skojarzone z nim metadane zasobu. Znaczniki umożliwiają łatwe organizowanie zasobów, ale w celu obowiązywać, należy upewnić się, że:

- Znaczniki poprawnie są stosowane do zasobów w czasie świadczenia
- Tagi prawidłowo są używane w procesie Showback-obciążenia zwrotnego powiązać zastosowania względem struktury konta organizacji.

Obie te wymagania może być trudne, zwłaszcza w przypadku, gdy istnieje proces ręcznego na dostarczenie lub ładowania strony. Błędnie, niepoprawne lub nawet brakujące znaczniki są wspólne skargi klientów, gdy za pomocą znaczników i tych błędów może utrudnić życia na stronie pobierania bardzo.

Przy użyciu nowego Azure użycie interfejsu API Cruiser chmury można pobierać informacje znakowania zasobów i za pomocą zaawansowanych narzędzi ETL o nazwie skoroszytów, usunąć te typowe błędy znakowania. Za pośrednictwem przekształcenia przy użyciu wyrażeń regularnych i korelacji danych Cruiser chmury zidentyfikuj zasoby niepoprawnie oznakowane, można zastosować poprawne znaczniki, zapewniającej poprawne włączenie zasobów z konsumenta.

Na stronie pobierania Cruiser chmury zautomatyzowanie procesów Showback-obciążenia zwrotnego i można wykorzystać informacje o znaczniku powiązać zastosowania odpowiednie konsumenta (dział dzielenia, Project, itp.). Ten automatyzacji zawiera duży poprawy jakości i zapewnia spójne i podlegających inspekcji proces ładowania.
 

### <a name="creating-a-resource-group-with-tags-on-microsoft-azure"></a>Tworzenie grupy zasobów za pomocą znaczników w programie Microsoft Azure
Pierwszy krok w ramach tego samouczka jest utworzenie grupy zasobów w portalu Azure, następnie utwórz nowe znaczniki do powiązania z zasobami. Na przykład możemy będą tworzyli następujące tagi: dział, środowiska, właściciel projektu.

Zrzut ekranu poniżej pokazano próbki grupa zasobów przy użyciu tagów skojarzone.

![Rysunek 11 - grupa zasobów przy użyciu tagów skojarzonego portalu Azure][11]

Następnym krokiem jest do pobrania informacji z interfejsu API zastosowania do Cruiser chmury. Użycie interfejsu API udostępnia obecnie odpowiedzi w formacie JSON. Oto przykładowa danych pobieranych:


    {
      "id": "/subscriptions/bb678b04-0e48-4b44-XXXX-XXXXXXX/providers/Microsoft.Commerce/UsageAggregates/Daily_BRSDT_20150623_0000",
      "name": "Daily_BRSDT_20150623_0000",
      "type": "Microsoft.Commerce/UsageAggregate",
      "properties":
      {
        "subscriptionId": "bb678b04-0e48-4b44-XXXX-XXXXXXXXX",
        "usageStartTime": "2015-06-22T00:00:00+00:00",
        "usageEndTime": "2015-06-23T00:00:00+00:00",
        "meterName": "Compute Hours",
        "meterRegion": "",
        "meterCategory": "Virtual Machines",
        "meterSubCategory": "Standard_D1 VM (Non-Windows)",
        "unit": "Hours",
        "instanceData": "{\"Microsoft.Resources\":{\"resourceUri\":\"/subscriptions/bb678b04-0e48-4b44-XXXX-XXXXXXXX/resourceGroups/DEMOUSAGEAPI/providers/Microsoft.Compute/virtualMachines/MyDockerVM\",\"location\":\"eastus\",\"tags\":{\"Department\":\"Sales\",\"Project\":\"Demo Usage API\",\"Environment\":\"Test\",\"Owner\":\"RSE\"},\"additionalInfo\":{\"ImageType\":\"Canonical\",\"ServiceType\":\"Standard_D1\"}}}",
        "meterId": "e60caf26-9ba0-413d-a422-6141f58081d6",
        "infoFields": {},
        "quantity": 8

      },
    },


### <a name="import-data-from-the-usage-api-into-cloud-cruiser"></a>Importowanie danych z interfejsu API zastosowania do Cruiser chmury

Skoroszyty Cruiser chmury umożliwiają automatyczne gromadzenia i przetwarzania informacji z interfejsu API zastosowania. Skoroszyt (Wyodrębnij przekształcenie obciążenia) ETL umożliwia skonfigurowanie zbioru, przekształcania i publikowanie danych do bazy danych Cruiser chmury.

Każdy skoroszyt może zawierać jeden lub wiele zbiorów. Pozwala na przeniesionym informacji z różnych źródeł, aby uzupełnić lub powiększyć danych dotyczących użycia. W tym przykładzie zostanie utworzony nowy arkusz w skoroszycie Azure szablonu (_UsageAPI)_ i ustawić nowy _zbiór_ zaimportować informacje z interfejsu API zastosowania.

![Rysunek 3 - danych dotyczących użycia interfejsu API zaimportowane do arkusza UsageAPI][12]

Zwróć uwagę, że w tym skoroszycie zawiera już innych arkuszy importowanego usług Azure (_ImportServices_) i przetwarza informacje zużycie z rachunków interfejsu API (_PublishData_).

Firma Microsoft następnie będzie przy użyciu interfejsu API zastosowania można wypełnić arkusz _UsageAPI_ i przeniesionym informacje z danymi zużycie z interfejsu API rozliczenia w arkuszu _PublishData_ .

### <a name="processing-the-tag-information-from-the-usage-api"></a>Przetwarzanie informacji znacznika z interfejsu API zastosowania

Po zaimportowaniu danych do skoroszytu, zostanie utworzony transformacja kroki w arkuszu _UsageAPI_ w celu przetworzenia informacji z interfejsu API. Pierwszym krokiem jest użycie procesora "JSON dzielenie" wyodrębnić znaczniki z jednego pola, a następnie utworzyć pola dla każdego z nich (dział, Project, właściciel i środowiska).

![Rysunek 4 - tworzyć nowe pola, aby uzyskać informacje o znaczniku][13]

Zwróć uwagę, usługa "Networking" nie ma informacji znacznika (pole żółty), ale można sprawdzić, to część tej samej grupy zasobów, sprawdzając pola _ResourceGroupName_ . Ponieważ mamy już znakowania inne zasoby z tej grupy zasobów, firma Microsoft korzysta z tych informacji stosowanie brakujące znaczniki do tego zasobu w dalszej części procesu.

Następnym krokiem jest utworzenie tabeli odnośników kojarzenie informacji z znaczniki do _ResourceGroupName_. Tej tabeli odnośników będzie używana w następnym kroku wzbogacić dane dotyczące zużycia z informacjami o znacznik.

### <a name="adding-the-tag-information-to-the-consumption-data"></a>Dodawanie informacji znacznika danych zużycie

Teraz możemy przejść do arkusza _PublishData_ , który przetwarza informacje zużycie z interfejsu API rozliczenia i Dodaj pola wyodrębnionych z znaczniki. Ten proces jest wykonywane przez w tabeli odnośników utworzony w poprzednim kroku, przy użyciu _ResourceGroupName_ jako klucza wyszukiwaniach.

![Rysunek 5 - wypełniać strukturę na podstawie informacji z wyszukiwaniach][14]

Zwróć uwagę, że pola struktury odpowiednie konto w usłudze "Networking" zostały zastosowane, rozwiązywanie problemu przy użyciu tagów Brak. Możemy również wypełnione pól struktury konta dla zasobów innych niż celem grupa zasobów z "Inne" w celu odróżnienia ich w raportach.

Teraz po prostu trzeba dodać krok do opublikowania danych dotyczących użycia. W tym kroku właściwe stawki dla każdej usługi, zdefiniowane na naszą kursu planowanie zostanie zastosowany do informacji zastosowania przy użyciu wynikowy opłaty załadowana do bazy danych.

Najważniejsze części to, że wystarczy przejść przez ten proces raz. Gdy skoroszyt zostanie zakończone, wystarczy ją dodać do harmonogramu i będzie działać co godzina lub codziennie w zaplanowanym terminie. Następnie jest wystarczy tworzenie nowych raportów lub dostosowywania istniejących, aby przeanalizować dane w celu uzyskanie wniosków zrozumiałej zastosowania do chmury.

### <a name="next-steps"></a>Następne kroki

+ Szczegółowe instrukcje dotyczące tworzenia skoroszytów Cruiser chmury i raporty zapoznaj się z chmury Cruiser online [dokumentacji](http://docs.cloudcruiser.com/) (wymagane logowanie prawidłowych).  Aby uzyskać więcej informacji o Cruiser w chmurze, skontaktuj się z [info@cloudcruiser.com](mailto:info@cloudcruiser.com).
+ Zawiera omówienie dotyczące użycia zasobów Azure oraz interfejsy API RateCard, zobacz [Uzyskiwanie wniosków do usługi Microsoft Azure zużycie zasobów](billing-usage-rate-card-overview.md) .
+ Zapoznaj się z [Azure rozliczenia pozostałych API Reference](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c) więcej informacji na temat obu interfejsów API, które są częścią zestawu interfejsów API udostępnianych przez Menedżera zasobów Azure.
+ Jeśli chcesz zagłębić się w prawo w kodzie przykładowych, zapoznaj się z naszą Microsoft Azure rozliczenia interfejsu API przykłady kodu na [Przykłady kodu Azure](https://azure.microsoft.com/documentation/samples/?term=billing).

### <a name="learn-more"></a>Dowiedz się więcej
+ Zobacz artykuł [Omówienie Menedżera zasobów Azure](azure-resource-manager/resource-group-overview.md) , aby dowiedzieć się więcej na temat Menedżera zasobów Azure.

<!--Image references-->
 
[1]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Create-New-Workbook-Collection.png "Rysunek 1 — Tworzenie nowej kolekcji"
[2]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Import-Data-From-RateCard.png "Rysunek 2 — importowanie danych z nowej kolekcji"
[3]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Transformation-Steps-Process-RateCard-Data.png "Rysunek 3 – przekształcenie kroki w celu przetwarzania danych zebranych z interfejsu API RateCard"
[4]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Publish-RateCard-Data-New-Services-Rates.png "Rysunek 4 - publikowanie danych z interfejsu API RateCard nowych usług i stawek"
[5]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Verify-Azure-Services-And-Pricing1.png "Rysunek 5 - sprawdzanie nowych usług"
[6]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Verify-Azure-Services-And-Pricing2.png "Rysunek 6 - weryfikowanie nowego kursu planowanie i skojarzone stawek"
[7]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Transforming-WAP-Normalize-Services.png "Rysunek 7 - przekształcania danych WAP, którą należy znormalizować usług"
[8]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Workbook-Scheduling.png "Rysunek 8 - planowania skoroszytu"
[9]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Workload-Cost-Simulation-Report.png "Rysunek 9 - przykładowy raport do tego scenariusza porównanie kosztów obciążenie pracą"
[10]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/1_ReportWithTags.png "Rysunek 10 - raport z awarii przy użyciu tagów"
[11]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/2_ResourceGroupsWithTags.png "Rysunek 11 - grupa zasobów przy użyciu tagów skojarzonego portalu Azure"
[12]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/3_ImportIntoUsageAPISheet.png "Rysunek 12 - danych dotyczących użycia interfejsu API zaimportowane do arkusza UsageAPI"
[13]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/4_NewTagField.png "Rysunek 13 - tworzyć nowe pola, aby uzyskać informacje o znaczniku"
[14]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/5_PopulateAccountStructure.png "Rysunek 14 - wypełniać strukturę na podstawie informacji z wyszukiwaniach"
