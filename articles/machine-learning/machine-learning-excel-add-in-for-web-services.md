<properties
    pageTitle="Dodatek programu Excel dla usług sieci Web uczenia maszyny | Microsoft Azure"
    description="Jak używać usługi Azure maszynowego Learning w sieci Web bezpośrednio w programie Excel bez pisania kodów."
    services="machine-learning"
    documentationCenter=""
    authors="tedway"
    manager="jhubbard"
    editor="cgronlun"
    tags=""/>

<tags
    ms.service="machine-learning"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="10/05/2016"
    ms.author="tedway;garye" />

# <a name="excel-add-in-for-azure-machine-learning-web-services"></a>Dodatek programu Excel dla usług Azure maszynowego Learning w sieci Web

Program Excel można łatwo połączeń usług sieci Web bezpośrednio bez konieczności pisania kodu.

## <a name="steps-to-use-an-existing-web-service-in-the-workbook"></a>Czynności, aby skorzystać z usługi istniejącą witrynę sieci Web w skoroszycie

1. Otwórz [Przykładowy plik programu Excel](http://aka.ms/amlexcel-sample-2), który zawiera dodatek programu Excel i dane o osób na Titanic.
2. Wybierz usługę sieci Web, klikając go-"Titanic renty predykcyjnych (próbka w dodatku programu Excel) [Wynik]" w tym przykładzie.

    ![Wybierz pozycję Usługa sieci Web][01]

3. Spowoduje to przejście do sekcji **Predict** .  W tym skoroszycie zawiera już dane przykładowe, ale dla pustego skoroszytu można zaznaczyć komórkę w programie Excel i kliknij pozycję **Użyj przykładowych danych**.
4. Zaznacz dane z nagłówkami i kliknij ikonę zakresu danych wejściowych.  Upewnij się, że jest zaznaczone pole "Moje dane mają nagłówki".
5. W obszarze **dane wyjściowe**wprowadź liczby komórek, w którym chcesz wynik być, na przykład "H1" poniżej.
6. Kliknij pozycję **przewidywanie**.

    ![Przewidywanie sekcji][02]

## <a name="steps-to-add-a-new-web-service"></a>Czynności, aby dodać usługę nowej sieci Web

Wdrażanie usługi sieci Web lub użyj istniejącej usługi sieci Web. Aby uzyskać więcej informacji o wdrażaniu usługi sieci Web, zobacz [Przewodnik krok 5: Wdrażanie usługi sieci Web uczenia maszynowego Azure](machine-learning-walkthrough-5-publish-web-service.md).

Uzyskaj klucz interfejsu API usługi sieci Web. Gdzie można to zrobić w zależności od tego, czy opublikowane usługi klasyczny maszynowego Learning w sieci Web usługi nowego komputera Learning w sieci Web.

**Za pomocą usługi sieci Web klasyczny** 

1. W komputerze nauki Studio kliknij sekcję **Usług sieci WEB** w okienku po lewej stronie, a następnie wybierz usługi sieci Web.

    ![Wybierz pozycję Studio usługi sieci Web][04]

2. Kopiowanie klucz interfejsu API usługi sieci Web.

    ![Klucz interfejsu API Studio][05]

3. Na karcie **pulpitu NAWIGACYJNEGO** usługi sieci Web kliknij łącze **Żądanie/odpowiedź** .
4. Poszukaj sekcji **Żądanie identyfikatora URI** .  Kopiowanie i zapisanie adresu URL.

>[AZURE.NOTE] Użytkownik może teraz zalogować się do portalu [Usługi sieci Web uczenia maszynowego Azure](https://services.azureml.net) klucz interfejsu API usługi klasyczny maszynowego Learning w sieci Web.

**Za pomocą usługi sieci Web nowy**

1. W portalu [Usługi sieci Web uczenia maszynowego Azure](https://services.azureml.net) kliknij **Usług sieci Web**, a następnie wybierz pozycję Usługa sieci Web. 
2. Kliknij pozycję **korzystać**.
3. Poszukaj sekcji **Zużycie podstawowe informacje** . Kopiowanie i zapisać **Klucz podstawowy** i adres URL **Żądanie odpowiedź** .


## <a name="steps-to-add-a-new-web-service"></a>Czynności, aby dodać usługę nowej sieci Web

1. Wdrażanie usługi sieci Web lub użyj istniejącej usługi sieci Web. Aby uzyskać więcej informacji o wdrażaniu usługi sieci Web, zobacz [Przewodnik krok 5: Wdrażanie usługi sieci Web uczenia maszynowego Azure](machine-learning-walkthrough-5-publish-web-service.md).
2. Kliknij pozycję **korzystać**.
3. Poszukaj sekcji **Zużycie podstawowe informacje** . Kopiowanie i zapisać **Klucz podstawowy** i adres URL **Żądanie odpowiedź** .
2. W programie Excel, przejdź do sekcji **Usług sieci Web** (Jeśli znajdujesz się w sekcji **Predict** , kliknij strzałkę Wstecz, aby przejść do listy usług sieci Web).

    ![Przejdź do Wybieranie usługi sieci Web][03]
    
3. Kliknij przycisk **Dodaj usługi sieci Web**.
4. Wklej adres URL w polu tekstowym w dodatku programu Excel etykietą **adresu URL**.
5. Wklej klucz interfejsu API-podstawowy w polu tekstowym **klucz interfejsu API**.
6. Kliknij przycisk **Dodaj**.

    ![Adres URL i interfejsu API klucz klasyczny usługi sieci Web.][06]

10. Aby korzystać z usługi sieci Web, postępuj zgodnie z instrukcjami poprzedniego, "Czynności, aby użyć usługi sieci Web istniejący".

## <a name="sharing-your-workbook"></a>Udostępnianie skoroszytu

Po zapisaniu skoroszytu klucz interfejsu API/podstawowy dla usług sieci Web, które zostały dodane również jest zapisywane. Oznacza to, że skoroszyt należy udostępniać tylko dla osób, którym ufasz.

Zadaj pytania w następnej sekcji komentarza lub nasze [forum](http://go.microsoft.com/fwlink/?LinkID=403669&clcid=0x409)w witrynie.

[01]: ./media/machine-learning-excel-add-in-for-web-services/image1.png
[02]: ./media/machine-learning-excel-add-in-for-web-services/image2.png
[03]: ./media/machine-learning-excel-add-in-for-web-services/image3.png
[04]: ./media/machine-learning-excel-add-in-for-web-services/image4.png
[05]: ./media/machine-learning-excel-add-in-for-web-services/image5.png
[06]: ./media/machine-learning-excel-add-in-for-web-services/image6.png
