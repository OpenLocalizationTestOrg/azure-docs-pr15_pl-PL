<properties
    pageTitle="Omówienie autoscale w środowisku maszyn wirtualnych systemu Microsoft Azure, usług w chmurze i aplikacjach sieci Web | Microsoft Azure"
    description="Omówienie autoscale platformy Microsoft Azure. Dotyczy maszyn wirtualnych, usług w chmurze i aplikacji sieci Web."
    authors="rboucher"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="robb"/>

# <a name="overview-of-autoscale-in-microsoft-azure-virtual-machines-cloud-services-and-web-apps"></a>Omówienie autoscale w środowisku maszyn wirtualnych systemu Microsoft Azure, usług w chmurze i aplikacji sieci Web

W tym artykule opisano, jakie autoscale Microsoft Azure, korzyści oraz jak wprowadzenie do korzystania z niego.  

Azure Monitor autoscale dotyczy tylko [Zestawy skali maszyn wirtualnych](https://azure.microsoft.com/services/virtual-machine-scale-sets/), [Usług w chmurze](https://azure.microsoft.com/services/cloud-services/)i [Aplikacji usługi — aplikacje sieci Web](https://azure.microsoft.com/services/app-service/web/).

>[AZURE.NOTE] Azure występują dwie metody autoscale. Ze starszej wersji programu autoscale dotyczy maszyn wirtualnych (dostępność zestawy). Ta funkcja jest ograniczona pomocy technicznej i zaleca się migrowanie do zestawów skali maszyn wirtualnych obsługi autoscale szybsze i bardziej niezawodne. Łącze dotyczące sposobu używania starszych technologiach znajduje się w tym artykule.  


## <a name="what-is-autoscale"></a>Co to jest autoscale?

Autoscale pozwala na prawo ilość zasobów do obsługi obciążenia w swojej aplikacji. Umożliwia dodanie zasobów w celu obsługi wzrost obciążenia i również oszczędności poprzez usunięcie zasobów, które są dostępne jako bezczynny. Określ minimalną i maksymalną liczbę wystąpień uruchamianie i dodawanie lub usuwanie maszyny wirtualne automatycznie w oparciu o zestaw reguł. O minimalne powoduje, że się, że aplikacja zawsze działa nawet nie obciążeniu. O maksymalnej ogranicza usługi możliwe co godzinę kosztów całkowitych. Automatyczne skalowanie między te dwie wartości graniczne przy użyciu reguł, które można utworzyć.

 ![Omówienie Autoscale. Dodawanie i usuwanie maszyny wirtualne](./media/monitoring-autoscale-overview/AutoscaleConcept.png)

Gdy warunki reguły są spełnione, zostanie wywołana jednego lub większej liczby akcji autoscale. Możesz dodać i usunąć maszyny wirtualne lub wykonać inne akcje. Poniższy diagram koncepcyjny przedstawia ten proces.  

 ![Diagram koncepcyjny Autoscale przepływu](./media/monitoring-autoscale-overview/AutoscaleOverview3.png)


## <a name="autoscale-process-explained"></a>Omówienie procesu Autoscale
Poniżej informacje dotyczą części poprzedniego diagramu.   

### <a name="resource-metrics"></a>Metryki zasobów
Zasoby wysyłać miar, które później są przetwarzane przez reguły. Metryki są formatowane za pomocą różnych metod.
Zestawy skali maszyn wirtualnych korzysta z danych telemetrycznego czynnikami diagnostyki Azure należy telemetrycznego dla aplikacji sieci Web i usług w chmurze pochodzi bezpośrednio z infrastruktury Azure. Niektóre często używane statystyka zawiera użycie Procesora, użycie pamięci zlicza wątku, długość kolejki i użycie dysku. Aby uzyskać listę danych telemetrycznych można użyć zobacz [Metryki typowych Autoscale](insights-autoscale-common-metrics.md).

### <a name="time"></a>Czas
Reguły oparte na harmonogram są oparte na UTC. Strefy czasowej należy ustawić prawidłowo, podczas konfigurowania reguł.  

### <a name="rules"></a>Reguły
Diagram zawiera tylko jedną regułę autoscale, ale może mieć wiele z nich. Można utworzyć złożone reguły nakładające się, w razie potrzeby w danej sytuacji.  Typy reguł  

 - **Oparte na metryczne** — na przykład czynności do wykonania po użycie Procesora powyżej 50%.
 - **Opartych na czasie** — na przykład wyzwalacza webhook każdej 8 am w sobotę danej strefy czasowej.

Reguły oparte na metryczne zmierzyć ładowania aplikacji i dodawanie lub usuwanie maszyny wirtualne oparte na tym Załaduj. Reguły oparte na harmonogram umożliwia skalowanie Zobacz desenie czasu w swojej obciążenia i chcesz, aby przeskalować przed zwiększona o możliwych ładowania lub Zmniejsz występuje.  


### <a name="actions-and-automation"></a>Akcje i automatyzacji

Reguły można uruchamiać jeden lub więcej typów akcji.

- **Skala** - maszyny wirtualne skali lub pomniejszyć
- **Wiadomości e-mail** — wysyłanie wiadomości e-mail do administratorów subskrypcji, administratorów współpracujących i/lub adresu e-mail dodatkowe
- **Automatyzacja za pośrednictwem webhooks** - webhooks połączenia, które może wyzwolić wielu złożonych akcji lub poza Azure. Wewnątrz Azure można uruchomić działań aranżacji automatyzacji Azure, funkcja Azure lub Azure logiczny aplikacji. Przykład 3 URL innych firm poza Azure obejmują usługi, takie jak zapas czasu i Twilio.


## <a name="autoscale-settings"></a>Ustawienia Autoscale
Autoscale za pomocą następujących terminologia i struktura.

- **Ustawienie autoscale** jest do odczytu przy użyciu aparatu autoscale, aby określić, czy chcesz skalować w górę lub w dół. Zawiera jedną lub więcej profilów, informacje o zasobu docelowego i ustawień powiadomień.
    - **Profil autoscale** to kombinacja pojemności ustawienie zestawu zasady wyzwalaczami i skali akcji dla profilu i danymi o cykliczności. Można mieć wiele profilów, które umożliwiają zajmować odmienne wymagania nakładające się.
        - **Ustawienie możliwości** wskazuje, minimum, maksimum i wartości domyślnych dla liczbę wystąpień. [odpowiednim miejscu umożliwia figowych 1]
        - **Reguła** zawiera wyzwalacza — metryczne wyzwalacza i wyzwalacz czas — i akcję skali wskazującą, czy autoscale powinien skalowanie w górę lub w dół po tej reguły jest spełniony.
        - **Cykl** wskazuje, kiedy autoscale powinny obowiązywać tego profilu. Na przykład możesz mieć profile autoscale różne dla różnych porach dzień lub dni tygodnia.
- **Ustawienie powiadomień** Określa, jakie powiadomienia powinna zostać wykonana po wystąpieniu zdarzenia autoscale oparte na spełniające kryteria jeden z profili ustawienie autoscale. Autoscale można powiadomienia o tym, co najmniej jeden adres e-mail lub nawiązywania połączeń z co najmniej jeden webhooks.

![Ustawienie Azure autoscale, profil i struktury reguły](./media/monitoring-autoscale-overview/AzureResourceManagerRuleStructure3.png)

Pełna lista pól można konfigurować i opisy jest dostępna w [Interfejsu API usługi REST Autoscale](https://msdn.microsoft.com/library/dn931928.aspx).

Aby uzyskać przykłady kodu zobacz

* [Konfiguracja zaawansowana Autoscale przy użyciu szablonów Menedżera zasobów dla zestawów skali maszyn wirtualnych](insights-advanced-autoscale-virtual-machine-scale-sets.md)  
* [Autoscale interfejsu API usługi REST](https://msdn.microsoft.com/library/dn931953.aspx)



## <a name="horizontal-vs-vertical-scaling"></a>Skalowanie pionowe w porównaniu z poziomej

Autoscale zwiększa zasobów w skali tylko poziomo, które jest większa ("się") lub spadek ("") w polu Liczba wystąpień maszyn wirtualnych.  Poziome skalowania, który jest bardziej elastyczne w sytuacji, w chmurze, jako umożliwia uruchamianie potencjalnie tysiące pośrednictwem SMS do obsługi obciążenia. Skalowanie pionowe różni się. Zachowuje taką samą liczbę maszyny wirtualne, ale powoduje, że maszyn wirtualnych więcej ("w górę") lub mniej ("w dół") zaawansowane. Power jest mierzony w pamięci, szybkość Procesora, miejsca na dysku itd.  Skalowanie pionowe ma więcej ograniczeń. Jest zależna od dostępności sprzętu większe, które mogą się różnić według regionów i szybko trafienia i górną granicę. Skalowanie pionowe również zazwyczaj wymaga start i stop maszyn wirtualnych. Aby uzyskać więcej informacji zobacz [pionowo skalowanie Azure maszyn wirtualnych przy użyciu automatyzacji Azure](../virtual-machines/virtual-machines-linux-vertical-scaling-automation.md).


## <a name="methods-of-access"></a>Metody programu access
Możesz skonfigurować autoscale za pośrednictwem

- [Azure portal](insights-how-to-scale.md)
- [Programu PowerShell](insights-powershell-samples.md#create-and-manage-autoscale-settings)
- [Interfejs wiersza polecenia i platform (polecenie)](insights-cli-samples.md#autoscale )
- [Monitorowanie Azure interfejsu API usługi REST](https://msdn.microsoft.com/library/azure/dn931953.aspx )

## <a name="supported-services-for-autoscale"></a>Obsługiwane usługi dla autoscale


| Usługa                              | Schemat i dokumentów                                       |
|--------------------------------------|-----------------------------------------------------|
| Aplikacje sieci Web                             | [Skalowanie aplikacji sieci Web](insights-how-to-scale.md)              |
| Usług w chmurze                       | [Autoscale usługi w chmurze](../cloud-services/cloud-services-how-to-scale.md) |
| Maszyn wirtualnych: klasyczny           | [Skalowanie zestawy dostępność klasyczny maszyn wirtualnych](https://blogs.msdn.microsoft.com/kaevans/2015/02/20/autoscaling-azurevirtual-machines/) |
| Środowisku maszyn wirtualnych systemu: Zestawy skali systemu Windows| [Skalowanie skali maszyn wirtualnych ustawia w systemie Windows](../virtual-machine-scale-sets/virtual-machine-scale-sets-windows-autoscale.md)  |
| Maszyn wirtualnych: Ustawia skali Linux  | [Skalowanie skali maszyn wirtualnych zestawów w Linux](../virtual-machine-scale-sets/virtual-machine-scale-sets-linux-autoscale.md) |
| Środowisku maszyn wirtualnych systemu: Przykład systemu Windows   | [Konfiguracja zaawansowana Autoscale przy użyciu szablonów Menedżera zasobów dla zestawów skali maszyn wirtualnych](insights-advanced-autoscale-virtual-machine-scale-sets.md) |

## <a name="next-steps"></a>Następne kroki

Aby dowiedzieć się więcej na temat autoscale, użyj instruktaży Autoscale wymienionych wcześniej lub zobacz następujące zasoby:

- [Azure metryki typowych autoscale monitora](insights-autoscale-common-metrics.md)
- [Najważniejsze wskazówki dotyczące autoscale Azure Monitor](insights-autoscale-best-practices.md)
- [Wysyłanie wiadomości e-mail oraz webhook alertów za pomocą akcji autoscale](insights-autoscale-to-webhook-email.md)
- [Autoscale interfejsu API usługi REST](https://msdn.microsoft.com/library/dn931953.aspx)
- [Rozwiązywania problemów Autoscale zestawy skali maszyn wirtualnych](../virtual-machine-scale-sets/virtual-machine-scale-sets-troubleshoot.md)
