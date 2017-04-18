Większość błędy uwierzytelniania w czasie wynika z ustawienia konfiguracji niepoprawne lub niespójne. Poniżej przedstawiono kilka wskazówek określone czynności do wykonania.

* Upewnij się, że nie przeoczyć dowolnym przycisk **Zapisz** . Często jest łatwe do, a wynik to będzie można patrzy prawidłowe wartości na stronie portalu, ale faktycznie nie zostały zapisane w środowisku Azure lub Azure AD aplikacji.
* Dla ustawienia skonfigurowane w karta **Ustawienia aplikacji** portalu Azure upewnij się, że poprawne aplikacji interfejsu API lub aplikacji sieci web został wybrany przy ustawienia zostały wprowadzone.  Również upewnij się, że ustawienia zostały wprowadzone jako **ustawień aplikacji** i nie **ciągów połączeń**, jak format dwie sekcje jest podobne.
* Do uwierzytelniania fronton języka JavaScript, Pobierz manifest ponownie, aby sprawdzić, czy `oauth2AllowImplicitFlow` pomyślnie została zmieniona na `true`.
* Sprawdź używane HTTPS, gdziekolwiek skonfigurowane adresy URL:

    * W kodzie projektu
    * W CORS
    * W obszarze Ustawienia aplikacji Azure środowiska dla każdego interfejsu API aplikacji i aplikacji web app
    * W obszarze Ustawienia aplikacji Azure AD.
    
    Należy zauważyć, że po skopiowaniu adres URL aplikacji interfejsu API z portalu często ma `http://` i trzeba ręcznie zmień ją na `https://`.

* Upewnij się, czy pomyślnie wdrożono zmiany kodu. Na przykład w rozwiązaniu wielu projektów istnieje możliwość Zmień kod projektu i przypadkowo wybierz jedną z innych, jeśli zamierzasz wdrożyć zmiany.
* Upewnij się, że ma być HTTPS adresy URL w przeglądarce nie adresów URL HTTP. Domyślnie program Visual Studio tworzy Publikowanie profilów przy użyciu adresu URL protokołu HTTP i jest to co zostanie otwarty w przeglądarce po wdrożeniu projektu.
* Uwierzytelniania fronton JavaScript upewnij się, że CORS została skonfigurowana w aplikacji interfejsu API, który wywołuje kod JavaScript. W razie wątpliwości, czy problem jest związany z CORS, spróbuj "*" jako dozwolone źródłowy adres URL. 
* Dla języka JavaScript fronton Otwórz kartę Deweloper narzędzia konsoli przeglądarki, aby uzyskać więcej informacji o błędzie, a następnie sprawdź żądania HTTP sieci. Jednak konsoli komunikaty o błędach mogą być mylące. Jeśli zostanie wyświetlony komunikat z informacją o błędzie CORS, rzeczywistą problemu może być uwierzytelniania. Możesz sprawdzić, jeśli jest to możliwe, uruchamiając aplikację z uwierzytelnianiem tymczasowo tymczasowo wyłączone.
* Dla aplikacji interfejsu API .NET upewnij się, że występują tyle informacji w komunikatach o błędach możliwie przez ustawienie [customErrors tryb wyłączony](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#remoteview).
* Dla aplikacji dla interfejsu API programu .NET Rozpocznij [zdalną sesję debugowania](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug)i sprawdzić wartości zmiennych, które są przekazywane do kodu, która używa ADAL umożliwia uzyskanie token okaziciela lub kod, który sprawdza roszczeń wobec identyfikator kapitału oczekiwanych usługi. Zauważ, że kod można wybrać wartości konfiguracji z wielu różnych źródeł, dzięki czemu jest możliwe znalezienie niespodzianki w ten sposób. Na przykład możesz ortograficzny `ida:ClientId` jako `ida:ClientID` podczas konfigurowania ustawień środowiska Azure aplikacji usługi, może zostać wyświetlony kod `ida:ClientId` wartości, która jest szukasz z pliku Web.config ignoruje ustawienie Azure aplikacji usługi. 
* Jeśli elementy nie działają w oknie programu Internet Explorer normalny, istniejącego dziennika programu może zakłócać; Funkcja InPrivate a następnie spróbuj Chrome lub Firefox.
