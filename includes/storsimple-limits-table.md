<!--author=alkohli last changed: 12/15/15-->

| Identyfikator limitu | Limit | Komentarze |
|----------------- | ------|--------- |
| Maksymalna liczba poświadczeń konta miejsca do magazynowania | 64 | |
| Maksymalna liczba kontenerów głośności | 64 | |
| Maksymalna liczba wielkości | 255 | |
| Maksymalna liczba harmonogramów w szablonie przepustowości | 168 | Harmonogram dla każdej godziny każdego dnia tygodnia (24 * 7). |
| Maksymalny rozmiar woluminu warstwowych na fizycznymi | 64 TB 8100 i 8600 | 8100 i 8600 są fizycznymi. |
| Maksymalny rozmiar woluminu warstwowych wirtualnego urządzenia platformy Azure | 30 TB dla 8010 <br></br> 64 TB dla 8020 | 8010 i 8020 są wirtualnych korzystające platformy Azure standardowego magazynu i miejsca do magazynowania Premium odpowiednio. |
| Maksymalny rozmiar woluminu lokalnie przypięty na fizycznymi | 9 TB dla 8100 <br></br> 24 TB dla 8600 | 8100 i 8600 są fizycznymi. |
| Maksymalna liczba połączeń iSCSI | 512 | |
| Maksymalna liczba połączeń iSCSI z inicjator | 512 | |
| Maksymalna liczba rekordów kontrolki programu access na urządzenie | 64 | |
| Maksymalna liczba wielkości na politykę wykonywania kopii zapasowych | 24 | |
| Maksymalna liczba kopii zapasowych zachowane na politykę wykonywania kopii zapasowych | 64 | |
| Maksymalna liczba harmonogramów na politykę wykonywania kopii zapasowych | 10 | |
| Maksymalna liczba migawek dowolnego typu, który może zostać zachowane objętości | 256 | Ta opcja uwzględnia lokalne migawek i migawki w chmurze. |
| Maksymalna liczba migawek, które mogą występować w dowolnym urządzeniu | 10 000 | |
| Maksymalna liczba wielkości, które mogą być przetwarzane równolegle dla kopii zapasowej, przywracanie lub klonowanie | 16 |<ul><li>Istnieje więcej niż 16 wielkości, będą one kolejno przetwarzane podczas przetwarzania gniazda stają się niedostępne.</li><li>Nowe kopie zapasowe sklonowanym lub przywrócenie ilością warstwowych nie można wykonać dopiero po zakończeniu operacji. Jednak woluminu lokalne kopie zapasowe są dozwolone po głośność jest w trybie online.</li></ul>|
| Przywracanie i klonowanie odzyskać czas wielkości warstwowych | < 2 minuty | <ul><li>Głośność jest udostępniana w ciągu dwóch minut operacji przywracania lub klonowanie, bez względu na rozmiar woluminu.</li><li>Wydajność woluminu początkowo może być mniejsza niż normalny, jak większość dane i metadane nadal znajdują się w chmurze. Jako przepływów danych z chmury na urządzeniu StorSimple może zwiększyć wydajność.</li><li>Całkowity czas, aby pobrać metadanych zależy od rozmiaru przydzielonego głośność. Metadane automatycznie zostanie przeniesiony do urządzenia w tle w wysokości 5 minut na TB danych przydzielonego głośność. Ten kurs może dotyczyć przepustowości internetowej w chmurze.</li><li>Przywróć lub operację na sklonuj jest wykonane, gdy wszystkie metadane jest na urządzeniu.</li><li>Nie można wykonać operacji kopii zapasowej do przywrócenia lub pełni zakończeniu operacji klonowanie.|
| Przywracanie odzyskiwanie terminu lokalnie przypięty wielkości | < 2 minuty | <ul><li>Głośność jest udostępniana w ciągu dwóch minut przywracanie, bez względu na rozmiar woluminu.</li><li>Wydajność woluminu początkowo może być mniejsza niż normalny, jak większość dane i metadane nadal znajdują się w chmurze. Jako przepływów danych z chmury na urządzeniu StorSimple może zwiększyć wydajność.</li><li>Całkowity czas, aby pobrać metadanych zależy od rozmiaru przydzielonego głośność. Metadane automatycznie zostanie przeniesiony do urządzenia w tle w wysokości 5 minut na TB danych przydzielonego głośność. Ten kurs może dotyczyć przepustowości internetowej w chmurze.</li><li>W odróżnieniu od wielkości warstwowych, w przypadku lokalnie przypięty ilości danych głośność jest również pobierane lokalnie na urządzeniu. Po wprowadzeniu wszystkich danych głośność na urządzeniu zakończeniu operacji przywracania.</li><li>Operacje przywracania może być długa i całkowity czas, aby ukończyć przywracanie zależy od rozmiaru ustanawianie obrót lokalne, przepustowość Internetu i istniejących danych na tym urządzeniu. Operacje wykonywania kopii zapasowych na lokalnie przypięty głośność są dozwolone w trakcie operacji przywracania.|
| Przywracanie cienkie dostępności | Ostatni pracy awaryjnej | |
| Maksymalna klienta przepustowość odczytu/zapisu (w przypadku obsługiwanych z warstwy SSD) * | 920/720 MB/s z jednym 10GbE interfejs sieciowy | Do 2 x z MPIO i dwóch interfejsów. |
| Maksymalna liczba klienta odczytu/zapisu przepustowość (w przypadku obsługiwanych z warstwy dysk twardy) * | 120-250 MB/s |
| Maksymalna klienta odczytu/zapisu przepustowość (w przypadku obsługiwanych z warstwy chmury) * | 11-41 MB/s | Przeczytaj przepustowość zależy od klientów generowania i utrzymywania wystarczających głębokość kolejki we/wy. |

& #42; Maksymalna przepustowość według typu we/wy pomiarów z w 100 procentach odczytu i zapisu w 100 procentach scenariuszy. Rzeczywista wydajność może być mniejsza i zależy od we/wy mieszać i sieci warunków.
