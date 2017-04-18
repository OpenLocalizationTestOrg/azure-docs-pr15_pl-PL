<properties
   pageTitle="Omówienie konfiguracji Azure usługi tkaninie zaufanego aktorów ReliableDictionaryActorStateProvider | Microsoft Azure"
   description="Informacje o konfigurowaniu tkaninie usługi Azure aktorów stanowe typu ReliableDictionaryActorStateProvider."
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
   ms.date="07/18/2016"
   ms.author="sumukhs"/>

# <a name="configuring-reliable-actors--reliabledictionaryactorstateprovider"></a>Konfigurowanie zaufanego aktorów — ReliableDictionaryActorStateProvider
Domyślna konfiguracja ReliableDictionaryActorStateProvider można zmodyfikować, zmieniając plik settings.xml generowany w katalogu głównym pakietu Visual Studio w folderze konfiguracji określonej aktora.

Środowisko uruchomieniowe tkaninie usługi Azure wyszukuje nazwy sekcji wstępnie zdefiniowanych w pliku settings.xml i postępuje wartości konfiguracji podczas tworzenia składniki runtime.

>[AZURE.NOTE] Czy **nie** usuwania i modyfikowania nazwy sekcji następujące ustawienia pliku settings.xml, który jest generowany w rozwiązania programu Visual Studio.

Dostępne są także ustawienia globalne, które wpływają na konfigurację ReliableDictionaryActorStateProvider.

## <a name="global-configuration"></a>Konfiguracja globalna

Konfiguracja globalna określono w manifeście klaster klaster w sekcji KtlLogger. Umożliwia konfigurację lokalizacji udostępnionej dziennika i rozmiaru oraz ograniczenia globalnej pamięci używane przez rejestratora. Uwaga wszystkich usług, które używają ReliableDictionaryActorStateProvider i niezawodne usługi stanowe wpływa na zmiany w manifeście klaster.

Manifest klaster jest pojedynczy plik XML, który zawiera ustawień i konfiguracji, które dotyczą wszystkich węzłów i usług w klastrze. Plik jest zazwyczaj nazywany ClusterManifest.xml. Zobaczysz klaster manifestu dla klaster za pomocą polecenia programu powershell Get-ServiceFabricClusterManifest.

### <a name="configuration-names"></a>Nazwy konfiguracji

|Nazwa|Jednostki|Wartość domyślna|Uwagi|
|----|----|-------------|-------|
|WriteBufferMemoryPoolMinimumInKB|Kilobajtów|8388608|Minimalna liczba KB przydzielić w trybie jądra dla puli pamięci bufora zapisu rejestratora. Tej puli pamięci jest używany w pamięci podręcznej informacje o stanie przed zapisywanie na dysku.|
|WriteBufferMemoryPoolMaximumInKB|Kilobajtów|Bez limitu|Maksymalny rozmiar, do którego rejestratora pisanie puli pamięci bufora można powiększyć.|
|SharedLogId|IDENTYFIKATOR GUID|""|Określa unikatowy identyfikator GUID służących do identyfikowania domyślne udostępnionego pliku dziennika używane przez wszystkich usług zaufanego na wszystkich węzłach w klastrze, które nie zostanie SharedLogId w konfiguracji określonych usług. Jeśli określono SharedLogId, następnie SharedLogPath musi być także określona.|
|SharedLogPath|W pełni kwalifikowana nazwa|""|Określa pełną ścieżkę miejsce, w którym plik dziennika udostępnionego używane przez wszystkich usług zaufanego na wszystkich węzłach w klastrze, które nie zostanie SharedLogPath w konfiguracji określonych usług. Jednak jeśli określono SharedLogPath, opcja SharedLogId musi być także określona.|
|SharedLogSizeInMB|Megabajtów|8192|Określa liczbę MB wolnego miejsca na przydzielają w dzienniku udostępnionych. Wartość musi być 2048 lub większej.|

### <a name="sample-cluster-manifest-section"></a>Sekcja manifestu klaster próbki
```xml
   <Section Name="KtlLogger">
     <Parameter Name="WriteBufferMemoryPoolMinimumInKB" Value="8192" />
     <Parameter Name="WriteBufferMemoryPoolMaximumInKB" Value="8192" />
     <Parameter Name="SharedLogId" Value="{7668BB54-FE9C-48ed-81AC-FF89E60ED2EF}"/>
     <Parameter Name="SharedLogPath" Value="f:\SharedLog.Log"/>
     <Parameter Name="SharedLogSizeInMB" Value="16383"/>
   </Section>
```

### <a name="remarks"></a>Uwagi
Rejestratora ma globalnej puli pamięć przydzielona z pamięci jądra nie stronicowania, które są dostępne dla wszystkich usług zaufanego w węźle w pamięci podręcznej danych o stanie przed zapisywane w dzienniku dedykowane skojarzone z replice niezawodne usługi. Rozmiar puli zależy od ustawień WriteBufferMemoryPoolMinimumInKB i WriteBufferMemoryPoolMaximumInKB. WriteBufferMemoryPoolMinimumInKB określa zarówno początkowy rozmiar tej puli pamięci, jak i rozmiar najniższe, do której może spowodować zmniejszenie puli pamięci. WriteBufferMemoryPoolMaximumInKB jest najwyższe rozmiar puli pamięci mogą. Każdej replice niezawodne usługi, która została otwarta może zwiększyć rozmiar puli pamięci kwotą systemem określonym maksymalnie WriteBufferMemoryPoolMaximumInKB. Jeśli istnieje więcej żądanie pamięci z puli pamięci nie jest dostępna, żądań pamięci zostaną opóźnione, aż do pamięci. W związku z tym jeśli puli pamięci buforu zapisu jest za mały dla określonego konfiguracji następnie wydajności mogą odczuwać.

Ustawienia SharedLogId i SharedLogPath zawsze są używane razem aby zdefiniować GUID i lokalizację udostępnionego dziennik domyślny dla wszystkich węzłów w klastrze. Dziennik udostępniony domyślne jest używana dla wszystkich zaufanego usług, które nie określić ustawienia w settings.xml dla określonej usługi. Aby uzyskać optymalną wydajność należy umieścić udostępnionych plików dziennika na dyskach, które są używane wyłącznie do pliku dziennika udostępnionego w celu zmniejszenia konfliktu.

SharedLogSizeInMB określa ilość miejsca na dysku do przydzielenia w dzienniku udostępnionego domyślnego we wszystkich węzłach.  SharedLogId i SharedLogPath nie muszą być określone w kolejności SharedLogSizeInMB określonych.

## <a name="replicator-security-configuration"></a>Konfiguracja zabezpieczeń Replikator
Replikator konfiguracji zabezpieczeń są używane do bezpiecznego kanału komunikacji, który jest używany podczas replikacji. Oznacza to, że usług nie widać innych osób ruch replikacji, zyskujemy pewność, że dane, które jest wysoce udostępnione jest również bezpieczne.
Domyślnie sekcji konfiguracji zabezpieczeń pustego zapobiega bezpieczeństwo replikacji.

### <a name="section-name"></a>Nazwy sekcji
&lt;ActorName&gt;ServiceReplicatorSecurityConfig

## <a name="replicator-configuration"></a>Konfiguracja Replikator
Konfiguracje Replikator służą do konfigurowania Replikator, który odpowiada za wprowadzanie stan dostawcy stan Aktor wysoce niezawodne replikacji i utrzymuje stan lokalnie.
Domyślna konfiguracja jest generowana przez szablon programu Visual Studio i wystarczy przeprowadzić. Ta sekcja zawiera informacje o dodatkowych ustawień, które można dostosować Replikator.

### <a name="section-name"></a>Nazwy sekcji
&lt;ActorName&gt;ServiceReplicatorConfig

### <a name="configuration-names"></a>Nazwy konfiguracji

|Nazwa|Jednostki|Wartość domyślna|Uwagi|
|----|----|-------------|-------|
|BatchAcknowledgementInterval|Sekundy|0.015|Okres, dla którego Replikator na pomocniczym czeka po otrzymaniu operacji przed wysłaniem z powrotem do podstawowej potwierdzenie. Inne potwierdzenia do wysłania dla operacji przetwarzane w tym przedziale czasu są wysyłane jako jedną odpowiedź.||
|ReplicatorEndpoint|N/D!|Brak domyślnej — wymaganego parametru|Ustawianie adresu IP i portu, używające Replikator głównego i pomocniczego można komunikować się z innymi replikatorów w replice. Powinna odwoływać się punktu końcowego zasobów TCP w manifestu usługi. Zapoznaj się z [zasobami manifestu usługi](service-fabric-service-manifest-resources.md) dowiedzieć się więcej o definiowaniu zasobów punkt końcowy w manifestu usługi. |
|MaxReplicationMessageSize|Bajtów|50 MB|Maksymalny rozmiar danych replikacji, które mogą być przenoszone w pojedynczej wiadomości.|
|MaxPrimaryReplicationQueueSize|Liczba operacji|8192|Maksymalna liczba operacji w kolejce podstawowego. Operacja jest nasz po podstawowego Replikator otrzyma potwierdzenie od wszystkich pomocniczej replikatorów. Ta wartość musi być większa niż 64 i potęgi liczby 2.|
|MaxSecondaryReplicationQueueSize|Liczba operacji|16384|Maksymalna liczba operacji w kolejce pomocniczą. Po wprowadzeniu wysoce dostępne za pośrednictwem utrzymywanie stanu jest nasz operacji. Ta wartość musi być większa niż 64 i potęgi liczby 2.|
|CheckpointThresholdInMB|MB|200|Ilość miejsca pliku dziennika, po upływie którego stan jest sprawdzany za.|
|MaxRecordSizeInKB|KB|1024|Największa rozmiar rekordu, który Replikator może tworzyć w dzienniku. Ta wartość musi być wielokrotnością liczby 4 i większe niż 16.|
|OptimizeLogForLowerDiskUsage|Wartość logiczna|wartość PRAWDA.|W przypadku wartości true dziennik są skonfigurowane tak, aby replice dedykowane pliku dziennika jest tworzony przy użyciu plikami NTFS. To obniża rzeczywisty miejsca na dysku dla pliku. Gdy ma wartość false, plik zostanie utworzona i stałych przydziałów, które oferują najlepszą wydajność zapisu.|
|SharedLogId|Identyfikator GUID|""|Określa unikatowy identyfikator guid służących do identyfikowania pliku dziennika udostępnionego z tej replice. Zazwyczaj usług, nie należy używać tego ustawienia. Jednak jeśli określono SharedLogId, opcja SharedLogPath musi być także określona.|
|SharedLogPath|W pełni kwalifikowana nazwa|""|Określa pełną ścieżkę, w której zostanie utworzony plik dziennika udostępnionego dla tej replice. Zazwyczaj usług, nie należy używać tego ustawienia. Jednak jeśli określono SharedLogPath, opcja SharedLogId musi być także określona.|


## <a name="sample-configuration-file"></a>Przykładowy plik konfiguracyjny

```xml
<?xml version="1.0" encoding="utf-8"?>
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Section Name="MyActorServiceReplicatorConfig">
      <Parameter Name="ReplicatorEndpoint" Value="MyActorServiceReplicatorEndpoint" />
      <Parameter Name="BatchAcknowledgementInterval" Value="0.05"/>
      <Parameter Name="CheckpointThresholdInMB" Value="180" />
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

Parametr CheckpointThresholdInMB określa ilość miejsca na dysku Replikator służy do przechowywania informacji o stanie w replice dedykowane pliku dziennika. Zwiększanie tego na wartość większą niż domyślny może spowodować krótszy czas ponowna konfiguracja po dodaniu nowych replice do zestawu. Jest to spowodowane częściowego transferze, która ma być wykonywana z powodu dostępności Historia więcej operacji w dzienniku. To potencjalnie zwiększenie podczas odzyskiwania replice po awarii.

Jeśli OptimizeForLowerDiskUsage jest ustawiona na PRAWDA, miejsce w pliku dziennika będzie nadmiernie ustanawianie tak, aby aktywnych replik można przechowywać więcej informacji o stanie w swoje pliki dziennika, gdy nieaktywny repliki użyje mniej miejsca na dysku. Pozwala udostępniać więcej repliki w węźle. Jeśli OptimizeForLowerDiskUsage jest ustawiona na false, informacje o stanie są zapisywane w plikach dziennika szybciej.

Ustawienie MaxRecordSizeInKB definiuje maksymalny rozmiar rekord, który Replikator mogą być zapisywane w pliku dziennika. W większości przypadków domyślny rozmiar rekordu 1024 KB jest optymalna. Jednak jeśli usługa powoduje większe elementy danych jako część informacji o stanie, ta wartość może być konieczne zwiększenie. Istnieje mały korzyść dokonując MaxRecordSizeInKB mniejszych niż 1024, jak mniejszych rekordów za pomocą tylko potrzebne mniejszych rekordu miejsce. Oczekuje się, że ta wartość należy zmienić tylko czasami.

Ustawienia SharedLogId i SharedLogPath są zawsze umożliwia razem usługi osobnych dziennika udostępnionego z domyślnego dziennika udostępnionego za pomocą węzła. Dla uzyskania najlepszej wydajności należy możliwie jak najwięcej usług możliwie należy określić tego samego dziennika udostępnionego. Udostępnionych plików dziennika powinny znajdować się na dyskach, które są używane wyłącznie do pliku dziennika udostępnionego w celu zmniejszenia konfliktu głowy przepływu. Oczekuje się, że te wartości należy zmienić tylko sporadycznie.
