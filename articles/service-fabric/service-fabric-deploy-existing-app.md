<properties
   pageTitle="Wdrażanie istniejący plik wykonywalny tkaninie usługi Azure | Microsoft Azure"
   description="Instrukcje dotyczące pakietu istniejącą aplikację jako gość plik wykonywalny, może zostać rozmieszczony na klastrze tkaninie usługi"
   services="service-fabric"
   documentationCenter=".net"
   authors="msfussell"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="na"
   ms.date="10/22/2016"
   ms.author="msfussell;mikhegn"/>

# <a name="deploy-a-guest-executable-to-service-fabric"></a>Wdrażanie Gość wykonywalny tkaninie usługi

Dowolny typ aplikacji, takich jak node.js, Java lub macierzyste aplikacje można uruchamiać na tkaninie usługi Azure. Usługa tkaninie odwołuje się do tych typów aplikacji wykonywalny gościa.
Pliki wykonywalne gościa są traktowane przez tkaninie usługi takie jak stateless usług. W wyniku są umieszczane w węzłach w klastrze, na podstawie dostępności i inne wskaźniki. W tym artykule opisano pakowanie i wdrażanie Gość wykonywalny klastrze tkaninie usługi przy użyciu programu Visual Studio lub narzędzie wiersza polecenia.

W tym artykule firma Microsoft obejmuje czynności, aby pakowanie Gość wykonywalny i Wdroż tkaninie usługi.  

## <a name="benefits-of-running-a-guest-executable-in-service-fabric"></a>Zalety uruchamiania Gość wykonywalny w tkaninie usługi

Istnieje kilka zalet dostarczane z uruchomionymi Gość wykonywalny w klastrze tkaninie usługi:

- Wysokiej dostępności. Aplikacje, które działają w tkaninie usługi są udostępniane wysoce. Tkaninie usługi gwarantuje, że są uruchomione wystąpienia aplikacji.
- Monitorowanie kondycji. Monitorowanie kondycji usługi tkaninie wykrywa Jeśli aplikacja jest uruchomiona i udostępnia informacje diagnostyczne w przypadku awarii.   
- Zarządzanie aplikacjami cyklu życia. Oprócz zapewnienia uaktualnienia bez przestojów, tkaninie Usługa udostępnia automatycznego przywrócenia poprzedniej wersji przypadku zdarzenie nieprawidłowe kondycji zgłoszone podczas uaktualniania.    
- Gęstość. Wiele aplikacji można uruchamiać w klastrze eliminuje konieczność każdej aplikacji działanie na własny sprzętu.


## <a name="overview-of-application-and-service-manifest-files"></a>Omówienie aplikacji i usług manifestu plików

W ramach wdrażania Gość wykonywalny warto zrozumieć opakowań tkaninie usługi i model wdrożenia jako opisane [model aplikacji](service-fabric-application-model.md). Model opakowań tkaninie usługi zależy od dwóch plików XML: manifesty aplikacji i usług. Definicja schematu pliki ApplicationManifest.xml i ServiceManifest.xml jest zainstalowany wraz z usługi SDK tkaninie do *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.

* **Pojawiają aplikacji** Manifest aplikacji jest używany do opisu aplikacji. Wyświetla listę usług, które tworzą go i inne parametry, które służą do określania, jak co najmniej jednej usługi powinny zostać wdrożone, takie jak liczba wystąpień.

  W tkaninie usługi aplikacji jest jednostka wdrażanie i uaktualnianie. Aplikacja można uaktualnić jako całość miejsce, w którym są zarządzane potencjalne błędy i potencjalnych wycofywania. Usługa tkaninie gwarantuje, że proces uaktualniania jest pomyślnie, lub, jeśli uaktualnienie nie powiedzie się, nie pozostaw aplikację w nieznany i stanie.

* **Usługa pojawiają** Manifestu usługi opisano składniki usługi. Zawiera dane, takie jak nazwa i typ usługi, a jego kodu, konfiguracji i danych. Manifestu usługi także pewne dodatkowe parametry, których można używać do konfigurowania usługi po wdrożeniu.


## <a name="application-package-file-structure"></a>Struktura pliku pakietu aplikacji
Aby wdrożyć aplikację na tkaninie usługi, wymagane przez aplikację do obserwowania strukturę katalogów wstępnie zdefiniowanych. Poniższy przykład tej struktury.

```
|-- ApplicationPackageRoot
  	|-- GuestService1Pkg
        |-- Code
            |-- existingapp.exe
        |-- Config
            |-- Settings.xml
        |-- Data
        |-- ServiceManifest.xml
  	|-- ApplicationManifest.xml
```

ApplicationPackageRoot znajduje się plik ApplicationManifest.xml, który definiuje aplikacji. Podkatalogów dla każdej usługi zawarte w aplikacji służy do wszystkich artefaktów wymagane przez usługę — ServiceManifest.xml i zazwyczaj następujące trzy katalogi zawierają:

- *Kod*. Ten katalog zawiera kod usługi.
- *Konfiguracji*. Ten katalog zawiera pliku Settings.xml (i innych plików w razie potrzeby) czy usługa mają dostęp w czasie wykonywania do pobierania określone ustawienia konfiguracji.
- *Dane*. To dodatkowe katalog do przechowywania dodatkowe dane lokalne, które usługa może być konieczne. Uwaga: Danych należy przechowywać tylko tymczasowych danych. Usługa tkaninie nie kopii skopiowanymi zmian w katalogu danych Jeśli usługa musi zostać przeniesiona — na przykład podczas pracy awaryjnej.

Uwaga: Nie trzeba tworzyć `config` i `data` katalogów, jeśli nie potrzebujesz.

## <a name="packaging-an-existing-executable"></a>Pakowanie istniejący plik wykonywalny

Gdy opakowania wykonywalny gościa, można albo szablon projektu programu Visual Studio lub [ręcznie utworzyć pakiet aplikacji](#manually). Przy użyciu programu Visual Studio, struktury pakietu aplikacji i pliki manifestu są tworzone przez Kreatora projektu dla Ciebie.

>[AZURE.NOTE] Najłatwiejszym sposobem pakowanie wykonywalny do usługi Windows istniejących jest za pomocą programu Visual Studio.

## <a name="using-visual-studio-to-package-an-existing-executable"></a>Pakowanie istniejący plik wykonywalny za pomocą programu Visual Studio

Program Visual Studio zawiera szablon usługi tkaninie usługi ułatwiające wdrażanie Gość wykonywalny do klastrów tkaninie usługi.

Przeglądanie następujące kroki, aby ukończyć publikowania:

1. Wybierz Plik -> Nowy projekt, a następnie utworzyć aplikację tkaninie usługi.
2. Wybierz plik wykonywalny gościa jako szablon usługi.
3. Kliknij przycisk Przeglądaj, wybierz folder z Twojej plik wykonywalny i Wypełnij pozostałą parametrów w celu utworzenia usługi.
    - Można ustawić *Zachowanie pakiet kod* skopiuj całą zawartość folderu do projektu programu Visual Studio, co jest przydatne, jeśli plik wykonywalny nie zmienia się. Jeśli plik wykonywalny, aby zmienić i mieć możliwość skompletowanie kompilacjach nowe dynamiczne, można utworzyć łącze do folderu. Należy zauważyć, że połączone folderów można używać podczas tworzenia projektu aplikacji w programie Visual Studio. Ten prowadzi do lokalizacji źródłowej z w projekcie, umożliwiając aktualizowanie wykonywalny gościa miejsca docelowego źródła o tych zmianach stają się częścią pakiet aplikacji na tworzenie.
    - *Program* — wybierz plik wykonywalny, który ma zostać wykonane, aby uruchomić usługę.
    - *Argumenty* - określ argumenty, które mają być przekazywane do plik wykonywalny. Można go, aby uzyskać listę parametrów z argumentami.
    - *WorkingFolder* - Określa katalog roboczy dla procesu, który ma zostać uruchomiony. Można określić trzech wartości:
        - `CodeBase`Określa, że katalogu roboczego ma być równa katalogu kod w pakiet aplikacji (`Code` katalogu w strukturze pliku pokazane poprzedzającego).
        - `CodePackage`Określa, że katalogu roboczego będzie można ustawić w katalogu głównym pakietu aplikacji (`GuestService1Pkg` w strukturze pliku pokazane poprzedzającego).
        - `Work`Określa, że pliki są umieszczane w podkatalogów o nazwie pracy
4. Nadaj nazwę tej usługi, a następnie kliknij przycisk OK.
5. Jeśli Twoja usługa wymaga punkt końcowy dla komunikacji, możesz teraz dodać do pliku ServiceManifest.xml protokół, Port i typ. e.g. `<Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000" UriScheme="http" PathSuffix="myapp/" Type="Input" />`.
6. Można teraz użycie funkcji Spakuj i publikować akcji przed lokalne klaster przez debugowania rozwiązania w programie Visual Studio. Gdy gotowe można publikować aplikacji klaster zdalny lub ewidencjonowanie rozwiązanie do kontroli źródła.
7. Przejdź na końcu tego artykułu, aby dowiedzieć się, jak do widoku, który gościa, za pomocą usługi wykonywalny w programie Explorer tkaninie usługi.

<a id="manually"></a>
## <a name="manually-packaging-and-deploying-an-existing-executable"></a>Ręczne pakowania i wdrażanie istniejący plik wykonywalny
Proces ręcznego opakowania wykonywalny gościa jest oparty na następujące czynności:

1. Tworzenie struktury katalogów pakiet.
2. Dodawanie kodu i konfiguracji pliki aplikacji.
3. Edytowanie pliku manifestu usługi.
4. Edytuj plik manifestu aplikacji.

<!--
>[AZURE.NOTE] We do provide a packaging tool that allows you to create the ApplicationPackage automatically. The tool is currently in preview. You can download it from [here](http://aka.ms/servicefabricpacktool).
-->

### <a name="create-the-package-directory-structure"></a>Tworzenie struktury katalogów pakietu
Możesz uruchomić tworzenie struktury katalogów opisane wcześniej.

### <a name="add-the-applications-code-and-configuration-files"></a>Dodawanie plików kodu i konfiguracji aplikacji
Po utworzeniu struktury katalogów, możesz dodać kod aplikacji i pliki konfiguracyjne w katalogach kod i konfiguracji. Możesz również utworzyć dodatkowe i podkatalogów w obszarze kod lub konfiguracji katalogów.

Tkaninie usługi czy xcopy zawartości katalogu głównego aplikacji, dlatego wstępnie zdefiniowanych struktury do użycia innego niż tworzenie dwie najważniejsze katalogów, kod i ustawienia. (Możesz wybrać różne nazwy przydatna. Szczegółowe informacje znajdują się w następnej sekcji.)

>[AZURE.NOTE] Upewnij się, że możesz umieścić wszystkie pliki i zależności wymagane przez aplikację. Usługi tkaninie kopiuje zawartość pakietu aplikacji we wszystkich węzłach w klastrze miejsce, w którym będą usługi aplikacji do wdrożenia. Pakiet powinien zawierać wszystkie kod, który ma być uruchamiana aplikacja. Zaleca się przy założeniu, że zależności są już zainstalowane.

### <a name="edit-the-service-manifest-file"></a>Edytowanie pliku manifestu usługi
Następnym krokiem jest do edytowania pliku manifestu usługi, aby uwzględnić następujące informacje:

- Nazwa typu usługi. To jest identyfikator tkaninie usługi jest używana usługa.
- Polecenie umożliwia uruchamianie aplikacji (ExeHost).
- Dowolny skrypt, który musi być uruchomione, aby ustawić Strzałka w górę i skonfiguruj aplikację (SetupEntrypoint).

Oto przykład `ServiceManifest.xml` pliku:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Name="NodeApp" Version="1.0.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceTypes>
      <StatelessServiceType ServiceTypeName="NodeApp" UseImplicitHost="true"/>
   </ServiceTypes>
   <CodePackage Name="code" Version="1.0.0.0">
      <SetupEntryPoint>
         <ExeHost>
             <Program>scripts\launchConfig.cmd</Program>
         </ExeHost>
      </SetupEntryPoint>
      <EntryPoint>
         <ExeHost>
            <Program>node.exe</Program>
            <Arguments>bin/www</Arguments>
            <WorkingFolder>CodePackage</WorkingFolder>
         </ExeHost>
      </EntryPoint>
   </CodePackage>
   <Resources>
      <Endpoints>
         <Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000" Type="Input" />
      </Endpoints>
   </Resources>
</ServiceManifest>
```

Przyjrzyjmy się na różne części pliku, który należy zaktualizować:

#### <a name="updating-the-servicetypes"></a>Aktualizowanie ServiceTypes

```xml
<ServiceTypes>
  <StatelessServiceType ServiceTypeName="NodeApp" UseImplicitHost="true" />
</ServiceTypes>
```

- Można wybrać dowolną nazwę, która ma `ServiceTypeName`. Wartość jest używana w `ApplicationManifest.xml` pliku do identyfikowania usługi.
- Należy określić `UseImplicitHost="true"`. Ten atrybut instruuje tkaninie usługi, że usługa jest oparty na samodzielne aplikacji, dlatego wszystkie potrzeby tkaninie usługi zrobić aby uruchomić go jako proces i monitorowanie jego kondycji.

#### <a name="updating-the-codepackage"></a>Aktualizowanie CodePackage
CodePackage element określa położenie (i wersji) kod usługi.

```xml
<CodePackage Name="Code" Version="1.0.0.0">
```

`Name` Element jest używany do określenia nazwy katalogu w pakiecie aplikacji, która zawiera kod usługi. `CodePackage`również `version` atrybut. Może służyć do określenia wersji kodu —, a także potencjalnie można uaktualnić kod usługi przy użyciu infrastruktury zarządzania cyklu życia aplikacji usługi tkaninie.
#### <a name="optional-updating-the-setupentrypoint"></a>Opcjonalnie: Aktualizowanie SetupEntrypoint

```xml
<SetupEntryPoint>
   <ExeHost>
       <Program>scripts\launchConfig.cmd</Program>
   </ExeHost>
</SetupEntryPoint>
```
SetupEntrypoint element jest używana do określenia dowolny plik wykonywalny lub partii powinna zostać wykonana przed kod usługi są uruchomione. Jest krok opcjonalny, więc nie mają zostać uwzględnione w przypadku ustawienia nie są inicjowanie-wymagane. SetupEntryPoint jest wykonywane każdorazowo po ponownym uruchomieniu usługi.

Aby skrypty instalacji i konfiguracji trzeba mają być grupowane w pliku jednej partii, jeśli aplikacji instalacji i konfiguracji wymaga wiele skryptów jest tylko jeden SetupEntrypoint. SetupEntrypoint można wykonywać dowolny typ pliku — wykonywalny plików, pliki wsadowe i poleceń cmdlet programu PowerShell. Aby uzyskać więcej informacji zobacz [Konfigurowanie SetupEntryPoint](service-fabric-application-runas-security.md).

W powyższym przykładzie SetupEntrypoint uruchamia plik wsadowy o nazwie `LaunchConfig.cmd` znajdującej się w `scripts` podkatalogów katalogu kodu (przy założeniu, WorkingFolder element jest ustawiona na ścieżka).

#### <a name="updating-the-entrypoint"></a>Aktualizowanie punktu wejścia

```xml
<EntryPoint>
  <ExeHost>
    <Program>node.exe</Program>
    <Arguments>bin/www</Arguments>
    <WorkingFolder>CodeBase</WorkingFolder>
  </ExeHost>
</EntryPoint>
```

`Entrypoint` Element w pliku manifestu usługi jest używany do określenia sposobu uruchamiania usługi. `ExeHost` Element Określa plik wykonywalny (i argumenty) należy uruchomić usługę.

- `Program`Nazwa wykonywalny, który należy wykonać, aby uruchomić usługę.
- `Arguments`Określa argumenty, które powinny być przekazywane do plik wykonywalny. Można go, aby uzyskać listę parametrów z argumentami.
- `WorkingFolder`Określa katalog roboczy dla procesu, który ma zostać uruchomiony. Można określić trzech wartości:
    - `CodeBase`Określa, że katalogu roboczego ma być równa katalogu kod w pakiet aplikacji (`Code` katalogu w poprzednim struktura pliku).
    - `CodePackage`Określa, że katalogu roboczego będzie można ustawić w katalogu głównym pakietu aplikacji (`GuestService1Pkg` w poprzednim struktura pliku).
  - `Work`Określa, że pliki są umieszczane w podkatalogów o nazwie pracy

WorkingFolder jest przydatna dla Ustawianie właściwą pracy, tak aby ścieżki względne mogą być używane przez aplikacji lub inicjowanie skryptów.

#### <a name="updating-the-endpoints-and-registering-with-naming-service-for-communication"></a>Aktualizowanie punkty końcowe i rejestrowanie Usługa nazewnictwa dla komunikacji

```xml
<Endpoints>
   <Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000" Type="Input" />
</Endpoints>

```
W powyższym przykładzie `Endpoint` element określa punkty końcowe, które wykrywać w aplikacji. W tym przykładzie aplikacji Node.js wykrywa HTTP na porcie 3000.

Ponadto możesz zadać tkaninie usługi Publikowanie tego punktu końcowego Usługa nazewnictwa tak innych usług mogą one znaleźć adres końcowy do tej usługi. Dzięki temu można mieć możliwość komunikowania się między usługami, które są pliki wykonywalne gościa.
Adres końcowy opublikowanych ma postać `UriScheme://IPAddressOrFQDN:Port/PathSuffix`. `UriScheme`i `PathSuffix` są atrybuty opcjonalne. `IPAddressOrFQDN`jest to adres IP lub w pełni kwalifikowaną nazwę domeny węzła ten plik wykonywalny zostaje umieszczony na i zostaną obliczone dla Ciebie.

W poniższym przykładzie po wdrożeniu usługi w Eksploratorze tkaninie usługi będzie widoczne punktu końcowego, które są podobne do `http://10.1.4.92:3000/myapp/` opublikowane wystąpienie usługi lub jeśli jest to komputer lokalny, zostanie wyświetlony `http://localhost:3000/myapp/`. 

```xml
<Endpoints>
   <Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000"  UriScheme="http" PathSuffix="myapp/" Type="Input" />
</Endpoints>
```
Za pomocą tych adresów i [zwrotny serwer proxy](service-fabric-reverseproxy.md) do komunikacji między usługami.

### <a name="edit-the-application-manifest-file"></a>Edytowanie pliku manifestu aplikacji

Po skonfigurowaniu `Servicemanifest.xml` pliku, należy wprowadzić zmiany do `ApplicationManifest.xml` pliku w celu zapewnienia, że są używane typ usługi poprawnego i nazwę.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="NodeAppType" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="NodeApp" ServiceManifestVersion="1.0.0.0" />
   </ServiceManifestImport>
</ApplicationManifest>
```

#### <a name="servicemanifestimport"></a>ServiceManifestImport

W `ServiceManifestImport` element, można określić jedną lub kilka usług, które mają zostać uwzględnione w aplikacji. Usługi pochodzą z `ServiceManifestName`, który określa nazwę katalogu miejsce, w którym `ServiceManifest.xml` znajduje się plik.

```xml
<ServiceManifestImport>
  <ServiceManifestRef ServiceManifestName="NodeApp" ServiceManifestVersion="1.0.0.0" />
</ServiceManifestImport>
```

## <a name="set-up-logging"></a>Konfigurowanie rejestrowania
Dla plików wykonywalnych gościa warto będą mogli zobaczyć konsoli Dzienniki, aby dowiedzieć się, jeśli skryptów aplikacji i konfiguracji zawierają wszystkie błędy.
Można skonfigurować przekierowanie konsoli `ServiceManifest.xml` plik za pomocą `ConsoleRedirection` elementu.

```xml
<EntryPoint>
  <ExeHost>
    <Program>node.exe</Program>
    <Arguments>bin/www</Arguments>
    <WorkingFolder>CodeBase</WorkingFolder>
    <ConsoleRedirection FileRetentionCount="5" FileMaxSizeInKb="2048"/>
  </ExeHost>
</EntryPoint>
```

* `ConsoleRedirection`można przekierowywać wyjścia konsoli (stdout i stderr) do katalogu roboczego, aby można było używać do weryfikacji, że nie ma żadnych błędów podczas instalacji lub wykonanie aplikacji w klastrze tkaninie usługi.

    * `FileRetentionCount`Określa, ile pliki są zapisane w katalogu roboczego. Wartość 5, na przykład oznacza, że pliki dziennika do poprzedniego wykonania pięć są przechowywane w katalogu roboczego.
    * `FileMaxSizeInKb`Określa maksymalny rozmiar plików dziennika.

Pliki dziennika są zapisywane w jednym z katalogów pracy usługi. Aby określić, gdzie znajdują się pliki, musisz określić, który węzeł jest uruchomiona, a które katalog pracy jest używany za pomocą Eksploratora tkaninie usługi. Ten proces jest objęta w dalszej części tego artykułu.

## <a name="deployment"></a>Wdrożenie
Ostatnim krokiem jest wdrażania aplikacji. Poniższy skrypt programu PowerShell przedstawiono sposób wdrażania aplikacji do klastrów rozwoju lokalnego i rozpocznij nową usługę tkaninie usługi.

```PowerShell

Connect-ServiceFabricCluster localhost:19000

Write-Host 'Copying application package...'
Copy-ServiceFabricApplicationPackage -ApplicationPackagePath 'C:\Dev\MultipleApplications' -ImageStoreConnectionString 'file:C:\SfDevCluster\Data\ImageStoreShare' -ApplicationPackagePathInImageStore 'nodeapp'

Write-Host 'Registering application type...'
Register-ServiceFabricApplicationType -ApplicationPathInImageStore 'nodeapp'

New-ServiceFabricApplication -ApplicationName 'fabric:/nodeapp' -ApplicationTypeName 'NodeAppType' -ApplicationTypeVersion 1.0

New-ServiceFabricService -ApplicationName 'fabric:/nodeapp' -ServiceName 'fabric:/nodeapp/nodeappservice' -ServiceTypeName 'NodeApp' -Stateless -PartitionSchemeSingleton -InstanceCount 1

```
Usługa tkaninie usługi mogą być rozmieszczone w różnych "konfiguracji". Na przykład mogą być rozmieszczone jako jednego lub wielu wystąpień lub mogą być rozmieszczone w taki sposób, że jest jedno wystąpienie usługi w każdym węźle klaster tkaninie usługi.

`InstanceCount` Parametr `New-ServiceFabricService` polecenia cmdlet służy do określania, ile wystąpień usługi powinna być uruchamiana w klastrze tkaninie usługi. Można ustawić `InstanceCount` wartości, w zależności od typu aplikacji, którym jest wykonywane wdrożenie. Są dwa najbardziej typowe scenariusze:

* `InstanceCount = "1"`. W tym przypadku tylko jedno wystąpienie usługi zostanie wdrożony w klastrze. Harmonogram usługi tkaninie Określa, który węzeł ma być rozmieszczone na usługę.

* `InstanceCount ="-1"`. W tym przypadku jedno wystąpienie usługi zostanie wdrożony w każdym węźle w klastrze tkaninie usługi. Wynik jest o jeden (i tylko jeden) wystąpienie usługi dla każdego węzła w klastrze.

Jest to przydatne konfigurowania aplikacji frontonu (na przykład, umieść punkt końcowy), ponieważ aplikacje klienckie wymaga nawiązania "" dowolną węzłów w klastrze za pomocą punkt końcowy. Również można tej konfiguracji, gdy na przykład wszystkich węzłów klaster tkaninie usługi jest podłączony do równoważenia obciążenia, aby ruch klientów może zostać rozłożone na usługa, która jest uruchomiony na wszystkich węzłach w klastrze.

## <a name="check-your-running-application"></a>Sprawdzanie uruchomionego aplikacji

W Eksploratorze usługi tkaninie identyfikowanie węzeł, na którym jest uruchomiona usługa. W tym przykładzie wykonywania w węźle Node1:

![Węzeł, w którym jest uruchomiona usługa](./media/service-fabric-deploy-existing-app/nodeappinsfx.png)

Jeśli przejdź do węzła i przejdź do aplikacji, pojawi się węzeł podstawowe informacje, w tym miejsca na dysku.

![Miejsce na dysku](./media/service-fabric-deploy-existing-app/locationondisk2.png)

Przejdź do katalogu za pomocą Eksploratora serwera, jak na poniższej ilustracji pokazano można znaleźć katalogu roboczego i folder dziennika usługi.

![Lokalizacja dziennika](./media/service-fabric-deploy-existing-app/loglocation.png)


## <a name="next-steps"></a>Następne kroki
W tym artykule zapoznaniu pakowanie wykonywalny gościa i Wdroż tkaninie usługi. Jako następnego kroku należy wyewidencjonować dodatkowe zawartość tego tematu.

- [Próbki dla opakowań i wdrażanie Gość wykonywalny na GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/master/GuestExe/SimpleApplication), łącznie z łączem do wstępną narzędzie opakowań
- [Wdrażanie wielu plików wykonywalnych gościa](service-fabric-deploy-multiple-apps.md)
- [Tworzenie pierwszej aplikacji usługi tkaninie przy użyciu programu Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md)
