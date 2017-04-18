<properties 
    pageTitle="Azure zaangażowania przenośnych przewodnik wprowadzenie z najlepszymi rozwiązaniami"
    description="Uzyskiwanie przewodnik uruchomiony zaangażowania Mobile Azure i najważniejsze wskazówki dotyczące ułatwiającej rozpoczęcie korzystania" 
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="wesmc7777"
    manager="erikre"
    editor=""/>

<tags
    ms.service="mobile-engagement"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="mobile-multiple"
    ms.workload="mobile" 
    ms.date="10/04/2016"
    ms.author="wesmc;ricksal"/>

# <a name="azure-mobile-engagement---getting-started-guide-with-best-practices"></a>Azure zaangażowania urządzeń przenośnych — wprowadzenie przewodnika z najlepszymi rozwiązaniami

## <a name="overview"></a>Omówienie

**Ekranu telefonu komórkowego jest bardzo dużo danych miejsca:** W 2013 badań znaleźć średnią urządzenia przenośnego zostały zainstalowane aplikacje 27. Użytkownicy zwykle poświęconych 30 godzin na miesiąc ich aplikacje. Większość tego czasu zajęło w sieci społecznościowej i gier (około 20 godzin). 2014 Android market ma około 1,5 miliona aplikacji dla użytkowników do wyboru. Apple store zawiera około 1,2 miliona aplikacje. Używanie aplikacji dla urządzeń przenośnych nadal jest zwiększenie, jak deweloperzy współzawodniczenia w tym wzrostu rynku. 

Średnia użytkownik urządzenia przenośnego zostanie instalowania i odinstalowywania aplikacji z wysokiej częstotliwości w zależności od zmieniania zainteresowań i możliwości pracy w aplikacji. W celu ustalenia powodzenia aplikacji staje się kluczową, aby dowiedzieć się więcej niż tylko ilu użytkowników instalacji aplikacji. Należy wiedzieć przydatne, jak aplikacja jest i jeśli zmiany tego trendu zastosowania. Ważne stają się następujące pytania:

- Czy użytkownicy początku, aby znaleźć aplikację uninteresting lub przestarzałe? 
- Ilu użytkowników zostały zatrzymane w ogóle korzystania z aplikacji? 
- Są zakupów w aplikacji popularny w górę lub w dół?
- Użytkownicy występują do wykonania przepływów pracy z powodu problemów z aplikacji lub brak zainteresowania? 
- Możesz przechowywać aplikacji przydatne i istotne przez naciśnięcie najnowszej treści do bazy użytkowników? 
- Czy tę zawartość świeży być taka sama dla wszystkich użytkowników lub ograniczony do segmentów użytkownika na podstawie zachowań w aplikacji? 
 
Odpowiedzi na pytania podobne do następujących może pomóc wydłużyć życia i zysk z Twojej aplikacji. Można również pomagają definiowanie i zachować bazy użytkowników. 

Multimedia związane z aplikacji mają niektóre najwyższe przechowywania wśród użytkowników. Jedną z przyczyn tego jest stale są udostępniane świeży zawartości do użytkowników. Przyjęcia powiadomienia wypychane przydatne do segmentu użytkowników na wczesnym etapie zwykle mają duży wpływ na przechowywanie aplikacji. 

Program zaangażowania Mobile Azure ma na celu pomóc rozszerzanie życia i przechowywania aplikacji, dostarczając metody do zbierania i analizowania szczegółowe informacje na temat używania aplikacji. Ułatwi to klasyfikować użytkownika podstawowy zgodnie z zachowaniem i tworzenie kampanii mającego fokus dostarczania powiadomień wypychanych i wiadomości w aplikacji odcinków określonego użytkownika. Kluczowe wskaźniki wydajności (KPI) zmierzyć, jak aktywni użytkownicy mogą się z różnych aspektów aplikacji. Azure zaangażowania Mobile udostępnia metody, należy ustalić następujące kluczowe wskaźniki wydajności. Pomaga zwiększyć zwrot z inwestycji, dostarczając infrastruktury należy zwiększyć zaangażowanie z Twojej aplikacji dla urządzeń przenośnych. 

Aby w pełni wykorzystać zaangażowania Mobile Azure, należy uruchomić z planem dobrze zaprojektowany zaangażowania. Plan pomoże zidentyfikować szczegółowe dane, które muszą mieć możliwość segmentu bazy użytkowników. Może to być oparty na zachowanie i możliwości pracy w aplikacji. Aby planu się pomyślnie jest dobrym rozwiązaniem, aby jasno określić kluczowego wskaźnika wydajności, które będą miały wymiary celów aplikacji. Ze wskaźnikami wydajności wyczyść zdefiniowane możesz łatwo osadzić niezbędne logiki aplikacji zbieranie danych lub dostosowywanie kolorów, którego użyjesz do analizowania i oceny kluczowych wskaźników wydajności. Ten temat dotyczy definiowanie kluczowych wskaźników wydajności, korzystające z planu zaangażowania najważniejsze wskazówki. 


## <a name="step-1-define-your-kpis-to-fit-the-bet-model"></a>Krok 1: Definiowanie kluczowych wskaźników wydajności, aby dopasować modelu TRAFIENIA


Definiowanie prawidłowo KPI może być trudne zadania do wykonania. Aplikacje przeznaczone dla różnych branży mieć własne szczegółów i celów. To zwykle należy mylić swoje podejście. Aby tego uniknąć, celów i wskaźników KPI należy można podzielić na trzy główne kategorie: **firm**, **zaangażowania**i **technicznych**. Jest to, co nazywamy **TRAFIENIE modelu**.

Dobry plan ma zazwyczaj cele ze wskaźnikami KPI, mierzące sukcesów w każdej z następujących kategorii modelu TRAFIENIE.


#### <a name="business-kpis"></a>Kluczowe wskaźniki wydajności biznesowej

Kluczowe wskaźniki wydajności biznesowej należy najprostszym części do utworzenia. Prawdopodobnie zdefiniowane w niektórych formularza podczas planowanych Twojej aplikacji dla urządzeń przenośnych. Wskaźniki zazwyczaj pomoc dotyczącą aplikacji możesz miara przychodu i zwrotu z inwestycji. Poniższa lista zawiera niektóre przykład firm kluczowych wskaźników wydajności, które mogą pomóc pomagają podczas definiowania usługi wskaźniki wydajności:

- Multimedia firm kluczowych wskaźników wydajności
    - Liczba reklam kliknięciu
    - Liczba odwiedzin strony dla poszczególnych użytkowników
    - Liczba bieżącej subskrypcji
- Kluczowe wskaźniki wydajności gier firm
    - Liczba-app zakupów
    - Średni przychód na użytkownika (ARPU)
    - Czas sesji
    - Dni odtwarzanych i bieżącego poziomu gier
- Kluczowe wskaźniki wydajności biznesowej elektronicznego
    - Aplikacja dni używane
    - Średni przychód na użytkownika (ARPU)
    - Średnia kwota w koszyku podczas realizacji transakcji
    - Kategoria produktów dla większości widoków i zakupów
- Bank i ubezpieczenia firm kluczowych wskaźników wydajności
    - Liczba kont
    - Funkcje aktywowane
    - Odwiedzonej strony ofert
    - Alerty kliknięte lub aktywowany      



#### <a name="engagement-kpis"></a>Kluczowe wskaźniki wydajności zaangażowania

Wskaźnik KPI zaangażowania jest wskaźnik wydajności do pomiaru zaangażowania użytkowników. Trendy w tym obszarze pomagają w określeniu, przechowywania aplikacji. Poniżej przedstawiono kilka wskaźniki wydajności przykład dla tego typu kluczowego wskaźnika wydajności:

- Aktywni użytkownicy w ciągu ostatnich 7 dni
- Liczba użytkowników nieaktywne ostatnich 7 dni
- Liczba użytkowników, którzy nie korzysta aplikacja 30 dni  

Niektóre oczywiste czynniki zewnętrzne mogą mieć wpływ wskaźniki w tym obszarze. Na przykład można rozważyć urządzenia przenośnego, aby się z tym użytkownikiem przez cały czas. Może to lub może nie być spełnione. Aplikacja gier mogą mają wyższą zastosowania wokół dni wolne, gdy gamer mogą być odtwarzane więcej podczas pracy i wylogowywanie się z szkoły.   

Dobrze określona kluczowe wskaźniki wydajności w tej kategorii należy ułatwiają zmierzyć relacje między aplikacji i klientów.



#### <a name="technical-kpis"></a>Techniczne kluczowych wskaźników wydajności

Wskaźniki wydajności w tej kategorii pomagają w określeniu, jeśli aplikacji jest zachowuje się poprawnie, blokuje się lub awarii. Wskaźniki te można kondycję aplikacji i określić problemy związane z użytecznością, które mogą uniemożliwić użytkownikom przy użyciu aplikacji. Informacje zebrane dla tej kategorii może również zawierać informacje wydajności dotyczyć działań marketingowych członkom zespołu. Dane również mogą być pomocne w rozwiązywaniu problemów przez IT i obsługa techniczna członkom zespołu do identyfikowania nieudokumentowanych błędów. 
 
Oto kilka przykładów techniczne kluczowych wskaźników wydajności:

- Informacje o wyjątku nieobsługiwanego lub obsłużone i liczba 
- Sygnatura czasowa dla ostatniej awarii
- Przycisk ostatniego lub odwiedzonej ostatniej strony
- Użycie pamięci przez aplikację
- Szybkość odtwarzania aplikacji
- Wersja systemu operacyjnego uruchomionym aplikacji
- Wersja aplikacji

Definiowanie tych kluczowych wskaźników wydajności do pomiaru wydajności aplikacji Pomoc i pinpoint potencjalne błędy. Ten wskaźniki powinny być pomocne skrócić czas potrzebny do przeprowadzania poprawki dla klientów. Można również pomagają zdefiniować segment użytkowników, które pojawiły się określonego problemów. Za pomocą tego segmentacji użytkownika na tworzenie kampanii do przeprowadzania powiadomień dotyczących potencjalnych promocje i dostępne poprawki, aby odzyskać zadowolenia klientów. 


#### <a name="playbook-exercise-1-create-your-kpi-dashboard"></a>Wykonywanie playbook 1: Tworzenie pulpitu nawigacyjnego kluczowego wskaźnika wydajności

Podczas definiowania strategii marketingowej, kluczowych wskaźników wydajności należy przedstawiać dla każdego z celów głównym. Powinny one być punktów danych jasno określone, które umożliwią zbieranie ważnych informacji monitorowanie aplikacji i działanie użytkownika końcowego.

Tworzenie kluczowego wskaźnika wydajności pulpitu nawigacyjnego, który zawiera poniżej informacji

1.  Co to są kluczowych wskaźników wydajności do aplikacji?
2.  Punkty danych będzie używać reprezentować każdego kluczowego wskaźnika wydajności?
3.  Gdzie znajduje się te dane dla mojej aplikacji (to znaczy ekranu, ustawienia, system...)?
4.  Dla tego kluczowego wskaźnika wydajności można odtwarzać sekwencji zaangażowania?

Za pomocą arkusza **Konstruktor kluczowego wskaźnika wydajności** w naszym [Szablon Playbook multimediów] [ Media Playbook link] dla wskazówki i przykłady.



## <a name="step-2-your-engagement-program"></a>Krok 2: Program zaangażowania


Program doskonałe zaangażowania przenośnych należy rozważyć klucza część aplikacji. Naprawdę obejmuje doskonałe powitalnej program, który wykonuje użytkownika pierwszych dni użycia aplikacji. Jest to prawdopodobnie mają bardzo dodatni wpływ na zatrudnienie i przechowywania aplikacji. Badania mają pokazano, że większość użytkowników Zatrzymaj przy użyciu aplikacji pierwszych kilka dni po zakończeniu instalacji. Chcesz dążyć do spotkania lub wcześniej przekracza odsetek kierowania oczekiwanie klienta, gdy użytkownik nadal jest ograniczony do aplikacji. Upewnij się, że prezentowanie wartości klucza i zalety pakietu aplikacji do klientów. 


![](./media/mobile-engagement-getting-started-best-practices/unsegmented-push-notifications.png)

Powiadomienia wypychane są najlepszym sposobem najwcześniejsze pozyskiwaniu użytkownikom urządzenia przenośnego. Jednak doskonałe ostrożność segmentacji użytkowników w celu powiadomienia wypychane. Ponieważ po wydaje się użytkownika są odbierane wiadomości-śmieci lub uninteresting powiadomienia, może mieć wpływ poważne. Kilka kliknięć użytkownik może usunąć aplikacji nigdy nie, aby powrócić. Użytkownik powinien otrzymywać zamiast rodzajowy spamu wysoce spersonalizowanych wartości w aplikacji.

Gdy aktywnie uczestniczą użytkowników, następnie programu zaangażowania może pomóc w sterują inne aspekty aplikacji.

Na przykład można skonfigurować kampanii żądania usługi aktywni użytkownicy, aby ocenić aplikację. Ponieważ tej części użytkownika jest najbardziej aktywne oraz większość doświadczeń z Tobą aplikacji, można oczekiwać je dokładność ocena. Po umieszczeniu ocena aplikacji duży, może ułatwić dysk w górę organiczny — pobieranie aplikacji także obniżyć koszty nabycia nowego klienta.



#### <a name="engagement-sequence"></a>Sekwencja zaangażowania


Globalne programu zaangażowania obejmuje różne zaangażowania sekwencji. Każda sekwencja ma na celu osiągnięcia kilka wskaźników.


###### <a name="life-push-sequence"></a>Życia wypychanych sekwencji


Cele życia sekwencji wypychanych różnią się w zależności od do zarządzania cyklem życia zaangażowania użytkownika za pomocą aplikacji. Określonego użytkownika może być nowe, nieaktywne lub bardzo aktywna. Na różnych etapach cyklu życia zaangażowania użytkownicy mogą korzystać z świeży zawartości w postaci porady lub łączy się z dokumentacją. 

Na przykład nowego użytkownika może potrzebujesz pomocy kolumnowo wprowadzenie do aplikacji lub korzystać z nowych chęć użytkownika podobny do następującego po raz pierwszy ich Uruchom aplikację...

*"Okazuje się do podłączone! Pamiętaj, aby zalogować się do przejść z 1 miesiąca bezpłatne!"*


###### <a name="behavioral-push-sequence"></a>Sekwencja wypychanych funkcjonalne

Sekwencja funkcjonalne wypychanych ma na celu zwiększyć zużycie na podstawie zachowań użytkownika zbierane aplikacji.  

Na przykład bardzo aktywnego użytkownika aplikacji piłka nożna wirtualnych mogą korzystać z uczestniczenia z następnego powiadomienia wypychane...

*"Jan są wentylatora poważne piłka nożna! Zaloguj się do sekcji naszych NFL i win swobodny dostęp do SuperBowl!"*


###### <a name="alerting-push-sequence"></a>Sekwencja wypychanych alertów

Użytkownicy będą Dziękujemy za ważne wiadomości ograniczony do ich interesy. Sekwencji wypychanych alert usprawnia zaangażowania, wysyłanie alertów według zainteresowania, które wyraźnie pokazuje użytkownika. Może to być jawne, gdy użytkownik wybierze ich własne interesy w aplikacji. Można również oznaczyć, niejawnie na podstawie danych zebranych podczas interakcji użytkownika z tej aplikacji.

Na przykład użytkownika aplikacji elektronicznego regularnie może kupić określonych Marka kawy, które przechwycone z firm kluczowego wskaźnika wydajności. Następujący alert można zwiększyć zaangażowania tego użytkownika przy użyciu aplikacji.
 
*"Witaj następującej, jedną z ulubionych marek kawy będzie widoczny na sprzedaż 25% pierwszy tydzień września 2015 r. Dziękujemy za jako klienta i chce upewnij się, możesz wiedziały".*

###### <a name="rentention-push-sequence"></a>Sekwencja wypychanych Rentention

Ta sekwencja ma na celu zachowanie użytkownicy kampanii powiadomień wypychanych powtarzające się ułatwiające dysk zwykła rodzaj atrakcyjnych za pomocą aplikacji. To może pomóc w poprawieniu przechowywania aplikacji, jeśli użytkownik korzysta z interakcji. 

Na przykład użytkownika aplikacji powiązanych sports może pojawić się następujące powiadomienie wypychanych tygodniowych według zespołów Ulubione użytkownika:

*"Dla możliwość zdobywają 200 punktów, czy Przejdź w głosowaniu Yankees Nowy Jork będzie win ich nożna ten tydzień przed Toronto niebieski Jays!"*


#### <a name="the-3w-approach"></a>Metody 3W

Opanowania sekwencji różnych wypychanych pomoże Ci współpracować z użytkowników końcowych. Jednak nadal należy użyć metody 3W personalizowania powiadomienia. Metody 3W zwracają, która co i kiedy dla każdego powiadomienia. Jeśli wyraźnie spełnia następujące trzy pytania możesz powiadomienia należy prawidłowo poświęcić dla zaangażowania.

![](./media/mobile-engagement-getting-started-best-practices/who-what-when.png)



###### <a name="who-the-user-segment-that-will-receive-messages"></a>Kto: Użytkownik segmenty, które będą otrzymywać wiadomości

Naciśnięcie powiadomienia do użytkowników należy rozważyć kanału komunikacji bardzo poufne. Upewnij się, że powiadomień, które mają na celu Wyślij do segmentu użytkowników oraz są ograniczone do zainteresowań części tego użytkownika. Powiadomienie o niepoprawnie routingu jest bardzo mogą mieć ujemny wpływ na użytkownika. Uznają go prowadzące do aplikacji odinstalowywania wiadomości-śmieci. 

Podczas definiowania segmenty użytkowników, których będziesz otrzymywać powiadomień, należy użyć kombinacji określone kryteria techniczne i funkcjonalne. Prosty przykład definiowania segmentu użytkowników może być podobna do następujących instrukcji:

"Wszyscy użytkownicy, którzy uruchomione aplikacji mobilnej pierwszy raz 3 dni i wcześniej odwiedzić stronę logowania dwa razy bez rzeczywistego wykonywania logowania".
 
Tej instrukcji pomaga zidentyfikować dane, które są potrzebne do zbierania do obsługi określonego scenariusza.


###### <a name="what-the-message-that-you-will-send"></a>Co: Zostanie wysłana wiadomość

**Ton**

W swojej pozyskiwaniu za pomocą sygnał, który jest odpowiedni dla Twojej dla segmentowany użytkowników. Która z pewnością jest dobrym sposobem na łączenie się z użytkowników końcowych i promowanie zainteresowanie użytkownika aplikacji. 

**Przekierowywanie**

Powiadomienia push można używać do więcej niż otwarcia aplikacji. Jeśli wiadomość z powiadomieniem zapewnia kontekst takich jak emitowanie wiadomości lub promocji produktu, to powiadomienie może głębokości bezpośrednie łącze do odpowiedniej zawartości w aplikacji. W tym musisz utworzyć schemat adresu URL, aby umożliwić aplikacji Zarządzanie przekierowywanie. Podczas pracy w sekwencji z zaangażowania, to jest ważny krok, która nie musi być zapomniane.

Przekierowywanie można również zarządzać dla innych systemów. Na przykład przy użyciu adresu URL akcji jest przekierowywać użytkowników końcowych do wielu innych systemów, łącznie z następujących czynności:

- Witryny sieci Web
- Konfigurowanie już skrzynki pocztowej za pomocą poczty e-mail
- Pole wiadomości SMS
- Usługa Wybieranie numeru
- Bezpośrednio do aplikacji przechowywanie ocena aplikacji. 

Dzięki temu wiele możliwości obsłudze użytkowników końcowych oraz tworzenie reguły automatycznego, aby poprawić występy.


**Formatowanie i zawartość**

Różne typy i formaty powiadomień wypychanych:

1. **Ogłoszenia** : umożliwia wiadomości reklamami wysłać do użytkowników w różnych chwil (wylogowywanie się z aplikacji w aplikacji lub w dowolnym momencie).
2. **Ankiety** : umożliwiającymi gromadzenie informacji z użytkownikami końcowymi przez ich pytania. Te odpowiedzi będą dostępne podczas tworzenia kryteriów dla użytkowników końcowych docelowej.
3. **Umieszcza danych** : umożliwia wysyłanie pliku danych binarnych lub base64, aby zaktualizować aplikację. Informacje zawarte w wypychanych dane są wysyłane do aplikacji do personalizowania środowiska użytkowników w aplikacji. Aplikacja musi być zaprojektowane do obsługi danych w wypychanych danych.
4. **Kafelki (tylko dla Windows Phone)** : umożliwiającymi za pomocą usługi wypychanych powiadomień firmy Microsoft (MPNS) można wysyłać natywnych powiadomienia Push zawierającego dane XML (obsługiwane od zestawu SDK w wersji 0.9.0. Ostatni ładunek kafelków nie może przekraczać 32 kilobajty.). Pojawi się bezpośrednio na kafelku do tablicy.
5. **Widok sieci Web** : widok sieci web jest podręcznym zawartością sieci web. Ta podręczna pojawia się, gdy użytkownik końcowy kliknie powiadomień push. Widok sieci web umożliwia ma więcej interakcji z użytkownika końcowego.
 
>[AZURE.NOTE] Upewnij się, że zawartości, do której wysyłasz jako powiadomień wypychanych jest zgodny z odpowiednich platformy (iOS, Android, Windows) wytyczne dotyczące opracowywania aplikacji i wysyłania powiadomień wypychanych.

 


###### <a name="when-the-timing-of-your-campaign"></a>Kiedy: Chronometrażu kampanii

Gdy jest najlepszej godziny na aktywowanie kampanii powodujące powiadomienia wypychane Czy ma być ręczne i automatyczne? Czy powinny być cykliczne? Określanie czasu w prawo lub częstotliwości jest nawiązanie użytkownikom najlepszych rezultatów. Dla każdego sekwencji zaangażowania i scenariusza, należy określić, kiedy będą najlepszej godziny na wysyłanie powiadomienia wypychane. Oto niektóre przykłady:

![](./media/mobile-engagement-getting-started-best-practices/campaign-timing-examples.png)

Jeśli wysyłasz dużo powiadomienia codziennie, należy wykonać uwagę poważne czy użytkownicy mogą odczuć komunikacji jako wiadomości-śmieci. 

Azure zaangażowania Mobile udostępnia dwa sposoby pomóc w uniknięciu komunikacji jest traktowany jako wiadomości-śmieci. Najpierw upewnij się, że nie celem tym użytkownikom za pomocą segmentacji ziarno poprawnie. Ponadto zaangażowania Mobile Azure udostępnia funkcję "przydział". Ta funkcja może ograniczyć powiadomienia wysyłane dla kampanii. Na przykład ustawienie przydział domyślny 5 tygodniowo będzie upewnij się, że użytkownik będących częścią kampanii segment użytkowników, otrzymuje powiadomienia o nie więcej niż 5 na ten tydzień.





#### <a name="playbook-exercise-2-create-your-engagement-program"></a>Wykonywanie playbook 2: Tworzenie programu zaangażowania

Poświęć trochę czasu, podsumowywania celów i definiowanie kampanii można się spodziewać odtwarzania przy użyciu określonej sekwencji. Upewnij się, że możesz zastosować podejście 3W do powiadomienia o jego kampanii. 

Za pomocą arkusza **Program zaangażowania** w [Szablonie Playbook multimediów] [ Media Playbook link] dla wskazówki i przykłady.


## <a name="step-3-app-integration"></a>Krok 3: Integracja aplikacji


#### <a name="create-a-tag-plan"></a>Tworzenie planu znacznika

Aby zintegrować zaangażowania Mobile Azure do aplikacji należy utworzyć plan znacznik. Plan znacznika jest podstawą projektu. Definiuje relację między obrotu specyfikacji przepływu pracy, aplikacji i rzeczywistą znacznik danych zebranych w aplikacji do pomiaru kluczowych wskaźników wydajności. Wskazuje, jakie analizy będzie widoczny w portalu. Pomaga także określ segmenty użytkownika, a następnie wyślij powiadomienia wypychane mającego fokus nawiązanie użytkowników końcowych. Raz masz plan znacznika definicja, dodanie kodu do integracji aplikacji jest proste, przy użyciu zestawu SDK zaangażowania Mobile Azure.

Plan znacznika nie powinna znakowanie wszystkich aplikacji. Powinien zawierać tylko danych znacznika, który jest częścią strategii zaangażowania urządzeń przenośnych. Są to prawdopodobnie różnorodną między aplikacjami. [Szablon Playbook Media] [ Media Playbook link] pod warunkiem przez zaangażowania Mobile Azure ułatwia tworzenie planu znacznika z danej metody. Użyj arkusza **Planu znacznika** jako szablon do tworzenia planu znacznik.

Podczas definiowania sekcji znacznika w arkuszu, być bardzo precyzyjne. Jest to bardzo ważne uniknąć zamieszania. Szczegóły poszczególnych planowane scenariusza, w którym będą wysyłane każdy znacznik. Dołącz nazwę tego działania w miejsce, w którym jest osadzony każdy znacznik. To wszystko uwzględnianych w **Informative** części arkusza. Znacznik arkusza planu powinny być głównym punktem odniesienia dla test weryfikacji. 

W sekcji **danych na potrzeby zbierania** zespół deweloperów powinien znajdować się typy, nazwy, wartości i pary wartość klucza dodatkowe informacje wymagane dla każdego znacznika, który zostanie osadzony w aplikacji.

Zaleca się przeglądanie plan znacznika zespołom wszystkie skojarzone z projektem. Wprowadź konieczne poprawki i upewnij się, że wszystko jest wyczyszczone dla obrotu i rozwoju członkom zespołu.

Arkusz **instrukcji pracy** można łatwiej poprowadzić, które wszyscy zaangażowanie w projekcie.


#### <a name="data-types"></a>Typy danych

Są często używanych typów danych pomocy technicznej przez zaangażowania Mobile Azure.

###### <a name="devices-and-users"></a>Urządzenia i użytkowników

Azure zaangażowania Mobile identyfikuje użytkowników za pomocą generuje unikatowy identyfikator dla każdego urządzenia. Ten identyfikator jest określana mianem identyfikator urządzenia (lub identyfikator urządzenia). Jest generowany w taki sposób, aby wszystkie aplikacje na tym samym urządzeniu miały ten sam identyfikator urządzenia.

###### <a name="sessions-and-activities"></a>Sesje i działania

Sesja jest jednego wystąpienia aplikacji uruchomione przez użytkownika. Sesja obejmuje z uruchomieniu aplikacji do zatrzymania.

Czynność jest logiczna grupa zestaw aplikacji może wykonać podczas sesji. Jest to zazwyczaj określonego ekranu w aplikacji, ale może być cokolwiek określone przez logikę aplikacji. Co najmniej należy oznakowanie każdego ekranu lub aktywności dla aplikacji. Dzięki temu będzie można łatwiej zrozumieć ścieżki użytkownika.


###### <a name="events"></a>Zdarzenia

Zdarzenia są używane do raportu interakcji użytkownika z tej aplikacji. Mogą być błyskawiczne działań, takich jak udostępnianie zawartości lub uruchamianie klipu wideo. Znakowanie zdarzeń umożliwi zbiorów danych, pokazujące interakcja użytkowników z tej aplikacji. 

###### <a name="jobs"></a>Zadania

Zadania są używane do raportu akcji, które mają czas trwania. Oto kilka przykładów będzie zawierać:

- Wykonanie interfejsu API
- Czas wyświetlania reklam
- Czas trwania zadania tła 
- Czas trwania procesu zakupu
- Wyświetlanie klipu wideo


###### <a name="errors"></a>Błędy

Błędy są używane do zgłaszać problemy wykryte za pomocą aplikacji. Na przykład akcje użytkownika niepoprawne lub błędy połączenia interfejsu API.

###### <a name="application-information"></a>Informacje o aplikacji

Informacje o aplikacji (informacje o aplikacji) jest używany do znakowanie dane dotyczące wrażenia użytkownika za pomocą aplikacji. Jest generowany przez użytkownika w aplikacji. 

Dla danej aplikacji info klucza zaangażowania Mobile Azure tylko rejestruje informacje o najnowszych wartości (nie Historia). Informacje o aplikacji powoduje wyświetlenie stanu aplikacji lub użytkowników końcowych. Na przykład stanu w dzienniku lub grupę Ulubione produktu użytkownika.

###### <a name="crash-data"></a>Dane awarii

Ulec awarii danych zebranych automatycznie przez telefon komórkowy zaangażowania SDK błędy aplikacji raportów nie są obsługiwane przez aplikację. Na przykład powodu nieobsługiwanego wyjątku suwaka.


###### <a name="extra-data"></a>Dodatkowe dane

Zdarzenia, błędy, działania i zadań można ulepszyć z parametrami. Jest to informacje dodatkowe, które deweloper może zapewnić jako określone dane z poziomu aplikacji. Jest to ważne określających szerokiego segmentacji. 

Na przykład wartość znacznik "artykuł" pozwoli użytkownikom części według kto wyświetlać określonego artykułu. Jednak, który może być za mało. Możliwe poprawić ten sam znacznik "artykuł" zawiera również dodatkowe — informacje, takie jak "news_category" działania. To przydaje się do określania dynamicznie Ulubione kategorie dla użytkownika. 

Dodatkowe informacje są zgłoszone jako pary klucz wartość. W tym przykładzie dla tej aplikacji multimediów informacje dodatkowe "news_category" będzie wartość dla tej kategorii. Na przykład "sports", "zużycia" lub "polityka".





#### <a name="tag-and-sdk-integration"></a>Integracja znacznika i SDK 

Aby uzyskać instrukcje krok po kroku zintegrować SDK zaangażowania Mobile Azure aplikacji wykonaj dokumentacji dotyczącej [Integracji SDK zaangażowania](mobile-engagement-windows-store-integrate-engagement.md) Azure witryny sieci Web. Wybierz docelową platformą z łącza u góry tej strony.

Zaleca się utworzenie projektów w celu obiema aplikacjami wbudowane na wierzchu zaangażowania Mobile Azure. Jeden rozwoju i tymczasowego badania i drugi tymczasowego produkcji. Działu informatycznego można promować z tymczasowego test produkcji po pomyślnym testowania akceptacji użytkownika.



#### <a name="user-acceptance-testing-uat"></a>Akceptacji użytkowników, testując (UAT)

Użytkownik akceptacji testowanie (UAT) polega, upewniając się, że wszystko działa zgodnie z przeznaczeniem. Przepływy pracy mogą być wykonywane i zbieranie wszystkie wymagane danych, w zależności od planu znacznika:
 
- Znakowanie informacje powinny być stosowane zgodnie z udokumentowanymi pojęcia AZME
- Wszystkie informacje, które są potrzebne są zbierane (w tym dodatkowe informacje o wartość aplikacji informacje)
- Dopasowuje nomenklatury według układu planowanie znacznika
- Nie ma żadnych zduplikowanych znaczniki wysłane

Dokładnie przetestować wszystkie typy zachowanie powiadomienie, które zostały osadzone w aplikacji

- Dane anonsy, ankiety, umieszcza się z aplikacji i w aplikacji
- Widoki tekstu i sieci Web
- Znaczek aktualizacji kategorii



#### <a name="setup"></a>Konfiguracja

Konfigurowanie Azure zaangażowania Mobile jest bardzo proste. Wszystkie dokumentację dotyczącą interfejsu użytkownika jest dostępna w witrynie sieci Web zaangażowania Mobile Azure [sposób nawigowania w obrębie interfejsu użytkownika](mobile-engagement-user-interface-home.md).

Zalecane jest, po uruchomieniu konfigurując prawo ról i członkostwo ról użytkowników projektu. Pomaga to zarządzanie odpowiedni dostęp do platformy dla wszystkich użytkowników. Role usługi mogą obejmować:

- Administratorzy
- Deweloperzy
- Osoby przeglądające 

Następnie:
- Zarejestruj się w swojej identyfikator urządzenia, aby przetestować na urządzeniu z systemem własnych.
- Przejdź do strony Ustawienia konta i Konfigurowanie strefy czasowej wykresy i powiadomień dostarczania czas strefy czasowej.
- Przejdź do strony Ustawienia aplikacji i zarejestrować "Aplikacji — informacje" musisz samodzielnie przez użytkownika końcowego docelowej.

Aby uzyskać więcej informacji na temat uruchamiania pierwszego kampanii powiadomienia wypychane Przejrzyj [jak rozpocząć pracę przy użyciu i zarządzanie nim umieszcza się skontaktować dla użytkowników końcowych](mobile-engagement-how-tos.md).



## <a name="conclusion"></a>Wnioski


Programy zaangażowania są iteracyjne i możesz należy ciągłego doskonalenia Twoje jako eksperymentować co najlepiej pasuje do aplikacji. 

Początkowo podczas opracowywania pracę w programie zaangażowania strategii nie próby Tworzenie strategii całego zaangażowania globalnej. Skorzystaj z podejście krok po kroku Identyfikowanie kluczowych wskaźników wydajności i jak korzystać z nich. Strategia zaangażowania będzie unikatowa dla każdej aplikacji.

Po utworzono niektóre możliwości, warto rozważyć dodawanie następujących programów pakietu zaangażowania:

- Śledzenie: Uzyskiwanie użytkowników i prawdopodobnie Zdefiniuj źródła zbierania danych. Do zbierania danych źródeł można połączyć Azure zaangażowania urządzeń przenośnych. Umożliwia monitorowanie parametrów każdego źródła. Te informacje będą interesujące maksymalizować inwestycji nabycia. 

- A / B testowania: jest to niezbędne część zaangażowania program. Każda aplikacja ma własny szczegóły. Z A / B badania, możesz poprawić programu zaangażowania.

- Lokalizacja Geo: Jest to duży szansy sprzedaży marek. Dzięki tej funkcji można osiągnąć w odpowiednim miejscu i godziny. Zaleca się sprawdzenie zostały zebrane za mało użytkowników końcowych zachowanie danych przed rozpoczęciem pracy geo lokalizacji.

- Wypychanych danych: wypychanych danych jest niewidoczna push. Wypychanych danych umożliwia dostosowywanie aplikacji na podstawie zachowań użytkowników końcowych. Na przykład jeśli segmentu użytkowników często sprawdza nowoczesne produktów, właściciel aplikacji wysłać wypychanych danych, które będzie personalizowanie jej strony głównej zawartości nowoczesne.



## <a name="next-steps"></a>Następne kroki

- [Utwórz konto Azure Mobile zaangażowania](mobile-engagement-create.md).
- Odwiedź stronę [Definiuj strategii zaangażowania Mobile](mobile-engagement-define-your-mobile-engagement-strategy.md) , aby dowiedzieć się więcej o definiowaniu strategii zaangażowania Mobile.



  

<!--Image references-->


<!--Link references-->
[Media Playbook link]: https://github.com/Azure/azure-mobile-engagement-samples/tree/master/Playbooks
