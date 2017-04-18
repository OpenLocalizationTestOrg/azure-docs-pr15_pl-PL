<properties 
    pageTitle="Korzystanie z funkcji logicznych aplikacji | Microsoft Azure" 
    description="Dowiedz się, jak korzystać z zaawansowanych funkcji logicznych aplikacji." 
    authors="stepsic-microsoft-com" 
    manager="erikre" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="03/28/2016"
    ms.author="stepsic"/> 
    
# <a name="use-logic-apps-features"></a>Korzystanie z funkcji logicznych aplikacji

W [poprzednim temacie](app-service-logic-create-a-logic-app.md)utworzono pierwszej aplikacji logicznych. Teraz możemy procedurach pokazano, jak tworzyć dokładniejszy proces przy użyciu aplikacji logika usług. W tym temacie pojęcia następujące nowe aplikacje logika:

- Wyrażenie warunkowe, które wykonuje akcję tylko wtedy, gdy określony warunek jest spełniony.
- Edytowanie istniejącej aplikacji logika widok kodu.
- Opcje uruchamiania przepływu pracy.

Zanim będzie można wykonać w tym temacie, należy wykonać kroki w sekcji [Tworzenie nowej aplikacji logicznych](app-service-logic-create-a-logic-app.md). W [portalu Azure]przejdź do aplikacji logika i kliknij **Wyzwalacze i akcje** podsumowanie dotyczące edytowania definicji aplikacji logika.

## <a name="reference-material"></a>Materiały informacyjne

Następujące dokumenty mogą okazać się przydatne:

- [Zarządzania i pozostałe interfejsów API czasu wykonywania](https://msdn.microsoft.com/library/azure/mt643787.aspx) - tym sposobów bezpośrednio wywołania logiki aplikacji
- [Dokumentacja języka](https://msdn.microsoft.com/library/azure/mt643789.aspx) — Pełna lista wszystkich obsługiwanych funkcje i wyrażeń
- [Typy wyzwalacza i akcji](https://msdn.microsoft.com/library/azure/mt643939.aspx) — różne typy akcji i danych wejściowych, które przyjmować
- [Omówienie aplikacji usługi](../app-service/app-service-value-prop-what-is.md) — opis jakie składniki wybierz, kiedy utworzenie rozwiązania

## <a name="adding-conditional-logic"></a>Dodawanie wyrażenie warunkowe

Chociaż oryginalny przepływ działa, istnieje kilka obszarów, które można poprawić. 


### <a name="conditional"></a>Warunkowe
Ta aplikacja logiczny może spowodować naliczenie możesz wprowadzenie wiele wiadomości e-mail. Następujące kroki dodać logikę, aby się upewnić, że tylko otrzymają wiadomość e-mail po tweet pochodzi od kogoś z określoną liczbę obserwujących. 

1. Kliknij przycisk plus i Znajdź akcji *Uzyskiwanie użytkownika* dla Twitter.

2. Przekazać w polu **Tweeted przez** z wyzwalacza, aby uzyskać informacje o użytkowniku Twitter.

    ![Uzyskiwanie użytkownika](./media/app-service-logic-use-logic-app-features/getuser.png)

3. Kliknij ponownie przycisk plus, ale tym razem wybierz pozycję **Dodaj warunek**

4. W pierwszym polu kliknij przycisk **...** pod **Uzyskiwanie użytkownika** do znalezienia pola **Liczba obserwujących** .

5. Na liście rozwijanej wybierz pozycję **większe niż**

6. W drugim polu wpisz liczbę obserwujących mają mieć użytkownicy.

    ![Warunkowe](./media/app-service-logic-use-logic-app-features/conditional.png)

7.  Na koniec przeciąganie i upuszczanie wiadomości e-mail pole w polu **Jeśli tak** . Oznacza to, że tylko otrzymasz wiadomości e-mail po spełnieniu statystykę następca.

## <a name="repeating-over-a-list-with-foreach"></a>Powtarzanie listy z forEach

Pętli forEach Określa tablicę, aby powtórzyć operację na. Jeśli nie jest tablicą przepływ zakończy się niepowodzeniem. Na przykład jeśli masz action1, która wyświetla tablicę wiadomości i chcesz wysłać każdej wiadomości można dołączyć to Instrukcja forEach we właściwościach działanie: forEach:"@action('action1').outputs.messages"
 

## <a name="using-the-code-view-to-edit-a-logic-app"></a>Edytowanie aplikacji logiczny przy użyciu widoku kodu

Oprócz designer można bezpośrednio edytować kod, który definiuje aplikacji logiczny, w następujący sposób. 

1. Kliknij przycisk **widoku kodu** na pasku poleceń. 

    Spowoduje to otwarcie pełny edytor, który pokazuje definicji, który został właśnie edytowany.

    ![Wyświetlanie kodu](./media/app-service-logic-use-logic-app-features/codeview.png)

    Za pomocą edytora tekstu, możesz skopiuj i Wklej dowolną liczbę Akcje poziomu samej aplikacji logiki i między aplikacjami logicznych. Można także łatwo można dodawać i usuwać całą sekcję z definicji i definicje można także udostępnić innym.

2. Po wprowadzeniu zmian w widoku kodu, po prostu kliknij przycisk **Zapisz**. 

### <a name="parameters"></a>Parametry
Istnieje kilka możliwości aplikacji logiczny, który może być używany tylko w widoku kodu. Przykładem tych jest parametry. Parametry ułatwiają do ponownego użycia wartości w całej aplikacji logicznych. Na przykład jeśli masz adres e-mail, którego chcesz użyć w kilku akcji, należy go zdefiniować jako parametru.

Następujące aktualizacje istniejących aplikacji logika używania parametrów terminu kwerendy.

1. W widoku Kod, Znajdź `parameters : {}` obiektu i wstawić obiekt następujących tematów:

        "topic" : {
            "type" : "string",
            "defaultValue" : "MicrosoftAzure"
        }
    
2. Przewiń do `twitterconnector` akcji, zlokalizuj wartość kwerendy i zamienić `#@{parameters('topic')}`.
    Funkcja **"concat"** można również użyć do łączenia dwóch lub więcej ciągów, na przykład: `@concat('#',parameters('topic'))` jest identyczny z powyższych. 
 
Parametry są dobrym sposobem wysunąć wartości, które mogą się znacznie zmieniać. Są one szczególnie przydatna w sytuacji, gdy trzeba zastępować parametrów w różnych środowiskach. Aby uzyskać więcej informacji na temat zastępowania parametrów na podstawie środowiska, zobacz naszą [dokumentację interfejsu API usługi REST](https://msdn.microsoft.com/library/mt643787.aspx).

Teraz po kliknięciu przycisku **Zapisz**co godzinę możesz uzyskać wszelkie nowe tweety, zawierające więcej niż 5 retweets dostarczane w folderze o nazwie **tweety** w Twojej skrzynki.

Aby dowiedzieć się więcej na temat definicji aplikacji logiczny, zobacz [Tworzenie definicji aplikacji logiczny](app-service-logic-author-definitions.md).

## <a name="starting-a-logic-app-workflow"></a>Uruchamianie przepływu pracy aplikacji warunków logicznych
Istnieje kilka opcji uruchamiania przepływu pracy zdefiniowanych w logiczny aplikacji. Przepływ pracy można zawsze uruchamiać na żądanie w [Azure portal].

### <a name="recurrence-triggers"></a>Cykl wyzwalaczy
Uruchamia wyzwalacz cyklu interwałem określonym przez użytkownika. Gdy wyzwalacz zawiera wyrażenie warunkowe, wyzwalacz Określa, czy przepływ pracy wymaga do uruchomienia. Wskazuje wyzwalacz ma być uruchamiany przez zwrócenie `200` kodu stanu. Jeśli nie potrzebuje do uruchomienia, zwraca wartość `202` kodu stanu.

### <a name="callback-using-rest-apis"></a>Za pomocą interfejsów API pozostałych zwrotnego
Usługi można zadzwonić punkt końcowy aplikacji logiki do uruchamiania przepływu pracy. Więcej informacji można znaleźć w [logiczny aplikacji jako żądanie punkty końcowe](app-service-logic-connector-http.md) . Aby rozpocząć tego rodzaju logiki aplikacji na żądanie, kliknij przycisk **Uruchom teraz** na pasku poleceń. 

<!-- Shared links -->
[Azure portal]: https://portal.azure.com 