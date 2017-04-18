
1. Na komputerze Mac odwiedź [Azure Portal]. Kliknij pozycję **Przeglądaj wszystkie** > **Aplikacji Mobile** > wewnętrznej bazy danych, która została właśnie utworzona. W obszarze Ustawienia aplikacji dla urządzeń przenośnych, kliknij **Szybki Start** > **iOS (cel-C)**. Jeśli wolisz Swift, kliknij pozycję **Szybki Start** > **iOS (Swift)** zamiast tego. W obszarze **pobrać i uruchomić iOS projektu**kliknij przycisk **Pobierz**. Spowoduje to pobranie cały projekt Xcode wstępnie skonfigurowane do łączenia się z wewnętrznej bazy danych aplikacji. Otwieranie projektu przy użyciu Xcode.

2. Kliknij przycisk **Uruchom** , aby budowania projektu i uruchamianie aplikacji w iOS simulator.

3. W aplikacji, wpisz opisową tekstu, takie jak _Wykonano samouczka_ , a następnie kliknij przycisk plus (**+**) ikona. To przesyła żądanie POST do Azure wewnętrznej bazy danych, które wcześniej wdrożono. Wewnętrznej bazy danych wstawia dane z żądania jest do tabeli TodoItem SQL i zwraca informacje o nowo towary powrót do aplikacji dla urządzeń przenośnych. Aplikacji dla urządzeń przenośnych wyświetla dane na liście. 

    ![](./media/app-service-mobile-ios-quickstart/mobile-quickstart-startup-ios.png)

[Azure Portal]: https://portal.azure.com/
