<properties 
    pageTitle="Jak zainstalować Qpid Apache protonów-C na maszyny Linux | Microsoft Azure"
    description="Jak utworzyć maszyny Linux CentOS przy użyciu maszyn wirtualnych Azure i jak tworzyć i zainstalować biblioteki Qpid Apache protonów-C."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" /> 
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/29/2016"
    ms.author="sethm" />

# <a name="install-apache-qpid-proton-c-on-an-azure-linux-vm"></a>Instalowanie Apache Qpid protonów C na Azure maszyn wirtualnych Linux

[AZURE.INCLUDE [service-bus-selector-amqp](../../includes/service-bus-selector-amqp.md)]

W tej sekcji przedstawiono sposób tworzenia maszyn wirtualnych CentOS Linux przy użyciu maszyn wirtualnych Azure i jak pobrać, tworzenie i zainstalować biblioteki Qpid Apache protonów-C wraz z powiązań języka Python i PHP. Po wykonaniu tych kroków, można uruchomić przykłady Python i PHP dołączone do tego przewodnika.

Pierwszym krokiem jest wykonywane za pomocą [portal Azure klasyczny][]. Na poniższym zrzucie ekranu przedstawiono sposób tworzenia maszyny CentOS o nazwie "Scotta centos":

![Protonów na Azure maszyn wirtualnych Linux][0]

Po zainicjowaniu obsługi administracyjnej, portalu wyświetli następujące informacje:

![Protonów na Azure maszyn wirtualnych Linux][1]

Aby zalogować się do komputera, konieczna jest znajomość port punktu końcowego dla SSH. Ta wartość można uzyskać z [portal Azure klasyczny][] , wybierając nowo utworzonego maszyn wirtualnych i klikając na karcie **punktów końcowych** . Na poniższym zrzucie ekranu pokazano, że publicznej port SSH dla tego komputera jest 57146.

![Protonów na Azure maszyn wirtualnych Linux][2]

Teraz można podłączyć do komputera za pomocą SSH. W tym przykładzie użyto narzędzie Kit, tak jak w następujących zrzut ekranu:

![Protonów na Azure maszyn wirtualnych Linux][3]

W przypadku aplikacji Python i PHP w tym przykładzie użyto protonów bibliotek klienta z Apache. Te biblioteki są dostępne do pobrania w [http://qpid.apache.org/download.html](http://qpid.apache.org/download.html). Plik Readme pakietu dystrybucji opisano kroki wymagane do zainstalowania zależności i tworzenie protonów. Poniżej przedstawiono podsumowanie kroków:

1.  Edytowanie pliku konfiguracji yum (-etc/yum.conf) i komentarz się wyłączenie aktualizacji do nagłówków jądra (\# wykluczyć = jądra\*). Jest to konieczne zainstalować kompilatora Zatoce.

2.  Zainstalować pakiety wstępne:

    ```
    # required dependencies 
    yum install gcc cmake libuuid-devel
    
    # dependencies needed for ssl support
    yum install openssl-devel
    
    # dependencies needed for bindings
    yum install swig python-devel ruby-devel php-devel java-1.6.0-openjdk
    
    # dependencies needed for python docs
    yum install epydoc
    ```

1.  Pobieranie biblioteki protonów:

    ```
    [azureuser@this-user ~]$ wget http://apache.panu.it/qpid/proton/0.9/qpid-proton-0.9.tar.gz
    --2016-04-17 14:45:03--  http://apache.panu.it/qpid/proton/0.9/qpid-proton-0.9.tar.gz
    Resolving apache.panu.it (apache.panu.it)... 81.208.22.71
    Connecting to apache.panu.it (apache.panu.it)|81.208.22.71|:80... connected.
    HTTP request sent, awaiting response... 200 OK
    Length: 868226 (848K) [application/x-gzip]
    Saving to: ‘qpid-proton-0.9.tar.gz’
    
    qpid-proton-0.9.tar.gz                               
    
    100%[====================================================================================================================>] 847.88K   102KB/s    in 8.4s    
    
    2016-04-17 14:45:12 (101 KB/s) - ‘qpid-proton-0.9.tar.gz’ saved [868226/868226]
    ```

1.  Wyodrębnianie kodu protonów z archiwum dystrybucyjnej:

    ```
    tar xvfz qpid-proton-0.9.tar.gz
    ```

1.  Tworzenie i zainstaluj kodu, wykonując poniższe czynności, pobrane z pliku Readme:

    ```
    From the directory where you found this README file:    
    
    mkdir build cd build
            
    # Set the install prefix. You may need to adjust depending on your      
    # system.       
    cmake -DCMAKE\_INSTALL\_PREFIX=/usr ..
            
    # Omit the docs target if you do not wish to build or install       
    # documentation.        
    make all docs
            
    # Note that this step will require root privileges.     
    make install
    ```

Po wykonaniu tych kroków, protonów jest zainstalowana na komputerze i gotowe do użycia.

## <a name="next-steps"></a>Następne kroki

Chcesz dowiedzieć się więcej? Odwiedź następujące łącze:

- [Omówienie usług Bus AMQP][]

[Omówienie usług Bus AMQP]: service-bus-amqp-overview.md
[0]: ./media/service-bus-amqp-apache/amqp-apache-1.png
[1]: ./media/service-bus-amqp-apache/amqp-apache-2.png
[2]: ./media/service-bus-amqp-apache/amqp-apache-3.png
[3]: ./media/service-bus-amqp-apache/amqp-apache-4.png

[Portal Azure klasyczny]: http://manage.windowsazure.com


