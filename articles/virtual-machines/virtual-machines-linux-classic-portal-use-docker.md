<properties
    pageTitle="Za pomocą rozszerzenia maszyn wirtualnych Docker Linux | Microsoft Azure"
    description="W tym artykule opisano Docker i rozszerzenia maszyn wirtualnych Azure i sposobu tworzenia maszyn wirtualnych Azure, które hostów docker za pomocą interfejsu wiersza polecenia Azure w modelu Klasyczny wdrożenia."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="squillace"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="05/27/2016"
    ms.author="rasquill"/>


# <a name="using-the-docker-vm-extension-with-the-azure-classic-portal"></a>Rozszerzenie maszyn wirtualnych Docker za pomocą portalu klasyczny Azure

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


[Docker](https://www.docker.com/) jest jedną z najpopularniejszych metod wirtualizacji używanych [kontenerów Linux](http://en.wikipedia.org/wiki/LXC) zamiast w środowisku maszyn wirtualnych systemu sposób izolowanie danych i korzystania z komputera do zasobów udostępnionych. Rozszerzenie Docker maszyn wirtualnych zarządzane przez [Agenta Linux Azure] umożliwia tworzenie maszyny Docker obsługującego dowolną liczbę kontenerów dla aplikacji Azure.

> [AZURE.NOTE] W tym temacie opisano sposób tworzenia maszyny Docker z portalu klasyczny Azure. Aby zobaczyć, jak utworzyć maszyny Docker w wierszu polecenia, zobacz, [jak korzystać z rozszerzeniem maszyn wirtualnych Docker z interfejsu wiersza polecenia Azure (polecenie Azure)]. Aby uzyskać omówienie wysokiego poziomu kontenerów i ich korzyści, zobacz [Docker wysoki poziom tablicy](http://channel9.msdn.com/Blogs/Regular-IT-Guy/Docker-High-Level-Whiteboard).

## <a name="create-a-new-vm-from-the-image-gallery"></a>Tworzenie nowych maszyn wirtualnych z galerii obrazów
Pierwszym krokiem wymaga maszyn wirtualnych Azure z obrazu Linux obsługującego rozszerzenia maszyn wirtualnych Docker, używając obrazu KÓW 14.04 Ubuntu z galerii obrazów jako przykład obrazu serwera i pulpitu 14.04 Ubuntu jako klienta. W portalu kliknij **+ Nowa** w lewym dolnym rogu do tworzenia nowego wystąpienia maszyn wirtualnych i wybierz obraz KÓW 14.04 Ubuntu z wyborów dostępnych lub z galerii wykonania obrazu, tak jak pokazano poniżej.

> [AZURE.NOTE] Obecnie tylko KÓW 14.04 Ubuntu obrazów nowsza niż lipiec 2014 obsługuje rozszerzenia maszyn wirtualnych Docker.

![Tworzenie nowego obrazu Ubuntu](./media/virtual-machines-linux-classic-portal-use-docker/ChooseUbuntu.png)

## <a name="create-docker-certificates"></a>Tworzenie certyfikatów Docker

Po utworzeniu maszyn wirtualnych upewnij się, że Docker jest zainstalowany na komputerze klienckim użytkownika. (Aby uzyskać szczegółowe informacje, zobacz [instrukcje dotyczące instalacji Docker](https://docs.docker.com/installation/#installation)).

Tworzenie certyfikatu i klucza plików komunikacji Docker zgodnie z [Systemem Docker przy użyciu protokołu https] , a następnie umieść je w **`~/.docker`** katalogu na komputerze klienckim użytkownika.

> [AZURE.NOTE] Rozszerzenie maszyn wirtualnych Docker w portalu obecnie wymaga poświadczeń base64 zakodowany.

W wierszu polecenia użyj **`base64`** lub innego narzędzia kodowania Ulubione do tworzenia zakodowany base64 tematów. W ten sposób za pomocą prostego zestawu plików certyfikatu i klucza może wyglądać następująco:

```
 ~/.docker$ ls
 ca-key.pem  ca.pem  cert.pem  key.pem  server-cert.pem  server-key.pem
 ~/.docker$ base64 ca.pem > ca64.pem
 ~/.docker$ base64 server-cert.pem > server-cert64.pem
 ~/.docker$ base64 server-key.pem > server-key64.pem
 ~/.docker$ ls
 ca64.pem    ca.pem    key.pem            server-cert.pem   server-key.pem
 ca-key.pem  cert.pem  server-cert64.pem  server-key64.pem
```

## <a name="add-the-docker-vm-extension"></a>Dodawanie numeru wewnętrznego maszyn wirtualnych Docker
Aby dodać rozszerzenie maszyn wirtualnych Docker, Znajdź utworzony wystąpienie maszyn wirtualnych i przewiń w dół do **rozszerzenia** i kliknij go, aby wyświetlić rozszerzenia maszyn wirtualnych, tak jak pokazano poniżej.
> [AZURE.NOTE] Ta funkcja jest obsługiwana w portalu Podgląd tylko: https://portal.azure.com/

![](./media/virtual-machines-linux-classic-portal-use-docker/ClickExtensions.png)
### <a name="add-an-extension"></a>Dodawanie numeru wewnętrznego
Kliknij przycisk **+ Dodaj** do wyświetlenia możliwe rozszerzenia maszyn wirtualnych można dodać do tego maszyn wirtualnych.

![](./media/virtual-machines-linux-classic-portal-use-docker/ClickAdd.png)
### <a name="select-the-docker-vm-extension"></a>Zaznacz rozszerzenie maszyn wirtualnych Docker
Wybierz rozszerzenie maszyn wirtualnych Docker spowoduje wyświetlenie opisu Docker i ważne łącza, i kliknij pozycję **Utwórz** u dołu, aby rozpocząć procedurę instalacji.

![](./media/virtual-machines-linux-classic-portal-use-docker/ChooseDockerExtension.png)

![](./media/virtual-machines-linux-classic-portal-use-docker/CreateButtonFocus.png)
### <a name="add-your-certificate-and-key-files"></a>Dodawanie certyfikatu i kluczy plików:

W polach formularza wprowadź zakodowany base64 wersji certyfikatu urzędu certyfikacji, certyfikatu serwera i klucza serwera, jak pokazano na poniższym rysunku.

![](./media/virtual-machines-linux-classic-portal-use-docker/AddExtensionFormFilled.png)

> [AZURE.NOTE] Należy zauważyć, że (tak jak w poprzednim obraz) 2376 zostanie wypełnione domyślnie. Możesz wprowadzić dowolny punkt końcowy poniżej, ale następnym krokiem jest otwarcie zgodnej punktu końcowego. Jeśli zmienisz wartość domyślna, upewnij się, otwarcie pasujące punkt końcowy w następnym kroku.

## <a name="add-the-docker-communication-endpoint"></a>Dodawanie punktu końcowego komunikacji Docker
Podczas przeglądania grupa zasobów, który został utworzony, zaznacz grupę zabezpieczeń sieci skojarzonych z Twojej maszyn wirtualnych, a następnie kliknij przycisk **Reguły przychodzące zabezpieczeń** , aby wyświetlić reguły, jak pokazano na ilustracji.

![](./media/virtual-machines-linux-classic-portal-use-docker/AddingEndpoint.png)

Kliknij przycisk **+ Dodaj** Aby dodać kolejnej reguły, a w przypadku domyślnego, wpisz nazwę dla punktu końcowego (w tym przykładzie **Docker**) oraz 2376 "zakres Port docelowy". Ustaw wartości z pola Protokół przedstawiający **TCP**i kliknij **przycisk OK** , aby utworzyć regułę.

![](./media/virtual-machines-linux-classic-portal-use-docker/AddEndpointFormFilledOut.png)


## <a name="test-your-docker-client-and-azure-docker-host"></a>Testowanie Docker klientem a Azure Docker hosta
Lokalizowanie i kopiowanie nazwę domeny swojego maszyn wirtualnych, a w wierszu polecenia komputera klienta, typ `docker --tls -H tcp://` *dockerextension* `.cloudapp.net:2376 info` (gdzie *dockerextension* zastępuje poddomeny dla swojego maszyn wirtualnych).

Wynik powinna wyglądać podobnie do następującej:

```
$ docker --tls -H tcp://dockerextension.cloudapp.net:2376 info
Containers: 0
Images: 0
Storage Driver: devicemapper
 Pool Name: docker-8:1-131214-pool
 Pool Blocksize: 65.54 kB
 Data file: /var/lib/docker/devicemapper/devicemapper/data
 Metadata file: /var/lib/docker/devicemapper/devicemapper/metadata
 Data Space Used: 305.7 MB
 Data Space Total: 107.4 GB
 Metadata Space Used: 729.1 kB
 Metadata Space Total: 2.147 GB
 Library Version: 1.02.82-git (2013-10-04)
Execution Driver: native-0.2
Kernel Version: 3.13.0-36-generic
WARNING: No swap limit support
```

Po wykonaniu powyższych czynności teraz masz pełną funkcjonalność hosta Docker uruchomionych Głosowa Azure, skonfigurowany do połączenia do zdalnego z innymi klientami.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Następne kroki

Możesz przystąpić do Przechodzenie do [Docker User Guide] i używanie maszyn wirtualnych usługi Docker. Jeśli chcesz zautomatyzować tworzenie hosts Docker na maszyny wirtualne Azure za pośrednictwem interfejsu wiersza polecenia, Dowiedz się, [jak korzystać z rozszerzeniem maszyn wirtualnych Docker z interfejsu wiersza polecenia Azure (polecenie Azure)]

<!--Anchors-->
[Create a new VM from the Image Gallery]: #createvm
[Create Docker Certificates]: #dockercerts
[Add the Docker VM Extension]: #adddockerextension
[Test Docker Client and Azure Docker Host]: #testclientandserver
[Next steps]: #next-steps

<!--Image references-->
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[6]: ./media/markdown-template-for-new-articles/pretty49.png
[7]: ./media/markdown-template-for-new-articles/channel-9.png


<!--Link references-->
[Jak korzystać z rozszerzeniem maszyn wirtualnych Docker z interfejsu wiersza polecenia Azure (polecenie Azure)]: http://azure.microsoft.com/documentation/articles/virtual-machines-docker-with-xplat-cli/
[Azure Linux Agent]: virtual-machines-linux-agent-user-guide.md
[Link 3 to another azure.microsoft.com documentation topic]: ../storage-whatis-account.md

[Uruchamianie Docker przy użyciu protokołu https]: http://docs.docker.com/articles/https/
[Przewodnik użytkownika docker]: https://docs.docker.com/userguide/
