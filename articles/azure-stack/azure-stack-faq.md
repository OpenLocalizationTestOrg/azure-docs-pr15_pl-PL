<properties
    pageTitle="Często zadawane pytania dotyczące stos Azure | Microsoft Azure"
    description="Stos Azure często zadawane pytania."
    services="azure-stack"
    documentationCenter=""
    authors="HeathL17"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/13/2016"
    ms.author="helaw"/>

# <a name="frequently-asked-questions-for-azure-stack"></a>Często zadawane pytania dotyczące stos Azure

## <a name="deployment"></a>Wdrożenie

### <a name="do-i-need-to-format-my-data-disks-before-starting-or-restarting-an-installation"></a>Czy muszę formatowanie dysków danych przed uruchomieniem lub ponowne uruchamianie instalacji?

Dyski powinny znajdować się w formacie nieprzetworzonych. Jeśli zostanie ponownie zainstalowany system operacyjny, może być konieczne Sprawdź, czy nadal istnieje stary puli miejsca do magazynowania i usunąć, wykonując następujące czynności:

1. Otwórz Menedżera serwera.
2. Wybierz pozycję puli miejsca do magazynowania.
3. Zobacz, czy jest wymieniony w puli miejsca do magazynowania.
4. Jeśli na liście i Włącz odczytu / zapisu, kliknij prawym przyciskiem myszy **puli miejsca do magazynowania** .
5. Kliknij prawym przyciskiem myszy **wirtualnego dysku twardego** (lewym dolnym rogu) i wybierz pozycję Usuń.
6. Kliknij prawym przyciskiem myszy **Puli miejsca do magazynowania** , a następnie kliknij pozycję Usuń.
7. Ponownie uruchom skrypt stos Azure i sprawdź, czy przekazuje weryfikacji dysku.

Opcjonalnie można następujący skrypt:

```PowerShell
$pools = Get-StoragePool -IsPrimordial $False -ErrorVariable Err -ErrorAction SilentlyContinue
if ($pools -ne $null) {
  $pools| Set-StoragePool -IsReadOnly $False -ErrorVariable Err -ErrorAction SilentlyContinue
  $pools = Get-StoragePool -IsPrimordial $False -ErrorVariable Err -ErrorAction SilentlyContinue
  $pools | Get-VirtualDisk | Remove-VirtualDisk -Confirm:$False
  $pools | Remove-StoragePool -Confirm:$False
}
```

### <a name="can-i-use-all-ssd-disks-for-the-storage-pool-in-the-poc-installation"></a>Czy można używać wszystkie dyski SSD dla puli miejsca do magazynowania w instalacji, aby Zapewnić?

Ta konfiguracja nie jest obsługiwane w tej wersji.  Aby uzyskać więcej informacji zobacz [Przewodnik wymagania](azure-stack-deploy.md) uzyskać więcej informacji.

### <a name="can-i-use-nvme-data-disks-for-the-microsoft-azure-stack-poc"></a>Czy można używać do zatwierdzenia Microsoft Azure stos Koncepcji dyski danych NVMe?

Podczas magazynowania spacje bezpośredni obsługuje dyski NVMe, stos Azure obsługuje tylko niektóre możliwe typy i układy dla bezpośredniego spacje miejsca do magazynowania. 

### <a name="how-can-i-reinstall-azure-stack"></a>Jak można ponownie zainstalować stos Azure?
Można wykonać czynności opisane w [przewodniku ponownego rozmieszczania](azure-stack-redeploy.md).  

## <a name="tenant"></a>Dzierżawy

### <a name="can-i-deploy-my-own-images-as-a-tenant"></a>Czy można wdrażać własnej obrazy, jako dzierżawy?

Tak, tak jak w Azure, dzierżawy przekazywać obrazy w stos Azure oprócz przy użyciu obrazów z administratorem usługi. Aby uzyskać omówienie zobacz [Dodawanie obrazu maszyn wirtualnych](azure-stack-add-vm-image.md). 

## <a name="testing"></a>Testowanie

### <a name="can-i-use-nested-virtualization-to-test-the-microsoft-azure-stack-poc"></a>Aby przetestować Zapewnić stos Microsoft Azure można używać zagnieżdżonych wirtualizacji?

Zagnieżdżone wirtualizacji nie jest obsługiwany ani z Azure stos Technical Preview 2.

## <a name="virtual-machines"></a>Maszyn wirtualnych

### <a name="does-azure-stack-support-dynamic-disks-for-virtual-machines"></a>Stos Azure obsługuje dyski dynamiczne dla maszyn wirtualnych?

Stos Azure nie obsługuje dysków dynamicznych.

## <a name="next-steps"></a>Następne kroki

[Rozwiązywanie problemów](azure-stack-troubleshooting.md)
