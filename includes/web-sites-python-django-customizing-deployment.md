Azure określi, że aplikacja używa Python, **Jeśli są spełnione oba poniższe warunki**:

- Plik Requirements.txt w folderze głównym
- dowolny plik .py w folderze głównym lub runtime.txt, która określa python

W przypadku, które będą używane skryptu wdrażania Python, który wykonuje standardowy synchronizację plików, a także dodatkowych operacji Python, takich jak:

- Automatyczne zarządzanie wirtualnego środowiska
- Instalacja pakietów wymienionych w requirements.txt przy użyciu pip
- Tworzenie odpowiedniego pliku Web.config na wybranej wersji Python podstawie.
- Zbieranie statyczne pliki aplikacji Django

Możesz sterować niektórych aspektów kroków wdrażania domyślne bez konieczności dostosować skrypt.

Jeśli chcesz pominąć wszystkie kroki wdrażania Python, można utworzyć ten pusty plik:

    \.skipPythonDeployment

Jeśli chcesz pominąć kolekcji plików statycznych aplikacji Django:

    \.skipDjango 

Mieć większą kontrolę nad wdrożenia można zmienić domyślny skrypt wdrożenia, tworząc następujące pliki:

    \.deployment
    \deploy.cmd

[Azure interfejs wiersza polecenia][] służy do tworzenia plików.  Użyj tego polecenia z folderu projektu:

    azure site deploymentscript --python

Tych plików nie istnieje, Azure tworzy skrypt wdrożenia tymczasowe i uruchamia je.  Jest identyczne z utworzonego przy użyciu polecenia powyżej.

[Azure interfejsu wiersza polecenia]: http://azure.microsoft.com/downloads/
