<properties
   pageTitle="Tworzenie równoważenia obciążenia internetową dostępną w Menedżerze zasobów za pomocą portalu Azure | Microsoft Azure"
   description="Dowiedz się, jak utworzyć równoważenia obciążenia internetową dostępną w Menedżerze zasobów za pomocą portalu Azure"
   services="load-balancer"
   documentationCenter="na"
   authors="anavinahar"
   manager="narayan"
   editor=""
   tags="azure-resource-manager"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/14/2016"
   ms.author="annahar" />

# <a name="creating-an-internet-facing-load-balancer-using-the-azure-portal"></a>Tworzenie za pomocą portalu Azure równoważenia obciążenia z Internetu

[AZURE.INCLUDE [load-balancer-get-started-internet-arm-selectors-include.md](../../includes/load-balancer-get-started-internet-arm-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]W tym artykule opisano, jak model wdrożenia Menedżera zasobów. Możesz także [dowiedzieć się, jak utworzyć za pomocą klasycznej wdrożenia usługi równoważenia obciążenia z Internetu](load-balancer-get-started-internet-classic-portal.md)

[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

Obejmuje sekwencji poszczególnych zadań, które ma zostać wykonane do tworzenia równoważenia obciążenia i wyjaśniono szczegóły, co jest są gotowe do realizacji celem.

## <a name="what-is-required-to-create-an-internet-facing-load-balancer"></a>Co to jest wymagane do utworzenia równoważenia obciążenia z Internetu?

Należy utworzyć i skonfigurować następujące obiekty do wdrożenia usługi równoważenia obciążenia.

- Zewnętrzną Konfiguracja IP — zawiera publicznych adresów IP dla ruchu sieci przychodzącego.

- Pula adresów wewnętrznej — zawiera interfejsów sieciowych (NIC) maszyn wirtualnych otrzymywać ruch sieciowy od usługi równoważenia obciążenia.

- Zasady równoważenia obciążenia - zawiera reguły mapowania portem publicznej usługi równoważenia obciążenia do portu w puli adresów wewnętrznej.

- Translatora adresów Sieciowych reguły przychodzące — zawiera reguły mapowanie portem publicznej usługi równoważenia obciążenia na port dla określonej maszyny wirtualnej w puli adresów wewnętrznej.

- Sondy — zawiera sondy kondycji umożliwia sprawdzanie dostępności wystąpień maszyn wirtualnych w puli adresów wewnętrznej.

Więcej informacji na temat obciążenia można przejść równoważenia składniki przy użyciu Menedżera zasobów Azure u [Azure Menedżera zasobów pomocy technicznej dotyczącej usługi równoważenia obciążenia](load-balancer-arm.md).


## <a name="set-up-a-load-balancer-in-azure-portal"></a>Konfigurowanie usługi równoważenia obciążenia, w portalu Azure

> [AZURE.IMPORTANT] W tym przykładzie założono, że masz wirtualną sieć o nazwie **myVNet**. Zobacz [Tworzenie wirtualnych sieci](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) , w tym celu. Zakłada się również jest podsieci **myVNet** o nazwie **Można podsieci kg** i dwóch maszyny wirtualne o nazwie **sieci Web 1** i **sieci Web 2** odpowiednio w ten sam zestaw dostępności o nazwie **myAvailSet** w **myVNet**. Zapoznaj się z [tego łącza](../virtual-machines/virtual-machines-windows-hero-tutorial.md) w celu utworzenia maszyny wirtualne.


1. W przeglądarce przejdź do portalu Azure: [http://portal.azure.com](http://portal.azure.com) a następnie zaloguj się za pomocą konta usługi Azure.

2. W lewej górnej części ekranu wybierz pozycję **Nowy** > **sieci** > **usługi równoważenia obciążenia.**

3. W karta **Tworzenie Usługa równoważenia obciążenia** wpisz nazwę usługi równoważenia obciążenia. W tym miejscu jest nazywany **myLoadBalancer**.

4. W obszarze **Typ**wybierz pozycję **publiczna**.

5. W obszarze **publiczny adres IP**Utwórz nowy publiczny adres IP o nazwie **myPublicIP**.

6. W obszarze Grupa zasobów wybierz **myRG**. Następnie wybierz odpowiednią **lokalizację**, a następnie kliknij **przycisk OK**. Usługi równoważenia obciążenia następnie rozpocznie się wdrażanie i może potrwać kilka minut Aby pomyślnie ukończyć wdrożenia.

![Aktualizowanie grupy zasobów równoważenia obciążenia](./media/load-balancer-get-started-internet-portal/1-load-balancer.png)


## <a name="create-a-back-end-address-pool"></a>Tworzenie puli adres wewnętrznej

1. Po pomyślnym rozmieszczeniu usługi równoważenia obciążenia wybierz go z poziomu zasobów. W obszarze Ustawienia wybierz pozycję Pule wewnętrznej bazy danych. Wpisz nazwę dla swojego puli wewnętrznej bazy danych. Następnie kliknij przycisk **Dodaj** w górnej części kartę, która jest wyświetlany.

2. Kliknij na **Dodawanie maszyny wirtualnej** karta **Dodaj puli wewnętrznej bazy danych** .  Wybierz, **Wybierz zestaw dostępność** w obszarze **Ustawianie dostępności** i wybierz **myAvailSet**. Następnie wybierz pozycję **Wybierz maszyn wirtualnych** w sekcji maszyn wirtualnych w karta i kliknij **sieci Web 1** i **sieci Web 2**, dwóch maszyny wirtualne utworzone dla równoważenia obciążenia. Upewnij się, że oba muszą niebieski znaczniki wyboru z lewej strony jak pokazano na poniższej ilustracji. Następnie kliknij przycisk **Wybierz** , w tym karta, a następnie przycisk OK w karta **maszyn wirtualnych wybierz pozycję** , a następnie **przycisk OK** w karta **Dodawanie puli wewnętrznej bazy danych** .

    ![Dodawanie do puli adresów wewnętrznej bazy danych — ](./media/load-balancer-get-started-internet-portal/3-load-balancer-backend-02.png)

3. Sprawdź, czy na liście rozwijanej powiadomienia aktualizacją dotyczące zapisywania puli wewnętrznej bazy danych usługi równoważenia obciążenia oprócz aktualizacji sieciowej maszyny wirtualne **sieci Web 1** i **sieci Web 2**.


## <a name="create-a-probe-lb-rule-and-nat-rules"></a>Tworzenie sondy, reguły kg i reguły translatora adresów Sieciowych

1. Tworzenie sondy kondycji.

    W obszarze Ustawienia usługi równoważenia obciążenia wybierz sondy. Następnie kliknij przycisk **Dodaj** znajduje się w górnej części karta.

    Istnieją dwa sposoby konfigurowania sondy: HTTP lub TCP. W tym przykładzie pokazano, że HTTP, ale TCP można skonfigurować w podobny sposób.
    Aktualizowanie niezbędne informacje. Jak wspomniano, **myLoadBalancer** będzie równoważenia obciążenia ruchu na Port 80. Wybrana ścieżka jest HealthProbe.aspx, interwał wynosi 15 sekundach i nieprawidłowe próg jest 2. Po zakończeniu kliknij **przycisk OK** , aby utworzyć sondy.

    Umieść wskaźnik myszy nad "i" ikonę, aby dowiedzieć się więcej o tych poszczególnych konfiguracji i jak może zostać zmieniony na zaspokojenia do własnych potrzeb.

    ![Dodawanie sondy](./media/load-balancer-get-started-internet-portal/4-load-balancer-probes.png)

2. Tworzenie reguły równoważenia obciążenia.

    Kliknij polecenie reguły w sekcji Ustawienia usługi równoważenia obciążenia równoważenia obciążenia. W nowej karta kliknij **Dodaj**. Nazwa reguły. W tym miejscu jest HTTP. Wybierz port serwera sieci Web i wewnętrznej bazy danych. W tym miejscu 80 wybrano dla obu. Wybierz pozycję **wewnętrznej bazy danych kg** jako puli użytkownika wewnętrznej bazy danych i poprzednio utworzone **HealthProbe** jako sondy. Inne konfiguracje można ustawić zgodnie z własnymi potrzebami. Kliknij przycisk OK, aby zapisać reguły równoważenia obciążenia.

    ![Dodawanie reguły równoważenia obciążenia](./media/load-balancer-get-started-internet-portal/5-load-balancing-rules.png)

3. Tworzenie reguły przychodzące translatora adresów Sieciowych

    Kliknij polecenie Reguły przychodzące translatora adresów Sieciowych w sekcji Ustawienia usługi równoważenia obciążenia. W nowej karta, kliknij przycisk **Dodaj**. Nadaj nazwę reguły przychodzące translatora adresów Sieciowych. W tym miejscu jest nazywany **inboundNATrule1**. Miejsce docelowe powinny być publiczny adres IP utworzone wcześniej. Wybierz niestandardowe w obszarze usługi i wybierz protokół, którego chcesz użyć. W tym miejscu TCP jest zaznaczone. Wprowadź portu 3441 oraz portu docelowego, w tym przypadku 3389. Kliknij przycisk OK, aby zapisać tę regułę.

    Po utworzeniu pierwszą regułę, powtórz ten krok dla drugiej reguły translatora adresów Sieciowych ruchu przychodzącego o nazwie inboundNATrule2 z portu 3442 do portu docelowego 3389.

    ![Dodawanie reguły ruchu przychodzącego translatora adresów Sieciowych](./media/load-balancer-get-started-internet-portal/6-load-balancer-inbound-nat-rules.png)

## <a name="remove-a-load-balancer"></a>Usuwanie usługi równoważenia obciążenia

Aby usunąć równoważenia obciążenia, wybierz pozycję równoważenia obciążenia, który chcesz usunąć. W karta *Równoważenia obciążenia* kliknij **Usuń** znajduje się w górnej części karta. Następnie wybierz pozycję **Tak,** po wyświetleniu monitu.

## <a name="next-steps"></a>Następne kroki

[Przed rozpoczęciem konfigurowania równoważenia obciążenia wewnętrznych](load-balancer-get-started-ilb-arm-cli.md)

[Konfigurowanie trybu rozkładu równoważenia obciążenia](load-balancer-distribution-mode.md)

[Konfigurowanie ustawienia przekroczenia limitu czasu bezczynności protokołu TCP dla usługi równoważenia obciążenia](load-balancer-tcp-idle-timeout.md)
