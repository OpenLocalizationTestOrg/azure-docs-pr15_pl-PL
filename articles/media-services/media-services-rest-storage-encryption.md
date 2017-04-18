<properties 
    pageTitle="Zaszyfrowanie zawartości za pomocą interfejsu API usługi REST AMS szyfrowania miejsca do magazynowania" 
    description="Dowiedz się, jak szyfrowanie zawartości za pomocą interfejsów API pozostałych AMS szyfrowania miejsca do magazynowania." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"
    ms.author="juliako"/>


#<a name="encrypting-your-content-with-storage-encryption-using-ams-rest-api"></a>Zaszyfrowanie zawartości za pomocą interfejsu API usługi REST AMS szyfrowania miejsca do magazynowania

Zdecydowanie zaleca się szyfrowanie treści lokalnie przy użyciu szyfrowania bitowego AES-256, a następnie przekazanie go do magazynu Azure miejsce, w którym będą przechowywane szyfrowane na pozostałych.

W tym artykule zestawienie AMS szyfrowania miejsca do magazynowania i pokazano, jak przekazać zawartość szyfrowane miejsca do magazynowania:

- Tworzenie zawartości klucza.
- Tworzenie środka trwałego. Ustaw AssetCreationOption StorageEncryption podczas tworzenia elementu.

     Zaszyfrowane aktywa muszą być skojarzone z klawiszami zawartości.
- Łącze klucz zawartości do elementu.  
- Ustawianie szyfrowania związane z nimi parametry na podmioty AssetFile.
 
>[AZURE.NOTE]Jeśli chcesz przedstawić zasób zaszyfrowanych miejsca do magazynowania, należy skonfigurować zasady dostarczania zawartości. Przed można przesłać strumieniowo do zawartości, serwera strumieniowych usuwa szyfrowania miejsca do magazynowania i umożliwia strumieniowe przesyłanie zawartości za pomocą zasad określonego odbiorcy. Aby uzyskać więcej informacji zobacz [Konfigurowanie zasad dostarczania zawartości](media-services-rest-configure-asset-delivery-policy.md).


>[AZURE.NOTE] Podczas pracy z interfejsu API usługi REST usługi multimediów należy uwzględnić następujące kwestie:
>
>Podczas uzyskiwania dostępu do obiektów w usługach multimediów, należy ustawić określone nagłówek pola i wartości w wezwaniach na HTTP. Aby uzyskać więcej informacji zobacz [Ustawienia dotyczące rozwoju multimediów usługi REST interfejsu API](media-services-rest-how-to-use.md).

>Po pomyślnym nawiązaniu połączenia z https://media.windows.net, otrzymasz 301 przekierowanie Określanie innego identyfikatora URI usługi multimediów. Musisz wprowadzić kolejnych zaproszeń do nowego identyfikatora URI w sposób opisany w [Nawiązywanie połączenia z usługą za pomocą interfejsu API usługi REST usługi multimediów](media-services-rest-connect-programmatically.md). 

##<a name="storage-encryption-overview"></a>Omówienie szyfrowania miejsca do magazynowania 

Szyfrowanie miejsca do magazynowania AMS dotyczy szyfrowania tryb **Kont AES.** cały plik.  Tryb kont AES. jest szyfrowania blokowego, który można zaszyfrować dowolnej długości danych bez potrzeby dopełnienia. Działa przez zaszyfrowanie bloku licznik przy algorytm AES, a następnie XOR toku wynik AES z danymi do szyfrowania lub odszyfrowywania.  Blokowanie licznik używany jest tworzona przez skopiowanie wartości InitializationVector bajtów 0-7 wartość licznika i bajtów 8-15 wartość licznika są ustawione na zero. 16 bajtów bloku licznik bajtów 8-15 (to znaczy najmniej znaczące bajty) są używane podczas prosty 64-bitowej wersji liczba całkowita bez znaku, który jest zwiększany o jeden dla każdego kolejnych bloku danych przetwarzania i są przechowywane w porządku bajtów sieci. Należy zauważyć, że jeśli ta liczba całkowita osiągnie wartość maksymalna (0xFFFFFFFFFFFFFFFF), zwiększając jego po Resetuje licznik blok zero (bajtów 8-15) bez wpływu na pozostałe 64 bity licznika (to znaczy bajtów 0-7).   W celu zapewnienia bezpieczeństwa szyfrowania tryb kont AES. wartość InitializationVector dla danego identyfikator klucza dla każdego klucza zawartości jest unikatowy dla każdego pliku i plików jest mniejsza niż 2 ^ 64 bloków długości.  To, aby upewnić się, że wartość licznika nigdy nie jest ponownie z klucz. Aby uzyskać więcej informacji o trybie kont zawiera [tej strony typu wiki](https://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#CTR) (Artykuł typu wiki używa terminów "Identyfikator jednorazowy" zamiast "InitializationVector").

Szyfrowanie Wyczyść zawartość lokalnie, przy użyciu szyfrowania bitowego AES-256 za pomocą **Szyfrowania miejsca do magazynowania** , a następnie przekazanie go do magazynu Azure miejsce, w którym jest przechowywany szyfrowane na pozostałych. Zasoby chronione za pomocą szyfrowania miejsca do magazynowania automatycznie bez szyfrowania i umieszczone w systemie zaszyfrowanego pliku przed kodowanie i opcjonalnie ponownie szyfrowane przed przekazaniem jako nowy trwały dane wyjściowe. W przypadku użycia podstawowego szyfrowania miejsca do magazynowania jest, gdy chcesz zabezpieczyć wysokiej jakości plików multimedialnych wprowadzania danych przy użyciu silnego szyfrowania spoczynku na dysku.

Dostarczenia zasób zaszyfrowanych miejsca do magazynowania, należy skonfigurować zasady dostarczenia zasobu, aby usługi multimediów wie, jak ma być dostarczania zawartości. Przed można przesłać strumieniowo do zawartości, serwera strumieniowych usuwa szyfrowania miejsca do magazynowania i umożliwia strumieniowe przesyłanie zawartości za pomocą zasad określonego odbiorcy (na przykład AES, typowych szyfrowania lub bez szyfrowania).

##<a name="create-contentkeys-used-for-encryption"></a>Tworzenie ContentKeys używany do szyfrowania

Zaszyfrowane aktywa muszą być skojarzone z kluczem szyfrowania miejsca do magazynowania. Musisz utworzyć klucz zawartości może być używany na potrzeby szyfrowania przed utworzeniem plików zawartości. W tej sekcji opisano, jak utworzyć klucz zawartości.

Poniżej przedstawiono ogólne kroki do wygenerowania kluczy zawartości, które zostanie skojarzony z elementów, które mają być szyfrowane. 

1. Do szyfrowania miejsca do magazynowania losowo wygenerować klucz AES 32-bajtowa. 

    Są to klucz zawartości do Twojej zawartości, co oznacza, że wszystkie pliki skojarzone z tego zasobu należy używać tego samego klucza zawartości podczas odszyfrowywania. 
2.  Wywoływanie metod [GetProtectionKeyId](https://msdn.microsoft.com/library/azure/jj683097.aspx#getprotectionkeyid) i [GetProtectionKey](https://msdn.microsoft.com/library/azure/jj683097.aspx#getprotectionkey) , aby uzyskać poprawny certyfikat X.509 używany do szyfrowania klucza zawartości.
3.  Szyfrowanie klucza zawartości z kluczem certyfikat X.509. 

    Multimedia usług .NET SDK używa RSA z OAEP podczas szyfrowania.  Można zobaczyć przykład .NET w [funkcji EncryptSymmetricKeyData](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.FileEncryption/EncryptionUtils.cs).
4.  Tworzenie sumy kontrolnej wartości obliczane za pomocą identyfikatora klucza i klucza zawartości. W poniższym przykładzie .NET oblicza sumy kontrolnej, używając GUID część identyfikatora klucza i wyczyść pole wyboru zawartości klucza.
    

        public static string CalculateChecksum(byte[] contentKey, Guid keyId)
        {
            const int ChecksumLength = 8;
            const int KeyIdLength = 16;

            byte[] encryptedKeyId = null;

            // Checksum is computed by AES-ECB encrypting the KID
            // with the content key.
            using (AesCryptoServiceProvider rijndael = new AesCryptoServiceProvider())
            {
                rijndael.Mode = CipherMode.ECB;
                rijndael.Key = contentKey;
                rijndael.Padding = PaddingMode.None;

                ICryptoTransform encryptor = rijndael.CreateEncryptor();
                encryptedKeyId = new byte[KeyIdLength];
                encryptor.TransformBlock(keyId.ToByteArray(), 0, KeyIdLength, encryptedKeyId, 0);
            }

            byte[] retVal = new byte[ChecksumLength];
            Array.Copy(encryptedKeyId, retVal, ChecksumLength);

            return Convert.ToBase64String(retVal);
        }


5. Utwórz klucz zawartości z **EncryptedContentKey** (przekonwertowana na ciąg zakodowany base64), **ProtectionKeyId**, **ProtectionKeyType**, **ContentKeyType**i wartości **sum kontrolnych** , otrzymany w poprzednich krokach.

    
    Do szyfrowania miejsca do magazynowania następujące właściwości powinien zostać uwzględniony w treści wezwania.
     
    Żądanie właściwości treści   | Opis
    ---|---
    Identyfikator | Identyfikator ContentKey, który pracujemy nad wygenerować w następującym formacie "nb:kid:UUID:<NEW GUID>".
    ContentKeyType | Typ zawartości klucza jest jako liczbę całkowitą dla tego klucza zawartości. Firma Microsoft przekazać wartość 1 do szyfrowania miejsca do magazynowania.
    EncryptedContentKey | Możemy utworzyć nową wartość klucza zawartości jest to wartość 256-bitowy (32 bajt). Klucz jest zaszyfrowany przy użyciu certyfikat X.509 szyfrowania miejsca do magazynowania, który pobieranie z usługi multimediów Azure firmy Microsoft przez wykonywanie żądanie HTTP GET dla GetProtectionKeyId i GetProtectionKey metody. Na przykład, zobacz następujący kod .NET: metoda **EncryptSymmetricKeyData** zdefiniowana [w tym miejscu](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.FileEncryption/EncryptionUtils.cs).
    ProtectionKeyId | To jest identyfikator klucza ochrony certyfikat X.509 szyfrowania miejsca do magazynowania, którego użyto do zaszyfrowania naszych zawartości klucza.
    ProtectionKeyType | Jest to typ szyfrowania klucza ochrony, którego użyto do zaszyfrowania klucza zawartości. Ta wartość jest StorageEncryption(1) w naszym przykładzie.
    Suma kontrolna |MD5 obliczoną sumę kontrolną dla zawartości klucza. Jest ona obliczana przez zaszyfrowanie zawartości identyfikator kluczem zawartości. Kod przykład przedstawia sposób obliczania sumy kontrolnej.
    

###<a name="retrieve-the-protectionkeyid"></a>Pobieranie ProtectionKeyId 
 

W poniższym przykładzie pokazano, jak pobrać ProtectionKeyId, odcisku palca certyfikatu, certyfikatu, które należy użyć w przypadku szyfrowania klucza zawartości. Wykonać ten krok, aby upewnić się, że masz już odpowiedni certyfikat na komputerze.


Żądanie:
    
    
    GET https://media.windows.net/api/GetProtectionKeyId?contentKeyType=0 HTTP/1.1
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423034908&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=7eSLe1GHnxgilr3F2FPCGxdL2%2bwy%2f39XhMPGY9IizfU%3d
    x-ms-version: 2.11
    Host: media.windows.net
    

Odpowiedź:
    
    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 139
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    request-id: 2b6aa7a4-3a09-4b08-b581-26b55667f817
    x-ms-request-id: 2b6aa7a4-3a09-4b08-b581-26b55667f817
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Wed, 04 Feb 2015 02:42:52 GMT
    
    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Edm.String","value":"7D9BB04D9D0A4A24800CADBFEF232689E048F69C"}

###<a name="retrieve-the-protectionkey-for-the-protectionkeyid"></a>Pobieranie ProtectionKey dla ProtectionKeyId

W poniższym przykładzie pokazano, jak pobrać certyfikat X.509 przy użyciu ProtectionKeyId otrzymany w poprzednim kroku.

Żądanie:
        
    GET https://media.windows.net/api/GetProtectionKey?ProtectionKeyId='7D9BB04D9D0A4A24800CADBFEF232689E048F69C' HTTP/1.1
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-e769-2233-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423141026&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=lDBz5YXKiWe5L7eXOHsLHc9kKEUcUiFJvrNFFSksgkM%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 78d1247a-58d7-40e5-96cc-70ff0dfa7382
    Host: media.windows.net
    


Odpowiedź:
    
    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 1227
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 78d1247a-58d7-40e5-96cc-70ff0dfa7382
    request-id: 1523e8f3-8ed2-40fe-8a9a-5d81eb572cc8
    x-ms-request-id: 1523e8f3-8ed2-40fe-8a9a-5d81eb572cc8
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Thu, 05 Feb 2015 07:52:30 GMT
    
    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Edm.String",
    "value":"MIIDSTCCAjGgAwIBAgIQqf92wku/HLJGCbMAU8GEnDANBgkqhkiG9w0BAQQFADAuMSwwKgYDVQQDEyN3YW1zYmx1cmVnMDAxZW5jcnlwdGFsbHNlY3JldHMtY2VydDAeFw0xMjA1MjkwNzAwMDBaFw0zMjA1MjkwNzAwMDBaMC4xLDAqBgNVBAMTI3dhbXNibHVyZWcwMDFlbmNyeXB0YWxsc2VjcmV0cy1jZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAzR0SEbXefvUjb9wCUfkEiKtGQ5Gc328qFPrhMjSo+YHe0AVviZ9YaxPPb0m1AaaRV4dqWpST2+JtDhLOmGpWmmA60tbATJDdmRzKi2eYAyhhE76MgJgL3myCQLP42jDusWXWSMabui3/tMDQs+zfi1sJ4Ch/lm5EvksYsu6o8sCv29VRwxfDLJPBy2NlbV4GbWz5Qxp2tAmHoROnfaRhwp6WIbquk69tEtu2U50CpPN2goLAqx2PpXAqA+prxCZYGTHqfmFJEKtZHhizVBTFPGS3ncfnQC9QIEwFbPw6E5PO5yNaB68radWsp5uvDg33G1i8IT39GstMW6zaaG7cNQIDAQABo2MwYTBfBgNVHQEEWDBWgBCOGT2hPhsvQioZimw8M+jOoTAwLjEsMCoGA1UEAxMjd2Ftc2JsdXJlZzAwMWVuY3J5cHRhbGxzZWNyZXRzLWNlcnSCEKn/dsJLvxyyRgmzAFPBhJwwDQYJKoZIhvcNAQEEBQADggEBABcrQPma2ekNS3Wc5wGXL/aHyQaQRwFGymnUJ+VR8jVUZaC/U/f6lR98eTlwycjVwRL7D15BfClGEHw66QdHejaViJCjbEIJJ3p2c9fzBKhjLhzB3VVNiLIaH6RSI1bMPd2eddSCqhDIn3VBN605GcYXMzhYp+YA6g9+YMNeS1b+LxX3fqixMQIxSHOLFZ1G/H2xfNawv0VikH3djNui3EKT1w/8aRkUv/AAV0b3rYkP/jA1I0CPn0XFk7STYoiJ3gJoKq9EMXhit+Iwfz0sMkfhWG12/XO+TAWqsK1ZxEjuC9OzrY7pFnNxs4Mu4S8iinehduSpY+9mDd3dHynNwT4="}

### <a name="create-the-content-key"></a>Tworzenie zawartości klucza 

Po pobrać certyfikat X.509 i umożliwia szyfrowanie klucza zawartości swój klucz publiczny, Utwórz podmiot **ContentKey** i odpowiednio ustawić wartości jego właściwości.

Jedną z wartości, że należy ustawić podczas tworzenia zawartości klucza jest typem. W przypadku szyfrowania miejsca do magazynowania wartość jest "1". 

W poniższym przykładzie pokazano, jak utworzyć **ContentKey** przy użyciu zestawu **ContentKeyType** szyfrowania miejsca do magazynowania ("1") i **ProtectionKeyType** równa "0", aby wskazać, że klucz ochrony identyfikator jest odcisku palca certyfikat X.509.  


Żądanie

    POST https://media.windows.net/api/ContentKeys HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423034908&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=7eSLe1GHnxgilr3F2FPCGxdL2%2bwy%2f39XhMPGY9IizfU%3d
    x-ms-version: 2.11
    Host: media.windows.net
    {
    "Name":"ContentKey",
    "ProtectionKeyId":"7D9BB04D9D0A4A24800CADBFEF232689E048F69C", 
    "ContentKeyType":"1", 
    "ProtectionKeyType":"0",
    "EncryptedContentKey":"your encrypted content key",
    "Checksum":"calculated checksum"
    }


Odpowiedź:
    
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 777
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://media.windows.net/api/ContentKeys('nb%3Akid%3AUUID%3A9c8ea9c6-52bd-4232-8a43-8e43d8564a99')
    Server: Microsoft-IIS/8.5
    request-id: 76e85e0f-5cf1-44cb-b689-b3455888682c
    x-ms-request-id: 76e85e0f-5cf1-44cb-b689-b3455888682c
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Wed, 04 Feb 2015 02:37:46 GMT
    
    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#ContentKeys/@Element",
    "Id":"nb:kid:UUID:9c8ea9c6-52bd-4232-8a43-8e43d8564a99","Created":"2015-02-04T02:37:46.9684379Z",
    "LastModified":"2015-02-04T02:37:46.9684379Z",
    "ContentKeyType":1,
    "EncryptedContentKey":"your encrypted content key",
    "Name":"ContentKey",
    "ProtectionKeyId":"7D9BB04D9D0A4A24800CADBFEF232689E048F69C",
    "ProtectionKeyType":0,
    "Checksum":"calculated checksum"}

## <a name="create-an-asset"></a>Tworzenie środka trwałego

W poniższym przykładzie pokazano, jak utworzyć środka trwałego.

**Żądanie HTTP**

    POST https://media.windows.net/api/Assets HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-2233-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: media.windows.net
    
    {"Name":"BigBuckBunny" "Options":1}


**Odpowiedź HTTP**

Jeśli kończy się pomyślnie, jest zwracany następujące czynności:
    
    HTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 452
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Assets('nb%3Acid%3AUUID%3A9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1')
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: c59de965-bc89-4295-9a57-75d897e5221e
    request-id: e98be122-ae09-473a-8072-0ccd234a0657
    x-ms-request-id: e98be122-ae09-473a-8072-0ccd234a0657
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 18 Jan 2015 22:06:40 GMT
    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Assets/@Element",
       "Id":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "State":0,
       "Created":"2015-01-18T22:06:40.6010903Z",
       "LastModified":"2015-01-18T22:06:40.6010903Z",
       "AlternateId":null,
       "Name":"BigBuckBunny.mp4",
       "Options":1,
       "Uri":"https://storagetestaccount001.blob.core.windows.net/asset-9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StorageAccountName":"storagetestaccount001"
    }
    
##<a name="associate-the-contentkey-with-an-asset"></a>Kojarzenie ContentKey z środka trwałego

Po utworzeniu ContentKey, skojarzyć go do zawartości za pomocą operacji $links, jak pokazano w poniższym przykładzie:
    
Żądanie:
    
    POST https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3Afbd7ce05-1087-401b-aaae-29f16383c801')/$links/ContentKeys HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Content-Type: application/json
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423141026&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=lDBz5YXKiWe5L7eXOHsLHc9kKEUcUiFJvrNFFSksgkM%3d
    x-ms-version: 2.11
    Host: media.windows.net

    
    {"uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/ContentKeys('nb%3Akid%3AUUID%3A01e6ea36-2285-4562-91f1-82c45736047c')"}

Odpowiedź:

    HTTP/1.1 204 No Content 

##<a name="create-an-assetfile"></a>Tworzenie AssetFile

Jednostka [AssetFile](http://msdn.microsoft.com/library/azure/hh974275.aspx) reprezentuje pliku audio lub wideo, który jest przechowywany w kontenerze obiektów blob. Plik zawartości są zawsze skojarzone z środka trwałego i środka trwałego może zawierać jednego lub wielu plików zawartości. Zadanie Media Encoder usług nie powiedzie się, czy obiekt plik zawartości nie jest skojarzone z plikiem cyfrowego w kontenerze obiektów blob.

Zauważ, że wystąpienie **AssetFile** oraz plik multimedialny rzeczywiste są dwa różne obiekty. Wystąpienia AssetFile zawiera metadanych dotyczących plików multimedialnych, gdy plik multimedialny z zawartością rzeczywiste.

Po wysłaniu do plików multimedialnych do kontenera obiektów blob użyje żądania HTTP **SCALANIE** zaktualizować AssetFile z informacjami o pliku multimediów (nie wyświetlane w tym temacie). 

**Żądanie HTTP**

    POST https://media.windows.net/api/Files HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-4ca2-2233-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: media.windows.net
    Content-Length: 164
    
    {  
       "IsEncrypted":"true",
       "EncryptionScheme" : "StorageEncryption", 
       "EncryptionVersion" : "1.0",       
       "EncryptionKeyId" : "nb:kid:UUID:32e6efaf-5fba-4538-b115-9d1cefe43510",
       "InitializationVector" : "397304628502661816</d:InitializationVector",
       "Options":0,
       "IsPrimary":"false",
       "MimeType":"video/mp4",
       "Name":"BigBuckBunny.mp4",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1"
    }

**Odpowiedź HTTP**

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 535
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Files('nb%3Acid%3AUUID%3Af13a0137-0a62-9d4c-b3b9-ca944b5142c5')
    Server: Microsoft-IIS/8.5
    request-id: 98a30e2d-f379-4495-988e-0b79edc9b80e
    x-ms-request-id: 98a30e2d-f379-4495-988e-0b79edc9b80e
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 00:34:07 GMT
    
    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Files/@Element",
       "Id":"nb:cid:UUID:f13a0137-0a62-9d4c-b3b9-ca944b5142c5",
       "Name":"BigBuckBunny.mp4",
       "ContentFileSize":"0",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "EncryptionVersion": "1.0",
       "EncryptionScheme": "StorageEncryption",
       "IsEncrypted":true,
       "EncryptionKeyId":"nb:kid:UUID:32e6efaf-5fba-4538-b115-9d1cefe43510",
       "InitializationVector":"397304628502661816</d:InitializationVector",
       "IsPrimary":false,
       "LastModified":"2015-01-19T00:34:08.1934137Z",
       "Created":"2015-01-19T00:34:08.1934137Z",
       "MimeType":"video/mp4",
       "ContentChecksum":null
    }
