<properties
   pageTitle="Zabezpieczanie klastrze z systemem Windows przy użyciu zabezpieczeń systemu Windows | Microsoft Azure"
   description="Dowiedz się, jak skonfigurować zabezpieczenia węzła do węzła i węzeł klient w klastrze autonomiczny z systemem Windows przy użyciu zabezpieczeń systemu Windows."
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/25/2016"
   ms.author="ryanwi"/>


# <a name="secure-a-standalone-cluster-on-windows-using-windows-security"></a>Zabezpieczanie klastrze autonomicznego w systemie Windows przy użyciu zabezpieczeń systemu Windows

Aby zapobieganie nieupoważnionemu dostępowi do klastrów tkaninie usługi należy zabezpieczyć, zwłaszcza w przypadku, gdy został uruchomiony obciążenia produkcji. W tym artykule opisano sposób konfigurowania zabezpieczeń węzła do węzła i węzeł klient przy użyciu zabezpieczeń systemu Windows w pliku *ClusterConfig.JSON* i odpowiada zabezpieczeń Konfiguruj krok [Utwórz klaster autonomicznego systemem Windows](service-fabric-cluster-creation-for-windows-server.md). Aby uzyskać więcej informacji o używaniu tkaninie usługi zabezpieczeń systemu Windows zobacz [scenariusze zabezpieczeń klaster](service-fabric-cluster-security.md).

>[AZURE.NOTE]
Wybór zabezpieczeń zabezpieczeń węzła do węzła należy rozważyć umiarem, ponieważ nie ma możliwości uaktualnienia klaster z wybór jednej zabezpieczeń do drugiego. Zmienianie zaznaczenia zabezpieczeń wymaga Odbuduj pełny klaster.

## <a name="configure-windows-security"></a>Konfigurowanie zabezpieczeń systemu Windows
Przykładowy plik konfiguracyjny *ClusterConfig.Windows.JSON* są pobierane razem z [Microsoft.Azure.ServiceFabric.WindowsServer.<version>. ZIP](http://go.microsoft.com/fwlink/?LinkId=730690) pakiet klaster autonomiczny zawiera szablon do konfigurowania zabezpieczeń systemu Windows.  W sekcji **Właściwości** skonfigurowano zabezpieczeń systemu Windows:

```
"security": {
            "ClusterCredentialType": "Windows",
            "ServerCredentialType": "Windows",
            "WindowsIdentities": {
        "ClusterIdentity" : "[domain\machinegroup]",
                "ClientIdentities": [{
                    "Identity": "[domain\username]",
                    "IsAdmin": true
                }]
            }
        }
```

|**Ustawienie konfiguracji**|**Opis**|
|-----------------------|--------------------------|
|ClusterCredentialType|Zabezpieczenia systemu Windows jest włączona, ustawiając parametr **ClusterCredentialType** do *systemu Windows*.|
|ServerCredentialType|Zabezpieczenia systemu Windows dla klientów jest włączona przez ustawienie parametru **ServerCredentialType** do *systemu Windows*. Oznacza to, że klienci klastrem i klastrem są uruchomione w ramach domeny usługi Active Directory.|
|WindowsIdentities|Zawiera tożsamości klaster i klienta.|
|ClusterIdentity|Konfiguruje zabezpieczenia węzła do węzła. Rozdzielaną średnikami listę grupy zarządza nazwy komputera lub konta usługi.|
|ClientIdentities|Konfiguruje zabezpieczenia węzeł klient. Tablica kont użytkowników klienta.|
|Tożsamości|Tożsamość klienta użytkownika domeny.|
|IsAdmin|PRAWDA Określa, że użytkownik domeny ma administratora dostępu klienta, FAŁSZ dla dostępu klienta użytkownika.|

[Węzeł bezpieczeństwa węzeł](service-fabric-cluster-security.md#node-to-node-security) jest konfigurowany przez ustawienie przy użyciu **ClusterIdentity**. Aby utworzyć relacje zaufania między węzły, ich należy pamiętać o od siebie. Można to zrobić na dwa sposoby: można określić grupy zarządzane konto usługi zawierająca wszystkie węzły w klastrze lub tożsamości węzeł domeny wszystkich węzłów w klastrze. Zdecydowanie zaleca się przy użyciu metody [Grupy zarządzanych kont usług (gMSA)](https://technet.microsoft.com/library/hh831782.aspx) , szczególnie w przypadku większych klastrów (więcej niż 10 węzłach) lub dla klastrów, które mogą powiększyć lub pomniejszyć.
Ta metoda umożliwia węzły dodawane lub usuwane z gMSA, bez wprowadzania zmian w manifeście klaster. Ta metoda nie wymaga tworzenia grupy domeny, do której administratorzy klaster udzielono uprawnień do dodawania i usuwania członków. Aby uzyskać więcej informacji zobacz [Wprowadzenie do zarządzanych kont usług grupy](http://technet.microsoft.com/library/jj128431.aspx).

[Klient bezpieczeństwa węzeł](service-fabric-cluster-security.md#client-to-node-security) jest skonfigurowany, przy użyciu **ClientIdentities**. W celu ustalenia zaufania między klientem a klastrem, musisz skonfigurować klaster wiedzieć wybrać klienta tożsamości, które mogą ufać. Można to zrobić na dwa sposoby: Określ użytkowników grupy domeny, których można połączyć lub określić użytkowników węzeł domeny łączących się. Tkaninie usługi obsługuje dwa typy kontroli dostępu innym dla klientów, którzy są połączeni z klastrem tkaninie usługi: administratorów i użytkowników. Kontrola dostępu umożliwia dla administratora klastrów, aby ograniczyć dostęp do niektórych rodzajów operacji klaster dla różnych grup użytkowników, zwiększanie bezpieczeństwa klaster.  Administratorzy mają pełny dostęp do funkcji zarządzania (w tym możliwości odczytu/zapisu). Jeśli jesteś użytkownikiem, domyślnie mają tylko do odczytu w możliwościach zarządzania (na przykład możliwość wykonywania kwerend) i możliwość usuwania aplikacji i usług.

Następną sekcję **zabezpieczeń** przykład konfiguruje zabezpieczeń systemu Windows i określa, że komputery w *ServiceFabric/clusterA.contoso.com* są częścią klaster i że *CONTOSO\usera* ma dostęp klienta administratora:

```
"security": {
    "ClusterCredentialType": "Windows",
    "ServerCredentialType": "Windows",
    "WindowsIdentities": {
        "ClusterIdentity" : "ServiceFabric/clusterA.contoso.com",
        "ClientIdentities": [{
            "Identity": "CONTOSO\\usera",
        "IsAdmin": true
        }]
    }
},
```

## <a name="next-steps"></a>Następne kroki

Po skonfigurowaniu zabezpieczeń systemu Windows w pliku *ClusterConfig.JSON* , wznowić proces tworzenia klaster w [Utwórz klaster autonomicznego systemem Windows](service-fabric-cluster-creation-for-windows-server.md).

Aby uzyskać więcej informacji na temat zabezpieczeń węzła do węzła, zabezpieczenia węzeł klient i kontrola dostępu oparta na rolach, zobacz [scenariusze zabezpieczeń klaster](service-fabric-cluster-security.md).

Zobacz [Łączenie z klastrem bezpiecznego](service-fabric-connect-to-secure-cluster.md) przykłady nawiązywanie połączenia za pomocą programu PowerShell lub FabricClient.
