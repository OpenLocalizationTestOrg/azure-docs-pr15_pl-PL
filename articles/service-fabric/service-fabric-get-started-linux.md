<properties
   pageTitle="Konfigurowanie środowiska programowania w systemie Linux | Microsoft Azure"
   description="Zainstaluj środowisko uruchomieniowe i zestaw SDK i utworzyć klaster rozwoju lokalnego w systemie Linux. Po zakończeniu instalacji ten użytkownik będzie gotowy do tworzenia aplikacji."
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/26/2016"
   ms.author="seanmck"/>

# <a name="prepare-your-development-environment-on-linux"></a>Przygotowywanie środowiska programowania w systemie Linux


> [AZURE.SELECTOR]
-[Systemu Windows](service-fabric-get-started.md)
- [Linux](service-fabric-get-started-linux.md)
- [OSX](service-fabric-get-started-mac.md)

 Wdrażanie i uruchamianie [aplikacji tkaninie usługi Azure](service-fabric-application-model.md) na komputerze rozwoju Linux, zainstaluj środowisko uruchomieniowe i typowych SDK. Można również zainstalować opcjonalne SDK dla języka Java i .NET Core.

## <a name="prerequisites"></a>Wymagania wstępne
### <a name="supported-operating-system-versions"></a>Obsługiwane wersje systemów operacyjnych
Dla rozwoju są obsługiwane z następujących wersji systemu operacyjnego:

- Ubuntu 16.04 (Xenial Xerus)

## <a name="update-your-apt-sources"></a>Aktualizowanie źródła stanie

Aby zainstalować zestawu SDK oraz pakiet skojarzone środowisko uruchomieniowe stanie alerty, możesz zaktualizować źródła stanie.

1. Otwórz terminal.
2. Dodawanie repo tkaninie usługi do listy źródeł.

    ```bash
    sudo sh -c 'echo "deb [arch=amd64] http://apt-mo.trafficmanager.net/repos/servicefabric/ trusty main" > /etc/apt/sources.list.d/servicefabric.list'
    ```

3. Dodaj nowy klucz GPG do swojego stanie zestaw kluczy.

    ```bash
    sudo apt-key adv --keyserver apt-mo.trafficmanager.net --recv-keys 417A0893
    ```

4. Odświeżanie listy pakiet oparte na nowo dodanym repozytoria.

    ```bash
    sudo apt-get update
    ```

## <a name="install-and-set-up-the-sdk"></a>Instalowanie i konfigurowanie zestawu SDK

Po zaktualizowaniu źródła, możesz zainstalować zestawu SDK.

1. Zainstaluj pakiet SDK tkaninie usługi. Zostanie wyświetlony monit o potwierdzenie instalacji i zaakceptować umowę licencyjną.

    ```bash
    sudo apt-get install servicefabricsdkcommon
    ```

2. Uruchom skrypt Instalatora zestawu SDK.

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/common/sdkcommonsetup.sh
    ```

## <a name="set-up-the-azure-cross-platform-cli"></a>Konfigurowanie Azure polecenie między platformami

[Polecenie między platformami Azure] [ azure-xplat-cli-github] zawiera polecenia umożliwiające interakcje z osobami tkaninie usługę, w tym klastrów i aplikacje. Jest oparty na Node.js tak, [Upewnij się, że zainstalowano węzeł] [ install-node] przed kontynuowaniem z poniższymi instrukcjami.

1. Klonowanie repo github na komputerze rozwoju.

    ```bash
    git clone https://github.com/Azure/azure-xplat-cli.git
    ```

2. Przełączanie do sklonowanym repo i zainstaluj zależności polecenie za pomocą Menedżera pakietów węzeł (npm).

    ```bash
    cd azure-xplat-cli
    npm install
    ```

3. Umożliwia tworzenie łącza symbolicznego z folderu Kosz i azure sklonowanym repo /usr/bin/azure, aby jest dodawany do ścieżki i polecenia są dostępne z dowolnego katalogu.

    ```bash
    sudo ln -s $(pwd)/bin/azure /usr/bin/azure
    ```

4. Na koniec należy włączyć automatyczne uzupełnianie tkaninie usługi poleceń.

    ```bash
    azure --completion >> ~/azure.completion.sh
    echo 'source ~/azure.completion.sh' >> ~/.bash_profile
    source ~/azure.completion.sh
    ```

## <a name="set-up-a-local-cluster"></a>Konfigurowanie klaster lokalny

Jeśli wszystko zainstalował pomyślnie, należy uruchomić klaster lokalny.

1. Uruchom skrypt konfiguracji klaster.

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/common/clustersetup/devclustersetup.sh
    ```

2. Otwórz przeglądarkę sieci web i przejdź do http://localhost:19080-Eksploratora. Jeśli klaster został uruchomiony, powinna być widoczna na pulpicie nawigacyjnym Eksploratora tkaninie usługi.

    ![Usługa tkaninie Explorer w systemie Linux][sfx-linux]

W tym momencie jest możliwe do wdrożenia gotowych pakietów aplikacji usługi tkaninie lub nowe na podstawie kontenerów gości lub pliki wykonywalne gościa. Do tworzenia nowych usług za pomocą języka Java lub .NET Core SDK, wykonaj poniższe czynności, Instalacja opcjonalna.

## <a name="install-the-java-sdk-and-eclipse-neon-plugin-optional"></a>Zainstaluj wtyczkę Java SDK i neonu Zaćmienie (opcjonalnie)

Java SDK udostępnia biblioteki i szablonów wymagane do utworzenia usług tkaninie usługi przy użyciu języka Java.

1. Zainstaluj pakiet Java SDK.

    ```bash
    sudo apt-get install servicefabricsdkjava
    ```

2. Uruchom skrypt Instalatora zestawu SDK.

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/java/sdkjavasetup.sh
    ```

Możesz zainstalować wtyczki Zaćmienie tkaninie usługi z poziomu Zaćmienie IDE neonu.

1. W Zaćmienie upewnij się, że masz Buildship wersji 1.0.17 lub nowszej. Możesz sprawdzić wersji zainstalowanych składników, wybierając pozycję **Pomoc > szczegółowe informacje dotyczące instalacji**. Możesz zaktualizować Buildship zgodnie z instrukcjami zawartymi [w tym miejscu][buildship-update].

2. Aby zainstalować wtyczkę tkaninie usług, wybierz pozycję **Pomoc > Instalowanie nowego oprogramowania...**

3. W polu tekstowym "Praca z" Wprowadź: http://dl.windowsazure.com/eclipse/servicefabric

4. Kliknij przycisk Dodaj.

    ![Dodatek Zaćmienie][sf-eclipse-plugin]

5. Wybierz dodatek tkaninie usługi, a następnie kliknij przycisk Dalej.

6. Postępuj zgodnie z instrukcjami instalacji i zaakceptuj umowę licencyjną użytkownika oprogramowania.

## <a name="install-the-net-core-sdk-optional"></a>Instalowanie SDK Core .NET (opcjonalnie)

.NET Core SDK udostępnia biblioteki i szablonów wymagane do utworzenia usług tkaninie usługi przy użyciu podstawowych .NET między platformami.

1. Zainstaluj pakiet .NET Core SDK.

    ```bash
    sudo apt-get install servicefabricsdkcsharp
    ```

2. Uruchom skrypt Instalatora zestawu SDK.

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/csharp/sdkcsharpsetup.sh
    ```

## <a name="next-steps"></a>Następne kroki

- [Tworzenie pierwszej aplikacji Java w systemie Linux](service-fabric-create-your-first-linux-application-with-java.md)

- [Przygotowywanie środowiska programowania na OSX](service-fabric-get-started-mac.md)


<!-- Links -->

[azure-xplat-cli-github]: https://github.com/Azure/azure-xplat-cli
[install-node]: https://nodejs.org/en/download/package-manager/#installing-node-js-via-package-manager
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship

<!--Images -->

[sf-eclipse-plugin]: ./media/service-fabric-get-started-linux/service-fabric-eclipse-plugin.png
[sfx-linux]: ./media/service-fabric-get-started-linux/sfx-linux.png
