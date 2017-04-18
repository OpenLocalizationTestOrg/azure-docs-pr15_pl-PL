<properties
    pageTitle="Omówienie podpisów dostępu do udostępnionych | Microsoft Azure"
    description="Co to są udostępniane podpisy dostęp, ich działania i jak z nich korzystać z węzeł, PHP i C#."
    services="service-bus"
    documentationCenter="na"
    authors="djrosanova"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/02/2016"
    ms.author="darosa;sethm"/>

# <a name="shared-access-signatures"></a>Podpisy udostępniania

*Udostępnione podpisów w programie Access* (SA) to mechanizm podstawowy zabezpieczeń Bus usługi, tym biegami zdarzenia, brokered wiadomości (kolejek i tematy) i przekazywane wiadomości. W tym artykule omówiono udostępniony podpisy programu Access, ich działania i sposobach ich używania w sposób niezależne od platformy.

## <a name="overview-of-sas"></a>Omówienie skojarzenia zabezpieczeń

Udostępnione sygnatury programu Access są mechanizmu uwierzytelniania na podstawie mieszania bezpiecznego 256 Agent kondycji systemu lub identyfikatory URI. Jest bardzo wydajne mechanizm, który jest używany przez wszystkie usługi Bus usługi. W rzeczywistości skojarzeń zabezpieczeń ma dwa składniki: *Udostępnione zasady dostępu* i *Udostępniane podpis programu Access* (często nazywane *token*).

[Udostępnione podpisu dostępu uwierzytelnianie za pomocą usługi Bus](service-bus-shared-access-signature-authentication.md)można znaleźć więcej szczegółowych informacji o udostępnionych podpisów programu Access przy użyciu usługi Bus.

## <a name="shared-access-policy"></a>Zasady dostępu udostępnione

Ważne jest, aby poznać zasady dotyczące skojarzeń zabezpieczeń to wszystko zaczyna się od zasady. Dla każdej zasady zdecydujesz się na trzy informacje: **Nazwa**, **zakres**i **uprawnienia**. **Nazwa** jest po prostu to; Unikatowa nazwa w ramach tego zakresu. Zakres jest dość proste: jest identyfikator URI wybranego zasobu. Nazw usługi Bus zakres jest w pełni kwalifikowaną nazwę domeny (FQDN), takich jak `https://<yournamespace>.servicebus.windows.net/`.

Uprawnienia dostępne dla zasady są głównie oczywiste:

  + Wyślij
  + Odsłuchać
  + Zarządzanie

Po utworzeniu zasady przypisano *Klucz podstawowy* i *Pomocniczy klucza*. Są to kryptograficznie silne klawiszy. Nie utracone lub ich pamięci — zawsze będzie dostępny w [Azure portal][]. Można użyć jednej z wygenerowanym klawiszy i można je odtworzyć w dowolnym momencie. Unieważniane żadnych udostępnionych dostępu podpisów utworzonych na jego podstawie jest będzie, jeśli odtworzyć lub zmienianie klucza podstawowego w zasadach.

Podczas tworzenia nazw Bus usługi zasady są tworzone automatycznie dla cały obszar nazw o nazwie **RootManageSharedAccessKey**i tych zasad obejmuje wszystkie uprawnienia. Nie możesz zalogować jako **główny**, aby nie używać tych zasad, chyba że naprawdę powody. Możesz utworzyć dodatkowe zasady, na karcie **Konfigurowanie** nazw w portalu. Ważne jest, aby notatki, że pojedynczego drzewa poziomie Bus usługi (nazw, kolejki, Centrum zdarzeń itp.) może zawierać tylko do 12 zasady dołączone do niego.

## <a name="shared-access-signature-token"></a>Udostępnione podpis programu Access (token)

Zasady sam nie jest token dostępu Bus usługi. Jest obiekt, z którego jest generowany token dostępu — za pomocą klucz podstawowy i pomocniczy. Token jest generowany przez starannie rzemieślniczy ciąg w następującym formacie:

```
SharedAccessSignature sig=<signature-string>&se=<expiry>&skn=<keyName>&sr=<URL-encoded-resourceURI>
```

Miejsce, w którym `signature-string` jest mieszania 256 Agent kondycji systemu zakresu tokenu (**zakres** zgodnie z opisem w poprzedniej sekcji) z CRLF dołączana i czas wygaśnięcia (w sekundach od epoch: `00:00:00 UTC` na 1 stycznia 1970).

Mieszanie będzie podobny do następującego kodu pseudo i zwraca 32 bajtów.

```
SHA-256('https://<yournamespace>.servicebus.windows.net/'+'\n'+ 1438205742)
```

Wartości mieszany są w ciągu **SharedAccessSignature** , dzięki czemu adresat może obliczyć skrótu z tymi samymi parametrami, należy upewnić się, że zwraca taki sam wynik. Identyfikator URI określa zakres, a nazwa klucza określa zasady może być używany do obliczenia skrótu. Jest to ważne z punktu widzenia zabezpieczeń. Jeśli odpowiedni podpis nie odpowiada, które oblicza adresata (usługa Bus), jest odmowa dostępu. Na tym etapie można się, że nadawca mieli dostęp do klucza i powinny mieć przyznane prawa określone w zasadzie.

## <a name="generating-a-signature-from-a-policy"></a>Generowanie podpisu z zasad

Jak można faktycznie zrobić to w kodzie? Spójrzmy na kilka następujących.

### <a name="nodejs"></a>NodeJS

```
function createSharedAccessToken(uri, saName, saKey) { 
    if (!uri || !saName || !saKey) { 
            throw "Missing required parameter"; 
        } 
    var encoded = encodeURIComponent(uri); 
    var now = new Date(); 
    var week = 60*60*24*7;
    var ttl = Math.round(now.getTime() / 1000) + week;
    var signature = encoded + '\n' + ttl; 
    var signatureUTF8 = utf8.encode(signature); 
    var hash = crypto.createHmac('sha256', saKey).update(signatureUTF8).digest('base64'); 
    return 'SharedAccessSignature sr=' + encoded + '&sig=' +  
        encodeURIComponent(hash) + '&se=' + ttl + '&skn=' + saName; 
}
``` 

### <a name="java"></a>Java

```
private static String GetSASToken(String resourceUri, String keyName, String key)
  {
      long epoch = System.currentTimeMillis()/1000L;
      int week = 60*60*24*7;
      String expiry = Long.toString(epoch + week);

      String sasToken = null;
      try {
          String stringToSign = URLEncoder.encode(resourceUri, "UTF-8") + "\n" + expiry;
          String signature = getHMAC256(key, stringToSign);
          sasToken = "SharedAccessSignature sr=" + URLEncoder.encode(resourceUri, "UTF-8") +"&sig=" +
                  URLEncoder.encode(signature, "UTF-8") + "&se=" + expiry + "&skn=" + keyName;
      } catch (UnsupportedEncodingException e) {

          e.printStackTrace();
      }

      return sasToken;
  }


public static String getHMAC256(String key, String input) {
    Mac sha256_HMAC = null;
    String hash = null;
    try {
        sha256_HMAC = Mac.getInstance("HmacSHA256");
        SecretKeySpec secret_key = new SecretKeySpec(key.getBytes(), "HmacSHA256");
        sha256_HMAC.init(secret_key);
        Encoder encoder = Base64.getEncoder();

        hash = new String(encoder.encode(sha256_HMAC.doFinal(input.getBytes("UTF-8"))));

    } catch (InvalidKeyException e) {
        e.printStackTrace();
    } catch (NoSuchAlgorithmException e) {
        e.printStackTrace();
   } catch (IllegalStateException e) {
        e.printStackTrace();
    } catch (UnsupportedEncodingException e) {
        e.printStackTrace();
    }

    return hash;
}
```

### <a name="php"></a>PHP

```
function generateSasToken($uri, $sasKeyName, $sasKeyValue) 
{ 
$targetUri = strtolower(rawurlencode(strtolower($uri))); 
$expires = time();  
$expiresInMins = 60; 
$week = 60*60*24*7;
$expires = $expires + $week; 
$toSign = $targetUri . "\n" . $expires; 
$signature = rawurlencode(base64_encode(hash_hmac('sha256',             
 $toSign, $sasKeyValue, TRUE))); 

$token = "SharedAccessSignature sr=" . $targetUri . "&sig=" . $signature . "&se=" . $expires .      "&skn=" . $sasKeyName; 
return $token; 
}
```
 
### <a name="c35"></a>C & #35;

```
private static string createToken(string resourceUri, string keyName, string key)
{
    TimeSpan sinceEpoch = DateTime.UtcNow - new DateTime(1970, 1, 1);
    var week = 60 * 60 * 24 * 7;
    var expiry = Convert.ToString((int)sinceEpoch.TotalSeconds + week);
    string stringToSign = HttpUtility.UrlEncode(resourceUri) + "\n" + expiry;
    HMACSHA256 hmac = new HMACSHA256(Encoding.UTF8.GetBytes(key));
    var signature = Convert.ToBase64String(hmac.ComputeHash(Encoding.UTF8.GetBytes(stringToSign)));
    var sasToken = String.Format(CultureInfo.InvariantCulture, "SharedAccessSignature sr={0}&sig={1}&se={2}&skn={3}", HttpUtility.UrlEncode(resourceUri), HttpUtility.UrlEncode(signature), expiry, keyName);
    return sasToken;
}
```

## <a name="using-the-shared-access-signature-at-http-level"></a>Przy użyciu podpisu dostępu do udostępnionego (na poziomie protokołu HTTP)
 
Po zapoznaniu się tworzenia podpisów dostępu udostępnione dla wszystkie elementy w Bus usługi, możesz przystąpić do wykonywania HTTP POST:

```
POST https://<yournamespace>.servicebus.windows.net/<yourentity>/messages
Content-Type: application/json
Authorization: SharedAccessSignature sr=https%3A%2F%2F<yournamespace>.servicebus.windows.net%2F<yourentity>&sig=<yoursignature from code above>&se=1438205742&skn=KeyName
ContentType: application/atom+xml;type=entry;charset=utf-8
``` 
    
Należy pamiętać, że działa to wszystko. Możesz utworzyć skojarzenia zabezpieczeń dla kolejki, temat, subskrypcji, Centrum zdarzenie lub przekazywania. Jeśli tożsamości dla programu publisher na potrzeby koncentratory zdarzenia, możesz po prostu dołączyć `/publishers/< publisherid>`.

Jeśli nadawcy lub klienta tokenu skojarzeń zabezpieczeń, nie mają oni klucz bezpośrednio, a ich nie można przywrócić skrót do uzyskania. Masz kontrolę nad co mogą uzyskać dostęp, a także jak długo. Ważne jest, aby pamiętać to, że zmiana klucza podstawowego w zasadach żadnych udostępnionych dostępu podpisów utworzonych na jego podstawie będzie można unieważniane.

## <a name="using-the-shared-access-signature-at-amqp-level"></a>Przy użyciu podpisu dostępu do udostępnionego (na poziomie AMQP)

W poprzedniej sekcji pokazano sposób używania token skojarzenia zabezpieczeń z żądaniem HTTP POST wysyłania danych Bus usługi. Wiesz, masz dostęp do usług Bus przy użyciu zaawansowane wiadomości Kolejkowanie Protocol (AMQP), który jest preferowanym protokołem korzystać ze względu na wydajność, w wielu scenariuszach. Użycie tokenu skojarzenia zabezpieczeń z AMQP opisano w dokumencie [AMQP Claim-Based zabezpieczenia wersji 1.0](https://www.oasis-open.org/committees/download.php/50506/amqp-cbs-v1%200-wd02%202013-08-12.doc) , który znajduje się w wersji roboczej pracę od 2013, ale dobrze jest obsługiwana przez Azure dzisiaj.

Przed rozpoczęciem wysyłanie danych do usługi Bus wydawcy musisz wysłać token skojarzeń zabezpieczeń w wiadomości AMQP przejrzyste węzła AMQP o nazwie **$cbs** (można to sprawdzić jako "specjalne" kolejka używane przez usługę pobierać i sprawdź poprawność wszystkich tokenów skojarzenia zabezpieczeń). Wydawcy musisz określić pole **parametr From** wewnątrz wiadomości AMQP; to jest węzeł, w którym usługa odpowiedzi w programie publisher z wynikiem token sprawdzania poprawności (wzorzec prostych żądanie/odpowiedź między programu publisher i usługa). Ten węzeł odpowiedzi zostanie utworzona "na bieżąco," mówić o "dynamiczne tworzenie zdalnego węzła", zgodnie z opisem w specyfikacji AMQP 1.0. Po sprawdzeniu, że token skojarzeń zabezpieczeń jest prawidłowy, wydawcy można przejść do przodu i rozpocząć wysyłanie danych do usługi.

Poniższe kroki pokazują jak wysłać token skojarzenia zabezpieczeń z używania biblioteki [AMQP.Net Lite](https://github.com/Azure/amqpnetlite) AMQP Protocol (protokół). To jest przydatne, jeśli nie można używać oficjalnym SDK Bus usługi (na przykład na WinRT, .net Compact Framework, .net Micro Framework i czarno-białego) opracowywania w C\#. Oczywiście tej biblioteki jest przydatne do zapoznanie się z zabezpieczeniami jak opartych na oświadczeniach działa na poziomie AMQP, jak pokazano, jak to działa na poziomie protokołu HTTP (z żądania HTTP POST i token skojarzeń zabezpieczeń wysyłane w nagłówku "Autoryzacji"). Jeśli nie potrzebujesz takie duże doświadczenie AMQP, możesz użyć oficjalnym SDK Bus usługi program .net Framework aplikacje, które zrobił to za Ciebie.

### <a name="c35"></a>C & #35;

```
/// <summary>
/// Send claim-based security (CBS) token
/// </summary>
/// <param name="shareAccessSignature">Shared access signature (token) to send</param>
private bool PutCbsToken(Connection connection, string sasToken)
{
    bool result = true;
    Session session = new Session(connection);

    string cbsClientAddress = "cbs-client-reply-to";
    var cbsSender = new SenderLink(session, "cbs-sender", "$cbs");
    var cbsReceiver = new ReceiverLink(session, cbsClientAddress, "$cbs");

    // construct the put-token message
    var request = new Message(sasToken);
    request.Properties = new Properties();
    request.Properties.MessageId = Guid.NewGuid().ToString();
    request.Properties.ReplyTo = cbsClientAddress;
    request.ApplicationProperties = new ApplicationProperties();
    request.ApplicationProperties["operation"] = "put-token";
    request.ApplicationProperties["type"] = "servicebus.windows.net:sastoken";
    request.ApplicationProperties["name"] = Fx.Format("amqp://{0}/{1}", sbNamespace, entity);
    cbsSender.Send(request);

    // receive the response
    var response = cbsReceiver.Receive();
    if (response == null || response.Properties == null || response.ApplicationProperties == null)
    {
        result = false;
    }
    else
    {
        int statusCode = (int)response.ApplicationProperties["status-code"];
        if (statusCode != (int)HttpStatusCode.Accepted && statusCode != (int)HttpStatusCode.OK)
        {
            result = false;
        }
    }

    // the sender/receiver may be kept open for refreshing tokens
    cbsSender.Close();
    cbsReceiver.Close();
    session.Close();

    return result;
}
```

`PutCbsToken()` Metody odbiera *połączenie* (AMQP połączenia klasy wystąpienia zgodnie z [biblioteki AMQP .NET Lite](https://github.com/Azure/amqpnetlite)) reprezentuje usługę i parametr *sasToken* , który jest token skojarzeń zabezpieczeń, aby wysłać połączenie TCP. 

> [AZURE.NOTE] Należy pamiętać, że połączenie jest tworzone **mechanizm uwierzytelniania SASL ustawiona na zewnętrzne** (a nie zwykły domyślne przy użyciu nazwy użytkownika i hasło używane podczas nie trzeba wysłać token skojarzenia zabezpieczeń).

Następnie wydawcy tworzy dwa łącza AMQP token skojarzeń zabezpieczeń wysyłania i odbierania odpowiedzi (wynik tokenu sprawdzanie poprawności) z usługi.

Wiadomość AMQP zawiera zestaw właściwości i dodatkowych informacji od zwykłych wiadomości. Token skojarzeń zabezpieczeń jest w treści wiadomości (za pomocą jego konstruktora). Właściwość **"Parametr From"** jest ustawiona na nazwę węzła odbieranie wynik sprawdzania poprawności łącze odbiorcy (możesz zmienić jego nazwę Jeśli chcesz i zostanie utworzona dynamicznie przez usługę). Trzy ostatnie właściwości aplikacji i niestandardowe są używane przez usługę oznaczającą, jakiego rodzaju operacji ma do wykonania. Zgodnie z opisem w specyfikacji wersja robocza obligacji zamiennych, muszą być **Nazwa operacji** ("token położenie"), **Wpisz tokenu** (w tym przypadku "servicebus.windows.net:sastoken") i **"Nazwa" grupy odbiorców** tokenu dotyczy (całej jednostki).

Po wysłaniu token skojarzeń zabezpieczeń łącze nadawcy, wydawcy musi przeczytaj odpowiedzi na łączu odbiorcy. Odpowiedź jest zwykłych wiadomości AMQP z właściwością aplikacji o nazwie **"kodu stanu"** , który może zawierać tych samych wartości jako kod stanu HTTP. 

## <a name="next-steps"></a>Następne kroki

Zobacz [odwołanie interfejsu API usługi REST Bus usługi](https://msdn.microsoft.com/library/azure/hh780717.aspx) Aby uzyskać więcej informacji na temat co można zrobić z tych tokenów skojarzeń zabezpieczeń.

Aby uzyskać więcej informacji o uwierzytelnianiu Bus usługi zobacz [Bus usługi uwierzytelniania i autoryzacji](service-bus-authentication-and-authorization.md). 

Więcej przykładów skojarzeń zabezpieczeń w C# i obsługę języka JavaScript są w [Ten wpis w blogu](http://developers.de/blogs/damir_dobric/archive/2013/10/17/how-to-create-shared-access-signature-for-service-bus.aspx).

[Azure portal]: https://portal.azure.com