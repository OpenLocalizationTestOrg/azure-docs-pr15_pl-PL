<properties 
    pageTitle="Zarządzanie aplikacji sieci web w usłudze Azure aplikacji" 
    description="Łącza do zasobów dotyczących zarządzania aplikacji sieci web w usłudze Azure aplikacji." 
    services="app-service\web" 
    documentationCenter="" 
    authors="erikre" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/24/2016" 
    ms.author="rachelap"/>

# <a name="manage-a-web-app-in-azure-app-service"></a>Zarządzanie aplikacji sieci web w usłudze Azure aplikacji

Ten temat zawiera łącza do zasobów dotyczących zarządzania aplikacji sieci web w [Usłudze Azure aplikacji](http://go.microsoft.com/fwlink/?LinkId=529714). Zarządzanie obejmuje wszystkie zadania, które zachowania aplikacji sieci web sprawne. 

W okresie istnienia aplikacji sieci web będzie wykonywać zadania zarządzania różnych, podczas przenoszenia z początkowej wdrożenia do normalnego działania, konserwacji i aktualizacji.

Wiele zadań zarządzania aplikacji sieci web mogą być wykonywane w Azure Portal.

## <a name="before-you-deploy-your-web-app-to-production"></a>Przed wdrożeniem aplikacji sieci web produkcji

### <a name="choose-a-tier"></a>Wybieranie poziomu

Usługa Azure aplikacji jest oferowane w pięciu poziomów: wolny, udostępnione, Basic, Standard i Premium. Aby uzyskać informacje o funkcjach i cenach dla każdej warstwy zobacz [ceny szczegóły](/pricing/details/app-service/). 

- [Plany aplikacji usługi](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) umożliwiają grupowanie wielu aplikacji sieci web w tej samej warstwie.
- Zawsze możesz [przełączyć poziomów](web-sites-scale.md) po utworzeniu aplikacji sieci web.

### <a name="configuration"></a>Konfiguracja

[Azure Portal](https://portal.azure.com/) umożliwia ustawienie różnych opcji konfiguracji. Aby uzyskać szczegółowe informacje zobacz [Konfigurowanie aplikacji sieci web w usłudze Azure w aplikacji](web-sites-configure.md). Oto szybki listy kontrolnej:

- Wybierz **środowisko uruchomieniowe wersji** .NET, PHP, Java lub Python, w razie potrzeby.
- Włączenie **WebSockets** aplikacji sieci web korzysta z protokołu WebSocket. (Zawiera aplikacje korzystające z [Programu ASP.NET SignalR](http://www.asp.net/signalr) lub [socket.io](web-sites-nodejs-chat-app-socketio.md)).
- Używasz zadania ciągły sieci web? Jeśli tak, włączyć **Zawsze**.
- Ustawianie **domyślnego dokumentu**, takie jak index.html.

Oprócz tych ustawień konfiguracji podstawowej można skonfigurować następujące ustawienia:

- Szyfrowanie **Secure Sockets Layer (SSL)** . Używanie protokołu SSL z niestandardowej nazwy domeny, możesz uzyskać certyfikat SSL i konfigurowanie aplikacji sieci web, aby go używać. Zobacz [Włączanie HTTPS dla aplikacji sieci web w usłudze Azure aplikacji](web-sites-configure-ssl-certificate.md).
- **Niestandardowej nazwy domeny.** Aplikacji sieci web automatycznie otrzymuje poddomeny w obszarze azurewebsites.net. Możesz skojarzyć nazwę domeny niestandardowej, na przykład contoso.com. Zobacz [Konfigurowanie niestandardowej nazwy domeny w usłudze Azure aplikacji](web-sites-custom-domain-name.md).

Konfiguracja specyficzne dla języka:

- **PHP**: [Konfigurowanie PHP w aplikacjach sieci Web Azure aplikacji usługi](web-sites-php-configure.md).
- **Python**: [Konfigurowanie Python z aplikacjami Web Azure aplikacji usługi](web-sites-python-configure.md)


## <a name="while-your-web-app-is-running"></a>Po uruchomieniu aplikacji sieci web

Po uruchomieniu aplikacji sieci web, który chcesz upewnij się, jest dostępny i że może spełniają ruch użytkownika. Ponadto może być konieczne rozwiązywanie problemów z błędami.

### <a name="monitoring"></a>Monitorowanie

- Za pośrednictwem portalu Azure możesz [dodać wskaźniki](web-sites-monitor.md) , takie jak użycie Procesora i liczba żądania klienta.
- [Skala aplikacji sieci web](web-sites-scale.md) w odpowiedzi na ruch. W zależności od usługi warstwy można skalować liczbę maszyny wirtualne i/lub rozmiar wystąpienia maszyn wirtualnych. W poziomów Standard i Premium możesz również skonfigurować autoscaling, więc aplikacji sieci web skale automatycznie, zgodnie z harmonogramem stały lub w odpowiedzi na ładowanie.  
 
### <a name="backups"></a>Wykonywanie kopii zapasowych

- Ustawianie [automatycznych kopii zapasowych](web-sites-backup.md) aplikacji sieci web. Dowiedz się więcej na temat wykonywania kopii zapasowych w [tym klipie wideo](https://azure.microsoft.com/documentation/videos/azure-websites-automatic-and-easy-backup/).
- Więcej informacji na temat opcji [odzyskiwanie bazy danych](../sql-database/sql-database-business-continuity.md) w bazie danych SQL Azure.

### <a name="troubleshooting"></a>Rozwiązywanie problemów

- Jeśli wystąpią problemy, możesz to zrobić, [Rozwiązywanie problemów w programie Visual Studio](web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug), za pomocą dzienników diagnostycznych i live debugowania w chmurze. 
- Poza Visual Studio istnieją różne sposoby zbierz dzienniki diagnostyczne. Zobacz [Włączanie diagnostyki rejestrowania aplikacji sieci web w usłudze Azure aplikacji](web-sites-enable-diagnostic-log.md).
- W przypadku aplikacji Node.js Zobacz, [jak debugowania Node.js aplikacji web app w usłudze Azure aplikacji](web-sites-nodejs-debug.md).

### <a name="restoring-data"></a>Przywracanie danych

- [Przywracanie](web-sites-restore.md) aplikacji sieci web, który został wcześniej kopie zapasowe.


## <a name="when-you-update-your-web-app"></a>Po zaktualizowaniu aplikacji sieci web

Jeśli nie zostało włączone automatyczne wykonywanie kopii zapasowych, możesz utworzyć [ręczną kopię zapasową](web-sites-backup.md).

Warto rozważyć użycie [umieszczane wdrożenia](web-sites-staged-publishing.md). Ta opcja umożliwia publikowanie aktualizacji wdrożeniu tymczasowy uruchamianej obok siebie z wdrożeniem produkcji. 

Jeśli używasz programu Visual Studio Team Services, możesz skonfigurować wdrożenie ciągły z kontrolki źródła:

- [Za pomocą kontroli wersji programu Team Foundation (TFVC)](../cloud-services/cloud-services-continuous-delivery-use-vso.md) 
- [Za pomocą cyfra](../cloud-services/cloud-services-continuous-delivery-use-vso-git.md)
 
<!-- Anchors. -->

[Before you deploy your site to production]: #before-you-deploy-your-site-to-production
[While your website is running]: #while-your-website-is-running
[When you update your website]: #when-you-update-your-website

  
