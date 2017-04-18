<properties 
   pageTitle="Dodawanie miejsca do magazynowania Azure za pomocą połączenia usług w programie Visual Studio | Microsoft Azure"
   description="Dodawanie miejsca do magazynowania Azure do aplikacji przy użyciu okna dialogowego Visual Studio Dodaj połączenie usługi"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="storage"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="adding-azure-storage-by-using-visual-studio-connected-services"></a>Dodawanie miejsca do magazynowania Azure za pomocą programu Visual Studio połączenia usług

## <a name="overview"></a>Omówienie

Przy użyciu programu Visual Studio 2015 r. można połączyć dowolny C# usługi w chmurze, usług mobilnych wewnętrznej bazy danych programu .NET, witryny sieci Web programu ASP.NET lub usługi, usługa ASP.NET 5 lub usługi Azure WebJob magazynem usługi Azure za pomocą okna dialogowego **Dodawanie usługami** . Funkcje usługi połączone dodaje wszystkie niezbędne odwołania i kod połączenia i odpowiednio zmienia plików konfiguracji. Okno dialogowe również przejście do dokumentacji informujący o tym, co następne kroki mają zostać rozpoczęte magazyn obiektów blob, kolejki i tabel.

## <a name="supported-project-types"></a>Typy obsługiwanego projektów

Okno dialogowe połączenie usługi umożliwia nawiązywanie połączenia z magazynem Azure w następujących typów projektu.

- Projekty sieci Web programu ASP.NET

- ASP.NET 5 projektów

- Role w .NET chmury usługi sieci Web i projektów roli pracownika

- Projekty usług mobilnych .NET

- Projekty Azure WebJob


## <a name="connect-to-azure-storage-using-the-connected-services-dialog"></a>Nawiązywanie połączenia z magazynem Azure za pomocą okna dialogowego usługami

1. Upewnij się, że masz konto usługi Azure. Jeśli nie masz konta usługi Azure, można Załóż [bezpłatnej wersji próbnej](http://go.microsoft.com/fwlink/?LinkId=518146). Po utworzeniu konta usługi Azure, można utworzyć konta miejsca do magazynowania, tworzenie usług mobilnych i konfigurowanie usługi Azure Active Directory.

1. Otwórz projekt w programie Visual Studio, otwórz menu kontekstowego dla węzła **odwołania** w Eksploratorze rozwiązań, a następnie wybierz **Dodawanie usługi połączone**.

    ![Dodawanie usługi połączone](./media/vs-azure-tools-connected-services-storage/IC796702.png)

1. W oknie dialogowym **Dodawanie usługi połączone** wybierz **Magazyn Azure**, a następnie kliknij przycisk **Konfiguruj** . Może zostać wyświetlony monit o zalogowanie się do Azure, jeśli jeszcze tego nie zrobiono.

    ![Dodawanie okno dialogowe połączenie usługi - miejsca do magazynowania](./media/vs-azure-tools-connected-services-storage/IC796703.png)

1. W oknie dialogowym **Magazyn Azure** wybierz istniejące konto miejsca do magazynowania, a następnie wybierz pozycję **Dodaj**.

    Jeśli chcesz utworzyć nowe konto miejsca do magazynowania, przejdź do następnego kroku. W przeciwnym razie przejdź do kroku 6.

    ![Okno dialogowe Azure miejsca do magazynowania](./media/vs-azure-tools-connected-services-storage/IC796704.png)

1. Aby utworzyć nowe konto miejsca do magazynowania: 

    1. Kliknij przycisk **Utwórz nowe konto miejsca do magazynowania** w dolnej części okna dialogowego magazyn Azure.

    1. Wypełnij okno dialogowe **Utwórz konto miejsca do magazynowania** , a następnie wybierz przycisk **Utwórz** .
    
        ![Okno dialogowe Azure miejsca do magazynowania](./media/vs-azure-tools-connected-services-storage/create-storage-account.png)

        Gdy zechcesz ponownie w oknie dialogowym **Magazyn Azure** , nowy magazyn zostanie wyświetlona na liście.

    1. Wybierz nowy magazyn na liście, a następnie wybierz pozycję **Dodaj**.

1. W obszarze węzła odwołania do usług projektu WebJob pojawi się usługą Magazyn połączone.

    ![Azure miejsca do magazynowania w sieci web zadania w programie project](./media/vs-azure-tools-connected-services-storage/IC796705.png)

1. Przejrzyj wyświetlonej stronie wprowadzenie i Dowiedz się, jak modyfikacji projektu. Wprowadzenie do strony pojawi się w przeglądarce po dodaniu usługi połączone. Możesz przejrzeć Sugerowane następne kroki i przykłady kodu lub przejść na stronę co się stało z odwołaniami, które zostały dodane do projektu i jak pliki kodu i konfiguracji zostały zmodyfikowane.

## <a name="how-your-project-is-modified"></a>Jak zmienić projektu

Po zakończeniu okno dialogowe programu Visual Studio dodaje odwołania i modyfikuje niektóre pliki konfiguracji. Konkretne ustawienia zależą od typu projektu. 

 - W przypadku projektów programu ASP.NET zobacz [co się stało z — projekty ASP.NET](http://go.microsoft.com/fwlink/p/?LinkId=513126). 
 - W przypadku projektów ASP.NET 5 zobacz [co się stało z — projekty 5 ASP.NET](http://go.microsoft.com/fwlink/p/?LinkId=513124). 
 - Chmury usługi projektów (role w sieci web i ról pracownika), zobacz, [co się stało z — projekty usługi w chmurze](http://go.microsoft.com/fwlink/p/?LinkId=516965). 
 - W przypadku projektów WebJob zobacz [co się stało z-WebJob projektów](./storage/vs-storage-webjobs-what-happened.md).

## <a name="next-steps"></a>Następne kroki

1. Przykłady kodu wprowadzenie do używania jako prowadnicy, utworzyć typ magazynowania, który ma, a następnie zacznij pisania kodu dostępu do konta miejsca do magazynowania!

1. Zadawanie pytań i uzyskiwanie pomocy
     - [Forum w witrynie MSDN: Azure przestrzeni dyskowej](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazuredata)

     - [Blog zespołu Azure przestrzeni dyskowej](http://blogs.msdn.com/b/windowsazurestorage/)

     - [Miejsce do magazynowania przy azure.microsoft.com](https://azure.microsoft.com/services/storage/)

     - [Dokumentację dotyczącą miejsca do magazynowania azure.microsoft.com](https://azure.microsoft.com/documentation/services/storage/)

