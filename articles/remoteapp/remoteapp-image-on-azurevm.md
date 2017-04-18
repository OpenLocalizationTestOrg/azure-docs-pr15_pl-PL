<properties
    pageTitle="Tworzenie obrazu Azure RemoteApp według maszyn wirtualnych Azure | Microsoft Azure"
    description="Dowiedz się, jak utworzyć obraz dla Azure RemoteApp, zaczynając od Azure maszyn wirtualnych."
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016" 
    ms.author="elizapo" />



# <a name="create-a-azure-remoteapp-image-based-on-an-azure-virtual-machine"></a>Tworzenie obrazu Azure RemoteApp według Azure maszyn wirtualnych

> [AZURE.IMPORTANT]
> Azure RemoteApp jest są przerywane. W tym artykule [anons](https://go.microsoft.com/fwlink/?linkid=821148) , aby uzyskać szczegółowe informacje.

Funkcja RemoteApp platformy Azure obrazy (naciśnij i przytrzymaj aplikacje, które udostępniasz w zbiorze) można utworzyć z Azure maszyn wirtualnych. Można także użyć obrazu maszyn wirtualnych dodane do galerii obrazów Azure maszyn wirtualnych spełniającego wszystkie wymagania obraz Azure RemoteApp — umożliwiają obrazu maszyn wirtualnych jako punktu startowego własne Głosowa, jeśli chcesz. Znajdź obraz "Windows Server hosta sesji pulpitu zdalnego" w bibliotece.

Istnieją dwa kroki Tworzenie własnego obrazu według maszyn wirtualnych Azure — Tworzenie obrazu, a następnie przekazanie go w bibliotece Azure maszyn wirtualnych do Azure RemoteApp.

## <a name="create-a-custom-image-based-on-an-azure-vm"></a>Tworzenie niestandardowego obrazu według maszyn wirtualnych Azure

Wykonaj poniższe kroki, aby utworzyć obraz według maszyn wirtualnych Azure.

1. Tworzenie Azure maszyn wirtualnych. Za pomocą "Windows Server hosta sesji pulpitu zdalnego" lub "Systemu Windows Server zdalnego pulpitu sesji hosta z programu Microsoft Office 365 ProPlus" obraz z galerii obrazów Azure maszyn wirtualnych. Ten obraz spełnia wszystkie Azure RemoteApp szablonu obraz wymagania.

    Aby uzyskać szczegółowe informacje zobacz [Tworzenie maszyn wirtualnych z systemem Windows](../virtual-machines/virtual-machines-windows-hero-tutorial.md).

2. Nawiązywanie połączenia z maszyn wirtualnych i zainstaluj i skonfiguruj aplikacje, które chcesz udostępnić przy użyciu funkcji RemoteApp. Pamiętaj wykonać dodatkowe konfiguracji systemu Windows wymagane przez aplikacje.

    Aby uzyskać szczegółowe informacje zobacz [jak zalogować się do maszyn wirtualnych uruchamiania systemu Windows Server](../virtual-machines/virtual-machines-windows-classic-connect-logon.md).

3. Jeśli używasz jednego z obrazów hosta sesji pulpitu zdalnego serwera systemu Windows jest dołączany sprawdzania poprawności skrypt, który będzie upewnij się, że do maszyn wirtualnych spełnia RemoteApp litery pre-reqs. Aby uruchomić skrypt, kliknij dwukrotnie **ValidateRemoteAppImage** na komputerze. Upewnij się, wszystkie błędy zgłoszone przez skrypt rozwiązywane przed przejściem do następnego kroku.

4. SYSPREP uogólniony, a przechwycić obraz. Aby uzyskać instrukcje, zobacz [jak przechwycić maszyny wirtualnej systemu Windows do użycia jako szablonu](../virtual-machines/virtual-machines-windows-classic-capture-image.md) .



## <a name="import-the-image-into-the-azure-remoteapp-image-library"></a>Importowanie obrazu do biblioteki obrazów Azure RemoteApp

Aby zaimportować nowy obraz do Azure RemoteApp, wykonaj następujące kroki:

1. Na karcie **Obrazów szablonów** :
    - Jeśli masz nie istniejących obrazów, kliknij pozycję **Przekaż lub importowanie obraz szablonu**.
    - Jeśli masz już co najmniej jeden obraz, kliknij przycisk **+** Aby dodać nowy obraz.

2. Wybierz pozycję **Importuj obraz z maszyn wirtualnych** biblioteki, a następnie kliknij przycisk **Dalej**.

3. Na następnej stronie Wybierz obraz niestandardowy z listy, a następnie upewnij się, że wykonano kroki wymienione po utworzeniu obrazu. Kliknij przycisk **Dalej**.
4. Wprowadź nazwę dla nowego obrazu RemoteApp i wybierz lokalizację, a następnie kliknij znacznik wyboru, aby rozpocząć proces importowania.

> [AZURE.NOTE] Obrazy można zaimportować z dowolnej lokalizacji Azure obsługiwane przez maszyn wirtualnych Azure w dowolnej lokalizacji Azure obsługiwanych przez Azure RemoteApp. W zależności od lokalizacji importowanie może potrwać maksymalnie 25 minut.

Teraz możesz przystąpić do tworzenia nowej kolekcji, albo w [chmurze](remoteapp-create-cloud-deployment.md) kolekcji lub [hybrydowe](remoteapp-create-hybrid-deployment.md), w zależności od potrzeb.
