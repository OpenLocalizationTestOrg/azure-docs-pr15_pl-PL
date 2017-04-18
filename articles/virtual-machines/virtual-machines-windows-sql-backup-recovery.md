<properties
    pageTitle="Wykonywanie kopii zapasowych i przywracanie dla programu SQL Server | Microsoft Azure"
    description="W tym artykule opisano zagadnienia kopia zapasowa i przywracanie baz danych programu SQL Server uruchomiony na maszyn wirtualnych Azure."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-resource-management" />

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="08/19/2016"
    ms.author="jroth" />

# <a name="backup-and-restore-for-sql-server-in-azure-virtual-machines"></a>Kopia zapasowa i przywracanie dla programu SQL Server w środowisku maszyn wirtualnych Azure

## <a name="overview"></a>Omówienie

Wykonywanie kopii zapasowych danych w bazach danych programu SQL Server jest ważne część strategii w zakresie ochrony przed utratą danych z powodu błędów aplikacji lub użytkownika. Dotyczy to również program SQL Server uruchomiony na Azure wirtualnych maszyn.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Dla programu SQL Server działa w maszyny wirtualne Azure można używać natywnych kopii zapasowych i przywracania technik za pomocą dysków dołączonych do miejsca docelowego plików kopii zapasowych. Jednak istnieje ograniczenie liczby dysków, które można dołączać do Azure maszyny wirtualnej, na podstawie [rozmiaru maszyny wirtualnej](virtual-machines-linux-sizes.md). Istnieje także ogólnych zarządzania dysku brać pod uwagę.

Począwszy od programu SQL Server 2014, można tworzenie kopii zapasowych i przywracania magazynem obiektów Blob platformy Azure firmy Microsoft. Program SQL Server 2016 także ulepszenia dla tej opcji. Ponadto plików bazy danych przechowywanych w magazynie obiektów Blob platformy Azure firmy Microsoft, SQL Server 2016 udostępnia opcję dla niemal chwilową wykonywania kopii zapasowych i przywracania szybkiego używania migawek Azure. W tym artykule omówiono te opcje i dodatkowe informacje można znaleźć w witrynie [SQL Server wykonywanie kopii zapasowych i przywracanie przy użyciu usługi Magazyn obiektów Blob platformy Azure Microsoft](https://msdn.microsoft.com/library/jj919148.aspx).

>[AZURE.NOTE] Aby uzyskać omówienie opcji do tworzenia kopii zapasowych bardzo dużych baz danych zobacz [Wielu terabajtów SQL Server bazy danych kopii zapasowej strategii maszyn wirtualnych Azure](http://blogs.msdn.com/b/igorpag/archive/2015/07/28/multi-terabyte-sql-server-database-backup-strategies-for-azure-virtual-machines.aspx).

Poniższe sekcje zawierają informacje specyficzne dla różnych wersji programu SQL Server obsługiwane w Azure maszyn wirtualnych.

## <a name="sql-server-virtual-machines"></a>Maszyn wirtualnych programu SQL Server

Gdy wystąpienie programu SQL Server jest uruchomiony na maszyn wirtualnych Azure, pliki bazy danych już znajdują się na dyskach danych platformy Azure. Te dyski na żywo w magazynie obiektów Blob platformy Azure. Dlatego powody wykonywanie kopii zapasowej bazy danych i metod potrwać zmień nieco. Należy rozważyć następujące kwestie. 

- Nie trzeba wykonywać kopie zapasowe bazy danych w celu zapewnienia ochrony przed awarii sprzętu lub multimediów, ponieważ Microsoft Azure zapewnia tej ochrony w ramach usługi Microsoft Azure.

- Nadal potrzebujesz do wykonywania kopii zapasowych bazy danych w celu zapewnienia ochrony przed błędy użytkownika lub archiwizacji, przepisami powodów, dla których lub celów administracyjnych.

- Plik kopii zapasowej można przechowywać bezpośrednio w Azure. Aby uzyskać więcej informacji zobacz następujące sekcje, które umożliwiają uzyskanie informacji w różnych wersjach programu SQL Server.

## <a name="sql-server-2016"></a>Program SQL Server 2016

Microsoft SQL Server 2016 obsługuje [Kopia zapasowa i przywracanie z obiektami blob Azure](https://msdn.microsoft.com/library/jj919148.aspx) funkcji dostępnych w 2014 r programu SQL Server. Ale również zawiera następujące udoskonalenia:

| Rozszerzenie 2016               | Szczegóły                          |
|---------------------|-------------------------------|
| **Rozkładanie**              | Podczas wykonywania kopii zapasowej magazynem obiektów blob platformy Microsoft Azure, SQL Server 2016 obsługuje wykonywanie kopii zapasowych wielu obiektów blob umożliwiających wykonywanie kopii zapasowej dużych baz danych, maksymalnie 12,8 TB.      |
| **Kopii zapasowej migawki**                | Za pomocą migawek Azure kopii zapasowej migawki pliku programu SQL Server udostępnia prawie chwilową kopii zapasowych i przywracania szybkiego przechowywane za pomocą usługi Magazyn obiektów Blob platformy Azure plików bazy danych. Ta funkcja umożliwia uprościć kopii zapasowych i przywracanie zasad. Kopii zapasowej migawki pliku obsługuje także punktu w Przywróć czasu. Aby uzyskać więcej informacji zobacz [Migawkę kopie zapasowe plików bazy danych platformy Azure](https://msdn.microsoft.com/library/mt169363%28v=sql.130%29.aspx).   |
| **Zarządzane, Planowanie kopii zapasowej**            | Serwer zarządzania kopia zapasowa SQL Azure obsługuje teraz harmonogramów niestandardowe. Aby uzyskać więcej informacji zobacz [Program SQL Server zarządzane kopia zapasowa Microsoft Azure](https://msdn.microsoft.com/library/dn449496.aspx).   |

Samouczek możliwości programu SQL Server 2016 podczas korzystania z magazynem obiektów Blob platformy Azure, zobacz [Samouczek: przy użyciu usługi Magazyn obiektów Blob platformy Azure firmy Microsoft z bazami danych programu SQL Server 2016](https://msdn.microsoft.com/library/dn466438.aspx).

## <a name="sql-server-2014"></a>Program SQL Server 2014 r.

Program SQL Server 2014 zawiera następujące ulepszenia:

1. **Wykonywanie kopii zapasowych i przywracanie Azure**:

 - *Wykonywanie kopii zapasowych programu SQL Server do adresu URL* zawiera teraz pomocy technicznej w programie SQL Server Management Studio. Opcja Utwórz kopię zapasową Azure teraz jest dostępna, podczas korzystania z kopii zapasowej lub przywracania zadania lub kreatora w programie SQL Server Management Studio. Aby uzyskać więcej informacji zobacz [Narzędzia Kopia zapasowa programu SQL Server do adresu URL](https://msdn.microsoft.com/library/jj919148%28v=sql.120%29.aspx).
 - *Program SQL Server zarządzane kopii zapasowej Azure* zawiera nowe funkcje, które umożliwiają automatyzację zarządzania kopii zapasowej. To jest szczególnie przydatne dla Automatyzowanie kopii zapasowej zarządzania wystąpienia programu SQL Server 2014 uruchomione na komputerze Azure. Aby uzyskać więcej informacji zobacz [Program SQL Server zarządzane kopia zapasowa Microsoft Azure](https://msdn.microsoft.com/library/dn449496%28v=sql.120%29.aspx).
 - *Automatyczne wykonywanie kopii zapasowych* zawiera dodatkowe automatyzacji automatycznie włączyć *Serwera zarządzania kopia zapasowa SQL Azure* na wszystkich istniejących i nowych baz danych dla programu SQL Server maszyny platformy Azure. Aby uzyskać więcej informacji zobacz [Automatyczne wykonywanie kopii zapasowych dla programu SQL Server w maszyn wirtualnych Azure](virtual-machines-windows-sql-automated-backup.md).
 - Aby uzyskać omówienie wszystkie opcje kopii zapasowej 2014 serwera SQL Azure zobacz [SQL Server wykonywanie kopii zapasowych i przywracanie przy użyciu usługi Magazyn obiektów Blob platformy Azure Microsoft](https://msdn.microsoft.com/library/jj919148%28v=sql.120%29.aspx).

1. **Szyfrowanie**: SQL Server 2014 obsługuje szyfrowanie danych podczas tworzenia kopii zapasowej. Obsługuje kilka algorytmy szyfrowania i użyj osf certyfikatu lub klucza asymetrycznym. Aby uzyskać więcej informacji zobacz [Szyfrowanie kopii zapasowej](https://msdn.microsoft.com/library/dn449489%28v=sql.120%29.aspx).

## <a name="sql-server-2012"></a>Program SQL Server 2012

Aby uzyskać szczegółowe informacje dotyczące programu SQL Server wykonywanie kopii zapasowych i ich przywracanie w programu SQL Server 2012 zobacz [Wykonywanie kopii zapasowych i przywracanie z serwera bazy danych SQL (SQL Server 2012)](https://msdn.microsoft.com/library/ms187048%28v=sql.110%29.aspx).

Począwszy od programu SQL Server 2012 z dodatkiem SP1 aktualizacji skumulowanej 2, można utworzyć kopię zapasową i przywrócić z usługi Magazyn obiektów Blob platformy Azure. To rozszerzenie umożliwia wykonywanie kopii zapasowej bazy danych programu SQL Server w programie SQL Server uruchomionych maszyn wirtualnych Azure lub wystąpienia lokalnego. Aby uzyskać więcej informacji zobacz [SQL Server wykonywanie kopii zapasowych i przywracanie z usługi Magazyn obiektów Blob platformy Azure](https://msdn.microsoft.com/library/jj919148%28v=sql.110%29.aspx).

Korzyści wynikające z używania usługi Magazyn obiektów Blob platformy Azure należą możliwość obejścia limit 16 dysku dysków dołączonych łatwiejsze zarządzanie bezpośredni dostępność pliku kopii zapasowej do innego wystąpienia programu SQL Server uruchomionych Azure maszyn wirtualnych lub wystąpienia lokalnego migracji lub operacji odzyskiwania. Aby uzyskać pełną listę korzyści wynikające z używania usługi Magazyn obiektów blob platformy Azure kopii zapasowych programu SQL Server zobacz sekcję *korzyści* w [SQL Server wykonywanie kopii zapasowych i przywracanie z usługi Magazyn obiektów Blob platformy Azure](https://msdn.microsoft.com/library/jj919148%28v=sql.110%29.aspx).

Za najbardziej skuteczne rozwiązanie zalecenia i informacje dotyczące rozwiązywania problemów Zobacz [Wykonywanie kopii zapasowych i przywracanie najważniejsze wskazówki (usługa Magazyn obiektów Blob platformy Azure)](https://msdn.microsoft.com/library/jj919149%28v=sql.110%29.aspx).

## <a name="sql-server-2008"></a>Program SQL Server 2008

Kopia zapasowa programu SQL Server i przywracanie w SQL Server 2008 R2 zobacz [wykonywania kopii zapasowych i przywracanie baz danych programu SQL Server (program SQL Server 2008 R2)](https://msdn.microsoft.com/library/ms187048%28v=sql.105%29.aspx).

SQL Server wykonywanie kopii zapasowych i przywracania programu SQL Server 2008 zobacz [wykonywania kopii zapasowych i przywracanie baz danych programu SQL Server (program SQL Server 2008)](https://msdn.microsoft.com/library/ms187048%28v=sql.100%29.aspx).

## <a name="next-steps"></a>Następne kroki

Jeśli planujesz wdrożenia programu SQL Server Azure maszyn wirtualnych obsługi administracyjnej wskazówki można znaleźć w następujących instrukcji: [inicjowania obsługi administracyjnej maszyny wirtualnej serwera SQL Azure przy użyciu Menedżera zasobów Azure](virtual-machines-windows-portal-sql-server-provision.md).

Chociaż Kopia zapasowa i przywracanie można przeprowadzić migrację danych, dostępne są potencjalnie łatwiejsze ścieżki migracji danych do programu SQL Server na maszyn wirtualnych Azure. Pełne omówienie opcji migracji i zalecenia zobacz [Migrowanie bazy danych programu SQL Server na maszyn wirtualnych Azure](virtual-machines-windows-migrate-sql.md).

Przejrzyj inne [Zasoby dotyczące uruchomiony program SQL Server w maszyn wirtualnych Azure](virtual-machines-windows-sql-server-iaas-overview.md).
