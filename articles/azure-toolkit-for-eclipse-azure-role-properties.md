<properties
    pageTitle="Właściwości roli Azure"
    description="Dowiedz się, jak skonfigurować ustawienia roli Azure za pomocą narzędzi Azure dla programu Eclipse."
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
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690945.aspx -->

# <a name="azure-role-properties"></a>Właściwości roli Azure #

Różne ustawienia konfiguracji rolą Azure można ustawić w ramach zestawu narzędzi Azure dla programu Eclipse.

## <a name="configuring-azure-role-properties"></a>Konfigurowanie właściwości roli Azure ##

Konfigurowanie właściwości roli Azure jest realizowana za pośrednictwem okna właściwości dla roli użytkownika pracownika. Otwórz menu kontekstowego dla roli w okienku Eksplorator projektu Zaćmienie osoby i wybierz podmenu **Azure** . (Jeśli nie widzisz roli w oknie Eksploratora projektu Zaćmienie, rozwiń Azure projektu w programie Project Explorer).

![][ic789599]

W tym temacie opisano różne właściwości, które można ustawić w oknach dialogowych **Właściwości** . Należy zauważyć, że wiele właściwości są wypełnione automatycznie podczas tworzenia nowego projektu Azure wdrożenia.

Następujące strony właściwości są dostępne dla ról Azure.

* [Właściwości maszyn wirtualnych](#virtual_machine_properties)
* [Właściwości buforowania](#caching_properties)
* [Właściwości certyfikatów](#certificates_properties)
* [Właściwości składników](#components_properties)
* [Debugowanie właściwości](#debugging_properties)
* [Właściwości punktów końcowych](#endpoints_properties)
* [Właściwości zmiennych środowiska](#environment_variables_properties)
* [Równoważenia obciążenia i właściwości sesji koligacji (alias "lepkich sesji")](#session_affinity_properties)
* [Właściwości magazynu lokalnego](#local_storage_properties)
* [Właściwości konfiguracji serwera](#server_configuration_properties)
* [SSL Odciążanie właściwości](#ssl_offloading_properties)
    
<a name="virtual_machine_properties"></a>
### <a name="virtual-machine-properties"></a>Właściwości maszyn wirtualnych ###

Otwórz menu kontekstowego dla roli w okienku Eksplorator projektu i Zaćmienie kliknij polecenie **Azure**, a następnie kliknij polecenie **Właściwości**, i będzie mieć możliwość zmiany rozmiaru maszyn wirtualnych, a także zmienić liczbę wystąpień, jak pokazano na poniższej ilustracji.

![][ic719499]

>[AZURE.NOTE] Tylko w systemie Windows: po ustawieniu liczbę wystąpień wartości większe niż 1, a także skonfigurować serwer aplikacji, zestaw narzędzi, aby umożliwić tylko 1 wystąpienie roli do uruchamiania w emulatorze, bez względu na to ustawienie. To, aby uniknąć konfliktów portów powiązania między wystąpieniami inny serwer (na przykład wszystkie próby powiązania z portem 8080) uruchomienie na tym samym komputerze. Liczba wystąpienia odpowiednie ustawienie jest zachowywany, ale jest uruchamiana tylko wtedy, gdy wdrażanie w chmurze.

<a name="caching_properties"></a> 
### <a name="caching-properties"></a>Właściwości buforowania ###

Otwieranie menu kontekstowego dla roli w okienku Eksplorator projektu Zaćmienie osoby, kliknij pozycję **Azure**, a następnie kliknij **buforowanie**. W tym oknie dialogowym można włączyć nazwanych znajdują się memcache zgodny z pamięci podręcznej, co pozwala przyspieszyć aplikacji sieci web.

![][ic719483]

Na stronie właściwości **buforowanie** można określić ustawienia globalne dla następujących czynności:

* czy znajdują się buforowanie jest włączone.
* rozmiar pamięci podręcznej jako procent pamięci.
* Nazwa konta miejsca do magazynowania dla Zapisywanie stanu pamięci podręcznej, po uruchomieniu aplikacji jako usługi w chmurze lub Brak, jeśli nie chcesz zapisywać stanie pamięci podręcznej. (Nazwę konta magazynu nie jest używane podczas uruchamiania aplikacji w emulatorze obliczeń.) Jeśli ustawisz nazwę konta magazynu **(automatycznie)** (jest to ustawienie domyślne), konfiguracji buforowania będzie automatycznie używać tego samego konta miejsca do magazynowania, który wybierz w oknie dialogowym **Publikowanie Azure** .

>[AZURE.NOTE] Ustawienie **(automatycznie)** ma uwzględnione tylko wtedy, gdy publikowanie wdrożenia przy użyciu narzędzi Zaćmienie Kreator publikacji. Jeśli zamiast tego możesz opublikować plik .cspkg ręcznie za pomocą mechanizm zewnętrznych, takich jak do [Portalu zarządzania Azure][]rozmieszczenia nie będzie działać prawidłowo.

To okno dialogowe wyświetlane właściwości w pamięci podręcznej.

![][ic719501]

* **Nazwa:** Nazwa znajdują się pamięci podręcznej.
* **Numer portu:** Numer portu, aby użyć pamięci podręcznej.
* **Zasad wygasania:** Jedną z następujących wartości, które umożliwia określenie, gdy klucza w pamięci podręcznej wygaśnie.
    * **Bezwzględne:** Klucz wygasa po osiągnięciu z wybranym **minut na żywo** .
    * **NeverExpires:** Klucz nie ma czasu wygaśnięcia.
    * **SlidingWindow:** Klucz wygaśnie, jeśli nie była używana przez czas określony przez **minut na żywo**. zawsze, gdy jest on dostępny, Zresetuj zegar wygaśnięcia.
* **Minut na żywo:** Maksymalna liczba minut na klucz memcached na żywo zasadom wygaśnięcia.
* **Wysokiej dostępności z zreplikowanej kopie zapasowe na inną rolę wystąpienia:** Jeśli włączono tę funkcję, ułatwia zapewnienie wysokiej dostępności wykorzystanie replikowane kopie zapasowe na inną rolę wystąpienia. Należy zauważyć, że co najmniej dwa wystąpienia roli muszą być w celu rozmieszczania dla tej funkcji do pracy.

Aby dodać nową pamięć podręczną, kliknij przycisk **Dodaj** na stronie właściwości **buforowanie** i okno dialogowe **Konfigurowanie pamięci podręcznej o nazwie** zostanie otwarty. Podaj wartości właściwości, które zostały opisane powyżej.

Aby zmodyfikować nazwanych pamięci podręcznej, zaznacz pamięci podręcznej, a następnie kliknij przycisk **Edytuj** na stronie właściwości **buforowanie** . Okno dialogowe zostanie otwarty, co pozwala modyfikować właściwości pamięci podręcznej. Naciśnij **przycisk OK** , aby zapisać wartości pamięci podręcznej.

Aby usunąć pamięci podręcznej, wybierz pozycję pamięci podręcznej i kliknij przycisk **Usuń** na stronie właściwości **buforowanie** , a następnie kliknij **Tak,** aby potwierdzić usunięcie.

Aby uzyskać więcej informacji na temat korzystania z pamięci podręcznej zobacz [jak za pomocą Co-located pamięci podręcznej][].

<a name="certificates_properties"></a> 
### <a name="certificates-properties"></a>Właściwości certyfikatów ###

Otwieranie menu kontekstowego dla roli w okienku Eksplorator projektu Zaćmienie osoby, kliknij pozycję **Azure**, a następnie kliknij **Certyfikaty**.

![][ic710964]

W tym oknie dialogowym można dodawać lub usuwanie odwołują się projektu Zaćmienie certyfikaty. Zauważ, że certyfikaty wymienione tutaj nie są automatycznie zapisywane w dowolnego elementu keystore Java i dlatego nie są automatycznie dostępne dla dowolnego z aplikacji Java. Tylko są one rejestrowane z Azure, dzięki czemu może być załadowane w systemie Windows certyfikat przechowywanie w przypadku maszyn wirtualnych uruchomiony wdrożenia i następnie używane przez inne oprogramowanie systemu Windows. Obecnie funkcja tylko zestaw narzędzi, używającej certyfikaty, do których odwołuje się w oknie dialogowym **certyfikatów** w ten sposób jest [Odciążanie protokołu SSL][], ze względu na jej zależność od informacji usług internetowych i aplikacji żądanie routingu (ARR), które wymagają odpowiedni certyfikat mają być dostępne w ten sposób.

Po wdrożeniu projektu Azure za pomocą Kreatora publikowania zostanie wyświetlony monit o wskaż plików wymiany informacji osobistych (PFX) odpowiadającą te certyfikaty wraz z ich haseł, aby automatycznie przekazywać ich z usługą Azure, tylko wtedy, gdy ta osoba nie zostały przekazane wcześniej.

<a name="components_properties"></a> 
### <a name="components-properties"></a>Właściwości składników ###

Otwieranie menu kontekstowego dla roli w okienku Eksplorator projektu Zaćmienie osoby, kliknij pozycję **Azure**, a następnie kliknij **składniki**. W tym oknie dialogowym możesz mieć możliwość Dodawanie, modyfikowanie, lub Usuń składniki roli użytkownika, a także zmienianie kolejności, w jakiej są przetwarzane.

![][ic719502]

Funkcja składniki umożliwia dodawanie zależności do projektu wdrożenia Azure, takie jak projekty aplikacji Java, specjalne pliki i instrukcje wykonywalny wiersza polecenia, które są wymagane przez wdrożenia.

Dla każdego składnika można określić:

* Etap, jakie należy podjąć podczas importowania składnika do projektu wdrożenia Azure, gdy jest tworzona.
* Etap, jakie należy podjąć w przypadku wdrażania tego składnika w chmurze Azure.

>[AZURE.NOTE] Określając składnik pliki lub wiersze polecenia pamiętać, że rozmieszczenia będzie opublikowany maszyn wirtualnych systemu Windows, aby etapów niestandardowe musi być prawidłowa dla systemu operacyjnego Windows. 

Składniki mają następujące właściwości:

* **Importu:** Metoda, która wskazuje, jak składnik zostaną zaimportowane do projektu podczas tworzenia projektu. Może to być jedna z następujących wartości:
    * **Kopiuj:** Składnik jest kopiowany z lokalnego ścieżka określona przez właściwość **z** w katalogu **approot** roli.
    * **Wyczyść:** Składnik jest zaimportowane z projektu aplikacji przedsiębiorstwa w lokalnej ścieżce określonej przez właściwość **z** archiwum przedsiębiorstwa (Wyczyść) Java. (To zostanie wykryty automatycznie przez zestaw narzędzi, w zależności od rodzaju projektu w tej lokalizacji).
    * **JAR:** Składnik jest archiwum Java (SŁOIK) i importowania z projektu Java w lokalnej ścieżce określonej przez właściwość **z** . (To zostanie wykryty automatycznie przez zestaw narzędzi, w zależności od rodzaju projektu w tej lokalizacji).
    * **Brak:** Nie, jest przyjmowana do zaimportowania składnika. Może to być stosowane, gdy składnik uznaje się już w katalogu **approot** roli, lub gdy składnik jest jedynie wykonywalny wiersza polecenia instrukcji, zgodnie z **jako** właściwość po metody **Deploy** **wykonywalna**.
    * **Też:** Składnik jest Java archiwum aplikacji sieci web (też) i importowania z dynamicznego projektu sieci Web w lokalnej ścieżce określonej przez właściwość **z** . (To zostanie wykryty automatycznie przez zestaw narzędzi, w zależności od rodzaju projektu w tej lokalizacji).
    * **pocztowy:** Składnik jest pliku zip i zaimportowaniu przez pakowanie katalogu lub określony przez właściwość **z** pliku.
* **Z:** Ścieżka źródłowa na komputerze lokalnym do folderu lub pliku, który reprezentuje elementów do zaimportowania do wdrożenia. Zmienne środowiska systemu Windows można użyć w tej właściwości. Wszystkie składniki importowane zostaną zaimportowane do tej roli **approot** katalogu podczas tworzenia projektu.
    
    Należy zauważyć, że masz możliwość wdrożenia składnik pobierać, wdrażając w chmurze (nie emulatorze obliczeń). Wyświetlanie informacji pokrewnych poniżej informacje o dodawaniu składnik.    
    
* **As:** Nazwa pliku, w którym składnika zostaną zaimportowane do tej roli **approot** katalogów i ostatecznie wdrożony w chmurze Azure. Pozostaw puste pole, aby zachować tę nazwę tak samo jak na komputerze lokalnym dla tej właściwości. (Wykonywalny składników, oznacza to, że te których metody **Deploy** ustawiono **wykonywalna**, może to być instrukcji dowolnego wiersza polecenia systemu Windows.)

    >[AZURE.IMPORTANT] Użycie znaków spacji dla tej wartości, będą one inaczej obsługiwane w zależności od metody deploy. Jeśli metody deploy jest **wykonywalna**, spacje będą interpretowane jako separatory argumentów wiersza polecenia, a nie jako część nazwy pliku. Dla wszystkich innych wdrażanie metod, spacje będzie interpretowany jako część nazwy pliku.
    
* **Wdrażanie:** Metoda, która wskazuje akcję zastosowane do składnika po uruchomieniu wdrożenia. Może to być jedna z następujących wartości:
    * **Kopiuj:** Składnik są kopiowane do ścieżki docelowej określonej przez właściwość **do** .
    * **wykonywalna:** Składnik jest wykonywalny instrukcji wiersza polecenia systemu Windows wykonywana w kontekście ścieżka określona przez właściwość **do** w momencie rozpoczęcia rozmieszczenia.
    * **Brak:** Nie jest stosowana do składnika wraz z wdrożenia.
    * **pocztowy:** Składnik jest rozpakowane do ścieżki docelowej określonej przez właściwość **do** . Ta metoda jest dostępna tylko wtedy, gdy właściwość **Importowanie** **zip**.
* **To:** Ścieżka docelowa na komputerze wirtualnych miejsce, w którym zostanie wdrożony składnik. Zmienne środowiska systemu Windows mogą być używane w tej właściwości i ścieżki pliku są względem **approot**.
    
Aby dodać nowy składnik, kliknij przycisk **Dodaj** na stronie właściwości **składników** , a zostaną otwarte okno dialogowe **Składnik roli Azure** . Podaj wartości właściwości, które zostały opisane powyżej. 

Poniżej przedstawiono przykład dodawania nowy składnik też.

![][ic719503]

Wdrażając w chmurze (nie emulatora obliczeń), jeśli chcesz wdrożyć pobierać, upewnij się, że jest zaznaczona **w chmurze, a nie w pakiecie, w tym wdrażanie z** . Jeśli chcesz pobrać z Twojego konta Azure miejsca do magazynowania, wybierz pozycję przechowywania konto z listy rozwijanej **kont miejsca do magazynowania** (można kliknąć łącze **konta** , aby zmodyfikować, co ma na liście), które częściowo wstawi w polu **adres URL** , a następnie wypełnij pozostała część adresu URL. Jeśli nie chcesz używać Azure miejsca do magazynowania, zaznacz pozycję **(Brak)** z listy rozwijanej **kont miejsca do magazynowania** , a następnie wprowadź adres URL w polu **adres URL** składnika. Określ jedną z następujących metod:

* **Kopiuj:** Składnik pobrać są kopiowane do ścieżki docelowej określony przez ścieżkę **Do katalogu** .
* **samej:** Tej samej metody dla **Rozmieszczanie możliwość pobierania** dla **Rozmieszczanie z pakietu**.
* **pocztowy:** Składnik pobierania jest rozpakowane do ścieżki docelowej określony przez ścieżkę **Do katalogu** .

Aby zmodyfikować składnik, wybierz składnik, a następnie kliknij przycisk **Edytuj** na stronie właściwości **składników** . Okno dialogowe zostanie otwarty, co pozwala modyfikować właściwości składnika. Naciśnij **przycisk OK** , aby zapisać wartości składowych.

Aby usunąć składnik, wybierz składnik i kliknij przycisk **Usuń** na stronie właściwości **składników** , a następnie kliknij przycisk **Tak,** aby potwierdzić usunięcie.

Składniki są przetwarzane w kolejności podanej na liście. Użyj przycisków **Przenieś w górę** i **Przenieś w dół** , aby rozmieścić kolejności.

>[AZURE.NOTE] Funkcja konfiguracji serwera zależy od także składniki. Te składniki nie można usunąć lub edytować bez usuwania odpowiednich konfiguracji serwera. Pojawi się monit o który podczas próby wprowadzanie zmian w tymi elementami.

<a name="debugging_properties"></a> 
### <a name="debugging-properties"></a>Debugowanie właściwości ###

Otwieranie menu kontekstowego dla roli w Zaćmienie w okienku Eksplorator projektu, kliknij pozycję **Azure**, a następnie kliknij **Debugowanie**. W tym oknie dialogowym możesz mieć możliwość włączenia lub wyłączenia zdalnego debugowania, a także tworzyć konfiguracje debugowania, jak pokazano na poniższej ilustracji.

![][ic719504]

Pokrewne informacje dotyczące debugowania zobacz [Debugowanie aplikacji Azure w Zaćmienie][].

<a name="endpoints_properties"></a> 
### <a name="endpoints-properties"></a>Właściwości punktów końcowych ###

Otwieranie menu kontekstowego dla roli w okienku Eksplorator projektu Zaćmienie osoby, kliknij pozycję **Azure**, a następnie kliknij **punktów końcowych**. W tym oknie dialogowym masz możliwość tworzenie punktu końcowego, a także edytować lub usunąć punktu końcowego, jak pokazano na poniższej ilustracji.

![][ic719505]

Aby dodać punkt końcowy, kliknij przycisk **Dodaj** na stronie właściwości **punkty końcowe** , a zostaną otwarte okno dialogowe **Dodawanie punktu końcowego** .

![][ic710897]

Wprowadź nazwę dla punktu końcowego, wybierz odpowiedni typ ( **danych wejściowych**, **wewnętrzny**lub **InstanceInput**) i określ port publiczne i prywatne. Naciśnij **przycisk OK** , aby zapisać nowe wartości punktu końcowego.

W zależności od typu punkt końcowy można użyć zakresy portów w następujący sposób:

* Dla punktu końcowego wprowadzania wystąpienie publicznej port może być zakresie portów (na przykład **2000 – 2010**) i prywatnych port jest wartością stałą.
* Dla punktu końcowego wewnętrznych publicznej port nie jest używany i prywatnych port może być zakresu lub pozostanie puste lub ustaw gwiazdkę, aby wskazać ją jest automatycznie ustawiany przez Azure.
* Dla punktów końcowych wprowadzania danych publiczne portu może zawierać tylko wartości stałej, a prywatne portu może być wartością stałą lub pozostanie puste lub ustawienie gwiazdka wskazuje, że jest on automatycznie ustawiony przez Azure.

Jeśli chcesz używać numeru portu pojedynczy zamiast zakresu, pozostaw pole tekstowe na koniec puste pole zakres.

Dla portów, które są ustawione na automatyczne, gdy trzeba określić, który port faktycznie jest używany podczas wykonywania, aplikacja może używać interfejs API środowisko uruchomieniowe usługi Azure, w którym opisano w [pakiet com.microsoft.windowsazure.serviceruntime podsumowania][].

Aby zobaczyć, jak można wprowadzania punkty końcowe wystąpienie ułatwiające debugowania wdrażania wielu wystąpień, zobacz [Debugowanie wystąpieniem konkretnej roli we wdrożeniu wielu wystąpień][].

Aby zmodyfikować punktu końcowego, wybierz punkt końcowy, a następnie kliknij przycisk **Edytuj** na stronie właściwości **punktów końcowych** . Okno dialogowe zostanie otwarty, co pozwala modyfikować Nazwa punktu końcowego, typu i porty publiczne i prywatne. Naciśnij **przycisk OK** , aby zapisać wartości zmienione punktu końcowego.

Aby usunąć punkt końcowy, wybierz punkt końcowy i kliknij przycisk **Usuń** na stronie właściwości **punkty końcowe** , a następnie kliknij przycisk **Tak,** aby potwierdzić usunięcie.

Aby poprawnie skonfigurować niektóre funkcje (na przykład buforowanie, zdalne debugowanie, koligacji sesji lub SSL Odciążanie) włączona przez użytkownika z uprawnieniami, zestaw narzędzi może automatycznie konfigurować specjalne punkty końcowe, wymienione wraz z punktów końcowych zdefiniowanych przez użytkownika. Zestaw narzędzi uniemożliwia edytowanie lub usuwanie takich automatycznie wygenerowane punkty końcowe, jak skojarzone funkcja jest włączona.

<a name="environment_variables_properties"></a> 
### <a name="environment-variables-properties"></a>Właściwości zmiennych środowiska ###

Otwieranie menu kontekstowego dla roli w okienku Eksplorator projektu Zaćmienie osoby, kliknij pozycję **Azure**, a następnie kliknij **Zmiennych środowiska**. W tym oknie dialogowym masz możliwość Tworzenie zmiennej środowiska, a także modyfikowanie lub usuwanie zmiennej środowiska, jak pokazano na poniższej ilustracji.

![][ic719506]

Zmienne środowiska są dostępne do skryptu uruchamiania, wraz z roli.

>[AZURE.NOTE] Określając zmienne środowiska, należy pamiętać o tym, że wdrożenia będzie opublikowany maszyn wirtualnych systemu Windows, aby zmiennych środowiska musi być prawidłowa dla systemu operacyjnego Windows.

Jako przykład zmiennej środowiska są dostępne, wraz z roli Utwórz nową zmienną środowiska, klikając przycisk **Dodaj** . Poniżej przedstawiono zmiennej środowiska o nazwie **MyRoleVersion** zostanie utworzone i przydzielone wartość **1.0**.

![][ic659268]

W kodzie jsp można wyświetlić za pomocą wartość `System.getenv` metody:

    <body>
      <b> Hello World!</b>
      <p>Running role version: <%= System.getenv("MyRoleVersion") %></p>
    </body>

Ostateczny wynik ten po uruchomieniu aplikacji:

![][ic552233]

Aby zmodyfikować zmienną środowiska, wybierz zmienną środowiska i kliknij przycisk **Edytuj** na stronie właściwości **Zmiennych środowiska** . Okno dialogowe zostanie otwarty, co pozwala modyfikować właściwości zmiennych środowiska. Naciśnij **przycisk OK** , aby zapisać środowiska wartościami zmiennych.

Aby usunąć zmienną środowiska, wybierz zmienną środowiska i kliknij przycisk **Usuń** na stronie właściwości **Zmienne środowiska** , a następnie kliknij **Tak,** aby potwierdzić usunięcie.

Aby można było poprawnie skonfigurować niektóre funkcje (na przykład konfiguracji serwera zdalnego debugowania lub magazynu lokalnego) włączona przez użytkownika z uprawnieniami, pakiet automatycznie skonfigurować zmienne środowiska specjalne, wymienione wraz z zmienne środowiska zdefiniowane przez użytkownika. Zestaw narzędzi uniemożliwia edytowanie lub usuwanie takich automatycznie wygenerowane zmienne środowiska, jak skojarzone funkcja jest włączona.

<a name="session_affinity_properties"></a> 
### <a name="load-balancing--session-affinity-aka-sticky-sessions-properties"></a>Równoważenia obciążenia i właściwości sesji koligacji (alias "lepkich sesji") ###

Otwieranie menu kontekstowego dla roli w Zaćmienie w okienku Eksplorator projektu, kliknij pozycję **Azure**, a następnie kliknij **Równoważenia obciążenia**. W tym oknie dialogowym masz możliwość włączania lub wyłączania koligacja sesji, jak pokazano na poniższej ilustracji.

![][ic719492]

Aby uzyskać odpowiednie informacje zobacz [Koligacje sesji][]. Należy również zauważyć zachowania tej funkcji w kontekście odciążanie protokołu SSL, zgodnie z opisem w [Odciążanie protokołu SSL][].

<a name="local_storage_properties"></a> 
### <a name="local-storage-properties"></a>Właściwości magazynu lokalnego ###

Otwieranie menu kontekstowego dla roli w okienku Eksplorator projektu Zaćmienie osoby, kliknij pozycję **Azure**, a następnie kliknij **Magazynu lokalnego**. W ramach tego okna dialogowego możesz mieć możliwość tworzenie, modyfikowanie lub usuwanie tymczasowych magazynu lokalnego dla maszyny wirtualnej, która jest uruchomiona aplikacja. Określone wartości można ustawić rozmiaru magazynu lokalnego, a także czy zawartości są zachowywane podczas roli zostanie odtworzony, jak pokazano na poniższej ilustracji.

![][ic719508]

Można również opcjonalnie określić zmiennej środowiska, który odpowiada magazynu lokalnego.

Domyślnie wszystkie zasoby, które wdrażanie do Azure jest umieszczony (i rozpakowane) w folderze **approot** wystąpienia roli. Będzie najbardziej proste wdrożeń dopasowania nawet po rozpakować miejsca przydzielonego do katalogu **approot** jest ograniczony, a nie przejrzyste (mniej niż 1 GB jest rozsądne zasada). W związku z tym aby upewnić się, że Azure przydziela dostateczna ilość miejsca w przypadku dużych wdrożeń, które mogą nie mieszczą się w folderze **approot** , możesz należy skonfigurować zasób magazynu lokalnego przy użyciu okna dialogowego **Magazynu lokalnego** . Aby uzyskać łatwy sposób to zrobić, zobacz [Wdrażanie dużych wdrożeń][].

Można łatwo odwoływać się zasób miejsca do magazynowania z skrypty uruchamiania (na przykład z **startup.cmd**) przy użyciu zmiennej środowiska automatycznie skojarzony przez zestaw narzędzi Zaćmienie zasobu, jak pokazano w oknie dialogowym **Magazynu lokalnego** . Tej zmiennej środowiska będzie zawierać pełną ścieżkę zasobów lokalnych, który skonfigurowano w czasie wykonywania skrypt uruchamiania. 

Aby zmodyfikować zasób magazynu lokalnego, wybierz zasób magazynu lokalnego i kliknij przycisk **Edytuj** na stronie właściwości **Magazynu lokalnego** . Okno dialogowe zostanie otwarty, co pozwala modyfikować właściwości zasobu magazynu lokalnego. Naciśnij **przycisk OK** , aby zapisać wartości zasobu magazynu lokalnego.

Aby usunąć zasób magazynu lokalnego, wybierz zasób magazynu lokalnego i kliknij przycisk **Usuń** na stronie właściwości **Magazynu lokalnego** , a następnie kliknij **Tak,** aby potwierdzić usunięcie.

<a name="server_configuration_properties"></a> 
### <a name="server-configuration-properties"></a>Właściwości konfiguracji serwera ###

Otwieranie menu kontekstowego dla roli w okienku Eksplorator projektu Zaćmienie osoby, kliknij pozycję **Azure**, a następnie kliknij **Konfiguracji serwera**. W tym oknie dialogowym mają możliwość dodawania, usuwania i modyfikowania JDK i Java serwer aplikacji używane przez wdrożenia, jak dodać lub usunąć aplikacji (na przykład plików też, SŁOIK lub wyczyść) używane przez wdrożenia.

### <a name="jdk-configuration"></a>Konfiguracja JDK ###

To okno dialogowe umożliwia określenie pakiet JDK w celu rozmieszczania. Jeśli używasz Zaćmienie w systemie Windows, można określić pakietu JDK używać lokalnie, gdy działa emulatorze Azure i istnieje możliwość wdrożenia lokalnego instalacji Azure. W systemach operacyjnych Windows nie ustawienie JDK emulatora nie dotyczy i nie można wdrożyć JDK zainstalowanymi lokalnie, ponieważ nie jest zgodna z systemem Windows. Jednak niezależnie od systemu operacyjnego, którego używasz, zawsze możesz między 3 pakietów JDK firmy wdrażanie Azure lub wskaż pakietu JDK zgodnych z systemem Windows z lokalizacji pobierania alternatywny.

Oto przykładowy sposób można określić JDK w systemie Windows:

![][ic780647]

Jeśli używasz Zaćmienie w systemie Windows, można określić JDK używać emulatorze obliczeń; w tym celu należy upewnić się, że zaznaczono **Użyj JDK z ta ścieżka pliku do testowania lokalnie** w sekcji **wdrożenia emulatora** . Następnie określ ścieżkę lokalną do swojego JDK; Jeśli ten, który ma być używany nie jest zaznaczone automatycznie, można przejść do innego JDK. Masz również możliwość wdrażanie usługi JDK z usługą Azure chmury; Aby to zrobić, wybierz odpowiednią opcję **Rozmieszczanie mojej lokalnej JDK (automatyczne przekazywanie do do magazynu w chmurze)** w sekcji **wdrożenia chmury** .

Uwaga: W systemach operacyjnych innych niż Windows, ustawienia **wdrażania emulatora** i opcja **Rozmieszczanie mojej lokalnej JDK** nie są dostępne. Poniższy przykład przedstawia, określając JDK na komputerze Mac lub innych obsługiwanego systemu operacyjnego — Windows:

![][ic789643]

Niezależnie od używanego systemu operacyjnego masz dwóch poniższych opcji **wdrażania chmury** dla źródła i typu pakietu JDK:

* **Wdrażanie 3 strona JDK pakiet dostępny na Azure** 
* **Wdrażanie niestandardowych pobierać** 

Jeśli korzystasz z opcją **Rozmieszczanie 3rd strony pakietu JDK dostępne z platformy Azure** :

1. Zaznacz pole wyboru o nazwie **Rozmieszczanie 3rd strony pakietu JDK udostępniła Azure**.
1. Z listy rozwijanej wybierz 3 pakiet JDK strony, który jest dostępny na Azure.
1. Tabulatory **JDK** będzie wyglądać podobnie do poniższego systemu Windows:  ![][ic780648] 
    i będzie wyglądać podobnie do następujących w systemie Mac OS lub inne obsługiwane systemy operacyjne — Windows: ![][ic789643]
1. Kliknij **przycisk OK** , aby zapisać zmiany.
1. Gdy zostanie wyświetlony monit, aby zaakceptować umowę licencyjną z 3 JDK pakiet dostawcy, z postanowieniami licencyjnymi. Zakładając, że Zaakceptuj warunki, kliknij przycisk **Tak,** aby zamknąć okno dialogowe **Zaakceptuj Umowę licencyjną** .
    Uwaga można dostosować źródłowych logiczny, dla którego elementy są wyświetlane na liście rozwijanej dla opcji **Rozmieszczanie 3rd strony pakietu JDK udostępniła Azure** . Aby dostosować elementy w oknie dialogowym **JDK** , kliknij łącze **Dostosuj** . Spowoduje to zamknij stronę właściwości **JDK** i Otwórz plik **componentsets.xml** w Zaćmienie, który można następnie zmodyfikuj stosownie do potrzeb. Dokumentacji **componentsets.xml** znajduje się w pliku **componentsets.xml** .

Jeśli korzystasz z opcji **rozmieszczania JDK pobierać niestandardowe** :

1. Tworzenie ZIP katalogu instalacji JDK zapewnia, że sam węzeł katalogu jest element podrzędny struktury ZIP, a nie jego zawartość. Zanotuj nazwę katalogu, zostaną była potrzebna później, a należy pamiętać, które ten JDK zostanie ona wdrożona maszyn wirtualnych systemu Windows.
1. Przekaż ZIP do konta usługi Azure magazynowania jako obiektu blob. Jak ten narzędzie zewnętrznie dostępne dla przekazywania BLOB do magazynu Azure. Zaleca się używanie prywatnych obiektów blob. Zwróć uwagę na adres URL obiektów blob treści ZIP.
1. Zaznacz pole wyboru o nazwie **Rozmieszczanie JDK pobierać niestandardowe**.
    Jeśli chcesz pobrać z Twojego konta Azure miejsca do magazynowania, wybierz pozycję przechowywania konto z listy rozwijanej **kont miejsca do magazynowania** (można kliknąć łącze **konta** , aby zmodyfikować, co ma na liście), które częściowo wstawi w polu **adres URL** , a następnie wypełnij pozostała część adresu URL. Jeśli nie chcesz używać Azure miejsca do magazynowania, zaznacz pozycję **(Brak)** z listy rozwijanej **kont miejsca do magazynowania** , a następnie wprowadź adres URL w polu **adres URL** pobierania JDK. Jeśli przy użyciu magazynu Azure, musi być mała nazw obiektów blob w adresie URL.
1. Upewnij się, że pole tekstowe **JAVA_HOME** odwołuje się do nazwy właściwą. Domyślnie go będzie odwoływać się do tej samej nazwie katalogu JDK jako wartość wybrana przeznaczony do użytku lokalnego. Jednak jeśli katalogu zawarte w ZIP ma inną nazwę (na przykład z powodu przy użyciu różnych wersji), zaktualizować katalogu nazwy w polu tekstowym **JAVA_HOME** w związku z tym, ponieważ to ustawienie zostanie użyte w chmurze (nie w emulatorze obliczeń).
1. Kliknij **przycisk OK** , aby zapisać zmiany.

To wszystko. Teraz podczas tworzenia dla chmury można zauważyć będzie znacznie mniejszy rozmiar pakietu, procesu tworzenia zwykle powinna zająć mniej czasu i wdrażanie się podczas publikowania w chmurze Poświęć mniej czasu. Należy zauważyć, że opcje **Rozmieszczanie mojej lokalnej JDK (automatyczne przekazywanie do do magazynu w chmurze)** lub **Rozmieszczanie JDK niestandardowe pobierać** obowiązywać tylko, gdy aplikacja zostanie wdrożony w chmurze. Nie mają wpływu na środowiska emulatora obliczeń; Wersja lokalna składników nadal będzie używana, po wdrożeniu emulatora obliczeń. 

### <a name="server-configuration"></a>Konfiguracji serwera ###

Oto przykładowy sposób można określić serwer aplikacji.

![][ic796926]

Upewnij się, że jest zaznaczone pole wyboru **Wdrażanie serwera tego typu** , a następnie wybierz typ serwera aplikacji, który ma być używany.

Do określania serwera służących do wdrażania chmurze, możesz korzystać z następujących opcji:

1. **Rozmieszczanie 3rd strony serwera dostępne na Azure** - dotyczy szczególnie w scenariuszach deweloperów/testowanie gdzie wdrażania i uproszczenia jest priorytet i serwer wymaga konfiguracji niestandardowej. Lub gdy chcesz użyć jednego z tych serwerów jako punkt początkowy, ale możesz umieścić dostosowywania odpowiedniego serwera procedura Logika uruchamiania z wdrożeniem.
1. **Wdrażanie niestandardowych pobierać** - dotyczy szczególnie w scenariuszach produkcji zawierających specjalnie przygotowanych i skonfigurowany serwera, który ma być używany w chmurze.
1. **Rozmieszczanie Moja instalacja lokalnego serwera** — to dotyczy szczególnie w instalacji lokalnego serwera jest już skonfigurowane niestandardowe przeznaczony do użytku. Jeśli wybierzesz tę opcję, musisz także określić ścieżkę serwera lokalnego w polu tekstowym **ścieżka lokalnego serwera** poniżej.

Jeśli korzystasz z opcją **Rozmieszczanie 3rd strony serwera dostępne na Azure** :

1. Zaznacz pole wyboru o nazwie **Rozmieszczanie 3rd strony serwera dostępne na Azure**.
1. Z menu rozwijanego wybierz oprogramowania żądany serwer do wdrożenia w chmurze. Uwaga, jeśli już określony typ serwera ma być używany wcześniej, będzie ograniczona do wybierania tylko serwer chmury, który znajduje się w tej samej rodziny jako odpowiedni typ serwera. Jednak jeśli nie wybrano typ serwera, można wybierać spośród wszystkich serwerów, które są obecnie dostępne Azure i typ serwera zostanie wybrany automatycznie dla Ciebie.
1. Kliknij **przycisk OK** , aby zapisać zmiany.

Jeśli za pomocą opcji **Rozmieszczanie pobierać niestandardowy** :

1. Upewnij się, wybrał typ serwera zgodnie z powyższych kroków. Informuje wtyczkę, jak wdrożyć na serwerze z pobierania niestandardowych, jako musi być z tej samej rodziny jako typ wybranego serwera.
1. Zaznacz pole wyboru o nazwie **Rozmieszczanie pobierać niestandardowe**.
    Jeśli chcesz pobrać z konta usługi Azure miejsca do magazynowania, wybierz pozycję przechowywania konto z listy rozwijanej **kont miejsca do magazynowania** (można kliknąć łącze **konta** , aby zmodyfikować, co ma na liście), które częściowo wstawi w polu **adres URL** , a następnie wypełnij pozostała część adresu URL do swojego serwera Pobierz ZIP (w przypadku korzystania z magazynu Azure, nazwy obiektów blob w adresie URL musi być mała). Jeśli nie chcesz używać Azure miejsca do magazynowania, zaznacz pozycję **(Brak)** z listy rozwijanej **kont miejsca do magazynowania** , a następnie wprowadź adres URL serwera pobierania ZIP w polu **adres URL** . ZIP zawiera folderze podrzędnym reprezentujące katalogu instalacji aplikacji serwera. Na przykład jeśli korzystasz z zip dla Tomcat Apache 7.0.35, w ramach zip będzie folder podrzędny reprezentujący katalogu instalacji, takich jak **apache-tomcat-7.0.35**. 
1. Określ wartości zmiennej środowiska katalogu macierzystego. Domyślnie użyta zostanie wartość używana dla serwera miejscowego, jeśli jakieś, ale można określić inną wartość, jeśli serwer aplikacji chmury różni się od serwera lokalnego aplikacji. Jednak trzeba upewnij się, że serwer aplikacji chmurze jest z tej samej rodziny, co serwer wcześniej wybranego typu.
    Po zaktualizowaniu chmury pocztowy serwer aplikacji w przyszłości, można ręcznie zmienić ustawienie katalogu macierzystego lub, aby go ponownie zgodne z ustawienie lokalne, (Jeśli zmienione jest zbyt serwera miejscowego).
1. Kliknij **przycisk OK** , aby zapisać zmiany.

Logika źródłowych, dla którego elementy są wyświetlane na karcie **serwer** na stronie właściwości **Konfiguracji serwera** można dostosować. To zaawansowana funkcja, która może być konieczne, jeśli potrzeb przekroczenie wartości domyślnych lub jeśli chcesz dodać inny serwer. Aby dostosować logiki, w oknie dialogowym **serwera** , kliknij łącze **Dostosuj** . Spowoduje to zamknij stronę właściwości **Konfiguracji serwera** i Otwórz plik **componentsets.xml** w Zaćmienie, które następnie można zmieniać, aby rozszerzyć szablon konfiguracji serwera. Dokumentacji **componentsets.xml** znajduje się w pliku **componentsets.xml** .

Jeśli korzystasz z opcją **Rozmieszczanie Moje lokalnego serwera (automatyczne przekazywanie do do magazynu w chmurze)** :

1. Zaznacz pole wyboru o nazwie **Rozmieszczanie Moje lokalnego serwera (automatyczne przekazywanie do do magazynu w chmurze)**.
1. Z listy rozwijanej **konta miejsca do magazynowania** , wybierz opcję **(automatycznie)**. Jeśli użytkownik określi **(automatycznie)** w tym miejscu, zestaw narzędzi Zaćmienie będzie używać tego samego konta miejsca do magazynowania dla serwera, który zostanie wybrana wdrożenia w oknie dialogowym **Publikowanie Azure** .
1. Kliknij **przycisk OK** , aby zapisać zmiany.

Wybierz w polu tekstowym **ścieżka lokalnego serwera** ścieżka instalacji serwera na komputerze, jeśli jest spełniony dowolny z następujących warunków:

* Chcesz przetestować wdrożenie w emulatorze (dotyczy tylko programu Windows).
* Chcesz wdrożyć serwer lokalnie zainstalowanej w chmurze.
* Chcesz używać niestandardowego serwera pobierania własne w chmurze, w której przypadku, również upewnij się, że zaznaczono opcję **Rozmieszczanie Moje lokalnego serwera (automatyczne przekazywanie do do magazynu w chmurze)** powyżej.

Jeśli żadna z powyższych opcji odnosi się do danej sytuacji, ustawienie lokalnego serwera jest opcjonalne.

### <a name="applications-configuration"></a>Konfiguracji aplikacji ###

Oto przykładowy sposób można określić aplikacji.

![][ic719512]

Kliknij przycisk **Dodaj** , aby dodać innej aplikacji lub **Usuń** , aby usunąć aplikację. Do celów efektywności Jeśli chcesz używać do pobrania dla źródła aplikacji, wdrażając w chmurze, umożliwia [Właściwości składników](#components_properties) Określanie adresu URL, na konto miejsca do magazynowania, itp. 

Począwszy od wersji kwietnia 2014, aplikacje są automatycznie wysyłane do tego samego konta miejsca do magazynowania (w kontenerze **eclipsedeploy** ) jako język wybrany do wdrożenia. Logika uruchamiania wdrażania zawiera krok, który najpierw do pobrania tych aplikacji z tego konta miejsca do magazynowania. Oznacza to, że mogą uaktualnić aplikacji we wdrożeniu programu bez konieczności odbudowanie i ponownie wdróż całego pakietu, przekazując ręcznie nowsze wersje aplikacji bezpośrednio do tego konta miejsca do magazynowania (na przykład za pomocą portalu Azure), pierwotnie zastępowanie plików też przekazane przez zestaw narzędzi. Następnie po prostu Rozpocznij odtwarzanie tych wystąpień roli za pomocą portalu zarządzania Azure i ponownie lub za pomocą narzędzi wiersza polecenia. (Powodujące roli odtwarzanie bezpośrednio z poziomu narzędzi Zaćmienie nie jest obecnie obsługiwane.)

### <a name="notes-about-server-configuration"></a>Uwagi dotyczące konfiguracji serwera ###

Zmiany wprowadzone przy użyciu strony właściwości **konfiguracji serwera** są odzwierciedlane w `<component>` elementy pliku plik package.xml.

Gdy używasz **automatycznie przekazywać...** lub opcji **rozmieszczania możliwość pobierania...** JDK lub serwera aplikacji i możesz tworzenia chmury (nie emulatorze obliczeń), a są połączone z siecią, można zauważyć tworzenia wiadomości między innymi następujące rezultaty konsoli zgodnie z konstruktora który sprawdza dostępność do pobrania:

`[windowsazurepackage] Verifying blob availability (https://example.blob.core.windows.net/temp/tomcat6.zip)...` 

Jeśli wybrano opcję **Rozmieszczanie możliwość pobierania...** , mogą zostać wyświetlone następujące ostrzeżenie, ale będzie kontynuować kompilacji:

`[windowsazurepackage] warning: Failed to confirm blob availability! Make sure the URL and/or the access key is correct (https://example.blob.core.windows.net/temp/tomcat6.zip).` 

To ostrzeżenie jest jedynym wpisem dostępność do pobrania nie została zweryfikowana. Dlatego jeśli wdrożeniu kończy się niepowodzeniem w chmurze jakiegoś powodu, sprawdź Jeśli otrzymano ostrzeżenie.

Jeśli chcesz wyłączyć weryfikacji plik do pobrania (na przykład, jeśli uważasz, że go zbyt wolniej Kompilacja), należy ustawić `verifydownloads` atrybutów do `false` w `<windowsazurepackage>` elementu plik package.xml: 

`<windowsazurepackage verifydownloads="false" ...>` 

Jeśli wybrano opcję **automatycznie Przekaż...** z następnie w oknie konsoli, które będą wyświetlane konieczne jest Raportowanie postępu przekazywania 5 sekund, gdy przekazywanie wiadomości kompilacji.

<a name="ssl_offloading_properties"></a> 
### <a name="ssl-offloading-properties"></a>SSL Odciążanie właściwości ###

Otwieranie menu kontekstowego dla roli w okienku Eksplorator projektu Zaćmienie osoby, kliknij pozycję **Azure**, a następnie kliknij **Odciążanie protokołu SSL**. 

![][ic719481]

W tym oknie dialogowym możesz włączyć odciążanie protokołu SSL, co umożliwia łatwe Włącz obsługę Hypertext Transfer Protocol bezpiecznego (HTTPS) w wdrożenia Java Azure, nie jest wymagane do skonfigurowania protokołu SSL na serwerze aplikacji Java. Aby uzyskać więcej informacji zobacz [Odciążanie protokołu SSL][] i [sposobu użycia odciążanie protokołu SSL][].

## <a name="see-also"></a>Zobacz też ##

[Azure zestaw narzędzi dla programu Eclipse][]

[Instalowanie narzędzi Azure dla programu Eclipse][]

[Tworzenie aplikacji Witaj świecie dla Azure w Zaćmienie][]

[Właściwości projektu Azure][]

[Listy kont Azure miejsca do magazynowania][]

Aby uzyskać więcej informacji na temat Azure za pomocą języka Java zobacz [Centrum deweloperów języka Java Azure][].

<!-- URL List -->

[Centrum deweloperów języka Java Azure]: http://go.microsoft.com/fwlink/?LinkID=699547
[Portal Azure zarządzania]: http://go.microsoft.com/fwlink/?LinkID=512959
[Azure zestaw narzędzi dla programu Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Właściwości projektu Azure]: http://go.microsoft.com/fwlink/?LinkID=699524
[Listy kont Azure miejsca do magazynowania]: http://go.microsoft.com/fwlink/?LinkID=699528
[Pakiet com.microsoft.windowsazure.serviceruntime podsumowania]: http://azure.github.io/azure-sdk-for-java/com/microsoft/windowsazure/serviceruntime/package-summary.html
[Tworzenie aplikacji Witaj świecie dla Azure w Zaćmienie]: http://go.microsoft.com/fwlink/?LinkID=699533
[Debugowanie w przypadku wystąpienia konkretnej roli we wdrożeniu wielu wystąpień]: http://go.microsoft.com/fwlink/?LinkID=699535#debugging_specific_role_instance
[Debugowanie aplikacji Azure w Zaćmienie]: http://go.microsoft.com/fwlink/?LinkID=699535
[Wdrażanie dużych wdrożeń]: http://go.microsoft.com/fwlink/?LinkID=699536
[Jak używać znajdują się pamięci podręcznej]: http://go.microsoft.com/fwlink/?LinkID=699542
[Jak używać odciążanie protokołu SSL]: http://go.microsoft.com/fwlink/?LinkID=699545
[Instalowanie narzędzi Azure dla programu Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546
[Koligacja sesji]: http://go.microsoft.com/fwlink/?LinkID=699548
[Odciążanie protokołu SSL]: http://go.microsoft.com/fwlink/?LinkID=699549

<!-- IMG List -->

[ic789599]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic789599.png
[ic719499]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719499.png
[ic719483]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719483.png
[ic719501]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719501.png
[ic710964]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic710964.png
[ic719502]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719502.png
[ic719503]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719503.png
[ic719504]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719504.png
[ic719505]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719505.png
[ic710897]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic710897.png
[ic719506]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719506.png
[ic659268]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic659268.png
[ic552233]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic552233.png
[ic719492]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719492.png
[ic719508]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719508.png
[ic780647]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic780647.png
[ic789643]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic789643.png
[ic780648]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic780648.png
[ic789643]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic789643.png
[ic796926]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic796926.png
[ic719512]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719512.png
[ic719481]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719481.png
