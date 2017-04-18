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

# <a name="mapping-an-existing-web-service-to-odata-through-csdl"></a>Mapowanie istniejącej usługi sieci web na OData za pośrednictwem CSDL

>[AZURE.IMPORTANT] **W tej chwili firma Microsoft nie są już ułatwiającej rozpoczęcie korzystania wszelkie nowe wydawcy usługi danych. Nowy dataservices nie będzie uzyskiwanie zatwierdzany dla listy.** Jeśli chcesz opublikować na AppSource aplikacji biznesowych władz akredytacji bezpieczeństwa można znaleźć więcej informacji [w tym miejscu](https://appsource.microsoft.com/partners). Jeśli masz aplikacje IaaS lub chcesz opublikować na Azure Marketplace usługi Deweloper można znaleźć więcej informacji [w tym miejscu](https://azure.microsoft.com/marketplace/programs/certified/).

Ten artykuł zawiera omówienie dotyczące używania CSDL do mapowania istniejąca Usługa zgodna usługi OData. Objaśniono, jak tworzenie dokumentu mapowania (CSDL) który przekształca wprowadzania żądanie od klienta za pośrednictwem połączenia usługi i wynik (dane) z powrotem do klienta za pośrednictwem OData zgodne kanału. Firmy Microsoft Azure Marketplace udostępnia usługi dla użytkownika końcowego przy użyciu protokołu OData. Usługi, które są dostępne przez dostawców zawartości (właścicieli danych) są dostępne w różnych formularzy, takich jak REST, SOAP, itp.

## <a name="what-is-a-csdl-and-its-structure"></a>Co to jest CSDL i jego strukturę?
CSDL (koncepcyjny schematu Definition Language) to specyfikacja definiowanie sposobu opisują lub usługa sieci web bazy danych w typowych XML sposób wyrażania usługą Azure Marketplace.

Prosta omówienie **żądanie przepływu:**

  `Client -> Azure Marketplace -> Content Provider’s Web Service (Get, Post, Delete, Put)`

**Przepływ danych** znajduje się w przeciwnym kierunku:

  `Client <- Azure Marketplace <- Content Provider’s WebService`

**Rysunek 1** diagramy, jak klienta może uzyskać danych z dostawcą zawartości (usługa) za pośrednictwem usługi Azure Marketplace.  CSDL jest używana przez składnik Mapowanie/przekształcenia do obsługi żądania i przekazywania danych między zawartości dostawcy usług i żądającego klienta.

*Rysunek 1: Szczegółowe przepływu z klienta do dostawcy zawartości za pośrednictwem usługi Azure Marketplace*

  ![Rysunek](media/marketplace-publishing-data-service-creation-odata-mapping/figure-1.png)

Aby uzyskać podstawowe informacje dotyczące Atom, Atom Pub i protokołu OData, na którym utworzyć rozszerzenia Azure Marketplace, należy przejrzeć: [http://msdn.microsoft.com/library/ff478141.aspx](http://msdn.microsoft.com/library/ff478141.aspx)

Fragment miejsc położonych powyżej łącze:     *"celem protokołu danych otwartych (dalej nazywany OData) jest oparte na pozostałych protokołu służą styl OBSŁUGIWAŁ operacji (Tworzenie, Odczyt, aktualizację i usuwanie) przed zasobów udostępniany jako usług danych. "Usługi danych" jest punktem końcowym przypadku danych uwidocznionych z jedną lub więcej "kolekcji" każdego z zero lub więcej "wpisów", które składają się z pary o nazwie wartość wpisana. OData jest opublikowany przez firmę Microsoft w obszarze Normalizacyjna OASIS (organizacji przejścia z strukturalnych informacji normy) tak, aby każda osoba, która chce można tworzyć serwerów, klientów lub narzędzia bez ograniczeń i należności licencyjne."*

### <a name="three-critical-pieces-that-have-to-be-defined-by-the-csdl-are"></a>Są trzy najistotniejsze, które mają określone przez CSDL:

- **Punkt końcowy** : usługa dostawcy sieci Web adres (URI) usługi
- Przekazywane jako danych wejściowych dla dostawcy usługi **Parametry danych** definicji parametrów są wysyłane do usługi dostawcy zawartości w dół do typu danych.
- **Schemat** danych zwracanych żądania usługi w schemacie dane są dostarczane przez usługę dostawcy zawartości, w tym kontenerze, zbiory i tabele, zmienne/kolumn i typy danych.

Na poniższym diagramie przedstawiono przegląd przepływ z miejsce, w którym klient wprowadzana instrukcji OData (połączenie do usługi sieci web dostawcy zawartości) do pobierania wyników i ponownie.

  ![Rysunek](media/marketplace-publishing-data-service-creation-odata-mapping/figure-2.png)

### <a name="steps"></a>Kroki:

1. Klient wysyła żądanie za pośrednictwem połączenia usługi zawierający dane wejściowe parametrów zdefiniowanych w XML do usługi Azure Marketplace
2. CSDL służy do sprawdzenia wywołania usługi.
    - Sformatowany zgłoszenia następnie są wysyłane do usługi dostawców zawartości, Azure Marketplace
3. Webservice wykonuje i preforms akcji zlecenie Http (to znaczy GET) dane są zwracane do usługi Azure Marketplace, gdzie żądanych danych (jeśli istnieje) jest umożliwia uzyskanie dostępu w formacie XML do klienta przy użyciu mapowania zdefiniowane w CSDL.
4. Klient przesłaniu danych (jeśli istnieje) w formacie XML lub JSON

## <a name="definitions"></a>Definicje

### <a name="odata-atom-pub"></a>Pub OData ATOM

Ustaw rozszerzenie pub ATOM, gdzie każdy zapis reprezentuje jeden wiersz wyniku. Część zawartości tego wpisu jest rozszerzony zawiera wartości w wierszu — jako pary wartości klucza. Więcej informacji znajduje się tutaj: [https://www.odata.org/documentation/odata-version-3-0/atom-format/](https://www.odata.org/documentation/odata-version-3-0/atom-format/)

### <a name="csdl---conceptual-schema-definition-language"></a>CSDL - języka definicji schematu koncepcyjny

Umożliwia definiowanie funkcje (SPROCs) i obiektów, które są dostępne za pośrednictwem bazy danych. Więcej informacji znaleźć tutaj: [http://msdn.microsoft.com/library/bb399292.aspx](http://msdn.microsoft.com/library/bb399292.aspx)  

> [AZURE.TIP] Kliknij listę rozwijaną **innych wersji** i wybierz wersję, jeśli nie widzisz tego artykułu.

### <a name="edm---entry-data-model"></a>EDM - wpis modelu danych
- Omówienie: [http://msdn.microsoft.com/library/vstudio/ee382825 (v=vs.100).aspx][OverviewLink] [OverviewLink]: http://msdn.microsoft.com/library/vstudio/ee382825 (v=vs.100).aspx
- Podgląd: [http://msdn.microsoft.com/library/aa697428 (v=vs.80).aspx][PreviewLink] [PreviewLink]: http://msdn.microsoft.com/library/aa697428 (v=vs.80).aspx
- Typy danych: [http://msdn.microsoft.com/library/bb399548 (v=VS.100).aspx][DataTypesLink] [DataTypesLink]: http://msdn.microsoft.com/library/bb399548 (v=VS.100).aspx

Tworzenie kopii poniższej szczegółowe od lewej do prawej przepływ w miejsce, w którym klient wprowadza instrukcji OData (połączenie do usługi sieci web dostawcy zawartości) do uzyskania wyników i danych:

  ![Rysunek](media/marketplace-publishing-data-service-creation-odata-mapping/figure-3.png)


## <a name="csdl-basics"></a>Podstawowe informacje o CSDL

CSDL (koncepcyjny schematu Definition Language) to specyfikacja definiowanie sposobu opisują lub usługa sieci web bazy danych w typowych XML sposób wyrażania usługą Azure Marketplace. CSDL w tym artykule opisano krytycznej kawałków, który **umożliwia przekazywanie danych ze źródła danych do usługi Azure Marketplace.** Poniżej opisano główne elementy:

- Interfejs informacje opisujące wszystkie funkcje publicznie (FunctionImport węzeł)
- Informacje o typie danych dla wszystkich wiadomości requests(input) i responses(outputs) wiadomości (EntityContainer-zestawu-dla obiektu węzłach)
- Wiązanie informacji na temat protokół transportowy być używane (nagłówek węzeł)
- Adres do lokalizacji określonej usługi (atrybut BaseURI)

CSDL reprezentuje w skrócie umowy platformy i język niezależnie między żądającego usługi i usługodawcy. Przy użyciu CSDL, klienta można zlokalizować usługi bazy danych i usługi sieci web i jej publicznie dostępne funkcje wywołania.

### <a name="relating-a-csdl-to-a-database-or-a-collection"></a>Dotyczących CSDL bazy danych lub kolekcji
**Specyfikacja CSDL**

CSDL jest gramatyki XML do opisu usługi sieci web. Specyfikacja sam jest podzielony na 4 głównych elementów: zestawu, FunctionImport; Przestrzeń nazw, a dla obiektu.

Aby ułatwić zrozumienie umożliwia ten abstrakcji dotyczą CSDL tabeli.

Pamiętaj;

  CSDL reprezentuje umowy platformy i język niezależnie między **żądającego usługi** i **usługodawcy**. Używanie CSDL, **klienta** można zlokalizować **usługi bazy danych i usługi sieci web** i wywołać dowolną z jej publicznie **funkcji.**

Usługi danych cztery części CSDL można traktować bazy danych, tabel, kolumn i procedury ze sklepu.

Odnoszących się one w następujący sposób dla usługi danych:

- EntityContainer ~ = bazy danych
- Zestawu ~ = tabeli
- Dla obiektu ~ = kolumny
- FunctionImport ~ = procedura składowana

**Dozwolone zleceń HTTP**
- Uzyskiwanie — zwraca wartości z bazy danych, (zwraca kolekcję)
- Publikowanie — używany do przekazywania danych i opcjonalnie wartości zwracane z bazy danych (Tworzenie nowego wpisu w zbiorze zwrotu id Identifier)
- USUWANIE — usuwanie danych z bazy danych, (usuwa zbiór)
- UMIESZCZENIE — aktualizowanie danych w bazie danych (Zamień zbioru lub utworzyć)

## <a name="metadatamapping-document"></a>Dokument metadanych i mapowania

Dokument metadanych i mapowania służy do mapować istniejących usług sieci web dostawcy zawartości, dzięki czemu można je wyświetlać jako usługi sieci web OData przez system Azure Marketplace. Jest oparty na CSDL i narzędzi kilka rozszerzenia CSDL, aby zezwalały na potrzeby pozostałych podstawie usług sieci web dostępne za pośrednictwem usługi Azure Marketplace. Pole rozszerzenia znajdują się w obszarze nazw [http://schemas.microsoft.com/dallas/2010/04](http://schemas.microsoft.com/dallas/2010/04) .

Poniżej przedstawiono przykład CSDL: (kopiowania i wklejania poniższym przykładzie CSDL w edytorze XML i zmień zgodnie z tej usługi.  Następnie wkleić do mapowania CSDL na karcie usługi danych podczas tworzenia usługi w [Portal publikowania Azure Marketplace](https://publish.windowsazure.com)).

**Terminów:** Dotyczące warunków CSDL [Portal publikowania](https://publish.windowsazure.com) warunków interfejsu użytkownika (PPUI).
- Oferują "Tytuł" PPUI odnosi się do MyWebOffer
- Moja firma w PPUI odnosi się do **Nazwy wyświetlanej w programie Publisher** w [Centrum deweloperów programu Microsoft](http://dev.windows.com/registration?accountprogram=azure) interfejsu użytkownika
- Z interfejsu API odnosi się do sieci Web lub usługi danych (Plan w PPUI)

**Hierarchia:** 
 firma (dostawca zawartości) jest właścicielem oferty, które mają plany, takich jak service(s), które wiersz w górę z interfejsem API.

### <a name="webservice-csdl-example"></a>Przykład WebService CSDL

Nawiązuje połączenie z usługą jest Uwidacznianie punktu końcowego aplikacji sieci web (na przykład aplikacji C#)

        <?xml version="1.0" encoding="utf-8"?>
        <!-- The namespace attribute below is used by our system to generate C#. You can change “MyCompany.MyOffer” to something that makes sense for you, but change “MyOffer” consistently throughout the document. -->
        <Schema Namespace="MyCompany.MyWebOffer" Alias="MyOffer" xmlns="http://schemas.microsoft.com/ado/2009/08/edm" xmlns:d="http://schemas.microsoft.com/dallas/2010/04" >
        <!-- EntityContainer groups all the web service calls together into a single offering. Every web service call has a FunctionImport definition. -->
          <EntityContainer Name="MyOffer">
        <!-- EntitySet is defined for CSDL compatibility reasons, not required for ReturnType=”Raw”
        @Name is used as reference by FunctionImport @EntitySet. And is used in the customer facing UI as name of the Service.
        @EntityType is used to point at the type definition near the bottom of this file. -->
            <EntitySet Name="MyEntities" EntityType="MyOffer.MyEntityType" />
        <!-- Add a FunctionImport for every service method. Multiple FunctionImports can share a single return type (EntityType). -->
        <!-- ReturnType is either Raw() for a stream or Collection() for an Atom feed. Ex. of Raw: ReturnType=”Raw(text/plain)” -->
        <!—EntitySet is the entityset defined above, and is needed if ReturnType is not Raw -->
        <!-- BaseURI attribute defines the service call, replace & with the encode value (&amp;).
        In the input name value pairs {param} represents passed in value.
        Or the value can be hard coded as with AccountKey. -->
        <!-- AllowedHttpMethods optional (default = “GET”), allows the CSDL to specifically specify the verb of the service, “Get”, “Post”, “Put”, or “Delete”. -->
        <!-- EncodeParameterValues, True encodes the parameter values, false does not. -->
        <!-- BaseURI is translated into an URITemplate which defines how the web service call is exposed to marketplace customers.
        Ex. https://api.datamarket.azure.com/mycompany/MyOfferPlan?name={name}
        BaseURI is XML encoded, the {...} point to the parameters defined below.
        Marketplace will read the parameters from this URITemplate and fill the values into the corresponding parameters of the BaseUri or RequestBody (below) during calls to your service.  
        It is okay for @d:BaseUri to include information only for Marketplace consumption, it will not be exposed to end users. i.e. the hardcoded AccountKey in the above BaseURI does not show up in the client facing URITemplate. -->
            <FunctionImport Name="MyWebServiceMethod"
                            EntitySet="MyEntities"
                            ReturnType="Collection(MyOffer.MyEntityType)"
        d:AllowedHttpMethods="GET"
        d:EncodeParameterValues="true"
        d:BaseUri="http://services.organization.net/MyServicePath?name={name}&amp;AccountKey=22AC643">
        <!-- Definition of the RequestBody is only required for HTTP POST requests and is optional for HTTP GET requests. -->
        <d:RequestBody d:httpMethod="POST">
                <!-- Use {} for placeholders to insert parameters. -->
                <!-- This example uses SOAP formatting, but any POST body can be used. -->
            <!-- This example shows how to pass userid and password via the header -->
                <![CDATA[<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:MyOffer="http://services.organization.net/MyServicePath">
                  <soapenv:Header/>
                  <soapenv:Body>
                    <MyOffer:ws_MyWebServiceMethod>
                      <myWebServiceMethodRequest>
                        <!--This information is not exposed to end users. -->
                        <UserId>userid</UserId>
                        <Password>password</Password>
                        <!-- {name} is replaced with the value read from @d:UriTemplate above -->
                        <Name>{name}</Name>
                        <!-- Parameters can be used more than once and are not limited to appearing as the value of an element -->
                        <CustomField Name="{name}" />
                        <MyField>Static content</MyField>
                      </myWebServiceMethodRequest>
                    </MyOffer:ws_MyWebServiceMethod>
                  </soapenv:Body>
                </soapenv:Envelope>      
              ]]>
        </d:RequestBody>
        <!-- Title, Rights and Description are optional and used to specify values to insert into the ATOM feed returned to the end user.  You can specify the element to contain a fixed message by providing a value for the element (this is the default value).  @d:Map is an XPath expression that points into the response returned by your service and is optional.  -->
        <d:Title d:Map="/MyResponse/Title">Default title.</d:Title>
        <d:Rights>© My copyright. This is a fixed response. It is okay to also add a d:Map attribute to override this text.</d:Rights>
        <d:Description d:Map="/MyResponse/Description"></d:Description>
        <d:Namespaces>
        <d:Namespace d:Prefix="p"  d:Uri="http://schemas.organization.net/2010/04/myNamespace" />
        <d:Namespace d:Prefix="p2" d:Uri="http://schemas.organization.net/2010/04/MyNamespace2" />
        </d:Namespaces>
        <!-- Parameters of the web service call:
        @Name should match exactly (case sensitive) the {…} placeholders in the @d:BaseUri, @d:UriTemplate, and d:RequestBody, i.e. “name” parameter in above BaseURI.
        @Mode is always "In", compatibility with CSDL
        @Type is the EDM.SimpleType of the parameter, see http://msdn.microsoft.com/library/bb399548(v=VS.100).aspx
        @d:Nullable indicates whether the parameter is required.
        @d:Regex - optional, attribute to describe the string, limiting unwanted input at the entry of the system
        @d:Description - optional, is used by Service Explorer as help information
        @d:SampleValues - optional, is used by Service Explorer as help information. Multiple Sample values are separated by '|', e.g. "804735132|234534224|23409823234"
        @d:Enum - optional for string type. Contains an enumeration of possible values separated by a '|', e.g. d:enum="english|metric|raw". Will be converted in a dropdown list in the Service Explorer.
        -->
        <Parameter name="name" Mode="In" Type="String" d:Nullable="false" d:Regex="^[a-zA-Z]*$" d:Description="A name that cannot contain any spaces or non-alpha non-English characters"
        d:Enum="George|John|Thomas|James"
        d:SampleValues="George"/>
        <Parameter Name=" AccountKey" Mode="In" Type="String" d:Nullable="false" />

        <!-- d:ErrorHandling is an optional element. Use it define standardized errors by evaluating the service response. -->
        <d:ErrorHandling>
        <!-- Any number of d:Condition elements are allowed, they are evaluated in the order listed.
        @d:Match is an Xpath query on the service response, it should return true or false where true indicates an error.
        @d:httpStatusCode is the error code to return if an response matches the error.
        @d:errorMessage is the user friendly message to return when an error occurs.
        -->
        <d:Condition d:Match="/Result/ErrorMessage[text()='Invalid token']" d:HttpStatusCode="403" d:ErrorMessage="User cannot connect to the service." />
        </d:ErrorHandling>
           </FunctionImport>

            <!-- The EntityContainer defines the output data schema -->
        </EntityContainer>
        <!-- The EntityType @d:Map defines the repeating node (an XPath query) in the response (output data schema). -->
        <!-- If these nodes are outside a namespace, add the prefix in the xpath. -->
        <!--
        @Name - define your user readable name, will become an XML element in the ATOM feed, so comply with the XML element naming restrictions (no spaces or other illegal characters).
        @Type is the EDM.SimpleType of the parameter, see http://msdn.microsoft.com/library/bb399548(v=VS.100).aspx.
        @d:Map uses an Xpath query to point at the location to extract the content from your services response.
        The "." is relative to the repeating node in the EntityType @d:Map Xpath expression.
        -->
            <EntityType Name="MyEntityType" d:Map="/MyResponse/MyEntities">
        <Property Name="ID" d:IsPrimaryKey="True" Type="Int32"  Nullable="false" d:Map="./Remaining[@Amount]"/>
        <Property Name="Amount" Type="Double"   Nullable="false" d:Map="./Remaining[@Amount]"/>
        <Property Name="City"   Type="String"   Nullable="false" d:Map="./City"/>
        <Property Name="State"  Type="String"   Nullable="false" d:Map="./State"/>
        <Property Name="Zip"    Type="Int32"    Nullable="false" d:Map="./Zip"/>
        <Property Name="Updated"    Type="DateTime" Nullable="false" d:Map="./Updated"/>
        <Property Name="AdditionalInfo" Type="String" Nullable="true"
        d:Map="./Info/More[1]"/>
            </EntityType>
        </Schema>

> [AZURE.TIP] Zobacz więcej przykładów CSDL usługi sieci Web w artykule [Przykłady mapowanie istniejącej usługi sieci web do OData za pośrednictwem CSDLs](marketplace-publishing-data-service-creation-odata-mapping-examples.md)

###<a name="dataservice-csdl-example"></a>Przykład CSDL usługi danych

Nawiązuje połączenie z usługą Uwidacznianie tabela bazy danych lub widoku jako punkt końcowy w poniższym przykładzie zawiera dwa interfejsy API dla bazy danych w zależności CSDL interfejsu API (można użyć widoków zamiast tabel).

        <?xml version="1.0"?>
        <!-- The namespace attribute below is used by our system to generate C#. You can change “MyCompany.MyOffer” to something that makes sense for you, but change “MyOffer” consistently throughout the document. -->
        <Schema Namespace="MyCompany.MyDataOffer" Alias="MyOffer" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ado/2009/08/edm">
        <!-- EntityContainer groups all the data service calls together into a single offering. Every web service call has a FunctionImport definition. -->
        <EntityContainer Name="MyOfferContainer">
        <!-- EntitySet is defined for CSDL compatibility reasons, not required for ReturnType=”Raw”
            Think of the EntitySet as a Service
        @Name is used in the customer facing UI as name of the Service.
        @EntityType is used to point at the type definition (returned set of table columns). -->
        <EntitySet Name="CompanyInfoEntitySet" EntityType="MyOffer.CompanyInfo" />
        <EntitySet Name="ProductInfoEntitySet" EntityType="MyOffer.ProductInfo" />
        </EntityContainer>
        <!-- EntityType defines result (output); the table (or view) and columns to be returned by the data service.)
            Map is the schema.tabel or schema.view
            dals.TableName is the table Name
            Name is the name identifier for the EntityType and the Name of the service exposed to the client via the UI.
            dals:IsExposed determines if the table schema is exposed (generally true).
            dals:IsView (optional) true if this is based on a view rather than a table
            dals:TableSchema is the schema name of the table/view
        -->
        <EntityType
        Map="[dbo].[CompanyInfo]"
        dals:TableName="CompanyInfo"
        Name="CompanyInfo"
        dals:IsExposed="true"
        dals:IsView="false"
        dals:TableSchema="dbo"
        xmlns:dals="http://schemas.microsoft.com/dallas/2010/04">
        <!-- Property defines the column properties and the output of the service.
            dals:ColumnName is the name of the column in the table /view.
            Type is the emd.SimpleType
            Nullable determines if NULL is a valid output value
            dals.CharMaxLenght is the maximum length of the output value
            Name is the name of the Property and is exposed to the client facing UI
            dals:IsReturned is the Boolean that determines if the Service exposes this value to the client.
            IsQueryable is the Boolean that determines if the column can be used in a database query
            (For data Services: To improve Performance make sure that columns marked ISQueryable=”true” are in an index.)
            dals:OrdinalPosition is the numerical position x in the table or the View, where x is from 1 to the number of columns in the table.
            dals:DatabaseDataType is the data type of the column in the database, i.e. SQL data type dals:IsPrimaryKey indicates if the column is the Primary key in the table/view.  (The columns marked ISPrimaryKey are used in the Order by clause when returning data.)
        -->
        <Property dals:ColumnName="data" Type="String" Nullable="true" dals:CharMaxLength="-1" Name="data" dals:IsReturned="true" dals:IsQueryable="false" dals:IsPrimaryKey="false" dals:OrdinalPosition="3" dals:DatabaseDataType="nvarchar" />
        <Property dals:ColumnName="id" Type="Int32" Nullable="false" Name="id" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="true" dals:OrdinalPosition="1" dals:NumericPrecision="10" dals:DatabaseDataType="int" />
        <Property dals:ColumnName="ticker" Type="String" Nullable="true" dals:CharMaxLength="10" Name="ticker" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="false" dals:OrdinalPosition="2" dals:DatabaseDataType="nvarchar" />
        </EntityType>
        <EntityType Map="[dbo].[ProductInfo]" dals:TableName="ProductInfo" Name="ProductInfo" dals:IsExposed="true" dals:IsView="false" dals:TableSchema="dbo" xmlns:dals="http://schemas.microsoft.com/dallas/2010/04">
        <Property dals:ColumnName="companyid" Type="Int32" Nullable="true" Name="companyid" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="false" dals:OrdinalPosition="2" dals:NumericPrecision="10" dals:DatabaseDataType="int" />
        <Property dals:ColumnName="id" Type="Int32" Nullable="false" Name="id" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="true" dals:OrdinalPosition="1" dals:NumericPrecision="10" dals:DatabaseDataType="int" />
        <Property dals:ColumnName="product" Type="String" Nullable="true" dals:CharMaxLength="50" Name="product" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="false" dals:OrdinalPosition="3" dals:DatabaseDataType="nvarchar" />
        </EntityType>
        </Schema>

## <a name="see-also"></a>Zobacz też
- Jeśli interesuje Cię nauki i opis określonych węzłów i ich parametrów w tym artykule [Węzłów mapowania danych usługi OData](marketplace-publishing-data-service-creation-odata-mapping-nodes.md) definicje i objaśnienia, przykłady i używać kontekstu wielkość liter.
- Jeśli jesteś zrecenzować przykładów, przeczytaj ten artykuł [Przykłady mapowania danych usługi OData](marketplace-publishing-data-service-creation-odata-mapping-examples.md) , aby wyświetlić przykładowy kod i opis Składnia kodu i kontekst.
- Aby powrócić do określonej ścieżki do publikowania danych usługi Azure Marketplace, w tym artykule [Podręcznik publikowania usług danych](marketplace-publishing-data-service-creation.md).
