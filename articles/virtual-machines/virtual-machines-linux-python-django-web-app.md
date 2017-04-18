<properties 
    pageTitle="Python web app z Django w systemie Linux | Microsoft Azure" 
    description="Dowiedz się, jak do obsługi aplikacji sieci web opartych na Django na Azure za pomocą maszyny wirtualnej Linux." 
    services="virtual-machines-linux" 
    documentationCenter="python" 
    authors="huguesv" 
    manager="wpickett" 
    editor=""
    tags="azure-resource-manager"/>

<tags 
    ms.service="virtual-machines-linux" 
    ms.workload="web" 
    ms.tgt_pltfrm="vm-linux" 
    ms.devlang="python" 
    ms.topic="article" 
    ms.date="11/17/2015" 
    ms.author="huvalo"/>
    
# <a name="django-hello-world-web-application-on-a-linux-vm"></a>Django Witaj świecie aplikacji sieci web na maszyny Linux

> [AZURE.SELECTOR]
- [Systemu Windows](virtual-machines-windows-classic-python-django-web-app.md)
- [Mac i Linux](virtual-machines-linux-python-django-web-app.md)

<br>

Ten samouczek opisano, jak udostępnić witrynę sieci Web opartych na Django na Microsoft Azure za pomocą maszyny wirtualnej Linux. Tego samouczka przyjęto założenie, że masz nie wcześniejszego doświadczenia w korzystaniu Azure. Po wykonaniu tego przewodnika, konieczne będzie Django aplikacja w górę i uruchamiania w chmurze.

Dowiesz się jak:

* Konfiguracja Azure maszyny wirtualnej hosta Django. Podczas tego samouczka wyjaśniono, jak to zrobić w obszarze **Linux**, to można również wykonać za pomocą maszyn wirtualnych systemu Windows Server obsługiwany w Azure. 
* Tworzenie nowej aplikacji Django z Linux.

Wykonując tego samouczka, zostanie utworzona prostych aplikacji sieci web Witaj świecie. Aplikacja będzie obsługiwany w Azure maszyn wirtualnych.

Zrzut ekranu przedstawiający wypełniony wniosek jest poniżej:

![Okno przeglądarki zawierające strony Witaj świecie Azure](./media/virtual-machines-linux-python-django-web-app/mac-linux-django-helloworld-browser.png)

[AZURE.INCLUDE [create-account-and-vms-note](../../includes/create-account-and-vms-note.md)]

## <a name="creating-and-configuring-an-azure-virtual-machine-to-host-django"></a>Tworzenie i konfigurowanie Azure maszyny wirtualnej hosta Django

1. Postępuj zgodnie z instrukcjami podane [tutaj](virtual-machines-linux-quick-create-portal.md) tworzenie Azure maszyn wirtualnych rozkładu *KÓW 14.04 serwera Ubuntu* .  Jeśli wolisz, możesz wybrać uwierzytelniania hasła zamiast kluczem publicznym SSH.

1. Edytowanie grupy zabezpieczeń sieci, aby zezwolić na ruch przychodzący http do portu 80 zgodnie z instrukcjami zawartymi [w tym miejscu](../virtual-network/virtual-networks-create-nsg-arm-pportal.md).

1. Domyślnie usługi nowa maszyna wirtualna nie jest w pełni kwalifikowaną nazwę domeny (FQDN).  Możesz utworzyć je, postępując zgodnie z instrukcjami [poniżej](virtual-machines-linux-portal-create-fqdn.md).  Ten krok jest opcjonalny do użycia tego samouczka.

## <a id="setup"> </a>Konfigurowanie środowiska projektowego

**Uwaga:** Jeśli musisz zainstalować Python lub ma zostać użyty bibliotek klienta, zobacz [Podręcznik instalacji Python](../python-how-to-install.md).

Maszyn wirtualnych Linux Ubuntu zawiera już 2.7 Python wstępnie zainstalowany, ale nie zawiera on Apache lub django zainstalowany.  Wykonaj poniższe czynności, aby połączyć się z maszyn wirtualnych i zainstalować Apache i django.

1.  Uruchom nowe okno **Terminal** .
    
1.  Wpisz następujące polecenie, aby nawiązać połączenie maszyn wirtualnych Azure.  Jeśli nie utworzysz FQDN umożliwia nawiązanie połączenia przy użyciu publicznego adresu IP wyświetlane sumaryczne w portalu klasyczny Azure maszyny wirtualnej.

        $ ssh yourusername@yourVmUrl

1.  Wpisz następujące polecenia, aby zainstalować django:

        $ sudo apt-get install python-setuptools python-pip
        $ sudo pip install django

1.  Wpisz następujące polecenie, aby zainstalować Apache mod wsgi:

        $ sudo apt-get install apache2 libapache2-mod-wsgi


## <a name="creating-a-new-django-application"></a>Tworzenie nowej aplikacji Django

1.  Otwórz okno **Terminal** używanego w poprzedniej sekcji, aby ssh do swojego maszyn wirtualnych.
    
1.  Wpisz następujące polecenia, aby utworzyć nowy projekt Django:

        $ cd /var/www
        $ sudo django-admin.py startproject helloworld

    Skrypt **django admin.py** generuje podstawowej struktury witryn sieci Web opartych na Django:
    -   **HelloWorld/Manage.PY** ułatwia rozpoczęcie hostingu i zatrzymywanie hostingu witryny sieci Web opartych na Django
    -   **HelloWorld/HelloWorld/Settings.PY** zawiera Django ustawienia aplikacji.
    -   **HelloWorld/HelloWorld/URLs.PY** zawiera kod mapowania między każdego adresu url i swoją opinię.

1.  Tworzenie nowego pliku o nazwie **views.py** w katalogu **/var/www/helloworld/helloworld** . To będzie zawierać widoku są renderowane za pomocą strony "Witaj świecie". Uruchom Edytor, a następnie wprowadź następujące informacje:
        
        from django.http import HttpResponse
        def home(request):
            html = "<html><body>Hello World!</body></html>"
            return HttpResponse(html)

1.  Teraz zamienić zawartość pliku **urls.py** następujące czynności:

        from django.conf.urls import patterns, url
        urlpatterns = patterns('',
            url(r'^$', 'helloworld.views.home', name='home'),
        )


## <a name="setting-up-apache"></a>Aby skonfigurować Apache

1.  Tworzenie pliku konfiguracji hosta wirtualnego Apache **/etc/apache2/sites-available/helloworld.conf**. Ustawianie zawartość do następujących i zastąpić *yourVmName* rzeczywista nazwa komputera, gdy używasz (na przykład *pyubuntu*).

        <VirtualHost *:80>
        ServerName yourVmName
        </VirtualHost>
        WSGIScriptAlias / /var/www/helloworld/helloworld/wsgi.py
        WSGIPythonPath /var/www/helloworld

1.  Włączanie witryn przy użyciu następującego polecenia:

        $ sudo a2ensite helloworld

1.  Uruchom ponownie Apache przy użyciu następującego polecenia:

        $ sudo service apache2 reload

1.  Na koniec ładowanie strony sieci web w przeglądarce:

    ![Okno przeglądarki zawierające strony Witaj świecie Azure](./media/virtual-machines-linux-python-django-web-app/mac-linux-django-helloworld-browser.png)


## <a name="shutting-down-your-azure-virtual-machine"></a>Zamykanie usługi Azure maszyny wirtualnej

Po zakończeniu z tego samouczka, zamknięcia i/lub usuń maszyny wirtualnej nowo utworzonego Azure, aby zwolnić zasoby dla innych samouczków dotyczących i nie przyjmować Azure kosztów.
