### <a name="prerequisites"></a>Wymagania wstępne

Musisz mieć konto [Usługi Bus](https://azure.microsoft.com/services/service-bus/) .  

Zanim będzie można używać konta Bus usługi Azure w aplikacji logiczny, musi zezwolić aplikacji logiki do łączenia się z kontem bus usługi. Na szczęście można łatwo z wykonać w logiczny aplikacji portalu Azure.  

Poniżej przedstawiono czynności, aby zezwolić aplikacji logiki do łączenia się z kontem usługi Bus:  

1. Aby utworzyć połączenie z Bus usługi w Projektancie aplikacji logiczny, wybierz pozycję **Pokaż Microsoft zarządzane interfejsy API** na liście rozwijanej. W polu wyszukiwania wpisz **bus usługi** . Zaznacz wyzwalacza lub akcję, którą chcesz użyć.  
    ![Obraz połączenia Bus usługi 1](./media/connectors-create-api-servicebus/servicebus-1.png)  

2. Jeśli nie utworzono dowolne połączenia Bus usługi przed, zostanie wyświetlony monit o podanie poświadczeń usługi Bus. Te poświadczenia są używane do zezwolić aplikacji logika nawiązywanie i uzyskać dostęp do swojego konta usługi Bus danych. Łącznik usługi Bus musi parametry połączenia dla nazw Bus usługi. Wymaga **Zarządzaj** uprawnieniami. Dobrym sposobem wiedzieć, jeśli ciąg połączenia jest obszaru nazw lub podmiot określonych czy zawiera `EntityPath` parametru. Jeśli tak, nie jest parametry połączenia prawym aplikacji logicznych.  
    ![Parametry połączenia Bus usługi](./media/connectors-create-api-servicebus/connectionstring.png)

1. Po otrzymaniu parametry połączenia dla obszaru nazw, możesz go używać połączenia interfejsu API w aplikacjach logicznych.  
    ![Obraz połączenia Bus usługi 2](./media/connectors-create-api-servicebus/servicebus-2.png)  

3. Zwróć uwagę, utworzono połączenie, a teraz są bezpłatne kontynuowanie innych czynności w aplikacji logicznych.  
    ![Obraz połączenia Bus usługi 3](./media/connectors-create-api-servicebus/servicebus-3.png)   
