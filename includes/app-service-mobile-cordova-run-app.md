
1. Odwiedź [Azure Portal]. Kliknij pozycję **Przeglądaj wszystkie** > **Aplikacji Mobile** > wewnętrznej bazy danych, która została właśnie utworzona. W obszarze Ustawienia aplikacji dla urządzeń przenośnych, kliknij **Szybki Start** > **Cordova**. W obszarze **Konfiguruj aplikację klienta**wybierz pozycję **Utwórz nową aplikację**, a następnie kliknij przycisk **Pobierz**. Spowoduje to pobranie cały projekt Cordova wstępnie skonfigurowane do łączenia się z wewnętrznej bazy danych aplikacji.

2. Rozpakowywanie pobrany plik ZIP do katalogu na dysku twardym, przejdź do pliku rozwiązania (.sln), a następnie otwórz go przy użyciu programu Visual Studio.

5. W programie Visual Studio wybierz platformy rozwiązanie (Android, iOS lub systemu Windows) z listy rozwijanej obok strzałkę start, a następnie wybierz urządzenie wdrażania lub emulatora, klikając pozycję na liście rozwijanej na zieloną strzałkę. Uwaga można platformy Android domyślne i wpływu emulatora. Bardziej zaawansowane samouczki wymagają zaznacz obsługiwanego urządzenia lub emulatora. 

6. Naciśnij klawisz F5 lub kliknij zieloną strzałkę, aby utworzyć i i uruchamianie aplikacji Cordova. Jeśli zobaczysz okno dialogowe zabezpieczeń w emulatora żądaniu dostępu do sieci, należy ją zaakceptować.   

7. Po uruchomieniu aplikacji na urządzeniu lub emulatora, wpisz tekst użyteczny w **Wprowadź nowy tekst**, takie jak _Wykonano samouczka_ , a następnie kliknij przycisk **Dodaj** .  
To przesyła żądanie POST do Azure wewnętrznej bazy danych, które wcześniej wdrożono. Wewnętrznej bazy danych wstawia dane z żądania jest do tabeli TodoItem w bazie danych SQL, a następnie zwraca informacje o nowo towary powrót do aplikacji dla urządzeń przenośnych. Aplikacji dla urządzeń przenośnych wyświetla dane na liście.

    ![](./media/app-service-mobile-cordova-quickstart/quickstart-startup.png)
    
8. Powtórz poprzednie trzy kroki dla każdego platformy urządzenia, która ma być obsługiwana.

[Azure Portal]: https://portal.azure.com/
