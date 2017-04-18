<properties 
    pageTitle="Analizy o przy użyciu komputera Azure nauki | Microsoft Azure" 
    description="Prawdopodobieństwo wystąpienia zdarzenia analizy o" 
    services="machine-learning" 
    documentationCenter="" 
    authors="zhangya" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/21/2016" 
    ms.author="zhangya"/> 


#<a name="survival-analysis"></a>Analiza o 

W obszarze wiele scenariuszy wynik głównym obszarze oceny jest czas na zdarzenie zainteresowania. Innymi słowy pytanie "gdy to zdarzenie wystąpi?" monit. Jako przykład należy rozważyć, czy sytuacji, gdy dane w tym artykule opisano czasu, jaki upłynął (dni, lat, przebieg itp.) do zdarzenia zainteresowania (relapse choroby, stopień Praca odebrana, błąd konsoli hamulca) występuje. Każde wystąpienie w danych reprezentuje określonego obiektu (pacjentów, ucznia, samochodami itp.).


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Ta [Usługa sieci web]( https://datamarket.azure.com/dataset/aml_labs/survivalanalysis) zawiera odpowiedzi na pytanie "co to jest prawdopodobieństwa, że zdarzenie odsetek wystąpi przez godzinę n x? obiektu" Dostarcz modelu analizy o tej usługi sieci web umożliwia użytkownikom dostarczać dane do modelu szkolenie i należy je przetestować. Motywu głównego doświadczenia jest modelu czas, jaki upłynął między nimi, aż zdarzenie odsetek. 

>Ta usługa sieci web może być zużywanej przez użytkowników — potencjalnie za pomocą aplikacji dla urządzeń przenośnych, za pośrednictwem witryny sieci Web lub nawet na komputerze lokalnym, na przykład. Ale celem usługi sieci web jest również służyć jako przykład jak Azure maszynowego uczenia umożliwia tworzenie usług sieci web u góry kod R. Z kilkoma wiersze kodu R i kliknięcia przycisku w oknie Azure maszynowego uczenia Studio doświadczenia można tworzyć z kodem R i opublikowany jako usługi sieci web. Usługa sieci web można następnie opublikowany w Azure Marketplace i zużywanej przez użytkowników i urządzeń w dowolnym miejscu na świecie bez ustawień infrastruktury autora usługi sieci web.  

##<a name="consumption-of-web-service"></a>Zużycie usługi sieci web

Schemat wprowadzania danych usługi sieci web jest wyświetlana w poniższej tabeli. Sześć rodzajów informacji potrzebnych jako danych wejściowych: szkolenia danych, testowanie, wymiar czasu zainteresowanie, indeks "time", indeks wymiar "zdarzenia" i typów zmiennych (ciągły lub współczynnik). Dane szkolenia odpowiada się ciągiem znaków, których wiersze są oddzielone przecinkami, a kolumny są oddzielone średnikami. Wiele funkcji danych jest elastyczna. Wszystkie elementy w ciągu wejściowego musi być liczbowe. Dane szkolenia wymiar "time" oznacza, że liczba jednostek czasu (dni, lat, przebieg itp.), jaka upłynęła od punkt początkowy analizy (pacjentów otrzymywania leków traktowania programów, analizy Praca z początkowym uczniów, samochodu rozpoczęciu być prowadzony itd) do zdarzenia zainteresowania (pacjentów powrót do zastosowania leków, uczniów uzyskania kąt Praca samochodu o błąd konsoli hamulca itp.) występuje. Wymiar "zdarzenia" wskazuje, czy zdarzenie odsetek na końcu badania. Wartość "zdarzenia = 1" oznacza, że zdarzenie odsetek w czasie wskazywana przez wymiaru "time"; "zdarzenie = 0" oznacza, że nie ma wystąpiło zdarzenie zainteresowania przez godzinę wskazywana przez wymiar "time".

- trainingdata — ciąg znaków. Wiersze są oddzielone przecinkami, a kolumny są rozdzielone średnikami. Każdy wiersz zawiera wymiar "time", "zdarzenia" wymiaru i zmiennymi predykcyjnymi.
- testingdata — jeden wiersz danych, który zawiera zmiennymi predykcyjnymi określonego obiektu.
- time_of_interest - czas n odsetek.
- index_time - indeksu kolumny wymiaru "time" (począwszy od 1).
- index_event - indeksu kolumny wymiaru "wydarzenia" (począwszy od 1).
- variable_types — ciąg znaków średnikami jako separatorów w nim. 0 reprezentuje ciągły zmienne i 1 reprezentuje zmienne współczynnik.


Wynik to prawdopodobieństwo zdarzenia występujące w określonym czasie. 

>Ta usługa hostowana w Azure Marketplace jest usługą OData; te mogą być nazywane za pomocą metody POST lub GET. 

Istnieje wiele sposobów przez inne usługi w zautomatyzowany sposób (aplikacja przykład znajduje się [poniżej](http://microsoftazuremachinelearning.azurewebsites.net/SurvivalAnalysis.aspx)). 

###<a name="starting-c-code-for-web-service-consumption"></a>Uruchamianie kodu C# zużycia usługi sieci web:
    public class Input
    {
            public string trainingdata;
            public string testingdata;
            public string timeofinterest;
            public string indextime;
            public string indexevent;
            public string variabletypes;
    }

    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { trainingdata = TextBox1.Text, testingdata = TextBox2.Text, timeofinterest = TextBox3.Text, indextime = TextBox4.Text, indexevent = TextBox5.Text, variabletypes = TextBox6.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();
    
            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
    
            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }




Ten test interpretacji wygląda następująco: Przy założeniu celem danych jest modelu czasu, jaki upłynął do powrotu do zastosowania leków dla pacjentów, którzy otrzymali jednego z dwóch traktowania programów. Wynik odczytuje usługi sieci web: pacjentów jest 35 lat, po poprzedniej leku traktowania 2 razy korzystających z programu długo mieszkalnego traktowania i z użyciem zarówno heroiny, jak i kokainy, prawdopodobieństwo powrót do zastosowania leków 95.64% dnia 500.

##<a name="creation-of-web-service"></a>Tworzenie usługi sieci web

>Ta usługa sieci web został utworzony przy użyciu Azure maszynowego uczenia. Aby uzyskać bezpłatną wersję próbną, a także klipy wideo na temat tworzenia doświadczeń i [publikowania usługi sieci web](machine-learning-publish-a-machine-learning-web-service.md)zobacz [azure.com/ml](http://azure.com/ml). Poniżej przedstawiono zrzut ekranu przedstawiający doświadczenia utworzony kod przykład i usługi sieci web dla każdego z modułów w ramach doświadczenia.

Z poziomu Azure maszynowego uczenia, eksperyment nowy pusty utworzono i dwóch [Wykonywanie skryptów R] [ execute-r-script] moduły były pobierane na obszar roboczy. Schemat danych został utworzony za pomocą prostego [Wykonywanie skryptów R][execute-r-script], która określa schematu wprowadzania danych usługi sieci web. Ten moduł następnie jest połączony z drugim [Wykonywanie skryptów R] [ execute-r-script] moduł, który główne pracy. Moduł ten oznacza wstępne przetwarzanie danych, tworzenie modelu i prognozowanie. W kroku wstępnego przetwarzania danych reprezentowane przez ciąg danych wejściowych jest przekształcane i przekonwertowana na ramkę danych. W kroku konstrukcyjnych modelu zewnętrznych pakietu R "survival_2.37 7.zip" pierwszej instalacji przeprowadzania analizy o. Następnie funkcja "coxph" jest wykonywana po serii zadań przetwarzania danych. Szczegóły funkcji "coxph" na potrzeby analizy o mogą być odczytywane z dokumentacji R. W kroku przewidywania testowania wystąpienie jest podawana do modelu przeszkolony z funkcją "surfit", a krzywej o dla tego wystąpienia testowania powstaje jako zmienna "krzywej". Na koniec uzyskuje się prawdopodobieństwo czasu zainteresowania. 

###<a name="experiment-flow"></a>Przepływ doświadczenia:

![Wypróbuj przepływu][1]

####<a name="module-1"></a>Moduł 1:

    #Data schema with example data (replaced with data from web service)
    trainingdata="53;1;29;0;0;3,79;1;34;0;1;2,45;1;27;0;1;1,37;1;24;0;1;1,122;1;30;0;1;1,655;0;41;0;0;1,166;1;30;0;0;3,227;1;29;0;0;3,805;0;30;0;0;1,104;1;24;0;0;1,90;1;32;0;0;1,373;1;26;0;0;1,70;1;36;0;0;1”
    testingdata="35;2;1;1"
    time_of_interest="500"
    index_time="1"
    index_event="2"
    
    # 0 - continuous; 1 -  factor
    variable_types="0;0;1;1"

    sampleInput=data.frame(trainingdata,testingdata,time_of_interest,index_time,index_event,variable_types)

    maml.mapOutputPort("sampleInput"); #send data to output port
    
####<a name="module-2"></a>Moduł 2:

    #Read data from input port
    data <- maml.mapInputPort(1) 
    colnames(data) <- c("trainingdata","testingdata","time_of_interest","index_time","index_event","variable_types")

    # Preprocessing training data
    traindingdata=data$trainingdata
    y=strsplit(as.character(data$trainingdata),",")
    n_row=length(unlist(y))
    z=sapply(unlist(y), strsplit, ";", simplify = TRUE)
    mydata <- data.frame(matrix(unlist(z), nrow=n_row, byrow=T), stringsAsFactors=FALSE)
    n_col=ncol(mydata)

    # Preprocessing testing data
    testingdata=as.character(data$testingdata)
    testingdata=unlist(strsplit(testingdata,";"))

    # Preprocessing other input parameters
    time_of_interest=data$time_of_interest
    time_of_interest=as.numeric(as.character(time_of_interest))
    index_time = data$index_time
    index_event = data$index_event
    variable_types = data$variable_types

    # Necessary R packages
    install.packages("src/packages_survival/survival_2.37-7.zip",lib=".",repos=NULL,verbose=TRUE)
    library(survival)

    # Prepare to build model
    attach(mydata)

    for (i in 1:n_col){ mydata[,i]=as.numeric(mydata[,i])} 
    d_time=paste("X",index_time,sep = "")
    d_event=paste("X",index_event,sep = "")
    v_time_event <- c(d_time,d_event)
    v_predictors = names(mydata)[!(names(mydata) %in% v_time_event)]

    variable_types = unlist(strsplit(as.character(variable_types),";"))

    len = length(v_predictors)
    c="" # Construct the execution string
    for (i in 1:len){
    if(i==len){
    if(variable_types[i]!=0){ c=paste(c, "factor(",v_predictors[i],")",sep="")}
     else{ c=paste(c, v_predictors[i])}
    }else{
    if(variable_types[i]!=0){c=paste(c, "factor(",v_predictors[i],") + ",sep="")}
    else{c=paste(c, v_predictors[i],"+")}
    }
    }
    f=paste("coxph(Surv(",d_time,",",d_event,") ~")
    f=paste(f,c)
    f=paste(f,", data=mydata )")

    # Fit a Cox proportional hazards model and get the predicted survival curve for a testing instance 
    fit=eval(parse(text=f))

    testingdata = as.data.frame(matrix(testingdata, ncol=len,byrow = TRUE),stringsAsFactors=FALSE)
    names(testingdata)=v_predictors
    for (i in 1:len){ testingdata[,i]=as.numeric(testingdata[,i])}

    curve=survfit(fit,testingdata)

    # Based on user input, find the event occurrence probability
    position_closest=which.min(abs(prob_event$time - time_of_interest))

    if(prob_event[position_closest,"time"]==time_of_interest){# exact match
    output=prob_event[position_closest,"prob"]
    }else{# not exact match
    if(time_of_interest>max(prob_event$time)){
    output=prob_event[position_closest,"prob"]
    }else if(time_of_interest<min(prob_event$time)){
    output=prob_event[position_closest,"prob"]
    }else{output=(prob_event[position_closest,"prob"]+prob_event[position_closest+1,"prob"])/2}
    }

    #Pull out results to send to web service
    output=paste(round(100*output, 2), "%") 
    maml.mapOutputPort("output"); #output port




##<a name="limitations"></a>Ograniczenia

Ta usługa sieci web można wykonać tylko wartości liczbowe, jak funkcja zmienne (kolumny). Kolumny "zdarzenia" może potrwać tylko wartość 0 lub 1. Kolumna "time" musi być dodatnią liczbą całkowitą.

##<a name="faq"></a>FAQ
Często zadawane pytania na temat zużycia usługi sieci web lub publikowania Azure Marketplace, można wyświetlić [w tym miejscu](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-survival-analysis/survive_img2.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
 
