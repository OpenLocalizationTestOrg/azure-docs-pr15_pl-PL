<properties
    pageTitle="Zarządzanie współbieżności w magazynie platformy Microsoft Azure"
    description="Jak zarządzać współbieżności dla usług obiektów Blob, kolejki tabeli i plików"
    services="storage"
    documentationCenter=""
    authors="jasonnewyork"
    manager="tadb"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="jahogg"/>

# <a name="managing-concurrency-in-microsoft-azure-storage"></a>Zarządzanie współbieżności w magazynie platformy Microsoft Azure

## <a name="overview"></a>Omówienie

Nowoczesne aplikacje internetowe podstawie zazwyczaj dysponują wielu użytkownikom wyświetlanie i aktualizowanie danych jednocześnie. W tym celu deweloperów aplikacji uważnie zastanowić, jak zapewnić przewidywalne obsługi dla użytkowników końcowych, szczególnie w przypadku scenariuszy, w którym wielu użytkowników można aktualizować te same dane. Istnieją trzy główne dane Strategie współbieżności, które deweloperów będzie zwykle należy rozważyć, czy:  


1.  Optymistyczny współbieżności — ostatnia wykonywania aplikacji które aktualizację w ramach jego aktualizacji sprawdzisz, jeśli dane zostały zmienione od aplikacji odczytywania danych. Na przykład jeśli dwóch użytkowników przeglądających strony typu wiki aktualizację do tej samej strony typu wiki platformy musi zapewnić drugiej aktualizacji nie zastępuje pierwszej aktualizacji — i zarówno użytkownikom zrozumieć, czy ich aktualizacji zakończyło się pomyślnie. Strategia ta jest najczęściej używana w aplikacjach sieci web.
2.  Pesymistyczny współbieżności — aplikacji chcą przeprowadzić aktualizację potrwa blokada obiektu zaktualizować dane do momentu zwolnieniu blokady możesz uniemożliwić innym użytkownikom. Na przykład w scenariuszu replikacji danych wzorca/podrzędny, gdzie tylko wzorcu wykona aktualizacje wzorcu zwykle chcesz wprowadzić w trybie wyłączności przez dłuższy czas na karcie dane, aby upewnić się, że nikt je zaktualizować.
3.  Ostatni wins writer — metody, która zezwala na wszystkie operacje aktualizacji kontynuować bez sprawdzania, jeśli dowolnej innej aplikacji zaktualizował danych od aplikacji najpierw odczytać danych. Ten strategii (lub brak strategii formalne) jest zazwyczaj używany, gdzie danych jest podzielona w taki sposób, że istnieje nie prawdopodobieństwo, że wielu użytkowników będzie uzyskać dostęp do tych samych danych. Również przydatne może być miejsce, w którym są przetwarzane strumienie krótkotrwałe danych.  

W tym artykule omówiono sposób platformy Azure magazynowania ułatwia rozwoju, zapewniając pierwszej klasie pomocy technicznej dla wszystkich trzech Strategie współbieżności.  

## <a name="azure-storage--simplifies-cloud-development"></a>Azure magazynowania — upraszcza tworzenie chmury
Usługa Azure magazynu obsługuje wszystkie trzy strategii, mimo że jest charakterystyczne w jej możliwość wyświetlania pełnej obsługi dla współbieżności optymistycznych i Pesymistyczny, ponieważ został on zaprojektowany obejmuje modelu silnych spójności, który gwarantuje, że Usługa magazynu zatwierdza Wstawianie danych lub wszystkich operacji aktualizacji dodatkowo uzyskuje dostęp do do że dane zostaną wyświetlone najnowszą aktualizację. Platformy miejsca do magazynowania, korzystające z modelu ewentualne spójności masz czasu zwłoki między zapisu jest wykonywane przez użytkowników i zaktualizowane dane są widoczne dla innych użytkowników, w ten sposób komplikując opracowywania aplikacji klienckich, aby zapobiec niespójności wpływające na użytkowników końcowych.  

Oprócz wybranie strategii współbieżności odpowiednie deweloperów również powinni wiedzieć jak platformy miejsca do magazynowania izoluje zmiany — zwłaszcza zmiany do tego samego obiektu przez transakcje. Usługa Azure magazynu używa izolacji migawki umożliwia operacji odczytu nastąpić równocześnie operacji zapisu w jedną partycją. W przeciwieństwie do innych poziomów izolacji izolacji migawki gwarantuje, że wszystkie operacje odczytu Zobacz spójne migawkę danych, nawet wtedy, gdy występuje aktualizacje — zasadniczo przywracając ostatnie wartości zatwierdzony podczas aktualizacji transakcja jest przetwarzana.  

## <a name="managing-concurrency-in-blob-storage"></a>Zarządzanie współbieżności w magazynie obiektów Blob
Możesz wybrać opcję umożliwia zarządzanie dostępem do obiektów blob i kontenerów w usłudze obiektów blob albo modeli optymistyczny lub pesymistyczny współbieżności. Jeśli nie określisz jawnie strategii ostatniego zapisu wins to ustawienie domyślne.  

### <a name="optimistic-concurrency-for-blobs-and-containers"></a>Optymistyczny współbieżności dla obiektów blob i kontenerów  

Usługa magazynu przypisuje identyfikator każdy obiekt przechowywane. Ten identyfikator jest aktualizowane po każdej aktualizacji jest wykonywane na obiekt. Identyfikator są zwracane do klienta jako część odpowiedzi HTTP GET przy użyciu nagłówek (jednostki znacznik) ETag zdefiniowaną protokołu HTTP. Wykonanie uaktualnienia na taki obiekt użytkownika można wysłać w oryginalnej ETag wraz z nagłówkiem warunkowe do upewnij się, że aktualizacja tylko wówczas, gdy została osiągnięta warunkowe — w tym przypadku warunek jest nagłówkiem "Jeżeli dopasowania", która wymaga usługa magazynu upewnić się, że wartość ETag określonego w żądania aktualizacji jest taki sam jak przechowywane w usłudze miejsca do magazynowania.  

Konspekt ten proces jest następująca:  

1.  Pobieranie z usługi Magazyn obiektów blob, odpowiedź zawiera wartość nagłówek ETag HTTP, która określa bieżącą wersję obiektu w usłudze miejsca do magazynowania.
2.  Po zaktualizowaniu obiektów blob zawierają wartość ETag otrzymany w kroku 1 w nagłówku warunkowe **Dopasowanie Jeżeli** żądanie, które możesz wysłać do usługi.
3.  Usługa porównuje wartość ETag w wezwaniu z bieżącą wartość ETag obiektów blob.
4.  Jeśli bieżącą wartość ETag to jest wersję inną niż ETag w nagłówku warunkowe **Jeżeli dopasowania** w wezwaniu, usługa zwraca 412 błędu do klienta. Ta wartość oznacza klientowi inny proces zaktualizował to od klienta pobrany.
5.  Bieżącą wartość ETag to w przypadku wersji zgodnej z wersją ETag w nagłówku warunkowe **Jeżeli dopasowania** w wezwaniu, usługa wykonuje żądaną operację i aktualizuje bieżącą wartość ETag obiektów blob, aby pokazać, że został utworzony nowej wersji.  

Poniższy fragment C# (za pomocą biblioteki magazynowania klienta 4.2.0) pokazuje prosty przykład sposobu tworzenia **AccessCondition dopasowanie Jeżeli** na podstawie ETag wartości, który jest dostępny we właściwościach obiektów blob, który został wcześniej pobranych lub wstawione. Następnie używa obiektu **AccessCondition** po jej aktualizowanie to: obiekt **AccessCondition** dodaje nagłówek **Jeżeli dopasowanie** do wezwania. Jeśli inny proces aktualizacji to usługa obiektów blob zwraca komunikat o stanie 412 HTTP (nie można wstępny). Możesz pobrać pełny przykładowe tutaj: [Zarządzanie współbieżności przy użyciu magazynu Azure](http://code.msdn.microsoft.com/Managing-Concurrency-using-56018114).  

    // Retrieve the ETag from the newly created blob
    // Etag is already populated as UploadText should cause a PUT Blob call
    // to storage blob service which returns the etag in response.
    string orignalETag = blockBlob.Properties.ETag;

    // This code simulates an update by a third party.
    string helloText = "Blob updated by a third party.";

    // No etag, provided so orignal blob is overwritten (thus generating a new etag)
    blockBlob.UploadText(helloText);
    Console.WriteLine("Blob updated. Updated ETag = {0}",
    blockBlob.Properties.ETag);

    // Now try to update the blob using the orignal ETag provided when the blob was created
    try
    {
        Console.WriteLine("Trying to update blob using orignal etag to generate if-match access condition");
        blockBlob.UploadText(helloText,accessCondition:
        AccessCondition.GenerateIfMatchCondition(orignalETag));
    }
    catch (StorageException ex)
    {
        if (ex.RequestInformation.HttpStatusCode == (int)HttpStatusCode.PreconditionFailed)
        {
            Console.WriteLine("Precondition failure as expected. Blob's orignal etag no longer matches");
            // TODO: client can decide on how it wants to handle the 3rd party updated content.
        }
        else
            throw;
    }  

Usługa magazynu obsługuje również dodatkowe nagłówki warunkowe takich jak **Jeżeli zmodyfikowany od**, **Jeżeli za w niezmienionej postaci od** i **Jeżeli żaden-dopasowania** , a także ich kombinacje. Aby uzyskać więcej informacji zobacz [Określanie warunkowe nagłówki działania usługi obiektów Blob](http://msdn.microsoft.com/library/azure/dd179371.aspx) w witrynie MSDN.  

W poniższej tabeli podsumowano operacji kontenera, które przyjmują warunkowe nagłówków, takich jak **Jeżeli dopasowania** w wezwaniu na i które zwracają wartość ETag w odpowiedzi.  

| Operacja                | Zwraca wartość ETag kontenera | Akceptuje warunkowe nagłówków |
|:-------------------------|:-----------------------------|:----------------------------|
| Tworzenie kontenera         | Tak                          | Brak                          |
| Uzyskiwanie właściwości kontenera | Tak                          | Brak                          |
| Pobieranie metadanych kontenera   | Tak                          | Brak                          |
| Ustawianie kontenera metadanych   | Tak                          | Tak                         |
| Uzyskiwanie ACL kontenera        | Tak                          | Brak                          |
| Ustawianie ACL kontenera        | Tak                          | Tak (*)                     |
| Usuń kontener         | Brak                           | Tak                         |
| Kontener dzierżawy          | Tak                          | Tak                         |
| Lista obiektów blob               | Brak                           | Brak                          |

(*) Uprawnienia zdefiniowane przez SetContainerACL w pamięci podręcznej i aktualizacji tych uprawnień potrwać 30 sekund propagowanie aktualizacji nie są w tym okresie gwarancji zachowanie spójności.  

W poniższej tabeli podsumowano operacji obiektów blob, które przyjmują warunkowe nagłówków, takich jak **Jeżeli dopasowania** w wezwaniu na i które zwracają wartość ETag w odpowiedzi.

| Operacja           | Zwraca wartość ETag | Akceptuje warunkowe nagłówków           |
|:--------------------|:-------------------|:--------------------------------------|
| Umieszczanie obiektów Blob            | Tak                | Tak                                   |
| Uzyskiwanie obiektów Blob            | Tak                | Tak                                   |
| Pobierz właściwości obiektów Blob | Tak                | Tak                                   |
| Ustawianie właściwości obiektów Blob | Tak                | Tak                                   |
| Pobieranie metadanych obiektów Blob   | Tak                | Tak                                   |
| Ustawianie metadanych obiektów Blob   | Tak                | Tak                                   |
| Obiektów Blob dzierżawy (*)      | Tak                | Tak                                   |
| Migawka obiektów Blob       | Tak                | Tak                                   |
| Kopiowanie obiektów Blob           | Tak                | Tak (w przypadku blob źródłowa i docelowa) |
| Przerwać kopiowanie obiektów Blob     | Brak                 | Brak                                    |
| Usuwanie obiektów Blob         | Brak                 | Tak                                   |
| Umieszczenie bloku           | Brak                 | Brak                                    |
| Umieszczanie Lista bloków      | Tak                | Tak                                   |
| Uzyskiwanie listy bloków      | Tak                | Brak                                    |
| Umieszczenie strony            | Tak                | Tak                                   |
| Pobieranie strony zakresów     | Tak                | Tak                                   |


(*) Blob dzierżawy nie zmienia się ETag na obiektów blob.  

### <a name="pessimistic-concurrency-for-blobs"></a>Pesymistyczny współbieżności dla obiektów blob
Aby zablokować obiektów blob w trybie wyłączności, mogą uzyskiwać [dzierżawy](http://msdn.microsoft.com/library/azure/ee691972.aspx) nad nim. Po uzyskaniu dzierżawę Określa jak długo należy dzierżawy: może to być dla od 15 do 60 sekund lub nieskończonej który wynosi w trybie wyłączności. Możesz odnowić dzierżawę ograniczone, aby rozszerzyć go, a może Zwolnij dowolny dzierżawy, po zakończeniu pracy nad nim. Usługa obiektów blob automatycznie udostępnia dzierżawy ograniczone po ich wygaśnięciu.  

Dzierżawy Włączanie synchronizacji różnych strategii obsługi, łącznie z wyłącznością zapisu / shared Odczyt, wyłączności zapisu / wyłącznego odczytu i zapisu udostępnione / odczyt wyłączności. W przypadku, gdy istnieje dzierżawę Usługa magazynu wymusza wyłączności zapisu (umieszczenie, ustawianie i usuwanie operacji) jednak zapewnienie wyłączności operacji odczytu wymaga projektanta upewnić się, że wszystkie aplikacje klienckie za pomocą Identyfikatora dzierżawy i że tylko jeden klient naraz ma identyfikator prawidłowa dzierżawa. W tym artykule operacji, które nie zawierają wyniku identyfikator dzierżawy w udostępnionej Odczyt.  

Poniższy fragment C# przedstawiono przykład uzyskiwanie wyłączności dzierżawy przez 30 sekund na obiektów blob, aktualizacji zawartości obiektów blob, a następnie zwolnienie dzierżawy. Jeśli jest już prawidłowa dzierżawa na to podczas próby uzyskać nową dzierżawę, usługa obiektów blob zwraca wynik stan "Konflikt HTTP (409)". Poniżej wstawek używa obiektu **AccessCondition** w celu umieszczać informacji o dzierżawie podczas wykonywania prośby o aktualizację obiektów blob usługi przestrzeni dyskowej.  Możesz pobrać pełny przykładowe tutaj: [Zarządzanie współbieżności przy użyciu magazynu Azure](http://code.msdn.microsoft.com/Managing-Concurrency-using-56018114).

    // Acquire lease for 15 seconds
    string lease = blockBlob.AcquireLease(TimeSpan.FromSeconds(15), null);
    Console.WriteLine("Blob lease acquired. Lease = {0}", lease);

    // Update blob using lease. This operation will succeed
    const string helloText = "Blob updated";
    var accessCondition = AccessCondition.GenerateLeaseCondition(lease);
    blockBlob.UploadText(helloText, accessCondition: accessCondition);
    Console.WriteLine("Blob updated using an exclusive lease");

    //Simulate third party update to blob without lease
    try
    {
        // Below operation will fail as no valid lease provided
        Console.WriteLine("Trying to update blob without valid lease");
        blockBlob.UploadText("Update without lease, will fail");
    }
    catch (StorageException ex)
    {
        if (ex.RequestInformation.HttpStatusCode == (int)HttpStatusCode.PreconditionFailed)
            Console.WriteLine("Precondition failure as expected. Blob's lease does not match");
        else
            throw;
    }  

Przy próbie operacji zapisu na dzierżawy obiektów blob bez przejścia identyfikator dzierżawy żądania zakończy się niepowodzeniem z błędem 412. Należy zauważyć, że jeśli dzierżawa wygasa przed wywoływanie metody **UploadText** , ale nadal przekazać identyfikator dzierżawy, żądanie również zakończy się niepowodzeniem z błędem **412** . Aby uzyskać więcej informacji o zarządzaniu czas wygaśnięcia dzierżawy i identyfikatory dzierżawy zapoznaj się z dokumentacją pozostałych [Obiektów Blob dzierżawy](http://msdn.microsoft.com/library/azure/ee691972.aspx) .  

Następujące operacje obiektów blob umożliwia zarządzanie pesymistyczny współbieżności dzierżawy:  


-   Umieszczanie obiektów Blob
-   Uzyskiwanie obiektów Blob
-   Pobierz właściwości obiektów Blob
-   Ustawianie właściwości obiektów Blob
-   Pobieranie metadanych obiektów Blob
-   Ustawianie metadanych obiektów Blob
-   Usuwanie obiektów Blob
-   Umieszczenie bloku
-   Umieszczenie Lista bloków
-   Uzyskiwanie listy bloków
-   Umieszczenie strony
-   Pobieranie strony zakresów
-   Migawka obiektów Blob — opcjonalnie, jeśli istnieje dzierżawę identyfikator dzierżawy
-   Kopiowanie obiektów Blob - identyfikator dzierżawy wymagane, jeśli dzierżawa znajduje się na miejsce docelowe obiektów blob
-   Przerwać kopiowanie obiektów Blob — dzierżawy identyfikator wymaganego nieskończonej dzierżawy istnieje w docelowej obiektów blob
-   Blob dzierżawy  

### <a name="pessimistic-concurrency-for-containers"></a>Pesymistyczny współbieżności dla kontenerów
Dzierżawy na kontenerów włączyć sam strategii synchronizacji w celu obsługi w obiektów blob (wyłączności pisanie / shared Odczyt, wyłączności zapisu / wyłącznego odczytu i zapisu udostępnione / odczyt wyłączności) jednak w przeciwieństwie do obiektów blob Usługa magazynu tylko wymusza wyłączności w operacji usuwania. Aby usunąć kontenera aktywne dzierżawy, klient musi zawierać identyfikator aktywnej dzierżawy z żądanie usunięcia. Wszystkie operacje kontenera powiodła się w kontenerze dzierżawy bez łącznie z Identyfikatorem dzierżawy w wyniku czego są udostępniane operacji. Jeśli wymagane jest wyłączności aktualizacji (położenie lub zestaw) lub operacji odczytu następnie deweloperów należy upewnić się, że wszystkich klientów za pomocą Identyfikatora dzierżawy i że tylko jeden klient naraz ma identyfikator prawidłowa dzierżawa.  

Następujące operacje kontenera umożliwia zarządzanie pesymistyczny współbieżności dzierżawy:  

-   Usuń kontener
-   Uzyskiwanie właściwości kontenera
-   Pobieranie metadanych kontenera
-   Ustawianie kontenera metadanych
-   Uzyskiwanie ACL kontenera
-   Ustawianie ACL kontenera
-   Kontener dzierżawy  

Aby uzyskać więcej informacji, zobacz:  

- [Określanie nagłówków warunkowego dla obiektów Blob działania usługi](http://msdn.microsoft.com/library/azure/dd179371.aspx)
- [Kontener dzierżawy](http://msdn.microsoft.com/library/azure/jj159103.aspx)
- [Blob dzierżawy](http://msdn.microsoft.com/library/azure/ee691972.aspx)

## <a name="managing-concurrency-in-the-table-service"></a>Zarządzanie współbieżności w usłudze tabeli
Usługa tabeli używa oznacza, że współbieżności sprawdza jako domyślne zachowanie podczas pracy z obiektami w przeciwieństwie do usługi obiektów blob, którym jawnie należy wybrać do sprawdzania współbieżności optymistyczny. Inne różnicę między tabeli i obiektów blob usługi to, że można tylko zarządzać zachowanie współbieżności jednostek należy z usługą obiektów blob możesz zarządzać współbieżności kontenerów i obiektów blob.  

Aby użyć optymistyczny współbieżności i sprawdź, czy inny proces zmodyfikowany jednostki, ponieważ został pobrany z usługi Magazyn tabel, służy wartość ETag otrzymaną po usłudze tabeli zwraca jednostkę. Konspekt ten proces jest następująca:  

1.  Pobieranie jednostka z usługi Magazyn tabel, odpowiedź zawiera wartość ETag, która określa bieżącego identyfikatora skojarzone z tego obiektu w usłudze miejsca do magazynowania.
2.  Po zaktualizowaniu jednostki zawierają wartość ETag otrzymany w kroku 1 w nagłówku **Dopasowanie Jeżeli** obowiązkowe żądanie, które możesz wysłać do usługi.
3.  Usługa porównuje wartość ETag w wezwaniu z bieżącą wartość ETag jednostki.
4.  Bieżącą wartość ETag jednostki jest inny niż ETag w nagłówku **Dopasowanie Jeżeli** obowiązkowe w wezwaniu, usługa zwraca błąd 412 do klienta. Ta wartość oznacza klientowi inny proces zaktualizował jednostki od klienta pobrany.
5.  Jeśli bieżącą wartość ETag jednostki jest taka sama jak ETag w nagłówku **Dopasowanie Jeżeli** obowiązkowe w wezwaniu lub nagłówek **Jeżeli dopasowania** zawiera symbol wieloznaczny (*), usługa wykonuje żądaną operację i aktualizuje bieżącą wartość ETag jednostki, aby pokazać, że zostały zaktualizowane.  

Uwaga w przeciwieństwie do usługi obiektów blob usługi tabel wymaga klienta objąć nagłówkiem **Dopasowanie Jeżeli** żądania aktualizacji. Jednak istnieje możliwość wymusić bezwarunkowe aktualizacji (ostatni writer wins strategii) i pominięcia sprawdzania współbieżności, jeżeli klienta ustawia nagłówka **Dopasowanie Jeżeli** wieloznaczny (*) w wezwaniu.  

Poniższy fragment C# zawiera jednostki klienta, która wcześniej została utworzona lub pobrane po jego adres e-mail zaktualizowane. Wstępne Wstawianie lub pobrać sklepy operacji wartość ETag w obiekcie klienta, a ponieważ próbce używa tego samego wystąpienia obiektu podczas wykonywania operacji Zamień, wysyła automatycznie wartość ETag ponownie do usługi tabeli włączania usługi w celu wyszukania naruszenia współbieżności. Jeśli inny proces został zaktualizowany jednostki w magazynie tabel, usługa zwraca komunikat o stanie 412 HTTP (nie można wstępny).  Możesz pobrać pełny przykładowe tutaj: [Zarządzanie współbieżności przy użyciu magazynu Azure](http://code.msdn.microsoft.com/Managing-Concurrency-using-56018114).

    try
    {
        customer.Email = "updatedEmail@contoso.org";
        TableOperation replaceCustomer = TableOperation.Replace(customer);
        customerTable.Execute(replaceCustomer);
        Console.WriteLine("Replace operation succeeded.");
    }
    catch (StorageException ex)
    {
        if (ex.RequestInformation.HttpStatusCode == 412)
            Console.WriteLine("Optimistic concurrency violation – entity has changed since it was retrieved.");
        else
            throw;
    }  

Aby wyłączyć jawnie wyboru współbieżności, należy ustawić właściwość **ETag** obiektu **pracownika** do "*" przed wykonaniem operacji Zamień.  

    customer.ETag = "*";  

W poniższej tabeli przedstawiono, jak operacje jednostki tabeli za pomocą ETag wartości:

| Operacja                | Zwraca wartość ETag | Wymaga nagłówka żądania Jeżeli dopasowania |
|:-------------------------|:-------------------|:---------------------------------|
| Jednostki kwerendy           | Tak                | Brak                               |
| Wstawianie obiektu            | Tak                | Brak                               |
| Aktualizowanie obiektu            | Tak                | Tak                              |
| Scalanie jednostek             | Tak                | Tak                              |
| Usuwanie obiektu            | Brak                 | Tak                              |
| Wstawianie lub zamienianie jednostki | Tak                | Brak                               |
| Wstawianie lub podmiotów korespondencji seryjnej   | Tak                | Brak                               |

Należy zauważyć, że operacje **Insert lub zamienianie jednostki** i **Wstaw lub podmiotów korespondencji seryjnej** wykonaj *nie* wykonać kontroli współbieżności, ponieważ nie wysyłaj wartość ETag z usługą tabeli.  

Ogólnie deweloperów przy użyciu tabel miały optymistyczny współbieżności podczas tworzenia aplikacji skalowalna. W razie potrzeby pesymistyczny blokowania deweloperów jeden z nich można podjąć w przypadku uzyskiwania dostępu do tabel do przypisywania wyznaczonych obiektów blob dla każdej tabeli i spróbuj zastosować dzierżawę to przed uruchomieniem w tabeli. Ta metoda wymaga aplikacji w celu zapewnienia wszystkie ścieżki dostępu do danych uzyskać dzierżawę przed działające w tabeli. Należy również zauważyć, że czas dzierżawy minimalne jest to 15 sekundach, które wymaga szczególnej ostrożności dla skalowalność.  

Aby uzyskać więcej informacji, zobacz:  

- [Operacje na jednostek](http://msdn.microsoft.com/library/azure/dd179375.aspx)  

## <a name="managing-concurrency-in-the-queue-service"></a>Zarządzanie współbieżności w usłudze kolejki
Jeden scenariusz, w którym współbieżności ma znaczenie w usłudze Kolejkowanie to miejsce, w którym wielu klientów pobieranych wiadomości z kolejki. Po pobraniu wiadomości z kolejki odpowiedź zawiera wiadomości i wartości pop potwierdzenia, która jest wymagana, aby usunąć wiadomość. Wiadomości nie są automatycznie usuwane z kolejki, ale po zostały pobrane, nie jest widoczny dla innych klientów określonym przez parametr visibilitytimeout przedziale czasu. Aby usunąć wiadomość, po przetworzeniu i wcześniej określone przez TimeNextVisible element odpowiedzi, która jest obliczana na podstawie wartości parametru visibilitytimeout jest planowane klienta, który pobiera wiadomość. Wartość visibilitytimeout jest dodawany do czasu, jaką wiadomość zostanie pobrana w celu określenia wartości TimeNextVisible.  

Usługa kolejki nie jest obsługiwane optymistyczny lub pesymistyczny współbieżności i w tym klientów powodu przetwarzania wiadomości pobierane z kolejki należy upewnić się, że wiadomości są przetwarzane w sposób idempotent. Ostatni strategii wins writer jest używana do aktualizacji operacji, takich jak SetQueueServiceProperties, SetQueueMetaData, SetQueueACL i UpdateMessage.  

Aby uzyskać więcej informacji, zobacz:  

- [Usługa kolejki interfejsu API usługi REST](http://msdn.microsoft.com/library/azure/dd179363.aspx)
- [Pobieranie wiadomości](http://msdn.microsoft.com/library/azure/dd179474.aspx)  

## <a name="managing-concurrency-in-the-file-service"></a>Zarządzanie współbieżności w usłudze pliku
Usługa plików są dostępne za pomocą dwóch różnych punktów końcowych protokołu — SMB i Zatrzymaj. Usługi REST nie jest obsługiwane optymistyczny blokowania albo pesymistyczny blokowania i wszystkie aktualizacje będą zgodne z ostatniego strategii wins writer. Klienci SMB, które instalacji udziały plików mogą korzystać z mechanizmy blokowania system plików do zarządzania dostęp do udostępnionych plików — w tym możliwość wykonywania pesymistyczny blokowania. Po otwarciu pliku klientem SMB określa dostęp do plików i udostępnianie tryb. Ustawianie opcji dostępu do plików, "Pisanie" lub "Odczytu/zapisu" razem z trybem udziale plików "Brak" spowoduje plik jest zablokowany przez klienta SMB, dopóki plik nie zostanie zamknięty. Jeśli jest próby operacji pozostałych na miejsce, w którym klient SMB ma pliku zablokowanego pliku usługi REST zwróci kodu stanu 409 (konflikt) z kodem błędu SharingViolation.  

Po otwarciu pliku do usunięcia klientem SMB oznacza plik jako oczekujących Usuń do innego klienta SMB są zamknięte Otwórz uchwyty tego pliku. Gdy plik został oznaczony jako oczekujące delete, dowolnej operacji pozostałych dla tego pliku może zwrócić kodu stanu 409 (konflikt) z kodem błędu SMBDeletePending. Kod stanu 404 (nie można odnaleźć) nie jest zwracana, ponieważ istnieje możliwość dla klienta SMB usunąć flagę oczekiwanie na usunięcie przed zamknięciem pliku. Innymi słowy kodu stanu 404 (nie można odnaleźć) jest planowane tylko, jeśli plik został usunięty. Należy zauważyć, że plik w trakcie SMB oczekujących Usuń stan, go nie zostaną uwzględnione w wynikach listy plików. Należy również zauważyć, że operacje pozostałych usuwanie plików i katalogów usuwanie pozostałych są przekazywane atomically i nie powodują oczekujących Usuń stan.  

Aby uzyskać więcej informacji, zobacz:  

- [Zarządzanie plikiem blokady](http://msdn.microsoft.com/library/azure/dn194265.aspx)  

## <a name="summary-and-next-steps"></a>Podsumowanie i następne kroki
Usługa Microsoft Azure magazynu został zaprojektowany do potrzeb najbardziej złożonych aplikacji online bez wymuszania programistom na złamanie lub przemyślenia założenia najważniejsze takich jak spójności współbieżności i dane, które pochodzą podjęcie udzielone.  

Na podstawie całą próbkę wymienianych w tym blogu:  

- [Zarządzanie współbieżności przy użyciu magazynu Azure - przykładowej aplikacji](http://code.msdn.microsoft.com/Managing-Concurrency-using-56018114)  

Aby uzyskać więcej informacji o magazyn Azure, zobacz:  

- [Strona główna pakietu Microsoft Azure miejsca do magazynowania](https://azure.microsoft.com/services/storage/)
- [Wprowadzenie do przechowywania Azure](storage-introduction.md)
- Wprowadzenie do aplikacji dla [obiektów Blob](storage-dotnet-how-to-use-blobs.md), [tabeli](storage-dotnet-how-to-use-tables.md) [kolejki](storage-dotnet-how-to-use-queues.md)i [pliki](storage-dotnet-how-to-use-files.md) miejsca do magazynowania
- Architektura pamięci — [Azure miejsca do magazynowania: usługi w chmurze wysokiej dostępności miejsca do magazynowania z spójności silnych](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)
