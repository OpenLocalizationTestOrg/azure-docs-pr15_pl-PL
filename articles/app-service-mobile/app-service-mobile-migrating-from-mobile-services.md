<properties
    pageTitle="Migrowanie z usług mobilnych do aplikacji usługi aplikacji dla urządzeń przenośnych"
    description="Dowiedz się, jak łatwo migrowanie aplikacji usług Mobile aplikacji Mobile aplikacji usługi"
    services="app-service\mobile"
    documentationCenter=""
    authors="adrianhall"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="adrianha"/>

# <a name="article-top"></a>Migrowanie do istniejącej usługi mobilnej Azure do Azure aplikacji usługi

Z [ogólnodostępną Azure aplikacji usługi]witryn usługi Azure Mobile można łatwo zmigrować w miejscu na korzystanie ze wszystkich funkcji usługi Azure aplikacji.  W tym dokumencie omówiono czego mogą się spodziewać podczas migrowania witryny z usługi Azure Mobile usłudze Azure w aplikacji.

## <a name="what-does-migration-do"></a>Co robi migracji do witryny

Migracji z usługi mobilnej Azure staje się do usługi mobilnej aplikacji [Azure aplikacji usługi] bez wpływu na kod.  Koncentratory powiadomienie, połączenia danych SQL, ustawień uwierzytelniania, zaplanowanych zadań i nazwy domeny pozostaną niezmienione.  Klienci przenośni przy użyciu usługi Azure Mobile kontynuowanie normalnego działania.  Ponowne uruchomienie usługi migracji, gdy są przenoszone do Azure aplikacji usługi.

[AZURE.INCLUDE [app-service-mobile-migrate-vs-upgrade](../../includes/app-service-mobile-migrate-vs-upgrade.md)]

## <a name="why-migrate"></a>Dlaczego należy przeprowadzić migrację witryny

Microsoft jest zalecenie, możesz przeprowadzić migrację do usługi mobilnej Azure, aby można było korzystać z funkcji Azure usługi aplikacji, w tym:

  *  Nowe funkcje hosta, takich jak [WebJobs] i [niestandardowe nazwy domen].
  *  Łączność z zasobów lokalnego przy użyciu [VNet] oprócz [Hybrydowych połączenia].
  *  Monitorowania i rozwiązywania problemów z nowego Relic lub [Wniosków aplikacji].
  *  Wbudowane narzędzia DevOps, w tym [tymczasowej gniazda]przywrócić poprzednie i testowania w produkcji.
  *  [Automatyczne skalowanie], równoważenia obciążenia i [Monitorowanie wydajności].

Aby uzyskać więcej informacji o zaletach Azure aplikacji usługi zobacz temat [Usług i aplikacji usługi mobilnej] .

## <a name="before-you-begin"></a>Przed rozpoczęciem

Przed rozpoczęciem starań głównych w witrynie, należy [Wykonywanie kopii zapasowej usługi Mobile] skrypty i baza danych SQL.

## <a name="migrating-site"></a>Migrowanie witryny

Proces migracji migruje wszystkich witryn w jednym regionie Azure.

Aby przeprowadzić migrację witryny:

  1.  Zaloguj się do [portalu klasyczny Azure].
  2.  Wybierz pozycję usługi mobilnej w obszarze, który chcesz przeprowadzić migrację.
  3.  Kliknij przycisk **Migruj do aplikacji usługi** .

    ![Przycisk migracji][0]

  4.  W tym artykule migracji umożliwia otwarcie okna dialogowego aplikacji usługi.
  5.  Wprowadź nazwę usługi Mobile w odpowiednim polu.  Na przykład jeśli Twoja domena jest contoso.azure mobile.net, wprowadź _contoso_ w odpowiednim polu.
  6.  Kliknij przycisk osi.

Monitorowanie stanu migracji w monitorze aktywności. Witryny jest wyświetlany jako *migracji* w portalu klasyczny Azure.

  ![Monitorowanie aktywności migracji][1]

Każdy migracji może potrwać od 3 do 15 minut na komórkowej migrowane.  Witryny pozostaje dostępny podczas migracji.
Witryny jest ponownie na końcu procesu migracji.  Witryna jest niedostępny w trakcie procesu ponownego uruchamiania może trwać kilka sekund.

## <a name="finalizing-migration"></a>Finalizowanie migracji

Planowanie przetestować swoją witrynę klienta przenośnego po zakończeniu procesu migracji.  Upewnij się, że można wykonywać wszystkie typowe Akcje klienta bez zmian klienta przenośnego.  

### <a name="update-app-service-tier"></a>Wybierz odpowiednie aplikacji usługi ceny warstwy

Masz największą elastyczność ceny po migracji do usługi aplikacji Azure.

  1.  Zaloguj się do [portalu Azure].
  2.  Zaznacz **wszystkie zasoby** lub **Aplikacji usługi** , a następnie kliknij nazwę swojego migracją usługi mobilnej.
  3.  Domyślnie zostanie wyświetlona karta Ustawienia.
  4.  W menu Ustawienia kliknij pozycję **Planowanie usługi aplikacji** .
  5.  Kliknij Kafelek **Poziomu ceny** .
  6.  Kliknij odpowiedni do własnych potrzeb, a następnie kliknij przycisk **Wybierz**.  Może być konieczne kliknij pozycję **Wyświetl wszystkie** , aby wyświetlić dostępne ceny warstwy.

Jako punkt wyjścia zaleca się następujących poziomów:

| Usługa mobilna ceny warstwy | Poziom ceny aplikacji usługi |
| :-------------------------- | :----------------------- |
| Bezpłatne                        | Bezpłatne F1                  |
| Podstawowe                       | Podstawowe B1                 |
| Standardowe                    | Standardowe s1              |

Istnieje dużą elastyczność w wyborze po prawej stronie ceny warstwy aplikacji.  Zapoznaj się z [Aplikacji usługi ceny] szczegółowe informacje o cenach nowej usługi aplikacji.

> [AZURE.TIP]Warstwie aplikacji usługi standardowe zawiera dostęp do wielu funkcji, które chcesz użyć, tym [tymczasowej gniazda]automatycznych kopii zapasowych i automatyczne skalowanie.  Zapoznaj się z nowych funkcji, gdy jesteś!

### <a name="review-migration-scheduler-jobs"></a>Przejrzyj zadania harmonogramu migracją

Harmonogram zadań nie będzie widoczny do momentu około 30 minut po migracji.  Zaplanowane zadania kontynuować działanie w tle.
Aby wyświetlić zaplanowanych zadań, gdy są one widoczne ponownie:

  1.  Zaloguj się do [portalu Azure].
  2.  Wybierz pozycję **Przejdź >**, w polu _Filtr_ wprowadź **Harmonogram** , a następnie wybierz **Kolekcje harmonogram**.

Istnieje ograniczoną liczbą bezpłatne harmonogram zadań dostępne po migracji.  Zapoznaj się z zastosowania i [Plany harmonogramu Azure].

### <a name="configure-cors"></a>Konfigurowanie CORS w razie potrzeby

Współużytkowanie zasobów między pochodzenia to technika do zezwalania witrynie sieci Web uzyskać dostęp do interfejs API sieci Web z innej domeny.  Jeśli korzystasz z usługi Azure Mobile ze skojarzonymi witrynami sieci Web, należy skonfigurować CORS w ramach migracji.  Jeśli uzyskujesz dostęp do usługi Azure Mobile wyłącznie z urządzeniami przenośnymi, CORS nie muszą być skonfigurowane z wyjątkiem czasami.

Ustawienia CORS migracją są dostępne jako wartość ustawienia aplikacji **MS_CrossDomainWhitelist** .  Aby przeprowadzić migrację witryny do funkcji CORS usługi aplikacji:

  1.  Zaloguj się do [portalu Azure].
  2.  Zaznacz **wszystkie zasoby** lub **Aplikacji usługi** , a następnie kliknij nazwę swojego migracją usługi mobilnej.
  3.  Domyślnie zostanie wyświetlona karta Ustawienia.
  4.  Kliknij menu interfejsu API **CORS** .
  5.  Wprowadź miejsc pochodzenia dozwolone w odpowiednim polu, naciskając klawisz Enter po każdym z nich.
  6.  Gdy na liście dozwolonych miejsc pochodzenia jest poprawny, kliknij przycisk Zapisz.

> [AZURE.TIP]Jedną z zalet korzystania z aplikacji usługi Azure jest, że w tym samym miejscu można uruchamiać i witryny sieci web usługi mobilnej.  Aby uzyskać więcej informacji zobacz sekcję [Następne kroki](#next-steps) .

### <a name="download-publish-profile"></a>Pobieranie nowego profilu publikowania

Publikowanie profilu witryny został zmieniony podczas migracji do usługi aplikacji Azure.  Jeśli zamierzasz publikowanie witryny z poziomu programu Visual Studio, należy nowego profilu publikowania.  Aby pobrać nowy profil publikowania:

  1.  Zaloguj się do [portalu Azure].
  2.  Zaznacz **wszystkie zasoby** lub **Aplikacji usługi** , a następnie kliknij nazwę swojego migracją usługi mobilnej.
  3.  Kliknij przycisk **Pobierz Opublikuj profilu**.

Na komputerze jest pobierany plik PublishSettings.  Jest to zazwyczaj _Nazwa witryny_. PublishSettings.  Importuj ustawienia publikowania do istniejącego projektu:

  1.  Otwórz program Visual Studio i projektu usługi mobilnej Azure.
  2.  Kliknij prawym przyciskiem myszy projektu w **Eksploratorze rozwiązań** i wybierz pozycję **Publikuj...**
  3.  Kliknij przycisk **Importuj**
  4.  Kliknij przycisk **Przeglądaj** i wybierz swojego pobrany plik ustawień publikowania.  Kliknij **przycisk OK**
  5.  Kliknij przycisk **Sprawdź poprawność połączenia** zapewnienie pracy ustawienia publikowania.
  6.  Kliknij przycisk **Publikuj** publikowanie witryny.


## <a name="working-with-your-site"></a>Praca z Twojej witryny po migracji

Rozpoczynanie pracy z nowej usługi aplikacji w [Azure portal] po migracji.  Poniżej przedstawiono kilka uwag na określonym operacje, używane do wykonywania w [Klasycznym Portal Azure]wraz z ich odpowiedniki aplikacji usługi.

### <a name="publishing-your-site"></a>Pobieranie i publikowanie witryny migracją

Witryna jest dostępna za pośrednictwem cyfra lub ftp i może być ponownie opublikowana z różnych różne mechanizmy, w tym WebDeploy, TFS Mercurial, GitHub i FTP.  Poświadczenia wdrożenia są migrowane z resztą witryny.  Jeśli nie zostanie ustawiona poświadczenia wdrożenia lub nie pamiętasz, możesz je zresetować:

  1. Zaloguj się do [portalu Azure].
  2. Zaznacz **wszystkie zasoby** lub **Aplikacji usługi** , a następnie kliknij nazwę swojego migracją usługi mobilnej.
  3. Domyślnie zostanie wyświetlona karta Ustawienia.
  4. Kliknij pozycję **wdrożenie poświadczeń** w publikacji menu.
  5. Wprowadź nowe poświadczenia wdrożenia w odpowiednich polach, a następnie kliknij przycisk Zapisz.

Za pomocą tych poświadczeń klonowanie witryny cyfra lub skonfigurować automatyczne wdrożeniach z GitHub, TFS lub Mercurial.  Aby uzyskać więcej informacji zobacz [dokumentację wdrożenia Azure aplikacji usługi].

### <a name="appsettings"></a>Ustawienia aplikacji

Większość ustawień migracją Usługa mobilna są dostępne za pośrednictwem ustawień aplikacji.  Można uzyskać listę ustawień aplikacji z [Azure portal].
Aby wyświetlić lub zmienić ustawienia aplikacji:

  1. Zaloguj się do [portalu Azure].
  2. Zaznacz **wszystkie zasoby** lub **Aplikacji usługi** , a następnie kliknij nazwę swojego migracją usługi mobilnej.
  3. Domyślnie zostanie wyświetlona karta Ustawienia.
  4. W menu Ogólne kliknij pozycję **Ustawienia aplikacji** .
  5. Przewiń do sekcji Ustawienia aplikacji i znaleźć ustawienia aplikacji.
  6. Kliknij wartość ustawienia aplikacji, aby edytować wartość.  Kliknij przycisk **Zapisz** , aby zapisać wartość.

W tym samym czasie, możesz zaktualizować wielu ustawień aplikacji.

> [AZURE.TIP]Istnieją dwa ustawienia aplikacji o tej samej wartości.  Na przykład może zostać wyświetlony _ApplicationKey_ i _MS\_ApplicationKey_.  Zaktualizuj ustawienia obu aplikacji w tym samym czasie.

### <a name="authentication"></a>Uwierzytelnianie

Wszystkie ustawienia uwierzytelniania są dostępne jako ustawień aplikacji w witrynie migracją.  Aby zaktualizować ustawienia uwierzytelniania, należy zmienić ustawienia odpowiednią aplikację.  W poniższej tabeli przedstawiono ustawienia aplikacji odpowiednie dla swojego dostawcy uwierzytelniania:

| Dostawcy          | Identyfikator klienta                 | Tajny klienta                | Inne ustawienia             |
| :---------------- | :------------------------ | :--------------------------- | :------------------------- |
| Konto Microsoft | **MS\_MicrosoftClientID**  | **MS\_MicrosoftClientSecret** | **MS\_MicrosoftPackageSID** |
| Facebook          | **MS\_FacebookAppID**      | **MS\_FacebookAppSecret**     |                            |
| Twitter           | **MS\_TwitterConsumerKey** | **MS\_TwitterConsumerSecret** |                            |
| Google            | **MS\_GoogleClientID**     | **MS\_GoogleClientSecret**    |                            |
| Azure AD          | **MS\_AadClientID**        |                              | **MS\_AadTenants**          |

Uwaga: **MS\_AadTenants** jest przechowywana jako rozdzielaną średnikami listę domen dzierżawy (pola "Dozwolone dzierżaw" w portalu usługi telefon komórkowy).

> [AZURE.WARNING] **Nie używaj mechanizmy uwierzytelniania w menu Ustawienia**
>
> Azure aplikacji usługi zapewnia oddzielnych "bez użycia kodu" uwierzytelniania i autoryzacji system w obszarze _uwierzytelniania i autoryzacji_ menu ustawień i opcji _Uwierzytelniania Mobile_ (przestarzałe) w menu Ustawienia.  Te opcje są niezgodne z migracją usługi mobilnej Azure.  Możesz [uaktualnić witryny](app-service-mobile-net-upgrading-from-mobile-services.md) skorzystać z uwierzytelniania usługi aplikacji Azure.

### <a name="easytables"></a>Dane

Na karcie _dane_ w usługach Mobile został zastąpiony _Łatwe tabel_ w Azure portal.  Aby uzyskać dostęp do tabel proste:

  1. Zaloguj się do [portalu Azure].
  2. Zaznacz **wszystkie zasoby** lub **Aplikacji usługi** , a następnie kliknij nazwę swojego migracją usługi mobilnej.
  3. Domyślnie zostanie wyświetlona karta Ustawienia.
  4. Kliknij przycisk **tabele łatwe** w menu urządzeń przenośnych.

Można dodać tabelę, klikając przycisk **Dodaj** lub uzyskać dostęp do istniejących tabel, klikając nazwę tabeli.  Istnieją różne operacje można zrobić to karta, w tym:

* Zmienianie uprawnień do tabeli
* Edytowanie operacyjne skryptów
* Zarządzanie schematem tabeli
* Usunięcie tabeli
* Wyczyszczenie zawartości tabeli
* Usuwanie określonych wierszy w tabeli

### <a name="easyapis"></a>INTERFEJS API

Na karcie _interfejsu API_ w usługach Mobile został zastąpiony _API łatwe_ w Azure portal.  Aby uzyskać dostęp do interfejsów API proste:

  1. Zaloguj się do [portalu Azure].
  2. Zaznacz **wszystkie zasoby** lub **Aplikacji usługi** , a następnie kliknij nazwę swojego migracją usługi mobilnej.
  3. Domyślnie zostanie wyświetlona karta Ustawienia.
  4. Kliknij pozycję **API łatwe** w menu urządzeń przenośnych.

Z migracją interfejsów API znajdują się już w karta.  Można również dodać interfejs API z tym karta.  Aby zarządzać określonego interfejsu API, kliknij pozycję API.
Z nowego karta można dopasować uprawnień i edytować skrypty w przypadku interfejsu API.

### <a name="on-demand-jobs"></a>Harmonogram zadań

Wszystkie zadania harmonogramu są dostępne w sekcji harmonogramu zadania zbiorów.  Aby uzyskać dostęp do zadania harmonogramu:

  1. Zaloguj się do [portalu Azure].
  2. Wybierz pozycję **Przejdź >**, w polu _Filtr_ wprowadź **Harmonogram** , a następnie wybierz **Kolekcje harmonogram**.
  3. Wybierz zbiór zadania dla witryny.  Ma nazwę _Nazwa witryny_— zadania.
  4. Kliknij przycisk **Ustawienia**.
  5. W obszarze Zarządzanie kliknij pozycję **Harmonogram zadań** .

Zaplanowane zadania są wyświetlane z częstotliwością określonej przed migracją.  Zadania na żądanie są wyłączone.  Aby uruchomić zadanie na żądanie:

  1. Zaznacz zadanie, które chcesz uruchomić.
  2. W razie potrzeby kliknij pozycję **Włącz** umożliwiające zadania.
  3. Kliknij pozycję **Ustawienia**, a następnie pozycję **Harmonogram**.
  4. Wybierz ponownemu **raz**, a następnie kliknij przycisk **Zapisz**

Zadań na żądanie znajdują się w `App_Data/config/scripts/scheduler post-migration`.  Firma Microsoft zaleca Konwertuj wszystkie zadania na żądanie [WebJobs] lub [Funkcje].  Napisz nowe zadania harmonogramu jako [WebJobs] lub [funkcji].

### <a name="notification-hubs"></a>Powiadomienie o koncentratory

Telefon komórkowy usług używa koncentratory powiadomienie powiadomienia wypychane.  Następujące ustawienia aplikacji są używane do łącze Centrum powiadomienie z usługą komórkowego po migracji:

| Ustawienia aplikacji                    | Opis                              |
| :------------------------------------- | :--------------------------------------- |
| **MS\_PushEntityNamespace**             | Powiadomienie o Namespace Centrum           |
| **MS\_NotificationHubName**             | Nazwa centrum powiadomienia                |
| **MS\_NotificationHubConnectionString** | Parametry połączenia Centrum powiadomień   |
| **MS\_NamespaceName**                   | Alias MS_PushEntityNamespace      |

Twoim Centrum powiadomienie odbywa się za pomocą [Azure portal].  Zanotuj nazwę koncentratora powiadomienie (znajdują się to za pomocą ustawień aplikacji):

  1. Zaloguj się do [portalu Azure].
  2. Wybierz pozycję **Przeglądaj**>, wybierz pozycję **Koncentratory powiadomień**
  3. Kliknij nazwę powiadomień Centrum skojarzone z usługi mobilnej.

> [AZURE.NOTE]Jeśli Twoim Centrum powiadomienie jest typu "Mieszane", nie jest widoczne.  "Mieszane" wpisz powiadomienie, że koncentratory są używane funkcje koncentratory powiadomienie i starszych Bus usługi.  Przed kontynuowaniem, [Konwertowanie usługi mieszane przestrzenie nazw] .  Po zakończeniu konwersji Twoim Centrum powiadomienie pojawia się w [portalu Azure].

Aby uzyskać więcej informacji należy zapoznać się z dokumentacją [Koncentratory powiadomienie] .

> [AZURE.TIP]Funkcje zarządzania koncentratory powiadomienie [Azure portal] nadal znajdują się w wersji preview.  [Klasyczny Portal Azure] pozostaje dostępny do zarządzania koncentratory powiadomienie.

### <a name="legacy-push"></a>Ustawienia starsze Push

Jeśli skonfigurowano wypychanych od urządzeń przenośnych usługi przed wprowadzeniem na koncentratory powiadomienie używasz _starsze push_.  Jeśli używasz wypychanych i koncentratora powiadomienie wymienionych w konfiguracji nie jest widoczne, następnie prawdopodobnie korzystasz z _wypychanych starsze_.  Ta funkcja jest migrowana z innymi funkcjami.  Jednak zaleca się uaktualnienie do koncentratorów powiadomienie po zakończeniu migracji.

W międzyczasie wszystkie ustawienia starsze wypychanych (z wyjątkiem godne uwagi certyfikat APN) są dostępne w obszarze Ustawienia aplikacji.  Aktualizowanie certyfikat APN, zamieniając odpowiedniego pliku na systemu plików.

### <a name="app-settings"></a>Inne ustawienia aplikacji

Następujące ustawienia dodatkowe aplikacji są zmigrowanych z usługi Mobile i dostępne w obszarze *Ustawienia* > *Ustawień aplikacji*:

| Ustawienia aplikacji              | Opis                             |
| :------------------------------- | :-------------------------------------- |
| **MS\_MobileServiceName**         | Nazwa aplikacji                    |
| **MS\_MobileServiceDomainSuffix** | Prefiks domeny. znaczy Azure mobile.net |
| **MS\_ApplicationKey**            | Klucz aplikacji                    |
| **MS\_umożliwia**                 | Klucz wzorzec aplikacji                     |

Klawisz aplikacji i klucza głównego są identyczne kluczy aplikacji z oryginalnym usługi Mobile.  W szczególności klawisz aplikacji jest wysyłane przez klientów przenośnych Aby sprawdzić poprawność ich zastosowania urządzeń przenośnych interfejsu API.

### <a name="cliequivalents"></a>Wiersze polecenia

Polecenie _azure mobile_ umożliwia już Zarządzanie witryną usługi Azure Mobile.  Zamiast tego polecenia _azure witryny_ zostały zastąpione wiele funkcji.  Aby znaleźć odpowiedniki dla popularnych poleceń, skorzystaj z poniższej tabeli:

| _Azure Mobile_ Polecenie                     | Polecenie odpowiadające mu _Witryny Azure_            |
| :----------------------------------------- | :----------------------------------------- |
| lokalizacje urządzeń przenośnych                           | Lista lokalizacji witryny                         |
| listy dla urządzeń przenośnych                                | Lista witryn                                  |
| Pokaż przenośnych _nazwy_                         | Pokaż witryny _Nazwa_                           |
| ponowne uruchomienie komputera przenośnego _nazwy_                      | Uruchom ponownie witryny _Nazwa_                        |
| urządzeń przenośnych ponownego wdrażania _nazwy_                     | Witryna wdrażania ponownego wdrażania _commitId_ _nazwy_ |
| _Nazwa_ _typu_ _wartość_ ustawiony klucz urządzeń przenośnych       | _Nazwa_ _klucza_ Usuń appsetting witryny <br/> Dodawanie witryny appsetting _klucza_=_wartość_ _nazwy_ |
| Lista konfiguracji urządzeń przenośnych _nazwy_                  | Lista appsetting witryny _Nazwa_                |
| Uzyskiwanie konfiguracji urządzeń przenośnych _nazwy_ _klucza_             | _Nazwa_ _klucza_ Pokaż appsetting witryny          |
| Ustawianie konfiguracji urządzeń przenośnych _nazwy_ _klucza_             | _Nazwa_ _klucza_ Usuń appsetting witryny <br/> Dodawanie witryny appsetting _klucza_=_wartość_ _nazwy_ |
| Lista urządzeń przenośnych domeny _nazwy_                  | Lista domeny witryny _Nazwa_                    |
| Domena przenośnych dodać _nazwy_ _domeny_          | domeny witryny Dodawanie _domeny_ _nazwy_            |
| Usuń domeny przenośnych _nazwy_                | _Nazwa_ _domeny_ Usuń domenę witryny         |
| Pokaż przenośnych skali _nazwy_                   | Pokaż witryny _Nazwa_                           |
| Skala przenośnych Zmień _nazwę_                 | _Tryb_ tryb skali witryny _Nazwa_ <br /> _wystąpienia_ _nazwy_ obiektów skali witryny |
| listy urządzeń przenośnych appsetting _nazwy_              | Lista appsetting witryny _Nazwa_                |
| appsetting urządzeń przenośnych Dodaj _klucz_ _Nazwa_ _wartości_ | Dodawanie witryny appsetting _klucza_=_wartość_ _nazwy_   |
| _Nazwa_ delete przenośnych appsetting _klucza_      | _Nazwa_ _klucza_ Usuń appsetting witryny        |
| _nazwy_ Pokaż przenośnych appsetting _klucza_        | _Nazwa_ _klucza_ Usuń appsetting witryny        |

Zaktualizuj uwierzytelniania lub ustawienia powiadomień wypychanych aktualizując odpowiednie ustawienia aplikacji.
Edytowanie plików i publikowanie witryny ftp lub cyfra.

### <a name="diagnostics"></a>Narzędzia diagnostyczne i rejestrowania

Rejestrowanie diagnostyczne zazwyczaj jest wyłączona w usłudze Azure aplikacji.  Aby włączyć rejestrowanie diagnostyczne:

  1. Zaloguj się do [portalu Azure].
  2. Zaznacz **wszystkie zasoby** lub **Aplikacji usługi** , a następnie kliknij nazwę swojego migracją usługi mobilnej.
  3. Domyślnie zostanie wyświetlona karta Ustawienia.
  4. Wybierz pozycję **Dzienniki diagnostyczne** w menu funkcje.
  5. **Polecenie dla następujących dzienniki** : **Aplikacji rejestrowania (system plików)**, **szczegółowe komunikaty o błędach**i **Śledzenie żądań nie powiodła się**
  6. Kliknij pozycję **System plików** dla rejestrowania serwera sieci Web
  7. Kliknij przycisk **Zapisz**

Aby wyświetlić dzienniki:

  1. Zaloguj się do [portalu Azure].
  2. Zaznacz **wszystkie zasoby** lub **Aplikacji usługi** , a następnie kliknij nazwę swojego migracją usługi mobilnej.
  3. Kliknij przycisk **Narzędzia**
  4. W OBSERVE menu, wybierz pozycję **strumienia dziennika** .

Dzienniki są wyświetlane w oknie, jak są one generowane.  Możesz też pobrać dzienniki dla przyszłych analiz przy użyciu swoich poświadczeń wdrożenia. Aby uzyskać więcej informacji zobacz dokumentację [rejestrowania] .

## <a name="known-issues"></a>Znane problemy

### <a name="deleting-a-migrated-mobile-app-clone-causes-a-site-outage"></a>Usuwanie migracji Mobile klonowanie aplikacji powoduje, że awaria witryny

Jeśli klonowanie migracją usług mobilnych przy użyciu programu PowerShell Azure, należy usunąć klonowanie, wpis DNS dla usługi produkcji zostanie usunięty.  Witryny nie jest dostępne w Internecie.  

Rozwiązanie: Jeśli chcesz klonowanie witryny, zrób to za pośrednictwem portalu.

### <a name="changing-webconfig-does-not-work"></a>Zmienianie Web.config nie działa

Jeśli masz witryny ASP.NET zmieni się na `Web.config` pliku nie zostać zastosowane.  Usługa aplikacji Azure tworzy odpowiedniej `Web.config` plik podczas uruchamiania do obsługi środowisko uruchomieniowe usługi Mobile.  Niektóre ustawienia (na przykład nagłówki niestandardowe) można zastąpić, używając pliku transformacji XML.  Tworzenie pliku w o nazwie `applicationHost.xdt` -ten plik musi trafiać `D:\home\site` katalogu w usłudze Azure.  Przekazywanie `applicationHost.xdt` plików za pomocą skryptu niestandardowego wdrożenia lub bezpośrednio przy użyciu Kudu.  Poniżej przedstawiono przykładowy dokument:

```
<?xml version="1.0" encoding="utf-8"?>
<configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
  <system.webServer>
    <httpProtocol>
      <customHeaders>
        <add name="X-Frame-Options" value="DENY" xdt:Transform="Replace" />
        <remove name="X-Powered-By" xdt:Transform="Insert" />
      </customHeaders>
    </httpProtocol>
    <security>
      <requestFiltering removeServerHeader="true" xdt:Transform="SetAttributes(removeServerHeader)" />
    </security>
  </system.webServer>
</configuration>
```

Aby uzyskać więcej informacji zobacz dokumentację [XDT Przekształcanie próbki] na GitHub.

### <a name="migrated-mobile-services-cannot-be-added-to-traffic-manager"></a>Migracją Mobile usług nie można dodać do Menedżera ruchu

Po utworzeniu profilu Menedżera ruch bezpośrednio nie można wybrać migracją Usługa mobilna do profilu.  Używanie "zewnętrzny punkt końcowy."  Punkt końcowy zewnętrzne można dodawać tylko przy użyciu programu PowerShell.  Aby uzyskać więcej informacji zobacz [Samouczek Menedżer ruchu](https://azure.microsoft.com/blog/azure-traffic-manager-external-endpoints-and-weighted-round-robin-via-powershell/).

## <a name="next-steps"></a>Następne kroki

Teraz, gdy aplikacja migracji do usługi aplikacji, istnieją dodatkowe funkcje, których można używać:

  * Wdrożenie [tymczasowej gniazda] umożliwiają etapu zmian do witryny i wykonać A / B testowania.
  * [WebJobs] podać zastępuje zadania zaplanowanego na żądanie.
  * Możesz [stale wdrażanie] witryny za pomocą łączenia witryny z GitHub, TFS lub Mercurial.
  * Za pomocą [Aplikacji wniosków] monitorowanie witryny.
  * Służyć witryny sieci Web i interfejs API Mobile z tego samego kodu.

### <a name="upgrading-your-site"></a>Uaktualnianie witryny usług Mobile do SDK aplikacje Mobile Azure

  * W przypadku projektów na serwerze opartym na Node.js nowego [Zestawu SDK Node.js aplikacje Mobile] udostępnia kilka nowych funkcji. Na przykład można teraz zrobić lokalne rozwoju i debugowanie, użyj dowolnej wersji Node.js powyżej 0,10 i dostosowywanie przy użyciu dowolnego pośredniczącym Express.js.

  * Aby. Projekty oparte na sieci serwera nowe [pakiety NuGet SDK aplikacje Mobile](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) jest bardziej elastyczne o współzależnościach NuGet.  Te pakiety obsługuje nowe uwierzytelnianie usługi aplikacji i zredaguj z projektu programu ASP.NET. Aby dowiedzieć się więcej na temat uaktualniania, zobacz [Uaktualnianie do istniejącej usługi mobilnej .NET do aplikacji usługi](app-service-mobile-net-upgrading-from-mobile-services.md).

<!-- Images -->
[0]: ./media/app-service-mobile-migrating-from-mobile-services/migrate-to-app-service-button.PNG
[1]: ./media/app-service-mobile-migrating-from-mobile-services/migration-activity-monitor.png
[2]: ./media/app-service-mobile-migrating-from-mobile-services/triggering-job-with-postman.png

<!-- Links -->
[Cennik aplikacji usługi]: https://azure.microsoft.com/en-us/pricing/details/app-service/
[Wnioski aplikacji]: ../application-insights/app-insights-overview.md
[Automatyczne skalowanie]: ../app-service-web/web-sites-scale.md
[Azure aplikacji usługi]: ../app-service/app-service-value-prop-what-is.md
[Azure dokumentacji wdrażania aplikacji usługi]: ../app-service-web/web-sites-deploy.md
[Portal Azure klasyczny]: https://manage.windowsazure.com
[Azure portal]: https://portal.azure.com
[Azure Region]: https://azure.microsoft.com/en-us/regions/
[Plany harmonogramu Azure]: ../scheduler/scheduler-plans-billing.md
[wdrażanie przez cały czas]: ../app-service-web/app-service-continuous-deployment.md
[Konwertowanie z nazw mieszanym]: https://azure.microsoft.com/en-us/blog/updates-from-notification-hubs-independent-nuget-installation-model-pmt-and-more/
[curl]: http://curl.haxx.se/
[niestandardowe nazwy domen]: ../app-service-web/web-sites-custom-domain-name.md
[Fiddler]: http://www.telerik.com/fiddler
[ogólnodostępną Azure aplikacji usługi]: https://azure.microsoft.com/blog/announcing-general-availability-of-app-service-mobile-apps/
[Hybrydowe połączenia]: ../app-service-web/web-sites-hybrid-connection-get-started.md
[Rejestrowanie]: ../app-service-web/web-sites-enable-diagnostic-log.md
[SDK Node.js aplikacji dla urządzeń przenośnych]: https://github.com/azure/azure-mobile-apps-node
[Usług i aplikacji usługi mobilnej]: app-service-mobile-value-prop-migration-from-mobile-services.md
[Powiadomienie o koncentratory]: ../notification-hubs/notification-hubs-push-notification-overview.md
[Monitorowanie wydajności]: ../app-service-web/web-sites-monitor.md
[Postman]: http://www.getpostman.com/
[Wykonywanie kopii zapasowej z usługi mobilnej]: ../mobile-services/mobile-services-disaster-recovery.md
[tymczasowy gniazda]: ../app-service-web/web-sites-staged-publishing.md
[VNet]: ../app-service-web/web-sites-integrate-with-vnet.md
[WebJobs]: ../app-service-web/websites-webjobs-resources.md
[Przykłady przekształcenie XDT]: https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples
[Funkcje]: ../azure-functions/functions-overview.md
