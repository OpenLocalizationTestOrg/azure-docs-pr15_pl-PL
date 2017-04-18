<properties
   pageTitle="Uzyskiwanie dostępu do Identyfikatora maszyn wirtualnych"
   description="W tym artykule opisano otwieranie i korzystanie z Azure maszyn wirtualnych Unikatowy identyfikator"
   services="virtual-machines-linux"
   documentationCenter="virtual-machines"
   authors="kmouss"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="02/08/2016"
   ms.author="kmouss"/>
   
# <a name="accessing-and-using-azure-vm-unique-id"></a>Otwieranie i korzystanie z Azure maszyn wirtualnych Unikatowy identyfikator

Azure maszyn wirtualnych Unikatowy identyfikator jest identyfikator 128-bitowe kodowane i przechowywane w SMBIOS wszystkich Azure IaaS maszyn wirtualnych i obecnie można odczytywać przy użyciu polecenia BIOS platformy.

Unikatowy identyfikator Azure maszyn wirtualnych jest właściwością tylko do odczytu. Azure Unikatowy identyfikator maszyn wirtualnych nie ulegnie zmianie po ponownym uruchomieniu zamknięcia (albo planowane niezaplanowane), Uruchom/Zatrzymaj usunąć przydział usługi korygujący lub przywrócić w miejscu. Jednak jeśli maszyn wirtualnych jest migawki i kopiowany w celu utworzenia nowego wystąpienia, nowy identyfikator maszyn wirtualnych Azure jest skonfigurowany.

> [AZURE.NOTE] Jeśli masz starszy maszyny wirtualne utworzone i od tej nowej funkcji masz wdrażania (18 września 2014), zobacz ponowne uruchomienie usługi maszyn wirtualnych, aby automatycznie pobierać Azure Unikatowy identyfikator.


Aby uzyskać dostęp do Azure unikatowy maszyn wirtualnych identyfikator z poziomu maszyn wirtualnych:


## <a name="create-a-vm"></a>Tworzenie maszyn wirtualnych
 

Aby uzyskać więcej informacji zobacz [Tworzenie maszyny wirtualnej](virtual-machines-linux-creation-choices.md)


## <a name="connect-to-the-vm"></a>Nawiązywanie połączenia z maszyn wirtualnych
 

Aby uzyskać więcej informacji zobacz [SSH z Linux](virtual-machines-linux-mac-create-ssh-keys.md)


## <a name="query-vm-unique-id"></a>Unikatowy identyfikator maszyn wirtualnych kwerendy

Polecenie (w przykładzie wykorzystano **Ubuntu**):

    sudo dmidecode | grep UUID
    
Przykład oczekiwane wyniki:

    UUID: 090556DA-D4FA-764F-A9F1-63614EDA019A
    
Ze względu na Big Endian bit kolejnością rzeczywisty Unikatowy identyfikator maszyn wirtualnych w tym przypadku będzie:

    DA 56 05 09 – FA D4 – 4f 76 - A9F1-63614EDA019A
    
    
Unikatowy identyfikator Azure maszyn wirtualnych może służyć w różnych scenariuszach, czy maszyn wirtualnych pracuje Azure lub lokalnego i może ułatwić wymagań licencjonowania, raportowania i ogólne śledzenie, które masz na wdrożeń Azure IaaS. Wiele niezależnych dostawców oprogramowania tworzenia aplikacji i potwierdzającego ich Azure może być wymagana do identyfikowania maszyn wirtualnych Azure całej jej cyklu życia i określić, czy maszyn wirtualnych jest uruchomiony Azure, lokalnego lub w innych dostawców usług w chmurze. Ten identyfikator platformy na przykład może pomóc w wykrywa, jeśli oprogramowanie jest licencjonowane lub pomocy do grupowania danych maszyn wirtualnych do źródła, takich jak pomoc na temat ustawiania prawo metryki dla odpowiedniej platformy oraz do śledzenia i przeniesionym tych metryki wśród innych celów.
