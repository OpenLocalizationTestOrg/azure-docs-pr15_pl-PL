<properties 
    pageTitle="Aplikacje logiczny ceny modelu | Microsoft Azure" 
    description="Szczegółowe informacje o sposobie działania ceny w aplikacjach warunków logicznych" 
    authors="kevinlam1" 
    manager="dwrede" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article" 
    ms.date="10/12/2016"
    ms.author="klam"/>

# <a name="logic-apps-pricing-model"></a>Aplikacje logiczny ceny modelu

Aplikacje logika Azure umożliwia skalowanie, a następnie wykonaj integracji przepływów pracy w chmurze.  Poniżej przedstawiono szczegółowe informacje o rozliczenie i ceny plany dla aplikacji logika.

## <a name="consumption-pricing"></a>Cennik zużycie

Nowo utworzona logiki aplikacji za pomocą planu zużycie. Z modelu cennik zużycie logiki aplikacji tylko opłatami, jakich można użyć.  Logika aplikacje nie są ograniczenie podczas korzystania z planu zużycie.
Taryfowe są wszystkie akcje wykonywane w uruchamiania wystąpień aplikacji logicznych.

### <a name="what-are-action-executions"></a>Co to są wykonania akcji?

Każdy krok w definicji aplikacji logika jest akcji.  Ta opcja uwzględnia wyzwalacze, etapy przepływu sterowania jak warunki, zakresy, dla każdego sprzężeń do pętli, połączenia do łączników i połączeń do natywnej akcji.
Wyzwalacze są tylko ze specjalnymi akcje, które mają na celu wystąpienia nowego wystąpienia aplikacji logika po wystąpieniu określonego zdarzenia.  Istnieje wiele różnych działań wyzwalaczy, które mogą mieć wpływ na sposób taryfowe aplikacji logika.

-   **Sondowanie wyzwalacza** — ten wyzwalacz ciągłe sprawdza punktu końcowego, dopóki odbierze wiadomość, która spełnia kryteria dotyczące tworzenia nowego wystąpienia aplikacji logika.  Można skonfigurować wyzwalacza w Projektancie aplikacji logika częstotliwości.  Każdego żądania ankieta nawet wtedy, gdy nie zostanie utworzona nowego wystąpienia aplikacji logiczny, będzie liczy jako wykonywanie akcji.

-   **Wyzwalacz Webhook** — ten wyzwalacz oczekuje klient przesyła żądanie dla określonego punktu końcowego.  Każdego żądania wysłany do punktu końcowego webhook liczy jako wykonywanie akcji. Żądanie i wyzwalacz HTTP Webhook są oba wyzwalaczy webhook.

-   **Cykl wyzwalacza** — ten wyzwalacz spowoduje utworzenie nowego wystąpienia aplikacji logiczny na podstawie interwału cyklu skonfigurowane w wyzwalacz.  Na przykład wyzwalacz cyklu można skonfigurować do uruchomienia co 3 dni lub nawet co minutę.

Wykonania wyzwalacza są widoczne w aplikacji logika karta zasobu w obszarze Historia wyzwalacza.

Wszystkie akcje, które zostały wykonane, czy zostały pomyślnie lub nie powiodło się są taryfowe jako wykonywanie akcji.  Akcje, które zostały pominięte z powodu warunku nie są spełnione lub akcje, które nie wykonać, ponieważ aplikacji logika zakończone przed zakończeniem nie są liczone jako wykonania akcji.

Akcje wykonywane w pętli są liczone na iteracji pętli.  Na przykład pojedynczy akcji w dla każdego pętli iteracji listy elementów 10 będą zliczane jako liczbę elementów na liście (10), pomnożoną przez liczbę akcji w pętli (1) plus jeden do rozpoczęcia pętli, który w tym przykładzie będzie (10 * 1) + 1 = 11 wykonania akcji.

Logika aplikacje, które są wyłączone nie mogą mieć nowych wystąpień utworzonych i w związku z tym w czasie, które są one wyłączone będzie nie uzyskać jest naliczany.  Należy zachować ostrożność, po wyłączeniu aplikacji logiczny może zająć trochę czasu wystąpień do przełączanie w stan spoczynku, zanim całkowicie wyłączone.

## <a name="app-service-plans"></a>Plany aplikacji usługi

Plany usługi aplikacji nie jest już powinni utworzyć aplikację logicznych.  Można także odwoływać się Planowanie aplikacji usługi z istniejącej aplikacji logicznych.  Aplikacje logiczny poprzednio utworzone za pomocą aplikacji Plan usług będzie nadal działają jak przed miejsce, w którym, w zależności od planu wybranej będzie uzyskiwanie ograniczenie po liczbę dzienny wykonania w przypadku przekroczenia i nie będzie zostanie wystawiona przy użyciu miernik wykonanie akcji.

Plany usługi aplikacji i ich dzienny wykonania akcji dozwolonych:

| |Wolny/udostępnione i Basic|Standardowe|Premium|
|---|---|---|---|
|Wykonania akcji na dzień| 200|10 000|50 000|

### <a name="convert-from-consumption-to-app-service-plan-pricing"></a>Konwertowanie zużycie na planowanie usługi aplikacji ceny

Aby odwołać się aplikacji usługi Planowanie zużycie logiki aplikacji, możesz po prostu [uruchomić poniżej skrypt programu PowerShell](https://github.com/logicappsio/ConsumptionToAppServicePlan).  Upewnij się, że masz najpierw zainstalowano [Narzędzia Azure programu PowerShell](https://github.com/Azure/azure-powershell) .

``` powershell
Param(
    [string] $AppService_RG = '<app-service-resource-group>',
    [string] $AppService_Name = '<app-service-name>',
    [string] $LogicApp_RG = '<logic-app-resource-group>',
    [string] $LogicApp_Name = '<logic-app-name>',
    [string] $subscriptionId = '<azure-subscription-id>'
)

Login-AzureRmAccount 
$subscription = Get-AzureRmSubscription -SubscriptionId $subscriptionId
$appserviceplan = Get-AzureRmResource -ResourceType "Microsoft.Web/serverFarms" -ResourceGroupName $AppService_RG -ResourceName $AppService_Name
$logicapp = Get-AzureRmResource -ResourceType "Microsoft.Logic/workflows" -ResourceGroupName $LogicApp_RG -ResourceName $LogicApp_Name

$sku = @{
    "name" = $appservicePlan.Sku.tier;
    "plan" = @{
      "id" = $appserviceplan.ResourceId;
      "type" = "Microsoft.Web/ServerFarms";
      "name" = $appserviceplan.Name  
    }
}

$updatedProperties = $logicapp.Properties | Add-Member @{sku = $sku;} -PassThru

$updatedLA = Set-AzureRmResource -ResourceId $logicapp.ResourceId -Properties $updatedProperties -ApiVersion 2015-08-01-preview
```

### <a name="convert-from-app-service-plan-pricing-to-consumption"></a>Konwersja z aplikacji usługi Planowanie ceny zużycia

Aby zmienić aplikację logika, która zawiera aplikację usługi Plan skojarzony z modelem zużycie Usuń odwołanie do planu usługi aplikacji w definicji aplikacji logika.  Można to zrobić przy użyciu połączenia do polecenia cmdlet programu PowerShell:

`Set-AzureRmLogicApp -ResourceGroupName ‘rgname’ -Name ‘wfname’ –UseConsumptionModel -Force`

## <a name="pricing"></a>Cennik

Cennik szczegóły można znaleźć [Ceny aplikacji logicznych](https://azure.microsoft.com/pricing/details/logic-apps/).

## <a name="next-steps"></a>Następne kroki

- [Omówienie aplikacji warunków logicznych][whatis]
- [Tworzenie pierwszej aplikacji warunków logicznych][create]

[pricing]: https://azure.microsoft.com/pricing/details/logic-apps/
[whatis]: app-service-logic-what-are-logic-apps.md
[create]: app-service-logic-create-a-logic-app.md

