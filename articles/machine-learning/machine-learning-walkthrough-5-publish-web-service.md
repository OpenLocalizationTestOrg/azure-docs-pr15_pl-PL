<properties
    pageTitle="Krok 5: Wdrażanie usługi sieci Web Learning maszyny | Microsoft Azure"
    description="Krok 5 opracowanie instruktażu predykcyjnej rozwiązanie: Wdrażanie cennik w studiu uczenia maszynowego jako usługi sieci Web."
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
    ms.date="10/05/2016"
    ms.author="garye"/>


# <a name="walkthrough-step-5-deploy-the-azure-machine-learning-web-service"></a>Przewodnik krok 5: Wdrożyć usługę Azure Machine Learning w sieci Web

Jest to piąty krok instruktażu, [opracowanie rozwiązania predykcyjną analityków](machine-learning-walkthrough-develop-predictive-solution.md)


1.  [Tworzenie obszaru roboczego](machine-learning-walkthrough-1-create-ml-workspace.md)
2.  [Przekaż istniejące dane](machine-learning-walkthrough-2-upload-data.md)
3.  [Utwórz nowy eksperyment](machine-learning-walkthrough-3-create-new-experiment.md)
4.  [Szkolenia i oceny modeli](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5.  **Wdrażanie usługi sieci Web**
6.  [Dostęp do usługi sieci Web](machine-learning-walkthrough-6-access-web-service.md)

----------

Aby dać innym szansę na wykorzystanie modelu predykcyjnej, opracowanych przez nas w tym instruktażu, możemy wdrożyć go jako usługi sieci Web Azure.

Do tego momentu możemy eksperymentować z naszego modelu szkolenia. Ale wdrożonej usługi jest już nie będzie do szkoleń - generuje poprzez ocenę punktową danych wprowadzonych przez użytkownika na podstawie naszego modelu prognozy. Więc mamy zamiar wykonać kilka prac przygotowawczych do konwersji tego doświadczenia z doświadczenia ***szkolenia*** do ***przewidywanych*** eksperymentu. 

Jest procesem dwuetapowym:  

1. Konwertowanie *eksperymentować szkolenia* stworzyliśmy na *cennik*
2. Wdrażanie cennik jako usługi sieci Web

Po pierwsze, musimy jednak trochę trim tego doświadczenia. Obecnie mamy dwa różne modele w doświadczeniu, ale chcemy tylko jeden model kiedy możemy to wdrożony jako usługi sieci Web.  

Załóżmy, że zdecydowaliśmy, że model drzewa promowanego był lepiej modelu w celu wykorzystania. Tak jest najpierw musisz usunąć [Dwie klasy obsługi wektor maszyny] [ two-class-support-vector-machine] moduł i moduły, które były używane do szkolenia. Można utworzyć kopię eksperymentu najpierw klikając polecenie **Zapisz jako** w dolnej części obszaru roboczego eksperymentu.

Należy usunąć następujących modułów:  

- [Obsługa dwie klasy Vector maszyny][two-class-support-vector-machine]
- [Szkolić Model] [ train-model] i [Modelu wynik] [ score-model] moduły, które były podłączone do niego
- [Normalizowanie danych] [ normalize-data] (oba z nich)
- [Ocena modelu][evaluate-model]

Wybierz moduł i naciśnij klawisz Delete lub kliknij moduł prawym przyciskiem myszy i wybierz polecenie **Usuń**.

Teraz mamy już gotowe do wdrożenia tego modelu za pomocą [Drzewa decyzji wzmocniony dwie klasy][two-class-boosted-decision-tree].

## <a name="convert-the-training-experiment-to-a-predictive-experiment"></a>Konwertuj eksperyment szkolenia do przewidywanych doświadczenia

Konwertowanie do przewidywanych doświadczenia przebiega w trzech krokach:

1. Zapisz model możemy przeszkolonych i Zastąp naszych modułów szkoleniowych
2. Przytnij doświadczenie aby usunąć moduły, które były potrzebne tylko w przypadku szkolenia
3. Zdefiniowanie, gdzie usługa sieci Web akceptuje danych wejściowych i gdzie generuje dane wyjściowe

Na szczęście wszystkie trzy kroki można osiągnąć przez kliknięcie przycisku **Ustaw Web service** w dolnej części obszaru roboczego doświadczenia (wybierz opcję **usługi sieci Web predykcyjne** ).

Po kliknięciu przycisku **Ustaw Web service**, ma miejsce kilka rzeczy:

- Wyszkolonych modelu są zapisywane jako pojedynczy moduł **Wyszkolonych modelu** w palecie moduł z lewej strony obszaru roboczego doświadczenia (można go znaleźć w obszarze **Wyszkolonych modeli**).
- Moduły, które były używane do szkolenia są usuwane. W szczególności:
  - [Drzewo decyzyjne promowanego dwie klasy][two-class-boosted-decision-tree]
  - [Model pociągu][train-model]
  - [Dzielenie danych][split]
  - drugi [Wykonanie skryptu R] [ execute-r-script] moduł, który był używany dla danych testowych
- Zapisane wyszkolonych modelu jest dodany do doświadczenia.
- Moduły **sieci Web service wejściowe** i **wyjściowe usługi sieci Web** są dodawane.

> [AZURE.NOTE] Eksperyment został zapisany w dwóch częściach w obszarze karty, które zostały dodane w górnej części obszaru roboczego eksperyment: pierwotnym doświadczeniu szkolenia jest na karcie **eksperymentować szkolenia**i nowo utworzone cennik jest pod **przewidywanie eksperymentować**.

Potrzebujemy dodatkowych krok z tej danego doświadczenia.
Dodano dwa [Wykonanie skryptu R] [ execute-r-script] modułów do zapewnienia funkcji ważenia do danych dla szkolenia i testowania. Firma Microsoft nie trzeba to zrobić w końcowym modelu.

Studio uczenia maszynowego usunięty jeden [Wykonanie skryptu R] [ execute-r-script] moduł usunięcie [podziału] [ split] modułu. Teraz można teraz usunąć inny i połączyć [Edytora metadanych] [ metadata-editor] bezpośrednio do [Modelu wynik][score-model].    

Nasze doświadczenie powinna teraz wyglądać następująco:  

![Punktacja wyszkolonych modelu][4]  

> [AZURE.NOTE] Możesz się zastanawiać, dlaczego zostało zestawu danych karty kredytowej niemiecki UCI w cennik. Usługa będzie wykorzystywać dane użytkownika, nie oryginalnego zestawu danych, dlaczego więc oryginalnego zestawu danych w modelu?
>
>To PRAWDA, że usługa nie potrzebuje oryginalnych danych karty kredytowej. Ale musi schematu dla danych, który zawiera informacje, takie jak liczbę kolumn są i kolumny, które są numeryczne. Informacje o schemacie jest konieczne do interpretacji danych użytkownika. Pozostawiamy te składniki połączone, tak aby punktacji moduł ma schemat zestawu danych, gdy usługa jest uruchomiona. Dane nie jest używany, tylko schemat.  

Uruchomienie eksperymentu raz ostatni (kliknij **Uruchom**). Jeśli chcesz sprawdzić, czy model jest nadal działa, kliknij przycisk dane wyjściowe [Modelu wynik] [ score-model] moduł i wybierz **Wyświetlanie wyników**. Zobaczysz, że oryginalne dane są wyświetlane wraz z wartość ryzyka kredytu ("etykiety zdobył") i punktacji wartości prawdopodobieństwa ("zdobył prawdopodobieństwa".) 

## <a name="deploy-the-web-service"></a>Wdrażanie usługi sieci Web

Można wdrożyć eksperymentu jako klasyczne usługi sieci Web lub usługą sieci Web oparty na Menedżera zasobów.

### <a name="deploy-as-a-classic-web-service"></a>Wdrażanie jako klasyczne usługi sieci Web ###

Aby wdrożyć klasyczne usługi sieci Web pochodzące z naszego eksperymentu, kliknij **Wdrażania usługi sieci Web** poniżej obszaru roboczego i wybierz **Usługi sieci Web wdrażania [Classic]**. Studio uczenia maszynowego wdraża eksperymentu jako usługi sieci Web i przejście do pulpitu nawigacyjnego dla tej usługi sieci Web. W tym miejscu można powrócić do doświadczenia (**Zobacz migawki** lub **Wyświetlanie najnowszych**) i uruchomić prosty test usługi sieci Web (zobacz **Test usługi sieci Web** poniżej). Istnieje również tutaj informacje dotyczące tworzenia aplikacji, które mogą uzyskać dostęp do usługi sieci Web (więcej o tym w następnym kroku tej procedury).

![Pulpit nawigacyjny usług sieci Web][6]

Usługę można skonfigurować, klikając kartę **Konfiguracja** . W tym miejscu można zmodyfikować nazwę usługi (to ma nazwę eksperymentu domyślnie przyznawany) i nadać jej opis. Możesz również dostarczyć więcej etykiet przyjazne dla kolumn wejściowych i wyjściowych.  

![Konfigurowanie usługi sieci Web][5]  

### <a name="deploy-as-a-new-web-service"></a>Wdrażanie jako usługi nowej sieci Web

Wdrożyć usługę nowej sieci Web pochodzące z naszego eksperymentu, kliknij **Wdrażania usługi sieci Web** poniżej obszaru roboczego i **Wdrażania usługi sieci Web [New]**. Studio uczenia maszynowego przenosi do strony eksperyment wdrażania usług Azure Machine Learning w sieci Web.

Wprowadź nazwę dla usługi sieci Web i wybrać plan taryfowy. Jeśli masz istniejącego planu taryfowego, że można go wybrać, w przeciwnym razie należy utworzyć nowy plan ceny dla usługi. 

1.  Na liście rozwijanej **Plan ceny** wybierz istniejący plan lub zaznacz opcję **Wybierz nowy plan** .
2.  W polu **Nazwa planu**wpisz nazwę, która identyfikuje plan na rachunku.
3.  Wybierz jeden z **Poziomów Plan miesięczny**. Domyślne poziomy planu do planów dla danego regionu domyślne oraz usługi sieci Web jest wdrażany do tego regionu.

Kliknij przycisk **Deploy** a stronie **Szybki Start** otwiera usługi sieci Web.

Usługę można skonfigurować, klikając opcję menu **konfiguracji** . W tym miejscu można zmodyfikować nazwę usługi (to ma nazwę eksperymentu domyślnie przyznawany) i nadać jej opis. 

Aby przetestować wybierz usługi sieci Web, kliknij opcję menu **Test** (zobacz poniżej **testowania usługi sieci Web** ). Aby uzyskać informacje na temat tworzenia aplikacji, które mogą uzyskać dostęp do usługi sieci Web kliknij opcję menu **zużywają** (więcej o tym w następnym kroku tej procedury).

> [AZURE.TIP] Po wdrożeniu go, można zaktualizować usługę sieci Web. Na przykład zmiany modelu Edycja doświadczenia szkolenia, dostosować parametry modelu i kliknij przycisk **Wdrażania usługi sieci Web**. Następnie wybierz **Usługę sieci Web wdrażania [Classic]** lub **Wdrożyć usługę sieci Web [New]**. Gdy ponownie wdrażać eksperymentu zastępuje usługi sieci Web, za pomocą zaktualizowanego modelu.  

## <a name="test-the-web-service"></a>Test usługi sieci Web

### <a name="test-a-classic-web-service"></a>Usługa sieci Web, klasycznym

Możesz sprawdzić tę usługę w studiu uczenia maszynowego lub w portalu Azure Machine Learning sieci Web Services. Testowanie w portalu Azure Machine Learning w sieci Web usług ma tę zaletę, ponieważ umożliwiają włączanie 

**Testowanie w studiu uczenia maszynowego**

Na stronie **pulpitu NAWIGACYJNEGO** kliknij przycisk **Test** w obszarze **Domyślny punkt końcowy**. Okno dialogowe będzie pojawiają się i monit o podanie danych wejściowych dla usługi. Są to te same kolumny, które pojawiły się w oryginale dataset ryzyko kredytowe niemiecki.  

Wprowadź zestaw danych, a następnie kliknij przycisk **OK**. 

**Testowanie w portalu usług sieci Web Azure Machine Learning**

Na stronie **pulpitu NAWIGACYJNEGO** kliknij przycisk **Testuj** łącze Podgląd w obszarze **Domyślny punkt końcowy**. Strona testowa w portalu usług sieci Web Azure Machine Learning dla punktu końcowego usługi sieci Web otwiera i monituje o dane wejściowe dla usługi. Są to te same kolumny, które pojawiły się w oryginale dataset ryzyko kredytowe niemiecki.

Kliknij przycisk **Test żądanie odpowiedź**. 

W usłudze sieci Web, dane przechodzi przez moduł **wprowadzania usługi sieci Web** za pomocą [Edytora metadanych] [ metadata-editor] modułu, a do [Modelu wynik] [ score-model] moduł, gdzie jest oceniane. Wyniki są następnie wyjście z usługi sieci Web przez **dane wyjściowe usługi sieci Web**.

**Testowanie nowej usługi sieci Web**

W portalu usług Azure Machine Learning w sieci Web kliknij przycisk **Test** widoczny u góry strony. Otwiera stronę **testową** i może wprowadzić dane dla usługi. Wejściowego pola wyświetlane odnoszą się do kolumn, które pojawiły się w oryginale dataset ryzyko kredytowe niemiecki. 

Wprowadź zestaw danych, a następnie kliknij przycisk **Test żądanie-odpowiedź**.

Wyniki badania będą wyświetlane na prawej stronie strony w kolumnie wyników. 

Podczas testowania w portalu Azure Machine Learning sieci Web Services, można włączyć dane przykładowe, które służy do testowania usługi żądanie-odpowiedź. Jeśli usługi sieci Web utworzone w studiu uczenia maszynowego, próbki są pobierane dane z danych zużyte do pociągu modelu.

> [AZURE.TIP] Sposób mamy cennik skonfigurowany, cały wynika z [Modelu wynik] [ score-model] modułu są zwracane. Obejmuje to wszystkie dane wejściowe plus wartość ryzyka kredytu i prawdopodobieństwo punktacji. Chciałem wrócić czegoś innego - na przykład, tylko kredyt wiąże się z ryzykiem wartość - a następnie można wstawić [Kolumny projektu] [ project-columns] moduł między [Modelu wynik] [ score-model] i **dane wyjściowe usługi sieci Web** aby wyeliminować kolumn, które nie mają usługi sieci Web, aby powrócić. 

## <a name="manage-the-web-service"></a>Zarządzanie usługą sieci Web

**Zarządzanie usługą sieci Web klasyczne**

Po wdrożeniu usługi sieci Web klasyczny, można nim zarządzać z [wersji zapoznawczej](https://manage.windowsazure.com).

1. Zaloguj się do [wersji zapoznawczej](https://manage.windowsazure.com).
2. W panelu usługi Microsoft Azure kliknij **Uczenia się maszyny**.
3. Kliknij obszar roboczy.
4. Kliknij kartę **usługi sieci Web** .
5. Kliknij usługę sieci Web, utworzonym.
6. Kliknij punkt końcowy "default".

W tym miejscu można wykonywać czynności takie, jak monitorowanie, jak to się robi usługi sieci Web i usprawnień wydajności marka, zmieniając liczbę równoczesnych połączeń z usługi może obsługiwać.
Można nawet opublikowanie usługi sieci Web na rynku usług Azure.

Aby uzyskać więcej szczegółów zobacz:

- [Tworzenie punktów końcowych](machine-learning-create-endpoint.md)
- [Skalowanie usługi sieci Web](machine-learning-scaling-webservice.md)
- [Publikowanie usług Azure Machine Learning w sieci Web do rynku usług Azure](machine-learning-publish-web-service-to-azure-marketplace.md)

**Zarządzanie usługą sieci Web w portalu usług sieci Web Azure Machine Learning**

Po wdrożeniu usługi sieci Web, klasycznym lub nowe, można nim zarządzać z poziomu [portalu usług Azure Machine Learning w sieci Web](https://servics.azureml.net).

Aby monitorować wydajność usługi sieci Web:

1. Zaloguj się w [portalu usług Azure Machine Learning w sieci Web](https://servics.azureml.net).
2. Kliknij opcję **usługi sieci Web**.
3. Kliknij opcję usługi sieci Web.
4. Kliknij **pulpit nawigacyjny**.

----------

**Następny: [Dostęp do usługi sieci Web](machine-learning-walkthrough-6-access-web-service.md)**

[1]: ./media/machine-learning-walkthrough-5-publish-web-service/publish1.png
[2]: ./media/machine-learning-walkthrough-5-publish-web-service/publish2.png
[3]: ./media/machine-learning-walkthrough-5-publish-web-service/publish3.png
[4]: ./media/machine-learning-walkthrough-5-publish-web-service/publish4.png
[5]: ./media/machine-learning-walkthrough-5-publish-web-service/publish5.png
[6]: ./media/machine-learning-walkthrough-5-publish-web-service/publish6.png


<!-- Module References -->
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[metadata-editor]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[normalize-data]: https://msdn.microsoft.com/library/azure/986df333-6748-4b85-923d-871df70d6aaf/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
[two-class-boosted-decision-tree]: https://msdn.microsoft.com/library/azure/e3c522f8-53d9-4829-8ea4-5c6a6b75330c/
[two-class-support-vector-machine]: https://msdn.microsoft.com/library/azure/12d8479b-74b4-4e67-b8de-d32867380e20/
[project-columns]: https://msdn.microsoft.com/en-us/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/