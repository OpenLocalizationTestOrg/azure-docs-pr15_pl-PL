<properties
    pageTitle="Azure aplikacji pokaz zaangażowania Mobile | Microsoft Azure"
    description="W tym artykule opisano pobierania, jak używać i korzyści wynikające z używania aplikacji pokaz zaangażowania Mobile Azure"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/10/2016"
    ms.author="piyushjo" />

# <a name="azure-mobile-engagement-demo-app"></a>Azure aplikacji pokaz zaangażowania Mobile

Firma Microsoft została opublikowana aplikacja pokaz Azure zaangażowania Mobile dla **systemu iOS**, **Android**i platformach **Windows** ułatwia znajdowanie przydatne zasoby i Dowiedz się więcej o zaangażowania Mobile.

Aplikacja ułatwia:

- Przydatne łącza do zaangażowania Mobile zasoby, takie jak klipy wideo, dokumentacji, na forum pomocy technicznej i gdzie można znaleźć lepszego funkcji łatwo znaleźć.
- Możliwości powiadomienia próbki, które są obsługiwane przez zaangażowania Mobile znaleźć pomysły na własne aplikacji dla urządzeń przenośnych.
- Za pomocą implementacji badaniu jak wdrażać zaangażowania Mobile do własnych aplikacji. Możesz uzyskać do:

    - Zbieranie danych analizy.
    - Implementowanie zaawansowanych scenariuszy powiadomienie typów, takie jak *międzysegmentowe pełnego ekranu* lub *podręcznym*.
    - Implementowanie ankiet i ankiety.
    - Wdrożenie scenariusze danych i wypychanych wypychanych odbiorcze.   

## <a name="app-installation"></a>Instalacji aplikacji
Ta aplikacja jest dostępna w magazynie następujących aplikacji:

- **Uniwersalny Windows pokaz aplikacji**:

    - Pobierz aplikację na [Przechowywanie aplikacja systemu Windows](https://www.microsoft.com/en-us/store/apps/azure-mobile-engagement/9nblggh4qmh2).
    - Aplikacja opracowano jako systemu Windows 10 uniwersalny aplikacji. Kod źródłowy jest dostępna na [GitHub](https://github.com/Azure/azure-mobile-engagement-app-windows).

- **iOS pokaz aplikacji**:

    - Pobierz aplikację na [Przechowywanie firmy Apple](https://itunes.apple.com/us/app/azure%20mobile%20engagement/id1105090090).
    - Aplikacja opracowano w systemie iOS Swift. Kod źródłowy jest dostępna na [GitHub](https://github.com/Azure/azure-mobile-engagement-app-ios).

- **Pokaz android aplikacji**:

    - Pobierz aplikację na [Przechowywanie Google Play](https://play.google.com/store/apps/details?id=com.microsoft.azure.engagement).
    - Kod źródłowy jest dostępna na [GitHub](https://github.com/Azure/azure-mobile-engagement-app-android).

![Aplikacja pokaz uniwersalny systemu Windows][1]

![iOS pokaz aplikacji][2]
![Android pokaz aplikacji][3]


## <a name="usage"></a>Użycie

Za pomocą tej aplikacji w następujący sposób:

**Pobierz aplikację na urządzeniu z poziomu aplikacji do przechowywania łączy (opisane wcześniej):**

>[AZURE.IMPORTANT] Konto Azure ani nie ma potrzeby wewnętrznej nawiązać połączenia z aplikacji nie jest potrzebny. Aplikacja działa niezależnie.

- Po umieszczeniu aplikacji na urządzeniu, można przejść przez łącza w menu po lewej stronie, aby znaleźć przydatne zasoby dotyczące zaangażowania Mobile.
- Dodaliśmy [usługi danych RSS](https://aka.ms/azmerssfeed) w tej aplikacji tak, że jest on zawsze informowany o najnowsze aktualizacje.
- Możesz również przejść do przykładowe scenariusze powiadomienie występują typ powiadomień, które są obsługiwane przez telefon komórkowy zaangażowania dla każdej platformy. Te powiadomienia mogą wystąpić lokalnie — czyli, można klikać przyciski na ekranach Tobie możliwości powiadomienia jest identyczny z wysyłania powiadomień z platformy zaangażowania Mobile.

![Menu aplikacji dla systemu Windows][4]

![Menu aplikacji dla systemu iOS][5]
![menu aplikacji dla systemu Android][6]

**Pobieranie kodu źródłowego z łącza GitHub (opisane wcześniej):**

- Po pobraniu kodu źródłowego, otwórz go w odpowiednie środowisko — XCode dla systemu iOS, Android Studio dla systemu Android i Visual Studio dla systemu Windows.
- Należy dalej wykonaj naszych [podstawowe kroki integracji SDK](mobile-engagement-windows-store-dotnet-get-started.md) tak, aby możesz nawiązać własne wystąpienie wewnętrznej zaangażowania Mobile tej aplikacji.
    - Musisz skonfigurować parametry połączenia w aplikacji.
    - Musisz również skonfigurować platformy powiadomień wypychanych dla aplikacji.
- Należy zauważyć, że samej aplikacji jest narzędzia z zaangażowania Mobile. Dlatego po otwarciu aplikacji po podłączeniu go do wewnętrzna będziesz mieć możliwość Zobacz sesji użytkownika, działania, zdarzeń i tak dalej, na karcie **Monitor** .
- Można także będzie wysyłania powiadomień dla tej aplikacji z własnych wystąpienia zaangażowania Mobile, zamiast lokalnych powiadomienia.
    - W tym miejscu możesz dodać urządzenia jako testowanie urządzenia za pomocą opcji menu **Pobierz identyfikator urządzenia** w aplikacji. Zwraca identyfikator urządzenia, która następnie zarejestrować jako testowanie urządzenia z wystąpienia wewnętrznej platformy.

    ![Identyfikator urządzenia w systemie Windows][7]

    ![Identyfikator urządzenia w systemie iOS][8]
    ![identyfikator urządzenia w systemie Android][9]

## <a name="key-features-of-the-demo-app"></a>Kluczowe funkcje aplikacji pokaz

- Jak wspomniano wcześniej, w tej aplikacji, masz kluczowych zasobów dla zaangażowania Mobile w dłoni. Możesz przejść do łącza w lewym menu.

- Mogą występować w nowym oknie aplikacji Powiadomienia dla każdej platformy. Te powiadomienia mogą być dostarczane jako **tylko powiadomienie**, gdzie klikając powiadomienie po prostu otwiera natywnej ekranu aplikacji (za pomocą **łączenia głębokości**) — lub **sieci Web anons**, gdzie zapewnia dodatkową zawartość HTML z wewnętrznej zaangażowania Mobile jest wyświetlane po kliknięciu powiadomienie.

    ![Powiadomienia w nowym oknie aplikacji][29]

- W systemie iOS musisz zamknąć aplikację, aby otrzymywać powiadomienia wypychane w nowym oknie aplikacji lub systemu. Możesz obejrzeć implementację dodawania **przycisków akcji**, takich jak te, które zostaną dodane do tego powiadomienia w nowym oknie aplikacji *opinii* i *Udostępnij* (tak, aby użytkownik może wymagać bezpośrednio akcji w powiadomieniu się).

    ![Powiadomienia w nowym oknie aplikacji w systemie iOS][11] ![Wyświetlania powiadomień w nowym oknie aplikacji w systemie iOS][14]

- W systemie Android opcji, które są obsługiwane są Dodawanie wielowierszowego tekst (**Duży tekst**) lub obraz powiadomienia (**Przeglądu**) na powiadomienie, wraz z **przyciskami akcji** (jak obsługiwane przez system iOS).

    ![Powiadomienia w nowym oknie aplikacji w systemie Android][12] ![Wyświetlanie powiadomień w nowym oknie aplikacji w systemie Android][15]

- Systemie Windows 10 można zobaczyć, jak powiadomienia o jego wyglądały na tym komputerze. Powiadomienie to również wyświetlane w **Centrum powiadomień**systemu Windows 10. Istnieje nie obsługuje dodawanie **przycisków akcji** w momencie w zestawie SDK systemu Windows.

    ![Powiadomienia w nowym oknie aplikacji w systemie Windows][10] ![Wyświetlanie w nowym oknie aplikacji w systemie Windows][13]

- Mogą występować powiadomienia "w aplikacji" domyślny dla każdego platformy. Jest to środowisko dwuetapowym miejsce, w którym okna **powiadomień** jest wyświetlany jako pierwszy. Po kliknięciu go otwiera się pełny ekran **Zawiadomienie o**wyświetlaną w następujących zrzut ekranu. Zawartość tego ogłoszenia pochodzi z wystąpienia wewnętrznej zaangażowania Mobile. Zestaw SDK zawiera szablony dla powiadomienia i anonsów. Można łatwo dostosować je, jak pokazano w tej aplikacji pokaz z dodatkiem naszych logo i kolorowanie łącze.  

    ![Powiadomienia w aplikacji w systemie Windows][16]

    ![Powiadomienia w aplikacji w systemie iOS][17]  ![Powiadomienia w aplikacji w systemie Android][18]

    **iOS**, **Android**

- Za pomocą funkcji **kategorii** zaangażowania Mobile Aby dostosować to domyślne. W aplikacji pokaz możemy zostały wykazać dwa typowe sposoby zmieniania obsługi powiadomień o wiadomościach. Należy zauważyć, że funkcja kategorii nie jest jeszcze obsługiwana w zestawie SDK systemu Windows.

    **Pełnoekranowy międzysegmentowe:**

    ![Powiadomienie w aplikacji — międzysegmentowe kategorii][30]

    ![Międzysegmentowe kategorii w systemie iOS][21]  ![Międzysegmentowe kategorii w systemie Android][22]

    **Powiadomienie podręczne:**

    ![Powiadomienie w aplikacji — podręczne kategorii][31]

    ![Powiadomienie podręczne w systemie iOS][19]   ![Powiadomienie podręczne w systemie Android][20]

**iOS**, **Android**

- Zaangażowania Mobile umożliwia typu specjalistyczne-app powiadomienie o nazwie **ankiety**. Pozwala na wysyłanie ankiet szybkie użytkownikom segmentowany aplikacji. Możesz dodać pytania i opcje dla każdego pytania, tak jak w następujących zrzut ekranu. Następnie będzie to uzyskać wyświetlany jako powiadomienie w aplikacji do użytkownika aplikacji.   

    ![Powiadomienia ankiety][32]

    ![Ankiety w systemie Windows][26]

    ![Badanie iOS][27]   ![Ankiety w systemie Android][28]

**iOS**, **Android**

- Zaangażowania Mobile umożliwia wysyłanie odbiorcze powiadomień **Push danych** . Z te powiadomienia w przypadku wysłania danych z usługi (na przykład dane JSON w poniższym przykładzie), które można obsługiwane w aplikacji, a także podjąć inną akcję. Przykładem jest, jak firma Microsoft zmieniasz cenę elementu selektywne za pomocą powiadomień wypychanych danych.

    ![Powiadomienia wypychane danych][33]

    ![Powiadomienia wypychane danych w systemie Windows][23]

    ![Powiadomienia push danych w systemie iOS][24]  ![Powiadomienia wypychane danych w systemie Android][25]

**iOS**, **Android**

> [AZURE.NOTE] Możesz wyświetlić szczegółowe instrukcje krok po kroku dla każdego z tych powiadomień, klikając pozycję **kliknij tutaj, aby uzyskać instrukcje dotyczące wysyłania te powiadomienia z platformy zaangażowania Mobile** na dowolnym ekranie powiadomienie próbki.


[1]: ./media/mobile-engagement-demo-apps/home-windows.png
[2]: ./media/mobile-engagement-demo-apps/home-ios.png
[3]: ./media/mobile-engagement-demo-apps/home-android.png
[4]: ./media/mobile-engagement-demo-apps/menu-windows.png
[5]: ./media/mobile-engagement-demo-apps/menu-ios.png
[6]: ./media/mobile-engagement-demo-apps/menu-android.png
[7]: ./media/mobile-engagement-demo-apps/device-id-windows.png
[8]: ./media/mobile-engagement-demo-apps/device-id-ios.png
[9]: ./media/mobile-engagement-demo-apps/device-id-android.png
[10]: ./media/mobile-engagement-demo-apps/out-of-app-windows.png
[11]: ./media/mobile-engagement-demo-apps/out-of-app-ios.png
[12]: ./media/mobile-engagement-demo-apps/out-of-app-android.png
[13]: ./media/mobile-engagement-demo-apps/out-of-app-display-windows.png
[14]: ./media/mobile-engagement-demo-apps/out-of-app-display-ios.png
[15]: ./media/mobile-engagement-demo-apps/out-of-app-display-android.png
[16]: ./media/mobile-engagement-demo-apps/in-app-windows.png
[17]: ./media/mobile-engagement-demo-apps/in-app-ios.png
[18]: ./media/mobile-engagement-demo-apps/in-app-android.png
[19]: ./media/mobile-engagement-demo-apps/pop-up-ios.png
[20]: ./media/mobile-engagement-demo-apps/pop-up-android.png
[21]: ./media/mobile-engagement-demo-apps/interstitial-ios.png
[22]: ./media/mobile-engagement-demo-apps/interstitial-android.png
[23]: ./media/mobile-engagement-demo-apps/data-push-windows.png
[24]: ./media/mobile-engagement-demo-apps/data-push-ios.png
[25]: ./media/mobile-engagement-demo-apps/data-push-android.png
[26]: ./media/mobile-engagement-demo-apps/survey-windows.png
[27]: ./media/mobile-engagement-demo-apps/survey-ios.png
[28]: ./media/mobile-engagement-demo-apps/survey-android.png
[29]: ./media/mobile-engagement-demo-apps/out-of-app.png
[30]: ./media/mobile-engagement-demo-apps/in-app-interstitial.png
[31]: ./media/mobile-engagement-demo-apps/in-app-pop-up.png
[32]: ./media/mobile-engagement-demo-apps/notification-poll.png
[33]: ./media/mobile-engagement-demo-apps/notification-data-push.png
