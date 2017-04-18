<properties 
   pageTitle="Wirtualna aktualizacji tablicy StorSimple wersji | Microsoft Azure"
   description="W tym artykule opisano krytyczne otwarte problemy i rozwiązania w tabeli wirtualnej StorSimple uruchomiony aktualizacji 0,3."
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
   ms.date="09/15/2016"
   ms.author="alkohli" />

# <a name="storsimple-virtual-array-update-03-release-notes"></a>Wirtualna aktualizacji tablicy StorSimple 0,3 informacje o wersji

## <a name="overview"></a>Omówienie

Następujące informacje o wersji zidentyfikować krytyczne otwarte problemy i rozpoznawania problemów dotyczących aktualizacji Microsoft Azure StorSimple wirtualnych tablicy.

Informacje o wersji są stale aktualizowane i jak zostaną wykryte problemy krytyczne wymagające obejść ten problem, są one dodawane. Przed wdrożeniem macierzy Virtual StorSimple dokładnie przejrzyj informacje zawarte w informacjach o wersji.

Aktualizacja 0,3 odpowiada wersję oprogramowania **10.0.10288.0**.

> [AZURE.NOTE] Aktualizacje są one i uruchom ponownie urządzenie. Jeśli we/wy jest w toku, urządzenie wiąże się przestoje.


## <a name="whats-new-in-the-update-03"></a>Co nowego w tej aktualizacji 0,3

Aktualizacja 0,3 jest przede wszystkim kompilacji poprawka błędu. W tej wersji zostały rozwiązane kilka usterek uzyskując kopii zapasowej błędy w poprzedniej wersji.

## <a name="issues-fixed-in-the-update-03"></a>Problemy rozwiązywane przez aktualizację 0,3

Poniższa tabela zawiera podsumowanie problemy rozwiązywane w tej wersji.

| Wartość nie.  | Funkcja                              | Problem                                                                                                                                                                                                                                                                                                                           |
|------|--------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1    | Wykonywanie kopii zapasowych                                |Problem było widoczne w starszej wersji, miejsce, w którym chcesz nie powiedzie się do udziału plików kopii zapasowych. Jeśli wystąpił ten problem, zadania wykonywania kopii zapasowej może zakończyć się niepowodzeniem i alert krytyczny został dostarczony w usłudze Menedżer StorSimple w celu powiadomienia użytkownika. Ten problem nie ma wpływu danych w akcji lub dostępu do danych. Przyczynę została identyfikacji i stałe w tej wersji. <br></br> Ta poprawka nie dotyczy wstecz akcji, które są już wyświetlane tego problemu. Użytkownicy widzą ten problem należy najpierw zastosować aktualizację 0,3, a następnie skontaktuj się z programu Microsoft Support wykonywania kopii zapasowej całego systemu, aby rozwiązać ten problem. Zamiast kontaktować się z Support firmy Microsoft, klienci można także przywrócić do nowego udziału z kopii zapasowej prawidłowy, którego dotyczy problem akcji.                                                                                                                                                                                 |
| 2    | iSCSI                         | Problem było widoczne w starszej wersji miejsce, w którym wielkość zniknie podczas kopiowania danych do woluminu na tablicy Virtual StorSimple. Ten problem został rozwiązany w tej wersji. <br></br> Poprawki zostaną zastosowane tylko na nowo utworzone wielkości. Poprawki nie dotyczą wstecz ilości, które są już wyświetlane tego problemu. Klienci są zaleca się, aby wyświetlić wielkość dotyczy w trybie online za pośrednictwem portalu klasyczny Azure, wykonywanie kopii zapasowej dla tych wielkości, a następnie przywrócić tych wielkości do nowej wielkości.                                                               |


## <a name="known-issues-in-the-update-03"></a>Znane problemy dotyczące aktualizacji 0,3

Poniższa tabela zawiera podsumowanie znane problemy dotyczące tablicy Virtual StorSimple oraz problemy zauważyć wersji z poprzednimi wersjami. 


| Wartość nie. | Funkcja | Problem | Obejście i komentarzy |
|-----|--------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **1.** | Aktualizacje | Nie można zaktualizować urządzeń wirtualnych utworzonych w wersji preview do wersji ogólnodostępną. | Tych urządzeń wirtualnych musi zainicjowana wydania ogólnodostępną za pomocą przepływu odzyskiwania (DR) danych. |
| **2.** | Ustanawianie danych dysku | Po mają obsługi administracyjnej dysku danych o określonym rozmiarze określonym i utworzony odpowiedniego urządzenia wirtualnego StorSimple, użytkownik musi nie powiększanie i zmniejszanie dysku danych. Próba wykonaj powoduje utratę wszystkich danych w lokalnym poziomów urządzenia. |   |
| **3.** | Zasady grupy | Gdy urządzenie jest domeny, stosowanie zasad grupy mogą negatywnie wpłynąć na działanie urządzenia. | Upewnij się, virtual macierzy znajduje się w osobnym jednostka organizacyjna (OU) usługi Active Directory, a nie obiekty zasad grupy (GPO) są stosowane do niego.|
| **4.** | Lokalny interfejs użytkownika sieci web | Ulepszone funkcje zabezpieczeń są włączone w programie Internet Explorer (IE ESC), niektóre lokalnego interfejsu użytkownika strony, takie jak rozwiązywanie problemów lub konserwacji może nie działać prawidłowo. Przyciski na następujących stronach również może nie działać. | Wyłącz funkcję ulepszone funkcje zabezpieczeń w programie Internet Explorer.|
| **5.** | Lokalny interfejs użytkownika sieci web | Na komputerze wirtualnej funkcji Hyper-V interfejsy sieciowe w sieci web interfejsu użytkownika są wyświetlane jako 10 GB interfejsów. | To zachowanie jest odbicie funkcji Hyper-v. Funkcji Hyper-V zawiera zawsze 10 GB dla kart wirtualnej sieci. |
| **6.** | Wielkości warstwowych lub akcji | Zakres bajtu blokowanie dla aplikacji, które współpracują z StorSimple warstwowych wielkości nie jest obsługiwane. Po włączeniu bajtu blokowanie zakresu obsługi StorSimple nie działa. | Zalecane środki obejmują: <br></br>Wyłączanie blokowania w logiki aplikacji zakres bajtów.<br></br>Wybierz umieścić wielkości lokalnie przypiętych odróżnieniu od ilości warstwowych dane dla tej aplikacji.<br></br>*Ostrzeżenie*: gdy przy użyciu lokalnie przypięte wielkości i blokowanie zakresu bajtów jest włączona, wielkość lokalnie przypięty może być online nawet, przed zakończeniem przywracania. W takich przypadkach jeśli przywracania jest w toku, następnie należy poczekać Przywróć, aby zakończyć. |
| **7.** | Akcje warstwowych | Praca z dużych plików może spowodować powolne warstwa się. | Podczas pracy z dużych plików, zaleca się, że największa plik jest mniejszy niż 3% wielkości Udostępnij. |
| **8.** | Używana pojemność akcji | Może zostać wyświetlony Udostępnij zużycie, jeśli nie ma żadnych danych w udziale. Jest to spowodowane możliwości używanych akcji zawiera metadanych. |   |
| **9.** | Odzyskiwanie | Można wykonać tylko odzyskiwanie na serwerze plików do tej samej domeny, co urządzenia. Odzyskiwanie do urządzenia w innej domeny nie jest obsługiwane w tej wersji. | To jest zaimplementowana w nowszej wersji. |
| **10.** | Azure programu PowerShell | Nie można zarządzać urządzeń wirtualnych StorSimple przy użyciu programu PowerShell Azure w tej wersji. | Zarządzanie urządzeń wirtualnych powinno być wykonywane za pośrednictwem portalu klasyczny Azure i lokalnych sieci web interfejsu użytkownika. |
| **11.** | Zmienianie hasła | Konsola urządzenia wirtualnego tablicy akceptuje tylko dane wejściowe w formacie klawiatury en US. |   |
| **12.** | CHAP | Nie można usunąć poświadczenia CHAP raz utworzone. Ponadto po zmodyfikowaniu poświadczeń CHAP należy przełączyć wielkość do trybu offline, a następnie przełącz je na online, aby zmiana została uwzględniona. | Ten problem został rozwiązany w nowszej wersji. |
| **13.** | serwerem  | "Używane miejsca do magazynowania" wyświetlany dla woluminu iSCSI mogą się różnić w usługę Menedżer StorSimple i iSCSI host. | ISCSI host ma widok systemu plików.<br></br>Urządzenie widzi bloków nadawany przy wielkość była w maksymalnym rozmiarze.|
| **14.** | Serwer plików  | Jeśli pliku w folderze zawiera alternatywny danych strumienia (AD) skojarzone z nim, REKLAM nie jest kopii zapasowej lub przywrócić za pośrednictwem awarii, klonowanie i odzyskiwanie poziom elementów.| |


## <a name="next-step"></a>Następny krok

[Zainstaluj aktualizację 0,3](storsimple-ova-install-update-01.md) na macierzy Virtual StorSimple.

## <a name="references"></a>Odwołania

Szukasz notatkę starszych wersji? Przejdź do: 

- [Informacje o aktualizacji tablicy Virtual StorSimple 0,1 i 0,2 wersji](storsimple-ova-update-01-release-notes.md)

- [StorSimple wirtualnych ogólne tablicy dostępność informacje o wersji](storsimple-ova-pp-release-notes.md)
 

