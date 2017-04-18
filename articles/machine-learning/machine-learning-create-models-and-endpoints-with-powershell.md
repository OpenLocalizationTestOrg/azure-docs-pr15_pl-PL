<properties
pageTitle="Tworzenie wielu modeli z doświadczenia | Microsoft Azure"
description="Umożliwia utworzenie wielu modeli nauki komputera i sieci web punkty końcowe usługi z samego algorytmu, ale szkolenie różnych zestawów danych programu PowerShell."
services="machine-learning"
documentationCenter=""
authors="hning86"
manager="jhubbard"
editor="cgronlun"/>

<tags
ms.service="machine-learning"
ms.workload="data-services"
ms.tgt_pltfrm="na"
ms.devlang="na"
ms.topic="article"
ms.date="10/03/2016"
ms.author="garye;haining"/>

# <a name="create-many-machine-learning-models-and-web-service-endpoints-from-one-experiment-using-powershell"></a>Tworzenie wielu modeli nauki komputera i sieci web usługi punkty końcowe z doświadczenia przy użyciu programu PowerShell

Oto typowy problem nauki komputera: chcesz utworzyć wiele modeli, które mają tego samego przepływu pracy szkolenia i za pomocą tego samego algorytmu, ale mają różne szkolenie zestawy danych jako danych wejściowych. W tym artykule pokazano, jak to zrobić w skali w Azure maszynowego uczenia Studio przy użyciu tylko eksperyment jednego.

Załóżmy na przykład, że własności firmy franchisingowe dzierżawa globalnej roweru. Chcesz utworzyć model regresji przewidywanie żądanie dzierżawy, na podstawie danych historycznych. Masz 1000 lokalizacji dzierżawy całym świecie i zgromadzonych zestawu danych dla każdej lokalizacji, która zawiera ważne funkcje, takie jak data, godzina, pogody i ruchu, które są specyficzne dla każdej lokalizacji.

Można przeszkolenie modelu raz przy użyciu wersji scalonych wszystkich zbiorów danych przez wszystkich lokalizacji. Ale ponieważ wszystkich lokalizacji ma unikatowy środowisko, lepszym rozwiązaniem może być przeszkolenie modelu regresji osobno dla każdej lokalizacji przy użyciu zestawu danych. W ten sposób każdego przeszkolony modelu może wziąć pod uwagę sklepu różne rozmiary, głośność, geography, populacji, ruch roweru przyjazne środowisko, *itp.*.

Może to być najlepszym rozwiązaniem, ale nie chcesz, aby utworzyć 1000 doświadczeń szkolenie w Azure maszynowego uczenia z każdego z nich reprezentującą unikatowej lokalizacji. Oprócz jest utrudnione zadania, jest również wydaje się dosyć nieefektywne, ponieważ każdego doświadczenia woli te same składniki z wyjątkiem szkolenie zestawu danych.

Na szczęście możemy można to zrobić przy użyciu [interfejsu API przeszkolenie Azure maszynowego uczenia](machine-learning-retrain-models-programmatically.md) , a Automatyzowanie zadań przy użyciu [Programu PowerShell nauki maszynowego Azure](machine-learning-powershell-module.md).

> [AZURE.NOTE] Aby nasze przykładowe działa szybciej, firma Microsoft będzie Zmniejsz liczbę lokalizacji od 1000 do 10. Jednak samej zasady i procedury dotyczą lokalizacje 1000. Jedyną różnicą jest, aby przeszkolenie od 1000 zestawy danych prawdopodobnie chcesz traktować równocześnie uruchomione następujące skryptów programu PowerShell. Jak to zrobić wykracza poza zakres tego artykułu, ale znajdują się przykłady programu PowerShell wiele wątków w Internecie.  

## <a name="set-up-the-training-experiment"></a>Konfigurowanie doświadczenia szkolenia

Firma Microsoft zacząć używać przykład [eksperymentować szkolenie](https://gallery.cortanaintelligence.com/Experiment/Bike-Rental-Training-Experiment-1) który mamy już utworzony w [Galerii analizy Cortana](http://gallery.cortanaintelligence.com). Otwórz tego doświadczenia w obszarze roboczym [Azure maszynowego uczenia Studio](https://studio.azureml.net) .

>[AZURE.NOTE] Do opracowania oraz w tym przykładzie, można użyć standardowego obszaru roboczego zamiast wolnego obszaru roboczego. Możemy utworzyć jeden punkt końcowy dla każdego z klientów — razem 10 punktów końcowych — i standardowy obszaru roboczego będzie wymagają od czasu wolnego obszaru roboczego jest ograniczona do 3 punktów końcowych. Jeśli masz tylko wolnego obszaru roboczego, po prostu zmodyfikować poniżej, aby umożliwić tylko 3 lokalizacje skryptów.

Doświadczenia używa modułu **Importowania danych** do zaimportowania zestawu danych szkolenia *customer001.csv* z konta Azure miejsca do magazynowania. Załóżmy, możemy zbierane szkolenie zestawy danych z wszystkich lokalizacji dzierżawy roweru i przechowywane ich w tej samej lokalizacji magazyn obiektów blob z zakresu od *rentalloc001.csv* do *rentalloc10.csv*nazwy plików.

![Obraz](./media/machine-learning-create-models-and-endpoints-with-powershell/reader-module.png)

Należy zauważyć, że moduł **Wyjściowych usługi sieci Web** został dodany do modułu **Modelu pociągu** .
Po wdrożeniu tego doświadczenia jako usługi sieci web punkt końcowy skojarzonych z danych wyjściowych może zwrócić modelu przeszkolony w formacie pliku .ilearner.

Należy również zauważyć, że skonfigurować parametr usługi sieci web dla adres URL, który używa modułu **Importowanie danych** . Pozwala użyć parametru, aby określić poszczególnych szkolenie zestawy danych w celu przeszkolenie modelu dla każdej lokalizacji.
Istnieją inne sposoby, które firma Microsoft może zostało to zrobione, takich jak pobieranie danych z bazy danych platformy SQL Azure, za pomocą kwerendy SQL z parametrem usługi sieci web lub po prostu za pomocą modułu **Wprowadzania usługi sieci Web** w celu przekazania w zestawie danych do usługi sieci web.

![Obraz](./media/machine-learning-create-models-and-endpoints-with-powershell/web-service-output.png)

Teraz przejdźmy uruchamianie tego doświadczenia szkoleniowego używanie wartości domyślnej *rental001.csv* jako szkolenie zestawu danych. Jeśli obejrzeć wynik moduł **Szacowanie** (kliknij pozycję dane wyjściowe i wybierz polecenie **Wizualizacja**), może wyświetlać, uzyskując zadowalający wydajności *AUC* = 0,91. W tym momencie jest gotowe do wdrożenia usługi sieci web poza tego doświadczenia szkolenie.

## <a name="deploy-the-training-and-scoring-web-services"></a>Wdrażanie szkolenia i wyników usług sieci web

Aby wdrożyć usługę sieci web szkolenia, kliknij przycisk **Ustaw usługi sieci Web** poniżej kanwy doświadczenia i wybierz **Wdrażanie usługi sieci Web**. Połączenie ta usługa sieci web "" dzierżawa szkolenie roweru".

Teraz należy wdrożyć wyników usługi sieci web.
Aby to zrobić, firma Microsoft może kliknij **Konto usługi sieci Web** poniżej kanwy i wybierz **Przewidywanych usługi sieci Web**. Spowoduje to utworzenie eksperyment wyników.
Będą potrzebne zmiany kilka pomocniczych Zrealizuj swoje jako usługi sieci web, na przykład usuwanie etykiety kolumny "cnt" z danych wejściowych i ograniczania produkcji tylko identyfikator wystąpienia i odpowiadający mu przewidywane wartości.

Aby zapisać sobie tej pracy, możesz po prostu otworzyć [przewidywanych doświadczenia](https://gallery.cortanaintelligence.com/Experiment/Bike-Rental-Predicative-Experiment-1) w galerii, która została już przygotowana.

Aby wdrożyć usługi sieci web, uruchamianie przewidywanych doświadczenia, a następnie kliknij przycisk **Wdrażanie usługi sieci Web** poniżej kanwy. Wyników usługi sieci web "Wyników dzierżawa roweru" Nazwa ".

## <a name="create-10-identical-web-service-endpoints-with-powershell"></a>Tworzenie 10 punktów końcowych usługi identyczne sieci web przy użyciu programu PowerShell

Ta usługa sieci web zawiera domyślny punkt końcowy. Jednak firma Microsoft interesuje nie jako domyślnego punktu końcowego, ponieważ nie można ich aktualizować. Co należy zrobić jest utworzenie 10 dodatkowe punkty końcowe, jedną dla każdej lokalizacji. Firma Microsoft będzie to zrobić przy użyciu programu PowerShell.

Najpierw należy skonfigurować możemy naszego środowiska PowerShell:

    Import-Module .\AzureMLPS.dll
    # Assume the default configuration file exists and is properly set to point to the valid Workspace.
    $scoringSvc = Get-AmlWebService | where Name -eq 'Bike Rental Scoring'
    $trainingSvc = Get-AmlWebService | where Name -eq 'Bike Rental Training'

Następnie uruchom następujące polecenia programu PowerShell:

    # Create 10 endpoints on the scoring web service.
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        Write-Host ('adding endpoint ' + $endpointName + '...')
        Add-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -Description $endpointName     
    }

Teraz możemy utworzono 10 punktów końcowych i wszystkie zawierają ten sam model przeszkolony ćwiczenie *customer001.csv*. Możesz je wyświetlać w portalu zarządzania Azure.

![Obraz](./media/machine-learning-create-models-and-endpoints-with-powershell/created-endpoints.png)

## <a name="update-the-endpoints-to-use-separate-training-datasets-using-powershell"></a>Aktualizowanie punktów końcowych do za pomocą osobnych szkolenie zestawy danych przy użyciu programu PowerShell

Następnym krokiem jest aktualizacja punkty końcowe z modelami jednoznacznie ćwiczenie każdego z klientów indywidualnych danych. Ale najpierw trzeba warzywa tych modeli z usługi sieci web **Szkolenie dzierżawa roweru** . Przyjrzyjmy się do usługi sieci web **Szkolenie dzierżawa roweru** . Trzeba zadzwonić punktu końcowego BES 10 razy z 10 szkolenie różnych zestawów danych w celu utworzenia 10 modeli. W tym celu użyjemy polecenia cmdlet programu PowerShell **InovkeAmlWebServiceBESEndpoint** .

Należy także o podanie poświadczeń konta magazyn obiektów blob do `$configContent`, to znaczy, w polach `AccountName`, `AccountKey` i `RelativeLocation`. `AccountName` Może mieć jedną z nazwy konta w **Portalu zarządzania Azure klasyczny** (karta*miejsca do magazynowania* ). Po kliknięciu przycisku na koncie usługi przechowywania jej `AccountKey` można znaleźć, naciskając przycisk **Zarządzaj klawiszy dostępu** u dołu i kopiowanie *Klucza podstawowego programu Access*. `RelativeLocation` Jest ścieżką względem z magazynu przechowywania nowy model. Na przykład ścieżka `hai/retrain/bike_rental/` skryptu poniżej wskazywany kontenera o nazwie `hai`, i `/retrain/bike_rental/` są podfolderami. Obecnie nie można tworzyć podfolderów za pośrednictwem portalu interfejsu użytkownika, ale istnieje [Kilka eksploratorów miejsca do magazynowania Azure](../storage/storage-explorers.md) można to zrobić. Zalecane jest, utworzyć nowy kontener w Twoim magazynie do przechowywania nowych modeli przeszkolony (pliki .ilearner) w następujący sposób: z Twojej strony miejsca do magazynowania, kliknij przycisk **Dodaj** u dołu i nadaj mu nazwę `retrain`. Podsumowując, niezbędne zmiany w celu poniższego skryptu dotyczą `AccountName`, `AccountKey` i `RelativeLocation` (:`"retrain/model' + $seq + '.ilearner"`).

    # Invoke the retraining API 10 times
    # This is the default (and the only) endpoint on the training web service
    $trainingSvcEp = (Get-AmlWebServiceEndpoint -WebServiceId $trainingSvc.Id)[0];
    $submitJobRequestUrl = $trainingSvcEp.ApiLocation + '/jobs?api-version=2.0';
    $apiKey = $trainingSvcEp.PrimaryKey;
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $inputFileName = 'https://bostonmtc.blob.core.windows.net/hai/retrain/bike_rental/BikeRental' + $seq + '.csv';
        $configContent = '{ "GlobalParameters": { "URI": "' + $inputFileName + '" }, "Outputs": { "output1": { "ConnectionString": "DefaultEndpointsProtocol=https;AccountName=<myaccount>;AccountKey=<mykey>", "RelativeLocation": "hai/retrain/bike_rental/model' + $seq + '.ilearner" } } }';
        Write-Host ('training regression model on ' + $inputFileName + ' for rental location ' + $seq + '...');
        Invoke-AmlWebServiceBESEndpoint -JobConfigString $configContent -SubmitJobRequestUrl $submitJobRequestUrl -ApiKey $apiKey
    }

>[AZURE.NOTE] Punkt końcowy BES jest tryb obsługiwane tylko dla tej operacji. Rekordy zasobów nie można używać do produkcji przeszkolony modeli.

Jak pokazano powyżej, zamiast tworzenia 10 różnych BES zadania konfiguracji json plików, możemy dynamicznie zamiast tego utworzyć parametry konfiguracji i kanał do parametru *jobConfigString* **InvokeAmlWebServceBESEndpoint** polecenia cmdlet, ponieważ jest naprawdę bez konieczności zachowywania kopii na dysku.

Jeśli wszystko odbywa się również, po jakimś czasie powinna być widoczna 10 plików .ilearner, od *model001.ilearner* do *model010.ilearner*na Twoim koncie Azure miejsca do magazynowania. Teraz możemy już zaktualizować naszych 10 wyników sieci web usługi punkty końcowe z tych modeli przy użyciu polecenia cmdlet programu PowerShell **AmlWebServiceEndpoint poprawki** . Ponownie należy pamiętać, że firma Microsoft tylko poprawki inne niż domyślne punkty końcowe utworzonego programowy wcześniej.

    # Patch the 10 endpoints with respective .ilearner models
    $baseLoc = 'http://bostonmtc.blob.core.windows.net/'
    $sasToken = '<my_blob_sas_token>'
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        $relativeLoc = 'hai/retrain/bike_rental/model' + $seq + '.ilearner';
        Write-Host ('Patching endpoint ' + $endpointName + '...');
        Patch-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -ResourceName 'Bike Rental [trained model]' -BaseLocation $baseLoc -RelativeLocation $relativeLoc -SasBlobToken $sasToken
    }

To powinna działać dość szybko. Po zakończeniu wykonywania zostanie pomyślnie utworzono 10 przewidywanych sieci web usługi punkty końcowe, zawierające modelu przeszkolony jednoznacznie ćwiczenie określonego zestawu danych w lokalizacji dzierżawy, wszystkie z eksperyment jednego szkolenie. Aby to sprawdzić, możesz spróbować nawiązywania połączeń z te punkty końcowe przy użyciu polecenia cmdlet **InvokeAmlWebServiceRRSEndpoint** dostarczanie im tych samych danych wejściowych, a spodziewać wyświetlić wyniki różnych przewidywania, ponieważ modele są szkolenia Szkolenie różnych zestawów.

## <a name="full-powershell-script"></a>Pełna skrypt programu PowerShell

Oto lista kodu źródłowego pełny:

    Import-Module .\AzureMLPS.dll
    # Assume the default configuration file exists and properly set to point to the valid workspace.
    $scoringSvc = Get-AmlWebService | where Name -eq 'Bike Rental Scoring'
    $trainingSvc = Get-AmlWebService | where Name -eq 'Bike Rental Training'

    # Create 10 endpoints on the scoring web service
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        Write-Host ('adding endpoint ' + $endpontName + '...')
        Add-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -Description $endpointName     
    }

    # Invoke the retraining API 10 times to produce 10 regression models in .ilearner format
    $trainingSvcEp = (Get-AmlWebServiceEndpoint -WebServiceId $trainingSvc.Id)[0];
    $submitJobRequestUrl = $trainingSvcEp.ApiLocation + '/jobs?api-version=2.0';
    $apiKey = $trainingSvcEp.PrimaryKey;
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $inputFileName = 'https://bostonmtc.blob.core.windows.net/hai/retrain/bike_rental/BikeRental' + $seq + '.csv';
        $configContent = '{ "GlobalParameters": { "URI": "' + $inputFileName + '" }, "Outputs": { "output1": { "ConnectionString": "DefaultEndpointsProtocol=https;AccountName=<myaccount>;AccountKey=<mykey>", "RelativeLocation": "hai/retrain/bike_rental/model' + $seq + '.ilearner" } } }';
        Write-Host ('training regression model on ' + $inputFileName + ' for rental location ' + $seq + '...');
        Invoke-AmlWebServiceBESEndpoint -JobConfigString $configContent -SubmitJobRequestUrl $submitJobRequestUrl -ApiKey $apiKey
    }

    # Patch the 10 endpoints with respective .ilearner models
    $baseLoc = 'http://bostonmtc.blob.core.windows.net/'
    $sasToken = '?test'
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        $relativeLoc = 'hai/retrain/bike_rental/model' + $seq + '.ilearner';
        Write-Host ('Patching endpoint ' + $endpointName + '...');
        Patch-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -ResourceName 'Bike Rental [trained model]' -BaseLocation $baseLoc -RelativeLocation $relativeLoc -SasBlobToken $sasToken
    }
