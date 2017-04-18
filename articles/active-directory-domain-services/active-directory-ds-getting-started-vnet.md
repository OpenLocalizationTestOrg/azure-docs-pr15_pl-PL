<properties
    pageTitle="Azure usługach domenowych AD: Tworzenie lub wybieranie wirtualnej sieci | Microsoft Azure"
    description="Wprowadzenie do usług domenowych Active Directory platformy Azure"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/03/2016"
    ms.author="maheshu"/>

# <a name="create-or-select-a-virtual-network-for-azure-ad-domain-services"></a>Tworzenie lub wybieranie wirtualną sieć usługi Azure AD domeny

## <a name="guidelines-to-select-an-azure-virtual-network"></a>Wskazówki, aby zaznaczyć Azure wirtualnej sieci
> [AZURE.NOTE] **Przed rozpoczęciem**: odwołują się do [sieci zagadnienia związane z usług domenowych AD Azure](active-directory-ds-networking.md).


## <a name="task-2-create-an-azure-virtual-network"></a>Zadanie 2: Tworzenie Azure wirtualnej sieci
Następnego zadania konfiguracji jest utworzenie Azure wirtualnej sieci i podsieci znajdujące się w nim. Wirtualna sieć należy włączyć usługi Azure AD domeny w danej podsieci. Jeśli masz już istniejące wirtualnej sieci, którego chcesz użyć, możesz pominąć ten krok.

> [AZURE.NOTE] Upewnij się, że Azure wirtualną sieć Tworzenie lub wybieranie korzystać z usług domenowych AD Azure należy do Azure region, który jest obsługiwany w usługach domenowych AD Azure. Odwiedź stronę [usług Azure według regionów](https://azure.microsoft.com/regions/#services/) wiedzieć Azure regiony, w których Azure AD Domain Services jest dostępny.

Zanotuj nazwę wirtualnej sieci, możesz wybrać prawo wirtualną sieć podczas włączania usług domenowych AD Azure w kroku kolejnych konfiguracji.

Wykonywanie poniższe kroki konfiguracji, aby utworzyć Azure wirtualnej sieci, w której chcesz włączyć usługi Azure AD domeny.

1. Przejdź do **portalu klasyczny Azure** ([https://manage.windowsazure.com](https://manage.windowsazure.com)).

2. Wybierz węzeł **sieci** w okienku po lewej stronie.

    ![Węzeł sieci](./media/active-directory-domain-services-getting-started/networks-node.png)

3. Kliknij przycisk **Nowy** w okienku zadań u dołu strony.

    ![Węzeł wirtualnych sieci](./media/active-directory-domain-services-getting-started/virtual-networks.png)

4. W węźle **Usług sieci** zaznacz **Wirtualnej sieci**.

5. Kliknij przycisk **Szybkie tworzenie** tworzenie wirtualnej sieci.

    ![Wirtualna sieć — szybkie tworzenie](./media/active-directory-domain-services-getting-started/virtual-network-quickcreate.png)

6. Określ **nazwę** dla wirtualnych sieci. Można również skonfigurować **przestrzeni adresów** lub **maszyn wirtualnych maksymalna liczba** dla tej sieci. Teraz można pozostawić ustawienia **serwera DNS** "Brak". Możesz zaktualizować ustawienia po swojej Włącz Azure AD domeny usługi serwera DNS.

7. Upewnij się, zaznacz obszar Azure obsługiwane na liście rozwijanej **lokalizacji** . Odwiedź stronę [usług Azure według regionów](https://azure.microsoft.com/regions/#services/) wiedzieć Azure regiony, w których Azure AD Domain Services jest dostępny.

8. Aby utworzyć wirtualnej sieci, kliknij przycisk **Utwórz wirtualnej sieci** .

    ![Tworzenie wirtualnych sieci usługi Azure AD domeny.](./media/active-directory-domain-services-getting-started/create-vnet.png)

9. Po utworzeniu wirtualną sieć zaznacz wirtualną sieć, a następnie kliknij kartę **Konfigurowanie** .

    ![Tworzenie podsieci](./media/active-directory-domain-services-getting-started/create-vnet-properties.png)

10. Przejdź do sekcji **wirtualną sieć adres spacje** . Kliknij przycisk **Dodaj podsieci** , a następnie określ podsieci o nazwie **AaddsSubnet**. Kliknij przycisk **Zapisz** , aby utworzyć podsieci.

    ![Tworzenie podsieci dla usług domenowych AD Azure.](./media/active-directory-domain-services-getting-started/create-vnet-add-subnet.png)


<br>

## <a name="task-3---enable-azure-ad-domain-services"></a>Zadania 3 — Włączanie usług Azure AD domeny
Następnego zadania konfiguracji należy [włączyć usługi Azure AD domeny](active-directory-ds-getting-started-enableaadds.md).
