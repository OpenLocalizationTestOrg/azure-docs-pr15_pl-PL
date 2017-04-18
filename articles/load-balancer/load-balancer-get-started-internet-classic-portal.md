
<properties
   pageTitle="Wprowadzenie do tworzenia internetowej przeciwległych równoważenia obciążenia w modelu Klasyczny wdrażania przy użyciu portalu klasyczny Azure | Microsoft Azure"
   description="Dowiedz się, jak utworzyć internetową przeciwległych równoważenia obciążenia w modelu Klasyczny wdrażania przy użyciu portalu klasyczny Azure"
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
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/31/2016"
   ms.author="sewhee" />

# <a name="get-started-creating-an-internet-facing-load-balancer-classic-in-the-azure-classic-portal"></a>Wprowadzenie do tworzenia internetowej przeciwległych równoważenia obciążenia (klasyczny) w portalu klasyczny Azure

[AZURE.INCLUDE [load-balancer-get-started-internet-classic-selectors-include.md](../../includes/load-balancer-get-started-internet-classic-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]W tym artykule opisano, jak model klasyczny wdrożenia. Można także [dowiedzieć się, jak utworzyć internetową przeciwległych równoważenia obciążenia za pomocą Menedżera zasobów Azure](load-balancer-get-started-internet-arm-ps.md).

[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]


## <a name="set-up-an-internet-facing-load-balancer-for-virtual-machines"></a>Konfigurowanie usługi równoważenia obciążenia z Internetu dla maszyn wirtualnych

Aby można było równoważenia obciążenia sieci ruchu z Internetu w środowisku maszyn wirtualnych systemu usługi w chmurze, możesz utworzyć zestaw równoważenia obciążenia. W poniższej procedurze założono, że zostały już utworzone maszyn wirtualnych i że są w tej samej usługi w chmurze.

**Aby skonfigurować zestaw równoważenia obciążenia dla maszyn wirtualnych**

1. W portalu klasyczny Azure kliknij **maszyn wirtualnych**, a następnie kliknij nazwę maszyny wirtualnej w zestawie równoważenia obciążenia.

2. Kliknij pozycję **punkty końcowe**, a następnie kliknij przycisk **Dodaj**.

3. Na stronie **Dodawanie punktu końcowego maszyn wirtualnych** kliknij strzałkę w prawo.

4. Na stronie **Określ szczegóły punkt końcowy** :

    * W polu **Nazwa**wpisz nazwę punktu końcowego lub wybierz nazwę z listy wstępnie zdefiniowanych punktów końcowych popularne protokoły.
    * W polu **Protokół**wybierz protokół wymagany przez typ punktu końcowego TCP i UDP, stosownie do potrzeb.
    * **Port publicznych i prywatnych**wpisz numery portów, odpowiednią maszyny wirtualnej, aby użyć, stosownie do potrzeb. Umożliwia prywatne portów i reguły zapory na komputerze wirtualnych przekierowanie ruchu w sposób, który jest odpowiedni dla aplikacji. Prywatne port może być taka sama jak port publicznej. Na przykład punktu końcowego dla ruchu w sieci web (HTTP) możesz przypisywać portu 80 do portu publiczne i prywatne.

5. Wybierz pozycję **Utwórz zestaw równoważenia obciążenia**, a następnie kliknij strzałkę w prawo.

6. Na stronie **Konfigurowanie zestawu równoważenia obciążenia** wpisz nazwę zestawu równoważenia obciążenia, a następnie przypisać wartości sondy zachowanie usługi równoważenia obciążenia Azure. Usługi równoważenia obciążenia używa sondy, aby sprawdzić, czy maszyn wirtualnych w zestawie równoważenia obciążenia są odbierać ruch przychodzący.

7. Kliknij znacznik wyboru, aby utworzyć punkt końcowy równoważenia obciążenia. W kolumnie **Nazwa zestawu równoważenia obciążenia** stronie **punkty końcowe** maszyny wirtualnej zobaczysz wartość **Tak** .

8. W portalu, w **środowisku maszyn wirtualnych systemu**kliknij nazwę dodatkowe maszyny wirtualnej w zestawie równoważenia obciążenia, kliknij pozycję **punkty końcowe**, a następnie kliknij przycisk **Dodaj**.

9. Na stronie **Dodawanie punktu końcowego maszyn wirtualnych** kliknij pozycję **Dodaj punkt końcowy do istniejącego zestawu równoważenia obciążenia**, wybierz nazwę zestawu równoważenia obciążenia, a następnie kliknij strzałkę w prawo.

10. Na stronie **Określ szczegóły punktu końcowego** wpisz nazwę punktu końcowego, a następnie kliknij znacznik wyboru.

Dla dodatkowych maszyn wirtualnych w zestawie równoważenia obciążenia Powtórz kroki 8-10.



## <a name="next-steps"></a>Następne kroki

[Przed rozpoczęciem konfigurowania równoważenia obciążenia wewnętrznych](load-balancer-get-started-ilb-arm-ps.md)

[Konfigurowanie trybu rozkładu równoważenia obciążenia](load-balancer-distribution-mode.md)

[Konfigurowanie ustawienia przekroczenia limitu czasu bezczynności protokołu TCP dla usługi równoważenia obciążenia](load-balancer-tcp-idle-timeout.md)

