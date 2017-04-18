<properties
    pageTitle="Obsługa udostępniania (CORS) między Origin zasobu | Microsoft Azure"
    description="Dowiedz się, jak włączyć obsługę CORS usługi Microsoft Azure miejsca do magazynowania."
    services="storage"
    documentationCenter=".net"
    authors="cbrooks"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/07/2016"
    ms.author="cbrooks"/>

# <a name="cross-origin-resource-sharing-cors-support-for-the-azure-storage-services"></a>Zasób Origin krzyżowe udostępniania (CORS) obsługę usług Azure miejsca do magazynowania

Począwszy od wersji 2013-08-15, usług Azure magazynu obsługuje udostępniania zasobu między Origin (CORS) dla usług obiektów Blob, tabeli kolejki i plik. CORS to funkcja HTTP, która umożliwia aplikacja sieci web w obszarze jedną domenę dostęp do zasobów w innej domeny. Przeglądarki sieci Web zaimplementować ograniczenie zabezpieczeń znana pod nazwą [zasad samego pochodzenia](http://www.w3.org/Security/wiki/Same_Origin_Policy) uniemożliwia strony sieci web z API połączeń do innej domeny; CORS udostępnia bezpieczny sposób, aby umożliwić jedną domenę (domeny pochodzenia) połączenie interfejsy API w innej domeny. Zobacz [Specyfikacja CORS](http://www.w3.org/TR/cors/) , aby uzyskać szczegółowe informacje na CORS.

Można ustawić reguły CORS indywidualnie dla każdej z usług przechowywania, dzwoniąc [Właściwości zestawu obiektów Blob usługi](https://msdn.microsoft.com/library/hh452235.aspx), [Ustawianie właściwości usługi kolejki](https://msdn.microsoft.com/library/hh452232.aspx)i [Ustawianie właściwości usługi tabeli](https://msdn.microsoft.com/library/hh452240.aspx). Po ustawieniu reguły CORS usługi do określenia, czy jest dozwolone zgodnie z regułami, które określono obliczane jest odpowiednio uwierzytelnieni wnioski przed usługę z innej domeny.

>[AZURE.NOTE] Należy zauważyć, że CORS nie jest mechanizm uwierzytelniania. Wszelkie wnioski przed zasób magazynowania po włączeniu CORS albo musi mieć podpis odpowiedniego uwierzytelnienia lub należy przed publicznej zasobów.

## <a name="understanding-cors-requests"></a>Opis żądania CORS

Żądanie CORS z domeny pochodzenia może się składać z dwóch oddzielnych żądania:

- Żądanie wstępnej, który bada ograniczeń CORS narzucane przez usługę. Żądanie wstępnej jest wymagane, chyba że metody żądania jest [prosta metoda](http://www.w3.org/TR/cors/), co oznacza GET, głowy lub WPIS.

- Żądanie rzeczywiste, wprowadzone przed żądanego zasobu.

### <a name="preflight-request"></a>Żądanie wstępnej

Kwerendy żądania wstępnej ograniczenia CORS, które zostały utworzone przez usługę magazynowania przez właściciela konta. Przeglądarki sieci web (lub agenta użytkownika) przesyła żądanie opcje, które zawierają nagłówków żądania, metody i pochodzenie domeny. Usługa magazynu oblicza zamierzonego operacji na podstawie wstępnie skonfigurowane zestawu reguł CORS, które określają, jakie origin domen, metod żądań i nagłówków żądania można określić na żądanie rzeczywiste przed zasobu miejsca do magazynowania.

CORS włączono usługę, jeśli ma reguły CORS odpowiadającego żądaniu wstępnej, usługa odpowiada z kodem stanu 200 (OK) i zawiera wymagane nagłówki kontroli dostępu w odpowiedzi.

Jeśli CORS nie jest włączona dla usługi lub nie CORS przez Reguła wstępnej żądanie, usługa będzie odpowiadał z kodem stanu 403 (Dostęp zabroniony).

Jeśli wezwanie na opcje nie zawiera wymagane nagłówki CORS (nagłówki pochodzenia i metoda kontroli dostępu — żądanie), usługa będzie odpowiadał z kodem stanu 400 (nieprawidłowe żądanie).

Należy zauważyć, że żądania wstępnej jest obliczane przed usługę (obiektów Blob, kolejki i tabeli) i nie przed żądanego zasobu. Właściciel konta musi mieć włączony CORS w ramach usługi właściwości konta w obszarze kolejność żądania powiodła się.

### <a name="actual-request"></a>Żądania

Po zaakceptowaniu wstępnej żądania i odpowiedzi jest zwracany, przeglądarce wyśle rzeczywisty żądanie zasobu miejsca do magazynowania. Przeglądarka uniemożliwi rzeczywistych żądanie od razu, jeśli wstępnej żądanie zostanie odrzucone.

Rzeczywiste wezwanie jest traktowana jako normalny żądania usługi miejsca do magazynowania. Obecność nagłówka Origin wskazuje, że żądanie jest żądanie CORS i usługa sprawdzi reguł dopasowania CORS. Jeśli zostanie znaleziony pasujący element, nagłówki kontroli dostępu są dodawane do odpowiedzi i wysyłane do klienta. Jeśli nie, nie są zwracane nagłówki kontrola dostępu CORS.

## <a name="enabling-cors-for-the-azure-storage-services"></a>Włączanie CORS usługi Magazyn Azure

CORS zasady są ustawione na poziomie usługi, więc należy włączyć lub wyłączyć CORS dla każdej usługi (obiektów Blob, kolejki i tabeli) oddzielnie. Domyślnie CORS jest wyłączona dla każdej usługi. Aby włączyć CORS, należy ustawić właściwości odpowiednią usługę przy użyciu wersji 2013-08-15 lub nowszym i dodawać reguły CORS właściwości usługi. Aby uzyskać szczegółowe informacje dotyczące sposobu Włączanie lub wyłączanie CORS usługi i ustawiania reguł CORS zapoznaj się [Właściwości zestawu obiektów Blob usługi](https://msdn.microsoft.com/library/hh452235.aspx), [Ustawianie właściwości usługi kolejki](https://msdn.microsoft.com/library/hh452232.aspx)i [Ustawianie właściwości usługi tabeli](https://msdn.microsoft.com/library/hh452240.aspx).

Oto przykładowa jednej reguły CORS określona za pomocą operacji Ustawianie właściwości usługi:

    <Cors>    
        <CorsRule>
            <AllowedOrigins>http://www.contoso.com, http://www.fabrikam.com</AllowedOrigins>
            <AllowedMethods>PUT,GET</AllowedMethods>
            <AllowedHeaders>x-ms-meta-data*,x-ms-meta-target*,x-ms-meta-abc</AllowedHeaders>
            <ExposedHeaders>x-ms-meta-*</ExposedHeaders>
            <MaxAgeInSeconds>200</MaxAgeInSeconds>
        </CorsRule>
    <Cors>

Poniżej opisano każdy element reguły CORS:

- **AllowedOrigins**: domen origin, które mogą być żądania usługi miejsca do magazynowania za pośrednictwem CORS. Domena pochodzenia jest domena, z której pochodzi żądanie. Należy zauważyć, że pochodzenie musi być dokładne dopasowanie uwzględniania wielkości liter, wraz ze źródłem, wysyłające wieku użytkownika z usługą. Można również użyć znaku wieloznacznego "*" umożliwia wszystkich domen origin zgłaszać żądań za pośrednictwem CORS. W powyższym przykładzie domen [http://www.contoso.com](http://www.contoso.com) i [http://www.fabrikam.com](http://www.fabrikam.com) pozwalają żądania usługi przy użyciu CORS.

- **AllowedMethods**: metod (zleceń żądania HTTP), używane w domenie pochodzenia może żądania CORS. W powyższym przykładzie są dozwolone tylko żądania ODBIERZ i Wyślij.

- **AllowedHeaders**: nagłówków żądania, które domenie pochodzenia mogą określać na żądanie CORS. W powyższym przykładzie wszystkie nagłówki metadanych, zaczynając od x-ms metadane x-ms-meta docelowej i x-ms-meta-abc jest dozwolone. Należy zauważyć, że symbol wieloznaczny "*" wskazuje, że dowolny nagłówek począwszy od określonego prefiksu jest dozwolone.

- **ExposedHeaders**: nagłówki odpowiedzi, które wysłanych w odpowiedzi na żądanie CORS i uwidocznione przez przeglądarkę, aby wystawcy wezwanie. W powyższym przykładzie przeglądarce jest instrukcja udostępniania dowolny nagłówek począwszy od x-ms-meta.

- **MaxAgeInSeconds**: żądanie maksymalnego czasu przeglądarki należy buforowania przez opcje inspekcji wstępnej.

Usługi Azure miejsca do magazynowania obsługują, określając wstępnie ustalonych nagłówków zarówno **AllowedHeaders** , jak i **ExposedHeaders** elementy. Aby umożliwić kategorii nagłówków, można określić typowe prefiks dla tej kategorii. Na przykład, określając *x-ms-meta** jako nagłówka wstępnie ustalonych Określa regułę, która będzie pasować do wszystkich nagłówków, które zaczynają się od x-ms-meta.

Następujące ograniczenia dotyczą CORS reguł:

- Można określić maksymalnie pięciu reguły CORS dla danej usługi miejsca do magazynowania (obiektów Blob, tabel i kolejki).
- Maksymalny rozmiar wszystkich CORS reguły ustawień na żądanie, z wyłączeniem znaczniki XML nie powinna przekraczać 2 KB.
- Długość dozwolonych nagłówka, dostępne nagłówek lub dozwolonych pochodzenia nie powinna przekraczać 256 znaków.
- Dozwolone nagłówki i nagłówki dostępne mogą być:
  - Nagłówki literałów miejsce, w którym jest dostarczany nazwę dokładnie nagłówka, takie jak **x-ms-meta przetwarzane**. Maksymalnie 64 literałów nagłówków można określić na żądanie.
  - Prefiks nagłówki, gdzie udostępnił prefiks nagłówka, takich jak **x-ms metadane***. Określanie prefiksu w ten sposób umożliwia lub udostępnia dowolny nagłówek, który zaczyna się od danego prefiksu. Można określić więcej niż dwóch wstępnie ustalonych nagłówków na żądanie.
- Metody (lub zleceń HTTP), określonego w elemencie **AllowedMethods** musi spełniać metody obsługiwane przez usługi Azure magazyn interfejsów API. Obsługiwane metody to usuń GET, głowy, korespondencji seryjnej, WPIS, opcje i położenie.

## <a name="understanding-cors-rule-evaluation-logic"></a>Opis CORS reguły oceny logiczny

Usługa magazynu otrzyma prośbę o wstępnej lub rzeczywistymi, wynikiem jest żądanie zgodnie z regułami CORS, które zostały ustalone przez usługę za pośrednictwem odpowiednią operację Ustawianie właściwości usługi. Reguły CORS są sprawdzane w kolejności, w jakiej zostały ustawione w treści wezwania operacji ustawić właściwości usługi.

Reguły CORS są obliczane w następujący sposób:

1. Najpierw domenie pochodzenia żądania sprawdza domen dla elementu **AllowedOrigins** na liście. Domeny pochodzenia znajduje się na liście, czy wszystkie domeny są dozwolone symbol wieloznaczny "*", następnie zasady oceny przychody. Jeśli domena pochodzenia nie jest uwzględniane, żądanie kończy się niepowodzeniem.

2. Następnie metoda (lub zlecenie HTTP) żądania jest sprawdzany według metod wymienionych w elemencie **AllowedMethods** . Jeśli metodę znajduje się na liście, następnie przechodzi reguły oceny; Jeśli nie, żądanie kończy się niepowodzeniem.

3. Jeśli żądanie nie odpowiada reguły w swojej domenie pochodzenia i jego metodę, tę regułę jest zaznaczone do procesu, które są obliczane żądania oraz żadnych dalszych reguł. Zanim mogą zostać obsłużone żądanie, jednak wszystkie nagłówki określonej na żądanie są porównywane nagłówki wymienione w elemencie **AllowedHeaders** . Nagłówki wysyłane nie odpowiadają dozwolonych nagłówków, żądanie kończy się niepowodzeniem.

Ponieważ reguły są przetwarzane w kolejności, w jakiej są dostępne w treści żądania, najlepszym rozwiązaniem jest określić największymi reguły dotyczące miejsc pochodzenia najpierw na liście tak, że są one obliczana jako pierwsza. Określ reguły, które są unikatowe — na przykład regułę, aby umożliwić wszystkich miejsc pochodzenia — na końcu listy.

### <a name="example--cors-rules-evaluation"></a>Przykład — CORS zasady oceny

W poniższym przykładzie pokazano częściowego treść dla operacji ustawić zasady CORS usług magazynu. Szczegółowe informacje na temat tworzenia żądania, zobacz [Ustawianie właściwości usługi obiektów Blob](https://msdn.microsoft.com/library/hh452235.aspx), [Ustawianie właściwości usługi kolejki](https://msdn.microsoft.com/library/hh452232.aspx)i [Ustawianie właściwości usługi tabeli](https://msdn.microsoft.com/library/hh452240.aspx) .

    <Cors>
        <CorsRule>
            <AllowedOrigins>http://www.contoso.com</AllowedOrigins>
            <AllowedMethods>PUT,HEAD</AllowedMethods>
            <MaxAgeInSeconds>5</MaxAgeInSeconds>
            <ExposedHeaders>x-ms-*</ExposedHeaders>
            <AllowedHeaders>x-ms-blob-content-type, x-ms-blob-content-disposition</AllowedHeaders>
        </CorsRule>
        <CorsRule>
            <AllowedOrigins>*</AllowedOrigins>
            <AllowedMethods>PUT,GET</AllowedMethods>
            <MaxAgeInSeconds>5</MaxAgeInSeconds>
            <ExposedHeaders>x-ms-*</ExposedHeaders>
            <AllowedHeaders>x-ms-blob-content-type, x-ms-blob-content-disposition</AllowedHeaders>
        </CorsRule>
        <CorsRule>
            <AllowedOrigins>http://www.contoso.com</AllowedOrigins>
            <AllowedMethods>GET</AllowedMethods>
            <MaxAgeInSeconds>5</MaxAgeInSeconds>
            <ExposedHeaders>x-ms-*</ExposedHeaders>
            <AllowedHeaders>x-ms-client-request-id</AllowedHeaders>
        </CorsRule>
    </Cors>

Następnie należy wziąć pod uwagę następujące żądania CORS:

Żądanie||| Odpowiedź||
---|---|---|---|---
**Metoda** |**Pochodzenia** |**Żądanie nagłówków** |**Uwzględnij reguły** |**Wynik**
**UMIESZCZENIE** | http://www.contoso.com |x-ms-obiektów blob typu zawartości | Pierwszą regułę |Sukcesu
**Pobierz** | http://www.contoso.com| x-ms-obiektów blob typu zawartości | Drugą regułę |Sukcesu
**Pobierz** | http://www.contoso.com| x-ms-obiektów blob typu zawartości | Drugą regułę | Błąd

Pierwsze żądanie odpowiada pierwszą regułę — domenie pochodzenia odpowiada dozwolonych miejsc pochodzenia, metodę dopasowuje dozwolone metody i nagłówek odpowiada dozwolonych nagłówki — i dlatego powiodło się.

Drugie żądanie nie odpowiada pierwszą regułę, ponieważ metoda nie pasuje do dozwolonych metod. Jednak odpowiadać drugą regułę, aby go zakończyło się powodzeniem.

Trzecia żądanie zastępuje drugą regułę w domenie pochodzenia i metody, więc żadnych dalszych reguł są obliczane. Jednak *Nagłówek x-ms klienta żądanie id* nie jest dozwolona przez drugą regułę, aby żądanie nie powiedzie się, mimo niedozwolonego znaczeń właściwych trzecia reguły czy mają być powiodła się.

>[AZURE.NOTE] Mimo że w tym przykładzie przedstawiono regułę unikatowe przed jeden bardziej restrykcyjnie, ogólnie najlepiej jest pierwsza lista największymi reguły.

## <a name="understanding-how-the-vary-header-is-set"></a>Opis konfiguracji nagłówka różne

Nagłówek *różne* jest standardowy nagłówek HTTP/1.1 składający się z zestawu pól nagłówka żądanie, które poinformowania agenta przeglądarki lub użytkownika informacje na temat kryteriów, które zostały wybrane przez serwer przetwarzania żądania. Nagłówek *różne* jest używana przede wszystkim w pamięci podręcznej przez serwery proxy, przeglądarki i sieci CDN, które należy określić, jak mają być buforowane odpowiedzi. Aby uzyskać szczegółowe informacje Zobacz specyfikacja [różne nagłówka](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).

Gdy przeglądarki lub innego agenta użytkownika buforowanie odpowiedź na żądanie CORS, domenie pochodzenia są buforowane jako punktu początkowego dozwolone. Jeżeli drugą domenę wydaje jednym żądaniu dla zasobu miejsca do magazynowania, gdy pamięci podręcznej jest aktywne, agenta użytkownika pobiera domenie pochodzenia pamięci podręcznej. Druga domena jest niezgodna domeny pamięci podręcznej, żądanie kończy się niepowodzeniem, gdy go w przeciwnym razie powiedzie się. W niektórych przypadkach magazyn Azure ustawia nagłówka różne **Origin** agenta użytkownika, aby wysłać wezwanie na kolejne CORS z usługą, gdy żądaniu domeny różni się od punktu początkowego pamięci podręcznej.

Azure magazynowania ustawia nagłówka *różne* **Origin** rzeczywiste żądania GET-głowy w następujących przypadkach:

- Gdy pochodzenie wniosku dokładnie pasuje do dozwolonych pochodzenie zdefiniowana przez regułę CORS. Aby uzyskać dokładne dopasowanie, reguły CORS nie może zawierać symboli wieloznacznych "*" znaków.

- Istnieje reguła nie pasujących pochodzenie wniosku, ale CORS włączono usługę Magazyn.

W przypadku których żądania GET-głowy pasuje regułę CORS, która zezwala wszystkich miejsc pochodzenia odpowiedź wskazuje, że wszystkie miejsc pochodzenia są dozwolone i pamięci podręcznej agenta użytkownika, aby umożliwić kolejne żądania z dowolnej domeny pochodzenia gdy pamięci podręcznej jest aktywna.

Należy zauważyć, że żądania przy użyciu metod innych niż GET-głowy, usług magazynu nie ustawiono nagłówka różne, ponieważ odpowiedzi na te metody nie są buforowane przez agentów użytkownika.

Poniższa tabela przedstawia sposób Azure magazynowania będą odpowiadać na żądania GET-głowy według wcześniej wspomniano przypadkach:

Żądanie|Ustawienia konta i wynik obliczeń przy użyciu reguł|||Odpowiedź|||
---|---|---|---|---|---|---|---|---
**Nagłówek Origin znajdują się na żądanie** | **Reguły CORS podane dla tej usługi** | **Reguły dopasowywania umożliwiającą umożliwia wszystkich miejsc pochodzenia (*)** | **Zgodne reguły istnieje dopasowanie dokładne pochodzenie** | **Odpowiedź zawiera nagłówek różne Ustaw pochodzenia** | **Odpowiedź zawiera Access Control-dozwolone-początkowego: "*"** | **Odpowiedź zawiera dostęp — kontrola-podsumowującymi-nagłówków**
Brak|Brak|Brak|Brak|Brak|Brak|Brak
Brak|Tak|Brak|Brak|Tak|Brak|Brak
Brak|Tak|Tak|Brak|Brak|Tak|Tak
Tak|Brak|Brak|Brak|Brak|Brak|Brak
Tak|Tak|Brak|Tak|Tak|Brak|Tak
Tak|Tak|Brak|Brak|Tak|Brak|Brak
Tak|Tak|Tak|Brak|Brak|Tak|Tak


## <a name="billing-for-cors-requests"></a>Rozliczenia dla żądania CORS

Pomyślne żądania wstępnej są zafakturowane po włączeniu CORS dla usług miejsca do magazynowania dla Twojego konta (dzwoniąc [Właściwości zestawu obiektów Blob usługi](https://msdn.microsoft.com/library/hh452235.aspx), [Ustawianie właściwości usługi kolejki](https://msdn.microsoft.com/library/hh452232.aspx)lub [Ustawić właściwości usługi tabeli](https://msdn.microsoft.com/library/hh452240.aspx)). Aby zminimalizować opłaty, rozważ ustawienie elementu **MaxAgeInSeconds** w reguł CORS na dużą wartość, aby agenta użytkownika buforowanie wezwanie.

Niepowodzenie żądania wstępnej nie będą naliczane.

## <a name="next-steps"></a>Następne kroki

[Ustawianie właściwości usługi obiektów Blob](https://msdn.microsoft.com/library/hh452235.aspx)

[Ustawianie właściwości usługi kolejki](https://msdn.microsoft.com/library/hh452232.aspx)

[Ustawianie właściwości usługi tabeli](https://msdn.microsoft.com/library/hh452240.aspx)

[Udostępnianie specyfikacja zasobów między Origin W3C](http://www.w3.org/TR/cors/)
