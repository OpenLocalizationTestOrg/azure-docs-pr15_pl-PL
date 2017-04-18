<properties
   pageTitle="Konfigurowanie środowiska programowania w systemie Mac OS X | Microsoft Azure"
   description="Zainstaluj środowisko uruchomieniowe, SDK i narzędzia i utworzyć klaster rozwoju lokalnego. Po zakończeniu instalacji ten użytkownik będzie gotowy do tworzenia aplikacji w systemie Mac OS X."
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
   ms.date="09/25/2016"
   ms.author="seanmck"/>

# <a name="set-up-your-development-environment-on-mac-os-x"></a>Konfigurowanie środowiska programowania w systemie Mac OS X

> [AZURE.SELECTOR]
-[Systemu Windows](service-fabric-get-started.md)
- [Linux](service-fabric-get-started-linux.md)
- [OSX](service-fabric-get-started-mac.md)

Można tworzyć tkaninie usługi uruchamiania aplikacji na klastrów Linux przy użyciu systemu Mac OS X. W tym artykule opisano, jak skonfigurować komputera Mac rozwoju.

## <a name="prerequisites"></a>Wymagania wstępne

Tkaninie usługi nie działa on w systemie OS X. Aby uruchomić lokalny klaster tkaninie usługi, firma Microsoft udostępnia wstępnie skonfigurowane maszyny wirtualnej Ubuntu przy użyciu Vagrant i VirtualBox. Przed rozpoczęciem należy:

- [Vagrant (v1.8.4 lub nowsza)](http://wwww.vagrantup.com/downloads)
- [VirtualBox](http://www.virtualbox.org/wiki/Downloads)

## <a name="create-the-local-vm"></a>Tworzenie lokalnej maszyn wirtualnych

Aby utworzyć lokalne maszyn wirtualnych zawierające klastrze tkaninie usługi węzeł 5, wykonaj następujące czynności:

1. Klonowanie Vagrantfile repo

    ```bash
    git clone https://github.com/azure/service-fabric-linux-vagrant-onebox.git
    ```

2. Przejdź do lokalnych duplikat repo

    ```bash
    cd service-fabric-linux-vagrant-onebox
    ```

3. (Opcjonalnie) Modyfikowanie domyślnych ustawień maszyn wirtualnych

    Domyślnie lokalne maszyn wirtualnych skonfigurowano w następujący sposób:

    - 3 GB pamięci przydzielone
    - Sieci prywatnych hosta skonfigurowane na IP 192.168.50.50 umożliwiające przekazywanie ruchu z hosta komputerów Mac

    Można zmienić w dowolnym z tych ustawień lub Dodaj inne konfiguracji do maszyn wirtualnych w Vagrantfile. Zapoznaj się z [dokumentacją Vagrant](http://www.vagrantup.com/docs) Aby uzyskać pełną listę opcji konfiguracji.

4. Tworzenie maszyn wirtualnych

    ```bash
    vagrant up
    ```

    W tym kroku pobiera wstępnie obrazu maszyn wirtualnych, uruchamiania, który go lokalnie, a następnie skonfiguruj lokalne tkaninie usługi Klaster w nim. Możesz się spodziewać się zająć kilka minut. Jeśli konfiguracja zakończy się pomyślnie, zostanie wyświetlony komunikat rezultaty, wskazująca, że klaster uruchamiania.

    ![Klaster Rozpoczynanie konfigurowania po maszyn wirtualnych inicjowania obsługi administracyjnej.][cluster-setup-script]

5. Przetestuj klaster został skonfigurowany poprawnie, przechodząc do usługi tkaninie Eksploratora u http://192.168.50.50:19080-Eksploratora (przy założeniu, że zachowane domyślne prywatnej sieci IP).

    ![Usługa tkaninie Eksploratora wyświetlać na hoście komputerów Mac][sfx-mac]


## <a name="install-the-service-fabric-plugin-for-eclipse-neon-optional"></a>Zainstaluj wtyczkę tkaninie usługi dla neonu Zaćmienie (opcjonalnie)

Usługa tkaninie przewiduje IDE neonu Zaćmienie, do którego można uprościć proces tworzenia i wdrażanie usług Java wtyczki.

1. W Zaćmienie upewnij się, że masz Buildship wersji 1.0.17 lub nowszej. Możesz sprawdzić wersji zainstalowanych składników, wybierając pozycję **Pomoc > szczegółowe informacje dotyczące instalacji**. Możesz zaktualizować Buildship zgodnie z instrukcjami zawartymi [w tym miejscu][buildship-update].

2. Aby zainstalować wtyczkę tkaninie usług, wybierz pozycję **Pomoc > Instalowanie nowego oprogramowania...**

3. W polu tekstowym "Praca z" Wprowadź: http://dl.windowsazure.com/eclipse/servicefabric.

4. Kliknij przycisk Dodaj.

    ![Dodatek neonu Zaćmienie tkaninie usługi][sf-eclipse-plugin-install]

5. Wybierz dodatek tkaninie usługi, a następnie kliknij przycisk Dalej.

6. Postępuj zgodnie z instrukcjami instalacji i zaakceptuj umowę licencyjną użytkownika oprogramowania.

## <a name="next-steps"></a>Następne kroki

- [Tworzenie pierwszej aplikacji usługi tkaninie Linux](service-fabric-create-your-first-linux-application-with-java.md)

<!-- Links -->

- [Utworzyć klaster tkaninie usługi w portalu Azure](service-fabric-cluster-creation-via-portal.md)
- [Tworzenie klaster tkaninie usługi za pomocą Menedżera zasobów Azure](service-fabric-cluster-creation-via-arm.md)
- [Opis modelu aplikacji tkaninie usługi](service-fabric-application-model.md)

<!-- Images -->
[cluster-setup-script]: ./media/service-fabric-get-started-mac/cluster-setup-mac.png
[sfx-mac]: ./media/service-fabric-get-started-mac/sfx-mac.png
[sf-eclipse-plugin-install]: ./media/service-fabric-get-started-mac/sf-eclipse-plugin-install.png
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship
