<properties
   pageTitle="Najważniejsze wskazówki dotyczące aktualizacji oprogramowania na IaaS platformy Microsoft Azure | Microsoft Azure"
   description="Artykuł stanowi kolekcję najważniejsze wskazówki dotyczące aktualizacji oprogramowania w środowisku Microsoft Azure IaaS.  Jest przeznaczona dla informatyków i analityków zabezpieczeń, którzy zajęcie się zmienić, aktualizacji i środków trwałych zarządzania oprogramowaniem codziennie, łącznie z tymi, które są odpowiedzialne za prac nad zabezpieczenia i zgodność w ich organizacji."
   services="security"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/18/2016"
   ms.author="yurid"/>

# <a name="best-practices-for-software-updates-on-microsoft-azure-iaas"></a>Najważniejsze wskazówki dotyczące aktualizacji oprogramowania na Microsoft Azure IaaS

Przed nurkowaniu do dowolnego rodzaju dyskusji na najważniejsze wskazówki dotyczące środowiska usługi Azure [IaaS](https://azure.microsoft.com/overview/what-is-iaas/) , należy scenariusze znaczeniu że będą mieć możesz zarządzania aktualizacje oprogramowania i obowiązki. Na poniższym diagramie powinien ułatwić zrozumienie te ograniczenia:

![Modele chmury i zobowiązań](./media/azure-security-best-practices-software-updates-iaas/sec-cloudstack-new.png)

Lewej kolumny zawiera siedem obowiązki (określonej w sekcjach) czy organizacji należy rozważyć, które współtworzyć bezpieczeństwa i prywatności środowiska komputerowego.
 
Dane i odpowiedzialności i ochronę klienta i końcowy obowiązki, które są wyłącznie w domenie klientów i obowiązki fizycznej, hosta i sieci jest w domenie dostawców usług w chmurze w modelach PaaS i władz akredytacji bezpieczeństwa. 

Pozostałe obowiązki są udostępniane między klientami i dostawców usług w chmurze. Niektóre obowiązki wymagają dostawcy i obsługi klientów administrowanie odpowiedzialność razem, w tym inspekcja swoich domen i zarządzanie nimi. Na przykład należy rozważyć, czy tożsamości i uzyskać dostęp do zarządzania przy użyciu Azure usługi Active Directory; Konfiguracja usług, takich jak uwierzytelnianie wieloskładnikowe zależy od klienta, ale zapewnia funkcjonalność skutecznych spoczywa Microsoft Azure.

> [AZURE.NOTE] Aby uzyskać więcej informacji na temat udostępnionych obowiązki w chmurze przeczytaj [Udostępnionego obowiązki dotyczące środowiska chmury](https://gallery.technet.microsoft.com/Shared-Responsibilities-81d0ff91/file/153019/1/Shared%20responsibilities%20for%20cloud%20computing.pdf) 

Te same zasady mają zastosowanie w scenariuszu hybrydowych miejsce, w którym firmy korzysta z maszyny wirtualne IaaS Azure, które komunikować się z zasobami w lokalnej, jak pokazano na poniższej ilustracji.

![Hybrydowe typowy scenariusz Microsoft Azure](./media/azure-security-best-practices-software-updates-iaas/sec-azconnectonpre.png)

## <a name="initial-assessment"></a>Ocena wstępna

Nawet jeśli firmy korzysta już z systemu zarządzania aktualizacji i masz już zasady aktualizacji oprogramowania w miejscu, należy często Kontroluj poprzedniego ocen zasad i zaktualizuj je zgodnie z własnymi potrzebami bieżącego. Oznacza to, że należy zapoznać się z bieżącym stanie zasobów w firmie. Aby uzyskać dostęp do tego stanu, konieczna jest znajomość:

-   Fizycznych i wirtualnych komputerach w przedsiębiorstwie.

-   Systemy operacyjne i wersje uruchomionych dla każdej z tych komputerów fizycznych i wirtualnych.

-   Aktualizacje oprogramowania został zainstalowany na każdym komputerze (wersje dodatków service pack, aktualizacje oprogramowania i inne zmiany).

-   Funkcja wykonuje każdy komputer w przedsiębiorstwie.

-   Aplikacje i programy uruchomione na każdym komputerze.

-   Własności i informacje kontaktowe dla każdego komputera.

-   Składniki majątku znajdujących się w środowiska i ich względną wartością stwierdzić, które obszary wymagane najbardziej uwagę i ochronę.

-   Zabezpieczenia znanych problemów oraz procesy przedsiębiorstwie występują w celu identyfikacyjnym nowe problemy zabezpieczeń lub zmian w poziomie.

-   Przeciwdziałania wdrożonych zabezpieczania środowiska.

Należy regularnie aktualizować te informacje, a powinny być łatwo dostępne dla biorące udział w procesie zarządzania aktualizacji oprogramowania.

## <a name="establish-a-baseline"></a>Ustanowienie linii bazowej

Ważne części procesu zarządzania aktualizacji oprogramowania jest tworzenie początkowej instalacji standardowej wersji systemu operacyjnego, aplikacji i sprzętu dla komputerów w przedsiębiorstwie; te są nazywane plany bazowe. Plan bazowy jest konfiguracji produktu lub systemu ustanowioną w określonym punkcie w czasie. Aplikacja lub system operacyjny według planu bazowego, na przykład umożliwia odbudowanie komputera lub usługi do określonego stanu.

Plany bazowe stanowić podstawę Znajdowanie i naprawianie potencjalne problemy i uprościć proces zarządzania aktualizacji oprogramowania, ograniczając liczbę aktualizacje oprogramowania, które należy wdrożyć w organizacji jak, zwiększając możliwości kontrolowania zgodności.

Po wykonaniu początkowej inspekcji przedsiębiorstwa, należy użyć informacji, które są uzyskiwane z inspekcji, aby zdefiniować operacyjne według planu bazowego dla składników IT w środowisku produkcyjnym. Wiele planów bazowych mogą być wymagane, w zależności od różnych rodzajów sprzętu i oprogramowania wdrożony w produkcji.

Na przykład niektóre serwery wymagają aktualizacji oprogramowania, aby uniemożliwić wysunięciem po wprowadzeniu proces zamykania podczas uruchamiania systemu Windows Server 2012. Plan bazowy tych serwerach powinien zawierać tej aktualizacji oprogramowania.

W dużych organizacjach warto często dzielony komputerach w przedsiębiorstwie kategorie składników majątku i utrzymywać każdej kategorii w standardowej według planu bazowego przy użyciu tej samej wersji oprogramowania i aktualizacji oprogramowania. Następnie można użyć tych kategorii zawartości w priorytetach rozpowszechniania aktualizacji oprogramowania.

## <a name="subscribe-to-the-appropriate-software-update-notification-services"></a>Subskrybowanie odpowiednie oprogramowanie usługi powiadomień aktualizacji

Po wykonaniu początkowej inspekcji oprogramowania używany w przedsiębiorstwie, należy określić właściwą metodę przeprowadzania otrzymuje powiadomienia o nowych aktualizacji oprogramowania dla każdego oprogramowania i wersji. W zależności od oprogramowania najlepszej metody powiadomienie może być powiadomienia pocztą e-mail, witryny sieci Web lub komputerze publikacji.

Na przykład Centrum odpowiedzi zabezpieczeń firmy Microsoft (gdy) odpowiada wszystkie problemy związane z zabezpieczeniami o produktach firmy Microsoft, a także usługi biuletyn zabezpieczeń firmy Microsoft, bezpłatne pocztą e-mail powiadomienia o nowo zidentyfikowanych luk i aktualizacje oprogramowania, które są opublikowana w celu rozwiązania tych luk. Możesz subskrybować tych usług http://www.microsoft.com/technet/security/bulletin/notify.mspx.

## <a name="software-update-considerations"></a>Zagadnienia dotyczące aktualizacji oprogramowania

Po wykonaniu początkowej inspekcji oprogramowania używany w przedsiębiorstwie, należy sprawdzić wymagania dotyczące konfiguracji systemu zarządzania aktualizacji oprogramowania, która zależy od używanego systemu zarządzania aktualizacji oprogramowania. Centrum systemu WSUS przeczytaj [Najważniejsze wskazówki z systemem Windows Server Update Services](https://technet.microsoft.com/library/Cc708536), odczytywanie [Planowanie aktualizacji oprogramowania w Menedżerze konfiguracji](https://technet.microsoft.com/library/gg712696).

Istnieją jednak, niektóre ogólne i najlepsze rozwiązania, które można stosować bez względu na to rozwiązanie, które używasz, jak pokazano w sekcjach znajdujący się.

### <a name="setting-up-the-environment"></a>Konfigurowanie środowiska

Należy rozważyć następujące wskazówki podczas planowania konfiguracji oprogramowania aktualizacji środowiska zarządzania:

-   **Utwórz produkcji oprogramowania aktualizacji zbiory w oparciu o kryteria stabilny**: na ogół tworzenie zbiorów spisu aktualizacji oprogramowania i dystrybucji za pomocą kryteriów stabilny może pomóc w celu uproszczenia wszystkie etapy procesu zarządzania aktualizacji oprogramowania. Stałe kryteria mogą być, wersja systemu operacyjnego zainstalowanego klienta i poziom dodatku service pack, rola systemu lub organizacji docelowej.

-   **Utwórz zbiory przygotowań obejmujące komputery odniesienia**: zbioru przygotowań powinien zawierać przedstawiciela konfiguracji wersji systemu operacyjnego, linia oprogramowania firm i inne oprogramowanie w przedsiębiorstwie.

Należy również rozważyć, gdzie serwera aktualizacji oprogramowania będą umieszczane, jeśli będzie ona w infrastrukturze Azure IaaS w chmurze lub będzie lokalnego. Jest to ważne, ponieważ potrzebne do oceny natężenia ruchu między zasobów lokalnych i infrastruktury Azure. Aby uzyskać więcej informacji na temat sposobu łączenia infrastruktury lokalnego Azure, przeczytaj [Łączenie sieci lokalnej wirtualną sieć Microsoft Azure](https://technet.microsoft.com/library/Dn786406.aspx) .

Opcje projektu, określających miejsce, w którym będą umieszczane na serwerze aktualizacji również zależy od bieżącego infrastruktury i aktualnie używanego systemu aktualizacji oprogramowania. WSUS odczytywanie [Wdrażanie Windows Server Update Services w organizacji](https://technet.microsoft.com/library/hh852340.aspx) oraz dla Menedżera konfiguracji centrum systemu Czytaj [Planowanie witryn i hierarchie w Menedżerze konfiguracji](https://technet.microsoft.com/library/Gg712681.aspx).

### <a name="backup"></a>Wykonywanie kopii zapasowych

Regularnego wykonywania kopii zapasowych są ważne nie tylko dla oprogramowania aktualizacji zarządzania platformy samej ale również dla serwerów, które zostaną zaktualizowane. Wymaga organizacji, które [zmienić procesu zarządzania](https://technet.microsoft.com/library/cc543216.aspx) w miejscu IT w celu Justowanie powodów, dlaczego serwer wymaga aktualizacji, szacowany przestoje i wpływu. Aby upewnić się, że masz wycofywania konfiguracji w miejscu w przypadku aktualizacji nie powiedzie się, upewnij się, że regularnie tworzyć kopie zapasowe systemu.

Niektóre opcje tworzenia kopii zapasowych dla Azure IaaS obejmują:

-   [Azure ochrony obciążenie pracą IaaS za pomocą Menedżera ochrona danych](https://azure.microsoft.com/blog/2014/09/08/azure-iaas-workload-protection-using-data-protection-manager/)

-   [Wykonywanie kopii zapasowej Azure maszyn wirtualnych](../backup/backup-azure-vms.md)

### <a name="monitoring"></a>Monitorowanie

Należy uruchamiać raporty regularne monitorowanie liczba brakujących lub zainstalowane aktualizacje lub aktualizacje o stanie niepełne dla każdej aktualizacji oprogramowania, który jest autoryzowany. Podobnie raportowania aktualizacji oprogramowania, które nie są jeszcze autoryzowanych może ułatwić łatwiejsze decyzje wdrożenia.

Rozważ też następujące zadania:

-   Przeprowadzanie inspekcji dotyczy i zainstalowanych aktualizacji dla wszystkich komputerów w firmie.

-   Autoryzuj i wdrażanie aktualizacje do odpowiednich komputerów.

-   Śledzenie zapasów i zaktualizuj stanu instalacji oraz informacje o postępie wszystkich komputerów w firmie.

Ćwiczenia dodatkowo do Ogólne zagadnienia, które zostały wyjaśnione w tym artykule, Rozważ też każdego produktu jest zalecane, na przykład: Jeśli masz maszyny platformy Azure z programem SQL Server, upewnij się, po rekomendacji aktualizacje oprogramowania dla tego produktu.

## <a name="next-steps"></a>Następne kroki

Ułatwiający określanie opcji najważniejsze aktualizacje oprogramowania dla maszyn wirtualnych Azure IaaS za pomocą wytycznych opisanych w tym artykule. Istnieje wiele podobieństwa między aktualizacji oprogramowania najlepsze rozwiązania w tradycyjnych centrum danych, a Azure IaaS, dlatego lepiej, warto dokonać przeglądu bieżącego zasad dotyczących aktualizacji oprogramowania, aby uwzględnić maszyny wirtualne Azure i dołączyć odpowiednie najlepsze rozwiązania z tego artykułu ogólnego procesu aktualizacji oprogramowania.
