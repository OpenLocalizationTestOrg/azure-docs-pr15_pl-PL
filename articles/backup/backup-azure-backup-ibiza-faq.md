<properties
   pageTitle="Usługi odzyskiwania magazynu — często zadawane pytania | Microsoft Azure"
   description="Ta wersja: często zadawane pytania obsługuje wersji Preview publicznej usługi Azure kopii zapasowej. Odpowiedzi na często zadawane pytania dotyczące agenta kopii zapasowej, wykonywanie kopii zapasowych i przechowywania, odzyskiwania, zabezpieczenia i inne często zadawane pytania dotyczące Azure tworzeniem kopii zapasowych."
   services="backup"
   documentationCenter=""
   authors="markgalioto"
   manager="jwhit"
   editor=""
   keywords="tworzenia kopii zapasowych; Usługa Kopia zapasowa"/>

<tags
   ms.service="backup"
   ms.workload="storage-backup-recovery"
     ms.tgt_pltfrm="na"
     ms.devlang="na"
     ms.topic="get-started-article"
     ms.date="10/21/2016"
     ms.author="trinadhk; markgal; jimpark;"/>

# <a name="recovery-services-vault---faq"></a>Odzyskiwanie usługi magazynu — często zadawane pytania


Ten artykuł zawiera informacje dotyczące magazynu usługi odzyskiwania i jej uzupełnia [Azure kopii zapasowych — często zadawane pytania](backup-azure-backup-faq.md). Często zadawane pytania Azure kopii zapasowej zawiera pełny zestaw pytania i odpowiedzi dotyczące usługi Azure kopii zapasowej.  

Można zadawać pytania o kopii zapasowej Azure w sekcji Disqus powiązanych artykule lub w tym artykule. Można również publikować pytania dotyczące usługi Azure kopii zapasowej na [forum dyskusyjne](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup).

## <a name="recovery-services-vaults-are-resource-manager-based-are-backup-vaults-classic-mode-still-supported-br"></a>Magazynami usług odzyskiwania są podstawie Menedżera zasobów. Wykonywanie kopii zapasowych magazynów (w trybie klasycznym) nadal są obsługiwane? <br/>
Tak, magazynami kopii zapasowej nadal są obsługiwane. Tworzenie kopii zapasowej magazynami w [klasycznym portalu](https://manage.windowsazure.com). Tworzenie magazynów usługi odzyskiwania w [Azure portal](https://portal.azure.com). Jednak Stanowczo zalecamy tworzenie magazynu usługi odzyskiwania jako wszystkie przyszłe ulepszenia jest dostępna tylko w magazynu usługi odzyskiwania.

## <a name="can-i-migrate-a-backup-vault-to-a-recovery-services-vault-br"></a>Można przeprowadzić migrację magazynu kopii zapasowej magazynu usługi odzyskiwania? <br/>
Niestety nie, w tej chwili nie można migrować zawartość magazynu kopii zapasowej do magazynu usługi odzyskiwania. Pracujemy obecnie nad Dodawanie tej funkcji, ale nie jest dostępna w ramach publicznej Podgląd.

## <a name="do-recovery-services-vaults-support-classic-vms-or-resource-manager-based-vms-br"></a>Menedżer zasobów systemem maszyny wirtualne lub magazynów usługi odzyskiwania obsługują klasyczny maszyny wirtualne? <br/>
Odzyskiwanie magazynami usług pomocy technicznej obu modeli.  Użytkownik może wykonywać kopie zapasowe maszyn wirtualnych utworzony w portalu klasyczny (które są maszyny wirtualne w trybie klasycznym) lub maszyny utworzony w portalu Azure (które są podstawie Menedżera zasobów) do magazynu usługi odzyskiwania.

## <a name="i-have-backed-up-my-classic-vms-in-backup-vault-now-i-want-to-migrate-my-vms-from-classic-mode-to-resource-manager-mode--how-can-i-backup-them-in-recovery-services-vault"></a>Masz zapasową Moje klasyczny maszyny wirtualne w kopii zapasowej magazynu. Teraz chcę Migrowanie Moje maszyn wirtualnych w trybie klasycznym do trybu Menedżera zasobów.  Jak można utworzyć kopię zapasową je w magazynu usługi odzyskiwania?
Kopie zapasowe klasyczny maszyny wirtualne w kopii zapasowej magazynu nie można przeprowadzić migrację automatycznie do magazynu usługi odzyskiwania podczas migracji maszyny wirtualne z klasyczny tryb Menedżera zasobów. Wykonaj te kroki dla migracji maszyn wirtualnych kopii zapasowych:

1. W kopii zapasowej magazynu przejdź do karty **Chronionych elementów** i wybierz maszyn wirtualnych. Polecenie [Zatrzymaj ochronę](backup-azure-manage-vms-classic.md#stop-protecting-virtual-machines). Nie zaznaczaj *usunąć skojarzone danych kopii zapasowej* opcja **niezaznaczone**.
2. Migrowanie maszyny wirtualnej w trybie klasycznym do trybu Menedżera zasobów. Upewnij się, że miejsca do magazynowania i odpowiadający mu maszyn wirtualnych sieci również są migrowane do trybu Menedżera zasobów.
3. Tworzenie magazynu usługi odzyskiwania i konfigurowanie kopii zapasowych na komputerze wirtualnych migracją przy użyciu akcji **kopii zapasowej** na wierzchu pulpitu nawigacyjnego magazynu. Dowiedz się więcej na temat [włączania kopia zapasowa odzyskiwania usług magazynu](backup-azure-vms-first-look-arm.md)
