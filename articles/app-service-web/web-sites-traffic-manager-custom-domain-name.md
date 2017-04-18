<properties
    pageTitle="Skonfiguruj nazwę domeny niestandardowej aplikacji sieci web w usłudze aplikacji Azure w korzystającego Menedżer ruchu do równoważenia obciążenia."
    description="Używanie niestandardowej nazwy domeny dla aplikacji sieci web usługi aplikacji Azure, który zawiera Menedżera ruch do równoważenia obciążenia."
    services="app-service\web"
    documentationCenter=""
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="robmcm"/>

# <a name="configuring-a-custom-domain-name-for-a-web-app-in-azure-app-service-using-traffic-manager"></a>Konfigurowanie niestandardowej nazwy domeny dla aplikacji sieci web w usłudze aplikacji Azure za pomocą Menedżera ruchu

[AZURE.INCLUDE [web-selector](../../includes/websites-custom-domain-selector.md)]

[AZURE.INCLUDE [intro](../../includes/custom-dns-web-site-intro-traffic-manager.md)]

Ten artykuł zawiera ogólne instrukcje dotyczące przy użyciu niestandardowej nazwy domeny w usłudze Azure aplikacji, używane w Menedżer ruchu równoważenia obciążenia.

[AZURE.INCLUDE [tmwebsitefooter](../../includes/custom-dns-web-site-traffic-manager-notes.md)]

[AZURE.INCLUDE [introfooter](../../includes/custom-dns-web-site-intro-notes.md)]

<a name="understanding-records"></a>
## <a name="understanding-dns-records"></a>Opis rekordy DNS

[AZURE.INCLUDE [understandingdns](../../includes/custom-dns-web-site-understanding-dns-traffic-manager.md)]

<a name="bkmk_configsharedmode"></a>
## <a name="configure-your-web-apps-for-standard-mode"></a>Konfigurowanie aplikacji sieci web do trybu standardowego

[AZURE.INCLUDE [modes](../../includes/custom-dns-web-site-modes-traffic-manager.md)]

<a name="bkmk_configurecname"></a>
## <a name="add-a-dns-record-for-your-custom-domain"></a>Dodawanie rekordu DNS dla domeny niestandardowej

> [AZURE.NOTE] Jeśli zakupiono domeny za pośrednictwem sieci Web usługi aplikacji Azure pominąć, wykonując kroki i odwołują się do ostatniego kroku artykułu [Kup domenę dla aplikacji sieci Web](custom-dns-web-site-buydomains-web-app.md) .

Aby skojarzyć własnej domeny z aplikacji sieci web w usłudze Azure aplikacji, należy dodać nowy wpis w tabeli DNS dla domeny niestandardowej za pomocą narzędzi dostarczony przez rejestratora domen, który został zakupiony nazwy domeny z. Wykonaj następujące czynności, aby zlokalizować i za pomocą narzędzia systemu DNS.

1. Zaloguj się do swojego konta u rejestratora domeny i sprawdź stronę służącą do zarządzania rekordami DNS. Poszukaj łącza lub obszarów witryny oznaczony jako **Nazwę domeny**, **DNS**lub **Zarządzanie serwerem nazw**. Często łącza na tej stronie można znaleźć wyświetlanie informacji o koncie, a następnie szukasz łącza, takie jak **moich domen**.

1. Po znalezieniu stronę Zarządzanie dla nazwy domeny, poszukaj łącza, które można edytować rekordy DNS. To może znajdować się w **pliku strefy**, **Rekordy DNS**, lub jako łącze Konfiguracja **Zaawansowane** .

    * Strona najprawdopodobniej będzie zawierać kilka rekordów już utworzony, takich jak kojarzenie wpis "**@**"lub"\*" strony "parkowania domeny". Może również zawierać rekordy dla typowych domeny podrzędne, takie jak **"www"**.
    * Strony wspomnieć **rekordy CNAME**lub podaj listy rozwijanej, aby wybrać typ rekordu. Może też Dodawanie wzmianki inne rekordy, takie jak **rekordów** i **rekordów MX**. W niektórych przypadkach rekordy CNAME zostanie wywołana przez inne nazwy, takie jak **Rekord aliasu**.
    * Strony będzie miał pola, które umożliwiają **mapy** w **polu Nazwa hosta** lub **nazwy domeny** na inną nazwę domeny.

1. Gdy różnią się szczegółowe informacje na temat poszczególnych rejestratora, ogólnie można mapować *z* niestandardowej nazwy domeny (na przykład **contoso.com**) *do* Menedżera ruch nazwę domeny (**contoso.trafficmanager.net**) używaną dla aplikacji sieci web.

    > [AZURE.NOTE] Jeśli rekord jest już używana, należy powiązać preemptively aplikacji do niego, można również utworzyć dodatkowy rekord CNAME. Na przykład powiązać preemptively **www.contoso.com** aplikacji sieci web, Utwórz rekord CNAME z **awverify.www** do **contoso.trafficmanager.net**. Następnie można dodać "www.contoso.com" do aplikacji sieci Web bez zmieniania rekord CNAME "www". Aby uzyskać więcej informacji, zobacz [Tworzenie rekordów DNS dla aplikacji sieci web w domenie niestandardowej][CREATEDNS].

1. Po zakończeniu dodawania lub modyfikowanie rekordów DNS u rejestratora domen, Zapisz zmiany.

<a name="enabledomain"></a>
## <a name="enable-traffic-manager"></a>Menedżer ruchu

[AZURE.INCLUDE [modes](../../includes/custom-dns-web-site-enable-on-traffic-manager.md)]

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji zobacz [Centrum deweloperów Node.js](/develop/nodejs/).

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[CREATEDNS]: ../dns/dns-web-sites-custom-domain.md
