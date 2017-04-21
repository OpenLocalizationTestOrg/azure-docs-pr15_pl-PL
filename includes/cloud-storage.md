#<a name="data-management-and-business-analytics"></a>Zarządzanie danymi i analiz biznesowych

Zarządzanie i analizowania danych w chmurze jest równie ważne jest miejscach. Z myślą o tym, Azure oferuje szeroką gamę technologii do pracy z danymi relacyjnymi i -relacyjnych. W tym artykule przedstawiono każdej z opcji. 

##<a name="table-of-contents"></a>Spis treści      

- [Magazyn obiektów blob](#blob)
- [Uruchamianie bazami danych w maszyny wirtualnej](#dbinvm)
- [Baza danych SQL](#sqldb)
    - [Synchronizacja danych SQL](#datasync)
    - [Przy użyciu maszyn wirtualnych raportowania danych SQL](#datarpt)
- [Magazyn tabel](#tblstor)
- [Hadoop](#hadoop)

## <a name="blob"></a>Magazyn obiektów blob

Wyraz "obiektów blob" jest skrótem "dużego obiektu binarnego", a jej w tym artykule opisano dokładnie jest jakie obiektów blob: kolekcja danych binarnych. Jeszcze chociaż jest proste, obiektów blob są bardzo przydatne. [Rysunek 1](#Fig1) przedstawia podstawy magazyn obiektów Blob platformy Azure.

<a name="Fig1"></a>![Diagram obiektów blob][blobs]
 
**Rysunek 1: Magazynem obiektów Blob platformy Azure przechowuje dane binarne - BLOB — w kontenerach.**

Aby użyć obiektów blob, najpierw należy utworzyć Azure *konta miejsca do magazynowania*. W ramach tego możesz określić Azure centrum danych, do której będzie przechowywana obiekty utworzone przy użyciu tego konta. Miejsce, w którym go umieszczono poszczególnych obiektów blob tworzonych należy do niektórych kontenera w Twoim koncie miejsca do magazynowania. Aby uzyskać dostęp do obiektów blob, aplikacja zapewnia adres URL formularza:

http://&lt;*StorageAccount*&gt;.blob.core.windows.net/&lt;*kontenera*&gt;/&lt;*BlobName*&gt;

&lt;*StorageAccount* &gt; jest unikatowym identyfikatorem przypisywana podczas tworzenia nowego konta magazynu podczas &lt; *kontenera* &gt; i &lt; *BlobName* &gt; są nazwy określonego kontenera i obiektów blob w danym kontenerze. 

Azure udostępnia dwa typy obiektów blob. Dostępne są następujące:

- *Blokowanie* blob, z których każdy może zawierać maksymalnie 200 GB danych. Jak sugeruje nazwa, blob blok jest podzielone na kilka liczba bloków. Jeśli w przypadku wystąpienia błędu podczas przesyłania blob blok ponowną wznowić z ostatnich blok zamiast ponownie wysłać całą obiektów blob. Blob blok są bardzo ogólne podejście do magazynu, a one typ najczęściej używane obiektów blob dzisiaj.

- Blob *strony* , które może być w jednym terabajtów. Blob strony są przeznaczone dla dostępie, a więc każdego z nich jest podzielony na kilka liczbę stron. Aplikacja jest bezpłatna do odczytu i zapisu poszczególnych stron losowo w obiekcie blob. W maszyn wirtualnych Azure na przykład pośrednictwem SMS, możesz utworzyć za pomocą obiektów blob strony jako wygenerowanie dla zarówno OS dyski i danych.

Czy wybrano blob blok lub strony blob aplikacji dostęp do danych typu blob na kilka różnych sposobów. Następujące opcje:

- Bezpośrednio w RESTful (to znaczy oparte na HTTP) protokół dostępu. Zarówno Azure aplikacji, jak i zewnętrzne aplikacje, w tym aplikacje uruchomione w środowisku lokalnym, można używać tej opcji.
- Za pośrednictwem biblioteki Azure klienta miejsca do magazynowania, który udostępnia interfejs bardziej przyjazne dla deweloperów na bieżąco z protokołu dostępu nieprzetworzonych RESTful obiektów blob. Ponownie Azure aplikacji i aplikacjach zewnętrznych mogą uzyskiwać dostęp za pomocą tej biblioteki obiektów blob.
- Za pomocą dysków Azure, opcja, która umożliwia aplikacji Azure Potraktuj blob strony jako dysk lokalny w systemie plików NTFS. Z aplikacją obiektów blob strony wygląda zwykłej systemu plików systemu Windows dostępne za pomocą standardowego pliku we/wy. W rzeczywistości odczytuje i zapisuje, są wysyłane do podstawowej blob strony, w którym wykonuje dysk Azure. 

Aby zabezpieczyć się przed awarie sprzętu i zwiększyć dostępność, co obiektów blob są replikowane między komputerami trzy w Azure centrum danych. Zapisywanie obiektów blob aktualizuje wszystkie trzy kopie, aby później Odczyt nie będą widoczne niezgodne wyniki. Można również określić, że obiekt blob dane mają zostać skopiowane do innego Azure centrum danych w tym samym geo, ale co najmniej 500 mila z dala od komputera. To kopiowanie, o nazwie *replikacji geo*się dzieje po upływie kilku minut aktualizacji to i są przydatne w przypadku awarii.

Dane w obiektów blob można także udostępniane za pośrednictwem Azure *Sieci dostarczania zawartości (CDN)*. Przez buforowanie kopii danych obiektów blob dziesiątki serwerów na świecie, CDN może przyspieszyć dostęp do informacji, który jest dostępny wielokrotnie. 

Proste, jak są one, obiektów BLOB to odpowiedni wybór w wielu sytuacjach. Przechowywanie i przesyłanie strumieniowe wideo i audio przedstawiają oczywiste, podczas wykonywania kopii zapasowych i innych rodzajów archiwizowania danych. Deweloperów służy także do przechowywania dowolnego rodzaju niestrukturalne dane, które były, takich jak obiektów blob. Zaskakująco przydatne może być o sposób proste do przechowywania i udostępniania danych binarnych.


## <a name="dbinvm"></a>Uruchamianie bazami danych w maszyny wirtualnej

Wiele aplikacji korzysta z niektórych rodzajów bazy danych zarządzania systemu. Relacyjne systemów przykład programu SQL Server są najczęściej używane wybór, ale -relacyjnych metod, nazywaną technologii *NoSQL* Uzyskaj więcej popularnych każdego dnia. Aby umożliwić aplikacje w chmurze za pomocą tych opcji zarządzania danymi, maszyn wirtualnych Azure umożliwia uruchamianie bazami danych (relacyjnej lub NoSQL) w maszyny. [Rysunek 2](#Fig2) pokazano, jak wygląda z programem SQL Server.

<a name="Fig2"></a>![Diagram programu SQL Server w maszyny wirtualnej][SQLSvr-vm]
 
**Rysunek 2: Azure maszyn wirtualnych umożliwia w systemie w Głosowa, utrzymywanie dostarczony przez blob bazami danych.**

Dla programistów i administratorów baz danych w tym scenariuszu znacznie wygląda tak: uruchamianie tego samego oprogramowania w Centrum własnych danych. W pokazanym tu przykładzie na przykład prawie wszystkie funkcje programu SQL Server można używać i masz administracyjne pełnego dostępu do sieci. Masz również odpowiedzialne za zarządzanie serwerem bazy danych, tak jakby były uruchomione lokalnie.

Jak pokazano na [ilustracji 2](#Fig2) , powinien być zapisany na lokalnym dysku maszyn wirtualnych, serwer jest uruchamiany w są wyświetlane baz danych. W obszarze okładki jednak każdy z tych dysków jest zapisywany obiektów blob platformy Azure. (Jest podobne do przy użyciu sieci SAN w Centrum własnych danych, z obiektów blob działające podobnie jak LUN.) Zgodnie z dowolnym obiektów blob platformy Azure, dane, które zawiera replikacji trzy razy w centrum danych i, jeśli zażądasz, geo replikować do innego centrum danych, w tym samym regionie. Użytkownik może również korzystać z opcji, takich jak bazy danych programu SQL Server odzwierciedlające dla większa niezawodność. 

Innym sposobem używania programu SQL Server w maszyny jest utworzyć aplikację hybrydowych, w którym dane przebywa Azure logiki aplikacji uruchomiony lokalnego. Na przykład to miały znaczenie, gdy aplikacje w wielu lokalizacjach lub na różnych urządzeniach przenośnych muszą mieć te same dane. Aby komunikacji między chmury bazy danych i lokalnych logikę prostsze, organizacji może być tworzenie połączenia wirtualnej sieci prywatnej (VPN) między Azure centrum danych i osobnym centrum danych lokalnych Azure wirtualnej sieci.


## <a name="sqldb"></a>Baza danych SQL

Dla wielu osób uruchomione bazami danych w maszyny jest pierwszą opcję się do zarządzania danych strukturalnych w chmurze. Go jest nie tylko wyboru, jednak ani nie jest zawsze najlepszym wyborem. W niektórych przypadkach zarządzanie danymi za pomocą platformy jako podejście usługi (PaaS) ma sens więcej. Azure udostępnia technologia PaaS o nazwie bazy danych SQL, która pozwala to zrobić dla danych relacyjnych. [Rysunek 3](#Fig3) przedstawia tę opcję. 

<a name="Fig3"></a>![Diagram bazy danych SQL][SQL-db]
 
**Rysunek 3: Bazy danych SQL udostępnia udostępnionych Usługa magazynu relacyjnych PaaS.**

Baza danych SQL nie udostępnia każdego z klientów własne fizycznie wystąpienie programu SQL Server. Zamiast tego zawiera wiele dzierżawy usługi, na serwerze bazy danych SQL logiczne dla każdego z klientów. Wszyscy klienci udostępnianie usługa zapewnia możliwości obliczeń i miejsca do magazynowania. I podobnie jak w przypadku magazyn obiektów Blob wszystkie dane w bazie danych SQL są przechowywane w trzech różnych komputerach w Azure centrum danych, podając baz danych wbudowane wysokiej dostępności (HA).

Do aplikacji bazy danych SQL znacznie wygląda jak program SQL Server. Aplikacje można wysyłania kwerend SQL dla tabel relacyjnych, za pomocą procedury przechowywanej T-SQL, a następnie wykonać transakcji w wielu tabelach. I aplikacje uzyskiwać dostęp do bazy danych SQL za pomocą protokołu strumienia danych tabelarycznych (TDS) tego samego protokołu umożliwiające dostęp do programu SQL Server może one pracować z danymi przy użyciu struktury obiektu, ADO.NET, JDBC i innych danych znanych interfejsów dostępu. 

Ale ponieważ bazy danych SQL jest uruchomiony w centrach danych Azure usługi w chmurze, nie trzeba zarządzać dowolnym fizycznie aspektów systemu, takich jak użycie dysku. Ponadto nie musisz martwić się o zaktualizowanie oprogramowania lub obsługi innych zadań administracyjnych niższego poziomu. Każdej organizacji klienta nadal określa własnej bazy danych, oczywiście, wraz z ich schematów i logowania użytkownika, ale wiele proste zadania administracyjne są wykonywane automatycznie. 

Gdy baza danych SQL wygląda podobnie programu SQL Server do aplikacji, go nie działają dokładnie tak samo jak bazami danych uruchomionych fizycznej lub maszyn wirtualnych. Ponieważ jest uruchamiana na sprzęcie udostępnionego wydajność różnią się obciążenia umieszczony na tym urządzeniu przez wszystkie jej klientów. Oznacza to, że wydajności, powiedzieć procedura składowana w bazie danych SQL może się różnić od jeden dzień do drugiej. 

Dzisiaj baza danych SQL pozwala utworzyć bazę danych, przytrzymując do 150 gigabajtów. Jeśli musisz podania udostępnia opcję o nazwie *Federacji*. Aby to zrobić, administrator bazy danych tworzy dwa lub więcej *Członkowie Federacji*, z których każdy jest oddzielnej bazy danych przy użyciu własny schemat. Danych jest rozmieszczony członkowie coś, co jest często nazywane *sharding*z każdego członka przypisany unikatowy *klucz Federacji*. Należy kierować kwerend SQL problemów aplikacji dla tych danych, określając klucza Federacji, który identyfikuje składnik Federacji kwerendy. Dzięki temu tradycyjne podejście relacyjnych za pomocą dużych ilości danych. Jak zawsze istnieje korzystnych rozwiązań; kwerendy ani transakcje może obejmować członków Federacji, na przykład. Ale gdy relacyjnych usługa PaaS jest to najlepszy wybór i możliwości te są dopuszczalne, przy użyciu Federacji SQL może być dobrym rozwiązaniem.

Baza danych SQL mogą być używane przez aplikacje na Azure lub w innych miejscach, takich jak w centrum danych lokalnych. To ułatwia przydatne w przypadku aplikacje w chmurze, które wymagają w relacyjnej bazie danych, a także aplikacje, które mogą korzystać z dane są przechowywane w chmurze lokalnego. Aplikacji mobilnej może zależeć bazy danych SQL do zarządzania udostępnionej danych relacyjnych, na przykład jako mogą być aplikacji zapasów wykonywana w wielu sprzedawcy na całym świecie.

Myślę o bazy danych SQL zgłasza problemu oczywiste (i ważne): możesz uruchomienia programu SQL Server w maszyny, a gdy jest baza danych SQL lepszym wyborem? W zwykły sposób ma możliwości, a więc lepiej jest którą metodę zależy od wymagań. 

Najprostszy sposób wziąć pod uwagę go jest wyświetlanie bazy danych SQL jako dla nowych aplikacji, gdy program SQL Server w maszyny jest lepszym rozwiązaniem, podczas przenoszenia istniejącego lokalnego aplikacji w chmurze. Również może być przydatne przyjrzeć się na tę decyzję w sposób bardziej precyzyjne, jednak. Na przykład baza danych SQL jest łatwiejszy w użyciu, ponieważ wymagane są minimalne konfiguracji i zarządzania. Uruchomiony program SQL Server w maszyny mogą mieć przewidywalne wydajności — nie jest usług udostępnionych - a obsługuje także większe niefederacyjnej bazach danych dla niż bazy danych SQL. Nadal baza danych SQL zawiera wbudowane replikacji zarówno danych, jak i przetwarzanie, skuteczne określeniu bazami danych wysokiej dostępności z pracą bardzo mało. Gdy program SQL Server zapewnia większą kontrolę i nieco szerszy zestaw opcji, łatwiej skonfigurowane i znacznie mniej pracy do zarządzania jest baza danych SQL.

Na koniec należy zwrócić uwagę, że baza danych SQL nie jest dostępne na Azure usługi danych tylko PaaS. Partnerów firmy Microsoft zapewniają również inne opcje. Na przykład ClearDB oferuje MySQL PaaS, oferuje, gdy opcja NoSQL sprzedawanych Cloudant. Usługi danych PaaS są właściwe rozwiązanie w wielu sytuacjach, a więc takie podejście do zarządzania danymi jest ważna część Azure.


### <a name="datasync"></a>Synchronizacja danych SQL

Jeśli baza danych SQL zachowana trzy kopie każdej bazy danych w jednym Azure centrum danych, jego danych nie automatycznie replikować między Azure centrach danych. Zamiast tego zawiera synchronizacja danych SQL, usługa, która umożliwia wykonaj następujące czynności. [Rysunek 4](#Fig4) pokazuje, jak wygląda.

<a name="Fig4"></a>![Diagram synchronizacja danych SQL][SQL-datasync]
 
**Rysunek 4: Synchronizacja danych SQL synchronizacja danych w bazie danych SQL z danymi w innych Azure i lokalnych centrach danych.**

Jak pokazano na diagramie, synchronizacja danych SQL można zsynchronizować danych w różnych lokalizacjach. Załóżmy, że korzystasz z aplikacji w wielu Azure centrach danych, na przykład z danymi przechowywanymi w bazie danych SQL. Synchronizacja danych SQL służy do tego dane są synchronizowane. Synchronizacja danych SQL można także synchronizować danych między Azure centrum danych i wystąpienie programu SQL Server uruchomionym w centrum danych lokalnych. Może to być przydatne do utrzymywania zarówno lokalną kopię dane używane przez aplikacje lokalnego i kopii chmurze używane przez aplikacje na Azure. A mimo że nie jest widoczny na rysunku, synchronizacja danych SQL można również do synchronizowania danych między bazy danych SQL i SQL Server uruchomionym w maszyny Azure lub innym programie.

Synchronizacja może być dwukierunkowego i określają dokładnie jakie dane są synchronizowane i jak często jest wykonywane. (Synchronizacji między bazami danych nie jest Atomowej, jednak - jest zawsze co najmniej pewne opóźnienie). I jednak są one używane, Konfigurowanie synchronizacji z synchronizacji danych SQL jest całkowicie konfiguracji sterowanych; nie ma żadnego kodu do pisania.


### <a name="datarpt"></a>Przy użyciu maszyn wirtualnych raportowania danych SQL

Po bazy danych zawiera dane, ktoś będą prawdopodobnie zechcesz tworzenia raportów przy użyciu tych danych. Azure może zostać uruchomiony program SQL Server Reporting Services (SSRS) w maszyn wirtualnych Azure czyli funkcjonalny równoznaczne z uruchomieniem usługi SQL Server Reporting Services lokalnego. Następnie można SSRS uruchamiać raporty danych przechowywanych w bazie danych SQL Azure.  [Rysunek 5](#Fig5) pokazano, jak działa proces.

<a name="Fig5"></a>![Diagram przedstawiający raportowania SQL][SQL-report]
 
**Rysunek 5: SQL Server Reporting Services działający w maszyn wirtualnych Azure udostępnia usług reporting services dla danych w bazie danych SQL. .**

Zanim użytkownik może wyświetlać raportu, osoby definiuje co tego raportu powinna wyglądać podobnie do (krok 1). Z SSRS na maszyny, można to zrobić przy użyciu jednej z dwóch narzędzi: SQL Server Data Tools, część programu SQL Server 2012 lub poprzednik Business Intelligence (BI) Development Studio. Podobnie jak w przypadku SSRS, tych definicji raportu wyrażone są w języku definicji raportu (RDL). Po utworzeniu pliki RDL dla raportu są one przekazywane do maszyn wirtualnych w chmurze (krok 2). Definicja raportu jest teraz gotowy do użycia.

Następnie użytkownika aplikacji uzyskuje dostęp do raportu (krok 3). Aplikacja przekazuje to żądanie do SSRS maszyn wirtualnych (krok 4), które kontakty bazy danych SQL lub innych źródeł danych, aby uzyskać dane (krok 5). SSRS używa tych danych i odpowiednie pliki RDL do renderowania raportu (krok 6), następnie zwraca raport z aplikacją (krok 7), wyświetla go do użytkownika (krok 8).

Osadzanie raportu w aplikacji, scenariusz pokazano na ilustracji, nie jest to jedyna opcja. Użytkownik może również wyświetlać raporty w programie Report Manager na Głosowa, programu SharePoint na maszyn wirtualnych lub w inny sposób. Raporty można również łączyć, z jednym raporcie zawierającą łącze do innego.

SSRS na maszyn wirtualnych Azure zapewnia pełną funkcjonalność jako rozwiązania raportowania w chmurze. Raporty można użyć dowolnego źródła danych obsługiwane przez SSRS. Aplikacje i raporty mogą zawierać kodu osadzonego lub zestawy do obsługi niestandardowej zachowania. Wykonanie raportu i renderowanie są szybkie, ponieważ zawartość serwera raportów i aparat razem na tym samym serwerze.



## <a name="tblstor"></a>Magazyn tabel

W relacyjnej bazie danych jest przydatne w wielu sytuacjach, ale nie zawsze jest odpowiednim wyborem. Jeśli aplikacja wymaga szybkie, proste dostęp do bardzo dużych ilości danych luźno strukturalnych, na przykład relacyjnej bazy danych nie może działać prawidłowo. Technologia NoSQL prawdopodobnie może być lepszym rozwiązaniem.

Przykładem tego rodzaju NoSQL podejście jest Azure magazyn tabel. Mimo jego nazwę magazyn tabel nie obsługuje standardowej relacyjnych tabel. Zamiast tego zawiera obsługującej jako *klucza/wartości przechowywane*, kojarzenie zestawu danych przy użyciu określonego klucza, a następnie co dostęp do aplikacji danych, dostarczając klucz. [Rysunek 6](#Fig6) przedstawia podstawy.

<a name="Fig6"></a>![Diagram przedstawiający Magazyn tabel][SQL-tblstor]
 
**Rysunek 6: Azure magazyn tabel jest magazynem klucza/wartości umożliwiające szybkie, proste dostęp do dużych ilości danych.**

Przykład obiektów blob każdej tabeli jest skojarzony z konta usługi Azure miejsca do magazynowania. Tabele są nazywane też znacznie takich jak obiektów blob przy użyciu adresu URL formularza

http://&lt;*StorageAccount*&gt;.table.core.windows.net/&lt;*TableName*&gt;

Jak pokazano na ilustracji, każda tabela jest podzielony na niektórych liczbę partycje, z których każdy będzie mogą być przechowywane na komputerze oddzielnych. (Jest to rodzaj sharding, podobnie jak w przypadku Federacji SQL.) Azure aplikacji i aplikacje w innych miejscach dostęp do tabeli za pomocą protokołu RESTful OData lub biblioteka Azure miejsca do magazynowania klienta.

Każdy partition w tabeli zawiera niektóre liczba *jednostek*, zawierającą maksymalnie 255 *Właściwości*. Dla każdej właściwości ma nazwę, typ (na przykład binarny, wartość logiczna, daty i godziny, Int lub ciąg) i wartości. W przeciwieństwie do relacyjnych miejscem do magazynowania w poniższych tabelach mają brak stałej schematu, a więc różnych obiektów w tej samej tabeli może zawierać właściwości z różnymi typami. Jeden podmiot może mieć tylko właściwość ciąg zawierający nazwę, na przykład podczas innego podmiotu w tej samej tabeli ma dwie właściwości Int zawierające numer Identyfikatora odbiorcy i zdolności kredytowej.

Aby zidentyfikować danej jednostce w tabeli, aplikacja zawiera klucz tego obiektu. Klucz ma dwie części: *partycją klucz* , który identyfikuje partycją określonych i *klucz wiersza* , który identyfikuje podmiotu należącego do niej. [Rysunek 6](#Fig6)na przykład klient żąda jednostka o kluczy partycją A i wiersza 3 i Magazyn tabel zwraca taki podmiot, w tym wszystkie właściwości, które zawiera.

Ta struktura pozwala tabel jest duży — jednej tabeli może zawierać maksymalnie 100 terabajtów danych — i umożliwia szybki dostęp do danych, które zawierają. Również przenosi ograniczenia, jednak. Na przykład to nie obsługuje aktualizacji transakcji, obejmujące tabele lub nawet partycje w jednej tabeli. Zestaw aktualizacji do tabeli tylko można podzielić na Atomowej transakcji, jeśli są wszystkie jednostek biorących udział w tym samym partycją. Jest również sposobem kwerendę tabeli na podstawie wartości właściwości nie ma obsługę sprzężenia w wielu tabelach. I w przeciwieństwie do relacyjnych baz danych należą nie obsługuje procedur składowanych tabel.

Magazyn tabel platformy Azure jest dobrym rozwiązaniem dla aplikacji, które potrzebują szybkie, tanie dużych ilości danych strukturalnych luźno. Na przykład aplikacja Internet, zawierający informacje z profilu dla wielu użytkowników może używać tabel. Ważne w takiej sytuacji jest szybki dostęp, a aplikacja prawdopodobnie nie potrzebuje pełnych możliwości programu SQL Server. Przekazująca ta funkcja umożliwia uzyskanie szybkość i rozmiar czasami miały znaczenie, a więc magazyn tabel jest po prostu właściwe rozwiązanie niektórych problemów.


## <a name="hadoop"></a>Hadoop

Organizacje mają zostały tworzenia dziesięcioleci magazynów danych. Kolekcje te informacje najczęściej przechowywane w tabelach relacyjnych umożliwiają użytkownikom przetwarzania i Dowiedz się z danych na wiele różnych sposobów. Z programem SQL Server na przykład są często za pomocą narzędzi, takich jak SQL Server Analysis Services, wykonaj następujące czynności.

Ale Załóżmy, że chcesz zrobić analizy danych relacyjnych. Danych może mieć wiele form: informacje z czujników lub znaczników RFID, pliki dziennika w farmy serwerów danych clickstream tworzone przez aplikacje sieci web, obrazów z medycznymi diagnostyczne i nie tylko. Te dane mogą być także duże, zbyt duży, może być używany skuteczne z magazynem tradycyjnych danych. Problemy związane z danymi duży tak, rzadkie kilka lat temu teraz stały się dość często.

Aby przeprowadzić analizę ten rodzaj danych jest duży, naszych branżowe znacznym stopniu zbliżył w jedno rozwiązanie: Otwórz źródło technologii Hadoop. Hadoop uruchamiania w klastrze fizycznej lub maszyn wirtualnych rozmieszczania danych, realizowanych w na tych komputerach i przetwarzanie równolegle. Więcej komputerów Hadoop ma korzystać, tym szybciej można wykonać, niezależnie od jej pracy robi.

Ten rodzaj problemu jest naturalne Dopasuj publicznej chmury. Zamiast zachowaniu maszyny wirtualne armii lokalnych serwerów, które mogą być znajdują się bezczynne umożliwia większość czasu uruchomiony Hadoop w chmurze utworzenie (i za pracę) tylko wtedy, gdy jest to potrzebne. Jeszcze lepiej — więcej duży danych, który chcesz przeanalizować z Hadoop i utworzono w chmurze, zapisywania możesz problemy z poruszanie się po aplikacji. Aby pomóc wykorzystać poniższe współdziałania, firma Microsoft świadczy usługi Hadoop Azure. [Rysunek 7](#Fig7) przedstawia najważniejsze składniki tej usługi.

<a name="Fig7"></a>![Diagram hadoop][hadoop]

**Rysunek 7: Hadoop Azure działa MapReduce zadań, które przetwarzanie danych przy użyciu wielu maszyn wirtualnych jednocześnie.**

Aby użyć Hadoop Azure, najpierw prosisz tej platformie chmurze, aby utworzyć klaster Hadoop określająca liczbę maszyny wirtualne potrzebujesz. Konfigurowanie klastrze Hadoop siebie jest zadaniem — uproszczony, a więc co Azure zrobił to za Ciebie sens. Po zakończeniu korzystania z klastrem, możesz zamknąć. Istnieje nie trzeba zapłacić za zasoby obliczeń, które nie są używane.

Aplikacja Hadoop, nazywanego dodatkiem *zadanie*używa model programowania znany jako *MapReduce*. Jak pokazano na ilustracji, logiczny dla zadania MapReduce uruchamia jednocześnie przez wielu maszyny wirtualne. Przetwarzanie danych równolegle, Hadoop można analizować dane dużo szybciej niż rozwiązania dla pojedynczego komputera.

Azure dane MapReduce zadania działa na jest zazwyczaj przechowywane w magazynie obiektów blob. W Hadoop jednak zadania MapReduce oczekiwać, że dane mają być przechowywane w *Pliku usługi Hadoop Distributed System (HDFS)*. HDFS jest podobne do magazyn obiektów Blob na kilka sposobów; na przykład replikuje dane na wielu serwerach fizycznie. Zamiast duplikować tę funkcję, Hadoop Azure zamiast udostępnia magazyn obiektów Blob za pośrednictwem interfejsu API HDFS, jak to pokazano na ilustracji. Podczas warunków logicznych w zadaniu MapReduce myśli, że uzyskuje dostęp do zwykłej plików HDFS, zadania w rzeczywistości działa z danymi strumieniowo do niego z obiektami blob. I do obsługi wielkości liter w miejsce, w którym wielu zadań są uruchamiane na tych samych danych, Hadoop Azure umożliwiają kopiowanie danych z obiektami BLOB do pełnego HDFS uruchomione w maszyny wirtualne. 

Zadania MapReduce często są zapisywane w języku Java obecnie metody, która obsługuje Hadoop Azure. Firma Microsoft dodała również obsługę tworzenia zadań MapReduce w innych językach, w tym C#, F # i JavaScript. Celem jest zapewnienie łatwiej dostępne dla większej grupy deweloperów tej technologii duży danych.

Oprócz HDFS i MapReduce Hadoop zawiera inne technologie, które innym użytkownikom analizowanie danych bez zapisywania zadanie MapReduce się. Na przykład świnka jest językiem wysokiego poziomu przeznaczona do analizy danych duży, gdy gałęzi oferuje języka SQL przypominających o nazwie HiveQL. Zarówno świnka, jak i gałęzi wygenerowanie MapReduce zadań, które przetwarzanie danych HDFS, ale ich ukryć tego rozwiązania od użytkowników. Oba mają do dyspozycji Hadoop Azure.

Firma Microsoft udostępnia również sterownik HiveQL dla programu Excel. Za pomocą dodatku programu Excel, analitycy biznesowi można utworzyć kwerendy HiveQL (i tym samym MapReduce zadań) bezpośrednio z programu Excel, a następnie procesu i wizualizowanie wyników przy użyciu dodatku PowerPivot i innych narzędzi programu Excel. Hadoop Azure zawiera również inne technologie, takie jak maszynowego uczenia bibliotek Mahout, system wyszukiwania graph Pegasus i innych.

Ważne jest duży, analizy, a więc Hadoop również ważne. Dostarcz Hadoop jako zarządzaną usługa Azure, oraz łącza do znanych narzędzi, takich jak program Excel, Microsoft ma na celu udostępnianie tej technologii szerszej grupie użytkowników.

Ważne jest bardziej ogólnie dane różnego rodzaju. Jest to, dlaczego Azure udostępnia szereg opcji zarządzania i małych firm analizy danych. Niezależnie od aplikacji próbujesz utworzyć, to prawdopodobnie, że znajdziesz coś w tej platformie chmury współpracujące dla Ciebie.

[blobs]: ./media/cloud-storage/Data_01_Blobs.png
[SQLSvr-vm]: ./media/cloud-storage/Data_02_SQLSvrVM.png
[SQL-db]: ./media/cloud-storage/Data_03_SQLdb.png
[SQL-datasync]: ./media/cloud-storage/Data_04_SQLDataSync.png
[SQL-report]: ./media/cloud-storage/Data_05_SQLReporting.png
[SQL-tblstor]: ./media/cloud-storage/Data_06_TblStorage.png
[hadoop]: ./media/cloud-storage/Data_07_Hadoop.png
