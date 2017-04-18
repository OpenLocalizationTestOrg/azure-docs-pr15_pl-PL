<properties
    pageTitle="Informacje o wersji usługi Azure BizTalk | Usługi BizTalk Microsoft Azure"
    description="Znane problemy dotyczące usługi Azure BizTalk list" 
    services="biztalk-services"
    documentationCenter=""
    authors="msftman"
    manager="erikre"
    editor=""/>

<tags
    ms.service="biztalk-services"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="deonhe"/>

# <a name="release-notes-for-azure-biztalk-services"></a>Informacje o wersji usługi Azure BizTalk

Informacje o wersji usługi Microsoft Azure BizTalk zawierają znane problemy w tej wersji.

## <a name="whats-new-in-the-november-update-of-biztalk-services"></a>Co nowego w aktualizacji z listopada BizTalk usług
* Szyfrowanie w pozostałych mogą być włączone w portalu usługi BizTalk. Zobacz [Włączanie szyfrowania spoczynku w portalu usługi BizTalk](https://msdn.microsoft.com/library/azure/dn874052.aspx).

## <a name="update-history"></a>Historia aktualizacji

### <a name="october-update"></a>Aktualizacja października

* Konta organizacji są obsługiwane:  
 * **Scenariusz**: zarejestrowana wdrażanie usługi BizTalk za pomocą konta Microsoft (takie jak user@live.com). W tym scenariuszu tylko użytkownicy Account Microsoft można zarządzać usługą BizTalk za pomocą portalu usługi BizTalk. Nie można używać konta organizacji.  
 * **Scenariusz**: zarejestrowana wdrażanie usługi BizTalk za pomocą konta organizacji w usłudze Azure Active Directory (takich jak user@fabrikam.com lub user@contoso.com). W tym scenariuszu tylko użytkownicy usługi Azure Active Directory w obrębie tej samej organizacji można zarządzać usługą BizTalk za pomocą portalu usługi BizTalk. Nie można używać konta Microsoft.  
* Po utworzeniu usługi BizTalk w portalu klasyczny Azure automatycznie są rejestrowane w portalu usługi BizTalk.
 * **Scenariusz**: Zaloguj się do portalu klasyczny Azure tworzenia usługi BizTalk, a następnie wybierz **Zarządzanie** po raz pierwszy. Gdy zostanie otwarta z portalu usługi BizTalk, usługę BizTalk automatycznie rejestruje i jest gotowy do wdrożeń.  
 Zobacz [Rejestrowanie i aktualizowanie wdrożeniu BizTalk usługi w portalu usługi BizTalk](https://msdn.microsoft.com/library/azure/hh689837.aspx).  

### <a name="august-14-update"></a>Aktualizacja sierpnia 14
* Umowa i Mostek rozdzielenie — partnera umów i mosty są teraz odłączona w portalu usługi BizTalk. Możesz teraz utworzyć umów i mosty oddzielnie, a w czasie wykonywania mosty rozpoznawać umowy na podstawie wartości w komunikacie grupy użytkowników. Zobacz [Tworzenie umów w usługach BizTalk Azure](https://msdn.microsoft.com/library/azure/hh689908.aspx), [utworzyć mostek grupy użytkowników za pomocą portalu usługi BizTalk](https://msdn.microsoft.com/library/azure/dn793986.aspx), [Utwórz mostka AS2 za pomocą portalu usługi BizTalk](https://msdn.microsoft.com/library/azure/dn793993.aspx)i [jak mosty usunąć umów w czasie wykonywania?](https://msdn.microsoft.com/library/azure/dn794001.aspx)  
* Opcja tworzenia szablonów dla umów jest przerywane.  
* Umowy Wyślij strony możesz teraz podać ogranicznika różnych zestawów dla każdego schematu. Ta konfiguracja jest określona w obszarze ustawienia protokołów Wyślij strony umowy. Aby uzyskać więcej informacji, zobacz [Tworzenie i X12 umowy w usługach BizTalk Azure](https://msdn.microsoft.com/library/azure/hh689847.aspx) i [Tworzenie umowę EDIFACT w usługach BizTalk Azure](https://msdn.microsoft.com/library/azure/dn606267.aspx). Dwie nowe jednostki są także dodawane API modelu OBIEKTOWEGO TPM w tym samym celu. Zobacz [X12DelimiterOverrides](https://msdn.microsoft.com/library/azure/dn798749.aspx) i [EDIFACTDelimiterOverride](https://msdn.microsoft.com/library/azure/dn798748.aspx).  
* Standardowy konstrukcji XSD, w tym typy pochodny, są teraz obsługiwane. Zobacz [Używanie XSD standardowy tworzy w map](https://msdn.microsoft.com/library/azure/dn793987.aspx) i [Typy uzyskana użycia w scenariuszach mapowanie i przykłady](https://msdn.microsoft.com/library/azure/dn793997.aspx).  
* AS2 obsługuje nowych algorytmów Mikrofon do podpisywania wiadomości i nowych algorytmów szyfrowania. Zobacz [Tworzenie umowę AS2 w usługach Azure BizTalk](https://msdn.microsoft.com/library/azure/hh689890.aspx).  
## <a name="know-issues"></a>Znasz problemów

### <a name="connectivity-issues-after-biztalk-services-portal-update"></a>Problemy z łącznością po BizTalk usług aktualizacja portalu

  Jeśli Portal usługi BizTalk otwarcie, gdy usług BizTalk jest uaktualniany do użycia w zmieni się z usługą, może być buźkę problemy z łącznością z portalem usługi BizTalk.  
  Obejść ten problem może być Uruchom ponownie przeglądarkę, usuwanie pamięci podręcznej przeglądarki i uruchomić portalu w trybie prywatne.  

### <a name="visual-studio-ide-cannot-locate-the-artifact-if-you-click-an-error-or-warning-in-a-biztalk-services-project"></a>Visual Studio IDE nie można odnaleźć artefaktu, kliknięcie przycisku o błędzie lub ostrzeżenie w projekcie usług BizTalk
Instalowanie programu Visual Studio 2012 aktualizacji 3 RC 1 Aby rozwiązać ten problem.  

### <a name="custom-binding-project-reference"></a>Wiązanie niestandardowe odwołania projektu
Należy rozważyć, czy z projektu usług BizTalk rozwiązania programu Visual Studio następujących sytuacjach:  
* W tym samym rozwiązaniu Visual Studio istnieje projektu usług BizTalk i projektu niestandardowego powiązania. Projekt usługi BizTalk zawiera odwołanie do tego pliku projektu niestandardowego powiązania.
* Projekt usługi BizTalk ma odwołanie do niestandardowego powiązania zachowania DLL.

"Tworzenia" rozwiązania w programie Visual Studio pomyślnie. Następnie "Odbudowanie" lub oczyścić rozwiązanie. Po wykonaniu tej odbudowanie lub oczyścić ponownie następujący błąd:  
  Nie można skopiować pliku <Path to DLL> do "bin\Debug\FileName.dll". Proces nie może uzyskać dostępu do pliku "bin\Debug\FileName.dll", ponieważ jest używany przez inny proces.  

#### <a name="workaround"></a>Obejście
* Jeśli jest zainstalowany [program Visual Studio 2012 aktualizacji 3](https://www.microsoft.com/download/details.aspx?id=39305) , dostępne są następujące dwie opcje:

  * Uruchom ponownie program Visual Studio, lub

  * Uruchom ponownie rozwiązanie. Następnie należy wykonać tylko Konstruuj na rozwiązanie.  

* Jeśli nie zainstalowano [programu Visual Studio 2012 aktualizacji 3](https://www.microsoft.com/download/details.aspx?id=39305) , Otwieranie Menedżera zadań, kliknij kartę procesy, kliknij ten proces MSBuild.exe, a następnie kliknij przycisk Zakończ proces.  

### <a name="routing-to-basichttprelay-endpoints-is-not-supported-from-bridges-and-biztalk-services-portal-if-non-printable-characters-are-promoted-as-http-headers"></a>Rozsyłanie do punktów końcowych BasicHttpRelay nie jest obsługiwana w mosty i portalu usługi BizTalk Jeśli znaki niedrukowalne są wyróżnione jako nagłówki HTTP

Jeśli używasz znaki niedrukowalne jako część promowane właściwości dla wiadomości, tych wiadomości nie kierowane do miejsc docelowych przekazywania, korzystające z powiązanie BasicHttpRelay. Ponadto promowane właściwości którego są dostępne jako część śledzenia są zakodowany adres URL dla obiektów blob i usuwając zakodowany do miejsc.  

### <a name="mdn-is-sent-asynchronously-even-if-the-send-asynchronous-mdn-option-is-unchecked"></a>MDN jest wysyłana asynchroniczne, nawet jeśli nie jest zaznaczona opcja Wyślij asynchroniczne MDN  
Należy rozważyć, czy w tym scenariuszu — zaznacz pole wyboru **Wyślij asynchroniczne MDN** , określ adres URL Wysyłanie asynchroniczne MDN, a następnie usuń zaznaczenie pola wyboru **Wyślij asynchroniczne MDN** ponownie MDN nadal są wysyłane do określonego adresu URL, nawet jeśli nie wybrano opcję, aby wysłać MDNs asynchroniczne.  
Obejść ten problem należy wyczyść określony adres URL przed usunięcie zaznaczenia pola wyboru **Wyślij asynchroniczne MDN** , a następnie wdrożyć umowę AS2.  

### <a name="whitespace-characters-beyond-a-valid-interchange-cause-an-empty-message-to-be-sent-to-the-suspend-endpoint"></a>Znaki odstępu powyżej prawidłowe wymiany powodować pusta wiadomość do wysłania do punktu końcowego wstrzymania  
W przypadku spacje poza części IEA dezasembler pakietów traktuje to jako koniec bieżącej wymiany i analizuje następnego zestawu spacje jako następnej wiadomości. Ponieważ nie jest prawidłową wymiany, może obserwować, że jeden pomyślnego wiadomość jest wysyłana do miejsca docelowego rozsyłania i jedną pustą wiadomość jest wysyłana punkt końcowy wstrzymania.  
### <a name="tracking-in-biztalk-services-portal"></a>Śledzenie w portalu usługi BizTalk  
Śledzenie zdarzeń są przechwytywane do przetwarzania wiadomości grupy użytkowników i wszelkie korelacji. Jeśli wiadomość nie powiedzie się poza scenie Protocol (protokół), śledzenie będzie wyświetlana jako pomyślnie. W tej sytuacji zapoznaj się z sekcją dziennika w obszarze kolumny **szczegółów** w **Śledzenie** , aby uzyskać szczegółowe informacje o błędzie.
X12 ustawień wysyłania i odbierania ([Tworzenie i X12 umowy w usługach BizTalk Azure](https://msdn.microsoft.com/library/azure/hh689847.aspx)) zawierają informacje o etapie Protocol (protokół).  

### <a name="update-agreement"></a>Aktualizowanie umowy  
Portal usługi BizTalk umożliwia modyfikowanie kwalifikator tożsamości po skonfigurowaniu umowy. Może to powodować właściwości jest niezgodna. Na przykład istnieje umowa przy użyciu ZZ:1234567 i ZZ:7654321 kwalifikator. W portalu usługi BizTalk ustawienia profilu możesz zmienić ZZ:1234567 się 01:ChangedValue. Otwórz umowę i 01:ChangedValue jest wyświetlany zamiast ZZ:1234567.
Aby zmodyfikować kwalifikator tożsamości, usunąć umowy, zaktualizuj **tożsamości** w profilu partnera, a następnie ponownie utworzyć umowę.  
> AZURE. Ostrzeżenie dotyczące to zachowanie wpływa na X12 i AS2.  

### <a name="as2-attachments"></a>Załączniki AS2  
Załączniki wiadomości nie są obsługiwane w AS2 wysyłać ani odbierać. W szczególności załączników bezgłośną są ignorowane i treść wiadomości jest przetwarzana jako zwykłego komunikat AS2.  
### <a name="resources-remembering-path"></a>Zasoby: Dokonywaniu ścieżki  
Podczas dodawania **zasobów**, okno dialogowe nie może pamiętać ścieżkę wcześniej użyty w celu dodania zasobu. Do zapamiętania wcześniej używana ścieżka, spróbuj dodać w witrynie sieci web BizTalk Portal usługi do **Zaufanych witryn** w programie Internet Explorer.  
### <a name="if-you-rename-the-entity-name-of-a-bridge-and-close-the-project-without-saving-changes-opening-the-entity-again-results-in-an-error"></a>Zmień nazwę jednostki mostka, zamknij projekt bez zapisywania zmian ponownie otworzyć jednostki powoduje błąd
Należy rozważyć scenariusz w następującej kolejności:  
* Dodawanie mostka (na przykład XML One-Way mostek) do projektu usługi BizTalk  

* Zmień nazwę mostka przez podanie wartości dla właściwości nazwy obiektu. Umożliwia zmianę nazwy pliku skojarzone .bridgeconfig o określonej nazwie.  

* Zamknij plik .bcs (zamykając karty w programie Visual Studio) bez zapisywania zmian.  

* Ponownie otwórz plik .bcs w Eksploratorze rozwiązań.  
Można zauważyć .bridgeconfig skojarzony plik ma nazwę nowej określonej, nazwę obiektu na powierzchni projektowej się nadal starymi nazwami. Jeśli spróbujesz otworzyć konfigurację mostka, klikając dwukrotnie składnik mostka, zostanie wyświetlony następujący komunikat o błędzie:  
  "<old name>"Jednostki jest skojarzony plik"<old name>.bridgeconfig" nie istnieje  
Aby uniknąć uruchomiony w tym scenariuszu, upewnij się, że zapisać zmiany, po zmianie nazwy obiektów w projekcie usługi BizTalk.  
### <a name="biztalk-service-project-builds-successfully-even-if-an-artifact-has-been-excluded-from-a-visual-studio-project"></a>Usługa BizTalk projektu pomyślnie tworzy nawet wtedy, gdy struktura został wyłączony z projektu programu Visual Studio
Należy rozważyć, czy scenariusza, w której jest dodawany Struktura (na przykład plik XSD) do projektu usługi BizTalk, obejmować tej artefaktu konfigurację mostka (na przykład, podając je jako typ wiadomości żądania) i wyklucz go z projektu programu Visual Studio. W takim przypadku tworzenia projektu nie udziela jakikolwiek błąd jak usunięte artefaktu jest dostępna na dysku w tej samej lokalizacji, z której została uwzględniona w projekcie programu Visual Studio.
### <a name="the-biztalk-service-project-does-not-check-for-schema-availability-while-configuring-the-bridges"></a>Projekt usługi BizTalk nie sprawdza dostępności schematu podczas konfigurowania mosty
W projekcie usługi BizTalk Jeśli schematu, który jest dodawany do projektu zostały zaimportowane przez inny schemat projektu usługi BizTalk nie sprawdza, czy zaimportowanego schematu jest dodawany do projektu. Jeśli spróbujesz utworzyć takie projektu, nie są błędy kompilacji.
### <a name="the-response-message-for-a-xml-request-reply-bridge-is-always-of-charset-utf-8"></a>Wiadomość odpowiedzi mostka żądanie-odpowiedź XML jest zawsze znaków UTF-8
W tej wersji znaków komunikat odpowiedzi z mostka żądanie-odpowiedź XML jest zawsze równa UTF-8.
### <a name="user-defined-datatypes"></a>Typy danych zdefiniowane przez użytkownika
Karty BizTalk karty dodatkiem Service Pack w funkcji Usługa karty BizTalk mogą korzystać zdefiniowane przez użytkownika typy danych dla operacji karty.
Gdy używasz zdefiniowane przez użytkownika typy danych, skopiuj pliki (dynamicznie dll) dysk: \Program Files\Microsoft Service\BAServiceRuntime\bin\ Karta BizTalk lub do pamięci podręcznej (globalnej) na serwerze obsługującym BizTalk karta usługi. W przeciwnym razie następujący komunikat o błędzie może wystąpić w kliencie:  
```<s:Fault xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">
  <faultcode>s:Client</faultcode>
  <faultstring xml:lang="en-US">The UDT with FullName "File, FileUDT, Version=Value, Culture=Value, PublicKeyToken=Value" could not be loaded. Try placing the assembly containing the UDT definition in the Global Assembly Cache.</faultstring>
  <detail>
    <AFConnectRuntimeFault xmlns="http://Microsoft.ApplicationServer.Integration.AFConnect/2011" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
      <ExceptionCode>ERROR_IN_SENDING_MESSAGE</ExceptionCode>
    </AFConnectRuntimeFault>
  </detail>
</s:Fault> ```  
> [AZURE.IMPORTANT] Zaleca się za pomocą programu GACUtil.exe do zainstalowania pliku do globalnej pamięci podręcznej zestawów. GACUtil.exe opisano sposób użycia tego narzędzia i opcje wiersza polecenia programu Visual Studio.  

### <a name="restarting-the-biztalk-adapter-service-web-site"></a>Ponowne uruchamianie w witrynie sieci Web usługi Karta BizTalk
Instalowanie **Środowisko uruchomieniowe usługi karty BizTalk** * tworzy * *Usługa karty BizTalk* * witryn sieci web w programie IIS, która zawiera * *BAService* * aplikacji.* *BAService** wewnętrznie aplikację powiązanie przekazywania zwiększyć zasięg punktu końcowego usługi w wersji lokalnej w chmurze. Usługa obsługiwana lokalnego odpowiednich końcowy przekazywania zostanie zarejestrowany na Bus usługi tylko wtedy, gdy uruchomienia usługi lokalnego.  

Zatrzymywanie, uruchom aplikację konfiguracji automatyczne uruchamianie aplikacji nie jest uznane. Aby po zatrzymaniu **BAService** należy zawsze ponownie w witrynie sieci web **Usługi karty BizTalk** zamiast tego. Nie uruchamiać i zatrzymywać aplikacji **BAService** .
### <a name="special-characters-should-not-be-used-for-address-and-entity-names-of-lob-components"></a>Nie należy używać znaków specjalnych nazw adresu i jednostki LOB składników
Nie należy używać znaków specjalnych, adres i jednostki nazw składników LOB. Jeśli zrobisz, zostanie wyświetlony komunikat o błędzie podczas wdrażania projektu usługi BizTalk. Dla niektórych znaki, takie jak "%", Usługa karty BizTalk witryny sieci Web może być Przejdź w stanie zatrzymania i będzie trzeba ręcznie uruchomić.
### <a name="test-map-with-get-context-property"></a>Testowanie mapy z właściwości kontekstu Get
Jeśli przekształcenie zawiera operacji mapy **Uzyskiwanie właściwości kontekstu** , **Mapy Test** zakończy się niepowodzeniem. Tymczasowe obejść ten problem Zamień operację mapy **Uzyskiwanie właściwości kontekstu** ciągu ZŁĄCZ.teksty mapy operacji fikcyjna danymi. To wypełnianie schematu docelowej i Zezwól programowi przetestować inne funkcje przekształcenie.
### <a name="test-map-property-does-not-display"></a>Właściwość mapy test nie są wyświetlane.
Właściwości **Mapy Test** nie są wyświetlane w programie Visual Studio. To może wystąpić, jeśli okno **Właściwości** i okna **Eksplorator rozwiązań** są nie jednocześnie zadokowane. Aby rozwiązać ten problem, Zadokuj **Właściwości** i **Rozwiązanie Eksploratora** windows.  
### <a name="datetime-reformat-drop-down-is-grayed-out"></a>Data/Godzina sformatować listy rozwijanej jest wyszarzona.
Podczas operacji mapy sformatować daty i godziny jest dodawany na powierzchnię projektu i skonfigurowana, na liście rozwijanej Format mogą być wyszarzone. To może się zdarzyć, jeśli komputer wyświetlania jest ustawiony **Średni — 125%** lub **większe — 150%**. Aby rozwiązać problem, należy ustawić wyświetlanie **mniejsze — 100% (ustawienie domyślne)** , wykonując poniższe czynności:  
1. Otwórz **Panel sterowania** i kliknij ikonę **Wygląd i personalizacja**.
2. Kliknij pozycję **Wyświetl**.
3. Kliknij **mniejsze — 100% (ustawienie domyślne)** , a następnie kliknij przycisk **Zastosuj**.

Na liście rozwijanej **Format** teraz powinna działać zgodnie z oczekiwaniami.
### <a name="duplicate-agreements-in-the-biztalk-services-portal"></a>Duplikowanie umów w portalu usługi BizTalk
Scenariusz:
1. Utwórz umowę za pomocą API modelu OBIEKTOWEGO Trading zarządzania partnera.
2. Otwórz umowę w portalu usługi BizTalk na dwóch różnych kartach.
3. Wdrażanie umowę z obu kart.
4. W wyniku umowami są wdrażane uzyskując zduplikowane pozycje w portalu usługi BizTalk

**Obejście**. Otwórz dowolny umów zduplikowane w portalu usługi BizTalk i wdrożyć.  

### <a name="bridges-do-not-use-updated-certificate-even-after-a-certificate-has-been-updated-in-the-artifact-store"></a>Mosty nie należy używać zaktualizowany certyfikat, nawet w przypadku, gdy certyfikat został zaktualizowany w magazynie artefaktu
Rozważmy następujące scenariusze:  

**Scenariusz 1: Używanie certyfikatów opartych na odcisku palca do zabezpieczania przesyłania wiadomości z mostka do punktu końcowego usługi**  
Należy rozważyć scenariusza, której używasz certyfikatów opartych na odcisku palca w projekcie usługi BizTalk. Zaktualizowanie certyfikatu w portalu usługi BizTalk z tej samej nazwie, ale różnych odcisku palca, ale nie są aktualizowane projektu usługi BizTalk odpowiednio. W takim przypadku mostka może być nadal przetwarzanie wiadomości, ponieważ starsze dane certyfikatu nadal mogą znajdować się w pamięci podręcznej kanału. Po wykonaniu tej przetwarzania wiadomości nie powiedzie się.  

**Rozwiązanie**: zaktualizuj certyfikatu w programie project usługi BizTalk i ponownie wdróż projektu.  

**Scenariusz 2: Za pomocą opartego na nazwie zachowań do identyfikowania certyfikatów do zabezpieczania przesyłania wiadomości z mostka do punktu końcowego usługi**

Rozważmy scenariusz, gdzie opartego na nazwie zachowania jest używana do identyfikowania certyfikatów w projekcie usługi BizTalk. Zaktualizowanie certyfikatu w portalu usługi BizTalk, ale nie są aktualizowane projektu usługi BizTalk odpowiednio. W takim przypadku mostka może być nadal przetwarzanie wiadomości, ponieważ starsze dane certyfikatu nadal mogą znajdować się w pamięci podręcznej kanału. Po wykonaniu tej przetwarzania wiadomości nie powiedzie się.  

**Rozwiązanie**: zaktualizuj certyfikatu w programie project usługi BizTalk i ponownie wdróż projektu.  

### <a name="bridges-continue-to-process-messages-even-when-the-sql-database-is-offline"></a>Mosty w dalszym ciągu przetwarzania wiadomości, nawet wtedy, gdy jest w trybie offline z bazą danych SQL
Mosty usług BizTalk kontynuować proces wiadomości trochę czasu, nawet jeśli Microsoft Azure SQL Database (który przechowuje uruchomionego informacje, takie jak wdrożonym artefakty i procesy), jest w trybie offline. Jest tak, ponieważ usługi BizTalk używają pamięci podręcznej artefakty i konfigurację mostka.
Jeśli nie chcesz, aby mosty przetwarzania wszystkie wiadomości, gdy jest w trybie offline z bazą danych SQL, umożliwia poleceń cmdlet programu PowerShell usługi BizTalk zatrzymać lub wstrzymać usługę BizTalk. Zobacz [Azure BizTalk usługi zarządzania próbki](http://go.microsoft.com/fwlink/p/?LinkID=329019) dla poleceń cmdlet programu Windows PowerShell do zarządzania operacji.  
### <a name="reading-the-xml-message-within-a-bridges-custom-code-component-includes-an-extra-bom-character"></a>Czytanie wiadomości XML mostka kodu niestandardowego składnika zawiera nadmiarowe znaki BOM
Rozważmy scenariusz, której chcesz odczytać wiadomość XML w kodzie niestandardowym mostka. Jeśli korzystasz z System.Text.Encoding.UTF8.GetString(bytes) interfejsu API .NET nadmiarowe znaki BOM znajduje się w wyniku kwerendy na początku wiadomości. Tak, jeśli nie chcesz, aby dane wyjściowe, aby uwzględnić nadmiarowe znaki BOM, należy użyć ```System.IO.StreamReader().ReadToEnd()```.
### <a name="sending-messages-to-a-bridge-using-wcf-does-not-scale"></a>Wysyłanie wiadomości do mostka przy użyciu WCF nie działa
Wiadomości wysyłane do mostka przy użyciu WCF nie działa. Zamiast tego należy używać HttpWebRequest, jeśli chcesz skalowalna klienta.
### <a name="upgrade-token-provider-error-after-upgrading-from-biztalk-services-preview-to-general-availability-ga"></a>UAKTUALNIANIE: Błąd dostawcy tokenu po uaktualnieniu z poziomu widoku Podgląd usług BizTalk do (GA, General Availability)
Istnieje Edytuj lub AS2 umowy z partie aktywna. Po uaktualnieniu usługi BizTalk z poziomu widoku Podgląd do GA mogą wystąpić następujące czynności:
* Błąd: Dostawca tokenu mógł zapewnić tokenu zabezpieczeń. Dostawca tokenu zwracany komunikat: nie można rozpoznać nazwy zdalnej.

* Zadania wsadowe są anulowane.

**Obejście**: Usługa BizTalk-po zaktualizowaniu do (GA, General Availability), ponownie wdróż umowę.  

### <a name="upgrade-toolbox-shows-the-old-bridge-icons-after-upgrading-the-biztalk-services-sdk"></a>UAKTUALNIANIE: Zestaw narzędzi pokazuje starej ikony mostka po uaktualnieniu SDK usług BizTalk
Po uaktualnieniu starszej wersji SDK usług BizTalk, która ma starej ikony reprezentujące mosty, zestaw narzędzi w dalszym ciągu Pokaż starej ikony mosty. Jednak jeśli dodasz mostka projektanta powierzchnię projektu usługi BizTalk powierzchni zawiera nową ikonę.  

**Obejście**. Można obejść ten problem, usuwając pliki .tbd w obszarze <system drive>: \Users\<użytkownika > \AppData\Local\Microsoft\VisualStudio\11.0.  

### <a name="upgrade-biztalk-portal-update-from-preview-to-ga-might-show-an-error-indicating-that-the-edi-capability-is-not-available"></a>UAKTUALNIANIE: Aktualizacja portalu BizTalk z poziomu widoku Podgląd GA wyświetli komunikat o błędzie informujący, że funkcja grupy użytkowników nie jest dostępna
Po zalogowaniu się do portalu usługi BizTalk podczas usług BizTalk jest uaktualniana z wersji Preview do GA, może zostać wyświetlony następujący komunikat o błędzie w portalu:  

Ta funkcja nie jest dostępna w ramach tej wersji programu Microsoft Azure BizTalk Services. Aby użyć tych funkcji Przełącz się do odpowiedniej wersji.  

**Rozdzielczość**: wylogowania z portalu, zamknij i Otwórz przeglądarkę, a następnie logowanie do portalu.  
### <a name="upgrade-new-tracking-data-does-not-show-up-after-biztalk-services-is-upgraded-to-ga"></a>UAKTUALNIANIE: Nowe dane śledzenia nie jest wyświetlany po uaktualnieniu usługi BizTalk do GA
Załóżmy scenariusza, w którym masz mostka XML wdrożony w subskrypcji BizTalk usług Podgląd. Wysyłanie wiadomości do mostka i odpowiadające im dane śledzenia jest dostępna w portalu usługi BizTalk. Teraz Jeśli bitów runtime Portal usługi BizTalk i usług BizTalk są uaktualniane do GA i wysłać wiadomość do samego punktu końcowego mostka wcześniej używany, dane śledzenia nie jest wyświetlany wiadomości wysłane po uaktualnieniu.  

### <a name="pipelines-vs-bridges"></a>Procesy mosty v/s
W tym dokumencie terminów "procesy" i "mostki" są używane zamiennie. Oba zasadniczo oznacza to samo jest wdrożony w usługach BizTalk jednostka przetwarzania wiadomości.  

### <a name="concepts"></a>Pojęcia  

[BizTalk usług](https://msdn.microsoft.com/library/azure/hh689864.aspx)   
