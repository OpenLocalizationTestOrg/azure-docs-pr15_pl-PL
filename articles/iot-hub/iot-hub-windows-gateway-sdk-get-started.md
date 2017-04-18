<properties
    pageTitle="Rozpoczynanie pracy z SDK bramy Centrum IoT | Microsoft Azure"
    description="Ilustrowanie podstawowych pojęć, że należy zrozumieć, kiedy używać SDK bramy IoT Azure za pomocą systemu Windows Azure Instruktaż IoT SDK bramy."
    services="iot-hub"
    documentationCenter=""
    authors="chipalost"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="cpp"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/25/2016"
     ms.author="andbuc"/>


# <a name="azure-iot-gateway-sdk-beta---get-started-using-windows"></a>Azure IoT SDK bramy (beta) — wprowadzenie do korzystania z systemu Windows

[AZURE.INCLUDE [iot-hub-gateway-sdk-getstarted-selector](../../includes/iot-hub-gateway-sdk-getstarted-selector.md)]

## <a name="how-to-build-the-sample"></a>Sposoby tworzenia próbki

Przed rozpoczęciem należy [zdefiniować środowiska programowania] [ lnk-setupdevbox] do pracy z zestawu SDK w systemie Windows.

1. Otwórz wiersz polecenia **wiersza polecenia Deweloper VS2015** .
2. Przejdź do folderu głównego w kopii lokalnej repozytorium **azure-iot brama sdk** .
3. Uruchamianie **narzędzia\\build.cmd** skrypt. Ten skrypt tworzy plik rozwiązania Visual Studio, tworzy rozwiązanie i uruchamiania testów. Rozwiązania programu Visual Studio można znaleźć w folderze **Tworzenie** lokalnej kopii repozytorium **azure-iot brama sdk** .

## <a name="how-to-run-the-sample"></a>Jak uruchomić próbki

1. Skrypt **build.cmd** tworzy folder o nazwie **tworzenia** w Twojej kopii lokalnej repozytorium. Ten folder zawiera dwa moduły używane w tym przykładzie.

    Skrypt kompilacji umieszcza **logger_hl.dll** w **Tworzenie\\moduły\\rejestratora\\debugowanie** folder i **hello_world_hl.dll** w **Tworzenie\\moduły\\hello_world\\debugowanie** folder. Jak pokazano poniżej plik ustawień JSON należy użyć poniższych ścieżek wartości **ścieżka modułu** .

2. Plik **hello_world_win.json** w **próbki\\hello_world\\src** folder jest przykładowy plik ustawień JSON dla systemu Windows, które umożliwia uruchamianie próbki. Ustawienia JSON przykładzie pokazano poniżej przyjęto założenie, że sklonowany repozytorium IoT bramy SDK w katalogu głównym dysku **C:** . Jeśli został pobrany do innej lokalizacji, musisz dostosować wartości **ścieżka modułu** **próbki\\hello_world\\src\\hello_world_win.json** w związku z tym plikiem.

3. Dla modułu **logger_hl** w sekcji **argumentów** wartość **filename** nazwy i ścieżki pliku, który będzie zawierać dane dziennika.

    To jest przykładowy plik ustawień JSON dla systemu Windows, która zapisze plik **log.txt** w katalogu głównym dysku **C:** .

    ```
    {
      "modules" :
      [
        {
          "module name" : "logger_hl",
          "module path" : "C:\\azure-iot-gateway-sdk\\build\\modules\\logger\\Debug\\logger_hl.dll",
          "args" : 
          {
            "filename":"C:\\log.txt"
          }
        },
        {
          "module name" : "hello_world",
          "module path" : "C:\\azure-iot-gateway-sdk\\build\\\\modules\\hello_world\\Debug\\hello_world_hl.dll",
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

3. W wierszu polecenia przejdź do folderu głównego lokalną kopię repozytorium **bramy sdk, azure iot w** .
4. Uruchom następujące polecenie:
  
  ```
  build\samples\hello_world\Debug\hello_world_sample.exe samples\hello_world\src\hello_world_win.json
  ```

[AZURE.INCLUDE [iot-hub-gateway-sdk-getstarted-code](../../includes/iot-hub-gateway-sdk-getstarted-code.md)]

<!-- Links -->
[lnk-setupdevbox]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/doc/devbox_setup.md