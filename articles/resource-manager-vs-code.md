<properties
   pageTitle="Przy użyciu kodu w PORÓWNANIU z szablonami Menedżera zasobów | Microsoft Azure"
   description="Pokazano, jak skonfigurować kod Visual Studio, można tworzyć szablony Azure Menedżera zasobów."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="cmatskas"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/26/2016"
   ms.author="chmatsk;tomfitz"/>

# <a name="working-with-azure-resource-manager-templates-in-visual-studio-code"></a>Praca z Azure szablonami Menedżera zasobów w kodzie programu Visual Studio

Pliki JSON, opisujące zasobu i powiązanych zależności są Azure szablony Menedżera zasobów. Tych plików może być czasem duże i skomplikowane, więc ważne jest narzędzie pomocy technicznej. Kod Visual Studio jest Edytor kodu nowego, uproszczonego, Otwórz źródło, między platformami. Obsługuje tworzenie i edytowanie szablonów Menedżera zasobów za pomocą [nowego rozszerzenia](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools). Kod w PORÓWNANIU z uruchamia wszędzie i nie wymaga dostęp do Internetu, chyba że chcesz również wdrażanie szablonów Menedżera zasobów.

Jeśli nie masz już w PORÓWNANIU z kodu, możesz zainstalować go na [https://code.visualstudio.com/](https://code.visualstudio.com/).

## <a name="install-the-resource-manager-extension"></a>Zainstaluj rozszerzenie Menedżera zasobów

Aby pracować z szablonami JSON w PORÓWNANIU z kodu, należy zainstalować rozszerzenie. Poniższe kroki pobrać i zainstalować obsługę języka dla szablonów JSON Menedżera zasobów:

1. Uruchamianie kodu w PORÓWNANIU z 
2. Otwieranie szybkie otwieranie (Ctrl + P) 
3. Uruchom następujące polecenie: 

        ext install azurerm-vscode-tools

4. Uruchom ponownie kod w PORÓWNANIU z po wyświetleniu monitu, aby włączyć rozszerzenie. 

 Zadanie gotowe!

## <a name="set-up-resource-manager-snippets"></a>Konfigurowanie wstawki Menedżera zasobów

Wykonanie powyższych czynności spowoduje zainstalowane narzędzia pomocy technicznej, ale teraz trzeba skonfigurować w PORÓWNANIU z kodu w celu użycia wstawki szablonu JSON.

1. Skopiuj zawartość pliku z repozytorium [azure-xplat-arm narzędzia](https://raw.githubusercontent.com/Azure/azure-xplat-arm-tooling/master/VSCode/armsnippets.json) do Schowka.
2. Uruchamianie kodu w PORÓWNANIU z 
3. W kodzie w PORÓWNANIU z możesz otworzyć plik wstawki JSON, albo Przechodzenie do **pliku** -> **Preferencje** -> **Wstawki użytkownika** -> **JSON**lub przez wybranie **F1** i wpisując **Preferencje** , dopóki nie można zaznaczyć **Preferencje: wstawki**.

    ![wstawki preferencji](./media/resource-manager-vs-code/preferences-snippets.png)

    Za pomocą opcji wybierz **JSON**.

    ![Wybierz pozycję json](./media/resource-manager-vs-code/select-json.png)

4. Wklej zawartość pliku w kroku 1 do pliku wstawki użytkownika przed końcowym "}" 
5. Upewnij się, JSON wygląda OK i istnieją nie squiggles w dowolnym miejscu. 
6. Zapisz i zamknij plik wstawki użytkownika.

To wszystko, potrzebne do rozpocząć korzystanie z narzędzia wstawki Menedżera zasobów. Następnie możemy przykleić Instalatora w teście.

## <a name="work-with-template-in-vs-code"></a>Praca z szablonem w PORÓWNANIU z kodu

Najłatwiejszym sposobem rozpocząć pracę przy użyciu szablonu jest chwyć Szybki Start dostępne szablony na [Github](https://github.com/Azure/azure-quickstart-templates) albo użyj jednej z własnych. Można łatwo [eksportować szablonu](resource-manager-export-template.md) , w przypadku każdej z grup zasobów za pośrednictwem portalu. 

1. Jeśli szablon jest wyeksportowany z Grupa zasobów, otwórz wyodrębnione pliki w PORÓWNANIU z kodu.

    ![Pokazywanie plików](./media/resource-manager-vs-code/show-files.png)

2. Otwórz plik template.json, tak aby można go edytować i dodać dodatkowe zasoby. Po **"zasobów": [** naciśnij klawisz enter, aby rozpocząć nowy wiersz. Jeśli wpiszesz **arm**, zostanie wyświetlony z listy opcji. Te opcje są wstawki szablonu, który został zainstalowany. Jego powinna wyglądać następująco: 

    ![Pokazywanie wstawki](./media/resource-manager-vs-code/type-snippets.png)

3. Wybierz fragment, który chcesz. W tym artykule wybierania **arm ip** , aby utworzyć nowy publiczny adres IP. Przecinka po nawiasie zamykającym "}" nowo utworzonego zasobu, aby upewnić się, że szablon składnia jest prawidłowa.

     ![Dodawanie przecinek](./media/resource-manager-vs-code/add-comma.png)

4. Kod w PORÓWNANIU z ma wbudowane technologii IntelliSense. Podczas edytowania szablonów kod w PORÓWNANIU z sugestią dostępne wartości. Na przykład, aby dodać sekcji Zmienne szablonu, należy dodać **""** (dwa podwójnych cudzysłowów) i wybierz pozycję **Klawisze Ctrl + spacja** między tymi oferty. Zostanie wyświetlona z poleceniem **zmiennych**.

    ![Dodawanie zmiennych](./media/resource-manager-vs-code/add-variables.png)

5. Lista IntelliSense można również Zaproponuj dostępne wartości lub funkcji. Aby ustawić właściwość wartości parametru, należy utworzyć wyrażenie **"[]"** i **Klawisze Ctrl + spacja**. Możesz rozpocząć wpisywanie nazwy funkcji. Po odnalezieniu funkcji, która ma, wybierz **kartę** .

    ![Dodawanie parametru](./media/resource-manager-vs-code/select-parameters.png)

6. Wybierz ponownie **Klawisze Ctrl + spacja** w funkcji, aby wyświetlić listę parametrów dostępnych w szablonie.

    ![Dodawanie parametru](./media/resource-manager-vs-code/select-avail-parameters.png)

7. Jeśli masz problemy sprawdzania poprawności schematu w szablonie, pojawi się znanych squiggles w edytorze. Można wyświetlić listę błędów i ostrzeżeń wpisywać **Klawisze Ctrl + Shift + M** lub zaznaczając symbole na dolnym pasku stanu po lewej stronie.

    ![błędy](./media/resource-manager-vs-code/errors.png)

    Sprawdzanie poprawności szablonu może pomóc w wykrywania problemów składni. można jednak także zobaczyć błędy, które można zignorować. W niektórych przypadkach edytorze porównuje szablonu ze schematem, który nie jest aktualne i w związku z tym zgłasza błąd, nawet jeśli wiadomo, że jest poprawny. Załóżmy na przykład, funkcja ostatnio dodano do Menedżera zasobów, ale nie zostały zaktualizowane schematu. Edytor zgłasza błąd mimo, które działa poprawnie podczas wdrażania.

    ![komunikat o błędzie](./media/resource-manager-vs-code/unrecognized-function.png)

## <a name="deploy-your-new-resources"></a>Wdrażanie nowych zasobów

Gdy szablon jest gotowy, należy wdrożyć nowych zasobów, postępując zgodnie z instrukcjami poniżej: 

### <a name="windows"></a>Systemu Windows

1. Otwórz okno wiersza polecenia programu PowerShell 
2. Typ logowania: 

        Login-AzureRmAccount 

3. Jeśli masz wiele subskrypcji uzyskać listę subskrypcji z:

        Get-AzureRmSubscription

    I wybierz subskrypcję, aby użyć.
   
        Select-AzureRmSubscription -SubscriptionId <Subscription Id>

4. Aktualizowanie parametrów w pliku parameters.json
5. Uruchamianie Deploy.ps1 wdrożenia szablonu Azure

### <a name="osxlinux"></a>OSX i Linux

1. Otwórz okno końcowych 
2. Typ logowania:

        azure login 

3. Jeśli masz wiele subskrypcji, wybierz prawym subskrypcję z:

        azure account set <subscriptionNameOrId> 

4. Aktualizowanie parametrów w pliku parameters.json.
5. Aby wdrożyć szablon, należy uruchomić:

        azure group deployment create -f <PathToTemplate> 

## <a name="next-steps"></a>Następne kroki

- Aby dowiedzieć się więcej na temat szablonów, zobacz [Szablony do tworzenia Azure Menedżera zasobów](resource-group-authoring-templates.md).
- Aby uzyskać informacje o funkcjach szablonu, zobacz [funkcje szablonu Azure Menedżera zasobów](resource-group-template-functions.md).
- Więcej przykładów pracy z kod Visual Studio zobacz [aplikacje chmury kompilacji kodem Visual Studio](https://github.com/Microsoft/HealthClinic.biz/wiki/Build-cloud-apps-with-Visual-Studio-Code) przy Połącz [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 [Pokaz](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/). Aby uzyskać więcej Przewodniki Szybki Start z pokaz HealthClinic.biz zobacz [Azure Deweloper narzędzia Przewodniki Szybki Start](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts).
