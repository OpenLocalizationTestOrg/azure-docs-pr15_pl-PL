<properties
   pageTitle="Kluczowe informacje o wersji magazynu interfejsu API 2.x .NET | Microsoft Azure"
   description=".NET deweloperzy użyje takiego interfejsu API kodu Azure klucza magazynu"
   services="key-vault"
   documentationCenter=""
   authors="BrucePerlerMS"
   manager="mbaldwin"
   editor="bruceper" />
<tags
   ms.service="key-vault"
   ms.devlang="CSharp"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/07/2016"
   ms.author="bruceper" />

# <a name="azure-key-vault-net-20---release-notes-and-migration-guide"></a>Azure magazynu klucza .NET 2.0 — informacje o wersji i przewodnik po migracji

Następujące wskazówki i notatki są dla deweloperów pracujących z .NET magazynu klucza Azure i C# biblioteki. W polu Zmień z wersji 1.0 do wersji 2.0 liczba aktualizacji wprowadzono ten będzie wymagają pracy migracji w kodzie w aby można było korzystać z funkcjonalności ulepszenia i funkcji Dodatki, takie jak obsługa Certyfikaty klucza magazynu.

Obsługa certyfikatów klucza magazynu zapewnia zarządzania programu x509 certyfikaty i następujących problemów:  

-   Umożliwia właściciela certyfikatu utworzyć certyfikat za pośrednictwem proces tworzenia klucza magazynu lub importowanie istniejący certyfikat. Ta opcja uwzględnia zarówno podpisany i urząd certyfikacji generowane certyfikaty.

- Umożliwia tylko właściciel certyfikatu klucza magazynu zaimplementowania bezpiecznej lokalizacji i zarządzanie X509 certyfikaty bez uwzględniania materiał klucza prywatnego.  

-   Umożliwia właściciela certyfikatu do utworzenia zasady kierujący magazynu klucz do zarządzania cyklu życia certyfikatu.  

-   Umożliwia właścicieli certyfikat dostarczać informacje o kontaktach powiadomień o zdarzeniach cyklu życia ważności i odnawiania certyfikatu.  

-   Obsługa automatycznego odnawiania z zaznaczonego emitentów - partnera magazynu klucza X509 certyfikatów dostawców / urzędy certyfikacji.
    - Uwaga — dostawców i źródeł innych niż współpracę również są dozwolone, ale nie obsługuje funkcji automatycznego odnawiania.


## <a name="net-support"></a>Obsługa .NET
- **.NET 4.0** nie jest obsługiwane w wersji 2.0 programu .NET Azure klucza magazynu-C# biblioteki
- **Podstawowe .NET** jest obsługiwana w wersji 2.0 programu .NET Azure klucza magazynu-C# biblioteki

## <a name="namespaces"></a>Przestrzenie nazw
- W obszarze nazw **modeli** została zmieniona z **Microsoft.Azure.KeyVault** na **Microsoft.Azure.KeyVault.Models**.
- Przestrzeń nazw **Microsoft.Azure.KeyVault.Internal** zostanie usunięte.
- Przestrzeń nazw zależności Azure SDK są zmieniane z **Hyak.Common** i **Hyak.Common.Internals** na **Microsoft.Rest** i **Microsoft.Rest.Serialization**


## <a name="type-changes"></a>Zmiana typu
- Zmiana *hasła* *SecretBundle*
- Zmiana *słownika* *IDictionary*
- *Lista<T>, ciąg []* zmiana *IList<T> *
- Zmiana *NextList* *NextPageLink*


## <a name="return-types"></a>Typy zwrotów
- Zwraca **KeyList** i **SecretList** *IPage<T> * zamiast *ListKeysResponseMessage*
- Wygenerowane **BackupKeyAsync** zwróci *BackupKeyResult* , która zawiera *wartość* (blob tworzenia kopii zapasowych). Przed metodę został zawinięty i zwracanie tylko wartości.

## <a name="exceptions"></a>Wyjątki
- *KeyVaultClientException* został zamieniony na *KeyVaultErrorException*
- Błąd usługi został zmieniony z wyjątku *. Błąd* *wyjątek. Body.Error.Message*.
- Usunięte dodatkowe informacje z komunikat o błędzie **[JsonExtensionData]**.

## <a name="constructors"></a>Konstruktory
- Zamiast akceptowania *HttpClient* jako argument konstruktora, konstruktora akceptuje tylko *HttpClientHandler* lub *DelegatingHandler []*.



## <a name="downloaded-packages"></a>Pobrany pakietów  
Gdy klient przetwarzania zależności magazynu klucza następujące pobranych
#### <a name="previous-package-list"></a>Lista poprzedniego pakietu
- Wersja pakietu id="Hyak.Common" = "1.0.2" targetFramework = "net45"
- Wersja pakietu id="Microsoft.Azure.Common" = "2.0.4" targetFramework = "net45"
- Wersja pakietu id="Microsoft.Azure.Common.Dependencies" = "1.0.0" targetFramework = "net45"
- Wersja pakietu id="Microsoft.Azure.KeyVault" = "1.0.0" targetFramework = "net45"
- Wersja pakietu id="Microsoft.Bcl" = "1.1.9" targetFramework = "net45"
- Wersja pakietu id="Microsoft.Bcl.Async" = "1.0.168" targetFramework = "net45"
- Wersja pakietu id="Microsoft.Bcl.Build" = "1.0.14" targetFramework = "net45"
- Wersja pakietu id="Microsoft.Net.Http" = "2.2.22" targetFramework = "net45"

#### <a name="current-package-list"></a>Bieżąca lista pakietu
- Wersja pakietu id="Microsoft.Azure.KeyVault" = "2.0.0-preview" targetFramework = "net45"
- Wersja pakietu id="Microsoft.Rest.ClientRuntime" = "2.2.0" targetFramework = "net45"
- Wersja pakietu id="Microsoft.Rest.ClientRuntime.Azure" = "3.2.0" targetFramework = "net45"


## <a name="class-changes"></a>Zmienia się

- Usunięto klasy **UnixEpoch**
- Klasy **Base64UrlConverter** jest zmieniana na **Base64UrlJsonConverter**

## <a name="other-changes"></a>Inne zmiany

- Obsługa konfiguracji zasad ponów próbę operacji KV przejściowych błędy dodano do tej wersji interfejsu API.



## <a name="microsoftazuremanagementkeyvault-nuget"></a>Microsoft.Azure.Management.KeyVault NuGet
- Zwracany typ operacji, które zwracane *magazynu*, była zajęć, która zawierała właściwości magazynu. Zwracany typ jest teraz *magazynu*.
- *PermissionsToKeys* i *PermissionsToSecrets* są teraz *Permissions.Keys* i *Permissions.Secrets*
- Kontrolka kontrolnego także niektóre zmiany zwracanych typów dotyczą.

## <a name="microsoftazurekeyvaultextensions-nuget"></a>Microsoft.Azure.KeyVault.Extensions NuGet
- Pakiet został podzielony **Microsoft.Azure.KeyVault.Extensions** i **Microsoft.Azure.KeyVault.Cryptography** dla operacji kryptograficzny.
