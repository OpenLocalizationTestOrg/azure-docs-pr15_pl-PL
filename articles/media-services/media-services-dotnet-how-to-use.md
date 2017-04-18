<properties 
    pageTitle="Jak skonfigurować komputer Media Services rozwoju program .NET" 
    description="Informacje na temat wymagania wstępne dotyczące usługi multimediów przy użyciu zestawu SDK usługi multimediów dla środowiska .NET. Także Dowiedz się, jak utworzyć aplikację programu Visual Studio." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="10/24/2016"  
    ms.author="juliako"/>

#<a name="media-services-development-with-net"></a>Opracowywania usługi multimediów z .NET

[AZURE.INCLUDE [media-services-selector-setup](../../includes/media-services-selector-setup.md)]

W tym temacie omówiono sposób rozpocząć tworzenie aplikacji usługi multimediów za pomocą .NET.

Biblioteka **SDK .NET usługi multimediów Azure** umożliwia program przed usługi multimediów za pomocą .NET. Aby ułatwić nawet opracowywaniu program .NET, znajduje się biblioteki **Rozszerzenia SDK .NET usług multimediów Azure** . Ta biblioteka zawiera zestaw metody rozszerzenia i funkcje pomocy, które będą Uprość kod .NET. Zarówno biblioteki są dostępne za pośrednictwem **NuGet** i **GitHub**.


##<a name="prerequisites"></a>Wymagania wstępne

-   Konto usługi multimediów Azure subskrypcji nowej lub istniejącej. Zobacz temat [jak utworzyć konto usługi multimediów](media-services-portal-create-account.md).
-   Systemy operacyjne: Systemu Windows 10, Windows 7, Windows 2008 R2 lub Windows 8.
-   .NET framework 4,5.
-    Visual Studio 2015, Visual Studio 2013, program Visual Studio 2012 lub Visual Studio 2010 z dodatkiem SP1 (Professional, Premium, Ultimate lub Express).


##<a name="create-and-configure-a-visual-studio-project"></a>Tworzenie i konfigurowanie projektu programu Visual Studio

W tej sekcji pokazano, jak utworzyć projekt w programie Visual Studio i przygotować publikację opracowywania usługi multimediów.  W tym przypadku projekt jest aplikacji konsoli C# systemu Windows, ale te same kroki konfiguracji tu dotyczą innych typów projektów, które można utworzyć dla aplikacji usługi multimediów (na przykład aplikacji Windows Forms lub aplikacji sieci Web programu ASP.NET).

W tej sekcji przedstawiono sposób użycia **NuGet** w celu dodania SDK .NET usługi multimediów i innych bibliotekach zależne.

Można również uzyskać najnowszą bitów SDK .NET usługi multimediów z GitHub ([github.com/Azure/azure-sdk-for-media-services](https://github.com/Azure/azure-sdk-for-media-services) i [github.com/Azure/azure-sdk-for-media-services-extensions](https://github.com/Azure/azure-sdk-for-media-services-extensions)), utworzyć rozwiązanie i dodać odwołania do projektu klienta. Należy zauważyć, że wszystkie niezbędne zależności pobieranie pobrane i wyodrębnionych automatycznie.

1. Tworzenie nowej aplikacji konsoli C# w Visual Studio 2010 z dodatkiem SP1 lub nowszym w PORÓWNANIU z. Wprowadź **nazwę**, **lokalizację**i **nazwę rozwiązanie**, a następnie kliknij przycisk OK.

2. Utworzyć rozwiązanie.

2. Aby zainstalować i dodać **Rozszerzenia SDK .NET usług multimediów Azure**za pomocą **NuGet** . Również instalowania tego pakietu, instaluje **Multimediów usług .NET SDK** oraz dodaje wszystkie wymagane zależności.

    Upewnij się, że masz najnowszą wersję NuGet zainstalowany. Aby uzyskać więcej informacji i instalacji instrukcje zobacz [NuGet](http://nuget.codeplex.com/).

2. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy nazwę projektu, a następnie wybierz pozycję Zarządzaj NuGet pakietów.

    Zostanie wyświetlone okno dialogowe Zarządzanie pakietami NuGet.

3. W galerii w trybie Online Wyszukaj rozszerzenia MediaServices Azure, wybierz pozycję Azure multimediów usługi .NET SDK rozszerzenia, a następnie kliknij przycisk Zainstaluj.

    Projekt jest zmodyfikowany i odwołania do rozszerzenia SDK .NET usługi multimediów, SDK .NET usługi multimediów i innych zestawy zależne zostaną dodane.

4. Promowanie oczyszczania środowisko projektowania, należy rozważyć, czy włączyć Przywracanie pakietu NuGet. Aby uzyskać więcej informacji, zobacz [Przywracanie pakietu NuGet "](http://docs.nuget.org/consume/package-restore).

3. Dodaj odwołanie do zestawu **System.Configuration** . Ten zestaw zawiera System.Configuration. Klasa **ConfigurationManager** , która umożliwia dostęp do plików konfiguracji (na przykład App.config).

    Aby dodać odwołania przy użyciu okna dialogowego Zarządzanie odwołania, kliknij prawym przyciskiem myszy nazwę projektu w Eksploratorze rozwiązań. Następnie wybierz pozycję Dodaj i odwołania.

    Zostanie wyświetlone okno dialogowe Zarządzanie odwołania.

4. W obszarze .NET framework Assembly Znajdź i zaznacz zestawu System.Configuration i kliknij przycisk OK.
5. Otwieranie pliku App.config (Dodaj go do projektu, jeśli nie został dodany domyślnie) i dodać sekcję *appSettings* do pliku.     
Ustaw wartość dla usługi multimediów Azure nazwy i konta klucz konta, jak pokazano w poniższym przykładzie.

    Aby znaleźć nazwę i klucz wartości, przejdź do portalu Azure i wybierz swoje konto. Po prawej stronie zostanie wyświetlone okno Ustawienia. W oknie Ustawienia wybierz pozycję klawiszy. Klikając ikonę obok każdego pola tekstowego kopiuje wartość do Schowka systemu.


        <configuration>
        ...
            <appSettings>
              <add key="MediaServicesAccountName" value="Media-Services-Account-Name" />
              <add key="MediaServicesAccountKey" value="Media-Services-Account-Key" />
            </appSettings>

        </configuration>

6. Zastąpienie istniejącego **przy użyciu** raportów na początku pliku Plik Program.cs poniższy kod.

        using System;
        using System.Collections.Generic;
        using System.Linq;
        using System.Text;
        using System.Threading.Tasks;
        using System.Configuration;
        using System.Threading;
        using System.IO;
        using Microsoft.WindowsAzure.MediaServices.Client;

W tym momencie możesz przystąpić do uruchomienia opracowywania aplikacji usługi multimediów.    


##<a name="media-services-learning-paths"></a>Ścieżek nauki usługi multimediów

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
