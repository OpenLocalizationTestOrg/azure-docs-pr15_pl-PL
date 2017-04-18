<properties
    pageTitle="Instalowanie Python i SDK - Azure"
    description="Dowiedz się, jak zainstalować Python i SDK Azure za pomocą."
    services=""
    documentationCenter="python"
    authors="lmazuel"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="lmazuel"/>

# <a name="installing-python-and-the-sdk"></a>Instalowanie Python i SDK

Python jest łatwa Konfiguracja w systemie Windows i jest preinstalowana na komputerze Mac, Linux i [Urodzinową dla systemu Windows](https://msdn.microsoft.com/commandline/wsl/about). Ten przewodnik przeprowadzi Cię przez instalacji i przygotowaniu komputera do użytku z Azure.

## <a name="whats-in-the-python-azure-sdk"></a>Co to jest w Python Azure SDK?

Zestaw SDK Azure dla Python obejmuje składniki, które umożliwiają opracowywania, wdrażania i zarządzania aplikacjami Python dla Azure. W szczególności SDK Azure dla Python jest następująca:

* **Zarządzanie bibliotekami**. Te biblioteki klasy zapewniają interfejs zarządzania zasobami Azure, takich jak konta miejsca do magazynowania, maszyn wirtualnych.

* **Środowisko uruchomieniowe bibliotek**. Te biblioteki klasy zapewniają interfejs do uzyskiwania dostępu do Azure funkcje, takie jak bus miejsca do magazynowania i usług.

## <a name="which-python-and-which-version-to-use"></a>Które Python i wersję, która umożliwia

Istnieje kilka podtypy tłumaczy Python: Przykłady:

* CPython - interpretera Python standardowych i najczęściej używane
* PyPy — szybkie, zgodny z alternatywnej implementacji CPython
* IronPython - interpretera Python, uruchamianej na .net/CLR
* Jython - interpretera Python, uruchamianej na środowiska Java

**CPython** v2.7 lub v3.3 + i PyPy 5.4.0 jest sprawdzany i obsługiwane dla zestawu SDK Azure Python.

## <a name="where-to-get-python"></a>Gdzie można uzyskać Python?

Istnieje kilka sposobów uzyskiwania CPython:

* Bezpośrednio z [www.python.org][]
* Z godny zaufania distro, takich jak [www.continuum.io][], [www.enthought.com][] lub [www.activestate.com][]
* Tworzenie źródła!

Jeśli nie masz określonych wymagań, zalecamy dwóch pierwszych opcji.

## <a name="sdk-installation-on-windows-linux-and-macos-client-libraries-only"></a>Instalacji zestawu SDK systemu Windows, Linux i MacOS (tylko w przypadku bibliotek klienta)

Jeśli masz już zainstalowany Python, należy zainstalować pakiet dla bibliotek klienta w istniejącej 2.7 Python lub Python 3.3 + środowiska przez osoby pip. Pobierze pakiety z [Indeksu pakiet Python][] (PyPI).

Może być konieczne uprawnienia administratora:

- Linux i MacOS, za pomocą `sudo` polecenia: `sudo pip install azure-mgmt-compute`.
- System Windows: Otwórz wiersz programu PowerShell i polecenia jako administrator

Możesz zainstalować pojedynczo każdej biblioteki dla każdej usługi Azure:

```console
   $ pip install azure-batch          # Install the latest Batch runtime library
   $ pip install azure-mgmt-scheduler # Install the latest Storage management library
```

Można zainstalować pakietów Podgląd za pomocą `--pre` flagi:

```console
   $ pip install --pre azure-mgmt-compute # will install only the latest Compute Management library
```

Można również zainstalować zestaw Azure bibliotek w jednym wierszu, w którym używana jest `azure` pakiet meta. Ponieważ nie wszystkie pakiety w tym pakiecie meta są publikowane jako stały jeszcze `azure` pakiet meta jest nadal w podglądzie. Jednak pakiety podstawowych, pod kątem jakości i kompletności kodu można uznać "stałe" w tej chwili
- go będzie urzędowo oznaczone jako takie synchronizacji z innych wersji językowych tak szybko, jak to możliwe. Firma Microsoft nie są planowania na głównych dalsze zmiany do tego czasu.

Ponieważ jest wersji preview, należy użyć `--pre` flagi:

```console
   $ pip install --pre azure
```
   
lub bezpośrednio

```console
   $ pip install azure==2.0.0rc6
```

## <a name="getting-more-packages"></a>Uzyskiwanie pakietów więcej

[Indeks pakiet Python][] (PyPI) ma sformatowanego zaznaczenia Python bibliotek.  Jeśli chcesz zainstalować Distro będzie już najbardziej interesujące bitów dla różnych scenariuszy rozwoju sieci web do obliczeń technicznych.


## <a name="python-tools-for-visual-studio"></a>Narzędzia Python programu Visual Studio

[Narzędzia Python programu Visual Studio][] (PTVS) to bezpłatne OSS dodatek Microsoft zmieni się w PORÓWNANIU z w pełnoprawny IDE Python:

![How-do-Zainstaluj python-ptvs](./media/python-how-to-install/how-to-install-python-ptvs.png)

Użycie PTVS jest opcjonalne, ale jest zalecane, ponieważ daje Python i rozwiązania Project Web pomocy technicznej, debugowanie profilowania, interakcyjna, edytowanie szablonu i technologii Intellisense.

PTVS ułatwia także wdrożenia Microsoft Azure z obsługą wdrożenia [Usług w chmurze][] i [witrynami sieci Web][].

PTVS współpracuje z programu istniejących 2013 Visual Studio lub instalacji 2015 r.  Dokumentacja, pobierania i dyskusje zobacz [Python Tools for Visual Studio].  

## <a name="python-azure-scenarios-for-linux-and-macos"></a>Python Azure scenariusze Linux i MacOS

W przypadku Linux lub MacOS, głównym Azure scenariuszy, które są obsługiwane:

1. Przez inne usługi Azure za pomocą bibliotek klienta Python

2. Uruchamianie aplikacji w maszyny Linux

3. Rozwijanie i publikowanie do witryn sieci Web Azure za pomocą cyfra

Pierwszy scenariusz umożliwia tworzenie aplikacji sieci web sformatowanym, które wykorzystać możliwości Azure PaaS takich jak [Magazyn obiektów blob][], [magazyn kolejek][], [Magazyn tabel][] itd., za pośrednictwem Pythonic otoki dla interfejsów API pozostałych Azure. Te działa tak samo na systemie Windows, Mac i Linux.  Za pomocą tych bibliotek klienta z komputera lokalnego rozwoju lub maszyny Linux uruchomionych Azure.

Dla tego scenariusza maszyn wirtualnych wystarczy uruchomić maszyny Linux wybranych przez użytkownika (Ubuntu, CentOS, Suse) i uruchom Zarządzanie Cię zainteresować.  Na przykład można uruchomić [IPython][] REPL-Notes na komputerze z systemem Windows i komputerów Mac i Linux i wskaż przeglądarki Linux lub maszyn wirtualnych obsługi wielu procesorów Windows uruchomionych aparat IPython Azure. Zobacz samouczek [Notesu IPython Azure][] , aby uzyskać więcej informacji.

Aby uzyskać informacje na temat konfigurowania maszyn wirtualnych Linux zobacz samouczek [Tworzenie maszyn wirtualnych systemem Linux][] .

Za pomocą cyfra, mogą tworzyć aplikacji sieci web Python i opublikować go Azure witryny sieci Web z żadnego systemu operacyjnego.  Po naciśnięciu repozytorium Azure automatycznie utworzy wirtualnego środowiska i pip zainstalować pakiety wymagane.

Aby uzyskać więcej informacji o tworzeniu i publikowania Azure witryn sieci Web Zobacz samouczki dla [Tworzenia witryn sieci Web z Django][], [Tworzenia witryn sieci Web z butelki][]i [Tworzenia witryn sieci Web z kolbą][]. Aby uzyskać więcej informacji na temat korzystania z dowolnego notacją WSGI framework zobacz [Konfigurowanie Python z Azure witrynami sieci Web][].


## <a name="additional-software-and-resources"></a>Dodatkowego oprogramowania i zasoby:

* [Azure SDK dla Python ReadTheDocs](http://azure-sdk-for-python.readthedocs.io/en/latest/)
* [Azure SDK dla Python Github](https://github.com/Azure/azure-sdk-for-python)
* [Oficjalne Azure próbki do Python](https://azure.microsoft.com/documentation/samples/?platform=python)
* [Rozkład Python analizy począwszy][]
* [Rozkład Enthought Python][]
* [Rozkład ActiveState Python][]
* [SciPy - pakietu naukowych Python bibliotek][]
* [NumPy - biblioteki numeryczne Python][]
* [Projekt Django — framework/CMS dojrzałych sieci web][]
* [IPython — zaawansowane REPL-Notes, Python][]
* [Notes IPython Azure][]
* [Python Tools for Visual Studio GitHub][]
* [Centrum deweloperów Python](/develop/python/)

[Rozkład Python analizy począwszy]: http://continuum.io
[Rozkład Enthought Python]: http://www.enthought.com
[Rozkład ActiveState Python]: http://www.activestate.com
[www.Python.org]: http://www.python.org
[www.continuum.IO]: http://continuum.io
[www.enthought.com]: http://www.enthought.com
[www.ActiveState.com]: http://www.activestate.com
[SciPy - pakietu naukowych Python bibliotek]: http://www.scipy.org
[NumPy - biblioteki numeryczne Python]: http://www.numpy.org
[Projekt Django — framework/CMS dojrzałych sieci web]: http://www.djangoproject.com
[IPython — zaawansowane REPL-Notes, Python]: http://ipython.org
[IPython]: http://ipython.org
[Notes IPython Azure]: virtual-machines-linux-jupyter-notebook.md
[Usług w chmurze]: cloud-services-python-ptvs.md
[Witryny sieci Web]: web-sites-python-ptvs-django-mysql.md
[Narzędzia Python programu Visual Studio]: http://aka.ms/ptvs
[Python Tools for Visual Studio GitHub]: https://github.com/microsoft/ptvs
[Indeks pakiet Python]: http://pypi.python.org/pypi
[Microsoft Azure SDK for Python 2.7]: http://go.microsoft.com/fwlink/?LinkId=254281
[Microsoft Azure SDK for Python 3.4]: http://go.microsoft.com/fwlink/?LinkID=516990
[Setting up a Linux VM via the Azure portal]: create-and-configure-opensuse-vm-in-portal.md
[How to use the Azure Command-Line Interface]: crossplat-cmd-tools.md
[Tworzenie maszyny wirtualnej systemem Linux]: virtual-machines-linux-quick-create-cli.md
[Tworzenie witryny sieci Web za pomocą Django]: web-sites-python-create-deploy-django-app.md
[Tworzenie witryny sieci Web za pomocą butelki]: web-sites-python-create-deploy-bottle-app.md
[Tworzenie witryny sieci Web z kolbą]: web-sites-python-create-deploy-flask-app.md
[Konfigurowanie Python z Azure witrynami sieci Web]: web-sites-python-configure.md
[Magazyn tabel]: storage-python-how-to-use-table-storage.md
[magazyn kolejek]: storage-python-how-to-use-queue-storage.md
[Magazyn obiektów blob]: storage-python-how-to-use-blob-storage.md
