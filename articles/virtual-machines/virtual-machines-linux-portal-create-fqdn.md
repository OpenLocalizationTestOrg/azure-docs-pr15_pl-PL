<properties
   pageTitle="Tworzenie nazwy FQDN dla maszyn wirtualnych w Azure portal | Microsoft Azure"
   description="Dowiedz się, jak utworzyć w pełni kwalifikowaną nazwę domeny lub nazwy FQDN dla Menedżera zasobów na podstawie maszyn wirtualnych w Azure portal."
   services="virtual-machines-linux"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor="tysonn"
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="08/23/2016"
   ms.author="iainfou"/>

# <a name="create-a-fully-qualified-domain-name-in-the-azure-portal"></a>Tworzenie w pełni kwalifikowaną nazwę domeny w portalu Azure
Po utworzeniu maszyny wirtualnej (maszyn wirtualnych) w [portalu Azure](https://portal.azure.com) przy użyciu modelu wdrożenia Menedżera zasobów publicznej zasobu adresów IP dla maszyny wirtualnej jest tworzona automatycznie. Ten adres IP umożliwia zdalny dostęp do maszyn wirtualnych. Mimo że portalu nie utworzyć [w pełni kwalifikowaną nazwę domeny](https://en.wikipedia.org/wiki/Fully_qualified_domain_name), lub nazwy FQDN, domyślnie, możesz dodać jeden po utworzeniu maszyn wirtualnych. W tym artykule przedstawiono procedurę tworzenia nazwy DNS lub nazwy FQDN.

[AZURE.INCLUDE [virtual-machines-common-portal-create-fqdn](../../includes/virtual-machines-common-portal-create-fqdn.md)]

Można teraz nawiązanie połączenia zdalnego maszyn wirtualnych, korzystając z tej nazwy DNS, takich jak z `ssh adminuser@testdnslabel.centralus.cloudapp.azure.com`.

## <a name="next-steps"></a>Następne kroki
Teraz, gdy usługi maszyn wirtualnych ma nazwę publicznej IP i systemie DNS, można wdrażać typowych środowisk aplikacji i usług, takich jak nginx, MongoDB, Docker, itp.

Możesz również więcej informacji o [za pomocą Menedżera zasobów](../azure-resource-manager/resource-group-overview.md) , aby uzyskać porady dotyczące tworzenia Azure wdrożeń.