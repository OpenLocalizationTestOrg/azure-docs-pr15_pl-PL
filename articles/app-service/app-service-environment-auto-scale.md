<properties
    pageTitle="Autoscaling i środowiska aplikacji usługi | Microsoft Azure"
    description="Autoscaling i środowiska aplikacji usługi"
    services="app-service"
    documentationCenter=""
    authors="btardif"
    manager="wpickett"
    editor=""
/>

<tags
    ms.service="app-service"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="byvinyal"
/>

# <a name="autoscaling-and-app-service-environment"></a>Autoscaling i środowiska aplikacji usługi

Azure środowiskach aplikacji usługi pomocy technicznej *autoscaling*. Możesz autoscale pul poszczególnych pracowników na podstawie metryki lub harmonogramu.

![Opcje Autoscale puli pracownika.][intro]

Autoscaling optymalizuje do wykorzystania zasobów przez automatycznie wzrostu i zmniejszania środowisku usługi aplikacji dopasowania budżetu i załadować profilu.

## <a name="configure-worker-pool-autoscale"></a>Konfigurowanie autoscale puli pracownika

Na karcie **Ustawienia** puli pracownik, masz dostęp do funkcji autoscale.

![Karta Ustawienia puli pracownika.][settings-scale]

W tym miejscu interfejs ma być dobrze znać, ponieważ jest takich samych warunkach, zobacz Kiedy skalować plan usług aplikacji. 

![Ustawienia ręcznego Skala.][scale-manual]

Można również skonfigurować profil autoscale.

![Ustawienia Autoscale.][scale-profile]

Profile Autoscale są przydatne ustawić limity dotyczące usługi skali. W ten sposób możesz mieć spójne wydajności, możliwości, ustawiając wartość skali Granica dolna (1) i zakończenie przewidywalne wydatków, ustawiając górną granicę (2).

![Zmienianie ustawień w profilu.][scale-profile2]

Po zdefiniowaniu profilu, można dodać reguł autoscale przeskalować w górę lub dół liczbę wystąpień w puli pracowników w zakresie zdefiniowane przez profil. Reguły Autoscale są oparte na metryki.

![Reguła Skala.][scale-rule]

 Roboczych ani metryki frontonu może służyć do definiowania reguły autoscale. Te wskaźniki są same miary można monitorować na wykresach karta zasobów i ustawianie alertów dla.

## <a name="autoscale-example"></a>Przykład Autoscale

Autoscale środowisku usługi aplikacji najlepiej można zilustrowane Instruktaż scenariusza.

W tym artykule wyjaśniono wszystkie niezbędne zagadnienia, podczas konfigurowania autoscale. Artykuł przeprowadzi Cię przez interakcje, które przychodzących do odtwarzania, gdy czynnika w autoscaling środowiskach aplikacji usługi, które są obsługiwane w środowisku usługi aplikacji.

### <a name="scenario-introduction"></a>Scenariusz wprowadzenie

Michał jest administratora systemu w przedsiębiorstwie, który ma migracji część obciążeń zarządza w środowisku usługi aplikacji.

Środowisko usługi aplikacji jest skonfigurowany do ręcznego skala w następujący sposób:

* **Wierzch kończy się:** 3
* **Roboczych 1**: 10
* **Roboczych 2**: 5
* **Roboczych 3**: 5

Roboczych 1 jest używana do obciążenia produkcji podczas roboczych 2 i roboczych 3 są używane do jakości (QA) i obciążenia rozwoju.

Plany aplikacji usługi dla aparatu pytania i odpowiedzi i deweloperów są skonfigurowane do ręcznego Skala. Produkcji plan usług aplikacji jest równa autoscale na zajęcie się warianty obciążenia i ruchu.

Michał jest bardzo zaznajomiony z aplikacją. On wie, że godzin Szczyt obciążenia jest między 9:00 AM a 6:00 PM, ponieważ jest to linia biznesowych aplikacji (LOB), używaną przez pracowników, gdy są one w biurze. Po wykonaniu tej pomija zastosowania, gdy użytkownicy są wykonywane dla tego dnia. Poza godzinami Szczyt jest nadal niektórych ładowania ponieważ użytkownicy mają dostęp do aplikacji zdalnie przy użyciu swoich urządzeń przenośnych lub komputerach domowych. Produkcji plan usług aplikacji jest już skonfigurowana do autoscale oparte na użycie Procesora z następującymi regułami:

![Określone ustawienia związane z aplikacją LOB.][asp-scale]

|   **Profil Autoscale — dni robocze — plan usług aplikacji**     |   **Profil Autoscale — weekendy — plan usług aplikacji**     |
|   ----------------------------------------------------    |   ----------------------------------------------------    |
|   **Nazwa:** Weekday profilu                               |   **Nazwa:** Weekend profilu                               |
|   **Skalowanie przez:** Planowanie i wydajności reguł            |   **Skalowanie przez:** Planowanie i wydajności reguł            |
|   **Profilu:** Dni tygodnia                                   |   **Profilu:** Weekend                                    |
|   **Typ:** Cykl                                    |   **Typ:** Cykl                                    |
|   **Zakres docelowej:** 5-20 wystąpień                     |   **Zakres docelowej:** wystąpienia 3-10                     |
|   **Dni:** Poniedziałek, Wtorek, Środa, czwartek, piątek  |   **Dni:** Sobota, niedziela                              |
|   **Czas rozpoczęcia:** 9:00 AM                                 |   **Czas rozpoczęcia:** 9:00 AM                                 |
|   **Strefa czasowa:** UTC 08                                   |   **Strefa czasowa:** UTC 08                                   |
|                                                           |                                                           |
|   **Reguła Autoscale (skala w górę)**                           |   **Reguła Autoscale (skala w górę)**                           |
|   **Zasobu:** Produkcji (środowisko aplikacji usługi)      |   **Zasobu:** Produkcji (środowisko aplikacji usługi)      |
|   **Metryki:** PROCESOR %                                       |   **Metryki:** PROCESOR %                                       |
|   **Operacji:** Większe niż 60%                         |   **Operacji:** Wynosi ponad 80%                         |
|   **Czas trwania:** 5 minut                                 |   **Czasu trwania:** 10 minut                                |
|   **Czasu agregacji:** Średnia                           |   **Czasu agregacji:** Średnia                           |
|   **Akcji:** Zwiększanie liczby 2                         |   **Akcji:** Zwiększanie liczby 1                         |
|   **Atrakcyjne w dół (w minutach):** 15                             |   **Atrakcyjne w dół (w minutach):** 20                             |
|                                                           |                                                           |
  	|   **Reguła Autoscale (skala w dół)**                     |   **Reguła Autoscale (skala w dół)**                         |
|   **Zasobu:** Produkcji (środowisko aplikacji usługi)      |   **Zasobu:** Produkcji (środowisko aplikacji usługi)      |
|   **Metryki:** PROCESOR %                                       |   **Metryki:** PROCESOR %                                       |
|   **Operacji:** Mniej niż 30%                            |   **Operacji:** Mniej niż 20%                            |
|   **Czasu trwania:** 10 minut                                |   **Czas trwania:** 15 minut                                |
|   **Czasu agregacji:** Średnia                           |   **Czasu agregacji:** Średnia                           |
|   **Akcja:** Zmniejszanie liczby 1                         |   **Akcji:** Zmniejszanie liczby 1                         |
|   **Atrakcyjne w dół (w minutach):** 20                             |   **Atrakcyjne w dół (w minutach):** 10                             |

### <a name="app-service-plan-inflation-rate"></a>Stawka inflacji planu aplikacji usługi

Plany aplikacji usługi, które są skonfigurowane do autoscale zrobić maksymalna szybkość na godzinę. Tego kursu można jest obliczana na podstawie wartości podane w regule autoscale.

Opis i obliczanie *stawka inflacji planu aplikacji usługi* jest ważna w przypadku aplikacji usługi środowiska autoscale, ponieważ zmiany skalę roboczych nie są chwilową.

Stawka inflacji planu aplikacji usługi jest obliczana w następujący sposób:

![Obliczanie stopy inflacji plan aplikacji usługi.][ASP-Inflation]

Oparte na Autoscale — reguły skala w górę w profilu Weekday produkcji plan usług aplikacji:

![Stawka inflacji planu aplikacji usługi dla dni tygodnia według Autoscale — reguły skala w górę.][Equation1]

W przypadku Autoscale — reguły skala w górę w profilu Weekend produkcji plan usług aplikacji, formuła będzie rozpoznawać:

![Aplikacja usługi kurs inflacji plan weekendy według Autoscale — reguły skala w górę.][Equation2]

Ta wartość oblicza się dla skali szczegółów operacji.

Oparte na Autoscale — reguły skali szczegółów profilu Weekday produkcji plan usług aplikacji, to będzie wyglądał w następujący sposób:

![Stawka inflacji planu aplikacji usługi dla dni tygodnia według Autoscale — reguły skala w dół.][Equation3]

W przypadku Autoscale — reguły skali szczegółów profilu Weekend produkcji plan usług aplikacji, formuła będzie rozpoznawać:  

![Aplikacja usługi kurs inflacji plan weekendy według Autoscale — reguły skala w dół.][Equation4]

Maksymalna liczba osiem wystąpienia i godzin w tygodniu i cztery godzinę wystąpienia podczas weekend maksymalny produkcji plan usług aplikacji. Jej Zwolnij sześć godzinę wystąpienia w weekendy i wystąpienia maksymalna szybkość cztery wystąpienia i godzin w tygodniu.

Jeśli wiele planów aplikacji usług są obsługiwane w puli pracownika, należy obliczyć *Całkowita liczba inflacji* jako sumę stopa inflacji w przypadku wszystkich planów usługi aplikacji, które są w tej puli pracownika.

![Całkowita liczba inflacji obliczeń dla wielu planów aplikacji usługi obsługiwany w puli pracownika.][ASP-Total-Inflation]

### <a name="use-the-app-service-plan-inflation-rate-to-define-worker-pool-autoscale-rules"></a>Umożliwia określenie reguł autoscale puli Pracownik stawka inflacji planu aplikacji usługi

Pracownik pul aplikacji usługi plany, które są skonfigurowane do autoscale potrzebne do przydzielenia bufor pojemność hoście. Bufor umożliwia operacje autoscale powiększanie i zmniejszanie plan usług aplikacji, stosownie do potrzeb. Minimalne bufor będzie obliczona suma aplikacji usługi Planowanie inflacji stopa.

Ponieważ operacji skali środowiska usługi aplikacji zająć trochę czasu, aby zastosować, zmiany należy uwzględniają dalszymi zmianami żądanie, które może się zdarzyć, w trakcie operacji Skala. Aby uwzględnić ten czas oczekiwania, zaleca się użycie obliczona suma aplikacji usługi Planowanie inflacji stopa jako minimalna liczba wystąpień, które zostały dodane w dla każdej operacji autoscale.

Przy użyciu tych informacji Michał można określić następujące profil autoscale i reguły:

![Autoscale reguły profilu, na przykład LOB.][Worker-Pool-Scale]

|   **Profil Autoscale — dni tygodnia**                        |   **Profil Autoscale — weekendy**                |
|   ----------------------------------------------------    |   --------------------------------------------    |
|   **Nazwa:** Weekday profilu                               |   **Nazwa:** Weekend profilu                       |
|   **Skalowanie przez:** Planowanie i wydajności reguł            |   **Skalowanie przez:** Planowanie i wydajności reguł    |
|   **Profilu:** Dni tygodnia                                   |   **Profilu:** Weekend                            |
|   **Typ:** Cykl                                    |   **Typ:** Cykl                            |
|   **Zakres docelowej:** 13 i 25 wystąpień                    |   **Zakres docelowej:** wystąpienia 6-15             |
|   **Dni:** Poniedziałek, Wtorek, Środa, czwartek, piątek  |   **Dni:** Sobota, niedziela                      |
|   **Czas rozpoczęcia:** 7:00:00                                 |   **Czas rozpoczęcia:** 9:00 AM                         |
|   **Strefa czasowa:** UTC 08                                   |   **Strefa czasowa:** UTC 08                           |
|                                                           |                                                   |
|   **Reguła Autoscale (skala w górę)**                           |   **Reguła Autoscale (skala w górę)**                   |
|   **Zasobu:** Roboczych 1                             |   **Zasobu:** Roboczych 1                     |
|   **Metryki:** WorkersAvailable                            |   **Metryki:** WorkersAvailable                    |
|   **Operacji:** Mniejsze niż 8                              |   **Operacji:** Mniej niż 3                      |
|   **Czas trwania:** 20 minut                                |   **Czas trwania:** 30 minut                        |
|   **Czasu agregacji:** Średnia                           |   **Czasu agregacji:** Średnia                   |
|   **Akcji:** Zwiększanie liczby 8                         |   **Akcji:** Zwiększanie liczby 3                 |
|   **Atrakcyjne w dół (w minutach):** 180                            |   **Atrakcyjne w dół (w minutach):** 180                    |
|                                                           |                                                   |
|   **Reguła Autoscale (skala w dół)**                         |   **Reguła Autoscale (skala w dół)**                 |
|   **Zasobu:** Roboczych 1                             |   **Zasobu:** Roboczych 1                     |
|   **Metryki:** WorkersAvailable                            |   **Metryki:** WorkersAvailable                    |
|   **Operacji:** Wartość większą niż 8                           |   **Operacji:** Większe niż 3                   |
|   **Czas trwania:** 20 minut                                |   **Czas trwania:** 15 minut                        |
|   **Czasu agregacji:** Średnia                           |   **Czasu agregacji:** Średnia                   |
|   **Akcji:** Zmniejszanie liczby 2                         |   **Akcji:** Zmniejszanie liczby 3                 |
|   **Atrakcyjne w dół (w minutach):** 120                            |   **Atrakcyjne w dół (w minutach):** 120                    |

Zakres docelowej zdefiniowana w profilu jest obliczana przez minimalny wystąpienia zdefiniowane w profilu dla aplikacji usługi plan + buforu.

Zakres maksymalny będzie sumą wszystkie zakresy maksymalna dla wszystkich planów usługi aplikacji hostowanej w puli pracownika.

Liczba zwiększenie skali reguł powinna być równa co najmniej 1 X stopa inflacji aplikacji usługi Plan skala w górę.

Zmniejszenie liczby można dostosować do innej między 1/2 X lub 1 X stopa inflacji aplikacji usługi Plan skala w dół.

### <a name="autoscale-for-front-end-pool"></a>Autoscale zewnętrzną puli

Zasady autoscale zewnętrzną są łatwiejsze niż puli pracownika. Przede wszystkim należy  
Upewnij się, że czas trwania pomiaru i jaka cooldown należy rozważyć, czy skali symulacji plan usług aplikacji nie są chwilową.

W tym scenariuszu Michał wiedzieli, że częstotliwość błędów zwiększa po interfejsy do 80% procesora i Ustawia regułę autoscale, aby zwiększyć wystąpienia w następujący sposób:

![Ustawienia Autoscale puli zewnętrzną.][Front-End-Scale]

|   **Profil Autoscale — przodu kończy się**              |
|   --------------------------------------------    |
|   **Nazwa:** Kończy wierzch Autoscale —                |
|   **Skalowanie przez:** Planowanie i wydajności reguł    |
|   **Profilu:** Codzienny                           |
|   **Typ:** Cykl                            |
|   **Zakres docelowej:** wystąpienia 3-10             |
|   **Dni:** Codzienny                              |
|   **Czas rozpoczęcia:** 9:00 AM                         |
|   **Strefa czasowa:** UTC 08                           |
|                                                   |
|   **Reguła Autoscale (skala w górę)**                   |
|   **Zasobu:** Pula zewnętrzną                    |
|   **Metryki:** PROCESOR %                               |
|   **Operacji:** Większe niż 60%                 |
|   **Czas trwania:** 20 minut                        |
|   **Czasu agregacji:** Średnia                   |
|   **Akcji:** Zwiększanie liczby 3                 |
|   **Atrakcyjne w dół (w minutach):** 120                    |
|                                                   |
|   **Reguła Autoscale (skala w dół)**                 |
|   **Zasobu:** Roboczych 1                     |
|   **Metryki:** PROCESOR %                               |
|   **Operacji:** Mniej niż 30%                    |
|   **Czas trwania:** 20 minut                        |
|   **Czasu agregacji:** Średnia                   |
|   **Akcji:** Zmniejszanie liczby 3                 |
|   **Atrakcyjne w dół (w minutach):** 120                    |

<!-- IMAGES -->
[intro]: ./media/app-service-environment-auto-scale/introduction.png
[settings-scale]: ./media/app-service-environment-auto-scale/settings-scale.png
[scale-manual]: ./media/app-service-environment-auto-scale/scale-manual.png
[scale-profile]: ./media/app-service-environment-auto-scale/scale-profile.png
[scale-profile2]: ./media/app-service-environment-auto-scale/scale-profile-2.png
[scale-rule]: ./media/app-service-environment-auto-scale/scale-rule.png
[asp-scale]: ./media/app-service-environment-auto-scale/asp-scale.png
[ASP-Inflation]: ./media/app-service-environment-auto-scale/asp-inflation-rate.png
[Equation1]: ./media/app-service-environment-auto-scale/equation1.png
[Equation2]: ./media/app-service-environment-auto-scale/equation2.png
[Equation3]: ./media/app-service-environment-auto-scale/equation3.png
[Equation4]: ./media/app-service-environment-auto-scale/equation4.png
[ASP-Total-Inflation]: ./media/app-service-environment-auto-scale/asp-total-inflation-rate.png
[Worker-Pool-Scale]: ./media/app-service-environment-auto-scale/wp-scale.png
[Front-End-Scale]: ./media/app-service-environment-auto-scale/fe-scale.png
