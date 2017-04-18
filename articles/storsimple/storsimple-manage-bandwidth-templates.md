<properties
   pageTitle="Zarządzanie szablonami przepustowości sieci StorSimple | Microsoft Azure"
   description="W tym artykule opisano Zarządzanie szablonami przepustowości StorSimple, które umożliwiają sterowanie przepustowości."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/16/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-manage-storsimple-bandwidth-templates"></a>Zarządzanie szablonami przepustowości StorSimple za pomocą usługi Menedżera StorSimple

## <a name="overview"></a>Omówienie

Szablony przepustowości umożliwiają konfigurowanie wykorzystania przepustowości sieci u wielu harmonogramów godziny dnia do poziomu dane z urządzenia StorSimple w chmurze.

Za pomocą przepustowości harmonogramów można:

- Określ harmonogramów przepustowości dostosowane w zależności od zastosowania obciążenie sieci.

- Scentralizowane zarządzanie i ponowne używanie harmonogramy na wielu urządzeniach w sposób łatwy i bezproblemowa.

> [AZURE.NOTE] Ta funkcja jest dostępna tylko w przypadku fizycznymi StorSimple, a nie do urządzenia wirtualne.

Wszystkie szablony przepustowości dla usługi są wyświetlane w formacie tabelarycznym i zawiera następujące informacje:

- **Nazwa** — nazwa przypisana do szablonu przepustowości podczas jej tworzenia.

- **Harmonogram** — liczba harmonogramów zawarte w szablonie danego przepustowości.

- **Używane przez** — objętości przy użyciu szablonów przepustowości.

Umożliwia stronę **Konfigurowanie** Menedżera StorSimple usługi w portalu klasyczny Azure Zarządzanie szablonami przepustowości.

Możesz również znaleźć dodatkowe informacje w celu skonfigurowania szablony przepustowości w:

- Pytania i odpowiedzi dotyczące przepustowości szablonów
- Najważniejsze wskazówki dotyczące przepustowości szablonów

## <a name="add-a-bandwidth-template"></a>Dodawanie szablonu przepustowości

Wykonaj poniższe czynności, aby utworzyć nowy szablon przepustowości.

#### <a name="to-add-a-bandwidth-template"></a>Aby dodać szablon przepustowości

1. Na stronie **Konfigurowanie** usługi Menedżera StorSimple kliknij **szablon przepustowości Dodawanie/edytowanie**.

2. W oknie dialogowym **Dodawanie/edytowanie przepustowości szablonu** :

   1. Z listy rozwijanej **szablon** wybierz pozycję **Utwórz nowy** , aby dodać nowy szablon przepustowości.
   2. Określ unikatową nazwę szablonu przepustowości.

3. Definiowanie **harmonogramu przepustowości**. Aby utworzyć harmonogram:

   1. Z listy rozwijanej wybierz dni tygodnia skonfigurowano harmonogramu. Można wybrać kilka dni, zaznaczając pola wyboru znajdującej się przed odpowiednich dni na liście.
   2. Zaznacz opcję **Cały dzień** harmonogramu są wymuszane przez cały dzień. Gdy ta opcja jest zaznaczona, nie będzie można określić **Czas rozpoczęcia** i **Czas zakończenia**. Harmonogram uruchamia z 12:00 AM do 11:59 PM.
   3. Z listy rozwijanej wybierz **Czas rozpoczęcia**. Jest to w przypadku rozpocznie harmonogramu.
   4. Z listy rozwijanej wybierz **Czas zakończenia**. Jest to w przypadku przestanie harmonogramu.

         > [AZURE.NOTE] Nakładające się harmonogramów nie są dozwolone. Jeśli godziny rozpoczęcia i zakończenia spowoduje nakładające się harmonogram, pojawi się komunikat o błędzie w tym celu.

   5. Określ **stopa przepustowości**. Jest to przepustowość w MB na sekundę (MB/s) używane przez urządzenia StorSimple w operacji dotyczących w chmurze (operacje przekazywania i pobierania). Wprowadź liczbę z zakresu od 1 do 1000 dla tego pola.

   6. Kliknij ikonę wyboru ![ikona modułu sprawdzania](./media/storsimple-manage-bandwidth-templates/HCS_CheckIcon.png). Szablon, który został utworzony zostanie dodana do listy szablonów przepustowości na stronie **Konfigurowanie** usługi.

    ![Tworzenie nowego szablonu przepustowości](./media/storsimple-manage-bandwidth-templates/HCS_CreateNewBT1.png)

4. Kliknij przycisk **Zapisz** u dołu strony, a następnie kliknij przycisk **Tak,** po wyświetleniu monitu o potwierdzenie. Spowoduje to zapisanie zmian wprowadzonych w konfiguracji.

## <a name="edit-a-bandwidth-template"></a>Edytowanie szablonu przepustowości

Wykonaj poniższe czynności, aby edytować szablon przepustowości.

### <a name="to-edit-a-bandwidth-template"></a>Aby edytować szablon przepustowości

1. Kliknij **szablon przepustowości Dodawanie/edytowanie**.

2. W oknie dialogowym **Dodawanie/edytowanie przepustowości szablonu** :

   1. Z listy rozwijanej **szablon** wybierz istniejący szablon przepustowości, który chcesz zmodyfikować.
   2. Wprowadź zmiany. (Można modyfikować istniejących ustawień.)
   3. Kliknij ikonę wyboru ![Ikona modułu sprawdzania](./media/storsimple-manage-bandwidth-templates/HCS_CheckIcon.png). Zmodyfikowany szablon na liście szablonów przepustowości widzą na stronie Konfigurowanie usługi.

3. Aby zapisać zmiany, kliknij pozycję **Zapisz** u dołu strony. Kliknij przycisk **Tak,** gdy zostanie wyświetlony monit o potwierdzenie.

> [AZURE.NOTE] Użytkownik będzie niemożliwe Aby zapisać zmiany, jeśli edytowana harmonogram nakłada się przy użyciu istniejącego harmonogramu w szablonie przepustowości modyfikowanej.

## <a name="delete-a-bandwidth-template"></a>Usuwanie szablonu przepustowości

Wykonaj poniższe czynności, aby usunąć szablon przepustowości.

#### <a name="to-delete-a-bandwidth-template"></a>Aby usunąć szablon przepustowości

1. Na liście tabelaryczny szablony przepustowości dla usługi wybierz szablon, który chcesz usunąć. Po skrajnej prawej stronie wybranego szablonu pojawi się w ikonę Usuń (**x**). Kliknij ikonę **x** , aby usunąć szablon.

2. Pojawi się monit o potwierdzenie. Kliknij **przycisk OK** , aby kontynuować.

Jeśli szablon jest używany przez dowolnego woluminów, nie będzie można ją usunąć. Zostanie wyświetlony komunikat o błędzie wskazujący, że szablon jest używany. Zostanie wyświetlone okno dialogowe komunikat o błędzie, przekazanie informacji powinny zostać usunięte wszystkie odwołania do szablonu.

Uzyskiwanie dostępu do strony **Głośność kontenerów** i modyfikując kontenerów głośność, które tak, aby użyć innego szablonu i używanie ustawień niestandardowych lub nieograniczoną przepustowości za pomocą tego szablonu można usunąć wszystkie odwołania do szablonu. Po usunięciu wszystkich odwołań, możesz usunąć szablon.

## <a name="use-a-default-bandwidth-template"></a>Użyj szablonu domyślnego przepustowości

Domyślny szablon przepustowości znajduje się i jest używany przez kontenerów głośność domyślnie wymuszenie kontrolek przepustowości podczas uzyskiwania dostępu do chmury. Domyślny szablon służy także jako gotowy dla użytkowników, którzy tworzyć własne szablony. Szczegóły tego szablonu domyślnego są:

- **Nazwa** — nieograniczoną nocy i weekendy

- **Harmonogram** — pojedynczego harmonogramu od poniedziałku do piątku, zastosowany przepustowości stopień 1 MB/s między 8 AM i 5 PM urządzenia godziny. Przepustowość jest ustawiona na nieograniczone do końca tygodnia.

Szablon domyślny można edytować. Użycie tego szablonu (w tym wersji edytowanego) są śledzone.

## <a name="create-an-all-day-bandwidth-template-that-starts-at-a-specified-time"></a>Tworzenie szablonu przepustowości całodzienne rozpoczyna się w określonym czasie

Wykonaj poniższą procedurę, aby utworzyć harmonogram rozpoczyna się w określonym czasie, który jest uruchamiany cały dzień. W tym przykładzie harmonogramu zaczyna się od 9 AM rano i uruchamia do 9 AM następnego dnia rano. Należy pamiętać, że godziny rozpoczęcia i zakończenia dla serii rozłożonych w danym muszą być zarówno zawarte w tym samym harmonogramem 24-godzinnego i nie może obejmować wiele dni. Jeśli musisz skonfigurować szablony przepustowości, obejmujące wiele dni, należy użyć wielu harmonogramów (jak pokazano w przykładzie).

#### <a name="to-create-an-all-day-bandwidth-template"></a>Aby utworzyć szablon całodzienne przepustowości

1. Utworzenie harmonogramu, który zaczyna się od 9 AM rano i uruchamia do północy.

2. Dodać kolejny harmonogram. Konfigurowanie drugiego harmonogramu do uruchomienia od północy do 9 AM rano.

3. Zapisywanie szablonu przepustowości.

Złożony harmonogram zostanie następnie uruchomienie jednocześnie wybrane i używanie całodzienne.

## <a name="questions-and-answers-about-bandwidth-templates"></a>Pytania i odpowiedzi dotyczące przepustowości szablonów

**Q**. Co się dzieje kontrolek przepustowości podczas pracy między harmonogramy? (Harmonogramu został zakończony i innym nie została jeszcze uruchomiona).

**A**. W takich przypadkach zostaną zatrudnionych bez kontrolek przepustowości. Oznacza to, że urządzenie można używać nieograniczoną przepustowości podczas tworzenia warstw danych w chmurze.

**Q**. Można modyfikować szablony przepustowości na urządzeniu z systemem offline?

**A**. Nie można modyfikować szablony przepustowości na kontenerów wielkości, jeśli odpowiedniego urządzenia jest w trybie offline.

**Q**. Można edytować szablon przepustowości skojarzony z kontenera głośność, gdy wielkość skojarzone są w trybie offline?

**A**. Można zmodyfikować szablon przepustowości skojarzone z kontenera głośność którego ilości pracy w trybie offline. Należy zauważyć, że po ilości pracy w trybie offline, bez danych będzie można tiered z urządzenia w chmurze.

**Q**. Szablon domyślny można usunąć?

**A**. Szablon domyślny można usuwać, nie jest dobrym pomysłem jest to zrobić. Zastosowania szablonu domyślnego, wersji edytowanego są śledzone. Dane śledzenia analizy i w ciągu godziny są używane do poprawy szablonu domyślnego.

**Q**. Jak można ustalić, szablony przepustowości muszą zostać zmodyfikowane?

**A**. Jest jednym ze znaków konieczne modyfikowanie szablonów przepustowości podczas uruchamiania sieć działa wolno lub podlewki wielokrotnie w ciągu dnia są wyświetlane. W takim przypadku monitorowania sieci magazynowania i zastosowania sprawdzając wykresy wydajność wejścia/wyjścia i przepustowość sieci.

Dane przepustowość sieci identyfikowanie godziny dnia i kontenerów głośność, w których występuje gardła sieci. Jeśli dzieje, gdy jest podejmowana warstwowa danych w chmurze (uzyskać te informacje z wydajność wejścia/wyjścia dla wszystkich kontenerów głośność urządzenia do chmury), a następnie będzie konieczne modyfikowanie szablonów przepustowości skojarzone z kontenerów głośność.

Po modyfikacji szablony są używane, może być konieczne monitorowania sieci ponownie, aby istotne opóźnienia. Jeśli te nadal istnieje, następnie będzie konieczne Kontroluj szablonów przepustowości.

**Q**. Co się stanie, jeśli masz wiele kontenerów głośność na moim urządzeniu planuje tę zakładkę, ale różne ograniczenia dotyczą wszystkich?

**A**. Załóżmy, że korzystasz z urządzenia z kontenerami obrót 3. Harmonogramy skojarzone z tych kontenerów całkowicie zakładki. Dla każdego z tych kontenerów ograniczenia przepustowości używane są 5, 10 i 15 MB/s odpowiednio. Podczas operacji dotyczących występują we wszystkich tych kontenerów w tym samym czasie, co najmniej 3 ograniczenia przepustowości mogą być stosowane: w tym przypadku 5 MB/s one wychodzące żądania We/Wy udostępnianie tej samej kolejki.

## <a name="best-practices-for-bandwidth-templates"></a>Najważniejsze wskazówki dotyczące przepustowości szablonów

Wykonaj poniższe najważniejsze wskazówki dotyczące urządzenia StorSimple:

- Konfigurowanie szablonów przepustowości na urządzeniu, aby włączyć zmiennych ograniczania przepustowości sieci przez urządzenie w różnych porach dnia. Tych szablonów przepustowości, w przypadku użycia z kopii zapasowej harmonogramów skuteczne mogą korzystać z dodatkowych przepustowości dla operacji chmury czasie mniejszego.

- Oblicz rzeczywisty przepustowość wymagane dla danego wdrożenia na podstawie rozmiaru wdrażanie i cel czas wymagany odzyskiwania (RTO).

## <a name="next-steps"></a>Następne kroki

Dowiedz się więcej o [korzystaniu z usługi Menedżera StorSimple administrowania urządzenia StorSimple](storsimple-manager-service-administration.md).
