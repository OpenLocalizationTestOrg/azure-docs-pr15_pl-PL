<properties
   pageTitle="Omówienie konfiguracji Azure usługi tkaninie zaufanego aktorów KVSActorStateProvider | Microsoft Azure"
   description="Informacje na temat konfigurowania tkaninie usługi Azure aktorów stanowe typu KVSActorStateProvider."
   services="Service-Fabric"
   documentationCenter=".net"
   authors="sumukhs"
   manager="timlt"
   editor=""/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/20/2016"
   ms.author="sumukhs"/>

# <a name="configuring-reliable-actors--kvsactorstateprovider"></a>Konfigurowanie zaufanego aktorów — KVSActorStateProvider
Domyślna konfiguracja KVSActorStateProvider można zmodyfikować, zmieniając pliku settings.xml, który jest generowany w katalogu głównym pakietu Microsoft Visual Studio w folderze konfiguracji określonej aktora.

Środowisko uruchomieniowe tkaninie usługi Azure wyszukuje nazwy sekcji wstępnie zdefiniowanych w pliku settings.xml i postępuje wartości konfiguracji podczas tworzenia składniki runtime.

>[AZURE.NOTE] Czy **nie** usuwania i modyfikowania nazwy sekcji następujące ustawienia pliku settings.xml, który jest generowany w rozwiązania programu Visual Studio.

## <a name="replicator-security-configuration"></a>Konfiguracja zabezpieczeń Replikator
Replikator konfiguracji zabezpieczeń są używane do bezpiecznego kanału komunikacji, który jest używany podczas replikacji. Oznacza to, że usług nie widać innych osób ruch replikacji, zapewnia, że dane, które są bardzo udostępniane również jest bezpieczny.
Domyślnie sekcji konfiguracji zabezpieczeń pustego zapobiega bezpieczeństwo replikacji.

### <a name="section-name"></a>Nazwy sekcji
&lt;ActorName&gt;ServiceReplicatorSecurityConfig

## <a name="replicator-configuration"></a>Konfiguracja Replikator
Konfiguracje Replikator skonfigurować Replikator, który odpowiada za wprowadzanie wysoce niezawodne stan Aktor stan dostawcy.
Domyślna konfiguracja jest generowana przez szablon programu Visual Studio i wystarczy przeprowadzić. Ta sekcja zawiera informacje o dodatkowych ustawień, które można dostosować Replikator.

### <a name="section-name"></a>Nazwy sekcji
&lt;ActorName&gt;ServiceReplicatorConfig

### <a name="configuration-names"></a>Nazwy konfiguracji

|Nazwa|Jednostki|Wartość domyślna|Uwagi|
|----|----|-------------|-------|
|BatchAcknowledgementInterval|Sekundy|0.015|Okres, dla którego Replikator na pomocniczym czeka po otrzymaniu operacji przed wysłaniem z powrotem do podstawowej potwierdzenie. Inne potwierdzenia do wysłania do operacji przetwarzania w tym przedziale czasu są wysyłane jako jedną odpowiedź.|
|ReplicatorEndpoint|N/D!|Brak domyślnej — wymaganego parametru|Ustawianie adresu IP i portu, używające Replikator głównego i pomocniczego można komunikować się z innymi replikatorów w replice. Powinna odwoływać się punktu końcowego zasobów TCP w manifestu usługi. Zapoznaj się z [zasobami manifestu usługi](service-fabric-service-manifest-resources.md) dowiedzieć się więcej o definiowaniu zasobów punkt końcowy w manifestu usługi. |
|RetryInterval|Sekundy|5|Okres, po upływie którego Replikator ponownie wysyła wiadomość nie otrzymania potwierdzenia operacji.|
|MaxReplicationMessageSize|Bajtów|50 MB|Maksymalny rozmiar danych replikacji, które mogą być przenoszone w pojedynczej wiadomości.|
|MaxPrimaryReplicationQueueSize|Liczba operacji|1024|Maksymalna liczba operacji w kolejce podstawowego. Operacja jest nasz po podstawowego Replikator otrzyma potwierdzenie od wszystkich pomocniczej replikatorów. Ta wartość musi być większa niż 64 i potęgi liczby 2.|
|MaxSecondaryReplicationQueueSize|Liczba operacji|2048|Maksymalna liczba operacji w kolejce pomocniczą. Po wprowadzeniu wysoce dostępne za pośrednictwem utrzymywanie stanu jest nasz operacji. Ta wartość musi być większa niż 64 i potęgi liczby 2.|

## <a name="store-configuration"></a>Konfiguracja magazynu
Magazyn konfiguracji służą do konfigurowania magazynu lokalnego, używanym do innej witryny Województwo, z którego są replikowane.
Domyślna konfiguracja jest generowana przez szablon programu Visual Studio i wystarczy przeprowadzić. Ta sekcja zawiera informacje o dodatkowych ustawień, które można dostosować magazynu lokalnego.

### <a name="section-name"></a>Nazwy sekcji
&lt;ActorName&gt;ServiceLocalStoreConfig

### <a name="configuration-names"></a>Nazwy konfiguracji

|Nazwa|Jednostki|Wartość domyślna|Uwagi|
|----|----|-------------|-------|
|MaxAsyncCommitDelayInMilliseconds|Milisekundy|200|Określa maksymalną tworzeniu partii interwał zatwierdzenia trwałego magazynu lokalnego.|
|MaxVerPages|Liczba stron|16384|Maksymalna liczba stron wersji lokalnego przechowywania bazy danych. Określa maksymalną liczbę zaległe transakcje.|

## <a name="sample-configuration-file"></a>Przykładowy plik konfiguracyjny

```xml
<?xml version="1.0" encoding="utf-8"?>
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Section Name="MyActorServiceReplicatorConfig">
      <Parameter Name="ReplicatorEndpoint" Value="MyActorServiceReplicatorEndpoint" />
      <Parameter Name="BatchAcknowledgementInterval" Value="0.05"/>
   </Section>
   <Section Name="MyActorServiceLocalStoreConfig">
      <Parameter Name="MaxVerPages" Value="8192" />
   </Section>
   <Section Name="MyActorServiceReplicatorSecurityConfig">
      <Parameter Name="CredentialType" Value="X509" />
      <Parameter Name="FindType" Value="FindByThumbprint" />
      <Parameter Name="FindValue" Value="9d c9 06 b1 69 dc 4f af fd 16 97 ac 78 1e 80 67 90 74 9d 2f" />
      <Parameter Name="StoreLocation" Value="LocalMachine" />
      <Parameter Name="StoreName" Value="My" />
      <Parameter Name="ProtectionLevel" Value="EncryptAndSign" />
      <Parameter Name="AllowedCommonNames" Value="My-Test-SAN1-Alice,My-Test-SAN1-Bob" />
   </Section>
</Settings>
```
## <a name="remarks"></a>Uwagi

Parametr BatchAcknowledgementInterval określa opóźnienia replikacji. Wartość "0" powoduje najniższe opóźnienie możliwe, związany z przepustowości (jak więcej komunikatów potwierdzenia musi być wysyłana i przetworzona, zawierające mniej potwierdzenia).
Większe wartości dla BatchAcknowledgementInterval, wyższa ogólnego replikacji przepustowość, związany z wyższymi opóźnienie operacji. Przekłada się bezpośrednio do oczekiwania zatwierdzenia transakcji.
