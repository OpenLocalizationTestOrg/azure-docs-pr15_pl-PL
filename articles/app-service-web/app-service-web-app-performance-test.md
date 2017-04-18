<properties
   pageTitle="Testowanie wydajności aplikacji sieci web Azure | Microsoft Azure"
   description="Uruchom testy wydajności aplikacji Azure sieci web, aby sprawdzić, jak aplikacji obsługuje ładowania użytkownika. Zmierz czas reakcji i Znajdź błędy, które mogą wskazywać problemów."
   services="app-service\web"
   documentationCenter=""
   authors="ecfan"
   manager="douge"
   editor="jimbe"/>

<tags
   ms.service="app-service-web"
   ms.workload="web"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="05/25/2016"
   ms.author="estfan; manasma; ahomer"/>

# <a name="performance-test-your-azure-web-app-under-load"></a>Testowanie wydajności aplikacji sieci web Azure obciążeniu

Sprawdzanie wydajności aplikacji sieci web, przed jego uruchamianie lub rozmieszczania aktualizacji produkcji. W ten sposób można można lepiej ocenić, czy aplikacja jest gotowy do wersji. Działanie większa pewność, że aplikacji może obsługiwać dane podczas używania Szczyt lub do następnej akcji marketingowej.

Podczas publicznej podglądu możesz testu wydajności aplikacji bezpłatnie w Azure Portal.
Testy zasymulowania użytkownika obciążenie aplikacji w określonym czasie i zmierzyć odpowiedzi aplikacji sieci. Na przykład wyświetlić wyniki testu szybkości aplikacji odpowiada określoną liczbę użytkowników. Jest wyświetlany, ile żądań nie powiodło się, które mogą występować problemy ze aplikacji.      

![Znajdowanie problemów z wydajnością w aplikacji sieci web](./media/app-service-web-app-performance-test/azure-np-perf-test-overview.png)

## <a name="before-you-start"></a>Przed rozpoczęciem

* Jeśli nie masz jeszcze trzeba [Azure subskrypcji](https://account.windowsazure.com/subscriptions). Dowiedz się, jak [utworzyć konto Azure bezpłatnie](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).

* Musisz mieć konto [Programu Visual Studio Team Services](https://www.visualstudio.com/products/what-is-visual-studio-online-vs) , aby zachować historię test wydajności. Odpowiednie konto zostanie utworzony automatycznie podczas konfigurowania usługi testu wydajności. Lub można utworzyć nowe konto lub użyj istniejącego konta, jeśli jesteś właścicielem konta. 

* Wdrażanie aplikacji do testowania w środowisku produkcyjnym nie. Masz plan aplikacji usługi niż plan produkcji za pomocą aplikacji. W ten sposób nie wpływa na wszystkich obecnych klientów lub spowolnić aplikacji produkcji. 

## <a name="set-up-and-run-your-performance-test"></a>Konfigurowanie i testu wydajności

0.  Zaloguj się do [portalu Azure](https://portal.azure.com). Za pomocą konta programu Visual Studio Team Services, że jesteś właścicielem, zaloguj się jako właściciel konta.

0.  Przejdź do aplikacji sieci web.

    ![Przejdź do pozycji Przeglądaj wszystkie aplikacje sieci Web, aplikacji sieci web](./media/app-service-web-app-performance-test/azure-np-web-apps.png)

0.  Przejdź do **testu wydajności**.

    ![Przejdź do narzędzia testu wydajności](./media/app-service-web-app-performance-test/azure-np-web-app-details-tools-expanded.png)
 
0. Teraz będzie łącze konta programu [Visual Studio Team Services](https://www.visualstudio.com/products/what-is-visual-studio-online-vs) do zachowania historii test wydajności.

    Jeśli masz konto usługi zespołów, które będzie używane, wybierz tego konta. Jeśli nie Utwórz nowe konto.

    ![Wybierz istniejące konto usługi zespołu lub Utwórz nowe konto](./media/app-service-web-app-performance-test/azure-np-no-vso-account.png)

0.  Tworzenie usługi testu wydajności. Ustawianie szczegółów i testu. 

Test uruchomiony, możesz obejrzeć wyniki są wyświetlane w czasie rzeczywistym.

Załóżmy, że mamy aplikację nadaje się kupony podczas sprzedaży świąt ostatni rok. To zdarzenie trwał 15 minut z obciążeniem Szczyt 100 klientów jednocześnie. Chcemy dwukrotnie liczby klientów w tym roku. Chcemy też poprawić zadowolenia klientów, skracanie czasu ładowania strony z 5 sekund na 2 sekundy. Tak firma Microsoft będzie testowanie wydajności naszych zaktualizowane aplikacji za pomocą 250 użytkowników na 15 minut.

Firma Microsoft będzie Symulacja obciążenia w naszym aplikacji wygenerowanie użytkowników wirtualnych (klienci) osób, które można znaleźć w witrynie sieci web w tym samym czasie. Spowoduje to pokazują liczbę żądań są awarii lub odpowiada powoli.

  ![Tworzenie, konfigurowanie i testu wydajności](./media/app-service-web-app-performance-test/azure-np-new-performance-test.png)

   *  Domyślny adres URL aplikacji sieci web jest dodawany automatycznie. 
   Możesz zmienić adres URL, aby przetestować innych stron (tylko HTTP GET żądania).

   *  Aby symulować lokalnych warunków i zmniejszyć opóźnienie, wybierz lokalizację najbliższych użytkowników do wygenerowania ładowania.

  W tym miejscu jest test w toku. Podczas pierwszej minucie naszych ładowania strony wolniej, niż oczekujemy.

  ![Test wydajności w toku z dane czasu rzeczywistego](./media/app-service-web-app-performance-test/azure-np-running-perf-test.png)

  Po zakończeniu badania firma Microsoft Dowiedz się znacznie szybsze ładowania strony po pierwszym minucie. Dzięki temu identyfikowanie miejsce firma Microsoft może chcesz rozpocząć rozwiązywanie problemu.

  ![Testowanie ukończony wydajności pokazano wyniki, w tym nie powiodło się żądania](./media/app-service-web-app-performance-test/azure-np-perf-test-done.png)

## <a name="test-multiple-urls"></a>Testowanie wiele adresów URL

Można również uruchomić zawierających wiele adresów URL, reprezentujących scenariusz użytkownika końcowego do końca, przekazując plik Visual Studio Web Test testów wydajności. Niektóre metody można utworzyć Visual Test Web Studio są pliku:

* [Przechwytywanie ruchu przy użyciu Fiddler i wyeksportować jako plik Visual Studio Test sieci Web](http://docs.telerik.com/fiddler/Save-And-Load-Traffic/Tasks/VSWebTest)
* [Tworzenie pliku test ładowania w programie Visual Studio](https://www.visualstudio.com/docs/test/performance-testing/run-performance-tests-app-before-release)

Aby przekazać i uruchom plik Visual Studio Web Test:
 
0. Wykonaj powyższe kroki, aby otworzyć karta **Testowanie wydajności** .
   W tym karta wybierz opcję CONFIGFURE testowanie za pomocą, aby otworzyć karta **test konfiguracji za pomocą** .  

    ![Otwieranie ładowania Konfigurowanie, testowanie karta](./media/app-service-web-app-performance-test/multiple-01-authoring-blade.png)

0. Sprawdź, czy typ testu jest ustawiona do **Visual Studio Test sieci Web** i wybierz plik archiwum HTTP.
    Użyj ikony "folder", aby otworzyć okno dialogowe selektor pliku.

    ![Przekazywanie wielu plików adresu URL Visual Studio Web Test](./media/app-service-web-app-performance-test/multiple-01-authoring-blade2.png)

    Po przekazaniu pliku zostanie wyświetlona lista adresów URL sprawdzone w sekcji SZCZEGÓŁÓW adresu URL.
 
0. Umożliwia określenie obciążenia użytkownika i przetestować czas trwania, a następnie wybierz **testu**.

    ![Zaznaczanie ładowania użytkownika i czas trwania](./media/app-service-web-app-performance-test/multiple-01-authoring-blade3.png)

    Po zakończeniu test możesz wyświetlić wyniki w dwa okienka. W okienku po lewej stronie są wyświetlane informacje performnace jako seria wykresów.

    ![Okienko wyników wydajności](./media/app-service-web-app-performance-test/multiple-01a-results.png)

    Okienku po prawej stronie zawiera listę żądania nie powiodło się, przy użyciu typu błędu i liczbę razy, w którym wystąpił.

    ![W okienku błędy żądanie](./media/app-service-web-app-performance-test/multiple-01b-results.png)

0. Ponownie uruchom test, wybierając ikonę **Uruchom** u góry prawego okienka.

    ![Powtórzenia badań](./media/app-service-web-app-performance-test/multiple-rerun-test.png)

##  <a name="q--a"></a>Pytania i odpowiedzi

#### <a name="q-is-there-a-limit-on-how-long-i-can-run-a-test"></a>P: czy istnieje limit na jakim można uruchamiać test? 

**A**: tak, można uruchomić programu test maksymalnie godzinę Azure Portal.

#### <a name="q-how-much-time-do-i-get-to-run-performance-tests"></a>P: jak długo przejść do uruchomienia testów wydajności? 

**A**: po publicznej podglądu zostanie wyświetlony 20 000 minut wirtualnych użytkownika (VUMs) bezpłatne każdego miesiąca przy użyciu konta programu Visual Studio Team Services. VUM jest to liczba użytkowników wirtualnych pomnożona przez liczbę minut w swojej przetestuj. Twoje potrzeby przekroczenia limitu bezpłatne można kupić więcej czasu i zapłacić tylko w przypadku służą.

#### <a name="q-where-can-i-check-how-many-vums-ive-used-so-far"></a>P: gdzie sprawdzić, ile VUMs wcześniej używanych pory?

**A**: możesz sprawdzić ta kwota w Azure Portal.

![Przejdź do swojego konta usługi zespołu](./media/app-service-web-app-performance-test/azure-np-vso-accounts.png)

![Sprawdzanie VUMs używane](./media/app-service-web-app-performance-test/azure-np-vso-accounts-vum-summary.png)

#### <a name="q-what-is-the-default-option-and-are-my-existing-tests-impacted"></a>P: co to jest domyślna opcja i ma wpływ moje istniejące testów?

**A**: opcji domyślnych dla testów wydajności ładowania jest test ręcznego — tak samo jak przed wielu adresu URL przetestować opcję zostało dodane do portalu.
Istniejące testy nadal korzystać skonfigurowanego adresu URL i będzie działać tak jak poprzednio.

#### <a name="q-what-features-not-supported-in-the-visual-studio-web-test-file"></a>P: jakie funkcje nie są obsługiwane w pliku programu Visual Studio Test sieci Web?

**A**: obecnie ta funkcja nie obsługuje wtyczki Testowanie sieci Web, źródeł danych i wyodrębniania reguły. Należy edytować pliku Test sieci Web, aby usunąć te elementy. Firma Microsoft dotyczy dodawanie pomocy technicznej dla tych funkcji w przyszłych aktualizacji.

#### <a name="q-does-it-support-any-other-web-test-file-formats"></a>P: obsługuje inne formaty plików Testowanie sieci Web?
  
**A**: na prezentowanie tylko Visual Studio Web Test plików w formacie są obsługiwane.
Firma Microsoft może być przyjemnością poznać Twoją opinię, jeśli potrzebujesz pomocy dotyczącej innych formatów plików. Wiadomość e-mail na adres [vsoloadtest@microsoft.com](mailto:vsoloadtest@microsoft.com).

#### <a name="q-what-else-can-i-do-with-a-visual-studio-team-services-account"></a>P: co jeszcze można zrobić za pomocą konta programu Visual Studio Team Services?

**A**: Aby znaleźć nowego konta, przejdź do ```https://{accountname}.visualstudio.com```. Udostępnianie kodu, tworzenie, testowanie, śledzenie pracy i oprogramowanie wysyłki — wszystko w chmurze za pomocą narzędzi lub język. Dowiedz się więcej o tym, jak funkcje [Programu Visual Studio Team Services](https://www.visualstudio.com/products/what-is-visual-studio-online-vs) i usług ułatwić zespołu łatwiej współpracować i wdrażanie przez cały czas.

## <a name="see-also"></a>Zobacz też

* [Uruchom testy wydajności prostej chmury](https://www.visualstudio.com/docs/test/performance-testing/getting-started/get-started-simple-cloud-load-test)
* [Uruchom testy wydajności Apache Jmeter](https://www.visualstudio.com/docs/test/performance-testing/getting-started/get-started-jmeter-test)
* [Rejestrowanie i powtarzania testów obciążenia opartego na chmurze](https://www.visualstudio.com/docs/test/performance-testing/getting-started/record-and-replay-cloud-load-tests)
* [Wydajność testowanie aplikacji w chmurze](https://www.visualstudio.com/docs/test/performance-testing/getting-started/getting-started-with-performance-testing)
