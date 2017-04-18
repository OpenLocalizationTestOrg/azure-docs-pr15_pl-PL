<properties 
   pageTitle="Wymiana składników sprzętu StorSimple | Microsoft Azure"
   description="W tym artykule opisano, jak bezpiecznie zastąpić PCMs, baterii, moduły kontrolerze, EBOD kontrolerów, dysków i obudowa urządzenia StorSimple."
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
   ms.date="10/11/2016"
   ms.author="alkohli" />

# <a name="storsimple-hardware-component-replacement"></a>Wymiana składników sprzętu StorSimple

## <a name="overview"></a>Omówienie

Wymiana składnik samouczki opisano składniki sprzętu urządzenia serii Microsoft Azure StorSimple 8000 i kroki niezbędne do usunięcia i zastąpić je. W tym artykule opisano ikony bezpieczeństwa, zawiera łącza do szczegółowych samouczki i składniki, które są zastąpienia.

>[AZURE.IMPORTANT] Przed podjęciem próby usunięcia lub zastąpienia każdego składnika StorSimple, upewnij się, możesz przejrzeć [Konwencje ikona bezpieczeństwa](#safety-icon-conventions) i inne [środki ostrożności](storsimple-safety.md).
 
### <a name="safety-icon-conventions"></a>Konwencje ikona bezpieczeństwa

W poniższej tabeli opisano ikony bezpieczeństwa użytymi w tych samouczkach. Podczas wykonywania kroków czynności, aby usunąć i zamienić składniki, należy zwrócić szczególną uwagę do ikony bezpieczeństwo.

| Ikona | Tekst | Dodatkowe informacje |
|:---- |:---- |:-----------|
|![Ikona Ostrzeżenie](./media/storsimple-hardware-component-replacement/Warning.png)| **ZAGROŻENIA!** | Wskazuje niebezpiecznych sytuacji, która nie uniknąć spowoduje śmierci lub poważnie. Ten wyraz sygnału jest ograniczone do najbardziej skrajnych sytuacji.|
|![Ikona Ostrzeżenie](./media/storsimple-hardware-component-replacement/Warning.png)| **OSTRZEŻENIE!** | Wskazuje niebezpiecznych sytuacji, jeśli nie uniknąć może spowodować śmierć lub poważne szkody.|
|![Ikona ostrzeżenia](./media/storsimple-hardware-component-replacement/Caution.png)| **UWAGA!** |Wskazuje niebezpiecznych sytuacji, jeśli nie uniknąć może spowodować szkody pomocniczą lub średnia.|
|![Ikona powiadomienia](./media/storsimple-hardware-component-replacement/NoticeIcon.png)| **UWAGA:** | Wskazuje informacje traktowane jako ważne, ale nie ryzyko związane z.|
|![Ikona wstrząsy elektryczne](./media/storsimple-hardware-component-replacement/Electric.png) | **Zagrożenia wstrząsy elektryczne** | Wskazuje wysokonapięciowego.|
|![Ikona intensywnie weight (waga)](./media/storsimple-hardware-component-replacement/Weight.png)| **Duże weight (waga)**| |
|![Ikona braku zdatne do użytku części użytkownika](./media/storsimple-hardware-component-replacement/NoUserServiceableParts.png)| **Nie części zdatne do użytku użytkownika** | Nie dostęp, chyba że prawidłowo szkolenia.|
|![Przeczytaj instrukcje ikony](./media/storsimple-hardware-component-replacement/ReadInstructions.png)|**Najpierw przeczytaj instrukcje wszystkich**| |
|![Ikona zagrożenia wskazówki](./media/storsimple-hardware-component-replacement/TipHazard.png)|**Porada ryzyka**| |

### <a name="before-you-begin"></a>Przed rozpoczęciem

Zapoznaj się z informacjami o bezpieczeństwo o urządzenia i bezpieczeństwo ikony używane w tym samouczku. Przejdź do [bezpiecznie zainstalować i działania urządzenia StorSimple](storsimple-safety.md) , aby uzyskać pełne informacje. Pamiętaj przejrzeć [ostrożności](storsimple-safety.md#handling-precautions) przed obsługiwać urządzenia StorSimple. 

Przed podjęciem próby wymiana części, rozważ następujące informacje.

![Ikona ostrzeżenia](./media/storsimple-hardware-component-replacement/Warning.png) ![ikona wstrząsy elektryczne](./media/storsimple-hardware-component-replacement/Electric.png) **Ostrzeżenie!** 

- Ziemi samodzielnie poprawnie przy użyciu elektrostatyczne lub antistatic efektem matowego papieru podczas obsługi modułów i składników urządzenia StorSimple.

- Nie dotknij dowolnego obwody. Za pomocą uchwytów podanym i prowadnice podczas obsługi składniki, które mogą być dostępne obwody.

![Ikona ostrzeżenia](./media/storsimple-hardware-component-replacement/Warning.png) ![ikony](./media/storsimple-hardware-component-replacement/NoticeIcon.png) **powiadomienie:**

Po zamianie moduł, **nigdy nie pozostaw puste kieszeni z tyłu załącznik**. Uzyskanie wymiana lub pusty moduł przed usunięciem fragment.

## <a name="hardware-component-replacement-procedures"></a>Procedury zastąpienie składnik sprzętu

Urządzenia serii StorSimple 8000 składa się z kilku wtyczki w podstawowych i/lub EBOD załączniki. 8100 zawiera pojedynczy załącznik podstawowego 8600 jest urządzenia dwa załącznik z podstawowego załącznik i załącznika EBOD.

Główny sprzętowej w urządzeniu przedstawiono w poniższych tabelach. Kliknij łącze w kolumnie **procedury zamienny** przejdź do skojarzone samouczka.

|Składniki|# Przeprowadzanie prezentacji|Wtyczki?|Wymiana procedury
|:---------|:--------|:--------------|:---------------------|
| Obudowa|1|Brak|[Zamienianie obudowy na urządzeniu StorSimple](storsimple-chassis-replacement.md) |
|Podstawowe kontrolery|2|Tak| [Zamień moduł kontrolera na urządzeniu StorSimple](storsimple-controller-replacement.md) |
|764W zasilania i chłodzenia modułów (PCMs)|2|Tak| [Zamienianie Power i chłodzenia modułu na urządzeniu StorSimple](storsimple-power-cooling-module-replacement.md) |
|Baterii kopii zapasowej|2|Tak| [Zastąpienie modułu baterii kopii zapasowej na urządzeniu StorSimple](storsimple-battery-replacement.md) |
|Dysków|12|Tak| [Zamień na dysku na urządzeniu StorSimple](storsimple-disk-drive-replacement.md) |

**Tabela 1** Składniki sprzętowe podstawowego załącznik

Załącznik podstawowego i załącznik EBOD różnią się w ich moduły we/wy. Ponadto PCMs mają różne moc. PCMs w podstawowy załącznik są 764 W, a znajdującymi się w załącznik EBOD 580 W. PCMs w podstawowy załącznik zawierają również moduł baterii kopii zapasowej.

|Składniki|# Przeprowadzanie prezentacji|Wtyczki?| Wymiana procedury
|:---------|:--------|:--------------|:---------------------|
|Obudowa|1|Brak| [Zamienianie obudowy na urządzeniu StorSimple](storsimple-chassis-replacement.md) |
|Kontrolery EBOD|2|Tak| [Zamienianie kontroler EBOD na urządzeniu StorSimple](storsimple-ebod-controller-replacement.md) |
|580W zasilania i chłodzenia modułów (PCMs)|2|Tak| [Zamienianie Power i chłodzenia modułu na urządzeniu StorSimple](storsimple-power-cooling-module-replacement.md) |
|Dysków|12|Tak| [Zamień na dysku na urządzeniu StorSimple](storsimple-disk-drive-replacement.md) |

**Tabela 2** Składniki sprzętowe załącznik EBOD

Wtyczki na tym urządzeniu zostaną wyróżnione w następujących przedniej i tylnej diagramów. Diagramy tych do określenia lokalizacji różnych wtyczki, jeśli wymagane jest zastępczego. Przednia diagramie pokazano stacji dysków i tylnej diagramów załącznik EBOD i Pokaż podstawowego załącznik wtyczki.

![Frontplane urządzenia z dysków](./media/storsimple-hardware-component-replacement/IC741028.png)

**Rysunek 1** Wierzch urządzenia

|Etykieta|Opis|
|:----|:----------|
|0 - 11|Dysków (razem 12)|

Zarówno podstawowy załącznik, jak i załącznik EBOD mają moduły carrier dysk. Obudowa zawiera dwanaście 3,5" dysków rozmieszczone w formacie 3, 4.

![Tablica połączeń urządzenia podstawowego załącznik modułów](./media/storsimple-hardware-component-replacement/IC740994.png)

**Rysunek 2** Tylnej strony podstawowej załącznik

|Etykieta|Opis|
|:----|:----------|
|1|PCM 0|
|2|PCM 1|
|3|Kontroler 0|
|4|Kontroler 1|

![Tablica połączeń z urządzenia EBOD załącznik wtyczki](./media/storsimple-hardware-component-replacement/IC769599.png)

**Rysunek 3** Tylnej strony załącznik EBOD

|Etykieta|Opis|
|:----|:----------|
|1|PCM 0|
|2|PCM 1|
|3|Kontroler EBOD 0|
|4|Kontroler EBOD 1|

## <a name="field-replaceable-units"></a>Pole jednostki

Następujące pola zastąpienia liczby jednostek (FRU) są dostępne dla swojego urządzenia StorSimple:

- Obudowa (łącznie z panelu Operacje zintegrowane)

- 764 W AC PCM

- 580 W AC PCM

- Dysk twardy z modułem carrier dysk

- Moduł kontrolera

- Moduł kontrolera EBOD

- Modułu akumulatora kopii zapasowej

- Instalowanie zestawu kolei stojaków

Sprawdź, [skontaktuj się z pomocą techniczną firmy Microsoft](storsimple-contact-microsoft-support.md) , aby zamówień z dowolnego z tych jednostek zamienny.

## <a name="next-steps"></a>Następne kroki

Przeglądanie wszystkich [informacji dotyczących bezpieczeństwa](storsimple-safety.md) , przed podjęciem próby wymiana części sprzętu StorSimple.
