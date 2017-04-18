<properties
 pageTitle="Przewodnik dewelopera — kontrolowanie dostępu do Centrum IoT | Microsoft Azure"
 description="Przewodnik Azure IoT Centrum deweloperów — jak kontrolowanie dostępu do Centrum IoT i zarządzać zabezpieczeniami"
 services="iot-hub"
 documentationCenter=".net"
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="dobett"/>

# <a name="control-access-to-iot-hub"></a>Kontrolowanie dostępu do Centrum IoT

## <a name="overview"></a>Omówienie

W tym artykule opisano opcje dotyczące zabezpieczania Twoim Centrum IoT. Centrum IoT są stosowane *uprawnienia* do udzielania dostępu do punktów końcowych każdego IoT Centrum. Limit uprawnień dostępu do koncentratora IoT w oparciu o funkcjonalność.

Ten artykuł zawiera opis:

- Inne uprawnienia może udzielić aplikacji dla urządzeń lub wewnętrznej uzyskiwania dostępu do Centrum IoT.
- Proces uwierzytelniania i tokeny używa w celu zweryfikowania uprawnień.
- Jak zakres poświadczenia, aby ograniczyć dostęp do określonych zasobów.
- Obsługa Centrum IoT certyfikaty X.509.
- Mechanizmy uwierzytelniania niestandardowe urządzenie korzystające z istniejących rejestry tożsamości urządzenia lub schematów uwierzytelniania.

### <a name="when-to-use"></a>Kiedy należy używać

Musi mieć odpowiednie uprawnienia ma dostęp do wszystkich punktów końcowych IoT Centrum. Na przykład urządzenie musi zawierać tokenu zawierającej poświadczeń zabezpieczeń wraz z każdej wiadomości wysyłane do koncentratora IoT.

## <a name="access-control-and-permissions"></a>Kontrola dostępu i uprawnienia

Możesz przyznać [uprawnienia](#iot-hub-permissions) w następujący sposób:

* **Poziom Centrum udostępnione zasady dostępu**. Zasady dostępu udostępnione można udzielać dowolną kombinację [uprawnień](#iot-hub-permissions). Zasady można zdefiniować w [Azure portal][lnk-management-portal], lub programowo przy użyciu [IoT Centrum zasobów dostawcy interfejsów API pozostałych][lnk-resource-provider-apis]. Nowo utworzony Centrum IoT występują następujące zasady domyślne:

    - **iothubowner**: zasady z wszystkie uprawnienia.
    - **Usługa**: zasady z uprawnieniem ServiceConnect.
    - **urządzenia**: zasady z uprawnieniem DeviceConnect.
    - **registryRead**: zasady z uprawnieniem RegistryRead.
    - **registryReadWrite**: zasad z uprawnieniami RegistryRead i RegistryWrite.


* **Poświadczenia zabezpieczeń na urządzenie**. Każdy Centrum IoT zawiera [urządzenia tożsamości rejestru][lnk-identity-registry]. Dla każdego urządzenia w tym rejestru można skonfigurować poświadczenia zabezpieczeń, które udzielanie uprawnień **DeviceConnect** występujące w odpowiednich punkty końcowe urządzenia.

Na przykład w typowe rozwiązanie IoT:

- Składnik Zarządzanie urządzenie używa zasady *registryReadWrite* .
- Składnik procesor zdarzeń używa zasad *usługi* .
- Składnik logiczny biznesowy urządzenia środowisko uruchomieniowe używa zasad *usługi* .
- Poszczególnych urządzeń nawiązywanie połączenia przy użyciu poświadczeń przechowywanych w rejestrze tożsamości Centrum IoT.

## <a name="authentication"></a>Uwierzytelnianie

Azure Centrum IoT udziela dostępu do punktów końcowych, sprawdzając tokenu przed zasady dostępu udostępnione i poświadczeń zabezpieczeń rejestru tożsamości urządzenia.

Poświadczenia zabezpieczeń, takich jak symetrycznej klawiszy, nigdy nie są wysyłane przez sieć.

> [AZURE.NOTE] Dostawcy Azure IoT Centrum zasobów jest zabezpieczone za pośrednictwem subskrypcji Azure są wszystkich dostawców w [Menedżera zasobów Azure][lnk-azure-resource-manager].

Aby uzyskać więcej informacji na temat sposobów tworzenia i używania tokenów zabezpieczających zobacz [tokenów zabezpieczających IoT Centrum][lnk-sas-tokens].

### <a name="protocol-specifics"></a>Szczegóły Protocol (protokół)

Każdego obsługiwanych protokołu, takich jak MQTT, AMQP i HTTP, protokoły transportowe tokeny na różne sposoby.

Podczas korzystania z MQTT pakiet POŁĄCZ identyfikator urządzenia nie ma jako ClientId, {iothubhostname}-{identyfikator urządzenia} w polu Nazwa użytkownika i tokenu skojarzeń zabezpieczeń w polu hasło. {iothubhostname} powinny być pełny CName z poziomu Centrum IoT (na przykład devices.net contoso.azure).

Podczas korzystania z [AMQP][lnk-amqp], Centrum IoT obsługuje [Zwykły SASL] [ lnk-sasl-plain] i [AMQP oświadczeniach-zabezpieczenia na poziomie -][lnk-cbs].

Użycie AMQP oświadczeniach zabezpieczenia na poziomie-standard określa sposób przesyłania tych tokenów.

**Nazwa użytkownika** może być dla zwykłego SASL:

* `{policyName}@sas.root.{iothubName}`Jeśli z poziomu Centrum tokeny.
* `{deviceId}@sas.{iothubname}`Jeśli tokeny występujące urządzenia.

W obu przypadkach, pole hasła zawiera token, zgodnie z opisem w [Centrum IoT tokenów zabezpieczających][lnk-sas-tokens].

HTTP wykonuje uwierzytelnianie przy tym prawidłowy token w nagłówku żądania **autoryzacji** .

#### <a name="example"></a>Przykład

Nazwa użytkownika (identyfikator urządzenia jest uwzględniana wielkość liter):`iothubname.azure-devices.net/DeviceId`

Hasło (generuje skojarzeń zabezpieczeń w Eksploratorze urządzenia):`SharedAccessSignature sr=iothubname.azure-devices.net%2fdevices%2fDeviceId&sig=kPszxZZZZZZZZZZZZZZZZZAhLT%2bV7o%3d&se=1487709501`

> [AZURE.NOTE] [Azure IoT Centrum SDK] [ lnk-sdks] automatyczne generowanie tokenów w przypadku nawiązywania połączenia z usługą. W niektórych przypadkach SDK nie obsługują wszystkie protokoły i metody uwierzytelniania.

### <a name="special-considerations-for-sasl-plain"></a>Uwagi dotyczące zwykły SASL

Gdy z AMQP przy użyciu zwykłego SASL, klienta nawiązującego połączenie koncentratora IoT, używając tokenu jednej dla każdego połączenia TCP. Po wygaśnięciu tokenu połączenia TCP odłącza od usługi i uruchamia ponownego połączenia. Takie zachowanie nie problemów składnik wewnętrznej aplikacji jest bardzo szkodliwy dla aplikacji po stronie urządzenia z następujących powodów:

*  Bram ze łączyć imieniu wielu urządzeń. Przy użyciu zwykłego SASL, mają tworzyć różne połączenia TCP dla każde urządzenie łączące do koncentratora IoT. W tym scenariuszu znacznie zwiększa zużycie energii elektrycznej i zasoby sieciowe i zwiększanie opóźnienie każdego połączenia urządzenia.
* Ograniczone zasobów urządzeń negatywnie dotyczy szerszego stosowania zasoby, aby ponownie nawiązać połączenie po każdym wygaśnięcia tokenu.

## <a name="scope-hub-level-credentials"></a>Zakres poziomu Centrum poświadczeń

Zasady zabezpieczeń na poziomie Centrum można ograniczyć, tworząc tokenów z ograniczeniami identyfikator URI zasobu. Na przykład punkt końcowy do wysyłania wiadomości urządzenia w chmurze z urządzenia jest **/devices/ {identyfikator urządzenia}-wiadomości i zdarzeń**. Za pomocą zasad udostępnionego dostępu na poziomie Centrum i **DeviceConnect** uprawnienia do podpisywania tokenu, którego resourceURI jest **/devices/ {identyfikator urządzenia}**. Ta metoda umożliwia utworzenie tokenu, którego można używać do wysyłania wiadomości w imieniu urządzenia **identyfikator urządzenia**tylko.

Ten mechanizm jest podobne do [zasad wydawcy koncentratory zdarzenia][lnk-event-hubs-publisher-policy]i umożliwia wdrożenie metod uwierzytelniania niestandardowego.

## <a name="security-tokens"></a>Tokenów zabezpieczających

Centrum IoT używa tokenów zabezpieczających do uwierzytelniania urządzenia i usługi, aby uniknąć przesyłanie kluczy w sieci. Ponadto tokenów zabezpieczających jest ograniczony czas ważności i zakresie. [Azure SDK Centrum IoT] [ lnk-sdks] automatyczne generowanie tokeny bez konieczności specjalnej konfiguracji. Kilka scenariuszy, jednak wymaga użytkownika do generowania i używać bezpośrednio tokenów zabezpieczających. Dotyczy to użycie bezpośrednio powierzchni MQTT, AMQP lub HTTP lub stosowania deseniu usługi tokenu zgodnie z opisem w przypadku [uwierzytelniania urządzenia niestandardowe][lnk-custom-auth].

Centrum IoT umożliwia urządzenia do uwierzytelnienia z Centrum IoT przy użyciu [certyfikatów X.509][lnk-x509]. 

### <a name="security-token-structure"></a>Struktura tokenu zabezpieczeń
Udzielanie ograniczony czas dostęp do urządzeń i usług do określonych funkcji w Centrum IoT za pomocą tokenów zabezpieczających. Aby upewnić się, że połączyć tylko autoryzowanych urządzenia i usługi, tokenów zabezpieczających muszą być zalogowani przy użyciu klucza zasad udostępniania lub klucz symetrycznej przechowywane urządzenia tożsamości w rejestrze tożsamości.

Token zalogowani za pomocą udostępniania zasad klucza nadaje prawa dostępu do wszystkich funkcji skojarzonych z udostępnionej zasad uprawnienia. Z drugiej strony tokenu podpisane kluczem symetrycznej urządzenia tożsamości udziela uprawnień **DeviceConnect** dla tożsamości skojarzone urządzenie.

Tokenu zabezpieczającego ma następujący format:

    SharedAccessSignature sig={signature-string}&se={expiry}&skn={policyName}&sr={URL-encoded-resourceURI}

Oto oczekiwanych wartości:

| Wartość | Opis |
| ----- | ----------- |
| {podpisu} | Ciąg podpisu HMAC SHA256 w postaci: `{URL-encoded-resourceURI} + "\n" + expiry`. **Ważne**: klucz jest odczytany z base64 i używane jako klucz do wykonywania obliczeń HMAC SHA256. |
| {resourceURI} | Prefiks URI (według segmentu) punktów końcowych, które są dostępne z tym tokenu, zaczynając od hostname Centrum IoT (nie protocol). Na przykład`myHub.azure-devices.net/devices/device1` |
| {wygaśnięcia} | Ciągi UTF8 liczbę sekund od czasu UTC 00:00:00 epoch na 1 stycznia 1970. |
| {Adres URL-zakodowany resourceURI} | Niższe sprawa kodowanie adresów URL identyfikator URI zasobu małe litery |
| {Nazwa_zasady} | Nazwa zasady dostępu udostępnione, do której odwołuje się ten token. Brak w przypadku tokeny odwołujące się do urządzenia rejestru poświadczeń. |

**Uwaga dotycząca prefiks**: Prefiks URI jest obliczana według części, a nie według znaków. Na przykład `/a/b` jest prefiksem dla `/a/b/c` , ale nie `/a/bc`.

Poniższy fragment Node.js zawiera funkcji o nazwie **generateSasToken** , która oblicza token z danych wejściowych `resourceUri, signingKey, policyName, expiresInMins`. Kolejne sekcje szczegółowo jak zainicjować różnych danych wejściowych na wypadek różnych Użyj tokenu.

    var generateSasToken = function(resourceUri, signingKey, policyName, expiresInMins) {
        resourceUri = encodeURIComponent(resourceUri.toLowerCase()).toLowerCase();

        // Set expiration in seconds
        var expires = (Date.now() / 1000) + expiresInMins * 60;
        expires = Math.ceil(expires);
        var toSign = resourceUri + '\n' + expires;

        // Use crypto
        var hmac = crypto.createHmac('sha256', new Buffer(signingKey, 'base64'));
        hmac.update(toSign);
        var base64UriEncoded = encodeURIComponent(hmac.digest('base64'));

        // Construct autorization string
        var token = "SharedAccessSignature sr=" + resourceUri + "&sig="
        + base64UriEncoded + "&se=" + expires;
        if (policyName) token += "&skn="+policyName;
        return token;
    };

Porównanie równoważne kod Python w celu wygenerowania tokenu zabezpieczającego jest jako:

    from base64 import b64encode, b64decode
    from hashlib import sha256
    from time import time
    from urllib import quote_plus, urlencode
    from hmac import HMAC

    def generate_sas_token(uri, key, policy_name, expiry=3600):
        ttl = time() + expiry
        sign_key = "%s\n%d" % ((quote_plus(uri)), int(ttl))
        print sign_key
        signature = b64encode(HMAC(b64decode(key), sign_key, sha256).digest())

        rawtoken = {
            'sr' :  uri,
            'sig': signature,
            'se' : str(int(ttl))
        }

        if policy_name is not None:
            rawtoken['skn'] = policy_name

        return 'SharedAccessSignature ' + urlencode(rawtoken)

> [AZURE.NOTE] Ponieważ czas ważności tokenu jest sprawdzany na komputerach Centrum IoT, ważne jest minimalnego konieczności odchylenie zegara komputera, na którym generuje token.

### <a name="use-sas-tokens-in-a-device-client"></a>W kliencie urządzenia za pomocą tokeny skojarzenia zabezpieczeń

Istnieją dwa sposoby, aby uzyskać uprawnienia **DeviceConnect** z Centrum IoT z tokenów zabezpieczających: przy użyciu [klucza symetrycznej urządzenia z rejestru tożsamości urządzenia](#use-a-symmetric-key-in-the-identity-registry)lub przy użyciu [klucza zasad udostępniania](#use-a-shared-access-policy).

Należy pamiętać, że wszystkie funkcje dostępne w urządzenia są wyświetlane zgodnie z projektem na punkty końcowe z prefiksem `/devices/{deviceId}`.

> [AZURE.IMPORTANT] Jedynym sposobem, że Centrum IoT uwierzytelnia konkretnego urządzenia korzysta z klucza symetrycznej tożsamości urządzenia. W przypadku gdy zasady udostępniania jest używana do korzystania z funkcji urządzenia rozwiązanie należy rozważyć składnika wydawania tokenu zabezpieczającego jako zaufanej podrzędny.

Urządzenie skierowaną punkty końcowe są (niezależnie od protokół):

| Punkt końcowy | Funkcje |
| ----- | ----------- |
| `{iot hub host name}/devices/{deviceId}/messages/events` | Wysyłanie wiadomości urządzenia w chmurze. |
| `{iot hub host name}/devices/{deviceId}/devicebound` | Odbieranie wiadomości w chmurze do urządzenia. |

### <a name="use-a-symmetric-key-in-the-identity-registry"></a>Za pomocą symetrycznej klucza rejestru tożsamości

Podczas korzystania z urządzenia tożsamości symetrycznej klucz do wygenerowania tokenu Nazwa_zasady (`skn`) zostanie pominięty elementem tokenu.

Na przykład tokenu utworzone, aby uzyskać dostęp do wszystkich funkcji urządzenia powinny mieć następujące parametry:

* Identyfikator URI: `{IoT hub name}.azure-devices.net/devices/{device id}`,
* klucz podpisywania: dowolny klawisz symetrycznej dla `{device id}` tożsamości
* Nazwa zasady nie
* dowolny czas wygaśnięcia.

Przykład użycia funkcji węzeł powyżej jest następujący:

    var endpoint ="myhub.azure-devices.net/devices/device1";
    var deviceKey ="...";

    var token = generateSasToken(endpoint, deviceKey, null, 60);

Wynik, która zapewnia dostęp do wszystkich funkcji device1, jest następujący:

    SharedAccessSignature sr=myhub.azure-devices.net%2fdevices%2fdevice1&sig=13y8ejUk2z7PLmvtwR5RqlGBOVwiq7rQR3WZ5xZX3N4%3D&se=1456971697

> [AZURE.NOTE] Istnieje możliwość wygenerowania tokenu zabezpieczeń za pomocą narzędzia .NET [Eksploratora urządzenia][lnk-device-explorer].

### <a name="use-a-shared-access-policy"></a>Za pomocą zasad udostępniania

Podczas tworzenia tokenu z zasad udostępniania, zasady Nazwa pola `skn` musi być ustawiona na nazwę zasady używane. Jest również wymagane, że zasady udziela uprawnień **DeviceConnect** .

Są dwa scenariusze głównym stosowania zasad udostępnionego dostępu do korzystania z funkcji urządzenia:

* [protokół bram w chmurze][lnk-endpoints],
* [Token usług] [ lnk-custom-auth] umożliwia wdrożenie systemów niestandardowego uwierzytelniania.

Ponieważ zasady dostępu udostępnione potencjalnie może udzielić dostępu do połączyć jako dowolnego urządzenia, ważne jest używany poprawny identyfikator URI zasobu do tworzenia tokenów zabezpieczających. Jest to szczególnie ważne dla usług tokenu, które mają zakres token do konkretnego urządzenia za pomocą identyfikatora URI zasobu. Ten punkt jest mniej odpowiednie dla protokołu bram, jak są one już pośredniczących ruch dla wszystkich urządzeń.

Na przykład usługa tokenu za pomocą zasad wstępnie zaprojektowanych udostępnienia o nazwie **urządzenia** spowodować tokenu następujących parametrów:

* Identyfikator URI: `{IoT hub name}.azure-devices.net/devices/{device id}`,
* klucz podpisywania: jeden z kluczy `device` zasad,
* Nazwa zasady: `device`,
* dowolny czas wygaśnięcia.

Przykład użycia funkcji węzeł powyżej jest następujący:

    var endpoint ="myhub.azure-devices.net/devices/device1";
    var policyName = 'device';
    var policyKey = '...';

    var token = generateSasToken(endpoint, policyKey, policyName, 60);

Wynik, która zapewnia dostęp do wszystkich funkcji device1, jest następujący:

    SharedAccessSignature sr=myhub.azure-devices.net%2fdevices%2fdevice1&sig=13y8ejUk2z7PLmvtwR5RqlGBOVwiq7rQR3WZ5xZX3N4%3D&se=1456971697&skn=device

Brama protokołu można użyć niż dla wszystkich urządzeń, po prostu ustawienie identyfikator URI zasobu do `myhub.azure-devices.net/devices`.

### <a name="use-security-tokens-from-service-components"></a>Tokenów zabezpieczających korzystanie ze składników usługi

Składniki usługi tylko mogą powodować zwrócenie tokenów zabezpieczających, korzystając z zasad udostępniania udzielanie odpowiednich uprawnień, zgodnie z opisem wcześniej.

Poniżej przedstawiono funkcje usługi, dostępne w punkty końcowe:

| Punkt końcowy | Funkcje |
| ----- | ----------- |
| `{iot hub host name}/devices` | Tworzenie, aktualizowanie, pobieranie i usuwanie tożsamości urządzenia. |
| `{iot hub host name}/messages/events` | Odbieranie wiadomości urządzenia w chmurze. |
| `{iot hub host name}/servicebound/feedback` | Pozwala uzyskiwać opinie dla wiadomości w chmurze do urządzenia. |
| `{iot hub host name}/devicebound` | Wysyłanie wiadomości w chmurze do urządzenia. |

Na przykład usługa generowanie przy użyciu wstępnie zaprojektowanych udostępnienia zasady o nazwie **registryRead** czy utworzyć tokenu z poniższych parametrów:

* Identyfikator URI: `{IoT hub name}.azure-devices.net/devices`,
* klucz podpisywania: jeden z kluczy `registryRead` zasad,
* Nazwa zasady: `registryRead`,
* dowolny czas wygaśnięcia.

    punkt końcowy wariancja = "myhub.azure-devices.net/devices";   Wariancja Nazwa_zasady = "urządzenia";   var policyKey =...;

    Wariancja token = generateSasToken (punkt końcowy, policyKey Nazwa_zasady, 60);

Wynik, które chcesz udzielić dostępu do odczytu wszystkich tożsamości urządzenia, jest następujący:

    SharedAccessSignature sr=myhub.azure-devices.net%2fdevices&sig=JdyscqTpXdEJs49elIUCcohw2DlFDR3zfH5KqGJo4r4%3D&se=1456973447&skn=registryRead

## <a name="supported-x509-certificates"></a>Obsługiwane certyfikaty X.509

Dowolny certyfikat X.509 służy do uwierzytelniania z urządzenia z Centrum IoT. Ta opcja uwzględnia:

-   **Istniejący certyfikat X.509**. Urządzenie może być już certyfikat X.509 skojarzone z nim. Urządzenie może używać tego certyfikatu do uwierzytelniania IoT Centrum.

-   **Certyfikat X-509 własny wygenerowanych i podpisem**. Producentem lub narzędzie we własnym zakresie wdrażania można wygenerować te certyfikaty i przechowywać odpowiedni klucz prywatny (i certyfikatu) na urządzeniu. Można użyć narzędzi, takich jak [OpenSSL] [ lnk-openssl] i [Windows SelfSignedCertificate] [ lnk-selfsigned] narzędzie do tego celu.

-   **Certyfikat X.509 podpisane przez urząd certyfikacji**. Za pomocą certyfikat X.509 wygenerowane i podpisany przez urząd certyfikacji (CA) do identyfikowania urządzenia i urządzenia z Centrum IoT uwierzytelniania.

Urządzenie może użyć certyfikat X.509 lub tokenu zabezpieczającego uwierzytelniania, ale nie obu.

### <a name="register-an-x509-client-certificate-for-a-device"></a>Zarejestruj się certyfikat X.509 klienta dla urządzenia

[Azure IoT usługi SDK dla C#] [ lnk-service-sdk] (wersja 1.0.8+) obsługuje rejestrowanie urządzenia, który używa certyfikat X.509 klienta do uwierzytelniania. Inne interfejsy API, takich jak Importuj/Eksportuj urządzeń obsługują także certyfikaty klienta X.509.

### <a name="c-support"></a>C\# pomocy technicznej

Klasa **RegistryManager** udostępnia programowe sposoby zarejestrować urządzenie. W szczególności metod **AddDeviceAsync** i **UpdateDeviceAsync** Włączanie użytkownika do rejestrowania i aktualizowania urządzenia w rejestrze Centrum Iot urządzenia tożsamości. Te dwie metody podjąć w przypadku wystąpienia **urządzenia** jako danych wejściowych. Klasy **urządzeń** zawiera właściwość **uwierzytelniania** , która umożliwia użytkownikowi określenie odciski palców certyfikat X.509 głównego i pomocniczego. Odcisku palca reprezentuje skrótu SHA-1 certyfikat X.509 (przechowywane przy użyciu kodowania binarny DER). Użytkownicy mają możliwość określenia podstawowego odcisku palca lub pomocniczej odcisku palca lub oba. Odciski palców głównego i pomocniczego są obsługiwane w celu obsługi scenariuszy najazd certyfikatu.

> [AZURE.NOTE] Centrum IoT nie wymaga ani nie przechowywać całą certyfikat klienta X.509, tylko odcisku palca.

Oto przykładowa C\# wstawkę kodu, aby zarejestrować urządzenie za pomocą certyfikat X.509 klienta:

```
var device = new Device(deviceId)
{
  Authentication = new AuthenticationMechanism()
  {
    X509Thumbprint = new X509Thumbprint()
    {
      PrimaryThumbprint = "921BC9694ADEB8929D4F7FE4B9A3A6DE58B0790B"
    }
  }
};
RegistryManager registryManager = RegistryManager.CreateFromConnectionString(deviceGatewayConnectionString);
await registryManager.AddDeviceAsync(device);
```

### <a name="use-an-x509-client-certificate-during-runtime-operations"></a>Podczas wykonywania operacji za pomocą certyfikat X.509 klienta

[Urządzenie Azure IoT SDK dla środowiska .NET] [ lnk-client-sdk] (wersja 1.0.11+) obsługuje certyfikaty X.509 klienta.

### <a name="c-support"></a>C\# pomocy technicznej

Klasy **DeviceAuthenticationWithX509Certificate** obsługuje tworzenie wystąpień  **DeviceClient** przy użyciu certyfikat X.509 klienta.

Oto przykładowy wstawkę kodu:

```
var authMethod = new DeviceAuthenticationWithX509Certificate("<device id>", x509Certificate);

var deviceClient = DeviceClient.Create("<IotHub DNS HostName>", authMethod);
```

## <a name="custom-device-authentication"></a>Uwierzytelnianie niestandardowe urządzenie

Można użyć Centrum IoT [rejestru tożsamości urządzenia] [ lnk-identity-registry] konfigurowanie na urządzenie poświadczeń zabezpieczeń i kontroli [tokeny]dostępu[lnk-sas-tokens]. Jednak jeśli rozwiązanie IoT już znaczących inwestycji w schemacie rejestru i/lub uwierzytelniania tożsamości niestandardowe urządzenie, można zintegrować ten istniejącej infrastruktury z Centrum IoT, tworząc *usługi tokenu*. W ten sposób można używać innych funkcji IoT w rozwiązaniu.

Usługa tokenu jest usługi w chmurze niestandardowe. Za pomocą Centrum IoT *zasady dostępu udostępnione* z uprawnieniami **DeviceConnect** tworzy tokeny *występujące urządzenia* . Tych tokenów włączyć urządzenie nawiązać połączenie z Centrum IoT.

  ![Procedura wzorzec usług tokenu][img-tokenservice]

Poniżej przedstawiono najważniejsze kroki wzorzec usług tokenu:

1. Tworzenie zasad dostępu do Centrum IoT udostępnione z uprawnieniami **DeviceConnect** do koncentratora IoT. Możesz utworzyć tych zasad w [Azure portal] [ lnk-management-portal] lub programowo. Usługa tokenu używa tych zasad do podpisania tokeny tworzone przez niego.
2. Jeśli urządzenie musi uzyskać dostęp do Centrum IoT, żąda podpisanego tokenu z usługi tokenu. Urządzenie może przeprowadzać uwierzytelnianie z schemat rejestru/uwierzytelniania tożsamości niestandardowe urządzenie do określania tożsamości urządzenia używające usługi tokenu do tworzenia tokenu.
3. Usługa tokenu zwraca token. Token jest tworzony przy użyciu `/devices/{deviceId}` jako `resourceURI`, z `deviceId` jako urządzenie uwierzytelnianie. Usługa tokenu wykorzystuje zasadę dostępu udostępnione do utworzenia tokenu.
4. Urządzenie używa tokenu bezpośrednio z poziomu Centrum IoT.

> [AZURE.NOTE] Można użyć klasy .NET [SharedAccessSignatureBuilder] [ lnk-dotnet-sas] lub klasy języka Java [IotHubServiceSasToken] [ lnk-java-sas] do tworzenia tokenów w usłudze tokenu.

Usługa tokenu można ustawić wygaśnięcia tokenu stosownie do potrzeb. Po wygaśnięciu tokenu Centrum IoT serwerów połączenia urządzenia. Następnie od usługi tokenu nowy token musisz poprosić urządzenia. Jeśli używasz czasu wygaśnięcia krótki zwiększa obciążenie urządzeniu i usługi tokenu.

Dla urządzenia, aby połączyć się z Centrum, możesz nadal ją dodać do rejestru tożsamości urządzenia Centrum IoT — nawet jeśli urządzenie jest tokenu i nie klucza urządzenia nawiązywanie połączenia za pomocą. W związku z tym, możesz nadal włączając lub wyłączając urządzenia tożsamości w [Centrum IoT tożsamości rejestru] za pomocą kontroli dostępu na urządzenie[ lnk-identity-registry] po uwierzytelnia urządzenie przy użyciu tokenu. Zmniejsza to zagrożenie ryzyka związanego z tokeny z czasem wygaśnięcia długa.

### <a name="comparison-with-a-custom-gateway"></a>Porównanie z bramą niestandardowe

Wzorzec usługi tokenu jest zalecany sposób zaimplementować schemat rejestru/uwierzytelniania tożsamości niestandardowej z Centrum IoT. Zalecane jest, ponieważ Centrum IoT nadal obsługuje większość ruchu rozwiązanie. Jednak istnieją przypadki, w której schemat uwierzytelniania niestandardowego tak jest sprzężony z protokołem, że Usługa przetwarzania cały ruch (*niestandardowe bramy*) jest wymagane. Na przykład jest [warstwy zabezpieczeń TLS (Transport) i klucze wstępne (PSKs)][lnk-tls-psk]. Aby uzyskać więcej informacji, zobacz [bramy protokołu] [ lnk-protocols] tematu.

## <a name="reference-topics"></a>Odwołanie tematy:

W poniższych tematach odwołanie dostarczać więcej informacji na temat kontrolowanie dostępu do Twojej Centrum IoT.

## <a name="iot-hub-permissions"></a>Centrum IoT uprawnień

W poniższej tabeli przedstawiono uprawnienia, które umożliwiają kontrolowanie dostępu do Twojej Centrum IoT.

| Uprawnień            | Notatki |
| --------------------- | ----- |
| **RegistryRead**      | W tym artykule dotacji dostępu do urządzenia rejestru tożsamości. Aby uzyskać więcej informacji, zobacz [urządzenia tożsamości rejestr][lnk-identity-registry]. |
| **RegistryReadWrite** | Dotacje odczytu i zapisu w rejestrze tożsamości urządzenia. Aby uzyskać więcej informacji, zobacz [urządzenia tożsamości rejestr][lnk-identity-registry]. |
| **ServiceConnect**    | Dotacje dostęp do usług skierowaną komunikacji i monitorowanie punkty końcowe w chmurze. Na przykład udziela uprawnienia do usług w chmurze wewnętrznej komunikaty urządzenia w chmurze, wysyłanie wiadomości w chmurze do urządzenia i pobrania odpowiednich potwierdzenia dostarczenia. |
| **DeviceConnect**     | Nadaje prawa dostępu do punktów końcowych komunikacji skierowaną urządzenia. Na przykład udziela uprawnienia do wysyłania wiadomości urządzenia w chmurze i odbierać wiadomości w chmurze do urządzenia. To uprawnienie jest używana przez urządzenia. |

## <a name="additional-reference-material"></a>Materiały dodatkowe informacje

Inne tematy odwołanie w podręczniku Deweloper obejmują:

- [Punkty końcowe Centrum IoT] [ lnk-endpoints] w tym artykule opisano różne punkty końcowe, które każdego centrum IoT udostępnia obsługę operacji obsługi i zarządzania.
- [Ograniczanie i przydziałami] [ lnk-quotas] w tym artykule opisano przydziałów, które mają zastosowanie do usługi Centrum IoT i ograniczania zachowanie się spodziewać podczas korzystania z usługi.
- [Centrum IoT urządzeń i usług SDK] [ lnk-sdks] listy język różne SDK możesz Użyj podczas opracowywania zarówno urządzenia i usługi aplikacji współpracujących z Centrum IoT.
- [Kwerendy języka dla zadań, metody i twins] [ lnk-query] w tym artykule opisano język kwerend, którego można używać do pobierania informacji z Centrum IoT o twins urządzenia, metody i zadania.
- [Obsługa IoT koncentratora MQTT] [ lnk-devguide-mqtt] znajdziesz więcej informacji na temat IoT Centrum obsługi protokołu MQTT.

## <a name="next-steps"></a>Następne kroki

Teraz znasz sposoby kontrolowania dostępu IoT Centrum, może Cię w następujących tematach przewodnik dewelopera:

- [Synchronizowanie stanu i konfiguracji za pomocą urządzenia twins][lnk-devguide-device-twins]
- [Wywoływanie metody bezpośrednio na urządzeniu][lnk-devguide-directmethods]
- [Planowanie zadań na wielu urządzeniach][lnk-devguide-jobs]

Jeśli chcesz wypróbować niektóre pojęcia opisane w tym artykule, może być zainteresować następujące samouczki Centrum IoT:

- [Wprowadzenie do Centrum IoT Azure][lnk-getstarted-tutorial]
- [Jak można wysyłać chmury do urządzenia z Centrum IoT][lnk-c2d-tutorial]
- [Sposób przetwarzania Centrum IoT urządzenia w chmurze wiadomości][lnk-d2c-tutorial]

<!-- links and images -->

[img-tokenservice]: ./media/iot-hub-devguide-security/tokenservice.png
[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-openssl]: https://www.openssl.org/
[lnk-selfsigned]: https://technet.microsoft.com/library/hh848633

[lnk-resource-provider-apis]: https://msdn.microsoft.com/library/mt548492.aspx
[lnk-sas-tokens]: iot-hub-devguide-security.md#security-tokens
[lnk-amqp]: https://www.amqp.org/
[lnk-azure-resource-manager]: ../azure-resource-manager/resource-group-overview.md
[lnk-cbs]: https://www.oasis-open.org/committees/download.php/50506/amqp-cbs-v1%200-wd02%202013-08-12.doc
[lnk-event-hubs-publisher-policy]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-99ce67ab
[lnk-management-portal]: https://portal.azure.com
[lnk-sasl-plain]: http://tools.ietf.org/html/rfc4616
[lnk-identity-registry]: iot-hub-devguide-identity-registry.md
[lnk-dotnet-sas]: https://msdn.microsoft.com/library/microsoft.azure.devices.common.security.sharedaccesssignaturebuilder.aspx
[lnk-java-sas]: http://azure.github.io/azure-iot-sdks/java/service/api_reference/com/microsoft/azure/iot/service/auth/IotHubServiceSasToken.html
[lnk-tls-psk]: https://tools.ietf.org/html/rfc4279
[lnk-protocols]: iot-hub-protocol-gateway.md
[lnk-custom-auth]: iot-hub-devguide-security.md#custom-device-authentication
[lnk-x509]: iot-hub-devguide-security.md#supported-x509-certificates
[lnk-devguide-device-twins]: iot-hub-devguide-device-twins.md
[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-service-sdk]: https://github.com/Azure/azure-iot-sdks/tree/master/csharp/service
[lnk-client-sdk]: https://github.com/Azure/azure-iot-sdks/tree/master/csharp/device
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdks/blob/master/tools/DeviceExplorer/doc/how_to_use_device_explorer.md

[lnk-getstarted-tutorial]: iot-hub-csharp-csharp-getstarted.md
[lnk-c2d-tutorial]: iot-hub-csharp-csharp-c2d.md
[lnk-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md
