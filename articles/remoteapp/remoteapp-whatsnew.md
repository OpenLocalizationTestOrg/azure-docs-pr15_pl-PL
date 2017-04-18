
<properties
    pageTitle="Co nowego w Azure RemoteApp? | Microsoft Azure"
    description="Więcej informacji na temat zmiany i ulepszenia wprowadzone Azure RemoteApp"
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />



# <a name="whats-new-in-azure-remoteapp"></a>Co nowego w Azure RemoteApp?

> [AZURE.IMPORTANT]
> Azure RemoteApp jest są przerywane. W tym artykule [anons](https://go.microsoft.com/fwlink/?linkid=821148) , aby uzyskać szczegółowe informacje.

Jedną z zalet Azure RemoteApp to, że zawsze chcemy go ulepszyć. Za każdym razem, gdy czynności, będziemy informować o tych zmian w tym miejscu.

## <a name="future-updates"></a>Przyszłych aktualizacji
Hej - czy wiesz, że zespołu Azure RemoteApp wpisów comiesięczna aktualizacja do blogu licencji? Można znaleźć, nie tylko co to jest zmiana Azure RemoteApp, ale także inne informacje o używaniu RDS. Zapoznaj się z ich blogu, [Blog usług pulpitu zdalnego](https://blogs.msdn.microsoft.com/rds/), aby uzyskać informacje. Na przykład kilka tygodni temu, są publikowane wpisu o [Podnoszenie i przesuwanie obciążenie pracą z Azure RemoteApp i Azure AD](https://blogs.msdn.microsoft.com/rds/2016/01/19/lift-and-shift-your-workloads-with-azure-remoteapp-and-azure-ad-domain-services/).
 
## <a name="september-2015"></a>Września 2015 r.
- Dodano Infopath obraz szablonu i Galeria usługi Microsoft Office 365. Jeśli chcesz udostępnić programu Infopath, upewnij się zaktualizować kolekcji z obrazem najnowszą.
- Aktualizacje klienta:
    - Windows klient zaktualizowane, aby umożliwić użytkownikom udostępnianie opinii, szczególnie wokół problemy z połączeniem.
    - Klient iOS zaktualizowane, aby rozwiązać problem komunikatów o błędach i rozwiązania problemu, gdzie poświadczenia wygasła wcześniej niż oczekiwano.
- Pracujemy nad uzyskiwanie pomocy technicznej dla pakietu Office 2016 sprawdzany. Po zakończeniu tego poszukaj zaktualizowanych obrazów.
- Opublikowany nowego artykułu o [różnice między zbiorami chmury i hybrydowego](remoteapp-collections.md) — dzięki temu wybierz typ kolekcji, który najlepiej aplikacje — tylko do chmury, chmury + VNET lub hybrydowych.
- Chcesz udostępnić QuickBooks przy użyciu Azure RemoteApp, ale nie masz pewności, kroków? Zapoznaj się z [jego marek nowy artykuł](remoteapp-quickbooks.md) informujący dokładnie co należy zrobić.

## <a name="august-2015"></a>Sierpnia 2015 r.
Duży zmiany się stało z sierpnia — Oto wyróżnienia:

- Teraz możesz używać VNET Azure z zbioru chmury! Zapoznaj się z [instrukcjami tworzenia chmury](remoteapp-create-cloud-deployment.md) czynności tworzenia nowego.
- Możliwe dodawanie aplikacji do **Start **menu dla klienta RemoteApp systemu Windows. Aplikacje będą widoczne na liście aplikacji, a przypiąć je do menu **Start **w systemie Windows.
- Dodany nowy obraz do galerii maszyn wirtualnych Azure - Windows Server hosta sesji pulpitu zdalnego z usługi Microsoft Office 365 ProPlus.
- O stałej klienta Mac, więc aplikacji z systemem windows modalne zatrzyma, blokując.
- Opisano, jak za pomocą [subskrypcji usługi Office 365 ProPlus](remoteapp-officesubscription.md) Azure RemoteApp.
- Szczegółowe, jak można [zabezpieczyć te aplikacje i dane](remoteapp-secure.md) w zbiorze Azure RemoteApp.

## <a name="july-2015"></a>Lipca 2015 r.

Lipiec Ustaw etapu zmiany wkrótce sierpnia, dlatego nie ma wiele się teraz, głównie aktualizacji dokumentu. Oto najnowsze zmiany:

- Dodane do portalu karty **pomocy technicznej** , aby łatwiej dostęp do zasobów pomocy technicznej, takich jak forach.
- Ponownie opracowane informacji umożliwiających rozwiązywanie problemów dotyczących tworzenia zbioru hybrydowych. Zapoznaj się z [najnowszych i największe](remoteapp-hybridtrouble.md) dla porady dotyczące takich jak rozpoznawania porty należy skonfigurować w swojej VNET rozwiązywania problemów.
- Opisano, jak utworzone i zapisane w Azure RemoteApp [danych użytkownika](remoteapp-upd.md) .
- Opisano sposób [blokowania aplikacji](remoteapp-secure.md).
- Opublikowanych [Azure RemoteApp poleceń cmdlet](https://msdn.microsoft.com/library/mt428031.aspx).
- A na koniec konwersacji możemy pracę z niektórych użytkowników Azure RemoteApp dotyczących terminologii. Poszukaj zmian w odpowiedni sposób możemy odwołują się do różnych opcji.

## <a name="june-2015"></a>Czerwca 2015 r.

Zmiany tak dużo! Zespół został bardzo zajęty, w czerwcu:

- Przeprojektowana Azure RemoteApp [Strona początkowa](https://www.remoteapp.windowsazure.com/) - wyewidencjonowywać!
- Aktualizacji wszystkich obrazów w ramach subskrypcji.
- Ulepszona w celu kolekcji hybrydowe, w tym wymuszonego tunelowania pomocy technicznej i sprawdzanie rozmiaru podsieć adresów IP przed przystąpieniem do tworzenia kolekcji.
- Wykryte, który * symboli wieloznacznych nie działa w przypadku kamer internetowych. Zamiast tego musisz określić Identyfikatora wystąpienia lub identyfikator GUID. Firma Microsoft będzie aktualizowanie informacji o przekazywaniu tak, aby odzwierciedlała, który.
- Wprowadzone go, dzięki czemu można dodać niestandardowe oprogramowanie antywirusowe obrazu podczas tworzenia obraz szablonu z galerii Azure.

Mamy więcej zmian wdrażania w lipca, więc możemy będzie wkrótce z innej aktualizacji.

## <a name="may-2015"></a>Maj 2015 r.

Są dostępne liczby dodatków (i miesięcy) od możemy utworzony w tym temacie, aby liście cheats nieco i pochodzi od początku marca za pośrednictwem maja. Zapoznaj się z tych nowych funkcjach:

- Automatyzowanie wszystko — Azure RemoteApp zawiera teraz [poleceń cmdlet w module Azure programu PowerShell](remoteapp-tutorial-arawithpowershell.md).
- [Utwórz obraz Azure RemoteApp z Azure maszyn wirtualnych](remoteapp-image-on-azurevm.md). Umożliwia przekazywanie obraz niestandardowy Azure znacznie szybsze.
- Nawiązywanie połączenia z zasobów w sieci firmowej Azure za pomocą VNET Azure zamiast RemoteApp VNET. Firma Microsoft został zaktualizowany z [hybrydowego zbioru instrukcje](remoteapp-create-hybrid-deployment.md) , aby przeprowadził Cię przez proces tworzenia VNET Azure (jest to krok 1).
- Czytanie z VNETs, zapoznaj się z [nowych wytycznych](remoteapp-vnetsizing.md) wokół ograniczenia i limity rozmiarów VNET.
- I czytanie limitów — po prostu co to jest [Usługa limity i ustawieniami domyślnymi](../azure-subscription-service-limits.md)?

Chcesz dowiedzieć się więcej na temat Azure RemoteApp? Zespół RemoteApp był się obowiązujących w Ignite sprzed kilka tygodni. Obejrzyj klip wideo na marek, [podstawy Microsoft Azure RemoteApp zarządzania i administracji](http://channel9.msdn.com/Events/Ignite/2015/BRK3868).

Potrzebujesz wyświetlić RemoteApp Azure rzeczywiste? Zapoznaj się z samouczka [Uruchom dowolną aplikację na dowolnym urządzeniu w dowolnym miejscu](remoteapp-anyapp.md) — go pokazano, jak korzystać z użytkowników, w tym udostępniania plików bazy danych. Ponadto mamy samouczek na [Tworzenie usługi Office 365](remoteapp-tutorial-o365anywhere.md) Uruchom tak samo na dowolnym urządzeniu.

Dziękujemy za Wbijanie z nami — wstecz następny miesiąc za pomocą więcej aktualizacji.


### <a name="help-us-help-you"></a>Pomóż nam ułatwiają
Czy wiesz, oprócz ocena niniejszego artykułu i wyrażania opinii w dół poniżej, umożliwia zmiany do tego samego artykułu? Coś Brak? Problem? Pisanie jest coś, co jest po prostu mylące czy? Przewijanie w górę, a następnie kliknij przycisk **Edytuj na GitHub** , aby wprowadzić zmiany — te ułatwiających kontakt z nami do recenzji, a następnie po możemy zalogować się nad nimi, zostanie wyświetlony do zmiany i ulepszenia w tym artykule.
