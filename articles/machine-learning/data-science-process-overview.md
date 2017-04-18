<properties
    pageTitle="Co to jest proces nauki danych zespołu?  | Microsoft Azure"
    description="Proces nauki danych zespołu jest systematyczną metodę tworzenia inteligentnych aplikacji, które używają zaawansowanej analizy."
    keywords="proces nauki danych, zespoły nauki danych"
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="bradsev" />


# <a name="what-is-the-team-data-science-process-tdsp"></a>Co to jest proces nauki danych zespołu (TDSP)?

Proces nauki danych zespołu (TDSP) zawiera systematyczną podejście do tworzenia inteligentnych aplikacji, która umożliwia członkom zespołu naukowców dane w celu współpracy cyklem pełny działań, aby włączyć te aplikacje na produkty.

W szczególności TDSP udostępnia obecnie zespoły nauki danych:

- **Metoda**: go przedstawiono sekwencji etapów, które Definiowanie Cykl opracowywania zawierające wskazówki na temat Definiowanie problem, analizowanie danych, tworzenie i oceny przewidywanych modeli, a następnie wdrożyć tych modeli w aplikacjach dla przedsiębiorstw.
- **Zasoby**: narzędzia i technologii, takich jak maszyn wirtualnych nauki danych w celu uproszczenia podczas konfigurowania środowiska działania nauki danych i praktyczne wskazówki dotyczące wprowadzający nowych technologii.

Oto Cykl opracowywania procesu nauki danych zespołu:

![Diagram: Danych nauki proces członkom zespołu ](./media/data-science-process-overview/data-science-process-for-teams-diagram.png)


Proces został **iteracyjne**: zrozumienia nowych i istniejących lub uściślenia w modelu rozwoju i wymaga konieczności wprowadzania zmian wcześniej wykonane w sekwencji czynności. Istniejące rozwoju organizacji i planowania procesów projektu są **łatwo dostosować** do pracy z zdefiniowane TDSP sekwencji czynności.

Kroki procedury są diagram i połączone w polu [ścieżka nauki TDSP](https://azure.microsoft.com/documentation/learning-paths/data-science-process/) i opisane poniżej.  


## <a name="planning-and-preparation-steps"></a>Planowanie i przygotowanie czynności

## <a name="p1-business-and-technology-planning"></a>P1. Firma i technologia planowania

Rozpoczynanie projektu analizy, definiując problemów i celów biznesowych. Są one określane w odniesieniu do **wymagań**. Głównym celem tego kroku jest zidentyfikowanie zmiennych biznesowymi o kluczowym (Prognoza lub prawdopodobieństwo kolejności fałszywej, na przykład), które analizy należy przewidywanie spełnia tych wymagań. Dodatkowe planowania jest zazwyczaj niezbędne do zrozumienia **źródeł danych** , aby adres celów projektu z perspektywy analitycznych. Nie jest nietypowych, na przykład, aby dowiedzieć się, że istniejących systemów potrzebne do zbierania i rejestrowania dodatkowych typów danych w celu rozwiązania problemu i osiągnięcia celów projektu. Aby uzyskać instrukcje zobacz [Planowanie środowiska dla procesu zespołu danych naukowych](machine-learning-data-science-plan-your-environment.md) i [scenariusze zaawansowanej analizy w Azure maszynowego uczenia](machine-learning-data-science-plan-sample-scenarios.md).  


## <a name="p2-plan-and-prepare-infrastructure"></a>P2. Planowanie i przygotowywanie infrastruktury

Środowisko analizy procesu nauki danych zespołu składa się z kilku części:

- **obszary robocze danych** miejsce, w którym dane są umieszczane do analizy i modelowania,
- **Przetwarzanie infrastruktury** wstępne przetwarzanie, eksplorowanie i modelowanie danych
- **środowisko uruchomieniowe infrastruktury** operationalize analitycznych modeli i uruchamianie aplikacji, które używają modeli inteligentnego klienta.  

Infrastruktura analizy, który musi zostać skonfigurowane często jest częścią środowisku różni się od core systemów operacyjnych. Ale zwykle wykorzystuje dane z wielu systemów w przedsiębiorstwie, a także źródeł zewnętrznych firmy. Infrastruktura analizy może być czysto opartej na chmurze, lub ustawienia lokalne lub hybrydowych dwóch. Opcji zobacz [Konfigurowanie środowiska nauki danych do użycia w procesie nauki danych zespołu](machine-learning-data-science-environment-setup.md).


## <a name="analytics-steps"></a>Analiza czynności:  

## <a name="1-ingest-the-data-into-the-data-platform"></a>1. mogły zjeść tej ostatniej dane na platformie danych

Pierwszym krokiem jest do wprowadzenia odpowiednich danych z różnych źródeł, albo z poziomu z spoza przedsiębiorstwa, w środowisku analizy przetwarzania danych. **Formatowanie** danych w źródle mogą różnić się od na format wymagany w miejscu docelowym. Dlatego niektóre przekształcenie danych mogą mieć także zostać wykonane przez narzędzia spożyciu. Opcji zobacz [Ładowanie danych do środowiska miejsca do magazynowania dla analizy](machine-learning-data-science-ingest-data.md)

Oprócz początkowej spożyciu danych wiele aplikacji inteligentne są wymagane do regularnie odświeżanie danych w ramach procesu trwających szkoleniowe. Można to zrobić, konfigurując **danych procesu** lub przepływu pracy. To stanowi część iteracyjne część procesu, która zawiera odbudowanie i ponownego obliczania analitycznych modeli używanych przez aplikację inteligentnego wdrożeniem rozwiązania programu. Zobacz, na przykład [Przenoszenie danych z lokalnego programu SQL server do platformy SQL Azure z Azure danych Factory](machine-learning-data-science-move-sql-azure-adf.md).


## <a name="2-explore-and-visualize-the-data"></a>2. Eksplorowanie i wizualizowanie danych

Następnym krokiem jest uzyskanie szczegółowego opis danych badanie jego **Podsumowanie statystyki**relacje, a za pomocą techniki takie **wizualizacji**. Jest to także, gdzie problemy dotyczące **jakości danych** i integralności, takich jak brakujące wartości, niezgodności typów danych i relacje niespójne dane są obsługiwane. Wstępne przetwarzanie przekształcenia są używane do oczyszczania nieprzetworzonych danych przed dalszej analizy i modelowania może być wykonywana. Aby uzyskać opis zobacz [nauki zadań w celu przygotowywanie danych dla rozszerzonego komputera](machine-learning-data-science-prepare-data.md).


## <a name="3-generate-and-select-features"></a>3. Generuj i wybranie funkcji

Naukowców danych, we współpracy z ekspertów domeny, musisz określić funkcje, które Przechwytywanie istotne właściwości zbioru danych i które najlepiej umożliwia przewidywanie zmienne biznesowymi o kluczowym zidentyfikował podczas planowania. Nowe funkcje mogą pochodzić z istniejących danych lub może wymagać dodatkowych danych do pobrania. Ten proces jest nazywany **techniczny funkcji** i jest jednym z najważniejszych kroki tworzenia system skutecznych analiz przewidywanych. Ten krok wymaga kombinacją creative specjalizacji domeny oraz wnioski uzyskane w kroku badań danych. Aby uzyskać instrukcje zobacz [funkcji techniczny w procesie nauki danych zespołu](machine-learning-data-science-create-features.md).


## <a name="4-create-and-train-machine-learning-models"></a>4. Tworzenie i przeszkolenie modeli nauki komputera

Naukowców danych utworzyć analitycznych modele przewidywanie kluczowych zmiennych oznaczona wymagania określone w kroku przy użyciu danych, który został już usunięty i featurized planowania. Komputer nauki systemy wielu **modelowania algorytmów** są stosowane do wielu różnych spraw. Aby uzyskać instrukcje zobacz [jak wybrać algorytmów Azure maszynowego uczenia](machine-learning-algorithm-choice.md).

Naukowców danych należy wybrać model najbardziej odpowiednie dla swoich zadań przewidywania i nie jest nietypowych, że wyniki z wielu modeli trzeba łączyć w celu uzyskania najlepszych wyników. Danych wejściowych dla modelowania zazwyczaj jest losowo podzielone na trzy elementy:

- szkolenie dotyczące zestawu danych,
- zestaw sprawdzania poprawności danych
- badania zbioru danych.

Modele utworzono przy użyciu **zestawu danych szkolenie**. Optymalnego połączenia modeli (z parametrami dostosowanych) jest zaznaczone, uruchamiając modeli i pomiaru błędów przewidywania **zbioru danych i sprawdzanie poprawności**. Na koniec **Testowanie zestawu danych** jest używane do analizowania wydajności wybranym modelu niezależnych danych, który nie został użyty do przeszkolenie lub Sprawdzanie poprawności modelu.  Aby uzyskać procedury zobacz, [jak ma być obliczona wydajność modelu w Azure maszynowego uczenia](machine-learning-evaluate-model-performance.md).


## <a name="5-deploy-and-consume-the-models-in-the-product"></a>5. wdrażanie i używanie modeli produktu

Gdy mamy zestaw modeli, które wykonują również mogą być **operationalized** dla korzystać z innych aplikacji. W zależności od potrzeb biznesowych przewidywań zostały wprowadzone w **czasie rzeczywistym** lub na podstawie **partię** . Aby operationalized, trzeba udostępnić z **Otwórz interfejs API** łatwo używanej z różnych aplikacji modeli takie witryny sieci Web online, arkuszy kalkulacyjnych, pulpitów nawigacyjnych lub linią aplikacji biznesowych i wewnętrznej bazy danych. Zobacz [Wdrażanie usługi sieci web Azure maszynowego uczenia](machine-learning-publish-a-machine-learning-web-service.md).


## <a name="summary-and-next-steps"></a>Podsumowanie i następne kroki

[Proces nauki zespołu danych](https://azure.microsoft.com/documentation/learning-paths/data-science-process/) w modelu jako sekwencji etapów powtórzyć tego **wskazówki** zadań potrzebne do utworzenia inteligentnego aplikacji za pomocą zaawansowanej analizy. Każdy krok także szczegółowe informacje na temat korzystania z różnych technologii Microsoft do wykonania zadania opisane.

Podczas TDSP nie określają określonych typów artefakty **dokumentacji** , jest dobrym rozwiązaniem dokumentu wyniki badań danych i modelowanie oceny, a także zapisywać stosowne kodu, dzięki czemu można należy powtórzyć analizę, gdy jest to wymagane. Ponowne użycie analizy pracy dzięki temu również podczas pracy w innych aplikacjach obejmujących podobne dane i przewidywania zadania.

Podano także instruktaży pełnego zakończenia do końca, pokazujących wszystkie kolejne etapy procesu dla **określonych scenariuszach** . Są one wyświetlane z opisami miniatur w temacie [instruktaży proces nauki danych zespołu](data-science-process-walkthroughs.md) .
