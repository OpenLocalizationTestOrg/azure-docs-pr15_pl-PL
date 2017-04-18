<properties
   pageTitle="Konfigurowanie programu PowerShell do tworzenia maszyn wirtualnych na rynku | Microsoft Azure"
   description="Instrukcje dotyczące konfigurowania programu PowerShell Azure i używać go jako opcjonalne proces przepływu do tworzenia obrazów maszyn wirtualnych wdrożenia i sprzedaży w Azure Marketplace"
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

<tags
   ms.service="marketplace"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="02/04/2016"
   ms.author="hascipio"/>

# <a name="set-up-azure-powershell-to-create-an-offer-for-the-azure-marketplace"></a>Konfigurowanie Azure programu PowerShell, aby utworzyć ofertę dla usługi Azure Marketplace
Aby uzyskać szczegółowe informacje na temat konfigurowania programu PowerShell platformy Azure zobacz [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md). Proste podejście jest użycie metody certyfikatu, która pobiera i importowanie certyfikatu, potrzebne do uwierzytelniania. Aby uzyskać certyfikat potrzebne, użyj polecenia cmdlet **Get-AzurePublishSettingsFile** . Gdy zostanie wyświetlony monit, Zapisz plik. Aby zaimportować certyfikat do sesji programu PowerShell, należy użyć polecenia cmdlet **AzurePublishSettingsFile importu** .

Aby skonfigurować i przechowywanie typowych ustawień subskrypcji Microsoft Azure sesji programu PowerShell, użyj polecenia cmdlet **Set-AzureSubscription** i **Wybierz pozycję AzureSubscription** :

        Set-AzureSubscription -SubscriptionName “mySubName” -CurrentStorageAccountName “mystorageaccount”
        Select-AzureSubscription -SubscriptionName "mySubName" –Current

Pierwsze polecenie kojarzy domyślne konto miejsca do magazynowania z subskrypcji (wymagane przez niektóre maszyn wirtualnych operacje obsługi administracyjnej).  Druga sprawia, że subskrypcja bieżąca (rozpoznany przez inne polecenia cmdlet).

## <a name="see-also"></a>Zobacz też
- [Wprowadzenie: jak publikować ofertę Azure Marketplace](marketplace-publishing-getting-started.md)
- [Tworzenie obrazu maszyn wirtualnych dla Marketplace](marketplace-publishing-vm-image-creation.md)
