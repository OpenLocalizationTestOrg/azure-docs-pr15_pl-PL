<properties
   pageTitle="Omówienie modelu programowania zaufanego tkaninie usługi | Microsoft Azure"
   description="Dowiedz się więcej o model programowania niezawodne usługi tkaninie usługi i rozpocząć pisanie własnych usług."
   services="Service-Fabric"
   documentationCenter=".net"
   authors="masnider"
   manager="timlt"
   editor="vturecek; mani-ramaswamy"/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="03/25/2016"
   ms.author="masnider;vturecek"/>

# <a name="reliable-services-overview"></a>Omówienie usług zaufanego
Azure tkaninie usługi upraszcza pisania i zarządzanie usługami zaufanego bezstanowe i stanowe. Ten dokument będzie porozmawiać o:

- Model programowania zaufania usługi dla usług bezstanowe i stanowe.
- Opcje, które należy związane z pisaniem niezawodne usługi.
- Niektóre scenariusze i przykłady podczas używania usług zaufanego i jak są zapisywane.

Niezawodne usługi jest jednym z modelach programowania dostępne na tkaninie usługi. Aby uzyskać więcej informacji na modelu programowania zaufanego uczestników zobacz [Wprowadzenie do uczestników zaufanego tkaninie usługi](service-fabric-reliable-actors-introduction.md).

W tkaninie usługi usługi składa się z konfiguracji, kod aplikacji i Województwo (opcjonalnie).

Usługa tkaninie zarządza ważności usług, z inicjowania obsługi administracyjnej i wdrożenia w ramach uaktualniania i usuwania za pośrednictwem [tkaninie usługi zarządzania aplikacjami](service-fabric-deploy-remove-applications.md).

## <a name="what-are-reliable-services"></a>Co to są usługi niezawodne?
Niezawodne usługi umożliwia prosty, skuteczny, najwyższego poziomu modelu programowania ułatwiające express, co to jest ważne do aplikacji. Z modelu programowania usług zaufanego zostanie wyświetlony komunikat:

- Stanowe usług modelu programowania usług zaufanego umożliwia spójne i niezawodne przechowywanie prawo stan wewnątrz usługi przy użyciu zaufanego zbiorów. To jest zestaw prosty klasy zbioru wysokiej dostępności, które jest znane każda osoba, która została użyta C# zbiorów. Zazwyczaj usługi potrzebne systemów zewnętrznych do zarządzania stanem zaufanego. Z zaufanego kolekcjami można przechowywać swój stan obok swojego obliczeń, z tym samym wysokiej dostępności i niezawodności zostały pochodzić oczekiwać sklepach zewnętrznych wysokiej dostępności i poprawiono dodatkowe opóźnienie dostarczających Współtworzenie lokalizowanie obliczeń i stan.

- Prostego modelu uruchamiania własny kod, który wygląda jak programowania modeli, którego użyto do. Kod ma punktu wejścia przejrzyste i łatwo zarządzanych cyklu życia.

- Model podłączany komunikacji. Za pomocą transportu wybranych przez użytkownika, takie jak HTTP z [Interfejsu API sieci Web](service-fabric-reliable-services-communication-webapi.md), WebSockets, niestandardowe protokoły TCP itp. Niezawodne usługi zapewniają niektóre doskonałe, opcje w nowym oknie można użyć lub Podaj własne.

## <a name="what-makes-reliable-services-different"></a>Co sprawia, że zaufania usługi innego?
Niezawodne usług w tkaninie usługi różni się od usług, które zostały zapisane przed. Usługi tkaninie zapewnia niezawodność, dostępność, spójności i skalowalność.  

- **Niezawodność**— usługi pozostanie w górę, nawet w mało wiarygodnych środowiska, w którym komputery może zakończyć się niepowodzeniem lub trafienie problemów z siecią.

- **Dostępność**— usługi będą dostępne i odpowiada. (To nie oznacza, że nie może mieć usług, których nie można odnaleźć lub osiągnięto z zewnątrz.)

- **Skalowalność**— usług są odłączona od sprzętu i ich powiększanie lub zmniejszanie stosownie do potrzeb przez dodanie lub usunięcie sprzętu lub zasobów wirtualnych. Usługi łatwo oddzielone od siebie (zwłaszcza w przypadku stanowe) zapewnienie niezależnych części usługi można skalować i odpowiadanie na błędy niezależnie. Na koniec tkaninie usługi zaleca usług być lightweight umożliwiając tysiące usług ma zostać zastrzeżona w pojedynczym procesie, zamiast wymaganie lub przypisanie całego wystąpienia OS jedno wystąpienie określonego obciążenie pracą.

- Zagwarantować **spójności**— informacje przechowywane w tej usłudze zachowanie spójności (dotyczy to tylko dla pełnowartościową usług — więcej w tym później)

## <a name="service-lifecycle"></a>Świadczenia usługi
Czy usługi jest stanowe lub bezpaństwowca, zaufanego usług zapewniają prosty cyklu życia, która pozwala szybko Podłącz w kodzie i rozpocząć pracę.  Istnieje tylko jednej lub dwóch metod, które należy zaimplementować uzyskanie usługi pracę.

- **CreateServiceReplicaListeners i CreateServiceInstanceListeners** — jest to miejsce, w którym usługę definiuje stosu komunikacji, który chce używać. Stos komunikacji, takie jak [Interfejs API sieci Web](service-fabric-reliable-services-communication-webapi.md), to, co definiuje słuchanie punktu końcowego lub punktów końcowych usługi (jak klienci będą mieli do niego dostępu). Definiuje również jak wiadomości, które są wyświetlane zakończenia interakcji z pozostałych kod usługi w górę.

- **RunAsync** — jest to miejsce, w którym usługa działa jego reguł biznesowych. Token anulowania, który znajduje się jest sygnałem dla po tej pracy należy zatrzymać. Na przykład jeśli masz usługę powinien stale pobrania wiadomości z zaufanego kolejki, a następnie procesu ich to miejsce, w którym się stanie, które działają.

### <a name="service-startup"></a>Uruchomienia usługi

Główne zdarzeń w cyklu świadczenia usługi zaufanego są następujące:

1. Obiekt usługi (elementu, który pochodzi od usługi bezstanowym lub stanowe) jest tworzona.

2. `CreateServiceReplicaListeners` / `CreateServiceInstanceListeners` Metoda jest wywoływana określeniu usługę szansy zwraca jeden lub więcej użytkowników komunikacji o wybór.
  - Należy zauważyć, że jest opcjonalne, mimo że większość usług będą uwidaczniane niektórych punktu końcowego bezpośrednio.

3. Po utworzeniu detektory komunikacji jest otwarty.
  - Słuchacze komunikacji mogą metodę o nazwie `OpenAsync()`, nazywanym na tym etapie i które zwraca słuchania adres usługi. Jeśli usługi zaufanego jest używany jeden z wbudowanych ICommunicationListeners, jest to obsługiwane dla Ciebie.

4. Po otwarciu odbiornika komunikacji `RunAsync()` metody na głównej usługi.
  - Należy zauważyć, że `RunAsync()` jest opcjonalna. Jeśli usługa nie wszystkie swojej pracy bezpośrednio w odpowiedzi na użytkownika tylko połączenia, nie jest wymagane w celu zaimplementowania `RunAsync()`.

### <a name="service-shutdown"></a>Zamykanie usługi

Gdy usługa jest zamykana (mają zostać usunięte, uaktualniony, przeniesione lub) kolejności połączenia jest zdublowany: najpierw token anulowania posiadanych przez `RunAsync()` jest anulowany; następnie `CloseAsync()` nosi nazwę na detektory komunikacji.

Istnieje kilka ważnych uwag o zamknięcie stanowe usług:

- Tkaninie usługi nie będą promować innej replice usługi podstawowego stanu do `CloseAsync` i `RunAsync` zwrócić. Jeśli korzystasz z listener wbudowanych komunikacji, `CloseAsync` metoda jest obsługiwane dla Ciebie.

- Wprawdzie na zwracanie z tych metod jest bez limitu czasu, od razu spowoduje utratę możliwości zapisu zaufanego zbiorów i dlatego nie można wykonać dowolną rzeczywistej pracy. Zaleca się powrót natychmiast po otrzymaniu żądania anulowania.

## <a name="example-services"></a>Przykład usług
Informacje o tym model programowania, Przyjrzyjmy się krótki przegląd dwóch różnych usług, aby zobaczyć, jak sztuk dopasowane.

### <a name="stateless-reliable-services"></a>Bezstanowa zaufania usługi
Bezstanowa usługi jest jeden, gdy jest dosłownie Państwa nie jest obsługiwane w usłudze lub stanu, w którym znajduje się całkowicie jednorazowe i nie wymaga synchronizacji, replikacji, trwałe lub wysokiej dostępności.

Na przykład można rozważyć kalkulatora pamięci i otrzymuje wszystkie terminy i czynności do wykonania na raz.

W tym przypadku RunAsync() usługi może być puste, ponieważ istnieje nie tła zadania przetwarzania usługę powinien zrobić. Po utworzeniu usługę kalkulatora zwróci ICommunicationListener (na przykład [Interfejs API sieci Web](service-fabric-reliable-services-communication-webapi.md)), który otwarty słuchanie punktu końcowego na niektórych portu. Ten słuchania punkt końcowy będzie Podłączanie do różnych metod (przykład: "Dodaj (n1, n2)") który Definiowanie publicznego interfejsu API kalkulatora.

Po nawiązaniu połączenia z klienta odpowiedniej metody, a usługa Kalkulator wykonuje operacje na danych dostarczonych i zwraca wynik. Go nie zawierają każde Państwo.

Nie są przechowywane stanów wewnętrznych sprawia, że ten Kalkulator przykład bardzo proste. Ale większości usług nie są naprawdę bezstanowym. Zamiast tego Zapisz ich stanu do niektórych innych magazynu. (Na przykład dowolnej aplikacji web app zależy od zachowanie stan sesji w sklepie kopii lub pamięci podręcznej nie jest całkowicie bezstanowym.)

Typowy przykład sposób stateless usługi są używane w usługi jest jako zewnętrzną, która udostępnia interfejs API publicznej dla aplikacji sieci web. Usługom następnie rozmawiają stanowe usług, aby ukończyć żądanie użytkownika. W tym przypadku połączeń z klientami są kierowane do znane portu, takich jak 80, gdzie oczekuje stateless usługi. Ta usługa stateless odbiera połączenie i określa, czy połączenie jest zaufany strony, jako także usługę jest przeznaczony do.  Następnie stateless usługi przesyłania dalej połączenie partycją poprawne stanowe usługi i czeka na odpowiedź. Gdy usługa stateless odbierze odpowiedź, odpowiedzi powrót do oryginalnego klienta.

### <a name="stateful-reliable-services"></a>Stanowe zaufania usługi
Usługa stanowe jest taki, który może zawierać część stan przechowywane spójne i znajdują się w kolejności usługi funkcji. Należy rozważyć, czy usługa, która oblicza stale stopniowe średnią pewne wartości w oparciu o aktualizacjach otrzymywanych. W tym celu musi on mieć bieżący zestaw przychodzących żądań, potrzebne do procesu, a także bieżącej wartości średniej. Wszelkie usługa, która pobiera, przetwarza i informacje są przechowywane w magazynie zewnętrznych (na przykład Azure obiektów blob lub tabeli magazyn dzisiaj) jest stanowe. Zachowuje tylko stanu w magazynie stan zewnętrznych.

Większości usług dzisiaj zawierają stanu zewnętrznie, ponieważ zewnętrznych magazyn jest, co zapewnia niezawodność, dostępność, skalowalność i spójność Państwa. Na tkaninie usługi stanowe usług nie jest wymagany do przechowywania stanu zewnętrznie; Usługa tkaninie trwa istotnych tych wymagań zarówno kod usługi i stanu usługi.

Załóżmy, że chcemy pisanie usługa, która przyjmuje żądania dla serii konwersji, które należy wykonać obrazu i obrazu, który ma zostać przekonwertowane.  Dla tej usługi zwróci odbiornika komunikacji (Załóżmy, że interfejs API sieci Web), otwiera port komunikacyjny, który umożliwia przesłanych elementów przy użyciu interfejsu API, takich jak `ConvertImage(Image i, IList<Conversion> conversions)`. W tym API usługi może uwzględnienia informacji i przechowywane żądanie w kolejce zaufanego i następnie zwrócenie niektórych token do klienta w celu go może zachować informacje o żądaniu (ponieważ żądania może zająć trochę czasu).

W tej usłudze RunAsync może być bardziej złożone. Usługa może mieć pętli wewnątrz jego RunAsync pobiera żądania poza IReliableQueue, wykonuje konwersji na liście i przechowuje wyniki w IReliableDictionary, tak, aby gdy klient nawiązuje ponownie, można uzyskiwać ich przekonwertowanych obrazów. Aby zapewnić, że nawet wtedy, gdy coś kończy się niepowodzeniem, obraz nie zostanie utracony, ta usługa zaufanego czy pobierają w kolejce, prowadzenie konwersje i zapisz wynik w transakcji. W tym przypadku wiadomość faktycznie zostanie usunięta tylko z kolejki, a wyniki są przechowywane w słowniku wynik po zakończeniu konwersji. Jeśli coś nie powiedzie się na środku (na przykład komputerze, na którym jest uruchomione to wystąpienie kodu), żądanie pozostaje w kolejce oczekujące na przetwarzanie ponownie.

Jedyne uwagi na temat tej usługi jest to brzmi jak normalnego działania usługi .NET. Jedyną różnicą jest są dostarczane przez usługę tkaninie struktury danych używane (IReliableQueue i IReliableDictionary), a więc zostały wprowadzone wysoce niezawodnych, dostępne i spójne.

## <a name="when-to-use-reliable-services-apis"></a>Kiedy należy używać zaufanego API usług
Jeśli dowolna z następujących charakteryzującą potrzebami usługi aplikacji, należy rozważyć zaufanego API usług:

- Należy podać zachowanie aplikacji przez wiele jednostek stan (np zamówienia i zamówienia pozycji).

- Stan usługi aplikacji można naturally zaprojektowany jako zaufanego słowniki i kolejek.

- Status musi być bardzo dostępne w programie access krótki czas oczekiwania.

- Tę aplikację do kontrolowania współbieżności lub szczegółowości transakcji operacji przez jeden lub więcej zbiorów zaufanego.

- Chcesz zarządzać komunikatów lub kontrolować schemat podziału tej usługi.

- Kod musi środowisku wątki runtime.

- Aplikacja musi dynamicznie tworzenia lub usuwania słowników wiarygodnych lub kolejki w czasie rzeczywistym.

- Musisz programowy kontrolować dostarczonego przez usługę tkaninie kopii zapasowych i przywracanie funkcje dla danej usługi stan *.

- Aplikacja należy zachować historii zmian dla jego jednostek stan *.

- Chcesz opracowania lub używanie opracowanych firmy innej niestandardowej stanie dostawców *.

> [AZURE.NOTE] * Funkcje dostępne na ogólnodostępną SDK.


## <a name="next-steps"></a>Następne kroki
+ [Niezawodne usług szybki start](service-fabric-reliable-services-quick-start.md)
+ [Niezawodne usługi Zaawansowane zastosowania](service-fabric-reliable-services-advanced-usage.md)
+ [Model programowania zaufanego uczestników](service-fabric-reliable-actors-introduction.md)
