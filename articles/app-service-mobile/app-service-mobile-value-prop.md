<properties
    pageTitle="Co to są aplikacje Mobile"
    description="Dowiedz się, jakie korzyści aplikacji usługi powoduje do Twojej organizacji aplikacji dla urządzeń przenośnych."
    services="app-service\mobile"
    documentationCenter=""
    authors="adrianhall"
    manager="yochayk"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="na"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="getting-started"> </a>Co to jest aplikacji Mobile?

Azure aplikacji usługi jest w pełni zarządzanych [platformy jako usługa](https://azure.microsoft.com/overview/what-is-paas/) (PaaS) oferowanie dla deweloperów to bogatego zestawu funkcji sieci Web urządzeń przenośnych i scenariusze integracji. *Aplikacje Mobile* w *Usłudze Azure aplikacji* oferują platformy opracowywania aplikacji mobilnej wysoce skalowalna, ogólnie dostępna dla deweloperów przedsiębiorstwa i integratorów, aby przesunąć bogatego zestawu funkcji deweloperów urządzeń przenośnych.

![Aplikacje dla urządzeń przenośnych](./media/app-service-mobile-value-prop/overview.png)

##<a name="why-mobile-apps"></a>Dlaczego aplikacji dla urządzeń przenośnych?
*Aplikacje Mobile* w *Usłudze Azure aplikacji* oferuje platformy opracowywania aplikacji mobilnej wysoce skalowalna, ogólnie dostępna dla deweloperów przedsiębiorstwa i integratorów, aby przesunąć bogatego zestawu funkcji deweloperów urządzeń przenośnych. Za pomocą aplikacji Mobile można:

- **Tworzenie natywne i między aplikacjami platformy** - czy konstruujesz natywnych iOS, Android i Windows aplikacji lub między platformami Xamarin lub Cordova (Phonegap) aplikacje, możesz korzystać z aplikacji usługi przy użyciu natywnego SDK.
- **Nawiązywanie połączenia z systemów enterprise** — z aplikacji Mobile, można dodawać firmową logowania na w minutach i nawiązywanie połączenia z Twojej organizacji lokalnej lub w chmurze zasobów.
- **Tworzenie aplikacji zawsze gotowe do trybu offline z synchronizacji danych** — nawiązywanie pracowników urządzeń przenośnych wydajność przez tworzenie aplikacji działające w trybie offline i za pomocą aplikacji Mobile dane synchronizacji w tle podczas łączności znajduje się z dowolnym źródła danych przedsiębiorstwa lub interfejsów API władz akredytacji bezpieczeństwa.
- **Powiadomienia wypychane miliony w sekundach** - prowadzenie klientów dzięki powiadomienia wypychane błyskawiczne na dowolnym urządzeniu spersonalizowanego do swoich potrzeb, wysyłane czas po prawej stronie.

## <a name="mobile-app-features"></a>Funkcje aplikacji dla urządzeń przenośnych
Ważne opracowywane przenośnych obsługiwanych w chmurze są następujące funkcje:

- **Uwierzytelnianie i autoryzacja** — wybierz z listy stale rosnącej dostawców tożsamości, w tym usługi Azure Active Directory dla uwierzytelniania przedsiębiorstwa oraz dostawców społecznościowych, takich jak Facebook, Google, Twitter i Account Microsoft.  Azure aplikacji Mobile udostępnia usługę OAuth 2.0 dla każdego dostawcy.  Można również zintegrować SDK dla dostawcy tożsamości dla określonych funkcji dostawcy.

  Dowiedz się więcej na temat naszych [Funkcje uwierzytelniania].

- **Dostęp do danych** — aplikacje Mobile Azure oferuje przyjazne mobile OData v3 źródła danych połączone z lokalnego programu SQL Server lub SQL Azure.  Ta usługa mogą być oparte na Framework jednostki, co umożliwia łatwe Integracja z innymi NoSQL i dostawców danych SQL, w tym dostawców [Magazyn tabel platformy Azure], MongoDB, [DocumentDB] i interfejsu API władz akredytacji bezpieczeństwa, takich jak usługi Office 365 i Salesforce.com.
- **Synchronizacja w trybie offline** — nasz SDK klienta Udostępnij zestaw go łatwo tworzyć niezawodne i odpowiada aplikacji dla urządzeń przenośnych pracujące z danych w trybie offline mogą być automatycznie synchronizowane z danymi wewnętrznej bazy danych, w tym pomoc techniczną dotyczącą rozwiązywania konfliktów.

  Dowiedz się więcej na temat naszych [funkcji danych].

- **Powiadomienia wypychane** - nasz SDK klienta Bezproblemowa integracja z funkcjami rejestracji biegami powiadomienie Azure, można wysłać powiadomień wypychanych miliony użytkowników jednocześnie.

  Dowiedz się więcej na temat naszych [wypychanych powiadomień w sieci].

- **SDK klienta** — firma Microsoft udostępnia pełny zestaw SDK klienta, które obejmują natywnych rozwoju ([iOS], [Android] i [systemu Windows]), wiele platform ([Xamarin dla systemów iOS i Android], [Formularze Xamarin]) i wdrożenie hybrydowe ([Apache Cordova]).  Każdy komputer kliencki SDK jest dostępna z licencją MIT i Otwórz źródło.

## <a name="azure-app-service-features"></a>Funkcje Azure aplikacji usługi.
Poniższe funkcje platformy są zwykle przydatne dla urządzeń przenośnych produkcji witryn.

- **Automatyczne skalowanie** - aplikacji usługi umożliwia szybkie skali na ekranie lub out obsługiwać wszystkie przychodzące obciążenia klienta. Ręcznie wybierz liczbę i rozmiar maszyny wirtualne lub skonfigurować automatyczne skalowanie skalowanie do wewnętrznej bazy danych aplikacji dla urządzeń przenośnych na podstawie obciążenia lub harmonogramu.

  Dowiedz się więcej na temat [Automatyczne skalowanie].

- **Przemieszczenia środowiskach** - aplikacji usługi można uruchamiać wielu wersji witryny, co umożliwi wykonywanie A / B testowania, testowanie produkcji jako część większego planu DevOps i w miejscu tymczasowym nowej wewnętrznej bazy danych.

  Dowiedz się więcej na temat [środowiska wzorcowego].

- **Ciągły wdrożenia** — aplikacji usługi można zintegrować z typowych systemów Menedżer sterowania usługami, co umożliwia automatyczne wdrażanie nową wersję do wewnętrznej bazy danych przez naciśnięcie gałąź systemu Menedżer sterowania usługami.

  Dowiedz się więcej na temat [opcji wdrażania].

- **Wirtualna sieć** - aplikacji usługi można nawiązać zasobów lokalnych za pomocą połączenia wirtualnej sieci, ExpressRoute lub hybrydowych.

  Dowiedz się więcej o [hybrydowych połączenia], [wirtualnych sieci]i [ExpressRoute].

- **Odizolowanych / dedykowane środowiskach** -aplikacji usługi mogą być uruchamiane w pełni odizolowanych i dedykowane środowiska dla bezpiecznego uruchamiania aplikacji Azure aplikacji usługi w większej skali wysoki.  Jest to idealne rozwiązanie w przypadku obciążenia aplikacji wymagających Skala bardzo wysoki, izolacji lub bezpieczny dostęp sieciowy.

  Dowiedz się więcej na temat [Środowiska usługi aplikacji].

## <a name="getting-started"></a>Wprowadzenie ##
Aby rozpocząć pracę z aplikacji Mobile, wykonaj samouczek [Wprowadzenie] .  Obejmuje to podstawy produkcji urządzeń przenośnych wewnętrznej bazy danych i klienta wybranych przez użytkownika, a następnie integracji uwierzytelniania, synchronizacja w trybie offline i powiadomienia wypychane.  Możesz wykonać samouczek [Wprowadzenie] kilka razy — raz dla każdej aplikacji klienta.

Więcej informacji dotyczących aplikacji Mobile Azure można znaleźć naszych [nauki mapy].
Aby uzyskać więcej informacji na platformie Azure aplikacji usługi zobacz [Usługa Azure aplikacji].

>[AZURE.NOTE]Jeśli chcesz rozpocząć pracę z Azure aplikacji usługi przed utworzeniem konta dla konta Azure, przejdź do [Spróbuj aplikacji usługi](https://tryappservice.azure.com/?appServiceName=mobile), którym natychmiast można utworzyć aplikację sieci web krótkotrwałe starter w aplikacji usługi. Nie kart kredytowych wymagane; nie zobowiązania.

<!-- URLs. -->
[Migrate your Mobile Service to App Service]: app-service-mobile-migrating-from-mobile-services.md
[Azure aplikacji usługi]: ../app-service/app-service-value-prop-what-is.md
[Rozpoczynanie pracy]: app-service-mobile-ios-get-started.md
[Magazyn tabel platformy Azure]: ../storage/storage-getting-started-guide.md
[DocumentDB]: ../documentdb/documentdb-get-started.md
[funkcje uwierzytelniania]: ./app-service-mobile-auth.md
[Funkcje danych]: ./app-service-mobile-offline-data-sync.md
[funkcje powiadomień wypychanych]: ../notification-hubs/notification-hubs-push-notification-overview.md
[iOS]: ./app-service-mobile-ios-how-to-use-client-library.md
[Android]: ./app-service-mobile-android-how-to-use-client-library.md
[Systemu Windows]: ./app-service-mobile-dotnet-how-to-use-client-library.md
[Xamarin dla systemów iOS i Android]: ./app-service-mobile-dotnet-how-to-use-client-library.md
[Formularze Xamarin]: ./app-service-mobile-xamarin-forms-get-started.md
[Apache Cordova]: ./app-service-mobile-cordova-how-to-use-client-library.md
[Automatyczne skalowanie]: ../app-service-web/web-sites-scale.md
[środowiska wzorcowego]: ../app-service-web/web-sites-staged-publishing.md
[Opcje wdrażania]: ../app-service-web/web-sites-deploy.md
[hybrydowe połączenia]: ../app-service-web/web-sites-hybrid-connection-get-started.md
[wirtualnych sieci]: ../app-service-web/web-sites-integrate-with-vnet.md
[ExpressRoute]: ../app-service-web/app-service-app-service-environment-network-configuration-expressroute.md
[Środowiskach aplikacji usługi]: ../app-service-web/app-service-app-service-environment-intro.md
[Nauka mapy]: https://azure.microsoft.com/en-us/documentation/learning-paths/appservice-mobileapps/
