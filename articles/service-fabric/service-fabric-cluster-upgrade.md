<properties
   pageTitle="Uaktualnianie klaster tkaninie usługi Azure | Microsoft Azure"
   description="Uaktualnianie kodu tkaninie usługi i/lub konfiguracji, która jest uruchomiony klaster tkaninie usługi, włącznie z ustawieniem tryb aktualizacji klaster uaktualniania certyfikatów, dodawanie portów aplikacji, wykonując poprawki systemu operacyjnego, i tak dalej. Czego można oczekiwać po odbywa się uaktualnień?"
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/10/2016"
   ms.author="chackdan"/>


# <a name="upgrade-an-azure-service-fabric-cluster"></a>Uaktualnianie klaster tkaninie usługi Azure

> [AZURE.SELECTOR]
- [Klaster Azure](service-fabric-cluster-upgrade.md)
- [Klaster autonomicznego](service-fabric-cluster-upgrade-windows-server.md)

Dla każdego nowoczesnego systemu projektowanie na możliwość jest kluczem do osiągnięcia sukcesu długoterminowe produktu. Klaster tkaninie usługi Azure to zasób właścicielem, ale odbywa się częściowo przez firmę Microsoft. W tym artykule opisano, co odbywa się automatycznie i ręcznie skonfigurować.

## <a name="controlling-the-fabric-version-that-runs-on-your-cluster"></a>Sterowanie wersji tkaninie uruchamianej na klaster

Możesz skonfigurować klaster uaktualnienia tkaninie automatyczne otrzymywanie firma Microsoft udostępnia nową wersję lub wybierz pozycję, aby wybrać wersję obsługiwane tkaninie potrzebne klaster w.

W tym celu ustawiania konfiguracji klaster "upgradeMode" w portalu lub przy użyciu Menedżera zasobów w momencie tworzenia lub nowszy na żywo klaster 

>[AZURE.NOTE] Upewnij się zachować klaster zawsze wersja obsługiwane tkaninie. Jak i firma Microsoft o nowej wersji materiału usługi, poprzedniej wersji jest oznaczony w celu obsługi po co najmniej 60 dni od tej daty. nowe wersje są wprowadzona [w blogu zespołu tkaninie usługi](https://blogs.msdn.microsoft.com/azureservicefabric/ ). Następnie wybierz znajduje się nowej wersji. 

14 dni przed upływem wersji, którą klaster jest uruchomiony, zdarzenie kondycji jest generowane wstawiana klaster do stanu kondycji ostrzeżenie. Klaster pozostaje w stanie ostrzeżenia, dopóki uaktualnianie do wersji obsługiwanych tkaninie.


### <a name="setting-the-upgrade-mode-via-portal"></a>Ustawianie trybu uaktualniania za pośrednictwem portalu 

Można ustawić klaster automatyczne lub ręczne podczas tworzenia klaster.

![Create_Manualmode][Create_Manualmode]

Można ustawić klaster automatyczne lub ręczne na żywo klaster korzystasz ze środowiska zarządzanie. 

#### <a name="upgrading-to-a-new-version-on-a-cluster-that-is-set-to-manual-mode-via-portal"></a>Uaktualnienie do nowej wersji w klastrze jest ustawiony tryb ręcznego za pośrednictwem portalu.
 
Aby uaktualnić do nowej wersji, wszystko, co należy zrobić jest wybierz z menu rozwijanego dostępnej wersji i zapisywanie. Uaktualnianie tkaninie otrzymuje kopać automatycznie. Zasady dotyczące kondycji klaster (kombinacja węzeł zdrowia i kondycji wszystkie aplikacje działające w klastrze) przestrzegać podczas uaktualniania.

Jeśli zasady dotyczące kondycji klaster nie są spełnione, uaktualnianie przywróceniu. Przewiń w dół ten dokument, aby przeczytać więcej o ustawianiu tych zasad niestandardowych kondycji. 

Po ustaleniu problemy, które spowodowały wycofywania, musisz zainicjować aktualizację ponownie, wykonując te same kroki jako przed.

![Manage_Automaticmode][Manage_Automaticmode]

### <a name="setting-the-upgrade-mode-via-a-resource-manager-template"></a>Ustawianie trybu uaktualniania za pomocą szablonu Menedżera zasobów 

Dodaj konfiguracji "upgradeMode" do definicji zasobów Microsoft.ServiceFabric/clusters i ustawiono "clusterCodeVersion" wersje obsługiwanych tkaninie, tak jak pokazano poniżej, a następnie wdrożyć szablonu. Są prawidłowe wartości "upgradeMode", "Ręczny" lub "Automatyczny"
 
![ARMUpgradeMode][ARMUpgradeMode]

#### <a name="upgrading-to-a-new-version-on-a-cluster-that-is-set-to-manual-mode-via-a-resource-manager-template"></a>Uaktualnienie do nowej wersji w klastrze jest ustawiony tryb ręcznego za pomocą szablonu Menedżera zasobów.
 
Gdy klaster jest w trybie ręcznym, przeprowadzić uaktualnienie do nowej wersji, zmienianie "clusterCodeVersion" do obsługiwanej wersji i wdrażanie go. Wdrażanie szablonu kopnięć uaktualnienia tkaninie otrzymuje kopać automatycznie. Zasady dotyczące kondycji klaster (kombinacja węzeł zdrowia i kondycji wszystkie aplikacje działające w klastrze) przestrzegać podczas uaktualniania.

Jeśli zasady dotyczące kondycji klaster nie są spełnione, uaktualnianie przywróceniu. Przewiń w dół ten dokument, aby przeczytać więcej o ustawianiu tych zasad niestandardowych kondycji. 

Po ustaleniu problemy, które spowodowały wycofywania, musisz zainicjować aktualizację ponownie, wykonując te same kroki jako przed.

### <a name="get-list-of-all-available-version-for-all-environments-for-a-given-subscription"></a>Pobierz listę wszystkich wersji dostępne we wszystkich środowiskach dla danej subskrypcji

Uruchom następujące polecenie, a otrzymasz wynik podobny do tego.

"supportExpiryUtc" zawiera informację, kiedy usługi danej wersji wygasa lub wygasła. Najnowszej wersji nie ma prawidłową datę — ma wartość "9999-12-31T23:59:59.9999999", co oznacza wystarczy, że data wygaśnięcia nie jest jeszcze ustawiony.

```REST
GET https://<endpoint>/subscriptions/{{subscriptionId}}/providers/Microsoft.ServiceFabric/clusterVersions?api-version= 2016-09-01

Output:
{
                  "value": [
                    {
                      "id": "subscriptions/35349203-a0b3-405e-8a23-9f1450984307/providers/Microsoft.ServiceFabric/environments/Windows/clusterVersions/5.0.1427.9490",
                      "name": "5.0.1427.9490",
                      "type": "Microsoft.ServiceFabric/environments/clusterVersions",
                      "properties": {
                        "codeVersion": "5.0.1427.9490",
                        "supportExpiryUtc": "2016-11-26T23:59:59.9999999",
                        "environment": "Windows"
                      }
                    },
                    {
                      "id": "subscriptions/35349203-a0b3-405e-8a23-9f1450984307/providers/Microsoft.ServiceFabric/environments/Windows/clusterVersions/4.0.1427.9490",
                      "name": "5.1.1427.9490",
                      "type": " Microsoft.ServiceFabric/environments/clusterVersions",
                      "properties": {
                        "codeVersion": "5.1.1427.9490",
                        "supportExpiryUtc": "9999-12-31T23:59:59.9999999",
                        "environment": "Windows"
                      }
                    },
                    {
                      "id": "subscriptions/35349203-a0b3-405e-8a23-9f1450984307/providers/Microsoft.ServiceFabric/environments/Windows/clusterVersions/4.4.1427.9490",
                      "name": "4.4.1427.9490",
                      "type": " Microsoft.ServiceFabric/environments/clusterVersions",
                      "properties": {
                        "codeVersion": "4.4.1427.9490",
                        "supportExpiryUtc": "9999-12-31T23:59:59.9999999",
                        "environment": "Linux"
                      }
                    }
                  ]
                }


```

## <a name="fabric-upgrade-behavior-when-the-cluster-upgrade-mode-is-automatic"></a>Tkaninie zachowanie podczas klaster tryb uaktualniania jest automatycznie

Firma Microsoft udostępnia kod tkaninie i konfiguracji, która działa w klastrze Azure. Automatyczne uaktualnienia monitorowane oprogramowania firma Microsoft wykonywać na zgodnie z potrzebami. Te uaktualnienia może być kod i konfiguracji. Aby upewnić się, że aplikacja cierpi nie wpływu lub minimalnego wpływu ze względu na te uaktualnienia, możemy wykonać uaktualnień na następujących etapach:

### <a name="phase-1-an-upgrade-is-performed-by-using-all-cluster-health-policies"></a>Faza 1: Uaktualnienie jest wykonywane przy użyciu wszystkich zasad dotyczących kondycji klaster

Podczas tej fazy uaktualniania Przejdź jedną domenę uaktualnienia naraz, a aplikacje działające w klastrze nadal będą uruchamiane bez dowolnego przestoje. Zasady dotyczące kondycji klaster (kombinacja węzeł zdrowia i kondycji wszystkie aplikacje działające w klastrze) przestrzegać podczas uaktualniania.

Jeśli zasady dotyczące kondycji klaster nie są spełnione, uaktualnianie przywróceniu. Następnie wiadomości e-mail są wysyłane do właściciela subskrypcji. Wiadomość e-mail zawiera następujące informacje:

- Powiadomienie, że było do przywrócenia uaktualnienie klaster.
- Sugerowanej działań naprawczych.
- Liczba dni (n), dopóki nie możemy wykonać etap 2.

Firma Microsoft, spróbuj wykonać uaktualnienia samego kilka razy w przypadku uaktualniania nie powiodło się ze względów infrastruktury. Po n dni od daty wysłania wiadomości e-mail możemy przejść do etap 2.

Jeśli zasady dotyczące kondycji klaster są spełnione, uaktualnianie jest traktowany jako pomyślnego i oznaczone jako ukończone. Dzieje się tak podczas początkowej uaktualnienia lub na dowolnej stronie powtórki uaktualnienia w tej fazy. Istnieje potwierdzenie pomyślne uruchomienie nie wiadomości e-mail. To aby uniknąć wysłanie możesz zbyt wiele wiadomości e-mail — odbieranie wiadomości e-mail powinny być widoczne jako wyjątek normalny. Oczekuje większość uaktualniania klaster się pomyślnie bez wpływania swojej dostępności aplikacji.

### <a name="phase-2-an-upgrade-is-performed-by-using-default-health-policies-only"></a>Faza 2: Uaktualnienie jest wykonywane przy użyciu domyślnej tylko zasady dotyczące kondycji

Zasady dotyczące kondycji na tym etapie są ustawiane w taki sposób, że liczba aplikacje, które zostały prawidłowy na początku uaktualnienia nie zmienia się na czas trwania procesu uaktualniania. Jak etap 1 uaktualnienia etap 2 Przejdź jedną domenę uaktualnienia naraz, a aplikacje działające w klastrze nadal będą uruchamiane bez dowolnego przestoje. Zasady dotyczące kondycji klaster (kombinacja węzeł zdrowia i kondycji wszystkie aplikacje działające w klastrze) przestrzegać na czas trwania uaktualnienia.

Jeśli zasady dotyczące kondycji klaster obowiązywać nie są spełnione, uaktualnianie przywróceniu. Następnie wiadomości e-mail są wysyłane do właściciela subskrypcji. Wiadomość e-mail zawiera następujące informacje:

- Powiadomienie, że było do przywrócenia uaktualnienie klaster.
- Sugerowanej działań naprawczych.
- Liczba dni (n), dopóki nie możemy wykonać fazy 3.

Firma Microsoft, spróbuj wykonać uaktualnienia samego kilka razy w przypadku uaktualniania nie powiodło się ze względów infrastruktury. Wiadomość e-mail z przypomnieniem są wysyłane na kilka dni przed rozpoczęciem n dni w górę. Po n dni od daty wysłania wiadomości e-mail możemy przejść do fazy 3. Wiadomości wysyłane w etap 2 należy poważnie i należy działań naprawczych.

Jeśli zasady dotyczące kondycji klaster są spełnione, uaktualnianie jest traktowany jako pomyślnego i oznaczone jako ukończone. Dzieje się tak podczas początkowej uaktualnienia lub na dowolnej stronie powtórki uaktualnienia w tej fazy. Istnieje potwierdzenie pomyślne uruchomienie nie wiadomości e-mail.

### <a name="phase-3-an-upgrade-is-performed-by-using-aggressive-health-policies"></a>Faza 3: Uaktualnienie odbywa się za pomocą zasad rygorystyczne kondycji

Zasady dotyczące kondycji na tym etapie są przeznaczone dla zakończenie uaktualniania zamiast Kondycja aplikacji. Niewielka liczba uaktualnień klaster trafiać tej fazy. Jeśli klaster uzyskuje do tej fazy, istnieje duże prawdopodobieństwo, że aplikacja staje się nieprawidłowe i/lub utraty dostępności.

Podobnie jak w dwóch etapów, uaktualnienia fazy 3 Przejdź jedną domenę uaktualnienia naraz.

Jeśli zasady dotyczące kondycji klaster nie są spełnione, uaktualnianie przywróceniu. Firma Microsoft, spróbuj wykonać uaktualnienia samego kilka razy w przypadku uaktualniania nie powiodło się ze względów infrastruktury. Po wykonaniu tej grupie zostanie przypięta tak, aby nie otrzymują pomocy technicznej i/lub uaktualnienia.

Wiadomość e-mail z te informacje są wysyłane do właściciela subskrypcji wraz z działań naprawczych. Firma Microsoft nie będzie wszelkie klastrów, aby przejść do stanu, w którym fazy 3 nie powiodło się.

Jeśli zasady dotyczące kondycji klaster są spełnione, uaktualnianie jest traktowany jako pomyślnego i oznaczone jako ukończone. Dzieje się tak podczas początkowej uaktualnienia lub na dowolnej stronie powtórki uaktualnienia w tej fazy. Istnieje potwierdzenie pomyślne uruchomienie nie wiadomości e-mail.

## <a name="cluster-configurations-that-you-control"></a>Konfiguracje klastrów, które można sterować

W tym miejscu są konfiguracji, które można zmienić na żywo klaster, oprócz możliwość określenia klaster tryb uaktualniania.

### <a name="certificates"></a>Certyfikaty

Można dodać nowe lub łatwo usunąć certyfikaty dla klastrów i klienta za pośrednictwem portalu. Odwołują się do [tego dokumentu, aby uzyskać szczegółowe instrukcje](service-fabric-cluster-security-update-certs-azure.md)

![Zrzut ekranu przedstawiający odciski palców certyfikatu w portalu Azure.][CertificateUpgrade]


### <a name="application-ports"></a>Porty aplikacji

Porty aplikacji można zmienić, modyfikując właściwości zasobu równoważenia obciążenia, które są skojarzone z typem węzła. Można korzystać z portalu lub można użyć programu PowerShell Menedżera zasobów bezpośrednio.

Aby otworzyć nowy port na wszystkich maszyny wirtualne w typ węzła, wykonaj następujące czynności:

1. Dodawanie nowego sondy do równoważenia obciążenia właściwe.

    Jeśli wdrożono klaster za pomocą portalu urządzenia do równoważenia obciążenia są nazywane "Kg Nazwa zasobu grupy NodeTypename", jedną dla każdego typu węzła. Ponieważ nazwy równoważenia obciążenia są unikatowe tylko w grupie zasobów, najlepiej w przypadku wyszukiwania dla nich w obszarze danej grupy zasobów.

    ![Zrzut ekranu przedstawiający dodawanie sondy do równoważenia obciążenia w portalu.][AddingProbes]

2. Dodaj nową regułę do równoważenia obciążenia.

    Dodawanie nowej reguły do samej usługi równoważenia obciążenia przy użyciu sondy utworzony w poprzednim kroku.

    ![Dodawanie nowej reguły do równoważenia obciążenia w portalu.][AddingLBRules]


### <a name="placement-properties"></a>Położenie właściwości

Dla każdego typu węzeł możesz dodać właściwości niestandardowe położenie, których chcesz użyć w aplikacjach. Typ węzła jest właściwością domyślną, którego można używać bez dodawania go jawnie.

>[AZURE.NOTE] Aby uzyskać szczegółowe informacje na używanie ograniczeń położenie, właściwości węzła i sposobu definiowania je można znaleźć w sekcji "Położenie ograniczenia i właściwości węzła" w dokumencie Menedżera zasobów usługi tkaninie klaster w [Klastrze i opisem](service-fabric-cluster-resource-manager-cluster-description.md).

### <a name="capacity-metrics"></a>Wskaźniki wydajności

Dla każdego typu węzeł możesz dodać metryki zdolności niestandardowy, którego chcesz użyć w aplikacjach załadować raportu. Szczegółowe informacje na temat stosowania metryki zdolności do raportu załadować, zajrzyj do dokumentów Menedżera zasobów klaster tkaninie usługi na [Klaster swój opisujących](service-fabric-cluster-resource-manager-cluster-description.md) i [metryki i załaduj](service-fabric-cluster-resource-manager-metrics.md).

### <a name="fabric-upgrade-settings---health-polices"></a>Zasady dotyczące kondycji ustawienia uaktualnienia tkaninie-

Można określić niestandardowe kondycja zasadami tkaninie uaktualnienia. Jeśli została wybrana klaster tkaninie automatyczne uaktualnienie, te zasady zostać zastosowane do fazy i aktualizacje automatyczne tkaninie.
Jeśli wybrano klaster tkaninie ręcznego uaktualnienia, te zasady zostać zastosowane podczas wybierania nowej wersji powodujące system, aby rozpocząć uaktualnianie tkaninie w klastrze. Jeśli nie zastępują zasady, będą używane ustawienia domyślne.

Można określić zasady kondycji niestandardowej lub przeglądać bieżące ustawienia w obszarze karta "tkaninie uaktualnienie", wybierając pozycję Zaawansowane ustawienia uaktualnienia. Przejrzyj następujące obrazu na temat. 

![Zarządzanie zasadami kondycji niestandardowe][HealthPolices]

### <a name="customize-fabric-settings-for-your-cluster"></a>Dostosuj ustawienia tkaninie klaster

Sprawdź [Ustawienia tkaninie klaster tkaninie usługi](service-fabric-cluster-fabric-settings.md) co oraz jak można je dostosować.

### <a name="os-patches-on-the-vms-that-make-up-the-cluster"></a>Poprawki systemu operacyjnego na maszyny wirtualne, które składają się klaster

Ta funkcja jest planowane na przyszłość jako funkcja automatycznego. Ale obecnie jesteś zobowiązany do poprawki pośrednictwem usługi SMS. Należy to jeden maszyn wirtualnych w danym momencie, tak aby nie mają w dół więcej niż jednym naraz.

### <a name="os-upgrades-on-the-vms-that-make-up-the-cluster"></a>Uaktualnianie systemów operacyjnych na maszyny wirtualne, które składają się klaster

Jeśli musisz uaktualnić obraz systemu operacyjnego w środowisku maszyn wirtualnych systemu klaster, należy go jeden maszyn wirtualnych jednocześnie. Odpowiada użytkownik dla tego uaktualnienia — nie jest obecnie nie automatyzacji, w tym.

## <a name="next-steps"></a>Następne kroki
- Dowiedz się, jak dostosowywać niektóre [Ustawienia tkaninie klaster tkaninie usługi](service-fabric-cluster-fabric-settings.md)
- Dowiedz się, jak [i zmniejszanie](service-fabric-cluster-scale-up-down.md) przeskalować klaster
- Więcej informacji na temat [uaktualnienia aplikacji](service-fabric-application-upgrade.md)

<!--Image references-->
[CertificateUpgrade]: ./media/service-fabric-cluster-upgrade/CertificateUpgrade2.png
[AddingProbes]: ./media/service-fabric-cluster-upgrade/addingProbes2.PNG
[AddingLBRules]: ./media/service-fabric-cluster-upgrade/addingLBRules.png
[HealthPolices]: ./media/service-fabric-cluster-upgrade/Manage_AutomodeWadvSettings.PNG
[ARMUpgradeMode]: ./media/service-fabric-cluster-upgrade/ARMUpgradeMode.PNG
[Create_Manualmode]: ./media/service-fabric-cluster-upgrade/Create_Manualmode.PNG
[Manage_Automaticmode]: ./media/service-fabric-cluster-upgrade/Manage_Automaticmode.PNG