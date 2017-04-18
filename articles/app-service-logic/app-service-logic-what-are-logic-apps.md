<properties 
    pageTitle="Co to są logiczny aplikacje?" 
    description="Dowiedz się więcej o logiczny usługi aplikacji" 
    authors="kevinlam1" 
    manager="dwrede" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article" 
    ms.date="10/12/2016"
    ms.author="klam"/>

# <a name="what-are-logic-apps"></a>Co to są logiczny aplikacje?

Logika aplikacje umożliwiają uprościć i wdrożenie integracji skalowalna i przepływy pracy w chmurze. Udostępnia projektanta graficznego do modelu i Automatyzowanie procesu jako pewne czynności znana pod nazwą przepływu pracy.  Istnieje [wiele łączników](../connectors/apis-list.md) w chmurze i lokalnych do szybkiego integrują w obrębie usługi i protokoły.  Aplikacja logikę zaczyna się od wyzwalacza (like "po dodaniu konta do Dynamics CRM") i po nim wypalania można rozpocząć wielu akcji kombinacje, konwersji i warunek logiczny.

Zalety korzystania z aplikacji logika są następujące:  

- Oszczędzanie czasu przez zaprojektowanie złożone procesy przy użyciu funkcji łatwe do zrozumienia narzędzia do projektowania
- Bezproblemowa wykonania wzorców i przepływy pracy, które w przeciwnym razie będzie trudne do wprowadzenia w kodzie
- Wprowadzenie do szybkiego z szablonów
- Dostosowywanie aplikacji logika z własnych niestandardowych interfejsów API, kod i akcje
- Nawiązywanie połączenia i Synchronizuj odmienne systemy w lokalnym a chmurą
- Tworzenie poza BizTalk server, zarządzania interfejsu API funkcji Azure i Bus usługi Azure z obsługą najlepszych integracji

Logika aplikacji jest w pełni zarządzane iPaaS (Integracja platformy jako usługa) umożliwiające programistom nie musisz się martwić tworzenie hostingu, skalowalność, dostępności i zarządzania.  Logika aplikacje będą rozbudowy automatycznie do zapotrzebowania.

![Projektant aplikacji przepływu](./media/app-service-logic-what-are-logic-apps/LogicAppCapture2.png)

Jak wspomniano, przy użyciu aplikacji logika można automatyzowanie procesów biznesowych. Oto kilka przykładów:  
 
* Przenoszenie plików przekazane do serwera FTP do magazynu platformy Azure
* Proces i rozsyłania zamówień w lokalnej i w chmurze systemów
* Monitorowanie wszystkich tweety dotyczących określonego tematu, analizowanie upodobania i tworzenie alertów i zadań dla elementów wymagających — Flaga monitująca.

Scenariusze, takie jak te można skonfigurować z projektanta graficznego i bez zapisywania jednego wiersza kodu. Uzyskaj wprowadzenie [tworzenia aplikacji logicznych teraz][create].  Po zapisaniu - aplikacji logika można [szybko wdrożony i konfigurowane](app-service-logic-create-deploy-template.md) w wielu środowiskach i regionów.

## <a name="why-logic-apps"></a>Dlaczego logiczny aplikacje?

Logika aplikacje przełącza szybkość i skalowalność do obszaru integracji przedsiębiorstwa.  Łatwe w użyciu projektanta, różnych dostępnych wyzwalaczami i akcje i zaawansowane narzędzia do zarządzania należy Centralizowanie do interfejsów API łatwiejsze niż kiedykolwiek.  Jak firmy przejścia do digitalization, aplikacje logiczny umożliwia łączenia systemów starszych i przełomowych.

Ponadto z naszych [Konta integracji przedsiębiorstwa] [ biztalk] można skalować dla dorosłych scenariuszy integracji z możliwości [wiadomości XML][xml], [trading zarządzania partnera][tpm]i nie tylko.

- **Narzędzia do projektowania łatwy w użyciu** — logikę aplikacji może być zaprojektowane końca do końca w przeglądarce lub za pomocą narzędzia programu Visual Studio. Uruchom z wyzwalacza — od prostej harmonogram, po utworzeniu problem GitHub. Następnie dodać akompaniament dowolną liczbę akcje przy użyciu galerii sformatowanego łączników.

- **Łączenie interfejsy API łatwo** — nawet skład zadania, które są łatwe do opisu są trudne do wprowadzenia w kodzie. Logika aplikacje można łatwo łączyć różnych systemów. Chcesz się połączyć z chmury marketingowych rozwiązanie do programu lokalnego rozliczenia systemu? Chcesz zapewnić scentralizowane wiadomości systemów z Bus usługi przedsiębiorstwa i interfejsów API? Logika aplikacje są najszybszy, Najbezpieczniejszym sposobem przeprowadzania rozwiązania tych problemów.

- **Wprowadzenie do szybkiego przy użyciu szablonów** — ułatwiające rozpoczęcie pracy udostępniamy [galerii szablonów] [ templates] umożliwiające szybkie tworzenie niektóre typowe rozwiązania. Z zaawansowane rozwiązania B2B proste połączenia władz akredytacji bezpieczeństwa i nawet kilka są tylko "dla zabawnych" — Galeria jest najszybszym sposobem na wprowadzenie do programu power logiczny aplikacji.

- **Wypiekany w rozszerzeń** - nie widać łącznik potrzebne? Logikę aplikacji jest przeznaczona do pracy z własnych interfejsów API i kod; Możesz łatwo utworzyć własną aplikację interfejsu API jako łącznik niestandardowy, lub dołączanie do [Funkcji Azure](https://functions.azure.com) wykonać wstawki kodu na żądanie. 

- **Angielski koń parowy rzeczywistą integracji** - rozpoczęcie łatwe i rozwijaniu potrzebne. Logika aplikacje można łatwo korzystanie z możliwości programu BizTalk, firmy Microsoft branżowe wiodące integracji rozwiązania pozwalają specjalistom integracji na tworzenie rozwiązań, które są potrzebne. Dowiedz się więcej na temat [Enterprise Integracja z dodatkiem Service Pack](./app-service-logic-enterprise-integration-overview.md).

## <a name="logic-app-concepts"></a>Logika pojęcia aplikacji

Poniżej przedstawiono niektóre ważne, które składają się możliwości logiczny aplikacji. 

- **Przepływ pracy** — aplikacje logiczny umożliwia graficzne do modelowania procesów biznesowych jako szereg kroków lub przepływu pracy.
- **Zarządzany łączników** — aplikacji logikę muszą mieć dostęp do danych i usług. Łączniki zarządzane są tworzone w celu pomocy możesz podczas łączenia się i pracy z danymi. Zobacz listę urzędów certyfikacji łączników teraz dostępna, w [zarządzaniu łączników][managedapis].
- **Wyzwalacze** - niektóre łączniki zarządzanego mogą także działać jako wyzwalacz. Wyzwalacz uruchamia nowe wystąpienie przepływu pracy według określonego zdarzenia, takie jak nadejście wiadomości e-mail lub zmiany na koncie magazyn Azure.
-  **Akcje** — każdy krok po wyzwalacza w przepływie pracy nosi nazwę akcji. Każda akcja zwykle mapy operacji na łącznik zarządzanych lub niestandardowe aplikacje interfejsu API.
- **Enterprise Integracja z dodatkiem Service Pack** — dla bardziej zaawansowanych scenariuszy integracji, aplikacje logiczny zawiera funkcje BizTalk. BizTalk jest platformą integracji wiodącym branżowe firmy Microsoft. Łączniki Enterprise Integracja z dodatkiem Service Pack umożliwiają łatwe obejmują sprawdzanie poprawności, przekształcania i więcej w logiczny aplikacji przepływów pracy.

## <a name="getting-started"></a>Wprowadzenie  

- Aby rozpocząć pracę z aplikacjami logiczny, wykonaj [Tworzenie aplikacji logika] [ create] samouczka.  
- [Przykłady typowych widoku i scenariusze](app-service-logic-examples-and-scenarios.md)
- [Można zautomatyzować procesów biznesowych za pomocą aplikacji warunków logicznych](http://channel9.msdn.com/Events/Build/2016/T694) 
- [Dowiedz się, jak integrację systemów z aplikacjami warunków logicznych](http://channel9.msdn.com/Events/Build/2016/P462)

[biztalk]: app-service-logic-enterprise-integration-accounts.md
[appservice]: ../app-service/app-service-value-prop-what-is.md
[create]: app-service-logic-create-a-logic-app.md
[managedapis]: ../connectors/apis-list.md
[tpm]: app-service-logic-enterprise-integration-accounts.md
[xml]: app-service-logic-enterprise-integration-b2b.md
[templates]: app-service-logic-use-logic-app-templates.md
