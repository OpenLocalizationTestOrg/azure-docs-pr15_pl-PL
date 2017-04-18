
<properties
    pageTitle="Rozwiązywanie problemów z tworzenia zbiorów hybrydowych RemoteApp | Microsoft Azure"
    description="Dowiedz się, jak rozwiązywać problemy tworzenia zbioru hybrydowych RemoteApp"
    services="remoteapp"
    documentationCenter=""
    authors="vkbucha"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />



# <a name="troubleshoot-creating-azure-remoteapp-hybrid-collections"></a>Rozwiązywanie problemów z tworzenia zbiorów hybrydowych Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp jest są przerywane. W tym artykule [anons](https://go.microsoft.com/fwlink/?linkid=821148) , aby uzyskać szczegółowe informacje.

Kolekcja hybrydowego jest obsługiwany w i przechowuje dane w chmurze Azure, ale również umożliwia użytkownikom dostęp do danych i zasobów przechowywanych w sieci lokalnej. Użytkownicy mogą korzystać z aplikacji, logując się przy użyciu poświadczeń firmową synchronizowanych lub federacji z usługą Azure Active Directory. Można wdrażać zbioru hybrydowych korzystającego z istniejącą sieć wirtualna Azure lub można utworzyć nowy wirtualnej sieci. Zalecamy tworzenia lub używanie podsieć wirtualnej sieci z zakresu CIDR wystarczającą dla oczekiwanego przyszłego zwiększenia ilości Azure RemoteApp.

Nie utworzono jeszcze kolekcji? Zobacz [Tworzenie zbioru hybrydowych](remoteapp-create-hybrid-deployment.md) kroki.

Jeśli występują problemy z tworzenia kolekcji lub jeśli kolekcji nie działa sposób uważasz, że powinno, zapoznaj się z następujących informacji.

## <a name="your-image-is-invalid"></a>Obraz jest nieprawidłowy ##
Jeśli zobaczysz komunikat podobny do "GoldImageInvalid" podczas oczekiwania Azure do zapewniania obsługi kolekcji, oznacza to, że obraz szablonu nie spełnia [określonych wymagań obrazu](remoteapp-imagereqs.md). Tak Przejdź przeczytaj tych [wymagań](remoteapp-imagereqs.md), rozwiązywanie obrazu i spróbuj utworzyć kolekcji ponownie.



## <a name="does-your-vnet-have-network-security-groups-defined"></a>Usługi VNET ma zdefiniowane grupy zabezpieczeń sieci? ##
Jeśli masz grup zabezpieczeń sieci zdefiniowane podsieci używasz zbioru, upewnij się, te [adresy URL i porty](remoteapp-ports.md) są dostępne w obrębie danej podsieci.

Grupy zabezpieczeń sieci dodatkowe można dodać do maszyny wirtualne, używany przez Ciebie w podsieć ściślejsza kontrola.

## <a name="are-you-using-your-own-dns-servers-and-are-they-accessible-from-your-vnet-subnet"></a>Są używane serwery DNS? I są one dostępne w danej podsieci VNET? ##
>[AZURE.NOTE] Masz upewnij się, że serwery DNS w sieci VNET są zawsze zadaniami górę i zawsze spróbować rozwiązać maszyn wirtualnych obsługiwany w VNET. Nie używaj Google DNS dla tej.


Dla zbiorów hybrydowego należy używać serwerów DNS. Można je określić w schemat konfiguracji sieci lub za pośrednictwem portalu zarządzania podczas tworzenia wirtualnych sieci. Serwery DNS są używane w kolejności, że są określane w sposób pracy awaryjnej (w przeciwieństwie do okrężnego).  
Zajrzyj do [Rozpoznawania nazw maszyny wirtualne i wystąpień roli](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) aby upewnić się, że serwery DNS są skonfigurowane correcly.

Upewnij się, serwerów DNS zbioru są dostępne i dostępne z podsieci VNET określona dla tej kolekcji.

Na przykład:

    <VirtualNetworkConfiguration>
    <Dns>
      <DnsServers>
        <DnsServer name="" IPAddress=""/>
      </DnsServers>
    </Dns>
    </VirtualNetworkConfiguration>

![Definiowanie systemu DNS](./media/remoteapp-hybridtrouble/dnsvpn.png)

## <a name="are-you-using-an-active-directory-domain-controller-in-your-collection"></a>Używasz kontrolera domeny usługi Active Directory w zbiorze? ##
Obecnie tylko jedną domenę usługi Active Directory może być skojarzone z Azure RemoteApp. Kolekcja hybrydowych obsługuje tylko konta usługi Azure Active Directory, które zostały zsynchronizowane przy użyciu narzędzia DirSync z wdrożenia usługi Active Directory systemu Windows Server; w szczególności albo zsynchronizowane z opcją synchronizacji haseł synchronizowane z skonfigurowane federacyjne Active Directory Federation Services (AD FS). Musisz utworzyć niestandardowe domeny, która odpowiada sufiksu głównej nazwy użytkownika domeny dla swojej domeny lokalnej i skonfigurować integracji katalogów.

Aby uzyskać więcej informacji, zobacz [Konfigurowanie usługi Active Directory dla Azure RemoteApp](remoteapp-ad.md) .

Upewnij się, że szczegóły domeny, dostępne są prawidłowe i kontrolera domeny jest dostępny z maszyn wirtualnych, utworzone w podsieci na potrzeby aplikacji zdalnego Azure. Upewnij się również dostarczone poświadczenia konta usługi mają uprawnienia do dodawania komputerów do podanej domeny i że podanej nazwy AD będzie możliwe z DNS opisane w VNET.

## <a name="what-domain-name-did-you-specify-when-you-created-your-collection"></a>Jaka nazwa domeny czy określono po utworzeniu kolekcji? ##

Nazwa domeny, utworzone lub dodane musi być nazwą domeny wewnętrznej (nie nazwę domeny Azure AD) i musi być w formacie DNS rozpoznawana (contoso.local). Na przykład nazwa wewnętrzny usługi Active Directory (contoso.local) i Active Directory głównej nazwy użytkownika (contoso.com) — należy użyć wewnętrzna nazwa podczas tworzenia kolekcji.
