<properties
    pageTitle="Konfigurowanie ustawień integracji Azure klucza magazynu dla programu SQL Server na Azure maszyny wirtualne (Menedżer zasobów)"
    description="Dowiedz się, jak do automatyzowania konfiguracji programu SQL Server szyfrowania do użytku z magazynu klucza Azure. W tym temacie wyjaśniono, jak używać Azure klucza magazynu integracji programu SQL Server maszyn wirtualnych utworzonego za pomocą Menedżera zasobów."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/25/2016"
    ms.author="jroth"/>

# <a name="configure-azure-key-vault-integration-for-sql-server-on-azure-vms-resource-manager"></a>Konfigurowanie ustawień integracji Azure klucza magazynu dla programu SQL Server na Azure maszyny wirtualne (Menedżer zasobów)

> [AZURE.SELECTOR]
- [Menedżer zasobów](virtual-machines-windows-ps-sql-keyvault.md)
- [Klasyczny](virtual-machines-windows-classic-ps-sql-keyvault.md)

## <a name="overview"></a>Omówienie
Istnieje wiele funkcji szyfrowania programu SQL Server, takich jak [szyfrowanie danych przezroczysty (TDE)](https://msdn.microsoft.com/library/bb934049.aspx), [szyfrowanie na poziomie kolumny (CLE)](https://msdn.microsoft.com/library/ms173744.aspx)i [szyfrowania kopii zapasowej](https://msdn.microsoft.com/library/dn449489.aspx). Te formy szyfrowania trzeba przechowywać klucze szyfrowania, których używasz na potrzeby szyfrowania i zarządzanie nimi. Usługa Azure klucza magazynu (AKV) ma na celu poprawę zabezpieczeń i zarządzania klucze w lokalizacji bezpieczne i wysokiej dostępności. [Program SQL Server Connector](http://www.microsoft.com/download/details.aspx?id=45344) umożliwia SQL Server, aby te klawisze z magazynu klucza Azure.

Jeśli zostanie uruchomiony program SQL Server z lokalnego maszyny tam czynności [można wykonać, aby uzyskać dostęp do magazynu klucza Azure z komputera lokalnego programu SQL Server](https://msdn.microsoft.com/library/dn198405.aspx). Ale dla programu SQL Server w maszyny wirtualne Azure, można zaoszczędzić czas przy użyciu funkcji *Integracji magazynu klucza Azure* .

Jeśli ta funkcja jest włączona, jej automatycznie instaluje program SQL Server Connector, konfiguruje dostawcy EKM, aby uzyskać dostęp do magazynu klucza Azure i tworzy poświadczeń, aby umożliwić użytkownikowi dostęp do magazynu. Jeśli elementu czynności opisane w dokumentacji lokalnych wcześniej wspomniano, widać, że ta funkcja pozwala zautomatyzować kroki 2 i 3. Tylko element, który jeszcze trzeba zrobić ręcznie jest utworzenie klucza magazynu oraz klawiszy. W tym miejscu jest zautomatyzowany wszystkie ustawienia maszyn wirtualnych usługi SQL. Po zakończeniu tej konfiguracji tej funkcji można wykonać instrukcji T SQL, aby rozpocząć szyfrowanie bazy danych lub z kopii zapasowych w normalny sposób.

[AZURE.INCLUDE [AKV Integration Prepare](../../includes/virtual-machines-sql-server-akv-prepare.md)]

## <a name="enabling-and-configuring-akv-integration"></a>Włączanie i Konfigurowanie integracji AKV
Możesz włączyć integrację z programem AKV podczas inicjowania obsługi administracyjnej lub jest skonfigurowana dla istniejących maszyny wirtualne.

### <a name="new-vms"></a>Nowe maszyny wirtualne
Jeśli są inicjowania obsługi administracyjnej nowa maszyna wirtualna programu SQL Server przy użyciu Menedżera zasobów, Azure portal zawiera czynności, aby włączyć integrację Azure klucza magazynu. Funkcja Azure klucza magazynu jest dostępna tylko w przypadku Enterprise, Developer i oceny wersje programu SQL Server.

![Integracja z SQL Azure klucza magazynu](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-arm-akv.png)

Aby uzyskać szczegółowy opis inicjowania obsługi administracyjnej zobacz [Obsługa administracyjna maszyny wirtualnej programu SQL Server w Azure Portal](virtual-machines-windows-portal-sql-server-provision.md).

### <a name="existing-vms"></a>Istniejące maszyny wirtualne
Dla istniejących maszyn wirtualnych programu SQL Server wybierz ten komputer wirtualnych programu SQL Server. Zaznacz sekcję karta **Ustawienia** **konfiguracji programu SQL Server** .

![Integracja AKV SQL dla istniejących maszyny wirtualne](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-rm-akv-existing-vms.png)

W karta **konfiguracji programu SQL Server** kliknij przycisk **Edytuj** w sekcji integracja automatycznego magazynu klucza.

![Konfigurowanie ustawień integracji AKV SQL dla istniejących maszyny wirtualne](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-rm-akv-configuration.png)

Po zakończeniu kliknij przycisk **OK** w dolnej części karta **konfiguracji programu SQL Server** , aby zapisać zmiany.

>[AZURE.NOTE] Można również skonfigurować integracji AKV przy użyciu szablonu. Aby uzyskać więcej informacji zobacz temat [szablonu Azure Szybki Start do integracji Azure klucza magazynu](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-keyvault-update).

[AZURE.INCLUDE [AKV Integration Next Steps](../../includes/virtual-machines-sql-server-akv-next-steps.md)]
