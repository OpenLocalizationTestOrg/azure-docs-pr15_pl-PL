<properties 
   pageTitle="Wirtualna aktualizacji tablicy StorSimple wersji | Microsoft Azure"
   description="W tym artykule opisano krytyczne otwarte problemy i rozwiązania w tabeli wirtualnej StorSimple uruchomiony aktualizacji 0,2 i 0,1."
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
   ms.workload="NA"
   ms.date="06/16/2016"
   ms.author="alkohli" />

# <a name="storsimple-virtual-array-update-02-and-01-release-notes"></a>Informacje o wersji aktualizacji tablicy wirtualnych 0,2 i 0,1 StorSimple

## <a name="overview"></a>Omówienie

Następujące informacje o wersji zidentyfikować krytyczne otwarte problemy i rozpoznawania problemów dotyczących aktualizacji Microsoft Azure StorSimple wirtualnych tablicy. (Microsoft Azure StorSimple wirtualnych tablicy jest nazywany StorSimple urządzenia wirtualnego lokalnego lub urządzenia wirtualnego StorSimple). 

Informacje o wersji są stale aktualizowane i jak zostaną wykryte problemy krytyczne wymagające obejść ten problem, są one dodawane. Przed wdrożeniem urządzenia wirtualnego StorSimple dokładnie przejrzyj informacje zawarte w informacjach o wersji.

Aktualizacja 0,2 odpowiada wersję oprogramowania **10.0.10280.0**; Aktualizacja 0,1 jest wersja **10.0.10279.0**. W sekcjach poniżej wymieniono zmiany dla każdej aktualizacji. 

> [AZURE.NOTE] Aktualizacje są one i będzie Uruchom ponownie urządzenie. Jeśli we/wy jest w toku, urządzenie będzie powodowało przestoje.

## <a name="issues-fixed-in-the-update-02"></a>Problemy rozwiązywane przez aktualizację 0,2
Aktualizacja 0,2 zawiera wszystkie zmiany z aktualizacji 0,1 Oprócz poprawki opisanej w poniższej tabeli:

Funkcja                              | Problem                                                                                                                                                                                                                                                                                                                           |
--------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
Aktualizacje                                 | W ostatniej wersji nie zostały wykryte aktualizacje automatycznie w portalu klasyczny Azure, trzeba było użyć lokalny interfejs sieci Web do zainstalowania aktualizacji. Ten problem został rozwiązany w tej wersji. Po zainstalowaniu aktualizacji 0,2, możesz zainstalować przyszłych aktualizacji przy użyciu portalu klasyczny Azure.                       

## <a name="whats-new-in-the-update-01"></a>Co nowego w tej aktualizacji 0,1

Aktualizacja 0,1 zawiera następujące usprawnień i poprawek. 

- **Ulepszone elastyczność dla dostawie chmury**: Ta wersja zawiera kilka poprawki wokół awarii, kopii zapasowej, przywracanie i obsługi w przypadku przerwania łączności chmury. 

- **Ulepszone przywrócić wydajność**: Ta wersja zawiera poprawki, które znacznie zmniejszyć czas zakończenia zadań przywracania.

- **Optymalizacja odzyskiwanie miejsca automatycznego**: gdy dane są usuwane znacznie ustanawianie ilości, bloków nieużywanego miejsca do magazynowania należy można odzyskać. Ta wersja uległa proces odzyskiwanie miejsca z chmury, uzyskując nieużywana staje się dostępna szybciej w porównaniu z poprzedniej wersji.

- **Nowe obrazy wirtualnego dysku**: nowy wirtualny dysk twardy, VHDX i VMDK są teraz dostępne za pośrednictwem portalu klasyczny Azure. Możesz pobrać te obrazy do zapewniania obsługi nowych aktualizacji 0,1 urządzeń.

- **Poprawianie dokładności stan zadań w portalu**: W starszej wersji oprogramowania, stan zadania raportowania w portalu nie został szczegółowego. Ten problem został rozwiązany w tej wersji.

- **Dołączanie domeny możliwości**: poprawki dotyczące dołączania do domeny i zmienianie nazw urządzenia.


## <a name="issues-fixed-in-the-update-01"></a>Problemy rozwiązywane przez aktualizację 0,1

Poniższa tabela zawiera podsumowanie problemy rozwiązywane w tej wersji.

| Wartość nie.  | Funkcja                              | Problem                                                                                                                                                                                                                                                                                                                           |
|------|--------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1    | VMDK                                 | W niektórych wersjach VMware dysku OS było widoczne jako rzadkie powodujące alerty i zakłócenia normalnego działania. Ten problem został rozwiązany w tej wersji.                                                                                                                                                                                    |
| 2    | serwerem                         | W ostatniej wersji użytkownik został wymagany do określenia bramy dla każdego enabled sieciowej urządzenia wirtualnego StorSimple. To zachowanie zostanie zmieniony w tej wersji, dlatego użytkownik musi skonfigurować co najmniej jednej bramy dla wszystkich interfejsów sieci włączone.                                                                              |
| 3    | Pakiet pomocy technicznej                      | W starszej wersji oprogramowania obsługuje zebrania pakietu nie powiodła się podczas rozmiarów pakiet zostały większym niż 1 GB. Ten problem został rozwiązany w tej wersji.                                                                                                                                                                               |
| 4    | Dostęp w chmurze                         |  W ostatniej wersji Jeżeli tablica Virtual StorSimple nie ma połączenia sieciowego i ponownego uruchomienia lokalnego interfejsu użytkownika będzie miała problemy z łącznością. Ten problem został rozwiązany w tej wersji.                                                                                                                            |
| 5    | Monitorowanie wykresów                    | W poprzedniej wersji, po trybie awaryjnym urządzenia wykresy wykorzystania możliwości chmury wyświetlane niepoprawnymi wartościami w portalu klasyczny Azure. Ten problem zostanie rozwiązany w bieżącej wersji.                                                                                                                          |



## <a name="known-issues-in-the-update-01"></a>Znane problemy dotyczące aktualizacji 0,1

Poniższa tabela zawiera podsumowanie znane problemy dotyczące tablicy Virtual StorSimple oraz problemy zauważyć wersji z poprzednimi wersjami. **W chwili wydania wersji problemy wymienione w tej wersji są oznaczone gwiazdką. Prawie wszystkie problemy na tej liście zostały przeniesione z wersji GA StorSimple wirtualnych tablicy.**


| Wartość nie. | Funkcja | Problem | Obejście i komentarzy |
|-----|--------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **1.** | Aktualizacje | Nie można zaktualizować urządzeń wirtualnych utworzonych w wersji preview do wersji ogólnodostępną. | Tych urządzeń wirtualnych musi zainicjowana wydania ogólnodostępną za pomocą przepływu odzyskiwania (DR) danych. |
| **2.** | Ustanawianie danych dysku | Po mają obsługi administracyjnej dyskiem danych o określonym rozmiarze określonym i utworzony odpowiedniego urządzenia wirtualnego StorSimple, użytkownik musi nie powiększanie i zmniejszanie dysku danych. Próby spowoduje utratę wszystkich danych w lokalnym poziomów urządzenia. |   |
| **3.** | Zasady grupy | Gdy urządzenie jest domeny, stosowanie zasad grupy mogą negatywnie wpłynąć na działanie urządzenia. | Upewnij się, virtual macierzy znajduje się w osobnym jednostka organizacyjna (OU) usługi Active Directory, a nie obiekty zasad grupy (GPO) są stosowane do niego.|
| **4.** | Lokalny interfejs użytkownika sieci web | Ulepszone funkcje zabezpieczeń są włączone w programie Internet Explorer (IE ESC), niektóre lokalnego interfejsu użytkownika strony, takie jak rozwiązywanie problemów lub konserwacji może nie działać prawidłowo. Przyciski na następujących stronach również może nie działać. | Wyłącz funkcję ulepszone funkcje zabezpieczeń w programie Internet Explorer.|
| **5.** | Lokalny interfejs użytkownika sieci web | Na komputerze wirtualnej funkcji Hyper-V interfejsy sieciowe w sieci web interfejsu użytkownika są wyświetlane jako 10 GB interfejsów. | To zachowanie jest odbicie funkcji Hyper-v. Funkcji Hyper-V zawiera zawsze 10 GB dla kart wirtualnej sieci. |
| **6.** | Wielkości warstwowych lub akcji | Zakres bajtu blokowanie dla aplikacji, które współpracują z StorSimple warstwowych wielkości nie jest obsługiwane. Po włączeniu bajtu blokowanie zakresu obsługi StorSimple nie będzie działać. | Zalecane środki obejmują: <br></br>Wyłączanie blokowania w logiki aplikacji zakres bajtów.<br></br>Wybierz umieścić wielkości lokalnie przypiętych odróżnieniu od ilości warstwowych dane dla tej aplikacji.<br></br>*Ostrzeżenie*: Jeśli przy użyciu lokalnie przypięte wielkości i blokowanie zakresu bajtów jest włączona, należy pamiętać, wielkość lokalnie przypięty można online nawet, przed zakończeniem przywracania. W takich przypadkach jeśli przywracania jest w toku, następnie należy poczekać Przywróć, aby zakończyć. |
| **7.** | Akcje warstwowych | Praca z dużych plików może spowodować powolne warstwa się. | Podczas pracy z dużych plików, zaleca się, że największa plik jest mniejszy niż 3% wielkości Udostępnij. |
| **8.** | Używana pojemność akcji | Może zostać wyświetlony udostępnianie zużycie w przypadku braku danych w udziale. Jest to spowodowane możliwości używanych akcji zawiera metadanych. |   |
| **9.** | Odzyskiwanie | Można wykonać tylko odzyskiwanie na serwerze plików do tej samej domeny, co urządzenia. Odzyskiwanie do urządzenia w innej domeny nie jest obsługiwane w tej wersji. | Są to realizowane w nowszej wersji. |
| **10.** | Azure programu PowerShell | Nie można zarządzać urządzeń wirtualnych StorSimple przy użyciu programu PowerShell Azure w tej wersji. | Zarządzanie urządzeń wirtualnych powinno być wykonywane za pośrednictwem portalu klasyczny Azure i lokalnych sieci web interfejsu użytkownika. |
| **11.** | Zmienianie hasła | Konsola urządzenia wirtualnego tablicy akceptuje tylko dane wejściowe w formacie klawiatury en US. |   |
| **12.** | CHAP | Nie można usunąć poświadczenia CHAP raz utworzone. Ponadto modyfikując poświadczeń CHAP, będzie konieczne wielkość przełączyć do trybu offline, a następnie przełącz je na online, aby zmiana została uwzględniona. | Te zostaną uwzględnione w nowszej wersji. |
| **13.** | serwerem  | "Używane miejsca do magazynowania" wyświetlany dla woluminu iSCSI mogą się różnić w usługę Menedżer StorSimple i iSCSI host. | ISCSI host ma widok systemu plików.<br></br>Urządzenie widzi bloków przydzielany podczas wielkość była w maksymalnym rozmiarze.|
| **14.** | Plik serwera *  | Jeśli pliku w folderze zawiera alternatywny danych strumienia (AD) skojarzone z nim, REKLAM nie jest kopii zapasowej lub przywrócić za pośrednictwem awarii, klonowanie i odzyskiwanie poziom elementów.| |


## <a name="next-step"></a>Następny krok

[Instalowanie aktualizacji](storsimple-ova-install-update-01.md) na macierzy Virtual StorSimple.
