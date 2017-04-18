<properties
   pageTitle="Testowanie pióra | Microsoft Azure"
   description="Artykuł zawiera omówienie przenikania testowania proces (pentest) i jak wykonaj pentest przed aplikacji działa w infrastrukturze Azure."
   services="security"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/25/2016"
   ms.author="yurid"/>

# <a name="pen-testing"></a>Testowanie pióra

Jest jedną z najważniejszych funkcji testowania aplikacji i wdrażania za pomocą Microsoft Azure, że nie musisz łączone infrastrukturę lokalnego do opracowywania, testowanie i wdrażania aplikacji. Wszystkie infrastruktury jest wykonywane obsługę w usługach platformy Microsoft Azure. Nie musisz martwić tworzenia zapotrzebowania, podczas uzyskiwania, "rozlania i układania" sprzęcie lokalnego.

Jest to doskonałe —, ale nadal trzeba upewnij się, wykonywanie normalny zabezpieczeń starannością ukończenia. Jedną z czynności należy wykonać jest penetracyjne przetestować aplikacje umieszczaniu w Azure.

Może być już wiesz, że Microsoft wykonuje [testy penetracyjne naszego Azure środowiska](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e). To pomagają nam poprawiać nasze platformy który prowadzi działania poprawy kontroli bezpieczeństwa, wprowadzenie do nowych kontroli bezpieczeństwa i poprawy naszych procesów zabezpieczeń.

Firma Microsoft nie pióra przetestować aplikację dla Ciebie, ale firma Microsoft wie, zostaną ma i wykonanie pióra badania na potrzeby własnych aplikacji. Wynika to z dobrym elementu, gdy można zwiększyć bezpieczeństwo aplikacji, możesz zabezpieczania całego ekosystemu Azure.

Gdy pióra można przetestować aplikacje, może wyglądać atakiem z nami. Firma Microsoft [stale monitorować](http://blogs.msdn.com/b/azuresecurity/archive/2015/07/05/best-practices-to-protect-your-azure-deployment-against-cloud-drive-by-attacks.aspx) ataki desenie i rozpocznie proces zdarzenia odpowiedzi, jeśli trzeba. Nie go ułatwiają i nie Pomóż nam, jeśli firma Microsoft wyzwalanie zdarzenia odpowiedź ze względu na własny ukończenia pióro starannością testowania.

Co należy zrobić?

Gdy zechcesz pióro Przetestuj aplikacje hostowanej Azure, należy poinformować nas o tym. Po możemy wiesz, że użytkownik chce wykonywać określone testy, firma Microsoft nie będzie przypadkowo zamknięty (na przykład blokowania testowania z adresem IP), jak testy odpowiadają Azure pióra testowania warunki i postanowienia.
Standardowe testy, które mogą wykonywać obejmują:

- Badania punktów końcowych do wykrycia [Górny luk 10 Otwieranie sieci Web aplikacji zabezpieczeń programu Project (OWASP)](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project)
- [Testowanie argumentu rozmycie](https://blogs.microsoft.com/cybertrust/2007/09/20/fuzz-testing-at-microsoft-and-the-triage-process/) punktów końcowych
- [Skanowanie portów](https://en.wikipedia.org/wiki/Port_scanner) punktów końcowych

Test, który nie można wykonać jest dowolnego rodzaju [odmowa](https://en.wikipedia.org/wiki/Denial-of-service_attack) usługi (DoS). Ta opcja uwzględnia inicjowania atakiem DoS, sam lub wykonywanie testów pokrewnych, które może ustalić, wykazać lub symulować dowolnego typu atakiem DoS.

Chcesz rozpocząć pracę z pióra testujesz aplikacji obsługiwany w programie Microsoft Azure? Jeśli tak, a następnie głowy na nad do [Przenikania Test omówienie](https://security-forms.azure.com/penetration-testing/terms) strony (i kliknij przycisk Utwórz żądanie badania u dołu strony. Ponadto znajdziesz więcej informacji na temat pióra, testując warunki i przydatne łącza na jak można zgłosić wady zabezpieczeń związanych z Azure lub innych usług firmy Microsoft.
