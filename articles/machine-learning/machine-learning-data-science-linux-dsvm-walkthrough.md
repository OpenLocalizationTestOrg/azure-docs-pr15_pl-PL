<properties 
    pageTitle="Nauka danych na Linux danych nauki maszyny wirtualnej | Microsoft Azure" 
    description="Jak wykonać kilka typowych zadań nauki danych za pomocą maszyn wirtualnych nauki danych Linux." 
    services="machine-learning"
    documentationCenter="" 
    authors="bradsev" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/12/2016" 
    ms.author="bradsev;paulsh" />


# <a name="data-science-on-the-linux-data-science-virtual-machine"></a>Nauka danych na Linux danych nauki maszyny wirtualnej

W tym instruktażu pokazano, jak wykonać kilka typowych zadań nauki danych za pomocą maszyn wirtualnych nauki danych Linux. Linux danych nauki maszyn wirtualnych (DSVM) jest dostępny na Azure, który jest zainstalowany z zestaw narzędzi powszechnie używany do analizy danych i nauka maszynowego obraz maszyn wirtualnych. Składniki oprogramowania są wyszczególnione w temacie [Obsługa administracyjna maszyny wirtualnej nauki danych Linux](machine-learning-data-science-linux-dsvm-intro.md) . Obraz maszyn wirtualnych można łatwo rozpocząć pracę, wykonując nauki danych w minutach, bez konieczności instalowania i konfigurowania poszczególnych narzędzi pojedynczo. Można łatwo rozbudowy Głosowa, w razie potrzeby i zatrzymywać go nieużywane. Dlatego tego zasobu jest elastyczne i wydajne. 

Zadania nauki danych pokazano w Obserwuj ten Instruktaż z instrukcjami podanymi w [Proces nauki danych zespołu](https://azure.microsoft.com/documentation/learning-paths/data-science-process/). Ten proces dostarcza systematyczną rozwiązanie do nauki danych, które umożliwia zespołom ekspertów naukowych danych, aby efektywna współpraca z cyklem tworzenia aplikacji inteligentnego. Proces nauki danych udostępnia iteracyjne framework dla nauki danych, który może nastąpić osoba.

Firma Microsoft analizowanie zestawu danych [spambase](https://archive.ics.uci.edu/ml/datasets/spambase) w tym instruktażu. To jest zbiór wiadomości e-mail, które mają być oznaczane jako wiadomości-śmieci lub szynki (to znaczy nie są wiadomości-śmieci) i zawiera również kilka statystyki na zawartość tej wiadomości e-mail. W drugiej, ale jednej sekcji omówiono statystyki uwzględnione. 


## <a name="prerequisites"></a>Wymagania wstępne

Zanim będzie można używać maszyny wirtualnej nauki danych Linux, musisz mieć następujące czynności:

- **Azure subskrypcję**. Jeśli nie masz już jeden, zobacz [Tworzenie dzisiaj bezpłatne konto Azure](https://azure.microsoft.com/free/).
- [**Linux danych nauki maszyn wirtualnych**](https://azure.microsoft.com/marketplace/partners/microsoft-ads/linux-data-science-vm). Uzyskać informacji na temat obsługi administracyjnej tego maszyn wirtualnych zobacz [Obsługa administracyjna maszyny wirtualnej nauki danych Linux](machine-learning-data-science-linux-dsvm-intro.md). 
- [X2Go](http://wiki.x2go.org/doku.php) zainstalowany na Twoim komputerze i otworzyć sesję XFCE. Aby uzyskać informacje o instalowaniu i konfigurowaniu **X2Go klienta**zobacz [Instalowanie i konfigurowanie X2Go klienta](machine-learning-data-science-linux-dsvm-intro.md#Installing-and-configuring-X2Go-client). 
- **Konto AzureML**. Jeśli nie masz jeszcze jedną Załóż nową na stronie [głównej AzureML](https://studio.azureml.net/). Ma zastosowania bezpłatne warstwa ułatwiające rozpoczęcie pracy.


## <a name="download-the-spambase-dataset"></a>Pobierz zestaw danych spambase

Zestaw danych [spambase](https://archive.ics.uci.edu/ml/datasets/spambase) jest stosunkowo małego zestawu danych, który zawiera przykłady tylko 4601. Jest to wygodny rozmiar ma być używana w pokazujące, że niektóre z najważniejszych funkcji programu maszyn wirtualnych nauki danych, ponieważ zachowuje wymagań dotyczących zasobów niewielkie.

>[AZURE.NOTE] W tym instruktażu został utworzony na D2 o rozmiarze w wersji 2 Linux danych nauki maszyny wirtualnej. Rozmiar DSVM jest w stanie obsługi procedury opisane w tym instruktażu.

Jeśli potrzebujesz więcej miejsca, można utworzyć dodatkowe dyski i dołączyć je do swojego maszyn wirtualnych. Te dyski za pomocą wygenerowanie Azure, aby dane są zachowywane nawet wtedy, gdy serwer jest reprovisioned ze względu na zmiany rozmiaru lub jest zamknięty. Aby dodać dysk i dołączyć go do swojej maszyn wirtualnych, postępuj zgodnie z instrukcjami [Dodaj dysk do maszyny Linux](../virtual-machines/virtual-machines-linux-add-disk.md). Poniższe czynności za pomocą interfejsu wiersza polecenia Azure (polecenie Azure), który jest już zainstalowany na DSVM. Dlatego poniższe procedury można całkowicie z maszyn wirtualnych się. Inną opcję, aby zwiększyć miejsca do magazynowania jest korzystanie z [plików Azure](../storage/storage-how-to-use-files-linux.md).

Aby pobrać dane, Otwórz okno końcowych i tego polecenia:

    wget http://archive.ics.uci.edu/ml/machine-learning-databases/spambase/spambase.data

Pobrany plik nie ma wiersza nagłówka, więc warto utworzyć inny plik, który ma nagłówka. Uruchom to polecenie, aby utworzyć plik z nagłówkami odpowiednie:

    echo 'word_freq_make, word_freq_address, word_freq_all, word_freq_3d,word_freq_our, word_freq_over, word_freq_remove, word_freq_internet,word_freq_order, word_freq_mail, word_freq_receive, word_freq_will,word_freq_people, word_freq_report, word_freq_addresses, word_freq_free,word_freq_business, word_freq_email, word_freq_you, word_freq_credit,word_freq_your, word_freq_font, word_freq_000, word_freq_money,word_freq_hp, word_freq_hpl, word_freq_george, word_freq_650, word_freq_lab,word_freq_labs, word_freq_telnet, word_freq_857, word_freq_data,word_freq_415, word_freq_85, word_freq_technology, word_freq_1999,word_freq_parts, word_freq_pm, word_freq_direct, word_freq_cs, word_freq_meeting,word_freq_original, word_freq_project, word_freq_re, word_freq_edu,word_freq_table, word_freq_conference, char_freq_semicolon, char_freq_leftParen,char_freq_leftBracket, char_freq_exclamation, char_freq_dollar, char_freq_pound, capital_run_length_average,capital_run_length_longest, capital_run_length_total, spam' > headers

Następnie ZŁĄCZ.teksty dwa pliki z poleceniem:

    cat spambase.data >> headers
    mv headers spambaseHeaders.data

Zestaw danych zawiera kilka typów statystyki w każdej wiadomości e-mail: 

- Kolumny, takie jak ***word\_Częst\_WORD*** wskazują wartość procentową wyrazów w wiadomości e-mail, które są zgodne z *programu WORD*. Na przykład jeśli *program word\_Częst\_wprowadź* ma wartość 1, a następnie 1% wszystkie wyrazy w wiadomości e-mail były, *Upewnij*. 
- Kolumny, takie jak ***znak\_Częst\_znak*** wskazują wartość procentową wszystkie znaki w wiadomości e-mail, które były *znak*. 
- ***kapitału\_uruchamianie\_długość\_najdłuższej*** jest długością najdłuższej sekwencji wielkimi literami. 
- ***kapitału\_uruchamianie\_długość\_średnia*** jest średnia długość wszystkich sekwencji wielkimi literami. 
- ***kapitału\_uruchamianie\_długość\_sumy*** jest całkowitą długość wszystkich sekwencji wielkimi literami. 
- ***wiadomości-śmieci*** wskazuje, czy wiadomość e-mail została uznana za spam, czy nie (1 = wiadomości-śmieci, 0 = nie spam).


## <a name="explore-the-dataset-with-microsoft-r-open"></a>Eksplorowanie zestawu danych przy użyciu programu Microsoft R Otwórz

Załóżmy sprawdzić dane i wykonaj kilka podstawowych maszynowego uczenia z R. Maszyn wirtualnych nauki danych zawiera [Microsoft R Otwórz](https://mran.revolutionanalytics.com/open/) preinstalowane. Biblioteki obliczenia wielowątkowe w tej wersji programu R oferuje lepszą wydajność niż różne wersje jednego z wątkami. Otwieranie R Microsoft udostępnia odtwarzalności przy użyciu migawki repozytorium pakietu CRAN.

Aby uzyskać kopię przykłady kodu używane w tym instruktażu, klonowanie repozytorium **Azure komputera — szkoleniowe — danych — naukowej** przy użyciu cyfra, która jest zainstalowany na maszyn wirtualnych. Uruchom z poziomu wiersza polecenia cyfra:

    git clone https://github.com/Azure/Azure-MachineLearning-DataScience.git

Otwórz okno końcowych i rozpocznij nową sesję R przy użyciu konsoli interakcyjnych R.

>[AZURE.NOTE] Za pomocą RStudio dla następujących procedur. Aby zainstalować RStudio, wykonanie tego polecenia na terminal:`./Desktop/DSVM\ tools/installRStudio.sh`

Aby zaimportować dane i skonfigurować środowisko, uruchom polecenie:

    data <- read.csv("spambaseHeaders.data")
    set.seed(123)

Aby wyświetlić statystykę sumaryczną każdej kolumny:

    summary(data)

Aby uzyskać inny widok danych:

    str(data)

Spowoduje to wyświetlenie możesz typ każdej zmiennej i pierwszy kilku wartości w zestawie danych. 

Kolumny *wiadomości-śmieci* odczytano jako liczbę całkowitą, ale jest faktycznie kategorii zmiennej (lub czynniki). Aby ustawić jego typ:

    data$spam <- as.factor(data$spam)

Aby wykonać kilka badawczych analizy, użyj pakietu [ggplot2](http://ggplot2.org/) popularne graficznych biblioteki R, który jest już zainstalowany na maszyn wirtualnych. Uwaga danych podsumowania wcześniej, wyświetlaną mamy podsumowanie statystyki na częstotliwość znak wykrzyknika. Załóżmy kreślenia tych częstotliwości tutaj z następujących poleceń:

    library(ggplot2)
    ggplot(data) + geom_histogram(aes(x=char_freq_exclamation), binwidth=0.25)

Ponieważ zero pasek jest pochylanie powierzchni, temu go:

    email_with_exclamation = data[data$char_freq_exclamation > 0, ]
    ggplot(email_with_exclamation) + geom_histogram(aes(x=char_freq_exclamation), binwidth=0.25)

Istnieje — uproszczony gęstości powyżej 1, która wygląda interesujące. Przyjrzyjmy się tylko tych danych:

    ggplot(data[data$char_freq_exclamation > 1, ]) + geom_histogram(aes(x=char_freq_exclamation), binwidth=0.25)

Następnie podziel go przez szynka w porównaniu z wiadomości-śmieci:

    ggplot(data[data$char_freq_exclamation > 1, ], aes(x=char_freq_exclamation)) +
    geom_density(lty=3) +
    geom_density(aes(fill=spam, colour=spam), alpha=0.55) +
    xlab("spam") +
    ggtitle("Distribution of spam \nby frequency of !") +
    labs(fill="spam", y="Density")

W tych przykładach należy umożliwiają tworzenie wykresów podobnych innych kolumn Eksplorowanie danych zawartych w nich.


## <a name="train-and-test-an-ml-model"></a>Szkolenie i przetestować ML model

Teraz Przyjrzyjmy przeszkolenie kilka modeli nauki maszynowego do klasyfikowania wiadomości e-mail w zestawie danych jako zawierające zakres lub szynka. Firma Microsoft przeszkolenie modelu drzewa decyzji i model las przekształcania przypadkowych pomysłów w tej sekcji, a następnie przetestuj ich dokładność ich przewidywań. 

>[AZURE.NOTE] Pakiet rpart (podziału cykliczne i drzew regresji) używane w poniższy kod jest już zainstalowany na maszyn wirtualnych nauki danych.


Na początek Przyjrzyjmy podzielić zestawu danych zestawy badania i szkolenia:

    rnd <- runif(dim(data)[1])
    trainSet = subset(data, rnd <= 0.7)
    testSet = subset(data, rnd > 0.7)

A następnie tworzenie drzewa decyzyjnego do klasyfikowania tej wiadomości e-mail.

    require(rpart)
    model.rpart <- rpart(spam ~ ., method = "class", data = trainSet)
    plot(model.rpart)
    text(model.rpart)

Wynik jest następujący:

![1](./media/machine-learning-data-science-linux-dsvm-walkthrough/decision-tree.png)

Aby określić, jak wykonuje na stronie Ustaw szkolenia, należy użyć następującego kodu:

    trainSetPred <- predict(model.rpart, newdata = trainSet, type = "class")
    t <- table(`Actual Class` = trainSet$spam, `Predicted Class` = trainSetPred)
    accuracy <- sum(diag(t))/sum(t)
    accuracy

Aby określić, jak również wykonuje na stronie Ustaw test:

    testSetPred <- predict(model.rpart, newdata = testSet, type = "class")
    t <- table(`Actual Class` = testSet$spam, `Predicted Class` = testSetPred)
    accuracy <- sum(diag(t))/sum(t)
    accuracy

Spróbuj również modelu las losowe. Przypadkowe lasów przeszkolenie wiele algorytmów i wyjściowych klasy, która jest tryb klasyfikacji ze wszystkich drzew poszczególnych decyzji. Stanowią bardziej zaawansowanych maszynowego uczenia podejście, zgodnie z ich naprawianie dla tendencji modelu drzewa decyzji do overfit szkolenie zestawu danych. 

    require(randomForest)
    trainVars <- setdiff(colnames(data), 'spam')
    model.rf <- randomForest(x=trainSet[, trainVars], y=trainSet$spam)

    trainSetPred <- predict(model.rf, newdata = trainSet[, trainVars], type = "class")
    table(`Actual Class` = trainSet$spam, `Predicted Class` = trainSetPred)

    testSetPred <- predict(model.rf, newdata = testSet[, trainVars], type = "class")
    t <- table(`Actual Class` = testSet$spam, `Predicted Class` = testSetPred)
    accuracy <- sum(diag(t))/sum(t)
    accuracy


## <a name="deploy-a-model-to-azure-ml"></a>Wdrażanie modelu Azure ML

[Azure maszynowego uczenia Studio](https://studio.azureml.net/) (AzureML) to usługa w chmurze, który ułatwia tworzenie i wdrażanie modeli przewidywanych analizy. Jedna z funkcji i AzureML jest możliwość publikowanie funkcję R jako usługi sieci web. Pakiet AzureML R ułatwia wdrożenie wykonaj bezpośrednio z naszych sesji R na DSVM. 

Aby wdrożyć kod drzewa decyzji z poprzedniej sekcji, musisz zalogować się do Azure maszynowego uczenia Studio. Potrzebujesz Identyfikatora obszaru roboczego i tokenu autoryzacji sigh w. Aby znaleźć te wartości i zainicjować zmiennych AzureML im:

W menu po lewej stronie wybierz pozycję **Ustawienia** . Uwaga swój **Identyfikator obszaru roboczego**. ![2](./media/machine-learning-data-science-linux-dsvm-walkthrough/workspace-id.png)

Z menu ogólnych wybierz **Tokeny autoryzacji** i zanotuj usługi **Tokenu autoryzacji podstawowego**. ![3](./media/machine-learning-data-science-linux-dsvm-walkthrough/workspace-token.png)

Ładowanie pakietu **AzureML** , a następnie ustaw wartości zmiennych przy użyciu swojego Identyfikatora tokenu i obszarów roboczych w sesji R na DSVM:


    require(AzureML)
    wsAuth = "<authorization-token>"
    wsID = "<workspace-id>"


Załóżmy uprościć model ułatwia wdrożenie ta. Wybierz trzy zmienne w najbliższym w katalogu głównym drzewa decyzji i tworzenie nowego drzewa za pomocą tylko trzy zmienne:

    colNames <- c("char_freq_dollar", "word_freq_remove", "word_freq_hp", "spam")
    smallTrainSet <- trainSet[, colNames]
    smallTestSet <- testSet[, colNames]
    model.rpart <- rpart(spam ~ ., method = "class", data = smallTrainSet)

Potrzebna funkcja przewidywania, która przyjmuje funkcji jako dane wejściowe i zwraca przewidywane wartości:

    predictSpam <- function(char_freq_dollar, word_freq_remove, word_freq_hp) {
        predictDF <- predict(model.rpart, data.frame("char_freq_dollar" = char_freq_dollar,
        "word_freq_remove" = word_freq_remove, "word_freq_hp" = word_freq_hp))
        return(colnames(predictDF)[apply(predictDF, 1, which.max)])
    }

Publikowanie funkcji predictSpam AzureML przy użyciu funkcji **publishWebService** : 

    spamWebService <- publishWebService("predictSpam",
        "spamWebService",
        list("char_freq_dollar"="float", "word_freq_remove"="float","word_freq_hp"="float"),
        list("spam"="int"),
        wsID, wsAuth)

Ta funkcja zajmie funkcji **predictSpam** , tworzy usługi sieci web o nazwie **spamWebService** z zdefiniowane dane wejściowe i wyjściowe, a następnie zwraca informacje o nowy punkt końcowy.

Wyświetlanie szczegółów usługi sieci web opublikowanych, łącznie z punktu końcowego interfejsu API i uzyskać dostęp do kluczy przy użyciu polecenia:

    spamWebService[[2]]

Aby wypróbować w pierwszym ustawić 10 wierszy test:

    consumeDataframe(spamWebService$endpoints[[1]]$PrimaryKey, spamWebService$endpoints[[1]]$ApiLocation, smallTestSet[1:10, 1:3])


## <a name="use-other-tools-available"></a>Korzystanie z innych narzędzi, które są dostępne

Pozostałych sekcjach przedstawiono sposoby używania narzędzi zainstalowanym maszyn wirtualnych nauki danych Linux. Poniżej przedstawiono listę narzędzia omówiony:

- XGBoost
- Python
- Jupyterhub
- Rattle
- PostgreSQL & Squirrel SQL
- Program SQL Server Data Warehouse


## <a name="xgboost"></a>XGBoost

[XGBoost](https://xgboost.readthedocs.org/en/latest/) to narzędzie oferuje implementacji szybkie i precyzyjne zwiększane drzewa.

    require(xgboost)
    data <- read.csv("spambaseHeaders.data")
    set.seed(123)

    rnd <- runif(dim(data)[1])
    trainSet = subset(data, rnd <= 0.7)
    testSet = subset(data, rnd > 0.7)

    bst <- xgboost(data = data.matrix(trainSet[,0:57]), label = trainSet$spam, nthread = 2, nrounds = 2, objective = "binary:logistic")

    pred <- predict(bst, data.matrix(testSet[, 0:57]))
    accuracy <- 1.0 - mean(as.numeric(pred > 0.5) != testSet$spam)
    print(paste("test accuracy = ", accuracy))

XGBoost można również nawiązać połączenie python lub wiersza polecenia.

## <a name="python"></a>Python

Rozwoju przy użyciu Python dystrybucji Anaconda Python 2.7 i 3.5 zostały zainstalowane w DSVM. 

>[AZURE.NOTE] Rozkład Anaconda zawiera [Condas](http://conda.pydata.org/docs/index.html), które mogą być używane do tworzenia niestandardowych środowiskach Python, której mają różne wersje i/lub zainstalowanych w nich.

Załóżmy czytanie w niektórych spambase zestawu danych i klasyfikować wiadomości maszyn wektorowa pomocy technicznej w scikit — informacje:

    import pandas
    from sklearn import svm    
    data = pandas.read_csv("spambaseHeaders.data", sep = ',\s*')
    X = data.ix[:, 0:57]
    y = data.ix[:, 57]
    clf = svm.SVC()
    clf.fit(X, y)

Aby wprowadzić przewidywań:

    clf.predict(X.ix[0:20, :])

Aby wyświetlić jak publikować punktu końcowego AzureML, można wprowadzić modelu prostsze trzy zmienne tak, jak firma Microsoft po opublikowaniu możemy modelu R wcześniej. 

    X = data.ix[["char_freq_dollar", "word_freq_remove", "word_freq_hp"]]
    y = data.ix[:, 57]
    clf = svm.SVC()
    clf.fit(X, y)

Publikowanie modelu AzureML:

    # Publish the model.
    workspace_id = "<workspace-id>"
    workspace_token = "<workspace-token>"
    from azureml import services
    @services.publish(workspace_id, workspace_token)
    @services.types(char_freq_dollar = float, word_freq_remove = float, word_freq_hp = float)
    @services.returns(int) # 0 or 1
    def predictSpam(char_freq_dollar, word_freq_remove, word_freq_hp):
        inputArray = [char_freq_dollar, word_freq_remove, word_freq_hp]
        return clf.predict(inputArray)

    # Get some info about the resulting model.
    predictSpam.service.url
    predictSpam.service.api_key

    # Call the model
    predictSpam.service(1, 1, 1)

>[AZURE.NOTE] To jest dostępne tylko dla python 2.7 i nie są jeszcze obsługiwane na 3.5. Uruchom z **/anaconda/bin/python2.7**.


## <a name="jupyterhub"></a>Jupyterhub

Rozkład Anaconda w DSVM pochodzi z danym notesem Jupyter środowisku i platform do udostępnienia kod Python, R lub Julia i analizy. Notes Jupyter jest dostępna za pośrednictwem JupyterHub. Zaloguj się przy użyciu lokalnego Linux oraz nazwę użytkownika i hasło na ***https://\<DNS maszyn wirtualnych nazwę lub adres IP\>: 8000-***. Wszystkie pliki konfiguracji JupyterHub znajdują się w katalogu **/etc/jupyterhub**.

Kilka przykładowych notesów są już zainstalowane na maszyn wirtualnych:

- Zobacz [IntroToJupyterPython.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IntroToJupyterPython.ipynb) notesu Python próbki.
- Zobacz [IntroTutorialinR](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IntroTutorialinR.ipynb) notesu **R** próbki.
- Zobacz [IrisClassifierPyMLWebService](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IrisClassifierPyMLWebService.ipynb) do innego notesu **Python** próbki.

>[AZURE.NOTE] Język Julia jest również dostępne w wierszu polecenia na maszyn wirtualnych nauki danych Linux.


## <a name="rattle"></a>Rattle

[Rattle](https://cran.r-project.org/web/packages/rattle/index.html) (R analitycznych narzędzie do informacje łatwo) to graficzne narzędzie R do wyszukiwania danych. Ma intuicyjny interfejs, który ułatwia załadować, eksplorowanie i przekształcania danych i tworzenia i oceny modeli.  Artykuł [Rattle: Graficznym wyszukiwania danych dla R](https://journal.r-project.org/archive/2009-2/RJournal_2009-2_Williams.pdf) zawiera instrukcje demonstrujący jego funkcji.

Zainstaluj i uruchom Rattle z następujących poleceń:

    if(!require("rattle")) install.packages("rattle")
    require(rattle)
    rattle()

>[AZURE.NOTE] Instalacja nie jest wymagana w DSVM. Ale Rattle może monit o zainstalowanie dodatkowe pakiety podczas jej ładowania.

Rattle używa interfejsu oparte na kartę. Większość kart odpowiadają kroków [Procesu nauki danych](https://azure.microsoft.com/documentation/learning-paths/data-science-process/), takich jak ładowanie danych lub jej eksplorację. Proces nauki danych pisany od lewej do prawej między kartami. Ale karty ostatniego zawiera dziennik poleceń R, uruchamiając Rattle. 


Ładowanie i skonfiguruj zestaw danych:

- Aby załadować plik, wybierz kartę **dane** , a następnie 
- Wybierz selektor obok **nazwy pliku** , a następnie wybierz pozycję **spambaseHeaders.data**. 
- Aby załadować plik. Wybierz pozycję **Wykonywanie** w górnym wierszu przycisków. Powinien zostać wyświetlony Podsumowanie kolumn, także jego typ danych określone dane wejściowe, docelowej lub inny rodzaj zmiennej i liczba unikatowych wartości.
- Rattle poprawnie zidentyfikował kolumny **wiadomości-śmieci** jako miejsce docelowe. Zaznacz kolumnę wiadomości-śmieci, a następnie ustaw **Docelowy typ danych** **Categoric**.

Eksplorowanie danych: 

- Wybierz kartę **Eksploruj** . 
- Kliknij pozycję **podsumowania**, a następnie **Wykonaj**pewne informacje na temat typów zmiennych i niektórych statystyk podsumowania. 
- Aby wyświetlić inne rodzaje statystyk dotyczących każdej zmiennej, wybierz inne opcje, takie jak **Opis** lub **podstawy**.

Na karcie **Eksploruj** umożliwia generowanie wielu powierzchni wnikliwi uczestnicy. Aby Wykreśl histogram danych:


- Zaznacz **podział**.
- Sprawdź **Histogram** **word_freq_remove** i **word_freq_you**.
- Wybierz pozycję **Wykonywanie**. Powinien zostać wyświetlony obu powierzchni gęstości w oknie pojedynczy wykres, jeżeli jest jasne, że wyraz "użytkownik" pojawia się częściej w wiadomościach e-mail niż "Usuwanie".

Interesujące są krzywe korelacji. Aby utworzyć:


- Następnie wybierz **korelacji** jako **Typ** 
- Wybierz pozycję **Wykonywanie**. 
- Rattle ostrzega, że zaleca maksymalnie 40 zmiennych. Wybierz pozycję **Tak,** Aby wyświetlić powierzchni. 

Istnieje kilka interesujące korelacji, które pojawiły się: "technologie" jest ściśle związana "HP" i "labs", na przykład. Jest ona również ściśle związana do "650", ponieważ numer kierunkowy dawców zestawu danych jest 650.

Wartości liczbowe dla korelacji między wyrazami są dostępne w oknie Eksploruj. Jest interesujące notatkę, na przykład, że "technologie" negatywny jest powiązane z "do" i "pieniądze".

Rattle można przekształcać zestawu danych do obsługi niektórych typowych problemów. Na przykład umożliwia ponowne skalowanie funkcje, przypisują brakujące wartości obsługiwać wartości odstających i Usuń zmienne lub uwagi z brakujących danych. Rattle można określić także reguły skojarzenia między obserwacji i/lub zmiennych. Te karty są poza zakresem dla tego instruktażu wprowadzającym.

Rattle można również przeprowadzić analizę klaster. Załóżmy wykluczyć niektóre funkcje, aby ułatwić czytanie danych wyjściowych. Na karcie **dane** wybierz pozycję **Ignoruj** obok każdej zmienne z wyjątkiem tych dziesięć elementów:

- word_freq_hp
- word_freq_technology
- word_freq_george
- word_freq_remove
- word_freq_your
- word_freq_dollar
- word_freq_money
- capital_run_length_longest
- word_freq_business
- wiadomości-śmieci

Następnie przejdź wstecz na kartę **klaster** wybierz **KMeans**i ustaw *liczbę klastrów* do 4. Następnie **Wykonaj**. Wyniki są wyświetlane w oknie dane wyjściowe. Jeden klaster ma wysokiej częstotliwości "Jerzy" i "hp" i prawdopodobnie jest godny zaufania firmową pocztę e-mail.

Aby utworzyć modelu nauki maszyny drzewa decyzji proste: 

- Wybierz kartę **modelu** 
- Jako **Typ**wybierz pozycję **drzewa** . 
- Wybierz pozycję **Wykonywanie** , aby wyświetlić drzewo w postaci tekstu w oknie dane wyjściowe. 
- Wybierz przycisk **Rysuj** , aby wyświetlić graficzną wersję. Wygląd podobna do drzewa, możemy otrzymaną wcześniej przy użyciu programu *rpart*.

Jedna z funkcji i Rattle jest możliwość szybkiego ich oceny i uruchomić kilka metod nauki komputera. Poniżej przedstawiono procedurę:

- **Typ**wybierz pozycję **wszystko** . 
- Wybierz pozycję **Wykonywanie**. 
- Po zakończeniu można kliknąć dowolny pojedynczy **Typ**, takich jak **SVM**i wyświetlić wyniki. 
- Możesz również porównać działanie modeli weryfikacji zestaw przy użyciu karty **Szacowanie** . Na przykład zaznaczenie **Macierzy błędu** pokazano macierz zamieszania, ogólny błąd i uśrednionych klasy błędu dla każdego modelu na stronie Ustaw sprawdzania poprawności. 
- Można również Wykreślone krzywe ROC, wykonywanie analizy wrażliwości i wykonaj inne rodzaje ocen modelu.

Po zakończeniu tworzenia modeli wybierz kartę **dziennika** , aby wyświetlić kod R, uruchamiając Rattle podczas sesji. Można wybierać przycisk **Eksportuj** , aby zapisać go. 

>[AZURE.NOTE] W bieżącej wersji programu Rattle występuje błąd. Aby zmodyfikować skrypt lub umożliwia później Powtórz czynności do wykonania, możesz wstawić znak # przed *Eksportowanie tego dziennika...* w polu Tekst dziennika. 


## <a name="postgresql--squirrel-sql"></a>PostgreSQL & Squirrel SQL

DSVM zawiera PostgreSQL zainstalowany. PostgreSQL jest zaawansowany, Otwórz źródło relacyjnej bazy danych. W tej sekcji przedstawiono sposób ładowania zestawu danych naszych wiadomości-śmieci do PostgreSQL i następnie kwerendy go.

Aby można załadować dane, musisz uwierzytelniania hasła z hosta. W wierszu polecenia:

    sudo gedit /var/lib/pgsql/data/pg_hba.conf

W dolnej części pliku konfiguracji przedstawiono kilka wierszy, które szczegółowo dozwolonych połączeń:

    # "local" is for Unix domain socket connections only
    local   all             all                                     trust
    # IPv4 local connections:
    host    all             all             127.0.0.1/32            ident
    # IPv6 local connections:
    host    all             all             ::1/128                 ident

Zmienianie linii "IPv4 połączenia lokalnego" używać md5 zamiast ident, aby można było Zaloguj się przy użyciu nazwy użytkownika i hasła:

    # IPv4 local connections:
    host    all             all             127.0.0.1/32            md5

I ponownie uruchom usługę postgres:

    sudo systemctl restart postgresql

Aby uruchomić psql, terminal interakcyjnie dla PostgreSQL jako użytkownik postgres wbudowanych, uruchom następujące polecenie w wierszu:

    sudo -u postgres psql

Utwórz nowe konto użytkownika, przy użyciu samej nazwy użytkownika jako konto Linux obecnie jesteś zalogowany jako i nadaj hasła:

    CREATE USER <username> WITH CREATEDB;
    CREATE DATABASE <username>;
    ALTER USER <username> password '<password>';
    \quit

Następnie zaloguj się do psql jako użytkownika:

    psql

I zaimportować dane do nowej bazy danych:

    CREATE DATABASE spam;
    \c spam
    CREATE TABLE data (word_freq_make real, word_freq_address real, word_freq_all real, word_freq_3d real,word_freq_our real, word_freq_over real, word_freq_remove real, word_freq_internet real,word_freq_order real, word_freq_mail real, word_freq_receive real, word_freq_will real,word_freq_people real, word_freq_report real, word_freq_addresses real, word_freq_free real,word_freq_business real, word_freq_email real, word_freq_you real, word_freq_credit real,word_freq_your real, word_freq_font real, word_freq_000 real, word_freq_money real,word_freq_hp real, word_freq_hpl real, word_freq_george real, word_freq_650 real, word_freq_lab real,word_freq_labs real, word_freq_telnet real, word_freq_857 real, word_freq_data real,word_freq_415 real, word_freq_85 real, word_freq_technology real, word_freq_1999 real,word_freq_parts real, word_freq_pm real, word_freq_direct real, word_freq_cs real, word_freq_meeting real,word_freq_original real, word_freq_project real, word_freq_re real, word_freq_edu real,word_freq_table real, word_freq_conference real, char_freq_semicolon real, char_freq_leftParen real,char_freq_leftBracket real, char_freq_exclamation real, char_freq_dollar real, char_freq_pound real, capital_run_length_average real, capital_run_length_longest real, capital_run_length_total real, spam integer);
    \copy data FROM /home/<username>/spambase.data DELIMITER ',' CSV;
    \quit

Teraz Przyjrzyjmy się danych i wykonywania niektórych zapytań za pomocą **Squirrel SQL**, narzędzie graficzne, które umożliwia interakcję z bazy danych za pomocą sterownika JDBC.

Aby rozpocząć pracę, uruchom program Squirrel SQL z menu aplikacji. Aby skonfigurować sterownik:

- Wybierz pozycję **Windows**następnie **sterowniki widoku**. 
- Kliknij prawym przyciskiem myszy **PostgreSQL** i wybierz **Modyfikowanie sterownik**. 
- Wybierz **ścieżkę klasy dodatkowe**, a następnie **Dodaj**. 
- Wprowadź **nazwę pliku** ***/usr/share/java/jdbcdrivers/postgresql-9.4.1208.jre6.jar*** i 
- Wybierz pozycję **Otwórz**.
- Sterowniki listy, a następnie zaznacz **org.postgresql.Driver** w polu **Nazwa klasy**i wybierz przycisk **OK**.

Aby skonfigurować połączenie z lokalnego serwera:
 
- Następnie wybierz pozycję **Windows** **Wyświetlanie aliasów.** 
- Wybierz pozycję **+** przycisk, aby wprowadzić nowy alias. 
- Nadaj mu nazwę *bazy danych wiadomości-śmieci*, wybierz z listy rozwijanej **sterownika** **PostgreSQL** .
- Ustaw adres URL *jdbc:postgresql://localhost/spam*. 
- Wprowadź swoją *nazwę użytkownika* i *hasło*. 
- Kliknij **przycisk OK**. 
- Aby otworzyć okno **połączenie** , kliknij dwukrotnie alias ***spamu bazy danych*** . 
- Zaznacz pole wyboru **Połącz**.

Aby uruchomić niektóre kwerendy:

- Wybierz kartę **SQL** .
- Wprowadź prostej kwerendy, na przykład `SELECT * from data;` w polu tekstowym kwerendy w górnej części na karcie SQL. 
- Naciśnij klawisze **Ctrl Enter** aby go uruchomić. Domyślnie Squirrel SQL zwraca 100 pierwszych wierszy z zapytania. 

Istnieje wiele więcej kwerend, że można uruchomić Eksplorowanie danych. Na przykład jaka jest częstotliwość słowo, *Wprowadź* różnica między wiadomości-śmieci i szynka?

    SELECT avg(word_freq_make), spam from data group by spam;

Lub co to są właściwości wiadomości e-mail zawierające często *3W*?

    SELECT * from data order by word_freq_3d desc;

Większość wiadomości e-mail, które mają wysoki występowania *3-w* są pozornie wiadomości-śmieci, więc może być przydatne do tworzenia modelu przewidywanych do klasyfikowania tej wiadomości e-mail.

Jeśli chcesz wykonać nauki komputera z danymi przechowywanymi w bazie danych PostgreSQL, rozważ użycie [MADlib](http://madlib.incubator.apache.org/).

## <a name="sql-server-data-warehouse"></a>Program SQL Server Data Warehouse

Magazyn danych SQL Azure jest oparte na chmurze, poza skalowanie bazy danych do przetwarzania dużych ilości danych relacyjnych i -relacyjnych. Aby uzyskać więcej informacji, zobacz [Co to jest magazynu danych SQL Azure?](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md)

Aby połączyć do magazynu danych i utworzyć tabelę, uruchom następujące polecenie w wierszu polecenia:

    sqlcmd -S <server-name>.database.windows.net -d <database-name> -U <username> -P <password> -I

Następnie po wyświetleniu monitu sqlcmd:

    CREATE TABLE spam (word_freq_make real, word_freq_address real, word_freq_all real, word_freq_3d real,word_freq_our real, word_freq_over real, word_freq_remove real, word_freq_internet real,word_freq_order real, word_freq_mail real, word_freq_receive real, word_freq_will real,word_freq_people real, word_freq_report real, word_freq_addresses real, word_freq_free real,word_freq_business real, word_freq_email real, word_freq_you real, word_freq_credit real,word_freq_your real, word_freq_font real, word_freq_000 real, word_freq_money real,word_freq_hp real, word_freq_hpl real, word_freq_george real, word_freq_650 real, word_freq_lab real,word_freq_labs real, word_freq_telnet real, word_freq_857 real, word_freq_data real,word_freq_415 real, word_freq_85 real, word_freq_technology real, word_freq_1999 real,word_freq_parts real, word_freq_pm real, word_freq_direct real, word_freq_cs real, word_freq_meeting real,word_freq_original real, word_freq_project real, word_freq_re real, word_freq_edu real,word_freq_table real, word_freq_conference real, char_freq_semicolon real, char_freq_leftParen real,char_freq_leftBracket real, char_freq_exclamation real, char_freq_dollar real, char_freq_pound real, capital_run_length_average real, capital_run_length_longest real, capital_run_length_total real, spam integer) WITH (CLUSTERED COLUMNSTORE INDEX, DISTRIBUTION = ROUND_ROBIN);
    GO

Skopiuj dane z bcp:

    bcp spam in spambaseHeaders.data -q -c -t  ',' -S <server-name>.database.windows.net -d <database-name> -U <username> -P <password> -F 1 -r "\r\n"

>[AZURE.NOTE] Końców w pobrany plik są stylu systemu Windows, ale bcp oczekuje typu UNIX, aby administrator powinien przekazać bcp, który flagą - r.

I kwerenda z sqlcmd:

    select top 10 spam, char_freq_dollar from spam;
    GO

Można również sprawdzać z Squirrel SQL. Wykonaj kroki podobne do PostgreSQL przy użyciu programu Microsoft MSSQL serwera JDBC sterownik, który można znaleźć w ***/usr/share/java/jdbcdrivers/sqljdbc42.jar***.

## <a name="next-steps"></a>Następne kroki

Aby uzyskać omówienie tematy, które znajdziesz szczegółową zadania, które składają się proces nauki danych platformy Azure zobacz [Proces nauki danych zespołu](http://aka.ms/datascienceprocess).

Aby uzyskać opis instruktaży innych end-to-end, który wykazać kroki w procesie nauki zespołu danych dla określonych scenariuszach zobacz [instruktaży proces nauki danych zespołu](data-science-process-walkthroughs.md). Instruktaży również przedstawiają sposób łączenia chmury i lokalnych narzędzia i usługi do przepływu pracy lub procesu, aby utworzyć aplikację inteligentnego.

