<properties
    pageTitle="Krok 3: Tworzenie nowych eksperyment maszynowego uczenia | Microsoft Azure"
    description="Krok 3 opracowanie skorzystać z przewidywanych rozwiązanie: Utwórz nowe doświadczenia szkolenie w Azure maszynowego uczenia Studio."
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


# <a name="walkthrough-step-3-create-a-new-azure-machine-learning-experiment"></a>Przewodnik krok 3: Tworzenie nowego eksperyment Azure maszynowego uczenia

To jest trzecim krokiem Instruktaż [opracowanie rozwiązania przewidywanych analizy w programie nauki maszynowego Azure](machine-learning-walkthrough-develop-predictive-solution.md)


1.  [Tworzenie obszaru roboczego nauki komputera](machine-learning-walkthrough-1-create-ml-workspace.md)
2.  [Przekaż istniejące dane](machine-learning-walkthrough-2-upload-data.md)
3.  **Tworzenie nowego doświadczenia**
4.  [Szkolenie i ocenianie modeli](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5.  [Wdrażanie usługi sieci web](machine-learning-walkthrough-5-publish-web-service.md)
6.  [Dostęp do usługi sieci web](machine-learning-walkthrough-6-access-web-service.md)

----------

Następny krok w tym instruktażu jest utworzenie doświadczenia w Studio nauki komputera korzystającego z zestawu danych, które możemy przekazane.  

1.  W Studio kliknij pozycję **+ Nowe** w dolnej części okna.
2.  Wybierz pozycję **doświadczenia**, a następnie wybierz "Doświadczenia puste". Wybierz nazwę domyślną doświadczenia w górnej części obszaru roboczego i zmień jej nazwę na nieco opisową

    > [AZURE.TIP] Jest dobrym rozwiązaniem jest wypełnij **Podsumowanie** i **Opis** dla doświadczenia w okienku **Właściwości** . Te właściwości pozwalają możliwość dokumentu doświadczenia, tak aby każda osoba, którzy oglądają go później zrozumiały cele i metodologii.

3.  Na palecie modułu po lewej stronie doświadczenia obszaru roboczego, rozwiń **Zapisane zestawy danych**.
4.  Znajdź zestaw danych utworzone w obszarze **Moje zestawy danych** i przeciągnij go do obszaru roboczego. Można również znaleźć zestaw danych, wpisując nazwę w polu **wyszukiwania** powyżej palety.  

##<a name="prepare-the-data"></a>Przygotowywanie danych
Kliknięcie przycisku port wyjściowy zestawu danych (małe kółko u dołu) i wybranie **Wizualizacja**, możesz wyświetlić 100 pierwszych wierszy danych i niektóre informacje statystyczne dla całego zestawu danych.  

Ponieważ plik danych nie został dostarczony z nagłówkami kolumn, Studio udostępniła ogólnego nagłówki *(Kol1, Kol2 itd.)*. Warto nagłówki nie są niezbędne do tworzenia modelu, ale to na łatwiejsze do pracy z danymi w doświadczeniu. Ponadto firma Microsoft po pewnym czasie publikowania ten model usługi sieci web, nagłówki może pomóc w zidentyfikować kolumny do użytkownika usługi.  

Można dodać nagłówki kolumn przy użyciu [Edytować metadane] [ edit-metadata] modułu.
Używanie [Edytować metadane] [ edit-metadata] moduł, aby zmienić metadane skojarzone z zestawu danych. W tym przypadku zapewnia więcej przyjazne nazwy dla nagłówków kolumn. 

Umożliwia [Edytowanie metadanych][edit-metadata], należy najpierw określić, które kolumny do modyfikacji (w tym przypadku każdy z nich.) Następnie należy określić akcję do wykonania na podstawie tych kolumn (w tym przypadku zmiana nagłówków kolumn.)

1.  Na palecie modułu wpisz "metadanych" w polu **wyszukiwania** . Pojawi się [Edytować metadane] [ edit-metadata] są wyświetlane na liście modułu.
2.  Kliknij i przeciągnij [Edytować metadane] [ edit-metadata] modułu do obszaru roboczego i upuść go pod dodana wcześniej zestawu danych.
3.  Łączenie zestawu danych do [Edytowania metadanych][edit-metadata]: kliknij port wyjściowy zestawu danych (małe kółko w dolnej części zestawu danych.) Następnie przeciągnij do wprowadzania portu [Edytować metadane] [ edit-metadata] (małe kółko w górnej części modułu) następnie zwolnij przycisk myszy. Zestaw danych i moduł nadal połączone, nawet jeśli można poruszać się albo w obszarze roboczym.

    Doświadczenia powinna wyglądać mniej więcej tak:  

    ![Dodawanie metadanych edycji][2]
    
    Czerwony wykrzyknik wskazuje, że firma Microsoft nie zdefiniowano właściwości dla tego modułu jeszcze. Firma Microsoft będzie to zrobić.
    
    > [AZURE.TIP] Możesz dodać komentarz do modułu przez dwukrotne kliknięcie w module i wprowadzania tekstu. To może ułatwić wyświetlenie rzut oka moduł czynności w swojej doświadczeniu. W tym przypadku kliknij dwukrotnie [Edytować metadane] [ edit-metadata] moduł i wpisz komentarz "Dodawanie nagłówków kolumn". Kliknij w dowolnym miejscu jeszcze w obszarze roboczym, aby zamknąć pole tekstowe. Aby wyświetlić komentarz, kliknij strzałkę w dół w module.

4.  Wybierz pozycję [Edytuj metadanych][edit-metadata], następnie w okienku **Właściwości** po prawej stronie obszaru roboczego, kliknij pozycję **Uruchom selektora kolumny**.
5.  W oknie dialogowym **Wybieranie kolumn** , zaznacz wszystkie wiersze w polu **Dostępne kolumny** , a następnie kliknij pozycję > Aby przenieść je do **Wybrane kolumny**.
Okno dialogowe powinna wyglądać następująco: ![selektora kolumny z wszystkie kolumny wybrane][4]
7.  Kliknij **przycisk OK** znacznik wyboru.
8.  W okienku **Właściwości** poszukaj parametr **nowych nazw kolumn** . W tym polu należy wprowadzić listę nazw kolumn 21 w zestawie danych, rozdzielając je przecinkami i w kolejności kolumn. Nazwy kolumn można uzyskać z dokumentacji zestawu danych w witrynie sieci Web UCI lub dla wygody można kopiować i wklejać poniżej:  

          Status of checking account, Duration in months, Credit history, Purpose, Credit amount, Savings account/bond, Present employment since, Installment rate in percentage of disposable income, Personal status and sex, Other debtors, Present residence since, Property, Age in years, Other installment plans, Housing, Number of existing credits, Job, Number of people providing maintenance for, Telephone, Foreign worker, Credit risk  

    W okienku właściwości będzie wyglądać następująco:

    ![Właściwości metadanych edycji][1]

> [AZURE.TIP] Jeśli chcesz sprawdzić nagłówki kolumn, uruchom doświadczenia (kliknij pozycję **Uruchom** poniżej kanwy doświadczenia). Po zakończeniu pracy (zielony znacznik wyboru będą wyświetlane na [Edytowanie metadanych][edit-metadata]), kliknij port wyjściowy [Edytować metadane] [ edit-metadata] moduł i wybierz pozycję **Wizualizacja**. Możesz wyświetlać dane wyjściowe dowolny moduł w taki sam sposób, aby wyświetlić postęp danych za pomocą doświadczenia.

##<a name="create-training-and-test-datasets"></a>Tworzenie szkolenia i testowanie zestawy danych

Następnym krokiem doświadczenia ma wygenerować oddzielnych zestawów danych będzie używana dla szkolenia i badania naszego modelu.

Aby to zrobić, firma Microsoft korzysta z [Danych podziału] [ split] modułu.  

1.  Znajdowanie [Danych podziału] [ split] moduł, przeciągnij go do obszaru roboczego i podłącz ją do ostatniej [Edycji metadanych] [ edit-metadata] modułu.
2.  Domyślnie współczynnik rozdzielania jest 0,5 i **Dzielenie Randomized** parametr jest ustawiony. Oznacza, że losowe połowie dane wyjściowe za pośrednictwem jednego portu, [Podzielone dane] [ split] moduł i połowa do drugiej. Możesz dostosować te, a także parametr **nasion losowo** , aby zmienić podziału między szkoleń i testowania danych. W tym przykładzie pozostanie je jako-jest.
    
    > [AZURE.TIP] Właściwość **ułamek rzędów w pierwszym zestawie danych dane wyjściowe** Określa, ile danych są kierowane do portu wyjściowego po lewej stronie. Na przykład jeśli ustawisz stosunek 0,7, następnie 70% danych jest wynik przez port po lewej stronie i 30% przez prawo port.  
    
3. Kliknij dwukrotnie [Podzielone dane] [ split] modułu i wprowadź komentarz, "szkolenie i badania danych dzielenie 50%". 

Firma Microsoft korzysta z wyjściowe, [Podzielone dane] [ split] modułu jednak firma Microsoft, takich jak, ale Przejdźmy wybrać jako dane szkolenia i po prawej stronie wyjściowe jak badania danych za pomocą raportu po lewej stronie.  

Wymienione w witrynie sieci Web UCI koszt misclassifying ryzyko kredytowe wysoka jako niską jest pięć razy większy niż koszt misclassifying niskie ryzyko kredytowe jako wysoki. Aby to uwzględniać, możemy wygenerować nowy zestaw danych odzwierciedla ta funkcja koszt. W nowy zestaw danych każdy przykład wysokim ryzykiem replikacji pięć razy podczas każdego przykład niskie ryzyko jest wyłączona.   

Możemy replikacja za pomocą kodu R:  

1.  Znajdowanie i przeciągnij [Wykonywanie skryptów R] [ execute-r-script] moduł na doświadczenia obszaru roboczego i łączenie port wyjściowy po lewej stronie, [Podzielone dane] [ split] moduł pierwszy port wejściowy ("Dataset1") [Wykonywanie skryptów R] [ execute-r-script] modułu.
2. Kliknij dwukrotnie [Wykonywanie skryptów R] [ execute-r-script] modułu i komentarz, "Konfigurowanie korekty kosztu".
2.  W okienku **Właściwości** Usuń tekst domyślny w **R** skryptu i wprowadź ten skrypt:

          dataset1 <- maml.mapInputPort(1)
          data.set<-dataset1[dataset1[,21]==1,]
          pos<-dataset1[dataset1[,21]==2,]
          for (i in 1:5) data.set<-rbind(data.set,pos)
          maml.mapOutputPort("data.set")


Trzeba wykonać tej samej operacji replikacji poszczególnych wyników, [Podzielone dane] [ split] modułu Aby uniknąć szkoleń i badania danych takiego samego dostosowania koszt.

1.  Kliknij prawym przyciskiem myszy [Wykonywanie skryptów R] [ execute-r-script] moduł i wybierz polecenie **Kopiuj**.
2.  Kliknij prawym przyciskiem myszy obszar roboczy doświadczenia i wybierz pozycję **Wklej**.
3.  Łączenie pierwszy port wejściowy [Wykonywanie skryptów R] [ execute-r-script] moduł po prawej stronie wyjściowy portu, [Podzielone dane] [ split] modułu. 
4.  U dołu obszaru roboczego kliknij przycisk **Uruchom**. 

> [AZURE.TIP] Kopiowanie modułu wykonywanie skryptów R zawiera sam skrypt jako oryginalny modułu. Podczas kopiowania i wklejania moduł w obszarze roboczym kopię zachowuje wszystkie właściwości strony.  

Nasze doświadczenia teraz wygląda mniej więcej tak:

![Dodawanie podziału moduł i skryptów R][3]

Aby uzyskać więcej informacji na temat korzystania z R skryptów w doświadczeniach z zobacz [rozszerzona usługi przeprowadzić przy użyciu R](machine-learning-extend-your-experiment-with-r.md).

**Następny: [pociągu i oceny modeli](machine-learning-walkthrough-4-train-and-evaluate-models.md)**


[1]: ./media/machine-learning-walkthrough-3-create-new-experiment/create1.png
[2]: ./media/machine-learning-walkthrough-3-create-new-experiment/create2.png
[3]: ./media/machine-learning-walkthrough-3-create-new-experiment/create3.png
[4]: ./media/machine-learning-walkthrough-3-create-new-experiment/columnselector.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
