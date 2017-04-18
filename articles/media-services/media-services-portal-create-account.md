<properties
    pageTitle=" Tworzenie konta usługi multimediów Azure Portal Azure | Microsoft Azure"
    description="Ten samouczek przeprowadzi Cię przez kroki tworzenia konta usługi multimediów Azure Portal Azure."
    services="media-services"
    documentationCenter=""
    authors="Juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/24/2016"
    ms.author="juliako"/>


# <a name="create-an-azure-media-services-account-using-the-azure-portal"></a>Tworzenie konta usługi multimediów Azure za pomocą portalu Azure

> [AZURE.SELECTOR]
- [Portal](media-services-portal-create-account.md)
- [Programu PowerShell](media-services-manage-with-powershell.md)
- [POZOSTAŁE](http://msdn.microsoft.com/library/azure/dn194267.aspx)

> [AZURE.NOTE] Aby użyć tego samouczka, potrzebne jest konto Azure. Aby uzyskać szczegółowe informacje zobacz [Azure bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/). 

Azure portal umożliwia szybkie tworzenie konta usługi multimediów Azure (AMS). Za pomocą konta dostępu do usług multimediów, które umożliwiają przechowywanie, szyfrowanie kodowanie, zarządzanie i przesyłanie strumieniowe zawartości multimedialnej na Azure. Podczas tworzenia konta usługi multimediów, możesz również utworzyć konto skojarzone przestrzeni dyskowej (lub użyj istniejącego) w tym samym regionu geograficznego jako konto usługi multimediów.

W tym artykule opisano niektóre typowe koncepcji i pokazano, jak utworzyć konto usługi multimediów Azure Portal.

## <a name="concepts"></a>Pojęcia

Uzyskiwanie dostępu do usługi multimediów wymaga dwóch skojarzonych kont:

- Konto usługi multimediów. Konta umożliwia dostęp do zestawu opartej na chmurze usługi multimediów, które są dostępne w Azure. Konto usługi multimediów zawartości multimedialnej rzeczywiste nie są zapisywane. Zamiast tego, przechowuje metadane dotyczącego zawartości i multimediów przetwarzanie zadań na Twoim koncie. Podczas tworzenia konta możesz wybrać dostępne usługi multimediów regionie. Region, które możesz wybrać jest centrum danych, który przechowuje rekordy metadanych dla Twojego konta.

    Regiony usługi multimediów (AMS) dostępne są następujące: północna Europa, Zachodnia Europa, zachód US, wschodniego US, Azji Południowo-Wschodniej, Azji Wschodniej Zachodnia Japonia wschód Japonii. Usługi multimediów nie korzysta z grup koligacji.
    
    AMS jest obecnie dostępny w następujących centrach danych: Południowej Brazylia, zachód Indie Południowej Indie i Indie centralnej. Azure portal umożliwia teraz utworzyć konta usługi multimediów i wykonywać różne zadania opisane w tym miejscu. Jednak Live kodowanie nie jest włączona w następujących centrach danych. Ponadto nie wszystkie typy jednostek zastrzeżone kodowanie są dostępne w tych centrach danych.
    
    - Brazylia południe: Tylko standardowe i podstawowe kodowanie zastrzeżone jednostki są dostępne.
    - Indie Zachodnia Południowej Indie: 

- Konto Azure miejsca do magazynowania. Konta miejsca do magazynowania musi znajdować się w tym samym regionu geograficznego jako konto usługi multimediów. Podczas tworzenia konta usługi multimediów, możesz wybrać istniejącego konta miejsca do magazynowania w tym samym regionie lub można utworzyć nowe konto miejsca do magazynowania w tym samym regionie. Jeśli usuniesz konto usługi multimediów blob na koncie powiązanych magazynowania nie zostaną usunięte.

## <a name="create-an-ams-account"></a>Tworzenie konta AMS

W tej sekcji pokazano, jak utworzyć konto AMS.

1. Zaloguj się w [portalu Azure](https://portal.azure.com/).
2. Kliknij pozycję **+ Nowy** > **Web + Mobile** > **usługi multimediów**.

    ![Tworzenie usługi multimediów](./media/media-services-portal-vod-get-started/media-services-new1.png)

3. **Tworzenie** konta usługi multimediów wprowadź żądane wartości.

    ![Tworzenie usługi multimediów](./media/media-services-portal-vod-get-started/media-services-new3.png)
    
    1. W polu **Nazwa konta**wpisz nazwę nowego konta AMS. Nazwa konta usługi multimediów są wszystkie liczby litera lub litery bez spacji i długości 3-24 znaków.
    2. W subskrypcji wybierz jeden z różnych subskrypcjach Azure, które mają dostęp do.
    
    2. **Grupa zasobów**wybierz zasób nowym lub istniejącym.  Grupa zasobów to zbiór zasobów, które współużytkują cyklu życia, uprawnień i zasady. Dowiedz się, [w tym miejscu](azure-resource-manager/resource-group-overview.md#resource-groups).
    3. W polu **Lokalizacja**wybierz regionu geograficznego, która będzie używana do przechowywania rekordów multimedia i metadanych dla Twojego konta usługi multimediów. Ten obszar będzie używana do przetwarzania i przesyłanie strumieniowe multimediów. Tylko dostępne regiony usługi multimediów są wyświetlane w polu listy rozwijanej. 
    
    3. **Miejsca do magazynowania konta**wybierz konto miejsca do magazynowania o podanie magazyn obiektów blob zawartości multimedialnej z konta usługi multimediów. Można wybrać istniejące konto miejsca do magazynowania w tym samym regionu geograficznego jako konta usługi multimediów, lub można utworzyć konto miejsca do magazynowania. W tym samym regionie tworzone jest nowe konto miejsca do magazynowania. Zasady przechowywania nazwy kont są takie same jak w przypadku kont usługi multimediów.

        Dowiedz się więcej o miejsca do magazynowania [w tym miejscu](storage-introduction.md).

    4. Wybierz pozycję **Przypnij do pulpitu nawigacyjnego** , aby wyświetlić postęp wdrożenia konta.
    
7. Kliknij przycisk **Utwórz** w dolnej części formularza.

    Po pomyślnym utworzeniu konta zostanie zmieniony na **Uruchamianie**. 

    ![Ustawienia usługi multimediów](./media/media-services-portal-vod-get-started/media-services-settings.png)

    Aby zarządzać swoim kontem AMS (na przykład przekazywanie klipów wideo, kodowanie składników majątku, monitorować postęp zadania) za pomocą okna **Ustawienia** .

## <a name="manage-keys"></a>Zarządzanie klawiszy

Potrzebujesz nazwę konta i informacje o kluczu podstawowym na programowy dostęp do konta usługi multimediów.

1. W portalu usługi Azure wybierz swoje konto. 

    Po prawej stronie zostanie wyświetlone okno **Ustawienia** . 

2. W oknie **Ustawienia** wybierz pozycję **klawiszy**. 

    **Zarządzaj kluczami** systemu windows zawiera nazwę konta, a zostanie wyświetlona kluczy podstawowych i pomocniczych. 
3. Naciśnij przycisk Kopiuj, aby skopiować wartości.
    
    ![Klucze usługi multimediów](./media/media-services-portal-vod-get-started/media-services-keys.png)

## <a name="next-steps"></a>Następne kroki

Teraz możesz przekazać pliki do konta AMS. Aby uzyskać więcej informacji zobacz [przekazywanie plików](media-services-portal-upload-files.md).

## <a name="media-services-learning-paths"></a>Ścieżek nauki usługi multimediów

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


