<properties
    pageTitle="Konwertowanie eksperyment szkolenie maszynowego uczenia eksperyment przewidywanych | Microsoft Azure"
    description="Jak przekonwertować eksperyment szkolenie maszynowego uczenia używane do szkolenia modelu analizy przewidywanych do przewidywanych doświadczenia, który można wdrożyć jako usługi sieci web."
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
    ms.date="08/19/2016"
    ms.author="garye"/>

# <a name="convert-a-machine-learning-training-experiment-to-a-predictive-experiment"></a>Konwertowanie eksperyment szkolenie maszynowego uczenia eksperyment przewidywanych

Azure nauki komputera umożliwia tworzenie, testowanie i wdrażanie rozwiązań przewidywanych analizy.

Po utworzeniu i powtórzyć na *poeksperymentować szkolenie* na szkolenie modelu analizy przewidywanych i będzie gotowe do użycia jej do wyniku nowych danych, należy przygotować i usprawnianie swojego doświadczenia wyników. Następnie można wdrażać tego *doświadczenia przewidywanych* jako usługi Azure sieci web tak, aby użytkownicy mogli wysyłać dane do modelu i odbierać przewidywań modelu.

Konwertując eksperyment przewidywanych, jest wyświetlany przeszkolony modelu gotowy do wdrożenia jako usługi sieci web. Użytkownicy usługi sieci web będą wysyłać dane wejściowe do modelu i model będzie odsyła wyniki przewidywania. Tak jak przekonwertować eksperyment przewidywanych mają mieć na uwadze, jak można się spodziewać modelu może być używany przez inne osoby.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Proces konwersji eksperyment szkolenie eksperyment przewidywanych obejmuje trzy kroki:

1.  Zapisywanie modelu nauki komputera, który został szkolenia, a następnie zamień maszynowego uczenia algorytmu i [Model pociągu] [ train-model] moduły z zapisanego przeszkolony modelu.
2.  Przycinanie doświadczenia tylko tych modułów, które są wymagane przez wyników. Eksperyment szkolenie zawiera wiele modułów, które są niezbędne do szkolenia, ale nie są już potrzebne po modelu wyszkolonych i użytkowników do korzystania z wyników.
3.  Definiowanie, gdzie w swojej doświadczeniu można zaakceptować danych od użytkownika usługi sieci web i jakie dane zostaną zwrócone.

## <a name="set-up-web-service-button"></a>Przycisk Konto usługi sieci Web

Po uruchomieniu programu doświadczenia (przycisku**Uruchom** w dolnej części obszaru roboczego doświadczenia), przycisk **Konto usługi sieci Web** (wybierz opcję **Przewidywanych usługi sieci Web** ) wykona dla Ciebie trzy kroki konwersji z doświadczenia szkolenie eksperyment przewidywanych:

1.  Jak moduł w sekcji **Przeszkolony modeli** palety modułu (do lewej strony obszaru roboczego doświadczenia), następnie zastępuje maszynowego uczenia algorytmu i [Model pociągu] zapisuje przeszkolony modelu[ train-model] moduły z modelem przeszkolony zapisane.
2.  Usuwa modułów, które wyraźnie nie są już potrzebne. W naszym przykładzie, ta opcja uwzględnia [Podzielone dane][split], 2 [Modelu wynik][score-model]i [Ocenianie Model] [ evaluate-model] moduły.
3.  Tworzy wprowadzania usługi sieci Web i wyjściowy moduły i dodaje je w domyślnej lokalizacji w sieci doświadczeniu.

Na przykład doświadczenia następujące przygotowuje modelu drzewa decyzji zwiększane dwóch zajęć za pomocą dane przykładowe dane demograficzne:

![Szkolenie dotyczące doświadczenia][figure1]

Moduły w tym doświadczeniu wykonywać zasadniczo cztery różne funkcje:

![Funkcje modułu][figure2]

Po przekonwertowaniu tego doświadczenia szkolenie na eksperyment przewidywanych niektóre z tych modułów nie są już potrzebne, lub mają innych celów:

- **Danych** — w tym zestawie danych przykładowych danych nie jest używana podczas tworzenia wyników — użytkownika usługi sieci web będzie dostarczać dane punktowanych. Jednak metadane z tego zestawu danych, takich jak typy danych jest używany przez przeszkolony modelu. Dlatego należy zachować zestawu danych w przewidywanych doświadczeniu, aby ona metadane.

- Te moduły **przedprodukcyjnym** — w zależności od danych, które zostaną przesłane wyników, może być lub może nie być konieczne przetwarzania przychodzących danych.

    Na przykład w tym przykładzie zestawu danych przykładowych może być brakujące wartości, a także kolumny, które nie są wymagane do przeszkolenie modelu. Dlatego [Oczyść brakujące dane] [ clean-missing-data] module był dołączony do dotyczą brakujące wartości i [Wybieranie kolumn w zestawie danych] [ select-columns] modułu został zakupiony wykluczyć te dodatkowe kolumny z przepływu danych. Jeśli wiesz, czy dane, które zostaną przesłane wyników za pomocą usługi sieci web nie będą mieć brakujące wartości, a następnie można usunąć [Oczyść brakujące dane] [ clean-missing-data] modułu. Ponieważ jednak [Wybieranie kolumn w zestawie danych] [ select-columns] modułu ułatwia definiują zestaw funkcji jest uzyskał, module powinna być.

- **Pociągu** - po pomyślnym szkolenia modelu, można go zapisać jako pojedynczy model przeszkolony modułu. Następnie zastąpić te indywidualne moduły z modelem przeszkolony zapisane.

- **Wynik** — w tym przykładzie moduł podziału umożliwia dzielenie strumienia danych w zestawie danych badania i szkolenia dotyczące danych. W przewidywanych doświadczeniu nie jest potrzebna i może być usuwany. Podobnie [Modelu wynik] 2[ score-model] moduł i [Ocenianie modelu] [ evaluate-model] modułu służą do porównywania wyników z testowymi danymi, aby te moduły nie są również potrzebne w przewidywanych doświadczeniu. Pozostała [Modelu wynik] [ score-model] moduł, jednak jest potrzebny do zwracać wyniku wynik za pośrednictwem usługi sieci web.

Poniżej opisano, jak wygląda naszym przykładzie po kliknięciu pozycji **Ustaw usługi sieci Web**:

![Przekonwertowane przewidywanych doświadczenia][figure3]

Może to być wystarczające przygotować się do doświadczenia do wdrożenia jako usługi sieci web. Można jednak wykonać kilka dodatkowych czynności specyficzne dla swojego doświadczenia.

### <a name="adjust-input-and-output-modules"></a>Dostosowywanie moduły wejścia i wyjścia

W swojej doświadczenia szkolenie używane zestawu danych szkolenia, a następnie czy część przetwarzania uzyskiwanie danych w formularzu, który potrzeby algorytm maszynowego uczenia. Jeśli dane, do których oczekuje się za pośrednictwem usługi sieci web nie będzie to przetwarzanie, można przenieść **Moduł wprowadzania danych usługi sieci Web** do innego węzła w swojej doświadczeniu.

Na przykład **Konto usługi sieci Web** domyślnie umieszcza moduł **wprowadzania usługi sieci Web** w górnej części usługi przepływu danych, jak pokazano na powyższym rysunku. Jednak jeśli dane wejściowe nie będzie to przetwarzanie, następnie możesz ręcznie umieścić **wprowadzania usług sieci Web** poza moduły przetwarzania danych:

![Przenoszenie danych wejściowych usługi sieci web][figure4]

Dane wejściowe dostępne za pośrednictwem usługi sieci web będzie teraz minąć bezpośrednio w module modelu wynik wszelkie wstępne przetwarzanie.

Podobnie domyślnie **Konto usługi sieci Web** umieszcza moduł dane wyjściowe usługi sieci Web u dołu usługi przepływu danych. W tym przykładzie usługi sieci web zwróci użytkownikowi wynik [Modelu wynik] [ score-model] moduł zawierający wektorowa wykonania danych wejściowych oraz oceny wyników.

Jednak jeśli wolisz powrócić coś innego — na przykład, tylko wyników wyniki i nie całą wektor wprowadzania danych — możesz można wstawiać, [Wybieranie kolumn w zestawie danych] [ select-columns] modułu wykluczyć wszystkie kolumny z wyjątkiem wyników wyników. Następnie przenieś moduł **wyjściowych usługi sieci Web** do wyników [Wybierz kolumny w zestawie danych] [ select-columns] modułu:

![Przenoszenie wyjściowych usługi sieci web][figure5]

### <a name="add-or-remove-additional-data-processing-modules"></a>Dodawanie lub usuwanie moduły dodatkowe przetwarzania danych

Jeśli istnieje więcej modułów w swojej doświadczeniu, który nie będą potrzebne podczas tworzenia wyników, można usunąć te. Na przykład, ponieważ możemy przeniesione moduł **wprowadzania usługi sieci Web** do punktu po moduły przetwarzania danych, możemy Usuń [Oczyść brakujące dane] [ clean-missing-data] modułu przewidywanych doświadczenia.

Nasze przewidywanych doświadczenia teraz wygląda następująco:

![Usuwanie moduł dodatkowy][figure6]

### <a name="add-optional-web-service-parameters"></a>Dodawanie parametrów usługi sieci Web

W niektórych przypadkach może być umożliwiające użytkownikom zmienić moduły podczas uzyskiwania dostępu do usługi Usługa sieci web. *Parametry usługi sieci Web* umożliwiają wykonaj następujące czynności.

[Importowanie danych] konfiguruje typowy przykład[ import-data] modułu tak, aby określić użytkowników usługi sieci web wdrożonym innego źródła danych podczas uzyskiwania dostępu do usługi sieci web. Lub skonfigurować [Eksportowanie danych] [ export-data] modułu tak, aby można określić inne miejsce docelowe.

Można definiować parametry usługi sieci Web i kojarzenie ich z jeden lub więcej parametrów modułu i czy są one wymagane czy opcjonalne. Użytkownik usługi sieci web można podać wartości dla tych parametrów podczas uzyskiwania dostępu do usługi i akcje modułu należy wprowadzić odpowiednie zmiany.

Aby uzyskać więcej informacji na temat parametrów usługi sieci Web, zobacz [Przy użyciu parametrów usługi sieci Web Azure maszynowego uczenia ] [ webserviceparameters].

[webserviceparameters]: machine-learning-web-service-parameters.md


## <a name="deploy-the-predictive-experiment-as-a-web-service"></a>Wdrażanie przewidywanych doświadczenia jako usługi sieci web

Teraz, gdy przewidywanych doświadczenia został wystarczająco przygotowany, należy wdrożyć go jako usługi Azure sieci web. Przy użyciu usługi sieci web, użytkownicy mogą wysyłać dane do modelu i model zwróci jej przewidywań.

Aby uzyskać więcej informacji na procesu wdrażania pełną zobacz [Wdrażanie usługi sieci web Azure maszynowego uczenia][deploy]

[deploy]: machine-learning-publish-a-machine-learning-web-service.md


<!-- Images -->
[figure1]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure1.png
[figure2]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure2.png
[figure3]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure3.png
[figure4]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure4.png
[figure5]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure5.png
[figure6]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure6.png


<!-- Module References -->
[clean-missing-data]: https://msdn.microsoft.com/library/azure/d2c5ca2f-7323-41a3-9b7e-da917c99f0c4/
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
[export-data]: https://msdn.microsoft.com/library/azure/7a391181-b6a7-4ad4-b82d-e419c0d6522c/
