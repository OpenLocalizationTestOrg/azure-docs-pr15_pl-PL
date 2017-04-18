<properties
 pageTitle="Tworzenie Twoim Centrum IoT i zarejestrować usługi malina Pi 3 | Microsoft Azure"
 description="Tworzenie grupy zasobów, tworzenie Centrum Azure IoT i zarejestrować do Pi w Centrum Azure IoT za pomocą interfejsu wiersza polecenia Azure."
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

# <a name="22-create-your-iot-hub-and-register-your-raspberry-pi-3"></a>2.2 utworzyć Twoim Centrum IoT i zarejestrować usługi malina Pi 3

## <a name="221-what-you-will-do"></a>2.2.1 co wykonasz

- Tworzenie grupy zasobów.
- Tworzenie Twoim Centrum Azure IoT w grupie zasobów.
- Dodawanie usługi malina Pi 3 do Centrum Azure IoT za pomocą interfejsu wiersza polecenia Azure.

Dodawanie do Pi do Twoim Centrum IoT za pomocą interfejsu wiersza polecenia Azure usługę generuje klucz dla swojego Pi do uwierzytelnienia z usługą. Jeśli są spełnione problemów szukanie rozwiązań [Rozwiązywanie problemów z strony](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="222-what-you-will-learn"></a>2.2.2 co dowiesz się

- Jak używać Azure interfejsu wiersza polecenia do tworzenia Centrum IoT Azure.
- Jak utworzyć urządzenia tożsamości dla swojego Pi w Twoim Centrum IoT Azure.

## <a name="223-what-you-need"></a>2.2.3 co jest potrzebne

- Konto Azure
- Mac lub komputerze z systemem Windows z polecenie Azure zainstalowany

## <a name="224-create-your-azure-iot-hub"></a>2.2.4 tworzenie Twoim Centrum Azure IoT

Azure Centrum IoT ułatwia łączenie, monitorowanie i zarządzanie miliony IoT aktywów. Aby utworzyć Twoim Centrum Azure IoT, wykonaj następujące kroki:

1. Zaloguj się do konta Azure, uruchamiając następujące polecenie:

    ```bash
    az login
    ```

    Wszystkie dostępne subskrypcje Azure znajdują się po pomyślnym logowania.

2. Ustawianie domyślnego Azure subskrypcję, do której chcesz użyć, uruchamiając następujące polecenie:

    ```bash
    az account set -n {subscription id or name}
    ```

    Identyfikator subskrypcji lub nazwę znajdują się w danych wyjściowych `az login`.

3. Rejestracja dostawcy, uruchamiając następujące polecenie:

    ```bash
    az resource provider register -n "Microsoft.Devices"
    ```

    Przed wdrożeniem Azure zasobu, która zapewnia dostawca, musisz zarejestrować dostawcy.

    > [AZURE.NOTE] Większość dostawców są rejestrowane automatycznie przez Azure portal lub polecenie Azure używasz, ale nie wszystkie. Aby uzyskać więcej informacji na temat dostawcy zobacz [Rozwiązywanie problemów z typowych błędów Azure wdrażania przy użyciu Menedżera zasobów Azure](../resource-manager-common-deployment-errors.md)

4. Tworzenie grupy zasobów o nazwie iot próbkami w regionie Zachód Stanów Zjednoczonych, uruchamiając następujące polecenie:

    ```bash
    az resource group create --name iot-sample --location westus
    ```

5. Tworzenie Centrum IoT w grupie zasobów iot próbki, uruchamiając następujące polecenie:

    ```bash
    az iot hub create --name {my hub name} --resource-group iot-sample
    ```

    Wydania domyślne Centrum IoT, które można tworzyć jest **F1**, która jest **bezpłatne**. Aby uzyskać więcej informacji zobacz [Centrum IoT Azure ceny](https://azure.microsoft.com/pricing/details/iot-hub/).

> [AZURE.NOTE] Nazwa Twojej Centrum IoT musi być globalnie unikatowa.
>
> Możesz utworzyć tylko jeden wydania **F1** Azure IoT Centrum w obszarze Azure subskrypcji.

## <a name="225-register-your-pi-in-your-iot-hub"></a>2.2.5 zarejestrować do Pi w Twoim Centrum IoT

Każde urządzenie, którego wysyła i odbiera wiadomości z Twoim Centrum Azure IoT musi być zarejestrowana w unikatowy identyfikator.

Rejestrowanie do Pi w Twoim Centrum Azure IoT przez uruchomione następujące polecenie:

```bash
az iot device create --device-id myraspberrypi --hub {my hub name} --resource-group iot-sample
```

## <a name="226-summary"></a>2.2.6 podsumowanie

Utworzeniu koncentratora Azure IoT i zarejestrowane do Pi z urządzenia tożsamości w Twoim Centrum Azure IoT. Możesz przejść do następnej lekcji Aby dowiedzieć się, jak wysyłać wiadomości z usługi Pi na Twoim Centrum IoT.

## <a name="next-steps"></a>Następne kroki

Możesz teraz rozpocząć lekcji 3, który zaczyna się [Tworzenie aplikacji funkcji Azure i konta magazyn Azure w celu przetwarzania i przechowywania IoT Centrum wiadomości](iot-hub-raspberry-pi-kit-node-lesson3-deploy-resource-manager-template.md).