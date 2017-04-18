<properties
   pageTitle="Instalowanie urządzenia StorSimple 8100 | Microsoft Azure"
   description="Informacje dotyczące Rozpakowywanie, stojaków instalacji i kabla urządzenia StorSimple 8100 przed wdrażanie i skonfigurować oprogramowanie."
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
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="unpack-rack-mount-and-cable-your-storsimple-8100-device"></a>Rozpakowywanie, Instalacja stojaków, a kabel urządzenia StorSimple 8100

## <a name="overview"></a>Omówienie

Usługi Microsoft Azure StorSimple 8100 jest zainstalowany stojaków urządzenia, pojedynczy załącznik. Ten samouczek wyjaśniono, jak Rozpakowywanie, stojaków i urządzenie kabla StorSimple 8100 przed skonfigurowaniem i wdrażanie urządzenia StorSimple.

## <a name="unpack-your-storsimple-8100-device"></a>Rozpakowywanie urządzenia StorSimple 8100

Poniższe czynności stanowią Wyczyść, szczegółowe instrukcje dotyczące Rozpakowywanie StorSimple 8100 urządzenia miejsca do magazynowania. To urządzenie jest dostarczany w jednym polu.

### <a name="prepare-to-unpack-your-device"></a>Przygotowywanie się do Rozpakowywanie urządzenia

Przed Rozpakowywanie urządzenia, przejrzyj poniższe informacje.

![Ikona ostrzeżenia](./media/storsimple-safety/IC740879.png)![ikona intensywnie grubość](./media/storsimple-8100-hardware-installation/HCS_HeavyWeight_Icon.png) **Ostrzeżenie!**

1. Upewnij się, że masz dwie osoby dostępny do zarządzania grubości załącznik, jeśli są obsługuje je ręcznie. W pełni skonfigurowane załącznik można oceny do 32 kg (70 kg.).
1. Umieść pole powierzchni płaskiej, poziomu.

Następnie wykonaj następujące czynności, aby Rozpakowywanie urządzenia.

#### <a name="to-unpack-your-device"></a>Aby Rozpakowywanie urządzenia

1. Sprawdź, czy pole i pianki opakowań crushes, części, uszkodzenia wody lub oczywistych uszkodzeń. Jeśli pole lub opakowanie poważnie jest uszkodzony, nie można otwierać okna. Zapoznaj [się z pomocą techniczną firmy Microsoft](storsimple-contact-microsoft-support.md) ułatwiające sprawdzenia, czy urządzenie znajduje się w dobrym stanie.

2. Rozpakowywanie pole. Poniższa ilustracja przedstawia widoku rozpakowane urządzenia StorSimple.

     ![Rozpakowywanie urządzenia miejsca do magazynowania](./media/storsimple-8100-hardware-installation/HCSUnpackyour2Udevice.png)

    **Widok rozpakowane urządzenia miejsca do magazynowania**

     Etykieta | Opis
     ----- | -------------
     1     | Pole dokumentu
     2     | Pianki dołu
     3     | Urządzenie
     4     | Górny pianki
     5     | Pole akcesoriów


3. Po Rozpakowywanie okna, upewnij się, że masz:

   - 1 załącznik pojedynczego urządzenia
   - 2 kable
   - 1 krzyżowym kabla Ethernet
   - 2 kable konsoli szeregowego
   - 1 Konwerter USB kolejną dostępu szeregowego
   - 1 śrubokrętu T10 odporne
   - 4 QSFP-do-SFP + kart do użytku z 10 GbE interfejsów
   - 1 stojaków instalacji kit (2 strony szyny z instalowanie sprzętu)
   - Uzyskiwanie dokumentacji wprowadzenie

    Jeśli nie masz każdy z elementów wymienionych powyżej, [skontaktuj się z pomocą techniczną firmy Microsoft](storsimple-contact-microsoft-support.md).

Następnym krokiem jest stojaków instalacji urządzenia.

## <a name="rack-mount-your-storsimple-8100-device"></a>Stojaków instalacji urządzenia StorSimple 8100

Wykonaj poniższe czynności, aby zainstalować urządzenia StorSimple 8100 miejsca do magazynowania w standardowej stojaków CAL 19 z przedniej i tylnej wpisów. Urządzenie StorSimple 8100 ma pojedynczy załącznik podstawowego.

Instalacja składa się z wielu kroków, z których każdy będzie został omówiony w następujących procedur.

> [AZURE.IMPORTANT]
Urządzenia StorSimple musi być zainstalowany stojaków prawidłowe działanie.

### <a name="prepare-the-site"></a>Przygotowywanie witryny

Urządzenie musi być zainstalowany w standardowej stojaków 19 cali, zarówno przedniej i tylnej wpisów. Poniższa procedura umożliwia przygotowanie do instalacji stojaków.

#### <a name="to-prepare-the-site-for-rack-installation"></a>Aby przygotować witryny instalacji stojaków

1. Upewnij się, że urządzenie bezpiecznie spoczywa na prostym, stabilny i poziomu pracy powierzchniowy (lub podobnych).

2. Sprawdź, czy witryny, gdy chcesz skonfigurować ma standardowy power AC z niezależne źródło lub stojaków jednostki dystrybucyjnej power (PDU) z dostarczanego awaryjny (UPS).

3. Upewnij się, że ten jeden przedział 2U jest dostępna na stojaków, w którym chcesz zainstalować urządzenie.

![Ikona ostrzeżenia](./media/storsimple-safety/IC740879.png)![ikona intensywnie grubość](./media/storsimple-8100-hardware-installation/HCS_HeavyWeight_Icon.png) **Ostrzeżenie!**

Upewnij się, że masz dwie osoby dostępny do zarządzania grubość, jeśli Instalator urządzenia są obsługuje ręcznie. W pełni skonfigurowane załącznik można oceny do 32 kg (70 kg.).

### <a name="rack-prerequisites"></a>Wymagania wstępne dotyczące stojaków

Załącznik 8100 jest przeznaczona do instalowania w standardowej stojaków 19 cali szafka z:

- Minimalna głębokość 27.84 cala z stojaków wpis do wpisu.
- Ciężar maksymalny 32 kg urządzenia
- Maksymalna liczba ciśnienia wstecz Pascala 5 (0,5 mm słupa wody).

### <a name="rack-mounting-rail-kit"></a>Instalowanie stojaków kolei kit

Ustawianie zainstalowanie szyn jest przeznaczony dla reprezentujący stojaków 19 cali. Szyny przetestowano do obsługi grubość maksymalna załącznik. Te szyny umożliwiają również instalację wiele załączników bez utraty obszaru stojaków.


#### <a name="to-install-the-device-on-the-rails"></a>Aby zainstalować urządzenie przy użyciu prowadnic

2. Tylko, jeśli wewnętrzne szyny nie są zainstalowane na urządzeniu, należy wykonać ten krok. Zazwyczaj szyn wewnętrzne są instalowane fabryczne. Jeśli nie zainstalowano szyny, zainstaluj kolei od lewej i prawej kolei slajdów na stronach obudowy załącznik. Dołącz ich za pomocą sześć śruby metryczne na każdej stronie. Ułatwiające orientacji slajdów kolei są oznaczone **LH — wierzch** i **RH — wierzch**i zakończenia, który jest dołączony do tyłu załącznik ma końcem.<br/>

    ![Dołączanie slajdów kolei podwoziem załącznik](./media/storsimple-8100-hardware-installation/HCSAttachingRailSlidestoEnclosureChassis.png)
    **Attaching rail wewnętrzne slajdów do boków załącznik**

       Etykieta | Opis
       ----- | -----------
       1     | M 3 x 4 śruby głowy przycisk
       2     | Obudowa slajdów

3. Dołącz zewnętrzne kolei po lewej stronie i zestawy zewnętrzne prawej stronie członkom stojaków szafka pionowy. Nawiasy są oznaczone **LH** **RH**i **Ta strona w górę** do przeprowadzenia właściwą orientacją.

4. Znajdź kolei Sworznie do przodu i do tyłu zestawu kolei. Rozszerzanie rail, aby zmieścił się między wpisy stojaków i wstawić numery PIN do przedniej i tylnej stojaków wpis element pionowy dziury. Upewnij się, że zestawu kolei jest poziom.

5. Za pomocą dwóch podanych śruby metryczne zabezpieczania zestawu kolei do szafy pionowa członków. Za pomocą jednego śruby na wierzch i jeden z tyłu.

6. Powtórz te czynności dla zestawu kolei.<br/>

     ![Dołączanie kolei slajdów do stojaków szafka](./media/storsimple-8100-hardware-installation/HCSAttachingRailSlidestoRackCabinet.png)

    **Dołączanie zestawów kolei zewnętrzne do szafy**

     Etykieta | Opis
     ----- | -----------
     1     | Zaciskowe śruby
     2     | Śruby wpis stojaków przedniej otworu kwadratowy
     3     | Po lewej stronie kolei przedniej lokalizacji PIN
     4     | Zaciskowe śruby
     5     | Po lewej stronie kolei tylnej lokalizacji PIN


### <a name="mounting-the-device-in-the-rack"></a>Instalowanie urządzenia w stojaków

Przy użyciu stojaku, po prostu zainstalowane, wykonaj następujące czynności do zainstalowania urządzenia w stojaków.

#### <a name="to-mount-the-device"></a>Aby zainstalować urządzenie

1. Asystent Podnieś załącznik i go wyrównać z stojaku.

2. Starannie Włóż urządzenie do szyny, a następnie push urządzenie całkowicie do szafy cab.<br/>

    ![Wstawianie urządzenia stojaków](./media/storsimple-8100-hardware-installation/HCSInsertingDeviceintheRack.png)

    **Instalowanie urządzenia w stojaków**


3. Usuwanie caps kołnierza lewy i prawy przedniej przenosząc naciśnij klawisze Caps Lock bezpłatne. Naciśnij klawisze caps kołnierza po prostu przyciąganie na stopka.

5. Bezpieczny załącznik w szafy instalując jeden dostarczonych śruby głowy Phillips za pośrednictwem każdego kołnierza, lewy i prawy.

4. Zainstaluj caps kołnierza, naciskając je w odpowiednim miejscu i przyciąganie ich w miejscu.<br/>

     ![Instalowanie kołnierza naciśnij klawisze Caps Lock](./media/storsimple-8100-hardware-installation/HCSInstallingFlangeCaps.png)

    **Instalowanie caps kołnierza**

     Etykieta | Opis
     ----- | -----------
     1     | Śruby zamknięcie załącznik

Następnym krokiem jest kabel urządzenia w celu power, sieci i szeregowego programu access.

## <a name="cable-your-storsimple-8100-device"></a>Kabel urządzenia StorSimple 8100

Poniższe procedury wyjaśniono, jak kabla StorSimple 8100 urządzenia w celu power, sieci i połączenia szeregowe.

### <a name="prerequisites"></a>Wymagania wstępne

Przed rozpoczęciem kable urządzenia, należy:

- Urządzenia magazynowania, całkowicie rozpakowane i stojaków instalowane.

- 2 kable power, dostarczone z urządzeniem

- Dostęp do 2 jednostki dystrybucji zasilania (zalecane).

- Kable sieciowe

- Opisane kable szeregowe

- Liczba kolejna Konwerter USB z odpowiedni sterownik zainstalowane na komputerze PC (w razie potrzeby)

- Opisane 4 QSFP-do-SFP + kart do użytku z 10 GbE interfejsów

- [Obsługa 10 interfejsów sieciowych GbE na urządzeniu z systemem StorSimple sprzętu](storsimple-supported-hardware-for-10-gbe-network-interfaces.md)


### <a name="power-cabling"></a>Power okablowania

Urządzenie zawiera zbędne dodatku i chłodzenia modułów (PCMs). Zarówno PCMs musi być zainstalowany i podłączony do różnych power źródeł zapewnienie wysokiej dostępności.

Wykonaj poniższe czynności, aby kabla urządzenia w celu power.

[AZURE.INCLUDE [storsimple-cable-8100-for-power](../../includes/storsimple-cable-8100-for-power.md)]

### <a name="network-cabling"></a>Kable sieciowe

Urządzenie jest aktywne gotowości konfiguracji: w dowolnym momencie jeden moduł kontrolera jest aktywna, a przetwarzanie wszystkie operacje dysku i sieci podczas moduł kontroler jest w stanie wstrzymania. Jeśli kontroler nie powiedzie się, wstrzymania kontroler natychmiast aktywowany i będzie nadal występował dysku i sieci operacji.

Do obsługi tej pracy awaryjnej zbędne kontrolerze, musisz kabla sieci urządzenia, zgodnie z opisem w poniższej procedurze.

#### <a name="to-cable-for-network-connection"></a>Do kabla dla połączenia sieciowego

1. Urządzenie ma sześć interfejsów sieciowych na każdym kontrolerze: cztery 1 GB i dwóch 10 GB Ethernet portów. Identyfikowanie różnych portów danych tablica połączeń urządzenia.

    ![Tablica połączeń 8100 urządzenia](./media/storsimple-8100-hardware-installation/HCSBackplaneof2UDevicewithPortsLabeled.jpg)

    **Ponownie urządzeń przedstawiający porty danych**

     Etykieta   | Opis
     ------- | -----------
     0,1,4,5 |  1 GbE interfejsów
     wartości 2 3     | 10 GbE interfejsów
     6       | Porty szeregowe

2. Zobacz poniższy diagram dla kable sieciowe. (Konfiguracja sieci minimalne znajduje się niebieską linią ciągłą. Dodatkowa konfiguracja wymagane wysokiej dostępności i wydajności znajduje się za pomocą linii kropkowanych).


    ![Kabel urządzenia 2U dla sieci](./media/storsimple-8100-hardware-installation/HCSCableYour2UDeviceforNetwork.png)

    **Okablowania dla swojego urządzenia sieciowego**


  	|Etykieta | Opis |
  	|----- | ----------- |
  	| A    | Sieci LAN z dostępem do Internetu |
  	| B    | Kontroler 0 |
  	| C    | PCM 0 |
  	| D    | Kontroler 1 |
  	| E    | PCM 1 |
  	| F, G | Hosts |
  	| 0-5  | Interfejsy |



Gdy kable urządzenia, minimalna konfiguracja wymaga:


- Co najmniej dwóch interfejsów połączony na każdym kontrolerze z jedną dostęp w chmurze i jedną dla iSCSI. DANE 0 port jest automatycznie włączone i skonfigurowane za pomocą konsoli szeregowego urządzenia. Niezależnie od danych 0 innego portu danych należy również można skonfigurować za pośrednictwem portalu klasyczny Azure. W tym przypadku łączenie danych 0 portu podstawowego sieci LAN (sieć z dostępem do Internetu). Inne porty danych mogą być połączone z SAN-iSCSI sieci LAN (VLAN) część sieci, w zależności od ma pełnić.

- Identyczne interfejsów na każdym kontrolerze podłączony do tej samej sieci w celu zapewnienia dostępności, jeśli awaria kontroler wystąpiła. Na przykład jeśli wybierzesz połączenie danych 0 i dane 3 dla jednej kontroler, musisz połączyć odpowiadające im dane 0 i 3 danych na innym kontrolerze.

Pamiętaj o wysokiej dostępności i wydajności:


- Jeśli to możliwe, skonfiguruj para sieciowej dostęp w chmurze (1 GbE) i inną parę iSCSI (10 GbE polecane) na każdym kontrolerze.

- Jeśli to możliwe, Połącz interfejsy z każdego kontrolera do dwóch różnych przełączników w celu zapewnienia dostępności przed awariami Przełącz. Na rysunku pokazano dwa 10 GbE interfejsy sieciowe, dane 2 i 3 dane z każdego kontrolera podłączone do dwóch różnych przełączników.

Aby uzyskać więcej informacji zapoznaj się z **interfejsów** w obszarze [wymagania wysoką dostępność dla swojego urządzenia StorSimple](storsimple-system-requirements.md#high-availability-requirements-for-storsimple).

>[AZURE.NOTE] Jeśli używasz odbiorcze SFP + z 10 GbE interfejsów sieciowych, za pomocą dostarczonych QSFP-SFP + karty. Aby uzyskać więcej informacji przejdź do [sprzętu obsługiwane 10 interfejsów sieciowych GbE na urządzeniu StorSimple](storsimple-supported-hardware-for-10-gbe-network-interfaces.md).



### <a name="serial-port-cabling"></a>Kable portu szeregowego

Wykonaj poniższe czynności, aby kabla portu szeregowego.

#### <a name="to-cable-for-serial-connection"></a>Do kabla dla połączenia szeregowego

1. Urządzenie ma portu szeregowego na każdym kontrolerze, które są oznaczane ikoną klucza. Zapoznaj się z ilustracją w sekcji [kable sieciowe](#network-cabling) , aby zlokalizować portów szeregowych tablica połączeń urządzenia.

2. Zidentyfikuj kontroler aktywne na tablica połączeń z urządzenia. Migający LED niebieski wskazuje, że kontroler jest aktywna.

3. Użyj dostarczonych kable szeregowe (w razie potrzeby konwerter kolejną USB komputera przenośnego) i połączyć konsoli lub komputer (z końcowych emulacji na urządzeniu) do portu szeregowego kontrolera active.

4. Instalowanie sterowników USB kolejną (dostarczane z urządzeniem) na komputerze.

5. Konfigurowanie połączenia szeregowego w następujący sposób: transmisji 115 200, 8 bitów danych, 1 bit zatrzymania, nie spójności i brak sterowania przepływem.

6. Upewnij się, że połączenie działa przez naciśnięcie klawisza Enter w konsoli. Menu szeregowego Konsola powinien być wyświetlany.

>[AZURE.NOTE] **Zarządzanie lights-Out**: po zainstalowaniu urządzenia w zdalnego centrum danych lub w pokoju komputer z ograniczonym dostępem upewnij się, że połączenia szeregowe do obu kontrolerów są zawsze połączone szeregowego console switch lub podobne urządzenia. Dzięki temu w nowym oknie harmonogramem zdalnego sterowania i operacji pomocy technicznej w przypadku zakłócenia sieci lub nieoczekiwane błędy.

Urządzenie jest teraz kablem power, dostęp do sieci i szeregowego łączności. Następnym krokiem jest konfigurowanie oprogramowania i wdrażanie urządzenia.

## <a name="next-steps"></a>Następne kroki

Dowiedz się, jak [wdrażać i skonfiguruj urządzenie StorSimple lokalnego](storsimple-deployment-walkthrough.md).
