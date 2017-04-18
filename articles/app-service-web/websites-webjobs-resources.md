<properties 
    pageTitle="Zasoby dokumentacji WebJobs Azure" 
    description="Zalecanych zasobów dla Dowiedz się, jak używać Azure WebJobs i Azure WebJobs SDK." 
    services="app-service" 
    documentationCenter=".net" 
    authors="tdykstra" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/28/2016" 
    ms.author="tdykstra"/>

# <a name="azure-webjobs-documentation-resources"></a>Zasoby dokumentacji WebJobs Azure

## <a name="overview"></a>Omówienie

Ten temat prowadzi do dokumentacji zasobów dotyczących sposobu używania Azure WebJobs i Azure WebJobs SDK. Azure WebJobs stanowią łatwy sposób na uruchamianie skryptów lub programów jako procesy w tle w kontekście [aplikacji usługi aplikacji sieci web, aplikacji interfejsu API lub aplikacji dla urządzeń przenośnych](../app-service/app-service-value-prop-what-is.md). Możesz przekazać i uruchom plik wykonywalny, takich jak jako cmd bat, exe (.NET), ps1, Pokaż, php, Kopiuj, js i słoik. Te programy uruchamiane jako WebJobs według harmonogramu (cron) albo w sposób ciągły.

Przeznaczenie [WebJobs SDK](websites-webjobs-resources.md) jest uprościć tworzonego umożliwiającą wykonywanie typowych zadań, że WebJob można wykonać, takie jak przetwarzanie obrazu, w kolejce przetwarzania kodu, RSS agregacji konserwacja plików i wysyłanie wiadomości e-mail. WebJobs SDK zawiera wbudowane funkcje do pracy z magazyn Azure i Bus usługi, planowanie zadań i obsługi błędów i wiele innych typowych scenariuszy. Ponadto go opracowano z myślą o być extensible, a jest [Otwórz źródło repozytorium rozszerzenia](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview). [Funkcje Azure](../azure-functions/functions-overview.md) (obecnie w podglądzie) jest oparty na wersję SDK WebJobs, działającego w programie C# skrypt, Node.js i innych języków. 

Tworzenie, wdrażania i zarządzania WebJobs jest bezproblemowa przy użyciu zintegrowanego narzędzia w programie Visual Studio. Tworzenie WebJobs przy użyciu szablonów, publikowanie i zarządzanie nimi (Uruchom i Zatrzymaj/monitor debugowanie) je. 

Pulpit nawigacyjny WebJobs w portalu Azure zapewnia możliwości zarządzania zaawansowanych, przedstawiające pełną kontrolę nad realizacji WebJobs, łącznie z możliwością wywołania poszczególnych funkcji w WebJobs. Pulpit nawigacyjny wyświetla także obsługi funkcji i rejestrowanie danych wyjściowych. 

##<a name="getstarted"></a>Wprowadzenie do WebJobs i WebJobs SDK

* [Wprowadzenie do Azure WebJobs](http://www.hanselman.com/blog/IntroducingWindowsAzureWebJobs.aspx)
* [Azure WebJobs jest klienci i należy zacząć korzystać je teraz!](http://www.troyhunt.com/2015/01/azure-webjobs-are-awesome-and-you.html) (Wpis w blogu, Troy Hunt.)
* [Funkcje Azure WebJobs](/blog/2014/10/22/webjobs-goes-into-full-production/)
* [Co to jest zestaw SDK WebJobs](websites-dotnet-webjobs-sdk.md)
* [Wskazówki zadania tła przez Microsoft wzorce i rozwiązania](/documentation/articles/best-practices-background-jobs/)
* [Informowanie o 1.1.0 RTM SDK WebJobs platformy Microsoft Azure](/blog/azure-webjobs-sdk-1-1-0-rtm/)
* [Rozpoczynanie pracy z Azure WebJobs SDK](websites-dotnet-webjobs-sdk-get-started.md)
* [Jak używać magazyn kolejek Azure z zestawu SDK WebJobs](websites-dotnet-webjobs-sdk-storage-queues-how-to.md)
* [Jak korzystać z zestawu SDK WebJobs magazyn obiektów blob platformy Azure](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md)
* [Jak korzystać z zestawu SDK WebJobs magazyn tabel platformy Azure](websites-dotnet-webjobs-sdk-storage-tables-how-to.md)
* [Jak używać Bus usługi Azure z zestawu SDK WebJobs](websites-dotnet-webjobs-sdk-service-bus.md)
* [Jak używać WebHooks z zestawu SDK WebJobs wraz z przykładami GitHub, IFTTT i HTTP](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/WebHooks-Walkthrough)
* [Azure WebJobs SDK podręczna karta informacyjna (Pobierz plik PDF)](http://go.microsoft.com/fwlink/?LinkID=524028&clcid=0x409)
* [Dokumentacja ustawienia WebJobs w GitHub](https://github.com/projectkudu/kudu/wiki/Web-jobs).
* Klipy wideo
    * [WebJobs i WebJobs SDK](http://channel9.msdn.com/Shows/Cloud+Cover/Episode-153-WebJobs-with-Pranav-Rastogi?utm_source=dlvr.it&utm_medium=twitter)
    * [Azure WebJobs seria wideo kanału 9](http://channel9.msdn.com/Tags/azurefridaywebjobs)
    * [Wprowadzenie do WebJobs narzędzie programu Visual Studio](http://channel9.msdn.com/Shows/Web+Camps+TV/Introducing-WebJobs-Tooling-for-Visual-Studio-with-Brady-Gaster) 
    * [Narzędzia WebJobs i debugowanie zdalne](http://channel9.msdn.com/Shows/Web+Camps+TV/WebJobs-GA-Series-Episode-1-WebJobs-Tooling-with-Brady-Gaster)
    * [Azure aktualizacji WebJobs za pomocą Pranav Rastogi - rozszerzeń w wersji 1.1](https://channel9.msdn.com/Shows/Cloud+Cover/Episode-183-Azure-WebJobs-Update-with-Pranav-Rastogi)

Zobacz też następujące sekcje [WebJobs wdrażanie](#deploy) i [Testowanie i debugowanie WebJobs](#debug).

##<a name="deploy"></a>Wdrażanie WebJobs

* [Jak wdrożyć WebJobs Azure za pomocą programu Visual Studio](websites-dotnet-deploy-webjobs.md)
* [Jak wdrożyć WebJobs za pomocą portalu Azure](web-sites-create-web-jobs.md)
* [Włączanie wiersza polecenia lub ciągły dostarczenie Azure WebJobs](https://azure.microsoft.com/blog/2014/08/18/enabling-command-line-or-continuous-delivery-of-azure-webjobs/)
* [Wdrażanie aplikacji konsoli programu .NET Azure za pomocą WebJobs cyfra](http://blog.amitapple.com/post/73574681678/git-deploy-console-app/)
* [Wdrażanie WebJob F # Azure](http://blogs.msdn.com/b/dave_crooks_dev_blog/archive/2015/02/18/deploying-f-web-job-to-azure.aspx)
* [Wdrażanie usługi niestandardowe jako Azure Webjobs](http://withouttheloop.com/articles/2015-06-23-deploying-custom-services-as-azure-webjobs/)
* Klipy wideo
    * [Wprowadzenie do WebJobs narzędzie programu Visual Studio](http://channel9.msdn.com/Shows/Web+Camps+TV/Introducing-WebJobs-Tooling-for-Visual-Studio-with-Brady-Gaster) 
    * [Narzędzia WebJobs i debugowanie zdalne](http://channel9.msdn.com/Shows/Web+Camps+TV/WebJobs-GA-Series-Episode-1-WebJobs-Tooling-with-Brady-Gaster) 

##<a name="schedule"></a>Planowanie WebJobs

* [Azure WebJob okno dialogowe Dodawanie](websites-dotnet-deploy-webjobs.md#configure)
* [Tworzenie WebJob według harmonogramu w portalu Azure](web-sites-create-web-jobs.md#CreateScheduled)
* [Podczas zadanie harmonogramu do WebJob](http://blog.davidebbo.com/2015/05/scheduled-webjob.html)
* [Planowanie Azure WebJobs cron wyrażeń](http://blog.amitapple.com/post/2015/06/scheduling-azure-webjobs/)
* [Planowanie poszczególnych funkcji WebJob za pomocą WebJobs SDK TimerTrigger](websites-dotnet-webjobs-sdk.md#schedule)

##<a name="debug"></a>Testowanie i debugowanie WebJobs

* [Nowe Deweloper i debugowanie funkcjami Azure WebJobs w programie Visual Studio](http://blogs.msdn.com/b/webdev/archive/2014/11/12/new-developer-and-debugging-features-for-azure-webjobs-in-visual-studio.aspx)
* [Wyświetlanie pulpitu nawigacyjnego WebJobs](websites-dotnet-webjobs-sdk-get-started.md#view-the-webjobs-sdk-dashboard)
* [Jak pisać dzienników przy użyciu zestawu SDK WebJobs i wyświetlać je na pulpicie nawigacyjnym](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#logs)
* [Zdalny WebJobs debugowania](web-sites-dotnet-troubleshoot-visual-studio.md#remotedebugwj)
* [Dany obiekt blob określonego autora?](http://blogs.msdn.com/b/jmstall/archive/2014/02/19/who-wrote-that-blob.aspx) 
* [Interakcyjne kod hostingu w chmurze](http://blogs.msdn.com/b/jmstall/archive/2014/04/26/hosting-interactive-code-in-the-cloud.aspx)
* [Dodawanie śledzenia Azure WebJobs](http://blogs.msdn.com/b/mcsuksoldev/archive/2014/09/04/adding-trace-to-azure-web-sites-and-web-jobs.aspx)
* [Monitorowanie, diagnozowanie i rozwiązywanie problemów z magazynem tabel platformy Microsoft Azure](../storage/storage-monitoring-diagnosing-troubleshooting.md)
* Klipy wideo
    * [Narzędzia WebJobs i debugowanie zdalne](http://channel9.msdn.com/Shows/Web+Camps+TV/WebJobs-GA-Series-Episode-1-WebJobs-Tooling-with-Brady-Gaster) 

##<a name="scale"></a>Skalowanie WebJobs

* [Skalowanie aplikacji sieci Web z Azure witrynami sieci Web](http://msdn.microsoft.com/magazine/dn786914.aspx)
* [Azure aplikacji usługi: Projektując aplikacje Web gotowe dla firm ogromną skalę](https://channel9.msdn.com/Events/Build/2014/3-626). Obejmuje skalowanie aplikacji sieci web z WebJobs, łącznie z zestawu SDK WebJobs.
* Klipy wideo
    * [Skalowanie zewnętrzne WebJobs](http://channel9.msdn.com/Shows/Azure-Friday/Azure-WebJobs-105-Scaling-out-Web-Jobs)

##<a name="additional"></a>Dodatkowe zasoby WebJobs

* [Azure WebJobs GA wpis w blogu, Magnus Mårtensson](http://magnusmartensson.com/azure-webjobs-ga)
* [Uruchomionych Azure aplikacji usługi sieci Web programu Powershell zadania](http://blogs.msdn.com/b/nicktrog/archive/2014/01/22/running-powershell-web-jobs-on-azure-websites.aspx)
* [Uzyskiwanie powiadomienia o usługi Azure wyzwalane WebJobs wykonuje](http://blog.amitapple.com/post/2014/03/webjobs-notification/)
* [Proste zasady przechowywania kopii zapasowej aplikacji sieci Web z WebJobs](https://azure.microsoft.com/blog/2014/04/28/simple-web-site-backup-retention-policy-with-webjobs/)
* [Aplikacje azure Web i usług w chmurze wolniej na pierwsze żądanie](http://wp.sjkp.dk/windows-azure-websites-and-cloud-services-slow-on-first-request/). Pokazano, jak za pomocą WebJobs zasymulowania funkcji (AlwaysOn), która jest dostępna tylko w warstwie cennik standardowy.
* [WebJobs bezpiecznie zamknięty](http://blog.amitapple.com/post/2014/05/webjobs-graceful-shutdown/#.U72Il_5OWUl). Dla SDK WebJobs bezpiecznie zamknięty, zobacz [łagodnego zamykania](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#graceful).)
* [Automatyzowanie wykonywania kopii zapasowych przy użyciu Azure WebJobs i AzCopy](http://markjbrown.com/azure-webjobs-azcopy/)
* Klipy wideo
    * [Azure wideo WebJobs przez Magnus Mårtensson](https://www.youtube.com/playlist?list=PLqp1ZOYYUSd81yEzMYLTw8cz91wx_LU9r)
    * [Azure WebJobs seria wideo kanału 9](http://channel9.msdn.com/Tags/azurefridaywebjobs)

##<a name="additionalsdk"></a>Dodatkowe zasoby WebJobs SDK

* [Informacje o wersji zestawu SDK WebJobs](https://github.com/Azure/azure-webjobs-sdk/wiki/Release-Notes)
* [Kod źródłowy WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk)
* [Kod źródłowy rozszerzenia WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk-extensions), [szczegółowe](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview)prowadnicy do modelu rozszerzania.  
* [Kod źródłowy skryptu SDK WebJobs](https://github.com/Azure/azure-webjobs-sdk-script/) (używany dla [Funkcji Azure](../azure-functions/functions-overview.md))
* [WebJob przekazywać pliki FREB do magazynowania Azure przy użyciu zestawu SDK WebJobs](http://thenextdoorgeek.com/post/WAWS-WebJob-to-upload-FREB-files-to-Azure-Storage-using-the-WebJobs-SDK)
* [Z zalet rejestrowania z platformy Azure hostingu Azure webjobs poza Azure hostowanej webjob](http://bypassion.dk/?p=510)
* [Tworzenie narzędzia importowanie danych z Azure WebJobs](http://www.freshconsulting.com/building-data-import-tool-azure-webjobs/)
* [Omówienie funkcji Azure](../azure-functions/functions-overview.md)
* Klipy wideo
    * [Azure WebJobs seria wideo kanału 9](http://channel9.msdn.com/Tags/azurefridaywebjobs)

##<a name="samples"></a>Przykładowe WebJob aplikacje

* [Przykładowe aplikacje dostarczonymi przez zespół WebJobs na GitHub](https://github.com/azure/azure-webjobs-sdk-samples)
* [Prosta Azure aplikacji sieci Web z WebJobs wewnętrznej bazy danych przy użyciu zestawu SDK WebJobs](http://code.msdn.microsoft.com/Simple-Azure-Website-with-b4391eeb)
* [SiteMonitR](http://code.msdn.microsoft.com/SiteMonitR-dd4fcf77). Pokazuje stosowania WebJobs zaplanowanych i sterowane zdarzeniami. Zobacz blogu [odbudowanie SiteMonitR przy użyciu zestawu SDK WebJobs Azure](http://www.bradygaster.com/post/rebuilding-the-sitemonitr-using-windows-azure-webjobs).

##<a name="blogs"></a>Blogów

* [Azure blogu](/blog)
* [Blog Amitowi firmy Apple](http://blog.amitapple.com/). Zespoły na WebJobs (nie SDK).
* [Blog Magnus Mårtensson](http://magnusmartensson.com/)

##<a name="gethelp"></a>Uzyskiwanie pomocy dotyczącej WebJobs

* [Zdarzeń StackOverflow dla WebJobs](http://stackoverflow.com/questions/tagged/azure-webjobs)
* [Zdarzeń StackOverflow dla zestawu SDK WebJobs](http://stackoverflow.com/questions/tagged/azure-webjobssdk)
* [Zdarzeń StackOverflow funkcje Azure](http://stackoverflow.com/questions/tagged/azure-functions)
* [Forum Azure i programu ASP.NET](http://forums.asp.net/1247.aspx)
* [Forum aplikacji sieci Web usługi Azure](http://social.msdn.microsoft.com/Forums/azure/home?forum=windowsazurewebsitespreview)
* [Azure witryny głosu użytkownika aplikacji sieci Web](https://feedback.azure.com/forums/169385-websites/)
* [Serwisu twitter](http://twitter.com/). Za pomocą hashtagu #AzureWebJobs.
* [Raportowanie błędów WebJobs lub emisja](https://github.com/projectkudu/kudu/wiki/Reporting-WebJobs-issues)

