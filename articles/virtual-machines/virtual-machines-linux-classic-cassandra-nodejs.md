<properties
    pageTitle="Uruchamianie Cassandra z systemem Linux Azure | Microsoft Azure"
    description="Jak uruchomić klaster Cassandra w systemie Linux w maszyn wirtualnych Azure za pomocą aplikacji Node.js"
    services="virtual-machines-linux"
    documentationCenter="nodejs"
    authors="hanuk"
    manager="wpickett"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="hanuk;robmcm"/>

# <a name="running-cassandra-with-linux-on-azure-and-accessing-it-from-nodejs"></a>Z Cassandra z systemem Linux Azure i uzyskiwanie dostępu do z Node.js

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Dowiedz się, jak [wykonać te kroki przy użyciu modelu Menedżera zasobów](https://azure.microsoft.com/documentation/templates/datastax-on-ubuntu/).

## <a name="overview"></a>Omówienie
Microsoft Azure jest platformą Otwórz chmury naśladujący oprogramowania również w innych firm, które zawiera systemów operacyjnych, serwerów aplikacji, pośredniczącym wiadomości, a także bazy danych SQL i NoSQL z obu modeli handlowe i Otwórz źródło zarówno do firmy Microsoft. Utworzenie mechanizm usług na publicznej chmury tym Azure wymaga uważnego planowania i architektury zamierzonego zarówno serwera aplikacji jako warstwy dobrze miejsca do magazynowania. Architektura rozproszonej pamięci Cassandra w sposób naturalny ułatwia tworzenie systemów wysokiej dostępności, które są odporność na uszkodzenia dla klastrów błędy. Cassandra jest skalą chmury NoSQL baza danych obsługiwana przez firmę Apache Software Foundation na cassandra.apache.org; Cassandra jest napisany w języku Java i w związku z tym działa na obu w systemie Windows, a także Linux platformach.

Ten artykuł jest Pokaż wdrożenia Cassandra na Ubuntu jako klaster jednej lub wielu danych Centrum używanie maszyn wirtualnych usługi Microsoft Azure i wirtualnych sieci. Wdrażanie klaster obciążenia produkcji zoptymalizowane wykracza poza zakres tego artykułu, jak wymaga konfiguracji węzeł wielu dysku, projekt topologii odpowiedniego pierścienia i modelowanie do obsługi replikacji potrzebne, spójności danych i przepustowość wymagań dotyczących wysokiej dostępności danych.

Ten artykuł ma podstawowe metody pokazanie co należy zrobić w tworzenia klaster Cassandra porównywane Docker, Kuchmistrz lub Puppet, które można rozmieszczanie infrastruktury znacznie ułatwić.  

## <a name="the-deployment-models"></a>Modele wdrażania
Sieć Microsoft Azure umożliwia wdrożenie odizolowanych klastrów prywatne, dostęp można ograniczyć do osiągnięcia zabezpieczeń sieci lub dostosowywanie kolorów.  Ten artykuł jest o wyświetlaniu wdrożenia Cassandra na poziomie podstawowych, firma Microsoft nie poświęcona poziomu spójności i projektowanie optymalnego magazynowania przepustowości. Oto lista sieciowych wymagania dotyczące naszych teoretyczna klaster:

- Systemy zewnętrzne nie może uzyskać dostępu Cassandra bazy danych z lub poza Azure
- Klaster Cassandra musi być za równoważenia obciążenia ruchu thrift
- Wdrażanie węzły Cassandra w dwóch grupach w każdym centrum danych dostępności klaster rozszerzone
- Zablokować klaster tak że tylko do farmy serwerów aplikacji bezpośrednio ma dostęp do bazy danych
- Nie publicznej sieci punkty końcowe niż SSH
- Każdy węzeł Cassandra wymaga stały wewnętrzny adres IP

Cassandra mogą być rozmieszczone w jeden z regionów Azure lub do wielu regionów w oparciu o charakter rozłożone obciążenie pracą. W przypadku wdrożenia modelu można użyć do służyć użytkownikom końcowym bliżej określonego Geografia za pośrednictwem infrastruktura Cassandra. Osoby Cassandra wbudowanych węzeł replikacji ma care synchronizacji wielu wzorzec zapisuje pochodzących z wielu centrach danych i przedstawia spójny widok danych do aplikacji. W przypadku wdrożenia służy łagodzenia ryzyka pełniejsza awarii usługi Azure. Osoby Cassandra ustawiane spójności i topologii replikacji może pomóc w spotkanie różne funkcje RPO aplikacji.

### <a name="single-region-deployment"></a>Wdrożenie jednego regionu
Firma Microsoft uruchomi rozmieszczenia pojedynczego regionu i zebrać learnings podczas tworzenia modelu wielu region. Azure wirtualnej sieci będzie używana do tworzenia podsieci odizolowanych spełnienia wymagań dotyczących zabezpieczeń sieciowych wymienionych powyżej.  Z procesu opisanego w tworzeniu wdrożenia jednego regionu używa KÓW 14.04 Ubuntu i Cassandra 2.08; Jednak proces można łatwo przyjąć inne warianty Linux. Poniżej przedstawiono niektóre właściwości systemowych wdrażania jednego regionu.  

**Wysokiej dostępności:** Węzły Cassandra pokazano na rysunku 1 są rozmieszczane dwa zestawy dostępność tak, aby węzły jest umieszczonych między wiele domen błędów wysokiej dostępności. Maszyny wirtualne odnoszący z każdego zestawu dostępności są mapowane do 2 domen błędów.  Przy użyciu programu Microsoft Azure pojęcia domeny błędów w celu zarządzania czasem niezaplanowane w dół (np. błędy sprzętu i oprogramowania) podczas pojęcia uaktualnienia domeny (host czy gość OS poprawki i aktualizacje, uaktualnienia aplikacji) jest używana do zarządzania zaplanowanego czasu. Zobacz [Odzyskiwanie i wysokiej dostępności aplikacji Azure](http://msdn.microsoft.com/library/dn251004.aspx) roli błędów i uaktualniania domen w celu osiągnięcia wysokiej dostępności.

![Wdrożenie jednego regionu](./media/virtual-machines-linux-classic-cassandra-nodejs/cassandra-linux1.png)

Rysunek 1: Wdrożenia jednego regionu

Uwaga podczas pisania tego dokumentu Azure nie zezwala jawnego mapowania grupy maszyny wirtualne do domeny określonej błędów; w związku z tym nawet w przypadku wdrożenia wzorem na rysunku 1, jest statystyczne prawdopodobieństwo, że wszystkie maszyn wirtualnych można zamapować na dwie domeny błędów zamiast czterech.

**Ruch Thrift równoważenia obciążenia:** Bibliotek klienta Thrift wewnątrz serwer sieci web połączyć się z klastrem za pośrednictwem usługi równoważenia obciążenia wewnętrzny. Wymaga to proces dodawania równoważenia obciążenia wewnętrznych do podsieci "dane" (zobacz rysunek 1) w kontekście hostingu klaster Cassandra usługi w chmurze. Gdy zdefiniowano równoważenia obciążenia wewnętrznych, każdy węzeł wymaga punkt końcowy równoważenia obciążenia do dodania z adnotacjami zestawu równoważenia obciążenia o nazwie równoważenia obciążenia wcześniej zdefiniowanych. Aby uzyskać więcej informacji, zobacz [Wewnętrzny równoważenia obciążenia Azure ](../load-balancer/load-balancer-internal-overview.md).

**Klaster nasion:** Należy zaznaczyć większość wysoce dostępne węzły ds nowe węzły będą komunikowanie się za pomocą węzły nasion do znalezienia topologii klaster. Jeden węzeł z każdego zestawu dostępność został wskazany jako węzły nasion Aby uniknąć pojedynczego punktu awarii.

**Czynniki z replikacją i spójność poziom:** Osoby Cassandra wbudowanego wysokiej dostępności i danych wytrzymałości jest określony przez współczynnik replikacji (Rosyjskiej - liczbę kopii każdy wiersz zapisany w klastrze) i spójność poziom (liczba operacji wstawiania być odczytywane/zapisywane cmd. wynik do osoby wywołującej danych). Czynniki z replikacją określono podczas tworzenia KEYSPACE (podobnie jak w relacyjnej bazie danych) określić poziom spójności podczas wydawania kwerendy OBSŁUGIWAŁ. Zobacz dokumentację dotyczącą Cassandra [Konfigurowanie spójności](http://www.datastax.com/documentation/cassandra/2.0/cassandra/dml/dml_config_consistency_c.html) szczegółowe spójności i formułę obliczeń kworum.

Cassandra obsługuje dwa typy modeli integralności danych — spójności i ewentualnego spójność; Czynniki z replikacją i spójność poziom razem określi, jeśli dane będą zgodne zaraz po ukończeniu operacji zapisu lub będzie ostatecznie spójne. Na przykład określając KWORUM, podobnie jak poziom spójności zawsze zapewnia danych spójności dowolnego poziomu spójności, pod numerem repliki są zapisywane w razie potrzeby do osiągnięcia KWORUM (np. jeden) wyników w danych po pewnym czasie zgodnych.

Klaster 8 węzłów powyższej ze wskaźnikiem replikacji 3 i KWORUM (2 węzły są odczytu lub zapisu spójności) odczytu/zapisu poziom spójności, czy po utracie teoretyczna najwyżej 1 węzła dla każdej grupy replikacji przed rozpoczęciem aplikacji awarii po raz pierwszy. Przyjęto założenie, że wszystkie spacje klucza jest również strategie odczytu/zapisu żądania.  Parametry, które użyjemy działającym klastrze są następujące:

Konfiguracja klaster Cassandra pojedynczy region:

| Parametr klaster | Wartość | Uwagi |
| ----------------- | ----- | ------- |
| Liczba węzłów (N) | 8   | Całkowita liczba węzłów w klastrze |
| Czynniki z replikacją (Rosyjskiej) | 3 | Liczba operacji wstawiania danego wiersza danych |
| Poziom spójności (Zapisz) | QUORUM[(RF/2) +1) = 2] wynikiem formuły jest zaokrąglana w dół | Zapisuje najwyżej 2 repliki przed wysłaniem odpowiedzi do rozmówcy; 3 replice są zapisywane w końcu spójny sposób. |
| Poziom spójności (do odczytu) | KWORUM [(RF/2) + 1 = 2] wynik formuły jest zaokrąglana w dół | Odczytuje repliki 2 przed wysłaniem odpowiedzi rozmówcy. |
| Strategia replikacji | NetworkTopologyStrategy zobacz [Replikacji danych](http://www.datastax.com/documentation/cassandra/2.0/cassandra/architecture/architectureDataDistributeReplication_c.html) w dokumentacji Cassandra, aby uzyskać więcej informacji | Zrozumienie topologię wdrażania i umieszcza repliki w węzłach tak, aby wszystkie repliki nie trafiają do tej samej szafy |
| Snitch | GossipingPropertyFileSnitch zobacz [Snitches](http://www.datastax.com/documentation/cassandra/2.0/cassandra/architecture/architectureSnitchesAbout_c.html) w dokumentacji Cassandra, aby uzyskać więcej informacji | NetworkTopologyStrategy używa pojęcia snitch w celu zrozumienia topologii. GossipingPropertyFileSnitch zapewnia lepszą kontrolę w mapowaniu każdy węzeł centrum danych i stojaków. Klaster używa następnie plotki propagowanie te informacje. Jest to znacznie upraszcza dynamiczne ustawienie IP względem PropertyFileSnitch |


**Azure zagadnienia związane z klaster Cassandra:** Funkcja maszyn wirtualnych usługi Microsoft Azure używa magazyn obiektów Blob platformy Azure utrzymywanie dysku; Azure magazynowania zapisuje 3 repliki każdego dysku dla wytrzymałości wysoki. Oznacza to, że każdy wiersz danych wstawione w tabeli Cassandra znajduje się już w repliki 3 i w związku z tym spójności danych jest już podjąć obsługę nawet wtedy, gdy współczynnik replikacji (Rosyjskiej) 1. Główny problem z czynniki replikacji jest liczba 1 jest aplikacji wystąpią przestojów nawet w przypadku awarii jednego węzła Cassandra. Jednak jeśli węzeł działa problemów (sprzęt, błędy oprogramowania systemu) rozpoznany przez kontroler tkaninie Azure, będzie obsługę nowego węzła w jego miejscu za pomocą samego dyski miejsca do magazynowania. Obsługa administracyjna nowy węzeł, aby zamienić starą może potrwać kilka minut.  Podobnie czynności planowanej konserwacji, jak zmiany systemu operacyjnego gościa, uaktualnienie Cassandra, i zmian w aplikacji kontroler tkaninie Azure wykonuje przewijanie uaktualnień węzły w klastrze.  Również uaktualnień stopniowych może potrwać kilka węzły w dół w danej chwili i w związku z tym klastrze mogą wystąpić krótkie przestoje kilka partycje. Jednak dane nie zostaną utracone z powodu wbudowanych nadmiarowości magazyn Azure.  

Dla systemów wdrożony Azure, która nie wymaga wysokiej dostępności (np. około 99,9, która odpowiada 8.76 rok godz; zobacz [Wysokiej dostępności](http://en.wikipedia.org/wiki/High_availability) , aby uzyskać szczegółowe informacje) można przeprowadzić z Rosyjskiej = 1 i poziomu spójności = jedną.  W przypadku aplikacji o wysokiej dostępności wymagań, Rosyjskiej = 3 i poziomu spójności = KWORUM dopuszczalnego czasu dół jednego z węzłów spośród repliki. Rosyjskiej = 1 w tradycyjnych wdrożeń (np. w wersji lokalnej) nie mogą być używane z powodu możliwości utraty danych wynikających z problemów, takie jak awarii dysku.   

## <a name="multi-region-deployment"></a>W przypadku wdrożenia
Osoby Cassandra pamiętać o danych — Centrum replikacji i spójność modelu powyższym opisem ułatwia wdrażania wielu region okno bez potrzeby dowolnego narzędzia zewnętrznych. To dość różni się od tradycyjnych relacyjnych baz danych gdzie ustawień odzwierciedlające bazy danych dla wielu wzorców zapisu może być dość złożone. Cassandra w regionie wielu konfigurowanie może pomóc w scenariusze użycia, łącznie z następujących czynności:

**Odległość podstawie wdrażania:** Aplikacje wielu dzierżawy, wyczyść mapowania użytkowników dzierżawy-do-regionu, można korzystali przez klaster w przypadku opóźnienia niski. Na przykład zarządzania nauki systemów dla instytucji edukacyjnych można wdrażać rozłożone klaster w regionach wschodniego Stanów Zjednoczonych i zachód USA służyć odpowiednich szkoły wyższe dla transakcji oraz analizy. Dane można lokalnie spójne w czasie Odczyt i zapis i może być ostatecznie spójny na obu regionów. Istnieją inne przykłady, takich jak rozkład multimediów, elektronicznego i nic, a wszystkie zasoby, które pełni użytkownika geo stężony podstawowej jest przypadek dobre wykorzystanie dla tego modelu wdrożenia.

**Wysokiej dostępności:** Nadmiarowości jest współczynnik klawiszy w celu osiągnięcia wysokiej dostępności oprogramowania i sprzętu; Aby uzyskać szczegółowe informacje, zobacz konstrukcyjnych zaufanego chmury systemów na Microsoft Azure. Na Microsoft Azure wyłącznie niezawodnych sposób osiągnięcia nadmiarowości PRAWDA jest wdrażając klaster wielu region. Aplikacje mogą być rozmieszczone w trybie aktywny aktywny i pasywne aktywne, a jeśli jeden z regionów nie działa, Menedżer ruchu Azure można przekierowywać ruch do aktywnego regionu.  Rozmieszczenia pojedynczego obszaru, w przypadku dostępności 99,9, wdrażania dwóch regionów może osiągać dostępność 99.9999 obliczane przez formułę: (1-(1-0.999) *(1-0.999))*100); Zobacz powyżej dokument, aby uzyskać szczegółowe informacje.

**Awarii:** W przypadku Cassandra klaster, odpowiednio zaprojektowane może wytrzymać dostawie centrum danych losowych. Jeśli jeden region nie działa, można uruchomić aplikacji wdrożony w innych regionach, serwowania użytkowników końcowych. Przykład wszelkich innych firm ciągłości implementacji aplikacja musi być na uszkodzenia dla niektórych utratą danych wynikających z danych w potoku asynchroniczne. Jednak Cassandra sprawia, że odzyskiwania znacznie usprawnić niż czas trwania procesów odzyskiwania tradycyjnych bazy danych. Rysunek 2 pokazuje modelu Typowe rozmieszczanie wielu region z osiem węzłów w każdym regionie. Dla obu regionów są to obrazy lustrzane każdego z pozostałych dla tego samego symetrii; projekty rzeczywistych zależą od typ obciążenia pracą (np. transakcji lub analitycznych), RPO, RTO, spójności danych i wymagań dotyczących dostępności.

![Wielokrotne region wdrażania](./media/virtual-machines-linux-classic-cassandra-nodejs/cassandra-linux2.png)

Rysunek 2: W przypadku Cassandra wdrożenia

### <a name="network-integration"></a>Integracja z siecią
Zestawy maszyn wirtualnych używany do sieci prywatnych znajdującej się na dwóch regionów komunikuje się ze sobą za pomocą tunelem VPN. Tunelem VPN łączy dwie bramy oprogramowania obsługi administracyjnej podczas procesu wdrażania sieci. Dla obu regionów mają podobne architektury sieci w odniesieniu do podsieci "web" i "dane"; Azure sieci umożliwia tworzenie możliwie jak najwięcej podsieci zgodnie z potrzebami i stosowanie ACL odpowiednio zabezpieczeń sieci. Podczas projektowania topologii klaster między opóźnienie komunikacji centrum danych i ekonomicznych wpływu na ruch sieciowy muszą być traktowane jako.

### <a name="data-consistency-for-multi-data-center-deployment"></a>Spójności danych do wdrożenia centrum danych wielu elementów
Rozpowszechniane na potrzeby wdrożeń obowiązujących klaster topologii wpływ na przepustowość i wysokiej dostępności. Rosyjskiej i spójność poziom muszą być zaznaczone w taki sposób, że kworum nie zależy od dostępność wszystkich centrów danych.
Systemu, który wymaga spójności wysoki LOCAL_QUORUM spójności poziomu (Odczyt i zapis) będzie upewnij się, że lokalne Odczyt i zapis są spełnione z lokalnym węzłów podczas asynchroniczne replikacji danych do centrów zdalnych danych.  Tabela 2 zawiera podsumowanie szczegóły konfiguracji klaster w przypadku opisane w dalszej części zapisu w górę.

**Konfiguracja klaster Cassandra dwóch regionów**


| Parametr klaster | Wartość | Uwagi |
| ----------------- | ----- | ------- |
| Liczba węzłów (N) | 8 + 8 | Całkowita liczba węzłów w klastrze |
| Czynniki z replikacją (Rosyjskiej) | 3 | Liczba operacji wstawiania danego wiersza danych |
| Poziom spójności (Zapisz) | LOCAL_QUORUM [(sum(RF)/2) +1) = 4] wynik formuły jest zaokrąglana w dół | 2 węzły zostaną zapisane w centrum danych pierwszego synchronicznie; dodatkowe węzły 2 wymagane przez kworum zostaną zapisane asynchroniczne 2 centrum danych. |
| Poziom spójności (do odczytu) | LOCAL_QUORUM ((RF/2) + 1) = 2 wynik formuły jest zaokrąglana w dół | Żądania odczytu są spełnione z tylko jedną regionu; 2 węzły są odczytywane przed wysłaniem odpowiedzi do klienta. |
| Strategia replikacji | NetworkTopologyStrategy zobacz [Replikacji danych](http://www.datastax.com/documentation/cassandra/2.0/cassandra/architecture/architectureDataDistributeReplication_c.html) w dokumentacji Cassandra, aby uzyskać więcej informacji | Zrozumienie topologię wdrażania i umieszcza repliki w węzłach tak, aby wszystkie repliki nie trafiają do tej samej szafy |
| Snitch | GossipingPropertyFileSnitch zobacz [Snitches](http://www.datastax.com/documentation/cassandra/2.0/cassandra/architecture/architectureSnitchesAbout_c.html) w dokumentacji Cassandra, aby uzyskać więcej informacji | NetworkTopologyStrategy używa pojęcia snitch w celu zrozumienia topologii. GossipingPropertyFileSnitch zapewnia lepszą kontrolę w mapowaniu każdy węzeł centrum danych i stojaków. Klaster używa następnie plotki propagowanie te informacje. Jest to znacznie upraszcza dynamiczne ustawienie IP względem PropertyFileSnitch |


##<a name="the-software-configuration"></a>KONFIGURACJA OPROGRAMOWANIA
Następujące wersje oprogramowania są używane podczas wdrażania:

<table>
<tr><th>Oprogramowanie</th><th>Źródła</th><th>Wersja</th></tr>
<tr><td>JRE </td><td>[JRE 8](http://www.oracle.com/technetwork/java/javase/downloads/server-jre8-downloads-2133154.html) </td><td>8U5</td></tr>
<tr><td>JNA </td><td>[JNA](https://github.com/twall/jna) </td><td> 3.2.7</td></tr>
<tr><td>Cassandra</td><td>[Apache Cassandra 2.0.8](http://www.apache.org/dist/cassandra/2.0.8/apache-cassandra-2.0.8-bin.tar.gz)</td><td> 2.0.8</td></tr>
<tr><td>Ubuntu  </td><td>[Microsoft Azure](https://azure.microsoft.com/) </td><td>14.04 KÓW</td></tr>
</table>

Ponieważ pobieranie JRE wymaga ręcznego akceptacji Oracle licencji, aby wdrażaniu, pobrać wymaganego oprogramowania na pulpicie do późniejszego przekazywania do Ubuntu obraz szablonu, który firma Microsoft zostanie utworzony jako macierzystych do wdrażania klastrów.

Pobieranie powyżej oprogramowania w katalogu pobierania znanego (np. %TEMP%/downloads w systemie Windows lub ~/Downloads na większości dystrybucji Linux lub Mac) na komputerze lokalnym.

### <a name="create-ubuntu-vm"></a>TWORZENIE MASZYN WIRTUALNYCH UBUNTU
W tym kroku procesu Ubuntu obrazu zostanie utworzony przy użyciu wstępnie wymagane oprogramowanie tak, aby obraz można ponownie inicjowania obsługi administracyjnej kilka węzłów Cassandra.  
####<a name="step-1-generate-ssh-key-pair"></a>Krok 1: Wygenerować SSH parę klucza
Azure musi X509 klucz publiczny PEM lub DER zakodowany podczas inicjowania obsługi administracyjnej. Generowanie pary kluczy publicznych i prywatnych, zgodnie z instrukcjami zawartymi w sposób użycia SSH z systemem Linux Azure. Jeśli zamierzasz użyć putty.exe jako klienta SSH albo w systemach Windows i Linux, należy przekonwertować PEM zakodowany klucz prywatny RSA format PPK przy użyciu puttygen.exe; instrukcje dotyczące to można znaleźć na stronie sieci web powyżej.

####<a name="step-2-create-ubuntu-template-vm"></a>Krok 2: Tworzenie szablonu Ubuntu maszyn wirtualnych
Aby utworzyć szablon maszyn wirtualnych, zaloguj się do portalu klasyczny Azure i użyć następującej procedury: kliknij pozycję Nowy, obliczeń, maszyn wirtualnych, z GALERII, UBUNTU, Ubuntu serwera 14.04 KÓW, a następnie kliknij strzałkę w prawo. Samouczek opisujący sposób tworzenia maszyny Linux Zobacz Tworzenie maszyn wirtualnych systemem Linux.

Na ekranie "konfigurację maszyny wirtualnej" #1, wprowadź następujące informacje:

<table>
<tr><th>NAZWA POLA              </td><td>       WARTOŚĆ POLA               </td><td>         UWAGI                </td><tr>
<tr><td>DATA WYDANIA WERSJI    </td><td> Wybierz datę z drow w dół</td><td></td><tr>
<tr><td>NAZWA MASZYN WIRTUALNYCH    </td><td> Szablon przypadku                </td><td> Jest to nazwa hosta maszyn wirtualnych </td><tr>
<tr><td>WARSTWY                     </td><td> STANDARDOWE                        </td><td> Pozostaw domyślne              </td><tr>
<tr><td>ROZMIAR                     </td><td> A1                              </td><td>Wybierz pozycję maszyn wirtualnych, w zależności od potrzeb Jo; w tym celu Pozostaw domyślne </td><tr>
<tr><td> NOWĄ NAZWĘ UŻYTKOWNIKA           </td><td> localadmin                      </td><td> "Administrator" jest nazwą użytkownika zastrzeżone w xx 12 Ubuntu i po</td><tr>
<tr><td> UWIERZYTELNIANIE      </td><td> Kliknij pole wyboru                 </td><td>Sprawdź, czy chcesz zabezpieczyć przy użyciu klucza SSH </td><tr>
<tr><td> CERTYFIKAT             </td><td> Nazwa pliku certyfikatu klucza publicznego </td><td> Klucz publiczny wcześniej wygenerowane za pomocą</td><tr>
<tr><td> Nowe hasło   </td><td> silnych haseł </td><td> </td><tr>
<tr><td> Potwierdź hasło   </td><td> silnych haseł </td><td></td><tr>
</table>

Na ekranie "konfigurację maszyny wirtualnej" #2, wprowadź następujące informacje:

<table>
<tr><th>NAZWA POLA             </th><th> WARTOŚĆ POLA                       </th><th> UWAGI                                 </th></tr>
<tr><td> USŁUGA W CHMURZE  </td><td> Tworzenie nowej usługi w chmurze    </td><td>Usługa w chmurze jest zasobów do obliczeń kontenera, takich jak maszyn wirtualnych</td></tr>
<tr><td> NAZWA DNS USŁUGI W CHMURZE </td><td>ubuntu template.cloudapp.net   </td><td>Nadaj nazwę równoważenia obciążenia o niesprecyzowanym komputera</td></tr>
<tr><td> REGION/GRUPY KOLIGACJI/WIRTUALNEJ SIECI </td><td>    Zachód Stany Zjednoczone </td><td> Wybierz region, w którym dostęp do aplikacji sieci web klaster Cassandra</td></tr>
<tr><td>KONTO MIEJSCA DO MAGAZYNOWANIA </td><td>   Użyj domyślnych </td><td>Za pomocą domyślnego konta miejsca do magazynowania lub konta wstępnie zaprojektowanych miejsca do magazynowania w wybranym obszarze</td></tr>
<tr><td>KONFIGUROWANIE DOSTĘPNOŚCI </td><td>  Brak </td><td>  Pozostaw to pole puste</td></tr>
<tr><td>PUNKTY KOŃCOWE   </td><td>Użyj domyślnych </td><td>  Używanie konfiguracji SSH domyślne </td></tr>
</table>

Kliknij strzałkę w prawo, pozostaw ustawienia domyślne na ekranie #3, a następnie kliknij przycisk "wyboru", aby zakończyć maszyn wirtualnych, proces tworzenia. Po upływie kilku minut maszyn wirtualnych za pomocą nazwy "ubuntu szablonu" powinny być w stanie "uruchomiony".

###<a name="install-the-necessary-software"></a>INSTALOWANIE OPROGRAMOWANIA NIEZBĘDNE
####<a name="step-1-upload-tarballs"></a>Krok 1: Przekazywania tarballs
Za pomocą połączenia lub pscp, skopiuj pobrane wcześniej oprogramowanie do katalogu ~/downloads w następującym formacie polecenia:

#####<a name="pscp-server-jre-8u5-linux-x64targz-localadminhk-cas-templatecloudappnethomelocaladmindownloadsserver-jre-8u5-linux-x64targz"></a>pscp server — jre-8u5-linux-x64.tar.gzlocaladmin@hk-cas-template.cloudapp.net:/home/localadmin/downloads/server-jre-8u5-linux-x64.tar.gz

Powtórz powyższe polecenie dla JRE także dla bitów Cassandra.

####<a name="step-2-prepare-the-directory-structure-and-extract-the-archives"></a>Krok 2: Przygotowywanie strukturę katalogów i wyodrębnianie archiwum
Zaloguj się do maszyn wirtualnych i utworzyć strukturę katalogów i wyodrębnianie oprogramowania jako administratorów, za pomocą skryptu imprezie poniżej:

    #!/bin/bash
    CASS_INSTALL_DIR="/opt/cassandra"
    JRE_INSTALL_DIR="/opt/java"
    CASS_DATA_DIR="/var/lib/cassandra"
    CASS_LOG_DIR="/var/log/cassandra"
    DOWNLOADS_DIR="~/downloads"
    JRE_TARBALL="server-jre-8u5-linux-x64.tar.gz"
    CASS_TARBALL="apache-cassandra-2.0.8-bin.tar.gz"
    SVC_USER="localadmin"

    RESET_ERROR=1
    MKDIR_ERROR=2

    reset_installation ()
    {
       rm -rf $CASS_INSTALL_DIR 2> /dev/null
       rm -rf $JRE_INSTALL_DIR 2> /dev/null
       rm -rf $CASS_DATA_DIR 2> /dev/null
       rm -rf $CASS_LOG_DIR 2> /dev/null
    }
    make_dir ()
    {
       if [ -z "$1" ]
       then
          echo "make_dir: invalid directory name"
          exit $MKDIR_ERROR
       fi

       if [ -d "$1" ]
       then
          echo "make_dir: directory already exists"
          exit $MKDIR_ERROR
       fi

       mkdir $1 2>/dev/null
       if [ $? != 0 ]
       then
          echo "directory creation failed"
          exit $MKDIR_ERROR
       fi
    }

    unzip()
    {
       if [ $# == 2 ]
       then
          tar xzf $1 -C $2
       else
          echo "archive error"
       fi

    }

    if [ -n "$1" ]
    then
       SVC_USER=$1
    fi

    reset_installation
    make_dir $CASS_INSTALL_DIR
    make_dir $JRE_INSTALL_DIR
    make_dir $CASS_DATA_DIR
    make_dir $CASS_LOG_DIR

    #unzip JRE and Cassandra
    unzip $HOME/downloads/$JRE_TARBALL $JRE_INSTALL_DIR
    unzip $HOME/downloads/$CASS_TARBALL $CASS_INSTALL_DIR

    #Change the ownership to the service credentials

    chown -R $SVC_USER:$GROUP $CASS_DATA_DIR
    chown -R $SVC_USER:$GROUP $CASS_LOG_DIR
    echo "edit /etc/profile to add JRE to the PATH"
    echo "installation is complete"


Jeśli wklejasz ten skrypt do okna vim, upewnij się, że usuwanie powrotu karetki ("\r") przy użyciu następującego polecenia:

    tr -d '\r' <infile.sh >outfile.sh

####<a name="step-3-edit-etcprofile"></a>Krok 3: Itp i profilu w trybie edycji
Dołącz na końcu następujące czynności:

    JAVA_HOME=/opt/java/jdk1.8.0_05
    CASS_HOME= /opt/cassandra/apache-cassandra-2.0.8
    PATH=$PATH:$HOME/bin:$JAVA_HOME/bin:$CASS_HOME/bin
    export JAVA_HOME
    export CASS_HOME
    export PATH

####<a name="step-4-install-jna-for-production-systems"></a>Krok 4: Zainstaluj JNA dla systemów produkcji
Należy użyć następującej polecenia: następujące polecenie zainstaluje jna 3.2.7.jar i jna platformy 3.2.7.jar do katalogu /usr/share.java sudo stanie get zainstalować libjna java

Tworzenie łącza symbolicznego w katalogu CASS_HOME/Biblioteka $ tak, aby Cassandra skrypt uruchamiania można znaleźć te słoików:

    ln -s /usr/share/java/jna-3.2.7.jar $CASS_HOME/lib/jna.jar

    ln -s /usr/share/java/jna-platform-3.2.7.jar $CASS_HOME/lib/jna-platform.jar

####<a name="step-5-configure-cassandrayaml"></a>Krok 5: Konfigurowanie cassandra.yaml
Edytowanie cassandra.yaml na poszczególnych maszyn wirtualnych, aby odzwierciedlała konfiguracji wymagane przez wszystkich maszyn wirtualnych [firma Microsoft będzie dostosować a to podczas rzeczywisty obsługi administracyjnej]:

<table>
<tr><th>Nazwa pola   </th><th> Wartość  </th><th> Uwagi </th></tr>
<tr><td>nazwa_klastra </td><td>  "CustomerService"   </td><td> Użyj nazwy odwołującej się do wdrożenia</td></tr>
<tr><td>listen_address  </td><td>[pozostaw to pole puste]   </td><td> Usuwanie "hosta lokalnego" </td></tr>
<tr><td>rpc_addres   </td><td>[pozostaw to pole puste]  </td><td> Usuwanie "hosta lokalnego" </td></tr>
<tr><td>nasiona   </td><td>"10.1.2.4, 10.1.2.6, 10.1.2.8" </td><td>Lista wszystkich adresów IP, które są oznaczone jako dane.</td></tr>
<tr><td>endpoint_snitch </td><td> org.apache.cassandra.locator.GossipingPropertyFileSnitch </td><td> To jest używany przez NetworkTopologyStrateg dla wnioskowanie centrum danych i szafy maszyn wirtualnych</td></tr>
</table>

####<a name="step-6-capture-the-vm-image"></a>Krok 6: Przechwycić obraz maszyn wirtualnych
Zaloguj się do maszyny wirtualnej przy użyciu hostname (hk — urzędów certyfikacji template.cloudapp.net) i klucz prywatny SSH utworzone wcześniej. Zobacz jak Użyj SSH z systemem Linux Azure, aby uzyskać szczegółowe informacje na temat zalogować się przy użyciu polecenia ssh lub putty.exe.

Wykonaj akcje, aby przechwycić obraz następującej:
#####<a name="1-deprovision"></a>1. deprovision
Użyj polecenia "sudo waagent — deprovision + użytkownika" Aby usunąć maszyn wirtualnych wystąpienia określonych informacji. Zobacz [Jak przechwytywać maszyny wirtualnej Linux](virtual-machines-linux-classic-capture-image.md) użycia jako szablonu więcej szczegółowych informacji na przechwytywanie obrazu.

#####<a name="2-shutdown-the-vm"></a>2: zamknięcia maszyn wirtualnych
Upewnij się, że maszyny wirtualnej jest wyróżniona, a następnie kliknij łącze zamknięcia na dolnym pasku poleceń.

#####<a name="3-capture-the-image"></a>3: Przechwytywanie obrazu
Upewnij się, że maszyny wirtualnej jest wyróżniona, a następnie kliknij łącze PRZECHWYTYWANIA na dolnym pasku poleceń. Na następnym ekranie Określ nazwę obrazu (np. hk-cas-2-08-ub-14-04-2014071), odpowiednio opis obrazu, a następnie kliknij znacznik "wyboru", aby zakończyć proces PRZECHWYTYWANIA.

To może potrwać kilka sekund, a następnie w sekcji Moje obrazy galerii obrazu obraz powinna być dostępna. Źródło maszyn wirtualnych będzie automatycznie delated po pomyślnie jest rejestrowany obrazu.

##<a name="single-region-deployment-process"></a>Proces wdrażania jednego regionu
**— Krok 1: Tworzenie wirtualnych sieci** Zaloguj się do portalu klasyczny Azure i tworzenie wirtualnej sieci przy użyciu Pokaż atrybuty w tabeli. Aby uzyskać szczegółowe instrukcje procesu, zobacz temat [Konfigurowanie wirtualnej sieci Cloud-Only w portalu klasyczny Azure](../virtual-network/virtual-networks-create-vnet-classic-portal.md) .      

<table>
<tr><th>Nazwa atrybutu maszyn wirtualnych</th><th>Wartość</th><th>Uwagi</th></tr>
<tr><td>Nazwa</td><td>vnet przypadku zachód us</td><td></td></tr>
<tr><td>Region</td><td>Zachód Stany Zjednoczone</td><td></td></tr>
<tr><td>Serwery DNS </td><td>Brak</td><td>Ignoruj tak, jak firma Microsoft nie używają serwera DNS</td></tr>
<tr><td>Konfigurowanie sieci VPN punktu do witryny</td><td>Brak</td><td> Ignoruj to</td></tr>
<tr><td>Konfigurowanie sieci VPN witryny do witryny</td><td>Nnone</td><td> Ignoruj to</td></tr>
<tr><td>Przestrzeń adresów</td><td>10.1.0.0/16</td><td></td></tr>
<tr><td>Uruchamianie IP</td><td>10.1.0.0</td><td></td></tr>
<tr><td>CIDR </td><td>-16 (65531)</td><td></td></tr>
</table>

Dodaj następujące podsieci:

<table>
<tr><th>Nazwa</th><th>Uruchamianie IP</th><th>CIDR</th><th>Uwagi</th></tr>
<tr><td>w sieci Web</td><td>10.1.1.0</td><td>-24 (251)</td><td>Podsieć farma sieci web</td></tr>
<tr><td>dane</td><td>10.1.2.0</td><td>-24 (251)</td><td>Podsieć dla węzłów bazy danych</td></tr>
</table>

Dane i podsieci Web mogą być chronione za pomocą grup zabezpieczeń sieci zakres znajduje się poza zakresem, w tym artykule.  

**— Krok 2: Inicjowanie obsługi maszyn wirtualnych** Za pomocą obrazu utworzonego wcześniej, możemy utworzy następujące maszyn wirtualnych na serwerze chmury "hk-c-svc Zachód" i powiązać je do odpowiednich podsieci, tak jak pokazano poniżej:

<table>
<tr><th>Nazwa komputera    </th><th>Podsieć </th><th>Adres IP </th><th>Konfigurowanie dostępności</th><th>Kontrolera domeny i stojaków</th><th>Zapełnić?</th></tr>
<tr><td>HK-c1-zachód us   </td><td>dane   </td><td>10.1.2.4   </td><td>HK-c-aset-1    </td><td>kontrolera domeny = stojaków WESTUS = rack1 </td><td>Tak</td></tr>
<tr><td>HK-c2-zachód us   </td><td>dane   </td><td>10.1.2.5   </td><td>HK-c-aset-1    </td><td>kontrolera domeny = stojaków WESTUS = rack1 </td><td>Brak </td></tr>
<tr><td>HK-c3-zachód us   </td><td>dane   </td><td>10.1.2.6   </td><td>HK-c-aset-1    </td><td>kontrolera domeny = stojaków WESTUS = rack2 </td><td>Tak</td></tr>
<tr><td>HK-c4-zachód us   </td><td>dane   </td><td>10.1.2.7   </td><td>HK-c-aset-1    </td><td>kontrolera domeny = stojaków WESTUS = rack2 </td><td>Brak </td></tr>
<tr><td>HK-c5-zachód us   </td><td>dane   </td><td>10.1.2.8   </td><td>HK-c-aset-2    </td><td>kontrolera domeny = stojaków WESTUS = rack3 </td><td>Tak</td></tr>
<tr><td>HK — c6-zachód us   </td><td>dane   </td><td>10.1.2.9   </td><td>HK-c-aset-2    </td><td>kontrolera domeny = stojaków WESTUS = rack3 </td><td>Brak </td></tr>
<tr><td>HK-c7-zachód us   </td><td>dane   </td><td>10.1.2.10  </td><td>HK-c-aset-2    </td><td>kontrolera domeny = stojaków WESTUS = rack4 </td><td>Tak</td></tr>
<tr><td>HK-c8-zachód us   </td><td>dane   </td><td>10.1.2.11  </td><td>HK-c-aset-2    </td><td>kontrolera domeny = stojaków WESTUS = rack4 </td><td>Brak </td></tr>
<tr><td>HK — w1-zachód us   </td><td>w sieci Web    </td><td>10.1.1.4   </td><td>HK-w-aset-1    </td><td>                       </td><td>N/D!</td></tr>
<tr><td>HK — w2-zachód us   </td><td>w sieci Web    </td><td>10.1.1.5   </td><td>HK-w-aset-1    </td><td>                       </td><td>N/D!</td></tr>
</table>

Tworzenie listy powyżej maszyny wirtualne wymaga następującym procesem:

1.  Tworzenie usługi w chmurze pustego w wybranym obszarze
2.  Tworzenie maszyn wirtualnych z wcześniej przechwycony obraz i dołączyć go do wirtualnej sieci utworzone wcześniej; Powtórz tę czynność dla wszystkich maszyny wirtualne
3.  Dodawanie usługi równoważenia obciążenia wewnętrznych do usługi w chmurze i dołączyć go do podsieci "dane"
4.  Każdy utworzony wcześniej Głosowa Dodaj punkt końcowy równoważenia obciążenia ruchu thrift zestawu równoważenia obciążenia równoważenia obciążenia wewnętrznych poprzednio utworzone połączenie

Powyższego procesu mogą być wykonywane za pomocą portalu klasyczny Azure; komputer systemu Windows (Użyj maszyn wirtualnych Azure, jeśli nie masz dostęp do komputera systemu Windows), używać tego skryptu programu PowerShell do zapewniania obsługi wszystkich 8 maszyny wirtualne automatycznie.

**Lista 1: Skrypt programu PowerShell dla inicjowania obsługi administracyjnej maszyn wirtualnych**

        #Tested with Azure Powershell - November 2014
        #This powershell script deployes a number of VMs from an existing image inside an Azure region
        #Import your Azure subscription into the current Powershell session before proceeding
        #The process: 1. create Azure Storage account, 2. create virtual network, 3.create the VM template, 2. crate a list of VMs from the template

        #fundamental variables - change these to reflect your subscription
        $country="us"; $region="west"; $vnetName = "your_vnet_name";$storageAccount="your_storage_account"
        $numVMs=8;$prefix = "hk-cass";$ilbIP="your_ilb_ip"
        $subscriptionName = "Azure_subscription_name";
        $vmSize="ExtraSmall"; $imageName="your_linux_image_name"
        $ilbName="ThriftInternalLB"; $thriftEndPoint="ThriftEndPoint"

        #generated variables
        $serviceName = "$prefix-svc-$region-$country"; $azureRegion = "$region $country"

        $vmNames = @()
        for ($i=0; $i -lt $numVMs; $i++)
        {
           $vmNames+=("$prefix-vm"+($i+1) + "-$region-$country" );
        }

        #select an Azure subscription already imported into Powershell session
        Select-AzureSubscription -SubscriptionName $subscriptionName -Current
        Set-AzureSubscription -SubscriptionName $subscriptionName -CurrentStorageAccountName $storageAccount

        #create an empty cloud service
        New-AzureService -ServiceName $serviceName -Label "hkcass$region" -Location $azureRegion
        Write-Host "Created $serviceName"

        $VMList= @()   # stores the list of azure vm configuration objects
        #create the list of VMs
        foreach($vmName in $vmNames)
        {
           $VMList += New-AzureVMConfig -Name $vmName -InstanceSize ExtraSmall -ImageName $imageName |
           Add-AzureProvisioningConfig -Linux -LinuxUser "localadmin" -Password "Local123" |
           Set-AzureSubnet "data"
        }

        New-AzureVM -ServiceName $serviceName -VNetName $vnetName -VMs $VMList

        #Create internal load balancer
        Add-AzureInternalLoadBalancer -ServiceName $serviceName -InternalLoadBalancerName $ilbName -SubnetName "data" -StaticVNetIPAddress "$ilbIP"
        Write-Host "Created $ilbName"
        #Add add the thrift endpoint to the internal load balancer for all the VMs
        foreach($vmName in $vmNames)
        {
            Get-AzureVM -ServiceName $serviceName -Name $vmName |
                Add-AzureEndpoint -Name $thriftEndPoint -LBSetName "ThriftLBSet" -Protocol tcp -LocalPort 9160 -PublicPort 9160 -ProbePort 9160 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 -InternalLoadBalancerName $ilbName |
                Update-AzureVM

            Write-Host "created $vmName"     
        }

**Krok 3: Konfigurowanie Cassandra w każdym maszyn wirtualnych**

Zaloguj się do maszyn wirtualnych i wykonaj następujące czynności:

* Edytuj $CASS_HOME/conf/cassandra-rackdc.properties Aby określić właściwości Centrum i stojaków danych:

       dc =EASTUS, rack =rack1

* Edytowanie cassandra.yaml do konfigurowania węzłów nasion, jak pokazano poniżej:

       Seeds: "10.1.2.4,10.1.2.6,10.1.2.8,10.1.2.10"

**Krok 4: Uruchom maszyny wirtualne i przetestuj klaster**

Zaloguj się do jednego z węzłów (np. hk-c1-zachód nami) i uruchom następujące polecenie, aby wyświetlić stan klaster:

       nodetool –h 10.1.2.4 –p 7199 status

Powinien zostać wyświetlony ekran podobny do tego poniżej dla klastrów węzeł 8:

<table>
<tr><th>Stan</th><th>Adres  </th><th>Ładowanie   </th><th>Tokeny </th><th>Właścicielem </th><th>Identyfikator hosta  </th><th>Stojaków</th></tr>
<tr><th>WYCZYŚĆ  </td><td>10.1.2.4   </td><td>87.81 KB   </td><td>256    </td><td>38.0%  </td><td>Identyfikator GUID (usunięte)</td><td>rack1</td></tr>
<tr><th>WYCZYŚĆ  </td><td>10.1.2.5   </td><td>41.08 KB   </td><td>256    </td><td>68.9%  </td><td>Identyfikator GUID (usunięte)</td><td>rack1</td></tr>
<tr><th>WYCZYŚĆ  </td><td>10.1.2.6   </td><td>55.29 KB   </td><td>256    </td><td>68.8%  </td><td>Identyfikator GUID (usunięte)</td><td>rack2</td></tr>
<tr><th>WYCZYŚĆ  </td><td>10.1.2.7   </td><td>55.29 KB   </td><td>256    </td><td>68.8%  </td><td>Identyfikator GUID (usunięte)</td><td>rack2</td></tr>
<tr><th>WYCZYŚĆ  </td><td>10.1.2.8   </td><td>55.29 KB   </td><td>256    </td><td>68.8%  </td><td>Identyfikator GUID (usunięte)</td><td>rack3</td></tr>
<tr><th>WYCZYŚĆ  </td><td>10.1.2.9   </td><td>55.29 KB   </td><td>256    </td><td>68.8%  </td><td>Identyfikator GUID (usunięte)</td><td>rack3</td></tr>
<tr><th>WYCZYŚĆ  </td><td>10.1.2.10  </td><td>55.29 KB   </td><td>256    </td><td>68.8%  </td><td>Identyfikator GUID (usunięte)</td><td>rack4</td></tr>
<tr><th>WYCZYŚĆ  </td><td>10.1.2.11  </td><td>55.29 KB   </td><td>256    </td><td>68.8%  </td><td>Identyfikator GUID (usunięte)</td><td>rack4</td></tr>
</table>

## <a name="test-the-single-region-cluster"></a>Testowanie klaster jednego regionu
Aby przetestować klaster, wykonaj następujące czynności:

1.    Przy użyciu polecenia Get-AzureInternalLoadbalancer polecenia programu Powershell, uzyskania adresu IP równoważenia obciążenia wewnętrznych (np.  10.1.2.101). Poniżej przedstawiono składnię polecenia: Get-AzureLoadbalancer — NazwaUsługi "hk-c-svc zachód us" [Wyświetla szczegóły usługi równoważenia obciążenia wewnętrznych wraz z jego adres IP]
2.  Logowanie do farmy sieci web maszyn wirtualnych (np. hk — w1-zachód nami) za pomocą Kit lub ssh
3.  Wykonywanie $CASS_HOME-Kosz cqlsh 10.1.2.101 9160
4.  Aby sprawdzić, czy klaster działa, należy użyć następujących poleceń CQL:

        CREATE KEYSPACE customers_ks WITH REPLICATION = { 'class' : 'SimpleStrategy', 'replication_factor' : 3 };
        USE customers_ks;
        CREATE TABLE Customers(customer_id int PRIMARY KEY, firstname text, lastname text);
        INSERT INTO Customers(customer_id, firstname, lastname) VALUES(1, 'John', 'Doe');
        INSERT INTO Customers(customer_id, firstname, lastname) VALUES (2, 'Jane', 'Doe');

        SELECT * FROM Customers;

Powinien zostać wyświetlony ekran, taki jak przedstawiony poniżej:

<table>
  <tr><th> customer_id </th><th> Imię </th><th> nazwisko </th></tr>
  <tr><td> 1 </td><td> Jan </td><td> Kowalski </td></tr>
  <tr><td> 2 </td><td> Joanna </td><td> Kowalski </td></tr>
</table>

Pamiętaj, że keyspace utworzony w kroku 4 używa SimpleStrategy z replication_factor 3. SimpleStrategy zaleca się pojedynczego danych wdrożeń Centrum należy NetworkTopologyStrategy dla wielu danych centrów. Replication_factor 3 da awarii węzłów na uszkodzenia.

##<a id="tworegion"> </a>Procesu wdrażania wielu regionu
Będzie wykorzystać wdrażanie jednego regionu zakończone i powtórz ten sam proces instalacji drugiego regionu. Klucza różnicę między wdrożenia region jednej lub wielu jest ustawiony tunelem VPN na potrzeby komunikacji między regionu; Firma Microsoft będzie rozpoczynać się instalacji sieciowej, obsługi administracyjnej maszyny wirtualne i konfigurowanie Cassandra.

###<a name="step-1-create-the-virtual-network-at-the-2nd-region"></a>Krok 1: Tworzenie wirtualnych sieci w regionie 2
Zaloguj się do portalu klasyczny Azure i tworzenie wirtualnej sieci przy użyciu Pokaż atrybuty w tabeli. Aby uzyskać szczegółowe instrukcje procesu, zobacz temat [Konfigurowanie wirtualnej sieci Cloud-Only w portalu klasyczny Azure](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) .      

<table>
<tr><th>Nazwa atrybutu    </th><th>Wartość    </th><th>Uwagi</th></tr>
<tr><td>Nazwa    </td><td>vnet przypadku wschód us</td><td></td></tr>
<tr><td>Region  </td><td>Wschodniej Stany Zjednoczone</td><td></td></tr>
<tr><td>Serwery DNS     </td><td></td><td>Ignoruj tak, jak firma Microsoft nie używają serwera DNS</td></tr>
<tr><td>Konfigurowanie sieci VPN punktu do witryny</td><td></td><td>     Ignoruj to</td></tr>
<tr><td>Konfigurowanie sieci VPN witryny do witryny</td><td></td><td>      Ignoruj to</td></tr>
<tr><td>Przestrzeń adresów   </td><td>10.2.0.0/16</td><td></td></tr>
<tr><td>Uruchamianie IP </td><td>10.2.0.0   </td><td></td></tr>
<tr><td>CIDR    </td><td>-16 (65531)</td><td></td></tr>
</table>

Dodaj następujące podsieci:
<table>
<tr><th>Nazwa    </th><th>Uruchamianie IP    </th><th>CIDR   </th><th>Uwagi</th></tr>
<tr><td>w sieci Web </td><td>10.2.1.0   </td><td>-24 (251)  </td><td>Podsieć farma sieci web</td></tr>
<tr><td>dane    </td><td>10.2.2.0   </td><td>-24 (251)  </td><td>Podsieć dla węzłów bazy danych</td></tr>
</table>


###<a name="step-2-create-local-networks"></a>Krok 2: Tworzenie sieci lokalnych
Sieci lokalnej w Azure wirtualnej sieci jest przestrzeń adresów serwera proxy, która mapy do witryny zdalnej tym chmurę prywatne lub innego regionu Azure. To miejsce adres serwera proxy jest związane z bramą zdalną dla routingu sieci do prawej sieci miejsc docelowych. Aby uzyskać instrukcje dotyczące nawiązywania połączenia VNET do VNET, zobacz temat [Konfigurowanie VNet VNet połączenia](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md) .

Utwórz dwie sieci lokalnej na następujące szczegóły:

| Nazwa sieciowa | Adres bramy sieci VPN | Przestrzeń adresów | Uwagi |
| ------------ | ------------------- | ------------- | ------- |
| HK-lnet-map-to-East-US | 23.1.1.1  | 10.2.0.0/16   | Podczas tworzenia sieci lokalnej nadać symbolu zastępczego adres bramy. Liczba rzeczywista bramy jest wypełniony po utworzeniu bramy. Upewnij się, że przestrzeń adresów dokładnie pasuje do odpowiednich VNET zdalnego; w tym przypadku VNET utworzony w regionu wschodniego USA. |
| HK-lnet-map-to-West-US | 23.2.2.2  | 10.1.0.0/16   | Podczas tworzenia sieci lokalnej nadać symbolu zastępczego adres bramy. Liczba rzeczywista bramy jest wypełniony po utworzeniu bramy. Upewnij się, że przestrzeń adresów dokładnie pasuje do odpowiednich VNET zdalnego; w tym przypadku VNET utworzonego w regionie Zachód USA. |


###<a name="step-3-map-local-network-to-the-respective-vnets"></a>Krok 3: Mapa "" sieci do odpowiednich VNETs
Z portalu klasyczny Azure zaznacz każdy vnet, kliknij "Konfigurowanie", sprawdź "Nawiązywanie połączenia z sieci lokalnej" i wybierz sieci lokalnych na następujące szczegóły:


| Wirtualna sieć | Sieci lokalnej |
| --------------- | ------------- |
| HK-vnet zachód us | HK-lnet-map-to-East-US |
| HK-vnet wschód us | HK-lnet-map-to-West-US |


###<a name="step-4-create-gateways-on-vnet1-and-vnet2"></a>Krok 4: Tworzenie bram na VNET1 i VNET2
Na pulpicie nawigacyjnym sieci wirtualnych kliknij pozycję Tworzenie bramy, który spowoduje Brama VPN inicjowania obsługi administracyjnej proces. Po kilku minutach pulpitu nawigacyjnego każdego wirtualnej sieci powinien być wyświetlany adres rzeczywisty bramy.

###<a name="step-5-update-local-networks-with-the-respective-gateway-addresses"></a>Krok 5: Aktualizacja sieci "Lokalnych" z adresami odpowiednich "brama"###
Edytowanie obu sieci lokalnych, aby zamienić adres IP bramy symbol zastępczy o adresie IP rzeczywistą tylko ustanawianie bramy. Za pomocą następujących mapowania:

<table>
<tr><th>Sieci lokalnej    </th><th>Brama wirtualnej sieci</th></tr>
<tr><td>HK-lnet-map-to-East-US </td><td>Brama hk-vnet zachód-Stanach Zjednoczonych</td></tr>
<tr><td>HK-lnet-map-to-West-US </td><td>Brama hk-vnet wschód-Stanach Zjednoczonych</td></tr>
</table>

###<a name="step-6-update-the-shared-key"></a>Krok 6: Aktualizowanie klucza udostępnionego
Użyj tego skryptu programu Powershell, aby zaktualizować klucz IPSec każdej bramy sieci VPN [przy użyciu klucza sake dla obu bram]: Ustawianie AzureVNetGatewayKey - VNetName hk-vnet wschód us - LocalNetworkSiteName hk-lnet-map-to-west-us - SharedKey D9E76BKK Set-AzureVNetGatewayKey - VNetName hk-vnet zachód us - LocalNetworkSiteName hk-lnet-map-to-east-us - SharedKey D9E76BKK

###<a name="step-7-establish-the-vnet-to-vnet-connection"></a>Krok 7: Ustanowienia połączenia VNET do VNET
Z portalu klasyczny Azure za pomocą menu "Pulpitu NAWIGACYJNEGO" sieci wirtualnych do nawiązania połączenia z bramy do bramy. Za pomocą elementy menu "POŁĄCZ" na dolnym pasku narzędzi. Po kilku minutach pulpitu nawigacyjnego powinien być wyświetlany szczegóły połączenia graficznego.

###<a name="step-8-create-the-virtual-machines-in-region-2"></a>Krok 8: Tworzenie maszyn wirtualnych w regionie #2
Utwórz obraz Ubuntu, zgodnie z opisem w regionie #1 wdrożenia, wykonując tej samej czynności lub kopią pliku wirtualnego dysku twardego obrazu do konta magazynu platformy Azure znajdującą się w regionie #2 i Utwórz obraz. Używanie ten obraz i tworzenie poniżej maszyn wirtualnych do nowej usługi w chmurze, hk-c-svc wschód nami:


| Nazwa komputera | Podsieć | Adres IP | Konfigurowanie dostępności | Kontrolera domeny i stojaków | Zapełnić? |
| ------------ | ------ | ---------- | ---------------- | ------- | ----- |
| HK-c1-wschód us | dane  | 10.2.2.4   | HK-c-aset-1      | kontrolera domeny = stojaków EASTUS = rack1 | Tak |
| HK-c2-wschód us | dane  | 10.2.2.5   | HK-c-aset-1      | kontrolera domeny = stojaków EASTUS = rack1 | Brak  |
| HK-c3-wschód us | dane  | 10.2.2.6   | HK-c-aset-1      | kontrolera domeny = stojaków EASTUS = rack2 | Tak |
| HK-c5-wschód us | dane  | 10.2.2.8   | HK-c-aset-2      | kontrolera domeny = stojaków EASTUS = rack3 | Tak |
| HK — c6-wschód us | dane  | 10.2.2.9   | HK-c-aset-2      | kontrolera domeny = stojaków EASTUS = rack3 | Brak  |
| HK-c7-wschód us | dane  | od 10.2.2.10  | HK-c-aset-2      | kontrolera domeny = stojaków EASTUS = rack4 | Tak |
| HK-c8-wschód us | dane  | 10.2.2.11  | HK-c-aset-2      | kontrolera domeny = stojaków EASTUS = rack4 | Brak  |
| HK — w1-wschód us | w sieci Web   | 10.2.1.4   | HK-w-aset-1      | N/D!                    | N/D! |
| HK — w2-wschód us | w sieci Web   | 10.2.1.5   | HK-w-aset-1      | N/D!                    | N/D! |


Postępuj zgodnie z instrukcjami samej jako region #1, ale za pomocą przestrzeni adresów 10.2.xxx.xxx.

###<a name="step-9-configure-cassandra-on-each-vm"></a>Krok 9: Konfigurowanie Cassandra w każdym maszyn wirtualnych
Zaloguj się do maszyn wirtualnych i wykonaj następujące czynności:

1. Edytowanie $CASS_HOME/conf/cassandra-rackdc.properties Aby określić właściwości Centrum i stojaków danych w formacie: kontrolera domeny = stojaków EASTUS = rack1
2. Edytowanie cassandra.yaml do konfigurowania węzłów nasion: nasion: "10.1.2.4,10.1.2.6,10.1.2.8,10.1.2.10,10.2.2.4,10.2.2.6,10.2.2.8,10.2.2.10"

###<a name="step-10-start-cassandra"></a>Krok 10: Cassandra Rozpocznij
Zaloguj się do każdej maszyn wirtualnych i uruchom Cassandra w tle, uruchamiając następujące polecenie: $CASS_HOME-Kosz cassandra

## <a name="test-the-multi-region-cluster"></a>Testowanie klaster wielu regionu
Już Cassandra wdrożeniu 16 węzły z 8 węzłów w każdym regionie Azure. Węzły te są w tym samym klastrze z typowych nazwę klaster i konfigurację węzeł nasion. Aby przetestować klaster zgodne z następującym procesem:

###<a name="step-1-get-the-internal-load-balancer-ip-for-both-the-regions-using-powershell"></a>Krok 1: Pobieranie IP usługi równoważenia obciążenia wewnętrznych dla obu regionów przy użyciu programu PowerShell
- Get AzureInternalLoadbalancer - NazwaUsługi "hk-c-svc zachód us"
- Get AzureInternalLoadbalancer - NazwaUsługi "hk-c-svc wschód us"  

    Uwaga adresów IP (np. zachód - 10.1.2.101, wschód - 10.2.2.101) wyświetlane.

###<a name="step-2-execute-the-following-in-the-west-region-after-logging-into-hk-w1-west-us"></a>Krok 2: Wykonywanie następujących czynności w regionie Zachód po zalogowaniu do hk — w1-zachód us
1.    Wykonywanie $CASS_HOME-Kosz cqlsh 10.1.2.101 9160
2.  Wykonaj następujące polecenia CQL:

        CREATE KEYSPACE customers_ks
        WITH REPLICATION = { 'class' : 'NetworkToplogyStrategy', 'WESTUS' : 3, 'EASTUS' : 3};
        USE customers_ks;
        CREATE TABLE Customers(customer_id int PRIMARY KEY, firstname text, lastname text);
        INSERT INTO Customers(customer_id, firstname, lastname) VALUES(1, 'John', 'Doe');
        INSERT INTO Customers(customer_id, firstname, lastname) VALUES (2, 'Jane', 'Doe');
        SELECT * FROM Customers;

Powinien zostać wyświetlony ekran, taki jak przedstawiony poniżej:

| customer_id | Imię | Nazwisko |
| ----------- | --------- | -------- |
| 1           | Jan      | Kowalski      |
| 2           | Joanna      | Kowalski      |


###<a name="step-3-execute-the-following-in-the-east-region-after-logging-into-hk-w1-east-us"></a>Krok 3: Wykonaj poniższe czynności w regionie Wschód po zalogowaniu do hk — w1-wschód nami:
1.    Wykonywanie $CASS_HOME-Kosz cqlsh 10.2.2.101 9160
2.  Wykonaj następujące polecenia CQL:

        USE customers_ks;
        CREATE TABLE Customers(customer_id int PRIMARY KEY, firstname text, lastname text);
        INSERT INTO Customers(customer_id, firstname, lastname) VALUES(1, 'John', 'Doe');
        INSERT INTO Customers(customer_id, firstname, lastname) VALUES (2, 'Jane', 'Doe');
        SELECT * FROM Customers;

Jak było widać w regionie Zachód powinna być widoczna samej wyświetlania:


| customer_id | Imię | Nazwisko   |
|------------ | --------- | ---------- |
| 1           | Jan      | Kowalski        |
| 2           | Joanna      | Kowalski        |


Wykonać kilka wstawia więcej i sprawdź, czy te replikowane zachód-nami klaster w części.

## <a name="test-cassandra-cluster-from-nodejs"></a>Test Cassandra klaster z Node.js
Przy użyciu jednego z maszyny wirtualne Linux utworzona w "web" poziomu wcześniej, firma Microsoft będzie wykonywać prosty skrypt Node.js, aby odczytać poprzednio wstawionej danych

**Krok 1: Instalowanie Node.js i Cassandra klienta**

1. Instalowanie Node.js i npm
2. Instalowanie pakietu "cassandra klienta w węźle" przy użyciu npm
3. Uruchom następujący skrypt w wierszu powłoki, który wyświetla ciąg json pobrane dane:

        var pooledCon = require('cassandra-client').PooledConnection;
        var ksName = "custsupport_ks";
        var cfName = "customers_cf";
        var hostList = ['internal_loadbalancer_ip:9160'];
        var ksConOptions = { hosts: hostList,
                             keyspace: ksName, use_bigints: false };

        function createKeyspace(callback){
           var cql = 'CREATE KEYSPACE ' + ksName + ' WITH strategy_class=SimpleStrategy AND strategy_options:replication_factor=1';
           var sysConOptions = { hosts: hostList,  
                                 keyspace: 'system', use_bigints: false };
           var con = new pooledCon(sysConOptions);
           con.execute(cql,[],function(err) {
           if (err) {
             console.log("Failed to create Keyspace: " + ksName);
             console.log(err);
           }
           else {
             console.log("Created Keyspace: " + ksName);
             callback(ksConOptions, populateCustomerData);
           }
           });
           con.shutdown();
        }

        function createColumnFamily(ksConOptions, callback){
          var params = ['customers_cf','custid','varint','custname',
                        'text','custaddress','text'];
          var cql = 'CREATE COLUMNFAMILY ? (? ? PRIMARY KEY,? ?, ? ?)';
        var con =  new pooledCon(ksConOptions);
          con.execute(cql,params,function(err) {
              if (err) {
                 console.log("Failed to create column family: " + params[0]);
                 console.log(err);
              }
              else {
                 console.log("Created column family: " + params[0]);
                 callback();
              }
          });
          con.shutdown();
        }

        //populate Data
        function populateCustomerData() {
           var params = ['John','Infinity Dr, TX', 1];
           updateCustomer(ksConOptions,params);

           params = ['Tom','Fermat Ln, WA', 2];
           updateCustomer(ksConOptions,params);
        }

        //update will also insert the record if none exists
        function updateCustomer(ksConOptions,params)
        {
          var cql = 'UPDATE customers_cf SET custname=?,custaddress=? where custid=?';
          var con = new pooledCon(ksConOptions);
          con.execute(cql,params,function(err) {
              if (err) console.log(err);
              else console.log("Inserted customer : " + params[0]);
          });
          con.shutdown();
        }

        //read the two rows inserted above
        function readCustomer(ksConOptions)
        {
          var cql = 'SELECT * FROM customers_cf WHERE custid IN (1,2)';
          var con = new pooledCon(ksConOptions);
          con.execute(cql,[],function(err,rows) {
              if (err)
                 console.log(err);
              else
                 for (var i=0; i<rows.length; i++)
                    console.log(JSON.stringify(rows[i]));
            });
           con.shutdown();
        }

        //exectue the code
        createKeyspace(createColumnFamily);
        readCustomer(ksConOptions)


## <a name="conclusion"></a>Wnioski
Microsoft Azure jest elastyczną platformę, umożliwiająca uruchamianie zarówno firmy Microsoft, jak i Otwórz źródło oprogramowania opisanym tego ćwiczenia. Wysokiej dostępności klastrów Cassandra mogą być rozmieszczone na jedno centrum danych za pośrednictwem rozłożenie w węzłach wiele domen błędów. Można także wdrożyć klastrów Cassandra u wielu geograficznie odległości regionów Azure systemów dowód awarii. Azure i Cassandra ze sobą umożliwia konstruowanie wysoce skalowalna, wysokiej dostępności i usług w chmurze odzyskania danych potrzeb za pośrednictwem Internetu dzisiejszą skalowanie usług.  

##<a name="references"></a>Odwołania##
- [http://cassandra.apache.org](http://cassandra.apache.org)
- [http://www.datastax.com](http://www.datastax.com)
- [http://www.nodejs.org](http://www.nodejs.org)
