<properties
    pageTitle="Podstawowe informacje o Azure wsadowe usługi | Microsoft Azure"
    description="Dowiedz się więcej o korzystaniu z usługi Azure partii równolegle na dużą skalę i obciążenia HPC"
    services="batch"
    documentationCenter=""
    authors="mmacy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.workload="big-compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/22/2016"
    ms.author="marsma"/>

# <a name="basics-of-azure-batch"></a>Podstawy partia Azure

Azure partii umożliwia uruchamianie aplikacji na dużą skalę równoległych i wysokiej wydajności przetwarzania (HPC) — w chmurze. Jest usługą platformę, która planuje obliczeniowych pracy do uruchomienia w zbiorze zarządzanych maszyn wirtualnych i automatycznie skali obliczyć zasobów do potrzeb zadań.

Usługa partii definiowania obliczeń Azure zasoby, aby wykonać aplikacji równolegle, a w skali. Można uruchomić na żądanie lub zaplanowanych zadań, dzięki czemu nie trzeba ręcznie utworzyć, konfigurowanie i zarządzanie klastrze HPC, poszczególnych maszyn wirtualnych, wirtualnych sieci lub złożone zadania i zadania planowania infrastruktury.

## <a name="use-cases-for-batch"></a>Używanie przypadków dla partii

Partii jest usługą Azure zarządzane, która służy do *Przetwarzanie wsadowe* lub *przetwarzania partii*— uruchomiony dużej liczby podobne zadania, aby uzyskać kilka oczekiwany wynik. Obliczanie partii najczęściej jest używany przez organizacji, w których regularnie procesu, przekształcanie i analizowanie dużych ilości danych.

Partia działa również z obciążeń pracą i leżą równoległe aplikacji (nazywane także "embarrassingly równolegle"). Leżą równoległe obciążenia łatwo są zamieniane na wielu zadań, które mogą pracować jednocześnie na wielu komputerach.

![Równoległe zadania][1]<br/>

Przykłady obciążeniami, które często są przetwarzane przy użyciu tej techniki są następujące:

* Modelowanie finansowe ryzyka
* Klimat i hydrologii analiza danych
* Renderowanie obrazu, analizy i przetwarzanie
* Kodowanie multimediów i przekodowanie
* Analiza genetycznych sekwencji
* Analiza obciążenia inżynierskie
* Testowanie oprogramowania

Partii może także wykonywać obliczenia równolegle z kroku zmniejszanie na końcu i wykonywać bardziej złożone obciążenia HPC, takich jak aplikacje [Wiadomości przechodzące Interface (MPI)](batch-mpi.md) .

Do porównania partii i inne opcje rozwiązanie HPC platformy Azure zobacz [partii i HPC rozwiązań](batch-hpc-solutions.md).

## <a name="developing-with-batch"></a>Opracowywanie z partii

Przetwarzanie równoległe obciążenie pracą z partii odbywa się zwykle programowo przy użyciu jednego z [Interfejsów API partię](#batch-development-apis). Z interfejsów API partię, Utwórz i Zarządzaj pul węzłów obliczeń (maszyn wirtualnych) i planowanie zadań i zadań do wykonania na tych węzłach. Aplikacji lub usługi, której tworzysz używa interfejsów API partii można komunikować się z usługą partię.

Możesz wydajność procesu dużych obciążeń pracą w organizacji lub ustawianie Fronton usług klientom, dzięki czemu mogą być uruchamiane zadań i zadań — na żądanie lub zgodnie z harmonogramem — na jedno, setki lub nawet tysiące węzły. Za pomocą partię jako część większych przepływ pracy, zarządzane za pomocą narzędzi, takich jak [Azure danych Factory](../data-factory/data-factory-data-processing-using-batch.md).

> [AZURE.TIP] Gdy wszystko będzie już gotowe wyświetlić partii API zrozumieć więcej informacji na temat funkcji, które znajdują się w nim, zapoznaj się z [Omówienie funkcji partii dla deweloperów](batch-api-basics.md).

### <a name="azure-accounts-youll-need"></a>Azure konta, które będzie potrzebne

Podczas opracowywania rozwiązań partię, które będą używane następujące konta platformy Microsoft Azure.

- **Konto azure i subskrypcji** — Jeśli nie masz jeszcze subskrypcji usługi Azure, można aktywować swojej [subskrybentów MSDN korzyści][msdn_benefits], lub Utwórz [bezpłatne konto Azure][free_account]. Po utworzeniu konta subskrypcji domyślny jest tworzony.

- **Konto partii** — po aplikacji interakcji z usługą partii nazwę konta, adres URL konta i klawisz dostępu są używane jako poświadczenia. Wszystkie zasoby partii takich jak pule, obliczyć węzły, zadań i zadania są skojarzone z kontem partię. Możesz [utworzyć konto partii](batch-account-create-portal.md) w portalu Azure.

- **Konta miejsca do magazynowania** — partia zawiera wbudowane funkcje do pracy z plikami w [Magazynie Azure][azure_storage]. Scenariusz niemal każdej partii korzysta z magazynu Azure — przemieszczania programy, które działają zadań i przetwarzania danych, a także do przechowywania danych wyjściowych, które generują. Aby utworzyć konto miejsca do magazynowania, zobacz [temat Azure miejsca do magazynowania konta](./../storage/storage-create-storage-account.md).

### <a name="batch-development-apis"></a>Rozwoju partii interfejsy API

Aplikacje i usługi może wydawać bezpośredniego połączenia interfejsu API usługi REST, użyj co najmniej jeden z następujących bibliotek klienta lub obu tych sposobów zarządzać obliczeniowych zasobów i działania równoległe obciążenie pracą w skali przy użyciu usługi partii.

| INTERFEJS API    | Interfejs API odwołania | Plik do pobrania | Przykłady kodu |
| ----------------- | ------------- | -------- | ------------ |
| **Partia REST** | [W WITRYNIE MSDN][batch_rest] | N/D! | [W WITRYNIE MSDN][batch_rest] |
| **Partia .NET**    | [W WITRYNIE MSDN][api_net] | [NuGet][api_net_nuget] | [GitHub][api_sample_net] |
| **Python partii**  | [readthedocs.IO][api_python] | [PyPI][api_python_pypi] |[GitHub][api_sample_python] |
| **Node.js partii** | [github.IO][api_nodejs] | [npm][api_nodejs_npm] | - |
| **Partia Java** (wersja preview) | [github.IO][api_java] | [Środowiska maven][api_java_jar] | [GitHub][api_sample_java] |

### <a name="batch-resource-management"></a>Zarządzanie zasobami partii

Oprócz klienta interfejsy API umożliwia także następujące do zarządzania zasobami w ramach konta partię.

- [Partii apletów poleceń programu PowerShell][batch_ps]: polecenia cmdlet wsadowe Azure w module [Azure programu PowerShell](../powershell-install-configure.md) umożliwiają zarządzanie zasobami partii przy użyciu programu PowerShell.

- [Polecenie Azure](../xplat-cli-install.md): interfejs wiersza polecenia w usłudze Azure (polecenie Azure) jest narzędzi i platform, zawierającego poleceń powłoki interakcji z wielu usług Azure, w tym partię.

- Biblioteka klienta [.NET zarządzania partii](batch-management-dotnet.md) : również dostępne za pośrednictwem [NuGet][api_net_mgmt_nuget], Biblioteka klienta partii zarządzania .NET umożliwia programistycznie zarządzać kontami partię, przydziałów i pakietów aplikacji. Odwołanie do biblioteki zarządzania w [witrynie MSDN][api_net_mgmt].

### <a name="batch-tools"></a>Narzędzia partii

Gdy nie jest wymagany tworzenie rozwiązań przy użyciu partii, Oto niektóre narzędzia przydatne używać podczas tworzenia i debugowania partii aplikacje i usługi.

 - [Azure portal][portal]: tworzenie, monitorowanie i usuwanie pul partię, zadania i zadania w Azure portal partii karty. Można wyświetlić informacje o stanie dla tych i innych zasobów podczas uruchamiania zadań, a nawet pobierać pliki z węzłów do uruchamiania usługi pul (Pobierz zadania nie powiodło się `stderr.txt` podczas rozwiązywania problemów, na przykład). Możesz również pobrać pliki Remote Desktop (RDP), które można zalogować się do obliczenia węzły.

 - [Azure Eksploratora partii][batch_explorer]: Eksploratora partia zawiera podobne partii funkcje zarządzania zasobami Azure portal, ale w autonomicznej aplikacji klienckiej platformy Windows Presentation Foundation (WPF). Jedną z .NET partii przykładowe aplikacje dostępne na [GitHub][github_samples], można go utworzyć, przy użyciu programu Visual Studio 2015 lub powyżej i używać go do przeglądania i zarządzania zasobami na koncie partii podczas projektowanie i sprawdzanie rozwiązanie partię. Zadania, puli i szczegóły zadania pobierania plików z węzły obliczeń i połączyć węzły zdalnie przy użyciu plików Remote Desktop (RDP), który można pobrać w Eksploratorze partię.

 - [Eksplorator magazynu usługi Microsoft Azure][storage_explorer]: podczas nie ściśle narzędzie Azure partię, Eksplorator magazynu jest inne przydatne narzędzie, aby mieć podczas opracowywania i debugowanie rozwiązanie partii.

## <a name="scenario-scale-out-a-parallel-workload"></a>Scenariusz: Skala się równoległe obciążenie pracą

Typowe rozwiązanie, które używa interfejsów API partii do współdziałania z usługą partii obejmuje skalowania prace leżą równoległe — takie jak renderowanie obrazów 3W scen — w puli węzłów obliczeń. Tej puli węzłów obliczeń może być farmie"renderowania", która zapewnia dziesiątki, setki lub nawet tysiące rdzenie do renderowania zadania, na przykład.

Na poniższym diagramie przedstawiono typowe partii przepływ pracy, przy użyciu aplikacji klienckiej lub hostowanej usługi przy użyciu partii do uruchomienia równoległe obciążenie pracą.

![Przepływ pracy rozwiązanie partii][2]

W tym scenariuszu typowych aplikacji lub usługi procesy obliczeniowa obciążenie pracą w partii Azure, wykonując następujące czynności:

1. Przekazywanie **plików wejściowych** i **aplikacji** , która będzie przetwarzał tych plików do swojego konta magazynu platformy Azure. Wprowadzania pliki mogą być dane, które będzie przetwarzał aplikacji, takich jak modelowanie finansowe danych lub plików wideo, które będą przekodowane. Pliki aplikacji może być dowolna aplikacja, która jest używana do przetwarzania danych, takich jak aplikacja renderowanie 3W lub transcoder multimediów.

2. Tworzenie partii **puli** węzłów obliczeń na koncie partii — węzły te są maszyn wirtualnych, które będzie wykonywać zadania. Należy określić właściwości, takich jak [rozmiar węzeł](./../cloud-services/cloud-services-sizes-specs.md), ich systemu operacyjnego i lokalizację w magazynie Azure aplikacji do zainstalowania węzły dołączania puli (aplikacja przekazane w kroku #1). Można również skonfigurować puli, [Automatyczne skalowanie](batch-automatic-scaling.md)— dynamicznie dostosować liczby węzłów obliczeń w puli — w odpowiedzi na obciążenie pracą, generowany zadań.

3. Tworzenie partii **zadanie** ma być uruchamiane obciążenie pracą w puli węzłów obliczeń. Po utworzeniu zadania skojarzyć z puli partię.

4. Dodawanie **zadań** do zadania. Po dodaniu zadań do zadania usługi partię automatycznie zaplanowanie zadania do wykonania w węzłach obliczeń w puli. Każdego zadania korzysta z aplikacji, której zostały przekazane do procesu plików wejściowych.

    - 4a. Przed wykonuje zadania, go pobrać danych (pliki wprowadzania) jest proces do węzła obliczeń, którego został przydzielony. Jeśli aplikacji nie została jeszcze zainstalowana w węźle (zobacz krok #2), można pobrać tutaj zamiast tego. Po zakończeniu pobierania zadania wykonać na ich węzły przydzielone.

5. Jak uruchomić zadań, można wysyłać kwerendy partii monitorowanie postępu zadania i jego zadań. Z aplikacji klienckiej lub usługi komunikuje się z usługą partii za pomocą protokołu HTTPS, a ponieważ może monitorowania tysięcy zadań uruchomionych tysięcy węzłów obliczeń, należy [kwerendy usługi partii wydajność](batch-efficient-list-queries.md).

6. Co wykonania zadania przekazywać dane o jego wynik do magazynu Azure. Można również pobrać pliki bezpośrednio z węzły obliczeń.

7. Gdy usługi monitorowania wykryje ukończono zadania w pracy, z aplikacją kliencką lub usługi można pobrać dane wyjściowe dla dalszego przetwarzania lub oceny.

Zachowaj pamiętać jest tylko jednym ze sposobów używania partię, a w tym scenariuszu opisano tylko kilka jej dostępnych funkcji. Na przykład można wykonywać [wielu zadań jednocześnie](batch-parallel-node-tasks.md) w każdym węźle obliczeń i [przygotowanie i ukończenia zadania](batch-job-prep-release.md) umożliwia przygotowanie węzły do zadań, a następnie Oczyszczanie później.

## <a name="next-steps"></a>Następne kroki

Teraz, gdy masz Omówienie usługi partii jest godzina, aby wyświetlić elementy, aby dowiedzieć się, jak go używać przetwarzania równoległego obciążeń pracą usługi obliczeniowych.

- Przeczytaj [Omówienie funkcji partii dla deweloperów](batch-api-basics.md), ważnymi informacjami dla wszystkich osób, przygotowywanie się do użycia partię. Ten artykuł zawiera więcej szczegółowych informacji o partii zasoby usługi, takie jak pul, węzły, zadań i zadań, a wiele funkcji interfejsu API, których można używać podczas tworzenia aplikacji partię.

- [Rozpoczynanie pracy z biblioteką partii Azure dla środowiska .NET](batch-dotnet-get-started.md) , aby dowiedzieć się, jak używać C# i biblioteki .NET partii do wykonywania prostych obciążenie pracą za pomocą typowych przepływu partii. W tym artykule należy do pierwszej punktów podczas Dowiedz się, jak korzystać z usługi partii. Istnieje także [wersji Python](batch-python-tutorial.md) samouczka.

- Pobierz [Przykłady kodu na GitHub] [ github_samples] aby zobaczyć, jak zarówno C# i Python mogą współpracować z partii do harmonogramu i proces obciążenia próbki.

- Zapoznaj się z [Ścieżka nauki partii] [ learning_path] Aby uzyskać ogólny obraz tego, zasobów dostępnych dla Ciebie Dowiedz się, jak pracować z partii.

[azure_storage]: https://azure.microsoft.com/services/storage/
[api_java]: http://azure.github.io/azure-sdk-for-java/
[api_java_jar]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-batch%22
[api_net]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_nuget]: https://www.nuget.org/packages/Azure.Batch/
[api_net_mgmt]: https://msdn.microsoft.com/library/azure/mt463120.aspx
[api_net_mgmt_nuget]: https://www.nuget.org/packages/Microsoft.Azure.Management.Batch/
[api_nodejs]: http://azure.github.io/azure-sdk-for-node/azure-batch/latest/
[api_nodejs_npm]: https://www.npmjs.com/package/azure-batch
[api_python]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.html
[api_python_pypi]: https://pypi.python.org/pypi/azure-batch
[api_sample_net]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp
[api_sample_python]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch
[api_sample_java]: https://github.com/Azure/azure-batch-samples/tree/master/Java/
[batch_ps]: https://msdn.microsoft.com/library/azure/mt125957.aspx
[batch_rest]: https://msdn.microsoft.com/library/azure/Dn820158.aspx
[free_account]: https://azure.microsoft.com/free/
[github_samples]: https://github.com/Azure/azure-batch-samples
[learning_path]: https://azure.microsoft.com/documentation/learning-paths/batch/
[msdn_benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[batch_explorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[storage_explorer]: http://storageexplorer.com/
[portal]: https://portal.azure.com

[1]: ./media/batch-technical-overview/tech_overview_01.png
[2]: ./media/batch-technical-overview/tech_overview_02.png
