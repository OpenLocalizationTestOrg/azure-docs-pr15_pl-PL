<properties
    pageTitle="Usługa kapitału uwierzytelniania dla aplikacji interfejsu API usługi aplikacji Azure | Microsoft Azure"
    description="Dowiedz się, jak chronić aplikacji interfejsu API usługi aplikacji Azure scenariuszach do usługi."
    services="app-service\api"
    documentationCenter=".net"
    authors="tdykstra"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-api"
    ms.workload="na"
    ms.tgt_pltfrm="dotnet"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/30/2016" 
    ms.author="rachelap"/>

# <a name="service-principal-authentication-for-api-apps-in-azure-app-service"></a>Usługa kapitału uwierzytelniania dla aplikacji interfejsu API usługi aplikacji Azure

## <a name="overview"></a>Omówienie

W tym artykule wyjaśniono, jak uwierzytelniania aplikacji usługi dla *wewnętrznych* dostęp do aplikacji interfejsu API. Scenariusz wewnętrzny jest miejsce, w którym masz aplikację interfejs API, który ma być eksploatacyjnych tylko w kodzie aplikacji. Zalecany sposób wdrożenia tego scenariusza w aplikacji usługi jest użycie Azure AD do ochrony nazywane aplikacji interfejsu API. Zadzwoń chronionego aplikacji interfejsu API token okaziciela pobranego z Azure AD, dostarczając tożsamości aplikacji poświadczeń (usługa kapitału). Dla rozwiązania alternatywne wobec przy użyciu Azure AD zobacz sekcję **do usługi uwierzytelniania** [Omówienie uwierzytelniania Azure aplikacji usługi](../app-service/app-service-authentication-overview.md#service-to-service-authentication).

W tym artykule opisano następujące zagadnienia:

* Jak używać usługi Azure Active Directory (Azure AD) ochrony aplikacji interfejsu API z nieuwierzytelnionych programu access.
* Sposób wykorzystywania chronionych aplikacji interfejsu API z interfejsu API aplikacji, w przeglądarce lub aplikacji dla urządzeń przenośnych za pomocą Azure AD poświadczenia kapitału (tożsamość aplikacji). Aby uzyskać informacje dotyczące korzystania z aplikacji logiczny zobacz [Korzystanie z niestandardowego interfejsu API hostowana w aplikacji usługi z aplikacjami logiczny](../app-service-logic/app-service-logic-custom-hosted-api.md).
* Jak zapewnić, że chronionej aplikacji interfejsu API nie można wywołać przy użyciu przeglądarki przez zalogowanych użytkowników.
* W jaki sposób aby upewnić się, że chronionej aplikacji interfejsu API może być wywoływana tylko według określonego Azure AD usługi kapitału.

Ten artykuł zawiera dwie sekcje:

* Sekcji [sposobu konfigurowania uwierzytelniania głównej usługi Azure aplikacji usługi](#authconfig) ogólnie opisano konfigurowanie uwierzytelniania dla dowolnej aplikacji interfejsu API oraz korzystać z chronionego aplikacji interfejsu API. W tej sekcji dotyczą jednakowo RAM wszystkie obsługiwane przez usługi aplikacji, w tym .NET, Node.js i Java.

* Począwszy od sekcji [kontynuowaniem samouczki .NET wprowadzenie do](#tutorialstart) samouczka przeprowadzi Cię przez skonfigurowanie scenariusz "wewnętrznych w programie access" przykładową aplikację .NET działa w aplikacji usługi. 

## <a id="authconfig"></a>Jak skonfigurować uwierzytelnianie głównej usługi Azure aplikacji usługi

Ta sekcja zawiera ogólne instrukcje, które dotyczą dowolnej aplikacji interfejsu API. Aby uzyskać procedury określonej aplikacji przykładowej do .NET listy wykonaj przejdź do [kontynuowaniem samouczka aplikacje interfejsu API programu .NET](#tutorialstart).

1. W [portalu Azure](https://portal.azure.com/), przejdź do karta **Ustawienia** aplikacji interfejsu API, którą chcesz chronić, a następnie znajdź sekcję **Funkcje** i kliknij pozycję **uwierzytelniania i autoryzacji**.

    ![Uwierzytelnianie i autoryzacja w Azure portal](./media/app-service-api-dotnet-user-principal-auth/features.png)

3. W **uwierzytelniania i autoryzacji** karta, **Wybierz polecenie**.

4. Na liście rozwijanej **akcję do wykonania po żądanie nie jest uwierzytelniony** wybierz pozycję **Zaloguj się przy użyciu usługi Azure Active Directory** .

5. W obszarze **Dostawców uwierzytelniania**wybierz **Usługi Azure Active Directory**.

    ![Karta uwierzytelniania i autoryzacji w Azure portal](./media/app-service-api-dotnet-user-principal-auth/authblade.png)

6. Konfigurowanie karta **Ustawienia usługi Azure Active Directory** , aby utworzyć nową aplikację Azure AD lub użyj istniejącej aplikacji Azure AD Jeśli masz już jeden, którego chcesz użyć.

    Scenariusze wewnętrznych obejmują zwykle aplikacji interfejsu API wywoływanie aplikacji interfejsu API. Korzystając z osobnych Azure AD aplikacje dla każdej aplikacji interfejsu API lub tylko jedną aplikację Azure AD.

    Aby uzyskać szczegółowe instrukcje na to karta zobacz [jak skonfigurować aplikację usługi aplikacji do logowania usługi Azure Active Directory za pomocą](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md).

7. Po wykonaniu tych Karta Konfiguracja dostawcy uwierzytelniania, kliknij **przycisk OK**.

7. W **uwierzytelniania i autoryzacji** karta, kliknij przycisk **Zapisz**.

    ![Kliknij przycisk Zapisz](./media/app-service-api-dotnet-service-principal-auth/authsave.png)

Gdy jest to możliwe, aplikacji usługi tylko zezwala na żądania od osób dzwoniących w skonfigurowanych Azure AD dzierżawy. Brak kodu uwierzytelniania i autoryzacji jest wymagane w chronionym aplikacji interfejsu API. Token okaziciela przekazywana do funkcji aplikacji interfejsu API wraz z najczęściej używanych roszczeń w nagłówkach HTTP, a można znaleźć te informacje w kodzie, aby sprawdzić, czy żądania pochodzą z określonego rozmówcy, takich jak wystawcy usługi.

Ta funkcja uwierzytelniania działa tak samo dla wszystkich języków obsługiwanych przez usługi aplikacji, takich jak .NET, Node.js i Java. 

#### <a name="how-to-consume-the-protected-api-app"></a>Jak korzystać z chronionego aplikacji interfejsu API

Wywołujący musi obsługiwać token okaziciela Azure AD przy użyciu interfejsu API. Aby uzyskać token okaziciela przy użyciu poświadczeń głównej usługi, wywołujący używa Active Directory Authentication Library (ADAL dla [.NET](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory), [Node.js](https://github.com/AzureAD/azure-activedirectory-library-for-nodejs)lub [Java](https://github.com/AzureAD/azure-activedirectory-library-for-java)). Aby uzyskać token, kod, który wywołuje ADAL przekazuje ADAL następujące informacje:

* Nazwa Twojej dzierżawy Azure AD.
* Identyfikator klienta i tajny klienta (klawisz aplikacji) aplikacji Azure AD skojarzone z do osoby wywołującej.
* Identyfikator klienta aplikacji Azure AD skojarzone z chronionego aplikacji interfejsu API. (Jeśli jest używana tylko jedną aplikację Azure AD, jest to samej identyfikator klienta, co dla wywołujący.)

Wartości te są dostępne na stronach Azure AD [portal Azure klasyczny](https://manage.windowsazure.com/).

Gdy uzyskano tokenu, wywołujący zawiera z żądania HTTP w nagłówku autoryzacji.  Aplikacja usługi sprawdza tokenu i umożliwia żądania do osiągnięcia chronionego aplikacji interfejsu API.

#### <a name="how-to-protect-the-api-app-from-access-by-users-in-the-same-tenant"></a>Jak zabezpieczyć aplikację interfejsu API z programu access przez użytkowników w tej samej dzierżawy

Tokeny okaziciela dla użytkowników w tej samej dzierżawy są traktowane jako prawidłowe chronionego aplikacji interfejsu API.  Jeśli chcesz upewnić się, że wystawcy usługi można zadzwonić chronionego aplikacji interfejsu API, Dodaj kod w chronionym aplikacji interfejsu API, aby sprawdzić poprawność następujące roszczeń związanych z tokenu:

* `appid`powinny być identyfikator klienta aplikacji Azure AD, która jest skojarzony z rozmówcy. 
* `oid`(`objectidentifier`) powinny być usługi kapitału identyfikator rozmówcy. 

Usługa aplikacji udostępnia również `objectidentifier` rościć sobie w nagłówku X-MS-klient-kapitału-ID.

### <a name="how-to-protect-the-api-app-from-browser-access"></a>Jak chronić aplikacji interfejsu API z programu access w przeglądarce

Jeśli nie Sprawdź poprawność oświadczeń w kodzie w chronionym aplikacji interfejsu API i użycie osobny aplikacji Azure AD chronionego aplikacji interfejsu API, upewnij się, że aplikacja Azure AD odpowiedź adres URL jest taki sam, jak podstawowy adres URL aplikacji interfejsu API. Jeśli adres URL odpowiedź wskazuje bezpośrednio do chronionej aplikacji interfejsu API, użytkownika w tej samej dzierżawy Azure AD może przejdź do aplikacji interfejsu API, zaloguj się i pomyślne wywołanie API.

## <a id="tutorialstart"></a>Kontynuowanie samouczka aplikacje interfejsu API .NET

Jeśli używasz Node.js lub Java samouczka aplikacje interfejsu API, przejdź do sekcji [Następne kroki](#next-steps) . 

W dalszej części tego artykułu będzie nadal występował samouczka .NET interfejsu API aplikacje i założono, że została ukończona [Samouczek uwierzytelnianie użytkownika](app-service-api-dotnet-user-principal-auth.md) i masz Przykładowa aplikacja działa w Azure z uwierzytelnianie użytkownika jest włączone.

## <a name="set-up-authentication-in-azure"></a>Konfigurowanie uwierzytelniania platformy Azure

W tej sekcji możesz skonfigurować usługę aplikacji tak, aby tylko żądania HTTP umożliwia uzyskanie aplikacji interfejsu API Warstwa danych są te, które mają prawidłowe Azure AD okaziciela tokeny. 

W poniższej sekcji można skonfigurować aplikację interfejsu API warstwy środkowej, aby wysyłanie poświadczeń aplikacji do Azure AD, wrócić token okaziciela i wysyłanie token okaziciela do aplikacji interfejsu API Warstwa danych. Ten proces przedstawiono na diagramie.

![Diagram uwierzytelniania usługi](./media/app-service-api-dotnet-service-principal-auth/appdiagram.png)

Jeśli napotkasz problemy podczas zgodnie z instrukcjami samouczka, zobacz sekcję [Rozwiązywanie problemów](#troubleshooting) na koniec samouczka. 

1. W [portalu Azure](https://portal.azure.com/)przejdź do karta **Ustawienia** aplikacji interfejsu API utworzonego dla aplikacji interfejsu API ToDoListDataAPI (Warstwa danych), a następnie kliknij **Ustawienia**.

2. W karta **Ustawienia** Znajdź sekcję **Funkcje** , a następnie kliknij pozycję **uwierzytelniania i autoryzacji**.

    ![Uwierzytelnianie i autoryzacja w Azure portal](./media/app-service-api-dotnet-user-principal-auth/features.png)

3. W **uwierzytelniania i autoryzacji** karta, **Wybierz polecenie**.

4. Na liście rozwijanej **akcję do wykonania po żądanie nie jest uwierzytelniony** wybierz pozycję **Zaloguj się przy użyciu usługi Azure Active Directory**.

    Jest to ustawienie, które powoduje wyświetlanie aplikacji usługi zapewniające tylko uwierzytelnieni reach żądania aplikacji interfejsu API. Aby uzyskać żądania, które mają prawidłowe okaziciela tokenów aplikacji usługi przekazuje tokeny wzdłuż do aplikacji interfejsu API i wypełnia nagłówki HTTP często używane roszczeń, aby łatwiej udostępnić te informacje w kodzie.

5. W obszarze **Dostawców uwierzytelniania**kliknij **Usługi Azure Active Directory**.

    ![Karta uwierzytelniania i autoryzacji w Azure portal](./media/app-service-api-dotnet-user-principal-auth/authblade.png)

6. Karta **Ustawienia usługi Azure Active Directory** kliknij **Express**.

    Z **Express** opcja Azure można automatycznie utworzyć aplikację AAD w Azure AD [dzierżawy](https://msdn.microsoft.com/en-us/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant). 

    Nie musisz utworzyć dzierżawy, ponieważ każdy konto Azure istnieje automatycznie.

7. W **trybie zarządzania**kliknij pozycję **Utwórz nową aplikację AD** , jeśli nie została jeszcze wybrana.

    Portalu podłączany wprowadzania okno **Tworzenie aplikacji** z wartością domyślną. Domyślnie aplikacja Azure AD nosi nazwę taka sama, jak aplikacja interfejsu API. Jeśli wolisz, należy wprowadzić pod inną nazwą.
    
    ![Ustawienia Azure AD](./media/app-service-api-dotnet-service-principal-auth/aadsettings.png)

    **Uwaga**: Alternatywnie, można użyć jednej Azure AD aplikacji dla połączeń aplikacji interfejsu API i chronionych aplikacji interfejsu API. Jeśli wybierzesz to alternatywne rozwiązanie nie jest wymagane odpowiednią opcję **Utworzenia nowej aplikacji AD** ponieważ aplikacji Azure AD już utworzone wcześniej podczas przerabiania samouczka uwierzytelnianie użytkownika. Ten samouczek, które będą używane oddzielić aplikacje Azure AD dla połączeń interfejsu API aplikacji i do chronionego interfejsu API.

8. Zanotuj wartość, która znajduje się w polu wprowadzania **Tworzenie aplikacji** ; wyszukiwanie będzie tej AAD aplikacji w portalu klasyczny Azure później.

7. Kliknij **przycisk OK**.

10. W **uwierzytelniania i autoryzacji** karta, kliknij przycisk **Zapisz**.

    ![Kliknij przycisk Zapisz](./media/app-service-api-dotnet-service-principal-auth/saveauth.png)

    Aplikacji usługi tworzy aplikację usługi Azure Active Directory z **adresem URL logowania jednokrotnego** i **Adres URL odpowiedzi** automatyczne ustawić adres URL aplikacji interfejsu API. Ostatnią wartością umożliwia użytkowników w dzierżawie AAD logowanie się i uzyskać dostęp do aplikacji interfejsu API.

### <a name="verify-that-the-api-app-is-protected"></a>Sprawdź, czy aplikacji interfejsu API jest chroniony

1. W przeglądarce przejdź do adresu URL aplikacji interfejsu API: karta **aplikacji interfejsu API** w portalu Azure, kliknij łącze pod **adresem URL**. 

    Nastąpi przekierowanie do ekranu logowania, ponieważ nieuwierzytelnionych żądania nie są dozwolone do osiągnięcia aplikacji interfejsu API. 

    Jeśli w przeglądarce przejdź do interfejsu użytkownika Swagger, przeglądarki już może zostać zarejestrowane--w takim przypadku Otwórz okno InPrivate lub Incognito i przejdź do adresu URL Swagger interfejsu użytkownika.

18. Zaloguj się przy użyciu poświadczeń użytkowników w dzierżawie AAD.

    Gdy użytkownik jest zalogowany, "utworzono" strona jest wyświetlana w przeglądarce.

## <a name="configure-the-todolistapi-project-to-acquire-and-send-the-azure-ad-token"></a>Konfigurowanie projektu ToDoListAPI, aby uzyskać i wysyłanie token Azure AD

W tej sekcji, możesz wykonać następujące zadania:

* Dodawanie kodu w aplikację interfejsu API warstwy środkowej, która używa poświadczeń aplikacji Azure AD uzyskać tokenu i wyślij go przy użyciu żądania HTTP do aplikacji interfejsu API Warstwa danych.
* Pobieranie poświadczeń, które są potrzebne z Azure AD.
* Wprowadź poświadczenia do ustawień środowiska środowisko uruchomieniowe Azure aplikacji usługi w warstwie środkowej interfejsu API aplikacji. 

### <a name="configure-the-todolistapi-project-to-acquire-and-send-the-azure-ad-token"></a>Konfigurowanie projektu ToDoListAPI, aby uzyskać i wysyłanie token Azure AD

Należy wprowadzić następujące zmiany w projekcie ToDoListAPI w programie Visual Studio.

1. Usuń komentarze cały kod w pliku *ServicePrincipal.cs* .

    Jest to kod, która używa ADAL dla środowiska .NET umożliwia uzyskanie token okaziciela Azure AD.  Używa kilku wartości konfiguracji, które skonfigurujesz w środowisku Azure środowisko uruchomieniowe później. Poniżej przedstawiono kod: 

        public static class ServicePrincipal
        {
            static string authority = ConfigurationManager.AppSettings["ida:Authority"];
            static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];
            static string clientSecret = ConfigurationManager.AppSettings["ida:ClientSecret"];
            static string resource = ConfigurationManager.AppSettings["ida:Resource"];
        
            public static AuthenticationResult GetS2SAccessTokenForProdMSA()
            {
                return GetS2SAccessToken(authority, resource, clientId, clientSecret);
            }
        
            static AuthenticationResult GetS2SAccessToken(string authority, string resource, string clientId, string clientSecret)
            {
                var clientCredential = new ClientCredential(clientId, clientSecret);
                AuthenticationContext context = new AuthenticationContext(authority, false);
                AuthenticationResult authenticationResult = context.AcquireToken(
                    resource,
                    clientCredential);
                return authenticationResult;
            }
        }

    **Uwaga:** Kod wymaga ADAL pakietu .NET NuGet (Microsoft.IdentityModel.Clients.ActiveDirectory), który jest już zainstalowany w projekcie. Jeśli były tworzone tego projektu od podstaw, trzeba zainstalować ten pakiet. Ten pakiet nie jest automatycznie instalowany przez szablon nowego projektu aplikacji interfejsu API.

2. W *Kontrolerów/ToDoListController*, Usuń komentarze kod w `NewDataAPIClient` metody, który zapewnia token HTTP żądania w nagłówku autoryzacji.

        client.HttpClient.DefaultRequestHeaders.Authorization =
            new AuthenticationHeaderValue("Bearer", ServicePrincipal.GetS2SAccessTokenForProdMSA().AccessToken);

3. Wdróż projekt ToDoListAPI. (Kliknij prawym przyciskiem myszy projektu, a następnie kliknij pozycję **Publikuj > Publikuj**.)

    Program Visual Studio wdraża projektu i otwarcie przeglądarki podstawowy adres URL aplikacji sieci web. To zostanie wyświetlona strony błędu 403, która jest normalnym próba przejdź do adresu URL podstawowy interfejs API sieci Web w przeglądarce.

4. Zamknij przeglądarkę.

### <a name="get-azure-ad-configuration-values"></a>Uzyskiwanie wartości konfiguracji Azure AD

11. W [portalu klasyczny Azure](https://manage.windowsazure.com/)przejdź do **Usługi Azure Active Directory**.

12. Na karcie **katalog** kliknij dzierżawy usługi AAD.

14. Kliknij pozycję **Aplikacje > aplikacje właścicielem mojej firmy**, a następnie kliknij znacznik wyboru.

15. Na liście aplikacji kliknij nazwę, w którym Azure tworzony po włączeniu uwierzytelniania dla aplikacji interfejsu API ToDoListDataAPI (Warstwa danych).

16. Kliknij kartę **Konfiguruj** .

5. Skopiuj wartość **Identyfikator klienta** i Zapisz miejsc, który można pobrać ze później. 

8. W portalu klasyczny Azure wróć do listy **aplikacji właścicielem mojej firmy**, a następnie kliknij aplikację AAD, utworzoną dla aplikacji interfejsu API ToDoListAPI warstwy środkowej (ten, który został utworzony w poprzednim samouczku, nie ten, który został utworzony w tym samouczku).

16. Kliknij kartę **Konfiguruj** .

5. Skopiuj wartość **Identyfikator klienta** i Zapisz miejsc, który można pobrać ze później.

6. W obszarze **klawiszy**wybierz **1 rok** z listy rozwijanej **Wybierz czas trwania** .

6. Kliknij przycisk **Zapisz**.

    ![Generowanie klawisz aplikacji](./media/app-service-api-dotnet-service-principal-auth/genkey.png)

7. Skopiuj wartość klucza i Zapisz miejsc, który można pobrać ze później.

    ![Skopiuj nowy klucz aplikacji](./media/app-service-api-dotnet-service-principal-auth/genkeycopy.png)

### <a name="configure-azure-ad-settings-in-the-middle-tier-api-apps-runtime-environment"></a>Konfigurowanie ustawień Azure AD w środowisku obsługi aplikacji warstwa środkowa interfejsu API

1. Przejdź do [portalu Azure](https://portal.azure.com/), a następnie przejdź do karta **Interfejsu API aplikacji** dla aplikacji interfejsu API obsługującego projektu TodoListAPI (inicjał drugiego poziomu).

2. Kliknij pozycję **Ustawienia > Ustawienia aplikacji**.

3. W sekcji **Ustawienia aplikacji** Dodaj następujące klucze i wartości:

  	| **Klawisz** | IDA: urząd |
  	|---|---|
  	| **Wartość** | https://login.microsoftonline.com/ {nazwa Twojej dzierżawy Azure AD} |
  	| **Przykład** | https://login.microsoftonline.com/contoso.onmicrosoft.com |

  	| **Klawisz** | IDA: ClientId |
  	|---|---|
  	| **Wartość** | Identyfikator klienta aplikacji wywołującej (środkowa warstwa - ToDoListAPI) |
  	| **Przykład** | 960adec2-b74a-484a-960adec2-b74a-484a |

  	| **Klawisz** | IDA: ClientSecret |
  	|---|---|
  	| **Wartość** | Klawisz aplikacji aplikacji wywołującej (środkowa warstwa - ToDoListAPI) |
  	| **Przykład** | e65e8fc9-5f6b-48e8-e65e8fc9-5f6b-48e8 |

  	| **Klawisz** | IDA: zasobów |
  	|---|---|
  	| **Wartość** | Identyfikator klienta, nazywane aplikacji (Warstwa danych - ToDoListDataAPI) |
  	| **Przykład** | e65e8fc9-5f6b-48e8-e65e8fc9-5f6b-48e8 |

    **Uwaga**: dla `ida:Resource`, upewnij się, użyj aplikacji o nazwie **identyfikator klienta** , a nie jego **Aplikacji identyfikator URI**.

    `ida:ClientId`i `ida:Resource` różnią się wartości dla tego samouczka, ponieważ korzystasz oddzielić applicaations Azure AD dla warstwa środkowa i warstwa danych. Za pomocą jednej aplikacji Azure AD dla połączeń interfejsu API aplikacji i do chronionych interfejsu API, czy używać tej samej wartości w obu `ida:ClientId` i `ida:Resource`.

    Kod użyto ConfigurationManager, aby uzyskać te wartości, aby może być przechowywany w pliku Web.config projektu lub w środowisku Azure runtime. Po uruchomieniu aplikacji ASP.NET w Azure aplikacji usługi ustawień środowiska automatycznie zastępują ustawienia z Web.config. Ustawienia środowiska są zazwyczaj [bezpieczniejsze do przechowywania informacji poufnych w porównaniu z pliku Web.config](http://www.asp.net/identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure).

6. Kliknij przycisk **Zapisz**.

    ![Kliknij przycisk Zapisz](./media/app-service-api-dotnet-service-principal-auth/appsettings.png)

### <a name="test-the-application"></a>Przetestuj aplikację

1. W przeglądarce przejdź do adresu URL HTTPS aplikację AngularJS frontonu sieci web.

2. Kliknij kartę **Lista zadań do wykonania** i zaloguj się przy użyciu poświadczeń dla użytkownika w dzierżawie usługi Azure AD. 

4. Dodawanie elementów do wykonania, aby sprawdzić, czy aplikacja działa.

    ![Lista zadań do wykonania strony](./media/app-service-api-dotnet-service-principal-auth/mvchome.png)

    Jeśli aplikacja nie działa zgodnie z oczekiwaniami, upewnij się wszystkich ustawień wprowadzone w portalu Azure. Jeśli są wyświetlane wszystkie ustawienia są prawidłowe, zobacz sekcję [Rozwiązywanie problemów](#troubleshooting) w dalszej części tego samouczka.

## <a name="protect-the-api-app-from-browser-access"></a>Ochrona aplikacji interfejsu API z programu access w przeglądarce

Ten samouczek utworzono innego aplikacji Azure AD dla aplikacji interfejsu API ToDoListDataAPI (Warstwa danych). Jak użytkownik zobaczył, podczas tworzenia aplikacji usługi aplikacji AAD, konfiguruje aplikacji AAD w celu umożliwienia użytkownikowi przejdź do adresu URL aplikacji interfejsu API w przeglądarce i zaloguj się. Oznacza to, że jest możliwe dla użytkownika końcowego w dzierżawie Azure AD, nie tylko głównej usługi, aby uzyskać dostęp do API. 

Jeśli chcesz uniemożliwić dostęp przeglądarki bez pisania kodów w chronionych aplikacji interfejsu API, możesz zmienić **Adres URL odpowiedzi** w aplikacji AAD tak, aby różni się od podstawowy adres URL aplikacji interfejsu API. 

### <a name="disable-browser-access"></a>Wyłączanie dostępu w przeglądarce

1. W portalu klasyczny karta **Konfiguruj** dla aplikacji AAD, która została utworzona dla TodoListService Zmień wartość w polu **Adres URL Odpowiedz** tak, aby był prawidłowy adres URL, ale nie adres URL aplikacji interfejsu API.
 
2. Kliknij przycisk **Zapisz**.

### <a name="verify-browser-access-no-longer-works"></a>Upewnij się, że dostęp z przeglądarki przestaje działać

Wcześniej upewnieniu się, że możesz przejść do adresu URL aplikacji interfejsu API w przeglądarce przez zalogowanie się przy użyciu poświadczeń poszczególnych użytkowników. W tej sekcji możesz sprawdzić, czy nie jest to możliwe. 

1. W nowym oknie przeglądarki przejdź do adresu URL aplikacji interfejsu API ponownie.

2. Zaloguj się, gdy zostanie wyświetlony monit, aby to zrobić.

3. Logowanie zakończyło się powodzeniem, ale powoduje wyświetlenie strony błędu.

    Skonfigurowano aplikacji AAD, dzięki czemu użytkownicy w dzierżawie AAD nie można zalogować się i uzyskać dostęp do API przy użyciu przeglądarki. Nadal można korzystać z interfejsu API aplikacji przy użyciu tokenu głównej usługi, które można sprawdzić, przechodząc do adresu URL aplikacji sieci web i dodawanie więcej elementów do wykonania.

## <a name="restrict-access-to-a-particular-service-principal"></a>Ograniczanie dostępu do określonej usługi podmiot zabezpieczeń  

Teraz prawym przyciskiem dowolne rozmówcy, które można uzyskać token dla użytkownika lub wystawcy usługi w dzierżawie usługi Azure AD można zadzwonić aplikacji interfejsu API TodoListDataAPI (Warstwa danych). Warto upewnij się, że aplikacja interfejsu API Warstwa danych akceptuje tylko połączenia z aplikacji interfejsu API TodoListAPI (warstwa środkowa) i tylko z określonej usługi podmiot zabezpieczeń. 

Możesz dodać tych ograniczeń, dodając kod, aby sprawdzić poprawność `appid` i `objectidentifier` roszczeń na połączenia przychodzące.

Ten samouczek umieszczasz kod sprawdza identyfikator aplikacji i identyfikator głównej usługi bezpośrednio w kontrolerze czynności.  Rozwiązania alternatywne mają używać niestandardowego `Authorize` atrybutów lub do wykonania tego sprawdzania poprawności w uruchamiania sekwencji (np. OWIN pośredniczącym). Na przykład tych zobacz [aplikacji przykładowej](https://github.com/mohitsriv/EasyAuthMultiTierSample/blob/master/MyDashDataAPI/Startup.cs). 

Wprowadzić następujące zmiany w projekcie TodoListDataAPI.

2. Otwórz plik *Controllers/TodoListController.cs* .

3. Usuń komentarze wiersze, które Ustawianie `trustedCallerClientId` i `trustedCallerServicePrincipalId`.

        private static string trustedCallerClientId = ConfigurationManager.AppSettings["todo:TrustedCallerClientId"];
        private static string trustedCallerServicePrincipalId = ConfigurationManager.AppSettings["todo:TrustedCallerServicePrincipalId"];

4. Usuń komentarze kod w metodzie CheckCallerId. Ta metoda jest wywoływana na początku każdej metody akcji na kontrolerze. 

        private static void CheckCallerId()
        {
            string currentCallerClientId = ClaimsPrincipal.Current.FindFirst("appid").Value;
            string currentCallerServicePrincipalId = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
            if (currentCallerClientId != trustedCallerClientId || currentCallerServicePrincipalId != trustedCallerServicePrincipalId)
            {
                throw new HttpResponseException(new HttpResponseMessage { StatusCode = HttpStatusCode.Unauthorized, ReasonPhrase = "The appID or service principal ID is not the expected value." });
            }
        }

5. Ponownie wdróż projektu ToDoListDataAPI Azure aplikacji usługi.

6. W przeglądarce przejdź do adresu URL HTTPS aplikacji AngularJS frontonu sieci web, a na stronie głównej kliknij kartę **Lista zadań do wykonania** .

    Aplikacja nie działa, ponieważ występują połączenia w celu przechodzenia wstecz. Nowy kod sprawdza rzeczywisty identyfikator aplikacji i objectidentifier, ale nie masz jeszcze prawidłowe wartości, aby sprawdzić ich przed. Przeglądarki konsoli narzędzia Projektant raportów, że serwer zwraca komunikat o błędzie HTTP 401.

    ![Błąd projektanta narzędzia konsoli](./media/app-service-api-dotnet-service-principal-auth/webapperror.png)

    W następującej procedurze można skonfigurować oczekiwanych wartości.

8. Przy użyciu programu AD PowerShell Azure, Pobierz wartość kapitału usługi aplikacji Azure AD utworzonego dla projektu TodoListWebApp.

    . Aby uzyskać instrukcje dotyczące instalacji Azure programu PowerShell i nawiązać połączenie z subskrypcji, zobacz [Za pomocą programu Azure przy użyciu Menedżera zasobów Azure](../powershell-azure-resource-manager.md).

    b. Aby uzyskać listę głównych usługi, należy wykonać `Login-AzureRmAccount` polecenia, a następnie `Get-AzureRmADServicePrincipal` polecenia.

    c. Znajdowanie identyfikator obiektu dla wystawcy usługi aplikacji TodoListAPI, a następnie zapisz go w lokalizacji, w której można kopiować z później.

7. W portalu Azure przejdź do interfejsu API karta aplikacji dla aplikacji interfejsu API wdrożeniu ToDoListDataAPI projektu.

9. Kliknij pozycję **Ustawienia > Ustawienia aplikacji**.

3. W sekcji **Ustawienia aplikacji** Dodaj następujące klucze i wartości:

  	| **Klawisz** | TODO:TrustedCallerServicePrincipalId |
  	|---|---|
  	| **Wartość** | Identyfikator głównej usługi połączenia aplikacji |
  	| **Przykład** | 4f4a94a4-6f0d-4072-4f4a94a4-6f0d-4072 |

  	| **Klawisz** | TODO:TrustedCallerClientId |
  	|---|---|
  	| **Wartość** | Identyfikator klienta połączenia aplikacja — skopiowane z aplikacji TodoListAPI Azure AD |
  	| **Przykład** | 960adec2-b74a-484a-960adec2-b74a-484a |

6. Kliknij przycisk **Zapisz**.

    ![Kliknij przycisk Zapisz](./media/app-service-api-dotnet-service-principal-auth/trustedcaller.png)

6. W przeglądarce wróć do adresu URL aplikacji sieci web, a na stronie głównej kliknij kartę **Lista zadań do wykonania** .

    Teraz aplikacja działa zgodnie z oczekiwaniami, ponieważ aplikacji zaufanych rozmówcy identyfikator i identyfikator głównej usługi są oczekiwanych wartości.

    ![Lista zadań do wykonania strony](./media/app-service-api-dotnet-service-principal-auth/mvchome.png)

## <a name="building-the-projects-from-scratch"></a>Tworzenia projektów od podstaw

Dwa projekty interfejs API sieci Web zostały utworzone przy użyciu szablonu projektu **Azure interfejsu API aplikacji** , a Zastępowanie kontrolera wartości domyślne kontrolera listy zadań. Uzyskania tokeny głównej usługi Azure AD w programie project ToDoListAPI został zainstalowany pakiet NuGet [Active Directory uwierzytelniania biblioteki (ADAL) dla środowiska .NET](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/) .
 
Aby dowiedzieć się, jak utworzyć aplikację jednostronicowy AngularJS przy użyciu interfejsu API usług Web wewnętrznej takich jak ToDoListAngular, zobacz [ręce na Ćwiczenia: tworzenie jednej strony aplikacji (SPA) przy użyciu interfejsu API sieci Web programu ASP.NET i Angular.js](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs). Aby dowiedzieć się, jak dodawać kod uwierzytelniania Azure AD zobacz [Zabezpieczanie AngularJS pojedynczej strony aplikacji z usługą Azure Active Directory](../active-directory/active-directory-devquickstarts-angular.md).

## <a name="troubleshooting"></a>Rozwiązywanie problemów

[AZURE.INCLUDE [troubleshooting](../../includes/app-service-api-auth-troubleshooting.md)]

* Upewnij się, że użytkownik nie należy mylić ToDoListAPI (warstwa środkowa) i ToDoListDataAPI (Warstwa danych). Na przykład, w tym samouczku możesz dodać uwierzytelniania do danych Warstwa interfejsu API aplikacji, **ale klucz aplikacji muszą pochodzić z poziomu aplikacji Azure AD utworzonego dla aplikacji warstwa środkowa interfejsu API**.

## <a name="next-steps"></a>Następne kroki

To jest ostatni samouczek szeregowo aplikacje interfejsu API. 

Aby uzyskać więcej informacji na temat usługi Azure Active Directory zobacz następujące zasoby.

* [Przewodnik Azure deweloperów AD](http://aka.ms/aaddev)
* [Scenariusze Azure AD](http://aka.ms/aadscenarios)
* [Azure AD próbki](http://aka.ms/aadsamples)

    Przykładowe [Aplikacji sieci Web — WebAPI-uwierzytelnianie OAuth2-AppIdentity-DotNet](http://github.com/AzureADSamples/WebApp-WebAPI-OAuth2-AppIdentity-DotNet) jest podobny do tego, co przedstawiono w tym samouczku, ale bez przy użyciu funkcji uwierzytelniania aplikacji usługi.

Aby uzyskać informacje o innych sposobach Wdroż projekty Visual Studio interfejsu API aplikacje, używając programu Visual Studio lub [Automatyzowanie wdrażania](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery) z [systemu kontroli źródła](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control)zobacz [jak wdrożyć aplikację Azure aplikacji usługi](../app-service-web/web-sites-deploy.md).
