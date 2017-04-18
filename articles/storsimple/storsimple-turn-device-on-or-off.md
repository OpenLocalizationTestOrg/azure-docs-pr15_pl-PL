<properties 
   pageTitle="Włączanie urządzenia StorSimple lub wyłączanie | Microsoft Azure"
   description="Wyjaśniono, jak włączyć na nowym urządzeniu StorSimple, włączyć urządzenie, który został zamknięty lub utracone power i wyłączyć uruchomione urządzenie."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="08/23/2016"
   ms.author="alkohli" />

# <a name="turn-your-storsimple-device-on-or-off"></a>Włączanie urządzenia StorSimple lub wyłączanie 

## <a name="overview"></a>Omówienie

Zamykanie urządzenia Microsoft Azure StorSimple nie jest wymagane jako część normalnego działania. Jednak może być konieczne Włącz nowe urządzenie lub urządzenie, które miały zostać wyłączony. Ogólnie rzecz biorąc zamknięcie jest wymagane w przypadkach, w których trzeba zamienić wadliwy sprzęt, fizycznie Przenoszenie jednostki lub wykonać urządzenie działa. Ten samouczek w tym artykule opisano procedury wymagane włączenie i zamykanie urządzenia StorSimple w różnych scenariuszach.

W poniższej tabeli wymieniono różne scenariusze włączenie i zamykanie urządzenia StorSimple i znajdują się łącza do odpowiednich procedur.

|Scenariusz|Tematy dodatkowe|
|:-------|:---------------|
|Włączanie na nowym urządzeniu|[Włączanie na nowym urządzeniu](#turn-on-a-new-device)<ul><li>[Nowe urządzenie z tylko podstawowy załącznik](#new-device-with-primary-enclosure-only)</li><li>[Nowe urządzenie z EBOD załącznik](#new-device-with-ebod-enclosure)</li></ul>|
|Włączanie urządzenia po zamknięciu|[Włączanie urządzenia po zamknięciu](#turn-on-a-device-after-shutdown)<ul><li>[Urządzenie z tylko podstawowy załącznik](#device-with-primary-enclosure-only)</li><li>[Urządzenie z EBOD załącznik](#device-with-ebod-enclosure)</li></ul>|
|Włączanie urządzenia po utracie power|[Włączanie urządzenia po utracie power](#turn-on-a-device-after-a-power-loss)<ul><li>[Urządzenie z tylko podstawowy załącznik](#8100)</li><li>[Urządzenie z EBOD załącznik](#8600)</li></ul>|
|Włączanie urządzenia po załącznik podstawowego i EBOD połączenie zostanie utracone|[Włączanie urządzenia po podstawowych i EBOD załącznik połączenie zostanie utracone](#turn-on-a-device-after-the-primary-and-ebod-enclosure-connection-is-lost)|
|Zamknięcia uruchomionego urządzenia|[Wyłączanie uruchomionego urządzenia](#turn-off-a-running-device)<ul><li>[Urządzenie z tylko podstawowy załącznik](#8100a)</li><li>[Urządzenie z EBOD załącznik](#8600a)</li></ul>|

## <a name="turn-on-a-new-device"></a>Włączanie na nowym urządzeniu

Procedura włączania urządzenia StorSimple po raz pierwszy różni się w zależności od tego, czy urządzenie jest 8100 lub modelu 8600. 8100 zawiera pojedynczy załącznik podstawowego 8600 jest urządzenia załącznik narożnej z podstawowego załącznik i załącznika EBOD. Uzyskać szczegółowe instrukcje dotyczące obu modeli opisano w poniższych sekcjach.

- [Nowe urządzenie z tylko podstawowy załącznik](#new-device-with-primary-enclosure-only)

- [Nowe urządzenie z EBOD załącznik](#new-device-with-ebod-enclosure)

### <a name="new-device-with-primary-enclosure-only"></a>Nowe urządzenie z tylko podstawowy załącznik

Model StorSimple 8100 to urządzenie pojedynczy załącznik. Urządzenie zawiera zbędne dodatku i chłodzenia modułów (PCMs). Zarówno PCMs musi być zainstalowany i podłączony do różnych power źródeł zapewnienie wysokiej dostępności.

Wykonaj poniższe czynności, aby kabla urządzenia w celu power.

[AZURE.INCLUDE [storsimple-cable-8100-for-power](../../includes/storsimple-cable-8100-for-power.md)]

>[AZURE.NOTE]Instalator wykonane urządzenia i kable instrukcje przejdź do [zainstalowania urządzenia StorSimple 8100](storsimple-8100-hardware-installation.md). Upewnij się, wykonaj instrukcje wyświetlane dokładnie.

### <a name="new-device-with-ebod-enclosure"></a>Nowe urządzenie z EBOD załącznik

Model StorSimple 8600 zawiera zarówno podstawowy załącznik, jak i załącznik EBOD. Wymaga to jednostki, które można ze sobą kablem łączności szeregowego dołączone SCSI (SA) i power.

Podczas konfigurowania tego urządzenia po raz pierwszy, wykonaj czynności dla skojarzeń zabezpieczeń kable najpierw, a następnie wypełnij instrukcje dotyczące okablowania power.

[AZURE.INCLUDE [storsimple-sas-cable-8600](../../includes/storsimple-sas-cable-8600.md)]

[AZURE.INCLUDE [storsimple-cable-8600-for-power](../../includes/storsimple-cable-8600-for-power.md)]

>[AZURE.NOTE]Instalator wykonane urządzenia i kable instrukcje przejdź do [zainstalowania urządzenia StorSimple 8600](storsimple-8600-hardware-installation.md). Upewnij się, wykonaj instrukcje wyświetlane dokładnie.

## <a name="turn-on-a-device-after-shutdown"></a>Włączanie urządzenia po zamknięciu

Procedura włączania urządzenia StorSimple po jego zamknięciu różnią się w zależności od tego, czy urządzenie jest 8100 lub modelu 8600. 8100 zawiera pojedynczy załącznik podstawowego 8600 jest urządzenia załącznik narożnej z podstawowego załącznik i załącznika EBOD.

- [Urządzenie z tylko podstawowy załącznik](#device-with-primary-enclosure-only)

- [Urządzenie z EBOD załącznik](#device-with-ebod-enclosure)

### <a name="device-with-primary-enclosure-only"></a>Urządzenie z tylko podstawowy załącznik

Po zamknięciu za pomocą poniższej procedury na urządzeniu StorSimple z podstawowego załącznik, a nie załącznik EBOD.

#### <a name="to-turn-on-a-device-with-a-primary-enclosure-only"></a>Aby włączyć na urządzeniu z tylko podstawowy załącznik

1. Upewnij się, że power przełącza się na obu Power i chłodzenia moduły (PCMs) w pozycji wyłączone. Jeśli przełączniki nie znajdują się w pozycji wyłączone, przerzucić do pozycji wyłączone i poczekaj, aż nastąpi kontrolki.

2. Włącz urządzenie, przerzucanie przełączniki power na obu PCMs do pozycji Wł. Urządzenie należy włączyć.

3. Sprawdź poniższe czynności, aby sprawdzić, czy urządzenie jest w pełni na:

    1. LED OK w obu moduły PCM są zielone.

    2. Stan LED na obu kontrolerach są zielone.

    3. Miganie jest niebieski LED na jednym kontroler wskazuje, że kontroler jest aktywna.

    Jeśli dowolny z tych warunków nie jest spełniony, urządzenie nie działa prawidłowo. Zapoznaj [się z pomocą techniczną firmy Microsoft](storsimple-contact-microsoft-support.md).

### <a name="device-with-ebod-enclosure"></a>Urządzenie z EBOD załącznik

Po zamknięciu za pomocą poniższej procedury na urządzeniu StorSimple z podstawowego załącznik i załącznika EBOD. Wykonywanie każdego kroku w sekwencji dokładnie zgodnie z opisem. Błąd w tym celu może spowodować utratę danych.

#### <a name="to-turn-on-a-device-with-a-primary-and-an-ebod-enclosure"></a>Aby włączyć na urządzeniu z głównego i załącznika EBOD

1. Upewnij się, że załącznik EBOD jest połączony podstawowego załącznik. Aby uzyskać więcej informacji zobacz [Instalowanie urządzenia StorSimple 8600](storsimple-8600-hardware-installation.md).

2. Upewnij się, że Power i chłodzenia modułów (PCMs) na EBOD i załączniki podstawowego znajdują się w pozycji wyłączone. Jeśli przełączniki nie znajdują się w pozycji wyłączone, przerzucić do pozycji wyłączone i poczekaj, aż nastąpi kontrolki.

3. Włącz załącznik EBOD pierwszy, przerzucanie przełączniki power na obu PCMs do pozycji Wł. LED PCM powinny być zielone. Zielony kontroler EBOD LED tego elementu wskazuje, że EBOD znajduje się na.

4. Włącz podstawowego załącznik, przerzucanie przełączniki power na obu PCMs do pozycji Wł. Cały system będą znajdować się na.

5. Upewnij się, że LED skojarzenia zabezpieczeń są zielone, który sprawia, że połączenie między załącznik EBOD i załącznik podstawowy jest dobrym.

## <a name="turn-on-a-device-after-a-power-loss"></a>Włączanie urządzenia po utracie power

Awarii zasilania lub przerwania może wyłączyć urządzenie StorSimple. Awarii zasilania może wystąpić w jednej z dostaw power lub obu power materiałów eksploatacyjnych. Kroki odzyskiwania różnią się w zależności od tego, czy urządzenie jest model 8100 lub 8600. 8100 zawiera pojedynczy załącznik podstawowego 8600 jest urządzenia załącznik narożnej z podstawowego załącznik i załącznika EBOD. W tej sekcji opisano procedury odzyskiwania dla każdego scenariusza.

- [Urządzenie z tylko podstawowy załącznik](#8100)

- [Urządzenie z EBOD załącznik](#8600)

### <a name="device-with-primary-enclosure-only-a-name8100"></a>Urządzenie z tylko podstawowy załącznik<a name="8100">

System nadal jego normalnego działania po utracie power do jednego z jej źródła energii. Jednak w celu zapewnienia wysokiej dostępności urządzenia, przywrócenie power zasilania tak szybko, jak to możliwe.

Jeśli istnieje awarii zasilania lub awarii zasilania na obu power materiałów eksploatacyjnych, system zostanie zamknięty w sposób uporządkowany i kontroli. Po przywróceniu zasilania systemu spowoduje automatyczne włączenie.

### <a name="device-with-ebod-enclosure-a-name8600"></a>Urządzenie z EBOD załącznik<a name="8600">

#### <a name="power-loss-on-one-power-supply"></a>Utrata Power na jednym power dostaw

System nadal jego normalnego działania po utracie power do jednego z jej źródła energii na podstawowy załącznik lub załącznik EBOD. Jednak w celu zapewnienia wysokiej dostępności urządzenia, Przywróć power do zasilania tak szybko, jak to możliwe.

#### <a name="power-loss-on-both-power-supplies-on-primary-and-ebod-enclosures"></a>Utrata Power na obu power dostawy na podstawowy i załączniki EBOD

Jeśli istnieje awarii zasilania lub awarii zasilania na obu power materiałów eksploatacyjnych, załącznik EBOD wyłączy natychmiast i podstawowego załącznik zostanie zamknięty w sposób uporządkowany i kontroli. Po przywróceniu zasilania urządzenia zostanie uruchomiony automatycznie.

Jeśli power jest wyłączany ręcznie, a następnie wykonaj następujące czynności, aby przywrócić power system.

1. Włącz załącznik EBOD.

2. Po włączeniu załącznik EBOD włączyć podstawowego załącznik.

### <a name="power-loss-on-both-power-supplies-on-ebod-enclosure"></a>Utrata Power na obu power dostawy na EBOD załącznik

Po skonfigurowaniu kable należy się upewnić, EBOD nigdy nie samodzielnie podłączony do oddzielnych PDU. Jeśli EBOD i załącznik podstawowego nie w tym samym czasie, system będzie odzyskać.

Jeśli tylko załącznik EBOD nie powiedzie się w obu power materiałów eksploatacyjnych, system nie zostanie automatycznie odzyskać. Wykonaj poniższe czynności, aby włączyć system i przywrócenie jego prawidłowy stan:

1. Jeśli podstawowy załącznik jest włączona, wyłącz zarówno dodatku i chłodzenia modułów (PCMs).

2. Poczekaj kilka minut na system w celu zamknięcia.

3. Włącz załącznik EBOD.

4. Po włączeniu załącznik EBOD włączyć podstawowego załącznik.

## <a name="turn-on-a-device-after-the-primary-and-ebod-enclosure-connection-is-lost"></a>Włączanie urządzenia po podstawowych i EBOD załącznik połączenie zostanie utracone

Jeśli połączenie zostanie utracony między wstrzymania kontroler i odpowiadające im kontroler EBOD, urządzenie będzie nadal działać. W przypadku utraty połączenia między kontrolera active systemu i odpowiadające im kontroler EBOD pracy awaryjnej powinna zostać wykonana i urządzenie należy kontynuować pracę w normalny sposób.

Gdy oba kable szeregowe dołączone SCSI (SA) są usuwane lub połączenie między załącznik EBOD i załącznik podstawowego są oddzielone, urządzenie przestanie działać. W tym momencie należy wykonać następujące czynności.

### <a name="to-turn-on-the-device-after-connection-is-lost"></a>Aby włączyć na tym urządzeniu, gdy połączenie zostanie utracone

1. Dostęp do tyłu urządzenia.

2. Jeśli połączenie kabla skojarzeń zabezpieczeń między załącznik EBOD i podstawowego załącznik zostanie zerwane, wszystkie skojarzenia zabezpieczeń toru LED na załącznik EBOD będzie wyłączone.

3. Zamknij zarówno dodatku i chłodzenia modułów (PCMs) na załącznik EBOD i podstawowego załącznik.

4. Poczekaj, aż Wyłącz wszystkie kontrolki z tyłu obu załączniki.

5. Włóż kable skojarzeń zabezpieczeń i upewnij się, że jest połączenie między załącznik EBOD i podstawowego załącznik.

6. Włącz załącznik EBOD pierwszy, przerzucanie oba przełączniki PCM do pozycji Wł.

7. Upewnij się, załącznik EBOD włączeniu, zaznaczając pole wyboru, aby zielona LED jest włączone.

8. Włącz funkcję podstawowego załącznik.

9. Upewnij się, jest podstawowy załącznik na, zaznaczając pole wyboru czy LED kontroler zielony jest włączone.

10. Sprawdź, czy połączenie EBOD załącznik z podstawowego załącznik jest dobrym, zaznaczając pole wyboru czy toru skojarzeń zabezpieczeń LED (cztery na kontrolerze EBOD) są włączone wszystkie.

>[AZURE.IMPORTANT] Jeśli połączenie między załącznik EBOD i podstawowego załącznik jest metoda, gdy włączysz w systemie kable skojarzeń zabezpieczeń jest uszkodzony, przejdzie on w trybie odzyskiwania. W takim przypadku zapoznaj [skontaktuj się z pomocą techniczną firmy Microsoft](storsimple-contact-microsoft-support.md) .

## <a name="turn-off-a-running-device"></a>Wyłączanie uruchomionego urządzenia

Uruchomione urządzenia StorSimple może być konieczne zamknięcie przenoszony jest wykonywane wylogowywanie się z usługi, lub składnik nieprawidłowo, który ma zostać zastąpiona. Czynności są różne w zależności od tego, czy urządzenia StorSimple jest 8100 lub modelu 8600. 8100 zawiera pojedynczy załącznik podstawowego 8600 jest urządzenia załącznik narożnej z podstawowego załącznik i załącznika EBOD. W tej sekcji opisano kroki w celu zamknięcia uruchomionego urządzenia.

- [Urządzenie z podstawowego załącznik](#8100a)

- [Urządzenie z EBOD załącznik](#8600a)

### <a name="device-with-primary-enclosure-a-name8100a"></a>Urządzenie z podstawowego załącznik<a name="8100a"> 

Zamykanie urządzenia w sposób uporządkowany i kontroli, możesz zrobić to za pośrednictwem portalu klasyczny Azure lub za pośrednictwem programu Windows PowerShell dla StorSimple. 

>[AZURE.IMPORTANT] Nie zamknięty uruchomionego urządzenia za pomocą przycisku power z tyłu urządzenia.
>
>Przed zamknięciem urządzenia, upewnij się, że wszystkie składniki urządzenie jest prawidłowy. W portalu klasyczny Azure, przejdź do **urządzenia** > **Konserwacja** > **Stan sprzętu**i sprawdź, czy stan wszystkich elementów jest zielony. Dotyczy to tylko w przypadku prawidłowy system. System jest zamknięty w celu Zamień składnik nieprawidłowo, zostanie wyświetlony nie powiodło się (czerwony) lub obniżonej wydajności (żółte) stan składnika odpowiednich **Stanu sprzętu**.

Po uzyskaniu dostępu programu Windows PowerShell dla StorSimple lub portalu klasyczny Azure, postępuj zgodnie z instrukcjami [zamknięcia urządzenia StorSimple](storsimple-manage-device-controller.md#shut-down-a-storsimple-device). 

### <a name="device-with-ebod-enclosure-a-name8600a"></a>Urządzenie z EBOD załącznik<a name="8600a">

>[AZURE.IMPORTANT] Przed zamknięciem załącznik podstawowego i załącznik EBOD, upewnij się, czy wszystkie składniki urządzenia są prawidłowy. W portalu klasyczny Azure, przejdź do **urządzenia** > **Konserwacja** > **Stan sprzętu**i sprawdź, czy wszystkie składniki prawidłowy.

#### <a name="to-shut-down-a-running-device-with-ebod-enclosure"></a>Aby zamknąć uruchomionego urządzenia z EBOD załącznik

1. Wykonaj wszystkie kroki wymienione w [zamknięcia urządzenia StorSimple](storsimple-manage-device-controller.md#shut-down-a-storsimple-device) dla podstawowego załącznik.

2. Po zamknięciu podstawowego załącznik zamknąć EBOD Przerzucanie wyłączanie obu Power i przełącza chłodzenia Moduł PCM ().

3. Aby sprawdzić, czy EBOD została zamknięta, sprawdź, czy wszystkie światła z tyłu załącznik EBOD są wyłączone.

>[AZURE.NOTE] Kable skojarzenia zabezpieczeń, które są używane do łączenia załącznik EBOD do podstawowego załącznik nie powinny być usuwane aż po zamknięciu systemu.

## <a name="next-steps"></a>Następne kroki

[Kontakt z pomocą techniczną firmy Microsoft](storsimple-contact-microsoft-support.md) w razie wystąpienia problemów podczas włączania lub wyłączania urządzenia StorSimple.

