<properties
    pageTitle="Tworzenie Centrum IoT za pomocą interfejsu wiersza polecenia Azure | Microsoft Azure"
    description="Postępuj zgodnie z tego artykułu, aby utworzyć koncentratora IoT przy użyciu interfejsu wiersza polecenia Azure."
    services="iot-hub"
    documentationCenter=".net"
    authors="BeatriceOltean"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="multiple"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/21/2016"
     ms.author="boltean"/>

# <a name="create-an-iot-hub-using-azure-cli"></a>Tworzenie Centrum IoT przy użyciu polecenie Azure

[AZURE.INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a>Wprowadzenie

Interfejs wiersza polecenia Azure umożliwia tworzenie i zarządzanie nimi koncentratory Azure IoT programowy. W tym artykule pokazano, jak używać Azure interfejsu wiersza polecenia do tworzenia koncentratora IoT.

Aby użyć tego samouczka są potrzebne następujące elementy:

- Konto Azure active. Jeśli nie masz konta, możesz utworzyć [bezpłatne konto] [ lnk-free-trial] na kilka minut.
- [Polecenie Azure 0.10.4] [ lnk-CLI-install] lub nowszym. Jeśli masz już polecenie Azure można sprawdzić poprawność bieżącą wersję przy użyciu następującego polecenia w wierszu polecenia:
```
    azure --version
```

> [AZURE.NOTE] Azure występują dwa modele rozmieszczania służące do tworzenia i pracy z zasobami: [Menedżer zasobów Azure i klasyczny](../resource-manager-deployment-model.md). Polecenie Azure musi być w trybie Azure Menedżera zasobów:
```
    azure config mode arm
```

## <a name="set-your-azure-account-and-subscription"></a>Ustaw swoje konto Azure i subskrypcji 

1. Podczas logowania wiersz polecenia, wpisując następujące polecenie
```
    azure login
```
Za pomocą przeglądarki sieci web sugerowane i kod do uwierzytelnienia.

2. Jeśli masz wiele subskrypcji Azure nawiązywanie Azure będzie udzielić dostępu do wszystkich subskrypcji Azure skojarzone z poświadczenia. Zobaczysz Azure subskrypcji, a także domyślnym, który jest za pomocą polecenia
```
    azure account list 
```

Aby ustawić kontekstu subskrypcji w obszarze, który chcesz uruchomić pozostałą część użyj poleceń

```
    azure account set <subscription name>
```

3. Jeśli nie masz grupa zasobów, można utworzyć jeden nazwany **exampleResourceGroup** 
```
    azure group create -n exampleResourceGroup -l westus
```

> [AZURE.TIP] Artykuł [polecenie Azure Zarządzanie Azure zasobów i grup zasobów za pomocą] [ lnk-CLI-arm] znajdziesz więcej informacji na temat używania polecenie Azure do zarządzania zasobami Azure. 


## <a name="create-an-iot-hub"></a>Tworzenie Centrum IoT

Wymagane parametry:

```
 azure iothub create -g <resource-group> -n <name> -l <location> -s <sku-name> -u <units>  
    - <resourceGroup> The resource group name (case insensitive alphanumeric, underscore and hyphen, 1-64 length)
    - <name> (The name of the IoT hub to be created. The format is case insensitive alphanumeric, underscore and hyphen, 3-50 length )
    - <location> (The location (azure region/datacenter) where the IoT hub will be provisioned.
    - <sku-name> (The name of the sku, one of: [F1, S1, S2, S3] etc. For the latest full list refer to the pricing page for IoT Hub.
    - <units> (The number of provisioned units. Range : F1 [1-1] : S1, S2 [1-200] : S3 [1-10]. IoT Hub units are based on your total message count and the number of devices you want to connect.)
```
Aby wyświetlić wszystkie parametry dostępne do utworzenia umożliwiają polecenia help w wierszu polecenia
```
    azure iothub create -h 
```
Szybki przykład:

 Aby utworzyć koncentratora IoT o nazwie **exampleIoTHubName** w grupie zasobów **exampleResourceGroup** po prostu uruchom następujące polecenie
```
    azure iothub create -g exampleResourceGroup -n exampleIoTHubName -l westus -k s1 -u 1
```

> [AZURE.NOTE] To polecenie polecenie Azure tworzy Centrum IoT standardowe S1 którego konta. Możesz usunąć IoT Centrum **exampleIoTHubName** , korzystając z następujących poleceń 
```
    azure iothub delete -g exampleResourceGroup -n exampleIoTHubName
```


## <a name="next-steps"></a>Następne kroki
Aby dowiedzieć się więcej o tworzeniu IoT koncentratora, zobacz następujące artykuły:
- [Centrum IoT SDK][lnk-sdks]

Aby jeszcze bardziej Poznawanie możliwości Centrum IoT, zobacz:

- [Zarządzanie Centrum IoT za pomocą Azure Portal][lnk-portal]

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-azure-portal]: https://portal.azure.com/
[lnk-status]: https://azure.microsoft.com/status/
[lnk-CLI-install]: ../xplat-cli-install.md
[lnk-rest-api]: https://msdn.microsoft.com/library/mt589014.aspx
[lnk-CLI-arm]: ../xplat-cli-azure-resource-manager.md

[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-portal]: iot-hub-create-through-portal.md 
