Równoważenie obciążenia jest równoważenia obciążenia warstwy 4 (TCP, UDP). Usługa równoważenia obciążenia zapewnia wysoką dostępność, poprzez dystrybuowanie przychodzącego ruchu wystąpień usług w dobrej kondycji w usługach w chmurze lub maszyn wirtualnych w zestawie usługi równoważenia obciążenia. Równoważenie obciążenia Azure można także przedstawić tych usług na wiele portów, wielu adresów IP lub oba.

Można skonfigurować usługę równoważenia obciążenia, aby:

* Załadować saldo przychodzącego ruchu internetowego do maszyn wirtualnych (VMs). Określamy równoważenia obciążenia, w tym scenariuszu jako [mechanizm równoważenia obciążenia z Internetu](../articles/load-balancer/load-balancer-internet-overview.md).
* Ruch równowagę obciążenia między maszynami wirtualnymi w sieci wirtualne (VNet), między maszynami wirtualnymi w usługach w chmurze lub między komputerami lokalnymi i maszyn wirtualnych w sieci wirtualnej rozproszonym. Określamy równoważenia obciążenia, w tym scenariuszu jako [wewnętrznego równoważenia obciążenia (ILB)](../articles/load-balancer/load-balancer-internal-overview.md).
* Przekazywania ruchu zewnętrznego do konkretnego wystąpienia maszyny Wirtualnej.
