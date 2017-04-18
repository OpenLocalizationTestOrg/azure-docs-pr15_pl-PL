<properties
    pageTitle="Instalowanie Azure interfejs wiersza polecenia | Microsoft Azure"
    description="Instalowanie interfejsu wiersza polecenia Azure (polecenie) dla komputerów Mac, Linux i systemu Windows rozpocząć korzystanie z usługi Azure"
    editor=""
    manager="timlt"
    documentationCenter=""
    authors="squillace"
    services="virtual-machines-linux,virtual-network,storage,azure-resource-manager"
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="multiple"
    ms.workload="multiple"
    ms.tgt_pltfrm="command-line-interface"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="rasquill"/>
    
# <a name="install-the-azure-cli"></a>Instalowanie polecenie Azure

> [AZURE.SELECTOR]
- [Programu PowerShell](powershell-install-configure.md)
- [Polecenie Azure](xplat-cli-install.md)

Szybko zainstalować interfejsu wiersza polecenia Azure (polecenie Azure) do użycia zestaw open source oparty na powłoce poleceń do tworzenia i zarządzania zasobami Microsoft Azure. Istnieje kilka opcji, aby zainstalować te narzędzia i platform na komputerze: 

* **pakiet npm** — Uruchom npm (Menedżer pakietu języka JavaScript), aby zainstalować najnowszy pakiet polecenie Azure na rozkład Linux lub system operacyjny. Wymaga node.js i npm na Twoim komputerze.
* **Instalator** - plik do pobrania i Instalatora prosta instalacja w systemie Mac lub Windows.
* **Kontener docker** - Rozpoczynanie korzystania z najnowszych polecenie w kontenerze Docker gotowe Szybka instalacja. Wymaga hosta Docker na Twoim komputerze.
    
Aby uzyskać więcej opcji i tła Zobacz repozytorium programu project na [GitHub](https://github.com/azure/azure-xplat-cli). 

Po polecenie Azure jest zainstalowany, [Podłącz je z subskrypcją usługi Azure](xplat-cli-connect.md) i uruchomieniu polecenia **azure** ze swojego interfejsu wiersza polecenia (imprezie, Terminal, wiersz polecenia i tak dalej) do pracy z zasobami Azure.



## <a name="option-1-install-an-npm-package"></a>Opcja 1: Instalowanie pakietu npm

Aby zainstalować polecenie z pakietu npm, upewnij się, zostały pobrane i zainstalowane [najnowsze Node.js i npm](https://nodejs.org/en/download/package-manager/). Następnie uruchom **npm zainstalować** , aby zainstalować pakiet azure polecenie: 

    npm install -g azure-cli

Na rozkład Linux może być konieczne **sudo** umożliwia pomyślne uruchomienie polecenia __npm__ w następujący sposób:

    sudo npm install -g azure-cli

> [AZURE.NOTE]Jeśli chcesz zainstalować lub zaktualizować Node.js i npm na rozkład Linux lub systemu operacyjnego, zaleca się zainstalowanie najnowszej wersji KÓW Node.js (4.x). Jeśli używasz starszej wersji, może wystąpić błędy instalacji. 

Jeśli wolisz, Pobierz najnowszą Linux [plik tar] [ linux-installer] pakietu npm lokalnie. Następnie zainstalować pakiet pobrany npm w następujący sposób (na rozkład Linux, może być konieczne używanie **sudo**):

    npm install -g <path to downloaded tar file>

## <a name="option-2-use-an-installer"></a>Opcja 2: Za pomocą Instalatora

Jeśli używasz komputera Mac i systemu Windows, następujących programów instalacyjnych interfejsu wiersza polecenia są dostępne do pobrania:

* [Instalator systemu Mac OS X][mac-installer]

* [Windows MSI][windows-installer] 

>[AZURE.TIP]W systemie Windows możesz pobrać [Instalatora platformy sieci Web](https://go.microsoft.com/?linkid=9828653) w celu zainstalowania interfejsu wiersza polecenia. Ten Instalator udostępnia opcję, aby zainstalować dodatkowe SDK Azure i narzędzi wiersza polecenia po zainstalowaniu polecenie. 


## <a name="option-3-use-a-docker-container"></a>Opcja 3: Używanie kontenera Docker

Jeśli masz skonfigurowane na komputerze jako [Docker](https://docs.docker.com/engine/understanding-docker/) host, można uruchamiać najnowszą polecenie Azure w kontenerze Docker. Uruchom następujące polecenie (na rozkład Linux, może być konieczne używanie **sudo**):

```
docker run -it microsoft/azure-cli
```


## <a name="run-azure-cli-commands"></a>Uruchom polecenie Azure polecenia
Po zainstalowaniu Azure interfejsu wiersza polecenia Uruchom polecenie **azure** z interfejsu użytkownika wiersza polecenia (imprezie, Terminal, wiersz polecenia i tak dalej). Na przykład aby uruchomić polecenia help, wpisz poniższe dane:

```
azure help
```
> [AZURE.NOTE]Na niektórych dystrybucji Linux może zostać wyświetlony komunikat o błędzie podobny do `/usr/bin/env: ‘node’: No such file or directory`. Ten błąd pochodzi z ostatnich instalacji Node.js jest zainstalowany w /usr/bin/nodejs. Aby rozwiązać problem, należy utworzyć łącza symbolicznego do /usr/bin/node wykonanie tego polecenia:

```
sudo ln -s /usr/bin/nodejs /usr/bin/node
```

Aby wyświetlić wersję polecenie Azure został zainstalowany, wpisz następujące polecenie:

```
azure --version
```

Teraz możesz przystąpić! Aby uzyskać dostęp do wszystkich poleceń interfejsu wiersza polecenia do pracy z własnych zasobów, [Nawiązywanie połączenia z subskrypcji pakietu Azure za pomocą interfejsu wiersza polecenia Azure](xplat-cli-connect.md).

>[AZURE.NOTE] Przy pierwszym użyciu polecenie Azure, zostanie wyświetlony komunikat z pytaniem, czy Zezwalaj firmie Microsoft na pobieranie informacji o użyciu. Uczestnictwo jest nieobowiązkowe. Jeśli chcesz uczestniczyć można wyłączyć w dowolnym momencie, uruchamiając `azure telemetry --disable`. Aby włączyć uczestnictwo w dowolnym momencie, uruchom `azure telemetry --enable`.


## <a name="update-the-cli"></a>Zaktualizuj interfejs wiersza polecenia

Firma Microsoft udostępnia często zaktualizowane wersje polecenie Azure. Zainstaluj ponownie polecenie za pomocą Instalatora systemu operacyjnego lub uruchomić najnowszą kontener Docker. Lub, jeśli masz najnowsze Node.js i npm zainstalowany, zaktualizuj wpisując następujące (na rozkład Linux, może być konieczne używanie **sudo**).

```
npm update -g azure-cli
```

## <a name="enable-tab-completion"></a>Włączanie karty ukończenia

Karta uzupełniania poleceń interfejsu wiersza polecenia jest obsługiwane dla komputerów Mac i Linux.

Aby włączyć ją w zsh, uruchom polecenie:

```
echo '. <(azure --completion)' >> .zshrc
```

Aby włączyć ją w imprezie, uruchom polecenie:

```
azure --completion >> ~/azure.completion.sh
echo 'source ~/azure.completion.sh' >> ~/.bash_profile
```


## <a name="next-steps"></a>Następne kroki 

* [Nawiązywanie połączenia z interfejsu wiersza polecenia do subskrypcji usługi Azure](xplat-cli-connect.md) tworzyć i zarządzać nimi Azure zasobów.

* Aby dowiedzieć się więcej o polecenie Azure, pobieranie kodu źródłowego, zgłaszania problemów lub przyczynić się do projektu, odwiedź [repozytorium GitHub polecenie Azure](https://github.com/azure/azure-xplat-cli).

* Jeśli masz pytania dotyczące używania polecenie Azure lub Azure, odwiedź [Fora Azure](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurescripting).

* Jeśli chcesz, możesz spróbować oparte na Python [Azure polecenie 2.0 Podgląd](https://github.com/azure/azure-cli).

[mac-installer]: http://aka.ms/mac-azure-cli
[windows-installer]: http://aka.ms/webpi-azure-cli
[linux-installer]: http://aka.ms/linux-azure-cli
[cliasm]: virtual-machines-command-line-tools.md
[cliarm]: ./virtual-machines/azure-cli-arm-commands.md
