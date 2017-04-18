<properties
    pageTitle="Wdrażanie dużych wdrożeń"
    description="Dowiedz się, jak wdrożyć dużych wdrożeń za pomocą narzędzi Azure dla programu Eclipse."
    services=""
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="multiple"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/dn268601.aspx -->

# <a name="deploying-large-deployments"></a>Wdrażanie dużych wdrożeń #

Wdrożenia jest zbyt duża, aby być zawarte w domyślnym folderze approot, można użyć zasobu magazynu lokalnego jako folder główny wdrożenia, dla swojego JDK i serwer aplikacji.

## <a name="to-use-a-local-storage-resource-as-the-deployment-root-folder-for-large-deployments"></a>Aby użyć zasobu lokalnego magazynu jako folder główny wdrożenia w dużych ##

1. Tworzenie nowego zasobu magazynu lokalnego. Nazwa zasobu nie ma znaczenia. Zasoby magazynowania są definiowane na poziomie roli. Najszybszym sposobem uzyskania dostępu do okna dialogowego konfiguracji magazynu lokalnego, z której można utworzyć nowego zasobu lokalnego magazynu jest w następujący sposób: kliknij prawym przyciskiem myszy roli w widoku **Eksplorator projektu** (rozwiń węzeł projektu Azure, jeśli nie widzisz roli), kliknij pozycję **Azure**, a następnie kliknij **Magazynu lokalnego**. W oknie dialogowym **Magazynu lokalnego** kliknij przycisk **Dodaj** , aby utworzyć nowy zasób magazynu lokalnego.
1. Ustaw odpowiedni rozmiar co najmniej 2048 MB (NIC mniej może powodować tego samego pliku rozmiar problemy podczas może wystąpić w approot).
1. Upewnij się, że **Oczyszczanie zawartości podczas wystąpienia rola zostanie odtworzony** jest zaznaczone pole wyboru; Dzięki temu zapobiec Logika uruchamiania w ramach wdrożenia występowania konfliktów z istniejącym plikami w zasobie po usunięciu wystąpienie roli.
1. Upewnij się, że **zmiennej środowiska przechowywania ścieżkę katalogu zasobu po wdrożeniu** ma wartość do ciągu **DEPLOYROOT**. Okno dialogowe z magazynu lokalnego zasobu będzie wyglądać podobnie do następującej.
    ![][ic667943]

Alternatywnie Jeśli używasz **DEPLOYROOT** jako *nazwę* zasobu lokalnego i nie zmienisz nazwę zmiennej środowiska generowane automatycznie (który jest równa **DEPLOYROOT_PATH** w takim przypadku), który będzie działać także aplikacji.

Właściwości [magazynu lokalnego][]znajdują się dodatkowe informacje o tworzeniu zasobu magazynu lokalnego.

## <a name="see-also"></a>Zobacz też ##

[Azure zestaw narzędzi dla programu Eclipse][]

[Tworzenie aplikacji Witaj świecie dla Azure w Zaćmienie][]

[Instalowanie narzędzi Azure dla programu Eclipse][] 

Aby uzyskać więcej informacji na temat Azure za pomocą języka Java zobacz [Centrum deweloperów języka Java Azure][].

<!-- URL List -->

[Centrum deweloperów języka Java Azure]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure zestaw narzędzi dla programu Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Tworzenie aplikacji Witaj świecie dla Azure w Zaćmienie]: http://go.microsoft.com/fwlink/?LinkID=699533
[Instalowanie narzędzi Azure dla programu Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546
[Właściwości magazynu lokalnego]: http://go.microsoft.com/fwlink/?LinkID=699525#local_storage_properties

<!-- IMG List -->

[ic667943]: ./media/azure-toolkit-for-eclipse-deploying-large-deployments/ic667943.png
