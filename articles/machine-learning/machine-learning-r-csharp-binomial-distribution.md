<properties 
    pageTitle="Pakiet rozkład dwumianowy | Microsoft Azure" 
    description="Pakiet rozkład dwumianowy." 
    services="machine-learning" 
    documentationCenter="" 
    authors="ireiter" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/04/2016" 
    ms.author="ireiter"/> 


#<a name="binomial-distribution-suite"></a>Pakiet rozkład dwumianowy.




Pakiet rozkład dwumianowy to zbiór usług sieci web próbki ([Dwumianowego Generator](https://datamarket.azure.com/dataset/aml_labs/bdg5), [Prawdopodobieństwo kalkulatora]( https://datamarket.azure.com/dataset/aml_labs/bdp4) [Kalkulatora kwantyl]( https://datamarket.azure.com/dataset/aml_labs/bdq5)), które pomagają w generowania i zajmujących się rozkład dwumianowy. Usługi pozwalają generowania sekwencji rozkład dwumianowy dowolną długość, obliczania quantiles z danej prawdopodobieństwa i obliczanie prawdopodobieństwa z danym kwantyl. Wszystkich usług emituje różnych wyjściowe według wybranej usługi (zobacz opis poniżej). Pakiet rozkład dwumianowy jest oparty na qbinom funkcje R, rbinom i pbinom, które są zawarte w pakiecie statystykę R. 


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

>Te usługi sieci web może być zużywanej przez użytkowników — potencjalnie bezpośrednio na rynku za pośrednictwem aplikacji dla urządzeń przenośnych, za pośrednictwem witryny sieci Web lub nawet na komputerze lokalnym, na przykład. Ale celem usługi sieci web jest również służyć jako przykład jak Azure maszynowego uczenia umożliwia tworzenie usług sieci web u góry kod R. Z kilkoma wiersze kodu R i kliknięcia przycisku w oknie Azure maszynowego uczenia Studio doświadczenia można tworzyć z kodem R i opublikowany jako usługi sieci web. Następnie można opublikować Azure Marketplace i zużywanej przez użytkowników i urządzeń w dowolnym miejscu na świecie usługi sieci web — ustawień infrastruktury autora usługi sieci web jest wymagane.

##<a name="consumption-of-web-service"></a>Zużycie usługi sieci web
Pakiet rozkład dwumianowy zawiera następujące usługi 3.

###<a name="binomial-distribution-quantile-calculator"></a>Kalkulator kwantyl rozkład dwumianowy.
Ta usługa akceptuje 4 argumenty rozkład normalny i oblicza skojarzone kwantyl.
Argumenty wejściowe są:

- p - pojedynczy zagregowane prawdopodobieństwo wielu prób.  
- rozmiar — liczba prób.
- PRAWDPD - prawdopodobieństwo sukcesu w wersji próbnej.
- Strona - L dla dolnej części rozkład U dla górna rozkładu. 

Dane wyjściowe usługi jest kwantyl obliczeniowych, skojarzonego z danym prawdopodobieństwa.

###<a name="binomial-distribution-probability-calculator"></a>Kalkulator prawdopodobieństwa rozkładu dwumianowego.
Ta usługa akceptuje 4 argumenty rozkład dwumianowy i oblicza prawdopodobieństwo skojarzone.
Argumenty wejściowe są:

- pytania pojedynczy kwantyl wydarzenia przy użyciu rozkład dwumianowy. 
- rozmiar — liczba prób.
- PRAWDPD - prawdopodobieństwo sukcesu w wersji próbnej.
- Strona - L dla dolnej części rozkład U dla górna część dystrybucyjnej lub E, która jest równa się pojedynczego liczba sukcesów.

Dane wyjściowe usługi są obliczeniowe prawdopodobieństwo skojarzone z danym kwantyl.

###<a name="binomial-distribution-generator"></a>Generator rozkład dwumianowy.
Ta usługa akceptuje 3 argumenty rozkład dwumianowy i generuje losową sekwencji liczb binomially dostarczane. Należy podać następujące argumenty do niego w żądanie:

- n - liczbą obserwacji. 
- rozmiar — liczba prób.
- PRAWDPD — prawdopodobieństwo sukcesu.

Dane wyjściowe usługi jest sekwencją n długość z rozkład dwumianowy na podstawie rozmiaru i PRAWDPD argumentów.

>Ta usługa hostowana w Azure Marketplace jest usługą OData; te mogą być nazywane za pomocą metody POST lub GET. 

Istnieje wiele sposobów przez inne usługi w zautomatyzowany sposób (przykład aplikacje są tutaj: [Generator](http://microsoftazuremachinelearning.azurewebsites.net/BinomialDistributionGenerator.aspx), [Prawdopodobieństwo kalkulatora](http://microsoftazuremachinelearning.azurewebsites.net/BinomialDistributionProbabilityCalculator.aspx) [Kalkulatora kwantyl](http://microsoftazuremachinelearning.azurewebsites.net/BinomialDistributionQuantileCalculator)). 

###<a name="starting-c-code-for-web-service-consumption"></a>Uruchamianie kodu C# zużycia usługi sieci web:

###<a name="binomial-distribution-quantile-calculator"></a>Kalkulator kwantyl rozkład dwumianowy.
    public class Input
    {
            public string p;
            public string size;
            public string prob;
            public string side;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void main()
    {
            var input = new Input() { p = TextBox1.Text, size = TextBox2.Text, prob = TextBox3.Text, side = TextBox4.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();
    
            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
    
            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }

###<a name="binomial-distribution-probability-calculator"></a>Kalkulator prawdopodobieństwa rozkładu dwumianowego.
    public class Input
    {
            public string q;
            public string size;
            public string prob;
            public string side;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { q = TextBox1.Text, size = TextBox2.Text, prob = TextBox3.Text, side = TextBox4.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = " PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();
    
            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
    
            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }


###<a name="binomial-distribution-generator"></a>Generator rozkład dwumianowy.
    public class Input
    {
            public string n;
            public string size;
            public string p;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { n = TextBox1.Text, size = TextBox2.Text, p = TextBox3.Text };
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

###<a name="binomial-distribution-quantile-calculator"></a>Kalkulator kwantyl rozkład dwumianowy.

![Tworzenie obszaru roboczego][4]

####<a name="module-1"></a>Moduł 1:
    #data schema with example data (replaced with data from web service)
    data.set=data.frame(p=0.1,size=10,prob=.5,side='L');
    maml.mapOutputPort("data.set"); #send data to output port
####<a name="module-2"></a>Moduł 2:

    dataset1 <- maml.mapInputPort(1) # class: data.frame
    param = dataset1
    if (param$p < 0 ) {
    print('Bad input: p must be between 0 and 1')
    param$p = 0
    } else if (param$p > 1) {
    print('Bad input: p must be between 0 and 1')
    param$p = 1
    }

    if (param$prob < 0 ) {
    print('Bad input: prob must be between 0 and 1')
    param$prob = 0
    } else if (param$prob > 1) {
    print('Bad input: prob must be between 0 and 1')
    param$prob = 1
    }

    quantile = qbinom(param$p,size=param$size,prob=param$prob)
    df = data.frame(x=1:param$size, prob=dbinom(1:param$size, param$size, prob=param$prob))
    quantile

    if (param$side == 'U'){
    quantile = qbinom(param$p,size=param$size,prob=param$prob,lower.tail = F)
    band=subset(df,x>quantile)
    } else if (param$side =='L') {
    quantile = qbinom(param$p,size=param$size,prob=param$prob,lower.tail = T)
    band=subset(df,x<=quantile)
    } else {
    print("Invalid side choice")
    }

    output = as.data.frame(quantile)
    
    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");


###<a name="binomial-distribution-probability-calculator"></a>Kalkulator prawdopodobieństwa rozkładu dwumianowego.

![Tworzenie obszaru roboczego][5]

####<a name="module-1"></a>Moduł 1:

    #data schema with example data (replaced with data from web service)
    data.set=data.frame(q=5,size=10,prob=.5,side='L');
    maml.mapOutputPort("data.set"); #send data to output port


####<a name="module-2"></a>Moduł 2:
    dataset1 <- maml.mapInputPort(1) # class: data.frame
    param = dataset1
    prob = pbinom(param$q,size=param$size,prob=param$prob)
    prob.eq = dbinom(param$q,size=param$size,prob=param$prob)
    df = data.frame(x=1:param$size, prob=dbinom(1:param$size, param$size, prob=param$prob))
    prob

    if (param$side == 'U'){
    prob = 1 - prob
    band=subset(df,x>param$q)
    } else if (param$side =='E') {
    prob = prob.eq
    band=subset(df,x==param$q)
    } else if (param$side =='L') {
    prob = prob
    band=subset(df,x<=param$q)
    } else {
    print("Invalid side choice")
    }

    output = as.data.frame(prob)
    
    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");

###<a name="binomial-distribution-generator"></a>Generator rozkład dwumianowy.

![Tworzenie obszaru roboczego][6]

####<a name="module-1"></a>Moduł 1:

    #data schema with example data (replaced with data from web service)
    data.set=data.frame(n=50,size=10,p=.5);
    maml.mapOutputPort("data.set"); #send data to output port

####<a name="module-2"></a>Moduł 2:
    dataset1 <- maml.mapInputPort(1) # class: data.frame
    param = dataset1
    dist = rbinom(param$n,param$size,param$p)

    output = as.data.frame(t(dist))
    
    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");

##<a name="limitations"></a>Ograniczenia 
Są to bardzo proste przykłady otaczającego rozkład dwumianowy. Jak wynika z przykładowego kodu powyżej, mało Przechwytywanie błąd jest zaimplementowana.

##<a name="faq"></a>FAQ
Często zadawane pytania na temat zużycia usługi sieci web lub publikowania Azure Marketplace, można wyświetlić [w tym miejscu](machine-learning-marketplace-faq.md).


[1]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_1.png

[2]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_2.png

[3]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_3.png

[4]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_4.png

[5]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_5.png

[6]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_6.png
 
