<properties
   pageTitle="Indeks wskazówki techniczne elastyczność | Microsoft Azure"
   description="Indeks artykułów na opis i projektowanie mechanizm, wysokiej dostępności, odporność na uszkodzenia aplikacji, a także planowania awarii ciągłości odzyskiwania i małych firm"
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

#<a name="azure-resiliency-technical-guidance"></a>Wytyczne techniczne elastyczność Azure

##<a name="introduction"></a>Wprowadzenie

Spotkania wysokiej dostępności i wymagania dotyczące odzyskiwania systemu wymaga dwóch rodzajów wiedzy:

- Szczegółowy opis technicznych możliwości platformie chmury
- Informacje o sposobach prawidłowo zaprojektować rozłożone usługi

Pierwsza obejmuje serii artykułów: funkcje i ograniczenia dotyczące platformy Azure dotyczące elastyczność (nazywane również Zapewnianie ciągłości). Jeśli Interesujesz się one, zapoznaj się z serii artykuł dotyczyła [Odzyskiwanie i wysoką dostępność dla aplikacji Azure](https://aka.ms/drtechguide). Mimo że tej serii artykuł dotykającego na desenie architektury i projektowania, to nie fokusu serii. Aby uzyskać wskazówki dotyczące projektowania zwróć się materiały w sekcji [dodatkowe zasoby](#additional-resources) .

Informacje są zorganizowane w następujących artykułach:

- [Odzyskiwanie z błędy lokalne](resiliency-technical-guidance-recovery-local-failures.md).
Sprzętu (na przykład dyski, serwerów i urządzeń sieciowych) może się nie powieść. Może ulec wyczerpaniu zasobów, podczas ładowania wzrósł. W tym artykule opisano możliwości, jakie Azure przekazuje obsługa wysokiej dostępności pod tymi warunkami.

- [Odzyskiwanie z zakłócenia Azure usługi region](resiliency-technical-guidance-recovery-loss-azure-region.md).
Błędy szerokie rzadkich, ale są one teoretycznie możliwe. Cały regionów może stać się odizolowane z powodu błędów sieci lub może być fizycznie uszkodzony z np. W tym artykule wyjaśniono, jak używać Azure do tworzenia aplikacji geograficznie różnych regionów.

- [Odzyskiwanie ze źródeł lokalnych Azure](resiliency-technical-guidance-recovery-on-premises-azure.md).
Chmura znacznie zmienia Zarządzanie odzyskiwania systemu, umożliwia organizacjom utworzyć drugą witrynę odzyskiwania za pomocą Azure. Można to zrobić na ułamek kosztów tworzenia i utrzymywania pomocniczej centrum danych. W tym artykule opisano możliwości, jakie Azure znajdują się rozszerzenia centrum danych lokalnych z chmurą.

- [Przypadkowego usunięcia lub uszkodzenia danych](resiliency-technical-guidance-recovery-data-corruption.md).
Aplikacje może mieć usterek uszkodzenie danych. Operatory niepoprawnie usunąć ważnych danych. W tym artykule wyjaśniono, Azure udostępnia tworzenia kopii zapasowych danych i przywracanie do poprzedniego punktu jego czasu.

##<a name="additional-resources"></a>Dodatkowe zasoby

- [Odzyskiwanie i wysoką dostępność dla aplikacji utworzonych na platformy Microsoft Azure](resiliency-disaster-recovery-high-availability-azure-applications.md).
Ten artykuł dotyczy szczegółowe omówienie dostępności i awarii. Obejmuje wezwanie ręczna replikacja dla transakcji danych i odwołania. Ostateczne sekcjach podsumowania różnego rodzaju topologii odzyskiwanie danych, obejmujące Azure regionów najwyższego poziomu dostępności.

- [Lista kontrolna wysokiej dostępności](resiliency-high-availability-checklist.md).
W tym artykule przedstawiono listę funkcji, usług, i projektów, które mogą pomóc zwiększyć elastyczność i dostępności aplikacji.

- [Wskazówki dotyczące ochrony usługi Microsoft Azure](resiliency-service-guidance-index.md).
Ten artykuł jest indeks usług Azure i łącza do zarówno orientacji odzyskiwanie danych, jak i wskazówki dotyczące projektowania.

- [Omówienie: firm ciągłości i bazy danych odzyskiwanie z bazą danych SQL w chmurze](../sql-database/sql-database-business-continuity.md).
Ten artykuł zawiera technik bazy danych SQL Azure dostępności. Przede wszystkim wyśrodkowanie strategii i przywracania kopii zapasowych. Jeśli korzystasz z bazy danych SQL Azure w usłudze w chmurze, należy zapoznać się w tym dokumencie i jego zasoby pokrewne.

- [Wysoką dostępność i odzyskiwanie w przypadku programu SQL Server w maszyn wirtualnych Azure](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md).
W tym artykule opisano opcje dostępności, przedstawiających użycie infrastruktury jako usługa (IaaS) do obsługi usługi bazy danych. Go w tym artykule omówiono grupy dostępności (AlwaysOn), odzwierciedlające bazy danych, wysyłanie dziennika i tworzenie kopii zapasowych i przywracanie. Kilkoma samouczki pokazano, jak za pomocą tych metod.

- [Najważniejsze wskazówki dotyczące projektowania na dużą skalę usług Azure usług w chmurze](https://azure.microsoft.com//blog/best-practices-for-designing-large-scale-services-on-windows-azure/).
W tym artykule omówiono opracowywania architektury wysoce skalowalna chmury. Wiele technik opartych na poprawić skalowalność również zwiększyć dostępność. Ponadto jeśli aplikacji nie mogę przeskalować obciążeniu lepszą, skalowalność staje się problem dostępności.

- [Kopia zapasowa i przywracanie dla programu SQL Server w maszyn wirtualnych Azure](../virtual-machines/virtual-machines-windows-sql-backup-recovery.md).
Ten artykuł zawiera wskazówki techniczne dotyczące tworzenia kopii zapasowych i przywracanie działa na Azure środowisku maszyn wirtualnych systemu Microsoft SQL Server.

- [Odporność: wskazówki dotyczące architektury mechanizm chmury](https://channel9.msdn.com/Series/FailSafe).
Ten artykuł zawiera wskazówki dotyczące tworzenia architektury mechanizm chmury, wskazówki dotyczące stosowania tych architektury na technologii firmy Microsoft i przepisy stosowania tych architektury w określonych scenariuszach.

- [Techniczne analiza przypadku: przy użyciu technologii chmurowych, aby poprawić odzyskiwanie](https://www.microsoft.com/itshowcase/Article/Content/737/Using-cloud-technologies-to-improve-disaster-recovery).
Ten przykład zastosowania pokazano, jak Microsoft IT użyć Azure aby poprawić awarii.

##<a name="next-steps"></a>Następne kroki

Ten artykuł jest częścią serii dotyczyła techniczne wytyczne dotyczące ochrony Azure. Jeśli interesują Cię inne artykuły w tej serii do czytania, można rozpocząć od [odzyskiwania błędy lokalne](resiliency-technical-guidance-recovery-local-failures.md).
