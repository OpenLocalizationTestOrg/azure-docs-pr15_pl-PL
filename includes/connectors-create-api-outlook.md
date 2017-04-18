## <a name="connect-to-outlookcom"></a>Połączenia z usługą Outlook.com

### <a name="prerequisites"></a>Wymagania wstępne
- Konta usługi Outlook.com

Zanim będzie można używać konta usługi Outlook.com w aplikacji logiczny, musi zezwolić aplikacji logiki do łączenia się z kontem usługi Outlook.com. Na szczęście można to z zrobić w aplikacji logiczny na Azure Portal. 

Poniżej przedstawiono czynności, aby zezwolić aplikacji logiki do łączenia się z kontem usługi Outlook.com:

1. Wszystkie aplikacje logiczny konieczne będzie wyzwalane przez wyzwalacza, aby po utworzeniu aplikacji logiczny, projektanta otwiera i wyświetla listę wyzwalane, który umożliwia uruchomić aplikację logiczny:

  ![](./media/connectors-create-api-outlook/office365-outlook-0.png)
2. W polu wyszukiwania wprowadź "outlook". Należy zauważyć, że lista jest filtrowana, aby wyświetlić listę wszystkich wyzwalaczy z "Outlook" w nazwie:![](./media/connectors-create-api-outlook/office365-outlook-0-5.png)
3. Wybierz pozycję **Office 365 Outlook — w nowej wiadomości e-mail**.   
  Jeśli nie utworzono wszystkie połączenia z programem Outlook przed, zostanie wyświetlony monit informujący o podanie poświadczeń usługi Outlook.com. Te poświadczenia są używane do Autoryzuj aplikacji logika nawiązania połączenia, a dostęp do danych konta usługi Outlook.com:![](./media/connectors-create-api-outlook/office365-outlook-1.png)
4. Podaj poświadczenia dla programu Outlook, a następnie zaloguj się:![](./media/connectors-create-api-outlook/office365-outlook-2.png)  
  To wszystko. Teraz utworzeniu połączenia z programem Outlook. To połączenie będzie dostępny do użytku w dowolnej innej aplikacji logicznych, które są tworzone.


