<properties
    pageTitle="Aktywne rozwiązanie stan replikacji katalogu w dzienniku analizy | Microsoft Azure"
    description="Pakietu rozwiązania Active Directory stan replikacji regularnie monitoruje środowiska usługi Active Directory dla wszystkie błędy replikacji i raporty wyników na pulpicie nawigacyjnym usługi OMS."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>

# <a name="active-directory-replication-status-solution-in-log-analytics"></a>Aktywne rozwiązanie stan replikacji katalogu w analizy dziennika

Usługa Active Directory jest składnik kluczowych przedsiębiorstwa informatyczną. W celu zapewnienia wysokiej dostępności i Wysoka wydajność, każdy kontroler domeny ma własną kopię bazy danych usługi Active Directory. Kontrolery replikować ze sobą, aby propagowanie zmian w przedsiębiorstwie. Błędy w tym procesie replikacji może spowodować wiele problemów w przedsiębiorstwie.

Stan replikacji AD pakietu rozwiązania regularnie monitoruje środowiska usługi Active Directory dla wszystkie błędy replikacji i raporty wyników na pulpicie nawigacyjnym usługi OMS.

## <a name="installing-and-configuring-the-solution"></a>Instalowanie i konfigurowanie rozwiązanie
Aby zainstalować i skonfigurować rozwiązanie, korzystając z następujących informacji.

- Czynniki musi być zainstalowany na kontrolerach domeny, które są członkami tej domeny ma zostać obliczone lub na serwerach członkowskich skonfigurowane wysyłanie AD replikacji danych do usługi OMS. Aby dowiedzieć się, jak nawiązać usługi OMS komputerach z systemem Windows, zobacz [Łączenie komputery do analizy dziennika](log-analytics-windows-agents.md). Jeśli kontrolera domeny jest już częścią istniejące środowisko System Center Operations Manager, którą chcesz nawiązać połączenie usługi OMS, zobacz [Łączenie programu Operations Manager do analizy dziennika](log-analytics-om-agents.md).
- Dodaj rozwiązanie Active Directory stan replikacji do obszaru roboczego usługi OMS przy użyciu procesu opisanego w [rozwiązań Dodawanie analizy dziennika z galerii rozwiązań](log-analytics-add-solutions.md).  Nie jest wymagana żadna konfiguracja dalsze.


## <a name="ad-replication-status-data-collection-details"></a>Szczegóły zbioru danych stan replikacji AD

W poniższej tabeli przedstawiono metody zbioru danych i inne szczegóły dotyczące sposobu dane są zbierane dla stan replikacji AD.

| platformy | Bezpośrednie agenta | SCOM agent | Azure miejsca do magazynowania | SCOM wymagane? | Dane agenta SCOM wysyłane za pośrednictwem grupy zarządzania | częstotliwość pobierania |
|---|---|---|---|---|---|---|
|Systemu Windows|![Tak](./media/log-analytics-ad-replication-status/oms-bullet-green.png)|![Tak](./media/log-analytics-ad-replication-status/oms-bullet-green.png)|![Brak](./media/log-analytics-ad-replication-status/oms-bullet-red.png)|![Brak](./media/log-analytics-ad-replication-status/oms-bullet-red.png)|![Tak](./media/log-analytics-ad-replication-status/oms-bullet-green.png)| co 5 dni|


## <a name="optionally-enable-a-non-domain-controller-to-send-ad-data-to-oms"></a>Opcjonalnie można włączyć kontrolera domeny nie wysyłać AD dane do usługi OMS
Jeśli nie chcesz, aby połączyć dowolny kontrolerach domeny bezpośrednio do usługi OMS, zbierać dane dla pakietu rozwiązania stan replikacji AD i dane są wysyłane za pomocą dowolnego połączenia usługi OMS komputera w domenie.

### <a name="to-enable-a-non-domain-controller-to-send-ad-data-to-oms"></a>Aby włączyć kontrolera domeny nie wysyłać AD dane do usługi OMS
1.  Upewnij się, że komputer jest członkiem domeny, którą chcesz monitorować za pomocą rozwiązanie AD stan replikacji.
2.  [Łączenie systemu Windows do usługi OMS](log-analytics-windows-agents.md) lub [Nawiązywanie połączenia przy użyciu usługi istniejące środowisko programu Operations Manager do usługi OMS](log-analytics-om-agents.md), jeśli nie jest już połączony.
3.  Na tym komputerze należy ustawić następujący klucz rejestru:
    - Klucz: **grupy HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\HealthService\Parameters\Management\<ManagementGroupName > \Solutions\ADReplication**
    - Wartość: **IsTarge**
    - Dane wartości: **PRAWDA**

    >[AZURE.NOTE]Te zmiany nie zostały wprowadzone aż do ponownego uruchomienia usługi agenta monitorowania firmy Microsoft (HealthService.exe).

## <a name="understanding-replication-errors"></a>Opis błędów replikacji
Po umieszczeniu danych stanu replikacji AD wysyłane do usługi OMS, zobaczysz kafelka podobny do następującego na pulpicie nawigacyjnym usługi OMS wskazująca liczbę błędy replikacji obecnie masz.  
![Stan replikacji AD kafelków](./media/log-analytics-ad-replication-status/oms-ad-replication-tile.png)

**Błędy krytyczne replikacji** są tymi, które są co najmniej 75% [istnienia schować](https://technet.microsoft.com/library/cc784932%28v=ws.10%29.aspx) las usługi Active Directory.

Po kliknięciu kafelka zobaczysz więcej informacji na temat błędów.
![Pulpit nawigacyjny stan replikacji AD](./media/log-analytics-ad-replication-status/oms-ad-replication-dash.png)


### <a name="destination-server-status-and-source-server-status"></a>Stan serwera docelowego i stan serwera źródła
Te karty Pokazywanie stanu docelowego i serwery źródła, których występują błędy replikacji. Liczba po nazwie kontrolera domeny wskazuje liczbę błędy replikacji na tym kontrolerze domeny.

Błędy zarówno serwery docelowego i serwery źródła są wyświetlane, ponieważ niektóre problemy są łatwiejsze rozwiązywać problemy z perspektywy serwera źródła i inni użytkownicy z perspektywy serwera docelowego.

W tym przykładzie widać, że wiele serwerów docelowego około mają taką samą liczbę błędów, ale istnieje jeden serwer źródła (ADDC35), który ma wiele błędów więcej niż wszystkie inne. Istnieje prawdopodobieństwo, że na ADDC35 powodującej go, aby się nie powieść, wysyłanie danych do partnerów replikacji jest problem. Rozwiązywanie problemów na ADDC35 prawdopodobnie rozwiąże wiele błędów, które są wyświetlane w karta serwera docelowego.

### <a name="replication-error-types"></a>Typy błędów replikacji
Ta karta zawiera informacje na temat typów błędów wykrytych w firmie. Każdy błąd ma unikatowy kod numeryczny i wiadomości, które mogą ułatwić podjęcie przyczyny błędu.

Pierścień u góry pozwala zorientować się pojawić się wykrywania błędów i rzadziej w środowisku.

To możesz wyświetlić po wielu kontrolerów domeny możliwości ten sam błąd replikacji. W tym przypadku można do znalezienia identyfikowanie rozwiązanie na jednym kontrolerze domeny, a następnie powtórz ją na innych kontrolerach domeny dotyczy ten sam błąd.

### <a name="tombstone-lifetime"></a>Czas użytkowania schować
Czas użytkowania schować Określa, jak długo usuniętego obiektu, określane jako schować, jest zachowywana w bazie danych usługi Active Directory. Gdy usuniętego obiektu przekazuje istnienia schować, proces zbierania śmieci automatycznie usuwa go z bazy danych usługi Active Directory.

Domyślny czas istnienia schować jest 180 dni do najnowszej wersji systemu Windows, ale został on 60 dni w starszych wersjach i mogą być zmieniane w jawnie przez administratora usługi Active Directory.

Należy wiedzieć, jeśli występują błędy replikacji, które zbliża się lub przekroczoną istnienia schować. Jeśli dwa kontrolery występuje błąd replikacji, która będzie nadal występował poza istnienia schować, replikacji zostanie wyłączona między te dwa kontrolery domeny, nawet jeśli źródłowych błąd replikacji został rozwiązany.

Karta istnienia schować ułatwia określenie miejsca, gdzie to jest zagrożenie dzieje. Każdy błąd w **ponad 100% TSL** kategorii reprezentuje partycją, która została wyłączona między swojego serwera źródłowej i docelowej dla co najmniej istnienia schować las.

W tej sytuacji po prostu naprawić błąd replikacji nie będą wystarczająco. Co najmniej musisz ręcznie badanie do identyfikowania i oczyszczania polegające obiektów przed można uruchomić ponownie replikacji. Może być konieczne nawet likwidacja kontrolera domeny.

Oprócz zidentyfikowania występują błędy replikacji, które trwają poza istnienia schować, warto również zwrócić uwagę na wszystkie błędy, należące do kategorii **TSL 50-75%** lub **TSL 75 100%** .

Są to błędy, które są wyraźnie polegające, nie przejściowych, więc potrzebują prawdopodobnie Twojego udziału, aby rozwiązać problem. Szczęście to, że ta osoba nie osiągnęły jeszcze istnienia schować. Jeśli niezwłocznie rozwiązywanie tych problemów i *przed* ich osiągnięcie istnienia schować, replikacji można uruchomić ponownie z minimalnymi ręczne.

Jak wspomniano wcześniej, kafelku pulpitu nawigacyjnego rozwiązanie stan replikacji AD zawiera numer *krytyczne* błędy replikacji w środowisku jest definiowana jako błędy, które są ponad 75% ważności schować (w tym błędy, które są ponad 100% TSL). Dążyć do zachowania ta liczba 0.

>[AZURE.NOTE] Wszystkie schować istnienia procent obliczenia są oparte na istnienia rzeczywisty schować las usługi Active Directory, więc można ufać, że te wartości procentowe są odpowiednie, nawet jeśli użytkownik ma ustawioną wartość istnienia schować niestandardowe.

### <a name="ad-replication-status-details"></a>Szczegóły stanu replikacji AD
Po kliknięciu dowolnego elementu w jednym z listy pojawi się dodatkowe informacje dotyczące go za pomocą wyszukiwania dziennika. Wyniki są przefiltrowane w celu wyświetlenia tylko błędy związane z tego elementu. Na przykład po kliknięciu na pierwszym kontrolerze domeny w obszarze **Stan serwera docelowego (ADDC02)**na liście pojawi się wyniki wyszukiwania filtrowane, aby błędy pokaz z tego kontrolera domeny oznaczone jako docelowym serwerze:

![Błędy stan replikacji AD w wynikach wyszukiwania](./media/log-analytics-ad-replication-status/oms-ad-replication-search-details.png)

W tym miejscu można filtrować dodatkowo, modyfikowanie kwerendy wyszukiwania i tak dalej. Aby uzyskać więcej informacji o korzystaniu z funkcji wyszukiwania dziennika zobacz [Wyszukiwanie dziennika](log-analytics-log-searches.md).

Pole **HelpLink** zawiera adres URL strony TechNet z dodatkowymi informacjami na temat tego błędu. Możesz skopiować i wkleić to łącze w oknie przeglądarki, aby wyświetlić informacje dotyczące rozwiązywania problemów i naprawianie błędów.

Możesz również kliknąć pozycję **Eksportuj** do Eksportuj wyniki do programu Excel. Umożliwia wizualizowanie danych o błędach replikacji w żaden sposób, którą chcesz.

![wyeksportowane AD replikacji stanu błędów w programie Excel](./media/log-analytics-ad-replication-status/oms-ad-replication-export.png)

## <a name="ad-replication-status-faq"></a>Stan replikacji AD — często zadawane pytania
**P: jak często jest aktualizacja AD replikacji stan danych?**
O: informacje są aktualizowane co 5 dni.

**P: czy istnieje możliwość skonfigurowania częstotliwość aktualizowania danych?**
O: nie w tej chwili.

**P: czy potrzebuję dodać wszystkie moich kontrolerów domeny do mojego obszaru roboczego usługi OMS, aby zobaczyć stan replikacji?**
O: nie można dodać tylko jeden kontroler domeny. Jeśli masz wiele kontrolerów domeny w obszarze roboczym usługi OMS dane ze wszystkich z nich są wysyłane do usługi OMS.

**P: nie chcę dodać dowolnego kontrolera domeny do mojego obszaru roboczego usługi OMS. Można nadal korzystać z rozwiązanie stan replikacji AD?**
O: tak. Można ustawić wartości klucza rejestru, aby włączyć tę opcję. Zobacz, [Aby włączyć kontrolera domeny nie wysyłanie AD danych do usługi OMS](#to-enable-a-non-domain-controller-to-send-ad-data-to-oms).

**P: co to jest nazwa procesu, który ma zbierania danych?**
O: AdvisorAssessment.exe

**P: jak długo trwa zbierania danych?**
O: czas zbioru danych zależy od rozmiaru środowiska usługi Active Directory, ale zwykle trwa mniej niż 15 minut.

**P: jaki typ danych są zbierane?**
O: replikacji informacje są zbierane przez LDAP.

**P: czy istnieje możliwość skonfigurowania, gdy dane są zbierane?**
O: nie w tej chwili.

**P: jakie uprawnienia należy zbieranie danych?**
O: Normalny uprawnienia do usługi Active Directory są zwykle wystarczające.

## <a name="troubleshoot-data-collection-problems"></a>Rozwiązywanie problemów zbioru danych
Aby można było zbierać dane, stan replikacji AD pakietu rozwiązania wymaga co najmniej jednego kontrolera domeny do przyłączania się do usługi OMS obszaru roboczego. Do momentu to zrobisz, pojawi się komunikat z informacją, że **dane są nadal zbierane**.

Jeśli potrzebujesz pomocy łączenie jednego z kontrolerów domeny, możesz wyświetlić dokumentacji na [komputerach Łączenie systemu Windows do analizy dziennika](log-analytics-windows-agents.md). Możesz też kontrolera domeny jest już połączony z istniejące środowisko System Center Operations Manager, możesz wyświetlać dokumentacji na [Łączenie System Center Operations Manager do analizy dziennika](log-analytics-om-agents.md).

Jeśli nie chcesz, aby połączyć dowolny kontrolerach domeny bezpośrednio do usługi OMS lub SCOM, zobacz [Aby włączyć kontrolera domeny nie wysyłanie AD danych do usługi OMS](#to-enable-a-non-domain-controller-to-send-ad-data-to-oms).


## <a name="next-steps"></a>Następne kroki

- [Dziennik wyszukiwania dziennika analizy](log-analytics-log-searches.md) służy do wyświetlania szczegółowych danych stanu replikacja usługi Active Directory.
