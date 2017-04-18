<properties
    pageTitle="Goście Linux Azure stosu | Microsoft Azure"
    description="Dowiedz się, jak utworzyć systemem Linux maszyn wirtualnych w stos Azure."
    services="azure-stack"
    documentationCenter=""
    authors="anjayajodha"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="anajod"/>
    
# <a name="deploy-linux-virtual-machines-on-azure-stack"></a>Wdrażanie maszyn wirtualnych Linux stosu Azure

Należy wdrożyć maszyn wirtualnych Linux na Zapewnić stosem Azure, dodając do Azure Marketplace stosem obraz z systemem Linux. Można utworzyć własne lub kilku dostawców Linux została przygotowana obrazów, które można dodawać do zatwierdzenia Koncepcji Azure stosu.

## <a name="download-an-image"></a>Pobierz obraz

 1. Pobieranie i wyodrębnianie obrazu Azure stos zgodnego z poniższych łączy lub przygotowania własnych:
  - [Bitnami](https://bitnami.com/azure-stack)
  - [CentOS](http://olstacks.cloudapp.net/latest/)
  - [CoreOS](https://stable.release.core-os.net/amd64-usr/current/coreos_production_azure_image.vhd.bz2)
  - [SuSE](https://download.suse.com/Download?buildid=VCFi7y7MsFQ~)
  - [Ubuntu 14.04 KÓW](https://partner-images.canonical.com/azure/azure_stack/) / [Ubuntu 16.04 KÓW](http://cloud-images.ubuntu.com/releases/xenial/release/ubuntu-16.04-server-cloudimg-amd64-disk1.vhd.zip)
  
 2. Wyodrębnianie obrazu wirtualnego dysku twardego w razie potrzeby i [Dodawanie obrazu do witryny Marketplace programu](azure-stack-add-vm-image.md). Upewnij się, że `OSType` parametr jest ustawiony na `Linux`.
 
 3. Po dodaniu obrazu do witryny Marketplace programu, utworzeniu elementu Marketplace i wdrożeniem maszyny wirtualnej Linux.
  
## <a name="prepare-your-own-image"></a>Przygotowywanie własnego obrazu

1. Przygotowywanie własnego obrazu Linux przy użyciu jednej z następujących instrukcji:
 - [Oparte na centOS dystrybucji](../virtual-machines/virtual-machines-linux-create-upload-centos.md)
 - [Debian Linux](../virtual-machines/virtual-machines-linux-debian-create-upload-vhd.md)
 - [Oracle Linux](../virtual-machines/virtual-machines-linux-oracle-create-upload-vhd.md)
 - [Czerwone funkcję Enterprise Linux](../virtual-machines/virtual-machines-linux-redhat-create-upload-vhd.md)
 - [SLES & openSUSE](../virtual-machines/virtual-machines-linux-suse-create-upload-vhd.md)
 - [Ubuntu](../virtual-machines/virtual-machines-linux-create-upload-ubuntu.md)

2. Pobieranie i instalowanie [Agenta Linux Azure](https://github.com/Azure/WALinuxAgent/)

    Linux Agent Azure wersji 2.1.3 lub wyższej jest wymagane do obsługi administracyjnej usługi maszyn wirtualnych Linux Azure stosu. Ta wersja agenta lub wyższej jako pakiet wiele dystrybucji wymienionych powyżej już zawiera w ich repozytoria (zwykle o nazwie `WALinuxAgent` lub `walinuxagent`). Jednak jeśli wersja pakietu Azure agenta jest mniejsza niż 2.1.3 (to znaczy 2.0.18 lub niższego poziomu), a następnie należy ręcznie zainstalować agenta. Zainstalowana wersja można określić z nazwa pakietu lub uruchamiając `/usr/sbin/waagent -version` na maszyn wirtualnych.

    Postępuj zgodnie z instrukcjami poniżej ręcznie - zainstalować agenta Azure Linux

 - Najpierw pobierz najnowszą wersję agent Azure Linux z [Github](https://github.com/Azure/WALinuxAgent/releases)przykład:

            # wget https://github.com/Azure/WALinuxAgent/archive/v2.2.0.tar.gz

 - Rozpakowywanie Azure agenta:

            # tar -vzxf v2.2.0.tar.gz

 - Instalowanie python setuptools

        **Debian / Ubuntu**

            # sudo apt-get update
            # sudo apt-get install python-setuptools

        **Ubuntu 16.04+**

            # sudo apt-get install python3-setuptools

        **RHEL / CentOS / Oracle Linux**

            # sudo yum install python-setuptools

 - Zainstaluj Azure agenta:

            # cd WALinuxAgent-2.2.0
            # sudo python setup.py install --register-service

    Systemy z Python 2.x i Python 3.x zainstalowany przez siebie może być konieczne uruchom następujące polecenie:

        # sudo python3 setup.py install --register-service

    Aby uzyskać więcej informacji zobacz Linux Agent Azure [— Plik README](https://github.com/Azure/WALinuxAgent/blob/master/README.md).

3. [Dodawanie obrazu do witryny Marketplace programu](azure-stack-add-vm-image.md). Upewnij się, że `OSType` parametr jest ustawiony na `Linux`.

4. Po dodaniu obrazu do witryny Marketplace programu, utworzeniu elementu Marketplace i wdrożeniem maszyny wirtualnej Linux.

## <a name="next-steps"></a>Następne kroki

[Często zadawane pytania dotyczące stos Azure](azure-stack-faq.md)