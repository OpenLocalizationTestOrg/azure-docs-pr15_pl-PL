<properties
   pageTitle="Wprowadzenie do Menedżera zasobów klaster tkaninie usługi | Microsoft Azure"
   description="Wprowadzenie do usługi tkaninie klaster Menedżera zasobów."
   services="service-fabric"
   documentationCenter=".net"
   authors="masnider"
   manager="timlt"
   editor=""/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/19/2016"
   ms.author="masnider"/>

# <a name="introducing-the-service-fabric-cluster-resource-manager"></a>Wprowadzenie do Menedżera zasobów klaster tkaninie usługi
Zazwyczaj zarządzanie systemów informatycznych lub zbiór usług są przeznaczone wprowadzenie kilku fizycznej lub maszyn wirtualnych zarezerwowane dla tych określonych usług lub systemów. Wiele usług głównych zostały podzielone na warstwa "" i "data" lub "miejsce do magazynowania" warstwa, być może z kilku innych składników specjalnych, takich jak pamięci podręcznej. Innych typów aplikacji musi warstwa wiadomości, gdzie żądania przepływu Powiększ i Pomniejsz połączony warstwa pracy analizy lub przekształcenie niezbędne jako części wiadomości. Każdego typu obciążenie pracą masz określonych komputerach, przeznaczonych do niego: bazy danych masz kilka urządzeń przeznaczonych do niej, serwer sieci web jako mało. Jeśli określonego typu obciążenie pracą uszkodzenie maszyny znajdował się na uruchamianie zbyt dostępu, a następnie dodać więcej komputerach z danym typem obciążenie pracą skonfigurowany do uruchomienia na nim lub zastąpione kilka maszyn większe. Prosty. Jeśli komputera nie powiodło się, tę część aplikacji ogólnej uruchomiono z wydajnością dolnym aż do komputera, można go przywrócić. Nadal stosunkowo łatwe (o ile nie musi być zabawa).

Teraz jednak Przejdźmy powiedz znalezieniu potrzeba skalowania i miały kontenerów i/lub już microservice. Nieoczekiwanie możesz znaleźć siebie za pomocą dziesiątki, setki lub nawet tysiące maszyn, dziesiątki różnych typów usług, być może setki innego wystąpienia tych usług, każda z jednego wystąpienia lub repliki dostępność Wysoka (HA).

Nieoczekiwanie Zarządzanie środowiska nie jest tak proste jak zarządzanie komputerami kilka zarezerwowane dla jednego rodzaju obciążenia. Są wirtualnych i już nie mają nazw serwerów (możesz *mieć* przełączenia mindsets z [domowych do bydła](http://www.slideshare.net/randybias/architectures-for-open-and-scalable-clouds/20) po). Konfiguracja jest mniejszy o maszyn i innych informacji dotyczących usług, się. Sprzęcie jest już przeszłość i usług stały małych systemów rozłożone, obejmujące wiele mniejszych elementów sprzętu towaru.

Wyniku dzielenie wcześniej wbudowanymi, warstwowych aplikacji na poszczególnych usług sprzęt towarów z systemem, teraz masz wiele więcej kombinacji na zajęcie się. Kto decyduje, jakie typy obciążenia można uruchamiać na jaki sprzęt i ile? Które obciążenia działa poprawnie w tym samym sprzęcie i które powodują konflikt? Gdy komputerze awarii... co jeszcze działała? Kto jest odpowiedzialny za upewnienia się, że obciążenie pracą uruchamiania ponownie? Czy czekać na komputerze (wirtualny?) wróci lub z obciążeń pracą automatycznie kończą się niepowodzeniem na innych komputerach i Zachowaj działa? Jest interwencja wymagane? Co z otwieraniem uaktualnienia w tego rodzaju środowiska?

Jako deweloperów i operatorów, w tym sortowanie świata możemy zamiar potrzebujesz pomocy w zarządzaniu wpływają i zostanie wyświetlony sens, że binge zatrudnienia i próby papieru na złożoność osobom nie jest prawidłowa odpowiedź.

Co należy zrobić?

## <a name="introducing-orchestrators"></a>Wprowadzenie do orchestrators
"Orchestrator" jest termin ogólny dla fragmentu oprogramowanie, które umożliwia administratorom zarządzanie środowiskach tego typu. Orchestrators są składniki, które pobierają w żądania, takich jak "Chcę 5 kopii ta usługa działa w mojej środowisku", wprowadź wartość true, a następnie (Wypróbuj) zachować je w ten sposób.

Orchestrators (nie ludzi) są, co w kierunku do akcji podczas komputera nie powiodła się lub kończy obciążenie pracą jakiegoś powodu nieoczekiwane. Większość Orchestrators więcej niż tylko zajęcie się błąd, na przykład przy wdrożeniach nowe, obsługa uaktualnienia i zajmujących się zużycie zasobów, ale istotnie są wszystkie informacje o zachowaniu, że niektóre potrzeby stan konfiguracji w środowisku. Chcesz być w stanie określić Orchestrator, co chcesz i ich wykonaj lifting ciężkich. Chronos Marathon u góry Mesos, floty rój, Kubernetes i tkaninie usługi należą Orchestrators (lub ich wbudowana). Więcej są tworzone przez cały czas jako złożoności zarządzania wdrożeń rzeczywistych w różnych rodzajów środowiska i warunków powiększanie i zmienić.

## <a name="orchestration-as-a-service"></a>Aranżacji jako usługa
Zadanie Orchestrator w klastrze tkaninie usługi jest obsługiwany przede wszystkim przez Menedżera zasobów klaster. Menedżer zasobów usługi tkaninie klaster jest jednym z usługi systemowe w ramach usługi tkaninie i automatycznie uruchamia się w każdym klastrze.  Ogólnie rzecz biorąc Menedżer zasobów klaster zadania jest podzielić na trzy elementy:

1. Wymuszanie reguł
2. Optymalizacja środowiska
3. Pomoc w innych procesów

### <a name="what-it-isnt"></a>Co to jest
W tradycyjnych N warstwa aplikacjach sieci web — była zawsze niektóre pojęcia "równoważenia obciążenia", zazwyczaj nazywana równoważenia obciążenia sieciowego (NLB) lub aplikacji ładowania równoważenia długopłetwy] (ALB) w zależności od tego, gdzie kot w stos sieci. Niektóre urządzenia do równoważenia obciążenia są oparte na przykład oferująca BigIP F5 na sprzęcie, inne osoby są oprogramowania, na przykład firmy Microsoft równoważenia obciążenia. W innych środowiskach może zostać wyświetlony podobną do HAProxy w tej roli. W tych architektury zadania równoważenia obciążenia jest upewnij się, że wszystkie na komputerach różnych stateless frontonu lub różnych komputerach w klastrze (w przybliżeniu) otrzymują samej ilości pracy. Strategie dotyczące to zróżnicowane, wysyłanie każdego innego połączenie na inny serwer do sesji przypinanie lepkości, aby rzeczywisty przydział oszacowanie i połączenia na podstawie jego przewidywany koszt i bieżące obciążenie komputera.

Należy zauważyć, że w najlepszym było strategie mechanizm zapewnienie, że warstwa pozostały w przybliżeniu. Strategie dotyczące równoważenia warstwy danych były całkowicie różne i zależnych mechanizm magazynowania danych, zazwyczaj wyśrodkowania wokół sharding danych, pamięci podręcznej, widoki zarządzane bazy danych i procedur składowanych, itp.

Gdy interesujące są niektóre z tych strategii, Menedżer zasobów klaster tkaninie usługi przypomina nie nic równoważenia obciążenia sieci lub pamięci podręcznej. Podczas równoważenia obciążenia sieci gwarantuje, że przednia punkty końcowe są zbilansowane, przenosząc ruch do miejsce, w którym usługi są uruchomione, Menedżer zasobów usługi tkaninie klaster ma całkowicie różne strategii — istotnie, tkaninie usługi powoduje przeniesienie *usług* na miejsce, w którym zgadzać najbardziej (i oczekuje ruch lub załadowania do obserwowania). Może to być na przykład węzły, które są obecnie zimnej, ponieważ usług, które są obecnie nie robią wiele zadań lub które zostały usunięte lub przeniesione w innym miejscu. Jako przykład innego menedżera zasobów klaster także można przenieść usługi dala od komputera, który ma zostać uaktualnione lub które nadmiernie z powodu kolekcji zużycia w usługach, które zostały uruchomione na nim. Ponieważ Menedżer zasobów klaster jest odpowiedzialny za przenoszenie usług wokół (nie dostarczania ruch sieciowy na miejsce, w którym są już usług), zawiera zestaw funkcji znacznie różni się w porównaniu z co chcesz znaleźć w równoważenia obciążenia sieci i wykorzystuje strategie różni się do zapewnienia, że zasoby sprzętowe w klastrze mogą być również wykorzystane.

## <a name="next-steps"></a>Następne kroki
- Aby uzyskać informacje na przepływu architektura i informacje w ramach Menedżera zasobów klaster zapoznaj się z [tego artykułu](service-fabric-cluster-resource-manager-architecture.md)
- Menedżer zasobów klaster zawiera wiele opcji opisu klaster. Aby dowiedzieć się więcej na temat ich zapoznaj się z niniejszego artykułu [opisem klastrze tkaninie usługi](service-fabric-cluster-resource-manager-cluster-description.md)
- Więcej informacji na temat innych opcji dostępnych do skonfigurowania usług wyewidencjonowanie tematu na innych konfiguracji Menedżera zasobów klaster dostępne [informacje na temat konfigurowania usług](service-fabric-cluster-resource-manager-configure-services.md)
- Metryki przedstawiono, jak Menedżer zasobów usługi tkaninie klaster zarządza zużycie i pojemnością w klastrze. Aby dowiedzieć się więcej na temat ich i sposobie ich konfigurowania zapoznaj się z [tego artykułu](service-fabric-cluster-resource-manager-metrics.md)
- Menedżer zasobów klaster działa z funkcjami zarządzania tkaninie usługi. [Aby dowiedzieć się więcej na temat tego integracji, w tym](service-fabric-cluster-resource-manager-management-integration.md) artykule
- Aby dowiedzieć się o jak Menedżer zasobów klaster zarządza i sald obciążenia w klastrze, zapoznaj się z artykułem na [równoważenia obciążenia](service-fabric-cluster-resource-manager-balancing.md)
