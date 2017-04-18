<properties
   pageTitle="Opis microservices | Microsoft Azure"
   description="Omówienie dlaczego jest ważna w przypadku tworzenia aplikacji nowoczesny konstruowania aplikacje w chmurze przy użyciu metody microservices i jak tkaninie usługi Azure udostępnia platformę, aby osiągnąć ten cel"
   services="service-fabric"
   documentationCenter=".net"
   authors="msfussell"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/20/2016"
   ms.author="mfussell"/>

# <a name="why-a-microservices-approach-to-building-applications"></a>Dlaczego microservices dojechać do tworzenia aplikacji?
Jako deweloperów, a nie ma nowych w jak możemy wziąć pod uwagę factoring aplikacji do części. Jest centralnej modelu Orientacja obiektu, poboru oprogramowania i componentization. Obecnie ten factorization powoduje formę klasy i interfejsy między udostępnianych bibliotek i technologia warstwy. Zazwyczaj wielopoziomowe podejście jest pobierana z magazynu wewnętrznej, reguł biznesowych warstwy środkowej i zewnętrzną interfejsu użytkownika. Jakie *występują* zmianie ciągu ostatnich kilka lat jest, że firma Microsoft, jako deweloperów tworzenia rozłożone aplikacjach dla chmury prowadzone przez firmy.

Zmienianie potrzeb biznesowych są:

- Tworzenie i działanie usługi w większej skali w celu osiągnięcia klientów z nowych regionach geograficznych (na przykład).
- Szybsze dostarczenie funkcji i możliwości, aby można było odpowiadanie na potrzeby klientów w sposób adaptacyjne.
- Wykorzystanie ulepszone zasobów, aby zmniejszyć koszty.

Te potrzeb biznesowych mają wpływ na *sposób* budowy aplikacji.

Aby uzyskać więcej informacji na jego Azure sposobem microservices, przeczytaj [Microservices: obrót aplikacji korzystająca z chmury](https://azure.microsoft.com/blog/microservices-an-application-revolution-powered-by-the-cloud/).

## <a name="monolithic-vs-microservice-design-approach"></a>Wbudowanymi a microservice podejście do projektowania
Wszystkie aplikacje są obsługiwane w czasie. Aplikacje pomyślnego rozwijać jest przydatne dla osób. Niepowodzenie aplikacji nie są obsługiwane i ostatecznie są przestarzałe. Staje się pytanie: ilość informacje dotyczące wymagań dzisiaj, a co oni będą w przyszłości? Załóżmy na przykład, że tworzysz aplikację raportowania dla działu. Masz pewność, że aplikacja pozostanie w zakresie firmy i że raportów będzie krótkotrwałe. Wybór metody różni się od, powiedzieć tworzenia usługi sieci dostarczania zawartości wideo na dziesiątki milionów klientów. Czasami pobieranie coś się drzwi jako zatwierdzania koncepcji jest współczynnik kierowania znajomości później przeprojektowana aplikacji. Istnieje mały punktu w nadmiernie rysunek techniczny coś, który nie jest używany. Jest zwykle zależność odtwarzania. Z drugiej strony mówiąc firmy dotyczących tworzenia dla chmury, oczekuje się wzrostu i zastosowania. Ten problem jest możliwość wzrostu i skalę nieprzewidywalny. Chcemy można było prototypu szybko podczas także informacje o jesteśmy na ścieżce na zajęcie się przyszłego sukcesu. Jest to metoda szczupłej uruchamiania: tworzenie, zmierzyć, Dowiedz się, iteracji.

Podczas ery klient / serwer możemy pielęgnowane skoncentrować się na tworzenie warstwowych aplikacji przy użyciu technologii określonych w każdej warstwie. Aplikacja "wbudowanymi" terminów pojawiło dla tych metod. Interfejsy pielęgnowane należeć do przedziału poziomy i bardziej ściśle związane projektu używane między składnikami w każdej warstwy. Deweloperów zaprojektowane i uwzględnić klas skompilowany do biblioteki, powiązane w kilka plików wykonywalnych i bibliotekach DLL. Istnieje korzyści podejście wbudowanymi projektu. Często jest prostsze projektowanie i ma szybszy połączeń między składnikami, ponieważ te połączenia są często przez IPC. Ponadto wszystkich testów pojedynczego produktu, która jest więcej osób z zasobami — wydajność. Wadą podwyższonego to, że przylegające sprzężenie wyniki warstwowych warstwy, a nie mogę przeskalować pojedynczych składników. Jeśli zachodzi potrzeba wykonania poprawki ani uaktualnienia, musisz zaczekać, aż inne osoby do końca badań. Jest trudniejsze być adaptacyjne.

Microservices rozwiązania tych downsides i więcej ściśle wyrównywanie wymagania firm, ale mają one również zarówno upsides, jak i downsides. Zalety microservices to, że każdy z nich zwykle obejmuje prostsze funkcji biznesowych, która skalowanie w górę lub dół test, wdrażanie i zarządzanie nimi niezależnie. Ważne korzyścią podejście microservice to, że członkom zespołu są więcej scenariusze biznesowe niż prowadzone przez technologii, która zaleca wielopoziomowe podejście. W praktyce mniejszych zespołów opracować microservice według scenariusza klienta przy użyciu dowolnego technologii, wybrane przez nich. Innymi słowy organizacji nie trzeba NORMALIZUJ tech to zachowanie wbudowanymi aplikacji. Poszczególne zespoły, które własnych usług można wykonać, co ma sens dla nich według specjalizacji zespołu lub co to jest najbardziej odpowiednie rozwiązania tego problemu. W praktyce o zestaw zalecane technologii na przykład w szczególności NoSQL ze sklepu lub sieci web AIF, lepiej jest.

Wadą podwyższonego microservices składa się z zarządzania zwiększona liczba jednostek oddzielnych i zajmujących się bardziej złożonych wdrożeń i przechowywanie wersji. Ruch sieciowy między microservices zwiększa oraz odpowiadające im występują opóźnienia sieciowe. Masz wiele chatty, szczegółowego usługi są przepisami dla nightmare wydajności. Bez narzędzia ułatwiające widoku zależności trudno "zobacz" całego systemu. Normy są, jakie będą wprowadzać ujęciu microservice pracy przez wyrażenie zgody na temat komunikowanie się i są na uszkodzenia możliwości, z usługą zamiast sztywne umów. Należy zdefiniować te kontakty do góry w projekcie, po niezależnie od innych usług. Opis innej coined projektowanie podejście microservices jest "szerokiego SOA."


***W najprostszym podejście do projektowania microservices jest rozłączone Federacja usług, przy użyciu niezależnych zmian do każdego i uzgodnione standardy komunikacji.***


Jak wyprodukowano więcej aplikacji w chmurze, osoby okazać, że ten dekompozycji ogólnego aplikacji do usług niezależnych, skoncentrowanym na scenariuszach lepszym rozwiązaniem długoterminowe.

## <a name="comparison-between-application-development-approaches"></a>Porównanie metod opracowywania aplikacji

![Tworzenie aplikacji platformy tkaninie usługi][Image1]

1. Wbudowanymi aplikacja zawiera funkcje specyficzne dla domeny i zazwyczaj jest dzielona przez funkcjonalności warstwy, takich jak sieci web, firm i danych.

2. Wbudowanymi aplikacji jest skalowanie przez klonowanie go na wiele serwerów i maszyny wirtualne i kontenerów.

3. Aplikacja microservice rozdzielający funkcji do poszczególnych usług mniejszy.

4. Tej metody skale się niezależnie od tego, wdrażając każdej usługi tworzenia wystąpienia tych usług przez serwery-maszyny wirtualne i kontenerów.


Projektowanie przy użyciu microservice podejście nie jest panaceum dla wszystkich projektów, ale lepiej wyrównać z celów biznesowych, opisanych wcześniej. Zaczynając od podejście wbudowanymi można zaakceptować, jeśli wiesz, że później nie ma możliwości zmian kodu do projektowania microservice, jeśli to konieczne. Zazwyczaj zaczyna wbudowanymi aplikacji i powoli podzielić je etapami, począwszy od funkcjonalności obszarów, które muszą być bardziej skalowalna lub adaptacyjne.

Podsumowując, rozwiązaniem microservice jest Zredaguj aplikacja wiele mniejszych usług.  Uruchom usługi w kontenerach wdrożony klastrze maszyn. Mniejszych zespołów opracowywaniu usługi omówiono scenariusz i niezależne przetestować, wersji, wdrażanie i skalowanie każdej usługi, aby ją jako całość może rozwijać.

## <a name="what-is-a-microservice"></a>Co to jest microservice?

Istnieją różne definicje microservices i wyszukiwanie w Internecie zawiera wiele dobre zasobów dostarczających własne punktu widzenia i definicje. Jednak większość następujące właściwości microservices są powszechnie uzgodnione:

- Umieszczać scenariusza biznesowego lub klienta. Co to jest problem, który jest rozwiązywania?
- Opracowany przez małych zespołów odtwarzania.
- Napisane w dowolnym języku programowania i użyj dowolnego framework.
- Składa się z kodu i (opcjonalnie) stanu, które są niezależne wersji wdrożony i skalowany.
- Interakcja z innymi microservices nad interfejsów przejrzyste i protokoły.
- Mają unikatowe nazwy (URL) umożliwia rozpoznanie ich lokalizacji.
- Pozostają spójne i dostępne w obecności błędy.

Umożliwia podsumowanie tych właściwości do:

***Aplikacje Microservice składają się z małych, niezależnie od siebie wersji i skalowalna ograniczony klienta usług, które komunikowania się między sobą przez standardowe protokoły przejrzyste interfejsy.***


Firma Microsoft objęta pierwszych dwóch punktów w poprzedniej sekcji, a teraz możemy rozwiń na i innych wyjaśnienia.

### <a name="written-in-any-programming-language-and-use-any-framework"></a>Napisane w dowolnym języku programowania i użyj dowolnego framework
Jako deweloperów będziemy wolnego wyboru, niezależnie od języka lub framework potrzebna, w zależności od naszych umiejętności lub na potrzeby usługi. W niektórych usług może być wartości wydajności zalety C++ przede wszystkim jeszcze. W innych usługach ułatwienia rozwoju zarządzanych w C# lub Java może być najbardziej istotne. W niektórych przypadkach może być konieczne używanie określonej biblioteki innych firm, technologia miejsca do magazynowania danych lub oznacza ujawnienia usług klientom.

Po wybraniu opcji technologii, możesz zadać do zarządzania operacyjne lub cykl życia i skalowanie usługi.

### <a name="allows-code-and-state-to-be-independently-versioned-deployed-and-scaled"></a>Umożliwia kod i stanie się niezależnie wersji wdrożony i skalowania  

Jednak chcesz napisać usługi microservices, kod i opcjonalnie stan należy niezależne wdrażanie, uaktualnianie i skalowanie. Faktycznie jest jednym z trudniejsze problemy, aby rozwiązać, ponieważ pochodzi w dół do wyboru technologii. Aby zapoznać się skalowanie, jak partition (lub shard) kod i stan jest trudne. Kod i stan Użyj oddzielnych technologii, (które dzisiaj mają zwykle wykonaj), skrypty wdrażania dla programu microservice muszą mieć możliwość radzenia sobie z skalowania oba. Jest to także o elastyczności i elastyczność, aby móc uaktualnić niektóre microservices bez konieczności aktualizacji je wszystkie jednocześnie.
Powrót do wbudowanymi i podejście microservice przez chwilę, na poniższym diagramie przedstawiono różnice w ujęciu do przechowywania stanu.

#### <a name="state-storage-between-application-styles"></a>Województwo miejsca do magazynowania między stylami aplikacji
![Usługa tkaninie platformy stan miejsca do magazynowania][Image2]

***Po lewej stronie jest ujęciu wbudowanymi w jednej bazie danych i poziomów określonych technologii.***

***Po prawej stronie jest rozwiązaniem microservices, wykres połączone microservices miejsce, w którym stan jest zwykle ograniczone do microservice i różnych technologii są używane.***

W podejściu wbudowanymi zazwyczaj znajduje się pojedynczego baza danych używanego przez aplikację. Zaletą jest jednym miejscu, co ułatwia wdrażanie. Każdy składnik może mieć jednej tabeli w celu przechowywania stanu. Członkom zespołu muszą być ściśle do oddzielania stanu, który jest trudne. Istnieje uniknięcia temptations dodać nową kolumnę do istniejącej tabeli odbiorcy, wykonaj sprzężenia między tabelami i Tworzenie współzależności w warstwie miejsca do magazynowania. Gdy w takim przypadku nie mogę przeskalować pojedynczych składników. W ujęciu microservices każdej usługi, zarządza i przechowuje własną stan. Każda usługa jest odpowiedzialny za skalowania stan razem w celu spełnienia wymagań usługę i kody. Wadą podwyższonego to, że w przypadku konieczności tworzenie widoków i kwerend danych aplikacji będzie konieczne kwerenda wykonywana w różnych stan sklepy. Zazwyczaj jest to rozwiązuje przez oddzielnych microservice, konstruująca widoku w kolekcji microservices. Jeśli zachodzi potrzeba wykonania wielu kwerend ad hoc na danych, każdej microservice należy uwzględnić, zapisywania danych do składu usługi dla trybu offline analizy danych.

Przechowywanie wersji jest specyficzny dla wdrożoną wersję microservice tak wiele, wersje wdrażanie i uruchamianie obok siebie. Przechowywanie wersji adresy scenariusze, w którym nowsza wersja microservice ulega awarii podczas uaktualniania i musi do przywrócenia starszej wersji. Inne scenariusz dla wersji wykonuje badania-B-styl, gdzie różnych użytkowników możliwości różnych wersji usługi. Na przykład jest typowych uaktualnienia microservice dla określonej grupy odbiorców, aby przetestować nową funkcję przed powszechnie stopniowych się więcej. Po cyklu zarządzania microservices to teraz przesuwa us komunikacji między nimi.


### <a name="interacts-with-other-microservices-over-well-defined-interfaces-and-protocols"></a>Współdziała z innymi microservices nad interfejsów przejrzyste i protokoły

W tym temacie musi nieco uwagę, ponieważ istnieje szczegółowe materiały na architekturze usługowych opublikowanych w ostatnich 10 lat opisem wzorców komunikacji. Ogólnie rzecz biorąc komunikacja między usługą używa podejście RESZTA z protokoły TCP i HTTP i XML lub JSON formacie szeregowania. Z perspektywy interfejsu jest o obejmującego podejście do projektowania sieci web. Ale nic on Brak spowodowany za pomocą protokołów binarnych lub własnych formatów danych. Należy przygotować osobom trudniejsze czas przy użyciu programu microservices, jeśli są one dostępne w sposób jawny.

### <a name="has-a-unique-name-url-used-to-resolve-its-location"></a>Ma unikatową nazwę (URL) umożliwia rozpoznanie lokalizacji

Należy pamiętać o tym, jak zachować możemy informujący, że podejście microservice przypomina sieci web? Przykład sieci web usługi microservice musi być adresach miejsce, w którym jest uruchomiony. Obsługiwanego o komputerach, który jest uruchomiony określonego microservice elementy zostaną niedługo szybko. W ten sam sposób, że usługa DNS rozpoznaje określonego adresu URL do określonego komputera sieci microservice musi mieć unikatową nazwę tak, aby bieżącego położenia odnajdowania. Microservices muszą mieć adresach nazwy, które były niezależnie od infrastruktury, które działają w. Oznacza to, że jest interakcji między jak wdrożyć usługi i jak okaże się, ponieważ musi istnieć rejestru usługi. Tak, gdy komputera kończy się niepowodzeniem rejestru usługi musi informujący o którym teraz jest uruchomiona usługa. Spowoduje to wyświetlenie nam do następnego tematu: odporność i spójność.

### <a name="remains-consistent-and-available-in-the-presence-of-failures"></a>Spójne i dostępne w obecności błędy

Sposób postępowania z nieoczekiwanych błędów jest jednym z najtrudniejsze problemy, aby rozwiązać, szczególnie w Rozproszony system. Duża część kod, którego możemy zapisać jako deweloperów obsługuje wyjątki, a to samo miejsce, w którym najlepiej jest czas do testowania. Ale problem jest bardziej skomplikowane niż pisania kodu do obsługi błędów. Co się dzieje, gdy komputera, na którym działa microservice kończy się niepowodzeniem? Nie tylko potrzebne wykrywanie tego błędu microservice (słabo problem samodzielnie), ale należy również czegoś o ponowne uruchomienie usługi microservice. Microservice musi być odporna na awarie i uruchom ponownie często na innym komputerze powodów dostępność. To również dostępny w dół do pozycji, w jakim stanie został zapisany w imieniu microservice, miejsce, w którym można go odzyskać ten stan z i czy jest w stanie ponownie uruchomić pomyślnie. Innymi słowy musi istnieć odporność w obliczeń (ponowne uruchomienie procesu), a także odporność w stanie lub danych (bez utraty danych i danych jest taki sam).

Problemy elastyczność jest rozliczana w innych sytuacjach, takich jak kiedy błędy wystąpić podczas uaktualniania aplikacji. Microservice, Praca z systemem wdrożenia, nie trzeba odzyskać. Należy również zdecydować, czy nadal można go przejść do przodu do nowszej wersji lub zamiast przywrócić poprzednią wersję do zachowania spójna. Pytania, takie jak czy istnieją za mało dostępnych do przodu zachowania przenoszenie komputerów i jak przywrócić poprzednie wersje o konieczności microservice należy wziąć pod uwagę. W tym celu microservice do wysyłania informacji o kondycji do mogli wprowadzać tych decyzji.

### <a name="reports-health-and-diagnostics"></a>Raporty dotyczące kondycji i diagnostyki

Może wydawać się oczywiste i często są pomijane, ale jest to, że microservice raporty zdrowia i diagnostyki. W przeciwnym razie jest nieco wglądu z perspektywy operacji. Korelacji zdarzeń diagnostycznych w zestawie niezależnych usług i zajmujących się pochyla zegar komputera rozsądnie kolejność zdarzeń jest trudne. W ten sam sposób interakcję microservice nad uzgodnione protokoły i formaty danych wynika potrzeba Normalizacyjna w jak rejestrować zdrowia i diagnostyczne zdarzenia, które ostatecznie trafiać w magazynie zdarzeń wykonywania kwerend i wyświetlania. W podejściu microservices jest klucz innym członkom zespołu uzgodnić format jednego rejestrowania.  Musi istnieć jednolity sposób wyświetlania zdarzeń diagnostycznych w aplikacji jako całość.

Kondycja różni się od narzędzia diagnostyczne. Kondycja jest microservice raportowania stanu bieżącego, aby wykonać odpowiednie czynności. Przykład współpracuje z mechanizmy uaktualnienia i wdrażania, aby utrzymać dostępność. Usługa może być obecnie nieprawidłowe ze względu na awarię proces lub uruchom ponownie komputer, ale nadal działania komputera. Ostatni element, potrzebne jest zapewnienie to zmniejszenie wykonując uaktualnienie. Najlepszym rozwiązaniem jest wykonanie dochodzenia najpierw lub czas microservice odzyskać. Kondycja wydarzenia z microservice zezwalają na świadome decyzje i, w celu utworzenia własny ran usług.

## <a name="service-fabric-as-a-microservices-platform"></a>Usługa tkaninie jako platformy microservices

Azure tkaninie usługi związane z firmy Microsoft przejście z przedstawiania pole produktów, które były zazwyczaj wbudowanymi w stylu, do przedstawiania usług. Środowisko konstrukcyjnych i świadczenia dużych usług, takich jak bazy danych programu SQL Azure i DocumentDB, kształt tkaninie usługi. Platformy utworzony na czasie przyjęta usług bardziej go. Należy pamiętać, że usługa tkaninie musiała uruchomić z dowolnego miejsca: nie tylko w Azure, ale również we wdrożeniach systemu Windows Server autonomicznego.

***W celu tkaninie usługi jest rozwiązywanie problemów twardym konstruowania i usługą i korzystanie z zasobów infrastruktury wydajność, umożliwiające członkom zespołu można rozwiązać problemy biznesowe przy użyciu metody microservices.***

Usługa tkaninie udostępnia dwa obszary ułatwia tworzenie aplikacji przy użyciu metody microservices:

- Platformę, która udostępnia usługi systemowe wdrażać, uaktualnianie, wykrywanie i uruchom ponownie nie powiodło się usług, poznawanie lokalizację usługi zarządzania stanem i monitorowanie kondycji. Te usługi systemu obowiązywać włączyć wiele cech microservices opisany.

-  Interfejsy API programowania lub RAM, ułatwia tworzenie aplikacji, co microservices: [zaufanego uczestników i niezawodne usługi](service-fabric-choose-framework.md). Oczywiście cały kod wyboru umożliwia tworzenie usługi microservice. Ale te interfejsy API wprowadź więcej proste zadania, a integrują platformie na poziomie szczegółowego. W ten sposób, na przykład stan i Diagnostyka informacji można znaleźć lub można korzystać z wbudowanych wysokiej dostępności.

***Usługa jest o niesprecyzowanym na sposób tworzenia usługi, a następnie można użyć innych technologii. Jednak zapewnia wbudowane programowania interfejsów API, które ułatwiają tworzenie microservices.***

### <a name="are-microservices-right-for-my-application"></a>Czy microservices prawa do mojej aplikacji?

Być może. Firma Microsoft wystąpił był, że jako bardziej zespołów w programie Microsoft rozpoczęła się do utworzenia chmury do pracy, wiele z nich realizowana zalety korzystających z metody microservice przypominających. Bing, na przykład występują zostały opracowywania microservices wyszukiwania lata. Dla innych zespołów podejście microservices było nowy. Znajdują które zostały twardym problemy do rozwiązania poza ich podstawowe kategorie siły. Jest to, dlatego usługa tkaninie uzyskane trakcji technologii wybór dotyczące tworzenia usług.

Celem materiału usługi jest zmniejszenie złożoności tworzenia aplikacji za pomocą metody microservice, dzięki czemu nie trzeba wykonywać jako wiele kosztów redesigns. Rozpoczynanie małych, skalowanie w razie potrzeby, zastąpić usług, dodawanie nowych i rozwijać z użyciem klienta — będącej podejście. Także dowiedzieć się, że w rzeczywistości istnieje wiele problemów jeszcze, które należy rozwiązać nawiązać microservices więcej przystępny dla deweloperów większości. Kontenerów i model programowania Aktor przedstawiają małymi krokami w tym kierunku, a firma Microsoft pewności, czy więcej innowacje pojawią się ułatwić.
 
<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Następne kroki

* Aby uzyskać więcej informacji:
    * [Omówienie terminologia tkaninie usługi](service-fabric-technical-overview.md)
    * [Microservices: Ma obrót aplikacji korzystająca z chmury](https://azure.microsoft.com/en-us/blog/microservices-an-application-revolution-powered-by-the-cloud/)


[Image1]: media/service-fabric-overview-microservices/monolithic-vs-micro.png
[Image2]: media/service-fabric-overview-microservices/statemonolithic-vs-micro.png
