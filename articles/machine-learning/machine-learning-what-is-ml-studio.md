<properties 
    pageTitle="Co to jest Azure maszynowego uczenia Studio? | Microsoft Azure"
    description="Omówienie Azure ML Studio narzędzia przeciągania i upuszczania szybkiego tworzenia modeli z biblioteki gotowych do użycia algorytmów i modułów."
    keywords="Azure maszynowego uczenia, azure ml ml studio"
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/09/2016"
    ms.author="garye"/>

# <a name="what-is-azure-machine-learning-studio"></a>Co to jest Azure maszynowego uczenia Studio?

Microsoft Azure maszynowego uczenia Studio jest to narzędzie współpracy, przeciągnij i upuść, które służy do tworzenia, testowanie i wdrażanie rozwiązań przewidywanych analizy warunkowej danych. Maszynowego uczenia Studio modele są publikowanie jako usługi sieci web, które mogą być łatwo wykorzystane przez niestandardowe aplikacje lub narzędzi analizy Biznesowej, takich jak program Excel.

Maszynowego uczenia Studio to miejsce, w którym spełniają nauki danych, analizy przewidywanych chmurze zasobów i danych.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="the-machine-learning-studio-interactive-workspace"></a>Obszar roboczy interakcyjnych maszynowego uczenia Studio

Aby opracować model przewidywanych analizy, możesz zwykle przy użyciu danych z jednej lub kilku źródeł, przekształcanie analizowanie danych za pomocą różnych manipulowania danymi i funkcji statystycznych i wygenerować zestaw wyników. Opracowywanie modelu tak jest procesem iteracyjne. Tak jak można zmodyfikować różne funkcje i ich parametry, wyniki zbieżne aż do uzyskania czy masz przeszkolony, wydajne modelu.

**Azure maszynowego uczenia Studio** umożliwia interakcyjne, wizualne obszaru roboczego łatwe tworzenie, testowanie i przejść na modelu przewidywanych analizy. Możesz przeciągać i upuszczać ***zestawy danych*** i analiza ***moduły*** na interakcyjne kanwę, łącząc obiektów, aby ***poeksperymentować***, który jest uruchamiany w Studio nauki komputera. Aby przejść na projekcie modelu edytowanie doświadczenia, Zapisz kopię w razie potrzeby i uruchom go ponownie. Gdy wszystko będzie już gotowe, można przekonwertować do ***szkolenia poeksperymentować*** ***przewidywanych doświadczenia***i opublikować go jako ***usługi sieci web*** tak, aby modelu są dostępne dla innych osób.

>[AZURE.TIP] Aby pobrać i wydrukować diagram zestawienie możliwości maszynowego uczenia Studio, zobacz [całościowego diagramu możliwości Azure maszynowego uczenia Studio](machine-learning-studio-overview-diagram.md).

Istnieje nie wymagają, programowania tylko wizualnie łączenia zestawy danych i moduły do utworzenia modelu przewidywanych analizy.

![Diagram ML Studio Azure: tworzenie doświadczeń, Odczyt danych dla wielu źródeł, zapis uzyskał danych, pisanie modeli.][ml-studio-overview]

## <a name="get-started-with-machine-learning-studio"></a>Rozpoczynanie pracy z komputera nauki Studio

Podczas wpisywania [Maszynowego uczenia Studio](https://studio.azureml.net) zostanie wyświetlona strona **dla użytkowników domowych** . W tym miejscu można wyświetlić dokumentacji, klipów wideo, seminaria w sieci Web i znaleźć inne przydatne zasoby.

Istnieją trzy karty u góry: **Strona główna** (której uruchamiasz), **Studio**i **galerii**.

### <a name="studio"></a>Studio

Kliknij kartę **Studio** i zostanie wyświetlony monit o zalogowanie się przy użyciu konta Microsoft lub konta służbowego lub szkolnego. Po zalogowaniu się po lewej stronie zostanie wyświetlone następujących kartach:

- **Projekty** — kolekcje doświadczeń, zestawy danych notesów i inne zasoby stanowiące pojedynczego projektu
- **Doświadczenia** - doświadczeń, które zostały utworzone, uruchom i zapisywane jako wersje robocze
- **Usług sieci WEB** — wdrożono z doświadczeń z usług sieci Web
- **NOTESY** — notesy Jupyter, które zostały utworzone
- **Zestawy danych** — zestawy danych, które zostały przekazane do Studio
- **Modele PRZESZKOLONY** - modeli, które szkolenie w doświadczeniach i zapisane w Studio
- **Ustawienia** — to zbiór ustawień, które można użyć do skonfigurowania swojego konta i zasobów.

### <a name="gallery"></a>Galeria

Kliknij kartę **Galeria** i zostaniesz przeniesiony do galerii analizy Cortana. Galeria to miejsce, w którym społeczności naukowców danych i deweloperów mogą udostępniać rozwiązań utworzonych przy użyciu składników pakietu analizy Cortana.

Aby uzyskać więcej informacji o galerii, zobacz [Udostępnianie i odnajdowanie rozwiązania w galerii analizy Cortana](machine-learning-gallery-how-to-use-contribute-publish.md).

## <a name="components-of-an-experiment"></a>Składniki doświadczenia

Doświadczenia składa się z zestawów danych, które odwołują się do modułów analitycznych, które łączenia tworzenia modelu przewidywanych analizy. W szczególności eksperyment prawidłowych ma następujące cechy:

- Doświadczenie co najmniej jeden zestaw danych i jednego modułu
- Zestawy danych może być połączony tylko z modułów
- Moduły może być połączony z zestawy danych lub innych modułów
- Wszystkie portów wejściowych dla modułów musi mieć kilka połączenie dla przepływu danych
- Wszystkie wymagane, należy ustawić parametry dla każdego modułu

Doświadczenia można utworzyć od podstaw, lub za pomocą istniejącego doświadczenia próbki jako szablon. Aby uzyskać więcej informacji zobacz [Używanie przykładowych doświadczenia w celu tworzenie nowych doświadczeń](machine-learning-sample-experiments.md).

Przykład tworzenia prostej doświadczenia zobacz [Tworzenie prostego doświadczenia w Azure maszynowego uczenia Studio](machine-learning-create-experiment.md).

Aby dokładniejszy opis utworzenie rozwiązania przewidywanych analizy zobacz [Przygotowanie przewidywanych rozwiązanie z Azure maszynowego uczenia](machine-learning-walkthrough-develop-predictive-solution.md).

### <a name="datasets"></a>Zestawy danych

Zestaw danych jest dane, które zostało ono przekazane Studio nauki komputera, aby mogą być używane w procesie modelowania. Przykładowe zbiorów danych jest dostarczana z komputera nauki Studio umożliwia wypróbowanie i możesz przekazać więcej zestawów danych, których potrzebujesz. Oto kilka przykładów uwzględnione zestawy danych:

- **MPG danych dla różnych samochodów** — km / wartości galon (MPG) dla samochodów oznaczone numerem butli, Angielski koń parowy itd.
- **Matki raka danych** — dane diagnostyki raka matki.
- **Las uruchamiany danych** — rozmiarów fire las w północno-wschodnich Portugalia.

Podczas tworzenia doświadczenia możesz wybrać z listy dostępne zestawy danych z lewej strony obszaru roboczego.

Aby uzyskać listę zestawy danych przykładowych objęte maszynowego uczenia Studio zobacz [Korzystanie z zestawów danych przykładowych w Azure maszynowego uczenia Studio](machine-learning-use-sample-datasets.md).

### <a name="modules"></a>Moduły

Moduł to algorytm, które można wykonywać na danych. Maszynowego uczenia Studio zawiera liczbę moduły od funkcji ingress danych do szkolenia, wyników i procesów sprawdzania poprawności. Oto kilka przykładów uwzględniane moduły:

- [Konwertowanie na ARFF] [ convert-to-arff] -Konwertuje zestaw danych .NET seryjnych do formatu pliku relacja atrybutu (ARFF).
- [Obliczanie statystyk szkół podstawowych] [ elementary-statistics] -oblicza statystyki szkół podstawowych, takich jak średnia, odchylenia standardowego itp.
- [Regresji liniowej] [ linear-regression] -tworzy online gradientu oparte na zejścia modelu regresji liniowej.
- [Wynik modelu] [ score-model] -widoczne przeszkolony model klasyfikacji lub regresji.

Podczas tworzenia doświadczenia możesz wybrać z listy dostępnych modułów z lewej strony obszaru roboczego.  

Moduł może być zestaw parametry, które umożliwia skonfigurowanie algorytmów wewnętrznych modułu. Po wybraniu moduł w obszarze roboczym parametrów modułu są wyświetlane w okienku **Właściwości** po prawej stronie obszaru roboczego. Możesz zmienić parametrów w tym okienku, aby dostosować modelu.

Dla niektórych pomocy poruszanie się dużej biblioteki maszynowego uczenia algorytmów dostępna, zobacz [jak wybrać algorytmów Microsoft Azure maszynowego uczenia](machine-learning-algorithm-choice.md).

## <a name="deploying-a-predictive-analytics-web-service"></a>Wdrażanie usługi sieci web analytics przewidywanych

Gdy modelu analizy przewidywanych będzie gotowa, należy wdrożyć go jako usługi sieci web bezpośrednio z komputera nauki Studio. Aby uzyskać więcej informacji o tym procesie zobacz temat [Deploy usługi sieci web Azure maszynowego uczenia](machine-learning-publish-a-machine-learning-web-service.md).

[ml-studio-overview]:./media/machine-learning-what-is-ml-studio/azure-ml-studio-diagram.jpg

<!-- Module References -->
[convert-to-arff]: https://msdn.microsoft.com/library/azure/62d2cece-d832-4a7a-a0bd-f01f03af0960/
[elementary-statistics]: https://msdn.microsoft.com/library/azure/3086b8d4-c895-45ba-8aa9-34f0c944d4d3/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
