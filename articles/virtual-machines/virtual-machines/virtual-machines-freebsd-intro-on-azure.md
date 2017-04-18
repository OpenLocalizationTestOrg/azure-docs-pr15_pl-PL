<properties
   pageTitle="Wprowadzenie do FreeBSD Azure | Microsoft Azure"
   description="Informacje o korzystaniu z maszyn wirtualnych FreeBSD Azure"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="KylieLiang"
   manager="timlt"
   editor=""
   tags="azure-service-management"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="08/27/2016"
   ms.author="kyliel"/>

# <a name="introduction-to-freebsd-on-azure"></a>Wprowadzenie do FreeBSD Azure
W tym temacie omówiono uruchamiania maszyny wirtualnej FreeBSD w Azure.

## <a name="overview"></a>Omówienie
FreeBSD dla Microsoft Azure jest system operacyjny komputera zaawansowane używane serwery power nowoczesny, komputery stacjonarne i osadzone platformy. Obraz FreeBSD 10.3 jest dostarczany przez firmę Microsoft Corporation i jest dostępna w Azure. Jest on oparty na wersji FreeBSD 10.3 i Azure agenta gościa maszyn wirtualnych [2.1.4](https://github.com/Azure/WALinuxAgent/releases/tag/v2.1.4) jest zainstalowany. Agent jest odpowiedzialny za komunikacji między maszyn wirtualnych FreeBSD i Azure tkaninie dla operacji, takich jak inicjowania obsługi administracyjnej maszyn wirtualnych przy pierwszym użyciu (nazwa użytkownika, hasło, nazwa hosta itp.) i włączanie funkcji selektywnego rozszerzeń maszyn wirtualnych.
Jak w przyszłych wersjach systemu FreeBSD strategii jest aktualne i udostępnić najnowszej wersji, krótko po ich wydawanych przez zespół inżynierów wersji FreeBSD. Nowej wersji jest [FreeBSD 11](https://www.freebsd.org/releases/11.0R/schedule.html).

## <a name="deploying-a-freebsd-virtual-machine"></a>Wdrażanie maszyny wirtualnej FreeBSD
Wdrażanie maszyny wirtualnej FreeBSD jest proste proces przy użyciu obrazu z [Usługi Azure Marketplace](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd103/).

## <a name="vm-extensions-for-freebsd"></a>Rozszerzenia maszyn wirtualnych FreeBSD
Oto obsługiwane rozszerzenia maszyn wirtualnych w FreeBSD.

### <a name="vmaccess"></a>VMAccess

Rozszerzenie [VMAccess](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess) wykonywać następujące czynności:

- Resetowanie hasła użytkownika sudo oryginalny.
- Tworzenie nowego użytkownika sudo z podane hasło.
- Ustaw klucz publiczny hosta kluczem podane.
- Zresetować klucz publiczny hosta podczas maszyn wirtualnych inicjowania obsługi administracyjnej, jeśli nie określono klucza hosta.
- Otwórz port SSH (22) i przywracanie sshd_config, jeśli reset_ssh jest ustawiona na PRAWDA.
- Usuwanie istniejącego użytkownika.
- Sprawdzanie dysków.
- Naprawianie dodanym dyskiem.

### <a name="customscript"></a>CustomScript

Rozszerzenie [CustomScript](https://github.com/Azure/azure-linux-extensions/tree/master/CustomScript) wykonywać następujące czynności:

- Jeśli podano pobrać dostosowane skrypty magazyn Azure lub zewnętrzne publicznej miejsce do magazynowania (na przykład GitHub).
- Uruchom skrypt punktu wejścia.
- Obsługuje poleceń w tekście.
- Konwertowanie stylu systemu Windows nowego wiersza w powłoki i skryptów Python automatycznie.
- Automatyczne usuwanie BOM w powłoki i Python skryptów.
- Ochrona ważnych danych w CommandToExecute.

## <a name="authentication-user-names-passwords-and-ssh-keys"></a>Uwierzytelnianie: nazwy użytkowników, hasła i klucze SSH
Podczas tworzenia maszyny wirtualnej FreeBSD za pomocą portalu Azure, musisz podać nazwę użytkownika, hasło lub kluczem publicznym SSH.
Nazwy użytkowników do wdrażania maszyny wirtualnej FreeBSD Azure nie musi odpowiadać nazwy konta systemowe (UID < 100) już istnieje w maszyny wirtualnej ("root," na przykład).
Obecnie jest obsługiwana tylko RSA SSH klucz. Wielowierszowe klucz SSH musi zaczynają się od "---początkowego SSH2 kluczem publicznym---" i kończy się "---zakończenia SSH2 kluczem publicznym---".

## <a name="obtaining-superuser-privileges"></a>Uzyskiwanie uprawnień administratora
Konto użytkownika, które zostało określone podczas wdrażania wystąpienie maszyn wirtualnych Azure jest kontem uprzywilejowanych. Zainstalowano pakiet sudo opublikowanych obrazu FreeBSD.
Po Cię zalogowano za pośrednictwem tego konta użytkownika polecenia jako główny można wykonać przy użyciu składni polecenia.

    # sudo <COMMAND>

Opcjonalnie można uzyskać powłoki głównego przy użyciu sudo -s.

## <a name="next-steps"></a>Następne kroki
- Przejdź do [Usługi Azure Marketplace](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd103/) , aby utworzyć maszyny FreeBSD.
- Jeśli chcesz wyświetlić własne FreeBSD Azure, zobacz [Tworzenie i przekazywanie FreeBSD wirtualnego dysku twardego Azure](../virtual-machines-linux-classic-freebsd-create-upload-vhd.md).
