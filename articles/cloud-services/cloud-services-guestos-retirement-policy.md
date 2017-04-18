<properties 
   pageTitle="Wsparcie i wycofywanie zasady przewodnik dla Azure gościnny system operacyjny | Microsoft Azure" 
   description="Informacje dotyczące co Microsoft udzieli pomocy technicznej w zakresie do system operacyjny gościa Azure używane przez usług w chmurze." 
   services="cloud-services" 
   documentationCenter="na" 
   authors="raiye" 
   manager="timlt" 
   editor=""/>

<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="tbd" 
   ms.date="10/24/2016"
   ms.author="raiye"/>

# <a name="azure-guest-os-supportability-and-retirement-policy"></a>Azure zasad obsługi technicznej i wycofywanie system operacyjny gościa
Informacje na tej stronie odnosi się do systemu operacyjnego gościa Azure ([System operacyjny gościa](cloud-services-guestos-update-matrix.md)) dla pracownika usług w chmurze i role w sieci web (PaaS). Nie ma zastosowania do maszyn wirtualnych (IaaS). 

Firma Microsoft udostępnia opublikowanych [Zasady pomocy technicznej dla systemu operacyjnego gościa](http://support.microsoft.com/gp/azure-cloud-lifecycle-faq). Stronę, którą czytasz teraz w tym artykule opisano sposób implementacji zasady.

Zgodnie z zasadami 

1. Microsoft udzieli pomocy technicznej **co najmniej najnowszych dwóch rodzin system operacyjny gościa**. Kiedy jest wycofana rodziny, klienci mają 12 miesięcy od daty oficjalnym Wycofywanie aktualizacji do nowszego obsługiwane rodziny System operacyjny gościa.
2. Microsoft udzieli pomocy technicznej **co najmniej dwie najnowsze wersje obsługiwanych rodzin gościnny system operacyjny**. 
3. Microsoft udzieli pomocy technicznej w **co najmniej dwie najnowsze wersje SDK Azure**. Kiedy wersji zestawu SDK jest wycofana, klienci będą mieli 12 miesięcy od daty oficjalnym Wycofywanie aktualizacji do nowszej wersji. 

W czasie więcej niż dwiema wersjami lub rodziny mogą być obsługiwane. Oficjalnych informacji pomocy technicznej system operacyjny gościa pojawią się na [wersjach systemu operacyjnego gościa Azure i SDK zgodności macierzy](cloud-services-guestos-update-matrix.md).


## <a name="when-a-guest-os-family-or-version-is-retired"></a>Gdy wycofana rodziny System operacyjny gościa lub wersji 


Nowy system operacyjny gościa **rodziny** wprowadza się kiedyś po wydaniu nowego oficjalnym wersji systemu operacyjnego Windows Server. Gdy zostanie wprowadzona nowa rodzina system operacyjny gościa, Microsoft przestanie być najstarszych rodziny System operacyjny gościa. 

Nowy system operacyjny gościa **wersji** wprowadzono o uwzględniać najnowsze aktualizacje MSRC co miesiąc. Ze względu na zwykłą comiesięcznych aktualizacji wersja systemu operacyjnego gościa jest zwykle wyłączone 60 dni po jego wersji. Dzięki temu co najmniej dwie wersje gościnny system operacyjny dla każdej rodziny dostępne do użycia. 

### <a name="process-during-a-guest-os-family-retirement"></a>Proces podczas emerytury rodziny System operacyjny gościa 


Po wycofywanie została ogłoszona, klienci mają w okresie 12 miesięcy "przejścia" przed rodzinie starsze urzędowo zostanie usunięty z usługi. Ten czas przejścia może być przedłużony według uznania firmy Microsoft. W [wersjach systemu operacyjnego gościa Azure i macierzy zgodności SDK](cloud-services-guestos-update-matrix.md)będą ogłaszane aktualizacje.

Proces stopniowego wycofywanie rozpocznie 6 miesięcy w okresie przejścia. W tym czasie:

1. Firma Microsoft będzie wysyłać klientów wycofywanie. 
2. Nowsza wersja Azure SDK nie obsługuje wycofanych rodziny System operacyjny gościa.
3. Nowe wdrażania i redeployments usług w chmurze nie jest dozwolony w rodzinie wycofanych

Firma Microsoft będzie wprowadzenie nowego wersja systemu operacyjnego gościa zawierających najnowsze aktualizacje MSRC do ostatniego dnia okresu przejścia, nazywane "datę wygaśnięcia". W tym czasie wszelkie usług w chmurze nadal uruchomiony będą obsługiwane w obszarze Azure SLA. Firma Microsoft udostępnia możliwość wymusić uaktualnienie, usuwanie lub zatrzymywanie tych usług po upływie tej daty.



### <a name="process-during-a-guest-os-version-retirement"></a>Proces podczas wycofywanie wersja systemu operacyjnego gościa 
Jeśli klienci ich gościnny system operacyjny, aby automatycznie zaktualizować, ich nie musisz się martwić o sposób postępowania z wersjami systemu operacyjnego gościa. Ta osoba będzie zawsze używać najnowszej wersji systemu operacyjnego gościa.

Wersje systemu operacyjnego gościa wydawanych co miesiąc. Ze względu na stopień zwykła wersjach w każdej wersji ma stały rozpiętość czasu.

W ciągu 60 dni do rozpiętość czasu jest wersja "*wyłączona*". "Wyłączone" oznacza, że usunięcie wersji z portalu klasycznym Azure. Ponadto można już ją ustawić z pliku konfiguracji CSCFG. Istniejące wdrożeń pozostają uruchomione, ale nie jest dozwolony nowych wdrożeń i kod i konfiguracji aktualizacje istniejących wdrożenia. 

Później, system operacyjny gościa wersji "*wygasa*" i wszelkie instalacje są nadal uruchomiony tej wersji życie uaktualnione i ustawiono automatyczne aktualizowanie systemu operacyjnego gościa w przyszłości. Wygasania jest zrobione partiami czas inwalidztwem ważności może być różna. 

Okresy te może się wydłużyć uznania firmy Microsoft w celu zmniejszenia przejścia klienta. Wszelkie zmiany będą przekazywane w [wersjach systemu operacyjnego gościa Azure i SDK zgodności macierzy](cloud-services-guestos-update-matrix.md).



### <a name="notifications-during-retirement"></a>Powiadomienia podczas wycofywanie 

* **Wycofywanie rodziny** <br>Microsoft będą używane wpisów w blogu i Azure klasyczny powiadomienie portalu. Klienci, którzy nadal korzystasz z wycofanych rodziny System operacyjny gościa zostaną powiadomieni za pośrednictwem bezpośredniej komunikacji (wiadomości e-mail, wiadomości portalu, rozmowy telefonicznej) dla administratorów usługi przydzielone. Wszystkie zmiany zostaną opublikowane do tej strony i źródło danych RSS wymienione na początku tej strony. 


* **Wycofywanie wersji** <br>Wszystkie zmiany zostaną opublikowane do tej strony i źródło danych RSS wymienione na początku strony, w tym wersji, wyłączona i daty wygaśnięcia. Administratorzy usługi będziesz otrzymywać wiadomości e-mail, jeśli mają wdrożeń uruchomionych wyłączone wersja systemu operacyjnego gościa i rodziny. Czasem te wiadomości e-mail może być różna. Zazwyczaj są co najmniej miesiąc przed inwalidztwo, mimo to chronometraż nie oficjalnym SLA. 


## <a name="frequently-asked-questions"></a>Często zadawane pytania

**Jak można ograniczyć wpływ migracji?**

Należy użyć najnowszej rodziny System operacyjny gościa do projektowania usług w chmurze. 

1. Rozpocząć planowanie migracji do nowszego rodziny wcześniej. 
2. Konfigurowanie wdrożeń test tymczasowe do testowania usługi Cloud uruchomionych Nowa rodzina. 
3. Ustaw swoją wersję systemu operacyjnego gościa na **Automatyczne** (element osVersion = * w pliku [.cscfg](cloud-services-model-and-package.md#cscfg) ) w celu migracji do nowej wersji systemu operacyjnego gościa tworzony automatycznie.

**Co zrobić, jeśli Moja aplikacja sieci web wymaga lepsza integracja z systemem operacyjnym?**

Architektura aplikacji sieci web wymaga szczegółowego zależność system operacyjny, użyj platformy obsługiwane funkcje, takie jak [uruchamiania zadań](cloud-services-startup-tasks.md) lub inne mechanizmy rozszerzeń, które mogą istnieć w przyszłości. Ponadto można [Azure maszyn wirtualnych](https://azure.microsoft.com/documentation/scenarios/virtual-machines/) (IaaS — infrastruktury jako usługa), gdzie użytkownik jest odpowiedzialny za utrzymywania system operacyjny.
 
## <a name="next-steps"></a>Następne kroki
Przejrzyj najnowsze [wydanie system operacyjny gościa](cloud-services-guestos-update-matrix.md).
