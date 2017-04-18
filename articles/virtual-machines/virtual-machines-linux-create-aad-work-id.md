<properties
   pageTitle="Tworzenie tożsamości służbowe w AAD | Microsoft Azure"
   description="Dowiedz się, jak tworzyć tożsamości służbowe w usługi Azure Active Directory do użytku z maszyn wirtualnych Linux."
   services="virtual-machines-linux"
   documentationCenter=""
   authors="squillace"
   manager="timlt"
   editor=""
   tags="azure-service-management,azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="08/23/2016"
   ms.author="rasquill"/>

# <a name="creating-a-work-or-school-identity-in-azure-active-directory-to-use-with-linux-vms"></a>Tworzenie tożsamości służbowe w usługi Azure Active Directory do użytku z maszyny wirtualne Linux

Jeśli została utworzona osobiste konto Azure lub osobistych subskrypcji MSDN i utworzyć konto Azure, aby można było korzystać środków MSDN Azure — użyto tożsamości *konta Microsoft* go utworzyć. Wieloma wspaniałymi z Azure — [Szablony grupy zasobów](../azure-resource-manager/resource-group-overview.md) jest przykładem — wymaga konta służbowego (tożsamości zarządzane przez usługi Azure Active Directory) do pracy. Aby wykonać poniższe instrukcje, aby utworzyć nowego lub konta służbowego, ponieważ na szczęście jest jedną z najlepszych rzeczy, o Azure konta osobistego, pochodzi z domeną domyślną usługi Azure Active Directory, który służy do tworzenia nowych zadań lub konta służbowego, którego można używać z Azure funkcje, które wymagają.

Jednak ostatnie zmiany umożliwiają zarządzanie subskrypcji z dowolnego typu konto Azure za pomocą `azure login` metody logowania interakcyjnego opisanych [tutaj](../xplat-cli-connect.md). Możesz użyć tego mechanizmu, lub możesz wykonać instrukcje, które należy wykonać. Można również [utworzyć służbowe lub szkolne tożsamości w usłudze Azure Active Directory do użytku z maszyny wirtualne systemu Windows](virtual-machines-windows-create-aad-work-id.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

[AZURE.INCLUDE [virtual-machines-common-create-aad-work-id](../../includes/virtual-machines-common-create-aad-work-id.md)]
