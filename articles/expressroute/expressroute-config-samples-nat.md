<properties
   pageTitle="Przykłady konfiguracji router klienta ExpressRoute | Microsoft Azure"
   description="Ta strona zawiera przykłady konfiguracji routera dla routera Cisco i Juniper."
   documentationCenter="na"
   services="expressroute"
   authors="cherylmc"
   manager="carmonm"
   editor="" />
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="cherylmc"/>

# <a name="router-configuration-samples-to-setup-and-manage-nat"></a>Przykłady konfiguracji routera konfiguracji i zarządzać nimi translatora adresów Sieciowych

Ta strona zawiera przykłady konfiguracji translatora adresów Sieciowych do routera serii Cisco ASA i Juniper SRX. Te mają być próbki tylko do celów informacyjnych i nie mogą być używane, tak jak. Możesz pracować z dostawcą opracowywane konfiguracji odpowiednie dla Twojej sieci. 

>[AZURE.IMPORTANT] Przykłady na tej stronie mają być czysto orientacji. Należy współpracy z z dostawcą sprzedaż / technicznych zespołu i sieci zespół opracowywane odpowiednie układy, stosownie do potrzeb użytkownika. Microsoft nie obsługuje problemy związane z konfiguracji opisanych na tej stronie. Problemów należy skontaktować się z dostawcą urządzenia.

Poniżej pokazano konfiguracji routera dotyczą peerings Azure publicznych i firmy Microsoft. Azure zaglądanie prywatne nie musisz skonfigurować translatora adresów Sieciowych. Przejrzyj [ExpressRoute peerings](expressroute-circuit-peerings.md) i [wymagania ExpressRoute translatora adresów Sieciowych](expressroute-nat.md) , aby uzyskać więcej informacji.

**Uwaga:** Łączność z Internetem i ExpressRoute, należy użyć osobnych pule translatora adresów IP. Przy użyciu tej samej puli translatora adresów IP przez internet oraz ExpressRoute spowoduje asymetrycznym routing i utrata łączności.

## <a name="cisco-asa-firewalls"></a>Cisco ASA zapory

### <a name="pat-configuration-for-traffic-from-customer-network-to-microsoft"></a>Konfiguracja Marta dla ruchu z sieci klienta do firmy Microsoft

    object network MSFT-PAT
      range <SNAT-START-IP> <SNAT-END-IP>
    
    
    object-group network MSFT-Range
      network-object <IP> <Subnet_Mask>
    
    object-group network on-prem-range-1
      network-object <IP> <Subnet-Mask>
    
    object-group network on-prem-range-2
      network-object <IP> <Subnet-Mask>
    
    object-group network on-prem
      network-object object on-prem-range-1
      network-object object on-prem-range-2
    
    nat (outside,inside) source dynamic on-prem pat-pool MSFT-PAT destination static MSFT-Range MSFT-Range

### <a name="pat-configuration-for-traffic-from-microsoft-to-customer-network"></a>Konfiguracja Marta ruchu przez firmę Microsoft do sieci klientów

#### <a name="interfaces-and-direction"></a>Interfejsy i kierunku:
    Source Interface (where the traffic enters the ASA): inside
    Destination Interface (where the traffic exits the ASA): outside

#### <a name="configuration"></a>Konfiguracja:
Pula translatora adresów Sieciowych:

    object network outbound-PAT
        host <NAT-IP>

Serwer docelowy:

    object network Customer-Network
        network-object <IP> <Subnet-Mask>

Grupy obiektów dla adresów IP klienta

    object-group network MSFT-Network-1
        network-object <MSFT-IP> <Subnet-Mask>
    
    object-group network MSFT-PAT-Networks
        network-object object MSFT-Network-1

Polecenia translatora adresów Sieciowych:

    nat (inside,outside) source dynamic MSFT-PAT-Networks pat-pool outbound-PAT destination static Customer-Network Customer-Network


## <a name="juniper-srx-series-routers"></a>Routery serii SRX juniper 

### <a name="1-create-redundant-ethernet-interfaces-for-the-cluster"></a>1. Tworzenie zbędne interfejsy Ethernet klaster

    interfaces {
        reth0 {
            description "To Internal Network";
            vlan-tagging;
            redundant-ether-options {
                redundancy-group 1;
            }
            unit 100 {
                vlan-id 100;
                family inet {
                    address <IP-Address/Subnet-mask>;
                }
            }
        }
        reth1 {
            description "To Microsoft via Edge Router";
            vlan-tagging;
            redundant-ether-options {
                redundancy-group 2;
            }
            unit 100 {
                description "To Microsoft via Edge Router";
                vlan-id 100;
                family inet {
                    address <IP-Address/Subnet-mask>;
                }
            }
        }
    }


### <a name="2-create-two-security-zones"></a>2. Utwórz dwie strefy zabezpieczeń

 - Strefy zaufania dla sieci wewnętrznej i Untrust dla sieci zewnętrznej przeciwległych routery krawędzi
 - Przypisywanie odpowiednich interfejsów do strefy
 - Zezwalaj na usług na interfejsach


    zabezpieczenia {strefy {strefy zabezpieczeń zaufania {hosta-— ruch przychodzący {system usług {ping;                  } protokoły {bgp;                  interfejsy}} {reth0.100;              {}} Untrust strefy zabezpieczeń {hosta-— ruch przychodzący {system usług {ping;                  } protokoły {bgp;                  interfejsy}} {reth1.100;              }          }      }  }


### <a name="3-create-security-policies-between-zones"></a>3. Tworzenie zasad zabezpieczeń między strefami
 
    security {
        policies {
            from-zone Trust to-zone Untrust {
                policy allow-any {
                    match {
                        source-address any;
                        destination-address any;
                        application any;
                    }
                    then {
                        permit;
                    }
                }
            }
            from-zone Untrust to-zone Trust {
                policy allow-any {
                    match {
                        source-address any;
                        destination-address any;
                        application any;
                    }
                    then {
                        permit;
                    }
                }
            }
        }
    }


### <a name="4-configure-nat-policies"></a>4. Konfigurowanie zasad translatora adresów Sieciowych
 - Utwórz dwa pul translatora adresów Sieciowych. Jeden posłuży do translatora adresów Sieciowych ruchu wychodzącego do firmy Microsoft i inne firmy Microsoft do klienta.
 - Tworzenie reguł translatora adresów Sieciowych odpowiednich ruchu

        security {
            nat {
                source {
                    pool SNAT-To-ExpressRoute {
                        routing-instance {
                            External-ExpressRoute;
                        }
                        address {
                            <NAT-IP-address/Subnet-mask>;
                        }
                    }
                    pool SNAT-From-ExpressRoute {
                        routing-instance {
                            Internal;
                        }
                        address {
                            <NAT-IP-address/Subnet-mask>;
                        }
                    }
                    rule-set Outbound_NAT {
                        from routing-instance Internal;
                        to routing-instance External-ExpressRoute;
                        rule SNAT-Out {
                            match {
                                source-address 0.0.0.0/0;
                            }
                            then {
                                source-nat {
                                    pool {
                                        SNAT-To-ExpressRoute;
                                    }
                                }
                            }
                        }
                    }
                    rule-set Inbound-NAT {
                        from routing-instance External-ExpressRoute;
                        to routing-instance Internal;
                        rule SNAT-In {
                            match {
                                source-address 0.0.0.0/0;
                            }
                            then {
                                source-nat {
                                    pool {
                                        SNAT-From-ExpressRoute;
                                    }
                                }
                            }
                        }
                    }
                }
            }
        }


### <a name="5-configure-bgp-to-advertise-selective-prefixes-in-each-direction"></a>5. Konfigurowanie BGP do ogłaszanie selektywne prefiksy w każdym kierunku

Zapoznaj się z próbki na stronie [próbki konfiguracji routingu](expressroute-config-samples-routing.md) .

### <a name="6-create-policies"></a>6. Tworzenie zasad

    routing-options {
                  autonomous-system <Customer-ASN>;
    }
    policy-options {
        prefix-list Microsoft-Prefixes {
            <IP-Address/Subnet-Mask;
            <IP-Address/Subnet-Mask;
        }
        prefix-list private-ranges {
            10.0.0.0/8;
            172.16.0.0/12;
            192.168.0.0/16;
            100.64.0.0/10;
        }
        policy-statement Advertise-NAT-Pools {
            from {
                protocol static;
                route-filter <NAT-Pool-Address/Subnet-mask> prefix-length-range /32-/32;
            }
            then accept;
        }
        policy-statement Accept-from-Microsoft {
            term 1 {
                from {
                    instance External-ExpressRoute;
                    prefix-list-filter Microsoft-Prefixes orlonger;
                }
                then accept;
            }
            term deny {
                then reject;
            }
        }
        policy-statement Accept-from-Internal {
            term no-private {
                from {
                    instance Internal;
                    prefix-list-filter private-ranges orlonger;
                }
                then reject;
            }
            term bgp {
                from {
                    instance Internal;
                    protocol bgp;
                }
                then accept;
            }
            term deny {
                then reject;
            }
        }
    }
    routing-instances {
        Internal {
            instance-type virtual-router;
            interface reth0.100;
            routing-options {
                static {
                    route <NAT-Pool-IP-Address/Subnet-mask> discard;
                }
                instance-import Accept-from-Microsoft;
            }
            protocols {
                bgp {
                    group customer {
                        export <Advertise-NAT-Pools>;
                        peer-as <Customer-ASN-1>;
                        neighbor <BGP-Neighbor-IP-Address>;
                    }
                }
            }
        }
        External-ExpressRoute {
            instance-type virtual-router;
            interface reth1.100;
            routing-options {
                static {
                    route <NAT-Pool-IP-Address/Subnet-mask> discard;
                }
                instance-import Accept-from-Internal;
            }
            protocols {
                bgp {
                    group edge-router {
                        export <Advertise-NAT-Pools>;
                        peer-as <Customer-Public-ASN>;
                        neighbor <BGP-Neighbor-IP-Address>;
                    }
                }
            }
        }
    }

## <a name="next-steps"></a>Następne kroki

Zobacz [ExpressRoute — często zadawane pytania](expressroute-faqs.md) , aby uzyskać więcej informacji.
