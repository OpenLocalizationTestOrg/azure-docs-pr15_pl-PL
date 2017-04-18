<properties 
    pageTitle="Różnica w badaniu proporcje | Microsoft Azure" 
    description="Różnica w badaniu proporcji" 
    services="machine-learning" 
    documentationCenter="" 
    authors="aniedea" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/12/2016" 
    ms.author="aniedea"/> 


#<a name="difference-in-proportions-test"></a>Różnica w badaniu proporcji


Różnią się dwóch proporcji statystyczne? Załóżmy, że użytkownik chce, aby porównać dwie filmy do określenia, czy znacznie większą część jednej filmu "znaczników Lubię to" po porównaniu ich z drugiej. Z próbki duży może być statystyczne znacząca różnica między proporcje 0,50 i 0,51. Z małych próbki może być za mało danych, aby sprawdzić, czy te proporcje są faktyczną różnych. 


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Ta [Usługa sieci web]( https://datamarket.azure.com/dataset/aml_labs/prop_test) prowadzi hipotetyczny test różnicy dwóch proporcji oparte na danych wprowadzonych przez użytkownika liczba sukcesów i całkowita liczba prób dla grup 2 porównania. W jednym z możliwych scenariuszy ta usługa sieci web może być wywołana z poziomu aplikacji porównanie filmu, informujący użytkownika, czy jeden z filmów jest naprawdę "łączone" częściej niż inne, według oceny filmu.

>Ta usługa sieci web może być zużywanej przez użytkowników — potencjalnie za pomocą aplikacji dla urządzeń przenośnych, za pośrednictwem witryny sieci Web lub nawet na komputerze lokalnym, na przykład. Ale celem usługi sieci web jest również służyć jako przykład jak Azure maszynowego uczenia umożliwia tworzenie usług sieci web u góry kod R. Z kilkoma wiersze kodu R i kliknięcia przycisku w oknie Azure maszynowego uczenia Studio doświadczenia można tworzyć z kodem R i opublikowany jako usługi sieci web. Usługa sieci web można następnie opublikowany w Azure Marketplace i zużywanej przez użytkowników i urządzeń w dowolnym miejscu na świecie bez ustawień infrastruktury autora usługi sieci web.


##<a name="consumption-of-web-service"></a>Zużycie usługi sieci web

Ta usługa akceptuje argumenty 4 i czy hipotezy testowanie proporcji.

Argumenty wejściowe są:

* Successes1 — liczba zdarzeń sukcesów w próbce 1.
* Successes2 — liczba zdarzeń sukcesów w próbce 2.
* Total1 — rozmiar próbki 1.
* Total2 — rozmiar próbki 2.

Dane wyjściowe usługi jest wynikiem hipotezę testowanie wraz z statystyczny, df\ssreg, chi-square w wartość p, a udział procentowy w przykładowe 1/2 a ufności granicami.

>Ta usługa hostowana w Azure Marketplace jest usługą OData; te mogą być nazywane za pomocą metody POST lub GET. 

Istnieje wiele sposobów przez inne usługi w zautomatyzowany sposób (aplikacja przykład znajduje się [poniżej](http://microsoftazuremachinelearning.azurewebsites.net/DifferenceInProportionsTest.aspx )).

###<a name="starting-c-code-for-web-service-consumption"></a>Uruchamianie kodu C# zużycia usługi sieci web:

    public class Input
    {
            public string successes1;
            public string successes2;
            public string total1;
            public string total2;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }

    void Main()
    {
            var input = new Input() { successes1 = TextBox1.Text, successes2 = TextBox2.Text, total1 = TextBox3.Text, total2 = TextBox4.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();
    
            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
    
            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }


##<a name="creation-of-web-service"></a>Tworzenie usługi sieci web

>Ta usługa sieci web został utworzony przy użyciu Azure maszynowego uczenia. Aby uzyskać bezpłatną wersję próbną, a także klipy wideo na temat tworzenia doświadczeń i [publikowania usługi sieci web](machine-learning-publish-a-machine-learning-web-service.md)zobacz [azure.com/ml](http://azure.com/ml). Poniżej przedstawiono zrzut ekranu przedstawiający doświadczenia utworzony kod przykład i usługi sieci web dla każdego z modułów w ramach doświadczenia.

Z poziomu Azure maszynowego uczenia, eksperyment nowy pusty utworzono dwa [Wykonywanie skryptów R] [ execute-r-script] moduły. W module pierwszego zdefiniowanych schemat danych a drugi moduł używa prop.test polecenia w R w celu wykonywania hipotetyczny test dla proporcje 2. 


###<a name="experiment-flow"></a>Przepływ doświadczenia:

![Przepływ doświadczenia][2]


####<a name="module-1"></a>Moduł 1:
    ####Schema definition  
    data.set=data.frame(successes1=50,successes2=60,total1=100,total2=100);
    maml.mapOutputPort("data.set"); #send data to output port
    dataset1 = maml.mapInputPort(1) #read data from input port
    

####<a name="module-2"></a>Moduł 2:

    test=prop.test(c(dataset1$successes1[1],dataset1$successes2[1]),c(dataset1$total1[1],dataset1$total2[1])) #conduct hypothesis test

    result=data.frame(
    result=ifelse(test$p.value<0.05,"The proportions are different!",
    "The proportions aren't different statistically."),
    ChiSquarestatistic=round(test$statistic,2),df=test$parameter,
    pvalue=round(test$p.value,4),
    proportion1=round(test$estimate[1],4),
    proportion2=round(test$estimate[2],4),
    confintlow=round(test$conf.int[1],4),
    confinthigh=round(test$conf.int[2],4)) 

    maml.mapOutputPort("result"); #output port
    

##<a name="limitations"></a>Ograniczenia 

To jest bardzo prosty przykład badania różnicy w proporcji 2. Jak wynika z przykładowego kodu powyżej, nie Przechwytywanie błąd jest zaimplementowana i usługa założono, że wszystkie zmienne jest ciągły.

##<a name="faq"></a>FAQ
Często zadawane pytania na temat zużycia usługi sieci web lub publikowania Azure Marketplace, można wyświetlić [w tym miejscu](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-difference-in-two-proportions/hyptest-img1.png
[2]: ./media/machine-learning-r-csharp-difference-in-two-proportions/hyptest-img2.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
 
