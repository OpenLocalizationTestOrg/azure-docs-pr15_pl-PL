<properties
    pageTitle="Instalowanie MongoDB na maszyn wirtualnych systemu Windows | Microsoft Azure"
    description="Dowiedz się, jak zainstalować MongoDB na maszyn wirtualnych Azure systemem Windows Server 2012 R2 utworzone za pomocą modelu wdrożenia Menedżera zasobów."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="iainfou"/>

# <a name="install-and-configure-mongodb-on-a-windows-vm-in-azure"></a>Instalowanie i konfigurowanie MongoDB maszyn wirtualnych systemu Windows platformy Azure
[MongoDB](http://www.mongodb.org) jest popularne Otwórz źródło, wysokiej wydajności NoSQL bazy danych. Ten artykuł prowadzi użytkownika przez instalowanie i konfigurowanie MongoDB na komputerze wirtualnych systemu Windows Server 2012 R2 (maszyn wirtualnych) platformy Azure. Możesz również [zainstalować MongoDB na maszyny Linux platformy Azure](virtual-machines-linux-install-mongodb.md).


## <a name="prerequisites"></a>Wymagania wstępne

Przed zainstalowaniem i konfigurowanie MongoDB, należy utworzyć maszyny i, najlepiej, Dodaj do niego dysku danych. Zobacz następujące artykuły, aby utworzyć maszyny i dodać dysku danych:

- [Tworzenie maszyn wirtualnych systemu Windows Server za pomocą portalu Azure](virtual-machines-windows-hero-tutorial.md) lub [Tworzenie maszyn wirtualnych systemu Windows Server przy użyciu programu PowerShell Azure](virtual-machines-windows-ps-create.md)
- [Dołącz dysku danych do maszyn wirtualnych systemu Windows Server za pomocą portalu Azure](virtual-machines-windows-attach-disk-portal.md) lub [Dołącz dysku danych do maszyn wirtualnych systemu Windows Server przy użyciu programu PowerShell Azure](https://msdn.microsoft.com/library/mt603673.aspx)
    
Aby rozpocząć MongoDB instalowania i konfigurowania, [Zaloguj się do maszyn wirtualnych usługi Windows Server](virtual-machines-windows-connect-logon.md) przy użyciu pulpitu zdalnego.


## <a name="install-mongodb"></a>Instalowanie MongoDB

> [AZURE.IMPORTANT] Funkcje zabezpieczeń MongoDB, takie jak uwierzytelnianie i powiązanie adresu IP, nie są domyślnie włączone. Funkcje zabezpieczeń powinna być włączona przed wdrożeniem MongoDB środowisku produkcyjnym. Aby uzyskać więcej informacji zobacz [MongoDB zabezpieczeń i uwierzytelniania](http://www.mongodb.org/display/DOCS/Security+and+Authentication).

1. Po podłączeniu do swojego maszyn wirtualnych za pomocą pulpitu zdalnego, Otwórz program Internet Explorer w menu **Start** na maszyn wirtualnych.

2. Wybierz pozycję **Użyj zalecanych zabezpieczenia, prywatność i ustawienia zgodności** , przy pierwszym otwarciu programu Internet Explorer, a następnie kliknij **przycisk OK**.

3. Konfiguracja zwiększonych zabezpieczeń programu Internet Explorer jest domyślnie włączony. Dodawanie witryny sieci Web MongoDB do listy dozwolonych witryn:

    - Wybierz ikonę **Narzędzia** w prawym górnym rogu.
    - W oknie dialogowym **Opcje internetowe**wybierz kartę **Zabezpieczenia** , a następnie wybierz ikonę **Zaufane witryny** .
    - Kliknij przycisk **witryny** . Dodawanie _https://\*. mongodb.org_ do listy zaufanych witryn, a następnie zamknij okno dialogowe.

    ![Konfigurowanie ustawień zabezpieczeń programu Internet Explorer](./media/virtual-machines-windows-install-mongodb/configure-internet-explorer-security.png)

4. Przejdź do strony [pobierania MongoDB -](http://www.mongodb.org/downloads) (http://www.mongodb.org/downloads).

5. Domyślnie należy zaznaczyć **Społeczności Server** edition i bieżącej najnowszej wersji stabilny dla systemu Windows Server 2008 R2 64-bitowej lub nowszy. Aby pobrać Instalatora, kliknij przycisk **Pobierz (msi)**.

    ![Pobierz instalatora MongoDB](./media/virtual-machines-windows-install-mongodb/download-mongodb.png)

    Uruchom Instalatora, po zakończeniu pobierania.

6. Przeczytaj i zaakceptuj umowę licencyjną. Gdy zostanie wyświetlony monit, wybierz pozycję Zainstaluj **wykonane** .

7. Na ostatnim ekranie kliknij przycisk **Zainstaluj**.


## <a name="configure-the-vm-and-mongodb"></a>Konfigurowanie maszyn wirtualnych i MongoDB

1. Zmienne ścieżek nie są aktualizowane przez Instalatora MongoDB. Bez MongoDB `bin` lokalizacji w sieci zmiennej path, musisz określić jego pełną ścieżkę każdym użyciu wykonywalny MongoDB. Aby dodać lokalizację usługi zmiennej ścieżki:

    - Kliknij prawym przyciskiem myszy **Start** menu, a następnie wybierz pozycję **System**.
    - Kliknij pozycję **Zaawansowane ustawienia systemu**, a następnie kliknij przycisk **Zmienne środowiska**.
    - W obszarze **zmienne systemu**wybierz **ścieżkę**, a następnie kliknij przycisk **Edytuj**.

    ![Konfigurowanie zmienne ŚCIEŻEK](./media/virtual-machines-windows-install-mongodb/configure-path-variables.png)

    Dodaj ścieżkę do swojego MongoDB `bin` folder. MongoDB zazwyczaj jest zainstalowany w `C:\Program Files\MongoDB`. Sprawdź, czy ścieżka instalacji na swojej maszyn wirtualnych. W poniższym przykładzie dodawane domyślnej lokalizacji do instalacji MongoDB `PATH` zmiennej:

    ```
    ;C:\Program Files\MongoDB\Server\3.2\bin
    ```

    > [AZURE.NOTE] Pamiętaj dodać wiodącego średnika (`;`) oznaczającą, że dodajesz lokalizację usługi `PATH` zmiennej.

2. Tworzenie katalogów MongoDB danych i dziennika na dysku danych. W **Start** menu wybierz pozycję **Wiersz polecenia**. Poniższe przykłady Utwórz katalogi na dysku F:

    ```
    mkdir F:\MongoData
    mkdir F:\MongoLogs
    ```

3. Uruchom wystąpienie MongoDB przy użyciu następującego polecenia dopasowywania ścieżkę dostępu do danych i w związku z tym logowania katalogów:

    ```
    mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log
    ```

    Może potrwać kilka minut MongoDB przydzielania pliki dziennika i Zacznij słuchać połączeń. Wszystkie wiadomości dziennika są kierowane do pliku *F:\MongoLogs\mongolog.log* jako `mongod.exe` serwera zostanie uruchomiony i przydziela pliki dziennika.

    > [AZURE.NOTE] Wiersz polecenia pozostaje Praca nad tym zadaniem, jest uruchomiona wystąpienia MongoDB. Pozostaw okno wiersza polecenia Otwórz, aby kontynuować MongoDB. Lub instalowanie MongoDB jako usługa wyszczególnione w następnym kroku.

4. Bardziej rozbudowany możliwości MongoDB zainstalować `mongod.exe` jako usługa. Tworzenie usługi oznacza, że nie musisz pozostawić uruchamianiu za każdym razem, którego chcesz użyć MongoDB wiersz polecenia. Utworzenie usługi w następujący sposób, w związku z tym dopasowywania ścieżkę dostępu do usługi katalogów danych i dziennika:

    ```
    mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log `
        --logappend  --install
    ```

    Poprzednie polecenie tworzy usługę o nazwie MongoDB, z opisem "Mongo DB". Następujące parametry również są określane:

    - `--dbpath` Opcja określa lokalizację katalogu danych.
    - `--logpath` Opcja musi być używana do określenia pliku dziennika, ponieważ uruchomionej usługi nie ma okna polecenia w celu wyświetlenia danych wyjściowych.
    - `--logappend` Opcja określa, że ponowne uruchomienie usługi powoduje, że dane wyjściowe dołączyć do istniejącego pliku dziennika.

  Aby uruchomić usługę MongoDB, uruchom następujące polecenie:

    ```
    net start MongoDB
    ```

    Aby uzyskać więcej informacji o tworzeniu usługę MongoDB zobacz [Konfigurowanie usługi systemu Windows dla MongoDB](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/#mongodb-as-a-windows-service).

## <a name="test-the-mongodb-instance"></a>Testowanie wystąpienia MongoDB

MongoDB zainstalowany jako usługa lub działającego jako jedno wystąpienie można teraz rozpocząć tworzenie i używanie bazy danych. Aby rozpocząć powłoki administracyjne MongoDB, otworzyć inne okno wiersza polecenia w **Start** menu i wpisz następujące polecenie:

```
mongo  
```

Można wyświetlić listę baz danych z `db` polecenia. Wstawianie niektóre dane w następujący sposób:

```
db.foo.insert( { a : 1 } )
```

Wyszukiwanie danych w następujący sposób:

```
db.foo.find()
```

Wynik jest podobny do następującego przykładu:

```
{ "_id" : "ObjectId("57f6a86cee873a6232d74842"), "a" : 1 }
```

Zakończ `mongo` konsoli w następujący sposób:

```
exit
```

## <a name="configure-firewall-and-network-security-group-rules"></a>Konfigurowanie zapory i zasady grupy zabezpieczeń sieci
Teraz, gdy MongoDB jest zainstalowana i uruchomiona, otwórz port w Zapora systemu Windows, aby zdalnie łączyć się MongoDB. Aby utworzyć nową regułę ruchu przychodzącego umożliwia TCP port 27017, otwórz administracyjne wierszu polecenia programu PowerShell i wpisz następujące polecenie:

```powerShell
New-NetFirewallRule -DisplayName "Allow MongoDB" -Direction Inbound `
    -Protocol TCP -LocalPort 27017 -Action Allow
```

Można także tworzyć reguły przy użyciu narzędzia do zarządzania graficzne **Zapora systemu Windows z zabezpieczeniami zaawansowanymi** . Utwórz nową regułę ruchu przychodzącego umożliwia TCP port 27017.

W razie potrzeby utwórz regułę grupy zabezpieczeń sieci umożliwienie dostępu do MongoDB z poza istniejącą podsieć Azure wirtualnej sieci. Można utworzyć reguły grupy zabezpieczeń sieci przy użyciu [Azure portal](virtual-machines-windows-nsg-quickstart-portal.md) lub [Azure programu PowerShell](virtual-machines-windows-nsg-quickstart-powershell.md). Podobnie jak w przypadku reguły zapory systemu Windows jest możliwe port TCP 27017 do interfejsu wirtualną sieć maszyn wirtualnych usługi MongoDB.

> [AZURE.NOTE] TCP port 27017 jest domyślny port używany przez MongoDB. Ten port można zmienić za pomocą `--port` parametrów podczas uruchamiania `mongod.exe` ręcznie lub z usługi. Jeśli zmienisz portu, upewnij się zaktualizować reguły zapory systemu Windows i grupy zabezpieczeń sieci w poprzednich krokach.


## <a name="next-steps"></a>Następne kroki
W tym samouczku wiesz, jak zainstalować i skonfigurować MongoDB na swojej maszyn wirtualnych systemu Windows. Teraz masz dostęp MongoDB na swojej Głosowa systemu Windows, wykonując poniższe tematy Zaawansowane w [dokumentacji MongoDB](https://docs.mongodb.com/manual/).
