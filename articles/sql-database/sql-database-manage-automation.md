<properties
    pageTitle="Zarządzanie bazami danych SQL Azure za pomocą automatyzacji Azure | Microsoft Azure"
    description="Dowiedz się, jak usługa Azure automatyzacji umożliwia zarządzanie bazami danych Azure SQL w skali."
    services="sql-database, automation"
    documentationCenter=""
    authors="jodoglevy"
    manager="jhubbard"
    editor="monicar"/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/26/2016"
    ms.author="jolevy"/>



#<a name="managing-azure-sql-databases-using-azure-automation"></a>Zarządzanie bazami danych programu SQL Azure za pomocą automatyzacji Azure

Ten przewodnik podstawowe informacje na temat usług Azure Automation Services i jak go można uprościć zarządzanie baz danych Azure SQL.


## <a name="what-is-azure-automation"></a>Co to jest Azure automatyzacji?

[Automatyzacja Azure](https://azure.microsoft.com/services/automation/) jest usługą Azure dotyczące uproszczenia zarządzania chmurze przez proces automatyzacji. Za pomocą automatyzacji Azure długotrwałe, ręcznego podatne na błędy i często powtarzających się zadań można zautomatyzować można zwiększyć niezawodność, wydajność i wartość czasu dla Twojej organizacji.

Automatyzacja Azure udostępnia aparat wykonania wysoce niezawodne i wysokiej dostępności przepływ pracy, który skale stosownie do potrzeb w miarę Twojej organizacji. W automatyzacji Azure procesów może być kopać ręcznie, systemów 3rd firm lub zaplanowanymi interwałami tak, aby zadania wystąpić dokładnie w razie potrzeby.

Zmniejsz obciążenie operacyjne i zwolnić IT / personelu DevOps skoncentrować się na pracy, który zapewnia firm wartość, przenosząc zadań zarządzania chmury mógł być automatycznie uruchamiany przez automatyzacji Azure.


## <a name="how-can-azure-automation-help-manage-azure-sql-databases"></a>Jak automatyzacji Azure ułatwia zarządzanie bazami danych Azure SQL?

Baza danych SQL Azure można zarządzać w automatyzacji Azure za pomocą [poleceń cmdlet środowiska PowerShell bazy danych SQL Azure](https://msdn.microsoft.com/library/dn546723.aspx) , że są dostępne na karcie [Narzędzia Azure programu PowerShell](https://msdn.microsoft.com/library/azure/jj156055.aspx). Azure automatyzacji ma tych poleceń cmdlet programu PowerShell bazy danych SQL Azure okno, dzięki czemu można wykonywać wszystkie zadania zarządzania bazy danych SQL w usłudze. Można także skojarzyć tych poleceń cmdlet automatyzacji Azure, przy użyciu polecenia cmdlet dla innych usług Azure, do automatyzowania zadań złożonych 3 systemów firmy i usług Azure.

Azure automatyzacji ma także możliwość komunikowania się z serwerami SQL bezpośrednio, wysyłając polecenia SQL przy użyciu programu PowerShell.

[Galeria działań aranżacji automatyzacji Azure](https://azure.microsoft.com/blog/2014/10/07/introducing-the-azure-automation-runbook-gallery/) zawiera szereg produktu zespołów i społeczności runbooks rozpocząć Automatyzowanie zarządzania baz danych programu SQL Azure, innych usług Azure i 3 systemów firmy. Galeria runbooks obejmują:

 * [Uruchamianie zapytania SQL w bazie danych programu SQL Server](https://gallery.technet.microsoft.com/scriptcenter/How-to-use-a-SQL-Command-be77f9d2)
 * [Skalowanie w pionie (w górę lub w dół) bazy danych SQL Azure zgodnie z harmonogramem](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-Database-e957354f)
 * [Obciąć tabelę SQL, jeżeli maksymalnego rozmiaru bazy danych](https://gallery.technet.microsoft.com/scriptcenter/Azure-Automation-Your-SQL-30f8736b)
 * [Indeksowanie tabel w bazie danych SQL Azure, gdy są one bardzo pofragmentowany](https://gallery.technet.microsoft.com/scriptcenter/Indexes-tables-in-an-Azure-73a2a8ea)

## <a name="next-steps"></a>Następne kroki

Teraz, gdy znasz już podstawy automatyzacji Azure i jak może być używana do zarządzania baz danych programu SQL Azure, wykonaj te łącza, aby dowiedzieć się więcej o automatyzacji Azure.

- [Omówienie Azure automatyzacji](../automation/automation-intro.md)
- [Mój pierwszy działań aranżacji](../automation/automation-first-runbook-graphical.md)
- [Mapa Azure nauki automatyzacji](https://azure.microsoft.com/documentation/learning-paths/automation/)
- [Azure automatyzacji: Usługi SQL agenta w chmurze](https://azure.microsoft.com/blog/2014/06/26/azure-automation-your-sql-agent-in-the-cloud/) 
 
