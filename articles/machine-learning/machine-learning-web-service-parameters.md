<properties 
    pageTitle="Azure maszynowego uczenia parametrów usługi sieci Web za pomocą | Microsoft Azure" 
    description="Jak za pomocą parametrów usługi sieci Web Azure maszynowego uczenia modyfikowanie zachowania modelu podczas uzyskiwania dostępu do usługi sieci web." 
    services="machine-learning" 
    documentationCenter="" 
    authors="raymondlaghaeian" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/10/2016" 
    ms.author="raymondl;garye"/>

#<a name="use-azure-machine-learning-web-service-parameters"></a>Używanie Azure maszynowego uczenia parametrów usługi sieci Web

Usługi sieci web Azure maszynowego uczenia jest tworzona przez opublikowanie doświadczenia zawierający moduły z parametry można konfigurować. W niektórych przypadkach można zmienić moduł uruchomiona usługa sieci web. *Parametry usługi sieci Web* umożliwiają wykonać to zadanie. 

[Importowanie danych] konfiguruje typowy przykład[ reader] modułu tak, aby określić użytkowników usługi sieci web opublikowanych innego źródła danych podczas uzyskiwania dostępu do usługi sieci web. Lub skonfigurować [Eksportowanie danych] [ writer] modułu tak, aby można określić inne miejsce docelowe. Inne przykłady Zmienianie liczbę bitów w [Funkcji mieszania] [ feature-hashing] modułu lub liczbę potrzeby funkcji [filtr oparty wybór funkcji] [ filter-based-feature-selection] modułu. 

Można ustawić parametry usługi sieci Web i kojarzenie ich z jeden lub więcej parametrów modułu w swojej doświadczeniu i czy są one wymagane czy opcjonalne. Użytkownik usługi sieci web można podać wartości dla tych parametrów podczas wywołują usługi sieci web. 

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


##<a name="how-to-set-and-use-web-service-parameters"></a>Jak ustawić i korzystają z parametrów usługi sieci Web

Należy zdefiniować parametr usługi sieci Web, klikając ikonę obok parametr dla modułu i wybierając "Ustaw jako parametr usługi sieci web". Tworzy nowy parametr usługi sieci Web, a nawiązywanie parametru modułu. Następnie podczas uzyskiwania dostępu do usługi sieci web, użytkownik może określić wartości parametru usługi sieci Web i zostanie zastosowana do parametru modułu.

Po zdefiniowaniu parametr usługi sieci Web są dostępne dla każdego parametru modułu w doświadczeniu. Po zdefiniowaniu parametr usługi sieci Web skojarzonego z parametrem dla jednego modułu służy tego samego parametr usługi sieci Web dla innych modułu, dopóki parametru przewiduje wartość tego samego typu. Na przykład jeśli parametr usługi sieci Web jest wartością liczbową, następnie go można używać tylko dla parametrów modułu, które się spodziewać wartość liczbową. Gdy użytkownik ustawia wartość parametru usługi sieci Web, zostanie zastosowany do wszystkich parametrów powiązany moduł.

Możesz zdecydować, czy chcesz podać wartość domyślną dla parametr usługi sieci Web. Jeśli jednak parametr jest opcjonalny dla użytkownika usługi sieci web. Jeśli nie zawiera wartości domyślnej, użytkownik jest wymagany do wprowadź wartość podczas uzyskiwania dostępu do usługi sieci web.

Dokumentacja interfejsu API usługi sieci web zawiera informacje dla użytkownika usługi sieci web na programowy określania parametr usługi sieci Web podczas uzyskiwania dostępu do usługi sieci web.

>[AZURE.NOTE] W dokumentacji interfejsu API usługi sieci web klasyczny znajduje się za pomocą łącza **Strona pomocy interfejsu API** usługi sieci web **pulpitu NAWIGACYJNEGO** w Studio nauki komputera. Dokumentacja API dla nowej usługi sieci web jest dostępna za pośrednictwem portalu [Usług sieci Web uczenia maszynowego Azure](https://services.azureml.net/Quickstart) na stronach **Consume** i **Swagger interfejsu API** usługi sieci web.


##<a name="example"></a>Przykład

Na przykład załóżmy mamy doświadczenia [Eksportowanie danych] [ writer] modułu, który przesyła informacje z magazynem obiektów blob platformy Azure. Będzie zdefiniować parametr usługi sieci Web o nazwie "Blob ścieżki", która umożliwia użytkownika usługi sieci web zmienić ścieżkę z magazynem obiektów blob podczas uzyskiwania dostępu do usługi.

1.  W Studio nauki komputera, kliknij pozycję [Eksportuj dane] [ writer] moduł, aby go zaznaczyć. Jego właściwości są wyświetlane w okienku właściwości po prawej stronie obszaru roboczego doświadczenia.

2.  Określanie typu magazynu:

    - W obszarze, **Podaj miejsce docelowe danych**wybierz "Magazyn obiektów Blob Azure".
    - W obszarze, **Podaj typ uwierzytelniania**wybierz "Konto".
    - Wprowadź informacje o koncie dla magazyn obiektów blob platformy Azure. 
    <p />

3.  Kliknij ikonę z prawej strony **ścieżkę do blob rozpoczynający się od kontenerze parametr**. Wygląda następująco:

    ![Ikona parametr usługi sieci Web][icon]

    Wybierz pozycję "Ustaw jako parametr usługi sieci web".

    Wpis zostanie dodany w obszarze **Parametry usługi sieci Web** w dolnej części okienka właściwości o nazwie "Ścieżkę do blob rozpoczynający się od kontenera". Jest to parametr usługi sieci Web, która jest teraz skojarzone z tym [Eksportowanie danych] [ writer] parametr modułu.

4.  Aby zmienić nazwę parametr usługi sieci Web, kliknij nazwę, wpisz "Blob ścieżki", a następnie naciśnij klawisz **Enter** . 
 
5.  Aby udostępnić wartość domyślną dla parametr usługi sieci Web, kliknij ikonę z prawej strony nazwy, wybierz pozycję "Podać wartość domyślną", wprowadź wartość (na przykład "container1/output1.csv") i naciśnij klawisz **Enter** .

    ![Parametr usługi sieci Web][parameter]

6.  Kliknij przycisk **Uruchom**. 

7.  Kliknij **Wdrażanie usługi sieci Web** i wybierz **Wdrażanie usługi sieci Web [klasyczny]** lub **Wdrażanie usługi sieci Web [nowy] do wdrożenia usługi sieci web** .

[Eksportowanie danych] użytkownika usługi sieci web można teraz wprowadzić nowego miejsca docelowego[ writer] modułu podczas uzyskiwania dostępu do usługi sieci web.

##<a name="more-information"></a>Więcej informacji

Na przykład bardziej szczegółowe zobacz [Parametry usługi sieci Web](http://blogs.technet.com/b/machinelearning/archive/2014/11/25/azureml-web-service-parameters.aspx) wpis w [Blogu nauki komputera](http://blogs.technet.com/b/machinelearning/archive/2014/11/25/azureml-web-service-parameters.aspx).

Aby uzyskać więcej informacji na uzyskiwanie dostępu do usługi sieci web maszynowego uczenia zobacz [jak zajmować opublikowanych maszynowego uczenia usługi sieci web](machine-learning-consume-web-services.md).



<!-- Images -->
[icon]: ./media/machine-learning-web-service-parameters/icon.png
[parameter]: ./media/machine-learning-web-service-parameters/parameter.png


<!-- Module References -->
[feature-hashing]: https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/
[filter-based-feature-selection]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[reader]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[writer]: https://msdn.microsoft.com/library/azure/7a391181-b6a7-4ad4-b82d-e419c0d6522c/
 
