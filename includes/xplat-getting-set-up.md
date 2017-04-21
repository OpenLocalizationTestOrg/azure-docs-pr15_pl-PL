<properties services="virtual-machines" title="Setting up Azure CLI for service management" authors="squillace" solutions="" manager="timlt" editor="tysonn" />

<tags
   ms.service="virtual-machine"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="linux"
   ms.workload="infrastructure"
   ms.date="04/13/2015"
   ms.author="rasquill" />

## <a name="using-azure-cli"></a>Za pomocą interfejsu wiersza polecenia Azure

Poniższe czynności ułatwiają za pomocą interfejsu wiersza polecenia Azure łatwo najnowszej wersji i pisane z wielkiej litery subskrypcji. Jeśli musisz zainstalować polecenie Azure i najpierw połączyć się z kontem, zobacz [interfejs wiersza polecenia Azure (polecenie Azure)](xplat-cli-install.md).

### <a name="step-1-update-azure-cli-version"></a>Krok 1: Polecenie Azure aktualizacji wersji

Aby użyć polecenie Azure konieczne poleceń w trybie zarządzania usługi, należy użyć najnowszej wersji, jeśli to możliwe. Aby sprawdzić swoją wersję, wpisz `azure --version`. Powinien zostać wyświetlony mniej więcej tak:

    $ azure --version
    0.8.17 (node: 0.10.25)

Jeśli chcesz zaktualizować wersję polecenie Azure, zobacz [Polecenie Azure](https://github.com/Azure/azure-xplat-cli).

### <a name="step-2-set-the-azure-account-and-subscription"></a>Krok 2: Ustawianie konto Azure i subskrypcji

Po połączeniu usługi Azure interfejsu wiersza polecenia przy użyciu konta, które ma być używany, może być więcej niż jedną subskrypcję. Jeśli zrobisz, należy zapoznać się subskrypcje dostępne dla Twojego konta, wpisując `azure account list`, a następnie wybierz subskrypcję, do którego chcesz użyć, wpisując `azure account set <subscription id or name> true` gdzie _identyfikator subskrypcji lub nazwa_ jest identyfikator subskrypcji lub nazwę subskrypcji, którą chcesz pracować w bieżącej sesji. Powinien zostać wyświetlony podobną do następujących czynności:

    $ azure account set "Visual Studio Ultimate with MSDN" true
    info:    Executing command account set
    info:    Setting subscription to "Visual Studio Ultimate with MSDN" with id "0e220bf6-5caa-4e9f-8383-51f16b6c109f".
    info:    Changes saved
    info:    account set command OK

> [AZURE.NOTE] Jeśli nie masz jeszcze konta usługi Azure, ale masz subskrypcję usługi subskrypcji MSDN, możesz pobrać bezpłatną Azure środków aktywując usługi [tutaj korzyści subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) — lub korzystając z bezpłatnego konta. Albo będzie działać dostępu Azure.
