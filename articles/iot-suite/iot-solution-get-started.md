<properties
    pageTitle="Przykład MyDriving Azure IoT: Szybki start | Microsoft Azure"
    description="Wprowadzenie do aplikacji, która jest pełna pokaz zaprojektować IoT system przy użyciu programu Microsoft Azure, w tym analizy strumieniu, nauki komputera i koncentratory zdarzenia."
    services=""
    documentationCenter=".net"
    suite=""
    authors="harikmenon"
    manager="douge"/>

<tags
    ms.service="multiple"
    ms.workload="tbd"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="03/25/2016"
    ms.author="harikm"/>

# <a name="mydriving-iot-system-quick-start"></a>System MyDriving IoT: Szybki start

MyDriving jest systemem demonstrujący projektowanie i wdrażanie typowe rozwiązania [Internet czynności](iot-suite-overview.md) (IoT) zbiera telemetrycznego z urządzeń, przetwarzania danych w chmurze i dotyczy maszynowego uczenia zapewnienie adaptacyjne odpowiedź. Pokaz rejestruje dane dotyczące podróży z samochodami, za pomocą danych z telefonu komórkowego i karty, która gromadzi informacje z systemu kontroli samochód. Aby przekazać opinię o Twój kierowania styl w porównaniu z systemem innym użytkownikom go używa tych danych.

Liczba rzeczywista celem MyDriving jest ułatwiające rozpoczęcie pracy podczas tworzenia rozwiązania IoT. Ale przed nim, Przejdźmy możesz rozpoczęcie pracy z aplikacją MyDriving się — jako członek nasz zespół użytkownika. Umożliwia obsługi aplikacji oraz system związany z nim klientów, zanim delve do architektury. On również poznasz HockeyApp, atrakcyjne sposób zarządzania alfa i beta podziału aplikacji do użytkowników testowych.

## <a name="use-the-mobile-experience"></a>Korzystanie z obsługi urządzeń przenośnych

Jeśli masz urządzenia z systemem Android, iOS lub systemu Windows 10, można użyć aplikacji MyDriving.

### <a name="android-and-windows-10-mobile-installation"></a>Android i instalacji systemu Windows 10 Mobile

Na urządzeniu:

1.  Zezwalaj opracowywania aplikacji:

    -   Android: W **ustawieniach** > **zabezpieczeń**Zezwalaj aplikacji z **nieznanych źródeł**.

    -   Windows 10: W **ustawieniach** > **aktualizacje** > **Dla deweloperów**ustawić **Tryb dewelopera**.

2.  Dołączanie nasz zespół beta przez zarejestrowanie u lub logowania się do [HockeyApp](https://rink.hockeyapp.net). HockeyApp ułatwia rozpowszechnianie wcześniejsze wersje aplikacji do użytkowników testowych.

    Jeśli korzystasz z systemu Windows 10, za pomocą przeglądarki krawędzi.

    Gdyby uczestnika kompilacji 2016, zaloguj się przy użyciu samej Microsoft konta wiadomości e-mail zarejestrowany konferencji przy użyciu jednego z przyciski przez firmę Microsoft. Już masz konto z HockeyApp.

    ![HockeyApp ekran logowania](./media/iot-solution-get-started/image1.png)

3.  Pobierz i zainstaluj aplikację w tym miejscu:

    -   [Android](http://rink.io/spMyDrivingAndroid)

    -   [Windows 10](http://rink.io/spMyDrivingUWP)

    Istnieją dwa elementy. Instalowanie certyfikatu w **Zaufanych osób**. Następnie zainstaluj aplikację.

*Problemy podczas uruchamiania aplikacji w systemie Windows 10 Mobile?* Telefon może być aktualizacja lub dwie pod spód. Upewnij się, że masz najnowsze aktualizacje lub instalacji:

 - [Microsoft.NET.Native.Framework.1.2.appx](https://download.hockeyapp.net/packages/win10/Microsoft.NET.Native.Framework.1.2.appx) 

 - [Microsoft.NET.Native.Runtime.1.1.appx](https://download.hockeyapp.net/packages/win10/Microsoft.NET.Native.Runtime.1.1.appx) 

 - [Microsoft.VCLibs.ARM.14.00.appx](https://download.hockeyapp.net/packages/win10/Microsoft.VCLibs.ARM.14.00.appx)


### <a name="ios-installation"></a>iOS instalacji

Jeśli które miały miejsce kompilacji 2016, Pobierz aplikację jako członek nasz zespół na HockeyApp:

1.  Na urządzeniu z systemem iOS Zaloguj się do [HockeyApp](https://rink.hockeyapp.net).
    Użyj jednej z Microsoft logowania przyciski i zaloguj się przy użyciu samej Microsoft konta wiadomości e-mail, którą zarejestrowanych w konferencji. (Nie używaj pola adresu e-mail i hasła).

    ![HockeyApp ekran logowania](./media/iot-solution-get-started/image1.png)

2.  Na pulpicie nawigacyjnym HockeyApp wybierz pozycję MyDriving i ją pobrać.

3.  Autoryzuj wersji beta z HockeyApp:

    . Przejdź do pozycji **Ustawienia** > **Ogólne** > **profilów i zarządzania urządzeniami.**

    b. Zaufania dla certyfikatu **GmbH Stadium bitową** .

Jeśli nie bierzesz udział kompilacji 2016, możesz tworzyć i samodzielnie wdrożyć aplikację:

1.   Pobieranie kodu [z GitHub].

2.   Tworzenie i wdrażanie przy [użyciu Xamarin].

Więcej informacji można znaleźć w [MyDriving przewodnik](http://aka.ms/mydrivingdocs).

## <a name="get-an-obd-adapter-optional"></a>Uzyskiwanie karty diagnostycznego (opcjonalnie)

To jest element, który sprawia, że to rzeczywistą system Internet rzeczy! Za pomocą aplikacji bez nich, ale jest urozmaicanie rzeczywistą element, a nie są drogich.

Płycie Diagnostyka jest funkcją samochód garaż używany do dostosować w górę samochód i diagnozowanie hałasu na stronach parzystych i światła ostrzeżenie. O ile samochód jest doskonałe antyków, znajdziesz socket w dowolnym miejscu w podręcznego, zwykle za klapy w obszarze pulpitu nawigacyjnego. Z prawej łącznika można uzyskać wskaźniki wydajności aparatu i wprowadzić niektórych dostosowań. Łącznik diagnostycznego można kupić tanio z zwykły miejsc. Łączy przy użyciu sieci Wi-Fi lub Bluetooth do aplikacji na telefonie.

W tym przypadku możemy zacząć łączenie samochodu w chmurze. Bezpośredniego połączenia z diagnostycznego jest na Twój telefon, ale działa nasz aplikacja przekazywania. Samochód telemetrycznego są wysyłane bezpośrednio do koncentratora MyDriving IoT, gdzie są przetwarzane dziennika podróży z drogi i oceny do kierowania styl.

Aby podłączyć urządzenie diagnostycznego:

1.  Sprawdź, czy samochód ma socket diagnostycznego.

2.  Aby uzyskać karty diagnostycznego:

    -   Jeśli korzystasz z systemem Android lub Windows phone, potrzebna jest karta II diagnostycznego technologię Bluetooth. Użyliśmy [BAFX 34t5 Bluetooth OBDII skanowania narzędzie do produktów].

    -   Jeśli korzystasz z telefonie z systemem iOS, potrzebna jest karta diagnostycznego włączone sieci Wi-Fi. Firma [ScanTool OBDLink MX sieci Wi-Fi: diagnostycznego karty/Diagnostic skanera].

3.  Postępuj zgodnie z instrukcjami dostarczonymi z kartą diagnostycznego, aby połączyć go z telefonu. Należy pamiętać o następujących pamiętać:

    -   Karta Bluetooth muszą łączyć się z telefonu, na stronie **Ustawienia** .

    -   Karta sieci Wi-Fi musi mieć adres w 192.168.xxx.xxx zakresie.

4.  Jeśli masz kilka samochody, możesz uzyskać karty osobnym dla każdego (maksymalnie trzy).

Jeśli nie masz karty diagnostycznego, aplikacja nadal będzie wysyłać lokalizacji i szybkości danych z telefonu GPS odbiorcy w celu przechodzenia wstecz i zostanie wyświetlone pytanie, czy chcesz symulować diagnostycznego.

Można znaleźć więcej informacji o używaniu danych z karty diagnostycznego aplikacji i informacji na temat opcji do tworzenia urządzenia diagnostycznego w sekcji 2.1 "Urządzeń IoT," [Przewodnik po MyDriving](http://aka.ms/mydrivingdocs).

## <a name="use-the-app"></a>Korzystanie z aplikacji

Uruchom aplikację. Istnieje początkowej Szybki Start do szczegółową jak to działa.

### <a name="track-your-trips"></a>Śledzenie usługi podróży

Naciśnij przycisk Rejestruj (duży czerwone zakreślenie u dołu ekranu), aby rozpocząć wycieczki, a następnie naciśnij ponownie, aby zakończyć.

![Ilustracja przedstawiająca przycisk Rejestruj podczas podróży śledzenia](./media/iot-solution-get-started/image2.png)

Podczas każdego uruchomienia podróży, w przypadku urządzenie nie diagnostycznego, zostanie wyświetlony monit Czy chcesz używać simulator.

Na końcu wycieczki naciśnij przycisk Zatrzymaj i zostanie wyświetlony podsumowanie.

![Przykład wyjazdu podsumowania](./media/iot-solution-get-started/image3.png)

### <a name="review-your-trips"></a>Przeglądanie z podróży

![Przykład ostatnich podróży](./media/iot-solution-get-started/image4.png)

### <a name="review-your-profile"></a>Przejrzyj swój profil

![Przykład profilu bo stylu](./media/iot-solution-get-started/image5.png)

## <a name="send-us-your-test-feedback"></a>Wyślij nam swoją opinię test

Ponieważ utworzonych MyDriving ułatwiające szybkiego rozpoczęcia tworzenia systemów IoT chcemy na pewno poznać Twoją opinię o tym, jak działa. Pozwól nam sprawdzić, czy:

- Możesz napotkać problemy lub problemach.

- Ma punktu wewnętrzny, który spowodowałby jej bardziej odpowiednie do rozwiązania.

- Możesz znaleźć bardziej wydajnym sposobem wykonywania określonych wymagań.

- Masz inne sugestii dotyczących poprawy MyDriving lub Niniejsza dokumentacja.

W samej aplikacji MyDriving można użyć wbudowanego mechanizmu opinii HockeyApp: w systemie iOS i Android, po prostu Przekaż telefonu wstrząsania lub za pomocą polecenia menu **opinii** . To automatycznie wstawi zrzut ekranu, tak aby firma Microsoft wie, co rozmowę o. A w przypadku dowolnej przykra awarii HockeyApp zbiera dzienniki awarię, aby przekazać nam o nich. Można również przesłać opinię za pośrednictwem [portalu HockeyApp].

Można także plików [problem na GitHub]lub zostaw komentarz poniżej (en-us edition).

Czekamy na słuchu od Ciebie!

## <a name="next-steps"></a>Następne kroki

-   Eksplorowanie [Przewodnik MyDriving](http://aka.ms/mydrivingdocs) należy rozumieć jak została firma Microsoft zaprojektowane i wbudowane całego systemu MyDriving.

-   [Tworzenie i wdrażanie własny system](iot-solution-build-system.md) przy użyciu naszych skryptów Azure Menedżera zasobów. [Przewodnik po MyDriving](http://aka.ms/mydrivingdocs) również przeprowadzi Cię przez obszary, gdzie będzie wprowadzić większości dostosowań.

  [z GitHub]: https://github.com/Azure-Samples/MyDriving
  [przy użyciu Xamarin]: https://developer.xamarin.com/guides/ios/getting_started/installation/
  [Narzędzie skanowania OBDII do BAFX produktów 34t5 Bluetooth]: http://www.amazon.com/gp/product/B005NLQAHS
  [Skaner karty Diagnostic diagnostycznego ScanTool OBDLink MX sieci Wi-Fi:]: http://www.amazon.com/gp/product/B00OCYXTYY/ref=s9_simh_gw_g263_i1_r?pf_rd_m=ATVPDKIKX0DER&pf_rd_s=desktop-2&pf_rd_r=1MWRMKXK4KK9VYMJ44MP
  [HockeyApp portal]: https://rink.hockeyapp.org
  [problem na GitHub]: https://github.com/Azure-Samples/MyDriving/issues
