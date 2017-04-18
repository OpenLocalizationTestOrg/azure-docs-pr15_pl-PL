<properties 
    pageTitle="Analiza upodobania podstawie słownika | Microsoft Azure" 
    description="Analiza upodobania podstawie słownika" 
    services="machine-learning" 
    documentationCenter="" 
    authors="pengxia" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/16/2016" 
    ms.author="pengxia"/> 



#<a name="lexicon-based-sentiment-analysis"></a>Analiza upodobania podstawie słownika 

Jak można pomiaru opinie użytkowników i postaw kierunku marek lub tematy w sieciami społecznościowymi online, takich jak Facebook wpisów tweety, przeglądy itp.? Analiza upodobania zapewnia metodę do analizy tych kwestii.


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Zazwyczaj są dwie metody analizy upodobania. Jeden używa algorytmu pod nadzorem szkoleń, a drugi mogą być traktowane jako nauki bez kontroli. Algorytm pod nadzorem nauki zazwyczaj tworzy model klasyfikacji na dużych Boże adnotacjami. Dokładność jest przede wszystkim na podstawie jakości adnotacji i zazwyczaj szkolenie zajmie dużo czasu. Oprócz, gdy stosujemy algorytm do innej domeny, wynik jest zazwyczaj nie warto. W porównaniu z nadzorowany nauki, oparte na słownika zastosowania bez kontroli nauki słownika upodobania, które nie wymagają przechowywania Boże dużych danych i szkolenia — występującą całego procesu szybciej. 

Nasze [usługi](https://datamarket.azure.com/dataset/aml_labs/lexicon_based_sentiment_analysis) jest oparty na słownika subiektywnej oceny MPQA (http://mpqa.cs.pitt.edu/lexicons/subj_lexicon/), które jest jednym z najczęściej używanych subiektywnej oceny lexicons. Istnieją 5097 ujemne i 2533 dodatnie wyrazy w MPQA. I wszystkie te wyrazy są przypisane jako silne lub słabych biegunowości. Cały Boże znajduje się w obszarze GNU General Public License. Usługa sieci web można stosować do dowolnego krótkie zdania, takich jak tweety i wpisów w serwisie Facebook. 

>Ta usługa sieci web można na przykład zużyte przez użytkowników — potencjalnie za pomocą aplikacji dla urządzeń przenośnych, za pośrednictwem witryny sieci Web lub nawet na komputerze lokalnym. Ale celem usługi sieci web jest również służyć jako przykład jak Azure maszynowego uczenia umożliwia tworzenie usług sieci web u góry kod R. Z kilkoma wiersze kodu R i kliknięcia przycisku w oknie Azure maszynowego uczenia Studio doświadczenia można tworzyć z kodem R i opublikowany jako usługi sieci web. Usługa sieci web można następnie opublikowany w Azure Marketplace i zużywanej przez użytkowników i urządzeń w dowolnym miejscu na świecie bez ustawień infrastruktury autora usługi sieci web.

##<a name="consumption-of-web-service"></a>Zużycie usługi sieci web

Dane wejściowe może być dowolny tekst, ale usługa sieci web sprawdza się lepiej z krótkie zdania. Wynik jest wartością liczbową od -1 do 1. Każda wartość poniżej 0 oznacza, że upodobania tekstu jest ujemna; Jeśli dodatni niż 0. Wartość bezwzględna liczby wynik oznacza siłę skojarzone upodobania. 

>Ta usługa hostowana w Azure Marketplace jest usługą OData; te mogą być nazywane za pomocą metody POST lub GET. 

Istnieje wiele sposobów przez inne usługi w zautomatyzowany sposób (aplikacja przykład znajduje się [poniżej](http://microsoftazuremachinelearning.azurewebsites.net/)).

###<a name="starting-c-code-for-web-service-consumption"></a>Uruchamianie kodu C# zużycia usługi sieci web:

    public class ScoreResult
    {
            [DataMember]
            public double result
            {
                get;
                set;
            }
    }

    void main()
    {
            using (var wb = new WebClient())
            {
                var acitionUri = new Uri("PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score");
                DataServiceContext ctx = new DataServiceContext(acitionUri);
                var cred = new NetworkCredential("PutEmailAddressHere", "ChangeToAPIKey");
                var cache = new CredentialCache();
    
                cache.Add(acitionUri, "Basic", cred);
                ctx.Credentials = cache;
                var query = ctx.Execute<ScoreResult>(acitionUri, "POST", true, new BodyOperationParameter("Text", TextBox1.Text));
                ScoreResult scoreResult = query.ElementAt(0);
                double result = scoreResult.result;
            }
    }



Dane wejściowe są "Dzisiaj jest dobrym dnia". Wynik jest "1", co oznacza dodatnią upodobania skojarzone z wprowadzania zdanie. 

##<a name="creation-of-web-service"></a>Tworzenie usługi sieci web
>Ta usługa sieci web został utworzony przy użyciu Azure maszynowego uczenia. Aby uzyskać bezpłatną wersję próbną, a także klipy wideo na temat tworzenia doświadczeń i [publikowania usługi sieci web](machine-learning-publish-a-machine-learning-web-service.md)zobacz [azure.com/ml](http://azure.com/ml). Poniżej przedstawiono zrzut ekranu przedstawiający doświadczenia utworzony kod przykład i usługi sieci web dla każdego z modułów w ramach doświadczenia.


Z poziomu Azure maszynowego uczenia, eksperyment nowy pusty został utworzony. Na poniższym rysunku przedstawiono przepływ doświadczenia analizy upodobania oparte na słownika. Plik "sent_dict.csv" jest słownika subiektywnej oceny MPQA i jest ustawiona jako jednego z wejść [Wykonywanie skryptów R][execute-r-script]. Inny dane wejściowe są próbki Recenzja z zestawu danych Recenzja Amazon do badania, miejsce, w którym było wykonywane zaznaczenia, zmiany nazwy kolumny i dzielenie operacji. Firma Microsoft korzysta z pakietu skrótu do przechowywania słownika subiektywnej oceny w pamięci i przyspieszenia procesu wynik obliczeń. Cały tekst zostaną plikach przez pakiet "tm" i w porównaniu z wyrazu w słowniku upodobania. Na koniec wynik będzie obliczana przez dodanie grubości każdego subiektywne wyrazu w tekście. 

###<a name="experiment-flow"></a>Przepływ doświadczenia:

![Wypróbuj przepływu][2]


####<a name="module-1"></a>Moduł 1:
    
    # Map 1-based optional input ports to variables
    sent_dict_data<- maml.mapInputPort(1) # class: data.frame
    dataset2 <- maml.mapInputPort(2) # class: data.frame
 
   # <a name="install-hash-package"></a>Zainstaluj pakiet skrótu
    install.packages("src/hash_2.2.6.zip", lib = ".", repos = NULL, verbose = TRUE)
    success <- library("hash", lib.loc = ".", logical.return = TRUE, verbose = TRUE)
    library(tm)
    library(stringr)

    #create sentiment dictionary
    negation_word <- c("not","nor", "no")
    result <- c()
    sent_dict <- hash()
    sent_dict <- hash(sent_dict_data$word, sent_dict_data$polarity)

    #  Compute sentiment score for each document
    for (m in 1:nrow(dataset2)){
    polarity_ratio <- 0
    polarity_total <- 0
    not <- 0
    sentence <- tolower(dataset2[m,1])
    if (nchar(sentence) > 0){
        token_array <- scan_tokenizer(sentence)
        for (j in 1:length(token_array)){
            word = str_replace_all(token_array[j], "[^[:alnum:]]", "")
            for (k in 1:length(negation_word)){
              if (word == negation_word[k]){
                not <- (not+1) %% 2

              }
            }
            if (word != ""){
                if (!is.null(sent_dict[[word]])){
                  polarity_ratio <- polarity_ratio + (-2*not+1)*sent_dict[[word]]
                  polarity_total <- polarity_total + abs(sent_dict[[word]])
                }
            }
          
        }
    }
    if (polarity_total > 0){
        result <- c(result, polarity_ratio/polarity_total)
    }else{
        result<- c(result,0)
    }
    }

    # Sample operation
    data.set <- data.frame(result)

    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("data.set")
    


##<a name="limitations"></a>Ograniczenia

Z perspektywy algorytm analiza oparte na słownika upodobania jest narzędzia analizy upodobania ogólne, które nie może działać lepiej niż metoda klasyfikacji dla określonych pól. Negacja problem nie jest również zająć. Firma Microsoft hardcode kilka Negacja słowa nasz program, ale lepiej jest przy użyciu słownika Negacja i utworzyć kilka reguł. Usługa sieci web wykonuje lepiej krótka i prosta zdań, takich jak tweety i wpisów w serwisie Facebook, niż na długie i złożonych zdań, takich jak przeglądy Amazon. 

##<a name="faq"></a>FAQ
Często zadawane pytania na temat zużycia usługi sieci web lub publikowania Azure Marketplace, można wyświetlić [w tym miejscu](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-lexicon-based-sentiment-analysis/sentiment_analysis_1.png
[2]: ./media/machine-learning-r-csharp-lexicon-based-sentiment-analysis/sentiment_analysis_2.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/

 
