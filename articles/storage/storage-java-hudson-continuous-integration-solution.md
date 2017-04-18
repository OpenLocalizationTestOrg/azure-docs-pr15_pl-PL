<properties
    pageTitle="Jak używać Hudson z magazynem obiektów Blob | Microsoft Azure"
    description="W tym artykule opisano sposób użycia Hudson z magazynem obiektów Blob platformy Azure repozytorium dla artefaktów kompilacji."
    services="storage"
    documentationCenter="java"
    authors="dineshmurthy"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="10/18/2016" 
    ms.author="dinesh"/>

# <a name="using-azure-storage-with-a-hudson-continuous-integration-solution"></a>Magazyn Azure za pomocą rozwiązanie Hudson ciągły integracji

## <a name="overview"></a>Omówienie

Poniższe informacje pokazano, jak za pomocą magazyn obiektów Blob jako repozytorium artefaktów kompilacji utworzone przez rozwiązanie Hudson ciągły integracji (CI) lub jako źródło plików do pobrania można używać w procesu tworzenia. Jest jedną scenariuszy, w którym chcesz znaleźć to przydatne, gdy w przypadku kodowania w środowisku dla deweloperów adaptacyjne (za pomocą języka Java lub innych wersji językowych), kompilacjach są uruchomione oparte na ciągły integracji i potrzebujesz repozytorium dla swojego artefaktów kompilacji tak, aby mógł, na przykład udostępniać je innym członkom organizacji, a dla klientów, lub Obsługa archiwum.  Inny scenariusz jest po sobie zadania kompilacji wymaga innych plików, na przykład zależności do pobrania w ramach kompilacji wprowadzania.

W tym samouczku będzie korzystać z wtyczki Azure miejsca do magazynowania dla CI Hudson udostępnianymi przez firmę Microsoft.

## <a name="introduction-to-hudson"></a>Wprowadzenie do Hudson ##

Hudson umożliwia ciągły integracji programu project oprogramowania, umożliwiając programistom łatwe ich zmiany kodu oraz wcześniej kompilacjach dostarczać automatycznie i często, a tym samym zwiększenie wydajności pracy deweloperów. Kompilacjach są wersji i kompilacji artefakty można przekazywać do różnych repozytoria. Tym artykule opisano sposób użycia magazyn obiektów Blob platformy Azure repozytorium artefaktów kompilacji. Jak pobrać współzależności z magazynem obiektów Blob platformy Azure również będzie widoczny.

Więcej informacji na temat Hudson można znaleźć w [Hudson spotkania](http://wiki.eclipse.org/Hudson-ci/Meet_Hudson).

## <a name="benefits-of-using-the-blob-service"></a>Korzyści wynikające z używania usługi obiektów Blob ##

Korzyści wynikające z używania usługi obiektów Blob do obsługi usługi artefakty kompilacja adaptacyjne rozwoju obejmują:

- Wysoka dostępność artefakty kompilacja i/lub zależności do pobrania.
- Wydajność podczas rozwiązania Hudson CI przesłane do artefaktów kompilacji.
- Wydajność Twoi klienci i partnerzy pobierać z artefaktów kompilacji.
- Kontrolę nad zasadami dostępu użytkownika do wyboru między dostęp anonimowy, oparte na wygasania udostępnienia podpisu w programie access, prywatne, dostęp itd.

## <a name="prerequisites"></a>Wymagania wstępne ##

Będą potrzebne poniższe czynności, aby za pomocą usługi obiektów Blob z rozwiązania Hudson CI:

- Rozwiązanie Hudson integracji ciągły.

    Jeśli obecnie nie masz rozwiązanie Hudson CI, może zostać uruchomiony rozwiązanie Hudson CI przy użyciu następujące techniki:

    1. Na komputerze z obsługą języka Java Pobierz też Hudson z <http://hudson-ci.org/>.
    2. W wierszu polecenia, która została otwarta do folderu, który zawiera też Hudson Uruchom też Hudson. Jeśli na przykład został pobrany wersji 3.1.2:

        `java -jar hudson-3.1.2.war`

    3. W przeglądarce, otwórz `http://localhost:8080/`. Spowoduje to otwarcie Hudson pulpitu nawigacyjnego.

    4. Przy pierwszym użyciu Hudson, zakończenia początkowej konfiguracji w `http://localhost:8080/`.

    5. Po zakończeniu początkowej konfiguracji, anulowanie uruchomionego wystąpienia też Hudson, uruchom ponownie też Hudson i ponownie otworzyć na pulpicie nawigacyjnym Hudson `http://localhost:8080/`, służącego do instalowania i konfigurowania wtyczki magazyn Azure.

        Gdy typowe rozwiązania Hudson CI należy skonfigurować do uruchamiania jako usługa działa też Hudson w wierszu polecenia będą wystarczające dla tego samouczka.

- Konto Azure. Możesz zalogować Azure konta u <http://www.azure.com>.

- Konto Azure miejsca do magazynowania. Jeśli nie masz jeszcze konta miejsca do magazynowania, możesz utworzyć jeden, wykonując kroki na [Tworzenie konta miejsca do magazynowania](storage-create-storage-account.md#create-a-storage-account).

- Znajomość rozwiązanie Hudson CI jest zalecane, ale nie jest to wymagane, zgodnie z następującej treści użyje prosty przykład wskazujący, że czynności podczas korzystania z usługi obiektów Blob jako repozytorium dla Hudson CI tworzenie artefakty.

## <a name="how-to-use-the-blob-service-with-hudson-ci"></a>Jak używać obiektów Blob usługi z Hudson CI ##

Do korzystania z usługi obiektów Blob z Hudson, musisz zainstalować dodatek magazyn Azure, skonfigurować dodatek, aby użyć konta miejsca do magazynowania, a następnie Utwórz akcję po kompilacji przekazująca programu artefakty kompilacji do swojego konta miejsca do magazynowania. W poniższych sekcjach opisano następujące kroki.

## <a name="how-to-install-the-azure-storage-plugin"></a>Jak zainstalować dodatek magazyn Azure ##

1. W obrębie pulpitu nawigacyjnego Hudson kliknij pozycję **Zarządzanie Hudson**.
2. Na stronie **Zarządzanie Hudson** kliknij pozycję **Zarządzaj wtyczki**.
3. Kliknij kartę **dostępne** .
4. Kliknij **inne osoby**.
5. W sekcji **Uploaders artefaktu** wybierz **dodatek Microsoft Azure miejsca do magazynowania**.
6. Kliknij przycisk **Zainstaluj**.
7. Po zakończeniu instalacji ponownie uruchom Hudson.

## <a name="how-to-configure-the-azure-storage-plugin-to-use-your-storage-account"></a>Jak skonfigurować dodatek magazyn Azure do przy użyciu konta miejsca do magazynowania ##

1. W obrębie pulpitu nawigacyjnego Hudson kliknij pozycję **Zarządzanie Hudson**.
2. Na stronie **Zarządzanie Hudson** kliknij pozycję **Konfigurowanie System**.
3. W sekcji **Konfiguracja konta Microsoft Azure miejsca do magazynowania** :

    . Wprowadź nazwę konta magazynu można uzyskać z [Azure Portal](https://portal.azure.com).

    b. Wprowadź klucz konta miejsca do magazynowania również częściowa z [Azure Portal](https://portal.azure.com).

    c. Użyj wartości domyślnej dla **Adresu URL punktu końcowego usługi obiektów Blob** , jeśli korzystasz z publicznej chmura Azure. Jeśli korzystasz z różnych chmura Azure za pomocą punktu końcowego zgodnie z [Azure Portal](https://portal.azure.com) dla Twojego konta miejsca do magazynowania.

    d. Kliknij przycisk **Sprawdź poprawność magazynu poświadczeń** w celu sprawdzenia poprawności konta miejsca do magazynowania.

    e. [Opcjonalne] Jeśli masz kont dodatkowego miejsca do magazynowania, które mają być udostępniane usługi CI Hudson, kliknij przycisk **Dodaj więcej miejsca do magazynowania kont**.

    f. Kliknij przycisk **Zapisz** , aby zapisać ustawienia.

## <a name="how-to-create-a-post-build-action-that-uploads-your-build-artifacts-to-your-storage-account"></a>Jak utworzyć akcję po kompilacji przekazująca programu artefakty kompilacja do swojego konta miejsca do magazynowania ##

Najpierw na potrzeby instrukcji będzie należy utworzyć zadanie, Utwórz kilka plików, a następnie dodaj w działaniu po kompilacji przekazywać pliki do konta miejsca do magazynowania.

1. W obrębie pulpitu nawigacyjnego Hudson kliknij przycisk **Nowe zadanie**.
2. Nadaj nazwę zadaniu **MyJob**, kliknij przycisk **Konstruuj zadanie oprogramowania wolny styl**, a następnie kliknij **przycisk OK**.
3. W sekcji **Tworzenie** konfiguracji zadań kliknij przycisk **Dodaj Konstruuj kroku** i wybierz **polecenie partii wykonaj systemu Windows**.
4. **Polecenie**należy użyć następujących poleceń:

        md text
        cd text
        echo Hello Azure Storage from Hudson > hello.txt
        date /t > date.txt
        time /t >> date.txt

5. W sekcji **Akcje po kompilacji** konfiguracji zadań kliknij przycisk **Przekaż artefakty magazynem obiektów Blob platformy Azure firmy Microsoft**.
6. W polu **Nazwę konta magazynu**wybierz konto miejsca do magazynowania do użycia.
7. W polu **Nazwa kontenera**Określ nazwę kontenera. (Kontener zostanie utworzony, jeśli już istnieje po przekazaniu są artefakty kompilacji.) Możesz używać zmiennych środowiska, więc w tym przykładzie wprowadź wartość **${JOB_NAME}** jako nazwa kontenera.

    **Porada**

    **Polecenie** sekcji, gdzie wprowadzono skrypt dla **Windows wykonywanie partii polecenie** Oto łącze do zmiennych środowiska rozpoznawanych Hudson. Kliknij pozycję tego łącza, aby dowiedzieć się, w nazwach zmiennych środowiska i opisy. Uwaga zmiennych środowiska, które zawierają znaki specjalne, takie jak zmienną środowiska **BUILD_URL** nie są dozwolone jako nazwa kontenera lub typowe ścieżki wirtualnej.

8. W tym przykładzie kliknij przycisk **Udostępnij publicznie domyślnie nowego kontenera** . (Jeśli chcesz użyć Kontener prywatny, musisz utworzyć podpis udostępniania umożliwiają dostęp. Który wykracza poza zakres tego artykułu. Przedstawiono więcej informacji na temat udostępniania podpisy na [Przy użyciu podpisów dostępu do udostępnionej (SA)](storage-dotnet-shared-access-signature-part-1.md).)
9. [Opcjonalne] Kliknij przycisk **Oczyść kontener przed przekazaniem** kontenerze wyczyszczone zawartości przed artefakty kompilacji są przekazywane (nie zaznaczaj jej wyboru, jeśli nie chcesz oczyścić zawartość pojemnika).
10. **Lista artefaktów, aby przekazać**wprowadź * *tekstu i*txt**.
11. **Typowe ścieżkę wirtualną przekazane artefakty**, wprowadź **${Tworzenie\_identyfikator}-${tworzenie\_numer}**.
12. Kliknij przycisk **Zapisz** , aby zapisać ustawienia.
13. Na pulpicie nawigacyjnym Hudson kliknij przycisk **Konstruuj teraz** przeprowadzić **MyJob**. Przejrzyj wyniki konsoli stanu. Komunikaty o stanie do przechowywania Azure są uwzględniane w wyniku konsoli podczas uruchamiania akcji po kompilacji do przekazania artefakty kompilacji.
14. Od pomyślnego zakończenia zadania można sprawdzić artefakty kompilacji, otwierając publicznej obiektów blob.

    . Zaloguj się do [portalu Azure](https://portal.azure.com).

    b. Kliknij pozycję **Magazyn**.

    c. Kliknij nazwę konta magazynu używanej Hudson.

    d. Kliknij pozycję **kontenerów**.

    e. Kliknij kontener o nazwie **myjob**, czyli małe wersji przypisany podczas tworzenia zadania Hudson Nazwa zadania. Kontener nazw i nazw obiektów blob są małymi literami (i uwzględniania wielkości liter) w magazynie Azure. Na liście obiektów blob kontenera o nazwie **myjob** jest wyświetlany, **hello.txt** i **date.txt**. Skopiuj adres URL dla dowolnego z tych elementów, a następnie otwórz go w przeglądarce. Plik tekstowy, który został przekazany zostanie wyświetlona jako artefaktu kompilacji.

Dla zadania można utworzyć tylko jedną akcję po kompilacji przekazująca artefakty magazynem obiektów Blob platformy Azure. Należy zauważyć, że pojedyncza akcja po kompilacji do przekazania artefakty magazynem obiektów Blob platformy Azure można określić różnych plików (łącznie z symboli wieloznacznych) oraz ścieżki do plików w obrębie **Listy artefakty, aby przekazać** przy użyciu średnikiem jako separator. Na przykład jeśli Twoją kompilację Hudson tworzy pliki JAR i TXT w folderze **Tworzenie** obszaru roboczego, a chcesz przekazać zarówno z magazynem obiektów Blob platformy Azure, użyj następującego wartość **Listy artefakty przekazywania** : **Tworzenie-\*.jar; tworzenie-\*txt**. Za pomocą składni podwójnego dwukropka Określ ścieżkę, aby używać wewnątrz nazwy obiektów blob. Na przykład jeśli chcesz słoików, aby uzyskać przekazane uzyskiwanie przekazane za pomocą **powiadomień** w ścieżce obiektów blob przy użyciu **plików binarnych** w ścieżce obiektów blob i pliki TXT, użyj następującego wartość **Listy artefakty, aby przekazać** : **Tworzenie-\*. jar::binaries; tworzenie-\*. txt::notices**.

## <a name="how-to-create-a-build-step-that-downloads-from-azure-blob-storage"></a>Jak utworzyć krok kompilacji, który pobiera z magazynem obiektów Blob platformy Azure ##

Poniższe kroki pokazują sposób konfigurowania kroku kompilacji do pobierania elementów z magazynem obiektów Blob platformy Azure. Może to być przydatne, jeśli chcesz uwzględnić elementy w swojej konstrukcji, na przykład słoików, które przechowujesz w magazynie obiektów Blob platformy Azure.

1. W sekcji **Tworzenie** konfiguracji zadań kliknij przycisk **Dodaj Konstruuj kroku** i wybierz pozycję **Pobierz z magazynem obiektów Blob platformy Azure**.
2. W polu **nazwę konta magazynu**wybierz konto miejsca do magazynowania do użycia.
3. W polu **Nazwa kontenera**Określ nazwę kontenera, który ma blob, który chcesz pobrać. Można używać zmiennych środowiska.
4. **Nazwy obiektów Blob**Określ nazwę obiektów blob. Można używać zmiennych środowiska. Ponadto można je znakiem gwiazdki jako symbol wieloznaczny po określeniu początkowe litery nazwy obiektów blob. Na przykład **projektu\* ** określa wszystkie obiekty BLOB, których nazwy zaczynają się z **programem project**.
5. [Opcjonalne] **Pobierz ścieżkę**Określ ścieżkę na komputerze Hudson, w którym chcesz pobierać pliki z magazynem obiektów Blob platformy Azure. Można także zmiennych środowiska. (Jeśli nie zostanie określona wartość dla **Pobierz ścieżkę**, pliki z magazynem obiektów Blob platformy Azure będą pobierane do obszaru roboczego zadania.)

Jeśli masz dodatkowe elementy, które mają zostać pobrane z magazynem obiektów Blob platformy Azure, możesz utworzyć tworzenie dodatkowych czynności.

Po uruchomieniu kompilacji, możesz sprawdzić wynik konsoli Historia kompilacji lub Spójrz na swojej lokalizacji pobierania, aby sprawdzić, czy pomyślnie pobranych obiektów blob oczekiwana.

## <a name="components-used-by-the-blob-service"></a>Składniki używane przez usługę obiektów Blob ##

Poniżej omówiono składniki usługi obiektów Blob.

- **Konto magazynu**: dostęp do magazynowania Azure odbywa się przy użyciu konta przestrzeni dyskowej. To jest najwyższego poziomu w obszarze nazw uzyskiwanie dostępu do obiektów blob. Konta może zawierać dowolną liczbę kontenery, dopóki ich całkowity rozmiar jest w obszarze 100 TB.
- **Kontener**: kontenera zawiera grupę zestawu obiektów blob. Wszystkie obiekty BLOB muszą znajdować się w kontenerze. Konta może zawierać dowolną liczbę kontenerów. Kontener mogą zawierać dowolną liczbę obiektów blob.
- **Obiektów blob**: plik dowolnego typu i rozmiar. Istnieją dwa typy obiektów blob, które mogą być przechowywane w magazynie Azure: blob blok i strony. Większość plików są blob blok. Blob pojedynczy blok może zawierać maksymalnie 200 GB. Samouczku blob blok. Strona blob, innego typu obiektów blob, można do 1 TB rozmiary i są bardziej efektywne modyfikacji zakresów bajtów w pliku są często. Aby uzyskać więcej informacji na temat obiektów blob zobacz [opis bloku blob, dołączanie obiektów blob i blob strony](http://msdn.microsoft.com/library/azure/ee691964.aspx).
- **Format adresu URL**: obiektów blob są adresach w następującym formacie adresu URL:

    `http://storageaccount.blob.core.windows.net/container_name/blob_name`

    (Format powyżej dotyczy publicznej chmura Azure. Jeśli korzystasz z różnych chmura Azure umożliwia punkt końcowy w [Azure Portal](https://portal.azure.com) określić punkt końcowy adresu URL.)

    W formacie powyżej `storageaccount` reprezentuje nazwę konta magazynu `container_name` odpowiada nazwie do kontenera i `blob_name` odpowiednio oznacza nazwę Twojej blob. W nazwie kontenera może mieć wiele ścieżek oddzielone znakiem ukośnika, **/**. Przykładowa nazwa kontenera w tym samouczku została **MyJob**i **${Tworzenie\_identyfikator}-${tworzenie\_numer}** użyto typowe ścieżki wirtualnej, uzyskując blob o adresu URL następującą postać:

    `http://example.blob.core.windows.net/myjob/2014-05-01_11-56-22/1/hello.txt`

## <a name="next-steps"></a>Następne kroki

- [Spełniają Hudson](http://wiki.eclipse.org/Hudson-ci/Meet_Hudson)
- [Azure miejsca do magazynowania SDK dla języka Java](https://github.com/azure/azure-storage-java)
- [Odwołanie SDK klienta Azure miejsca do magazynowania](http://dl.windowsazure.com/storage/javadoc/)
- [Usługi Azure magazyn interfejsu API usługi REST](https://msdn.microsoft.com/library/azure/dd179355.aspx )
- [Blog zespołu Azure miejsca do magazynowania](http://blogs.msdn.com/b/windowsazurestorage/)

Aby uzyskać więcej informacji zobacz też [Centrum deweloperów języka Java](https://azure.microsoft.com/develop/java/).