<properties
    pageTitle="Wprowadzenie do badania produkcji dla aplikacji sieci Web"
    description="Informacje na temat Test funkcji produkcji (nachylenie) w Azure aplikacji usługi sieci Web."
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="01/13/2016"
    ms.author="cephalin"/>

# <a name="get-started-with-test-in-production-for-web-apps"></a>Wprowadzenie do badania produkcji dla aplikacji sieci Web

Produkcji, albo live — testowania aplikacji sieci web przy użyciu ruchu live klientów, jest deweloperów aplikacji rosnącej złożoności integrowanie ich metodologii [adaptacyjne opracowywania](https://en.wikipedia.org/wiki/Agile_software_development) strategii test. Umożliwia testowanie jakości aplikacji z ruchem live użytkownika w środowisku produkcyjnym w przeciwieństwie syntetyzowanej danych w środowisku testowym. Umożliwiając z nowej aplikacji użytkownikom rzeczywistą, może być poinformowany na rzeczywistą problemów, które mogą się spodziewać aplikacji po wdrożeniu. Można sprawdzić funkcjonalności, działania i wartość aktualizacje aplikacji przed głośność, prędkości i różnych ruch rzeczywistego użytkownika, którego nie można określić w przybliżeniu w środowisku testowym.

## <a name="traffic-routing-in-app-service-web-apps"></a>Ruch routingu w aplikacjach sieci Web aplikacji usługi

Z funkcją routingu ruchu w [Usłudze Azure aplikacji](http://go.microsoft.com/fwlink/?LinkId=529714)bezpośredni część ruch live użytkownika do jednej lub kilku [gniazd wdrożenia](web-sites-staged-publishing.md)i następnie analizować aplikacji z [Wniosków aplikacji Azure](/services/application-insights/) lub [Usługa Azure HDInsight](/services/hdinsight/)lub narzędzia innej firmy, takich jak [Nowy Relic](/marketplace/partners/newrelic/newrelic/) do sprawdzania poprawności zmiany. Na przykład za pomocą usługi aplikacji można zaimplementować następujące scenariusze:

- Poznawanie funkcjonalności usterek lub pinpoint gardła wydajności w aktualizacje przed wdrożeniem całej witryny
- Wykonywanie "test kontroli lotów" wprowadzonych zmian za pomocą pomiaru metryki usibility w aplikacji w wersji beta
- Stopniowo rozpocząć wydajną maksymalnie nowe aktualizacje, a bezpiecznie z powrotem w dół do bieżącej wersji w przypadku wystąpienia błędu 
- Optymalizowanie wyników biznesowych z aplikacji, uruchamiając [A / B testów](https://en.wikipedia.org/wiki/A/B_testing) lub [testów wielu zmiennych](https://en.wikipedia.org/wiki/Multivariate_testing_in_marketing) w wielu gniazdach wdrażania

### <a name="requirements-for-using-traffic-routing-in-web-apps"></a>Wymagania dotyczące używania routingu ruchu w aplikacjach sieci Web

- Aplikacji sieci web należy uruchomić w warstwie **Standard** lub **Premium** , sposób jest wymagany dla wielu gniazda wdrożenia.
- Aby można było działa prawidłowo, routingu ruchu wymaga pliki cookie są włączone w przeglądarce użytkowników. Routing ruchu używa plików cookie, aby przypiąć przeglądarki klienta do przedziału wdrożenia o życia sesji klienta.
- Routing ruchu obsługuje zaawansowanych scenariuszy Porada za pomocą poleceń cmdlet środowiska PowerShell Azure.

## <a name="route-traffic-segment-to-a-deployment-slot"></a>Części ruch trasy do przedziału wdrażania

Na poziomie podstawowe w każdym scenariuszu Porada wstępnie zdefiniowanych wartości procentowej ruchu live jest rozsyłana do przedziału wdrożenia wartością produkcji. Aby to zrobić, wykonaj następujące czynności:

>[AZURE.NOTE] Opisane tu czynności zakłada już [przedział wdrożenia wartością produkcji](web-sites-staged-publishing.md) i że składnik web app ich zawartość jest już [wdrożony](web-sites-deploy.md) do niego.

1. Logowanie do [portalu Azure](https://portal.azure.com/).
2. Karta aplikacji sieci web kliknij **Ustawienia** > **Routingu ruchu**.
  ![](./media/app-service-web-test-in-production/01-traffic-routing.png)
3. Wybierz przedział, który chcesz skierować ruch do i procent sumy ruch najszybciej, następnie kliknij przycisk **Zapisz**.

    ![](./media/app-service-web-test-in-production/02-select-slot.png)

4. Przejdź do karta przedział wdrożenia. Powinien zostać wyświetlony live ruch kierowane do niego.

    ![](./media/app-service-web-test-in-production/03-traffic-routed.png)

Po skonfigurowaniu routingu ruchu określony procent klientów będą losowo kierowane do usługi przedział wartością produkcji. Jednak należy pamiętać, że po klienta automatycznie jest kierowane do określonego przedziału, go będzie można "przypięte" do tego przedział dla tej sesji klienta. To gotowe użycie plików cookie, aby przypiąć sesję użytkownika. Jeśli sprawdzanie żądania HTTP, znajdziesz `TipMix` plików cookie w wszystkie kolejne żądania.

![](./media/app-service-web-test-in-production/04-tip-cookie.png)

## <a name="force-client-requests-to-a-specific-slot"></a>Wymuszanie żądań do określonego przedziału

Oprócz routingu ruchu automatyczne, aplikacji usługi będzie mógł rozsyłanie żądań do określonego przedziału. Jest to przydatne, gdy chcesz, aby użytkownicy mogli dołączać do lub zrezygnować z aplikacji w wersji beta. W tym celu należy użyć `x-ms-routing-name` kwerenda parametryczna.

Aby przekierować użytkowników do korzystania z określonych przedział `x-ms-routing-name`, należy się upewnić, że przedział został już dodany do listy rozsyłanie ruch. Ponieważ chcesz skierować do przedział jawnie rzeczywiste routingu wartość procentową, ustawić nie ma znaczenia. Jeśli chcesz, możesz jednostki "łącze beta", które użytkownicy mogą kliknąć, aby uzyskać dostęp do aplikacji w wersji beta.

![](./media/app-service-web-test-in-production/06-enable-x-ms-routing-name.png)

### <a name="opt-users-out-of-beta-app"></a>Wybrać użytkowników aplikacji w wersji beta

Aby umożliwić użytkownikom zaprzestać korzystania z aplikacji w wersji beta, na przykład, możesz umieścić łącze na stronie sieci web:

    <a href="<webappname>.azurewebsites.net/?x-ms-routing-name=self">Go back to production app</a>

Ciąg `x-ms-routing-name=self` Określa przedział produkcji. Po przeglądarki klienta dostęp do łącza, nie tylko jest przekierowywana przedział produkcji, ale będzie zawierać wszystkie kolejne żądania `x-ms-routing-name=self` plików cookie, który połączeniowymi sesję przedział produkcji.

![](./media/app-service-web-test-in-production/05-access-production-slot.png)

### <a name="opt-users-in-to-beta-app"></a>Opcjonalna użytkownikom w aplikacji w wersji beta

Aby umożliwić użytkownikom korzystania z aplikacji w wersji beta, należy ustawić ten sam parametr kwerendy do nazwy przedział wartością produkcji na przykład:

        <webappname>.azurewebsites.net/?x-ms-routing-name=staging

## <a name="more-resources"></a>Więcej zasobów ##

-   [Konfigurowanie przemieszczenia środowiska dla aplikacji sieci web w usłudze Azure aplikacji](web-sites-staged-publishing.md)
-   [Wdrażanie złożonej aplikacji właściwie platformy Azure](app-service-deploy-complex-application-predictably.md)
-   [Adaptacyjne programowania Azure aplikacji usługi](app-service-agile-software-development.md)
-   [Efektywne używanie środowiskach DevOps dla aplikacji sieci web programu](app-service-web-staged-publishing-realworld-scenarios.md)