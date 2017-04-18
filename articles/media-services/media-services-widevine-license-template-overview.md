<properties 
    pageTitle="Omówienie szablonu licencji Widevine | Microsoft Azure" 
    description="W tym temacie przedstawiono omówienie szablonu licencji Widevine, który umożliwia konfigurowanie Widevine licencji." 
    authors="juliako" 
    manager="erikre" 
    editor="" 
    services="media-services" 
    documentationCenter=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"  
    ms.author="juliako"/>

#<a name="widevine-license-template-overview"></a>Omówienie szablonu licencji Widevine

##<a name="overview"></a>Omówienie

Usługi multimediów Azure umożliwia teraz skonfigurować i poproś Widevine licencji. Gdy użytkownik końcowy próbą odtwarzanie zawartości Widevine chronione, jest wysyłane żądanie usługi dostarczania licencji uzyskanie licencji. Jeśli usługa licencji zatwierdzi żądanie, problemy licencji, które są wysyłane do klienta i może służyć do odszyfrowywanie i odtwarzanie określonej zawartości.

Żądanie licencji Widevine jest sformatowany jako wiadomości JSON.  

Zauważ, że można utworzyć wiadomość pustego bez wartości tylko "{}" i zostanie utworzony szablon licencji z wszystkie ustawienia domyślne.  

    {  
       “payload”:“<license challenge>”,
       “content_id”: “<content id>” 
       “provider”: ”<provider>”
       “allowed_track_types”:“<types>”,
       “content_key_specs”:[  
          {  
             “track_type”:“<track type 1>”
          },
          {  
             “track_type”:“<track type 2>”
          },
          …
       ],
       “policy_overrides”:{  
          “can_play”:<can play>,
          “can persist”:<can persist>,
          “can_renew”:<can renew>,
          “rental_duration_seconds”:<rental duration>,
          “playback_duration_seconds”:<playback duration>,
          “license_duration_seconds”:<license duration>,
          “renewal_recovery_duration_seconds”:<renewal recovery duration>,
          “renewal_server_url”:”<renewal server url>”,
          “renewal_delay_seconds”:<renewal delay>,
          “renewal_retry_interval_seconds”:<renewal retry interval>,
          “renew_with_usage”:<renew with usage>
       }
    }

##<a name="json-message"></a>JSON wiadomości

Nazwa | Wartość | Opis
---|---|---
ładunek |Ciąg zakodowany Base64 |Żądanie licencji wysyłane przez klienta. 
content_id | Ciąg zakodowany Base64|Identyfikator użyty do uzyskania KeyId(s) i klawisze zawartości dla każdego content_key_specs.track_type.
dostawcy |ciąg |Umożliwia wyszukiwanie zawartości klucze i zasady. Wymagane.
nazwa_zasady | ciąg |Nazwa zasady wcześniej zarejestrowanych. Opcjonalne
allowed_track_types | wyliczenia  | SD_ONLY lub SD_HD. Kontrolki, których zawartość klawiszy powinien zostać uwzględniony w licencji
content_key_specs | Tablica JSON struktur, zobacz **Specyfikacja klucza zawartości** poniżej | Bardziej precyzyjne lub kolorów kontrolki w zawartości klawiszy zwraca. Aby uzyskać szczegółowe informacje, zobacz zawartości specyfikacja klucza poniżej.  Można określić tylko jeden z allowed_track_types i content_key_specs. 
use_policy_overrides_exclusively | wartość logiczna. PRAWDA lub FAŁSZ | Użyj atrybutów zasad określony przez policy_overrides i pominąć wszystkie wcześniej zapisane zasady.
policy_overrides | JSON struktury, zobacz **Zasady zastępują** poniżej | Ustawienia zasad dla tej licencji.  W przypadku tego trwałego zawiera wstępnie zdefiniowanych zasad, zostanie użyty tych określonej wartości. 
session_init | JSON struktury, zobacz **Inicjowanie sesji** poniżej | Opcjonalnie dane przekazywane do licencji.
parse_only | wartość logiczna. PRAWDA lub FAŁSZ | Żądanie licencji jest analizowany, ale nie licencji jest wydawany. Jednak wartości formularza żądanie licencji są zwracane w odpowiedzi.  

##<a name="content-key-specs"></a>Najważniejszych zawartości 

Jeśli istnieje istniejącego zasady, jest niepotrzebna, aby określić wartości w zawartości specyfikacja klucza.  Istniejące zasady skojarzone z tej zawartości będą służące do ustalania ochrony wyjściowej, takich jak HDCP i CGMS.  Jeśli zasady istniejącego nie jest zarejestrowany z serwerem licencji Widevine, dostawca zawartości można wprowadzić wartości do żądanie licencji.   


Każdy content_key_specs musi być określony dla wszystkie ścieżki, bez względu na to use_policy_overrides_exclusively opcji. 


Nazwa | Wartość | Opis
---|---|---
content_key_specs. track_type | ciąg | Nazwa typu śledzenia. Jeśli określono content_key_specs w wezwaniu licencji, upewnij się określić, że śledzenie wszystkich typów jawnie. Brak w tym celu spowoduje błąd do odtwarzania ostatnich 10 sekund. 
content_key_specs  <br/> security_level | UInt32 | Określa wymagania niezawodności klienta do odtwarzania. <br/> 1 - wymagane jest whitebox programowy szyfrowania. <br/> 2 - oprogramowania szyfrowania i zaciemnionego dekoder jest wymagane. <br/> 3 materiału klucza i szyfrowania operacji musi być wykonane w środowisku wykonanie kopii zaufanych sprzętu. <br/> 4 szyfrowania i dekodowanie zawartości musi być wykonane w środowisku wykonanie kopii zaufanych sprzętu.  <br/> 5 szyfrowania, dekodowania i wszystkie obsługi multimediów (kompresowane i nieskompresowane) muszą traktowane w środowisku wykonanie kopii zaufanych sprzętu.  
content_key_specs <br/> required_output_protection.hdc | ciąg — jeden z: HDCP_V2 HDCP_NONE, HDCP_V1, | Wskazuje, czy jest wymagane HDCP
content_key_specs <br/>klawisz | Base64 <br/>Ciąg zakodowany|Klucz zawartości do używania dla tego śledzenia. Jeśli określony, wymagany jest track_type lub key_id.  Ta opcja umożliwia dostawca zawartości do dodania klucz zawartości do tej ścieżki zamiast co serwera licencji Widevine Generowanie lub wyszukać klucz.
content_key_specs.key_id| Kodowane Base64 ciąg binarny, 16 bajtów | Unikatowy identyfikator klucza. 


##<a name="policy-overrides"></a>Zastępowanie zasad 

Nazwa | Wartość | Opis
---|---|---
policy_overrides. can_play | wartość logiczna. PRAWDA lub FAŁSZ | Wskazuje tego odtwarzania zawartości jest dozwolone. Wartość domyślna to false.
policy_overrides. can_persist | wartość logiczna. PRAWDA lub FAŁSZ |Wskazuje, że licencja może być zachowywane do pamięć w trybie offline. Wartość domyślna to false.
policy_overrides. can_renew | wartość logiczna PRAWDA lub FAŁSZ |Wskazuje, że jest dozwolona odnawiania tej licencji. Jeśli wartość true, czas trwania licencji można przedłużyć weryfikowania. Wartość domyślna to false. 
policy_overrides. license_duration_seconds | Int64 | Wskazuje przedział czasu dla określonych licencji. Wartość 0 wskazuje, że nie jest ograniczona do czasu trwania. Domyślnie wynosi 0. 
policy_overrides. rental_duration_seconds | Int64 | Wskazuje z przedziału czasu podczas odtwarzania jest dozwolone. Wartość 0 wskazuje, że nie jest ograniczona do czasu trwania. Domyślnie wynosi 0. 
policy_overrides. playback_duration_seconds | Int64 | Wyświetlanie okna czasu po odtwarzanie rozpoczyna się w czasie trwania licencji. Wartość 0 wskazuje, że nie jest ograniczona do czasu trwania. Domyślnie wynosi 0. 
policy_overrides. renewal_server_url |ciąg | Wszystkie żądania weryfikowania (odnowienie) dla tej licencji jest skierowana do określonego adresu URL. To pole jest używane tylko, jeśli jest spełniony can_renew.
policy_overrides. renewal_delay_seconds |Int64 |Ile sekund po license_start_time, przed odnawianiu jest najpierw. To pole jest używane tylko, jeśli jest spełniony can_renew. Domyślny jest równy 0 
policy_overrides. renewal_retry_interval_seconds | Int64 | Określa opóźnienie w sekundach między żądania odnowienia kolejnych licencji, w przypadku awarii. To pole jest używane tylko, jeśli jest spełniony can_renew. 
policy_overrides. renewal_recovery_duration_seconds | Int64 | Czas, w którym odtwarzanie może kontynuować podczas odnawiania jest ono próby, jeszcze zakończy się niepowodzeniem z powodu problemów wewnętrznej bazy danych z serwera licencji. Wartość 0 wskazuje, że nie jest ograniczona do czasu trwania. To pole jest używane tylko, jeśli jest spełniony can_renew.
policy_overrides. renew_with_usage | wartość logiczna PRAWDA lub FAŁSZ |Wskazuje, że licencja są przesyłane odnowienie po uruchomieniu zastosowania. To pole jest używane tylko, jeśli jest spełniony can_renew. 

##<a name="session-initialization"></a>Inicjowanie sesji

Nazwa | Wartość | Opis
---|---|---
provider_session_token | Ciąg zakodowany Base64 |Ten token sesji jest przekazany z powrotem do licencji i wystąpią w kolejnych odnowienia.  Tokenu sesji nie zostaną zachowywane poza sesji. 
provider_client_token | Ciąg zakodowany Base64 | Token klienta, aby wysłać ponownie w odpowiedzi licencji.  Jeśli żądanie licencji zawiera token klienta, ta wartość jest ignorowana. Token klienta będzie umieszczony poza sesje licencji.
override_provider_client_token | wartość logiczna. PRAWDA lub FAŁSZ |Jeśli wartości false i żądanie licencji zawiera token klienta, należy użyć token z żądania, nawet jeśli określono token klienta struktury.  Jeśli wartość true, zawsze używaj określony struktury token.

##<a name="configure-your-widevine-licenses-using-net-types"></a>Konfigurowanie licencji Widevine przy użyciu typów .NET

Usługi multimediów zawiera .NET interfejsów API, które umożliwiają skonfigurowanie licencji Widevine. 

###<a name="classes-as-defined-in-the-media-services-net-sdk"></a>Klas zdefiniowanych w SDK .NET usługi multimediów

Poniżej przedstawiono definicje tych typów.

    public class WidevineMessage
    {
        public WidevineMessage();
    
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public AllowedTrackTypes? allowed_track_types { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public ContentKeySpecs[] content_key_specs { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public object policy_overrides { get; set; }
    }

    [JsonConverter(typeof(StringEnumConverter))]
    public enum AllowedTrackTypes
    {
        SD_ONLY = 0,
        SD_HD = 1
    }
    public class ContentKeySpecs
    {
        public ContentKeySpecs();

        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public string key_id { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public RequiredOutputProtection required_output_protection { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public int? security_level { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public string track_type { get; set; }
    }

    public class RequiredOutputProtection
    {
        public RequiredOutputProtection();

        public Hdcp hdcp { get; set; }
    }

    [JsonConverter(typeof(StringEnumConverter))]
    public enum Hdcp
    {
        HDCP_NONE = 0,
        HDCP_V1 = 1,
        HDCP_V2 = 2
    }

###<a name="example"></a>Przykład

W poniższym przykładzie pokazano sposób użycia interfejsów API .NET skonfigurowanie prostych licencji Widevine.

    private static string ConfigureWidevineLicenseTemplate()
    {
        var template = new WidevineMessage
        {
            allowed_track_types = AllowedTrackTypes.SD_HD,
            content_key_specs = new[]
            {
                new ContentKeySpecs
                {
                    required_output_protection = new RequiredOutputProtection { hdcp = Hdcp.HDCP_NONE},
                    security_level = 1,
                    track_type = "SD"
                }
            },
            policy_overrides = new
            {
                can_play = true,
                can_persist = true,
                can_renew = false
            }
        };

        string configuration = JsonConvert.SerializeObject(template);
        return configuration;
    }


##<a name="media-services-learning-paths"></a>Ścieżek nauki usługi multimediów

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="see-also"></a>Zobacz też

[Przy użyciu PlayReady i/lub dynamiczne Widevine typowych szyfrowania](media-services-protect-with-drm.md)
