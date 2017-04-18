<properties
    pageTitle="Wykonaj kopię zapasową baz danych z obsługą rozciąganie | Microsoft Azure"
    description="Dowiedz się, jak utworzyć kopię zapasową rozciąganie\-włączone baz danych."
    services="sql-server-stretch-database"
    documentationCenter=""
    authors="douglaslMS"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-server-stretch-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/14/2016"
    ms.author="douglasl"/>

# <a name="backup-stretch-enabled-databases"></a>Kopii zapasowej bazy danych, które są włączone rozciąganie

Kopie zapasowe baz danych ułatwiają odzyskiwanie z wielu typów błędów, błędów i awarii.  

-   Masz do tworzenia kopii zapasowych usługi rozciąganie\-włączone bazy danych programu SQL Server.  

-   Microsoft Azure automatycznie tworzy kopię zapasową bazy danych rozciąganie występują migracji z programu SQL Server Azure dane zdalne.  

>    [AZURE.NOTE] Kopia zapasowa jest tylko jednej części pełną wysoką dostępność i rozwiązania biznesowego ciągłości. Aby uzyskać więcej informacji o wysokiej dostępności zobacz [Wysoka dostępność rozwiązań](https://msdn.microsoft.com/library/ms190202.aspx).

## <a name="back-up-your-sql-server-data"></a>Kopie zapasowe danych programu SQL Server  

Do tworzenia kopii zapasowych usługi rozciąganie\-włączone bazy danych programu SQL Server, możesz nadal korzystać z metod kopii zapasowej programu SQL Server, które obecnie dostępne. Aby uzyskać więcej informacji zobacz [kopii zapasowych i przywracanie z serwera bazy danych programu SQL](https://msdn.microsoft.com/library/ms187048.aspx).

Kopie zapasowe bazy danych z obsługą rozciąganie programu SQL Server zawierają tylko dane lokalne oraz dane kwalifikujących się do migracji w punkcie w czasie, gdy jest uruchomiony kopii zapasowej. \(Uprawniony dane znajdują się dane, które nie ma jeszcze migracji, ale będzie migrowana Azure na podstawie ustawień migracji tabel.\) To jest znany jako kopię zapasową **skrócona** i nie zawiera danych już migracji Azure.  

## <a name="back-up-your-remote-azure-data"></a>Wykonywanie kopii zapasowej zdalnych danych Azure   

Microsoft Azure automatycznie tworzy kopię zapasową bazy danych rozciąganie występują migracji z programu SQL Server Azure dane zdalne.  

### <a name="azure-reduces-the-risk-of-data-loss-with-automatic-backup"></a>Azure zmniejsza ryzyko utraty danych z automatycznej kopii zapasowej  
Usługa bazy danych programu SQL Server rozciąganie Azure chroni zdalnego baz danych z migawki automatyczne przechowywanie co najmniej 8 godzin. Zachowuje każdego migawkę przez 7 dni do zapewnienia szeroką gamę możliwości punktów.  

### <a name="azure-reduces-the-risk-of-data-loss-with-geo-redundancy"></a>Azure zmniejsza ryzyko utraty danych z geo\-nadmiarowości  
Kopie zapasowe Azure bazy danych są przechowywane na geo\-zbędne magazyn Azure (Pomoc Zdalna\-GRS) i dlatego geo\-zbędne domyślnie. Geo\-zbędne miejsca do magazynowania replikuje dane na pomocniczym regionu, który jest setki mila poza podstawowym region. W regionach podstawowych i pomocniczych dane są replikowane trzy razy każdego w osobnych błędów domen i uaktualniania domen. Dzięki temu dane są trwałe nawet w przypadku wykonania awarii regionalne lub danych, które są renderowane za pomocą regionów Azure niedostępne.

### <a name="stretchRPO"></a>Rozciąganie bazy danych zmniejsza ryzyko utraty danych Azure danych przez tymczasowo Zachowywanie wierszy migracją
Po rozciąganie bazy danych migruje wierszy uprawniony do korzystania z programu SQL Server Azure, zachowuje tych wierszy w tabeli etapów, przez co najmniej 8 godzin. Jeśli zapasowej Azure bazy danych bazy danych rozciąganie używa wierszy zapisanych w tabeli etapów uzgodnić programu SQL Server i Azure baz danych.

Po przywróceniu kopii zapasowej Azure danych, należy uruchomić procedura składowana [sys.sp_rda_reauthorize_db](https://msdn.microsoft.com/library/mt131016.aspx) , aby ponownie nawiązać połączenie rozciąganie\-włączone bazy danych programu SQL Server Azure zdalna baza danych. Po uruchomieniu **sys.sp_rda_reauthorize_db**bazy danych rozciąganie druga automatycznie programu SQL Server i Azure baz danych.

Aby zwiększyć liczbę godzin migracją danych bazy danych rozciąganie tymczasowo zachowuje się w tabeli etapów, uruchom procedura składowana [sys.sp_rda_set_rpo_duration](https://msdn.microsoft.com/library/mt707766.aspx) i określ liczbę godzin wartość większą niż 8. Określanie, ile przechowywania danych, należy rozważyć następujące kwestie:
-   Częstotliwość automatycznych kopii zapasowych Azure (co najmniej raz 8 godzin).
-   Czas wymagany po problem rozpoznanie problemu i zdecydować przywrócić kopię zapasową.
-   Czas trwania operacji przywracania Azure.

> [AZURE.NOTE] Zwiększanie ilości danych bazy danych rozciąganie tymczasowo zachowuje się w tabeli etapów zwiększa ilość miejsca na serwerze SQL.

Aby sprawdzić liczbę godzin danych bazy danych rozciąganie obecnie zachowuje tymczasowo w tabeli etapów, uruchom procedura składowana [sys.sp_rda_get_rpo_duration](https://msdn.microsoft.com/library/mt707767.aspx).

## <a name="see-also"></a>Zobacz też

[Zarządzanie i rozwiązywanie problemów z rozciąganie bazy danych](sql-server-stretch-database-manage.md)

[sys.sp_rda_reauthorize_db (w języku Transact-SQL)](https://msdn.microsoft.com/library/mt131016.aspx)

[Wykonywanie kopii zapasowych i przywracanie bazy danych programu SQL Server](https://msdn.microsoft.com/library/ms187048.aspx)
