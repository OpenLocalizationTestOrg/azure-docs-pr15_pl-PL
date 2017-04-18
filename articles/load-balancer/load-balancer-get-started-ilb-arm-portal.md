<properties
   pageTitle="Wprowadzenie do tworzenia równoważenia obciążenia wewnętrznych w Menedżerze zasobów za pomocą portalu Azure | Microsoft Azure"
   description="Dowiedz się, jak utworzyć równoważenia obciążenia wewnętrznych w Menedżerze zasobów za pomocą portalu Azure"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />

# <a name="create-an-internal-load-balancer-in-the-azure-portal"></a>Tworzenie równoważenia obciążenia wewnętrznych w portalu Azure

[AZURE.INCLUDE [load-balancer-get-started-ilb-arm-selectors-include.md](../../includes/load-balancer-get-started-ilb-arm-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][model klasyczny wdrożenia](load-balancer-get-started-ilb-classic-ps.md).

[AZURE.INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## <a name="get-started-creating-an-internal-load-balancer-using-azure-portal"></a>Wprowadzenie do tworzenia równoważenia obciążenia wewnętrznych, za pomocą portalu Azure

Wykonaj następujące czynności, aby utworzyć równoważenia obciążenia wewnętrznych z Azure Portal.

1. Otwórz przeglądarkę, przejdź do [portalu Azure](http://portal.azure.com)i zaloguj się przy użyciu konta Azure.
2. W lewej górnej części ekranu kliknij przycisk **Nowy** > **sieci** > **usługi równoważenia obciążenia**.
3. W karta **Tworzenie Usługa równoważenia obciążenia** wprowadź **nazwę** dla usługi równoważenia obciążenia.
4. W obszarze **schemat**kliknij **wewnątrz**.
5. Kliknij pozycję **wirtualnej sieci**, a następnie wybierz miejsce, w którym chcesz utworzyć równoważenia obciążenia sieci wirtualnej.

    >[AZURE.NOTE] Jeśli nie widzisz wirtualnej sieci, który ma być wyświetlany, sprawdź **lokalizacji** jest używana do równoważenia obciążenia i odpowiednio go zmienić.

6. Kliknij pozycję **podsieci**, a następnie wybierz miejsce, w którym chcesz utworzyć równoważenia obciążenia podsieci.
7. W obszarze **Przypisywanie adresów IP**kliknij **dynamiczne** lub **statyczne**, w zależności od tego, czy ma adres IP usługi równoważenia obciążenia ustalana (statyczny), czy nie.

    >[AZURE.NOTE] Wybranie korzystania ze statycznego adresu IP, musisz podać adres dla usługi równoważenia obciążenia.

8. W obszarze **grupy zasobów** Określ nazwę nowej grupy zasobów dla usługi równoważenia obciążenia, lub kliknij pozycję **Wybierz istniejące** i wybierz istniejącej grupy zasobów.
9. Kliknij przycisk **Utwórz**.

## <a name="configure-load-balancing-rules"></a>Konfigurowanie reguł równoważenia obciążenia

Po utworzeniu równoważenia obciążenia przejdź do zasobu usługi równoważenia obciążenia jest skonfigurowana.
Musisz najpierw skonfigurować puli adresów wewnętrznej i sondy przed rozpoczęciem konfigurowania reguły równoważenia obciążenia.

### <a name="step-1-configure-a-back-end-pool"></a>Krok 1: Konfigurowanie puli wewnętrznej

1. W portalu Azure, kliknij przycisk **Przeglądaj** > **urządzenia do równoważenia obciążenia**, a następnie kliknij pozycję równoważenia obciążenia utworzone powyżej.
2. Karta **Ustawienia** kliknij **pul wewnętrznej bazy danych**.
3. W karta **Pule adresów wewnętrznej bazy danych** kliknij przycisk **Dodaj**.
4. W karta **Dodaj puli wewnętrznej bazy danych** wprowadź **nazwę** dla puli wewnętrznej bazy danych, a następnie kliknij **przycisk OK**.

### <a name="step-2-configure-a-probe"></a>Krok 2: Konfigurowanie sondy

1. W portalu Azure, kliknij przycisk **Przeglądaj** > **urządzenia do równoważenia obciążenia**, a następnie kliknij pozycję równoważenia obciążenia utworzone powyżej.
2. Karta **Ustawienia** kliknij **sondy**.
3. W karta **sondy** kliknij przycisk **Dodaj**.
4. W karta **sondy Dodaj** wprowadź **nazwę** dla sondy.
5. W obszarze **Protocol (protokół)**zaznacz **HTTP** (w przypadku witryn sieci web) lub **port TCP** (w przypadku innych aplikacji TCP podstawie).
6. W obszarze **Port**Określ port używany podczas uzyskiwania dostępu do sondy.
7. W obszarze **ścieżki** (w przypadku tylko sondy HTTP) określ ścieżkę ma zostać użyte jako sondy.
8. W obszarze **Interwał** umożliwia określenie sposobu często sondy aplikacji.
9. W obszarze **Próg nieprawidłowe**Określ liczbę prób ulegnie awarii przed maszyny wirtualnej wewnętrznej bazy danych jest oznaczony jako nieprawidłowe.
10. Kliknij **przycisk OK** , aby utworzyć sondy.

### <a name="step-3-configure-load-balancing-rules"></a>Krok 3: Konfigurowanie reguł równoważenia obciążenia

1. W portalu Azure, kliknij przycisk **Przeglądaj** > **urządzenia do równoważenia obciążenia**, a następnie kliknij pozycję równoważenia obciążenia utworzone powyżej.
2. W karta **Ustawienia** kliknij przycisk **reguły równoważenia obciążenia**.
3. W karta **równoważenia obciążenia reguł** kliknij przycisk **Dodaj**.
4. W karta **Dodaj ładowanie równoważenia reguły** wpisz **nazwę** reguły.
5. W obszarze **Protocol (protokół)**zaznacz **HTTP** (w przypadku witryn sieci web) lub **port TCP** (w przypadku innych aplikacji TCP podstawie).
6. W obszarze **Port**Określ port klienci łączą się z w równoważenia obciążenia.
7. W obszarze **port wewnętrznej bazy danych**, określ port może być używany w puli wewnętrznej bazy danych (zwykle port równoważenia obciążenia i port wewnętrznej bazy danych są takie same).
8. W obszarze **puli wewnętrznej bazy danych**wybierz utworzoną wcześniej puli wewnętrznej bazy danych.
9. W obszarze **pozostawanie sesji**wybierz żądany sposób sesji do innej witryny.
10. W obszarze **limit czasu bezczynności (w minutach)**Określ limit czasu bezczynności.
11. W obszarze **Przestawny IP (zwrotu bezpośredni server)**kliknij **wyłączone** lub **włączone**.
12. Kliknij **przycisk OK**.

## <a name="next-steps"></a>Następne kroki

[Konfigurowanie trybu rozkładu równoważenia obciążenia](load-balancer-distribution-mode.md)

[Konfigurowanie ustawienia przekroczenia limitu czasu bezczynności protokołu TCP dla usługi równoważenia obciążenia](load-balancer-tcp-idle-timeout.md)