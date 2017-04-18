<properties
   pageTitle="Przykłady konfiguracji routera klienta ExpressRoute | Microsoft Azure"
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

# <a name="router-configuration-samples-to-setup-and-manage-routing"></a>Przykłady konfiguracji routera konfiguracji i zarządzać nimi routingu

Ta strona zawiera interfejs i routingu przykłady konfiguracji dla routera serii Cisco IOS-XE i Juniper MX. Te mają być próbki tylko do celów informacyjnych i nie mogą być używane, tak jak. Możesz pracować z dostawcą opracowywane konfiguracji odpowiednie dla Twojej sieci. 

>[AZURE.IMPORTANT] Przykłady na tej stronie mają być czysto orientacji. Należy współpracy z z dostawcą sprzedaż / technicznych zespołu i sieci zespół opracowywane odpowiednie układy, stosownie do potrzeb użytkownika. Microsoft nie obsługuje problemy związane z konfiguracji opisanych na tej stronie. Problemów należy skontaktować się z dostawcą urządzenia.

Poniżej pokazano konfiguracji routera Zastosuj do wszystkich peerings. Przegląd [ExpressRoute peerings](expressroute-circuit-peerings.md) i [wymagań dotyczących routingu ExpressRoute](expressroute-routing.md) więcej informacji na temat kierowania.

## <a name="cisco-ios-xe-based-routers"></a>Cisco IOS-XE podstawie routery

Przykłady w tej sekcji dotyczą dowolnego routera rodziny z systemem operacyjnym IOS XE.

### <a name="1-configuring-interfaces-and-sub-interfaces"></a>1. Konfigurowanie interfejsach i podrzędny

Będzie wymagało interfejs sub na zaglądanie w każdej routera służący do firmy Microsoft. Interfejs podsieci można określić przy użyciu Identyfikatora VLAN lub skumulowany pary VLAN nazwy i adresu IP.

#### <a name="dot1q-interface-definition"></a>Definicja interfejsu Dot1Q

W tym przykładzie przedstawiono definicję interfejsu podrzędnego interfejsu podrzędnego z jednym identyfikatorem VLAN. Identyfikatora sieci jest unikatowy dla zaglądanie. Ostatni oktet adresu IP protokołu IPv4 jest zawsze nieparzysta.

    interface GigabitEthernet<Interface_Number>.<Number>
     encapsulation dot1Q <VLAN_ID>
     ip address <IPv4_Address><Subnet_Mask>

#### <a name="qinq-interface-definition"></a>Definicja interfejsu QinQ

W tym przykładzie przedstawiono definicję interfejsu podrzędnego interfejsu podrzędnego z dwóch identyfikatorów VLAN. Zewnętrzne Identyfikatora sieci (s znacznik), jeśli użyty zmienia się na wszystkich peerings. Wewnętrzne Identyfikatora sieci (c znacznik) jest unikatowy dla zaglądanie. Ostatni oktet adresu IP protokołu IPv4 jest zawsze nieparzysta.

    interface GigabitEthernet<Interface_Number>.<Number>
     encapsulation dot1Q <s-tag> seconddot1Q <c-tag>
     ip address <IPv4_Address><Subnet_Mask>
    
### <a name="2-setting-up-ebgp-sessions"></a>2. Definiowanie sesji eBGP

Musisz skonfigurować sesję BGP u firmy Microsoft dla każdej zaglądanie. Poniższe przykładowe umożliwia konfigurowanie sesji BGP z firmą Microsoft. Jeśli adres IP protokołu IPv4 używanego interfejsu pakietu sub a.b.c.d, adres IP sąsiada BGP (Microsoft) będzie a.b.c.d+1. Ostatni oktet sąsiada BGP adres IP protokołu IPv4 jest zawsze liczbą parzystą.

    router bgp <Customer_ASN>
     bgp log-neighbor-changes
     neighbor <IP#2_used_by_Azure> remote-as 12076
     !        
     address-family ipv4
     neighbor <IP#2_used_by_Azure> activate
     exit-address-family
    !

### <a name="3-setting-up-prefixes-to-be-advertised-over-the-bgp-session"></a>3. Definiowanie prefiksy jej ogłoszenia sesji BGP

Można skonfigurować routera do ogłaszanie Wybierz prefiksy do firmy Microsoft. Możesz to zrobić przy użyciu poniższych próbki.

    router bgp <Customer_ASN>
     bgp log-neighbor-changes
     neighbor <IP#2_used_by_Azure> remote-as 12076
     !        
     address-family ipv4
      network <Prefix_to_be_advertised> mask <Subnet_mask>
      neighbor <IP#2_used_by_Azure> activate
     exit-address-family
    !

### <a name="4-route-maps"></a>4. mapy rozsyłania

Możesz użyć mapy rozsyłania i prefiks list filtru prefiksy propagowane w sieci. Poniższe przykładowe służy do wykonywania tego zadania. Upewnij się, że ustawienia list prefiksu.

    router bgp <Customer_ASN>
     bgp log-neighbor-changes
     neighbor <IP#2_used_by_Azure> remote-as 12076
     !        
     address-family ipv4
      network <Prefix_to_be_advertised> mask <Subnet_mask>
      neighbor <IP#2_used_by_Azure> activate
      neighbor <IP#2_used_by_Azure> route-map <MS_Prefixes_Inbound> in
     exit-address-family
    !
    route-map <MS_Prefixes_Inbound> permit 10
     match ip address prefix-list <MS_Prefixes>
    !


## <a name="juniper-mx-series-routers"></a>Routery serii juniper MX 

Przykłady w tej sekcji dotyczą routerach serii Juniper MX.

### <a name="1-configuring-interfaces-and-sub-interfaces"></a>1. Konfigurowanie interfejsach i podrzędny

#### <a name="dot1q-interface-definition"></a>Definicja interfejsu Dot1Q

W tym przykładzie przedstawiono definicję interfejsu podrzędnego interfejsu podrzędnego z jednym identyfikatorem VLAN. Identyfikatora sieci jest unikatowy dla zaglądanie. Ostatni oktet adresu IP protokołu IPv4 jest zawsze nieparzysta.

    interfaces {
        vlan-tagging;
        <Interface_Number> {
            unit <Number> {
                vlan-id <VLAN_ID>;
                family inet {
                    address <IPv4_Address/Subnet_Mask>;
                }
            }
        }
    }


#### <a name="qinq-interface-definition"></a>Definicja interfejsu QinQ

W tym przykładzie przedstawiono definicję interfejsu podrzędnego interfejsu podrzędnego z dwóch identyfikatorów VLAN. Zewnętrzne Identyfikatora sieci (s znacznik), jeśli użyty zmienia się na wszystkich peerings. Wewnętrzne Identyfikatora sieci (c znacznik) jest unikatowy dla zaglądanie. Ostatni oktet adresu IP protokołu IPv4 zawsze jest nieparzysta.

    interfaces {
        <Interface_Number> {
            flexible-vlan-tagging;
            unit <Number> {
                vlan-tags outer <S-tag> inner <C-tag>;
                family inet {
                    address <IPv4_Address/Subnet_Mask>;
                }                           
            }                               
        }                                   
    }                           

### <a name="2-setting-up-ebgp-sessions"></a>2. Definiowanie sesji eBGP

Musisz skonfigurować sesję BGP u firmy Microsoft dla każdej zaglądanie. Poniższe przykładowe umożliwia konfigurowanie sesji BGP z firmą Microsoft. Jeśli adres IP protokołu IPv4 używanego interfejsu pakietu sub a.b.c.d, adres IP sąsiada BGP (Microsoft) będzie a.b.c.d+1. Ostatni oktet sąsiada BGP adres IP protokołu IPv4 jest zawsze liczbą parzystą.

    routing-options {
        autonomous-system <Customer_ASN>;
    }
    }
    protocols {
        bgp { 
            group <Group_Name> { 
                peer-as 12076;              
                neighbor <IP#2_used_by_Azure>;
            }                               
        }                                   
    }

### <a name="3-setting-up-prefixes-to-be-advertised-over-the-bgp-session"></a>3. Definiowanie prefiksy jej ogłoszenia sesji BGP

Można skonfigurować routera do ogłaszanie Wybierz prefiksy do firmy Microsoft. Możesz to zrobić przy użyciu poniższych próbki.

    policy-options {
        policy-statement <Policy_Name> {
            term 1 {
                from protocol OSPF;
        route-filter <Prefix_to_be_advertised/Subnet_Mask> exact;
                then {
                    accept;
                }
            }
        }
    }
    protocols {
        bgp { 
            group <Group_Name> { 
                export <Policy_Name>
                peer-as 12076;              
                neighbor <IP#2_used_by_Azure>;
            }                               
        }                                   
    }


### <a name="4-route-maps"></a>4. mapy rozsyłania

Możesz użyć mapy rozsyłania i prefiks list filtru prefiksy propagowane w sieci. Poniższe przykładowe służy do wykonywania tego zadania. Upewnij się, że ustawienia list prefiksu.

    policy-options {
        prefix-list MS_Prefixes {
            <IP_Prefix_1/Subnet_Mask>;
            <IP_Prefix_2/Subnet_Mask>;
        }
        policy-statement <MS_Prefixes_Inbound> {
            term 1 {
                from {
        prefix-list MS_Prefixes;
                }
                then {
                    accept;
                }
            }
        }
    }
    protocols {
        bgp { 
            group <Group_Name> { 
                export <Policy_Name>
                import <MS_Prefixes_Inbound>
                peer-as 12076;              
                neighbor <IP#2_used_by_Azure>;
            }                               
        }                                   
    }

## <a name="next-steps"></a>Następne kroki

Zobacz [ExpressRoute — często zadawane pytania](expressroute-faqs.md) , aby uzyskać więcej informacji.
