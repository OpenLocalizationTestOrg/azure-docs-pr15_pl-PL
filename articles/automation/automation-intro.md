<properties
    pageTitle="Co to jest Azure automatyzacji | Microsoft Azure"
    description="Dowiedz się, wartość, jaką zawiera automatyzacji Azure i odpowiedzi na często zadawane pytania, tak, aby można rozpocząć tworzenie, przy użyciu runbooks i DSC automatyzacji Azure."
    services="automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="jwhit"
    editor=""
    keywords="Co to jest automatyzacja, azure automatyzacji, przykłady automatyzacji azure"/>
<tags
    ms.service="automation"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article" 
    ms.date="05/10/2016"
    ms.author="magoedte;bwren"/>

# <a name="azure-automation-overview"></a>Omówienie automatyzacji Azure

Microsoft Azure automatyzacji umożliwia użytkownikom Automatyzowanie ręcznego, długim podatne na błędy i często powtarzających się zadań, często wykonywanych w środowisku chmury i enterprise. Pozwala zaoszczędzić czas i zapewnia większą niezawodność zwykła zadania administracyjne i nawet planuje je do wykonania automatycznie w regularnych odstępach czasu. Można zautomatyzować przy użyciu runbooks procesów lub zautomatyzować przy użyciu konfiguracji stan potrzeby zarządzania konfiguracją. Ten artykuł zawiera krótkie omówienie automatyzacji Azure i zawiera odpowiedzi na często zadawane pytania. Inne artykuły w tej bibliotece, aby uzyskać szczegółowe informacje na różne tematy może dotyczyć.


## <a name="automating-processes-with-runbooks"></a>Automatyzowanie procesów za pomocą runbooks

Działań aranżacji jest zestaw zadań, które wykonać kilka zautomatyzowany proces automatyzacji Azure. Prosty proces, takich jak Rozpoczynanie maszyny wirtualnej oraz tworzenie pozycji dziennika może być lub może być złożone działań aranżacji łączący innych mniejszych runbooks do wykonywania złożonym procesem, przez wielu zasobów lub nawet wielu chmury i w środowiskach lokalna.  

Na przykład możesz może mieć istniejącego procesu ręcznego dla obcinanie z bazą danych SQL, gdy zbliża się maksymalny rozmiar, który zawiera wiele kroków, takich jak łączenie się z serwerem nawiązywanie połączenia z bazą danych, pobieranie rozmiar bieżącej bazy danych, sprawdzanie, jeśli próg przekroczony obciąć go i powiadomienia użytkownika. Zamiast ręcznie wykonywania każdy z tych kroków, można utworzyć działań aranżacji, które będzie wykonywać wszystkie te zadania jako pojedynczy proces. Czy Rozpoczynanie działań aranżacji, wprowadź wymagane informacje, takie jak nazwa serwera SQL, nazwa bazy danych i poczty e-mail adresata, a następnie znajdują się ponownie, gdy proces zostanie zakończony. 


## <a name="what-can-runbooks-automate"></a>Co można zautomatyzować runbooks?

Runbooks w automatyzacji Azure na podstawie środowiska Windows PowerShell lub Windows PowerShell przepływ pracy, tak jak wszystkie elementy, które można wykonywać programu PowerShell. Jeśli w aplikacji lub usługi jest interfejs API, działań aranżacji można pracować z nim. Jeśli masz modułu programu PowerShell dla aplikacji, można załadować tego modułu do automatyzacji Azure i obejmować tych poleceń cmdlet programu działań aranżacji. Azure runbooks automatyzacji Uruchom w chmurze Azure i dostęp do zasobów w chmurze ani zasobów zewnętrznych, które są dostępne z chmury. Korzystając z [Hybrydowego działań aranżacji pracownik](automation-hybrid-runbook-worker.md), runbooks można uruchamiać za pomocą Centrum danych lokalnych do zarządzania zasobów lokalnych. 


## <a name="getting-runbooks-from-the-community"></a>Uzyskiwanie runbooks z społeczności

[Galeria działań aranżacji](automation-runbook-gallery.md#runbooks-in-runbook-gallery) zawiera runbooks od firmy Microsoft i społeczności, że można użyć bez zmian w środowisku lub je dostosować do własnych celów. Są one również przydatne jako odwołania, aby dowiedzieć się, jak tworzyć własne runbooks. Można nawet Współtworzenie własne runbooks do galerii, że zachodzi podejrzenie, że inni użytkownicy mogą być przydatne. 


## <a name="creating-runbooks-with-azure-automation"></a>Tworzenie Runbooks za pomocą automatyzacji Azure 

Możesz [utworzyć własne runbooks](automation-creating-importing-runbook.md) od podstaw lub modyfikowanie runbooks z [Galerii działań aranżacji](http://msdn.microsoft.com/library/azure/dn781422.aspx) własne zgodnie z wymaganiami. Istnieją trzy różne [Typy działań aranżacji](automation-runbook-types.md) , który można wybrać w zależności od wymagania i obsługi programu PowerShell. Jeśli wolisz pracować bezpośrednio z kodem programu PowerShell, można [działań aranżacji programu PowerShell](automation-runbook-types.md#powershell-runbooks) lub edytowany w trybie offline lub za pomocą [edytora tekstowy](http://msdn.microsoft.com/library/azure/dn879137.aspx) w portalu Azure [działań aranżacji przepływu pracy programu PowerShell](automation-runbook-types.md#powershell-workflow-runbooks) . Jeśli wolisz edytować działań aranżacji bez na kodu, można utworzyć [graficzny działań aranżacji](automation-runbook-types.md#graphical-runbooks) przy użyciu [edytora graficznego](automation-graphical-authoring-intro.md) w portalu Azure. 

Wolisz oglądanie do odczytu? Masz przyjrzeć się poniżej klipu wideo z sesji Microsoft Ignite maj 2015 r. Uwaga: Gdy pojęcia i funkcje omówione w tym klipie wideo są poprawne, automatyzacji Azure zanotowano znacznie od ten klip wideo został zapisany, go teraz ma szerszej interfejs użytkownika w portalu Azure i obsługuje dodatkowe funkcje.

> [AZURE.VIDEO microsoft-ignite-2015-automating-operational-and-management-tasks-using-azure-automation]


## <a name="automating-configuration-management-with-desired-state-configuration"></a>Automatyzowanie zarządzania konfiguracją potrzeby stan konfiguracji 

[DSC programu PowerShell](https://technet.microsoft.com/library/dn249912.aspx) jest platformy zarządzania, która umożliwia zarządzanie, wdrażania i wymusić konfiguracji dla hostów fizycznych i maszyn wirtualnych przy użyciu deklaracyjnych składni programu PowerShell. Można zdefiniować konfiguracji na centralnej DSC pobierają serwerze komputerach docelowych automatycznie można pobrać i zastosować. DSC udostępnia zestaw poleceń cmdlet programu PowerShell służące do zarządzania konfiguracji i zasobów.  

[DSC automatyzacji Azure](automation-dsc-overview.md) to rozwiązanie oparte na chmurze DSC programu PowerShell, udostępniająca usługi wymagane w środowiskach korporacyjnych.  Możesz zarządzać zasobami DSC w automatyzacji Azure i Zastosuj konfiguracji maszyn wirtualnych lub fizycznych, które pobrane z serwera pobierają DSC w chmurze Azure.  Umożliwia także usług reporting services, informujące, że możesz ważne wydarzenia takie jak węzły odpowiadają regułom z konfiguracją przydzielone oraz nowej konfiguracji została zastosowana. 


## <a name="creating-your-own-dsc-configurations-with-azure-automation"></a>Tworzenie konfiguracji DSC przy użyciu automatyzacji Azure

[Konfiguracje DSC](automation-dsc-overview.md#azure-automation-dsc-terms) Określ żądany stan węzła.  Wiele węzłów można stosować tej samej konfiguracji, aby zagwarantować wszystkich zachowanie stan identyczne.  Można utworzyć edytora konfigurację przy użyciu dowolnego tekstu na komputerze lokalnym, a następnie zaimportować je do automatyzacji Azure miejsce, w którym go skompilować i zastosować go węzły.


## <a name="getting-modules-and-configurations"></a>Uzyskiwanie moduły i konfiguracji 

Możesz uzyskać [moduły programu PowerShell](automation-runbook-gallery.md#modules-in-powershell-gallery) zawierające polecenia cmdlet, używanej w runbooks i konfiguracji DSC z [Galerii programu PowerShell](http://www.powershellgallery.com/). Można uruchomić tej galerii z portalu Azure i moduły bezpośrednio zaimportować automatyzacji Azure lub możesz pobrać i zaimportować je ręcznie. Nie można zainstalować moduł bezpośrednio z poziomu portalu Azure, ale można pobrać zainstaluj je, jak w przypadku innych modułu. 


## <a name="example-practical-applications-of-azure-automation"></a>Przykład praktyczne zastosowania automatyzacji Azure 

Poniżej przedstawiono kilka przykładów co to są rodzaje scenariusze automatyzacji przy użyciu automatyzacji Azure. 

* Tworzenie i kopiowanie maszyn wirtualnych w różnych subskrypcjach Azure. 
* Planowanie kopii pliku z komputera lokalnego do kontenera magazyn obiektów Blob platformy Azure. 
* Automatyzowanie funkcji zabezpieczeń, takich jak odmówić żądań w kliencie zostanie wykryta odmowa usługi. 
* Upewnij się, że maszyny ciągłe Wyrównaj za pomocą zasad zabezpieczeń skonfigurowane.
* Zarządzanie wdrażaniem ciągły kodu aplikacji, w chmurze i infrastruktury lokalnej. 
* Tworzenie las usługi Active Directory platformy Azure w środowisku usługi ćwiczenia. 
* W przypadku zbliża się maksymalny rozmiar bazy danych, należy obciąć tabeli w bazie danych SQL. 
* Zdalne zaktualizuj ustawienia środowiska Azure witryny sieci Web. 


## <a name="how-does-azure-automation-relate-to-other-automation-tools"></a>Jaki jest związek między automatyzacji Azure inne narzędzia automatyzacji?

[Usługa zarządzania automatyzacji (SMA)](http://technet.microsoft.com/library/dn469260.aspx) jest przeznaczona do automatyzowania zadań zarządzania w chmurze prywatne. Jest ona instalowana lokalnie w centrum danych jako składnik pakietu [Microsoft Azure](https://www.microsoft.com/en-us/server-cloud/). SMA i automatyzacji Azure za pomocą tego samego formatu działań aranżacji na podstawie środowiska Windows PowerShell i przepływu pracy programu Windows PowerShell, ale SMA nie obsługuje [runbooks graficznego](automation-graphical-authoring-intro.md).  

[System Centrum 2012 Orchestrator](http://technet.microsoft.com/library/hh237242.aspx) jest przeznaczona dla automatyzacji zasobów lokalnych. Go w formacie działań aranżacji innego niż automatyzacji Azure i Usługa zarządzania automatyzacji i ma interfejsem graficznym utworzyć runbooks bez konieczności dowolnego skryptów. Jej runbooks składają się z działań z pakiety integracji, które są napisane specjalnie dla Orchestrator. 


## <a name="where-can-i-get-more-information"></a>Gdzie można uzyskać więcej informacji? 

Różnych zasobów dostępnych dla Aby dowiedzieć się więcej na temat automatyzacji Azure i tworzenie własnych runbooks. 

* **Biblioteka automatyzacji Azure** to, gdzie są w tej chwili. Artykuły w tej bibliotece dostarczenie dokumentacji pełną od konfiguracji i administrowania automatyzacji Azure i tworzenia własnych runbooks. 
* [Polecenia cmdlet programu PowerShell Azure](http://msdn.microsoft.com/library/jj156055.aspx) zawiera informacje dotyczące Automatyzacja operacji Azure za pomocą programu Windows PowerShell. Runbooks do pracy z zasobami Azure za pomocą tych poleceń cmdlet. 
* [Blog dotyczący zarządzania](https://azure.microsoft.com/blog/tag/azure-automation/) zawiera najnowsze informacje na automatyzacji Azure i inne technologie zarządzania od firmy Microsoft. Należy subskrybować tego blogu, aby być na bieżąco z najnowszymi od zespołu automatyzacji Azure. 
* [Forum automatyzacji](http://go.microsoft.com/fwlink/p/?LinkId=390561) umożliwia publikować pytania dotyczące automatyzacji Azure należy zająć się przez firmę Microsoft i społeczności automatyzacji. 
* [Polecenia cmdlet automatyzacji Azure](https://msdn.microsoft.com/library/mt244122.aspx) zawiera informacje dotyczące Automatyzowanie zadań administracyjnych. Zawiera polecenia cmdlet, aby zarządzanie kontami automatyzacji, składniki majątku runbooks, DSC.


## <a name="can-i-provide-feedback"></a>Czy można przekazywać opinie? 

**Sprawdź Przekaż nam swoją opinię!** Jeśli szukasz rozwiązania działań aranżacji automatyzacji Azure lub modułu integracji Opublikuj prośbę o skrypt w Centrum skryptów. Jeśli masz żądania opinii lub funkcji automatyzacji Azure publikowanie ich na [Głos użytkownika](http://feedback.windowsazure.com/forums/34192--general-feedback). Dziękujemy! 


