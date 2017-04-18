<properties
    pageTitle="Modułu programu PowerShell szkoleniowe na komputerze | Microsoft Azure"
    description="Modułu programu PowerShell Azure szkoleniowe na komputerze jest dostępna w trybie Podgląd publicznej. Tworzenie i zarządzanie obszarów roboczych, doświadczeń, usług sieci web i przy użyciu programu PowerShell."
    keywords="Wypróbuj regresji liniowej, maszynowego uczenia algorytmów, maszynowego uczenia samouczek technik modelowania przewidywanych, doświadczenia nauki danych"
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
    ms.date="08/05/2016"
    ms.author="garye;haining"/>

# <a name="powershell-module-for-microsoft-azure-machine-learning"></a>Modułu programu PowerShell usługi Microsoft Azure maszynowego szkoleniowe

Moduł programu PowerShell dla Azure maszynowego uczenia jest zaawansowane narzędzia, które umożliwia zarządzanie obszarami roboczymi, doświadczeń, zestawy danych i serivces sieci web przy użyciu programu Windows PowerShell.

Można Przejrzyj dokumentację dotyczącą i pobrać moduł wraz z kodu źródłowego pełny, u [https://aka.ms/amlps](https://aka.ms/amlps). 

## <a name="what-is-the-machine-learning-powershell-module"></a>Co to jest moduł programu PowerShell nauki komputerze?

Moduł programu PowerShell nauki komputera. Oparte na sieci DLL modułu, który pozwala w pełni Zarządzanie Azure maszynowego uczenia obszarów roboczych, doświadczeń zestawy danych, usługi sieci web i punkty końcowe usługi sieci web z programu Windows PowerShell. Wraz z modułu możesz pobrać kodu źródłowego pełny, zawierającego testu oddzielone [warstwy interfejsu API języka C#](https://github.com/hning86/azuremlps/blob/master/code/AzureMLSDK.cs). Oznacza to, może odwoływać się do tej biblioteki z projektu .NET i zarządzać nauki maszynowego Azure za pomocą kodu .NET. Ponadto bibliotekę DLL zależy od źródłowych interfejsów API pozostałych, które można wykorzystać bezpośrednio z ulubionych klienta.

## <a name="what-can-i-do-with-the-powershell-module"></a>Co można zrobić z modułu programu PowerShell?

Poniżej przedstawiono kilka zadań, które można wykonywać za pomocą tego modułu programu PowerShell. Zapoznaj się z [pełną dokumentację](https://aka.ms/amlps) i wiele więcej funkcji.

- Inicjowanie obsługi nowego obszaru roboczego przy użyciu certyfikatu zarządzania ([Nowy AmlWorkspace](https://github.com/hning86/azuremlps#new-amlworkspace))
- Eksportowanie i importowanie pliku JSON reprezentującą wykresu doświadczenia ([AmlExperimentGraph eksportu](https://github.com/hning86/azuremlps#export-amlexperimentgraph) i [Importu AmlExperimentGraph](https://github.com/hning86/azuremlps#import-amlexperimentgraph))
- Uruchamianie doświadczenia ([Start AmlExperiment](https://github.com/hning86/azuremlps#start-amlexperiment))
- Tworzenie usługi sieci web poza eksperyment przewidywanych ([Nowy AmlWebService](https://github.com/hning86/azuremlps#new-amlwebservice))
- Utworzyć nowy punkt końcowy na usługi sieci web opublikowanych ([Dodaj AmlWebServiceEndpoint](https://github.com/hning86/azuremlps#add-amlwebserviceendpoint))
- Wywołaj rekordy zasobów i/lub BES web punkt końcowy usługi ([Wywołaj AmlWebServiceRRSEndpoint](https://github.com/hning86/azuremlps#invoke-amlwebservicerrsendpoint) i [Wywołaj AmlWebServicBESEndpoint](https://github.com/hning86/azuremlps#invoke-amlwebservicebesendpoint))

Oto przykład Szybkie uruchamianie istniejących doświadczenia przy użyciu programu PowerShell:

        #Find the first Experiment named “xyz”
        $exp = (Get-AmlExperiment | where Description -eq ‘xyz’)[0]
        #Run the Experiment
        Start-AmlExperiment -ExperimentId $exp.ExperimentId 

Aby uzyskać więcej informacji na temat przypadków użycia, zobacz ten artykuł dotyczący używaniu modułu programu PowerShell, aby zautomatyzować bardzo często wymagane zadanie: [Tworzenie wielu modeli nauki komputera i sieci web usługi punkty końcowe z doświadczenia przy użyciu programu PowerShell](machine-learning-create-models-and-endpoints-with-powershell.md).

## <a name="how-do-i-get-started"></a>Jak rozpocząć?

Aby rozpocząć pracę z programem PowerShell nauki komputer, Pobierz [pakiet Zwolnij](https://github.com/hning86/azuremlps/releases) z GitHub i postępuj zgodnie z [instrukcjami dotyczącymi instalacji](https://github.com/hning86/azuremlps/blob/master/README.md). Musisz odblokować pobierane rozpakowane DLL, a następnie zaimportować je do środowiska PowerShell. Większość poleceń cmdlet wymagają podania identyfikator obszaru roboczego, token autoryzacji obszaru roboczego i Azure region, który znajduje się w obszarze roboczym. Najłatwiejszym sposobem udostępnienia tych jest za pośrednictwem pliku config.json domyślny, który jest objęta omówione w z instrukcjami instalacji. Oczywiście można również klonowanie drzewa cyfra i modyfikowanie i kompilowania kodu lokalnie, przy użyciu programu Visual Studio.

## <a name="next-steps"></a>Następne kroki

Moduł programu PowerShell będzie ulepszona i rozwinięta w tym okresie Podgląd. Kontroli na [analizy Cortana i blogu nauki komputera](https://blogs.technet.microsoft.com/machinelearning/) dla więcej wiadomości i informacje.
