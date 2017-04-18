### <a name="prerequisites"></a>Wymagania wstępne
- Konto Twilio
- Zatwierdzonych numer telefonu Twilio, który może odbierać wiadomości SMS
- Zatwierdzonych numer telefonu Twilio, które może przesyłać wiadomości SMS

>[AZURE.NOTE] Jeśli używasz Twilio konto wersji próbnej usługi SMS można wysłać tylko do **weryfikacji** numerów telefonów.  

Zanim będzie można używać konta Twilio w aplikacji logiczny, możesz zezwolić aplikacji logiki do łączenia się z kontem Twilio. Na szczęście można można to zrobić z poziomu aplikacji logiczny na Azure Portal. 

Poniżej przedstawiono czynności, aby zezwolić aplikacji logiki do łączenia się z kontem Twilio:

1. Aby utworzyć połączenie z Twilio, w Projektancie logiki aplikacji, wybierz pozycję **Pokaż Microsoft zarządzane interfejsy API** na liście rozwijanej, a następnie wprowadź *Twilio* w polu wyszukiwania. Zaznacz wyzwalacz lub akcji będzie chcesz użyć:  
  ![](./media/connectors-create-api-twilio/twilio-0.png)
2. Jeśli nie utworzono dowolne połączenia Twilio przed, zostanie wyświetlony monit informujący o podanie poświadczeń Twilio. Te poświadczenia są używane do zezwolić aplikacji logika nawiązywania połączenia z i uzyskać dostęp do swojego konta Twilio danych:  
  ![](./media/connectors-create-api-twilio/twilio-1.png)  
3. Musisz **identyfikator konta Twilio** i **Twilio token dostępu** z pulpitu nawigacyjnego w Twilio, aby zalogować się do konta Twilio teraz wykonać tych dwóch rodzajów informacji:  
  ![](./media/connectors-create-api-twilio/twilio-2.png)  
4. Aplikacje Twilio i układy logiczne za pomocą różnych nazw do identyfikowania tych dwóch rodzajów informacji. Poniżej opisano, jak należy zamapować je do okna dialogowego aplikacje logiczny:![](./media/connectors-create-api-twilio/twilio-3.png)  
5. Wybierz przycisk **Utwórz połączenie** :  
  ![](./media/connectors-create-api-twilio/twilio-4.png)
6. Zwróć uwagę, połączenie została utworzona i teraz są bezpłatne kontynuowanie innych czynności w aplikacji logiczny:  
  ![](./media/connectors-create-api-twilio/twilio-5.png)