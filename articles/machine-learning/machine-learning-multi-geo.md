<properties
   pageTitle="Wielokrotne Geo dokumentacji | Microsoft Azure"
   description="Dowiedz się, jak utworzyć obszar roboczy i publikowanie usługi sieci web w regionie Azure różnych z Południowej Stanów Zjednoczonych centralnej (SCUS) Azure region."
   services="machine-learning"
   documentationCenter=""
   authors="tedway"
   manager="jhubbard"
   editor="rmca14"
   tags=""/>

<tags
   ms.service="machine-learning"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/16/2016"
   ms.author="tedway; neerajkh"/>

# <a name="multi-geo-help-documentation"></a>Wielokrotne Geo dokumentacji

W tym artykule opisano, jak można utworzyć obszaru roboczego i publikować usługi sieci web w innych regionach Azure.

## <a name="create-a-workspace"></a>Tworzenie obszaru roboczego

1. Zaloguj się do portalu klasyczny Azure.

2.  Kliknij pozycję **+ Nowe** > **usług danych** > **MASZYNOWEGO UCZENIA** > **szybkiego tworzenia**.  W obszarze **Lokalizacja** zaznacz innego regionu, takich jak **Azji Południowo-Wschodniej**.
![Wielokrotne Geo pomocy obraz 1][1]
3. Zaznacz obszar roboczy, a następnie kliknij przycisk **Zaloguj się do ML Studio**.
![Wielokrotne Geo pomocy obraz 2][2]

4. Teraz masz obszaru roboczego w innym regionie, których można używać tak samo jak inne obszaru roboczego. Aby przełączać się między obszarów roboczych, poszukaj do prawego górnego rogu ekranu. Kliknij listę rozwijaną, wybierz region, a następnie wybierz obszaru roboczego. Wszystko jest lokalne do obszaru roboczego; na przykład wszystkich usług sieci web utworzona na podstawie obszaru roboczego będzie, w tym samym regionie, który obszar roboczy znajduje się w.
![Wielokrotne Geo pomocy obraz 3][3]

## <a name="open-an-experiment-from-gallery"></a>Otwieranie doświadczenia z galerii

Jeśli otworzysz doświadczenia z galerii, można też zaznaczyć region, które chcesz skopiować doświadczenia.

![Wielokrotne Geo pomocy obraz 4][4a]

## <a name="web-service-management"></a>Zarządzanie usługą sieci Web

Programowo zarządzać usług sieci web, takich jak dla przeszkolenie, użyj adresu określonego regionu:
- https://asiasoutheast.Management.azureml.NET
- https://europewest.Management.azureml.NET

### <a name="things-to-note"></a>Uwag

1.  Można kopiować tylko doświadczeń między obszary robocze, które należą do tego samego regionu w ten sposób. Jeśli chcesz skopiować doświadczenia w obszarach roboczych w różnych regionach, można użyć polecenia [programu PowerShell](http://aka.ms/amlps) [*AmlExperiment Kopiuj*](https://github.com/hning86/azuremlps/blob/master/README.md#copy-amlexperiment) do wykonania, który. Inne obejście jest opublikowanie doświadczenia w galerii w trybie spoza listy, otwórz go w obszarze roboczym z innego regionu.
2.  Selektor regionu będą wyświetlane tylko obszarów roboczych dla jednego regionu jednocześnie. W przyszłości można wyświetlić pełną listę obszarów roboczych, masz dostęp do całej wszystkich regionów w tym samym czasie.  
3.  Bezpłatne obszaru roboczego lub gości (anonimowy) w obszarze roboczym zostanie utworzona i obsługiwany w centralnej Południowej Stany Zjednoczone W przyszłości można utworzyć Free/gości obszarów roboczych w obszarze, w którym możesz wybrać.  
4.  Używany do obszaru roboczego w Azji Południowo usług sieci Web będzie również obsługiwane w Azji Południowo. W przyszłości można mieć możliwości tworzenia doświadczenia w jednym regionie i wdrażanie wygenerowane punkty końcowe usługi sieci web do różnych regionów.  

## <a name="more-information"></a>Więcej informacji

Zadaj pytanie na [forum Azure maszynowego uczenia](https://social.msdn.microsoft.com/Forums/azure/home?forum=MachineLearning).

<!--Image references-->
[1]: ./media/machine-learning-multi-geo/multi-geo_1.png
[2]: ./media/machine-learning-multi-geo/multi-geo_2.png
[3]: ./media/machine-learning-multi-geo/multi-geo_3.png
[4a]: ./media/machine-learning-multi-geo/multi-geo_4a.png
