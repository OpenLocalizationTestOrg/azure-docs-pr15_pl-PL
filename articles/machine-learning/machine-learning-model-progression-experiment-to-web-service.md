<properties
    pageTitle="Jak modelu maszynowego uczenia kontynuowana z doświadczenia operationalized usługi sieci Web | Microsoft Azure"
    description="Omówienie weryfikowaniu jak usługi Azure maszynowego uczenia modelu wraz z postępem z rozwinięcie eksperymentować do operationalized usługi sieci Web."
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
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="garye"/>


# <a name="how-a-machine-learning-model-progresses-from-an-experiment-to-an-operationalized-web-service"></a>Jak modelu maszynowego uczenia kontynuowana z doświadczenia operationalized usługi sieci Web

Azure maszynowego uczenia Studio znajdują się interakcyjne obszaru roboczego, który umożliwia opracowywanie, uruchamianie, testowanie i iteracyjne ***poeksperymentować*** modelu przewidywanych analizy. Istnieje szereg moduły dostępne, które można:

* Dane wejściowe do swojego doświadczenia
* Modyfikowanie danych
* Przeszkolenie modelu za pomocą komputera nauki algorytmów
* Wynik modelu
* Oceny wyników
* Ostateczne wartości wyjściowych

Po zakończeniu swojego doświadczenia można wdrożyć go jako ***usługi sieci Web klasyczny Azure maszynowego uczenia*** lub ***usługi sieci Web nowego Azure maszynowego uczenia*** , tak, aby użytkownicy mogli przesyła nowe dane i otrzymywać wyniki Wstecz.

W tym artykule możemy nadać omówienie weryfikowaniu jak poeksperymentować z komputera nauki modelu wraz z postępem z rozwinięcie do operationalized usługi sieci Web.

>[AZURE.NOTE] Istnieją inne sposoby opracowywania i wdrażania modeli nauki komputera, ale w tym artykule jest ograniczony do wykorzystania Studio nauki komputera. Aby uzyskać informacje dotyczące sposobu tworzenia klasyczny przewidywanych usługi sieci Web przy użyciu R, zobacz blogu [Tworzenie i wdrażanie przewidywanych Web aplikacji przy użyciu RStudio Azure ML](http://blogs.technet.com/b/machinelearning/archive/2015/09/25/build-and-deploy-a-predictive-web-app-using-rstudio-and-azure-ml.aspx).

Gdy Azure maszynowego uczenia Studio jest przeznaczony do rozszerzania i wdrożeniu *modelu przewidywanych analizy*, prawdopodobnie Studio umożliwia opracowywanie doświadczenia, który nie zawiera modelu przewidywanych analizy. Na przykład doświadczenia może po prostu wprowadzania danych, zmian i dane wyjściowe wyniki. Podobnie jak eksperyment przewidywanych analizy wdrożeniem tego doświadczenia przewidywanych jako usługi sieci Web, ale jest upraszcza proces, ponieważ doświadczenia nie jest szkolenia lub wyników modelu maszynowego uczenia. Gdy nie działa typowe używać Studio w ten sposób, firma Microsoft będzie Uwzględnij go w dyskusji, aby możemy podać pełne wyjaśnienie działania Studio.

## <a name="developing-and-deploying-a-predictive-web-service"></a>Opracowywanie i wdrażanie przewidywanych usługi sieci Web

Poniżej przedstawiono te etapy, które typowe rozwiązania występuje podczas opracowywania oraz wdrożyć go przy użyciu komputera nauki Studio:

![Przepływ wdrożenia](media\machine-learning-model-progression-experiment-to-web-service\model-stages-from-experiment-to-web-service.png)

*Rysunek 1 - etapów modelu typowe przewidywanych analizy*

### <a name="the-training-experiment"></a>Doświadczenia szkolenia

***Szkolenie poeksperymentować*** jest wstępnej fazy opracowywania usługi sieci Web w Studio nauki komputera. Celem doświadczenia szkolenia jest udostępnia lokalizację, aby opracować, testowanie, iteracji i ostatecznie przeszkolenie maszynowego uczenia modelu. Możesz to zrobić jeszcze później wielu modeli jednocześnie poszukaj jest najlepszym rozwiązaniem, ale po wykonaniu eksperymentowanie możesz wybrać jeden szkolenia modelu i wyeliminować reszta z doświadczenia. Przykład opracowywania analizę przewidywanych poeksperymentować, zobacz [opracowanie rozwiązanie przewidywanych analizy oceny ryzyka kredytowej w Azure maszynowego uczenia](machine-learning-walkthrough-develop-predictive-solution.md).

### <a name="the-predictive-experiment"></a>Przewidywanych doświadczenia

Po umieszczeniu modelu przeszkolony w swojej doświadczenia szkolenie kliknij **Konto usługi sieci Web** i wybierz **Przewidywanych usługi sieci Web** w komputerze nauki Studio Aby zainicjować proces konwersji z doświadczenia szkolenie ***przewidywanych doświadczenia***. Celem przewidywanych doświadczenia jest wynik nowe dane w celu ostatecznie staje się operationalized jako usługi sieci Web Azure za pomocą przeszkolony modelu.

Konwersja jest zrobiona przez następujące etapy:

-   Konwertowanie zestawu moduły używane do szkolenia do pojedynczego modułu i zapisz go jako przeszkolony modelu

-   Wyeliminować zbędne moduły nie związane z wyników

-   Dodawanie portów wejściowe i wyjściowe, używających ewentualne usługi sieci Web

Może istnieć więcej zmian, które mają być na przygotowanie do przewidywanych doświadczenia do wdrożenia jako usługi sieci Web. Na przykład jeśli chcesz, aby usługa sieci Web w celu wyprowadzenia tylko niektóre wyników, możesz dodać filtrowania moduł przed port wyjściowy.

W tym procesie konwersji doświadczenia szkolenie nie są usuwane. Po zakończeniu procesu masz dwie karty w Studio: jeden umożliwia doświadczenia szkolenia i jedną dla przewidywanych doświadczenia. Ten sposób, które można wprowadzić zmiany do doświadczenia szkolenie przed wdrażanie usługi sieci Web i odbudowanie przewidywanych doświadczenia. Można też zapisać kopię doświadczenia szkolenia, aby rozpocząć kolejny wiersz eksperyment.

>[AZURE.NOTE] Po kliknięciu **Przewidywanych usługi sieci Web** Rozpoczynanie procesu automatycznego, aby przekonwertować z doświadczenia szkolenie eksperyment przewidywanych, a to sprawdza się dobrze w większości przypadków. W przypadku usługi doświadczenia szkolenie złożonych (na przykład masz wiele ścieżek szkoleń, które można łączyć), warto wykonać konwersja ręcznie. Aby uzyskać więcej informacji zobacz [Konwertowanie eksperyment szkolenie nauki maszynowego do przewidywanych doświadczenia](machine-learning-convert-training-experiment-to-scoring-experiment.md).

### <a name="the-web-service"></a>Usługa sieci Web

Po zakończeniu czy usługi przewidywanych doświadczenia jest gotowa do wysłania, możesz wdrożyć usługi jako usługa sieci Web klasyczny lub usługi sieci Web nowego oparte na Menedżera zasobów Azure. Aby operationalize modelu wdrażając go jako *usługi klasyczny maszynowego Learning w sieci Web*, kliknij **Wdrażanie usługi sieci Web** i wybierz **Wdrażanie usługi sieci Web [klasyczny]**. Aby wdrożyć jako *Usługa nowego komputera Learning w sieci Web*, kliknij **Wdrażanie usługi sieci Web** i wybierz **Wdrażanie usługi sieci Web [nowy]**. Użytkownicy mogą teraz wysyłać dane do modelu przy użyciu usługi sieci Web interfejsu API usługi REST i ponownie otrzymać wyniki. Aby uzyskać więcej informacji zobacz [jak korzystać z usługi Azure maszynowego Learning w sieci Web, który został wdrożony na komputerze nauki doświadczenia](machine-learning-consume-web-services.md).

## <a name="the-non-typical-case-creating-a-non-predictive-web-service"></a>Typowy przypadek: tworzenie-przewidywanych usługi sieci Web

Jeśli do doświadczenia nie przeszkolenie modelu przewidywanych analizy, możesz nie trzeba tworzyć zarówno eksperyment szkolenia i eksperyment wyników — jest tylko jeden doświadczenia i można ją wdrożyć jako usługi sieci Web. Maszynowego uczenia Studio wykrywa, czy usługi doświadczenia nie zawiera modelu przewidywanych analizując moduły, w których użyto.

Po zostały powtórzyć na swojej doświadczenia i uzyskaniu go:

1.  Kliknij **Konto usługi sieci Web** i wybierz pozycję **Przeszkolenie usługi sieci Web** — węzły wejściowe i wyjściowe są automatycznie dodawane.

2.  Kliknij polecenie **Uruchom**

3. Kliknij **Wdrażanie usługi sieci Web** i wybierz **Wdrażanie usługi sieci Web [klasyczny]** lub **Wdrażanie usługi sieci Web [nowy]** w zależności od środowiska, do którego chcesz wdrożyć.

Usługa sieci Web został wdrożony i można uzyskać dostęp do i zarządzać go tak jak przewidywanych usługi sieci Web.

## <a name="updating-your-web-service"></a>Aktualizowanie usługi sieci Web

Teraz, gdy został wdrożony z doświadczenia jako usługi sieci Web, co zrobić, jeśli chcesz je zaktualizować?

To zależy od co należy zaktualizować:

**Aby zmienić wejścia lub wyjścia lub chcesz zmodyfikować, jak usługa sieci Web manipulowanie danymi**

Jeśli nie chcesz zmienić model, ale zmieniana tylko obsługi danych usługi sieci Web, możesz można edytować przewidywanych doświadczenia i kliknij **Wdrażanie usługi sieci Web** i wybierz **Wdrażanie usługi sieci Web [klasyczny]** lub **Wdrażanie usługi sieci Web [nowy]** ponownie. Zatrzymaniu usługi sieci Web, zaktualizowaną doświadczeniu przewidywanych zostanie wdrożony i uruchomieniu usługi sieci Web.

Oto przykład: Załóżmy, że do przewidywanych doświadczenia zwraca cały wiersz danych wejściowych przy użyciu przewidywane wynik. Może być stwierdzisz, że chcesz po prostu zwraca wynik usługi sieci Web. Dlatego możesz dodać moduł **Kolumn programu Project** w przewidywanych doświadczeniu, bezpośrednio przed port wyjściowy, aby wykluczyć kolumny niż wynik. Po kliknięciu **Wdrażanie usługi sieci Web** i ponownie wybierz **Wdrażanie usługi sieci Web [klasyczny]** lub **Wdrażanie usługi sieci Web [nowy]** , usługi sieci Web jest aktualizowana.

**Aby przy przekwalifikowaniu modelu nowymi danymi**

Jeśli chcesz zachować komputera nauki model, ale chcesz przy przekwalifikowaniu go nowymi danymi, dostępne są dwie opcje:

1.  **Ponowne próbkowanie modelu jest uruchomiona usługa sieci Web** — Jeśli chcesz przy przekwalifikowaniu modelu podczas przewidywanych działa usługa sieci Web, możesz to zrobić, wykonując kilka zmian do doświadczenia szkolenia, aby nadać mu ***przekwalifikowania doświadczenia***, a następnie wdrożyć go jako **Usługa *przekwalifikowania sieci web* **. Aby uzyskać instrukcje dotyczące wykonywania tego zadania zobacz [przy przekwalifikowaniu maszynowego uczenia programowy modeli](machine-learning-retrain-models-programmatically.md).

2.  **Wróć do oryginalnej doświadczenia szkolenie Użyj innego szkolenie danych do projektowania modelu** - usługi przewidywanych doświadczenia jest połączony z usługi sieci Web, ale doświadczenia szkolenie nie są bezpośrednio związane w ten sposób. Jeśli zmodyfikować pierwotnym doświadczeniu szkolenia i kliknij pozycję **Ustaw usługi sieci Web**, utworzy *Nowy* przewidywanych poeksperymentować, po wdrożeniu spowoduje utworzenie *nowej* usługi sieci Web. Ją po prostu nie aktualizuje oryginalny usługi sieci Web.

    Jeśli chcesz zmodyfikować doświadczenia szkolenia, otwórz ją i kliknij polecenie **Zapisz jako** do skopiowania. To pozostawić bez zmian pierwotnym doświadczeniu szkolenia, przewidywanych doświadczenia i usługi sieci Web. Teraz można utworzyć nowej usługi sieci Web z wprowadzonymi zmianami. Gdy została wdrożyć usługę sieci Web następnie można zdecydować, czy chcesz zatrzymać poprzedniej usługi sieci Web lub działać razem z pakietem na nową.

**Chcesz przeszkolenie innego modelu**

Jeśli chcesz wprowadzić zmiany w pierwotnym doświadczeniu przewidywanych, takich jak zaznaczanie algorytmu nauki innym komputerze próbuje szkolenie różne metody, itd., a następnie należy postępować zgodnie z procedurą drugi opisany powyżej dla przeszkolenie modelu: Otwórz doświadczenia szkolenia, kliknij pozycję **Zapisz jako** do skopiowania, a następnie uruchom nową ścieżkę opracowywania modelu w dół , tworzenie przewidywanych doświadczenia i wdrażanie usługi sieci web. Spowoduje to utworzenie nowej sieci Web, usługa niepowiązane do oryginalnego — można określić, która z nich lub oba, aby działanie.

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji dotyczących procesu opracowywania i doświadczenia zobacz następujące artykuły:

-   Konwertowanie doświadczenia - [Konwertowanie eksperyment szkolenie nauki maszynowego do przewidywanych doświadczenia](machine-learning-convert-training-experiment-to-scoring-experiment.md)

-   Wdrażanie usługi sieci Web — [Rozmieszczanie usługi Azure maszynowego uczenia sieci web](machine-learning-publish-a-machine-learning-web-service.md)

-   przeszkolenie model — [przy przekwalifikowaniu maszynowego uczenia modeli programowy](machine-learning-retrain-models-programmatically.md)

Aby zapoznać się z przykładami całego procesu zobacz:

-   [Maszynowego uczenia samouczek: tworzenie z pierwszym doświadczeniu w Azure maszynowego uczenia Studio](machine-learning-create-experiment.md)

-   [Przewodnik: Utworzyć rozwiązanie przewidywanych analizy oceny ryzyka kredytowej w nauki maszynowego Azure](machine-learning-walkthrough-develop-predictive-solution.md)