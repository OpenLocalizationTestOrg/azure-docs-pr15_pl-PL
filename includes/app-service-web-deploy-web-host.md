### <a name="app-service-plan"></a>Plan usług aplikacji

Tworzy plan usług do obsługi aplikacji sieci web. Podaj nazwę plan przy użyciu parametru **hostingPlanName** . Lokalizacja planu jest tym samym miejscu na potrzeby grup zasobów. Cennik rozmiar warstwy i pracowników są określane w parametrach **sku** i **workerSize**

    {
      "apiVersion": "2015-08-01",
      "name": "[parameters('hostingPlanName')]",
      "type": "Microsoft.Web/serverfarms",
      "location": "[resourceGroup().location]",
      "sku": {
        "name": "[parameters('sku')]",
        "capacity": "[parameters('workerSize')]"
      },
      "properties": {
        "name": "[parameters('hostingPlanName')]"
      }
    },

