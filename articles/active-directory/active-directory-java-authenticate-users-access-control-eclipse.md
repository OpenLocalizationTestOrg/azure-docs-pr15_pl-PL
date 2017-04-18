<properties
    pageTitle="Jak używać kontroli dostępu (Java) | Microsoft Azure"
    description="Dowiedz się, jak można opracowywać i kontroli dostępu za pomocą języka Java platformy Azure."
    services="active-directory" 
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor="" />

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm" />

# <a name="how-to-authenticate-web-users-with-azure-access-control-service-using-eclipse"></a>Sposób uwierzytelniania użytkowników w sieci Web z usługą Kontrola dostępu Azure za pomocą Zaćmienie

Ten przewodnik procedurach pokazano, jak używać usługi kontroli dostępu (ACS) Azure, w ramach zestawu narzędzi Azure dla programu Eclipse. Aby uzyskać więcej informacji o ACS zobacz sekcję [Następne kroki](#next_steps) .

> [AZURE.NOTE]
> Filtr sterowania usług programu Access Azure jest podgląd technologii społeczności. Jako wersji wstępnej oprogramowania jest formalnego nieobsługiwany przez firmę Microsoft.

## <a name="what-is-acs"></a>Co to jest ACS?

Większość deweloperów są ekspertów tożsamości i zwykle nie chcesz poświęcać czasu opracowywania mechanizmy uwierzytelniania i autoryzacji dla swoich aplikacji i usług. ACS to usługa Azure, która zapewnia łatwy sposób uwierzytelniania użytkowników, którzy chcą korzystać z aplikacji i usług bez konieczności współczynnik złożonych uwierzytelniania logiki do kodu.

Następujące funkcje są dostępne w ACS:

-   Integracja z systemem Windows Identity Foundation (WIF).
-   Obsługa dostawcy tożsamości popularny w Internecie (adresy IP) w tym identyfikator Windows Live ID, Google, Yahoo! i serwisu Facebook.
-   Pomoc techniczna dla usług federacyjnych Active Directory (AD FS) 2.0.
-   Protokołu Open Data (OData)-podstawie usługi zarządzania, która zapewnia programowy dostęp do ustawień ACS.
-   Do portalu zarządzania, który umożliwia dostęp administracyjny do ustawień ACS.

Aby uzyskać więcej informacji na temat ACS zobacz [2.0 usługi kontroli dostępu][].

## <a name="concepts"></a>Pojęcia

Azure ACS jest oparty na podmioty opartych na oświadczeniach tożsamości — spójne podejście do tworzenia mechanizmy uwierzytelniania dla aplikacji uruchamianych lokalnego lub w chmurze. Tożsamości opartych na oświadczeniach umożliwia wspólnych dla aplikacji i usług, które umożliwia uzyskanie tożsamości potrzebne informacje o użytkownikach w ich organizacji w innych organizacjach i w Internecie.

Aby wykonać zadania w tym przewodniku, zapoznaj się następujące pojęcia:

**Klient** — w kontekście ten przewodnik jest przeglądarki, która próbuje uzyskać dostęp do aplikacji sieci web.

**Aplikacja firmy (RP) Relying** - RP aplikacji jest witryny sieci Web lub usługa, która outsources uwierzytelniania jednego zewnętrznego urzędu. W żargon tożsamości mówi się, że RP zaufanie tego urzędu. Ten przewodnik wyjaśniono, jak skonfigurować aplikację do zaufania ACS.

**Token** - tokenu to zbiór danych zabezpieczeń, który jest zazwyczaj wydawane po pomyślnym uwierzytelnianie użytkownika. Zawiera zestaw *oświadczeń*, atrybuty uwierzytelnionego użytkownika. Roszczenia może oznaczać nazwy użytkownika, identyfikator roli użytkownika, do której należy użytkownik wieku i tak dalej. Token jest zazwyczaj podpisany cyfrowo, co oznacza może być sprowadzana zawsze powrót do jego wystawcy i jego zawartość nie będzie możliwe modyfikowanie. Użytkownik uzyskuje dostęp do aplikacji RP prezentując prawidłowy token wystawiony przez urząd, którego aplikacji RP zaufania.

**Dostawcy tożsamości (IP)** - IP jest urzędu, który uwierzytelnia tożsamości użytkowników i tokeny zabezpieczające. Praca rzeczywista wystawienia tokenów jest zaimplementowana, jeśli specjalne usługi o nazwie zabezpieczeń Token usługi (STS). Typowe adresy IP przykładami identyfikator Windows Live ID, Facebook, repozytoria użytkowników biznesowych (na przykład usługa Active Directory) i tak dalej.
Gdy jest skonfigurowana do zaufania IP, system zaakceptować i sprawdź poprawność tokeny wydawany przez tego adresów IP. ACS wiarygodność wielu adresy IP jednocześnie, co oznacza, że po aplikacji zaufania ACS, możesz od razu zaoferować aplikacji dla wszystkich uwierzytelnionych użytkowników wszystkie adresy IP że ACS zaufania w Twoim imieniu.

**Federacja dostawcy FP** - adresy IP bezpośredni znają użytkowników, ich przy użyciu poświadczeń uwierzytelniania i problemu oświadczeniach o wiedzą o nich. Federacja dostawcy FP jest inny rodzaj Urząd: zamiast bezpośrednio uwierzytelniania użytkowników, działa jako pośrednik i brokerów uwierzytelniania między RP jednego i jeden lub więcej adresy IP. Adresy IP jak również k/s wystawienia tokenów zabezpieczających, w związku z tym korzystają zabezpieczeń Token Services (STS). ACS jest jednym FP.

**Aparat reguł ACS** — logiczny umożliwia przekształcanie przychodzących tokenów z zaufanych adresy IP tokeny mają być używane przez RP są oznaczone w formularzu oświadczeniach prostego przekształcenia reguł. ACS funkcji aparatem reguły, który ma zwracać stosowania niezależnie od logiki transformacja określone dla swojego RP.

**Namespace ACS** - przestrzeń nazw jest partycją najwyższego poziomu ACS służące do organizowania ustawień. Przestrzeń nazw zawiera adresy IP, które są godne zaufania, aplikacje RP, których chcesz używać, reguły można się spodziewać reguły aparat przetwarzania przychodzących tokenów z i tak dalej. Przestrzeń nazw udostępnia różne punkty końcowe, które będą używane przez aplikację i projektanta uzyskanie ACS jego funkcji.

Na poniższej ilustracji pokazano, jak działa uwierzytelnianie ACS z aplikacji sieci web:

![Diagram przepływu ACS][acs_flow]

1.  (W tym przypadku przeglądarce) zażąda strony z RP.
2.  Ponieważ żądanie nie jest jeszcze uwierzytelniony, RP przekierowuje użytkownika do urzędu, zaufaną, która jest ACS. ACS będzie zawierał użytkownika z opcją adresy IP, które zostały określone dla tego RP. Gdy użytkownik wybierze odpowiednich adresów IP.
3.  Klient przejdzie do strony uwierzytelniania IP i monit o zalogowanie się.
4.  Po uwierzytelnieniu klienta (na przykład tożsamości, wprowadzone poświadczenia) IP problemy tokenu zabezpieczającego.
5.  Po wydaniu tokenu zabezpieczającego, IP przekierowuje klienta do ACS, a klient wysyła tokenu zabezpieczającego wydawane przez IP ACS.
6.  ACS sprawdza tokenu zabezpieczającego wydawany przez adresów IP, wartości wejściowych tożsamości roszczeń w tym token do aparat reguł ACS, oblicza wynik oświadczeń tożsamości i problemy nowy token zawierający roszczenia te dane wyjściowe.
7.  ACS przekierowuje klienta do RP. Klient wysyła nowy token wydawane przez ACS RP. RP sprawdza podpis na tokenu zabezpieczającego wydawany przez ACS, sprawdza roszczeń w tym token i zwraca stronę pierwotnie żądany.

## <a name="prerequisites"></a>Wymagania wstępne

Aby wykonać zadania w tym przewodniku, będą potrzebne następujące elementy:

- Java Developer Kit (JDK), v 1,6 lub nowszym.
- Zaćmienie-IDE dla deweloperów Estonia Java indygo lub nowszym. To można pobrać z <http://www.eclipse.org/downloads/>. 
- Rozkład serwera aplikacji, takich jak Apache Tomcat, GlassFish, serwer aplikacji JBoss lub molo lub serwera sieci web opartych na Java.
- Azure subskrypcji, które mogą pochodzić z <http://www.microsoft.com/windowsazure/offers/>.
- Zestaw narzędzi Azure dla Zaćmienie, zwolnij kwietnia 2014 lub nowszym. Aby uzyskać więcej informacji zobacz [Instalowanie narzędzi Azure dla programu Eclipse](http://msdn.microsoft.com/library/windowsazure/hh690946.aspx).
- Certyfikat X.509 korzystać z aplikacji. Konieczne będzie ten certyfikat zarówno certyfikatu publicznego (cer), jak i wymiana informacji osobistych (. Format PFX). (Opcji do tworzenia ten certyfikat będzie opisane w dalszej części tego samouczka).
- Znajomość Azure obliczyć emulatora i technik wdrażania omawiane na [Tworzenie aplikacji Witaj świecie dla Azure w Zaćmienie](http://msdn.microsoft.com/library/windowsazure/hh690944.aspx).

## <a name="create-an-acs-namespace"></a>Tworzenie Namespace ACS

Aby rozpocząć korzystanie z usługi kontroli dostępu (ACS) platformy Azure, możesz utworzyć nazw ACS. Obszar nazw zapewnia unikatowe zakres adresowania ACS zasobów z poziomu aplikacji.

1. Logowanie do [portalu zarządzania Azure][].
2. Kliknij pozycję **usługi Active Directory**. 
3. Aby utworzyć nowy obszar nazw kontrola dostępu, kliknij przycisk **Nowy**, kliknij pozycję **Usługi aplikacji**, kliknij opcję **Kontrola dostępu**, a następnie kliknij **Szybkie tworzenie**. 
4. Wprowadź nazwę obszaru nazw. Azure sprawdza, czy nazwa jest unikatowy.
5. Wybierz region, w którym jest używany obszaru nazw. Aby uzyskać optymalną wydajność Użyj region, w którym są wdrażanie aplikacji.
6. Jeśli masz więcej niż jedną subskrypcję, wybierz subskrypcję, do której chcesz użyć dla nazw ACS.
7. Kliknij przycisk **Utwórz**.

Azure tworzy i włącza obszaru nazw. Poczekaj, aż stan nowy obszar nazw jest **aktywna** , przed kontynuowaniem. 

## <a name="add-identity-providers"></a>Dodawanie dostawcy tożsamości

W tym zadaniu możesz dodać adresy IP do użytku z aplikacji RP uwierzytelniania. Ze względów pokaz to zadanie pokazano, jak dodawać usługi Windows Live jako adresów IP, ale można użyć dowolnej z adresy IP, wyświetlana w portalu zarządzania ACS.


1.  W [Portalu zarządzania Azure][]kliknij **Usługi Active Directory**, zaznacz nazw kontrola dostępu, a następnie kliknij pozycję **Zarządzaj**. Zostanie otwarty do portalu zarządzania ACS.
2.  W okienku nawigacji po lewej stronie w portalu zarządzania ACS kliknij **dostawcy tożsamości**.
3.  Identyfikator Windows Live ID jest domyślnie włączona, a nie można usunąć. Na potrzeby tego samouczka jest używana tylko identyfikator Windows Live ID. Ten ekran jest jednak miejsce, w którym można dodać inne adresy IP, klikając przycisk **Dodaj** .

Identyfikator Windows Live ID jest teraz włączone jako adresów IP dla usługi nazw ACS. Następnie określ aplikacji sieci web języka Java (aby utworzone później) jako RP.

## <a name="add-a-relying-party-application"></a>Dodawanie uzależnionej aplikacji innych firm

W tym zadaniu można skonfigurować ACS rozpoznanie aplikacji sieci web Java jako prawidłową aplikacją RP.

1.  W portalu zarządzania ACS kliknij polecenie **Relying aplikacji innych firm**.
2.  Na stronie **Polegaj aplikacji innych firm** kliknij przycisk **Dodaj**.
3.  Na stronie **Dodawanie Polegaj aplikacji innych firm** wykonaj następujące czynności:
    1.  W polu **Nazwa**wpisz nazwę RP. Na potrzeby tego samouczka wpisz **Azure Web App**.
    2.  W **trybie**wybierz **Wprowadź ustawienia ręcznie**.
    3.  W **obszarze**wpisz identyfikator URI, do którego ma zastosowanie tokenu zabezpieczającego wydawany przez ACS. Dla tego zadania, wpisz **8080 /**.
        ![Polegaj obszaru strony do użycia w emulatora obliczeń][relying_party_realm_emulator]
    4.  **Zwraca** adres URL wpisz adres URL, do którego ACS zwraca tokenu zabezpieczającego. Dla tego zadania, wpisz **http://localhost:8080/MyACSHelloWorld/index.jsp**
        ![firmy Relying adres zwrotny URL do użycia w emulatora obliczeń][relying_party_return_url_emulator]
    5.  Zaakceptuj wartości domyślne w pozostałych pól.

4.  Kliknij przycisk **Zapisz**.

Teraz pomyślnie skonfigurowano aplikacji sieci web Java uruchomienie w emulatorze Azure obliczeń (u 8080 /) należy RP w obszarze nazw ACS. Następnie należy utworzyć reguły, które ACS korzysta z przetwarzania roszczeń dotyczących RP.

## <a name="create-rules"></a>Tworzenie reguł

W tym zadaniu definiowania reguły, które wpływają na sposób przekazywania roszczeń związanych z adresy IP do swojego RP. Do tego przewodnika skonfiguruje możemy po prostu ACS, aby skopiować typy oświadczeń wejściowych i wartości bezpośrednio w token dane wyjściowe bez filtrowania lub modyfikowanie ich.

1.  Na stronie głównej portalu zarządzania ACS kliknij pozycję **reguły grupy**.
2.  Na stronie **Grupy reguł** kliknij przycisk **Domyślne grupy reguł dla aplikacji sieci Web Azure**.
3.  Na stronie **Edytowanie reguły grupy** kliknij pozycję **Generuj**.
4.  Na **wygenerować reguł: domyślne grupy reguł dla aplikacji sieci Web Azure** strony, upewnij się, identyfikator Windows Live ID jest zaznaczone pole wyboru, a następnie kliknij pozycję **Generuj**.    
5.  Na stronie **Edytowanie reguły grupy** kliknij przycisk **Zapisz**.

## <a name="upload-a-certificate-to-your-acs-namespace"></a>Przekazywanie certyfikatu do obszaru nazw ACS

W tym zadaniu przekazywania. PFX certyfikat, który będzie używany do podpisywania tokenu żądania utworzone przez obszaru nazw ACS.

1.  Na stronie głównej portalu zarządzania ACS kliknij **certyfikatów i kluczy**.
2.  Na stronie **certyfikatów i kluczy** kliknij przycisk **Dodaj** powyżej **Podpisywania tokenu**.
3.  Na stronie **Dodawanie Token podpisywania lub klucza** :
    1. W sekcji **używane dla** kliknij **Polegaj aplikacji** i wybierz **Azure Web App** (który wcześniej ustawiony jako nazwę uzależnionej aplikacji innych firm).
    2. W sekcji **Typ** wybierz **Certyfikat X.509**.
    3. W sekcji **certyfikat** kliknij przycisk Przeglądaj i przejdź do pliku certyfikat X.509, który ma być używany. Są to. Plik PFX. Zaznacz plik, kliknij przycisk **Otwórz**, a następnie w polu tekstowym **hasło** wprowadź hasło certyfikatu. Należy zauważyć, że do celów testowych, możesz korzystać z włączenie-signed certyfikat. Aby utworzyć certyfikat z podpisem własnym, za pomocą przycisku **Nowy** w oknie dialogowym **Biblioteka filtru ACS** (opisana dalej) lub za pomocą narzędzia **encutil.exe** z [witryny sieci Web projektu][] Azure Starter Kit dla języka Java.
    4. Upewnij się, że **Jako główny** jest zaznaczone. Strony **Dodawanie Token podpisywania lub klawisz** powinna wyglądać podobny do następującego.
        ![Dodawanie certyfikatu podpisywania tokenu][add_token_signing_cert]
    5. Kliknij przycisk **Zapisz** , aby zapisać ustawienia i zamknij stronę **certyfikatu podpisywania tokenu Dodawanie lub klucza** .

Następnie należy zapoznać się z informacjami na stronie integracji aplikacji i skopiować identyfikator URI, który należy skonfigurować aplikację sieci web Java umożliwia ACS.

## <a name="review-the-application-integration-page"></a>Sprawdź stronę integracji aplikacji

Można znaleźć wszystkie informacje i kod niezbędne do skonfigurowania aplikacji web Java (RP aplikacji) do pracy z ACS na stronie integracji aplikacji portalu zarządzania ACS. Te informacje będą potrzebne podczas konfigurowania aplikacji sieci web Java federacyjnych uwierzytelniania.

1.  W portalu zarządzania ACS kliknij **integracji aplikacji**.  
2.  Na stronie **Integracji aplikacji** kliknij pozycję **Strony logowania**.
3.  Na stronie **Logowania strony integracji** kliknij **Azure Web App**.

W **integracji strony logowania: Azure Web App** strony adresu URL w **opcję 1: łącze do strony logowania hostowanej ACS** zostanie użyte w aplikacji sieci web języka Java. Po dodaniu biblioteki filtru usług kontroli dostępu Azure do aplikacji Java, należy tę wartość.

## <a name="create-a-java-web-application"></a>Tworzenie aplikacji sieci web języka Java
1. W ramach Zaćmienie w menu kliknij pozycję **plik**, kliknij pozycję **Nowy**, a następnie kliknij **Dynamiczne projektu sieci Web**. (Jeśli nie widzisz **Dynamiczne projektu sieci Web** na liście jako projekt dostępne po kliknięciu pozycji **plik**, **Nowy**, wykonaj następujące czynności: kliknij pozycję **plik**, kliknij przycisk **Nowy**, kliknij pozycję **Projekt**, rozwiń **sieci Web**, kliknij pozycję **Project Web dynamiczne**i kliknij przycisk **Dalej**.) Na potrzeby tego samouczka nazwę projektu **MyACSHelloWorld**. (Upewnij się, możesz użyć tej nazwy, kolejne kroki w tym samouczku dowiesz się spodziewać pliku też nosić nazwę MyACSHelloWorld). Na ekranie pojawią się podobny do następującego:

    ![Tworzenie projektu Witaj świecie ACS exampple][create_acs_hello_world]

    Kliknij przycisk **Zakończ**.
2. W widoku Eksplorator projektu i Zaćmienie rozwiń **MyACSHelloWorld**. Kliknij prawym przyciskiem myszy **sieć Web, zawartość**, kliknij pozycję **Nowy**, a następnie kliknij **Plik JSP**.
3. W oknie dialogowym **Nowy plik JSP** nazwę pliku **index.jsp**. Zachowaj folder nadrzędny jako MyACSHelloWorld-sieć Web, zawartość, jak pokazano poniżej:

    ![Dodawanie pliku JSP, na przykład ACS][add_jsp_file_acs]

    Kliknij przycisk **Dalej**.

4. W oknie dialogowym **Wybierz szablon JSP** wybierz pozycję **Nowy plik JSP (html)** , a następnie kliknij przycisk **Zakończ**.
5. Po otwarciu pliku index.jsp w Zaćmienie dodatek tekst do wyświetlenia **Witaj ACS świecie!** w istniejącej `<body>` elementu. Usługi zaktualizowanych `<body>` zawartości powinien być wyświetlany jako następujące czynności:

        <body>
          <b><% out.println("Hello ACS World!"); %></b>
        </body>
    
    Zapisz index.jsp.
  
## <a name="add-the-acs-filter-library-to-your-application"></a>Dodawanie biblioteki ACS filtru w aplikacji

1. W oknie Project Explorer Zaćmienie osoby kliknij prawym przyciskiem myszy **MyACSHelloWorld**, kliknij przycisk **Konstruuj ścieżkę**, a następnie kliknij **Konfigurowanie Tworzenie ścieżki**.
2. W oknie dialogowym **Ścieżka tworzenie Java** kliknij kartę **bibliotek** .
3. Kliknij przycisk **Dodaj biblioteki**.
4. Kliknij **Filtru usług kontrolki programu Access Azure (według Tech Otwórz MS)** i kliknij przycisk **Dalej**. Zostanie wyświetlone okno dialogowe **Filtr usług kontroli dostępu Azure** .  (Pole **Lokalizacja** może być ścieżki różne w zależności od tego, w którym zainstalowano Zaćmienie, a numer wersji może być inny, w zależności od aktualizacji oprogramowania).

    ![Dodawanie Biblioteka ACS filtru][add_acs_filter_lib]

5. Za pomocą przeglądarki otwarte na stronę **Integracji strony logowania** w portalu zarządzania skopiuj adres URL wymienionych w **opcję 1: łącze do strony logowania hostowanej ACS** pól i wklej je w polu **Punkt końcowy uwierzytelniania ACS** okna dialogowego Zaćmienie.
6. W przeglądarce otwarty na stronę **Edytowanie Polegaj aplikacji innych firm** do portalu zarządzania, skopiuj adres URL w polu **obszaru** na liście i wkleić go w polu **Polegaj obszaru Strona** okna dialogowego Zaćmienie.
7. W sekcji **Zabezpieczenia** w oknie dialogowym Zaćmienie Jeśli chcesz skorzystać z istniejącego certyfikatu, kliknij przycisk **Przeglądaj**, przejdź do certyfikatu, których chcesz użyć, zaznacz go i kliknij przycisk **Otwórz**. Lub, jeśli chcesz utworzyć nowy certyfikat, kliknij przycisk **Nowy** , aby wyświetlić okno dialogowe **Nowego certyfikatu** , określić hasło, nazwę pliku cer i nazwę pliku PFX dla nowego certyfikatu.
8. Sprawdź **osadzania certyfikatu w pliku też**. Osadzanie certyfikatu w ten sposób zawiera go w wdrożenia bez konieczności ręcznie dodać jako składnik. (Jeśli zamiast tego muszą być przechowywane certyfikatu zewnętrznie z pliku też, możesz dodać certyfikat jako części roli i wyczyść pole wyboru **osadzania certyfikatu w pliku też**.)
9. [Opcjonalne] Zachowaj **połączeń HTTPS wymagają** zaznaczone. Jeśli ustawisz tę opcję, musisz uzyskać dostęp do aplikacji przy użyciu protokołu HTTPS. Jeśli nie chcesz, aby wymagają połączeń HTTPS, usuń zaznaczenie tej opcji.
10. Wdrożenia do emulatora obliczeń ustawień **Filtrowania ACS Azure** będzie wyglądać podobnie do następującego.

    ![Azure ustawień filtrowania ACS do wdrożenia na emulatorze obliczeń][add_acs_filter_lib_emulator]

11. Kliknij przycisk **Zakończ**.
12. Kliknij przycisk **Tak,** gdy prezentowany z okno dialogowe pole informacją utworzony plik web.xml.
13. Kliknij **przycisk OK** , aby zamknąć okno dialogowe **Ścieżka tworzenie Java** .

## <a name="deploy-to-the-compute-emulator"></a>Wdrażanie emulatora obliczeń

1. W oknie Project Explorer Zaćmienie osoby kliknij prawym przyciskiem myszy **MyACSHelloWorld**, kliknij pozycję **Azure**, a następnie kliknij **pakiet dla Azure**.
2. W polu **Nazwa projektu**wpisz **MyAzureACSProject** , a następnie kliknij przycisk **Dalej**.
3. Wybierz pozycję JDK i serwer aplikacji. (Poniższe kroki są objęte szczegółowo samouczek [tworzenia aplikacji Witaj świecie dla Azure w Zaćmienie](http://msdn.microsoft.com/library/windowsazure/hh690944.aspx) ).
4. Kliknij przycisk **Zakończ**.
5. Kliknij przycisk **Uruchom w emulatora Azure** .
6. Po uruchomieniu aplikacji sieci web języka Java w emulatorze obliczeń, zamknij wszystkie wystąpienia programu przeglądarki (dzięki temu wszelkie bieżącej sesji przeglądarki nie przeszkadzają testem logowania ACS).
7. Uruchamianie aplikacji, otwierając <> 8080/MyACSHelloWorld/przeglądarki (lub <> https://localhost:8080/MyACSHelloWorld/zaznaczenie **połączeń wymagają HTTPS**). Powinien zostać wyświetlony monit logowania usługi Windows Live ID, a następnie możesz należy zwrotnego adresu URL określonego uzależnionej aplikacji innych firm.
99.  Po zakończeniu przeglądania aplikacji, kliknij przycisk **Resetuj emulatora Azure** .

## <a name="deploy-to-azure"></a>Wdrażanie Azure

Aby wdrożyć Azure, musisz zmienić uzależnionej obszar strony i powrócić adresu URL dla obszaru nazw ACS.

1. W portalu zarządzania Azure, na stronie **Edytowanie Polegaj aplikacji innych firm** zmodyfikuj **obszaru** jako adres URL witryny wdrożonym. Zamień **przykład** nazwy DNS wybrane do wdrożenia.

    ![Polegaj obszaru strony do użycia w produkcji][relying_party_realm_production]

2. Modyfikowanie **Zwrotny adres URL** jako adres URL aplikacji. Zamień **przykład** nazwy DNS wybrane do wdrożenia.

    ![Polegaj zwrotny adres URL strony do użycia w produkcji][relying_party_return_url_production]

3. Kliknij przycisk **Zapisz** , aby zapisać usługi zaktualizowanych odpowiedzieć obszaru strony i wrócić zmiany adresu URL.
4. Nie zamykaj strony **Integracji strony logowania** w przeglądarce, konieczne będzie wkrótce kopiowanie z niego.
5. W oknie Project Explorer Zaćmienie osoby kliknij prawym przyciskiem myszy **MyACSHelloWorld**, kliknij przycisk **Konstruuj ścieżkę**, a następnie kliknij **Konfigurowanie Tworzenie ścieżki**.
6. Kliknij kartę **biblioteki** , kliknij **Filtr usług kontroli dostępu Azure**, a następnie kliknij przycisk **Edytuj**.
7. Za pomocą przeglądarki otwarte na stronę **Integracji strony logowania** w portalu zarządzania skopiuj adres URL wymienionych w **opcję 1: łącze do strony logowania hostowanej ACS** pól i wklej je w polu **Punkt końcowy uwierzytelniania ACS** okna dialogowego Zaćmienie.
8. W przeglądarce otwarty na stronę **Edytowanie Polegaj aplikacji innych firm** do portalu zarządzania, skopiuj adres URL w polu **obszaru** na liście i wkleić go w polu **Polegaj obszaru Strona** okna dialogowego Zaćmienie.
9. W sekcji **Zabezpieczenia** w oknie dialogowym Zaćmienie Jeśli chcesz skorzystać z istniejącego certyfikatu, kliknij przycisk **Przeglądaj**, przejdź do certyfikatu, których chcesz użyć, zaznacz go i kliknij przycisk **Otwórz**. Lub, jeśli chcesz utworzyć nowy certyfikat, kliknij przycisk **Nowy** , aby wyświetlić okno dialogowe **Nowego certyfikatu** , określić hasło, nazwę pliku cer i nazwę pliku PFX dla nowego certyfikatu.
10. Zachowaj **osadzania certyfikatu w pliku też** zaznaczone, przy założeniu, który chcesz osadzić certyfikatu w pliku też.
11. [Opcjonalne] Zachowaj **połączeń HTTPS wymagają** zaznaczone. Jeśli ustawisz tę opcję, musisz uzyskać dostęp do aplikacji przy użyciu protokołu HTTPS. Jeśli nie chcesz, aby wymagają połączeń HTTPS, usuń zaznaczenie tej opcji.
12. Wdrożenia Azure ustawień filtrowania ACS Azure będzie wyglądać podobnie do następującej.

    ![Azure ustawień filtrowania ACS wdrożenia produkcji][add_acs_filter_lib_production]

13. Kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe **Edytowanie biblioteki** .
14. Kliknij **przycisk OK** , aby zamknąć okno dialogowe **Właściwości dla MyACSHelloWorld** .
15. W Zaćmienie kliknij przycisk **Publikuj w chmurze Azure** . Odpowiedz na monity, podobnie jak wykonane w sekcji **wdrażania aplikacji Azure** temat [Tworzenie aplikacji Witaj świecie dla Azure w Zaćmienie](http://msdn.microsoft.com/library/windowsazure/hh690944.aspx) . 

Po wdrożeniu aplikacji sieci web, należy zamknąć wszystkie sesje Otwieranie przeglądarki, uruchamianie aplikacji sieci web i powinien zostać wyświetlony monit o zalogowanie się przy użyciu poświadczeń usługi Windows Live ID, a po nim wysyłana do ze zwrotnym adresem URL uzależnionej aplikacji innych firm.

Po zakończeniu przy użyciu aplikacji ACS Witaj świecie, pamiętaj, aby usunąć wdrożenia (możesz dowiedzieć się, jak usunąć wdrożenia w temacie [Tworzenie aplikacji Witaj świecie dla Azure w Zaćmienie](http://msdn.microsoft.com/library/windowsazure/hh690944.aspx) ).


## <a name="next_steps"></a>Następne kroki

Do badania z zabezpieczeń potwierdzenia SAML (Markup Language) zwracane przez ACS do aplikacji zobacz [sposobu wyświetlania SAML zwracane przez usługę kontrola dostępu Azure][]. Do dalszych Eksplorowanie funkcji ACS firmy i Aby poeksperymentować z bardziej zaawansowanych scenariuszy, zobacz [2.0 usługi kontroli dostępu][].

Ponadto w tym przykładzie użyto opcji **osadzania certyfikatu w pliku też** . Ta opcja ułatwia wdrażanie certyfikatu. Jeśli zamiast tego chcesz zachować osobno z pliku też certyfikat podpisujący, można wykonać następujące techniki:

1. W sekcji **Zabezpieczenia** w oknie dialogowym **Filtr usług kontroli dostępu Azure** wpisz $ **{Koperta. JAVA_HOME}/MyCert.cer** i wyczyść pole **Embed certyfikatu w pliku też**. (Dostosuj mycert.cer, nazwy pliku certyfikatu jest inny). Kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe.
2. Kopiowanie certyfikatu składnikiem wdrożenia: Eksplorator projektu w Zaćmienie osoby, rozwiń pozycję **MyAzureACSProject**, kliknij prawym przyciskiem myszy **WorkerRole1**, kliknij pozycję **Właściwości**, rozwiń **Roli Azure**i kliknij przycisk **składniki**.
3. Kliknij przycisk **Dodaj**.
4. W oknie dialogowym **Dodawanie składników** :
    1. W sekcji **Importowanie** :
        1. Przycisk **plik** przejdź do certyfikat, który ma być używany. 
        2. **Metoda**wybierz polecenie **Kopiuj**.
    2. W polu **Nazwa**kliknij pole tekstowe, a następnie zaakceptować domyślną nazwę.
    3. W sekcji **Rozmieszczanie** :
        1. **Metoda**wybierz polecenie **Kopiuj**.
        2. **Do katalogu**wpisz **JAVA_HOME %**.
    4. Usługi okno dialogowe **Dodawanie składników** powinno wyglądać podobnie do następującego.

        ![Dodawanie składnika certyfikatu][add_cert_component]

    5. Kliknij **przycisk OK**.

W tym momencie certyfikatu może być dołączone do wdrożenia. Należy zauważyć, że niezależnie od tego, czy osadzić certyfikatu w pliku też lub dodać jako składnik do wdrożenia, należy przekazać do obszaru nazw zgodnie z opisem w sekcji [przekazywanie certyfikatu do obszaru nazw ACS][] certyfikatu.

[What is ACS?]: #what-is
[Concepts]: #concepts
[Prerequisites]: #pre
[Create a Java web application]: #create-java-app
[Create an ACS Namespace]: #create-namespace
[Add Identity Providers]: #add-IP
[Add a Relying Party Application]: #add-RP
[Create Rules]: #create-rules
[Przekazywanie certyfikatu do obszaru nazw ACS]: #upload-certificate
[Review the Application Integration Page]: #review-app-int
[Configure Trust between ACS and Your ASP.NET Web Application]: #config-trust
[Add the ACS Filter library to your application]: #add_acs_filter_library
[Deploy to the compute emulator]: #deploy_compute_emulator
[Deploy to Azure]: #deploy_azure
[Next steps]: #next_steps
[projekt witryny sieci Web]: http://wastarterkit4java.codeplex.com/releases/view/61026
[Jak wyświetlić SAML zwrócony przez usługę Azure kontroli dostępu]: /en-us/develop/java/how-to-guides/view-saml-returned-by-acs/
[Usługa sterowania programu Access 2.0]: http://go.microsoft.com/fwlink/?LinkID=212360
[Windows Identity Foundation]: http://www.microsoft.com/download/en/details.aspx?id=17331
[Windows Identity Foundation SDK]: http://www.microsoft.com/download/en/details.aspx?id=4451
[Portal Azure zarządzania]: https://manage.windowsazure.com
[acs_flow]: ./media/active-directory-java-authenticate-users-access-control-eclipse/ACSFlow.png

<!-- Eclipse-specific -->
[add_acs_filter_lib]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddACSFilterLibrary.png
[add_acs_filter_lib_emulator]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddACSFilterLibraryEmulator.png
[add_acs_filter_lib_production]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddACSFilterLibraryProduction.png

[relying_party_realm_emulator]: ./media/active-directory-java-authenticate-users-access-control-eclipse/RelyingPartyRealmEmulator.png
[relying_party_return_url_emulator]: ./media/active-directory-java-authenticate-users-access-control-eclipse/RelyingPartyReturnURLEmulator.png
[relying_party_realm_production]: ./media/active-directory-java-authenticate-users-access-control-eclipse/RelyingPartyRealmProduction.png
[relying_party_return_url_production]: ./media/active-directory-java-authenticate-users-access-control-eclipse/RelyingPartyReturnURLProduction.png
[add_cert_component]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddCertificateComponent.png
[add_jsp_file_acs]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddJSPFileACS.png
[create_acs_hello_world]: ./media/active-directory-java-authenticate-users-access-control-eclipse/CreateACSHelloWorld.png
[add_token_signing_cert]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddTokenSigningCertificate.png
 
