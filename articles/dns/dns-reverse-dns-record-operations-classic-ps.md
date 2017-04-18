<properties
   pageTitle="Zarządzanie odwrotnej rekordy DNS dla usługi Azure (klasyczny) przy użyciu programu PowerShell | Microsoft Azure"
   description="Jak zarządzać odwrotnej rekordy DNS lub rekordy PTR usługi Azure przy użyciu programu PowerShell w modelu Klasyczny wdrożenia. "
   services="DNS"
   documentationCenter="na"
   authors="s-malone"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags
   ms.service="DNS"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/28/2016"
   ms.author="smalone" />

# <a name="how-to-manage-reverse-dns-records-for-your-azure-services-classic-using-azure-powershell"></a>Jak zarządzać odwrotnej rekordy DNS dla usługi Azure (klasyczny) przy użyciu programu PowerShell Azure

[AZURE.INCLUDE [dns-reverse-dns-record-operations-arm-selectors-include.md](../../includes/dns-reverse-dns-record-operations-arm-selectors-include.md)]
<BR>
[AZURE.INCLUDE [DNS-reverse-dns-record-operations-intro-include.md](../../includes/dns-reverse-dns-record-operations-intro-include.md)]
<BR>
[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Dowiedz się, jak [wykonać te kroki przy użyciu modelu Menedżera zasobów](dns-reverse-dns-record-operations-ps.md).

## <a name="validation-of-reverse-dns-records"></a>Sprawdzanie poprawności odwrotnej rekordów DNS
Aby upewnić się, że innych firm nie można utworzyć mapowania do swoich domen DNS odwrotnej rekordy DNS, Azure tylko umożliwia tworzenie rekordu DNS odwrotnej miejsce, w którym jest spełniony jeden z następujących czynności:

- Odwrotna DNS FQDN jest nazwę usług w chmurze dla którego został określony lub dowolną nazwę usługi w chmurze w obrębie tej samej subskrypcji np, odwrotnej system DNS jest "contosoapp1.cloudapp.net.".
- Prześlij dalej w odwrotnej FQDN DNS jest rozpoznawana jako imię i nazwisko lub adres IP usługi chmury, dla którego określono lub dowolną nazwę usługi w chmurze lub adresów IP w obrębie tej samej subskrypcji np odwrotnej system DNS jest "app1.contoso.com." będąca alias CName contosoapp1.cloudapp.net.

Sprawdzanie poprawności tylko są wykonywane po odwrotnej właściwość DNS dla usługi w chmurze jest ustawiony lub zmodyfikowane. Okresowe ponowne sprawdzanie poprawności nie zostanie wykonana.

## <a name="add-reverse-dns-to-existing-cloud-services"></a>Dodawanie odwrotnej DNS do istniejących usług w chmurze
Rekord DNS odwrotnej można dodać do istniejącej usługi w chmurze przy użyciu polecenia cmdlet "Zestawu AzureService":

    PS C:\> Set-AzureService –ServiceName “contosoapp1” –Description “App1 with Reverse DNS” –ReverseDnsFqdn “contosoapp1.cloudapp.net.”

## <a name="create-a-cloud-service-with-reverse-dns"></a>Tworzenie usługi w chmurze przy użyciu odwrotnej DNS
Możesz dodać nowe usługi w chmurze za pomocą odwrotnej właściwość DNS określony przy użyciu polecenia cmdlet "Zestawu AzureService":

    PS C:\> New-AzureService –ServiceName “contosoapp1” –Location “West US” –Description “App1 with Reverse DNS” –ReverseDnsFqdn “contosoapp1.cloudapp.net.”

## <a name="view-reverse-dns-for-existing-cloud-services"></a>Widok odwrotnej DNS dla istniejących usług w chmurze
Dla istniejących usługi w chmurze przy użyciu polecenia cmdlet "Get-AzureService" można wyświetlać ustawiona wartość:

    PS C:\> Get-AzureService "contosoapp1"

## <a name="remove-reverse-dns-from-existing-cloud-services"></a>Usuwanie odwrotnej DNS z istniejącymi usługami w chmurze
Właściwość odwrotnej DNS można usunąć z istniejącej usługi w chmurze przy użyciu polecenia cmdlet "Zestawu AzureService". Można to zrobić, ustawiając wartość właściwości odwrotnej DNS do pustego:

    PS C:\> Set-AzureService –ServiceName “contosoapp1” –Description “App1 with Reverse DNS” –ReverseDnsFqdn “”

[AZURE.INCLUDE [FAQ1](../../includes/dns-reverse-dns-record-operations-faq-host-own-arpa-zone-include.md)]

[AZURE.INCLUDE [FAQ2](../../includes/dns-reverse-dns-record-operations-faq-asm-include.md)]
