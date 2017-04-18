<properties 
    pageTitle="Prognozowanie - ETS + STL | Microsoft Azure" 
    description="Prognozowanie - ETS + STL" 
    services="machine-learning" 
    documentationCenter="" 
    authors="xueshanz" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/17/2016" 
    ms.author="yijichen"/> 

#<a name="forecasting---ets--stl"></a>Prognozowanie — ETS + STL  

Ta [Usługa sieci web]( https://datamarket.azure.com/dataset/aml_labs/demand_forecast) zawiera modeli sezonowy trendu dekompozycji (STL) i wygładzania wykładniczego (ETS) da przewidywań na podstawie danych historycznych przez użytkownika. Żądanie dotyczące określonego produktu zwiększa w tym roku? Można I przewidywanie Moje sprzedaży produktu dla świąteczne bożonarodzeniowe tak, aby I efektywnego zaplanowania stan magazynu? Modele prognozowania są w stanie adres takie kwestie. Mieć poprzednie dane, tych modeli sprawdzić, czy ukryte trendów i sezonowości przewidywanie przyszłych trendów. 


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)] 
 
>Ta usługa sieci web może być zużywanej przez użytkowników — potencjalnie za pomocą aplikacji dla urządzeń przenośnych, za pośrednictwem witryny sieci Web lub nawet na komputerze lokalnym, na przykład. Ale celem usługi sieci web jest również służyć jako przykład jak Azure maszynowego uczenia umożliwia tworzenie usług sieci web u góry kod R. Z kilkoma wiersze kodu R i kliknięcia przycisku w oknie Azure maszynowego uczenia Studio doświadczenia można tworzyć z kodem R i opublikowany jako usługi sieci web. Usługa sieci web można następnie opublikowany w Azure Marketplace i zużywanej przez użytkowników i urządzeń w dowolnym miejscu na świecie bez ustawień infrastruktury autora usługi sieci web.  
 
##<a name="consumption-of-web-service"></a>Zużycie usługi sieci web 

Ta usługa akceptuje argumenty 4 i oblicza prognozy.
Argumenty wejściowe są:

* Częstotliwość - wskazuje częstotliwość nieprzetworzonych danych (codziennie-co tydzień/co miesiąc/kwartalny i lata).
* Horyzont - przyszłe prognozy przedziału czasu.
* Data — Dodaj nowe dane serii czasu raz.
* Wartość — dodatek nowe wartości czasu serii danych.

Dane wyjściowe usługi są obliczone wartości prognozy.
 
Przykładowe dane wejściowe może być: 

* Częstotliwość – 12
* Horyzont – 12
* Data — 2012-1-15 2012-2-15 2012-3-15; 2012-4-15; 2012-5-15; 2012-6-15; 2012-15-7; 8- 2012-15 2012-9-15 2012-10-15; 2012-11-15; 2012-12-15; 2013-1-15 2013-2-15 2013-3-15; 2013-4-15; 2013-5-15; 2013-6-15; 2013-15-7; 8- 15-2013 2013-9-15 2013-10-15; 2013-11-15; 2013-12-15; 2014-1-15 2014-2-15 2014-3-15; 2014-4-15; 2014-5-15; 2014-6-15; 2014-15-7; 8- 2014-15; 2014-9-15
* Wartość — 3.479; 3.68; 3.832; 3.941; 3.797; 3.586; 3.508; 3.731; 3.915; 3.844; 3.634; 3.549; 3.557; 3.785; 3.782; 3.601; 3.544; 3.556; 3.65; 3.709; 3.682; 3.511; 3.429 3.51 3.523; 3.525; 3.626; 3.695; 3.711; 3.711; 3.693; 3.571; 3.509

>Ta usługa hostowana w Azure Marketplace jest usługą OData; te mogą być nazywane za pomocą metody POST lub GET. 

Istnieje wiele sposobów przez inne usługi w zautomatyzowany sposób (aplikacja przykład znajduje się [poniżej](http://microsoftazuremachinelearning.azurewebsites.net/StlEtsForecasting.aspx )).

###<a name="starting-c-code-for-web-service-consumption"></a>Uruchamianie kodu C# zużycia usługi sieci web:

    public class Input
    {
            public string frequency;
            public string horizon;
            public string date;
            public string value;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { frequency = TextBox1.Text, horizon = TextBox2.Text, date = TextBox3.Text, value = TextBox4.Text };         var json = JsonConvert.SerializeObject(input);
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

Z poziomu Azure maszynowego uczenia, eksperyment nowy pusty został utworzony. Przykładowych danych wejściowych przekazano ze schematem wstępnie zdefiniowanych danych. Połączone z schemat danych jest [Wykonanie skryptu R] [ execute-r-script] modułu, który generuje STL i ETS prognozowania modeli za pomocą "stl", "ets" i "prognozy" funkcje z R. 

###<a name="experiment-flow"></a>Przepływ doświadczenia:

![Przepływ doświadczenia][2]

####<a name="module-1"></a>Moduł 1:
 
    # Add in the CSV file with the data in the format shown below 
![Przykładowe dane][3]   

####<a name="module-2"></a>Moduł 2:

    # Data input
    data <- maml.mapInputPort(1) # class: data.frame
    library(forecast)
    
    # Preprocessing
    colnames(data) <- c("frequency", "horizon", "dates", "values")
    dates <- strsplit(data$dates, ";")[[1]]
    values <- strsplit(data$values, ";")[[1]]
    
    dates <- as.Date(dates, format = '%m/%d/%Y')
    values <- as.numeric(values)
    
    # Fit a time series model
    train_ts<- ts(values, frequency=data$frequency)
    fit1 <- stl(train_ts,  s.window="periodic")
    train_model <- forecast(fit1, h = data$horizon, method = 'ets')
    plot(train_model)
    
    # Produce forcasting
    train_pred <- round(train_model$mean,2)
    data.forecast <- as.data.frame(t(train_pred))
    colnames(data.forecast) <- paste("Forecast", 1:data$horizon, sep="")
    
    # Data output
    maml.mapOutputPort("data.forecast");

##<a name="limitations"></a>Ograniczenia 

To jest bardzo prosty przykład ETS + STL prognozowania. Jak wynika z przykładowego kodu powyżej, nie Przechwytywanie błąd jest zaimplementowana, a usługa przyjęto założenie, że wszystkie zmienne są wartościami ciągły dodatnie i częstotliwość powinna być liczbą całkowitą większą niż 1. Długość wektorów wartość daty i powinna być taka sama, a długość szeregu czasowego powinna być większa niż 2 * częstotliwości. Zmienna daty powinny być zgodne z format "mm-dd-yyyy".

##<a name="faq"></a>FAQ
Często zadawane pytania na temat zużycia usługi sieci web lub publikowania Azure Marketplace, można wyświetlić [w tym miejscu](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-retail-demand-forecasting/retail-img1.png
[2]: ./media/machine-learning-r-csharp-retail-demand-forecasting/retail-img2.png
[3]: ./media/machine-learning-r-csharp-retail-demand-forecasting/retail-img3.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
 
