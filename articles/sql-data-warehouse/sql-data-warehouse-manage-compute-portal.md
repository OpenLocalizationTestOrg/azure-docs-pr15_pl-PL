<properties
   pageTitle="Zarządzanie power obliczeń w magazynie danych SQL Azure (Azure portal) | Microsoft Azure"
   description="Azure portalu zadań w celu zarządzania obliczyć power. Skala obliczyć zasobów za pomocą dostosowania DWUs. Lub wstrzymywanie i wznawianie obliczenia kosztów zasobów."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/22/2016"
   ms.author="barbkess;sonyama"/>

# <a name="manage-compute-power-in-azure-sql-data-warehouse-azure-portal"></a>Zarządzanie power obliczeń w magazynie danych SQL Azure (Azure portal)

> [AZURE.SELECTOR]
- [Omówienie](sql-data-warehouse-manage-compute-overview.md)
- [Portal](sql-data-warehouse-manage-compute-portal.md)
- [Programu PowerShell](sql-data-warehouse-manage-compute-powershell.md)
- [POZOSTAŁE](sql-data-warehouse-manage-compute-rest-api.md)
- [TSQL](sql-data-warehouse-manage-compute-tsql.md)


Wydajność skali Skalowanie zewnętrzne obliczyć zasoby i pamięć do spełnienia wymagań zmiana z pracą. Zapisz kosztów według skalowania zasobów wstecz podczas godzin niezwiązanych Szczyt lub wstrzymywanie obliczeń całkowicie. 

Ten zbiór zadań używa portal Azure, aby:

- Skala obliczeń
- Wstrzymaj obliczeń
- Życiorys obliczeń

Aby uzyskać więcej informacji zobacz [Zarządzanie obliczyć Przegląd][].

<a name="scale-performance-bk"></a>
<a name="scale-compute-bk"></a>

## <a name="scale-compute-power"></a>Skala power obliczeń

[AZURE.INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

Aby zmienić obliczeń zasoby:

1. Otwórz [Azure portal][], otwórz bazę danych, a następnie kliknij pozycję **Skala**.

    ![Kliknij pozycję Skala][1]

1. W karta Skala Przesuń suwak w lewo lub w prawo zmienić ustawienie DWU.

    ![Przesuń suwak][2]

1. Kliknij przycisk **Zapisz**. Zostanie wyświetlony komunikat z potwierdzeniem. Kliknij przycisk **Tak,** aby potwierdzić lub **nie** anulowania.

    ![Kliknij przycisk Zapisz][3]

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a>Wstrzymaj obliczeń

[AZURE.INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

Aby wstrzymać bazy danych:

1. Otwórz [Azure portal][] i otwórz bazę danych. Zwróć uwagę, że jej stan to **Online**. 

    ![Stan online][6]

1. Aby zawiesić zasobów pamięci i obliczeń, kliknij przycisk **Wstrzymaj**, a następnie komunikat potwierdzający zostanie wyświetlone. Kliknij przycisk **Tak,** aby potwierdzić lub **nie** anulowania.

    ![Upewnij się, Wstrzymaj][7]

1. Podczas uruchamiania bazy danych SQL Data Warehouse, jej stan to **Wstrzymywanie**.
2. Jeśli jest w stanie **wstrzymania**, operacja Wstrzymaj zakończy się i już nie jest naliczany dla DWUs.

    ![Wstrzymaj stanu][4]

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a>Życiorys obliczeń

[AZURE.INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]Aby wznowić bazy danych:

1. Otwórz [Azure portal][] i otwórz bazę danych. Zwróć uwagę, że jej stan to **wstrzymana**. 

    ![Wstrzymaj bazy danych][4]

1. Aby wznowić bazy danych kliknij przycisk **Start**, a następnie pojawi się komunikat potwierdzający. Kliknij przycisk **Tak,** aby potwierdzić lub **nie** anulowania.

    ![Potwierdź życiorysu][5]

1. Podczas uruchamiania bazy danych SQL Data Warehouse, jej stan to "Resuming".
2. Jeśli stan to **online**, baza danych jest przygotowana.

    ![Stan online][6]

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Następne kroki
Aby uzyskać więcej informacji zobacz [Omówienie zarządzania][].

<!--Image references-->
[1]: ./media/sql-data-warehouse-manage-compute-portal/click-scale.png
[2]: ./media/sql-data-warehouse-manage-compute-portal/move-slider.png
[3]: ./media/sql-data-warehouse-manage-compute-portal/click-save.png
[4]: ./media/sql-data-warehouse-manage-compute-portal/resume-database.png
[5]: ./media/sql-data-warehouse-manage-compute-portal/resume-confirm.png
[6]: ./media/sql-data-warehouse-manage-compute-portal/pause-database.png
[7]: ./media/sql-data-warehouse-manage-compute-portal/pause-confirm.png

<!--Article references-->
[Omówienie zarządzania]: ./sql-data-warehouse-overview-manage.md
[Zarządzanie omówienie obliczeń]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->


<!--Other Web references-->

[Azure portal]: http://portal.azure.com/
