<properties
   pageTitle="Przewodnik tworzenia usługi danych dla Marketplace | Microsoft Azure"
   description="Szczegółowe informacje, jak tworzyć, certyfikowanie i wdrażanie usługi danych dla zakupu na Azure Marketplace."
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

   <tags
      ms.service="marketplace"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="na"
      ms.date="08/26/2016"
      ms.author="hascipio; avikova" />

# <a name="understanding-the-nodes-schema-for-mapping-an-existing-web-service-to-odata-through-csdl"></a>Opis schematu węzły do mapowania OData za pośrednictwem CSDL istniejącej usługi sieci web

>[AZURE.IMPORTANT] **W tej chwili firma Microsoft nie są już ułatwiającej rozpoczęcie korzystania wszelkie nowe wydawcy usługi danych. Nowy dataservices nie będzie uzyskiwanie zatwierdzany dla listy.** Jeśli chcesz opublikować na AppSource aplikacji biznesowych władz akredytacji bezpieczeństwa można znaleźć więcej informacji [w tym miejscu](https://appsource.microsoft.com/partners). Jeśli masz aplikacje IaaS lub chcesz opublikować na Azure Marketplace usługi Deweloper można znaleźć więcej informacji [w tym miejscu](https://azure.microsoft.com/marketplace/programs/certified/).

Ten dokument może pomóc w objaśnianie struktury węzeł do mapowania protokołu OData CSDL. Należy pamiętać, że struktura węzeł jest również utworzone XML. Dlatego schematu głównego, elementy nadrzędne i podrzędne dotyczy podczas projektowania mapowanie OData.

## <a name="ignored-elements"></a>Zignorowane elementy
Poniżej przedstawiono wysokiego poziomu elementów CSDL (węzłach XML), które nie mają być używane przez usługi Azure Marketplace wewnętrznej bazy danych podczas importowania metadanych usługi sieci web. Mogą występować, ale będą ignorowane.

| Element | Zakres |
|----|----|
| Za pomocą elementu | Węzeł, węzły podrzędne i wszystkich atrybutów |
| Dokumentacja elementu | Węzeł, węzły podrzędne i wszystkich atrybutów |
| ComplexType | Węzeł, węzły podrzędne i wszystkich atrybutów |
| Element skojarzenia | Węzeł, węzły podrzędne i wszystkich atrybutów |
| Właściwości rozszerzone | Węzeł, węzły podrzędne i wszystkich atrybutów |
| EntityContainer | Nie są uwzględniane następujące atrybuty: *rozszerza* i *AssociationSet* |
| Schematu | Nie są uwzględniane następujące atrybuty: *Namespace* |
| FunctionImport | Nie są uwzględniane następujące atrybuty: *Tryb* (przyjmowana jest wartość domyślna ln) |
| Dla obiektu | Tylko następujące węzły podrzędne są ignorowane: *klucz* i *PropertyRef* |

Poniżej opisano zmiany (dodane i zignorowane elementy) do różnych węzłów CSDL XML szczegółowo.

## <a name="functionimport-node"></a>Węzeł FunctionImport
Węzeł FunctionImport reprezentuje jeden adres URL (punktu wejścia), który udostępnia usługi do użytkownika końcowego. Węzeł pozwala przedstawiający, jak adres URL jest adresatem, które parametry są dostępne dla użytkowników końcowych i tych parametrów są dostarczane.

Szczegółowe informacje o tym węźle znajdują się w [tutaj] [MSDNFunctionImportLink]

[MSDNFunctionImportLink]: (https://msdn.microsoft.com/library/cc716710 (v=vs.100).aspx)

Oto dodatkowe atrybuty (lub uzupełnienia atrybuty) które są dostępne w węźle FunctionImport:

**d:BaseUri** -
szablon identyfikatora URI dla zasobu pozostałych uwidaczniany do witryny Marketplace. Marketplace korzysta z szablonu do konstruowania kwerend dla usługi sieci web REST. Szablon identyfikatora URI zawiera symbole zastępcze dla parametrów w postaci {nazwa parametru}, gdzie nazwa parametru jest nazwą parametru. Np. apiVersion = {apiVersion}.
Parametry mogą pojawić się jako identyfikator URI parametrów lub jako część ścieżki identyfikatora URI. W przypadku pojawienia się w ścieżce były zawsze obowiązkowe (nie można oznaczyć jako wartości null). *Przykład:*`d:BaseUri="http://api.MyWeb.com/Site/{url}/v1/visits?start={start}&amp;end={end}&amp;ApiKey=3fadcaa&amp;Format=XML"`

**Nazwa** - nazwę funkcji zaimportowane.  Nie może być taka sama, jak inne zdefiniowanych nazw w CSDL.  Np. Nazwa = "GetModelUsageFile"

**Zestawu** *(opcjonalnie)* — Jeśli funkcja zwraca zbiór typów jednostek wartości **zestawu** musi być jednostki Ustaw, do której należy kolekcji. W przeciwnym razie nie należy użyć atrybutu **zestawu** . *Przykład:*`EntitySet="GetUsageStatisticsEntitySet"`

**Zwracany_typ** *(Opcjonalnie)* - Określa typ elementów zwróconych przez identyfikator URI.  Nie należy używać tego atrybutu, jeśli funkcja nie zwraca wartości. Obsługiwane typy są następujące:

 - **Zbioru (<Entity type name>)**: określa zbiór typów zdefiniowanych jednostek. Nazwa występuje atrybut nazwy węzła dla obiektu. Przykładem jest zbioru (WXC. HourlyResult).
 - **Raw (<mime type>)**: Określa nieprzetworzonych dokumentu i blob, który został zwrócony do użytkownika. Przykład jest Raw(image/jpeg) inne przykłady:

  - ReturnType="Raw(text/plain)"
  - Zwracany_typ = "zbioru (sage. DeleteAllUsageFilesEntity) "*

**d:Paging** - określa sposób obsługi stronicowania przez zasób POZOSTAŁEJ. Wartości parametrów są używane w nawiasy gałęzi produkcji, np. strony = {$page} & wartość elementu itemsperpage = {$size} dostępne są następujące opcje:

- **Brak:** stronicowania nie jest dostępna
- **Pomiń:** stronicowania jest wyrażona za pomocą logiczne "Pomiń" i "Cofnij" (u góry). Pomiń przechodzi na elementy M i sporządzanie zwraca następnego elementy N. Wartość parametru: $skip
- **Wykonać:** Przejmowanie zwraca następnego elementy N. Wartość parametru: $take
- **PageSize:** stronicowania jest wyrażona za pomocą logicznej strony i rozmiar (elementy na stronie). Strona reprezentuje bieżącej strony, który został zwrócony. Wartość parametru: $page
- **Rozmiar:** rozmiar reprezentuje liczbę elementów zwróconych dla każdej strony. Wartość parametru: $size

**d:AllowedHttpMethods** *(Opcjonalnie)* - Określa, które czasownikowe jest obsługiwany przez zasób pozostałych. Ponadto ogranicza czasownikowe zaakceptowanych do określonej wartości.  Domyślne = wpisu.  *Przykład:* `d:AllowedHttpMethods="GET"` Dostępne są następujące opcje:

- **Pobieranie:** zazwyczaj używane do zwracania danych
- **Wpisu:** zazwyczaj służy do wstawiania nowych danych
- **Umieszczenie:** zazwyczaj używany do aktualizowania danych
- **Usuwanie:** służy do usuwania danych

Węzły podrzędne dodatkowe (nie ujęte w dokumentacji CSDL) w węźle FunctionImport są następujące:

**d:RequestBody** *(Opcjonalnie)* - treści żądania jest używany do wskazują, że żądania oczekuje treści do wysłania. Parametry mogą być podawane w treści wezwania. Są one np. wyrażone w nawiasach klamrowych {Nazwa parametru}. Parametry te są mapowane z użytkownikiem w treści przesyłanych do usługi dostawcy zawartości. RequestBody element ma atrybut o nazwie element httpMethod. Atrybut umożliwia dwie wartości:

- **WPIS:** Używane w przypadku żądania HTTP POST
- **Pobieranie:** Używane w przypadku żądanie HTTP GET

    Przykład:

        `<d:RequestBody d:httpMethod="POST">
        <![CDATA[
        <req1:Request xmlns:r1="http://schemas.mysite.com//generic/requests/1" Version="1.0">
        <req1:Query>{Query}</req1:Query><req1:AppId>D453474</req1:AppId>
        <req:DestinationSchemas><req1:Schema>Generic.RequestResponse[1.0]</req1:Schema>
        </req1:DestinationSchemas>
        </req1: Request>
        ]]>
        </d:RequestBody>`

**d:Namespaces** i **d:Namespace** — ten węzeł opisano nazw są definiowane w formacie XML, który został zwrócony przez importowanie funkcji (punkt końcowy Identifier). Plik XML, który został zwrócony przez usługę wewnętrznej bazy danych może zawierać dowolną liczbę nazw, aby odróżnić zawartości, która jest zwracana. **Wszystkie te obszary nazw, jeśli używane w kwerendach XPath d:Map lub d:Match muszą znajdować się na liście.** Węzeł d:Namespaces zawiera zestaw listę węzłów d:Namespace. Każdy z nich lista jeden nazw używany w odpowiedzi usług wewnętrznej bazy danych. Atrybut węzła d:Namespace są następujące:

-   **d:Prefix:** Prefiks obszaru nazw w wyniki XML zwrócone przez usługę, np. f:FirstName, f:LastName, gdzie f jest prefiks.
- **d:Uri:** Pełny identyfikator URI obszaru nazw używany w dokumencie wynik. Reprezentuje wartość, która odpowiada prefiks w czasie rzeczywistym.

**d:ErrorHandling** *(Opcjonalnie)* - ten węzeł zawiera warunki dotyczące obsługi błędów. Każdy z warunków jest sprawdzany na wynik, który został zwrócony przez usługę dostawcy zawartości. Jeśli warunek zastępuje proponowany kod błędu protokołu HTTP, który jest zwracany komunikat o błędzie do użytkownika końcowego.

**d:ErrorHandling** *(Opcjonalnie)* i **d:Condition** *(opcjonalnie)* - węzeł warunku zawiera jednym warunkiem, że jest zaznaczone pole wyboru w wyniku zwracane przez usługę dostawcy zawartości. **Wymagane** atrybuty są następujące:

- **d:Match:** Wyrażenie XPath, która sprawdza, czy danej węzeł i wartości znajduje się w danych wyjściowych dostawcy zawartości XML. Wyrażenie XPath jest wykonywana dane wyjściowe i powinna zwrócić wartość PRAWDA, jeśli warunek jest inny sposób dopasowania lub FAŁSZ.
- **d:HttpStatusCode:** Zastępuje kod stanu HTTP, który powinien być zwrócony przez Marketplace w przypadku warunek. Marketplace signalizes błędy za pośrednictwem protokołu HTTP kody stanu. Lista kodów stanu HTTP są dostępne na http://en.wikipedia.org/wiki/HTTP_status_code
- **d:ErrorMessage:** Komunikat o błędzie, który został zwrócony — z kodem stanu HTTP — do użytkownika końcowego. Należy to przyjazne komunikaty o błędach nie zawierającym wszystkie hasła.

**d:title** *(Opcjonalnie)* - umożliwia opisem nazwy funkcji. Wartość w polu tytuł pochodzi z

- Atrybut opcjonalne mapy (xpath) określającą, gdzie znaleźć tytuł w odpowiedzi zwrócony przez żądanie usługi.
- - Lub - tytułu określony jako wartość węzła.

**d:Rights** *(Opcjonalnie)* - (np. praw autorskich) prawa skojarzone z funkcją. Wartość w polu praw pochodzi:

- Atrybut opcjonalne mapy (xpath) określającą, gdzie znaleźć praw w odpowiedzi zwrócony przez żądanie usługi.
-   - Lub - praw określony jako wartość węzła.

**d:Description** *(Opcjonalnie)* - krótki opis funkcji. Wartość w polu Opis pochodzi z

- Atrybut opcjonalne mapy (xpath) określającą, gdzie znaleźć opis w odpowiedzi zwrócony przez żądanie usługi.
- — Lub — opis określony jako wartość węzła.

**d:EmitSelfLink** - *Zobacz powyżej przykładzie "FunctionImport"Stronicowanie"za pośrednictwem zwrócone dane"*

**d:EncodeParameterValue** — opcjonalne rozszerzenie OData

**d:QueryResourceCost** — opcjonalne rozszerzenie OData

**d:Map** — opcjonalne rozszerzenie OData

**d:Headers** — opcjonalne rozszerzenie OData

**d:Headers** — opcjonalne rozszerzenie OData

**d:Value** — opcjonalne rozszerzenie OData

**d:HttpStatusCode** — opcjonalne rozszerzenie OData

**d:ErrorMessage** — opcjonalne rozszerzenie OData

## <a name="parameter-node"></a>Węzeł parametru

Ten węzeł zawiera jeden parametr uwidaczniany jako część szablonu URI / treści, który został określony w węźle FunctionImport żądania.

Strony dokumentu bardzo przydatne szczegółowe informacje o węzeł "Element parametru" znajduje się na [poniżej](http://msdn.microsoft.com/library/ee473431.aspx) (Użyj menu rozwijanego **Innych wersji** do wybierz inną wersję, jeśli to konieczne wyświetlić dokumentację). *Przykład:*`<Parameter Name="Query" Nullable="false" Mode="In" Type="String" d:Description="Query" d:SampleValues="Rudy Duck" d:EncodeParameterValue="true" MaxLength="255" FixedLength="false" Unicode="false" annotation:StoreGeneratedPattern="Identity"/>`

| Atrybut parametru | Wymagane jest | Wartość |
|----|----|----|
| Nazwa | Tak | Nazwa parametru. Uwzględnij wielkość liter!  Uwzględnij wielkość liter BaseUri. **Przykład:**`<Property Name="IsDormant" Type="Byte" />` |
| Typ | Tak | Typ parametru. Wartość musi być **EDMSimpleType** lub typ złożony, który znajduje się w zakresie modelu. Aby uzyskać więcej informacji zobacz "6 typy obsługiwane parametru-właściwości".  (Jest uwzględniana wielkość liter! Pierwszy znak jest wielkie litery, pozostałe są małe litery).  Zobacz też, [koncepcyjny Model typy (CSDL)] [MSDNParameterLink]. **Przykład:**`<Property Name="LimitedPartnershipID " Type="Int32" />` |
| Tryb | Brak | **W**, lub wartość i wynik w zależności od tego, czy parametr jest wejścia, wyjścia lub parametr wejścia i wyjścia. (Tylko "W" jest dostępna w Azure Marketplace). **Przykład:**`<Parameter Name="StudentID" Mode="In" Type="Int32" />` |
| MaxLength | Brak | Maksymalna dozwolona długość parametru. **Przykład:**`<Property Name="URI" Type="String" MaxLength="100" FixedLength="false" Unicode="false" />` |
| Dokładność | Brak | Dokładność parametru. **Przykład:**`<Property Name="PreviousDate" Type="DateTime" Precision="0" />` |
| Skala | Brak | Skala parametru. **Przykład:**`<Property Name="SICCode" Type="Decimal" Precision="10" Scale="0" />` |

[MSDNParameterLink]: (http://msdn.microsoft.com/library/bb399548 (v=VS.100).aspx)

Atrybuty, które zostały dodane do specyfikacji CSDL są następujące:

| Atrybut parametru | Opis |
|----|----|
| **d:regex** *(Opcjonalnie)* | Zaświadczenie regex używany do sprawdzania poprawności wprowadzania wartości parametru. Jeśli nie są zgodne z instrukcji wartości wejściowej wartość się niepowodzeniem. Dzięki temu może także określić zestaw możliwych wartości, np. ^ [0-9] +? $ Zezwalaj tylko liczby. **Przykład:** "< Nazwa parametru ="Nazwa"tryb ="W"typ ="Ciąg"d: wartości null = d:Regex"wartość false"=" ^ [a-zA-Z] * zł "d:Description ="na nazwę, która nie może zawierać spacji ani znaków innej niż angielska alfa"d:SampleValues ="Jerzy|Jan|Thomas|Michał "->" |
| **d:Enum** *(Opcjonalnie)* | Potok oddzielone listy wartości prawidłowych parametru. Typ wartości musi odpowiadać zdefiniowane typ parametru. Przykład: "angielski|Metryka|nieprzetworzonych`. Enum will display as a selectable dropdown list of parameters in the UI (service explorer). **Example:** `< Nazwa parametru = "Czas trwania" typ = "Ciąg" Tryb = "Nullable w" = "true" d:Enum = "1 rok|5years|10years "/ >" |
| **d: wartości null** *(Opcjonalnie)* | Umożliwia definiowanie, czy parametr może mieć wartości null. Wartość domyślna to: PRAWDA. Jednak parametry, które są dostępne jako część ścieżki w szablonie URI nie może mieć wartości null. Gdy atrybut jest ustawiona na wartość FAŁSZ dla tych parametrów — dane wprowadzone przez użytkownika jest wyłączona. **Przykład:**`<Parameter Name="BikeType" Type="String" Mode="In" Nullable="false"/>` |
| **d:SampleValue** *(Opcjonalnie)* | Przykładowa wartość ma być wyświetlany jako notatkę do klienta w interfejsie użytkownika.  Umożliwia dodawanie kilku wartości przy użyciu listy potoku oddzielone, to znaczy "|b|c` **Example:** `< Nazwa parametru = "BikeOwner" typ = "Ciąg" tryb = "W" d:SampleValues = "Jerzy|Jan|Thomas|Michał "->" |

## <a name="entitytype-node"></a>Węzeł dla obiektu

Ten węzeł reprezentuje jedną typów, które są zwracane z witryny Marketplace do użytkownika końcowego. Zawiera on również mapowania z zestawu wyników, który został zwrócony przez usługę dostawcy zawartości do wartości, które są zwracane do użytkownika końcowego.

Szczegółowe informacje o tym węźle znajdują się w [poniżej](http://msdn.microsoft.com/library/bb399206.aspx) (Użyj menu rozwijanego **Innych wersji** do wybierz inną wersję, jeśli to konieczne wyświetlić dokumentację.)

| Nazwa atrybutu | Wymagane jest | Wartość |
|----|----|----|
| Nazwa | Tak | Nazwa typu obiektu. **Przykład:**`<EntityType Name="ListOfAllEntities" d:Map="//EntityModel">` |
| Typ bazy | Brak | Nazwa innego typu obiektu, który jest typem bazowym typu obiektu, który jest definiowana. **Przykład:**`<EntityType Name="PhoneRecord" BaseType="dqs:RequestRecord">` |

Atrybuty, które zostały dodane do specyfikacji CSDL są następujące:

**d:Map** - wyrażenie XPath wykonywane wynik usługi. W tym miejscu zakłada się, że wynik usługi zawiera zestaw elementów, które powtarzają, takie jak ATOM kanałów gdzie ustawiono węzły wpis, które powtarzają. Każdy z tych powtarzających się węzły zawiera jeden rekord. Następnie określono wyrażenie XPath, aby wskazywały na poszczególnych węzeł powtarzających się w wyniku usługi dostawcy zawartości, który przechowuje wartości dla poszczególnych rekordów. Przykład z usługi:

        `<foo>
          <bar> … content … </bar>
          <bar> … content … </bar>
          <bar> … content … </bar>
        </foo>`

Wyrażenie XPath może być /foo/paska, ponieważ każdy paska węzeł jest węzeł powtarzających się w wyniku kwerendy i zawiera rzeczywistej zawartości, który został zwrócony do użytkownika końcowego.

**Klucz** — ten atrybut jest ignorowany przez Marketplace. Umieść podstawie usług sieci web, na ogół nie uwidaczniają klucza podstawowego.


## <a name="property-node"></a>Węzeł property

Ten węzeł zawiera jedną właściwość rekordu.

Szczegółowe informacje o tym węźle znajdują się w [http://msdn.microsoft.com/library/bb399546.aspx](http://msdn.microsoft.com/library/bb399546.aspx) (Użyj menu rozwijanego **Innych wersji** do wybierz inną wersję, jeśli to konieczne wyświetlić dokumentację.) *Przykład:*
        `<EntityType Name="MetaDataEntityType" d:Map="/MyXMLPath">
        <Property Name="Name"   Type="String" Nullable="true" d:Map="./Service/Name" d:IsPrimaryKey="true" DefaultValue=”Joe Doh” MaxLength="25" FixedLength="true" />
        ...
        </EntityType>`

| Nazwa_atrybutu | Wymagane | Wartość |
|----|----|----|
| Nazwa | Tak | Nazwa właściwości. |
| Typ | Tak | Typ wartości właściwości. Typ wartości właściwości musi być **EDMSimpleType** lub typ złożonych (wskazywany przez w pełni kwalifikowaną nazwę), który jest zakresem modelu. Aby uzyskać więcej informacji zobacz Typy Model koncepcyjny (CSDL). |
| Wartości null | Brak | **PRAWDA** (wartość domyślna) lub **FAŁSZ,** w zależności od tego, czy właściwości mogą mieć wartości null. Uwaga: W wersji CSDL wskazywana przez nazw [http://schemas.microsoft.com/ado/2006/04/edm](http://schemas.microsoft.com/ado/2006/04/edm) właściwości złożonych typ musi mieć Nullable = "False". |
| Wartość domyślna | Brak | Wartość domyślna właściwości. |
|MaxLength | Brak | Maksymalna długość wartości właściwości. |
| Dla wpisu | Brak | **Wartość PRAWDA** lub **FAŁSZ** w zależności od tego, czy wartość właściwości będą przechowywane jako ciąg długości fiexed. |
| Dokładność | Brak | Odwołuje się do maksymalną liczbę cyfr, aby utrzymać na wartość liczbową. |
| Skala | Brak | Maksymalna liczba miejsc dziesiętnych, aby utrzymać na wartość liczbową. |
| Unicode | Brak | **Wartość PRAWDA** lub **FAŁSZ** w zależności od tego, czy wartość właściwości być przechowywane jako ciąg znaków Unicode. |
| Sortowanie | Brak | Ciąg, który określa kolejność sortowania może być używany w źródle danych. |
| Tryb | Brak | **Brak** (wartość domyślna) lub **Stała**. Jeśli wartość jest ustawiona na **stałe**, wartość właściwości zostanie użyte w testy optymistyczny współbieżności. |

Poniżej przedstawiono dodatkowe atrybuty, które zostały dodane do specyfikacji CSDL:

**d:Map** - wyrażenie XPath wykonywane usługę wyjściowy i wyodrębnia jedną właściwość danych wyjściowych. Wyrażenie XPath określony jest względem węzeł powtarzających się zaznaczonego na XPath węzła dla obiektu. Użytkownik może również określić bezwzględne XPath umożliwia tym statyczne zasobu w węzłach dane wyjściowe, takie jak na przykład informację o prawach autorskich, której tylko raz w usłudze oryginalny wyjściowy, ale powinny znajdować się w każdej z wierszy w wynikach OData. Przykład z usługi:

        `<foo>
          <bar>
           <baz0>… value …</baz0>
           <baz1>… value …</baz1>
           <baz2>… value …</baz2>
          </bar>
        </foo>`

Wyrażenie XPath w tym miejscu będzie ./bar/baz0 uzyskać węzeł baz0 z usługi dostawcy zawartości.

**d:CharMaxLength** - typu ciąg można określić maksymalna długość. Zobacz przykład CSDL usługi danych

**d:IsPrimaryKey** - wskazuje, czy kolumna jest klucza podstawowego w tabeli i widoku. Zobacz przykład CSDL usługi danych.

**d:isExposed** - Określa, jeśli są udostępniane schematu tabeli (zazwyczaj PRAWDA). Zobacz przykład CSDL usługi danych

**d:IsView** *(Opcjonalnie)* - wartość PRAWDA, jeśli to jest oparty na widoku zamiast tabeli.  Zobacz przykład CSDL usługi danych

**d:Tableschema** — Zobacz przykład CSDL usługi danych

**d:ColumnName** — to nazwa kolumny w tabeli i widoku.  Zobacz przykład CSDL usługi danych

**d:IsReturned** - jest logiczną, która określa, jeśli usługa udostępnia tę wartość do klienta.  Zobacz przykład CSDL usługi danych

**d:IsQueryable** - jest wartością logiczną, która określa Jeśli kolumny mogą być używane w kwerendzie bazy danych.   Zobacz przykład CSDL usługi danych

**d:OrdinalPosition** — to liczbowe pozycję kolumny wyglądu, x, a w tabeli lub widoku, gdzie x jest od 1 do liczby kolumn w tabeli.  Zobacz przykład CSDL usługi danych

**d:DatabaseDataType** - jest typu danych kolumny w bazie danych, to znaczy typ danych SQL. Zobacz przykład CSDL usługi danych

## <a name="supported-parametersproperty-types"></a>Typy obsługiwanego parametrów i właściwości
Oto obsługiwane typy dla parametrów i właściwości. (Jest uwzględniana wielkość liter)

| Typy pierwotne | Opis |
|----|----|
| Wartości null | Reprezentuje braku wartości. |
| Wartość logiczna | Reprezentuje pojęcia matematyczne logiki przechowywanymi w postaci dwójkowej.|
| Bajt | Liczba całkowita bez znaku 8-bitową wartość|
|Daty i godziny| Przedstawia datę i godzinę z wartości z zakresu od północy 12:00:00, 1 styczeń 1753 A.D. za pośrednictwem 11:59:59 godzinach od, A.D. 9999 grudnia|
|Dziesiętna | Reprezentuje wartości liczbowe z stałej dokładności i skali. Ten typ można opisać wartość liczbową od ujemne 10 ^ 255 + 1 do 10 dodatnia ^ 255 -1|
| Naciśnij dwukrotnie | Reprezentuje punkt przestawny liczb z dokładnością do 15 cyfr, reprezentujące wartości przy użyciu zakresie rzędu temperaturze 2, 23E-308 do 1, 79E + 308. **Używanie dziesiętnych z powodu problemu eksportu Exel**|
| Pojedynczy | Reprezentuje punkt przestawny numer dokładności 7 cyfr, reprezentujące wartości przy użyciu przybliżonego zakresem temperaturze 1.18e-38 do 3, 40E + 38|
|Identyfikator GUID |Reprezentuje wartość 16-bajtowa (128-bitowego) Unikatowy identyfikator |
|Int16|Reprezentuje wartość całkowita 16-bitowa |
|Int32|Reprezentuje wartość całkowita 32-bitowa |
|Int64|Reprezentuje wartość całkowita 64-bitowa |
|Ciąg | Reprezentuje lub zmienna — danych stałej długości znaków |


## <a name="see-also"></a>Zobacz też
- Jeśli interesuje Cię opis ogólny proces mapowania OData i przeznaczenie, w tym artykule [Mapowania OData usługi danych](marketplace-publishing-data-service-creation-odata-mapping.md) do przejrzenia definicje, struktur i instrukcje.
- Jeśli jesteś zrecenzować przykładów, przeczytaj ten artykuł [Przykłady mapowania danych usługi OData](marketplace-publishing-data-service-creation-odata-mapping-examples.md) , aby wyświetlić przykładowy kod i opis Składnia kodu i kontekst.
- Aby powrócić do określonej ścieżki do publikowania danych usługi Azure Marketplace, w tym artykule [Podręcznik publikowania usług danych](marketplace-publishing-data-service-creation.md).
