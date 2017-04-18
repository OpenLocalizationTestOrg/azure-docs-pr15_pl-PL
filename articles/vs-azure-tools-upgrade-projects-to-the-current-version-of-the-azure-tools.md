<properties
   pageTitle="Jak przeprowadzić uaktualnienie projektów do bieżącej wersji Azure narzędzia | Microsoft Azure"
   description="Dowiedz się, jak przeprowadzić uaktualnienie Azure projektu w programie Visual Studio do bieżącej wersji narzędzia Azure"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="how-to-upgrade-projects-to-the-current-version-of-the-azure-tools-for-visual-studio"></a>Jak przeprowadzić uaktualnienie projektów do bieżącej wersji narzędzia Azure programu Visual Studio

## <a name="overview"></a>Omówienie

Po zainstalowaniu narzędzia Azure (lub poprzedniej wersji, która jest nowsza od 1.6) bieżącej wersji wszystkich projektów, które zostały utworzone za pomocą narzędzi Azure Zwolnij przed 1.6 (listopad 2011) zostanie automatycznie uaktualniony zaraz po ich otwarciu. Jeśli nadal masz zainstalowany zwolnienia utworzone projektów przy użyciu 1,6 wersji (listopad 2011) z tych narzędzi, można otworzyć tych projektów w starszej wersji i później okazało się, czy chcesz uaktualnić.

## <a name="how-your-project-changes-when-you-upgrade-it"></a>Jak projektu zmienia się po uaktualnieniu

Jeśli projekt jest automatycznie aktualizowane, określ, czy chcesz uaktualnić go projektu jest zmodyfikowany do pracy z bieżącej wersji niektórych zestawów, a niektóre właściwości także zostaną zmienione, jak opisano w tej sekcji. Jeśli projekt wymaga inne zmiany, aby były zgodne z nowszą wersją narzędzia, musisz wprowadzić te zmiany ręcznie.

- Pliku web.config role w sieci web i pliku app.config dla ról pracownika są aktualizowane zostać utworzone odwołanie w nowszej wersji Microsoft.WindowsAzure.Diagnostics.DiagnosticMonitoirTraceListener.dll.

- Zestawy Microsoft.WindowsAzure.StorageClient.dll, Microsoft.WindowsAzure.Diagnostics.dll i Microsoft.WindowsAzure.ServiceRuntime.dll są uaktualniane do nowej wersji.

- Profile publikowania, które są zapisywane w pliku Azure projektu (.ccproj) są przenoszone w osobnym pliku z .azurePubXml rozszerzenia, w podkatalogów **Publikuj** .

- Niektóre właściwości w profilu publikowania zostaną zaktualizowane do obsługi nowych i zmienionych funkcji. Zastępuje **AllowUpgrade** się **DeploymentReplacementMethod** , ponieważ możesz zaktualizować usługi w chmurze wdrożonym równocześnie lub stopniowo.

- Właściwość **UseIISExpressByDefault** zostanie dodany i ustawianie ma wartość FAŁSZ, tak aby serwer sieci web używaną do debugowania zmienią się automatycznie z usług (internetowych informacyjnych) aby wyrazić usług IIS. Express usług IIS jest domyślnego serwera sieci web dla projektów, które są tworzone w nowszych wersjach narzędzia.

- Jeśli Azure pamięci podręcznej znajduje się w jednej lub większej liczby ról projektu, niektóre właściwości w konfiguracji usługi (plik .cscfg) i definicji usługi (plik .csdef) zostaną zmienione, gdy projekt zostanie uaktualniony. Jeśli projekt jest używany pakiet NuGet pamięci podręcznej Azure, projektu jest uaktualniany do najnowszej wersji pakietu. Należy otworzyć plik web.config i sprawdź, czy konfiguracji klienta znajdował poprawnie podczas procesu uaktualniania. Po dodaniu odwołania do pamięci podręcznej Azure zestawów klienta bez użycia pakietu NuGet te zestawy nie zostaną zaktualizowane; należy ręcznie zaktualizować te odwołania do nowej wersji.

>[AZURE.IMPORTANT] W przypadku F # projektów należy ręcznie zaktualizować odwołania do zestawów Azure, tak aby odwołują nowsze wersje zestawów.

### <a name="how-to-upgrade-an-azure-project-to-the-current-release"></a>Jak przeprowadzić uaktualnienie Azure projektu do bieżącej wersji

1. Zainstalować najnowszą wersję narzędzi Azure do instalacji programu Visual Studio, który ma być używany dla uaktualnionej projektu, a następnie otwórz projekt, który chcesz uaktualnić. Jeśli projekt został utworzony przy użyciu narzędzi Azure Zwolnij przed 1.6 (listopad 2011) projektu automatycznie jest uaktualniany do bieżącej wersji. Jeśli utworzono projektu z listopada 2011 Zwolnij zwolnienia jest nadal zainstalowany, projekt zostanie otwarty w tej wersji.

1. W oknie Eksplorator rozwiązań Otwieranie menu skrótów dla węzła projektu, wybierz polecenie **Właściwości**, a następnie wybierz kartę **aplikacji** okna dialogowego, które zostanie wyświetlone.

    Karta **aplikacji** zawiera narzędzia wersja, który jest skojarzony z projektem. Jeśli pojawi się aktualną wersję narzędzia Azure, już została uaktualniona projektu. Jeśli zainstalowano nowszą wersję narzędzia niż zawiera kartę, wyświetlany jest przycisk **uaktualnienia** .

1. Wybierz przycisk **Uaktualnij** przeprowadzić uaktualnienie projektu do bieżącej wersji narzędzia.

1. Tworzenie projektu, a następnie adres jakiekolwiek błędy, powstałe w wyniku zmiany w interfejsie API. Aby uzyskać informacje o sposobie modyfikowania kodu dla nowej wersji zapoznaj się z dokumentacją dla określonego interfejsu API.
