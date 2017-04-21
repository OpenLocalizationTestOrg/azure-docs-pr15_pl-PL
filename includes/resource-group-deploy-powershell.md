## <a name="how-to-deploy-with-powershell"></a>Jak wdrożyć przy użyciu programu PowerShell

1. Zaloguj się do konta Azure.

          Add-AzureAccount

   Po zapewnieniu poświadczenia, polecenie zwraca informacje o Twoim koncie.

          Id                             Type       ...
          --                             ----    
          example@contoso.com            User       ...   

2. Jeśli masz wiele subskrypcji, podaj identyfikator subskrypcji, którego chcesz użyć do wdrożenia. 

          Select-AzureSubscription -SubscriptionID <YourSubscriptionId>

3. Przełączanie do modułu Azure Menedżera zasobów.

          Switch-AzureMode AzureResourceManager

4. Jeśli nie masz istniejącej grupy zasobów, Utwórz nową grupę zasobów. Wprowadź nazwę grupy zasobów i lokalizacji, w której potrzebne do tego rozwiązania.

        New-AzureResourceGroup -Name ExampleResourceGroup -Location "West US"

   Podsumowanie nowej grupy zasobów jest zwracana.

        ResourceGroupName : ExampleResourceGroup
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
                    Actions  NotActions
                    =======  ==========
                    *
        ResourceId        : /subscriptions/######/resourceGroups/ExampleResourceGroup

5. Aby utworzyć nowy wdrożenia grupy zasobów, uruchom polecenie **Nowy AzureResourceGroupDeployment** i podaj wymaganych parametrów. Parametry będzie zawierać nazwę rozmieszczenia, nazwę swojej grupy zasobów, ścieżka lub adres URL, aby utworzony szablon i inne parametry potrzebne do rozwiązania. 
   
   Dostępne są następujące opcje umożliwiające wartości parametrów: 
   
   - Używanie parametrów w tekście.

            New-AzureResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathOrLinkToTemplate> -myParameterName "parameterValue"

   - Za pomocą obiektu parametru.

            $parameters = @{"<ParameterName>"="<Parameter Value>"}
            New-AzureResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathOrLinkToTemplate> -TemplateParameterObject $parameters

   - Za pomocą pliku parametru.

            New-AzureResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathOrLinkToTemplate> -TemplateParameterFile <PathOrLinkToParameterFile>

  Po wdrożeniu grupa zasobów, zobaczysz podsumowanie wdrożenia.

             DeploymentName    : ExampleDeployment
             ResourceGroupName : ExampleResourceGroup
             ProvisioningState : Succeeded
             Timestamp         : 4/14/2015 7:00:27 PM
             Mode              : Incremental
             ...

6. Aby uzyskać informacje na temat wdrażania błędy.

        Get-AzureResourceGroupLog -ResourceGroup ExampleResourceGroup -Status Failed

7. Aby uzyskać szczegółowe informacje na temat wdrażania błędy.

        Get-AzureResourceGroupLog -ResourceGroup ExampleResourceGroup -Status Failed -DetailedOutput
