<properties 
    pageTitle="Konfigurowanie i używanie interfejsu API zalecenia nauki komputera | Microsoft Azure" 
    description="Interfejs API zalecenia Microsoft skonstruowane za pomocą Azure maszynowego uczenia — często zadawane pytania" 
    services="machine-learning" 
    documentationCenter="" 
    authors="LuisCabrer" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/08/2016" 
    ms.author="luisca"/> 

#<a name="setting-up-and-using-machine-learning-recommendations-api-faq"></a>Konfigurowanie i używanie komputera nauki zalecenia interfejsu API często zadawane pytania


**Co to jest zalecenia?**

>[AZURE.NOTE]Należy zacząć korzystać z usługi poznawcze interfejsu API zalecenia zamiast tej wersji. Usługa poznawcze zalecenia będzie zamieniania tej usługi, a opracowane zostaną wszystkie nowe funkcje. Ma nowych funkcji, takich jak tworzeniu partii pomocy technicznej lepiej interfejsu API Eksploratora, oczyszczania środowisko tworzenia konta i rozliczenia powierzchniowy, bardziej spójny interfejs API, itp.
> Dowiedz się więcej na temat [migrowania do nowej usługi poznawcze](http://aka.ms/recomigrate)

W przypadku organizacji i firmy korzystające z zalecenia dotyczące sprzedaży i sprzedaż w górę produktów i usług klientom zalecenia Azure maszynowego uczenia udostępnia aparat Samoobsługowe zalecenia. Jest stosowania wspólnej filtrowania, używającej factorization macierzy jako swojego algorytmu core. Deweloperów aplikacji dostępu zalecenia przy użyciu interfejsów API pozostałych. 

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

**Co można zrobić z ZALECENIAMI?**

ZALECENIA przyjmuje jako danych wejściowych element lub zestawu elementów i zwraca listę odpowiednich zaleceń. Na przykład: Odbiorca online sprzedawcy kliknie produktu. Online u sprzedawcy detalicznego wysyła tego produktu jako dane wejściowe do zalecenia, otrzymuje listę produktów w zamian i decyduje, którego z tych produktów będą wyświetlane do klienta. Warto zalecenia za pomocą zoptymalizować magazynie online lub nawet w celu poinformowania do wewnątrz sprzedaży Centrum działu lub połączenia.

**Czy istnieje ograniczenie zastosowania?**

Zalecenia dotyczące występują następujące ograniczenia użycia:
* Maksymalna liczba modeli na subskrypcję: 10
* Maksymalna liczba elementów, które mogą być przechowywane w katalogu: 100 000
* Maksymalna liczba punktów zastosowania, które są przechowywane jest ~ 5,000,000. Najstarszego zostaną usunięte, jeśli nowe zostaną przekazane lub zgłoszone.
* Maksymalny rozmiar danych, które mogą być wysyłane pocztą e-mail (na przykład importowanie wykazu danych, importowanie danych dotyczących użycia) wynosi 200 MB
* Liczba transakcji na sekundę (TPS) dla kompilacji modelu zalecenia, która nie jest aktywna jest TPS ~ 2. Zalecenia kompilacji modelu, który jest aktywny może zawierać maksymalnie 20 TPS.

##<a name="purchase-and-billing"></a>Zakup i rozliczenia 


**Jaki jest koszt zalecenia w okresie uruchamianie?**

Zalecenia dotyczące to usługa subskrypcji. Ładowanie jest oparty na wielkość transakcji na miesiąc. Możesz sprawdzić [page oferty] (https://datamarket.azure.com/dataset/amla/recommendations) w programie Microsoft Azure Marketplace, aby uzyskać informacje o cenach.

**Czy istnieją koszty skojarzone z konieczności zalecenia śledzenie i przechowywanie aktywności użytkownika?**

Nie w momencie.

**Zalecenia dotyczące ma bezpłatna wersja próbna?**

Istnieje dziennik bezpłatne, która jest ograniczona do 10 000 transakcji w miesiącu.

**Kiedy będzie można naliczona opłata za zalecenia?**

Płatnej subskrypcji jest żadnej subskrypcji, dla którego jest opłaty miesięcznej. Po zakupieniu subskrypcji płatnej natychmiast są naliczane do użytku w pierwszym miesiącu. Są naliczane wielkość, który jest skojarzony z oferty na stronę subskrypcji (plus odpowiednich podatków). Miesięczna opłata składa każdego miesiąca na tę samą datę kalendarza jako Zakup oryginalny do momentu anulować subskrypcję. 

**Jak uaktualnić z usługą wyższy poziom**

Możesz kupić lub aktualizować subskrypcji [page oferty] strony (https://datamarket.azure.com/dataset/amla/recommendations) w programie Microsoft Azure Marketplace.

Po uaktualnieniu subskrypcji:

* Transakcje, które pozostają na starej subskrypcji nie są dodawane do nowej subskrypcji. 
* Płatność pełną cenę nowej subskrypcji, mimo że masz nieużywane transakcji starej subskrypcji.

Proces uaktualniania subskrypcji:

* Nevigate ze stroną [oferty] (https://datamarket.azure.com/dataset/amla/recommendations).
* Zaloguj się do witryny Marketplace programu Jeśli jeszcze nie są zalogowane.
* W okienku po prawej stronie znajdują się wszystkie dostępne plany. Kliknij przycisk radiowy planu, który chcesz uaktualnić do.
* Jeśli chcesz uaktualnić, kliknij **przycisk OK**. Jeśli nie chcesz uaktualnić, kliknij przycisk **Anuluj**.

**Ważne** Przed uaktualnieniem, ponieważ nie istnieją rozliczenia i użyj dokładnie zapoznać się z okna dialogowego.

**Kiedy zakończy się subskrypcję zalecenia**

Twoja subskrypcja zakończy się w przypadku anulowania. Jeśli chcesz anulować subskrypcji, zobacz poniższe instrukcje.

**Jak anulować subskrypcję zalecenia?**

Aby anulować subskrypcję, wykonaj następujące czynności. Jeśli bieżącej subskrypcji jest skojarzony z opłaconą subskrypcją, subskrypcji nadal obowiązywać do końca bieżącego okresu rozliczeniowego. W razie potrzeby anulowania się skuteczne od razu, skontaktuj się z nami w [Pomocy technicznej firmy Microsoft](https://support.microsoft.com/oas/default.aspx?gprid=17024&st=1&wfxredirect=1&sd=gn).

**Uwaga** Nie zwrotu znajduje się w przypadku anulowania subskrypcji przed upływem okresu rozliczeniowego lub nieużywane transakcji w okresie rozliczeniowym.

* Przejdź do [page oferty] (https://datamarket.azure.com/dataset/amla/recommendations).
* Zaloguj się do witryny Marketplace programu Jeśli jeszcze nie są zalogowane.
* Kliknij przycisk **Anuluj** po prawej stronie nazwę zestawu danych i stan. Można użyć tej subskrypcji do końca bieżący okres rozliczeniowy lub usługi transakcji limitu (cokolwiek nastąpi najpierw).

Jeśli chcesz anulować subskrypcję natychmiast tak zakupieniu nowej subskrypcji, plików bilet w [Pomocy technicznej firmy Microsoft](https://support.microsoft.com/oas/default.aspx?gprid=17024&st=1&wfxredirect=1&sd=gn).

##<a name="getting-started-with-recommendations"></a>Wprowadzenie do zalecenia

**Zalecenia dotyczące jest dla mnie?** 

Zalecenia maszynowego uczenia dotyczy organizacji i firmy korzystające z zalecenia dotyczące sprzedaży i sprzedaż w górę produktów lub usług klientom. Jeśli masz klienta witrynie sieci Web, działu sprzedaży, wewnętrznej działu sprzedaży lub Centrum połączenia, a jeśli wykaz więcej niż kilku dozen produktów lub usług, linii dolnej mogą korzystać z zalecenia. 

Eksperymentowanie z zalecenia ma być dość proste. Bieżącej wersji opartej na interfejsu API wymaga podstawowych umiejętności programowania. Jeśli potrzebujesz pomocy, skontaktuj się z dostawcą, twórcy witryny sieci Web. Jeśli masz wewnętrzny pracownika działu informatycznego lub lokalnego projektanta, należy je można uzyskać zalecenia do pracy. 

**Jakie są wymagania wstępne dotyczące konfigurowania zalecenia?**

Zalecenia dotyczące wymaga dziennika opcji użytkownika w odniesieniu do wykazu. Jeśli nie t takie Dziennik i masz witrynę sieci Web przeciwległych klienta, zalecenia może zbierać aktywności użytkownika dla Ciebie. 

Zalecenia dotyczące wymaga również z katalogiem produktów i usług. Jeśli nie t jest wykazu — zalecenia możesz korzystać z danych dotyczących użycia konkretnego odbiorcy i przetwarzania wykazu. Wykaz dorozumianych nie będzie zawierać elementy, które nie zostały zgłoszone jako część transakcje użytkownika.

**Jak skonfigurować zalecenia po raz pierwszy**

Po [subskrypcja] (https://datamarket.azure.com/dataset/amla/recommendations) aby zalecenia, należy użyć w dokumentacji interfejsu API [Azure maszynowego uczenia zalecenia Przewodnik Szybki Start:](machine-learning-recommendation-api-quick-start-guide.md) Aby skonfigurować usługę.

**Gdzie można znaleźć dokumentację interfejsu API?** 

Dokumentacja interfejsu API jest [Azure maszynowego uczenia zalecenia Przewodnik Szybki Start](machine-learning-recommendation-api-quick-start-guide.md).

**Jakie opcje mam przekazać wykazu i zastosowania danych do zalecenia?**

Istnieją dwie opcje przekazywania danych wykazu i zastosowania: Możesz wyeksportować dane z programu CRM lub innych dzienników i przekaż go do zalecenia lub znaczniki można dodać do witryny sieci Web, które będzie śledzone aktywności użytkownika. Jeśli używasz druga metoda, dane będą przechowywane w Azure.

##<a name="maintenance-and-support"></a>Konserwacja i obsługa techniczna

**Jak duże może być zestawu danych?**

Każdy zestaw danych może zawierać maksymalnie 100 000 elementów wykazu i w górę do 2048 MB danych dotyczących użycia.
Ponadto subskrypcji może zawierać maksymalnie 10 zestawów danych (modele).

**Gdzie można uzyskać pomoc techniczną dotyczącą zalecenia?**

Pomoc techniczna jest dostępna w witrynie [Microsoft Azure pomocy technicznej](https://social.msdn.microsoft.com/forums/azure/home?forum=MachineLearning) .

**Gdzie można znaleźć warunki użytkowania?**

[Microsoft Azure maszynowego uczenia zalecenia interfejsu API warunki użytkowania usługi](https://datamarket.azure.com/dataset/amla/recommendations#terms).



 
