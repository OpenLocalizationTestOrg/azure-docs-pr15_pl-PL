<properties
    pageTitle="Zagadnienia dotyczące zabezpieczeń dla programu SQL Server w Azure | Microsoft Azure"
    description="W tym temacie dotyczą zasobów utworzone za pomocą modelu Klasyczny wdrożenia i zawiera ogólne wskazówki dotyczące zabezpieczania SQL Server uruchomionym w maszyn wirtualnych Azure."
    services="virtual-machines-windows"
    documentationCenter="na"
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
    ms.date="06/24/2016"
    ms.author="jroth" />

# <a name="security-considerations-for-sql-server-in-azure-virtual-machines"></a>Zagadnienia dotyczące zabezpieczeń dla programu SQL Server w środowisku maszyn wirtualnych Azure
 
Ten temat zawiera ogólne zasad zabezpieczeń, które pomagają ustalić bezpiecznego dostępu do wystąpienia programu SQL Server w maszyn wirtualnych Azure. Aby zapewnić lepszą ochronę wystąpieniach bazy danych programu SQL Server platformy Azure, zalecamy zaimplementować wskazówki dotyczące zabezpieczeń lokalnych tradycyjnych oprócz najważniejsze wskazówki dotyczące zabezpieczeń dla Azure.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Aby uzyskać więcej informacji na temat wskazówki dotyczące zabezpieczeń programu SQL Server zobacz [program SQL Server 2008 R2 najważniejsze wskazówki dotyczące zabezpieczeń — operacyjne i zadania administracyjne](http://download.microsoft.com/download/1/2/A/12ABE102-4427-4335-B989-5DA579A4D29D/SQL_Server_2008_R2_Security_Best_Practice_Whitepaper.docx)

Azure jest zgodny z kilku przepisów branżowe oraz standardów, które umożliwiają tworzenie rozwiązanie zgodności z programem SQL Server działa w maszyny wirtualnej. Aby uzyskać informacje dotyczące zgodności z przepisami z Azure zobacz [Centrum zaufania Azure](https://azure.microsoft.com/support/trust-center/).

Oto lista zalecenia dotyczące zabezpieczeń, które należy uwzględnić podczas konfigurowania i łączenie się z wystąpieniem programu SQL Server w maszyn wirtualnych Azure.

## <a name="considerations-for-managing-accounts"></a>Zagadnienia dotyczące zarządzania kontami:

- Tworzenie unikatowych lokalnego konta administratora nie o nazwie **Administrator**.

- Używanie złożonych silnych haseł dla wszystkich kont. Aby uzyskać więcej informacji na temat tworzenia silnego hasła, zobacz artykuł [porady dotyczące tworzenia silnych haseł](http://windows.microsoft.com/en-us/windows-vista/Tips-for-creating-a-strong-password) .

- Domyślnie Azure zaznacza uwierzytelniania systemu Windows podczas instalacji programu SQL Server Virtual Machine. Dlatego logowania **SA** jest wyłączona, a hasło jest przypisane przez Instalatora. Firma Microsoft zalecamy, aby logowania **SA** powinny być nie należy używać ani włączone. Poniżej przedstawiono alternatywnych strategii, jeśli jest konieczne logowanie SQL:
    - Utwórz konto SQL, który jest członkiem grupy.
    - Jeśli musisz użyć logowania **SA** , włączyć logowania i zmień jej nazwę i przypisz nowe hasło.
    - Oba wybrane opcje wspomniano wcześniej wymagają zmiany tryb uwierzytelniania w **trybie uwierzytelniania Windows**do programu SQL Server. Aby uzyskać więcej informacji zobacz [Zmienianie tryb uwierzytelniania serwera](https://msdn.microsoft.com/library/ms188670.aspx).

## <a name="considerations-for-securing-connections-to-azure-virtual-machine"></a>Zagadnienia dotyczące zabezpieczania połączeń Azure maszyn wirtualnych:

- Warto rozważyć użycie [Azure wirtualną sieć](../virtual-network/virtual-networks-overview.md) administrowania maszyn wirtualnych zamiast publicznej porty RDP.

- Aby zezwolić lub odmówić ruch sieciowy usługi maszyn wirtualnych za pomocą [Grupy zabezpieczeń sieci](../virtual-network/virtual-networks-nsg.md) (NSG). Jeśli chcesz używać NSG i zawiera już punktu końcowego ACL w miejscu, najpierw usuń punkt końcowy ACL. Aby dowiedzieć się, jak to zrobić zobacz [Zarządzanie listy kontroli dostępu (ACL) dla punktów końcowych przy użyciu programu PowerShell](../virtual-network/virtual-networks-acl-powershell.md).

- Jeśli korzystasz z punktów końcowych, należy usunąć wszelkie punkty końcowe na komputerze wirtualnych, jeśli nie należy używać. Aby uzyskać instrukcje dotyczące używania ACL z punktów końcowych zobacz [Zarządzanie ACL dla punktu końcowego](../virtual-network/virtual-machines-windows-classic-setup-endpoints.md#manage-the-acl-on-an-endpoint).

- Włącz opcję zaszyfrowanego połączenia w przypadku wystąpienia aparat bazy danych programu SQL Server w maszyn wirtualnych Azure. Skonfiguruj instalacja programu SQL server za pomocą certyfikatu z podpisem. Aby uzyskać więcej informacji zobacz [Włączanie połączenia szyfrowane aparatu bazy danych](https://msdn.microsoft.com/library/ms191192.aspx) i [Składni parametry połączenia](https://msdn.microsoft.com/library/ms254500.aspx).

- Jeśli maszyn wirtualnych powinni mieć dostęp tylko z określonej sieci, aby ograniczyć dostęp do określonych adresów IP lub podsieci za pomocą zapory systemu Windows.

## <a name="next-steps"></a>Następne kroki

Jeśli interesuje Cię także najważniejsze wskazówki dotyczące ogólnej wydajności, zobacz [Wydajność najważniejsze wskazówki dotyczące programu SQL Server w maszyn wirtualnych Azure](virtual-machines-windows-sql-performance.md).

Aby uzyskać innych tematów związanych z programem SQL Server w maszyny wirtualne Azure zobacz [Programu SQL Server w środowisku maszyn wirtualnych systemu Azure Przegląd](virtual-machines-windows-sql-server-iaas-overview.md).
