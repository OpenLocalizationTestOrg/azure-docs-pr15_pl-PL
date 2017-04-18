<properties
    pageTitle="Jak utworzyć szablon niestandardowy dla Azure RemoteApp | Microsoft Azure"
    description="Dowiedz się, jak utworzyć szablon niestandardowy dla Azure RemoteApp. Za pomocą tego szablonu z kolekcji hybrydowego lub w chmurze."
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016" 
    ms.author="elizapo"/>

# <a name="how-to-create-a-custom-template-image-for-azure-remoteapp"></a>Jak utworzyć szablon niestandardowy dla Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp jest są przerywane. W tym artykule [anons](https://go.microsoft.com/fwlink/?linkid=821148) , aby uzyskać szczegółowe informacje.

Azure RemoteApp używa obraz szablonu Windows Server 2012 R2, aby obsługiwać wszystkie programy, które chcesz udostępnić innym użytkownikom. Aby utworzyć obraz niestandardowy szablon RemoteApp, możesz rozpocząć z istniejącego obrazu lub Utwórz nowy. 


> [AZURE.TIP] Czy wiesz, że można utworzyć obrazu z maszyn wirtualnych Azure? Artykuł PRAWDA, a ogranicza ilość czasu go ma do zaimportowania obrazu. Zapoznaj się z instrukcjami [poniżej](remoteapp-image-on-azurevm.md).

Wymagania dotyczące obrazu, który można przekazywać do użytku z Azure RemoteApp są:


- Rozmiar obrazu powinny być wielokrotnością MB. Jeśli spróbujesz przekazywanie obrazu, który nie jest wielokrotnością, przekazywanie nie powiedzie się.
- Rozmiar obrazu musi być 127 GB lub mniej.
- Musi znajdować się na plik wirtualnego dysku twardego (VHDX pliki [Hyper-V wirtualnych dyskach twardych] nie są obecnie obsługiwane).
- Wirtualny dysk twardy nie może być maszyny wirtualnej Generowanie 2.
- Wirtualny dysk twardy może być stałym rozmiarze lub dynamicznie powiększających. Dynamicznie powiększające wirtualny dysk twardy jest zalecana, ponieważ trwa mniej czasu na przekazanie do Azure niż stałym rozmiarze pliku wirtualnego dysku twardego.
- Dysk musi być zainicjowany przy użyciu rekordu uruchamiania główny (MBR) podziału styl. Identyfikator GUID partition tabeli (GPT) partition stylu nie jest obsługiwane.
- Wirtualny dysk twardy musi zawierać jednej instalacji systemu Windows Server 2012 R2. Może zawierać przypisaną, ale tylko jeden zawierających instalacji systemu Windows.
- Musi być zainstalowany roli hosta zdalnej sesji pulpitu (RDSH) i funkcję Środowisko pulpitu.
- Musisz roli Broker połączeń pulpitu zdalnego *nie* jest zainstalowany.
- Musi być wyłączony (szyfrowania plików systemowych).
- Obraz musi być przetworzonej przez program Sysprep przy użyciu parametrów **/oobe / generalize/shutdown** (nie użyty parametr **/mode:vm** ).
- Przekazywanie do wirtualnego dysku twardego z łańcucha migawki nie jest obsługiwane.


**Przed rozpoczęciem**

Należy wykonać następujące czynności przed utworzeniem usługi:

- [Utwórz konto](https://azure.microsoft.com/services/remoteapp/) w usłudze RemoteApp.
- Tworzenie konta użytkownika w usłudze Active Directory ma zostać użyte jako konto usługi RemoteApp. Ograniczyć uprawnienia dla tego konta, tak aby tylko można dołączyć komputerów do domeny. Aby uzyskać więcej informacji, zobacz [Konfigurowanie usługi Azure Active Directory dla RemoteApp](remoteapp-ad.md) .
- Zbieranie informacji o sieci lokalnej: adres IP, informacje i szczegóły urządzenia VPN.
- Zainstaluj moduł [Azure programu PowerShell](../powershell-install-configure.md) .
- Gromadzenie informacji o użytkownikach, których chcesz udzielić dostępu do. Może to być informacje o koncie Microsoft i informacje o koncie pracy usługi Active Directory dla użytkowników.



## <a name="create-a-template-image"></a>Utwórz obraz szablonu ##

Oto wysokiego poziomu czynności, aby utworzyć nowy obraz szablonu od podstaw:

1.  Zlokalizuj obraz systemu Windows Server 2012 R2 aktualizacji DVD lub ISO.
2.  Tworzenie pliku wirtualnego dysku twardego.
4.  Instalowanie systemu Windows Server 2012 R2.
5.  Zainstaluj rolę hosta zdalnej sesji pulpitu (RDSH) i funkcję Środowisko pulpitu.
6.  Zainstaluj dodatkowe funkcje wymagane przez aplikacje.
7.  Instalowanie i konfigurowanie aplikacji. Aby ułatwić udostępniania aplikacji, Dodaj wszelkie aplikacje lub programy, które chcesz udostępnić do menu **Start** obrazu, szczególnie w **%systemdrive%\ProgramData\Microsoft\Windows\Start Menu\Programs.
8.  Wykonaj wymagane przez aplikacje dodatkowe konfiguracji systemu Windows.
9.  Wyłączanie systemu szyfrowania plików (EFS).
10. **Wymagane:** Przejdź do strony Windows Update i zainstaluj wszystkie ważne aktualizacje.
9.  SYSPREP obrazu.

Szczegółowe informacje na temat tworzenia nowego obrazu są:

1.  Zlokalizuj obraz systemu Windows Server 2012 R2 aktualizacji DVD lub ISO.
2.  Tworzenie pliku wirtualnego dysku twardego przy użyciu funkcji Zarządzanie dysku.
    1.  Uruchamianie zarządzania dysku (diskmgmt.msc).
    2.  Utwórz dynamicznie powiększających wirtualny dysk twardy 40 GB lub więcej rozmiar. (Wymaganą ilość miejsca na potrzeby systemu Windows, aplikacji i dostosowania oszacować. Windows Server z roli RDSH i zainstalowano funkcję Środowisko pulpitu wymaga 10 GB miejsca).
        1.  Kliknij pozycję **akcji > tworzenia wirtualnego dysku twardego**.
        2.  Określ lokalizację, rozmiar i format wirtualnego dysku twardego. Zaznacz **dynamiczne rozwijanie**, a następnie kliknij **przycisk OK**.

            Zostanie uruchomiony przez kilka sekund. Po zakończeniu tworzenia wirtualnego dysku twardego powinna być widoczna nowego dysku bez dowolną literę i "nie" zainicjowane w konsoli zarządzania dysku.

        - Kliknij prawym przyciskiem myszy dysku (bez spacji nieprzydzielone), a następnie kliknij **Inicjowania dysku**. Wybierz pozycję **MBR** (rekord uruchamiania wzorzec) jako stylu partition, a następnie kliknij **przycisk OK**.
        - Tworzenie nowego woluminu: kliknij prawym przyciskiem myszy nieprzydzielone miejsce, a następnie kliknij **Nowy prosty**. Można zaakceptować wartości domyślne w kreatorze, ale upewnij się, że Przypisywanie litery dysku, aby uniknąć potencjalnych problemów podczas ładowania obraz szablonu.
        - Kliknij prawym przyciskiem myszy dysk, a następnie kliknij **Odłączanie wirtualnego dysku twardego**.





1. Instalowanie systemu Windows Server 2012 R2:
    1. Tworzenie nowej maszyny wirtualnej. Za pomocą Kreatora nowych w funkcji Hyper-V menedżera lub klienta funkcji Hyper-V.
        1. Na stronie Określanie Generowanie wybierz **Generowanie 1**.
        2. Na stronie Połącz wirtualny dysk twardy zaznacz pole wyboru **Użyj istniejący wirtualny dysk twardy**, a następnie przejdź do wirtualnego dysku twardego utworzony w poprzednim kroku.
        2. Na stronie Opcje instalacji wybierz pozycję **Instalowanie z dysku CD/DVD-ROM uruchamiania systemu operacyjnego**, a następnie wybierz lokalizację nośnika instalacyjnego systemu Windows Server 2012 R2.
        3. Wybierz inne opcje w Kreatorze niezbędne do zainstalowania systemu Windows i aplikacji. Zakończyć działanie kreatora.
    2.  Po zakończeniu pracy Kreator edytować ustawienia maszyny wirtualnej i wprowadź niezbędne do zainstalowania systemu Windows i programów, takich jak liczba procesorów wirtualnych, inne zmiany, a następnie kliknij **przycisk OK**.
    4.  Łączenie maszyn wirtualnych i zainstaluj system Windows Server 2012 R2.
1. Zainstaluj rolę hosta zdalnej sesji pulpitu (RDSH) i funkcję Środowisko pulpitu:
    1. Uruchamianie Menedżera serwera.
    2. Na ekranie powitalnym lub z menu **Zarządzaj** , kliknij pozycję **Dodaj role i funkcje** .
    3. Kliknij przycisk **Dalej** na stronie przed rozpoczęciem.
    4. Wybrana **Instalacja oparta na rolach lub funkcji**, a następnie kliknij przycisk **Dalej**.
    5. Z listy wybierz komputer lokalny, a następnie kliknij przycisk **Dalej**.
    6. Zaznacz **Usługami pulpitu zdalnego**, a następnie kliknij przycisk **Dalej**.
    7. Rozwiń **interfejsów użytkownika oraz infrastrukturę** i wybierz **Środowisko pulpitu**.
    8. Kliknij pozycję **Dodaj funkcje**, a następnie kliknij przycisk **Dalej**.
    9. Na stronie usługi pulpitu zdalnego kliknij przycisk **Dalej**.
    10. Kliknij przycisk **hosta sesji pulpitu zdalnego**.
    11. Kliknij pozycję **Dodaj funkcje**, a następnie kliknij przycisk **Dalej**.
    12. Na stronie Potwierdzanie opcji instalacji wybierz pozycję **Uruchom ponownie docelowym serwerze automatycznie, jeśli jest to wymagane**, a następnie kliknij **Tak** na ostrzeżenie o ponowne uruchomienie komputera.
    13. Kliknij przycisk **Zainstaluj**. Komputer uruchomi się ponownie.
1.  Zainstaluj dodatkowe funkcje wymagane przez aplikacje, takie jak program .NET Framework 3.5. Aby zainstalować funkcje, uruchom Kreatora funkcje i Dodaj role.
7.  Instalowanie i konfigurowanie programów i aplikacji, którą chcesz opublikować przy użyciu funkcji RemoteApp.

>[AZURE.IMPORTANT]
>
>Zainstaluj rolę RDSH przed zainstalowaniem aplikacji, aby upewnić się, zanim obrazu jest przekazane do funkcji RemoteApp wykrycia problemów ze zgodnością aplikacji.
>
>Upewnij się, że skrót do aplikacji (pliku**.lnk** ) jest wyświetlany w menu **Start** dla wszystkich użytkowników (%systemdrive%\ProgramData\Microsoft\Windows\Start Menu\Programs). Również upewnij się, że ikony, które są widoczne w **Start** menu ma być widoczny dla użytkowników. Jeśli nie, zmień go. (Nie *ma* Aby dodać aplikację do początku menu, ale ułatwia publikowanie aplikacji w RemoteApp. W przeciwnym razie musisz podać ścieżka instalacji aplikacji podczas publikowania aplikacji.)


8.  Wykonaj wymagane przez aplikacje dodatkowe konfiguracji systemu Windows.
9.  Wyłączanie systemu szyfrowania plików (EFS). W oknie polecenia z podwyższonym poziomem uprawnień, uruchom następujące polecenie:

        Fsutil behavior set disableencryption 1

    Można także ustawić lub Dodaj następującą wartość DWORD rejestru:

        HKLM\System\CurrentControlSet\Control\FileSystem\NtfsDisableEncryption = 1
9.  Jeśli tworzysz obrazu wewnątrz Azure maszyn wirtualnych, zmienianie nazwy ** \%windir%\Panther\Unattend.xml** pliku i jak to blokuje skryptu wysyłania późniejszego użycia w pracy. Zmień nazwę tego pliku na Unattend.old tak, aby nadal będziesz mieć plik w przypadku, gdy chcesz przywrócić wdrożenia.
10. Przejdź do strony Windows Update i zainstaluj wszystkie ważne aktualizacje. Może być konieczne uruchomienie usługi Windows Update kilka razy, aby otrzymywać wszystkie aktualizacje. (Czasami zainstalowania aktualizacji, a tej samej aktualizacji wymagana jest aktualizacja).
10. SYSPREP obrazu. W wierszu polecenia z podwyższonym poziomem uprawnień uruchom następujące polecenie:

    **C:\Windows\System32\sysprep\sysprep.exe / generalize/oobe / shutdown**

    **Uwaga:** Nie należy używać przełącznika **/mode:vm** polecenia SYSPREP, mimo że jest to maszyny wirtualnej.


## <a name="next-steps"></a>Następne kroki ##
Teraz, gdy masz obraz szablonu niestandardowego, musisz przekazać ten obraz do kolekcji RemoteApp. Aby utworzyć zbiór, skorzystaj z informacji w następujących artykułach:


- [Jak utworzyć zbiór hybrydowych RemoteApp](remoteapp-create-hybrid-deployment.md)
- [Jak utworzyć zbiór RemoteApp w chmurze](remoteapp-create-cloud-deployment.md)
 