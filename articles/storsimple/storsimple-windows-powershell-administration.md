<properties 
   pageTitle="PowerShell do zarządzania urządzeniami StorSimple | Microsoft Azure"
   description="Dowiedz się, jak za pomocą programu Windows PowerShell dla StorSimple Zarządzanie Twoim urządzeniem StorSimple."
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
   ms.date="08/18/2016"
   ms.author="alkohli@microsoft.com" />

# <a name="use-windows-powershell-for-storsimple-to-administer-your-device"></a>Administrowanie urządzenia za pomocą programu Windows PowerShell dla StorSimple

## <a name="overview"></a>Omówienie

Windows PowerShell dla StorSimple udostępnia interfejs wiersza polecenia, który umożliwia zarządzanie Twoim urządzeniem Microsoft Azure StorSimple. Jak wynika z nazwy, jest oparty na programu Windows PowerShell, interfejs wiersza polecenia, który jest wbudowany w ograniczone działania. Z perspektywy użytkownika w wierszu polecenia ograniczone działania jest wyświetlany jako ograniczeniami wersję programu Windows PowerShell. Przy zachowaniu niektóre podstawowe funkcje programu Windows PowerShell, to interfejs ma dodatkowe dedykowane polecenia cmdlet, które są przeznaczone dla zarządzanie urządzeniem Microsoft Azure StorSimple. 

W tym artykule opisano środowiska Windows PowerShell dla funkcji StorSimple, w tym, jak można nawiązać tego interfejsu i zawiera łącza do procedury krok po kroku lub przepływów pracy, które można wykonywać przy użyciu tego interfejsu. Przepływy pracy zawierają jak zarejestrować urządzenie, skonfigurować interfejs sieciowy na urządzeniu, zainstaluj aktualizacje, które wymagają urządzenie, aby być w trybie konserwacji, Zmień stan urządzenie i spróbuj rozwiązać wszelkie problemy, które mogą wystąpić.

Po zapoznaniu się w tym artykule, będą mogli:

- Połącz urządzenie z systemem StorSimple przy użyciu programu Windows PowerShell dla StorSimple.

- Administrowanie urządzenia StorSimple przy użyciu programu Windows PowerShell dla StorSimple.

- Uzyskaj pomoc w programie Windows PowerShell dla StorSimple.

>[AZURE.NOTE]   

>- Windows PowerShell dla StorSimple polecenia cmdlet umożliwia zarządzanie Twoim urządzeniem StorSimple z poziomu konsoli szeregowego lub zdalnie za pośrednictwem programu Windows PowerShell zdalnych. Aby uzyskać więcej informacji na temat poszczególnych poszczególnych poleceń cmdlet, który może być używany w tym interfejsie przejdź do [informacje dotyczące poleceń cmdlet środowiska Windows PowerShell dla StorSimple](https://technet.microsoft.com/library/dn688168.aspx).

>- Polecenia cmdlet StorSimple programu PowerShell Azure są różne zbiorem polecenia cmdlet, umożliwiająca celu zautomatyzowania StorSimple poziomu usług i zadania związane z migracją z poziomu wiersza polecenia. Aby uzyskać więcej informacji na temat polecenia cmdlet programu PowerShell Azure dla StorSimple przejdź do [Azure StorSimple informacje dotyczące poleceń cmdlet](https://msdn.microsoft.com/library/azure/dn920427.aspx).

Uzyskiwania dostępu do programu Windows PowerShell dla StorSimple przy użyciu jednej z następujących metod:

- [Nawiązywanie połączenia z konsoli szeregowego urządzenia StorSimple](#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console)
- [Nawiązanie połączenia zdalnego StorSimple przy użyciu programu Windows PowerShell](#connect-remotely-to-storsimple-using-windows-powershell-for-storsimple)
    

## <a name="connect-to-windows-powershell-for-storsimple-via-the-device-serial-console"></a>Łączenie się ze środowiska Windows PowerShell dla StorSimple za pomocą konsoli szeregowego urządzenia

Możesz [pobrać Kit](http://www.putty.org/) lub podobne terminal emulacji oprogramowania do łączenia z programu Windows PowerShell dla StorSimple. Musisz skonfigurować Kit specjalnie, aby uzyskać dostęp do urządzenia Microsoft Azure StorSimple. Następujące tematy zawierają uzyskać szczegółowe instrukcje dotyczące konfigurowania Kit i połączyć się z urządzeniem. Także opisano różne opcje menu w konsoli szeregowego.

### <a name="putty-settings"></a>Ustawienia Kit

Upewnij się, użyj poniższych ustawień Kit do łączenia się z interfejsem programu Windows PowerShell z konsoli szeregowego.

#### <a name="to-configure-putty"></a>Aby skonfigurować Kit

1. W oknie dialogowym Kit **zmiany konfiguracji** w okienku **Kategoria** wybierz **klawiatury**.

2. Upewnij się, że wybierane są następujące opcje (są ustawienia domyślne po rozpoczęciu nowej sesji). 

  	|Element klawiatury|Wybierz pozycję|
  	|---|---|
  	|Klawisz BACKSPACE|Kontrola-? (127)|
  	|Dla użytkowników domowych i Zakończ klawiszy|Standardowe|
  	|Klawisze funkcyjne i klawiatury|ESC [n ~|
  	|Początkowy stan klawisze kursora|Normalny|
  	|Początkowy stan numerycznej|Normalny|
  	|Włączanie funkcji dodatkowej klawiatury|Control Alt różni się od AltGr|

    ![Obsługiwane ustawienia Kit](./media/storsimple-windows-powershell-administration/IC740877.png)

3. Kliknij przycisk **Zastosuj**.

4. W okienku **Kategoria** wybierz pozycję **Tłumaczenie**.

5. W polu listy **zestaw znaków Remote** wybierz **UTF-8**.

6. W obszarze **Obsługa znaków Rysowanie linii**zaznacz **punkty kodowe Unicode Użyj Rysowanie linii**. Na poniższej ilustracji przedstawiono poprawne pozycje Kit.

    ![Kit UTF ustawienia](./media/storsimple-windows-powershell-administration/IC740878.png)

7. Kliknij przycisk **Zastosuj**.


Kit umożliwia teraz łączenie do konsoli szeregowego urządzenia, wykonując następujące czynności.

[AZURE.INCLUDE [storsimple-use-putty](../../includes/storsimple-use-putty.md)]


### <a name="about-the-serial-console"></a>Informacje o konsoli szeregowego

Podczas uzyskiwania dostępu do programu Windows PowerShell interfejsu urządzenia StorSimple za pomocą konsoli szeregowego, wiadomość transparentu są prezentowane, a po nim opcje menu. 

Wiadomość Transparent zawiera podstawowe informacje urządzenia StorSimple, takie jak model, nazwa, zainstalowana wersja oprogramowania i stan kontroler, który chcesz uzyskać dostęp. Poniższa ilustracja przedstawia przykładową wiadomość transparentu.

![Liczba kolejna transparentem](./media/storsimple-windows-powershell-administration/IC741098.png)

>[AZURE.IMPORTANT] Komunikat w postaci transparentu służy do stwierdzenia, czy kontroler, które są połączeni z jest aktywne lub pasywne.

Na poniższej ilustracji przedstawiono różnych opcji działania, które są dostępne w menu Konsola szeregowa.

![Zarejestruj się w urządzeniu 2](./media/storsimple-windows-powershell-administration/IC740906.png)

Dostępne są następujące ustawienia:

1. **Zaloguj się przy użyciu pełnego dostępu** Ta opcja umożliwia nawiązywanie połączenia (za pomocą odpowiednich poświadczeń) do działania **SSAdminConsole** na lokalnym kontrolerze. (Lokalny kontroler jest kontrolerze, które mają obecnie dostęp do za pomocą konsoli szeregowego urządzenia StorSimple). Ta opcja może także używany w celu umożliwienia Microsoft Support dostęp nieograniczony działania (sesji pomocy technicznej) rozwiązywać problemy z dowolnego urządzenia możliwych do. Zaloguj się przy użyciu opcja 1, możesz zezwolić odtwarzania Support firmy Microsoft uzyskać dostęp do działania bez ograniczeń, uruchamiając określonego polecenia cmdlet. Aby uzyskać szczegółowe informacje zapoznaj się z [Rozpoczynanie sesji pomocy technicznej](storsimple-contact-microsoft-support.md#start-a-support-session-in-windows-powershell-for-storsimple).

2. **Zaloguj się na kontrolerze równorzędnych z pełnym dostępem** Ta opcja jest tak samo, jak opcja 1, z wyjątkiem, że możesz połączyć się (z odpowiednich poświadczeń) do działania **SSAdminConsole** na kontrolerze równorzędnych. Ponieważ urządzenie StorSimple jest wysokiej dostępności z dwoma kontrolerami w konfiguracji pasywne aktywne, funkcja odwołuje się do innego kontrolera w urządzenie, którego chcesz uzyskać dostęp za pomocą konsoli szeregowego).
Podobnie jak opcja 1, ta opcja może również umożliwia Microsoft Support dostęp nieograniczony działania na kontrolerze równorzędnych do.

3. **Nawiązywanie połączenia z ograniczonym dostępem** Ta opcja służy do interfejsu programu Windows PowerShell w trybie ograniczony dostęp. Nie zostanie wyświetlony monit o poświadczenia dostępu. Ta opcja umożliwia nawiązywanie większym ograniczeniem działania w porównaniu z opcji 1 i 2.  Niektóre zadania, które są dostępne za pomocą opcji 1 który **nie* można wykonać w tym działania są:

    - Przywracanie ustawień fabrycznych
    - Zmienianie hasła
    - Włączanie lub wyłączanie dostępu pomocy technicznej
    - Stosowanie aktualizacji
    - Instalowanie poprawki 
                                                

    >[AZURE.NOTE] **Jest to preferowaną opcję, jeśli pamiętasz hasła administratora urządzenia i nie można połączyć za pomocą opcji 1 lub 2.**

4. **Zmiana języka** Ta opcja pozwala zmienić język wyświetlania interfejsu programu Windows PowerShell. Języki obsługiwane są w języku angielskim, japońskim, rosyjski, francuski, koreański południe, hiszpański, włoski, niemiecki, chiński i portugalski (Brazylia).


## <a name="connect-remotely-to-storsimple-using-windows-powershell-for-storsimple"></a>Nawiązanie połączenia zdalnego StorSimple przy użyciu programu Windows PowerShell dla StorSimple

Zdalne programu Windows PowerShell umożliwia łączenie się z urządzeniem StorSimple. Po podłączeniu w ten sposób nie zobaczą menu. (Wyświetlony menu tylko wtedy, gdy używasz konsoli szeregowego na tym urządzeniu do łączenia. Łączenie zdalnie przejście bezpośrednio do odpowiednika "opcja 1 — pełny dostęp" na konsoli szeregowego.) Zdalne programu Windows PowerShell możesz połączyć się określone działania. Można także określić język wyświetlania. 

Język wyświetlania jest niezależny od języka, którego można ustawić przy użyciu opcji **Zmień język** w menu Konsola szeregowa. Ustawienia regionalne urządzenia nawiązujesz Jeśli nie określono automatycznie użyje zdalnej obsługi programu PowerShell.

>[AZURE.NOTE] Jeśli pracujesz z hosts wirtualna Microsoft Azure i urządzeń wirtualnych StorSimple, umożliwia zdalnej programu Windows PowerShell i hosta wirtualnego połączyć się z urządzeniem wirtualną. Jeśli masz skonfigurowane lokalizację udziału na hoście, na którym chcesz zapisać informacje z sesji środowiska Windows PowerShell, należy pamiętać, że główne Wszyscy zawiera tylko uwierzytelnieni użytkownicy. W związku z tym jeśli skonfigurowano Udostępnij zezwolić na dostęp przez wszystkich użytkowników i łączenie bez określania poświadczeń, nieuwierzytelnionych kapitału anonimowe będą używane i zostanie wyświetlony komunikat o błędzie. Aby rozwiązać ten problem, w udziale hosta możesz musi włączyć konta gościa i następnie nadaj gościa pełny dostęp do udziału lub należy określić prawidłowe poświadczenia wraz z polecenia cmdlet programu Windows PowerShell.

Za pomocą protokołu HTTP lub HTTPS do łączenia za pomocą programu Windows PowerShell zdalnych. Skorzystaj z instrukcji w następujące samouczki:

- [Nawiązywanie połączenia przy użyciu protokołu HTTP](storsimple-remote-connect.md#connect-through-http)
- [Nawiązywanie połączenia przy użyciu protokołu HTTPS](storsimple-remote-connect.md#connect-through-https)

## <a name="connection-security-considerations"></a>Zagadnienia dotyczące zabezpieczeń połączenia

Wybierając sposobu łączenia się programu Windows PowerShell dla StorSimple, należy rozważyć następujące kwestie:

- Nawiązywanie połączenia bezpośrednio do konsoli szeregowego urządzenia są bezpieczne, ale połączenie z konsoli szeregowego za pośrednictwem sieci przełączniki nie jest. Należy zachować ostrożność z ryzyko związane z zabezpieczeniami podczas łączenia się urządzenia szeregowego przez sieć przełączniki.

- Nawiązywanie połączenia za pośrednictwem sesji protokołu HTTP mogą oferować wyższy poziom zabezpieczeń niż połączenie za pomocą konsoli szeregowego za pośrednictwem. Mimo że nie jest najbezpieczniejsza metoda, dopuszczalne jest w zaufanych sieciach.

- Łączenie za pośrednictwem sesji HTTPS jest najbezpieczniejsza i zalecana opcja.


## <a name="administer-your-storsimple-device-using-windows-powershell-for-storsimple"></a>Administrowanie urządzenia StorSimple przy użyciu programu Windows PowerShell dla StorSimple
W poniższej tabeli przedstawiono podsumowanie wszystkich zadań administracyjnych i złożone przepływy pracy, które mogą być wykonywane w ramach interfejsu programu Windows PowerShell urządzenia StorSimple. Aby uzyskać więcej informacji na temat każdego przepływu pracy kliknij odpowiednią pozycję w tabeli.

#### <a name="windows-powershell-for-storsimple-workflows"></a>Windows PowerShell dla StorSimple przepływów pracy

|Jeśli chcesz to zrobić...|Wykonaj poniższą procedurę.|
|---|---|
|Zarejestruj się w urządzeniu|[Konfigurowanie i zarejestrować urządzenia przy użyciu programu Windows PowerShell dla StorSimple](storsimple-deployment-walkthrough.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple) |
|Konfigurowanie serwera proxy sieci web</br>Wyświetlanie ustawień serwera proxy w sieci web|[Konfigurowanie serwera proxy sieci web dla swojego urządzenia StorSimple](storsimple-configure-web-proxy.md)|
|Modyfikowanie ustawień interfejsu 0 sieci danych na urządzeniu|[Modyfikowanie danych 0 sieciowej dla swojego urządzenia StorSimple](storsimple-modify-data-0.md)|
|Zatrzymywanie kontrolera </br> Ponownie uruchomić lub zamknąć kontrolera </br> Zamknij urządzenie</br>Przywrócić domyślne ustawienia fabryczne urządzenia|[Zarządzanie kontrolery urządzeń](storsimple-manage-device-controller.md)|
|Zainstaluj aktualizacje tryb konserwacji i poprawek|[Aktualizowanie urządzenia](storsimple-update-device.md)|
|Przejście do trybu konserwacji </br>Zamknij tryb konserwacji|[Tryby urządzenia StorSimple](storsimple-device-modes.md)|
|Tworzenie pakietu pomocy technicznej</br>Odszyfrowywanie i Edytuj pakiet pomocy technicznej|[Tworzenie i zarządzanie nimi pakiet pomocy technicznej](storsimple-create-manage-support-package.md)|
|Rozpoczynanie sesji pomocy technicznej</br>|[Rozpoczynanie sesji pomocy technicznej w programie Windows PowerShell dla StorSimple](/storsimple-contact-microsoft-support.md#start-a-support-session-in-windows-powershell-for-storsimple)
 

## <a name="get-help-in-windows-powershell-for-storsimple"></a>Uzyskaj pomoc w programie Windows PowerShell dla StorSimple

W programie Windows PowerShell dla StorSimple polecenia cmdlet pomoc jest dostępna. Online, aktualne wersji Pomocy jest również dostępne, którym można zaktualizować pomocy w systemie.

Uzyskiwanie pomocy w tym interfejsie jest podobna do tej w programie Windows PowerShell i większość polecenia cmdlet związane z pomocy będą działać. Pomoc dla programu Windows PowerShell online można znaleźć w bibliotece TechNet: [Tworzenie skryptów w programie Windows PowerShell](http://go.microsoft.com/fwlink/?LinkID=108518).

Oto krótki opis typów pomocy dla tego interfejsu programu Windows PowerShell, w tym jak zaktualizować pomocy.

#### <a name="to-get-help-for-a-cmdlet"></a>Aby uzyskać pomoc dotyczącą polecenia cmdlet

- Aby uzyskać pomoc dotyczącą funkcji lub polecenia cmdlet, użyj następującego polecenia:`Get-Help <cmdlet-name>`

- Aby uzyskać pomoc online dla dowolnego polecenia cmdlet, należy użyć polecenia cmdlet poprzedniego z `-Online` parametr:`Get-Help <cmdlet-name> -Online`

- Aby uzyskać pełną pomoc, można użyć `–Full` parametru i przykłady, użyj `–Examples` parametru.

#### <a name="to-update-help"></a>Aby zaktualizować — pomoc

Można łatwo aktualizować pomocy w interfejsie programu Windows PowerShell. Wykonaj poniższe czynności, aby zaktualizować pomocy w systemie.

#### <a name="to-update-cmdlet-help"></a>Aby zaktualizować polecenia cmdlet — pomoc

1. Uruchom program Windows PowerShell z opcją **Uruchom jako administrator** .

1. W wierszu polecenia wpisz: `Update-Help`

1. Zaktualizowane pliki pomocy zostanie zainstalowana.

1. Po zainstalowaniu plików pomocy, wpisz: `Get-Help Get-Command`. Spowoduje to wyświetlenie listy poleceń cmdlet, dla którego jest dostępna Pomoc.


>[AZURE.NOTE] Aby uzyskać listę wszystkich dostępnych poleceń cmdlet w działania, zaloguj się do odpowiedniej opcji menu i uruchom `Get-Command` polecenia cmdlet.

## <a name="next-steps"></a>Następne kroki
Jeśli napotkasz jakiekolwiek problemy z urządzeniem StorSimple, wykonując jedną z powyższych przepływy pracy, zapoznaj się z [Narzędzia do rozwiązywania problemów z StorSimple wdrożenia](storsimple-troubleshoot-deployment.md#tools-for-troubleshooting-storsimple-deployments).

