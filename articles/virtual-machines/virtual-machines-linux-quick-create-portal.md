<properties
    pageTitle="Tworzenie maszyny Linux przy użyciu Azure Portal | Microsoft Azure"
    description="Tworzenie za pomocą Azure Portal maszyny Linux."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="vlivech"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"
/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="10/25/2016"
    ms.author="v-livech"
/>

# <a name="create-a-linux-vm-on-azure-using-the-portal"></a>Tworzenie maszyny Linux Azure za pomocą portalu


W tym artykule pokazano, jak używać [Azure portal](https://portal.azure.com/) do tworzenia maszyny wirtualnej Linux.

Dostępne są następujące wymagania:

- [Konto Azure](https://azure.microsoft.com/pricing/free-trial/)

- [SSH publicznych i prywatnych plików kluczowych](virtual-machines-linux-mac-create-ssh-keys.md)


1. Logowanie do portalu Azure przy użyciu tożsamości usługi Azure konta, kliknij pozycję **+ Nowe** w lewym górnym rogu:

    ![screen1](../media/virtual-machines-linux-quick-create-portal/screen1.png)

2. Kliknij **maszyn wirtualnych** w **witrynie Marketplace** , a następnie **Ubuntu serwera 14.04 KÓW** z **Polecane aplikacje** obrazów listy.  Zweryfikuj u dołu model wdrożenia `Resource Manager` , a następnie kliknij przycisk **Utwórz**.

    ![screen2](../media/virtual-machines-linux-quick-create-portal/screen2.png)

3. Na stronie **— informacje podstawowe** wprowadź:
    - nazwę maszyn wirtualnych
    - Nazwa użytkownika dla użytkownika administratora
    - Ustaw typ uwierzytelniania **SSH klucz publiczny**
    - Kluczem publicznym SSH w postaci ciągu (z usługi `~/.ssh/` katalogu)
    - zasób Nazwa grupy lub wybierz istniejącej grupy

    i kliknij **przycisk OK** , aby kontynuować i wybrać rozmiar pamięci Wirtualnej; jego powinien wyglądać następująco:

    ![screen3](../media/virtual-machines-linux-quick-create-portal/screen3.png)

4. Wybierz rozmiar **DS1** instaluje Ubuntu na Premium SSD i kliknij przycisk **Wybierz** , aby skonfigurować ustawienia.

    ![screen4](../media/virtual-machines-linux-quick-create-portal/screen4.png)

5. W obszarze **Ustawienia**pozostaw ustawienia domyślne wartości miejsca do magazynowania i sieci, a następnie kliknij **przycisk OK** , aby wyświetlić podsumowanie.  Powiadomienie o typu dysku ustawiono Premium SSD, wybierając pozycję DS1, **S** notates SSD.

    ![screen5](../media/virtual-machines-linux-quick-create-portal/screen5.png)

6. Potwierdź ustawienia do nowych maszyn wirtualnych Ubuntu, a następnie kliknij **przycisk OK**.

    ![screen6](../media/virtual-machines-linux-quick-create-portal/screen6.png)

7. Otwórz na pulpicie nawigacyjnym portalu i w **oknie Interfejsy sieciowe** Wybierz swojego NIC

    ![screen7](../media/virtual-machines-linux-quick-create-portal/screen7.png)

8. Otwarcie menu adresów IP publicznej w obszarze Ustawienia NIC

    ![screen8](../media/virtual-machines-linux-quick-create-portal/screen8.png)

9. SSH do publiczny adres IP przy użyciu klucza publicznego SSH

```
ssh -i ~/.ssh/azure_id_rsa ubuntu@13.91.99.206
```

## <a name="next-steps"></a>Następne kroki

Teraz została utworzona maszyny Linux szybko służącej do testowania lub pokaz. Aby utworzyć maszyny Linux dostosowane do infrastruktury, można wykonać dowolną z następujących artykułów.

- [Tworzenie maszyny Linux Azure korzystania z szablonów](virtual-machines-linux-cli-deploy-templates.md)
- [Tworzenie SSH zabezpieczone Linux maszyn wirtualnych Azure korzystania z szablonów](virtual-machines-linux-create-ssh-secured-vm-from-template.md)
- [Tworzenie maszyny Linux za pomocą interfejsu wiersza polecenia Azure](virtual-machines-linux-create-cli-complete.md)
