<properties
pageTitle="Zagadnienia dotyczące korzystania z obrazów maszyn wirtualnych Oracle | Microsoft Azure"
description="Więcej informacji na temat obsługiwanych konfiguracji i ograniczenia dotyczące maszyn wirtualnych Oracle w systemie Windows Server platformy Azure przed wdrożeniem."
services="virtual-machines-windows"
documentationCenter=""
manager="timlt"
authors="rickstercdn"
tags="azure-service-management"/>

<tags
ms.service="virtual-machines-windows"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="vm-windows"
ms.workload="infrastructure-services"
ms.date="09/06/2016"
ms.author="rclaus" />

#<a name="miscellaneous-considerations-for-oracle-virtual-machine-images"></a>Różne zagadnienia związane z obrazami maszyn wirtualnych programu Oracle



W tym artykule opisano zagadnienia związane z maszyn wirtualnych Oracle w Azure, które są oparte na obrazy oprogramowania Oracle dostarczony przez Oracle.  

-  Obrazy maszyn wirtualnych bazy danych Oracle
-  Oracle WebLogic Server maszyn wirtualnych obrazów
-  Oracle JDK maszyn wirtualnych obrazów

##<a name="oracle-database-virtual-machine-images"></a>Obrazy maszyn wirtualnych bazy danych Oracle
### <a name="clustering-rac-is-not-supported"></a>Klastrów (RAC) nie jest obsługiwane.

Azure nie obsługuje obecnie Oracle rzeczywistą aplikacji klastrów (RAC) bazy danych programu Oracle. Możliwe jest tylko autonomicznego wystąpienia bazy danych programu Oracle. Jest tak, ponieważ Azure obecnie nie obsługuje wirtualnych udostępniania dysku w sposób odczytu/zapisu spośród wielu wystąpień maszyn wirtualnych. Również multiemisji UDP nie jest obsługiwane.

### <a name="no-static-internal-ip"></a>Nie statyczny adres IP wewnętrznego

Azure przypisuje każdej wirtualnej maszyny wewnętrzny adres IP. Jeśli maszyna wirtualna nie jest częścią wirtualnej sieci, adres IP maszyny wirtualnej jest dynamiczne i może się zmienić po ponownym uruchomieniu komputera wirtualnych. Może to powodować problemy, ponieważ bazy danych Oracle oczekuje adres IP statycznej. Aby uniknąć tego problemu, Rozważ dodanie maszyny wirtualnej Azure wirtualną siecią. Aby uzyskać więcej informacji, zobacz [Wirtualnej sieci](https://azure.microsoft.com/documentation/services/virtual-network/) i [Tworzenie wirtualnej sieci platformy Azure](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) .

### <a name="attached-disk-configuration-options"></a>Opcje konfiguracji dysku dołączonym

Pliki danych można umieścić na dysku systemu operacyjnego maszyny wirtualnej lub na dyskach dołączone, nazywane też dyski danych. Dołączonych dysków może oferuje lepszą wydajność i rozmiar elastyczność niż dysk systemu operacyjnego. Dysk systemu operacyjnego może być lepiej tylko dla baz danych w obszarze 10 gigabajtów (GB).

Dysków dołączonych korzystają z usługi Magazyn obiektów Blob platformy Azure. Każdy dysk jest w stanie teoretyczna maksymalnie około 500 operacji wejścia i wyjścia na sekundę (operacji i/o na SEKUNDĘ). Wydajność dołączonych dysków może nie być optymalne początkowo i można znacznie zwiększyć wydajność operacji i/o na SEKUNDĘ po upływie "nagrywania w" około 60 90 minut pracy ciągłej. Jeśli później dysku pozostaje bezczynne, wydajności operacji i/o na SEKUNDĘ może zmniejszyć do nagrywania w okresie kolejnych pracy ciągłej. Krótko mówiąc więcej aktywne na dysku, tym bardziej prawdopodobne jest podejście uzyskanie optymalnej wydajności operacji i/o na SEKUNDĘ.

Mimo że najprostsze podejście jest dołączyć jednego dysku maszyn wirtualnych i umieszczenie plików bazy danych na dysku, to podejście jest również najbardziej ograniczanie pod względem wydajności. Zamiast tego możesz razy poprawić efektywnej wydajności operacji i/o na SEKUNDĘ Jeśli przy użyciu wielu dysków dołączone, rozciągnąć ich bazy danych, a następnie użyj programu Oracle automatyczne miejsca do magazynowania zarządzania (ASM). Uzyskać więcej informacji, zobacz [Omówienie magazynu automatyczne Oracle](http://www.oracle.com/technetwork/database/index-100339.html) . Chociaż istnieje możliwość użycia rozkładanie wiele dysków na poziomie systemu operacyjnego, podejście jest zalecane, ponieważ nie wiadomo, aby zwiększyć wydajność operacji i/o na SEKUNDĘ.

Należy rozważyć dwie strategie dołączanie wielu dysków, w zależności od tego, czy ma Wyznaczanie priorytetów wydajności operacji odczytu lub zapisu operacji bazy danych:

- **Oracle ASM samodzielnie** to spowodować lepszą wydajność operacji zapisu, ale co najgorsze operacji i/o na SEKUNDĘ operacji odczytu porównaniu ujęciu za pomocą tablic dysku. Poniższa ilustracja przedstawia logicznie ramach tej Umowy.  
    ![](media/virtual-machines-windows-classic-oracle-considerations/image2.png)

>[AZURE.IMPORTANT] Ocenianie zależność między wydajność zapisu i wydajność odczytu na podstawie każdym przypadku. Rzeczywiste wyniki mogą się różnić, korzystając z tego.

### <a name="high-availability-and-disaster-recovery-considerations"></a>Zagadnienia odzyskiwania wysokiej dostępności i danych

Korzystając z bazą danych Oracle w środowisku maszyn wirtualnych Azure, odpowiadasz stosowania wysoka rozwiązania odzyskiwania dostępności i danych w celu uniknięcia dowolnego przestoje. Możesz również są odpowiedzialne za wykonywanie kopii zapasowej własnych danych i aplikacji.

Wysoka dostępność i awarii odzyskiwania Oracle Database Enterprise Edition (bez RAC) Azure można osiągnąć [zabezpieczenia danych, Active straż danych](http://www.oracle.com/technetwork/articles/oem/dataguardoverview-083155.html)lub [Oracle Golden bramy](http://www.oracle.com/technetwork/middleware/goldengate)za pomocą dwóch baz danych w dwóch oddzielnych maszyn wirtualnych. Oba maszyn wirtualnych powinno być w tej samej [usługi w chmurze](virtual-machines-linux-classic-connect-vms.md) i tej samej [wirtualnej sieci](https://azure.microsoft.com/documentation/services/virtual-network/) upewnij się, że mogą uzyskać dostęp wzajemnie przez prywatny trwałych adres IP.  Ponadto zaleca się, umieszczając maszyn wirtualnych w samej [Ustawianie dostępności](virtual-machines-windows-manage-availability.md) umożliwia Azure, aby umieścić je w osobnych błędów domen i uaktualniania domen. W ten sam zestaw dostępności mogą uczestniczyć tylko maszyn wirtualnych w tym samym usługi w chmurze. Każda maszyna wirtualna musi mieć co najmniej 2 GB pamięci i 5 GB miejsca na dysku.

Z straż danych programu Oracle, szybkiej uzyskuje się z podstawową bazą danych w maszyn wirtualnych, pomocniczego (wstrzymania) bazy danych w innej maszyny wirtualnej i jednokierunkowe replikacji Konfigurowanie między nimi. Wynik to Odczyt kopii bazy danych. Z Oracle GoldenGate można skonfigurować dwukierunkowego replikacji między dwiema bazami danych. Aby dowiedzieć się, jak skonfigurować rozwiązanie wysokiej dostępności baz danych za pomocą tych narzędzi, zobacz dokumentację [Aktywne straż danych](http://www.oracle.com/technetwork/database/features/availability/data-guard-documentation-152848.html) i [GoldenGate](http://docs.oracle.com/goldengate/1212/gg-winux/index.html) w Oracle witryny sieci Web. Jeśli możesz potrzebujesz odczytu i zapisu dostęp do kopii bazy danych, możesz użyć [Aktywnego straż danych programu Oracle](http://www.oracle.com/uk/products/database/options/active-data-guard/overview/index.html).

##<a name="oracle-weblogic-server-virtual-machine-images"></a>Oracle WebLogic Server maszyn wirtualnych obrazów

-  **Klaster jest obsługiwana w wersji Enterprise tylko.** Możesz są licencję WebLogic klaster tylko w przypadku używania wersji Enterprise WebLogic Server. Nie należy używać klaster z WebLogic Server Standard Edition.

-  **Limitów czasu połączeń:** Jeśli aplikacja zależy od połączeń publicznej punkty końcowe innej usłudze w chmurze Azure (na przykład usługa Warstwa bazy danych), Azure może zamknąć te otwarte połączenia po czterech minutach braku aktywności. Ponieważ połączeń, które są aktywne dla więcej niż to ograniczenie nie może być ważność może wpłynąć funkcji i aplikacji Polegaj na Pule połączeń. Jeśli to dotyczy aplikacji, należy rozważyć włączenie "aktywności" logikę do pul połączeń.

    Jeśli punkt końcowy jest *wewnętrznych* do wdrożenia usługi Azure chmurze (na przykład autonomicznego maszyna wirtualna bazy danych w *tym samym* usługi w chmurze jako maszyn wirtualnych WebLogic), następnie połączenie jest bezpośrednia i nie korzysta równoważenia obciążenia Azure i limit czasu połączenia w związku z tym nie podlega.

-  **Multiemisja UDP nie jest obsługiwana.** Azure obsługuje UDP emisją pojedynczą, ale nie multiemisji lub emisji. Serwer WebLogic jest możliwość korzystania z funkcji emisji pojedynczej Azure UDP. Dla najlepiej wyniki Polegaj na emisji pojedynczej UDP, zaleca się że rozmiar klaster WebLogic zostaną zachowane statyczne lub zostaną zachowane z nie więcej niż 10 serwerów zarządzanych znajdujących się w grupie.

-  **Serwer WebLogic oczekuje publicznych i prywatnych porty te same dostępu T3 (na przykład podczas korzystania z Enterprise JavaBeans).** Rozważmy scenariusz wielu, w którym aplikacją usługi warstwy (EJB) jest uruchomiona w klastrze serwerów WebLogic składający się z dwóch lub więcej serwery zarządzane, w usłudze w chmurze o nazwie **SLWLS**. Warstwa klienta znajduje się w usługi w chmurze różnych, uruchamianie prostego programu Java próbuje nawiązać połączenie EJB warstwy usług. Ponieważ jest to konieczne do warstwy usługi równoważenia obciążenia, publicznej końcowy równoważenia obciążenia musi być utworzony dla maszyn wirtualnych w klastrze serwerów WebLogic. Jeśli port prywatne, który można określić dla tego punktu końcowego różni się od publicznej port (na przykład 7006:7008), wystąpi błąd następujące:

        [java] javax.naming.CommunicationException [Root exception is java.net.ConnectException: t3://example.cloudapp.net:7006:

        Bootstrap to: example.cloudapp.net/138.91.142.178:7006' over: 't3' got an error or timed out]

    Jest to spowodowane dla dowolnego dostępu zdalnego T3 serwera WebLogic oczekuje portu równoważenia obciążenia i port serwera WebLogic zarządzane być taka sama. W powyższym przypadku klient uzyskuje dostęp do portu 7006 (port usługi równoważenia obciążenia) i serwer zarządzane nasłuchują na 7008 (port prywatnych). To ograniczenie ma zastosowanie tylko do dostępu T3, nie HTTP.

    Aby uniknąć tego problemu, użyj jednej z poniższych rozwiązań:

    -  Za pomocą samego numeru portu prywatne i publiczne dla punktów końcowych równoważenia obciążenia przeznaczonych do programu access T3.

    -  Podczas uruchamiania serwera WebLogic, należy dodać parametr maszyny wirtualnej Java następujące czynności:

            -Dweblogic.rjvm.enableprotocolswitch=true

Do powiązanych informacji zobacz artykuł KB **860340.1** na <http://support.oracle.com>.

-  **Dynamiczne klaster i równoważenia obciążenia ograniczenia.** Załóżmy, że chcesz użyć dynamicznego klaster w WebLogic serwera i udostępnić go za pośrednictwem jednym, publicznej równoważenia obciążenia punktu końcowego platformy Azure. Można to zrobić pod warunkiem za pomocą numeru portu stały dla każdego z zarządzanych serwerów (nie dynamiczne przypisanych z zakresu) i nie zaczynają się bardziej zarządzanych serwerów niż maszyny administrator są śledzone (oznacza to, że nie więcej niż jednego zarządzanych na maszyn wirtualnych serwera). Jeśli konfiguracji wynikiem większej liczby serwerów WebLogic uruchamiany niż maszyn wirtualnych (oznacza to, że miejsce, w którym wielu wystąpień serwera WebLogic udział w tym samym komputerze wirtualnych), a następnie nie jest możliwe dla więcej niż jednej z tych wystąpień WebLogic serwerów powiązać z danego numeru portu — innych osób na tym komputerze wirtualnych nie powiedzie się.

    Z drugiej strony Jeśli skonfigurujesz administracyjnego serwera automatycznie przypisuje unikatowe numery portów do jego serwery zarządzane, następnie równoważenia obciążenia jest niemożliwe, ponieważ Azure nie obsługuje mapowania z jednego portu publicznej do wielu portów prywatne, jak jest wymagana dla tej konfiguracji.

-  **Wiele wystąpień serwera Weblogic na komputerze wirtualnych.** W zależności od potrzeb z wdrożeniem należy rozważyć opcji uruchamiania wielu wystąpień serwera WebLogic na tym samym komputerze wirtualnych, jeśli maszyny wirtualnej jest dostateczna. Na przykład na komputerze wirtualnych średnim rozmiarze, który zawiera dwa rdzenie, można wybrać do uruchamiania wystąpień dwóch WebLogic serwera. Jednak należy pamiętać, że nadal zaleca się uniknąć wprowadzenie pojedynczych punktów awarii w architekturze może być użycie po prostu maszyn wirtualnych działa wiele wystąpień serwera WebLogic. Przy użyciu co najmniej dwóch maszyn wirtualnych może być lepszym rozwiązaniem, a następnie każdego z tych maszyn wirtualnych uruchomić wielu wystąpień serwera WebLogic. Każdy z tych wystąpień serwera WebLogic może być nadal część tym samym klastrze. Notatkę, jednak jest obecnie nie można używać Azure do punktów końcowych równoważenia obciążenia, które są dostępne w takich wdrożeń WebLogic serwera w tym samym komputerze wirtualnych, ponieważ równoważenia obciążenia Azure wymaga równoważenia obciążenia serwerów ma być rozdzielona między unikatowe maszyn wirtualnych.

##<a name="oracle-jdk-virtual-machine-images"></a>Oracle JDK maszyn wirtualnych obrazów

-  **JDK 6 i 7 najnowszych aktualizacji.** Gdy jest zalecane przy użyciu najnowszych publicznej, obsługiwanych wersji języka Java (obecnie Java 8) Azure również udostępnia JDK 6 i 7 obrazy. To jest przeznaczona dla starszych aplikacji, które nie są jeszcze gotowy do uaktualnienia JDK 8. Podczas aktualizacji do poprzedniego obrazów JDK może nie być już dostępna publicznie podane partnerstwa firmy Microsoft z bazą danych Oracle, JDK 6 i 7 obrazy dostarczony przez Azure mają zawierać najnowszą aktualizację innych niż publicznej, zwykle oferowanego przez Oracle tylko dla grupy wybranych klientów obsługiwanych programu Oracle. Nowe wersje obrazów JDK będą dostępne w zależności od zaktualizowanej wersji JDK 6 i 7.

    JDK dostępne w tym JDK 6 i 7 obrazy i maszyn wirtualnych i obrazy pochodząca z nich, można używać tylko w Azure.

-  **JDK 64-bitowej.** Obrazy maszyn wirtualnych programu Oracle WebLogic Server i obrazy maszyn wirtualnych Oracle JDK dostarczony przez Azure zawierają 64-bitowych wersjach systemu Windows Server i JDK.

##<a name="additional-resources"></a>Dodatkowe zasoby
[Obrazy maszyn wirtualnych Oracle dla Azure](virtual-machines-linux-classic-oracle-images.md)
