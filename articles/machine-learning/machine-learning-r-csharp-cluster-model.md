<properties 
    pageTitle="Klaster modelu | Microsoft Azure" 
    description="Klaster modelu" 
    services="machine-learning" 
    documentationCenter="" 
    authors="FrancescaLazzeri" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/17/2016" 
    ms.author="lazzeri"/> 


#<a name="cluster-model"></a>Klaster modelu    

Jak możemy przewidywanie grup zachowania posiadacze kart mający kredytowej w celu zmniejszenia ryzyka opłaty wyłączanie kart kredytowych Można zdefiniować grupy cech osobowość pracowników w celu zwiększenia jego wydajności w pracy? Jak lekarzy klasyfikowania pacjentów w grupach według cech ich chorób Zasadniczo czy wszystkie z tych pytań odpowiedzi za pomocą analizy klaster.   


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)] 
   
Analiza klaster klasyfikuje zbioru obserwacji w dwóch lub więcej wzajemnie wykluczających się nieznany grupy na podstawie kombinacji zmiennych. Celem analizy klaster jest poznawanie systemu organizacji uwagi, zwykle osoby lub ich właściwości, w grupach, miejsce, w którym członkowie grup udostępnianie właściwości wspólne. Ta [Usługa](https://datamarket.azure.com/dataset/aml_labs/k_cluster_model) używa metodologii oznacza K, często używane techniki klastrów, aby klaster dowolne dane w grupach. Ta usługa sieci web pobiera dane i liczba k klastrów jako danych wejściowych i tworzy przewidywań o grupach k, do których należy każdy uwagi. 

>Ta usługa sieci web może być zużywanej przez użytkowników — potencjalnie za pomocą aplikacji dla urządzeń przenośnych, za pośrednictwem witryny sieci Web lub nawet na komputerze lokalnym, na przykład. Ale celem usługi sieci web jest również służyć jako przykład jak Azure maszynowego uczenia umożliwia tworzenie usług sieci web u góry kod R. Z kilkoma wiersze kodu R i kliknięcia przycisku w oknie Azure maszynowego uczenia Studio doświadczenia można tworzyć z kodem R i opublikowany jako usługi sieci web. Usługa sieci web można następnie opublikowany w Azure Marketplace i zużywanej przez użytkowników i urządzeń w dowolnym miejscu na świecie bez ustawień infrastruktury autora usługi sieci web.  

##<a name="consumption-of-web-service"></a>Zużycie usługi sieci web   
Ta usługa sieci web grupowanie danych w z zestawu grup k i wyświetla przypisanie grupy dla każdego wiersza. Usługa sieci web oczekuje użytkownikowi wprowadzanie danych w postaci ciągu, gdzie wiersze są rozdzielone średnikami (;) i kolumny są rozdzielone średnikami (;). Usługa sieci web oczekuje wiersza 1 w danej chwili. Przykład zestawu danych może wyglądać tak:

![Przykładowe dane][1]

Załóżmy, że użytkownik chce oddzielić te dane do 3 wzajemnie wykluczających się grup. Dane wejściowe dla powyższego zestawu danych może być następujące: wartość = "10; 5; 2,18; 1; 6,7; 5; 5,22; 3; 4,12; 2; 1,10; 3; 4"; k = "3". Wynik to członkostwa w grupach przewidywane dla poszczególnych wierszy.

>Ta usługa hostowana w Azure Marketplace jest usługą OData; te mogą być nazywane za pomocą metody POST lub GET. 

Istnieje wiele sposobów przez inne usługi w zautomatyzowany sposób (aplikacja przykład znajduje się [poniżej](http://microsoftazuremachinelearning.azurewebsites.net/ClusterModel.aspx )).

###<a name="starting-c-code-for-web-service-consumption"></a>Uruchamianie kodu C# zużycia usługi sieci web:

    public class Input
    {
            public string value;
            public string k;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { value = TextBox1.Text, k = TextBox2.Text };
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

Z poziomu Azure maszynowego uczenia, eksperyment nowy pusty utworzono i dwóch [Wykonywanie skryptów R] [ execute-r-script] moduły wczytywane do obszaru roboczego. Schemat danych został utworzony za pomocą prostego [Wykonywanie skryptów R][execute-r-script]. Następnie schemat danych został połączony z sekcji modelu klaster ponownie utworzone za pomocą [Wykonywanie skryptów R][execute-r-script]. W obszarze [Wykonywanie skryptów R] [ execute-r-script] używany dla modelu klaster, usługi sieci web następnie używa funkcji "k oznacza", czyli gotowych do [Wykonywanie skryptów R] [ execute-r-script] Azure maszynowego nauczania.    
   

     
![Przepływ doświadczenia][3]

####<a name="module-1"></a>Moduł 1: 
    #Enter the input data as a string 
    mydata <- data.frame(value = "1; 3; 5; 6; 7; 7, 5; 5; 6; 7; 2; 1, 3; 7; 2; 9; 56; 6, 1; 4; 5; 26; 4; 23, 15; 35; 6; 7; 12; 1, 32; 51; 62; 7; 21; 1", k=5, stringsAsFactors=FALSE)
    
    maml.mapOutputPort("mydata");     
    

####<a name="module-2"></a>Moduł 2:
    # Map 1-based optional input ports to variables
    mydata <- maml.mapInputPort(1) # class: data.frame

    data.split <- strsplit(mydata[1,1], ",")[[1]]
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE)
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE)
    data.split <- as.data.frame(t(data.split))

    data.split <- data.matrix(data.split)
    data.split <- data.frame(data.split)

    # K-Means cluster analysis
    fit <- kmeans(data.split, mydata$k) # k-cluster solution

    # Get cluster means 
    aggregate(data.split,by=list(fit$cluster),FUN=mean)
    # Append cluster assignment
    mydatafinal <- data.frame(t(fit$cluster))
    n_col=ncol(mydatafinal)
    colnames(mydatafinal) <- paste("V",1:n_col,sep="")

    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("mydatafinal");
   
 
##<a name="limitations"></a>Ograniczenia
To jest bardzo prosty przykład klastrów usługi sieci web. Jak wynika z przykładowego kodu powyżej, nie Przechwytywanie błąd jest zaimplementowana i usługa założono, że wszystko jest zmienną ciągły (nie kategorii funkcji dozwolone), jako usługa tylko wartości wejściowych wartości liczbowych w momencie tworzenia tej usługi sieci web. Ponadto usługi obecnie obsługuje ograniczone danych, rozmiar, ukończenia charakter żądanie/odpowiedź wywołania usługi sieci web i fakt, że model jest nadaje się zawsze, gdy zostanie wywołana usługi sieci web. 

##<a name="faq"></a>FAQ
Często zadawane pytania na temat zużycia usługi sieci web lub publikowania Azure Marketplace, można wyświetlić [w tym miejscu](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-cluster-model/cluster-img1.png
[2]: ./media/machine-learning-r-csharp-cluster-model/cluster-img2.png
[3]: ./media/machine-learning-r-csharp-cluster-model/cluster-img3.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
 
