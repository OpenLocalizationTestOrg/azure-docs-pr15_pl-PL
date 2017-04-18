<properties
    pageTitle="Tworzenie modelu przy użyciu interfejsu użytkownika Recommnendations | Microsoft Azure"
    description="Azure zalecenia maszynowego uczenia — Tworzenie modelu z interfejsu użytkownika zalecenia"
    services="cognitive-services"
    documentationCenter=""
    authors="luiscabrer"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="cognitive-services"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/11/2016"
    ms.author="luisca"/>

# <a name="building-a-model-with-the-recommendations-ui"></a>Tworzenie modelu z interfejsu użytkownika zalecenia

Ten dokument jest przewodnik krok po kroku. Naszym celem jest szczegółową kroki niezbędne do przeszkolenie modelu za pomocą [Interfejsu użytkownika zalecenia](https://recommendations-portal.azurewebsites.net/).
Pośrednictwa wykonywania umożliwia zrozumieć proces tworzenia modelu przed wykonaniem czynności programistycznie. On również zaznajomić z interfejsu użytkownika, która jest przydatne podczas uruchamiania przy użyciu usługi.

Tego ćwiczenia trwa około 30 minut.

<a name="Step1"></a>
## <a name="step-1---sign-in-to-the-recommendations-ui"></a>Krok 1 — logowania w zaleceń interfejsu użytkownika ##

1. Jeśli jeszcze tego nie zrobiono, należy [zapisów](https://portal.azure.com/#create/Microsoft.CognitiveServices/apitype/Recommendations/pricingtier/S1) dla nowej subskrypcji [Interfejsu API zalecenia](https://www.microsoft.com/cognitive-services/en-us/recommendations-api) . Może się Utwórz konto w usłudze **Azure** [http://portal.azure.com/](https://portal.azure.com/#create/Microsoft.CognitiveServices/apitype/Recommendations/pricingtier/S1) i zaloguj się za pomocą konta usługi Azure. Szczegółowe informacje dotyczące proces tworzenia konta znajdują się w *zadania 1* [Przewodnika wprowadzenie](cognitive-services-recommendations-quick-start.md) 

1. Po uzyskaniu **klucza** dla subskrypcji interfejsu API zalecenia, przejdź do [Zalecenia interfejsu użytkownika](https://recommendations-portal.azurewebsites.net/). 

1. Zaloguj się do portalu po wprowadzeniu klucza w polu **Klucz konta** , a następnie kliknij przycisk **Zaloguj** .

    ![Zalecenia dotyczące interfejsu użytkownika: Logowania okno dialogowe.][reco_signin]


<a name="Step2"></a>
## <a name="step-2---lets-gather-some-training-data"></a>Krok 2 — Przejdźmy zbieranie danych szkolenia ##

Przed utworzeniem kompilacji aparat wymaga dwóch rodzajów informacji: plik wykazu i zestaw pliki użycia. 

Plik wykazu zawiera informacje o elementach ofercie do klienta. Plik zastosowania zawiera informacje dotyczące sposobu używania tych elementów lub transakcje z Twojej firmy.

Zazwyczaj czy kwerendy bazy danych magazynu dla następujących rodzajów informacji. Dzisiaj dlatego możesz dowiedzieć się, jak korzystać z interfejsu API zalecenia zostały zamieszczone kilka przykładowych danych dla Ciebie.

Dane można pobrać z [http://aka.ms/RecoSampleData](http://aka.ms/RecoSampleData). Kopiowanie i rozpakuj plik **Books.Zip** do folderu na komputerze lokalnym. Na przykład **c:\data**.

Szczegółowe informacje o schemacie wykazu i zastosowania plików można znaleźć w artykule [Collecting dane do modelu przeszkolenie](cognitive-services-recommendations-collecting-data.md) .
 
W tym celu możemy przejdziesz do pracy z pliku, dzięki czemu nie trzeba bardzo czekać zbyt długo szkolenie. Jeśli użytkownik chce wypróbować bliższy pliku, możemy również umieszczony **MsStoreData.zip** zawierający przykładowe transakcje z Microsoft Store w [tym samym miejscu](http://aka.ms/RecoSampleData).

<a name="Step3"></a>
## <a name="step-3---create-a-project-and-upload-catalog-and-usage-data"></a>Krok 3 — Tworzenie projektu i przekaż wykazu i użycia danych ##

Podczas logowania do [Zalecenia interfejsu użytkownika](https://recommendations-portal.azurewebsites.net/), zostanie wyświetlona strona projektów. Jeśli wcześniej utworzono wszystkich projektów, zobacz je tutaj.
Projekt (nazywane także *modelu* w odwołaniu interfejsu API) jest kontenera dla wykazu i zastosowania danych. Możesz utworzyć kilka *tworzy* wewnątrz projektu. Firma Microsoft przeprowadzi Cię przez proces w następnych kroków.

1. Aby utworzyć nowy projekt, wpisz nazwę w polu tekstowym (następująca: "MyFirstModel" będzie działać) i kliknij pozycję **Dodaj projekt**.
 
    ![Zalecenia dotyczące interfejsu użytkownika: Strona projektów.][reco_projects]

1. Po utworzeniu pobiera projektu, kliknij przycisk **Przeglądaj w poszukiwaniu pliku** w sekcji **Dodawanie pliku wykazu** . Przekazywanie katalogu, które masz w kroku 2. Jeśli został zapisany w *c:\data*, należy przejść do tego folderu.

    ![Zalecenia dotyczące interfejsu użytkownika: Dodawanie danych do projektu.][reco_firstmodel]

1. Po przekazaniu wykazu, kliknij przycisk **Przeglądaj w poszukiwaniu pliku** w sekcji **Dodawanie pliki użycia** . Dodawanie pliku usage_large.txt.

> **Co zrobić, jeśli mam dużego pliku danych dotyczących użycia?**
>
> Można przekazać tylko pliki zastosowania mniejsze niż 200 MB. Który wspomniano, systemu mogą być przechowywane w górę do 2 GB wartość transakcji danych, aby móc przekazywać więcej niż jeden plik, jeśli to konieczne.
> Nie może być konieczne takiej ilości danych, aby wygenerować właściwy model, nie jest po prostu rozmiar danych, która ma znaczenie, ale jakości danych. Są często w celu wyświetlenia danych dotyczących użycia miejsce, w którym większość transakcji są tylko na kilku najpopularniejsze elementy, a istnieje "nieco sygnału" dla innych elementów.

<a name="Step4"></a>
## <a name="step-4---lets-do-some-training"></a>Krok 4 — Przejdźmy niektórych szkolenie! ##

Teraz, gdy przekazano zarówno wykaz, jak i danych dotyczących użycia, możemy przystąpić do przeszkolenie aparat tak, aby go więcej wzorców z naszych danych.

1.  Kliknij przycisk **Konstruuj nowy** .

1.  Wybierz typ konstruowania **zaleceń** . Powiadomienie o obsługujemy tworzenie klasyfikowania i zmianie (często zakupiony razem) wysokości progów tworzyć typy także.

    ![Zalecenia dotyczące interfejsu użytkownika: Tworzenie okno dialogowe.][reco_build_dialog.png]


    Tworzenie zmianie wysokości progów umożliwia identyfikowanie wzorców dla produktów, które są zwykle zakupionych i zużyte w tej samej transakcji.
    Tworzenie klasyfikacji służy do identyfikowania funkcje stopa. 
    Firma Microsoft nie rozpoczynają bardzo głębokich zmianie wysokości progów klasyfikowania tworzy w tym workshop, ale jeśli interesuje Cię należy wyewidencjonować [Tworzenie typów i stronę dokumentacji jakości modelu](cognitive-services-recommendations-buildtypes.md).

1. Kliknij przycisk **Konstruuj** . Procesu tworzenia trwa około pięć minut, jeśli korzystasz z dane źródłowe. Trwa dłużej na większe zestawy danych.

<a name="Step5"></a>
## <a name="step-5---lets-find-out-what-the-machine-learned"></a>Krok 5 — Przejdźmy Dowiedz się, co umiesz komputera! ##

Po zakończeniu kompilacji programu można zauważyć nową kompilację na liście kompilacjach. Zalecenia dotyczące przedmiotu i użytkownika można stosować kwerendy zawierające tej kompilacji.

1. Po zakończeniu kompilacji programu kliknij **wynik**. Umożliwia odtwarzanie z modelem i zobacz, jakie informacje są zalecane.

    ![Zalecenia interfejsu użytkownika: Przycisk wynik][reco_score_button]

1. Wybierz element, aby zobaczyć, które elementy są zwracane jako zalecenia dotyczące tego elementu. Zwróć uwagę, że jeśli jest za mało transakcji przewidywanie zalecenia dotyczące określonego elementu, system nie zwróci wszelkie zalecenia dotyczące tego elementu.  Jeśli jakiegoś powodu elementu, który zwraca żadne zalecenia, spróbuj wyników innych elementów.

<a name="Step6"></a>
## <a name="step-6---next-steps"></a>Krok 6 - następne kroki ##
Gratulacje! masz szkolenia modelu, a następnie masz zalecenia z tego modelu.  Zalecenia dotyczące interfejsu użytkownika jest przydatne narzędzie, które umożliwia wyświetlenie stanu projektów i tworzy. 

Teraz, gdy masz modelu można dowiedzieć się, jak programowo wykonaj powyższe kroki. Aby dowiedzieć się nawiązać połączenie z interfejsu API programowy, zachęcamy zaznacz [Odwołanie interfejsu API zalecenia](http://go.microsoft.com/fwlink/?LinkId=759348) i pobierania [Aplikacji przykładowej zalecenia](http://go.microsoft.com/fwlink/?LinkID=759344).


[reco_signin]:../media/cognitive-services/reco_signin.PNG
[reco_projects]:../media/cognitive-services/reco_projects.PNG
[reco_firstmodel]:../media/cognitive-services/reco_firstmodel.png
[reco_build_dialog.png]:../media/cognitive-services/reco_build_dialog.png
[reco_score_button]:../media/cognitive-services/reco_score_button.png
