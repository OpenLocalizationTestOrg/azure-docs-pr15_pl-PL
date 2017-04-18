<properties
  pageTitle="Za pomocą Azure DNS z innymi usługami Azure | Microsoft Azure"
  description="Opis sposobu rozpoznawania nazw dla innych usług Azure za pomocą Azure DNS"
  services="dns"
  documentationCenter="na"
  authors="sdwheeler"
  manager="carmonm"
  editor=""
  tags="azure dns"
/>
<tags
  ms.service="dns"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="na"
  ms.workload="infrastructure-services"
  ms.date="09/21/2016"
  ms.author="sewhee"
/>

# <a name="using-azure-dns-with-other-azure-services"></a>Za pomocą Azure DNS z innymi usługami Azure

Azure DNS jest hostowanej usługi DNS zarządzania i nazwę rozdzielczość. Umożliwia tworzenie publicznej nazw DNS dla innych aplikacji i usług, który został wdrożony w Azure. Tworzenie nazwy dla usługi Azure w domenie niestandardowej jest prosty sposób dodawania rekordu typu poprawna tej usługi.

* Dla przydzielany dynamicznie adresy IP musisz utworzyć rekord DNS CNAME, który mapy do nazwy DNS Azure utworzone dla tej usługi. Standardy DNS uniemożliwiają przy użyciu rekordu CNAME dla wierzchołka strefy.
* Dla statycznie przydzielonego adresów IP możesz utworzyć za pomocą dowolnej nazwy, łącznie z nazwą _domeny bez_ u wierzchołka strefy rekord DNS.

W poniższej tabeli przedstawiono typy rekordów obsługiwane, które mogą być używane dla różnych usług Azure. Jak widać z tej tabeli, Azure DNS obsługuje tylko rekordy DNS dla zasobów sieciowych w Internecie. Azure DNS nie można używać do rozpoznawania nazw adresów wewnętrzne, prywatne.

| Usługa Azure | Interfejs sieciowy | Opis |
|---------------|-------------------|-------------|
| Brama aplikacji | Frontonu publiczny adres IP | Możesz utworzyć rekordu DNS A lub CNAME. |
| Usługa równoważenia obciążenia | Frontonu publiczny adres IP | Możesz utworzyć rekordu DNS A lub CNAME. Usługi równoważenia obciążenia może mieć jest dynamicznie przypisywany adres publiczny adres IP protokołu IPv6. Dlatego należy utworzyć rekord CNAME dla adresów IPv6. |
| Menedżer ruchu | Nazwa publiczna | Można tworzyć tylko CNAME mapowany do nazwy trafficmanager.net przydzielonych do swojego profilu Menedżer ruchu. Aby uzyskać więcej informacji zobacz [działa jak Menedżer ruchu](../traffic-manager/traffic-manager-how-traffic-manager-works.md#traffic-manager-example). |
| Usługa w chmurze | Publiczny adres IP | Dla statycznie przydzielonego adresów IP możesz utworzyć rekord A systemu DNS. Dla przydzielany dynamicznie adresy IP musisz utworzyć rekord CNAME, która łączy do nazwy _cloudapp.net_ . Ta reguła dotyczy maszyny wirtualne utworzone w portalu klasyczny, ponieważ są oni wdrażane jako usługi w chmurze. Aby uzyskać więcej informacji zobacz [Konfigurowanie niestandardowej nazwy domeny w usług w chmurze](../cloud-services/cloud-services-custom-domain-name-portal.md). |
| Aplikacji usługi | Zewnętrzne IP | Dla zewnętrznych adresów IP możesz utworzyć rekord A systemu DNS. W przeciwnym razie należy utworzyć rekord CNAME, która łączy do nazwy azurewebsites.net. Aby uzyskać więcej informacji zobacz [Mapowanie niestandardowej nazwy domeny do Azure aplikacji](../app-service-web/web-sites-custom-domain-name.md) |
| Menedżer zasobów maszyny wirtualne | Publiczny adres IP | Menedżer zasobów maszyny wirtualne może zawierać publiczne adresy IP. Maszyn wirtualnych przy użyciu adresu IP publicznej mogą być również za usługi równoważenia obciążenia. Możesz utworzyć rekordu DNS A lub CNAME dla adresu publicznej. Ten niestandardowej nazwy umożliwia pomijanie VIP w module równoważenia obciążenia. |
| Klasyczny maszyny wirtualne | Publiczny adres IP | Klasyczny maszyny wirtualne utworzony przy użyciu programu PowerShell lub interfejsu wiersza polecenia można skonfigurować przy użyciu dynamiczne lub statyczne (zarezerwowane) wirtualnego adresu. Możesz utworzyć DNS CNAME lub rekord, odpowiednio. |
