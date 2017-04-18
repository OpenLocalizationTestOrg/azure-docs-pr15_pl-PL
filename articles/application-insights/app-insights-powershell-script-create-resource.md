<properties 
    pageTitle="Skrypt programu PowerShell w celu utworzenia wniosków aplikacji zasobów" 
    description="Zautomatyzować tworzenie wniosków aplikacji zasobów." 
    services="application-insights" 
    documentationCenter="windows"
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="02/19/2016" 
    ms.author="awills"/>

#  <a name="powershell-script-to-create-an-application-insights-resource"></a>Skrypt programu PowerShell w celu utworzenia wniosków aplikacji zasobów

*Wnioski aplikacji jest w podglądzie.*

Gdy chcesz monitorować nowej aplikacji - lub nowej wersji aplikacji — przy użyciu [Programu Visual Studio aplikacji wniosków](https://azure.microsoft.com/services/application-insights/), możesz skonfigurować nowego zasobu w programie Microsoft Azure. Zasób jest miejsce, w którym analizowane i wyświetlania danych telemetrycznych z Twojej aplikacji. 

Tworzenie nowego zasobu można zautomatyzować przy użyciu programu PowerShell.

Na przykład opracowywania aplikacji dla urządzeń przenośnych, istnieje prawdopodobieństwo, że, w dowolnej chwili będzie kilka opublikowane wersje aplikacji używany przez klientów. Nie chcesz otrzymywać wyniki telemetrycznego z różnych wersji zamienione. Dlatego możesz uzyskać procesu Konstruuj, aby utworzyć nowy zasób dla każdego kompilacji.

## <a name="script-to-create-an-application-insights-resource"></a>Skrypt w celu utworzenia wniosków aplikacji zasobów

Specyfikacje odpowiednie polecenia cmdlet, zobacz:

* [Nowy AzureRmResource](https://msdn.microsoft.com/library/mt652510.aspx)
* [Nowy AzureRmRoleAssignment](https://msdn.microsoft.com/library/mt678995.aspx)


*Skrypt programu PowerShell*  

```PowerShell


###########################################
# Set Values
###########################################

# If running manually, uncomment before the first 
# execution to login to the Azure Portal:

# Add-AzureRmAccount

# Set the name of the Application Insights Resource

$appInsightsName = "TestApp"

# Set the application name used for the value of the Tag "AppInsightsApp" 
# - http://azure.microsoft.com/documentation/articles/azure-preview-portal-using-tags/
$applicationTagName = "MyApp"

# Set the name of the Resource Group to use.  
# Default is the application name.
$resourceGroupName = "MyAppResourceGroup"

###################################################
# Create the Resource and Output the name and iKey
###################################################

#Select the azure subscription
Select-AzureSubscription -SubscriptionName "MySubscription"

# Create the App Insights Resource

$resource = New-AzureRmResource `
  -ResourceName $appInsightsName `
  -ResourceGroupName $resourceGroupName `
  -Tag @{ Name = "AppInsightsApp"; Value = $applicationTagName} `
  -ResourceType "Microsoft.Insights/Components" `
  -Location "Central US" `
  -PropertyObject @{"Type"="ASP.NET"} `
  -Force

# Give owner access to the team

New-AzureRmRoleAssignment `
  -SignInName "myteam@fabrikam.com" `
  -RoleDefinitionName Owner `
  -Scope $resource.ResourceId 


#Display iKey
Write-Host "App Insights Name = " $resource.Name
Write-Host "IKey = " $resource.Properties.InstrumentationKey

```

## <a name="what-to-do-with-the-ikey"></a>Co zrobić z iKey

Każdemu zasobowi jest identyfikowany za pomocą klucza oprzyrządowania (iKey). IKey jest wyjściowe skrypt tworzenia zasobów. Skrypt kompilacji powinien zawierać, że iKey do zestawu SDK wniosków aplikacji osadzony w aplikacji.

Istnieją dwa sposoby, aby udostępnić iKey zestawu SDK:
  
* W [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md): 
 * `<instrumentationkey>`*ikey*`</instrumentationkey>`
* I w [kodzie inicjowanie](app-insights-api-custom-events-metrics.md): 
 * `Microsoft.ApplicationInsights.Extensibility.
    TelemetryConfiguration.Active.InstrumentationKey = "`*iKey*`";`



## <a name="see-also"></a>Zobacz też

* [Tworzenie wniosków aplikacji i zasobów test sieci web przy użyciu szablonów](app-insights-powershell.md)
* [Konfigurowanie monitorowania Azure diagnostyki przy użyciu programu PowerShell](app-insights-powershell-azure-diagnostics.md) 
* [Ustawianie alertów przy użyciu programu PowerShell](app-insights-powershell-alerts.md)

 