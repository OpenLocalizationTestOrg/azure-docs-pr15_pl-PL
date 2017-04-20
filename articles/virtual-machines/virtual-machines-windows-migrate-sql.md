<properties
    pageTitle="Migracja bazy danych programu SQL Server do programu SQL Server na maszynie Wirtualnej | Microsoft Azure"
    description="Dowiedz się więcej na temat migracji do programu SQL Server bazy danych użytkowników lokalnych w Azure maszyny wirtualnej."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="sabotta"
    manager="jhubbard"
    editor=""
    tags="azure-service-management" />
<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="carlasab"/>


# <a name="migrate-a-sql-server-database-to-sql-server-in-an-azure-vm"></a>Migracja bazy danych programu SQL Server do programu SQL Server w Maszynę wirtualną Azure

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]Model Menedżera zasobów.


Istnieje wiele metod migrowania bazy danych lokalnych użytkowników programu SQL Server do programu SQL Server w Maszynę wirtualną Azure. W tym artykule będzie krótko omówiono różne metody, zaleca się najlepszą metodę dla różnych scenariuszy i dołączaj [samouczka](#azure-vm-deployment-wizard-tutorial) ułatwi wykonanie za pomocą kreatora **Rozmieszczanie bazy danych programu SQL Server do maszyny Wirtualnej Microsoft Azure** . 

Metoda, za pomocą kreatora **wdrożyć bazy danych programu SQL Server do maszyny Wirtualnej Microsoft Azure** opisane w [Poradniku](#azure-vm-deployment-wizard-tutorial) służy tylko model klasyczny wdrażania. 

## <a name="what-are-the-primary-migration-methods"></a>Jakie są metody migracji podstawowego?

Są to metody migracji podstawowego:

- Użyć rozmieszczanie bazy danych programu SQL Server do kreatora Microsoft Azure VM
- Wykonaj kopię zapasową lokalnej przy użyciu kompresji i ręcznie skopiować plik kopii zapasowej do maszyny wirtualnej Azure
- Wykonywanie kopii zapasowej do adresu URL i przywrócić do maszyny wirtualnej Azure z adresu URL
- Odłączyć i następnie skopiować do magazynu obiektów blob Azure plików danych i dziennika, a następnie dołączyć do programu SQL Server w Azure VM z adresu URL
- Konwertowanie lokalnego komputera fizycznego do wirtualnego dysku twardego funkcji Hyper-V, przekazać do magazynu obiektów Blob, a następnie wdrożyć jako nowej maszyny Wirtualnej za pomocą przekazać wirtualnego dysku twardego
- Statek dysk twardy przy użyciu systemu Windows usługi importu/eksportu
- Jeśli masz lokalnego wdrożenia programu AlwaysOn, utworzyć repliki w Azure, a następnie pracy awaryjnej, wskazując użytkownikom wystąpieniu Azure bazy danych za pomocą [Kreatora dodawania repliki Azure](virtual-machines-windows-classic-sql-onprem-availability.md)
- Użyj programu SQL Server, [replikacji transakcyjnej](https://msdn.microsoft.com/library/ms151176.aspx) skonfigurować wystąpienie serwera SQL Azure jako subskrybent produktu, a następnie wyłączyć replikacji, wskazując użytkownikom wystąpieniu Azure baza danych



## <a name="choosing-your-migration-method"></a>Wybierając metodę migracji

Wydajność transferu danych optymalne migracja plików bazy danych do maszyny Wirtualnej Azure przy użyciu skompresowany plik kopii zapasowej jest generalnie najlepszą metodą. To jest metoda, która używa [Wdrażanie bazy danych programu SQL Server do kreatora Microsoft Azure VM](#azure-vm-deployment-wizard-tutorial) . Ten kreator jest zalecaną metodą Migrowanie lokalnego użytkownika bazy danych uruchomiony na SQL Server 2005 lub większa do 2014 serwera SQL lub większa, gdy plik kopii zapasowej skompresowanej bazy danych jest mniejsza niż 1 TB.

Aby zminimalizować czas przestoju podczas procesu migracji bazy danych, należy użyć opcji AlwaysOn albo opcji replikacji transakcyjnej.

Jeśli nie jest możliwe skorzystanie z powyższych metod, ręcznie przeprowadzić migrację bazy danych. Metoda ta będzie zazwyczaj rozpoczyna się z kopii zapasowej bazy danych, a następnie kopię bazy danych kopii zapasowej do Azure, a następnie wykonać przywracanie bazy danych. Można również skopiować same pliki bazy danych do Azure, a następnie dołącza je. Istnieje kilka metod za pomocą których można wykonywać ten ręczny proces migracji baz danych do Maszynę wirtualną Azure.

> [AZURE.NOTE] Podczas uaktualniania do programu SQL Server 2014 lub programu SQL Server 2016 ze starszych wersji programu SQL Server, należy rozważyć, czy są potrzebne zmiany. Firma Microsoft zaleca adresu wszystkie zależności na funkcje nie obsługiwane przez nową wersję programu SQL Server jako część na migrację. Aby uzyskać więcej informacji dotyczących obsługiwanych wersji i scenariuszy zobacz [uaktualnienie do programu SQL Server](https://msdn.microsoft.com/library/bb677622.aspx).

W poniższej tabeli przedstawiono każdy z metody migracji podstawowego i omówiono, kiedy korzystanie z każdej z tych metod jest najbardziej odpowiedni.

| Metoda  | Wersja bazy danych źródłowych  |  Wersji bazy danych miejsca docelowego | Ograniczenie rozmiaru kopii zapasowej bazy danych źródłowych  | Notatki  |
|---|---|---|---|---|
| [Użyć rozmieszczanie bazy danych programu SQL Server do kreatora Microsoft Azure VM](#azure-vm-deployment-wizard-tutorial) | SQL Server 2005 lub nowszy | SQL Server 2014 lub nowszy | < 1 TB  | Najszybsza i najprostsza metoda, użyj, jeśli to możliwe przeprowadzić migrację do nowego lub istniejącego wystąpienia programu SQL Server w Azure maszyny wirtualnej | 
| [Użyj Azure repliki Kreator dodawania](virtual-machines-windows-classic-sql-onprem-availability.md) | SQL Server 2012 lub nowszy | SQL Server 2012 lub nowszy | [Limit magazynowania Azure maszyny Wirtualnej](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) | Minimalizuje przestoje, stosowane, gdy masz wdrożenia lokalnego (AlwaysOn) |
| [Replikacja transakcyjna programu SQL Server](https://msdn.microsoft.com/library/ms151176.aspx) | SQL Server 2005 lub nowszy | SQL Server 2005 lub nowszy | [Limit magazynowania Azure maszyny Wirtualnej](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) | Użyć, gdy trzeba zminimalizować czas przestoju i nie masz wdrożenia lokalnego (AlwaysOn) |
| [Wykonaj kopię zapasową lokalnej przy użyciu kompresji i ręcznie skopiować plik kopii zapasowej do maszyny wirtualnej Azure](#backup-to-file-and-copy-to-vm-and-restore) | SQL Server 2005 lub nowszy | SQL Server 2005 lub nowszy | [Limit magazynowania Azure maszyny Wirtualnej](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) | Użyj tylko, gdy nie można użyć kreatora, na przykład kiedy wersji bazy danych miejsca docelowego jest mniejsza niż CU2 dodatku SP1 dla programu SQL Server 2012 lub rozmiar kopii zapasowej bazy danych jest większy niż 1 TB (12,8 TB w programie SQL Server 2016) |
| [Wykonywanie kopii zapasowej do adresu URL i przywrócić do maszyny wirtualnej Azure z adresu URL](#backup-to-url-and-restore) | CU2 dodatku SP1 dla programu SQL Server 2012 lub nowszego | CU2 dodatku SP1 dla programu SQL Server 2012 lub nowszego | < 12,8 TB dla programu SQL Server 2016, w przeciwnym razie < 1 TB | Ogólnie za pomocą [kopii zapasowej do adresu URL](https://msdn.microsoft.com/library/dn435916.aspx) jest równoważne wydajności do za pomocą kreatora i nie bardzo łatwe |
| [Odłączyć i następnie skopiować do magazynu obiektów blob Azure plików danych i dziennika, a następnie dołączyć do programu SQL Server w maszyny wirtualnej Azure z adresu URL](#detach-and-copy-to-url-and-attach-from-url) | SQL Server 2005 lub nowszy | SQL Server 2014 lub nowszy | [Limit magazynowania Azure maszyny Wirtualnej](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) | Ta metoda, gdy planuje [zapisać te pliki przy użyciu usługi magazynu obiektów Blob Azure](https://msdn.microsoft.com/library/dn385720.aspx) i dołączyć je do SQL Server w Maszynę wirtualną Azure, zwłaszcza w przypadku bardzo dużych baz danych |
| [Konwertowanie lokalnego komputera na funkcji Hyper-V wirtualnych dysków twardych, przekazać do magazynu obiektów Blob, a następnie wdrożyć maszynę wirtualną za pomocą przesłane wirtualnego dysku twardego](#convert-to-vm-and-upload-to-url-and-deploy-as-new-vm) | SQL Server 2005 lub nowszy | SQL Server 2005 lub nowszy | [Limit magazynowania Azure maszyny Wirtualnej](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) | Stosowane, gdy [wniesienie licencja programu SQL Server](../sql-database/sql-database-paas-vs-sql-server-iaas.md), kiedy dokonywana jest migracja bazy danych, która zostanie uruchomiona w starszej wersji programu SQL Server, lub kiedy dokonywana jest migracja systemu i użytkownika bazy danych razem w ramach migracji bazy danych zależy od innych baz danych użytkowników i/lub baz danych systemu. |
| [Statek dysk twardy przy użyciu systemu Windows usługi importu/eksportu](#ship-hard-drive) | SQL Server 2005 lub nowszy | SQL Server 2005 lub nowszy | [Limit magazynowania Azure maszyny Wirtualnej](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) | Korzystać z [Usługi importu/eksportu systemu Windows](../storage/storage-import-export-service.md) , gdy metodą ręcznych kopii jest zbyt wolno, takie jak w przypadku bardzo dużych baz danych |

## <a name="azure-vm-deployment-wizard-tutorial"></a>Azure Samouczek Kreatora wdrażania maszyn wirtualnych

Użyj kreatora **wdrożyć bazy danych programu SQL Server do maszyny Wirtualnej Microsoft Azure** w Microsoft SQL Server Management Studio do migracji programu SQL Server 2005, SQL Server 2008, programu SQL Server 2008 R2, programu SQL Server 2012, SQL Server 2014 lub SQL Server 2016 użytkownika lokalnego bazy danych (1 TB) do 2014 r. programu SQL Server lub SQL Server 2016 w Azure maszyny wirtualnej. Ten kreator służy do migracji bazy danych użytkownika, aby istniejące maszyny wirtualnej Azure albo Maszynę wirtualną Azure z programem SQL Server, utworzony przez kreatora podczas procesu migracji. Kiedy dokonywana jest migracja bazy danych do nowszej wersji programu SQL Server, bazy danych jest automatycznie uaktualniane podczas procesu.

Metoda jest tylko klasyczny wdrażania modelu. 

### <a name="get-latest-version-of-the-deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard"></a>Pobierz najnowszą wersję programu Deploy bazy danych programu SQL Server do kreatora Microsoft Azure VM

Użyj najnowszej wersji programu Microsoft SQL Server Management Studio dla programu SQL Server do zapewnienia, że masz najnowszą wersję programu Kreator **Wdrażanie bazy danych programu SQL Server do maszyny Wirtualnej Microsoft Azure** . Najnowsza wersja tego kreatora zawiera najnowsze aktualizacje do wersji zapoznawczej i obsługuje najnowsze obrazy Azure VM w galerii (starszych wersji kreatora mogą nie działać). Aby uzyskać najnowszą wersję programu Microsoft SQL Server Management Studio dla programu SQL Server, [to Pobierz](http://go.microsoft.com/fwlink/?LinkId=616025) i zainstalować go na komputerze klienckim z łączność z bazą danych, że plan migracji i Internetu.

### <a name="configure-the-existing-azure-virtual-machine-and-sql-server-instance-if-applicable"></a>Konfigurowanie istniejącej maszyny wirtualnej Azure i wystąpienie serwera SQL (jeśli dotyczy)

Jeśli są migrowane do istniejących VM Azure, wymagane są następujące czynności konfiguracyjne:

- Konfigurowanie maszyn wirtualnych Azure i wystąpienia programu SQL Server, aby umożliwić łączność z innego komputera przez wykonanie kroków, aby [połączyć się z wystąpieniem programu SQL Server VM z SSMS na innym komputerze](virtual-machines-windows-sql-connect.md). Tylko 2014 serwera SQL i programu SQL Server 2016 obrazy w galerii są obsługiwane podczas migrowania za pomocą kreatora.
- Skonfiguruj punkt końcowy otwarte dla usługi SQL Server Adapter chmury na gateway Microsoft Azure z prywatnego portu 11435. Ten port jest tworzona jako część SQL Server 2014 lub SQL Server 2016 inicjowania obsługi administracyjnej na maszynie Wirtualnej Microsoft Azure. Karta chmura tworzy również reguły zapory systemu Windows, aby umożliwić jego przychodzących połączeń TCP na domyślny port 11435. Ten punkt końcowy umożliwia korzystanie z Kreatora korzystania z usługi chmury Adapter, aby skopiować pliki kopii zapasowej z lokalnego wystąpienia z maszyną wirtualną Azure. Aby uzyskać więcej informacji zobacz temat [Karty chmura dla programu SQL Server](https://msdn.microsoft.com/library/dn169301.aspx).

    ![Utwórz punkt końcowy karty chmury](./media/virtual-machines-windows-migrate-sql/cloud-adapter-endpoint.png)

### <a name="run-the-use-the-deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard"></a>Uruchom użycie wdrażanie bazy danych programu SQL Server do kreatora Microsoft Azure VM

1. Otwórz Microsoft SQL Server Management Studio dla programu Microsoft SQL Server 2016 i połączyć się z wystąpieniem programu SQL Server zawierające bazy danych użytkowników, które będą migrowane do Maszynę wirtualną Azure.
2. Kliknij prawym przyciskiem myszy bazę danych, która jest przeprowadzana migracja, wskaż polecenie zadania, a następnie kliknij przycisk Deploy do maszyny Wirtualnej Microsoft Azure.

    ![Uruchom Kreatora](./media/virtual-machines-windows-migrate-sql/start-wizard.png)

3. Na stronie wprowadzenia kliknij przycisk Dalej.
4. Na stronie Ustawienia źródła połączyć się z wystąpieniem programu SQL Server, zawierający bazę danych, którą chcesz migrować do Maszynę wirtualną Azure.
5. Określ lokalizację tymczasową dla plików kopii zapasowych. Jeśli połączenie z serwerem zdalnym, należy określić dysk sieciowy.

    ![Ustawienia źródła](./media/virtual-machines-windows-migrate-sql/source-settings.png)

6. Kliknij przycisk Dalej.
7. Na stronie Microsoft Azure Sign-In kliknij opcję Zarejestruj i zaloguj się do swojego konta Azure.
8. Wybierz subskrypcję, którą chcesz użyć i kliknij przycisk Dalej.

    ![Azure zalogowania](./media/virtual-machines-windows-migrate-sql/azure-signin.png)

9. Na stronie Ustawienia wdrażania można określić nową lub istniejącą usługę chmury nazwa i Nazwa maszyny wirtualnej:

 - Określ nową nazwę chmury nazwę usługi i inną maszynę wirtualną do utworzenia nowej usługi w chmurze z nowej maszyny wirtualnej Azure przy użyciu obrazu programu SQL Server 2014 lub Galeria 2016 serwera SQL.

     - Jeśli określisz nazwę nowej usługi w chmurze, określ konto magazynu, który będzie używany.

     - Jeśli określisz nazwę istniejącego usługa w chmurze, konto magazynu będą pobierane i wprowadzony automatycznie.

 - Określenie istniejącej nazwy usługa w chmurze i nową nazwę maszyny wirtualnej do tworzenia nowej maszyny wirtualnej Azure w istniejącej usługi chmury. Określić tylko obraz galerii SQL Server 2014 lub SQL Server 2016.
 - Określenie istniejącej nazwy usługa w chmurze i nazwę maszyny wirtualnej, aby użyć istniejącej Azure maszyny wirtualnej. Najpierw obraz zbudowany przy użyciu obrazu galerii SQL Server 2014 lub SQL Server 2016.

        ![Deployment Settings](./media/virtual-machines-windows-migrate-sql/deployment-settings.png)

10. Kliknij przycisk Ustawienia
  - Jeśli określono istniejącej nazwy usługa w chmurze i nazwę maszyny wirtualnej, pojawi się monit o podanie nazwy użytkownika i hasła.

        ![Ustawienia maszyny Azure](./media/virtual-machines-windows-migrate-sql/azure-machine-settings.png)

    - Jeśli określono nazwę nowej maszyny wirtualnej, pojawi się monit wybierz obraz z listy obrazów w galerii i podaj następujące informacje:
      - Obraz — zaznacz tylko SQL Server 2014 lub SQL Server 2016
        - Nazwa użytkownika
        - Nowe hasło
        - Potwierdź hasło
        - Lokalizacja
        - Rozmiar.
    - Ponadto, kliknij, aby zaakceptować generowane przez siebie certyfikatu dla tej nowej firmy Microsoft Azure maszyny wirtualnej, a następnie kliknij przycisk OK.

    ![Azure nowe ustawienia komputera](./media/virtual-machines-windows-migrate-sql/azure-new-machine-settings.png)

11. Określ nazwę docelowej bazy danych, jeśli różni się od Nazwa źródłowej bazy danych. Jeśli docelowa baza danych już istnieje, system będzie automatycznie zwiększane Nazwa bazy danych zamiast zastąpienie istniejącej bazy danych.
12. Kliknij przycisk Dalej, a następnie kliknij przycisk Zakończ.

    ![Wyniki](./media/virtual-machines-windows-migrate-sql/results.png)

13. Po zakończeniu działania kreatora, nawiązać połączenie z maszyną wirtualną i sprawdź, przeniesiono bazę danych.
14. Jeśli tworzona jest nowa maszyna wirtualna, konfigurowanie maszyny wirtualnej Azure i wystąpienia programu SQL Server przez wykonanie kroków, aby [połączyć się z wystąpieniem programu SQL Server VM z SSMS na innym komputerze](virtual-machines-windows-sql-connect.md).

## <a name="backup-to-file-and-copy-to-vm-and-restore"></a>Kopię zapasową do pliku i skopiowanie do maszyny Wirtualnej i przywracania

Ta metoda nie może użyć rozmieszczanie bazy danych programu SQL Server do kreatora Microsoft Azure VM ponieważ są migrowane do wersji SQL Server przed SQL Server 2014 lub pliku kopii zapasowej jest większy niż 1 TB. Jeśli plik kopii zapasowej jest większy niż 1 TB, musi paski ponieważ maksymalny rozmiar dysku maszyny Wirtualnej jest 1 TB. Umożliwia migrację bazy danych użytkowników tą metodą ręcznych następujące ogólne kroki:

1.  Wykonywanie kopii zapasowej pełnej bazy danych do lokalizacji lokalnego.
2.  Utwórz lub przesłać z wersją programu SQL Server na potrzeby maszyny wirtualnej.
3.  Konfiguracja połączenia w oparciu o Twoje wymagania. Zobacz [połączyć z maszyną wirtualną serwera SQL Azure (Menedżer zasobów)](virtual-machines-windows-sql-connect.md).
4.  Skopiuj pliki kopii zapasowej maszynę Wirtualną za pomocą pulpitu zdalnego, Eksploratora Windows lub polecenia Kopiuj z wiersza polecenia.

## <a name="backup-to-url-and-restore"></a>Kopia zapasowa, aby adres URL i przywracania

Jeśli nie możesz używać wdrażanie bazy danych programu SQL Server do kreatora Microsoft Azure VM, ponieważ plik kopii zapasowej jest większy niż 1 TB i są migrowane z i do programu SQL Server 2016, należy użyć metody [kopii zapasowej do adresu URL](https://msdn.microsoft.com/library/dn435916.aspx) . W przypadku baz danych jest mniejszy niż 1 TB lub wersją programu SQL Server przed SQL Server 2016 zaleca się stosowanie kreatora. Z programu SQL Server 2016 paski zestawy kopii zapasowych są obsługiwane, są zalecane dla wydajności i wymagane przekraczają limity rozmiaru dla obiektu blob. Dla bardzo dużych baz danych zaleca się korzystanie z [Usługi importu/eksportu systemu Windows](../storage/storage-import-export-service.md) .

## <a name="detach-and-copy-to-url-and-attach-from-url"></a>Odłączanie i skopiuj adres URL i dołączanie z adresu URL

Metoda ta jest używana, gdy planuje [zapisać te pliki przy użyciu usługi magazynu obiektów Blob Azure](https://msdn.microsoft.com/library/dn385720.aspx) i dołączyć je do SQL Server w Maszynę wirtualną Azure, zwłaszcza w przypadku bardzo dużych baz danych. Umożliwia migrację bazy danych użytkowników tą metodą ręcznych następujące ogólne kroki:

1.  Odłączanie plików bazy danych z lokalnego wystąpienia bazy danych.
2.  Skopiuj pliki odłączoną bazę danych do przechowywania Azure blob przy użyciu [Narzędzia wiersza polecenia AZCopy](../storage/storage-use-azcopy.md).
3.  Załączanie plików bazy danych z adresu URL Azure do wystąpienia programu SQL Server w Azure VM.

## <a name="convert-to-vm-and-upload-to-url-and-deploy-as-new-vm"></a>Konwertuj na maszyny Wirtualnej i przesłać do adresu URL i wdrożyć jako nowej maszyny Wirtualnej

Ta metoda umożliwia migrację wszystkich baz danych systemu i użytkownika w lokalnym wystąpieniu programu SQL Server do maszyny wirtualnej Azure. Umożliwia migrację całej instancji SQL Server tą metodą ręcznych następujące ogólne kroki:

1.  Przekonwertować fizycznych i maszyn wirtualnych z dyskami VHD funkcji Hyper-V za pomocą [Programu Microsoft Virtual Machine Converter](http://technet.microsoft.com/library/dn873998.aspx).
2.  Przekazywanie plików dysków VHD do magazynu Azure przy użyciu [polecenia cmdlet Add-AzureVHD](https://msdn.microsoft.com/library/windowsazure/dn495173.aspx).
3.  Wdrażanie nowej maszyny wirtualnej przy użyciu przekazanego dysku VHD.

> [AZURE.NOTE] Aby przeprowadzić migrację całej aplikacji, należy rozważyć użycie [Azure Site Recovery](../site-recovery/site-recovery-overview.md)].

## <a name="ship-hard-drive"></a>Dysk twardy statku

[Metoda usługi importu/eksportu systemu Windows](../storage/storage-import-export-service.md) umożliwia transfer dużych ilości danych z pliku do magazynu obiektów Blob w sytuacjach, gdy przesyłanie przez sieć jest niemożliwe lub nadmiernie kosztowne. Z tą usługą możesz wysłać jeden lub więcej dysków twardych, zawierającą te dane do centrum danych Azure, gdzie dane są przesyłane na koncie magazynu.

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji na temat uruchamiania programu SQL Server na maszynach wirtualnych Azure zobacz [SQL Server na maszynach wirtualnych Azure Przegląd](virtual-machines-windows-sql-server-iaas-overview.md).

Aby uzyskać instrukcje tworzenia maszyny wirtualnej serwera SQL Azure z przechwyconym obrazie zobacz [porady i wskazówki na temat klonowania Azure SQL maszyn wirtualnych z przechwyconych obrazów](https://blogs.msdn.microsoft.com/psssql/2016/07/06/tips-tricks-on-cloning-azure-sql-virtual-machines-from-captured-images/) w blogu CSS SQL Server inżynierów.
