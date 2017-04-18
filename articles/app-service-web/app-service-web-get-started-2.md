<properties
    pageTitle="Dodawanie funkcji do pierwszej aplikacji sieci web"
    description="Dodawanie atrakcyjne funkcje, które do pierwszej aplikacji sieci web w ciągu kilku minut."
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""
/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="05/12/2016"
    ms.author="cephalin"
/>

# <a name="add-functionality-to-your-first-web-app"></a>Dodawanie funkcji do pierwszej aplikacji sieci web

W [Wdrażanie aplikacji sieci web pierwszego Azure w pięć minut](app-service-web-get-started.md)aplikacji sieci web dla próbki jest wdrożony w [Usłudze Azure aplikacji](../app-service/app-service-value-prop-what-is.md). W tym artykule będzie szybko dodać kilka doskonałych funkcji do aplikacji sieci web wdrożonej. W ciągu kilku minut konieczne będzie:

- Wymuszanie uwierzytelniania dla użytkowników
- Automatyczne skalowanie aplikacji
- otrzymywać alerty dotyczące wydajności aplikacji

Niezależnie od tego, w której aplikacji przykładowych wdrożony w artykule poprzedniego mogli wykonać opisane w samouczku.

Trzy czynności w ramach tego samouczka są tylko kilka przykładów wiele użytecznych funkcji, które pojawia się po umieszczeniu aplikacji sieci web w aplikacji usługi. Te funkcje są dostępne w warstwie **wolnego** (czyli pierwszy aplikacji sieci web działa na), i możesz korzystać z wersji próbnej środków wypróbować działanie funkcji, które wymagają wyższej ceny poziomów. Możesz mieć pewność, że aplikacji sieci web pozostaje w warstwie **wolny** chyba że jawnie zmian do innej warstwy cennik.

>[AZURE.NOTE] Aplikacji sieci web, utworzone za pomocą interfejsu wiersza polecenia Azure działa w warstwie **wolny** , dzięki czemu tylko jedno wystąpienie maszyn wirtualnych udostępnionego z przydziałów zasobów. Aby uzyskać więcej informacji na uzyskiwanie z poziomu **wolny** zobacz [limity aplikacji usługi](../azure-subscription-service-limits.md#app-service-limits).

## <a name="authenticate-your-users"></a>Uwierzytelniania użytkowników

Teraz zobaczmy, jak łatwe jest dodawanie uwierzytelniania do aplikacji (więcej informacji w [Aplikacji usługi uwierzytelniania i autoryzacji](https://azure.microsoft.com/blog/announcing-app-service-authentication-authorization/)).

1. W portalu karta dla aplikacji, która właśnie otwarty, kliknij pozycję **Ustawienia** > **uwierzytelniania i autoryzacji**.  
    ![Uwierzytelnianie — karta Ustawienia](./media/app-service-web-get-started/aad-login-settings.png)

2. Kliknij przycisk **w** celu Włącz uwierzytelnianie.  

4. W oknie dialogowym **Dostawców uwierzytelniania**kliknij pozycję **Usługi Azure Active Directory**.  
    ![Uwierzytelnianie — wybierz Azure AD](./media/app-service-web-get-started/aad-login-config.png)

5. W karta **Ustawienia usługi Azure Active Directory** kliknij polecenie **Express**, a następnie kliknij **przycisk OK**. Domyślne ustawienia Tworzenie nowej aplikacji Azure AD w katalogu domyślne.  
 ![Uwierzytelnianie - express konfiguracji](./media/app-service-web-get-started/aad-login-express.png)

6. Kliknij przycisk **Zapisz**.  
    ![Uwierzytelnianie — Zapisywanie konfiguracji](./media/app-service-web-get-started/aad-login-save.png)

    Po pomyślnym zmiana pojawi się mila powiadomienie Włączanie zielony wraz z przyjazną wiadomości.

7. W portalu karta aplikacji kliknij łącze **adres URL** (lub **Przejdź** na pasku menu). Łącze jest adres HTTP.  
    ![Uwierzytelnianie — przejdź do adresu URL](./media/app-service-web-get-started/aad-login-browse-click.png)  
    Ale po jego otwarciu aplikacji na nowej karcie adres URL przekierowania kilka razy i polu kończy się w dniu aplikacji adres HTTPS. Co to jest wyświetlany jest jesteś już zalogowany do subskrypcji usługi Azure, a użytkownik jest automatycznie uwierzytelniony w aplikacji.  
    ![Uwierzytelnianie — zalogowany](./media/app-service-web-get-started/aad-login-browse-http-postclick.png)  
    Aby teraz podczas otwierania nieuwierzytelnionych sesji za pomocą innej przeglądarki, po przejściu do tego samego adresu URL zostanie wyświetlony ekran logowania.  
    <!-- ![Authenticate - login page](./media/app-service-web-get-started/aad-login-browse.png)  -->
   Nigdy nie wykonaniu nic z usługą Azure Active Directory, domyślny katalog może nie ma żadnych użytkowników Azure AD. W takim przypadku prawdopodobnie tylko konto w niego jest konto Microsoft wraz z subskrypcji usługi Azure. Dlatego były automatycznie zalogowany w tej samej przeglądarki wcześniej aplikacji.
   Można użyć tego samego konta Microsoft, aby zalogować się na tej stronie logowania.

Gratulacje, cały ruch są uwierzytelniania aplikacji sieci web.

Możesz zauważyć w **uwierzytelniania i autoryzacji** karta, które może wykonać wiele innych obiektów, takich jak:

- Włączanie społecznościowych logowania
- Włączanie wielu opcje logowania
- Zmienić domyślne zachowanie, gdy użytkownicy najpierw przechodzić do aplikacji

Aplikacji usługi zawiera klucz Włącz rozwiązanie w przypadku niektórych typowych uwierzytelniania musi więc nie trzeba samodzielnie logiczny uwierzytelniania.
Aby uzyskać więcej informacji zobacz [Aplikacji usług uwierzytelniania i autoryzacji](https://azure.microsoft.com/blog/announcing-app-service-authentication-authorization/).

## <a name="scale-your-app-automatically-based-on-demand"></a>Skalowanie aplikacji automatycznie oparte na żądanie

Następny, Przejdźmy autoscale aplikacji tak, aby go będzie automatycznie dopasować go zdolności odpowiedzieć na żądanie użytkownika (więcej informacji w [skali w górę aplikacji platformy Azure](web-sites-scale.md) i [Skalowanie licznik wystąpień ręcznie lub automatycznie](../monitoring-and-diagnostics/insights-how-to-scale.md)).

Krótko skalowanie aplikacji sieci web na dwa sposoby:

- [Rozbudowy](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling): Uzyskaj więcej Procesora, pamięci, miejsca na dysku i dodatkowe funkcje, takie jak dedykowane maszyny wirtualne, domen niestandardowych i certyfikatów, tymczasowej gniazda, autoscaling i innych elementów. Skalowanie w górę, zmieniając warstwie cennik plan usług aplikacji, której należy aplikacji.
- [Skala się](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling): zwiększenie liczby maszyn wirtualnych obiektów uruchamianie aplikacji.
Można skalować się nawet 50 wystąpienia, w zależności od usługi cennik warstwy.

Bez dalszego ado skonfiguruj autoscaling.

1. Najpierw Przyjrzyjmy rozbudowy umożliwiający autoscaling. W portalu karta aplikacji, kliknij przycisk **Ustawienia** > **Skala w górę (Plan usługi aplikacji)**.  
    ![Rozbudowy — karta Ustawienia](./media/app-service-web-get-started/scale-up-settings.png)

2. Przewiń i zaznacz warstwy **Standardowe S1** warstwie najniższe obsługuje autoscaling (w kółku w zrzut ekranu), a następnie kliknij przycisk **Wybierz**.  
    ![Rozbudowy — Wybieranie poziomu](./media/app-service-web-get-started/scale-up-select.png)

    Gotowe skalowania w górę.

    >[AZURE.IMPORTANT] Ten poziom wydatków do bezpłatnej wersji próbnej środków. Jeśli masz konto płatności użycia go z opłatami do swojego konta.

3. Następnie Załóżmy Konfigurowanie autoscaling. W portalu karta aplikacji, kliknij przycisk **Ustawienia** > **Skala się (Plan usługi aplikacji)**.  
    ![Skala się — karta Ustawienia](./media/app-service-web-get-started/scale-out-settings.png)

4. Zmienianie **skali przez** wartość procentową **Procesora**. Suwaki poniżej listy rozwijanej zaktualizuj odpowiednio. Następnie zdefiniuj zakres **wystąpień** między **1** i **2** i **docelowej Zakres** między **40** i **80**. To zrobić przez wpisanie tekstu w polach lub przesuwając suwaki.  
 ![Możliwość skalowania - Konfigurowanie autoscaling](./media/app-service-web-get-started/scale-out-configure.png)

    W zależności od konfiguracji, aplikacji jest automatycznie skalowany się po procesora jest ponad 80% i skale w przypadku procesora poniżej 40%.

5. Kliknij przycisk **Zapisz** na pasku menu.

Gratulacje, aplikacja jest autoscaling.

Możesz zauważyć w karta **Ustawień skali** , które może wykonać wiele innych obiektów, takich jak:

- Ręczne skalowanie do określonej liczby wystąpień
- Skalowanie przez inne wskaźniki wydajności, takich jak kolejki wartość procentową lub dysk pamięci
- Dostosowywanie sposobu skalowania wyzwolenia reguły wydajności
- Autoscale zgodnie z harmonogramem
- Ustawianie zachowania autoscaling dla przyszłych zdarzenia

Aby uzyskać więcej informacji o Skalowanie wewnętrzne aplikacji zobacz [rozbudowy aplikacji platformy Azure](../app-service-web/web-sites-scale.md). Aby uzyskać więcej informacji na Skalowanie zewnętrzne zobacz [Skalowanie licznik wystąpień ręcznie lub automatycznie](../monitoring-and-diagnostics/insights-how-to-scale.md).

## <a name="receive-alerts-for-your-app"></a>Odbieranie alertów dla aplikacji

Teraz, gdy aplikacja jest autoscaling, co się dzieje po osiągnięciu wystąpienie maksymalna liczba (2) i Procesor jest powyżej odpowiedniej wykorzystania (80%)?
Możesz skonfigurować alert (więcej informacji na [odbieranie alertów](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)) informujące o takiej sytuacji aby dodatkowo można skalować Strzałka w górę/w nowym oknie aplikacji, na przykład. Szybko skonfiguruj alert dla tego scenariusza.

1. W portalu karta aplikacji, kliknij pozycję **Narzędzia** > **alertów**.  
    ![Alerty — karta Ustawienia](./media/app-service-web-get-started/alert-settings.png)

2. Kliknij przycisk **Dodaj alert**. Następnie w oknie dialogowym **zasobu** wybierz zasób, którego kończy się **(serverfarms)**. To jest plan aplikacji usługi.  
    ![Alerty — Dodawanie alert dotyczący plan usług aplikacji](./media/app-service-web-get-started/alert-add.png)

3. Określ **nazwę** jako `CPU Maxed`, **Metryka** jako **Wartość procentową Procesora**i **Próg** `90`, następnie wybierz pozycję **Właściciele poczty E-mail, uczestników i czytników**, a następnie kliknij **przycisk OK**.   
 ![Alerty — Konfigurowanie alertów](./media/app-service-web-get-started/alert-configure.png)

    Po zakończeniu tworzenia alertu Azure zobaczysz go w karta **alertów** .  
    ![Alerty — widok na koniec](./media/app-service-web-get-started/alert-done.png)

Gratulacje, teraz wyświetlany alertów.

To ustawienie alertów sprawdza procesora co pięć minut. Jeśli ten numer obejmuje ponad 90%, otrzymasz alert e-mail wraz z każdego, kto jest uprawniony. Aby wyświetlić każdego, kto jest uprawniony do otrzymywania alertów, wróć do portalu karta aplikacji, a następnie kliknij przycisk **dostępu** .  
![Zobacz, kto alertów](./media/app-service-web-get-started/alert-rbac.png)

Powinien zostać wyświetlony **Administratorzy subskrypcji** są już **właścicielem** tej aplikacji. Jeśli jesteś administratorem konta subskrypcji usługi Azure (np. subskrypcji wersji próbnej) będzie zawierać tej grupy. Aby uzyskać więcej informacji o kontrola dostępu oparta na rolach Azure zobacz [Kontrola dostępu Azure Role-Based](../active-directory/role-based-access-control-configure.md).

> [AZURE.NOTE] Reguły alertów jest funkcją Azure. Aby uzyskać więcej informacji zobacz [odbieranie alertów](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).

## <a name="next-steps"></a>Następne kroki

Rozpocząć konfigurowanie alert możesz zauważyć bogatego zestawu narzędzi w karta **Narzędzia** . W tym miejscu możesz rozwiązać problemy, monitorowanie wydajności testowanie luk, zarządzanie zasobami interakcję konsolę maszyn wirtualnych i Dodaj przydatne rozszerzenia. Zachęcamy do kliknięcia o każdym z tych narzędzi, aby odkryć narzędzia prostego i użytecznego u porady palcem.

Dowiedz się, jak wykonać inne czynności z wdrożonym aplikacji. Poniżej przedstawiono częściową listę:

- [Zakup i konfigurowanie niestandardowej nazwy domeny](custom-dns-web-site-buydomains-web-app.md) — Kup atrakcyjnego domenę dla aplikacji sieci web, zamiast *. azurewebsites.net domeny. Lub korzystać z domeny, które już posiadasz.
- [Konfigurowanie tymczasowej środowiskach](web-sites-staged-publishing.md) - wdrażanie aplikacji tymczasowy adres URL przed wprowadzeniem go do produkcji. Aktualizowanie aplikacji sieci live web sprawne. Konfigurowanie opracowywać rozwiązania DevOps z wieloma gniazdami wdrożenia.
- [Konfigurowanie rozmieszczania ciągły](app-service-continuous-deployment.md) - integrowanie wdrażaniem aplikacji źródłowego systemu kontroli. Rozmieszczanie Azure za pomocą każdej transakcji.
- [Dostęp lokalnych zasobów](web-sites-hybrid-connection-get-started.md) - dostęp istniejącego lokalnej bazy danych lub systemu CRM.
- [Kopię zapasową aplikacji](web-sites-backup.md) — Konfigurowanie kopii zapasowych i przywracania dla aplikacji sieci web. Przygotowywanie do nieoczekiwanych błędów i odzyskiwanie z nich.
- [Włączanie dzienników diagnostycznych](web-sites-enable-diagnostic-log.md) - przeczytaj dzienniki programu IIS śledzenia Azure lub aplikacji. Przeczytaj je w strumieniu, pobierz je lub port je do [Wniosków aplikacji](../application-insights/app-insights-overview.md) na potrzeby analizy Włącz klucz.
- [Skanowanie aplikacji luk](https://azure.microsoft.com/blog/web-vulnerability-scanning-for-azure-app-service-powered-by-tinfoil-security/) -
skanowanie aplikacji sieci web przed zagrożeniami nowoczesny przy użyciu usługi [Zabezpieczeń Tinfoil](https://www.tinfoilsecurity.com/).
- [Uruchamianie zadań wykonywanych w tle](../azure-functions/functions-overview.md) — Uruchom zadania przetwarzanie danych raportowania, itd.
- [Dowiedz się, jak działa usługa aplikacji](../app-service/app-service-how-works-readme.md)
