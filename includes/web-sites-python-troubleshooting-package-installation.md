Niektóre pakiety mogą nie można zainstalować przy użyciu pip uruchomienia Azure.  Po prostu możliwe, że pakietu nie jest dostępna w indeksie pakiet Python.  Jest to możliwe, że nie jest wymagane kompilatora (kompilatora nie jest dostępny na komputerze, na którym działa aplikacji sieci web w usłudze Azure aplikacji).

W tej sekcji opisano sposobach radzenia sobie z tego problemu.

### <a name="request-wheels"></a>Żądanie koła

Jeśli instalacja pakietu wymaga kompilatora, możesz spróbować skontaktować się z właścicielem pakiet żądania koła być udostępniane dla pakietu.

Z ostatnich dostępność [Microsoft Visual C++ kompilatora Python 2.7][]jest teraz łatwiejsze do utworzenia pakietów, które masz kodu macierzystego Python 2.7.

### <a name="build-wheels-requires-windows"></a>Tworzenie koła (wymaga systemu Windows)

Uwaga: Podczas korzystania z tej opcji, upewnij się, że skompilować pakiet przy użyciu zgodnej z platformy architektura i wersją używaną w aplikacji sieci web w usłudze Azure aplikacji w środowisku Python (Windows/32-bit/2.7 lub 3.4).

Jeśli nie możesz zainstalować pakiet ponieważ wymaga kompilatora, można zainstalować kompilatora na komputerze lokalnym i tworzenie kołem pakietu, który można następnie włączy repozytorium.

Użytkownicy komputerów Mac i Linux: Jeśli nie masz dostęp do komputera systemu Windows, zobacz [Tworzenie maszyn wirtualnych systemie Windows][] na temat tworzenia maszyn wirtualnych Azure.  Można użyć go do tworzenia koła, dodaj je do repozytorium i odrzucanie maszyn wirtualnych w razie potrzeby. 

W przypadku Python 2.7 można zainstalować [Programu Microsoft Visual C++ kompilatora Python 2.7][].

W przypadku Python 3.4 można zainstalować [Programu Microsoft Visual C++ 2010 Express][].

Aby utworzyć koła, musisz pakietu koło:

    env\scripts\pip install wheel

Za pomocą `pip wheel` skompilować zależność:

    env\scripts\pip wheel azure==0.8.4

Spowoduje to utworzenie pliku .whl w folderze \wheelhouse.  Dodawanie plików folder i koła \wheelhouse do repozytorium.

Edytowanie swojego requirements.txt, aby dodać `--find-links` opcję u góry. To informuje pip, aby wyszukać dokładne dopasowanie w lokalnym folderze przed przejściem do indeksu pakiet python.

    --find-links wheelhouse
    azure==0.8.4

Jeśli chcesz uwzględnić wszystkie zależności między w folderze \wheelhouse nie indeksu i stosowanie python pakiet ogóle można wymusić pip, aby zignorować indeksu pakietu, dodając `--no-index` na początek usługi requirements.txt.

    --no-index

### <a name="customize-installation"></a>Dostosowywanie instalacji

Możesz dostosować skrypt wdrożenia, aby zainstalować pakiet w środowisku wirtualnych przy użyciu alternatywnych Instalator, takich jak łatwe\_instalacji.  Zobacz deploy.cmd, na przykład, w którym jest ujęta w komentarz.  Upewnij się, że przesyłek nie są wymienione w requirements.txt, aby uniemożliwić pip instalowanie ich.

Dodać to skrypt wdrożenia:

    env\scripts\easy_install somepackage

Może być również mogli używać łatwe\_Zainstaluj do instalacji Instalatora exe (niektóre są zip zgodnych, tak proste\_zainstaluj je obsługuje).  Dodaj Instalator repozytorium, a wywołania łatwe\_zainstalować przekazując ścieżkę do plik wykonywalny.

Dodać to skrypt wdrożenia:

    env\scripts\easy_install "%DEPLOYMENT_SOURCE%\installers\somepackage.exe"

### <a name="include-the-virtual-environment-in-the-repository-requires-windows"></a>Dołączanie wirtualnego środowiska w repozytorium (wymaga systemu Windows)

Uwaga: Podczas korzystania z tej opcji, upewnij się, że za pomocą wirtualnego środowiska, odpowiadający platformy i architektury wersji używanego w aplikacji sieci web w usłudze Azure aplikacji (Windows/32-bit/2.7 lub 3.4).

Jeśli dołączysz wirtualnego środowiska w repozytorium, skrypt wdrożenia można zapobiec zmienianiu wirtualnego środowiska zarządzania Azure, tworząc pusty plik:

    .skipPythonDeployment

Zaleca się, że możesz usunąć istniejące środowisko wirtualnych w aplikacji, aby pozostała pliki z po wirtualnego środowiska został automatycznie zarządzać.


[Tworzenie wirtualnych komputera z systemem Windows]: http://azure.microsoft.com/documentation/articles/virtual-machines-windows-hero-tutorial/
[Microsoft Visual C++ kompilatora dla Python 2.7]: http://aka.ms/vcpython27
[Microsoft Visual C++ 2010 Express]: http://go.microsoft.com/?linkid=9709949
