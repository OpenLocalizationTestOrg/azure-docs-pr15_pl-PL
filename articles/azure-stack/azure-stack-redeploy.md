<properties
    pageTitle="Ponownie wdróż Azure stos | Microsoft Azure"
    description="Ponownie wdróż Azure stosu."
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/29/2016"
    ms.author="erikje"/>

# <a name="redeploy-azure-stack"></a>Ponownie wdróż stos Azure

Aby ponownie wdróż stos Azure, należy uruchomić nad od podstaw zgodnie z poniższym opisem.

## <a name="steps-to-redeploy-azure-stack"></a>Czynności, aby ponownie rozmieścić stos Azure

1. W oryginalnym system operacyjny (zainstalowany od zera), należy ponownie uruchomić hosta. Jest to nie to domyślne ustawienie w menu uruchamiania, trzeba używać KVM lub konsoli lokalnej, aby go zaznaczyć podczas ponownym uruchomieniu komputera (podczas instalacji, możesz o nazwie "Uruchamiania z wirtualnego dysku twardego" OS do "AzureStack TP2", pomoże zidentyfikować, czyli systemu operacyjnego, który).

    Nie trzeba usunąć istniejącego wpisu (nowe pomocy technicznej skrypt, który "PrepareBootFromVHD.ps1" Trwa istotnych tej usługi.)

2. Jeśli nie masz KVM lub chcesz wybierz system operacyjny uruchamiania uruchomienie:
    
    1. Znajdź.\BootMenuNoKVM.ps1 skrypt. Ten plik jest dostępna z innych skrypty obsługi znajdujące się wraz z tej kompilacji.
    
    2. Uruchom skrypt z podwyższonym poziomem uprawnień. Wybierz nazwę oryginalnego systemu operacyjnego hosta. To uruchomi się hosta do pierwotnego hosta OS bez konieczności uzyskiwania dostępu KVM.
    
    3. Po zakończeniu skrypt zostanie wyświetlony monit o potwierdzenie Uruchom ponownie komputer.

    - W przypadku zalogowania innych użytkowników, to polecenie nie powiedzie się.

    - Po prostu uruchom następujące polecenie: ponowne uruchomienie komputera — wymuszanie 
 
3. Usuń plik CloudBuilder.vhdx, który był używany jako część poprzedniego wdrożenia.

    Nie musisz usunąć istniejące puli miejsca do magazynowania z poprzedniego wdrożenia TP2. Skrypt wdrożenia wykrywa wyczyści istniejącą, a następnie tworzy nowy.

5. Ponownie wdróż kopiowanie nową kopię CloudBuilder.vhdx, uruchom go, itd.

## <a name="next-steps"></a>Następne kroki

[Nawiązywanie połączenia z stos Azure](azure-stack-connect-azure-stack.md)
