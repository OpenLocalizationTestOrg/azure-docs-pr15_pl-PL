<properties
    pageTitle="Co nowego w Azure zestaw narzędzi dla programu Eclipse"
    description="Informacje o najnowszych funkcji narzędzi Azure dla programu Eclipse."
    services=""
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="multiple"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/26/2016" 
    ms.author="robmcm;asirveda;martinsawicki"/>

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh694270.aspx -->

# <a name="whats-new-in-the-azure-toolkit-for-eclipse"></a>Co nowego w Azure zestaw narzędzi dla programu Eclipse

## <a name="azure-toolkit-for-eclipse-releases"></a>Azure zestaw narzędzi dla wersji Zaćmienie

Ten artykuł zawiera informacje na temat różnych wersji i najnowsze aktualizacje, aby zestaw narzędzi Azure dla Zaćmienie.

> [AZURE.NOTE] Istnieje także zestaw narzędzi Azure dla IntelliJ IDE. Aby uzyskać więcej informacji zobacz [Zestaw narzędzi Azure dla IntelliJ].

### <a name="august-26-2016"></a>26 sierpnia 2016

Zestaw narzędzi Azure dla Zaćmienie - wersji 2016 sierpnia zawiera następujące ulepszenia:

* **Rozkład JDK niestandardowe**. Zestaw narzędzi Azure dla Zaćmienie obsługuje teraz określanie i wdrażanie dowolnego wersji JDK do kontenera usługi Azure aplikacji sieci Web:
  - Oprócz JDKs dostarczony przez Azure można wybierać szeroki wybór wersji Zulu OpenJDK udostępnione Azure przez systemy Azul.
  - Można również określić własne rozkładu JDK podczas przesyłania jako pliku ZIP do swojego konta miejsca do magazynowania.
* **Ulepszenia widoku Eksplorator Azure**:
  - Obsługa zarządzania maszyn wirtualnych przy użyciu nowego modelu Menedżera zasobów firmy Azure: może listy, tworzyć i usuwać maszyn wirtualnych oparte na Menedżera zasobów bez opuszczania IDE.
  - Obsługa magazyn obiektów blob zarządzaniem za pomocą Menedżera zasobów Azure osoby, które uzupełnia istniejące funkcje do zarządzania kontami "klasycznego" przestrzeni dyskowej.
* **Sterownik Microsoft JDBC 6.0 dla programu SQL Server**. Ta aktualizacja zawiera najnowszy sterownik JDBC dla programu Microsoft SQL Server (wersja 6.0), która jest obecnie dostępny jako biblioteki, który można łatwo dodawać do projektów języka Java, co zastępowanie starszej wersji.

### <a name="june-29-2016"></a>29 czerwca 2016

Zestaw narzędzi Azure dla Zaćmienie - wersji 2016 czerwca zawiera następujące ulepszenia:

* **Wymaganie Java 8**. Zestaw narzędzi Azure dla Zaćmienie teraz wymaga Java 8, mimo że to wymaganie dotyczy tylko dla zestawu narzędzi — aplikacji nadal korzystać wszystkie wersje Java, które są obsługiwane przez Azure.
* **Obsługa najnowszych JDKs Java**. Najnowsze wersje Java JDKs są teraz obsługiwane przez zestaw narzędzi Azure dla Zaćmienie.
* **Obsługa v2.9.1 Azure SDK**. Najnowsza wersja pakietu Azure jest teraz minimalne wstępnie wymagane dla narzędzi Azure dla programu Eclipse.
* **Zintegrowane próbki**. Zestaw narzędzi Azure dla Zaćmienie funkcji teraz kilka przykładowych aplikacji ułatwiające deweloperów rozpocząć pracę.
* **Integracja usługi HDInsight narzędzie**. Osoby Azure HDInsight narzędzia są teraz połączone z zestawu narzędzi Azure dla programu Eclipse. Aby uzyskać więcej informacji zobacz [Dodatek Narzędzia HDInsight dla programu Eclipse].
* **Zdalnego debugowania Java aplikacji sieci Web**. Zestaw narzędzi Azure dla Zaćmienie obsługuje teraz zdalne debugowanie aplikacji sieci web języka Java na Azure aplikacji usługi.
* **Obsługa wersji Luna Zaćmienie.** Nowa minimalne wymagane wersja IDE Zaćmienie jest Luna.

### <a name="april-12-2016"></a>12 kwietnia 2016

Zestaw narzędzi Azure dla Zaćmienie - wersji 2016 kwietnia zawiera następujące ulepszenia:

* **Obsługa v2.9.0 Azure SDK**. Najnowsza wersja pakietu Azure jest teraz minimalne wstępnie wymagane dla narzędzi Azure dla programu Eclipse.
* **Różne użyteczności, czas reakcji i wydajności ulepszenia dotyczące obsługi aplikacji sieci Web Azure**. Liczba optymalizacji wydajności w jak zestaw narzędzi komunikuje Azure wynik w usprawnia interfejsu użytkownika.
* **Możliwość usuwania istniejącego kontenera aplikacji sieci Web platformy Azure z poziomu Zaćmienie**. Zestaw narzędzi Azure dla Zaćmienie umożliwia teraz usunąć istniejącego kontenera Azure Web bez opuszczania Zaćmienie.

### <a name="march-7-2016"></a>7 marca 2016

Zestaw narzędzi Azure dla Zaćmienie - wersji 2016 marca zawiera następujące ulepszenia:

* **Obsługa szybkiego rozmieszczania lightweight aplikacje Java**. Zestaw narzędzi Azure dla Zaćmienie obsługuje teraz szybkiego rozmieszczania lightweight aplikacji Java do Azure kontenery aplikacji sieci Web, więc teraz wdrażania aplikacji Java trwa sekund zamiast minut.
* **Obsługa zarządzania aplikacji sieci Web przy użyciu widoku Eksplorator Azure**. Widok Eksploratora Azure w zestawie narzędzi obsługuje teraz pozycje, uruchamianie i zatrzymywanie Azure aplikacji sieci Web.
* **Aktualizacja Tomcat, molo, oraz Zulu OpenJDK**. Zestaw narzędzi Azure dla Zaćmienie zapewnia obsługę zaktualizowanej wersji Tomcat, molo i Zulu OpenJDK wdrożeniach Java do usług w chmurze Azure.

### <a name="january-4-2016"></a>4 stycznia 2016 r.

Zestaw narzędzi Azure dla Zaćmienie - wersji stycznia 2016 zawiera następujące ulepszenia:

* **Obsługa aktualizacji Zulu OpenJDK**. Aby uzyskać więcej informacji zobacz [strony sieci web systemów Azul dla Zulu OpenJDK].
* **Aktualizacja Tomcat i molo podziału**. Rozkład molo i Tomcat, które są dostępne w programie Microsoft Azure do użytku z zestawu narzędzi Azure dla programu Eclipse zostały zaktualizowane.
* **Funkcja zgodność pomiędzy Zaćmienie i narzędzi IntelliJ dla Azure**. Zestaw narzędzi Azure Zaćmienie i [Zestaw narzędzi Azure dla IntelliJ] obsługuje teraz ten sam zestaw funkcji.

### <a name="september-1-2015"></a>1 września 2015 r.

Zestaw narzędzi Azure dla Zaćmienie - wersji 2015 września zawiera następujące ulepszenia:

* **Obsługa aktualizacji Zulu OpenJDK**. Aby uzyskać więcej informacji zobacz [strony sieci web systemów Azul dla Zulu OpenJDK].
* **Aktualizacja Tomcat i molo podziału**. Rozkład molo i Tomcat, które są dostępne w programie Microsoft Azure do użytku z zestawu narzędzi Azure dla programu Eclipse zostały zaktualizowane. (Tych dystrybucji umożliwia programistom szybkie tworzenie i testowanie projektami przy użyciu narzędzi Azure dla programu Eclipse.
* **Obsługa aktualizowane automatycznie Tomcat i molo odwołania**. Oprócz dotyczące określonej wersji Tomcat i molo dostępnych na Azure, programista można teraz odwoływać się rozkładu nazywane "najnowszej (aktualizowane automatycznie)", którego zostanie automatycznie zaktualizowany najnowszą rozkład w każdej wersji głównych molo lub Tomcat przy następnym wystąpienia do roli są ponownie. (Odtwarzanie tworzony automatycznie, ale deweloperów można ręcznie wyzwalanie odtwarzania za pośrednictwem portalu Azure). Tej nowej funkcji oznacza, że programista nie trzeba ponownie wdróż ich stosowania, aby mieć możliwość aktualizacji oprogramowania serwera. (
*  Ta funkcja jest obecnie przeznaczony tylko do celów projektowania i testowania lub innych niż kluczowych aplikacji, a nie jest zalecane dla produkcji.)
* **Widok Eksploratora azure dla obiektów blob, kolejkach i tabel w magazynie Azure**. Dzięki temu deweloperzy mogą wykonywać zestaw typowych zadań za pomocą ich artefakty miejsca do magazynowania bezpośrednio z IDE Zaćmienie. Na przykład: usuwanie, przekazywania lub pobierania obiektów blob.

### <a name="august-1-2015"></a>1 sierpnia 2015 r.

Zestaw narzędzi Azure dla Zaćmienie - wersji 2015 sierpień zawiera następujące ulepszenia:

* **Zarządzania kluczami oprzyrządowania wniosków aplikacji**. Tej aktualizacji pozwala uzyskać, tworzyć i zarządzać nimi kluczy oprzyrządowania wniosków aplikacji bezpośrednio z IDE Zaćmienie.
* **Sterownik Microsoft JDBC 4.1 dla programu SQL Server**. Aktualizacja obsługę najnowszych sterowników JDBC dla programu Microsoft SQL Server.
* **Wersja 2.7 Azure SDK**. Ten najnowszą aktualizację Azure SDK jest nowe wstępnie wymagane dla zestawu narzędzi zainstalowany w systemie Windows. (Należy zauważyć, że nie jest to potrzebne w systemach operacyjnych Windows nie).
* **Obsługa 7 Zulu OpenJDK aktualizacji**. Aby uzyskać więcej informacji zobacz [strony sieci web systemów Azul dla Zulu OpenJDK].

### <a name="may-1-2015"></a>1 maj 2015 r.

Zestaw narzędzi Azure dla Zaćmienie - maj 2015 wersji zawiera następujące ulepszenia:

* **Ulepszone funkcje wybór serwera interfejsu użytkownika**. Ta wersja upraszcza korzystanie z narzędzi w systemach operacyjnych innych niż Windows.
* **Pomoc techniczna dla środowiska Maven projektów**. Ta wersja obsługuje projektów środowiska Maven jako aplikacje, które można wdrożyć pakiet Azure i konfigurowanie aplikacji wnioski.
* **Wersji 2.6 Azure SDK**. Ten najnowszą aktualizację Azure SDK jest nowe wstępnie wymagane dla zestawu narzędzi zainstalowany w systemie Windows. (Należy zauważyć, że nie jest to potrzebne w systemach operacyjnych Windows nie).
* **Uaktualnianie wdrożenia zamiast ponownie opublikować**. Jeśli poprzedniej wersji jest już live jest ponowne publikowanie projektu wdrożenia, pakiet teraz używa funkcji uaktualnienia wdrażania Azure w zamiast zamykania poprzedniego wdrożenia i ponowne publikowanie od podstaw, tak jak w przeszłości. Umożliwia do usługi w chmurze do działania bez zakłóceń, jeśli to możliwe, pomoc osiągnięcia wysokiej dostępności nawet podczas aktualizacji, to oraz przyspiesza ponowne publikowanie.
* **Obsługa najnowszych v8 Zulu OpenJDK — aktualizacja 40**. Aby uzyskać więcej informacji zobacz [strony sieci web systemów Azul dla Zulu OpenJDK].

### <a name="march-9-2015"></a>9 marca 2015 r.

Zestaw narzędzi Azure dla Zaćmienie - wersji marca 2015 r zawiera następujące ulepszenia:

* **Pomoc techniczna dla komputerów Mac, Ubuntu i dodatkowe podtypy Linux**. W tej wersji zestawu narzędzi Azure dla programu Eclipse są dodawane pomocy technicznej dla systemu Mac OS i kilka platformy systemu Unix, więc deweloperów można zainstalować zestaw narzędzi do tworzenia, konfigurowanie i publikowanie projektów języka Java Azure usługami w chmurze (PaaS) z Zaćmienie działa w systemach operacyjnych innych niż Windows.

>[AZURE.NOTE] Ta funkcja jest na podglądzie, a nie jest to zalecane do użytku w środowisku produkcyjnym. Nie ma żadnych pomoc techniczną dotyczącą Umowa dotycząca poziomu usług (SLA), ale wszystkie opinie jest zwiększyć i zalecane.

* **Dodatek nowe wnioski aplikacji**. Deweloperzy są teraz można skonfigurować telemetrycznego automatyczne server za pomocą aplikacji wniosków na Azure.
* **Automatyzacji wdrożenia oparte na który wiersza polecenia**. Ta funkcja umożliwia projektantom Automatyzowanie publikowania nowsze wersje wdrożeń przy użyciu który poza Zaćmienie. Skrypt wstępnie wygenerowanych jest konfigurowane automatycznie dla projektu, po pierwszym wdrożeniu z Zaćmienie, a kolejne wdrożenia zautomatyzować za pomocą skryptu pełni wdrożeń z wiersza polecenia.
* **Dostępność tomcat i molo Azure wdrożenia łatwiejszych i szybszych**. Deweloperów można teraz odwoływać się różnych wersji Tomcat i molo, które są dostępne w Azure bezpośrednio zamiast tego konieczność przekazywanie serwer Java do swoich kont (lub za pomocą narzędzi), więc zachodzi potrzeba przekazywanie serwer Java scenariuszach szybki i — wprowadzenie.
* **Szybka metoda publikowania Java aplikacji sieci web z usługami w chmurze Azure**. Aby zmniejszyć nauki dla proste scenariusze tworzenia i testowania, deweloperzy teraz można publikować aplikacje Java więcej bezpośrednio do Azure. Zamiast przechodzą przez proces tworzenia i konfigurowania projektu Azure wdrażania aplikacji zostanie wdrożony z w przypadku wystąpienia domyślnego Tomcat v8 i Zulu maszyny wirtualnej Java (OpenJDK).

### <a name="january-30-2015"></a>30 stycznia 2015 r.

Zestaw narzędzi Azure dla Zaćmienie - wersji stycznia 2015 r zawiera następujące ulepszenia:

* **Obsługa IBM® WebSphere® aplikacji serwera wolności Core**. Ta wersja dodaje IBM WebSphere aplikacji Server wolności Core do listy serwerów obsługiwanych aplikacji, z których jest niemożliwe Azure zestaw narzędzi. Ten najnowszy dodatek powoduje rozszerzenie bieżącą listę serwerów aplikacji, które są obsługiwane &quot;w nowym polu&quot; zestaw narzędzi, które już dołączone różne wersje Tomcat, molo, JBoss i GlassFish.
* **Włączenie aplikacji wniosków SDK**. Ta biblioteka nowo wydane interfejs API klienta (v0.9.0) jest teraz część pakietu Azure bibliotek Java.
* **Aktualizacja pakietu Azure bibliotek Java**. Zawiera ona bibliotek Azure Java v0.7.0 i v2.0.0 interfejsu API klienta miejsca do magazynowania, a także v0.9.0 SDK wniosków aplikacji nowo wydane.

### <a name="november-12-2014"></a>12 listopada 2014 r.

Zestaw Azure dla programu Eclipse — wersja z listopada 2014 zawiera następujące ulepszenia:

* **Obsługa Azure SDK 2,5**. Ten najnowszą aktualizację Azure SDK jest nowe wstępnie wymagane dla zestawu narzędzi.
* **Obsługa zaktualizowanej wersji Zulu OpenJDK v1.8, v1.7 i v1.6 pakietów**. Aby uzyskać więcej informacji zobacz [strony sieci web systemów Azul dla Zulu OpenJDK].
* **Obsługa nowe rozmiary standardowe D dla usług w chmurze**, które oferują zwiększenie wydajności i pamięci dodatkowe zasoby. Aby uzyskać więcej informacji zobacz [maszyn wirtualnych i rozmiarów usługi Cloud Azure].

### <a name="october-17-2014"></a>17 października 2014 r.

Zestaw narzędzi Azure dla Zaćmienie — wersja z października 2014 zawiera następujące ulepszenia:

* **Ulepszenia dotyczące wydajności w Publikuj w chmurze scenariuszy**. Ładowanie informacje o subskrypcji przebiega szybciej, jeśli użytkownicy mają wiele subskrypcji i kont miejsca do magazynowania.
* **Obsługa zaktualizowanej wersji pakietu v1.8 Zulu OpenJDK**. Aby uzyskać więcej informacji zobacz [strony sieci web systemów Azul dla Zulu OpenJDK].
* **Obsługa zaniechanie korzystania starszych wersji 3 JDKs innych firm**. Przestarzałe pakietów JDK już nie pojawi się w menu rozwijanym dla nowych projektów wdrożenia. Istniejące projekty odwołujący się przestarzałe pakietów JDK będą nadal jest stanie się po raz czasie, ale zaleca się uaktualnienie takich projektów zależne najnowsze informacje.
* **Aktualizacja pakiet w wersji dla bibliotek Azure Biblioteka interfejsu API języka Java klienta**. Aby uzyskać więcej informacji zobacz [Interfejsu API programu Microsoft Azure klienta].
* **Poprawki.** Ta wersja zawiera liczbę różne poprawki, oparte na raporty użytkownika i testowania.

### <a name="august-5-2014"></a>5 sierpnia 2014 r.

Zestaw Azure dla programu Eclipse — wersja z sierpnia 2014 zawiera następujące ulepszenia

* **Obsługa Azure SDK 2,4.** Starsze wersje programu narzędzi Zaćmienie nie zadziała z nowo opublikowanej zestaw SDK.
* **Zaktualizowane wersje v1.6 Zulu OpenJDK pakiety 1.7 i v1.8.** Aby uzyskać więcej informacji zobacz [strony sieci web systemów Azul dla Zulu OpenJDK].
* **Zaktualizowana wersja pakietu dla bibliotek Azure Biblioteka interfejsu API języka Java klienta.** Aby uzyskać więcej informacji zobacz [Interfejsu API programu Microsoft Azure klienta].
* **Obsługa r Publikuj ustawienia formatu pliku.** Dodano obsługę dla wersji 2.0 na format pliku ustawienia publikowania.
* **Zmiany w architekturze za Publikuj w chmurze funkcji.** Zestaw narzędzi używa teraz interfejsu API nowo opublikowanej Microsoft Azure klienta dla języka Java jej obsługi publikowanie w chmurze.
* **Poprawki.** Ta wersja zawiera numer poprawki wymagane przez użytkownika.

### <a name="june-12-2014"></a>12 czerwca 2014 r.

Zestaw narzędzi Azure dla Zaćmienie — wersja 2014 czerwca jest nieznaczną aktualizację obsługi, która udostępnia następujące ulepszenia:

* **Obsługa v1.8 pakietu Zulu OpenJDK.** Aby uzyskać więcej informacji zobacz [strony sieci web systemów Azul dla Zulu OpenJDK].
* **Zaktualizowane wersje v1.6 Zulu OpenJDK i 1.7 pakietów.** Aby uzyskać więcej informacji zobacz [strony sieci web systemów Azul dla Zulu OpenJDK].
* **Zaktualizowana wersja pakietu dla bibliotek Azure Biblioteka interfejsu API języka Java klienta.** Aby uzyskać więcej informacji zobacz [Interfejsu API programu Microsoft Azure klienta].
* **Poprawki.** Ta wersja zawiera numer poprawki wymagane przez użytkownika.

### <a name="april-4-2014"></a>4 kwietnia 2014 r.

Opublikowała dodatek Azure dla programu Eclipse — wersja z kwietnia 2014. Jest to aktualizacja towarzyszące wersji 2.3 SDK Azure jest wstępnie wymagane i będą pobierane automatycznie podczas instalacji wtyczki. Tej aktualizacji zawiera nowe funkcje, poprawki i kilka udoskonaleń sterowanych opinii użyteczności, ponieważ luty 2014 podglądu:

* **Obsługa wersji Azure SDK 2.3.** Dodatek Azure dla programu Eclipse — wersja z kwietnia 2014 wymaga Azure SDK 2.3. Korzystając z nowego wtyczkę, jeśli nie masz już Azure SDK 2.3, pojawi umożliwia jego instalację. Nie należy używać Azure SDK 2.3 z wcześniejszymi wersjami programu wtyczkę.
* **Uaktualnianie aplikacji bez kompletnego pakietu wdrażania.** Podczas rozmieszczania aplikacji Java, które są częścią projektu, wtyczkę teraz automatycznie przekaże je do swojego konta wybranego miejsca do magazynowania tak, aby można je zaktualizować i Odtwórz wystąpienia roli wdrożenia najnowszą bitów aplikacji bez konieczności odbudowanie i ponownie wdróż całego pakietu.
* **Tomcat 8 jest teraz serwer rozpoznany aplikacji.** Po wybraniu Tomcat 8 katalogu instalacji na komputerze, na karcie **serwer** w oknie **Projektu wdrażania Azure** wtyczkę teraz automatycznie wykryje go i mieć możliwość wdrażanie Tomcat 8 w sposób automatycznego podobne do starszych wersji Tomcat już na liście.
* **Aktualizacje pakietu Azul Zulu OpenJDK: Aktualizacja v1.7 51 i v1.6 aktualizowanie 47.** Skuteczna w tej wersji, Azul System JDK Otwórz Zulu 7 pakiet 51 dostępna jest aktualizacja. Również Otwórz JDK Zulu 6 pakietów uruchomić jest dostępna, począwszy od aktualizacji 47. Te aktualizacje są oprócz pakietu 7 JDK Otwórz Zulu dostępnej aktualizacji 45, 40 i aktualizowanie 25.
* **Obsługa rozmiaru A8 i A9 Microsoft Azure Virtual Machine.** Teraz można wdrożyć usługi w chmurze do pamięci wysokiej A8 i rozmiarów maszyn wirtualnych A9. Aby uzyskać więcej informacji na temat tych rozmiarów maszyn wirtualnych zobacz [maszyn wirtualnych i rozmiarów usługi Cloud Azure].
* **Automatyczne przekierowanie z HTTP na HTTPS dla ról włączony protokół SSL.** Usługa w chmurze zawiera tylko role HTTPS, jeśli żądanie użytkownika określa HTTP, spowoduje automatyczne przekierowanie na HTTPS. Istnieje nie trzeba utworzyć osobne rolę obsługującego żądania HTTP.
* **Można wyrazić emulatora używana dla lokalnej emulacji.** Azure Express Emulator teraz służy jako emulator debugowania aplikacji lokalnie.
* **Azure został przemianowany jako Microsoft Azure.** Ekranów interfejsu użytkownika odzwierciedlają teraz, że został przemianowany i nie jest już o nazwie Azure Azure.

### <a name="february-6-2014"></a>6 lutego 2014 r.

Dodatek Azure Zaćmienie — luty 2014 opublikowała Podgląd. Tej aktualizacji zawiera nowe funkcje, poprawki i kilka udoskonaleń sterowanych opinii użyteczności, ponieważ października 2013 Preview:

* **Obsługa odciążanie protokołu SSL.** Bezpieczny Odciążanie Sockets Layer (SSL) zostało dodane jako funkcji, co umożliwia łatwe Włącz obsługę Hypertext Transfer Protocol bezpiecznego (HTTPS) w wdrożenia Java Azure, nie jest wymagane do skonfigurowania protokołu SSL na serwerze aplikacji Java. To jest szczególnie istotne w sesji koligacji i/lub uwierzytelniony scenariusze komunikacji. Na przykład podczas korzystania z filtru usługi kontroli dostępu (ACS), który jest już obsługiwany przez pakiet. Aby uzyskać więcej informacji zobacz [Odciążanie protokołu SSL] i [sposobu użycia odciążanie protokołu SSL].
* **GlassFish 4 jest teraz serwer rozpoznany aplikacji.** Jeśli wybierzesz katalog instalacyjny GlassFish 4 na tym komputerze na karcie **serwer** w oknie **Projektu wdrażania Azure** , wtyczkę teraz automatycznie wykryje go i mieć możliwość wdrażanie GlassFish OSE 4 w zautomatyzowany sposób, podobnie jak w wersji GlassFish OSE 3 już na liście.
* **Aktualizacja pakietu Azul Zulu OpenJDK 45.** Skuteczna w tej wersji, Azul System Zulu (Otwórz JDK 7 pakiet) 45 dostępna jest aktualizacja; jest to oprócz dostępnej aktualizacji 40 i Aktualizuj 25.
* **Obsługa "auto" prywatnych porty punktu końcowego.** Można ustawić prywatnych port automatycznego dla wprowadzania punkty końcowe i wewnętrznych aby umożliwić Azure automatyczne przypisywanie portu do tego punktu końcowego. Wcześniej można przypisywać tylko określonego numeru portu.
* **Obsługa Dostosowywanie nazwa certyfikatu (CN) w tworzenie certyfikatu z podpisem własnym interfejsu użytkownika.** Wcześniej użyto tej samej nazwy stałe dla wszystkich nowych certyfikatów; Teraz możesz określić własną nazwę certyfikatu, aby odróżnić wiele certyfikatów w portalu Azure używane do różnych celów.
* **Azure narzędzi:** Azure pasek narzędzi zawiera zaktualizowany o następujące zmiany: 
    * ![][ic710876]Ta ikona został dodany **Nowy Projekt wdrożenia Azure**.
    * ![][ic710877]Ta ikona został dodany jako skrót do okna dialogowego Tworzenie certyfikatu z podpisem własnym.
* **Obsługa rozmiar maszyn wirtualnych Azure A5.** Teraz można wdrożyć usługi w chmurze do pamięci wysokiej rozmiar maszyn wirtualnych A5. Aby uzyskać więcej informacji o tym rozmiar pamięci Wirtualnej zobacz [maszyn wirtualnych i rozmiarów usługi Cloud Azure].
* **Pomocy technicznej dla systemu Microsoft Windows Server 2012 R2.** Można wybierać Windows Server 2012 R2 w chmurze system operacyjny.

### <a name="october-22-2013"></a>22 października 2013 r.

Dodatek Azure Zaćmienie — październik 2013 Preview został wydany. Tej aktualizacji zawiera nowe funkcje, poprawki i kilka udoskonaleń sterowanych opinii użyteczności, ponieważ wrzesień 2013 Preview:

* **Obsługa wersji Azure SDK 2.2.** Dodatek Azure dla Zaćmienie - października 2013 Preview obsługuje Azure SDK 2.2. Dodatek będzie nadal działać z Azure SDK 2.1 i automatycznie zainstaluje Azure SDK 2.2, jeśli nie masz już co najmniej Azure SDK 2.1 zainstalowany.
* **Aktualizacja pakietu Azul Zulu OpenJDK 40.** Jak przewidziano dla wrzesień 2013 Podgląd umożliwia teraz dodatek za pomocą JDK dostarczonego przez innych firm bezpośrednio na Azure, bez konieczności przekazywania własnych JDK. W wersji 2013 października Azul System Zulu (JDK Otwórz pakiet 7) 40 dostępna jest aktualizacja; jest to oprócz aktualizacji pierwotnie opublikowanych 25.
* **Łącze do chmury wdrażaniu w dzienniku aktywności.** W dzienniku aktywności Azure rozmieszczenia stanów **opublikowany**, można kliknąć pozycję **opublikowany** ponieważ jest teraz łącza do wdrożenia; następnie wdrożenia zostanie otwarty w przeglądarce. (Stan **opublikowany** został wcześniej oznaczony **uruchomione**.)
* **Wybór systemu operacyjnego docelowej, dostępnego pod adresem publikowanie czasu.** Okno dialogowe **Publikowanie Azure** zawiera nowego pola **Docelowego systemu operacyjnego**, który umożliwia bardziej odnajdowania można ustawić system operacyjny docelowej.
* **Automatyczne zastąpienie poprzedniego wdrożenia.** Okno dialogowe **Publikowanie Azure** zawiera nowe pole wyboru **Zastąp poprzedniego wdrożenia**. Jeśli ta opcja jest zaznaczona, kiedy nowy wdrożenia jest publikowana automatycznie spowoduje zastąpienie poprzedniego wdrożenia; czy nie występują &quot;409 konflikt&quot; problemy podczas publikowania w tej samej lokalizacji bez pierwszego publikowanie poprzedniego wdrożenia.
* **Molo 9 jest teraz serwer rozpoznany aplikacji.** Po wybraniu 9 molo katalogu instalacji na komputerze, na karcie **serwer** w oknie **Projektu wdrażania Azure** wtyczkę teraz automatycznie wykryje go i mieć możliwość wdrażanie 9 molo w sposób automatyczną podobne do starszych wersji molo już na liście.
* **Dodawanie roli z menu kontekstowego projektu.** Menu kontekstowe projekt **Azure** zawiera teraz nowy element menu **Dodawanie roli**, która udostępnia szybciej i więcej odnajdowania sposób dodać nową rolę do Azure projektu.
* **Aktualizacja dla pakietu bibliotek Azure Java biblioteki.** To jest oparty na wersję 0.4.6 [Interfejsu API programu Microsoft Azure klienta].

### <a name="september-25-2013"></a>25 wrzesień 2013

Dodatek Azure Zaćmienie — wrzesień 2013 Preview został wydany. Tej aktualizacji zawiera nowe funkcje, poprawki i kilka udoskonaleń sterowanych opinii użyteczności, ponieważ sierpień 2013 Preview:

* **Możliwość wdrażania pakietu Azul Zulu OpenJDK dostępne na Azure.** Nowa opcja został dodany, określając JDK korzystać z wdrożeniem Azure. Przy użyciu tej opcji, należy wdrożyć pakiet JDK innych firm bezpośrednio w chmurze Azure, bez konieczności przekazać własne. Systemy Azul udostępnia pierwszej takie pakowanie nazywane Zulu, oparte na OpenJDK, który teraz można wdrożyć przy użyciu tej opcji.
* **Aktualizacja dla pakietu bibliotek Azure Java biblioteki.** To jest oparty na wersję 0.4.5 [Interfejsu API programu Microsoft Azure klienta].

### <a name="august-1-2013"></a>1 sierpnia 2013

Dodatek Azure Zaćmienie — sierpień 2013 Preview został wydany. Jest to aktualizacja towarzyszących wersji 2.1 SDK Azure jest wstępnie wymagane i będą pobierane automatycznie podczas instalacji wtyczki. Tej aktualizacji zawiera nowe funkcje, poprawki i kilka udoskonaleń sterowanych opinii użyteczności, ponieważ lipca 2013 Preview:

* **Usuwanie opcje, aby uwzględnić serwera miejscowego i lokalnych JDK jako część pakietu wdrażania.** Pobieranie zestawu JDK i serwer aplikacji z magazynu w chmurze podczas wdrażania jest pożądane osadzenie składniki w pakiecie, od pobierania wyników elementów w mniejszym rozmiarze pakietu wdrażania, krótszy czas wdrażania i konserwacji łatwiejsze. W wyniku opcje, aby uwzględnić JDK i serwer aplikacji w pakiecie wdrażania zostały usunięte. Istniejące projektów, które zostały skonfigurowane do włączenia JDK lokalne oraz miejscowego serwera jako część pakietu wdrażania automatycznie zostaną przekonwertowane na automatycznego wysyłania JDK i serwer aplikacji do magazynu w chmurze.
* **Obsługa wersji Azure SDK 2.1.** Dodatek Azure Zaćmienie — sierpień 2013 Preview wymaga Azure SDK 2.1. Nie należy używać podglądu sierpień 2013 we wcześniejszych wersjach programu Azure SDK, a nie należy używać Azure SDK 2.1 z wcześniejszymi wersjami programu wtyczki Azure Zaćmienie.
* **Obsługa wersji Kepler Zaćmienie.** Związane z tym, nowy minimalny wymagany jest wersja IDE Zaćmienie Indigo. Dodatek Azure dla programu Eclipse już urzędowo jest sprawdzany na Helios.

### <a name="july-3-2013"></a>3 lipca 2013

Dodatek Azure Zaćmienie — lipiec 2013 Preview został wydany. Tej aktualizacji zawiera nowe funkcje, poprawki i kilka udoskonaleń sterowanych opinii użyteczności, ponieważ maja 2013 Preview:

* **Możliwość tworzenia nowego konta miejsca do magazynowania.** Przycisk **Nowy** został dodany do okna dialogowego **Dodawanie konta miejsca do magazynowania** . Umożliwia tworzenie konta miejsca do magazynowania w ramach wtyczki Zaćmienie bez konieczności logowania się w portalu zarządzania Azure. (Wcześniej trzeba Azure subskrypcji, aby użyć tej funkcji). Aby uzyskać więcej informacji na temat tworzenia nowego konta miejsca do magazynowania zobacz [Tworzenie nowego konta miejsca do magazynowania].
* **Nowy &quot;(automatycznie)&quot; opcji dla konta magazynu używane do automatycznego wdrożenia JDK i serwera oraz w pamięci podręcznej.** Podczas korzystania z opcji **automatycznie przekazywać** dla zestawu JDK i serwer aplikacji, możesz teraz podać **(automatycznie)** na adres URL i miejsca do magazynowania konto używane podczas przekazywania JDK i serwer aplikacji lub podczas korzystania z pamięci podręcznej Azure. Następnie te funkcje będą automatycznie używać tego samego konta miejsca do magazynowania, wybierz w oknie dialogowym **Publikowanie Azure** . [Tworzenie aplikacji Witaj świecie dla Azure w Zaćmienie] samouczek została zaktualizowana do korzystania z nowych opcji **(automatyczne)** .
* **Możliwość ustawienia usługi Azure punktów końcowych.** Określ punkty końcowe usługi, które określają, czy aplikacja jest używany i zarządzane przez globalnej platformy Azure, Azure obsługiwana przez firmę 21Vianet w Chinach lub prywatną platformy Azure. Aby uzyskać więcej informacji zobacz [Punkty końcowe usługi Azure].
* **Duże wdrożeniach można określić zasób magazynu lokalnego.** W przypadku, gdy wdrożenia jest zbyt duża, aby być zawarte w domyślnym folderze approot, możesz teraz podać zasób magazynu lokalnego jako miejsca docelowego wdrożenia usługi JDK i serwer aplikacji. Aby uzyskać więcej informacji zobacz [Wdrażanie dużych wdrożeń].
* **Obsługa rozmiarów A6 i A7 maszyn wirtualnych Azure.** Teraz można wdrożyć usługi w chmurze do pamięci wysokiej A6 i A7 maszyn wirtualnych rozmiarów. Aby uzyskać więcej informacji na temat tych rozmiarów zobacz [maszyn wirtualnych i rozmiarów usługi Cloud Azure].
* **Aktualizacja dla pakietu bibliotek Azure Java biblioteki.** To jest oparty na wersję 0.4.4 [Interfejsu API programu Microsoft Azure klienta].

### <a name="may-1-2013"></a>1 maj 2013

Dodatek Azure dla Zaćmienie - może 2013 Preview opublikowała. Jest to aktualizacja głównych towarzyszących z wersji 2.0 SDK Azure, które jest wstępnie wymagane i będą pobierane automatycznie podczas instalacji wtyczki. Ta wersja zawiera nowe funkcje, poprawki i kilka udoskonaleń sterowanych opinii użyteczności od lutego 2013 Preview:

* **Automatyczne przekazywanie JDK serwera aplikacji i wdrażania z magazynu Azure.** Opcja nowy, która automatycznie przekazywanie zaznaczonego JDK i serwer aplikacji, w razie potrzeby, na konto Azure określonej i wdrożeniu jej składniki stamtąd zamiast osadzania w pakiecie wdrażania lub konieczności przekazywania użytkownika następnie ręcznie. Ta funkcja często wymagane można znacznie zwiększyć ułatwienia wdrażania składniki JDK i serwera, szczególnie dla początkujących użytkowników. Aby uzyskać samouczek, która korzysta z tych opcji zobacz [Tworzenie Witaj świecie stosowania Azure w Zaćmienie].
* **Scentralizowane konto miejsca do magazynowania śledzenia i możliwość magazynowania konta odwołania więcej łatwo (za pomocą kontrolki listy rozwijanej).** Dotyczy to wiele funkcji, które korzystają z miejscem do magazynowania, takich jak JDK i wdrażanie składnika serwera oraz pamięci podręcznej. Aby uzyskać więcej informacji zobacz [Listę kont Azure miejsca do magazynowania].
* **Uproszczone ustawienia dostępu zdalnego w oknie Publikowanie w Kreatorze chmury.** To wszystko, co należy zrobić wpisz nazwę użytkownika i hasło do Włączanie dostępu zdalnego lub pozostaw to pole puste, aby zachować dostępu zdalnego wyłączone.
* **Aktualizacja dla pakietu bibliotek Azure Java biblioteki.** To jest oparty na wersję 0.4.2 [Interfejsu API programu Microsoft Azure klienta].
* **Obsługa lepkich sesji w systemie Windows Server 2012.** Sesji lepkich pracowała tylko w systemie Windows Server 2008 R2, teraz zarówno w chmurze koligacji sesji pomocy technicznej elementów docelowych systemu operacyjnego.
* **Ulepszenia dotyczące wydajności przekazywania pakiet.** Nawet wtedy, gdy JDK i serwer aplikacji są osadzone w pakiecie wdrażania, przekazywania część procesu wdrażania może być około dwukrotnie szybkie w porównaniu z poprzednich wersji.

### <a name="february-8-2013"></a>8 lutego 2013

Dodatek Azure Zaćmienie — lutego 2013 Preview został wydany. Jest to aktualizacja pomocniczą, która zawiera poprawki, ulepszenia sterowanych opinii użyteczności i niektórych nowych funkcji, ponieważ Podgląd listopad 2012:

* Obsługa wdrażania JDKs, serwery aplikacji i dowolnego inne składniki z platformy Azure publiczny czy prywatny blob pobierania miejsca do magazynowania zamiast tym ich w pakiecie wdrażania, wdrażając w chmurze.
* Zmienianie kolejności przetwarzania zdefiniowane przez użytkownika składniki roli, przez dodanie przycisków **Przenieś w górę** i **Przenieś w dół** w sekcji **składniki** **Azure właściwości roli**możliwość.
* Aktualizacja w bibliotece **pakiet bibliotek Azure Java** , zależnie od wersji 0.4.0 [Interfejsu API programu Microsoft Azure klienta].

### <a name="november-5-2012"></a>5 listopad 2012

Dodatek Azure Zaćmienie — listopad 2012 opublikowała Preview. Oto główne aktualizacji, która zawiera wiele nowych funkcji, a także dodatkowe poprawki i udoskonalenia sterowanych opinii użyteczności, ponieważ Podgląd 2012 września:

* Pomocy technicznej dla systemu Microsoft Windows Server 2012 w chmurze system operacyjny.
* Obsługa Azure znajdują się buforowania pomocy technicznej dla klientów memcached.
* Dołączenie bibliotek klienta Apache Qpid JMS dla skorzystać z systemem Azure AMQP wiadomości.
* Ulepszone Kreator **Nowego projektu** z nową stronę na końcu, umożliwiająca użytkownikom możliwość szybkiego włączania kilka typowych najważniejszych funkcjach w jego projektu: lepkich sesje, pamięci podręcznej i zdalne debugowanie.
* Automatyczne zmniejszanie wystąpień roli 1, gdy działa emulatora obliczeń, aby uniknąć konfliktów portów powiązania między wystąpieniami serwera.

### <a name="september-28-2012"></a>28 września 2012

Dodatek Azure Zaćmienie — wrzesień 2012 opublikowała Podgląd. Tej aktualizacji usługi zawiera kilka dodatkowych poprawki, ponieważ sierpnia 2012 wyświetlić podgląd, a także niektóre udoskonalenia użyteczności sterowanych opinii w istniejących funkcji:

* Obsługa programu Microsoft Windows 8 i Microsoft Windows Server 2012 jako rozwoju systemu operacyjnego, rozwiązywanie problemów, które wcześniej mogli wtyczkę działa poprawnie w tych systemach operacyjnych.
* Ulepszona obsługa określać zakresy port punktu końcowego.
* Poprawki związane z ścieżki plików zawierających spacje.
* Rola ulepszenia menu kontekstowego szybszy dostęp do ustawień konfiguracji poszczególnych ról.
* Pomocnicze uściślenia w kreatorze **publikowania do chmury** oraz liczby kolejne poprawki.

### <a name="august-28-2012"></a>28 sierpnia 2012

Dodatek Azure Zaćmienie — sierpnia 2012 opublikowała Podgląd. Tej aktualizacji usługi zawiera dodatkowe poprawki, ponieważ lipiec 2012 wyświetlić podgląd, a także kilka udoskonaleń sterowanych opinii użyteczności dla istniejących funkcji:

* W oknie dialogowym Filtr usług kontroli dostępu Azure:
    * **Można osadzać certyfikatu podpisywania** w pliku też aplikacji wdrażaniu chmury.
    * **Opcję, aby utworzyć certyfikat z podpisem własnym** w filtrze ACS interfejsu użytkownika. Aby uzyskać dodatkowe informacje na temat filtru usług kontroli dostępu Azure zobacz [jak służą do uwierzytelniania użytkowników sieci Web z Zaćmienie przy użyciu usługi kontroli dostępu Azure].
* W Kreatorze Azure wdrożenia projektu (dotyczy również programu strona właściwości konfiguracji serwera roli):
    * **Automatycznego wykrywania JDK lokalizację** na komputerze (który można zmienić w razie potrzeby).
    * **Funkcja automatycznego wykrywania serwera typu** po wybraniu katalogu instalacji aplikacji serwera.

### <a name="july-15-2012"></a>15 lipiec 2012

Dodatek Azure Zaćmienie — lipiec 2012 opublikowała Podgląd adresy liczbę najwyższy priorytet usterek znaleziono i/lub zgłoszone przez użytkowników po wydaniu czerwca 2012. Jest to aktualizacja usługi, znajdują się nie nowych funkcji.

### <a name="june-7-2012"></a>7 czerwca 2012

Azure dodatek dla programu Eclipse — czerwca 2012 CTP opublikowała. Nowe funkcje:

* **Kreator nowego projektu wdrożenia Azure:** Umożliwia wybranie usługi JDK serwer aplikacji Java i aplikacje Java bezpośrednio w Kreatorze udoskonalony interfejs użytkownika. Na liście konfiguracji serwera w nowym polu wybierać obejmuje Tomcat 6, Tomcat 7, GlassFish OSE 3, molo 7, 8 molo, JBoss 6 i 7 JBoss (autonomiczny). Ponadto można dostosować listę konfiguracji serwera. To ulepszenie interfejsu użytkownika może być używany zamiast przeciąganie i upuszczanie plików skompresowany skopiowaniu na uruchamianie skryptów, co wcześniej było ujęciu głównym. Ta metoda nadal działa prawidłowo, ale prawdopodobnie będzie używany tylko dla bardziej zaawansowanych scenariuszy.
* **Strona właściwości roli konfiguracji serwera:** Umożliwia łatwe przełączanie JDKs, serwerach aplikacji Java i aplikacje skojarzone z wdrożeniem po utworzeniu projektu. Aby uzyskać więcej informacji zobacz [Właściwości konfiguracji serwera].
* **&quot;Publikowanie w chmurze&quot; kreatora:** umożliwia łatwe do wdrożenia projektu Azure bezpośrednio z Zaćmienie, automatyzowanie wcześniej ręcznego ciężkich podnoszenie: pobieranie poświadczeń, logowanie się do portalu zarządzania Azure przekazywanie pakietu itp. Na przykład bezpośrednio wdrażania projektu Azure zobacz [Tworzenie Witaj świecie stosowania Azure w Zaćmienie].
* **Azure narzędzi:** Azure narzędzi jest teraz dostępny w Zaćmienie przyciskami, który wywołania następujące funkcje:
    * ![][ic710879]**Uruchamianie w emulatora Azure**: działa projektu w emulatorze.
    * ![][ic710880]**Resetowanie emulatora Azure**: resetuje emulatorze.
    * ![][ic710881]**Tworzenie pakietu chmura Azure**: kompilowanie opakowaniu do wdrożenia.
    * ![][ic710876]**Nowy Projekt wdrożenia Azure**: tworzy nowy projekt Azure wdrożenia.
    * ![][ic710882]**Publikuj w chmurze Azure**: publikuje projektu Azure.
    * ![][ic710883]**Cofnij publikowanie**: usuwa wdrożenia.
    * Wiele z tych przycisków Azure narzędzi są używane podczas [tworzenia Witaj świecie stosowania Azure w Zaćmienie].
* **Azure bibliotek dla języka Java:** Teraz dostępna w ramach pojedynczy pakiet dla bibliotek Azure w bibliotece Java programu Zaćmienie, towarzyszące instalacji wtyczki i zawierający wszystkie niezbędne zależności także. Po prostu Dodaj jedno odwołanie do biblioteki w projekcie Java i nie musisz nic pobierać oddzielnie. Aby uzyskać więcej informacji zobacz [Instalowanie narzędzi Azure dla programu Eclipse].
* **4.0 sterownik JDBC firmy Microsoft dla programu SQL Server dostępne podczas instalacji wtyczki:** Podczas instalacji wtyczki nowy można zainstalować najnowszą wersję sterownik JDBC firmy Microsoft dla programu SQL Server.
* **Dostępne podczas instalacji wtyczki filtr usługi kontroli dostępu azure:** Ten nowy składnik dostępne jako biblioteki Zaćmienie w zestawie narzędzi umożliwia aplikacji sieci web Java bezproblemowo skorzystać z uwierzytelniania usługi kontroli dostępu Azure (ACS) za pomocą różnych dostawców tożsamości, takich jak Google, Live.com i Yahoo!. Nie będzie wymagało pisanie logiczny uwierzytelniania, samodzielnie, po prostu skonfigurować kilka opcji i umożliwić filtr wykonaj lifting ciężkich umożliwienie użytkownikom zalogować się przy użyciu ACS. Możesz po prostu skoncentrować się na pisania kodu, który umożliwia użytkownikom dostęp do zasobów opartych na jego tożsamości, zwracane do aplikacji przez filtr wewnątrz obiektu wezwanie. Samouczek dotyczący przy użyciu filtru ACS zobacz [jak służą do uwierzytelniania użytkowników sieci Web z Zaćmienie przy użyciu usługi kontroli dostępu Azure].
* **Automatycznego wykrywania wstępny Azure SDK 1.7:** Po utworzeniu nowego projektu wdrożenia Azure Azure SDK 1.7 będą automatycznie pobierane, jeśli nie jest już zainstalowany.
* **Wystąpienia punkty końcowe:** Umożliwia dostęp punktu końcowego bezpośredni portów dla komunikacji z wystąpieniami roli równoważenia obciążenia. Punkty końcowe wystąpienie można dodawać za pośrednictwem interfejsu użytkownika, dostępne na stronie [Właściwości punkty końcowe] punkty końcowe. Dzięki temu Włączanie debugowania zdalnego i JMX informacje diagnostyczne dla określonych obliczyć uruchomionych w chmurze w scenariuszach z wielu wystąpień-wystąpienia wdrożenia. 
* **Składniki interfejsu użytkownika:** Ułatwia zaawansowanych użytkowników skonfigurować projektu zależności między poszczególnych ról Azure w projekcie i inne zasoby zewnętrzne, takie jak projekty aplikacji Java; ułatwia także powinno zawierać opis ich logiczny wdrożenia. Aby uzyskać więcej informacji zobacz [Właściwości składników].
* **Uaktualnianie wcześniejszych wersji projektów:** Po otwarciu obszaru roboczego zawierającego Azure projektu utworzonego przy użyciu poprzedniej wersji wtyczkę starych projektów będą wyświetlane w Zaćmienie zamknięty, ponieważ poprzednich wersji projektów nie są zgodne z nowej wersji. Jeśli spróbujesz otworzyć jeden z tych starych projektów, zostanie uruchomiony Kreator uaktualnienia. Jeśli akceptujesz uaktualnienie nowego projektu z **_Upgraded** dołączonym do nazwy, zostanie utworzona i automatycznie aktualizowane do pracy z nowej wersji. W razie potrzeby można zmienić nazwę nowego projektu. W ramach uaktualniania oryginalnego projektu nie można zmodyfikować (i pozostanie zamknięty).

### <a name="december-10-2011"></a>10 grudzień 2011

Azure dodatek dla programu Eclipse — grudzień 2011 CTP opublikowała. Nowe funkcje:

* **Koligacji sesji (&quot;lepkich sesji&quot;) obsługuje:** Pomoc w Włącz stateful, wykres kolumnowy grupowany aplikacje Java przy użyciu tylko jednego pola wyboru. Aby uzyskać więcej informacji zobacz [Koligacje sesji].
* **Wstępnie wprowadzone przykłady skryptów uruchamiania:** Najpopularniejsze Java serwerów (Tomcat molo, JBoss, GlassFish), które możesz możesz po prostu skopiować/wkleić z katalogu próbki projektu do skryptu uruchamiania.
* **Emulatora uruchamiania wyjścia w czasie rzeczywistym:** Możesz teraz obejrzeć wykonanie odpowiednich kroków będzie łatwiejsze ze skryptu uruchamiania w oknie konsoli dedykowane pokazywanie błędów i postępu w skrypt jako wykonane Azure.
* **Monitorowania automatycznego, uproszczonej java.exe:** Który spowoduje wymuszenie Kosza roli, gdy java.exe przestanie działać, za pomocą skryptu uproszczonego, gotowych automatycznie dołączone wdrożenia.
* **Aplikacji Java zdalnego debugowania interfejsu użytkownika konfiguracji:** Pozwala na łatwe Włączanie debugowania zdalnego Zaćmienie osoby uzyskać dostęp do aplikacji Java uruchomiony emulatorze lub w chmurze Azure, aby można było krok po kroku i debugowanie kodu języka Java w czasie rzeczywistym. Aby uzyskać więcej informacji zobacz [Debugowanie aplikacji Azure w Zaćmienie].
* **Interfejsu użytkownika konfiguracji zasobów magazynu lokalnego:** Dlatego masz już konfigurowania zasobów lokalnych za bezpośrednio manipulowania XML. Ta funkcja umożliwia dostęp do skutecznych ścieżkę do zasobów lokalnych po wdrożeniu za pośrednictwem zmiennej środowiska, które można odwoływać się bezpośrednio ze skryptu uruchamiania. Aby uzyskać więcej informacji zobacz [właściwości magazynu lokalnego].
* **Konfiguracji zmiennych środowiska interfejsu użytkownika:** Dlatego nie jest już trzeba przełączyć zmienne środowiska za pomocą edycji ręcznej konfiguracji XML. Aby uzyskać więcej informacji zobacz [właściwości zmiennych środowiska].
* **JDBC sterownik platformy SQL Azure:** Jest instalowana przez wtyczkę zintegrowanych biblioteki Zaćmienie włączaniem łatwiejszego programowania przed platformy SQL Azure. 
* **Szybkie menu kontekstowego dostęp do roli konfiguracji interfejsu użytkownika**: po prostu kliknij prawym przyciskiem myszy folder ról i kliknij polecenie **Właściwości**.
* **Ikony folderu projektu i roli Azure niestandardowe:** W przypadku poprawić widoczność obrazu i ułatwienia nawigacji w Twojej workspace i project.

## <a name="see-also"></a>Zobacz też ##

Aby uzyskać więcej informacji na temat narzędzi Azure dla języka Java IDEs zobacz poniższe łącza:

- [Azure zestaw narzędzi dla programu Eclipse]
  - [Instalowanie narzędzi Azure dla programu Eclipse]
  - [Tworzenie aplikacji sieci Web dla Witaj świecie dla Azure w Zaćmienie]
  - *Co nowego w Azure zestaw narzędzi dla Zaćmienie (ten artykuł)*
- [Azure zestaw narzędzi dla IntelliJ]
  - [Instalowanie narzędzi Azure dla IntelliJ]
  - [Tworzenie aplikacji sieci Web dla Witaj świecie dla Azure w IntelliJ]
  - [Co nowego w Azure zestaw narzędzi dla IntelliJ]

Aby uzyskać więcej informacji na temat Azure za pomocą języka Java zobacz [Centrum deweloperów języka Java Azure].

<!-- URL List -->

[Azure zestaw narzędzi dla programu Eclipse]: ./azure-toolkit-for-eclipse.md
[Azure zestaw narzędzi dla IntelliJ]: ./azure-toolkit-for-intellij.md
[Tworzenie aplikacji sieci Web dla Witaj świecie dla Azure w Zaćmienie]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Tworzenie aplikacji sieci Web dla Witaj świecie dla Azure w IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Instalowanie narzędzi Azure dla programu Eclipse]: ./azure-toolkit-for-eclipse-installation.md
[Instalowanie narzędzi Azure dla IntelliJ]: ./azure-toolkit-for-intellij-installation.md
[What's New in the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md
[Co nowego w Azure zestaw narzędzi dla IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md

[Centrum deweloperów języka Java Azure]: http://go.microsoft.com/fwlink/?LinkID=699547

[Systemy Azul strony sieci web dla Zulu OpenJDK]: http://go.microsoft.com/fwlink/?LinkId=402457
[Usługa Azure punkty końcowe]: http://go.microsoft.com/fwlink/?LinkID=699526
[Listy kont Azure miejsca do magazynowania]: http://go.microsoft.com/fwlink/?LinkID=699528
[Właściwości składników]: http://go.microsoft.com/fwlink/?LinkID=699525#components_properties
[Tworzenie aplikacji Witaj świecie dla Azure w Zaćmienie]: http://go.microsoft.com/fwlink/?LinkID=699533
[Debugowanie aplikacji Azure w Zaćmienie]: http://go.microsoft.com/fwlink/?LinkID=699535
[Wdrażanie dużych wdrożeń]: http://go.microsoft.com/fwlink/?LinkID=699536
[Właściwości punktów końcowych]: http://go.microsoft.com/fwlink/?LinkID=699525#endpoints_properties
[Właściwości zmiennych środowiska]: http://go.microsoft.com/fwlink/?LinkID=699525#environment_variables_properties
[Usługa HDInsight narzędzia dodatek dla programu Eclipse]: ./hdinsight/hdinsight-apache-spark-eclipse-tool-plugin.md
[Sposób uwierzytelniania użytkowników w sieci Web z usługą Kontrola dostępu Azure za pomocą Zaćmienie]: http://go.microsoft.com/fwlink/?LinkID=264703
[Jak używać odciążanie protokołu SSL]: http://go.microsoft.com/fwlink/?LinkID=699545
[Instalowanie narzędzi Azure dla programu Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546
[Właściwości magazynu lokalnego]: http://go.microsoft.com/fwlink/?LinkID=699525#local_storage_properties
[Interfejs API klienta Microsoft Azure]: http://go.microsoft.com/fwlink/?LinkId=280397
[Właściwości konfiguracji serwera]: http://go.microsoft.com/fwlink/?LinkID=699525#server_configuration_properties
[Koligacja sesji]: http://go.microsoft.com/fwlink/?LinkID=699548
[Odciążanie protokołu SSL]: http://go.microsoft.com/fwlink/?LinkID=699549
[Aby utworzyć nowe konto miejsca do magazynowania]: http://go.microsoft.com/fwlink/?LinkID=699528#create_new
[Maszyn wirtualnych i rozmiarów usługi Cloud Azure]: http://go.microsoft.com/fwlink/?LinkId=466520

<!-- IMG List -->

[ic710876]: ./media/azure-toolkit-for-eclipse-whats-new/ic710876.png
[ic710877]: ./media/azure-toolkit-for-eclipse-whats-new/ic710877.png
[ic710879]: ./media/azure-toolkit-for-eclipse-whats-new/ic710879.png
[ic710880]: ./media/azure-toolkit-for-eclipse-whats-new/ic710880.png
[ic710881]: ./media/azure-toolkit-for-eclipse-whats-new/ic710881.png
[ic710876]: ./media/azure-toolkit-for-eclipse-whats-new/ic710876.png
[ic710882]: ./media/azure-toolkit-for-eclipse-whats-new/ic710882.png
[ic710883]: ./media/azure-toolkit-for-eclipse-whats-new/ic710883.png
