<properties 
    pageTitle="Przywracanie aplikacji platformy Azure" 
    description="Dowiedz się, jak przywrócić aplikacji z kopii zapasowej." 
    services="app-service" 
    documentationCenter="" 
    authors="cephalin" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/06/2016" 
    ms.author="cephalin"/>

# <a name="restore-an-app-in-azure"></a>Przywracanie aplikacji platformy Azure

W tym artykule pokazano, jak przywrócić aplikacji w [Usłudze Azure aplikacji](../app-service/app-service-value-prop-what-is.md) , która wcześniej wykonano kopię zapasową (patrz [wyżej aplikacji platformy Azure](web-sites-backup.md)). Można przywrócić aplikacji z jego połączonych baz danych (baza danych SQL lub MySQL) na żądanie do poprzedniego stanu lub utworzenia nowej aplikacji na podstawie jednej z aplikacji oryginalnej kopii zapasowej. Tworzenie nowej aplikacji uruchamianej równolegle do najnowszej wersji może być przydatne w przypadku A / B testowania.

Przywracanie z kopii zapasowej jest dostępna dla aplikacji działa w warstwie **standardowych** i **Premium** . Aby dowiedzieć się, jak skalowanie wewnętrzne aplikacji zobacz [rozbudowy aplikacji platformy Azure](web-sites-scale.md). Poziom **Premium** umożliwia większą liczbę codzienną do wykonania niż **Standardowy** warstwy.

<a name="PreviousBackup"></a>
## <a name="restore-an-app-from-an-existing-backup"></a>Przywracanie aplikacji z istniejącą kopię zapasową

1. Wybierz polecenie karta **Ustawienia** aplikacji w Azure Portal, **kopie zapasowe** , aby wyświetlić karta **kopie zapasowe** . Następnie kliknij pozycję **Przywróć teraz** na pasku poleceń. 
    
    ![Wybierz pozycję Przywróć teraz][ChooseRestoreNow]

3. W karta **Przywracanie** najpierw wybierz źródłowy kopii zapasowej. 

    ![](./media/web-sites-restore/021ChooseSource.png)
    
    **Kopia zapasowa aplikacji** opcja powoduje wyświetlenie wszystkich istniejących kopii zapasowych bieżącej aplikacji, a następnie można łatwo wybrać. 
    Opcja **magazynowania** pozwala wybrać dowolny plik kopii zapasowej ZIP z wszelkie istniejące konto Azure miejsca do magazynowania i kontenera w ramach subskrypcji. 
    Jeśli próbujesz przywrócić innej aplikacji, za pomocą opcji **miejsca do magazynowania** .

4. Następnie należy określić miejsce docelowe do przywracania aplikacji w **Przywracanie miejsca docelowego**.

    ![](./media/web-sites-restore/022ChooseDestination.png)
    
    >[AZURE.WARNING] Jeśli wybierzesz **Zastąp**, zostaną usunięte wszystkie dane znajdujące się w bieżącej aplikacji. Zanim klikniesz **przycisk OK**, upewnij się, że jest dokładnie co chcesz zrobić.
    
    Możesz wybrać **Istniejącej aplikacji** , aby przywrócić kopię zapasową aplikacji w tej samej grupy identyczny innej aplikacji. Aby użyć tej opcji, należy zostały już utworzone innej aplikacji w grupie zasobów z odzwierciedlające konfiguracji bazy danych do zdefiniowana w kopii zapasowej aplikacji. 
    
5. Kliknij **przycisk OK**.

<a name="StorageAccount"></a>
## <a name="download-or-delete-a-backup-from-a-storage-account"></a>Pobieranie i usuwanie kopii zapasowej konta miejsca do magazynowania
    
1. W głównym **Przeglądanie** karta portalu Azure wybierz pozycję **konta miejsca do magazynowania**.
    
    Zostanie wyświetlona lista Twoich istniejących kont miejsca do magazynowania. 
    
2. Wybierz konto miejsca do magazynowania, który zawiera kopię zapasową, który chcesz pobrać lub usuwanie.
    
    Zostanie wyświetlona karta konta miejsca do magazynowania.

3. W karta accountn miejsca do magazynowania wybierz kontenera, który ma
    
    ![Widok kontenerów][ViewContainers]

4. Zaznacz plik kopii zapasowej, który chcesz pobrać lub usunąć.

    ![ViewContainers](./media/web-sites-restore/03ViewFiles.png)

5. W zależności od tego, co chcesz zrobić, kliknij przycisk **Pobierz** lub **usunąć** .  

<a name="OperationLogs"></a>
## <a name="monitor-a-restore-operation"></a>Monitorowanie operacji przywracania
    
1. Aby wyświetlić szczegółowe informacje o powodzeniu lub niepowodzeniu operacji przywracania aplikacji, przejdź do karta **Dziennik inspekcji** w portalu Azure. 
    
    Karta **dzienników inspekcji** zostaną wyświetlone wszystkie operacji, wraz z poziomu, stan, zasobów i szczegóły czasu.
    
2. Przewiń w dół do żądane operacji przywracania i kliknij przycisk, aby go zaznaczyć.

Karta Szczegóły wyświetli dostępne informacje dotyczące operacji przywracania.
    
## <a name="next-steps"></a>Następne kroki

Można również kopia zapasowa i przywracanie aplikacji usługi aplikacji za pomocą interfejsu API usługi REST (zobacz [Pozostałych Użyj tworzenia kopii zapasowych i przywracanie aplikacji usługi aplikacji](websites-csm-backup.md)).

>[AZURE.NOTE] Jeśli chcesz rozpocząć pracę z Azure aplikacji usługi przed utworzeniem konta dla konta Azure, przejdź do [Spróbuj aplikacji usługi](http://go.microsoft.com/fwlink/?LinkId=523751), którym natychmiast można utworzyć aplikację sieci web krótkotrwałe starter w aplikacji usługi. Nie kart kredytowych wymagane; nie zobowiązania.


<!-- IMAGES -->
[ChooseRestoreNow]: ./media/web-sites-restore/02ChooseRestoreNow.png
[ViewContainers]: ./media/web-sites-restore/03ViewContainers.png
[StorageAccountFile]: ./media/web-sites-restore/02StorageAccountFile.png
[BrowseCloudStorage]: ./media/web-sites-restore/03BrowseCloudStorage.png
[StorageAccountFileSelected]: ./media/web-sites-restore/04StorageAccountFileSelected.png
[ChooseRestoreSettings]: ./media/web-sites-restore/05ChooseRestoreSettings.png
[ChooseDBServer]: ./media/web-sites-restore/06ChooseDBServer.png
[RestoreToNewSQLDB]: ./media/web-sites-restore/07RestoreToNewSQLDB.png
[NewSQLDBConfig]: ./media/web-sites-restore/08NewSQLDBConfig.png
[RestoredContosoWebSite]: ./media/web-sites-restore/09RestoredContosoWebSite.png
[DashboardOperationLogsLink]: ./media/web-sites-restore/10DashboardOperationLogsLink.png
[ManagementServicesOperationLogsList]: ./media/web-sites-restore/11ManagementServicesOperationLogsList.png
[DetailsButton]: ./media/web-sites-restore/12DetailsButton.png
[OperationDetails]: ./media/web-sites-restore/13OperationDetails.png
 
