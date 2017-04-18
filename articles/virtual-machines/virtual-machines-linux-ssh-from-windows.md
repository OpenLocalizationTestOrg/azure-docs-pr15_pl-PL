<properties 
    pageTitle="Używanie klawiszy SSH z systemu Windows dla maszyny wirtualne Linux | Microsoft Azure" 
    description="Dowiedz się, jak Generowanie i użyj klawiszy SSH na komputerze z systemem Windows, aby połączyć się z komputerem wirtualnych Linux Azure." 
    services="virtual-machines-linux" 
    documentationCenter="" 
    authors="squillace" 
    manager="timlt" 
    editor=""
    tags="azure-service-management,azure-resource-manager" />

<tags 
    ms.service="virtual-machines-linux" 
    ms.workload="infrastructure-services" 
    ms.tgt_pltfrm="vm-linux" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="rasquill"/>

# <a name="how-to-use-ssh-keys-with-windows-on-azure"></a>Jak klawiszy SSH korzystanie z systemu Windows Azure

> [AZURE.SELECTOR]
- [Systemu Windows](virtual-machines-linux-ssh-from-windows.md)
- [Linux Mac](virtual-machines-linux-mac-create-ssh-keys.md)

Podczas łączenia się Linux wirtualnych maszyn platformy Azure, należy użyć [kryptograficzny kluczy publicznych](https://wikipedia.org/wiki/Public-key_cryptography) umożliwi zabezpieczyć logować się do swojego maszyn wirtualnych Linux. Proces ten obejmuje wymianę kluczy publicznych i prywatnych przy użyciu polecenia bezpiecznego shell (SSH) do uwierzytelnienia użytkownika, a nie nazwę użytkownika i hasło. Hasła są podatne na siłowych atakami, szczególnie w Internecie maszyny wirtualne, takich jak serwery sieci web. Ten artykuł zawiera omówienie kluczy SSH i jak wygenerować odpowiednie klawisze na komputerze z systemem Windows.


## <a name="overview-of-ssh-and-keys"></a>Omówienie SSH oraz klawiszy

Można bezpiecznie zalogowaniu się do swojego Linux maszyn wirtualnych przy użyciu kluczy publicznych i prywatnych:

- **Klucz publiczny** jest umieszczony na maszyn wirtualnych usługi Linux lub innej usługi, które mają być używane z kryptograficzny kluczy publicznych.
- **Klucz prywatny** jest, co prezentowania maszyn wirtualnych usługi Linux po zalogowaniu się, w celu zweryfikowania tożsamości. Chroń klucz prywatny. Nie udostępniaj go.

Klucze publiczne i prywatne można używać w wielu usługach i maszyny wirtualne. Nie ma potrzeby para kluczy dla każdego maszyn wirtualnych lub usługę, którą chcesz mieć dostęp. Aby uzyskać bardziej szczegółowe omówienie zobacz [kryptograficzny kluczy publicznych](https://wikipedia.org/wiki/Public-key_cryptography).

SSH jest protokół szyfrowanego połączenia, który umożliwia bezpiecznego logowania połączeniach niezabezpieczoną. Jest domyślnym protokołem połączenia dla maszyny wirtualne Linux obsługiwany w Azure. Chociaż SSH samej zaszyfrowanego połączenia, przy użyciu hasła z połączeniami SSH nadal pozostawia maszyn wirtualnych podatne na siłowych atakami lub odgadnięcie haseł. Bezpieczniejsze i preferowane, metodę łączenia się przy użyciu SSH maszyny jest za pomocą tych kluczy publicznych i prywatnych, nazywane też klawiszami SSH.

Jeśli nie chcesz za pomocą klawiszy SSH, można nadal zalogowaniu się do swojego maszyny wirtualne Linux przy użyciu hasła. Jeśli do maszyn wirtualnych nie jest udostępniana w Internecie, może być wystarczające przy użyciu hasła. Jednak musisz nadal zarządzać hasła dla każdego maszyn wirtualnych Linux i obsługa zasady haseł prawidłowy i wskazówki, takie jak minimalną długość hasła i regularne aktualizowanie ich. Użyj klawiszy SSH zmniejsza złożoność zarządzania poszczególnych poświadczeń w wielu maszyny wirtualne.


## <a name="windows-packages-and-ssh-clients"></a>Pakiety systemu Windows i SSH klientów

Nawiązywanie połączenia i zarządzanie maszyny wirtualne Linux platformy Azure za pomocą klienta **ssh** . Komputerach z systemem Windows nie zwykle zainstalowano **ssh** klienta. Typowe Windows SSH klientów, z którymi możesz zainstalować znajdują się w następujących pakietach:

- [Cyfra dla systemu Windows](https://git-for-windows.github.io/)
- [Kit](http://www.chiark.greenend.org.uk/~sgtatham/putty/)
- [MobaXterm](http://mobaxterm.mobatek.net/)
- [Programów Cygwin](https://cygwin.com/)

> [AZURE.NOTE] Najnowsza aktualizacja systemu Windows 10 rocznicy zawiera urodzinową dla systemu Windows. Ta funkcja umożliwia uruchamianie podsystemu Windows Linux i access narzędzi, takich jak SSH klienta. Urodzinową dla systemu Windows jest nadal w fazie projektowania i jest traktowany jako wersji beta. Aby uzyskać więcej informacji na temat urodzinową dla systemu Windows zobacz [urodzinową na Ubuntu w systemie Windows](https://msdn.microsoft.com/commandline/wsl/about).


## <a name="which-key-files-do-you-need-to-create"></a>Pliki, które klucza należy utworzyć?

Azure wymaga co najmniej 2048-bitowy, **ssh-rsa** formatowanie klucze publiczne i prywatne. Jeśli zarządzasz Azure zasobów za pomocą modelu wdrożenia klasyczny, należy również do generowania PEM (`.pem` pliku).

Poniżej przedstawiono scenariusze wdrażania i typy plików, które w każdej:

1. klucze **SSH-rsa** są wymagane dla każdego wdrożenia za pomocą [Azure portal](https://portal.azure.com)i wdrożenia Menedżera zasobów za pomocą [Interfejsu wiersza polecenia Azure](../xplat-cli-install.md).
    - Klucze są zwykle, że potrzebujesz wszystkich większość osób.
2. `.pem`plik jest wymagany do utworzenia maszyny wirtualne za pomocą [portalu klasyczny](https://manage.windowsazure.com). Klucze są również obsługiwane w klasycznym wdrożonych za pomocą [Interfejsu wiersza polecenia Azure](../xplat-cli-install.md).
    - Musisz utworzyć te certyfikaty i dodatkowe klawisze, jeśli zarządzasz zasobów utworzony przy użyciu modelu wdrożenia klasyczny.


## <a name="install-git-for-windows"></a>Instalowanie cyfra dla systemu Windows

Poprzedniej sekcji na liście kilku pakietach, które zawierają `openssl` narzędzie dla systemu Windows. To narzędzie jest potrzebne do utworzenia kluczy publicznych i prywatnych. Poniższe przykłady szczegółowo sposobu instalacji i używania **Cyfra dla systemu Windows**, ale możesz wybrać niezależnie od pakietu wolisz. **Cyfra dla systemu Windows** umożliwia dostęp do narzędzia dodatkowe oprogramowanie open source ([OSS](https://en.wikipedia.org/wiki/Open-source_software)) i narzędzi, które mogą być przydatne podczas pracy z maszyny wirtualne Linux.

1. Pobieranie i instalowanie **Cyfra dla systemu Windows** z następującej lokalizacji: [https://git-for-windows.github.io/](https://git-for-windows.github.io/).

2. Zaakceptuj wartości domyślne podczas instalacji, chyba że należy je zmienić.

3. Uruchamianie **Imprezie cyfra** z **Start Menu** > **cyfra** > **Imprezie cyfra**. Konsola będzie podobny do następującego przykładu:

    ![Powłoki cyfra dla urodzinową systemu Windows](./media/virtual-machines-linux-ssh-from-windows/git-bash-window.png)


## <a name="create-a-private-key"></a>Tworzenie klucz prywatny

1. W oknie **Imprezie cyfra** , za pomocą `openssl.exe` utworzyć klucz prywatny. W poniższym przykładzie tworzony klucz o nazwie `myPrivateKey` i certyfikat o nazwie `myCert.pem`:

    ```bash
    openssl.exe req -x509 -nodes -days 365 -newkey rsa:2048 \
        -keyout myPrivateKey.key -out myCert.pem
    ```

    Wynik będzie podobny do następującego przykładu:

    ```bash
    Generating a 2048 bit RSA private key
    .......................................+++
    .......................+++
    writing new private key to 'myPrivateKey.key'
    -----
    You are about to be asked to enter information that will be incorporated
    into your certificate request.
    What you are about to enter is what is called a Distinguished Name or a DN.
    There are quite a few fields but you can leave some blank
    For some fields there will be a default value,
    If you enter '.', the field will be left blank.
    -----
    Country Name (2 letter code) [AU]:
    ```

2. Odbierz monity nazwa kraju, lokalizacja, nazwa organizacji itd.

3. Nowy klucz prywatny i certyfikat usługi są tworzone w bieżącym katalogu roboczym. Dla najważniejsze wskazówki dotyczące zabezpieczeń należy ustawić uprawnienia na klucz prywatny tak, aby tylko można do niego dostęp:

    ```bash
    chmod 0600 myPrivateKey
    ```

4. Jeśli musisz także zarządzanie zasobami klasyczny, konwertowanie `myCert.pem` do `myCert.cer` (X509 zakodowany w formacie DER certyfikatu). Wykonać ten krok opcjonalny tylko wtedy, gdy trzeba specjalnie zarządzania zasobami starsze klasyczny. 

    Konwertowanie certyfikat przy użyciu następującego polecenia:

    ```bash
    openssl.exe  x509 -outform der -in myCert.pem -out myCert.cer
    ```

## <a name="create-a-private-key-for-putty"></a>Tworzenie klucz prywatny dla Kit

Kit jest klientem SSH wspólne dla systemu Windows. Mogą do dowolnego SSH klienta, którego chcesz użyć. Aby użyć Kit, należy utworzyć dodatkowy typ klucza - prywatny klucz Kit (PPK). Jeśli nie chcesz używać Kit, należy pominąć tę sekcję.

Poniższy przykład tworzy dodatkowe klucz prywatny specjalnie dla Kit umożliwia:

1. **Cyfra imprezie** umożliwia konwertowanie RSA klucz prywatny, który mogą zrozumieć PuTTYgen klucz prywatny. W poniższym przykładzie tworzony klucz o nazwie `myPrivateKey_rsa` z istniejącego klucza o nazwie `myPrivateKey`:

    ```bash
    openssl rsa -in ./myPrivateKey.key -out myPrivateKey_rsa
    ```

    Dla najważniejsze wskazówki dotyczące zabezpieczeń należy ustawić uprawnienia na klucz prywatny tak, aby tylko można do niego dostęp:

    ```bash
    chmod 0600 myPrivateKey_rsa
    ```

2. Pobierz i uruchom PuTTYgen z następującej lokalizacji: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)

3. Kliknij menu: **plik** > **obciążenia klucz prywatny**

4. Znajdź klucz prywatny (`myPrivateKey_rsa` w poprzednim przykładzie). Katalog domyślny podczas uruchamiania **Imprezie cyfra** jest `C:\Users\%username%`. Zmienianie filtru pliku w celu wyświetlenia **wszystkie pliki (\*.\*)**:

    ![Ładowanie istniejący klucz prywatny do PuTTYgen](./media/virtual-machines-linux-ssh-from-windows/load-private-key.png)

5. Kliknij przycisk **Otwórz**. Monit wskazuje, że klucz został pomyślnie zaimportowany:

    ![Pomyślnie zaimportowane klucza PuTTYgen](./media/virtual-machines-linux-ssh-from-windows/successfully-imported-key.png)

6. Kliknij przycisk **OK** , aby zamknąć okno wiersza.

7. Klucz publiczny jest wyświetlany w górnej części okna **PuTTYgen** . Skopiować i wkleić ten klucz publiczny Azure portal lub Menedżer zasobów Azure szablonu podczas tworzenia maszyny Linux. Można też kliknąć przycisk **Zapisz klucz publiczny** można zapisać jego kopię na komputer:

    ![Zapisz plik Kit klucza publicznego](./media/virtual-machines-linux-ssh-from-windows/save-public-key.png)

    W poniższym przykładzie pokazano, jak chcesz skopiować i wkleić ten klucz publiczny do Azure portal, podczas tworzenia maszyny Linux. Klucz publiczny zazwyczaj jest następnie przechowywany w `~/.ssh/authorized_keys` na Twojej nowej maszyn wirtualnych.

    ![Po utworzeniu maszyny w portalu Azure za pomocą klucz publiczny](./media/virtual-machines-linux-ssh-from-windows/use-public-key-azure-portal.png)

7. Po powrocie do **PuTTYgen**, kliknij przycisk **Zapisz klucz prywatny**:

    ![Zapisz plik Kit klucz prywatny](./media/virtual-machines-linux-ssh-from-windows/save-ppk-file.png)

    > [AZURE.WARNING] Monit z pytaniem, czy chcesz kontynuować bez wprowadzania hasło dla klucza. Hasło przypomina hasło dołączone do klucz prywatny. Nawet jeśli ktoś były uzyskać klucz prywatny, są nadal będzie nie będą mogli przeprowadzać uwierzytelniania przy użyciu tylko klucza. Czy muszą hasło. Bez hasła Jeśli ktoś uzyskuje kluczem prywatnym ich można Zaloguj się do dowolnej maszyn wirtualnych lub usługa, która korzysta z tego klawisza. Zalecamy zapoznanie Utwórz hasło. Jeśli zapomnisz hasło, istnieje jednak nie można go odzyskać.

    Jeśli chcesz wprowadzić hasło, kliknij przycisk **nie**, wprowadź hasło w oknie głównym PuTTYgen, a następnie ponownie kliknij przycisk **Zapisz klucz prywatny** . W przeciwnym razie kliknij przycisk **Tak,** aby kontynuować bez podania hasła opcjonalne.

8. Wprowadź nazwę i lokalizację do zapisania pliku PPK.


## <a name="use-putty-to-ssh-to-a-linux-machine"></a>Używanie Kit do SSH do komputera Linux

Ponownie Kit jest klientem SSH wspólne dla systemu Windows. Mogą do dowolnego SSH klienta, którego chcesz użyć. Poniższe kroki szczegółowo jak za pomocą uwierzytelniania maszyn wirtualnych usługi Azure, przy użyciu SSH klucz prywatny. Czynności są podobne w innych SSH najważniejszych klientów pod względem konieczności ładowanie klucz prywatny do uwierzytelnienia połączenia SSH.

1. Plik do pobrania i uruchamianie Kit z następującej lokalizacji: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)

2. Wypełnij pola Nazwa hosta lub adres IP swojego maszyn wirtualnych z portalu Azure:

    ![Otwórz nowe połączenie Kit](./media/virtual-machines-linux-ssh-from-windows/putty-new-connection.png)

3. Przed wybraniem **otwarty**, kliknij pozycję **połączenie** > **SSH** > kartę**uwierzytelniania** . Przejdź do i wybierz klucz prywatny:

    ![Wybierz swój klucz prywatny Kit uwierzytelniania](./media/virtual-machines-linux-ssh-from-windows/putty-auth-dialog.png)

4. Kliknij przycisk **Otwórz** nawiązywania połączenia z komputera wirtualnych
 

## <a name="next-steps"></a>Następne kroki
Można również tworzyć kluczy publicznych i prywatnych [przy użyciu systemu OS X i Linux](virtual-machines-linux-mac-create-ssh-keys.md).

Aby uzyskać więcej informacji na temat urodzinową dla systemu Windows i korzyści OSS narzędzia dostępne na komputerze z systemem Windows zobacz [urodzinową na Ubuntu w systemie Windows](https://msdn.microsoft.com/commandline/wsl/about).

Jeśli masz problemy z używaniem SSH, aby nawiązać połączenie z maszyny wirtualne Linux, zobacz [Rozwiązywanie problemów z SSH połączeń maszyn wirtualnych Azure Linux](virtual-machines-linux-troubleshoot-ssh-connection.md).