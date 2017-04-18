<properties 
    pageTitle="Przewodnik dla deweloperów Azure dla instytucji rządowych" 
    description="Umożliwia porównanie funkcji i wskazówki dotyczące tworzenia aplikacji dla instytucji rządowych Azure" 
    services="" 
    cloud="gov"
    documentationCenter="" 
    authors="Joharve2" 
    manager="Chrisnie" 
    editor=""/>

<tags 
    ms.service="multiple" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="azure-government" 
    ms.date="10/29/2015" 
    ms.author="jharve"/>


#  <a name="microsoft-azure-government-developer-guide"></a>Przewodnik dewelopera dla instytucji rządowych platformy Microsoft Azure 

<p> Microsoft Azure dla instytucji rządowych jest fizycznie i sieci odizolowanych wystąpienia programu Microsoft Azure.  Ten przewodnik deweloperów zawiera szczegóły dotyczące różnic deweloperów tej aplikacji i Administratorzy należy interakcje i Praca z tych oddzielnych regionów Azure.

<!--Table of contents for topic, the words in brackets must match the heading wording exactly-->


## <a name="in-this-topic"></a>W tym temacie


+ [Omówienie](#Overview)
+ [Wskazówki dotyczące deweloperów](#Guidance)
+ [Funkcje jest obecnie dostępna w programie Microsoft Azure dla instytucji rządowych](#Features)
+ [Mapowanie punktu końcowego](#Endpoint)
+ [Następne kroki](#next)


## <a name="Overview"></a>Omówienie

Microsoft Azure dla instytucji rządowych jest osobne wystąpienie usługi Microsoft Azure odpowiadać na potrzeby zabezpieczenia i zgodność federal administracji państwowej USA, stan i lokalnych, szczebla i ich dostawców rozwiązań. Azure dla instytucji rządowych oferuje fizycznej i sieci izolacji od wdrożeń poza USA dla instytucji rządowych i udostępnia podsiecią personel USA. 

Firma Microsoft udostępnia wiele narzędzi do tworzenia i wdrażania aplikacje w chmurze firmy Microsoft globalne usługi Microsoft Azure ("globalne usługi") i usługi Microsoft Azure dla instytucji rządowych.

Podczas tworzenia i wdrażania aplikacji deweloperów usługi dla instytucji rządowych Azure, zamiast globalnego usługi, należy mieć na uwadze kluczowych różnic dwóch usług.  Specjalnie wokół instalowaniu i konfigurowaniu ich środowisko programowania, konfigurowanie punkty końcowe pisania aplikacji i wdrażanie ich jako usługi Azure dla instytucji rządowych.

W tym dokumencie podsumowano różnice te i uzupełnia dostępnych informacji w witrynie(http://www.azure.com/gov "Dla instytucji rządowych Azure") [Azure dla instytucji rządowych]i [Biblioteka techniczna firmy Microsoft Azure](http://msdn.microsoft.com/cloud-app-development-msdn "MSDN") w witrynie MSDN. Oficjalne informacje mogą być również dostępne w innych lokalizacjach takich jak [Microsoft Azure Trust Center] (https://azure.microsoft.com/support/trust-center/ "Microsoft Azure Trust Center"-), [Centrum dokumentacji Azure](https://azure.microsoft.com/documentation/) w [Azure blogów] (https://azure.microsoft.com/blog/ "Azure blogów"-). 

Ta zawartość jest przeznaczona dla partnerów i deweloperów, którzy są wdrażania Microsoft Azure dla instytucji rządowych.



## <a name="Guidance"></a>Wskazówki dotyczące deweloperów
Ponieważ większość technicznych zawartości, która jest obecnie dostępna przyjęto założenie, że aplikacje są opracowywane globalne usługi, a nie dla Microsoft Azure dla instytucji rządowych, jest ważna w przypadku w celu zapewnienia, że programiści wiedzą o istnieniu istotnych kluczowych różnic dla aplikacji utworzonych być obsługiwana przez Azure dla instytucji rządowych.

- Najpierw istnieją usług i różnic między funkcjami, oznacza to, że niektóre funkcje, które są w określonych regionach globalne usługi nie mogą być dostępne w Azure dla instytucji rządowych.

- Drugi funkcji, które są dostępne w Azure dla instytucji rządowych, istnieją konfiguracji różnice w usłudze globalnej.  W związku z tym należy zapoznać się z przykładowego kodu, konfiguracji i kroki w celu zapewnienia, że tworzenia i wykonywania w środowisku usług w chmurze dla instytucji rządowych Azure.


## <a name="Features"></a>Funkcje jest obecnie dostępna w programie Microsoft Azure dla instytucji rządowych
Azure dla instytucji rządowych obecnie występują następujące usługi dostępne w regionach zarówno IOWA GOV nam i VIRGINIA US GOV:

- Maszyn wirtualnych
- Usług w chmurze
- Miejsca do magazynowania
- Usługi Active Directory
- Harmonogram
- Wirtualna sieć
- Baza danych SQL
- Pliki Azure
- Usługi multimediów
- Menedżer ruchu
- Usługa Bus
- StorSimple
- Redis pamięci podręcznej
- Kopia zapasowa Azure
- Automatyzacji
- ExpressRoute
- itd.

Inne usługi są dostępne i innych usług, zostaną dodane w sposób ciągły.  Najnowsze listy usług zobacz [obszary strony](https://azure.microsoft.com/regions/#services) , który wyróżni każdego regionu dostępne i ich usług.  

Obecnie Iowa GOV nam Virginia GOV nam się centrach danych pomocniczych Azure dla instytucji rządowych.  Zajrzyj do strony regionów powyżej dla bieżącego centrach danych i dostępne usługi.

## <a name="Endpoint"></a>Mapowanie punktu końcowego

Skorzystaj z poniższej tabeli pomocne podczas mapowania publicznej Microsoft Azure i baza danych SQL punkty końcowe do określonych punktów końcowych Azure dla instytucji rządowych.


Typ usługi|Azure publicznej|Azure dla instytucji rządowych
---|---|---
Portal zarządzania|manage.windowsazure.com|manage.windowsazure.us
Ogólne|*. windows.net|*. usgovcloudapi.net
Podstawowe|*. core.windows.net|*. core.usgovcloudapi.net
Obliczenia|*. cloudapp.net|*. usgovcloudapp.net
Magazyn obiektów blob|*. blob.core.windows.net|   *. blob.core.usgovcloudapi.net
Magazyn kolejek|*. queue.core.windows.net|*. queue.core.usgovcloudapi.net
Magazyn tabel|*. table.core.windows.net|*. table.core.usgovcloudapi.net
Zarządzanie usługą|Management.Core.Windows.NET|Management.Core.usgovcloudapi.NET
Baza danych SQL|*. database.windows.net|*. database.usgovcloudapi.net
Punkt końcowy równoważenia obciążenia ARM|https://Management.Windows.NET|https://Management.usgovcloudapi.NET  

* W przypadku uwierzytelniania ARM za pośrednictwem Azure AD odwołanie [Uwierzytelniania żądania Menedżera zasobów Azure](https://msdn.microsoft.com/library/azure/dn790557.aspx)

## <a name="next"></a>Następne kroki

Jeśli interesuje Cię dowiedzieć się więcej i dla instytucji rządowych Azure należy korzystać z niektórych z poniższych łączy.

- **[Załóż konto wersji próbnej](https://azuregov.microsoft.com/trial/azuregovtrial)**

- **[Pobieranie i uzyskiwanie dostępu do Azure dla instytucji rządowych](http://azure.com/gov)**

- **[Omówienie Azure dla instytucji rządowych](/azure-government-overview)**

- **[Blog Azure dla instytucji rządowych](http://blogs.msdn.com/b/azuregov/)**

- **[Zgodność Azure](https://azure.microsoft.com/support/trust-center/compliance/)**

<!--Anchors-->



<!-- Images. -->

[1]: ./media/azure-government-developer-guide/publisherguide.png


<!--Link references-->
[Link 1 to another azure.microsoft.com documentation topic]: virtual-machines-windows-hero-tutorial.md
[Link 2 to another azure.microsoft.com documentation topic]: web-sites-custom-domain-name.md
[Link 3 to another azure.microsoft.com documentation topic]: storage-whatis-account.md
