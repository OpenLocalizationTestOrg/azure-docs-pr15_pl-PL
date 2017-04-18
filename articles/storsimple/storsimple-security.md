<properties 
   pageTitle="Zabezpieczenia StorSimple | Microsoft Azure" 
   description="W tym artykule opisano funkcje zabezpieczeń i prywatności, które chronią usługi StorSimple, urządzenia i dane w wersji lokalnej i w chmurze." 
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
   ms.date="05/03/2016"
   ms.author="v-sharos"/>

# <a name="storsimple-security-and-data-protection"></a>StorSimple zabezpieczeń i ochrony danych

## <a name="overview"></a>Omówienie

Bezpieczeństwo jest głównym problemem dla każdego, kto jest przyjęcie nową technologię, zwłaszcza w przypadku, gdy technologia jest używana z poufne lub własnych danych. Jak sprawdzić różnych technologii, należy rozważyć zwiększone ryzyko i koszty ochrony danych. Microsoft Azure StorSimple zawiera zabezpieczeń i prywatności rozwiązanie dla ochrony danych w celu zapewnienia: 

- **Poufności** — tylko do autoryzowanych jednostki, można wyświetlać dane. 
- **Integralność** — tylko dozwolone jednostek można zmodyfikować lub usunąć dane.

Rozwiązanie Microsoft Azure StorSimple składa się z czterech głównych współpracujących ze sobą części:

- **Usługa Menedżera StorSimple obsługiwany w programie Microsoft Azure** — Usługa zarządzania, która służy do konfigurowania i obsługi administracyjnej urządzenia StorSimple.
- **Urządzenie StorSimple** — urządzenia fizycznego zainstalowanych w centrum danych. Wszystkie hosty i klienci, generujących danych połączyć się z urządzeniem StorSimple, a urządzenie zarządza dane i przenosi je do chmury Azure zależnie od potrzeb.
- **Klienci hosty podłączone do urządzenia** — klientom w infrastrukturze połączyć się z urządzeniem StorSimple oraz generowanie danych, który ma być chroniony.
- **Magazynu w chmurze** — lokalizacji w chmurze Azure, w której są przechowywane dane.

W poniższych sekcjach opisano funkcje zabezpieczeń StorSimple, które zapewniają ochronę każdy z tych składników oraz danych przechowywanych w nich. Zawiera listę problemów, jakie może być dotyczących zabezpieczeń firmy Microsoft Azure StorSimple i skojarzonych z nimi odpowiedzi.

## <a name="storsimple-manager-service-protection"></a>Ochrona usługi Menedżera StorSimple

Usługa Menedżera StorSimple to usługa zarządzania obsługiwany w programie Microsoft Azure i używana do zarządzania wszystkich urządzeń StorSimple, które ma zamówionych Twojej organizacji. Można uzyskać dostęp do usług Menedżera StorSimple przy użyciu poświadczeń organizacji do logowania się do portalu klasyczny Azure za pomocą przeglądarki sieci web. 

Dostęp do usługi Menedżera StorSimple wymaga, że Twoja organizacja ma subskrypcję usługi Azure, która zawiera StorSimple. Twoja subskrypcja decyduje funkcje, których będziesz mieć dostęp w portalu klasyczny Azure. Jeśli Twoja organizacja nie ma subskrypcję usługi Azure i chcesz dowiedzieć się więcej o nich, zobacz [konta w usłudze Azure jako organizacji](../active-directory/sign-up-organization.md). 

Ponieważ usługa Menedżera StorSimple znajduje się w Azure, jest chroniony przez funkcje zabezpieczeń Azure. Aby uzyskać więcej informacji o funkcjach zabezpieczeń firmy Microsoft Azure przejdź do [Centrum zaufania programu Microsoft Azure](https://azure.microsoft.com/support/trust-center/security/).

## <a name="storsimple-device-protection"></a>Ochrona urządzenia StorSimple

StorSimple urządzenie jest lokalnego hybrydowych miejsca do magazynowania zawierający stanie stałym dyski SSD i dysków twardych (dysków twardych), razem z nadmiarowe kontrolery i możliwości automatyczne przejście. Kontrolery Zarządzanie magazynem obsługi, umieszczania obecnie używane (lub ciepłej) danych w magazynie lokalnym (w StorSimple urządzenia lub lokalnych serwerów), podczas przenoszenia mniej danych często używanych w chmurze.

Tylko autoryzowane StorSimple urządzenia są dozwolone do przyłączenia się do usługi Menedżera StorSimple utworzony w subskrypcji usługi Azure. Aby zezwolić na urządzeniu, musisz zarejestrować go usłudze Menedżer StorSimple, dostarczając klucza rejestru usługi. Klucz rejestru usługi jest kluczem losowe 128-bitowego wygenerowane w portalu klasyczny Azure. 

![Klucz rejestru usługi](./media/storsimple-security/ServiceRegistrationKey.png)

Aby dowiedzieć się, jak uzyskać dostęp do klucza rejestru usługi, przejdź do pozycji [Krok 2: uzyskiwanie klucza rejestru usługi](storsimple-deployment-walkthrough.md#step-2-get-the-service-registration-key).

Klucz rejestru usługi jest długi klucz zawierający 100 + znaków. Możesz skopiować klucz i zapisz go w pliku tekstowym w bezpiecznej lokalizacji, aby można go autoryzować dodatkowych urządzeń stosownie do potrzeb. Jeśli klucz rejestru usługi zostaną utracone po zarejestrowaniu urządzenia pierwszy, można wygenerować nowy klucz w usłudze Menedżer StorSimple. Nie dotyczy to działanie istniejących urządzeń. 

Po zarejestrowaniu urządzenia używa tokenów można komunikować się z Microsoft Azure. Klucz rejestru usługi nie jest używany po zarejestrowaniu urządzenia.

> [AZURE.NOTE] Zaleca się ponownie wygenerować klucza rejestru usługi po każdym użyciu.

## <a name="protect-your-storsimple-solution-via-passwords"></a>Ochrona rozwiązania StorSimple za pomocą hasła

Hasła są ważnym aspektem zabezpieczeń komputera i są szeroko w rozwiązaniu StorSimple aby zapewnić, że dane są dostępne tylko autoryzowani użytkownicy. StorSimple można skonfigurować następujące hasła:

- Hasło administratora urządzenia StorSimple
- Wezwania hasła Inicjator i docelowej uzgadniania uwierzytelniania CHAP (Protocol)
- Hasło StorSimple migawkę Manager

### <a name="windows-powershell-for-storsimple-and-the-storsimple-device-administrator-password"></a>Windows PowerShell dla StorSimple i hasła administratora urządzenia StorSimple

Windows PowerShell dla StorSimple to interfejs wiersza polecenia, który umożliwia zarządzanie urządzeniem StorSimple. Windows PowerShell dla StorSimple zawiera funkcje, które umożliwiają rejestrowanie urządzenia, skonfigurować interfejs sieciowy na urządzeniu, zainstalować niektórych rodzajów aktualizacje, rozwiązywanie problemów z urządzenia po zalogowaniu się do sesji pomocy technicznej i Zmień stan urządzenia. Można korzystać z programu Windows PowerShell dla StorSimple przez nawiązanie połączenia z konsoli szeregowego na urządzeniu lub za pomocą programu Windows PowerShell zdalnych.

Zdalne programu PowerShell można zrobić na HTTPS lub HTTP. Po włączeniu zdalnego zarządzania za pośrednictwem protokołu HTTPS, będzie konieczne certyfikat zarządzania zdalnego z urządzenia go pobrać i zainstalować na komputerze zdalnym klienta. Aby uzyskać więcej informacji na temat zdalnej programu PowerShell przejdź do [Łączenie zdalnie na urządzeniu StorSimple](storsimple-remote-connect.md).

Po nawiązywanie połączenia z urządzenia za pomocą programu Windows PowerShell dla StorSimple może być konieczne podanie hasła administratora urządzenia, aby zalogować się do urządzenia.

![Hasło administratora urządzenia](./media/storsimple-security/DeviceAdminPW.png)

Pamiętaj o następujących wskazówkach:

- Zarządzania zdalnego jest domyślnie wyłączona. Za pomocą usługi Menedżera StorSimple ją włączyć. Ze względów bezpieczeństwa dostęp zdalny powinno być włączone tylko w tym okresie, które są faktyczną potrzebne.
- Jeśli zmienisz hasło, pamiętaj powiadomić wszystkich użytkowników dostępu zdalnego tak, aby nie będą występować utracie łączności nieoczekiwane.
- Usługa Menedżera StorSimple nie można pobrać istniejących haseł: tylko mogą resetować je. Zaleca się przechowywać wszystkie hasła w bezpiecznym miejscu, dzięki czemu nie trzeba do resetowania hasła jest zapomnienia. Jeśli potrzebujesz zresetować hasło, pamiętaj powiadomić wszystkich użytkowników, zanim je zresetować. 

Masz dostęp do interfejsu programu Windows PowerShell, za pomocą połączenia szeregowego na urządzeniu. Możesz również do niego dostęp zdalny przy użyciu protokołu HTTP lub HTTPS, które zapewniają dodatkowe zabezpieczenia. Protokół HTTPS oferuje wyższy poziom zabezpieczeń niż kolejną lub połączenia HTTP. Jednak użycie protokołu HTTPS, należy najpierw zainstalować certyfikat na komputerze klienckim, które będą korzystać z danego urządzenia. Na stronie Konfiguracja urządzenia w usłudze Menedżer StorSimple, możesz pobrać certyfikat dostępu zdalnego. Jeśli certyfikat dla dostępu zdalnego zostaną utracone, należy pobrać nowego certyfikatu i propagowanie go do wszystkich klientów, którzy mogą korzystać zdalnego zarządzania.

### <a name="challenge-handshake-authentication-protocol-chap-initiator-and-target-passwords"></a>Wezwania hasła Inicjator i docelowej uzgadniania uwierzytelniania CHAP (Protocol)

Protokół CHAP jest schematu uwierzytelniania używany przez urządzenie StorSimple do sprawdzania tożsamości klientów zdalnych. Weryfikacja jest oparty na wspólne hasło. CHAP może być jednokierunkowa (jednokierunkowe) lub wzajemnej (dwukierunkowe). Z jednokierunkowe CHAP target (urządzenie StorSimple) uwierzytelnia Inicjator (hosta). Wzajemnego lub odwrotnej CHAP wymaga celu uwierzytelnienia Inicjator i następnie inicjator uwierzytelnić docelowej. Aby korzystać z innej metody można skonfigurować usługi StorSimple.

Po skonfigurowaniu CHAP należy pamiętać o następujących czynności:

- Nazwa użytkownika CHAP musi zawierać mniej niż 233 znaki.
- Hasło CHAP musi zawierać od 12 do 16 znaków. Próba dłużej nazwę użytkownika lub hasło spowoduje błędu uwierzytelniania na hoście systemu Windows.
- Nie można używać tego samego hasła, zarówno inicjator CHAP i docelowej CHAP.
- Po ustawieniu hasła, można ją zmienić, ale nie można pobrać. Po zmianie hasła należy powiadomić wszystkich użytkowników dostępu zdalnego tak, aby można połączyć się z urządzeniem StorSimple.

Aby uzyskać więcej informacji na temat CHAP i skonfiguruj go dotyczących rozwiązania StorSimple przejdź do [Skonfigurowania CHAP dla swojego urządzenia StorSimple](storsimple-configure-chap.md).

### <a name="storsimple-snapshot-manager-password"></a>Hasło StorSimple migawkę Manager

Menedżer migawkę StorSimple jest przystawką programu Microsoft Management Console (MMC) używanego głośność grupy i usługi kopiowania cień głośność systemu Windows do generowania spójną z aplikacją kopie zapasowe. Ponadto można użyć StorSimple migawkę Menedżera do tworzenia kopii zapasowej harmonogramów i klonowanie lub przywracanie wielkości.

Po skonfigurowaniu urządzenia za pomocą Menedżera migawkę StorSimple, trzeba będzie o podanie hasła StorSimple migawkę menedżera. To hasło jest najpierw ustawiona w programie Windows PowerShell dla StorSimple podczas rejestrowania. Można również ustawić hasło i zmienione przez usługę Menedżer StorSimple. To hasło uwierzytelnia urządzenia przy użyciu Menedżera migawkę StorSimple.

![Hasło StorSimple migawkę Manager](./media/storsimple-security/SnapshotMgrPassword.png)

Hasło StorSimple migawkę Manager musi być 14 do 15 znaków i musi zawierać 3 lub więcej z kombinacji wielkie litery, małe litery, liczbowe i znaki specjalne. Po ustawieniu hasła Menedżer migawkę StorSimple mogą być zmieniane, ale nie można pobrać. Jeśli zmienisz hasło, pamiętaj powiadomić wszystkich użytkowników zdalnych.

Aby uzyskać więcej informacji na temat Menedżera migawkę StorSimple, przejdź do [Co to jest Menedżer migawkę StorSimple?](storsimple-what-is-snapshot-manager.md)

### <a name="password-best-practices"></a>Najważniejsze wskazówki dotyczące haseł

Firma Microsoft zaleca używanie tych wskazówek w celu zapewnienia, że StorSimple haseł silnych i dobrze chronione:

- Zmienianie hasła na trzy miesiące. Zmienianie hasła są wymuszane rocznym.
- Używaj silnych haseł. Aby uzyskać więcej informacji przejdź do [tworzyć silniejsze hasła i ich ochrony](http://blogs.microsoft.com/cybertrust/2014/08/25/create-stronger-passwords-and-protect-them/).
- Zawsze używaj różnych haseł dla różnych dostępu mechanizmy; Każdy haseł, które można określić musi być unikatowa.
- Nie są udostępniane haseł każda osoba, która nie jest autoryzowany uzyskać dostęp do urządzenia StorSimple.
- Czytaj o hasło przed innymi osobami lub nie lodowej format hasła.
- Jeśli podejrzewasz, że konto lub hasło jest już bezpieczne, Zgłoś przypadek do działu bezpieczeństwa informacji.
- Wszystkie hasła traktowane jako poufne informacje poufne. 

## <a name="storsimple-data-protection"></a>Ochrona danych StorSimple

W tej sekcji opisano funkcje zabezpieczeń StorSimple, które ochrony danych w drodze i przechowywanych danych.

Zgodnie z opisem w innej sekcji, hasła są używane do autoryzacji i uwierzytelniania użytkowników, przed ich mogą uzyskać dostęp do swojego rozwiązania StorSimple. Kolejne zagadnienie zabezpieczeń Ochrona danych przez nieautoryzowanych użytkowników, podczas są przesyłane między systemami miejsca do magazynowania, a także podczas są przechowywane. W poniższych sekcjach opisano funkcje ochrony danych dostarczoną z StorSimple.

> [AZURE.NOTE] Deduplication zapewnia dodatkową ochronę danych przechowywanych na tym urządzeniu StorSimple i w magazynie Microsoft Azure. Gdy dane jest deduplicated, obiekty danych są przechowywane niezależnie od metadanych służący do mapowania i uzyskiwać do nich dostęp: nie jest dostępny kontekst poziom przechowywania odtworzenie danych na podstawie struktury woluminu, system plików lub nazwę pliku.

## <a name="protect-data-flowing-through-the-service"></a>Ochrona danych ułożony za pośrednictwem usługi

Głównym celem usługę Menedżer StorSimple jest zarządzanie i konfigurowanie urządzenia StorSimple. Usługa Menedżer StorSimple działa w Microsoft Azure. Wprowadzanie danych o konfiguracji urządzenia za pomocą portalu klasyczny Azure, a następnie Microsoft Azure używa usługi Menedżera StorSimple wysyłanie danych do urządzenia. StorSimple korzysta z systemu asymetrycznym par kluczy, aby upewnić się, że złamanie usługi Azure nie spowoduje ujawnienia informacji. 

![Szyfrowanie danych w samolotu](./media/storsimple-security/DataEncryption.png)

Asymetrycznym system klucza chroni danych za pośrednictwem usługi w następujący sposób:

1. Certyfikat szyfrowania danych, który używa asymetrycznym klucza publicznych i prywatnych pary jest generowany na tym urządzeniu i są one używane do ochrony danych. Klucze są generowane po pierwszym urządzeniu jest zarejestrowany. 
2. Klucze certyfikatu szyfrowania dane są eksportowane do pliku wymiana informacji osobistych (pfx), który jest chroniony klucza szyfrowania danych usługi, czyli silnego klucza 128-bitowego, jest generowany losowo przy pierwszym urządzeniu podczas rejestrowania.
3. Klucz publiczny certyfikatu bezpieczne były dostępne dla usługi Menedżera StorSimple, a klucz prywatny pozostaje z urządzeniem.
4. Wprowadzanie usługę danych jest zaszyfrowany przy użyciu publicznego klucza i odszyfrowane przy użyciu klucza prywatnego przechowywanych na urządzeniu, zapewnienie, że usługa Azure nie można odszyfrować danych ułożony na urządzeniu.

Klucza szyfrowania danych usługi jest generowany na tylko pierwsze urządzenie zarejestrowane w usłudze. Wszystkie kolejne urządzenia, które są zarejestrowane w usłudze należy użyć tego samego klucza szyfrowania danych usługi. 

> [AZURE.IMPORTANT] 
> 
> Jest bardzo ważne, aby utworzyć kopię klucza szyfrowania danych usługi i zapisz go w bezpiecznym miejscu. Kopię klucza szyfrowania danych usługi powinny znajdować się w taki sposób, mogą uzyskiwać dostęp do autoryzowanych osoby i można łatwo opisywać administratora urządzenia.
>
> W przypadku utracone klucza szyfrowania danych usługi pomocy technicznej firmy Microsoft może ułatwić go pobrać, pod warunkiem, że masz co najmniej jedno urządzenie w trybie online. Zaleca się zmienianie klucza szyfrowania danych usługi, po pobraniu go. Aby uzyskać instrukcje przejdź do sekcji [Zmienianie klucza szyfrowania danych usługi](storsimple-service-dashboard.md#change-the-service-data-encryption-key).

Klucz szyfrowania danych usługi i odpowiadające im certyfikat szyfrowania danych można zmienić, wybierając opcję **Zmienianie klucza szyfrowania danych usługi** na pulpicie nawigacyjnym usługi. W celu zapewnienia, że nie odczyta zabezpieczania danych, należy użyć urządzenia fizycznego StorSimple, aby zmienić klucza szyfrowania danych usługi. Zmiana kluczy szyfrowania wymaga, że wszystkie urządzenia jest zaktualizowany przy użyciu nowego klucza. Dlatego zalecamy Zmienianie klucza, gdy wszystkie urządzenia są w trybie online. Jeśli urządzenia są w trybie offline, klucze mogą być zmieniane w innym czasie. Urządzenia z klawiszami nieaktualne nadal będzie możliwe wykonywania kopii zapasowych, ale nie będzie można przywrócić dane do momentu klucz jest aktualizowana. Aby uzyskać więcej informacji przejdź do tematu [Używanie pulpit nawigacyjny usługi Menedżera StorSimple](storsimple-service-dashboard.md).

Nie wygasa klucza szyfrowania danych usługi i certyfikat szyfrowania danych. Jednak zaleca się zmianę klucza szyfrowania danych usługi rocznym w celu zapobiegania złamania klucza.

## <a name="protect-data-at-rest"></a>Ochrona danych spoczynku

Urządzenie StorSimple zarządza dane przechowywane w poziomów lokalnie, jak i w chmurze, w zależności od częstość używania. Wszystkich komputerach hosta, które są połączone z urządzeniem dane są wysyłane do urządzenia, które następnie przenosi dane w chmurze, zależnie od potrzeb. Dane są przenoszone z urządzenia w chmurze bezpiecznie przez Internet. Każde urządzenie ma jeden obiekt docelowy iSCSI, który wyświetla wszystkie ilości udostępnionych na tym urządzeniu. Wszystkie dane są szyfrowane przed wysłaniem go do magazynu w chmurze. 

![Klucza szyfrowania w chmurze miejsca do magazynowania](./media/storsimple-security/CloudStorageEncryption.png)

Aby zapewnić zabezpieczeń i integralności danych przenoszone w chmurze, StorSimple umożliwia definiowanie kluczy szyfrowania miejsca do magazynowania w chmurze w następujący sposób:

- Podczas tworzenia kontenera głośność określić klucza szyfrowania miejsca do magazynowania w chmurze. Klucz nie można modyfikować ani dodane później. 
- Wszystkie ilości w kontenerze głośność udostępnianie tego samego klucza szyfrowania. Jeśli chcesz innego formularza szyfrowania dla określonego woluminu, zaleca się utworzenie nowego kontenera głośności do obsługi tego woluminu.
- Po wprowadzeniu klucza szyfrowania chmury miejsca do magazynowania w usłudze Menedżer StorSimple klucz jest zaszyfrowany przy użyciu części publicznej klucza szyfrowania danych usługi, a następnie wysyłane do urządzenia.
- Klucza szyfrowania miejsca do magazynowania w chmurze nie jest przechowywana w dowolnym miejscu w usłudze i jest znany tylko do urządzenia.
- Określenie kluczem szyfrowania chmury miejsca do magazynowania jest opcjonalne. Możesz wysłać dane, które zostały zaszyfrowane na hoście na urządzeniu.

### <a name="additional-security-best-practices"></a>Najważniejsze wskazówki dotyczące zabezpieczeń dodatkowe

- Dzielenie ruchu: wyodrębnienia wdrażanie całkowicie rozdzielone sieci i używając VLAN, gdzie fizycznie izolacji nie jest opcja iSCSI SAN z ruch użytkownika w firmowej sieci LAN. Sieci dla magazynu iSCSI będzie zapewnienia bezpieczeństwa i wydajności biznesowej ważnych danych. Mieszanie ruch użytkownika i magazynowania w firmowej sieci LAN nie jest zalecane, można zwiększyć opóźnienie i powodować awarie sieci.

- Dla sieci po stronie hosta zabezpieczeń za pomocą interfejsów sieciowych, które obsługują protokół TCP/IP Offload Engine (TOE). TOE zmniejsza obciążenie procesora CPU przez przetwarzania TCP na karcie sieciowej.

## <a name="protect-data-via-storage-accounts"></a>Ochrona danych za pomocą konta miejsca do magazynowania

Każdej subskrypcji Microsoft Azure można utworzyć jeden lub więcej kont miejsca do magazynowania. (Konto miejsca do magazynowania zawiera unikatowych nazw do pracy z danymi przechowywanymi w chmurze Azure). Dostęp do konta miejsca do magazynowania steruje klawiszy subskrypcji i programu access, związane z tym kontem miejsca do magazynowania. 

Po utworzeniu konta magazynu Microsoft Azure generuje dwóch klawiszy dostępu 512-bitowej miejsca do magazynowania, z których jedna jest używany do uwierzytelniania, gdy urządzenie StorSimple uzyskuje dostęp do konta miejsca do magazynowania. Należy zauważyć, że tylko jeden z tych kluczy jest używany. Inne klucza jest przechowywana w minimalnej, co umożliwia obracanie klucze okresowo. Aby obrócić klawiszy, uaktywnić klucza pomocniczego, a następnie usuń klucz podstawowy. Następnie możesz utworzyć nowy klucz do użytku podczas następnego obrotu. (Ze względów bezpieczeństwa wiele centrach danych wymagają klucza obrotu). 

Zaleca się, należy wykonać poniższe najważniejsze wskazówki dotyczące obrotu kluczy:

- Należy obrócić klawiszy konta miejsca do magazynowania regularnie, aby upewnić się, że Twoje konto miejsca do magazynowania nie jest dostępna przez nieupoważnionych użytkowników.
- Okresowo Azure administrator należy zmienić lub wygenerować klucz podstawowy i pomocniczy za pomocą sekcji miejsca do magazynowania Azure portalu klasyczny bezpośrednio dostępu do konta miejsca do magazynowania.


## <a name="protect-data-via-encryption"></a>Ochrona danych za pomocą szyfrowania

StorSimple używa następujące algorytmy szyfrowania w celu ochrony danych przechowywanych w lub podróże między składnikami rozwiązania StorSimple.

| Algorytm | Długość klucza | Protokoły/applications/komentarzy |
| --------- | ---------- | ------------------------------- |
| RSA       | 2048       | 1.5 RSA PKCS 1 jest używany przez portal Azure klasyczny do szyfrowania danych konfiguracji, które są wysyłane do urządzenia: na przykład przechowywania poświadczeń, StorSimple urządzenia konfiguracji konta i kluczy szyfrowania magazynu w chmurze. |
| AES       | 256        | AES z CBC jest używany do szyfrowania publicznej części klucza szyfrowania danych usług, przed wysłaniem do portalu klasyczny Azure za pomocą urządzenia StorSimple. Również jest używany przez urządzenie StorSimple w do szyfrowania danych przed wysłaniem danych do chmury konta miejsca do magazynowania. |


## <a name="storsimple-virtual-device-security"></a>StorSimple urządzenia wirtualnego zabezpieczeń

[AZURE.INCLUDE [storsimple virtual device security](../../includes/storsimple-virtual-device-security.md)]

## <a name="frequently-asked-questions-faq"></a>Często zadawane pytania

Poniżej przedstawiono niektóre pytania i odpowiedzi dotyczące zabezpieczeń i Microsoft Azure StorSimple.

**Q:** Moje usługi jest bezpieczne. Co należy Moje następne kroki?

**A:** Należy natychmiast zmienić klucza szyfrowania danych usługi i klucze konta miejsca do magazynowania dla konta miejsca do magazynowania, który jest używany do obsługi danych. Aby uzyskać instrukcje przejdź do: 

- [Zmienianie klucza szyfrowania danych usługi](storsimple-service-dashboard.md#change-the-service-data-encryption-key)
- [Kluczowe obrotu kont miejsca do magazynowania](storsimple-manage-storage-accounts.md#key-rotation-of-storage-accounts)

**Q:** Mam nowe urządzenie StorSimple, którego żąda klucza rejestru usługi. Jak pobrać go

**A:** Ten klucz został utworzony podczas tworzenia usługę Menedżer StorSimple. Gdy używasz usługi Menedżera StorSimple do nawiązania połączenia z urządzeniem, w stronę szybki start dla usługi umożliwia wyświetlanie i ponownie wygenerować klucza rejestru usługi. Generowanie nowego klucza rejestru usługi nie wpływa na istniejących urządzeniach zarejestrowane. Aby uzyskać instrukcje przejdź do:

- [Wyświetlanie i ponownie wygenerować klucza rejestru usługi](storsimple-service-dashboard.md#view-or-regenerate-the-service-registration-key)

**Q:** Utrata mojego klucza szyfrowania danych usługi. Co należy zrobić?

**A:** Kontakt z pomocą techniczną firmy Microsoft. Czy logowania się do sesji pomocy technicznej na urządzeniu i pomocy pobrać klucz (pod warunkiem, co najmniej jedno urządzenie jest w trybie online). Natychmiast po uzyskaniu klucza szyfrowania danych usługi, należy zmienić ją, aby upewnić się, że nowy klucz jest znany tylko dla Ciebie. Aby uzyskać instrukcje przejdź do:

- [Zmienianie klucza szyfrowania danych usługi](storsimple-service-dashboard.md#change-the-service-data-encryption-key)

**Q:**  Autoryzowanych urządzenia w celu zmiany klucza szyfrowania danych usługi, ale nie został uruchomiony proces zmiany klucza. Co należy zrobić?

**A:** Po wygaśnięciu limitu czasu będzie konieczne ponownie autoryzować urządzenia w celu zmiany klucza szyfrowania danych usługi i ponownie uruchomić proces.

**Q:**  Po zmianie klucza szyfrowania danych usługi, ale nie była dostępna aktualizacja innych urządzeń w ciągu 4 godzin. Czy muszę ponownie uruchomić?

**A:** Okres 4-godzinnym jest tylko do inicjowania zmiany. Po rozpoczęciu procesu aktualizacji na tym urządzeniu autoryzowanych StorSimple zezwolenie jest prawidłowy, dopóki nie zostaną zaktualizowane wszystkich urządzeń.

**Q:** Nasze administrator StorSimple opuścił firmy. Co należy zrobić?

**A:** Zmienianie i resetowanie haseł, które umożliwiają dostęp do urządzenia StorSimple i zmienianie klucza szyfrowania danych usługi, aby upewnić się, że nowe informacje nie jest znany nieupoważnionym osobom. Aby uzyskać instrukcje przejdź do:

- [Zmienianie hasła storsimple za pomocą usługi Menedżera StorSimple](storsimple-change-passwords.md)
- [Zmienianie klucza szyfrowania danych usługi](storsimple-service-dashboard.md#change-the-service-data-encryption-key)
- [Konfigurowanie CHAP dla swojego urządzenia StorSimple](storsimple-configure-chap.md)

**Q:** Chcę, aby hasło StorSimple migawkę Manager do hosta, który łączy się z urządzenia StorSimple, ale hasło nie jest dostępne. Co można zrobić?

**A:** Jeśli pamiętasz hasła, należy utworzyć nową. Następnie upewnij się poinformować wszystkich istniejących użytkowników, że hasło zostało zmienione i należy zaktualizowania klientom przy użyciu nowego hasła. Aby uzyskać instrukcje przejdź do:

- [Zmienianie hasła Menedżer migawkę StorSimple](storsimple-change-passwords.md#change-the-storsimple-snapshot-manager-password)
- [Uwierzytelnianie urządzenia](storsimple-snapshot-manager-manage-devices.md#authenticate-a-device)

**Q:** Certyfikat dla zdalnego dostępu do programu Windows PowerShell dla StorSimple została zmieniona na urządzeniu. Jak zaktualizować klientom dostępu zdalnego?

**A:** Można pobrać usługę Menedżer StorSimple nowego certyfikatu, a następnie podaj go musi być zainstalowany w magazynie certyfikatów klientów dostępu zdalnego. Aby uzyskać instrukcje przejdź do:

- [Polecenie cmdlet Importowanie certyfikatu](https://technet.microsoft.com/library/hh848630.aspx)

**Q:** Jest moje dane chronione Menedżera StorSimple usługi jest bezpieczne?

**A:** Dane konfiguracji usługi są zawsze szyfrowane kluczem publicznym podczas wyświetlania w przeglądarce sieci web. Ponieważ usługa nie ma dostępu do klucz prywatny, usługa nie będzie mógł wyświetlić wszystkie dane. Jeśli usługa Menedżera StorSimple jest bezpieczne, nie ma żadnego wpływu są przechowywane w usłudze Menedżer StorSimple kluczy.

**Q:** Gdy ktoś uzyskuje dostęp do danych certyfikat szyfrowania, będzie odczyta można Moje dane?

**A:** Microsoft Azure przechowuje klucza szyfrowania danych klienta (plik pfx) w postaci zaszyfrowanej. Ponieważ plik PFX są szyfrowane i usługa StorSimple nie ma klucza szyfrowania danych usługi odszyfrować plik PFX, po prostu uzyskiwanie dostępu do pliku PFX będzie nie uwidaczniają wszelkie hasła.

**Q:** Co się stanie, jeśli podmiot rządowych zwróci firmy Microsoft dla moich danych?

**A:** Ponieważ wszystkie dane są szyfrowane w usłudze i klucz prywatny jest przechowywany z urządzeniem, rządowych jednostki muszą uzyskać dane klienta. 

## <a name="next-steps"></a>Następne kroki

[Rozmieszczanie urządzenia StorSimple](storsimple-deployment-walkthrough.md).
 
