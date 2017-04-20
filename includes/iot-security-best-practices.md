# <a name="internet-of-things-security-best-practices"></a>Najważniejsze wskazówki dotyczące zabezpieczeń Internet rzeczy

Aby zabezpieczyć Internet rzeczy (IoT) infrastruktury wymaga rygorystyczne strategii zabezpieczeń w głębi. Ta strategia wymaga zabezpieczania danych w chmurze, ochrony integralności danych w trakcie tranzytu publicznie w Internecie i bezpieczne wprowadzenia urządzeń. Każda warstwa opiera się gwarancję bezpieczeństwa w całej infrastruktury.

## <a name="secure-an-iot-infrastructure"></a>Zabezpieczenia infrastruktury IoT

Może być opracowany i wykonany przy użyciu aktywne uczestnictwo różnych podmiotów z wytwarzania, rozwoju i wdrażania infrastruktury i urządzeń IoT tej strategii zabezpieczeń w głębi. Poniżej znajduje się opis wysokiego poziomu dotyczący tych podmiotów.  

- **Producent sprzętu IoT/integrator**: zazwyczaj są to producenci sprzętu IoT w wdrażanego integratorów montaż sprzętu od różnych producentów lub dostawcami sprzętu dla wdrożenia IoT produkowanych lub zintegrowane przez innych dostawców.
- **Deweloperów rozwiązania IoT**: opracowania rozwiązania IoT odbywa się zwykle przez deweloperów rozwiązania. Część może tego dewelopera własny zespół lub integrator systemów (SI) specjalizujących się w tej działalności. Deweloperów rozwiązania IoT można rozwijać różne składniki rozwiązania IoT od podstaw, integracji różnych składników gotowych lub typu open source lub przyjąć wstępnie skonfigurowanych rozwiązań drobnych modyfikacjach.
- **Narzędzie wdrażania rozwiązania IoT**: opracowany po IoT rozwiązanie, należy go wdrożyć w polu. Ten proces obejmuje wdrażania sprzętu, wzajemne połączenia urządzeń i wdrażania rozwiązań w polu urządzenia sprzętowe lub chmury.
- **Operator rozwiązania IoT**: roztwór po IoT jest rozmieszczana, wymaga długoterminowego monitorowania, aktualizacje i obsługę. Można to zrobić przez własny zespół, który składa się z informacji specjaliści, operacjami sprzętowymi i zespoły konserwacyjne i domeny specjalistów, którzy monitorują poprawne zachowanie całej infrastruktury IoT.

Sekcje zawierają wskazówki dla każdego z tych graczy, aby pomóc opracowania, wdrażania i funkcjonowania bezpiecznej infrastruktury IoT.

## <a name="iot-hardware-manufacturerintegrator"></a>Producent sprzętu IoT/integrator

Oto najważniejsze wskazówki dla producentów sprzętu IoT i integratorów sprzętu.

- **Zakres sprzętu do minimalnych wymagań**: projekt sprzętu powinny zawierać minimalne funkcje wymagane dla funkcjonowania sprzętu i nic więcej. Przykładem jest uwzględnienie porty USB tylko wtedy, gdy jest to niezbędne do działania urządzenia. Te dodatkowe funkcje otworzyć urządzenia dla niechcianych ataków, których należy unikać.
- **Że sprzęt gwarancyjnie**: budowanie mechanizmy w celu wykrywania fałszowania fizyczne, takie jak otwarcie pokrywy urządzenie lub część urządzenia. Modyfikować te sygnały mogą stanowić część strumienia danych przekazany do chmury, który mógłby zaalarmować operatory te zdarzenia.
- **Budowanie wokół bezpiecznego sprzętu**: KWS jeśli zezwala na, budowanie funkcje zabezpieczeń, takie jak bezpieczne i szyfrowane pamięci masowej lub uruchomienia funkcji oparte na moduł zaufanej platformy (TPM). Te cechy sprawiają urządzeń zwiększyć bezpieczeństwo i chronić całej infrastruktury IoT.
- **Aktualizacje zabezpieczeń**: aktualizacje oprogramowania układowego w czasie użytkowania urządzenia są nieuniknione. Budowanie urządzeń z bezpiecznej ścieżki uaktualnień i kryptograficzne zapewnianie wersje oprogramowania sprzętowego pozwoli urządzenie będzie bezpieczne podczas i po zakończeniu uaktualnienia.

## <a name="iot-solution-developer"></a>Deweloperów rozwiązania IoT

Poniżej przedstawiono najważniejsze wskazówki dla deweloperów rozwiązań IoT:

- **Wykonaj bezpieczne metodologii pracy**: rozwój bezpiecznego oprogramowania wymaga podstaw myślenia o bezpieczeństwie, od momentu rozpoczęcia projektu aż do jego wdrożenia, testowania i wdrażania. Wybory platform, języków i narzędzi są zależne w tej metodologii. Cykl projektowania zabezpieczeń Microsoft krok po kroku do tworzenia bezpiecznego oprogramowania.
- **Oprogramowanie open source wybierz ostrożnie**: oprogramowanie Open source daje możliwość szybkiego tworzenia rozwiązania. Przy wyborze oprogramowania typu open source, należy wziąć pod uwagę poziom działalności Wspólnoty dla każdego składnika typu open source. Active Wspólnoty zapewnia, czy oprogramowanie jest obsługiwany i że problemy są wykryte i kierowane. Alternatywnie mogą nie być obsługiwane niejasne i nieaktywnych oprogramowania typu open source i problemów prawdopodobnie nie zostaną odnalezione.
- **Integrowanie z ostrożnością**: istnieje wiele luk w zabezpieczeniach oprogramowania na granicy bibliotek i interfejsów API. Funkcje, które nie mogą być wymagane dla bieżącego wdrażania nadal może być dostępny za pośrednictwem z warstwy interfejsu API. Aby zapewnić całkowite bezpieczeństwo, upewnij się sprawdzić wszystkie interfejsy składników integracji dla luki w zabezpieczeniach.      

## <a name="iot-solution-deployer"></a>Narzędzie wdrażania rozwiązania IoT

Poniżej przedstawiono najważniejsze wskazówki dotyczą usługodawców, IoT rozwiązanie:

- **Bezpieczny sposób wdrażania sprzętu**: wdrożeń IoT może wymagać sprzętu można wdrożyć w niezabezpieczonych lokalizacjach, takich jak przestrzenie publiczne lub dopuszczać ustawień regionalnych. W takich sytuacjach, upewnij się, że wdrażania sprzętu jest odporne na manipulacje w maksymalnym zakresie. Jeśli są dostępne za pomocą sprzętu USB lub innych portów, upewnij się, że są one objęte bezpiecznie. Wiele ataków służą jako punkty wejścia.
- **Bezpieczeństwo kluczy uwierzytelniania**: podczas wdrażania, każde urządzenie wymaga identyfikatory urządzeń i skojarzone klucze generowane przez usługę w chmurze. Bezpieczeństwo tych kluczy fizycznie nawet po jego wdrożeniu. Każdy złamany klucz może służyć za pomocą urządzenia złośliwego podszyć się pod istniejące urządzenie.

## <a name="iot-solution-operator"></a>Operator rozwiązania IoT

Poniżej przedstawiono najważniejsze wskazówki dotyczące operatorów rozwiązania IoT:

- **Regularne aktualizowanie systemu**: zapewnienia, że systemy operacyjne i wszystkie sterowniki urządzeń są uaktualniane do najnowszej wersji. Po włączeniu funkcji Aktualizacje automatyczne w systemie Windows 10 (IoT lub inne wersje) Microsoft utrzymuje go na bieżąco, zapewniając bezpieczny system operacyjny dla urządzeń IoT. Utrzymanie innych systemów operacyjnych (takich jak Linux) na bieżąco pomaga zapewnić również są chronione przed złośliwymi atakami.
- **Ochrona przed szkodliwą działalność**: Jeśli pozwala na to system operacyjny, należy zainstalować najnowsze funkcje antywirusowe i złośliwym oprogramowaniem w każdym systemie operacyjnym urządzenia. Może to pomóc w osłabianiu najbardziej zewnętrzne zagrożenia. Większość nowoczesnych systemów operacyjnych przed zagrożeniami można chronić przez podjęcie odpowiednich kroków.
- **Często inspekcja**: inspekcja IoT infrastruktury dla problemów związanych z zabezpieczeniami jest kluczem reagowaniu na zagrożenia bezpieczeństwa. Większość systemów operacyjnych zapewnia rejestrowanie zdarzeń wbudowanych, które powinno się poddać często, aby upewnić się, że nie nastąpiło żadne naruszenia zabezpieczeń. Informacje o inspekcji mogą być wysyłane jako strumień oddzielnych telemetrycznego do usługa w chmurze gdzie mogą być analizowane.
- **Ochronę fizyczną infrastruktury IoT**: najgorszym ataków na zabezpieczenia infrastruktury IoT są uruchamiane przy użyciu fizycznego dostępu do urządzenia. Jedna z praktyk ważne dla bezpieczeństwa jest ochronę przed złośliwym korzystaniem z portów USB i inne fizycznego dostępu. Jeden klucz do wykrywania naruszeń, które mogą mieć miejsce jest rejestrowanie fizycznego dostępu, takie jak używanie portu USB. Ponownie Windows 10 (IoT i inne wersje) włącza szczegółowe rejestrowanie tych zdarzeń.
- **Chroń chmurze poświadczenia**: chmura poświadczeń uwierzytelniania używany do konfigurowania i obsługi wdrożenia IoT ewentualnie są najprostszym sposobem uzyskania dostępu i złamanie IoT. Chronić poświadczenia często zmieniając hasło i powstrzymać się od przy użyciu tych poświadczeń na komputerach publicznych.

Możliwości różnych urządzeń IoT różnią się. Niektóre urządzenia mogą być komputery z systemem wspólne systemy operacyjne, a niektóre urządzenia może działać bardzo lekki systemów operacyjnych. Najważniejsze wskazówki dotyczące zabezpieczeń opisane wcześniej może stosuje się do tych urządzeń w różnym stopniu. Jeśli dostępne, należy przestrzegać dodatkowych zabezpieczeń i wdrażania najlepszych praktyk z producenci tych urządzeń.

Niektóre starsze i ograniczonego urządzenia może nie zostały zaprojektowane specjalnie dla wdrożenia IoT. Tych urządzeń może być brak zdolności do szyfrowania danych, połączyć się z Internetem lub dostarczyć zaawansowanych zasad inspekcji. W takich przypadkach bramy nowoczesnych i bezpieczne pole agregowanie danych ze starszych urządzeń i zapewniają zabezpieczenia, wymaganego do połączenia tych urządzeń w Internecie. Pole bram może zapewnić bezpieczne uwierzytelnianie, negocjację zaszyfrowanych sesji, otrzymania polecenia z chmury i inne funkcje zabezpieczeń.
