
<properties
    pageTitle="Azure wymagania obraz RemoteApp | Microsoft Azure"
    description="Więcej informacji na temat wymagań dotyczących tworzenia obrazów do użytku z Azure RemoteApp"
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



# <a name="requirements-for-azure-remoteapp-images"></a>Wymagania dotyczące obrazów Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp jest są przerywane. W tym artykule [anons](https://go.microsoft.com/fwlink/?linkid=821148) , aby uzyskać szczegółowe informacje.

Azure RemoteApp używa obrazu Windows Server 2012 R2, aby obsługiwać wszystkie programy, które chcesz udostępnić innym użytkownikom. Aby utworzyć obraz niestandardowy, można uruchomić z istniejącym obrazem lub [Utwórz nowy](remoteapp-create-custom-image.md).

> [AZURE.TIP] Czy wiesz, że Twoja subskrypcja Azure RemoteApp umożliwia dostęp do obrazu systemu Windows Server 2012 R2 w galerii Azure maszyn wirtualnych, które umożliwiają tworzenie własnego obrazu szablonu? [Wyewidencjonuj](remoteapp-image-on-azurevm.md).  


Wymagania dotyczące obrazu, który można przekazywać do użytku z Azure RemoteApp są:


- Aplikacje niestandardowe nie przechowywanie danych lokalnie na obrazie. Te obrazy są bezstanowe i powinien zawierać tylko aplikacji.
- Obraz nie zawiera dane, które mogą zostać utracone.
- Rozmiar obrazu powinny być wielokrotnością MB. Jeśli spróbujesz przekazywanie obrazu, który nie jest wielokrotnością, przekazywanie nie powiedzie się.
- Rozmiar obrazu musi być 127 GB lub mniej.
- Musi znajdować się na plik wirtualnego dysku twardego (VHDX pliki nie są obecnie obsługiwane).
- Wirtualny dysk twardy nie może być maszyny wirtualnej Generowanie 2.
- Wirtualny dysk twardy może być stałym rozmiarze lub dynamicznie powiększających. Dynamicznie powiększające wirtualny dysk twardy jest zalecana, ponieważ trwa mniej czasu na przekazanie do Azure niż stałym rozmiarze pliku wirtualnego dysku twardego.
- Dysk musi być zainicjowany przy użyciu rekordu uruchamiania główny (MBR) podziału styl. Identyfikator GUID partition tabeli (GPT) partition stylu nie jest obsługiwane.
- Wirtualny dysk twardy musi zawierać jednej instalacji systemu Windows Server 2012 R2. Może zawierać przypisaną, ale tylko jeden zawierających instalacji systemu Windows.
- Musi być zainstalowany roli hosta zdalnej sesji pulpitu (RDSH) i funkcję Środowisko pulpitu.
- Musisz roli Broker połączeń pulpitu zdalnego *nie* jest zainstalowany.
- Musi być wyłączony (szyfrowania plików systemowych).
- Obraz musi być przetworzonej przez program Sysprep przy użyciu parametrów **/oobe / generalize/shutdown** (nie użyty parametr **/mode:vm** ).
- Przekazywanie do wirtualnego dysku twardego z łańcucha migawki nie jest obsługiwane.

Aby uzyskać więcej informacji na temat tworzenia obrazów na potrzeby Azure RemoteApp, zobacz [Tworzenie obrazu Azure RemoteApp](remoteapp-imageoptions.md) .
