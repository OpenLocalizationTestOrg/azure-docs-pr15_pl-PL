##<a name="tcp-settings-for-azure-vms"></a>Ustawienia protokołu TCP dla maszyny wirtualne Azure

Azure maszyny wirtualne komunikowanie się za pomocą publicznego Internetu przy użyciu [protokołu NAT] [ nat] (translatora adresów sieciowych). Urządzenia NAT przypisać publiczny adres IP i port Głosowa Azure, umożliwiając, który maszyn wirtualnych ustanowienie socket komunikacji z innych urządzeń. Jeśli pakiety zatrzymać ułożony za pośrednictwem tego socket po upływie określonego czasu, urządzenie NAT kasuje mapowanie i socket jest bezpłatna do użytku za pośrednictwem innych SMS.

Jest to typowe zachowanie translatora adresów Sieciowych, co może spowodować problemy komunikacji na podstawie TCP aplikacje, które się spodziewać socket utrzymania poza limit czasu. Istnieją dwa ustawienia czasu bezczynności brać pod uwagę sesji w stanie *ustanowić połączenie* :

- **ruch przychodzący** przez [Usługa równoważenia obciążenia Azure][azure-lb-timeout]. Ten limit czasu domyślne 4 minut, a następnie można dostosować w górę do 30 minut.
- **ruchu wychodzącego** przy użyciu [SNAT] [ snat] (źródło NAT). Ten limit czasu jest ustawiony na 4 minut, a nie można dostosować.

Aby upewnić się, że połączenia nie zostaną utracone poza limit czasu, należy upewnij się, aplikacji zachowuje sesję aktywności, lub możesz skonfigurować system operacyjny, aby to zrobić. Ustawienia może być używany różnią się dla systemów Windows i Linux, tak jak pokazano poniżej.

[Linux][linux], należy zmienić zmienne jądra poniżej.
NET.IPv4.tcp_keepalive_time = 120 net.ipv4.tcp_keepalive_intvl = 30 net.ipv4.tcp_keepalive_probes = 8
 
Dla [systemu Windows][windows], należy zmienić poniżej wartości rejestru.
KeepAliveInterval = 30 KeepAliveTime = 120 TcpMaxDataRetransmissions = 8


Powyższe ustawienia upewnij się, Zachowaj pakiet aktywności jest wysłane po 2 minuty (120 sekund) czasu bezczynności, a następnie wysłane co 30 sekund. A jeśli się nie powieść, 8 pakiety, sesja zostanie usunięte.

<!-- links -->
[nat]: http://computer.howstuffworks.com/nat.htm
[snat]: ../load-balancer/load-balancer-overview.md/#source-nat
[linux]: http://tldp.org/HOWTO/TCP-Keepalive-HOWTO/usingkeepalive.html
[windows]: http://blogs.technet.com/b/nettracer/archive/2010/06/03/things-that-you-may-want-to-know-about-tcp-keepalives.aspx
[azure-lb-timeout]: ../load-balancer/load-balancer-tcp-idle-timeout.md