<properties 
    pageTitle="Nawiązywanie połączenia z kontem usługi multimediów przy użyciu programu .NET" 
    description="W tym temacie przedstawiono sposób nawiązywania połączenia z usługi multimediów uisng .NET." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="09/26/2016"
    ms.author="juliako"/>


# <a name="connecting-to-media-services-account-using-media-services-sdk-for-net"></a>Nawiązywanie połączenia z kontem usługi multimediów przy użyciu zestawu SDK usługi multimediów dla środowiska .NET

> [AZURE.SELECTOR]
- [POZOSTAŁE](media-services-rest-connect-programmatically.md)
- [.NET](media-services-dotnet-connect-programmatically.md)


W tym temacie opisano sposób uzyskiwania połączenia programowych usługi multimediów Azure firmy Microsoft są programowania z SDK usługi multimediów dla środowiska .NET.


## <a name="connecting-to-media-services"></a>Nawiązywanie połączenia z usługi multimediów

Aby połączyć się usługi multimediów programowy, musi już skonfigurowane konto Azure, skonfigurowane usługi multimediów z tego konta i następnie skonfiguruj projektu programu Visual Studio rozwoju z SDK usługi multimediów dla środowiska .NET. Aby uzyskać więcej informacji zobacz ustawienia dotyczące rozwoju z SDK usługi multimediów dla środowiska .NET.

Na końcu procesu konfigurowania konta usługi multimediów możesz uzyskać następujące wartości wymagane połączenie. Skorzystaj z poniższych nawiązywanie połączeń programistyczny do usługi multimediów.

- Nazwa konta usługi multimediów.

- Klucz konta usługi multimediów.

Aby znaleźć te wartości, przejdź do portalu zarządzania Azure, wybierz swoje konto usługi multimediów, a następnie kliknij ikonę "**Narzędzia do zarządzania KLUCZAMI**" w dolnej części okna portalu. Klikając ikonę obok każdego pola tekstowego kopiuje wartość do Schowka systemu.


## <a name="creating-a-cloudmediacontext-instance"></a>Tworzenie wystąpienia CloudMediaContext

Aby rozpocząć programowania przed usługi multimediów należy utworzyć w przypadku wystąpienia **CloudMediaContext** reprezentujący kontekstu serwera. **CloudMediaContext** zawiera odwołania do kolekcji ważne, w tym zadania, zasoby, pliki, zasady dostępu i Locator.

>[AZURE.NOTE] Klasa **CloudMediaContext** nie jest wielowątkowość. Czy utworzyć nowy CloudMediaContext na wątku lub zestaw operacji.


CloudMediaContext zawiera pięć overloads konstruktora. Zaleca się używanie konstruktorów **MediaServicesCredentials** jako parametru. Aby uzyskać więcej informacji zobacz **Ponowne używanie tokeny usługi kontroli dostępu** znajdujący się. 

W poniższym przykładzie użyto publicznego konstruktora CloudMediaContext(MediaServicesCredentials credentials):

    // _cachedCredentials and _context are class member variables. 
    _cachedCredentials = new MediaServicesCredentials(
                    _mediaServicesAccountName,
                    _mediaServicesAccountKey);
    
    _context = new CloudMediaContext(_cachedCredentials);


## <a name="reusing-access-control-service-tokens"></a>Ponowne używanie tokeny usługi kontroli dostępu

W tej sekcji przedstawiono sposób ponowne używanie tokeny usługi kontroli dostępu za pomocą konstruktorów CloudMediaContext MediaServicesCredentials jako parametru.


[Kontrola dostępu usługi Azure Active Directory](https://msdn.microsoft.com/library/hh147631.aspx) (nazywane także usługa Kontrola dostępu lub ACS) to usługa opartej na chmurze, która umożliwia łatwe uwierzytelniania i autoryzacji użytkowników do uzyskania dostępu do swoich aplikacji sieci web. Usługi multimediów Azure Microsoft sterowanie dostępem do swoich usług jednak protokół OAuth, która wymaga tokenu ACS. Usługi multimediów otrzymuje tokeny ACS z serwera autoryzacji.

Podczas tworzenia z SDK usługi multimediów, użytkownik może nie dotyczyć tokenów, ponieważ zestawu SDK kod menedżerów je dla Ciebie. Jednak co SDK pełni zarządzać tokeny ACS prowadzi do niepotrzebne token żądania. Żądania tokenów czas i powoduje zużycie zasobów klienta i serwera. Ponadto na serwerze ACS ogranicza żądania, jeśli stopa jest zbyt duża. Limit wynosi 30 żądania na sekundę, zobacz [Ograniczenia dotyczące usług ACS](https://msdn.microsoft.com/library/gg185909.aspx) uzyskać więcej szczegółowych informacji.

Począwszy od SDK usługi multimediów wersji 3.0.0.0, można ponownie użyć tokeny ACS. Konstruktory **CloudMediaContext** , przyjmujących **MediaServicesCredentials** jako parametr Włączanie funkcji udostępniania tokenów ACS między wieloma konteksty. Klasy MediaServicesCredentials obejmuje poświadczeń usługi multimediów. Jeśli ACS token jest dostępny i jest znany jego czas wygaśnięcia, można utworzyć nowe wystąpienie MediaServicesCredentials przy użyciu tokenu i przekazać je do konstruktora CloudMediaContext. Należy zauważyć, że SDK usługi multimediów automatycznie odświeża tokenów, gdy wygasną. Istnieją dwa sposoby ponowne tokeny ACS, jak pokazano w poniższym przykładzie.

- Może buforować obiektu **MediaServicesCredentials** w pamięci (na przykład w zmiennej klasy statycznej). Następnie należy przekazać obiektu w pamięci podręcznej do konstruktora CloudMediaContext. Obiekt MediaServicesCredentials zawiera token ACS, który może nastąpić, jeśli jest nadal ważna. Jeśli token nie jest prawidłowa, zostaną odświeżone przez Media SDK usługi przy użyciu poświadczeń dostarczonych do konstruktora MediaServicesCredentials.

    Uwaga obiekt **MediaServicesCredentials** otrzymuje prawidłowy token, po nosi nazwę RefreshToken. **CloudMediaContext** wywołuje metodę **RefreshToken** w konstruktorze. Jeśli planujesz Zapisz token wartości pamięci zewnętrznej, upewnij się sprawdzić, czy wartość TokenExpiration jest prawidłowa, przed zapisaniem token danych. Jeśli nie jest prawidłowa, zadzwoń RefreshToken przed pamięci podręcznej.

        // Create and cache the Media Services credentials in a static class variable.
        _cachedCredentials = new MediaServicesCredentials(_mediaServicesAccountName, _mediaServicesAccountKey);

        
        // Use the cached credentials to create a new CloudMediaContext object.
        if(_cachedCredentials == null)
        {
            _cachedCredentials = new MediaServicesCredentials(_mediaServicesAccountName, _mediaServicesAccountKey);
        }
        
        CloudMediaContext context = new CloudMediaContext(_cachedCredentials);

- Można buforować ciąg AccessToken i wartości TokenExpiration. Aby utworzyć nowy obiekt MediaServicesCredentials zapisujące dane token można później wartości.  To jest szczególnie przydatne w przypadku scenariuszy miejsce, w którym tokenu można bezpiecznie udostępniać wielu procesów lub komputerach.

    Następujące fragmenty kodu połączeń metod SaveTokenDataToExternalStorage, GetTokenDataFromExternalStorage i UpdateTokenDataInExternalStorageIfNeeded, które nie są zdefiniowane w tym przykładzie. Można zdefiniować tych metod do przechowywania, pobieranie i aktualizowanie danych tokenu w pamięci zewnętrznej. 

        CloudMediaContext context1 = new CloudMediaContext(_mediaServicesAccountName, _mediaServicesAccountKey);
        
        // Get token values from the context.
        var accessToken = context1.Credentials.AccessToken;
        var tokenExpiration = context1.Credentials.TokenExpiration;
        
        // Save token values for later use. 
        // The SaveTokenDataToExternalStorage method should check 
        // whether the TokenExpiration value is valid before saving the token data. 
        // If it is not valid, call MediaServicesCredentials’s RefreshToken before caching.
        SaveTokenDataToExternalStorage(accessToken, tokenExpiration);
        
    Umożliwia utworzenie MediaServicesCredentials zapisane wartości token.


        var accessToken = "";
        var tokenExpiration = DateTime.UtcNow;
        
        // Retrieve saved token values.
        GetTokenDataFromExternalStorage(out accessToken, out tokenExpiration);
        
        // Create a new MediaServicesCredentials object using saved token values.
        MediaServicesCredentials credentials = new MediaServicesCredentials(_mediaServicesAccountName, _mediaServicesAccountKey)
        {
            AccessToken = accessToken,
            TokenExpiration = tokenExpiration
        };
        
        CloudMediaContext context2 = new CloudMediaContext(credentials);

    Zaktualizuj token kopię, w przypadku, gdy token został zaktualizowany przez SDK usługi multimediów. 
    
        if(tokenExpiration != context2.Credentials.TokenExpiration)
        {
            UpdateTokenDataInExternalStorageIfNeeded(accessToken, context2.Credentials.TokenExpiration);
        }
        

- Jeśli masz wiele kont usługi multimediów (na przykład w przypadku udostępniania celów lub Geo dystrybucji obciążenia) można pamięci podręcznej obiektów MediaServicesCredentials za pomocą kolekcji System.Collections.Concurrent.ConcurrentDictionary (zbioru ConcurrentDictionary reprezentuje kolekcję bezpiecznych wątku par klucz wartość, które mogą być udostępniane przez wiele wątków jednocześnie). Metoda GetOrAdd umożliwia następnie uzyskać poświadczenia z pamięci podręcznej. 

        // Declare a static class variable of the ConcurrentDictionary type in which the Media Services credentials will be cached.  
        private static readonly ConcurrentDictionary<string, MediaServicesCredentials> mediaServicesCredentialsCache = 
            new ConcurrentDictionary<string, MediaServicesCredentials>();
        

        // Cache (or get already cached) Media Services credentials. Use these credentials to create a new CloudMediaContext object.
        static public CloudMediaContext CreateMediaServicesContext(string accountName, string accountKey)
        {
            CloudMediaContext cloudMediaContext;
            MediaServicesCredentials mediaServicesCredentials;
        
            mediaServicesCredentials = mediaServicesCredentialsCache.GetOrAdd(
                accountName,
                valueFactory => new MediaServicesCredentials(accountName, accountKey));
        
            cloudMediaContext = new CloudMediaContext(mediaServicesCredentials);
        
            return cloudMediaContext;
        }
        
## <a name="connecting-to-a-media-services-account-located-in-the-north-china-region"></a>Nawiązywanie połączenia z kontem usługi multimediów znajduje się w regionie Północnej Chinach

Jeśli Twoje konto znajduje się w regionie Północnej Chiny, za pomocą konstruktora następujące czynności:

    public CloudMediaContext(Uri apiServer, string accountName, string accountKey, string scope, string acsBaseAddress)

Na przykład:


    _context = new CloudMediaContext(
        new Uri("https://wamsbjbclus001rest-hs.chinacloudapp.cn/API/"),
        _mediaServicesAccountName,
        _mediaServicesAccountKey,
        "urn:WindowsAzureMediaServices",
        "https://wamsprodglobal001acs.accesscontrol.chinacloudapi.cn");


## <a name="storing-connection-values-in-configuration"></a>Przechowywanie wartości połączenia w konfiguracji

Jest zdecydowanie zaleca do przechowywania wartości połączenia, zwłaszcza poufnych wartości, takie jak nazwa konta i hasło, w konfiguracji. Ponadto jest zaleca szyfrowanie poufne dane konfiguracyjne. Plik konfiguracyjny całego można szyfrowanie przy użyciu (Windows szyfrowania plików systemowych). Aby włączyć system szyfrowania plików w pliku, kliknij prawym przyciskiem myszy plik, wybierz pozycję **Właściwości**, a następnie włączyć szyfrowanie na karcie Ustawienia **Zaawansowane** . Lub można utworzyć niestandardowe rozwiązanie do szyfrowania zaznaczonych części pliku konfiguracji przy użyciu konfiguracji chroniony. Zobacz [szyfrowania konfiguracja informacji za pomocą chronionego konfiguracji](https://msdn.microsoft.com/library/53tyfkaw.aspx).

Następujące pliku App.config zawiera wartości wymagane połączenie. Wartości w <appSettings> elementu są wymagane wartości, które masz od procesu konfigurowania konta usługi multimediów.

    <configuration>
      <appSettings>
        <add key="MediaServicesAccountName" value="Media-Services-Account-Name" />
        <add key="MediaServicesAccountKey" value="Media-Services-Account-Key" />
      </appSettings>
    </configuration>


Aby pobrać wartości połączenia z konfiguracji, można użyć klasy **ConfigurationManager** i przypisując wartości do pól w kodzie:
    
    private static readonly string _accountName = ConfigurationManager.AppSettings["MediaServicesAccountName"];
    private static readonly string _accountKey = ConfigurationManager.AppSettings["MediaServicesAccountKey"];



##<a name="media-services-learning-paths"></a>Ścieżek nauki usługi multimediów

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
