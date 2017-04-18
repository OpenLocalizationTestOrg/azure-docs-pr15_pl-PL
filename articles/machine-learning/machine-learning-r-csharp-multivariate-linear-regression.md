<properties 
    pageTitle="Wielu zmiennych regresji liniowej | Microsoft Azure" 
    description="Wielu zmiennych regresji liniowej." 
    services="machine-learning" 
    documentationCenter="" 
    authors="jaymathe" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/14/2016" 
    ms.author="jaymathe"/> 


#<a name="multivariate-linear-regression"></a>Wielu zmiennych regresji liniowej.   
 

 
Załóżmy, że masz zestawu danych i chcesz szybko przewidywanie zmienną zależną (y) dla każdej osoby (i), na podstawie niezależnej zmiennych. Regresji liniowej jest popularne techniki statystyczne na potrzeby takich przewidywań. W tym miejscu y zmienną zależną zakłada się, że wartość ciągły.  


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]  

Prosty scenariusz może być, gdzie analityka systemów próbuje przewidywanie grubość osoby (y) oparte na ich wysokość (x). Bardziej zaawansowanych scenariuszy może być miejsce, w którym analityka systemów zawiera dodatkowe informacje dotyczące poszczególnych (na przykład grubość, płeć, wyścigowe) i próbuje przewidywanie grubości poszczególnych. Ta [Usługa sieci web]( https://datamarket.azure.com/dataset/aml_labs/multivariate_regression) ogólnej modelu regresji liniowej do danych i wyświetla przewidywane wartości (y) dla każdej z nich obserwacji w danych.

>Ta usługa sieci web może być zużywanej przez użytkowników — potencjalnie za pomocą aplikacji dla urządzeń przenośnych, za pośrednictwem witryny sieci Web lub nawet na komputerze lokalnym, na przykład. Ale celem usługi sieci web jest również służyć jako przykład jak Azure maszynowego uczenia umożliwia tworzenie usług sieci web u góry kod R. Z kilkoma wiersze kodu R i kliknięcia przycisku w oknie Azure maszynowego uczenia Studio doświadczenia można tworzyć z kodem R i opublikowany jako usługi sieci web. Usługa sieci web można następnie opublikowany w Azure Marketplace i zużywanej przez użytkowników i urządzeń w dowolnym miejscu na świecie bez ustawień infrastruktury autora usługi sieci web.  

##<a name="consumption-of-web-service"></a>Zużycie usługi sieci web  
Ta usługa sieci web daje przewidywane wartości na podstawie zmiennych niezależnej dla wszystkich uwag zmienną zależną. Usługa sieci web oczekuje użytkownikowi wprowadzanie danych w postaci ciągu, gdzie wiersze są rozdzielone średnikami (;) i kolumny są rozdzielone średnikami (;). Usługa sieci web oczekuje wiersza 1 w danej chwili i oczekuje pierwszej kolumny jest zmienną zależną. Przykład zestawu danych może wyglądać tak:

![Przykładowe dane][1]

Uwagi bez zmienną zależną należy wprowadzić jako "NA" y. Dane wejściowe dla powyższego zestawu danych będzie następujący ciąg: "10; 5; 2,18; 1; 6,6; 5.3; 2.1,7; 5; 5,22; 3; 4,12; 2; 1, Brak; 3; 4". Wynik to prognozowanej wartości dla poszczególnych wierszy na podstawie zmiennych niezależnej. 

>Ta usługa hostowana w Azure Marketplace jest usługą OData; te mogą być nazywane za pomocą metody POST lub GET. 

Istnieje wiele sposobów przez inne usługi w zautomatyzowany sposób (aplikacja przykład znajduje się [poniżej](http://microsoftazuremachinelearning.azurewebsites.net/MultipleLinearRegressionService.aspx )).

###<a name="starting-c-code-for-web-service-consumption"></a>Uruchamianie kodu C# zużycia usługi sieci web:

    public class Input
    {
            public string value;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { value = TextBox1.Text };
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


Z poziomu Azure maszynowego uczenia, eksperyment nowy pusty utworzono i dwóch [Wykonywanie skryptów R] [ execute-r-script] moduły były pobierane na obszar roboczy. Ta usługa sieci web działa doświadczenia Azure maszynowego uczenia źródłowych scenariusz R. Istnieją 2 części do tego doświadczenia: definicja schematu i szkolenia dotyczące modelu + wyników. Pierwszego modułu definiuje oczekiwane struktury wprowadzania zestawu danych, gdzie pierwsza zmienna jest zmienną zależną i pozostałe zmienne są niezależne. Drugi moduł dopasowuje wzór ogólnego regresji liniowej danych wejściowych.  
  
![Przepływ doświadczenia][3]

####<a name="module-1"></a>Moduł 1:
 
####<a name="schema-definition"></a>Definicja schematu  
    data <- data.frame(value = "1;2;3,4;5;6,7;8;9", stringsAsFactors=FALSE) maml.mapOutputPort("data");  

####<a name="module-2"></a>Moduł 2:
####<a name="lm-modeling"></a>Modelowanie LM   
    data <- maml.mapInputPort(1) # class: data.frame  
  
    data.split <- strsplit(data[1,1], ",")[[1]]  
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE)  
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE)  
    data.split <- as.data.frame(t(data.split)) 
    data.split <- data.matrix(data.split) 
    data.split <- data.frame(data.split) 
    model <- lm(data.split)  

    out=data.frame(predict(model,data.split))  
    out <- data.frame(t(out))

    maml.mapOutputPort("out");  
 
##<a name="limitations"></a>Ograniczenia
To jest bardzo prosty przykład wielu usługi sieci web regresji liniowej. Jak wynika z przykładowego kodu powyżej, nie Przechwytywanie błąd jest zaimplementowana i usługa założono, że wszystko jest zmienną ciągły (nie kategorii funkcji dozwolone), jako usługa tylko wartości wejściowych wartości liczbowych w momencie tworzenia tej usługi sieci web. Ponadto usługi obecnie obsługuje ograniczone danych, rozmiar, ukończenia charakter żądanie/odpowiedź wywołania usługi sieci web i fakt, że model jest nadaje się zawsze, gdy zostanie wywołana usługi sieci web. 

##<a name="faq"></a>FAQ
Często zadawane pytania na temat zużycia usługi sieci web lub publikowania Azure Marketplace, można wyświetlić [w tym miejscu](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-multivariate-linear-regression/multireg-img1.png
[2]: ./media/machine-learning-r-csharp-multivariate-linear-regression/multireg-img2.png
[3]: ./media/machine-learning-r-csharp-multivariate-linear-regression/multireg-img3.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
 
