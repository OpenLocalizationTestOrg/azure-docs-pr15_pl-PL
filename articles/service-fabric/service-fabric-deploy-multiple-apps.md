<properties
   pageTitle="Wdrażanie aplikacji Node.js, która korzysta z MongoDB | Microsoft Azure"
   description="Przewodnik po dotyczące pakietu wielu plików wykonywalnych gościa wdrożenia klaster tkaninie usługi Azure"
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
   ms.workload="NA"
   ms.date="10/22/2016"
   ms.author="msfussell;mikhegn"/>


# <a name="deploy-multiple-guest-executables"></a>Wdrażanie wielu plików wykonywalnych gościa

W tym artykule przedstawiono sposób pakowanie i wdrażanie wielu plików wykonywalnych gościa tkaninie usługi Azure. Tworzenie i wdrażanie pojedynczy pakiet tkaninie usługi odczytywanie jak [wdrożyć Gość wykonywalny na tkaninie usługi](service-fabric-deploy-existing-app.md).

Podczas tego instruktażu pokazano, jak wdrożyć aplikację z Node.js fronton korzystającego z MongoDB do przechowywania danych, możesz zastosować kroki dla innych aplikacji, która ma zależności w innej aplikacji.   

Za pomocą programu Visual Studio da pakiet aplikacji, który zawiera wiele plików wykonywalnych gościa. Zobacz [Przy użyciu programu Visual Studio, aby pakiet istniejącej aplikacji](service-fabric-deploy-existing-app.md#using-visual-studio-to-package-an-existing-executable). Po dodaniu pierwszego wykonywalny gościa, kliknij prawym przyciskiem myszy w projekcie aplikacji i wybierz pozycję **Dodaj -> Nowy tkaninie usługi usługi** Aby dodać drugi projekt wykonywalny gościa do rozwiązania. Uwaga: Jeśli wybierzesz połączenia źródła w projekcie programu Visual Studio, tworzenie rozwiązania programu Visual Studio będzie upewnij się, że pakietu aplikacji jest aktualne informacje o zmiany wprowadzone w źródle. 

## <a name="manually-package-the-multiple-guest-executable-application"></a>Ręczne pakowanie wielu aplikacja wykonywalna gościa
Można również ręcznie spakować wykonywalny gościa. W przypadku ręcznego opakowania, w tym artykule używa narzędzie opakowań tkaninie usług, które są dostępne w [http://aka.ms/servicefabricpacktool](http://aka.ms/servicefabricpacktool).

### <a name="packaging-the-nodejs-application"></a>Pakowanie aplikacji Node.js
W tym artykule założono, że Node.js nie jest zainstalowany w węzłach w klastrze tkaninie usługi. W konsekwencji musisz dodać Node.exe do katalogu głównego aplikacji węzeł przed opakowaniu. Strukturę katalogu zastosowania Node.js (za pomocą Express web struktura i aparat Jade szablonu) powinna wyglądać podobnie do poniższego:

```
|-- NodeApplication
  	|-- bin
        |-- www
  	|-- node_modules
        |-- .bin
        |-- express
        |-- jade
        |-- etc.
  	|-- public
        |-- images
        |-- etc.
  	|-- routes
        |-- index.js
        |-- users.js
  	|-- views
        |-- index.jade
        |-- etc.
  	|-- app.js
  	|-- package.json
  	|-- node.exe
```

Następnym krokiem możesz utworzyć pakiet aplikacji w celu zastosowania Node.js. Poniższy kod tworzy pakiet aplikacji usługi tkaninie zawierającego aplikację Node.js.

```
.\ServiceFabricAppPackageUtil.exe /source:'[yourdirectory]\MyNodeApplication' /target:'[yourtargetdirectory] /appname:NodeService /exe:'node.exe' /ma:'bin/www' /AppType:NodeAppType
```

Poniżej zamieszczono opis parametry, które są używane:

- **/Source** wskazuje katalog aplikacji, który powinien być spakowany.
- **/ TARGET** określa katalogu, w którym można utworzyć pakiet. Ten katalog ma różni się od katalogu źródłowego.
- **elementów/appname** Określa nazwę aplikacji istniejącej aplikacji. Należy zrozumieć przekłada do nazwy usługi w manifeście, a nie do nazwy aplikacji usługi tkaninie.
- **/exe** Określa plik wykonywalny tkaninie usługa ma uruchomić, w tym przypadku `node.exe`.
- **/ma** Określa argument, który jest używany do uruchamiania plik wykonywalny. Jak Node.js nie jest zainstalowany, tkaninie usługi należy uruchomić serwer sieci web Node.js przez wykonywanie `node.exe bin/www`.  `/ma:'bin/www'`Wskazuje narzędzie opakowań, aby użyć `bin/ma` jako argumentu node.exe.
- **/AppType** Określa nazwę typu aplikacji usługi tkaninie.

Po przejściu do katalogu, który został określony w parametrze/TARGET, możesz zobaczyć narzędzie utworzyła pakiet tkaninie usługi w pełni funkcjonalny tak jak pokazano poniżej:

```
|--[yourtargetdirectory]
  	|-- NodeApplication
        |-- C
              |-- bin
              |-- data
              |-- node_modules
              |-- public
              |-- routes
              |-- views
              |-- app.js
              |-- package.json
              |-- node.exe
        |-- config
              |--Settings.xml
        |-- ServiceManifest.xml
  	|-- ApplicationManifest.xml
```
Wygenerowane ServiceManifest.xml ma teraz sekcję, która zawiera opis sposobu uruchomione Node.js serwer sieci web, jak pokazano poniżej wstawkę kodu:

```xml
<CodePackage Name="C" Version="1.0">
    <EntryPoint>
        <ExeHost>
            <Program>node.exe</Program>
            <Arguments>'bin/www'</Arguments>
            <WorkingFolder>CodePackage</WorkingFolder>
        </ExeHost>
    </EntryPoint>
</CodePackage>
```
W tym przykładzie serwer sieci web Node.js wykrywa do portu 3000, tak aby zaktualizować informacje punktu końcowego w pliku ServiceManifest.xml, tak jak pokazano poniżej.   

```xml
<Resources>
      <Endpoints>
        <Endpoint Name="NodeServiceEndpoint" Protocol="http" Port="3000" Type="Input" />
      </Endpoints>
</Resources>
```
### <a name="packaging-the-mongodb-application"></a>Pakowanie aplikacji MongoDB
Teraz, gdy masz spakowany aplikacji Node.js, możesz teraz i pakowanie MongoDB. Wymienione przed kroki, przechodzących przez teraz nie są specyficzne dla Node.js i MongoDB. W rzeczywistości dotyczą wszystkie aplikacje, które mają być dostarczana razem w jednej aplikacji usługi tkaninie.  

Aby pakiet MongoDB, chcesz upewnij się, że pakiet Mongod.exe i Mongo.exe. Oba pliki binarne znajdują się w `bin` katalogu MongoDB katalogu instalacji. Struktura katalogów będzie podobny do poniższego.

```
|-- MongoDB
  	|-- bin
        |-- mongod.exe
        |-- mongo.exe
        |-- anybinary.exe
```
Usługi tkaninie musi rozpoczynać MongoDB podobnego do polecenia poniżej, więc należy używać `/ma` parametru w przypadku pakowania MongoDB.

```
mongod.exe --dbpath [path to data]
```
> [AZURE.NOTE] Dane nie jest są zachowywane w przypadku awarii węzła, jeśli katalog danych MongoDB zostanie umieszczony w katalogu lokalnym węzła. Należy używać magazynu trwałego lub zaimplementować replice MongoDB, aby zapobiec utracie danych.  

PowerShell lub powłoki poleceń możemy Uruchom narzędzie opakowań z poniższych parametrów:

```
.\ServiceFabricAppPackageUtil.exe /source: [yourdirectory]\MongoDB' /target:'[yourtargetdirectory]' /appname:MongoDB /exe:'bin\mongod.exe' /ma:'--dbpath [path to data]' /AppType:NodeAppType
```

Aby dodać MongoDB do pakietu aplikacji tkaninie usługi, musisz upewnij się, że punkty parametru/TARGET do tego samego katalogu, która już zawiera aplikację pojawiają się wraz z aplikacji Node.js. Należy upewnić się, że korzystasz z tej samej nazwy ApplicationType.

Załóżmy przejdź do katalogu i sprawdź, co utworzyła narzędzie.

```
|--[yourtargetdirectory]
  	|-- MyNodeApplication
  	|-- MongoDB
        |-- C
            |--bin
                |-- mongod.exe
                |-- mongo.exe
                |-- etc.
        |-- config
            |--Settings.xml
        |-- ServiceManifest.xml
  	|-- ApplicationManifest.xml
```
Jak widać, narzędzie dodany nowy folder, MongoDB, do katalogu zawierającego pliki binarne MongoDB. Po otwarciu `ApplicationManifest.xml` pliku, widać, że pakiet zawiera teraz aplikację Node.js i MongoDB. Poniższy kod zawartość manifest aplikacji.

```xml
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyNodeApp" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MongoDB" ServiceManifestVersion="1.0" />
   </ServiceManifestImport>
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="NodeService" ServiceManifestVersion="1.0" />
   </ServiceManifestImport>
   <DefaultServices>
      <Service Name="MongoDBService">
         <StatelessService ServiceTypeName="MongoDB">
            <SingletonPartition />
         </StatelessService>
      </Service>
      <Service Name="NodeServiceService">
         <StatelessService ServiceTypeName="NodeService">
            <SingletonPartition />
         </StatelessService>
      </Service>
   </DefaultServices>
</ApplicationManifest>  
```

### <a name="publishing-the-application"></a>Publikowanie aplikacji
Ostatnim krokiem jest opublikowanie aplikacji do lokalnych klastrów tkaninie usługi za pomocą skryptów programu PowerShell poniżej:

```
Connect-ServiceFabricCluster localhost:19000

Write-Host 'Copying application package...'
Copy-ServiceFabricApplicationPackage -ApplicationPackagePath '[yourtargetdirectory]' -ImageStoreConnectionString 'file:C:\SfDevCluster\Data\ImageStoreShare' -ApplicationPackagePathInImageStore 'NodeAppType'

Write-Host 'Registering application type...'
Register-ServiceFabricApplicationType -ApplicationPathInImageStore 'NodeAppType'

New-ServiceFabricApplication -ApplicationName 'fabric:/NodeApp' -ApplicationTypeName 'NodeAppType' -ApplicationTypeVersion 1.0  
```

Gdy aplikacja zostanie pomyślnie opublikowana klaster lokalny, masz dostęp do aplikacji Node.js na porcie, który firma Microsoft wprowadzony w manifestu usługi aplikacji Node.js — na przykład http://localhost:3000.

W tym samouczku przejrzane jak łatwo pakiety dwóch istniejących aplikacji jednej aplikacji usługi tkaninie. Również zapoznaniu wdrożenie go na tkaninie usługi, tak aby mogą korzystać z niektórych funkcji tkaninie usługi, takie jak wysokiej dostępności i kondycji systemu integracji.

## <a name="next-steps"></a>Następne kroki

- Więcej informacji na temat wdrażania kontenerów Przegląd [usług i kontenerów](service-fabric-containers-overview.md)
