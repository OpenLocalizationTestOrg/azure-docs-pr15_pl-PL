
1. Odwiedź [Azure Portal]. Kliknij pozycję **Przeglądaj wszystkie** > **Aplikacji Mobile** > wewnętrznej bazy danych, która została właśnie utworzona. W obszarze Ustawienia aplikacji dla urządzeń przenośnych, kliknij **Szybki Start** > **Android)**. W obszarze **Konfiguruj aplikację klienta**kliknij przycisk **Pobierz**. Spowoduje to pobranie cały projekt Android wstępnie skonfigurowane do łączenia się z wewnętrznej bazy danych aplikacji. 

2. Otwórz projekt przy użyciu **Android Studio**, przy użyciu **Importowanie projektu (ADT Zaćmienie, Gradle itp.)**. Upewnij się, że wybierz odpowiednią pozycję Importuj, aby uniknąć błędów JDK.

3. Kliknij przycisk **Uruchom "aplikację"** do tworzenia projektu i uruchom aplikację w Android simulator.

4. W aplikacji wpisz opisową tekst, na przykład _Wykonano samouczka_ , a następnie kliknij przycisk "Dodaj". To przesyła żądanie POST do Azure wewnętrznej bazy danych, które wcześniej wdrożono. Wewnętrznej bazy danych wstawia dane z żądania jest do tabeli TodoItem SQL i zwraca informacje o nowo towary powrót do aplikacji dla urządzeń przenośnych. Aplikacji dla urządzeń przenośnych wyświetla dane na liście. 

    ![](./media/app-service-mobile-android-quickstart/mobile-quickstart-startup-android.png)

[Azure Portal]: https://portal.azure.com/
