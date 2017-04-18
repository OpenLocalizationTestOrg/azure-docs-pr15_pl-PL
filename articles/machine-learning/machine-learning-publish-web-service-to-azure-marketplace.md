<properties 
    pageTitle="Publikowanie maszynowego uczenia sieci web usługi Azure Marketplace | Microsoft Azure" 
    description="Jak publikować usługi Azure maszynowego uczenia sieci Web do usługi Azure Marketplace" 
    services="machine-learning" 
    documentationCenter="" 
    authors="BharathS" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/08/2016" 
    ms.author="bharaths"/>

# <a name="publish-azure-machine-learning-web-service-to-the-azure-marketplace"></a>Publikowanie Azure usługi sieci Web uczenia Azure Marketplace 

Azure Marketplace umożliwia publikowanie Azure maszynowego uczenia usług sieci web jako zapłacone lub zwolnij usług zużycia przez klientów zewnętrznych. Ten artykuł zawiera omówienie tego procesu z łączami do wskazówki ułatwiające rozpoczęcie pracy. Korzystając z tego procesu, można udostępnić usług sieci web dla innych deweloperów do wykorzystania w swoich aplikacjach.


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="overview-of-the-publishing-process"></a>Omówienie procesu publikowania 

Poniżej przedstawiono kroki do publikowania usługi sieci web Azure maszynowego uczenia Azure Marketplace:

1. Tworzenie i publikowanie usługi szkoleniowe komputera żądanie-odpowiedź (RR)
2. Wdrażanie usługi produkcji oraz uzyskiwanie informacji punktu końcowego klucz interfejsu API i OData.
3. Użyj adresu URL usługi sieci web opublikowanych publikowanie [Azure Marketplace (Data Market)](https://publish.windowsazure.com/workspace/) 
4. Po przesłane, przejrzeć Twoją ofertę i musi być zatwierdzone przed klientów można rozpocząć zakupu go. Proces publikowania może zająć kilka dni roboczych. 

## <a name="walk-through"></a>Szczegółową
###<a name="step-1-create-and-publish-a-machine-learning-request-response-service-rrs"></a>Krok 1: Tworzenie i publikowanie usługi szkoleniowe komputera żądanie-odpowiedź (RR)###
 Jeśli jeszcze tego nie to już, Poświęć przyjrzeć się tym [szczegółową](machine-learning-walkthrough-5-publish-web-service.md).

###<a name="step-2-deploy-the-service-to-production-and-obtain-the-api-key-and-odata-endpoint-information"></a>Krok 2: Wdrażanie usługi produkcji i uzyskać klucz interfejsu API oraz informacje punktu końcowego OData###
1. W [Klasycznym Portal Azure](http://manage.windowsazure.com)wybierz opcję **Nauka komputera** z na lewym pasku nawigacyjnym, a następnie wybierz obszaru roboczego. 

2. Kliknij kartę **Usług sieci WEB** , a następnie wybierz usługę sieci web, którą chcesz opublikować na rynku.

    ![Azure Marketplace][workspace]

3. Wybierz punkt końcowy chcesz mieć marketplace korzystać. Jeśli nie została utworzona wszelkie dodatkowe punkty końcowe, możesz wybrać **domyślny** punkt końcowy.

4. Po kliknięciu przycisku dla punktu końcowego, można wyświetlić **Klucz interfejsu API**. Konieczne będzie ten element informacji później w kroku 3, dlatego należy jego kopię.

    ![Azure Marketplace][apikey]

5. Kliknij metodę **Żądanie/odpowiedź** w tym punkcie, że firma Microsoft nie obsługuje publikowania usług wykonanie partii do witryny marketplace programu. Spowoduje to do strony pomocy interfejsu API metody żądanie/odpowiedź.

6. Skopiuj **Adres końcowy OData**, konieczne będzie te informacje później w kroku 3.

    ![Azure Marketplace][odata]




Wdrażanie usługi do produkcji.



###<a name="step-3-use-the-url-of-the-published-web-service-to-publish-to-azure-marketplace-datamarket"></a>Krok 3: Użyj adresu URL usługi sieci web opublikowanych publikowanie Azure Marketplace (DataMarket)###

1.  Przejdź do [usługi Azure Marketplace (Data Market)](http://datamarket.azure.com/home) 
2.  Kliknij łącze **Publikuj** w górnej części strony. Spowoduje to [Portal publikowania Microsoft Azure](https://publish.windowsazure.com)
3.  Kliknij sekcję **wydawcy** , aby zarejestrować jako wydawcy.
4.  Podczas tworzenia nowej oferty, wybierz pozycję **Usług danych**, a następnie kliknij pozycję **Utwórz nową usługę danych**. 
 
    ![Azure Marketplace][image1]

    <br />


5.  W obszarze **Plany** zawierają informacje dotyczące oferty, w tym planu cennik. Zdecyduj, jeśli będzie oferować usługę bezpłatnych i płatnych. Otrzymywania płatności, podaj informacje o płatności, takich jak informacje o banku i podatku.

6.  W obszarze **marketingowych** zawierają informacje o Twoją ofertę, takie jak tytuł i opis Twoją ofertę.

7.  W obszarze **ceny** można ustawić cenę dla planów dla określonych krajów lub pozostawić "autoprice" Twoją ofertę.

8. Na karcie **Usługi danych** kliknij **Usługi sieci Web** jako **Źródła danych**.

    ![Azure Marketplace][image2]

9.  Uzyskiwanie klucza adres URL i interfejsu API usługi sieci web z portalu klasyczny Azure, zgodnie z opisem w kroku 2 powyżej.

10. W oknie dialogowym Konfiguracja usługi danych Marketplace Wklej adres końcowy OData w polu tekstowym **Adres URL usługi** .

11. W przypadku **uwierzytelniania**wybierz **Nagłówek** jako **Schemat uwierzytelniania**.

    - Wprowadź "Autoryzacji" **Nazwa nagłówka**.
    - **Wartość nagłówka**wprowadź "Okaziciela" (bez cudzysłowów), kliknij na pasku **miejsca** , a następnie wklej klucz interfejsu API.
    - Zaznacz pole wyboru **tej usługi jest OData** .
    - Kliknij przycisk **Testuj połączenie** , aby przetestować połączenie.

12. W obszarze **Kategorie**upewnij się, że wybrano **Nauki komputera** .

13. Po zakończeniu wprowadzania wszystkich metadanych dotyczących Twoją ofertę polecenie **Publikuj**, a następnie **przekazać do tymczasowego**. W tym momencie będzie być powiadamiany o wszystkich pozostałych problemów, które trzeba usunąć.

14. Po upewnieniu się ukończenia wszystkich pozostałych problemów, kliknij na **żądanie zatwierdzenia do produkcji**. Proces publikowania może zająć kilka dni roboczych. 


[image1]:./media/machine-learning-publish-web-service-to-azure-marketplace/image1.png
[image2]:./media/machine-learning-publish-web-service-to-azure-marketplace/image2.png
[workspace]:./media/machine-learning-publish-web-service-to-azure-marketplace/selectworkspace.png
[apikey]:./media/machine-learning-publish-web-service-to-azure-marketplace/apikey.png
[odata]:./media/machine-learning-publish-web-service-to-azure-marketplace/odata.png
 
