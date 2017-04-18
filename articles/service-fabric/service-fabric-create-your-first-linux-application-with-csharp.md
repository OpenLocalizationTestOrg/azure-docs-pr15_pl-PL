<properties
   pageTitle="Tworzenie pierwszej aplikacji usługi tkaninie na Linux przy użyciu C# | Microsoft Azure"
   description="Tworzenie i wdrażanie aplikacji tkaninie usługi, za pomocą C#"
   services="service-fabric"
   documentationCenter="csharp"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="csharp"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/04/2016"
   ms.author="subramar"/>


# <a name="create-your-first-azure-service-fabric-application"></a>Tworzenie pierwszej aplikacji tkaninie usługi Azure

> [AZURE.SELECTOR]
- [C# - systemu Windows](service-fabric-create-your-first-application-in-visual-studio.md)
- [Java — Linux](service-fabric-create-your-first-linux-application-with-java.md)
- [C# - Linux](service-fabric-create-your-first-linux-application-with-csharp.md)

Usługa tkaninie przewiduje SDK usług w oparciu o Linux zarówno w podstawowych .NET i języka Java. W tym samouczku przyjrzymy się jak utworzyć aplikację Linux i tworzyć usługi przy użyciu języka C# (.NET Core).

## <a name="prerequisites"></a>Wymagania wstępne

Przed rozpoczęciem, upewnij się, że [Konfigurowanie środowiska programowania Linux](service-fabric-get-started-linux.md). Jeśli korzystasz z systemu Mac OS X, możesz [Konfigurowanie środowiska Linux jednego pola w maszyny wirtualnej przy użyciu Vagrant](service-fabric-get-started-mac.md).

## <a name="create-the-application"></a>Tworzenie aplikacji

Aplikacja usługi tkaninie może zawierać jedną lub więcej usług, każda z konkretnej roli w przedstawiania funkcje aplikacji. Usługa SDK tkaninie Linux zawiera generator [Yeoman](http://yeoman.io/) , który ułatwia tworzenie pierwszej usługi i, aby dodać więcej później. Aby utworzyć aplikację z pojedynczą usługą, można użyć Yeoman.

1. W terminal wpisz następujące polecenie, aby rozpocząć tworzenie rusztowania:`yo azuresfcsharp`

2. Nazwa aplikacji.

3. Wybierz typ usługi pierwszy i nadaj mu nazwę. Na potrzeby tego samouczka wybrany niezawodne usługi aktora.

  ![Generator Yeoman tkaninie usługi dla C#][sf-yeoman]

>[AZURE.NOTE] Aby uzyskać więcej informacji o tych opcjach zobacz [Omówienie modelu programowania tkaninie usługi](service-fabric-choose-framework.md).

## <a name="build-the-application"></a>Tworzenie aplikacji

Szablony Yeoman tkaninie usługi zawierają skryptu budowania, który służy do tworzenia aplikacji ze terminal (po Przechodzenie do folderu aplikacji).

  ```bash
 cd myapp 
 ./build.sh 
  ```

## <a name="deploy-the-application"></a>Wdrażanie aplikacji

Po skonstruowaniu aplikacji należy wdrożyć go z klastrem lokalnej przy użyciu interfejsu wiersza polecenia Azure.

1. Połącz się z lokalnym klastrem tkaninie usługi.

    ```bash
    azure servicefabric cluster connect
    ```

2. Skorzystaj z tego skryptu instalacji opisane w szablonie, skopiuj pakiet aplikacji do sklepu obraz klaster, zarejestrować typ aplikacji oraz tworzenia wystąpienie aplikacji.

    ```bash
    ./install.sh
    ```

3. Otwórz przeglądarkę sieci i przejdź do usługi tkaninie Eksploratora u http://localhost:19080-Eksploratora (Zamień host lokalny z prywatne IP maszyn wirtualnych, korzystając z Vagrant w systemie Mac OS X).

4. Rozwiń węzeł aplikacji i zwróć uwagę, że jest teraz wpis typu aplikacji, a innej pierwsze wystąpienie tego typu.

## <a name="start-the-test-client-and-perform-a-failover"></a>Uruchom klienta test i wykonać trybie awaryjnym

Projekty aktor nie dowolnego elementu na własne. Wymagają innych usług lub klienta są wysyłane wiadomości. Szablon Aktor zawiera skrypt prosty test, który służy do współdziałania z usługą aktora.

1. Uruchom skrypt wyświetlane usługę Aktor przy użyciu narzędzia czujki.

    ```bash
    cd myactorsvcTestClient
    watch -n 1 ./testclient.sh
    ```

2. W Eksploratorze tkaninie usługi Odszukaj węzeł obsługujący podstawowego replice usługi aktora. Poniżej ekranu jest węzeł 3.

    ![Znajdowanie podstawowego replice w Eksploratorze tkaninie usługi][sfx-primary]

3. Kliknij węzeł, który został znaleziony w poprzednim kroku, a następnie z menu Akcje wybierz pozycję **Dezaktywuj (ponowne uruchomienie komputera)** . Ta akcja ponowne uruchomienie jednego z pięciu węzłów w klastrze lokalne wymuszania trybie awaryjnym replice pomocniczą w innym. Jak wykonać tę akcję, należy zwrócić uwagę na wynik testu klienta i należy zauważyć, że licznik będzie nadal występował, aby dodać kolejne pomimo tym przełączeniu.


## <a name="next-steps"></a>Następne kroki

- [Dowiedz się więcej o zaufanego uczestników](service-fabric-reliable-actors-introduction.md)
- [Interakcja z klastrów tkaninie usługi przy użyciu interfejsu wiersza polecenia Azure](service-fabric-azure-cli.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-create-your-first-linux-application-with-csharp/yeoman-csharp.png
[sfx-primary]: ./media/service-fabric-create-your-first-linux-application-with-csharp/sfx-primary.png
