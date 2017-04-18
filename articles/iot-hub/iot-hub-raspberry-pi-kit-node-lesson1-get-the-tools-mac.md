<properties
 pageTitle="Pobierz narzędzia (macOS 10.10) | Microsoft Azure"
 description="Pobierz i zainstaluj na macOS niezbędne narzędzia i oprogramowanie do pierwszej przykładowej aplikacji usługi liczby pi."
 services="iot-hub"
 documentationCenter=""
 authors="shizn"
 manager="timlt"
 tags=""
 keywords=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/21/2016"
 ms.author="xshi"/>

# <a name="12-get-the-tools-macos-1010"></a>1.2 uzyskać dostęp do narzędzi (macOS 10.10)

> [AZURE.SELECTOR]
- [Windows 7 +](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-win32.md)
- [Ubuntu 16.04](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-ubuntu.md)
- [macOS 10.10](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-mac.md)

## <a name="121-what-you-will-do"></a>1.2.1 będzie czego

Pobierz narzędzia programistyczne i oprogramowanie do pierwszej przykładowej aplikacji dla programu malina Pi 3. Jeśli są spełnione problemów szukanie rozwiązań [Rozwiązywanie problemów z strony](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="122-what-you-will-learn"></a>1.2.2 co dowiesz się
W tej sekcji przedstawiono:

- Jak zainstalować cyfra i Node.js
    - [Cyfra](https://git-scm.com) jest Otwórz źródło distributed systemu kontroli wersji. Przykładowej aplikacji na potrzeby tej lekcji są przechowywane na cyfra.
    - [Node.js](https://nodejs.org/en/) jest obsługi języka JavaScript z ekosystemu sformatowanego pakiet.
- Jak zainstalować dodatkowe narzędzia rozwoju Node.js za pomocą NPM.
  - Minimalna wersja wymagane Node.js jest KÓW 4,5.
  - [NPM](https://www.npmjs.com) jest jednym z menedżerów pakiet Node.js.

## <a name="123-what-you-need"></a>1.2.3 co jest potrzebne

- Połączenie internetowe, aby pobrać narzędzia programistyczne i oprogramowania
- Mac uruchomionym Yosemite macOS (10.10) lub nowszym

## <a name="124-install-git-and-nodejs"></a>1.2.4 zainstalować cyfra i Node.js

Aby zainstalować cyfra i Node.js, należy użyć narzędzia zarządzania pakietu [Homebrew](http://brew.sh) , wykonując następujące czynności:

1. Zainstaluj Homebrew. Jeśli masz już zainstalowaną Homebrew, przejdź do kroku 2.
  1. Naciśnij klawisz `Cmd + Space` i wprowadź `Terminal` aby otworzyć okno końcowych.
  2. Uruchom następujące polecenie:

    ```bash
    /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
    ```
2. Zainstaluj cyfra i Node.js, uruchamiając następujące polecenie:

    ```bash
    brew install node git
    ```

## <a name="125-install-additional-nodejs-development-tools"></a>1.2.5 dodatkowe narzędzia rozwoju Node.js zainstalować

[Gulp.js](http://gulpjs.com) służy do automatyzowania wdrażania aplikacji przykładowych do Pi. [Urządzenie odnajdowania — polecenie](https://github.com/Azure/device-discovery-cli) jest również używać do pobierania informacji dotyczących sieci o urządzeniach IoT.

Instalowanie `gulp` i `device-discovery-cli` , uruchamiając następujące polecenie w oknie końcowych:

```bash
sudo npm install -g device-discovery-cli gulp
```

Jeśli występują problemy z instalacją Node.js i te narzędzia programistyczne dodatkowe na macOS, zobacz [Podręcznik rozwiązywania problemów](iot-hub-raspberry-pi-kit-node-troubleshooting.md) dla rozwiązania typowych problemów.

## <a name="126-install-visual-studio-code"></a>1.2.6 instalowanie kodu programu Visual Studio

[Pobierz](https://code.visualstudio.com/docs/setup/osx) i zainstaluj program Visual Studio kod. Kod Visual Studio to proste, ale wydajnych źródła Edytor kodu dla systemu Windows, Linux i macOS. Aby edytować kod przykładowy za pomocą tego edytora w dalszej części samouczka.

## <a name="127-summary"></a>1.2.7 podsumowanie

Po zainstalowaniu narzędzi do tworzenia wymaganych i oprogramowania przy pierwszym zastosowaniu próbki. W następnej sekcji Tworzenie, wdrażanie i uruchamianie aplikacji próbki w swojej Pi.

## <a name="next-steps"></a>Następne kroki

[1.3 Tworzenie i wdrażanie aplikacji przykładowej miganie](iot-hub-raspberry-pi-kit-node-lesson1-deploy-blink-app.md)
