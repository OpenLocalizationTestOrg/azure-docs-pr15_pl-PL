<properties 
   pageTitle="Tablica Virtual StorSimple informacje o wersji | Microsoft Azure"
   description="W tym artykule opisano krytyczne otwarte problemy i rozwiązania w tabeli wirtualnej StorSimple."
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
   ms.date="05/13/2016"
   ms.author="alkohli" />

# <a name="storsimple-virtual-array-release-notes"></a>Tablica Virtual StorSimple informacje o wersji

## <a name="overview"></a>Omówienie

Następujące informacje o wersji zidentyfikowanie krytycznych otwarte problemy dla wersji ogólnodostępną (GA) 2016 marca Microsoft Azure StorSimple wirtualnych tablicy (nazywane także StorSimple urządzenia wirtualnego lokalnego lub urządzenia wirtualnego StorSimple). Ta wersja odpowiada wersję oprogramowania 10.0.10271.0.

Informacje o wersji są stale aktualizowane i jak zostaną wykryte problemy krytyczne wymagające obejść ten problem, są one dodawane. Przed wdrożeniem urządzenia wirtualnego StorSimple dokładnie przejrzyj informacje zawarte w informacjach o wersji. 

Poniższa tabela zawiera podsumowanie znane problemy w tej wersji.


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
