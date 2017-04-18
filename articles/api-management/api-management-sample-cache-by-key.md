<properties
    pageTitle="Niestandardowe pamięci podręcznej w zarządzaniu interfejsu API platformy Azure"
    description="Dowiedz się, jak pamięci podręcznej elementów przez klucz w zarządzaniu interfejsu API platformy Azure"
    services="api-management"
    documentationCenter=""
    authors="darrelmiller"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/25/2016"
    ms.author="darrmi"/>

# <a name="custom-caching-in-azure-api-management"></a>Niestandardowe pamięci podręcznej w zarządzaniu interfejsu API platformy Azure
Azure usługi zarządzania interfejsu API obsługuje [buforowanie odpowiedzi HTTP](api-management-howto-cache.md) , używając adresu URL zasobu jako klucz. Klucz można modyfikować za pomocą nagłówków żądania `vary-by` właściwości. Jest to przydatne w pamięci podręcznej całego odpowiedzi HTTP (czyli odwzorowań), ale czasami warto pamięci podręcznej tylko część postać. Nowe zasady [buforowania wyszukiwania](https://msdn.microsoft.com/library/azure/dn894086.aspx#GetFromCacheByKey) i [buforowania sklepu](https://msdn.microsoft.com/library/azure/dn894086.aspx#StoreToCacheByKey) umożliwiają przechowywanie i pobieranie dowolnego części danych z poziomu definicje zasad. Taką możliwość również dodaje wartość do zasad wcześniej wprowadzonych na [Żądanie wysłania](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) ponieważ teraz pamięci podręcznej odpowiedzi od usług zewnętrznych.

## <a name="architecture"></a>Architektura  
Usługa zarządzania interfejsu API używa pamięci podręcznej danych dzierżawy udostępnione, tak, aby podczas skalowania do wiele jednostek będzie nadal uzyskać dostęp do tego samego dane w pamięci podręcznej. Jednak podczas pracy z wdrożeniu w przypadku istnieje niezależnych pamięci podręcznej w każdym z regionów. Ze względu na to należy traktuje pamięci podręcznej jako magazynu danych, tam, gdzie jest źródle tylko niektóre informacją. Jeśli zostało, a później przez danego korzystać w przypadku wdrożenia, klientom użytkowników, których podróży mogą utracić dostęp do tych danych w pamięci podręcznej.

## <a name="fragment-caching"></a>Fragment pamięci podręcznej
Istnieje niektórych przypadkach, gdy odpowiedzi zwracanych zawiera część danych drogich ustalenie i jeszcze pozostaje świeży rozsądne ilość czasu. Na przykład należy rozważyć, czy usługi utworzoną przez lotnicza znajdują się informacje dotyczące Rezerwacje lotów, stan lotu itp. Jeśli użytkownik jest członkiem programu punktów linii lotniczych, również zostałyby informacji dotyczących ich bieżący stan i przebiegu skumulowaną. Te informacje dotyczące użytkowników mogą być przechowywane w układzie różnych, ale może być pożądane dołączyć go do odpowiedzi dotyczące stanu lotów i rezerwacji zwrócone. Można to zrobić przy użyciu procesu o nazwie fragment pamięci podręcznej. Podstawowy reprezentacja może zostać zwrócony z serwera pochodzenia za pomocą niektórych rodzajów token, aby wskazać miejsce, w którym ma można wstawiać informacje dotyczące użytkowników. 

Należy rozważyć następujące odpowiedź JSON interfejsu API wewnętrznej bazie danych.


    {
      "airline" : "Air Canada",
      "flightno" : "871",
      "status" : "ontime",
      "gate" : "B40",
      "terminal" : "2A",
      "userprofile" : "$userprofile$"
    }  

I pomocniczą zasobów w `/userprofile/{userid}` że wygląda,

     { "username" : "Bob Smith", "Status" : "Gold" }

W celu ustalenia odpowiednie informacje użytkownika do uwzględnienia, trzeba określić, kto jest użytkownika końcowego. Ten mechanizm jest implementacją zależne. Na przykład korzystam z `Subject` rościć sobie z `JWT` token. 

    <set-variable
      name="enduserid"
      value="@(context.Request.Headers.GetValueOrDefault("Authorization","").Split(' ')[1].AsJwt()?.Subject)" />

To przechowywane `enduserid` wartość w zmiennej kontekstu do późniejszego użycia. Następnym krokiem jest ustalenie, jeśli poprzednie żądanie już pobrane informacje o użytkowniku i on zapisany w pamięci podręcznej. W tym firma Microsoft korzysta z `cache-lookup-value` zasad.

      <cache-lookup-value
      key="@("userprofile-" + context.Variables["enduserid"])"
      variable-name="userprofile" />

Jeśli w pamięci podręcznej, odpowiadający wartości klucza, a następnie nie jest puste `userprofile` zmiennej kontekst zostaną utworzone. Trwa sprawdzanie powodzenia za pomocą wyszukiwania `choose` zasad przepływu sterowania.

    <choose>
        <when condition="@(!context.Variables.ContainsKey("userprofile"))">
            <!— If the userprofile context variable doesn’t exist, make an HTTP request to retrieve it.  -->
        </when>
    </choose>


Jeśli `userprofile` zmiennej kontekstu nie istnieje, a następnie możemy mają należy żądanie HTTP, aby go pobrać.

    <send-request
      mode="new"
      response-variable-name="userprofileresponse"
      timeout="10"
      ignore-error="true">

      <!-- Build a URL that points to the profile for the current end-user -->
      <set-url>@(new Uri(new Uri("https://apimairlineapi.azurewebsites.net/UserProfile/"),
          (string)context.Variables["enduserid"]).AbsoluteUri)
      </set-url>
      <set-method>GET</set-method>
    </send-request>

Firma Microsoft korzysta z `enduserid` do utworzenia adresu URL do zasobu profilu użytkownika. Gdy mamy już odpowiedzi, udostępniamy pobierają tekstu podstawowego poza odpowiedź i zapisać go ponownie do zmiennej kontekstu.

    <set-variable
        name="userprofile"
        value="@(((IResponse)context.Variables["userprofileresponse"]).Body.As<string>())" />

Aby uniknąć nam konieczności to żądanie HTTP ponownie, w przypadku tego samego użytkownika kolejnego żądania, możemy zawierają profilu użytkownika w pamięci podręcznej.

    <cache-store-value
        key="@("userprofile-" + context.Variables["enduserid"])"
        value="@((string)context.Variables["userprofile"])" duration="100000" />

Wartości przechowywane w pamięci podręcznej, za pomocą klawisza sam dokładnie możemy pierwotnie próba na pobranie ich z. Czas trwania wybranego do przechowywania wartości powinny być oparte na temat często zmiany informacji i jak na uszkodzenia użytkowników są do nieaktualne informacje. 

Należy uznasz, że pobieranie z pamięci podręcznej nadal jest out of process, żądanie sieci i potencjalnie może nadal Dodawanie dziesiątki milisekund do wezwania na. Korzyści pochodzą podczas określania informacji dotyczących profilów użytkowników trwa znacznie dłużej niż z powodu konieczności bazy danych kwerendy lub zagregowanych informacji z wielu końce Wstecz.

Ostatnim krokiem procesu jest aktualizacja zwracane odpowiedzi z naszych informacji w profilu użytkownika.

    <!—Update response body with user profile-->
    <find-and-replace
        from='"$userprofile$"'
        to="@((string)context.Variables["userprofile"])" />

Wybrano być tokenu uwzględniane cudzysłowów, tak aby nawet wtedy, gdy nie zachodzi Zamień, odpowiedź była nadal ważna JSON. To jest przede wszystkim ułatwia debugowanie.

Poniższe czynności są ze sobą połączyć, w wyniku po zasady, który jest podobny do następującego.

     <policies>
        <inbound>
            <!-- How you determine user identity is application dependent -->
            <set-variable
              name="enduserid"
              value="@(context.Request.Headers.GetValueOrDefault("Authorization","").Split(' ')[1].AsJwt()?.Subject)" />

            <!--Look for userprofile for this user in the cache -->
            <cache-lookup-value
              key="@("userprofile-" + context.Variables["enduserid"])"
              variable-name="userprofile" />

            <!-- If we don’t find it in the cache, make a request for it and store it -->
            <choose>
                <when condition="@(!context.Variables.ContainsKey("userprofile"))">
                    <!—Make HTTP request to get user profile -->
                    <send-request
                      mode="new"
                      response-variable-name="userprofileresponse"
                      timeout="10"
                      ignore-error="true">

                       <!-- Build a URL that points to the profile for the current end-user -->
                        <set-url>@(new Uri(new Uri("https://apimairlineapi.azurewebsites.net/UserProfile/"),(string)context.Variables["enduserid"]).AbsoluteUri)</set-url>
                        <set-method>GET</set-method>
                    </send-request>

                    <!—Store response body in context variable -->
                    <set-variable
                      name="userprofile"
                      value="@(((IResponse)context.Variables["userprofileresponse"]).Body.As<string>())" />

                    <!—Store result in cache -->
                    <cache-store-value
                      key="@("userprofile-" + context.Variables["enduserid"])"
                      value="@((string)context.Variables["userprofile"])"
                      duration="100000" />
                </when>
            </choose>
            <base />
        </inbound>
        <outbound>
            <!—Update response body with user profile-->
            <find-and-replace
                  from='"$userprofile$"'
                  to="@((string)context.Variables["userprofile"])" />
            <base />
        </outbound>
     </policies>

Tej metody buforowania jest używana przede wszystkim w witrynach sieci web miejsce, w którym HTML składa się po stronie serwera, aby może być renderowana jako pojedynczej strony. Jednak może również być przydatne w interfejsy API miejsce, w którym klienci nie klienta strona buforowanie HTTP lub jest nie chcesz, aby umieścić że odpowiedzialność na komputerze klienckim.

Można także przeprowadzić tego samego rodzaju fragment pamięci podręcznej na serwerach web wewnętrznej bazy danych za pomocą Redis pamięci podręcznej serwera, jednak wykonać tę pracę za pomocą usługi zarządzania interfejsu API jest przydatny, gdy fragmenty pamięci podręcznej są pochodzące z różnych końców wstecz niż podstawowy odpowiedzi.

## <a name="transparent-versioning"></a>Przezroczysty przechowywania wersji
Jest wspólnych ćwiczenia dla różnych wersji różnych implementacji interfejsu API obsługi w dowolnym momencie. Jest to prawdopodobnie do obsługi różnych środowiskach, takich jak deweloperów, testowanie, produkcję itd., lub może być do obsługi starszych wersji interfejsu API dają dla klientów indywidualnych interfejsu API przeprowadzić migrację do nowszej wersji. 

Jeden z nich do obsługi to zamiast deweloperów klienta zmienić adresy URL z `/v1/customers` do `/v2/customers` mają być przechowywane w danych profilu dla klientów indywidualnych wersji interfejsu API obecnie chcą za pomocą i połączeń adres URL odpowiedniej wewnętrznej bazy danych. W celu ustalenia adresu URL poprawne wewnętrznej bazy danych do połączeń dla określonego klienta, należy kwerendy danych konfiguracji. Buforowanie danych konfiguracji, możemy zminimalizować ograniczeń wydajności wykonanie tego odnośnika.

Pierwszym krokiem jest identyfikator używany do konfigurowania odpowiedniej wersji. W tym przykładzie wybrano skojarzyć wersji do subskrypcji klucza produktu. 

        <set-variable name="clientid" value="@(context.Subscription.Key)" />

Firma Microsoft wykonaj wyszukiwania pamięci podręcznej, aby zobaczyć, jeśli mamy już pobrane wersja klienta odpowiednie.

        <cache-lookup-value
        key="@("clientversion-" + context.Variables["clientid"])"
        variable-name="clientversion" />

Następnie możemy Sprawdź, jeśli nie znaleziono go w pamięci podręcznej.

    <choose>
        <when condition="@(!context.Variables.ContainsKey("clientversion"))">

Jeśli firma Microsoft nie możemy przejść i go pobrać.

    <send-request
        mode="new"
        response-variable-name="clientconfiguresponse"
        timeout="10"
        ignore-error="true">
                <set-url>@(new Uri(new Uri(context.Api.ServiceUrl.ToString() + "api/ClientConfig/"),(string)context.Variables["clientid"]).AbsoluteUri)</set-url>
                <set-method>GET</set-method>
    </send-request>

Wyodrębnianie tekstu podstawowego odpowiedzi z odpowiedzi.

    <set-variable
          name="clientversion"
          value="@(((IResponse)context.Variables["clientconfiguresponse"]).Body.As<string>())" />

Zapisz go ponownie w pamięci podręcznej do użycia w przyszłości.

    <cache-store-value
          key="@("clientversion-" + context.Variables["clientid"])"
          value="@((string)context.Variables["clientversion"])"
          duration="100000" />

I na końcu zaktualizuj adres URL wewnętrznej, aby wybrać wersję żądanej przez klienta usługi.

    <set-backend-service
          base-url="@(context.Api.ServiceUrl.ToString() + "api/" + (string)context.Variables["clientversion"] + "/")" />

Zasady całkowicie wygląda następująco:

     <inbound>
        <base />
        <set-variable name="clientid" value="@(context.Subscription.Key)" />
        <cache-lookup-value key="@("clientversion-" + context.Variables["clientid"])" variable-name="clientversion" />

        <!-- If we don’t find it in the cache, make a request for it and store it -->
        <choose>
            <when condition="@(!context.Variables.ContainsKey("clientversion"))">
                <send-request mode="new" response-variable-name="clientconfiguresponse" timeout="10" ignore-error="true">
                    <set-url>@(new Uri(new Uri(context.Api.ServiceUrl.ToString() + "api/ClientConfig/"),(string)context.Variables["clientid"]).AbsoluteUri)</set-url>
                    <set-method>GET</set-method>
                </send-request>
                <!-- Store response body in context variable -->
                <set-variable name="clientversion" value="@(((IResponse)context.Variables["clientconfiguresponse"]).Body.As<string>())" />
                <!-- Store result in cache -->
                <cache-store-value key="@("clientversion-" + context.Variables["clientid"])" value="@((string)context.Variables["clientversion"])" duration="100000" />
            </when>
        </choose>
        <set-backend-service base-url="@(context.Api.ServiceUrl.ToString() + "api/" + (string)context.Variables["clientversion"] + "/")" />
    </inbound>


Umożliwiając użytkownikom interfejsu API przezroczysty kontrolować, która wersja wewnętrznej bazy danych jest dostępny przez klientów bez konieczności zaktualizuj i ponownie wdróż klientów jest rozwiązaniem eleganckie, która rozwiązuje wiele problemów przechowywania wersji interfejsu API.

## <a name="tenant-isolation"></a>Izolacji dzierżawy

We wdrożeniach większej, wielu dzierżawy w niektórych firmach usunięcie utworzenie oddzielnych grup dzierżaw na odrębnych wdrożeń sprzętu wewnętrznej bazy danych. To minimalizuje liczbę klientów, którzy ma wpływ problem ze sprzętem do wewnętrznej bazy danych. Umożliwia także nowe wersje oprogramowania do wdrażania etapami. Najlepiej Ta architektura wewnętrznej bazy danych powinien być niewidoczne dla klientów indywidualnych interfejsu API. Można to osiągnąć w podobny sposób jak przechowywanie wersji przezroczysty ponieważ jest oparty na tym samym techniki manipulowania adres URL wewnętrznej bazy danych przy użyciu stanu konfiguracji na klucz interfejsu API.  

Zamiast zwracać preferowaną wersję interfejsu API dla każdego klucza subskrypcji, zwróci identyfikator odpowiadającą dzierżawy grupa przydzielonych sprzętu. Ten identyfikator może służyć do utworzenia adresu URL odpowiedniej wewnętrznej bazy danych.

## <a name="summary"></a>Podsumowanie
Swobody do przechowywania każdego typu danych za pomocą interfejsu API Azure pamięci podręcznej zarządzania umożliwia zapewnienia wydajnego dostępu do danych konfiguracji, które mogą mieć wpływ na sposób przetwarzania przychodzącego żądania. Również można go do przechowywania fragmenty danych, które można rozszerzyć odpowiedzi, zwrócone przez interfejs API wewnętrznej bazie danych.

## <a name="next-steps"></a>Następne kroki
Podaj swoją opinię w wątku Disqus dla tego tematu istnieją inne scenariusze, w których te zasady jest włączona dla Ciebie, czy w przypadku scenariuszy chcesz nadać ale nie mieć wrażenia, są obecnie dostępne.
