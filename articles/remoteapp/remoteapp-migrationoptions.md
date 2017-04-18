
<properties 
    pageTitle="Opcje migracji poza Azure RemoteApp | Microsoft Azure" 
    description="Więcej informacji na temat opcji migrowania poza Azure RemoteApp." 
    services="remoteapp" 
    documentationCenter="" 
    authors="ericorman" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/06/2016" 
    ms.author="elizapo" />

# <a name="options-for-migrating-out-of-azure-remoteapp"></a>Opcje migracji poza Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp jest są przerywane. W tym artykule [anons](https://go.microsoft.com/fwlink/?linkid=821148) , aby uzyskać szczegółowe informacje.

Jeśli zostały zatrzymane za pomocą Azure RemoteApp ze względu na [Zawiadomienie o wycofywanie](https://go.microsoft.com/fwlink/?linkid=821148) lub jego zakończeniu oceny, należy więc przeprowadzić migrację poza Azure RemoteApp do innej aplikacji usługi. Istnieją dwie metody różnych migracji: samodzielnie zarządzanymi (często nazywane infrastruktury jako usługa [IaaS]) wdrożenia lub pełni zarządzany (często nazywane Platform jako usługa) lub oprogramowania jako usługa [PaaS-władz akredytacji bezpieczeństwa] oferowanie. 

Sklep internetowy IaaS jest wdrożeniu Zrób zarządzane, obsługiwanej i należącą do Ciebie wdrożony bezpośrednio na wirtualnych maszyn lub systemów fizycznych. Po drugiej stronie pełni zarządzany PaaS-władz akredytacji bezpieczeństwa oferowanie jest bardzo podobne do Azure RemoteApp - partnera zapewnia warstwy usług nad rozwiązaniem zdalnych, która obsługuje operacyjne i obsługi, podczas, jako klienta, wykonaj kilka zarządzania obrazu i aplikacji.

Czytaj dalej, aby uzyskać więcej informacji, włącznie z przykładami różne opcje hostingu.    

## <a name="self-managed-iaas-solutions"></a>Samodzielnie zarządzanymi rozwiązań (IaaS)

### <a name="rds-on-iaas"></a>**Licencji na IaaS** 
Można wdrażać natywnych sesji wdrażania na podstawie usługami pulpitu zdalnego (w systemie Windows Server) za pomocą RemoteApp lub pulpitów lokalnego lub w środowisku hostowanej (jak na maszyny wirtualne Azure). Licencji na wdrożeń IaaS są dostosowane do klientów zna i które mają istniejące wiedzy technicznej z wdrożenia licencji. 

> [AZURE.NOTE]
> Potrzebujesz z Software Assurance (SA) licencje dostępu klienta usług pulpitu zdalnego tej opcji wdrażania licencjonowania zbiorowego.

Wdrażanie licencji na maszyny wirtualne Azure jest łatwiejsza niż kiedykolwiek, kiedy używa się wdrażania i poprawianie szablony (zapoznaj się z [Omówienie](https://blogs.technet.microsoft.com/enterprisemobility/2015/07/13/azure-resource-manager-template-for-rds-deployment/) , a następnie [pobrać je](https://aka.ms/rdautomation)). Możesz uzyskać takie same możliwości skalowania elastyczne Azure klasyczny wdrożenia modelu zasoby (nie Azure zasobów modelu zasobów) w ramach Azure RemoteApp za pomocą [Automatyczne skalowanie skrypt](https://gallery.technet.microsoft.com/scriptcenter/Automatic-Scaling-of-9b4f5e76), mimo że istnieje więcej dostosowań i konfiguracji. Po wdrożeniu licencji na maszyny wirtualne Azure są obsługiwane za pośrednictwem [Azure pomocy technicznej](https://azure.microsoft.com/support/plans/), tym samym pracowników pomocy technicznej, obsługiwane z Azure RemoteApp. Szacowanie na podstawie wartości z istniejących zużycia, kontaktując się z [Pomocy technicznej Azure](https://azure.microsoft.com/support/plans/)można uzyskać kosztów lub można wykonywać obliczenia siebie za pomocą szybko zostać wydane Kalkulator koszt.  Ponadto maszyny wirtualne serii N (obecnie w podglądzie prywatnych) umożliwia dodawanie vGPU - usłyszeć więcej informacji o dodawaniu vGPU i jak o [Ulepszenia licencji w systemie Windows Server 2016](https://myignite.microsoft.com/videos/2794) w naszym Ignite sesji.   

Zawiera wskazówki dotyczące rozmieszczania krok po kroku dla [Systemu Windows Server 2012 R2](http://aka.ms/rdsonazure) i [Windows Server 2016](http://aka.ms/rdsonazure2016) pomagające wdrożenia. Zapoznaj się z [pulpitu zdalnego blogu](https://blogs.technet.microsoft.com/enterprisemobility/?product=windows-server-remote-desktop-services) dla najnowsze informacje.
 
### <a name="citrix-on-iaas"></a>**Citrix na IaaS** 
Natywne Citrix, wdrożenie programu opartego na sesjach program XenApp lub XenDesktop może być używany lokalnego lub w środowisku hostowanej (takie jak na maszyny wirtualne Azure). 

Zapoznaj się z Podręcznik wdrażania krok po kroku, [Citrix XA 7.6 Azure](http://www.citrixandmicrosoft.com/Documents/Citrix-Azure Deployment Guide-v.1.0.docx), aby uzyskać więcej informacji. Dowiedz się więcej o [Citrix Azure](http://www.citrixandmicrosoft.com/Solutions/AzureCloud.aspx), łącznie z Kalkulatora ceny. Możesz również znaleźć [kontakt Citrix](http://citrix.com/English/contact/index.asp) aby omówić opcje z.

## <a name="fully-managed-paassaas-offerings"></a>W pełni zarządzane ofertą (PaaS-SaaS)

### <a name="citrix-cloud"></a>**Chmura Citrix** 
[Citrix istniejącego rozwiązania chmurze](https://www.citrix.com/products/citrix-cloud/), taka sama, jak pod względem architektury Express program XenApp Citrix. Dla istniejących klientów Azure RemoteApp Citrix oferuje [promocji rabat 50%](https://www.citrix.com/blogs/2016/10/03/special-promotion-for-microsoft-azure-remoteapp-customers/) . 

### <a name="citrix-xenapp-express-in-tech-preview"></a>**Citrix program XenApp Express (w wersji tech preview)**
[Zarejestruj się w ich w wersji tech Preview](http://now.citrix.com/remoteapp)i obejrzyj jego [sesji Ignite](https://myignite.microsoft.com/videos/2792) (począwszy od minutę 20:30). Program XenApp Express jest pod względem architektury identyczne w chmurze Citrix, z wyjątkiem zawiera uproszczone zarządzanie interfejsu użytkownika i innych funkcji i możliwości, które są podobne do Azure RemoteApp. 

Dowiedz się więcej na temat [Express program XenApp Citrix](http://now.citrix.com/remoteapp).   

### <a name="citrix-service-provider-program"></a>**Program dostawcy usługi Citrix** 
Program dostawcy usługi Citrix ułatwia usługodawców do przeprowadzania prostotę chmury wirtualnych w środowisku SMB, oferowanie ich usług, które mają się proste, płatne model. Citrix usługodawców rozwoju firmy Microsoft SPLA i rozwiń inwestycji platformy licencji z dowolnego urządzenia, uniwersalny dostęp najszerszym katalog application support, bogate wrażenia, zwiększyć bezpieczeństwo i lepszą skalowalność. Z kolei Citrix usługodawców przyciągnąć więcej subskrybentów, Zwiększ zadowolenie klienta i zmniejszyć ich koszty operacyjne. [Aby uzyskać więcej informacji](http://www.citrix.com/products/service-providers.html) lub [Znajdź partnera](https://www.citrix.com/buy/partnerlocator.html).

### <a name="microsoft-hosted-service-provider"></a>**Microsoft hostowanej dostawcy usługi** 
Partnerzy hostingu zwykle oferują w pełni zarządzane hostowaną pulpitu systemu Windows i usługi aplikacji, które mogą obejmować zarządzanie Azure zasobów, systemy operacyjne, aplikacje i zgłoszeń za pomocą partnera firmy licencji zbiorowych firmie Microsoft i innych dostawców oprogramowania wraz z jest umowę licencyjną dostawcy usługi, aby umożliwić odsprzedaży z licencji dostępu subskrybenta (SAL). Szczegółowe informacje i informacje kontaktowe dla niektórych hosters, specjalizujących udzielania pomocy użytkownikom ich migracji Azure RemoteApp zawiera następujące informacje. Zapoznaj się z [bieżącą listę obsługiwanych dostawców usług](http://aka.ms/rdsonazurecertified) , która została ukończona licencji na IaaS nauki ścieżkę i ocena.  

#### <a name="aspex"></a>**ASPEX**
[ASPEX](http://www.aspex.be/en) specjalizuje się w zakresie migrowania do chmury niezależnych dostawców oprogramowania i model "chcą zoptymalizować ich bieżące ustawienia chmury. ASPEX oferuje szeroką gamę usług zarządzanych, devops i usługi doradcze.  

Lokalizacja podstawowa: Antwerpia, Belgia

Operacja region: Europa Zachodnia

Stan partnera: [Silver](https://partnercenter.microsoft.com/pcv/solution-providers/aspex_9397f5dd-ebdd-405b-b926-19a5bda61f7a/cfe00bac-ea36-4591-a60b-ec001c4c3dff)

Dostawca usług chmury firmy Microsoft: tak

Oferowanie sesji rozwiązań programów RemoteApp i pulpitu: tak, oba

Azure rozwiązania migracji RemoteApp: tak, [Dowiedz się więcej](https://www.aspex.be/en/azure-remote-apps)

**Kontakt:**

- Telefon: +3232202198
- Poczta:[info@aspex.be](mailto:info@aspex.be)
- Sieć Web: [http://cloud.aspex.be/contact-ara-0](http://cloud.aspex.be/contact-ara-0)

#### <a name="conexlink-platform-name-mycloudit"></a>**Conexlink (nazwa platformy: MyCloudIT)**
[MyCloudIT](http://www.mycloudit.com) jest platformą automatyzacji dla firm IT uprościć, optymalizowanie i skalowanie migracji i dostarczanie Pulpity zdalne, zdalnego aplikacji i infrastruktury chmury firmy Microsoft Azure. 

Platforma MyCloudIT skraca czas wdrażania przez 95%, Azure kosztów za pomocą 30% i przenosi całą infrastrukturę informatyczną swoich klientów w chmurze w ciągu kilku kilka naciskania klawiszy. Partnerzy mogą teraz zarządzać klientów z jednego globalnego pulpitu nawigacyjnego, użytkowników końcowych usługi na świecie, jak nigdy wcześniej i rozwijaniu przychodów bez dodawania dodatkowe obciążenie lub rozległa szkolenie Azure.  

Lokalizacja podstawowa: Dallas, TX, USA

Operacja region: na całym świecie

Stan partnera: [Złoty](https://partnercenter.microsoft.com/pcv/solution-providers/conexlink_4298787366/843036_1?k=Conexlink)

Dostawca usług chmury firmy Microsoft: tak

Oferowanie sesji rozwiązań programów RemoteApp i pulpitu: tak, oba

Azure rozwiązania migracji RemoteApp: tak, [Dowiedz się więcej](https://mycloudit.com/remote-app-microsoft/)

**Kontakt:**
- Garoutte Michała, VP rozwoju firmy

   Telefon: 972-218-0741

   Adres e-mail:[brian.garoutte@conexlink.com](mailto:brian.garoutte@conexlink.com)

#### <a name="acuutech"></a>**Acuutech**
[Acuutech](http://www.acuutech.com) specjalizuje się w udostępniania pulpitu rozwiązań obsługiwanych przez firmę pełnego pulpitu i aplikacji niezależnych producentów oprogramowania środowiska oparte na technologii Microsoft klientowi globalnej podstawowego z Azure i ich centrach danych.

Lokalizacja podstawowa: Londyn, Zjednoczone Królestwo; Singapur; Houston, TX

Operacja region: na całym świecie

Stan partnera: złoty

Dostawca usług chmury firmy Microsoft: tak

Oferowanie sesji rozwiązań programów RemoteApp i pulpitu: tak, oba

Azure rozwiązania migracji RemoteApp: tak, [Dowiedz się więcej](http://www.acuutech.com/ara-migration/)

**Kontakt:**

- Wielka Brytania:

  Dom Jorku 5 i 6, drogi Langston

  Loughton, Essex IG10 3TQ
  
  Telefon: +44 (0) 20 8502 2155
 
- Singapur:

  Ulica 100 Cecil, #09-02 
  
  Globusa Singapur 069532
  
  Numer telefonu: + 65 6709 4933
 
- Ameryka Północna: 

  3601 S. Sandman St.
  
  Pakiet 200, Houston, TX 77098
  
  Telefon: +1 713 691 0800

## <a name="need-more-help"></a>Potrzebujesz dodatkowej pomocy?
Nadal potrzebujesz pomocy, wybierając pozycję lub dalszego masz pytania? Aby uzyskać pomoc dotyczącą za pomocą jednej z następujących metod. 

1.  Wiadomość e-mail na adres [arainfo@microsoft.com](mailto:arainfo@microsoft.com).
2.  Skontaktuj się z [Azure pomocy technicznej](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). Zacznij od otwierania [Azure obsługuje wielkość liter](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
3.  Zadzwoń. [Znajdź numer lokalny sprzedaży](https://azure.microsoft.com/overview/sales-number/).
