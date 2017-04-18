<properties
    pageTitle="Jak używać Azure miejsca do magazynowania dla programu SQL Server kopii zapasowych i przywracanie | Microsoft Azure"
    description="Dowiedz się, jak utworzyć kopię zapasową programu SQL Server do magazynu Azure. Omówiono zalety wykonywanie kopii zapasowej bazy danych programu SQL Azure magazynem."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="MikeRayMSFT"
    manager="jhubbard"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="07/22/2016"
    ms.author="mikeray"/>

# <a name="use-azure-storage-for-sql-server-backup-and-restore"></a>Używanie Azure miejsca do magazynowania dla i przywracania kopii zapasowych programu SQL Server

## <a name="overview"></a>Omówienie

Począwszy od programu SQL Server 2012 z dodatkiem SP1 CU2, możesz teraz zapisać kopie zapasowe programu SQL Server bezpośrednio z usługą Magazyn obiektów Blob platformy Azure. Ta funkcja umożliwia kopii zapasowych i przywracanie z usługi Azure obiektów Blob z bazy danych programu SQL Server lokalnego lub bazy danych programu SQL Server w Azure maszyn wirtualnych. Wykonywanie kopii zapasowych do chmury oferuje zalety dostępność, nieograniczony replikowane geo poza nim miejsca do magazynowania i ułatwienia migracji danych do i z chmury. Wykonywanie kopii zapasowych i przywracanie instrukcji może wydawać przy użyciu języka Transact-SQL lub SMO.

Program SQL Server 2016 przedstawiono nowe funkcje; [kopii zapasowej migawki pliku](http://msdn.microsoft.com/library/mt169363.aspx) umożliwia wykonywanie niemal chwilową kopii zapasowych i przywracania niezwykle szybki.

W tym temacie wyjaśniono, dlaczego warto wybrać korzystania z magazynu Azure kopii zapasowych SQL i zawiera opis elementów związanych. Dostęp do instruktaży i dodatkowe informacje w celu korzystania z tej usługi z kopii zapasowych programu SQL Server, można użyć zasobów na końcu tego artykułu.

## <a name="benefits-of-using-the-azure-blob-service-for-sql-server-backups"></a>Korzyści wynikające z używania usługi obiektów Blob platformy Azure kopii zapasowych serwera SQL

Istnieje kilka problemów, które buźkę podczas wykonywania kopii zapasowej programu SQL Server. Te problemy obejmują zarządzanie magazynem ryzyko niepowodzenia miejsca do magazynowania, dostępu do magazynowania poza i konfiguracja sprzętu. Wiele te problemy dotyczą przy użyciu usługi Magazyn obiektów Blob platformy Azure kopii zapasowych programu SQL Server. Należy rozważyć następujące korzyści:

- **Łatwość użycia**: przechowywania kopii zapasowych w Azure blob może być wygodne, elastyczne i łatwo uzyskać dostęp do opcji poza nim. Tworzenie poza nim miejsca do magazynowania dla kopie zapasowe programu SQL Server może być tak proste jak modyfikowanie usługi istniejących skryptów/zadań, aby użyć tej składni **Kopii zapasowej do adresu URL** . Zazwyczaj należy poza magazynowania daleko od lokalizacji bazy danych produkcji, aby zapobiec pojedynczej awarii, które mogą mieć wpływ na obu produkcji i poza nim lokalizacje baz danych. Wybierając [obiektów blob usługi Azure skopiowanymi geo](../storage/storage-redundancy.md), masz dodatkową warstwę ochrony danych, które mogą wpłynąć na całego regionu.
- **Wykonywanie kopii zapasowych archiwum**: Usługa Magazyn obiektów Blob platformy Azure oferuje lepszym rozwiązaniem opcji taśmą często używane do archiwizacji kopie zapasowe. Magazynu może wymagać fizycznie transportu poza pomieszczenia i środki ochrony multimediów. Przechowywania kopii zapasowych w magazynie obiektów Blob platformy Azure udostępnia moment wysoką dostępność i trwałe archiwizacji opcji.
- **Sprzętowe zarządzanych**: się nie dodatkowe Zarządzanie sprzętu przy użyciu usług Azure. Usług Azure zarządzanie sprzęt i podaj replikacji geo nadmiarowości i ochrony przed awarie sprzętu.
- **Nieograniczoną miejsca do magazynowania**: po włączeniu kopii zapasowej bezpośrednio do obiektów blob platformy Azure, masz dostęp do nieograniczoną miejsca do magazynowania. Możesz też wykonywanie kopii zapasowych na dysk Azure maszyn wirtualnych występują ograniczenia na podstawie rozmiaru komputera. Istnieje ograniczenie liczby dysków, które można dołączać do Azure maszyn wirtualnych kopii zapasowych. Ten limit jest 16 dysków w przypadku wystąpienia bardzo duże i mniej mniejszych wystąpień.
- **Dostępność kopii zapasowej**: kopie zapasowe przechowywane w Azure BLOB są dostępne z dowolnego miejsca i w dowolnym momencie i można łatwo można uzyskać dostępu do przywracania do lokalnego programu SQL Server lub innego programu SQL Server uruchomiony w Azure maszyny wirtualnej, bez potrzeby Dołączanie i odłączanie bazy danych lub z pobieraniem i dołączanie wirtualnego dysku twardego.
- **Koszt**: opłatami tylko usługa, która jest używana. Może być efektywne pod względem kosztów jako opcja archiwum poza i kopii zapasowej. Zobacz [Azure cennik kalkulatora](http://go.microsoft.com/fwlink/?LinkId=277060 "Kalkulatora ceny")i [ceny Azure artykuł](http://go.microsoft.com/fwlink/?LinkId=277059 "artykuł") Aby uzyskać więcej informacji.
- **Magazyn migawek**: pliki bazy danych są przechowywane w obiektów blob platformy Azure i korzystasz z programu SQL Server 2016, możesz korzystać [kopii zapasowej migawki pliku](http://msdn.microsoft.com/library/mt169363.aspx) do wykonywania niemal chwilową kopii zapasowych i przywracanie niezwykle szybki.

Aby uzyskać więcej informacji zobacz [SQL Server wykonywanie kopii zapasowych i przywracanie z usługi Magazyn obiektów Blob platformy Azure](http://go.microsoft.com/fwlink/?LinkId=271617).

Następujące dwie sekcje wprowadzić Usługa Magazyn obiektów Blob platformy Azure, w tym wymagane składniki programu SQL Server. Należy zrozumieć składniki i ich interakcji pomyślnie kopii zapasowych i przywracanie z usługą Magazyn obiektów Blob platformy Azure.

## <a name="azure-blob-storage-service-components"></a>Składniki usługi Magazyn obiektów Blob platformy Azure

Następujące składniki Azure są używane podczas wykonywania kopii zapasowej z usługą Magazyn obiektów Blob platformy Azure.

| Składnik               | Opis                          |
|---------------------|-------------------------------|
| **Konto miejsca do magazynowania** | Konto miejsca do magazynowania jest punktem początkowym dla wszystkich usług miejsca do magazynowania. Aby uzyskać dostęp do usługi Magazyn obiektów Blob platformy Azure, należy najpierw utworzyć konto Azure miejsca do magazynowania. Aby uzyskać więcej informacji na temat usługi Magazyn obiektów Blob platformy Azure zobacz [jak do korzystania z usługi Magazyn obiektów Blob platformy Azure](https://azure.microsoft.com/develop/net/how-to-guides/blob-storage/) |
| **Kontener** | Kontener zawiera grupę zestawu obiektów blob i mogą zawierać dowolną liczbę obiektów blob. Napisać programu SQL Server kopii zapasowej do usługi obiektów Blob platformy Azure są konieczne co najmniej kontenera root utworzone. |
| **Obiektów blob** | Plik dowolnego typu i rozmiar. BLOB są adresach w następującym formacie adresu URL: **https://[storage account].blob.core.windows.net/[container]/[blob]**. Aby uzyskać więcej informacji o blob strony zobacz [opis bloku i blob strony](http://msdn.microsoft.com/library/azure/ee691964.aspx) |

## <a name="sql-server-components"></a>Składniki programu SQL Server

Następujące składniki programu SQL Server są używane podczas wykonywania kopii zapasowej z usługą Magazyn obiektów Blob platformy Azure.

| Składnik               | Opis                          |
|---------------------|-------------------------------|
| **ADRES URL** | Adres URL określa zasobów identyfikator URI (Uniform) do pliku kopii zapasowej unikatowe. Adres URL jest używany do Podaj lokalizację i nazwę pliku kopii zapasowej programu SQL Server. Adres URL musi wskazywać rzeczywiste obiektów blob nie tylko kontenera. Jeśli to nie istnieje, jest tworzony. Jeśli istniejący blob jest określony, kopia zapasowa nie powiedzie się, chyba że > Formatowanie z opcji. Oto przykład adresu URL należy określić w tym poleceniu kopii zapasowej: **http[s]://[storageaccount].blob.core.windows.net/[container]/[FILENAME.bak]**. Protokół HTTPS jest zalecane, ale nie są wymagane. |
| **Poświadczenia** | Informacje, które są wymagane do łączenia i uwierzytelniania usługi Magazyn obiektów Blob platformy Azure są przechowywane jako poświadczenie.  Aby SQL Server, aby zapisać kopie zapasowe obiektów Blob platformy Azure lub przywracania z niej należy utworzyć poświadczeń programu SQL Server. Aby uzyskać więcej informacji zobacz [Poświadczeń serwera SQL](https://msdn.microsoft.com/library/ms189522.aspx). |

> [AZURE.NOTE] Jeśli wybierzesz do skopiowania, a następnie przekaż plik kopii zapasowej z usługą Magazyn obiektów Blob platformy Azure, musisz użyć typ obiektów blob strony możliwość miejsca do magazynowania, jeśli zamierzasz użyć tego pliku dla operacji przywracania. Przywracanie z typ obiektów blob bloku zakończy się niepowodzeniem z powodu błędu.

## <a name="next-steps"></a>Następne kroki

1. Utwórz konto Azure, jeśli nie masz jeszcze jedną. Jeśli użytkownik ocenia Azure, warto rozważyć [bezpłatnej wersji próbnej](https://azure.microsoft.com/free/).

1. Następnie przejdź za pośrednictwem jednego z następujące samouczki, które pomagają przez tworzenie konta miejsca do magazynowania i wykonywanie przywracania.

    - **Program SQL Server 2014**: [Samouczek: SQL Server 2014 wykonywanie kopii zapasowych i przywracanie platformy Microsoft Azure Blob usługi Magazyn](https://msdn.microsoft.com/library/jj720558\(v=sql.120\).aspx).
    - **Program SQL Server 2016**: [Samouczek: przy użyciu usługi Magazyn obiektów Blob platformy Azure firmy Microsoft z bazami danych programu SQL Server 2016](https://msdn.microsoft.com/library/dn466438.aspx)

1. Przejrzyj dokumentację począwszy od [SQL Server wykonywanie kopii zapasowych i przywracanie przy użyciu usługi Magazyn obiektów Blob platformy Azure Microsoft](https://msdn.microsoft.com/library/jj919148.aspx).

Jeśli masz problemy z Przejrzyj temat [Narzędzia Kopia zapasowa programu SQL Server do adresu URL najlepszymi rozwiązaniami i rozwiązywanie problemów](https://msdn.microsoft.com/library/jj919149.aspx).

Dla innych programu SQL Server kopia zapasowa i przywracanie opcji, zobacz [Kopia zapasowa i przywracanie dla programu SQL Server w maszyn wirtualnych Azure](../virtual-machines/virtual-machines-windows-sql-backup-recovery.md).
