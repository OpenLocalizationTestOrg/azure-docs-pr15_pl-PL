<properties 
   pageTitle="Limity system StorSimple | Microsoft Azure"
   description="W tym artykule opisano ograniczenia systemu i polecane rozmiarów składniki serii StorSimple 8000 i połączenia."
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
   ms.date="10/06/2016"
   ms.author="alkohli" />

# <a name="storsimple-system-limits"></a>Limity systemu StorSimple

## <a name="overview"></a>Omówienie

StorSimple zapewnia skalowalna i elastyczne magazyn dla centrum danych. Istnieją jednak pewne ograniczenia, które należy mieć na uwadze podczas planowania, wdrażania i działania rozwiązania StorSimple. Poniższej tabeli opisano limity te i udostępnia kilka zaleceń, tak aby użytkownik może w pełni wykorzystać rozwiązania StorSimple.

| Identyfikator limitu | Limit | Komentarze |
|----------------- | ------|--------- |
| Maksymalna liczba poświadczeń konta miejsca do magazynowania | 64 | |
| Maksymalna liczba kontenerów głośności | 64 | |
| Maksymalna liczba wielkości | 255 | |
| Maksymalna liczba lokalnie przypięty wielkości | 32 | |
| Maksymalna liczba harmonogramów w szablonie przepustowości | 168 | Harmonogram dla każdej godziny każdego dnia tygodnia (24 * 7). |
| Maksymalny rozmiar woluminu warstwowych na urządzeniach fizycznej | 64 TB 8100 i 8600 | 8100 i 8600 są fizycznymi. |
| Maksymalny rozmiar woluminu warstwowych wirtualnego urządzenia platformy Azure | 30 TB dla 8010 <br></br> 64 TB dla 8020 | 8010 i 8020 są wirtualnych korzystające platformy Azure standardowego magazynu i miejsca do magazynowania Premium odpowiednio. |
| Maksymalny rozmiar woluminu lokalnie przypięty na fizycznymi | 8,5 TB dla 8100 <br></br> 22,5 TB dla 8600 | 8100 i 8600 są fizycznymi. |
| Maksymalna liczba połączeń iSCSI | 512 | |
| Maksymalna liczba połączeń iSCSI z inicjator | 512 | |
| Maksymalna liczba rekordów kontrolki programu access na urządzenie | 64 | |
| Maksymalna liczba wielkości na politykę wykonywania kopii zapasowych | 24 | |
| Maksymalna liczba kopii zapasowych zachowane według harmonogramu (w zasady tworzenia kopii zapasowych) | 64 | |
| Maksymalna liczba harmonogramów na politykę wykonywania kopii zapasowych | 10 | |
| Maksymalna liczba migawek dowolnego typu, który może zostać zachowane objętości | 256 | Numer zawiera lokalne migawek i migawki w chmurze. |
| Maksymalna liczba migawek, które mogą występować w dowolnym urządzeniu | 10 000 | |
| Maksymalna liczba wielkości, które mogą być przetwarzane równolegle dla kopii zapasowej, przywracanie lub klonowanie | 16 |<ul><li>Jeśli istnieje więcej niż 16 wielkości, są przetwarzane kolejno podczas przetwarzania gniazda staną się dostępne.</li><li>Nowe kopie zapasowe sklonowanym lub przywrócenie ilością warstwowych nie można wykonać dopiero po zakończeniu operacji. Jednak woluminu lokalne kopie zapasowe są dozwolone po głośność jest w trybie online.</li></ul>|
| Przywracanie i klonowanie odzyskać czas wielkości warstwowych | < 2 minuty | <ul><li>Głośność jest udostępniana w ciągu dwóch minut operacji przywracania lub klonowanie, bez względu na rozmiar woluminu.</li><li>Wydajność woluminu początkowo może być mniejsza niż normalny, jak większość dane i metadane nadal znajdują się w chmurze. Jako przepływów danych z chmury na urządzeniu StorSimple może zwiększyć wydajność.</li><li>Całkowity czas, aby pobrać metadanych zależy od rozmiaru przydzielonego głośność. Metadane automatycznie zostanie przeniesiony do urządzenia w tle w wysokości 5 minut na TB danych przydzielonego głośność. Ten kurs może dotyczyć przepustowości internetowej w chmurze.</li><li>Przywróć lub operację na sklonuj jest wykonane, gdy wszystkie metadane jest na urządzeniu.</li><li>Nie można wykonać operacji kopii zapasowej do przywrócenia lub pełni zakończeniu operacji klonowanie.|
| Przywracanie odzyskiwanie terminu lokalnie przypięty wielkości | < 2 minuty | <ul><li>Głośność jest udostępniana w ciągu dwóch minut przywracanie, bez względu na rozmiar woluminu.</li><li>Wydajność woluminu początkowo może być mniejsza niż normalny, jak większość dane i metadane nadal znajdują się w chmurze. Jako przepływów danych z chmury na urządzeniu StorSimple może zwiększyć wydajność.</li><li>Całkowity czas, aby pobrać metadanych zależy od rozmiaru przydzielonego głośność. Metadane automatycznie zostanie przeniesiony do urządzenia w tle w wysokości 5 minut na TB danych przydzielonego głośność. Ten kurs może dotyczyć przepustowości internetowej w chmurze.</li><li>W odróżnieniu od wielkości warstwowych lokalnie przypięty ilości danych głośność jest również pobierane lokalnie na urządzeniu. Po wprowadzeniu wszystkich danych głośność na urządzeniu zakończeniu operacji przywracania.</li><li>Operacje przywracania może być długa. Całkowity czas, aby ukończyć przywracanie zależy od rozmiaru ustanawianie obrót lokalne, przepustowość Internetu i istniejących danych na tym urządzeniu. Operacje wykonywania kopii zapasowych na lokalnie przypięty głośność są dozwolone w trakcie operacji przywracania.|
|Przetwarzanie kurs migawek chmury| 15 minut i TB | <ul><li>Minimalny czas, aby wprowadzić w chmurze migawki gotowy do przekazania na TB danych przydzielonego głośności w kopii zapasowej. </li><li> Chmury całkowity czas migawki jest obliczany przez dodanie tej chwili na czas przekazywania migawki, którego dotyczy przepustowości internetowej do chmury.
| Maksymalna klienta odczytu/zapisu przepustowość (w przypadku obsługiwanych z warstwy SSD) * | 920/720 MB/s przy użyciu jednego interfejsu sieci 10 GbE | Do 2 x z MPIO i dwóch interfejsów. |
| Maksymalna liczba klienta odczytu/zapisu przepustowość (w przypadku obsługiwanych z warstwy dysk twardy) * | 120-250 MB/s |
| Maksymalna liczba klienta odczytu/zapisu przepustowość (w przypadku obsługiwanych z warstwy chmury)* dla aktualizacji 3 lub w nowszej wersji** | 40-60 MB/s służący do tiered wielkości<br><br>60-80 MB/s służący do tiered wielkości archiwizacji opcja jest zaznaczona, podczas tworzenia głośności | Przeczytaj przepustowość zależy od klientów generowania i utrzymywania wystarczających głębokość kolejki we/wy. <br><br>Szybkość osiągnięta zależy od szybkości podstawowej konto miejsca do magazynowania używane. | 

& #42; Maksymalna przepustowość według typu we/wy był mierzony w 100 procentach odczytu i zapisu w 100 procentach scenariusze. Rzeczywista wydajność może być mniejsza i zależy od we/wy mieszać i sieci warunków.

& #42; & #42; Numery wydajności przed aktualizacji 3 może być niższa.

## <a name="next-steps"></a>Następne kroki

Sprawdzanie [wymagań systemowych StorSimple](storsimple-system-requirements.md). 
