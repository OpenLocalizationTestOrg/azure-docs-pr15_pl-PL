<properties 
    pageTitle="Pulpit nawigacyjny monitora, skala skonfigurować i połączenia hybrydowych w usługach BizTalk | Microsoft Azure" 
    description="Więcej informacji na temat kontrolek i monitorowanie wydajności na kartach portalu klasyczny dla usług BizTalk: pulpitu nawigacyjnego, Monitor, skali, konfigurowanie i hybrydowego połączenia. MABS, WABS" 
    services="biztalk-services" 
    documentationCenter="" 
    authors="MandiOhlinger" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="biztalk-services" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/23/2016" 
    ms.author="mandia"/>




# <a name="review-the-dashboard-monitor-scale-configure-and-hybrid-connection-tabs"></a>Przejrzyj karty pulpitu nawigacyjnego, Monitor, skali, konfigurowanie i hybrydowego połączenia

Po utworzeniu tej usługi BizTalk i wdrażanie aplikacji, możesz zmienić niektóre ustawienia usługi BizTalk i monitorowanie wydajności aplikacji. 

Po otwarciu portalu klasyczny Azure są automatycznie umieszczane na karcie **Wszystkie elementy** . Aby wyświetlić usługi BizTalk, wybierz usługi BizTalk na karcie **Wszystkie elementy** , lub wybierz kartę **BIZTALK usług** ; a następnie wybierz nazwę usługi BizTalk.

Spowoduje to otwarcie nowego okna zawierającego następujące karty. W tym temacie opisano następujące karty.

## <a name="quick-start-quick-startquickstart"></a>(Szybki Start![Szybki Start][QuickStart])
W zależności od BizTalk Services Edition wszystkie opcje na liście może nie być dostępne. 
<table border="1">
    <tr>
        <td><strong>Pobierz narzędzia</strong></td>
        <td>Pobierz zestaw SDK usług BizTalk, aby zainstalować szablony projektów programu Visual Studio na komputerze lokalnym dewelopera. Te szablony tworzenie <strong>Usług BizTalk</strong> (mostek) i wdrożonych w usłudze BizTalk projekty Visual Studio <strong>Artefaktów BizTalk</strong> (przekształcenie).
        <br/><br/>
        <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302335">Jak mogę rozpocząć przy użyciu SDK usługi Azure BizTalk</a> i <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=241589">zainstalowaniu SDK usługi Azure BizTalk</a> lista czynności, aby rozpocząć pracę.
        </td>
    </tr>
    <tr>
        <td><strong>Tworzenie partnera umów</strong></td>
        <td>Zostanie otwarta Portal usługi BizTalk Azure hostowanej Azure miejsce dodawania partnerów i tworzenie X12, AS2 i Edytuj EDIFACT umowy.
        <br/><br/>
        <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">Konfigurowanie składników Edytuj wiadomości w portalu usługi BizTalk</a> przedstawiono czynności, aby rozpocząć pracę.
        </td>
    </tr>

<tr>
        <td><strong>Dowiedz się więcej o usługach BizTalk</strong></td>
        <td>Przejdź do <a HREF="http://azure.microsoft.com/documentation/services/biztalk-services/">Centrum nauki</a> dowiedzieć się więcej na temat usług BizTalk Azure.</td>
</tr>
</table>


Na pasku zadań u dołu możesz wykonywać następujące czynności:

<table border="1">

<tr>
<td><strong>Zarządzanie</strong> wdrożenia aplikacji</td>
<td>Zostanie otwarty z portalu usługi Azure BizTalk. Portal usługi BizTalk jest wejścia do konfiguracji grupy użytkowników, w tym dodawanie partnerów i tworzenie X12, AS2 i EDIFACT umowy.
<br/><br/>
To jest taka sama, jak <strong>Tworzenie partnera umów</strong> na karcie <strong>Szybki Start</strong> .
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">Konfigurowanie składników Edytuj wiadomości w portalu usługi BizTalk</a> znajdziesz więcej informacji w portalu usługi BizTalk.</td>
</tr>

<tr>
<td><strong>Informacje o połączeniu</strong> z Namespace kontroli dostępu</td>
<td>Gdy wybierzesz informacje o połączeniu, następnie Namespace kontrola dostępu, wystawcy domyślne i domyślny klawisz są wyświetlane. Możesz skopiować te wartości.
<br/><br/>
Można również otworzyć Portal kontroli dostępu. <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285670">Tworzenie kontrola dostępu Namespace</a> znajdziesz więcej informacji w portalu kontroli dostępu.</td>
</tr>

<tr>
<td><strong>Synchronizowanie klawiszy</strong> na koncie miejsca do magazynowania</td>
<td>Podczas tworzenia konta miejsca do magazynowania, klucz podstawowy i pomocniczy klucz są tworzone automatycznie. Klucze szyfrowania kontrolowanie dostępu do konta miejsca do magazynowania. Twoja usługa BizTalk automatycznie używa klucza podstawowego. <strong>Klucze synchronizacji</strong> umożliwiają użytkownikom przełączać się między klucz podstawowy i pomocniczy klucz bez przerywania usługi BizTalk.
<br/><br/>
Na przykład chcesz używać nowego klucza podstawowego dla konta miejsca do magazynowania w usłudze BizTalk. Aby to zrobić:
<br/><br/>
<ol>
<li>Wybierz usługi BizTalk oraz <strong>Klawiszy synchronizacji</strong>. Wybierz pozycję klucza pomocniczego. Gdy to zrobisz, usługa BizTalk jest uruchamiana przy użyciu klucza pomocniczego.</li>
<li>W portalu klasyczny Azure wybierz swoje konto miejsca do magazynowania i ponownie wygenerować klucz podstawowy. Należy pamiętać, że usługi BizTalk korzysta z kluczem pomocniczym.</li>
<li>Wybierz usługi BizTalk oraz <strong>Klawiszy synchronizacji</strong>. Teraz zaznacz klucz podstawowy. Jest to nowy klucz podstawowy zostanie ponownie wygenerowane.</li>
<li>W portalu klasyczny Azure wybierz swoje konto miejsca do magazynowania i ponownie wygenerować klucza pomocniczego.</li>
</ol>
<br/>
Ten proces jest nazywany "najazd klawiszy". Celem jest w celu umożliwienia użytkownikom przełączać się między klucz podstawowy i pomocniczy klucz bez przerywania usługi BizTalk.</td>
</tr>

<tr>
<td><strong>Usuwanie</strong> aplikacji</td>
<td>Po wybraniu usuwania i usługi BizTalk oraz zostaną usunięte wszystkie elementy znajdujący się na.</td>
</tr>
</table>


## <a name="dashboard"></a>Pulpit nawigacyjny
W zależności od BizTalk Services Edition wszystkie opcje na liście może nie być dostępne. 

Gdy wybierzesz nazwę usługi BizTalk, karta Pulpit nawigacyjny jest wyświetlana. Pulpit nawigacyjny można wykonywać następujące czynności:

##### <a name="usage-overview-shows-the-number-of-used-hybrid-connections"></a>Omówienie zastosowania: Pokazuje liczbę używanych połączeń hybrydowego
Powoduje wyświetlenie także korzystanie z danych w GB. 

##### <a name="metric-graph-shows-a-fixed-list-of-performance-metrics"></a>: Metryka wykres ustalona lista wskaźniki
Te metryki podać wartości w czasie rzeczywistym dotyczących kondycji usługi BizTalk. Możesz również wybrać wartości **względne** i **bezwzględne** i przedziału czasu **interwału** miar, które są wyświetlane na wykresie. 

Aby uzyskać opis tych wskaźniki przejdź do [Metryk dostępne](#Metrics) w tym temacie.


##### <a name="quick-glance-lists-your-biztalk-service-properties"></a>Szybkie rzut: List właściwości usługi BizTalk

<table border="1">

<tr>
<td><strong>Aktualizowanie poświadczeń śledzenia bazy danych</strong></td>
<td>Zmienia nazwę użytkownika i hasło używane do logowania się do bazy danych śledzenia.</td>
</tr>
<tr>
<td><strong>Aktualizowanie certyfikat SSL</strong></td>
<td>Można aktualizować usługę BizTalk, aby zastosować inny certyfikat SSL. Certyfikat SSL z podpisem własnym jest tworzona automatycznie, gdy <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">utworzyć usługi BizTalk</a>.</td>
</tr>
<tr>
<td><strong>Pobierz certyfikat</strong></td>
<td>Możesz pobrać certyfikat SSL używany przez usługę BizTalk na komputer lokalny.</td>
</tr>
<tr>
<td><strong>Stan</strong></td>
<td>Jest wyświetlany bieżący stan usługi BizTalk. Zobacz <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=329870">usług BizTalk: wykres stanu usługi</a>. </td>
</tr>
<tr>
<td><strong>Adres URL usługi</strong></td>
<td>Adres URL usługi BizTalk. Działa to tak samo jak adres <strong>URL domeny</strong> wprowadzane podczas tworzenia usługi BizTalk.</td>
</tr>
<tr>
<td><strong>Adres wirtualny publiczny adres IP (VIP)</strong></td>
<td>Adres IP przypisany do usługi BizTalk. Jest używany dla wszystkich punktów końcowych wprowadzania i adres źródła dla ruchu wychodzącego. Ten adres IP należy do usługi BizTalk, ile po jego utworzeniu. Jeśli usuniesz usługę BizTalk adres IP jest przypisany do innej usługi BizTalk.</td>
</tr>
<tr>
<td><strong>Namespace ACS</strong></td>
<td>Uwierzytelnianie za pomocą usługi BizTalk.</td>
</tr>
<tr>
<td><strong>Edition</strong></td>
<td>List wersji wprowadzone po utworzeniu usługę BizTalk.</td>
</tr>
<tr>
<td><strong>Lokalizacja</strong></td>
<td>Wyświetla regionu geograficznego, obsługującego usługi BizTalk.</td>
</tr>
<tr>
<td><strong>Utworzone</strong></td>
<td>Wyświetla datę i godzinę, które usługa BizTalk została utworzona.</td>
</tr>
<tr>
<td><strong>Śledzenie bazy danych</strong></td>
<td>Nazwa bazy danych SQL Azure, która przechowuje tabele śledzenia używane przez usługę BizTalk. 
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">Wymagania dotyczące</a> podano szczegółowe informacje dotyczące śledzenia bazy danych.</td>
</tr>
<tr>
<td><strong>Magazyn monitorowania i archiwizacji</strong></td>
<td>Nazwa konta magazynu platformy Azure przechowuje monitorowania dane wyjściowe usługi BizTalk.
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">Wymagania dotyczące</a> zawiera szczegółowe informacje na rachunku miejsca do magazynowania.</td>
</tr>
<tr>
<td><strong>Nazwy subskrypcji.</strong></td>
<td>Lista subskrypcji, która obsługuje usługi BizTalk. Subskrypcja decyduje dostępu do portalu klasyczny Azure.</td>
</tr>
<tr>
<td><strong>Identyfikator subskrypcji</strong></td>
<td>Identyfikator subskrypcji jest generowany automatycznie podczas tworzenia subskrypcji. Korzystając z pozostałych interfejsy API, może być konieczne podać nazwę subskrypcji.</td>
</tr>
</table>

[Usług BizTalk: obsługi administracyjnej portal klasyczny Azure za pomocą](http://go.microsoft.com/fwlink/p/?LinkID=302280) przedstawiono kroki, aby utworzyć usługę BizTalk.


##### <a name="manage-connection-information-sync-keys-and-delete-in-the-task-bar"></a>Zarządzanie, informacje o połączeniu, klawiszy synchronizacji i Usuń na pasku zadań:

<table border="1">

<tr>
<td><strong>Zarządzanie</strong> wdrożenia aplikacji</td>
<td>Zostanie otwarty z portalu usługi Azure BizTalk. Portal usługi BizTalk jest wejścia do konfiguracji grupy użytkowników, w tym dodawanie partnerów i tworzenie X12, AS2 i EDIFACT umowy.
<br/><br/>
To jest taka sama, jak <strong>Tworzenie partnera umów</strong> na karcie <strong>Szybki Start</strong> .
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">Konfigurowanie składników Edytuj wiadomości w portalu usługi BizTalk</a> znajdziesz więcej informacji w portalu usługi BizTalk.</td>
</tr>
<tr>
<td><strong>Informacje o połączeniu</strong> z Namespace kontroli dostępu</td>
<td>Wyświetla Namespace kontrola dostępu, wystawcy domyślne i wartości domyślne klucza; której można je skopiować.
<br/><br/>
Można również otworzyć Portal kontroli dostępu. Ten Portal kontrola dostępu jest taka sama, jak za pomocą opcji usługi Active Directory w okienku nawigacji po lewej stronie.
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285670">Zarządzanie i Namespace ACS</a> znajdziesz więcej informacji w portalu kontroli dostępu.</td>
</tr>
<tr>
<td><strong>Synchronizowanie klawiszy</strong> na koncie miejsca do magazynowania</td>
<td>Podczas tworzenia konta miejsca do magazynowania, klucz podstawowy i pomocniczy klucz są tworzone automatycznie. Klucze szyfrowania kontrolowanie dostępu do konta miejsca do magazynowania. Twoja usługa BizTalk automatycznie używa klucza podstawowego. <strong>Klucze synchronizacji</strong> umożliwiają użytkownikom przełączać się między klucz podstawowy i pomocniczy klucz bez przerywania usługi BizTalk.
<br/><br/>
Na przykład chcesz używać nowego klucza podstawowego dla konta miejsca do magazynowania w usłudze BizTalk. Aby to zrobić:
<br/><br/>
<ol>
<li>Wybierz usługi BizTalk oraz <strong>Klawiszy synchronizacji</strong>. Wybierz pozycję klucza pomocniczego. Gdy to zrobisz, usługa BizTalk jest uruchamiana przy użyciu klucza pomocniczego.</li>
<li>W portalu klasyczny Azure wybierz swoje konto miejsca do magazynowania i ponownie wygenerować klucz podstawowy. Należy pamiętać, że usługi BizTalk korzysta z kluczem pomocniczym.</li>
<li>Wybierz usługi BizTalk oraz <strong>Klawiszy synchronizacji</strong>. Teraz zaznacz klucz podstawowy. Jest to nowy klucz podstawowy zostanie ponownie wygenerowane.</li>
<li>W portalu klasyczny Azure wybierz swoje konto miejsca do magazynowania i ponownie wygenerować klucza pomocniczego.</li>
</ol>
<br/>
Ten proces jest nazywany "najazd klawiszy". Celem jest w celu umożliwienia użytkownikom przełączać się między klucz podstawowy i pomocniczy klucz bez przerywania usługi BizTalk.</td>
</tr>

<tr>
<td><strong>Usuwanie</strong> aplikacji</td>
<td>Twoja usługa BizTalk i wszystkie elementy znajdujący się na zostaną usunięte.</td>
</tr>
</table>


## <a name="monitor"></a>Monitorowanie
Nie dotyczy bezpłatna wersja.

Gdy wybierzesz nazwę usługi BizTalk, na karcie Monitor jest dostępna i wyświetli następujące informacje:

##### <a name="metric-graph-displays-the-selected-performance-metrics"></a>Metryczne wykresu: Wybrane wskaźniki są wyświetlane
Te metryki podać wartości w czasie rzeczywistym dotyczących kondycji usługi BizTalk. Możesz wybrać, które wskaźniki są wyświetlane. Maksymalnie sześć wskaźniki mogą być wyświetlane jednocześnie. 

Możesz również wybrać wartości **względne** i **bezwzględne** i przedziału czasu **interwału** miar, które są wyświetlane. 

##### <a name="to-remove-or-display-metrics-in-the-graph"></a>Aby usunąć lub wyświetlić metryki na wykresie:
1. Wybierz kartę **Monitor** .
2. Na pasku zadań, wybierz pozycję **Dodaj metryk** :  
![Wybierz pozycję Dodaj metryk][AddMetrics]
3. Sprawdź wskaźniki, które mają być wyświetlane.
4. Zaznacz znacznik wyboru, aby wrócić do karty **Monitor** .
5. Wybierz pozycję koło obok metryki, aby wyświetlić wartość tego metryki na wykresie.  

    Na przykład metryka **Użycie Procesora** jest wyszarzona; dane wyjściowe nie są wyświetlane na wykresie:  
![Metryka użycie Procesora jest wyszarzona.][GrayedMetric]  

    Wybierz wygaszonych koła, aby włączyć metryki **Użycie Procesora** wyświetlić jej wyniki na wykresie:  
![Metryka użycie Procesora jest włączona][EnabledMetric]

6. Aby usunąć metryki z wykresu wyświetlania i listy, wybierz pozycję **Usuwanie metryki** na pasku zadań. Aby dodać metryczne wstecz do listy, wybierz pozycję **Dodaj metryki** na pasku zadań, sprawdź Metryka i zaznacz znacznik wyboru, aby wrócić do karty **Monitor** . Zaznacz wygaszonych koła, aby włączyć metrykę.

## <a name="Metrics"></a>Dostępne wskaźniki
Dostępne są następujące liczniki wydajności i wskaźniki:

<table border="1">

<tr>
<td><strong>Opóźnienie RountdTrip</strong></td>
<td>Wyświetla wszystkie mosty Średni czas procesu wiadomości od czasu, aż wiadomość w pełni jest przetwarzana przez usługę BizTalk odbioru wiadomości (w milisekundach) (ms). Zliczane są tylko wiadomości przetwarzania.<br/><br/>
Gdy występują następujące zdarzenia, tworzona jest sygnatura czasowa:
<ul>
<li>Wiadomość wprowadza bramy</li>
<li>Wiadomości są kierowane do miejsca docelowego</li>
<li>Miejsce docelowe odpowiedzi</li>
<li>Wysłane do bramy odpowiedzi potwierdzenia miejsca docelowego</li>
</ul>
<br/>
Ta metryka wyświetla wynik następującego obliczenia:
<br/><br/>
[Wysłane do bramy odpowiedzi potwierdzenia docelowego] - [wiadomości wprowadza bramy]</td>
</tr>
<tr>
<td><strong>Błędy w źródle</strong></td>
<td>Wyświetla całkowitą liczbę wiadomości, które nie zostały przez usługę BizTalk podczas pobierania danych wiadomości od punkty końcowe źródła.</td>
</tr>
<tr>
<td><strong>Użycie Procesora</strong></td>
<td>Lista średnio % czasu procesora wszystkich wystąpień roli.</td>
</tr>
<tr>
<td><strong>Opóźnienie przetwarzania</strong></td>
<td>Wyświetla średni czas (w milisekundach) (ms) przetwarzania wiadomości przez usługę BizTalk wszystkich mosty, z wyłączeniem czasu poświęconego na miejsc docelowych. Zliczane są tylko wiadomości pomyślnie przetworzona.<br/><br/>
Każda z poniższych zdarzeń wystąpieniu sygnatura czasowa jest tworzona:

<ul>
<li>Wiadomość wprowadza bramy</li>
<li>Wiadomości są kierowane do miejsca docelowego</li>
<li>Miejsce docelowe odpowiedzi</li>
<li>Miejsce docelowe potwierdzenia odpowiedzi wysyłane do bramy</li>
</ul>
<br/>Ta metryka wyświetla wynik następującego obliczenia:<br/><br/>
[Wysłane do bramy odpowiedzi potwierdzenia docelowego] - [wiadomości wprowadza bramy] - [miejsce docelowe odpowiedzi] + [wiadomości jest kierowane do miejsca docelowego]</td>
</tr>
<tr>
<td><strong>Błędy w procesie</strong></td>
<td>Wyświetla całkowitą liczbę wiadomości, które nie powiodła się podczas przetwarzania przez usługę BizTalk przez wszystkich mosty w przedziale czasu.</td>
</tr>
<tr>
<td><strong>Wiadomości wysłanych</strong></td>
<td>Wyświetla całkowitą liczbę wiadomości przesyłane przez usługę BizTalk wszystkich mosty w przedziale czasu. Ta metryka jest zwiększany, gdy wiadomość wysłana z potok osiągnie miejsce docelowe rozsyłania. Tej metryki nie oznacza, że wiadomość jest przetwarzana pomyślnie.<br/><br/>
W scenariuszu żądanie-odpowiedź metryka jest zwiększany, gdy miejsce docelowe rozsyłania odsyła potwierdzenia potwierdzenia proces.</td>
</tr>
<tr>
<td><strong>Odebrane wiadomości</strong></td>
<td>Wyświetla całkowitą liczbę wiadomości odebranych przez usługę BizTalk przez wszystkich mosty w przedziale czasu. Ta metryka jest zwiększany po otrzymaniu nowej wiadomości przez proces.</td>
</tr>
<tr>
<td><strong>Wiadomości w procesie</strong></td>
<td>Wyświetla całkowitą liczbę wiadomości obecnie przetwarzanych przez usługę BizTalk w przedziale czasu.</td>
</tr>
<tr>
<td><strong>Wiadomości</strong></td>
<td>Wyświetla całkowitą liczbę wiadomości pomyślnie przetwarzane przez usługę BizTalk we wszystkich mosty w przedziale czasu. Ta metryka jest zwiększany, gdy wiadomość jest pomyślnie odebrane przez potok i pomyślnie kierowane do miejsca docelowego.</td>
</tr>
</table>


## <a name="scale"></a>Skala
Na karcie Skala możesz dodać lub odjąć liczby jednostek używane przez usługę BizTalk. Domyślnie jest język, który skonfigurowano jednostki. Dodatkowe jednostki można dodawać do skalowania usługi BizTalk. W przypadku powiększenia skali, są zwiększenie produktywności. Ilość zasobów zwiększa także, tym wdrożonym mosty, umów, LOB połączeń i przetwarzania power. Na przykład można zwiększyć skalowanie podmiotu 1 do 2 jednostki. W tej sytuacji można wdrożyć podwójny liczbę mosty, dwukrotnie umów, dwukrotnie połączeń LOB i dwukrotnie power przetwarzania.

Niektóre wersje BizTalk nie oferuje opcję Skala. W tej sytuacji jedną jednostkę jest dozwolone. Aby określić liczbę jednostek, wydanie można skalować, zapoznaj się z [usług BizTalk: wersje wykresu](biztalk-editions-feature-chart.md).

Zwiększenie liczby jednostek mogą mieć wpływ na ceny. Jeśli zwiększysz jednostki, wybierając **Zapisywanie** wyświetli komunikat informujący o tym, jeśli dotyczy to rozliczenia. Następnie możesz wybrać kontynuować. Gdy zwiększenie liczby jednostek, zmian stanu usługi BizTalk z Aktywna aktualizacja. W trakcie aktualizacja usługi BizTalk będzie działać.

[Usług BizTalk: wykres wersje](biztalk-editions-feature-chart.md) definiuje "Jednostka".


## <a name="configure"></a>Konfigurowanie
Nie dotyczy hybrydowych połączenia.

Ustawia stan kopii zapasowej Brak lub automatyczne. Gdy jest ustawiony na wartość Brak, nie wykonywania kopii zapasowych są tworzone automatycznie. Wybranie ustawienia automatycznie, można skonfigurować lokalizację kopii zapasowej, częstotliwości wykonywania kopii zapasowej oraz jak długo przechowywania plików kopii zapasowej. 

[Usług BizTalk: wykonywanie kopii zapasowych i przywracanie](biztalk-backup-restore.md) zawiera informacje. 


## <a name="HybridConnections"></a>Połączenia hybrydowego
Hybrydowe połączenia aplikacji Azure, na przykład telefon komórkowy aplikacji w usłudze Azure aplikacji lub aplikacji sieci Web do zasobu w lokalnej używającej statyczne port TCP, takich jak program SQL Server, MySQL HTTP sieci Web API i większość niestandardowych usług sieci Web. Połączenia hybrydowych są zarządzane w usługach BizTalk w portalu klasyczny Azure.

Aby utworzyć połączenia hybrydowych w usłudze Azure w aplikacji, zobacz [Dostęp lokalnych zasobów za pomocą połączenia hybrydowych w usłudze Azure aplikacji](../app-service-web/web-sites-hybrid-connection-get-started.md).

Aby utworzyć lub Zarządzanie połączeniami hybrydowych w usługach BizTalk Azure, zobacz [Hybrydowych połączenia](integration-hybrid-connection-overview.md).



## <a name="next"></a>Następny
Teraz użytkownicy zaznajomieni z różnych kart, możesz dowiedzieć się więcej na temat funkcji usługi Azure BizTalk:

- [Usługi BizTalk: ograniczanie](biztalk-throttling-thresholds.md)  
- [Usługi BizTalk: Nazwa wystawcy i klucza wystawcy](biztalk-issuer-name-issuer-key.md)  
- [BizTalk usługi: Kopia zapasowa i przywracanie](biztalk-backup-restore.md)

## <a name="see-also"></a>Zobacz też
- [Połączenia hybrydowego](integration-hybrid-connection-overview.md)  
- [Usługi BizTalk: Deweloper, podstawowej, standardowe i wykres wersji Premium](biztalk-editions-feature-chart.md)  
- [Usługi BizTalk: Klasyczny portal Azure za pomocą inicjowania obsługi administracyjnej](biztalk-provision-services.md)  
- [BizTalk usługi: Wykres stanu usługi BizTalk](biztalk-service-state-chart.md)  
- [Jak mogę rozpocząć przy użyciu SDK usług BizTalk Azure](http://go.microsoft.com/fwlink/p/?LinkID=302335)

[QuickStart]: ./media/biztalk-dashboard-monitor-scale-tabs/QuickStartIcon.png
[AddMetrics]: ./media/biztalk-dashboard-monitor-scale-tabs/WABS_AddMetrics.png
[GrayedMetric]: ./media/biztalk-dashboard-monitor-scale-tabs/WABS_GrayedMetric.png
[EnabledMetric]: ./media/biztalk-dashboard-monitor-scale-tabs/WABS_EnabledMetric.png
 
