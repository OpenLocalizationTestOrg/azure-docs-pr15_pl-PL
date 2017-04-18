<properties
    pageTitle="Grupy komputerów w dzienniku analizy logowania wyszukiwania | Microsoft Azure"
    description="Grupy komputerów w dzienniku analizy umożliwiają zakres dziennika wyszukiwań do określonego zestawu komputerów.  Ten artykuł zawiera opis różnych metod, które umożliwiają tworzenie grup komputerów i sposobach ich używania w wynikach wyszukiwania dziennika."
    services="log-analytics"
    documentationCenter=""
    authors="bwren"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="bwren"/>

# <a name="computer-groups-in-log-analytics-log-searches"></a>Grupy komputerów w dzienniku analizy logowania wyszukiwania
Grupy komputerów w dzienniku analizy umożliwiają zakres [logowania wyszukiwań](log-analytics-log-searches.md) do określonego zestawu komputerów.  Każda grupa jest wpisywany z komputerami za pomocą kwerendy zdefiniowanego lub importując grupy z różnych źródeł.  W przypadku grupy znajduje się w wynikach wyszukiwania dziennika, wyniki są ograniczone do rekordów, które są zgodne z komputerów w grupie.

## <a name="creating-a-computer-group"></a>Tworzenie grupy komputera
Możesz utworzyć grupę komputerów do analizy dziennika przy użyciu jednej z metod w poniższej tabeli.  W poniższych sekcjach znajdują się szczegółowe informacje o każdej z tych metod. 

| Metoda | Opis |
|:---|:---|
| Przeszukiwanie dziennika       | Tworzenie wyszukiwania dziennika, która zwraca listę komputerów i Zapisz wyniki jako grupę komputerów. |
| Przeszukiwanie dziennika interfejsu API   | Za pomocą interfejsu API wyszukiwania dziennika programowo utworzyć grupę komputerów, na podstawie wyników wyszukiwania dziennika. |
| Usługi Active Directory | Automatyczne skanowanie członkostwa w grupach komputerów agenta, które są członkami domeny usługi Active Directory i utworzyć grupę w dzienniku analizy dla każdej grupy zabezpieczeń.
| WSUS              | Automatycznie szuka kierowanie grupy serwerów WSUS lub Twoi klienci i tworzenie grupy w dzienniku analizy dla każdej z nich. |


### <a name="log-search"></a>Przeszukiwanie dziennika

Grupy komputerów utworzone na podstawie wyszukiwania dziennika będzie zawierać wszystkie komputery zwróconych przez kwerendę wyszukiwania, zdefiniowana.  Ta kwerenda jest uruchamiany każdorazowo grupy komputerów jest używana, dzięki czemu będą odzwierciedlane zmiany po utworzeniu grupy.

Poniższa procedura umożliwia tworzenie grupy komputera z wyszukiwania dziennika.

1. [Tworzenie wyszukiwania dziennika](log-analytics-log-searches.md) , która zwraca listę komputerów.  Wyszukiwanie musi zwracać odrębnych zestaw komputerów przy użyciu coś takie jak **Odrębnych komputera** lub **count() miary przez komputer** w kwerendzie.  
2. Kliknij przycisk **Zapisz** w górnej części ekranu.
3. Wybierz pozycję **Tak,** aby **Zapisz to zapytanie jako grupę komputerów:**.
4. Wpisz **nazwę** i **kategorii** dla grupy.  Jeśli wyszukiwanie na podstawie tę samą nazwę i kategorii już istnieje, następnie możesz wyświetli monit o zastąpienie go.  Masz wiele wyszukiwania o tej samej nazwie w różnych kategoriach. 

Poniżej przedstawiono przykład wyszukiwania, które można zapisać jako grupę komputerów.

    Computer="Computer1" OR Computer="Computer2" | distinct Computer 
    Computer=*srv* | measure count() by Computer

### <a name="log-search-api"></a>Przeszukiwanie dziennika interfejsu API

Grupy komputerów utworzone za pomocą interfejsu API wyszukiwania dziennika różnią się od wyszukiwania utworzone za pomocą wyszukiwania dziennika.

Aby uzyskać szczegółowe informacje na temat tworzenia grupy komputera za pomocą interfejsu API wyszukiwania dziennika zobacz [grupy komputerów w dzienniku analizy logowania wyszukiwania interfejsu API usługi REST](log-analytics-log-search-api.md#computer-groups).

### <a name="active-directory"></a>Usługi Active Directory

Po skonfigurowaniu analizy dziennika do zaimportowania członkostwa w grupach usługi Active Directory, będzie ją analizowanie członkostwa w grupach dowolnych komputerów domeny połączone z agentem usługi OMS.  Grupa komputerów zostanie utworzona w analizy dziennika dla każdej grupy zabezpieczeń w usłudze Active Directory, a każdy komputer jest dodawany do grupy komputerów odpowiadający grup zabezpieczeń, do których należą.  Członkostwo są stale aktualizowane co 4 godziny.  

Konfigurowanie analizy dziennika do zaimportowania grup zabezpieczeń usługi Active Directory z menu **Grup komputerów** w dzienniku analizy **Ustawienia**.  Wybierz **automatyzacji** , a następnie **członkostwa w grupach usługi Active Directory importowanie z komputerów**.  Nie jest wymagana żadna konfiguracja dalsze.

![Grupy komputer z usługi Active Directory](media/log-analytics-computer-groups/configure-activedirectory.png)

Po zaimportowaniu grup menu zostanie wyświetlona liczba komputerów z członkostwa w grupach wykryte i liczby grup zaimportowane.  Możesz kliknąć jedną z tych łączy zwraca rekordy **ComputerGroup** z tych informacji.

### <a name="windows-server-update-service"></a>Usługa aktualizacji systemu Windows Server

Po skonfigurowaniu analizy dziennika do zaimportowania członkostwa w grupach WSUS go będzie analizowanie określania docelowych członkostwa w grupach komputerów z agentem usługi OMS.  Jeśli używasz klienta kierowanie dowolny komputer jest połączony z usługi OMS, który jest częścią dowolnej WSUS kierowanie grupy uzyskuje jego członkostwo w grupach zaimportowane do analizy dziennika. Jeśli korzystasz z po stronie serwera kierowanie usługi OMS agenta powinna zostać zainstalowana na serwerze WSUS w kolejności, aby uzyskać informacje o członkostwie grup do zaimportowania do usługi OMS.  Członkostwo są stale aktualizowane co 4 godziny. 

Konfigurowanie analizy dziennika do zaimportowania grup zabezpieczeń usługi Active Directory z menu **Grup komputerów** w dzienniku analizy **Ustawienia**.  Wybierz **Usługi Active Directory** , a następnie **członkostwa w grupach usługi Active Directory importowanie z komputerów**.  Nie jest wymagana żadna konfiguracja dalsze.

![Grupy komputer z usługi Active Directory](media/log-analytics-computer-groups/configure-wsus.png)

Po zaimportowaniu grup menu zostanie wyświetlona liczba komputerów z członkostwa w grupach wykryte i liczby grup zaimportowane.  Możesz kliknąć jedną z tych łączy zwraca rekordy **ComputerGroup** z tych informacji.

## <a name="managing-computer-groups"></a>Zarządzanie grupami komputerów

Możesz wyświetlić grupy komputerów, które zostały utworzone na podstawie wyszukiwania dziennika lub interfejsu API wyszukiwania dziennika z menu **Grup komputerów** w dzienniku analizy **Ustawienia**.  Kliknij przycisk **x** w kolumnie **Usuń** , aby usunąć grupę komputerów.  Kliknij ikonę **Wyświetlanie członków** grupy uruchomić wyszukiwanie dziennika grupy, która zwraca jej członków. 

![Grupy komputerów zapisane](media/log-analytics-computer-groups/configure-saved.png)

Aby zmodyfikować grupę, tworzenie nowej grupy o tej samej **kategorii** i **nazwy** Aby zastąpić oryginalne grupy.

## <a name="using-a-computer-group-in-a-log-search"></a>Korzystanie z grupy komputera w wynikach wyszukiwania dziennika
Zapoznaj się z grupy komputerów w wynikach wyszukiwania dziennika za pomocą następującej składni.  Określanie, czy **Kategoria** jest opcjonalne i tylko wymagane, jeśli masz komputer grupy o tej samej nazwie w różnych kategoriach. 

    $ComputerGroups[Category: Name]

Po uruchomieniu wyszukiwania najpierw rozpoznawania członków grupy komputera uwzględnione w wyszukiwaniu.  Jeśli grupa jest oparty na przeszukiwanie dziennika, że wyszukiwanie jest uruchamiany zwraca członków grupy przed wykonaniem wyszukiwania najwyższego poziomu dziennika.

Grupy komputerów są najczęściej używane z **klauzuli w polu Wyszukaj dziennika, tak jak w poniższym przykładzie** .

    Type=UpdateSummary Computer IN $ComputerGroups[My Computer Group]

## <a name="computer-group-records"></a>Komputer Grupowanie rekordów

Rekord jest tworzony w repozytorium usługi OMS dla każdego członkostwo w grupie komputerów, utworzone za pomocą usługi Active Directory lub WSUS.  Te rekordy mają typ **ComputerGroup** i mają właściwości w poniższej tabeli.  Rekordy nie są tworzone dla grup komputerów według wyszukiwania dziennika.

| Właściwość | Opis |
|:--|:--|
| Typ                | *ComputerGroup* |
| SourceSystem        | *SourceSystem*  |
| Komputer            | Nazwa komputera członka. |
| Grupa               | Nazwa grupy. |
| GroupFullName       | Pełna ścieżka do grupy, w tym źródłowa i nazwa źródła.
| GroupSource         | Źródło tej grupy zostało pobrane od. <br><br>Pozycji<br>WSUS<br>WSUSClientTargeting |
| GroupSourceName     | Nazwa grupy zebrane ze źródła.  Dla usługi Active Directory jest to nazwa domeny. |
| ManagementGroupName | Nazwa grupy zarządzania SCOM agentów.  W przypadku innych czynników jest AOI —\<identyfikator obszaru roboczego\> |
| TimeGenerated       | Data i godzina utworzenia lub aktualizacji grupy komputerów. |



## <a name="next-steps"></a>Następne kroki

- Informacje na temat [wyszukiwania dziennika](log-analytics-log-searches.md) do analizowania danych zebranych ze źródła danych i rozwiązań.  