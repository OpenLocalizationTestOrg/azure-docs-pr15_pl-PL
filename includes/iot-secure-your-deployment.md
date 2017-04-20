# <a name="securing-your-iot-deployment"></a>Zabezpieczanie wdrożenia IoT

Ten artykuł zawiera następny poziom szczegółów dla Zabezpieczanie infrastruktury na podstawie Azure IoT Internetu rzeczy (IoT). Łączy do poziomu szczegółów implementacji dla konfigurowania i wdrażania każdego składnika. Umożliwia także porównań i wybór między różne metody konkurencyjnych.

Zabezpieczanie wdrożenia Azure IoT można podzielić na następujące obszary zabezpieczeń trzech:

- **Bezpieczeństwo urządzeń**: Zabezpieczanie urządzenia IoT, podczas gdy jest wdrożony w środowisku naturalnym.
- **Zabezpieczenia połączeń**: zapewnienie wszystkie dane przesyłane między urządzeniem IoT a Centrum IoT jest poufny i fałszerstwami.
- **Zabezpieczenia w chmurze**: zapewnienie środków do zabezpieczania danych, podczas gdy porusza się i są przechowywane w chmurze.

![Trzy obszary zabezpieczeń][img-overview]

## <a name="secure-device-provisioning-and-authentication"></a>Bezpieczne udostępnianie urządzeń i uwierzytelniania

Pakietem IoT Azure zabezpieczenie urządzeń IoT przy użyciu następujących dwóch metod:

- Zapewniając klucz unikatowy tożsamości (tokenów zabezpieczających) dla każdego urządzenia, który może służyć przez urządzenie do komunikowania się z Centrum IoT.

- Za pomocą na urządzeniu [certyfikatu X.509] [ lnk-x509] i klucz prywatny jest środkiem do uwierzytelnić urządzenie do Centrum IoT. Ta metoda uwierzytelniania zapewnia, że klucza prywatnego na urządzeniu nie wiadomo poza urządzeniem w dowolnym czasie, zapewniając wyższy poziom zabezpieczeń.

Metoda tokenu zabezpieczeń zapewnia uwierzytelnianie dla każdego wywołania wykonane przez urządzenie do Centrum IoT przez skojarzenie klucza symetrycznego do każdego wywołania. Uwierzytelnianie oparte na protokole X.509 pozwala na uwierzytelnianie urządzenia IoT w warstwie fizycznej jako część ustanawianie połączenia TLS. Metoda na podstawie tokenu zabezpieczeń może służyć bez uwierzytelniania X.509, który jest mniej bezpieczny wzorem. Wybór między dwiema metodami jest przede wszystkim podyktowane jak bezpieczne urządzenia uwierzytelniania musi być i dostępność bezpiecznego przechowywania na urządzeniu (Aby bezpiecznie przechowywać klucz prywatny).

## <a name="iot-hub-security-tokens"></a>Centrum IoT tokenów zabezpieczających

Centrum IoT używa tokenów zabezpieczeń do uwierzytelniania urządzeń i usług, aby uniknąć wysyłania kluczy w sieci. Ponadto tokenów zabezpieczających są ograniczone w czasie ważności i zakres. Zestawy SDK platformy Azure koncentrator IoT automatycznego generowania tokenów bez konieczności specjalnej konfiguracji. Niektóre scenariusze wymagają jednak użytkownika do generowania i użyć bezpośrednio tokenów zabezpieczających. Należą do nich bezpośredniego wykorzystania powierzchni MQTT, AMQP lub HTTP lub wykonania wzorca usługi tokenu.

Więcej informacji na temat struktury token zabezpieczający oraz jego użytkowania można znaleźć w następujących artykułach:

-   [Struktura tokenu zabezpieczeń][lnk-security-tokens]
-   [Jako urządzenie przy użyciu tokenów SAS][lnk-sas-tokens]

Każdego centrum IoT ma [Rejestru tożsamość urządzeń] [ lnk-identity-registry] może być używany do tworzenia zasobów na urządzenie w usłudze, takich jak kolejki zawierającej wiadomości urządzenia cloud podczas lotu i aby zezwolić na dostęp do punktów końcowych urządzeń skierowaną. Centrum IoT rejestru tożsamość pozwala na bezpieczne przechowywanie tożsamości urządzeń i kluczy zabezpieczeń dla rozwiązania. Indywidualne lub grupy tożsamości urządzeń można dodać do listy dozwolonych lub listę zablokowanych, umożliwiając pełną kontrolę nad dostęp do urządzeń. Następujące artykuły zawierają bardziej szczegółowe informacje o strukturze rejestru tożsamość urządzeń i obsługiwane operacje.

[Centrum IoT obsługuje protokoły, takie jak MQTT, AMQP i HTTP][lnk-protocols]. Każdy z tych protokołów należy użyć tokenów zabezpieczających z urządzenia IoT Centrum IoT inaczej:

- AMQP: SASL zwykłe i AMQP oświadczeń zabezpieczeń ({policyName}@sas.root.{iothubName} w przypadku tokenów koncentrator poziomu; {deviceId} w przypadku tokeny o zakresie urządzenia).

- MQTT: POŁĄCZ używa pakietów {deviceId} jako {ClientId} {IoThubhostname} / {deviceId} w polu **Nazwa użytkownika** i token SAS w polu **hasło** .

- HTTP: Nieprawidłowy token jest w nagłówku żądania autoryzacji.

IoT Hub urządzenia tożsamości rejestru może służyć do konfigurowania poświadczeń zabezpieczeń na urządzenie i kontroli dostępu. Jednakże jeśli rozwiązania ma już znacznych inwestycji w [System rejestru i/lub uwierzytelniania tożsamości niestandardowe urządzenie][lnk-custom-auth], mogą być włączone do istniejącej infrastruktury z Centrum IoT przez tworzenie tokenu usługi.

### <a name="x509-certificate-based-device-authentication"></a>Uwierzytelnianie oparte na certyfikatach urządzenia X.509

Użycie [certyfikatu X.509 opartych na urządzeniach] [ lnk-use-x509] i jego skojarzony prywatny i publiczny pary kluczy umożliwia dodatkowe uwierzytelnianie w warstwie fizycznej. Klucz prywatny są bezpiecznie przechowywane w urządzeniu, a nie jest wykrywalne poza urządzeniem. Certyfikat X.509 zawiera informacje o urządzenie, takie jak identyfikator urządzenia i inne szczegóły organizacyjne. Podpis certyfikatu jest generowany przy użyciu klucza prywatnego.

Urządzenia wysokiego poziomu przepływu inicjowania obsługi administracyjnej:

- Skojarzyć identyfikator do fizycznego urządzenia — urządzenia tożsamości i/lub związane z urządzeniem podczas urządzeń produkcyjnych lub oddanie certyfikatu X.509.

- Tworzenie odpowiedniego wpisu tożsamości w Centrum IoT — urządzenia tożsamości i informacje skojarzone urządzenie w rejestrze urządzenia IoT koncentratora.

- W rejestrze urządzenia Centrum IoT zapewniają bezpieczne przechowywanie odcisk palca certyfikatu X.509.

### <a name="root-certificate-on-device"></a>Certyfikat główny na urządzeniu

Podczas nawiązywania bezpiecznego połączenia TLS z Centrum IoT, urządzenia IoT uwierzytelnia Centrum IoT za pomocą certyfikat główny, który jest częścią zestawu SDK urządzenia. Zestaw SDK klienta C certyfikat znajduje się w folderze o nazwie "\\c\\certyfikatów" w katalogu głównym repo. Chociaż te główne certyfikaty są długotrwałe, one nadal mogą stracić ważność lub zostać odwołany. Nie istnieje sposób aktualizowania certyfikatów na urządzeniu, urządzenie może nie być w stanie później połączyć się Centrum IoT (lub inne usługi w chmurze). Posiadanie środków do aktualizacji certyfikatu głównego po wdrożeniu urządzenia IoT skutecznie pozwala zmniejszyć to zagrożenie.

## <a name="securing-the-connection"></a>Zabezpieczanie połączeń

Połączenie z Internetem urządzenia IoT i Centrum IoT jest zabezpieczony za pomocą standardowego protokołu zabezpieczeń TLS (Transport Layer). Azure IoT obsługuje [TLS 1.2][lnk-tls12], TLS 1.1 i TLS 1.0, w tej kolejności. Obsługę protokołu TLS 1.0 zapewnia zgodność z poprzednimi wersjami. Zaleca się używać protokołu TLS 1.2, ponieważ to daje najwięcej zabezpieczeń.

Pakiet iot Azure obsługuje następujących mechanizmów szyfrowania, w tej kolejności.

| Mechanizmy szyfrowania | Długość |
|--------------|--------|
| TLS\_ECDHE\_RSA\_z\_AES\_256\_CBC\_SHA384 secp384r1 ECDH (0xc028) (korektora FS 7680 bits RSA) | 256    |
| TLS\_ECDHE\_RSA\_z\_AES\_128\_CBC\_SHA256 secp256r1 ECDH (0xc027) (korektora FS 3072 bits RSA) | 128    |
| TLS\_ECDHE\_RSA\_z\_AES\_256\_CBC\_SHA (0xc014) ECDH secp384r1 (korektora FS 7680 bits RSA)           | 256    |
| TLS\_ECDHE\_RSA\_z\_AES\_128\_CBC\_SHA (0xc013) ECDH secp256r1 (korektora FS 3072 bits RSA)           | 128    |
| TLS\_RSA\_z\_AES\_256\_GCM\_SHA384 (0x9d) | 256    |
| TLS\_RSA\_z\_AES\_128\_GCM\_SHA256 (0x9c) | 128    |
| TLS\_RSA\_z\_AES\_256\_CBC\_SHA256 (0x3d) | 256    |
| TLS\_RSA\_z\_AES\_128\_CBC\_SHA256 (0x3c) | 128    |
| TLS\_RSA\_z\_AES\_256\_CBC\_SHA (0x35)    | 256    |
| TLS\_RSA\_z\_AES\_128\_CBC\_SHA (0x2f)    | 128    |
| TLS\_RSA\_z\_3DES\_EDE\_CBC\_SHA (0xa)    | 112    |

## <a name="securing-the-cloud"></a>Zabezpieczanie chmury

Centrum IoT Azure pozwala na określenie [zasad kontroli dostępu] [ lnk-protocols] dla każdego klucza zabezpieczeń. Aby udzielić dostępu do każdego z punktów końcowych Centrum IoT używa następujący zestaw uprawnień. Uprawnienia ograniczyć dostęp do Centrum iot — oparte na funkcji.

- **RegistryRead**. Udziela dostępu do odczytu do rejestru tożsamości urządzenia. Aby uzyskać więcej informacji, zobacz [Rejestr tożsamości urządzenia][lnk-identity-registry].

- **RegistryReadWrite**. Udziela uprawnienia odczytu i zapisu w rejestrze tożsamości urządzenia. Aby uzyskać więcej informacji, zobacz [Rejestr tożsamości urządzenia][lnk-identity-registry].

- **ServiceConnect**. Dotacje dostęp wychodzący usługi komunikacji i monitorowania punkty końcowe w chmurze. Na przykład udziela uprawnienia do usług w chmurze back-end do odbierania wiadomości urządzenia do chmury, wysyłać wiadomości urządzenia cloud i pobrać odpowiedni potwierdzeń dostarczenia.

- **DeviceConnect**. Przyznającą dostęp do punktów końcowych komunikacji skierowaną w urządzeniu. Na przykład udziela uprawnień do wiadomości urządzenia do chmury wysyłania i odbierania wiadomości chmury do urządzeń. To uprawnienie jest używane przez urządzenia.

Istnieją dwa sposoby uzyskania uprawnień **DeviceConnect** z Centrum IoT tokeny [zabezpieczające][lnk-sas-tokens]: za pomocą klucza tożsamości urządzenia lub klucza zasad dostępu współdzielonego. Ponadto, ważne jest, aby pamiętać, że wszystkie funkcje dostępne z urządzeń jest udostępniane przez projekt na punktach końcowych z prefiksem `/devices/{deviceId}`.

[Składniki usługi można tylko wygenerować tokeny zabezpieczające] [ lnk-service-tokens] za pomocą udostępnionych zasad dostępu udzielić odpowiednich uprawnień.

Centrum IoT Azure i innych usług, które mogą być częścią rozwiązania umożliwiają zarządzanie użytkownikami za pomocą Azure Active Directory.

Danych przetwarzanych przez Centrum IoT Azure mogą być używane przez różne usługi, takie jak analiza strumienia Azure i magazynu obiektów blob Azure. Usługi te umożliwiają dostęp do funkcji zarządzania. Dowiedz się więcej o tych usług i dostępne opcje poniżej:

- [Azure DocumentDB][lnk-docdb]: Usługa bazy danych skalowalne, pełni indeksowane półstrukturalnych danych, który zarządza metadanych dla urządzeń zastrzegania, takich jak atrybuty, konfiguracji i właściwości zabezpieczeń. DocumentDB oferuje przetwarzania wysokiej wydajności i wysokiej przepustowości, niezależnej od schematu indeksowania danych i bogaty interfejs kwerendy SQL.

- [Analiza strumienia Azure][lnk-asa]: strumienia w czasie rzeczywistym przetwarzanie w chmurze, która umożliwia szybkie tworzenie i wdrażanie rozwiązania analytics tanich uzyskiwanie w czasie rzeczywistym szczegółowych informacji z urządzeń, czujników, infrastruktury i aplikacji. Dane z tej usługi w pełni zarządzane mogą być skalowane do dowolnej wielkości przy jednoczesnym zachowaniu wysokiej przepustowości, małe opóźnienia i elastyczność.

- [Usług Azure aplikacji][lnk-appservices]: platformy w chmurze do budowania sieci web i aplikacji mobilnych, które połączyć się z danymi w dowolnym miejscu; w chmurze lub lokalnie. Twórz interesujące aplikacje mobilne dla iOS, Android i Windows. Integracja z oprogramowaniem jako usługa (SaaS) i aplikacje dla przedsiębiorstw z łącznością out-of--box do kilkudziesięciu usług w chmurze i aplikacji dla przedsiębiorstw. Kod w języku ulubionych i IDE (.NET, NodeJS, PHP, Python lub Java) do tworzenia interfejsów API i aplikacje sieci web szybciej niż kiedykolwiek.

- [Aplikacje logiki][lnk-logicapps]: funkcja aplikacji logiki usługi aplikacji Azure pomaga zintegrować rozwiązanie IoT do istniejących systemów biznesowych i zautomatyzować procesy przepływu pracy. Aplikacje logiki pozwala deweloperom na projektowanie przepływów pracy, które rozpocząć od wyzwalacza, a następnie wykonać pewne czynności — reguł i akcji, które umożliwia wydajne złącza zintegrować z procesów biznesowych. Aplikacje logiki oferuje out of box łączność z obszernym ekosystemem władz akredytacji bezpieczeństwa, oparte na chmurze i aplikacji lokalnych.

- [Magazynu obiektów blob Azure][lnk-blob]: niezawodne i ekonomiczne w chmurze dla danych, które urządzenia Wyślij do chmury.

## <a name="conclusion"></a>Wniosek

Ten artykuł zawiera omówienie implementacji poziom szczegółów dotyczące projektowania i wdrażania infrastruktury IoT za pomocą Azure IoT. Konfigurowanie każdy składnik ma być bezpieczna jest kluczem do zabezpieczania całej infrastruktury IoT. Projekt, opcje dostępne w Centrum deweloperów IoT zapewnia pewien poziom elastyczności i wyboru; Jednak każdy wybór może mieć wpływ na zabezpieczenia. Zalecane jest, że każdej z tych opcji można ocenić poprzez ocenę ryzyka i kosztów.

[img-overview]: media/iot-secure-your-deployment/overview.png

[lnk-security-tokens]: ../articles/iot-hub/iot-hub-devguide-security.md#security-token-structure
[lnk-sas-tokens]: ../articles/iot-hub/iot-hub-devguide-security.md#use-sas-tokens-as-a-device
[lnk-identity-registry]: ../articles/iot-hub/iot-hub-devguide-identity-registry.md
[lnk-protocols]: ../articles/iot-hub/iot-hub-devguide-security.md
[lnk-custom-auth]: ../articles/iot-hub/iot-hub-devguide-security.md#custom-device-authentication
[lnk-x509]: http://www.itu.int/rec/T-REC-X.509-201210-I/en
[lnk-use-x509]: ../articles/iot-hub/iot-hub-devguide-security.md
[lnk-tls12]: https://tools.ietf.org/html/rfc5246
[lnk-service-tokens]: ../articles/iot-hub/iot-hub-devguide-security.md#using-security-tokens-from-service-components
[lnk-docdb]: https://azure.microsoft.com/services/documentdb/
[lnk-asa]: https://azure.microsoft.com/services/stream-analytics/
[lnk-appservices]: https://azure.microsoft.com/services/app-service/
[lnk-logicapps]: https://azure.microsoft.com/services/app-service/logic/
[lnk-blob]: https://azure.microsoft.com/services/storage/
