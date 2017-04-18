<properties
    pageTitle="Azure aplikacji usługi sieci web, mobile i aplikacje interfejsu API | Microsoft Azure"
    description="Dowiedz się, jak usługa Azure aplikacji ułatwia opracowywanie, wdrażanie i zarządzanie nią sieci web i aplikacji dla urządzeń przenośnych."
    keywords="aplikacji usługi azure aplikacji usługi, aplikacji usługi kosztów, Skaluj skalowalna, wdrażaniem aplikacji, wdrażaniem aplikacji azure, paas, platformy jako usługi, witryny sieci Web, witryny sieci web, sieci web, azure urządzeń przenośnych"
    services="app-service"
    documentationCenter=""
    authors="omarkmsft"
    manager="erikre"
    editor="cephalin"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/26/2016"
    ms.author="omark"/>

# <a name="what-is-azure-app-service"></a>Co to jest usługa Azure aplikacji?

*Aplikacja* jest [platformy jako usługi](https://en.wikipedia.org/wiki/Platform_as_a_service) (PaaS) oferowanie pakietu Microsoft Azure. Tworzenie witryny sieci web i aplikacji dla urządzeń przenośnych dla dowolnej platformie lub urządzeniu. Integracja aplikacji z rozwiązań władz akredytacji bezpieczeństwa, łączenie się z aplikacji lokalnej i automatyzowanie procesów biznesowych. Azure działa aplikacji w pełni zarządzany wirtualnych maszyn, z wybraną udostępnionych maszyn wirtualnych lub dedykowane maszyny wirtualne.

Usługa aplikacji obejmuje sieci web i urządzeń przenośnych możliwości, które było wcześniej dostarczone oddzielnie jako Azure witryn sieci Web i usługi Azure Mobile. Zawiera również nowe funkcje dla hostingu w chmurze interfejsy API i automatyzowanie procesów biznesowych. Jako pojedyncze usługi zintegrowane aplikacji usługi pozwala utworzyć różne składniki — witryn sieci Web, końce tylnego aplikacji dla urządzeń przenośnych, API RESTful i procesów biznesowych — w obrębie pojedynczego rozwiązania.

W następujących 4-minutowym klipie wideo przedstawiono krótki opis jak aplikacji usługi odnoszą się do wcześniejszej Azure ofert i co nowego w nim.

+[AZURE.VIDEO app-service-history-lesson]

## <a name="why-use-app-service"></a>Dlaczego warto używać aplikacji usługi?

Poniżej przedstawiono niektóre kluczowe funkcje i możliwości aplikacji usługi:

- **Wielu języków i struktury** - aplikacji usługi obsługuje najlepszych dla programu ASP.NET, Node.js Java, PHP i Python. Można również uruchomić [programu Windows PowerShell i innych skryptów lub plików wykonywalnych](../app-service-web/web-sites-create-web-jobs.md) na pośrednictwem aplikacji usługi SMS.

- **Optymalizacja DevOps** — Konfigurowanie [ciągły integracji i wdrażania](../app-service-web/app-service-continuous-deployment.md) za pomocą programu Visual Studio Team Services, GitHub lub BitBucket. Promowanie aktualizacji za pośrednictwem [test i przygotowania środowiska](../app-service-web/web-sites-staged-publishing.md). Wykonywanie [A / testowanie B](../app-service-web/app-service-web-test-in-production-get-start.md). Zarządzanie aplikacji w aplikacji usługi przy użyciu [Azure programu PowerShell](../powershell-install-configure.md) lub [interfejsu wiersza polecenia i platform (polecenie)](../xplat-cli-install.md).

- **Globalne skalowanie wysoką dostępność** - skala w [górę](../app-service-web/web-sites-scale.md) lub na [zewnątrz](../monitoring-and-diagnostics/insights-how-to-scale.md) ręcznie lub automatycznie. Obsługiwać aplikacji dowolne miejsce w infrastrukturze globalnego centrum danych firmy Microsoft, a aplikacji usługi [SLA](https://azure.microsoft.com/support/legal/sla/app-service/) projektuje wysokiej dostępności.

- **Połączenia z platformy władz akredytacji bezpieczeństwa i danych lokalnych** — wybierz z więcej niż 50 [Łączniki](../connectors/apis-list.md) w przypadku systemów przedsiębiorstwa (na przykład SAP, Siebel i Oracle), władz akredytacji bezpieczeństwa (na przykład usług Salesforce i usługi Office 365) i internet usług (takich jak Facebook i Twitter). Dostęp lokalnych danych przy użyciu [Połączenia hybrydowych](../biztalk-services/integration-hybrid-connection-overview.md) i [Azure wirtualnych sieci](../app-service-web/web-sites-integrate-with-vnet.md).

- **Zabezpieczenia i zgodność** - aplikacji usługi jest [ISO, SOC i PCI zgodności](https://www.microsoft.com/TrustCenter/).

- **Szablony aplikacji** - wybierz z listy rozległa szablonów aplikacji [Usługi Azure Marketplace](https://azure.microsoft.com/marketplace/) , które umożliwiają instalowanie popularnych Otwórz źródło, takich jak WordPress, Joomla i Drupal za pomocą kreatora.

- **Integracja z programem visual Studio** - dedykowane narzędzia w programie Visual Studio usprawnić pracę tworzenia, wdrażania i debugowania.

## <a name="app-types-in-app-service"></a>Typy aplikacji w aplikacji usługi

Usługa aplikacji oferuje kilka *typów aplikacji*, z których każdy jest przeznaczona do obsługi określonego rodzaju obciążenie pracą:

- [**Aplikacje sieci web**](../app-service-web/app-service-web-overview.md) — do obsługi aplikacji sieci web i witryn sieci Web.

- [**Aplikacje dla urządzeń przenośnych**](../app-service-mobile/app-service-mobile-value-prop.md) Do obsługi aplikacji dla urządzeń przenośnych z powrotem zostanie zakończone.

- [**Interfejs API aplikacje**](../app-service-api/app-service-api-apps-why-best-platform.md) - dla hostingu API RESTful.

- [**Logika aplikacje**](../app-service-logic/app-service-logic-what-are-logic-apps.md) - automatyzowanie procesów biznesowych i integracji systemów i danych przez chmury bez pisania kodu.

Word *aplikacji* tutaj odwołuje się do zasobów hostingu pracuje nad systemem obciążenie pracą. Sporządzanie "aplikacji web app" jako przykład Użytkownicy przyzwyczajeni prawdopodobnie do Myślę o aplikacji sieci web jako zasoby obliczeń i kod aplikacji, którego razem przeprowadzania funkcji w przeglądarce. Ale w aplikacji usługi dla *aplikacji sieci web* jest zasoby obliczeń, które Azure przewiduje hostingu kodzie aplikacji. Jeśli składa się z aplikacji sieci Web frontonu i RESTful interfejsu API wewnętrzna baza danych, może umieszczaniu do aplikacji sieci web lub kodzie frontonu może wdrażanie aplikacji sieci web i kodzie wewnętrznej do aplikacji interfejsu API. Aplikacja może się składać z wielu aplikacji aplikacji usługi różnego rodzaju.

## <a name="app-service-plans"></a>Plany aplikacji usługi

[Plany usługi aplikacji](azure-web-sites-web-hosting-plans-in-depth-overview.md) określić rodzaj zasoby obliczeń, które uruchomić aplikacji. Jeśli obciążenia małym ruchu, można użyć udostępnionego maszyny wirtualne (**wolny** i **udostępnione** ceny poziomów). W przypadku wyższa obciążeń możesz wybrać kilka rozmiarów dedykowane maszyny wirtualne (poziomy**podstawowe**, **Standard**i **Premium** ). Wiele aplikacji usługi aplikacji można udostępniać tego samego planu, a ich skalowanie cennik poziomów w górę lub w dół razem w planie.

Jeśli potrzebujesz więcej skalowalność i izolacji sieci można uruchamiać aplikacji w [Środowisku usługi aplikacji](../app-service-web/app-service-app-service-environment-intro.md).

## <a name="pricing"></a>Cennik

Aby dowiedzieć się, ile koszty aplikacji usługi zobacz [Cennik usługi aplikacji](https://azure.microsoft.com/pricing/details/app-service/).

## <a name="test-drive-app-service"></a>Testowanie aplikacji usługi

[Tworzenie aplikacji sieci web próbki, aplikacji dla urządzeń przenośnych lub logiczny aplikacji](http://go.microsoft.com/fwlink/?LinkId=523751) i odtwarzanie z nim 1 godzina z karty kredytowej wymagane, nie zobowiązań bez kłopotów.

[Bezpłatne konto Azure](https://azure.microsoft.com/pricing/free-trial/)i wypróbuj jedną z naszych samouczki — wprowadzenie:

* [Samouczek: Tworzenie aplikacji sieci web](../app-service-web/app-service-web-get-started.md)
* [Samouczek: Tworzenie aplikacji dla urządzeń przenośnych](../app-service-mobile/app-service-mobile-android-get-started.md)
* [Samouczek: Tworzenie aplikacji interfejsu API](../app-service-api/app-service-api-dotnet-get-started.md)
* [Samouczek: Tworzenie aplikacji warunków logicznych](../app-service-logic/app-service-logic-create-a-logic-app.md)
