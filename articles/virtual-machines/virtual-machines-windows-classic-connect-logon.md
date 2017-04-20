<properties
    pageTitle="Zaloguj się do maszyny Wirtualnej Azure klasyczne | Microsoft Azure"
    description="Użyj wersji zapoznawczej zalogować się do maszyny wirtualnej systemu Windows utworzone za pomocą modelu Klasyczny wdrożenia."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="cynthn"/>


# <a name="log-on-to-a-windows-virtual-machine-using-the-azure-classic-portal"></a>Zaloguj się do maszyny wirtualnej systemu Windows przy użyciu platformy Microsoft Azure

W wersji zapoznawczej możesz użyć przycisku **Connect** , aby rozpocząć sesję pulpitu zdalnego i zaloguj się do maszyny Wirtualnej systemu Windows.

Czy chcesz połączyć się Linux VM? Zobacz [jak zalogować się do maszyny wirtualnej z systemem Linux](virtual-machines-linux-mac-create-ssh-keys.md).

Dowiedz się, jak [wykonać te czynności za pomocą nowego portalu Azure](virtual-machines-windows-connect-logon.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)] 

## <a name="video-walkthrough"></a>Instruktaż wideo

Oto wideo przewodnik kroki opisane w tej instrukcji. Obejmuje ona również punktów końcowych i publiczne i prywatne porty używane do łączenia się z maszyny Wirtualnej Windows Azure.

[AZURE.VIDEO logging-on-to-vm-running-windows-server-on-azure]


## <a name="connect-to-the-virtual-machine"></a>Połączyć z maszyną wirtualną

1. Zaloguj się do wersji zapoznawczej.

2. Kliknij pozycję **maszyny wirtualne**, a następnie wybierz maszynę wirtualną.

3. Na pasku poleceń u dołu strony kliknij przycisk **Połącz**.

    ![Zaloguj się do maszyny wirtualnej](./media/virtual-machines-windows-classic-connect-logon/connectwindows.png)
    
> [AZURE.TIP] Jeśli przycisk **łączenia** nie jest dostępna, zobacz wskazówki dotyczące rozwiązywania problemów na końcu tego artykułu.

## <a name="log-on-to-the-virtual-machine"></a>Zaloguj się do maszyny wirtualnej

[AZURE.INCLUDE [virtual-machines-log-on-win-server](../../includes/virtual-machines-log-on-win-server.md)]

## <a name="next-steps"></a>Następne kroki

-   Jeśli przycisk **Connect** jest nieaktywny lub występują inne problemy z podłączania pulpitu zdalnego, spróbuj zresetować konfigurację. W panelu maszyny wirtualnej w obszarze **Szybkiego skrót**, kliknij przycisk **Resetuj konfiguracji zdalnej**.
-   Występują problemy przy użyciu hasła spróbuj zresetować go. W panelu maszyny wirtualnej w obszarze **Szybkiego skrót**, kliknij przycisk **Resetuj hasło**.

Jeśli te wskazówki nie działają lub nie to, czego potrzebujesz, zobacz [Rozwiązywanie problemów z pulpitem zdalnym połączenia do systemu Windows Azure maszyny wirtualnej](virtual-machines-windows-troubleshoot-rdp-connection.md). W tym artykule przedstawiono procedurę diagnozowania i rozwiązywania typowych problemów.


