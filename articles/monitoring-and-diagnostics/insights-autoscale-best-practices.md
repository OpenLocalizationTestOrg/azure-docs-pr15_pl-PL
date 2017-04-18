<properties
    pageTitle="Najważniejsze wskazówki dotyczące autoscaling Azure Monitor. | Microsoft Azure"
    description="Dowiedz się, zasady efektywnie używać autoscaling w monitorze Azure."
    authors="kamathashwin"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="ashwink"/>

# <a name="best-practices-for-azure-monitor-autoscaling"></a>Najważniejsze wskazówki dotyczące autoscaling Azure Monitor

Poniższe sekcje w tym dokumencie ułatwić Ci zrozumienie najważniejsze wskazówki dla w autoscale Azure. Po przejrzeniu te informacje, można będzie lepiej efektywnie używać autoscale w infrastrukturze Azure.

## <a name="autoscale-concepts"></a>Pojęcia Autoscale

- Zasób można mieć tylko *jeden* ustawienie autoscale
- Ustawienie autoscale może mieć jeden lub więcej profilów i każdego profilu mogą zawierać jedną lub więcej reguł autoscale.
- Ustawienie autoscale skale wystąpienia poziomo, czyli *się* , zwiększając wystąpienia i *w* zmniejszając liczbę wystąpień.
 Ustawienie autoscale ma maksimum, minimum i wartość domyślna wystąpień.
- Zadanie autoscale odczytuje zawsze skojarzone metryki przeskalować, sprawdzania, czy występują skrzyżowane skonfigurowany próg dla skali w nowym oknie lub w skali. Możesz wyświetlić listę miar tego autoscale można skalować przez u [metryki typowych autoscaling Azure Monitor](insights-autoscale-common-metrics.md).
- Wszystkie Progi są obliczane na poziomie wystąpienie. Na przykład "skali przez wystąpienie 1 gdy średnia Procesora > 80%, w przypadku wystąpienia liczba 2", oznacza, że skala w nowym oknie, gdy średnia Procesora we wszystkich wystąpieniach jest większa niż 80%.
- Otrzymywanie powiadomienia o niepowodzeniu za pośrednictwem poczty e-mail. W szczególności właściciela, współautorów i czytników zasobu docelowego odbierać wiadomości e-mail. Gdy autoscale odzyskuje awarii i uruchamia, działa prawidłowo, również zawsze otrzymać wiadomość e-mail z *odzyskiwania* .
- Użytkownik może Wyraź zgodę na powiadomienie pomyślnego skali akcji za pośrednictwem poczty e-mail i webhooks.

## <a name="autoscale-best-practices"></a>Autoscale najważniejsze wskazówki

Jak używać autoscale za pomocą poniższych najlepszych rozwiązań.

### <a name="ensure-the-maximum-and-minimum-values-are-different-and-have-an-adequate-margin-between-them"></a>Upewnij się, wartości maksymalne i minimalne są różne i mają odpowiedni poziom między nimi
Jeśli masz ustawienie, które ma minimalny = 2, maksymalna = 2 i bieżącego wystąpienia liczba jest 2, może wystąpić żadnego działania w skali. Zachowaj odpowiednie marginesu między zlicza wystąpienie maksymalne i minimalne, które są włącznie. Autoscale zawsze skale między te limity.

### <a name="manual-scaling-is-reset-by-autoscale-min-and-max"></a>Ręczne skalowanie zresetowaniem przez autoscale min i max
Jeśli ręcznie zaktualizować statystykę wystąpienie na wartość powyżej lub poniżej wartości maksymalnej, aparat autoscale jest automatycznie skalowany powrót do minimum (Jeśli poniżej) lub maksimum (jeśli jest powyżej). Na przykład możesz ustawić zakres od 3 do 6. Jeśli masz jedno wystąpienie uruchomionego aparat autoscale skale 3 wystąpieniach po jego następnym uruchomieniu. Ponadto go czy skala w wystąpieniach 8 z powrotem do 6 po jego następnym uruchomieniu.  Ręczne skalowanie jest bardzo tymczasowy, chyba że resetowanie z regułami autoscale.

### <a name="always-use-a-scale-out-and-scale-in-rule-combination-that-performs-an-increase-and-decrease"></a>Zawsze używaj kombinacji reguła poza skalowanie i skala w wykonującego Zwiększ i Zmniejsz
Jeśli używasz tylko jednej części kombinacja autoscale skali — w tym pojedynczy przy, aż do maksimum lub minimum, osiągnięciu.

### <a name="do-not-switch-between-the-azure-portal-and-the-azure-classic-portal-when-managing-autoscale"></a>Nie przełączać się między Azure portal i portalu klasyczny Azure podczas zarządzania Autoscale
Dla usług w chmurze i aplikacja usług (aplikacje sieci Web) Użyj portal Azure (portal.azure.com), aby utworzyć i zarządzać ustawieniami autoscale. Dla zestawów skali maszyn wirtualnych za pomocą PoSH, polecenie lub interfejsu API usługi REST tworzyć i zarządzać nimi ustawienie autoscale. Nie przełączać się między Azure portal klasyczny (manage.windowsazure.com) i portal Azure (portal.azure.com) podczas zarządzania autoscale konfiguracji. Azure klasyczny portal i jego źródłowych wewnętrznej bazy danych ma ograniczenia. Przejście do portalu Azure Zarządzanie autoscale przy użyciu graficznego interfejsu użytkownika. Aby użyć autoscale programu PowerShell, polecenie lub interfejsu API usługi REST (za pomocą Eksploratora zasobów Azure) są następujące opcje.

### <a name="choose-the-appropriate-statistic-for-your-diagnostics-metric"></a>Wybierz odpowiednie statystyki dla swojego metryki diagnostyki
Narzędzia diagnostyczne miar można wybierać między *Średnia*, *Minimum*, *maksymalne* i *sumy* jako metryki przeskalować przez. Najczęściej używane statystyczny jest *Średnia*.

### <a name="choose-the-thresholds-carefully-for-all-metric-types"></a>Wybierz pozycję progi starannie dla wszystkich typów metryczne
Zalecamy wybranie starannie różne progi poza skalowanie i skala w według sytuacjach praktycznych.

Firma Microsoft *nie jest zalecane* ustawienia autoscale, takich jak przykładach poniżej z wartości progowe takie same lub bardzo podobne do, a w warunki:

- Zwiększanie wystąpienia 1 zliczanie, gdy liczba wątków < = 600
- Zmniejszanie wystąpienia 1 zliczanie, gdy liczba wątków > = 600

Przyjrzyjmy się z przykładem co może prowadzić do zachowanie, które może wydawać się skomplikowane. Cosider następującej.

1. Przyjmijmy istnieją sytuacje, 2 będzie zaczynać się, a następnie średnia liczba wątków dla każdego wystąpienia zawiera 625.
2. Skale Autoscale się dodać 3 wystąpienie.
3. Następnie przyjęto założenie, że 575 mieści się liczba wątków średnia przez wystąpienie.
4. Przed skalowania, próbuje autoscale oszacowania jakie stan końcowy będzie, jeśli skalowany go w. Na przykład 575 x 3 (bieżącego wystąpienia liczba) = 1,725-2 (wersja ostateczna liczba wystąpień podczas skalowania w dół) = 862.5 wątków. Oznacza to, że autoscale wymaga od razu poza skalowanie ponownie nawet po skalowana, jeśli liczba wątków średnia pozostaje taki sam lub nawet przypada tylko mała ilość. Jednak w przypadku zmiany skali go ponownie, całego procesu będzie powtórzyć, prowadzące do nieskończonej pętli.
5. Aby tego uniknąć (określane jako "flapping"), autoscale nie działa w dół w ogóle. Zamiast tego pomija i reevaluates warunek ponownie przy następnym wykonuje zadanie usługi. Może to mylić wiele osób, ponieważ nie ma działać, gdy liczba wątków średnia została 575 autoscale.

Szacowanie podczas skala w ma unikania "flappy". Należy zachować to zachowanie w zdanie po wybraniu takie same progi dla poza skalowanie i.

Zalecamy wybranie odpowiedniego marginesu między poza skalowanie i progi. Na przykład należy rozważyć następujące lepiej kombinacji reguły.

- Zwiększanie wystąpienia 1 podczas zliczania CPU % > = 80
- Zmniejszanie wystąpienia 1 podczas zliczania CPU % < = 60

W tym przypadku  

1. Przyjęto założenie, że nie ma zaczynać się wystąpienia 2.
2. Jeśli średnia % CPU w jego wystąpieniach przechodzi na 80, autoscale skale się dodawanie trzecie wystąpienie.
3. Załóżmy teraz czasie CPU % należącą do 60.
4. Osoby Autoscale w skali reguły szacuje stan końcowy, gdyby w skali. Na przykład 60 x 3 (bieżącego wystąpienia liczba) = 180-2 (wersja ostateczna liczba wystąpień podczas skalowania w dół) = o 90 stopni. Aby autoscale nie Skala w ponieważ będą musiały poza skalowanie ponownie natychmiast. Zamiast tego pomija skalowania.
5. Sprawdza następnego autoscale czas, Procesora w dalszym ciągu należeć do 50. Ponownie — szacuje wystąpienie 50 x 3 = 150 / 2 wystąpień = 75, czyli poniżej progu poza skalowanie 80, więc go może w pomyślnie wystąpień 2.

### <a name="considerations-for-scaling-threshold-values-for-special-metrics"></a>Zagadnienia dotyczące skalowania wartości progowe specjalne miar
 Specjalne miar, takich jak miejsca do magazynowania lub usługa Bus kolejki metryki długość progu jest średnia liczba wystąpień wiadomości na bieżącą liczbę wystąpień. Starannie wybierz wybierz wartość progowa dla tej metryki.

Załóżmy przedstawić go z przykładem, aby upewnić się, że rozumiesz zachowanie lepiej.

- Zwiększa wystąpienia według liczby 1, jeśli liczba wiadomości kolejki miejsca do magazynowania > = 50
- Zmniejszanie wystąpienia według liczby 1, gdy liczba wiadomości kolejki miejsca do magazynowania < = 10

Należy rozważyć następujące sekwencji:

1. Istnieją sytuacje kolejki, 2 miejsca do magazynowania.
2. Zachowaj wkrótce wiadomości i po przejrzeniu kolejki miejsca do magazynowania łączną liczbę elementów odczytuje 50. Możesz może założono, że tej autoscale powinna rozpoczynać akcję skala w nowym oknie. Jednak należy pamiętać, że nadal 50/2 = 25 wiadomości dla każdego wystąpienia. Tak poza skalowanie nie występuje. Do pierwszej skali numerów nastąpić całkowita liczba wiadomości w kolejce miejsca do magazynowania powinny być 100.
3. Następnie przyjęto założenie, że całkowita liczba wiadomości osiągnie 100.
4. 3 wystąpieniu kolejki miejsca do magazynowania jest dodawany z powodu akcji skala w nowym oknie.  Kolejną czynnością poza skalowanie nie nastąpi aż całkowita liczba wiadomości w kolejce osiągnie 150, ponieważ 150/3 = 50.
5. Teraz liczba wiadomości w kolejce otrzymuje mniejszy. Z 3 wystąpienia pierwszej akcji skali się dzieje, gdy całkowita liczba wiadomości we wszystkich kolejkach dodać do 30, ponieważ 30-3 = 10 wiadomości dla każdego wystąpienia, czyli progu Skala.

### <a name="considerations-for-scaling-when-multiple-profiles-are-configured-in-an-autoscale-setting"></a>Zagadnienia dotyczące podczas profili skonfigurowano ustawienie autoscale skalowania

Ustawienie autoscale można wybrać profilu domyślnego jest zawsze stosowana bez zależność harmonogram lub godziny, lub możesz wybrać profil cyklicznego lub profil na czas z zakres dat i godzin.

Kiedy autoscale usługa przetwarza je, zawsze sprawdza w następującej kolejności:

1. Stała data profilu
2. Cykliczne profilu
3. Domyślny profilu ("zawsze")

Jeśli spełnienia warunku profilu autoscale nie sprawdza następnego warunek profilu znajdujących się pod nim. Autoscale przetwarza tylko jeden profil naraz. Oznacza to, jeśli chcesz także zawierać warunek przetwarzania z profilu niższego poziomu te reguły musi zawierać także w bieżącym profilu.

Przeanalizujmy to w przykładzie:

Poniższa ilustracja przedstawia ustawienie autoscale z profilu domyślnego wystąpień minimalne = 2, a maksymalna liczba wystąpień = 10. W tym przykładzie reguły są skonfigurowane do skali w nowym oknie, gdy liczba wiadomości w kolejce jest większa niż 10 i skala w gdy liczba wiadomości w kolejce jest mniejsza niż 3. Aby teraz zasobu można skalować między wystąpieniami 2-10.

Ponadto jest cykliczne profilu od poniedziałku. Jest ustawiona dla wystąpień minimalne = 2, a maksymalna liczba wystąpień = 12. Oznacza to, w poniedziałek, pierwszy autoscale czasu sprawdza, czy ten warunek, Zliczanie wystąpień w przypadku 2, skale go do nowej co najmniej 3. Jak długo autoscale będzie nadal występował znaleźć ten warunek profilu spełnione (poniedziałek), tylko przetwarza Procesora reguły oparte na skali w nowym oknie-ruch przychodzący skonfigurowane dla tego profilu. W tej chwili go nie sprawdza długość kolejki. Jednak jeśli chcesz również warunek długość kolejki do sprawdzenia, możesz dołączyć te reguły z profilu domyślnego także swój profil Poniedziałek.

Podobnie kiedy autoscale przełączy się do profilu domyślnego, najpierw sprawdza, jeśli są spełnione warunki minimum i maksimum. Jeśli liczba wystąpień w czasie jest 12, jego skale w 10, maksymalną dozwoloną dla profilu domyślnego.

![Ustawienia autoscale](./media/insights-autoscale-best-practices/insights-autoscale-best-practices.png)

### <a name="considerations-for-scaling-when-multiple-rules-are-configured-in-a-profile"></a>Zagadnienia dotyczące skalowania, gdy wiele reguł są skonfigurowane w profilu
Istnieją przypadki, w których może być konieczne ustawić wiele reguł w profilu. Następujący zestaw reguł autoscale są używane przy użyciu usług, gdy skonfigurowano wiele reguł.

Na *skali się*autoscale działa, jeśli jest spełniony dowolny reguły.
*W skali*autoscale wymagają wszystkie reguły, które muszą zostać spełnione.

Aby przedstawić, przyjęto założenie, że masz następujące reguły autoscale 4:

- Jeśli Procesor < 30%, skali w przez 1
- Jeśli pamięć < 50%, skala w przez 1
- Jeśli Procesor > 75%, poza skalowanie przez 1
- Jeśli pamięć > 75%, poza skalowanie przez 1

Następnie występuje poniżej:

- Jeśli 76% jest Procesora i pamięci jest 50%, możemy skala w nowym oknie.
- Jeśli 50% jest Procesora i pamięci jest 76% możemy skala w nowym oknie.

Z drugiej strony, jeśli Procesora wynosi 25%, a pamięć jest autoscale 51% wykonuje **nie** skala w. W celu skala w, Procesora musi być 29% i pamięci 49%.

### <a name="always-select-a-safe-default-instance-count"></a>Zawsze wybierz pozycję liczność wystąpienia domyślnego bezpieczne
Liczba wystąpienia domyślnego jest ważne, że autoscale skale usługi do tej liczby, gdy metryki nie są dostępne. W związku z tym wybierz pozycję Liczba wystąpienia domyślnego jest bezpieczny dla swojego obciążenia.

### <a name="configure-autoscale-notifications"></a>Konfigurowanie powiadomień dla autoscale
Autoscale powiadamia administratorów i współautorzy zasobu za pomocą poczty e-mail, jeśli wystąpi którykolwiek z następujących warunków:

- usługi autoscale nie powiedzie się wykonaj akcję.
- Metryki nie są dostępne dla usługi autoscale podjęcie decyzji Skala.
- Metryki są dostępne (odzyskiwania) ponownie w celu podjęcia decyzji Skala.
Oprócz powyższych warunkach możesz skonfigurować powiadomienia e-mail lub webhook otrzymywanie powiadomień o do pomyślnego skali akcji.
