<properties
    pageTitle="Niestandardowe ustawienia dla środowiska usługi aplikacji"
    description="Niestandardowych ustawień konfiguracji dla środowiska usługi aplikacji"
    services="app-service"
    documentationCenter=""
    authors="stefsch"
    manager="nirma"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="stefsch"/>

# <a name="custom-configuration-settings-for-app-service-environments"></a>Niestandardowych ustawień konfiguracji dla środowiska usługi aplikacji

## <a name="overview"></a>Omówienie ##
Ponieważ środowiska usługi aplikacji znajdują się odizolowane jednego odbiorcy, istnieją pewne ustawienia konfiguracji, które mogą być stosowane wyłącznie do środowiska usługi aplikacji. W tym artykule opisano różne specjalnych dostosowań dostępnych dla środowiska usługi aplikacji.

Jeśli nie masz środowisku usługi aplikacji, zobacz [jak utworzyć środowisku usługi aplikacji](app-service-web-how-to-create-an-app-service-environment.md).

Mogą zawierać dostosowania środowiska usługi aplikacji przy użyciu tablicy w nowy atrybut **clusterSettings** . Ten atrybut znajduje się w słowniku "Właściwości" *hostingEnvironments* jednostki Azure Menedżera zasobów.

Poniższy fragment szablonu skróconą Menedżera zasobów zawiera atrybut **clusterSettings** :


    "resources": [
    {
       "apiVersion": "2015-08-01",
       "type": "Microsoft.Web/hostingEnvironments",
       "name": ...,
       "location": ...,
       "properties": {
          "clusterSettings": [
             {
                 "name": "nameOfCustomSetting",
                 "value": "valueOfCustomSetting"
             }
          ],
          "workerPools": [ ...],
          etc...
       }
    }

Atrybut **clusterSettings** może być dołączone do szablonu Menedżera zasobów, aby zaktualizować środowisko usługi aplikacji.

## <a name="use-azure-resource-explorer-to-update-an-app-service-environment"></a>Aktualizowanie środowisku usługi aplikacji za pomocą Eksploratora zasobów Azure
Alternatywnie możesz zaktualizować środowisko usługi aplikacji przy użyciu [Eksploratora zasobów Azure](https://resources.azure.com).  

1. W Eksploratorze zasobów, przejdź do węzła w środowisku usługi aplikacji (**Subskrypcje** > **resourceGroups** > **dostawców** > **Micrososft.Web** > **hostingEnvironments**). Kliknij pozycję określonego środowiska usługi aplikacji, którą chcesz zaktualizować.

2. W okienku po prawej stronie kliknij **Odczytu/zapisu** na pasku górnym umożliwia interakcyjne edycji w Eksploratorze zasobów.  

3. Kliknij niebieski **Edytowanie** przycisk, aby utworzyć szablon Menedżera zasobów można edytować.

4. Przewiń w dół w okienku po prawej stronie. Atrybut **clusterSettings** jest na samym dole, gdzie można wprowadzić lub aktualizowanie jej wartość.

5. Wpisz (lub kopiowanie i wklejanie) tablicę wartości konfiguracji, które chcesz atrybutu **clusterSettings** .  

6. Kliknij zielony przycisk **umieszczenie** , który ma znajduje się u góry prawego okienka, aby zatwierdzić zmiany środowisko usługi aplikacji.

Jednak przesłać zmiany, trwa około 30 minut pomnożoną przez liczbę przedniej kończy się w środowisko usługi aplikacji, aby zmiana została uwzględniona.
Na przykład jeśli środowisku aplikacji usługi zawiera cztery przedniej zostanie zakończone, potrwa około dwie godziny do zakończenia aktualizacji konfiguracji. Podczas zmiany konfiguracji jest rozwijane, nie skalowania operacje lub operacje zmiany konfiguracji mogą mieć miejsce w środowisku usługi aplikacji.

## <a name="disable-tls-10"></a>Wyłączanie TLS 1.0 ##
Cykliczne pytania klientów, szczególnie w przypadku klientów, którzy są dotyczących zgodności PCI inspekcji, jest jawnie wyłączania TLS 1.0 dla swoich aplikacji.

TLS 1.0 może zostać wyłączona przez następujący wpis **clusterSettings** :

        "clusterSettings": [
            {
                "name": "DisableTls1.0",
                "value": "1"
            }
        ],

## <a name="change-tls-cipher-suite-order"></a>Zmienianie TLS szyfrowania pakiet kolejności ##
Następne pytanie od odbiorców jest Jeśli można zmienić na liście szyfrowania wynegocjowane przez ich serwera i można to osiągnąć, modyfikując **clusterSettings** , tak jak pokazano poniżej. Na liście dostępnych pakietów szyfrowania można pobrać z [w tym artykule w witrynie MSDN] (https://msdn.microsoft.com/library/windows/desktop/aa374757(v=vs.85\).aspx).

        "clusterSettings": [
            {
                "name": "FrontEndSSLCipherSuiteOrder",
                "value": "TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384_P256,TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256_P256,TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384_P256,TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256_P256,TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA_P256,TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA_P256"
            }
        ],

> [AZURE.WARNING]  Jeśli pakietu szyfrowania, która SChannel nie może rozpoznać niepoprawnymi wartościami, całą komunikację TLS do swojego serwera może przestać działać. W takim przypadku należy usunąć wpis *FrontEndSSLCipherSuiteOrder* z **clusterSettings** i przesyłanie uaktualnionego szablonu Menedżera zasobów, aby przywrócić domyślne ustawienia pakietu szyfrowania.  Użyj tej funkcji z rozwagą.

## <a name="get-started"></a>Rozpoczynanie pracy
Witryna szablonu Menedżera zasobów Azure Szybki Start zawiera szablon z definicją podstawowej do [tworzenia w środowisku usługi aplikacji](https://azure.microsoft.com/documentation/templates/201-web-app-ase-create/).


<!-- LINKS -->

<!-- IMAGES -->
