<properties
    pageTitle="Najważniejsze wskazówki dotyczące Azure aplikacji usługi"
    description="Dowiedz się, najważniejsze wskazówki i rozwiązywania problemów dla usługi aplikacji Azure."
    services="app-service"
    documentationCenter=""
    authors="dariagrigoriu"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/30/2016"
    ms.author="dariagrigoriu"/>
    
# <a name="best-practices-for-azure-app-service"></a>Najważniejsze wskazówki dotyczące Azure aplikacji usługi

W tym artykule przedstawiono najważniejsze wskazówki dotyczące korzystania z [Platformy Azure aplikacji usługi](http://go.microsoft.com/fwlink/?LinkId=529714). 

## <a name="colocation"></a>Wspólna lokalizacja
Gdy zasoby Azure redagowania rozwiązanie, takich jak aplikacji sieci web i bazy danych znajdują się w różnych regionów efekty mogą być następujące:

*  Zwiększenie opóźnienia w komunikacji między zasoby
*  Pieniężnych opłaty dla ruchu wychodzącego danych przeniesienie region krzyżowe zaznaczono na [Azure strony cennik](https://azure.microsoft.com/pricing/details/data-transfers).

Wspólna lokalizacja w tym samym regionie jest zalecane dla zasobów Azure redagowania rozwiązanie, takich jak aplikacji sieci web i konto bazy danych lub miejsce do magazynowania służy do przechowywania danych. Podczas tworzenia zasobów, które należy upewnić się znajdują się w tym samym regionie Azure chyba że masz firmy lub projektowanie powód ich nie należy. Możesz przenieść aplikację aplikacji usługi do tego samego regionu, co bazę danych, korzystając z [aplikacji usługi klonowanie funkcji](app-service-web-app-cloning-portal.md) dostępnych w przypadku aplikacji Premium aplikacji usługi Planowanie.   

## <a name="memoryresources"></a>Gdy aplikacje zajmują więcej pamięci niż oczekiwano
Jeśli okaże się, więcej pamięci niż przewidywano wskazywane przez monitorowanie korzystanie z aplikacji lub rekomendacje usług należy rozważyć, czy [Funkcja korygujący automatyczne usługi aplikacji](https://azure.microsoft.com/blog/auto-healing-windows-azure-web-sites). Jedną z opcji dla funkcji korygujący automatyczne trwa akcji niestandardowej w oparciu o próg pamięci. Akcje obejmować zakres subskrypcję powiadomień e-mail postępowania za pośrednictwem zrzut pamięci, aby od razu łagodzenia odtwarzając proces roboczy. Korygujący automatyczne można skonfigurować za pośrednictwem web.config i za pośrednictwem interfejsu użytkownika przyjaznego zgodnie z opisem w tym we wpisie w blogu dla [Aplikacji usługi pomocy technicznej witryny rozszerzenia](https://azure.microsoft.com/blog/additional-updates-to-support-site-extension-for-azure-app-service-web-apps).   

## <a name="CPUresources"></a>Gdy aplikacje używają Procesor więcej niż oczekiwano
Jeśli widać, że Procesora więcej niż oczekiwano lub napotkania korzystanie z aplikacji powtarzane Procesora tych najwyższych wartościach wskazywane przez monitorowanie lub rekomendacje usług rozważ skalowanie w górę lub skalowania planu aplikacji usługi. Jeśli aplikacja jest statefull, Skalowanie wewnętrzne jest to jedyna opcja, a jeśli aplikacja jest w nowym oknie bezstanowym, skalowania umożliwi większą elastyczność i nowszy potencjał skali. 

Aby uzyskać więcej informacji na temat aplikacji "bezstanowych" a "statefull" możesz obejrzeć ten klip wideo: [Planowanie aplikację wielu skalowalna zakończenia do końca w programie Microsoft Azure Web App](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/DEV-B414#fbid=?hashlink=fbid). Aby uzyskać więcej informacji na temat opcji skalowania i autoscaling aplikacji usługi przeczytaj: [Skala aplikacji sieci Web w usłudze Azure aplikacji](web-sites-scale.md).  

## <a name="socketresources"></a>Po wyczerpaniu socket zasobów
Typowe przyczyny podłączonych połączenia wychodzące TCP jest używanie bibliotek klienta, które nie są wykonywane ponowne połączeń lub w przypadku poziomu protokół wyższej takich jak HTTP - aktywności nie osobie. Przejrzyj dokumentację dla każdej biblioteki odwołuje się aplikacji w swojej aplikacji usługi Planowanie upewnij się, są one skonfigurowane lub dostępne w kodzie wydajność ponownego użycia połączeń wychodzących. Również Wykonaj wskazówki dokumentacji biblioteki dotyczące tworzenia pisane z wielkiej litery i udostępniania lub oczyszczanie, aby uniknąć przeciek połączenia. W czasie dochodzenia bibliotek takie klienta wpływ postępu może być ograniczona przez Skalowanie zewnętrzne do wielu wystąpień.  

## <a name="appbackup"></a>Gdy aplikacji Kopia zapasowa rozpoczyna się niepowodzeniem
Dwie najbardziej typowe przyczyny, dlaczego są aplikacji kopii zapasowej kończy się niepowodzeniem: nieprawidłowe miejsca do magazynowania i konfiguracji nieprawidłowej bazy danych. Te błędy zwykle wystąpić, gdy są zmiany w zasobach miejsca do magazynowania lub bazy danych lub zmiany dotyczące dostęp do tych zasobów (np. poświadczeń aktualizacji dla wybranej w ustawieniach kopii zapasowej bazy danych). Wykonywanie kopii zapasowych zazwyczaj Uruchom zgodnie z harmonogramem i wymagają dostępu do magazynowania (w przypadku wyprowadzania kopie zapasowe plików) i baz danych (kopiowania i odczytu zawartości, które mają zostać uwzględnione w kopii zapasowej). Nie można uzyskać dostęp do jednej z następujących zasobów wynik będzie spójne niepowodzeniu wykonywania kopii zapasowej. 

Gdy wystąpić błędy kopii zapasowej, Przejrzyj najnowsze wyniki, aby dowiedzieć się, jakiego typu błąd się dzieje. W przypadku błędy dostępu miejsca do magazynowania Przejrzyj i zaktualizuj ustawienia dotyczące miejsca do magazynowania w kopii zapasowej konfiguracji. W przypadku niepowodzenia dostępu do bazy danych należy przejrzeć i zaktualizować ciągów do połączenia w ustawieniach aplikacji; Przejdź do aktualizacji konfiguracja kopii zapasowej, aby poprawnie uwzględnić wymagane bazy danych. Więcej informacji na temat aplikacji Kopia zapasowa Zobacz dokumentację [Wykonaj kopię zapasową aplikacji sieci web w usłudze Azure aplikacji](web-sites-backup.md) .

## <a name="nodejs"></a>Gdy nowe aplikacje Node.js są rozmieszczane Azure aplikacji usługi
Azure konfiguracji domyślnej aplikacji usługi w przypadku aplikacji Node.js ma najlepiej potrzeb najczęściej używanych aplikacji. Jeśli Konfiguracja aplikacji Node.js chcesz skorzystać z spersonalizowanych Dostosowywanie zwiększenie wydajności i zoptymalizować użycie zasobów dla zasobów Procesora i pamięci i sieci, może przejrzeć swoje najważniejsze wskazówki i procedurę rozwiązywania problemów. W tym artykule dokumentacji opisano ustawienia iisnode, może być konieczne skonfigurowanie dla aplikacji Node.js, w tym artykule opisano różne scenariusze lub problemów aplikacji może być przeciwległych i przedstawiono sposoby rozwiązania tych problemów: [Najważniejsze wskazówki i rozwiązywania problemów — przewodnik dotyczący aplikacji węzeł na Azure aplikacji usługi](app-service-web-nodejs-best-practices-and-troubleshoot-guide.md).   


