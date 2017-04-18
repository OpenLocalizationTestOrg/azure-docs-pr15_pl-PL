<properties
   pageTitle="Instalowanie urządzenia StorSimple 8600 | Microsoft Azure"
   description="Informacje dotyczące Rozpakowywanie, stojaków instalacji i kabla urządzenia StorSimple 8600 przed wdrażanie i skonfigurować oprogramowanie."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="10/24/2016"
   ms.author="alkohli" />

# <a name="unpack-rack-mount-and-cable-your-storsimple-8600-device"></a>Rozpakowywanie, Instalacja stojaków, a kabel urządzenia StorSimple 8600

## <a name="overview"></a>Omówienie
Usługi Microsoft Azure StorSimple 8600 jest to urządzenie podwójne załącznik i składa się z głównego i załącznika EBOD. Ten samouczek wyjaśniono, jak Rozpakowywanie, stojaków i kabla StorSimple 8600 urządzenie przed skonfigurować oprogramowanie StorSimple.

## <a name="unpack-your-storsimple-8600-device"></a>Rozpakowywanie urządzenia StorSimple 8600

Poniższe kroki zawierają Wyczyść, szczegółowe instrukcje dotyczące Rozpakowywanie urządzenia magazynowania StorSimple 8600. To urządzenie jest dostarczany w dwóch polach, jedną dla podstawowego załącznik i drugi załącznik EBOD. Te dwa pola następnie są umieszczane w jednym polu.

### <a name="prepare-to-unpack-your-device"></a>Przygotowywanie się do Rozpakowywanie urządzenia

Przed Rozpakowywanie urządzenia, przejrzyj poniższe informacje.


![Ikona ostrzeżenia](./media/storsimple-safety/IC740879.png)![ikona intensywnie grubość](./media/storsimple-8600-hardware-installation/HCS_HeavyWeight_Icon.png) **Ostrzeżenie!**

1. Upewnij się, że masz dwie osoby dostępny do zarządzania grubość urządzenia, jeśli są obsługuje je ręcznie. W pełni skonfigurowane załącznik można oceny do 32 kg (70 kg.).
1. Umieść pole powierzchni płaskiej, poziomu.

Następnie wykonaj następujące czynności, aby Rozpakowywanie urządzenia.

#### <a name="to-unpack-your-device"></a>Aby Rozpakowywanie urządzenia

1. Sprawdź, czy pole i pianki opakowań crushes, części, uszkodzenia wody lub oczywistych uszkodzeń. Jeśli pole lub opakowanie poważnie jest uszkodzony, nie można otwierać okna. Zapoznaj [się z pomocą techniczną firmy Microsoft](storsimple-contact-microsoft-support.md) ułatwiające sprawdzenia, czy urządzenie znajduje się w dobrym stanie.

2. Otwieranie okna zewnętrzne, a następnie wykupić dwa pola odpowiadające podstawowego i EBOD załączniki. Można teraz Rozpakowywanie podstawowy i EBOD załączniki. Na poniższej ilustracji pokazano widoku rozpakowane jednego z załączników.

    ![Rozpakowywanie urządzenia miejsca do magazynowania](./media/storsimple-8600-hardware-installation/HCSUnpackyour4Udevice.png)

    **Widok rozpakowane urządzenia miejsca do magazynowania**

     Etykieta | Opis
     ----- | -------------
     1     | Pole dokumentu
     2     | Kable skojarzeń zabezpieczeń (w pasku Akcesoria i kable)
     3     | Pianki dołu
     4     | Urządzenie
     5     | Górny pianki
     6     | Pole akcesoriów

3. Po Rozpakowywanie upewnij się, że masz dwa pola:

  - 1 załącznik podstawowego (załącznik podstawowego i załącznik EBOD są w dwóch oddzielnych polach)
  - 1 załącznik EBOD
  - kable 4, 2 w odpowiednich polach
  - 2 kable skojarzeń zabezpieczeń (nawiązać podstawowego załącznik załącznik EBOD)
  - 1 krzyżowym kabla Ethernet
  - 2 kable konsoli szeregowego
  - 1 Konwerter USB kolejną dostępu szeregowego
  - 4 QSFP-do-SFP + kart do użytku z 10 GbE interfejsów
  - 2 stojaków zestawów (4 szynach strony z instalowanie sprzętu, 2 załącznik podstawowego i załącznik EBOD), 1 w odpowiednich polach
  - Uzyskiwanie dokumentacji wprowadzenie

    Jeśli nie masz każdy z elementów wymienionych powyżej, [skontaktuj się z pomocą techniczną firmy Microsoft](storsimple-contact-microsoft-support.md).  

Następnym krokiem jest stojaków instalacji urządzenia.

## <a name="rack-mount-your-storsimple-8600-device"></a>Stojaków instalacji urządzenia StorSimple 8600

Wykonaj poniższe czynności, aby zainstalować urządzenia StorSimple 8600 miejsca do magazynowania w standardowej stojaków CAL 19 z przedniej i tylnej wpisów. To urządzenie zawiera dwa załączniki: załącznik podstawowego i załącznika EBOD. Oba te muszą być stojaków zainstalowany.

Instalacja składa się z wielu kroków, z których każdy będzie został omówiony w następujących procedur.

> [AZURE.IMPORTANT]
Urządzenia StorSimple musi być zainstalowany stojaków prawidłowe działanie.



### <a name="site-preparation"></a>Przygotowanie miejsca

Załączniki musi być zainstalowany w standardowej stojaków 19 cali, zarówno przedniej i tylnej wpisów. Poniższa procedura umożliwia przygotowanie do instalacji stojaków.

#### <a name="to-prepare-the-site-for-rack-installation"></a>Aby przygotować witryny instalacji stojaków

1. Upewnij się, że podstawowy i EBOD załączniki są nieaktywnych bezpiecznie na powierzchni Praca stała, stabilny i poziomu (lub podobnych).

2. Sprawdź, czy witryny, gdy chcesz skonfigurować ma standardowy power AC z niezależne źródło lub stojaków jednostki dystrybucji zasilania (PDU) z dostarczanego awaryjny (UPS).

3. Upewnij się, że ten 4U jeden przedział (2 X 2U) jest dostępny na stojaków, w którym chcesz zainstalować załączniki.

![Ikona ostrzeżenia](./media/storsimple-safety/IC740879.png)![ikona intensywnie grubość](./media/storsimple-8600-hardware-installation/HCS_HeavyWeight_Icon.png) **Ostrzeżenie!**

 Upewnij się, że masz dwie osoby dostępny do zarządzania grubość, jeśli Instalator urządzenia są obsługuje ręcznie. W pełni skonfigurowane załącznik można oceny do 32 kg (70 kg.).

### <a name="rack-prerequisites"></a>Wymagania wstępne dotyczące stojaków

Załączniki są przeznaczone dla instalacji w standardowej stojaków 19 cali szafka z:

- Minimalna głębokość 27.84 cala z stojaków wpis do wpisu
- Ciężar maksymalny 32 kg urządzenia
- Maksymalna liczba wstecz ciśnienia Pascala 5 (0,5 mm słupa wody)

### <a name="rack-mounting-rail-kit"></a>Instalowanie stojaków kolei kit

Zestaw zainstalowanie szyn zostaną udostępnione przez reprezentujący stojaków 19 cali. Szyny przetestowano do obsługi grubość maksymalna załącznik. Te szyny umożliwiają również instalację wiele załączników bez utraty obszaru stojaków. Najpierw zainstaluj załącznik EBOD.

#### <a name="to-install-the-ebod-enclosure-on-the-rails"></a>Aby zainstalować załącznik EBOD przy użyciu prowadnic

2. Tylko, jeśli wewnętrzne szyny nie są zainstalowane na urządzeniu, należy wykonać ten krok. Zazwyczaj szyn wewnętrzne są instalowane fabryczne. Jeśli nie zainstalowano szyny, zainstaluj kolei od lewej i prawej kolei slajdów na stronach obudowy załącznik. Dołącz ich za pomocą sześć śruby metryczne na każdej stronie. Ułatwiające orientacji slajdów kolei są oznaczone **LH — wierzch** i **RH — wierzch**i zakończenia, który jest dołączony do tyłu załącznik ma końcem.

    ![Dołączanie slajdów kolei podwoziem załącznik](./media/storsimple-8600-hardware-installation/HCSAttachingRailSlidestoEnclosureChassis.png)

    **Dołączanie kolei slajdów do boków załącznik**

    Etykieta | Opis
    ----- | -----------
    1     | M 3 x 4 śruby głowy przycisk
    2     | Obudowa slajdów

3. Dołącz kolei lewej i prawej stronie zestawów członkom stojaków szafka pionowy. Nawiasy są oznaczone **LH** **RH**i **Ta strona w górę** do przeprowadzenia właściwą orientacją.

4. Znajdź kolei Sworznie do przodu i do tyłu zestawu kolei. Rozszerzanie rail, aby zmieścił się między wpisów stojaków i wstawić numery PIN do przodu i tyłu stojaków wpis element pionowy dziury. Upewnij się, że zestawu kolei jest poziom.

5. Bezpieczny zestawu kolei do szafy pionowa członków za pomocą dwóch metryczne śruby opisane. Za pomocą jednego śruby na wierzch i jeden z tyłu.

6. Powtórz te czynności dla zestawu kolei.

     ![Dołączanie kolei slajdów do stojaków szafka](./media/storsimple-8600-hardware-installation/HCSAttachingRailSlidestoRackCabinet.png)

    **Dołączanie do szafy kolei zestawów**

     Etykieta | Opis
     ----- | -----------
     1     | Zaciskowe śruby
     2     | Śruby wpis stojaków przedniej otworu kwadratowy
     3     | Przednia kolei lokalizacji PIN w lewo
     4     | Zaciskowe śruby
     5     | Po lewej stronie kolei tylnej lokalizacji PIN

### <a name="mounting-the-ebod-enclosure-in-the-rack"></a>Instalowanie załącznik EBOD w stojaków

Przy użyciu stojaku, po prostu zainstalowane, wykonaj następujące czynności do zainstalowania załącznik EBOD w stojaków.

#### <a name="to-mount-the-ebod-enclosure"></a>Aby zainstalować załącznik EBOD

1. Asystent Podnieś załącznik i go wyrównać z stojaku.

2. Starannie wstawić załącznik do szyny, a następnie naciśnij go całkowicie do szafy cab.

    ![Wstawianie urządzenia stojaków](./media/storsimple-8600-hardware-installation/HCSInsertingDeviceintheRack.png)

    **Instalowanie załącznik w stojaków**

3. Usuwanie caps kołnierza lewy i prawy przedniej przenosząc naciśnij klawisze Caps Lock bezpłatne. Naciśnij klawisze caps kołnierza po prostu przyciąganie na stopka.

4. Bezpieczny załącznik do szafy instalując jeden dostarczonych śruby głowy Phillips za pośrednictwem każdego kołnierza, lewy i prawy.

4. Zainstaluj caps kołnierza, naciskając je w odpowiednim miejscu i przyciąganie ich na miejscu.

     ![Instalowanie kołnierza naciśnij klawisze Caps Lock](./media/storsimple-8600-hardware-installation/HCSInstallingFlangeCaps.png)

    **Instalowanie caps kołnierza**

     Etykieta | Opis
     ----- | -----------
     1     | Śruby zamknięcie załącznik


### <a name="mounting-the-primary-enclosure-in-the-rack"></a>Instalowanie podstawowego załącznik w stojaków

Po zakończeniu instalowanie załącznik EBOD należy zainstalować podstawowy załącznik, wykonując te same czynności.

> [AZURE.NOTE]
>
> - Użytkownik może mieć kilka gniazdo w stojaków między załącznik podstawowego i załącznik EBOD.
> - Nawiązywanie połączenia podstawowego załącznik z załącznik EBOD za pomocą kabla skojarzeń zabezpieczeń dostarczonych 2m.
> - Istnieje nie ograniczeń położenie względne jednostki głowy jednostce EBOD. Dlatego podstawowego załącznik można umieścić w górnym przedział i załącznik EBOD poniżej — lub odwrotnie.

Następnym krokiem jest kabel urządzenia w celu power, sieci i szeregowego programu access.

## <a name="cable-your-storsimple-8600-device"></a>Kabel urządzenia StorSimple 8600

Poniższe procedury wyjaśniono, jak kabla StorSimple 8600 urządzenia w celu power, sieci i połączenia szeregowe.

### <a name="prerequisites"></a>Wymagania wstępne

Przed rozpoczęciem kabla urządzenia, należy:

- Załącznik z podstawowego i załącznik EBOD całkowicie rozpakowane.
- 4 kable power (2 dla podstawowych i załącznik EBOD) dostarczone z urządzeniem
- 2 kable skojarzeń zabezpieczeń dostarczone z urządzeniem, aby nawiązać załącznik EBOD podstawowego załącznik
- Dostęp do 2 jednostki dystrybucji zasilania (PDU) (zalecane)
- Kable sieciowe
- Opisane kable szeregowe
- Konwerter USB kolejną za pomocą sterownika odpowiednie zainstalowane na komputerze PC (w razie potrzeby)
- Opisane 4 QSFP-do-SFP + kart do użytku z 10 GbE interfejsów
- [Obsługa 10 interfejsów sieciowych GbE na urządzeniu z systemem StorSimple sprzętu](storsimple-supported-hardware-for-10-gbe-network-interfaces.md)

### <a name="sas-and-power-cabling"></a>Skojarzenia zabezpieczeń i Power okablowania

Urządzenie ma zarówno podstawowy załącznik, jak i załącznik EBOD. Wymaga to jednostki, które można ze sobą kablem łączności szeregowego dołączone SCSI (SA) i power.

Podczas konfigurowania tego urządzenia po raz pierwszy, wykonaj czynności dla skojarzeń zabezpieczeń kable najpierw, a następnie wypełnij instrukcje dotyczące okablowania power.

[AZURE.INCLUDE [storsimple-cable-8600-for-SAS](../../includes/storsimple-sas-cable-8600.md)]

[AZURE.INCLUDE [storsimple-cable-8600-for-power](../../includes/storsimple-cable-8600-for-power.md)]

### <a name="network-cabling"></a>Kable sieciowe

Urządzenie jest w stanie wstrzymania aktywne konfiguracji: w dowolnym momencie jeden moduł kontrolera jest aktywna, a przetwarzanie wszystkie operacje dysku i sieci podczas moduł kontroler jest w stanie wstrzymania. Uszkodzenie kontrolera, wstrzymania kontroler natychmiast uaktywnia i będzie nadal występował wszystkie operacje dysku i sieci.

Do obsługi tej pracy awaryjnej zbędne kontrolera, musisz kabla sieci urządzenia, jak pokazano w następującej procedurze.

#### <a name="to-cable-for-network-connection"></a>Do kabla dla połączenia sieciowego

1. Urządzenie ma sześć interfejsów sieciowych na każdym kontrolerze: cztery 1 GB i dwóch 10 GB Ethernet porty. Zapoznaj się z poniższej ilustracji do identyfikowania portów danych tablica połączeń urządzenia.

     ![Tablica połączeń 8600 urządzenia](./media/storsimple-8600-hardware-installation/HCSBackplaneof2UDevicewithPortsLabeled.jpg)

    **Powrót z urządzenia przedstawiający porty danych**

     Etykieta   | Opis
     ------- | -----------
     0,1,4,5 |  1 GbE interfejsów
     wartości 2 3     | 10 GbE interfejsów
     6       | Porty szeregowe



1. Zobacz poniższy diagram dla kable sieciowe. (Konfiguracja sieci minimalne znajduje się niebieską linią ciągłą. Wysokiej dostępności i wydajności dodatkowa konfiguracja wymagane są wyświetlane jako linie kropkowane.)

![Kabel urządzenia 4U dla sieci](./media/storsimple-8600-hardware-installation/HCSCableYour4UDeviceforNetwork.png)

**Okablowania dla swojego urządzenia sieciowego**

Etykieta | Opis
----- | -----------
A    | Sieci LAN z dostępem do Internetu
B    | Kontroler 0
C    | PCM 0
D    | Kontroler 1
E    | PCM 1
F    | Kontroler EBOD 0
G    | Kontroler EBOD 1
H, I  | Hostów (na przykład plik serwerów)
0-5  | Interfejsy
6    | Podstawowy załącznik
7    | Załącznik EBOD

Gdy kable urządzenia, minimalna konfiguracja wymaga:


- Co najmniej dwóch interfejsów połączony na każdym kontrolerze z jedną dostęp w chmurze i jedną dla iSCSI. DANE 0 port jest automatycznie włączone i skonfigurowane za pomocą konsoli szeregowego urządzenia. Niezależnie od danych 0 innego portu danych należy również można skonfigurować za pośrednictwem portalu klasyczny Azure. W tym przypadku łączenie danych 0 portu podstawowego sieci LAN (sieć z dostępem do Internetu). Inne porty danych mogą być połączone z SAN-iSCSI sieci LAN (VLAN) część sieci, w zależności od ma pełnić.

- Identyczne interfejsów na każdym kontrolerze podłączony do tej samej sieci w celu zapewnienia dostępności, jeśli awaria kontroler wystąpiła. Na przykład jeśli wybierzesz połączenie danych 0 i dane 3 dla jednej kontroler, musisz połączyć odpowiadające im dane 0 i 3 danych na innym kontrolerze.

Pamiętaj o wysokiej dostępności i wydajności:


- Jeśli to możliwe, skonfiguruj para sieciowej dostęp w chmurze (1 GbE) i inną parę iSCSI (10 GbE polecane) na każdym kontrolerze.

- Jeśli to możliwe, Połącz interfejsy z każdego kontrolera do dwóch różnych przełączników w celu zapewnienia dostępności przed awariami Przełącz. Na rysunku pokazano dwa 10 GbE interfejsy sieciowe, dane 2 i 3 dane z każdego kontrolera podłączone do dwóch różnych przełączników. Aby uzyskać więcej informacji zapoznaj się z **interfejsów** w obszarze [wymagania wysoką dostępność dla swojego urządzenia StorSimple](storsimple-system-requirements.md#high-availability-requirements-for-storsimple).

>[AZURE.NOTE] Jeśli odbiorcze SFP + z 10 GbE interfejsów sieciowych, przy użyciu podanych QSFP-SFP + karty. Aby uzyskać więcej informacji przejdź do [sprzętu obsługiwane 10 interfejsów sieciowych GbE na urządzeniu StorSimple](storsimple-supported-hardware-for-10-gbe-network-interfaces.md).

### <a name="serial-port-cabling"></a>Kable portu szeregowego

Wykonaj poniższe czynności, aby kabla portu szeregowego.

#### <a name="to-cable-for-serial-connection"></a>Do kabla dla połączenia szeregowego

1. Urządzenie ma portu szeregowego na każdym kontrolerze, które są oznaczane ikoną klucza. Aby zlokalizować portów szeregowych, skorzystaj z ilustracji, w którym dane są wyświetlane porty z tyłu urządzenia.

2. Zidentyfikuj kontroler aktywne na tablica połączeń z urządzenia. Migający LED niebieski wskazuje, że kontroler jest aktywna.

3. Dostarczonych kabla szeregowego (w razie potrzeby konwerter kolejną USB komputera przenośnego), a podłączyć do portu szeregowego aktywnego kontrolera konsoli lub komputer (z końcowych emulacji do urządzenia).

4. Instalowanie sterowników USB kolejną (dostarczane z urządzeniem) na komputerze.

5. Konfigurowanie połączenia szeregowego w następujący sposób:
   - transmisji 115 200
   - 8 bitów danych
   - 1 bit zatrzymania
   - Nie spójności
   - **Brak** sterowania przepływem

6. Upewnij się, że połączenie działa przez naciśnięcie klawisza Enter w konsoli. Menu szeregowego Konsola powinien być wyświetlany.

> [AZURE.NOTE] **Zarządzania lights-Out:** Po zainstalowaniu urządzenia w zdalnego centrum danych lub w pokoju komputer z ograniczonym dostępem upewnij się, że połączenia szeregowe do obu kontrolerów są zawsze połączone szeregowego console switch lub podobne urządzenia. Dzięki temu w nowym oknie harmonogramem zdalnego sterowania i operacji pomocy technicznej w przypadku awarii sieci lub nieoczekiwane błędy.

Ukończono kable urządzenia w celu power, dostęp do sieci i szeregowo. Następnym krokiem jest skonfigurowanie oprogramowania na urządzeniu.

## <a name="next-steps"></a>Następne kroki

Teraz możesz przystąpić do [wdrożenia i skonfiguruj urządzenie StorSimple lokalnego](storsimple-deployment-walkthrough-u2.md).
