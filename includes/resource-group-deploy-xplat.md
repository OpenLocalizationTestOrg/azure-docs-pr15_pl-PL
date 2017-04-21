## <a name="how-to-deploy-with-azure-cli"></a>Jak wdrożyć z polecenie Azure

1. Zaloguj się do konta Azure.

        azure login

  Po zapewnieniu poświadczenia, polecenie zwraca wynik nazwę użytkownika.

        ...
        info:    login command OK

2. Jeśli masz wiele subskrypcji, podaj identyfikator subskrypcji, którego chcesz użyć do wdrożenia.

        azure account set <YourSubscriptionNameOrId>

3. Przełączanie do modułu Menedżera zasobów Azure

        azure config mode arm

   Otrzymasz potwierdzenie nowy tryb.

        info:     New mode is arm

4. Jeśli nie masz istniejącej grupy zasobów, należy utworzyć nową grupę zasobów. Wprowadź nazwę grupy zasobów i lokalizacji, w której potrzebne do tego rozwiązania.

        azure group create -n ExampleResourceGroup -l "West US"

   Podsumowanie nowej grupy zasobów jest zwracana.

        info:    Executing command group create
        + Getting resource group ExampleResourceGroup
        + Creating resource group ExampleResourceGroup
        info:    Created resource group ExampleResourceGroup
        data:    Id:                  /subscriptions/####/resourceGroups/ExampleResourceGroup
        data:    Name:                ExampleResourceGroup
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags:
        data:
        info:    group create command OK

5. Aby utworzyć nowy wdrożenia grupy zasobów, uruchom następujące polecenie i podaj wymaganych parametrów. Parametry będzie zawierać nazwę rozmieszczenia, nazwę swojej grupy zasobów, ścieżka lub adres URL, aby utworzony szablon i inne parametry potrzebną do rozwiązania.

   Dostępne są następujące opcje umożliwiające wartości parametrów:

   - Używanie parametrów w tekście i lokalnych szablonu.

             azure group deployment create -f <PathToTemplate> {"ParameterName":"ParameterValue"} -g ExampleResourceGroup -n ExampleDeployment

   - Używanie parametrów w tekście i łącza do szablonu.

             azure group deployment create --template-uri <LinkToTemplate> {"ParameterName":"ParameterValue"} -g ExampleResourceGroup -n ExampleDeployment

   - Za pomocą pliku parametrów.

             azure group deployment create -f <PathToTemplate> -e <PathToParameterFile> -g ExampleResourceGroup -n ExampleDeployment

  Po wdrożeniu grupa zasobów, zobaczysz podsumowanie wdrożenia.

         info:    Executing command group deployment create
         + Initializing template configurations and parameters
         + Creating a deployment
         ...
         info:    group deployment create command OK


6. Aby uzyskać informacje o najnowszych wdrożenia.

         azure group log show -l ExampleResourceGroup

7. Aby uzyskać szczegółowe informacje na temat wdrażania błędy.

         azure group log show -l -v ExampleResourceGroup
