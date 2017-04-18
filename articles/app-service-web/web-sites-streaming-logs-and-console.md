<properties 
    pageTitle="Przesyłanie strumieniowe dzienniki i konsoli" 
    description="Omówienie konsoli i przesyłanie strumieniowe dzienników" 
    authors="btardif" 
    manager="wpickett" 
    editor="" 
    services="app-service\web" 
    documentationCenter=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="10/12/2016" 
    ms.author="byvinyal"/>

# <a name="streaming-logs-and-the-console"></a>Przesyłanie strumieniowe dzienniki i konsoli

## <a name="streaming-logs"></a>Przesyłanie strumieniowe dzienników

**Azure portal** udostępnia zintegrowane przesyłanie strumieniowe viewer dziennika, która umożliwia przeglądanie zdarzenia śledzenia z aplikacji **Usługi aplikacji** w czasie rzeczywistym.  

Aby skonfigurować ta funkcja wymaga kilka prostych czynności:

- Pisanie śledzenia w kodzie
- Włączanie aplikacji **Dzienniki diagnostyczne** dla aplikacji
- Wyświetlanie strumienia z wbudowanych interfejsu użytkownika **Streaming dzienniki** **Azure portal**.

### <a name="how-to-write-traces-in-your-code"></a>Jak pisać śledzenia w kodzie ###

Pisanie śledzenia w kodzie jest łatwe.  W języku C# jest tak proste jak pisanie następujący kod:

`````````````````````````
Trace.TraceInformation("My trace statement");
`````````````````````````

`````````````````````````
Trace.TraceWarning("My warning statement");
`````````````````````````

`````````````````````````
Trace.TraceError("My error statement");
`````````````````````````

Klasy śledzenia znajdują się w obszarze nazw System.Diagnostics.

W aplikacji node.js można napisać ten kod, aby uzyskać ten sam efekt:

`````````````````````````
console.log("My trace statement").
`````````````````````````

### <a name="how-to-enable-and-view-the-streaming-logs"></a>Jak włączyć i wyświetlanie streaming dzienniki
![][BrowseSitesScreenshot]Narzędzia diagnostyczne są włączone na podstawie dla aplikacji. Rozpocznij, przechodząc do witryny chcesz włączyć tę funkcję.  
  
![][DiagnosticsLogs]Z menu Ustawienia przewiń w dół do sekcji **monitorowania** i kliknij pozycję **Dzienniki diagnostyczne (1)**. Następnie **(2) Włącz** **Rejestrowanie aplikacji (system plików)** lub **Aplikacji rejestrowania (blob)** opcja **poziom** pozwala zmienić poziom ważności śledzenia w celu rejestrowania. Jeśli próbujesz po prostu zapoznać się z funkcji, należy ustawić poziom na **pełne** , aby upewnić się, że wszystkie instrukcji śledzenia były zbierane.

Kliknij przycisk **ZAPISZ** w górnej części karta i będzie gotowe wyświetlić dzienniki.

>[AZURE.NOTE] Im wyższy **poziom ważności** , który więcej zasobów są używane do dziennika i śledzenia więcej wyprodukowano. Upewnij się, że **poziom ważności** jest skonfigurowany do poprawne szczegółowości produkcji lub duży ruch witryny. 

![][StreamingLogsScreenshot]Aby wyświetlić **strumieniowego przesyłania dzienników** z poziomu portalu Azure, kliknij przycisk **strumienia dziennika (1)** w dalszej części **Monitorowanie** w menu Ustawienia. Jeśli aplikacji aktywnie zapisuje śledzenia instrukcji, następnie powinna być widoczna je w **streaming (2) dzienniki interfejsu użytkownika** w czasie rzeczywistym najbliższego.

## <a name="console"></a>Konsoli
**Azure portal** zapewnia dostęp do aplikacji. Można eksplorować system plików usługi aplikacji i uruchamianie skryptów programu powershell i cmd. Związana są takie same uprawnienia, Ustaw jako kod aplikacji uruchomione podczas wykonywania polecenia konsoli. Dostęp do katalogów chronionych lub uruchamianie skryptów, które wymagają podwyższonym poziomem uprawnień jest zablokowany.  

![][ConsoleScreenshot]Z menu Ustawienia przewiń w dół do sekcji **Narzędzi do tworzenia** i kliknij na **Konsoli (1)** i **(2) konsoli** interfejsu użytkownika zostanie otwarty w prawo.

Aby zapoznać się z **konsoli**, spróbuj wykonać podstawowe poleceń, takich jak:

`````````````````````````
dir
`````````````````````````

`````````````````````````
cd
`````````````````````````

<!-- Images. -->
[DiagnosticsLogs]: ./media/web-sites-streaming-logs-and-console/diagnostic-logs.png
[BrowseSitesScreenshot]: ./media/web-sites-streaming-logs-and-console/browse-sites.png
[StreamingLogsScreenshot]: ./media/web-sites-streaming-logs-and-console/streaming-logs.png
[ConsoleScreenshot]: ./media/web-sites-streaming-logs-and-console/console.png
