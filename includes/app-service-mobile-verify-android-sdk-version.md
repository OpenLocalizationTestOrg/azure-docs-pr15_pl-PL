Ze względu na bieżący rozwój zainstalowanej w Android Studio wersji Android SDK mogą być niezgodne wersji w kodzie. Android SDK, do których odwołują się tego samouczka jest wersja 23, r podczas pisania. Numer wersji może zwiększyć nowości zestawu SDK są wyświetlane, a zalecamy używanie najnowszej wersji dostępne.

Są dwa objawów niezgodność wersji:

1. Podczas tworzenia lub odbudowanie projektu może zostać wyświetlony komunikat o błędzie Gradle, takie jak "**nie można odnaleźć docelowej Google Inc.:Google APIs:n**".

2. Na podstawie standardowych Android obiektów w kodzie, który należy rozwiązać `import` instrukcji generować komunikaty o błędach.

Jeśli dowolnego z tych są wyświetlane, wersji zestawu SDK systemu Android zainstalowanych w Android Studio mogą nie odpowiadać docelowej SDK pobrany projektu.  Aby sprawdzić wersję, należy wprowadzić następujące zmiany:


1. W Android Studio, kliknij pozycję **Narzędzia** => **Android** => **SDK Menedżera**. Jeśli nie zainstalowano najnowszą wersję platformy SDK, kliknij go zainstalować. Zanotuj numer wersji.

2. Na karcie Eksplorator projektu, w obszarze **Gradle skryptów**, otwórz plik **build.gradle (modeule: aplikacja)**. Upewnij się, że **compileSdkVersion** i **buildToolsVersion** są ustawione na najnowszą wersję zestawu SDK zainstalowany. Znaczniki może wyglądać następująco:
 
            compileSdkVersion 'Google Inc.:Google APIs:23'
            buildToolsVersion "23.0.2"
    
3. W Eksploratorze projektu Android Studio kliknij prawym przyciskiem myszy węzeł projektu, wybierz pozycję **Właściwości**, a w kolumnie po lewej stronie wybierz pozycję **Android**. Upewnij się, że **Docelowej Tworzenie projektu** jest ustawiona na tej samej wersji SDK jako **targetSdkVersion**.

4. W Android Studio pliku manifestu nie jest już służy do określania docelowych SDK i minimalna wersja SDK, w odróżnieniu od w przypadku Zaćmienie.
