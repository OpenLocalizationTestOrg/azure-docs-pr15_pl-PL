<properties
   pageTitle="Aktualizowanie urządzenia StorSimple | Microsoft Azure"
   description="Wyjaśniono, jak za pomocą funkcji aktualizacji StorSimple Zainstaluj aktualizacje tryb zwykłego i konserwacja i poprawki."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="06/28/2016"
   ms.author="v-sharos" />

# <a name="update-your-storsimple-8000-series-device"></a>Aktualizowanie urządzenia serii 8000 StorSimple

## <a name="overview"></a>Omówienie

Funkcje aktualizacji StorSimple umożliwiają łatwe aktualne urządzenia StorSimple. W zależności od typu aktualizacji można zastosować aktualizacje do urządzenia za pośrednictwem portalu klasyczny Azure lub za pośrednictwem interfejsu programu Windows PowerShell. Ten przewodnik opisuje typy aktualizacji i zainstalować każdego z nich.

Możesz zastosować dwa typy aktualizacji urządzenia: 

- Zwykłe (ani w trybie normalnym) aktualizacji
- Aktualizacje tryb konserwacji

Możesz zainstalować regularne aktualizacje za pośrednictwem portalu klasyczny Azure lub środowiska Windows PowerShell; za pomocą programu Windows PowerShell należy jednak instalowanie aktualizacji tryb konserwacji. 

Typ każdej aktualizacji jest opisane oddzielnie, poniżej.

### <a name="regular-updates"></a>Regularne aktualizacje

Regularne aktualizacje są Brak aktualizacje, które mogą zostać zainstalowane, gdy urządzenie jest w trybie normalnym. Te aktualizacje są stosowane do witryny Microsoft Update w każdym kontrolerze urządzenia. 

> [AZURE.IMPORTANT] Kontroler trybie awaryjnym mogą wystąpić podczas procesu aktualizacji. Jednak nie dotyczy to dostępność systemu lub operacji.

- Aby uzyskać szczegółowe informacje na temat instalacji regularne aktualizacje za pośrednictwem portalu klasyczny Azure, zobacz [Zainstaluj aktualizacje zwykła za pośrednictwem Azure portal(#install-regular-updates-via-the-azure-classic-portal) klasyczny.

- Można również zainstalować regularne aktualizacje za pomocą programu Windows PowerShell dla StorSimple. Aby uzyskać szczegółowe informacje zobacz [Instalowanie regularne aktualizacje za pomocą programu Windows PowerShell dla StorSimple](#install-regular-updates-via-windows-powershell-for-storsimple).

### <a name="maintenance-mode-updates"></a>Aktualizacje tryb konserwacji

Aktualizacje tryb konserwacji to kłopotliwe środki aktualizacje, takie jak aktualizacje oprogramowania dysku. Aktualizacje te wymagają wprowadzane w trybie konserwacji na tym urządzeniu. Aby uzyskać szczegółowe informacje, zobacz [Krok 2: tryb konserwacji wprowadź](#step2). Klasyczny portalu Azure nie umożliwia instalowanie aktualizacji tryb konserwacji. Zamiast tego należy użyć programu Windows PowerShell dla StorSimple. 

Aby uzyskać szczegółowe informacje na temat instalacji aktualizacji tryb konserwacji zobacz [Instalowanie konserwacja tryb aktualizacji przy użyciu programu Windows PowerShell dla StorSimple](#install-maintenance-mode-updates-via-windows-powershell-for-storsimple).

> [AZURE.IMPORTANT] Aktualizacje tryb konserwacji musi być stosowane oddzielnie każdy kontroler. 

## <a name="install-regular-updates-via-the-azure-classic-portal"></a>Instalowanie aktualizacji zwykła za pośrednictwem portalu klasyczny Azure

Klasyczny portal Azure umożliwia stosowanie aktualizacji do urządzenia StorSimple.

[AZURE.INCLUDE [storsimple-install-updates-manually](../../includes/storsimple-install-updates-manually.md)]

## <a name="install-regular-updates-via-windows-powershell-for-storsimple"></a>Instalowanie regularne aktualizacje za pomocą programu Windows PowerShell dla StorSimple

Za pomocą programu Windows PowerShell dla StorSimple można również zastosować aktualizacje zwykłego (w trybie normalnym).

> [AZURE.IMPORTANT] Mimo że można zainstalować regularne aktualizacje przy użyciu programu Windows PowerShell dla StorSimple, firma Microsoft zaleca zainstalowanie aktualizacji zwykła za pośrednictwem portalu klasyczny Azure. Począwszy od aktualizacji 1, przed testy zostaną wykonane przed zainstalowaniem aktualizacji z portalu. Kontrole przed będzie zastępują błędów i upewnij się bardziej płynną pracę. 

[AZURE.INCLUDE [storsimple-install-regular-updates-powershell](../../includes/storsimple-install-regular-updates-powershell.md)]

## <a name="install-maintenance-mode-updates-via-windows-powershell-for-storsimple"></a>Instalowanie aktualizacji tryb konserwacji za pomocą programu Windows PowerShell dla StorSimple

Stosowanie aktualizacji tryb konserwacji na urządzeniu StorSimple za pomocą programu Windows PowerShell dla StorSimple. Wszystkie żądania We/Wy są wstrzymywane w tym trybie. Usług, takich jak pamięci RAM trwała (NVRAM) lub usługę klastrów także zostały zatrzymane. Oba kontrolery są ponownie uruchomić po wprowadzeniu lub opuścić ten tryb. Po zamknięciu tego trybu wszystkich usług będzie życiorys i powinna być prawidłowy. (To może potrwać kilka minut).

Jeśli chcesz zastosować aktualizacje tryb konserwacji, otrzymasz alert za pośrednictwem portalu klasyczny Azure czy masz aktualizacje, które muszą być zainstalowane. Ten alert będzie zawierać instrukcje dotyczące instalacji aktualizacji przy użyciu programu Windows PowerShell dla StorSimple. Po zaktualizowaniu urządzenia umożliwia zmianę urządzenia do zwykłego trybu tę samą procedurę. Aby uzyskać instrukcje krok po kroku, zobacz [Krok 4: tryb konserwacji wyjścia](#step4).

> [AZURE.IMPORTANT] 
> 
> - Przed wprowadzeniem tryb konserwacji, sprawdź, czy oba kontrolery urządzeń są prawidłowy, zaznaczając pole wyboru **Stan sprzętu** na stronie **Obsługa** w portalu klasyczny Azure. Jeśli administrator nie działa prawidłowo, skontaktuj się Support firmy Microsoft dla następnych kroków. Aby uzyskać więcej informacji przejdź do kontakt z pomocą techniczną firmy Microsoft. 
> - Podczas pracy w trybie konserwacji, należy najpierw zainstalować aktualizację na jednym kontrolerze, a następnie na innym kontrolerze.

### <a name="step-1-connect-to-the-serial-console-a-namestep1"></a>Krok 1: Nawiązywanie połączenia z konsoli szeregowego<a name="step1">

Aby uzyskać dostęp do konsoli szeregowego najpierw użyj aplikacji, takiej jak Kit. Poniższa procedura wyjaśniono, jak Kit nawiązywania połączenia za pomocą konsoli szeregowego.

[AZURE.INCLUDE [storsimple-use-putty](../../includes/storsimple-use-putty.md)]

### <a name="step-2-enter-maintenance-mode-a-namestep2"></a>Krok 2: Przejście do trybu konserwacji<a name="step2">

Po podłączeniu do konsoli Określ, czy są dostępne aktualizacje, aby zainstalować i przejście do trybu konserwacji mają zostać zainstalowane.

[AZURE.INCLUDE [storsimple-enter-maintenance-mode](../../includes/storsimple-enter-maintenance-mode.md)]

### <a name="step-3-install-your-updates-a-namestep3"></a>Krok 3: Instalowanie aktualizacji<a name="step3">

Następnie zainstaluj aktualizacje.

[AZURE.INCLUDE [storsimple-install-maintenance-mode-updates](../../includes/storsimple-install-maintenance-mode-updates.md)]
 
### <a name="step-4-exit-maintenance-mode-a-namestep4"></a>Krok 4: Tryb konserwacji wyjścia<a name="step4">

Na koniec Zamknij tryb konserwacji.

[AZURE.INCLUDE [storsimple-exit-maintenance-mode](../../includes/storsimple-exit-maintenance-mode.md)]

## <a name="install-hotfixes-via-windows-powershell-for-storsimple"></a>Instalowanie poprawki przy użyciu programu Windows PowerShell dla StorSimple

W przeciwieństwie do aktualizacji dla programu Microsoft Azure StorSimple poprawki nie są zainstalowane w folderze udostępnionym. Podobnie jak w przypadku aktualizacji, istnieją dwa rodzaje poprawki: 

- Zwykłe poprawek 
- Konserwacja poprawki  

Poniższe procedury wyjaśniono, jak używać programu Windows PowerShell dla StorSimple do zainstalowania zwykłego i konserwacja poprawki.

[AZURE.INCLUDE [storsimple-install-regular-hotfixes](../../includes/storsimple-install-regular-hotfixes.md)]

[AZURE.INCLUDE [storsimple-install-maintenance-mode-hotfixes](../../includes/storsimple-install-maintenance-mode-hotfixes.md)]

## <a name="what-happens-to-updates-if-you-perform-a-factory-reset-of-the-device"></a>Co się dzieje aktualizacji po wykonaniu Resetowanie factory urządzenie?

Jeśli urządzenie jest przywrócić ustawienia fabryczne, wszystkie aktualizacje zostaną utracone. Po urządzenia factory resetowanie jest zarejestrowana i skonfigurowana, może być konieczne ręczne instalowanie aktualizacji przez portal Azure klasyczny i/lub środowiska Windows PowerShell dla StorSimple. Aby uzyskać więcej informacji na temat factory resetowania, zobacz [Resetowanie domyślne ustawienia fabryczne urządzenia](storsimple-manage-device-controller.md#reset-the-device-to-factory-default-settings).

## <a name="next-steps"></a>Następne kroki

- Dowiedz się więcej o [korzystaniu z programu Windows PowerShell dla StorSimple administrowania urządzenia StorSimple](storsimple-windows-powershell-administration.md).
- Dowiedz się więcej o [korzystaniu z usługi Menedżera StorSimple administrowania urządzenia StorSimple](storsimple-manager-service-administration.md).
