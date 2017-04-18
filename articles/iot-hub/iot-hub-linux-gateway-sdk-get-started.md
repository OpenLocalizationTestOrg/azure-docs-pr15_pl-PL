<properties
    pageTitle="Rozpoczynanie pracy z SDK bramy Centrum IoT | Microsoft Azure"
    description="W tym instruktażu Azure IoT bramy SDK używa Linux, aby przedstawić podstawowych pojęć, które należy zrozumieć użycie SDK bramy IoT Azure."
    services="iot-hub"
    documentationCenter=""
    authors="chipalost"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="cpp"
     ms.topic="get-started-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/25/2016"
     ms.author="andbuc"/>


# <a name="azure-iot-gateway-sdk-beta---get-started-using-linux"></a>Azure IoT SDK bramy (beta) — wprowadzenie do korzystania z Linux

[AZURE.INCLUDE [iot-hub-gateway-sdk-getstarted-selector](../../includes/iot-hub-gateway-sdk-getstarted-selector.md)]

## <a name="how-to-build-the-sample"></a>Sposoby tworzenia próbki

Przed rozpoczęciem należy [zdefiniować środowiska programowania] [ lnk-setupdevbox] do pracy z zestawu SDK w Linux.

1. Otwórz powłokę.
2. Przejdź do folderu głównego w kopii lokalnej repozytorium **azure-iot brama sdk** .
3. Uruchom skrypt **tools/build.sh** . Ten skrypt używa narzędzie **cmake** utworzyć folder o nazwie **tworzenia** w folderze głównym lokalną kopię repozytorium **azure-iot brama sdk** i generowanie makefile. Następnie skrypt rozwiązanie tworzy i uruchamia testów.

> [AZURE.NOTE]  Każdym uruchomieniu skrypt **build.sh** usuwa i utworzy ponownie **utworzyć** folder w folderze głównym lokalną kopię repozytorium **azure-iot brama sdk** .

## <a name="how-to-run-the-sample"></a>Jak uruchomić próbki

1. Skrypt **build.sh** generuje dane wyjściowe w folderze **tworzenia** w Twojej kopii lokalnej repozytorium **azure-iot brama sdk** . Obejmuje dwa moduły używane w tym przykładzie.

    Skryptu budowania umieszcza **liblogger_hl.so** w **kompilacji i moduły rejestratora-** folder i **libhello_world_hl.so** w **kompilacji i moduły hello_world-** folder. Jak pokazano poniżej plik ustawień JSON należy użyć poniższych ścieżek wartości **ścieżka modułu** .

2. Plik **hello_world_lin.json** w folderze **próbki hello_world-src** jest przykładowy plik ustawień JSON Linux służące do uruchamiania próbki. Ustawienia JSON przykładzie pokazano poniżej przyjęto założenie, że będzie uruchamiana próbki na poziomie głównym lokalną kopię repozytorium **azure-iot brama sdk** .

3. Dla modułu **logger_hl** w sekcji **argumentów** wartość **filename** nazwy i ścieżki pliku, który będzie zawierać dane dziennika.

    To jest przykładowy plik ustawień JSON Linux, który zapisze **log.txt** do folderu, w której jest uruchomiony próbki.

    ```
    {
      "modules" :
      [ 
        {
          "module name" : "logger_hl",
          "module path" : "./build/modules/logger/liblogger_hl.so",
          "args" : 
          {
            "filename":"./log.txt"
          }
        },
        {
          "module name" : "hello_world",
          "module path" : "./build/modules/hello_world/libhello_world_hl.so",
          "args" : null
        }
      ],
      "links" :
      [
        {
          "source": "hello_world",
          "sink": "logger_hl"
        }
      ]
    }
    ```

3. W swojej powłoki przejdź do folderu **azure-iot brama sdk** .
4. Uruchom następujące polecenie:
  
  ```
  ./build/samples/hello_world/hello_world_sample ./samples/hello_world/src/hello_world_lin.json
  ``` 

[AZURE.INCLUDE [iot-hub-gateway-sdk-getstarted-code](../../includes/iot-hub-gateway-sdk-getstarted-code.md)]

<!-- Links -->
[lnk-setupdevbox]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/doc/devbox_setup.md
