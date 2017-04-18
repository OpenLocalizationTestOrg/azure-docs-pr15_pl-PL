<properties
   pageTitle="Ładowanie danych do magazynu danych SQL za pomocą Studio platformy danych firmy Redgate | Microsoft Azure"
   description="Dowiedz się, jak za pomocą Studio platformy danych firmy Redgate dla scenariuszy magazynowanie danych."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="twounder"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="10/13/2016"
   ms.author="mausher;barbkess"/>


# <a name="load-data-with-redgate-data-platform-studio"></a>Ładowanie danych za pomocą Redgate danych platformy Studio

> [AZURE.SELECTOR]
- [Redgate](sql-data-warehouse-load-with-redgate.md)
- [Factory danych](sql-data-warehouse-get-started-load-with-azure-data-factory.md)
- [PolyBase](sql-data-warehouse-get-started-load-with-polybase.md)
- [BCP](sql-data-warehouse-load-with-bcp.md)

Ten samouczek pokazano, jak przenieść dane z programu SQL Server lokalnego magazynu danych SQL Azure za pomocą [Studio platformy danych firmy Redgate](http://www.red-gate.com/products/azure-development/data-platform-studio/) (DPS). Studio platformy danych dotyczy najbardziej odpowiednie poprawki zgodności i optymalizacje, aby była najszybszym sposobem, aby rozpocząć pracę z magazynu danych SQL.

> [AZURE.NOTE] [Redgate](http://www.red-gate.com) jest interesują partnera firmy Microsoft, która zapewnia różne narzędzia programu SQL Server. Ta funkcja w danych platformy Studio udostępniono swobodnego do użytku komercyjnego i niekomercyjnego.

## <a name="before-you-begin"></a>Przed rozpoczęciem
### <a name="create-or-identify-resources"></a>Tworzenie lub zidentyfikuj zasoby

Przed rozpoczęciem tego samouczka, muszą być:

- **W wersji lokalnej bazy danych serwera SQL**: dane mają zostać zaimportowane do magazynu danych SQL muszą pochodzić z lokalnego programu SQL Server (2008R2 wersję lub nowszy). Studio platformy danych nie można importować dane bezpośrednio z bazy danych SQL Azure lub z plików tekstowych.
- **Konta na platformie Azure**: danych platformy Studio etapach danych w magazynie obiektów Blob platformy Azure przed ładowania go do magazynu danych SQL. Zamiast "Klasycznego" model wdrożenia konta miejsca do magazynowania musi korzystać z modelu wdrażania "Menedżera zasobów" (ustawienie domyślne). Jeśli nie masz konta miejsca do magazynowania, Dowiedz się, jak utworzyć konto miejsca do magazynowania. 
- **Program SQL Data Warehouse**: ten samouczek przenosi dane z lokalnego programu SQL Server do magazynu danych SQL, trzeba mieć magazynu danych w trybie online. Jeśli nie masz już magazynu danych, Dowiedz się, jak utworzyć magazynu danych SQL Azure.

> [AZURE.NOTE] Zwiększona wydajność, jeśli konto miejsca do magazynowania i magazynu danych są tworzone w tym samym regionie.

## <a name="step-1-sign-in-to-data-platform-studio-with-your-azure-account"></a>Krok 1: Logowanie się do danych platformy Studio za pomocą konta usługi Azure
Otwórz przeglądarkę sieci web i przejdź do witryny sieci Web [Studio platformy danych](https://www.dataplatformstudio.com/) . Zaloguj się przy użyciu tego samego konta Azure, który został użyty do utworzenia magazyn przechowywania konta i dane. Jeśli Twój adres e-mail jest skojarzony z utworu lub konta służbowego i konta Microsoft, należy koniecznie wybierz konto, które ma dostęp do zasobów.

> [AZURE.NOTE] Jeśli jest używany po raz pierwszy za pomocą danych platformy Studio, zostanie wyświetlony monit o udzielanie uprawnień aplikacji do zarządzania zasobami Azure.

## <a name="step-2-start-the-import-wizard"></a>Krok 2: Uruchomić Kreatora importu
Na ekranie głównym DPS wybierz Importuj do magazynu danych SQL Azure łącza, aby uruchomić Kreatora importu.

![][1]

## <a name="step-3-install-the-data-platform-studio-gateway"></a>Krok 3: Instalowanie bramy Studio platformy danych
Aby połączyć się z bazą danych programu SQL Server w wersji lokalnej, należy zainstalować bramę DPS. Brama jest agenta klienta, który umożliwia dostęp do środowiska lokalnego, wyodrębnia dane i przekazuje go do swojego konta miejsca do magazynowania. Dane nigdy nie przechodzi przez serwery Redgate osoby. Aby zainstalować bramy:

1.  Kliknij łącze **Utwórz bramy**
2. Pobieranie i instalowanie bramy przy użyciu podanych Instalatora

![][2]

> [AZURE.NOTE] Brama można zainstalować na dowolnym komputerze z dostępem do sieci źródłową bazą danych programu SQL Server. Jego uzyskuje dostęp do bazy danych programu SQL Server przy użyciu funkcji uwierzytelniania systemu Windows przy użyciu poświadczeń bieżącego użytkownika.

Po zainstalowaniu stanu bramy zmieni się połączony i czy wybierz przycisk Dalej.

## <a name="step-4-identify-the-source-database"></a>Krok 4: Identyfikowanie źródłowej bazy danych
W polu tekstowym *Wprowadź nazwę serwera* wprowadź nazwę serwera, który obsługuje bazy danych, a następnie wybierz przycisk **Dalej**. Następnie z menu rozwijanego wybierz do zaimportowania danych z bazy danych.

![][3]

DPS sprawdza wybranej bazy danych dla tabel do zaimportowania. Domyślnie DPS importuje wszystkie tabele w bazie danych. Czy zaznacz lub Wyczyść tabele po rozwinięciu łącze wszystkich tabel. Wybierz przycisk Dalej, aby przejść do przodu.

## <a name="step-5-choose-a-storage-account-to-stage-the-data"></a>Krok 5: Wybierz konto miejsca do magazynowania do etapu danych
DPS monituje o lokalizację do etapu danych. Wybierz istniejące konto miejsca do magazynowania z subskrypcji, a następnie wybierz przycisk **Dalej**.

> [AZURE.NOTE] DPS utworzy nowy kontener obiektów blob na koncie wybranego miejsca do magazynowania i używanie folderu usługi odrębnych dla każdego importu.

![][4]

## <a name="step-6-select-a-data-warehouse"></a>Krok 6: Wybierz magazyn danych
Następnie możesz wybrać bazy danych online [Magazynu danych SQL Azure](http://aka.ms/sqldw) zaimportować dane. Po wybraniu bazy danych, należy wprowadzić poświadczenia, aby połączenia z bazą danych, a następnie wybierz przycisk **Dalej**.

![][5]

> [AZURE.NOTE] DPS scala źródła danych w tabelach magazynu danych. DPS ostrzega, jeśli nazwa tabeli wymagają, aby zastąpić istniejące tabele w magazynie danych. Można opcjonalnie usunąć istniejących obiektów w magazynie danych usuń zaznaczenie wszystkich istniejących obiektów źródłowych.

## <a name="step-7-import-the-data"></a>Krok 7: Importowanie danych
DPS potwierdzenie, że chcesz zaimportować dane. Po prostu kliknij przycisk Importuj Start, aby rozpocząć importowanie danych.

![][6]

DPS Wyświetla wizualizacji, która przedstawia postęp wyodrębnianie i przekazywaniu danych z programu SQL Server w wersji lokalnej i postęp importowania do magazynu danych SQL.

![][7]

Po zakończeniu importu DPS Wyświetla podsumowanie importu danych i zmień raport poprawek zgodności, które zostały wykonane.

![][8]

## <a name="next-steps"></a>Następne kroki
Eksplorowanie danych w obrębie magazynu danych SQL, zacznij od wyświetlania:

- [Kwerenda Azure SQL Data Warehouse (Visual Studio)][]
- [Wizualizowanie danych za pomocą usługi Power BI][]

Aby dowiedzieć się więcej na temat Studio platformy danych firmy Redgate:

- [Odwiedź stronę główną DPS](http://www.dataplatformstudio.com/)
- [Obejrzyj pokaz DPS na Channel9](https://channel9.msdn.com/Blogs/cloud-with-a-silver-lining/Loading-data-into-Azure-SQL-Datawarehouse-with-Redgate-Data-Platform-Studio)

Aby uzyskać omówienie inne sposoby migrowania i ładowanie danych w magazynie danych SQL, zobacz:

- [Migrowanie rozwiązania do magazynu danych SQL][]
- [Ładowanie danych do magazynu danych SQL Azure](./sql-data-warehouse-overview-load.md)

Aby uzyskać więcej porad rozwoju zobacz [Omówienie tworzenia magazynu danych SQL](./sql-data-warehouse-overview-develop.md).

<!--Image references-->
[1]: media/sql-data-warehouse-redgate/2016-10-05_15-59-56.png
[2]: media/sql-data-warehouse-redgate/2016-10-05_11-16-07.png
[3]: media/sql-data-warehouse-redgate/2016-10-05_11-17-46.png
[4]: media/sql-data-warehouse-redgate/2016-10-05_11-20-41.png
[5]: media/sql-data-warehouse-redgate/2016-10-05_11-31-24.png
[6]: media/sql-data-warehouse-redgate/2016-10-05_11-32-20.png
[7]: media/sql-data-warehouse-redgate/2016-10-05_11-49-53.png
[8]: media/sql-data-warehouse-redgate/2016-10-05_12-57-10.png

<!--Article references-->
[Kwerenda Azure SQL Data Warehouse (Visual Studio)]: ./sql-data-warehouse-query-visual-studio.md
[Wizualizowanie danych za pomocą usługi Power BI]: ./sql-data-warehouse-get-started-visualize-with-power-bi.md
[Migrowanie rozwiązania do magazynu danych SQL]: ./sql-data-warehouse-overview-migrate.md
[Load data into Azure SQL Data Warehouse]: ./sql-data-warehouse-overview-load.md
[SQL Data Warehouse development overview]: ./sql-data-warehouse-overview-develop.md