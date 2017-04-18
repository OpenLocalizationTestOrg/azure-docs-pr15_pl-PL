<properties
    pageTitle="Rozwiązanie ocena aktualizacji systemu do analizy dziennika | Microsoft Azure"
    description="Za pomocą rozwiązanie aktualizacje systemu analizy dziennika ułatwiające stosowania brakujących aktualizacji do serwerów w infrastrukturze."
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
    ms.date="08/11/2016"
    ms.author="banders"/>

# <a name="system-update-assessment-solution-in-log-analytics"></a>Rozwiązanie ocena aktualizacji systemu do analizy dziennika

Za pomocą rozwiązanie aktualizacje systemu analizy dziennika ułatwiające stosowania brakujących aktualizacji do serwerów w infrastrukturze. Po zainstalowaniu rozwiązanie, możesz wyświetlić aktualizacje, które nie są widoczne na serwerach monitorowane przy użyciu kafelka **Ocenę aktualizacji systemu** na stronie **Omówienie** w usługi OMS.

Jeśli nie zostaną znalezione brakujące aktualizacje, szczegóły są wyświetlane na pulpicie nawigacyjnym **aktualizacji** . Pulpit nawigacyjny **aktualizacje** służy do pracy z brakujących aktualizacji i opracowanie planu dotyczą serwerów, które są potrzebne.

## <a name="installing-and-configuring-the-solution"></a>Instalowanie i konfigurowanie rozwiązanie
Aby zainstalować i skonfigurować rozwiązanie, korzystając z następujących informacji.

- Dodaj rozwiązanie ocenę aktualizacji systemu do usługi OMS obszaru roboczego przy użyciu procesu opisanego w [rozwiązań Dodawanie analizy dziennika z galerii rozwiązań](log-analytics-add-solutions.md).  Nie jest wymagana żadna konfiguracja dalsze.

## <a name="system-update-data-collection-details"></a>Szczegóły zbioru danych aktualizacji systemu

Ocena aktualizacji systemu zbiera metadanych i stan danych przy użyciu czynników, które są włączone.

W poniższej tabeli przedstawiono metody zbioru danych i inne szczegóły dotyczące sposobu dane są zbierane oceny aktualizacji systemu.

| platformy | Bezpośrednie agenta | SCOM agent | Azure miejsca do magazynowania | SCOM wymagane? | Dane agenta SCOM wysyłane za pośrednictwem grupy zarządzania | częstotliwość pobierania |
|---|---|---|---|---|---|---|
|Systemu Windows|![Tak](./media/log-analytics-system-update/oms-bullet-green.png)|![Tak](./media/log-analytics-system-update/oms-bullet-green.png)|![Brak](./media/log-analytics-system-update/oms-bullet-red.png)|            ![Brak](./media/log-analytics-system-update/oms-bullet-red.png)|![Tak](./media/log-analytics-system-update/oms-bullet-green.png)| Co najmniej 2 razy na dzień i 15 minut po zainstalowaniu aktualizacji|

W poniższej tabeli pokazano przykłady typów danych zebranych przez ocenę aktualizacji systemu:

|**Typ danych**|**Pola**|
|---|---|
|Metadane|BaseManagedEntityId, ObjectStatus, jednostka organizacyjna, ActiveDirectoryObjectSid, PhysicalProcessors, NazwaSieci, adres IP, ForestDNSName, NetbiosComputerName, VirtualMachineName, LastInventoryDate, HostServerNameIsVirtualMachine, adres IP, NetbiosDomainName, LogicalProcessors, Nazwa_dns, DisplayName, DomainDnsName, ActiveDirectorySite, PrincipalName, OffsetInMinuteFromGreenwichTime|
|Województwo|StateChangeEventId, StateId, NewHealthState, OldHealthState, kontekst, TimeGenerated, TimeAdded, StateId2, BaseManagedEntityId, MonitorId, HealthState, Ostatnia modyfikacja, LastGreenAlertGenerated, DatabaseTimeModified|


### <a name="to-work-with-updates"></a>Aby pracować z aktualizacji

1. Na stronie **Przegląd** kliknij **Ocenę aktualizacji systemu** .  
    ![System oceny aktualizacji kafelków](./media/log-analytics-system-update/sys-update-tile.png)
2. Na pulpicie nawigacyjnym **aktualizacje** umożliwia wyświetlanie kategorii aktualizacji.  
    ![Aktualizacje z pulpitu nawigacyjnego](./media/log-analytics-system-update/sys-updates02.png)
3. Przewiń do prawej strony, aby wyświetlić karta **Aktualizacje krytyczne i zabezpieczeń systemu Windows** , a następnie w obszarze **klasyfikacji**, kliknij **Aktualizacji**.  
    ![Aktualizacje zabezpieczeń](./media/log-analytics-system-update/sys-updates03.png)
4. Na stronie wyszukiwania dziennika przedstawiono różne informacje o aktualizacjach zabezpieczeń, których brakuje serwery odnalezione w infrastrukturze. Kliknij pozycję **listy** , aby wyświetlić szczegółowe informacje o aktualizacjach.  
    ![Wyniki wyszukiwania — listy](./media/log-analytics-system-update/sys-updates04.png)
5. Na stronie wyszukiwania dziennika przedstawiono szczegółowe informacje o każdej aktualizacji. Obok pozycji liczba KBID kliknij przycisk **Widok** do wyświetlania odpowiedniego artykułu w witrynie pomocy technicznej firmy Microsoft w sieci Web.  
    ![Zaloguj się wyników wyszukiwania — widok KB](./media/log-analytics-system-update/sys-updates05.png)
6. Przeglądarki sieci web otwiera stronę pomocy technicznej firmy Microsoft w sieci Web aktualizacji w nowej karcie. Wyświetlanie informacji o aktualizacji, który nie jest widoczny.  
    ![Strona sieci web Microsoft Support](./media/log-analytics-system-update/sys-updates06.png)
7. Przy użyciu informacji znalezieniu, można utworzyć plan ręcznie zastosować aktualizację brakujące lub możesz nadal po pozostałe kroki, aby automatycznie zastosować aktualizację.
8. Jeśli chcesz automatycznie zastosować aktualizację Brak, wróć do pulpitu nawigacyjnego **aktualizacje** , a następnie w obszarze **Aktualizowanie zostanie uruchomiona**, kliknij, **kliknij, aby zaplanować aktualizacji, uruchom**.  
    ![Zostanie uruchomiona aktualizacja — karta według harmonogramu](./media/log-analytics-system-update/sys-updates07.png)
9. Na stronie **Zostanie uruchomiona zaktualizować** na karcie **Harmonogram** kliknij przycisk **Dodaj** , aby utworzyć nowy Uruchom aktualizacji.  
    ![Zaplanowane karty — Dodawanie](./media/log-analytics-system-update/sys-updates08.png)
10. Na stronie **Nowych aktualizacji uruchom** , wpisz tekst Uruchom nazwę aktualizacji, Dodawanie poszczególnych komputerów lub grup komputerów, Definiuj harmonogram i kliknij przycisk **Zapisz**.  
    ![Uruchom nowych aktualizacji](./media/log-analytics-system-update/sys-updates09.png)
11. Karta **Harmonogram** w pokazuje strony **Aktualizowanie zostanie uruchomiona** Uruchamianie nowej aktualizacji, które zostały zaplanowane.  
    ![Karta według harmonogramu](./media/log-analytics-system-update/sys-updates10.png)
12. Po uruchomieniu aktualizacji, uruchom pojawi się informacje dotyczące go na karcie **uruchomiony** .  
    ![Karta uruchomionego](./media/log-analytics-system-update/sys-updates11.png)
13. Po zakończeniu aktualizacji uruchomić, na karcie **wykonane** jest wyświetlany stan.
14. Aktualizacje zostały zastosowane z aktualizacji, uruchom w karta **Aktualizacje krytyczne i zabezpieczeń systemu Windows** , zobaczysz zmniejsza się liczba aktualizacji.  
    ![Karta aktualizacje krytyczne i zabezpieczeń systemu Windows — zmniejszenia licznika aktualizacji](./media/log-analytics-system-update/sys-updates12.png)



## <a name="next-steps"></a>Następne kroki

- [Dzienniki wyszukiwania](log-analytics-log-searches.md) , aby wyświetlić szczegółowy system aktualizowania danych.
