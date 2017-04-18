Przy użyciu Menedżera zasobów Azure Definiowanie parametrów wartości, które chcesz określić po wdrożeniu szablonu. Szablon zawiera sekcję o nazwie parametry, która zawiera wszystkie wartości parametru.
Należy zdefiniować parametr te wartości, które różnią się na podstawie projektu, który instalujesz lub na podstawie środowiska, który instalujesz do. Nie Definiowanie parametrów do wartości, które zawsze pozostanie takie same. Każda wartość parametru jest używana w szablonie Aby zdefiniować zasoby, które są rozmieszczane. 

Podczas definiowania parametrów, należy użyć pola **allowedValues** , aby określić wartości, które umożliwiają użytkownika podczas wdrażania. Aby przypisać wartość parametru należy użyć pola **wartość domyślna** , jeśli wartość nie jest dostępna podczas wdrażania.

Firma Microsoft opisując każdego parametru w szablonie.

### <a name="sitename"></a>Nazwa witryny

Nazwa aplikacji sieci web, którą chcesz utworzyć.

    "siteName":{
      "type":"string"
    }

### <a name="hostingplanname"></a>hostingPlanName

Nazwa aplikacji usługi będzie używany do obsługi aplikacji sieci web.
    
    "hostingPlanName":{
      "type":"string"
    }

### <a name="sku"></a>Jednostka SKU

Cennik warstwa hostingu planu.

    "sku": {
      "type": "string",
      "allowedValues": [
        "F1",
        "D1",
        "B1",
        "B2",
        "B3",
        "S1",
        "S2",
        "S3",
        "P1",
        "P2",
        "P3",
        "P4"
      ],
      "defaultValue": "S1",
      "metadata": {
        "description": "The pricing tier for the hosting plan."
      }
    }

Szablon określa wartości, które są dozwolone dla tego parametru i powoduje przypisanie wartości domyślnej (S1), jeśli nie określono wartości.

### <a name="workersize"></a>workerSize

Rozmiar wystąpienie hostingu planu (małe, średnich i dużych).

    "workerSize":{
      "type":"string",
      "allowedValues":[
        "0",
        "1",
        "2"
      ],
      "defaultValue":"0"
    }
    
Szablon określa wartości, które są dozwolone dla tego parametru (0, 1 lub 2), a powoduje przypisanie wartości domyślnej (0), jeśli nie określono wartości. Wartości odpowiadają małe, średnich i dużych.
