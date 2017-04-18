<properties
    pageTitle="Krok 2: Przekazywanie danych do eksperyment maszynowego uczenia | Microsoft Azure"
    description="Krok 2 opracowanie skorzystać z przewidywanych rozwiązanie: przekazywania przechowywanych danych publicznych w Azure maszynowego uczenia Studio."
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016" 
    ms.author="garye"/>


# <a name="walkthrough-step-2-upload-existing-data-into-an-azure-machine-learning-experiment"></a>Przewodnik krok 2: Przekazywanie istniejących danych do doświadczenia nauki maszynowego Azure

To jest drugi krok Instruktaż [opracowanie rozwiązania przewidywanych analizy w programie nauki maszynowego Azure](machine-learning-walkthrough-develop-predictive-solution.md)


1.  [Tworzenie obszaru roboczego nauki komputera](machine-learning-walkthrough-1-create-ml-workspace.md)
2.  **Przekaż istniejące dane**
3.  [Tworzenie nowego doświadczenia](machine-learning-walkthrough-3-create-new-experiment.md)
4.  [Szkolenie i ocenianie modeli](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5.  [Wdrażanie usługi sieci web](machine-learning-walkthrough-5-publish-web-service.md)
6.  [Dostęp do usługi sieci web](machine-learning-walkthrough-6-access-web-service.md)

----------

Aby opracować model przewidywanych ryzyka kredytowej, potrzebujemy używamy szkolenie i przetestowanie modelu danych. Dla tego instruktażu użyjemy "UCI Statlog (niemiecki dane kredytowej) zestawu danych" z repozytorium UCI maszynowego uczenia. Możesz go znaleźć tutaj:  
<a href="http://archive.ics.uci.edu/ml/datasets/Statlog+(German+Credit+Data)">http://Archive.ICS.uci.edu/ml/DataSets/Statlog+(German+Credit+Data)</a>

Użyjemy plik o nazwie **german.data**. Pobierz ten plik na lokalnym dysku twardym.  

Tego zestawu danych zawiera wiersze zmiennych 20 do 1000 ostatnich szukających kredytowej. Zmienne 20 reprezentują wektorowa funkcji zestawu danych, która udostępnia cechy identyfikacyjne dla każdego zgłaszającego kredytowej. Dodatkową kolumnę w każdym wierszu reprezentuje ryzyko kredytowe obliczeniowe kandydata, z kandydatami 700 uznane jako niskie ryzyko kredytowe i 300 wysokim ryzykiem.

W witrynie sieci Web UCI opisano atrybuty wektora funkcji, które obejmują informacji finansowych, historię zatrudnienia statusu i informacji osobistych. Dla każdego zgłaszającego binarne ocena została danej, wskazująca, czy są one niski lub wysoki kredytu ryzyka.  

Użyjemy te dane do modelu analizy przewidywanych przeszkolenie. Gdy mamy już gotowe, naszego modelu należy można akceptować informacje dla nowych użytkowników i przewidywania, czy są one ryzyka wysoki lub niski.  

Oto jeden interesujące dołączony. Opis zestawu danych wyjaśniono, że misclassifying osoby, tak jak 5 razy więcej kosztów finansowych instytucji niż misclassifying niskie ryzyko kredytowe jako wysoki niskie ryzyko kredytowe Uczestnicząc w rzeczywistości ryzyko kredytowe wysoki. Najprostszy sposób to brać pod uwagę w naszym doświadczeniu jest zduplikowane (5 razy) te pozycje reprezentujących osoby z ryzyko kredytowe wysoki. Następnie jeśli modelu klasyfikuje ryzyko kredytowe wysoka jako niską, będzie on wykonywać tego błędu klasyfikacji 5 godzin raz dla wszystkich duplikatów. Dzięki temu zwiększy koszt ten błąd w wynikach szkolenie.  

##<a name="convert-the-dataset-format"></a>Konwertowanie formatu zestawu danych
Oryginalny zestawu danych w formacie oddzielone puste. Maszynowego uczenia Studio sprawdza się lepiej się plik wartości rozdzielanych przecinkami (CSV), firma Microsoft będzie Konwertowanie zestawu danych, zamieniając spacje przecinki.  

Istnieje wiele sposobów przekonwertować te dane. Jednym ze sposobów jest za pomocą następującego polecenia środowiska Windows PowerShell:   

    cat german.data | %{$_ -replace " ",","} | sc german.csv  

Innym sposobem jest przy użyciu polecenia sed Unix:  

    sed 's/ /,/g' german.data > german.csv  

W obu przypadkach został utworzony w wersji danych w pliku o nazwie **german.csv** , która będzie używana w naszym doświadczeniu przecinkami.

##<a name="upload-the-dataset-to-machine-learning-studio"></a>Przekazywanie zestawu danych do komputera nauki Studio

Gdy dane przekonwertowany na CSV format, trzeba przekazać go do komputera nauki Studio. Aby uzyskać więcej informacji na temat rozpoczynania pracy z komputera nauki Studio zobacz [Microsoft Azure maszynowego uczenia Studio dla użytkowników domowych](https://studio.azureml.net/).

1.  Otwórz maszynowego uczenia Studio ([https://studio.azureml.net](https://studio.azureml.net)). Gdy zostanie wyświetlone pytanie, aby się zalogować, użyj konta, które zostały określone jako właściciel obszaru roboczego.
1.  Kliknij kartę **Studio** u góry okna.
1.  Kliknij pozycję **+ Nowe** w dolnej części okna.
1.  Wybierz **zestaw danych**.
1.  Wybierz pozycję **z pliku lokalnego**.
1.  W oknie dialogowym **Przekaż nowy zestaw danych** kliknij przycisk **Przeglądaj** i Znajdź plik **german.csv** , który został utworzony.
1.  Wprowadź nazwę zestawu danych. Dla tego instruktażu będzie nazywamy je "UCI niemiecki karty kredytowej Data".
1.  Typ danych wybierz **uniwersalny plik CSV bez nagłówka (. nh.csv)**.
1.  Jeśli chcesz, Dodaj opis.
1.  Kliknij **przycisk OK**.  

![Przekazywanie zestawu danych][1]  


W module zestawu danych, które pomagają w doświadczeniu wydajnie przekazuje dane.

> [AZURE.TIP] Aby zarządzać zestawy danych, które zostały przekazane do Studio, kliknij kartę **zestawy danych** po lewej stronie okna Studio.

Aby uzyskać więcej informacji na temat importowania różnych typów danych do doświadczenia zobacz [Importowanie danych szkolenie w Azure maszynowego uczenia Studio](machine-learning-data-science-import-data.md).

**Następny: [Tworzenie nowego doświadczenia](machine-learning-walkthrough-3-create-new-experiment.md)**

[1]: ./media/machine-learning-walkthrough-2-upload-data/upload1.png
