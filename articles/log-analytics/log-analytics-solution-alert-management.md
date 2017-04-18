<properties
   pageTitle="Alert rozwiązanie do zarządzania w pakiecie operacje zarządzania usługi (OMS) | Microsoft Azure"
   description="Rozwiązanie zarządzania alertami w dzienniku analizy ułatwia analizowanie wszystkie alerty w środowisku.  Oprócz konsolidowania alerty generowane w ramach usługi OMS, jego importuje alerty z połączonych grup zarządzania System Center Operations Manager (SCOM) do analizy dziennika."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/06/2016"
   ms.author="bwren" />

# <a name="alert-management-solution-in-operations-management-suite-oms"></a>Alert rozwiązanie do zarządzania w pakiecie operacje zarządzania usługi (OMS)

![Ikona zarządzania alertu](media/log-analytics-solution-alert-management/icon.png) Rozwiązanie zarządzania alertami ułatwia analizowanie wszystkie alerty w środowisku.  Oprócz konsolidowania alerty generowane w ramach usługi OMS, jego importuje alerty z połączonych grup zarządzania System Center Operations Manager (SCOM) do analizy dziennika.  W środowiskach z wieloma zarządzania grupami rozwiązanie zarządzania alertami zapewni skonsolidowany widok alerty we wszystkich grupach zarządzania.

## <a name="prerequisites"></a>Wymagania wstępne

- Aby zaimportować alerty z SCOM, to rozwiązanie wymaga połączenia między obszaru roboczego usługi OMS i grupę zarządzania SCOM przy użyciu procesu opisanego w [Łączenie analizy dziennika programu Operations Manager](log-analytics-om-agents.md).  


## <a name="configuration"></a>Konfiguracja

Dodaj rozwiązanie zarządzania alertami do obszaru roboczego usługi OMS przy użyciu procesu opisanego w [Dodaj rozwiązań](log-analytics-add-solutions.md).  Nie jest wymagana żadna konfiguracja dalsze.

## <a name="management-packs"></a>Pakiety zarządzania

Grupy zarządzania SCOM jest podłączone do obszaru roboczego usługi OMS, następnie następujące pakiety zarządzania zostanie zainstalowany w SCOM podczas dodawania tego rozwiązania.  Istnieje konfiguracji lub konserwacji te pakiety narzędzi zarządzania wymagane.  

- Centrum systemu zarządzania alertami Advisor (Microsoft.IntelligencePacks.AlertManagement)

Aby uzyskać więcej informacji na sposób aktualizowania pakietów zarządzania rozwiązanie zobacz [Łączenie analizy dziennika programu Operations Manager](log-analytics-om-agents.md).

## <a name="data-collection"></a>Zbieranie danych

### <a name="agents"></a>Czynniki

W poniższej tabeli opisano połączonych źródeł, które są obsługiwane przy użyciu tego rozwiązania.

| Połączone źródła | Pomoc techniczna | Opis |
|:--|:--|:--|
| [Czynniki systemu Windows](log-analytics-windows-agents.md) | Brak | Bezpośrednie agentów systemu Windows nie generują alertów SCOM. |
| [Czynniki Linux](log-analytics-linux-agents.md) | Brak | Bezpośrednie agentów Linux nie generują alertów SCOM. |
| [Grupa Zarządzanie SCOM](log-analytics-om-agents.md) | Tak | Alertów generowanych agentów SCOM są dostarczane do grupy zarządzania, a następnie przesłane dalej do analizy dziennika.<br><br>Bezpośrednie połączenie z agentem SCOM analizy dziennika nie jest wymagane. Alert dane są przesyłane w grupie Zarządzanie do repozytorium usługi OMS. |
| [Konto Azure miejsca do magazynowania](log-analytics-azure-storage.md) | Brak | Alerty SCOM nie są przechowywane w konta Azure miejsca do magazynowania. |

### <a name="collection-frequency"></a>Częstotliwość pobierania

Alerty generowane w ramach usługi OMS są dostępne w rozwiązaniu natychmiast.  Alert są wysyłane dane w grupie Zarządzanie SCOM do analizy dziennika co 3 minuty.  

## <a name="using-the-solution"></a>Za pomocą rozwiązanie

Po dodaniu rozwiązanie zarządzania alertami do obszaru roboczego usługi OMS kafelków **Zarządzania alertami** zostanie dodana do pulpitu nawigacyjnego usługi OMS.  Ten fragment Wyświetla ile.liczb i graficzna reprezentacja liczbę aktywnych alertów, które zostały wygenerowane w ciągu ostatnich 24 godzin.  Nie można zmienić tego zakresu czasu.

![Alert kafelków zarządzania](media/log-analytics-solution-alert-management/tile.png)

Kliknij Kafelek **Zarządzania alertami** , aby otworzyć pulpit nawigacyjny **Zarządzania alertami** .  Pulpit nawigacyjny zawiera kolumny w tabeli poniżej.  Każda kolumna zawiera listę górny alerty dziesięć według liczby spełniające kryteria tej kolumny dla określonego zakresu i przedział czasu.  Można przeprowadzić wyszukiwanie dziennika zawierającego całą listę, klikając u dołu kolumny **wyświetlić wszystkie** lub klikając jej nagłówek.

| Kolumny| Opis |
|:--|:--|
| Krytyczne ostrzeżenia | Wszystkie alerty z ważności krytyczne pogrupowane według nazwy alertu.  Kliknij nazwę alertu, aby uruchomić wyszukiwanie dziennika zwraca wszystkie rekordy dla tego alertu. |
| Ostrzeżenie dotyczące alertów | Wszystkie alerty z ważności ostrzeżenie pogrupowane według nazwy alertu.  Kliknij nazwę alertu, aby uruchomić wyszukiwanie dziennika zwraca wszystkie rekordy dla tego alertu. |
| Aktywne SCOM alertów | Wszystkie alerty SCOM z dowolnym Państwie innym niż *zamknięte* pogrupowane według źródła, który wygenerował alert. |
| Wszystkie aktywne alerty | Wszystkie alerty z dowolnej ważności pogrupowane według nazwy alertu. Obejmuje tylko alerty SCOM z dowolnego stanu inną niż *zamknięte*.|

Przewiń listę w prawo spowoduje wyświetlenie listy kilka typowych kwerend, które można kliknąć w celu wykonywania [wyszukiwania dziennika](log-analytics-log-searches.md) dla dane alertów przez pulpitu nawigacyjnego.

![Pulpit nawigacyjny zarządzania alertów](media/log-analytics-solution-alert-management/dashboard.png)

## <a name="scope-and-time-range"></a>Zakres zakres i godzin

Domyślnie zakres alerty analizowane w rozwiązanie zarządzania alertami jest ze wszystkich grup połączonego zarządzania uzyskiwanych w ciągu ostatnich 7 dni.  

![Alert zakresu zarządzania](media/log-analytics-solution-alert-management/scope.png)

- Aby zmienić grupy zarządzania włączone do analizy, kliknij przycisk **zakres** u góry pulpitu nawigacyjnego.  Możesz wybrać albo **globalne** dla wszystkich grup połączonego zarządzania lub **Management Group** , aby zaznaczyć grupę zarządzania pojedynczy.

- Aby zmienić zakres czasu alertów, zaznacz **dane na podstawie** u góry pulpitu nawigacyjnego.  Możesz wybrać alerty generowane w ciągu ostatnich 7 dni, 1 dzień lub 6 godzin.  Lub można wybrać **Niestandardowy** i określ niestandardowy zakres dat.

## <a name="log-analytics-records"></a>Rekordy analizy dziennika

Rozwiązanie zarządzania alertami analizuje żadnych rekordów z typu **alertu**.  Zostaną również importowanie alerty z SCOM i utwórz odpowiedni rekord dla każdego typu **alertów** i SourceSystem **OpsManager**.  Te rekordy mają właściwości w poniższej tabeli.  

| Właściwość | Opis |
|:--|:--|
| Typ | *Alert* |
| SourceSystem | *OpsManager* |
| AlertContext | Szczegóły elementu danych, które spowodowały alert do wygenerowania w formacie XML. |
| AlertDescription | Szczegółowy opis alertu. |
| AlertId | Identyfikator GUID alertu. |
| AlertName | Nazwa alertu. |
| AlertPriority | Poziom priorytetu alertu. |
| AlertSeverity | Poziom ważności alertu. |
| AlertState | Najnowsze stan rozdzielczość alertu. |
| Ostatnio zmodyfikowane przez | Nazwa użytkownika, która jako ostatnia zmodyfikowała alert. |
| ManagementGroupName | Nazwa grupy zarządzania, gdzie został wygenerowany alert. |
| RepeatCount | Liczbę czasu wygenerowania tego samego ostrzeżenia dla tego samego monitorowane obiektu od spowodowanych. |
| ResolvedBy | Nazwa użytkownika, który alert. Pusty, jeśli alert nie został jeszcze rozwiązany. |
| SourceDisplayName | Nazwa wyświetlana monitorowania obiektu, który wygenerował alert. |
| SourceFullName | Imię i nazwisko monitorowania obiektu, który wygenerował alert. |
| TicketId | Identyfikator biletów alertu, jeśli środowiska SCOM jest zintegrowany z procesem służące do przypisywania bilety alertów.  Pusty bilet nie zostanie przypisany identyfikator. |
| TimeGenerated | Data i godzina utworzenia alertu. |
| TimeLastModified | Data i godzina ostatniej modyfikacji alert. |
| TimeRaised | Data i godzina wygenerowania alert. |
| TimeResolved | Data i godzina, który został usunięty alert. Pusty, jeśli alert nie został jeszcze rozwiązany. |

## <a name="sample-log-searches"></a>Przykładowy dziennik wyszukiwania

Poniższa tabela zawiera przykładowe dziennika wyszukiwanie rekordów alertów, zebrane przy użyciu tego rozwiązania.  

| Kwerendy | Opis |
|:--|:--|
| Typ = Alert SourceSystem = OpsManager AlertSeverity = błąd TimeRaised > teraz - 24-GODZINNEGO | Krytyczne ostrzeżenia podniesione w ciągu ostatnich 24 godzin |
| Typ = Alert AlertSeverity = ostrzeżenie TimeRaised > teraz - 24-GODZINNEGO | Ostrzeżenie dotyczące alertów podniesioną w ciągu ostatnich 24 godzin  |
| Typ = Alert SourceSystem = OpsManager AlertState! = zamknięty TimeRaised > teraz - 24-GODZINNEGO & #124; Zmierz count() jako liczba według SourceDisplayName | Źródłami active alertami podniesione w ciągu ostatnich 24 godzin |
| Typ = Alert SourceSystem = OpsManager AlertSeverity = błąd TimeRaised > AlertState teraz - 24-GODZINNEGO! = zamknięty | Krytyczne ostrzeżenia podniesione w ciągu ostatnich 24 godzin, które są nadal aktywne |
| Typ = Alert SourceSystem = OpsManager TimeRaised > AlertState teraz - 24-GODZINNEGO = zamknięty | Alerty podniesioną w ciągu ostatnich 24 godzin, które są zamknięty |
| Typ = Alert SourceSystem = OpsManager TimeRaised > teraz - 1 dzień & #124; Zmierz count() jako liczba według AlertSeverity | Alerty podczas ostatnich 1 dzień pogrupowane według ich ważności |
| Typ = Alert SourceSystem = OpsManager TimeRaised > teraz - 1 dzień & #124; Sortowanie RepeatCount desc | Alerty podczas ostatnich 1 dzień, sortowane według ich wartości liczbę powtórzeń |

## <a name="next-steps"></a>Następne kroki

- Dowiedz się więcej o [alerty w dzienniku analizy](log-analytics-alerts.md) szczegółowe informacje na temat generowania alertów z analizy dziennika.
