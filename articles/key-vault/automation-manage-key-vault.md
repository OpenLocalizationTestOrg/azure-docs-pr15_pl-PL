<properties
    pageTitle="Zarządzanie Azure klucza magazynu przy użyciu automatyzacji Azure | Microsoft Azure"
    description="Dowiedz się, jak usługa Azure automatyzacji można zarządzać Azure klucza magazynu."
    services="Key-Vault, automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/29/2016"
    ms.author="magoedte;csand"/>

#<a name="managing-azure-key-vault-using-azure-automation"></a>Zarządzanie Azure klucza magazynu przy użyciu automatyzacji Azure

Ten przewodnik podstawowe informacje na temat usług Azure Automation Services i jak go można uprościć zarządzanie klucze i hasła Azure klucza magazynu.

## <a name="what-is-azure-automation"></a>Co to jest Azure automatyzacji?

[Automatyzacja Azure](../automation/automation-intro.md) jest usługą Azure dotyczące uproszczenia zarządzania chmurze przez proces automatyzacji i konfiguracji pożądany stan. Przy użyciu automatyzacji Azure ręcznego, powtarzać, długim i podatne na błędy zadania można zautomatyzować można zwiększyć niezawodność, wydajność i wartość czasu dla Twojej organizacji.

Automatyzacja Azure udostępnia aparat wykonania wysoce niezawodne, wysokiej dostępności przepływu pracy, który skale stosownie do potrzeb. W automatyzacji Azure procesów może być kopać ręcznie, systemów 3rd firm lub zaplanowanymi interwałami tak, aby zadania wystąpić dokładnie w razie potrzeby.

Zmniejszyć operacyjne i zwolnić IT i personelu DevOps skoncentrować się na pracy, które dodaje wartości biznesowej, przenosząc zadań zarządzania chmury mógł być automatycznie uruchamiany przez automatyzacji Azure.


## <a name="how-can-azure-automation-help-manage-azure-key-vault"></a>Jak automatyzacji Azure ułatwia zarządzanie magazynu klucza Azure?

Klucz magazynu można zarządzać w automatyzacji Azure za pomocą [AzureRM klucza magazynu cmdlet] (https://www.powershellgallery.com/packages/AzureRM.KeyVault/1.1.4) i [polecenia cmdlet Azure klasyczny klucza magazynu](https://msdn.microsoft.com/library/azure/dn868052.aspx). Moduł Azure zarządzania klasyczny magazynu klucza jest dostępne automatycznie w automatyzacji Azure i [moduł AzureRM KeyVault](https://www.powershellgallery.com/packages/AzureRM.KeyVault/1.1.4) można zaimportować do automatyzacji Azure tak, aby można było wykonywać wiele zadań zarządzania klucza magazynu w usłudze. Można także skojarzyć tych poleceń cmdlet automatyzacji Azure, przy użyciu polecenia cmdlet dla innych usług Azure, do automatyzowania zadań złożonych 3 systemów firmy i usług Azure.

Przy użyciu poleceń cmdlet Azure klucza magazynu można wykonywać następujących zadań między innymi: 

- Tworzenie i Konfigurowanie klucza magazynu
- Tworzenie lub Importowanie klucza
- Tworzenie lub aktualizowanie hasła
- Aktualizowanie atrybuty klucza
- Uzyskiwanie klucza lub hasło
- Usuwanie hasła lub klucza

Oto kilka przykładów przy użyciu programu PowerShell do zarządzania klucza magazynu:  

* [Azure klucza magazynu — krok po kroku](https://blogs.technet.microsoft.com/kv/2015/06/02/azure-key-vault-step-by-step)
* [Instalowanie i konfigurowanie Azure magazynu klucza](https://www.simple-talk.com/cloud/platform-as-a-service/setting-up-and-configuring-an-azure-key-vault)


## <a name="next-steps"></a>Następne kroki

Teraz, gdy znasz już podstawy automatyzacji Azure i jak może służyć do zarządzania Azure klucza magazynu, wykonaj te łącza, aby dowiedzieć się więcej o automatyzacji Azure.

* Zobacz Azure automatyzacji [wprowadzenie samouczka](../automation/automation-first-runbook-graphical.md).
* Zobacz [skryptów programu PowerShell magazynu klucza Azure](https://gallery.technet.microsoft.com/scriptcenter/site/search?query=azure%20key%20vault&f%5B0%5D.Value=azure%20key%20vault&f%5B0%5D.Type=SearchText&ac=5).
