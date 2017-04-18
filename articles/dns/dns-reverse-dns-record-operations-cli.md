<properties
   pageTitle="Zarządzanie odwrotnej rekordy DNS dla usługi Azure za pomocą interfejsu wiersza polecenia Azure | Microsoft Azure"
   description="Jak zarządzać odwrotnej rekordy DNS lub rekordy PTR usługi Azure za pomocą interfejsu wiersza polecenia Azure w Menedżerze zasobów"
   services="DNS"
   documentationCenter="na"
   authors="s-malone"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags
   ms.service="DNS"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/28/2016"
   ms.author="smalone" />

# <a name="how-to-manage-reverse-dns-records-for-your-azure-services-using-the-azure-cli"></a>Jak zarządzać odwrotnej rekordy DNS dla usługi Azure za pomocą interfejsu wiersza polecenia Azure

[AZURE.INCLUDE [DNS-reverse-dns-record-operations-arm-selectors-include.md](../../includes/dns-reverse-dns-record-operations-arm-selectors-include.md)]
<BR>
[AZURE.INCLUDE [DNS-reverse-dns-record-operations-intro-include.md](../../includes/dns-reverse-dns-record-operations-intro-include.md)]
<BR>
[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][model klasyczny wdrożenia](dns-reverse-dns-record-operations-classic-ps.md).

## <a name="validation-of-reverse-dns-records"></a>Sprawdzanie poprawności odwrotnej rekordów DNS
Aby upewnić się, że innych firm nie można utworzyć mapowania do swoich domen DNS odwrotnej rekordy DNS, Azure tylko umożliwia tworzenie rekordu DNS odwrotnej miejsce, w którym jest spełniony jeden z następujących czynności:

- "ReverseFqdn" jest taki sam jak dla zasobu publiczny adres IP, dla których określono "kwalifikowaną" lub "Fqdn" dowolny publiczny adres IP w tej samej subskrypcji np., "ReverseFqdn" jest "contosoapp1.northus.cloudapp.azure.com.".

- "ReverseFqdn" do przodu jest rozpoznawana jako nazwa lub IP publiczny adres IP, dla którego określono lub publiczny adres IP "Fqdn" lub adresów IP w tej samej subskrypcji np "ReverseFqdn" jest "app1.contoso.com." czyli alias CName "contosoapp1.northus.cloudapp.azure.com."

Sprawdzanie poprawności tylko są wykonywane po odwrotnej właściwość DNS publiczny adres IP ma wartość lub zmodyfikowane. Okresowe ponowne sprawdzanie poprawności nie zostanie wykonana.

## <a name="add-reverse-dns-to-existing-public-ip-addresses"></a>Dodawanie odwrotnej DNS do istniejącego publiczne adresy IP
DNS odwrotnej można dodać do istniejącego publiczny adres IP przy użyciu zestawu adresów ip publicznej sieci azure:

    azure network public-ip set -n PublicIp -g NRP-DemoRG-PS -f contosoapp1.westus.cloudapp.azure.com.

Jeśli chcesz dodać odwrotnej DNS istniejącego publiczny adres IP, który nie ma jeszcze nazwę DNS, możesz także określić nazwę DNS. Możesz dodać osiągnąć przy użyciu zestawu adresów ip publicznej sieci azure:

    azure network public-ip set -n PublicIp -g NRP-DemoRG-PS -d contosoapp1 -f contosoapp1.westus.cloudapp.azure.com.

## <a name="create-a-public-ip-address-with-reverse-dns"></a>Tworzenie publiczny adres IP przy użyciu odwrotnej DNS
Tworzenie nowego publiczny adres IP odwrotnej właściwość DNS określona za pomocą publicznej sieci azure ip można dodać:

    azure network public-ip create -n PublicIp3 -g NRP-DemoRG-PS -l westus -d contosoapp3 -f contosoapp3.westus.cloudapp.azure.com.

## <a name="view-reverse-dns-for-existing-public-ip-addresses"></a>Widok odwrotnej DNS dla istniejących publicznych adresów IP
Dla istniejących publiczny adres IP przy użyciu Pokaż ip publicznej sieci azure można wyświetlać ustawiona wartość:

    azure network public-ip show -n PublicIp3 -g NRP-DemoRG-PS

## <a name="remove-reverse-dns-from-existing-public-ip-addresses"></a>Usuwanie odwrotnej DNS z istniejących publicznych adresów IP
Właściwość odwrotnej DNS można usunąć z istniejących publiczny adres IP przy użyciu zestawu adresów ip publicznej sieci azure. Można to zrobić przez ustawienie właściwości ReverseFqdn wartość puste:

    azure network public-ip set -n PublicIp3 -g NRP-DemoRG-PS –f “”

[AZURE.INCLUDE [FAQ1](../../includes/dns-reverse-dns-record-operations-faq-host-own-arpa-zone-include.md)]

[AZURE.INCLUDE [FAQ2](../../includes/dns-reverse-dns-record-operations-faq-arm-include.md)]
