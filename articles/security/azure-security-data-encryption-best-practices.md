<properties
   pageTitle="Najważniejsze wskazówki dotyczące zabezpieczania danych i szyfrowania | Microsoft Azure"
   description="Ten artykuł zawiera zestaw najważniejsze wskazówki dotyczące zabezpieczania danych i szyfrowania przy użyciu wbudowana Azure możliwości."
   services="security"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/16/2016"
   ms.author="yuridio"/>

#<a name="azure-data-security-and-encryption-best-practices"></a>Najlepsze praktyki dotyczące zabezpieczania danych Azure i szyfrowania

Jeden z klawiszy do ochrony danych w chmurze uwzględniania stanów, w którym mogą wystąpić danych i jakie mechanizmy są dostępne dla tego Państwa. Azure danych w celu zabezpieczenia i szyfrowanie najważniejsze wskazówki zalecenia będzie wokół stany poniższe dane:

- W pozostałych: Ta opcja uwzględnia wszystkie informacje, które obiekty miejsca do magazynowania, kontenerów i typy znajdujące się statycznie na nośniku fizycznym, można go magnetyczne lub dysku optycznego.

- W drodze: Gdy dane są przesyłane między składnikami, lokalizacji lub programów, takich jak w sieci, przez usługę bus (z lokalnego do chmury i odwrotnie, łącznie z połączeniami hybrydowe, takich jak ExpressRoute) lub w procesie wejścia i wyjścia go jest traktować jako w ruchu.

W tym artykule omówimy zbiór danych Azure zabezpieczeń i szyfrowania najlepsze rozwiązania. Poniższe najważniejsze wskazówki pochodzą z naszych doświadczeń z zabezpieczeniami Azure danych i szyfrowania i jej klientów, takich jak samodzielnie.

Dla każdego najlepiej będzie możemy wyjaśnić:

- Co to jest najbardziej skuteczne rozwiązanie
- Dlaczego użytkownik ma mieć możliwość tego najważniejsze wskazówki
- Jeśli nie można włączyć najlepiej, co może być skutkiem
- Możliwe rozwiązania alternatywne wobec najlepiej
- Jak możesz dowiedzieć się włączyć najlepiej

W tym artykule Azure zabezpieczania danych oraz najlepsze rozwiązania szyfrowania jest oparty na opinii zgody i możliwości platformy Azure i zestawy funkcji, jakie istnieją w czasie, który został zapisany w tym artykule. Opinie i technologie zmian w czasie i regularnie, aby uwzględnić zmiany zostaną zaktualizowane w tym artykule.

Azure danych zabezpieczeń i szyfrowania najważniejsze wskazówki omawiane w tym artykule obejmują:

- Wymusić uwierzytelnianie wieloskładnikowe
- Kontrola dostępu (RBAC) oparta na rolach użycia
- Szyfrowanie Azure maszyn wirtualnych
- Używanie modelami zabezpieczeń sprzętu
- Zarządzanie z bezpiecznego stanowiska pracy
- Włączanie szyfrowania danych SQL
- Ochrona danych podczas przesyłania
- Wymuszanie szyfrowania danych poziomu plików


## <a name="enforce-multi-factor-authentication"></a>Wymusić uwierzytelnianie wieloskładnikowe

Pierwszym krokiem dostęp do danych i kontroli platformy Microsoft Azure jest do uwierzytelnienia użytkownika. [Uwierzytelnianie wieloskładnikowe Azure (MFA)](../multi-factor-authentication/multi-factor-authentication.md) jest metodą zweryfikować tożsamość użytkownika przy użyciu innej metody niż tylko nazwy użytkownika i hasła. Ten uwierzytelniania metody ułatwia ochronę informacji dostęp do danych i aplikacje podczas spotkania żądanie użytkownika prosty proces logowania.

Po włączeniu Azure MFA dla użytkowników, dodajesz drugiej warstwy zabezpieczeń dodatki logowania użytkownika i transakcje. W tym przypadku transakcji mogą uzyskiwać dostęp do dokumentu znajdującego się na serwerze plików lub usługi SharePoint Online. Pozwala Azure MFA IT w celu zmniejszenia prawdopodobieństwa, że poświadczenie złamany uzyskuje dostęp do danych organizacji.

Na przykład: Jeśli Wymuszaj Azure MFA dla użytkowników i skonfigurować go do używania rozmowy telefonicznej lub wiadomość jako weryfikacji, jeśli poświadczenia użytkownika jest bezpieczne, tej nie będą mogli uzyskać dostęp do wszystkich zasobów, ponieważ on nie będą mieć dostępu do telefonu użytkownika. Organizacji, które nie dodawaj tego dodatkową warstwę ochrony tożsamości są bardziej podatne na ataki Kradzieże poświadczeń, które mogą prowadzić do ujawnienia danych.

Jeden alternatywę dla organizacji, które chcesz zachować uwierzytelniania sterowania lokalnego jest użycie [Serwera uwierzytelniania wieloskładnikowego Azure](../multi-factor-authentication/multi-factor-authentication-get-started-server.md), nazywany MFA lokalnego. Przy użyciu tej metody nadal będzie mógł wymusić uwierzytelnianie wieloskładnikowe, zachowując MFA lokalnego serwera.

Aby uzyskać więcej informacji o Azure MFA przeczytaj artykuł [Wprowadzenie do uwierzytelnianie wieloskładnikowe Azure w chmurze](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md).

## <a name="use-role-based-access-control-rbac"></a>Kontrola dostępu (RBAC) oparta na rolach użycia
Ograniczenia dostępu w oparciu o zasady zabezpieczeń [wiedzieć](https://en.wikipedia.org/wiki/Need_to_know) i [jak najmniejszych uprawnień](https://en.wikipedia.org/wiki/Principle_of_least_privilege) . Jest to konieczne, w przypadku organizacji, które mają być wymuszania zasad zabezpieczeń, aby uzyskać dostęp do danych. Azure oparta na rolach programu Access Control (RBAC) może służyć do przypisywania uprawnień do użytkowników, grupy i wniosków określonego zakresu. Zakres przypisania roli może być subskrypcji, grupa zasobów lub pojedynczego zasobu.

Mogą korzystać z [wbudowanych RBAC ról](../active-directory/role-based-access-built-in-roles.md) w Azure, aby przypisać uprawnienia do użytkowników. Warto rozważyć użycie *Trybu współautora konta miejsca do magazynowania* dla chmury operatory, których potrzebujesz do zarządzania kontami miejsca do magazynowania i rola *Współautora konta w usłudze klasyczny miejsca do magazynowania* do zarządzania kontami klasyczny miejsca do magazynowania. Dla chmury operatory, których potrzebuje do zarządzania maszyny wirtualne i konta miejsca do magazynowania Rozważ dodanie ich do roli *Współautora maszyn wirtualnych* .

Organizacji, które nie Wymuszaj kontrola dostępu do danych dzięki zastosowaniu funkcje, takie jak RBAC może być podanie uprawnienia więcej niż jest to niezbędne dla swoich użytkowników. To może prowadzić do ujawnienia danych przez niektórych użytkowników mających dostęp do danych, które nie powinny być w pierwszej kolejności.

Więcej o Azure RBAC można znaleźć, czytając artykuł [Azure Role-Based kontrola dostępu](../active-directory/role-based-access-control-configure.md).

## <a name="encrypt-azure-virtual-machines"></a>Szyfrowanie Azure maszyn wirtualnych
W przypadku wielu organizacji [szyfrowanie danych spoczynku](https://blogs.microsoft.com/cybertrust/2015/09/10/cloud-security-controls-series-encrypting-data-at-rest/) jest obowiązkowe krok w kierunku prywatność danych, zgodności i suwerenności danych. Szyfrowanie dysków Azure umożliwia administratorom szyfrowanie dysków systemu Windows i Linux oraz IaaS maszyn wirtualnych (VM). Azure szyfrowania dysku wykorzystuje branżowe standardowa funkcja BitLocker systemu Windows i funkcji kryptograficznego DM Linux oraz zapewnienie szyfrowania woluminu systemu operacyjnego i dyski danych.

Mogą korzystać z szyfrowania dysku Azure, aby chronić i ochrony danych w celu spełnienia organizacji bezpieczeństwa i zgodności z przepisami. Organizacje też rozważyć użycie szyfrowania ułatwiające ograniczanie dostępu do danych związanych z nieautoryzowanego ryzyka. Zalecane jest również szyfrowania dysków przed zapisywania ważnych danych.

Upewnij się, do szyfrowania ilości danych z maszyn wirtualnych i głośność uruchamiania w celu ochrony danych w stanie spoczynku na koncie Azure miejsca do magazynowania. Zabezpieczenia kluczy szyfrowania i hasła, wykorzystując [Azure klucza magazynu](../key-vault/key-vault-whatis.md).

Aby lokalne serwery systemu Windows należy rozważyć następujące szyfrowania najlepsze rozwiązania:

- Za pomocą [funkcji BitLocker](https://technet.microsoft.com/library/dn306081.aspx) do szyfrowania danych
- Przechowywanie informacji odzyskiwania w usługach AD DS.
- W przypadku jakiekolwiek wątpliwości, że zostały złamane kluczy funkcji BitLocker, zaleca się sformatowanie dysku, aby usunąć wszystkie wystąpienia metadanych funkcji BitLocker z dysku lub odszyfrować, a następnie ponownie szyfrować cały dysk.

Organizacji, które nie wymuszanie szyfrowania danych będą chętniej udostępnić problemów integralności danych, takich jak złośliwy lub rogue użytkowników kradzież danych i zostało naruszone kontami uzyskania nieautoryzowanego dostępu do danych w Wyczyść formatowanie. Oprócz tego ryzyka firmy, które muszą być zgodne z przepisami branżowe musi udowodnić sumiennym i ich za pomocą kontrolek poprawnych zabezpieczeń Aby zwiększyć bezpieczeństwo danych.

Czy więcej informacji o szyfrowanie dysków Azure, czytając artykuł [Azure dysku szyfrowania dla systemu Windows i Linux oraz IaaS w maszyny wirtualne](azure-security-disk-encryption.md).

## <a name="use-hardware-security-modules"></a>Używanie moduły sprzętu

Rozwiązania zgodne ze standardami szyfrowania klawisze poufne dane są szyfrowane. Dlatego niezwykle ważne jest bezpieczne przechowywanie tych klawiszy. Zarządzania kluczami staje się integralną częścią ochrony danych, ponieważ będzie można użyć do przechowywania klucze tajne, które są używane do szyfrowania danych.

Szyfrowanie dysków Azure używa [Magazynu klucza Azure](https://azure.microsoft.com/services/key-vault/) ułatwiające kontrolowanie i zarządzanie nimi kluczy szyfrowania dysku i hasła w ramach subskrypcji klucza magazynu, zapewniając, że wszystkie dane w dyski maszyn wirtualnych są szyfrowane na pozostałych w magazynie Azure. Za pomocą magazynu klucza Azure należy inspekcji kluczy i użycia zasad.

Istnieje wiele związane z ryzyko związane z nie ma odpowiednią kontrolę zabezpieczeń w celu ochrony klucze tajne, które były używane do szyfrowania danych. Ataki ma dostęp do klucze tajne, będą mogli odszyfrowywanie danych i potencjalnie mieć dostęp do informacji poufnych.

Więcej informacji o ogólne zalecenia dotyczące zarządzania certyfikat platformy Azure, czytając artykuł [Zarządzanie certyfikatem platformy Azure: porady](https://blogs.msdn.microsoft.com/azuresecurity/2015/07/13/certificate-management-in-azure-dos-and-donts/).

Aby uzyskać więcej informacji na temat magazynu klucza Azure przeczytaj [rozpocząć pracę z magazynu klucza Azure](../key-vault/key-vault-get-started.md).

## <a name="manage-with-secure-workstations"></a>Zarządzanie z bezpiecznego stanowiska pracy

Ponieważ większość atakach skierować użytkownika końcowego, staje się punkt końcowy jeden z punktów podstawowego atakiem. Jeśli osoba atakująca intensywność pogarsza punkt końcowy, on wykorzystać poświadczenia użytkownika, aby uzyskać dostęp do danych organizacji. Większość atakami punktu końcowego będą mogli korzystać z faktu, że użytkownicy końcowi są administratorów w swoje lokalne stanowiska pracy.

Za pomocą roboczej bezpiecznego zarządzania można zmniejszyć tego ryzyka. Firma Microsoft zaleca używanie [Uprzywilejowanych dostępu stanowiska pracy (ŁAPY)](https://technet.microsoft.com/library/mt634654.aspx) w celu zmniejszenia powierzchni atakiem w stanowiska pracy. Te stanowiska pracy bezpiecznego zarządzania może pomóc w zmniejszeniu niektóre z nich atakami zapewnić danych jest bezpieczniejsze. Upewnij się, aby zabezpieczyć i zablokować pracy za pomocą ŁAPY. To jest ważny krok o podanie gwarancje wysokim poziomie zabezpieczeń w przypadku kont poufnych, zadań i ochrony danych.

Brak ochrona punktu końcowego mogą zagrożenie danych, upewnij się, że wymuszania zasad zabezpieczeń na wszystkich urządzeniach, które są używane do korzystania z danych, niezależnie od lokalizacji danych (chmury lub lokalnego).

Możesz dowiedzieć się więcej o uprawnieniach dostępu roboczej, czytając artykuł [Dotyczący zabezpieczania uprzywilejowanych dostępu](https://technet.microsoft.com/library/mt631194.aspx).

## <a name="enable-sql-data-encryption"></a>Włączanie szyfrowania danych SQL

[Szyfrowanie przezroczysty danych bazy danych SQL Azure](https://msdn.microsoft.com/library/dn948096.aspx) (TDE) ułatwia ochronę przed zagrożenia złośliwych działań, wykonując w czasie rzeczywistym szyfrowanie i odszyfrowywanie bazy danych, skojarzony kopie zapasowe oraz pliki dziennika transakcji w stanie spoczynku bez wprowadzania zmian w aplikacji.  TDE są szyfrowane na przechowywanie całej bazy danych przy użyciu klucza symetrycznej o nazwie klucza szyfrowania bazy danych.

Nawet w przypadku, gdy są szyfrowane całej przestrzeni dyskowej, bardzo ważne jest również szyfrowanie bazy danych sam. To jest implementacją obrony w głębi podejście do ochrony danych. Jeśli używasz [Bazy danych SQL Azure](https://msdn.microsoft.com/library/0bf7e8ff-1416-4923-9c4c-49341e208c62.aspx) i chcesz chronić poufne dane, takie jak karty kredytowej lub numer PESEL, można zaszyfrować baz danych z FIPS 140-2 sprawdzana poprawność 256 szyfrowania bitowego AES spełniający wymagania wiele standardami (na przykład HIPAA, PCI).

Aby dowiedzieć się, że plików związanych z [rozszerzenia puli buforu](https://msdn.microsoft.com/library/dn133176.aspx) (BPE) nie są szyfrowane podczas bazy danych jest zaszyfrowany przy użyciu TDE ważne jest. Należy użyć narzędzia szyfrowanie na poziomie systemu plików, takie jak BitLocker lub pliki związane z [Systemu szyfrowania plików](https://technet.microsoft.com/library/cc700811.aspx) szyfrowania dla BPE.

Od uprawnień takie jak uprawnienia administratora zabezpieczeń lub administratorem bazy danych mają dostęp do danych, nawet jeśli baza danych jest zaszyfrowany przy użyciu TDE, należy również wykonać zalecenia poniżej:

- Uwierzytelnianie programu SQL na poziomie bazy danych
- Azure uwierzytelnianie AD przy użyciu RBAC ról
- Użytkownicy i aplikacje należy używać osobne konta do uwierzytelnienia. W ten sposób można ograniczyć uprawnienia dla użytkowników i aplikacji i zmniejszyć ryzyko złośliwych działań.
- Implementowanie zabezpieczeń na poziomie bazy danych przy użyciu ról stałej bazy danych (na przykład db_datareader lub db_datawriter), lub można tworzyć niestandardowe role aplikacji do udzielania jawne uprawnienia do zaznaczonych obiektach bazy danych

Organizacji, które nie używają poziomu szyfrowanie bazy danych mogą być bardziej podatne na ataki, które mogą złamanie dane znajdujące się w bazach danych programu SQL.

Przedstawiono więcej informacji na temat szyfrowania SQL TDE, czytając artykuł [Przezroczyste szyfrowanie danych z bazy danych SQL Azure](https://msdn.microsoft.com/library/0bf7e8ff-1416-4923-9c4c-49341e208c62.aspx).

## <a name="protect-data-in-transit"></a>Ochrona danych podczas przesyłania

Ochrona danych podczas przesyłania powinny być podstawowe części strategii ochrony danych. Ponieważ dane zostaną przenoszenia i z powrotem w wielu lokalizacjach, ogólne zalecenia jest zawsze używaj protokołów SSL/TLS do wymiany danych w różnych lokalizacjach. W niektórych przypadkach może być celu wyodrębnienia kanału całej komunikacji między lokalnym a chmurą infrastruktury przy użyciu wirtualną sieć prywatną (VPN).

Przechodzenie między infrastruktury lokalnego i Azure danych należy uwzględnić odpowiednie zabezpieczenia, takie jak HTTPS lub sieci VPN.

W przypadku organizacji, które należy bezpieczny dostęp do wielu stacji roboczych znajduje lokalnego Azure, użyj [Azure VPN witryny do witryny](../vpn-gateway/vpn-gateway-site-to-site-create.md).

W przypadku organizacji, które trzeba bezpieczny dostęp z jednej stacji roboczej znajduje się w lokalnej do Azure, użycie [punkt do witryny sieci VPN](../vpn-gateway/vpn-gateway-point-to-site-create.md).

Dane większe zestawy można przenosić w dedykowanej WAN szybkie łącze na przykład [ExpressRoute](https://azure.microsoft.com/services/expressroute/). Jeśli zdecydujesz się na użycie ExpressRoute, można również zaszyfrować dane na poziomie aplikacji przy użyciu [Protokołu SSL/TLS](https://support.microsoft.com/kb/257591) lub inne protokoły dla dodane ochrony.

Jeśli użytkownik współpracuje z magazynu Azure za pośrednictwem portalu Azure, wszystkie transakcje wystąpić przy użyciu protokołu HTTPS. Można również [Interfejsu API usługi REST miejsca do magazynowania](https://msdn.microsoft.com/library/azure/dd179355.aspx) za pomocą protokołu HTTPS do współdziałania z [Bazy danych SQL Azure](https://azure.microsoft.com/services/sql-database/)i [Magazynowania Azure](https://azure.microsoft.com/services/storage/) .

Organizacje, których nie powiodło się do ochrony danych w drodze są bardziej podatne [na pośrednik atakami](https://technet.microsoft.com/library/gg195821.aspx), [podsłuchiwanie](https://technet.microsoft.com/library/gg195641.aspx) i przejęcie kontroli sesji. Ataki te mogą być pierwszy krok w uzyskaniu dostępu do danych poufnych.

Więcej informacji na temat opcji Azure VPN można znaleźć, czytając artykuł [Planowanie i projektowanie dla bramy sieci VPN](../vpn-gateway/vpn-gateway-plan-design.md).

## <a name="enforce-file-level-data-encryption"></a>Wymuszanie szyfrowania danych poziomu plików

Dodatkowej warstwy zabezpieczeń można zwiększyć poziom zabezpieczeń dla określonego typu danych jest szyfrowanie pliku, niezależnie od lokalizacji pliku.

[Azure RMS](https://technet.microsoft.com/library/jj585026.aspx) używa szyfrowania, tożsamości i autoryzacji do zabezpieczania plików i wiadomości e-mail. Azure RMS działa na wielu urządzeniach — telefony, tabletów i komputerów chroniąc w obrębie organizacji i spoza organizacji. Ta funkcja jest możliwe, ponieważ usługą Azure RMS dodaje poziom ochrony, która pozostaje z danymi, nawet wtedy, gdy pozostawia ograniczenia Twojej organizacji.

Korzystając z usługą Azure RMS ochrony plików, używają standardowych kryptograficzny z pełną obsługą [FIPS 140-2](http://csrc.nist.gov/groups/STM/cmvp/standards.html). Kiedy korzystać z usługą Azure RMS ochrony danych, musisz gwarancji, że ochrony pozostaje z plikiem, nawet jeśli jest kopiowana do magazynowania, który nie znajduje się pod kontrolę nad IT, takich jak usługi magazynu w chmurze. Taki sam występuje pliki udostępnione za pośrednictwem poczty e-mail, plik jest chroniony jako załącznik do wiadomości e-mail z instrukcjami, jak otworzyć załącznik chroniony.

Podczas planowania ułatwiające rozpoczęcie pracy z usługą Azure RMS zaleca się następujące czynności:

- Instalowanie [usługi RMS udostępniania aplikacji](https://technet.microsoft.com/library/dn339006.aspx). Ta aplikacja można zintegrować z pakietem Office aplikacji przez zainstalowanie pakietu Office dodatek tak, aby użytkownicy łatwo można chronić pliki bezpośrednio.
- Konfigurowanie aplikacji i usług pomocy technicznej usługi Azure RMS
- Tworzenie [szablonów niestandardowych](https://technet.microsoft.com/library/dn642472.aspx) odzwierciedlająca potrzeb biznesowych. Na przykład: szablonu do góry poufne dane, które powinny być stosowane w wszystkich ściśle tajne dotyczące wiadomości e-mail.

Organizacje, które są słabych na ochronę [klasyfikacji danych](http://download.microsoft.com/download/0/A/3/0A3BE969-85C5-4DD2-83B6-366AA71D1FE3/Data-Classification-for-Cloud-Readiness.pdf) i plik może być bardziej podatne na wycieku danych. Bez ochrony plików pisane z wielkiej litery organizacji nie będą mogli uzyskać więcej informacji biznesowych, monitorować abuse i wyeliminować złośliwy dostępu do plików.

Więcej informacji o usłudze Azure RMS można znaleźć, czytając artykuł [Wprowadzenie do usługi Azure Rights Management](https://technet.microsoft.com/library/jj585016.aspx).
