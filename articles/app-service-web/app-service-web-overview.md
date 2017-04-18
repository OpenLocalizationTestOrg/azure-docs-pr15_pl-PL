<properties
    pageTitle="Omówienie aplikacji Web | Microsoft Azure"
    description="Dowiedz się, jak usługa Azure aplikacji pomaga opracowywać i obsługi aplikacji sieci web"
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/28/2016"
    ms.author="cephalin"/>

# <a name="web-apps-overview"></a>Omówienie aplikacji sieci Web

*Aplikacji sieci Web usługi* jest platformą obliczeń w pełni zarządzane, zoptymalizowanej pod kątem hostingu aplikacje sieci web i witryn sieci Web. Ta oferta [platformy jako usługi](https://en.wikipedia.org/wiki/Platform_as_a_service) (PaaS): umożliwia Microsoft Azure umożliwia skoncentrowanie się na logiki biznesowej podczas Azure trwa istotnych infrastruktury, aby uruchomić i skalowanie aplikacji.

Następujące 5-minutowym klipie wideo przedstawiono Azure aplikacji usługi sieci Web.

[AZURE.VIDEO azure-app-service-web-apps-with-yochay-kiriaty]

>[AZURE.INCLUDE [app-service-linux](../../includes/app-service-linux.md)]

## <a name="what-is-a-web-app-in-app-service"></a>Co to jest aplikacji sieci web w aplikacji usługi?

W aplikacji usługi dla *aplikacji sieci web* jest zasoby obliczeń, które Azure przewiduje hostingu witryny sieci Web lub aplikacji sieci web.  

Zasoby obliczeń może być udostępnionych lub dedykowane wirtualnych maszyn, w zależności od cennik warstwie, którą należy wybrać. Kod aplikacji jest uruchamiany w zarządzanych maszyn wirtualnych, który się odizolowane od innych klientów.

Kod może być w każdym języku lub struktury, która jest obsługiwana przez [Usługę Azure aplikacji](../app-service/app-service-value-prop-what-is.md), takiej jak ASP.NET, Node.js, Java, PHP lub Python. Można również uruchomić skrypty za pomocą [programu PowerShell i inne języki skryptów](web-sites-create-web-jobs.md#acceptablefiles) w aplikacji sieci web.

Przykłady aplikacji typowych scenariuszy, które możesz za pomocą aplikacji sieci Web dla można znaleźć w sekcji **scenariusze i zalecenia** [Azure aplikacji usługi](choose-web-site-cloud-service-vm.md#scenarios), porównanie maszyn wirtualnych, usług i usług w chmurze i [scenariusze aplikacji sieci Web](https://azure.microsoft.com/documentation/scenarios/web-app/) .

## <a name="why-use-web-apps"></a>Dlaczego warto używać aplikacji sieci Web?

Poniżej przedstawiono niektóre kluczowe funkcje aplikacji usługi, których dotyczy aplikacji sieci Web:

- **Wielu języków i struktury** - aplikacji usługi obsługuje najlepszych dla programu ASP.NET, Node.js Java, PHP i Python. Można również uruchomić [programu PowerShell i innych skryptów lub plików wykonywalnych](../app-service-web/web-sites-create-web-jobs.md) na pośrednictwem aplikacji usługi SMS.

- **Optymalizacja DevOps** — Konfigurowanie [ciągły integracji i wdrażania](../app-service-web/app-service-continuous-deployment.md) za pomocą programu Visual Studio Team Services, GitHub lub BitBucket. Promowanie aktualizacji za pośrednictwem [test i przygotowania środowiska](../app-service-web/web-sites-staged-publishing.md). Wykonywanie [A / testowanie B](../app-service-web/app-service-web-test-in-production-get-start.md). Zarządzanie aplikacji w aplikacji usługi przy użyciu [Azure programu PowerShell](../powershell-install-configure.md) lub [interfejsu wiersza polecenia i platform (polecenie)](../xplat-cli-install.md).

- **Globalne skalowanie wysoką dostępność** - skala w [górę](../app-service-web/web-sites-scale.md) lub na [zewnątrz](../monitoring-and-diagnostics/insights-how-to-scale.md) ręcznie lub automatycznie. Obsługiwać aplikacji dowolne miejsce w infrastrukturze globalnego centrum danych firmy Microsoft, a aplikacji usługi [SLA](https://azure.microsoft.com/support/legal/sla/app-service/) projektuje wysokiej dostępności.

- **Połączenia z platformy władz akredytacji bezpieczeństwa i danych lokalnych** — wybierz z więcej niż 50 [Łączniki](../connectors/apis-list.md) w przypadku systemów przedsiębiorstwa (na przykład SAP, Siebel i Oracle), władz akredytacji bezpieczeństwa (na przykład usług Salesforce i usługi Office 365) i internet usług (takich jak Facebook i Twitter). Dostęp lokalnych danych przy użyciu [Połączenia hybrydowych](../biztalk-services/integration-hybrid-connection-overview.md) i [Azure wirtualnych sieci](../app-service-web/web-sites-integrate-with-vnet.md).

- **Zabezpieczenia i zgodność** - aplikacji usługi jest [ISO, SOC i PCI zgodności](https://www.microsoft.com/TrustCenter/).

- **Szablony aplikacji** - wybierz z listy rozległa szablonów aplikacji [Usługi Azure Marketplace](https://azure.microsoft.com/marketplace/) , które umożliwiają instalowanie popularnych Otwórz źródło, takich jak WordPress, Joomla i Drupal za pomocą kreatora.

- **Integracja z programem visual Studio** - dedykowane narzędzia w programie Visual Studio usprawnić pracę tworzenia, wdrażania i debugowania.

Ponadto aplikacji sieci web można korzystać z funkcji oferowanych przez [Aplikacje interfejsu API](../app-service-api/app-service-api-apps-why-best-platform.md) (takie jak CORS obsługuje) i [Aplikacje Mobile](../app-service-mobile/app-service-mobile-value-prop.md) (takie jak powiadomienia wypychane). Aby uzyskać więcej informacji na temat typów aplikacji w aplikacji usługi zobacz [Omówienie Azure aplikacji usługi](../app-service/app-service-value-prop-what-is.md).

Oprócz aplikacji Web Apps w aplikacji usługi Azure oferuje innych usług, które mogą być używane do obsługi aplikacji sieci web i witryn sieci Web. W przypadku większości scenariuszy aplikacji sieci Web jest to najlepszy wybór.  Architektura microservice należy rozważyć, czy [Usługi tkaninie](https://azure.microsoft.com/documentation/services/service-fabric)i jeśli potrzebujesz mieć większą kontrolę nad maszyny wirtualne, które kodu działa na należy rozważyć, czy [maszyn wirtualnych Azure](https://azure.microsoft.com/documentation/services/virtual-machines/). Aby uzyskać więcej informacji na temat wybrać jedną z tych usług Azure zobacz [Azure aplikacji usługi, maszyn wirtualnych, tkaninie usługi i porównanie usług w chmurze](choose-web-site-cloud-service-vm.md).

## <a name="getting-started"></a>Wprowadzenie

Aby rozpocząć pracę, wdrażając przykładowy kod do nowej aplikacji sieci web w aplikacji usługi, wykonaj samouczek [Wdrażanie aplikacji sieci web pierwszego Azure za 5 minut](app-service-web-get-started.md) . Konieczne będzie bezpłatne konto Azure.

Jeśli chcesz rozpocząć pracę z Azure aplikacji usługi przed utworzeniem konta dla konta Azure, przejdź do [Spróbuj aplikacji usługi](http://go.microsoft.com/fwlink/?LinkId=523751), którym natychmiast można utworzyć aplikację sieci web krótkotrwałe starter w aplikacji usługi. Nie kart kredytowych wymagane; nie zobowiązania.
