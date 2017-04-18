## <a name="scenario"></a>Scenariusz

Ten dokument będzie szczegółową wdrożenia wykorzystującego kart w maszyny wirtualne w określonej sytuacji. W tym scenariuszu masz dwuwarstwowej IaaS obciążenie pracą obsługiwany w Azure. Każdą jest wdrożona w osobnej podsieci wirtualnej sieci (VNet). Warstwa fronton składa się z kilku serwery sieci web, zgrupowane w równoważenia obciążenia, ustaw wysokiej dostępności. Poziom wewnętrznej składa się z kilku serwerów bazy danych. Te serwery bazy danych zostanie wdrożony z dwóch nic, jedną dla bazy danych programu access, drugi — zarządzanie. Scenariusz obejmuje również grupy zabezpieczeń sieci (NSGs), aby kontrolować, jakie ruch jest dozwolony z każdą i NIC w ramach wdrożenia. Na poniższym rysunku przedstawiono podstawowe architektury w tym scenariuszu.  

![Scenariusz MultiNIC](./media/virtual-network-deploy-multinic-scenario-include/Figure1.png)

