<properties 
   pageTitle="Modyfikowanie konfiguracji urządzenia StorSimple | Microsoft Azure" 
   description="Informacje dotyczące używania usługi Menedżera StorSimple konfigurację urządzenia StorSimple, który już został wdrożony." 
   services="storsimple" 
   documentationCenter="NA" 
   authors="SharS" 
   manager="carmonm" 
   editor=""/>

<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD" 
   ms.date="09/29/2016"
   ms.author="v-sharos"/>

# <a name="use-the-storsimple-manager-service-to-modify-your-storsimple-device-configuration"></a>Aby zmodyfikować konfigurację urządzeń StorSimple za pomocą usługi Menedżera StorSimple

## <a name="overview"></a>Omówienie 

Azure portal klasyczny strony **Konfigurowanie** zawiera wszystkie parametry urządzenia, które można zmienić na urządzeniu StorSimple, która zarządza usługa Menedżera StorSimple. Ten samouczek wyjaśniono, jak na stronie **Konfigurowanie** umożliwia wykonywanie następujących zadań na poziomie urządzenia:

- Modyfikowanie ustawień urządzenia 
- Modyfikowanie ustawień czasu 
- Modyfikowanie ustawień DNS 
- Modyfikowanie interfejsów sieciowych
- Zamień lub Przydziel ponownie adresy IP

## <a name="modify-device-settings"></a>Modyfikowanie ustawień urządzenia

Ustawienia urządzenia zawierać przyjazną nazwę urządzenia i opis urządzenia.

Urządzenia StorSimple, który jest połączony z usługą Menedżera StorSimple przypisano nazwę domyślną. Domyślna nazwa zazwyczaj odzwierciedla numer seryjny urządzenia. Na przykład domyślna nazwa urządzenia to 15 znaków, takich jak 8600 SHX0991003G44HT, oznacza następujące czynności:

- **8600** — wskazuje modelu urządzenia.
- **SHX** — wskazuje miejsce produkcji.
- **0991003** - wskazuje określonego produktu.
- **G44HT**- 5 ostatnich cyfr są zwiększane, aby utworzyć unikatowe numery seryjne. Nie może to być zestaw sekwencyjne.

Klasyczny portalu Azure umożliwia zmiana nazwy urządzenia i przypisz mu unikatową przyjazną nazwę wybranych przez użytkownika. Przyjazna nazwa może zawierać znaków i może zawierać maksymalnie 64 znaki.

Można również określić opis urządzenia. Opis urządzenia pomaga zazwyczaj identyfikowanie właściciel i lokalizacji fizycznej urządzenia. Opis pole musi zawierać mniej niż 256 znaków.
 
## <a name="modify-time-settings"></a>Modyfikowanie ustawień czasu

Urządzenie, należy zsynchronizować czasu w celu uwierzytelnienia u usługodawcy miejsca do magazynowania chmury. Wybierz strefę czasową z listy rozwijanej, a następnie określ do dwóch serwerów czasu NTP (Network Protocol). Podstawowy serwer NTP jest wymagany i określono podczas konfigurowania urządzenia za pomocą programu Windows PowerShell dla StorSimple. Windows Server domyślny **time.windows.com** można określić jako serwera NTP. Można wyświetlić podstawowego konfiguracji serwera NTP za pośrednictwem portalu klasyczny Azure, ale należy użyć interfejsu programu Windows PowerShell, aby go zmienić.

Pomocnicza konfiguracji serwera NTP jest opcjonalna. Za pomocą portalu klasyczny skonfigurowanie serwera NTP pomocniczego. 

Podczas konfigurowania serwera NTP, upewnij się, że sieci zezwala na ruch NTP przekazywania z centrum danych z Internetem. Określając publicznej serwera NTP, należy się upewnić, że z zapory sieciowe i innych urządzeń zabezpieczeń są skonfigurowane do zezwalanie na ruch NTP do i z sieci zewnętrznej. Jeśli dwukierunkowy NTP ruch nie jest dozwolony, należy użyć na serwerze wewnętrznym NTP (Ta funkcja zapewnia kontrolera domeny systemu Windows). Jeśli urządzenia nie może zsynchronizować czas, nie można komunikować się z dostawcy miejsca do magazynowania w chmurze.

Aby wyświetlić listę serwerów NTP publicznych, przejdź do [NTP serwery w sieci Web](http://support.ntp.org/bin/view/Servers/WebHome). 

### <a name="what-happens-if-the-device-is-deployed-in-a-different-time-zone"></a>Co się stanie, gdy urządzenie jest wdrożona w innej strefie czasowej?

Jeśli urządzenie jest wdrożona w innej strefie czasowej, strefa czasowa urządzenie ulegnie zmianie. Zakładając, że wszystkie kopii zapasowej zasady za pomocą strefę czasową urządzenia, zasady kopii zapasowej automatycznie dopasuje zgodnie z nowej strefy czasowej. Wymagane jest nie udziału użytkownika.

## <a name="modify-dns-settings"></a>Modyfikowanie ustawień DNS

Serwer DNS jest używany podczas urządzenie próbuje komunikować się z miejsca do magazynowania chmury usługodawcy. Wysoką dostępność są wymagane do skonfigurowania podstawowych i pomocniczych serwerów DNS podczas wdrażania początkowej urządzenia. O skonfigurowanie podstawowy serwer DNS, konieczne będzie korzystać z interfejsu programu Windows PowerShell na urządzeniu StorSimple.

Aby zmodyfikować pomocniczy serwer DNS, możesz użyć portalu klasyczny Azure.



## <a name="modify-network-interfaces"></a>Modyfikowanie interfejsów sieciowych

Urządzenie ma sześć urządzenia interfejsów, z których cztery 1 GbE i dwie z nich jest 10 GbE. Interfejsy te są oznaczone jako dane 0 – 5 danych. DANE 0, 1 danych, danych 4 i 5 danych są 1 GbE danych 2 i 3 dane są 10 GbE interfejsów.

Konfigurowanie **Ustawień interfejsu sieci** dla każdego z interfejsów ma być używany. W celu zapewnienia wysokiej dostępności, zalecamy mieć co najmniej dwa iSCSI interfejsach i dwóch obsługiwanych w chmurze na urządzeniu. Firma Microsoft zaleca, ale nie wymagają wyłączone nieużywane interfejsów.

Po skonfigurowaniu w interfejsach sieci, musisz skonfigurować wirtualnych IP (VIP).

DANE 0 jest chmury domyślnie. Podczas konfigurowania danych 0, również musisz skonfigurować dwóch stałych adresy IP, jedną dla każdego kontrolera. Te stałe adresy IP może służyć do uzyskują bezpośredni dostęp do kontrolerów urządzeń i są przydatne podczas instalowania aktualizacji na urządzeniu lub podczas uzyskiwania dostępu do kontrolerów w celu rozwiązywania problemów.

StorSimple 8000 serii Update 1 metryki routingu danych 0 jest ustawiona do najniższego; w związku z tym jeśli Twoje urządzenie działa StorSimple 8000 serii Update 1, cały ruch chmury będą kierowane do danych 0. Należy zwrócić uwagę to, jeśli masz więcej niż jeden interfejs sieci chmury na urządzeniu StorSimple.

>[AZURE.NOTE] Stały adresów IP dla kontrolera są używane do obsługi aktualizacji do urządzenia. Dlatego stały adresy IP muszą być routingu i mógł połączyć się z Internetem.

Dla każdego interfejsu sieciowego są wyświetlane następujące parametry:

- **Szybkość** — nie można konfigurować użytkownika parametru. DANE 0, 1 danych, danych 4 i 5 dane są zawsze 1 GbE danych 2 i 3 dane są 10 GbE interfejsów.

     >[AZURE.NOTE] Szybkość i dupleks są zawsze automatycznie negocjowane. Duże ramki nie są obsługiwane.
 
- **Stan interfejsu** — interfejs może być włączona lub wyłączona. Jeśli funkcja jest włączona, urządzenie spróbuje za pomocą interfejsu. Zalecamy włączenia tylko tych interfejsów, które są połączone z siecią i używane. Wyłącz wszystkie interfejsy, które nie używają.

- **Typ interfejsu** — ten parametr umożliwia wyodrębnienia ruch iSCSI z ruchu magazynu w chmurze. Ten parametr może mieć jedną z następujących czynności:

    - **Chmura włączone** — po włączeniu, urządzenie przy użyciu tego interfejsu komunikowanie się z chmurą.
    - **iSCSI włączone** — po włączeniu urządzenia przy użyciu tego interfejsu komunikowanie się za pomocą hosta iSCSI.

    Zaleca się przyczyną ruch iSCSI z ruchu magazynu w chmurze. Pamiętaj również, jeśli hosta znajduje się w tej samej podsieci co urządzenia, nie musisz przypisać bramę; Jednak jeśli hosta znajduje się w innej podsieci niż urządzenia, należy przypisać bramę.

- **Adres IP** — może to być IP protokołu IPv4 lub IPv6 lub oba. Rodziny adres IP protokołu IPv4 i IPv6 są obsługiwane urządzenia interfejsów sieciowych. Podczas korzystania z protokołu IPv4, określ 32-bitowy adres IP (*xxx.xxx.xxx.xxx*) w notacji kropki dziesiętnej. Podczas korzystania z protokołu IPv6, wystarczy podać prefiks 4-cyfrowy, a adres 128-bitowego zostanie utworzona automatycznie dla swojego urządzenia sieciowej na podstawie tego prefiksu.

- **Podsieć** — to odwołuje się do maski podsieci oraz została ona skonfigurowana za pomocą interfejsu programu Windows PowerShell.

- **Brama** — jest to bramy domyślnej, który ma być używany przez ten interfejs podczas próby komunikowania się z węzłów, które nie należą do tego samego obszaru adres IP (podsieć). Brama domyślna musi być w tym samym miejscu adres (podsieć) jako interfejs w polu adres IP określoną przez maskę podsieci.

- **Adres IP stały** — w tym polu jest dostępna tylko wtedy, gdy Konfigurowanie danych 0 interfejsu. Dla operacji, takich jak aktualizacje lub Rozwiązywanie problemów z urządzenia może być konieczne bezpośrednio połączyć kontroler urządzenia. Stały adres IP może służyć do udostępniania elementów aktywności i pasywne kontroler na urządzeniu.

Można ponownie skonfigurować kontroler 0 i 1 kontroler za pośrednictwem portalu klasyczny Azure.

>[AZURE.NOTE] 
>
>- Aby zapewnić poprawne działanie, sprawdź szybkość interfejsu i drukowanie dwustronne na przełączniku każdy interfejs urządzenie jest podłączone do. Przełączanie interfejsów albo należy Wynegocjuj z lub można skonfigurować dla Gigabit Ethernet (1000 MB/s) i ich pełny dupleks. Interfejsy operacyjnym zmniejszoną szybkością lub producentem spowoduje występują problemy z wydajnością.
>
>- Aby zminimalizować zakłócenia i przestoje, zaleca się włączenie portfast dla wszystkich portów przełącznika, które interfejsu sieci iSCSI urządzenia będą łączyć się z. Temu będziesz mieć pewność, że łączność sieciowa można szybko ustalić wypadku trybie awaryjnym.
 
## <a name="swap-or-reassign-ips"></a>Zamień lub Przydziel ponownie adresy IP

Obecnie Jeżeli dowolny sieciowej na kontrolerze przydzielono VIP, która jest używana (w tym samym urządzeniu lub innego urządzenia w sieci), następnie kontroler zakończy się niepowodzeniem na. Możesz więc należy wykonać procedurę pisane z wielkiej litery, jeśli są wymienianie służącą interfejsu sieci urządzenia, ponieważ spowoduje utworzenie zduplikowanych sytuacji adresów IP.

Wykonaj poniższe czynności, aby Zamień lub Przydziel ponownie służącą dla każdego z interfejsów sieciowych:

#### <a name="to-reassign-ips"></a>Aby ponownie przydzielić adresy IP

1. Wyczyść adres IP dla obu interfejsów.

2. Po adresy IP są wyczyszczone, przypisz nowe adresy IP do odpowiednich interfejsów.

## <a name="next-steps"></a>Następne kroki

- Dowiedz się, jak [skonfigurować MPIO dla swojego urządzenia StorSimple](storsimple-configure-mpio-windows-server.md).

- Dowiedz się, jak [używać usługi Menedżera StorSimple administrowania urządzenia StorSimple](storsimple-manager-service-administration.md).
     
