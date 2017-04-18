<properties
   pageTitle="Wytyczne techniczne elastyczność odzyskiwania z przypadkowego usunięcia lub uszkodzenia danych | Microsoft Azure"
   description="Artykuł na uzyskiwanie danych uszkodzenie danych lub usunięcia przypadkowe danych i projektowanie aplikacji na uszkodzenia błędów mechanizm, wysokiej dostępności, a także planowania awarii"
   services=""
   documentationCenter="na"
   authors="adamglick"
   manager="saladki"
   editor=""/>

<tags
   ms.service="resiliency"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/18/2016"
   ms.author="aglick"/>

#<a name="azure-resiliency-technical-guidance-recovery-from-data-corruption-or-accidental-deletion"></a>Wytyczne techniczne Azure elastyczność: przypadkowego usunięcia lub uszkodzenia danych

Część niezawodne ciągłości Biznesplan ma planu, jeśli danych jest uszkodzona lub przypadkowemu usunięciu. Oto informacji na temat odzyskiwania po danych został uszkodzony lub przypadkowemu usunięciu z powodu błędu operator lub błędy aplikacji.

##<a name="virtual-machines"></a>Maszyn wirtualnych

Aby chronić Azure maszyn wirtualnych (nazywane pośrednictwem infrastruktury jako usługi SMS) z błędów aplikacji lub przypadkowym usunięciom, używanie [Narzędzia Kopia zapasowa Azure](https://azure.microsoft.com/services/backup/). Kopia zapasowa Azure umożliwia tworzenie kopii zapasowych, które są spójne na wielu dyskach maszyn wirtualnych. Ponadto magazynu kopii zapasowej mogą być replikowane między różnymi regionami o podanie odzyskiwania przed utratą region.

##<a name="storage"></a>Miejsca do magazynowania

Należy zauważyć, że magazyn Azure zapewnia elastyczność danych za pośrednictwem repliki automatyczną, jednocześnie nie ogranicza usługi kod aplikacji (lub deweloperów użytkownicy) na uszkodzenia danych przez usunięcie przypadkowe lub niezamierzonych, aktualizacji i tak dalej. Zachowywanie wierności danych obliczu błędu aplikację lub użytkownika wymaga bardziej zaawansowanych technik, takie jak kopiowanie danych do lokalizacji przechowywania pomocniczej z dziennika inspekcji. Deweloperów można korzystać z [możliwości migawkę](https://msdn.microsoft.com/library/azure/ee691971.aspx)obiektów blob, co może prowadzić do migawki w chwili tylko do odczytu treści obiektów blob. To może służyć jako podstawa rozwiązanie wierności danych dla obiektów blob platformy Azure miejsca do magazynowania.

###<a name="blob-and-table-storage-backup"></a>Obiektów blob i kopia zapasowa miejsca do magazynowania tabeli

BLOB i tabele są bardzo trwałe, zawsze przedstawiają bieżący stan danych. Modyfikacji lub usunięcie danych może wymagać przywracanie danych do poprzedniego stanu. Można to osiągnąć wykorzystując możliwości oferowane przez Azure do przechowywania i zachować kopie w chwili.

Dla obiektów blob platformy Azure można wykonywać kopie zapasowe w chwili przy użyciu [funkcji migawki obiektów blob](https://msdn.microsoft.com/library/ee691971.aspx). Dla każdej migawki są tylko naliczane do przechowywania wymaganych do przechowania różnice w to od czasu ostatniego migawki stanu. Migawki są zależne od istnienia oryginalny obiektów blob, które są one oparte na, więc operacji kopiowania do innej obiektów blob lub nawet innego konta miejsca do magazynowania jest zalecane. Dzięki temu właściwą ochronę przed przypadkowym usunięciem danych kopii zapasowej. W przypadku tabel Azure można wykonać kopie w chwili do innej tabeli lub do obiektów blob platformy Azure. Bardziej szczegółowe wskazówki i przykłady dotyczące wykonywania kopii zapasowych poziomie aplikacji tabel i obiektów blob można znaleźć tutaj:

  * [Ochrona tabel przed błędów aplikacji](https://blogs.msdn.microsoft.com/windowsazurestorage/2010/05/03/protecting-your-tables-against-application-errors/)
  * [Ochrona z obiektami blob przed błędów aplikacji](https://blogs.msdn.microsoft.com/windowsazurestorage/2010/04/29/protecting-your-blobs-against-application-errors/)

##<a name="database"></a>Bazy danych

Istnieje kilka opcji [ciągłości](../sql-database/sql-database-business-continuity.md) (kopii zapasowej, przywracanie) dostępne dla bazy danych SQL Azure. Używając funkcji [Kopii bazy danych](../sql-database/sql-database-copy.md) lub [Eksportowanie](../sql-database/sql-database-export.md) i [Importowanie](https://msdn.microsoft.com/library/hh710052.aspx) pliku Plecak programu SQL Server można kopiować baz danych. Kopia bazy danych zawiera transakcyjnie spójne wyniki, a Plecak (za pośrednictwem usługi Importuj/Eksportuj) nie. Obu tych opcji Uruchom jako opartych na kolejkach usług w centrum danych, a nie zapewniają obecnie SLA czas do zakończenia.

>[AZURE.NOTE]Opcje kopiowania i Importuj/Eksportuj bazy danych umieść znacznym stopniu obciążenia na źródłowej bazy danych. Mogą wyzwalać konfliktu zasobów lub ograniczania zdarzeń.

###<a name="sql-database-backup"></a>Kopia zapasowa bazy danych SQL

Kopii zapasowych w chwili Microsoft Azure SQL Database zostaną osiągnięte, [kopiując bazy danych Azure SQL](../sql-database/sql-database-copy.md). To polecenie umożliwia tworzenie transakcyjnie spójne kopii bazy danych na tym samym serwerze logiczne bazy danych lub na inny serwer. W obu przypadkach kopia bazy danych jest w pełni funkcjonalny i całkowicie niezależny źródłowej bazy danych. Skoroszyty utworzone reprezentuje opcję odzyskiwania w chwili. Stan bazy danych można odzyskać całkowicie przez zmianę nazwy nowej bazy danych Nazwa źródłowej bazy danych. Można też kliknąć można odzyskać określony podzestaw danych z nowej bazy danych za pomocą zapytań w języku Transact-SQL. Aby uzyskać dodatkowe informacje na temat bazy danych SQL zobacz [Omówienie ciągłości z bazy danych SQL Azure](../sql-database/sql-database-business-continuity.md).

###<a name="sql-server-on-virtual-machines-backup"></a>Program SQL Server w środowisku maszyn wirtualnych systemu kopii zapasowej

Dla programu SQL Server używany z infrastrukturą Azure jako maszyn wirtualnych usługi (często nazywane IaaS lub IaaS maszyny wirtualne), dostępne są dwie opcje: wysyłanie dziennika i tradycyjny kopie zapasowe. Przy użyciu tradycyjnego kopie zapasowe umożliwia przywracanie do określonego punktu w czasie, ale proces odzyskiwania jest powolne. Przywracanie kopii zapasowych tradycyjnych wymaga począwszy od wstępnej pełnej kopii zapasowej, a następnie stosując wszelkie kopie zapasowe wykonane po. Jest to druga opcja jest skonfigurowanie dziennika wysyłki sesji opóźnienia przywracania kopii zapasowych dziennika (na przykład o dwie godziny). Dzięki temu okna, aby naprawić błędy w podstawowych.

##<a name="other-azure-platform-services"></a>Inne usługi platformy Azure

Niektóre usługi platformy Azure przechowywania informacji konta magazynu kontrolowane przez użytkownika lub bazy danych SQL Azure. Jeśli konto lub miejsca do magazynowania zasobu jest usunięte lub uszkodzony, może to spowodować poważne błędy z usługą. W tych przypadkach należy zachować kopie zapasowe umożliwiające chcesz ponownie utworzyć te zasoby, jeśli zostały usunięte lub uszkodzony.

Dla witryny sieci Web Azure i usługi Azure Mobile możesz utworzyć kopię zapasową i zachować skojarzone bazy danych. Dla usługi multimediów Azure i maszyn wirtualnych możesz zachować wszystkie zasoby tego konta i skojarzonego konta magazynu platformy Azure. Na przykład dla maszyn wirtualnych musi wykonywanie kopii zapasowej i zarządzanie nimi dysków maszyn wirtualnych w magazynie obiektów blob platformy Azure.

##<a name="checklists-for-data-corruption-or-accidental-deletion"></a>Listy kontrolne przypadkowego usunięcia lub uszkodzenia danych

##<a name="virtual-machines-checklist"></a>Lista kontrolna maszyn wirtualnych

  1. Zapoznaj się z sekcją maszyn wirtualnych tego dokumentu.
  2. Wykonywanie kopii zapasowej i przechowywanie dysków maszyn wirtualnych z kopii zapasowej Azure (lub kopii zapasowej systemu przy użyciu magazynem obiektów blob Azure i migawek wirtualnego dysku twardego).

##<a name="storage-checklist"></a>Lista kontrolna miejsca do magazynowania

  1. Zapoznaj się z sekcją miejsca do magazynowania w niniejszym dokumencie.
  2. Regularnie wykonywać kopie zapasowe zasobów magazynowania krytyczne.
  3. Warto rozważyć użycie funkcji migawki dla obiektów blob.

##<a name="database-checklist"></a>Lista kontrolna bazy danych

  1. Zapoznaj się z sekcją bazy danych tego dokumentu.
  2. Tworzenie kopii zapasowych w chwili przy użyciu polecenia Kopiuj bazy danych.

##<a name="sql-server-on-virtual-machines-backup-checklist"></a>Program SQL Server na kopii zapasowych maszyn wirtualnych — Lista kontrolna

  1. Przejrzyj programu SQL Server w środowisku maszyn wirtualnych systemu kopii zapasowej część tego dokumentu.
  2. Używać tradycyjnych kopii zapasowych i przywracania technik.
  3. Tworzenie dziennika opóźnione wysyłki sesji.

##<a name="web-apps-checklist"></a>Lista kontrolna aplikacji sieci Web

  1. Wykonywanie kopii zapasowej i przechowywanie powiązanej bazy danych, jeśli istnieją.

##<a name="media-services-checklist"></a>Lista kontrolna dotycząca usługi multimediów

  1. Wykonywanie kopii zapasowych i obsługa zasobów skojarzone miejsca do magazynowania.

##<a name="more-information"></a>Więcej informacji

Aby uzyskać więcej informacji o funkcjach i przywracania kopii zapasowych w Azure zobacz [miejsca do magazynowania, scenariusze i przywracania kopii zapasowych](https://azure.microsoft.com/documentation/scenarios/storage-backup-recovery/).

##<a name="next-steps"></a>Następne kroki

Ten artykuł jest częścią serii ograniczony do [wytycznych technicznych elastyczność Azure](./resiliency-technical-guidance.md). Jeśli szukasz więcej elastyczność odzyskiwanie i wysokiej dostępności zasobów, zobacz wskazówki techniczne Azure elastyczność [dodatkowe zasoby](./resiliency-technical-guidance.md#additional-resources).