<properties
    pageTitle="Tworzenie aplikacji sieci web za pomocą Django platformy Azure"
    description="Samouczek, w której przedstawiono uruchamiania aplikacji sieci web Python w aplikacjach sieci Web usługi aplikacji Azure."
    services="app-service\web"
    documentationCenter="python"
    tags="python"
    authors="huguesv" 
    manager="wpickett" 
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="hero-article" 
    ms.date="02/19/2016"
    ms.author="huvalo"/>


# <a name="creating-web-apps-with-django-in-azure"></a>Tworzenie aplikacji sieci web za pomocą Django platformy Azure

Ten samouczek opisano, jak rozpocząć uruchomionych Python [Azure aplikacji usługi sieci Web](http://go.microsoft.com/fwlink/?LinkId=529714). Aplikacje sieci Web udostępnia ograniczoną bezpłatne hosting i szybkiego rozmieszczania i Python! W miarę aplikacji można przełączyć się hostingu płatnej, a także można zintegrować z wszystkich innych usług Azure.

Utworzysz aplikacji przy użyciu struktury sieci web Django (patrz alternatywnych wersji tego samouczka [kolbę](web-sites-python-create-deploy-flask-app.md) i [butelki](web-sites-python-create-deploy-bottle-app.md)). Będzie tworzenie aplikacji sieci web z usługi Azure Marketplace, skonfigurować wdrożenie cyfra i klonowanie repozytorium lokalnie. Następnie uruchomisz aplikacji lokalnie, wprowadź zmiany, zatwierdzanie i przekazać je do Azure. Samouczek pokazano, jak to zrobić z systemem Windows lub komputerów Mac i Linux.

[AZURE.INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

>[AZURE.NOTE] Jeśli chcesz rozpocząć pracę z Azure aplikacji usługi przed utworzeniem konta dla konta Azure, przejdź do [Spróbuj aplikacji usługi](http://go.microsoft.com/fwlink/?LinkId=523751), którym natychmiast można utworzyć aplikację sieci web krótkotrwałe starter w aplikacji usługi. Nie kart kredytowych wymagane; nie zobowiązania.


## <a name="prerequisites"></a>Wymagania wstępne

- System Windows, Mac lub Linux
- Python 2.7 lub 3.4
- setuptools pip, virtualenv (tylko 2.7 Python)
- Cyfra
- [Narzędzia Python programu Visual Studio][] Uwaga (PTVS) -: jest to opcjonalne

**Uwaga**: publikowanie TFS nie jest obecnie obsługiwane w przypadku projektów Python.

### <a name="windows"></a>Systemu Windows

Jeśli nie masz jeszcze Python 2.7 lub 3.4 zainstalowanych (32-bitowa), zaleca się zainstalowanie [SDK Azure dla Python 2.7] lub [SDK Azure dla Python 3.4] za pomocą Instalatora platformy sieci Web. To jest instalowana 32-bitową wersję Python setuptools, pip, virtualenv, itp (Python 32-bitowa jest zainstalowanych na komputerach Azure hosta). Możesz również przejść Python z [python.org].

Cyfra zalecamy [Cyfra dla systemu Windows] lub [GitHub dla systemu Windows]. Jeśli używasz programu Visual Studio umożliwia zintegrowaną obsługę cyfra.

Ponadto zaleca się zainstalowanie [Python 2.2 narzędzia programu Visual Studio]. To jest opcjonalne, ale jeśli masz [Visual Studio], łącznie z bezpłatnego 2013 społeczności programu Visual Studio lub Visual Studio Express 2013 dla sieci Web, następnie zapewni to doskonałe IDE Python.

### <a name="maclinux"></a>Mac i Linux

Powinny mieć Python i cyfra już zainstalowany, ale upewnij się, że masz Python 2.7 lub 3.4.


## <a name="web-app-creation-on-portal"></a>Tworzenie aplikacji sieci Web w portalu

Pierwszym krokiem podczas tworzenia aplikacji jest utworzenie aplikacji sieci web przez [Azure Portal](https://portal.azure.com).

1. Zaloguj się do portalu Azure i kliknij przycisk **Nowy** w lewym dolnym rogu.
3. W polu wyszukiwania wpisz "python".
4. W wynikach wyszukiwania zaznacz **Django** (opublikowane przez PTVS), a następnie kliknij przycisk **Utwórz**.
5. Konfigurowanie nowej aplikacji Django, takich jak tworzenie nowego planu aplikacji usługi i nowej grupy zasobów dla niego. Następnie kliknij przycisk **Utwórz**.
6. Konfigurowanie publikowania cyfra dla aplikacji sieci web nowo utworzonego zgodnie z instrukcjami w [Lokalnym rozmieszczania cyfra Azure aplikacji usługi](app-service-deploy-local-git.md).

## <a name="application-overview"></a>Omówienie aplikacji

### <a name="git-repository-contents"></a>Cyfra repozytorium zawartości

Poniżej przedstawiono plików, które można znaleźć w początkowej repozytorium cyfra, które będą możemy klonowanie w następnej sekcji.

    \app\__init__.py
    \app\forms.py
    \app\models.py
    \app\tests.py
    \app\views.py
    \app\static\content\
    \app\static\fonts\
    \app\static\scripts\
    \app\templates\about.html
    \app\templates\contact.html
    \app\templates\index.html
    \app\templates\layout.html
    \app\templates\login.html
    \app\templates\loginpartial.html
    \DjangoWebProject\__init__.py
    \DjangoWebProject\settings.py
    \DjangoWebProject\urls.py
    \DjangoWebProject\wsgi.py

Główne źródła dla aplikacji. Składa się z 3 stron (o kontakcie indeks) z Układ wzorca. Zawartość statyczną i skryptów zawierają początkowego, jquery, modernizr i odpowiedź.

    \manage.py

Zarządzanie lokalne i obsługi serwera rozwoju. Umożliwia uruchamianie aplikacji lokalnie, zsynchronizuj bazy danych itp.

    \db.sqlite3

Domyślna baza danych. Zawiera wymagane tabele dla aplikacji, aby działała, ale nie zawiera żadnych użytkowników (synchronizacja ma miejsce bazy danych, aby utworzyć użytkownika).

    \DjangoWebProject.pyproj
    \DjangoWebProject.sln

Pliki programu Project do użytku z [Python Tools for Visual Studio].

    \ptvs_virtualenv_proxy.py

Serwer proxy usług IIS dla środowiska wirtualne i PTVS zdalnego, obsługa debugowania.

    \requirements.txt

Zewnętrzne pakiety wymagane przez tę aplikację. Skrypt wdrożenia będzie pip zainstalować pakiety wymienione w tym pliku.

    \web.2.7.config
    \web.3.4.config

Pliki konfiguracji usług IIS. Skrypt wdrożenia będzie używać odpowiednich web.x.y.config i skopiuj go jako web.config.

### <a name="optional-files---customizing-deployment"></a>Pliki opcjonalne - Dostosowywanie wdrażania

[AZURE.INCLUDE [web-sites-python-django-customizing-deployment](../../includes/web-sites-python-django-customizing-deployment.md)]

### <a name="optional-files---python-runtime"></a>Pliki opcjonalne - środowisko uruchomieniowe Python

[AZURE.INCLUDE [web-sites-python-customizing-runtime](../../includes/web-sites-python-customizing-runtime.md)]

### <a name="additional-files-on-server"></a>Dodatkowe pliki na serwerze

Niektóre pliki istnieje na serwerze, ale nie są dodawane do repozytorium cyfra. Te są tworzone przez skrypt wdrożenia.

    \web.config

Plik konfiguracyjny usług IIS. Utworzone na podstawie web.x.y.config każdej wdrożenia.

    \env\

Środowisko wirtualne Python. Utworzone podczas wdrażania, jeżeli zgodne środowisko wirtualne nie ma jeszcze w aplikacji sieci web. Pakiety wymienione w requirements.txt są pip zainstalowany, ale pip pominie instalacji, jeśli pakiety są już zainstalowane.

Następne 3 sekcjach opisano sposoby przystąpić do tworzenia aplikacji sieci web w różnych środowiskach 3:

- Systemu Windows, korzystając z narzędzi Python programu Visual Studio
- Systemu Windows, z wiersza polecenia
- Mac i Linux z wiersza polecenia


## <a name="web-app-development---windows---python-tools-for-visual-studio"></a>Programowania aplikacji sieci Web Python — Windows — Tools for Visual Studio

### <a name="clone-the-repository"></a>Klonowanie repozytorium

Najpierw klonowanie repozytorium przy użyciu adresu URL, na Azure Portal. Aby uzyskać więcej informacji zobacz [Lokalnego wdrożenia cyfra usłudze Azure w aplikacji](app-service-deploy-local-git.md).

Otwórz plik rozwiązania (.sln), który znajduje się w katalogu głównym repozytorium.

![](./media/web-sites-python-create-deploy-django-app/ptvs-solution-django.png)

### <a name="create-virtual-environment"></a>Tworzenie wirtualnych środowiska

Teraz utworzymy wirtualnego środowiska w celu rozwoju lokalnego. Kliknij prawym przyciskiem myszy, wybierz pozycję **Środowiskach Python** **Dodawanie wirtualnego środowiska...**.

- Upewnij się, nazwę środowiska jest `env`.

- Wybierz pozycję interpretera podstawowej. Upewnij się użyć tej samej wersji Python, który jest zaznaczone dla aplikacji sieci web (w runtime.txt lub karta **Ustawienia aplikacji** aplikacji sieci web w Azure Portal).

- Upewnij się, że zaznaczono opcję, aby pobrać i zainstalować pakiety.

![](./media/web-sites-python-create-deploy-django-app/ptvs-add-virtual-env-27.png)

Kliknij przycisk **Utwórz**. To będzie utworzyć wirtualne środowisko, a następnie zainstaluj zależności wymienionych w requirements.txt.

### <a name="create-a-superuser"></a>Tworzenie administratora

Bazy danych dołączonej do aplikacji nie ma wszelkie zdefiniowane przez administratora. Aby można było korzystać z funkcji rejestrowania w aplikacji lub interfejsu administracyjnego Django (Jeśli zdecydujesz się ją włączyć), musisz utworzyć administratora.

Uruchom to z wiersza polecenia z folderu projektu:

    env\scripts\python manage.py createsuperuser

Postępuj zgodnie z instrukcjami, aby ustawić nazwę użytkownika, hasło itp.

### <a name="run-using-development-server"></a>Uruchamianie przy użyciu serwera opracowywania

Naciśnij klawisz F5, aby rozpocząć debugowanie i przeglądarki sieci web zostanie otwarty na stronę uruchomionej na komputerze lokalnym.

![](./media/web-sites-python-create-deploy-django-app/windows-browser-django.png)

Punkty kontrolne można ustawić w źródeł, za pomocą systemu windows czujki itp. Zobacz [Narzędzia Python dokumentacji programu Visual Studio] , aby uzyskać więcej informacji na temat różnych funkcji.

### <a name="make-changes"></a>Wprowadzanie zmian

Teraz możesz poeksperymentować wprowadzenie zmian w aplikacji źródeł i/lub szablony.

Po przetestowaniu zmiany, należy je zatwierdzić do repozytorium cyfra:

![](./media/web-sites-python-create-deploy-django-app/ptvs-commit-django.png)

### <a name="install-more-packages"></a>Zainstalować pakiety więcej

Aplikacja może być zależności poza Python i Django.

Możesz zainstalować dodatkowe pakiety przy użyciu pip. Aby zainstalować pakiet, kliknij prawym przyciskiem myszy wirtualnego środowiska i wybierz pozycję **Zainstaluj pakiet Python**.

Na przykład, aby zainstalować Azure SDK dla Python, które umożliwia dostęp do przechowywania Azure, bus usługi i innych usług Azure, wprowadź `azure`:

![](./media/web-sites-python-create-deploy-django-app/ptvs-install-package-dialog.png)

Kliknij prawym przyciskiem myszy wirtualnego środowiska i wybierz pozycję **Generuj requirements.txt** aktualizacji requirements.txt.

Następnie Zatwierdź zmiany w requirements.txt do repozytorium cyfra.

### <a name="deploy-to-azure"></a>Wdrażanie Azure

Aby wyzwolić wdrożeniu, kliknij przycisk **Synchronizuj** lub **Push**. Synchronizacja działa zarówno i wypychania.

![](./media/web-sites-python-create-deploy-django-app/ptvs-git-push.png)

Pierwszy wdrożenia zajmie trochę czasu, jak utworzy wirtualnego środowiska, zainstaluj pakiety itd.

Program Visual Studio nie pokazuje postęp wdrożenia. Jeśli chcesz Przejrzyj wyniki, zobacz sekcję [Rozwiązywanie problemów — wdrożenia](#troubleshooting-deployment).

Przejdź do adresu URL Azure, aby wyświetlić swoje zmiany.


## <a name="web-app-development---windows---command-line"></a>Wiersz polecenia rozwoju — Windows — aplikacji sieci Web

### <a name="clone-the-repository"></a>Klonowanie repozytorium

Najpierw klonowanie repozytorium przy użyciu adresu URL, na Azure Portal i dodawanie repozytorium Azure jako zdalny. Aby uzyskać więcej informacji zobacz [Lokalnego wdrożenia cyfra usłudze Azure w aplikacji](app-service-deploy-local-git.md).

    git clone <repo-url>
    cd <repo-folder>
    git remote add azure <repo-url>

### <a name="create-virtual-environment"></a>Tworzenie wirtualnych środowiska

Utworzymy nowego wirtualnego środowiska w celu programistycznych (nie dodawać je do repozytorium). Środowiska wirtualne w Python nie są relocatable, aby każdej używająca na pasku aplikacji będą tworzyć własne lokalnie.

Upewnij się użyć tej samej wersji Python, który jest zaznaczone dla aplikacji sieci web (w runtime.txt lub karta Ustawienia aplikacji aplikacji sieci web w Azure Portal).

Aby uzyskać Python 2.7:

    c:\python27\python.exe -m virtualenv env

Aby uzyskać Python 3.4:

    c:\python34\python.exe -m venv env

Zainstaluj wszystkie zewnętrzne pakiety wymagane przez aplikację. Plik requirements.txt w katalogu głównym repozytorium służy do instalowania pakietów w środowisku wirtualnej:

    env\scripts\pip install -r requirements.txt

### <a name="create-a-superuser"></a>Tworzenie administratora

Bazy danych dołączonej do aplikacji nie ma wszelkie zdefiniowane przez administratora. Aby można było korzystać z funkcji rejestrowania w aplikacji lub interfejsu administracyjnego Django (Jeśli zdecydujesz się ją włączyć), musisz utworzyć administratora.

Uruchom to z wiersza polecenia z folderu projektu:

    env\scripts\python manage.py createsuperuser

Postępuj zgodnie z instrukcjami, aby ustawić nazwę użytkownika, hasło itp.

### <a name="run-using-development-server"></a>Uruchamianie przy użyciu serwera opracowywania

Można uruchomić aplikację, w obszarze serwer dla programistów przy użyciu następującego polecenia:

    env\scripts\python manage.py runserver

Na konsoli zostanie wyświetlony adres URL i wykrywa port serwera:

![](./media/web-sites-python-create-deploy-django-app/windows-run-local-django.png)

Następnie otwórz przeglądarkę sieci web do tego adresu URL.

![](./media/web-sites-python-create-deploy-django-app/windows-browser-django.png)

### <a name="make-changes"></a>Wprowadzanie zmian

Teraz możesz poeksperymentować wprowadzenie zmian w aplikacji źródeł i/lub szablony.

Po przetestowaniu zmiany, należy je zatwierdzić do repozytorium cyfra:

    git add <modified-file>
    git commit -m "<commit-comment>"

### <a name="install-more-packages"></a>Zainstalować pakiety więcej

Aplikacja może być zależności poza Python i Django.

Możesz zainstalować dodatkowe pakiety przy użyciu pip. Na przykład aby zainstalować Azure SDK dla Python, które umożliwia dostęp do przechowywania Azure, bus usługi i innych usług Azure, wpisz:

    env\scripts\pip install azure

Upewnij się zaktualizować requirements.txt:

    env\scripts\pip freeze > requirements.txt

Zatwierdź zmiany:

    git add requirements.txt
    git commit -m "Added azure package"

### <a name="deploy-to-azure"></a>Wdrażanie Azure

Aby wyzwolić wdrożeniu, należy przekazać zmiany do Azure:

    git push azure master

Zostaną wyświetlone dane wyjściowe skrypt wdrożenia, łącznie z wirtualnego środowiska tworzenia instalacji opakowań, tworzenie web.config.

Przejdź do adresu URL Azure, aby wyświetlić swoje zmiany.


## <a name="web-app-development---maclinux---command-line"></a>Wiersz polecenia rozwoju Mac i Linux — aplikacji sieci Web

### <a name="clone-the-repository"></a>Klonowanie repozytorium

Najpierw klonowanie repozytorium przy użyciu adresu URL, na Azure Portal i dodawanie repozytorium Azure jako zdalny. Aby uzyskać więcej informacji zobacz [Lokalnego wdrożenia cyfra usłudze Azure w aplikacji](app-service-deploy-local-git.md).

    git clone <repo-url>
    cd <repo-folder>
    git remote add azure <repo-url>

### <a name="create-virtual-environment"></a>Tworzenie wirtualnych środowiska

Utworzymy nowego wirtualnego środowiska w celu programistycznych (nie dodawać je do repozytorium). Środowiska wirtualne w Python nie są relocatable, aby każdej używająca na pasku aplikacji będą tworzyć własne lokalnie.

Upewnij się użyć tej samej wersji Python, który jest zaznaczone dla aplikacji sieci web (w runtime.txt lub karta Ustawienia aplikacji aplikacji sieci web w Azure Portal).

Aby uzyskać Python 2.7:

    python -m virtualenv env

Aby uzyskać Python 3.4:

    python -m venv env

lub

    pyvenv env

Zainstaluj wszystkie zewnętrzne pakiety wymagane przez aplikację. Plik requirements.txt w katalogu głównym repozytorium służy do instalowania pakietów w środowisku wirtualnej:

    env/bin/pip install -r requirements.txt

### <a name="create-a-superuser"></a>Tworzenie administratora

Bazy danych dołączonej do aplikacji nie ma wszelkie zdefiniowane przez administratora. Aby można było korzystać z funkcji rejestrowania w aplikacji lub interfejsu administracyjnego Django (Jeśli zdecydujesz się ją włączyć), musisz utworzyć administratora.

Uruchom to z wiersza polecenia z folderu projektu:

    env/bin/python manage.py createsuperuser

Postępuj zgodnie z instrukcjami, aby ustawić nazwę użytkownika, hasło itp.

### <a name="run-using-development-server"></a>Uruchamianie przy użyciu serwera opracowywania

Można uruchomić aplikację, w obszarze serwer dla programistów przy użyciu następującego polecenia:

    env/bin/python manage.py runserver

Na konsoli zostanie wyświetlony adres URL i wykrywa port serwera:

![](./media/web-sites-python-create-deploy-django-app/mac-run-local-django.png)

Następnie otwórz przeglądarkę sieci web do tego adresu URL.

![](./media/web-sites-python-create-deploy-django-app/mac-browser-django.png)

### <a name="make-changes"></a>Wprowadzanie zmian

Teraz możesz poeksperymentować wprowadzenie zmian w aplikacji źródeł i/lub szablony.

Po przetestowaniu zmiany, należy je zatwierdzić do repozytorium cyfra:

    git add <modified-file>
    git commit -m "<commit-comment>"

### <a name="install-more-packages"></a>Zainstalować pakiety więcej

Aplikacja może być zależności poza Python i Django.

Możesz zainstalować dodatkowe pakiety przy użyciu pip. Na przykład aby zainstalować Azure SDK dla Python, które umożliwia dostęp do przechowywania Azure, bus usługi i innych usług Azure, wpisz:

    env/bin/pip install azure

Upewnij się zaktualizować requirements.txt:

    env/bin/pip freeze > requirements.txt

Zatwierdź zmiany:

    git add requirements.txt
    git commit -m "Added azure package"

### <a name="deploy-to-azure"></a>Wdrażanie Azure

Aby wyzwolić wdrożeniu, należy przekazać zmiany do Azure:

    git push azure master

Zostaną wyświetlone dane wyjściowe skrypt wdrożenia, łącznie z wirtualnego środowiska tworzenia instalacji opakowań, tworzenie web.config.

Przejdź do adresu URL Azure, aby wyświetlić swoje zmiany.


## <a name="troubleshooting---package-installation"></a>Rozwiązywanie problemów — instalacji pakietu

[AZURE.INCLUDE [web-sites-python-troubleshooting-package-installation](../../includes/web-sites-python-troubleshooting-package-installation.md)]


## <a name="troubleshooting---virtual-environment"></a>Rozwiązywanie problemów — środowisko wirtualne

[AZURE.INCLUDE [web-sites-python-troubleshooting-virtual-environment](../../includes/web-sites-python-troubleshooting-virtual-environment.md)]


## <a name="troubleshooting---static-files"></a>Rozwiązywanie problemów — pliki statyczne

Django ma pojęcia pobierania plików statycznych. Zostanie wyświetlone wszystkie statycznego pliki w ich pierwotnej lokalizacji i skopiowanie ich do jednego folderu. Dla tej aplikacji, zostaną skopiowane do `/static`.

Można to zrobić, ponieważ pliki statyczne mogą pochodzić z różnych Django "aplikacje". Na przykład pliki statyczne z interfejsów administrator Django znajdują się w podfolderze biblioteki Django środowisko wirtualne. Pliki statyczne zdefiniowanych przez tę aplikację znajdują się w `/app/static`. Gdy używasz więcej Django "aplikacji" masz statyczne plików znajdujących się w wielu miejscach.

Podczas uruchamiania aplikacji w trybie debugowania, aplikacja służy pliki statyczne w ich pierwotnej lokalizacji.

Podczas uruchamiania aplikacji w trybie wersji, czy aplikacja **nie** udostępniać pliki statyczne. Jest odpowiedzialny za serwer sieci web do obsługi plików. Dla tej aplikacji usług IIS będzie służyć pliki statyczne z `/static`.

Kolekcja plików statycznych odbywa się automatycznie jako część skrypt wdrożenia, wyczyszczenie wcześniej pobrane pliki. Oznacza to, kolekcji występuje co wdrożenia zmniejszają wdrożenia nieco, ale gwarantuje, że przestarzałych plików nie będą dostępne, można uniknąć potencjalnych problemów zabezpieczeń.

Jeśli chcesz pominąć zbiór statyczne plików aplikacji Django:

    \.skipDjango

Następnie należy wykonać zbioru ręcznie na komputerze lokalnym:

    env\scripts\python manage.py collectstatic

Następnie usuń `\static` folderu z `.gitignore` i dodaj go do repozytorium cyfra.


## <a name="troubleshooting---settings"></a>Rozwiązywanie problemów — ustawienia

Różne ustawienia aplikacji można zmienić w `DjangoWebProject/settings.py`.

Dla wygody deweloper jest włączony tryb debugowania. Jeden i strony tego powoduje, że będziesz mieć możliwość wyświetlane obrazy i inną zawartość statyczną, podczas wykonywania lokalnie, bez konieczności zbieranie pliki statyczne.

Aby wyłączyć tryb debugowania:

    DEBUG = False

Po wyłączeniu debugowania, wartość dla `ALLOWED_HOSTS` należy zaktualizować zawiera nazwę hosta Azure. Na przykład:

    ALLOWED_HOSTS = (
        'pythonapp.azurewebsites.net',
    )

lub włączyć dowolną:

    ALLOWED_HOSTS = (
        '*',
    )

W praktyce może chcesz zrobić coś bardziej złożone na zajmowanie przełączania się między debugowania oraz Zwolnij trybu i wprowadzenie nazwa hosta.

Zmienne środowiska za pośrednictwem portalu Azure stronie **CONFIGURE** , można ustawić w sekcji **Ustawienia aplikacji** .  Może to być przydatne w przypadku ustawienia wartości odpowiednią mogą nie być wyświetlane w źródłach (parametry połączenia, haseł itp.) lub chcesz ustawić inaczej między Azure i komputer lokalny. W `settings.py`, można wysyłać kwerendy zmiennych środowiska za pomocą `os.getenv`.


## <a name="using-a-database"></a>Korzystając z bazy danych

Bazy danych, która jest dołączona do aplikacji jest sqlite bazy danych. Jest to wygodne i użyteczne domyślną bazę danych używaną rozwoju, wymaga prawie konfiguracji. Baza danych znajduje się w pliku db.sqlite3 w folderze projektu.

Azure udostępnia usługi bazy danych, które są łatwe w użyciu z aplikacji Django. Samouczki dotyczące korzystania z [Bazy danych SQL] i [MySQL] z aplikacji Django Pokaż kroki niezbędne do utworzenia bazy danych usługi, zmienianie ustawień bazy danych w `DjangoWebProject/settings.py`i bibliotek wymagane do zainstalowania.

Oczywiście jeśli wolisz zarządzać serwery bazy danych, możesz wykonać za pomocą systemu Windows i Linux oraz maszyn wirtualnych uruchomionych Azure.


## <a name="django-admin-interface"></a>Django interfejsu administracyjnego

Po rozpoczęciu tworzenia modelach, chcesz wypełnić bazy danych zawierającego dane. Łatwe dodawanie i edytowanie zawartości interakcyjnie jest za pomocą interfejsu administracyjnego Django.

Kod interfejsu administracyjnego jest ujęta w komentarz w źródłach aplikacji, ale jest wyraźnie oznaczone, więc można je łatwo włączyć (wyszukiwanie "admin").

Po włączeniu, zsynchronizować bazę danych, uruchom aplikację i przejdź do strony `/admin`.


## <a name="next-steps"></a>Następne kroki

Wykonaj te łącza, aby dowiedzieć się więcej o Django i Python narzędzia programu Visual Studio:

- [Dokumentacja Django]
- [Narzędzia Python dokumentacji programu Visual Studio]

Aby uzyskać informacje na temat korzystania z bazy danych SQL i MySQL:

- [Django i MySQL Azure narzędziami Python programu Visual Studio]
- [Django i baza danych SQL Azure, korzystając z narzędzi Python programu Visual Studio]

Aby uzyskać więcej informacji zobacz [Centrum deweloperów Python](/develop/python/).


## <a name="whats-changed"></a>Informacje o zmianach
* Przewodnika do zmiany z witryn sieci Web do usługi aplikacji Zobacz: [Usługa Azure aplikacji i jego wpływ na istniejące usługi Azure](http://go.microsoft.com/fwlink/?LinkId=529714)


<!--Link references-->
[Django i MySQL Azure narzędziami Python programu Visual Studio]: web-sites-python-ptvs-django-mysql.md
[Django i baza danych SQL Azure, korzystając z narzędzi Python programu Visual Studio]: web-sites-python-ptvs-django-sql.md
[Baza danych SQL]: web-sites-python-ptvs-django-sql.md
[MySQL]: web-sites-python-ptvs-django-mysql.md

<!--External Link references-->
[Azure SDK dla Python 2.7]: http://go.microsoft.com/fwlink/?linkid=254281
[Azure SDK dla Python 3.4]: http://go.microsoft.com/fwlink/?linkid=516990
[Python.org]: http://www.python.org/
[Cyfra dla systemu Windows]: http://msysgit.github.io/
[GitHub dla systemu Windows]: https://windows.github.com/
[Narzędzia Python programu Visual Studio]: http://aka.ms/ptvs
[Python narzędzia 2.2 programu Visual Studio]: http://go.microsoft.com/fwlink/?LinkID=624025
[Programu Visual Studio]: http://www.visualstudio.com/
[Narzędzia Python dokumentacji programu Visual Studio]: http://aka.ms/ptvsdocs
[Dokumentacja Django]: https://www.djangoproject.com/
