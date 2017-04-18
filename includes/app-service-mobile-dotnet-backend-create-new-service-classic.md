1. Zaloguj się w [portalu Azure].

2. Kliknij pozycję **+ Nowe** > **Web + Mobile** > **Aplikacji Mobile**, następnie podaj nazwę dla twojej aplikacji Mobile wewnętrznej bazy danych.

3. **Grupa zasobów**wybierz istniejącej grupy zasobów lub utworzyć nową (pod tą samą nazwą jako aplikacji). 
 
    Można wybrać inny plan usługi aplikacji lub Utwórz nowy. Aby uzyskać więcej informacji o usługach aplikacji planów i jak utworzyć nowy plan w innej ceny warstwa i w odpowiedniej lokalizacji, zobacz [Omówienie szczegółowo planów Azure aplikacji usługi](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).

4. **Plan usług aplikacji**domyślny plan (w [standardowej warstwa](https://azure.microsoft.com/pricing/details/app-service/)) jest zaznaczone. Można także wybrać innego planu, lub [Utwórz nowy](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md#create-an-app-service-plan). Planowanie aplikacji usługi ustawień określić [lokalizację, funkcje, kosztów i obliczanie zasobów](https://azure.microsoft.com/pricing/details/app-service/) skojarzony z aplikacją pakietu. 

    Po określeniu w planie, kliknij przycisk **Utwórz**. Spowoduje to utworzenie aplikacji Mobile wewnętrznej bazy danych. 
    
6. W karta **Ustawienia** dla nowej aplikacji Mobile wewnętrznej bazy danych, kliknij **Szybki start** > platformy aplikacji klienckich > **Połącz z bazy danych**. 

    ![](./media/app-service-mobile-dotnet-backend-create-new-service/dotnet-backend-create-data-connection.png)

7. W karta **Dodaj połączenie danych** , kliknij przycisk **Baza danych SQL** > **Utwórz nową bazę danych**, wpisz **nazwę**bazy danych, wybierz cennik warstwy, a następnie kliknij **serwer**.  Można ponownie użyć tej nowej bazy danych. Jeśli masz już bazy danych, w tym samym miejscu, możesz zamiast tego wybrać opcję **Użyj istniejącej bazy danych**. Korzystanie z bazy danych w innej lokalizacji nie jest zalecane z powodu kosztów przepustowość i opóźnienie wyższą.
 
    ![](./media/app-service-mobile-dotnet-backend-create-new-service/dotnet-backend-create-db.png)

8. W karta **nowego serwera** wpisz nazwę serwera unikatowe w polu **Nazwa serwera** , podaj nazwę logowania i hasło, zaznacz **usługi azure Zezwalaj dostępu do serwera**i kliknij **przycisk OK**. Spowoduje to utworzenie nowej bazy danych.

9. Ponownie w karta **Dodaj połączenie danych** kliknij przycisk **Parametry połączenia**, wpisz wartości identyfikator logowania i hasło dla bazy danych i kliknij **przycisk OK**. Poczekaj kilka minut dla bazy danych do wdrożenia pomyślnie przed kontynuowaniem.

<!-- URLs. -->
[Azure Portal]: https://portal.azure.com/
