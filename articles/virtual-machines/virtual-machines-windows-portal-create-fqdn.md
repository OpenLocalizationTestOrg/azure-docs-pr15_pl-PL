<properties
   pageTitle="Tworzenie nazwy FQDN dla maszyn wirtualnych w Azure portal | Microsoft Azure"
   description="Dowiedz się, jak utworzyć w pełni kwalifikowaną nazwę domeny lub nazwy FQDN dla Menedżera zasobów na podstawie maszyn wirtualnych w Azure portal."
   services="virtual-machines-windows"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor="tysonn"
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="08/24/2016"
   ms.author="iainfou"/>

# <a name="create-a-fully-qualified-domain-name-in-the-azure-portal"></a>Tworzenie w pełni kwalifikowaną nazwę domeny w Azure portal
Po utworzeniu maszyny wirtualnej (maszyn wirtualnych) w [portalu Azure](https://portal.azure.com) przy użyciu modelu wdrożenia Menedżera zasobów publicznej zasobu adresów IP dla maszyny wirtualnej jest tworzona automatycznie. Ten adres IP umożliwia zdalny dostęp do maszyn wirtualnych. Mimo że portalu nie utworzyć [w pełni kwalifikowaną nazwę domeny](https://en.wikipedia.org/wiki/Fully_qualified_domain_name), lub nazwy FQDN, domyślnie, można go utworzyć po utworzeniu maszyn wirtualnych. W tym artykule przedstawiono procedurę tworzenia nazwy DNS lub nazwy FQDN.

[AZURE.INCLUDE [virtual-machines-common-portal-create-fqdn](../../includes/virtual-machines-common-portal-create-fqdn.md)]

Można teraz nawiązanie połączenia zdalnego maszyn wirtualnych, korzystając z tej nazwy DNS takich jak dla protokołu RDP (Remote Desktop).

## <a name="next-steps"></a>Następne kroki
Teraz, gdy usługi maszyn wirtualnych ma nazwę publicznej IP i systemie DNS, należy wdrożyć typowych środowisk aplikacji i usług, takich jak IIS, SQL lub programu SharePoint.

Możesz również więcej informacji o [za pomocą Menedżera zasobów](../azure-resource-manager/resource-group-overview.md) , aby uzyskać porady dotyczące tworzenia Azure wdrożeń.