<properties 
    pageTitle="Protokół połączeń hybrydowych Azure przekazywania | Microsoft Azure"
    description="zure przekazywania hybrydowych połączeń protokołu przewodnika."
    services="service-bus"
    documentationCenter="na"
    authors="clemensv"
    manager="timlt"
    editor="tysonn" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/28/2016"
    ms.author="sethm" />

# <a name="azure-relay-hybrid-connections-protocol"></a>Protokół połączeń hybrydowych Azure przekazywania

Azure przekazywania jest jednym z kolumn klucza możliwości platformy Azure Bus usługi. Przekazywania nowych "Hybrydowych połączenia" jest kształtowanie bezpieczne, Otwórz protokół, na podstawie protokołu HTTP i WebSockets.

Zastępuje pierwsze, jednakowo o nazwie "BizTalk usług" funkcją, której został utworzony na foundation własnych Protocol (protokół). Integrację usługi aplikacji Azure hybrydowych połączenia będą nadal działać jako-jest.

"Hybrydowych połączenia" umożliwia ustanawianie strumienia dwukierunkowego, binarnych komunikacji między dwiema aplikacjami sieci, w której jedną lub obie strony mogą znajdować się za NAT lub zapory.

W tym dokumencie opisano interakcje po stronie klienta z przekazywania połączeń hybrydowych nawiązywania połączenia klienci ról odbiornika i nadawcy i jak detektory akceptować nowe połączenia.

## <a name="interaction-model"></a>Model interakcji

Przekazywania połączeń hybrydowych łączy dwie strony, dostarczając punktu rendez-vous w chmurze Azure, która zarówno strony można wykryć i łączenie się z perspektywy własną sieć. Tego punktu rendez-vous nosi nazwę "Hybrydowych połączenia", w tym i innych dokumentów za pośrednictwem interfejsów API, a także w Azure Portal. Punktu końcowego usługi połączeń hybrydowych będą określane jako "Usługa" w dalszej części tego dokumentu.

Model interakcji leans na Nomenklaturze ustanowioną przez wielu innych sieci interfejsów API:

Istnieje odbiornik najpierw wskazuje gotowości powinien obsługiwać połączenia przychodzące, a następnie akceptuje je pojawiające się. Na drugiej stronie jest łączących klienta, który będzie łączyć odbiornika oczekiwano tego połączenia zaakceptowane ustalania ścieżki komunikacja dwukierunkowa. "Łączenie", "Słuchać", "Akceptuj" są tych samych warunkach, które można znaleźć w większości socket interfejsów API.

Dowolny model komunikacji odnośnie ma jednej ze stron nawiązywać połączenia wychodzące do punktu końcowego usługi, który wykonuje "detektor" również "klient" potocznej używany i może powodować inne overloads terminologia; dokładne jakimi używamy dlatego hybrydowego połączeń jest następująca:

Programy po obu stronach połączenia są nazywane "klient", ponieważ są one klientów z usługą. Klienta, który czeka i akceptuje połączenia jest "detektor" lub znajdować się w roli"detektor". Klient, który inicjuje nowe połączenie kierunku odbiornika za pomocą usługi nosi nazwę "nadawca" lub "roli nadawcy".

## <a name="listener-interactions"></a>Interakcje odbiornika

Odbiornik zawiera cztery interakcji z usługą; wszystkie szczegóły przewodowy opisano w dalszej części tego dokumentu w sekcji odwołania.

### <a name="listen"></a>Odsłuchać

Aby wskazać gotowość do usługi, która jest detektor może zaakceptować połączenia, tworzy połączenia wychodzące web socket. Uzgadnianie połączenia przenosi nazwa połączenia hybrydowych skonfigurowanych w obszarze nazw przekazywania i tokenu zabezpieczającego który przyznaje "Słuchać" pracę z o nazwie. Po zaakceptowaniu socket sieci web przez usługę rejestracji zostało zakończone i socket ustanowienia sieci web jest życiu jako "kanału kontroli" umożliwiających wszystkich kolejnych interakcji. Usługa umożliwia maksymalnie 25 detektory równoczesne połączenie hybrydowych. W przypadku detektory 2 lub więcej aktywnego połączenia przychodzące zostaną zbilansowane w notatkach w kolejności losowej; rozkład projektu Naukowego nie jest gwarantowana.

### <a name="accept"></a>Zaakceptuj

Przy każdym otwarciu nadawcy nowe połączenie w usłudze, usługa wybierz i powiadom jedną detektorów aktywnego połączenia hybrydowych. Powiadomienie zostanie wysłane do odbiornika kanałem sterowania otwórz jako wiadomości JSON zawierający adres URL punktu końcowego socket sieci Web, który musi się połączyć odbiornika akceptowania połączenia.

Adres URL można i mogą być używane bezpośrednio przez odbiornik bez dodatkowych starań; zakodowany informacji jest prawidłowa tylko na krótko czasu, zasadniczo tak długo, jak nadawca jest gotowa czekać połączenie za wskazanych zakończenia do końca, ale maksymalnie do 30 sekund. Adres URL można używać tylko dla jednego połączenia próby. Zaraz po nawiązaniu połączenia socket sieci Web z adresem URL rendez-vous wszystkie kolejne czynności na tym socket sieci Web są przekazywane od i do nadawcy, bez interwencji lub interpretacji przez usługę.

### <a name="renew"></a>Odnawianie 

Tokenu zabezpieczającego, używanym do rejestrowania odbiornika i zachować kanału kontroli mogą wygaśnie, gdy odbiornika jest aktywna. Token wygaśnięcia nie ma wpływu na bieżąco połączenia, ale spowoduje kanału kontroli ma być przerwane przez usługę w lub krótko po moment wygaśnięcia. Gest "Odnawianie" jest ono JSON odbiornika można wysłać do Zamień token skojarzony z kanału kontroli utrzymywanie kanału kontroli mogą być przez dłuższy czas.

### <a name="ping"></a>Polecenie ping 

Jeśli kanał kontrolny pozostaje bezczynne przez długi czas, pośredników w ten sposób, takich jak ładowania urządzenia do równoważenia lub NAT może upuść połączenia TCP. Gest "ping" który pozwala uniknąć przez wysłanie niewielkiej ilości danych w kanale, przypominający wszystkim trasę sieciową, która połączenia mają być aktywności, a także służy jako liveness test odbiornika. Jeśli polecenie ping nie powiedzie się, kanał kontrolny należy rozważyć nie będzie można używać i odbiornika należy ponownie nawiązać połączenie.

## <a name="sender-interaction"></a>Interakcja nadawcy

Nadawca zawiera tylko pojedynczej interakcji z usługą, łączy.

### <a name="connect"></a>Nawiązywanie połączenia

Gest "łączenie" otwiera socket sieci Web w usłudze podanie nazwy połączenia hybrydowych oraz (opcjonalny, ale wymagany domyślnie) tokenu zabezpieczającego dające uprawnień "Wyślij" w ciągu kwerendy. Usługa zostanie następnie współpracować z odbiornika w sposób opisany powyżej i mają odbiornika, tworzenie połączenia rendez-vous przyłączonym z tym socket sieci Web. Po zaakceptowaniu Web socket wszystkich dalszych interakcji na socket sieci Web będzie się zatem z odbiornikiem połączonego.

## <a name="interaction-summary"></a>Podsumowanie interakcji

Wynik tego modelu interakcji to, że klienta nadawcy pochodzi z uzgadnianie z "Oczyść" socket sieci Web jest połączony detektor i wymaga nie dalszych preambles lub przygotowania. Dzięki temu praktycznie dowolnego istniejącej sieci Web socket klienta implementacji łatwo skorzystać z Usługa połączenia hybrydowych po prostu, podając poprawnie utworzony adres URL do ich klienta SSL sieci Web.

Połączenie rendez-vous Socket sieci Web, odbiornika uzyskujące za pośrednictwem interakcji Zaakceptuj jest również Oczyść i jest przekazywany wszelkie istniejące sieci Web socket implementacji serwera z niektórych minimalnego dodatkowe dysproporcja rozróżnia operacji "Akceptuj" na ich struktury sieci lokalnej detektory i połączeń hybrydowych operacje zdalnego "Akceptuj".

## <a name="protocol-reference"></a>Odwołanie do protokołu

W tej sekcji opisano interakcji protokołu opisany powyżej.

Wszystkie połączenia socket sieci Web są wykonywane na porcie 443 jako uaktualnienie z 1.1 HTTPS, który jest często usunięte przez niektóre struktury socket sieci Web lub interfejsu API. Opis, w tym miejscu są przechowywane implementacji neutralne, bez sugerowania strukturą określonych.

## <a name="listener-protocol"></a>Protokół odbiornika

Protokół odbiornik składa się z dwóch gestów połączenia i trzech operacji wiadomości.

### <a name="listener-control-channel-connection"></a>Połączenia kanału sterowania odbiornika

Kanału kontroli zostanie otwarty z tworzenia połączenia socket sieci Web:

*wss: / / {nazw adres}-* *$servicebus* */* *hybridconnection-**{ścieżka}? sb hc akcji =... i sb-hc-id =... i sb hc token =... "*

*Adres nazw* jest w pełni kwalifikowaną nazwę domeny nazw przekazywania Azure, obsługującym połączenia hybrydowe, zwykle formy {*mojanazwa}. servicebus.windows.net.*

Dostępne są następujące opcje parametru ciągu kwerendy

| Parametr        | Wymagane? | Opis                                                                                                                                                                                        |
|--------------|-----------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| SB hc akcji | Tak       | Dla roli detektora parametr musi być **sb hc akcji = słuchać**                                                                                                                                |
| {Ścieżka}       | Tak       | Ścieżka zakodowany adres URL nazw wstępnie skonfigurowane połączenie hybrydowego zarejestrować tego odbiornika. To wyrażenie jest dołączany do stałej * **$servicebus**/**hybridconnection-*** część ścieżki. |
| SB hc tokenów  | Tak\*     | Odbiornika należy podać prawidłowy, zakodowany adres URL usługi Bus udostępnione dostępu Token nazw lub połączenie hybrydowych uprawnia **odsłuchać** .                                           |
| SB-hc-id     | Brak        | Ten identyfikator opcjonalne dostarczonym klienta umożliwia śledzenie diagnostyczne zakończenia do końca.                                                                                                                             |

Jeśli połączenie Web Socket kończy się niepowodzeniem z powodu ścieżkę hybrydowych połączenia nie jest zarejestrowany lub token nieprawidłowe lub brakujące lub inny błąd, zostaną udostępnione opinii błędu przy użyciu zwykłego modelu opinii stanu HTTP 1.1. Opis stanu będzie zawierać błędu-identyfikator śledzenia mogą być przekazywane do pomocy technicznej Azure:

| Kod | Błąd          | Opis                                                            |
|------|----------------|------------------------------------------------------------------------|
| 404  | Nie można odnaleźć      | Połączenie hybrydowych **ścieżka** jest nieprawidłowa lub podstawowy adres URL jest uszkodzony |
| 401  | Brak autoryzacji   | Tokenu zabezpieczającego lub nieprawidłowo lub jest nieprawidłowy                  |
| 403  | Dostęp zabroniony      | Tokenu zabezpieczającego nie jest prawidłowa dla tej ścieżki dla tej akcji          |
| 500  | Błąd wewnętrzny | Wystąpił problem w usłudze                                    |

Wyłączenie połączenia socket Web celowo przez usługę po została początkowo prawidłowo skonfigurowana powód to będą przekazywane przy użyciu odpowiednie Web socket protokół kod błędu wraz z komunikat z opisem błędu, które będzie również zawierać identyfikator śledzenia. Usługa nie wyłączy kanału kontroli bez występują błąd. Wszelkie Oczyść zamknięcie jest kontrolowane klienta.

| Stan WS | Opis                                                                        |
|-----------|------------------------------------------------------------------------------------|
| 1001      | Ścieżka połączenia hybrydowych został usunięty lub wyłączony.                           |
| 1008      | Tokenu zabezpieczającego wygasła i w związku z tym naruszenia zasad autoryzacji. |
| 1011      | Wystąpił problem w usłudze.                                           |

### <a name="accept-handshake"></a>Zaakceptuj uzgadniania 

Powiadomienie Zaakceptuj jest wysyłane przez usługę odbiornika kanałem sterowania wcześniej wskazanych w jako wiadomości JSON w ramce tekstu socket sieci Web. Brak jest odpowiedzi na tę wiadomość.

Wiadomość zawiera obiekt JSON o nazwie "Akceptuj", który definiuje następujące właściwości w tej chwili:

-   **adres** — ciąg adresu URL, który ma być używany dla ustanowienia Socket sieci Web z usługą, aby zaakceptować połączenie przychodzące.

-   **id** — Unikatowy identyfikator dla tego połączenia. Jeśli identyfikator został dostarczony przez klienta nadawcy, jest to wartość dostarczane nadawcy, w przeciwnym razie wartość generowane przez system.

-   **connectHeaders** — wszystkie nagłówki HTTP, które zostały przekazane do punktu końcowego przekazywania przez nadawcę, który uwzględnia również protokół WebSocket s i nagłówki s-WebSocket-rozszerzenia.

| Akceptowanie wiadomości                                                                     |
|------------------------------------------------------------------------------------|
```json
{                                                                                  
    "accept" : {                                                                                    
        "address" : "wss://168.61.148.205:443/$servicebus/hybridconnection/{path}?...",                                                                          
        "id" : "4cb542c3-047a-4d40-a19f-bdc66441e736\_G0\_G1",                         
        "connectHeaders" : {                                                                
            "Host" : "…",                                                                       
            "Sec-WebSocket-Protocol" : "…",                                                     
            "Sec-WebSocket-Extensions" : "…"                                                    
        }                                                                                     
    }
}
```

Adres URL podanymi w wiadomości, JSON jest używana przez odbiornik ustanawiania Socket sieci Web, aby zaakceptować lub odrzucić socket nadawcy.

#### <a name="accepting-the-socket"></a>Akceptowanie socket

Aby zaakceptować, odbiornika nawiąże połączenie WebSocket z podanego adresu.

Należy pamiętać, że okres Podgląd adres URI może użyć adresu IP od zera i TLS certyfikatu podanego przez serwer nie powiedzie się sprawdzanie poprawności na ten adres. Spowoduje to usunięcia podczas podglądu.

Jeśli wiadomość "zaakceptować" przenosi nagłówka "S-WebSocket-Protocol", jest planowane że odbiornika tylko zaakceptuje WebSocket Jeśli obsługuje protokołu i że ustalonej WebSocket ustawia nagłówek.

To samo dotyczy do nagłówka "S-WebSocket-rozszerzenia". Jeśli w ramach obsługuje rozszerzenia, ustala nagłówek odpowiedź po stronie *serwera* wymagane uzgadniania "S-WebSocket-rozszerzenia" dla rozszerzenia.

Adres URL musi być używany jako-jest ustalania socket Zaakceptuj, ale zawiera następujące informacje:

| Parametr        | Wymagane? | Opis                                                                                                                                                                                                                                                                                   |
|--------------|-----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| SB hc akcji | Tak       | Akceptowania socket parametr musi być **sb hc akcji = zaakceptować**                                                                                                                                                                                                                          |
| {Ścieżka}       | Tak       | Ścieżka zakodowany adres URL nazw wstępnie skonfigurowane połączenie hybrydowego zarejestrować tego odbiornika. To wyrażenie jest dołączany do stałej * **$servicebus**/**hybridconnection-*** część ścieżki.                                                                                            
                                                                                                                                                                                                                                                                                                                           
                            The path expression MAY be extended with a suffix and a query string expression that follows the registered name after a separating forward slash.                                                                                                                                             
                                                                                                                                                                                                                                                                                                                           
                            This allows the sender client to pass dispatch arguments to the accepting listener when it is not possible to include HTTP headers.                                                                                                                                                            
                                                                                                                                                                                                                                                                                                                           
                            The expectation is that the listener framework will parse out the fixed path portion and the registered name from the path and make the remainder, possibly without any query string arguments prefixed by “sb-“, available to the application for deciding whether to accept the connection.  
                                                                                                                                                                                                                                                                                                                           
                            For more detail see the “Sender Protocol” section below.                                                                                                                                                                                                                                       |
| SB hc identyfikator | No        | Zobacz opis **identyfikator** powyżej.                                                                                                                                                                                                                                                              |

Jeśli występuje błąd, usługa może wysłać odpowiedź w następujący sposób:

| Kod | Błąd          | Opis                         |
|------|----------------|-------------------------------------|
| 403  | Dostęp zabroniony      | Adres URL jest nieprawidłowy.               |
| 500  | Błąd wewnętrzny | Wystąpił problem w usłudze |

Po nawiązaniu połączenia, serwer zostanie zamknięty socket sieci Web po nadawcy socket Web przebiega w dół lub z następujących statusów

| Stan WS | Opis                                                                        |
|-----------|------------------------------------------------------------------------------------|
| 1001      | Klienta nadawcy zamknięcie połączenia                                        |
| 1001      | Ścieżka połączenia hybrydowych został usunięty lub wyłączony.                           |
| 1008      | Tokenu zabezpieczającego wygasła i w związku z tym naruszenia zasad autoryzacji. |
| 1011      | Wystąpił problem w usłudze.                                           |

#### <a name="rejecting-the-socket"></a>Odrzucanie socket

Odrzucanie socket po sprawdzanie komunikat "udzielenia" wymaga podobne uzgadniania kod stanu i opis stanu komunikowania się powód odrzucenia może przechodzić do nadawcy.

Wybór projektu Protocol (protokół) w tym miejscu jest użycie uzgadniania WebSocket (przeznaczonego do końca w stanie błędu zdefiniowane), aby implementacji klienta odbiornika nadal opierają się na komputerze klienckim WebSocket i nie musisz stosować dodatkową, b klienta HTTP.

Aby odrzucić socket, klient ma adres identyfikatora URI z poziomu wiadomości "zaakceptować" i dołącza dwa parametry ciągu kwerendy:

| Parametr             | Wymagane? | Opis                             |
|-------------------|-----------|-----------------------------------------|
| statusCode        | Tak       | Kod numeryczny znaku stan HTTP                |
| statusDescription | Tak       | Ludzi czytelne powód odrzucenia |

Wynikowa identyfikatora URI służy do nawiązania połączenia WebSocket; ponownie Pamiętaj, że certyfikat TLS może różnić się adres w wersji preview, więc sprawdzania poprawności może być konieczne zostanie wyłączona.

Po zakończeniu poprawnie, ten uzgadniania celowo zakończy się niepowodzeniem z kodem błędu HTTP 410, ponieważ nie WebSocket zostały ustanowione. Jeśli wystąpi błąd, są następujące opcje:

| Kod | Błąd          | Opis                         |
|------|----------------|-------------------------------------|
| 403  | Dostęp zabroniony      | Adres URL jest nieprawidłowy.               |
| 500  | Błąd wewnętrzny | Wystąpił problem w usłudze |

### <a name="listener-token-renewal"></a>Odnawiania tokenów odbiornika

Po token odbiornika zbliża się termin wygaśnięcia go można zastąpić, wysyłając wiadomość ramka tekstu z usługą za pośrednictwem kanału ustanowienia kontroli. Wiadomość zawiera obiekt JSON o nazwie "renewToken", który definiuje następujące właściwości w tej chwili:

-   **token** — prawidłowy, zakodowany adres URL usługi Bus udostępnione dostępu Token obszaru nazw lub połączenie hybrydowych uprawnia **odsłuchać** .

| renewToken wiadomości                                                                                                                                                    
|------------------------------------------------------------------------------------------------|
```json
{
    "renewToken" : {      
        "token" : "SharedAccessSignature sr=http%3a%2f%2fcontoso.servicebus.windows.net%2fHybridConnection1%2f&amp;sig=XXXXXXXXXX%3d&amp;se=1471633754&amp;skn=SasKeyName"
    }                                                                                                                                                            
}
```                                                                                                                                                                      

Jeśli token sprawdzania poprawności kończy się niepowodzeniem, odmowa dostępu, a usługa w chmurze zostanie zamknięty websocket kanału sterowania się komunikat o błędzie, w przeciwnym razie jest nie odpowiedzi.

| Stan WS | Opis                                                                        |
|-----------|------------------------------------------------------------------------------------|
| 1008      | Tokenu zabezpieczającego wygasła i w związku z tym naruszenia zasad autoryzacji. |

<a name="sender-protocol"></a>Protokół nadawcy
---------------

Protokół nadawcy jest identyczne jak ustalić detektor. Celem jest maksymalna przezroczystości socket Web zakończenia do końca. Adres, aby nawiązać połączenie, jest taka sama jak listener, ale różni się "działania", a token potrzebuje różnych uprawnień:

*wss: / / {nazw adres}-* *$servicebus* */* *hybridconnection-**{ścieżka}? sb hc akcji =... i sb-hc-id =... \[i sbc hc token =... \]*

*Adres nazw* jest w pełni kwalifikowaną nazwę domeny nazw przekazywania Azure, obsługującym połączenia hybrydowe, zwykle formy {*mojanazwa}. servicebus.windows.net.*

Żądanie może zawierać dowolnego dodatkowe, nagłówki HTTP, łącznie z nich zdefiniowane przez aplikację. Wszystkie nagłówki podanym przepływu dla słuchacza i znajduje się na obiekt "connectHeader" komunikatu kontroli "zaakceptować".

Dostępne są następujące opcje parametru ciągu kwerendy

| Parametr        | Wymagane? | Opis                                                                                                                                                                                                       |
|--------------|-----------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| SB hc akcji | Tak       | Dla roli detektora parametr musi być **akcji = łączenie**                                                                                                                                                    |
| {Ścieżka}       | Tak       | Ścieżka zakodowany adres URL nazw wstępnie skonfigurowane połączenie hybrydowego zarejestrować tego odbiornika.                                                                                                               
                                                                                                                                                                                                                                               
                            The path expression MAY be extended with a suffix and a query string expression to communicate further                                                                                                             
                                                                                                                                                                                                                                               
                            If the Hybrid Connection is registered under the path “hyco”, the path expression can be “**hyco/**suffix?param=value&…” followed by the query string parameters defined here. A complete expression may then be:  
                                                                                                                                                                                                                                               
                            *wss://{ns}/**$servicebus**/**hybridconnection/*** **hyco/**suffix?param=value*& sb-hc-action=…&sb-hc-id=…\[&sbc-hc-token=…\]*                                                                                     
                                                                                                                                                                                                                                               
                            The path expression is passed through to the listener in the address URI contained in the “accept” control message.                                                                                                |
| SB hc tokenów | Yes\*     | Odbiornika należy podać prawidłowy, zakodowany adres URL usługi Bus udostępnione dostępu Token nazw lub połączenie hybrydowych uprawnia **Wyślij** .                                                            | | SB hc identyfikator | No        | Identyfikator opcjonalne, który umożliwia śledzenie diagnostyczne zakończenia do końca i będą dostępne dla odbiornika podczas uzgadniania zaakceptuj.                                                                                       |

Jeśli połączenie Web Socket kończy się niepowodzeniem z powodu ścieżkę hybrydowych połączenia nie jest zarejestrowany lub token nieprawidłowe lub brakujące lub inny błąd, zostaną udostępnione opinii błędu przy użyciu zwykłego modelu opinii stanu HTTP 1.1. Opis stanu będzie zawierać błędu-identyfikator śledzenia mogą być przekazywane do pomocy technicznej Azure:

| Kod | Błąd          | Opis                                                            |
|------|----------------|------------------------------------------------------------------------|
| 404  | Nie można odnaleźć      | Połączenie hybrydowych **ścieżka** jest nieprawidłowa lub podstawowy adres URL jest uszkodzony |
| 401  | Brak autoryzacji   | Tokenu zabezpieczającego lub nieprawidłowo lub jest nieprawidłowy                  |
| 403  | Dostęp zabroniony      | Tokenu zabezpieczającego nie jest prawidłowa dla tej ścieżki dla tej akcji          |
| 500  | Błąd wewnętrzny | Wystąpił problem w usłudze                                    |

Wyłączenie połączenia socket Web celowo przez usługę po została początkowo prawidłowo skonfigurowana powód to będą przekazywane przy użyciu odpowiednie Web socket protokół kod błędu wraz z komunikat z opisem błędu, które będzie również zawierać identyfikator śledzenia.

| Stan WS | Opis                                                                        |
|-----------|------------------------------------------------------------------------------------|
| 1000.      | Odbiornik Zamknij socket.                                                 |
| 1001      | Ścieżka połączenia hybrydowych został usunięty lub wyłączony.                           |
| 1008      | Tokenu zabezpieczającego wygasła i w związku z tym naruszenia zasad autoryzacji. |
| 1011      | Wystąpił problem w usłudze.                                           |

## <a name="next-steps"></a>Następny krok:

- [Relay — często zadawane pytania](relay-faq.md)
- [Tworzenie obszaru nazw](relay-create-namespace-portal.md)
- [Wprowadzenie do programu .NET](relay-hybrid-connections-dotnet-get-started.md)
- [Wprowadzenie do węzła](relay-hybrid-connections-node-get-started.md)