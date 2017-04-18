<properties
   pageTitle="Wprowadzenie do zabezpieczeń pakietu zarządzania operacje i rozwiązanie inspekcji | Microsoft Azure"
   description="Ten dokument pomaga rozpocząć pracę z operacji zarządzania pakietu zabezpieczeń i możliwości rozwiązanie inspekcji monitorowanie usługi cloud hybrydowych."
   services="operations-management-suite"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="operations-management-suite"
   ms.topic="get-started-article" 
   ms.devlang="na"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/20/2016"
   ms.author="yurid"/>
 
# <a name="getting-started-with-operations-management-suite-security-and-audit-solution"></a>Wprowadzenie do zabezpieczeń pakietu zarządzania operacje i rozwiązanie inspekcji
Ten dokument pomaga szybko Rozpocznij pracę z możliwości rozwiązań operacje zarządzania pakietu usługi (OMS) zabezpieczeń i inspekcji poszczególnych opcji przeprowadzi Cię przez.

## <a name="what-is-oms"></a>Co to są usługi OMS?
Pakiet Microsoft operacje zarządzania usługi (OMS) jest firmy Microsoft w chmurze IT zarządzania rozwiązanie, które ułatwia zarządzanie i ochrona sieci lokalnej i w chmurze infrastruktury. Aby uzyskać więcej informacji na temat usługi OMS przeczytaj artykuł [Pakiet administracyjny operacji](https://technet.microsoft.com/library/mt484091.aspx).

## <a name="oms-security-and-audit-dashboard"></a>Zabezpieczenia usługi OMS i inspekcji pulpitu nawigacyjnego

Rozwiązanie zabezpieczeń usługi OMS i inspekcji udostępnia kompleksowy widok do Twojej organizacji postawie zabezpieczeń IT z kwerendami wbudowane wyszukiwanie problemów godne uwagi, wymagające uwagi. Pulpit nawigacyjny **zabezpieczeń i inspekcji** jest ekran główny wszystko związane z zabezpieczeniami w usługi OMS. Udostępnia wysokiego poziomu wgląd w stan zabezpieczeń komputerami. Zawiera także możliwość wyświetlania wszystkie zdarzenia w ciągu ostatnich 24 godziny, 7 dni lub innych niestandardowych przedziału czasu. Aby uzyskać dostęp do pulpitu nawigacyjnego **zabezpieczeń i inspekcji** , wykonaj następujące czynności:

1. Na pulpicie nawigacyjnym głównym **Pakietu zarządzania operacje Microsoft** kliknij kafelka **Ustawienia** po lewej stronie.
2. Karta **Ustawienia** w obszarze **rozwiązań** kliknij opcję **zabezpieczeń i inspekcji** .
3. Pojawi się na pulpicie nawigacyjnym **zabezpieczeń i inspekcji** :

    ![Zabezpieczenia usługi OMS i inspekcji pulpitu nawigacyjnego](./media/oms-security-getting-started/oms-getting-started-fig1-ga.png)

Jeśli uzyskujesz dostęp do tego pulpitu nawigacyjnego po raz pierwszy i nie ma urządzenia monitorowane przez usługi OMS, Kafelki nie zostanie umieszczony dane uzyskane od agenta. Po zainstalowaniu agenta, może upłynąć trochę czasu, aby wypełnić, dlatego początkowo Zobacz może brakować niektórych danych jako nadal przekazywania w chmurze.  W tym przypadku jest normalny niektórych kafelków bez materialne informacji. Przeczytaj [połączyć komputery bezpośrednio do usługi OMS](https://technet.microsoft.com/library/mt484108.aspx) , aby uzyskać więcej informacji na temat instalacji agenta usługi OMS w systemie Windows i [Linux oraz łączenie komputerów usługi OMS](https://technet.microsoft.com/library/mt622052.aspx) , aby uzyskać więcej informacji na temat wykonać to zadanie w systemie Linux.

> [AZURE.NOTE] Agent zapisze informacje dotyczące bieżącego zdarzenia, które są włączone, na przykład nazwy komputera, IP adres i nazwa użytkownika. Jednak nie plików i dokumentów, nazwa bazy danych lub dane prywatne są zbierane.   

Rozwiązania są zbioru reguł nabycia logiczny, wizualizacji i danych, które wyzwania klucza klienta. Zabezpieczenia i inspekcji jest jedno rozwiązanie, inne osoby można dodać oddzielnie. Przeczytaj artykuł [rozwiązań Dodaj](https://technet.microsoft.com/library/mt674635.aspx) , aby uzyskać więcej informacji na temat dodawania nowego rozwiązania.

Pulpit nawigacyjny zabezpieczeń usługi OMS i inspekcji jest podzielony na cztery kategorie głównych:

- **Domen zabezpieczeń**: w tym obszarze będzie możliwe dalsze Eksplorowanie rekordów zabezpieczeń w czasie, dostęp do oceny przed złośliwym oprogramowaniem, zaktualizuj oceny, zabezpieczeń sieciowych, tożsamości i dostępu do informacji, komputerach z zdarzenia zabezpieczeń i szybko mają dostęp do Centrum zabezpieczeń Azure pulpitu nawigacyjnego.
- **Problemy z godne uwagi**: Ta opcja umożliwia szybką identyfikację liczba aktywnych problemów i zagrożeniami związanymi z tych problemów.
- **Wykryć (wersja Preview)**: umożliwia identyfikowanie wzorców atakiem przez wizualizacji alertów zabezpieczeń, zgodnie z ich miejsce przed zasobów.
- **Zagrożenia analizy**: umożliwia identyfikowanie wzorców atakiem przez całkowitą liczbę serwerów z ruchu wychodzącego złośliwy IP, typ złośliwy zagrożenia i mapy, który pokazuje, gdzie te adresy IP są pochodzące z wizualizacji. 
- **Typowe kwerendy zabezpieczeń**: Ta opcja zawiera listę najczęstsze kwerendy zabezpieczeń, które umożliwiają monitorowanie środowiska. Po kliknięciu w jednym z kwerendami otwiera karta **Wyszukiwanie** wyników danej kwerendy.

> [AZURE.NOTE] Aby uzyskać więcej informacji dotyczących sposobu usługi OMS chroni dane przeczytaj jak usługi OMS zabezpiecza danych.

## <a name="security-domains"></a>Domen zabezpieczeń

Monitorowanie zasobów, jest ważne można było szybko uzyskać dostęp do bieżącego stanu środowiska. Jednak także jest ważne można było śledzić zdarzenia Wstecz w przeszłości, który może prowadzić do lepszego zrozumienia co się dzieje w środowisku w określonym miejscu w czasie. 

> [AZURE.NOTE] przechowywanie danych jest według usługi OMS ceny planu. Aby uzyskać więcej informacji można znaleźć [Pakiet zarządzania operacje Microsoft](https://www.microsoft.com/server-cloud/operations-management-suite/pricing.aspx) ceny strony.

Zdarzenia scenariusze dochodzenia odpowiedź i forensics bezpośrednio będą korzystać z wyników dostępnych na danym kafelku **Rekordów zabezpieczeń w czasie** .

![Rekordy zabezpieczeń w czasie](./media/oms-security-getting-started/oms-getting-started-fig2.JPG)

Po kliknięciu na tym kafelku, karta **wyszukiwania** zostanie otwarty, przedstawiający wynik kwerendy dla **Zdarzenia zabezpieczeń** (typ = SecurityEvents) z danymi według ostatnich siedmiu dni, tak jak pokazano poniżej:

![Rekordy zabezpieczeń w czasie](./media/oms-security-getting-started/oms-getting-started-fig3.JPG)

Wynik wyszukiwania dzieli się na dwa okienka: okienku po lewej stronie zawiera podział liczby zdarzeń zabezpieczeń, które zostały odnalezione komputerach, w których znaleziono się te zdarzenia, liczba kont wykryte w tych komputerów i typy działań. Okienku po prawej stronie zawiera całkowita liczba wyników i chronologicznie widoku zdarzeń zabezpieczeń z działaniem nazwę i zdarzeń na komputerze. Możesz również kliknąć **Pokaż więcej** , aby wyświetlić więcej szczegółów dotyczących to zdarzenie, takie jak dane zdarzenia, identyfikator zdarzenia i źródło zdarzenia.

> [AZURE.NOTE] Aby uzyskać więcej informacji na temat usługi OMS kwerendy wyszukiwania przeczytaj [usługi OMS wyszukiwanie odwołania](https://technet.microsoft.com/library/mt450427.aspx).

### <a name="antimalware-assessment"></a>Ocena ochrony przed złośliwym oprogramowaniem

Ta opcja umożliwia szybką identyfikację komputerach z ochrony za mało i komputerów, które są zostało naruszone przez fragmentu złośliwego oprogramowania. Stan oceny przed złośliwym oprogramowaniem i wykryto zagrożeń na serwerach monitorowane są odczytywane, a następnie dane są wysyłane do usługę w chmurze do przetwarzania. Serwery z wykryty zagrożeń i serwery zabezpieczonych za mało są wyświetlane na pulpicie oceny przed złośliwym oprogramowaniem, która jest dostępna po kliknięciu kafelku **Ocena ochrony przed złośliwym oprogramowaniem** . 

![oceny przed złośliwym oprogramowaniem](./media/oms-security-getting-started/oms-getting-started-fig4-ga.png)

Podobnie jak wszystkie inne dynamiczny Kafelek dostępne na pulpicie nawigacyjnym usługi OMS po kliknięciu, karta **wyszukiwania** zostanie otwarty z wyników kwerendy. Dla tej opcji po kliknięciu opcji **Nie raportowania** w obszarze **Statusu ochrony**będą mieć wynik kwerendy pokazujący zapisu pojedyncze, które zawiera nazwę komputera i jego pozycję, tak jak pokazano poniżej:

![wynik wyszukiwania](./media/oms-security-getting-started/oms-getting-started-fig5.png)

> [AZURE.NOTE] *Pozycja* jest klasy, podając tak, aby odzwierciedlała stanu ochrony (, wyłączanie aktualizacji itd) i zagrożeniami, które znajdują się. Jako liczba ułatwia agregacji, o który.

Kliknięcie w polu Nazwa komputera, konieczne będzie widoku chronologicznej statusu ochrony dla tego komputera. To jest bardzo przydatne w przypadku scenariuszy, w którym należy zrozumieć, jeśli po zainstalowano ochrony przed złośliwym oprogramowaniem i w pewnym momencie, który został usunięty.   

### <a name="update-assessment"></a>Ocena aktualizacji 

Ta opcja umożliwia szybkie sprawdzenie ogólnego narażenie na potencjalne problemy z zabezpieczeniami czy lub jak krytyczne aktualizacje w środowisku usługi. Zabezpieczenia usługi OMS i rozwiązanie inspekcji zapewniają tylko wizualizacji te aktualizacje, rzeczywistą dane pochodzą z [Rozwiązań aktualizacji systemu](https://technet.microsoft.com/library/mt484096.aspx), czyli innego modułu w ramach usługi OMS. Oto przykład aktualizacji:

![aktualizacje systemu](./media/oms-security-getting-started/oms-getting-started-fig6.png)

> [AZURE.NOTE] Aby uzyskać więcej informacji na temat aktualizacji rozwiązanie przeczytaj [zaktualizować serwery z rozwiązanie aktualizacji systemu](https://technet.microsoft.com/library/mt484096.aspx).

### <a name="identity-and-access"></a>Tożsamości i dostęp

Tożsamość powinny być płaszczyzny sterowania w przedsiębiorstwie, ochrona tożsamości powinny być usługi najwyższy priorytet. Mimo że w przeszłości zostały strefy wokół organizacji i te strefy zostały jeden podstawowy granic obronnych, dzisiaj z większej ilości danych i więcej aplikacji przenoszenie do chmury tożsamości staje się nowy obwód kształtu. 

> [AZURE.NOTE] obecnie danych tylko na podstawie zdarzeń zabezpieczeń logowania danych (zdarzenie 4624 identyfikator) w przyszłości logowania do usługi Office 365, a także zostaną dołączone dane Azure AD.

Monitorowanie działań tożsamości można wykonać czynności aktywne przed zdarzeniem miejscu lub reaktywne akcje, aby zatrzymać próba atakiem. Pulpit nawigacyjny **tożsamości i Access** umożliwia przeglądanie status tożsamości, w tym liczbę nieudanej próby zalogowania, konta użytkownika, które były używane podczas tych prób, konta, które zostały zablokowane, kont z zmieniono lub resetowanie hasła i obecnie liczba kont, które są rejestrowane w. 

Po kliknięciu kafelka **tożsamości i Access** pojawią się następujące pulpitu nawigacyjnego:

![tożsamości i dostęp](./media/oms-security-getting-started/oms-getting-started-fig7-ga.png)

Informacje dostępne w tym pulpitu nawigacyjnego można od razu pomocne w przypadku do identyfikowania potencjalnych podejrzane działania. Na przykład istnieje 338 próby zalogowania się jako **Administrator** i 100% tych prób nie powiodło się. Może to być spowodowane atakami z tym kontem. Po kliknięciu na tym koncie będzie uzyskać więcej informacji, które mogą pomóc ustalenie zasobowi docelowej potencjalne ataki:

![wyniki wyszukiwania](./media/oms-security-getting-started/oms-getting-started-fig8.JPG)

Szczegółowy raport zawiera ważne informacje o to zdarzenie, w tym: komputer docelowy, typ logowania (w tym przypadku logowania sieciowego), to działanie (w tym przypadku wielkość liter 4625) i Pełna oś czasu każdej próbie. 

### <a name="computers"></a>Komputery

Tym kafelku, można uzyskać dostęp do wszystkich komputerach aktywnie zdarzeń zabezpieczeń. Po kliknięciu w tym kafelku zobaczysz listę komputerów z zdarzenia zabezpieczeń i liczba zdarzeń na każdym komputerze:

![Komputery](./media/oms-security-getting-started/oms-getting-started-fig9.JPG)

Możesz kontynuować do badania, klikając na każdym komputerze i przeglądanie zdarzeń zabezpieczeń, które zostały oflagowane.

### <a name="azure-security-center"></a>Centrum zabezpieczeń Azure

Ten fragment zasadniczo jest skrót, aby uzyskać dostęp do pulpitu nawigacyjnego Centrum zabezpieczeń Azure. Aby uzyskać więcej informacji na temat tego rozwiązania, przeczytaj [Wprowadzenie do Centrum zabezpieczeń Azure](../security-center/security-center-get-started.md) .

## <a name="notable-issues"></a>Problemy z godne uwagi

Główny cel tej grupy opcji jest zapewnienie szybki podgląd problemy, które masz w środowisku usługi Kategoryzacja krytyczne, ostrzeżenia i informacyjne. Kafelków typu aktywny problem jest wizualizację tych problemów, ale nie zezwala na poszukiwania więcej szczegółowych informacji o nich, musisz użyć dolnej części tym kafelku, zawierającą nazwę problemu (nazwa), jak wiele obiektów gdyby tak się stało (liczba) i jak krytyczne jest (ważności).

![Problemy z godne uwagi](./media/oms-security-getting-started/oms-getting-started-fig10.JPG)

Widać, że te problemy były już objęte w różnych obszarach grupy **Domen zabezpieczeń** , która umacnia przeznaczenie tego widoku: wizualizowanie najważniejsze zagadnienia w środowisku z jednego miejsca.

## <a name="detections-preview"></a>Wykryć (wersja Preview)

Główny cel ta opcja jest umożliwienie IT szybkie identyfikowanie potencjalnych zagrożeń dla jego środowiska przez i ważności zagrożenie.

![Zagrożenia firmy Intel](./media/oms-security-getting-started/oms-getting-started-fig12.png)

Ta opcja może również podczas analizy odpowiedzi zdarzenia do wykonywania oceny i uzyskać więcej informacji na temat atakiem.

> [AZURE.NOTE] Aby uzyskać więcej informacji na temat korzystania z usługi OMS odpowiedź zdarzeniem Obejrzyj [jak korzystać z Centrum zabezpieczeń Azure i pakiet zarządzania operacje Microsoft odpowiedź zdarzenia](https://channel9.msdn.com/Blogs/Taste-of-Premier/ToP1703).

## <a name="threat-intelligence"></a>Zagrożenia analizy

Nową sekcję analizy zagrożenia rozwiązania zabezpieczeń i inspekcji spowoduje wizualizację desenie możliwe atakiem na kilka sposobów: liczba serwerów przy użyciu ruchu wychodzącego złośliwy IP, typ złośliwy zagrożenia i mapy, który pokazuje, gdzie te adresy IP są pochodzące z. Można korzystać z planem i kliknij pozycję adresy IP, aby uzyskać więcej informacji.

Żółty włosów na mapie wskazują, ruch przychodzący od złośliwy adresy IP. Nie jest nietypowych dla serwerów, które są dostępne z Internetem, aby wyświetlić złośliwy ruchu przychodzącego, ale zaleca się przeglądanie tych prób, aby upewnić się, że żaden z nich nie zakończyło się pomyślnie. Wskaźniki te są oparte na dzienniki programu IIS, WireData i dzienniki zapory systemu Windows.  

![Zagrożenia firmy Intel](./media/oms-security-getting-started/oms-getting-started-fig11-ga.png)

## <a name="common-security-queries"></a>Typowe kwerendy zabezpieczeń

Lista typowych kwerend zabezpieczeń może być przydatne w przypadku można szybko uzyskać dostęp do informacji o zasobie i dostosuj go na podstawie potrzeb środowiska usługi. Poniższe wspólne kwerendy są:

- Wszystkie działania zabezpieczeń
- Działań związanych z zabezpieczeniami na tym komputerze "computer01.contoso.com" (zamień nazwę komputera)
- Działań związanych z zabezpieczeniami na tym komputerze "computer01.contoso.com" dla konta "Administrator" (Zamień własnej nazwy komputera i konta)
- Zaloguj się aktywności przez komputer
- Konta, które zamykane ochrony przed złośliwym oprogramowaniem firmy Microsoft na dowolnym komputerze
- Komputery, gdzie został zakończony proces ochrony przed złośliwym oprogramowaniem firmy Microsoft
- Komputery, gdzie został "hash.exe" wykonane (Zastąp nazwą inny proces)
- Wszystkie nazwy procesu, które zostały wykonane
- Zaloguj się działania według klientów
- Konta, które zdalnie zarejestrowane na tym komputerze "computer01.contoso.com" (zamień nazwę komputera)

## <a name="see-also"></a>Zobacz też

W tym dokumencie zostały wprowadzone do rozwiązania zabezpieczeń usługi OMS i inspekcji. Aby dowiedzieć się więcej na temat zabezpieczeń usługi OMS, zobacz następujące artykuły:

- [Przegląd operacji usługi zarządzania pakietu (OMS)](operations-management-suite-overview.md)
- [Monitorowanie i reagowanie na alerty zabezpieczeń w operacji zarządzania pakietu zabezpieczeń i rozwiązanie inspekcji](oms-security-responding-alerts.md)
- [Monitorowanie zasobów operacje zarządzania pakietu zabezpieczeń i rozwiązanie inspekcji](oms-security-monitoring-resources.md)
