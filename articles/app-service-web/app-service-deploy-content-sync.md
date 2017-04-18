<properties
    pageTitle="Synchronizowanie zawartości z folderu chmura Azure aplikacji usługi"
    description="Dowiedz się, jak wdrożyć aplikację Azure aplikacji usługi za pośrednictwem synchronizacji zawartości z folderu chmury."
    services="app-service"
    documentationCenter=""
    authors="dariagrigoriu"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/13/2016"
    ms.author="dariagrigoriu"/>
    
# <a name="sync-content-from-a-cloud-folder-to-azure-app-service"></a>Synchronizowanie zawartości z folderu chmura Azure aplikacji usługi

Ten samouczek pokazano, jak wdrożyć [Azure aplikacji usługi](http://go.microsoft.com/fwlink/?LinkId=529714) przez synchronizowanie zawartości z chmury popularne miejsca do magazynowania usług, takich jak usługi Dropbox i OneDrive. 

## <a name="overview"></a>Omówienie rozmieszczania zawartości synchronizacji

Wdrażanie synchronizacji zawartości na żądanie jest obsługiwany przez [aparat wdrożenia Kudu](https://github.com/projectkudu/kudu/wiki) zintegrowany z aplikacji usługi. [Azure Portal](https://portal.azure.com)można wyznaczyć folder w magazynu w chmurze, podczas pracy z kodem aplikacji i zawartości, w tym folderze i synchronizowania z aplikacji usługi za pomocą kliknięcia przycisku. Synchronizowanie zawartości wykorzystuje proces Kudu tworzenie i wdrażanie. 
    
## <a name="contentsync"></a>Jak włączyć rozmieszczania zawartości synchronizacji
Aby włączyć synchronizowanie zawartości z [Azure Portal](https://portal.azure.com), wykonaj następujące czynności:

1. W swojej aplikacji karta w Azure Portal, kliknij przycisk **Ustawienia** > **Źródło rozmieszczania**. Kliknij pozycję **Wybierz źródło**, a następnie wybierz pozycję **OneDrive** lub **Dropbox** jako źródła do wdrożenia. 

    ![Synchronizowanie zawartości](./media/app-service-deploy-content-sync/deployment_source.png)

    >[AZURE.NOTE] Ze względu na podstawowej różnic w za pośrednictwem interfejsów API **usługi OneDrive dla firm** nie jest obsługiwane w tej chwili. 

2. Kończenie przepływu pracy autoryzacji, aby włączyć usługę aplikacji uzyskać dostęp do określonej ścieżki wyznaczonych wstępnie zdefiniowane dla usługi OneDrive lub Dropbox miejsce, w którym wszystkie aplikacji usługi zawartości będą przechowywane.  
    Po uzyskaniu zezwolenia aplikacji usługi platformy pozwala odpowiednią opcję, aby utworzyć folder zawartości w obszarze określonej ścieżce zawartości lub wybrać istniejący folder zawartości w obszarze tej określonej ścieżce zawartości. Wyznaczonych ścieżek zawartości w obszarze konta miejsca do magazynowania chmury używane do synchronizacji usługi aplikacji są następujące:  
    * **OneDrive**:`Apps\Azure Web Apps` 
    * **Dropbox**:`Dropbox\Apps\Azure`

3. Po początkowej synchronizacji zawartości zawartości synchronizacji może zostać zainicjowana na żądanie z poziomu portalu Azure. Historia wdrożenia jest dostępna z karta **wdrożenia** .

    ![Historia wdrażania](./media/app-service-deploy-content-sync/onedrive_sync.png)
 
Więcej informacji do wdrożenia skrzynki jest dostępne w obszarze [Rozmieszczanie z usługi Dropbox](http://blogs.msdn.com/b/windowsazure/archive/2013/03/19/new-deploy-to-windows-azure-web-sites-from-dropbox.aspx). 


